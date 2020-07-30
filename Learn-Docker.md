# Docker学习

## 1. 创建新镜像

`docker commit -a "xiongdongdong" -m "my ubuntu" image_id myubuntu:v1 `

`-a`代表作者

`-m`代表提交信息

`myubuntu:v1`代表标签

## 2. 结合阿里云操作

* 将镜像推送到Registry

  `docker login --username=叫我熊咚咚 registry.cn-hangzhou.aliyuncs.com`

  `docker tag [ImageId] registry.cn-hangzhou.aliyuncs.com/xiongdongdong/ubuntu_xiong:[镜像版本号]`

  `docker push registry.cn-hangzhou.aliyuncs.com/xiongdongdong/ubuntu_xiong:[镜像版本号]`