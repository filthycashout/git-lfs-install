# Git LFS Install Guide

Git Large File Storage (Git LFS) keeps large binary assets out of normal Git blobs while preserving versioned pointers in your repository. This guide installs Git LFS and shows how to enable it for a repository.

## 1. Install Git LFS

Install Git first if it is not already available, then choose your operating system.

### Windows

Using winget:

```powershell
winget install --id GitHub.GitLFS
```

If that package is unavailable, install Git LFS from the official releases page:
https://git-lfs.com/

### macOS

Using Homebrew:

```bash
brew install git-lfs
```

### Ubuntu / Debian

```bash
sudo apt update
sudo apt install git-lfs
```

If your distribution does not provide a current package, use the official installation instructions:
https://github.com/git-lfs/git-lfs/blob/main/INSTALLING.md

### Fedora

```bash
sudo dnf install git-lfs
```

### Arch Linux

```bash
sudo pacman -S git-lfs
```

## 2. Verify the installation

```bash
git --version
git lfs version
```

Then initialize Git LFS for your user account:

```bash
git lfs install
```

Run `git lfs install` once per user account (or again if you need to repair the Git hooks configuration).

## 3. Enable LFS in a repository

From the root of the repository:

```bash
git lfs track "*.psd"
git lfs track "*.zip"
```

Use patterns that match the large file types your project actually needs. Tracking rules are written to `.gitattributes`; commit that file:

```bash
git add .gitattributes
git commit -m "Configure Git LFS tracking"
```

Add and commit files normally after tracking is configured:

```bash
git add path/to/large-file.psd
git commit -m "Add large asset"
git push
```

Make sure the Git hosting provider and repository plan support the expected LFS storage and bandwidth usage.

## 4. Clone a repository that already uses LFS

```bash
git lfs install
git clone <repository-url>
cd <repository-directory>
git lfs pull
```

If you cloned without downloading the large objects, `git lfs pull` fetches the objects for the checked-out revision. To download LFS objects for a specific branch, check out that branch first.

## 5. Check tracked files and troubleshoot

List tracked LFS patterns:

```bash
git lfs track
```

Inspect LFS-managed files:

```bash
git lfs ls-files
git lfs status
```

Common fixes:

- **The command is not found:** reopen the terminal after installation, then verify that Git LFS is on your `PATH`.
- **Files show as pointer text:** run `git lfs pull` and confirm that you have access to the remote repository.
- **A large file was committed before tracking:** adding a pattern does not retroactively convert existing Git history. For a new file, track the pattern first, then re-add and commit the file. Rewriting existing history requires a deliberate migration such as `git lfs migrate` and can affect collaborators.
- **Push fails:** check repository permissions, LFS storage/bandwidth quotas, and the hosting provider's LFS support.

## Official references

- [Git LFS website](https://git-lfs.com/)
- [Git LFS documentation](https://github.com/git-lfs/git-lfs)
- [Installation instructions](https://github.com/git-lfs/git-lfs/blob/main/INSTALLING.md)
- [Git LFS migration](https://github.com/git-lfs/git-lfs/blob/main/docs/man/git-lfs-migrate.adoc)

This repository contains installation documentation; it does not bundle Git LFS binaries or install software automatically.
