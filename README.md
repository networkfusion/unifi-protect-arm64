# Ubiquiti UniFi Protect container for ARM64

Run UniFi Protect in a Docker container on ARM64 hardware.

## Usage

NOTE: This container needs root access, so use `sudo` if necessary.
Run the container as a daemon:

```bash
docker run -d --name unifi-protect-arm64  \
    --privileged \
    --tmpfs /run \
    --tmpfs /run/lock \
    --tmpfs /tmp \
    -v /sys/fs/cgroup:/sys/fs/cgroup:ro \
    -v ~/unifi-protect/srv:/srv \
    -v ~/unifi-protect/data:/data \
    -v ~/unifi-protect/persistent:/persistent \
    --network host \
    markdegroot/unifi-protect-arm64:latest
```

Now you can access UniFi Protect at `https://localhost/`.

## Storage
UniFi Protect needs a lot of storage to record video. Protect will fail to start if there is not at least 100GB disk space available, so make sure to store your Docker volumes on a disk with some space (`/storage` in the above run command).

### Using a different drive for storage
This will help when running the docker on the root file system, but need to have a large enough drive.
Note: the drive must be formatted to `ext4` (NTFS, EXFAT and FAT will probably not work).
NOTE: This container needs root access, so use `sudo` if necessary.

```bash
docker run -d --name unifi-protect-arm64  \
    --privileged \
    --tmpfs /run \
    --tmpfs /run/lock \
    --tmpfs /tmp \
    -v /sys/fs/cgroup:/sys/fs/cgroup:ro \
    -v /media/sdcard/unifi-protect/srv:/srv \
    -v /media/sdcard/unifi-protect/data:/data \
    -v /media/sdcard/unifi-protect/persistent:/persistent \
    --network host \
    markdegroot/unifi-protect-arm64:latest
```

## Stuck at "Device Updating"
If you are stuck at a popup saying "Device Updating" with a blue loading bar after the initial setup, just run `systemctl restart unifi-core` inside the container or restart the entire container. This happens only the first time after the initial setup.

## Build your own container
To build your own container put the deb file for `unifi-core` (for unifi-protect 1.17.3 you need unifi-core 1.6.65) in the `put-unifi-core-deb-here` folder and run:
```bash
docker build -t markdegroot/unifi-protect-arm64 .
```

## Disclaimer
This Docker image is not associated with Ubiquiti Unifi in any way. We do not distribute any third party software and only use packages that are freely available on the internet.
