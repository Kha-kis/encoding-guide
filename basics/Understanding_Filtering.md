
# Understanding Filtering

Filtering is like cleaning and improving your video before you save it. This process helps remove problems in the video, make it look better, and prepare it for encoding.

---

## What is Filtering?

When you apply filters to a video, you’re fixing issues or making it look better. Think of filters as tools in a toolbox:
- Some clean the video by removing noise or artifacts.
- Others adjust its size or improve its sharpness.

---

## Why Do We Need Filters?

Filters are helpful because they:
- Remove unwanted things like noise or color bands.
- Make videos look clearer and more professional.
- Prepare the video for smaller file sizes without losing quality.

---

## Types of Filters (Beginner-Friendly)

### 1. **Denoising**
- **What It Does**: Removes "noise" or grain from the video to make it look smoother.
- **When to Use**: If your video looks rough or has tiny random dots all over.
- **Simple Example**:
  ```python
  clip = core.knlm.KNLMeansCL(clip, d=1, a=2, h=1.2)  # Basic denoising
  ```

---

### 2. **Debanding**
- **What It Does**: Fixes color bands that make gradients look like steps instead of smooth transitions.
- **When to Use**: If you see stripes in the sky or gradients in your video.
- **Simple Example**:
  ```python
  clip = core.f3kdb.Deband(clip, range=15, y=64, cb=64, cr=64)
  ```

---

### 3. **Resizing**
- **What It Does**: Changes the video’s size (e.g., shrinking 4K to 1080p).
- **When to Use**: If you need a smaller resolution to save space or fit your screen.
- **Simple Example**:
  ```python
  clip = core.resize.Bilinear(clip, width=1280, height=720)
  ```

---

### 4. **Cropping**
- **What It Does**: Removes unwanted edges or black bars from the video.
- **When to Use**: If your video has black borders you want to cut out.
- **Simple Example**:
  ```python
  clip = core.std.CropRel(clip, left=10, right=10, top=10, bottom=10)
  ```

---

### 5. **Sharpening**
- **What It Does**: Makes details pop and enhances edges.
- **When to Use**: If your video looks blurry or lacks sharpness.
- **Simple Example**:
  ```python
  clip = core.std.Sharpen(clip, amount=0.5)
  ```

---

### 6. **Graining**
- **What It Does**: Adds grain (small dots) to make the video look more natural or cinematic.
- **When to Use**: If the video looks too smooth or flat.
- **Simple Example**:
  ```python
  clip = core.grain.Add(clip, var=5.0)
  ```

---

## Tips for Beginners

1. **Start Simple**:
   - Use one or two filters to see how they change the video.
   - For example, try just resizing and denoising first.

2. **Watch Your Video**:
   - After applying filters, preview the video to check the changes.
   - Use this command to preview:
     ```bash
     vspipe --y4m script.vpy - | ffplay -
     ```

3. **Avoid Overdoing It**:
   - Too much filtering can make the video look unnatural.
   - If you’re unsure, use mild settings for each filter.

---

## Why Filtering Order Matters

The order in which you apply filters is important. Here’s a beginner-friendly order to follow:
1. **Fix issues first** (e.g., remove noise or banding).
2. **Resize or crop** the video.
3. **Enhance details** with sharpening or graining.

---

## Conclusion

Filters are tools to make your video look better. Start with simple filters, preview your results, and don’t overcomplicate things. As you practice, you’ll understand how each filter works and when to use them.
