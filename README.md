# webdav-fetch

Minimal WebDAV client for Linux. List and download files using nothing but `bash` and `curl`.

## Requirements

- `curl` (pre-installed on Debian)
- Optional: WireGuard for VPN routing

## Install

```bash
chmod +x webdav
sudo cp webdav /usr/local/bin/webdav
```

## Configure

Edit the top of the script:

```bash
URL="https://your-webdav-server.com"
USER="your-username"
PASS="your-password"
VPN=""   # optional: set to VPN interface e.g. wg0
```

## Usage

List a directory:
```bash
webdav ls /
webdav ls /folder/
```

Download a single file:
```bash
webdav get /folder/file.mp4
```

Download all files in a folder:
```bash
webdav ls /folder/ | grep -v '/$' | while read -r path; do
  webdav get "$path"
done
```

Filter by file type:
```bash
webdav ls /folder/ | grep '\.mp4$' | while read -r path; do
  webdav get "$path"
done
```

## VPN (optional)

Set `VPN="wg0"` (or your interface name) at the top of the script to route all traffic through WireGuard. Check your interface name with `sudo wg show`.

## Rename URL-encoded filenames (optional)

If downloaded filenames contain `%20`, `%28` etc., run this in the download folder:

```bash
python3 -c "import urllib.parse, os; [os.rename(f, urllib.parse.unquote(f)) for f in os.listdir('.') if urllib.parse.unquote(f) != f]"
```
