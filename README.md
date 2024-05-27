# batch-sid-converter

A Bash script to batch-convert a collection of SID files (such as the
[HVSC](https://hvsc.de)) to a playable format (MP3, AAC, OGG) using
[sidplayfp](https://github.com/libsidplayfp/sidplayfp),
[FFmpeg](https://ffmpeg.org/) and
[GNU Parallel](https://www.gnu.org/software/parallel/)

## Features

* Resume (but cannot detect changed SIDs)

* Parallelized conversion (controlled by `-j`, by default uses all CPU cores)

* Is a massive pile of hacks

## tmp-on-tmpfs users

Your tmpfs will have to be pretty big depending on how many jobs
you're runnning, about 32 GB for 16 jobs. Remount `/tmp`:

```console
# mount -o remount,size=32G /tmp
```

and consider using zram swap.

## Examples

To convert the HVSC to 128 kbit/s 44.1 kHz MP3s:

```console
$ batch-sid-converter --codec libmp3lame --extension mp3 --bitrate 128k --rate 44100 C64Music C64Music-MP3
```

To convert to 44.1 kHz 16-bit FLAC:

```console
$ batch-sid-converter -c flac -e flac -r 44100 --16-bit C64Music flacs
```

## How long will it take?

Proportional to however fast your machine can run `sidplayfp` × core count.

As a point of reference, HVSC #80 took 3 days, 3 hours on a
Ryzen 7 3700U (Zen+, 4 cores, 8 threads) to convert.

## How much disk space will it take?

HVSC #80 is 107 days' worth of music. For some common bitrates:

| **Bitrate (kbit/s)**    | **Disk space needed (GB)** |
|-------------------------|----------------------------|
| 96                      | 110                        |
| 128                     | 150                        |
| 160                     | 180                        |
| 192                     | 220                        |
| 256                     | 300                        |
| 320                     | 370                        |
| 400[^1] (16/44.1 FLAC)  | 460                        |
| 705.6 (CD Audio)        | 820                        |
| 1152 (24/48)            | 1300                       |
| 2304 (24/96)            | 2700                       |
| 4608 (24/192)           | 5300                       |

[^1]: Estimated from a random sample of 1000 SIDs.
