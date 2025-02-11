# Basics of Encoding Settings: x264

Encoding video is the process of compressing raw footage into a manageable size while maintaining as much quality as possible. With x264, you can achieve excellent results by tweaking its advanced settings to match your source and goals.

---

## What is Encoding?

Encoding reduces the size of video files while ensuring playback compatibility across devices. It’s essential for:
- **Storing media efficiently**: Saves disk space while retaining quality.
- **Streaming**: Ensures smooth playback over the internet.
- **Compatibility**: Makes videos playable on a wide range of devices.

---

## Understanding x264

x264 is an encoder for the H.264 (AVC) format. It balances quality, compression, and playback compatibility. With x264, you can adjust numerous settings to fine-tune your output. These settings fall into two categories:
1. **General Settings**: Rarely changed between encodes.
2. **Source-Specific Settings**: Adjusted based on the source material.

---

## General Settings

These settings can usually stay consistent across encodes.

### 1. **Preset**
The preset determines how fast or slow the encoder works. Slower presets take longer but provide better compression and quality.
- **Recommended**: `--preset placebo`
- Why? Although slower, this preset provides the most compression efficiency.

---

### 2. **Level**
The level limits the encoder’s features to ensure compatibility with playback devices.
- **Recommended**: `--level 41` (suitable for most hardware)
- Omit if device compatibility isn’t a concern.

---

### 3. **Motion Estimation (ME)**
Motion estimation helps the encoder predict and compress motion between frames.
- **Recommended**: `--me umh`
- Why? It's a balance between encoding speed and accuracy.

---

### 4. **Ratecontrol Lookahead**
This setting determines how far ahead the encoder can look when distributing bits.
- **Recommended**: `--rc-lookahead 250`
- Why? Higher values improve quality but increase memory usage. Lower to `60` on systems with limited RAM.

---

## Source-Specific Settings

These settings depend on the type of video you’re encoding.

### 1. **Profile**
Defines the encoder’s features and compatibility with playback devices.
- **Recommended**:
  - `--profile high` (for 8-bit 4:2:0 content)
  - `--profile high444` (for 10-bit 4:4:4 or lossless content)

---

### 2. **Ratecontrol**
Controls how bits are distributed across frames.

#### CRF (Constant Rate Factor)
- **Recommended**: `--crf 16.9`
- Why? Ensures consistent quality across frames. Lower values (e.g., `15.0`) increase quality but produce larger files.

#### Two-Pass Encoding
- Use two-pass for targeting specific bitrates:
  1. First pass:
     ```bash
     vspipe -c y4m script.vpy - | x264 --demuxer y4m --preset placebo --pass 1 --bitrate 8500 -o /dev/null -
     ```
  2. Second pass:
     ```bash
     vspipe -c y4m script.vpy - | x264 --demuxer y4m --preset placebo --pass 2 --bitrate 8500 -o out.h264 -
     ```

---

### 3. **Deblock**
Smooths edges between blocks, reducing artifacts.
- **Recommended**: `--deblock -2:-1`
- Why? Prevents blurring while avoiding excessive blockiness. For animation, consider `--deblock 0:0`.

---

### 4. **Chroma Quantizer Offset**
Adjusts how bits are allocated to chroma (color) channels.
- **Recommended**: `--chroma-qp-offset -1`
- Why? Helps prevent chroma distortion.

---

### 5. **Quantizer Curve Compression**
Adjusts bit distribution between complex and simple scenes.
- **Recommended**: `--qcomp 0.70` (when using macroblock tree)
- Why? Ensures high-motion scenes get enough bits without starving static ones.

---

### 6. **Macroblock Tree**
Improves bit distribution within complex scenes.
- **Recommended**: `--mbtree`
- Why? Reduces file size while maintaining quality in static areas.

---

### 7. **Adaptive Quantization**
Distributes bits within frames and across adjacent frames.
- **Recommended**: `--aq-mode 3 --aq-strength 0.80`
- Why? Prevents banding and preserves fine details like grain or dither.

---

### 8. **Psycho-Visual Rate Distortion Optimization (Psy-RDO)**
Improves perceptual quality by slightly distorting frames for sharpness.
- **Recommended**: `--psy-rd 1.00:0`
- Why? Enhances sharpness without introducing ringing.

---

## Full Recommended Command

Here’s a full command combining the settings above:
```bash
vspipe -c y4m script.vpy - | x264 --demuxer y4m --preset placebo --level 41 --crf 16.9 --me umh --rc-lookahead 250 --profile high --deblock -2:-1 --chroma-qp-offset -1 --qcomp 0.70 --aq-mode 3 --aq-strength 0.80 --psy-rd 1.00:0 -o output.h264 -
```

---

## Conclusion

The recommended x264 settings provide a starting point for high-quality encodes. Adjust these based on your source material and playback requirements to achieve the best results.

