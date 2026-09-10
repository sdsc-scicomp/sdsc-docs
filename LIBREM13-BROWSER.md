# Opening a link in the browser on librem13

When the user asks to open a link or URL in their Chrome on the `librem13` machine, SSH in and launch Chrome on the active desktop display.

```bash
ssh librem13 'export DISPLAY=:0; export XAUTHORITY=$(ls /run/user/1000/.mutter-Xwaylandauth.* 2>/dev/null | head -1); export XDG_RUNTIME_DIR=/run/user/1000; setsid google-chrome "URL" >/tmp/open_chrome.log 2>&1 & sleep 3; head /tmp/open_chrome.log'
```

- SSH alias `librem13` is in `~/.ssh/config` (HostName `100.64.32.110`, User zonca).
- Chrome is at `/usr/bin/google-chrome` (also `google-chrome-stable`, `chromium`).
- The desktop session runs on display `:0` (Wayland/Xwayland). The SSH session has no DISPLAY, so set `DISPLAY=:0` and a valid `XAUTHORITY` (the running Xwayland auth file in `/run/user/1000/.mutter-Xwaylandauth.*`).
- A reply of "Opening in existing browser session." means the URL was handed to the already-running Chrome and a new tab opened; that is success.
