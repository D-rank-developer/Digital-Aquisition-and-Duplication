# Digital Forensic Investigation Lab: FTK Imager, Autopsy, and Linux Imaging

![Platform](https://img.shields.io/badge/Platform-Windows%20%26%20Linux-blue)
![Domain](https://img.shields.io/badge/Domain-Digital%20Forensics-green)
![Tools](https://img.shields.io/badge/Tools-FTK%20Imager%20%7C%20Autopsy%20%7C%20dd%20%7C%20dcfldd-orange)
![Focus](https://img.shields.io/badge/Focus-Evidence%20Acquisition%20%26%20Analysis-red)

## Overview

This project documents a beginner-friendly **digital forensics workflow** using three core approaches:

- **FTK Imager** for forensic acquisition, preview, and evidence loading
- **Autopsy** for case-based forensic analysis, keyword searching, artifact review, tagging, and report generation
- **Linux command-line tools** such as `dd` and `dcfldd` for low-level forensic imaging and verification

The central forensic principle followed throughout this lab is simple:

> **Never analyse the original evidence directly.** Create a verified forensic image first, confirm its integrity with hashes, and perform analysis on the copy.

---

## What This Lab Demonstrates

This project walks through the core stages of a standard forensic workflow:

1. **Identify the evidence source**
2. **Protect it from modification**
3. **Create a forensic image**
4. **Verify the image using hashes**
5. **Load the image into a forensic analysis platform**
6. **Examine files, metadata, registry artifacts, and deleted content**
7. **Perform keyword and artifact-based searches**
8. **Tag relevant evidence**
9. **Generate an investigation report**

---

## Tools Used

### FTK Imager
Used to:
- Create forensic images
- Verify acquired images
- Load image files as evidence
- Preview files and metadata
- Mount images as virtual drives

### Autopsy
Used to:
- Create a case and add image evidence
- Run ingest modules
- Inspect partitions, files, and deleted content
- Analyse registry artifacts such as `NTUSER.DAT`
- Search for keywords, URLs, and suspicious items
- Tag evidence and generate reports

### Linux Forensic Tools
Used to:
- Identify the correct source device
- Enforce software read-only mode
- Acquire a bit-for-bit raw image
- Hash and verify evidence during acquisition
- Mount images read-only for safe review

Key commands and tools:
- `lsblk`
- `fdisk -l`
- `blockdev`
- `dd`
- `dcfldd`
- `stat`
- `file`
- `losetup`
- `mount`
- `umount`

---

## Forensic Concepts Applied

### Forensic Image
A forensic image is an **exact bit-for-bit copy** of a storage device. It may include:
- active files
- deleted files
- slack space
- unallocated space
- file system metadata

### Hash Verification
Hashing is used to prove that the forensic copy matches the original source.
Common hash values include:
- MD5
- SHA-1
- SHA-256

If the source and the image produce matching hashes, the copy can be treated as an accurate forensic duplicate.

### Read-Only Handling
Evidence must be protected from modification. This can be achieved using:
- hardware write blockers
- software read-only controls
- read-only mounting during analysis

---

## Project Structure Suggestion

Place the screenshots in an `images/` folder in your GitHub repository so the markdown renders correctly.

```text
.
├── README.md
└── images/
    ├── ftk-add-evidence-source.png
    ├── ftk-browse-family-pix.png
    ├── ftk-file-properties-mysister.png
    ├── autopsy-ntuser-dat-registry-view.png
    ├── autopsy-keyword-lists-urls.png
    ├── autopsy-suspicious-items.png
    └── ...
```

---

# Part 1: FTK Imager Lab — Data Acquisition and Forensic Imaging

## Purpose

The FTK Imager lab focuses on **forensic acquisition**. The goal is to safely create a copy of the source media so that later investigation can be performed on the image rather than the original device.

## Why This Matters

In digital forensics, browsing a suspect USB drive directly can alter timestamps or metadata. A forensic image preserves the source and supports repeatable, defensible analysis.

## Core FTK Imager Workflow

### 1. Create a forensic image
This involves selecting the evidence source, choosing an image format such as `E01`, entering case metadata, defining a destination path, and enabling verification.

### 2. Verify the image
Hash verification confirms that the created image matches the original source.

### 3. Load the image back as evidence
The image can then be opened in FTK Imager for preview and examination.

### 4. Browse folders and inspect file metadata
This allows the examiner to review file names, timestamps, and file properties without modifying the original evidence.

---

## FTK Imager Procedure Summary

### Step 1: Start acquisition
- Open **FTK Imager** as Administrator
- Go to **File > Create Disk Image**
- Select either **Physical Drive** or **Contents of a Folder** depending on the lab route
- Choose the correct evidence source carefully

### Step 2: Choose the image format
- Select **E01** for a structured forensic image format
- Enter case name, examiner name, and evidence number
- Choose the image save location
- Enable **Verify images after creation**

### Step 3: Save logs and verify integrity
- Review image size, hash values, and summary output
- Save the text log for documentation and evidence tracking

### Step 4: Load the image for analysis
- Go to **File > Add Evidence Item > Image File**
- Browse to the `.E01` image
- Expand the evidence tree and navigate folders inside the mounted image

---

## FTK Imager Screenshots

### Adding the forensic image as evidence
This step loads the previously acquired image back into FTK Imager for preview and structured examination.

![FTK add evidence source](https://github.com/D-rank-developer/Digital-Aquisition-and-Duplication/blob/7d84d5ca2e52c0203de5f36b9d8dc80ffcc3e1d7/image%20assets/image1.png)

### Browsing the image contents
The evidence tree shows the `Thumbdrive.E01` image, the FAT32 volume, and folders such as `Family Pix`. This demonstrates how the forensic image can be explored without touching the original USB device.

![FTK browse Family Pix](https://github.com/D-rank-developer/Digital-Aquisition-and-Duplication/blob/7d84d5ca2e52c0203de5f36b9d8dc80ffcc3e1d7/image%20assets/image2.png)

### Reviewing a file inside the image
Here, `My Sister.jpg` is selected and previewed inside FTK Imager. This is useful for beginner-level evidence inspection and media review.

![FTK preview file](https://github.com/D-rank-developer/Digital-Aquisition-and-Duplication/blob/7d84d5ca2e52c0203de5f36b9d8dc80ffcc3e1d7/image%20assets/image3.png)

### Inspecting file properties
The properties window shows file metadata such as size, dimensions, and created/modified dates. This is an example of extracting file-level evidence from an acquired image.

![FTK file properties](https://github.com/D-rank-developer/Digital-Aquisition-and-Duplication/blob/7d84d5ca2e52c0203de5f36b9d8dc80ffcc3e1d7/image%20assets/image4.png)

---

# Part 2: Autopsy Lab — Forensic Analysis of the Image

## Purpose

Autopsy is used after acquisition to perform structured forensic analysis. Instead of creating the image, Autopsy helps the examiner:

- create a case
- add evidence
- run ingest modules
- inspect partitions and files
- identify suspicious material
- review artifacts and registry data
- generate reports

## Why This Matters

Autopsy turns a raw forensic image into an organised investigation workspace. It helps beginners understand how evidence is grouped by file type, artifact category, report output, and search results.

---

## Autopsy Procedure Summary

### 1. Create a case
- Create a new case named `NTFS_CAPTURE`
- Enter the examiner ID and case number
- Set the evidence image path
- Use the correct timezone (`Europe/London` in this lab)

### 2. Add the image as a data source
- Select the `.E01` image
- Leave ingest modules enabled for full processing
- Allow indexing and artifact generation to complete

### 3. Examine the data source
- Expand the NTFS volume and unallocated space
- Review files by extension, MIME type, and deleted status
- Locate significant files such as `NTUSER.DAT`
- Use thumbnail, text, and hex views where appropriate

### 4. Search and interpret evidence
- Use keyword lists such as URLs
- Search for file names or activity artifacts
- Review suspicious items and generated reports

### 5. Tag evidence and generate a report
- Tag relevant images or documents as bookmarks
- Add comments where needed
- Generate an HTML report based on tagged results

---

## Autopsy Evidence Review

### `NTUSER.DAT` examination
`NTUSER.DAT` is a Windows user hive that can reveal user preferences, software settings, and traces of user activity. In the lab, this file is highlighted because it is a valuable artifact during Windows investigations.

![Autopsy NTUSER.DAT registry view](https://github.com/D-rank-developer/Digital-Aquisition-and-Duplication/blob/7d84d5ca2e52c0203de5f36b9d8dc80ffcc3e1d7/image%20assets/Screenshot%20(185).png)

### Report generated from `NTUSER.DAT`
Autopsy also shows generated outputs linked to modules such as `RecentActivity`, including report paths related to `RegRipper` output for `NTUSER.DAT`.

![Autopsy reports pane RegRipper](https://github.com/D-rank-developer/Digital-Aquisition-and-Duplication/blob/7d84d5ca2e52c0203de5f36b9d8dc80ffcc3e1d7/image%20assets/Screenshot%20(184).png)

---

## File Type and Media Analysis

### Images grouped by extension
Autopsy can group files by extension, making it easy to review media artifacts without manually browsing every folder path.

![Autopsy images tagged view](https://github.com/D-rank-developer/Digital-Aquisition-and-Duplication/blob/7d84d5ca2e52c0203de5f36b9d8dc80ffcc3e1d7/image%20assets/Screenshot%20(186).png)

### Thumbnail review of image evidence
Thumbnail view helps quickly identify visual evidence and spot relevant media content.

![Autopsy images thumbnail view]()

### Additional thumbnail review with selected image set
This screenshot further demonstrates how Autopsy supports visual inspection of image files discovered inside the forensic image.

![Autopsy images thumbnail view selected](https://github.com/D-rank-developer/Digital-Aquisition-and-Duplication/blob/7d84d5ca2e52c0203de5f36b9d8dc80ffcc3e1d7/image%20assets/Screenshot%20(187).png)

---

## Keyword and Artifact Search

### Keyword list selection
Autopsy includes predefined keyword list categories such as:
- phone numbers
- IP addresses
- email addresses
- URLs
- credit card numbers

In this case, the **URLs** list is selected to identify web-related activity.

![Autopsy keyword lists URLs](images/autopsy-keyword-lists-urls.png)

### Keyword search targeting `NTUSER.dat`
This screenshot shows a search configured for `NTUSER.dat`, demonstrating how Autopsy can be used to locate specific high-value artifacts by name or pattern.

![Autopsy keyword search NTUSER](https://github.com/D-rank-developer/Digital-Aquisition-and-Duplication/blob/7d84d5ca2e52c0203de5f36b9d8dc80ffcc3e1d7/image%20assets/Screenshot%20(183).png)

### Suspicious items identified by Autopsy
Autopsy highlights suspicious items based on analysis results, hit patterns, and indexed artifacts. This does not automatically mean malicious activity, but it gives the investigator leads to review.

![Autopsy suspicious items](images/autopsy-suspicious-items.png)

---

## Report Generation in Autopsy

### Choosing tagged results for reporting
The report wizard allows the examiner to export either all results or only tagged results. In this project, the report focuses on **tagged evidence** for a cleaner investigative summary.

![Autopsy generate report tagged results](https://github.com/D-rank-developer/Digital-Aquisition-and-Duplication/blob/7d84d5ca2e52c0203de5f36b9d8dc80ffcc3e1d7/image%20assets/imageeee.png)

### Selecting HTML report output
The HTML module is selected so the report can be viewed easily in a browser and shared as part of the lab deliverable.

![Autopsy report module HTML]()

### Report generation in progress
Autopsy generates thumbnails and compiles tagged artifacts into the final report output.

![Autopsy report generation progress](https://github.com/D-rank-developer/Digital-Aquisition-and-Duplication/blob/7d84d5ca2e52c0203de5f36b9d8dc80ffcc3e1d7/image%20assets/image6.png)

### Generated HTML forensic report
The final HTML report includes case details, image information, and tagged evidence selected during the investigation.

![Autopsy HTML report output](https://github.com/D-rank-developer/Digital-Aquisition-and-Duplication/blob/7d84d5ca2e52c0203de5f36b9d8dc80ffcc3e1d7/image%20assets/image9.png)

---

# Part 3: Linux Imaging with `dd` and `dcfldd`

## Purpose

The Linux section introduces command-line forensic imaging. This is important because forensic analysts should understand imaging at a lower level, not only through GUI tools.

## Step 1: Identify the source device
Use the following commands to identify the correct disk before imaging:

```bash
sudo lsblk -o NAME,MODEL,SIZE,TYPE,MOUNTPOINT
sudo fdisk -l
```

This helps confirm which device is the evidence source, for example `/dev/sdb`, and avoids imaging the wrong disk.

## Step 2: Set read-only mode

```bash
sudo blockdev --setro /dev/sdb
sudo blockdev --getro /dev/sdb
```

If the second command returns `1`, the device is set to read-only mode.

## Step 3: Create a raw forensic image using `dd`

```bash
sudo dd if=/dev/sdb of=usb_image.dd bs=4M conv=noerror,sync status=progress
sync
```

### Explanation
- `if=/dev/sdb` → input device
- `of=usb_image.dd` → output image file
- `bs=4M` → block size
- `conv=noerror,sync` → continue on errors and maintain alignment
- `status=progress` → show progress during acquisition

## Step 4: Record image metadata

```bash
stat usb_image.dd
file usb_image.dd
fdisk -l usb_image.dd
```

These commands help confirm the image file exists, identify its type, and review its partition information.

## Step 5: Mount the image read-only

```bash
sudo losetup -fP -r --show usb_image.dd
lsblk /dev/loop0
sudo mkdir -p /mnt/usb_img
sudo mount -o ro /dev/loop0p1 /mnt/usb_img
ls /mnt/usb_img
```

To clean up afterward:

```bash
sudo umount /mnt/usb_img
sudo losetup -d /dev/loop0
```

---

## Imaging with `dcfldd`

`dcfldd` is a forensic-enhanced version of `dd` that adds:
- hashing
- logging
- verification
- better forensic workflow support

### Example acquisition with SHA-256 hashing

```bash
sudo dcfldd if=/dev/sdb of=evidence.dd bs=4M conv=noerror,sync hash=sha256 hashlog=evidence_hash.txt statusinterval=30
```

### Example verification workflow

```bash
sudo dcfldd if=/dev/sdb of=evidence.dd vf=yes hash=sha256 hashlog=verify_log.txt
```

This makes `dcfldd` more suitable for forensic acquisition where integrity documentation is required as part of the imaging process.

---

# Key Findings and Learning Outcomes

This lab reinforced several core forensic principles:

- A forensic examiner should **never work directly on the original evidence**
- Imaging must be **repeatable, verifiable, and documented**
- Hash verification is essential to prove evidence integrity
- Registry artifacts such as `NTUSER.DAT` can reveal important traces of user activity
- Thumbnail, keyword, and suspicious-item views help prioritise evidence review
- Tagged evidence makes final reporting cleaner and more defensible
- GUI tools and command-line tools both play important roles in real forensic workflows

---

# Why This Project Matters for a Cybersecurity Portfolio

This project demonstrates hands-on understanding of:

- digital evidence handling
- forensic imaging principles
- chain-of-custody-aware workflow
- Windows artifact review
- basic media and metadata examination
- keyword-based evidence discovery
- investigative report generation
- command-line forensic acquisition in Linux

It is relevant for roles in:
- Digital Forensics
- DFIR
- SOC Analysis
- Incident Response
- Cybersecurity Investigation

---

# Notes

- This project is based on a controlled educational lab environment.
- The screenshots show forensic training workflows rather than a live criminal investigation.
- For a public GitHub portfolio, place all images in the repository under `images/` so the markdown renders correctly.

---

# Optional GitHub Commit Structure

```bash
git init
git add README.md images/
git commit -m "Add digital forensics investigation lab portfolio project"
git branch -M main
git remote add origin <your-repo-url>
git push -u origin main
```

