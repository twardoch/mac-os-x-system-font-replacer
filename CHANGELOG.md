# Changelog

All notable changes to this project will be documented in this file.

## [Unreleased]

### Changed
- Code formatting improvements: Updated string formatting from old-style % formatting to modern .format() method
- Improved code style and readability with better indentation and line breaks
- Reorganized import statements for better organization

### Known Issues
- Python 2.7 dependency (deprecated since January 2020)
- May not work with modern macOS versions due to System Integrity Protection (SIP)
- Limited to TTF/OTF font formats only

## [1.1] - Previous Release

### Added
- Initial tool implementation for replacing macOS system UI fonts
- Support for custom font scaling with -s option
- Installation and uninstallation functionality
- Font metrics patching to match system fonts
- Command-line interface with multiple options

### Features
- Safe font replacement without modifying original system fonts
- Support for all macOS UI font weights and styles
- Automatic font cache reset
- Custom output folder support