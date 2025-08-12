# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Flutter package called `readmore` that provides expanding/collapsing text functionality with support for custom annotations (hashtags, URLs, mentions). The package allows text to be trimmed by either character length or line count.

## Development Commands

### Testing
- Run example app: `cd example && flutter run`
- Run tests: `flutter test`
- Run widget tests in example: `cd example && flutter test`

### Code Quality
- Check for issues: `flutter analyze`
- Format code: `flutter format .`

### Package Management
- Get dependencies: `flutter pub get`
- Publish (dry run): `flutter pub publish --dry-run`

## Architecture

### Core Components

**ReadMoreText Widget** (`lib/readmore.dart`):
- Main widget with two constructors: `ReadMoreText()` for plain text and `ReadMoreText.rich()` for rich text
- Supports two trim modes: `TrimMode.Length` (character count) and `TrimMode.Line` (line count)
- Uses `ValueNotifier<bool>` for collapse/expand state management
- Implements complex text measuring and trimming logic using `TextPainter`

**Annotation System**:
- `Annotation` class allows regex-based pattern matching with custom styling
- Supports interactive elements like clickable hashtags, URLs, and mentions
- Only available with `ReadMoreText()` constructor, not with `ReadMoreText.rich()`

### Key Implementation Details

**Text Trimming Logic**:
- `_trimTextSpan()` method recursively trims `TextSpan` trees
- Handles both character-based (`splitByRunes: true`) and position-based trimming
- Complex layout calculations determine optimal trim points for line-based trimming

**Layout Measurement**:
- Uses `TextPainter` to measure text dimensions and calculate trim positions
- Accounts for "read more"/"show less" link sizes when determining trim points
- Handles RTL text direction and various text styling options

**State Management**:
- External `ValueNotifier<bool>` can control collapse state
- Internal state management when no external notifier provided
- Proper disposal of gesture recognizers and value notifiers

## Common Issues

### Gradle Plugin Error
If you encounter "Flutter's app_plugin_loader Gradle plugin imperatively using the apply script method" error:

**Problem**: The project uses deprecated `apply from` method for Flutter Gradle plugin.

**Solution**: Update to declarative plugin block:
1. Update `android/settings.gradle` to use `pluginManagement` block
2. Update `android/app/build.gradle` to use `plugins` block instead of `apply plugin`
3. Clean and rebuild: `flutter clean && cd example && flutter pub get && flutter run`

## Development Notes

- The example app in `/example` demonstrates all major features with interactive controls
- Widget uses `LayoutBuilder` to adapt to different container widths
- Text measurement logic is performance-critical - avoid unnecessary recomputations
- Annotation regex patterns are merged into a single pattern for efficiency
- Rich text constructor bypasses annotation system for direct `TextSpan` control