FFmpeg is the leading multimedia (audio, video) framework, able to decode, encode, transcode, mux, demux, stream, filter and play pretty much anything that humans and machines have created. It supports the most obscure ancient formats up to the cutting edge.

# Terminology

**Muxer (multiplexer)**

Combines multiple input streams (such as audio, video, subtitles, metadata, etc.) into a single output file or stream. This process is called "multiplexing" or "muxing". The muxer ensures that the different streams are synchronized and properly formatted within the output container format.

# Install

```
sudo apt install ffmpeg
```

# Bitrate

The amount of data (bits) processed per second (bps) i.e. the quality and size of the file.

```
File size (in bits)
  = 500 MB * 8 * 1024 * 1024
  = 4194304000 bits

Duration (in seconds)
  = 2 hours * 60 minutes * 60 seconds
  = 7200 seconds

Bitrate (in bits per second)
  = 4194304000 bits / 7200 seconds
  ≈ 582500 bps
  ≈ 583 kbps
```

Video:

1080p is the most commonly used one.

| Quality       | Bitrate        | Resolution      |
| ------------- | -------------- | --------------- |
| 4K            | 30+ Mbps       | 3840 x 2160     |
| QHD 1440p     | 13,000 kbps    | 2560 x 1440     |
| **FHD 1080p** | **6,000 kbps** | **1920 x 1080** |
| HD 720p       | 4,000 kbps     | 1280 x 720      |
| SD 720p       | 2,000 kbps     | 640 x 480       |
| SD 480p       | 1,000 kbps     | 854 x 480       |
| SD 360p       | 600 kbps       | 640 x 360       |
| SD 240p       | 400 kbps       | 426 x 240       |

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

| Preset     | Speed                     |          | File Size              |        |
| ---------- | ------------------------- | -------- | ---------------------- | ------ |
| ultrafast  | 10-20 times faster        | 3 min    | 2-3 times larger       | 3 GB   |
| superfast  | 5-10 times faster         | 6 min    | 1.5-2.5 times larger   | 2.5 GB |
| veryfast   | 3-5 times faster          | 15 min   | 1.3-2 times larger     | 2 GB   |
| faster     | 2-3 times faster          | 20 min   | 1.2-1.8 times larger   | 1.8 GB |
| fast       | 1.5-2 times faster        | 30 min   | 1.1-1.5 times larger   | 1.5 GB |
| **medium** | **Baseline speed**        | 1 hour   | **Baseline file size** | 1 GB   |
| slow       | 1.5-2 times slower        | 2 hours  | 0.8-0.9 times smaller  | 900 MB |
| slower     | 2-3 times slower          | 3 hours  | 0.7-0.85 times smaller | 800 MB |
| veryslow   | 3-5 times slower          | 5 hours  | 0.6-0.8 times smaller  | 700 MB |
| placebo    | 5-10 times slower or more | 10 hours | 0.5-0.75 times smaller | 600 MB |

# Video

```bash
 ffmpeg -i input.mp4 -vf "scale=1280:-1" -b:v 5000k -b:a 128k -preset medium output.mp4
```

-   `-i` stands for input.
-   `-vf "scale=1280:-1"` scales the video width to 1280 pixels while maintaining the aspect ratio. Adjust the resolution as needed.
-   `-b:v 5000k` sets the video bitrate to 5,000 kbps.
-   `-b:a 128k` sets the audio bitrate to 128 kbps. Adjust this value if necessary based on the audio quality required.
-   `-preset medium` sets the encoding speed/quality balance.

**Result**

```
Preset:                         medium       superfast
Processing:                     1 hour       20 min

Duration:      2 hours          same         same
Size:          2 GB             554 MB       554 MB
Width:         1280             same         same
Height:        720              same         same

Video
Data rate:      2,572 kbps      584 kbps     584 kbps
Bit rate:       2,700 kbps      712 kbps     712 kbps
Frame rate:     25 fps          same         same

Audio
Bitrate:        128kbps         same         same
Sample rate:    44.100 kHz      same         same
```

# Audio

```
ffmpeg -i input.wav -b:a 320k output.mp3
```

**Result**

```
Processing:                    4 min

Duration:       3 hours        same
Size:           1.9 GB         428 MB

Bit rate:       1,411 kbps     320 kbps
```

# Batch script

Process multiple files.

```bash
#!/bin/bash

# Directory containing the videos
input_dir="path/to/input_videos"
output_dir="path/to/output_videos"

# Create the output directory if it doesn't exist
mkdir -p "$output_dir"

# Loop through all .mp4 files in the input directory
for input_file in "$input_dir"/*.mp4; do
  # Get the base name of the file (without path and extension)
  base_name=$(basename "$input_file" .mp4)

  # Define the output file path
  output_file="$output_dir/${base_name}_720p.mp4"

  # Run ffmpeg to resize the video to 720p
  ffmpeg -i "$input_file" -vf "scale=1280:720" -c:a copy "$output_file"
done
```
