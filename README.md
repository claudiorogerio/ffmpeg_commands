# ffmpeg_commands

## *Dependencies:*
- [x] sudo apt install ffmpeg

## *Examples using ffmpeg via terminal*
> Record WebCam
```shell
ffmpeg -f v4l2 -framerate 30 -video_size 320x240 -i /dev/video0 out.mp4
```


> Record Microphone
```shell
ffmpeg -f alsa -ac 2 -ar 44100 -i default out.wav
```


> Record Desktop 1 (1366x768)
```shell
ffmpeg -f x11grab -framerate 30 -video_size 1366x768 -i :0.0  out.mp4
```


> Record Desktop 2 (800x400) initial position
```shell
ffmpeg -f x11grab -video_size 800x400 -i :0.0+pos_x,pos_y  out.mp4
```


> Record Desktop Audio
```shell
pactl list sources | grep 'analog-stereo.monitor' | awk '{print $2}' > audio 
file_object  = open( "audio", "r" )
driver = file_object.read().split()
driver = driver[0] 
file_object.close()

ffmpeg -f pulse -i driver out.mp4
```


> Record Desktop (1366x768) and Microphone
```shell
ffmpeg -f x11grab -framerate 30 -video_size 1366x768 -i :0.0  -f alsa -ac 2 -ar 44100 -i default  out.mp4
```


> Record WebCam and Desktop
```shell
ffmpeg -f v4l2 -framerate 30 -video_size 320x240 -i /dev/video0  -filter_complex  "[0:v]pad=iw:768:0:(oh-ih)/3[left];[left][1:v]hstack" out.mp4
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

> Add dB amplitude volume 
```shell
ffmpeg -i video_01.mp4 -af volume=5dB -c:v copy -ac aac -b:a 192k out.mp4 
```

> Normalize videos audio
```shell
ffmpeg -i video_01.mp4 -filter:a loudnorm out.mp4 
```

> Compression video
```shell
ffmpeg -i video.mp4 -vcodec h264 -acodec mp2 out.mp4
```

> Create audio noise
```shell
ffmpeg -f lavfi -i "anoisesrc=d=5:c=pink:r=44100:a=0.5" noise.wav 
```

> Add filters low and highpass
```shell
ffmpeg -i audio.wav -af "highpass=f=200, lowpass=f=3000" out.wav
```

> Images to GIF
```shell
ffmpeg -framerate 1/5 -f image2 -i images_%d.png  out.gif 
```

> Images to MP4
```shell
ffmpeg -framerate 1/5 -f image2 -i images_%d.png  out.mp4 
```

> MP4 to GIF
```shell
ffmpeg -i out.mp4 -vf fps=${3:-10},scale=${2:-640}:-1:flags=lanczos,palettegen out.png
ffmpeg -i out.mp4 -i out.png -filter_complex "fps=${3:-10},scale=${2:-640}:-1:flags=lanczos[x];[x][1:v]paletteuse" out.gif
rm out.png
```
