## HiLens 模型支持列表

HiLens 目前支持DDK893, DDK893支持能够转换的模型算子列表，可在HiLens的文档网站进行查询，所有支持的算子创建的模型，都可用在ModelArts 或者 HiLens平台进行模型在线转换。


Tensorflow 模型：
1. `L2_normalize` 默认的eps是`1e-13`，因为内部是`FP16`计算，这个eps太小会校验认为不合法导致融合失败。另外`axis=-1`改`3，目前用的都是非负数，负数会报错。
2. `leakyReLu` ( tf 1.12及以下版本支持 )
3. tensorflow object detection api得到的pb需要特殊处理，去掉check的部分节点
4. tensorflow 的模型用于转换时，需要用frozen之后的pb模型进行转。




## Tensorflow 模型支持列表

| 模型 | 输入图像大小 |	是否支持 |	模型转换前大小(MB) |	模型转换后大小(MB) |
| :---: | :---: | --- | --- | --- | 
| Alexnet |	224*224 | 是 | 176 | 88.3 |
|InceptionV1 | 	224*224 | 是 |	21.4 |	11 |
| InceptionV3 |	224*224 | 是 |	83.1 |	41.9 |
|InceptionV4 |	224*224 |	是 |	157 |	79.1 |
| mobileNetV1 |	224*224 |	是 |	12.2 |	7.52 |
| mobileNetV2 |	224*224 |	是 |	8.74 |	6.27 |
| Resnet50V1 |	224*224 |	是 |	89.6 |	45.1 |
| Resnet50V2 |	224*224 |	是 |	89.8 |	45.3 |
| VGG16 | 224*224 |	是 |	512 |	256 |
| GoogLeNet | 	224*224 |	是 |	26.7 |	13.7 |
| VGG16_SSD |	300*300 |	是 |	553 |	280 |
| Resnet18_YOLOV3 |	544*544 |	是 |	45.5 |	23 |



## Caffe 模型支持列表


| 模型 | 输入图像大小 |是否支持 |	模型转换前大小(MB) |	模型转换后大小(MB) |
| :---: | :---: | --- | --- | --- | 
| Alexnet |	227*227 |	是 |	232	| 119 |
| InceptionV3 |	395*395 |	是 |	91.1 |	45.8 |
| InceptionV4 |	299*299 |	是 |	163 |	81.9 |
| mobileNetV1 |	224*224 |	是 |	16.2 |	9.44 |
| mobileNetV2 |	224*224 |	是 |	13.5 |	8.71 |
| Resnet101V1 |	224*224 |	是 |	170 |	85.3 |
| VGG16 |	224*224 |	是 |	527 |	119 |
| GoogLeNet |	224*224 |	是 |	51 |	13.7 |
| Mobilenet-SSD |	300*300 |	是 |	22.2 |	20 |
| Resnet18-YOLOV3 |	416*736 |	是 |	45.5 |	22.97 |

## ModelArts 远端算法 支持Ascend 310 推理

https://console.huaweicloud.com/modelarts/?region=cn-north-4#/aiMarket/models/public?list_type=algo


## 可转换算法参考 

| 算法 | 框架 | 功能 | 步骤 | 描述 | 网址 |
| :---: | :---: | :---: | --- | --- | --- | 
| face_landmarks | Caffe | 68个人脸关键点检测 | <br/>1. github页面下载protxt和caffemodel，上传到OBS </br> 2. ModelArts或者HiLens模型转换页面进行转换|  | https://github.com/lsy17096535/face-landmark |
| face_landmarks | Tensorflow | 68个人脸关键点检测 | <br/>1. github页面下载作者训练好的TensorFlow模型，上传到OBS </br>  2. ModelArts或者HiLens模型转换页面进行转换|  | https://github.com/yinguobing/head-pose-estimation |
| openpose | Keras/TensorFlow | 18个人体关键点检测 | <br/>1. github页面下载作者训练好的Keras模型 </br>2. 转换为TensorFlow模型，输出节点是heatmap和pafmap，不做resize，上传到OBS <br/> 3. ModelArts或者HiLens模型转换页面进行转换 | 转换为TensorFlow模型时stage数量和输入大小也可以缩小一些，防止模型太大、运行较慢 | https://github.com/kevinlin311tw/keras-openpose-reproduce |
| cpm | TensorFlow | 21个手部关键点检测 | 1. github页面下载作者训练好的TensorFlow模型，上传到OBS 2. ModelArts或者HiLens模型转换页面进行转换 | 模型的stage数量和输入大小可以缩小一些，防止模型太大、运行较慢 | https://github.com/timctho/convolutional-pose-machines-tensorflow | 
| yolo_v3 | TensorFlow | 物体检测 | <br/> 1. 训练模型，或者github页面下载作者训练好的TensorFlow checkpoint模型 </br> 2. 转换为frozen pb模型，输出节点设为三个feature map，上传到OBS <br/> 3. ModelArts或者HiLens模型转换页面进行转换<br/> | 暂不支持Leaky_relu算子，需要替换为relu算子 | https://github.com/wizyoung/YOLOv3_TensorFlow|
| vgg_ssd | TensorFlow | 物体检测 | <br/>1. 训练模型，保存成ckpt。 </br> 2. 剔除模型的后处理部分（保留6个输出head及其之前的网络结构）<br/> 3.将剩余的模型转换成pb并上传到obs，通过ModelArts或者Hilens进行OM模型的转换<br/> |  | https://github.com/ljanyst/ssd-tensorflow |
| unet | Keras/TensorFlow | 语义分割 | <br/>1. 训练模型，或者github页面下载作者训练好的Keras模型 </br> 2. 转换为TensorFlow frozen pb模型，上传到OBS <br/>3. ModelArts或者HiLens模型转换页面进行转换换</br>|  | https://github.com/zhixuhao/unet |
| deeplabv3+ | TensorFlow | 语义分割 | <br/>1. 训练模型，或者github页面下载作者训练好的Tensorflow checkpoint模型 </br> 2. 转换为TensorFlow frozen pb模型，上传到OBS <br/>3. ModelArts或者HiLens模型转换页面进行转换</br>| tf.image_resize算子对align_corner参数有一定限制，某些情况下不支持align_corner=True | https://github.com/tensorflow/models/tree/master/research/deeplab |
| densenet | Keras/TensorFlow | 物体分类 | <br/>1. 训练模型，保存成frozen pb格式，上传到obs </br>2. ModelArts或者Hilens进行OM模型的转换 <br/>|  |https://github.com/LaurentMazare/deep-models/tree/master/densenet |
| shufflenet_v2 | TensorFlow | 物体分类 | <br/>1. 训练模型，或者github页面下载作者训练好的Tensorflow checkpoint模型 </br>2. 转换为TensorFlow frozen pb模型，上传到OBS <br/>3. ModelArts或者HiLens模型转换页面进行转换</br>| tf.transpose算子暂不支持5维数据的转换，在固化模型时，concat_shuffle_split中的相关操作需要使用其他方式替代 | https://github.com/TropComplique/shufflenet-v2-tensorflow |
