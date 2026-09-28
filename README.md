# Trimarr
<img width="2042" height="447" alt="Trimarr" src="assets/trimarr-logo.png" />

Automated media transcoding, cleanup, and optimization for Radarr and Sonarr libraries.

Trimarr watches your Radarr and Sonarr imports and runs each file through a reusable Flow, a set of your own rules for video codec, hardware encoder, quality, audio and subtitle language priority, subtitle handling, and cleanup. It then replaces the original file once processing succeeds.

Trimarr integrates directly with Radarr and Sonarr, processing each file automatically once it lands in your library, plus on-demand runs for anything already there. It supports hardware-accelerated transcoding on Intel QSV, NVIDIA NVENC, and AMD VAAPI/AMF, with full control over audio and subtitle language priority, commentary-track removal, and subtitle embedding or extraction. Audio and subtitle tracks left tagged as undetermined by the source release get their actual spoken language identified automatically using Whisper speech detection. It can detect and crop black bars, strip Dolby Vision metadata for broader compatibility, and cap resolution, channel count, and bitrate to keep libraries lean. Trailer files are picked up and processed automatically through a Trailarr integration. Processing runs on a weekly schedule with full power, low power, and paused modes, and the web UI shows a live queue with progress and ETA, a full library browser, and a running total of storage saved. For larger setups, Trimarr Pro adds support for external processing nodes, letting you spread transcode load across multiple machines. The whole thing runs behind a dark, responsive web interface.

## Quick start

```yaml
services:
  trimarr:
    image: thehef/trimarr:latest
    container_name: trimarr
    restart: unless-stopped
    environment:
      PUID: 1000
      PGID: 1000
      TZ: Europe/Copenhagen
    ports:
      - "8000:8000"
    volumes:
      - /path/to/trimarr/data:/data
      - /path/to/downloads:/downloads
      - /path/to/movies:/movies
      - /path/to/tv:/tv
    #devices:
    #  - /dev/dri:/dev/dri
    #deploy:
    #  resources:
    #    reservations:
    #      devices:
    #        - driver: nvidia
    #          count: 1
    #          capabilities: [gpu]
```

The `/movies` and `/tv` volume paths must exactly match the container-side paths Radarr and Sonarr themselves use for the same folders in their own containers, since Trimarr locates files using the paths their APIs report.

The `devices` line is for Intel QSV or AMD VAAPI hardware encoding and requires the host's `/dev/dri` render node. The `deploy` block is for NVIDIA and requires the NVIDIA Container Toolkit on the host. Both sections are commented out by default so the example works unmodified on any host; uncomment whichever one matches your hardware. Both can be active at once if the host has both an Intel or AMD GPU and a separate NVIDIA GPU.

Open the container's port 8000 in a browser and connect your Radarr and Sonarr instances to get started.

## Links

- [Docker Hub](https://hub.docker.com/r/thehef/trimarr)

Trimarr is free to download, use, and modify for personal or internal use; external processing nodes require a Trimarr Pro license. Third-party components bundled in the image (ffmpeg, MKVToolNix, and others) are each listed with their own license in the source repository's `THIRD-PARTY-NOTICES.md`.
