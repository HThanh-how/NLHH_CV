# Nguyen Luong Huu Huy - Resume

LaTeX-based resume/CV repository with automated PDF generation using GitHub Actions.

## Overview

This repository contains a professional resume built with LaTeX that automatically compiles to PDF on every push to the main branch.

## Features

- **Professional LaTeX Design**: Clean, modern CV layout with proper typography
- **Automated Build**: GitHub Actions workflow automatically compiles PDF on push
- **Auto Release**: Creates GitHub releases with changelog from commit history
- **ATS-Friendly**: PDF is optimized for Applicant Tracking Systems

## File Structure

```
.
├── main.tex              # Main LaTeX source file
├── .github/
│   └── workflows/
│       └── latex.yml     # GitHub Actions workflow for building PDF
└── README.md             # This file
```

## Building Locally

### Prerequisites

- TeX Live distribution (full installation recommended)
- XeLaTeX compiler
- Required LaTeX packages:
  - fontawesome5
  - fontspec
  - geometry
  - titlesec
  - xcolor
  - enumitem
  - hyperref
  - paracol
  - And others (see main.tex for full list)

### Build Command

```bash
xelatex -shell-escape main.tex
```

Or using latexmk:

```bash
latexmk -xelatex -shell-escape main.tex
```

The output will be `main.pdf` which can be renamed to `Nguyen_Luong_Huu_Huy_Resume.pdf`.

## GitHub Actions Workflow

The workflow (`.github/workflows/latex.yml`) automatically:

1. **Builds PDF** on every push and pull request
2. **Uploads artifacts** (PDF and logs)
3. **Creates GitHub Release** (on push to main branch only)
4. **Generates changelog** from commits since last successful build

### Release Process

- Each successful build creates a release with tag `build-{run_number}`
- Changelog includes commits since the last successful build tag
- PDF is attached to the release

## Customization

To customize this resume:

1. Edit `main.tex` to update personal information, experience, skills, etc.
2. Modify colors in the preamble (currently using RGB `{0, 79, 144}`)
3. Adjust spacing and layout in the environment definitions

## License

This resume template is for personal use.

