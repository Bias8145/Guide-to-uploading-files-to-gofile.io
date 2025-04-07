# GoFile Multi-File Uploader (Bash Script)

This is a simple Bash script for uploading one or more files to [GoFile.io](https://gofile.io). It's fast, easy to use, and runs directly from your terminal.

---

## How to Use

### 1. Goes to the Project Directory

---

### 2. Create the Script

Create a file named `upload.sh`:

```bash
nano upload.sh
```

Paste the following content into it:

```bash
#!/bin/bash

# GoFile multi-file uploader script
# Credit: Based on community GoFile CLI scripts with enhancements
# Usage: ./upload.sh file1 file2 file3 ...

if [ $# -eq 0 ]; then
  echo "Usage: ./upload.sh file1 [file2 file3 ...]"
  exit 1
fi

echo "============================================="
echo "🚀 GoFile Multi-File Uploader"
echo "============================================="

for FILE in "$@"; do
  if [ ! -f "$FILE" ]; then
    echo "❌ Error: File not found: $FILE"
    continue
  fi

  echo "📤 Uploading: $FILE"
  UPLOAD_RESPONSE=$(curl -s -F "file=@$FILE" https://upload.gofile.io/uploadFile)

  DOWNLOAD_URL=$(echo "$UPLOAD_RESPONSE" | grep -o '"downloadPage":"[^"]*' | cut -d'"' -f4)
  FILE_NAME=$(echo "$UPLOAD_RESPONSE" | grep -o '"name":"[^"]*' | cut -d'"' -f4)
  FILE_SIZE=$(echo "$UPLOAD_RESPONSE" | grep -o '"size":[^,}]*' | cut -d':' -f2)

  if command -v bc >/dev/null 2>&1; then
    FILE_SIZE_MB=$(echo "scale=2; $FILE_SIZE/1048576" | bc)
    SIZE_INFO="$FILE_SIZE_MB MB"
  else
    SIZE_INFO="$FILE_SIZE bytes"
  fi

  echo "✅ Upload successful!"
  echo "📁 File: $FILE_NAME"
  echo "📊 Size: $SIZE_INFO"
  echo "🔗 Download URL: $DOWNLOAD_URL"
  echo "---------------------------------------------"
done

echo "📝 All uploads completed!"
echo "============================================="
```

---

### 3. Make It Executable

```bash
chmod +x upload.sh
```

---

### 4. Run the Script

You can now use the script by passing one or more file paths as arguments:

```bash
./upload.sh /path/to/file1.zip /path/to/file2.img
```

Example:

```bash
./upload.sh sunfish/tpp/out/target/product/sunfish/out/target/product/sunfish/PixelProject_sunfish-2.0-unofficial-20250407-0455.zip sunfish/tpp/out/target/product/sunfish/boot.img
```

Berikut ini adalah bagian **`Sample Output`** yang siap kamu **copy-paste langsung ke `README.md` GitHub**, dalam format Markdown yang rapi:

---

## Sample Output

Here's an example of what you’ll see after uploading:

```
=============================================
🚀 GoFile Multi-File Uploader
=============================================
📤 Uploading: PixelProject_sunfish-2.0.zip
✅ Upload successful!
📁 File: PixelProject_sunfish-2.0.zip
📊 Size: 1.52 MB
🔗 Download URL: https://gofile.io/d/abc123xyz
---------------------------------------------
📤 Uploading: boot.img
✅ Upload successful!
📁 File: boot.img
📊 Size: 64.00 MB
🔗 Download URL: https://gofile.io/d/def456uvw
---------------------------------------------
📝 All uploads completed!
=============================================
```
---

### 5. Requirements

Make sure you have the following installed:

```bash
sudo apt install curl bc
```

- `curl`: Required for uploading
- `bc`: Optional, used for MB file size calculation

---

### License

Free to use and modify — credit is appreciated!
```

---

You can copy and paste that entire block as your GitHub README or documentation. Let me know if you want the Markdown file version (`README.md`) too.
