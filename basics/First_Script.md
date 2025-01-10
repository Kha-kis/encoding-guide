
# Baby's First Script

Congratulations on setting up your encoding environment! Now it’s time to write and run your first VapourSynth script. This will involve loading a video source, applying basic filters, and preparing the file for encoding.

---

## What is VapourSynth?

VapourSynth is a scripting-based video processing tool. It allows you to load, filter, and manipulate video files before encoding. VapourSynth uses Python for scripting, so familiarity with basic Python syntax will help.

---

## Writing Your First Script

### Step 1: Create a New Script File

1. Open a text editor (e.g., Notepad++, Visual Studio Code, or Sublime Text).
2. Save a new file with the `.vpy` extension, such as `first_script.vpy`.

---

### Step 2: Load Your Video Source

To load a video source, use one of VapourSynth's core plugins, such as `ffms2` or `LSMASHSource`. Here’s a basic example:

```python
import vapoursynth as vs
core = vs.core

# Load video source
clip = core.ffms2.Source("example.mkv")

# Output the video
clip.set_output()
```

---

### Step 3: Apply Basic Filters

Filters enhance the video by addressing artifacts or resizing the resolution. For example:

#### Crop and Resize:
```python
# Crop 10 pixels from each side and resize to 1280x720
clip = core.crop(clip, left=10, right=10, top=10, bottom=10)
clip = core.resize.Bilinear(clip, width=1280, height=720)
```

#### Deband:
```python
# Apply debanding to smooth gradients
clip = core.f3kdb.Deband(clip, range=15, y=64, cb=64, cr=64)
```

#### Denoise:
```python
# Apply light denoising to remove noise
clip = core.knlm.KNLMeansCL(clip, d=1, a=2, h=1.2)
```

---

### Step 4: Save and Preview Your Script

1. Save the script file.
2. Preview it using a media player like `mpv` or by piping the script to `ffplay`:
   ```bash
   vspipe --y4m first_script.vpy - | ffplay -
   ```

---

## Understanding the Script

Here’s a complete example of a basic VapourSynth script:
```python
import vapoursynth as vs
core = vs.core

# Load the video
clip = core.ffms2.Source("example.mkv")

# Apply basic filters
clip = core.crop(clip, left=10, right=10, top=10, bottom=10)  # Crop edges
clip = core.resize.Bilinear(clip, width=1280, height=720)     # Resize
clip = core.f3kdb.Deband(clip, range=15, y=64, cb=64, cr=64)  # Deband

# Output the video
clip.set_output()
```

---

## Running the Script

1. **Preview the Script**:
   Run the following command to preview your script:
   ```bash
   vspipe --y4m first_script.vpy - | ffplay -
   ```

2. **Encode the Video**:
   To encode the video, pipe the output into an encoder like `x264` or `x265`:
   ```bash
   vspipe first_script.vpy - | x264 --preset slow --crf 18 -o output.mkv -
   ```

---

## Troubleshooting Tips

- **Error Loading Plugins**:
  Ensure that VapourSynth plugins like `ffms2` are installed correctly.
- **Video Not Playing**:
  Double-check the file path and format of your source file.
- **Filters Not Working**:
  Verify that the required filters are installed and correctly referenced in the script.

---

## Conclusion

You’ve written and executed your first VapourSynth script! This foundational knowledge prepares you for more advanced filtering and encoding workflows. In the next section, we’ll explore filtering techniques in depth.
