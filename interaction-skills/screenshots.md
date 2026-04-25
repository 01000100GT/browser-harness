# Screenshots

`capture_screenshot()` writes a PNG at **device pixels** (e.g. 4592×2286 on a 2× display), but `click_at_xy()` takes **CSS pixels** (e.g. 2296×1143). Reading a coordinate off the image and clicking it directly will miss by a factor of `window.devicePixelRatio` on any HiDPI / Retina display.

```python
from PIL import Image
path = capture_screenshot()
img = Image.open(path)
dpr = js("window.devicePixelRatio") or 1
# (px, py) read off the image in image pixels
click_at_xy(px / dpr, py / dpr)
```

`page_info()` returns CSS dimensions (`w`, `h`) — compare against `img.size` to see the ratio for the current display.

## Discovery vs verification

- Discovery: `capture_screenshot(full=True)` captures the full scrollable page, including content below the fold. Use it to find a target before scrolling.
- Verification: plain `capture_screenshot()` (viewport only) is enough to confirm a click landed, a menu opened, or a navigation completed.

After every meaningful action, re-screenshot before assuming it worked.
