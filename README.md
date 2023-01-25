# Fontdocs

This is a system for generating simple multi-platform documentation for font projects. It was specifically developed for SIL font projects that use the workflow documented at [SIL Font Development Notes (silfontdev)](https://silnrsi.github.io/silfontdev/en-US/index.html) but could provide ideas for other projects and workflows.

From markdown source the system generates:

- HTML docs that use a simple template and webfonts
- PDF versions of the HTML docs
- Alternate markdown used for SIL product web sites

It supports OpenType font features using CSS webfonts.

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
- Create and edit your docs as individual markdown files in the `source` folder, based on the format in provided examples
- Prepare the `makedocs` shell script:
    - Add the following line to the .gitattributes file of your project: `makedocs text eol=lf`
    - Copy the `makedocs` shell script to the root of your project
    - Adjust it to produce your docs
    - Make the file executable by running: `chmod +x makedocs`
- Run `./makedocs` from the project root folder - the results will be in:
    - `documentation` (HTML)
    - `documentation/pdf` (PDF)
    - `documentation/source/productsite` (Alternative markdown for SIL product sites)

## Authoring content

Individual pages are stored as markdown files in the `source` folder. Each file needs a YAML-style header with two required key-value pairs: *title* and *fontversion*, as in:

```
---
title: Charis SIL - About
fontversion: 6.001
---
```

The markdown style is generic with some extensions. The `source/markdowntest.md` file provides examples of the supported syntax and details of special features:

- Links to other files and external sources that work in the HTML version (but not PDF)
- Images (with special alternate syntax for product sites)
- Webfonts
- OpenType features

Examples of many of these are provided in the example Charis SIL docs.

You will want to remove the Charis SIL examples, the `markdowntest.md` file, and any generated HTML/PDF/md versions of them from your final font package.

## SIL Product Sites usage

The system can create alternate-flavor markdown specifically for SIL font Product Sites. The content of these files can be copied and pasted directly into product site page text edit fields. There are a few important considerations:

Any page that displays the webfonts must have a special shortcode added that lists the fonts used on the page and references the fonts as uploaded to the server. It is enclosed in a CSS comment. For an example see `features.md`. These comments are processed by the `makepsmd.py` script and used to transform the markdown into what is expected by the product sites system.

```
<!-- PRODUCT SITE ONLY
[font id='charis' face='CharisSIL-Regular' italic='CharisSIL-Italic' bold='CharisSIL-Bold' bolditalic='CharisSIL-BoldItalic' size='150%']
-->
```

The `makepsmd.py` script will also transform any explicit font feature styling into a new class unique to that setting and add a supporting [font] shortcode at the end of the document. This is to work around issues with WordPress filtering of explicit styling.

Images on Product Site pages also require a special comment syntax that specifies the exact link for the image: (for an example see `design.md`)

```
![Charis SIL Sample - Precomposed Latin Diacritics](assets/images/CharisSILTypePage.png){.fullsize}
<!-- PRODUCT SITE IMAGE SRC http://software.sil.org/charis/wp-content/uploads/sites/14/2015/12/CharisSILTypePage.png -->
<figcaption>This is the caption</figcaption>
```

Finally, the class definitions in `webfonts.css` and references on individual pages must match the pattern expected by the Product Sites, where the [font] shortcode on pages refers to a general family name (e.g. 'charis') and the classes are defined with extensions to that id (e.g. '.charis-R', '.charis-BI').

## Further notes

### Text font

By default this system produces docs that use a generic system sans serif font for the main text. This allows most project webfonts to stand out better when used as an example on a page. This can be changed in `theme.css`. There is already a serif alternative line commented out for the <body> entity that can be used instead. To change the font used for PDF versions modify `themepdf.css`.

### PDF links

Unfortunately most links will not work in the PDF versions due to longstanding Weasyprint limitations. Links will still be styled as links (in blue) but will not have any link destination. Sorry - there's not much we can do to fix this. Exceptions are external links that are in templates, such as the footer. 

## License

This software is Copyright (c) 2021-2023 SIL International (http://www.sil.org) and released under the MIT license. Font software from the Charis SIL project is released under the SIL Open Font License, Version 1.1 (http://scripts.sil.org/OFL). (see [LICENSE](LICENSE) for details).

