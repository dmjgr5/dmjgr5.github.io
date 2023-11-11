---
layout: default
title: CSS
parent: Programming Language
nav_order: 6
---

# CSS
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---
 
{: .note}
CSS is Cascading Style Sheets that make more beautiful HTML. There are some cases to configure CSS for HTML.

 
## Main Poperties

-  Basic usage : <TAGNAME style=""></TAGNAME >
  - color
  - backbround-color
  - width
  - font-align
  - font-size
  - font-weight
  - border
- <div></div> : define an area


## 3 way to design CSS

### External Style Sheet
  - link style sheet file with css format

  ```html
    <head>
      <link rel="stylename" type="text/css" href="style.css">
    </head>
  ```
  - It is easy to maintain if there are some updates.



### Internal Style Sheet
  - Insert `<style>` between `<head>` and `</head>`
  - It is better to use for one HTML page.

  ```html
  <head>
    <style type="text/css">
        Selector {property: value;}
        ex) h1 {color:red} 
    </style>
  </head>
  ```



### Inline Style
  - Insert style format directly

  ```html
  <p style="font-size: 15px;">this is 15pixel</p>
  ```