---
name: xisupdf
description: >
  Strict LaTeX typesetting skill for standard university term reports (e.g., XISU / 西安外国语大学).
  Covers precise formatting requirements including font sizes (Xiaosi Songti for body, Xiaosan Heiti for headings),
  margins (2.54cm/3.17cm), line spread (1.5x), custom TOC with tocloft, fancyhdr for bottom-right page numbers,
  three-line tables, and specific reference list formatting. Use this skill whenever
  the user requests to format a report matching strict Chinese university guidelines (like 数字化运营期末考核报告).
---

# XISU PDF Skill — 数字化运营及标准大学课程报告 LaTeX 排版规范

Reference knowledge for typesetting long-form Chinese academic reports, strictly aligning with standard university guidelines (e.g., 西安外国语大学期末考核报告模板).

## 1. 页面设置与整体版面 (Page & Layout)

- **纸张与装订：** A4 纸大小，左侧装订（左页边距等效 3.17cm）。
- **页边距 (Margins)：** 上 2.54cm，下 2.54cm，左 3.17cm，右 3.17cm。
- **行距 (Line Spacing)：** 1.5倍行距 (`\linespread{1.5}`)。
- **页码 (Page Numbers)：** 位于页面底端右侧。
  ```latex
  \usepackage{fancyhdr}
  \fancypagestyle{plain}{
      \fancyhf{}
      \fancyfoot[R]{\thepage}
      \renewcommand{\headrulewidth}{0pt}
      \renewcommand{\footrulewidth}{0pt}
  }
  \pagestyle{plain}
  ```

## 2. 标题格式要求 (Heading Styles)

必须使用 `ctex` 宏包的 `\ctexset` 进行全局控制：
- **一级标题 (Section)：** 小三号黑体，居中，段前空 1.5 行，段后空 1 行。
- **二级标题 (Subsection)：** 四号黑体，居左，顶格，段前、段后 0.5 行。
- **三级标题 (Subsubsection)：** 四号黑体，居左，顶格，段前、段后 0.2 行。

```latex
\ctexset{
    section = {
        break = \clearpage,
        format = \centering\zihao{-3}\heiti,
        name = {, .}, % 渲染为 "1."
        number = \arabic{section},
        beforeskip = 1.5\baselineskip,
        afterskip = 1.0\baselineskip
    },
    % subsection, subsubsection 按规范同理设置
}
```

## 3. 正文与字体规范 (Body Text & Fonts)

- **正文内容：** 小四号宋体 (`\songti\zihao{-4}`)，必须覆盖全文主体。
- **英文/数字字体：** Times New Roman（通常 XeLaTeX 中声明 `\setmainfont{Times New Roman}` 即可，CTeX 会自动区分中英文）。

## 4. 目录排版 (Table of Contents)

- 目录标题需为三号黑体，居中。
- 目录项需做到：章节编号使用 Times New Roman，其他中文使用小四宋体。
- 必须借助 `tocloft` 宏包，并将各级目录引导线补齐。
```latex
\usepackage{tocloft}
\renewcommand{\cftsecfont}{\songti\zihao{-4}}
\renewcommand{\cftsubsecfont}{\songti\zihao{-4}}
\renewcommand{\cftsubsubsecfont}{\songti\zihao{-4}}
\renewcommand{\cftsecpagefont}{\songti\zihao{-4}}
\renewcommand{\cftsubsecpagefont}{\songti\zihao{-4}}
\renewcommand{\cftsubsubsecpagefont}{\songti\zihao{-4}}
\renewcommand{\cftsecleader}{\cftdotfill{\cftdotsep}}
```

## 5. 图表规范 (Figures & Tables)

- **图/表编号格式：** 必须按章编号，中间用连字符（例如 `图1-1`、`表2-1`）。
- **图/表标题：** 五号宋体 (`\songti\zihao{5}`)。表题在表上居中，图题在图下居中。
- **表格样式：** 必须为三线表，顶线和底线 1.5 磅，中间分隔线 1 磅（使用 `booktabs` 宏包的 `\toprule[1.5pt]`, `\midrule[1pt]`, `\bottomrule[1.5pt]`）。
- **资料来源：** 若有，置于图表下方居中，五号宋体。

## 6. 摘要与关键词 (Abstract & Keywords)

- “摘要”二字需使用小四号黑体 (`\heiti\zihao{-4}`)。
- 摘要正文与关键词需使用小四号宋体。
- 在 LaTeX 中需注意另起新段并正确留白。

## 7. 参考文献规范 (References)

- **标题：** “参考文献”必须为三号黑体，居中，段前空 1.5 行，段后空 1 行。在 LaTeX 中可通过重定义 `\refname` 实现：`\renewcommand{\refname}{\zihao{3} 参考文献}`。
- **文献条目：** 五号宋体，行间距 17 磅。
  ```latex
  \begin{thebibliography}{99}
  \addcontentsline{toc}{section}{参考文献}
  \zihao{5}\songti\setlength{\baselineskip}{17pt}
  \bibitem{ref1} ...
  \end{thebibliography}
  ```

## 总结使用建议

当用户要求修改报告以“符合模板”或“严格排版”时，请确保核对上述字号 (`\zihao`) 与间距设定，特别是通过 `tocloft` 对目录的深度干预和通过 `fancyhdr` 强行覆盖 LaTeX 默认正中页码的设置。
