  ---
  description: Resize screenshots for App Store (portrait: 1242x2688, landscape: 2688x1242)
  allowed-tools: Bash(sips:*), Bash(for:*), Bash(do:*), Bash(done:*)
  argument-hint: [directory]
  ---

  Resize all screenshot images in the specified directory for App Store submission:

  1. Find images with "portrait" in the filename and resize to 1242x2688
  2. Find images with "landscape" in the filename and resize to 2688x1242

  Directory to process: $ARGUMENTS

  Use sips to resize the images and verify the dimensions after resizing.

  Usage

  /resize-app-store /path/to/screenshots

