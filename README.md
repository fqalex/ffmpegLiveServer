# ffmpegLiveServer
A live video server based on the ffmpeg 
基于ffmpeg的直播服务器，需要安装ffmpeg，python或nodejs

# Linux启动方法 Start the server

1. 挂载内存磁盘
shell运行
``./ramdisk.sh``
或者shell里运行
``sudo mount -t tmpfs -o size=32M tmpfs ./ram``
挂载内存磁盘，挂载位置为
``./ram``
2. 利用ffmpeg捕捉屏幕或文件转换为hls视频流
   ````
   cd ./ram
   ffmpeg -video_size 1920x1080 -f x11grab -i :0.0+0,0 -f pulse -ac 2 -i 0 -f pulse -ac 2 -i default -filter_complex amix=inputs=2:duration=longest -preset ultrafast -maxrate 1M -bf 0  -r 30 -g 20 -movflags +faststart -tune zerolatency -hls_time 0.2 -hls_list_size 3 -hls_wrap 10  -hls_allow_cache 0   -preset ultrafast ".live.m3u8"
3. 运行web服务
   以python3为例：
   ````
   python3 -m http.server 8888 #8888为端口号
4. 复制zip中的文件到当前目录
   ````index.html````
5. 浏览器访问ip:8888即可推荐使用Safrai、Chrome、Edge、Firefox等,Firefox 需要设置web服务器的Content-Type:audio/mpegurl