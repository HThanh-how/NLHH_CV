# 🔧 GitHub Actions Workflow Documentation

This directory contains the GitHub Actions workflow configuration for automated LaTeX PDF compilation and release management.

## 📄 Files

- **`latex.yml`**: Main workflow configuration file

## 🎯 Workflow Overview

The workflow automatically builds English and bilingual LaTeX resume PDFs (both with photo and without photo versions), uploads artifacts, and creates GitHub releases with changelogs.

## 🔄 Workflow Jobs

### Job 1: `build-pdf`

**Trigger**: Runs on every push, pull request, and manual workflow dispatch

**Steps**:

1. **Checkout**: Clones the repository
2. **Diagnose**: Displays diagnostic information (branch, commit SHA, file listing)
3. **Compile LaTeX (XeLaTeX)**:
   - Uses `xu-cheng/latex-action@v3` for compilation
   - Compiles each configured matrix file using XeLaTeX:
     - `main.tex` (English, no photo)
     - `main-photo.tex` (English, with photo)
     - `cv-bilingual.tex` (Bilingual EN/VI, no photo)
     - `cv-bilingual-photo.tex` (Bilingual EN/VI, with photo)
   - Installs required packages: `fontawesome5`, `fontspec`, `lm`, `babel-english`, `etoolbox`, `libertinus`, `tex-gyre`
   - Enables shell escape for advanced features
4. **Publish PDF**: Copies generated PDFs into `dist/` with release-ready filenames:
   - `Nguyen_Luong_Huu_Huy_Resume_EN.pdf`
   - `Nguyen_Luong_Huu_Huy_Resume_EN_Photo.pdf`
   - `Nguyen_Luong_Huu_Huy_Resume_Bilingual.pdf`
   - `Nguyen_Luong_Huu_Huy_Resume_Bilingual_Photo.pdf`
5. **Upload Artifact (PDF)**: Uploads each PDF as a separate artifact
6. **Upload Logs**: Uploads compilation logs for each build (always, even on failure)

**Output**: 
- `resume-en-pdf`
- `resume-en-photo-pdf`
- `resume-bilingual-pdf`
- `resume-bilingual-photo-pdf`
- Corresponding build log artifacts

### Job 2: `release`

**Trigger**: Only runs on push to main branch (after `build-pdf` succeeds)

**Steps**:

1. **Checkout**: Clones repository with full history (`fetch-depth: 0`)
2. **Generate Changelog**: 
   - Finds the last successful build tag (pattern: `build-*`)
   - Generates changelog from commits since last build
   - Creates `release_notes.md` file
   - Falls back to recent commits if no previous build found
3. **Download Artifacts**: Downloads all 4 PDF artifacts from the `build-pdf` job into `dist/`
4. **Create GitHub Release**:
   - Creates release with tag: `build-{run_number}`
   - Uses generated changelog as release notes
   - Attaches all 4 PDFs to release

**Output**: GitHub Release with all 4 PDF attachments

## 🔍 Changelog Generation Logic

The changelog generation follows this logic:

1. **Find Last Build Tag**:
   ```bash
   git tag -l "build-*" | sed 's/build-//' | sort -n | tail -1
   ```
   - Searches for tags matching pattern `build-*`
   - Extracts build numbers and finds the highest one
   - This represents the last successful build

2. **Generate Changelog**:
   - If last build tag found: Lists commits from `{LAST_TAG}..HEAD`
   - If no tag found: Lists last 20 commits
   - Format: `*   **Commit Message** (commit-hash)`

3. **Fallback**:
   - If no commits found or file is empty, uses last 10 commits
   - Ensures changelog is never empty

## ⚙️ Configuration

### Required Packages

The workflow installs these packages via `tlmgr`:

```yaml
pre_compile: |
  tlmgr update --self
  tlmgr install fontawesome5 fontspec lm babel-english etoolbox libertinus tex-gyre
```

### LaTeX Compilation Settings

```yaml
root_file: ${{ matrix.root_file }}
latexmk_use_xelatex: true
latexmk_shell_escape: true
```

- **XeLaTeX**: Required for font support and Unicode
- **Shell Escape**: Required for some advanced LaTeX features

### Permissions

```yaml
permissions:
  contents: write  # Required to create releases
```

## 📊 Workflow Triggers

| Event | Condition | Runs Jobs |
|-------|-----------|-----------|
| `push` | Any branch except release condition | `build-pdf` |
| `push` | `main` branch | `build-pdf` + `release` |
| `pull_request` | Any PR | `build-pdf` only |
| `workflow_dispatch` | Manual trigger | `build-pdf` only |

## 💡 Tips

1. **Test Locally First**: Always test LaTeX compilation locally before pushing
2. **Check Logs**: Review compilation logs in artifacts for warnings
3. **Incremental Builds**: Workflow uses latexmk which handles multiple passes automatically
4. **Tag Management**: Build tags are created automatically, no manual management needed
