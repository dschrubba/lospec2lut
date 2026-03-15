# lospec2lut
Converts a Lospec palette to a 3D LUT file

### Currently supported output formats:

| Format | Description                                              |
|--------|----------------------------------------------------------|  
| .cube  | Adobe/DaVinci Resolve standard (floating-point, 0.0–1.0) |  
| .3dl   | Autodesk/Lustre format (integer, selectable bit depth)   |  

## How it works

A Lospec palette is a small set of discrete RGB colors (usually 2–256).
A 3D LUT maps every possible RGB input value to an output value via a
regular grid. This script populates the grid using nearest-neighbor
quantization: each grid point is mapped to whichever palette color is
closest in color space. The result is a "palette snapper" LUT — run any
image through it and colors will be snapped to the nearest palette entry.

## Distance metrics
----------------
#### sRGB (default)  
Euclidean distance in gamma-encoded sRGB space.  
Fast, works well for most palettes.

#### Linear
Euclidean distance in linear light.   
More perceptually uniform, try if results look off.

## LUT sizes
| Size | Entries         | Description                                     |
|------|-----------------|-------------------------------------------------|  
| 17   | 4,913 entries   | Fast, lower boundary precision                  |
| 33   | 35,937 entries  | Good default with precise boundaries            |
| 65   | 274,625 entries | High quality with noticeably slower to generate |

Palette snapping LUTs don't need huge sizes. The quantization is always sharp, but a larger grid laces the color-zone boundaries more accurately, which matters most for palettes with closely neighbouring colors.

## Usage
```
# fetch from Lospec API
python lospec2lut.py <slug>    
```
```
python lospec2lut.py <slug> --size 65
```
```
python lospec2lut.py <slug> --format both
```
```
python lospec2lut.py <slug> --format 3dl --bit-depth 10
```
```
python lospec2lut.py my-pal --hex "9bbc0f 8bac0f 306230 0f380f"
```
```
# perceptual distance
python lospec2lut.py <slug> --linear      
```
```
python lospec2lut.py <slug> --output ./luts/
```

## Examples
```
python lospec2lut.py resurrect-64 --output ./luts/
```
```
python lospec2lut.py lospec500 --size 33 --format both
```
```
python lospec2lut.py nintendo-gameboy-bgb --format 3dl --bit-depth 12
```