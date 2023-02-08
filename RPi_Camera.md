# RPi Camera 擷取影像並做影音串流

參考資料：

* [Getting started with the Camera Module](https://projects.raspberrypi.org/en/projects/getting-started-with-picamera/3)
* [Streaming Video Using VLC Player](https://raspberry-projects.com/pi/pi-hardware/raspberry-pi-camera/streaming-video-using-vlc-player)

## 影像擷取

命令列指令：

擷取影像：

```
$ raspistill -o image.jpg
```

設定影像大小：

```
$ raspistill -o image-small.jpg -w 640 -h 480
```

Python程式：

啟動 preview:

```python
from picamera import PiCamera
from time import sleep

camera = PiCamera()

camera.start_preview()
sleep(5)
camera.stop_preview()
```

使用 capture() 擷取影像：

```python
from picamera import PiCamera
from time import sleep

camera = PiCamera()

camera.start_preview()
sleep(5)
camera.capture('image.jpg')
camera.stop_preview()
```
官網上說，至少要睡2秒，好讓sensor調整光線。

## 視頻擷取

命令列指令：

擷取影像：

```
$ raspivid -o video.h264
```

擷取完後，可用 vlc media player 打開。

影音串流：

RTSP:
```
$ raspivid -o - -t 0 -n | cvlc -vvv stream:///dev/stdin --sout '#rtp{sdp=rtsp://:8554/}' :demux=h264
```

HTTP:

```
$ raspivid -o - -t 99999 | cvlc -vvv stream:///dev/stdin --sout '#standard{access=http,mux=ts,dst=:8090}' :demux=h264
```