# Understanding Encoding Basics

Encoding is the process of converting video or audio from one format to another, often balancing file size and quality. This section introduces the fundamental concepts and terms you'll encounter when working with encoding.

---

## What is Encoding?

Encoding is the act of compressing video or audio into a format that is more storage-efficient or compatible with playback devices. The goal is to reduce file size while maintaining as much quality as possible. 

### Common Reasons for Encoding:
- Reducing storage requirements.
- Creating media compatible with specific devices or platforms.
- Enhancing the quality of poorly compressed sources.
- Adding or editing metadata, subtitles, or audio tracks.

---

## Codecs and Formats

### **What is a Codec?**
A **codec** (compressor-decompressor) is a tool that encodes and decodes video and audio. It determines how data is compressed and decompressed.

#### Popular Codecs:
- **H.264 (x264)**: Widely used for its balance of quality and compression efficiency.
- **H.265 (x265)**: Offers better compression than H.264, especially for 4K and HDR content.
- **AV1**: A next-generation codec with even greater efficiency, though slower to encode.
- **AAC**: A common codec for audio, offering high-quality compression.

---

### **What is a Container?**
A **container** is a file format that holds video, audio, subtitles, and metadata. It acts as a "wrapper" for these components.

#### Common Containers:
- **MKV (Matroska)**: Supports nearly any codec and is highly versatile.
- **MP4**: Widely compatible with most devices and platforms.
- **AVI**: An older format with limited codec support.

---

## Key Concepts in Encoding

### **Resolution**
Resolution refers to the number of pixels in a video, typically represented as **width x height**. Common resolutions include:
- **1080p**: 1920x1080 pixels.
- **2160p (4K)**: 3840x2160 pixels.

Higher resolutions offer more detail but require more storage and processing power.

---

### **Bitrate**
Bitrate measures the amount of data used per second of video or audio, typically in **kbps (kilobits per second)** or **Mbps (megabits per second)**. It directly impacts quality:
- **Higher Bitrate** = Better Quality = Larger File Size.
- **Lower Bitrate** = Worse Quality = Smaller File Size.

#### Types of Bitrate:
- **CBR (Constant Bitrate)**: Maintains the same bitrate throughout the video.
- **VBR (Variable Bitrate)**: Adjusts the bitrate based on scene complexity, optimizing file size.

---

### **Frame Rate**
Frame rate (frames per second or **fps**) refers to the number of images displayed per second. Common frame rates:
- **23.976 fps**: Standard for most films.
- **30 fps**: Common for TV and streaming.
- **60 fps**: Smooth playback for sports or gaming.

---

### **Compression**
Compression reduces file size by removing redundant or unnecessary data. It comes in two forms:
- **Lossy Compression**: Removes data to reduce file size (e.g., H.264).
- **Lossless Compression**: Preserves all original data, resulting in larger files (e.g., FLAC for audio).

---

### **Color Depth**
Color depth determines the range of colors a video can display:
- **8-bit**: Common for standard video.
- **10-bit**: Allows for more colors and smoother gradients, essential for HDR content.

---

### **HDR (High Dynamic Range)**
HDR increases the range of brightness and colors in a video, enhancing visual fidelity. Types of HDR include:
- **HDR10**: The most common HDR format.
- **Dolby Vision**: A premium format with dynamic metadata.
- **HDR10+**: Similar to Dolby Vision but open source.

---

## Encoding Profiles and Levels

### **Profiles**
Profiles define the capabilities of a codec, such as supported resolutions and bitrates. For example:
- **Baseline**: For low-complexity encoding (e.g., video calls).
- **Main**: Balanced for most use cases.
- **High**: For higher quality and resolutions.

### **Levels**
Levels set limits on resolution, bitrate, and frame rate. For example:
- **Level 4.1**: Supports 1080p at 30 fps.
- **Level 5.1**: Supports 4K at 30 fps.

---

## Encoding Pipeline Overview

1. **Source Selection**:
   - Choose high-quality sources for better results (e.g., Blu-ray, UHD discs).
2. **Filtering**:
   - Apply filters to clean, resize, or enhance the video.
3. **Encoding**:
   - Compress the video using an encoder like x264 or x265.
4. **Muxing**:
   - Combine the encoded video with audio, subtitles, and metadata into a container (e.g., MKV).
5. **Quality Check**:
   - Verify the final file for synchronization and visual quality.

---

## Conclusion

Understanding these basics is essential for producing high-quality encodes. In the next section, we’ll dive deeper into filtering techniques and how to prepare your source for encoding.
