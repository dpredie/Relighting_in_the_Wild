# Relighting Humans in the Wild: Monocular Full-Body Human Relighting with Domain Adaptation

![teaser](https://github.com/majita06/Relighting_in_the_Wild/blob/main/docs/teaser_pg.jpg)

This code is an implementation of the following paper:

Daichi Tajima, Yoshihiro Kanamori, Yuki Endo: "Relighting Humans in the Wild: Monocular Full-Body Human Relighting with Domain Adaptation," Computer Graphics Forum (Proc. of Pacific Graphics 2021), 2021. [[Project]](http://cgg.cs.tsukuba.ac.jp/~tajima/pub/relighting_in_the_wild/)[[PDF]](http://cgg.cs.tsukuba.ac.jp/~tajima/pub/relighting_in_the_wild/pdf/tajima_PG21.pdf)

## Prerequisites
1. Python3
2. PyTorch(>=1.5)

## Demo
1. Download our [two pre-trained models](https://drive.google.com/drive/folders/1q4dxQxM4hZ19Eo2e4YF-F197mjScfeT8?usp=sharing).
2. Make a "trained_models" directory in the parent directory and put "model_1st.pth" and "model_2nd.pth".

### Applying to images
To relight images under `./data/test_images`, run the following code:
```
sh ./scripts/demo_image.sh ./data/sample_images
```
The relighting results will be saved in `./demo/relighting_image/2nd`.
NOTE: If you want to change the light for relighting, please edit the script directly.

### Applying to videos
To relight video frames under `./data/test_video/sample_frames`, run the following code:
```
sh ./scripts/demo_video.sh ./data/test_video/sample_frames
```
The flicker-tolerant relighting results will be saved in `./demo/relighting_video/flicker_reduction`.
Please terminate the training manually before noise appears in the result.
NOTE: If you want to change the light for relighting, please edit the script directly.

### Training
#### 1st stage network
1. 
  - 3Dモデルから取得したbinary masks("XXX_mask.png"), albedo maps("XXX_tex.png"), transport maps("XXX_transport.npz"), skin masks("XXX_parsing.png")を`./data/train_human_1st`に置きます．
  - 環境マップから取得したSH光源("YYY.npy")を`./data/train_light_1st`と`./data/test_light_1st`に置きます．
2. Run train_1st.py
```
python3 train_1st.py --train_dir ./data/train_human_1st --test_dir ./data/test_human_1st ./data/train_light --train_light_dir --test_light_dir ./data/test_light --out_dir ./result/output_1st
```
#### 2nd stage network
1. Reconstruct the real photo dataset by a trained 1st stage model.
```
python3 make_dataset_2nd.py --in_dir ./data/real_photo_dataset --out_dir_train ./data/train_human_2nd --out_dir_test ./data/test_human_2nd --model_path ./trained_models/model_1st.pth
```
2. Run train_2nd.py.
```
python3 train_2nd.py --train_dir ./data/train_human_2nd --test_dir ./data/test_human_2nd --out_dir ./result/output_2nd
```

## Citation
Please cite our paper if you find the code useful:
```
@article{tajimaPG21,
  author    = {Daichi Tajima,
               Yoshihiro Kanamori,
               Yuki Endo},
  title     = {Relighting Humans in the Wild: Monocular Full-Body Human Relighting with Domain Adaptation},
  journal   = {Comput. Graph. Forum},
  volume    = {40},
  number    = {7},
  pages     = {--},
  year      = {2021},
}
```