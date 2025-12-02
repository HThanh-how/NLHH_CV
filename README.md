# 📄 Nguyen Luong Huu Huy - Resume

<div align="center">

![LaTeX](https://img.shields.io/badge/LaTeX-47A141?style=for-the-badge&logo=LaTeX&logoColor=white)
![XeLaTeX](https://img.shields.io/badge/XeLaTeX-008080?style=for-the-badge)
![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-2088FF?style=for-the-badge&logo=github-actions&logoColor=white)
![PDF](https://img.shields.io/badge/PDF-FF0000?style=for-the-badge&logo=adobe-acrobat-reader&logoColor=white)

**Professional LaTeX-based resume with automated PDF generation**

[📥 Download Latest PDF](https://github.com/HThanh-how/NLHH_CV/releases/latest) • [🔧 Workflow Details](.github/workflows/README.md)

</div>

---

## ✨ Features

- 🎨 **Professional Design**: Clean, modern CV layout with proper typography and color scheme
- 🤖 **Automated Build**: GitHub Actions workflow automatically compiles PDF on every push
- 📦 **Auto Release**: Creates GitHub releases with changelog from commit history
- 🔍 **ATS-Friendly**: PDF is optimized for Applicant Tracking Systems (machine-readable)
- 📱 **Responsive Layout**: Two-column layout with proper spacing and formatting
- 🔗 **Clickable Links**: Hyperlinks for email, LinkedIn, and GitHub profiles
- 🎯 **Icon Support**: FontAwesome5 icons for contact information

## 📁 Repository Structure

```
NLHH/
├── main.tex                    # Main LaTeX source file
├── .github/
│   └── workflows/
│       ├── latex.yml          # GitHub Actions workflow configuration
│       └── README.md          # Workflow documentation
├── README.md                   # This file
└── Nguyen_Luong_Huu_Huy_Resume.pdf  # Generated PDF (after build)
```

## 🚀 Quick Start

### Prerequisites

Before building locally, ensure you have:

- **TeX Live** distribution (full installation recommended)
  - Windows: [MiKTeX](https://miktex.org/) or [TeX Live](https://www.tug.org/texlive/)
  - Linux: `sudo apt-get install texlive-full`
  - macOS: `brew install --cask mactex`
- **XeLaTeX** compiler (included in TeX Live)
- **Required LaTeX packages** (automatically installed by workflow):
  - `fontawesome5` - Icons
  - `fontspec` - Font handling for XeLaTeX
  - `geometry` - Page layout
  - `titlesec` - Section formatting
  - `xcolor` - Colors
  - `enumitem` - List customization
  - `hyperref` - Hyperlinks
  - `paracol` - Multi-column layout
  - And more (see `main.tex` for complete list)

### Building Locally

#### Option 1: Using XeLaTeX directly

```bash
xelatex -shell-escape main.tex
```

#### Option 2: Using latexmk (recommended)

```bash
latexmk -xelatex -shell-escape main.tex
```

#### Option 3: Using Docker (if available)

```bash
docker run --rm -v "$PWD":/workdir xu-cheng/latex-action:latest \
  latexmk -xelatex -shell-escape main.tex
```

**Output**: The compiled PDF will be `main.pdf`, which can be renamed to `Nguyen_Luong_Huu_Huy_Resume.pdf`.

### Building Multiple Times

LaTeX often requires multiple passes for references and table of contents:

```bash
latexmk -xelatex -shell-escape -pdf main.tex
```

This automatically runs the necessary number of passes.

## 🔄 GitHub Actions Workflow

The repository includes an automated CI/CD pipeline that:

1. ✅ **Builds PDF** on every push and pull request
2. 📤 **Uploads artifacts** (PDF and compilation logs)
3. 🏷️ **Creates GitHub Release** (on push to main branch only)
4. 📝 **Generates changelog** from commits since last successful build

### Workflow Details

For detailed information about the workflow, see [`.github/workflows/README.md`](.github/workflows/README.md).

### Release Process

- Each successful build creates a release with tag `build-{run_number}`
- Changelog includes commits since the last successful build tag
- PDF is automatically attached to the release
- Artifacts are available for download from the Actions tab

### Viewing Build Status

Check the latest build status:
- Go to the **Actions** tab in GitHub
- View workflow runs and download artifacts
- Access the latest PDF from the **Releases** page

## 🎨 Customization Guide

### Updating Personal Information

Edit the header section in `main.tex`:

```latex
\begin{header}
    \textbf{\fontsize{24pt}{24pt}\selectfont YOUR NAME}
    % ... contact information
\end{header}
```

### Changing Colors

Modify the primary color in the preamble:

```latex
\definecolor{primaryColor}{RGB}{0, 79, 144}  % Change RGB values
```

### Adjusting Spacing

Modify the `highlights` environment:

```latex
\newenvironment{highlights}{
    \begin{itemize}[
        topsep=0.15cm,    % Space before list
        itemsep=0.10cm,   % Space between items
        % ... other options
    ]
}
```

### Adding Sections

Follow the existing section structure:

```latex
\section{SECTION NAME}
\begin{onecolentry}
    % Content here
\end{onecolentry}
```

## 📋 Resume Sections

The current resume includes:

- **Objective**: Career goals and interests
- **Education**: Academic background and coursework
- **Honors & Awards**: Academic achievements
- **Certifications**: Professional certifications
- **Skills**: Technical skills and competencies
- **Work Experience**: Professional experience and projects
- **Interests**: Personal interests and hobbies

## 🛠️ Troubleshooting

### Common Issues

**Issue**: Package not found
```
Solution: Install missing package using tlmgr install <package-name>
```

**Issue**: Font not found (XeLaTeX)
```
Solution: Ensure fonts are installed system-wide or use system fonts
```

**Issue**: Build fails in GitHub Actions
```
Solution: Check workflow logs in Actions tab, ensure all packages are listed in pre_compile
```

**Issue**: Hyperlinks not working
```
Solution: Ensure hyperref package is loaded and PDF is compiled with proper settings
```

### Getting Help

- Check the [workflow documentation](.github/workflows/README.md)
- Review GitHub Actions logs for build errors
- Verify all required packages are installed locally

## 📊 Build Statistics

- **Compilation Time**: ~30-60 seconds (GitHub Actions)
- **PDF Size**: ~50-100 KB (optimized)
- **Pages**: 1-2 pages (depending on content)
- **Format**: PDF/A compatible

## 🔐 Privacy & Security

- Personal information in this resume is public
- Contact information can be modified before sharing
- GitHub Actions logs may contain compilation details

## 📝 License

This resume template is for personal use. Feel free to fork and customize for your own resume.

## 🙏 Acknowledgments

- LaTeX community for excellent packages
- [xu-cheng/latex-action](https://github.com/xu-cheng/latex-action) for GitHub Actions workflow
- FontAwesome for icons

---

<div align="center">

**Made with ❤️ using LaTeX**

[⬆ Back to Top](#-nguyen-luong-huu-huy---resume)

</div>
