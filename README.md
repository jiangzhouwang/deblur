# Dynamic Scene Deblurring with Parameter Selective Sharing and Nested Skip Connections
by Hongyun Gao, Xin Tao, Xiaoyong Shen, Jiaya Jia. Please refer to the [paper](http://jiaya.me/papers/deblur_cvpr19.pdf) for the details.


## Prerequisites
- Python2.7 or Python3.6
- Opencv3.4
- Numpy
- Tensorflow 1.7 with NVIDIA GPU or CPU (cpu testing is very slow)

## Installation
Clone this repository to your PC. 

```bash
git clone https://github.com/firenxygao/deblur.git
cd deblur
```

## Testing

If GPU is available, you can use `--gpu` argument and add the gpu id to enable GPU computation. Otherwise, the code will use CPU for computation. 

```bash
python run_model.py --gpu=0
```

We provided 2 models for testing. The first model is trained on default data released by the paper \`\`Deep Multi-scale Convolutional Neural Network for Dynamic Scene Deblurring''. The second model is trained by mixing default data with our own generated data. You can use `--model` argument to choose between `default` or `alldata`.
Our 2 models and data can be downloaded by the links. [Models](https://www.dropbox.com/s/rcdz2fsob3jkhdq/checkpoints.zip?dl=0). [Dataset](https://www.dropbox.com/s/0jq86y7fa01awgl/data.tar?dl=0).

```bash
python run_model.py --model=default
```

You can test one single image or a folder of images by using `--input_path` argument. If you test images in `testing_imgs`, the output images will be saved into `testing_imgs_res`. If you test one single image `testing_img.jpg`, the result will be named `testing_img_res.jpg`.

```bash
python run_model.py --input_path=testing_imgs
```

To test the model, the height and width of the input tensor should be pre-defined as `--max_height` and `--max_width`. Our network requires the height and width to be multiples of `16` and the dimension should be assigned to the maximum size to accommodate all the images. 

In our implementation, we first check the image dimension. If the height is larger than the width, we transpose the image such that the width is larger than the height. Then we check whether image can be fit into the placeholder pre-defined by `max_height` and `max_width`. Otherwise, the images will be downsampled by the largest scale factor to 
be fed into the placeholder. And results will be upsampled to the original size.

According to our experience, a 720\*1280 image will take 9GB memory. Users can adjust `max_height` and `max_width` according to the memory conditions.

```bash
python run_model.py --max_height=720 --max_width=1280
```


## Reference
If you find our released models useful, please consider citing: 

```bibtex
@inproceedings{gao2019dynamic,
  title={Dynamic scene deblurring with parameter selective sharing and nested skip connections},
  author={Gao, Hongyun and Tao, Xin and Shen, Xiaoyong and Jia, Jiaya},
  booktitle={Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition},
  pages={3848--3856},
  year={2019}
}
```