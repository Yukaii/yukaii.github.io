---
layout: post
title: Upload binary file with netcat(nc) command without scp/sftp (Recover DD-WRT with flash firmware through command line)
---

Last weekend, I tried to fix a TP-Link router with DD-WRT firmware.

My old friend flashed it with beta version firmware, and its web interface got broken, so there's no way to upload the new firmware via web interface. Then I tried `telnet` into it, and it works! But the `ssh` service cannot be started with command line (a least I didn't find a way), so the `scp` command would not function either.

I found the post [Using Netcat for File Transfers][netcat-transfer]. With the help of netcat, you can start a file listening server on the router(receiver), then upload the firmware binary from the client side(sender).

Run the first command in DD-WRT console:

```bash
nc -l -p 1234 > dd-wrt.bin
```

Then run the command for sending file

```bash
nc -w 3 192.168.1.1 1234 < dd-wrt.bin
```

Where `-w` argument for transfer timeout. The default IP address of DD-WRT router is `192.168.1.1`.

Finally follow the [installation guide of DD-WRT][ddwrt-flash], flash the firmware with the commands:

```bash
write dd-wrt.bin linux
```

My router is back now :p.

[netcat-transfer]: https://nakkaya.com/2009/04/15/using-netcat-for-file-transfers/
[ddwrt-flash]: http://www.dd-wrt.com/wiki/index.php/Installation#Method_3:_Flashing_with_Command_Line
