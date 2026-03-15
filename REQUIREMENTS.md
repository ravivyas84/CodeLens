# Requirements

This repo is mostly Markdown guidance for a Codex skill, so the setup is light.

## What Is Required

- A Codex environment that can load and use local skills.
- `ripgrep` (`rg`) if you want to follow the scan commands in the skill docs as written.
- Node.js and `npm` if you want to run the generated VitePress docs site locally.

## What Is Recommended

- Homebrew on macOS for installing missing tools.
- Git for repository activity checks such as last commit date and contributor counts.

## Install Homebrew (macOS)

Check whether Homebrew is already installed:

```bash
brew --version
```

If that command fails, install Homebrew:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Verify:

```bash
brew --version
```

## Install Required Tools

Install `ripgrep`:

```bash
brew install ripgrep
```

Verify:

```bash
rg --version
```

Install Node.js and `npm`:

```bash
brew install node
```

Verify:

```bash
node --version
npm --version
```

## Install Recommended Tools

Install Git if needed:

```bash
brew install git
```

Verify:

```bash
git --version
```

## Quick Verification Checklist

Run these commands after setup:

```bash
brew --version
rg --version
node --version
npm --version
git --version
```

If all of them print versions, the local toolchain for this skill is in place.

## Notes

- If `rg` is missing, the skill can still be adapted to use `grep`, but the current docs
  assume `ripgrep`.
- If Node.js is missing, you can still use the skill to generate Markdown output, but you
  will not be able to preview the generated VitePress site locally.
