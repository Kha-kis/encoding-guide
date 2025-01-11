
# Temporal Filtering

Temporal filtering smooths motion and reduces noise by analyzing frames over time, leveraging patterns across multiple frames to improve visual consistency and reduce artifacts. It is particularly useful for addressing flickering, noise, and motion artifacts in video sources.

This guide will explore temporal filtering in detail, including common use cases, techniques, and example scripts.

---

## What is Temporal Filtering?

Temporal filtering operates across frames to:
1. **Reduce Flickering**: Stabilizes inconsistent lighting or noise.
2. **Preserve Motion**: Ensures objects in motion retain their natural flow.
3. **Enhance Compression**: Smooths static areas to save bits for dynamic scenes.
4. **Improve Visual Quality**: Reduces temporal noise while maintaining texture and detail.

---

## Types of Temporal Artifacts

### 1. **Temporal Noise**
- **Description**: Random, fluctuating noise that changes frame-to-frame.
- **Sources**: Low-light recordings, analog sources.
- **Impact**: Distracting grain that worsens compression.

### 2. **Motion Artifacts**
- **Description**: Ghosting or smearing in moving objects.
- **Sources**: Low frame rate or poorly implemented motion interpolation.
- **Impact**: Motion appears unnatural or blurry.

### 3. **Flickering**
- **Description**: Sudden changes in brightness or color between frames.
- **Sources**: Poor lighting, unstable camera settings.
- **Impact**: Distracts from the content.

---

## Techniques for Temporal Filtering

### 1. **Basic Temporal Smoothing**

Temporal smoothing reduces noise and flickering by averaging pixel values across frames.

- **Tool**: `TemporalSoften`
- **Example**:
  ```python
  import vapoursynth as vs
  core = vs.core
  clip = core.temporal.TemporalSoften(clip, radius=2, luma_threshold=5, chroma_threshold=10)
  ```
  - `radius`: Number of frames to include for analysis.
  - `luma_threshold`: Intensity threshold for luma plane adjustments.
  - `chroma_threshold`: Intensity threshold for chroma plane adjustments.

---

### 2. **Motion-Compensated Temporal Filtering**

Motion-compensated filtering identifies and adjusts motion across frames, preserving sharpness and detail in moving objects.

- **Tool**: `MCTemporalDenoise`
- **Example**:
  ```python
  clip = core.mctemporal.MCTemporalDenoise(clip, sigma=2)
  ```
  - `sigma`: Noise reduction strength.

- **Advanced Settings**:
  ```python
  clip = core.mctemporal.MCTemporalDenoise(clip, sigma=3, block_size=16, overlap=8)
  ```
  - `block_size`: Size of the motion estimation blocks.
  - `overlap`: Overlap percentage between blocks to improve accuracy.

---

### 3. **Temporal Degrain**

Temporal degrain targets grain and noise specifically, useful for film sources or older analog content.

- **Tool**: `SMDegrain`
- **Example**:
  ```python
  clip = core.smd.SMD(clip, tr=3, thSAD=300)
  ```
  - `tr`: Temporal radius (number of frames to analyze).
  - `thSAD`: Threshold for noise detection.

---

### 4. **Flicker Removal**

Flicker removal stabilizes lighting inconsistencies or color shifts between frames.

- **Tool**: `Deflicker`
- **Example**:
  ```python
  clip = core.deflicker.Deflicker(clip, mode="light")
  ```
  - `mode`: Choose from "light" (general flicker) or "strong" (severe flicker).

---

## Combining Temporal and Spatial Filtering

Combining temporal and spatial filters ensures balanced results, reducing noise and flickering across frames while maintaining intra-frame details.

### Workflow:
1. Apply **temporal filtering** first to address frame-to-frame inconsistencies.
2. Follow with **spatial filtering** to refine individual frames.

- **Example**:
  ```python
  # Temporal Filtering
  clip = core.mctemporal.MCTemporalDenoise(clip, sigma=2)

  # Spatial Filtering
  clip = core.knlm.KNLMeansCL(clip, d=1, h=1.2)
  ```

---

## Advanced Techniques

### 1. **Masking for Dynamic Filtering**

Use masks to apply temporal filtering only to specific areas, such as noisy backgrounds.

- **Example**:
  ```python
  mask = core.std.Prewitt(clip)
  filtered = core.mctemporal.MCTemporalDenoise(clip, sigma=2)
  clip = core.std.MaskedMerge(clip, filtered, mask)
  ```

---

### 2. **Adjusting Temporal Radius**

Higher temporal radius improves smoothing but increases computational load.

- **Low Radius (tr=2)**:
  - Suitable for low-motion or static content.
- **High Radius (tr=5)**:
  - Ideal for high-motion or grainy content.

---

### 3. **Frame Interpolation**

Frame interpolation generates new frames for smoother playback or frame rate conversion.

- **Tool**: `SVP`
- **Example**:
  ```python
  clip = core.svp.SmoothMotion(clip, preset="fast")
  ```
  - `preset`: Choose from "fast", "medium", or "quality".

---

## Example Comprehensive Script

Here’s a complete example for addressing temporal artifacts:
```python
import vapoursynth as vs
core = vs.core

# Load source
clip = core.ffms2.Source("noisy_source.mkv")

# Step 1: Temporal Filtering (Noise and Grain)
clip = core.smd.SMD(clip, tr=3, thSAD=400)

# Step 2: Flicker Removal
clip = core.deflicker.Deflicker(clip, mode="light")

# Step 3: Motion-Compensated Filtering
clip = core.mctemporal.MCTemporalDenoise(clip, sigma=2)

# Step 4: Spatial Filtering (Refinement)
clip = core.knlm.KNLMeansCL(clip, d=1, h=1.2)

# Output
clip.set_output()
```

---

## Best Practices for Temporal Filtering

1. **Start Small**: Test settings on a short segment of video before applying them to the full source.
2. **Avoid Over-Smoothing**: Over-aggressive temporal filtering can introduce ghosting or smearing.
3. **Combine Filters**: Use a combination of temporal and spatial techniques for the best results.
4. **Adjust Parameters**: Tailor settings to the specific needs of your source material.

---

## Conclusion

Temporal filtering is a powerful tool for improving video quality, particularly for noisy, grainy, or flickering sources. By understanding the techniques and tools outlined in this guide, you can achieve cleaner, more professional results tailored to your content’s needs. Experiment with different approaches and fine-tune parameters to find the perfect balance between noise reduction and detail preservation.