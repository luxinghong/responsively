# Responsively - Responsive Design Preview Tool

A lightweight responsive design preview tool that allows you to preview web pages across multiple device sizes simultaneously, with synchronized operations and pagination support.

![Demo Screenshot](https://ji-zhu.com/static/pics/responsively_compact.gif)

> The website shown in the demo is [Ji-Zhu (记住)](https://ji-zhu.com) (https://ji-zhu.com) - a quite excellent niche learning website, worth checking out.

## Features

- 🖥️ **Multi-Device Preview**: Preview web pages across multiple device sizes simultaneously
- 📱 **Device Groups**: Pre-configured 4xl desktop and mobile (<md) device groups
- 🔄 **Synchronized Operations**: Operations in one device can be synchronized to others (when same-origin)
- 📄 **Pagination**: Grid mode supports pagination to avoid performance issues
- 🎛️ **Layout Toggle**: Supports both vertical and grid display modes
- ⚡ **Performance Optimized**: Lazy loading iframes to prevent browser crashes

## Quick Start

### Integrate with Existing Application
**Important**: To enable synchronized operations, the tool must be deployed under the same domain as your target application.

#### Nginx Configuration
```nginx
location /tools/responsively {
    alias /path/to/your/responsively.html;
    add_header Content-Type text/html;
    
    # Optional: Disable cache for debugging
    add_header Cache-Control "no-cache, no-store, must-revalidate";
    add_header Pragma "no-cache";
    add_header Expires "0";
}
```

#### Deployment Example
Assuming your application runs on `http://localhost`:
1. Place `responsively.html` on your server path
2. Access the tool via `http://localhost/tools/responsively`
3. The tool loads `http://localhost` (TARGET_URL) in multiple iframes
4. Since both main page and iframes are under the same domain, sync operations work properly

## Usage

1. **Select Device Group**: Choose a device group from the dropdown menu
2. **Toggle Display Mode**:
   - **Vertical**: Devices arranged vertically, max 6 devices
   - **Grid**: Devices in grid layout, 4 per row with pagination support
3. **Enable Sync Operations**: Check the "Sync Operations" checkbox (only works with same-origin)
4. **Pagination**: Use Previous/Next buttons in grid mode

## Device Groups

> **Note**: All device dimension data comes from real-world devices, providing highly practical reference value.

### 4xl Group (Desktop Devices)
- 2560x1271, 2560x919, 2552x1284
- 2560x1440, 2560x1313, 3064x1580
- 3440x1243, 2844x1464, 2552x970
- 2752x983, 3840x2071, 3024x1764
- 2552x1284

### <md Group (Mobile Devices)
- 360x706, 360x680, 360x560, 360x740
- 393x754, 393x851, 393x599
- 390x753, 390x844, 390x660
- 412x732, 412x820, 412x544
- 400x780, 384x745, 384x696
- 414x804, 430x834, 375x724
- 428x835, 402x774

## Technical Notes

### Sync Operations and TARGET_URL Relationship
The core of synchronized operations is that the main page and TARGET_URL must be same-origin:

#### Same-Origin Example (✅ Sync Works)
- Main page: `http://localhost/tools/responsively`
- TARGET_URL: `http://localhost`
- Result: Same domain, sync operations work normally

#### Cross-Origin Example (❌ Sync Blocked)
- Main page: `http://localhost:8080/responsively.html`
- TARGET_URL: `http://localhost`
- Result: Different ports, browser blocks iframe access

#### Key Points
1. **Domain must match**: Main page and TARGET_URL domain must be identical
2. **Port must match**: Even with same domain, different ports are considered cross-origin
3. **Protocol must match**: Cannot mix http and https
4. **Deployment location matters**: Tool must be deployed on the target application's server

### Performance Optimizations
- **Lazy Loading**: Grid mode loads iframes sequentially with 1-second intervals
- **Memory Management**: Clears old iframes when switching groups to prevent memory leaks
- **Pagination Limits**: Max 8 devices per page to prevent browser crashes

## Customization

### Modify Target URL
Change the `TARGET_URL` variable in the code:
```javascript
const TARGET_URL = "http://localhost"; // Change to your target URL
```

### Add Device Sizes
Add new device configurations to the `GROUPS` array:
```javascript
{
  name: "Custom Group",
  devices: [
    { name: "1920x1080", width: 1920, height: 1080 },
    // Add more devices...
  ]
}
```

### Modify Page Size
Change the `PAGE_SIZE` constant:
```javascript
const PAGE_SIZE = 8; // Number of devices per page
```

## Troubleshooting

### Issue: iframes won't load
- Check if target URL is accessible
- Verify there are no CORS restrictions

### Issue: Sync operations don't work
- Ensure main page and target page are same-origin
- Check browser console for cross-origin errors

### Issue: Browser crashes
- Reduce number of simultaneously displayed devices
- Use pagination feature
- Check system memory usage

## Browser Support

- Chrome 60+
- Firefox 55+
- Safari 12+
- Edge 79+

## License

MIT License