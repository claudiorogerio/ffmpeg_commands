# ffmpeg_commands
## *Examples to using ffmpeg via terminal*


* *Dependencies:*
- [x] sudo apt install ffmpeg

> Record WebCam
```shell
ffmpeg -f v4l2 -framerate 30 -video_size 320x240 -i /dev/video0
```


> Record Microphone
```shell
ffmpeg -f alsa -ac 2 -ar 44100 -i default
```


> Record Desktop
```shell
ffmpeg -f x11grab -framerate 30 -video_size 1366x768 -i :0.0 
```


> Record Audio Desktop
```shell
pactl list sources | grep 'analog-stereo.monitor' | awk '{print $2}' > audio 
file_object  = open( "audio", "r" )
driver = file_object.read().split()
driver = driver[0] 
file_object.close()

ffmpeg -f pulse -i driver
```


> Record Desktop (1366x768) and Microphone
```shell
ffmpeg -f x11grab -framerate 30 -video_size 1366x768 -i :0.0  -f alsa -ac 2 -ar 44100 -i default 
```


> Record WebCam and Desktop
```shell
ffmpeg -f v4l2 -framerate 30 -video_size 320x240 -i /dev/video0  -filter_complex  "[0:v]pad=iw:768:0:(oh-ih)/3[left];[left][1:v]hstack"
```

> Scale videos
```shell
ffmpeg -i video_01.mp4 -vf scale=1920:1080 -preset slow -crf 10 out.mp4
```


> Merge videos
```shell
ffmpeg -i video_01.mp4 -i video_02.mp4 -filter_complex "[0:v:0][0:a:0][1:v:0][1:a:0]concat=n=2:v=1:a=1[outv][outa]" -map "[outv]" -map "[outa]" out.mp4
```

> Add audio in video
```shell
ffmpeg -i video.mp4 -i audio.wav -c:v copy -map 0:v:0 -map 1:a:0 out.mp4
```


> Create audio sinusoid mono/stereo
```shell
ffmpeg -f lavfi -i "sine=frequency=200:duration=3" sine_200.wav
ffmpeg -f lavfi -i "sine=frequency=500:duration=3" sine_500.wav
ffmpeg -f lavfi -i "sine=frequency=1000:duration=5" -ac 2 sine_1000.wav
ffmpeg -f lavfi -i "sine=frequency=1500:duration=5" -ac 2 sine_1500.wav
```


> Merge sequential audios
```shell
ffmpeg -i sine_200.wav -i sine_500.wav -filter_complex amix=inputs=2:duration=first:dropout_transition=2  out.wav
```

> Add dB amplitude volume in videos
```shell
ffmpeg -i video_01.mp4 -af volume=5dB -c:v copy -ac aac -b:a 192k out.mp4 
```

> Normalize videos audio
```shell
ffmpeg -i video_01.mp4 -filter:a loudnorm out.mp4 
```



> Create audio noise
```shell
ffmpeg -f lavfi -i "anoisesrc=d=5:c=pink:r=44100:a=0.5" noise.wav -y
```


> Add filters low and highpass
```shell
ffmpeg -i audio.wav -af "highpass=f=200, lowpass=f=3000"
```
