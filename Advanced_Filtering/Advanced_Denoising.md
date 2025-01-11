
# Advanced Denoising Techniques

Denoising is the process of removing unwanted noise or grain from video sources, improving clarity and compression efficiency. It’s especially crucial for older or low-quality sources, as excessive noise can lead to larger file sizes and visible artifacts during playback.

This guide explores the intricacies of temporal and spatial denoising, provides detailed examples, and offers insights into combining techniques for optimal results.

---

## Why Denoise?

### 1. **Improve Visual Quality**
Noise reduction enhances the perceived clarity of videos, especially in dark or complex scenes.

### 2. **Optimize Compression**
Noise takes up unnecessary bits during encoding. Denoising helps focus bits on important details, reducing file size.

### 3. **Prepare for Other Filters**
Many advanced filters (e.g., sharpening or upscaling) perform better when noise is reduced.

---

## Types of Noise

### 1. **Random Noise**
Common in older video sources or low-light recordings, appearing as fine grain or static.

### 2. **Compression Artifacts**
Blocky patterns or banding caused by poor compression techniques.

### 3. **Temporal Noise**
Fluctuating noise that changes from frame to frame, common in analog sources.

---

## Temporal Denoising

Temporal denoising analyzes multiple frames to detect and reduce noise over time. This approach preserves motion and details while minimizing artifacts.

### Advantages:
- Excellent for reducing flickering or moving noise.
- Preserves static backgrounds and consistent patterns.

### Techniques:

#### **BM3D (Block-Matching and 3D Filtering)**
BM3D is a state-of-the-art algorithm that reduces noise by grouping similar blocks across frames.

- **Example:**
  ```python
  import vapoursynth as vs
  core = vs.core
  clip = core.bm3d.Basic(clip, sigma=3)  # Adjust sigma for noise strength
  ```

#### **MCTemporalDenoise**
MCTemporalDenoise uses motion compensation to smooth noise while retaining motion fidelity.

- **Example:**
  ```python
  clip = core.mctemporal.MCTemporalDenoise(clip, sigma=2)
  ```

#### **SMDegrain**
A powerful denoiser based on motion vectors, suitable for both mild and aggressive denoising.

- **Example:**
  ```python
  clip = core.smd.SMD(clip, tr=3, thSAD=300)  # Adjust thSAD for noise threshold
  ```

---

## Spatial Denoising

Spatial denoising works within each frame, reducing grain and artifacts on a per-pixel basis.

### Advantages:
- Excellent for removing static noise or grain.
- Useful for still scenes or areas with little motion.

### Techniques:

#### **KNLMeansCL**
A fast, GPU-accelerated denoiser ideal for medium-strength noise reduction.

- **Example:**
  ```python
  clip = core.knlm.KNLMeansCL(clip, d=2, a=2, h=1.2)
  ```

#### **DFTTest**
Performs frequency-domain filtering to reduce noise without blurring details.

- **Example:**
  ```python
  clip = core.dfttest.DFTTest(clip, sigma=16)
  ```

#### **HQDN3D**
A simple but effective denoiser for light to moderate noise.

- **Example:**
  ```python
  clip = core.hqdn3d.HQDenoise3D(clip, luma_spatial=4.0, luma_temporal=6.0)
  ```

---

## Combining Temporal and Spatial Denoising

For the best results, combine both temporal and spatial denoising. This approach reduces noise across frames while ensuring that individual frames remain clean and detailed.

### Example Workflow:
```python
# Temporal denoising with SMDegrain
clip = core.smd.SMD(clip, tr=3, thSAD=300)

# Spatial denoising with KNLMeansCL
clip = core.knlm.KNLMeansCL(clip, d=2, a=2, h=1.2)
```

### Order Matters:
1. Start with temporal denoising to reduce inter-frame noise.
2. Apply spatial denoising to clean up remaining artifacts.

---

## Best Practices for Denoising

1. **Test on a Sample Clip**: Use a short segment to preview the effects before applying filters to the entire video.
2. **Avoid Over-Denoising**: Over-aggressive denoising can create unnatural smoothness and remove desired details like film grain.
3. **Adjust Parameters**: Fine-tune sigma, strength, and thresholds based on the source material.
4. **Preserve Artistic Intent**: For film sources, retain some grain to maintain the original look.

---

## Advanced Tips

1. **Use Zoning**: Apply stronger denoising to noisy scenes and lighter denoising to clean areas.
   ```python
   clip = core.knlm.KNLMeansCL(clip, d=2, a=2, h=1.5)
   clip = core.std.FrameEval(clip, lambda n, f: strong_clip if n in noisy_frames else clip)
   ```

2. **Pre-Process the Source**: For severely degraded sources, combine denoising with artifact reduction (e.g., deblocking or debanding).

3. **Temporal Radius**: Increase the temporal radius (e.g., `tr` in SMDegrain) for smoother results on static scenes.

4. **Analyze the Output**: Use tools like VapourSynth's `Preview` or test encodes to verify the effectiveness of your settings.

---

## Example Script

Here’s a complete script combining temporal and spatial denoising for a noisy source:
```python
import vapoursynth as vs
core = vs.core

# Load source
clip = core.ffms2.Source("noisy_source.mkv")

# Temporal denoising
clip = core.smd.SMD(clip, tr=3, thSAD=400)

# Spatial denoising
clip = core.knlm.KNLMeansCL(clip, d=2, a=2, h=1.2)

# Output
clip.set_output()
```

---

## Conclusion

Denoising is both an art and a science. By understanding and applying the techniques outlined in this guide, you can achieve clean, visually appealing videos while preserving essential details. Experiment with different methods and settings to tailor the results to your specific needs.