# Agent Guidelines for AI-Books Project

This repository contains a LaTeX-based book project. The following guidelines help agents work effectively with this codebase.

## Project Structure

```
AI-Books/
├── Book/
│   ├── makefile           # Build automation
│   ├── install.sh        # Dependency installation
│   ├── README.md
│   ├── LICENSE
│   └── src/main/latex/
│       ├── main.tex              # Main document
│       ├── config.tex            # Configuration
│       ├── Bibliography.bib      # Bibliography
│       ├── Kapitel/              # Chapters
│       │   ├── 1_Einleitung.tex
│       │   ├── 2_Kapitel.tex
│       │   ├── 3_Schluss.tex
│       │   ├── Anhang.tex
│       │   └── KapitelSammlung.tex
│       └── Vortext/              # Front matter
│           ├── Titelseite.tex
│           ├── Vorwort.tex
│           ├── Abkürzungsverzeichnis.tex
│           └── Bibliographie.tex
```

## Build Commands

All commands run from the `Book/` directory:

```bash
# Build the PDF
make

# Clean build artifacts
make clean

# Clean everything including PDF
make cleanall

# Install TeX dependencies
make install-deps

# Manual build (alternative)
cd Book/build/aux && latexmk -pdf main.tex
```

**Dependencies required:**
- TeX Live (recommended) or MiKTeX
- latexmk
- biber
- rsync

## LaTeX Code Style Guidelines

### File Organization

- Use section comments with `---` separators:
  ```latex
  % -----------------------------------------------------------------------------
  % SECTION NAME
  % -----------------------------------------------------------------------------
  ```
- Start each chapter with a header comment:
  ```latex
  % =============================================================================
  % KAPITEL N: TITLE
  % =============================================================================
  ```

### Package Loading Order

1. `RequirePackage` for packages that must load early (in config.tex)
2. `usepackage` for packages that load later (in main.tex)
3. Load geometry, hyperref, and bibliography last

### Naming Conventions

- Commands: `\commandName` (camelCase)
- File names: `snake_case.tex`
- Chapter files: `N_Name.tex` (numbered prefix)

### Configuration (config.tex)

Personal data is defined at the top in section 1:
```latex
\newcommand{\myTitle}{Title}
\newcommand{\mySubtitle}{Subtitle}
\newcommand{\myName}{Author Name}
\newcommand{\myLocation}{City}
\newcommand{\myYear}{2025}
```

### Bibliography

- Uses biblatex with biber backend
- Style: authoryear
- Add entries to `Bibliography.bib`
- Reference with `\cite{key}` or `\textcite{key}`

### TODOs and Notes

- Use `\todox{inline note}` for inline todos
- Use `\todo{note}` for margin todos
- Comment out with `\setuptodonotes{disabled}` when done

### Mathematical Content

- Theorem environments: `defi`, `bsp`, `satz`
- Reference with `\autoref{defi:label}`
- Use `equation` environment for equations

### Code Listings

- Use `lstlisting` environment with `\lstinputlisting{Python}{file.py}`
- Define custom styles in config.tex

## Common Tasks

### Adding a New Chapter

1. Create file: `Kapitel/N_Name.tex`
2. Add header comment
3. Add to `KapitelSammlung.tex` with `\include{Kapitel/N_Name}`
4. Update chapter number in filename

### Adding Bibliography Entries

```bibtex
@book{key,
  author = {Author Name},
  title = {Book Title},
  year = {2025},
  publisher = {Publisher}
}
```

### Adding Acronyms

Use the `acronym` package and define in `Vortext/Abkürzungsverzeichnis.tex`:
```latex
\acro{AI}{Artificial Intelligence}
```
Reference with `\ac{AI}`.

## Error Handling

- **Missing package:** Run `make install-deps` or install manually
- **Build fails:** Check `*.log` files in `build/aux/`
- **Bibliography errors:** Clear cache (`make clean`) and rebuild
- **Unicode errors:** Ensure UTF-8 encoding (`\RequirePackage[utf8]{inputenc}`)

## Best Practices

1. Always run `make clean` before major changes
2. Test builds frequently during editing
3. Use version control for all .tex and .bib files
4. Keep the bibliography in sync with citations
5. Use semantic markup (e.g., `\section{}` not bold text)
6. Reference labels consistently: `fig:`, `tab:`, `eq:`, `defi:`, `chap:`
