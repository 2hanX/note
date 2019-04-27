## Digital Forensics, Part 16 (Extracting EXIF Data from Image Files)

### EXIF Data [^1]

### Graphic Image Types [^2]

- PSD
- JPEG
- BMP
- TIFF
- PNG
- GIF
- BPG
- JFIF
- ...

### Install Exifreader （[Windows](http://www.takenet.or.jp/~ryuuji/minisoft/exifread/english/)）

### Extract EXIF from picture  file

- Make
- Model
- Datetime
- DateTimeOriginal
- ...

### Extracting GPS Data

- GPSLatitude
- GPSLongitude
- ...
- `20 20'9.85" N 110 17'10.11" E`
- `20° 20'9.85" N 110°17'10.11" E`

[Exif Website](https://exif.tuchong.com/)



[原文](https://null-byte.wonderhowto.com/how-to/hack-like-pro-digital-forensics-for-aspiring-hacker-part-16-extracting-exif-data-from-image-files-0170128/)

---

[^1]: 可交换图像文件格式（EXIF）是数码相机行业设定的标准，用于识别数字图像和声音文件的格式。此信息包括相机设置，时间，日期，快门速度，曝光，是否使用闪光灯，压缩，相机名称以及其他对在图像编辑软件中查看和编辑图像至关重要的信息。这些信息对法医调查员也很有用。最初是为JPG和JPEG文件格式开发的，其他一些格式也使用EXIF数据，但这些数据**不适用**于PNG和GIF图像文件类型
[^2]: 有许多应用程序可以从图形文件中提取此EXIF数据。几乎每个主要的取证套件（EnCase，FTK，Oxygen等）都内置了这种功能