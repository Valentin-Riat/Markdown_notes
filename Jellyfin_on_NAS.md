# Jellyfin on docker on a NAS

[Tuto for setting up jellyfin on a Synology NAS with hardware acceleration](https://www.wundertech.net/how-to-set-up-jellyfin-on-a-synology-nas/)

## Activate hardware acceleration

To know what CPU architecture is in the NAS : https://kb.synology.com/en-global/DSM/tutorial/What_kind_of_CPU_does_my_NAS_have
To know what CPU architecture supports what CODEC for HW accel : https://en.wikipedia.org/wiki/Intel_Quick_Sync_Video



# Video encoding

## General info
H.264 (=AVC)  : is the mode wide spread encoding for streaming because high quality with low bit rate
H.265 (=HEVC) : is the successor to H.264. It's about 2x more efficient (half the bit rate for the same quality)

for max 

## Handbrake Software

Used to encode a file 

## MKVToolNix Software

Used to modify .MKV files. useful to modify subtitles

## MakeMKV Software

Used to rip DVD/Blu-ray into mkv files


# Hardware

NAS : synology DS718+  (supports harware acceleration for H.264 and H.265)

TV Val : UE46F8080ST (only supports H.264)
TV GE cave : QE75Q7FAMTXZG (supports H.264 and H.265)
TV GE salon : UE55HU8580Q (supports H.264 and H.265)


# TODO

 - [ ] Play with encoding to get best quality/file size ratio 
 - [ ] Open connection to internet
 - [ ] Install Jellyfin on a TV