# beheader - linux
Polyglot generator for media files.

### Dependencies
This project requires the [Bun JavaScript runtime](https://bun.sh/), and is built for Linux systems. You'll need `ffmpeg`, `ffprobe`, ImageMagick's `convert`, `zip`, and `unzip` in your `PATH`, as well as an executable [mp4edit](https://www.bento4.com/) binary in your working directory.

If you have Nix installed on your system, be it just `nix` on your favorite distribution, or you're running NixOS, you can (after cloning) use the `flake.nix` to automatically get all of the above dependencies.

### Usage
With all dependencies set up, you should be able to run:
```
$ bun run beheader.js <output> <image> <video|audio> [-options] [appendable...]
```

**Positional arguments:**
- `output` - Path of resulting polyglot file.
- `image` - Path of input image file.
- `video|audio` - Path of input video (or audio) file.
- `appendable` - Path(s) of files to append without parsing.

**Optional flags:**
- `-h` (or `--html`) `<path>` - Path to HTML document.
- `-p` (or `--pdf`) `<path>` - Path to PDF document.
- `-z` (or `--zip`) `<path>` -  Path to ZIP-like archive. Can be repeated to merge multiple ZIPs.
- `-e` (or `--extra`) `<path>` - Path to short (<200 byte) file to include near the header.
- `--help` - Print usage guide and exit.

**Technical notes:**
1. The merging process is not necessarily lossless. Video (or audio) gets re-encoded to MP4, images get converted to PNG (in an ICO container), HTML is coupled with a stylesheet, PDF offsets are adjusted, and ZIP archives get re-packed.
2. There are many other file formats that use the ZIP structure under the hood. Popular examples include JAR, APK, PPTX, DOCX, XLSX, and a few others. Note that "appendables" are inserted *before* any ZIPs.
3. The \`--extra\` data gets inserted at address 22. Input size is not regulated - exceeding ~200 bytes (or less!) may break other components.

The output file will be a polyglot of all of its inputs. On most systems, it will change behavior depending on its file extension:
- `.ico` displays the input image;
- `.mp4` plays the input video;
- `.html` shows the input webpage;
- `.pdf` opens the input PDF (if applicable);
- `.zip` extracts the input archive (if applicable).

Because of the several unholy beheadings that this script performs, some less tolerant (or less compliant) programs may fail early with errors about bad metadata or file type.
