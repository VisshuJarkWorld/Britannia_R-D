# Britannia Biscuit Vision System Research

## Objective

Build a computer vision pipeline capable of:

- Detecting biscuits from images/videos
- Extracting biscuit contours
- Measuring biscuit dimensions in pixels
- Converting pixel measurements into real-world dimensions (mm)
- Estimating thickness/dome height using depth methods
- Counting biscuits in real-time
- Supporting reject decision generation for defective/out-of-spec biscuits

---

# Initial Computer Vision Pipeline

```text
Input Image
    ↓
Preprocessing
    ↓
Grayscale Conversion
    ↓
Blurring / Noise Reduction
    ↓
Thresholding
    ↓
Contour Detection
    ↓
Shape Approximation
    ↓
2D Dimension Estimation
    ↓
Pixel → mm Conversion

---

# Current Progress

## Implemented

- Image preprocessing
- Grayscale conversion
- Gaussian blurring
- Thresholding
- Contour extraction
- Rectangle-based dimension estimation
- Polygon approximation
- Circular contour approximation
- 2D width/height estimation in pixels

---

# Depth Sensing Research

| Method | Features | Disadvantages | Suitable? |
|---|---|---|---|
| Stereo Vision | No extra hardware, side cameras act as stereo pair, cost effective, full surface depth map possible | Computationally heavier, sensitive to lighting, requires calibration | ✅ Yes |
| Laser Line Triangulation | Industry standard, highly accurate, fast at conveyor speed, works on low-texture surfaces | Extra laser hardware, calibration complexity, safety considerations | ✅ Yes |
| Time of Flight (ToF) | Fast depth acquisition, low-light capable, simple setup | Low precision for ±0.3mm accuracy, IR interference | ⚠️ Maybe |
| Structured Light (RealSense) | Easy integration, full 3D surface reconstruction | Sensitive to factory light/dust, less industrial grade | ❌ No |
| Photometric Stereo | No special hardware, good dome profiling | Requires multiple exposures and controlled lighting | ❌ No |
| Fringe Projection | Extremely accurate metrology | Too slow, vibration sensitive, overkill | ❌ No |
| Confocal / Chromatic Confocal | Micron-level precision | Extremely expensive and slow | ❌ No |
| Deflectometry | Excellent for reflective surfaces | Biscuits are semi-matte, setup complexity | ❌ No |
| X-Ray / Terahertz | Detects internal defects | Extremely expensive, not for dimensional measurement | ❌ No |

---

# Current Preferred Approaches

## Stereo Vision

- Easier prototyping
- Lower hardware complexity
- Full depth map possible
- Suitable for early experimentation

---

## Laser Line Triangulation

- Better industrial reliability
- Higher precision
- Better for conveyor-based height estimation
- Strong candidate for deployment phase

---

# System Requirements

Our system requires cameras capable of handling:

| Requirement | Why |
|---|---|
| Moving conveyor | Avoid motion distortion |
| Real-time measurement | Stable frame capture |
| Small dimensional tolerances | High image clarity |
| Factory lighting | Robust imaging |
| Multi-camera setup | Synchronization |
| Depth/thickness estimation | Geometric consistency |

---

# Global Shutter vs Rolling Shutter

| Feature | Global Shutter | Rolling Shutter |
|---|---|---|
| Capture Method | Entire frame simultaneously | Line-by-line sequential capture |
| Best For | Moving objects / industrial systems | Static scenes |
| Motion Distortion | Minimal | High |
| Shape Accuracy | Accurate | Can skew/stretch objects |
| Conveyor Usage | Excellent | Poor |
| Dimension Accuracy | Reliable | Error-prone |
| Multi-Camera Sync | Easier | Harder |
| Industrial Usage | Standard | Rare |
| Cost | Higher | Lower |

---

# Why Global Shutter is Necessary

In this biscuit inspection system:

- Conveyor is continuously moving
- Dimensions must remain accurate
- Multiple cameras must synchronize
- Reject decisions depend on precise measurements

With rolling shutter:

- biscuits may appear stretched
- tilted
- warped
- geometrically inconsistent

This directly affects:

- contour accuracy
- dimension estimation
- calibration stability
- depth estimation reliability

Therefore:

> Global shutter cameras are strongly preferred.
