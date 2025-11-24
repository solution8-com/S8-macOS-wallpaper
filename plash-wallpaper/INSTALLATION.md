# Quick Start Guide

## Installation (5 minutes)

### Step 1: Download Plash
- Download [Plash](https://sindresorhus.com/plash) from Mac App Store (Free)
- Or visit: https://apps.apple.com/app/plash/id1494023538

### Step 2: Start Local Server
Open Terminal and run:
```bash
cd /path/to/meh/plash-wallpaper
python3 -m http.server 8000
```

Or use the provided script:
```bash
cd /path/to/meh/plash-wallpaper
./serve.sh
```

### Step 3: Configure Plash
1. Open Plash from menu bar
2. Click "Add Website..."
3. Enter URL: `http://localhost:8000`
4. Click "Add"

### Step 4: Customize (Optional)
- **Opacity**: Adjust in Plash menu
- **Display**: Choose which monitor(s)
- **Reload**: Right-click Plash icon → Reload

## What You'll See

A beautiful, animated warped gradient background that:
- ✨ Smoothly animates and morphs
- 🎨 Uses purple/blue color palette
- 🖱️ Allows desktop icons to remain clickable
- 📱 Automatically adapts to screen size
- ⚡ Runs at smooth 60 FPS

## Troubleshooting

### "Cannot connect" error
- Make sure the server is running (check Terminal)
- Verify URL is `http://localhost:8000` (not https)
- Try restarting the server

### Black screen
- Check browser console (right-click → Inspect)
- Verify three.min.js loaded successfully
- Try refreshing Plash (right-click icon → Reload)

### Poor performance
- Lower geometry resolution in animation.js
- Reduce number of warp layers
- Close other GPU-intensive applications

### Desktop icons not clickable
- This should work automatically
- Check Plash "Click Through" setting is enabled

## Customizing Colors

Edit `animation.js`, find the `palette()` function around line 50:

```javascript
vec3 palette(float t) {
  vec3 a = vec3(0.5, 0.5, 0.5);
  vec3 b = vec3(0.5, 0.5, 0.5);
  vec3 c = vec3(1.0, 1.0, 1.0);
  vec3 d = vec3(0.263, 0.416, 0.557);  // Change these for different colors!
  
  return a + b * cos(6.28318 * (c * t + d));
}
```

And the base color around line 80:
```javascript
vec3 baseColor = vec3(0.09, 0.09, 0.22); // RGB values (0-1)
```

## Running on Boot

To start the server automatically on login:

1. Create a LaunchAgent file:
```bash
mkdir -p ~/Library/LaunchAgents
nano ~/Library/LaunchAgents/com.plash.wallpaper.plist
```

2. Add this content (update PATH):
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>Label</key>
    <string>com.plash.wallpaper</string>
    <key>ProgramArguments</key>
    <array>
        <string>/usr/bin/python3</string>
        <string>-m</string>
        <string>http.server</string>
        <string>8000</string>
    </array>
    <key>WorkingDirectory</key>
    <string>/FULL/PATH/TO/meh/plash-wallpaper</string>
    <key>RunAtLoad</key>
    <true/>
    <key>KeepAlive</key>
    <false/>
</dict>
</plist>
```

3. Load it:
```bash
launchctl load ~/Library/LaunchAgents/com.plash.wallpaper.plist
```

## Support

For issues or questions:
- Check the main README.md
- Review THREE.js documentation
- Open an issue on GitHub

Enjoy your animated wallpaper! 🎉
