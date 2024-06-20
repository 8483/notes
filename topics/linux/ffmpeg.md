FFmpeg is the leading multimedia (audio, video) framework, able to decode, encode, transcode, mux, demux, stream, filter and play pretty much anything that humans and machines have created. It supports the most obscure ancient formats up to the cutting edge.

# Install

```
sudo apt install ffmpeg
```

# Bitrate

The amount of data (bits) processed per second (bps) i.e. the quality and size of the file.

```
File size (in bits) = 500 MB * 8 * 1024 * 1024
                           = 4194304000 bits

Duration (in seconds) = 2 hours * 60 minutes * 60 seconds
                      = 7200 seconds

Bitrate (in bits per second) = 4194304000 bits / 7200 seconds
                                    ≈ 582500 bps
                                    ≈ 583 kbps
```

Video:

-   4K: 30-60+ Mbps
-   HD: 5-10 Mbps
-   1080p: 3,000 kbps
-   720p: 1,500 kbps (standard)
-   480p: 1,000 kbps
-   360p: 600 kbps
-   240p: 400 kbps

Audio:

-   mp3: 320 kbps

# Video presets

Balance encoding speed and compression efficiency.

Depends on:

-   The complexity and length of the input video.
-   The performance of the hardware (CPU, GPU, memory).
-   The specific settings and parameters used in the FFmpeg command.
-   The codec being used.

Fastest to slowest:

| Preset     | Speed                     | File Size              |
| ---------- | ------------------------- | ---------------------- |
| ultrafast  | 10-20 times faster        | 2-3 times larger       |
| superfast  | 5-10 times faster         | 1.5-2.5 times larger   |
| veryfast   | 3-5 times faster          | 1.3-2 times larger     |
| faster     | 2-3 times faster          | 1.2-1.8 times larger   |
| fast       | 1.5-2 times faster        | 1.1-1.5 times larger   |
| **medium** | **Baseline speed**        | **Baseline file size** |
| slow       | 1.5-2 times slower        | 0.8-0.9 times smaller  |
| slower     | 2-3 times slower          | 0.7-0.85 times smaller |
| veryslow   | 3-5 times slower          | 0.6-0.8 times smaller  |
| placebo    | 5-10 times slower or more | 0.5-0.75 times smaller |

# Video

```bash
 ffmpeg -i input.mp4 -vf "scale=1280:-1" -b:v 583k -b:a 128k -preset medium output.mp4
```

-   `-i` stands for input.
-   `-vf "scale=1280:-1"` scales the video width to 1280 pixels while maintaining the aspect ratio. Adjust the resolution as needed.
-   `-b:v 583k` sets the video bitrate to 583 kbps.
-   `-b:a 128k` sets the audio bitrate to 128 kbps. Adjust this value if necessary based on the audio quality required.
-   `-preset medium` sets the encoding speed/quality balance.

**Result**

```
Preset:                       medium       superfast
Processing:                   1 hour       20 min

Duration:      2 hours        same         same
Size:          2 GB           554 MB       554 MB
Width:         1280           same         same
Height:        720            same         same

Video
Data rate:      2,572 kbps    584 kbps     584 kbps
Bit rate:       2,700 kbps    712 kbps     712 kbps
Frame rate:     25 fps        same         same

Audio
Bitrate:       128kbps        same         same
Sample rate:   44.100 kHz     same         same
```

# Audio

```
ffmpeg -i input.wav -b:a 320k output.mp3
```

**Result**

```
Processing:                   4 min

Duration:      3 hours        same
Size:          1.9 GB         428 MB

Bit rate:       1,411 kbps    320 kbps
```
