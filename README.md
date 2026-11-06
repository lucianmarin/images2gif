# images2gif

Pillow-compatible version of `images2gif` for reading and writing animated GIF files in Python using `PIL` / `Pillow` and `NumPy`.

## Compatibility Fix

The original `images2gif` module relied on `PIL.GifImagePlugin.getheader(im)[1]`, which broke in Pillow 3.x+. This version updates palette extraction to use Pillow's `im.palette.getdata()[1]`:

```python
for im in images:
    palettes.append(im.palette.getdata()[1])
```

## Features

- **`writeGif(filename, images, duration=0.1, repeat=True, dither=False, nq=0, subRectangles=True, dispose=None)`**:
  - Writes a sequence of PIL `Image` objects or `numpy` arrays (`uint8` `0–255` or `float` `0.0–1.0`) as an animated GIF.
  - Supports custom per-frame or uniform `duration`, loop counts (`repeat`), dithering (`dither`), automatic changed-region cropping (`subRectangles`) for smaller file sizes, and frame disposal modes (`dispose`).
  - Includes a pure-Python **NeuQuant** neural-network color quantization implementation (`nq=1..100`) for higher-quality 256-color palettes.
- **`readGif(filename, asNumpy=True)`**:
  - Reads all frames from an animated GIF and returns them as a list of `numpy` arrays (or PIL `Image` instances when `asNumpy=False`).

## Requirements

- `Pillow` (`PIL`)
- `numpy`
- `scipy` (optional, for resizing utilities)

## Usage

```python
from PIL import Image
from images2gif import readGif, writeGif

# Read frames from an existing animated GIF
frames = readGif("input.gif", asNumpy=False)

# Write PIL images or NumPy arrays to an animated GIF
writeGif("output.gif", frames, duration=0.1, repeat=True, subRectangles=True)
```
