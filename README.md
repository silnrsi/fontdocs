# Fontdocs

This is a system for generating simple multi-platform documentation for font projects. It was specifically developed for SIL font projects that use the workflow documented at [SIL Font Development Notes (silfontdev)](https://silnrsi.github.io/silfontdev/en-US/index.html) but could provide ideas for other projects and workflows.

From Markdown source the system generates:

- HTML docs that use a simple template and webfonts
- PDF versions of the HTML docs
- Alternate Markdown used for SIL product web sites

It supports:

- OpenType font features using CSS webfonts
- Links to other documents and external sites, both in HTML and PDF versions

The design and functionality of the docs is intentionally basic. The visual appearance can be adjusted by modifying the HTML templates and CSS.

Most of the documentation is by example, using some files from the [Charis SIL](https://software.sil.org/charis) project.

## Requirements

The system runs on Linux or macOS and requires the following to be installed:

- [Pandoc](https://pandoc.org/)
- [Weasyprint](https://weasyprint.org/)
- [Python](https://www.python.org/) - version 3.6 or later
- [pysilfont](https://github.com/silnrsi/pysilfont)
- The [Roboto](https://fonts.google.com/specimen/Roboto) and [Roboto Mono](https://fonts.google.com/specimen/Roboto+Mono) font families - these seem to produce PDFs that match the HTML docs most closely

These tools are already installed in the [Linux-based virtual machine we use for font development](https://silnrsi.github.io/silfontdev/en-US/Setting_Up_Tools.html). Setting up Weasyprint on other platforms (such as macOS) can be troublesome but does work.

The system also requires both web (WOFF2) and desktop (TTF or OTF) versions of your fonts.

## Setup

To use this system for a font project:

- Install the required tools and fonts (unless you're using the silfontdev VM)
- Copy the `documentation` folder (or its contents) into your project
- Adjust the footer info in the `source/template.html` and `source/templatepdf.html` files
- Place WOFF2 versions of your fonts in a `web` folder (as is done in this repo with Charis SIL), then adjust the `/assets/css/webfonts.css` file:
    - To point to the WOFF2 fonts
    - To define the classes you will use in the markdown to refer to your fonts
- Adjust `assets/css/webfontsttf.css` to point to wherever the TTF or OTF versions of your fonts can be found (typically in a `/results` folder when building SIL font projects)
    - This repo contains the full Charis SIL 6.001 package in the `results` folder, however only the font themselves are needed
    - Do not include the Charis SIL fonts in your project - they are only there for the examples
- Create and edit your docs as individual Markdown files in the `source` folder, based on the format in provided examples
- Copy the `makedocs` shell script to the root of your project and adjust it to produce your docs (you will also need to make it executable)
- Run `makedocs` - the results will be in:
    - `documentation` (HTML)
    - `documentation/pdf` (PDF)
    - `documentation/source/producesite` (Alternative Markdown for SIL product sites)

## Authoring content

Individual pages are stored as markdown files in the `source` folder. Each file needs a YAML-style header with two required key-value pairs _title_ and 

header
version
footer
links
images
(see markdowntest)

## Options and Notes

Serif vs sensserif font - theme.css
Markdowntest -markdown flavour
Images
Product site shortcodes CSS classes

## License

MIT
Charis





This is only a playground for experimental development of font documentation processes. Please do not fork this project as it is not yet under an open license.



Copyright © 2021 SIL International.

