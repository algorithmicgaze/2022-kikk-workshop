# KIKK Training Scripts

# pix2pix

The `pix2pix_train_colab.ipynb` script is set up to run in Google Colab. If you have Colab Pro, the script will execute substantially faster.

# pix2pixHD

Trying to run the default image on Windows using Docker:

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
