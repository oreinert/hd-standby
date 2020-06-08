# hd-standby

A small Linux utility to make it easy to configure a mechanical harddisk to
enter standby mode (where the platters stop spinning) after a certain period of
inactivity.

The implementation is based on the `hdparm`(8) program to control the disks.

A systemd service unit can be used to ensure that the configuration is always
applied consistently when the computer boots as well as waking up from a
low-power state.

## Usage

The script `hdstandby` applies a timeout value to a disk:

    hdstandby <disk> <timeout>

The parameters are:

 * `disk`: The name of a disk (see below) to apply the timeout to.
 * `timeout`: A timeout value like the one given to `hdparm -S`.

The script checks that a rotational disk is specified, but other than that
there's no error checking. Even though this permits setting a timeout on types
of block devices that are not plain harddisks (e.g., MD arrays), doing so is
not supported, and the effects of it are undefined.

## Configuration

To configure a permanent standby timeout (10 minutes by default) for a disk
(e.g., `/dev/sda`), run:

    systemctl enable --now hd-standby@sda

Repeat this for every disk in your system you want to apply a standby timeout to.

## Disk names

You can use names found in the following directories to specify a disk name:

  * `/dev/`
  * `/dev/disk/by-id/`
  * `/dev/disk/by-path/`

For example, if the name `/dev/disk/by-path/pci-0000:00:04.0` refers to a
harddisk on your system, you can enable the standby timer for it with

    systemctl enable --now hd-standby@pci-0000:00:04.0

## References

 * [`hdparm`(8)](https://man7.org/linux/man-pages/man8/hdparm.8.html)
