---
title: pandoc
subtitle: Converting documents for fun and profit! 😉
author: Mauro Bieg
date: 2019-07-25
theme: sunblind
navigationMode: linear
controls: 0
---

# Introduction

## Hands 🙌

- Who knows pandoc?
- Who has used pandoc?
- Who has used pandoc as a Haskell library?

## The *pan*-universal<br> *doc*&#8202;ument converter

> Pandoc is a Haskell library for converting from one markup format to another, and a command-line tool that uses this library.

[pandoc.org](https://pandoc.org)

---

::: {style="overflow: scroll; height: 90vh"}
![](https://pandoc.org/diagram.jpg)
:::

## Pandoc and me 👨🏾‍💻

- Main-dev: John MacFarlane, Prof. of Philosophy at UC Berkeley
- first commit Oct 2006 on Google Code (GitHub was founded 2008)
- I used it for my bachelor thesis, soon made first pull request
- newbie-friendly code-base and community


# Demo

## Demo

- basics
- templates
- LaTeX and PDFs
- Math `echo '$\\frac{n!}{k!(n-k)!} = \\binom{n}{k}$'`


# Pandoc API

## Readers and writers

                          input format

                              ↓ reader

                          Pandoc AST

                              ↓ writer

                          output format

## The document AST

`Text.Pandoc.Definition` in [pandoc-types](http://hackage.haskell.org/package/pandoc-types-1.19/docs/Text-Pandoc-Definition.html)

```haskell
data Pandoc = Pandoc Meta [Block]

data Block
  = Para [Inline]
  | BulletList [[Block]]
  | Header Int Attr [Inline]
  | Div Attr [Block]

data Inline
  = Str String
  | Emph [Inline]
  | Strong [Inline]
  | Link Attr [Inline] Target
```

## Pandoc filters

    pandoc input.md -t json | pandoc-citeproc | pandoc -f json -o output.html

    pandoc input.md --filter pandoc-citeproc -o output.html

## Builder Demo

Generate Word documents without ever opening Word!\
(for example)

## [Haskell Filter](https://johnmacfarlane.net/BayHac2014/#/example-emphtocaps.hs)

```haskell
import Data.Char (toUpper)
import Text.Pandoc.Definition
import Text.Pandoc.JSON
import Text.Pandoc.Walk

allCaps :: Inline -> Inline
allCaps (Str xs) = Str $ map toUpper xs
allCaps x = x

emphToCaps :: Inline -> [Inline]
emphToCaps (Emph xs) = walk allCaps xs
emphToCaps x = [x]

main :: IO ()
main = toJSONFilter emphToCaps
```

# Thanks! 🤗
