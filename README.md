# DataSoc Resources

This repository stores the resources that appear on the DataSoc website Resources page, acting as the source of truth for resource files and metadata.

The basic workflow is:

```text
Upload file into this repo in the appropriate folder
↓
GitHub Action updates resources.json
↓
User updates required fields in resources.json
↓
DataSoc website fetches resources.json
↓
Resource appears on the Resources page
```

---

## What this repo does

This repo is responsible for:

- storing resource files
- storing resource metadata in `resources.json`
- serving files through GitHub Pages
- automatically generating missing JSON entries for uploaded files
- automatically removing JSON entries when files are deleted

---

## Repo structure

```text
resources/
├── .github/
│   └── workflows/
│       └── gen-code.yml
├── scripts/
│   └── generate.mjs
├── files/
│   ├── careers/
│   └── education/
├── resources.json
├── index.html
└── README.md
```

---

## Supported file types

Currently supported:

```text
.pdf
.csv
.png
.jpg
.jpeg
```
---

## How to add a new file resource

### Step 1: Upload the file

Upload the file inside the `files/` folder.

For example:

```text
files/careers/resume-guide.pdf
files/education/sql-practice.csv
```

Use a folder that matches the type of resource.

---

### Step 2: Wait for GitHub Actions

After uploading, go to the **Actions** tab.

Wait until the workflow finishes and turns green.

**DO NOT** make more changes to `main` while the Action is running.

If `main` changes while the Action is running, the Action might fail when it tries to push the updated `resources.json`.

---

### Step 3: Edit `resources.json`

After the Action finishes, it should create a skeleton entry in `resources.json`.

Example generated entry:

```json
{
  "id": "careers-resume-guide",
  "title": "TODO: WRITE TITLE!",
  "type": "file",
  "category": "TODO: Insert Category Here",
  "description": "TODO: Add short description.",
  "url": "https://unsw-data-soc.github.io/resources/files/careers/resume-guide.pdf",
  "tags": [],
  "needsReview": true
}
```

Replace the relevant fields with proper details.

Example finished entry:

```json
{
  "id": "careers-resume-guide",
  "title": "Resume Guide",
  "type": "file",
  "category": "Careers",
  "description": "A short guide for writing a strong data science resume.",
  "url": "https://unsw-data-soc.github.io/resources/files/careers/resume-guide.pdf",
  "tags": ["resume", "careers"],
  "needsReview": false
}
```

---

## How file deletion works

If a file is deleted from the `files/` folder, the GitHub Action will remove the matching file entry from `resources.json`.

Example:

```text
Delete:
files/careers/resume-guide.pdf

Action removes matching entry from:
resources.json
```

This helps prevent broken links from appearing on the website.


---

## GitHub Pages

This repo uses GitHub Pages to serve resource files.

Base URL:

```text
https://unsw-data-soc.github.io/resources/
```

Example file URL:

```text
https://unsw-data-soc.github.io/resources/files/careers/resume-guide.pdf
```

GitHub Pages should be set up with:

```text
Source: Deploy from a branch
Branch: main
Folder: / root
```

---

## Website connection

The main DataSoc website should fetch:

```text
https://unsw-data-soc.github.io/resources/resources.json
```

This allows the website to display resources without storing the files directly in the main website repo.
