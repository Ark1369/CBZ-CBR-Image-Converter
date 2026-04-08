# CBZ/CBR Image Converter

A desktop GUI tool designed to optimize and convert images within comic book archives (`.cbz` and `.cbr`) into modern, highly-compressed formats like **WEBP**, **JXL**, and **AVIF**. It supports bulk processing, drag-and-drop, and intelligent auto-previewing to ensure your archives are actually saving space before modifying them.

---

## 🚀 Features

* **Multiple Formats:** Convert WEBP, AVIF, JXL, JPG, PNG images in CBZ/CBR to WEBP, JXL, or AVIF (Default 80 Quality).
    * To change default quality edit `quality_var = tk.StringVar(value="80")` to desired quality.
* **Preview feature** - Randomly samples 3 or more images from the target CBZ and converts them in memory to showcase what the size would look like (Useful to check before converting Large Volumes/Omnibus).
    * Under `def preview_estimate():`  change `res = estimate_archive_conversion(cbz, fmt, quality, 3, force_jxl_lossy, use_cli)` from 3 to desired image count.      
* **Smart Auto-Preview:** Automatically tests a sample of images inside large archives (>200MB). If the conversion won't save space, it skips the file to save time.
    * To change the default Auto Preview Size edit `auto_preview_var = tk.StringVar(value="200")` to the desired Size.
* **Native & Python Engines:** Choose between easy-to-use Python (Pillow) conversion or lightning-fast Native C++ executable engines.
* **Batch Processing:** Drag and drop files/folders or select an entire directory for bulk processing.
* **Large Image Splitting:** Automatically splits excessively tall images (like Webtoons) into seamless tiles to prevent format limits.
    * To change the default Max Dimension, edit `max_dim_var = tk.StringVar(value="10000")` to the desired dimension. WEBP limits are 16383 x 16383 Pixels.
* **Safe Conversions:** Creates `.bak` backups during processing and safely rolls back if the output file is suspiciously small, missing images, or larger than the original.
* **CBR to CBZ:** Easily repackage RAR-based `.cbr` files into universally supported `.cbz` (ZIP) files.

<img width="1202" height="931" alt="webp_converter_Hyx1bLk5NJ" src="https://github.com/user-attachments/assets/3e447637-c407-49ae-9ea6-ab03b623b8ba" />

*Image showing Tile Splitting, Auto Preview in action*

---

## 🛠️ Requirements & Installation

To launch the tool, Run the .exe provided in Release.

For the .py script version - Run the following command in the folder where you placed the script after fulfilling the requirements given below:
```bash
python webp_converter.py
```

### 1. Python Requirements (Base Setup)
You need **Python 3.x** installed. Install the required Python libraries by opening your terminal or command prompt and running:

Base Requirements
```bash
pip install pillow tkinterdnd2
```
CBR, AVIF, JXL Support [OPTIONAL]
```bash
pip install pillow tkinterdnd2 rarfile pillow-avif-plugin pillow-jxl-plugin
```

*Note on `.cbr` files:* If you want to repack files as `.cbr`, you must have WinRAR installed on your system. To avoid this entirely, simply check the **"Save CBR as CBZ"** option in the app to convert them to ZIP-based archives.

### 2. C++ Native CLI Engine Requirements (Recommended for WEBP Conversion/Lossless JXL Transcoding) [OPTIONAL]
For vastly improved speed and compression, you can use Native C++ tools. Download the respective standalone command-line binaries for the formats you want and place them in the **same folder as the script**:
* **WEBP:** Requires `cwebp.exe` (Download from Google's WebP site)
* **JXL:** Requires `cjxl.exe` (Download from the JPEG XL Github)
* **AVIF:** Requires `avifenc.exe` (Download from AOMedia Github)

---

## ⚡ Pillow (Python) vs. Native C++ Engine

For non-technical users, the most important choice is whether to use the default **Pillow Engine** or check the box for the **Native CLI Engine**.

### Pillow Engine (Default)
* **Pros:** Works immediately out of the box after running the `pip install` command. Converts everything in-memory without creating temporary image files on your drive.
* **Cons:** Slower for WEBP Conversion.

### Native C++ Engine (.exe tools)
* **Pros:** **Significantly faster and more efficient for WEBP.** Tools like `cwebp.exe` are written in C++ and optimized directly by their creators. `cjxl.exe` allows you to Losslessly Transcode JPEG files to JXL, gaining size saving without any Quality Loss.
* **Cons:** Requires you to manually download the `.exe` files and place them next to the script. Slower in AVIF/JXL Lossy Conversion Compared to Pillow.
* *Tip: If you are converting gigabytes of comics to WEBP, taking the 2 minutes to download the C++ files will save you hours of waiting.*

---

## Interface Options

* **Select Folder / Drag & Drop:** Click to select a directory containing `.cbz`/`.cbr` files, or drag and drop files straight into the app window.
* **Output Format:** Choose your target format (**WEBP**, **JXL**, or **AVIF**).
* **Quality (0-100):** Sets the image quality. **80** is the recommended default for a great balance between visual fidelity and file size reduction.
* **Max Dim (WEBP Only):** Sets a maximum pixel dimension for width or height (Default: `10000`). If an image exceeds this, it is automatically split into smaller tiles.
* **Auto-Preview (MB):** Automatically tests a small sample of images inside any archive larger than this size (Default: `200` MB). If the test reveals no space savings, the archive is skipped.
* **Preview Button:** Manually runs the size-saving estimation on your selected files without actually converting them.
* **Save CBR as CBZ:** Check this to repack `.cbr` (RAR) archives into `.cbz` (ZIP) archives after conversion. **Highly recommended** as ZIP is universally supported.
* **Use Native CLI Engine (.exe tools):** Tells the app to look for and use `cwebp.exe`, `cjxl.exe`, or `avifenc.exe` for faster conversion. If it can't find them, it safely warns you and falls back to Pillow.
* **Force JPEG to JXL Lossy (JXL + CLI Only):** By default, the JXL C++ engine converts JPEGs losslessly (keeping 100% original quality). Checking this box forces the engine to apply lossy compression for much smaller file sizes.
