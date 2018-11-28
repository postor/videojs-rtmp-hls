# 使用video.js根据环境选择hls和rtmp进行播放

## 1.相关知识

- docker，rtmp转流服务，支持hls： https://store.docker.com/community/images/shaneen31/nginx-rtmp-hls
- 使用ffmpeg将mp4转rtmp推流：ffmpeg -threads 2 -re -fflags +genpts -stream_loop -1 -i ./big_buck_bunny_720p_5mb.mp4 -c copy -ar 44100 -f flv rtmp://192.168.5.138:1936/live/infinitetest
- video.js: https://github.com/videojs/video.js https://github.com/videojs/videojs-flash

## 2.准备hls和rtmp流

如果已经准备好hls和rtmp流可直接跳到下一节

1.启动转流服务，因为80占用所以hls调整到了81

```
docker run -p 1935:1935 -p 81:80 shaneen31/nginx-rtmp-hls
```

2.开启推流

```
 ffmpeg -threads 2 -re -fflags +genpts -stream_loop -1 -i ./big_buck_bunny_720p_5mb.mp4 -c copy -ar 44100 -f flv rtmp://192.168.5.148:1935/live/infinitetest
```

需要提前准备好big_buck_bunny_720p_5mb.mp4文件，192.168.5.148是我的转流服务主机IP

3.测试拉流

使用vlc播放以下地址，hls的地址会慢一点

rtmp://192.168.5.148:1935/live/infinitetest

http://192.168.5.148:81/hls/infinitetest.m3u8

## 3.打开网页测试

把[index.html](./index.html)放到http环境中，并使用浏览器/手机访问

```
git clone https://github.com/postor/videojs-rtmp-hls.git
cd videojs-rtmp-hls
http-server
```

