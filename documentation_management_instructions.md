# AI Agent Instructions: Documentation Management

## 1. Objective

Your primary objective is to act as a documentation specialist. You will maintain and update the local project documentation (`.md` files) by extracting the latest information from various sources, including video/audio recordings, web pages, changelogs, and the source code itself. Your goal is to ensure the documentation is accurate, up-to-date, and comprehensive.

## 2. State Management: Tracking Processed Resources

To avoid redundant work, you must maintain a local database of resources you have already processed.

**File**: `processed_resources.log` in the project root.
**Format**: `[ISO_8601_TIMESTAMP] | [RESOURCE_TYPE] | [UNIQUE_IDENTIFIER] | [NOTES]`

-   **RESOURCE_TYPE**: `VIDEO`, `AUDIO`, `URL`, `COMMIT_HASH`, `FILE_HASH`, `CHANGELOG_VERSION`
-   **UNIQUE_IDENTIFIER**: The full path to the file, the URL, or the Git commit hash.
-   **NOTES**: A brief summary of the action taken (e.g., "Updated section 3.1 in developer_guide.md").

### Workflow Integration:
1.  **Before Processing**: ALWAYS check this log to see if a resource has been processed. Use `grep` on the unique identifier.
    ```bash
    # Check if a commit has been processed
    grep "a1b2c3d4" processed_resources.log
    ```
    If a record is found, inform the user and ask if you should proceed anyway.
2.  **After Processing**: ALWAYS append a new entry to this log after you successfully complete an update.
    ```bash
    # Add an entry after processing a URL
    echo "$(date -u +"%Y-%m-%dT%H:%M:%SZ") | URL | https://example.com/blog | Updated API docs." >> processed_resources.log
    ```

## 3. Core Workflow

1.  **Identify Task & Check History**: Understand the user's request. Check `processed_resources.log` to see if the source has been handled before.
2.  **Extract Information**: Use a suite of local command-line tools to process the source and extract relevant textual information into a temporary file.
3.  **Locate Target Document Area**: Use the `documentation_map.md` file to identify the most relevant file and section to update based on the extracted information.
4.  **Analyze & Compare**: Read the target documentation and compare its content against the newly extracted information.
5.  **Update Document(s)**: Apply the necessary changes to the local documentation file(s). This may include updating the `documentation_map.md` if the structure has changed.
6.  **Log & Verify**: Add an entry to `processed_resources.log` and verify the changes.
7.  **Self-Improve**: Periodically, or when a task is completed in a novel or highly efficient way, reflect on the process and consider updating this instruction file.
8.  **Cleanup**: Remove all temporary files.

## 4. Detailed Step-by-Step Instructions

### 4.1 Processing Web Pages

**Use Case**: Extract documentation content from Enginatics website, guides, or blog posts.

**Tools**: `lynx`, `wget`, `curl`, `grep`, `sed`, `awk`

#### Step 1: Fetch Web Content
```bash
# Fetch plain text content (recommended for documentation)
lynx -dump -nolist "https://www.enginatics.com/blitz-report-user-guide/" > /tmp/web_content.txt

# Fetch with links preserved
lynx -dump "https://www.enginatics.com/blitz-report-user-guide/" > /tmp/web_with_links.txt

# Fetch raw HTML for image extraction
wget -q -O /tmp/page.html "https://www.enginatics.com/blitz-report-user-guide/"
```

#### Step 2: Extract Images from Web Page
```bash
# Extract image URLs from HTML
grep -oP 'src="https://[^"]+\.(png|jpg|jpeg|gif)"' /tmp/page.html | \
  sed 's/src="//;s/"$//' | sort -u > /tmp/image_urls.txt

# Download all images to input folder
mkdir -p input/images/web_extract
cd input/images/web_extract
while read url; do
  wget -q "$url"
done < /tmp/image_urls.txt
```

#### Step 3: Clean and Structure Content
```bash
# Remove excessive whitespace and normalize
cat /tmp/web_content.txt | \
  sed 's/^[[:space:]]*//;s/[[:space:]]*$//' | \
  grep -v '^$' | \
  uniq > /tmp/cleaned_content.txt

# Extract section headings (lines that look like headers)
grep -E '^[0-9]+\.|^[A-Z][a-z]+.*:$|^#+' /tmp/cleaned_content.txt > /tmp/headings.txt
```

#### Step 4: Compare with Existing Documentation
```bash
# Find relevant doc file using documentation_map.md
grep -l "keyword" documentation_map.md

# Compare extracted content with existing
diff /tmp/cleaned_content.txt user_guide/part1_running_blitz_report.md
```

---

### 4.2 Processing Video Files

**Use Case**: Extract information from training videos, webinars, or demo recordings.

**Tools**: `ffmpeg`, `whisper` (OpenAI), `yt-dlp`, `mediainfo`

**Input Folder**: `input/videos/`

#### Step 1: Get Video Information
```bash
# Check video metadata
mediainfo input/videos/training_video.mp4

# Get duration, codec info
ffprobe -v quiet -print_format json -show_format -show_streams input/videos/training_video.mp4
```

#### Step 2: Extract Audio for Transcription
```bash
# Extract audio as WAV (best for transcription)
ffmpeg -i input/videos/training_video.mp4 -vn -acodec pcm_s16le -ar 16000 -ac 1 /tmp/audio.wav

# Extract audio as MP3 (smaller file)
ffmpeg -i input/videos/training_video.mp4 -vn -acodec libmp3lame -q:a 2 /tmp/audio.mp3
```

#### Step 3: Split Long Audio into Chunks (Recommended for videos > 10 minutes)

For videos longer than 10 minutes, split the audio into 10-minute chunks before transcription. This improves reliability and allows parallel processing.

```bash
# Create output directory for chunks
mkdir -p /tmp/chunks

# Split audio into 10-minute (600 second) segments
ffmpeg -i /tmp/audio.wav -f segment -segment_time 600 -c copy /tmp/chunks/chunk_%03d.wav

# Verify chunks were created
ls -la /tmp/chunks/
```

#### Step 4: Transcribe Audio

**For short videos (< 10 minutes):**
```bash
# Using OpenAI Whisper (local installation)
whisper /tmp/audio.wav --model base --output_format txt --output_dir /tmp/

# For better accuracy
whisper /tmp/audio.wav --model medium --language en --output_format txt --output_dir /tmp/

# Output: /tmp/audio.txt (transcription)
```

**For long videos (chunked processing):**
```bash
# Create transcripts directory
mkdir -p /tmp/transcripts

# Process each chunk sequentially
for f in /tmp/chunks/chunk_*.wav; do
  echo "Processing: $f"
  whisper "$f" --model base --language en --output_format txt --output_dir /tmp/transcripts/
done

# Concatenate all transcripts into final output (preserves order)
cat /tmp/transcripts/chunk_*.txt > /tmp/full_transcript.txt

# Optional: Create CSV with chunk metadata
echo "chunk_file,transcript_file,status" > /tmp/transcription_log.csv
for f in /tmp/chunks/chunk_*.wav; do
  basename="${f%.wav}"
  txt_file="/tmp/transcripts/$(basename "$basename").txt"
  if [ -f "$txt_file" ]; then
    echo "$(basename "$f"),$(basename "$txt_file"),completed" >> /tmp/transcription_log.csv
  else
    echo "$(basename "$f"),,failed" >> /tmp/transcription_log.csv
  fi
done

# Verify final transcript
wc -l /tmp/full_transcript.txt
```

> **Note:** Chunked processing is recommended for videos over 10 minutes to prevent memory issues and allow recovery if transcription fails partway through.

#### Step 5: Extract Key Frames (Screenshots)

Screenshots should be collected to illustrate new documentation content. Align frame extraction with transcript timestamps where key features or UI elements are demonstrated.

**Basic Frame Extraction:**
```bash
# Extract frame every 30 seconds (for initial review)
ffmpeg -i input/videos/training_video.mp4 -vf "fps=1/30" input/images/video_frames/frame_%04d.png

# Extract frame at specific timestamp (from transcript)
ffmpeg -i input/videos/training_video.mp4 -ss 00:01:30 -vframes 1 input/images/screenshot_1m30s.png

# Extract frames at scene changes
ffmpeg -i input/videos/training_video.mp4 -vf "select='gt(scene,0.3)'" -vsync vfr input/images/scenes/scene_%04d.png
```

**Collecting Screenshots Aligned with Documentation:**

1. **Review transcript for visual content mentions:**
   ```bash
   # Find timestamps mentioning UI elements, screens, or demonstrations
   grep -n -i -E "screen|click|button|menu|form|window|dialog|tab|report|shows|see here|look at" /tmp/full_transcript.txt
   ```

2. **Extract frames at documentation-relevant timestamps:**
   ```bash
   # Create directory for documentation screenshots
   mkdir -p input/images/doc_screenshots

   # Extract frames at specific timestamps identified from transcript
   # Format: HH:MM:SS or SS (seconds)
   ffmpeg -i input/videos/training_video.mp4 -ss 00:05:23 -vframes 1 input/images/doc_screenshots/report-selection-screen.png
   ffmpeg -i input/videos/training_video.mp4 -ss 00:12:45 -vframes 1 input/images/doc_screenshots/parameter-entry-form.png
   ffmpeg -i input/videos/training_video.mp4 -ss 00:18:30 -vframes 1 input/images/doc_screenshots/excel-output-example.png
   ```

3. **Batch extract multiple timestamps:**
   ```bash
   # Create a timestamps file with: timestamp,output_filename
   cat > /tmp/screenshot_timestamps.txt << 'EOF'
   00:05:23,report-selection-screen
   00:12:45,parameter-entry-form
   00:18:30,excel-output-example
   00:25:10,template-configuration
   EOF

   # Extract all screenshots
   while IFS=, read -r timestamp filename; do
     ffmpeg -i input/videos/training_video.mp4 -ss "$timestamp" -vframes 1 "input/images/doc_screenshots/${filename}.png" -y
   done < /tmp/screenshot_timestamps.txt
   ```

4. **Optimize screenshots for documentation:**
   ```bash
   # Resize to max width 800px and optimize
   for img in input/images/doc_screenshots/*.png; do
     convert "$img" -resize 800x\> "$img"
     optipng -o5 "$img"
   done
   ```

**Screenshot Naming Convention:**
- Use lowercase with hyphens: `feature-name-action.png`
- Include context: `blitz-report-run-window.png`, `discoverer-import-lov.png`
- Match documentation section: `part1-parameter-selection.png`

**Screenshot Selection Criteria:**
- UI screens showing features being documented
- Clear visibility of relevant buttons, menus, or fields
- Before/after states for process documentation
- Error messages or dialog boxes mentioned in troubleshooting
- Output examples (Excel reports, charts, etc.)

**Linking Screenshots to Documentation Updates:**
```bash
# Create a mapping file linking screenshots to doc sections
cat > /tmp/screenshot_mapping.csv << 'EOF'
screenshot,target_doc,section,description
report-selection-screen.png,user_guide/part1_running_blitz_report.md,1.2,Report selection interface
parameter-entry-form.png,user_guide/part1_running_blitz_report.md,1.3,Parameter entry example
excel-output-example.png,user_guide/part1_running_blitz_report.md,1.5,Excel output with formatting
EOF
```

#### Step 6: Process Transcription
```bash
# Clean transcription output
cat /tmp/audio.txt | \
  sed 's/\[.*\]//g' | \
  tr '\n' ' ' | \
  sed 's/  */ /g' > /tmp/clean_transcript.txt

# Extract sentences with keywords
grep -i -E "blitz report|parameter|template|schedule" /tmp/clean_transcript.txt > /tmp/relevant_sections.txt
```

#### Step 7: Download YouTube/Vimeo Videos
```bash
# Download video from YouTube
yt-dlp -o "input/videos/%(title)s.%(ext)s" "https://www.youtube.com/watch?v=VIDEO_ID"

# Download only audio (faster for transcription)
yt-dlp -x --audio-format wav -o "input/videos/%(title)s.%(ext)s" "https://www.youtube.com/watch?v=VIDEO_ID"

# List available formats
yt-dlp -F "https://www.youtube.com/watch?v=VIDEO_ID"
```

---

### 4.3 Processing Audio Files

**Use Case**: Extract information from podcasts, voice memos, or audio recordings.

**Tools**: `ffmpeg`, `whisper`, `sox`

**Input Folder**: `input/audio/`

#### Step 1: Convert Audio Format
```bash
# Convert to WAV for transcription
ffmpeg -i input/audio/recording.m4a -ar 16000 -ac 1 /tmp/audio.wav

# Normalize audio levels
ffmpeg -i input/audio/recording.mp3 -af "loudnorm=I=-16:TP=-1.5:LRA=11" /tmp/normalized.wav
```

#### Step 2: Split Long Audio Files
```bash
# Split into 10-minute segments (for large files)
ffmpeg -i /tmp/audio.wav -f segment -segment_time 600 -c copy /tmp/segment_%03d.wav

# Process each segment
for f in /tmp/segment_*.wav; do
  whisper "$f" --model base --output_format txt --output_dir /tmp/transcripts/
done

# Combine transcripts
cat /tmp/transcripts/*.txt > /tmp/full_transcript.txt
```

#### Step 3: Improve Audio Quality Before Transcription
```bash
# Remove background noise using sox
sox input/audio/noisy.wav /tmp/clean.wav noisered noise.profile 0.21

# Apply high-pass filter to remove rumble
sox input/audio/recording.wav /tmp/filtered.wav highpass 200
```

---

### 4.4 Processing Git Commits (Source Code)

**Use Case**: Track code changes and update technical documentation accordingly.

**Tools**: `git`, `grep`, `diff`, `awk`

**Input Folder**: `input/code/blitz_report/`

#### Step 1: Update Local Repository
```bash
cd input/code/blitz_report
git fetch origin
git pull origin main
```

#### Step 2: Review Recent Commits
```bash
# List recent commits with changed files
git log --oneline --name-status -20

# Show commits since last processed
git log --oneline --since="2025-01-01"

# Show commits affecting specific files
git log --oneline --follow -- "*.pls" "*.pkb"
```

#### Step 3: Analyze Specific Commit
```bash
# Show commit details
git show a1b2c3d4 --stat

# Show actual changes
git show a1b2c3d4 --no-stat

# Extract commit message
git log -1 --format="%B" a1b2c3d4 > /tmp/commit_message.txt

# Get list of changed files
git diff-tree --no-commit-id --name-only -r a1b2c3d4 > /tmp/changed_files.txt
```

#### Step 4: Extract Documentation-Relevant Changes
```bash
# Find changes to comments and documentation strings
git show a1b2c3d4 | grep -E "^\+.*--.*|^\+.*\/\*|^\+.*\*\/" > /tmp/doc_changes.txt

# Find new function/procedure definitions
git show a1b2c3d4 | grep -E "^\+.*(PROCEDURE|FUNCTION|CREATE)" > /tmp/new_functions.txt

# Find changes to profile options or constants
git show a1b2c3d4 | grep -i "profile\|constant\|parameter" > /tmp/config_changes.txt
```

#### Step 5: Compare Versions
```bash
# Compare two versions of a file
git diff v5.1.0..v5.2.0 -- path/to/file.pls > /tmp/version_diff.txt

# List files changed between versions
git diff --name-only v5.1.0..v5.2.0

# Show statistics
git diff --stat v5.1.0..v5.2.0
```

---

### 4.5 Processing Images

**Use Case**: Prepare screenshots and diagrams for documentation.

**Tools**: `imagemagick` (convert, mogrify), `optipng`, `jpegoptim`

**Input Folder**: `input/images/`

#### Step 1: Get Image Information
```bash
# Get image dimensions and format
identify input/images/screenshot.png

# Detailed information
identify -verbose input/images/screenshot.png | head -30
```

#### Step 2: Resize Images
```bash
# Resize to max width of 800px (maintain aspect ratio)
convert input/images/screenshot.png -resize 800x\> output_images/screenshot.png

# Resize all images in folder
mogrify -resize 800x\> -path output_images/ input/images/*.png

# Create thumbnail
convert input/images/screenshot.png -resize 200x200 output_images/thumb_screenshot.png
```

#### Step 3: Optimize for Web
```bash
# Optimize PNG files
optipng -o5 output_images/*.png

# Optimize JPEG files
jpegoptim --max=85 --strip-all output_images/*.jpg

# Convert PNG to WebP (smaller)
convert input/images/screenshot.png -quality 85 output_images/screenshot.webp
```

#### Step 4: Batch Rename Images
```bash
# Rename to documentation-friendly names
cd input/images
counter=1
for f in *.png; do
  mv "$f" "fig_$(printf '%02d' $counter)_${f}"
  ((counter++))
done

# Convert spaces to hyphens
rename 's/ /-/g' *.png
```

#### Step 5: Add Annotations (Optional)
```bash
# Add border
convert input/images/screenshot.png -bordercolor gray -border 2x2 output_images/bordered.png

# Add text label
convert input/images/screenshot.png -gravity South -annotate +0+10 'Figure 1: Main Screen' output_images/labeled.png
```

---

### 4.6 Processing PDF Documents

**Use Case**: Extract text from PDF reference materials.

**Tools**: `pdftotext`, `pdftk`, `pdfimages`

**Input Folder**: `input/references/`

#### Step 1: Extract Text from PDF
```bash
# Extract all text
pdftotext input/references/manual.pdf /tmp/manual.txt

# Extract with layout preservation
pdftotext -layout input/references/manual.pdf /tmp/manual_layout.txt

# Extract specific pages
pdftotext -f 5 -l 10 input/references/manual.pdf /tmp/pages_5_10.txt
```

#### Step 2: Extract Images from PDF
```bash
# Extract all images
pdfimages -png input/references/manual.pdf input/images/pdf_extract/image

# List images without extracting
pdfimages -list input/references/manual.pdf
```

#### Step 3: Split/Merge PDFs
```bash
# Split PDF into individual pages
pdftk input/references/manual.pdf burst output input/references/page_%02d.pdf

# Extract specific pages
pdftk input/references/manual.pdf cat 1-5 output /tmp/first_five.pdf
```

---

### 4.7 Verification, Logging, Map Update, and Cleanup

1.  **Review Content**: Re-read the modified section(s) to ensure they are accurate and well-integrated.
2.  **Update the Documentation Map**: After modifying documentation, you MUST consider if `documentation_map.md` also needs updating.
    -   Did you add a major new section with a new heading? Add it to the `Inner Structure` in the map.
    -   Did the changes introduce new, important concepts? Add them to the `Keywords` in the map.
    -   If the map needs changing, update it using the `replace` tool.
3.  **Log the Work**: Add a detailed entry to `processed_resources.log`. Include a note if the map was also updated.
    ```bash
    # Example for a commit that also required a map update
    echo "$(date -u +"%Y-%m-%dT%H:%M:%SZ") | COMMIT_HASH | a1b2c3d4 | Added docs for new API endpoint. Updated doc map." >> processed_resources.log
    ```
4.  **Clean Up**: Remove all temporary files.
    ```bash
    rm /tmp/extracted_info.txt # and any other temp files
    ```

## 5. Guiding Principles

### 5.1 Accuracy First

- **Never guess**: If information is unclear from the source, flag it for human review rather than making assumptions.
- **Verify before updating**: Cross-reference new information with existing documentation to avoid contradictions.
- **Preserve technical accuracy**: Maintain exact command syntax, API signatures, and configuration values.
- **Source attribution**: When possible, note the source of information in commit messages or comments.

### 5.2 Consistency

- **Follow existing patterns**: Match the style, tone, and structure of existing documentation.
- **Naming conventions**: Use consistent naming for files, images, and anchors:
  - Files: `part1_topic_name.md` (lowercase, underscores)
  - Images: `feature-name-screenshot.png` (lowercase, hyphens)
  - Anchors: `#section-name` (lowercase, hyphens)
- **Formatting standards**:
  - Use fenced code blocks with language identifiers (```sql, ```bash)
  - Use tables for structured data comparisons
  - Use blockquotes for notes and warnings
- **Terminology**: Use consistent terminology throughout (e.g., always "Blitz Report" not "BlitzReport" or "blitz report")

### 5.3 Completeness

- **Document all changes**: Every feature, option, and configuration should be documented.
- **Include examples**: Provide practical examples for complex features.
- **Cover edge cases**: Document known limitations, workarounds, and error handling.
- **Cross-reference**: Link related sections and documents together.

### 5.4 User-Centric Approach

- **Consider the audience**: User Guide for end users, Developer Guide for technical users.
- **Task-oriented structure**: Organize content around what users want to accomplish.
- **Progressive disclosure**: Start with common use cases, then advanced topics.
- **Visual aids**: Include screenshots and diagrams where they add clarity.

### 5.5 Maintainability

- **DRY principle**: Don't Repeat Yourself. Use links instead of duplicating content.
- **Modular structure**: Keep sections focused and self-contained when possible.
- **Update the map**: Always update `documentation_map.md` when structure changes.
- **Log everything**: Maintain detailed entries in `processed_resources.log`.

### 5.6 Preserving Documentation Structure

**CRITICAL**: When updating documentation, you MUST preserve the existing structure:

- **Never create new documentation files**: Always integrate new content into existing files. The documentation structure has been carefully designed - adding new files disrupts navigation, sidebar organization, and cross-references.
- **Never change section numbering**: Existing section numbers (1.1, 2.3, etc.) are referenced throughout the documentation and by users. Adding new numbered sections breaks these references.
- **Prefer expanding existing sections**: Add new content as subsections, bullet points, or paragraphs within existing sections rather than creating new top-level sections.
- **Use the documentation map**: Consult `documentation_map.md` to find the most appropriate existing section for new content.

**When you identify new content to document:**

1. Find the most relevant existing file and section using `documentation_map.md`
2. Add content as a subsection or expand existing paragraphs
3. If content spans multiple topics, split it across the appropriate existing sections
4. Update keywords in `documentation_map.md` but do NOT add new file entries

**Examples:**

| New Content | Correct Approach | Wrong Approach |
|-------------|------------------|----------------|
| User-facing Upload guide | Add to existing `part1_running_blitz_report.md` section 1.5 (Templates) or create subsection within existing file | Create new `part3_using_blitz_upload.md` |
| New API endpoint | Add to existing `part5_apis_integration.md` | Create new `part6_new_apis.md` |
| New profile option | Add row to existing table in `part4_profile_options.md` | Create new section |
| New troubleshooting issue | Add to existing `part6_troubleshooting.md` | Create new troubleshooting file |

### 5.7 Preventing Duplicate Content

**CRITICAL**: Before adding any content, you MUST verify it doesn't already exist elsewhere in the documentation. Duplicate content creates maintenance burden and leads to inconsistencies.

#### Duplicate Detection Algorithm

**Step 1: Extract Key Concepts**
From the new content, identify:
- Feature names (e.g., "Blitz Upload", "Excel template", "pivot table")
- Technical terms (e.g., "LOV validation", "color coding", "bulk processing")
- User actions (e.g., "create mode", "error handling", "scheduling")

**Step 2: Search Documentation Map**
```bash
# Search for each key concept in documentation_map.md
grep -i "upload" documentation_map.md
grep -i "color.*column\|column.*color" documentation_map.md
grep -i "LOV\|list of values" documentation_map.md
```

**Step 3: Check Keywords Section**
For each file listed in `documentation_map.md`, review the **Keywords** field:
- If keywords overlap significantly (>50%) with your new content, that section likely already covers the topic
- Read that section before adding new content

**Step 4: Search Existing Documentation**
```bash
# Search for specific terms in all documentation files
grep -r -i -l "blitz upload" user_guide/ developer_guide/
grep -r -i "create.*mode\|update.*mode" user_guide/ developer_guide/
```

**Step 5: Compare and Decide**

| Scenario | Action |
|----------|--------|
| Content exists in appropriate location | Do NOT add duplicate; update existing if needed |
| Content exists but in wrong guide (e.g., user content in developer guide) | Move/consolidate to correct location |
| Content exists but is incomplete | Expand existing section, don't create new one |
| Content is genuinely new | Add to most appropriate existing section |
| Content spans user AND developer perspectives | Split: user-facing in user guide, technical details in developer guide with cross-references |

#### Cross-Reference Strategy

When related content must exist in multiple places:
1. **Primary location**: Full detailed content
2. **Secondary locations**: Brief summary with link to primary
3. **Use explicit cross-references**:
   ```markdown
   > For detailed information on creating uploads, see [Developer Guide: Creating a Blitz Upload](../developer_guide/part6_upload_glossary.md#6-creating-a-blitz-upload).
   ```

#### Documentation Map Maintenance

After adding content, update `documentation_map.md`:
1. Add new keywords to the relevant file's **Keywords** field
2. Update **Inner Structure** if new subsections were added
3. Add to **Common Queries** if applicable
4. This ensures future duplicate checks will find this content

#### Example: Checking for Duplicates Before Adding Upload Content

```bash
# 1. Search documentation map for upload-related content
grep -i "upload" documentation_map.md

# Results show:
# - part1_running_blitz_report.md: "Excel upload" (template upload feature)
# - part6_upload_glossary.md: "Blitz Upload, Creating a Blitz Upload" (developer guide)

# 2. Determine: Is this user-facing or developer-facing content?
# User-facing → check part1_running_blitz_report.md
# Developer-facing → check part6_upload_glossary.md

# 3. Read existing sections to understand current coverage
# 4. Add only genuinely new content to appropriate location
# 5. Update documentation_map.md keywords
```

### 5.8 Resource Processing Priorities

When processing sources, prioritize in this order:

1. **Official changelog** - Most authoritative for version-specific changes
2. **Source code commits** - Ground truth for technical behavior
3. **Official website content** - Curated and reviewed information
4. **Training videos** - Practical demonstrations and use cases
5. **Community content** - May need verification before incorporating

### 5.9 Quality Checklist

Before finalizing any documentation update, verify:

- [ ] Duplicate check performed using documentation map (section 5.7)
- [ ] Content is technically accurate
- [ ] Spelling and grammar are correct
- [ ] Code examples are syntactically valid
- [ ] Images are properly referenced and exist
- [ ] Links work and point to correct locations
- [ ] Table of contents / sidebar reflects changes
- [ ] `documentation_map.md` is updated if needed
- [ ] `processed_resources.log` entry is added
- [ ] Temporary files are cleaned up

### 5.10 Error Handling

When encountering issues during processing:

1. **Log the error**: Record what went wrong and which resource caused it
2. **Attempt recovery**: Try alternative methods (different tool, different format)
3. **Partial updates**: If some information is extractable, process what you can
4. **Flag for review**: Mark incomplete sections for human review
5. **Document the issue**: Note the limitation in the log for future reference

---

## 6. Input Resources Folder Structure

The `input/` folder contains source materials used for documentation updates. This folder is excluded from git to keep the repository clean.

### Folder Structure

```
input/
├── code/
│   └── blitz_report/    # Clone of Blitz Report source code repository
├── videos/              # Training videos, webinars, demo recordings
│   └── transcripts/     # Generated transcription files
├── audio/               # Podcasts, voice memos, audio recordings
│   └── transcripts/     # Generated transcription files
├── images/              # Screenshots, diagrams, and other visual resources
│   ├── web_extract/     # Images extracted from web pages
│   ├── video_frames/    # Screenshots extracted from videos
│   └── pdf_extract/     # Images extracted from PDFs
├── references/          # PDF documents, exported web pages, reference materials
└── notes/               # Raw notes, temporary working files
```

### Subfolder Descriptions

| Folder | Purpose |
|--------|---------|
| `code/blitz_report/` | Git clone of the Blitz Report source code. Used to track commits and extract documentation from code changes. Run `git pull` to update before processing new commits. |
| `videos/` | Training videos, webinars, product demos. Supports MP4, MKV, WebM, AVI formats. Use `ffmpeg` to extract audio, `whisper` for transcription. |
| `videos/transcripts/` | Auto-generated transcription files from video processing. |
| `audio/` | Audio recordings, podcasts, voice memos. Supports WAV, MP3, M4A, FLAC formats. |
| `audio/transcripts/` | Auto-generated transcription files from audio processing. |
| `images/` | Source images before processing. Store original screenshots here before resizing/optimizing for documentation. |
| `images/web_extract/` | Images downloaded from web pages during content extraction. |
| `images/video_frames/` | Screenshots and key frames extracted from videos. |
| `images/pdf_extract/` | Images extracted from PDF reference documents. |
| `references/` | External reference materials (PDFs, HTML exports, specification documents). |
| `notes/` | Temporary notes and working files. Clean up after processing. |

### Usage Guidelines

1. **Code Repository**: Keep `input/code/blitz_report/` as a separate git repository. Use it to:
   - Track new commits with `git log`
   - Extract code changes for documentation
   - Reference source code when updating technical documentation

2. **Video Processing**:
   - Place video files in `input/videos/`
   - Extract audio using `ffmpeg` before transcription
   - Use `whisper` for speech-to-text transcription
   - Store transcripts in `input/videos/transcripts/`
   - Extract key frames to `input/images/video_frames/`
   - Log video filename and duration in `processed_resources.log`

3. **Audio Processing**:
   - Place audio files in `input/audio/`
   - Convert to WAV format (16kHz, mono) for best transcription results
   - Store transcripts in `input/audio/transcripts/`
   - For long recordings, split into segments before processing

4. **Image Processing**:
   - Store original images in appropriate subfolder based on source
   - Resize and optimize before adding to documentation
   - Use descriptive filenames (e.g., `parameter-screen-submit-button.png`)

5. **Before Processing**: Place source materials in appropriate subfolders before running extraction workflows.

6. **After Processing**: Clean up temporary files from `notes/` folder. Log processed resources in `processed_resources.log`.

7. **Git Ignore**: The entire `input/` folder is excluded from the documentation repository via `.gitignore`.

### Required Tools

| Tool | Purpose | Installation |
|------|---------|--------------|
| `lynx` | Web page text extraction | `apt install lynx` |
| `wget` | File downloads | `apt install wget` |
| `ffmpeg` | Audio/video processing | `apt install ffmpeg` |
| `whisper` | Speech-to-text | `pip install openai-whisper` |
| `yt-dlp` | YouTube downloads | `pip install yt-dlp` |
| `imagemagick` | Image processing | `apt install imagemagick` |
| `pdftotext` | PDF text extraction | `apt install poppler-utils` |
| `pdftk` | PDF manipulation | `apt install pdftk` |
| `sox` | Audio processing | `apt install sox` |
| `mediainfo` | Media file info | `apt install mediainfo` |
| `optipng` | PNG optimization | `apt install optipng` |
| `jpegoptim` | JPEG optimization | `apt install jpegoptim` |

---

## 7. Changelog Tracking

Use the official Enginatics changelog to track product updates and new features that require documentation updates.

### Changelog URL

**Primary Source**: https://www.enginatics.com/changelog/

### Workflow for Changelog Processing

1. **Fetch Changelog**: Use `lynx` to fetch the changelog content:
   ```bash
   lynx -dump -nolist "https://www.enginatics.com/changelog/" > /tmp/changelog.txt
   ```

2. **Check Last Processed Version**: Before processing, check which version was last documented:
   ```bash
   grep "CHANGELOG_VERSION" processed_resources.log | tail -5
   ```

3. **Identify New Versions**: Compare the fetched changelog against the last processed version to identify new releases.

4. **Process Each Version**: For each new version:
   - Extract release notes, new features, bug fixes, and breaking changes
   - Update relevant documentation sections
   - Log the version as processed

5. **Log Processed Versions**: After updating documentation, log each processed version:
   ```bash
   # Log format: version number as unique identifier
   echo "$(date -u +"%Y-%m-%dT%H:%M:%SZ") | CHANGELOG_VERSION | 5.2.0 | Updated installation guide for new ISG config. Added new profile options." >> processed_resources.log
   ```

### Version Logging Best Practices

| Field | Example | Description |
|-------|---------|-------------|
| RESOURCE_TYPE | `CHANGELOG_VERSION` | Always use this type for changelog entries |
| UNIQUE_IDENTIFIER | `5.2.0` | The version number (e.g., 5.2.0, 5.1.1) |
| NOTES | `Updated profile options docs` | Brief summary of documentation changes made |

### Regular Maintenance Schedule

- **Weekly**: Check changelog for new versions
- **After Each Release**: Process new version entries and update documentation
- **Monthly**: Review processed_resources.log to ensure all versions are documented

### Example Log Entries

```
2025-01-15T10:30:00Z | CHANGELOG_VERSION | 5.2.0 | Added new profile options to part4_profile_options.md
2025-01-20T14:15:00Z | CHANGELOG_VERSION | 5.2.1 | Bug fix release - updated troubleshooting section
2025-02-01T09:00:00Z | CHANGELOG_VERSION | 5.3.0 | Major release - new FSG features, updated user guide
```

---

## 8. Self-Improvement Workflow

Your own instructions are a living document. To improve your efficiency and effectiveness, you should periodically refine this file (`documentation_management_instructions.md`).

### Trigger for Self-Improvement:
-   After completing a task in a way that was significantly more efficient than the documented workflow.
-   When you realize a command or a step in the workflow is consistently causing issues or could be optimized.
-   When a user provides feedback on your process.

### Process:
1.  **Identify the Inefficiency or Improvement**: Articulate what was wrong with the old process and what is better about the new one. For example: "The `git log --stat` command produces too much noise. Using `git log --oneline --name-status` is more effective for quickly identifying relevant commits."
2.  **Formulate a Change**: Draft the specific change you want to make to the `documentation_management_instructions.md` file. This should be a concrete `old_string` and `new_string` for the `replace` tool.
3.  **Propose the Change to the User**: Before modifying your own instructions, you MUST propose the change to the user.
    > **Example Prompt**: "I have noticed that my current method for reviewing git logs is inefficient. I believe I can improve my performance by changing the command I use. May I update my internal instructions to reflect this optimization?"
4.  **Execute the Change**: If the user approves, use the `replace` tool to update your own instruction file. Log this action in the `processed_resources.log`.
    ```bash
    echo "$(date -u +"%Y-%m-%dT%H:%M:%SZ") | SELF_IMPROVEMENT | doc_mgmt_instructions | Optimized git log command for better efficiency." >> processed_resources.log
    ```
