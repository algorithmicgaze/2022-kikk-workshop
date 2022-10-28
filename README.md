# KIKK 2022 Creative AI Workshop

![Banner](.github/kikk-creative-ai-banner.png)

# Agenda

```
10:00 Introductions
10:10 Introduction to Creative AI
11:10 Break
11:30 Introduction to pix2pix
12:30-13:30 Lunch Break
13:30 Figment workshop
14:00 Training basic model via Google Colab
15:00 Collecting data: video, Pinterest
16:00 Segmentation exercise
17:00 Training custom model
16:30 Scaling up: pix2pixHD
16:45 Conclusion & Reflection
```

# Figment

Download the latest version of Figment.

Build a simple hand/face segmentation pipeline using the webcam.

![Example output](.github/figment-segmentation-result.jpg)
[Example Figment File](figment_segmentation/face_segmentation_webcam.fgmt)

Save all output files as `.jpg` files in a directory; then create a ZIP file of the directory (On Mac: Right-Click > Compress)

# pix2pix

The `pix2pix_training/pix2pix_train_colab.ipynb` script is set up to run in [Google Colab](https://colab.research.google.com/):

1. Go to [Google Colab](https://colab.research.google.com/)
2. Choose the GitHub tab and type `https://github.com/algorithmicgaze/2022-kikk-workshop`
3. Choose the pix2pix_train_colab.ipynb script.
4. Run the first cell. You'll get a warning that the notebook was not authored by Google. Click "Run Anyway".
5. Open the "Files" sidebar (click the icon on the left) and drag your ZIP file in the panel.
6. Run all other cells, except for the cell marked as optional (the one that says "curl").
7. After the training has completed, you will see a tfjs.zip file in the `output` folder. Click the three dots to download that file.

If you have [Colab Pro](https://colab.research.google.com/signup/pricing), the script will execute substantially faster.

# Inference

Once the model has trained, you should be able to convert it to TensorFlow.js by running the last cells of the script.

If you don't have a model yet, you can download a complete prepared model from this link:

[unsplash_woman_tfjs.zip](https://enigmeta.s3.amazonaws.com/2022-kikk-workshop/unsplash_woman_tfjs.zip)

Unzip the file in the `figment/assets` folder. So there should be a folder `figment/assets/unsplash_woman_tfjs`.

Open the `face_inference.fgmt` script in the `figment` folder with Figment. You should be able to control the face with your webcam!

![Figment Inference Example](.github/figment-inference.jpeg)

# Next Steps

Once the model is trained, try feeding in different things than a face. What happens if the lines are hands? Or shapes? Can you scan a drawing with your webcam and control the face that way?

There are some quality improvements you can do too:

- Training for longer can help a lot
- Curating the dataset better might produce better results
- For the next step in quality, try pix2pixHD.

# pix2pixHD

[pix2pixHD](https://github.com/NVIDIA/pix2pixHD) is a version of pix2pix with substantial improvements in quality.

It's possible to run this in Google Colab. Look at the [dougrosman pix2pixHD](https://colab.research.google.com/github/dougrosman/pix2pixHD/blob/master/pix2pixHD.ipynb) implementation.

# pix2pixHD on a local machine

If you have a fast machine (NVIDIA RTX 2080 or better) you can run it with Docker as well:

```
docker run -it --rm -v ${PWD}:/work -w /work --gpus all --network=host pytorch/pytorch
```

## Install requirements (Lambda Stack)

```
pip install dominate scipy
```

## Training

```
python pix2pix_hd_train.py --name ai_laugh --resize_or_crop crop --fineSize 512  --dataroot assets/ai_laugh_only_teeth/ --label_nc 0 --no_instance --no_flip
```

## Testing

```
python test.py --name afritzuid --resize_or_crop crop --fineSize 1024 --how_many 326  --dataroot datasets/afritzuid_control_1 --label_nc 0 --no_instance
```

Results will be in `results/PROJECT/test_latest/images`. Convert to video:

```
cd results/PROJECT/test_latest/
ffmpeg -i images/%06d_synthesized_image.jpg -pix_fmt yuv420p -r 30 2022-09-11-faces-control-fdb-1.mp4
```
