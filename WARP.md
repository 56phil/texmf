# WARP.md

This file provides guidance to WARP (warp.dev) when working with code in this repository.

## Repository Purpose

This is a personal TeX User Home Directory (`TEXMFHOME`), containing custom LaTeX packages and style files. The repository follows the TDS (TeX Directory Structure) standard and is automatically searched by TeX Live for packages and styles.

## Directory Structure

```
tex/latex/
├── coffeestains/     # Third-party package: adds coffee stain decorations to documents
├── sagetex/          # Third-party package: embeds Sage mathematics into LaTeX
└── local/            # Custom local packages (maintained by user)
    ├── compiledstamp.sty      # Inserts compile timestamp from shell script
    └── datetime-fallback.sty  # Fallback datetime macros with weekday/Julian day support
```

## Architecture

### Package Organization
- **Third-party packages** (`coffeestains/`, `sagetex/`): External packages installed locally for development or when not available in main TeX distribution
- **Local packages** (`local/`): Custom `.sty` files created for personal use across multiple LaTeX projects

### Custom Packages

#### `compiledstamp.sty`
- Provides `\compiledstamp` command to insert compilation timestamp
- Expects generated timestamp from external shell script in `build/stamp.tex`
- Requires `\write18` (shell escape) to be enabled for automatic timestamp generation

#### `datetime-fallback.sty`
- Requires `datetime2` and `datetime2-calc` packages
- Provides fallback macros: `\DTMcurrentyear`, `\DTMcurrentmonth`, `\DTMcurrentday`, `\DTMcurrenthour`, `\DTMcurrentminute`, `\DTMcurrenttime`, `\DTMcurrentweekday`, `\DTMcurrentjulian`
- English language support for day names

## Working with LaTeX Packages

### Testing Package Changes
```bash
# Test compilation of a package (generates .log file)
xelatex tex/latex/local/PACKAGE.sty
```

Note: Direct compilation of `.sty` files will fail with "no legal \end found" - this is expected. Check the `.log` file for package loading errors.

### Updating the ls-R Database
After adding or removing packages, update the filename database:
```bash
mktexlsr ~/texmf
```

Or let TeX Live auto-detect changes (usually automatic with modern distributions).

### Verifying Package Installation
```bash
# Check if package is found by TeX
kpsewhich PACKAGE.sty

# Verify TEXMFHOME is set correctly
kpsewhich --var-value=TEXMFHOME
```

## Development Guidelines

### Adding New Local Packages
1. Create `.sty` file in `tex/latex/local/`
2. Use `\NeedsTeXFormat{LaTeX2e}` and `\ProvidesPackage{name}[date description]` headers
3. Test in a real document, not by compiling the `.sty` file directly
4. Commit both the `.sty` file and any generated `.log` files for reference

### Modifying Existing Packages
- Custom packages in `local/` can be freely modified
- Third-party packages (`coffeestains`, `sagetex`) should generally not be modified; document any local patches clearly

### Package Dependencies
When creating new packages that depend on other packages:
- Use `\RequirePackage{package}` rather than `\usepackage{package}` within `.sty` files
- Document all dependencies in comments at the top of the file
