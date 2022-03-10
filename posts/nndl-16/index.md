# ResNet--人脸识别Demo


ResNet--Face Recognation
<!--more-->

[**项目Github地址**](https://github.com/coneypo/Dlib_face_recognition_from_camera)

# 安装踩坑
## 安装dlib
```bash
pip3 install opencv-python
pip3 install scikit-image
pip3 install dlib
```
在安装dlib时报错,提示安装visual studio c++运行库
**解决方式**
下载对应python3.7的whl文件(注意python版本对应)
在目录下执行
```bash
pip3 install dlib-19.17.99-cp37-cp37m-win_amd64.whl
```

## 使用国内镜像
### 单次安装
使用 -i 指定镜像源
```bash
pip3 install numpy -i https://pypi.tuna.tsinghua.edu.cn/simple
```
### Windows文件配置
Windows下，你需要在当前对用户目录下（C:\Users\xx\pip，xx 表示当前使用对用户，比如张三）创建一个 pip.ini在pip.ini文件中输入以下内容：
```bash
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
[install]
trusted-host = pypi.tuna.tsinghua.edu.cn
```

## vscode中的pylint对cv2提示报错
[参考blog](https://blog.csdn.net/qq_33757398/article/details/107673099)
在vscode的setting中找到参数
```bash
Pylint Args
```
![](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/NNDL/16/1.png)
之后添加两项即可
```bash
--errors-only
--generated-members=numpy.* ,torch.* ,cv2.* , cv.*
```

# 参数解释
## VideoCapture()
```python
cap = cv2.VideoCapture(1) 
#参数为1代表调用外置摄像头,0代表调用本机摄像头(笔记本情况)
```
[参考blog](https://blog.csdn.net/qq_37764129/article/details/82964997)
