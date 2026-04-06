# LaTeX Template for Mathematical Proof Documents

This template is designed for formalizing algorithm proofs with French theorem naming conventions.

## Usage Instructions

1. Copy the code block below into a `.tex` file
2. Fill in the `\title{}`, `\author{}`, and `\date{}` fields
3. Replace placeholder content in each section with your mathematical content
4. Compile with `pdflatex` (run twice for references):
   ```bash
   pdflatex -interaction=nonstopmode document.tex
   pdflatex -interaction=nonstopmode document.tex
   ```

## Package Compatibility Notes

- `amsmath`, `amsthm`, and `amssymb` are fully compatible and should always be used together.
- `listings` is used for Python code display.
- `tikz` is independent and has no known conflicts with the other packages.

## Theorem Environments

- `theorem` — numbered independently, labeled "Théorème"
- `lemma` — shares counter with theorem, labeled "Lemme"
- `definition` — numbered independently, labeled "Définition"

## Python Listing Style

The `listings` package is pre-configured for Python syntax highlighting. Use:
```latex
\begin{lstlisting}[language=Python]
def example():
    pass
\end{lstlisting}
```

---

```latex
\documentclass[11pt, a4paper]{article}

% ============================================================
% Required Packages
% ============================================================
\usepackage{amsmath}      % Advanced math typesetting
\usepackage{amsthm}       % Theorem environments
\usepackage{amssymb}      % Additional math symbols
\usepackage{listings}     % Source code listings
\usepackage{tikz}         % Diagrams and graphics

% ============================================================
% Theorem Environments (French naming)
% ============================================================
\newtheorem{theorem}{Théorème}
\newtheorem{lemma}[theorem]{Lemme}
\newtheorem{definition}{Définition}

% ============================================================
% Python Listing Style
% ============================================================
\lstset{
    language=Python,
    basicstyle=\ttfamily\small,
    keywordstyle=\color{blue}\bfseries,
    stringstyle=\color{red},
    commentstyle=\color{green!60!black},
    numbers=left,
    numberstyle=\tiny\color{gray},
    stepnumber=1,
    numbersep=8pt,
    showstringspaces=false,
    breaklines=true,
    frame=single,
    tabsize=4,
    captionpos=b,
    escapeinside={(*@}{@*)}
}

% ============================================================
% Document Metadata
% ============================================================
\title{Titre du Document}
\author{Auteur}
\date{\today}

\begin{document}

\maketitle

% ============================================================
% Abstract
% ============================================================
\begin{abstract}
% Résumé du document et des résultats principaux.
\end{abstract}

% ============================================================
% Algorithm Description
% ============================================================
\section{Algorithme}

% Décrivez l'algorithme ici.
% Utilisez un environnement verbatim ou lstlisting pour le pseudocode :
%
% \begin{lstlisting}[caption={Nom de l'algorithme}]
% Entrée: ...
% Sortie: ...
% \end{lstlisting}

% ============================================================
% Definitions
% ============================================================
\section{Définitions}

\begin{definition}
% Première définition formelle.
\end{definition}

% ============================================================
% Theorems
% ============================================================
\section{Théorèmes}

\begin{theorem}
% Énoncé du théorème principal.
\end{theorem}

\begin{lemma}
% Lemme auxiliaire.
\end{lemma}

% ============================================================
% Proofs
% ============================================================
\section{Preuves}

\begin{proof}
% Démonstration du théorème.
\end{proof}

% ============================================================
% Complexity Analysis
% ============================================================
\section{Analyse de Complexité}

% Analyse de la complexité temporelle et spatiale.
% Exemple de listing Python :
%
% \begin{lstlisting}[caption={Implémentation Python}]
% def fonction(n):
%     return n
% \end{lstlisting}

\end{document}
```
