# Instructions for preparing GPTP-2016 chapters

The page provides instructions for producing latex chapters for the GPTP 2016 proceedings volume, as well as links to style files. **Please follow these very important directions closely.** This will help us _immensely_ and decrease copy editing time enough to make this project feasible.

Before you start, all instances of `Smith` in this documentation should be replaced with your lead author's family name (to be clear, `Smith` replaces `YourName` in the files we provide; `YourName` should be your lead author's family name, but for purposes of this documentation, we use `Smith` as an example). Capitalization in this tutorial is _purposeful_. `Smith` and `smith` are different! These step by step directions should guide you through the process without a hitch. Please follow them carefully.

**CAPITALIZATION MATTERS!**

Note that the instructions are given in terms of Linux commands. If you are using Mac OSX, the same commands will work, and you should use the [Terminal.app](https://en.wikipedia.org/wiki/Terminal_\(OS_X\)) utility for these commands. If you are using Windows, you should make the file name and within-file edits that we suggest below. We have added special instructions for windows in the final step. If you need help, please contact [Bill Tozier on the Workshop's Slack Team](https://gptp2016.slack.com/messages/@bill_tozier/).


1. [Fork this repository on GitHub](https://help.github.com/articles/fork-a-repo/) and then clone or download the forked copy with your own `git` account.

2. Use the following files in your local clone of tehe repository to compose your chapter:
  - `chapter.bib`: to create a chapter-specific bibliography file, including _all_ citations you use in your paper
  - `gpbook.tex`: do not edit this; it is the "book wrapper" around your chapter
  - `chapter.tex`: an example chapter with tips about new package for book. _Replace the contents of this file with the contents of your chapter, keeping what you need for formatting._ That is, change the words and pictures, but _type_ the words and pictures in the way this example chapter indicates.


3. As you work, rename files you edit:
  - `chapter.tex` should be renamed to the lead author's last name in all lower case. For example, a paper by John Smith and Donald Duck should be renamed `smith.tex`.
  - `chapter.bib` should contain all citations you use in your paper. You may want to download a copy of the [most recent `gp-bibliography.bib` file from Bill Langdon's servers](http://www.cs.bham.ac.uk/~wbl/biblio/gp-bibliography.html) as a reference, but please copy any citations from that file into your `chapter.bib` file. Rename `chapter.bib` to `smith.bib`, as with the `.tex` file.
  - within `chapter.tex`, make sure to change the last lines of the document from:
  
    ```latex
    \bibliographystyle{spbasic}
    \bibliography{gp-bibliography,chapter}
    ```
  to
    ```latex
    \bibliographystyle{spbasic}
    \bibliography{gp-bibliography,smith}
    ```
  This will ensure your non-gp-bibliography entries are included in your references.
  - to render your chapter as you edit it, gpbook.tex file, changing line 47 from `\input{chapter}` to `\input{smith}`

4. Do not use additional packages without contacting the editors and asking beforehand. The `usepackage` lines in `gpbook.tex` should provide everything you need, and will not crash the rendering of the entire book. Adding extra packages _may_ crash the rendering of the entire book, and is to be avoided. If your document crashes the rendering of the entire book, we will ask your colleagues to fix it for you.

5. To create a pdf of the finished chapter, use the following commands. The resulting pdf will include the bibliography but not the index. Make sure you provide the necessary information for both the bibliography and index in your chapter file! That is, be sure to use `\index{}` and `\citep{}` when composing `chapter.tex`!
  
  ```text
  latex gpbook
  bibtex gpbook
  latex gpbook
  latex gpbook
  dvips gpbook -o gpbook.ps
  ps2pdf -dEmbedAllFonts=true gpbook.ps gpbook.pdf
  ```

5. All figures must be in EPS format. Do not use pixel-based formats or unusual fonts. Include all files in your chapter folder.


## Notes

- Please put all your `tex`, `bib`, and `eps` files at one level in the directory with a name that is your primary author's or group's name.
- Avoid custom extensions.
- Do not use additional packages without permission.
- Use `\citep{}` for all citations of papers, use `\cite{}` for citations-as-a-noun. So for example
  > "While `\cite{bill:2018}` mentions this amazing thing, other authors do not talk about it often `\citep{brian:2019}`."
  
  And include all cited works in your own `bib` file!
- You may use the `gp-bibliography.bib` as a source for many GP papers, but you will need to add additional citations to your own `chapter.bib` file.
- For figures and table numbers, include a `\label{}` in the tags, and then refer to that figure or table with a `\ref{}`. This is explained in the `chapter.tex` example file, and is not complicated. Just do what it has already shown you how to do.
- Use `\index{}` for indexing, as explained in `chapter.tex`.
- If you have questions or problems, feel free to open a GitHub issue, or ask on the [Workshop's Slack channels](http://gptp2016.slack.com). 

