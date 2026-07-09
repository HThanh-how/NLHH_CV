# 🔧 GitHub Actions Workflow Documentation

This directory contains the GitHub Actions workflow configuration for automated LaTeX PDF compilation and release management.

## 📄 Files

- **`latex.yml`**: Main workflow configuration file

## 🎯 Workflow Overview

The workflow automatically builds the English and bilingual LaTeX resume PDFs, uploads artifacts, and creates GitHub releases with changelogs.

## 🔄 Workflow Jobs

### Job 1: `build-pdf`

**Trigger**: Runs on every push, pull request, and manual workflow dispatch

**Steps**:

1. **Checkout**: Clones the repository
2. **Diagnose**: Displays diagnostic information (branch, commit SHA, file listing)
3. **Compile LaTeX (XeLaTeX)**:
   - Uses `xu-cheng/latex-action@v3` for compilation
   - Compiles each configured matrix file using XeLaTeX:
     - `main.tex`
     - `cv-bilingual.tex`
   - Installs required packages: `fontawesome5`, `fontspec`, `lm`, `babel-english`, `etoolbox`
   - Installs `fonts-dejavu-core` for Vietnamese glyph support in the bilingual CV
   - Enables shell escape for advanced features
4. **Publish PDF**: Renames generated PDFs to release-ready filenames
5. **Upload Artifact (PDF)**: Uploads each PDF as a separate artifact
6. **Upload Logs**: Uploads compilation logs for each build (always, even on failure)

**Output**: 
- English PDF artifact: `resume-en-pdf`
- Bilingual PDF artifact: `resume-bilingual-pdf`
- Logs artifacts: `resume-en-pdf-logs`, `resume-bilingual-pdf-logs`

### Job 2: `release`

**Trigger**: Only runs on push to main branch (after `build-pdf` succeeds)

**Steps**:

1. **Checkout**: Clones repository with full history (`fetch-depth: 0`)
2. **Generate Changelog**: 
   - Finds the last successful build tag (pattern: `build-*`)
   - Generates changelog from commits since last build
   - Creates `release_notes.md` file
   - Falls back to recent commits if no previous build found
3. **Download Artifacts**: Downloads both PDF artifacts from the `build-pdf` job
4. **Create GitHub Release**:
   - Creates release with tag: `build-{run_number}`
   - Uses generated changelog as release notes
   - Attaches both PDFs to release

**Output**: GitHub Release with English and bilingual PDF attachments

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
  tlmgr install fontawesome5 fontspec lm babel-english etoolbox
```

The workflow also installs the Ubuntu package `fonts-dejavu-core` for Unicode Vietnamese rendering.

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

## 🐛 Troubleshooting

### Build Fails

**Check**:
1. View logs in Actions tab
2. Check for missing LaTeX packages
3. Verify `main.tex` syntax is correct
4. Ensure all required packages are listed in `pre_compile`

**Common Issues**:
- Missing package: Add to `pre_compile` section
- Syntax error: Check LaTeX compilation locally first
- Font issues: Ensure fonts are available in TeX Live

### Release Not Created

**Possible Causes**:
1. Build job failed (release only runs on success)
2. Not pushing to `main` branch
3. Insufficient permissions (check repository settings)

### Changelog Empty

**Check**:
1. Verify tags exist: `git tag -l "build-*"`
2. Check if commits exist between last tag and HEAD
3. Review changelog generation script in workflow

### Artifact Not Found

**Check**:
1. Ensure `build-pdf` job completed successfully
2. Verify artifact names match: `resume-en-pdf` and `resume-bilingual-pdf`
3. Check artifact retention settings (default: 90 days)

## 🔐 Security Considerations

- **Shell Escape**: Enabled for LaTeX features, but be cautious with untrusted content
- **Permissions**: Only `contents: write` is needed (for releases)
- **Secrets**: No secrets required for this workflow

## 📈 Performance

- **Average Build Time**: 30-60 seconds
- **Artifact Size**: ~50-100 KB (PDF) + logs
- **Release Creation**: ~5-10 seconds

## 🔄 Workflow Updates

To modify the workflow:

1. Edit `.github/workflows/latex.yml`
2. Test locally if possible
3. Push changes to trigger workflow
4. Monitor Actions tab for results

## 📚 Related Documentation

- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [xu-cheng/latex-action](https://github.com/xu-cheng/latex-action)
- [LaTeX Documentation](https://www.latex-project.org/help/documentation/)

## 💡 Tips

1. **Test Locally First**: Always test LaTeX compilation locally before pushing
2. **Check Logs**: Review compilation logs in artifacts for warnings
3. **Incremental Builds**: Workflow uses latexmk which handles multiple passes automatically
4. **Tag Management**: Build tags are created automatically, no manual management needed

---

**Last Updated**: See workflow file for latest configuration

