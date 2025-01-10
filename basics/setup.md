
# First-Time Setup

Setting up your environment correctly is crucial for efficient and high-quality video encoding. This guide ensures that you start with a clean slate and have all necessary tools configured.

---

## Getting a Clean Slate

If you’re on **Linux**, you can likely skip this step, as the system setup is typically clean. However, for **Windows users**, it is essential to remove old or conflicting installations of VapourSynth and Python before proceeding.

### Steps to Clean Up Old Installations

1. **Uninstall Python**:
   - On **Windows**:
     - Go to `Control Panel > Programs > Programs and Features`, locate Python, and uninstall all versions.
   - On **Linux/macOS**:
     ```bash
     sudo apt-get remove python3
     ```

2. **Uninstall VapourSynth**:
   - On **Windows**:
     - Navigate to `Control Panel > Programs > Programs and Features`, locate VapourSynth, and uninstall it.
     - Delete any VapourSynth-related entries in the system's `PATH` variable.
   - On **Linux/macOS**:
     ```bash
     sudo apt-get remove vapoursynth
     ```

3. **Remove Leftover Directories**:
   - Check and delete the following directories (on **Windows**):
     - `%APPDATA%/VapourSynth`
     - `%APPDATA%/Python`
     - `%LOCALAPPDATA%/Programs/VapourSynth`
     - `%LOCALAPPDATA%/Programs/Python`

4. **Clean Environment Variables**:
   - Open **System Properties** > **Environment Variables**.
   - Remove any entries related to old Python or VapourSynth installations from the `PATH` variable.

5. **Reboot Your System**:
   - After cleanup, restart your computer to ensure all changes take effect.

---

## Tools You’ll Need

Here’s a list of essential tools for video encoding:

1. **[Python](https://www.python.org/)** (v3.12 or higher): Required for running VapourSynth scripts.
2. **[VapourSynth](https://www.vapoursynth.com/)**: A frame server for filtering and processing video files.
3. **Encoders**:
   - **[x264](https://www.videolan.org/developers/x264.html)**: For H.264 encoding.
   - **[x265](https://x265.readthedocs.io/)**: For H.265 encoding.
4. **[FFmpeg](https://ffmpeg.org/)**: For demuxing, remuxing, and audio processing.
5. **[MKVToolNix](https://mkvtoolnix.download/)**: To merge audio, video, and subtitles.
6. **Other Utilities**:
   - **[LSMASH Works](https://github.com/HomeOfVapourSynthEvolution/L-SMASH-Works)**: For source loading.
   - **[D2V Witch](https://github.com/dubhater/D2VWitch)**: For indexing MPEG-2 videos.
   - **[mpv](https://mpv.io/)**: A media player to preview scripts.

---

## Installing the Tools

### Step 1: Install Python

1. **Download**: Obtain Python 3.12+ from [python.org](https://www.python.org/downloads/).
2. **Install**: During installation, select the option to add Python to the `PATH` variable.
3. **Verify Installation**:
   ```bash
   python --version
   pip --version
   ```
4. **Upgrade pip**:
   ```bash
   python -m pip install --upgrade pip
   ```

### Step 2: Install VapourSynth

1. **Download**: Get VapourSynth from its [GitHub page](https://github.com/vapoursynth/vapoursynth/releases).
2. **Install**:
   - On **Windows**: Run the installer and select all required components.
   - On **Linux/macOS**:
     ```bash
     sudo apt-get install vapoursynth
     ```
3. **Verify Installation**:
   ```bash
   vspipe --version
   ```

### Step 3: Install Encoders

#### x264
1. **Download**: Get the latest build from [VideoLAN](https://www.videolan.org/developers/x264.html).
2. **Install**: Place the binary in a folder added to your system’s `PATH`.

#### x265
1. **Download**: Obtain the encoder from [x265's repository](https://bitbucket.org/multicoreware/x265/downloads/).
2. **Install**: Extract the binary and update your `PATH`.

### Step 4: Install FFmpeg
1. **Download**: Visit [FFmpeg’s website](https://ffmpeg.org/download.html).
2. **Install**: Add the `bin` folder to your `PATH`.

### Step 5: Install MKVToolNix
1. **Download**: Get the installer from [MKVToolNix’s website](https://mkvtoolnix.download/).
2. **Verify Installation**:
   ```bash
   mkvmerge --version
   ```

---

## Verifying Your Setup

1. **Create a VapourSynth Script**:
   ```python
   import vapoursynth as vs
   core = vs.get_core()
   clip = core.ffms2.Source("example.mkv")
   clip.set_output()
   ```
2. **Run the Script**:
   ```bash
   vspipe --y4m script.vpy - | ffplay -
   ```
3. **Check Output**: If the video plays, your setup is complete!

---

With a clean slate and the tools installed, you’re ready to explore advanced filtering and encoding techniques.
