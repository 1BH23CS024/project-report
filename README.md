# VeerDrishti — Major Project Phase-I Report

This repository contains the editable LaTeX source for the **VeerDrishti:
Combat Vision System** project report. It is a report template and Phase-I
submission document; it is not the implementation of the computer-vision
system.

The generated report is [`main.pdf`](main.pdf). Edit the source files, rebuild
the document, and inspect the new PDF. Do not edit the PDF directly.

## Quick build

The project uses **XeLaTeX**, **latexmk**, **Biber**, and the **Times New Roman**
font. From the directory containing `main.tex`:

```sh
latexmk -xelatex -interaction=nonstopmode -halt-on-error -file-line-error main.tex \
  && latexmk -c -bibtex main.tex
```

The successful build leaves `main.pdf` in the repository root. The cleanup
command removes intermediate files but keeps the PDF. To build in VS Code,
install the LaTeX Workshop extension, open the repository folder, and run the
configured **latexmk (XeLaTeX)** recipe.

## Files

```text
main.tex                         Report entry point and page order
report.sty                       Shared typography, headings and page commands
front_layout.tex                 Named front-page placements
sections/                        Cover, certificate, front matter and lists
chapters/                        Report chapters
images/                          Logos, letterhead and diagrams
references.bib                   Bibliography entries
Guidelines-for-Project-work- ... VTU 2022-scheme guidelines
main.pdf                         Generated report
```

The document order is controlled by the `\input{...}` lines in `main.tex`.

### Editing front pages

The front pages retain their measured college layout, but section files no
longer contain distracting coordinate groups. Use named placements:

```latex
\FrontLine{cover-university}{\textbf{VISVESVARAYA TECHNOLOGICAL UNIVERSITY}}
\FrontParagraph{certificate-body}{Your certificate text.}
```

The placement names and their measurements are kept in `front_layout.tex`.
Change wording in `sections/`; change a fixed position only when necessary in
`front_layout.tex`. This keeps the visual output consistent with the current
PDF while making the report content easier to read and edit.

For body content, edit the matching file in `chapters/`. Add figures to
`images/`, cite sources with their keys from `references.bib`, and rebuild
after every significant change.

## Minimal Linux setup

The current preparation machine uses a user-level TinyTeX installation and
the same command-line tools listed above. First check whether the tools and
font already exist:

```sh
command -v xelatex latexmk biber tlmgr
fc-match -f '%{family}\n' 'Times New Roman'
```

If these commands succeed and the font lookup reports **Times New Roman**, no
new installation is needed.

### Ubuntu or Debian

Install only the operating-system prerequisites:

```sh
sudo apt update
sudo apt install --no-install-recommends curl ca-certificates perl xz-utils fontconfig
```

### Arch Linux

```sh
sudo pacman -S --needed curl ca-certificates perl xz fontconfig
```

### Install the user-level TinyTeX toolchain

Run this only when a suitable TeX installation is not already present. The
installer uses `~/.TinyTeX`; do not run it over an installation you want to
keep.

```sh
mkdir -p "$HOME/.local/bin"
curl -fsSL https://tinytex.yihui.org/install-bin-unix.sh \
  -o /tmp/install-tinytex.sh
TINYTEX_INSTALLER=TinyTeX-0 sh /tmp/install-tinytex.sh
export PATH="$HOME/.local/bin:$PATH"

tlmgr install latex-bin xetex latexmk biber biblatex biblatex-ieee \
  fontspec lm geometry booktabs caption enumitem etoolbox fancyhdr \
  graphics tools pgfgantt refcount setspace pgf titlesec xcolor hyperref
tlmgr path add
```

Keep `~/.local/bin` in your shell `PATH` after restarting the terminal. Do not
mix `sudo tlmgr` with a distribution-managed TeX installation. Use the package
manager that owns the TeX installation.

### Times New Roman

The source explicitly selects Times New Roman and the repository does not
bundle Microsoft fonts. For the same line breaks and page layout as the
current PDF, install the regular, bold, italic, and bold-italic font files
from a lawful source in a user font directory:

```sh
mkdir -p "$HOME/.local/share/fonts/times-new-roman"
# Copy the four licensed Times New Roman .ttf files into that directory.
fc-cache -f "$HOME/.local/share/fonts/times-new-roman"
fc-match -f '%{family}\n' 'Times New Roman'
```

If an installed substitute is selected in `report.sty`, the report will still
build, but line breaks and page counts can change.

### Verify the repository

From the repository root:

```sh
latexmk -xelatex -interaction=nonstopmode -halt-on-error -file-line-error main.tex
pdfinfo main.pdf                 # optional: confirms pages and A4 size
pdffonts main.pdf                # optional: checks embedded fonts
latexmk -c -bibtex main.tex
```

If Biber on Arch reports `libcrypt.so.1` missing, install the compatibility
package and rebuild:

```sh
sudo pacman -S --needed libxcrypt-compat
```

## VTU guideline report

The supplied **Guidelines for Final Year Project Work — 2022 scheme** were
checked against the source. The current Phase-I report implements the relevant
report-preparation requirements as follows:

| Guideline | Current report |
|---|---|
| Paper and margins | A4; left 1.25 in, right 1 in, top and bottom 0.75 in |
| Spacing and body text | 1.5 line spacing; 12 pt justified body text |
| Heading sizes | Chapter label 16 pt, chapter title 18 pt, section 16 pt, subsection 14 pt |
| Front matter | Cover, certificate, declaration, acknowledgements, abstract, index, figure list and table list |
| Numbering | Roman preliminary pages and Arabic-numbered chapters |
| Figures and tables | Numbered by chapter and included in their lists |
| Abstract | 96 words, within the 100-word limit |
| References | Numbered in citation order through `biblatex` IEEE style |
| Report body | Introduction, problem statement, literature review, scope, methodology, requirements, design, schedule and conclusion |

The report is intentionally marked **Phase-I**. Implementation, measurements,
results, discussion, appendices, and any hardware/software deliverables should
be added only when they actually exist. Before printing or final submission,
confirm the exact certificate wording, title-page arrangement, officials,
academic year, signatures, copies, binding, appendices, and external-viva
requirements with the project guide and department. Successful compilation
does not replace that approval.

## Final check before submission

- Build `main.pdf` successfully and inspect every page visually.
- Check names, USNs, title, academic year, guide, officials and signatures.
- Keep the abstract at or below 100 words.
- Confirm that claims from papers are cited and are not presented as project measurements.
- Add implementation and evaluation evidence before calling the work complete.
- Obtain guide and department approval before printing.
