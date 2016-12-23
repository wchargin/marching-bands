# marching-bands: a collection of puzzles

This repository contains some **Marching Bands puzzles,** like
[this one][sample puzzle], along with the
code required to typeset them as worksheets and as solutions.

  [sample puzzle]: http://blogs.wsj.com/puzzle/2016/07/22/marching-bands-saturday-puzzle-14/

## Building

Each `.tex` file in the top level directory is a build target. You can
run `latexmk --pdf` to build them all, or `pdflatex something.tex` to
build just one file. Multiple runs of `pdflatex` should not be needed.

Tested on TeX Live 2013.

## Organization

Most of the interesting code is in `marchingbands.sty`. This package
contains an environment to typeset a puzzle, either empty or with
arbitrary contents. It provides a `\puzzlerow` command for easily
typesetting the solution to a puzzle.

The puzzles in this document are all 6-band puzzles, but the code in
`marchingbands.sty` is configurable to draw puzzles of any size.

The file `puzzles.tex` contains some commands to nicely format a puzzle
and its clues. The `\puzzle` command emits two pages: one for the blank
puzzle with clues, and one for the solved puzzle (also with clues).

Each of the files in the `data/` directory, like `data/puzzle01.tex`,
will emit a single puzzle when `\input` into the main document.

## Source

The puzzles in the `data/` directory come from the Wall Street Journal's
online puzzle archive. Each puzzle's original source URL and author are
listed in a comment at the top of the puzzle data file.

## Formatting the data

To create the data files, I started by transcribing the files in a
lighter format than the final `puzzle??.tex` files: each clue was on its
own line, and each set of clues was separated by a blank line:

```tex
Vinegar variety originating in northern Italy
Resident in Central Italy

Blackouts, for example
Taiwan setting (2~wds.)
```

Then, I used some vim magic to convert these into the required format.
By recording the macro `vip>.gv:g/./norm O\item^Mvip,it}j` into `@q`,
I could place my cursor on the top of the first line and press `@q`
until all the blocks were converted:

```tex
\begin{itemize}
  \item
    Vinegar variety originating in northern Italy
  \item
    Resident in Central Italy
\end{itemize}

\begin{itemize}
  \item
    Blackouts, for example
  \item
    Taiwan setting (2~wds.)
\end{itemize}
```

(Note that this macro requires [LaTeX-suite][]; the visual-mode command
denoted by `,it` will enclose the selection in an `itemize`
environment.)

  [LaTeX-suite]: https://github.com/wchargin/vim-latexsuite

Then, I added a blank line at the top, selected the whole section, and
executed the macro `>gv:g/^$/norm S\item^M` to add the `\item`s before
each set of clues.

I repeated this process for the rows and bands sections of each puzzle.

For the puzzle solutions, I simply transcribed each solution,
capitalized it, and added `\puzzlerow ` to the front of each line and a
semicolon to the end.

## License

MIT
