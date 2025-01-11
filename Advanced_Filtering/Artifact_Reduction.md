
# Artifact Reduction

Artifact reduction is a vital process for improving the quality of video sources, particularly those that have been heavily compressed or poorly encoded. Artifacts manifest as visual distortions and can significantly detract from the viewing experience. This guide explores the most common artifacts in depth, including practical solutions tailored to address them.

---

## Understanding Common Artifacts

### 1. **Blocking**
- **Description**: Appears as square block patterns, often in flat areas of the image, caused by low bitrate or aggressive compression.
- **Typical Sources**: Low-bitrate streams, older DVDs, over-compressed Blu-rays.
- **Key Impact**: Makes flat backgrounds look unnatural and distracts from the overall image quality.

### 2. **Banding**
- **Description**: Visible, unnatural lines or steps in gradients, especially in skies, shadows, or transitions between colors.
- **Typical Sources**: Poor color depth, over-compression.
- **Key Impact**: Loss of smooth transitions; highly noticeable in HDR content.

### 3. **Ringing**
- **Description**: Halo-like distortions around edges, often from aggressive sharpening or low-pass filtering.
- **Typical Sources**: Upscaling, lossy compression.
- **Key Impact**: Amplifies the artificial look of edges, distracting the viewer.

### 4. **Mosquito Noise**
- **Description**: Tiny, grainy distortions that “swarm” around sharp edges.
- **Typical Sources**: Low-bitrate video codecs, over-compression.
- **Key Impact**: Reduces edge clarity, creating a buzzing effect.

### 5. **Debris**
- **Description**: Random specks or remnants from poor analog-to-digital conversion.
- **Typical Sources**: VHS, LaserDisc, older film transfers.
- **Key Impact**: Visual clutter that detracts from the main content.

---

## Reducing Specific Artifacts

### 1. **Blocking**
Blocking is common in heavily compressed content and can be particularly challenging to address.

#### **Basic Deblocking**
- **Tool**: `Deblock`
- **Example:**
  ```python
  clip = core.deblock.Deblock(clip, quant=25)
  ```
  - `quant`: Adjust to control the intensity. Start at 20 for mild blocking and increase for severe cases.

#### **Advanced Deblocking**
- Combine deblocking with light spatial filtering for smoother results.
  ```python
  clip = core.deblock.Deblock(clip, quant=18)
  clip = core.knlm.KNLMeansCL(clip, d=1, h=0.8)
  ```

#### **Multi-Pass Deblocking**
- Process the clip multiple times with varying `quant` values to refine the results.
  ```python
  clip = core.deblock.Deblock(clip, quant=22)
  clip = core.deblock.Deblock(clip, quant=16)
  ```

---

### 2. **Banding**
Banding occurs in gradients and smooth transitions. It’s especially noticeable in skies, shadows, and HDR content.

#### **Debanding with f3kdb**
- **Tool**: `f3kdb`
- **Example:**
  ```python
  clip = core.f3kdb.Deband(clip, range=18, y=64, cb=64, cr=64, grainy=16, grainc=16)
  ```
  - `range`: Increase for stronger debanding.
  - `grainy`, `grainc`: Adds dithering to hide residual bands.

#### **Gradient Masking**
- Apply a mask to target specific gradient areas for debanding.
  ```python
  mask = core.std.Binarize(clip.std.PlaneStats(), threshold=150)
  clip = core.std.MaskedMerge(clip, debanded_clip, mask)
  ```

#### **HDR-Specific Banding**
- HDR sources often require stronger settings due to higher color depth:
  ```python
  clip = core.f3kdb.Deband(clip, range=20, y=80, cb=80, cr=80, grainy=20, grainc=20)
  ```

---

### 3. **Ringing**
Ringing can be subtle but becomes highly distracting when sharpening filters are over-applied.

#### **Basic Deringing**
- **Tool**: `dering`
- **Example:**
  ```python
  clip = core.dering.Dering(clip, strength=30)
  ```
  - `strength`: Adjust based on the severity of the ringing.

#### **Edge-Aware Filtering**
- Use an edge mask to selectively reduce ringing around sharp transitions.
  ```python
  edges = core.std.Prewitt(clip)
  filtered = core.dering.Dering(clip, strength=25)
  clip = core.std.MaskedMerge(clip, filtered, edges)
  ```

---

### 4. **Mosquito Noise**
Mosquito noise clusters around edges and often requires a combination of temporal and spatial filtering.

#### **Targeted Mosquito Noise Reduction**
- **Tool**: `HQDN3D`
- **Example:**
  ```python
  clip = core.hqdn3d.HQDenoise3D(clip, luma_spatial=4.0, luma_temporal=6.0)
  ```

#### **Temporal-Specific Noise Reduction**
- Combine temporal smoothing with edge enhancement to retain sharpness:
  ```python
  clip = core.knlm.KNLMeansCL(clip, d=2, h=1.0)
  ```

---

### 5. **General Artifacts**
For sources with a mix of artifacts, use versatile tools like `SMDegrain`:

#### **Multi-Artifact Reduction**
- **Tool**: `SMDegrain`
- **Example:**
  ```python
  clip = core.smd.SMD(clip, tr=3, thSAD=300)
  ```
  - `tr`: Temporal radius for analyzing frames.
  - `thSAD`: Adjust to control the sensitivity.

#### **Pre-Processing for General Artifacts**
- Pre-process with deblocking, then apply SMDegrain:
  ```python
  clip = core.deblock.Deblock(clip, quant=16)
  clip = core.smd.SMD(clip, tr=2, thSAD=200)
  ```

---

## Example Comprehensive Workflow

Here’s a full script addressing multiple artifacts:
```python
import vapoursynth as vs
core = vs.core

# Load source
clip = core.ffms2.Source("artifacted_source.mkv")

# Step 1: Deblocking
clip = core.deblock.Deblock(clip, quant=20)

# Step 2: Debanding
clip = core.f3kdb.Deband(clip, range=18, y=64, cb=64, cr=64, grainy=16, grainc=16)

# Step 3: Deringing
clip = core.dering.Dering(clip, strength=30)

# Step 4: Mosquito Noise Reduction
clip = core.hqdn3d.HQDenoise3D(clip, luma_spatial=4.0, luma_temporal=6.0)

# Output
clip.set_output()
```

---

## Conclusion

Artifact reduction requires a keen eye and patience. Each type of artifact demands specific techniques, and combining them thoughtfully will yield clean, high-quality results. Always preview your changes and test on small clips before committing to a full encode.