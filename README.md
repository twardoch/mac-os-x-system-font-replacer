# MacOSXSystemFontReplacer
Version 1.1

With `MacOSXSystemFontReplacer`, you can customize the user interface fonts on your Mac OS X 10.10 Yosemite (and potentially newer versions) by replacing them with fonts of your choice. This tool offers a way to personalize your macOS experience, improve readability, or use specific fonts for aesthetic or accessibility reasons.

![Example: Work Sans](example-work-sans.png?raw=true "Example: Work Sans used as a replacement UI font")
*This example shows the default UI system fonts of Mac OS X 10.10 replaced with the open-source Work Sans font family, using the `-s 105` option to visually increase the font sizes by 5%.*

## Overview

### What This Tool Does
`MacOSXSystemFontReplacer` is a Python script that allows you to replace the standard system UI fonts in macOS (like those used in menus, window titles, and Finder) with almost any TrueType (`.ttf`) or OpenType (`.otf`) font. It does this in a "safe" way:
*   It does **not** modify your original system fonts.
*   Instead, it creates patched versions of *your chosen fonts* and installs them into `/Library/Fonts/`.
*   You can easily uninstall the custom fonts and revert to the system defaults using the tool itself.

### Who Is It For?
This tool is for macOS users who:
*   Want to personalize the look and feel of their operating system.
*   Find the default system fonts difficult to read and wish to use a more accessible alternative.
*   Are designers or developers who want to test different UI aesthetics or ensure their work looks good with various fonts.
*   Simply prefer a different typeface for their daily interactions with their Mac.

### Why Is It Useful?
*   **Personalization:** Tailor your Mac's appearance to your liking.
*   **Readability & Accessibility:** Choose fonts that are easier for you to read, potentially larger or with different characteristics than the default.
*   **Aesthetics:** Experiment with different typographic styles for your OS interface.
*   **Safe & Reversible:** Try out new fonts without risking damage to your system files, and easily revert back.

## Installation

1.  **Prerequisites:**
    *   **macOS:** Originally designed for Mac OS X 10.10 Yosemite. It may work on newer versions, especially if you specify the correct system font file using the `-t` option.
    *   **Python:** The script was developed with Python 2.7. It relies on `fontTools`. (Note: The script uses Python 2 syntax like `print "..."`. `fontTools` itself supports current Python versions.)
    *   **`fontTools` Library:** This powerful library is used for manipulating font files.

2.  **Install `fontTools`:**
    The recommended way to install `fontTools` is using `pip`. Open Terminal.app and run:
    ```bash
    pip install fonttools
    ```
    If you don't have `pip` or prefer a different method, you can find installation instructions on the [fontTools GitHub repository](https://github.com/fonttools/fonttools/).

3.  **Download `MacOSXSystemFontReplacer.py`:**
    *   Download the `MacOSXSystemFontReplacer.py` script from its official repository (e.g., from the [project's GitHub page](http://bit.ly/MacOSXSystemFontReplacer)).
    *   Save it to a convenient location on your computer, for example, in your `Downloads` folder or a dedicated `scripts` folder.

## Usage

### 1. Prepare Your Fonts
This is a critical step for the tool to work correctly.
*   Gather the `.otf` or `.ttf` font files you wish to use as replacements.
*   **You must rename your font files** to a specific naming scheme that the tool expects. To see the exact list of required filenames, open Terminal.app, navigate to where you saved the script, and run:
    ```bash
    python MacOSXSystemFontReplacer.py -H
    ```
    This command will print a list of filenames like `System Font Regular.otf`, `System Font Bold.otf`, `System Font Italic.otf`, etc. Your font files **must** be renamed to these exact names, including the `.otf` extension (even if your original font was a `.ttf`).
*   Place all these renamed font files into a single folder (e.g., `my_custom_fonts`).

### 2. Run the Script (Command Line)
*   Open Terminal.app.
*   Navigate (`cd`) to the directory where you saved `MacOSXSystemFontReplacer.py`.
    ```bash
    # Example: if you saved it in a folder named "FontReplacer" on your Desktop
    cd ~/Desktop/FontReplacer
    ```
*   **To install your custom fonts:**
    Run the script using `sudo` (as it typically writes to `/Library/Fonts/`) and point it to your folder of prepared fonts using the `-i` option.
    ```bash
    sudo python MacOSXSystemFontReplacer.py -i /path/to/your_custom_fonts_folder
    ```
    If your prepared fonts are in the *same directory* as the script, you can simplify this:
    ```bash
    sudo python MacOSXSystemFontReplacer.py -i .
    ```
    If the script is in the current directory and your fonts are also in the current directory (e.g., you `cd`-ed into the folder containing both the script and your fonts):
    ```bash
    sudo python ./MacOSXSystemFontReplacer.py
    ```
    (The default for `-i` is the current working directory).

*   **Example with font size adjustment:**
    If you find the replaced fonts appear too small or large, you can use the `-s` option to scale them. For example, to make them appear 5% larger:
    ```bash
    sudo python MacOSXSystemFontReplacer.py -i /path/to/your_custom_fonts_folder -s 105
    ```

*   **Log Out and Log In:** After the script successfully installs the fonts and resets the font caches, you **must log out of your macOS user session and log back in** for the changes to take effect. (This step is not needed if you use the `-c` option to prevent cache resetting).

### 3. Uninstalling Fonts
*   To remove the custom fonts installed by this tool and revert to the macOS default system fonts:
    ```bash
    sudo python MacOSXSystemFontReplacer.py -u
    ```
*   Again, you will need to **log out and log back in** for the changes to be fully applied (unless `-c` was used during uninstallation, though a logout/login is generally good practice to ensure system state is clean).

### Command-Line Options
Here's a summary of the available command-line options. You can also view this by running `python MacOSXSystemFontReplacer.py -h`:

```
  -h, --help            Show this help message and exit.
  -i INPUT_FOLDER, --input-folder INPUT_FOLDER
                        Path to the folder containing your prepared input font
                        files (which you've renamed according to the tool's
                        expected scheme). Defaults to the current directory.
  -s FONT_SIZE, --font-size FONT_SIZE
                        Scale the relative font size in percent. For example,
                        100 means no change, 105 makes the font 5% optically
                        larger, 95 makes it 5% smaller. Default is 100.
  -o OUTPUT_FOLDER, --output-folder OUTPUT_FOLDER
                        Custom path to the folder where your patched fonts
                        will be installed. The default is /Library/Fonts/.
                        Writing to the default location typically requires
                        running the script with 'sudo'.
  -H, --help-more       Print the exact font filenames that the tool expects
                        to find in the -i INPUT_FOLDER and then exit. This
                        is crucial for preparing your fonts.
  -u, --uninstall       Uninstall previously patched fonts from the system
                        folder (or the custom -o OUTPUT_FOLDER) and then exit.
  -c, --no-reset-caches
                        Do not reset macOS font caches after installing or
                        uninstalling. This is useful if you are testing,
                        especially with a custom -o OUTPUT_FOLDER, as it avoids
                        the need to log out and back in repeatedly.
                        However, for changes to be visible system-wide,
                        caches usually need resetting.
  -t SYSTEM_TTC, --system-ttc SYSTEM_TTC
                        Custom path to the system's .ttc (TrueType Collection)
                        font file. The default is
                        /System/Library/Fonts/HelveticaNeueDeskInterface.ttc
                        (for OS X 10.10 Yosemite). This option allows the tool
                        to potentially work with different versions of macOS
                        that might store their UI fonts elsewhere or in a
                        differently named file.
```

### Programmatic Use
This script is primarily designed and intended for command-line operation. The core font patching logic is within the `patchFont` function. While it might be possible to import and use this function in other Python scripts, it would require careful management of its parameters (which are typically derived from `argparse`) and understanding its side effects (file system operations, calls to `atsutil`). For most users and common use cases, interacting with `MacOSXSystemFontReplacer.py` via the command line is the recommended and supported method.

---

## Technical Details

### How It Works
The `MacOSXSystemFontReplacer.py` script automates the complex process of replacing system UI fonts. Here's a breakdown of its operation:

1.  **Argument Parsing:** The script first parses any command-line arguments provided by the user (e.g., input folder, uninstall flag, font size scaling).
2.  **System Font Iteration:** It targets a system font collection file (a `.ttc` file, by default `HelveticaNeueDeskInterface.ttc` located in `/System/Library/Fonts/`). The script iterates through each individual font face defined within this TTC.
3.  **Font Name Extraction:** For each font face from the system TTC, it extracts its full font name (e.g., "System Font Regular", "System Font Bold"). This name is key to matching it with a user-provided font.
4.  **User Font Matching:** The script then looks in the user-specified input folder (`-i`) for a font file whose name exactly matches the extracted system font name (e.g., `System Font Regular.otf`).
5.  **Font Patching (if installing and a match is found):**
    This is the core of the tool, performed by the `patchFont` function using the `fontTools` library:
    *   **Open Fonts:** It opens the relevant system font (from the TTC) and the corresponding user-provided font file.
    *   **Calculate Scale Factor:** A `scaleFactor` is computed based on the `unitsPerEm` values of the system font and the user's font, and adjusted by the user-provided percentage scaling (`-s` option). This factor aims to make the user's font optically similar in size to the system font it's replacing.
    *   **Metric Adjustments:** Critical vertical font metrics in the user's font are modified. These include `OS/2` table values (`sTypoAscender`, `sTypoDescender`, `sTypoLineGap`, `usWinAscent`, `usWinDescent`) and `hhea` table values (`ascent`, `descent`, `lineGap`). These are scaled relative to the system font's metrics using the calculated `scaleFactor`. This step is vital for ensuring the replaced font aligns correctly within UI elements (e.g., doesn't get clipped or appear offset).
    *   **Name Table Replacement:** The entire `name` table from the original system font is copied directly into the user's font. The `name` table contains various identifying strings for the font, such as family name, style name, PostScript name, full name, etc. By using the system font's name table, macOS is tricked into recognizing and using the patched font as if it were the original system UI font.
    *   **UnitsPerEm Scaling:** The `head.unitsPerEm` value of the output (patched) font is also scaled according to the `-s` option.
    *   **CFF Data Adjustments (for OpenType CFF fonts):** If the user's font is an OpenType font with CFF outlines (not TrueType outlines), the script also adjusts its `FontMatrix` (which defines the transformation from glyph coordinates to text space units) using the `fontScale` (derived from the `-s` option). It also attempts to update the `FullName`, `FamilyName`, and PostScript name within the CFF private dictionary, taking these from the system font's `name` table.
    *   **Saving the Patched Font:** The modified user font (now containing the system font's name table and adjusted metrics) is saved into the designated output folder (default: `/Library/Fonts/`). It is saved with the system font name (e.g., `System Font Regular.otf`).
6.  **Uninstallation Process:** If the `-u` (uninstall) flag is used, the script identifies the font files it would normally install (based on names from the system TTC) and attempts to delete these files from the output folder.
7.  **Font Cache Management:** After installing or uninstalling fonts, the script (by default) executes the command `sudo atsutil databases -remove`. This command clears macOS's font caches. A system logout and login are then required for these cache changes (and thus the font changes) to take effect system-wide. If the `-c` option is used, this cache reset step is skipped.

### Important Considerations
These points were highlighted in the original documentation and remain important:

*   **Font EULAs:** When you use a font with this tool, you are modifying it. Please check the End User License Agreement (EULA) of your chosen fonts to ensure that such modification is permitted.
*   **Redistribution Warning:** The patched fonts created by this tool incorporate name data from Apple's copyrighted system fonts. **Therefore, you should not redistribute or share the patched font files generated by this tool.** Use them only for your personal system customization.
*   **File Naming is Key:** You must use the exact filenames that the tool expects. Run `python MacOSXSystemFontReplacer.py -H` to get this list. Always use the `.otf` extension for your prepared files, even if the original font was a `.ttf`.
*   **Partial Replacement is Possible:** You don't need to provide replacements for *all* system UI fonts. If you only provide `System Font Regular.otf` and `System Font Bold.otf`, for example, other UI elements that use different weights or styles will fall back to the default system fonts.
*   **Visual Size Tuning:** If, after applying the changes, your new UI fonts appear too small or too large, use the `-s FONT_SIZE` option. For instance, `-s 105` will attempt to make them 5% optically larger. You might need to experiment to find the perfect size.
*   **Default System TTC:** In Mac OS X 10.10 Yosemite, the default UI fonts are located in `/System/Library/Fonts/HelveticaNeueDeskInterface.ttc`. The `-t` option allows you to specify a different TTC file, which might make the tool adaptable to newer versions of macOS if the location or name of this file changes.

## Contributing

Contributions to `MacOSXSystemFontReplacer` are welcome! Whether it's bug reports, feature suggestions, or code contributions, your help is appreciated.

*   **Reporting Issues:** If you encounter a bug, have a question, or want to suggest an enhancement, please open an issue on the project's GitHub repository. Provide as much detail as possible, including your macOS version, Python version, the steps to reproduce the issue, and any error messages.
*   **Pull Requests:** If you'd like to contribute code:
    1.  Fork the repository.
    2.  Create a new branch for your feature or bug fix (e.g., `git checkout -b feature/my-new-feature` or `fix/some-bug`).
    3.  Make your changes. Adhere to the existing code style if possible.
    4.  Test your changes thoroughly to ensure they work as expected and don't introduce new problems.
    5.  Commit your changes with clear and descriptive commit messages.
    6.  Push your branch to your fork on GitHub.
    7.  Submit a pull request to the main repository, explaining the purpose and nature of your changes.
*   **Coding Style:** The script is written in Python and was originally targeted for Python 2.7 (e.g. uses `print "..."` syntax). Please ensure any changes maintain this compatibility or clearly document if a newer Python version is required. The core font manipulations rely on the `fontTools` library.
*   **Authorship:** Please refer to the `AUTHORS` and `CONTRIBUTORS` files for information on project authorship and contributors.

## License

This tool is licensed under the **MIT License**. You can find the full text of the license in the [LICENSE](./LICENSE) file in this repository.

*(Note: The script's internal version string and its header docstring mention "Apache 2 license". However, the `LICENSE` file in this repository is the MIT License. For open-source projects, the `LICENSE` file in the repository root is typically considered the canonical license. This README defers to the `LICENSE` file.)*

## Credits
*   **Original Author:** Adam Twardoch (see `AUTHORS` file).
*   **Version:** 1.1 (as per script: `VERSION = "1.1"`)
*   **Inspiration:** The development of this tool was inspired by:
    *   [jenskutilek/FiraSystemFontReplacement](https://github.com/jenskutilek/FiraSystemFontReplacement)
    *   [dtinth/YosemiteSystemFontPatcher](https://github.com/dtinth/YosemiteSystemFontPatcher)
*   **Homepage:** For more information or to get the latest version, visit the project page (e.g., [MacOSXSystemFontReplacer on GitHub](http://bit.ly/MacOSXSystemFontReplacer) - *link from original README*).

---
*This README was last updated on YYYY-MM-DD.*
