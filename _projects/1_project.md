---
layout: page
title: Airfares analysis
description: Exploratory data analysis of airfares data
img: assets/img/12.jpg
importance: 1
category: Airfares analysis
related_publications: false
---

As part of an interview process, I was given a data set of airfares as they are observed over 2 months. In the code below you can see the work I did to clean the data and prepare it for meaningful analysis and observation.<br> 
I've calculated the volatility of the airfares of each flight based on the average and the standard deviation, and added the information to the data set so it can be used in viewing the trends of the prices over time. <br><br>

In the other link, I created a dashboard for visualizing the airfares prices. The user can choose the flight market (e.g. BOSRIC - Boston to Richmond), the departure date, and even select a specific flight number. In addition, he/she can filter by volatility level (low, mid or high) or the trend of the prices (positive, negative). If there is more than one flight (e.g. the 5 flights from Boston to Richmond on May 5 that their price is going down) then the plot will show the average price over the observation time period.


Here's the code for analyzing the data:

{% raw %}


<html>
<head><meta charset="utf-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>airfares_construction</title><script src="https://cdnjs.cloudflare.com/ajax/libs/require.js/2.1.10/require.min.js"></script>




<style type="text/css">
    pre { line-height: 125%; }
td.linenos .normal { color: inherit; background-color: transparent; padding-left: 5px; padding-right: 5px; }
span.linenos { color: inherit; background-color: transparent; padding-left: 5px; padding-right: 5px; }
td.linenos .special { color: #000000; background-color: #ffffc0; padding-left: 5px; padding-right: 5px; }
span.linenos.special { color: #000000; background-color: #ffffc0; padding-left: 5px; padding-right: 5px; }
.highlight .hll { background-color: var(--jp-cell-editor-active-background) }
.highlight { background: var(--jp-cell-editor-background); color: var(--jp-mirror-editor-variable-color) }
.highlight .c { color: var(--jp-mirror-editor-comment-color); font-style: italic } /* Comment */
.highlight .err { color: var(--jp-mirror-editor-error-color) } /* Error */
.highlight .k { color: var(--jp-mirror-editor-keyword-color); font-weight: bold } /* Keyword */
.highlight .o { color: var(--jp-mirror-editor-operator-color); font-weight: bold } /* Operator */
.highlight .p { color: var(--jp-mirror-editor-punctuation-color) } /* Punctuation */
.highlight .ch { color: var(--jp-mirror-editor-comment-color); font-style: italic } /* Comment.Hashbang */
.highlight .cm { color: var(--jp-mirror-editor-comment-color); font-style: italic } /* Comment.Multiline */
.highlight .cp { color: var(--jp-mirror-editor-comment-color); font-style: italic } /* Comment.Preproc */
.highlight .cpf { color: var(--jp-mirror-editor-comment-color); font-style: italic } /* Comment.PreprocFile */
.highlight .c1 { color: var(--jp-mirror-editor-comment-color); font-style: italic } /* Comment.Single */
.highlight .cs { color: var(--jp-mirror-editor-comment-color); font-style: italic } /* Comment.Special */
.highlight .kc { color: var(--jp-mirror-editor-keyword-color); font-weight: bold } /* Keyword.Constant */
.highlight .kd { color: var(--jp-mirror-editor-keyword-color); font-weight: bold } /* Keyword.Declaration */
.highlight .kn { color: var(--jp-mirror-editor-keyword-color); font-weight: bold } /* Keyword.Namespace */
.highlight .kp { color: var(--jp-mirror-editor-keyword-color); font-weight: bold } /* Keyword.Pseudo */
.highlight .kr { color: var(--jp-mirror-editor-keyword-color); font-weight: bold } /* Keyword.Reserved */
.highlight .kt { color: var(--jp-mirror-editor-keyword-color); font-weight: bold } /* Keyword.Type */
.highlight .m { color: var(--jp-mirror-editor-number-color) } /* Literal.Number */
.highlight .s { color: var(--jp-mirror-editor-string-color) } /* Literal.String */
.highlight .ow { color: var(--jp-mirror-editor-operator-color); font-weight: bold } /* Operator.Word */
.highlight .pm { color: var(--jp-mirror-editor-punctuation-color) } /* Punctuation.Marker */
.highlight .w { color: var(--jp-mirror-editor-variable-color) } /* Text.Whitespace */
.highlight .mb { color: var(--jp-mirror-editor-number-color) } /* Literal.Number.Bin */
.highlight .mf { color: var(--jp-mirror-editor-number-color) } /* Literal.Number.Float */
.highlight .mh { color: var(--jp-mirror-editor-number-color) } /* Literal.Number.Hex */
.highlight .mi { color: var(--jp-mirror-editor-number-color) } /* Literal.Number.Integer */
.highlight .mo { color: var(--jp-mirror-editor-number-color) } /* Literal.Number.Oct */
.highlight .sa { color: var(--jp-mirror-editor-string-color) } /* Literal.String.Affix */
.highlight .sb { color: var(--jp-mirror-editor-string-color) } /* Literal.String.Backtick */
.highlight .sc { color: var(--jp-mirror-editor-string-color) } /* Literal.String.Char */
.highlight .dl { color: var(--jp-mirror-editor-string-color) } /* Literal.String.Delimiter */
.highlight .sd { color: var(--jp-mirror-editor-string-color) } /* Literal.String.Doc */
.highlight .s2 { color: var(--jp-mirror-editor-string-color) } /* Literal.String.Double */
.highlight .se { color: var(--jp-mirror-editor-string-color) } /* Literal.String.Escape */
.highlight .sh { color: var(--jp-mirror-editor-string-color) } /* Literal.String.Heredoc */
.highlight .si { color: var(--jp-mirror-editor-string-color) } /* Literal.String.Interpol */
.highlight .sx { color: var(--jp-mirror-editor-string-color) } /* Literal.String.Other */
.highlight .sr { color: var(--jp-mirror-editor-string-color) } /* Literal.String.Regex */
.highlight .s1 { color: var(--jp-mirror-editor-string-color) } /* Literal.String.Single */
.highlight .ss { color: var(--jp-mirror-editor-string-color) } /* Literal.String.Symbol */
.highlight .il { color: var(--jp-mirror-editor-number-color) } /* Literal.Number.Integer.Long */
  </style>



<style type="text/css">
/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

/*
 * Mozilla scrollbar styling
 */

/* use standard opaque scrollbars for most nodes */
[data-jp-theme-scrollbars='true'] {
  scrollbar-color: rgb(var(--jp-scrollbar-thumb-color))
    var(--jp-scrollbar-background-color);
}

/* for code nodes, use a transparent style of scrollbar. These selectors
 * will match lower in the tree, and so will override the above */
[data-jp-theme-scrollbars='true'] .CodeMirror-hscrollbar,
[data-jp-theme-scrollbars='true'] .CodeMirror-vscrollbar {
  scrollbar-color: rgba(var(--jp-scrollbar-thumb-color), 0.5) transparent;
}

/* tiny scrollbar */

.jp-scrollbar-tiny {
  scrollbar-color: rgba(var(--jp-scrollbar-thumb-color), 0.5) transparent;
  scrollbar-width: thin;
}

/*
 * Webkit scrollbar styling
 */

/* use standard opaque scrollbars for most nodes */

[data-jp-theme-scrollbars='true'] ::-webkit-scrollbar,
[data-jp-theme-scrollbars='true'] ::-webkit-scrollbar-corner {
  background: var(--jp-scrollbar-background-color);
}

[data-jp-theme-scrollbars='true'] ::-webkit-scrollbar-thumb {
  background: rgb(var(--jp-scrollbar-thumb-color));
  border: var(--jp-scrollbar-thumb-margin) solid transparent;
  background-clip: content-box;
  border-radius: var(--jp-scrollbar-thumb-radius);
}

[data-jp-theme-scrollbars='true'] ::-webkit-scrollbar-track:horizontal {
  border-left: var(--jp-scrollbar-endpad) solid
    var(--jp-scrollbar-background-color);
  border-right: var(--jp-scrollbar-endpad) solid
    var(--jp-scrollbar-background-color);
}

[data-jp-theme-scrollbars='true'] ::-webkit-scrollbar-track:vertical {
  border-top: var(--jp-scrollbar-endpad) solid
    var(--jp-scrollbar-background-color);
  border-bottom: var(--jp-scrollbar-endpad) solid
    var(--jp-scrollbar-background-color);
}

/* for code nodes, use a transparent style of scrollbar */

[data-jp-theme-scrollbars='true'] .CodeMirror-hscrollbar::-webkit-scrollbar,
[data-jp-theme-scrollbars='true'] .CodeMirror-vscrollbar::-webkit-scrollbar,
[data-jp-theme-scrollbars='true']
  .CodeMirror-hscrollbar::-webkit-scrollbar-corner,
[data-jp-theme-scrollbars='true']
  .CodeMirror-vscrollbar::-webkit-scrollbar-corner {
  background-color: transparent;
}

[data-jp-theme-scrollbars='true']
  .CodeMirror-hscrollbar::-webkit-scrollbar-thumb,
[data-jp-theme-scrollbars='true']
  .CodeMirror-vscrollbar::-webkit-scrollbar-thumb {
  background: rgba(var(--jp-scrollbar-thumb-color), 0.5);
  border: var(--jp-scrollbar-thumb-margin) solid transparent;
  background-clip: content-box;
  border-radius: var(--jp-scrollbar-thumb-radius);
}

[data-jp-theme-scrollbars='true']
  .CodeMirror-hscrollbar::-webkit-scrollbar-track:horizontal {
  border-left: var(--jp-scrollbar-endpad) solid transparent;
  border-right: var(--jp-scrollbar-endpad) solid transparent;
}

[data-jp-theme-scrollbars='true']
  .CodeMirror-vscrollbar::-webkit-scrollbar-track:vertical {
  border-top: var(--jp-scrollbar-endpad) solid transparent;
  border-bottom: var(--jp-scrollbar-endpad) solid transparent;
}

/* tiny scrollbar */

.jp-scrollbar-tiny::-webkit-scrollbar,
.jp-scrollbar-tiny::-webkit-scrollbar-corner {
  background-color: transparent;
  height: 4px;
  width: 4px;
}

.jp-scrollbar-tiny::-webkit-scrollbar-thumb {
  background: rgba(var(--jp-scrollbar-thumb-color), 0.5);
}

.jp-scrollbar-tiny::-webkit-scrollbar-track:horizontal {
  border-left: 0px solid transparent;
  border-right: 0px solid transparent;
}

.jp-scrollbar-tiny::-webkit-scrollbar-track:vertical {
  border-top: 0px solid transparent;
  border-bottom: 0px solid transparent;
}

/*
 * Phosphor
 */

.lm-ScrollBar[data-orientation='horizontal'] {
  min-height: 16px;
  max-height: 16px;
  min-width: 45px;
  border-top: 1px solid #a0a0a0;
}

.lm-ScrollBar[data-orientation='vertical'] {
  min-width: 16px;
  max-width: 16px;
  min-height: 45px;
  border-left: 1px solid #a0a0a0;
}

.lm-ScrollBar-button {
  background-color: #f0f0f0;
  background-position: center center;
  min-height: 15px;
  max-height: 15px;
  min-width: 15px;
  max-width: 15px;
}

.lm-ScrollBar-button:hover {
  background-color: #dadada;
}

.lm-ScrollBar-button.lm-mod-active {
  background-color: #cdcdcd;
}

.lm-ScrollBar-track {
  background: #f0f0f0;
}

.lm-ScrollBar-thumb {
  background: #cdcdcd;
}

.lm-ScrollBar-thumb:hover {
  background: #bababa;
}

.lm-ScrollBar-thumb.lm-mod-active {
  background: #a0a0a0;
}

.lm-ScrollBar[data-orientation='horizontal'] .lm-ScrollBar-thumb {
  height: 100%;
  min-width: 15px;
  border-left: 1px solid #a0a0a0;
  border-right: 1px solid #a0a0a0;
}

.lm-ScrollBar[data-orientation='vertical'] .lm-ScrollBar-thumb {
  width: 100%;
  min-height: 15px;
  border-top: 1px solid #a0a0a0;
  border-bottom: 1px solid #a0a0a0;
}

.lm-ScrollBar[data-orientation='horizontal']
  .lm-ScrollBar-button[data-action='decrement'] {
  background-image: var(--jp-icon-caret-left);
  background-size: 17px;
}

.lm-ScrollBar[data-orientation='horizontal']
  .lm-ScrollBar-button[data-action='increment'] {
  background-image: var(--jp-icon-caret-right);
  background-size: 17px;
}

.lm-ScrollBar[data-orientation='vertical']
  .lm-ScrollBar-button[data-action='decrement'] {
  background-image: var(--jp-icon-caret-up);
  background-size: 17px;
}

.lm-ScrollBar[data-orientation='vertical']
  .lm-ScrollBar-button[data-action='increment'] {
  background-image: var(--jp-icon-caret-down);
  background-size: 17px;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Copyright (c) 2014-2017, PhosphorJS Contributors
|
| Distributed under the terms of the BSD 3-Clause License.
|
| The full license is in the file LICENSE, distributed with this software.
|----------------------------------------------------------------------------*/


/* <DEPRECATED> */ .p-Widget, /* </DEPRECATED> */
.lm-Widget {
  box-sizing: border-box;
  position: relative;
  overflow: hidden;
  cursor: default;
}


/* <DEPRECATED> */ .p-Widget.p-mod-hidden, /* </DEPRECATED> */
.lm-Widget.lm-mod-hidden {
  display: none !important;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Copyright (c) 2014-2017, PhosphorJS Contributors
|
| Distributed under the terms of the BSD 3-Clause License.
|
| The full license is in the file LICENSE, distributed with this software.
|----------------------------------------------------------------------------*/


/* <DEPRECATED> */ .p-CommandPalette, /* </DEPRECATED> */
.lm-CommandPalette {
  display: flex;
  flex-direction: column;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}


/* <DEPRECATED> */ .p-CommandPalette-search, /* </DEPRECATED> */
.lm-CommandPalette-search {
  flex: 0 0 auto;
}


/* <DEPRECATED> */ .p-CommandPalette-content, /* </DEPRECATED> */
.lm-CommandPalette-content {
  flex: 1 1 auto;
  margin: 0;
  padding: 0;
  min-height: 0;
  overflow: auto;
  list-style-type: none;
}


/* <DEPRECATED> */ .p-CommandPalette-header, /* </DEPRECATED> */
.lm-CommandPalette-header {
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}


/* <DEPRECATED> */ .p-CommandPalette-item, /* </DEPRECATED> */
.lm-CommandPalette-item {
  display: flex;
  flex-direction: row;
}


/* <DEPRECATED> */ .p-CommandPalette-itemIcon, /* </DEPRECATED> */
.lm-CommandPalette-itemIcon {
  flex: 0 0 auto;
}


/* <DEPRECATED> */ .p-CommandPalette-itemContent, /* </DEPRECATED> */
.lm-CommandPalette-itemContent {
  flex: 1 1 auto;
  overflow: hidden;
}


/* <DEPRECATED> */ .p-CommandPalette-itemShortcut, /* </DEPRECATED> */
.lm-CommandPalette-itemShortcut {
  flex: 0 0 auto;
}


/* <DEPRECATED> */ .p-CommandPalette-itemLabel, /* </DEPRECATED> */
.lm-CommandPalette-itemLabel {
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}

.lm-close-icon {
	border:1px solid transparent;
  background-color: transparent;
  position: absolute;
	z-index:1;
	right:3%;
	top: 0;
	bottom: 0;
	margin: auto;
	padding: 7px 0;
	display: none;
	vertical-align: middle;
  outline: 0;
  cursor: pointer;
}
.lm-close-icon:after {
	content: "X";
	display: block;
	width: 15px;
	height: 15px;
	text-align: center;
	color:#000;
	font-weight: normal;
	font-size: 12px;
	cursor: pointer;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Copyright (c) 2014-2017, PhosphorJS Contributors
|
| Distributed under the terms of the BSD 3-Clause License.
|
| The full license is in the file LICENSE, distributed with this software.
|----------------------------------------------------------------------------*/


/* <DEPRECATED> */ .p-DockPanel, /* </DEPRECATED> */
.lm-DockPanel {
  z-index: 0;
}


/* <DEPRECATED> */ .p-DockPanel-widget, /* </DEPRECATED> */
.lm-DockPanel-widget {
  z-index: 0;
}


/* <DEPRECATED> */ .p-DockPanel-tabBar, /* </DEPRECATED> */
.lm-DockPanel-tabBar {
  z-index: 1;
}


/* <DEPRECATED> */ .p-DockPanel-handle, /* </DEPRECATED> */
.lm-DockPanel-handle {
  z-index: 2;
}


/* <DEPRECATED> */ .p-DockPanel-handle.p-mod-hidden, /* </DEPRECATED> */
.lm-DockPanel-handle.lm-mod-hidden {
  display: none !important;
}


/* <DEPRECATED> */ .p-DockPanel-handle:after, /* </DEPRECATED> */
.lm-DockPanel-handle:after {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  content: '';
}


/* <DEPRECATED> */
.p-DockPanel-handle[data-orientation='horizontal'],
/* </DEPRECATED> */
.lm-DockPanel-handle[data-orientation='horizontal'] {
  cursor: ew-resize;
}


/* <DEPRECATED> */
.p-DockPanel-handle[data-orientation='vertical'],
/* </DEPRECATED> */
.lm-DockPanel-handle[data-orientation='vertical'] {
  cursor: ns-resize;
}


/* <DEPRECATED> */
.p-DockPanel-handle[data-orientation='horizontal']:after,
/* </DEPRECATED> */
.lm-DockPanel-handle[data-orientation='horizontal']:after {
  left: 50%;
  min-width: 8px;
  transform: translateX(-50%);
}


/* <DEPRECATED> */
.p-DockPanel-handle[data-orientation='vertical']:after,
/* </DEPRECATED> */
.lm-DockPanel-handle[data-orientation='vertical']:after {
  top: 50%;
  min-height: 8px;
  transform: translateY(-50%);
}


/* <DEPRECATED> */ .p-DockPanel-overlay, /* </DEPRECATED> */
.lm-DockPanel-overlay {
  z-index: 3;
  box-sizing: border-box;
  pointer-events: none;
}


/* <DEPRECATED> */ .p-DockPanel-overlay.p-mod-hidden, /* </DEPRECATED> */
.lm-DockPanel-overlay.lm-mod-hidden {
  display: none !important;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Copyright (c) 2014-2017, PhosphorJS Contributors
|
| Distributed under the terms of the BSD 3-Clause License.
|
| The full license is in the file LICENSE, distributed with this software.
|----------------------------------------------------------------------------*/


/* <DEPRECATED> */ .p-Menu, /* </DEPRECATED> */
.lm-Menu {
  z-index: 10000;
  position: absolute;
  white-space: nowrap;
  overflow-x: hidden;
  overflow-y: auto;
  outline: none;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}


/* <DEPRECATED> */ .p-Menu-content, /* </DEPRECATED> */
.lm-Menu-content {
  margin: 0;
  padding: 0;
  display: table;
  list-style-type: none;
}


/* <DEPRECATED> */ .p-Menu-item, /* </DEPRECATED> */
.lm-Menu-item {
  display: table-row;
}


/* <DEPRECATED> */
.p-Menu-item.p-mod-hidden,
.p-Menu-item.p-mod-collapsed,
/* </DEPRECATED> */
.lm-Menu-item.lm-mod-hidden,
.lm-Menu-item.lm-mod-collapsed {
  display: none !important;
}


/* <DEPRECATED> */
.p-Menu-itemIcon,
.p-Menu-itemSubmenuIcon,
/* </DEPRECATED> */
.lm-Menu-itemIcon,
.lm-Menu-itemSubmenuIcon {
  display: table-cell;
  text-align: center;
}


/* <DEPRECATED> */ .p-Menu-itemLabel, /* </DEPRECATED> */
.lm-Menu-itemLabel {
  display: table-cell;
  text-align: left;
}


/* <DEPRECATED> */ .p-Menu-itemShortcut, /* </DEPRECATED> */
.lm-Menu-itemShortcut {
  display: table-cell;
  text-align: right;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Copyright (c) 2014-2017, PhosphorJS Contributors
|
| Distributed under the terms of the BSD 3-Clause License.
|
| The full license is in the file LICENSE, distributed with this software.
|----------------------------------------------------------------------------*/


/* <DEPRECATED> */ .p-MenuBar, /* </DEPRECATED> */
.lm-MenuBar {
  outline: none;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}


/* <DEPRECATED> */ .p-MenuBar-content, /* </DEPRECATED> */
.lm-MenuBar-content {
  margin: 0;
  padding: 0;
  display: flex;
  flex-direction: row;
  list-style-type: none;
}


/* <DEPRECATED> */ .p--MenuBar-item, /* </DEPRECATED> */
.lm-MenuBar-item {
  box-sizing: border-box;
}


/* <DEPRECATED> */
.p-MenuBar-itemIcon,
.p-MenuBar-itemLabel,
/* </DEPRECATED> */
.lm-MenuBar-itemIcon,
.lm-MenuBar-itemLabel {
  display: inline-block;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Copyright (c) 2014-2017, PhosphorJS Contributors
|
| Distributed under the terms of the BSD 3-Clause License.
|
| The full license is in the file LICENSE, distributed with this software.
|----------------------------------------------------------------------------*/


/* <DEPRECATED> */ .p-ScrollBar, /* </DEPRECATED> */
.lm-ScrollBar {
  display: flex;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}


/* <DEPRECATED> */
.p-ScrollBar[data-orientation='horizontal'],
/* </DEPRECATED> */
.lm-ScrollBar[data-orientation='horizontal'] {
  flex-direction: row;
}


/* <DEPRECATED> */
.p-ScrollBar[data-orientation='vertical'],
/* </DEPRECATED> */
.lm-ScrollBar[data-orientation='vertical'] {
  flex-direction: column;
}


/* <DEPRECATED> */ .p-ScrollBar-button, /* </DEPRECATED> */
.lm-ScrollBar-button {
  box-sizing: border-box;
  flex: 0 0 auto;
}


/* <DEPRECATED> */ .p-ScrollBar-track, /* </DEPRECATED> */
.lm-ScrollBar-track {
  box-sizing: border-box;
  position: relative;
  overflow: hidden;
  flex: 1 1 auto;
}


/* <DEPRECATED> */ .p-ScrollBar-thumb, /* </DEPRECATED> */
.lm-ScrollBar-thumb {
  box-sizing: border-box;
  position: absolute;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Copyright (c) 2014-2017, PhosphorJS Contributors
|
| Distributed under the terms of the BSD 3-Clause License.
|
| The full license is in the file LICENSE, distributed with this software.
|----------------------------------------------------------------------------*/


/* <DEPRECATED> */ .p-SplitPanel-child, /* </DEPRECATED> */
.lm-SplitPanel-child {
  z-index: 0;
}


/* <DEPRECATED> */ .p-SplitPanel-handle, /* </DEPRECATED> */
.lm-SplitPanel-handle {
  z-index: 1;
}


/* <DEPRECATED> */ .p-SplitPanel-handle.p-mod-hidden, /* </DEPRECATED> */
.lm-SplitPanel-handle.lm-mod-hidden {
  display: none !important;
}


/* <DEPRECATED> */ .p-SplitPanel-handle:after, /* </DEPRECATED> */
.lm-SplitPanel-handle:after {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  content: '';
}


/* <DEPRECATED> */
.p-SplitPanel[data-orientation='horizontal'] > .p-SplitPanel-handle,
/* </DEPRECATED> */
.lm-SplitPanel[data-orientation='horizontal'] > .lm-SplitPanel-handle {
  cursor: ew-resize;
}


/* <DEPRECATED> */
.p-SplitPanel[data-orientation='vertical'] > .p-SplitPanel-handle,
/* </DEPRECATED> */
.lm-SplitPanel[data-orientation='vertical'] > .lm-SplitPanel-handle {
  cursor: ns-resize;
}


/* <DEPRECATED> */
.p-SplitPanel[data-orientation='horizontal'] > .p-SplitPanel-handle:after,
/* </DEPRECATED> */
.lm-SplitPanel[data-orientation='horizontal'] > .lm-SplitPanel-handle:after {
  left: 50%;
  min-width: 8px;
  transform: translateX(-50%);
}


/* <DEPRECATED> */
.p-SplitPanel[data-orientation='vertical'] > .p-SplitPanel-handle:after,
/* </DEPRECATED> */
.lm-SplitPanel[data-orientation='vertical'] > .lm-SplitPanel-handle:after {
  top: 50%;
  min-height: 8px;
  transform: translateY(-50%);
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Copyright (c) 2014-2017, PhosphorJS Contributors
|
| Distributed under the terms of the BSD 3-Clause License.
|
| The full license is in the file LICENSE, distributed with this software.
|----------------------------------------------------------------------------*/


/* <DEPRECATED> */ .p-TabBar, /* </DEPRECATED> */
.lm-TabBar {
  display: flex;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}


/* <DEPRECATED> */ .p-TabBar[data-orientation='horizontal'], /* </DEPRECATED> */
.lm-TabBar[data-orientation='horizontal'] {
  flex-direction: row;
  align-items: flex-end;
}


/* <DEPRECATED> */ .p-TabBar[data-orientation='vertical'], /* </DEPRECATED> */
.lm-TabBar[data-orientation='vertical'] {
  flex-direction: column;
  align-items: flex-end;
}


/* <DEPRECATED> */ .p-TabBar-content, /* </DEPRECATED> */
.lm-TabBar-content {
  margin: 0;
  padding: 0;
  display: flex;
  flex: 1 1 auto;
  list-style-type: none;
}


/* <DEPRECATED> */
.p-TabBar[data-orientation='horizontal'] > .p-TabBar-content,
/* </DEPRECATED> */
.lm-TabBar[data-orientation='horizontal'] > .lm-TabBar-content {
  flex-direction: row;
}


/* <DEPRECATED> */
.p-TabBar[data-orientation='vertical'] > .p-TabBar-content,
/* </DEPRECATED> */
.lm-TabBar[data-orientation='vertical'] > .lm-TabBar-content {
  flex-direction: column;
}


/* <DEPRECATED> */ .p-TabBar-tab, /* </DEPRECATED> */
.lm-TabBar-tab {
  display: flex;
  flex-direction: row;
  box-sizing: border-box;
  overflow: hidden;
}


/* <DEPRECATED> */
.p-TabBar-tabIcon,
.p-TabBar-tabCloseIcon,
/* </DEPRECATED> */
.lm-TabBar-tabIcon,
.lm-TabBar-tabCloseIcon {
  flex: 0 0 auto;
}


/* <DEPRECATED> */ .p-TabBar-tabLabel, /* </DEPRECATED> */
.lm-TabBar-tabLabel {
  flex: 1 1 auto;
  overflow: hidden;
  white-space: nowrap;
}


.lm-TabBar-tabInput {
  user-select: all;
  width: 100%;
  box-sizing : border-box;
}


/* <DEPRECATED> */ .p-TabBar-tab.p-mod-hidden, /* </DEPRECATED> */
.lm-TabBar-tab.lm-mod-hidden {
  display: none !important;
}


.lm-TabBar-addButton.lm-mod-hidden {
  display: none !important;
}


/* <DEPRECATED> */ .p-TabBar.p-mod-dragging .p-TabBar-tab, /* </DEPRECATED> */
.lm-TabBar.lm-mod-dragging .lm-TabBar-tab {
  position: relative;
}


/* <DEPRECATED> */
.p-TabBar.p-mod-dragging[data-orientation='horizontal'] .p-TabBar-tab,
/* </DEPRECATED> */
.lm-TabBar.lm-mod-dragging[data-orientation='horizontal'] .lm-TabBar-tab {
  left: 0;
  transition: left 150ms ease;
}


/* <DEPRECATED> */
.p-TabBar.p-mod-dragging[data-orientation='vertical'] .p-TabBar-tab,
/* </DEPRECATED> */
.lm-TabBar.lm-mod-dragging[data-orientation='vertical'] .lm-TabBar-tab {
  top: 0;
  transition: top 150ms ease;
}


/* <DEPRECATED> */
.p-TabBar.p-mod-dragging .p-TabBar-tab.p-mod-dragging,
/* </DEPRECATED> */
.lm-TabBar.lm-mod-dragging .lm-TabBar-tab.lm-mod-dragging {
  transition: none;
}

.lm-TabBar-tabLabel .lm-TabBar-tabInput {
  user-select: all;
  width: 100%;
  box-sizing : border-box;
  background: inherit;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Copyright (c) 2014-2017, PhosphorJS Contributors
|
| Distributed under the terms of the BSD 3-Clause License.
|
| The full license is in the file LICENSE, distributed with this software.
|----------------------------------------------------------------------------*/


/* <DEPRECATED> */ .p-TabPanel-tabBar, /* </DEPRECATED> */
.lm-TabPanel-tabBar {
  z-index: 1;
}


/* <DEPRECATED> */ .p-TabPanel-stackedPanel, /* </DEPRECATED> */
.lm-TabPanel-stackedPanel {
  z-index: 0;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Copyright (c) 2014-2017, PhosphorJS Contributors
|
| Distributed under the terms of the BSD 3-Clause License.
|
| The full license is in the file LICENSE, distributed with this software.
|----------------------------------------------------------------------------*/

@charset "UTF-8";
html{
  -webkit-box-sizing:border-box;
          box-sizing:border-box; }

*,
*::before,
*::after{
  -webkit-box-sizing:inherit;
          box-sizing:inherit; }

body{
  font-size:14px;
  font-weight:400;
  letter-spacing:0;
  line-height:1.28581;
  text-transform:none;
  color:#182026;
  font-family:-apple-system, "BlinkMacSystemFont", "Segoe UI", "Roboto", "Oxygen", "Ubuntu", "Cantarell", "Open Sans", "Helvetica Neue", "Icons16", sans-serif; }

p{
  margin-bottom:10px;
  margin-top:0; }

small{
  font-size:12px; }

strong{
  font-weight:600; }

::-moz-selection{
  background:rgba(125, 188, 255, 0.6); }

::selection{
  background:rgba(125, 188, 255, 0.6); }
.bp3-heading{
  color:#182026;
  font-weight:600;
  margin:0 0 10px;
  padding:0; }
  .bp3-dark .bp3-heading{
    color:#f5f8fa; }

h1.bp3-heading, .bp3-running-text h1{
  font-size:36px;
  line-height:40px; }

h2.bp3-heading, .bp3-running-text h2{
  font-size:28px;
  line-height:32px; }

h3.bp3-heading, .bp3-running-text h3{
  font-size:22px;
  line-height:25px; }

h4.bp3-heading, .bp3-running-text h4{
  font-size:18px;
  line-height:21px; }

h5.bp3-heading, .bp3-running-text h5{
  font-size:16px;
  line-height:19px; }

h6.bp3-heading, .bp3-running-text h6{
  font-size:14px;
  line-height:16px; }
.bp3-ui-text{
  font-size:14px;
  font-weight:400;
  letter-spacing:0;
  line-height:1.28581;
  text-transform:none; }

.bp3-monospace-text{
  font-family:monospace;
  text-transform:none; }

.bp3-text-muted{
  color:#5c7080; }
  .bp3-dark .bp3-text-muted{
    color:#a7b6c2; }

.bp3-text-disabled{
  color:rgba(92, 112, 128, 0.6); }
  .bp3-dark .bp3-text-disabled{
    color:rgba(167, 182, 194, 0.6); }

.bp3-text-overflow-ellipsis{
  overflow:hidden;
  text-overflow:ellipsis;
  white-space:nowrap;
  word-wrap:normal; }
.bp3-running-text{
  font-size:14px;
  line-height:1.5; }
  .bp3-running-text h1{
    color:#182026;
    font-weight:600;
    margin-bottom:20px;
    margin-top:40px; }
    .bp3-dark .bp3-running-text h1{
      color:#f5f8fa; }
  .bp3-running-text h2{
    color:#182026;
    font-weight:600;
    margin-bottom:20px;
    margin-top:40px; }
    .bp3-dark .bp3-running-text h2{
      color:#f5f8fa; }
  .bp3-running-text h3{
    color:#182026;
    font-weight:600;
    margin-bottom:20px;
    margin-top:40px; }
    .bp3-dark .bp3-running-text h3{
      color:#f5f8fa; }
  .bp3-running-text h4{
    color:#182026;
    font-weight:600;
    margin-bottom:20px;
    margin-top:40px; }
    .bp3-dark .bp3-running-text h4{
      color:#f5f8fa; }
  .bp3-running-text h5{
    color:#182026;
    font-weight:600;
    margin-bottom:20px;
    margin-top:40px; }
    .bp3-dark .bp3-running-text h5{
      color:#f5f8fa; }
  .bp3-running-text h6{
    color:#182026;
    font-weight:600;
    margin-bottom:20px;
    margin-top:40px; }
    .bp3-dark .bp3-running-text h6{
      color:#f5f8fa; }
  .bp3-running-text hr{
    border:none;
    border-bottom:1px solid rgba(16, 22, 26, 0.15);
    margin:20px 0; }
    .bp3-dark .bp3-running-text hr{
      border-color:rgba(255, 255, 255, 0.15); }
  .bp3-running-text p{
    margin:0 0 10px;
    padding:0; }

.bp3-text-large{
  font-size:16px; }

.bp3-text-small{
  font-size:12px; }
a{
  color:#106ba3;
  text-decoration:none; }
  a:hover{
    color:#106ba3;
    cursor:pointer;
    text-decoration:underline; }
  a .bp3-icon, a .bp3-icon-standard, a .bp3-icon-large{
    color:inherit; }
  a code,
  .bp3-dark a code{
    color:inherit; }
  .bp3-dark a,
  .bp3-dark a:hover{
    color:#48aff0; }
    .bp3-dark a .bp3-icon, .bp3-dark a .bp3-icon-standard, .bp3-dark a .bp3-icon-large,
    .bp3-dark a:hover .bp3-icon,
    .bp3-dark a:hover .bp3-icon-standard,
    .bp3-dark a:hover .bp3-icon-large{
      color:inherit; }
.bp3-running-text code, .bp3-code{
  font-family:monospace;
  text-transform:none;
  background:rgba(255, 255, 255, 0.7);
  border-radius:3px;
  -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2);
          box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2);
  color:#5c7080;
  font-size:smaller;
  padding:2px 5px; }
  .bp3-dark .bp3-running-text code, .bp3-running-text .bp3-dark code, .bp3-dark .bp3-code{
    background:rgba(16, 22, 26, 0.3);
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4);
    color:#a7b6c2; }
  .bp3-running-text a > code, a > .bp3-code{
    color:#137cbd; }
    .bp3-dark .bp3-running-text a > code, .bp3-running-text .bp3-dark a > code, .bp3-dark a > .bp3-code{
      color:inherit; }

.bp3-running-text pre, .bp3-code-block{
  font-family:monospace;
  text-transform:none;
  background:rgba(255, 255, 255, 0.7);
  border-radius:3px;
  -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.15);
          box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.15);
  color:#182026;
  display:block;
  font-size:13px;
  line-height:1.4;
  margin:10px 0;
  padding:13px 15px 12px;
  word-break:break-all;
  word-wrap:break-word; }
  .bp3-dark .bp3-running-text pre, .bp3-running-text .bp3-dark pre, .bp3-dark .bp3-code-block{
    background:rgba(16, 22, 26, 0.3);
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4);
    color:#f5f8fa; }
  .bp3-running-text pre > code, .bp3-code-block > code{
    background:none;
    -webkit-box-shadow:none;
            box-shadow:none;
    color:inherit;
    font-size:inherit;
    padding:0; }

.bp3-running-text kbd, .bp3-key{
  -webkit-box-align:center;
      -ms-flex-align:center;
          align-items:center;
  background:#ffffff;
  border-radius:3px;
  -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 0 0 rgba(16, 22, 26, 0), 0 1px 1px rgba(16, 22, 26, 0.2);
          box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 0 0 rgba(16, 22, 26, 0), 0 1px 1px rgba(16, 22, 26, 0.2);
  color:#5c7080;
  display:-webkit-inline-box;
  display:-ms-inline-flexbox;
  display:inline-flex;
  font-family:inherit;
  font-size:12px;
  height:24px;
  -webkit-box-pack:center;
      -ms-flex-pack:center;
          justify-content:center;
  line-height:24px;
  min-width:24px;
  padding:3px 6px;
  vertical-align:middle; }
  .bp3-running-text kbd .bp3-icon, .bp3-key .bp3-icon, .bp3-running-text kbd .bp3-icon-standard, .bp3-key .bp3-icon-standard, .bp3-running-text kbd .bp3-icon-large, .bp3-key .bp3-icon-large{
    margin-right:5px; }
  .bp3-dark .bp3-running-text kbd, .bp3-running-text .bp3-dark kbd, .bp3-dark .bp3-key{
    background:#394b59;
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 0 0 rgba(16, 22, 26, 0), 0 1px 1px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 0 0 rgba(16, 22, 26, 0), 0 1px 1px rgba(16, 22, 26, 0.4);
    color:#a7b6c2; }
.bp3-running-text blockquote, .bp3-blockquote{
  border-left:solid 4px rgba(167, 182, 194, 0.5);
  margin:0 0 10px;
  padding:0 20px; }
  .bp3-dark .bp3-running-text blockquote, .bp3-running-text .bp3-dark blockquote, .bp3-dark .bp3-blockquote{
    border-color:rgba(115, 134, 148, 0.5); }
.bp3-running-text ul,
.bp3-running-text ol, .bp3-list{
  margin:10px 0;
  padding-left:30px; }
  .bp3-running-text ul li:not(:last-child), .bp3-running-text ol li:not(:last-child), .bp3-list li:not(:last-child){
    margin-bottom:5px; }
  .bp3-running-text ul ol, .bp3-running-text ol ol, .bp3-list ol,
  .bp3-running-text ul ul,
  .bp3-running-text ol ul,
  .bp3-list ul{
    margin-top:5px; }

.bp3-list-unstyled{
  list-style:none;
  margin:0;
  padding:0; }
  .bp3-list-unstyled li{
    padding:0; }
.bp3-rtl{
  text-align:right; }

.bp3-dark{
  color:#f5f8fa; }

:focus{
  outline:rgba(19, 124, 189, 0.6) auto 2px;
  outline-offset:2px;
  -moz-outline-radius:6px; }

.bp3-focus-disabled :focus{
  outline:none !important; }
  .bp3-focus-disabled :focus ~ .bp3-control-indicator{
    outline:none !important; }

.bp3-alert{
  max-width:400px;
  padding:20px; }

.bp3-alert-body{
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex; }
  .bp3-alert-body .bp3-icon{
    font-size:40px;
    margin-right:20px;
    margin-top:0; }

.bp3-alert-contents{
  word-break:break-word; }

.bp3-alert-footer{
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  -webkit-box-orient:horizontal;
  -webkit-box-direction:reverse;
      -ms-flex-direction:row-reverse;
          flex-direction:row-reverse;
  margin-top:10px; }
  .bp3-alert-footer .bp3-button{
    margin-left:10px; }
.bp3-breadcrumbs{
  -webkit-box-align:center;
      -ms-flex-align:center;
          align-items:center;
  cursor:default;
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  -ms-flex-wrap:wrap;
      flex-wrap:wrap;
  height:30px;
  list-style:none;
  margin:0;
  padding:0; }
  .bp3-breadcrumbs > li{
    -webkit-box-align:center;
        -ms-flex-align:center;
            align-items:center;
    display:-webkit-box;
    display:-ms-flexbox;
    display:flex; }
    .bp3-breadcrumbs > li::after{
      background:url("data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 16 16'%3e%3cpath fill-rule='evenodd' clip-rule='evenodd' d='M10.71 7.29l-4-4a1.003 1.003 0 00-1.42 1.42L8.59 8 5.3 11.29c-.19.18-.3.43-.3.71a1.003 1.003 0 001.71.71l4-4c.18-.18.29-.43.29-.71 0-.28-.11-.53-.29-.71z' fill='%235C7080'/%3e%3c/svg%3e");
      content:"";
      display:block;
      height:16px;
      margin:0 5px;
      width:16px; }
    .bp3-breadcrumbs > li:last-of-type::after{
      display:none; }

.bp3-breadcrumb,
.bp3-breadcrumb-current,
.bp3-breadcrumbs-collapsed{
  -webkit-box-align:center;
      -ms-flex-align:center;
          align-items:center;
  display:-webkit-inline-box;
  display:-ms-inline-flexbox;
  display:inline-flex;
  font-size:16px; }

.bp3-breadcrumb,
.bp3-breadcrumbs-collapsed{
  color:#5c7080; }

.bp3-breadcrumb:hover{
  text-decoration:none; }

.bp3-breadcrumb.bp3-disabled{
  color:rgba(92, 112, 128, 0.6);
  cursor:not-allowed; }

.bp3-breadcrumb .bp3-icon{
  margin-right:5px; }

.bp3-breadcrumb-current{
  color:inherit;
  font-weight:600; }
  .bp3-breadcrumb-current .bp3-input{
    font-size:inherit;
    font-weight:inherit;
    vertical-align:baseline; }

.bp3-breadcrumbs-collapsed{
  background:#ced9e0;
  border:none;
  border-radius:3px;
  cursor:pointer;
  margin-right:2px;
  padding:1px 5px;
  vertical-align:text-bottom; }
  .bp3-breadcrumbs-collapsed::before{
    background:url("data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 16 16'%3e%3cg fill='%235C7080'%3e%3ccircle cx='2' cy='8.03' r='2'/%3e%3ccircle cx='14' cy='8.03' r='2'/%3e%3ccircle cx='8' cy='8.03' r='2'/%3e%3c/g%3e%3c/svg%3e") center no-repeat;
    content:"";
    display:block;
    height:16px;
    width:16px; }
  .bp3-breadcrumbs-collapsed:hover{
    background:#bfccd6;
    color:#182026;
    text-decoration:none; }

.bp3-dark .bp3-breadcrumb,
.bp3-dark .bp3-breadcrumbs-collapsed{
  color:#a7b6c2; }

.bp3-dark .bp3-breadcrumbs > li::after{
  color:#a7b6c2; }

.bp3-dark .bp3-breadcrumb.bp3-disabled{
  color:rgba(167, 182, 194, 0.6); }

.bp3-dark .bp3-breadcrumb-current{
  color:#f5f8fa; }

.bp3-dark .bp3-breadcrumbs-collapsed{
  background:rgba(16, 22, 26, 0.4); }
  .bp3-dark .bp3-breadcrumbs-collapsed:hover{
    background:rgba(16, 22, 26, 0.6);
    color:#f5f8fa; }
.bp3-button{
  display:-webkit-inline-box;
  display:-ms-inline-flexbox;
  display:inline-flex;
  -webkit-box-orient:horizontal;
  -webkit-box-direction:normal;
      -ms-flex-direction:row;
          flex-direction:row;
  -webkit-box-align:center;
      -ms-flex-align:center;
          align-items:center;
  border:none;
  border-radius:3px;
  cursor:pointer;
  font-size:14px;
  -webkit-box-pack:center;
      -ms-flex-pack:center;
          justify-content:center;
  padding:5px 10px;
  text-align:left;
  vertical-align:middle;
  min-height:30px;
  min-width:30px; }
  .bp3-button > *{
    -webkit-box-flex:0;
        -ms-flex-positive:0;
            flex-grow:0;
    -ms-flex-negative:0;
        flex-shrink:0; }
  .bp3-button > .bp3-fill{
    -webkit-box-flex:1;
        -ms-flex-positive:1;
            flex-grow:1;
    -ms-flex-negative:1;
        flex-shrink:1; }
  .bp3-button::before,
  .bp3-button > *{
    margin-right:7px; }
  .bp3-button:empty::before,
  .bp3-button > :last-child{
    margin-right:0; }
  .bp3-button:empty{
    padding:0 !important; }
  .bp3-button:disabled, .bp3-button.bp3-disabled{
    cursor:not-allowed; }
  .bp3-button.bp3-fill{
    display:-webkit-box;
    display:-ms-flexbox;
    display:flex;
    width:100%; }
  .bp3-button.bp3-align-right,
  .bp3-align-right .bp3-button{
    text-align:right; }
  .bp3-button.bp3-align-left,
  .bp3-align-left .bp3-button{
    text-align:left; }
  .bp3-button:not([class*="bp3-intent-"]){
    background-color:#f5f8fa;
    background-image:-webkit-gradient(linear, left top, left bottom, from(rgba(255, 255, 255, 0.8)), to(rgba(255, 255, 255, 0)));
    background-image:linear-gradient(to bottom, rgba(255, 255, 255, 0.8), rgba(255, 255, 255, 0));
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1);
    color:#182026; }
    .bp3-button:not([class*="bp3-intent-"]):hover{
      background-clip:padding-box;
      background-color:#ebf1f5;
      -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1);
              box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1); }
    .bp3-button:not([class*="bp3-intent-"]):active, .bp3-button:not([class*="bp3-intent-"]).bp3-active{
      background-color:#d8e1e8;
      background-image:none;
      -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 1px 2px rgba(16, 22, 26, 0.2);
              box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 1px 2px rgba(16, 22, 26, 0.2); }
    .bp3-button:not([class*="bp3-intent-"]):disabled, .bp3-button:not([class*="bp3-intent-"]).bp3-disabled{
      background-color:rgba(206, 217, 224, 0.5);
      background-image:none;
      -webkit-box-shadow:none;
              box-shadow:none;
      color:rgba(92, 112, 128, 0.6);
      cursor:not-allowed;
      outline:none; }
      .bp3-button:not([class*="bp3-intent-"]):disabled.bp3-active, .bp3-button:not([class*="bp3-intent-"]):disabled.bp3-active:hover, .bp3-button:not([class*="bp3-intent-"]).bp3-disabled.bp3-active, .bp3-button:not([class*="bp3-intent-"]).bp3-disabled.bp3-active:hover{
        background:rgba(206, 217, 224, 0.7); }
  .bp3-button.bp3-intent-primary{
    background-color:#137cbd;
    background-image:-webkit-gradient(linear, left top, left bottom, from(rgba(255, 255, 255, 0.1)), to(rgba(255, 255, 255, 0)));
    background-image:linear-gradient(to bottom, rgba(255, 255, 255, 0.1), rgba(255, 255, 255, 0));
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2);
    color:#ffffff; }
    .bp3-button.bp3-intent-primary:hover, .bp3-button.bp3-intent-primary:active, .bp3-button.bp3-intent-primary.bp3-active{
      color:#ffffff; }
    .bp3-button.bp3-intent-primary:hover{
      background-color:#106ba3;
      -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2);
              box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2); }
    .bp3-button.bp3-intent-primary:active, .bp3-button.bp3-intent-primary.bp3-active{
      background-color:#0e5a8a;
      background-image:none;
      -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 1px 2px rgba(16, 22, 26, 0.2);
              box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 1px 2px rgba(16, 22, 26, 0.2); }
    .bp3-button.bp3-intent-primary:disabled, .bp3-button.bp3-intent-primary.bp3-disabled{
      background-color:rgba(19, 124, 189, 0.5);
      background-image:none;
      border-color:transparent;
      -webkit-box-shadow:none;
              box-shadow:none;
      color:rgba(255, 255, 255, 0.6); }
  .bp3-button.bp3-intent-success{
    background-color:#0f9960;
    background-image:-webkit-gradient(linear, left top, left bottom, from(rgba(255, 255, 255, 0.1)), to(rgba(255, 255, 255, 0)));
    background-image:linear-gradient(to bottom, rgba(255, 255, 255, 0.1), rgba(255, 255, 255, 0));
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2);
    color:#ffffff; }
    .bp3-button.bp3-intent-success:hover, .bp3-button.bp3-intent-success:active, .bp3-button.bp3-intent-success.bp3-active{
      color:#ffffff; }
    .bp3-button.bp3-intent-success:hover{
      background-color:#0d8050;
      -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2);
              box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2); }
    .bp3-button.bp3-intent-success:active, .bp3-button.bp3-intent-success.bp3-active{
      background-color:#0a6640;
      background-image:none;
      -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 1px 2px rgba(16, 22, 26, 0.2);
              box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 1px 2px rgba(16, 22, 26, 0.2); }
    .bp3-button.bp3-intent-success:disabled, .bp3-button.bp3-intent-success.bp3-disabled{
      background-color:rgba(15, 153, 96, 0.5);
      background-image:none;
      border-color:transparent;
      -webkit-box-shadow:none;
              box-shadow:none;
      color:rgba(255, 255, 255, 0.6); }
  .bp3-button.bp3-intent-warning{
    background-color:#d9822b;
    background-image:-webkit-gradient(linear, left top, left bottom, from(rgba(255, 255, 255, 0.1)), to(rgba(255, 255, 255, 0)));
    background-image:linear-gradient(to bottom, rgba(255, 255, 255, 0.1), rgba(255, 255, 255, 0));
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2);
    color:#ffffff; }
    .bp3-button.bp3-intent-warning:hover, .bp3-button.bp3-intent-warning:active, .bp3-button.bp3-intent-warning.bp3-active{
      color:#ffffff; }
    .bp3-button.bp3-intent-warning:hover{
      background-color:#bf7326;
      -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2);
              box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2); }
    .bp3-button.bp3-intent-warning:active, .bp3-button.bp3-intent-warning.bp3-active{
      background-color:#a66321;
      background-image:none;
      -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 1px 2px rgba(16, 22, 26, 0.2);
              box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 1px 2px rgba(16, 22, 26, 0.2); }
    .bp3-button.bp3-intent-warning:disabled, .bp3-button.bp3-intent-warning.bp3-disabled{
      background-color:rgba(217, 130, 43, 0.5);
      background-image:none;
      border-color:transparent;
      -webkit-box-shadow:none;
              box-shadow:none;
      color:rgba(255, 255, 255, 0.6); }
  .bp3-button.bp3-intent-danger{
    background-color:#db3737;
    background-image:-webkit-gradient(linear, left top, left bottom, from(rgba(255, 255, 255, 0.1)), to(rgba(255, 255, 255, 0)));
    background-image:linear-gradient(to bottom, rgba(255, 255, 255, 0.1), rgba(255, 255, 255, 0));
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2);
    color:#ffffff; }
    .bp3-button.bp3-intent-danger:hover, .bp3-button.bp3-intent-danger:active, .bp3-button.bp3-intent-danger.bp3-active{
      color:#ffffff; }
    .bp3-button.bp3-intent-danger:hover{
      background-color:#c23030;
      -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2);
              box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2); }
    .bp3-button.bp3-intent-danger:active, .bp3-button.bp3-intent-danger.bp3-active{
      background-color:#a82a2a;
      background-image:none;
      -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 1px 2px rgba(16, 22, 26, 0.2);
              box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 1px 2px rgba(16, 22, 26, 0.2); }
    .bp3-button.bp3-intent-danger:disabled, .bp3-button.bp3-intent-danger.bp3-disabled{
      background-color:rgba(219, 55, 55, 0.5);
      background-image:none;
      border-color:transparent;
      -webkit-box-shadow:none;
              box-shadow:none;
      color:rgba(255, 255, 255, 0.6); }
  .bp3-button[class*="bp3-intent-"] .bp3-button-spinner .bp3-spinner-head{
    stroke:#ffffff; }
  .bp3-button.bp3-large,
  .bp3-large .bp3-button{
    min-height:40px;
    min-width:40px;
    font-size:16px;
    padding:5px 15px; }
    .bp3-button.bp3-large::before,
    .bp3-button.bp3-large > *,
    .bp3-large .bp3-button::before,
    .bp3-large .bp3-button > *{
      margin-right:10px; }
    .bp3-button.bp3-large:empty::before,
    .bp3-button.bp3-large > :last-child,
    .bp3-large .bp3-button:empty::before,
    .bp3-large .bp3-button > :last-child{
      margin-right:0; }
  .bp3-button.bp3-small,
  .bp3-small .bp3-button{
    min-height:24px;
    min-width:24px;
    padding:0 7px; }
  .bp3-button.bp3-loading{
    position:relative; }
    .bp3-button.bp3-loading[class*="bp3-icon-"]::before{
      visibility:hidden; }
    .bp3-button.bp3-loading .bp3-button-spinner{
      margin:0;
      position:absolute; }
    .bp3-button.bp3-loading > :not(.bp3-button-spinner){
      visibility:hidden; }
  .bp3-button[class*="bp3-icon-"]::before{
    font-family:"Icons16", sans-serif;
    font-size:16px;
    font-style:normal;
    font-weight:400;
    line-height:1;
    -moz-osx-font-smoothing:grayscale;
    -webkit-font-smoothing:antialiased;
    color:#5c7080; }
  .bp3-button .bp3-icon, .bp3-button .bp3-icon-standard, .bp3-button .bp3-icon-large{
    color:#5c7080; }
    .bp3-button .bp3-icon.bp3-align-right, .bp3-button .bp3-icon-standard.bp3-align-right, .bp3-button .bp3-icon-large.bp3-align-right{
      margin-left:7px; }
  .bp3-button .bp3-icon:first-child:last-child,
  .bp3-button .bp3-spinner + .bp3-icon:last-child{
    margin:0 -7px; }
  .bp3-dark .bp3-button:not([class*="bp3-intent-"]){
    background-color:#394b59;
    background-image:-webkit-gradient(linear, left top, left bottom, from(rgba(255, 255, 255, 0.05)), to(rgba(255, 255, 255, 0)));
    background-image:linear-gradient(to bottom, rgba(255, 255, 255, 0.05), rgba(255, 255, 255, 0));
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4);
    color:#f5f8fa; }
    .bp3-dark .bp3-button:not([class*="bp3-intent-"]):hover, .bp3-dark .bp3-button:not([class*="bp3-intent-"]):active, .bp3-dark .bp3-button:not([class*="bp3-intent-"]).bp3-active{
      color:#f5f8fa; }
    .bp3-dark .bp3-button:not([class*="bp3-intent-"]):hover{
      background-color:#30404d;
      -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4);
              box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4); }
    .bp3-dark .bp3-button:not([class*="bp3-intent-"]):active, .bp3-dark .bp3-button:not([class*="bp3-intent-"]).bp3-active{
      background-color:#202b33;
      background-image:none;
      -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.6), inset 0 1px 2px rgba(16, 22, 26, 0.2);
              box-shadow:0 0 0 1px rgba(16, 22, 26, 0.6), inset 0 1px 2px rgba(16, 22, 26, 0.2); }
    .bp3-dark .bp3-button:not([class*="bp3-intent-"]):disabled, .bp3-dark .bp3-button:not([class*="bp3-intent-"]).bp3-disabled{
      background-color:rgba(57, 75, 89, 0.5);
      background-image:none;
      -webkit-box-shadow:none;
              box-shadow:none;
      color:rgba(167, 182, 194, 0.6); }
      .bp3-dark .bp3-button:not([class*="bp3-intent-"]):disabled.bp3-active, .bp3-dark .bp3-button:not([class*="bp3-intent-"]).bp3-disabled.bp3-active{
        background:rgba(57, 75, 89, 0.7); }
    .bp3-dark .bp3-button:not([class*="bp3-intent-"]) .bp3-button-spinner .bp3-spinner-head{
      background:rgba(16, 22, 26, 0.5);
      stroke:#8a9ba8; }
    .bp3-dark .bp3-button:not([class*="bp3-intent-"])[class*="bp3-icon-"]::before{
      color:#a7b6c2; }
    .bp3-dark .bp3-button:not([class*="bp3-intent-"]) .bp3-icon, .bp3-dark .bp3-button:not([class*="bp3-intent-"]) .bp3-icon-standard, .bp3-dark .bp3-button:not([class*="bp3-intent-"]) .bp3-icon-large{
      color:#a7b6c2; }
  .bp3-dark .bp3-button[class*="bp3-intent-"]{
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4); }
    .bp3-dark .bp3-button[class*="bp3-intent-"]:hover{
      -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4);
              box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4); }
    .bp3-dark .bp3-button[class*="bp3-intent-"]:active, .bp3-dark .bp3-button[class*="bp3-intent-"].bp3-active{
      -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 1px 2px rgba(16, 22, 26, 0.2);
              box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 1px 2px rgba(16, 22, 26, 0.2); }
    .bp3-dark .bp3-button[class*="bp3-intent-"]:disabled, .bp3-dark .bp3-button[class*="bp3-intent-"].bp3-disabled{
      background-image:none;
      -webkit-box-shadow:none;
              box-shadow:none;
      color:rgba(255, 255, 255, 0.3); }
    .bp3-dark .bp3-button[class*="bp3-intent-"] .bp3-button-spinner .bp3-spinner-head{
      stroke:#8a9ba8; }
  .bp3-button:disabled::before,
  .bp3-button:disabled .bp3-icon, .bp3-button:disabled .bp3-icon-standard, .bp3-button:disabled .bp3-icon-large, .bp3-button.bp3-disabled::before,
  .bp3-button.bp3-disabled .bp3-icon, .bp3-button.bp3-disabled .bp3-icon-standard, .bp3-button.bp3-disabled .bp3-icon-large, .bp3-button[class*="bp3-intent-"]::before,
  .bp3-button[class*="bp3-intent-"] .bp3-icon, .bp3-button[class*="bp3-intent-"] .bp3-icon-standard, .bp3-button[class*="bp3-intent-"] .bp3-icon-large{
    color:inherit !important; }
  .bp3-button.bp3-minimal{
    background:none;
    -webkit-box-shadow:none;
            box-shadow:none; }
    .bp3-button.bp3-minimal:hover{
      background:rgba(167, 182, 194, 0.3);
      -webkit-box-shadow:none;
              box-shadow:none;
      color:#182026;
      text-decoration:none; }
    .bp3-button.bp3-minimal:active, .bp3-button.bp3-minimal.bp3-active{
      background:rgba(115, 134, 148, 0.3);
      -webkit-box-shadow:none;
              box-shadow:none;
      color:#182026; }
    .bp3-button.bp3-minimal:disabled, .bp3-button.bp3-minimal:disabled:hover, .bp3-button.bp3-minimal.bp3-disabled, .bp3-button.bp3-minimal.bp3-disabled:hover{
      background:none;
      color:rgba(92, 112, 128, 0.6);
      cursor:not-allowed; }
      .bp3-button.bp3-minimal:disabled.bp3-active, .bp3-button.bp3-minimal:disabled:hover.bp3-active, .bp3-button.bp3-minimal.bp3-disabled.bp3-active, .bp3-button.bp3-minimal.bp3-disabled:hover.bp3-active{
        background:rgba(115, 134, 148, 0.3); }
    .bp3-dark .bp3-button.bp3-minimal{
      background:none;
      -webkit-box-shadow:none;
              box-shadow:none;
      color:inherit; }
      .bp3-dark .bp3-button.bp3-minimal:hover, .bp3-dark .bp3-button.bp3-minimal:active, .bp3-dark .bp3-button.bp3-minimal.bp3-active{
        background:none;
        -webkit-box-shadow:none;
                box-shadow:none; }
      .bp3-dark .bp3-button.bp3-minimal:hover{
        background:rgba(138, 155, 168, 0.15); }
      .bp3-dark .bp3-button.bp3-minimal:active, .bp3-dark .bp3-button.bp3-minimal.bp3-active{
        background:rgba(138, 155, 168, 0.3);
        color:#f5f8fa; }
      .bp3-dark .bp3-button.bp3-minimal:disabled, .bp3-dark .bp3-button.bp3-minimal:disabled:hover, .bp3-dark .bp3-button.bp3-minimal.bp3-disabled, .bp3-dark .bp3-button.bp3-minimal.bp3-disabled:hover{
        background:none;
        color:rgba(167, 182, 194, 0.6);
        cursor:not-allowed; }
        .bp3-dark .bp3-button.bp3-minimal:disabled.bp3-active, .bp3-dark .bp3-button.bp3-minimal:disabled:hover.bp3-active, .bp3-dark .bp3-button.bp3-minimal.bp3-disabled.bp3-active, .bp3-dark .bp3-button.bp3-minimal.bp3-disabled:hover.bp3-active{
          background:rgba(138, 155, 168, 0.3); }
    .bp3-button.bp3-minimal.bp3-intent-primary{
      color:#106ba3; }
      .bp3-button.bp3-minimal.bp3-intent-primary:hover, .bp3-button.bp3-minimal.bp3-intent-primary:active, .bp3-button.bp3-minimal.bp3-intent-primary.bp3-active{
        background:none;
        -webkit-box-shadow:none;
                box-shadow:none;
        color:#106ba3; }
      .bp3-button.bp3-minimal.bp3-intent-primary:hover{
        background:rgba(19, 124, 189, 0.15);
        color:#106ba3; }
      .bp3-button.bp3-minimal.bp3-intent-primary:active, .bp3-button.bp3-minimal.bp3-intent-primary.bp3-active{
        background:rgba(19, 124, 189, 0.3);
        color:#106ba3; }
      .bp3-button.bp3-minimal.bp3-intent-primary:disabled, .bp3-button.bp3-minimal.bp3-intent-primary.bp3-disabled{
        background:none;
        color:rgba(16, 107, 163, 0.5); }
        .bp3-button.bp3-minimal.bp3-intent-primary:disabled.bp3-active, .bp3-button.bp3-minimal.bp3-intent-primary.bp3-disabled.bp3-active{
          background:rgba(19, 124, 189, 0.3); }
      .bp3-button.bp3-minimal.bp3-intent-primary .bp3-button-spinner .bp3-spinner-head{
        stroke:#106ba3; }
      .bp3-dark .bp3-button.bp3-minimal.bp3-intent-primary{
        color:#48aff0; }
        .bp3-dark .bp3-button.bp3-minimal.bp3-intent-primary:hover{
          background:rgba(19, 124, 189, 0.2);
          color:#48aff0; }
        .bp3-dark .bp3-button.bp3-minimal.bp3-intent-primary:active, .bp3-dark .bp3-button.bp3-minimal.bp3-intent-primary.bp3-active{
          background:rgba(19, 124, 189, 0.3);
          color:#48aff0; }
        .bp3-dark .bp3-button.bp3-minimal.bp3-intent-primary:disabled, .bp3-dark .bp3-button.bp3-minimal.bp3-intent-primary.bp3-disabled{
          background:none;
          color:rgba(72, 175, 240, 0.5); }
          .bp3-dark .bp3-button.bp3-minimal.bp3-intent-primary:disabled.bp3-active, .bp3-dark .bp3-button.bp3-minimal.bp3-intent-primary.bp3-disabled.bp3-active{
            background:rgba(19, 124, 189, 0.3); }
    .bp3-button.bp3-minimal.bp3-intent-success{
      color:#0d8050; }
      .bp3-button.bp3-minimal.bp3-intent-success:hover, .bp3-button.bp3-minimal.bp3-intent-success:active, .bp3-button.bp3-minimal.bp3-intent-success.bp3-active{
        background:none;
        -webkit-box-shadow:none;
                box-shadow:none;
        color:#0d8050; }
      .bp3-button.bp3-minimal.bp3-intent-success:hover{
        background:rgba(15, 153, 96, 0.15);
        color:#0d8050; }
      .bp3-button.bp3-minimal.bp3-intent-success:active, .bp3-button.bp3-minimal.bp3-intent-success.bp3-active{
        background:rgba(15, 153, 96, 0.3);
        color:#0d8050; }
      .bp3-button.bp3-minimal.bp3-intent-success:disabled, .bp3-button.bp3-minimal.bp3-intent-success.bp3-disabled{
        background:none;
        color:rgba(13, 128, 80, 0.5); }
        .bp3-button.bp3-minimal.bp3-intent-success:disabled.bp3-active, .bp3-button.bp3-minimal.bp3-intent-success.bp3-disabled.bp3-active{
          background:rgba(15, 153, 96, 0.3); }
      .bp3-button.bp3-minimal.bp3-intent-success .bp3-button-spinner .bp3-spinner-head{
        stroke:#0d8050; }
      .bp3-dark .bp3-button.bp3-minimal.bp3-intent-success{
        color:#3dcc91; }
        .bp3-dark .bp3-button.bp3-minimal.bp3-intent-success:hover{
          background:rgba(15, 153, 96, 0.2);
          color:#3dcc91; }
        .bp3-dark .bp3-button.bp3-minimal.bp3-intent-success:active, .bp3-dark .bp3-button.bp3-minimal.bp3-intent-success.bp3-active{
          background:rgba(15, 153, 96, 0.3);
          color:#3dcc91; }
        .bp3-dark .bp3-button.bp3-minimal.bp3-intent-success:disabled, .bp3-dark .bp3-button.bp3-minimal.bp3-intent-success.bp3-disabled{
          background:none;
          color:rgba(61, 204, 145, 0.5); }
          .bp3-dark .bp3-button.bp3-minimal.bp3-intent-success:disabled.bp3-active, .bp3-dark .bp3-button.bp3-minimal.bp3-intent-success.bp3-disabled.bp3-active{
            background:rgba(15, 153, 96, 0.3); }
    .bp3-button.bp3-minimal.bp3-intent-warning{
      color:#bf7326; }
      .bp3-button.bp3-minimal.bp3-intent-warning:hover, .bp3-button.bp3-minimal.bp3-intent-warning:active, .bp3-button.bp3-minimal.bp3-intent-warning.bp3-active{
        background:none;
        -webkit-box-shadow:none;
                box-shadow:none;
        color:#bf7326; }
      .bp3-button.bp3-minimal.bp3-intent-warning:hover{
        background:rgba(217, 130, 43, 0.15);
        color:#bf7326; }
      .bp3-button.bp3-minimal.bp3-intent-warning:active, .bp3-button.bp3-minimal.bp3-intent-warning.bp3-active{
        background:rgba(217, 130, 43, 0.3);
        color:#bf7326; }
      .bp3-button.bp3-minimal.bp3-intent-warning:disabled, .bp3-button.bp3-minimal.bp3-intent-warning.bp3-disabled{
        background:none;
        color:rgba(191, 115, 38, 0.5); }
        .bp3-button.bp3-minimal.bp3-intent-warning:disabled.bp3-active, .bp3-button.bp3-minimal.bp3-intent-warning.bp3-disabled.bp3-active{
          background:rgba(217, 130, 43, 0.3); }
      .bp3-button.bp3-minimal.bp3-intent-warning .bp3-button-spinner .bp3-spinner-head{
        stroke:#bf7326; }
      .bp3-dark .bp3-button.bp3-minimal.bp3-intent-warning{
        color:#ffb366; }
        .bp3-dark .bp3-button.bp3-minimal.bp3-intent-warning:hover{
          background:rgba(217, 130, 43, 0.2);
          color:#ffb366; }
        .bp3-dark .bp3-button.bp3-minimal.bp3-intent-warning:active, .bp3-dark .bp3-button.bp3-minimal.bp3-intent-warning.bp3-active{
          background:rgba(217, 130, 43, 0.3);
          color:#ffb366; }
        .bp3-dark .bp3-button.bp3-minimal.bp3-intent-warning:disabled, .bp3-dark .bp3-button.bp3-minimal.bp3-intent-warning.bp3-disabled{
          background:none;
          color:rgba(255, 179, 102, 0.5); }
          .bp3-dark .bp3-button.bp3-minimal.bp3-intent-warning:disabled.bp3-active, .bp3-dark .bp3-button.bp3-minimal.bp3-intent-warning.bp3-disabled.bp3-active{
            background:rgba(217, 130, 43, 0.3); }
    .bp3-button.bp3-minimal.bp3-intent-danger{
      color:#c23030; }
      .bp3-button.bp3-minimal.bp3-intent-danger:hover, .bp3-button.bp3-minimal.bp3-intent-danger:active, .bp3-button.bp3-minimal.bp3-intent-danger.bp3-active{
        background:none;
        -webkit-box-shadow:none;
                box-shadow:none;
        color:#c23030; }
      .bp3-button.bp3-minimal.bp3-intent-danger:hover{
        background:rgba(219, 55, 55, 0.15);
        color:#c23030; }
      .bp3-button.bp3-minimal.bp3-intent-danger:active, .bp3-button.bp3-minimal.bp3-intent-danger.bp3-active{
        background:rgba(219, 55, 55, 0.3);
        color:#c23030; }
      .bp3-button.bp3-minimal.bp3-intent-danger:disabled, .bp3-button.bp3-minimal.bp3-intent-danger.bp3-disabled{
        background:none;
        color:rgba(194, 48, 48, 0.5); }
        .bp3-button.bp3-minimal.bp3-intent-danger:disabled.bp3-active, .bp3-button.bp3-minimal.bp3-intent-danger.bp3-disabled.bp3-active{
          background:rgba(219, 55, 55, 0.3); }
      .bp3-button.bp3-minimal.bp3-intent-danger .bp3-button-spinner .bp3-spinner-head{
        stroke:#c23030; }
      .bp3-dark .bp3-button.bp3-minimal.bp3-intent-danger{
        color:#ff7373; }
        .bp3-dark .bp3-button.bp3-minimal.bp3-intent-danger:hover{
          background:rgba(219, 55, 55, 0.2);
          color:#ff7373; }
        .bp3-dark .bp3-button.bp3-minimal.bp3-intent-danger:active, .bp3-dark .bp3-button.bp3-minimal.bp3-intent-danger.bp3-active{
          background:rgba(219, 55, 55, 0.3);
          color:#ff7373; }
        .bp3-dark .bp3-button.bp3-minimal.bp3-intent-danger:disabled, .bp3-dark .bp3-button.bp3-minimal.bp3-intent-danger.bp3-disabled{
          background:none;
          color:rgba(255, 115, 115, 0.5); }
          .bp3-dark .bp3-button.bp3-minimal.bp3-intent-danger:disabled.bp3-active, .bp3-dark .bp3-button.bp3-minimal.bp3-intent-danger.bp3-disabled.bp3-active{
            background:rgba(219, 55, 55, 0.3); }
  .bp3-button.bp3-outlined{
    background:none;
    -webkit-box-shadow:none;
            box-shadow:none;
    border:1px solid rgba(24, 32, 38, 0.2);
    -webkit-box-sizing:border-box;
            box-sizing:border-box; }
    .bp3-button.bp3-outlined:hover{
      background:rgba(167, 182, 194, 0.3);
      -webkit-box-shadow:none;
              box-shadow:none;
      color:#182026;
      text-decoration:none; }
    .bp3-button.bp3-outlined:active, .bp3-button.bp3-outlined.bp3-active{
      background:rgba(115, 134, 148, 0.3);
      -webkit-box-shadow:none;
              box-shadow:none;
      color:#182026; }
    .bp3-button.bp3-outlined:disabled, .bp3-button.bp3-outlined:disabled:hover, .bp3-button.bp3-outlined.bp3-disabled, .bp3-button.bp3-outlined.bp3-disabled:hover{
      background:none;
      color:rgba(92, 112, 128, 0.6);
      cursor:not-allowed; }
      .bp3-button.bp3-outlined:disabled.bp3-active, .bp3-button.bp3-outlined:disabled:hover.bp3-active, .bp3-button.bp3-outlined.bp3-disabled.bp3-active, .bp3-button.bp3-outlined.bp3-disabled:hover.bp3-active{
        background:rgba(115, 134, 148, 0.3); }
    .bp3-dark .bp3-button.bp3-outlined{
      background:none;
      -webkit-box-shadow:none;
              box-shadow:none;
      color:inherit; }
      .bp3-dark .bp3-button.bp3-outlined:hover, .bp3-dark .bp3-button.bp3-outlined:active, .bp3-dark .bp3-button.bp3-outlined.bp3-active{
        background:none;
        -webkit-box-shadow:none;
                box-shadow:none; }
      .bp3-dark .bp3-button.bp3-outlined:hover{
        background:rgba(138, 155, 168, 0.15); }
      .bp3-dark .bp3-button.bp3-outlined:active, .bp3-dark .bp3-button.bp3-outlined.bp3-active{
        background:rgba(138, 155, 168, 0.3);
        color:#f5f8fa; }
      .bp3-dark .bp3-button.bp3-outlined:disabled, .bp3-dark .bp3-button.bp3-outlined:disabled:hover, .bp3-dark .bp3-button.bp3-outlined.bp3-disabled, .bp3-dark .bp3-button.bp3-outlined.bp3-disabled:hover{
        background:none;
        color:rgba(167, 182, 194, 0.6);
        cursor:not-allowed; }
        .bp3-dark .bp3-button.bp3-outlined:disabled.bp3-active, .bp3-dark .bp3-button.bp3-outlined:disabled:hover.bp3-active, .bp3-dark .bp3-button.bp3-outlined.bp3-disabled.bp3-active, .bp3-dark .bp3-button.bp3-outlined.bp3-disabled:hover.bp3-active{
          background:rgba(138, 155, 168, 0.3); }
    .bp3-button.bp3-outlined.bp3-intent-primary{
      color:#106ba3; }
      .bp3-button.bp3-outlined.bp3-intent-primary:hover, .bp3-button.bp3-outlined.bp3-intent-primary:active, .bp3-button.bp3-outlined.bp3-intent-primary.bp3-active{
        background:none;
        -webkit-box-shadow:none;
                box-shadow:none;
        color:#106ba3; }
      .bp3-button.bp3-outlined.bp3-intent-primary:hover{
        background:rgba(19, 124, 189, 0.15);
        color:#106ba3; }
      .bp3-button.bp3-outlined.bp3-intent-primary:active, .bp3-button.bp3-outlined.bp3-intent-primary.bp3-active{
        background:rgba(19, 124, 189, 0.3);
        color:#106ba3; }
      .bp3-button.bp3-outlined.bp3-intent-primary:disabled, .bp3-button.bp3-outlined.bp3-intent-primary.bp3-disabled{
        background:none;
        color:rgba(16, 107, 163, 0.5); }
        .bp3-button.bp3-outlined.bp3-intent-primary:disabled.bp3-active, .bp3-button.bp3-outlined.bp3-intent-primary.bp3-disabled.bp3-active{
          background:rgba(19, 124, 189, 0.3); }
      .bp3-button.bp3-outlined.bp3-intent-primary .bp3-button-spinner .bp3-spinner-head{
        stroke:#106ba3; }
      .bp3-dark .bp3-button.bp3-outlined.bp3-intent-primary{
        color:#48aff0; }
        .bp3-dark .bp3-button.bp3-outlined.bp3-intent-primary:hover{
          background:rgba(19, 124, 189, 0.2);
          color:#48aff0; }
        .bp3-dark .bp3-button.bp3-outlined.bp3-intent-primary:active, .bp3-dark .bp3-button.bp3-outlined.bp3-intent-primary.bp3-active{
          background:rgba(19, 124, 189, 0.3);
          color:#48aff0; }
        .bp3-dark .bp3-button.bp3-outlined.bp3-intent-primary:disabled, .bp3-dark .bp3-button.bp3-outlined.bp3-intent-primary.bp3-disabled{
          background:none;
          color:rgba(72, 175, 240, 0.5); }
          .bp3-dark .bp3-button.bp3-outlined.bp3-intent-primary:disabled.bp3-active, .bp3-dark .bp3-button.bp3-outlined.bp3-intent-primary.bp3-disabled.bp3-active{
            background:rgba(19, 124, 189, 0.3); }
    .bp3-button.bp3-outlined.bp3-intent-success{
      color:#0d8050; }
      .bp3-button.bp3-outlined.bp3-intent-success:hover, .bp3-button.bp3-outlined.bp3-intent-success:active, .bp3-button.bp3-outlined.bp3-intent-success.bp3-active{
        background:none;
        -webkit-box-shadow:none;
                box-shadow:none;
        color:#0d8050; }
      .bp3-button.bp3-outlined.bp3-intent-success:hover{
        background:rgba(15, 153, 96, 0.15);
        color:#0d8050; }
      .bp3-button.bp3-outlined.bp3-intent-success:active, .bp3-button.bp3-outlined.bp3-intent-success.bp3-active{
        background:rgba(15, 153, 96, 0.3);
        color:#0d8050; }
      .bp3-button.bp3-outlined.bp3-intent-success:disabled, .bp3-button.bp3-outlined.bp3-intent-success.bp3-disabled{
        background:none;
        color:rgba(13, 128, 80, 0.5); }
        .bp3-button.bp3-outlined.bp3-intent-success:disabled.bp3-active, .bp3-button.bp3-outlined.bp3-intent-success.bp3-disabled.bp3-active{
          background:rgba(15, 153, 96, 0.3); }
      .bp3-button.bp3-outlined.bp3-intent-success .bp3-button-spinner .bp3-spinner-head{
        stroke:#0d8050; }
      .bp3-dark .bp3-button.bp3-outlined.bp3-intent-success{
        color:#3dcc91; }
        .bp3-dark .bp3-button.bp3-outlined.bp3-intent-success:hover{
          background:rgba(15, 153, 96, 0.2);
          color:#3dcc91; }
        .bp3-dark .bp3-button.bp3-outlined.bp3-intent-success:active, .bp3-dark .bp3-button.bp3-outlined.bp3-intent-success.bp3-active{
          background:rgba(15, 153, 96, 0.3);
          color:#3dcc91; }
        .bp3-dark .bp3-button.bp3-outlined.bp3-intent-success:disabled, .bp3-dark .bp3-button.bp3-outlined.bp3-intent-success.bp3-disabled{
          background:none;
          color:rgba(61, 204, 145, 0.5); }
          .bp3-dark .bp3-button.bp3-outlined.bp3-intent-success:disabled.bp3-active, .bp3-dark .bp3-button.bp3-outlined.bp3-intent-success.bp3-disabled.bp3-active{
            background:rgba(15, 153, 96, 0.3); }
    .bp3-button.bp3-outlined.bp3-intent-warning{
      color:#bf7326; }
      .bp3-button.bp3-outlined.bp3-intent-warning:hover, .bp3-button.bp3-outlined.bp3-intent-warning:active, .bp3-button.bp3-outlined.bp3-intent-warning.bp3-active{
        background:none;
        -webkit-box-shadow:none;
                box-shadow:none;
        color:#bf7326; }
      .bp3-button.bp3-outlined.bp3-intent-warning:hover{
        background:rgba(217, 130, 43, 0.15);
        color:#bf7326; }
      .bp3-button.bp3-outlined.bp3-intent-warning:active, .bp3-button.bp3-outlined.bp3-intent-warning.bp3-active{
        background:rgba(217, 130, 43, 0.3);
        color:#bf7326; }
      .bp3-button.bp3-outlined.bp3-intent-warning:disabled, .bp3-button.bp3-outlined.bp3-intent-warning.bp3-disabled{
        background:none;
        color:rgba(191, 115, 38, 0.5); }
        .bp3-button.bp3-outlined.bp3-intent-warning:disabled.bp3-active, .bp3-button.bp3-outlined.bp3-intent-warning.bp3-disabled.bp3-active{
          background:rgba(217, 130, 43, 0.3); }
      .bp3-button.bp3-outlined.bp3-intent-warning .bp3-button-spinner .bp3-spinner-head{
        stroke:#bf7326; }
      .bp3-dark .bp3-button.bp3-outlined.bp3-intent-warning{
        color:#ffb366; }
        .bp3-dark .bp3-button.bp3-outlined.bp3-intent-warning:hover{
          background:rgba(217, 130, 43, 0.2);
          color:#ffb366; }
        .bp3-dark .bp3-button.bp3-outlined.bp3-intent-warning:active, .bp3-dark .bp3-button.bp3-outlined.bp3-intent-warning.bp3-active{
          background:rgba(217, 130, 43, 0.3);
          color:#ffb366; }
        .bp3-dark .bp3-button.bp3-outlined.bp3-intent-warning:disabled, .bp3-dark .bp3-button.bp3-outlined.bp3-intent-warning.bp3-disabled{
          background:none;
          color:rgba(255, 179, 102, 0.5); }
          .bp3-dark .bp3-button.bp3-outlined.bp3-intent-warning:disabled.bp3-active, .bp3-dark .bp3-button.bp3-outlined.bp3-intent-warning.bp3-disabled.bp3-active{
            background:rgba(217, 130, 43, 0.3); }
    .bp3-button.bp3-outlined.bp3-intent-danger{
      color:#c23030; }
      .bp3-button.bp3-outlined.bp3-intent-danger:hover, .bp3-button.bp3-outlined.bp3-intent-danger:active, .bp3-button.bp3-outlined.bp3-intent-danger.bp3-active{
        background:none;
        -webkit-box-shadow:none;
                box-shadow:none;
        color:#c23030; }
      .bp3-button.bp3-outlined.bp3-intent-danger:hover{
        background:rgba(219, 55, 55, 0.15);
        color:#c23030; }
      .bp3-button.bp3-outlined.bp3-intent-danger:active, .bp3-button.bp3-outlined.bp3-intent-danger.bp3-active{
        background:rgba(219, 55, 55, 0.3);
        color:#c23030; }
      .bp3-button.bp3-outlined.bp3-intent-danger:disabled, .bp3-button.bp3-outlined.bp3-intent-danger.bp3-disabled{
        background:none;
        color:rgba(194, 48, 48, 0.5); }
        .bp3-button.bp3-outlined.bp3-intent-danger:disabled.bp3-active, .bp3-button.bp3-outlined.bp3-intent-danger.bp3-disabled.bp3-active{
          background:rgba(219, 55, 55, 0.3); }
      .bp3-button.bp3-outlined.bp3-intent-danger .bp3-button-spinner .bp3-spinner-head{
        stroke:#c23030; }
      .bp3-dark .bp3-button.bp3-outlined.bp3-intent-danger{
        color:#ff7373; }
        .bp3-dark .bp3-button.bp3-outlined.bp3-intent-danger:hover{
          background:rgba(219, 55, 55, 0.2);
          color:#ff7373; }
        .bp3-dark .bp3-button.bp3-outlined.bp3-intent-danger:active, .bp3-dark .bp3-button.bp3-outlined.bp3-intent-danger.bp3-active{
          background:rgba(219, 55, 55, 0.3);
          color:#ff7373; }
        .bp3-dark .bp3-button.bp3-outlined.bp3-intent-danger:disabled, .bp3-dark .bp3-button.bp3-outlined.bp3-intent-danger.bp3-disabled{
          background:none;
          color:rgba(255, 115, 115, 0.5); }
          .bp3-dark .bp3-button.bp3-outlined.bp3-intent-danger:disabled.bp3-active, .bp3-dark .bp3-button.bp3-outlined.bp3-intent-danger.bp3-disabled.bp3-active{
            background:rgba(219, 55, 55, 0.3); }
    .bp3-button.bp3-outlined:disabled, .bp3-button.bp3-outlined.bp3-disabled, .bp3-button.bp3-outlined:disabled:hover, .bp3-button.bp3-outlined.bp3-disabled:hover{
      border-color:rgba(92, 112, 128, 0.1); }
    .bp3-dark .bp3-button.bp3-outlined{
      border-color:rgba(255, 255, 255, 0.4); }
      .bp3-dark .bp3-button.bp3-outlined:disabled, .bp3-dark .bp3-button.bp3-outlined:disabled:hover, .bp3-dark .bp3-button.bp3-outlined.bp3-disabled, .bp3-dark .bp3-button.bp3-outlined.bp3-disabled:hover{
        border-color:rgba(255, 255, 255, 0.2); }
    .bp3-button.bp3-outlined.bp3-intent-primary{
      border-color:rgba(16, 107, 163, 0.6); }
      .bp3-button.bp3-outlined.bp3-intent-primary:disabled, .bp3-button.bp3-outlined.bp3-intent-primary.bp3-disabled{
        border-color:rgba(16, 107, 163, 0.2); }
      .bp3-dark .bp3-button.bp3-outlined.bp3-intent-primary{
        border-color:rgba(72, 175, 240, 0.6); }
        .bp3-dark .bp3-button.bp3-outlined.bp3-intent-primary:disabled, .bp3-dark .bp3-button.bp3-outlined.bp3-intent-primary.bp3-disabled{
          border-color:rgba(72, 175, 240, 0.2); }
    .bp3-button.bp3-outlined.bp3-intent-success{
      border-color:rgba(13, 128, 80, 0.6); }
      .bp3-button.bp3-outlined.bp3-intent-success:disabled, .bp3-button.bp3-outlined.bp3-intent-success.bp3-disabled{
        border-color:rgba(13, 128, 80, 0.2); }
      .bp3-dark .bp3-button.bp3-outlined.bp3-intent-success{
        border-color:rgba(61, 204, 145, 0.6); }
        .bp3-dark .bp3-button.bp3-outlined.bp3-intent-success:disabled, .bp3-dark .bp3-button.bp3-outlined.bp3-intent-success.bp3-disabled{
          border-color:rgba(61, 204, 145, 0.2); }
    .bp3-button.bp3-outlined.bp3-intent-warning{
      border-color:rgba(191, 115, 38, 0.6); }
      .bp3-button.bp3-outlined.bp3-intent-warning:disabled, .bp3-button.bp3-outlined.bp3-intent-warning.bp3-disabled{
        border-color:rgba(191, 115, 38, 0.2); }
      .bp3-dark .bp3-button.bp3-outlined.bp3-intent-warning{
        border-color:rgba(255, 179, 102, 0.6); }
        .bp3-dark .bp3-button.bp3-outlined.bp3-intent-warning:disabled, .bp3-dark .bp3-button.bp3-outlined.bp3-intent-warning.bp3-disabled{
          border-color:rgba(255, 179, 102, 0.2); }
    .bp3-button.bp3-outlined.bp3-intent-danger{
      border-color:rgba(194, 48, 48, 0.6); }
      .bp3-button.bp3-outlined.bp3-intent-danger:disabled, .bp3-button.bp3-outlined.bp3-intent-danger.bp3-disabled{
        border-color:rgba(194, 48, 48, 0.2); }
      .bp3-dark .bp3-button.bp3-outlined.bp3-intent-danger{
        border-color:rgba(255, 115, 115, 0.6); }
        .bp3-dark .bp3-button.bp3-outlined.bp3-intent-danger:disabled, .bp3-dark .bp3-button.bp3-outlined.bp3-intent-danger.bp3-disabled{
          border-color:rgba(255, 115, 115, 0.2); }

a.bp3-button{
  text-align:center;
  text-decoration:none;
  -webkit-transition:none;
  transition:none; }
  a.bp3-button, a.bp3-button:hover, a.bp3-button:active{
    color:#182026; }
  a.bp3-button.bp3-disabled{
    color:rgba(92, 112, 128, 0.6); }

.bp3-button-text{
  -webkit-box-flex:0;
      -ms-flex:0 1 auto;
          flex:0 1 auto; }

.bp3-button.bp3-align-left .bp3-button-text, .bp3-button.bp3-align-right .bp3-button-text,
.bp3-button-group.bp3-align-left .bp3-button-text,
.bp3-button-group.bp3-align-right .bp3-button-text{
  -webkit-box-flex:1;
      -ms-flex:1 1 auto;
          flex:1 1 auto; }
.bp3-button-group{
  display:-webkit-inline-box;
  display:-ms-inline-flexbox;
  display:inline-flex; }
  .bp3-button-group .bp3-button{
    -webkit-box-flex:0;
        -ms-flex:0 0 auto;
            flex:0 0 auto;
    position:relative;
    z-index:4; }
    .bp3-button-group .bp3-button:focus{
      z-index:5; }
    .bp3-button-group .bp3-button:hover{
      z-index:6; }
    .bp3-button-group .bp3-button:active, .bp3-button-group .bp3-button.bp3-active{
      z-index:7; }
    .bp3-button-group .bp3-button:disabled, .bp3-button-group .bp3-button.bp3-disabled{
      z-index:3; }
    .bp3-button-group .bp3-button[class*="bp3-intent-"]{
      z-index:9; }
      .bp3-button-group .bp3-button[class*="bp3-intent-"]:focus{
        z-index:10; }
      .bp3-button-group .bp3-button[class*="bp3-intent-"]:hover{
        z-index:11; }
      .bp3-button-group .bp3-button[class*="bp3-intent-"]:active, .bp3-button-group .bp3-button[class*="bp3-intent-"].bp3-active{
        z-index:12; }
      .bp3-button-group .bp3-button[class*="bp3-intent-"]:disabled, .bp3-button-group .bp3-button[class*="bp3-intent-"].bp3-disabled{
        z-index:8; }
  .bp3-button-group:not(.bp3-minimal) > .bp3-popover-wrapper:not(:first-child) .bp3-button,
  .bp3-button-group:not(.bp3-minimal) > .bp3-button:not(:first-child){
    border-bottom-left-radius:0;
    border-top-left-radius:0; }
  .bp3-button-group:not(.bp3-minimal) > .bp3-popover-wrapper:not(:last-child) .bp3-button,
  .bp3-button-group:not(.bp3-minimal) > .bp3-button:not(:last-child){
    border-bottom-right-radius:0;
    border-top-right-radius:0;
    margin-right:-1px; }
  .bp3-button-group.bp3-minimal .bp3-button{
    background:none;
    -webkit-box-shadow:none;
            box-shadow:none; }
    .bp3-button-group.bp3-minimal .bp3-button:hover{
      background:rgba(167, 182, 194, 0.3);
      -webkit-box-shadow:none;
              box-shadow:none;
      color:#182026;
      text-decoration:none; }
    .bp3-button-group.bp3-minimal .bp3-button:active, .bp3-button-group.bp3-minimal .bp3-button.bp3-active{
      background:rgba(115, 134, 148, 0.3);
      -webkit-box-shadow:none;
              box-shadow:none;
      color:#182026; }
    .bp3-button-group.bp3-minimal .bp3-button:disabled, .bp3-button-group.bp3-minimal .bp3-button:disabled:hover, .bp3-button-group.bp3-minimal .bp3-button.bp3-disabled, .bp3-button-group.bp3-minimal .bp3-button.bp3-disabled:hover{
      background:none;
      color:rgba(92, 112, 128, 0.6);
      cursor:not-allowed; }
      .bp3-button-group.bp3-minimal .bp3-button:disabled.bp3-active, .bp3-button-group.bp3-minimal .bp3-button:disabled:hover.bp3-active, .bp3-button-group.bp3-minimal .bp3-button.bp3-disabled.bp3-active, .bp3-button-group.bp3-minimal .bp3-button.bp3-disabled:hover.bp3-active{
        background:rgba(115, 134, 148, 0.3); }
    .bp3-dark .bp3-button-group.bp3-minimal .bp3-button{
      background:none;
      -webkit-box-shadow:none;
              box-shadow:none;
      color:inherit; }
      .bp3-dark .bp3-button-group.bp3-minimal .bp3-button:hover, .bp3-dark .bp3-button-group.bp3-minimal .bp3-button:active, .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-active{
        background:none;
        -webkit-box-shadow:none;
                box-shadow:none; }
      .bp3-dark .bp3-button-group.bp3-minimal .bp3-button:hover{
        background:rgba(138, 155, 168, 0.15); }
      .bp3-dark .bp3-button-group.bp3-minimal .bp3-button:active, .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-active{
        background:rgba(138, 155, 168, 0.3);
        color:#f5f8fa; }
      .bp3-dark .bp3-button-group.bp3-minimal .bp3-button:disabled, .bp3-dark .bp3-button-group.bp3-minimal .bp3-button:disabled:hover, .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-disabled, .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-disabled:hover{
        background:none;
        color:rgba(167, 182, 194, 0.6);
        cursor:not-allowed; }
        .bp3-dark .bp3-button-group.bp3-minimal .bp3-button:disabled.bp3-active, .bp3-dark .bp3-button-group.bp3-minimal .bp3-button:disabled:hover.bp3-active, .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-disabled.bp3-active, .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-disabled:hover.bp3-active{
          background:rgba(138, 155, 168, 0.3); }
    .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-primary{
      color:#106ba3; }
      .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-primary:hover, .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-primary:active, .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-primary.bp3-active{
        background:none;
        -webkit-box-shadow:none;
                box-shadow:none;
        color:#106ba3; }
      .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-primary:hover{
        background:rgba(19, 124, 189, 0.15);
        color:#106ba3; }
      .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-primary:active, .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-primary.bp3-active{
        background:rgba(19, 124, 189, 0.3);
        color:#106ba3; }
      .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-primary:disabled, .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-primary.bp3-disabled{
        background:none;
        color:rgba(16, 107, 163, 0.5); }
        .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-primary:disabled.bp3-active, .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-primary.bp3-disabled.bp3-active{
          background:rgba(19, 124, 189, 0.3); }
      .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-primary .bp3-button-spinner .bp3-spinner-head{
        stroke:#106ba3; }
      .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-primary{
        color:#48aff0; }
        .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-primary:hover{
          background:rgba(19, 124, 189, 0.2);
          color:#48aff0; }
        .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-primary:active, .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-primary.bp3-active{
          background:rgba(19, 124, 189, 0.3);
          color:#48aff0; }
        .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-primary:disabled, .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-primary.bp3-disabled{
          background:none;
          color:rgba(72, 175, 240, 0.5); }
          .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-primary:disabled.bp3-active, .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-primary.bp3-disabled.bp3-active{
            background:rgba(19, 124, 189, 0.3); }
    .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-success{
      color:#0d8050; }
      .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-success:hover, .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-success:active, .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-success.bp3-active{
        background:none;
        -webkit-box-shadow:none;
                box-shadow:none;
        color:#0d8050; }
      .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-success:hover{
        background:rgba(15, 153, 96, 0.15);
        color:#0d8050; }
      .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-success:active, .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-success.bp3-active{
        background:rgba(15, 153, 96, 0.3);
        color:#0d8050; }
      .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-success:disabled, .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-success.bp3-disabled{
        background:none;
        color:rgba(13, 128, 80, 0.5); }
        .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-success:disabled.bp3-active, .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-success.bp3-disabled.bp3-active{
          background:rgba(15, 153, 96, 0.3); }
      .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-success .bp3-button-spinner .bp3-spinner-head{
        stroke:#0d8050; }
      .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-success{
        color:#3dcc91; }
        .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-success:hover{
          background:rgba(15, 153, 96, 0.2);
          color:#3dcc91; }
        .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-success:active, .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-success.bp3-active{
          background:rgba(15, 153, 96, 0.3);
          color:#3dcc91; }
        .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-success:disabled, .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-success.bp3-disabled{
          background:none;
          color:rgba(61, 204, 145, 0.5); }
          .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-success:disabled.bp3-active, .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-success.bp3-disabled.bp3-active{
            background:rgba(15, 153, 96, 0.3); }
    .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-warning{
      color:#bf7326; }
      .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-warning:hover, .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-warning:active, .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-warning.bp3-active{
        background:none;
        -webkit-box-shadow:none;
                box-shadow:none;
        color:#bf7326; }
      .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-warning:hover{
        background:rgba(217, 130, 43, 0.15);
        color:#bf7326; }
      .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-warning:active, .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-warning.bp3-active{
        background:rgba(217, 130, 43, 0.3);
        color:#bf7326; }
      .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-warning:disabled, .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-warning.bp3-disabled{
        background:none;
        color:rgba(191, 115, 38, 0.5); }
        .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-warning:disabled.bp3-active, .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-warning.bp3-disabled.bp3-active{
          background:rgba(217, 130, 43, 0.3); }
      .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-warning .bp3-button-spinner .bp3-spinner-head{
        stroke:#bf7326; }
      .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-warning{
        color:#ffb366; }
        .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-warning:hover{
          background:rgba(217, 130, 43, 0.2);
          color:#ffb366; }
        .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-warning:active, .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-warning.bp3-active{
          background:rgba(217, 130, 43, 0.3);
          color:#ffb366; }
        .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-warning:disabled, .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-warning.bp3-disabled{
          background:none;
          color:rgba(255, 179, 102, 0.5); }
          .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-warning:disabled.bp3-active, .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-warning.bp3-disabled.bp3-active{
            background:rgba(217, 130, 43, 0.3); }
    .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-danger{
      color:#c23030; }
      .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-danger:hover, .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-danger:active, .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-danger.bp3-active{
        background:none;
        -webkit-box-shadow:none;
                box-shadow:none;
        color:#c23030; }
      .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-danger:hover{
        background:rgba(219, 55, 55, 0.15);
        color:#c23030; }
      .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-danger:active, .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-danger.bp3-active{
        background:rgba(219, 55, 55, 0.3);
        color:#c23030; }
      .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-danger:disabled, .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-danger.bp3-disabled{
        background:none;
        color:rgba(194, 48, 48, 0.5); }
        .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-danger:disabled.bp3-active, .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-danger.bp3-disabled.bp3-active{
          background:rgba(219, 55, 55, 0.3); }
      .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-danger .bp3-button-spinner .bp3-spinner-head{
        stroke:#c23030; }
      .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-danger{
        color:#ff7373; }
        .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-danger:hover{
          background:rgba(219, 55, 55, 0.2);
          color:#ff7373; }
        .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-danger:active, .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-danger.bp3-active{
          background:rgba(219, 55, 55, 0.3);
          color:#ff7373; }
        .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-danger:disabled, .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-danger.bp3-disabled{
          background:none;
          color:rgba(255, 115, 115, 0.5); }
          .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-danger:disabled.bp3-active, .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-danger.bp3-disabled.bp3-active{
            background:rgba(219, 55, 55, 0.3); }
  .bp3-button-group .bp3-popover-wrapper,
  .bp3-button-group .bp3-popover-target{
    display:-webkit-box;
    display:-ms-flexbox;
    display:flex;
    -webkit-box-flex:1;
        -ms-flex:1 1 auto;
            flex:1 1 auto; }
  .bp3-button-group.bp3-fill{
    display:-webkit-box;
    display:-ms-flexbox;
    display:flex;
    width:100%; }
  .bp3-button-group .bp3-button.bp3-fill,
  .bp3-button-group.bp3-fill .bp3-button:not(.bp3-fixed){
    -webkit-box-flex:1;
        -ms-flex:1 1 auto;
            flex:1 1 auto; }
  .bp3-button-group.bp3-vertical{
    -webkit-box-align:stretch;
        -ms-flex-align:stretch;
            align-items:stretch;
    -webkit-box-orient:vertical;
    -webkit-box-direction:normal;
        -ms-flex-direction:column;
            flex-direction:column;
    vertical-align:top; }
    .bp3-button-group.bp3-vertical.bp3-fill{
      height:100%;
      width:unset; }
    .bp3-button-group.bp3-vertical .bp3-button{
      margin-right:0 !important;
      width:100%; }
    .bp3-button-group.bp3-vertical:not(.bp3-minimal) > .bp3-popover-wrapper:first-child .bp3-button,
    .bp3-button-group.bp3-vertical:not(.bp3-minimal) > .bp3-button:first-child{
      border-radius:3px 3px 0 0; }
    .bp3-button-group.bp3-vertical:not(.bp3-minimal) > .bp3-popover-wrapper:last-child .bp3-button,
    .bp3-button-group.bp3-vertical:not(.bp3-minimal) > .bp3-button:last-child{
      border-radius:0 0 3px 3px; }
    .bp3-button-group.bp3-vertical:not(.bp3-minimal) > .bp3-popover-wrapper:not(:last-child) .bp3-button,
    .bp3-button-group.bp3-vertical:not(.bp3-minimal) > .bp3-button:not(:last-child){
      margin-bottom:-1px; }
  .bp3-button-group.bp3-align-left .bp3-button{
    text-align:left; }
  .bp3-dark .bp3-button-group:not(.bp3-minimal) > .bp3-popover-wrapper:not(:last-child) .bp3-button,
  .bp3-dark .bp3-button-group:not(.bp3-minimal) > .bp3-button:not(:last-child){
    margin-right:1px; }
  .bp3-dark .bp3-button-group.bp3-vertical > .bp3-popover-wrapper:not(:last-child) .bp3-button,
  .bp3-dark .bp3-button-group.bp3-vertical > .bp3-button:not(:last-child){
    margin-bottom:1px; }
.bp3-callout{
  font-size:14px;
  line-height:1.5;
  background-color:rgba(138, 155, 168, 0.15);
  border-radius:3px;
  padding:10px 12px 9px;
  position:relative;
  width:100%; }
  .bp3-callout[class*="bp3-icon-"]{
    padding-left:40px; }
    .bp3-callout[class*="bp3-icon-"]::before{
      font-family:"Icons20", sans-serif;
      font-size:20px;
      font-style:normal;
      font-weight:400;
      line-height:1;
      -moz-osx-font-smoothing:grayscale;
      -webkit-font-smoothing:antialiased;
      color:#5c7080;
      left:10px;
      position:absolute;
      top:10px; }
  .bp3-callout.bp3-callout-icon{
    padding-left:40px; }
    .bp3-callout.bp3-callout-icon > .bp3-icon:first-child{
      color:#5c7080;
      left:10px;
      position:absolute;
      top:10px; }
  .bp3-callout .bp3-heading{
    line-height:20px;
    margin-bottom:5px;
    margin-top:0; }
    .bp3-callout .bp3-heading:last-child{
      margin-bottom:0; }
  .bp3-dark .bp3-callout{
    background-color:rgba(138, 155, 168, 0.2); }
    .bp3-dark .bp3-callout[class*="bp3-icon-"]::before{
      color:#a7b6c2; }
  .bp3-callout.bp3-intent-primary{
    background-color:rgba(19, 124, 189, 0.15); }
    .bp3-callout.bp3-intent-primary[class*="bp3-icon-"]::before,
    .bp3-callout.bp3-intent-primary > .bp3-icon:first-child,
    .bp3-callout.bp3-intent-primary .bp3-heading{
      color:#106ba3; }
    .bp3-dark .bp3-callout.bp3-intent-primary{
      background-color:rgba(19, 124, 189, 0.25); }
      .bp3-dark .bp3-callout.bp3-intent-primary[class*="bp3-icon-"]::before,
      .bp3-dark .bp3-callout.bp3-intent-primary > .bp3-icon:first-child,
      .bp3-dark .bp3-callout.bp3-intent-primary .bp3-heading{
        color:#48aff0; }
  .bp3-callout.bp3-intent-success{
    background-color:rgba(15, 153, 96, 0.15); }
    .bp3-callout.bp3-intent-success[class*="bp3-icon-"]::before,
    .bp3-callout.bp3-intent-success > .bp3-icon:first-child,
    .bp3-callout.bp3-intent-success .bp3-heading{
      color:#0d8050; }
    .bp3-dark .bp3-callout.bp3-intent-success{
      background-color:rgba(15, 153, 96, 0.25); }
      .bp3-dark .bp3-callout.bp3-intent-success[class*="bp3-icon-"]::before,
      .bp3-dark .bp3-callout.bp3-intent-success > .bp3-icon:first-child,
      .bp3-dark .bp3-callout.bp3-intent-success .bp3-heading{
        color:#3dcc91; }
  .bp3-callout.bp3-intent-warning{
    background-color:rgba(217, 130, 43, 0.15); }
    .bp3-callout.bp3-intent-warning[class*="bp3-icon-"]::before,
    .bp3-callout.bp3-intent-warning > .bp3-icon:first-child,
    .bp3-callout.bp3-intent-warning .bp3-heading{
      color:#bf7326; }
    .bp3-dark .bp3-callout.bp3-intent-warning{
      background-color:rgba(217, 130, 43, 0.25); }
      .bp3-dark .bp3-callout.bp3-intent-warning[class*="bp3-icon-"]::before,
      .bp3-dark .bp3-callout.bp3-intent-warning > .bp3-icon:first-child,
      .bp3-dark .bp3-callout.bp3-intent-warning .bp3-heading{
        color:#ffb366; }
  .bp3-callout.bp3-intent-danger{
    background-color:rgba(219, 55, 55, 0.15); }
    .bp3-callout.bp3-intent-danger[class*="bp3-icon-"]::before,
    .bp3-callout.bp3-intent-danger > .bp3-icon:first-child,
    .bp3-callout.bp3-intent-danger .bp3-heading{
      color:#c23030; }
    .bp3-dark .bp3-callout.bp3-intent-danger{
      background-color:rgba(219, 55, 55, 0.25); }
      .bp3-dark .bp3-callout.bp3-intent-danger[class*="bp3-icon-"]::before,
      .bp3-dark .bp3-callout.bp3-intent-danger > .bp3-icon:first-child,
      .bp3-dark .bp3-callout.bp3-intent-danger .bp3-heading{
        color:#ff7373; }
  .bp3-running-text .bp3-callout{
    margin:20px 0; }
.bp3-card{
  background-color:#ffffff;
  border-radius:3px;
  -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.15), 0 0 0 rgba(16, 22, 26, 0), 0 0 0 rgba(16, 22, 26, 0);
          box-shadow:0 0 0 1px rgba(16, 22, 26, 0.15), 0 0 0 rgba(16, 22, 26, 0), 0 0 0 rgba(16, 22, 26, 0);
  padding:20px;
  -webkit-transition:-webkit-transform 200ms cubic-bezier(0.4, 1, 0.75, 0.9), -webkit-box-shadow 200ms cubic-bezier(0.4, 1, 0.75, 0.9);
  transition:-webkit-transform 200ms cubic-bezier(0.4, 1, 0.75, 0.9), -webkit-box-shadow 200ms cubic-bezier(0.4, 1, 0.75, 0.9);
  transition:transform 200ms cubic-bezier(0.4, 1, 0.75, 0.9), box-shadow 200ms cubic-bezier(0.4, 1, 0.75, 0.9);
  transition:transform 200ms cubic-bezier(0.4, 1, 0.75, 0.9), box-shadow 200ms cubic-bezier(0.4, 1, 0.75, 0.9), -webkit-transform 200ms cubic-bezier(0.4, 1, 0.75, 0.9), -webkit-box-shadow 200ms cubic-bezier(0.4, 1, 0.75, 0.9); }
  .bp3-card.bp3-dark,
  .bp3-dark .bp3-card{
    background-color:#30404d;
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4), 0 0 0 rgba(16, 22, 26, 0), 0 0 0 rgba(16, 22, 26, 0);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4), 0 0 0 rgba(16, 22, 26, 0), 0 0 0 rgba(16, 22, 26, 0); }

.bp3-elevation-0{
  -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.15), 0 0 0 rgba(16, 22, 26, 0), 0 0 0 rgba(16, 22, 26, 0);
          box-shadow:0 0 0 1px rgba(16, 22, 26, 0.15), 0 0 0 rgba(16, 22, 26, 0), 0 0 0 rgba(16, 22, 26, 0); }
  .bp3-elevation-0.bp3-dark,
  .bp3-dark .bp3-elevation-0{
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4), 0 0 0 rgba(16, 22, 26, 0), 0 0 0 rgba(16, 22, 26, 0);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4), 0 0 0 rgba(16, 22, 26, 0), 0 0 0 rgba(16, 22, 26, 0); }

.bp3-elevation-1{
  -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 0 0 rgba(16, 22, 26, 0), 0 1px 1px rgba(16, 22, 26, 0.2);
          box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 0 0 rgba(16, 22, 26, 0), 0 1px 1px rgba(16, 22, 26, 0.2); }
  .bp3-elevation-1.bp3-dark,
  .bp3-dark .bp3-elevation-1{
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 0 0 rgba(16, 22, 26, 0), 0 1px 1px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 0 0 rgba(16, 22, 26, 0), 0 1px 1px rgba(16, 22, 26, 0.4); }

.bp3-elevation-2{
  -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 1px 1px rgba(16, 22, 26, 0.2), 0 2px 6px rgba(16, 22, 26, 0.2);
          box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 1px 1px rgba(16, 22, 26, 0.2), 0 2px 6px rgba(16, 22, 26, 0.2); }
  .bp3-elevation-2.bp3-dark,
  .bp3-dark .bp3-elevation-2{
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 1px 1px rgba(16, 22, 26, 0.4), 0 2px 6px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 1px 1px rgba(16, 22, 26, 0.4), 0 2px 6px rgba(16, 22, 26, 0.4); }

.bp3-elevation-3{
  -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 2px 4px rgba(16, 22, 26, 0.2), 0 8px 24px rgba(16, 22, 26, 0.2);
          box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 2px 4px rgba(16, 22, 26, 0.2), 0 8px 24px rgba(16, 22, 26, 0.2); }
  .bp3-elevation-3.bp3-dark,
  .bp3-dark .bp3-elevation-3{
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 2px 4px rgba(16, 22, 26, 0.4), 0 8px 24px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 2px 4px rgba(16, 22, 26, 0.4), 0 8px 24px rgba(16, 22, 26, 0.4); }

.bp3-elevation-4{
  -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 4px 8px rgba(16, 22, 26, 0.2), 0 18px 46px 6px rgba(16, 22, 26, 0.2);
          box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 4px 8px rgba(16, 22, 26, 0.2), 0 18px 46px 6px rgba(16, 22, 26, 0.2); }
  .bp3-elevation-4.bp3-dark,
  .bp3-dark .bp3-elevation-4{
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 4px 8px rgba(16, 22, 26, 0.4), 0 18px 46px 6px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 4px 8px rgba(16, 22, 26, 0.4), 0 18px 46px 6px rgba(16, 22, 26, 0.4); }

.bp3-card.bp3-interactive:hover{
  -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 2px 4px rgba(16, 22, 26, 0.2), 0 8px 24px rgba(16, 22, 26, 0.2);
          box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 2px 4px rgba(16, 22, 26, 0.2), 0 8px 24px rgba(16, 22, 26, 0.2);
  cursor:pointer; }
  .bp3-card.bp3-interactive:hover.bp3-dark,
  .bp3-dark .bp3-card.bp3-interactive:hover{
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 2px 4px rgba(16, 22, 26, 0.4), 0 8px 24px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 2px 4px rgba(16, 22, 26, 0.4), 0 8px 24px rgba(16, 22, 26, 0.4); }

.bp3-card.bp3-interactive:active{
  -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 0 0 rgba(16, 22, 26, 0), 0 1px 1px rgba(16, 22, 26, 0.2);
          box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 0 0 rgba(16, 22, 26, 0), 0 1px 1px rgba(16, 22, 26, 0.2);
  opacity:0.9;
  -webkit-transition-duration:0;
          transition-duration:0; }
  .bp3-card.bp3-interactive:active.bp3-dark,
  .bp3-dark .bp3-card.bp3-interactive:active{
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 0 0 rgba(16, 22, 26, 0), 0 1px 1px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 0 0 rgba(16, 22, 26, 0), 0 1px 1px rgba(16, 22, 26, 0.4); }

.bp3-collapse{
  height:0;
  overflow-y:hidden;
  -webkit-transition:height 200ms cubic-bezier(0.4, 1, 0.75, 0.9);
  transition:height 200ms cubic-bezier(0.4, 1, 0.75, 0.9); }
  .bp3-collapse .bp3-collapse-body{
    -webkit-transition:-webkit-transform 200ms cubic-bezier(0.4, 1, 0.75, 0.9);
    transition:-webkit-transform 200ms cubic-bezier(0.4, 1, 0.75, 0.9);
    transition:transform 200ms cubic-bezier(0.4, 1, 0.75, 0.9);
    transition:transform 200ms cubic-bezier(0.4, 1, 0.75, 0.9), -webkit-transform 200ms cubic-bezier(0.4, 1, 0.75, 0.9); }
    .bp3-collapse .bp3-collapse-body[aria-hidden="true"]{
      display:none; }

.bp3-context-menu .bp3-popover-target{
  display:block; }

.bp3-context-menu-popover-target{
  position:fixed; }

.bp3-divider{
  border-bottom:1px solid rgba(16, 22, 26, 0.15);
  border-right:1px solid rgba(16, 22, 26, 0.15);
  margin:5px; }
  .bp3-dark .bp3-divider{
    border-color:rgba(16, 22, 26, 0.4); }
.bp3-dialog-container{
  opacity:1;
  -webkit-transform:scale(1);
          transform:scale(1);
  -webkit-box-align:center;
      -ms-flex-align:center;
          align-items:center;
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  -webkit-box-pack:center;
      -ms-flex-pack:center;
          justify-content:center;
  min-height:100%;
  pointer-events:none;
  -webkit-user-select:none;
     -moz-user-select:none;
      -ms-user-select:none;
          user-select:none;
  width:100%; }
  .bp3-dialog-container.bp3-overlay-enter > .bp3-dialog, .bp3-dialog-container.bp3-overlay-appear > .bp3-dialog{
    opacity:0;
    -webkit-transform:scale(0.5);
            transform:scale(0.5); }
  .bp3-dialog-container.bp3-overlay-enter-active > .bp3-dialog, .bp3-dialog-container.bp3-overlay-appear-active > .bp3-dialog{
    opacity:1;
    -webkit-transform:scale(1);
            transform:scale(1);
    -webkit-transition-delay:0;
            transition-delay:0;
    -webkit-transition-duration:300ms;
            transition-duration:300ms;
    -webkit-transition-property:opacity, -webkit-transform;
    transition-property:opacity, -webkit-transform;
    transition-property:opacity, transform;
    transition-property:opacity, transform, -webkit-transform;
    -webkit-transition-timing-function:cubic-bezier(0.54, 1.12, 0.38, 1.11);
            transition-timing-function:cubic-bezier(0.54, 1.12, 0.38, 1.11); }
  .bp3-dialog-container.bp3-overlay-exit > .bp3-dialog{
    opacity:1;
    -webkit-transform:scale(1);
            transform:scale(1); }
  .bp3-dialog-container.bp3-overlay-exit-active > .bp3-dialog{
    opacity:0;
    -webkit-transform:scale(0.5);
            transform:scale(0.5);
    -webkit-transition-delay:0;
            transition-delay:0;
    -webkit-transition-duration:300ms;
            transition-duration:300ms;
    -webkit-transition-property:opacity, -webkit-transform;
    transition-property:opacity, -webkit-transform;
    transition-property:opacity, transform;
    transition-property:opacity, transform, -webkit-transform;
    -webkit-transition-timing-function:cubic-bezier(0.54, 1.12, 0.38, 1.11);
            transition-timing-function:cubic-bezier(0.54, 1.12, 0.38, 1.11); }

.bp3-dialog{
  background:#ebf1f5;
  border-radius:6px;
  -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 4px 8px rgba(16, 22, 26, 0.2), 0 18px 46px 6px rgba(16, 22, 26, 0.2);
          box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 4px 8px rgba(16, 22, 26, 0.2), 0 18px 46px 6px rgba(16, 22, 26, 0.2);
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  -webkit-box-orient:vertical;
  -webkit-box-direction:normal;
      -ms-flex-direction:column;
          flex-direction:column;
  margin:30px 0;
  padding-bottom:20px;
  pointer-events:all;
  -webkit-user-select:text;
     -moz-user-select:text;
      -ms-user-select:text;
          user-select:text;
  width:500px; }
  .bp3-dialog:focus{
    outline:0; }
  .bp3-dialog.bp3-dark,
  .bp3-dark .bp3-dialog{
    background:#293742;
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 4px 8px rgba(16, 22, 26, 0.4), 0 18px 46px 6px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 4px 8px rgba(16, 22, 26, 0.4), 0 18px 46px 6px rgba(16, 22, 26, 0.4);
    color:#f5f8fa; }

.bp3-dialog-header{
  -webkit-box-align:center;
      -ms-flex-align:center;
          align-items:center;
  background:#ffffff;
  border-radius:6px 6px 0 0;
  -webkit-box-shadow:0 1px 0 rgba(16, 22, 26, 0.15);
          box-shadow:0 1px 0 rgba(16, 22, 26, 0.15);
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  -webkit-box-flex:0;
      -ms-flex:0 0 auto;
          flex:0 0 auto;
  min-height:40px;
  padding-left:20px;
  padding-right:5px;
  z-index:30; }
  .bp3-dialog-header .bp3-icon-large,
  .bp3-dialog-header .bp3-icon{
    color:#5c7080;
    -webkit-box-flex:0;
        -ms-flex:0 0 auto;
            flex:0 0 auto;
    margin-right:10px; }
  .bp3-dialog-header .bp3-heading{
    overflow:hidden;
    text-overflow:ellipsis;
    white-space:nowrap;
    word-wrap:normal;
    -webkit-box-flex:1;
        -ms-flex:1 1 auto;
            flex:1 1 auto;
    line-height:inherit;
    margin:0; }
    .bp3-dialog-header .bp3-heading:last-child{
      margin-right:20px; }
  .bp3-dark .bp3-dialog-header{
    background:#30404d;
    -webkit-box-shadow:0 1px 0 rgba(16, 22, 26, 0.4);
            box-shadow:0 1px 0 rgba(16, 22, 26, 0.4); }
    .bp3-dark .bp3-dialog-header .bp3-icon-large,
    .bp3-dark .bp3-dialog-header .bp3-icon{
      color:#a7b6c2; }

.bp3-dialog-body{
  -webkit-box-flex:1;
      -ms-flex:1 1 auto;
          flex:1 1 auto;
  line-height:18px;
  margin:20px; }

.bp3-dialog-footer{
  -webkit-box-flex:0;
      -ms-flex:0 0 auto;
          flex:0 0 auto;
  margin:0 20px; }

.bp3-dialog-footer-actions{
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  -webkit-box-pack:end;
      -ms-flex-pack:end;
          justify-content:flex-end; }
  .bp3-dialog-footer-actions .bp3-button{
    margin-left:10px; }
.bp3-multistep-dialog-panels{
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex; }

.bp3-multistep-dialog-left-panel{
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  -webkit-box-flex:1;
      -ms-flex:1;
          flex:1;
  -webkit-box-orient:vertical;
  -webkit-box-direction:normal;
      -ms-flex-direction:column;
          flex-direction:column; }
  .bp3-dark .bp3-multistep-dialog-left-panel{
    background:#202b33; }

.bp3-multistep-dialog-right-panel{
  background-color:#f5f8fa;
  border-left:1px solid rgba(16, 22, 26, 0.15);
  border-radius:0 0 6px 0;
  -webkit-box-flex:3;
      -ms-flex:3;
          flex:3;
  min-width:0; }
  .bp3-dark .bp3-multistep-dialog-right-panel{
    background-color:#293742;
    border-left:1px solid rgba(16, 22, 26, 0.4); }

.bp3-multistep-dialog-footer{
  background-color:#ffffff;
  border-radius:0 0 6px 0;
  border-top:1px solid rgba(16, 22, 26, 0.15);
  padding:10px; }
  .bp3-dark .bp3-multistep-dialog-footer{
    background:#30404d;
    border-top:1px solid rgba(16, 22, 26, 0.4); }

.bp3-dialog-step-container{
  background-color:#f5f8fa;
  border-bottom:1px solid rgba(16, 22, 26, 0.15); }
  .bp3-dark .bp3-dialog-step-container{
    background:#293742;
    border-bottom:1px solid rgba(16, 22, 26, 0.4); }
  .bp3-dialog-step-container.bp3-dialog-step-viewed{
    background-color:#ffffff; }
    .bp3-dark .bp3-dialog-step-container.bp3-dialog-step-viewed{
      background:#30404d; }

.bp3-dialog-step{
  -webkit-box-align:center;
      -ms-flex-align:center;
          align-items:center;
  background-color:#f5f8fa;
  border-radius:6px;
  cursor:not-allowed;
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  margin:4px;
  padding:6px 14px; }
  .bp3-dark .bp3-dialog-step{
    background:#293742; }
  .bp3-dialog-step-viewed .bp3-dialog-step{
    background-color:#ffffff;
    cursor:pointer; }
    .bp3-dark .bp3-dialog-step-viewed .bp3-dialog-step{
      background:#30404d; }
  .bp3-dialog-step:hover{
    background-color:#f5f8fa; }
    .bp3-dark .bp3-dialog-step:hover{
      background:#293742; }

.bp3-dialog-step-icon{
  -webkit-box-align:center;
      -ms-flex-align:center;
          align-items:center;
  background-color:rgba(92, 112, 128, 0.6);
  border-radius:50%;
  color:#ffffff;
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  height:25px;
  -webkit-box-pack:center;
      -ms-flex-pack:center;
          justify-content:center;
  width:25px; }
  .bp3-dark .bp3-dialog-step-icon{
    background-color:rgba(167, 182, 194, 0.6); }
  .bp3-active.bp3-dialog-step-viewed .bp3-dialog-step-icon{
    background-color:#2b95d6; }
  .bp3-dialog-step-viewed .bp3-dialog-step-icon{
    background-color:#8a9ba8; }

.bp3-dialog-step-title{
  color:rgba(92, 112, 128, 0.6);
  -webkit-box-flex:1;
      -ms-flex:1;
          flex:1;
  padding-left:10px; }
  .bp3-dark .bp3-dialog-step-title{
    color:rgba(167, 182, 194, 0.6); }
  .bp3-active.bp3-dialog-step-viewed .bp3-dialog-step-title{
    color:#2b95d6; }
  .bp3-dialog-step-viewed:not(.bp3-active) .bp3-dialog-step-title{
    color:#182026; }
    .bp3-dark .bp3-dialog-step-viewed:not(.bp3-active) .bp3-dialog-step-title{
      color:#f5f8fa; }
.bp3-drawer{
  background:#ffffff;
  -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 4px 8px rgba(16, 22, 26, 0.2), 0 18px 46px 6px rgba(16, 22, 26, 0.2);
          box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 4px 8px rgba(16, 22, 26, 0.2), 0 18px 46px 6px rgba(16, 22, 26, 0.2);
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  -webkit-box-orient:vertical;
  -webkit-box-direction:normal;
      -ms-flex-direction:column;
          flex-direction:column;
  margin:0;
  padding:0; }
  .bp3-drawer:focus{
    outline:0; }
  .bp3-drawer.bp3-position-top{
    height:50%;
    left:0;
    right:0;
    top:0; }
    .bp3-drawer.bp3-position-top.bp3-overlay-enter, .bp3-drawer.bp3-position-top.bp3-overlay-appear{
      -webkit-transform:translateY(-100%);
              transform:translateY(-100%); }
    .bp3-drawer.bp3-position-top.bp3-overlay-enter-active, .bp3-drawer.bp3-position-top.bp3-overlay-appear-active{
      -webkit-transform:translateY(0);
              transform:translateY(0);
      -webkit-transition-delay:0;
              transition-delay:0;
      -webkit-transition-duration:200ms;
              transition-duration:200ms;
      -webkit-transition-property:-webkit-transform;
      transition-property:-webkit-transform;
      transition-property:transform;
      transition-property:transform, -webkit-transform;
      -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
              transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
    .bp3-drawer.bp3-position-top.bp3-overlay-exit{
      -webkit-transform:translateY(0);
              transform:translateY(0); }
    .bp3-drawer.bp3-position-top.bp3-overlay-exit-active{
      -webkit-transform:translateY(-100%);
              transform:translateY(-100%);
      -webkit-transition-delay:0;
              transition-delay:0;
      -webkit-transition-duration:100ms;
              transition-duration:100ms;
      -webkit-transition-property:-webkit-transform;
      transition-property:-webkit-transform;
      transition-property:transform;
      transition-property:transform, -webkit-transform;
      -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
              transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
  .bp3-drawer.bp3-position-bottom{
    bottom:0;
    height:50%;
    left:0;
    right:0; }
    .bp3-drawer.bp3-position-bottom.bp3-overlay-enter, .bp3-drawer.bp3-position-bottom.bp3-overlay-appear{
      -webkit-transform:translateY(100%);
              transform:translateY(100%); }
    .bp3-drawer.bp3-position-bottom.bp3-overlay-enter-active, .bp3-drawer.bp3-position-bottom.bp3-overlay-appear-active{
      -webkit-transform:translateY(0);
              transform:translateY(0);
      -webkit-transition-delay:0;
              transition-delay:0;
      -webkit-transition-duration:200ms;
              transition-duration:200ms;
      -webkit-transition-property:-webkit-transform;
      transition-property:-webkit-transform;
      transition-property:transform;
      transition-property:transform, -webkit-transform;
      -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
              transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
    .bp3-drawer.bp3-position-bottom.bp3-overlay-exit{
      -webkit-transform:translateY(0);
              transform:translateY(0); }
    .bp3-drawer.bp3-position-bottom.bp3-overlay-exit-active{
      -webkit-transform:translateY(100%);
              transform:translateY(100%);
      -webkit-transition-delay:0;
              transition-delay:0;
      -webkit-transition-duration:100ms;
              transition-duration:100ms;
      -webkit-transition-property:-webkit-transform;
      transition-property:-webkit-transform;
      transition-property:transform;
      transition-property:transform, -webkit-transform;
      -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
              transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
  .bp3-drawer.bp3-position-left{
    bottom:0;
    left:0;
    top:0;
    width:50%; }
    .bp3-drawer.bp3-position-left.bp3-overlay-enter, .bp3-drawer.bp3-position-left.bp3-overlay-appear{
      -webkit-transform:translateX(-100%);
              transform:translateX(-100%); }
    .bp3-drawer.bp3-position-left.bp3-overlay-enter-active, .bp3-drawer.bp3-position-left.bp3-overlay-appear-active{
      -webkit-transform:translateX(0);
              transform:translateX(0);
      -webkit-transition-delay:0;
              transition-delay:0;
      -webkit-transition-duration:200ms;
              transition-duration:200ms;
      -webkit-transition-property:-webkit-transform;
      transition-property:-webkit-transform;
      transition-property:transform;
      transition-property:transform, -webkit-transform;
      -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
              transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
    .bp3-drawer.bp3-position-left.bp3-overlay-exit{
      -webkit-transform:translateX(0);
              transform:translateX(0); }
    .bp3-drawer.bp3-position-left.bp3-overlay-exit-active{
      -webkit-transform:translateX(-100%);
              transform:translateX(-100%);
      -webkit-transition-delay:0;
              transition-delay:0;
      -webkit-transition-duration:100ms;
              transition-duration:100ms;
      -webkit-transition-property:-webkit-transform;
      transition-property:-webkit-transform;
      transition-property:transform;
      transition-property:transform, -webkit-transform;
      -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
              transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
  .bp3-drawer.bp3-position-right{
    bottom:0;
    right:0;
    top:0;
    width:50%; }
    .bp3-drawer.bp3-position-right.bp3-overlay-enter, .bp3-drawer.bp3-position-right.bp3-overlay-appear{
      -webkit-transform:translateX(100%);
              transform:translateX(100%); }
    .bp3-drawer.bp3-position-right.bp3-overlay-enter-active, .bp3-drawer.bp3-position-right.bp3-overlay-appear-active{
      -webkit-transform:translateX(0);
              transform:translateX(0);
      -webkit-transition-delay:0;
              transition-delay:0;
      -webkit-transition-duration:200ms;
              transition-duration:200ms;
      -webkit-transition-property:-webkit-transform;
      transition-property:-webkit-transform;
      transition-property:transform;
      transition-property:transform, -webkit-transform;
      -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
              transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
    .bp3-drawer.bp3-position-right.bp3-overlay-exit{
      -webkit-transform:translateX(0);
              transform:translateX(0); }
    .bp3-drawer.bp3-position-right.bp3-overlay-exit-active{
      -webkit-transform:translateX(100%);
              transform:translateX(100%);
      -webkit-transition-delay:0;
              transition-delay:0;
      -webkit-transition-duration:100ms;
              transition-duration:100ms;
      -webkit-transition-property:-webkit-transform;
      transition-property:-webkit-transform;
      transition-property:transform;
      transition-property:transform, -webkit-transform;
      -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
              transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
  .bp3-drawer:not(.bp3-position-top):not(.bp3-position-bottom):not(.bp3-position-left):not(
  .bp3-position-right):not(.bp3-vertical){
    bottom:0;
    right:0;
    top:0;
    width:50%; }
    .bp3-drawer:not(.bp3-position-top):not(.bp3-position-bottom):not(.bp3-position-left):not(
    .bp3-position-right):not(.bp3-vertical).bp3-overlay-enter, .bp3-drawer:not(.bp3-position-top):not(.bp3-position-bottom):not(.bp3-position-left):not(
    .bp3-position-right):not(.bp3-vertical).bp3-overlay-appear{
      -webkit-transform:translateX(100%);
              transform:translateX(100%); }
    .bp3-drawer:not(.bp3-position-top):not(.bp3-position-bottom):not(.bp3-position-left):not(
    .bp3-position-right):not(.bp3-vertical).bp3-overlay-enter-active, .bp3-drawer:not(.bp3-position-top):not(.bp3-position-bottom):not(.bp3-position-left):not(
    .bp3-position-right):not(.bp3-vertical).bp3-overlay-appear-active{
      -webkit-transform:translateX(0);
              transform:translateX(0);
      -webkit-transition-delay:0;
              transition-delay:0;
      -webkit-transition-duration:200ms;
              transition-duration:200ms;
      -webkit-transition-property:-webkit-transform;
      transition-property:-webkit-transform;
      transition-property:transform;
      transition-property:transform, -webkit-transform;
      -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
              transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
    .bp3-drawer:not(.bp3-position-top):not(.bp3-position-bottom):not(.bp3-position-left):not(
    .bp3-position-right):not(.bp3-vertical).bp3-overlay-exit{
      -webkit-transform:translateX(0);
              transform:translateX(0); }
    .bp3-drawer:not(.bp3-position-top):not(.bp3-position-bottom):not(.bp3-position-left):not(
    .bp3-position-right):not(.bp3-vertical).bp3-overlay-exit-active{
      -webkit-transform:translateX(100%);
              transform:translateX(100%);
      -webkit-transition-delay:0;
              transition-delay:0;
      -webkit-transition-duration:100ms;
              transition-duration:100ms;
      -webkit-transition-property:-webkit-transform;
      transition-property:-webkit-transform;
      transition-property:transform;
      transition-property:transform, -webkit-transform;
      -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
              transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
  .bp3-drawer:not(.bp3-position-top):not(.bp3-position-bottom):not(.bp3-position-left):not(
  .bp3-position-right).bp3-vertical{
    bottom:0;
    height:50%;
    left:0;
    right:0; }
    .bp3-drawer:not(.bp3-position-top):not(.bp3-position-bottom):not(.bp3-position-left):not(
    .bp3-position-right).bp3-vertical.bp3-overlay-enter, .bp3-drawer:not(.bp3-position-top):not(.bp3-position-bottom):not(.bp3-position-left):not(
    .bp3-position-right).bp3-vertical.bp3-overlay-appear{
      -webkit-transform:translateY(100%);
              transform:translateY(100%); }
    .bp3-drawer:not(.bp3-position-top):not(.bp3-position-bottom):not(.bp3-position-left):not(
    .bp3-position-right).bp3-vertical.bp3-overlay-enter-active, .bp3-drawer:not(.bp3-position-top):not(.bp3-position-bottom):not(.bp3-position-left):not(
    .bp3-position-right).bp3-vertical.bp3-overlay-appear-active{
      -webkit-transform:translateY(0);
              transform:translateY(0);
      -webkit-transition-delay:0;
              transition-delay:0;
      -webkit-transition-duration:200ms;
              transition-duration:200ms;
      -webkit-transition-property:-webkit-transform;
      transition-property:-webkit-transform;
      transition-property:transform;
      transition-property:transform, -webkit-transform;
      -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
              transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
    .bp3-drawer:not(.bp3-position-top):not(.bp3-position-bottom):not(.bp3-position-left):not(
    .bp3-position-right).bp3-vertical.bp3-overlay-exit{
      -webkit-transform:translateY(0);
              transform:translateY(0); }
    .bp3-drawer:not(.bp3-position-top):not(.bp3-position-bottom):not(.bp3-position-left):not(
    .bp3-position-right).bp3-vertical.bp3-overlay-exit-active{
      -webkit-transform:translateY(100%);
              transform:translateY(100%);
      -webkit-transition-delay:0;
              transition-delay:0;
      -webkit-transition-duration:100ms;
              transition-duration:100ms;
      -webkit-transition-property:-webkit-transform;
      transition-property:-webkit-transform;
      transition-property:transform;
      transition-property:transform, -webkit-transform;
      -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
              transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
  .bp3-drawer.bp3-dark,
  .bp3-dark .bp3-drawer{
    background:#30404d;
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 4px 8px rgba(16, 22, 26, 0.4), 0 18px 46px 6px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 4px 8px rgba(16, 22, 26, 0.4), 0 18px 46px 6px rgba(16, 22, 26, 0.4);
    color:#f5f8fa; }

.bp3-drawer-header{
  -webkit-box-align:center;
      -ms-flex-align:center;
          align-items:center;
  border-radius:0;
  -webkit-box-shadow:0 1px 0 rgba(16, 22, 26, 0.15);
          box-shadow:0 1px 0 rgba(16, 22, 26, 0.15);
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  -webkit-box-flex:0;
      -ms-flex:0 0 auto;
          flex:0 0 auto;
  min-height:40px;
  padding:5px;
  padding-left:20px;
  position:relative; }
  .bp3-drawer-header .bp3-icon-large,
  .bp3-drawer-header .bp3-icon{
    color:#5c7080;
    -webkit-box-flex:0;
        -ms-flex:0 0 auto;
            flex:0 0 auto;
    margin-right:10px; }
  .bp3-drawer-header .bp3-heading{
    overflow:hidden;
    text-overflow:ellipsis;
    white-space:nowrap;
    word-wrap:normal;
    -webkit-box-flex:1;
        -ms-flex:1 1 auto;
            flex:1 1 auto;
    line-height:inherit;
    margin:0; }
    .bp3-drawer-header .bp3-heading:last-child{
      margin-right:20px; }
  .bp3-dark .bp3-drawer-header{
    -webkit-box-shadow:0 1px 0 rgba(16, 22, 26, 0.4);
            box-shadow:0 1px 0 rgba(16, 22, 26, 0.4); }
    .bp3-dark .bp3-drawer-header .bp3-icon-large,
    .bp3-dark .bp3-drawer-header .bp3-icon{
      color:#a7b6c2; }

.bp3-drawer-body{
  -webkit-box-flex:1;
      -ms-flex:1 1 auto;
          flex:1 1 auto;
  line-height:18px;
  overflow:auto; }

.bp3-drawer-footer{
  -webkit-box-shadow:inset 0 1px 0 rgba(16, 22, 26, 0.15);
          box-shadow:inset 0 1px 0 rgba(16, 22, 26, 0.15);
  -webkit-box-flex:0;
      -ms-flex:0 0 auto;
          flex:0 0 auto;
  padding:10px 20px;
  position:relative; }
  .bp3-dark .bp3-drawer-footer{
    -webkit-box-shadow:inset 0 1px 0 rgba(16, 22, 26, 0.4);
            box-shadow:inset 0 1px 0 rgba(16, 22, 26, 0.4); }
.bp3-editable-text{
  cursor:text;
  display:inline-block;
  max-width:100%;
  position:relative;
  vertical-align:top;
  white-space:nowrap; }
  .bp3-editable-text::before{
    bottom:-3px;
    left:-3px;
    position:absolute;
    right:-3px;
    top:-3px;
    border-radius:3px;
    content:"";
    -webkit-transition:background-color 100ms cubic-bezier(0.4, 1, 0.75, 0.9), -webkit-box-shadow 100ms cubic-bezier(0.4, 1, 0.75, 0.9);
    transition:background-color 100ms cubic-bezier(0.4, 1, 0.75, 0.9), -webkit-box-shadow 100ms cubic-bezier(0.4, 1, 0.75, 0.9);
    transition:background-color 100ms cubic-bezier(0.4, 1, 0.75, 0.9), box-shadow 100ms cubic-bezier(0.4, 1, 0.75, 0.9);
    transition:background-color 100ms cubic-bezier(0.4, 1, 0.75, 0.9), box-shadow 100ms cubic-bezier(0.4, 1, 0.75, 0.9), -webkit-box-shadow 100ms cubic-bezier(0.4, 1, 0.75, 0.9); }
  .bp3-editable-text:hover::before{
    -webkit-box-shadow:0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), inset 0 0 0 1px rgba(16, 22, 26, 0.15);
            box-shadow:0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), inset 0 0 0 1px rgba(16, 22, 26, 0.15); }
  .bp3-editable-text.bp3-editable-text-editing::before{
    background-color:#ffffff;
    -webkit-box-shadow:0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2);
            box-shadow:0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
  .bp3-editable-text.bp3-disabled::before{
    -webkit-box-shadow:none;
            box-shadow:none; }
  .bp3-editable-text.bp3-intent-primary .bp3-editable-text-input,
  .bp3-editable-text.bp3-intent-primary .bp3-editable-text-content{
    color:#137cbd; }
  .bp3-editable-text.bp3-intent-primary:hover::before{
    -webkit-box-shadow:0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), inset 0 0 0 1px rgba(19, 124, 189, 0.4);
            box-shadow:0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), inset 0 0 0 1px rgba(19, 124, 189, 0.4); }
  .bp3-editable-text.bp3-intent-primary.bp3-editable-text-editing::before{
    -webkit-box-shadow:0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2);
            box-shadow:0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
  .bp3-editable-text.bp3-intent-success .bp3-editable-text-input,
  .bp3-editable-text.bp3-intent-success .bp3-editable-text-content{
    color:#0f9960; }
  .bp3-editable-text.bp3-intent-success:hover::before{
    -webkit-box-shadow:0 0 0 0 rgba(15, 153, 96, 0), 0 0 0 0 rgba(15, 153, 96, 0), inset 0 0 0 1px rgba(15, 153, 96, 0.4);
            box-shadow:0 0 0 0 rgba(15, 153, 96, 0), 0 0 0 0 rgba(15, 153, 96, 0), inset 0 0 0 1px rgba(15, 153, 96, 0.4); }
  .bp3-editable-text.bp3-intent-success.bp3-editable-text-editing::before{
    -webkit-box-shadow:0 0 0 1px #0f9960, 0 0 0 3px rgba(15, 153, 96, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2);
            box-shadow:0 0 0 1px #0f9960, 0 0 0 3px rgba(15, 153, 96, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
  .bp3-editable-text.bp3-intent-warning .bp3-editable-text-input,
  .bp3-editable-text.bp3-intent-warning .bp3-editable-text-content{
    color:#d9822b; }
  .bp3-editable-text.bp3-intent-warning:hover::before{
    -webkit-box-shadow:0 0 0 0 rgba(217, 130, 43, 0), 0 0 0 0 rgba(217, 130, 43, 0), inset 0 0 0 1px rgba(217, 130, 43, 0.4);
            box-shadow:0 0 0 0 rgba(217, 130, 43, 0), 0 0 0 0 rgba(217, 130, 43, 0), inset 0 0 0 1px rgba(217, 130, 43, 0.4); }
  .bp3-editable-text.bp3-intent-warning.bp3-editable-text-editing::before{
    -webkit-box-shadow:0 0 0 1px #d9822b, 0 0 0 3px rgba(217, 130, 43, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2);
            box-shadow:0 0 0 1px #d9822b, 0 0 0 3px rgba(217, 130, 43, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
  .bp3-editable-text.bp3-intent-danger .bp3-editable-text-input,
  .bp3-editable-text.bp3-intent-danger .bp3-editable-text-content{
    color:#db3737; }
  .bp3-editable-text.bp3-intent-danger:hover::before{
    -webkit-box-shadow:0 0 0 0 rgba(219, 55, 55, 0), 0 0 0 0 rgba(219, 55, 55, 0), inset 0 0 0 1px rgba(219, 55, 55, 0.4);
            box-shadow:0 0 0 0 rgba(219, 55, 55, 0), 0 0 0 0 rgba(219, 55, 55, 0), inset 0 0 0 1px rgba(219, 55, 55, 0.4); }
  .bp3-editable-text.bp3-intent-danger.bp3-editable-text-editing::before{
    -webkit-box-shadow:0 0 0 1px #db3737, 0 0 0 3px rgba(219, 55, 55, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2);
            box-shadow:0 0 0 1px #db3737, 0 0 0 3px rgba(219, 55, 55, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
  .bp3-dark .bp3-editable-text:hover::before{
    -webkit-box-shadow:0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), inset 0 0 0 1px rgba(255, 255, 255, 0.15);
            box-shadow:0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), inset 0 0 0 1px rgba(255, 255, 255, 0.15); }
  .bp3-dark .bp3-editable-text.bp3-editable-text-editing::before{
    background-color:rgba(16, 22, 26, 0.3);
    -webkit-box-shadow:0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4); }
  .bp3-dark .bp3-editable-text.bp3-disabled::before{
    -webkit-box-shadow:none;
            box-shadow:none; }
  .bp3-dark .bp3-editable-text.bp3-intent-primary .bp3-editable-text-content{
    color:#48aff0; }
  .bp3-dark .bp3-editable-text.bp3-intent-primary:hover::before{
    -webkit-box-shadow:0 0 0 0 rgba(72, 175, 240, 0), 0 0 0 0 rgba(72, 175, 240, 0), inset 0 0 0 1px rgba(72, 175, 240, 0.4);
            box-shadow:0 0 0 0 rgba(72, 175, 240, 0), 0 0 0 0 rgba(72, 175, 240, 0), inset 0 0 0 1px rgba(72, 175, 240, 0.4); }
  .bp3-dark .bp3-editable-text.bp3-intent-primary.bp3-editable-text-editing::before{
    -webkit-box-shadow:0 0 0 1px #48aff0, 0 0 0 3px rgba(72, 175, 240, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px #48aff0, 0 0 0 3px rgba(72, 175, 240, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4); }
  .bp3-dark .bp3-editable-text.bp3-intent-success .bp3-editable-text-content{
    color:#3dcc91; }
  .bp3-dark .bp3-editable-text.bp3-intent-success:hover::before{
    -webkit-box-shadow:0 0 0 0 rgba(61, 204, 145, 0), 0 0 0 0 rgba(61, 204, 145, 0), inset 0 0 0 1px rgba(61, 204, 145, 0.4);
            box-shadow:0 0 0 0 rgba(61, 204, 145, 0), 0 0 0 0 rgba(61, 204, 145, 0), inset 0 0 0 1px rgba(61, 204, 145, 0.4); }
  .bp3-dark .bp3-editable-text.bp3-intent-success.bp3-editable-text-editing::before{
    -webkit-box-shadow:0 0 0 1px #3dcc91, 0 0 0 3px rgba(61, 204, 145, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px #3dcc91, 0 0 0 3px rgba(61, 204, 145, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4); }
  .bp3-dark .bp3-editable-text.bp3-intent-warning .bp3-editable-text-content{
    color:#ffb366; }
  .bp3-dark .bp3-editable-text.bp3-intent-warning:hover::before{
    -webkit-box-shadow:0 0 0 0 rgba(255, 179, 102, 0), 0 0 0 0 rgba(255, 179, 102, 0), inset 0 0 0 1px rgba(255, 179, 102, 0.4);
            box-shadow:0 0 0 0 rgba(255, 179, 102, 0), 0 0 0 0 rgba(255, 179, 102, 0), inset 0 0 0 1px rgba(255, 179, 102, 0.4); }
  .bp3-dark .bp3-editable-text.bp3-intent-warning.bp3-editable-text-editing::before{
    -webkit-box-shadow:0 0 0 1px #ffb366, 0 0 0 3px rgba(255, 179, 102, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px #ffb366, 0 0 0 3px rgba(255, 179, 102, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4); }
  .bp3-dark .bp3-editable-text.bp3-intent-danger .bp3-editable-text-content{
    color:#ff7373; }
  .bp3-dark .bp3-editable-text.bp3-intent-danger:hover::before{
    -webkit-box-shadow:0 0 0 0 rgba(255, 115, 115, 0), 0 0 0 0 rgba(255, 115, 115, 0), inset 0 0 0 1px rgba(255, 115, 115, 0.4);
            box-shadow:0 0 0 0 rgba(255, 115, 115, 0), 0 0 0 0 rgba(255, 115, 115, 0), inset 0 0 0 1px rgba(255, 115, 115, 0.4); }
  .bp3-dark .bp3-editable-text.bp3-intent-danger.bp3-editable-text-editing::before{
    -webkit-box-shadow:0 0 0 1px #ff7373, 0 0 0 3px rgba(255, 115, 115, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px #ff7373, 0 0 0 3px rgba(255, 115, 115, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4); }

.bp3-editable-text-input,
.bp3-editable-text-content{
  color:inherit;
  display:inherit;
  font:inherit;
  letter-spacing:inherit;
  max-width:inherit;
  min-width:inherit;
  position:relative;
  resize:none;
  text-transform:inherit;
  vertical-align:top; }

.bp3-editable-text-input{
  background:none;
  border:none;
  -webkit-box-shadow:none;
          box-shadow:none;
  padding:0;
  white-space:pre-wrap;
  width:100%; }
  .bp3-editable-text-input::-webkit-input-placeholder{
    color:rgba(92, 112, 128, 0.6);
    opacity:1; }
  .bp3-editable-text-input::-moz-placeholder{
    color:rgba(92, 112, 128, 0.6);
    opacity:1; }
  .bp3-editable-text-input:-ms-input-placeholder{
    color:rgba(92, 112, 128, 0.6);
    opacity:1; }
  .bp3-editable-text-input::-ms-input-placeholder{
    color:rgba(92, 112, 128, 0.6);
    opacity:1; }
  .bp3-editable-text-input::placeholder{
    color:rgba(92, 112, 128, 0.6);
    opacity:1; }
  .bp3-editable-text-input:focus{
    outline:none; }
  .bp3-editable-text-input::-ms-clear{
    display:none; }

.bp3-editable-text-content{
  overflow:hidden;
  padding-right:2px;
  text-overflow:ellipsis;
  white-space:pre; }
  .bp3-editable-text-editing > .bp3-editable-text-content{
    left:0;
    position:absolute;
    visibility:hidden; }
  .bp3-editable-text-placeholder > .bp3-editable-text-content{
    color:rgba(92, 112, 128, 0.6); }
    .bp3-dark .bp3-editable-text-placeholder > .bp3-editable-text-content{
      color:rgba(167, 182, 194, 0.6); }

.bp3-editable-text.bp3-multiline{
  display:block; }
  .bp3-editable-text.bp3-multiline .bp3-editable-text-content{
    overflow:auto;
    white-space:pre-wrap;
    word-wrap:break-word; }
.bp3-divider{
  border-bottom:1px solid rgba(16, 22, 26, 0.15);
  border-right:1px solid rgba(16, 22, 26, 0.15);
  margin:5px; }
  .bp3-dark .bp3-divider{
    border-color:rgba(16, 22, 26, 0.4); }
.bp3-control-group{
  -webkit-transform:translateZ(0);
          transform:translateZ(0);
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  -webkit-box-orient:horizontal;
  -webkit-box-direction:normal;
      -ms-flex-direction:row;
          flex-direction:row;
  -webkit-box-align:stretch;
      -ms-flex-align:stretch;
          align-items:stretch; }
  .bp3-control-group > *{
    -webkit-box-flex:0;
        -ms-flex-positive:0;
            flex-grow:0;
    -ms-flex-negative:0;
        flex-shrink:0; }
  .bp3-control-group > .bp3-fill{
    -webkit-box-flex:1;
        -ms-flex-positive:1;
            flex-grow:1;
    -ms-flex-negative:1;
        flex-shrink:1; }
  .bp3-control-group .bp3-button,
  .bp3-control-group .bp3-html-select,
  .bp3-control-group .bp3-input,
  .bp3-control-group .bp3-select{
    position:relative; }
  .bp3-control-group .bp3-input{
    border-radius:inherit;
    z-index:2; }
    .bp3-control-group .bp3-input:focus{
      border-radius:3px;
      z-index:14; }
    .bp3-control-group .bp3-input[class*="bp3-intent"]{
      z-index:13; }
      .bp3-control-group .bp3-input[class*="bp3-intent"]:focus{
        z-index:15; }
    .bp3-control-group .bp3-input[readonly], .bp3-control-group .bp3-input:disabled, .bp3-control-group .bp3-input.bp3-disabled{
      z-index:1; }
  .bp3-control-group .bp3-input-group[class*="bp3-intent"] .bp3-input{
    z-index:13; }
    .bp3-control-group .bp3-input-group[class*="bp3-intent"] .bp3-input:focus{
      z-index:15; }
  .bp3-control-group .bp3-button,
  .bp3-control-group .bp3-html-select select,
  .bp3-control-group .bp3-select select{
    -webkit-transform:translateZ(0);
            transform:translateZ(0);
    border-radius:inherit;
    z-index:4; }
    .bp3-control-group .bp3-button:focus,
    .bp3-control-group .bp3-html-select select:focus,
    .bp3-control-group .bp3-select select:focus{
      z-index:5; }
    .bp3-control-group .bp3-button:hover,
    .bp3-control-group .bp3-html-select select:hover,
    .bp3-control-group .bp3-select select:hover{
      z-index:6; }
    .bp3-control-group .bp3-button:active,
    .bp3-control-group .bp3-html-select select:active,
    .bp3-control-group .bp3-select select:active{
      z-index:7; }
    .bp3-control-group .bp3-button[readonly], .bp3-control-group .bp3-button:disabled, .bp3-control-group .bp3-button.bp3-disabled,
    .bp3-control-group .bp3-html-select select[readonly],
    .bp3-control-group .bp3-html-select select:disabled,
    .bp3-control-group .bp3-html-select select.bp3-disabled,
    .bp3-control-group .bp3-select select[readonly],
    .bp3-control-group .bp3-select select:disabled,
    .bp3-control-group .bp3-select select.bp3-disabled{
      z-index:3; }
    .bp3-control-group .bp3-button[class*="bp3-intent"],
    .bp3-control-group .bp3-html-select select[class*="bp3-intent"],
    .bp3-control-group .bp3-select select[class*="bp3-intent"]{
      z-index:9; }
      .bp3-control-group .bp3-button[class*="bp3-intent"]:focus,
      .bp3-control-group .bp3-html-select select[class*="bp3-intent"]:focus,
      .bp3-control-group .bp3-select select[class*="bp3-intent"]:focus{
        z-index:10; }
      .bp3-control-group .bp3-button[class*="bp3-intent"]:hover,
      .bp3-control-group .bp3-html-select select[class*="bp3-intent"]:hover,
      .bp3-control-group .bp3-select select[class*="bp3-intent"]:hover{
        z-index:11; }
      .bp3-control-group .bp3-button[class*="bp3-intent"]:active,
      .bp3-control-group .bp3-html-select select[class*="bp3-intent"]:active,
      .bp3-control-group .bp3-select select[class*="bp3-intent"]:active{
        z-index:12; }
      .bp3-control-group .bp3-button[class*="bp3-intent"][readonly], .bp3-control-group .bp3-button[class*="bp3-intent"]:disabled, .bp3-control-group .bp3-button[class*="bp3-intent"].bp3-disabled,
      .bp3-control-group .bp3-html-select select[class*="bp3-intent"][readonly],
      .bp3-control-group .bp3-html-select select[class*="bp3-intent"]:disabled,
      .bp3-control-group .bp3-html-select select[class*="bp3-intent"].bp3-disabled,
      .bp3-control-group .bp3-select select[class*="bp3-intent"][readonly],
      .bp3-control-group .bp3-select select[class*="bp3-intent"]:disabled,
      .bp3-control-group .bp3-select select[class*="bp3-intent"].bp3-disabled{
        z-index:8; }
  .bp3-control-group .bp3-input-group > .bp3-icon,
  .bp3-control-group .bp3-input-group > .bp3-button,
  .bp3-control-group .bp3-input-group > .bp3-input-left-container,
  .bp3-control-group .bp3-input-group > .bp3-input-action{
    z-index:16; }
  .bp3-control-group .bp3-select::after,
  .bp3-control-group .bp3-html-select::after,
  .bp3-control-group .bp3-select > .bp3-icon,
  .bp3-control-group .bp3-html-select > .bp3-icon{
    z-index:17; }
  .bp3-control-group .bp3-select:focus-within{
    z-index:5; }
  .bp3-control-group:not(.bp3-vertical) > *:not(.bp3-divider){
    margin-right:-1px; }
  .bp3-control-group:not(.bp3-vertical) > .bp3-divider:not(:first-child){
    margin-left:6px; }
  .bp3-dark .bp3-control-group:not(.bp3-vertical) > *:not(.bp3-divider){
    margin-right:0; }
  .bp3-dark .bp3-control-group:not(.bp3-vertical) > .bp3-button + .bp3-button{
    margin-left:1px; }
  .bp3-control-group .bp3-popover-wrapper,
  .bp3-control-group .bp3-popover-target{
    border-radius:inherit; }
  .bp3-control-group > :first-child{
    border-radius:3px 0 0 3px; }
  .bp3-control-group > :last-child{
    border-radius:0 3px 3px 0;
    margin-right:0; }
  .bp3-control-group > :only-child{
    border-radius:3px;
    margin-right:0; }
  .bp3-control-group .bp3-input-group .bp3-button{
    border-radius:3px; }
  .bp3-control-group .bp3-numeric-input:not(:first-child) .bp3-input-group{
    border-bottom-left-radius:0;
    border-top-left-radius:0; }
  .bp3-control-group.bp3-fill{
    width:100%; }
  .bp3-control-group > .bp3-fill{
    -webkit-box-flex:1;
        -ms-flex:1 1 auto;
            flex:1 1 auto; }
  .bp3-control-group.bp3-fill > *:not(.bp3-fixed){
    -webkit-box-flex:1;
        -ms-flex:1 1 auto;
            flex:1 1 auto; }
  .bp3-control-group.bp3-vertical{
    -webkit-box-orient:vertical;
    -webkit-box-direction:normal;
        -ms-flex-direction:column;
            flex-direction:column; }
    .bp3-control-group.bp3-vertical > *{
      margin-top:-1px; }
    .bp3-control-group.bp3-vertical > :first-child{
      border-radius:3px 3px 0 0;
      margin-top:0; }
    .bp3-control-group.bp3-vertical > :last-child{
      border-radius:0 0 3px 3px; }
.bp3-control{
  cursor:pointer;
  display:block;
  margin-bottom:10px;
  position:relative;
  text-transform:none; }
  .bp3-control input:checked ~ .bp3-control-indicator{
    background-color:#137cbd;
    background-image:-webkit-gradient(linear, left top, left bottom, from(rgba(255, 255, 255, 0.1)), to(rgba(255, 255, 255, 0)));
    background-image:linear-gradient(to bottom, rgba(255, 255, 255, 0.1), rgba(255, 255, 255, 0));
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2);
    color:#ffffff; }
  .bp3-control:hover input:checked ~ .bp3-control-indicator{
    background-color:#106ba3;
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2); }
  .bp3-control input:not(:disabled):active:checked ~ .bp3-control-indicator{
    background:#0e5a8a;
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 1px 2px rgba(16, 22, 26, 0.2);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 1px 2px rgba(16, 22, 26, 0.2); }
  .bp3-control input:disabled:checked ~ .bp3-control-indicator{
    background:rgba(19, 124, 189, 0.5);
    -webkit-box-shadow:none;
            box-shadow:none; }
  .bp3-dark .bp3-control input:checked ~ .bp3-control-indicator{
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4); }
  .bp3-dark .bp3-control:hover input:checked ~ .bp3-control-indicator{
    background-color:#106ba3;
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4); }
  .bp3-dark .bp3-control input:not(:disabled):active:checked ~ .bp3-control-indicator{
    background-color:#0e5a8a;
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 1px 2px rgba(16, 22, 26, 0.2);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 1px 2px rgba(16, 22, 26, 0.2); }
  .bp3-dark .bp3-control input:disabled:checked ~ .bp3-control-indicator{
    background:rgba(14, 90, 138, 0.5);
    -webkit-box-shadow:none;
            box-shadow:none; }
  .bp3-control:not(.bp3-align-right){
    padding-left:26px; }
    .bp3-control:not(.bp3-align-right) .bp3-control-indicator{
      margin-left:-26px; }
  .bp3-control.bp3-align-right{
    padding-right:26px; }
    .bp3-control.bp3-align-right .bp3-control-indicator{
      margin-right:-26px; }
  .bp3-control.bp3-disabled{
    color:rgba(92, 112, 128, 0.6);
    cursor:not-allowed; }
  .bp3-control.bp3-inline{
    display:inline-block;
    margin-right:20px; }
  .bp3-control input{
    left:0;
    opacity:0;
    position:absolute;
    top:0;
    z-index:-1; }
  .bp3-control .bp3-control-indicator{
    background-clip:padding-box;
    background-color:#f5f8fa;
    background-image:-webkit-gradient(linear, left top, left bottom, from(rgba(255, 255, 255, 0.8)), to(rgba(255, 255, 255, 0)));
    background-image:linear-gradient(to bottom, rgba(255, 255, 255, 0.8), rgba(255, 255, 255, 0));
    border:none;
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1);
    cursor:pointer;
    display:inline-block;
    font-size:16px;
    height:1em;
    margin-right:10px;
    margin-top:-3px;
    position:relative;
    -webkit-user-select:none;
       -moz-user-select:none;
        -ms-user-select:none;
            user-select:none;
    vertical-align:middle;
    width:1em; }
    .bp3-control .bp3-control-indicator::before{
      content:"";
      display:block;
      height:1em;
      width:1em; }
  .bp3-control:hover .bp3-control-indicator{
    background-color:#ebf1f5; }
  .bp3-control input:not(:disabled):active ~ .bp3-control-indicator{
    background:#d8e1e8;
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 1px 2px rgba(16, 22, 26, 0.2);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 1px 2px rgba(16, 22, 26, 0.2); }
  .bp3-control input:disabled ~ .bp3-control-indicator{
    background:rgba(206, 217, 224, 0.5);
    -webkit-box-shadow:none;
            box-shadow:none;
    cursor:not-allowed; }
  .bp3-control input:focus ~ .bp3-control-indicator{
    outline:rgba(19, 124, 189, 0.6) auto 2px;
    outline-offset:2px;
    -moz-outline-radius:6px; }
  .bp3-control.bp3-align-right .bp3-control-indicator{
    float:right;
    margin-left:10px;
    margin-top:1px; }
  .bp3-control.bp3-large{
    font-size:16px; }
    .bp3-control.bp3-large:not(.bp3-align-right){
      padding-left:30px; }
      .bp3-control.bp3-large:not(.bp3-align-right) .bp3-control-indicator{
        margin-left:-30px; }
    .bp3-control.bp3-large.bp3-align-right{
      padding-right:30px; }
      .bp3-control.bp3-large.bp3-align-right .bp3-control-indicator{
        margin-right:-30px; }
    .bp3-control.bp3-large .bp3-control-indicator{
      font-size:20px; }
    .bp3-control.bp3-large.bp3-align-right .bp3-control-indicator{
      margin-top:0; }
  .bp3-control.bp3-checkbox input:indeterminate ~ .bp3-control-indicator{
    background-color:#137cbd;
    background-image:-webkit-gradient(linear, left top, left bottom, from(rgba(255, 255, 255, 0.1)), to(rgba(255, 255, 255, 0)));
    background-image:linear-gradient(to bottom, rgba(255, 255, 255, 0.1), rgba(255, 255, 255, 0));
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2);
    color:#ffffff; }
  .bp3-control.bp3-checkbox:hover input:indeterminate ~ .bp3-control-indicator{
    background-color:#106ba3;
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2); }
  .bp3-control.bp3-checkbox input:not(:disabled):active:indeterminate ~ .bp3-control-indicator{
    background:#0e5a8a;
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 1px 2px rgba(16, 22, 26, 0.2);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 1px 2px rgba(16, 22, 26, 0.2); }
  .bp3-control.bp3-checkbox input:disabled:indeterminate ~ .bp3-control-indicator{
    background:rgba(19, 124, 189, 0.5);
    -webkit-box-shadow:none;
            box-shadow:none; }
  .bp3-dark .bp3-control.bp3-checkbox input:indeterminate ~ .bp3-control-indicator{
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4); }
  .bp3-dark .bp3-control.bp3-checkbox:hover input:indeterminate ~ .bp3-control-indicator{
    background-color:#106ba3;
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4); }
  .bp3-dark .bp3-control.bp3-checkbox input:not(:disabled):active:indeterminate ~ .bp3-control-indicator{
    background-color:#0e5a8a;
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 1px 2px rgba(16, 22, 26, 0.2);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 1px 2px rgba(16, 22, 26, 0.2); }
  .bp3-dark .bp3-control.bp3-checkbox input:disabled:indeterminate ~ .bp3-control-indicator{
    background:rgba(14, 90, 138, 0.5);
    -webkit-box-shadow:none;
            box-shadow:none; }
  .bp3-control.bp3-checkbox .bp3-control-indicator{
    border-radius:3px; }
  .bp3-control.bp3-checkbox input:checked ~ .bp3-control-indicator::before{
    background-image:url("data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 16 16'%3e%3cpath fill-rule='evenodd' clip-rule='evenodd' d='M12 5c-.28 0-.53.11-.71.29L7 9.59l-2.29-2.3a1.003 1.003 0 00-1.42 1.42l3 3c.18.18.43.29.71.29s.53-.11.71-.29l5-5A1.003 1.003 0 0012 5z' fill='white'/%3e%3c/svg%3e"); }
  .bp3-control.bp3-checkbox input:indeterminate ~ .bp3-control-indicator::before{
    background-image:url("data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 16 16'%3e%3cpath fill-rule='evenodd' clip-rule='evenodd' d='M11 7H5c-.55 0-1 .45-1 1s.45 1 1 1h6c.55 0 1-.45 1-1s-.45-1-1-1z' fill='white'/%3e%3c/svg%3e"); }
  .bp3-control.bp3-radio .bp3-control-indicator{
    border-radius:50%; }
  .bp3-control.bp3-radio input:checked ~ .bp3-control-indicator::before{
    background-image:radial-gradient(#ffffff, #ffffff 28%, transparent 32%); }
  .bp3-control.bp3-radio input:checked:disabled ~ .bp3-control-indicator::before{
    opacity:0.5; }
  .bp3-control.bp3-radio input:focus ~ .bp3-control-indicator{
    -moz-outline-radius:16px; }
  .bp3-control.bp3-switch input ~ .bp3-control-indicator{
    background:rgba(167, 182, 194, 0.5); }
  .bp3-control.bp3-switch:hover input ~ .bp3-control-indicator{
    background:rgba(115, 134, 148, 0.5); }
  .bp3-control.bp3-switch input:not(:disabled):active ~ .bp3-control-indicator{
    background:rgba(92, 112, 128, 0.5); }
  .bp3-control.bp3-switch input:disabled ~ .bp3-control-indicator{
    background:rgba(206, 217, 224, 0.5); }
    .bp3-control.bp3-switch input:disabled ~ .bp3-control-indicator::before{
      background:rgba(255, 255, 255, 0.8); }
  .bp3-control.bp3-switch input:checked ~ .bp3-control-indicator{
    background:#137cbd; }
  .bp3-control.bp3-switch:hover input:checked ~ .bp3-control-indicator{
    background:#106ba3; }
  .bp3-control.bp3-switch input:checked:not(:disabled):active ~ .bp3-control-indicator{
    background:#0e5a8a; }
  .bp3-control.bp3-switch input:checked:disabled ~ .bp3-control-indicator{
    background:rgba(19, 124, 189, 0.5); }
    .bp3-control.bp3-switch input:checked:disabled ~ .bp3-control-indicator::before{
      background:rgba(255, 255, 255, 0.8); }
  .bp3-control.bp3-switch:not(.bp3-align-right){
    padding-left:38px; }
    .bp3-control.bp3-switch:not(.bp3-align-right) .bp3-control-indicator{
      margin-left:-38px; }
  .bp3-control.bp3-switch.bp3-align-right{
    padding-right:38px; }
    .bp3-control.bp3-switch.bp3-align-right .bp3-control-indicator{
      margin-right:-38px; }
  .bp3-control.bp3-switch .bp3-control-indicator{
    border:none;
    border-radius:1.75em;
    -webkit-box-shadow:none !important;
            box-shadow:none !important;
    min-width:1.75em;
    -webkit-transition:background-color 100ms cubic-bezier(0.4, 1, 0.75, 0.9);
    transition:background-color 100ms cubic-bezier(0.4, 1, 0.75, 0.9);
    width:auto; }
    .bp3-control.bp3-switch .bp3-control-indicator::before{
      background:#ffffff;
      border-radius:50%;
      -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 1px 1px rgba(16, 22, 26, 0.2);
              box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 1px 1px rgba(16, 22, 26, 0.2);
      height:calc(1em - 4px);
      left:0;
      margin:2px;
      position:absolute;
      -webkit-transition:left 100ms cubic-bezier(0.4, 1, 0.75, 0.9);
      transition:left 100ms cubic-bezier(0.4, 1, 0.75, 0.9);
      width:calc(1em - 4px); }
  .bp3-control.bp3-switch input:checked ~ .bp3-control-indicator::before{
    left:calc(100% - 1em); }
  .bp3-control.bp3-switch.bp3-large:not(.bp3-align-right){
    padding-left:45px; }
    .bp3-control.bp3-switch.bp3-large:not(.bp3-align-right) .bp3-control-indicator{
      margin-left:-45px; }
  .bp3-control.bp3-switch.bp3-large.bp3-align-right{
    padding-right:45px; }
    .bp3-control.bp3-switch.bp3-large.bp3-align-right .bp3-control-indicator{
      margin-right:-45px; }
  .bp3-dark .bp3-control.bp3-switch input ~ .bp3-control-indicator{
    background:rgba(16, 22, 26, 0.5); }
  .bp3-dark .bp3-control.bp3-switch:hover input ~ .bp3-control-indicator{
    background:rgba(16, 22, 26, 0.7); }
  .bp3-dark .bp3-control.bp3-switch input:not(:disabled):active ~ .bp3-control-indicator{
    background:rgba(16, 22, 26, 0.9); }
  .bp3-dark .bp3-control.bp3-switch input:disabled ~ .bp3-control-indicator{
    background:rgba(57, 75, 89, 0.5); }
    .bp3-dark .bp3-control.bp3-switch input:disabled ~ .bp3-control-indicator::before{
      background:rgba(16, 22, 26, 0.4); }
  .bp3-dark .bp3-control.bp3-switch input:checked ~ .bp3-control-indicator{
    background:#137cbd; }
  .bp3-dark .bp3-control.bp3-switch:hover input:checked ~ .bp3-control-indicator{
    background:#106ba3; }
  .bp3-dark .bp3-control.bp3-switch input:checked:not(:disabled):active ~ .bp3-control-indicator{
    background:#0e5a8a; }
  .bp3-dark .bp3-control.bp3-switch input:checked:disabled ~ .bp3-control-indicator{
    background:rgba(14, 90, 138, 0.5); }
    .bp3-dark .bp3-control.bp3-switch input:checked:disabled ~ .bp3-control-indicator::before{
      background:rgba(16, 22, 26, 0.4); }
  .bp3-dark .bp3-control.bp3-switch .bp3-control-indicator::before{
    background:#394b59;
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4); }
  .bp3-dark .bp3-control.bp3-switch input:checked ~ .bp3-control-indicator::before{
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4); }
  .bp3-control.bp3-switch .bp3-switch-inner-text{
    font-size:0.7em;
    text-align:center; }
  .bp3-control.bp3-switch .bp3-control-indicator-child:first-child{
    line-height:0;
    margin-left:0.5em;
    margin-right:1.2em;
    visibility:hidden; }
  .bp3-control.bp3-switch .bp3-control-indicator-child:last-child{
    line-height:1em;
    margin-left:1.2em;
    margin-right:0.5em;
    visibility:visible; }
  .bp3-control.bp3-switch input:checked ~ .bp3-control-indicator .bp3-control-indicator-child:first-child{
    line-height:1em;
    visibility:visible; }
  .bp3-control.bp3-switch input:checked ~ .bp3-control-indicator .bp3-control-indicator-child:last-child{
    line-height:0;
    visibility:hidden; }
  .bp3-dark .bp3-control{
    color:#f5f8fa; }
    .bp3-dark .bp3-control.bp3-disabled{
      color:rgba(167, 182, 194, 0.6); }
    .bp3-dark .bp3-control .bp3-control-indicator{
      background-color:#394b59;
      background-image:-webkit-gradient(linear, left top, left bottom, from(rgba(255, 255, 255, 0.05)), to(rgba(255, 255, 255, 0)));
      background-image:linear-gradient(to bottom, rgba(255, 255, 255, 0.05), rgba(255, 255, 255, 0));
      -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4);
              box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4); }
    .bp3-dark .bp3-control:hover .bp3-control-indicator{
      background-color:#30404d; }
    .bp3-dark .bp3-control input:not(:disabled):active ~ .bp3-control-indicator{
      background:#202b33;
      -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.6), inset 0 1px 2px rgba(16, 22, 26, 0.2);
              box-shadow:0 0 0 1px rgba(16, 22, 26, 0.6), inset 0 1px 2px rgba(16, 22, 26, 0.2); }
    .bp3-dark .bp3-control input:disabled ~ .bp3-control-indicator{
      background:rgba(57, 75, 89, 0.5);
      -webkit-box-shadow:none;
              box-shadow:none;
      cursor:not-allowed; }
    .bp3-dark .bp3-control.bp3-checkbox input:disabled:checked ~ .bp3-control-indicator, .bp3-dark .bp3-control.bp3-checkbox input:disabled:indeterminate ~ .bp3-control-indicator{
      color:rgba(167, 182, 194, 0.6); }
.bp3-file-input{
  cursor:pointer;
  display:inline-block;
  height:30px;
  position:relative; }
  .bp3-file-input input{
    margin:0;
    min-width:200px;
    opacity:0; }
    .bp3-file-input input:disabled + .bp3-file-upload-input,
    .bp3-file-input input.bp3-disabled + .bp3-file-upload-input{
      background:rgba(206, 217, 224, 0.5);
      -webkit-box-shadow:none;
              box-shadow:none;
      color:rgba(92, 112, 128, 0.6);
      cursor:not-allowed;
      resize:none; }
      .bp3-file-input input:disabled + .bp3-file-upload-input::after,
      .bp3-file-input input.bp3-disabled + .bp3-file-upload-input::after{
        background-color:rgba(206, 217, 224, 0.5);
        background-image:none;
        -webkit-box-shadow:none;
                box-shadow:none;
        color:rgba(92, 112, 128, 0.6);
        cursor:not-allowed;
        outline:none; }
        .bp3-file-input input:disabled + .bp3-file-upload-input::after.bp3-active, .bp3-file-input input:disabled + .bp3-file-upload-input::after.bp3-active:hover,
        .bp3-file-input input.bp3-disabled + .bp3-file-upload-input::after.bp3-active,
        .bp3-file-input input.bp3-disabled + .bp3-file-upload-input::after.bp3-active:hover{
          background:rgba(206, 217, 224, 0.7); }
      .bp3-dark .bp3-file-input input:disabled + .bp3-file-upload-input, .bp3-dark
      .bp3-file-input input.bp3-disabled + .bp3-file-upload-input{
        background:rgba(57, 75, 89, 0.5);
        -webkit-box-shadow:none;
                box-shadow:none;
        color:rgba(167, 182, 194, 0.6); }
        .bp3-dark .bp3-file-input input:disabled + .bp3-file-upload-input::after, .bp3-dark
        .bp3-file-input input.bp3-disabled + .bp3-file-upload-input::after{
          background-color:rgba(57, 75, 89, 0.5);
          background-image:none;
          -webkit-box-shadow:none;
                  box-shadow:none;
          color:rgba(167, 182, 194, 0.6); }
          .bp3-dark .bp3-file-input input:disabled + .bp3-file-upload-input::after.bp3-active, .bp3-dark
          .bp3-file-input input.bp3-disabled + .bp3-file-upload-input::after.bp3-active{
            background:rgba(57, 75, 89, 0.7); }
  .bp3-file-input.bp3-file-input-has-selection .bp3-file-upload-input{
    color:#182026; }
  .bp3-dark .bp3-file-input.bp3-file-input-has-selection .bp3-file-upload-input{
    color:#f5f8fa; }
  .bp3-file-input.bp3-fill{
    width:100%; }
  .bp3-file-input.bp3-large,
  .bp3-large .bp3-file-input{
    height:40px; }
  .bp3-file-input .bp3-file-upload-input-custom-text::after{
    content:attr(bp3-button-text); }

.bp3-file-upload-input{
  -webkit-appearance:none;
     -moz-appearance:none;
          appearance:none;
  background:#ffffff;
  border:none;
  border-radius:3px;
  -webkit-box-shadow:0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), inset 0 0 0 1px rgba(16, 22, 26, 0.15), inset 0 1px 1px rgba(16, 22, 26, 0.2);
          box-shadow:0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), inset 0 0 0 1px rgba(16, 22, 26, 0.15), inset 0 1px 1px rgba(16, 22, 26, 0.2);
  color:#182026;
  font-size:14px;
  font-weight:400;
  height:30px;
  line-height:30px;
  outline:none;
  padding:0 10px;
  -webkit-transition:-webkit-box-shadow 100ms cubic-bezier(0.4, 1, 0.75, 0.9);
  transition:-webkit-box-shadow 100ms cubic-bezier(0.4, 1, 0.75, 0.9);
  transition:box-shadow 100ms cubic-bezier(0.4, 1, 0.75, 0.9);
  transition:box-shadow 100ms cubic-bezier(0.4, 1, 0.75, 0.9), -webkit-box-shadow 100ms cubic-bezier(0.4, 1, 0.75, 0.9);
  vertical-align:middle;
  overflow:hidden;
  text-overflow:ellipsis;
  white-space:nowrap;
  word-wrap:normal;
  color:rgba(92, 112, 128, 0.6);
  left:0;
  padding-right:80px;
  position:absolute;
  right:0;
  top:0;
  -webkit-user-select:none;
     -moz-user-select:none;
      -ms-user-select:none;
          user-select:none; }
  .bp3-file-upload-input::-webkit-input-placeholder{
    color:rgba(92, 112, 128, 0.6);
    opacity:1; }
  .bp3-file-upload-input::-moz-placeholder{
    color:rgba(92, 112, 128, 0.6);
    opacity:1; }
  .bp3-file-upload-input:-ms-input-placeholder{
    color:rgba(92, 112, 128, 0.6);
    opacity:1; }
  .bp3-file-upload-input::-ms-input-placeholder{
    color:rgba(92, 112, 128, 0.6);
    opacity:1; }
  .bp3-file-upload-input::placeholder{
    color:rgba(92, 112, 128, 0.6);
    opacity:1; }
  .bp3-file-upload-input:focus, .bp3-file-upload-input.bp3-active{
    -webkit-box-shadow:0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2);
            box-shadow:0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
  .bp3-file-upload-input[type="search"], .bp3-file-upload-input.bp3-round{
    border-radius:30px;
    -webkit-box-sizing:border-box;
            box-sizing:border-box;
    padding-left:10px; }
  .bp3-file-upload-input[readonly]{
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.15);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.15); }
  .bp3-file-upload-input:disabled, .bp3-file-upload-input.bp3-disabled{
    background:rgba(206, 217, 224, 0.5);
    -webkit-box-shadow:none;
            box-shadow:none;
    color:rgba(92, 112, 128, 0.6);
    cursor:not-allowed;
    resize:none; }
  .bp3-file-upload-input::after{
    background-color:#f5f8fa;
    background-image:-webkit-gradient(linear, left top, left bottom, from(rgba(255, 255, 255, 0.8)), to(rgba(255, 255, 255, 0)));
    background-image:linear-gradient(to bottom, rgba(255, 255, 255, 0.8), rgba(255, 255, 255, 0));
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1);
    color:#182026;
    min-height:24px;
    min-width:24px;
    overflow:hidden;
    text-overflow:ellipsis;
    white-space:nowrap;
    word-wrap:normal;
    border-radius:3px;
    content:"Browse";
    line-height:24px;
    margin:3px;
    position:absolute;
    right:0;
    text-align:center;
    top:0;
    width:70px; }
    .bp3-file-upload-input::after:hover{
      background-clip:padding-box;
      background-color:#ebf1f5;
      -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1);
              box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1); }
    .bp3-file-upload-input::after:active, .bp3-file-upload-input::after.bp3-active{
      background-color:#d8e1e8;
      background-image:none;
      -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 1px 2px rgba(16, 22, 26, 0.2);
              box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 1px 2px rgba(16, 22, 26, 0.2); }
    .bp3-file-upload-input::after:disabled, .bp3-file-upload-input::after.bp3-disabled{
      background-color:rgba(206, 217, 224, 0.5);
      background-image:none;
      -webkit-box-shadow:none;
              box-shadow:none;
      color:rgba(92, 112, 128, 0.6);
      cursor:not-allowed;
      outline:none; }
      .bp3-file-upload-input::after:disabled.bp3-active, .bp3-file-upload-input::after:disabled.bp3-active:hover, .bp3-file-upload-input::after.bp3-disabled.bp3-active, .bp3-file-upload-input::after.bp3-disabled.bp3-active:hover{
        background:rgba(206, 217, 224, 0.7); }
  .bp3-file-upload-input:hover::after{
    background-clip:padding-box;
    background-color:#ebf1f5;
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1); }
  .bp3-file-upload-input:active::after{
    background-color:#d8e1e8;
    background-image:none;
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 1px 2px rgba(16, 22, 26, 0.2);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 1px 2px rgba(16, 22, 26, 0.2); }
  .bp3-large .bp3-file-upload-input{
    font-size:16px;
    height:40px;
    line-height:40px;
    padding-right:95px; }
    .bp3-large .bp3-file-upload-input[type="search"], .bp3-large .bp3-file-upload-input.bp3-round{
      padding:0 15px; }
    .bp3-large .bp3-file-upload-input::after{
      min-height:30px;
      min-width:30px;
      line-height:30px;
      margin:5px;
      width:85px; }
  .bp3-dark .bp3-file-upload-input{
    background:rgba(16, 22, 26, 0.3);
    -webkit-box-shadow:0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
    color:#f5f8fa;
    color:rgba(167, 182, 194, 0.6); }
    .bp3-dark .bp3-file-upload-input::-webkit-input-placeholder{
      color:rgba(167, 182, 194, 0.6); }
    .bp3-dark .bp3-file-upload-input::-moz-placeholder{
      color:rgba(167, 182, 194, 0.6); }
    .bp3-dark .bp3-file-upload-input:-ms-input-placeholder{
      color:rgba(167, 182, 194, 0.6); }
    .bp3-dark .bp3-file-upload-input::-ms-input-placeholder{
      color:rgba(167, 182, 194, 0.6); }
    .bp3-dark .bp3-file-upload-input::placeholder{
      color:rgba(167, 182, 194, 0.6); }
    .bp3-dark .bp3-file-upload-input:focus{
      -webkit-box-shadow:0 0 0 1px #137cbd, 0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
              box-shadow:0 0 0 1px #137cbd, 0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4); }
    .bp3-dark .bp3-file-upload-input[readonly]{
      -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4);
              box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4); }
    .bp3-dark .bp3-file-upload-input:disabled, .bp3-dark .bp3-file-upload-input.bp3-disabled{
      background:rgba(57, 75, 89, 0.5);
      -webkit-box-shadow:none;
              box-shadow:none;
      color:rgba(167, 182, 194, 0.6); }
    .bp3-dark .bp3-file-upload-input::after{
      background-color:#394b59;
      background-image:-webkit-gradient(linear, left top, left bottom, from(rgba(255, 255, 255, 0.05)), to(rgba(255, 255, 255, 0)));
      background-image:linear-gradient(to bottom, rgba(255, 255, 255, 0.05), rgba(255, 255, 255, 0));
      -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4);
              box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4);
      color:#f5f8fa; }
      .bp3-dark .bp3-file-upload-input::after:hover, .bp3-dark .bp3-file-upload-input::after:active, .bp3-dark .bp3-file-upload-input::after.bp3-active{
        color:#f5f8fa; }
      .bp3-dark .bp3-file-upload-input::after:hover{
        background-color:#30404d;
        -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4);
                box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4); }
      .bp3-dark .bp3-file-upload-input::after:active, .bp3-dark .bp3-file-upload-input::after.bp3-active{
        background-color:#202b33;
        background-image:none;
        -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.6), inset 0 1px 2px rgba(16, 22, 26, 0.2);
                box-shadow:0 0 0 1px rgba(16, 22, 26, 0.6), inset 0 1px 2px rgba(16, 22, 26, 0.2); }
      .bp3-dark .bp3-file-upload-input::after:disabled, .bp3-dark .bp3-file-upload-input::after.bp3-disabled{
        background-color:rgba(57, 75, 89, 0.5);
        background-image:none;
        -webkit-box-shadow:none;
                box-shadow:none;
        color:rgba(167, 182, 194, 0.6); }
        .bp3-dark .bp3-file-upload-input::after:disabled.bp3-active, .bp3-dark .bp3-file-upload-input::after.bp3-disabled.bp3-active{
          background:rgba(57, 75, 89, 0.7); }
      .bp3-dark .bp3-file-upload-input::after .bp3-button-spinner .bp3-spinner-head{
        background:rgba(16, 22, 26, 0.5);
        stroke:#8a9ba8; }
    .bp3-dark .bp3-file-upload-input:hover::after{
      background-color:#30404d;
      -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4);
              box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4); }
    .bp3-dark .bp3-file-upload-input:active::after{
      background-color:#202b33;
      background-image:none;
      -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.6), inset 0 1px 2px rgba(16, 22, 26, 0.2);
              box-shadow:0 0 0 1px rgba(16, 22, 26, 0.6), inset 0 1px 2px rgba(16, 22, 26, 0.2); }
.bp3-file-upload-input::after{
  -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1);
          box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1); }
.bp3-form-group{
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  -webkit-box-orient:vertical;
  -webkit-box-direction:normal;
      -ms-flex-direction:column;
          flex-direction:column;
  margin:0 0 15px; }
  .bp3-form-group label.bp3-label{
    margin-bottom:5px; }
  .bp3-form-group .bp3-control{
    margin-top:7px; }
  .bp3-form-group .bp3-form-helper-text{
    color:#5c7080;
    font-size:12px;
    margin-top:5px; }
  .bp3-form-group.bp3-intent-primary .bp3-form-helper-text{
    color:#106ba3; }
  .bp3-form-group.bp3-intent-success .bp3-form-helper-text{
    color:#0d8050; }
  .bp3-form-group.bp3-intent-warning .bp3-form-helper-text{
    color:#bf7326; }
  .bp3-form-group.bp3-intent-danger .bp3-form-helper-text{
    color:#c23030; }
  .bp3-form-group.bp3-inline{
    -webkit-box-align:start;
        -ms-flex-align:start;
            align-items:flex-start;
    -webkit-box-orient:horizontal;
    -webkit-box-direction:normal;
        -ms-flex-direction:row;
            flex-direction:row; }
    .bp3-form-group.bp3-inline.bp3-large label.bp3-label{
      line-height:40px;
      margin:0 10px 0 0; }
    .bp3-form-group.bp3-inline label.bp3-label{
      line-height:30px;
      margin:0 10px 0 0; }
  .bp3-form-group.bp3-disabled .bp3-label,
  .bp3-form-group.bp3-disabled .bp3-text-muted,
  .bp3-form-group.bp3-disabled .bp3-form-helper-text{
    color:rgba(92, 112, 128, 0.6) !important; }
  .bp3-dark .bp3-form-group.bp3-intent-primary .bp3-form-helper-text{
    color:#48aff0; }
  .bp3-dark .bp3-form-group.bp3-intent-success .bp3-form-helper-text{
    color:#3dcc91; }
  .bp3-dark .bp3-form-group.bp3-intent-warning .bp3-form-helper-text{
    color:#ffb366; }
  .bp3-dark .bp3-form-group.bp3-intent-danger .bp3-form-helper-text{
    color:#ff7373; }
  .bp3-dark .bp3-form-group .bp3-form-helper-text{
    color:#a7b6c2; }
  .bp3-dark .bp3-form-group.bp3-disabled .bp3-label,
  .bp3-dark .bp3-form-group.bp3-disabled .bp3-text-muted,
  .bp3-dark .bp3-form-group.bp3-disabled .bp3-form-helper-text{
    color:rgba(167, 182, 194, 0.6) !important; }
.bp3-input-group{
  display:block;
  position:relative; }
  .bp3-input-group .bp3-input{
    position:relative;
    width:100%; }
    .bp3-input-group .bp3-input:not(:first-child){
      padding-left:30px; }
    .bp3-input-group .bp3-input:not(:last-child){
      padding-right:30px; }
  .bp3-input-group .bp3-input-action,
  .bp3-input-group > .bp3-input-left-container,
  .bp3-input-group > .bp3-button,
  .bp3-input-group > .bp3-icon{
    position:absolute;
    top:0; }
    .bp3-input-group .bp3-input-action:first-child,
    .bp3-input-group > .bp3-input-left-container:first-child,
    .bp3-input-group > .bp3-button:first-child,
    .bp3-input-group > .bp3-icon:first-child{
      left:0; }
    .bp3-input-group .bp3-input-action:last-child,
    .bp3-input-group > .bp3-input-left-container:last-child,
    .bp3-input-group > .bp3-button:last-child,
    .bp3-input-group > .bp3-icon:last-child{
      right:0; }
  .bp3-input-group .bp3-button{
    min-height:24px;
    min-width:24px;
    margin:3px;
    padding:0 7px; }
    .bp3-input-group .bp3-button:empty{
      padding:0; }
  .bp3-input-group > .bp3-input-left-container,
  .bp3-input-group > .bp3-icon{
    z-index:1; }
  .bp3-input-group > .bp3-input-left-container > .bp3-icon,
  .bp3-input-group > .bp3-icon{
    color:#5c7080; }
    .bp3-input-group > .bp3-input-left-container > .bp3-icon:empty,
    .bp3-input-group > .bp3-icon:empty{
      font-family:"Icons16", sans-serif;
      font-size:16px;
      font-style:normal;
      font-weight:400;
      line-height:1;
      -moz-osx-font-smoothing:grayscale;
      -webkit-font-smoothing:antialiased; }
  .bp3-input-group > .bp3-input-left-container > .bp3-icon,
  .bp3-input-group > .bp3-icon,
  .bp3-input-group .bp3-input-action > .bp3-spinner{
    margin:7px; }
  .bp3-input-group .bp3-tag{
    margin:5px; }
  .bp3-input-group .bp3-input:not(:focus) + .bp3-button.bp3-minimal:not(:hover):not(:focus),
  .bp3-input-group .bp3-input:not(:focus) + .bp3-input-action .bp3-button.bp3-minimal:not(:hover):not(:focus){
    color:#5c7080; }
    .bp3-dark .bp3-input-group .bp3-input:not(:focus) + .bp3-button.bp3-minimal:not(:hover):not(:focus), .bp3-dark
    .bp3-input-group .bp3-input:not(:focus) + .bp3-input-action .bp3-button.bp3-minimal:not(:hover):not(:focus){
      color:#a7b6c2; }
    .bp3-input-group .bp3-input:not(:focus) + .bp3-button.bp3-minimal:not(:hover):not(:focus) .bp3-icon, .bp3-input-group .bp3-input:not(:focus) + .bp3-button.bp3-minimal:not(:hover):not(:focus) .bp3-icon-standard, .bp3-input-group .bp3-input:not(:focus) + .bp3-button.bp3-minimal:not(:hover):not(:focus) .bp3-icon-large,
    .bp3-input-group .bp3-input:not(:focus) + .bp3-input-action .bp3-button.bp3-minimal:not(:hover):not(:focus) .bp3-icon,
    .bp3-input-group .bp3-input:not(:focus) + .bp3-input-action .bp3-button.bp3-minimal:not(:hover):not(:focus) .bp3-icon-standard,
    .bp3-input-group .bp3-input:not(:focus) + .bp3-input-action .bp3-button.bp3-minimal:not(:hover):not(:focus) .bp3-icon-large{
      color:#5c7080; }
  .bp3-input-group .bp3-input:not(:focus) + .bp3-button.bp3-minimal:disabled,
  .bp3-input-group .bp3-input:not(:focus) + .bp3-input-action .bp3-button.bp3-minimal:disabled{
    color:rgba(92, 112, 128, 0.6) !important; }
    .bp3-input-group .bp3-input:not(:focus) + .bp3-button.bp3-minimal:disabled .bp3-icon, .bp3-input-group .bp3-input:not(:focus) + .bp3-button.bp3-minimal:disabled .bp3-icon-standard, .bp3-input-group .bp3-input:not(:focus) + .bp3-button.bp3-minimal:disabled .bp3-icon-large,
    .bp3-input-group .bp3-input:not(:focus) + .bp3-input-action .bp3-button.bp3-minimal:disabled .bp3-icon,
    .bp3-input-group .bp3-input:not(:focus) + .bp3-input-action .bp3-button.bp3-minimal:disabled .bp3-icon-standard,
    .bp3-input-group .bp3-input:not(:focus) + .bp3-input-action .bp3-button.bp3-minimal:disabled .bp3-icon-large{
      color:rgba(92, 112, 128, 0.6) !important; }
  .bp3-input-group.bp3-disabled{
    cursor:not-allowed; }
    .bp3-input-group.bp3-disabled .bp3-icon{
      color:rgba(92, 112, 128, 0.6); }
  .bp3-input-group.bp3-large .bp3-button{
    min-height:30px;
    min-width:30px;
    margin:5px; }
  .bp3-input-group.bp3-large > .bp3-input-left-container > .bp3-icon,
  .bp3-input-group.bp3-large > .bp3-icon,
  .bp3-input-group.bp3-large .bp3-input-action > .bp3-spinner{
    margin:12px; }
  .bp3-input-group.bp3-large .bp3-input{
    font-size:16px;
    height:40px;
    line-height:40px; }
    .bp3-input-group.bp3-large .bp3-input[type="search"], .bp3-input-group.bp3-large .bp3-input.bp3-round{
      padding:0 15px; }
    .bp3-input-group.bp3-large .bp3-input:not(:first-child){
      padding-left:40px; }
    .bp3-input-group.bp3-large .bp3-input:not(:last-child){
      padding-right:40px; }
  .bp3-input-group.bp3-small .bp3-button{
    min-height:20px;
    min-width:20px;
    margin:2px; }
  .bp3-input-group.bp3-small .bp3-tag{
    min-height:20px;
    min-width:20px;
    margin:2px; }
  .bp3-input-group.bp3-small > .bp3-input-left-container > .bp3-icon,
  .bp3-input-group.bp3-small > .bp3-icon,
  .bp3-input-group.bp3-small .bp3-input-action > .bp3-spinner{
    margin:4px; }
  .bp3-input-group.bp3-small .bp3-input{
    font-size:12px;
    height:24px;
    line-height:24px;
    padding-left:8px;
    padding-right:8px; }
    .bp3-input-group.bp3-small .bp3-input[type="search"], .bp3-input-group.bp3-small .bp3-input.bp3-round{
      padding:0 12px; }
    .bp3-input-group.bp3-small .bp3-input:not(:first-child){
      padding-left:24px; }
    .bp3-input-group.bp3-small .bp3-input:not(:last-child){
      padding-right:24px; }
  .bp3-input-group.bp3-fill{
    -webkit-box-flex:1;
        -ms-flex:1 1 auto;
            flex:1 1 auto;
    width:100%; }
  .bp3-input-group.bp3-round .bp3-button,
  .bp3-input-group.bp3-round .bp3-input,
  .bp3-input-group.bp3-round .bp3-tag{
    border-radius:30px; }
  .bp3-dark .bp3-input-group .bp3-icon{
    color:#a7b6c2; }
  .bp3-dark .bp3-input-group.bp3-disabled .bp3-icon{
    color:rgba(167, 182, 194, 0.6); }
  .bp3-input-group.bp3-intent-primary .bp3-input{
    -webkit-box-shadow:0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), inset 0 0 0 1px #137cbd, inset 0 0 0 1px rgba(16, 22, 26, 0.15), inset 0 1px 1px rgba(16, 22, 26, 0.2);
            box-shadow:0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), inset 0 0 0 1px #137cbd, inset 0 0 0 1px rgba(16, 22, 26, 0.15), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
    .bp3-input-group.bp3-intent-primary .bp3-input:focus{
      -webkit-box-shadow:0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2);
              box-shadow:0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
    .bp3-input-group.bp3-intent-primary .bp3-input[readonly]{
      -webkit-box-shadow:inset 0 0 0 1px #137cbd;
              box-shadow:inset 0 0 0 1px #137cbd; }
    .bp3-input-group.bp3-intent-primary .bp3-input:disabled, .bp3-input-group.bp3-intent-primary .bp3-input.bp3-disabled{
      -webkit-box-shadow:none;
              box-shadow:none; }
  .bp3-input-group.bp3-intent-primary > .bp3-icon{
    color:#106ba3; }
    .bp3-dark .bp3-input-group.bp3-intent-primary > .bp3-icon{
      color:#48aff0; }
  .bp3-input-group.bp3-intent-success .bp3-input{
    -webkit-box-shadow:0 0 0 0 rgba(15, 153, 96, 0), 0 0 0 0 rgba(15, 153, 96, 0), inset 0 0 0 1px #0f9960, inset 0 0 0 1px rgba(16, 22, 26, 0.15), inset 0 1px 1px rgba(16, 22, 26, 0.2);
            box-shadow:0 0 0 0 rgba(15, 153, 96, 0), 0 0 0 0 rgba(15, 153, 96, 0), inset 0 0 0 1px #0f9960, inset 0 0 0 1px rgba(16, 22, 26, 0.15), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
    .bp3-input-group.bp3-intent-success .bp3-input:focus{
      -webkit-box-shadow:0 0 0 1px #0f9960, 0 0 0 3px rgba(15, 153, 96, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2);
              box-shadow:0 0 0 1px #0f9960, 0 0 0 3px rgba(15, 153, 96, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
    .bp3-input-group.bp3-intent-success .bp3-input[readonly]{
      -webkit-box-shadow:inset 0 0 0 1px #0f9960;
              box-shadow:inset 0 0 0 1px #0f9960; }
    .bp3-input-group.bp3-intent-success .bp3-input:disabled, .bp3-input-group.bp3-intent-success .bp3-input.bp3-disabled{
      -webkit-box-shadow:none;
              box-shadow:none; }
  .bp3-input-group.bp3-intent-success > .bp3-icon{
    color:#0d8050; }
    .bp3-dark .bp3-input-group.bp3-intent-success > .bp3-icon{
      color:#3dcc91; }
  .bp3-input-group.bp3-intent-warning .bp3-input{
    -webkit-box-shadow:0 0 0 0 rgba(217, 130, 43, 0), 0 0 0 0 rgba(217, 130, 43, 0), inset 0 0 0 1px #d9822b, inset 0 0 0 1px rgba(16, 22, 26, 0.15), inset 0 1px 1px rgba(16, 22, 26, 0.2);
            box-shadow:0 0 0 0 rgba(217, 130, 43, 0), 0 0 0 0 rgba(217, 130, 43, 0), inset 0 0 0 1px #d9822b, inset 0 0 0 1px rgba(16, 22, 26, 0.15), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
    .bp3-input-group.bp3-intent-warning .bp3-input:focus{
      -webkit-box-shadow:0 0 0 1px #d9822b, 0 0 0 3px rgba(217, 130, 43, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2);
              box-shadow:0 0 0 1px #d9822b, 0 0 0 3px rgba(217, 130, 43, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
    .bp3-input-group.bp3-intent-warning .bp3-input[readonly]{
      -webkit-box-shadow:inset 0 0 0 1px #d9822b;
              box-shadow:inset 0 0 0 1px #d9822b; }
    .bp3-input-group.bp3-intent-warning .bp3-input:disabled, .bp3-input-group.bp3-intent-warning .bp3-input.bp3-disabled{
      -webkit-box-shadow:none;
              box-shadow:none; }
  .bp3-input-group.bp3-intent-warning > .bp3-icon{
    color:#bf7326; }
    .bp3-dark .bp3-input-group.bp3-intent-warning > .bp3-icon{
      color:#ffb366; }
  .bp3-input-group.bp3-intent-danger .bp3-input{
    -webkit-box-shadow:0 0 0 0 rgba(219, 55, 55, 0), 0 0 0 0 rgba(219, 55, 55, 0), inset 0 0 0 1px #db3737, inset 0 0 0 1px rgba(16, 22, 26, 0.15), inset 0 1px 1px rgba(16, 22, 26, 0.2);
            box-shadow:0 0 0 0 rgba(219, 55, 55, 0), 0 0 0 0 rgba(219, 55, 55, 0), inset 0 0 0 1px #db3737, inset 0 0 0 1px rgba(16, 22, 26, 0.15), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
    .bp3-input-group.bp3-intent-danger .bp3-input:focus{
      -webkit-box-shadow:0 0 0 1px #db3737, 0 0 0 3px rgba(219, 55, 55, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2);
              box-shadow:0 0 0 1px #db3737, 0 0 0 3px rgba(219, 55, 55, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
    .bp3-input-group.bp3-intent-danger .bp3-input[readonly]{
      -webkit-box-shadow:inset 0 0 0 1px #db3737;
              box-shadow:inset 0 0 0 1px #db3737; }
    .bp3-input-group.bp3-intent-danger .bp3-input:disabled, .bp3-input-group.bp3-intent-danger .bp3-input.bp3-disabled{
      -webkit-box-shadow:none;
              box-shadow:none; }
  .bp3-input-group.bp3-intent-danger > .bp3-icon{
    color:#c23030; }
    .bp3-dark .bp3-input-group.bp3-intent-danger > .bp3-icon{
      color:#ff7373; }
.bp3-input{
  -webkit-appearance:none;
     -moz-appearance:none;
          appearance:none;
  background:#ffffff;
  border:none;
  border-radius:3px;
  -webkit-box-shadow:0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), inset 0 0 0 1px rgba(16, 22, 26, 0.15), inset 0 1px 1px rgba(16, 22, 26, 0.2);
          box-shadow:0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), inset 0 0 0 1px rgba(16, 22, 26, 0.15), inset 0 1px 1px rgba(16, 22, 26, 0.2);
  color:#182026;
  font-size:14px;
  font-weight:400;
  height:30px;
  line-height:30px;
  outline:none;
  padding:0 10px;
  -webkit-transition:-webkit-box-shadow 100ms cubic-bezier(0.4, 1, 0.75, 0.9);
  transition:-webkit-box-shadow 100ms cubic-bezier(0.4, 1, 0.75, 0.9);
  transition:box-shadow 100ms cubic-bezier(0.4, 1, 0.75, 0.9);
  transition:box-shadow 100ms cubic-bezier(0.4, 1, 0.75, 0.9), -webkit-box-shadow 100ms cubic-bezier(0.4, 1, 0.75, 0.9);
  vertical-align:middle; }
  .bp3-input::-webkit-input-placeholder{
    color:rgba(92, 112, 128, 0.6);
    opacity:1; }
  .bp3-input::-moz-placeholder{
    color:rgba(92, 112, 128, 0.6);
    opacity:1; }
  .bp3-input:-ms-input-placeholder{
    color:rgba(92, 112, 128, 0.6);
    opacity:1; }
  .bp3-input::-ms-input-placeholder{
    color:rgba(92, 112, 128, 0.6);
    opacity:1; }
  .bp3-input::placeholder{
    color:rgba(92, 112, 128, 0.6);
    opacity:1; }
  .bp3-input:focus, .bp3-input.bp3-active{
    -webkit-box-shadow:0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2);
            box-shadow:0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
  .bp3-input[type="search"], .bp3-input.bp3-round{
    border-radius:30px;
    -webkit-box-sizing:border-box;
            box-sizing:border-box;
    padding-left:10px; }
  .bp3-input[readonly]{
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.15);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.15); }
  .bp3-input:disabled, .bp3-input.bp3-disabled{
    background:rgba(206, 217, 224, 0.5);
    -webkit-box-shadow:none;
            box-shadow:none;
    color:rgba(92, 112, 128, 0.6);
    cursor:not-allowed;
    resize:none; }
  .bp3-input.bp3-large{
    font-size:16px;
    height:40px;
    line-height:40px; }
    .bp3-input.bp3-large[type="search"], .bp3-input.bp3-large.bp3-round{
      padding:0 15px; }
  .bp3-input.bp3-small{
    font-size:12px;
    height:24px;
    line-height:24px;
    padding-left:8px;
    padding-right:8px; }
    .bp3-input.bp3-small[type="search"], .bp3-input.bp3-small.bp3-round{
      padding:0 12px; }
  .bp3-input.bp3-fill{
    -webkit-box-flex:1;
        -ms-flex:1 1 auto;
            flex:1 1 auto;
    width:100%; }
  .bp3-dark .bp3-input{
    background:rgba(16, 22, 26, 0.3);
    -webkit-box-shadow:0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
    color:#f5f8fa; }
    .bp3-dark .bp3-input::-webkit-input-placeholder{
      color:rgba(167, 182, 194, 0.6); }
    .bp3-dark .bp3-input::-moz-placeholder{
      color:rgba(167, 182, 194, 0.6); }
    .bp3-dark .bp3-input:-ms-input-placeholder{
      color:rgba(167, 182, 194, 0.6); }
    .bp3-dark .bp3-input::-ms-input-placeholder{
      color:rgba(167, 182, 194, 0.6); }
    .bp3-dark .bp3-input::placeholder{
      color:rgba(167, 182, 194, 0.6); }
    .bp3-dark .bp3-input:focus{
      -webkit-box-shadow:0 0 0 1px #137cbd, 0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
              box-shadow:0 0 0 1px #137cbd, 0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4); }
    .bp3-dark .bp3-input[readonly]{
      -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4);
              box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4); }
    .bp3-dark .bp3-input:disabled, .bp3-dark .bp3-input.bp3-disabled{
      background:rgba(57, 75, 89, 0.5);
      -webkit-box-shadow:none;
              box-shadow:none;
      color:rgba(167, 182, 194, 0.6); }
  .bp3-input.bp3-intent-primary{
    -webkit-box-shadow:0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), inset 0 0 0 1px #137cbd, inset 0 0 0 1px rgba(16, 22, 26, 0.15), inset 0 1px 1px rgba(16, 22, 26, 0.2);
            box-shadow:0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), inset 0 0 0 1px #137cbd, inset 0 0 0 1px rgba(16, 22, 26, 0.15), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
    .bp3-input.bp3-intent-primary:focus{
      -webkit-box-shadow:0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2);
              box-shadow:0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
    .bp3-input.bp3-intent-primary[readonly]{
      -webkit-box-shadow:inset 0 0 0 1px #137cbd;
              box-shadow:inset 0 0 0 1px #137cbd; }
    .bp3-input.bp3-intent-primary:disabled, .bp3-input.bp3-intent-primary.bp3-disabled{
      -webkit-box-shadow:none;
              box-shadow:none; }
    .bp3-dark .bp3-input.bp3-intent-primary{
      -webkit-box-shadow:0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), inset 0 0 0 1px #137cbd, inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
              box-shadow:0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), inset 0 0 0 1px #137cbd, inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4); }
      .bp3-dark .bp3-input.bp3-intent-primary:focus{
        -webkit-box-shadow:0 0 0 1px #137cbd, 0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
                box-shadow:0 0 0 1px #137cbd, 0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4); }
      .bp3-dark .bp3-input.bp3-intent-primary[readonly]{
        -webkit-box-shadow:inset 0 0 0 1px #137cbd;
                box-shadow:inset 0 0 0 1px #137cbd; }
      .bp3-dark .bp3-input.bp3-intent-primary:disabled, .bp3-dark .bp3-input.bp3-intent-primary.bp3-disabled{
        -webkit-box-shadow:none;
                box-shadow:none; }
  .bp3-input.bp3-intent-success{
    -webkit-box-shadow:0 0 0 0 rgba(15, 153, 96, 0), 0 0 0 0 rgba(15, 153, 96, 0), inset 0 0 0 1px #0f9960, inset 0 0 0 1px rgba(16, 22, 26, 0.15), inset 0 1px 1px rgba(16, 22, 26, 0.2);
            box-shadow:0 0 0 0 rgba(15, 153, 96, 0), 0 0 0 0 rgba(15, 153, 96, 0), inset 0 0 0 1px #0f9960, inset 0 0 0 1px rgba(16, 22, 26, 0.15), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
    .bp3-input.bp3-intent-success:focus{
      -webkit-box-shadow:0 0 0 1px #0f9960, 0 0 0 3px rgba(15, 153, 96, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2);
              box-shadow:0 0 0 1px #0f9960, 0 0 0 3px rgba(15, 153, 96, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
    .bp3-input.bp3-intent-success[readonly]{
      -webkit-box-shadow:inset 0 0 0 1px #0f9960;
              box-shadow:inset 0 0 0 1px #0f9960; }
    .bp3-input.bp3-intent-success:disabled, .bp3-input.bp3-intent-success.bp3-disabled{
      -webkit-box-shadow:none;
              box-shadow:none; }
    .bp3-dark .bp3-input.bp3-intent-success{
      -webkit-box-shadow:0 0 0 0 rgba(15, 153, 96, 0), 0 0 0 0 rgba(15, 153, 96, 0), 0 0 0 0 rgba(15, 153, 96, 0), inset 0 0 0 1px #0f9960, inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
              box-shadow:0 0 0 0 rgba(15, 153, 96, 0), 0 0 0 0 rgba(15, 153, 96, 0), 0 0 0 0 rgba(15, 153, 96, 0), inset 0 0 0 1px #0f9960, inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4); }
      .bp3-dark .bp3-input.bp3-intent-success:focus{
        -webkit-box-shadow:0 0 0 1px #0f9960, 0 0 0 1px #0f9960, 0 0 0 3px rgba(15, 153, 96, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
                box-shadow:0 0 0 1px #0f9960, 0 0 0 1px #0f9960, 0 0 0 3px rgba(15, 153, 96, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4); }
      .bp3-dark .bp3-input.bp3-intent-success[readonly]{
        -webkit-box-shadow:inset 0 0 0 1px #0f9960;
                box-shadow:inset 0 0 0 1px #0f9960; }
      .bp3-dark .bp3-input.bp3-intent-success:disabled, .bp3-dark .bp3-input.bp3-intent-success.bp3-disabled{
        -webkit-box-shadow:none;
                box-shadow:none; }
  .bp3-input.bp3-intent-warning{
    -webkit-box-shadow:0 0 0 0 rgba(217, 130, 43, 0), 0 0 0 0 rgba(217, 130, 43, 0), inset 0 0 0 1px #d9822b, inset 0 0 0 1px rgba(16, 22, 26, 0.15), inset 0 1px 1px rgba(16, 22, 26, 0.2);
            box-shadow:0 0 0 0 rgba(217, 130, 43, 0), 0 0 0 0 rgba(217, 130, 43, 0), inset 0 0 0 1px #d9822b, inset 0 0 0 1px rgba(16, 22, 26, 0.15), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
    .bp3-input.bp3-intent-warning:focus{
      -webkit-box-shadow:0 0 0 1px #d9822b, 0 0 0 3px rgba(217, 130, 43, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2);
              box-shadow:0 0 0 1px #d9822b, 0 0 0 3px rgba(217, 130, 43, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
    .bp3-input.bp3-intent-warning[readonly]{
      -webkit-box-shadow:inset 0 0 0 1px #d9822b;
              box-shadow:inset 0 0 0 1px #d9822b; }
    .bp3-input.bp3-intent-warning:disabled, .bp3-input.bp3-intent-warning.bp3-disabled{
      -webkit-box-shadow:none;
              box-shadow:none; }
    .bp3-dark .bp3-input.bp3-intent-warning{
      -webkit-box-shadow:0 0 0 0 rgba(217, 130, 43, 0), 0 0 0 0 rgba(217, 130, 43, 0), 0 0 0 0 rgba(217, 130, 43, 0), inset 0 0 0 1px #d9822b, inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
              box-shadow:0 0 0 0 rgba(217, 130, 43, 0), 0 0 0 0 rgba(217, 130, 43, 0), 0 0 0 0 rgba(217, 130, 43, 0), inset 0 0 0 1px #d9822b, inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4); }
      .bp3-dark .bp3-input.bp3-intent-warning:focus{
        -webkit-box-shadow:0 0 0 1px #d9822b, 0 0 0 1px #d9822b, 0 0 0 3px rgba(217, 130, 43, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
                box-shadow:0 0 0 1px #d9822b, 0 0 0 1px #d9822b, 0 0 0 3px rgba(217, 130, 43, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4); }
      .bp3-dark .bp3-input.bp3-intent-warning[readonly]{
        -webkit-box-shadow:inset 0 0 0 1px #d9822b;
                box-shadow:inset 0 0 0 1px #d9822b; }
      .bp3-dark .bp3-input.bp3-intent-warning:disabled, .bp3-dark .bp3-input.bp3-intent-warning.bp3-disabled{
        -webkit-box-shadow:none;
                box-shadow:none; }
  .bp3-input.bp3-intent-danger{
    -webkit-box-shadow:0 0 0 0 rgba(219, 55, 55, 0), 0 0 0 0 rgba(219, 55, 55, 0), inset 0 0 0 1px #db3737, inset 0 0 0 1px rgba(16, 22, 26, 0.15), inset 0 1px 1px rgba(16, 22, 26, 0.2);
            box-shadow:0 0 0 0 rgba(219, 55, 55, 0), 0 0 0 0 rgba(219, 55, 55, 0), inset 0 0 0 1px #db3737, inset 0 0 0 1px rgba(16, 22, 26, 0.15), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
    .bp3-input.bp3-intent-danger:focus{
      -webkit-box-shadow:0 0 0 1px #db3737, 0 0 0 3px rgba(219, 55, 55, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2);
              box-shadow:0 0 0 1px #db3737, 0 0 0 3px rgba(219, 55, 55, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
    .bp3-input.bp3-intent-danger[readonly]{
      -webkit-box-shadow:inset 0 0 0 1px #db3737;
              box-shadow:inset 0 0 0 1px #db3737; }
    .bp3-input.bp3-intent-danger:disabled, .bp3-input.bp3-intent-danger.bp3-disabled{
      -webkit-box-shadow:none;
              box-shadow:none; }
    .bp3-dark .bp3-input.bp3-intent-danger{
      -webkit-box-shadow:0 0 0 0 rgba(219, 55, 55, 0), 0 0 0 0 rgba(219, 55, 55, 0), 0 0 0 0 rgba(219, 55, 55, 0), inset 0 0 0 1px #db3737, inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
              box-shadow:0 0 0 0 rgba(219, 55, 55, 0), 0 0 0 0 rgba(219, 55, 55, 0), 0 0 0 0 rgba(219, 55, 55, 0), inset 0 0 0 1px #db3737, inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4); }
      .bp3-dark .bp3-input.bp3-intent-danger:focus{
        -webkit-box-shadow:0 0 0 1px #db3737, 0 0 0 1px #db3737, 0 0 0 3px rgba(219, 55, 55, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
                box-shadow:0 0 0 1px #db3737, 0 0 0 1px #db3737, 0 0 0 3px rgba(219, 55, 55, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4); }
      .bp3-dark .bp3-input.bp3-intent-danger[readonly]{
        -webkit-box-shadow:inset 0 0 0 1px #db3737;
                box-shadow:inset 0 0 0 1px #db3737; }
      .bp3-dark .bp3-input.bp3-intent-danger:disabled, .bp3-dark .bp3-input.bp3-intent-danger.bp3-disabled{
        -webkit-box-shadow:none;
                box-shadow:none; }
  .bp3-input::-ms-clear{
    display:none; }
textarea.bp3-input{
  max-width:100%;
  padding:10px; }
  textarea.bp3-input, textarea.bp3-input.bp3-large, textarea.bp3-input.bp3-small{
    height:auto;
    line-height:inherit; }
  textarea.bp3-input.bp3-small{
    padding:8px; }
  .bp3-dark textarea.bp3-input{
    background:rgba(16, 22, 26, 0.3);
    -webkit-box-shadow:0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
    color:#f5f8fa; }
    .bp3-dark textarea.bp3-input::-webkit-input-placeholder{
      color:rgba(167, 182, 194, 0.6); }
    .bp3-dark textarea.bp3-input::-moz-placeholder{
      color:rgba(167, 182, 194, 0.6); }
    .bp3-dark textarea.bp3-input:-ms-input-placeholder{
      color:rgba(167, 182, 194, 0.6); }
    .bp3-dark textarea.bp3-input::-ms-input-placeholder{
      color:rgba(167, 182, 194, 0.6); }
    .bp3-dark textarea.bp3-input::placeholder{
      color:rgba(167, 182, 194, 0.6); }
    .bp3-dark textarea.bp3-input:focus{
      -webkit-box-shadow:0 0 0 1px #137cbd, 0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
              box-shadow:0 0 0 1px #137cbd, 0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4); }
    .bp3-dark textarea.bp3-input[readonly]{
      -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4);
              box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4); }
    .bp3-dark textarea.bp3-input:disabled, .bp3-dark textarea.bp3-input.bp3-disabled{
      background:rgba(57, 75, 89, 0.5);
      -webkit-box-shadow:none;
              box-shadow:none;
      color:rgba(167, 182, 194, 0.6); }
label.bp3-label{
  display:block;
  margin-bottom:15px;
  margin-top:0; }
  label.bp3-label .bp3-html-select,
  label.bp3-label .bp3-input,
  label.bp3-label .bp3-select,
  label.bp3-label .bp3-slider,
  label.bp3-label .bp3-popover-wrapper{
    display:block;
    margin-top:5px;
    text-transform:none; }
  label.bp3-label .bp3-button-group{
    margin-top:5px; }
  label.bp3-label .bp3-select select,
  label.bp3-label .bp3-html-select select{
    font-weight:400;
    vertical-align:top;
    width:100%; }
  label.bp3-label.bp3-disabled,
  label.bp3-label.bp3-disabled .bp3-text-muted{
    color:rgba(92, 112, 128, 0.6); }
  label.bp3-label.bp3-inline{
    line-height:30px; }
    label.bp3-label.bp3-inline .bp3-html-select,
    label.bp3-label.bp3-inline .bp3-input,
    label.bp3-label.bp3-inline .bp3-input-group,
    label.bp3-label.bp3-inline .bp3-select,
    label.bp3-label.bp3-inline .bp3-popover-wrapper{
      display:inline-block;
      margin:0 0 0 5px;
      vertical-align:top; }
    label.bp3-label.bp3-inline .bp3-button-group{
      margin:0 0 0 5px; }
    label.bp3-label.bp3-inline .bp3-input-group .bp3-input{
      margin-left:0; }
    label.bp3-label.bp3-inline.bp3-large{
      line-height:40px; }
  label.bp3-label:not(.bp3-inline) .bp3-popover-target{
    display:block; }
  .bp3-dark label.bp3-label{
    color:#f5f8fa; }
    .bp3-dark label.bp3-label.bp3-disabled,
    .bp3-dark label.bp3-label.bp3-disabled .bp3-text-muted{
      color:rgba(167, 182, 194, 0.6); }
.bp3-numeric-input .bp3-button-group.bp3-vertical > .bp3-button{
  -webkit-box-flex:1;
      -ms-flex:1 1 14px;
          flex:1 1 14px;
  min-height:0;
  padding:0;
  width:30px; }
  .bp3-numeric-input .bp3-button-group.bp3-vertical > .bp3-button:first-child{
    border-radius:0 3px 0 0; }
  .bp3-numeric-input .bp3-button-group.bp3-vertical > .bp3-button:last-child{
    border-radius:0 0 3px 0; }

.bp3-numeric-input .bp3-button-group.bp3-vertical:first-child > .bp3-button:first-child{
  border-radius:3px 0 0 0; }

.bp3-numeric-input .bp3-button-group.bp3-vertical:first-child > .bp3-button:last-child{
  border-radius:0 0 0 3px; }

.bp3-numeric-input.bp3-large .bp3-button-group.bp3-vertical > .bp3-button{
  width:40px; }

form{
  display:block; }
.bp3-html-select select,
.bp3-select select{
  display:-webkit-inline-box;
  display:-ms-inline-flexbox;
  display:inline-flex;
  -webkit-box-orient:horizontal;
  -webkit-box-direction:normal;
      -ms-flex-direction:row;
          flex-direction:row;
  -webkit-box-align:center;
      -ms-flex-align:center;
          align-items:center;
  border:none;
  border-radius:3px;
  cursor:pointer;
  font-size:14px;
  -webkit-box-pack:center;
      -ms-flex-pack:center;
          justify-content:center;
  padding:5px 10px;
  text-align:left;
  vertical-align:middle;
  background-color:#f5f8fa;
  background-image:-webkit-gradient(linear, left top, left bottom, from(rgba(255, 255, 255, 0.8)), to(rgba(255, 255, 255, 0)));
  background-image:linear-gradient(to bottom, rgba(255, 255, 255, 0.8), rgba(255, 255, 255, 0));
  -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1);
          box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1);
  color:#182026;
  -moz-appearance:none;
  -webkit-appearance:none;
  border-radius:3px;
  height:30px;
  padding:0 25px 0 10px;
  width:100%; }
  .bp3-html-select select > *, .bp3-select select > *{
    -webkit-box-flex:0;
        -ms-flex-positive:0;
            flex-grow:0;
    -ms-flex-negative:0;
        flex-shrink:0; }
  .bp3-html-select select > .bp3-fill, .bp3-select select > .bp3-fill{
    -webkit-box-flex:1;
        -ms-flex-positive:1;
            flex-grow:1;
    -ms-flex-negative:1;
        flex-shrink:1; }
  .bp3-html-select select::before,
  .bp3-select select::before, .bp3-html-select select > *, .bp3-select select > *{
    margin-right:7px; }
  .bp3-html-select select:empty::before,
  .bp3-select select:empty::before,
  .bp3-html-select select > :last-child,
  .bp3-select select > :last-child{
    margin-right:0; }
  .bp3-html-select select:hover,
  .bp3-select select:hover{
    background-clip:padding-box;
    background-color:#ebf1f5;
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1); }
  .bp3-html-select select:active,
  .bp3-select select:active, .bp3-html-select select.bp3-active,
  .bp3-select select.bp3-active{
    background-color:#d8e1e8;
    background-image:none;
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 1px 2px rgba(16, 22, 26, 0.2);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 1px 2px rgba(16, 22, 26, 0.2); }
  .bp3-html-select select:disabled,
  .bp3-select select:disabled, .bp3-html-select select.bp3-disabled,
  .bp3-select select.bp3-disabled{
    background-color:rgba(206, 217, 224, 0.5);
    background-image:none;
    -webkit-box-shadow:none;
            box-shadow:none;
    color:rgba(92, 112, 128, 0.6);
    cursor:not-allowed;
    outline:none; }
    .bp3-html-select select:disabled.bp3-active,
    .bp3-select select:disabled.bp3-active, .bp3-html-select select:disabled.bp3-active:hover,
    .bp3-select select:disabled.bp3-active:hover, .bp3-html-select select.bp3-disabled.bp3-active,
    .bp3-select select.bp3-disabled.bp3-active, .bp3-html-select select.bp3-disabled.bp3-active:hover,
    .bp3-select select.bp3-disabled.bp3-active:hover{
      background:rgba(206, 217, 224, 0.7); }

.bp3-html-select.bp3-minimal select,
.bp3-select.bp3-minimal select{
  background:none;
  -webkit-box-shadow:none;
          box-shadow:none; }
  .bp3-html-select.bp3-minimal select:hover,
  .bp3-select.bp3-minimal select:hover{
    background:rgba(167, 182, 194, 0.3);
    -webkit-box-shadow:none;
            box-shadow:none;
    color:#182026;
    text-decoration:none; }
  .bp3-html-select.bp3-minimal select:active,
  .bp3-select.bp3-minimal select:active, .bp3-html-select.bp3-minimal select.bp3-active,
  .bp3-select.bp3-minimal select.bp3-active{
    background:rgba(115, 134, 148, 0.3);
    -webkit-box-shadow:none;
            box-shadow:none;
    color:#182026; }
  .bp3-html-select.bp3-minimal select:disabled,
  .bp3-select.bp3-minimal select:disabled, .bp3-html-select.bp3-minimal select:disabled:hover,
  .bp3-select.bp3-minimal select:disabled:hover, .bp3-html-select.bp3-minimal select.bp3-disabled,
  .bp3-select.bp3-minimal select.bp3-disabled, .bp3-html-select.bp3-minimal select.bp3-disabled:hover,
  .bp3-select.bp3-minimal select.bp3-disabled:hover{
    background:none;
    color:rgba(92, 112, 128, 0.6);
    cursor:not-allowed; }
    .bp3-html-select.bp3-minimal select:disabled.bp3-active,
    .bp3-select.bp3-minimal select:disabled.bp3-active, .bp3-html-select.bp3-minimal select:disabled:hover.bp3-active,
    .bp3-select.bp3-minimal select:disabled:hover.bp3-active, .bp3-html-select.bp3-minimal select.bp3-disabled.bp3-active,
    .bp3-select.bp3-minimal select.bp3-disabled.bp3-active, .bp3-html-select.bp3-minimal select.bp3-disabled:hover.bp3-active,
    .bp3-select.bp3-minimal select.bp3-disabled:hover.bp3-active{
      background:rgba(115, 134, 148, 0.3); }
  .bp3-dark .bp3-html-select.bp3-minimal select, .bp3-html-select.bp3-minimal .bp3-dark select,
  .bp3-dark .bp3-select.bp3-minimal select, .bp3-select.bp3-minimal .bp3-dark select{
    background:none;
    -webkit-box-shadow:none;
            box-shadow:none;
    color:inherit; }
    .bp3-dark .bp3-html-select.bp3-minimal select:hover, .bp3-html-select.bp3-minimal .bp3-dark select:hover,
    .bp3-dark .bp3-select.bp3-minimal select:hover, .bp3-select.bp3-minimal .bp3-dark select:hover, .bp3-dark .bp3-html-select.bp3-minimal select:active, .bp3-html-select.bp3-minimal .bp3-dark select:active,
    .bp3-dark .bp3-select.bp3-minimal select:active, .bp3-select.bp3-minimal .bp3-dark select:active, .bp3-dark .bp3-html-select.bp3-minimal select.bp3-active, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-active,
    .bp3-dark .bp3-select.bp3-minimal select.bp3-active, .bp3-select.bp3-minimal .bp3-dark select.bp3-active{
      background:none;
      -webkit-box-shadow:none;
              box-shadow:none; }
    .bp3-dark .bp3-html-select.bp3-minimal select:hover, .bp3-html-select.bp3-minimal .bp3-dark select:hover,
    .bp3-dark .bp3-select.bp3-minimal select:hover, .bp3-select.bp3-minimal .bp3-dark select:hover{
      background:rgba(138, 155, 168, 0.15); }
    .bp3-dark .bp3-html-select.bp3-minimal select:active, .bp3-html-select.bp3-minimal .bp3-dark select:active,
    .bp3-dark .bp3-select.bp3-minimal select:active, .bp3-select.bp3-minimal .bp3-dark select:active, .bp3-dark .bp3-html-select.bp3-minimal select.bp3-active, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-active,
    .bp3-dark .bp3-select.bp3-minimal select.bp3-active, .bp3-select.bp3-minimal .bp3-dark select.bp3-active{
      background:rgba(138, 155, 168, 0.3);
      color:#f5f8fa; }
    .bp3-dark .bp3-html-select.bp3-minimal select:disabled, .bp3-html-select.bp3-minimal .bp3-dark select:disabled,
    .bp3-dark .bp3-select.bp3-minimal select:disabled, .bp3-select.bp3-minimal .bp3-dark select:disabled, .bp3-dark .bp3-html-select.bp3-minimal select:disabled:hover, .bp3-html-select.bp3-minimal .bp3-dark select:disabled:hover,
    .bp3-dark .bp3-select.bp3-minimal select:disabled:hover, .bp3-select.bp3-minimal .bp3-dark select:disabled:hover, .bp3-dark .bp3-html-select.bp3-minimal select.bp3-disabled, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-disabled,
    .bp3-dark .bp3-select.bp3-minimal select.bp3-disabled, .bp3-select.bp3-minimal .bp3-dark select.bp3-disabled, .bp3-dark .bp3-html-select.bp3-minimal select.bp3-disabled:hover, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-disabled:hover,
    .bp3-dark .bp3-select.bp3-minimal select.bp3-disabled:hover, .bp3-select.bp3-minimal .bp3-dark select.bp3-disabled:hover{
      background:none;
      color:rgba(167, 182, 194, 0.6);
      cursor:not-allowed; }
      .bp3-dark .bp3-html-select.bp3-minimal select:disabled.bp3-active, .bp3-html-select.bp3-minimal .bp3-dark select:disabled.bp3-active,
      .bp3-dark .bp3-select.bp3-minimal select:disabled.bp3-active, .bp3-select.bp3-minimal .bp3-dark select:disabled.bp3-active, .bp3-dark .bp3-html-select.bp3-minimal select:disabled:hover.bp3-active, .bp3-html-select.bp3-minimal .bp3-dark select:disabled:hover.bp3-active,
      .bp3-dark .bp3-select.bp3-minimal select:disabled:hover.bp3-active, .bp3-select.bp3-minimal .bp3-dark select:disabled:hover.bp3-active, .bp3-dark .bp3-html-select.bp3-minimal select.bp3-disabled.bp3-active, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-disabled.bp3-active,
      .bp3-dark .bp3-select.bp3-minimal select.bp3-disabled.bp3-active, .bp3-select.bp3-minimal .bp3-dark select.bp3-disabled.bp3-active, .bp3-dark .bp3-html-select.bp3-minimal select.bp3-disabled:hover.bp3-active, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-disabled:hover.bp3-active,
      .bp3-dark .bp3-select.bp3-minimal select.bp3-disabled:hover.bp3-active, .bp3-select.bp3-minimal .bp3-dark select.bp3-disabled:hover.bp3-active{
        background:rgba(138, 155, 168, 0.3); }
  .bp3-html-select.bp3-minimal select.bp3-intent-primary,
  .bp3-select.bp3-minimal select.bp3-intent-primary{
    color:#106ba3; }
    .bp3-html-select.bp3-minimal select.bp3-intent-primary:hover,
    .bp3-select.bp3-minimal select.bp3-intent-primary:hover, .bp3-html-select.bp3-minimal select.bp3-intent-primary:active,
    .bp3-select.bp3-minimal select.bp3-intent-primary:active, .bp3-html-select.bp3-minimal select.bp3-intent-primary.bp3-active,
    .bp3-select.bp3-minimal select.bp3-intent-primary.bp3-active{
      background:none;
      -webkit-box-shadow:none;
              box-shadow:none;
      color:#106ba3; }
    .bp3-html-select.bp3-minimal select.bp3-intent-primary:hover,
    .bp3-select.bp3-minimal select.bp3-intent-primary:hover{
      background:rgba(19, 124, 189, 0.15);
      color:#106ba3; }
    .bp3-html-select.bp3-minimal select.bp3-intent-primary:active,
    .bp3-select.bp3-minimal select.bp3-intent-primary:active, .bp3-html-select.bp3-minimal select.bp3-intent-primary.bp3-active,
    .bp3-select.bp3-minimal select.bp3-intent-primary.bp3-active{
      background:rgba(19, 124, 189, 0.3);
      color:#106ba3; }
    .bp3-html-select.bp3-minimal select.bp3-intent-primary:disabled,
    .bp3-select.bp3-minimal select.bp3-intent-primary:disabled, .bp3-html-select.bp3-minimal select.bp3-intent-primary.bp3-disabled,
    .bp3-select.bp3-minimal select.bp3-intent-primary.bp3-disabled{
      background:none;
      color:rgba(16, 107, 163, 0.5); }
      .bp3-html-select.bp3-minimal select.bp3-intent-primary:disabled.bp3-active,
      .bp3-select.bp3-minimal select.bp3-intent-primary:disabled.bp3-active, .bp3-html-select.bp3-minimal select.bp3-intent-primary.bp3-disabled.bp3-active,
      .bp3-select.bp3-minimal select.bp3-intent-primary.bp3-disabled.bp3-active{
        background:rgba(19, 124, 189, 0.3); }
    .bp3-html-select.bp3-minimal select.bp3-intent-primary .bp3-button-spinner .bp3-spinner-head, .bp3-select.bp3-minimal select.bp3-intent-primary .bp3-button-spinner .bp3-spinner-head{
      stroke:#106ba3; }
    .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-primary, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-primary,
    .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-primary, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-primary{
      color:#48aff0; }
      .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-primary:hover, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-primary:hover,
      .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-primary:hover, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-primary:hover{
        background:rgba(19, 124, 189, 0.2);
        color:#48aff0; }
      .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-primary:active, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-primary:active,
      .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-primary:active, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-primary:active, .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-primary.bp3-active, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-primary.bp3-active,
      .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-primary.bp3-active, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-primary.bp3-active{
        background:rgba(19, 124, 189, 0.3);
        color:#48aff0; }
      .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-primary:disabled, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-primary:disabled,
      .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-primary:disabled, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-primary:disabled, .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-primary.bp3-disabled, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-primary.bp3-disabled,
      .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-primary.bp3-disabled, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-primary.bp3-disabled{
        background:none;
        color:rgba(72, 175, 240, 0.5); }
        .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-primary:disabled.bp3-active, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-primary:disabled.bp3-active,
        .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-primary:disabled.bp3-active, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-primary:disabled.bp3-active, .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-primary.bp3-disabled.bp3-active, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-primary.bp3-disabled.bp3-active,
        .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-primary.bp3-disabled.bp3-active, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-primary.bp3-disabled.bp3-active{
          background:rgba(19, 124, 189, 0.3); }
  .bp3-html-select.bp3-minimal select.bp3-intent-success,
  .bp3-select.bp3-minimal select.bp3-intent-success{
    color:#0d8050; }
    .bp3-html-select.bp3-minimal select.bp3-intent-success:hover,
    .bp3-select.bp3-minimal select.bp3-intent-success:hover, .bp3-html-select.bp3-minimal select.bp3-intent-success:active,
    .bp3-select.bp3-minimal select.bp3-intent-success:active, .bp3-html-select.bp3-minimal select.bp3-intent-success.bp3-active,
    .bp3-select.bp3-minimal select.bp3-intent-success.bp3-active{
      background:none;
      -webkit-box-shadow:none;
              box-shadow:none;
      color:#0d8050; }
    .bp3-html-select.bp3-minimal select.bp3-intent-success:hover,
    .bp3-select.bp3-minimal select.bp3-intent-success:hover{
      background:rgba(15, 153, 96, 0.15);
      color:#0d8050; }
    .bp3-html-select.bp3-minimal select.bp3-intent-success:active,
    .bp3-select.bp3-minimal select.bp3-intent-success:active, .bp3-html-select.bp3-minimal select.bp3-intent-success.bp3-active,
    .bp3-select.bp3-minimal select.bp3-intent-success.bp3-active{
      background:rgba(15, 153, 96, 0.3);
      color:#0d8050; }
    .bp3-html-select.bp3-minimal select.bp3-intent-success:disabled,
    .bp3-select.bp3-minimal select.bp3-intent-success:disabled, .bp3-html-select.bp3-minimal select.bp3-intent-success.bp3-disabled,
    .bp3-select.bp3-minimal select.bp3-intent-success.bp3-disabled{
      background:none;
      color:rgba(13, 128, 80, 0.5); }
      .bp3-html-select.bp3-minimal select.bp3-intent-success:disabled.bp3-active,
      .bp3-select.bp3-minimal select.bp3-intent-success:disabled.bp3-active, .bp3-html-select.bp3-minimal select.bp3-intent-success.bp3-disabled.bp3-active,
      .bp3-select.bp3-minimal select.bp3-intent-success.bp3-disabled.bp3-active{
        background:rgba(15, 153, 96, 0.3); }
    .bp3-html-select.bp3-minimal select.bp3-intent-success .bp3-button-spinner .bp3-spinner-head, .bp3-select.bp3-minimal select.bp3-intent-success .bp3-button-spinner .bp3-spinner-head{
      stroke:#0d8050; }
    .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-success, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-success,
    .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-success, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-success{
      color:#3dcc91; }
      .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-success:hover, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-success:hover,
      .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-success:hover, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-success:hover{
        background:rgba(15, 153, 96, 0.2);
        color:#3dcc91; }
      .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-success:active, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-success:active,
      .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-success:active, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-success:active, .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-success.bp3-active, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-success.bp3-active,
      .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-success.bp3-active, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-success.bp3-active{
        background:rgba(15, 153, 96, 0.3);
        color:#3dcc91; }
      .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-success:disabled, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-success:disabled,
      .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-success:disabled, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-success:disabled, .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-success.bp3-disabled, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-success.bp3-disabled,
      .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-success.bp3-disabled, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-success.bp3-disabled{
        background:none;
        color:rgba(61, 204, 145, 0.5); }
        .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-success:disabled.bp3-active, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-success:disabled.bp3-active,
        .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-success:disabled.bp3-active, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-success:disabled.bp3-active, .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-success.bp3-disabled.bp3-active, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-success.bp3-disabled.bp3-active,
        .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-success.bp3-disabled.bp3-active, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-success.bp3-disabled.bp3-active{
          background:rgba(15, 153, 96, 0.3); }
  .bp3-html-select.bp3-minimal select.bp3-intent-warning,
  .bp3-select.bp3-minimal select.bp3-intent-warning{
    color:#bf7326; }
    .bp3-html-select.bp3-minimal select.bp3-intent-warning:hover,
    .bp3-select.bp3-minimal select.bp3-intent-warning:hover, .bp3-html-select.bp3-minimal select.bp3-intent-warning:active,
    .bp3-select.bp3-minimal select.bp3-intent-warning:active, .bp3-html-select.bp3-minimal select.bp3-intent-warning.bp3-active,
    .bp3-select.bp3-minimal select.bp3-intent-warning.bp3-active{
      background:none;
      -webkit-box-shadow:none;
              box-shadow:none;
      color:#bf7326; }
    .bp3-html-select.bp3-minimal select.bp3-intent-warning:hover,
    .bp3-select.bp3-minimal select.bp3-intent-warning:hover{
      background:rgba(217, 130, 43, 0.15);
      color:#bf7326; }
    .bp3-html-select.bp3-minimal select.bp3-intent-warning:active,
    .bp3-select.bp3-minimal select.bp3-intent-warning:active, .bp3-html-select.bp3-minimal select.bp3-intent-warning.bp3-active,
    .bp3-select.bp3-minimal select.bp3-intent-warning.bp3-active{
      background:rgba(217, 130, 43, 0.3);
      color:#bf7326; }
    .bp3-html-select.bp3-minimal select.bp3-intent-warning:disabled,
    .bp3-select.bp3-minimal select.bp3-intent-warning:disabled, .bp3-html-select.bp3-minimal select.bp3-intent-warning.bp3-disabled,
    .bp3-select.bp3-minimal select.bp3-intent-warning.bp3-disabled{
      background:none;
      color:rgba(191, 115, 38, 0.5); }
      .bp3-html-select.bp3-minimal select.bp3-intent-warning:disabled.bp3-active,
      .bp3-select.bp3-minimal select.bp3-intent-warning:disabled.bp3-active, .bp3-html-select.bp3-minimal select.bp3-intent-warning.bp3-disabled.bp3-active,
      .bp3-select.bp3-minimal select.bp3-intent-warning.bp3-disabled.bp3-active{
        background:rgba(217, 130, 43, 0.3); }
    .bp3-html-select.bp3-minimal select.bp3-intent-warning .bp3-button-spinner .bp3-spinner-head, .bp3-select.bp3-minimal select.bp3-intent-warning .bp3-button-spinner .bp3-spinner-head{
      stroke:#bf7326; }
    .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-warning, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-warning,
    .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-warning, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-warning{
      color:#ffb366; }
      .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-warning:hover, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-warning:hover,
      .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-warning:hover, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-warning:hover{
        background:rgba(217, 130, 43, 0.2);
        color:#ffb366; }
      .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-warning:active, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-warning:active,
      .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-warning:active, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-warning:active, .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-warning.bp3-active, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-warning.bp3-active,
      .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-warning.bp3-active, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-warning.bp3-active{
        background:rgba(217, 130, 43, 0.3);
        color:#ffb366; }
      .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-warning:disabled, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-warning:disabled,
      .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-warning:disabled, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-warning:disabled, .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-warning.bp3-disabled, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-warning.bp3-disabled,
      .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-warning.bp3-disabled, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-warning.bp3-disabled{
        background:none;
        color:rgba(255, 179, 102, 0.5); }
        .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-warning:disabled.bp3-active, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-warning:disabled.bp3-active,
        .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-warning:disabled.bp3-active, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-warning:disabled.bp3-active, .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-warning.bp3-disabled.bp3-active, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-warning.bp3-disabled.bp3-active,
        .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-warning.bp3-disabled.bp3-active, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-warning.bp3-disabled.bp3-active{
          background:rgba(217, 130, 43, 0.3); }
  .bp3-html-select.bp3-minimal select.bp3-intent-danger,
  .bp3-select.bp3-minimal select.bp3-intent-danger{
    color:#c23030; }
    .bp3-html-select.bp3-minimal select.bp3-intent-danger:hover,
    .bp3-select.bp3-minimal select.bp3-intent-danger:hover, .bp3-html-select.bp3-minimal select.bp3-intent-danger:active,
    .bp3-select.bp3-minimal select.bp3-intent-danger:active, .bp3-html-select.bp3-minimal select.bp3-intent-danger.bp3-active,
    .bp3-select.bp3-minimal select.bp3-intent-danger.bp3-active{
      background:none;
      -webkit-box-shadow:none;
              box-shadow:none;
      color:#c23030; }
    .bp3-html-select.bp3-minimal select.bp3-intent-danger:hover,
    .bp3-select.bp3-minimal select.bp3-intent-danger:hover{
      background:rgba(219, 55, 55, 0.15);
      color:#c23030; }
    .bp3-html-select.bp3-minimal select.bp3-intent-danger:active,
    .bp3-select.bp3-minimal select.bp3-intent-danger:active, .bp3-html-select.bp3-minimal select.bp3-intent-danger.bp3-active,
    .bp3-select.bp3-minimal select.bp3-intent-danger.bp3-active{
      background:rgba(219, 55, 55, 0.3);
      color:#c23030; }
    .bp3-html-select.bp3-minimal select.bp3-intent-danger:disabled,
    .bp3-select.bp3-minimal select.bp3-intent-danger:disabled, .bp3-html-select.bp3-minimal select.bp3-intent-danger.bp3-disabled,
    .bp3-select.bp3-minimal select.bp3-intent-danger.bp3-disabled{
      background:none;
      color:rgba(194, 48, 48, 0.5); }
      .bp3-html-select.bp3-minimal select.bp3-intent-danger:disabled.bp3-active,
      .bp3-select.bp3-minimal select.bp3-intent-danger:disabled.bp3-active, .bp3-html-select.bp3-minimal select.bp3-intent-danger.bp3-disabled.bp3-active,
      .bp3-select.bp3-minimal select.bp3-intent-danger.bp3-disabled.bp3-active{
        background:rgba(219, 55, 55, 0.3); }
    .bp3-html-select.bp3-minimal select.bp3-intent-danger .bp3-button-spinner .bp3-spinner-head, .bp3-select.bp3-minimal select.bp3-intent-danger .bp3-button-spinner .bp3-spinner-head{
      stroke:#c23030; }
    .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-danger, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-danger,
    .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-danger, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-danger{
      color:#ff7373; }
      .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-danger:hover, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-danger:hover,
      .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-danger:hover, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-danger:hover{
        background:rgba(219, 55, 55, 0.2);
        color:#ff7373; }
      .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-danger:active, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-danger:active,
      .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-danger:active, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-danger:active, .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-danger.bp3-active, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-danger.bp3-active,
      .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-danger.bp3-active, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-danger.bp3-active{
        background:rgba(219, 55, 55, 0.3);
        color:#ff7373; }
      .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-danger:disabled, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-danger:disabled,
      .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-danger:disabled, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-danger:disabled, .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-danger.bp3-disabled, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-danger.bp3-disabled,
      .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-danger.bp3-disabled, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-danger.bp3-disabled{
        background:none;
        color:rgba(255, 115, 115, 0.5); }
        .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-danger:disabled.bp3-active, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-danger:disabled.bp3-active,
        .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-danger:disabled.bp3-active, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-danger:disabled.bp3-active, .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-danger.bp3-disabled.bp3-active, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-danger.bp3-disabled.bp3-active,
        .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-danger.bp3-disabled.bp3-active, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-danger.bp3-disabled.bp3-active{
          background:rgba(219, 55, 55, 0.3); }

.bp3-html-select.bp3-large select,
.bp3-select.bp3-large select{
  font-size:16px;
  height:40px;
  padding-right:35px; }

.bp3-dark .bp3-html-select select, .bp3-dark .bp3-select select{
  background-color:#394b59;
  background-image:-webkit-gradient(linear, left top, left bottom, from(rgba(255, 255, 255, 0.05)), to(rgba(255, 255, 255, 0)));
  background-image:linear-gradient(to bottom, rgba(255, 255, 255, 0.05), rgba(255, 255, 255, 0));
  -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4);
          box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4);
  color:#f5f8fa; }
  .bp3-dark .bp3-html-select select:hover, .bp3-dark .bp3-select select:hover, .bp3-dark .bp3-html-select select:active, .bp3-dark .bp3-select select:active, .bp3-dark .bp3-html-select select.bp3-active, .bp3-dark .bp3-select select.bp3-active{
    color:#f5f8fa; }
  .bp3-dark .bp3-html-select select:hover, .bp3-dark .bp3-select select:hover{
    background-color:#30404d;
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4); }
  .bp3-dark .bp3-html-select select:active, .bp3-dark .bp3-select select:active, .bp3-dark .bp3-html-select select.bp3-active, .bp3-dark .bp3-select select.bp3-active{
    background-color:#202b33;
    background-image:none;
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.6), inset 0 1px 2px rgba(16, 22, 26, 0.2);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.6), inset 0 1px 2px rgba(16, 22, 26, 0.2); }
  .bp3-dark .bp3-html-select select:disabled, .bp3-dark .bp3-select select:disabled, .bp3-dark .bp3-html-select select.bp3-disabled, .bp3-dark .bp3-select select.bp3-disabled{
    background-color:rgba(57, 75, 89, 0.5);
    background-image:none;
    -webkit-box-shadow:none;
            box-shadow:none;
    color:rgba(167, 182, 194, 0.6); }
    .bp3-dark .bp3-html-select select:disabled.bp3-active, .bp3-dark .bp3-select select:disabled.bp3-active, .bp3-dark .bp3-html-select select.bp3-disabled.bp3-active, .bp3-dark .bp3-select select.bp3-disabled.bp3-active{
      background:rgba(57, 75, 89, 0.7); }
  .bp3-dark .bp3-html-select select .bp3-button-spinner .bp3-spinner-head, .bp3-dark .bp3-select select .bp3-button-spinner .bp3-spinner-head{
    background:rgba(16, 22, 26, 0.5);
    stroke:#8a9ba8; }

.bp3-html-select select:disabled,
.bp3-select select:disabled{
  background-color:rgba(206, 217, 224, 0.5);
  -webkit-box-shadow:none;
          box-shadow:none;
  color:rgba(92, 112, 128, 0.6);
  cursor:not-allowed; }

.bp3-html-select .bp3-icon,
.bp3-select .bp3-icon, .bp3-select::after{
  color:#5c7080;
  pointer-events:none;
  position:absolute;
  right:7px;
  top:7px; }
  .bp3-html-select .bp3-disabled.bp3-icon,
  .bp3-select .bp3-disabled.bp3-icon, .bp3-disabled.bp3-select::after{
    color:rgba(92, 112, 128, 0.6); }
.bp3-html-select,
.bp3-select{
  display:inline-block;
  letter-spacing:normal;
  position:relative;
  vertical-align:middle; }
  .bp3-html-select select::-ms-expand,
  .bp3-select select::-ms-expand{
    display:none; }
  .bp3-html-select .bp3-icon,
  .bp3-select .bp3-icon{
    color:#5c7080; }
    .bp3-html-select .bp3-icon:hover,
    .bp3-select .bp3-icon:hover{
      color:#182026; }
    .bp3-dark .bp3-html-select .bp3-icon, .bp3-dark
    .bp3-select .bp3-icon{
      color:#a7b6c2; }
      .bp3-dark .bp3-html-select .bp3-icon:hover, .bp3-dark
      .bp3-select .bp3-icon:hover{
        color:#f5f8fa; }
  .bp3-html-select.bp3-large::after,
  .bp3-html-select.bp3-large .bp3-icon,
  .bp3-select.bp3-large::after,
  .bp3-select.bp3-large .bp3-icon{
    right:12px;
    top:12px; }
  .bp3-html-select.bp3-fill,
  .bp3-html-select.bp3-fill select,
  .bp3-select.bp3-fill,
  .bp3-select.bp3-fill select{
    width:100%; }
  .bp3-dark .bp3-html-select option, .bp3-dark
  .bp3-select option{
    background-color:#30404d;
    color:#f5f8fa; }
  .bp3-dark .bp3-html-select option:disabled, .bp3-dark
  .bp3-select option:disabled{
    color:rgba(167, 182, 194, 0.6); }
  .bp3-dark .bp3-html-select::after, .bp3-dark
  .bp3-select::after{
    color:#a7b6c2; }

.bp3-select::after{
  font-family:"Icons16", sans-serif;
  font-size:16px;
  font-style:normal;
  font-weight:400;
  line-height:1;
  -moz-osx-font-smoothing:grayscale;
  -webkit-font-smoothing:antialiased;
  content:""; }
.bp3-running-text table, table.bp3-html-table{
  border-spacing:0;
  font-size:14px; }
  .bp3-running-text table th, table.bp3-html-table th,
  .bp3-running-text table td,
  table.bp3-html-table td{
    padding:11px;
    text-align:left;
    vertical-align:top; }
  .bp3-running-text table th, table.bp3-html-table th{
    color:#182026;
    font-weight:600; }
  
  .bp3-running-text table td,
  table.bp3-html-table td{
    color:#182026; }
  .bp3-running-text table tbody tr:first-child th, table.bp3-html-table tbody tr:first-child th,
  .bp3-running-text table tbody tr:first-child td,
  table.bp3-html-table tbody tr:first-child td,
  .bp3-running-text table tfoot tr:first-child th,
  table.bp3-html-table tfoot tr:first-child th,
  .bp3-running-text table tfoot tr:first-child td,
  table.bp3-html-table tfoot tr:first-child td{
    -webkit-box-shadow:inset 0 1px 0 0 rgba(16, 22, 26, 0.15);
            box-shadow:inset 0 1px 0 0 rgba(16, 22, 26, 0.15); }
  .bp3-dark .bp3-running-text table th, .bp3-running-text .bp3-dark table th, .bp3-dark table.bp3-html-table th{
    color:#f5f8fa; }
  .bp3-dark .bp3-running-text table td, .bp3-running-text .bp3-dark table td, .bp3-dark table.bp3-html-table td{
    color:#f5f8fa; }
  .bp3-dark .bp3-running-text table tbody tr:first-child th, .bp3-running-text .bp3-dark table tbody tr:first-child th, .bp3-dark table.bp3-html-table tbody tr:first-child th,
  .bp3-dark .bp3-running-text table tbody tr:first-child td,
  .bp3-running-text .bp3-dark table tbody tr:first-child td,
  .bp3-dark table.bp3-html-table tbody tr:first-child td,
  .bp3-dark .bp3-running-text table tfoot tr:first-child th,
  .bp3-running-text .bp3-dark table tfoot tr:first-child th,
  .bp3-dark table.bp3-html-table tfoot tr:first-child th,
  .bp3-dark .bp3-running-text table tfoot tr:first-child td,
  .bp3-running-text .bp3-dark table tfoot tr:first-child td,
  .bp3-dark table.bp3-html-table tfoot tr:first-child td{
    -webkit-box-shadow:inset 0 1px 0 0 rgba(255, 255, 255, 0.15);
            box-shadow:inset 0 1px 0 0 rgba(255, 255, 255, 0.15); }

table.bp3-html-table.bp3-html-table-condensed th,
table.bp3-html-table.bp3-html-table-condensed td, table.bp3-html-table.bp3-small th,
table.bp3-html-table.bp3-small td{
  padding-bottom:6px;
  padding-top:6px; }

table.bp3-html-table.bp3-html-table-striped tbody tr:nth-child(odd) td{
  background:rgba(191, 204, 214, 0.15); }

table.bp3-html-table.bp3-html-table-bordered th:not(:first-child){
  -webkit-box-shadow:inset 1px 0 0 0 rgba(16, 22, 26, 0.15);
          box-shadow:inset 1px 0 0 0 rgba(16, 22, 26, 0.15); }

table.bp3-html-table.bp3-html-table-bordered tbody tr td,
table.bp3-html-table.bp3-html-table-bordered tfoot tr td{
  -webkit-box-shadow:inset 0 1px 0 0 rgba(16, 22, 26, 0.15);
          box-shadow:inset 0 1px 0 0 rgba(16, 22, 26, 0.15); }
  table.bp3-html-table.bp3-html-table-bordered tbody tr td:not(:first-child),
  table.bp3-html-table.bp3-html-table-bordered tfoot tr td:not(:first-child){
    -webkit-box-shadow:inset 1px 1px 0 0 rgba(16, 22, 26, 0.15);
            box-shadow:inset 1px 1px 0 0 rgba(16, 22, 26, 0.15); }

table.bp3-html-table.bp3-html-table-bordered.bp3-html-table-striped tbody tr:not(:first-child) td{
  -webkit-box-shadow:none;
          box-shadow:none; }
  table.bp3-html-table.bp3-html-table-bordered.bp3-html-table-striped tbody tr:not(:first-child) td:not(:first-child){
    -webkit-box-shadow:inset 1px 0 0 0 rgba(16, 22, 26, 0.15);
            box-shadow:inset 1px 0 0 0 rgba(16, 22, 26, 0.15); }

table.bp3-html-table.bp3-interactive tbody tr:hover td{
  background-color:rgba(191, 204, 214, 0.3);
  cursor:pointer; }

table.bp3-html-table.bp3-interactive tbody tr:active td{
  background-color:rgba(191, 204, 214, 0.4); }

.bp3-dark table.bp3-html-table{ }
  .bp3-dark table.bp3-html-table.bp3-html-table-striped tbody tr:nth-child(odd) td{
    background:rgba(92, 112, 128, 0.15); }
  .bp3-dark table.bp3-html-table.bp3-html-table-bordered th:not(:first-child){
    -webkit-box-shadow:inset 1px 0 0 0 rgba(255, 255, 255, 0.15);
            box-shadow:inset 1px 0 0 0 rgba(255, 255, 255, 0.15); }
  .bp3-dark table.bp3-html-table.bp3-html-table-bordered tbody tr td,
  .bp3-dark table.bp3-html-table.bp3-html-table-bordered tfoot tr td{
    -webkit-box-shadow:inset 0 1px 0 0 rgba(255, 255, 255, 0.15);
            box-shadow:inset 0 1px 0 0 rgba(255, 255, 255, 0.15); }
    .bp3-dark table.bp3-html-table.bp3-html-table-bordered tbody tr td:not(:first-child),
    .bp3-dark table.bp3-html-table.bp3-html-table-bordered tfoot tr td:not(:first-child){
      -webkit-box-shadow:inset 1px 1px 0 0 rgba(255, 255, 255, 0.15);
              box-shadow:inset 1px 1px 0 0 rgba(255, 255, 255, 0.15); }
  .bp3-dark table.bp3-html-table.bp3-html-table-bordered.bp3-html-table-striped tbody tr:not(:first-child) td{
    -webkit-box-shadow:inset 1px 0 0 0 rgba(255, 255, 255, 0.15);
            box-shadow:inset 1px 0 0 0 rgba(255, 255, 255, 0.15); }
    .bp3-dark table.bp3-html-table.bp3-html-table-bordered.bp3-html-table-striped tbody tr:not(:first-child) td:first-child{
      -webkit-box-shadow:none;
              box-shadow:none; }
  .bp3-dark table.bp3-html-table.bp3-interactive tbody tr:hover td{
    background-color:rgba(92, 112, 128, 0.3);
    cursor:pointer; }
  .bp3-dark table.bp3-html-table.bp3-interactive tbody tr:active td{
    background-color:rgba(92, 112, 128, 0.4); }

.bp3-key-combo{
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  -webkit-box-orient:horizontal;
  -webkit-box-direction:normal;
      -ms-flex-direction:row;
          flex-direction:row;
  -webkit-box-align:center;
      -ms-flex-align:center;
          align-items:center; }
  .bp3-key-combo > *{
    -webkit-box-flex:0;
        -ms-flex-positive:0;
            flex-grow:0;
    -ms-flex-negative:0;
        flex-shrink:0; }
  .bp3-key-combo > .bp3-fill{
    -webkit-box-flex:1;
        -ms-flex-positive:1;
            flex-grow:1;
    -ms-flex-negative:1;
        flex-shrink:1; }
  .bp3-key-combo::before,
  .bp3-key-combo > *{
    margin-right:5px; }
  .bp3-key-combo:empty::before,
  .bp3-key-combo > :last-child{
    margin-right:0; }

.bp3-hotkey-dialog{
  padding-bottom:0;
  top:40px; }
  .bp3-hotkey-dialog .bp3-dialog-body{
    margin:0;
    padding:0; }
  .bp3-hotkey-dialog .bp3-hotkey-label{
    -webkit-box-flex:1;
        -ms-flex-positive:1;
            flex-grow:1; }

.bp3-hotkey-column{
  margin:auto;
  max-height:80vh;
  overflow-y:auto;
  padding:30px; }
  .bp3-hotkey-column .bp3-heading{
    margin-bottom:20px; }
    .bp3-hotkey-column .bp3-heading:not(:first-child){
      margin-top:40px; }

.bp3-hotkey{
  -webkit-box-align:center;
      -ms-flex-align:center;
          align-items:center;
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  -webkit-box-pack:justify;
      -ms-flex-pack:justify;
          justify-content:space-between;
  margin-left:0;
  margin-right:0; }
  .bp3-hotkey:not(:last-child){
    margin-bottom:10px; }
.bp3-icon{
  display:inline-block;
  -webkit-box-flex:0;
      -ms-flex:0 0 auto;
          flex:0 0 auto;
  vertical-align:text-bottom; }
  .bp3-icon:not(:empty)::before{
    content:"" !important;
    content:unset !important; }
  .bp3-icon > svg{
    display:block; }
    .bp3-icon > svg:not([fill]){
      fill:currentColor; }

.bp3-icon.bp3-intent-primary, .bp3-icon-standard.bp3-intent-primary, .bp3-icon-large.bp3-intent-primary{
  color:#106ba3; }
  .bp3-dark .bp3-icon.bp3-intent-primary, .bp3-dark .bp3-icon-standard.bp3-intent-primary, .bp3-dark .bp3-icon-large.bp3-intent-primary{
    color:#48aff0; }

.bp3-icon.bp3-intent-success, .bp3-icon-standard.bp3-intent-success, .bp3-icon-large.bp3-intent-success{
  color:#0d8050; }
  .bp3-dark .bp3-icon.bp3-intent-success, .bp3-dark .bp3-icon-standard.bp3-intent-success, .bp3-dark .bp3-icon-large.bp3-intent-success{
    color:#3dcc91; }

.bp3-icon.bp3-intent-warning, .bp3-icon-standard.bp3-intent-warning, .bp3-icon-large.bp3-intent-warning{
  color:#bf7326; }
  .bp3-dark .bp3-icon.bp3-intent-warning, .bp3-dark .bp3-icon-standard.bp3-intent-warning, .bp3-dark .bp3-icon-large.bp3-intent-warning{
    color:#ffb366; }

.bp3-icon.bp3-intent-danger, .bp3-icon-standard.bp3-intent-danger, .bp3-icon-large.bp3-intent-danger{
  color:#c23030; }
  .bp3-dark .bp3-icon.bp3-intent-danger, .bp3-dark .bp3-icon-standard.bp3-intent-danger, .bp3-dark .bp3-icon-large.bp3-intent-danger{
    color:#ff7373; }

span.bp3-icon-standard{
  font-family:"Icons16", sans-serif;
  font-size:16px;
  font-style:normal;
  font-weight:400;
  line-height:1;
  -moz-osx-font-smoothing:grayscale;
  -webkit-font-smoothing:antialiased;
  display:inline-block; }

span.bp3-icon-large{
  font-family:"Icons20", sans-serif;
  font-size:20px;
  font-style:normal;
  font-weight:400;
  line-height:1;
  -moz-osx-font-smoothing:grayscale;
  -webkit-font-smoothing:antialiased;
  display:inline-block; }

span.bp3-icon:empty{
  font-family:"Icons20";
  font-size:inherit;
  font-style:normal;
  font-weight:400;
  line-height:1; }
  span.bp3-icon:empty::before{
    -moz-osx-font-smoothing:grayscale;
    -webkit-font-smoothing:antialiased; }

.bp3-icon-add::before{
  content:""; }

.bp3-icon-add-column-left::before{
  content:""; }

.bp3-icon-add-column-right::before{
  content:""; }

.bp3-icon-add-row-bottom::before{
  content:""; }

.bp3-icon-add-row-top::before{
  content:""; }

.bp3-icon-add-to-artifact::before{
  content:""; }

.bp3-icon-add-to-folder::before{
  content:""; }

.bp3-icon-airplane::before{
  content:""; }

.bp3-icon-align-center::before{
  content:""; }

.bp3-icon-align-justify::before{
  content:""; }

.bp3-icon-align-left::before{
  content:""; }

.bp3-icon-align-right::before{
  content:""; }

.bp3-icon-alignment-bottom::before{
  content:""; }

.bp3-icon-alignment-horizontal-center::before{
  content:""; }

.bp3-icon-alignment-left::before{
  content:""; }

.bp3-icon-alignment-right::before{
  content:""; }

.bp3-icon-alignment-top::before{
  content:""; }

.bp3-icon-alignment-vertical-center::before{
  content:""; }

.bp3-icon-annotation::before{
  content:""; }

.bp3-icon-application::before{
  content:""; }

.bp3-icon-applications::before{
  content:""; }

.bp3-icon-archive::before{
  content:""; }

.bp3-icon-arrow-bottom-left::before{
  content:"↙"; }

.bp3-icon-arrow-bottom-right::before{
  content:"↘"; }

.bp3-icon-arrow-down::before{
  content:"↓"; }

.bp3-icon-arrow-left::before{
  content:"←"; }

.bp3-icon-arrow-right::before{
  content:"→"; }

.bp3-icon-arrow-top-left::before{
  content:"↖"; }

.bp3-icon-arrow-top-right::before{
  content:"↗"; }

.bp3-icon-arrow-up::before{
  content:"↑"; }

.bp3-icon-arrows-horizontal::before{
  content:"↔"; }

.bp3-icon-arrows-vertical::before{
  content:"↕"; }

.bp3-icon-asterisk::before{
  content:"*"; }

.bp3-icon-automatic-updates::before{
  content:""; }

.bp3-icon-badge::before{
  content:""; }

.bp3-icon-ban-circle::before{
  content:""; }

.bp3-icon-bank-account::before{
  content:""; }

.bp3-icon-barcode::before{
  content:""; }

.bp3-icon-blank::before{
  content:""; }

.bp3-icon-blocked-person::before{
  content:""; }

.bp3-icon-bold::before{
  content:""; }

.bp3-icon-book::before{
  content:""; }

.bp3-icon-bookmark::before{
  content:""; }

.bp3-icon-box::before{
  content:""; }

.bp3-icon-briefcase::before{
  content:""; }

.bp3-icon-bring-data::before{
  content:""; }

.bp3-icon-build::before{
  content:""; }

.bp3-icon-calculator::before{
  content:""; }

.bp3-icon-calendar::before{
  content:""; }

.bp3-icon-camera::before{
  content:""; }

.bp3-icon-caret-down::before{
  content:"⌄"; }

.bp3-icon-caret-left::before{
  content:"〈"; }

.bp3-icon-caret-right::before{
  content:"〉"; }

.bp3-icon-caret-up::before{
  content:"⌃"; }

.bp3-icon-cell-tower::before{
  content:""; }

.bp3-icon-changes::before{
  content:""; }

.bp3-icon-chart::before{
  content:""; }

.bp3-icon-chat::before{
  content:""; }

.bp3-icon-chevron-backward::before{
  content:""; }

.bp3-icon-chevron-down::before{
  content:""; }

.bp3-icon-chevron-forward::before{
  content:""; }

.bp3-icon-chevron-left::before{
  content:""; }

.bp3-icon-chevron-right::before{
  content:""; }

.bp3-icon-chevron-up::before{
  content:""; }

.bp3-icon-circle::before{
  content:""; }

.bp3-icon-circle-arrow-down::before{
  content:""; }

.bp3-icon-circle-arrow-left::before{
  content:""; }

.bp3-icon-circle-arrow-right::before{
  content:""; }

.bp3-icon-circle-arrow-up::before{
  content:""; }

.bp3-icon-citation::before{
  content:""; }

.bp3-icon-clean::before{
  content:""; }

.bp3-icon-clipboard::before{
  content:""; }

.bp3-icon-cloud::before{
  content:"☁"; }

.bp3-icon-cloud-download::before{
  content:""; }

.bp3-icon-cloud-upload::before{
  content:""; }

.bp3-icon-code::before{
  content:""; }

.bp3-icon-code-block::before{
  content:""; }

.bp3-icon-cog::before{
  content:""; }

.bp3-icon-collapse-all::before{
  content:""; }

.bp3-icon-column-layout::before{
  content:""; }

.bp3-icon-comment::before{
  content:""; }

.bp3-icon-comparison::before{
  content:""; }

.bp3-icon-compass::before{
  content:""; }

.bp3-icon-compressed::before{
  content:""; }

.bp3-icon-confirm::before{
  content:""; }

.bp3-icon-console::before{
  content:""; }

.bp3-icon-contrast::before{
  content:""; }

.bp3-icon-control::before{
  content:""; }

.bp3-icon-credit-card::before{
  content:""; }

.bp3-icon-cross::before{
  content:"✗"; }

.bp3-icon-crown::before{
  content:""; }

.bp3-icon-cube::before{
  content:""; }

.bp3-icon-cube-add::before{
  content:""; }

.bp3-icon-cube-remove::before{
  content:""; }

.bp3-icon-curved-range-chart::before{
  content:""; }

.bp3-icon-cut::before{
  content:""; }

.bp3-icon-dashboard::before{
  content:""; }

.bp3-icon-data-lineage::before{
  content:""; }

.bp3-icon-database::before{
  content:""; }

.bp3-icon-delete::before{
  content:""; }

.bp3-icon-delta::before{
  content:"Δ"; }

.bp3-icon-derive-column::before{
  content:""; }

.bp3-icon-desktop::before{
  content:""; }

.bp3-icon-diagnosis::before{
  content:""; }

.bp3-icon-diagram-tree::before{
  content:""; }

.bp3-icon-direction-left::before{
  content:""; }

.bp3-icon-direction-right::before{
  content:""; }

.bp3-icon-disable::before{
  content:""; }

.bp3-icon-document::before{
  content:""; }

.bp3-icon-document-open::before{
  content:""; }

.bp3-icon-document-share::before{
  content:""; }

.bp3-icon-dollar::before{
  content:"$"; }

.bp3-icon-dot::before{
  content:"•"; }

.bp3-icon-double-caret-horizontal::before{
  content:""; }

.bp3-icon-double-caret-vertical::before{
  content:""; }

.bp3-icon-double-chevron-down::before{
  content:""; }

.bp3-icon-double-chevron-left::before{
  content:""; }

.bp3-icon-double-chevron-right::before{
  content:""; }

.bp3-icon-double-chevron-up::before{
  content:""; }

.bp3-icon-doughnut-chart::before{
  content:""; }

.bp3-icon-download::before{
  content:""; }

.bp3-icon-drag-handle-horizontal::before{
  content:""; }

.bp3-icon-drag-handle-vertical::before{
  content:""; }

.bp3-icon-draw::before{
  content:""; }

.bp3-icon-drive-time::before{
  content:""; }

.bp3-icon-duplicate::before{
  content:""; }

.bp3-icon-edit::before{
  content:"✎"; }

.bp3-icon-eject::before{
  content:"⏏"; }

.bp3-icon-endorsed::before{
  content:""; }

.bp3-icon-envelope::before{
  content:"✉"; }

.bp3-icon-equals::before{
  content:""; }

.bp3-icon-eraser::before{
  content:""; }

.bp3-icon-error::before{
  content:""; }

.bp3-icon-euro::before{
  content:"€"; }

.bp3-icon-exchange::before{
  content:""; }

.bp3-icon-exclude-row::before{
  content:""; }

.bp3-icon-expand-all::before{
  content:""; }

.bp3-icon-export::before{
  content:""; }

.bp3-icon-eye-off::before{
  content:""; }

.bp3-icon-eye-on::before{
  content:""; }

.bp3-icon-eye-open::before{
  content:""; }

.bp3-icon-fast-backward::before{
  content:""; }

.bp3-icon-fast-forward::before{
  content:""; }

.bp3-icon-feed::before{
  content:""; }

.bp3-icon-feed-subscribed::before{
  content:""; }

.bp3-icon-film::before{
  content:""; }

.bp3-icon-filter::before{
  content:""; }

.bp3-icon-filter-keep::before{
  content:""; }

.bp3-icon-filter-list::before{
  content:""; }

.bp3-icon-filter-open::before{
  content:""; }

.bp3-icon-filter-remove::before{
  content:""; }

.bp3-icon-flag::before{
  content:"⚑"; }

.bp3-icon-flame::before{
  content:""; }

.bp3-icon-flash::before{
  content:""; }

.bp3-icon-floppy-disk::before{
  content:""; }

.bp3-icon-flow-branch::before{
  content:""; }

.bp3-icon-flow-end::before{
  content:""; }

.bp3-icon-flow-linear::before{
  content:""; }

.bp3-icon-flow-review::before{
  content:""; }

.bp3-icon-flow-review-branch::before{
  content:""; }

.bp3-icon-flows::before{
  content:""; }

.bp3-icon-folder-close::before{
  content:""; }

.bp3-icon-folder-new::before{
  content:""; }

.bp3-icon-folder-open::before{
  content:""; }

.bp3-icon-folder-shared::before{
  content:""; }

.bp3-icon-folder-shared-open::before{
  content:""; }

.bp3-icon-follower::before{
  content:""; }

.bp3-icon-following::before{
  content:""; }

.bp3-icon-font::before{
  content:""; }

.bp3-icon-fork::before{
  content:""; }

.bp3-icon-form::before{
  content:""; }

.bp3-icon-full-circle::before{
  content:""; }

.bp3-icon-full-stacked-chart::before{
  content:""; }

.bp3-icon-fullscreen::before{
  content:""; }

.bp3-icon-function::before{
  content:""; }

.bp3-icon-gantt-chart::before{
  content:""; }

.bp3-icon-geolocation::before{
  content:""; }

.bp3-icon-geosearch::before{
  content:""; }

.bp3-icon-git-branch::before{
  content:""; }

.bp3-icon-git-commit::before{
  content:""; }

.bp3-icon-git-merge::before{
  content:""; }

.bp3-icon-git-new-branch::before{
  content:""; }

.bp3-icon-git-pull::before{
  content:""; }

.bp3-icon-git-push::before{
  content:""; }

.bp3-icon-git-repo::before{
  content:""; }

.bp3-icon-glass::before{
  content:""; }

.bp3-icon-globe::before{
  content:""; }

.bp3-icon-globe-network::before{
  content:""; }

.bp3-icon-graph::before{
  content:""; }

.bp3-icon-graph-remove::before{
  content:""; }

.bp3-icon-greater-than::before{
  content:""; }

.bp3-icon-greater-than-or-equal-to::before{
  content:""; }

.bp3-icon-grid::before{
  content:""; }

.bp3-icon-grid-view::before{
  content:""; }

.bp3-icon-group-objects::before{
  content:""; }

.bp3-icon-grouped-bar-chart::before{
  content:""; }

.bp3-icon-hand::before{
  content:""; }

.bp3-icon-hand-down::before{
  content:""; }

.bp3-icon-hand-left::before{
  content:""; }

.bp3-icon-hand-right::before{
  content:""; }

.bp3-icon-hand-up::before{
  content:""; }

.bp3-icon-header::before{
  content:""; }

.bp3-icon-header-one::before{
  content:""; }

.bp3-icon-header-two::before{
  content:""; }

.bp3-icon-headset::before{
  content:""; }

.bp3-icon-heart::before{
  content:"♥"; }

.bp3-icon-heart-broken::before{
  content:""; }

.bp3-icon-heat-grid::before{
  content:""; }

.bp3-icon-heatmap::before{
  content:""; }

.bp3-icon-help::before{
  content:"?"; }

.bp3-icon-helper-management::before{
  content:""; }

.bp3-icon-highlight::before{
  content:""; }

.bp3-icon-history::before{
  content:""; }

.bp3-icon-home::before{
  content:"⌂"; }

.bp3-icon-horizontal-bar-chart::before{
  content:""; }

.bp3-icon-horizontal-bar-chart-asc::before{
  content:""; }

.bp3-icon-horizontal-bar-chart-desc::before{
  content:""; }

.bp3-icon-horizontal-distribution::before{
  content:""; }

.bp3-icon-id-number::before{
  content:""; }

.bp3-icon-image-rotate-left::before{
  content:""; }

.bp3-icon-image-rotate-right::before{
  content:""; }

.bp3-icon-import::before{
  content:""; }

.bp3-icon-inbox::before{
  content:""; }

.bp3-icon-inbox-filtered::before{
  content:""; }

.bp3-icon-inbox-geo::before{
  content:""; }

.bp3-icon-inbox-search::before{
  content:""; }

.bp3-icon-inbox-update::before{
  content:""; }

.bp3-icon-info-sign::before{
  content:"ℹ"; }

.bp3-icon-inheritance::before{
  content:""; }

.bp3-icon-inner-join::before{
  content:""; }

.bp3-icon-insert::before{
  content:""; }

.bp3-icon-intersection::before{
  content:""; }

.bp3-icon-ip-address::before{
  content:""; }

.bp3-icon-issue::before{
  content:""; }

.bp3-icon-issue-closed::before{
  content:""; }

.bp3-icon-issue-new::before{
  content:""; }

.bp3-icon-italic::before{
  content:""; }

.bp3-icon-join-table::before{
  content:""; }

.bp3-icon-key::before{
  content:""; }

.bp3-icon-key-backspace::before{
  content:""; }

.bp3-icon-key-command::before{
  content:""; }

.bp3-icon-key-control::before{
  content:""; }

.bp3-icon-key-delete::before{
  content:""; }

.bp3-icon-key-enter::before{
  content:""; }

.bp3-icon-key-escape::before{
  content:""; }

.bp3-icon-key-option::before{
  content:""; }

.bp3-icon-key-shift::before{
  content:""; }

.bp3-icon-key-tab::before{
  content:""; }

.bp3-icon-known-vehicle::before{
  content:""; }

.bp3-icon-lab-test::before{
  content:""; }

.bp3-icon-label::before{
  content:""; }

.bp3-icon-layer::before{
  content:""; }

.bp3-icon-layers::before{
  content:""; }

.bp3-icon-layout::before{
  content:""; }

.bp3-icon-layout-auto::before{
  content:""; }

.bp3-icon-layout-balloon::before{
  content:""; }

.bp3-icon-layout-circle::before{
  content:""; }

.bp3-icon-layout-grid::before{
  content:""; }

.bp3-icon-layout-group-by::before{
  content:""; }

.bp3-icon-layout-hierarchy::before{
  content:""; }

.bp3-icon-layout-linear::before{
  content:""; }

.bp3-icon-layout-skew-grid::before{
  content:""; }

.bp3-icon-layout-sorted-clusters::before{
  content:""; }

.bp3-icon-learning::before{
  content:""; }

.bp3-icon-left-join::before{
  content:""; }

.bp3-icon-less-than::before{
  content:""; }

.bp3-icon-less-than-or-equal-to::before{
  content:""; }

.bp3-icon-lifesaver::before{
  content:""; }

.bp3-icon-lightbulb::before{
  content:""; }

.bp3-icon-link::before{
  content:""; }

.bp3-icon-list::before{
  content:"☰"; }

.bp3-icon-list-columns::before{
  content:""; }

.bp3-icon-list-detail-view::before{
  content:""; }

.bp3-icon-locate::before{
  content:""; }

.bp3-icon-lock::before{
  content:""; }

.bp3-icon-log-in::before{
  content:""; }

.bp3-icon-log-out::before{
  content:""; }

.bp3-icon-manual::before{
  content:""; }

.bp3-icon-manually-entered-data::before{
  content:""; }

.bp3-icon-map::before{
  content:""; }

.bp3-icon-map-create::before{
  content:""; }

.bp3-icon-map-marker::before{
  content:""; }

.bp3-icon-maximize::before{
  content:""; }

.bp3-icon-media::before{
  content:""; }

.bp3-icon-menu::before{
  content:""; }

.bp3-icon-menu-closed::before{
  content:""; }

.bp3-icon-menu-open::before{
  content:""; }

.bp3-icon-merge-columns::before{
  content:""; }

.bp3-icon-merge-links::before{
  content:""; }

.bp3-icon-minimize::before{
  content:""; }

.bp3-icon-minus::before{
  content:"−"; }

.bp3-icon-mobile-phone::before{
  content:""; }

.bp3-icon-mobile-video::before{
  content:""; }

.bp3-icon-moon::before{
  content:""; }

.bp3-icon-more::before{
  content:""; }

.bp3-icon-mountain::before{
  content:""; }

.bp3-icon-move::before{
  content:""; }

.bp3-icon-mugshot::before{
  content:""; }

.bp3-icon-multi-select::before{
  content:""; }

.bp3-icon-music::before{
  content:""; }

.bp3-icon-new-drawing::before{
  content:""; }

.bp3-icon-new-grid-item::before{
  content:""; }

.bp3-icon-new-layer::before{
  content:""; }

.bp3-icon-new-layers::before{
  content:""; }

.bp3-icon-new-link::before{
  content:""; }

.bp3-icon-new-object::before{
  content:""; }

.bp3-icon-new-person::before{
  content:""; }

.bp3-icon-new-prescription::before{
  content:""; }

.bp3-icon-new-text-box::before{
  content:""; }

.bp3-icon-ninja::before{
  content:""; }

.bp3-icon-not-equal-to::before{
  content:""; }

.bp3-icon-notifications::before{
  content:""; }

.bp3-icon-notifications-updated::before{
  content:""; }

.bp3-icon-numbered-list::before{
  content:""; }

.bp3-icon-numerical::before{
  content:""; }

.bp3-icon-office::before{
  content:""; }

.bp3-icon-offline::before{
  content:""; }

.bp3-icon-oil-field::before{
  content:""; }

.bp3-icon-one-column::before{
  content:""; }

.bp3-icon-outdated::before{
  content:""; }

.bp3-icon-page-layout::before{
  content:""; }

.bp3-icon-panel-stats::before{
  content:""; }

.bp3-icon-panel-table::before{
  content:""; }

.bp3-icon-paperclip::before{
  content:""; }

.bp3-icon-paragraph::before{
  content:""; }

.bp3-icon-path::before{
  content:""; }

.bp3-icon-path-search::before{
  content:""; }

.bp3-icon-pause::before{
  content:""; }

.bp3-icon-people::before{
  content:""; }

.bp3-icon-percentage::before{
  content:""; }

.bp3-icon-person::before{
  content:""; }

.bp3-icon-phone::before{
  content:"☎"; }

.bp3-icon-pie-chart::before{
  content:""; }

.bp3-icon-pin::before{
  content:""; }

.bp3-icon-pivot::before{
  content:""; }

.bp3-icon-pivot-table::before{
  content:""; }

.bp3-icon-play::before{
  content:""; }

.bp3-icon-plus::before{
  content:"+"; }

.bp3-icon-polygon-filter::before{
  content:""; }

.bp3-icon-power::before{
  content:""; }

.bp3-icon-predictive-analysis::before{
  content:""; }

.bp3-icon-prescription::before{
  content:""; }

.bp3-icon-presentation::before{
  content:""; }

.bp3-icon-print::before{
  content:"⎙"; }

.bp3-icon-projects::before{
  content:""; }

.bp3-icon-properties::before{
  content:""; }

.bp3-icon-property::before{
  content:""; }

.bp3-icon-publish-function::before{
  content:""; }

.bp3-icon-pulse::before{
  content:""; }

.bp3-icon-random::before{
  content:""; }

.bp3-icon-record::before{
  content:""; }

.bp3-icon-redo::before{
  content:""; }

.bp3-icon-refresh::before{
  content:""; }

.bp3-icon-regression-chart::before{
  content:""; }

.bp3-icon-remove::before{
  content:""; }

.bp3-icon-remove-column::before{
  content:""; }

.bp3-icon-remove-column-left::before{
  content:""; }

.bp3-icon-remove-column-right::before{
  content:""; }

.bp3-icon-remove-row-bottom::before{
  content:""; }

.bp3-icon-remove-row-top::before{
  content:""; }

.bp3-icon-repeat::before{
  content:""; }

.bp3-icon-reset::before{
  content:""; }

.bp3-icon-resolve::before{
  content:""; }

.bp3-icon-rig::before{
  content:""; }

.bp3-icon-right-join::before{
  content:""; }

.bp3-icon-ring::before{
  content:""; }

.bp3-icon-rotate-document::before{
  content:""; }

.bp3-icon-rotate-page::before{
  content:""; }

.bp3-icon-satellite::before{
  content:""; }

.bp3-icon-saved::before{
  content:""; }

.bp3-icon-scatter-plot::before{
  content:""; }

.bp3-icon-search::before{
  content:""; }

.bp3-icon-search-around::before{
  content:""; }

.bp3-icon-search-template::before{
  content:""; }

.bp3-icon-search-text::before{
  content:""; }

.bp3-icon-segmented-control::before{
  content:""; }

.bp3-icon-select::before{
  content:""; }

.bp3-icon-selection::before{
  content:"⦿"; }

.bp3-icon-send-to::before{
  content:""; }

.bp3-icon-send-to-graph::before{
  content:""; }

.bp3-icon-send-to-map::before{
  content:""; }

.bp3-icon-series-add::before{
  content:""; }

.bp3-icon-series-configuration::before{
  content:""; }

.bp3-icon-series-derived::before{
  content:""; }

.bp3-icon-series-filtered::before{
  content:""; }

.bp3-icon-series-search::before{
  content:""; }

.bp3-icon-settings::before{
  content:""; }

.bp3-icon-share::before{
  content:""; }

.bp3-icon-shield::before{
  content:""; }

.bp3-icon-shop::before{
  content:""; }

.bp3-icon-shopping-cart::before{
  content:""; }

.bp3-icon-signal-search::before{
  content:""; }

.bp3-icon-sim-card::before{
  content:""; }

.bp3-icon-slash::before{
  content:""; }

.bp3-icon-small-cross::before{
  content:""; }

.bp3-icon-small-minus::before{
  content:""; }

.bp3-icon-small-plus::before{
  content:""; }

.bp3-icon-small-tick::before{
  content:""; }

.bp3-icon-snowflake::before{
  content:""; }

.bp3-icon-social-media::before{
  content:""; }

.bp3-icon-sort::before{
  content:""; }

.bp3-icon-sort-alphabetical::before{
  content:""; }

.bp3-icon-sort-alphabetical-desc::before{
  content:""; }

.bp3-icon-sort-asc::before{
  content:""; }

.bp3-icon-sort-desc::before{
  content:""; }

.bp3-icon-sort-numerical::before{
  content:""; }

.bp3-icon-sort-numerical-desc::before{
  content:""; }

.bp3-icon-split-columns::before{
  content:""; }

.bp3-icon-square::before{
  content:""; }

.bp3-icon-stacked-chart::before{
  content:""; }

.bp3-icon-star::before{
  content:"★"; }

.bp3-icon-star-empty::before{
  content:"☆"; }

.bp3-icon-step-backward::before{
  content:""; }

.bp3-icon-step-chart::before{
  content:""; }

.bp3-icon-step-forward::before{
  content:""; }

.bp3-icon-stop::before{
  content:""; }

.bp3-icon-stopwatch::before{
  content:""; }

.bp3-icon-strikethrough::before{
  content:""; }

.bp3-icon-style::before{
  content:""; }

.bp3-icon-swap-horizontal::before{
  content:""; }

.bp3-icon-swap-vertical::before{
  content:""; }

.bp3-icon-symbol-circle::before{
  content:""; }

.bp3-icon-symbol-cross::before{
  content:""; }

.bp3-icon-symbol-diamond::before{
  content:""; }

.bp3-icon-symbol-square::before{
  content:""; }

.bp3-icon-symbol-triangle-down::before{
  content:""; }

.bp3-icon-symbol-triangle-up::before{
  content:""; }

.bp3-icon-tag::before{
  content:""; }

.bp3-icon-take-action::before{
  content:""; }

.bp3-icon-taxi::before{
  content:""; }

.bp3-icon-text-highlight::before{
  content:""; }

.bp3-icon-th::before{
  content:""; }

.bp3-icon-th-derived::before{
  content:""; }

.bp3-icon-th-disconnect::before{
  content:""; }

.bp3-icon-th-filtered::before{
  content:""; }

.bp3-icon-th-list::before{
  content:""; }

.bp3-icon-thumbs-down::before{
  content:""; }

.bp3-icon-thumbs-up::before{
  content:""; }

.bp3-icon-tick::before{
  content:"✓"; }

.bp3-icon-tick-circle::before{
  content:""; }

.bp3-icon-time::before{
  content:"⏲"; }

.bp3-icon-timeline-area-chart::before{
  content:""; }

.bp3-icon-timeline-bar-chart::before{
  content:""; }

.bp3-icon-timeline-events::before{
  content:""; }

.bp3-icon-timeline-line-chart::before{
  content:""; }

.bp3-icon-tint::before{
  content:""; }

.bp3-icon-torch::before{
  content:""; }

.bp3-icon-tractor::before{
  content:""; }

.bp3-icon-train::before{
  content:""; }

.bp3-icon-translate::before{
  content:""; }

.bp3-icon-trash::before{
  content:""; }

.bp3-icon-tree::before{
  content:""; }

.bp3-icon-trending-down::before{
  content:""; }

.bp3-icon-trending-up::before{
  content:""; }

.bp3-icon-truck::before{
  content:""; }

.bp3-icon-two-columns::before{
  content:""; }

.bp3-icon-unarchive::before{
  content:""; }

.bp3-icon-underline::before{
  content:"⎁"; }

.bp3-icon-undo::before{
  content:"⎌"; }

.bp3-icon-ungroup-objects::before{
  content:""; }

.bp3-icon-unknown-vehicle::before{
  content:""; }

.bp3-icon-unlock::before{
  content:""; }

.bp3-icon-unpin::before{
  content:""; }

.bp3-icon-unresolve::before{
  content:""; }

.bp3-icon-updated::before{
  content:""; }

.bp3-icon-upload::before{
  content:""; }

.bp3-icon-user::before{
  content:""; }

.bp3-icon-variable::before{
  content:""; }

.bp3-icon-vertical-bar-chart-asc::before{
  content:""; }

.bp3-icon-vertical-bar-chart-desc::before{
  content:""; }

.bp3-icon-vertical-distribution::before{
  content:""; }

.bp3-icon-video::before{
  content:""; }

.bp3-icon-volume-down::before{
  content:""; }

.bp3-icon-volume-off::before{
  content:""; }

.bp3-icon-volume-up::before{
  content:""; }

.bp3-icon-walk::before{
  content:""; }

.bp3-icon-warning-sign::before{
  content:""; }

.bp3-icon-waterfall-chart::before{
  content:""; }

.bp3-icon-widget::before{
  content:""; }

.bp3-icon-widget-button::before{
  content:""; }

.bp3-icon-widget-footer::before{
  content:""; }

.bp3-icon-widget-header::before{
  content:""; }

.bp3-icon-wrench::before{
  content:""; }

.bp3-icon-zoom-in::before{
  content:""; }

.bp3-icon-zoom-out::before{
  content:""; }

.bp3-icon-zoom-to-fit::before{
  content:""; }
.bp3-submenu > .bp3-popover-wrapper{
  display:block; }

.bp3-submenu .bp3-popover-target{
  display:block; }
  .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-menu-item{ }

.bp3-submenu.bp3-popover{
  -webkit-box-shadow:none;
          box-shadow:none;
  padding:0 5px; }
  .bp3-submenu.bp3-popover > .bp3-popover-content{
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 2px 4px rgba(16, 22, 26, 0.2), 0 8px 24px rgba(16, 22, 26, 0.2);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 2px 4px rgba(16, 22, 26, 0.2), 0 8px 24px rgba(16, 22, 26, 0.2); }
  .bp3-dark .bp3-submenu.bp3-popover, .bp3-submenu.bp3-popover.bp3-dark{
    -webkit-box-shadow:none;
            box-shadow:none; }
    .bp3-dark .bp3-submenu.bp3-popover > .bp3-popover-content, .bp3-submenu.bp3-popover.bp3-dark > .bp3-popover-content{
      -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 2px 4px rgba(16, 22, 26, 0.4), 0 8px 24px rgba(16, 22, 26, 0.4);
              box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 2px 4px rgba(16, 22, 26, 0.4), 0 8px 24px rgba(16, 22, 26, 0.4); }
.bp3-menu{
  background:#ffffff;
  border-radius:3px;
  color:#182026;
  list-style:none;
  margin:0;
  min-width:180px;
  padding:5px;
  text-align:left; }

.bp3-menu-divider{
  border-top:1px solid rgba(16, 22, 26, 0.15);
  display:block;
  margin:5px; }
  .bp3-dark .bp3-menu-divider{
    border-top-color:rgba(255, 255, 255, 0.15); }

.bp3-menu-item{
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  -webkit-box-orient:horizontal;
  -webkit-box-direction:normal;
      -ms-flex-direction:row;
          flex-direction:row;
  -webkit-box-align:start;
      -ms-flex-align:start;
          align-items:flex-start;
  border-radius:2px;
  color:inherit;
  line-height:20px;
  padding:5px 7px;
  text-decoration:none;
  -webkit-user-select:none;
     -moz-user-select:none;
      -ms-user-select:none;
          user-select:none; }
  .bp3-menu-item > *{
    -webkit-box-flex:0;
        -ms-flex-positive:0;
            flex-grow:0;
    -ms-flex-negative:0;
        flex-shrink:0; }
  .bp3-menu-item > .bp3-fill{
    -webkit-box-flex:1;
        -ms-flex-positive:1;
            flex-grow:1;
    -ms-flex-negative:1;
        flex-shrink:1; }
  .bp3-menu-item::before,
  .bp3-menu-item > *{
    margin-right:7px; }
  .bp3-menu-item:empty::before,
  .bp3-menu-item > :last-child{
    margin-right:0; }
  .bp3-menu-item > .bp3-fill{
    word-break:break-word; }
  .bp3-menu-item:hover, .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-menu-item{
    background-color:rgba(167, 182, 194, 0.3);
    cursor:pointer;
    text-decoration:none; }
  .bp3-menu-item.bp3-disabled{
    background-color:inherit;
    color:rgba(92, 112, 128, 0.6);
    cursor:not-allowed; }
  .bp3-dark .bp3-menu-item{
    color:inherit; }
    .bp3-dark .bp3-menu-item:hover, .bp3-dark .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-menu-item, .bp3-submenu .bp3-dark .bp3-popover-target.bp3-popover-open > .bp3-menu-item{
      background-color:rgba(138, 155, 168, 0.15);
      color:inherit; }
    .bp3-dark .bp3-menu-item.bp3-disabled{
      background-color:inherit;
      color:rgba(167, 182, 194, 0.6); }
  .bp3-menu-item.bp3-intent-primary{
    color:#106ba3; }
    .bp3-menu-item.bp3-intent-primary .bp3-icon{
      color:inherit; }
    .bp3-menu-item.bp3-intent-primary::before, .bp3-menu-item.bp3-intent-primary::after,
    .bp3-menu-item.bp3-intent-primary .bp3-menu-item-label{
      color:#106ba3; }
    .bp3-menu-item.bp3-intent-primary:hover, .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-primary.bp3-menu-item, .bp3-menu-item.bp3-intent-primary.bp3-active{
      background-color:#137cbd; }
    .bp3-menu-item.bp3-intent-primary:active{
      background-color:#106ba3; }
    .bp3-menu-item.bp3-intent-primary:hover, .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-primary.bp3-menu-item, .bp3-menu-item.bp3-intent-primary:hover::before, .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-primary.bp3-menu-item::before, .bp3-menu-item.bp3-intent-primary:hover::after, .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-primary.bp3-menu-item::after,
    .bp3-menu-item.bp3-intent-primary:hover .bp3-menu-item-label,
    .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-primary.bp3-menu-item .bp3-menu-item-label, .bp3-menu-item.bp3-intent-primary:active, .bp3-menu-item.bp3-intent-primary:active::before, .bp3-menu-item.bp3-intent-primary:active::after,
    .bp3-menu-item.bp3-intent-primary:active .bp3-menu-item-label, .bp3-menu-item.bp3-intent-primary.bp3-active, .bp3-menu-item.bp3-intent-primary.bp3-active::before, .bp3-menu-item.bp3-intent-primary.bp3-active::after,
    .bp3-menu-item.bp3-intent-primary.bp3-active .bp3-menu-item-label{
      color:#ffffff; }
  .bp3-menu-item.bp3-intent-success{
    color:#0d8050; }
    .bp3-menu-item.bp3-intent-success .bp3-icon{
      color:inherit; }
    .bp3-menu-item.bp3-intent-success::before, .bp3-menu-item.bp3-intent-success::after,
    .bp3-menu-item.bp3-intent-success .bp3-menu-item-label{
      color:#0d8050; }
    .bp3-menu-item.bp3-intent-success:hover, .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-success.bp3-menu-item, .bp3-menu-item.bp3-intent-success.bp3-active{
      background-color:#0f9960; }
    .bp3-menu-item.bp3-intent-success:active{
      background-color:#0d8050; }
    .bp3-menu-item.bp3-intent-success:hover, .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-success.bp3-menu-item, .bp3-menu-item.bp3-intent-success:hover::before, .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-success.bp3-menu-item::before, .bp3-menu-item.bp3-intent-success:hover::after, .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-success.bp3-menu-item::after,
    .bp3-menu-item.bp3-intent-success:hover .bp3-menu-item-label,
    .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-success.bp3-menu-item .bp3-menu-item-label, .bp3-menu-item.bp3-intent-success:active, .bp3-menu-item.bp3-intent-success:active::before, .bp3-menu-item.bp3-intent-success:active::after,
    .bp3-menu-item.bp3-intent-success:active .bp3-menu-item-label, .bp3-menu-item.bp3-intent-success.bp3-active, .bp3-menu-item.bp3-intent-success.bp3-active::before, .bp3-menu-item.bp3-intent-success.bp3-active::after,
    .bp3-menu-item.bp3-intent-success.bp3-active .bp3-menu-item-label{
      color:#ffffff; }
  .bp3-menu-item.bp3-intent-warning{
    color:#bf7326; }
    .bp3-menu-item.bp3-intent-warning .bp3-icon{
      color:inherit; }
    .bp3-menu-item.bp3-intent-warning::before, .bp3-menu-item.bp3-intent-warning::after,
    .bp3-menu-item.bp3-intent-warning .bp3-menu-item-label{
      color:#bf7326; }
    .bp3-menu-item.bp3-intent-warning:hover, .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-warning.bp3-menu-item, .bp3-menu-item.bp3-intent-warning.bp3-active{
      background-color:#d9822b; }
    .bp3-menu-item.bp3-intent-warning:active{
      background-color:#bf7326; }
    .bp3-menu-item.bp3-intent-warning:hover, .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-warning.bp3-menu-item, .bp3-menu-item.bp3-intent-warning:hover::before, .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-warning.bp3-menu-item::before, .bp3-menu-item.bp3-intent-warning:hover::after, .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-warning.bp3-menu-item::after,
    .bp3-menu-item.bp3-intent-warning:hover .bp3-menu-item-label,
    .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-warning.bp3-menu-item .bp3-menu-item-label, .bp3-menu-item.bp3-intent-warning:active, .bp3-menu-item.bp3-intent-warning:active::before, .bp3-menu-item.bp3-intent-warning:active::after,
    .bp3-menu-item.bp3-intent-warning:active .bp3-menu-item-label, .bp3-menu-item.bp3-intent-warning.bp3-active, .bp3-menu-item.bp3-intent-warning.bp3-active::before, .bp3-menu-item.bp3-intent-warning.bp3-active::after,
    .bp3-menu-item.bp3-intent-warning.bp3-active .bp3-menu-item-label{
      color:#ffffff; }
  .bp3-menu-item.bp3-intent-danger{
    color:#c23030; }
    .bp3-menu-item.bp3-intent-danger .bp3-icon{
      color:inherit; }
    .bp3-menu-item.bp3-intent-danger::before, .bp3-menu-item.bp3-intent-danger::after,
    .bp3-menu-item.bp3-intent-danger .bp3-menu-item-label{
      color:#c23030; }
    .bp3-menu-item.bp3-intent-danger:hover, .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-danger.bp3-menu-item, .bp3-menu-item.bp3-intent-danger.bp3-active{
      background-color:#db3737; }
    .bp3-menu-item.bp3-intent-danger:active{
      background-color:#c23030; }
    .bp3-menu-item.bp3-intent-danger:hover, .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-danger.bp3-menu-item, .bp3-menu-item.bp3-intent-danger:hover::before, .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-danger.bp3-menu-item::before, .bp3-menu-item.bp3-intent-danger:hover::after, .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-danger.bp3-menu-item::after,
    .bp3-menu-item.bp3-intent-danger:hover .bp3-menu-item-label,
    .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-danger.bp3-menu-item .bp3-menu-item-label, .bp3-menu-item.bp3-intent-danger:active, .bp3-menu-item.bp3-intent-danger:active::before, .bp3-menu-item.bp3-intent-danger:active::after,
    .bp3-menu-item.bp3-intent-danger:active .bp3-menu-item-label, .bp3-menu-item.bp3-intent-danger.bp3-active, .bp3-menu-item.bp3-intent-danger.bp3-active::before, .bp3-menu-item.bp3-intent-danger.bp3-active::after,
    .bp3-menu-item.bp3-intent-danger.bp3-active .bp3-menu-item-label{
      color:#ffffff; }
  .bp3-menu-item::before{
    font-family:"Icons16", sans-serif;
    font-size:16px;
    font-style:normal;
    font-weight:400;
    line-height:1;
    -moz-osx-font-smoothing:grayscale;
    -webkit-font-smoothing:antialiased;
    margin-right:7px; }
  .bp3-menu-item::before,
  .bp3-menu-item > .bp3-icon{
    color:#5c7080;
    margin-top:2px; }
  .bp3-menu-item .bp3-menu-item-label{
    color:#5c7080; }
  .bp3-menu-item:hover, .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-menu-item{
    color:inherit; }
  .bp3-menu-item.bp3-active, .bp3-menu-item:active{
    background-color:rgba(115, 134, 148, 0.3); }
  .bp3-menu-item.bp3-disabled{
    background-color:inherit !important;
    color:rgba(92, 112, 128, 0.6) !important;
    cursor:not-allowed !important;
    outline:none !important; }
    .bp3-menu-item.bp3-disabled::before,
    .bp3-menu-item.bp3-disabled > .bp3-icon,
    .bp3-menu-item.bp3-disabled .bp3-menu-item-label{
      color:rgba(92, 112, 128, 0.6) !important; }
  .bp3-large .bp3-menu-item{
    font-size:16px;
    line-height:22px;
    padding:9px 7px; }
    .bp3-large .bp3-menu-item .bp3-icon{
      margin-top:3px; }
    .bp3-large .bp3-menu-item::before{
      font-family:"Icons20", sans-serif;
      font-size:20px;
      font-style:normal;
      font-weight:400;
      line-height:1;
      -moz-osx-font-smoothing:grayscale;
      -webkit-font-smoothing:antialiased;
      margin-right:10px;
      margin-top:1px; }

button.bp3-menu-item{
  background:none;
  border:none;
  text-align:left;
  width:100%; }
.bp3-menu-header{
  border-top:1px solid rgba(16, 22, 26, 0.15);
  display:block;
  margin:5px;
  cursor:default;
  padding-left:2px; }
  .bp3-dark .bp3-menu-header{
    border-top-color:rgba(255, 255, 255, 0.15); }
  .bp3-menu-header:first-of-type{
    border-top:none; }
  .bp3-menu-header > h6{
    color:#182026;
    font-weight:600;
    overflow:hidden;
    text-overflow:ellipsis;
    white-space:nowrap;
    word-wrap:normal;
    line-height:17px;
    margin:0;
    padding:10px 7px 0 1px; }
    .bp3-dark .bp3-menu-header > h6{
      color:#f5f8fa; }
  .bp3-menu-header:first-of-type > h6{
    padding-top:0; }
  .bp3-large .bp3-menu-header > h6{
    font-size:18px;
    padding-bottom:5px;
    padding-top:15px; }
  .bp3-large .bp3-menu-header:first-of-type > h6{
    padding-top:0; }

.bp3-dark .bp3-menu{
  background:#30404d;
  color:#f5f8fa; }

.bp3-dark .bp3-menu-item{ }
  .bp3-dark .bp3-menu-item.bp3-intent-primary{
    color:#48aff0; }
    .bp3-dark .bp3-menu-item.bp3-intent-primary .bp3-icon{
      color:inherit; }
    .bp3-dark .bp3-menu-item.bp3-intent-primary::before, .bp3-dark .bp3-menu-item.bp3-intent-primary::after,
    .bp3-dark .bp3-menu-item.bp3-intent-primary .bp3-menu-item-label{
      color:#48aff0; }
    .bp3-dark .bp3-menu-item.bp3-intent-primary:hover, .bp3-dark .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-primary.bp3-menu-item, .bp3-submenu .bp3-dark .bp3-popover-target.bp3-popover-open > .bp3-intent-primary.bp3-menu-item, .bp3-dark .bp3-menu-item.bp3-intent-primary.bp3-active{
      background-color:#137cbd; }
    .bp3-dark .bp3-menu-item.bp3-intent-primary:active{
      background-color:#106ba3; }
    .bp3-dark .bp3-menu-item.bp3-intent-primary:hover, .bp3-dark .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-primary.bp3-menu-item, .bp3-submenu .bp3-dark .bp3-popover-target.bp3-popover-open > .bp3-intent-primary.bp3-menu-item, .bp3-dark .bp3-menu-item.bp3-intent-primary:hover::before, .bp3-dark .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-primary.bp3-menu-item::before, .bp3-submenu .bp3-dark .bp3-popover-target.bp3-popover-open > .bp3-intent-primary.bp3-menu-item::before, .bp3-dark .bp3-menu-item.bp3-intent-primary:hover::after, .bp3-dark .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-primary.bp3-menu-item::after, .bp3-submenu .bp3-dark .bp3-popover-target.bp3-popover-open > .bp3-intent-primary.bp3-menu-item::after,
    .bp3-dark .bp3-menu-item.bp3-intent-primary:hover .bp3-menu-item-label,
    .bp3-dark .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-primary.bp3-menu-item .bp3-menu-item-label,
    .bp3-submenu .bp3-dark .bp3-popover-target.bp3-popover-open > .bp3-intent-primary.bp3-menu-item .bp3-menu-item-label, .bp3-dark .bp3-menu-item.bp3-intent-primary:active, .bp3-dark .bp3-menu-item.bp3-intent-primary:active::before, .bp3-dark .bp3-menu-item.bp3-intent-primary:active::after,
    .bp3-dark .bp3-menu-item.bp3-intent-primary:active .bp3-menu-item-label, .bp3-dark .bp3-menu-item.bp3-intent-primary.bp3-active, .bp3-dark .bp3-menu-item.bp3-intent-primary.bp3-active::before, .bp3-dark .bp3-menu-item.bp3-intent-primary.bp3-active::after,
    .bp3-dark .bp3-menu-item.bp3-intent-primary.bp3-active .bp3-menu-item-label{
      color:#ffffff; }
  .bp3-dark .bp3-menu-item.bp3-intent-success{
    color:#3dcc91; }
    .bp3-dark .bp3-menu-item.bp3-intent-success .bp3-icon{
      color:inherit; }
    .bp3-dark .bp3-menu-item.bp3-intent-success::before, .bp3-dark .bp3-menu-item.bp3-intent-success::after,
    .bp3-dark .bp3-menu-item.bp3-intent-success .bp3-menu-item-label{
      color:#3dcc91; }
    .bp3-dark .bp3-menu-item.bp3-intent-success:hover, .bp3-dark .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-success.bp3-menu-item, .bp3-submenu .bp3-dark .bp3-popover-target.bp3-popover-open > .bp3-intent-success.bp3-menu-item, .bp3-dark .bp3-menu-item.bp3-intent-success.bp3-active{
      background-color:#0f9960; }
    .bp3-dark .bp3-menu-item.bp3-intent-success:active{
      background-color:#0d8050; }
    .bp3-dark .bp3-menu-item.bp3-intent-success:hover, .bp3-dark .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-success.bp3-menu-item, .bp3-submenu .bp3-dark .bp3-popover-target.bp3-popover-open > .bp3-intent-success.bp3-menu-item, .bp3-dark .bp3-menu-item.bp3-intent-success:hover::before, .bp3-dark .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-success.bp3-menu-item::before, .bp3-submenu .bp3-dark .bp3-popover-target.bp3-popover-open > .bp3-intent-success.bp3-menu-item::before, .bp3-dark .bp3-menu-item.bp3-intent-success:hover::after, .bp3-dark .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-success.bp3-menu-item::after, .bp3-submenu .bp3-dark .bp3-popover-target.bp3-popover-open > .bp3-intent-success.bp3-menu-item::after,
    .bp3-dark .bp3-menu-item.bp3-intent-success:hover .bp3-menu-item-label,
    .bp3-dark .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-success.bp3-menu-item .bp3-menu-item-label,
    .bp3-submenu .bp3-dark .bp3-popover-target.bp3-popover-open > .bp3-intent-success.bp3-menu-item .bp3-menu-item-label, .bp3-dark .bp3-menu-item.bp3-intent-success:active, .bp3-dark .bp3-menu-item.bp3-intent-success:active::before, .bp3-dark .bp3-menu-item.bp3-intent-success:active::after,
    .bp3-dark .bp3-menu-item.bp3-intent-success:active .bp3-menu-item-label, .bp3-dark .bp3-menu-item.bp3-intent-success.bp3-active, .bp3-dark .bp3-menu-item.bp3-intent-success.bp3-active::before, .bp3-dark .bp3-menu-item.bp3-intent-success.bp3-active::after,
    .bp3-dark .bp3-menu-item.bp3-intent-success.bp3-active .bp3-menu-item-label{
      color:#ffffff; }
  .bp3-dark .bp3-menu-item.bp3-intent-warning{
    color:#ffb366; }
    .bp3-dark .bp3-menu-item.bp3-intent-warning .bp3-icon{
      color:inherit; }
    .bp3-dark .bp3-menu-item.bp3-intent-warning::before, .bp3-dark .bp3-menu-item.bp3-intent-warning::after,
    .bp3-dark .bp3-menu-item.bp3-intent-warning .bp3-menu-item-label{
      color:#ffb366; }
    .bp3-dark .bp3-menu-item.bp3-intent-warning:hover, .bp3-dark .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-warning.bp3-menu-item, .bp3-submenu .bp3-dark .bp3-popover-target.bp3-popover-open > .bp3-intent-warning.bp3-menu-item, .bp3-dark .bp3-menu-item.bp3-intent-warning.bp3-active{
      background-color:#d9822b; }
    .bp3-dark .bp3-menu-item.bp3-intent-warning:active{
      background-color:#bf7326; }
    .bp3-dark .bp3-menu-item.bp3-intent-warning:hover, .bp3-dark .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-warning.bp3-menu-item, .bp3-submenu .bp3-dark .bp3-popover-target.bp3-popover-open > .bp3-intent-warning.bp3-menu-item, .bp3-dark .bp3-menu-item.bp3-intent-warning:hover::before, .bp3-dark .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-warning.bp3-menu-item::before, .bp3-submenu .bp3-dark .bp3-popover-target.bp3-popover-open > .bp3-intent-warning.bp3-menu-item::before, .bp3-dark .bp3-menu-item.bp3-intent-warning:hover::after, .bp3-dark .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-warning.bp3-menu-item::after, .bp3-submenu .bp3-dark .bp3-popover-target.bp3-popover-open > .bp3-intent-warning.bp3-menu-item::after,
    .bp3-dark .bp3-menu-item.bp3-intent-warning:hover .bp3-menu-item-label,
    .bp3-dark .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-warning.bp3-menu-item .bp3-menu-item-label,
    .bp3-submenu .bp3-dark .bp3-popover-target.bp3-popover-open > .bp3-intent-warning.bp3-menu-item .bp3-menu-item-label, .bp3-dark .bp3-menu-item.bp3-intent-warning:active, .bp3-dark .bp3-menu-item.bp3-intent-warning:active::before, .bp3-dark .bp3-menu-item.bp3-intent-warning:active::after,
    .bp3-dark .bp3-menu-item.bp3-intent-warning:active .bp3-menu-item-label, .bp3-dark .bp3-menu-item.bp3-intent-warning.bp3-active, .bp3-dark .bp3-menu-item.bp3-intent-warning.bp3-active::before, .bp3-dark .bp3-menu-item.bp3-intent-warning.bp3-active::after,
    .bp3-dark .bp3-menu-item.bp3-intent-warning.bp3-active .bp3-menu-item-label{
      color:#ffffff; }
  .bp3-dark .bp3-menu-item.bp3-intent-danger{
    color:#ff7373; }
    .bp3-dark .bp3-menu-item.bp3-intent-danger .bp3-icon{
      color:inherit; }
    .bp3-dark .bp3-menu-item.bp3-intent-danger::before, .bp3-dark .bp3-menu-item.bp3-intent-danger::after,
    .bp3-dark .bp3-menu-item.bp3-intent-danger .bp3-menu-item-label{
      color:#ff7373; }
    .bp3-dark .bp3-menu-item.bp3-intent-danger:hover, .bp3-dark .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-danger.bp3-menu-item, .bp3-submenu .bp3-dark .bp3-popover-target.bp3-popover-open > .bp3-intent-danger.bp3-menu-item, .bp3-dark .bp3-menu-item.bp3-intent-danger.bp3-active{
      background-color:#db3737; }
    .bp3-dark .bp3-menu-item.bp3-intent-danger:active{
      background-color:#c23030; }
    .bp3-dark .bp3-menu-item.bp3-intent-danger:hover, .bp3-dark .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-danger.bp3-menu-item, .bp3-submenu .bp3-dark .bp3-popover-target.bp3-popover-open > .bp3-intent-danger.bp3-menu-item, .bp3-dark .bp3-menu-item.bp3-intent-danger:hover::before, .bp3-dark .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-danger.bp3-menu-item::before, .bp3-submenu .bp3-dark .bp3-popover-target.bp3-popover-open > .bp3-intent-danger.bp3-menu-item::before, .bp3-dark .bp3-menu-item.bp3-intent-danger:hover::after, .bp3-dark .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-danger.bp3-menu-item::after, .bp3-submenu .bp3-dark .bp3-popover-target.bp3-popover-open > .bp3-intent-danger.bp3-menu-item::after,
    .bp3-dark .bp3-menu-item.bp3-intent-danger:hover .bp3-menu-item-label,
    .bp3-dark .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-danger.bp3-menu-item .bp3-menu-item-label,
    .bp3-submenu .bp3-dark .bp3-popover-target.bp3-popover-open > .bp3-intent-danger.bp3-menu-item .bp3-menu-item-label, .bp3-dark .bp3-menu-item.bp3-intent-danger:active, .bp3-dark .bp3-menu-item.bp3-intent-danger:active::before, .bp3-dark .bp3-menu-item.bp3-intent-danger:active::after,
    .bp3-dark .bp3-menu-item.bp3-intent-danger:active .bp3-menu-item-label, .bp3-dark .bp3-menu-item.bp3-intent-danger.bp3-active, .bp3-dark .bp3-menu-item.bp3-intent-danger.bp3-active::before, .bp3-dark .bp3-menu-item.bp3-intent-danger.bp3-active::after,
    .bp3-dark .bp3-menu-item.bp3-intent-danger.bp3-active .bp3-menu-item-label{
      color:#ffffff; }
  .bp3-dark .bp3-menu-item::before,
  .bp3-dark .bp3-menu-item > .bp3-icon{
    color:#a7b6c2; }
  .bp3-dark .bp3-menu-item .bp3-menu-item-label{
    color:#a7b6c2; }
  .bp3-dark .bp3-menu-item.bp3-active, .bp3-dark .bp3-menu-item:active{
    background-color:rgba(138, 155, 168, 0.3); }
  .bp3-dark .bp3-menu-item.bp3-disabled{
    color:rgba(167, 182, 194, 0.6) !important; }
    .bp3-dark .bp3-menu-item.bp3-disabled::before,
    .bp3-dark .bp3-menu-item.bp3-disabled > .bp3-icon,
    .bp3-dark .bp3-menu-item.bp3-disabled .bp3-menu-item-label{
      color:rgba(167, 182, 194, 0.6) !important; }

.bp3-dark .bp3-menu-divider,
.bp3-dark .bp3-menu-header{
  border-color:rgba(255, 255, 255, 0.15); }

.bp3-dark .bp3-menu-header > h6{
  color:#f5f8fa; }

.bp3-label .bp3-menu{
  margin-top:5px; }
.bp3-navbar{
  background-color:#ffffff;
  -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 0 0 rgba(16, 22, 26, 0), 0 1px 1px rgba(16, 22, 26, 0.2);
          box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 0 0 rgba(16, 22, 26, 0), 0 1px 1px rgba(16, 22, 26, 0.2);
  height:50px;
  padding:0 15px;
  position:relative;
  width:100%;
  z-index:10; }
  .bp3-navbar.bp3-dark,
  .bp3-dark .bp3-navbar{
    background-color:#394b59; }
  .bp3-navbar.bp3-dark{
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), 0 0 0 rgba(16, 22, 26, 0), 0 1px 1px rgba(16, 22, 26, 0.4);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), 0 0 0 rgba(16, 22, 26, 0), 0 1px 1px rgba(16, 22, 26, 0.4); }
  .bp3-dark .bp3-navbar{
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 0 0 rgba(16, 22, 26, 0), 0 1px 1px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 0 0 rgba(16, 22, 26, 0), 0 1px 1px rgba(16, 22, 26, 0.4); }
  .bp3-navbar.bp3-fixed-top{
    left:0;
    position:fixed;
    right:0;
    top:0; }

.bp3-navbar-heading{
  font-size:16px;
  margin-right:15px; }

.bp3-navbar-group{
  -webkit-box-align:center;
      -ms-flex-align:center;
          align-items:center;
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  height:50px; }
  .bp3-navbar-group.bp3-align-left{
    float:left; }
  .bp3-navbar-group.bp3-align-right{
    float:right; }

.bp3-navbar-divider{
  border-left:1px solid rgba(16, 22, 26, 0.15);
  height:20px;
  margin:0 10px; }
  .bp3-dark .bp3-navbar-divider{
    border-left-color:rgba(255, 255, 255, 0.15); }
.bp3-non-ideal-state{
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  -webkit-box-orient:vertical;
  -webkit-box-direction:normal;
      -ms-flex-direction:column;
          flex-direction:column;
  -webkit-box-align:center;
      -ms-flex-align:center;
          align-items:center;
  height:100%;
  -webkit-box-pack:center;
      -ms-flex-pack:center;
          justify-content:center;
  text-align:center;
  width:100%; }
  .bp3-non-ideal-state > *{
    -webkit-box-flex:0;
        -ms-flex-positive:0;
            flex-grow:0;
    -ms-flex-negative:0;
        flex-shrink:0; }
  .bp3-non-ideal-state > .bp3-fill{
    -webkit-box-flex:1;
        -ms-flex-positive:1;
            flex-grow:1;
    -ms-flex-negative:1;
        flex-shrink:1; }
  .bp3-non-ideal-state::before,
  .bp3-non-ideal-state > *{
    margin-bottom:20px; }
  .bp3-non-ideal-state:empty::before,
  .bp3-non-ideal-state > :last-child{
    margin-bottom:0; }
  .bp3-non-ideal-state > *{
    max-width:400px; }

.bp3-non-ideal-state-visual{
  color:rgba(92, 112, 128, 0.6);
  font-size:60px; }
  .bp3-dark .bp3-non-ideal-state-visual{
    color:rgba(167, 182, 194, 0.6); }

.bp3-overflow-list{
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  -ms-flex-wrap:nowrap;
      flex-wrap:nowrap;
  min-width:0; }

.bp3-overflow-list-spacer{
  -ms-flex-negative:1;
      flex-shrink:1;
  width:1px; }

body.bp3-overlay-open{
  overflow:hidden; }

.bp3-overlay{
  bottom:0;
  left:0;
  position:static;
  right:0;
  top:0;
  z-index:20; }
  .bp3-overlay:not(.bp3-overlay-open){
    pointer-events:none; }
  .bp3-overlay.bp3-overlay-container{
    overflow:hidden;
    position:fixed; }
    .bp3-overlay.bp3-overlay-container.bp3-overlay-inline{
      position:absolute; }
  .bp3-overlay.bp3-overlay-scroll-container{
    overflow:auto;
    position:fixed; }
    .bp3-overlay.bp3-overlay-scroll-container.bp3-overlay-inline{
      position:absolute; }
  .bp3-overlay.bp3-overlay-inline{
    display:inline;
    overflow:visible; }

.bp3-overlay-content{
  position:fixed;
  z-index:20; }
  .bp3-overlay-inline .bp3-overlay-content,
  .bp3-overlay-scroll-container .bp3-overlay-content{
    position:absolute; }

.bp3-overlay-backdrop{
  bottom:0;
  left:0;
  position:fixed;
  right:0;
  top:0;
  opacity:1;
  background-color:rgba(16, 22, 26, 0.7);
  overflow:auto;
  -webkit-user-select:none;
     -moz-user-select:none;
      -ms-user-select:none;
          user-select:none;
  z-index:20; }
  .bp3-overlay-backdrop.bp3-overlay-enter, .bp3-overlay-backdrop.bp3-overlay-appear{
    opacity:0; }
  .bp3-overlay-backdrop.bp3-overlay-enter-active, .bp3-overlay-backdrop.bp3-overlay-appear-active{
    opacity:1;
    -webkit-transition-delay:0;
            transition-delay:0;
    -webkit-transition-duration:200ms;
            transition-duration:200ms;
    -webkit-transition-property:opacity;
    transition-property:opacity;
    -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
            transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
  .bp3-overlay-backdrop.bp3-overlay-exit{
    opacity:1; }
  .bp3-overlay-backdrop.bp3-overlay-exit-active{
    opacity:0;
    -webkit-transition-delay:0;
            transition-delay:0;
    -webkit-transition-duration:200ms;
            transition-duration:200ms;
    -webkit-transition-property:opacity;
    transition-property:opacity;
    -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
            transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
  .bp3-overlay-backdrop:focus{
    outline:none; }
  .bp3-overlay-inline .bp3-overlay-backdrop{
    position:absolute; }
.bp3-panel-stack{
  overflow:hidden;
  position:relative; }

.bp3-panel-stack-header{
  -webkit-box-align:center;
      -ms-flex-align:center;
          align-items:center;
  -webkit-box-shadow:0 1px rgba(16, 22, 26, 0.15);
          box-shadow:0 1px rgba(16, 22, 26, 0.15);
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  -ms-flex-negative:0;
      flex-shrink:0;
  height:30px;
  z-index:1; }
  .bp3-dark .bp3-panel-stack-header{
    -webkit-box-shadow:0 1px rgba(255, 255, 255, 0.15);
            box-shadow:0 1px rgba(255, 255, 255, 0.15); }
  .bp3-panel-stack-header > span{
    -webkit-box-align:stretch;
        -ms-flex-align:stretch;
            align-items:stretch;
    display:-webkit-box;
    display:-ms-flexbox;
    display:flex;
    -webkit-box-flex:1;
        -ms-flex:1;
            flex:1; }
  .bp3-panel-stack-header .bp3-heading{
    margin:0 5px; }

.bp3-button.bp3-panel-stack-header-back{
  margin-left:5px;
  padding-left:0;
  white-space:nowrap; }
  .bp3-button.bp3-panel-stack-header-back .bp3-icon{
    margin:0 2px; }

.bp3-panel-stack-view{
  bottom:0;
  left:0;
  position:absolute;
  right:0;
  top:0;
  background-color:#ffffff;
  border-right:1px solid rgba(16, 22, 26, 0.15);
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  -webkit-box-orient:vertical;
  -webkit-box-direction:normal;
      -ms-flex-direction:column;
          flex-direction:column;
  margin-right:-1px;
  overflow-y:auto;
  z-index:1; }
  .bp3-dark .bp3-panel-stack-view{
    background-color:#30404d; }
  .bp3-panel-stack-view:nth-last-child(n + 4){
    display:none; }

.bp3-panel-stack-push .bp3-panel-stack-enter, .bp3-panel-stack-push .bp3-panel-stack-appear{
  -webkit-transform:translateX(100%);
          transform:translateX(100%);
  opacity:0; }

.bp3-panel-stack-push .bp3-panel-stack-enter-active, .bp3-panel-stack-push .bp3-panel-stack-appear-active{
  -webkit-transform:translate(0%);
          transform:translate(0%);
  opacity:1;
  -webkit-transition-delay:0;
          transition-delay:0;
  -webkit-transition-duration:400ms;
          transition-duration:400ms;
  -webkit-transition-property:opacity, -webkit-transform;
  transition-property:opacity, -webkit-transform;
  transition-property:transform, opacity;
  transition-property:transform, opacity, -webkit-transform;
  -webkit-transition-timing-function:ease;
          transition-timing-function:ease; }

.bp3-panel-stack-push .bp3-panel-stack-exit{
  -webkit-transform:translate(0%);
          transform:translate(0%);
  opacity:1; }

.bp3-panel-stack-push .bp3-panel-stack-exit-active{
  -webkit-transform:translateX(-50%);
          transform:translateX(-50%);
  opacity:0;
  -webkit-transition-delay:0;
          transition-delay:0;
  -webkit-transition-duration:400ms;
          transition-duration:400ms;
  -webkit-transition-property:opacity, -webkit-transform;
  transition-property:opacity, -webkit-transform;
  transition-property:transform, opacity;
  transition-property:transform, opacity, -webkit-transform;
  -webkit-transition-timing-function:ease;
          transition-timing-function:ease; }

.bp3-panel-stack-pop .bp3-panel-stack-enter, .bp3-panel-stack-pop .bp3-panel-stack-appear{
  -webkit-transform:translateX(-50%);
          transform:translateX(-50%);
  opacity:0; }

.bp3-panel-stack-pop .bp3-panel-stack-enter-active, .bp3-panel-stack-pop .bp3-panel-stack-appear-active{
  -webkit-transform:translate(0%);
          transform:translate(0%);
  opacity:1;
  -webkit-transition-delay:0;
          transition-delay:0;
  -webkit-transition-duration:400ms;
          transition-duration:400ms;
  -webkit-transition-property:opacity, -webkit-transform;
  transition-property:opacity, -webkit-transform;
  transition-property:transform, opacity;
  transition-property:transform, opacity, -webkit-transform;
  -webkit-transition-timing-function:ease;
          transition-timing-function:ease; }

.bp3-panel-stack-pop .bp3-panel-stack-exit{
  -webkit-transform:translate(0%);
          transform:translate(0%);
  opacity:1; }

.bp3-panel-stack-pop .bp3-panel-stack-exit-active{
  -webkit-transform:translateX(100%);
          transform:translateX(100%);
  opacity:0;
  -webkit-transition-delay:0;
          transition-delay:0;
  -webkit-transition-duration:400ms;
          transition-duration:400ms;
  -webkit-transition-property:opacity, -webkit-transform;
  transition-property:opacity, -webkit-transform;
  transition-property:transform, opacity;
  transition-property:transform, opacity, -webkit-transform;
  -webkit-transition-timing-function:ease;
          transition-timing-function:ease; }
.bp3-panel-stack2{
  overflow:hidden;
  position:relative; }

.bp3-panel-stack2-header{
  -webkit-box-align:center;
      -ms-flex-align:center;
          align-items:center;
  -webkit-box-shadow:0 1px rgba(16, 22, 26, 0.15);
          box-shadow:0 1px rgba(16, 22, 26, 0.15);
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  -ms-flex-negative:0;
      flex-shrink:0;
  height:30px;
  z-index:1; }
  .bp3-dark .bp3-panel-stack2-header{
    -webkit-box-shadow:0 1px rgba(255, 255, 255, 0.15);
            box-shadow:0 1px rgba(255, 255, 255, 0.15); }
  .bp3-panel-stack2-header > span{
    -webkit-box-align:stretch;
        -ms-flex-align:stretch;
            align-items:stretch;
    display:-webkit-box;
    display:-ms-flexbox;
    display:flex;
    -webkit-box-flex:1;
        -ms-flex:1;
            flex:1; }
  .bp3-panel-stack2-header .bp3-heading{
    margin:0 5px; }

.bp3-button.bp3-panel-stack2-header-back{
  margin-left:5px;
  padding-left:0;
  white-space:nowrap; }
  .bp3-button.bp3-panel-stack2-header-back .bp3-icon{
    margin:0 2px; }

.bp3-panel-stack2-view{
  bottom:0;
  left:0;
  position:absolute;
  right:0;
  top:0;
  background-color:#ffffff;
  border-right:1px solid rgba(16, 22, 26, 0.15);
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  -webkit-box-orient:vertical;
  -webkit-box-direction:normal;
      -ms-flex-direction:column;
          flex-direction:column;
  margin-right:-1px;
  overflow-y:auto;
  z-index:1; }
  .bp3-dark .bp3-panel-stack2-view{
    background-color:#30404d; }
  .bp3-panel-stack2-view:nth-last-child(n + 4){
    display:none; }

.bp3-panel-stack2-push .bp3-panel-stack2-enter, .bp3-panel-stack2-push .bp3-panel-stack2-appear{
  -webkit-transform:translateX(100%);
          transform:translateX(100%);
  opacity:0; }

.bp3-panel-stack2-push .bp3-panel-stack2-enter-active, .bp3-panel-stack2-push .bp3-panel-stack2-appear-active{
  -webkit-transform:translate(0%);
          transform:translate(0%);
  opacity:1;
  -webkit-transition-delay:0;
          transition-delay:0;
  -webkit-transition-duration:400ms;
          transition-duration:400ms;
  -webkit-transition-property:opacity, -webkit-transform;
  transition-property:opacity, -webkit-transform;
  transition-property:transform, opacity;
  transition-property:transform, opacity, -webkit-transform;
  -webkit-transition-timing-function:ease;
          transition-timing-function:ease; }

.bp3-panel-stack2-push .bp3-panel-stack2-exit{
  -webkit-transform:translate(0%);
          transform:translate(0%);
  opacity:1; }

.bp3-panel-stack2-push .bp3-panel-stack2-exit-active{
  -webkit-transform:translateX(-50%);
          transform:translateX(-50%);
  opacity:0;
  -webkit-transition-delay:0;
          transition-delay:0;
  -webkit-transition-duration:400ms;
          transition-duration:400ms;
  -webkit-transition-property:opacity, -webkit-transform;
  transition-property:opacity, -webkit-transform;
  transition-property:transform, opacity;
  transition-property:transform, opacity, -webkit-transform;
  -webkit-transition-timing-function:ease;
          transition-timing-function:ease; }

.bp3-panel-stack2-pop .bp3-panel-stack2-enter, .bp3-panel-stack2-pop .bp3-panel-stack2-appear{
  -webkit-transform:translateX(-50%);
          transform:translateX(-50%);
  opacity:0; }

.bp3-panel-stack2-pop .bp3-panel-stack2-enter-active, .bp3-panel-stack2-pop .bp3-panel-stack2-appear-active{
  -webkit-transform:translate(0%);
          transform:translate(0%);
  opacity:1;
  -webkit-transition-delay:0;
          transition-delay:0;
  -webkit-transition-duration:400ms;
          transition-duration:400ms;
  -webkit-transition-property:opacity, -webkit-transform;
  transition-property:opacity, -webkit-transform;
  transition-property:transform, opacity;
  transition-property:transform, opacity, -webkit-transform;
  -webkit-transition-timing-function:ease;
          transition-timing-function:ease; }

.bp3-panel-stack2-pop .bp3-panel-stack2-exit{
  -webkit-transform:translate(0%);
          transform:translate(0%);
  opacity:1; }

.bp3-panel-stack2-pop .bp3-panel-stack2-exit-active{
  -webkit-transform:translateX(100%);
          transform:translateX(100%);
  opacity:0;
  -webkit-transition-delay:0;
          transition-delay:0;
  -webkit-transition-duration:400ms;
          transition-duration:400ms;
  -webkit-transition-property:opacity, -webkit-transform;
  transition-property:opacity, -webkit-transform;
  transition-property:transform, opacity;
  transition-property:transform, opacity, -webkit-transform;
  -webkit-transition-timing-function:ease;
          transition-timing-function:ease; }
.bp3-popover{
  -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 2px 4px rgba(16, 22, 26, 0.2), 0 8px 24px rgba(16, 22, 26, 0.2);
          box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 2px 4px rgba(16, 22, 26, 0.2), 0 8px 24px rgba(16, 22, 26, 0.2);
  -webkit-transform:scale(1);
          transform:scale(1);
  border-radius:3px;
  display:inline-block;
  z-index:20; }
  .bp3-popover .bp3-popover-arrow{
    height:30px;
    position:absolute;
    width:30px; }
    .bp3-popover .bp3-popover-arrow::before{
      height:20px;
      margin:5px;
      width:20px; }
  .bp3-tether-element-attached-bottom.bp3-tether-target-attached-top > .bp3-popover{
    margin-bottom:17px;
    margin-top:-17px; }
    .bp3-tether-element-attached-bottom.bp3-tether-target-attached-top > .bp3-popover > .bp3-popover-arrow{
      bottom:-11px; }
      .bp3-tether-element-attached-bottom.bp3-tether-target-attached-top > .bp3-popover > .bp3-popover-arrow svg{
        -webkit-transform:rotate(-90deg);
                transform:rotate(-90deg); }
  .bp3-tether-element-attached-left.bp3-tether-target-attached-right > .bp3-popover{
    margin-left:17px; }
    .bp3-tether-element-attached-left.bp3-tether-target-attached-right > .bp3-popover > .bp3-popover-arrow{
      left:-11px; }
      .bp3-tether-element-attached-left.bp3-tether-target-attached-right > .bp3-popover > .bp3-popover-arrow svg{
        -webkit-transform:rotate(0);
                transform:rotate(0); }
  .bp3-tether-element-attached-top.bp3-tether-target-attached-bottom > .bp3-popover{
    margin-top:17px; }
    .bp3-tether-element-attached-top.bp3-tether-target-attached-bottom > .bp3-popover > .bp3-popover-arrow{
      top:-11px; }
      .bp3-tether-element-attached-top.bp3-tether-target-attached-bottom > .bp3-popover > .bp3-popover-arrow svg{
        -webkit-transform:rotate(90deg);
                transform:rotate(90deg); }
  .bp3-tether-element-attached-right.bp3-tether-target-attached-left > .bp3-popover{
    margin-left:-17px;
    margin-right:17px; }
    .bp3-tether-element-attached-right.bp3-tether-target-attached-left > .bp3-popover > .bp3-popover-arrow{
      right:-11px; }
      .bp3-tether-element-attached-right.bp3-tether-target-attached-left > .bp3-popover > .bp3-popover-arrow svg{
        -webkit-transform:rotate(180deg);
                transform:rotate(180deg); }
  .bp3-tether-element-attached-middle > .bp3-popover > .bp3-popover-arrow{
    top:50%;
    -webkit-transform:translateY(-50%);
            transform:translateY(-50%); }
  .bp3-tether-element-attached-center > .bp3-popover > .bp3-popover-arrow{
    right:50%;
    -webkit-transform:translateX(50%);
            transform:translateX(50%); }
  .bp3-tether-element-attached-top.bp3-tether-target-attached-top > .bp3-popover > .bp3-popover-arrow{
    top:-0.3934px; }
  .bp3-tether-element-attached-right.bp3-tether-target-attached-right > .bp3-popover > .bp3-popover-arrow{
    right:-0.3934px; }
  .bp3-tether-element-attached-left.bp3-tether-target-attached-left > .bp3-popover > .bp3-popover-arrow{
    left:-0.3934px; }
  .bp3-tether-element-attached-bottom.bp3-tether-target-attached-bottom > .bp3-popover > .bp3-popover-arrow{
    bottom:-0.3934px; }
  .bp3-tether-element-attached-top.bp3-tether-element-attached-left > .bp3-popover{
    -webkit-transform-origin:top left;
            transform-origin:top left; }
  .bp3-tether-element-attached-top.bp3-tether-element-attached-center > .bp3-popover{
    -webkit-transform-origin:top center;
            transform-origin:top center; }
  .bp3-tether-element-attached-top.bp3-tether-element-attached-right > .bp3-popover{
    -webkit-transform-origin:top right;
            transform-origin:top right; }
  .bp3-tether-element-attached-middle.bp3-tether-element-attached-left > .bp3-popover{
    -webkit-transform-origin:center left;
            transform-origin:center left; }
  .bp3-tether-element-attached-middle.bp3-tether-element-attached-center > .bp3-popover{
    -webkit-transform-origin:center center;
            transform-origin:center center; }
  .bp3-tether-element-attached-middle.bp3-tether-element-attached-right > .bp3-popover{
    -webkit-transform-origin:center right;
            transform-origin:center right; }
  .bp3-tether-element-attached-bottom.bp3-tether-element-attached-left > .bp3-popover{
    -webkit-transform-origin:bottom left;
            transform-origin:bottom left; }
  .bp3-tether-element-attached-bottom.bp3-tether-element-attached-center > .bp3-popover{
    -webkit-transform-origin:bottom center;
            transform-origin:bottom center; }
  .bp3-tether-element-attached-bottom.bp3-tether-element-attached-right > .bp3-popover{
    -webkit-transform-origin:bottom right;
            transform-origin:bottom right; }
  .bp3-popover .bp3-popover-content{
    background:#ffffff;
    color:inherit; }
  .bp3-popover .bp3-popover-arrow::before{
    -webkit-box-shadow:1px 1px 6px rgba(16, 22, 26, 0.2);
            box-shadow:1px 1px 6px rgba(16, 22, 26, 0.2); }
  .bp3-popover .bp3-popover-arrow-border{
    fill:#10161a;
    fill-opacity:0.1; }
  .bp3-popover .bp3-popover-arrow-fill{
    fill:#ffffff; }
  .bp3-popover-enter > .bp3-popover, .bp3-popover-appear > .bp3-popover{
    -webkit-transform:scale(0.3);
            transform:scale(0.3); }
  .bp3-popover-enter-active > .bp3-popover, .bp3-popover-appear-active > .bp3-popover{
    -webkit-transform:scale(1);
            transform:scale(1);
    -webkit-transition-delay:0;
            transition-delay:0;
    -webkit-transition-duration:300ms;
            transition-duration:300ms;
    -webkit-transition-property:-webkit-transform;
    transition-property:-webkit-transform;
    transition-property:transform;
    transition-property:transform, -webkit-transform;
    -webkit-transition-timing-function:cubic-bezier(0.54, 1.12, 0.38, 1.11);
            transition-timing-function:cubic-bezier(0.54, 1.12, 0.38, 1.11); }
  .bp3-popover-exit > .bp3-popover{
    -webkit-transform:scale(1);
            transform:scale(1); }
  .bp3-popover-exit-active > .bp3-popover{
    -webkit-transform:scale(0.3);
            transform:scale(0.3);
    -webkit-transition-delay:0;
            transition-delay:0;
    -webkit-transition-duration:300ms;
            transition-duration:300ms;
    -webkit-transition-property:-webkit-transform;
    transition-property:-webkit-transform;
    transition-property:transform;
    transition-property:transform, -webkit-transform;
    -webkit-transition-timing-function:cubic-bezier(0.54, 1.12, 0.38, 1.11);
            transition-timing-function:cubic-bezier(0.54, 1.12, 0.38, 1.11); }
  .bp3-popover .bp3-popover-content{
    border-radius:3px;
    position:relative; }
  .bp3-popover.bp3-popover-content-sizing .bp3-popover-content{
    max-width:350px;
    padding:20px; }
  .bp3-popover-target + .bp3-overlay .bp3-popover.bp3-popover-content-sizing{
    width:350px; }
  .bp3-popover.bp3-minimal{
    margin:0 !important; }
    .bp3-popover.bp3-minimal .bp3-popover-arrow{
      display:none; }
    .bp3-popover.bp3-minimal.bp3-popover{
      -webkit-transform:scale(1);
              transform:scale(1); }
      .bp3-popover-enter > .bp3-popover.bp3-minimal.bp3-popover, .bp3-popover-appear > .bp3-popover.bp3-minimal.bp3-popover{
        -webkit-transform:scale(1);
                transform:scale(1); }
      .bp3-popover-enter-active > .bp3-popover.bp3-minimal.bp3-popover, .bp3-popover-appear-active > .bp3-popover.bp3-minimal.bp3-popover{
        -webkit-transform:scale(1);
                transform:scale(1);
        -webkit-transition-delay:0;
                transition-delay:0;
        -webkit-transition-duration:100ms;
                transition-duration:100ms;
        -webkit-transition-property:-webkit-transform;
        transition-property:-webkit-transform;
        transition-property:transform;
        transition-property:transform, -webkit-transform;
        -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
                transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
      .bp3-popover-exit > .bp3-popover.bp3-minimal.bp3-popover{
        -webkit-transform:scale(1);
                transform:scale(1); }
      .bp3-popover-exit-active > .bp3-popover.bp3-minimal.bp3-popover{
        -webkit-transform:scale(1);
                transform:scale(1);
        -webkit-transition-delay:0;
                transition-delay:0;
        -webkit-transition-duration:100ms;
                transition-duration:100ms;
        -webkit-transition-property:-webkit-transform;
        transition-property:-webkit-transform;
        transition-property:transform;
        transition-property:transform, -webkit-transform;
        -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
                transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
  .bp3-popover.bp3-dark,
  .bp3-dark .bp3-popover{
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 2px 4px rgba(16, 22, 26, 0.4), 0 8px 24px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 2px 4px rgba(16, 22, 26, 0.4), 0 8px 24px rgba(16, 22, 26, 0.4); }
    .bp3-popover.bp3-dark .bp3-popover-content,
    .bp3-dark .bp3-popover .bp3-popover-content{
      background:#30404d;
      color:inherit; }
    .bp3-popover.bp3-dark .bp3-popover-arrow::before,
    .bp3-dark .bp3-popover .bp3-popover-arrow::before{
      -webkit-box-shadow:1px 1px 6px rgba(16, 22, 26, 0.4);
              box-shadow:1px 1px 6px rgba(16, 22, 26, 0.4); }
    .bp3-popover.bp3-dark .bp3-popover-arrow-border,
    .bp3-dark .bp3-popover .bp3-popover-arrow-border{
      fill:#10161a;
      fill-opacity:0.2; }
    .bp3-popover.bp3-dark .bp3-popover-arrow-fill,
    .bp3-dark .bp3-popover .bp3-popover-arrow-fill{
      fill:#30404d; }

.bp3-popover-arrow::before{
  border-radius:2px;
  content:"";
  display:block;
  position:absolute;
  -webkit-transform:rotate(45deg);
          transform:rotate(45deg); }

.bp3-tether-pinned .bp3-popover-arrow{
  display:none; }

.bp3-popover-backdrop{
  background:rgba(255, 255, 255, 0); }

.bp3-transition-container{
  opacity:1;
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  z-index:20; }
  .bp3-transition-container.bp3-popover-enter, .bp3-transition-container.bp3-popover-appear{
    opacity:0; }
  .bp3-transition-container.bp3-popover-enter-active, .bp3-transition-container.bp3-popover-appear-active{
    opacity:1;
    -webkit-transition-delay:0;
            transition-delay:0;
    -webkit-transition-duration:100ms;
            transition-duration:100ms;
    -webkit-transition-property:opacity;
    transition-property:opacity;
    -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
            transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
  .bp3-transition-container.bp3-popover-exit{
    opacity:1; }
  .bp3-transition-container.bp3-popover-exit-active{
    opacity:0;
    -webkit-transition-delay:0;
            transition-delay:0;
    -webkit-transition-duration:100ms;
            transition-duration:100ms;
    -webkit-transition-property:opacity;
    transition-property:opacity;
    -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
            transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
  .bp3-transition-container:focus{
    outline:none; }
  .bp3-transition-container.bp3-popover-leave .bp3-popover-content{
    pointer-events:none; }
  .bp3-transition-container[data-x-out-of-boundaries]{
    display:none; }

span.bp3-popover-target{
  display:inline-block; }

.bp3-popover-wrapper.bp3-fill{
  width:100%; }

.bp3-portal{
  left:0;
  position:absolute;
  right:0;
  top:0; }
@-webkit-keyframes linear-progress-bar-stripes{
  from{
    background-position:0 0; }
  to{
    background-position:30px 0; } }
@keyframes linear-progress-bar-stripes{
  from{
    background-position:0 0; }
  to{
    background-position:30px 0; } }

.bp3-progress-bar{
  background:rgba(92, 112, 128, 0.2);
  border-radius:40px;
  display:block;
  height:8px;
  overflow:hidden;
  position:relative;
  width:100%; }
  .bp3-progress-bar .bp3-progress-meter{
    background:linear-gradient(-45deg, rgba(255, 255, 255, 0.2) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.2) 50%, rgba(255, 255, 255, 0.2) 75%, transparent 75%);
    background-color:rgba(92, 112, 128, 0.8);
    background-size:30px 30px;
    border-radius:40px;
    height:100%;
    position:absolute;
    -webkit-transition:width 200ms cubic-bezier(0.4, 1, 0.75, 0.9);
    transition:width 200ms cubic-bezier(0.4, 1, 0.75, 0.9);
    width:100%; }
  .bp3-progress-bar:not(.bp3-no-animation):not(.bp3-no-stripes) .bp3-progress-meter{
    animation:linear-progress-bar-stripes 300ms linear infinite reverse; }
  .bp3-progress-bar.bp3-no-stripes .bp3-progress-meter{
    background-image:none; }

.bp3-dark .bp3-progress-bar{
  background:rgba(16, 22, 26, 0.5); }
  .bp3-dark .bp3-progress-bar .bp3-progress-meter{
    background-color:#8a9ba8; }

.bp3-progress-bar.bp3-intent-primary .bp3-progress-meter{
  background-color:#137cbd; }

.bp3-progress-bar.bp3-intent-success .bp3-progress-meter{
  background-color:#0f9960; }

.bp3-progress-bar.bp3-intent-warning .bp3-progress-meter{
  background-color:#d9822b; }

.bp3-progress-bar.bp3-intent-danger .bp3-progress-meter{
  background-color:#db3737; }
@-webkit-keyframes skeleton-glow{
  from{
    background:rgba(206, 217, 224, 0.2);
    border-color:rgba(206, 217, 224, 0.2); }
  to{
    background:rgba(92, 112, 128, 0.2);
    border-color:rgba(92, 112, 128, 0.2); } }
@keyframes skeleton-glow{
  from{
    background:rgba(206, 217, 224, 0.2);
    border-color:rgba(206, 217, 224, 0.2); }
  to{
    background:rgba(92, 112, 128, 0.2);
    border-color:rgba(92, 112, 128, 0.2); } }
.bp3-skeleton{
  -webkit-animation:1000ms linear infinite alternate skeleton-glow;
          animation:1000ms linear infinite alternate skeleton-glow;
  background:rgba(206, 217, 224, 0.2);
  background-clip:padding-box !important;
  border-color:rgba(206, 217, 224, 0.2) !important;
  border-radius:2px;
  -webkit-box-shadow:none !important;
          box-shadow:none !important;
  color:transparent !important;
  cursor:default;
  pointer-events:none;
  -webkit-user-select:none;
     -moz-user-select:none;
      -ms-user-select:none;
          user-select:none; }
  .bp3-skeleton::before, .bp3-skeleton::after,
  .bp3-skeleton *{
    visibility:hidden !important; }
.bp3-slider{
  height:40px;
  min-width:150px;
  width:100%;
  cursor:default;
  outline:none;
  position:relative;
  -webkit-user-select:none;
     -moz-user-select:none;
      -ms-user-select:none;
          user-select:none; }
  .bp3-slider:hover{
    cursor:pointer; }
  .bp3-slider:active{
    cursor:-webkit-grabbing;
    cursor:grabbing; }
  .bp3-slider.bp3-disabled{
    cursor:not-allowed;
    opacity:0.5; }
  .bp3-slider.bp3-slider-unlabeled{
    height:16px; }

.bp3-slider-track,
.bp3-slider-progress{
  height:6px;
  left:0;
  right:0;
  top:5px;
  position:absolute; }

.bp3-slider-track{
  border-radius:3px;
  overflow:hidden; }

.bp3-slider-progress{
  background:rgba(92, 112, 128, 0.2); }
  .bp3-dark .bp3-slider-progress{
    background:rgba(16, 22, 26, 0.5); }
  .bp3-slider-progress.bp3-intent-primary{
    background-color:#137cbd; }
  .bp3-slider-progress.bp3-intent-success{
    background-color:#0f9960; }
  .bp3-slider-progress.bp3-intent-warning{
    background-color:#d9822b; }
  .bp3-slider-progress.bp3-intent-danger{
    background-color:#db3737; }

.bp3-slider-handle{
  background-color:#f5f8fa;
  background-image:-webkit-gradient(linear, left top, left bottom, from(rgba(255, 255, 255, 0.8)), to(rgba(255, 255, 255, 0)));
  background-image:linear-gradient(to bottom, rgba(255, 255, 255, 0.8), rgba(255, 255, 255, 0));
  -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1);
          box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1);
  color:#182026;
  border-radius:3px;
  -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 1px 1px rgba(16, 22, 26, 0.2);
          box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 1px 1px rgba(16, 22, 26, 0.2);
  cursor:pointer;
  height:16px;
  left:0;
  position:absolute;
  top:0;
  width:16px; }
  .bp3-slider-handle:hover{
    background-clip:padding-box;
    background-color:#ebf1f5;
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1); }
  .bp3-slider-handle:active, .bp3-slider-handle.bp3-active{
    background-color:#d8e1e8;
    background-image:none;
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 1px 2px rgba(16, 22, 26, 0.2);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 1px 2px rgba(16, 22, 26, 0.2); }
  .bp3-slider-handle:disabled, .bp3-slider-handle.bp3-disabled{
    background-color:rgba(206, 217, 224, 0.5);
    background-image:none;
    -webkit-box-shadow:none;
            box-shadow:none;
    color:rgba(92, 112, 128, 0.6);
    cursor:not-allowed;
    outline:none; }
    .bp3-slider-handle:disabled.bp3-active, .bp3-slider-handle:disabled.bp3-active:hover, .bp3-slider-handle.bp3-disabled.bp3-active, .bp3-slider-handle.bp3-disabled.bp3-active:hover{
      background:rgba(206, 217, 224, 0.7); }
  .bp3-slider-handle:focus{
    z-index:1; }
  .bp3-slider-handle:hover{
    background-clip:padding-box;
    background-color:#ebf1f5;
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1);
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 1px 1px rgba(16, 22, 26, 0.2);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 1px 1px rgba(16, 22, 26, 0.2);
    cursor:-webkit-grab;
    cursor:grab;
    z-index:2; }
  .bp3-slider-handle.bp3-active{
    background-color:#d8e1e8;
    background-image:none;
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 1px 2px rgba(16, 22, 26, 0.2);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 1px 2px rgba(16, 22, 26, 0.2);
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 1px 1px rgba(16, 22, 26, 0.1);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 1px 1px rgba(16, 22, 26, 0.1);
    cursor:-webkit-grabbing;
    cursor:grabbing; }
  .bp3-disabled .bp3-slider-handle{
    background:#bfccd6;
    -webkit-box-shadow:none;
            box-shadow:none;
    pointer-events:none; }
  .bp3-dark .bp3-slider-handle{
    background-color:#394b59;
    background-image:-webkit-gradient(linear, left top, left bottom, from(rgba(255, 255, 255, 0.05)), to(rgba(255, 255, 255, 0)));
    background-image:linear-gradient(to bottom, rgba(255, 255, 255, 0.05), rgba(255, 255, 255, 0));
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4);
    color:#f5f8fa; }
    .bp3-dark .bp3-slider-handle:hover, .bp3-dark .bp3-slider-handle:active, .bp3-dark .bp3-slider-handle.bp3-active{
      color:#f5f8fa; }
    .bp3-dark .bp3-slider-handle:hover{
      background-color:#30404d;
      -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4);
              box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4); }
    .bp3-dark .bp3-slider-handle:active, .bp3-dark .bp3-slider-handle.bp3-active{
      background-color:#202b33;
      background-image:none;
      -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.6), inset 0 1px 2px rgba(16, 22, 26, 0.2);
              box-shadow:0 0 0 1px rgba(16, 22, 26, 0.6), inset 0 1px 2px rgba(16, 22, 26, 0.2); }
    .bp3-dark .bp3-slider-handle:disabled, .bp3-dark .bp3-slider-handle.bp3-disabled{
      background-color:rgba(57, 75, 89, 0.5);
      background-image:none;
      -webkit-box-shadow:none;
              box-shadow:none;
      color:rgba(167, 182, 194, 0.6); }
      .bp3-dark .bp3-slider-handle:disabled.bp3-active, .bp3-dark .bp3-slider-handle.bp3-disabled.bp3-active{
        background:rgba(57, 75, 89, 0.7); }
    .bp3-dark .bp3-slider-handle .bp3-button-spinner .bp3-spinner-head{
      background:rgba(16, 22, 26, 0.5);
      stroke:#8a9ba8; }
    .bp3-dark .bp3-slider-handle, .bp3-dark .bp3-slider-handle:hover{
      background-color:#394b59; }
    .bp3-dark .bp3-slider-handle.bp3-active{
      background-color:#293742; }
  .bp3-dark .bp3-disabled .bp3-slider-handle{
    background:#5c7080;
    border-color:#5c7080;
    -webkit-box-shadow:none;
            box-shadow:none; }
  .bp3-slider-handle .bp3-slider-label{
    background:#394b59;
    border-radius:3px;
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 2px 4px rgba(16, 22, 26, 0.2), 0 8px 24px rgba(16, 22, 26, 0.2);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 2px 4px rgba(16, 22, 26, 0.2), 0 8px 24px rgba(16, 22, 26, 0.2);
    color:#f5f8fa;
    margin-left:8px; }
    .bp3-dark .bp3-slider-handle .bp3-slider-label{
      background:#e1e8ed;
      -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 2px 4px rgba(16, 22, 26, 0.4), 0 8px 24px rgba(16, 22, 26, 0.4);
              box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 2px 4px rgba(16, 22, 26, 0.4), 0 8px 24px rgba(16, 22, 26, 0.4);
      color:#394b59; }
    .bp3-disabled .bp3-slider-handle .bp3-slider-label{
      -webkit-box-shadow:none;
              box-shadow:none; }
  .bp3-slider-handle.bp3-start, .bp3-slider-handle.bp3-end{
    width:8px; }
  .bp3-slider-handle.bp3-start{
    border-bottom-right-radius:0;
    border-top-right-radius:0; }
  .bp3-slider-handle.bp3-end{
    border-bottom-left-radius:0;
    border-top-left-radius:0;
    margin-left:8px; }
    .bp3-slider-handle.bp3-end .bp3-slider-label{
      margin-left:0; }

.bp3-slider-label{
  -webkit-transform:translate(-50%, 20px);
          transform:translate(-50%, 20px);
  display:inline-block;
  font-size:12px;
  line-height:1;
  padding:2px 5px;
  position:absolute;
  vertical-align:top; }

.bp3-slider.bp3-vertical{
  height:150px;
  min-width:40px;
  width:40px; }
  .bp3-slider.bp3-vertical .bp3-slider-track,
  .bp3-slider.bp3-vertical .bp3-slider-progress{
    bottom:0;
    height:auto;
    left:5px;
    top:0;
    width:6px; }
  .bp3-slider.bp3-vertical .bp3-slider-progress{
    top:auto; }
  .bp3-slider.bp3-vertical .bp3-slider-label{
    -webkit-transform:translate(20px, 50%);
            transform:translate(20px, 50%); }
  .bp3-slider.bp3-vertical .bp3-slider-handle{
    top:auto; }
    .bp3-slider.bp3-vertical .bp3-slider-handle .bp3-slider-label{
      margin-left:0;
      margin-top:-8px; }
    .bp3-slider.bp3-vertical .bp3-slider-handle.bp3-end, .bp3-slider.bp3-vertical .bp3-slider-handle.bp3-start{
      height:8px;
      margin-left:0;
      width:16px; }
    .bp3-slider.bp3-vertical .bp3-slider-handle.bp3-start{
      border-bottom-right-radius:3px;
      border-top-left-radius:0; }
      .bp3-slider.bp3-vertical .bp3-slider-handle.bp3-start .bp3-slider-label{
        -webkit-transform:translate(20px);
                transform:translate(20px); }
    .bp3-slider.bp3-vertical .bp3-slider-handle.bp3-end{
      border-bottom-left-radius:0;
      border-bottom-right-radius:0;
      border-top-left-radius:3px;
      margin-bottom:8px; }

@-webkit-keyframes pt-spinner-animation{
  from{
    -webkit-transform:rotate(0deg);
            transform:rotate(0deg); }
  to{
    -webkit-transform:rotate(360deg);
            transform:rotate(360deg); } }

@keyframes pt-spinner-animation{
  from{
    -webkit-transform:rotate(0deg);
            transform:rotate(0deg); }
  to{
    -webkit-transform:rotate(360deg);
            transform:rotate(360deg); } }

.bp3-spinner{
  -webkit-box-align:center;
      -ms-flex-align:center;
          align-items:center;
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  -webkit-box-pack:center;
      -ms-flex-pack:center;
          justify-content:center;
  overflow:visible;
  vertical-align:middle; }
  .bp3-spinner svg{
    display:block; }
  .bp3-spinner path{
    fill-opacity:0; }
  .bp3-spinner .bp3-spinner-head{
    stroke:rgba(92, 112, 128, 0.8);
    stroke-linecap:round;
    -webkit-transform-origin:center;
            transform-origin:center;
    -webkit-transition:stroke-dashoffset 200ms cubic-bezier(0.4, 1, 0.75, 0.9);
    transition:stroke-dashoffset 200ms cubic-bezier(0.4, 1, 0.75, 0.9); }
  .bp3-spinner .bp3-spinner-track{
    stroke:rgba(92, 112, 128, 0.2); }

.bp3-spinner-animation{
  -webkit-animation:pt-spinner-animation 500ms linear infinite;
          animation:pt-spinner-animation 500ms linear infinite; }
  .bp3-no-spin > .bp3-spinner-animation{
    -webkit-animation:none;
            animation:none; }

.bp3-dark .bp3-spinner .bp3-spinner-head{
  stroke:#8a9ba8; }

.bp3-dark .bp3-spinner .bp3-spinner-track{
  stroke:rgba(16, 22, 26, 0.5); }

.bp3-spinner.bp3-intent-primary .bp3-spinner-head{
  stroke:#137cbd; }

.bp3-spinner.bp3-intent-success .bp3-spinner-head{
  stroke:#0f9960; }

.bp3-spinner.bp3-intent-warning .bp3-spinner-head{
  stroke:#d9822b; }

.bp3-spinner.bp3-intent-danger .bp3-spinner-head{
  stroke:#db3737; }
.bp3-tabs.bp3-vertical{
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex; }
  .bp3-tabs.bp3-vertical > .bp3-tab-list{
    -webkit-box-align:start;
        -ms-flex-align:start;
            align-items:flex-start;
    -webkit-box-orient:vertical;
    -webkit-box-direction:normal;
        -ms-flex-direction:column;
            flex-direction:column; }
    .bp3-tabs.bp3-vertical > .bp3-tab-list .bp3-tab{
      border-radius:3px;
      padding:0 10px;
      width:100%; }
      .bp3-tabs.bp3-vertical > .bp3-tab-list .bp3-tab[aria-selected="true"]{
        background-color:rgba(19, 124, 189, 0.2);
        -webkit-box-shadow:none;
                box-shadow:none; }
    .bp3-tabs.bp3-vertical > .bp3-tab-list .bp3-tab-indicator-wrapper .bp3-tab-indicator{
      background-color:rgba(19, 124, 189, 0.2);
      border-radius:3px;
      bottom:0;
      height:auto;
      left:0;
      right:0;
      top:0; }
  .bp3-tabs.bp3-vertical > .bp3-tab-panel{
    margin-top:0;
    padding-left:20px; }

.bp3-tab-list{
  -webkit-box-align:end;
      -ms-flex-align:end;
          align-items:flex-end;
  border:none;
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  -webkit-box-flex:0;
      -ms-flex:0 0 auto;
          flex:0 0 auto;
  list-style:none;
  margin:0;
  padding:0;
  position:relative; }
  .bp3-tab-list > *:not(:last-child){
    margin-right:20px; }

.bp3-tab{
  overflow:hidden;
  text-overflow:ellipsis;
  white-space:nowrap;
  word-wrap:normal;
  color:#182026;
  cursor:pointer;
  -webkit-box-flex:0;
      -ms-flex:0 0 auto;
          flex:0 0 auto;
  font-size:14px;
  line-height:30px;
  max-width:100%;
  position:relative;
  vertical-align:top; }
  .bp3-tab a{
    color:inherit;
    display:block;
    text-decoration:none; }
  .bp3-tab-indicator-wrapper ~ .bp3-tab{
    background-color:transparent !important;
    -webkit-box-shadow:none !important;
            box-shadow:none !important; }
  .bp3-tab[aria-disabled="true"]{
    color:rgba(92, 112, 128, 0.6);
    cursor:not-allowed; }
  .bp3-tab[aria-selected="true"]{
    border-radius:0;
    -webkit-box-shadow:inset 0 -3px 0 #106ba3;
            box-shadow:inset 0 -3px 0 #106ba3; }
  .bp3-tab[aria-selected="true"], .bp3-tab:not([aria-disabled="true"]):hover{
    color:#106ba3; }
  .bp3-tab:focus{
    -moz-outline-radius:0; }
  .bp3-large > .bp3-tab{
    font-size:16px;
    line-height:40px; }

.bp3-tab-panel{
  margin-top:20px; }
  .bp3-tab-panel[aria-hidden="true"]{
    display:none; }

.bp3-tab-indicator-wrapper{
  left:0;
  pointer-events:none;
  position:absolute;
  top:0;
  -webkit-transform:translateX(0), translateY(0);
          transform:translateX(0), translateY(0);
  -webkit-transition:height, width, -webkit-transform;
  transition:height, width, -webkit-transform;
  transition:height, transform, width;
  transition:height, transform, width, -webkit-transform;
  -webkit-transition-duration:200ms;
          transition-duration:200ms;
  -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
          transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
  .bp3-tab-indicator-wrapper .bp3-tab-indicator{
    background-color:#106ba3;
    bottom:0;
    height:3px;
    left:0;
    position:absolute;
    right:0; }
  .bp3-tab-indicator-wrapper.bp3-no-animation{
    -webkit-transition:none;
    transition:none; }

.bp3-dark .bp3-tab{
  color:#f5f8fa; }
  .bp3-dark .bp3-tab[aria-disabled="true"]{
    color:rgba(167, 182, 194, 0.6); }
  .bp3-dark .bp3-tab[aria-selected="true"]{
    -webkit-box-shadow:inset 0 -3px 0 #48aff0;
            box-shadow:inset 0 -3px 0 #48aff0; }
  .bp3-dark .bp3-tab[aria-selected="true"], .bp3-dark .bp3-tab:not([aria-disabled="true"]):hover{
    color:#48aff0; }

.bp3-dark .bp3-tab-indicator{
  background-color:#48aff0; }

.bp3-flex-expander{
  -webkit-box-flex:1;
      -ms-flex:1 1;
          flex:1 1; }
.bp3-tag{
  display:-webkit-inline-box;
  display:-ms-inline-flexbox;
  display:inline-flex;
  -webkit-box-orient:horizontal;
  -webkit-box-direction:normal;
      -ms-flex-direction:row;
          flex-direction:row;
  -webkit-box-align:center;
      -ms-flex-align:center;
          align-items:center;
  background-color:#5c7080;
  border:none;
  border-radius:3px;
  -webkit-box-shadow:none;
          box-shadow:none;
  color:#f5f8fa;
  font-size:12px;
  line-height:16px;
  max-width:100%;
  min-height:20px;
  min-width:20px;
  padding:2px 6px;
  position:relative; }
  .bp3-tag.bp3-interactive{
    cursor:pointer; }
    .bp3-tag.bp3-interactive:hover{
      background-color:rgba(92, 112, 128, 0.85); }
    .bp3-tag.bp3-interactive.bp3-active, .bp3-tag.bp3-interactive:active{
      background-color:rgba(92, 112, 128, 0.7); }
  .bp3-tag > *{
    -webkit-box-flex:0;
        -ms-flex-positive:0;
            flex-grow:0;
    -ms-flex-negative:0;
        flex-shrink:0; }
  .bp3-tag > .bp3-fill{
    -webkit-box-flex:1;
        -ms-flex-positive:1;
            flex-grow:1;
    -ms-flex-negative:1;
        flex-shrink:1; }
  .bp3-tag::before,
  .bp3-tag > *{
    margin-right:4px; }
  .bp3-tag:empty::before,
  .bp3-tag > :last-child{
    margin-right:0; }
  .bp3-tag:focus{
    outline:rgba(19, 124, 189, 0.6) auto 2px;
    outline-offset:0;
    -moz-outline-radius:6px; }
  .bp3-tag.bp3-round{
    border-radius:30px;
    padding-left:8px;
    padding-right:8px; }
  .bp3-dark .bp3-tag{
    background-color:#bfccd6;
    color:#182026; }
    .bp3-dark .bp3-tag.bp3-interactive{
      cursor:pointer; }
      .bp3-dark .bp3-tag.bp3-interactive:hover{
        background-color:rgba(191, 204, 214, 0.85); }
      .bp3-dark .bp3-tag.bp3-interactive.bp3-active, .bp3-dark .bp3-tag.bp3-interactive:active{
        background-color:rgba(191, 204, 214, 0.7); }
    .bp3-dark .bp3-tag > .bp3-icon, .bp3-dark .bp3-tag .bp3-icon-standard, .bp3-dark .bp3-tag .bp3-icon-large{
      fill:currentColor; }
  .bp3-tag > .bp3-icon, .bp3-tag .bp3-icon-standard, .bp3-tag .bp3-icon-large{
    fill:#ffffff; }
  .bp3-tag.bp3-large,
  .bp3-large .bp3-tag{
    font-size:14px;
    line-height:20px;
    min-height:30px;
    min-width:30px;
    padding:5px 10px; }
    .bp3-tag.bp3-large::before,
    .bp3-tag.bp3-large > *,
    .bp3-large .bp3-tag::before,
    .bp3-large .bp3-tag > *{
      margin-right:7px; }
    .bp3-tag.bp3-large:empty::before,
    .bp3-tag.bp3-large > :last-child,
    .bp3-large .bp3-tag:empty::before,
    .bp3-large .bp3-tag > :last-child{
      margin-right:0; }
    .bp3-tag.bp3-large.bp3-round,
    .bp3-large .bp3-tag.bp3-round{
      padding-left:12px;
      padding-right:12px; }
  .bp3-tag.bp3-intent-primary{
    background:#137cbd;
    color:#ffffff; }
    .bp3-tag.bp3-intent-primary.bp3-interactive{
      cursor:pointer; }
      .bp3-tag.bp3-intent-primary.bp3-interactive:hover{
        background-color:rgba(19, 124, 189, 0.85); }
      .bp3-tag.bp3-intent-primary.bp3-interactive.bp3-active, .bp3-tag.bp3-intent-primary.bp3-interactive:active{
        background-color:rgba(19, 124, 189, 0.7); }
  .bp3-tag.bp3-intent-success{
    background:#0f9960;
    color:#ffffff; }
    .bp3-tag.bp3-intent-success.bp3-interactive{
      cursor:pointer; }
      .bp3-tag.bp3-intent-success.bp3-interactive:hover{
        background-color:rgba(15, 153, 96, 0.85); }
      .bp3-tag.bp3-intent-success.bp3-interactive.bp3-active, .bp3-tag.bp3-intent-success.bp3-interactive:active{
        background-color:rgba(15, 153, 96, 0.7); }
  .bp3-tag.bp3-intent-warning{
    background:#d9822b;
    color:#ffffff; }
    .bp3-tag.bp3-intent-warning.bp3-interactive{
      cursor:pointer; }
      .bp3-tag.bp3-intent-warning.bp3-interactive:hover{
        background-color:rgba(217, 130, 43, 0.85); }
      .bp3-tag.bp3-intent-warning.bp3-interactive.bp3-active, .bp3-tag.bp3-intent-warning.bp3-interactive:active{
        background-color:rgba(217, 130, 43, 0.7); }
  .bp3-tag.bp3-intent-danger{
    background:#db3737;
    color:#ffffff; }
    .bp3-tag.bp3-intent-danger.bp3-interactive{
      cursor:pointer; }
      .bp3-tag.bp3-intent-danger.bp3-interactive:hover{
        background-color:rgba(219, 55, 55, 0.85); }
      .bp3-tag.bp3-intent-danger.bp3-interactive.bp3-active, .bp3-tag.bp3-intent-danger.bp3-interactive:active{
        background-color:rgba(219, 55, 55, 0.7); }
  .bp3-tag.bp3-fill{
    display:-webkit-box;
    display:-ms-flexbox;
    display:flex;
    width:100%; }
  .bp3-tag.bp3-minimal > .bp3-icon, .bp3-tag.bp3-minimal .bp3-icon-standard, .bp3-tag.bp3-minimal .bp3-icon-large{
    fill:#5c7080; }
  .bp3-tag.bp3-minimal:not([class*="bp3-intent-"]){
    background-color:rgba(138, 155, 168, 0.2);
    color:#182026; }
    .bp3-tag.bp3-minimal:not([class*="bp3-intent-"]).bp3-interactive{
      cursor:pointer; }
      .bp3-tag.bp3-minimal:not([class*="bp3-intent-"]).bp3-interactive:hover{
        background-color:rgba(92, 112, 128, 0.3); }
      .bp3-tag.bp3-minimal:not([class*="bp3-intent-"]).bp3-interactive.bp3-active, .bp3-tag.bp3-minimal:not([class*="bp3-intent-"]).bp3-interactive:active{
        background-color:rgba(92, 112, 128, 0.4); }
    .bp3-dark .bp3-tag.bp3-minimal:not([class*="bp3-intent-"]){
      color:#f5f8fa; }
      .bp3-dark .bp3-tag.bp3-minimal:not([class*="bp3-intent-"]).bp3-interactive{
        cursor:pointer; }
        .bp3-dark .bp3-tag.bp3-minimal:not([class*="bp3-intent-"]).bp3-interactive:hover{
          background-color:rgba(191, 204, 214, 0.3); }
        .bp3-dark .bp3-tag.bp3-minimal:not([class*="bp3-intent-"]).bp3-interactive.bp3-active, .bp3-dark .bp3-tag.bp3-minimal:not([class*="bp3-intent-"]).bp3-interactive:active{
          background-color:rgba(191, 204, 214, 0.4); }
      .bp3-dark .bp3-tag.bp3-minimal:not([class*="bp3-intent-"]) > .bp3-icon, .bp3-dark .bp3-tag.bp3-minimal:not([class*="bp3-intent-"]) .bp3-icon-standard, .bp3-dark .bp3-tag.bp3-minimal:not([class*="bp3-intent-"]) .bp3-icon-large{
        fill:#a7b6c2; }
  .bp3-tag.bp3-minimal.bp3-intent-primary{
    background-color:rgba(19, 124, 189, 0.15);
    color:#106ba3; }
    .bp3-tag.bp3-minimal.bp3-intent-primary.bp3-interactive{
      cursor:pointer; }
      .bp3-tag.bp3-minimal.bp3-intent-primary.bp3-interactive:hover{
        background-color:rgba(19, 124, 189, 0.25); }
      .bp3-tag.bp3-minimal.bp3-intent-primary.bp3-interactive.bp3-active, .bp3-tag.bp3-minimal.bp3-intent-primary.bp3-interactive:active{
        background-color:rgba(19, 124, 189, 0.35); }
    .bp3-tag.bp3-minimal.bp3-intent-primary > .bp3-icon, .bp3-tag.bp3-minimal.bp3-intent-primary .bp3-icon-standard, .bp3-tag.bp3-minimal.bp3-intent-primary .bp3-icon-large{
      fill:#137cbd; }
    .bp3-dark .bp3-tag.bp3-minimal.bp3-intent-primary{
      background-color:rgba(19, 124, 189, 0.25);
      color:#48aff0; }
      .bp3-dark .bp3-tag.bp3-minimal.bp3-intent-primary.bp3-interactive{
        cursor:pointer; }
        .bp3-dark .bp3-tag.bp3-minimal.bp3-intent-primary.bp3-interactive:hover{
          background-color:rgba(19, 124, 189, 0.35); }
        .bp3-dark .bp3-tag.bp3-minimal.bp3-intent-primary.bp3-interactive.bp3-active, .bp3-dark .bp3-tag.bp3-minimal.bp3-intent-primary.bp3-interactive:active{
          background-color:rgba(19, 124, 189, 0.45); }
  .bp3-tag.bp3-minimal.bp3-intent-success{
    background-color:rgba(15, 153, 96, 0.15);
    color:#0d8050; }
    .bp3-tag.bp3-minimal.bp3-intent-success.bp3-interactive{
      cursor:pointer; }
      .bp3-tag.bp3-minimal.bp3-intent-success.bp3-interactive:hover{
        background-color:rgba(15, 153, 96, 0.25); }
      .bp3-tag.bp3-minimal.bp3-intent-success.bp3-interactive.bp3-active, .bp3-tag.bp3-minimal.bp3-intent-success.bp3-interactive:active{
        background-color:rgba(15, 153, 96, 0.35); }
    .bp3-tag.bp3-minimal.bp3-intent-success > .bp3-icon, .bp3-tag.bp3-minimal.bp3-intent-success .bp3-icon-standard, .bp3-tag.bp3-minimal.bp3-intent-success .bp3-icon-large{
      fill:#0f9960; }
    .bp3-dark .bp3-tag.bp3-minimal.bp3-intent-success{
      background-color:rgba(15, 153, 96, 0.25);
      color:#3dcc91; }
      .bp3-dark .bp3-tag.bp3-minimal.bp3-intent-success.bp3-interactive{
        cursor:pointer; }
        .bp3-dark .bp3-tag.bp3-minimal.bp3-intent-success.bp3-interactive:hover{
          background-color:rgba(15, 153, 96, 0.35); }
        .bp3-dark .bp3-tag.bp3-minimal.bp3-intent-success.bp3-interactive.bp3-active, .bp3-dark .bp3-tag.bp3-minimal.bp3-intent-success.bp3-interactive:active{
          background-color:rgba(15, 153, 96, 0.45); }
  .bp3-tag.bp3-minimal.bp3-intent-warning{
    background-color:rgba(217, 130, 43, 0.15);
    color:#bf7326; }
    .bp3-tag.bp3-minimal.bp3-intent-warning.bp3-interactive{
      cursor:pointer; }
      .bp3-tag.bp3-minimal.bp3-intent-warning.bp3-interactive:hover{
        background-color:rgba(217, 130, 43, 0.25); }
      .bp3-tag.bp3-minimal.bp3-intent-warning.bp3-interactive.bp3-active, .bp3-tag.bp3-minimal.bp3-intent-warning.bp3-interactive:active{
        background-color:rgba(217, 130, 43, 0.35); }
    .bp3-tag.bp3-minimal.bp3-intent-warning > .bp3-icon, .bp3-tag.bp3-minimal.bp3-intent-warning .bp3-icon-standard, .bp3-tag.bp3-minimal.bp3-intent-warning .bp3-icon-large{
      fill:#d9822b; }
    .bp3-dark .bp3-tag.bp3-minimal.bp3-intent-warning{
      background-color:rgba(217, 130, 43, 0.25);
      color:#ffb366; }
      .bp3-dark .bp3-tag.bp3-minimal.bp3-intent-warning.bp3-interactive{
        cursor:pointer; }
        .bp3-dark .bp3-tag.bp3-minimal.bp3-intent-warning.bp3-interactive:hover{
          background-color:rgba(217, 130, 43, 0.35); }
        .bp3-dark .bp3-tag.bp3-minimal.bp3-intent-warning.bp3-interactive.bp3-active, .bp3-dark .bp3-tag.bp3-minimal.bp3-intent-warning.bp3-interactive:active{
          background-color:rgba(217, 130, 43, 0.45); }
  .bp3-tag.bp3-minimal.bp3-intent-danger{
    background-color:rgba(219, 55, 55, 0.15);
    color:#c23030; }
    .bp3-tag.bp3-minimal.bp3-intent-danger.bp3-interactive{
      cursor:pointer; }
      .bp3-tag.bp3-minimal.bp3-intent-danger.bp3-interactive:hover{
        background-color:rgba(219, 55, 55, 0.25); }
      .bp3-tag.bp3-minimal.bp3-intent-danger.bp3-interactive.bp3-active, .bp3-tag.bp3-minimal.bp3-intent-danger.bp3-interactive:active{
        background-color:rgba(219, 55, 55, 0.35); }
    .bp3-tag.bp3-minimal.bp3-intent-danger > .bp3-icon, .bp3-tag.bp3-minimal.bp3-intent-danger .bp3-icon-standard, .bp3-tag.bp3-minimal.bp3-intent-danger .bp3-icon-large{
      fill:#db3737; }
    .bp3-dark .bp3-tag.bp3-minimal.bp3-intent-danger{
      background-color:rgba(219, 55, 55, 0.25);
      color:#ff7373; }
      .bp3-dark .bp3-tag.bp3-minimal.bp3-intent-danger.bp3-interactive{
        cursor:pointer; }
        .bp3-dark .bp3-tag.bp3-minimal.bp3-intent-danger.bp3-interactive:hover{
          background-color:rgba(219, 55, 55, 0.35); }
        .bp3-dark .bp3-tag.bp3-minimal.bp3-intent-danger.bp3-interactive.bp3-active, .bp3-dark .bp3-tag.bp3-minimal.bp3-intent-danger.bp3-interactive:active{
          background-color:rgba(219, 55, 55, 0.45); }

.bp3-tag-remove{
  background:none;
  border:none;
  color:inherit;
  cursor:pointer;
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  margin-bottom:-2px;
  margin-right:-6px !important;
  margin-top:-2px;
  opacity:0.5;
  padding:2px;
  padding-left:0; }
  .bp3-tag-remove:hover{
    background:none;
    opacity:0.8;
    text-decoration:none; }
  .bp3-tag-remove:active{
    opacity:1; }
  .bp3-tag-remove:empty::before{
    font-family:"Icons16", sans-serif;
    font-size:16px;
    font-style:normal;
    font-weight:400;
    line-height:1;
    -moz-osx-font-smoothing:grayscale;
    -webkit-font-smoothing:antialiased;
    content:""; }
  .bp3-large .bp3-tag-remove{
    margin-right:-10px !important;
    padding:0 5px 0 0; }
    .bp3-large .bp3-tag-remove:empty::before{
      font-family:"Icons20", sans-serif;
      font-size:20px;
      font-style:normal;
      font-weight:400;
      line-height:1; }
.bp3-tag-input{
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  -webkit-box-orient:horizontal;
  -webkit-box-direction:normal;
      -ms-flex-direction:row;
          flex-direction:row;
  -webkit-box-align:start;
      -ms-flex-align:start;
          align-items:flex-start;
  cursor:text;
  height:auto;
  line-height:inherit;
  min-height:30px;
  padding-left:5px;
  padding-right:0; }
  .bp3-tag-input > *{
    -webkit-box-flex:0;
        -ms-flex-positive:0;
            flex-grow:0;
    -ms-flex-negative:0;
        flex-shrink:0; }
  .bp3-tag-input > .bp3-tag-input-values{
    -webkit-box-flex:1;
        -ms-flex-positive:1;
            flex-grow:1;
    -ms-flex-negative:1;
        flex-shrink:1; }
  .bp3-tag-input .bp3-tag-input-icon{
    color:#5c7080;
    margin-left:2px;
    margin-right:7px;
    margin-top:7px; }
  .bp3-tag-input .bp3-tag-input-values{
    display:-webkit-box;
    display:-ms-flexbox;
    display:flex;
    -webkit-box-orient:horizontal;
    -webkit-box-direction:normal;
        -ms-flex-direction:row;
            flex-direction:row;
    -webkit-box-align:center;
        -ms-flex-align:center;
            align-items:center;
    -ms-flex-item-align:stretch;
        align-self:stretch;
    -ms-flex-wrap:wrap;
        flex-wrap:wrap;
    margin-right:7px;
    margin-top:5px;
    min-width:0; }
    .bp3-tag-input .bp3-tag-input-values > *{
      -webkit-box-flex:0;
          -ms-flex-positive:0;
              flex-grow:0;
      -ms-flex-negative:0;
          flex-shrink:0; }
    .bp3-tag-input .bp3-tag-input-values > .bp3-fill{
      -webkit-box-flex:1;
          -ms-flex-positive:1;
              flex-grow:1;
      -ms-flex-negative:1;
          flex-shrink:1; }
    .bp3-tag-input .bp3-tag-input-values::before,
    .bp3-tag-input .bp3-tag-input-values > *{
      margin-right:5px; }
    .bp3-tag-input .bp3-tag-input-values:empty::before,
    .bp3-tag-input .bp3-tag-input-values > :last-child{
      margin-right:0; }
    .bp3-tag-input .bp3-tag-input-values:first-child .bp3-input-ghost:first-child{
      padding-left:5px; }
    .bp3-tag-input .bp3-tag-input-values > *{
      margin-bottom:5px; }
  .bp3-tag-input .bp3-tag{
    overflow-wrap:break-word; }
    .bp3-tag-input .bp3-tag.bp3-active{
      outline:rgba(19, 124, 189, 0.6) auto 2px;
      outline-offset:0;
      -moz-outline-radius:6px; }
  .bp3-tag-input .bp3-input-ghost{
    -webkit-box-flex:1;
        -ms-flex:1 1 auto;
            flex:1 1 auto;
    line-height:20px;
    width:80px; }
    .bp3-tag-input .bp3-input-ghost:disabled, .bp3-tag-input .bp3-input-ghost.bp3-disabled{
      cursor:not-allowed; }
  .bp3-tag-input .bp3-button,
  .bp3-tag-input .bp3-spinner{
    margin:3px;
    margin-left:0; }
  .bp3-tag-input .bp3-button{
    min-height:24px;
    min-width:24px;
    padding:0 7px; }
  .bp3-tag-input.bp3-large{
    height:auto;
    min-height:40px; }
    .bp3-tag-input.bp3-large::before,
    .bp3-tag-input.bp3-large > *{
      margin-right:10px; }
    .bp3-tag-input.bp3-large:empty::before,
    .bp3-tag-input.bp3-large > :last-child{
      margin-right:0; }
    .bp3-tag-input.bp3-large .bp3-tag-input-icon{
      margin-left:5px;
      margin-top:10px; }
    .bp3-tag-input.bp3-large .bp3-input-ghost{
      line-height:30px; }
    .bp3-tag-input.bp3-large .bp3-button{
      min-height:30px;
      min-width:30px;
      padding:5px 10px;
      margin:5px;
      margin-left:0; }
    .bp3-tag-input.bp3-large .bp3-spinner{
      margin:8px;
      margin-left:0; }
  .bp3-tag-input.bp3-active{
    background-color:#ffffff;
    -webkit-box-shadow:0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2);
            box-shadow:0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
    .bp3-tag-input.bp3-active.bp3-intent-primary{
      -webkit-box-shadow:0 0 0 1px #106ba3, 0 0 0 3px rgba(16, 107, 163, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2);
              box-shadow:0 0 0 1px #106ba3, 0 0 0 3px rgba(16, 107, 163, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
    .bp3-tag-input.bp3-active.bp3-intent-success{
      -webkit-box-shadow:0 0 0 1px #0d8050, 0 0 0 3px rgba(13, 128, 80, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2);
              box-shadow:0 0 0 1px #0d8050, 0 0 0 3px rgba(13, 128, 80, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
    .bp3-tag-input.bp3-active.bp3-intent-warning{
      -webkit-box-shadow:0 0 0 1px #bf7326, 0 0 0 3px rgba(191, 115, 38, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2);
              box-shadow:0 0 0 1px #bf7326, 0 0 0 3px rgba(191, 115, 38, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
    .bp3-tag-input.bp3-active.bp3-intent-danger{
      -webkit-box-shadow:0 0 0 1px #c23030, 0 0 0 3px rgba(194, 48, 48, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2);
              box-shadow:0 0 0 1px #c23030, 0 0 0 3px rgba(194, 48, 48, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
  .bp3-dark .bp3-tag-input .bp3-tag-input-icon, .bp3-tag-input.bp3-dark .bp3-tag-input-icon{
    color:#a7b6c2; }
  .bp3-dark .bp3-tag-input .bp3-input-ghost, .bp3-tag-input.bp3-dark .bp3-input-ghost{
    color:#f5f8fa; }
    .bp3-dark .bp3-tag-input .bp3-input-ghost::-webkit-input-placeholder, .bp3-tag-input.bp3-dark .bp3-input-ghost::-webkit-input-placeholder{
      color:rgba(167, 182, 194, 0.6); }
    .bp3-dark .bp3-tag-input .bp3-input-ghost::-moz-placeholder, .bp3-tag-input.bp3-dark .bp3-input-ghost::-moz-placeholder{
      color:rgba(167, 182, 194, 0.6); }
    .bp3-dark .bp3-tag-input .bp3-input-ghost:-ms-input-placeholder, .bp3-tag-input.bp3-dark .bp3-input-ghost:-ms-input-placeholder{
      color:rgba(167, 182, 194, 0.6); }
    .bp3-dark .bp3-tag-input .bp3-input-ghost::-ms-input-placeholder, .bp3-tag-input.bp3-dark .bp3-input-ghost::-ms-input-placeholder{
      color:rgba(167, 182, 194, 0.6); }
    .bp3-dark .bp3-tag-input .bp3-input-ghost::placeholder, .bp3-tag-input.bp3-dark .bp3-input-ghost::placeholder{
      color:rgba(167, 182, 194, 0.6); }
  .bp3-dark .bp3-tag-input.bp3-active, .bp3-tag-input.bp3-dark.bp3-active{
    background-color:rgba(16, 22, 26, 0.3);
    -webkit-box-shadow:0 0 0 1px #137cbd, 0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px #137cbd, 0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4); }
    .bp3-dark .bp3-tag-input.bp3-active.bp3-intent-primary, .bp3-tag-input.bp3-dark.bp3-active.bp3-intent-primary{
      -webkit-box-shadow:0 0 0 1px #106ba3, 0 0 0 3px rgba(16, 107, 163, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
              box-shadow:0 0 0 1px #106ba3, 0 0 0 3px rgba(16, 107, 163, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4); }
    .bp3-dark .bp3-tag-input.bp3-active.bp3-intent-success, .bp3-tag-input.bp3-dark.bp3-active.bp3-intent-success{
      -webkit-box-shadow:0 0 0 1px #0d8050, 0 0 0 3px rgba(13, 128, 80, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
              box-shadow:0 0 0 1px #0d8050, 0 0 0 3px rgba(13, 128, 80, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4); }
    .bp3-dark .bp3-tag-input.bp3-active.bp3-intent-warning, .bp3-tag-input.bp3-dark.bp3-active.bp3-intent-warning{
      -webkit-box-shadow:0 0 0 1px #bf7326, 0 0 0 3px rgba(191, 115, 38, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
              box-shadow:0 0 0 1px #bf7326, 0 0 0 3px rgba(191, 115, 38, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4); }
    .bp3-dark .bp3-tag-input.bp3-active.bp3-intent-danger, .bp3-tag-input.bp3-dark.bp3-active.bp3-intent-danger{
      -webkit-box-shadow:0 0 0 1px #c23030, 0 0 0 3px rgba(194, 48, 48, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
              box-shadow:0 0 0 1px #c23030, 0 0 0 3px rgba(194, 48, 48, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4); }

.bp3-input-ghost{
  background:none;
  border:none;
  -webkit-box-shadow:none;
          box-shadow:none;
  padding:0; }
  .bp3-input-ghost::-webkit-input-placeholder{
    color:rgba(92, 112, 128, 0.6);
    opacity:1; }
  .bp3-input-ghost::-moz-placeholder{
    color:rgba(92, 112, 128, 0.6);
    opacity:1; }
  .bp3-input-ghost:-ms-input-placeholder{
    color:rgba(92, 112, 128, 0.6);
    opacity:1; }
  .bp3-input-ghost::-ms-input-placeholder{
    color:rgba(92, 112, 128, 0.6);
    opacity:1; }
  .bp3-input-ghost::placeholder{
    color:rgba(92, 112, 128, 0.6);
    opacity:1; }
  .bp3-input-ghost:focus{
    outline:none !important; }
.bp3-toast{
  -webkit-box-align:start;
      -ms-flex-align:start;
          align-items:flex-start;
  background-color:#ffffff;
  border-radius:3px;
  -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 2px 4px rgba(16, 22, 26, 0.2), 0 8px 24px rgba(16, 22, 26, 0.2);
          box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 2px 4px rgba(16, 22, 26, 0.2), 0 8px 24px rgba(16, 22, 26, 0.2);
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  margin:20px 0 0;
  max-width:500px;
  min-width:300px;
  pointer-events:all;
  position:relative !important; }
  .bp3-toast.bp3-toast-enter, .bp3-toast.bp3-toast-appear{
    -webkit-transform:translateY(-40px);
            transform:translateY(-40px); }
  .bp3-toast.bp3-toast-enter-active, .bp3-toast.bp3-toast-appear-active{
    -webkit-transform:translateY(0);
            transform:translateY(0);
    -webkit-transition-delay:0;
            transition-delay:0;
    -webkit-transition-duration:300ms;
            transition-duration:300ms;
    -webkit-transition-property:-webkit-transform;
    transition-property:-webkit-transform;
    transition-property:transform;
    transition-property:transform, -webkit-transform;
    -webkit-transition-timing-function:cubic-bezier(0.54, 1.12, 0.38, 1.11);
            transition-timing-function:cubic-bezier(0.54, 1.12, 0.38, 1.11); }
  .bp3-toast.bp3-toast-enter ~ .bp3-toast, .bp3-toast.bp3-toast-appear ~ .bp3-toast{
    -webkit-transform:translateY(-40px);
            transform:translateY(-40px); }
  .bp3-toast.bp3-toast-enter-active ~ .bp3-toast, .bp3-toast.bp3-toast-appear-active ~ .bp3-toast{
    -webkit-transform:translateY(0);
            transform:translateY(0);
    -webkit-transition-delay:0;
            transition-delay:0;
    -webkit-transition-duration:300ms;
            transition-duration:300ms;
    -webkit-transition-property:-webkit-transform;
    transition-property:-webkit-transform;
    transition-property:transform;
    transition-property:transform, -webkit-transform;
    -webkit-transition-timing-function:cubic-bezier(0.54, 1.12, 0.38, 1.11);
            transition-timing-function:cubic-bezier(0.54, 1.12, 0.38, 1.11); }
  .bp3-toast.bp3-toast-exit{
    opacity:1;
    -webkit-filter:blur(0);
            filter:blur(0); }
  .bp3-toast.bp3-toast-exit-active{
    opacity:0;
    -webkit-filter:blur(10px);
            filter:blur(10px);
    -webkit-transition-delay:0;
            transition-delay:0;
    -webkit-transition-duration:300ms;
            transition-duration:300ms;
    -webkit-transition-property:opacity, -webkit-filter;
    transition-property:opacity, -webkit-filter;
    transition-property:opacity, filter;
    transition-property:opacity, filter, -webkit-filter;
    -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
            transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
  .bp3-toast.bp3-toast-exit ~ .bp3-toast{
    -webkit-transform:translateY(0);
            transform:translateY(0); }
  .bp3-toast.bp3-toast-exit-active ~ .bp3-toast{
    -webkit-transform:translateY(-40px);
            transform:translateY(-40px);
    -webkit-transition-delay:50ms;
            transition-delay:50ms;
    -webkit-transition-duration:100ms;
            transition-duration:100ms;
    -webkit-transition-property:-webkit-transform;
    transition-property:-webkit-transform;
    transition-property:transform;
    transition-property:transform, -webkit-transform;
    -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
            transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
  .bp3-toast .bp3-button-group{
    -webkit-box-flex:0;
        -ms-flex:0 0 auto;
            flex:0 0 auto;
    padding:5px;
    padding-left:0; }
  .bp3-toast > .bp3-icon{
    color:#5c7080;
    margin:12px;
    margin-right:0; }
  .bp3-toast.bp3-dark,
  .bp3-dark .bp3-toast{
    background-color:#394b59;
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 2px 4px rgba(16, 22, 26, 0.4), 0 8px 24px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 2px 4px rgba(16, 22, 26, 0.4), 0 8px 24px rgba(16, 22, 26, 0.4); }
    .bp3-toast.bp3-dark > .bp3-icon,
    .bp3-dark .bp3-toast > .bp3-icon{
      color:#a7b6c2; }
  .bp3-toast[class*="bp3-intent-"] a{
    color:rgba(255, 255, 255, 0.7); }
    .bp3-toast[class*="bp3-intent-"] a:hover{
      color:#ffffff; }
  .bp3-toast[class*="bp3-intent-"] > .bp3-icon{
    color:#ffffff; }
  .bp3-toast[class*="bp3-intent-"] .bp3-button, .bp3-toast[class*="bp3-intent-"] .bp3-button::before,
  .bp3-toast[class*="bp3-intent-"] .bp3-button .bp3-icon, .bp3-toast[class*="bp3-intent-"] .bp3-button:active{
    color:rgba(255, 255, 255, 0.7) !important; }
  .bp3-toast[class*="bp3-intent-"] .bp3-button:focus{
    outline-color:rgba(255, 255, 255, 0.5); }
  .bp3-toast[class*="bp3-intent-"] .bp3-button:hover{
    background-color:rgba(255, 255, 255, 0.15) !important;
    color:#ffffff !important; }
  .bp3-toast[class*="bp3-intent-"] .bp3-button:active{
    background-color:rgba(255, 255, 255, 0.3) !important;
    color:#ffffff !important; }
  .bp3-toast[class*="bp3-intent-"] .bp3-button::after{
    background:rgba(255, 255, 255, 0.3) !important; }
  .bp3-toast.bp3-intent-primary{
    background-color:#137cbd;
    color:#ffffff; }
  .bp3-toast.bp3-intent-success{
    background-color:#0f9960;
    color:#ffffff; }
  .bp3-toast.bp3-intent-warning{
    background-color:#d9822b;
    color:#ffffff; }
  .bp3-toast.bp3-intent-danger{
    background-color:#db3737;
    color:#ffffff; }

.bp3-toast-message{
  -webkit-box-flex:1;
      -ms-flex:1 1 auto;
          flex:1 1 auto;
  padding:11px;
  word-break:break-word; }

.bp3-toast-container{
  -webkit-box-align:center;
      -ms-flex-align:center;
          align-items:center;
  display:-webkit-box !important;
  display:-ms-flexbox !important;
  display:flex !important;
  -webkit-box-orient:vertical;
  -webkit-box-direction:normal;
      -ms-flex-direction:column;
          flex-direction:column;
  left:0;
  overflow:hidden;
  padding:0 20px 20px;
  pointer-events:none;
  right:0;
  z-index:40; }
  .bp3-toast-container.bp3-toast-container-in-portal{
    position:fixed; }
  .bp3-toast-container.bp3-toast-container-inline{
    position:absolute; }
  .bp3-toast-container.bp3-toast-container-top{
    top:0; }
  .bp3-toast-container.bp3-toast-container-bottom{
    bottom:0;
    -webkit-box-orient:vertical;
    -webkit-box-direction:reverse;
        -ms-flex-direction:column-reverse;
            flex-direction:column-reverse;
    top:auto; }
  .bp3-toast-container.bp3-toast-container-left{
    -webkit-box-align:start;
        -ms-flex-align:start;
            align-items:flex-start; }
  .bp3-toast-container.bp3-toast-container-right{
    -webkit-box-align:end;
        -ms-flex-align:end;
            align-items:flex-end; }

.bp3-toast-container-bottom .bp3-toast.bp3-toast-enter:not(.bp3-toast-enter-active),
.bp3-toast-container-bottom .bp3-toast.bp3-toast-enter:not(.bp3-toast-enter-active) ~ .bp3-toast, .bp3-toast-container-bottom .bp3-toast.bp3-toast-appear:not(.bp3-toast-appear-active),
.bp3-toast-container-bottom .bp3-toast.bp3-toast-appear:not(.bp3-toast-appear-active) ~ .bp3-toast,
.bp3-toast-container-bottom .bp3-toast.bp3-toast-exit-active ~ .bp3-toast,
.bp3-toast-container-bottom .bp3-toast.bp3-toast-leave-active ~ .bp3-toast{
  -webkit-transform:translateY(60px);
          transform:translateY(60px); }
.bp3-tooltip{
  -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 2px 4px rgba(16, 22, 26, 0.2), 0 8px 24px rgba(16, 22, 26, 0.2);
          box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 2px 4px rgba(16, 22, 26, 0.2), 0 8px 24px rgba(16, 22, 26, 0.2);
  -webkit-transform:scale(1);
          transform:scale(1); }
  .bp3-tooltip .bp3-popover-arrow{
    height:22px;
    position:absolute;
    width:22px; }
    .bp3-tooltip .bp3-popover-arrow::before{
      height:14px;
      margin:4px;
      width:14px; }
  .bp3-tether-element-attached-bottom.bp3-tether-target-attached-top > .bp3-tooltip{
    margin-bottom:11px;
    margin-top:-11px; }
    .bp3-tether-element-attached-bottom.bp3-tether-target-attached-top > .bp3-tooltip > .bp3-popover-arrow{
      bottom:-8px; }
      .bp3-tether-element-attached-bottom.bp3-tether-target-attached-top > .bp3-tooltip > .bp3-popover-arrow svg{
        -webkit-transform:rotate(-90deg);
                transform:rotate(-90deg); }
  .bp3-tether-element-attached-left.bp3-tether-target-attached-right > .bp3-tooltip{
    margin-left:11px; }
    .bp3-tether-element-attached-left.bp3-tether-target-attached-right > .bp3-tooltip > .bp3-popover-arrow{
      left:-8px; }
      .bp3-tether-element-attached-left.bp3-tether-target-attached-right > .bp3-tooltip > .bp3-popover-arrow svg{
        -webkit-transform:rotate(0);
                transform:rotate(0); }
  .bp3-tether-element-attached-top.bp3-tether-target-attached-bottom > .bp3-tooltip{
    margin-top:11px; }
    .bp3-tether-element-attached-top.bp3-tether-target-attached-bottom > .bp3-tooltip > .bp3-popover-arrow{
      top:-8px; }
      .bp3-tether-element-attached-top.bp3-tether-target-attached-bottom > .bp3-tooltip > .bp3-popover-arrow svg{
        -webkit-transform:rotate(90deg);
                transform:rotate(90deg); }
  .bp3-tether-element-attached-right.bp3-tether-target-attached-left > .bp3-tooltip{
    margin-left:-11px;
    margin-right:11px; }
    .bp3-tether-element-attached-right.bp3-tether-target-attached-left > .bp3-tooltip > .bp3-popover-arrow{
      right:-8px; }
      .bp3-tether-element-attached-right.bp3-tether-target-attached-left > .bp3-tooltip > .bp3-popover-arrow svg{
        -webkit-transform:rotate(180deg);
                transform:rotate(180deg); }
  .bp3-tether-element-attached-middle > .bp3-tooltip > .bp3-popover-arrow{
    top:50%;
    -webkit-transform:translateY(-50%);
            transform:translateY(-50%); }
  .bp3-tether-element-attached-center > .bp3-tooltip > .bp3-popover-arrow{
    right:50%;
    -webkit-transform:translateX(50%);
            transform:translateX(50%); }
  .bp3-tether-element-attached-top.bp3-tether-target-attached-top > .bp3-tooltip > .bp3-popover-arrow{
    top:-0.22183px; }
  .bp3-tether-element-attached-right.bp3-tether-target-attached-right > .bp3-tooltip > .bp3-popover-arrow{
    right:-0.22183px; }
  .bp3-tether-element-attached-left.bp3-tether-target-attached-left > .bp3-tooltip > .bp3-popover-arrow{
    left:-0.22183px; }
  .bp3-tether-element-attached-bottom.bp3-tether-target-attached-bottom > .bp3-tooltip > .bp3-popover-arrow{
    bottom:-0.22183px; }
  .bp3-tether-element-attached-top.bp3-tether-element-attached-left > .bp3-tooltip{
    -webkit-transform-origin:top left;
            transform-origin:top left; }
  .bp3-tether-element-attached-top.bp3-tether-element-attached-center > .bp3-tooltip{
    -webkit-transform-origin:top center;
            transform-origin:top center; }
  .bp3-tether-element-attached-top.bp3-tether-element-attached-right > .bp3-tooltip{
    -webkit-transform-origin:top right;
            transform-origin:top right; }
  .bp3-tether-element-attached-middle.bp3-tether-element-attached-left > .bp3-tooltip{
    -webkit-transform-origin:center left;
            transform-origin:center left; }
  .bp3-tether-element-attached-middle.bp3-tether-element-attached-center > .bp3-tooltip{
    -webkit-transform-origin:center center;
            transform-origin:center center; }
  .bp3-tether-element-attached-middle.bp3-tether-element-attached-right > .bp3-tooltip{
    -webkit-transform-origin:center right;
            transform-origin:center right; }
  .bp3-tether-element-attached-bottom.bp3-tether-element-attached-left > .bp3-tooltip{
    -webkit-transform-origin:bottom left;
            transform-origin:bottom left; }
  .bp3-tether-element-attached-bottom.bp3-tether-element-attached-center > .bp3-tooltip{
    -webkit-transform-origin:bottom center;
            transform-origin:bottom center; }
  .bp3-tether-element-attached-bottom.bp3-tether-element-attached-right > .bp3-tooltip{
    -webkit-transform-origin:bottom right;
            transform-origin:bottom right; }
  .bp3-tooltip .bp3-popover-content{
    background:#394b59;
    color:#f5f8fa; }
  .bp3-tooltip .bp3-popover-arrow::before{
    -webkit-box-shadow:1px 1px 6px rgba(16, 22, 26, 0.2);
            box-shadow:1px 1px 6px rgba(16, 22, 26, 0.2); }
  .bp3-tooltip .bp3-popover-arrow-border{
    fill:#10161a;
    fill-opacity:0.1; }
  .bp3-tooltip .bp3-popover-arrow-fill{
    fill:#394b59; }
  .bp3-popover-enter > .bp3-tooltip, .bp3-popover-appear > .bp3-tooltip{
    -webkit-transform:scale(0.8);
            transform:scale(0.8); }
  .bp3-popover-enter-active > .bp3-tooltip, .bp3-popover-appear-active > .bp3-tooltip{
    -webkit-transform:scale(1);
            transform:scale(1);
    -webkit-transition-delay:0;
            transition-delay:0;
    -webkit-transition-duration:100ms;
            transition-duration:100ms;
    -webkit-transition-property:-webkit-transform;
    transition-property:-webkit-transform;
    transition-property:transform;
    transition-property:transform, -webkit-transform;
    -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
            transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
  .bp3-popover-exit > .bp3-tooltip{
    -webkit-transform:scale(1);
            transform:scale(1); }
  .bp3-popover-exit-active > .bp3-tooltip{
    -webkit-transform:scale(0.8);
            transform:scale(0.8);
    -webkit-transition-delay:0;
            transition-delay:0;
    -webkit-transition-duration:100ms;
            transition-duration:100ms;
    -webkit-transition-property:-webkit-transform;
    transition-property:-webkit-transform;
    transition-property:transform;
    transition-property:transform, -webkit-transform;
    -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
            transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
  .bp3-tooltip .bp3-popover-content{
    padding:10px 12px; }
  .bp3-tooltip.bp3-dark,
  .bp3-dark .bp3-tooltip{
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 2px 4px rgba(16, 22, 26, 0.4), 0 8px 24px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 2px 4px rgba(16, 22, 26, 0.4), 0 8px 24px rgba(16, 22, 26, 0.4); }
    .bp3-tooltip.bp3-dark .bp3-popover-content,
    .bp3-dark .bp3-tooltip .bp3-popover-content{
      background:#e1e8ed;
      color:#394b59; }
    .bp3-tooltip.bp3-dark .bp3-popover-arrow::before,
    .bp3-dark .bp3-tooltip .bp3-popover-arrow::before{
      -webkit-box-shadow:1px 1px 6px rgba(16, 22, 26, 0.4);
              box-shadow:1px 1px 6px rgba(16, 22, 26, 0.4); }
    .bp3-tooltip.bp3-dark .bp3-popover-arrow-border,
    .bp3-dark .bp3-tooltip .bp3-popover-arrow-border{
      fill:#10161a;
      fill-opacity:0.2; }
    .bp3-tooltip.bp3-dark .bp3-popover-arrow-fill,
    .bp3-dark .bp3-tooltip .bp3-popover-arrow-fill{
      fill:#e1e8ed; }
  .bp3-tooltip.bp3-intent-primary .bp3-popover-content{
    background:#137cbd;
    color:#ffffff; }
  .bp3-tooltip.bp3-intent-primary .bp3-popover-arrow-fill{
    fill:#137cbd; }
  .bp3-tooltip.bp3-intent-success .bp3-popover-content{
    background:#0f9960;
    color:#ffffff; }
  .bp3-tooltip.bp3-intent-success .bp3-popover-arrow-fill{
    fill:#0f9960; }
  .bp3-tooltip.bp3-intent-warning .bp3-popover-content{
    background:#d9822b;
    color:#ffffff; }
  .bp3-tooltip.bp3-intent-warning .bp3-popover-arrow-fill{
    fill:#d9822b; }
  .bp3-tooltip.bp3-intent-danger .bp3-popover-content{
    background:#db3737;
    color:#ffffff; }
  .bp3-tooltip.bp3-intent-danger .bp3-popover-arrow-fill{
    fill:#db3737; }

.bp3-tooltip-indicator{
  border-bottom:dotted 1px;
  cursor:help; }
.bp3-tree .bp3-icon, .bp3-tree .bp3-icon-standard, .bp3-tree .bp3-icon-large{
  color:#5c7080; }
  .bp3-tree .bp3-icon.bp3-intent-primary, .bp3-tree .bp3-icon-standard.bp3-intent-primary, .bp3-tree .bp3-icon-large.bp3-intent-primary{
    color:#137cbd; }
  .bp3-tree .bp3-icon.bp3-intent-success, .bp3-tree .bp3-icon-standard.bp3-intent-success, .bp3-tree .bp3-icon-large.bp3-intent-success{
    color:#0f9960; }
  .bp3-tree .bp3-icon.bp3-intent-warning, .bp3-tree .bp3-icon-standard.bp3-intent-warning, .bp3-tree .bp3-icon-large.bp3-intent-warning{
    color:#d9822b; }
  .bp3-tree .bp3-icon.bp3-intent-danger, .bp3-tree .bp3-icon-standard.bp3-intent-danger, .bp3-tree .bp3-icon-large.bp3-intent-danger{
    color:#db3737; }

.bp3-tree-node-list{
  list-style:none;
  margin:0;
  padding-left:0; }

.bp3-tree-root{
  background-color:transparent;
  cursor:default;
  padding-left:0;
  position:relative; }

.bp3-tree-node-content-0{
  padding-left:0px; }

.bp3-tree-node-content-1{
  padding-left:23px; }

.bp3-tree-node-content-2{
  padding-left:46px; }

.bp3-tree-node-content-3{
  padding-left:69px; }

.bp3-tree-node-content-4{
  padding-left:92px; }

.bp3-tree-node-content-5{
  padding-left:115px; }

.bp3-tree-node-content-6{
  padding-left:138px; }

.bp3-tree-node-content-7{
  padding-left:161px; }

.bp3-tree-node-content-8{
  padding-left:184px; }

.bp3-tree-node-content-9{
  padding-left:207px; }

.bp3-tree-node-content-10{
  padding-left:230px; }

.bp3-tree-node-content-11{
  padding-left:253px; }

.bp3-tree-node-content-12{
  padding-left:276px; }

.bp3-tree-node-content-13{
  padding-left:299px; }

.bp3-tree-node-content-14{
  padding-left:322px; }

.bp3-tree-node-content-15{
  padding-left:345px; }

.bp3-tree-node-content-16{
  padding-left:368px; }

.bp3-tree-node-content-17{
  padding-left:391px; }

.bp3-tree-node-content-18{
  padding-left:414px; }

.bp3-tree-node-content-19{
  padding-left:437px; }

.bp3-tree-node-content-20{
  padding-left:460px; }

.bp3-tree-node-content{
  -webkit-box-align:center;
      -ms-flex-align:center;
          align-items:center;
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  height:30px;
  padding-right:5px;
  width:100%; }
  .bp3-tree-node-content:hover{
    background-color:rgba(191, 204, 214, 0.4); }

.bp3-tree-node-caret,
.bp3-tree-node-caret-none{
  min-width:30px; }

.bp3-tree-node-caret{
  color:#5c7080;
  cursor:pointer;
  padding:7px;
  -webkit-transform:rotate(0deg);
          transform:rotate(0deg);
  -webkit-transition:-webkit-transform 200ms cubic-bezier(0.4, 1, 0.75, 0.9);
  transition:-webkit-transform 200ms cubic-bezier(0.4, 1, 0.75, 0.9);
  transition:transform 200ms cubic-bezier(0.4, 1, 0.75, 0.9);
  transition:transform 200ms cubic-bezier(0.4, 1, 0.75, 0.9), -webkit-transform 200ms cubic-bezier(0.4, 1, 0.75, 0.9); }
  .bp3-tree-node-caret:hover{
    color:#182026; }
  .bp3-dark .bp3-tree-node-caret{
    color:#a7b6c2; }
    .bp3-dark .bp3-tree-node-caret:hover{
      color:#f5f8fa; }
  .bp3-tree-node-caret.bp3-tree-node-caret-open{
    -webkit-transform:rotate(90deg);
            transform:rotate(90deg); }
  .bp3-tree-node-caret.bp3-icon-standard::before{
    content:""; }

.bp3-tree-node-icon{
  margin-right:7px;
  position:relative; }

.bp3-tree-node-label{
  overflow:hidden;
  text-overflow:ellipsis;
  white-space:nowrap;
  word-wrap:normal;
  -webkit-box-flex:1;
      -ms-flex:1 1 auto;
          flex:1 1 auto;
  position:relative;
  -webkit-user-select:none;
     -moz-user-select:none;
      -ms-user-select:none;
          user-select:none; }
  .bp3-tree-node-label span{
    display:inline; }

.bp3-tree-node-secondary-label{
  padding:0 5px;
  -webkit-user-select:none;
     -moz-user-select:none;
      -ms-user-select:none;
          user-select:none; }
  .bp3-tree-node-secondary-label .bp3-popover-wrapper,
  .bp3-tree-node-secondary-label .bp3-popover-target{
    -webkit-box-align:center;
        -ms-flex-align:center;
            align-items:center;
    display:-webkit-box;
    display:-ms-flexbox;
    display:flex; }

.bp3-tree-node.bp3-disabled .bp3-tree-node-content{
  background-color:inherit;
  color:rgba(92, 112, 128, 0.6);
  cursor:not-allowed; }

.bp3-tree-node.bp3-disabled .bp3-tree-node-caret,
.bp3-tree-node.bp3-disabled .bp3-tree-node-icon{
  color:rgba(92, 112, 128, 0.6);
  cursor:not-allowed; }

.bp3-tree-node.bp3-tree-node-selected > .bp3-tree-node-content{
  background-color:#137cbd; }
  .bp3-tree-node.bp3-tree-node-selected > .bp3-tree-node-content,
  .bp3-tree-node.bp3-tree-node-selected > .bp3-tree-node-content .bp3-icon, .bp3-tree-node.bp3-tree-node-selected > .bp3-tree-node-content .bp3-icon-standard, .bp3-tree-node.bp3-tree-node-selected > .bp3-tree-node-content .bp3-icon-large{
    color:#ffffff; }
  .bp3-tree-node.bp3-tree-node-selected > .bp3-tree-node-content .bp3-tree-node-caret::before{
    color:rgba(255, 255, 255, 0.7); }
  .bp3-tree-node.bp3-tree-node-selected > .bp3-tree-node-content .bp3-tree-node-caret:hover::before{
    color:#ffffff; }

.bp3-dark .bp3-tree-node-content:hover{
  background-color:rgba(92, 112, 128, 0.3); }

.bp3-dark .bp3-tree .bp3-icon, .bp3-dark .bp3-tree .bp3-icon-standard, .bp3-dark .bp3-tree .bp3-icon-large{
  color:#a7b6c2; }
  .bp3-dark .bp3-tree .bp3-icon.bp3-intent-primary, .bp3-dark .bp3-tree .bp3-icon-standard.bp3-intent-primary, .bp3-dark .bp3-tree .bp3-icon-large.bp3-intent-primary{
    color:#137cbd; }
  .bp3-dark .bp3-tree .bp3-icon.bp3-intent-success, .bp3-dark .bp3-tree .bp3-icon-standard.bp3-intent-success, .bp3-dark .bp3-tree .bp3-icon-large.bp3-intent-success{
    color:#0f9960; }
  .bp3-dark .bp3-tree .bp3-icon.bp3-intent-warning, .bp3-dark .bp3-tree .bp3-icon-standard.bp3-intent-warning, .bp3-dark .bp3-tree .bp3-icon-large.bp3-intent-warning{
    color:#d9822b; }
  .bp3-dark .bp3-tree .bp3-icon.bp3-intent-danger, .bp3-dark .bp3-tree .bp3-icon-standard.bp3-intent-danger, .bp3-dark .bp3-tree .bp3-icon-large.bp3-intent-danger{
    color:#db3737; }

.bp3-dark .bp3-tree-node.bp3-tree-node-selected > .bp3-tree-node-content{
  background-color:#137cbd; }
.bp3-omnibar{
  -webkit-filter:blur(0);
          filter:blur(0);
  opacity:1;
  background-color:#ffffff;
  border-radius:3px;
  -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 4px 8px rgba(16, 22, 26, 0.2), 0 18px 46px 6px rgba(16, 22, 26, 0.2);
          box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 4px 8px rgba(16, 22, 26, 0.2), 0 18px 46px 6px rgba(16, 22, 26, 0.2);
  left:calc(50% - 250px);
  top:20vh;
  width:500px;
  z-index:21; }
  .bp3-omnibar.bp3-overlay-enter, .bp3-omnibar.bp3-overlay-appear{
    -webkit-filter:blur(20px);
            filter:blur(20px);
    opacity:0.2; }
  .bp3-omnibar.bp3-overlay-enter-active, .bp3-omnibar.bp3-overlay-appear-active{
    -webkit-filter:blur(0);
            filter:blur(0);
    opacity:1;
    -webkit-transition-delay:0;
            transition-delay:0;
    -webkit-transition-duration:200ms;
            transition-duration:200ms;
    -webkit-transition-property:opacity, -webkit-filter;
    transition-property:opacity, -webkit-filter;
    transition-property:filter, opacity;
    transition-property:filter, opacity, -webkit-filter;
    -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
            transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
  .bp3-omnibar.bp3-overlay-exit{
    -webkit-filter:blur(0);
            filter:blur(0);
    opacity:1; }
  .bp3-omnibar.bp3-overlay-exit-active{
    -webkit-filter:blur(20px);
            filter:blur(20px);
    opacity:0.2;
    -webkit-transition-delay:0;
            transition-delay:0;
    -webkit-transition-duration:200ms;
            transition-duration:200ms;
    -webkit-transition-property:opacity, -webkit-filter;
    transition-property:opacity, -webkit-filter;
    transition-property:filter, opacity;
    transition-property:filter, opacity, -webkit-filter;
    -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
            transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
  .bp3-omnibar .bp3-input{
    background-color:transparent;
    border-radius:0; }
    .bp3-omnibar .bp3-input, .bp3-omnibar .bp3-input:focus{
      -webkit-box-shadow:none;
              box-shadow:none; }
  .bp3-omnibar .bp3-menu{
    background-color:transparent;
    border-radius:0;
    -webkit-box-shadow:inset 0 1px 0 rgba(16, 22, 26, 0.15);
            box-shadow:inset 0 1px 0 rgba(16, 22, 26, 0.15);
    max-height:calc(60vh - 40px);
    overflow:auto; }
    .bp3-omnibar .bp3-menu:empty{
      display:none; }
  .bp3-dark .bp3-omnibar, .bp3-omnibar.bp3-dark{
    background-color:#30404d;
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 4px 8px rgba(16, 22, 26, 0.4), 0 18px 46px 6px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 4px 8px rgba(16, 22, 26, 0.4), 0 18px 46px 6px rgba(16, 22, 26, 0.4); }

.bp3-omnibar-overlay .bp3-overlay-backdrop{
  background-color:rgba(16, 22, 26, 0.2); }

.bp3-select-popover .bp3-popover-content{
  padding:5px; }

.bp3-select-popover .bp3-input-group{
  margin-bottom:0; }

.bp3-select-popover .bp3-menu{
  max-height:300px;
  max-width:400px;
  overflow:auto;
  padding:0; }
  .bp3-select-popover .bp3-menu:not(:first-child){
    padding-top:5px; }

.bp3-multi-select{
  min-width:150px; }

.bp3-multi-select-popover .bp3-menu{
  max-height:300px;
  max-width:400px;
  overflow:auto; }

.bp3-select-popover .bp3-popover-content{
  padding:5px; }

.bp3-select-popover .bp3-input-group{
  margin-bottom:0; }

.bp3-select-popover .bp3-menu{
  max-height:300px;
  max-width:400px;
  overflow:auto;
  padding:0; }
  .bp3-select-popover .bp3-menu:not(:first-child){
    padding-top:5px; }
/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

/* This file was auto-generated by ensureUiComponents() in @jupyterlab/buildutils */

/**
 * (DEPRECATED) Support for consuming icons as CSS background images
 */

/* Icons urls */

:root {
  --jp-icon-add: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDI0IDI0Ij4KICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiPgogICAgPHBhdGggZD0iTTE5IDEzaC02djZoLTJ2LTZINXYtMmg2VjVoMnY2aDZ2MnoiLz4KICA8L2c+Cjwvc3ZnPgo=);
  --jp-icon-bug: url(data:image/svg+xml;base64,PHN2ZyB2aWV3Qm94PSIwIDAgMjQgMjQiIHdpZHRoPSIxNiIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KICA8ZyBjbGFzcz0ianAtaWNvbjMganAtaWNvbi1zZWxlY3RhYmxlIiBmaWxsPSIjNjE2MTYxIj4KICAgIDxwYXRoIGQ9Ik0yMCA4aC0yLjgxYy0uNDUtLjc4LTEuMDctMS40NS0xLjgyLTEuOTZMMTcgNC40MSAxNS41OSAzbC0yLjE3IDIuMTdDMTIuOTYgNS4wNiAxMi40OSA1IDEyIDVjLS40OSAwLS45Ni4wNi0xLjQxLjE3TDguNDEgMyA3IDQuNDFsMS42MiAxLjYzQzcuODggNi41NSA3LjI2IDcuMjIgNi44MSA4SDR2MmgyLjA5Yy0uMDUuMzMtLjA5LjY2LS4wOSAxdjFINHYyaDJ2MWMwIC4zNC4wNC42Ny4wOSAxSDR2MmgyLjgxYzEuMDQgMS43OSAyLjk3IDMgNS4xOSAzczQuMTUtMS4yMSA1LjE5LTNIMjB2LTJoLTIuMDljLjA1LS4zMy4wOS0uNjYuMDktMXYtMWgydi0yaC0ydi0xYzAtLjM0LS4wNC0uNjctLjA5LTFIMjBWOHptLTYgOGgtNHYtMmg0djJ6bTAtNGgtNHYtMmg0djJ6Ii8+CiAgPC9nPgo8L3N2Zz4K);
  --jp-icon-build: url(data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMTYiIHZpZXdCb3g9IjAgMCAyNCAyNCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiPgogICAgPHBhdGggZD0iTTE0LjkgMTcuNDVDMTYuMjUgMTcuNDUgMTcuMzUgMTYuMzUgMTcuMzUgMTVDMTcuMzUgMTMuNjUgMTYuMjUgMTIuNTUgMTQuOSAxMi41NUMxMy41NCAxMi41NSAxMi40NSAxMy42NSAxMi40NSAxNUMxMi40NSAxNi4zNSAxMy41NCAxNy40NSAxNC45IDE3LjQ1Wk0yMC4xIDE1LjY4TDIxLjU4IDE2Ljg0QzIxLjcxIDE2Ljk1IDIxLjc1IDE3LjEzIDIxLjY2IDE3LjI5TDIwLjI2IDE5LjcxQzIwLjE3IDE5Ljg2IDIwIDE5LjkyIDE5LjgzIDE5Ljg2TDE4LjA5IDE5LjE2QzE3LjczIDE5LjQ0IDE3LjMzIDE5LjY3IDE2LjkxIDE5Ljg1TDE2LjY0IDIxLjdDMTYuNjIgMjEuODcgMTYuNDcgMjIgMTYuMyAyMkgxMy41QzEzLjMyIDIyIDEzLjE4IDIxLjg3IDEzLjE1IDIxLjdMMTIuODkgMTkuODVDMTIuNDYgMTkuNjcgMTIuMDcgMTkuNDQgMTEuNzEgMTkuMTZMOS45NjAwMiAxOS44NkM5LjgxMDAyIDE5LjkyIDkuNjIwMDIgMTkuODYgOS41NDAwMiAxOS43MUw4LjE0MDAyIDE3LjI5QzguMDUwMDIgMTcuMTMgOC4wOTAwMiAxNi45NSA4LjIyMDAyIDE2Ljg0TDkuNzAwMDIgMTUuNjhMOS42NTAwMSAxNUw5LjcwMDAyIDE0LjMxTDguMjIwMDIgMTMuMTZDOC4wOTAwMiAxMy4wNSA4LjA1MDAyIDEyLjg2IDguMTQwMDIgMTIuNzFMOS41NDAwMiAxMC4yOUM5LjYyMDAyIDEwLjEzIDkuODEwMDIgMTAuMDcgOS45NjAwMiAxMC4xM0wxMS43MSAxMC44NEMxMi4wNyAxMC41NiAxMi40NiAxMC4zMiAxMi44OSAxMC4xNUwxMy4xNSA4LjI4OTk4QzEzLjE4IDguMTI5OTggMTMuMzIgNy45OTk5OCAxMy41IDcuOTk5OThIMTYuM0MxNi40NyA3Ljk5OTk4IDE2LjYyIDguMTI5OTggMTYuNjQgOC4yODk5OEwxNi45MSAxMC4xNUMxNy4zMyAxMC4zMiAxNy43MyAxMC41NiAxOC4wOSAxMC44NEwxOS44MyAxMC4xM0MyMCAxMC4wNyAyMC4xNyAxMC4xMyAyMC4yNiAxMC4yOUwyMS42NiAxMi43MUMyMS43NSAxMi44NiAyMS43MSAxMy4wNSAyMS41OCAxMy4xNkwyMC4xIDE0LjMxTDIwLjE1IDE1TDIwLjEgMTUuNjhaIi8+CiAgICA8cGF0aCBkPSJNNy4zMjk2NiA3LjQ0NDU0QzguMDgzMSA3LjAwOTU0IDguMzM5MzIgNi4wNTMzMiA3LjkwNDMyIDUuMjk5ODhDNy40NjkzMiA0LjU0NjQzIDYuNTA4MSA0LjI4MTU2IDUuNzU0NjYgNC43MTY1NkM1LjM5MTc2IDQuOTI2MDggNS4xMjY5NSA1LjI3MTE4IDUuMDE4NDkgNS42NzU5NEM0LjkxMDA0IDYuMDgwNzEgNC45NjY4MiA2LjUxMTk4IDUuMTc2MzQgNi44NzQ4OEM1LjYxMTM0IDcuNjI4MzIgNi41NzYyMiA3Ljg3OTU0IDcuMzI5NjYgNy40NDQ1NFpNOS42NTcxOCA0Ljc5NTkzTDEwLjg2NzIgNC45NTE3OUMxMC45NjI4IDQuOTc3NDEgMTEuMDQwMiA1LjA3MTMzIDExLjAzODIgNS4xODc5M0wxMS4wMzg4IDYuOTg4OTNDMTEuMDQ1NSA3LjEwMDU0IDEwLjk2MTYgNy4xOTUxOCAxMC44NTUgNy4yMTA1NEw5LjY2MDAxIDcuMzgwODNMOS4yMzkxNSA4LjEzMTg4TDkuNjY5NjEgOS4yNTc0NUM5LjcwNzI5IDkuMzYyNzEgOS42NjkzNCA5LjQ3Njk5IDkuNTc0MDggOS41MzE5OUw4LjAxNTIzIDEwLjQzMkM3LjkxMTMxIDEwLjQ5MiA3Ljc5MzM3IDEwLjQ2NzcgNy43MjEwNSAxMC4zODI0TDYuOTg3NDggOS40MzE4OEw2LjEwOTMxIDkuNDMwODNMNS4zNDcwNCAxMC4zOTA1QzUuMjg5MDkgMTAuNDcwMiA1LjE3MzgzIDEwLjQ5MDUgNS4wNzE4NyAxMC40MzM5TDMuNTEyNDUgOS41MzI5M0MzLjQxMDQ5IDkuNDc2MzMgMy4zNzY0NyA5LjM1NzQxIDMuNDEwNzUgOS4yNTY3OUwzLjg2MzQ3IDguMTQwOTNMMy42MTc0OSA3Ljc3NDg4TDMuNDIzNDcgNy4zNzg4M0wyLjIzMDc1IDcuMjEyOTdDMi4xMjY0NyA3LjE5MjM1IDIuMDQwNDkgNy4xMDM0MiAyLjA0MjQ1IDYuOTg2ODJMMi4wNDE4NyA1LjE4NTgyQzIuMDQzODMgNS4wNjkyMiAyLjExOTA5IDQuOTc5NTggMi4yMTcwNCA0Ljk2OTIyTDMuNDIwNjUgNC43OTM5M0wzLjg2NzQ5IDQuMDI3ODhMMy40MTEwNSAyLjkxNzMxQzMuMzczMzcgMi44MTIwNCAzLjQxMTMxIDIuNjk3NzYgMy41MTUyMyAyLjYzNzc2TDUuMDc0MDggMS43Mzc3NkM1LjE2OTM0IDEuNjgyNzYgNS4yODcyOSAxLjcwNzA0IDUuMzU5NjEgMS43OTIzMUw2LjExOTE1IDIuNzI3ODhMNi45ODAwMSAyLjczODkzTDcuNzI0OTYgMS43ODkyMkM3Ljc5MTU2IDEuNzA0NTggNy45MTU0OCAxLjY3OTIyIDguMDA4NzkgMS43NDA4Mkw5LjU2ODIxIDIuNjQxODJDOS42NzAxNyAyLjY5ODQyIDkuNzEyODUgMi44MTIzNCA5LjY4NzIzIDIuOTA3OTdMOS4yMTcxOCA0LjAzMzgzTDkuNDYzMTYgNC4zOTk4OEw5LjY1NzE4IDQuNzk1OTNaIi8+CiAgPC9nPgo8L3N2Zz4K);
  --jp-icon-caret-down-empty-thin: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDIwIDIwIj4KCTxnIGNsYXNzPSJqcC1pY29uMyIgZmlsbD0iIzYxNjE2MSIgc2hhcGUtcmVuZGVyaW5nPSJnZW9tZXRyaWNQcmVjaXNpb24iPgoJCTxwb2x5Z29uIGNsYXNzPSJzdDEiIHBvaW50cz0iOS45LDEzLjYgMy42LDcuNCA0LjQsNi42IDkuOSwxMi4yIDE1LjQsNi43IDE2LjEsNy40ICIvPgoJPC9nPgo8L3N2Zz4K);
  --jp-icon-caret-down-empty: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDE4IDE4Ij4KICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiIHNoYXBlLXJlbmRlcmluZz0iZ2VvbWV0cmljUHJlY2lzaW9uIj4KICAgIDxwYXRoIGQ9Ik01LjIsNS45TDksOS43bDMuOC0zLjhsMS4yLDEuMmwtNC45LDVsLTQuOS01TDUuMiw1Ljl6Ii8+CiAgPC9nPgo8L3N2Zz4K);
  --jp-icon-caret-down: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDE4IDE4Ij4KICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiIHNoYXBlLXJlbmRlcmluZz0iZ2VvbWV0cmljUHJlY2lzaW9uIj4KICAgIDxwYXRoIGQ9Ik01LjIsNy41TDksMTEuMmwzLjgtMy44SDUuMnoiLz4KICA8L2c+Cjwvc3ZnPgo=);
  --jp-icon-caret-left: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDE4IDE4Ij4KCTxnIGNsYXNzPSJqcC1pY29uMyIgZmlsbD0iIzYxNjE2MSIgc2hhcGUtcmVuZGVyaW5nPSJnZW9tZXRyaWNQcmVjaXNpb24iPgoJCTxwYXRoIGQ9Ik0xMC44LDEyLjhMNy4xLDlsMy44LTMuOGwwLDcuNkgxMC44eiIvPgogIDwvZz4KPC9zdmc+Cg==);
  --jp-icon-caret-right: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDE4IDE4Ij4KICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiIHNoYXBlLXJlbmRlcmluZz0iZ2VvbWV0cmljUHJlY2lzaW9uIj4KICAgIDxwYXRoIGQ9Ik03LjIsNS4yTDEwLjksOWwtMy44LDMuOFY1LjJINy4yeiIvPgogIDwvZz4KPC9zdmc+Cg==);
  --jp-icon-caret-up-empty-thin: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDIwIDIwIj4KCTxnIGNsYXNzPSJqcC1pY29uMyIgZmlsbD0iIzYxNjE2MSIgc2hhcGUtcmVuZGVyaW5nPSJnZW9tZXRyaWNQcmVjaXNpb24iPgoJCTxwb2x5Z29uIGNsYXNzPSJzdDEiIHBvaW50cz0iMTUuNCwxMy4zIDkuOSw3LjcgNC40LDEzLjIgMy42LDEyLjUgOS45LDYuMyAxNi4xLDEyLjYgIi8+Cgk8L2c+Cjwvc3ZnPgo=);
  --jp-icon-caret-up: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDE4IDE4Ij4KCTxnIGNsYXNzPSJqcC1pY29uMyIgZmlsbD0iIzYxNjE2MSIgc2hhcGUtcmVuZGVyaW5nPSJnZW9tZXRyaWNQcmVjaXNpb24iPgoJCTxwYXRoIGQ9Ik01LjIsMTAuNUw5LDYuOGwzLjgsMy44SDUuMnoiLz4KICA8L2c+Cjwvc3ZnPgo=);
  --jp-icon-case-sensitive: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDIwIDIwIj4KICA8ZyBjbGFzcz0ianAtaWNvbjIiIGZpbGw9IiM0MTQxNDEiPgogICAgPHJlY3QgeD0iMiIgeT0iMiIgd2lkdGg9IjE2IiBoZWlnaHQ9IjE2Ii8+CiAgPC9nPgogIDxnIGNsYXNzPSJqcC1pY29uLWFjY2VudDIiIGZpbGw9IiNGRkYiPgogICAgPHBhdGggZD0iTTcuNiw4aDAuOWwzLjUsOGgtMS4xTDEwLDE0SDZsLTAuOSwySDRMNy42LDh6IE04LDkuMUw2LjQsMTNoMy4yTDgsOS4xeiIvPgogICAgPHBhdGggZD0iTTE2LjYsOS44Yy0wLjIsMC4xLTAuNCwwLjEtMC43LDAuMWMtMC4yLDAtMC40LTAuMS0wLjYtMC4yYy0wLjEtMC4xLTAuMi0wLjQtMC4yLTAuNyBjLTAuMywwLjMtMC42LDAuNS0wLjksMC43Yy0wLjMsMC4xLTAuNywwLjItMS4xLDAuMmMtMC4zLDAtMC41LDAtMC43LTAuMWMtMC4yLTAuMS0wLjQtMC4yLTAuNi0wLjNjLTAuMi0wLjEtMC4zLTAuMy0wLjQtMC41IGMtMC4xLTAuMi0wLjEtMC40LTAuMS0wLjdjMC0wLjMsMC4xLTAuNiwwLjItMC44YzAuMS0wLjIsMC4zLTAuNCwwLjQtMC41QzEyLDcsMTIuMiw2LjksMTIuNSw2LjhjMC4yLTAuMSwwLjUtMC4xLDAuNy0wLjIgYzAuMy0wLjEsMC41LTAuMSwwLjctMC4xYzAuMiwwLDAuNC0wLjEsMC42LTAuMWMwLjIsMCwwLjMtMC4xLDAuNC0wLjJjMC4xLTAuMSwwLjItMC4yLDAuMi0wLjRjMC0xLTEuMS0xLTEuMy0xIGMtMC40LDAtMS40LDAtMS40LDEuMmgtMC45YzAtMC40LDAuMS0wLjcsMC4yLTFjMC4xLTAuMiwwLjMtMC40LDAuNS0wLjZjMC4yLTAuMiwwLjUtMC4zLDAuOC0wLjNDMTMuMyw0LDEzLjYsNCwxMy45LDQgYzAuMywwLDAuNSwwLDAuOCwwLjFjMC4zLDAsMC41LDAuMSwwLjcsMC4yYzAuMiwwLjEsMC40LDAuMywwLjUsMC41QzE2LDUsMTYsNS4yLDE2LDUuNnYyLjljMCwwLjIsMCwwLjQsMCwwLjUgYzAsMC4xLDAuMSwwLjIsMC4zLDAuMmMwLjEsMCwwLjIsMCwwLjMsMFY5Ljh6IE0xNS4yLDYuOWMtMS4yLDAuNi0zLjEsMC4yLTMuMSwxLjRjMCwxLjQsMy4xLDEsMy4xLTAuNVY2Ljl6Ii8+CiAgPC9nPgo8L3N2Zz4K);
  --jp-icon-check: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDI0IDI0Ij4KICA8ZyBjbGFzcz0ianAtaWNvbjMganAtaWNvbi1zZWxlY3RhYmxlIiBmaWxsPSIjNjE2MTYxIj4KICAgIDxwYXRoIGQ9Ik05IDE2LjE3TDQuODMgMTJsLTEuNDIgMS40MUw5IDE5IDIxIDdsLTEuNDEtMS40MXoiLz4KICA8L2c+Cjwvc3ZnPgo=);
  --jp-icon-circle-empty: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDI0IDI0Ij4KICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiPgogICAgPHBhdGggZD0iTTEyIDJDNi40NyAyIDIgNi40NyAyIDEyczQuNDcgMTAgMTAgMTAgMTAtNC40NyAxMC0xMFMxNy41MyAyIDEyIDJ6bTAgMThjLTQuNDEgMC04LTMuNTktOC04czMuNTktOCA4LTggOCAzLjU5IDggOC0zLjU5IDgtOCA4eiIvPgogIDwvZz4KPC9zdmc+Cg==);
  --jp-icon-circle: url(data:image/svg+xml;base64,PHN2ZyB2aWV3Qm94PSIwIDAgMTggMTgiIHdpZHRoPSIxNiIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiPgogICAgPGNpcmNsZSBjeD0iOSIgY3k9IjkiIHI9IjgiLz4KICA8L2c+Cjwvc3ZnPgo=);
  --jp-icon-clear: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDI0IDI0Ij4KICA8bWFzayBpZD0iZG9udXRIb2xlIj4KICAgIDxyZWN0IHdpZHRoPSIyNCIgaGVpZ2h0PSIyNCIgZmlsbD0id2hpdGUiIC8+CiAgICA8Y2lyY2xlIGN4PSIxMiIgY3k9IjEyIiByPSI4IiBmaWxsPSJibGFjayIvPgogIDwvbWFzaz4KCiAgPGcgY2xhc3M9ImpwLWljb24zIiBmaWxsPSIjNjE2MTYxIj4KICAgIDxyZWN0IGhlaWdodD0iMTgiIHdpZHRoPSIyIiB4PSIxMSIgeT0iMyIgdHJhbnNmb3JtPSJyb3RhdGUoMzE1LCAxMiwgMTIpIi8+CiAgICA8Y2lyY2xlIGN4PSIxMiIgY3k9IjEyIiByPSIxMCIgbWFzaz0idXJsKCNkb251dEhvbGUpIi8+CiAgPC9nPgo8L3N2Zz4K);
  --jp-icon-close: url(data:image/svg+xml;base64,PHN2ZyB2aWV3Qm94PSIwIDAgMjQgMjQiIHdpZHRoPSIxNiIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KICA8ZyBjbGFzcz0ianAtaWNvbi1ub25lIGpwLWljb24tc2VsZWN0YWJsZS1pbnZlcnNlIGpwLWljb24zLWhvdmVyIiBmaWxsPSJub25lIj4KICAgIDxjaXJjbGUgY3g9IjEyIiBjeT0iMTIiIHI9IjExIi8+CiAgPC9nPgoKICA8ZyBjbGFzcz0ianAtaWNvbjMganAtaWNvbi1zZWxlY3RhYmxlIGpwLWljb24tYWNjZW50Mi1ob3ZlciIgZmlsbD0iIzYxNjE2MSI+CiAgICA8cGF0aCBkPSJNMTkgNi40MUwxNy41OSA1IDEyIDEwLjU5IDYuNDEgNSA1IDYuNDEgMTAuNTkgMTIgNSAxNy41OSA2LjQxIDE5IDEyIDEzLjQxIDE3LjU5IDE5IDE5IDE3LjU5IDEzLjQxIDEyeiIvPgogIDwvZz4KCiAgPGcgY2xhc3M9ImpwLWljb24tbm9uZSBqcC1pY29uLWJ1c3kiIGZpbGw9Im5vbmUiPgogICAgPGNpcmNsZSBjeD0iMTIiIGN5PSIxMiIgcj0iNyIvPgogIDwvZz4KPC9zdmc+Cg==);
  --jp-icon-code: url(data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMjIiIGhlaWdodD0iMjIiIHZpZXdCb3g9IjAgMCAyOCAyOCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KCTxnIGNsYXNzPSJqcC1pY29uMyIgZmlsbD0iIzYxNjE2MSI+CgkJPHBhdGggZD0iTTExLjQgMTguNkw2LjggMTRMMTEuNCA5LjRMMTAgOEw0IDE0TDEwIDIwTDExLjQgMTguNlpNMTYuNiAxOC42TDIxLjIgMTRMMTYuNiA5LjRMMTggOEwyNCAxNEwxOCAyMEwxNi42IDE4LjZWMTguNloiLz4KCTwvZz4KPC9zdmc+Cg==);
  --jp-icon-console: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDIwMCAyMDAiPgogIDxnIGNsYXNzPSJqcC1pY29uLWJyYW5kMSBqcC1pY29uLXNlbGVjdGFibGUiIGZpbGw9IiMwMjg4RDEiPgogICAgPHBhdGggZD0iTTIwIDE5LjhoMTYwdjE1OS45SDIweiIvPgogIDwvZz4KICA8ZyBjbGFzcz0ianAtaWNvbi1zZWxlY3RhYmxlLWludmVyc2UiIGZpbGw9IiNmZmYiPgogICAgPHBhdGggZD0iTTEwNSAxMjcuM2g0MHYxMi44aC00MHpNNTEuMSA3N0w3NCA5OS45bC0yMy4zIDIzLjMgMTAuNSAxMC41IDIzLjMtMjMuM0w5NSA5OS45IDg0LjUgODkuNCA2MS42IDY2LjV6Ii8+CiAgPC9nPgo8L3N2Zz4K);
  --jp-icon-copy: url(data:image/svg+xml;base64,PHN2ZyB2aWV3Qm94PSIwIDAgMTggMTgiIHdpZHRoPSIxNiIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiPgogICAgPHBhdGggZD0iTTExLjksMUgzLjJDMi40LDEsMS43LDEuNywxLjcsMi41djEwLjJoMS41VjIuNWg4LjdWMXogTTE0LjEsMy45aC04Yy0wLjgsMC0xLjUsMC43LTEuNSwxLjV2MTAuMmMwLDAuOCwwLjcsMS41LDEuNSwxLjVoOCBjMC44LDAsMS41LTAuNywxLjUtMS41VjUuNEMxNS41LDQuNiwxNC45LDMuOSwxNC4xLDMuOXogTTE0LjEsMTUuNWgtOFY1LjRoOFYxNS41eiIvPgogIDwvZz4KPC9zdmc+Cg==);
  --jp-icon-copyright: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIGVuYWJsZS1iYWNrZ3JvdW5kPSJuZXcgMCAwIDI0IDI0IiBoZWlnaHQ9IjI0IiB2aWV3Qm94PSIwIDAgMjQgMjQiIHdpZHRoPSIyNCI+CiAgPGcgY2xhc3M9ImpwLWljb24zIiBmaWxsPSIjNjE2MTYxIj4KICAgIDxwYXRoIGQ9Ik0xMS44OCw5LjE0YzEuMjgsMC4wNiwxLjYxLDEuMTUsMS42MywxLjY2aDEuNzljLTAuMDgtMS45OC0xLjQ5LTMuMTktMy40NS0zLjE5QzkuNjQsNy42MSw4LDksOCwxMi4xNCBjMCwxLjk0LDAuOTMsNC4yNCwzLjg0LDQuMjRjMi4yMiwwLDMuNDEtMS42NSwzLjQ0LTIuOTVoLTEuNzljLTAuMDMsMC41OS0wLjQ1LDEuMzgtMS42MywxLjQ0QzEwLjU1LDE0LjgzLDEwLDEzLjgxLDEwLDEyLjE0IEMxMCw5LjI1LDExLjI4LDkuMTYsMTEuODgsOS4xNHogTTEyLDJDNi40OCwyLDIsNi40OCwyLDEyczQuNDgsMTAsMTAsMTBzMTAtNC40OCwxMC0xMFMxNy41MiwyLDEyLDJ6IE0xMiwyMGMtNC40MSwwLTgtMy41OS04LTggczMuNTktOCw4LThzOCwzLjU5LDgsOFMxNi40MSwyMCwxMiwyMHoiLz4KICA8L2c+Cjwvc3ZnPgo=);
  --jp-icon-cut: url(data:image/svg+xml;base64,PHN2ZyB2aWV3Qm94PSIwIDAgMjQgMjQiIHdpZHRoPSIxNiIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiPgogICAgPHBhdGggZD0iTTkuNjQgNy42NGMuMjMtLjUuMzYtMS4wNS4zNi0xLjY0IDAtMi4yMS0xLjc5LTQtNC00UzIgMy43OSAyIDZzMS43OSA0IDQgNGMuNTkgMCAxLjE0LS4xMyAxLjY0LS4zNkwxMCAxMmwtMi4zNiAyLjM2QzcuMTQgMTQuMTMgNi41OSAxNCA2IDE0Yy0yLjIxIDAtNCAxLjc5LTQgNHMxLjc5IDQgNCA0IDQtMS43OSA0LTRjMC0uNTktLjEzLTEuMTQtLjM2LTEuNjRMMTIgMTRsNyA3aDN2LTFMOS42NCA3LjY0ek02IDhjLTEuMSAwLTItLjg5LTItMnMuOS0yIDItMiAyIC44OSAyIDItLjkgMi0yIDJ6bTAgMTJjLTEuMSAwLTItLjg5LTItMnMuOS0yIDItMiAyIC44OSAyIDItLjkgMi0yIDJ6bTYtNy41Yy0uMjggMC0uNS0uMjItLjUtLjVzLjIyLS41LjUtLjUuNS4yMi41LjUtLjIyLjUtLjUuNXpNMTkgM2wtNiA2IDIgMiA3LTdWM3oiLz4KICA8L2c+Cjwvc3ZnPgo=);
  --jp-icon-download: url(data:image/svg+xml;base64,PHN2ZyB2aWV3Qm94PSIwIDAgMjQgMjQiIHdpZHRoPSIxNiIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiPgogICAgPHBhdGggZD0iTTE5IDloLTRWM0g5djZINWw3IDcgNy03ek01IDE4djJoMTR2LTJINXoiLz4KICA8L2c+Cjwvc3ZnPgo=);
  --jp-icon-edit: url(data:image/svg+xml;base64,PHN2ZyB2aWV3Qm94PSIwIDAgMjQgMjQiIHdpZHRoPSIxNiIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiPgogICAgPHBhdGggZD0iTTMgMTcuMjVWMjFoMy43NUwxNy44MSA5Ljk0bC0zLjc1LTMuNzVMMyAxNy4yNXpNMjAuNzEgNy4wNGMuMzktLjM5LjM5LTEuMDIgMC0xLjQxbC0yLjM0LTIuMzRjLS4zOS0uMzktMS4wMi0uMzktMS40MSAwbC0xLjgzIDEuODMgMy43NSAzLjc1IDEuODMtMS44M3oiLz4KICA8L2c+Cjwvc3ZnPgo=);
  --jp-icon-ellipses: url(data:image/svg+xml;base64,PHN2ZyB2aWV3Qm94PSIwIDAgMjQgMjQiIHdpZHRoPSIxNiIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiPgogICAgPGNpcmNsZSBjeD0iNSIgY3k9IjEyIiByPSIyIi8+CiAgICA8Y2lyY2xlIGN4PSIxMiIgY3k9IjEyIiByPSIyIi8+CiAgICA8Y2lyY2xlIGN4PSIxOSIgY3k9IjEyIiByPSIyIi8+CiAgPC9nPgo8L3N2Zz4K);
  --jp-icon-extension: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDI0IDI0Ij4KICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiPgogICAgPHBhdGggZD0iTTIwLjUgMTFIMTlWN2MwLTEuMS0uOS0yLTItMmgtNFYzLjVDMTMgMi4xMiAxMS44OCAxIDEwLjUgMVM4IDIuMTIgOCAzLjVWNUg0Yy0xLjEgMC0xLjk5LjktMS45OSAydjMuOEgzLjVjMS40OSAwIDIuNyAxLjIxIDIuNyAyLjdzLTEuMjEgMi43LTIuNyAyLjdIMlYyMGMwIDEuMS45IDIgMiAyaDMuOHYtMS41YzAtMS40OSAxLjIxLTIuNyAyLjctMi43IDEuNDkgMCAyLjcgMS4yMSAyLjcgMi43VjIySDE3YzEuMSAwIDItLjkgMi0ydi00aDEuNWMxLjM4IDAgMi41LTEuMTIgMi41LTIuNVMyMS44OCAxMSAyMC41IDExeiIvPgogIDwvZz4KPC9zdmc+Cg==);
  --jp-icon-fast-forward: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIyNCIgaGVpZ2h0PSIyNCIgdmlld0JveD0iMCAwIDI0IDI0Ij4KICAgIDxnIGNsYXNzPSJqcC1pY29uMyIgZmlsbD0iIzYxNjE2MSI+CiAgICAgICAgPHBhdGggZD0iTTQgMThsOC41LTZMNCA2djEyem05LTEydjEybDguNS02TDEzIDZ6Ii8+CiAgICA8L2c+Cjwvc3ZnPgo=);
  --jp-icon-file-upload: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDI0IDI0Ij4KICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiPgogICAgPHBhdGggZD0iTTkgMTZoNnYtNmg0bC03LTctNyA3aDR6bS00IDJoMTR2Mkg1eiIvPgogIDwvZz4KPC9zdmc+Cg==);
  --jp-icon-file: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDIyIDIyIj4KICA8cGF0aCBjbGFzcz0ianAtaWNvbjMganAtaWNvbi1zZWxlY3RhYmxlIiBmaWxsPSIjNjE2MTYxIiBkPSJNMTkuMyA4LjJsLTUuNS01LjVjLS4zLS4zLS43LS41LTEuMi0uNUgzLjljLS44LjEtMS42LjktMS42IDEuOHYxNC4xYzAgLjkuNyAxLjYgMS42IDEuNmgxNC4yYy45IDAgMS42LS43IDEuNi0xLjZWOS40Yy4xLS41LS4xLS45LS40LTEuMnptLTUuOC0zLjNsMy40IDMuNmgtMy40VjQuOXptMy45IDEyLjdINC43Yy0uMSAwLS4yIDAtLjItLjJWNC43YzAtLjIuMS0uMy4yLS4zaDcuMnY0LjRzMCAuOC4zIDEuMWMuMy4zIDEuMS4zIDEuMS4zaDQuM3Y3LjJzLS4xLjItLjIuMnoiLz4KPC9zdmc+Cg==);
  --jp-icon-filter-list: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDI0IDI0Ij4KICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiPgogICAgPHBhdGggZD0iTTEwIDE4aDR2LTJoLTR2MnpNMyA2djJoMThWNkgzem0zIDdoMTJ2LTJINnYyeiIvPgogIDwvZz4KPC9zdmc+Cg==);
  --jp-icon-folder: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDI0IDI0Ij4KICA8cGF0aCBjbGFzcz0ianAtaWNvbjMganAtaWNvbi1zZWxlY3RhYmxlIiBmaWxsPSIjNjE2MTYxIiBkPSJNMTAgNEg0Yy0xLjEgMC0xLjk5LjktMS45OSAyTDIgMThjMCAxLjEuOSAyIDIgMmgxNmMxLjEgMCAyLS45IDItMlY4YzAtMS4xLS45LTItMi0yaC04bC0yLTJ6Ii8+Cjwvc3ZnPgo=);
  --jp-icon-html5: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDUxMiA1MTIiPgogIDxwYXRoIGNsYXNzPSJqcC1pY29uMCBqcC1pY29uLXNlbGVjdGFibGUiIGZpbGw9IiMwMDAiIGQ9Ik0xMDguNCAwaDIzdjIyLjhoMjEuMlYwaDIzdjY5aC0yM1Y0NmgtMjF2MjNoLTIzLjJNMjA2IDIzaC0yMC4zVjBoNjMuN3YyM0gyMjl2NDZoLTIzbTUzLjUtNjloMjQuMWwxNC44IDI0LjNMMzEzLjIgMGgyNC4xdjY5aC0yM1YzNC44bC0xNi4xIDI0LjgtMTYuMS0yNC44VjY5aC0yMi42bTg5LjItNjloMjN2NDYuMmgzMi42VjY5aC01NS42Ii8+CiAgPHBhdGggY2xhc3M9ImpwLWljb24tc2VsZWN0YWJsZSIgZmlsbD0iI2U0NGQyNiIgZD0iTTEwNy42IDQ3MWwtMzMtMzcwLjRoMzYyLjhsLTMzIDM3MC4yTDI1NS43IDUxMiIvPgogIDxwYXRoIGNsYXNzPSJqcC1pY29uLXNlbGVjdGFibGUiIGZpbGw9IiNmMTY1MjkiIGQ9Ik0yNTYgNDgwLjVWMTMxaDE0OC4zTDM3NiA0NDciLz4KICA8cGF0aCBjbGFzcz0ianAtaWNvbi1zZWxlY3RhYmxlLWludmVyc2UiIGZpbGw9IiNlYmViZWIiIGQ9Ik0xNDIgMTc2LjNoMTE0djQ1LjRoLTY0LjJsNC4yIDQ2LjVoNjB2NDUuM0gxNTQuNG0yIDIyLjhIMjAybDMuMiAzNi4zIDUwLjggMTMuNnY0Ny40bC05My4yLTI2Ii8+CiAgPHBhdGggY2xhc3M9ImpwLWljb24tc2VsZWN0YWJsZS1pbnZlcnNlIiBmaWxsPSIjZmZmIiBkPSJNMzY5LjYgMTc2LjNIMjU1Ljh2NDUuNGgxMDkuNm0tNC4xIDQ2LjVIMjU1Ljh2NDUuNGg1NmwtNS4zIDU5LTUwLjcgMTMuNnY0Ny4ybDkzLTI1LjgiLz4KPC9zdmc+Cg==);
  --jp-icon-image: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDIyIDIyIj4KICA8cGF0aCBjbGFzcz0ianAtaWNvbi1icmFuZDQganAtaWNvbi1zZWxlY3RhYmxlLWludmVyc2UiIGZpbGw9IiNGRkYiIGQ9Ik0yLjIgMi4yaDE3LjV2MTcuNUgyLjJ6Ii8+CiAgPHBhdGggY2xhc3M9ImpwLWljb24tYnJhbmQwIGpwLWljb24tc2VsZWN0YWJsZSIgZmlsbD0iIzNGNTFCNSIgZD0iTTIuMiAyLjJ2MTcuNWgxNy41bC4xLTE3LjVIMi4yem0xMi4xIDIuMmMxLjIgMCAyLjIgMSAyLjIgMi4ycy0xIDIuMi0yLjIgMi4yLTIuMi0xLTIuMi0yLjIgMS0yLjIgMi4yLTIuMnpNNC40IDE3LjZsMy4zLTguOCAzLjMgNi42IDIuMi0zLjIgNC40IDUuNEg0LjR6Ii8+Cjwvc3ZnPgo=);
  --jp-icon-inspector: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDI0IDI0Ij4KICA8cGF0aCBjbGFzcz0ianAtaWNvbjMganAtaWNvbi1zZWxlY3RhYmxlIiBmaWxsPSIjNjE2MTYxIiBkPSJNMjAgNEg0Yy0xLjEgMC0xLjk5LjktMS45OSAyTDIgMThjMCAxLjEuOSAyIDIgMmgxNmMxLjEgMCAyLS45IDItMlY2YzAtMS4xLS45LTItMi0yem0tNSAxNEg0di00aDExdjR6bTAtNUg0VjloMTF2NHptNSA1aC00VjloNHY5eiIvPgo8L3N2Zz4K);
  --jp-icon-json: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDIyIDIyIj4KICA8ZyBjbGFzcz0ianAtaWNvbi13YXJuMSBqcC1pY29uLXNlbGVjdGFibGUiIGZpbGw9IiNGOUE4MjUiPgogICAgPHBhdGggZD0iTTIwLjIgMTEuOGMtMS42IDAtMS43LjUtMS43IDEgMCAuNC4xLjkuMSAxLjMuMS41LjEuOS4xIDEuMyAwIDEuNy0xLjQgMi4zLTMuNSAyLjNoLS45di0xLjloLjVjMS4xIDAgMS40IDAgMS40LS44IDAtLjMgMC0uNi0uMS0xIDAtLjQtLjEtLjgtLjEtMS4yIDAtMS4zIDAtMS44IDEuMy0yLTEuMy0uMi0xLjMtLjctMS4zLTIgMC0uNC4xLS44LjEtMS4yLjEtLjQuMS0uNy4xLTEgMC0uOC0uNC0uNy0xLjQtLjhoLS41VjQuMWguOWMyLjIgMCAzLjUuNyAzLjUgMi4zIDAgLjQtLjEuOS0uMSAxLjMtLjEuNS0uMS45LS4xIDEuMyAwIC41LjIgMSAxLjcgMXYxLjh6TTEuOCAxMC4xYzEuNiAwIDEuNy0uNSAxLjctMSAwLS40LS4xLS45LS4xLTEuMy0uMS0uNS0uMS0uOS0uMS0xLjMgMC0xLjYgMS40LTIuMyAzLjUtMi4zaC45djEuOWgtLjVjLTEgMC0xLjQgMC0xLjQuOCAwIC4zIDAgLjYuMSAxIDAgLjIuMS42LjEgMSAwIDEuMyAwIDEuOC0xLjMgMkM2IDExLjIgNiAxMS43IDYgMTNjMCAuNC0uMS44LS4xIDEuMi0uMS4zLS4xLjctLjEgMSAwIC44LjMuOCAxLjQuOGguNXYxLjloLS45Yy0yLjEgMC0zLjUtLjYtMy41LTIuMyAwLS40LjEtLjkuMS0xLjMuMS0uNS4xLS45LjEtMS4zIDAtLjUtLjItMS0xLjctMXYtMS45eiIvPgogICAgPGNpcmNsZSBjeD0iMTEiIGN5PSIxMy44IiByPSIyLjEiLz4KICAgIDxjaXJjbGUgY3g9IjExIiBjeT0iOC4yIiByPSIyLjEiLz4KICA8L2c+Cjwvc3ZnPgo=);
  --jp-icon-julia: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDMyNSAzMDAiPgogIDxnIGNsYXNzPSJqcC1icmFuZDAganAtaWNvbi1zZWxlY3RhYmxlIiBmaWxsPSIjY2IzYzMzIj4KICAgIDxwYXRoIGQ9Ik0gMTUwLjg5ODQzOCAyMjUgQyAxNTAuODk4NDM4IDI2Ni40MjE4NzUgMTE3LjMyMDMxMiAzMDAgNzUuODk4NDM4IDMwMCBDIDM0LjQ3NjU2MiAzMDAgMC44OTg0MzggMjY2LjQyMTg3NSAwLjg5ODQzOCAyMjUgQyAwLjg5ODQzOCAxODMuNTc4MTI1IDM0LjQ3NjU2MiAxNTAgNzUuODk4NDM4IDE1MCBDIDExNy4zMjAzMTIgMTUwIDE1MC44OTg0MzggMTgzLjU3ODEyNSAxNTAuODk4NDM4IDIyNSIvPgogIDwvZz4KICA8ZyBjbGFzcz0ianAtYnJhbmQwIGpwLWljb24tc2VsZWN0YWJsZSIgZmlsbD0iIzM4OTgyNiI+CiAgICA8cGF0aCBkPSJNIDIzNy41IDc1IEMgMjM3LjUgMTE2LjQyMTg3NSAyMDMuOTIxODc1IDE1MCAxNjIuNSAxNTAgQyAxMjEuMDc4MTI1IDE1MCA4Ny41IDExNi40MjE4NzUgODcuNSA3NSBDIDg3LjUgMzMuNTc4MTI1IDEyMS4wNzgxMjUgMCAxNjIuNSAwIEMgMjAzLjkyMTg3NSAwIDIzNy41IDMzLjU3ODEyNSAyMzcuNSA3NSIvPgogIDwvZz4KICA8ZyBjbGFzcz0ianAtYnJhbmQwIGpwLWljb24tc2VsZWN0YWJsZSIgZmlsbD0iIzk1NThiMiI+CiAgICA8cGF0aCBkPSJNIDMyNC4xMDE1NjIgMjI1IEMgMzI0LjEwMTU2MiAyNjYuNDIxODc1IDI5MC41MjM0MzggMzAwIDI0OS4xMDE1NjIgMzAwIEMgMjA3LjY3OTY4OCAzMDAgMTc0LjEwMTU2MiAyNjYuNDIxODc1IDE3NC4xMDE1NjIgMjI1IEMgMTc0LjEwMTU2MiAxODMuNTc4MTI1IDIwNy42Nzk2ODggMTUwIDI0OS4xMDE1NjIgMTUwIEMgMjkwLjUyMzQzOCAxNTAgMzI0LjEwMTU2MiAxODMuNTc4MTI1IDMyNC4xMDE1NjIgMjI1Ii8+CiAgPC9nPgo8L3N2Zz4K);
  --jp-icon-jupyter-favicon: url(data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMTUyIiBoZWlnaHQ9IjE2NSIgdmlld0JveD0iMCAwIDE1MiAxNjUiIHZlcnNpb249IjEuMSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KICA8ZyBjbGFzcz0ianAtaWNvbi13YXJuMCIgZmlsbD0iI0YzNzcyNiI+CiAgICA8cGF0aCB0cmFuc2Zvcm09InRyYW5zbGF0ZSgwLjA3ODk0NywgMTEwLjU4MjkyNykiIGQ9Ik03NS45NDIyODQyLDI5LjU4MDQ1NjEgQzQzLjMwMjM5NDcsMjkuNTgwNDU2MSAxNC43OTY3ODMyLDE3LjY1MzQ2MzQgMCwwIEM1LjUxMDgzMjExLDE1Ljg0MDY4MjkgMTUuNzgxNTM4OSwyOS41NjY3NzMyIDI5LjM5MDQ5NDcsMzkuMjc4NDE3MSBDNDIuOTk5Nyw0OC45ODk4NTM3IDU5LjI3MzcsNTQuMjA2NzgwNSA3NS45NjA1Nzg5LDU0LjIwNjc4MDUgQzkyLjY0NzQ1NzksNTQuMjA2NzgwNSAxMDguOTIxNDU4LDQ4Ljk4OTg1MzcgMTIyLjUzMDY2MywzOS4yNzg0MTcxIEMxMzYuMTM5NDUzLDI5LjU2Njc3MzIgMTQ2LjQxMDI4NCwxNS44NDA2ODI5IDE1MS45MjExNTgsMCBDMTM3LjA4Nzg2OCwxNy42NTM0NjM0IDEwOC41ODI1ODksMjkuNTgwNDU2MSA3NS45NDIyODQyLDI5LjU4MDQ1NjEgTDc1Ljk0MjI4NDIsMjkuNTgwNDU2MSBaIiAvPgogICAgPHBhdGggdHJhbnNmb3JtPSJ0cmFuc2xhdGUoMC4wMzczNjgsIDAuNzA0ODc4KSIgZD0iTTc1Ljk3ODQ1NzksMjQuNjI2NDA3MyBDMTA4LjYxODc2MywyNC42MjY0MDczIDEzNy4xMjQ0NTgsMzYuNTUzNDQxNSAxNTEuOTIxMTU4LDU0LjIwNjc4MDUgQzE0Ni40MTAyODQsMzguMzY2MjIyIDEzNi4xMzk0NTMsMjQuNjQwMTMxNyAxMjIuNTMwNjYzLDE0LjkyODQ4NzggQzEwOC45MjE0NTgsNS4yMTY4NDM5IDkyLjY0NzQ1NzksMCA3NS45NjA1Nzg5LDAgQzU5LjI3MzcsMCA0Mi45OTk3LDUuMjE2ODQzOSAyOS4zOTA0OTQ3LDE0LjkyODQ4NzggQzE1Ljc4MTUzODksMjQuNjQwMTMxNyA1LjUxMDgzMjExLDM4LjM2NjIyMiAwLDU0LjIwNjc4MDUgQzE0LjgzMzA4MTYsMzYuNTg5OTI5MyA0My4zMzg1Njg0LDI0LjYyNjQwNzMgNzUuOTc4NDU3OSwyNC42MjY0MDczIEw3NS45Nzg0NTc5LDI0LjYyNjQwNzMgWiIgLz4KICA8L2c+Cjwvc3ZnPgo=);
  --jp-icon-jupyter: url(data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMzkiIGhlaWdodD0iNTEiIHZpZXdCb3g9IjAgMCAzOSA1MSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KICA8ZyB0cmFuc2Zvcm09InRyYW5zbGF0ZSgtMTYzOCAtMjI4MSkiPgogICAgPGcgY2xhc3M9ImpwLWljb24td2FybjAiIGZpbGw9IiNGMzc3MjYiPgogICAgICA8cGF0aCB0cmFuc2Zvcm09InRyYW5zbGF0ZSgxNjM5Ljc0IDIzMTEuOTgpIiBkPSJNIDE4LjI2NDYgNy4xMzQxMUMgMTAuNDE0NSA3LjEzNDExIDMuNTU4NzIgNC4yNTc2IDAgMEMgMS4zMjUzOSAzLjgyMDQgMy43OTU1NiA3LjEzMDgxIDcuMDY4NiA5LjQ3MzAzQyAxMC4zNDE3IDExLjgxNTIgMTQuMjU1NyAxMy4wNzM0IDE4LjI2OSAxMy4wNzM0QyAyMi4yODIzIDEzLjA3MzQgMjYuMTk2MyAxMS44MTUyIDI5LjQ2OTQgOS40NzMwM0MgMzIuNzQyNCA3LjEzMDgxIDM1LjIxMjYgMy44MjA0IDM2LjUzOCAwQyAzMi45NzA1IDQuMjU3NiAyNi4xMTQ4IDcuMTM0MTEgMTguMjY0NiA3LjEzNDExWiIvPgogICAgICA8cGF0aCB0cmFuc2Zvcm09InRyYW5zbGF0ZSgxNjM5LjczIDIyODUuNDgpIiBkPSJNIDE4LjI3MzMgNS45MzkzMUMgMjYuMTIzNSA1LjkzOTMxIDMyLjk3OTMgOC44MTU4MyAzNi41MzggMTMuMDczNEMgMzUuMjEyNiA5LjI1MzAzIDMyLjc0MjQgNS45NDI2MiAyOS40Njk0IDMuNjAwNEMgMjYuMTk2MyAxLjI1ODE4IDIyLjI4MjMgMCAxOC4yNjkgMEMgMTQuMjU1NyAwIDEwLjM0MTcgMS4yNTgxOCA3LjA2ODYgMy42MDA0QyAzLjc5NTU2IDUuOTQyNjIgMS4zMjUzOSA5LjI1MzAzIDAgMTMuMDczNEMgMy41Njc0NSA4LjgyNDYzIDEwLjQyMzIgNS45MzkzMSAxOC4yNzMzIDUuOTM5MzFaIi8+CiAgICA8L2c+CiAgICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiPgogICAgICA8cGF0aCB0cmFuc2Zvcm09InRyYW5zbGF0ZSgxNjY5LjMgMjI4MS4zMSkiIGQ9Ik0gNS44OTM1MyAyLjg0NEMgNS45MTg4OSAzLjQzMTY1IDUuNzcwODUgNC4wMTM2NyA1LjQ2ODE1IDQuNTE2NDVDIDUuMTY1NDUgNS4wMTkyMiA0LjcyMTY4IDUuNDIwMTUgNC4xOTI5OSA1LjY2ODUxQyAzLjY2NDMgNS45MTY4OCAzLjA3NDQ0IDYuMDAxNTEgMi40OTgwNSA1LjkxMTcxQyAxLjkyMTY2IDUuODIxOSAxLjM4NDYzIDUuNTYxNyAwLjk1NDg5OCA1LjE2NDAxQyAwLjUyNTE3IDQuNzY2MzMgMC4yMjIwNTYgNC4yNDkwMyAwLjA4MzkwMzcgMy42Nzc1N0MgLTAuMDU0MjQ4MyAzLjEwNjExIC0wLjAyMTIzIDIuNTA2MTcgMC4xNzg3ODEgMS45NTM2NEMgMC4zNzg3OTMgMS40MDExIDAuNzM2ODA5IDAuOTIwODE3IDEuMjA3NTQgMC41NzM1MzhDIDEuNjc4MjYgMC4yMjYyNTkgMi4yNDA1NSAwLjAyNzU5MTkgMi44MjMyNiAwLjAwMjY3MjI5QyAzLjYwMzg5IC0wLjAzMDcxMTUgNC4zNjU3MyAwLjI0OTc4OSA0Ljk0MTQyIDAuNzgyNTUxQyA1LjUxNzExIDEuMzE1MzEgNS44NTk1NiAyLjA1Njc2IDUuODkzNTMgMi44NDRaIi8+CiAgICAgIDxwYXRoIHRyYW5zZm9ybT0idHJhbnNsYXRlKDE2MzkuOCAyMzIzLjgxKSIgZD0iTSA3LjQyNzg5IDMuNTgzMzhDIDcuNDYwMDggNC4zMjQzIDcuMjczNTUgNS4wNTgxOSA2Ljg5MTkzIDUuNjkyMTNDIDYuNTEwMzEgNi4zMjYwNyA1Ljk1MDc1IDYuODMxNTYgNS4yODQxMSA3LjE0NDZDIDQuNjE3NDcgNy40NTc2MyAzLjg3MzcxIDcuNTY0MTQgMy4xNDcwMiA3LjQ1MDYzQyAyLjQyMDMyIDcuMzM3MTIgMS43NDMzNiA3LjAwODcgMS4yMDE4NCA2LjUwNjk1QyAwLjY2MDMyOCA2LjAwNTIgMC4yNzg2MSA1LjM1MjY4IDAuMTA1MDE3IDQuNjMyMDJDIC0wLjA2ODU3NTcgMy45MTEzNSAtMC4wMjYyMzYxIDMuMTU0OTQgMC4yMjY2NzUgMi40NTg1NkMgMC40Nzk1ODcgMS43NjIxNyAwLjkzMTY5NyAxLjE1NzEzIDEuNTI1NzYgMC43MjAwMzNDIDIuMTE5ODMgMC4yODI5MzUgMi44MjkxNCAwLjAzMzQzOTUgMy41NjM4OSAwLjAwMzEzMzQ0QyA0LjU0NjY3IC0wLjAzNzQwMzMgNS41MDUyOSAwLjMxNjcwNiA2LjIyOTYxIDAuOTg3ODM1QyA2Ljk1MzkzIDEuNjU4OTYgNy4zODQ4NCAyLjU5MjM1IDcuNDI3ODkgMy41ODMzOEwgNy40Mjc4OSAzLjU4MzM4WiIvPgogICAgICA8cGF0aCB0cmFuc2Zvcm09InRyYW5zbGF0ZSgxNjM4LjM2IDIyODYuMDYpIiBkPSJNIDIuMjc0NzEgNC4zOTYyOUMgMS44NDM2MyA0LjQxNTA4IDEuNDE2NzEgNC4zMDQ0NSAxLjA0Nzk5IDQuMDc4NDNDIDAuNjc5MjY4IDMuODUyNCAwLjM4NTMyOCAzLjUyMTE0IDAuMjAzMzcxIDMuMTI2NTZDIDAuMDIxNDEzNiAyLjczMTk4IC0wLjA0MDM3OTggMi4yOTE4MyAwLjAyNTgxMTYgMS44NjE4MUMgMC4wOTIwMDMxIDEuNDMxOCAwLjI4MzIwNCAxLjAzMTI2IDAuNTc1MjEzIDAuNzEwODgzQyAwLjg2NzIyMiAwLjM5MDUxIDEuMjQ2OTEgMC4xNjQ3MDggMS42NjYyMiAwLjA2MjA1OTJDIDIuMDg1NTMgLTAuMDQwNTg5NyAyLjUyNTYxIC0wLjAxNTQ3MTQgMi45MzA3NiAwLjEzNDIzNUMgMy4zMzU5MSAwLjI4Mzk0MSAzLjY4NzkyIDAuNTUxNTA1IDMuOTQyMjIgMC45MDMwNkMgNC4xOTY1MiAxLjI1NDYyIDQuMzQxNjkgMS42NzQzNiA0LjM1OTM1IDIuMTA5MTZDIDQuMzgyOTkgMi42OTEwNyA0LjE3Njc4IDMuMjU4NjkgMy43ODU5NyAzLjY4NzQ2QyAzLjM5NTE2IDQuMTE2MjQgMi44NTE2NiA0LjM3MTE2IDIuMjc0NzEgNC4zOTYyOUwgMi4yNzQ3MSA0LjM5NjI5WiIvPgogICAgPC9nPgogIDwvZz4+Cjwvc3ZnPgo=);
  --jp-icon-jupyterlab-wordmark: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIyMDAiIHZpZXdCb3g9IjAgMCAxODYwLjggNDc1Ij4KICA8ZyBjbGFzcz0ianAtaWNvbjIiIGZpbGw9IiM0RTRFNEUiIHRyYW5zZm9ybT0idHJhbnNsYXRlKDQ4MC4xMzY0MDEsIDY0LjI3MTQ5MykiPgogICAgPGcgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoMC4wMDAwMDAsIDU4Ljg3NTU2NikiPgogICAgICA8ZyB0cmFuc2Zvcm09InRyYW5zbGF0ZSgwLjA4NzYwMywgMC4xNDAyOTQpIj4KICAgICAgICA8cGF0aCBkPSJNLTQyNi45LDE2OS44YzAsNDguNy0zLjcsNjQuNy0xMy42LDc2LjRjLTEwLjgsMTAtMjUsMTUuNS0zOS43LDE1LjVsMy43LDI5IGMyMi44LDAuMyw0NC44LTcuOSw2MS45LTIzLjFjMTcuOC0xOC41LDI0LTQ0LjEsMjQtODMuM1YwSC00Mjd2MTcwLjFMLTQyNi45LDE2OS44TC00MjYuOSwxNjkuOHoiLz4KICAgICAgPC9nPgogICAgPC9nPgogICAgPGcgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoMTU1LjA0NTI5NiwgNTYuODM3MTA0KSI+CiAgICAgIDxnIHRyYW5zZm9ybT0idHJhbnNsYXRlKDEuNTYyNDUzLCAxLjc5OTg0MikiPgogICAgICAgIDxwYXRoIGQ9Ik0tMzEyLDE0OGMwLDIxLDAsMzkuNSwxLjcsNTUuNGgtMzEuOGwtMi4xLTMzLjNoLTAuOGMtNi43LDExLjYtMTYuNCwyMS4zLTI4LDI3LjkgYy0xMS42LDYuNi0yNC44LDEwLTM4LjIsOS44Yy0zMS40LDAtNjktMTcuNy02OS04OVYwaDM2LjR2MTEyLjdjMCwzOC43LDExLjYsNjQuNyw0NC42LDY0LjdjMTAuMy0wLjIsMjAuNC0zLjUsMjguOS05LjQgYzguNS01LjksMTUuMS0xNC4zLDE4LjktMjMuOWMyLjItNi4xLDMuMy0xMi41LDMuMy0xOC45VjAuMmgzNi40VjE0OEgtMzEyTC0zMTIsMTQ4eiIvPgogICAgICA8L2c+CiAgICA8L2c+CiAgICA8ZyB0cmFuc2Zvcm09InRyYW5zbGF0ZSgzOTAuMDEzMzIyLCA1My40Nzk2MzgpIj4KICAgICAgPGcgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoMS43MDY0NTgsIDAuMjMxNDI1KSI+CiAgICAgICAgPHBhdGggZD0iTS00NzguNiw3MS40YzAtMjYtMC44LTQ3LTEuNy02Ni43aDMyLjdsMS43LDM0LjhoMC44YzcuMS0xMi41LDE3LjUtMjIuOCwzMC4xLTI5LjcgYzEyLjUtNywyNi43LTEwLjMsNDEtOS44YzQ4LjMsMCw4NC43LDQxLjcsODQuNywxMDMuM2MwLDczLjEtNDMuNywxMDkuMi05MSwxMDkuMmMtMTIuMSwwLjUtMjQuMi0yLjItMzUtNy44IGMtMTAuOC01LjYtMTkuOS0xMy45LTI2LjYtMjQuMmgtMC44VjI5MWgtMzZ2LTIyMEwtNDc4LjYsNzEuNEwtNDc4LjYsNzEuNHogTS00NDIuNiwxMjUuNmMwLjEsNS4xLDAuNiwxMC4xLDEuNywxNS4xIGMzLDEyLjMsOS45LDIzLjMsMTkuOCwzMS4xYzkuOSw3LjgsMjIuMSwxMi4xLDM0LjcsMTIuMWMzOC41LDAsNjAuNy0zMS45LDYwLjctNzguNWMwLTQwLjctMjEuMS03NS42LTU5LjUtNzUuNiBjLTEyLjksMC40LTI1LjMsNS4xLTM1LjMsMTMuNGMtOS45LDguMy0xNi45LDE5LjctMTkuNiwzMi40Yy0xLjUsNC45LTIuMywxMC0yLjUsMTUuMVYxMjUuNkwtNDQyLjYsMTI1LjZMLTQ0Mi42LDEyNS42eiIvPgogICAgICA8L2c+CiAgICA8L2c+CiAgICA8ZyB0cmFuc2Zvcm09InRyYW5zbGF0ZSg2MDYuNzQwNzI2LCA1Ni44MzcxMDQpIj4KICAgICAgPGcgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoMC43NTEyMjYsIDEuOTg5Mjk5KSI+CiAgICAgICAgPHBhdGggZD0iTS00NDAuOCwwbDQzLjcsMTIwLjFjNC41LDEzLjQsOS41LDI5LjQsMTIuOCw0MS43aDAuOGMzLjctMTIuMiw3LjktMjcuNywxMi44LTQyLjQgbDM5LjctMTE5LjJoMzguNUwtMzQ2LjksMTQ1Yy0yNiw2OS43LTQzLjcsMTA1LjQtNjguNiwxMjcuMmMtMTIuNSwxMS43LTI3LjksMjAtNDQuNiwyMy45bC05LjEtMzEuMSBjMTEuNy0zLjksMjIuNS0xMC4xLDMxLjgtMTguMWMxMy4yLTExLjEsMjMuNy0yNS4yLDMwLjYtNDEuMmMxLjUtMi44LDIuNS01LjcsMi45LTguOGMtMC4zLTMuMy0xLjItNi42LTIuNS05LjdMLTQ4MC4yLDAuMSBoMzkuN0wtNDQwLjgsMEwtNDQwLjgsMHoiLz4KICAgICAgPC9nPgogICAgPC9nPgogICAgPGcgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoODIyLjc0ODEwNCwgMC4wMDAwMDApIj4KICAgICAgPGcgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoMS40NjQwNTAsIDAuMzc4OTE0KSI+CiAgICAgICAgPHBhdGggZD0iTS00MTMuNywwdjU4LjNoNTJ2MjguMmgtNTJWMTk2YzAsMjUsNywzOS41LDI3LjMsMzkuNWM3LjEsMC4xLDE0LjItMC43LDIxLjEtMi41IGwxLjcsMjcuN2MtMTAuMywzLjctMjEuMyw1LjQtMzIuMiw1Yy03LjMsMC40LTE0LjYtMC43LTIxLjMtMy40Yy02LjgtMi43LTEyLjktNi44LTE3LjktMTIuMWMtMTAuMy0xMC45LTE0LjEtMjktMTQuMS01Mi45IFY4Ni41aC0zMVY1OC4zaDMxVjkuNkwtNDEzLjcsMEwtNDEzLjcsMHoiLz4KICAgICAgPC9nPgogICAgPC9nPgogICAgPGcgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoOTc0LjQzMzI4NiwgNTMuNDc5NjM4KSI+CiAgICAgIDxnIHRyYW5zZm9ybT0idHJhbnNsYXRlKDAuOTkwMDM0LCAwLjYxMDMzOSkiPgogICAgICAgIDxwYXRoIGQ9Ik0tNDQ1LjgsMTEzYzAuOCw1MCwzMi4yLDcwLjYsNjguNiw3MC42YzE5LDAuNiwzNy45LTMsNTUuMy0xMC41bDYuMiwyNi40IGMtMjAuOSw4LjktNDMuNSwxMy4xLTY2LjIsMTIuNmMtNjEuNSwwLTk4LjMtNDEuMi05OC4zLTEwMi41Qy00ODAuMiw0OC4yLTQ0NC43LDAtMzg2LjUsMGM2NS4yLDAsODIuNyw1OC4zLDgyLjcsOTUuNyBjLTAuMSw1LjgtMC41LDExLjUtMS4yLDE3LjJoLTE0MC42SC00NDUuOEwtNDQ1LjgsMTEzeiBNLTMzOS4yLDg2LjZjMC40LTIzLjUtOS41LTYwLjEtNTAuNC02MC4xIGMtMzYuOCwwLTUyLjgsMzQuNC01NS43LDYwLjFILTMzOS4yTC0zMzkuMiw4Ni42TC0zMzkuMiw4Ni42eiIvPgogICAgICA8L2c+CiAgICA8L2c+CiAgICA8ZyB0cmFuc2Zvcm09InRyYW5zbGF0ZSgxMjAxLjk2MTA1OCwgNTMuNDc5NjM4KSI+CiAgICAgIDxnIHRyYW5zZm9ybT0idHJhbnNsYXRlKDEuMTc5NjQwLCAwLjcwNTA2OCkiPgogICAgICAgIDxwYXRoIGQ9Ik0tNDc4LjYsNjhjMC0yMy45LTAuNC00NC41LTEuNy02My40aDMxLjhsMS4yLDM5LjloMS43YzkuMS0yNy4zLDMxLTQ0LjUsNTUuMy00NC41IGMzLjUtMC4xLDcsMC40LDEwLjMsMS4ydjM0LjhjLTQuMS0wLjktOC4yLTEuMy0xMi40LTEuMmMtMjUuNiwwLTQzLjcsMTkuNy00OC43LDQ3LjRjLTEsNS43LTEuNiwxMS41LTEuNywxNy4ydjEwOC4zaC0zNlY2OCBMLTQ3OC42LDY4eiIvPgogICAgICA8L2c+CiAgICA8L2c+CiAgPC9nPgoKICA8ZyBjbGFzcz0ianAtaWNvbi13YXJuMCIgZmlsbD0iI0YzNzcyNiI+CiAgICA8cGF0aCBkPSJNMTM1Mi4zLDMyNi4yaDM3VjI4aC0zN1YzMjYuMnogTTE2MDQuOCwzMjYuMmMtMi41LTEzLjktMy40LTMxLjEtMy40LTQ4Ljd2LTc2IGMwLTQwLjctMTUuMS04My4xLTc3LjMtODMuMWMtMjUuNiwwLTUwLDcuMS02Ni44LDE4LjFsOC40LDI0LjRjMTQuMy05LjIsMzQtMTUuMSw1My0xNS4xYzQxLjYsMCw0Ni4yLDMwLjIsNDYuMiw0N3Y0LjIgYy03OC42LTAuNC0xMjIuMywyNi41LTEyMi4zLDc1LjZjMCwyOS40LDIxLDU4LjQsNjIuMiw1OC40YzI5LDAsNTAuOS0xNC4zLDYyLjItMzAuMmgxLjNsMi45LDI1LjZIMTYwNC44eiBNMTU2NS43LDI1Ny43IGMwLDMuOC0wLjgsOC0yLjEsMTEuOGMtNS45LDE3LjItMjIuNywzNC00OS4yLDM0Yy0xOC45LDAtMzQuOS0xMS4zLTM0LjktMzUuM2MwLTM5LjUsNDUuOC00Ni42LDg2LjItNDUuOFYyNTcuN3ogTTE2OTguNSwzMjYuMiBsMS43LTMzLjZoMS4zYzE1LjEsMjYuOSwzOC43LDM4LjIsNjguMSwzOC4yYzQ1LjQsMCw5MS4yLTM2LjEsOTEuMi0xMDguOGMwLjQtNjEuNy0zNS4zLTEwMy43LTg1LjctMTAzLjcgYy0zMi44LDAtNTYuMywxNC43LTY5LjMsMzcuNGgtMC44VjI4aC0zNi42djI0NS43YzAsMTguMS0wLjgsMzguNi0xLjcsNTIuNUgxNjk4LjV6IE0xNzA0LjgsMjA4LjJjMC01LjksMS4zLTEwLjksMi4xLTE1LjEgYzcuNi0yOC4xLDMxLjEtNDUuNCw1Ni4zLTQ1LjRjMzkuNSwwLDYwLjUsMzQuOSw2MC41LDc1LjZjMCw0Ni42LTIzLjEsNzguMS02MS44LDc4LjFjLTI2LjksMC00OC4zLTE3LjYtNTUuNS00My4zIGMtMC44LTQuMi0xLjctOC44LTEuNy0xMy40VjIwOC4yeiIvPgogIDwvZz4KPC9zdmc+Cg==);
  --jp-icon-kernel: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDI0IDI0Ij4KICAgIDxwYXRoIGNsYXNzPSJqcC1pY29uMiIgZmlsbD0iIzYxNjE2MSIgZD0iTTE1IDlIOXY2aDZWOXptLTIgNGgtMnYtMmgydjJ6bTgtMlY5aC0yVjdjMC0xLjEtLjktMi0yLTJoLTJWM2gtMnYyaC0yVjNIOXYySDdjLTEuMSAwLTIgLjktMiAydjJIM3YyaDJ2MkgzdjJoMnYyYzAgMS4xLjkgMiAyIDJoMnYyaDJ2LTJoMnYyaDJ2LTJoMmMxLjEgMCAyLS45IDItMnYtMmgydi0yaC0ydi0yaDJ6bS00IDZIN1Y3aDEwdjEweiIvPgo8L3N2Zz4K);
  --jp-icon-keyboard: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDI0IDI0Ij4KICA8cGF0aCBjbGFzcz0ianAtaWNvbjMganAtaWNvbi1zZWxlY3RhYmxlIiBmaWxsPSIjNjE2MTYxIiBkPSJNMjAgNUg0Yy0xLjEgMC0xLjk5LjktMS45OSAyTDIgMTdjMCAxLjEuOSAyIDIgMmgxNmMxLjEgMCAyLS45IDItMlY3YzAtMS4xLS45LTItMi0yem0tOSAzaDJ2MmgtMlY4em0wIDNoMnYyaC0ydi0yek04IDhoMnYySDhWOHptMCAzaDJ2Mkg4di0yem0tMSAySDV2LTJoMnYyem0wLTNINVY4aDJ2MnptOSA3SDh2LTJoOHYyem0wLTRoLTJ2LTJoMnYyem0wLTNoLTJWOGgydjJ6bTMgM2gtMnYtMmgydjJ6bTAtM2gtMlY4aDJ2MnoiLz4KPC9zdmc+Cg==);
  --jp-icon-launcher: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDI0IDI0Ij4KICA8cGF0aCBjbGFzcz0ianAtaWNvbjMganAtaWNvbi1zZWxlY3RhYmxlIiBmaWxsPSIjNjE2MTYxIiBkPSJNMTkgMTlINVY1aDdWM0g1YTIgMiAwIDAwLTIgMnYxNGEyIDIgMCAwMDIgMmgxNGMxLjEgMCAyLS45IDItMnYtN2gtMnY3ek0xNCAzdjJoMy41OWwtOS44MyA5LjgzIDEuNDEgMS40MUwxOSA2LjQxVjEwaDJWM2gtN3oiLz4KPC9zdmc+Cg==);
  --jp-icon-line-form: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDI0IDI0Ij4KICAgIDxwYXRoIGZpbGw9IndoaXRlIiBkPSJNNS44OCA0LjEyTDEzLjc2IDEybC03Ljg4IDcuODhMOCAyMmwxMC0xMEw4IDJ6Ii8+Cjwvc3ZnPgo=);
  --jp-icon-link: url(data:image/svg+xml;base64,PHN2ZyB2aWV3Qm94PSIwIDAgMjQgMjQiIHdpZHRoPSIxNiIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiPgogICAgPHBhdGggZD0iTTMuOSAxMmMwLTEuNzEgMS4zOS0zLjEgMy4xLTMuMWg0VjdIN2MtMi43NiAwLTUgMi4yNC01IDVzMi4yNCA1IDUgNWg0di0xLjlIN2MtMS43MSAwLTMuMS0xLjM5LTMuMS0zLjF6TTggMTNoOHYtMkg4djJ6bTktNmgtNHYxLjloNGMxLjcxIDAgMy4xIDEuMzkgMy4xIDMuMXMtMS4zOSAzLjEtMy4xIDMuMWgtNFYxN2g0YzIuNzYgMCA1LTIuMjQgNS01cy0yLjI0LTUtNS01eiIvPgogIDwvZz4KPC9zdmc+Cg==);
  --jp-icon-list: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDI0IDI0Ij4KICAgIDxwYXRoIGNsYXNzPSJqcC1pY29uMiBqcC1pY29uLXNlbGVjdGFibGUiIGZpbGw9IiM2MTYxNjEiIGQ9Ik0xOSA1djE0SDVWNWgxNG0xLjEtMkgzLjljLS41IDAtLjkuNC0uOS45djE2LjJjMCAuNC40LjkuOS45aDE2LjJjLjQgMCAuOS0uNS45LS45VjMuOWMwLS41LS41LS45LS45LS45ek0xMSA3aDZ2MmgtNlY3em0wIDRoNnYyaC02di0yem0wIDRoNnYyaC02ek03IDdoMnYySDd6bTAgNGgydjJIN3ptMCA0aDJ2Mkg3eiIvPgo8L3N2Zz4=);
  --jp-icon-listings-info: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCA1MC45NzggNTAuOTc4IiBzdHlsZT0iZW5hYmxlLWJhY2tncm91bmQ6bmV3IDAgMCA1MC45NzggNTAuOTc4OyIgeG1sOnNwYWNlPSJwcmVzZXJ2ZSI+Cgk8Zz4KCQk8cGF0aCBzdHlsZT0iZmlsbDojMDEwMDAyOyIgZD0iTTQzLjUyLDcuNDU4QzM4LjcxMSwyLjY0OCwzMi4zMDcsMCwyNS40ODksMEMxOC42NywwLDEyLjI2NiwyLjY0OCw3LjQ1OCw3LjQ1OAoJCQljLTkuOTQzLDkuOTQxLTkuOTQzLDI2LjExOSwwLDM2LjA2MmM0LjgwOSw0LjgwOSwxMS4yMTIsNy40NTYsMTguMDMxLDcuNDU4YzAsMCwwLjAwMSwwLDAuMDAyLDAKCQkJYzYuODE2LDAsMTMuMjIxLTIuNjQ4LDE4LjAyOS03LjQ1OGM0LjgwOS00LjgwOSw3LjQ1Ny0xMS4yMTIsNy40NTctMTguMDNDNTAuOTc3LDE4LjY3LDQ4LjMyOCwxMi4yNjYsNDMuNTIsNy40NTh6CgkJCSBNNDIuMTA2LDQyLjEwNWMtNC40MzIsNC40MzEtMTAuMzMyLDYuODcyLTE2LjYxNSw2Ljg3MmgtMC4wMDJjLTYuMjg1LTAuMDAxLTEyLjE4Ny0yLjQ0MS0xNi42MTctNi44NzIKCQkJYy05LjE2Mi05LjE2My05LjE2Mi0yNC4wNzEsMC0zMy4yMzNDMTMuMzAzLDQuNDQsMTkuMjA0LDIsMjUuNDg5LDJjNi4yODQsMCwxMi4xODYsMi40NCwxNi42MTcsNi44NzIKCQkJYzQuNDMxLDQuNDMxLDYuODcxLDEwLjMzMiw2Ljg3MSwxNi42MTdDNDguOTc3LDMxLjc3Miw0Ni41MzYsMzcuNjc1LDQyLjEwNiw0Mi4xMDV6Ii8+CgkJPHBhdGggc3R5bGU9ImZpbGw6IzAxMDAwMjsiIGQ9Ik0yMy41NzgsMzIuMjE4Yy0wLjAyMy0xLjczNCwwLjE0My0zLjA1OSwwLjQ5Ni0zLjk3MmMwLjM1My0wLjkxMywxLjExLTEuOTk3LDIuMjcyLTMuMjUzCgkJCWMwLjQ2OC0wLjUzNiwwLjkyMy0xLjA2MiwxLjM2Ny0xLjU3NWMwLjYyNi0wLjc1MywxLjEwNC0xLjQ3OCwxLjQzNi0yLjE3NWMwLjMzMS0wLjcwNywwLjQ5NS0xLjU0MSwwLjQ5NS0yLjUKCQkJYzAtMS4wOTYtMC4yNi0yLjA4OC0wLjc3OS0yLjk3OWMtMC41NjUtMC44NzktMS41MDEtMS4zMzYtMi44MDYtMS4zNjljLTEuODAyLDAuMDU3LTIuOTg1LDAuNjY3LTMuNTUsMS44MzIKCQkJYy0wLjMwMSwwLjUzNS0wLjUwMywxLjE0MS0wLjYwNywxLjgxNGMtMC4xMzksMC43MDctMC4yMDcsMS40MzItMC4yMDcsMi4xNzRoLTIuOTM3Yy0wLjA5MS0yLjIwOCwwLjQwNy00LjExNCwxLjQ5My01LjcxOQoJCQljMS4wNjItMS42NCwyLjg1NS0yLjQ4MSw1LjM3OC0yLjUyN2MyLjE2LDAuMDIzLDMuODc0LDAuNjA4LDUuMTQxLDEuNzU4YzEuMjc4LDEuMTYsMS45MjksMi43NjQsMS45NSw0LjgxMQoJCQljMCwxLjE0Mi0wLjEzNywyLjExMS0wLjQxLDIuOTExYy0wLjMwOSwwLjg0NS0wLjczMSwxLjU5My0xLjI2OCwyLjI0M2MtMC40OTIsMC42NS0xLjA2OCwxLjMxOC0xLjczLDIuMDAyCgkJCWMtMC42NSwwLjY5Ny0xLjMxMywxLjQ3OS0xLjk4NywyLjM0NmMtMC4yMzksMC4zNzctMC40MjksMC43NzctMC41NjUsMS4xOTljLTAuMTYsMC45NTktMC4yMTcsMS45NTEtMC4xNzEsMi45NzkKCQkJQzI2LjU4OSwzMi4yMTgsMjMuNTc4LDMyLjIxOCwyMy41NzgsMzIuMjE4eiBNMjMuNTc4LDM4LjIydi0zLjQ4NGgzLjA3NnYzLjQ4NEgyMy41Nzh6Ii8+Cgk8L2c+Cjwvc3ZnPgo=);
  --jp-icon-markdown: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDIyIDIyIj4KICA8cGF0aCBjbGFzcz0ianAtaWNvbi1jb250cmFzdDAganAtaWNvbi1zZWxlY3RhYmxlIiBmaWxsPSIjN0IxRkEyIiBkPSJNNSAxNC45aDEybC02LjEgNnptOS40LTYuOGMwLTEuMy0uMS0yLjktLjEtNC41LS40IDEuNC0uOSAyLjktMS4zIDQuM2wtMS4zIDQuM2gtMkw4LjUgNy45Yy0uNC0xLjMtLjctMi45LTEtNC4zLS4xIDEuNi0uMSAzLjItLjIgNC42TDcgMTIuNEg0LjhsLjctMTFoMy4zTDEwIDVjLjQgMS4yLjcgMi43IDEgMy45LjMtMS4yLjctMi42IDEtMy45bDEuMi0zLjdoMy4zbC42IDExaC0yLjRsLS4zLTQuMnoiLz4KPC9zdmc+Cg==);
  --jp-icon-new-folder: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDI0IDI0Ij4KICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiPgogICAgPHBhdGggZD0iTTIwIDZoLThsLTItMkg0Yy0xLjExIDAtMS45OS44OS0xLjk5IDJMMiAxOGMwIDEuMTEuODkgMiAyIDJoMTZjMS4xMSAwIDItLjg5IDItMlY4YzAtMS4xMS0uODktMi0yLTJ6bS0xIDhoLTN2M2gtMnYtM2gtM3YtMmgzVjloMnYzaDN2MnoiLz4KICA8L2c+Cjwvc3ZnPgo=);
  --jp-icon-not-trusted: url(data:image/svg+xml;base64,PHN2ZyBmaWxsPSJub25lIiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDI1IDI1Ij4KICAgIDxwYXRoIGNsYXNzPSJqcC1pY29uMiIgc3Ryb2tlPSIjMzMzMzMzIiBzdHJva2Utd2lkdGg9IjIiIHRyYW5zZm9ybT0idHJhbnNsYXRlKDMgMykiIGQ9Ik0xLjg2MDk0IDExLjQ0MDlDMC44MjY0NDggOC43NzAyNyAwLjg2Mzc3OSA2LjA1NzY0IDEuMjQ5MDcgNC4xOTkzMkMyLjQ4MjA2IDMuOTMzNDcgNC4wODA2OCAzLjQwMzQ3IDUuNjAxMDIgMi44NDQ5QzcuMjM1NDkgMi4yNDQ0IDguODU2NjYgMS41ODE1IDkuOTg3NiAxLjA5NTM5QzExLjA1OTcgMS41ODM0MSAxMi42MDk0IDIuMjQ0NCAxNC4yMTggMi44NDMzOUMxNS43NTAzIDMuNDEzOTQgMTcuMzk5NSAzLjk1MjU4IDE4Ljc1MzkgNC4yMTM4NUMxOS4xMzY0IDYuMDcxNzcgMTkuMTcwOSA4Ljc3NzIyIDE4LjEzOSAxMS40NDA5QzE3LjAzMDMgMTQuMzAzMiAxNC42NjY4IDE3LjE4NDQgOS45OTk5OSAxOC45MzU0QzUuMzMzMTkgMTcuMTg0NCAyLjk2OTY4IDE0LjMwMzIgMS44NjA5NCAxMS40NDA5WiIvPgogICAgPHBhdGggY2xhc3M9ImpwLWljb24yIiBzdHJva2U9IiMzMzMzMzMiIHN0cm9rZS13aWR0aD0iMiIgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoOS4zMTU5MiA5LjMyMDMxKSIgZD0iTTcuMzY4NDIgMEwwIDcuMzY0NzkiLz4KICAgIDxwYXRoIGNsYXNzPSJqcC1pY29uMiIgc3Ryb2tlPSIjMzMzMzMzIiBzdHJva2Utd2lkdGg9IjIiIHRyYW5zZm9ybT0idHJhbnNsYXRlKDkuMzE1OTIgMTYuNjgzNikgc2NhbGUoMSAtMSkiIGQ9Ik03LjM2ODQyIDBMMCA3LjM2NDc5Ii8+Cjwvc3ZnPgo=);
  --jp-icon-notebook: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDIyIDIyIj4KICA8ZyBjbGFzcz0ianAtaWNvbi13YXJuMCBqcC1pY29uLXNlbGVjdGFibGUiIGZpbGw9IiNFRjZDMDAiPgogICAgPHBhdGggZD0iTTE4LjcgMy4zdjE1LjRIMy4zVjMuM2gxNS40bTEuNS0xLjVIMS44djE4LjNoMTguM2wuMS0xOC4zeiIvPgogICAgPHBhdGggZD0iTTE2LjUgMTYuNWwtNS40LTQuMy01LjYgNC4zdi0xMWgxMXoiLz4KICA8L2c+Cjwvc3ZnPgo=);
  --jp-icon-numbering: url(data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMjIiIGhlaWdodD0iMjIiIHZpZXdCb3g9IjAgMCAyOCAyOCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KCTxnIGNsYXNzPSJqcC1pY29uMyIgZmlsbD0iIzYxNjE2MSI+CgkJPHBhdGggZD0iTTQgMTlINlYxOS41SDVWMjAuNUg2VjIxSDRWMjJIN1YxOEg0VjE5Wk01IDEwSDZWNkg0VjdINVYxMFpNNCAxM0g1LjhMNCAxNS4xVjE2SDdWMTVINS4yTDcgMTIuOVYxMkg0VjEzWk05IDdWOUgyM1Y3SDlaTTkgMjFIMjNWMTlIOVYyMVpNOSAxNUgyM1YxM0g5VjE1WiIvPgoJPC9nPgo8L3N2Zz4K);
  --jp-icon-offline-bolt: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCAyNCAyNCIgd2lkdGg9IjE2Ij4KICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiPgogICAgPHBhdGggZD0iTTEyIDIuMDJjLTUuNTEgMC05Ljk4IDQuNDctOS45OCA5Ljk4czQuNDcgOS45OCA5Ljk4IDkuOTggOS45OC00LjQ3IDkuOTgtOS45OFMxNy41MSAyLjAyIDEyIDIuMDJ6TTExLjQ4IDIwdi02LjI2SDhMMTMgNHY2LjI2aDMuMzVMMTEuNDggMjB6Ii8+CiAgPC9nPgo8L3N2Zz4K);
  --jp-icon-palette: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDI0IDI0Ij4KICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiPgogICAgPHBhdGggZD0iTTE4IDEzVjIwSDRWNkg5LjAyQzkuMDcgNS4yOSA5LjI0IDQuNjIgOS41IDRINEMyLjkgNCAyIDQuOSAyIDZWMjBDMiAyMS4xIDIuOSAyMiA0IDIySDE4QzE5LjEgMjIgMjAgMjEuMSAyMCAyMFYxNUwxOCAxM1pNMTkuMyA4Ljg5QzE5Ljc0IDguMTkgMjAgNy4zOCAyMCA2LjVDMjAgNC4wMSAxNy45OSAyIDE1LjUgMkMxMy4wMSAyIDExIDQuMDEgMTEgNi41QzExIDguOTkgMTMuMDEgMTEgMTUuNDkgMTFDMTYuMzcgMTEgMTcuMTkgMTAuNzQgMTcuODggMTAuM0wyMSAxMy40MkwyMi40MiAxMkwxOS4zIDguODlaTTE1LjUgOUMxNC4xMiA5IDEzIDcuODggMTMgNi41QzEzIDUuMTIgMTQuMTIgNCAxNS41IDRDMTYuODggNCAxOCA1LjEyIDE4IDYuNUMxOCA3Ljg4IDE2Ljg4IDkgMTUuNSA5WiIvPgogICAgPHBhdGggZmlsbC1ydWxlPSJldmVub2RkIiBjbGlwLXJ1bGU9ImV2ZW5vZGQiIGQ9Ik00IDZIOS4wMTg5NEM5LjAwNjM5IDYuMTY1MDIgOSA2LjMzMTc2IDkgNi41QzkgOC44MTU3NyAxMC4yMTEgMTAuODQ4NyAxMi4wMzQzIDEySDlWMTRIMTZWMTIuOTgxMUMxNi41NzAzIDEyLjkzNzcgMTcuMTIgMTIuODIwNyAxNy42Mzk2IDEyLjYzOTZMMTggMTNWMjBINFY2Wk04IDhINlYxMEg4VjhaTTYgMTJIOFYxNEg2VjEyWk04IDE2SDZWMThIOFYxNlpNOSAxNkgxNlYxOEg5VjE2WiIvPgogIDwvZz4KPC9zdmc+Cg==);
  --jp-icon-paste: url(data:image/svg+xml;base64,PHN2ZyBoZWlnaHQ9IjI0IiB2aWV3Qm94PSIwIDAgMjQgMjQiIHdpZHRoPSIyNCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KICAgIDxnIGNsYXNzPSJqcC1pY29uMyIgZmlsbD0iIzYxNjE2MSI+CiAgICAgICAgPHBhdGggZD0iTTE5IDJoLTQuMThDMTQuNC44NCAxMy4zIDAgMTIgMGMtMS4zIDAtMi40Ljg0LTIuODIgMkg1Yy0xLjEgMC0yIC45LTIgMnYxNmMwIDEuMS45IDIgMiAyaDE0YzEuMSAwIDItLjkgMi0yVjRjMC0xLjEtLjktMi0yLTJ6bS03IDBjLjU1IDAgMSAuNDUgMSAxcy0uNDUgMS0xIDEtMS0uNDUtMS0xIC40NS0xIDEtMXptNyAxOEg1VjRoMnYzaDEwVjRoMnYxNnoiLz4KICAgIDwvZz4KPC9zdmc+Cg==);
  --jp-icon-pdf: url(data:image/svg+xml;base64,PHN2ZwogICB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCAyMiAyMiIgd2lkdGg9IjE2Ij4KICAgIDxwYXRoIHRyYW5zZm9ybT0icm90YXRlKDQ1KSIgY2xhc3M9ImpwLWljb24tc2VsZWN0YWJsZSIgZmlsbD0iI0ZGMkEyQSIKICAgICAgIGQ9Im0gMjIuMzQ0MzY5LC0zLjAxNjM2NDIgaCA1LjYzODYwNCB2IDEuNTc5MjQzMyBoIC0zLjU0OTIyNyB2IDEuNTA4NjkyOTkgaCAzLjMzNzU3NiBWIDEuNjUwODE1NCBoIC0zLjMzNzU3NiB2IDMuNDM1MjYxMyBoIC0yLjA4OTM3NyB6IG0gLTcuMTM2NDQ0LDEuNTc5MjQzMyB2IDQuOTQzOTU0MyBoIDAuNzQ4OTIgcSAxLjI4MDc2MSwwIDEuOTUzNzAzLC0wLjYzNDk1MzUgMC42NzgzNjksLTAuNjM0OTUzNSAwLjY3ODM2OSwtMS44NDUxNjQxIDAsLTEuMjA0NzgzNTUgLTAuNjcyOTQyLC0xLjgzNDMxMDExIC0wLjY3Mjk0MiwtMC42Mjk1MjY1OSAtMS45NTkxMywtMC42Mjk1MjY1OSB6IG0gLTIuMDg5Mzc3LC0xLjU3OTI0MzMgaCAyLjIwMzM0MyBxIDEuODQ1MTY0LDAgMi43NDYwMzksMC4yNjU5MjA3IDAuOTA2MzAxLDAuMjYwNDkzNyAxLjU1MjEwOCwwLjg5MDAyMDMgMC41Njk4MywwLjU0ODEyMjMgMC44NDY2MDUsMS4yNjQ0ODAwNiAwLjI3Njc3NCwwLjcxNjM1NzgxIDAuMjc2Nzc0LDEuNjIyNjU4OTQgMCwwLjkxNzE1NTEgLTAuMjc2Nzc0LDEuNjM4OTM5OSAtMC4yNzY3NzUsMC43MTYzNTc4IC0wLjg0NjYwNSwxLjI2NDQ4IC0wLjY1MTIzNCwwLjYyOTUyNjYgLTEuNTYyOTYyLDAuODk1NDQ3MyAtMC45MTE3MjgsMC4yNjA0OTM3IC0yLjczNTE4NSwwLjI2MDQ5MzcgaCAtMi4yMDMzNDMgeiBtIC04LjE0NTg1NjUsMCBoIDMuNDY3ODIzIHEgMS41NDY2ODE2LDAgMi4zNzE1Nzg1LDAuNjg5MjIzIDAuODMwMzI0LDAuNjgzNzk2MSAwLjgzMDMyNCwxLjk1MzcwMzE0IDAsMS4yNzUzMzM5NyAtMC44MzAzMjQsMS45NjQ1NTcwNiBRIDkuOTg3MTk2MSwyLjI3NDkxNSA4LjQ0MDUxNDUsMi4yNzQ5MTUgSCA3LjA2MjA2ODQgViA1LjA4NjA3NjcgSCA0Ljk3MjY5MTUgWiBtIDIuMDg5Mzc2OSwxLjUxNDExOTkgdiAyLjI2MzAzOTQzIGggMS4xNTU5NDEgcSAwLjYwNzgxODgsMCAwLjkzODg2MjksLTAuMjkzMDU1NDcgMC4zMzEwNDQxLC0wLjI5ODQ4MjQxIDAuMzMxMDQ0MSwtMC44NDExNzc3MiAwLC0wLjU0MjY5NTMxIC0wLjMzMTA0NDEsLTAuODM1NzUwNzQgLTAuMzMxMDQ0MSwtMC4yOTMwNTU1IC0wLjkzODg2MjksLTAuMjkzMDU1NSB6IgovPgo8L3N2Zz4K);
  --jp-icon-python: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDIyIDIyIj4KICA8ZyBjbGFzcz0ianAtaWNvbi1icmFuZDAganAtaWNvbi1zZWxlY3RhYmxlIiBmaWxsPSIjMEQ0N0ExIj4KICAgIDxwYXRoIGQ9Ik0xMS4xIDYuOVY1LjhINi45YzAtLjUgMC0xLjMuMi0xLjYuNC0uNy44LTEuMSAxLjctMS40IDEuNy0uMyAyLjUtLjMgMy45LS4xIDEgLjEgMS45LjkgMS45IDEuOXY0LjJjMCAuNS0uOSAxLjYtMiAxLjZIOC44Yy0xLjUgMC0yLjQgMS40LTIuNCAyLjh2Mi4ySDQuN0MzLjUgMTUuMSAzIDE0IDMgMTMuMVY5Yy0uMS0xIC42LTIgMS44LTIgMS41LS4xIDYuMy0uMSA2LjMtLjF6Ii8+CiAgICA8cGF0aCBkPSJNMTAuOSAxNS4xdjEuMWg0LjJjMCAuNSAwIDEuMy0uMiAxLjYtLjQuNy0uOCAxLjEtMS43IDEuNC0xLjcuMy0yLjUuMy0zLjkuMS0xLS4xLTEuOS0uOS0xLjktMS45di00LjJjMC0uNS45LTEuNiAyLTEuNmgzLjhjMS41IDAgMi40LTEuNCAyLjQtMi44VjYuNmgxLjdDMTguNSA2LjkgMTkgOCAxOSA4LjlWMTNjMCAxLS43IDIuMS0xLjkgMi4xaC02LjJ6Ii8+CiAgPC9nPgo8L3N2Zz4K);
  --jp-icon-r-kernel: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDIyIDIyIj4KICA8cGF0aCBjbGFzcz0ianAtaWNvbi1jb250cmFzdDMganAtaWNvbi1zZWxlY3RhYmxlIiBmaWxsPSIjMjE5NkYzIiBkPSJNNC40IDIuNWMxLjItLjEgMi45LS4zIDQuOS0uMyAyLjUgMCA0LjEuNCA1LjIgMS4zIDEgLjcgMS41IDEuOSAxLjUgMy41IDAgMi0xLjQgMy41LTIuOSA0LjEgMS4yLjQgMS43IDEuNiAyLjIgMyAuNiAxLjkgMSAzLjkgMS4zIDQuNmgtMy44Yy0uMy0uNC0uOC0xLjctMS4yLTMuN3MtMS4yLTIuNi0yLjYtMi42aC0uOXY2LjRINC40VjIuNXptMy43IDYuOWgxLjRjMS45IDAgMi45LS45IDIuOS0yLjNzLTEtMi4zLTIuOC0yLjNjLS43IDAtMS4zIDAtMS42LjJ2NC41aC4xdi0uMXoiLz4KPC9zdmc+Cg==);
  --jp-icon-react: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMTUwIDE1MCA1NDEuOSAyOTUuMyI+CiAgPGcgY2xhc3M9ImpwLWljb24tYnJhbmQyIGpwLWljb24tc2VsZWN0YWJsZSIgZmlsbD0iIzYxREFGQiI+CiAgICA8cGF0aCBkPSJNNjY2LjMgMjk2LjVjMC0zMi41LTQwLjctNjMuMy0xMDMuMS04Mi40IDE0LjQtNjMuNiA4LTExNC4yLTIwLjItMTMwLjQtNi41LTMuOC0xNC4xLTUuNi0yMi40LTUuNnYyMi4zYzQuNiAwIDguMy45IDExLjQgMi42IDEzLjYgNy44IDE5LjUgMzcuNSAxNC45IDc1LjctMS4xIDkuNC0yLjkgMTkuMy01LjEgMjkuNC0xOS42LTQuOC00MS04LjUtNjMuNS0xMC45LTEzLjUtMTguNS0yNy41LTM1LjMtNDEuNi01MCAzMi42LTMwLjMgNjMuMi00Ni45IDg0LTQ2LjlWNzhjLTI3LjUgMC02My41IDE5LjYtOTkuOSA1My42LTM2LjQtMzMuOC03Mi40LTUzLjItOTkuOS01My4ydjIyLjNjMjAuNyAwIDUxLjQgMTYuNSA4NCA0Ni42LTE0IDE0LjctMjggMzEuNC00MS4zIDQ5LjktMjIuNiAyLjQtNDQgNi4xLTYzLjYgMTEtMi4zLTEwLTQtMTkuNy01LjItMjktNC43LTM4LjIgMS4xLTY3LjkgMTQuNi03NS44IDMtMS44IDYuOS0yLjYgMTEuNS0yLjZWNzguNWMtOC40IDAtMTYgMS44LTIyLjYgNS42LTI4LjEgMTYuMi0zNC40IDY2LjctMTkuOSAxMzAuMS02Mi4yIDE5LjItMTAyLjcgNDkuOS0xMDIuNyA4Mi4zIDAgMzIuNSA0MC43IDYzLjMgMTAzLjEgODIuNC0xNC40IDYzLjYtOCAxMTQuMiAyMC4yIDEzMC40IDYuNSAzLjggMTQuMSA1LjYgMjIuNSA1LjYgMjcuNSAwIDYzLjUtMTkuNiA5OS45LTUzLjYgMzYuNCAzMy44IDcyLjQgNTMuMiA5OS45IDUzLjIgOC40IDAgMTYtMS44IDIyLjYtNS42IDI4LjEtMTYuMiAzNC40LTY2LjcgMTkuOS0xMzAuMSA2Mi0xOS4xIDEwMi41LTQ5LjkgMTAyLjUtODIuM3ptLTEzMC4yLTY2LjdjLTMuNyAxMi45LTguMyAyNi4yLTEzLjUgMzkuNS00LjEtOC04LjQtMTYtMTMuMS0yNC00LjYtOC05LjUtMTUuOC0xNC40LTIzLjQgMTQuMiAyLjEgMjcuOSA0LjcgNDEgNy45em0tNDUuOCAxMDYuNWMtNy44IDEzLjUtMTUuOCAyNi4zLTI0LjEgMzguMi0xNC45IDEuMy0zMCAyLTQ1LjIgMi0xNS4xIDAtMzAuMi0uNy00NS0xLjktOC4zLTExLjktMTYuNC0yNC42LTI0LjItMzgtNy42LTEzLjEtMTQuNS0yNi40LTIwLjgtMzkuOCA2LjItMTMuNCAxMy4yLTI2LjggMjAuNy0zOS45IDcuOC0xMy41IDE1LjgtMjYuMyAyNC4xLTM4LjIgMTQuOS0xLjMgMzAtMiA0NS4yLTIgMTUuMSAwIDMwLjIuNyA0NSAxLjkgOC4zIDExLjkgMTYuNCAyNC42IDI0LjIgMzggNy42IDEzLjEgMTQuNSAyNi40IDIwLjggMzkuOC02LjMgMTMuNC0xMy4yIDI2LjgtMjAuNyAzOS45em0zMi4zLTEzYzUuNCAxMy40IDEwIDI2LjggMTMuOCAzOS44LTEzLjEgMy4yLTI2LjkgNS45LTQxLjIgOCA0LjktNy43IDkuOC0xNS42IDE0LjQtMjMuNyA0LjYtOCA4LjktMTYuMSAxMy0yNC4xek00MjEuMiA0MzBjLTkuMy05LjYtMTguNi0yMC4zLTI3LjgtMzIgOSAuNCAxOC4yLjcgMjcuNS43IDkuNCAwIDE4LjctLjIgMjcuOC0uNy05IDExLjctMTguMyAyMi40LTI3LjUgMzJ6bS03NC40LTU4LjljLTE0LjItMi4xLTI3LjktNC43LTQxLTcuOSAzLjctMTIuOSA4LjMtMjYuMiAxMy41LTM5LjUgNC4xIDggOC40IDE2IDEzLjEgMjQgNC43IDggOS41IDE1LjggMTQuNCAyMy40ek00MjAuNyAxNjNjOS4zIDkuNiAxOC42IDIwLjMgMjcuOCAzMi05LS40LTE4LjItLjctMjcuNS0uNy05LjQgMC0xOC43LjItMjcuOC43IDktMTEuNyAxOC4zLTIyLjQgMjcuNS0zMnptLTc0IDU4LjljLTQuOSA3LjctOS44IDE1LjYtMTQuNCAyMy43LTQuNiA4LTguOSAxNi0xMyAyNC01LjQtMTMuNC0xMC0yNi44LTEzLjgtMzkuOCAxMy4xLTMuMSAyNi45LTUuOCA0MS4yLTcuOXptLTkwLjUgMTI1LjJjLTM1LjQtMTUuMS01OC4zLTM0LjktNTguMy01MC42IDAtMTUuNyAyMi45LTM1LjYgNTguMy01MC42IDguNi0zLjcgMTgtNyAyNy43LTEwLjEgNS43IDE5LjYgMTMuMiA0MCAyMi41IDYwLjktOS4yIDIwLjgtMTYuNiA0MS4xLTIyLjIgNjAuNi05LjktMy4xLTE5LjMtNi41LTI4LTEwLjJ6TTMxMCA0OTBjLTEzLjYtNy44LTE5LjUtMzcuNS0xNC45LTc1LjcgMS4xLTkuNCAyLjktMTkuMyA1LjEtMjkuNCAxOS42IDQuOCA0MSA4LjUgNjMuNSAxMC45IDEzLjUgMTguNSAyNy41IDM1LjMgNDEuNiA1MC0zMi42IDMwLjMtNjMuMiA0Ni45LTg0IDQ2LjktNC41LS4xLTguMy0xLTExLjMtMi43em0yMzcuMi03Ni4yYzQuNyAzOC4yLTEuMSA2Ny45LTE0LjYgNzUuOC0zIDEuOC02LjkgMi42LTExLjUgMi42LTIwLjcgMC01MS40LTE2LjUtODQtNDYuNiAxNC0xNC43IDI4LTMxLjQgNDEuMy00OS45IDIyLjYtMi40IDQ0LTYuMSA2My42LTExIDIuMyAxMC4xIDQuMSAxOS44IDUuMiAyOS4xem0zOC41LTY2LjdjLTguNiAzLjctMTggNy0yNy43IDEwLjEtNS43LTE5LjYtMTMuMi00MC0yMi41LTYwLjkgOS4yLTIwLjggMTYuNi00MS4xIDIyLjItNjAuNiA5LjkgMy4xIDE5LjMgNi41IDI4LjEgMTAuMiAzNS40IDE1LjEgNTguMyAzNC45IDU4LjMgNTAuNi0uMSAxNS43LTIzIDM1LjYtNTguNCA1MC42ek0zMjAuOCA3OC40eiIvPgogICAgPGNpcmNsZSBjeD0iNDIwLjkiIGN5PSIyOTYuNSIgcj0iNDUuNyIvPgogIDwvZz4KPC9zdmc+Cg==);
  --jp-icon-redo: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIGhlaWdodD0iMjQiIHZpZXdCb3g9IjAgMCAyNCAyNCIgd2lkdGg9IjE2Ij4KICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiPgogICAgICA8cGF0aCBkPSJNMCAwaDI0djI0SDB6IiBmaWxsPSJub25lIi8+PHBhdGggZD0iTTE4LjQgMTAuNkMxNi41NSA4Ljk5IDE0LjE1IDggMTEuNSA4Yy00LjY1IDAtOC41OCAzLjAzLTkuOTYgNy4yMkwzLjkgMTZjMS4wNS0zLjE5IDQuMDUtNS41IDcuNi01LjUgMS45NSAwIDMuNzMuNzIgNS4xMiAxLjg4TDEzIDE2aDlWN2wtMy42IDMuNnoiLz4KICA8L2c+Cjwvc3ZnPgo=);
  --jp-icon-refresh: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDE4IDE4Ij4KICAgIDxnIGNsYXNzPSJqcC1pY29uMyIgZmlsbD0iIzYxNjE2MSI+CiAgICAgICAgPHBhdGggZD0iTTkgMTMuNWMtMi40OSAwLTQuNS0yLjAxLTQuNS00LjVTNi41MSA0LjUgOSA0LjVjMS4yNCAwIDIuMzYuNTIgMy4xNyAxLjMzTDEwIDhoNVYzbC0xLjc2IDEuNzZDMTIuMTUgMy42OCAxMC42NiAzIDkgMyA1LjY5IDMgMy4wMSA1LjY5IDMuMDEgOVM1LjY5IDE1IDkgMTVjMi45NyAwIDUuNDMtMi4xNiA1LjktNWgtMS41MmMtLjQ2IDItMi4yNCAzLjUtNC4zOCAzLjV6Ii8+CiAgICA8L2c+Cjwvc3ZnPgo=);
  --jp-icon-regex: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDIwIDIwIj4KICA8ZyBjbGFzcz0ianAtaWNvbjIiIGZpbGw9IiM0MTQxNDEiPgogICAgPHJlY3QgeD0iMiIgeT0iMiIgd2lkdGg9IjE2IiBoZWlnaHQ9IjE2Ii8+CiAgPC9nPgoKICA8ZyBjbGFzcz0ianAtaWNvbi1hY2NlbnQyIiBmaWxsPSIjRkZGIj4KICAgIDxjaXJjbGUgY2xhc3M9InN0MiIgY3g9IjUuNSIgY3k9IjE0LjUiIHI9IjEuNSIvPgogICAgPHJlY3QgeD0iMTIiIHk9IjQiIGNsYXNzPSJzdDIiIHdpZHRoPSIxIiBoZWlnaHQ9IjgiLz4KICAgIDxyZWN0IHg9IjguNSIgeT0iNy41IiB0cmFuc2Zvcm09Im1hdHJpeCgwLjg2NiAtMC41IDAuNSAwLjg2NiAtMi4zMjU1IDcuMzIxOSkiIGNsYXNzPSJzdDIiIHdpZHRoPSI4IiBoZWlnaHQ9IjEiLz4KICAgIDxyZWN0IHg9IjEyIiB5PSI0IiB0cmFuc2Zvcm09Im1hdHJpeCgwLjUgLTAuODY2IDAuODY2IDAuNSAtMC42Nzc5IDE0LjgyNTIpIiBjbGFzcz0ic3QyIiB3aWR0aD0iMSIgaGVpZ2h0PSI4Ii8+CiAgPC9nPgo8L3N2Zz4K);
  --jp-icon-run: url(data:image/svg+xml;base64,PHN2ZyBoZWlnaHQ9IjI0IiB2aWV3Qm94PSIwIDAgMjQgMjQiIHdpZHRoPSIyNCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KICAgIDxnIGNsYXNzPSJqcC1pY29uMyIgZmlsbD0iIzYxNjE2MSI+CiAgICAgICAgPHBhdGggZD0iTTggNXYxNGwxMS03eiIvPgogICAgPC9nPgo8L3N2Zz4K);
  --jp-icon-running: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDUxMiA1MTIiPgogIDxnIGNsYXNzPSJqcC1pY29uMyIgZmlsbD0iIzYxNjE2MSI+CiAgICA8cGF0aCBkPSJNMjU2IDhDMTE5IDggOCAxMTkgOCAyNTZzMTExIDI0OCAyNDggMjQ4IDI0OC0xMTEgMjQ4LTI0OFMzOTMgOCAyNTYgOHptOTYgMzI4YzAgOC44LTcuMiAxNi0xNiAxNkgxNzZjLTguOCAwLTE2LTcuMi0xNi0xNlYxNzZjMC04LjggNy4yLTE2IDE2LTE2aDE2MGM4LjggMCAxNiA3LjIgMTYgMTZ2MTYweiIvPgogIDwvZz4KPC9zdmc+Cg==);
  --jp-icon-save: url(data:image/svg+xml;base64,PHN2ZyBoZWlnaHQ9IjI0IiB2aWV3Qm94PSIwIDAgMjQgMjQiIHdpZHRoPSIyNCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KICAgIDxnIGNsYXNzPSJqcC1pY29uMyIgZmlsbD0iIzYxNjE2MSI+CiAgICAgICAgPHBhdGggZD0iTTE3IDNINWMtMS4xMSAwLTIgLjktMiAydjE0YzAgMS4xLjg5IDIgMiAyaDE0YzEuMSAwIDItLjkgMi0yVjdsLTQtNHptLTUgMTZjLTEuNjYgMC0zLTEuMzQtMy0zczEuMzQtMyAzLTMgMyAxLjM0IDMgMy0xLjM0IDMtMyAzem0zLTEwSDVWNWgxMHY0eiIvPgogICAgPC9nPgo8L3N2Zz4K);
  --jp-icon-search: url(data:image/svg+xml;base64,PHN2ZyB2aWV3Qm94PSIwIDAgMTggMTgiIHdpZHRoPSIxNiIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiPgogICAgPHBhdGggZD0iTTEyLjEsMTAuOWgtMC43bC0wLjItMC4yYzAuOC0wLjksMS4zLTIuMiwxLjMtMy41YzAtMy0yLjQtNS40LTUuNC01LjRTMS44LDQuMiwxLjgsNy4xczIuNCw1LjQsNS40LDUuNCBjMS4zLDAsMi41LTAuNSwzLjUtMS4zbDAuMiwwLjJ2MC43bDQuMSw0LjFsMS4yLTEuMkwxMi4xLDEwLjl6IE03LjEsMTAuOWMtMi4xLDAtMy43LTEuNy0zLjctMy43czEuNy0zLjcsMy43LTMuN3MzLjcsMS43LDMuNywzLjcgUzkuMiwxMC45LDcuMSwxMC45eiIvPgogIDwvZz4KPC9zdmc+Cg==);
  --jp-icon-settings: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDI0IDI0Ij4KICA8cGF0aCBjbGFzcz0ianAtaWNvbjMganAtaWNvbi1zZWxlY3RhYmxlIiBmaWxsPSIjNjE2MTYxIiBkPSJNMTkuNDMgMTIuOThjLjA0LS4zMi4wNy0uNjQuMDctLjk4cy0uMDMtLjY2LS4wNy0uOThsMi4xMS0xLjY1Yy4xOS0uMTUuMjQtLjQyLjEyLS42NGwtMi0zLjQ2Yy0uMTItLjIyLS4zOS0uMy0uNjEtLjIybC0yLjQ5IDFjLS41Mi0uNC0xLjA4LS43My0xLjY5LS45OGwtLjM4LTIuNjVBLjQ4OC40ODggMCAwMDE0IDJoLTRjLS4yNSAwLS40Ni4xOC0uNDkuNDJsLS4zOCAyLjY1Yy0uNjEuMjUtMS4xNy41OS0xLjY5Ljk4bC0yLjQ5LTFjLS4yMy0uMDktLjQ5IDAtLjYxLjIybC0yIDMuNDZjLS4xMy4yMi0uMDcuNDkuMTIuNjRsMi4xMSAxLjY1Yy0uMDQuMzItLjA3LjY1LS4wNy45OHMuMDMuNjYuMDcuOThsLTIuMTEgMS42NWMtLjE5LjE1LS4yNC40Mi0uMTIuNjRsMiAzLjQ2Yy4xMi4yMi4zOS4zLjYxLjIybDIuNDktMWMuNTIuNCAxLjA4LjczIDEuNjkuOThsLjM4IDIuNjVjLjAzLjI0LjI0LjQyLjQ5LjQyaDRjLjI1IDAgLjQ2LS4xOC40OS0uNDJsLjM4LTIuNjVjLjYxLS4yNSAxLjE3LS41OSAxLjY5LS45OGwyLjQ5IDFjLjIzLjA5LjQ5IDAgLjYxLS4yMmwyLTMuNDZjLjEyLS4yMi4wNy0uNDktLjEyLS42NGwtMi4xMS0xLjY1ek0xMiAxNS41Yy0xLjkzIDAtMy41LTEuNTctMy41LTMuNXMxLjU3LTMuNSAzLjUtMy41IDMuNSAxLjU3IDMuNSAzLjUtMS41NyAzLjUtMy41IDMuNXoiLz4KPC9zdmc+Cg==);
  --jp-icon-spreadsheet: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDIyIDIyIj4KICA8cGF0aCBjbGFzcz0ianAtaWNvbi1jb250cmFzdDEganAtaWNvbi1zZWxlY3RhYmxlIiBmaWxsPSIjNENBRjUwIiBkPSJNMi4yIDIuMnYxNy42aDE3LjZWMi4ySDIuMnptMTUuNCA3LjdoLTUuNVY0LjRoNS41djUuNXpNOS45IDQuNHY1LjVINC40VjQuNGg1LjV6bS01LjUgNy43aDUuNXY1LjVINC40di01LjV6bTcuNyA1LjV2LTUuNWg1LjV2NS41aC01LjV6Ii8+Cjwvc3ZnPgo=);
  --jp-icon-stop: url(data:image/svg+xml;base64,PHN2ZyBoZWlnaHQ9IjI0IiB2aWV3Qm94PSIwIDAgMjQgMjQiIHdpZHRoPSIyNCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KICAgIDxnIGNsYXNzPSJqcC1pY29uMyIgZmlsbD0iIzYxNjE2MSI+CiAgICAgICAgPHBhdGggZD0iTTAgMGgyNHYyNEgweiIgZmlsbD0ibm9uZSIvPgogICAgICAgIDxwYXRoIGQ9Ik02IDZoMTJ2MTJINnoiLz4KICAgIDwvZz4KPC9zdmc+Cg==);
  --jp-icon-tab: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDI0IDI0Ij4KICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiPgogICAgPHBhdGggZD0iTTIxIDNIM2MtMS4xIDAtMiAuOS0yIDJ2MTRjMCAxLjEuOSAyIDIgMmgxOGMxLjEgMCAyLS45IDItMlY1YzAtMS4xLS45LTItMi0yem0wIDE2SDNWNWgxMHY0aDh2MTB6Ii8+CiAgPC9nPgo8L3N2Zz4K);
  --jp-icon-table-rows: url(data:image/svg+xml;base64,PHN2ZyBoZWlnaHQ9IjI0IiB2aWV3Qm94PSIwIDAgMjQgMjQiIHdpZHRoPSIyNCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KICAgIDxnIGNsYXNzPSJqcC1pY29uMyIgZmlsbD0iIzYxNjE2MSI+CiAgICAgICAgPHBhdGggZD0iTTAgMGgyNHYyNEgweiIgZmlsbD0ibm9uZSIvPgogICAgICAgIDxwYXRoIGQ9Ik0yMSw4SDNWNGgxOFY4eiBNMjEsMTBIM3Y0aDE4VjEweiBNMjEsMTZIM3Y0aDE4VjE2eiIvPgogICAgPC9nPgo8L3N2Zz4=);
  --jp-icon-tag: url(data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMjgiIGhlaWdodD0iMjgiIHZpZXdCb3g9IjAgMCA0MyAyOCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KCTxnIGNsYXNzPSJqcC1pY29uMyIgZmlsbD0iIzYxNjE2MSI+CgkJPHBhdGggZD0iTTI4LjgzMzIgMTIuMzM0TDMyLjk5OTggMTYuNTAwN0wzNy4xNjY1IDEyLjMzNEgyOC44MzMyWiIvPgoJCTxwYXRoIGQ9Ik0xNi4yMDk1IDIxLjYxMDRDMTUuNjg3MyAyMi4xMjk5IDE0Ljg0NDMgMjIuMTI5OSAxNC4zMjQ4IDIxLjYxMDRMNi45ODI5IDE0LjcyNDVDNi41NzI0IDE0LjMzOTQgNi4wODMxMyAxMy42MDk4IDYuMDQ3ODYgMTMuMDQ4MkM1Ljk1MzQ3IDExLjUyODggNi4wMjAwMiA4LjYxOTQ0IDYuMDY2MjEgNy4wNzY5NUM2LjA4MjgxIDYuNTE0NzcgNi41NTU0OCA2LjA0MzQ3IDcuMTE4MDQgNi4wMzA1NUM5LjA4ODYzIDUuOTg0NzMgMTMuMjYzOCA1LjkzNTc5IDEzLjY1MTggNi4zMjQyNUwyMS43MzY5IDEzLjYzOUMyMi4yNTYgMTQuMTU4NSAyMS43ODUxIDE1LjQ3MjQgMjEuMjYyIDE1Ljk5NDZMMTYuMjA5NSAyMS42MTA0Wk05Ljc3NTg1IDguMjY1QzkuMzM1NTEgNy44MjU2NiA4LjYyMzUxIDcuODI1NjYgOC4xODI4IDguMjY1QzcuNzQzNDYgOC43MDU3MSA3Ljc0MzQ2IDkuNDE3MzMgOC4xODI4IDkuODU2NjdDOC42MjM4MiAxMC4yOTY0IDkuMzM1ODIgMTAuMjk2NCA5Ljc3NTg1IDkuODU2NjdDMTAuMjE1NiA5LjQxNzMzIDEwLjIxNTYgOC43MDUzMyA5Ljc3NTg1IDguMjY1WiIvPgoJPC9nPgo8L3N2Zz4K);
  --jp-icon-terminal: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDI0IDI0IiA+CiAgICA8cmVjdCBjbGFzcz0ianAtaWNvbjIganAtaWNvbi1zZWxlY3RhYmxlIiB3aWR0aD0iMjAiIGhlaWdodD0iMjAiIHRyYW5zZm9ybT0idHJhbnNsYXRlKDIgMikiIGZpbGw9IiMzMzMzMzMiLz4KICAgIDxwYXRoIGNsYXNzPSJqcC1pY29uLWFjY2VudDIganAtaWNvbi1zZWxlY3RhYmxlLWludmVyc2UiIGQ9Ik01LjA1NjY0IDguNzYxNzJDNS4wNTY2NCA4LjU5NzY2IDUuMDMxMjUgOC40NTMxMiA0Ljk4MDQ3IDguMzI4MTJDNC45MzM1OSA4LjE5OTIyIDQuODU1NDcgOC4wODIwMyA0Ljc0NjA5IDcuOTc2NTZDNC42NDA2MiA3Ljg3MTA5IDQuNSA3Ljc3NTM5IDQuMzI0MjIgNy42ODk0NUM0LjE1MjM0IDcuNTk5NjEgMy45NDMzNiA3LjUxMTcyIDMuNjk3MjcgNy40MjU3OEMzLjMwMjczIDcuMjg1MTYgMi45NDMzNiA3LjEzNjcyIDIuNjE5MTQgNi45ODA0N0MyLjI5NDkyIDYuODI0MjIgMi4wMTc1OCA2LjY0MjU4IDEuNzg3MTEgNi40MzU1NUMxLjU2MDU1IDYuMjI4NTIgMS4zODQ3NyA1Ljk4ODI4IDEuMjU5NzcgNS43MTQ4NEMxLjEzNDc3IDUuNDM3NSAxLjA3MjI3IDUuMTA5MzggMS4wNzIyNyA0LjczMDQ3QzEuMDcyMjcgNC4zOTg0NCAxLjEyODkxIDQuMDk1NyAxLjI0MjE5IDMuODIyMjdDMS4zNTU0NyAzLjU0NDkyIDEuNTE1NjIgMy4zMDQ2OSAxLjcyMjY2IDMuMTAxNTZDMS45Mjk2OSAyLjg5ODQ0IDIuMTc5NjkgMi43MzQzNyAyLjQ3MjY2IDIuNjA5MzhDMi43NjU2MiAyLjQ4NDM4IDMuMDkxOCAyLjQwNDMgMy40NTExNyAyLjM2OTE0VjEuMTA5MzhINC4zODg2N1YyLjM4MDg2QzQuNzQwMjMgMi40Mjc3MyA1LjA1NjY0IDIuNTIzNDQgNS4zMzc4OSAyLjY2Nzk3QzUuNjE5MTQgMi44MTI1IDUuODU3NDIgMy4wMDE5NSA2LjA1MjczIDMuMjM2MzNDNi4yNTE5NSAzLjQ2NjggNi40MDQzIDMuNzQwMjMgNi41MDk3NyA0LjA1NjY0QzYuNjE5MTQgNC4zNjkxNCA2LjY3MzgzIDQuNzIwNyA2LjY3MzgzIDUuMTExMzNINS4wNDQ5MkM1LjA0NDkyIDQuNjM4NjcgNC45Mzc1IDQuMjgxMjUgNC43MjI2NiA0LjAzOTA2QzQuNTA3ODEgMy43OTI5NyA0LjIxNjggMy42Njk5MiAzLjg0OTYxIDMuNjY5OTJDMy42NTAzOSAzLjY2OTkyIDMuNDc2NTYgMy42OTcyNyAzLjMyODEyIDMuNzUxOTVDMy4xODM1OSAzLjgwMjczIDMuMDY0NDUgMy44NzY5NSAyLjk3MDcgMy45NzQ2MUMyLjg3Njk1IDQuMDY4MzYgMi44MDY2NCA0LjE3OTY5IDIuNzU5NzcgNC4zMDg1OUMyLjcxNjggNC40Mzc1IDIuNjk1MzEgNC41NzgxMiAyLjY5NTMxIDQuNzMwNDdDMi42OTUzMSA0Ljg4MjgxIDIuNzE2OCA1LjAxOTUzIDIuNzU5NzcgNS4xNDA2MkMyLjgwNjY0IDUuMjU3ODEgMi44ODI4MSA1LjM2NzE5IDIuOTg4MjggNS40Njg3NUMzLjA5NzY2IDUuNTcwMzEgMy4yNDAyMyA1LjY2Nzk3IDMuNDE2MDIgNS43NjE3MkMzLjU5MTggNS44NTE1NiAzLjgxMDU1IDUuOTQzMzYgNC4wNzIyNyA2LjAzNzExQzQuNDY2OCA2LjE4NTU1IDQuODI0MjIgNi4zMzk4NCA1LjE0NDUzIDYuNUM1LjQ2NDg0IDYuNjU2MjUgNS43MzgyOCA2LjgzOTg0IDUuOTY0ODQgNy4wNTA3OEM2LjE5NTMxIDcuMjU3ODEgNi4zNzEwOSA3LjUgNi40OTIxOSA3Ljc3NzM0QzYuNjE3MTkgOC4wNTA3OCA2LjY3OTY5IDguMzc1IDYuNjc5NjkgOC43NUM2LjY3OTY5IDkuMDkzNzUgNi42MjMwNSA5LjQwNDMgNi41MDk3NyA5LjY4MTY0QzYuMzk2NDggOS45NTUwOCA2LjIzNDM4IDEwLjE5MTQgNi4wMjM0NCAxMC4zOTA2QzUuODEyNSAxMC41ODk4IDUuNTU4NTkgMTAuNzUgNS4yNjE3MiAxMC44NzExQzQuOTY0ODQgMTAuOTg4MyA0LjYzMjgxIDExLjA2NDUgNC4yNjU2MiAxMS4wOTk2VjEyLjI0OEgzLjMzMzk4VjExLjA5OTZDMy4wMDE5NSAxMS4wNjg0IDIuNjc5NjkgMTAuOTk2MSAyLjM2NzE5IDEwLjg4MjhDMi4wNTQ2OSAxMC43NjU2IDEuNzc3MzQgMTAuNTk3NyAxLjUzNTE2IDEwLjM3ODlDMS4yOTY4OCAxMC4xNjAyIDEuMTA1NDcgOS44ODQ3NyAwLjk2MDkzOCA5LjU1MjczQzAuODE2NDA2IDkuMjE2OCAwLjc0NDE0MSA4LjgxNDQ1IDAuNzQ0MTQxIDguMzQ1N0gyLjM3ODkxQzIuMzc4OTEgOC42MjY5NSAyLjQxOTkyIDguODYzMjggMi41MDE5NSA5LjA1NDY5QzIuNTgzOTggOS4yNDIxOSAyLjY4OTQ1IDkuMzkyNTggMi44MTgzNiA5LjUwNTg2QzIuOTUxMTcgOS42MTUyMyAzLjEwMTU2IDkuNjkzMzYgMy4yNjk1MyA5Ljc0MDIzQzMuNDM3NSA5Ljc4NzExIDMuNjA5MzggOS44MTA1NSAzLjc4NTE2IDkuODEwNTVDNC4yMDMxMiA5LjgxMDU1IDQuNTE5NTMgOS43MTI4OSA0LjczNDM4IDkuNTE3NThDNC45NDkyMiA5LjMyMjI3IDUuMDU2NjQgOS4wNzAzMSA1LjA1NjY0IDguNzYxNzJaTTEzLjQxOCAxMi4yNzE1SDguMDc0MjJWMTFIMTMuNDE4VjEyLjI3MTVaIiB0cmFuc2Zvcm09InRyYW5zbGF0ZSgzLjk1MjY0IDYpIiBmaWxsPSJ3aGl0ZSIvPgo8L3N2Zz4K);
  --jp-icon-text-editor: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDI0IDI0Ij4KICA8cGF0aCBjbGFzcz0ianAtaWNvbjMganAtaWNvbi1zZWxlY3RhYmxlIiBmaWxsPSIjNjE2MTYxIiBkPSJNMTUgMTVIM3YyaDEydi0yem0wLThIM3YyaDEyVjd6TTMgMTNoMTh2LTJIM3Yyem0wIDhoMTh2LTJIM3Yyek0zIDN2MmgxOFYzSDN6Ii8+Cjwvc3ZnPgo=);
  --jp-icon-toc: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIyNCIgaGVpZ2h0PSIyNCIgdmlld0JveD0iMCAwIDI0IDI0Ij4KICA8ZyBjbGFzcz0ianAtaWNvbjMganAtaWNvbi1zZWxlY3RhYmxlIiBmaWxsPSIjNjE2MTYxIj4KICAgIDxwYXRoIGQ9Ik03LDVIMjFWN0g3VjVNNywxM1YxMUgyMVYxM0g3TTQsNC41QTEuNSwxLjUgMCAwLDEgNS41LDZBMS41LDEuNSAwIDAsMSA0LDcuNUExLjUsMS41IDAgMCwxIDIuNSw2QTEuNSwxLjUgMCAwLDEgNCw0LjVNNCwxMC41QTEuNSwxLjUgMCAwLDEgNS41LDEyQTEuNSwxLjUgMCAwLDEgNCwxMy41QTEuNSwxLjUgMCAwLDEgMi41LDEyQTEuNSwxLjUgMCAwLDEgNCwxMC41TTcsMTlWMTdIMjFWMTlIN000LDE2LjVBMS41LDEuNSAwIDAsMSA1LjUsMThBMS41LDEuNSAwIDAsMSA0LDE5LjVBMS41LDEuNSAwIDAsMSAyLjUsMThBMS41LDEuNSAwIDAsMSA0LDE2LjVaIiAvPgogIDwvZz4KPC9zdmc+Cg==);
  --jp-icon-tree-view: url(data:image/svg+xml;base64,PHN2ZyBoZWlnaHQ9IjI0IiB2aWV3Qm94PSIwIDAgMjQgMjQiIHdpZHRoPSIyNCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KICAgIDxnIGNsYXNzPSJqcC1pY29uMyIgZmlsbD0iIzYxNjE2MSI+CiAgICAgICAgPHBhdGggZD0iTTAgMGgyNHYyNEgweiIgZmlsbD0ibm9uZSIvPgogICAgICAgIDxwYXRoIGQ9Ik0yMiAxMVYzaC03djNIOVYzSDJ2OGg3VjhoMnYxMGg0djNoN3YtOGgtN3YzaC0yVjhoMnYzeiIvPgogICAgPC9nPgo8L3N2Zz4=);
  --jp-icon-trusted: url(data:image/svg+xml;base64,PHN2ZyBmaWxsPSJub25lIiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDI0IDI1Ij4KICAgIDxwYXRoIGNsYXNzPSJqcC1pY29uMiIgc3Ryb2tlPSIjMzMzMzMzIiBzdHJva2Utd2lkdGg9IjIiIHRyYW5zZm9ybT0idHJhbnNsYXRlKDIgMykiIGQ9Ik0xLjg2MDk0IDExLjQ0MDlDMC44MjY0NDggOC43NzAyNyAwLjg2Mzc3OSA2LjA1NzY0IDEuMjQ5MDcgNC4xOTkzMkMyLjQ4MjA2IDMuOTMzNDcgNC4wODA2OCAzLjQwMzQ3IDUuNjAxMDIgMi44NDQ5QzcuMjM1NDkgMi4yNDQ0IDguODU2NjYgMS41ODE1IDkuOTg3NiAxLjA5NTM5QzExLjA1OTcgMS41ODM0MSAxMi42MDk0IDIuMjQ0NCAxNC4yMTggMi44NDMzOUMxNS43NTAzIDMuNDEzOTQgMTcuMzk5NSAzLjk1MjU4IDE4Ljc1MzkgNC4yMTM4NUMxOS4xMzY0IDYuMDcxNzcgMTkuMTcwOSA4Ljc3NzIyIDE4LjEzOSAxMS40NDA5QzE3LjAzMDMgMTQuMzAzMiAxNC42NjY4IDE3LjE4NDQgOS45OTk5OSAxOC45MzU0QzUuMzMzMiAxNy4xODQ0IDIuOTY5NjggMTQuMzAzMiAxLjg2MDk0IDExLjQ0MDlaIi8+CiAgICA8cGF0aCBjbGFzcz0ianAtaWNvbjIiIGZpbGw9IiMzMzMzMzMiIHN0cm9rZT0iIzMzMzMzMyIgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoOCA5Ljg2NzE5KSIgZD0iTTIuODYwMTUgNC44NjUzNUwwLjcyNjU0OSAyLjk5OTU5TDAgMy42MzA0NUwyLjg2MDE1IDYuMTMxNTdMOCAwLjYzMDg3Mkw3LjI3ODU3IDBMMi44NjAxNSA0Ljg2NTM1WiIvPgo8L3N2Zz4K);
  --jp-icon-undo: url(data:image/svg+xml;base64,PHN2ZyB2aWV3Qm94PSIwIDAgMjQgMjQiIHdpZHRoPSIxNiIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiPgogICAgPHBhdGggZD0iTTEyLjUgOGMtMi42NSAwLTUuMDUuOTktNi45IDIuNkwyIDd2OWg5bC0zLjYyLTMuNjJjMS4zOS0xLjE2IDMuMTYtMS44OCA1LjEyLTEuODggMy41NCAwIDYuNTUgMi4zMSA3LjYgNS41bDIuMzctLjc4QzIxLjA4IDExLjAzIDE3LjE1IDggMTIuNSA4eiIvPgogIDwvZz4KPC9zdmc+Cg==);
  --jp-icon-vega: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDIyIDIyIj4KICA8ZyBjbGFzcz0ianAtaWNvbjEganAtaWNvbi1zZWxlY3RhYmxlIiBmaWxsPSIjMjEyMTIxIj4KICAgIDxwYXRoIGQ9Ik0xMC42IDUuNGwyLjItMy4ySDIuMnY3LjNsNC02LjZ6Ii8+CiAgICA8cGF0aCBkPSJNMTUuOCAyLjJsLTQuNCA2LjZMNyA2LjNsLTQuOCA4djUuNWgxNy42VjIuMmgtNHptLTcgMTUuNEg1LjV2LTQuNGgzLjN2NC40em00LjQgMEg5LjhWOS44aDMuNHY3Ljh6bTQuNCAwaC0zLjRWNi41aDMuNHYxMS4xeiIvPgogIDwvZz4KPC9zdmc+Cg==);
  --jp-icon-yaml: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDIyIDIyIj4KICA8ZyBjbGFzcz0ianAtaWNvbi1jb250cmFzdDIganAtaWNvbi1zZWxlY3RhYmxlIiBmaWxsPSIjRDgxQjYwIj4KICAgIDxwYXRoIGQ9Ik03LjIgMTguNnYtNS40TDMgNS42aDMuM2wxLjQgMy4xYy4zLjkuNiAxLjYgMSAyLjUuMy0uOC42LTEuNiAxLTIuNWwxLjQtMy4xaDMuNGwtNC40IDcuNnY1LjVsLTIuOS0uMXoiLz4KICAgIDxjaXJjbGUgY2xhc3M9InN0MCIgY3g9IjE3LjYiIGN5PSIxNi41IiByPSIyLjEiLz4KICAgIDxjaXJjbGUgY2xhc3M9InN0MCIgY3g9IjE3LjYiIGN5PSIxMSIgcj0iMi4xIi8+CiAgPC9nPgo8L3N2Zz4K);
}

/* Icon CSS class declarations */

.jp-AddIcon {
  background-image: var(--jp-icon-add);
}
.jp-BugIcon {
  background-image: var(--jp-icon-bug);
}
.jp-BuildIcon {
  background-image: var(--jp-icon-build);
}
.jp-CaretDownEmptyIcon {
  background-image: var(--jp-icon-caret-down-empty);
}
.jp-CaretDownEmptyThinIcon {
  background-image: var(--jp-icon-caret-down-empty-thin);
}
.jp-CaretDownIcon {
  background-image: var(--jp-icon-caret-down);
}
.jp-CaretLeftIcon {
  background-image: var(--jp-icon-caret-left);
}
.jp-CaretRightIcon {
  background-image: var(--jp-icon-caret-right);
}
.jp-CaretUpEmptyThinIcon {
  background-image: var(--jp-icon-caret-up-empty-thin);
}
.jp-CaretUpIcon {
  background-image: var(--jp-icon-caret-up);
}
.jp-CaseSensitiveIcon {
  background-image: var(--jp-icon-case-sensitive);
}
.jp-CheckIcon {
  background-image: var(--jp-icon-check);
}
.jp-CircleEmptyIcon {
  background-image: var(--jp-icon-circle-empty);
}
.jp-CircleIcon {
  background-image: var(--jp-icon-circle);
}
.jp-ClearIcon {
  background-image: var(--jp-icon-clear);
}
.jp-CloseIcon {
  background-image: var(--jp-icon-close);
}
.jp-CodeIcon {
  background-image: var(--jp-icon-code);
}
.jp-ConsoleIcon {
  background-image: var(--jp-icon-console);
}
.jp-CopyIcon {
  background-image: var(--jp-icon-copy);
}
.jp-CopyrightIcon {
  background-image: var(--jp-icon-copyright);
}
.jp-CutIcon {
  background-image: var(--jp-icon-cut);
}
.jp-DownloadIcon {
  background-image: var(--jp-icon-download);
}
.jp-EditIcon {
  background-image: var(--jp-icon-edit);
}
.jp-EllipsesIcon {
  background-image: var(--jp-icon-ellipses);
}
.jp-ExtensionIcon {
  background-image: var(--jp-icon-extension);
}
.jp-FastForwardIcon {
  background-image: var(--jp-icon-fast-forward);
}
.jp-FileIcon {
  background-image: var(--jp-icon-file);
}
.jp-FileUploadIcon {
  background-image: var(--jp-icon-file-upload);
}
.jp-FilterListIcon {
  background-image: var(--jp-icon-filter-list);
}
.jp-FolderIcon {
  background-image: var(--jp-icon-folder);
}
.jp-Html5Icon {
  background-image: var(--jp-icon-html5);
}
.jp-ImageIcon {
  background-image: var(--jp-icon-image);
}
.jp-InspectorIcon {
  background-image: var(--jp-icon-inspector);
}
.jp-JsonIcon {
  background-image: var(--jp-icon-json);
}
.jp-JuliaIcon {
  background-image: var(--jp-icon-julia);
}
.jp-JupyterFaviconIcon {
  background-image: var(--jp-icon-jupyter-favicon);
}
.jp-JupyterIcon {
  background-image: var(--jp-icon-jupyter);
}
.jp-JupyterlabWordmarkIcon {
  background-image: var(--jp-icon-jupyterlab-wordmark);
}
.jp-KernelIcon {
  background-image: var(--jp-icon-kernel);
}
.jp-KeyboardIcon {
  background-image: var(--jp-icon-keyboard);
}
.jp-LauncherIcon {
  background-image: var(--jp-icon-launcher);
}
.jp-LineFormIcon {
  background-image: var(--jp-icon-line-form);
}
.jp-LinkIcon {
  background-image: var(--jp-icon-link);
}
.jp-ListIcon {
  background-image: var(--jp-icon-list);
}
.jp-ListingsInfoIcon {
  background-image: var(--jp-icon-listings-info);
}
.jp-MarkdownIcon {
  background-image: var(--jp-icon-markdown);
}
.jp-NewFolderIcon {
  background-image: var(--jp-icon-new-folder);
}
.jp-NotTrustedIcon {
  background-image: var(--jp-icon-not-trusted);
}
.jp-NotebookIcon {
  background-image: var(--jp-icon-notebook);
}
.jp-NumberingIcon {
  background-image: var(--jp-icon-numbering);
}
.jp-OfflineBoltIcon {
  background-image: var(--jp-icon-offline-bolt);
}
.jp-PaletteIcon {
  background-image: var(--jp-icon-palette);
}
.jp-PasteIcon {
  background-image: var(--jp-icon-paste);
}
.jp-PdfIcon {
  background-image: var(--jp-icon-pdf);
}
.jp-PythonIcon {
  background-image: var(--jp-icon-python);
}
.jp-RKernelIcon {
  background-image: var(--jp-icon-r-kernel);
}
.jp-ReactIcon {
  background-image: var(--jp-icon-react);
}
.jp-RedoIcon {
  background-image: var(--jp-icon-redo);
}
.jp-RefreshIcon {
  background-image: var(--jp-icon-refresh);
}
.jp-RegexIcon {
  background-image: var(--jp-icon-regex);
}
.jp-RunIcon {
  background-image: var(--jp-icon-run);
}
.jp-RunningIcon {
  background-image: var(--jp-icon-running);
}
.jp-SaveIcon {
  background-image: var(--jp-icon-save);
}
.jp-SearchIcon {
  background-image: var(--jp-icon-search);
}
.jp-SettingsIcon {
  background-image: var(--jp-icon-settings);
}
.jp-SpreadsheetIcon {
  background-image: var(--jp-icon-spreadsheet);
}
.jp-StopIcon {
  background-image: var(--jp-icon-stop);
}
.jp-TabIcon {
  background-image: var(--jp-icon-tab);
}
.jp-TableRowsIcon {
  background-image: var(--jp-icon-table-rows);
}
.jp-TagIcon {
  background-image: var(--jp-icon-tag);
}
.jp-TerminalIcon {
  background-image: var(--jp-icon-terminal);
}
.jp-TextEditorIcon {
  background-image: var(--jp-icon-text-editor);
}
.jp-TocIcon {
  background-image: var(--jp-icon-toc);
}
.jp-TreeViewIcon {
  background-image: var(--jp-icon-tree-view);
}
.jp-TrustedIcon {
  background-image: var(--jp-icon-trusted);
}
.jp-UndoIcon {
  background-image: var(--jp-icon-undo);
}
.jp-VegaIcon {
  background-image: var(--jp-icon-vega);
}
.jp-YamlIcon {
  background-image: var(--jp-icon-yaml);
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

/**
 * (DEPRECATED) Support for consuming icons as CSS background images
 */

.jp-Icon,
.jp-MaterialIcon {
  background-position: center;
  background-repeat: no-repeat;
  background-size: 16px;
  min-width: 16px;
  min-height: 16px;
}

.jp-Icon-cover {
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
}

/**
 * (DEPRECATED) Support for specific CSS icon sizes
 */

.jp-Icon-16 {
  background-size: 16px;
  min-width: 16px;
  min-height: 16px;
}

.jp-Icon-18 {
  background-size: 18px;
  min-width: 18px;
  min-height: 18px;
}

.jp-Icon-20 {
  background-size: 20px;
  min-width: 20px;
  min-height: 20px;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

/**
 * Support for icons as inline SVG HTMLElements
 */

/* recolor the primary elements of an icon */
.jp-icon0[fill] {
  fill: var(--jp-inverse-layout-color0);
}
.jp-icon1[fill] {
  fill: var(--jp-inverse-layout-color1);
}
.jp-icon2[fill] {
  fill: var(--jp-inverse-layout-color2);
}
.jp-icon3[fill] {
  fill: var(--jp-inverse-layout-color3);
}
.jp-icon4[fill] {
  fill: var(--jp-inverse-layout-color4);
}

.jp-icon0[stroke] {
  stroke: var(--jp-inverse-layout-color0);
}
.jp-icon1[stroke] {
  stroke: var(--jp-inverse-layout-color1);
}
.jp-icon2[stroke] {
  stroke: var(--jp-inverse-layout-color2);
}
.jp-icon3[stroke] {
  stroke: var(--jp-inverse-layout-color3);
}
.jp-icon4[stroke] {
  stroke: var(--jp-inverse-layout-color4);
}
/* recolor the accent elements of an icon */
.jp-icon-accent0[fill] {
  fill: var(--jp-layout-color0);
}
.jp-icon-accent1[fill] {
  fill: var(--jp-layout-color1);
}
.jp-icon-accent2[fill] {
  fill: var(--jp-layout-color2);
}
.jp-icon-accent3[fill] {
  fill: var(--jp-layout-color3);
}
.jp-icon-accent4[fill] {
  fill: var(--jp-layout-color4);
}

.jp-icon-accent0[stroke] {
  stroke: var(--jp-layout-color0);
}
.jp-icon-accent1[stroke] {
  stroke: var(--jp-layout-color1);
}
.jp-icon-accent2[stroke] {
  stroke: var(--jp-layout-color2);
}
.jp-icon-accent3[stroke] {
  stroke: var(--jp-layout-color3);
}
.jp-icon-accent4[stroke] {
  stroke: var(--jp-layout-color4);
}
/* set the color of an icon to transparent */
.jp-icon-none[fill] {
  fill: none;
}

.jp-icon-none[stroke] {
  stroke: none;
}
/* brand icon colors. Same for light and dark */
.jp-icon-brand0[fill] {
  fill: var(--jp-brand-color0);
}
.jp-icon-brand1[fill] {
  fill: var(--jp-brand-color1);
}
.jp-icon-brand2[fill] {
  fill: var(--jp-brand-color2);
}
.jp-icon-brand3[fill] {
  fill: var(--jp-brand-color3);
}
.jp-icon-brand4[fill] {
  fill: var(--jp-brand-color4);
}

.jp-icon-brand0[stroke] {
  stroke: var(--jp-brand-color0);
}
.jp-icon-brand1[stroke] {
  stroke: var(--jp-brand-color1);
}
.jp-icon-brand2[stroke] {
  stroke: var(--jp-brand-color2);
}
.jp-icon-brand3[stroke] {
  stroke: var(--jp-brand-color3);
}
.jp-icon-brand4[stroke] {
  stroke: var(--jp-brand-color4);
}
/* warn icon colors. Same for light and dark */
.jp-icon-warn0[fill] {
  fill: var(--jp-warn-color0);
}
.jp-icon-warn1[fill] {
  fill: var(--jp-warn-color1);
}
.jp-icon-warn2[fill] {
  fill: var(--jp-warn-color2);
}
.jp-icon-warn3[fill] {
  fill: var(--jp-warn-color3);
}

.jp-icon-warn0[stroke] {
  stroke: var(--jp-warn-color0);
}
.jp-icon-warn1[stroke] {
  stroke: var(--jp-warn-color1);
}
.jp-icon-warn2[stroke] {
  stroke: var(--jp-warn-color2);
}
.jp-icon-warn3[stroke] {
  stroke: var(--jp-warn-color3);
}
/* icon colors that contrast well with each other and most backgrounds */
.jp-icon-contrast0[fill] {
  fill: var(--jp-icon-contrast-color0);
}
.jp-icon-contrast1[fill] {
  fill: var(--jp-icon-contrast-color1);
}
.jp-icon-contrast2[fill] {
  fill: var(--jp-icon-contrast-color2);
}
.jp-icon-contrast3[fill] {
  fill: var(--jp-icon-contrast-color3);
}

.jp-icon-contrast0[stroke] {
  stroke: var(--jp-icon-contrast-color0);
}
.jp-icon-contrast1[stroke] {
  stroke: var(--jp-icon-contrast-color1);
}
.jp-icon-contrast2[stroke] {
  stroke: var(--jp-icon-contrast-color2);
}
.jp-icon-contrast3[stroke] {
  stroke: var(--jp-icon-contrast-color3);
}

/* CSS for icons in selected items in the settings editor */
#setting-editor .jp-PluginList .jp-mod-selected .jp-icon-selectable[fill] {
  fill: #fff;
}
#setting-editor
  .jp-PluginList
  .jp-mod-selected
  .jp-icon-selectable-inverse[fill] {
  fill: var(--jp-brand-color1);
}

/* CSS for icons in selected filebrowser listing items */
.jp-DirListing-item.jp-mod-selected .jp-icon-selectable[fill] {
  fill: #fff;
}
.jp-DirListing-item.jp-mod-selected .jp-icon-selectable-inverse[fill] {
  fill: var(--jp-brand-color1);
}

/* CSS for icons in selected tabs in the sidebar tab manager */
#tab-manager .lm-TabBar-tab.jp-mod-active .jp-icon-selectable[fill] {
  fill: #fff;
}

#tab-manager .lm-TabBar-tab.jp-mod-active .jp-icon-selectable-inverse[fill] {
  fill: var(--jp-brand-color1);
}
#tab-manager
  .lm-TabBar-tab.jp-mod-active
  .jp-icon-hover
  :hover
  .jp-icon-selectable[fill] {
  fill: var(--jp-brand-color1);
}

#tab-manager
  .lm-TabBar-tab.jp-mod-active
  .jp-icon-hover
  :hover
  .jp-icon-selectable-inverse[fill] {
  fill: #fff;
}

/**
 * TODO: come up with non css-hack solution for showing the busy icon on top
 *  of the close icon
 * CSS for complex behavior of close icon of tabs in the sidebar tab manager
 */
#tab-manager
  .lm-TabBar-tab.jp-mod-dirty
  > .lm-TabBar-tabCloseIcon
  > :not(:hover)
  > .jp-icon3[fill] {
  fill: none;
}
#tab-manager
  .lm-TabBar-tab.jp-mod-dirty
  > .lm-TabBar-tabCloseIcon
  > :not(:hover)
  > .jp-icon-busy[fill] {
  fill: var(--jp-inverse-layout-color3);
}

#tab-manager
  .lm-TabBar-tab.jp-mod-dirty.jp-mod-active
  > .lm-TabBar-tabCloseIcon
  > :not(:hover)
  > .jp-icon-busy[fill] {
  fill: #fff;
}

/**
* TODO: come up with non css-hack solution for showing the busy icon on top
*  of the close icon
* CSS for complex behavior of close icon of tabs in the main area tabbar
*/
.lm-DockPanel-tabBar
  .lm-TabBar-tab.lm-mod-closable.jp-mod-dirty
  > .lm-TabBar-tabCloseIcon
  > :not(:hover)
  > .jp-icon3[fill] {
  fill: none;
}
.lm-DockPanel-tabBar
  .lm-TabBar-tab.lm-mod-closable.jp-mod-dirty
  > .lm-TabBar-tabCloseIcon
  > :not(:hover)
  > .jp-icon-busy[fill] {
  fill: var(--jp-inverse-layout-color3);
}

/* CSS for icons in status bar */
#jp-main-statusbar .jp-mod-selected .jp-icon-selectable[fill] {
  fill: #fff;
}

#jp-main-statusbar .jp-mod-selected .jp-icon-selectable-inverse[fill] {
  fill: var(--jp-brand-color1);
}
/* special handling for splash icon CSS. While the theme CSS reloads during
   splash, the splash icon can loose theming. To prevent that, we set a
   default for its color variable */
:root {
  --jp-warn-color0: var(--md-orange-700);
}

/* not sure what to do with this one, used in filebrowser listing */
.jp-DragIcon {
  margin-right: 4px;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

/**
 * Support for alt colors for icons as inline SVG HTMLElements
 */

/* alt recolor the primary elements of an icon */
.jp-icon-alt .jp-icon0[fill] {
  fill: var(--jp-layout-color0);
}
.jp-icon-alt .jp-icon1[fill] {
  fill: var(--jp-layout-color1);
}
.jp-icon-alt .jp-icon2[fill] {
  fill: var(--jp-layout-color2);
}
.jp-icon-alt .jp-icon3[fill] {
  fill: var(--jp-layout-color3);
}
.jp-icon-alt .jp-icon4[fill] {
  fill: var(--jp-layout-color4);
}

.jp-icon-alt .jp-icon0[stroke] {
  stroke: var(--jp-layout-color0);
}
.jp-icon-alt .jp-icon1[stroke] {
  stroke: var(--jp-layout-color1);
}
.jp-icon-alt .jp-icon2[stroke] {
  stroke: var(--jp-layout-color2);
}
.jp-icon-alt .jp-icon3[stroke] {
  stroke: var(--jp-layout-color3);
}
.jp-icon-alt .jp-icon4[stroke] {
  stroke: var(--jp-layout-color4);
}

/* alt recolor the accent elements of an icon */
.jp-icon-alt .jp-icon-accent0[fill] {
  fill: var(--jp-inverse-layout-color0);
}
.jp-icon-alt .jp-icon-accent1[fill] {
  fill: var(--jp-inverse-layout-color1);
}
.jp-icon-alt .jp-icon-accent2[fill] {
  fill: var(--jp-inverse-layout-color2);
}
.jp-icon-alt .jp-icon-accent3[fill] {
  fill: var(--jp-inverse-layout-color3);
}
.jp-icon-alt .jp-icon-accent4[fill] {
  fill: var(--jp-inverse-layout-color4);
}

.jp-icon-alt .jp-icon-accent0[stroke] {
  stroke: var(--jp-inverse-layout-color0);
}
.jp-icon-alt .jp-icon-accent1[stroke] {
  stroke: var(--jp-inverse-layout-color1);
}
.jp-icon-alt .jp-icon-accent2[stroke] {
  stroke: var(--jp-inverse-layout-color2);
}
.jp-icon-alt .jp-icon-accent3[stroke] {
  stroke: var(--jp-inverse-layout-color3);
}
.jp-icon-alt .jp-icon-accent4[stroke] {
  stroke: var(--jp-inverse-layout-color4);
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

.jp-icon-hoverShow:not(:hover) svg {
  display: none !important;
}

/**
 * Support for hover colors for icons as inline SVG HTMLElements
 */

/**
 * regular colors
 */

/* recolor the primary elements of an icon */
.jp-icon-hover :hover .jp-icon0-hover[fill] {
  fill: var(--jp-inverse-layout-color0);
}
.jp-icon-hover :hover .jp-icon1-hover[fill] {
  fill: var(--jp-inverse-layout-color1);
}
.jp-icon-hover :hover .jp-icon2-hover[fill] {
  fill: var(--jp-inverse-layout-color2);
}
.jp-icon-hover :hover .jp-icon3-hover[fill] {
  fill: var(--jp-inverse-layout-color3);
}
.jp-icon-hover :hover .jp-icon4-hover[fill] {
  fill: var(--jp-inverse-layout-color4);
}

.jp-icon-hover :hover .jp-icon0-hover[stroke] {
  stroke: var(--jp-inverse-layout-color0);
}
.jp-icon-hover :hover .jp-icon1-hover[stroke] {
  stroke: var(--jp-inverse-layout-color1);
}
.jp-icon-hover :hover .jp-icon2-hover[stroke] {
  stroke: var(--jp-inverse-layout-color2);
}
.jp-icon-hover :hover .jp-icon3-hover[stroke] {
  stroke: var(--jp-inverse-layout-color3);
}
.jp-icon-hover :hover .jp-icon4-hover[stroke] {
  stroke: var(--jp-inverse-layout-color4);
}

/* recolor the accent elements of an icon */
.jp-icon-hover :hover .jp-icon-accent0-hover[fill] {
  fill: var(--jp-layout-color0);
}
.jp-icon-hover :hover .jp-icon-accent1-hover[fill] {
  fill: var(--jp-layout-color1);
}
.jp-icon-hover :hover .jp-icon-accent2-hover[fill] {
  fill: var(--jp-layout-color2);
}
.jp-icon-hover :hover .jp-icon-accent3-hover[fill] {
  fill: var(--jp-layout-color3);
}
.jp-icon-hover :hover .jp-icon-accent4-hover[fill] {
  fill: var(--jp-layout-color4);
}

.jp-icon-hover :hover .jp-icon-accent0-hover[stroke] {
  stroke: var(--jp-layout-color0);
}
.jp-icon-hover :hover .jp-icon-accent1-hover[stroke] {
  stroke: var(--jp-layout-color1);
}
.jp-icon-hover :hover .jp-icon-accent2-hover[stroke] {
  stroke: var(--jp-layout-color2);
}
.jp-icon-hover :hover .jp-icon-accent3-hover[stroke] {
  stroke: var(--jp-layout-color3);
}
.jp-icon-hover :hover .jp-icon-accent4-hover[stroke] {
  stroke: var(--jp-layout-color4);
}

/* set the color of an icon to transparent */
.jp-icon-hover :hover .jp-icon-none-hover[fill] {
  fill: none;
}

.jp-icon-hover :hover .jp-icon-none-hover[stroke] {
  stroke: none;
}

/**
 * inverse colors
 */

/* inverse recolor the primary elements of an icon */
.jp-icon-hover.jp-icon-alt :hover .jp-icon0-hover[fill] {
  fill: var(--jp-layout-color0);
}
.jp-icon-hover.jp-icon-alt :hover .jp-icon1-hover[fill] {
  fill: var(--jp-layout-color1);
}
.jp-icon-hover.jp-icon-alt :hover .jp-icon2-hover[fill] {
  fill: var(--jp-layout-color2);
}
.jp-icon-hover.jp-icon-alt :hover .jp-icon3-hover[fill] {
  fill: var(--jp-layout-color3);
}
.jp-icon-hover.jp-icon-alt :hover .jp-icon4-hover[fill] {
  fill: var(--jp-layout-color4);
}

.jp-icon-hover.jp-icon-alt :hover .jp-icon0-hover[stroke] {
  stroke: var(--jp-layout-color0);
}
.jp-icon-hover.jp-icon-alt :hover .jp-icon1-hover[stroke] {
  stroke: var(--jp-layout-color1);
}
.jp-icon-hover.jp-icon-alt :hover .jp-icon2-hover[stroke] {
  stroke: var(--jp-layout-color2);
}
.jp-icon-hover.jp-icon-alt :hover .jp-icon3-hover[stroke] {
  stroke: var(--jp-layout-color3);
}
.jp-icon-hover.jp-icon-alt :hover .jp-icon4-hover[stroke] {
  stroke: var(--jp-layout-color4);
}

/* inverse recolor the accent elements of an icon */
.jp-icon-hover.jp-icon-alt :hover .jp-icon-accent0-hover[fill] {
  fill: var(--jp-inverse-layout-color0);
}
.jp-icon-hover.jp-icon-alt :hover .jp-icon-accent1-hover[fill] {
  fill: var(--jp-inverse-layout-color1);
}
.jp-icon-hover.jp-icon-alt :hover .jp-icon-accent2-hover[fill] {
  fill: var(--jp-inverse-layout-color2);
}
.jp-icon-hover.jp-icon-alt :hover .jp-icon-accent3-hover[fill] {
  fill: var(--jp-inverse-layout-color3);
}
.jp-icon-hover.jp-icon-alt :hover .jp-icon-accent4-hover[fill] {
  fill: var(--jp-inverse-layout-color4);
}

.jp-icon-hover.jp-icon-alt :hover .jp-icon-accent0-hover[stroke] {
  stroke: var(--jp-inverse-layout-color0);
}
.jp-icon-hover.jp-icon-alt :hover .jp-icon-accent1-hover[stroke] {
  stroke: var(--jp-inverse-layout-color1);
}
.jp-icon-hover.jp-icon-alt :hover .jp-icon-accent2-hover[stroke] {
  stroke: var(--jp-inverse-layout-color2);
}
.jp-icon-hover.jp-icon-alt :hover .jp-icon-accent3-hover[stroke] {
  stroke: var(--jp-inverse-layout-color3);
}
.jp-icon-hover.jp-icon-alt :hover .jp-icon-accent4-hover[stroke] {
  stroke: var(--jp-inverse-layout-color4);
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

.jp-switch {
  display: flex;
  align-items: center;
  padding-left: 4px;
  padding-right: 4px;
  font-size: var(--jp-ui-font-size1);
  background-color: transparent;
  color: var(--jp-ui-font-color1);
  border: none;
  height: 20px;
}

.jp-switch:hover {
  background-color: var(--jp-layout-color2);
}

.jp-switch-label {
  margin-right: 5px;
}

.jp-switch-track {
  cursor: pointer;
  background-color: var(--jp-border-color1);
  -webkit-transition: 0.4s;
  transition: 0.4s;
  border-radius: 34px;
  height: 16px;
  width: 35px;
  position: relative;
}

.jp-switch-track::before {
  content: '';
  position: absolute;
  height: 10px;
  width: 10px;
  margin: 3px;
  left: 0px;
  background-color: var(--jp-ui-inverse-font-color1);
  -webkit-transition: 0.4s;
  transition: 0.4s;
  border-radius: 50%;
}

.jp-switch[aria-checked='true'] .jp-switch-track {
  background-color: var(--jp-warn-color0);
}

.jp-switch[aria-checked='true'] .jp-switch-track::before {
  /* track width (35) - margins (3 + 3) - thumb width (10) */
  left: 19px;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

/* Sibling imports */

/* Override Blueprint's _reset.scss styles */
html {
  box-sizing: unset;
}

*,
*::before,
*::after {
  box-sizing: unset;
}

body {
  color: unset;
  font-family: var(--jp-ui-font-family);
}

p {
  margin-top: unset;
  margin-bottom: unset;
}

small {
  font-size: unset;
}

strong {
  font-weight: unset;
}

/* Override Blueprint's _typography.scss styles */
a {
  text-decoration: unset;
  color: unset;
}
a:hover {
  text-decoration: unset;
  color: unset;
}

/* Override Blueprint's _accessibility.scss styles */
:focus {
  outline: unset;
  outline-offset: unset;
  -moz-outline-radius: unset;
}

/* Styles for ui-components */
.jp-Button {
  border-radius: var(--jp-border-radius);
  padding: 0px 12px;
  font-size: var(--jp-ui-font-size1);
}

/* Use our own theme for hover styles */
button.jp-Button.bp3-button.bp3-minimal:hover {
  background-color: var(--jp-layout-color2);
}
.jp-Button.minimal {
  color: unset !important;
}

.jp-Button.jp-ToolbarButtonComponent {
  text-transform: none;
}

.jp-InputGroup input {
  box-sizing: border-box;
  border-radius: 0;
  background-color: transparent;
  color: var(--jp-ui-font-color0);
  box-shadow: inset 0 0 0 var(--jp-border-width) var(--jp-input-border-color);
}

.jp-InputGroup input:focus {
  box-shadow: inset 0 0 0 var(--jp-border-width)
      var(--jp-input-active-box-shadow-color),
    inset 0 0 0 3px var(--jp-input-active-box-shadow-color);
}

.jp-InputGroup input::placeholder,
input::placeholder {
  color: var(--jp-ui-font-color3);
}

.jp-BPIcon {
  display: inline-block;
  vertical-align: middle;
  margin: auto;
}

/* Stop blueprint futzing with our icon fills */
.bp3-icon.jp-BPIcon > svg:not([fill]) {
  fill: var(--jp-inverse-layout-color3);
}

.jp-InputGroupAction {
  padding: 6px;
}

.jp-HTMLSelect.jp-DefaultStyle select {
  background-color: initial;
  border: none;
  border-radius: 0;
  box-shadow: none;
  color: var(--jp-ui-font-color0);
  display: block;
  font-size: var(--jp-ui-font-size1);
  height: 24px;
  line-height: 14px;
  padding: 0 25px 0 10px;
  text-align: left;
  -moz-appearance: none;
  -webkit-appearance: none;
}

/* Use our own theme for hover and option styles */
.jp-HTMLSelect.jp-DefaultStyle select:hover,
.jp-HTMLSelect.jp-DefaultStyle select > option {
  background-color: var(--jp-layout-color2);
  color: var(--jp-ui-font-color0);
}
select {
  box-sizing: border-box;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

.jp-Collapse {
  display: flex;
  flex-direction: column;
  align-items: stretch;
  border-top: 1px solid var(--jp-border-color2);
  border-bottom: 1px solid var(--jp-border-color2);
}

.jp-Collapse-header {
  padding: 1px 12px;
  color: var(--jp-ui-font-color1);
  background-color: var(--jp-layout-color1);
  font-size: var(--jp-ui-font-size2);
}

.jp-Collapse-header:hover {
  background-color: var(--jp-layout-color2);
}

.jp-Collapse-contents {
  padding: 0px 12px 0px 12px;
  background-color: var(--jp-layout-color1);
  color: var(--jp-ui-font-color1);
  overflow: auto;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

/*-----------------------------------------------------------------------------
| Variables
|----------------------------------------------------------------------------*/

:root {
  --jp-private-commandpalette-search-height: 28px;
}

/*-----------------------------------------------------------------------------
| Overall styles
|----------------------------------------------------------------------------*/

.lm-CommandPalette {
  padding-bottom: 0px;
  color: var(--jp-ui-font-color1);
  background: var(--jp-layout-color1);
  /* This is needed so that all font sizing of children done in ems is
   * relative to this base size */
  font-size: var(--jp-ui-font-size1);
}

/*-----------------------------------------------------------------------------
| Modal variant
|----------------------------------------------------------------------------*/

.jp-ModalCommandPalette {
  position: absolute;
  z-index: 10000;
  top: 38px;
  left: 30%;
  margin: 0;
  padding: 4px;
  width: 40%;
  box-shadow: var(--jp-elevation-z4);
  border-radius: 4px;
  background: var(--jp-layout-color0);
}

.jp-ModalCommandPalette .lm-CommandPalette {
  max-height: 40vh;
}

.jp-ModalCommandPalette .lm-CommandPalette .lm-close-icon::after {
  display: none;
}

.jp-ModalCommandPalette .lm-CommandPalette .lm-CommandPalette-header {
  display: none;
}

.jp-ModalCommandPalette .lm-CommandPalette .lm-CommandPalette-item {
  margin-left: 4px;
  margin-right: 4px;
}

.jp-ModalCommandPalette
  .lm-CommandPalette
  .lm-CommandPalette-item.lm-mod-disabled {
  display: none;
}

/*-----------------------------------------------------------------------------
| Search
|----------------------------------------------------------------------------*/

.lm-CommandPalette-search {
  padding: 4px;
  background-color: var(--jp-layout-color1);
  z-index: 2;
}

.lm-CommandPalette-wrapper {
  overflow: overlay;
  padding: 0px 9px;
  background-color: var(--jp-input-active-background);
  height: 30px;
  box-shadow: inset 0 0 0 var(--jp-border-width) var(--jp-input-border-color);
}

.lm-CommandPalette.lm-mod-focused .lm-CommandPalette-wrapper {
  box-shadow: inset 0 0 0 1px var(--jp-input-active-box-shadow-color),
    inset 0 0 0 3px var(--jp-input-active-box-shadow-color);
}

.jp-SearchIconGroup {
  color: white;
  background-color: var(--jp-brand-color1);
  position: absolute;
  top: 4px;
  right: 4px;
  padding: 5px 5px 1px 5px;
}

.jp-SearchIconGroup svg {
  height: 20px;
  width: 20px;
}

.jp-SearchIconGroup .jp-icon3[fill] {
  fill: var(--jp-layout-color0);
}

.lm-CommandPalette-input {
  background: transparent;
  width: calc(100% - 18px);
  float: left;
  border: none;
  outline: none;
  font-size: var(--jp-ui-font-size1);
  color: var(--jp-ui-font-color0);
  line-height: var(--jp-private-commandpalette-search-height);
}

.lm-CommandPalette-input::-webkit-input-placeholder,
.lm-CommandPalette-input::-moz-placeholder,
.lm-CommandPalette-input:-ms-input-placeholder {
  color: var(--jp-ui-font-color2);
  font-size: var(--jp-ui-font-size1);
}

/*-----------------------------------------------------------------------------
| Results
|----------------------------------------------------------------------------*/

.lm-CommandPalette-header:first-child {
  margin-top: 0px;
}

.lm-CommandPalette-header {
  border-bottom: solid var(--jp-border-width) var(--jp-border-color2);
  color: var(--jp-ui-font-color1);
  cursor: pointer;
  display: flex;
  font-size: var(--jp-ui-font-size0);
  font-weight: 600;
  letter-spacing: 1px;
  margin-top: 8px;
  padding: 8px 0 8px 12px;
  text-transform: uppercase;
}

.lm-CommandPalette-header.lm-mod-active {
  background: var(--jp-layout-color2);
}

.lm-CommandPalette-header > mark {
  background-color: transparent;
  font-weight: bold;
  color: var(--jp-ui-font-color1);
}

.lm-CommandPalette-item {
  padding: 4px 12px 4px 4px;
  color: var(--jp-ui-font-color1);
  font-size: var(--jp-ui-font-size1);
  font-weight: 400;
  display: flex;
}

.lm-CommandPalette-item.lm-mod-disabled {
  color: var(--jp-ui-font-color2);
}

.lm-CommandPalette-item.lm-mod-active {
  color: var(--jp-ui-inverse-font-color1);
  background: var(--jp-brand-color1);
}

.lm-CommandPalette-item.lm-mod-active .lm-CommandPalette-itemLabel > mark {
  color: var(--jp-ui-inverse-font-color0);
}

.lm-CommandPalette-item.lm-mod-active .jp-icon-selectable[fill] {
  fill: var(--jp-layout-color0);
}

.lm-CommandPalette-item.lm-mod-active .lm-CommandPalette-itemLabel > mark {
  color: var(--jp-ui-inverse-font-color0);
}

.lm-CommandPalette-item.lm-mod-active:hover:not(.lm-mod-disabled) {
  color: var(--jp-ui-inverse-font-color1);
  background: var(--jp-brand-color1);
}

.lm-CommandPalette-item:hover:not(.lm-mod-active):not(.lm-mod-disabled) {
  background: var(--jp-layout-color2);
}

.lm-CommandPalette-itemContent {
  overflow: hidden;
}

.lm-CommandPalette-itemLabel > mark {
  color: var(--jp-ui-font-color0);
  background-color: transparent;
  font-weight: bold;
}

.lm-CommandPalette-item.lm-mod-disabled mark {
  color: var(--jp-ui-font-color2);
}

.lm-CommandPalette-item .lm-CommandPalette-itemIcon {
  margin: 0 4px 0 0;
  position: relative;
  width: 16px;
  top: 2px;
  flex: 0 0 auto;
}

.lm-CommandPalette-item.lm-mod-disabled .lm-CommandPalette-itemIcon {
  opacity: 0.6;
}

.lm-CommandPalette-item .lm-CommandPalette-itemShortcut {
  flex: 0 0 auto;
}

.lm-CommandPalette-itemCaption {
  display: none;
}

.lm-CommandPalette-content {
  background-color: var(--jp-layout-color1);
}

.lm-CommandPalette-content:empty:after {
  content: 'No results';
  margin: auto;
  margin-top: 20px;
  width: 100px;
  display: block;
  font-size: var(--jp-ui-font-size2);
  font-family: var(--jp-ui-font-family);
  font-weight: lighter;
}

.lm-CommandPalette-emptyMessage {
  text-align: center;
  margin-top: 24px;
  line-height: 1.32;
  padding: 0px 8px;
  color: var(--jp-content-font-color3);
}

/*-----------------------------------------------------------------------------
| Copyright (c) 2014-2017, Jupyter Development Team.
|
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

.jp-Dialog {
  position: absolute;
  z-index: 10000;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  top: 0px;
  left: 0px;
  margin: 0;
  padding: 0;
  width: 100%;
  height: 100%;
  background: var(--jp-dialog-background);
}

.jp-Dialog-content {
  display: flex;
  flex-direction: column;
  margin-left: auto;
  margin-right: auto;
  background: var(--jp-layout-color1);
  padding: 24px;
  padding-bottom: 12px;
  min-width: 300px;
  min-height: 150px;
  max-width: 1000px;
  max-height: 500px;
  box-sizing: border-box;
  box-shadow: var(--jp-elevation-z20);
  word-wrap: break-word;
  border-radius: var(--jp-border-radius);
  /* This is needed so that all font sizing of children done in ems is
   * relative to this base size */
  font-size: var(--jp-ui-font-size1);
  color: var(--jp-ui-font-color1);
  resize: both;
}

.jp-Dialog-button {
  overflow: visible;
}

button.jp-Dialog-button:focus {
  outline: 1px solid var(--jp-brand-color1);
  outline-offset: 4px;
  -moz-outline-radius: 0px;
}

button.jp-Dialog-button:focus::-moz-focus-inner {
  border: 0;
}

button.jp-Dialog-close-button {
  padding: 0;
  height: 100%;
  min-width: unset;
  min-height: unset;
}

.jp-Dialog-header {
  display: flex;
  justify-content: space-between;
  flex: 0 0 auto;
  padding-bottom: 12px;
  font-size: var(--jp-ui-font-size3);
  font-weight: 400;
  color: var(--jp-ui-font-color0);
}

.jp-Dialog-body {
  display: flex;
  flex-direction: column;
  flex: 1 1 auto;
  font-size: var(--jp-ui-font-size1);
  background: var(--jp-layout-color1);
  overflow: auto;
}

.jp-Dialog-footer {
  display: flex;
  flex-direction: row;
  justify-content: flex-end;
  flex: 0 0 auto;
  margin-left: -12px;
  margin-right: -12px;
  padding: 12px;
}

.jp-Dialog-title {
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}

.jp-Dialog-body > .jp-select-wrapper {
  width: 100%;
}

.jp-Dialog-body > button {
  padding: 0px 16px;
}

.jp-Dialog-body > label {
  line-height: 1.4;
  color: var(--jp-ui-font-color0);
}

.jp-Dialog-button.jp-mod-styled:not(:last-child) {
  margin-right: 12px;
}

/*-----------------------------------------------------------------------------
| Copyright (c) 2014-2016, Jupyter Development Team.
|
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

.jp-HoverBox {
  position: fixed;
}

.jp-HoverBox.jp-mod-outofview {
  display: none;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

.jp-IFrame {
  width: 100%;
  height: 100%;
}

.jp-IFrame > iframe {
  border: none;
}

/*
When drag events occur, `p-mod-override-cursor` is added to the body.
Because iframes steal all cursor events, the following two rules are necessary
to suppress pointer events while resize drags are occurring. There may be a
better solution to this problem.
*/
body.lm-mod-override-cursor .jp-IFrame {
  position: relative;
}

body.lm-mod-override-cursor .jp-IFrame:before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: transparent;
}

.jp-Input-Boolean-Dialog {
  flex-direction: row-reverse;
  align-items: end;
  width: 100%;
}

.jp-Input-Boolean-Dialog > label {
  flex: 1 1 auto;
}

/*-----------------------------------------------------------------------------
| Copyright (c) 2014-2016, Jupyter Development Team.
|
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

.jp-MainAreaWidget > :focus {
  outline: none;
}

/**
 * google-material-color v1.2.6
 * https://github.com/danlevan/google-material-color
 */
:root {
  --md-red-50: #ffebee;
  --md-red-100: #ffcdd2;
  --md-red-200: #ef9a9a;
  --md-red-300: #e57373;
  --md-red-400: #ef5350;
  --md-red-500: #f44336;
  --md-red-600: #e53935;
  --md-red-700: #d32f2f;
  --md-red-800: #c62828;
  --md-red-900: #b71c1c;
  --md-red-A100: #ff8a80;
  --md-red-A200: #ff5252;
  --md-red-A400: #ff1744;
  --md-red-A700: #d50000;

  --md-pink-50: #fce4ec;
  --md-pink-100: #f8bbd0;
  --md-pink-200: #f48fb1;
  --md-pink-300: #f06292;
  --md-pink-400: #ec407a;
  --md-pink-500: #e91e63;
  --md-pink-600: #d81b60;
  --md-pink-700: #c2185b;
  --md-pink-800: #ad1457;
  --md-pink-900: #880e4f;
  --md-pink-A100: #ff80ab;
  --md-pink-A200: #ff4081;
  --md-pink-A400: #f50057;
  --md-pink-A700: #c51162;

  --md-purple-50: #f3e5f5;
  --md-purple-100: #e1bee7;
  --md-purple-200: #ce93d8;
  --md-purple-300: #ba68c8;
  --md-purple-400: #ab47bc;
  --md-purple-500: #9c27b0;
  --md-purple-600: #8e24aa;
  --md-purple-700: #7b1fa2;
  --md-purple-800: #6a1b9a;
  --md-purple-900: #4a148c;
  --md-purple-A100: #ea80fc;
  --md-purple-A200: #e040fb;
  --md-purple-A400: #d500f9;
  --md-purple-A700: #aa00ff;

  --md-deep-purple-50: #ede7f6;
  --md-deep-purple-100: #d1c4e9;
  --md-deep-purple-200: #b39ddb;
  --md-deep-purple-300: #9575cd;
  --md-deep-purple-400: #7e57c2;
  --md-deep-purple-500: #673ab7;
  --md-deep-purple-600: #5e35b1;
  --md-deep-purple-700: #512da8;
  --md-deep-purple-800: #4527a0;
  --md-deep-purple-900: #311b92;
  --md-deep-purple-A100: #b388ff;
  --md-deep-purple-A200: #7c4dff;
  --md-deep-purple-A400: #651fff;
  --md-deep-purple-A700: #6200ea;

  --md-indigo-50: #e8eaf6;
  --md-indigo-100: #c5cae9;
  --md-indigo-200: #9fa8da;
  --md-indigo-300: #7986cb;
  --md-indigo-400: #5c6bc0;
  --md-indigo-500: #3f51b5;
  --md-indigo-600: #3949ab;
  --md-indigo-700: #303f9f;
  --md-indigo-800: #283593;
  --md-indigo-900: #1a237e;
  --md-indigo-A100: #8c9eff;
  --md-indigo-A200: #536dfe;
  --md-indigo-A400: #3d5afe;
  --md-indigo-A700: #304ffe;

  --md-blue-50: #e3f2fd;
  --md-blue-100: #bbdefb;
  --md-blue-200: #90caf9;
  --md-blue-300: #64b5f6;
  --md-blue-400: #42a5f5;
  --md-blue-500: #2196f3;
  --md-blue-600: #1e88e5;
  --md-blue-700: #1976d2;
  --md-blue-800: #1565c0;
  --md-blue-900: #0d47a1;
  --md-blue-A100: #82b1ff;
  --md-blue-A200: #448aff;
  --md-blue-A400: #2979ff;
  --md-blue-A700: #2962ff;

  --md-light-blue-50: #e1f5fe;
  --md-light-blue-100: #b3e5fc;
  --md-light-blue-200: #81d4fa;
  --md-light-blue-300: #4fc3f7;
  --md-light-blue-400: #29b6f6;
  --md-light-blue-500: #03a9f4;
  --md-light-blue-600: #039be5;
  --md-light-blue-700: #0288d1;
  --md-light-blue-800: #0277bd;
  --md-light-blue-900: #01579b;
  --md-light-blue-A100: #80d8ff;
  --md-light-blue-A200: #40c4ff;
  --md-light-blue-A400: #00b0ff;
  --md-light-blue-A700: #0091ea;

  --md-cyan-50: #e0f7fa;
  --md-cyan-100: #b2ebf2;
  --md-cyan-200: #80deea;
  --md-cyan-300: #4dd0e1;
  --md-cyan-400: #26c6da;
  --md-cyan-500: #00bcd4;
  --md-cyan-600: #00acc1;
  --md-cyan-700: #0097a7;
  --md-cyan-800: #00838f;
  --md-cyan-900: #006064;
  --md-cyan-A100: #84ffff;
  --md-cyan-A200: #18ffff;
  --md-cyan-A400: #00e5ff;
  --md-cyan-A700: #00b8d4;

  --md-teal-50: #e0f2f1;
  --md-teal-100: #b2dfdb;
  --md-teal-200: #80cbc4;
  --md-teal-300: #4db6ac;
  --md-teal-400: #26a69a;
  --md-teal-500: #009688;
  --md-teal-600: #00897b;
  --md-teal-700: #00796b;
  --md-teal-800: #00695c;
  --md-teal-900: #004d40;
  --md-teal-A100: #a7ffeb;
  --md-teal-A200: #64ffda;
  --md-teal-A400: #1de9b6;
  --md-teal-A700: #00bfa5;

  --md-green-50: #e8f5e9;
  --md-green-100: #c8e6c9;
  --md-green-200: #a5d6a7;
  --md-green-300: #81c784;
  --md-green-400: #66bb6a;
  --md-green-500: #4caf50;
  --md-green-600: #43a047;
  --md-green-700: #388e3c;
  --md-green-800: #2e7d32;
  --md-green-900: #1b5e20;
  --md-green-A100: #b9f6ca;
  --md-green-A200: #69f0ae;
  --md-green-A400: #00e676;
  --md-green-A700: #00c853;

  --md-light-green-50: #f1f8e9;
  --md-light-green-100: #dcedc8;
  --md-light-green-200: #c5e1a5;
  --md-light-green-300: #aed581;
  --md-light-green-400: #9ccc65;
  --md-light-green-500: #8bc34a;
  --md-light-green-600: #7cb342;
  --md-light-green-700: #689f38;
  --md-light-green-800: #558b2f;
  --md-light-green-900: #33691e;
  --md-light-green-A100: #ccff90;
  --md-light-green-A200: #b2ff59;
  --md-light-green-A400: #76ff03;
  --md-light-green-A700: #64dd17;

  --md-lime-50: #f9fbe7;
  --md-lime-100: #f0f4c3;
  --md-lime-200: #e6ee9c;
  --md-lime-300: #dce775;
  --md-lime-400: #d4e157;
  --md-lime-500: #cddc39;
  --md-lime-600: #c0ca33;
  --md-lime-700: #afb42b;
  --md-lime-800: #9e9d24;
  --md-lime-900: #827717;
  --md-lime-A100: #f4ff81;
  --md-lime-A200: #eeff41;
  --md-lime-A400: #c6ff00;
  --md-lime-A700: #aeea00;

  --md-yellow-50: #fffde7;
  --md-yellow-100: #fff9c4;
  --md-yellow-200: #fff59d;
  --md-yellow-300: #fff176;
  --md-yellow-400: #ffee58;
  --md-yellow-500: #ffeb3b;
  --md-yellow-600: #fdd835;
  --md-yellow-700: #fbc02d;
  --md-yellow-800: #f9a825;
  --md-yellow-900: #f57f17;
  --md-yellow-A100: #ffff8d;
  --md-yellow-A200: #ffff00;
  --md-yellow-A400: #ffea00;
  --md-yellow-A700: #ffd600;

  --md-amber-50: #fff8e1;
  --md-amber-100: #ffecb3;
  --md-amber-200: #ffe082;
  --md-amber-300: #ffd54f;
  --md-amber-400: #ffca28;
  --md-amber-500: #ffc107;
  --md-amber-600: #ffb300;
  --md-amber-700: #ffa000;
  --md-amber-800: #ff8f00;
  --md-amber-900: #ff6f00;
  --md-amber-A100: #ffe57f;
  --md-amber-A200: #ffd740;
  --md-amber-A400: #ffc400;
  --md-amber-A700: #ffab00;

  --md-orange-50: #fff3e0;
  --md-orange-100: #ffe0b2;
  --md-orange-200: #ffcc80;
  --md-orange-300: #ffb74d;
  --md-orange-400: #ffa726;
  --md-orange-500: #ff9800;
  --md-orange-600: #fb8c00;
  --md-orange-700: #f57c00;
  --md-orange-800: #ef6c00;
  --md-orange-900: #e65100;
  --md-orange-A100: #ffd180;
  --md-orange-A200: #ffab40;
  --md-orange-A400: #ff9100;
  --md-orange-A700: #ff6d00;

  --md-deep-orange-50: #fbe9e7;
  --md-deep-orange-100: #ffccbc;
  --md-deep-orange-200: #ffab91;
  --md-deep-orange-300: #ff8a65;
  --md-deep-orange-400: #ff7043;
  --md-deep-orange-500: #ff5722;
  --md-deep-orange-600: #f4511e;
  --md-deep-orange-700: #e64a19;
  --md-deep-orange-800: #d84315;
  --md-deep-orange-900: #bf360c;
  --md-deep-orange-A100: #ff9e80;
  --md-deep-orange-A200: #ff6e40;
  --md-deep-orange-A400: #ff3d00;
  --md-deep-orange-A700: #dd2c00;

  --md-brown-50: #efebe9;
  --md-brown-100: #d7ccc8;
  --md-brown-200: #bcaaa4;
  --md-brown-300: #a1887f;
  --md-brown-400: #8d6e63;
  --md-brown-500: #795548;
  --md-brown-600: #6d4c41;
  --md-brown-700: #5d4037;
  --md-brown-800: #4e342e;
  --md-brown-900: #3e2723;

  --md-grey-50: #fafafa;
  --md-grey-100: #f5f5f5;
  --md-grey-200: #eeeeee;
  --md-grey-300: #e0e0e0;
  --md-grey-400: #bdbdbd;
  --md-grey-500: #9e9e9e;
  --md-grey-600: #757575;
  --md-grey-700: #616161;
  --md-grey-800: #424242;
  --md-grey-900: #212121;

  --md-blue-grey-50: #eceff1;
  --md-blue-grey-100: #cfd8dc;
  --md-blue-grey-200: #b0bec5;
  --md-blue-grey-300: #90a4ae;
  --md-blue-grey-400: #78909c;
  --md-blue-grey-500: #607d8b;
  --md-blue-grey-600: #546e7a;
  --md-blue-grey-700: #455a64;
  --md-blue-grey-800: #37474f;
  --md-blue-grey-900: #263238;
}

/*-----------------------------------------------------------------------------
| Copyright (c) 2017, Jupyter Development Team.
|
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

.jp-Spinner {
  position: absolute;
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 10;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  background: var(--jp-layout-color0);
  outline: none;
}

.jp-SpinnerContent {
  font-size: 10px;
  margin: 50px auto;
  text-indent: -9999em;
  width: 3em;
  height: 3em;
  border-radius: 50%;
  background: var(--jp-brand-color3);
  background: linear-gradient(
    to right,
    #f37626 10%,
    rgba(255, 255, 255, 0) 42%
  );
  position: relative;
  animation: load3 1s infinite linear, fadeIn 1s;
}

.jp-SpinnerContent:before {
  width: 50%;
  height: 50%;
  background: #f37626;
  border-radius: 100% 0 0 0;
  position: absolute;
  top: 0;
  left: 0;
  content: '';
}

.jp-SpinnerContent:after {
  background: var(--jp-layout-color0);
  width: 75%;
  height: 75%;
  border-radius: 50%;
  content: '';
  margin: auto;
  position: absolute;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
}

@keyframes fadeIn {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}

@keyframes load3 {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

/*-----------------------------------------------------------------------------
| Copyright (c) 2014-2017, Jupyter Development Team.
|
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

button.jp-mod-styled {
  font-size: var(--jp-ui-font-size1);
  color: var(--jp-ui-font-color0);
  border: none;
  box-sizing: border-box;
  text-align: center;
  line-height: 32px;
  height: 32px;
  padding: 0px 12px;
  letter-spacing: 0.8px;
  outline: none;
  appearance: none;
  -webkit-appearance: none;
  -moz-appearance: none;
}

input.jp-mod-styled {
  background: var(--jp-input-background);
  height: 28px;
  box-sizing: border-box;
  border: var(--jp-border-width) solid var(--jp-border-color1);
  padding-left: 7px;
  padding-right: 7px;
  font-size: var(--jp-ui-font-size2);
  color: var(--jp-ui-font-color0);
  outline: none;
  appearance: none;
  -webkit-appearance: none;
  -moz-appearance: none;
}

input[type='checkbox'].jp-mod-styled {
  appearance: checkbox;
  -webkit-appearance: checkbox;
  -moz-appearance: checkbox;
  height: auto;
}

input.jp-mod-styled:focus {
  border: var(--jp-border-width) solid var(--md-blue-500);
  box-shadow: inset 0 0 4px var(--md-blue-300);
}

.jp-FileDialog-Checkbox {
  margin-top: 35px;
  display: flex;
  flex-direction: row;
  align-items: end;
  width: 100%;
}

.jp-FileDialog-Checkbox > label {
  flex: 1 1 auto;
}

.jp-select-wrapper {
  display: flex;
  position: relative;
  flex-direction: column;
  padding: 1px;
  background-color: var(--jp-layout-color1);
  height: 28px;
  box-sizing: border-box;
  margin-bottom: 12px;
}

.jp-select-wrapper.jp-mod-focused select.jp-mod-styled {
  border: var(--jp-border-width) solid var(--jp-input-active-border-color);
  box-shadow: var(--jp-input-box-shadow);
  background-color: var(--jp-input-active-background);
}

select.jp-mod-styled:hover {
  background-color: var(--jp-layout-color1);
  cursor: pointer;
  color: var(--jp-ui-font-color0);
  background-color: var(--jp-input-hover-background);
  box-shadow: inset 0 0px 1px rgba(0, 0, 0, 0.5);
}

select.jp-mod-styled {
  flex: 1 1 auto;
  height: 32px;
  width: 100%;
  font-size: var(--jp-ui-font-size2);
  background: var(--jp-input-background);
  color: var(--jp-ui-font-color0);
  padding: 0 25px 0 8px;
  border: var(--jp-border-width) solid var(--jp-input-border-color);
  border-radius: 0px;
  outline: none;
  appearance: none;
  -webkit-appearance: none;
  -moz-appearance: none;
}

/*-----------------------------------------------------------------------------
| Copyright (c) 2014-2016, Jupyter Development Team.
|
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

:root {
  --jp-private-toolbar-height: calc(
    28px + var(--jp-border-width)
  ); /* leave 28px for content */
}

.jp-Toolbar {
  color: var(--jp-ui-font-color1);
  flex: 0 0 auto;
  display: flex;
  flex-direction: row;
  border-bottom: var(--jp-border-width) solid var(--jp-toolbar-border-color);
  box-shadow: var(--jp-toolbar-box-shadow);
  background: var(--jp-toolbar-background);
  min-height: var(--jp-toolbar-micro-height);
  padding: 2px;
  z-index: 1;
  overflow-x: auto;
}

/* Toolbar items */

.jp-Toolbar > .jp-Toolbar-item.jp-Toolbar-spacer {
  flex-grow: 1;
  flex-shrink: 1;
}

.jp-Toolbar-item.jp-Toolbar-kernelStatus {
  display: inline-block;
  width: 32px;
  background-repeat: no-repeat;
  background-position: center;
  background-size: 16px;
}

.jp-Toolbar > .jp-Toolbar-item {
  flex: 0 0 auto;
  display: flex;
  padding-left: 1px;
  padding-right: 1px;
  font-size: var(--jp-ui-font-size1);
  line-height: var(--jp-private-toolbar-height);
  height: 100%;
}

/* Toolbar buttons */

/* This is the div we use to wrap the react component into a Widget */
div.jp-ToolbarButton {
  color: transparent;
  border: none;
  box-sizing: border-box;
  outline: none;
  appearance: none;
  -webkit-appearance: none;
  -moz-appearance: none;
  padding: 0px;
  margin: 0px;
}

button.jp-ToolbarButtonComponent {
  background: var(--jp-layout-color1);
  border: none;
  box-sizing: border-box;
  outline: none;
  appearance: none;
  -webkit-appearance: none;
  -moz-appearance: none;
  padding: 0px 6px;
  margin: 0px;
  height: 24px;
  border-radius: var(--jp-border-radius);
  display: flex;
  align-items: center;
  text-align: center;
  font-size: 14px;
  min-width: unset;
  min-height: unset;
}

button.jp-ToolbarButtonComponent:disabled {
  opacity: 0.4;
}

button.jp-ToolbarButtonComponent span {
  padding: 0px;
  flex: 0 0 auto;
}

button.jp-ToolbarButtonComponent .jp-ToolbarButtonComponent-label {
  font-size: var(--jp-ui-font-size1);
  line-height: 100%;
  padding-left: 2px;
  color: var(--jp-ui-font-color1);
}

#jp-main-dock-panel[data-mode='single-document']
  .jp-MainAreaWidget
  > .jp-Toolbar.jp-Toolbar-micro {
  padding: 0;
  min-height: 0;
}

#jp-main-dock-panel[data-mode='single-document']
  .jp-MainAreaWidget
  > .jp-Toolbar {
  border: none;
  box-shadow: none;
}

/*-----------------------------------------------------------------------------
| Copyright (c) 2014-2017, Jupyter Development Team.
|
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Copyright (c) 2014-2017, PhosphorJS Contributors
|
| Distributed under the terms of the BSD 3-Clause License.
|
| The full license is in the file LICENSE, distributed with this software.
|----------------------------------------------------------------------------*/


/* <DEPRECATED> */ body.p-mod-override-cursor *, /* </DEPRECATED> */
body.lm-mod-override-cursor * {
  cursor: inherit !important;
}

/*-----------------------------------------------------------------------------
| Copyright (c) 2014-2016, Jupyter Development Team.
|
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

.jp-JSONEditor {
  display: flex;
  flex-direction: column;
  width: 100%;
}

.jp-JSONEditor-host {
  flex: 1 1 auto;
  border: var(--jp-border-width) solid var(--jp-input-border-color);
  border-radius: 0px;
  background: var(--jp-layout-color0);
  min-height: 50px;
  padding: 1px;
}

.jp-JSONEditor.jp-mod-error .jp-JSONEditor-host {
  border-color: red;
  outline-color: red;
}

.jp-JSONEditor-header {
  display: flex;
  flex: 1 0 auto;
  padding: 0 0 0 12px;
}

.jp-JSONEditor-header label {
  flex: 0 0 auto;
}

.jp-JSONEditor-commitButton {
  height: 16px;
  width: 16px;
  background-size: 18px;
  background-repeat: no-repeat;
  background-position: center;
}

.jp-JSONEditor-host.jp-mod-focused {
  background-color: var(--jp-input-active-background);
  border: 1px solid var(--jp-input-active-border-color);
  box-shadow: var(--jp-input-box-shadow);
}

.jp-Editor.jp-mod-dropTarget {
  border: var(--jp-border-width) solid var(--jp-input-active-border-color);
  box-shadow: var(--jp-input-box-shadow);
}

/* BASICS */

.CodeMirror {
  /* Set height, width, borders, and global font properties here */
  font-family: monospace;
  height: 300px;
  color: black;
  direction: ltr;
}

/* PADDING */

.CodeMirror-lines {
  padding: 4px 0; /* Vertical padding around content */
}
.CodeMirror pre.CodeMirror-line,
.CodeMirror pre.CodeMirror-line-like {
  padding: 0 4px; /* Horizontal padding of content */
}

.CodeMirror-scrollbar-filler, .CodeMirror-gutter-filler {
  background-color: white; /* The little square between H and V scrollbars */
}

/* GUTTER */

.CodeMirror-gutters {
  border-right: 1px solid #ddd;
  background-color: #f7f7f7;
  white-space: nowrap;
}
.CodeMirror-linenumbers {}
.CodeMirror-linenumber {
  padding: 0 3px 0 5px;
  min-width: 20px;
  text-align: right;
  color: #999;
  white-space: nowrap;
}

.CodeMirror-guttermarker { color: black; }
.CodeMirror-guttermarker-subtle { color: #999; }

/* CURSOR */

.CodeMirror-cursor {
  border-left: 1px solid black;
  border-right: none;
  width: 0;
}
/* Shown when moving in bi-directional text */
.CodeMirror div.CodeMirror-secondarycursor {
  border-left: 1px solid silver;
}
.cm-fat-cursor .CodeMirror-cursor {
  width: auto;
  border: 0 !important;
  background: #7e7;
}
.cm-fat-cursor div.CodeMirror-cursors {
  z-index: 1;
}
.cm-fat-cursor-mark {
  background-color: rgba(20, 255, 20, 0.5);
  -webkit-animation: blink 1.06s steps(1) infinite;
  -moz-animation: blink 1.06s steps(1) infinite;
  animation: blink 1.06s steps(1) infinite;
}
.cm-animate-fat-cursor {
  width: auto;
  border: 0;
  -webkit-animation: blink 1.06s steps(1) infinite;
  -moz-animation: blink 1.06s steps(1) infinite;
  animation: blink 1.06s steps(1) infinite;
  background-color: #7e7;
}
@-moz-keyframes blink {
  0% {}
  50% { background-color: transparent; }
  100% {}
}
@-webkit-keyframes blink {
  0% {}
  50% { background-color: transparent; }
  100% {}
}
@keyframes blink {
  0% {}
  50% { background-color: transparent; }
  100% {}
}

/* Can style cursor different in overwrite (non-insert) mode */
.CodeMirror-overwrite .CodeMirror-cursor {}

.cm-tab { display: inline-block; text-decoration: inherit; }

.CodeMirror-rulers {
  position: absolute;
  left: 0; right: 0; top: -50px; bottom: 0;
  overflow: hidden;
}
.CodeMirror-ruler {
  border-left: 1px solid #ccc;
  top: 0; bottom: 0;
  position: absolute;
}

/* DEFAULT THEME */

.cm-s-default .cm-header {color: blue;}
.cm-s-default .cm-quote {color: #090;}
.cm-negative {color: #d44;}
.cm-positive {color: #292;}
.cm-header, .cm-strong {font-weight: bold;}
.cm-em {font-style: italic;}
.cm-link {text-decoration: underline;}
.cm-strikethrough {text-decoration: line-through;}

.cm-s-default .cm-keyword {color: #708;}
.cm-s-default .cm-atom {color: #219;}
.cm-s-default .cm-number {color: #164;}
.cm-s-default .cm-def {color: #00f;}
.cm-s-default .cm-variable,
.cm-s-default .cm-punctuation,
.cm-s-default .cm-property,
.cm-s-default .cm-operator {}
.cm-s-default .cm-variable-2 {color: #05a;}
.cm-s-default .cm-variable-3, .cm-s-default .cm-type {color: #085;}
.cm-s-default .cm-comment {color: #a50;}
.cm-s-default .cm-string {color: #a11;}
.cm-s-default .cm-string-2 {color: #f50;}
.cm-s-default .cm-meta {color: #555;}
.cm-s-default .cm-qualifier {color: #555;}
.cm-s-default .cm-builtin {color: #30a;}
.cm-s-default .cm-bracket {color: #997;}
.cm-s-default .cm-tag {color: #170;}
.cm-s-default .cm-attribute {color: #00c;}
.cm-s-default .cm-hr {color: #999;}
.cm-s-default .cm-link {color: #00c;}

.cm-s-default .cm-error {color: #f00;}
.cm-invalidchar {color: #f00;}

.CodeMirror-composing { border-bottom: 2px solid; }

/* Default styles for common addons */

div.CodeMirror span.CodeMirror-matchingbracket {color: #0b0;}
div.CodeMirror span.CodeMirror-nonmatchingbracket {color: #a22;}
.CodeMirror-matchingtag { background: rgba(255, 150, 0, .3); }
.CodeMirror-activeline-background {background: #e8f2ff;}

/* STOP */

/* The rest of this file contains styles related to the mechanics of
   the editor. You probably shouldn't touch them. */

.CodeMirror {
  position: relative;
  overflow: hidden;
  background: white;
}

.CodeMirror-scroll {
  overflow: scroll !important; /* Things will break if this is overridden */
  /* 50px is the magic margin used to hide the element's real scrollbars */
  /* See overflow: hidden in .CodeMirror */
  margin-bottom: -50px; margin-right: -50px;
  padding-bottom: 50px;
  height: 100%;
  outline: none; /* Prevent dragging from highlighting the element */
  position: relative;
}
.CodeMirror-sizer {
  position: relative;
  border-right: 50px solid transparent;
}

/* The fake, visible scrollbars. Used to force redraw during scrolling
   before actual scrolling happens, thus preventing shaking and
   flickering artifacts. */
.CodeMirror-vscrollbar, .CodeMirror-hscrollbar, .CodeMirror-scrollbar-filler, .CodeMirror-gutter-filler {
  position: absolute;
  z-index: 6;
  display: none;
  outline: none;
}
.CodeMirror-vscrollbar {
  right: 0; top: 0;
  overflow-x: hidden;
  overflow-y: scroll;
}
.CodeMirror-hscrollbar {
  bottom: 0; left: 0;
  overflow-y: hidden;
  overflow-x: scroll;
}
.CodeMirror-scrollbar-filler {
  right: 0; bottom: 0;
}
.CodeMirror-gutter-filler {
  left: 0; bottom: 0;
}

.CodeMirror-gutters {
  position: absolute; left: 0; top: 0;
  min-height: 100%;
  z-index: 3;
}
.CodeMirror-gutter {
  white-space: normal;
  height: 100%;
  display: inline-block;
  vertical-align: top;
  margin-bottom: -50px;
}
.CodeMirror-gutter-wrapper {
  position: absolute;
  z-index: 4;
  background: none !important;
  border: none !important;
}
.CodeMirror-gutter-background {
  position: absolute;
  top: 0; bottom: 0;
  z-index: 4;
}
.CodeMirror-gutter-elt {
  position: absolute;
  cursor: default;
  z-index: 4;
}
.CodeMirror-gutter-wrapper ::selection { background-color: transparent }
.CodeMirror-gutter-wrapper ::-moz-selection { background-color: transparent }

.CodeMirror-lines {
  cursor: text;
  min-height: 1px; /* prevents collapsing before first draw */
}
.CodeMirror pre.CodeMirror-line,
.CodeMirror pre.CodeMirror-line-like {
  /* Reset some styles that the rest of the page might have set */
  -moz-border-radius: 0; -webkit-border-radius: 0; border-radius: 0;
  border-width: 0;
  background: transparent;
  font-family: inherit;
  font-size: inherit;
  margin: 0;
  white-space: pre;
  word-wrap: normal;
  line-height: inherit;
  color: inherit;
  z-index: 2;
  position: relative;
  overflow: visible;
  -webkit-tap-highlight-color: transparent;
  -webkit-font-variant-ligatures: contextual;
  font-variant-ligatures: contextual;
}
.CodeMirror-wrap pre.CodeMirror-line,
.CodeMirror-wrap pre.CodeMirror-line-like {
  word-wrap: break-word;
  white-space: pre-wrap;
  word-break: normal;
}

.CodeMirror-linebackground {
  position: absolute;
  left: 0; right: 0; top: 0; bottom: 0;
  z-index: 0;
}

.CodeMirror-linewidget {
  position: relative;
  z-index: 2;
  padding: 0.1px; /* Force widget margins to stay inside of the container */
}

.CodeMirror-widget {}

.CodeMirror-rtl pre { direction: rtl; }

.CodeMirror-code {
  outline: none;
}

/* Force content-box sizing for the elements where we expect it */
.CodeMirror-scroll,
.CodeMirror-sizer,
.CodeMirror-gutter,
.CodeMirror-gutters,
.CodeMirror-linenumber {
  -moz-box-sizing: content-box;
  box-sizing: content-box;
}

.CodeMirror-measure {
  position: absolute;
  width: 100%;
  height: 0;
  overflow: hidden;
  visibility: hidden;
}

.CodeMirror-cursor {
  position: absolute;
  pointer-events: none;
}
.CodeMirror-measure pre { position: static; }

div.CodeMirror-cursors {
  visibility: hidden;
  position: relative;
  z-index: 3;
}
div.CodeMirror-dragcursors {
  visibility: visible;
}

.CodeMirror-focused div.CodeMirror-cursors {
  visibility: visible;
}

.CodeMirror-selected { background: #d9d9d9; }
.CodeMirror-focused .CodeMirror-selected { background: #d7d4f0; }
.CodeMirror-crosshair { cursor: crosshair; }
.CodeMirror-line::selection, .CodeMirror-line > span::selection, .CodeMirror-line > span > span::selection { background: #d7d4f0; }
.CodeMirror-line::-moz-selection, .CodeMirror-line > span::-moz-selection, .CodeMirror-line > span > span::-moz-selection { background: #d7d4f0; }

.cm-searching {
  background-color: #ffa;
  background-color: rgba(255, 255, 0, .4);
}

/* Used to force a border model for a node */
.cm-force-border { padding-right: .1px; }

@media print {
  /* Hide the cursor when printing */
  .CodeMirror div.CodeMirror-cursors {
    visibility: hidden;
  }
}

/* See issue #2901 */
.cm-tab-wrap-hack:after { content: ''; }

/* Help users use markselection to safely style text background */
span.CodeMirror-selectedtext { background: none; }

.CodeMirror-dialog {
  position: absolute;
  left: 0; right: 0;
  background: inherit;
  z-index: 15;
  padding: .1em .8em;
  overflow: hidden;
  color: inherit;
}

.CodeMirror-dialog-top {
  border-bottom: 1px solid #eee;
  top: 0;
}

.CodeMirror-dialog-bottom {
  border-top: 1px solid #eee;
  bottom: 0;
}

.CodeMirror-dialog input {
  border: none;
  outline: none;
  background: transparent;
  width: 20em;
  color: inherit;
  font-family: monospace;
}

.CodeMirror-dialog button {
  font-size: 70%;
}

.CodeMirror-foldmarker {
  color: blue;
  text-shadow: #b9f 1px 1px 2px, #b9f -1px -1px 2px, #b9f 1px -1px 2px, #b9f -1px 1px 2px;
  font-family: arial;
  line-height: .3;
  cursor: pointer;
}
.CodeMirror-foldgutter {
  width: .7em;
}
.CodeMirror-foldgutter-open,
.CodeMirror-foldgutter-folded {
  cursor: pointer;
}
.CodeMirror-foldgutter-open:after {
  content: "\25BE";
}
.CodeMirror-foldgutter-folded:after {
  content: "\25B8";
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

.CodeMirror {
  line-height: var(--jp-code-line-height);
  font-size: var(--jp-code-font-size);
  font-family: var(--jp-code-font-family);
  border: 0;
  border-radius: 0;
  height: auto;
  /* Changed to auto to autogrow */
}

.CodeMirror pre {
  padding: 0 var(--jp-code-padding);
}

.jp-CodeMirrorEditor[data-type='inline'] .CodeMirror-dialog {
  background-color: var(--jp-layout-color0);
  color: var(--jp-content-font-color1);
}

/* This causes https://github.com/jupyter/jupyterlab/issues/522 */
/* May not cause it not because we changed it! */
.CodeMirror-lines {
  padding: var(--jp-code-padding) 0;
}

.CodeMirror-linenumber {
  padding: 0 8px;
}

.jp-CodeMirrorEditor {
  cursor: text;
}

.jp-CodeMirrorEditor[data-type='inline'] .CodeMirror-cursor {
  border-left: var(--jp-code-cursor-width0) solid var(--jp-editor-cursor-color);
}

/* When zoomed out 67% and 33% on a screen of 1440 width x 900 height */
@media screen and (min-width: 2138px) and (max-width: 4319px) {
  .jp-CodeMirrorEditor[data-type='inline'] .CodeMirror-cursor {
    border-left: var(--jp-code-cursor-width1) solid
      var(--jp-editor-cursor-color);
  }
}

/* When zoomed out less than 33% */
@media screen and (min-width: 4320px) {
  .jp-CodeMirrorEditor[data-type='inline'] .CodeMirror-cursor {
    border-left: var(--jp-code-cursor-width2) solid
      var(--jp-editor-cursor-color);
  }
}

.CodeMirror.jp-mod-readOnly .CodeMirror-cursor {
  display: none;
}

.CodeMirror-gutters {
  border-right: 1px solid var(--jp-border-color2);
  background-color: var(--jp-layout-color0);
}

.jp-CollaboratorCursor {
  border-left: 5px solid transparent;
  border-right: 5px solid transparent;
  border-top: none;
  border-bottom: 3px solid;
  background-clip: content-box;
  margin-left: -5px;
  margin-right: -5px;
}

.CodeMirror-selectedtext.cm-searching {
  background-color: var(--jp-search-selected-match-background-color) !important;
  color: var(--jp-search-selected-match-color) !important;
}

.cm-searching {
  background-color: var(
    --jp-search-unselected-match-background-color
  ) !important;
  color: var(--jp-search-unselected-match-color) !important;
}

.CodeMirror-focused .CodeMirror-selected {
  background-color: var(--jp-editor-selected-focused-background);
}

.CodeMirror-selected {
  background-color: var(--jp-editor-selected-background);
}

.jp-CollaboratorCursor-hover {
  position: absolute;
  z-index: 1;
  transform: translateX(-50%);
  color: white;
  border-radius: 3px;
  padding-left: 4px;
  padding-right: 4px;
  padding-top: 1px;
  padding-bottom: 1px;
  text-align: center;
  font-size: var(--jp-ui-font-size1);
  white-space: nowrap;
}

.jp-CodeMirror-ruler {
  border-left: 1px dashed var(--jp-border-color2);
}

/**
 * Here is our jupyter theme for CodeMirror syntax highlighting
 * This is used in our marked.js syntax highlighting and CodeMirror itself
 * The string "jupyter" is set in ../codemirror/widget.DEFAULT_CODEMIRROR_THEME
 * This came from the classic notebook, which came form highlight.js/GitHub
 */

/**
 * CodeMirror themes are handling the background/color in this way. This works
 * fine for CodeMirror editors outside the notebook, but the notebook styles
 * these things differently.
 */
.CodeMirror.cm-s-jupyter {
  background: var(--jp-layout-color0);
  color: var(--jp-content-font-color1);
}

/* In the notebook, we want this styling to be handled by its container */
.jp-CodeConsole .CodeMirror.cm-s-jupyter,
.jp-Notebook .CodeMirror.cm-s-jupyter {
  background: transparent;
}

.cm-s-jupyter .CodeMirror-cursor {
  border-left: var(--jp-code-cursor-width0) solid var(--jp-editor-cursor-color);
}
.cm-s-jupyter span.cm-keyword {
  color: var(--jp-mirror-editor-keyword-color);
  font-weight: bold;
}
.cm-s-jupyter span.cm-atom {
  color: var(--jp-mirror-editor-atom-color);
}
.cm-s-jupyter span.cm-number {
  color: var(--jp-mirror-editor-number-color);
}
.cm-s-jupyter span.cm-def {
  color: var(--jp-mirror-editor-def-color);
}
.cm-s-jupyter span.cm-variable {
  color: var(--jp-mirror-editor-variable-color);
}
.cm-s-jupyter span.cm-variable-2 {
  color: var(--jp-mirror-editor-variable-2-color);
}
.cm-s-jupyter span.cm-variable-3 {
  color: var(--jp-mirror-editor-variable-3-color);
}
.cm-s-jupyter span.cm-punctuation {
  color: var(--jp-mirror-editor-punctuation-color);
}
.cm-s-jupyter span.cm-property {
  color: var(--jp-mirror-editor-property-color);
}
.cm-s-jupyter span.cm-operator {
  color: var(--jp-mirror-editor-operator-color);
  font-weight: bold;
}
.cm-s-jupyter span.cm-comment {
  color: var(--jp-mirror-editor-comment-color);
  font-style: italic;
}
.cm-s-jupyter span.cm-string {
  color: var(--jp-mirror-editor-string-color);
}
.cm-s-jupyter span.cm-string-2 {
  color: var(--jp-mirror-editor-string-2-color);
}
.cm-s-jupyter span.cm-meta {
  color: var(--jp-mirror-editor-meta-color);
}
.cm-s-jupyter span.cm-qualifier {
  color: var(--jp-mirror-editor-qualifier-color);
}
.cm-s-jupyter span.cm-builtin {
  color: var(--jp-mirror-editor-builtin-color);
}
.cm-s-jupyter span.cm-bracket {
  color: var(--jp-mirror-editor-bracket-color);
}
.cm-s-jupyter span.cm-tag {
  color: var(--jp-mirror-editor-tag-color);
}
.cm-s-jupyter span.cm-attribute {
  color: var(--jp-mirror-editor-attribute-color);
}
.cm-s-jupyter span.cm-header {
  color: var(--jp-mirror-editor-header-color);
}
.cm-s-jupyter span.cm-quote {
  color: var(--jp-mirror-editor-quote-color);
}
.cm-s-jupyter span.cm-link {
  color: var(--jp-mirror-editor-link-color);
}
.cm-s-jupyter span.cm-error {
  color: var(--jp-mirror-editor-error-color);
}
.cm-s-jupyter span.cm-hr {
  color: #999;
}

.cm-s-jupyter span.cm-tab {
  background: url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADAAAAAMCAYAAAAkuj5RAAAAAXNSR0IArs4c6QAAAGFJREFUSMft1LsRQFAQheHPowAKoACx3IgEKtaEHujDjORSgWTH/ZOdnZOcM/sgk/kFFWY0qV8foQwS4MKBCS3qR6ixBJvElOobYAtivseIE120FaowJPN75GMu8j/LfMwNjh4HUpwg4LUAAAAASUVORK5CYII=);
  background-position: right;
  background-repeat: no-repeat;
}

.cm-s-jupyter .CodeMirror-activeline-background,
.cm-s-jupyter .CodeMirror-gutter {
  background-color: var(--jp-layout-color2);
}

/* Styles for shared cursors (remote cursor locations and selected ranges) */
.jp-CodeMirrorEditor .remote-caret {
  position: relative;
  border-left: 2px solid black;
  margin-left: -1px;
  margin-right: -1px;
  box-sizing: border-box;
}

.jp-CodeMirrorEditor .remote-caret > div {
  white-space: nowrap;
  position: absolute;
  top: -1.15em;
  padding-bottom: 0.05em;
  left: -2px;
  font-size: 0.95em;
  background-color: rgb(250, 129, 0);
  font-family: var(--jp-ui-font-family);
  font-weight: bold;
  line-height: normal;
  user-select: none;
  color: white;
  padding-left: 2px;
  padding-right: 2px;
  z-index: 3;
  transition: opacity 0.3s ease-in-out;
}

.jp-CodeMirrorEditor .remote-caret.hide-name > div {
  transition-delay: 0.7s;
  opacity: 0;
}

.jp-CodeMirrorEditor .remote-caret:hover > div {
  opacity: 1;
  transition-delay: 0s;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

/*-----------------------------------------------------------------------------
| RenderedText
|----------------------------------------------------------------------------*/

:root {
  /* This is the padding value to fill the gaps between lines containing spans with background color. */
  --jp-private-code-span-padding: calc(
    (var(--jp-code-line-height) - 1) * var(--jp-code-font-size) / 2
  );
}

.jp-RenderedText {
  text-align: left;
  padding-left: var(--jp-code-padding);
  line-height: var(--jp-code-line-height);
  font-family: var(--jp-code-font-family);
}

.jp-RenderedText pre,
.jp-RenderedJavaScript pre,
.jp-RenderedHTMLCommon pre {
  color: var(--jp-content-font-color1);
  font-size: var(--jp-code-font-size);
  border: none;
  margin: 0px;
  padding: 0px;
}

.jp-RenderedText pre a:link {
  text-decoration: none;
  color: var(--jp-content-link-color);
}
.jp-RenderedText pre a:hover {
  text-decoration: underline;
  color: var(--jp-content-link-color);
}
.jp-RenderedText pre a:visited {
  text-decoration: none;
  color: var(--jp-content-link-color);
}

/* console foregrounds and backgrounds */
.jp-RenderedText pre .ansi-black-fg {
  color: #3e424d;
}
.jp-RenderedText pre .ansi-red-fg {
  color: #e75c58;
}
.jp-RenderedText pre .ansi-green-fg {
  color: #00a250;
}
.jp-RenderedText pre .ansi-yellow-fg {
  color: #ddb62b;
}
.jp-RenderedText pre .ansi-blue-fg {
  color: #208ffb;
}
.jp-RenderedText pre .ansi-magenta-fg {
  color: #d160c4;
}
.jp-RenderedText pre .ansi-cyan-fg {
  color: #60c6c8;
}
.jp-RenderedText pre .ansi-white-fg {
  color: #c5c1b4;
}

.jp-RenderedText pre .ansi-black-bg {
  background-color: #3e424d;
  padding: var(--jp-private-code-span-padding) 0;
}
.jp-RenderedText pre .ansi-red-bg {
  background-color: #e75c58;
  padding: var(--jp-private-code-span-padding) 0;
}
.jp-RenderedText pre .ansi-green-bg {
  background-color: #00a250;
  padding: var(--jp-private-code-span-padding) 0;
}
.jp-RenderedText pre .ansi-yellow-bg {
  background-color: #ddb62b;
  padding: var(--jp-private-code-span-padding) 0;
}
.jp-RenderedText pre .ansi-blue-bg {
  background-color: #208ffb;
  padding: var(--jp-private-code-span-padding) 0;
}
.jp-RenderedText pre .ansi-magenta-bg {
  background-color: #d160c4;
  padding: var(--jp-private-code-span-padding) 0;
}
.jp-RenderedText pre .ansi-cyan-bg {
  background-color: #60c6c8;
  padding: var(--jp-private-code-span-padding) 0;
}
.jp-RenderedText pre .ansi-white-bg {
  background-color: #c5c1b4;
  padding: var(--jp-private-code-span-padding) 0;
}

.jp-RenderedText pre .ansi-black-intense-fg {
  color: #282c36;
}
.jp-RenderedText pre .ansi-red-intense-fg {
  color: #b22b31;
}
.jp-RenderedText pre .ansi-green-intense-fg {
  color: #007427;
}
.jp-RenderedText pre .ansi-yellow-intense-fg {
  color: #b27d12;
}
.jp-RenderedText pre .ansi-blue-intense-fg {
  color: #0065ca;
}
.jp-RenderedText pre .ansi-magenta-intense-fg {
  color: #a03196;
}
.jp-RenderedText pre .ansi-cyan-intense-fg {
  color: #258f8f;
}
.jp-RenderedText pre .ansi-white-intense-fg {
  color: #a1a6b2;
}

.jp-RenderedText pre .ansi-black-intense-bg {
  background-color: #282c36;
  padding: var(--jp-private-code-span-padding) 0;
}
.jp-RenderedText pre .ansi-red-intense-bg {
  background-color: #b22b31;
  padding: var(--jp-private-code-span-padding) 0;
}
.jp-RenderedText pre .ansi-green-intense-bg {
  background-color: #007427;
  padding: var(--jp-private-code-span-padding) 0;
}
.jp-RenderedText pre .ansi-yellow-intense-bg {
  background-color: #b27d12;
  padding: var(--jp-private-code-span-padding) 0;
}
.jp-RenderedText pre .ansi-blue-intense-bg {
  background-color: #0065ca;
  padding: var(--jp-private-code-span-padding) 0;
}
.jp-RenderedText pre .ansi-magenta-intense-bg {
  background-color: #a03196;
  padding: var(--jp-private-code-span-padding) 0;
}
.jp-RenderedText pre .ansi-cyan-intense-bg {
  background-color: #258f8f;
  padding: var(--jp-private-code-span-padding) 0;
}
.jp-RenderedText pre .ansi-white-intense-bg {
  background-color: #a1a6b2;
  padding: var(--jp-private-code-span-padding) 0;
}

.jp-RenderedText pre .ansi-default-inverse-fg {
  color: var(--jp-ui-inverse-font-color0);
}
.jp-RenderedText pre .ansi-default-inverse-bg {
  background-color: var(--jp-inverse-layout-color0);
  padding: var(--jp-private-code-span-padding) 0;
}

.jp-RenderedText pre .ansi-bold {
  font-weight: bold;
}
.jp-RenderedText pre .ansi-underline {
  text-decoration: underline;
}

.jp-RenderedText[data-mime-type='application/vnd.jupyter.stderr'] {
  background: var(--jp-rendermime-error-background);
  padding-top: var(--jp-code-padding);
}

/*-----------------------------------------------------------------------------
| RenderedLatex
|----------------------------------------------------------------------------*/

.jp-RenderedLatex {
  color: var(--jp-content-font-color1);
  font-size: var(--jp-content-font-size1);
  line-height: var(--jp-content-line-height);
}

/* Left-justify outputs.*/
.jp-OutputArea-output.jp-RenderedLatex {
  padding: var(--jp-code-padding);
  text-align: left;
}

/*-----------------------------------------------------------------------------
| RenderedHTML
|----------------------------------------------------------------------------*/

.jp-RenderedHTMLCommon {
  color: var(--jp-content-font-color1);
  font-family: var(--jp-content-font-family);
  font-size: var(--jp-content-font-size1);
  line-height: var(--jp-content-line-height);
  /* Give a bit more R padding on Markdown text to keep line lengths reasonable */
  padding-right: 20px;
}

.jp-RenderedHTMLCommon em {
  font-style: italic;
}

.jp-RenderedHTMLCommon strong {
  font-weight: bold;
}

.jp-RenderedHTMLCommon u {
  text-decoration: underline;
}

.jp-RenderedHTMLCommon a:link {
  text-decoration: none;
  color: var(--jp-content-link-color);
}

.jp-RenderedHTMLCommon a:hover {
  text-decoration: underline;
  color: var(--jp-content-link-color);
}

.jp-RenderedHTMLCommon a:visited {
  text-decoration: none;
  color: var(--jp-content-link-color);
}

/* Headings */

.jp-RenderedHTMLCommon h1,
.jp-RenderedHTMLCommon h2,
.jp-RenderedHTMLCommon h3,
.jp-RenderedHTMLCommon h4,
.jp-RenderedHTMLCommon h5,
.jp-RenderedHTMLCommon h6 {
  line-height: var(--jp-content-heading-line-height);
  font-weight: var(--jp-content-heading-font-weight);
  font-style: normal;
  margin: var(--jp-content-heading-margin-top) 0
    var(--jp-content-heading-margin-bottom) 0;
}

.jp-RenderedHTMLCommon h1:first-child,
.jp-RenderedHTMLCommon h2:first-child,
.jp-RenderedHTMLCommon h3:first-child,
.jp-RenderedHTMLCommon h4:first-child,
.jp-RenderedHTMLCommon h5:first-child,
.jp-RenderedHTMLCommon h6:first-child {
  margin-top: calc(0.5 * var(--jp-content-heading-margin-top));
}

.jp-RenderedHTMLCommon h1:last-child,
.jp-RenderedHTMLCommon h2:last-child,
.jp-RenderedHTMLCommon h3:last-child,
.jp-RenderedHTMLCommon h4:last-child,
.jp-RenderedHTMLCommon h5:last-child,
.jp-RenderedHTMLCommon h6:last-child {
  margin-bottom: calc(0.5 * var(--jp-content-heading-margin-bottom));
}

.jp-RenderedHTMLCommon h1 {
  font-size: var(--jp-content-font-size5);
}

.jp-RenderedHTMLCommon h2 {
  font-size: var(--jp-content-font-size4);
}

.jp-RenderedHTMLCommon h3 {
  font-size: var(--jp-content-font-size3);
}

.jp-RenderedHTMLCommon h4 {
  font-size: var(--jp-content-font-size2);
}

.jp-RenderedHTMLCommon h5 {
  font-size: var(--jp-content-font-size1);
}

.jp-RenderedHTMLCommon h6 {
  font-size: var(--jp-content-font-size0);
}

/* Lists */

.jp-RenderedHTMLCommon ul:not(.list-inline),
.jp-RenderedHTMLCommon ol:not(.list-inline) {
  padding-left: 2em;
}

.jp-RenderedHTMLCommon ul {
  list-style: disc;
}

.jp-RenderedHTMLCommon ul ul {
  list-style: square;
}

.jp-RenderedHTMLCommon ul ul ul {
  list-style: circle;
}

.jp-RenderedHTMLCommon ol {
  list-style: decimal;
}

.jp-RenderedHTMLCommon ol ol {
  list-style: upper-alpha;
}

.jp-RenderedHTMLCommon ol ol ol {
  list-style: lower-alpha;
}

.jp-RenderedHTMLCommon ol ol ol ol {
  list-style: lower-roman;
}

.jp-RenderedHTMLCommon ol ol ol ol ol {
  list-style: decimal;
}

.jp-RenderedHTMLCommon ol,
.jp-RenderedHTMLCommon ul {
  margin-bottom: 1em;
}

.jp-RenderedHTMLCommon ul ul,
.jp-RenderedHTMLCommon ul ol,
.jp-RenderedHTMLCommon ol ul,
.jp-RenderedHTMLCommon ol ol {
  margin-bottom: 0em;
}

.jp-RenderedHTMLCommon hr {
  color: var(--jp-border-color2);
  background-color: var(--jp-border-color1);
  margin-top: 1em;
  margin-bottom: 1em;
}

.jp-RenderedHTMLCommon > pre {
  margin: 1.5em 2em;
}

.jp-RenderedHTMLCommon pre,
.jp-RenderedHTMLCommon code {
  border: 0;
  background-color: var(--jp-layout-color0);
  color: var(--jp-content-font-color1);
  font-family: var(--jp-code-font-family);
  font-size: inherit;
  line-height: var(--jp-code-line-height);
  padding: 0;
  white-space: pre-wrap;
}

.jp-RenderedHTMLCommon :not(pre) > code {
  background-color: var(--jp-layout-color2);
  padding: 1px 5px;
}

/* Tables */

.jp-RenderedHTMLCommon table {
  border-collapse: collapse;
  border-spacing: 0;
  border: none;
  color: var(--jp-ui-font-color1);
  font-size: 12px;
  table-layout: fixed;
  margin-left: auto;
  margin-right: auto;
}

.jp-RenderedHTMLCommon thead {
  border-bottom: var(--jp-border-width) solid var(--jp-border-color1);
  vertical-align: bottom;
}

.jp-RenderedHTMLCommon td,
.jp-RenderedHTMLCommon th,
.jp-RenderedHTMLCommon tr {
  vertical-align: middle;
  padding: 0.5em 0.5em;
  line-height: normal;
  white-space: normal;
  max-width: none;
  border: none;
}

.jp-RenderedMarkdown.jp-RenderedHTMLCommon td,
.jp-RenderedMarkdown.jp-RenderedHTMLCommon th {
  max-width: none;
}

:not(.jp-RenderedMarkdown).jp-RenderedHTMLCommon td,
:not(.jp-RenderedMarkdown).jp-RenderedHTMLCommon th,
:not(.jp-RenderedMarkdown).jp-RenderedHTMLCommon tr {
  text-align: right;
}

.jp-RenderedHTMLCommon th {
  font-weight: bold;
}

.jp-RenderedHTMLCommon tbody tr:nth-child(odd) {
  background: var(--jp-layout-color0);
}

.jp-RenderedHTMLCommon tbody tr:nth-child(even) {
  background: var(--jp-rendermime-table-row-background);
}

.jp-RenderedHTMLCommon tbody tr:hover {
  background: var(--jp-rendermime-table-row-hover-background);
}

.jp-RenderedHTMLCommon table {
  margin-bottom: 1em;
}

.jp-RenderedHTMLCommon p {
  text-align: left;
  margin: 0px;
}

.jp-RenderedHTMLCommon p {
  margin-bottom: 1em;
}

.jp-RenderedHTMLCommon img {
  -moz-force-broken-image-icon: 1;
}

/* Restrict to direct children as other images could be nested in other content. */
.jp-RenderedHTMLCommon > img {
  display: block;
  margin-left: 0;
  margin-right: 0;
  margin-bottom: 1em;
}

/* Change color behind transparent images if they need it... */
[data-jp-theme-light='false'] .jp-RenderedImage img.jp-needs-light-background {
  background-color: var(--jp-inverse-layout-color1);
}
[data-jp-theme-light='true'] .jp-RenderedImage img.jp-needs-dark-background {
  background-color: var(--jp-inverse-layout-color1);
}
/* ...or leave it untouched if they don't */
[data-jp-theme-light='false'] .jp-RenderedImage img.jp-needs-dark-background {
}
[data-jp-theme-light='true'] .jp-RenderedImage img.jp-needs-light-background {
}

.jp-RenderedHTMLCommon img,
.jp-RenderedImage img,
.jp-RenderedHTMLCommon svg,
.jp-RenderedSVG svg {
  max-width: 100%;
  height: auto;
}

.jp-RenderedHTMLCommon img.jp-mod-unconfined,
.jp-RenderedImage img.jp-mod-unconfined,
.jp-RenderedHTMLCommon svg.jp-mod-unconfined,
.jp-RenderedSVG svg.jp-mod-unconfined {
  max-width: none;
}

.jp-RenderedHTMLCommon .alert {
  padding: var(--jp-notebook-padding);
  border: var(--jp-border-width) solid transparent;
  border-radius: var(--jp-border-radius);
  margin-bottom: 1em;
}

.jp-RenderedHTMLCommon .alert-info {
  color: var(--jp-info-color0);
  background-color: var(--jp-info-color3);
  border-color: var(--jp-info-color2);
}
.jp-RenderedHTMLCommon .alert-info hr {
  border-color: var(--jp-info-color3);
}
.jp-RenderedHTMLCommon .alert-info > p:last-child,
.jp-RenderedHTMLCommon .alert-info > ul:last-child {
  margin-bottom: 0;
}

.jp-RenderedHTMLCommon .alert-warning {
  color: var(--jp-warn-color0);
  background-color: var(--jp-warn-color3);
  border-color: var(--jp-warn-color2);
}
.jp-RenderedHTMLCommon .alert-warning hr {
  border-color: var(--jp-warn-color3);
}
.jp-RenderedHTMLCommon .alert-warning > p:last-child,
.jp-RenderedHTMLCommon .alert-warning > ul:last-child {
  margin-bottom: 0;
}

.jp-RenderedHTMLCommon .alert-success {
  color: var(--jp-success-color0);
  background-color: var(--jp-success-color3);
  border-color: var(--jp-success-color2);
}
.jp-RenderedHTMLCommon .alert-success hr {
  border-color: var(--jp-success-color3);
}
.jp-RenderedHTMLCommon .alert-success > p:last-child,
.jp-RenderedHTMLCommon .alert-success > ul:last-child {
  margin-bottom: 0;
}

.jp-RenderedHTMLCommon .alert-danger {
  color: var(--jp-error-color0);
  background-color: var(--jp-error-color3);
  border-color: var(--jp-error-color2);
}
.jp-RenderedHTMLCommon .alert-danger hr {
  border-color: var(--jp-error-color3);
}
.jp-RenderedHTMLCommon .alert-danger > p:last-child,
.jp-RenderedHTMLCommon .alert-danger > ul:last-child {
  margin-bottom: 0;
}

.jp-RenderedHTMLCommon blockquote {
  margin: 1em 2em;
  padding: 0 1em;
  border-left: 5px solid var(--jp-border-color2);
}

a.jp-InternalAnchorLink {
  visibility: hidden;
  margin-left: 8px;
  color: var(--md-blue-800);
}

h1:hover .jp-InternalAnchorLink,
h2:hover .jp-InternalAnchorLink,
h3:hover .jp-InternalAnchorLink,
h4:hover .jp-InternalAnchorLink,
h5:hover .jp-InternalAnchorLink,
h6:hover .jp-InternalAnchorLink {
  visibility: visible;
}

.jp-RenderedHTMLCommon kbd {
  background-color: var(--jp-rendermime-table-row-background);
  border: 1px solid var(--jp-border-color0);
  border-bottom-color: var(--jp-border-color2);
  border-radius: 3px;
  box-shadow: inset 0 -1px 0 rgba(0, 0, 0, 0.25);
  display: inline-block;
  font-size: 0.8em;
  line-height: 1em;
  padding: 0.2em 0.5em;
}

/* Most direct children of .jp-RenderedHTMLCommon have a margin-bottom of 1.0.
 * At the bottom of cells this is a bit too much as there is also spacing
 * between cells. Going all the way to 0 gets too tight between markdown and
 * code cells.
 */
.jp-RenderedHTMLCommon > *:last-child {
  margin-bottom: 0.5em;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

.jp-MimeDocument {
  outline: none;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

/*-----------------------------------------------------------------------------
| Variables
|----------------------------------------------------------------------------*/

:root {
  --jp-private-filebrowser-button-height: 28px;
  --jp-private-filebrowser-button-width: 48px;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

.jp-FileBrowser {
  display: flex;
  flex-direction: column;
  color: var(--jp-ui-font-color1);
  background: var(--jp-layout-color1);
  /* This is needed so that all font sizing of children done in ems is
   * relative to this base size */
  font-size: var(--jp-ui-font-size1);
}

.jp-FileBrowser-toolbar.jp-Toolbar {
  border-bottom: none;
  height: auto;
  margin: var(--jp-toolbar-header-margin);
  box-shadow: none;
}

.jp-BreadCrumbs {
  flex: 0 0 auto;
  margin: 8px 12px 8px 12px;
}

.jp-BreadCrumbs-item {
  margin: 0px 2px;
  padding: 0px 2px;
  border-radius: var(--jp-border-radius);
  cursor: pointer;
}

.jp-BreadCrumbs-item:hover {
  background-color: var(--jp-layout-color2);
}

.jp-BreadCrumbs-item:first-child {
  margin-left: 0px;
}

.jp-BreadCrumbs-item.jp-mod-dropTarget {
  background-color: var(--jp-brand-color2);
  opacity: 0.7;
}

/*-----------------------------------------------------------------------------
| Buttons
|----------------------------------------------------------------------------*/

.jp-FileBrowser-toolbar.jp-Toolbar {
  padding: 0px;
  margin: 8px 12px 0px 12px;
}

.jp-FileBrowser-toolbar.jp-Toolbar {
  justify-content: flex-start;
}

.jp-FileBrowser-toolbar.jp-Toolbar .jp-Toolbar-item {
  flex: 0 0 auto;
  padding-left: 0px;
  padding-right: 2px;
}

.jp-FileBrowser-toolbar.jp-Toolbar .jp-ToolbarButtonComponent {
  width: 40px;
}

.jp-FileBrowser-toolbar.jp-Toolbar
  .jp-Toolbar-item:first-child
  .jp-ToolbarButtonComponent {
  width: 72px;
  background: var(--jp-brand-color1);
}

.jp-FileBrowser-toolbar.jp-Toolbar
  .jp-Toolbar-item:first-child
  .jp-ToolbarButtonComponent:focus-visible {
  background-color: var(--jp-brand-color0);
}

.jp-FileBrowser-toolbar.jp-Toolbar
  .jp-Toolbar-item:first-child
  .jp-ToolbarButtonComponent
  .jp-icon3 {
  fill: white;
}

/*-----------------------------------------------------------------------------
| Other styles
|----------------------------------------------------------------------------*/

.jp-FileDialog.jp-mod-conflict input {
  color: var(--jp-error-color1);
}

.jp-FileDialog .jp-new-name-title {
  margin-top: 12px;
}

.jp-LastModified-hidden {
  display: none;
}

.jp-FileBrowser-filterBox {
  padding: 0px;
  flex: 0 0 auto;
  margin: 8px 12px 0px 12px;
}

/*-----------------------------------------------------------------------------
| DirListing
|----------------------------------------------------------------------------*/

.jp-DirListing {
  flex: 1 1 auto;
  display: flex;
  flex-direction: column;
  outline: 0;
}

.jp-DirListing:focus-visible {
  border: 1px solid var(--jp-brand-color1);
}

.jp-DirListing-header {
  flex: 0 0 auto;
  display: flex;
  flex-direction: row;
  overflow: hidden;
  border-top: var(--jp-border-width) solid var(--jp-border-color2);
  border-bottom: var(--jp-border-width) solid var(--jp-border-color1);
  box-shadow: var(--jp-toolbar-box-shadow);
  z-index: 2;
}

.jp-DirListing-headerItem {
  padding: 4px 12px 2px 12px;
  font-weight: 500;
}

.jp-DirListing-headerItem:hover {
  background: var(--jp-layout-color2);
}

.jp-DirListing-headerItem.jp-id-name {
  flex: 1 0 84px;
}

.jp-DirListing-headerItem.jp-id-modified {
  flex: 0 0 112px;
  border-left: var(--jp-border-width) solid var(--jp-border-color2);
  text-align: right;
}

.jp-id-narrow {
  display: none;
  flex: 0 0 5px;
  padding: 4px 4px;
  border-left: var(--jp-border-width) solid var(--jp-border-color2);
  text-align: right;
  color: var(--jp-border-color2);
}

.jp-DirListing-narrow .jp-id-narrow {
  display: block;
}

.jp-DirListing-narrow .jp-id-modified,
.jp-DirListing-narrow .jp-DirListing-itemModified {
  display: none;
}

.jp-DirListing-headerItem.jp-mod-selected {
  font-weight: 600;
}

/* increase specificity to override bundled default */
.jp-DirListing-content {
  flex: 1 1 auto;
  margin: 0;
  padding: 0;
  list-style-type: none;
  overflow: auto;
  background-color: var(--jp-layout-color1);
}

.jp-DirListing-content mark {
  color: var(--jp-ui-font-color0);
  background-color: transparent;
  font-weight: bold;
}

.jp-DirListing-content .jp-DirListing-item.jp-mod-selected mark {
  color: var(--jp-ui-inverse-font-color0);
}

/* Style the directory listing content when a user drops a file to upload */
.jp-DirListing.jp-mod-native-drop .jp-DirListing-content {
  outline: 5px dashed rgba(128, 128, 128, 0.5);
  outline-offset: -10px;
  cursor: copy;
}

.jp-DirListing-item {
  display: flex;
  flex-direction: row;
  padding: 4px 12px;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}

.jp-DirListing-item[data-is-dot] {
  opacity: 75%;
}

.jp-DirListing-item.jp-mod-selected {
  color: var(--jp-ui-inverse-font-color1);
  background: var(--jp-brand-color1);
}

.jp-DirListing-item.jp-mod-dropTarget {
  background: var(--jp-brand-color3);
}

.jp-DirListing-item:hover:not(.jp-mod-selected) {
  background: var(--jp-layout-color2);
}

.jp-DirListing-itemIcon {
  flex: 0 0 20px;
  margin-right: 4px;
}

.jp-DirListing-itemText {
  flex: 1 0 64px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  user-select: none;
}

.jp-DirListing-itemModified {
  flex: 0 0 125px;
  text-align: right;
}

.jp-DirListing-editor {
  flex: 1 0 64px;
  outline: none;
  border: none;
}

.jp-DirListing-item.jp-mod-running .jp-DirListing-itemIcon:before {
  color: var(--jp-success-color1);
  content: '\25CF';
  font-size: 8px;
  position: absolute;
  left: -8px;
}

.jp-DirListing-item.jp-mod-running.jp-mod-selected
  .jp-DirListing-itemIcon:before {
  color: var(--jp-ui-inverse-font-color1);
}

.jp-DirListing-item.lm-mod-drag-image,
.jp-DirListing-item.jp-mod-selected.lm-mod-drag-image {
  font-size: var(--jp-ui-font-size1);
  padding-left: 4px;
  margin-left: 4px;
  width: 160px;
  background-color: var(--jp-ui-inverse-font-color2);
  box-shadow: var(--jp-elevation-z2);
  border-radius: 0px;
  color: var(--jp-ui-font-color1);
  transform: translateX(-40%) translateY(-58%);
}

.jp-DirListing-deadSpace {
  flex: 1 1 auto;
  margin: 0;
  padding: 0;
  list-style-type: none;
  overflow: auto;
  background-color: var(--jp-layout-color1);
}

.jp-Document {
  min-width: 120px;
  min-height: 120px;
  outline: none;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

/*-----------------------------------------------------------------------------
| Private CSS variables
|----------------------------------------------------------------------------*/

:root {
}

/*-----------------------------------------------------------------------------
| Main OutputArea
| OutputArea has a list of Outputs
|----------------------------------------------------------------------------*/

.jp-OutputArea {
  overflow-y: auto;
}

.jp-OutputArea-child {
  display: flex;
  flex-direction: row;
}

body[data-format='mobile'] .jp-OutputArea-child {
  flex-direction: column;
}

.jp-OutputPrompt {
  flex: 0 0 var(--jp-cell-prompt-width);
  color: var(--jp-cell-outprompt-font-color);
  font-family: var(--jp-cell-prompt-font-family);
  padding: var(--jp-code-padding);
  letter-spacing: var(--jp-cell-prompt-letter-spacing);
  line-height: var(--jp-code-line-height);
  font-size: var(--jp-code-font-size);
  border: var(--jp-border-width) solid transparent;
  opacity: var(--jp-cell-prompt-opacity);
  /* Right align prompt text, don't wrap to handle large prompt numbers */
  text-align: right;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  /* Disable text selection */
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}

body[data-format='mobile'] .jp-OutputPrompt {
  flex: 0 0 auto;
  text-align: left;
}

.jp-OutputArea-output {
  height: auto;
  overflow: auto;
  user-select: text;
  -moz-user-select: text;
  -webkit-user-select: text;
  -ms-user-select: text;
}

.jp-OutputArea-child .jp-OutputArea-output {
  flex-grow: 1;
  flex-shrink: 1;
}

body[data-format='mobile'] .jp-OutputArea-child .jp-OutputArea-output {
  margin-left: var(--jp-notebook-padding);
}

/**
 * Isolated output.
 */
.jp-OutputArea-output.jp-mod-isolated {
  width: 100%;
  display: block;
}

/*
When drag events occur, `p-mod-override-cursor` is added to the body.
Because iframes steal all cursor events, the following two rules are necessary
to suppress pointer events while resize drags are occurring. There may be a
better solution to this problem.
*/
body.lm-mod-override-cursor .jp-OutputArea-output.jp-mod-isolated {
  position: relative;
}

body.lm-mod-override-cursor .jp-OutputArea-output.jp-mod-isolated:before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: transparent;
}

/* pre */

.jp-OutputArea-output pre {
  border: none;
  margin: 0px;
  padding: 0px;
  overflow-x: auto;
  overflow-y: auto;
  word-break: break-all;
  word-wrap: break-word;
  white-space: pre-wrap;
}

/* tables */

.jp-OutputArea-output.jp-RenderedHTMLCommon table {
  margin-left: 0;
  margin-right: 0;
}

/* description lists */

.jp-OutputArea-output dl,
.jp-OutputArea-output dt,
.jp-OutputArea-output dd {
  display: block;
}

.jp-OutputArea-output dl {
  width: 100%;
  overflow: hidden;
  padding: 0;
  margin: 0;
}

.jp-OutputArea-output dt {
  font-weight: bold;
  float: left;
  width: 20%;
  padding: 0;
  margin: 0;
}

.jp-OutputArea-output dd {
  float: left;
  width: 80%;
  padding: 0;
  margin: 0;
}

/* Hide the gutter in case of
 *  - nested output areas (e.g. in the case of output widgets)
 *  - mirrored output areas
 */
.jp-OutputArea .jp-OutputArea .jp-OutputArea-prompt {
  display: none;
}

/*-----------------------------------------------------------------------------
| executeResult is added to any Output-result for the display of the object
| returned by a cell
|----------------------------------------------------------------------------*/

.jp-OutputArea-output.jp-OutputArea-executeResult {
  margin-left: 0px;
  flex: 1 1 auto;
}

/* Text output with the Out[] prompt needs a top padding to match the
 * alignment of the Out[] prompt itself.
 */
.jp-OutputArea-executeResult .jp-RenderedText.jp-OutputArea-output {
  padding-top: var(--jp-code-padding);
  border-top: var(--jp-border-width) solid transparent;
}

/*-----------------------------------------------------------------------------
| The Stdin output
|----------------------------------------------------------------------------*/

.jp-OutputArea-stdin {
  line-height: var(--jp-code-line-height);
  padding-top: var(--jp-code-padding);
  display: flex;
}

.jp-Stdin-prompt {
  color: var(--jp-content-font-color0);
  padding-right: var(--jp-code-padding);
  vertical-align: baseline;
  flex: 0 0 auto;
}

.jp-Stdin-input {
  font-family: var(--jp-code-font-family);
  font-size: inherit;
  color: inherit;
  background-color: inherit;
  width: 42%;
  min-width: 200px;
  /* make sure input baseline aligns with prompt */
  vertical-align: baseline;
  /* padding + margin = 0.5em between prompt and cursor */
  padding: 0em 0.25em;
  margin: 0em 0.25em;
  flex: 0 0 70%;
}

.jp-Stdin-input:focus {
  box-shadow: none;
}

/*-----------------------------------------------------------------------------
| Output Area View
|----------------------------------------------------------------------------*/

.jp-LinkedOutputView .jp-OutputArea {
  height: 100%;
  display: block;
}

.jp-LinkedOutputView .jp-OutputArea-output:only-child {
  height: 100%;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

.jp-Collapser {
  flex: 0 0 var(--jp-cell-collapser-width);
  padding: 0px;
  margin: 0px;
  border: none;
  outline: none;
  background: transparent;
  border-radius: var(--jp-border-radius);
  opacity: 1;
}

.jp-Collapser-child {
  display: block;
  width: 100%;
  box-sizing: border-box;
  /* height: 100% doesn't work because the height of its parent is computed from content */
  position: absolute;
  top: 0px;
  bottom: 0px;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

/*-----------------------------------------------------------------------------
| Header/Footer
|----------------------------------------------------------------------------*/

/* Hidden by zero height by default */
.jp-CellHeader,
.jp-CellFooter {
  height: 0px;
  width: 100%;
  padding: 0px;
  margin: 0px;
  border: none;
  outline: none;
  background: transparent;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

/*-----------------------------------------------------------------------------
| Input
|----------------------------------------------------------------------------*/

/* All input areas */
.jp-InputArea {
  display: flex;
  flex-direction: row;
  overflow: hidden;
}

body[data-format='mobile'] .jp-InputArea {
  flex-direction: column;
}

.jp-InputArea-editor {
  flex: 1 1 auto;
  overflow: hidden;
}

.jp-InputArea-editor {
  /* This is the non-active, default styling */
  border: var(--jp-border-width) solid var(--jp-cell-editor-border-color);
  border-radius: 0px;
  background: var(--jp-cell-editor-background);
}

body[data-format='mobile'] .jp-InputArea-editor {
  margin-left: var(--jp-notebook-padding);
}

.jp-InputPrompt {
  flex: 0 0 var(--jp-cell-prompt-width);
  color: var(--jp-cell-inprompt-font-color);
  font-family: var(--jp-cell-prompt-font-family);
  padding: var(--jp-code-padding);
  letter-spacing: var(--jp-cell-prompt-letter-spacing);
  opacity: var(--jp-cell-prompt-opacity);
  line-height: var(--jp-code-line-height);
  font-size: var(--jp-code-font-size);
  border: var(--jp-border-width) solid transparent;
  opacity: var(--jp-cell-prompt-opacity);
  /* Right align prompt text, don't wrap to handle large prompt numbers */
  text-align: right;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  /* Disable text selection */
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}

body[data-format='mobile'] .jp-InputPrompt {
  flex: 0 0 auto;
  text-align: left;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

/*-----------------------------------------------------------------------------
| Placeholder
|----------------------------------------------------------------------------*/

.jp-Placeholder {
  display: flex;
  flex-direction: row;
  flex: 1 1 auto;
}

.jp-Placeholder-prompt {
  box-sizing: border-box;
}

.jp-Placeholder-content {
  flex: 1 1 auto;
  border: none;
  background: transparent;
  height: 20px;
  box-sizing: border-box;
}

.jp-Placeholder-content .jp-MoreHorizIcon {
  width: 32px;
  height: 16px;
  border: 1px solid transparent;
  border-radius: var(--jp-border-radius);
}

.jp-Placeholder-content .jp-MoreHorizIcon:hover {
  border: 1px solid var(--jp-border-color1);
  box-shadow: 0px 0px 2px 0px rgba(0, 0, 0, 0.25);
  background-color: var(--jp-layout-color0);
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

/*-----------------------------------------------------------------------------
| Private CSS variables
|----------------------------------------------------------------------------*/

:root {
  --jp-private-cell-scrolling-output-offset: 5px;
}

/*-----------------------------------------------------------------------------
| Cell
|----------------------------------------------------------------------------*/

.jp-Cell {
  padding: var(--jp-cell-padding);
  margin: 0px;
  border: none;
  outline: none;
  background: transparent;
}

/*-----------------------------------------------------------------------------
| Common input/output
|----------------------------------------------------------------------------*/

.jp-Cell-inputWrapper,
.jp-Cell-outputWrapper {
  display: flex;
  flex-direction: row;
  padding: 0px;
  margin: 0px;
  /* Added to reveal the box-shadow on the input and output collapsers. */
  overflow: visible;
}

/* Only input/output areas inside cells */
.jp-Cell-inputArea,
.jp-Cell-outputArea {
  flex: 1 1 auto;
}

/*-----------------------------------------------------------------------------
| Collapser
|----------------------------------------------------------------------------*/

/* Make the output collapser disappear when there is not output, but do so
 * in a manner that leaves it in the layout and preserves its width.
 */
.jp-Cell.jp-mod-noOutputs .jp-Cell-outputCollapser {
  border: none !important;
  background: transparent !important;
}

.jp-Cell:not(.jp-mod-noOutputs) .jp-Cell-outputCollapser {
  min-height: var(--jp-cell-collapser-min-height);
}

/*-----------------------------------------------------------------------------
| Output
|----------------------------------------------------------------------------*/

/* Put a space between input and output when there IS output */
.jp-Cell:not(.jp-mod-noOutputs) .jp-Cell-outputWrapper {
  margin-top: 5px;
}

.jp-CodeCell.jp-mod-outputsScrolled .jp-Cell-outputArea {
  overflow-y: auto;
  max-height: 200px;
  box-shadow: inset 0 0 6px 2px rgba(0, 0, 0, 0.3);
  margin-left: var(--jp-private-cell-scrolling-output-offset);
}

.jp-CodeCell.jp-mod-outputsScrolled .jp-OutputArea-prompt {
  flex: 0 0
    calc(
      var(--jp-cell-prompt-width) -
        var(--jp-private-cell-scrolling-output-offset)
    );
}

/*-----------------------------------------------------------------------------
| CodeCell
|----------------------------------------------------------------------------*/

/*-----------------------------------------------------------------------------
| MarkdownCell
|----------------------------------------------------------------------------*/

.jp-MarkdownOutput {
  flex: 1 1 auto;
  margin-top: 0;
  margin-bottom: 0;
  padding-left: var(--jp-code-padding);
}

.jp-MarkdownOutput.jp-RenderedHTMLCommon {
  overflow: auto;
}

.jp-showHiddenCellsButton {
  margin-left: calc(var(--jp-cell-prompt-width) + 2 * var(--jp-code-padding));
  margin-top: var(--jp-code-padding);
  border: 1px solid var(--jp-border-color2);
  background-color: var(--jp-border-color3) !important;
  color: var(--jp-content-font-color0) !important;
}

.jp-showHiddenCellsButton:hover {
  background-color: var(--jp-border-color2) !important;
}

.jp-collapseHeadingButton {
  display: none;
}

.jp-MarkdownCell:hover .jp-collapseHeadingButton {
  display: flex;
  min-height: var(--jp-cell-collapser-min-height);
  position: absolute;
  right: 0;
  top: 0;
  bottom: 0;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

/*-----------------------------------------------------------------------------
| Variables
|----------------------------------------------------------------------------*/

/*-----------------------------------------------------------------------------

/*-----------------------------------------------------------------------------
| Styles
|----------------------------------------------------------------------------*/

.jp-NotebookPanel-toolbar {
  padding: 2px;
}

.jp-Toolbar-item.jp-Notebook-toolbarCellType .jp-select-wrapper.jp-mod-focused {
  border: none;
  box-shadow: none;
}

.jp-Notebook-toolbarCellTypeDropdown select {
  height: 24px;
  font-size: var(--jp-ui-font-size1);
  line-height: 14px;
  border-radius: 0;
  display: block;
}

.jp-Notebook-toolbarCellTypeDropdown span {
  top: 5px !important;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

/*-----------------------------------------------------------------------------
| Private CSS variables
|----------------------------------------------------------------------------*/

:root {
  --jp-private-notebook-dragImage-width: 304px;
  --jp-private-notebook-dragImage-height: 36px;
  --jp-private-notebook-selected-color: var(--md-blue-400);
  --jp-private-notebook-active-color: var(--md-green-400);
}

/*-----------------------------------------------------------------------------
| Imports
|----------------------------------------------------------------------------*/

/*-----------------------------------------------------------------------------
| Notebook
|----------------------------------------------------------------------------*/

.jp-NotebookPanel {
  display: block;
  height: 100%;
}

.jp-NotebookPanel.jp-Document {
  min-width: 240px;
  min-height: 120px;
}

.jp-Notebook {
  padding: var(--jp-notebook-padding);
  outline: none;
  overflow: auto;
  background: var(--jp-layout-color0);
}

.jp-Notebook.jp-mod-scrollPastEnd::after {
  display: block;
  content: '';
  min-height: var(--jp-notebook-scroll-padding);
}

.jp-MainAreaWidget-ContainStrict .jp-Notebook * {
  contain: strict;
}

.jp-Notebook-render * {
  contain: none !important;
}

.jp-Notebook .jp-Cell {
  overflow: visible;
}

.jp-Notebook .jp-Cell .jp-InputPrompt {
  cursor: move;
  float: left;
}

/*-----------------------------------------------------------------------------
| Notebook state related styling
|
| The notebook and cells each have states, here are the possibilities:
|
| - Notebook
|   - Command
|   - Edit
| - Cell
|   - None
|   - Active (only one can be active)
|   - Selected (the cells actions are applied to)
|   - Multiselected (when multiple selected, the cursor)
|   - No outputs
|----------------------------------------------------------------------------*/

/* Command or edit modes */

.jp-Notebook .jp-Cell:not(.jp-mod-active) .jp-InputPrompt {
  opacity: var(--jp-cell-prompt-not-active-opacity);
  color: var(--jp-cell-prompt-not-active-font-color);
}

.jp-Notebook .jp-Cell:not(.jp-mod-active) .jp-OutputPrompt {
  opacity: var(--jp-cell-prompt-not-active-opacity);
  color: var(--jp-cell-prompt-not-active-font-color);
}

/* cell is active */
.jp-Notebook .jp-Cell.jp-mod-active .jp-Collapser {
  background: var(--jp-brand-color1);
}

/* cell is dirty */
.jp-Notebook .jp-Cell.jp-mod-dirty .jp-InputPrompt {
  color: var(--jp-warn-color1);
}
.jp-Notebook .jp-Cell.jp-mod-dirty .jp-InputPrompt:before {
  color: var(--jp-warn-color1);
  content: '•';
}

.jp-Notebook .jp-Cell.jp-mod-active.jp-mod-dirty .jp-Collapser {
  background: var(--jp-warn-color1);
}

/* collapser is hovered */
.jp-Notebook .jp-Cell .jp-Collapser:hover {
  box-shadow: var(--jp-elevation-z2);
  background: var(--jp-brand-color1);
  opacity: var(--jp-cell-collapser-not-active-hover-opacity);
}

/* cell is active and collapser is hovered */
.jp-Notebook .jp-Cell.jp-mod-active .jp-Collapser:hover {
  background: var(--jp-brand-color0);
  opacity: 1;
}

/* Command mode */

.jp-Notebook.jp-mod-commandMode .jp-Cell.jp-mod-selected {
  background: var(--jp-notebook-multiselected-color);
}

.jp-Notebook.jp-mod-commandMode
  .jp-Cell.jp-mod-active.jp-mod-selected:not(.jp-mod-multiSelected) {
  background: transparent;
}

/* Edit mode */

.jp-Notebook.jp-mod-editMode .jp-Cell.jp-mod-active .jp-InputArea-editor {
  border: var(--jp-border-width) solid var(--jp-cell-editor-active-border-color);
  box-shadow: var(--jp-input-box-shadow);
  background-color: var(--jp-cell-editor-active-background);
}

/*-----------------------------------------------------------------------------
| Notebook drag and drop
|----------------------------------------------------------------------------*/

.jp-Notebook-cell.jp-mod-dropSource {
  opacity: 0.5;
}

.jp-Notebook-cell.jp-mod-dropTarget,
.jp-Notebook.jp-mod-commandMode
  .jp-Notebook-cell.jp-mod-active.jp-mod-selected.jp-mod-dropTarget {
  border-top-color: var(--jp-private-notebook-selected-color);
  border-top-style: solid;
  border-top-width: 2px;
}

.jp-dragImage {
  display: block;
  flex-direction: row;
  width: var(--jp-private-notebook-dragImage-width);
  height: var(--jp-private-notebook-dragImage-height);
  border: var(--jp-border-width) solid var(--jp-cell-editor-border-color);
  background: var(--jp-cell-editor-background);
  overflow: visible;
}

.jp-dragImage-singlePrompt {
  box-shadow: 2px 2px 4px 0px rgba(0, 0, 0, 0.12);
}

.jp-dragImage .jp-dragImage-content {
  flex: 1 1 auto;
  z-index: 2;
  font-size: var(--jp-code-font-size);
  font-family: var(--jp-code-font-family);
  line-height: var(--jp-code-line-height);
  padding: var(--jp-code-padding);
  border: var(--jp-border-width) solid var(--jp-cell-editor-border-color);
  background: var(--jp-cell-editor-background-color);
  color: var(--jp-content-font-color3);
  text-align: left;
  margin: 4px 4px 4px 0px;
}

.jp-dragImage .jp-dragImage-prompt {
  flex: 0 0 auto;
  min-width: 36px;
  color: var(--jp-cell-inprompt-font-color);
  padding: var(--jp-code-padding);
  padding-left: 12px;
  font-family: var(--jp-cell-prompt-font-family);
  letter-spacing: var(--jp-cell-prompt-letter-spacing);
  line-height: 1.9;
  font-size: var(--jp-code-font-size);
  border: var(--jp-border-width) solid transparent;
}

.jp-dragImage-multipleBack {
  z-index: -1;
  position: absolute;
  height: 32px;
  width: 300px;
  top: 8px;
  left: 8px;
  background: var(--jp-layout-color2);
  border: var(--jp-border-width) solid var(--jp-input-border-color);
  box-shadow: 2px 2px 4px 0px rgba(0, 0, 0, 0.12);
}

/*-----------------------------------------------------------------------------
| Cell toolbar
|----------------------------------------------------------------------------*/

.jp-NotebookTools {
  display: block;
  min-width: var(--jp-sidebar-min-width);
  color: var(--jp-ui-font-color1);
  background: var(--jp-layout-color1);
  /* This is needed so that all font sizing of children done in ems is
    * relative to this base size */
  font-size: var(--jp-ui-font-size1);
  overflow: auto;
}

.jp-NotebookTools-tool {
  padding: 0px 12px 0 12px;
}

.jp-ActiveCellTool {
  padding: 12px;
  background-color: var(--jp-layout-color1);
  border-top: none !important;
}

.jp-ActiveCellTool .jp-InputArea-prompt {
  flex: 0 0 auto;
  padding-left: 0px;
}

.jp-ActiveCellTool .jp-InputArea-editor {
  flex: 1 1 auto;
  background: var(--jp-cell-editor-background);
  border-color: var(--jp-cell-editor-border-color);
}

.jp-ActiveCellTool .jp-InputArea-editor .CodeMirror {
  background: transparent;
}

.jp-MetadataEditorTool {
  flex-direction: column;
  padding: 12px 0px 12px 0px;
}

.jp-RankedPanel > :not(:first-child) {
  margin-top: 12px;
}

.jp-KeySelector select.jp-mod-styled {
  font-size: var(--jp-ui-font-size1);
  color: var(--jp-ui-font-color0);
  border: var(--jp-border-width) solid var(--jp-border-color1);
}

.jp-KeySelector label,
.jp-MetadataEditorTool label {
  line-height: 1.4;
}

.jp-NotebookTools .jp-select-wrapper {
  margin-top: 4px;
  margin-bottom: 0px;
}

.jp-NotebookTools .jp-Collapse {
  margin-top: 16px;
}

/*-----------------------------------------------------------------------------
| Presentation Mode (.jp-mod-presentationMode)
|----------------------------------------------------------------------------*/

.jp-mod-presentationMode .jp-Notebook {
  --jp-content-font-size1: var(--jp-content-presentation-font-size1);
  --jp-code-font-size: var(--jp-code-presentation-font-size);
}

.jp-mod-presentationMode .jp-Notebook .jp-Cell .jp-InputPrompt,
.jp-mod-presentationMode .jp-Notebook .jp-Cell .jp-OutputPrompt {
  flex: 0 0 110px;
}

/*-----------------------------------------------------------------------------
| Placeholder
|----------------------------------------------------------------------------*/

.jp-Cell-Placeholder {
  padding-left: 55px;
}

.jp-Cell-Placeholder-wrapper {
  background: #fff;
  border: 1px solid;
  border-color: #e5e6e9 #dfe0e4 #d0d1d5;
  border-radius: 4px;
  -webkit-border-radius: 4px;
  margin: 10px 15px;
}

.jp-Cell-Placeholder-wrapper-inner {
  padding: 15px;
  position: relative;
}

.jp-Cell-Placeholder-wrapper-body {
  background-repeat: repeat;
  background-size: 50% auto;
}

.jp-Cell-Placeholder-wrapper-body div {
  background: #f6f7f8;
  background-image: -webkit-linear-gradient(
    left,
    #f6f7f8 0%,
    #edeef1 20%,
    #f6f7f8 40%,
    #f6f7f8 100%
  );
  background-repeat: no-repeat;
  background-size: 800px 104px;
  height: 104px;
  position: relative;
}

.jp-Cell-Placeholder-wrapper-body div {
  position: absolute;
  right: 15px;
  left: 15px;
  top: 15px;
}

div.jp-Cell-Placeholder-h1 {
  top: 20px;
  height: 20px;
  left: 15px;
  width: 150px;
}

div.jp-Cell-Placeholder-h2 {
  left: 15px;
  top: 50px;
  height: 10px;
  width: 100px;
}

div.jp-Cell-Placeholder-content-1,
div.jp-Cell-Placeholder-content-2,
div.jp-Cell-Placeholder-content-3 {
  left: 15px;
  right: 15px;
  height: 10px;
}

div.jp-Cell-Placeholder-content-1 {
  top: 100px;
}

div.jp-Cell-Placeholder-content-2 {
  top: 120px;
}

div.jp-Cell-Placeholder-content-3 {
  top: 140px;
}

</style>

    <style type="text/css">
/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

/*
The following CSS variables define the main, public API for styling JupyterLab.
These variables should be used by all plugins wherever possible. In other
words, plugins should not define custom colors, sizes, etc unless absolutely
necessary. This enables users to change the visual theme of JupyterLab
by changing these variables.

Many variables appear in an ordered sequence (0,1,2,3). These sequences
are designed to work well together, so for example, `--jp-border-color1` should
be used with `--jp-layout-color1`. The numbers have the following meanings:

* 0: super-primary, reserved for special emphasis
* 1: primary, most important under normal situations
* 2: secondary, next most important under normal situations
* 3: tertiary, next most important under normal situations

Throughout JupyterLab, we are mostly following principles from Google's
Material Design when selecting colors. We are not, however, following
all of MD as it is not optimized for dense, information rich UIs.
*/

:root {
  /* Elevation
   *
   * We style box-shadows using Material Design's idea of elevation. These particular numbers are taken from here:
   *
   * https://github.com/material-components/material-components-web
   * https://material-components-web.appspot.com/elevation.html
   */

  --jp-shadow-base-lightness: 0;
  --jp-shadow-umbra-color: rgba(
    var(--jp-shadow-base-lightness),
    var(--jp-shadow-base-lightness),
    var(--jp-shadow-base-lightness),
    0.2
  );
  --jp-shadow-penumbra-color: rgba(
    var(--jp-shadow-base-lightness),
    var(--jp-shadow-base-lightness),
    var(--jp-shadow-base-lightness),
    0.14
  );
  --jp-shadow-ambient-color: rgba(
    var(--jp-shadow-base-lightness),
    var(--jp-shadow-base-lightness),
    var(--jp-shadow-base-lightness),
    0.12
  );
  --jp-elevation-z0: none;
  --jp-elevation-z1: 0px 2px 1px -1px var(--jp-shadow-umbra-color),
    0px 1px 1px 0px var(--jp-shadow-penumbra-color),
    0px 1px 3px 0px var(--jp-shadow-ambient-color);
  --jp-elevation-z2: 0px 3px 1px -2px var(--jp-shadow-umbra-color),
    0px 2px 2px 0px var(--jp-shadow-penumbra-color),
    0px 1px 5px 0px var(--jp-shadow-ambient-color);
  --jp-elevation-z4: 0px 2px 4px -1px var(--jp-shadow-umbra-color),
    0px 4px 5px 0px var(--jp-shadow-penumbra-color),
    0px 1px 10px 0px var(--jp-shadow-ambient-color);
  --jp-elevation-z6: 0px 3px 5px -1px var(--jp-shadow-umbra-color),
    0px 6px 10px 0px var(--jp-shadow-penumbra-color),
    0px 1px 18px 0px var(--jp-shadow-ambient-color);
  --jp-elevation-z8: 0px 5px 5px -3px var(--jp-shadow-umbra-color),
    0px 8px 10px 1px var(--jp-shadow-penumbra-color),
    0px 3px 14px 2px var(--jp-shadow-ambient-color);
  --jp-elevation-z12: 0px 7px 8px -4px var(--jp-shadow-umbra-color),
    0px 12px 17px 2px var(--jp-shadow-penumbra-color),
    0px 5px 22px 4px var(--jp-shadow-ambient-color);
  --jp-elevation-z16: 0px 8px 10px -5px var(--jp-shadow-umbra-color),
    0px 16px 24px 2px var(--jp-shadow-penumbra-color),
    0px 6px 30px 5px var(--jp-shadow-ambient-color);
  --jp-elevation-z20: 0px 10px 13px -6px var(--jp-shadow-umbra-color),
    0px 20px 31px 3px var(--jp-shadow-penumbra-color),
    0px 8px 38px 7px var(--jp-shadow-ambient-color);
  --jp-elevation-z24: 0px 11px 15px -7px var(--jp-shadow-umbra-color),
    0px 24px 38px 3px var(--jp-shadow-penumbra-color),
    0px 9px 46px 8px var(--jp-shadow-ambient-color);

  /* Borders
   *
   * The following variables, specify the visual styling of borders in JupyterLab.
   */

  --jp-border-width: 1px;
  --jp-border-color0: var(--md-grey-400);
  --jp-border-color1: var(--md-grey-400);
  --jp-border-color2: var(--md-grey-300);
  --jp-border-color3: var(--md-grey-200);
  --jp-border-radius: 2px;

  /* UI Fonts
   *
   * The UI font CSS variables are used for the typography all of the JupyterLab
   * user interface elements that are not directly user generated content.
   *
   * The font sizing here is done assuming that the body font size of --jp-ui-font-size1
   * is applied to a parent element. When children elements, such as headings, are sized
   * in em all things will be computed relative to that body size.
   */

  --jp-ui-font-scale-factor: 1.2;
  --jp-ui-font-size0: 0.83333em;
  --jp-ui-font-size1: 13px; /* Base font size */
  --jp-ui-font-size2: 1.2em;
  --jp-ui-font-size3: 1.44em;

  --jp-ui-font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Helvetica,
    Arial, sans-serif, 'Apple Color Emoji', 'Segoe UI Emoji', 'Segoe UI Symbol';

  /*
   * Use these font colors against the corresponding main layout colors.
   * In a light theme, these go from dark to light.
   */

  /* Defaults use Material Design specification */
  --jp-ui-font-color0: rgba(0, 0, 0, 1);
  --jp-ui-font-color1: rgba(0, 0, 0, 0.87);
  --jp-ui-font-color2: rgba(0, 0, 0, 0.54);
  --jp-ui-font-color3: rgba(0, 0, 0, 0.38);

  /*
   * Use these against the brand/accent/warn/error colors.
   * These will typically go from light to darker, in both a dark and light theme.
   */

  --jp-ui-inverse-font-color0: rgba(255, 255, 255, 1);
  --jp-ui-inverse-font-color1: rgba(255, 255, 255, 1);
  --jp-ui-inverse-font-color2: rgba(255, 255, 255, 0.7);
  --jp-ui-inverse-font-color3: rgba(255, 255, 255, 0.5);

  /* Content Fonts
   *
   * Content font variables are used for typography of user generated content.
   *
   * The font sizing here is done assuming that the body font size of --jp-content-font-size1
   * is applied to a parent element. When children elements, such as headings, are sized
   * in em all things will be computed relative to that body size.
   */

  --jp-content-line-height: 1.6;
  --jp-content-font-scale-factor: 1.2;
  --jp-content-font-size0: 0.83333em;
  --jp-content-font-size1: 14px; /* Base font size */
  --jp-content-font-size2: 1.2em;
  --jp-content-font-size3: 1.44em;
  --jp-content-font-size4: 1.728em;
  --jp-content-font-size5: 2.0736em;

  /* This gives a magnification of about 125% in presentation mode over normal. */
  --jp-content-presentation-font-size1: 17px;

  --jp-content-heading-line-height: 1;
  --jp-content-heading-margin-top: 1.2em;
  --jp-content-heading-margin-bottom: 0.8em;
  --jp-content-heading-font-weight: 500;

  /* Defaults use Material Design specification */
  --jp-content-font-color0: rgba(0, 0, 0, 1);
  --jp-content-font-color1: rgba(0, 0, 0, 0.87);
  --jp-content-font-color2: rgba(0, 0, 0, 0.54);
  --jp-content-font-color3: rgba(0, 0, 0, 0.38);

  --jp-content-link-color: var(--md-blue-700);

  --jp-content-font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI',
    Helvetica, Arial, sans-serif, 'Apple Color Emoji', 'Segoe UI Emoji',
    'Segoe UI Symbol';

  /*
   * Code Fonts
   *
   * Code font variables are used for typography of code and other monospaces content.
   */

  --jp-code-font-size: 13px;
  --jp-code-line-height: 1.3077; /* 17px for 13px base */
  --jp-code-padding: 5px; /* 5px for 13px base, codemirror highlighting needs integer px value */
  --jp-code-font-family-default: Menlo, Consolas, 'DejaVu Sans Mono', monospace;
  --jp-code-font-family: var(--jp-code-font-family-default);

  /* This gives a magnification of about 125% in presentation mode over normal. */
  --jp-code-presentation-font-size: 16px;

  /* may need to tweak cursor width if you change font size */
  --jp-code-cursor-width0: 1.4px;
  --jp-code-cursor-width1: 2px;
  --jp-code-cursor-width2: 4px;

  /* Layout
   *
   * The following are the main layout colors use in JupyterLab. In a light
   * theme these would go from light to dark.
   */

  --jp-layout-color0: white;
  --jp-layout-color1: white;
  --jp-layout-color2: var(--md-grey-200);
  --jp-layout-color3: var(--md-grey-400);
  --jp-layout-color4: var(--md-grey-600);

  /* Inverse Layout
   *
   * The following are the inverse layout colors use in JupyterLab. In a light
   * theme these would go from dark to light.
   */

  --jp-inverse-layout-color0: #111111;
  --jp-inverse-layout-color1: var(--md-grey-900);
  --jp-inverse-layout-color2: var(--md-grey-800);
  --jp-inverse-layout-color3: var(--md-grey-700);
  --jp-inverse-layout-color4: var(--md-grey-600);

  /* Brand/accent */

  --jp-brand-color0: var(--md-blue-900);
  --jp-brand-color1: var(--md-blue-700);
  --jp-brand-color2: var(--md-blue-300);
  --jp-brand-color3: var(--md-blue-100);
  --jp-brand-color4: var(--md-blue-50);

  --jp-accent-color0: var(--md-green-900);
  --jp-accent-color1: var(--md-green-700);
  --jp-accent-color2: var(--md-green-300);
  --jp-accent-color3: var(--md-green-100);

  /* State colors (warn, error, success, info) */

  --jp-warn-color0: var(--md-orange-900);
  --jp-warn-color1: var(--md-orange-700);
  --jp-warn-color2: var(--md-orange-300);
  --jp-warn-color3: var(--md-orange-100);

  --jp-error-color0: var(--md-red-900);
  --jp-error-color1: var(--md-red-700);
  --jp-error-color2: var(--md-red-300);
  --jp-error-color3: var(--md-red-100);

  --jp-success-color0: var(--md-green-900);
  --jp-success-color1: var(--md-green-700);
  --jp-success-color2: var(--md-green-300);
  --jp-success-color3: var(--md-green-100);

  --jp-info-color0: var(--md-cyan-900);
  --jp-info-color1: var(--md-cyan-700);
  --jp-info-color2: var(--md-cyan-300);
  --jp-info-color3: var(--md-cyan-100);

  /* Cell specific styles */

  --jp-cell-padding: 5px;

  --jp-cell-collapser-width: 8px;
  --jp-cell-collapser-min-height: 20px;
  --jp-cell-collapser-not-active-hover-opacity: 0.6;

  --jp-cell-editor-background: var(--md-grey-100);
  --jp-cell-editor-border-color: var(--md-grey-300);
  --jp-cell-editor-box-shadow: inset 0 0 2px var(--md-blue-300);
  --jp-cell-editor-active-background: var(--jp-layout-color0);
  --jp-cell-editor-active-border-color: var(--jp-brand-color1);

  --jp-cell-prompt-width: 64px;
  --jp-cell-prompt-font-family: var(--jp-code-font-family-default);
  --jp-cell-prompt-letter-spacing: 0px;
  --jp-cell-prompt-opacity: 1;
  --jp-cell-prompt-not-active-opacity: 0.5;
  --jp-cell-prompt-not-active-font-color: var(--md-grey-700);
  /* A custom blend of MD grey and blue 600
   * See https://meyerweb.com/eric/tools/color-blend/#546E7A:1E88E5:5:hex */
  --jp-cell-inprompt-font-color: #307fc1;
  /* A custom blend of MD grey and orange 600
   * https://meyerweb.com/eric/tools/color-blend/#546E7A:F4511E:5:hex */
  --jp-cell-outprompt-font-color: #bf5b3d;

  /* Notebook specific styles */

  --jp-notebook-padding: 10px;
  --jp-notebook-select-background: var(--jp-layout-color1);
  --jp-notebook-multiselected-color: var(--md-blue-50);

  /* The scroll padding is calculated to fill enough space at the bottom of the
  notebook to show one single-line cell (with appropriate padding) at the top
  when the notebook is scrolled all the way to the bottom. We also subtract one
  pixel so that no scrollbar appears if we have just one single-line cell in the
  notebook. This padding is to enable a 'scroll past end' feature in a notebook.
  */
  --jp-notebook-scroll-padding: calc(
    100% - var(--jp-code-font-size) * var(--jp-code-line-height) -
      var(--jp-code-padding) - var(--jp-cell-padding) - 1px
  );

  /* Rendermime styles */

  --jp-rendermime-error-background: #fdd;
  --jp-rendermime-table-row-background: var(--md-grey-100);
  --jp-rendermime-table-row-hover-background: var(--md-light-blue-50);

  /* Dialog specific styles */

  --jp-dialog-background: rgba(0, 0, 0, 0.25);

  /* Console specific styles */

  --jp-console-padding: 10px;

  /* Toolbar specific styles */

  --jp-toolbar-border-color: var(--jp-border-color1);
  --jp-toolbar-micro-height: 8px;
  --jp-toolbar-background: var(--jp-layout-color1);
  --jp-toolbar-box-shadow: 0px 0px 2px 0px rgba(0, 0, 0, 0.24);
  --jp-toolbar-header-margin: 4px 4px 0px 4px;
  --jp-toolbar-active-background: var(--md-grey-300);

  /* Statusbar specific styles */

  --jp-statusbar-height: 24px;

  /* Input field styles */

  --jp-input-box-shadow: inset 0 0 2px var(--md-blue-300);
  --jp-input-active-background: var(--jp-layout-color1);
  --jp-input-hover-background: var(--jp-layout-color1);
  --jp-input-background: var(--md-grey-100);
  --jp-input-border-color: var(--jp-border-color1);
  --jp-input-active-border-color: var(--jp-brand-color1);
  --jp-input-active-box-shadow-color: rgba(19, 124, 189, 0.3);

  /* General editor styles */

  --jp-editor-selected-background: #d9d9d9;
  --jp-editor-selected-focused-background: #d7d4f0;
  --jp-editor-cursor-color: var(--jp-ui-font-color0);

  /* Code mirror specific styles */

  --jp-mirror-editor-keyword-color: #008000;
  --jp-mirror-editor-atom-color: #88f;
  --jp-mirror-editor-number-color: #080;
  --jp-mirror-editor-def-color: #00f;
  --jp-mirror-editor-variable-color: var(--md-grey-900);
  --jp-mirror-editor-variable-2-color: #05a;
  --jp-mirror-editor-variable-3-color: #085;
  --jp-mirror-editor-punctuation-color: #05a;
  --jp-mirror-editor-property-color: #05a;
  --jp-mirror-editor-operator-color: #aa22ff;
  --jp-mirror-editor-comment-color: #408080;
  --jp-mirror-editor-string-color: #ba2121;
  --jp-mirror-editor-string-2-color: #708;
  --jp-mirror-editor-meta-color: #aa22ff;
  --jp-mirror-editor-qualifier-color: #555;
  --jp-mirror-editor-builtin-color: #008000;
  --jp-mirror-editor-bracket-color: #997;
  --jp-mirror-editor-tag-color: #170;
  --jp-mirror-editor-attribute-color: #00c;
  --jp-mirror-editor-header-color: blue;
  --jp-mirror-editor-quote-color: #090;
  --jp-mirror-editor-link-color: #00c;
  --jp-mirror-editor-error-color: #f00;
  --jp-mirror-editor-hr-color: #999;

  /* Vega extension styles */

  --jp-vega-background: white;

  /* Sidebar-related styles */

  --jp-sidebar-min-width: 250px;

  /* Search-related styles */

  --jp-search-toggle-off-opacity: 0.5;
  --jp-search-toggle-hover-opacity: 0.8;
  --jp-search-toggle-on-opacity: 1;
  --jp-search-selected-match-background-color: rgb(245, 200, 0);
  --jp-search-selected-match-color: black;
  --jp-search-unselected-match-background-color: var(
    --jp-inverse-layout-color0
  );
  --jp-search-unselected-match-color: var(--jp-ui-inverse-font-color0);

  /* Icon colors that work well with light or dark backgrounds */
  --jp-icon-contrast-color0: var(--md-purple-600);
  --jp-icon-contrast-color1: var(--md-green-600);
  --jp-icon-contrast-color2: var(--md-pink-600);
  --jp-icon-contrast-color3: var(--md-blue-600);
}
</style>

<style type="text/css">
/* Force rendering true colors when outputing to pdf */
* {
  -webkit-print-color-adjust: exact;
}

/* Misc */
a.anchor-link {
  display: none;
}

.highlight  {
  margin: 0.4em;
}

/* Input area styling */
.jp-InputArea {
  overflow: hidden;
}

.jp-InputArea-editor {
  overflow: hidden;
}

.CodeMirror pre {
  margin: 0;
  padding: 0;
}

/* Using table instead of flexbox so that we can use break-inside property */
/* CSS rules under this comment should not be required anymore after we move to the JupyterLab 4.0 CSS */


.jp-CodeCell.jp-mod-outputsScrolled .jp-OutputArea-prompt {
  min-width: calc(
    var(--jp-cell-prompt-width) - var(--jp-private-cell-scrolling-output-offset)
  );
}

.jp-OutputArea-child {
  display: table;
  width: 100%;
}

.jp-OutputPrompt {
  display: table-cell;
  vertical-align: top;
  min-width: var(--jp-cell-prompt-width);
}

body[data-format='mobile'] .jp-OutputPrompt {
  display: table-row;
}

.jp-OutputArea-output {
  display: table-cell;
  width: 100%;
}

body[data-format='mobile'] .jp-OutputArea-child .jp-OutputArea-output {
  display: table-row;
}

.jp-OutputArea-output.jp-OutputArea-executeResult {
  width: 100%;
}

/* Hiding the collapser by default */
.jp-Collapser {
  display: none;
}

@media print {
  .jp-Cell-inputWrapper,
  .jp-Cell-outputWrapper {
    display: block;
  }

  .jp-OutputArea-child {
    break-inside: avoid-page;
  }
}
</style>

<!-- Load mathjax -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.7/latest.js?config=TeX-AMS_CHTML-full,Safe"> </script>
    <!-- MathJax configuration -->
    <script type="text/x-mathjax-config">
    init_mathjax = function() {
        if (window.MathJax) {
        // MathJax loaded
            MathJax.Hub.Config({
                TeX: {
                    equationNumbers: {
                    autoNumber: "AMS",
                    useLabelIds: true
                    }
                },
                tex2jax: {
                    inlineMath: [ ['$','$'], ["\\(","\\)"] ],
                    displayMath: [ ['$$','$$'], ["\\[","\\]"] ],
                    processEscapes: true,
                    processEnvironments: true
                },
                displayAlign: 'center',
                CommonHTML: {
                    linebreaks: {
                    automatic: true
                    }
                }
            });

            MathJax.Hub.Queue(["Typeset", MathJax.Hub]);
        }
    }
    init_mathjax();
    </script>
    <!-- End of mathjax configuration --></head>
<body class="jp-Notebook" data-jp-theme-light="true" data-jp-theme-name="JupyterLab Light">
<div class="jp-Cell jp-CodeCell jp-Notebook-cell jp-mod-noOutputs  ">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea">
<div class="jp-InputPrompt jp-InputArea-prompt">In&nbsp;[&nbsp;]:</div>
<div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline">
     <div class="CodeMirror cm-s-jupyter">
<div class=" highlight hl-python"><pre><span></span><span class="c1"># importing the libraries</span>
<span class="kn">import</span> <span class="nn">numpy</span> <span class="k">as</span> <span class="nn">np</span>
<span class="kn">import</span> <span class="nn">pandas</span> <span class="k">as</span> <span class="nn">pd</span>
<span class="c1"># Libraries to help with data visualization</span>
<span class="kn">import</span> <span class="nn">matplotlib.pyplot</span> <span class="k">as</span> <span class="nn">plt</span>
<span class="kn">import</span> <span class="nn">seaborn</span> <span class="k">as</span> <span class="nn">sns</span>

<span class="c1"># Command to tell Python to actually display the graphs</span>
<span class="o">%</span><span class="n">matplotlib</span> <span class="n">inline</span>
</pre></div>

     </div>
</div>
</div>
</div>

</div><div class="jp-Cell jp-CodeCell jp-Notebook-cell   ">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea">
<div class="jp-InputPrompt jp-InputArea-prompt">In&nbsp;[&nbsp;]:</div>
<div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline">
     <div class="CodeMirror cm-s-jupyter">
<div class=" highlight hl-python"><pre><span></span><span class="kn">from</span> <span class="nn">google.colab</span> <span class="kn">import</span> <span class="n">drive</span>
<span class="n">drive</span><span class="o">.</span><span class="n">mount</span><span class="p">(</span><span class="s1">'/content/drive'</span><span class="p">)</span>
</pre></div>

     </div>
</div>
</div>
</div>

<div class="jp-Cell-outputWrapper">
<div class="jp-Collapser jp-OutputCollapser jp-Cell-outputCollapser">
</div>


<div class="jp-OutputArea jp-Cell-outputArea">

<div class="jp-OutputArea-child">

    
    <div class="jp-OutputPrompt jp-OutputArea-prompt"></div>


<div class="jp-RenderedText jp-OutputArea-output" data-mime-type="text/plain">
<pre>Mounted at /content/drive
</pre>
</div>
</div>

</div>

</div>

</div><div class="jp-Cell jp-CodeCell jp-Notebook-cell jp-mod-noOutputs  ">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea">
<div class="jp-InputPrompt jp-InputArea-prompt">In&nbsp;[&nbsp;]:</div>
<div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline">
     <div class="CodeMirror cm-s-jupyter">
<div class=" highlight hl-python"><pre><span></span><span class="n">path</span><span class="o">=</span><span class="s2">"/content/drive/MyDrive/airfares.csv"</span>
<span class="n">data</span><span class="o">=</span><span class="n">pd</span><span class="o">.</span><span class="n">read_csv</span><span class="p">(</span><span class="n">path</span><span class="p">)</span>
</pre></div>

     </div>
</div>
</div>
</div>

</div><div class="jp-Cell jp-CodeCell jp-Notebook-cell   ">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea">
<div class="jp-InputPrompt jp-InputArea-prompt">In&nbsp;[&nbsp;]:</div>
<div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline">
     <div class="CodeMirror cm-s-jupyter">
<div class=" highlight hl-python"><pre><span></span><span class="n">data</span><span class="o">.</span><span class="n">info</span><span class="p">()</span>
</pre></div>

     </div>
</div>
</div>
</div>

<div class="jp-Cell-outputWrapper">
<div class="jp-Collapser jp-OutputCollapser jp-Cell-outputCollapser">
</div>


<div class="jp-OutputArea jp-Cell-outputArea">

<div class="jp-OutputArea-child">

    
    <div class="jp-OutputPrompt jp-OutputArea-prompt"></div>


<div class="jp-RenderedText jp-OutputArea-output" data-mime-type="text/plain">
<pre>&lt;class &#39;pandas.core.frame.DataFrame&#39;&gt;
RangeIndex: 1135211 entries, 0 to 1135210
Data columns (total 15 columns):
 #   Column            Non-Null Count    Dtype  
---  ------            --------------    -----  
 0   observation_date  1135211 non-null  object 
 1   observation_time  1135211 non-null  object 
 2   origin            1135211 non-null  object 
 3   destination       1135211 non-null  object 
 4   market            1135211 non-null  object 
 5   carrier           1135211 non-null  object 
 6   flight_number     1135211 non-null  int64  
 7   departure_date    1135211 non-null  object 
 8   departure_time    1135211 non-null  object 
 9   fare_basis        1135211 non-null  object 
 10  booking_class     1135211 non-null  object 
 11  price_exc_tax     1129558 non-null  float64
 12  tax               1129558 non-null  float64
 13  price             1135211 non-null  float64
 14  currency          1135211 non-null  object 
dtypes: float64(3), int64(1), object(11)
memory usage: 129.9+ MB
</pre>
</div>
</div>

</div>

</div>

</div><div class="jp-Cell jp-CodeCell jp-Notebook-cell jp-mod-noOutputs  ">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea">
<div class="jp-InputPrompt jp-InputArea-prompt">In&nbsp;[&nbsp;]:</div>
<div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline">
     <div class="CodeMirror cm-s-jupyter">
<div class=" highlight hl-python"><pre><span></span><span class="n">data</span><span class="p">[</span><span class="s1">'departure_date'</span><span class="p">]</span> <span class="o">=</span> <span class="n">pd</span><span class="o">.</span><span class="n">to_datetime</span><span class="p">(</span><span class="n">data</span><span class="p">[</span><span class="s1">'departure_date'</span><span class="p">])</span>
<span class="n">data</span><span class="p">[</span><span class="s1">'observation_date'</span><span class="p">]</span>  <span class="o">=</span> <span class="n">pd</span><span class="o">.</span><span class="n">to_datetime</span><span class="p">(</span><span class="n">data</span><span class="p">[</span><span class="s1">'observation_date'</span><span class="p">])</span>
</pre></div>

     </div>
</div>
</div>
</div>

</div><div class="jp-Cell jp-CodeCell jp-Notebook-cell   ">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea">
<div class="jp-InputPrompt jp-InputArea-prompt">In&nbsp;[&nbsp;]:</div>
<div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline">
     <div class="CodeMirror cm-s-jupyter">
<div class=" highlight hl-python"><pre><span></span><span class="n">data</span><span class="o">.</span><span class="n">head</span><span class="p">()</span>
</pre></div>

     </div>
</div>
</div>
</div>

<div class="jp-Cell-outputWrapper">
<div class="jp-Collapser jp-OutputCollapser jp-Cell-outputCollapser">
</div>


<div class="jp-OutputArea jp-Cell-outputArea">

<div class="jp-OutputArea-child">

    
    <div class="jp-OutputPrompt jp-OutputArea-prompt">Out[&nbsp;]:</div>



<div class="jp-RenderedHTMLCommon jp-RenderedHTML jp-OutputArea-output jp-OutputArea-executeResult" data-mime-type="text/html">

  <div id="df-5976e6bf-46d2-45bd-923b-5b912d66cac9" class="colab-df-container">
    <div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>observation_date</th>
      <th>observation_time</th>
      <th>origin</th>
      <th>destination</th>
      <th>market</th>
      <th>carrier</th>
      <th>flight_number</th>
      <th>departure_date</th>
      <th>departure_time</th>
      <th>fare_basis</th>
      <th>booking_class</th>
      <th>price_exc_tax</th>
      <th>tax</th>
      <th>price</th>
      <th>currency</th>
      <th>new_departure_date</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2019-01-12</td>
      <td>02:33:00</td>
      <td>BOS</td>
      <td>LGA</td>
      <td>BOSLGA</td>
      <td>BB</td>
      <td>2118</td>
      <td>2019-05-02</td>
      <td>14:00:00</td>
      <td>B63AA</td>
      <td>B</td>
      <td>63.26</td>
      <td>19.04</td>
      <td>82.3</td>
      <td>USD</td>
      <td>2019-05-02</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2019-01-12</td>
      <td>02:36:00</td>
      <td>BOS</td>
      <td>LGA</td>
      <td>BOSLGA</td>
      <td>BB</td>
      <td>2118</td>
      <td>2019-05-12</td>
      <td>14:00:00</td>
      <td>B63AA</td>
      <td>B</td>
      <td>63.26</td>
      <td>19.04</td>
      <td>82.3</td>
      <td>USD</td>
      <td>2019-05-12</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2019-01-12</td>
      <td>02:33:00</td>
      <td>BOS</td>
      <td>LGA</td>
      <td>BOSLGA</td>
      <td>BB</td>
      <td>2118</td>
      <td>2019-05-01</td>
      <td>14:00:00</td>
      <td>B63AA</td>
      <td>B</td>
      <td>63.26</td>
      <td>19.04</td>
      <td>82.3</td>
      <td>USD</td>
      <td>2019-05-01</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2019-01-12</td>
      <td>02:34:00</td>
      <td>BOS</td>
      <td>LGA</td>
      <td>BOSLGA</td>
      <td>BB</td>
      <td>2118</td>
      <td>2019-05-06</td>
      <td>14:00:00</td>
      <td>B63AA</td>
      <td>B</td>
      <td>63.26</td>
      <td>19.04</td>
      <td>82.3</td>
      <td>USD</td>
      <td>2019-05-06</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2019-01-12</td>
      <td>02:35:00</td>
      <td>BOS</td>
      <td>LGA</td>
      <td>BOSLGA</td>
      <td>BB</td>
      <td>2118</td>
      <td>2019-05-07</td>
      <td>14:00:00</td>
      <td>B63AA</td>
      <td>B</td>
      <td>63.26</td>
      <td>19.04</td>
      <td>82.3</td>
      <td>USD</td>
      <td>2019-05-07</td>
    </tr>
  </tbody>
</table>
</div>
    <div class="colab-df-buttons">

  <div class="colab-df-container">
    <button class="colab-df-convert" onclick="convertToInteractive('df-5976e6bf-46d2-45bd-923b-5b912d66cac9')"
            title="Convert this dataframe to an interactive table."
            style="display:none;">

  <svg xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 -960 960 960">
    <path d="M120-120v-720h720v720H120Zm60-500h600v-160H180v160Zm220 220h160v-160H400v160Zm0 220h160v-160H400v160ZM180-400h160v-160H180v160Zm440 0h160v-160H620v160ZM180-180h160v-160H180v160Zm440 0h160v-160H620v160Z"/>
  </svg>
    </button>

  <style>
    .colab-df-container {
      display:flex;
      gap: 12px;
    }

    .colab-df-convert {
      background-color: #E8F0FE;
      border: none;
      border-radius: 50%;
      cursor: pointer;
      display: none;
      fill: #1967D2;
      height: 32px;
      padding: 0 0 0 0;
      width: 32px;
    }

    .colab-df-convert:hover {
      background-color: #E2EBFA;
      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);
      fill: #174EA6;
    }

    .colab-df-buttons div {
      margin-bottom: 4px;
    }

    [theme=dark] .colab-df-convert {
      background-color: #3B4455;
      fill: #D2E3FC;
    }

    [theme=dark] .colab-df-convert:hover {
      background-color: #434B5C;
      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));
      fill: #FFFFFF;
    }
  </style>

    <script>
      const buttonEl =
        document.querySelector('#df-5976e6bf-46d2-45bd-923b-5b912d66cac9 button.colab-df-convert');
      buttonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';

      async function convertToInteractive(key) {
        const element = document.querySelector('#df-5976e6bf-46d2-45bd-923b-5b912d66cac9');
        const dataTable =
          await google.colab.kernel.invokeFunction('convertToInteractive',
                                                    [key], {});
        if (!dataTable) return;

        const docLinkHtml = 'Like what you see? Visit the ' +
          '<a target="_blank" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'
          + ' to learn more about interactive tables.';
        element.innerHTML = '';
        dataTable['output_type'] = 'display_data';
        await google.colab.output.renderOutput(dataTable, element);
        const docLink = document.createElement('div');
        docLink.innerHTML = docLinkHtml;
        element.appendChild(docLink);
      }
    </script>
  </div>


<div id="df-1d6c5936-6a3e-43f3-8559-666a3669ccfa">
  <button class="colab-df-quickchart" onclick="quickchart('df-1d6c5936-6a3e-43f3-8559-666a3669ccfa')"
            title="Suggest charts"
            style="display:none;">

<svg xmlns="http://www.w3.org/2000/svg" height="24px"viewBox="0 0 24 24"
     width="24px">
    <g>
        <path d="M19 3H5c-1.1 0-2 .9-2 2v14c0 1.1.9 2 2 2h14c1.1 0 2-.9 2-2V5c0-1.1-.9-2-2-2zM9 17H7v-7h2v7zm4 0h-2V7h2v10zm4 0h-2v-4h2v4z"/>
    </g>
</svg>
  </button>

<style>
  .colab-df-quickchart {
      --bg-color: #E8F0FE;
      --fill-color: #1967D2;
      --hover-bg-color: #E2EBFA;
      --hover-fill-color: #174EA6;
      --disabled-fill-color: #AAA;
      --disabled-bg-color: #DDD;
  }

  [theme=dark] .colab-df-quickchart {
      --bg-color: #3B4455;
      --fill-color: #D2E3FC;
      --hover-bg-color: #434B5C;
      --hover-fill-color: #FFFFFF;
      --disabled-bg-color: #3B4455;
      --disabled-fill-color: #666;
  }

  .colab-df-quickchart {
    background-color: var(--bg-color);
    border: none;
    border-radius: 50%;
    cursor: pointer;
    display: none;
    fill: var(--fill-color);
    height: 32px;
    padding: 0;
    width: 32px;
  }

  .colab-df-quickchart:hover {
    background-color: var(--hover-bg-color);
    box-shadow: 0 1px 2px rgba(60, 64, 67, 0.3), 0 1px 3px 1px rgba(60, 64, 67, 0.15);
    fill: var(--button-hover-fill-color);
  }

  .colab-df-quickchart-complete:disabled,
  .colab-df-quickchart-complete:disabled:hover {
    background-color: var(--disabled-bg-color);
    fill: var(--disabled-fill-color);
    box-shadow: none;
  }

  .colab-df-spinner {
    border: 2px solid var(--fill-color);
    border-color: transparent;
    border-bottom-color: var(--fill-color);
    animation:
      spin 1s steps(1) infinite;
  }

  @keyframes spin {
    0% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
      border-left-color: var(--fill-color);
    }
    20% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    30% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
      border-right-color: var(--fill-color);
    }
    40% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    60% {
      border-color: transparent;
      border-right-color: var(--fill-color);
    }
    80% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-bottom-color: var(--fill-color);
    }
    90% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
    }
  }
</style>

  <script>
    async function quickchart(key) {
      const quickchartButtonEl =
        document.querySelector('#' + key + ' button');
      quickchartButtonEl.disabled = true;  // To prevent multiple clicks.
      quickchartButtonEl.classList.add('colab-df-spinner');
      try {
        const charts = await google.colab.kernel.invokeFunction(
            'suggestCharts', [key], {});
      } catch (error) {
        console.error('Error during call to suggestCharts:', error);
      }
      quickchartButtonEl.classList.remove('colab-df-spinner');
      quickchartButtonEl.classList.add('colab-df-quickchart-complete');
    }
    (() => {
      let quickchartButtonEl =
        document.querySelector('#df-1d6c5936-6a3e-43f3-8559-666a3669ccfa button');
      quickchartButtonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';
    })();
  </script>
</div>

    </div>
  </div>

</div>

</div>

</div>

</div>

</div><div class="jp-Cell jp-CodeCell jp-Notebook-cell   ">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea">
<div class="jp-InputPrompt jp-InputArea-prompt">In&nbsp;[&nbsp;]:</div>
<div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline">
     <div class="CodeMirror cm-s-jupyter">
<div class=" highlight hl-python"><pre><span></span><span class="n">data</span><span class="o">.</span><span class="n">groupby</span><span class="p">(</span><span class="s1">'market'</span><span class="p">)[</span><span class="s1">'price'</span><span class="p">]</span><span class="o">.</span><span class="n">describe</span><span class="p">()</span>
</pre></div>

     </div>
</div>
</div>
</div>

<div class="jp-Cell-outputWrapper">
<div class="jp-Collapser jp-OutputCollapser jp-Cell-outputCollapser">
</div>


<div class="jp-OutputArea jp-Cell-outputArea">

<div class="jp-OutputArea-child">

    
    <div class="jp-OutputPrompt jp-OutputArea-prompt">Out[&nbsp;]:</div>



<div class="jp-RenderedHTMLCommon jp-RenderedHTML jp-OutputArea-output jp-OutputArea-executeResult" data-mime-type="text/html">

  <div id="df-351aa87c-4c2f-417f-9447-d1423e00da9f" class="colab-df-container">
    <div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>count</th>
      <th>mean</th>
      <th>std</th>
      <th>min</th>
      <th>25%</th>
      <th>50%</th>
      <th>75%</th>
      <th>max</th>
    </tr>
    <tr>
      <th>market</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>ABQJFK</th>
      <td>19354.0</td>
      <td>172.383544</td>
      <td>41.141688</td>
      <td>113.98</td>
      <td>145.98</td>
      <td>160.98</td>
      <td>178.98</td>
      <td>525.15</td>
    </tr>
    <tr>
      <th>AUSMCO</th>
      <td>51526.0</td>
      <td>116.349139</td>
      <td>52.176950</td>
      <td>38.30</td>
      <td>78.29</td>
      <td>103.30</td>
      <td>141.62</td>
      <td>568.30</td>
    </tr>
    <tr>
      <th>BOSLGA</th>
      <td>853613.0</td>
      <td>111.809690</td>
      <td>48.313285</td>
      <td>48.30</td>
      <td>82.30</td>
      <td>97.30</td>
      <td>112.30</td>
      <td>598.30</td>
    </tr>
    <tr>
      <th>BOSRIC</th>
      <td>189877.0</td>
      <td>121.245474</td>
      <td>51.917115</td>
      <td>65.30</td>
      <td>82.30</td>
      <td>98.30</td>
      <td>148.30</td>
      <td>590.69</td>
    </tr>
    <tr>
      <th>FLLPVD</th>
      <td>20841.0</td>
      <td>143.380255</td>
      <td>52.357114</td>
      <td>83.30</td>
      <td>113.98</td>
      <td>124.98</td>
      <td>153.05</td>
      <td>580.97</td>
    </tr>
  </tbody>
</table>
</div>
    <div class="colab-df-buttons">

  <div class="colab-df-container">
    <button class="colab-df-convert" onclick="convertToInteractive('df-351aa87c-4c2f-417f-9447-d1423e00da9f')"
            title="Convert this dataframe to an interactive table."
            style="display:none;">

  <svg xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 -960 960 960">
    <path d="M120-120v-720h720v720H120Zm60-500h600v-160H180v160Zm220 220h160v-160H400v160Zm0 220h160v-160H400v160ZM180-400h160v-160H180v160Zm440 0h160v-160H620v160ZM180-180h160v-160H180v160Zm440 0h160v-160H620v160Z"/>
  </svg>
    </button>

  <style>
    .colab-df-container {
      display:flex;
      gap: 12px;
    }

    .colab-df-convert {
      background-color: #E8F0FE;
      border: none;
      border-radius: 50%;
      cursor: pointer;
      display: none;
      fill: #1967D2;
      height: 32px;
      padding: 0 0 0 0;
      width: 32px;
    }

    .colab-df-convert:hover {
      background-color: #E2EBFA;
      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);
      fill: #174EA6;
    }

    .colab-df-buttons div {
      margin-bottom: 4px;
    }

    [theme=dark] .colab-df-convert {
      background-color: #3B4455;
      fill: #D2E3FC;
    }

    [theme=dark] .colab-df-convert:hover {
      background-color: #434B5C;
      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));
      fill: #FFFFFF;
    }
  </style>

    <script>
      const buttonEl =
        document.querySelector('#df-351aa87c-4c2f-417f-9447-d1423e00da9f button.colab-df-convert');
      buttonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';

      async function convertToInteractive(key) {
        const element = document.querySelector('#df-351aa87c-4c2f-417f-9447-d1423e00da9f');
        const dataTable =
          await google.colab.kernel.invokeFunction('convertToInteractive',
                                                    [key], {});
        if (!dataTable) return;

        const docLinkHtml = 'Like what you see? Visit the ' +
          '<a target="_blank" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'
          + ' to learn more about interactive tables.';
        element.innerHTML = '';
        dataTable['output_type'] = 'display_data';
        await google.colab.output.renderOutput(dataTable, element);
        const docLink = document.createElement('div');
        docLink.innerHTML = docLinkHtml;
        element.appendChild(docLink);
      }
    </script>
  </div>


<div id="df-3cf87704-16b8-4d2f-af2f-083531700d3f">
  <button class="colab-df-quickchart" onclick="quickchart('df-3cf87704-16b8-4d2f-af2f-083531700d3f')"
            title="Suggest charts"
            style="display:none;">

<svg xmlns="http://www.w3.org/2000/svg" height="24px"viewBox="0 0 24 24"
     width="24px">
    <g>
        <path d="M19 3H5c-1.1 0-2 .9-2 2v14c0 1.1.9 2 2 2h14c1.1 0 2-.9 2-2V5c0-1.1-.9-2-2-2zM9 17H7v-7h2v7zm4 0h-2V7h2v10zm4 0h-2v-4h2v4z"/>
    </g>
</svg>
  </button>

<style>
  .colab-df-quickchart {
      --bg-color: #E8F0FE;
      --fill-color: #1967D2;
      --hover-bg-color: #E2EBFA;
      --hover-fill-color: #174EA6;
      --disabled-fill-color: #AAA;
      --disabled-bg-color: #DDD;
  }

  [theme=dark] .colab-df-quickchart {
      --bg-color: #3B4455;
      --fill-color: #D2E3FC;
      --hover-bg-color: #434B5C;
      --hover-fill-color: #FFFFFF;
      --disabled-bg-color: #3B4455;
      --disabled-fill-color: #666;
  }

  .colab-df-quickchart {
    background-color: var(--bg-color);
    border: none;
    border-radius: 50%;
    cursor: pointer;
    display: none;
    fill: var(--fill-color);
    height: 32px;
    padding: 0;
    width: 32px;
  }

  .colab-df-quickchart:hover {
    background-color: var(--hover-bg-color);
    box-shadow: 0 1px 2px rgba(60, 64, 67, 0.3), 0 1px 3px 1px rgba(60, 64, 67, 0.15);
    fill: var(--button-hover-fill-color);
  }

  .colab-df-quickchart-complete:disabled,
  .colab-df-quickchart-complete:disabled:hover {
    background-color: var(--disabled-bg-color);
    fill: var(--disabled-fill-color);
    box-shadow: none;
  }

  .colab-df-spinner {
    border: 2px solid var(--fill-color);
    border-color: transparent;
    border-bottom-color: var(--fill-color);
    animation:
      spin 1s steps(1) infinite;
  }

  @keyframes spin {
    0% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
      border-left-color: var(--fill-color);
    }
    20% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    30% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
      border-right-color: var(--fill-color);
    }
    40% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    60% {
      border-color: transparent;
      border-right-color: var(--fill-color);
    }
    80% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-bottom-color: var(--fill-color);
    }
    90% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
    }
  }
</style>

  <script>
    async function quickchart(key) {
      const quickchartButtonEl =
        document.querySelector('#' + key + ' button');
      quickchartButtonEl.disabled = true;  // To prevent multiple clicks.
      quickchartButtonEl.classList.add('colab-df-spinner');
      try {
        const charts = await google.colab.kernel.invokeFunction(
            'suggestCharts', [key], {});
      } catch (error) {
        console.error('Error during call to suggestCharts:', error);
      }
      quickchartButtonEl.classList.remove('colab-df-spinner');
      quickchartButtonEl.classList.add('colab-df-quickchart-complete');
    }
    (() => {
      let quickchartButtonEl =
        document.querySelector('#df-3cf87704-16b8-4d2f-af2f-083531700d3f button');
      quickchartButtonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';
    })();
  </script>
</div>

    </div>
  </div>

</div>

</div>

</div>

</div>

</div><div class="jp-Cell jp-CodeCell jp-Notebook-cell   ">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea">
<div class="jp-InputPrompt jp-InputArea-prompt">In&nbsp;[&nbsp;]:</div>
<div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline">
     <div class="CodeMirror cm-s-jupyter">
<div class=" highlight hl-python"><pre><span></span><span class="n">data_4_15</span> <span class="o">=</span> <span class="n">data</span><span class="p">[</span><span class="n">data</span><span class="p">[</span><span class="s1">'observation_date'</span><span class="p">]</span> <span class="o">==</span> <span class="s1">'2019-04-15'</span><span class="p">][</span><span class="n">data</span><span class="p">[</span><span class="s1">'market'</span><span class="p">]</span> <span class="o">==</span> <span class="s1">'AUSMCO'</span><span class="p">]</span>
<span class="n">data_4_15</span><span class="o">.</span><span class="n">describe</span><span class="p">()</span>
</pre></div>

     </div>
</div>
</div>
</div>

<div class="jp-Cell-outputWrapper">
<div class="jp-Collapser jp-OutputCollapser jp-Cell-outputCollapser">
</div>


<div class="jp-OutputArea jp-Cell-outputArea">

<div class="jp-OutputArea-child">

    
    <div class="jp-OutputPrompt jp-OutputArea-prompt"></div>


<div class="jp-RenderedText jp-OutputArea-output" data-mime-type="application/vnd.jupyter.stderr">
<pre>&lt;ipython-input-52-f689b35de9e7&gt;:1: UserWarning: Boolean Series key will be reindexed to match DataFrame index.
  data_4_15 = data[data[&#39;observation_date&#39;] == &#39;2019-04-15&#39;][data[&#39;market&#39;] == &#39;AUSMCO&#39;]
</pre>
</div>
</div>

<div class="jp-OutputArea-child">

    
    <div class="jp-OutputPrompt jp-OutputArea-prompt">Out[&nbsp;]:</div>



<div class="jp-RenderedHTMLCommon jp-RenderedHTML jp-OutputArea-output jp-OutputArea-executeResult" data-mime-type="text/html">

  <div id="df-edcea75b-044d-4c77-97de-b3bec9a46737" class="colab-df-container">
    <div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>flight_number</th>
      <th>price_exc_tax</th>
      <th>tax</th>
      <th>price</th>
      <th>new_departure_date</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>566.000000</td>
      <td>566.000000</td>
      <td>566.000000</td>
      <td>566.000000</td>
      <td>566</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>1613.846290</td>
      <td>70.186307</td>
      <td>36.765000</td>
      <td>106.951307</td>
      <td>2019-06-27 17:28:11.872791552</td>
    </tr>
    <tr>
      <th>min</th>
      <td>732.000000</td>
      <td>-0.670000</td>
      <td>17.720000</td>
      <td>38.300000</td>
      <td>2019-05-01 00:00:00</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>1018.000000</td>
      <td>36.280000</td>
      <td>21.560000</td>
      <td>72.300000</td>
      <td>2019-05-28 00:00:00</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>1732.000000</td>
      <td>64.190000</td>
      <td>35.320000</td>
      <td>97.300000</td>
      <td>2019-06-24 00:00:00</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>2416.000000</td>
      <td>96.740000</td>
      <td>54.830000</td>
      <td>122.300000</td>
      <td>2019-07-28 18:00:00</td>
    </tr>
    <tr>
      <th>max</th>
      <td>2417.000000</td>
      <td>333.950000</td>
      <td>75.630000</td>
      <td>373.300000</td>
      <td>2019-08-31 00:00:00</td>
    </tr>
    <tr>
      <th>std</th>
      <td>744.992966</td>
      <td>52.587715</td>
      <td>16.035978</td>
      <td>51.783271</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>
    <div class="colab-df-buttons">

  <div class="colab-df-container">
    <button class="colab-df-convert" onclick="convertToInteractive('df-edcea75b-044d-4c77-97de-b3bec9a46737')"
            title="Convert this dataframe to an interactive table."
            style="display:none;">

  <svg xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 -960 960 960">
    <path d="M120-120v-720h720v720H120Zm60-500h600v-160H180v160Zm220 220h160v-160H400v160Zm0 220h160v-160H400v160ZM180-400h160v-160H180v160Zm440 0h160v-160H620v160ZM180-180h160v-160H180v160Zm440 0h160v-160H620v160Z"/>
  </svg>
    </button>

  <style>
    .colab-df-container {
      display:flex;
      gap: 12px;
    }

    .colab-df-convert {
      background-color: #E8F0FE;
      border: none;
      border-radius: 50%;
      cursor: pointer;
      display: none;
      fill: #1967D2;
      height: 32px;
      padding: 0 0 0 0;
      width: 32px;
    }

    .colab-df-convert:hover {
      background-color: #E2EBFA;
      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);
      fill: #174EA6;
    }

    .colab-df-buttons div {
      margin-bottom: 4px;
    }

    [theme=dark] .colab-df-convert {
      background-color: #3B4455;
      fill: #D2E3FC;
    }

    [theme=dark] .colab-df-convert:hover {
      background-color: #434B5C;
      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));
      fill: #FFFFFF;
    }
  </style>

    <script>
      const buttonEl =
        document.querySelector('#df-edcea75b-044d-4c77-97de-b3bec9a46737 button.colab-df-convert');
      buttonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';

      async function convertToInteractive(key) {
        const element = document.querySelector('#df-edcea75b-044d-4c77-97de-b3bec9a46737');
        const dataTable =
          await google.colab.kernel.invokeFunction('convertToInteractive',
                                                    [key], {});
        if (!dataTable) return;

        const docLinkHtml = 'Like what you see? Visit the ' +
          '<a target="_blank" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'
          + ' to learn more about interactive tables.';
        element.innerHTML = '';
        dataTable['output_type'] = 'display_data';
        await google.colab.output.renderOutput(dataTable, element);
        const docLink = document.createElement('div');
        docLink.innerHTML = docLinkHtml;
        element.appendChild(docLink);
      }
    </script>
  </div>


<div id="df-9fc65976-f236-4a4e-bc93-7db220d0ebf6">
  <button class="colab-df-quickchart" onclick="quickchart('df-9fc65976-f236-4a4e-bc93-7db220d0ebf6')"
            title="Suggest charts"
            style="display:none;">

<svg xmlns="http://www.w3.org/2000/svg" height="24px"viewBox="0 0 24 24"
     width="24px">
    <g>
        <path d="M19 3H5c-1.1 0-2 .9-2 2v14c0 1.1.9 2 2 2h14c1.1 0 2-.9 2-2V5c0-1.1-.9-2-2-2zM9 17H7v-7h2v7zm4 0h-2V7h2v10zm4 0h-2v-4h2v4z"/>
    </g>
</svg>
  </button>

<style>
  .colab-df-quickchart {
      --bg-color: #E8F0FE;
      --fill-color: #1967D2;
      --hover-bg-color: #E2EBFA;
      --hover-fill-color: #174EA6;
      --disabled-fill-color: #AAA;
      --disabled-bg-color: #DDD;
  }

  [theme=dark] .colab-df-quickchart {
      --bg-color: #3B4455;
      --fill-color: #D2E3FC;
      --hover-bg-color: #434B5C;
      --hover-fill-color: #FFFFFF;
      --disabled-bg-color: #3B4455;
      --disabled-fill-color: #666;
  }

  .colab-df-quickchart {
    background-color: var(--bg-color);
    border: none;
    border-radius: 50%;
    cursor: pointer;
    display: none;
    fill: var(--fill-color);
    height: 32px;
    padding: 0;
    width: 32px;
  }

  .colab-df-quickchart:hover {
    background-color: var(--hover-bg-color);
    box-shadow: 0 1px 2px rgba(60, 64, 67, 0.3), 0 1px 3px 1px rgba(60, 64, 67, 0.15);
    fill: var(--button-hover-fill-color);
  }

  .colab-df-quickchart-complete:disabled,
  .colab-df-quickchart-complete:disabled:hover {
    background-color: var(--disabled-bg-color);
    fill: var(--disabled-fill-color);
    box-shadow: none;
  }

  .colab-df-spinner {
    border: 2px solid var(--fill-color);
    border-color: transparent;
    border-bottom-color: var(--fill-color);
    animation:
      spin 1s steps(1) infinite;
  }

  @keyframes spin {
    0% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
      border-left-color: var(--fill-color);
    }
    20% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    30% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
      border-right-color: var(--fill-color);
    }
    40% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    60% {
      border-color: transparent;
      border-right-color: var(--fill-color);
    }
    80% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-bottom-color: var(--fill-color);
    }
    90% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
    }
  }
</style>

  <script>
    async function quickchart(key) {
      const quickchartButtonEl =
        document.querySelector('#' + key + ' button');
      quickchartButtonEl.disabled = true;  // To prevent multiple clicks.
      quickchartButtonEl.classList.add('colab-df-spinner');
      try {
        const charts = await google.colab.kernel.invokeFunction(
            'suggestCharts', [key], {});
      } catch (error) {
        console.error('Error during call to suggestCharts:', error);
      }
      quickchartButtonEl.classList.remove('colab-df-spinner');
      quickchartButtonEl.classList.add('colab-df-quickchart-complete');
    }
    (() => {
      let quickchartButtonEl =
        document.querySelector('#df-9fc65976-f236-4a4e-bc93-7db220d0ebf6 button');
      quickchartButtonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';
    })();
  </script>
</div>

    </div>
  </div>

</div>

</div>

</div>

</div>

</div><div class="jp-Cell jp-CodeCell jp-Notebook-cell   ">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea">
<div class="jp-InputPrompt jp-InputArea-prompt">In&nbsp;[&nbsp;]:</div>
<div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline">
     <div class="CodeMirror cm-s-jupyter">
<div class=" highlight hl-python"><pre><span></span><span class="n">grouped</span> <span class="o">=</span> <span class="n">data</span><span class="o">.</span><span class="n">groupby</span><span class="p">([</span><span class="s1">'flight_number'</span><span class="p">,</span><span class="s1">'carrier'</span><span class="p">])</span><span class="o">.</span><span class="n">size</span><span class="p">()</span><span class="o">.</span><span class="n">reset_index</span><span class="p">(</span><span class="n">name</span><span class="o">=</span><span class="s1">'Count'</span><span class="p">)</span>
</pre></div>

     </div>
</div>
</div>
</div>

<div class="jp-Cell-outputWrapper">
<div class="jp-Collapser jp-OutputCollapser jp-Cell-outputCollapser">
</div>


<div class="jp-OutputArea jp-Cell-outputArea">

<div class="jp-OutputArea-child">

    
    <div class="jp-OutputPrompt jp-OutputArea-prompt">Out[&nbsp;]:</div>




<div class="jp-RenderedText jp-OutputArea-output jp-OutputArea-executeResult" data-mime-type="text/plain">
<pre>flight_number  carrier  Count
65             C7       9676     1
66             C7       9678     1
301            EM       315      1
306            EM       631      1
310            EM       26       1
                                ..
6267           EM       34       1
6280           EM       457      1
6289           EM       466      1
6291           EM       388      1
6302           EM       265      1
Name: count, Length: 295, dtype: int64</pre>
</div>

</div>

</div>

</div>

</div><div class="jp-Cell jp-CodeCell jp-Notebook-cell jp-mod-noOutputs  ">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea">
<div class="jp-InputPrompt jp-InputArea-prompt">In&nbsp;[&nbsp;]:</div>
<div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline">
     <div class="CodeMirror cm-s-jupyter">
<div class=" highlight hl-python"><pre><span></span><span class="n">flights_with_multiple_markets</span> <span class="o">=</span> <span class="n">grouped</span><span class="o">.</span><span class="n">groupby</span><span class="p">(</span><span class="s1">'flight_number'</span><span class="p">)</span><span class="o">.</span><span class="n">filter</span><span class="p">(</span><span class="k">lambda</span> <span class="n">x</span><span class="p">:</span> <span class="n">x</span><span class="p">[</span><span class="s1">'carrier'</span><span class="p">]</span><span class="o">.</span><span class="n">nunique</span><span class="p">()</span> <span class="o">&gt;</span> <span class="mi">1</span><span class="p">)</span>
</pre></div>

     </div>
</div>
</div>
</div>

</div><div class="jp-Cell jp-CodeCell jp-Notebook-cell   ">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea">
<div class="jp-InputPrompt jp-InputArea-prompt">In&nbsp;[&nbsp;]:</div>
<div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline">
     <div class="CodeMirror cm-s-jupyter">
<div class=" highlight hl-python"><pre><span></span><span class="nb">print</span><span class="p">(</span><span class="n">flights_with_multiple_markets</span><span class="p">)</span>
</pre></div>

     </div>
</div>
</div>
</div>

<div class="jp-Cell-outputWrapper">
<div class="jp-Collapser jp-OutputCollapser jp-Cell-outputCollapser">
</div>


<div class="jp-OutputArea jp-Cell-outputArea">

<div class="jp-OutputArea-child">

    
    <div class="jp-OutputPrompt jp-OutputArea-prompt"></div>


<div class="jp-RenderedText jp-OutputArea-output" data-mime-type="text/plain">
<pre>    flight_number carrier  Count
12            381      C7  10423
13            381      EM     26
45            732      C7   8479
46            732      OL   8230
</pre>
</div>
</div>

</div>

</div>

</div><div class="jp-Cell jp-CodeCell jp-Notebook-cell jp-mod-noOutputs  ">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea">
<div class="jp-InputPrompt jp-InputArea-prompt">In&nbsp;[&nbsp;]:</div>
<div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline">
     <div class="CodeMirror cm-s-jupyter">
<div class=" highlight hl-python"><pre><span></span><span class="n">data</span><span class="p">[</span><span class="s1">'departure_year'</span><span class="p">]</span> <span class="o">=</span> <span class="n">data</span><span class="p">[</span><span class="s1">'departure_date'</span><span class="p">]</span><span class="o">.</span><span class="n">dt</span><span class="o">.</span><span class="n">year</span>
</pre></div>

     </div>
</div>
</div>
</div>

</div><div class="jp-Cell jp-CodeCell jp-Notebook-cell jp-mod-noOutputs  ">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea">
<div class="jp-InputPrompt jp-InputArea-prompt">In&nbsp;[&nbsp;]:</div>
<div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline">
     <div class="CodeMirror cm-s-jupyter">
<div class=" highlight hl-python"><pre><span></span><span class="n">data</span><span class="p">[</span><span class="s1">'departure_month'</span><span class="p">]</span> <span class="o">=</span>  <span class="n">data</span><span class="p">[</span><span class="s1">'departure_date'</span><span class="p">]</span><span class="o">.</span><span class="n">dt</span><span class="o">.</span><span class="n">month</span>
</pre></div>

     </div>
</div>
</div>
</div>

</div><div class="jp-Cell jp-CodeCell jp-Notebook-cell jp-mod-noOutputs  ">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea">
<div class="jp-InputPrompt jp-InputArea-prompt">In&nbsp;[&nbsp;]:</div>
<div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline">
     <div class="CodeMirror cm-s-jupyter">
<div class=" highlight hl-python"><pre><span></span><span class="n">data</span><span class="o">.</span><span class="n">head</span><span class="p">()</span>
</pre></div>

     </div>
</div>
</div>
</div>

</div><div class="jp-Cell jp-CodeCell jp-Notebook-cell jp-mod-noOutputs  ">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea">
<div class="jp-InputPrompt jp-InputArea-prompt">In&nbsp;[&nbsp;]:</div>
<div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline">
     <div class="CodeMirror cm-s-jupyter">
<div class=" highlight hl-python"><pre><span></span><span class="n">c7_flights</span> <span class="o">=</span> <span class="n">data</span><span class="p">[(</span><span class="n">data</span><span class="p">[</span><span class="s1">'carrier'</span><span class="p">]</span> <span class="o">==</span> <span class="s1">'C7'</span><span class="p">)</span> <span class="o">&amp;</span> <span class="p">(</span><span class="n">data</span><span class="p">[</span><span class="s1">'departure_year'</span><span class="p">]</span> <span class="o">==</span> <span class="mi">2019</span><span class="p">)</span> <span class="o">&amp;</span> <span class="p">(</span><span class="n">data</span><span class="p">[</span><span class="s1">'departure_month'</span><span class="p">]</span> <span class="o">==</span> <span class="mi">6</span><span class="p">)]</span>
</pre></div>

     </div>
</div>
</div>
</div>

</div><div class="jp-Cell jp-CodeCell jp-Notebook-cell   ">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea">
<div class="jp-InputPrompt jp-InputArea-prompt">In&nbsp;[&nbsp;]:</div>
<div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline">
     <div class="CodeMirror cm-s-jupyter">
<div class=" highlight hl-python"><pre><span></span><span class="n">c7_flights</span><span class="o">.</span><span class="n">groupby</span><span class="p">(</span><span class="s1">'market'</span><span class="p">)[</span><span class="s1">'price'</span><span class="p">]</span><span class="o">.</span><span class="n">describe</span><span class="p">()</span>
</pre></div>

     </div>
</div>
</div>
</div>

<div class="jp-Cell-outputWrapper">
<div class="jp-Collapser jp-OutputCollapser jp-Cell-outputCollapser">
</div>


<div class="jp-OutputArea jp-Cell-outputArea">

<div class="jp-OutputArea-child">

    
    <div class="jp-OutputPrompt jp-OutputArea-prompt">Out[&nbsp;]:</div>



<div class="jp-RenderedHTMLCommon jp-RenderedHTML jp-OutputArea-output jp-OutputArea-executeResult" data-mime-type="text/html">

  <div id="df-bfd9c0b3-8870-4d18-9987-94a5e9cc2029" class="colab-df-container">
    <div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>count</th>
      <th>mean</th>
      <th>std</th>
      <th>min</th>
      <th>25%</th>
      <th>50%</th>
      <th>75%</th>
      <th>max</th>
    </tr>
    <tr>
      <th>market</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>ABQJFK</th>
      <td>5160.0</td>
      <td>179.456136</td>
      <td>37.052482</td>
      <td>113.98</td>
      <td>160.98</td>
      <td>169.80</td>
      <td>190.50</td>
      <td>387.98</td>
    </tr>
    <tr>
      <th>AUSMCO</th>
      <td>5160.0</td>
      <td>144.688027</td>
      <td>57.301812</td>
      <td>58.30</td>
      <td>103.30</td>
      <td>138.05</td>
      <td>163.30</td>
      <td>568.30</td>
    </tr>
    <tr>
      <th>BOSLGA</th>
      <td>24248.0</td>
      <td>90.789030</td>
      <td>19.664549</td>
      <td>58.30</td>
      <td>82.30</td>
      <td>82.30</td>
      <td>95.30</td>
      <td>251.30</td>
    </tr>
    <tr>
      <th>BOSRIC</th>
      <td>20290.0</td>
      <td>119.489469</td>
      <td>48.766870</td>
      <td>72.30</td>
      <td>82.30</td>
      <td>98.30</td>
      <td>148.30</td>
      <td>315.30</td>
    </tr>
    <tr>
      <th>FLLPVD</th>
      <td>5160.0</td>
      <td>132.219483</td>
      <td>37.003207</td>
      <td>88.98</td>
      <td>113.98</td>
      <td>124.98</td>
      <td>143.98</td>
      <td>396.98</td>
    </tr>
  </tbody>
</table>
</div>
    <div class="colab-df-buttons">

  <div class="colab-df-container">
    <button class="colab-df-convert" onclick="convertToInteractive('df-bfd9c0b3-8870-4d18-9987-94a5e9cc2029')"
            title="Convert this dataframe to an interactive table."
            style="display:none;">

  <svg xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 -960 960 960">
    <path d="M120-120v-720h720v720H120Zm60-500h600v-160H180v160Zm220 220h160v-160H400v160Zm0 220h160v-160H400v160ZM180-400h160v-160H180v160Zm440 0h160v-160H620v160ZM180-180h160v-160H180v160Zm440 0h160v-160H620v160Z"/>
  </svg>
    </button>

  <style>
    .colab-df-container {
      display:flex;
      gap: 12px;
    }

    .colab-df-convert {
      background-color: #E8F0FE;
      border: none;
      border-radius: 50%;
      cursor: pointer;
      display: none;
      fill: #1967D2;
      height: 32px;
      padding: 0 0 0 0;
      width: 32px;
    }

    .colab-df-convert:hover {
      background-color: #E2EBFA;
      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);
      fill: #174EA6;
    }

    .colab-df-buttons div {
      margin-bottom: 4px;
    }

    [theme=dark] .colab-df-convert {
      background-color: #3B4455;
      fill: #D2E3FC;
    }

    [theme=dark] .colab-df-convert:hover {
      background-color: #434B5C;
      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));
      fill: #FFFFFF;
    }
  </style>

    <script>
      const buttonEl =
        document.querySelector('#df-bfd9c0b3-8870-4d18-9987-94a5e9cc2029 button.colab-df-convert');
      buttonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';

      async function convertToInteractive(key) {
        const element = document.querySelector('#df-bfd9c0b3-8870-4d18-9987-94a5e9cc2029');
        const dataTable =
          await google.colab.kernel.invokeFunction('convertToInteractive',
                                                    [key], {});
        if (!dataTable) return;

        const docLinkHtml = 'Like what you see? Visit the ' +
          '<a target="_blank" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'
          + ' to learn more about interactive tables.';
        element.innerHTML = '';
        dataTable['output_type'] = 'display_data';
        await google.colab.output.renderOutput(dataTable, element);
        const docLink = document.createElement('div');
        docLink.innerHTML = docLinkHtml;
        element.appendChild(docLink);
      }
    </script>
  </div>


<div id="df-3da91756-26a6-4ae1-a74a-bc05910fd766">
  <button class="colab-df-quickchart" onclick="quickchart('df-3da91756-26a6-4ae1-a74a-bc05910fd766')"
            title="Suggest charts"
            style="display:none;">

<svg xmlns="http://www.w3.org/2000/svg" height="24px"viewBox="0 0 24 24"
     width="24px">
    <g>
        <path d="M19 3H5c-1.1 0-2 .9-2 2v14c0 1.1.9 2 2 2h14c1.1 0 2-.9 2-2V5c0-1.1-.9-2-2-2zM9 17H7v-7h2v7zm4 0h-2V7h2v10zm4 0h-2v-4h2v4z"/>
    </g>
</svg>
  </button>

<style>
  .colab-df-quickchart {
      --bg-color: #E8F0FE;
      --fill-color: #1967D2;
      --hover-bg-color: #E2EBFA;
      --hover-fill-color: #174EA6;
      --disabled-fill-color: #AAA;
      --disabled-bg-color: #DDD;
  }

  [theme=dark] .colab-df-quickchart {
      --bg-color: #3B4455;
      --fill-color: #D2E3FC;
      --hover-bg-color: #434B5C;
      --hover-fill-color: #FFFFFF;
      --disabled-bg-color: #3B4455;
      --disabled-fill-color: #666;
  }

  .colab-df-quickchart {
    background-color: var(--bg-color);
    border: none;
    border-radius: 50%;
    cursor: pointer;
    display: none;
    fill: var(--fill-color);
    height: 32px;
    padding: 0;
    width: 32px;
  }

  .colab-df-quickchart:hover {
    background-color: var(--hover-bg-color);
    box-shadow: 0 1px 2px rgba(60, 64, 67, 0.3), 0 1px 3px 1px rgba(60, 64, 67, 0.15);
    fill: var(--button-hover-fill-color);
  }

  .colab-df-quickchart-complete:disabled,
  .colab-df-quickchart-complete:disabled:hover {
    background-color: var(--disabled-bg-color);
    fill: var(--disabled-fill-color);
    box-shadow: none;
  }

  .colab-df-spinner {
    border: 2px solid var(--fill-color);
    border-color: transparent;
    border-bottom-color: var(--fill-color);
    animation:
      spin 1s steps(1) infinite;
  }

  @keyframes spin {
    0% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
      border-left-color: var(--fill-color);
    }
    20% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    30% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
      border-right-color: var(--fill-color);
    }
    40% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    60% {
      border-color: transparent;
      border-right-color: var(--fill-color);
    }
    80% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-bottom-color: var(--fill-color);
    }
    90% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
    }
  }
</style>

  <script>
    async function quickchart(key) {
      const quickchartButtonEl =
        document.querySelector('#' + key + ' button');
      quickchartButtonEl.disabled = true;  // To prevent multiple clicks.
      quickchartButtonEl.classList.add('colab-df-spinner');
      try {
        const charts = await google.colab.kernel.invokeFunction(
            'suggestCharts', [key], {});
      } catch (error) {
        console.error('Error during call to suggestCharts:', error);
      }
      quickchartButtonEl.classList.remove('colab-df-spinner');
      quickchartButtonEl.classList.add('colab-df-quickchart-complete');
    }
    (() => {
      let quickchartButtonEl =
        document.querySelector('#df-3da91756-26a6-4ae1-a74a-bc05910fd766 button');
      quickchartButtonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';
    })();
  </script>
</div>

    </div>
  </div>

</div>

</div>

</div>

</div>

</div><div class="jp-Cell jp-CodeCell jp-Notebook-cell jp-mod-noOutputs  ">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea">
<div class="jp-InputPrompt jp-InputArea-prompt">In&nbsp;[&nbsp;]:</div>
<div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline">
     <div class="CodeMirror cm-s-jupyter">
<div class=" highlight hl-python"><pre><span></span><span class="n">c7_flights</span><span class="o">.</span><span class="n">groupby</span><span class="p">([</span><span class="s1">'market'</span><span class="p">,</span><span class="s1">'flight_number'</span><span class="p">,</span><span class="s1">'origin'</span><span class="p">,</span><span class="s1">'destination'</span><span class="p">])[</span><span class="s1">'price'</span><span class="p">]</span><span class="o">.</span><span class="n">mean</span><span class="p">()</span>
</pre></div>

     </div>
</div>
</div>
</div>

</div><div class="jp-Cell jp-CodeCell jp-Notebook-cell jp-mod-noOutputs  ">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea">
<div class="jp-InputPrompt jp-InputArea-prompt">In&nbsp;[&nbsp;]:</div>
<div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline">
     <div class="CodeMirror cm-s-jupyter">
<div class=" highlight hl-python"><pre><span></span><span class="n">c7_flights</span><span class="o">.</span><span class="n">groupby</span><span class="p">([</span><span class="s1">'market'</span><span class="p">,</span><span class="s1">'flight_number'</span><span class="p">,</span><span class="s1">'origin'</span><span class="p">,</span><span class="s1">'destination'</span><span class="p">])[</span><span class="s1">'price'</span><span class="p">]</span><span class="o">.</span><span class="n">std</span><span class="p">()</span>
</pre></div>

     </div>
</div>
</div>
</div>

</div><div class="jp-Cell jp-CodeCell jp-Notebook-cell jp-mod-noOutputs  ">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea">
<div class="jp-InputPrompt jp-InputArea-prompt">In&nbsp;[&nbsp;]:</div>
<div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline">
     <div class="CodeMirror cm-s-jupyter">
<div class=" highlight hl-python"><pre><span></span><span class="n">data</span><span class="p">[</span><span class="s1">'days_remain'</span><span class="p">]</span> <span class="o">=</span> <span class="p">(</span><span class="n">data</span><span class="p">[</span><span class="s1">'departure_date'</span><span class="p">]</span> <span class="o">-</span> <span class="n">data</span><span class="p">[</span><span class="s1">'observation_date'</span><span class="p">])</span><span class="o">.</span><span class="n">dt</span><span class="o">.</span><span class="n">days</span>
</pre></div>

     </div>
</div>
</div>
</div>

</div><div class="jp-Cell jp-CodeCell jp-Notebook-cell jp-mod-noOutputs  ">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea">
<div class="jp-InputPrompt jp-InputArea-prompt">In&nbsp;[&nbsp;]:</div>
<div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline">
     <div class="CodeMirror cm-s-jupyter">
<div class=" highlight hl-python"><pre><span></span><span class="n">data</span><span class="o">.</span><span class="n">groupby</span><span class="p">(</span><span class="s1">'booking_class'</span><span class="p">)[</span><span class="s1">'price'</span><span class="p">]</span><span class="o">.</span><span class="n">count</span><span class="p">()</span>
</pre></div>

     </div>
</div>
</div>
</div>

</div><div class="jp-Cell jp-CodeCell jp-Notebook-cell jp-mod-noOutputs  ">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea">
<div class="jp-InputPrompt jp-InputArea-prompt">In&nbsp;[&nbsp;]:</div>
<div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline">
     <div class="CodeMirror cm-s-jupyter">
<div class=" highlight hl-python"><pre><span></span><span class="n">grouped_data</span> <span class="o">=</span> <span class="n">data</span><span class="o">.</span><span class="n">groupby</span><span class="p">([</span><span class="s1">'carrier'</span><span class="p">,</span><span class="s1">'market'</span><span class="p">,</span><span class="s1">'flight_number'</span><span class="p">,</span><span class="s1">'origin'</span><span class="p">,</span><span class="s1">'destination'</span><span class="p">,</span><span class="s1">'departure_date'</span><span class="p">,</span><span class="s1">'booking_class'</span><span class="p">])[</span><span class="s1">'price'</span><span class="p">]</span><span class="o">.</span><span class="n">std</span><span class="p">()</span>
</pre></div>

     </div>
</div>
</div>
</div>

</div><div class="jp-Cell jp-CodeCell jp-Notebook-cell jp-mod-noOutputs  ">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea">
<div class="jp-InputPrompt jp-InputArea-prompt">In&nbsp;[&nbsp;]:</div>
<div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline">
     <div class="CodeMirror cm-s-jupyter">
<div class=" highlight hl-python"><pre><span></span><span class="n">grouped_data</span> <span class="o">=</span> <span class="n">grouped_data</span><span class="o">.</span><span class="n">reset_index</span><span class="p">()</span>
</pre></div>

     </div>
</div>
</div>
</div>

</div><div class="jp-Cell jp-CodeCell jp-Notebook-cell jp-mod-noOutputs  ">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea">
<div class="jp-InputPrompt jp-InputArea-prompt">In&nbsp;[&nbsp;]:</div>
<div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline">
     <div class="CodeMirror cm-s-jupyter">
<div class=" highlight hl-python"><pre><span></span><span class="n">grouped_data</span><span class="o">.</span><span class="n">to_csv</span><span class="p">(</span><span class="s1">'/content/drive/MyDrive/airfares_std.csv'</span><span class="p">,</span><span class="n">index</span><span class="o">=</span><span class="kc">False</span><span class="p">)</span>
</pre></div>

     </div>
</div>
</div>
</div>

</div><div class="jp-Cell jp-CodeCell jp-Notebook-cell   ">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea">
<div class="jp-InputPrompt jp-InputArea-prompt">In&nbsp;[&nbsp;]:</div>
<div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline">
     <div class="CodeMirror cm-s-jupyter">
<div class=" highlight hl-python"><pre><span></span><span class="n">top_3</span> <span class="o">=</span> <span class="n">grouped_data</span><span class="o">.</span><span class="n">nlargest</span><span class="p">(</span><span class="mi">3</span><span class="p">,</span> <span class="s1">'price'</span><span class="p">)</span>
<span class="nb">print</span><span class="p">(</span><span class="n">top_3</span><span class="p">)</span>
</pre></div>

     </div>
</div>
</div>
</div>

<div class="jp-Cell-outputWrapper">
<div class="jp-Collapser jp-OutputCollapser jp-Cell-outputCollapser">
</div>


<div class="jp-OutputArea jp-Cell-outputArea">

<div class="jp-OutputArea-child">

    
    <div class="jp-OutputPrompt jp-OutputArea-prompt"></div>


<div class="jp-RenderedText jp-OutputArea-output" data-mime-type="text/plain">
<pre>      carrier  market  flight_number origin destination departure_date  \
27885      EM  BOSLGA           6031    BOS         LGA     2019-05-23   
22893      EM  BOSLGA           5870    BOS         LGA     2019-07-05   
16919      EM  BOSLGA            485    BOS         LGA     2019-05-17   

      booking_class      price  
27885             E  74.558627  
22893             E  69.613412  
16919             E  66.972631  
</pre>
</div>
</div>

</div>

</div>

</div><div class="jp-Cell jp-CodeCell jp-Notebook-cell jp-mod-noOutputs  ">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea">
<div class="jp-InputPrompt jp-InputArea-prompt">In&nbsp;[&nbsp;]:</div>
<div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline">
     <div class="CodeMirror cm-s-jupyter">
<div class=" highlight hl-python"><pre><span></span><span class="n">flight_6031</span> <span class="o">=</span> <span class="n">data</span><span class="p">[(</span><span class="n">data</span><span class="p">[</span><span class="s1">'flight_number'</span><span class="p">]</span> <span class="o">==</span> <span class="mi">6031</span><span class="p">)</span> <span class="o">&amp;</span> <span class="p">(</span><span class="n">data</span><span class="p">[</span><span class="s1">'booking_class'</span><span class="p">]</span> <span class="o">==</span> <span class="s1">'E'</span><span class="p">)</span> <span class="o">&amp;</span> <span class="p">(</span><span class="n">data</span><span class="p">[</span><span class="s1">'departure_date'</span><span class="p">]</span> <span class="o">==</span> <span class="s1">'2019-05-23'</span><span class="p">)]</span>
<span class="n">flight_5870</span> <span class="o">=</span> <span class="n">data</span><span class="p">[(</span><span class="n">data</span><span class="p">[</span><span class="s1">'flight_number'</span><span class="p">]</span> <span class="o">==</span> <span class="mi">5870</span><span class="p">)</span> <span class="o">&amp;</span> <span class="p">(</span><span class="n">data</span><span class="p">[</span><span class="s1">'booking_class'</span><span class="p">]</span> <span class="o">==</span> <span class="s1">'E'</span><span class="p">)</span> <span class="o">&amp;</span> <span class="p">(</span><span class="n">data</span><span class="p">[</span><span class="s1">'departure_date'</span><span class="p">]</span> <span class="o">==</span> <span class="s1">'2019-07-05'</span><span class="p">)]</span>
<span class="n">flight_485</span> <span class="o">=</span> <span class="n">data</span><span class="p">[(</span><span class="n">data</span><span class="p">[</span><span class="s1">'flight_number'</span><span class="p">]</span> <span class="o">==</span> <span class="mi">485</span><span class="p">)</span> <span class="o">&amp;</span> <span class="p">(</span><span class="n">data</span><span class="p">[</span><span class="s1">'booking_class'</span><span class="p">]</span> <span class="o">==</span> <span class="s1">'E'</span><span class="p">)</span> <span class="o">&amp;</span> <span class="p">(</span><span class="n">data</span><span class="p">[</span><span class="s1">'departure_date'</span><span class="p">]</span> <span class="o">==</span> <span class="s1">'2019-05-17'</span><span class="p">)]</span>
</pre></div>

     </div>
</div>
</div>
</div>

</div><div class="jp-Cell jp-CodeCell jp-Notebook-cell   ">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea">
<div class="jp-InputPrompt jp-InputArea-prompt">In&nbsp;[&nbsp;]:</div>
<div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline">
     <div class="CodeMirror cm-s-jupyter">
<div class=" highlight hl-python"><pre><span></span><span class="c1"># prompt: plot 'price' over 'observation_date'</span>

<span class="n">flight_6031</span><span class="o">.</span><span class="n">plot</span><span class="p">(</span><span class="n">x</span><span class="o">=</span><span class="s1">'observation_date'</span><span class="p">,</span> <span class="n">y</span><span class="o">=</span><span class="s1">'price'</span><span class="p">,</span> <span class="n">kind</span><span class="o">=</span><span class="s1">'line'</span><span class="p">)</span>
</pre></div>

     </div>
</div>
</div>
</div>

<div class="jp-Cell-outputWrapper">
<div class="jp-Collapser jp-OutputCollapser jp-Cell-outputCollapser">
</div>


<div class="jp-OutputArea jp-Cell-outputArea">

<div class="jp-OutputArea-child">

    
    <div class="jp-OutputPrompt jp-OutputArea-prompt">Out[&nbsp;]:</div>




<div class="jp-RenderedText jp-OutputArea-output jp-OutputArea-executeResult" data-mime-type="text/plain">
<pre>&lt;Axes: xlabel=&#39;observation_date&#39;&gt;</pre>
</div>

</div>

<div class="jp-OutputArea-child">

    
    <div class="jp-OutputPrompt jp-OutputArea-prompt"></div>




<div class="jp-RenderedImage jp-OutputArea-output ">
<img src="data:image/png;base64,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
"
class="
"
>
</div>

</div>

</div>

</div>

</div><div class="jp-Cell jp-CodeCell jp-Notebook-cell   ">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea">
<div class="jp-InputPrompt jp-InputArea-prompt">In&nbsp;[&nbsp;]:</div>
<div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline">
     <div class="CodeMirror cm-s-jupyter">
<div class=" highlight hl-python"><pre><span></span><span class="n">flight_5870</span><span class="o">.</span><span class="n">plot</span><span class="p">(</span><span class="n">x</span><span class="o">=</span><span class="s1">'observation_date'</span><span class="p">,</span> <span class="n">y</span><span class="o">=</span><span class="s1">'price'</span><span class="p">,</span> <span class="n">kind</span><span class="o">=</span><span class="s1">'line'</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">show</span><span class="p">()</span>
</pre></div>

     </div>
</div>
</div>
</div>

<div class="jp-Cell-outputWrapper">
<div class="jp-Collapser jp-OutputCollapser jp-Cell-outputCollapser">
</div>


<div class="jp-OutputArea jp-Cell-outputArea">

<div class="jp-OutputArea-child">

    
    <div class="jp-OutputPrompt jp-OutputArea-prompt"></div>




<div class="jp-RenderedImage jp-OutputArea-output ">
<img src="data:image/png;base64,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
"
class="
"
>
</div>

</div>

</div>

</div>

</div><div class="jp-Cell jp-CodeCell jp-Notebook-cell   ">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea">
<div class="jp-InputPrompt jp-InputArea-prompt">In&nbsp;[&nbsp;]:</div>
<div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline">
     <div class="CodeMirror cm-s-jupyter">
<div class=" highlight hl-python"><pre><span></span><span class="n">flight_485</span><span class="o">.</span><span class="n">plot</span><span class="p">(</span><span class="n">x</span><span class="o">=</span><span class="s1">'observation_date'</span><span class="p">,</span> <span class="n">y</span><span class="o">=</span><span class="s1">'price'</span><span class="p">,</span> <span class="n">kind</span><span class="o">=</span><span class="s1">'line'</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">show</span><span class="p">()</span>
</pre></div>

     </div>
</div>
</div>
</div>

<div class="jp-Cell-outputWrapper">
<div class="jp-Collapser jp-OutputCollapser jp-Cell-outputCollapser">
</div>


<div class="jp-OutputArea jp-Cell-outputArea">

<div class="jp-OutputArea-child">

    
    <div class="jp-OutputPrompt jp-OutputArea-prompt"></div>




<div class="jp-RenderedImage jp-OutputArea-output ">
<img src="data:image/png;base64,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
"
class="
"
>
</div>

</div>

</div>

</div>

</div><div class="jp-Cell jp-CodeCell jp-Notebook-cell   ">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea">
<div class="jp-InputPrompt jp-InputArea-prompt">In&nbsp;[&nbsp;]:</div>
<div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline">
     <div class="CodeMirror cm-s-jupyter">
<div class=" highlight hl-python"><pre><span></span><span class="n">correlation_results</span> <span class="o">=</span> <span class="n">data</span><span class="o">.</span><span class="n">groupby</span><span class="p">(</span>
    <span class="p">[</span><span class="s1">'carrier'</span><span class="p">,</span> <span class="s1">'market'</span><span class="p">,</span> <span class="s1">'flight_number'</span><span class="p">,</span> <span class="s1">'origin'</span><span class="p">,</span> <span class="s1">'destination'</span><span class="p">,</span> <span class="s1">'departure_date'</span><span class="p">,</span> <span class="s1">'booking_class'</span><span class="p">]</span>
<span class="p">)</span><span class="o">.</span><span class="n">apply</span><span class="p">(</span><span class="k">lambda</span> <span class="n">x</span><span class="p">:</span> <span class="n">x</span><span class="p">[</span><span class="s1">'price'</span><span class="p">]</span><span class="o">.</span><span class="n">corr</span><span class="p">(</span><span class="n">x</span><span class="p">[</span><span class="s1">'days_remain'</span><span class="p">]))</span><span class="o">.</span><span class="n">reset_index</span><span class="p">(</span><span class="n">name</span><span class="o">=</span><span class="s1">'correlation'</span><span class="p">)</span>
</pre></div>

     </div>
</div>
</div>
</div>

<div class="jp-Cell-outputWrapper">
<div class="jp-Collapser jp-OutputCollapser jp-Cell-outputCollapser">
</div>


<div class="jp-OutputArea jp-Cell-outputArea">

<div class="jp-OutputArea-child">

    
    <div class="jp-OutputPrompt jp-OutputArea-prompt"></div>


<div class="jp-RenderedText jp-OutputArea-output" data-mime-type="application/vnd.jupyter.stderr">
<pre>/usr/local/lib/python3.10/dist-packages/numpy/lib/function_base.py:2889: RuntimeWarning: Degrees of freedom &lt;= 0 for slice
  c = cov(x, y, rowvar, dtype=dtype)
/usr/local/lib/python3.10/dist-packages/numpy/lib/function_base.py:2748: RuntimeWarning: divide by zero encountered in divide
  c *= np.true_divide(1, fact)
</pre>
</div>
</div>

</div>

</div>

</div><div class="jp-Cell jp-CodeCell jp-Notebook-cell jp-mod-noOutputs  ">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea">
<div class="jp-InputPrompt jp-InputArea-prompt">In&nbsp;[&nbsp;]:</div>
<div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline">
     <div class="CodeMirror cm-s-jupyter">
<div class=" highlight hl-python"><pre><span></span><span class="n">correlation_cleaned_1</span> <span class="o">=</span> <span class="n">correlation_results</span><span class="p">[(</span><span class="n">correlation_results</span><span class="p">[</span><span class="s1">'correlation'</span><span class="p">]</span> <span class="o">&lt;</span> <span class="mf">0.99999</span><span class="p">)</span> <span class="o">&amp;</span> <span class="p">(</span><span class="n">correlation_results</span><span class="p">[</span><span class="s1">'correlation'</span><span class="p">]</span> <span class="o">&gt;</span> <span class="o">-</span><span class="mf">0.99999</span><span class="p">)</span> <span class="o">&amp;</span> <span class="p">(</span><span class="n">correlation_results</span><span class="p">[</span><span class="s1">'booking_class'</span><span class="p">]</span> <span class="o">==</span> <span class="s1">'E'</span><span class="p">)]</span>
</pre></div>

     </div>
</div>
</div>
</div>

</div><div class="jp-Cell jp-CodeCell jp-Notebook-cell   ">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea">
<div class="jp-InputPrompt jp-InputArea-prompt">In&nbsp;[&nbsp;]:</div>
<div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline">
     <div class="CodeMirror cm-s-jupyter">
<div class=" highlight hl-python"><pre><span></span><span class="n">top_3_corr</span> <span class="o">=</span> <span class="n">correlation_cleaned_1</span><span class="o">.</span><span class="n">nlargest</span><span class="p">(</span><span class="mi">3</span><span class="p">,</span> <span class="s1">'correlation'</span><span class="p">)</span>
<span class="nb">print</span><span class="p">(</span><span class="n">top_3_corr</span><span class="p">)</span>
</pre></div>

     </div>
</div>
</div>
</div>

<div class="jp-Cell-outputWrapper">
<div class="jp-Collapser jp-OutputCollapser jp-Cell-outputCollapser">
</div>


<div class="jp-OutputArea jp-Cell-outputArea">

<div class="jp-OutputArea-child">

    
    <div class="jp-OutputPrompt jp-OutputArea-prompt"></div>


<div class="jp-RenderedText jp-OutputArea-output" data-mime-type="text/plain">
<pre>      carrier  market  flight_number origin destination departure_date  \
30398      EM  BOSLGA           6058    LGA         BOS     2019-05-05   
36309      EM  BOSRIC           3794    RIC         BOS     2019-06-09   
27126      EM  BOSLGA           5996    LGA         BOS     2019-07-05   

      booking_class  correlation  
30398             E     0.998941  
36309             E     0.998119  
27126             E     0.990383  
</pre>
</div>
</div>

</div>

</div>

</div><div class="jp-Cell jp-CodeCell jp-Notebook-cell jp-mod-noOutputs  ">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea">
<div class="jp-InputPrompt jp-InputArea-prompt">In&nbsp;[&nbsp;]:</div>
<div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline">
     <div class="CodeMirror cm-s-jupyter">
<div class=" highlight hl-python"><pre><span></span><span class="n">flight_6058</span> <span class="o">=</span> <span class="n">data</span><span class="p">[(</span><span class="n">data</span><span class="p">[</span><span class="s1">'flight_number'</span><span class="p">]</span> <span class="o">==</span> <span class="mi">6058</span><span class="p">)</span> <span class="o">&amp;</span> <span class="p">(</span><span class="n">data</span><span class="p">[</span><span class="s1">'booking_class'</span><span class="p">]</span> <span class="o">==</span> <span class="s1">'E'</span><span class="p">)</span> <span class="o">&amp;</span> <span class="p">(</span><span class="n">data</span><span class="p">[</span><span class="s1">'departure_date'</span><span class="p">]</span> <span class="o">==</span> <span class="s1">'2019-05-05'</span><span class="p">)]</span>
</pre></div>

     </div>
</div>
</div>
</div>

</div><div class="jp-Cell jp-CodeCell jp-Notebook-cell   ">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea">
<div class="jp-InputPrompt jp-InputArea-prompt">In&nbsp;[&nbsp;]:</div>
<div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline">
     <div class="CodeMirror cm-s-jupyter">
<div class=" highlight hl-python"><pre><span></span><span class="n">flight_6058</span><span class="o">.</span><span class="n">plot</span><span class="p">(</span><span class="n">x</span><span class="o">=</span><span class="s1">'observation_date'</span><span class="p">,</span> <span class="n">y</span><span class="o">=</span><span class="s1">'price'</span><span class="p">,</span> <span class="n">kind</span><span class="o">=</span><span class="s1">'line'</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">show</span><span class="p">()</span>
</pre></div>

     </div>
</div>
</div>
</div>

<div class="jp-Cell-outputWrapper">
<div class="jp-Collapser jp-OutputCollapser jp-Cell-outputCollapser">
</div>


<div class="jp-OutputArea jp-Cell-outputArea">

<div class="jp-OutputArea-child">

    
    <div class="jp-OutputPrompt jp-OutputArea-prompt"></div>




<div class="jp-RenderedImage jp-OutputArea-output ">
<img src="data:image/png;base64,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iIiIiKhMWJor0L+NJwBgadwNAMDyw4k4eTMD9lbm+KxnYznL+1cMRERERFRm+rfxgkIC4q7ew4ErdzF96wUAwAch9eGmtH7Ks+XDQERERERlxsPRBp0augEA3l50BNn5RWhWQ4WB/rXkLewpGIiIiIioTA30f3hwdb5aC4UEfN27KcwUksxV/TsGIiIiIipTAXWc4e1sBwAY3L4WmlRXyVzR0/E6RERERFSmFAoJP/RvgT0JqRgaWFvuckrF4GeIYmNj0bNnT3h4eECSJKxbt67YekmSSnz897//1W2Tnp6OAQMGQKlUwtHREUOHDkVOTk4F7wkREZHpaFJdhYgX6xnkNYdKYvCBKDc3F76+vpg9e3aJ6+/cuVPs8euvv0KSJPTt21e3zYABA3D27FlER0dj06ZNiI2NxfDhwytqF4iIiMjASUIIIXcRpSVJEtauXYuwsLAnbhMWFobs7Gzs3LkTAHD+/Hk0atQIR44cQatWrQAAW7duRbdu3XDr1i14eHiU6r2zsrKgUqmQmZkJpVL53PtCRERE5a+0n98GP0Okj5SUFPz1118YOnSobllcXBwcHR11YQgAOnXqBIVCgUOHDj3xtQoKCpCVlVXsQURERJVTpQpEixcvhoODA/r06aNblpycDFfX4pcJNzc3h5OTE5KTk5/4WlOnToVKpdI9PD09y61uIiIiklelCkS//vorBgwYAGvr578S5oQJE5CZmal73Lx5swwqJCIiIkNUaU6737t3LxISEvDHH38UW+7u7o7U1NRiy4qKipCeng53d/cnvp6VlRWsrKzKpVYiIiIyLJVmhuiXX36Bn58ffH19iy339/dHRkYGjh07plu2a9cuaLVatG3btqLLJCIiIgNk8DNEOTk5uHz5su7na9eu4eTJk3BycoKXlxeAh0eQr1q1Ct99991jz2/YsCG6dOmCd955B3PnzoVarUZERARef/31Up9hRkRERJWbwc8QHT16FC1atECLFi0AAJGRkWjRogUmTZqk22bFihUQQqB///4lvsayZcvQoEEDvPTSS+jWrRsCAwMxb968CqmfiIiIDJ9RXYdITrwOERERkfExyesQERERET0Lgz+GyFA8mkjjBRqJiIiMx6PP7ad9IcZAVErZ2dkAwAs0EhERGaHs7GyoVKonrucxRKWk1WqRlJQEBwcHSJL0r9tmZWXB09MT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jcjJydFts23bNuTl5aF3794AgNzcXERGRuLo0aPYuXMnFAoFevfuDa1WCwA4fPgwAGDHjh24c+cO1qxZ88z7DwD/+c9/8N133+Ho0aMwNzfH22+/Xar+rly5EpMnT8bXX3+No0ePolq1apgzZ06xbbKzszF48GDs27cPBw8eRL169dCtWzdkZ2cDgC6gLVy4EHfu3NH9vHfvXgwaNAhjxozBuXPn8PPPP2PRokX46quvSlUbkckRREQlyMnJERYWFmLZsmW6ZYWFhcLDw0NMnz5d7N69WwAQK1as0K2/d++esLGxEX/88YcQQoimTZuKyZMnl/j6X3zxhQgJCSm27ObNmwKASEhIEEIIERwcLFq0aFFsG7VaLZydncWSJUt0y/r37y9ee+21J+5LWlqaACDi4+OFEEJcu3ZNABAnTpwott3gwYNFr169SrX/QghdD3bs2KHb5q+//hIAxIMHD55YzyP+/v5i5MiRxZa1bdtW+Pr6PvE5Go1GODg4iI0bN+qWARBr164ttt1LL70kvv7662LLli5dKqpVq/bUuohMEWeIiKhEV65cgVqtRkBAgG6ZhYUF2rRpg/Pnz+uW+fv76/7byckJPj4+uvWjR4/Gl19+iYCAAHz22Wc4ffq0bttTp05h9+7dsLe31z0aNGige+9H/Pz8itVlbm6OV199FcuWLQPwcDZo/fr1GDBggG6bS5cuoX///qhduzaUSqXua6TExMQy338AaNasme6/q1WrBgBITU196nucP38ebdu2Lbbs7/0EgJSUFLzzzjuoV68eVCoVlEolcnJynrovp06dwueff16sv++88w7u3LmDvLy8p9ZGZGp4UDURlZthw4YhNDQUf/31F7Zv346pU6fiu+++w6hRo5CTk4OePXvim2++eex5j0IFANjZ2T22fsCAAQgODkZqaiqio6NhY2ODLl266Nb37NkTNWvWxPz58+Hh4QGtVosmTZqgsLCwXPbTwsJC99+SJAGA7uu55zV48GDcu3cPM2fORM2aNWFlZQV/f/+n7ktOTg6mTJmCPn36PLbO2tq6TGojqkw4Q0REJapTp47u2J1H1Go1jhw5gkaNGumWHTx4UPff9+/fx8WLF9GwYUPdMk9PT7z33ntYs2YN3n//fcyfPx8A0LJlS5w9exa1atVC3bp1iz1KCkF/1759e3h6euKPP/7AsmXL8Morr+hCyb1795CQkIBPP/0UL730Eho2bIj79+8Xe/6jM9U0Gs1z7//zaNiwIQ4dOlRs2d/7CTw8aH306NHo1q0bGjduDCsrq2IHngMPA9k/96Vly5ZISEh4rLd169aFQsFf/UT/xBkiIiqRnZ0dRowYgfHjx8PJyQleXl6YPn068vLyMHToUJw6dQoA8Pnnn6Nq1apwc3PDf/7zHzg7O+vO1Bo7diy6du2K+vXr4/79+9i9e7cuLIWHh2P+/Pno378/PvzwQzg5OeHy5ctYsWIFFixYADMzs3+t74033sDcuXNx8eJF7N69W7e8SpUqqFq1KubNm4dq1aohMTERH3/8cbHnurq6wsbGBlu3bkWNGjVgbW392Cn3T9v/sjBmzBgMGTIErVq1QkBAAJYtW4azZ8+idu3aum3q1auHpUuXolWrVsjKysL48eNhY2NT7HVq1aqFnTt3IiAgAFZWVqhSpQomTZqEHj16wMvLC/369YNCocCpU6dw5swZfPnll2VSP1GlIvdBTERkuB48eCBGjRolnJ2dhZWVlQgICBCHDx8WQvzvgOKNGzeKxo0bC0tLS9GmTRtx6tQp3fMjIiJEnTp1hJWVlXBxcREDBw4Ud+/e1a2/ePGi6N27t3B0dBQ2NjaiQYMGYuzYsUKr1QohHh5UPWbMmBJrO3funAAgatasqdv+kejoaNGwYUNhZWUlmjVrJvbs2fPYgcfz588Xnp6eQqFQiODgYCFE8YOqn7b/f+/B/fv3dctOnDghAIhr166VqsdfffWVcHZ2Fvb29mLw4MHiww8/LHZQ9fHjx0WrVq2EtbW1qFevnli1apWoWbOmiIqK0m2zYcMGUbduXWFubi5q1qypW75161bRvn17YWNjI5RKpWjTpo2YN29eqeoiMjWSEELImsiIiIiIZMYvkomIiMjkMRAREZWTxo0bFzvt/e+PR5cNICLDwK/MiIjKyY0bN3RX7f6nR7frICLDwEBEREREJo9fmREREZHJYyAiIiIik8dARERERCaPgYiIiIhMHgMRERERmTwGIiIiIjJ5DERERERk8hiIiIiIyOT9HyJA1hjVYmFHAAAAAElFTkSuQmCC
"
class="
"
>
</div>

</div>

</div>

</div>

</div>
<div class="jp-Cell jp-MarkdownCell jp-Notebook-cell">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea"><div class="jp-InputPrompt jp-InputArea-prompt">
</div><div class="jp-RenderedHTMLCommon jp-RenderedMarkdown jp-MarkdownOutput " data-mime-type="text/markdown">
<p>correlation positive - Price goes down as getting close to departure date
correlation negative - Price goes up as getting close to departure date
create a union between correlation_cleaned_1, grouped_data and data called merged_data</p>

</div>
</div>
</div>
</div><div class="jp-Cell jp-CodeCell jp-Notebook-cell   ">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea">
<div class="jp-InputPrompt jp-InputArea-prompt">In&nbsp;[&nbsp;]:</div>
<div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline">
     <div class="CodeMirror cm-s-jupyter">
<div class=" highlight hl-python"><pre><span></span><span class="n">plt</span><span class="o">.</span><span class="n">figure</span><span class="p">(</span><span class="n">figsize</span><span class="o">=</span><span class="p">(</span><span class="mi">20</span><span class="p">,</span><span class="mi">7</span><span class="p">))</span>
<span class="n">sns</span><span class="o">.</span><span class="n">boxplot</span><span class="p">(</span><span class="n">data</span><span class="o">=</span><span class="n">correlation_cleaned_1</span><span class="p">,</span> <span class="n">x</span><span class="o">=</span><span class="s1">'correlation'</span><span class="p">)</span>
</pre></div>

     </div>
</div>
</div>
</div>

<div class="jp-Cell-outputWrapper">
<div class="jp-Collapser jp-OutputCollapser jp-Cell-outputCollapser">
</div>


<div class="jp-OutputArea jp-Cell-outputArea">

<div class="jp-OutputArea-child">

    
    <div class="jp-OutputPrompt jp-OutputArea-prompt">Out[&nbsp;]:</div>




<div class="jp-RenderedText jp-OutputArea-output jp-OutputArea-executeResult" data-mime-type="text/plain">
<pre>&lt;Axes: xlabel=&#39;correlation&#39;&gt;</pre>
</div>

</div>

<div class="jp-OutputArea-child">

    
    <div class="jp-OutputPrompt jp-OutputArea-prompt"></div>




<div class="jp-RenderedImage jp-OutputArea-output ">
<img src="data:image/png;base64,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MAEAAAAAAKQRJgAAAAAAgDTCBAAAAAAAkEaYAAAAAAAA0ggTAAAAAABAGmECAAAAAABII0wAAAAAAABphAkAAAAAACCNMAEAAAAAAKQRJgAAAAAAgDTCBAAAAAAAkEaYAAAAAAAA0ggTAAAAAABAmk7NvWNRFBERUV9f32KTAQAAAAAA2qa9vWBvPziQZoeJLVu2RETEgAEDmnsKAAAAAADgCLNly5bo0aPHAW+vKA6WLg6goaEhNm7cGN27d4+KiopmT/BIUF9fHwMGDIiXX345ampqWns60GTWMG2dNUxbZv3S1lnDtGXWL22dNUxbZw3Tllm/+1cURWzZsiX69+8fHToc+J0kmv2KiQ4dOsSxxx7b3LsfkWpqaixC2jRrmLbOGqYts35p66xh2jLrl7bOGqats4Zpy6zffb3fKyX28ubXAAAAAABAGmECAAAAAABII0y0gKqqqrjuuuuiqqqqtacCzWIN09ZZw7Rl1i9tnTVMW2b90tZZw7R11jBtmfV7aJr95tcAAAAAAABN5RUTAAAAAABAGmECAAAAAABII0wAAAAAAABphAkAAAAAACCNMNFIN998c4wZMya6du0aPXv2bNR9iqKIa6+9Nvr16xddunSJM888M1544YWyMW+99VZceOGFUVNTEz179owpU6bE1q1bD8MjoD1r6jp76aWXoqKiYr8f9913X2nc/m6fP39+xkOinWnOXnn66afvsz6//OUvl43ZsGFDTJgwIbp27RrHHHNMXHXVVbF79+7D+VBop5q6ht9666249NJLY+jQodGlS5cYOHBgXHbZZVFXV1c2zj7M4XDnnXfGhz/84aiuro5Ro0bFk08++b7j77vvvjjppJOiuro6hg8fHgsWLCi7vTHXxNCSmrKGZ8+eHX/4h38YRx99dBx99NFx5pln7jN+8uTJ++y148ePP9wPg3asKWt43rx5+6zP6urqsjH2YTI1Zf3u73u2ioqKmDBhQmmMPZgsjz/+eHzmM5+J/v37R0VFRfz0pz896H0WLVoUH//4x6OqqiqGDBkS8+bN22dMU6+t2xNhopF27twZn/vc5+IrX/lKo+9z6623xne/+9343ve+F0uXLo1u3brF2WefHe+++25pzIUXXhjPPvtsPPLII/Hv//7v8fjjj8e0adMOx0OgHWvqOhswYEC89tprZR833HBDHHXUUXHO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22+/HU8//XRpzH/+539GQ0ND6Qk0OJDGrt9DXWdz5syJP/mTP4k+ffocdOzy5cvj6KOP9mQYjZK1hvdavnx5RETpG7LRo0fHihUryp4wfuSRR6KmpiY+8pGPtMyD5Ih2uNdwfX19nHXWWVFZWRk/+9nPyn5690DswxyKysrK+P3f//2y69eGhoZ49NFHy34a971Gjx5dNj7i//fSveMbc00MLaU5azgi4tZbb42bbropFi5cWPZ+QAfyyiuvxJtvvln2JC+0hOau4ffas2dPrFixorQ+7cNkOZT1e99998WOHTviC1/4wkG/jj2YD4qDXQe3xJ5+xGvtd99uK9avX18sW7asuOGGG4qjjjqqWLZsWbFs2bJiy5YtpTFDhw4tHnjggdLns2bNKnr27Fk8+OCDxTPPPFOce+65xeDBg4vt27eXxowfP74YMWJEsXTp0uK///u/ixNOOKGYOHFi6mPjyHewdfbKK68UQ4cOLZYuXVp2vxdeeKGoqKgoHnrooX3O+bOf/ayYPXt2sWLFiuKFF14o7rrrrqJr167Ftddee9gfD+1PU9fwmjVrihtvvLH49a9/Xaxbt6548MEHi+OOO6741Kc+VbrP7t27i4997GPFWWedVSxfvrxYuHBh0adPn2LmzJnpj48jX1PXcF1dXTFq1Khi+PDhxZo1a4rXXnut9LF79+6iKOzDHB7z588vqqqqinnz5hW//e1vi2nTphU9e/YsNm3aVBRFUVx00UXFjBkzSuMXL15cdOrUqbj99tuLVatWFdddd13RuXPnYsWKFaUxjbkmhpbS1DU8a9asorKysrj//vvL9tq93+dt2bKluPLKK4slS5YU69atK37xi18UH//4x4sTTjihePfdd1vlMXJka+oavuGGG4qHH364WLt2bfH0008XF1xwQVFdXV08++yzpTH2YbI0df3u9clPfrI4//zz9zluDybTli1bSs/3RkTx7W9/u1i2bFmxfv36oiiKYsaMGcVFF11UGv/iiy8WXbt2La666qpi1apVxZ133ll07NixWLhwYWnMwf5OtHfCRCNNmjSpiIh9Ph577LHSmIgo5s6dW/q8oaGhuOaaa4ra2tqiqqqqGDduXPHcc8+VnffNN98sJk6cWBx11FFFTU1N8ed//udlsQNawsHW2bp16/ZZz0VRFDNnziwGDBhQ7NmzZ59zPvTQQ8Wpp55aHHXUUUW3bt2KU045pfje976337FwqJq6hjds2FB86lOfKnr16lVUVVUVQ4YMKa666qqirq6u7LwvvfRScc455xRdunQpevfuXVxxxRXFrl27Mh8a7URT1/Bjjz223+uOiCjWrVtXFIV9mMPn7//+74uBAwcWlZWVxciRI4tf/epXpdvGjh1bTJo0qWz8j3/84+LEE08sKisri49+9KPFf/zHf5Td3phrYmhJTVnDgwYN2u9ee9111xVFURTvvPNOcdZZZxV9+vQpOnfuXAwaNKiYOnWqJxQ4rJqyhr/xjW+UxtbW1haf/vSni9/85jdl57MPk6mp1xGrV68uIqL4+c9/vs+57MFkOtD3YHvX7KRJk4qxY8fuc59TTz21qKysLI477riy54X3er+/E+1dRVEURdKLMwAAAAAAgHauXb/HBAAAAAAAkEuYAAAAAAAA0ggTAAAAAABAGmECAAAAAABII0wAAAAAAABphAkAAAAAACCNMAEAAAAAAKQRJgAAgGZ76aWXoqKiIpYvX/6BOA8AAPDB16m1JwAAALQvkydPjrfffjt++tOflo4NGDAgXnvttejdu3frTQwAAEjhFRMAANDO7dy5c7/Hd+3alTaHjh07Rt++faNTJz87BQAARzphAgAA2qCGhoa49dZbY8iQIVFVVRUDBw6Mm2++OSIiVqxYEWeccUZ06dIlPvShD8W0adNi69atpftOnjw5zjvvvLj55pujf//+MXTo0NKvUvrRj34UY8eOjerq6vjBD34QERHf//73Y9iwYVFdXR0nnXRS3HXXXQec1549e2LKlCkxePDg6NKlSwwdOjT+7u/+rnT79ddfH/fcc088+OCDUVFRERUVFbFo0aL9/iqnX/7ylzFy5MioqqqKfv36xYwZM2L37t2l208//fS47LLL4uqrr45evXpF37594/rrr2+hP2EAAOBw8eNIAADQBs2cOTNmz54d3/nOd+KTn/xkvPbaa7F69erYtm1bnH322TF69Oh46qmn4vXXX4+LL744vva1r8W8efNK93/00UejpqYmHnnkkbLzzpgxI771rW/FiBEjSnHi2muvjTvuuCNGjBgRy5Yti6lTp0a3bt1i0qRJ+8yroaEhjj322LjvvvviQx/6UDzxxBMxbdq06NevX3z+85+PK6+8MlatWhX19fUxd+7ciIjo1atXbNy4sew8r776anz605+OyZMnx7333hurV6+OqVOnRnV1dVl8uOeee+Lyyy+PpUuXxpIlS2Ly5MnxiU98Iv74j/+45f6wAQCAFlVRFEXR2pMAAAAab8uWLdGnT5+444474uKLLy67bfbs2TF9+vR4+eWXo1u3bhERsWDBgvjMZz4TGzdujNra2pg8eXIsXLgwNmzYEJWVlRHx/28+PXjw4Pjbv/3b+PrXv14635AhQ+Kmm26KiRMnlo799V//dSxYsCCeeOKJ0v2WLVsWp5566n7n+7WvfS02bdoU999/f0Ts/z0mfvc83/zmN+Pf/u3fYtWqVVFRUREREXfddVdMnz496urqokOHDnH66afHnj174r/+679K5xk5cmScccYZMWvWrOb/AQMAAIeVV0wAAEAbs2rVqtixY0eMGzduv7edcsoppSgREfGJT3wiGhoa4rnnnova2tqIiBg+fHgpSrzXaaedVvr/bdu2xdq1a2PKlCkxderU0vHdu3dHjx49Dji/O++8M+6+++7YsGFDbN++PXbu3HnAaPF+j3H06NGlKLH3cWzdujVeeeWVGDhwYEREnHzyyWX369evX7z++utN+loAAEAuYQIAANqYLl26HPI53hsuDnR87/tSzJ49O0aNGlU2rmPHjvu9//z58+PKK6+Mb33rWzF69Ojo3r173HbbbbF06dJDnvP+dO7cuezzioqKaGhoOCxfCwAAaBne/BoAANqYE044Ibp06RKPPvroPrcNGzYs/ud//ie2bdtWOrZ48eLo0KFDDB06tElfp7a2Nvr37x8vvvhiDBkypOxj8ODB+73P4sWLY8yYMXHJJZfEiBEjYsiQIbF27dqyMZWVlbFnz573/drDhg2LJUuWxHt/8+zixYuje/fuceyxxzbpcQAAAB8swgQAALQx1dXVMX369Lj66qvj3nvvjbVr18avfvWrmDNnTlx44YVRXV0dkyZNipUrV8Zjjz0Wl156aVx00UWlX+PUFDfccEPccsst8d3vfjeef/75WLFiRcydOze+/e1v73f8CSecEL/+9a/j4Ycfjueffz6uueaaeOqpp8rGfPjDH45nnnkmnnvuuXjjjTdi165d+5znkksuiZdffjkuvfTSWL16dTz44INx3XXXxeWXXx4dOvg2BgAA2jJX9AAA0AZdc801ccUVV8S1114bw4YNi/PPPz9ef/316Nq1azz88MPx1ltvxR/8wR/En/7pn8a4cePijjvuaNbXufjii+P73/9+zJ07N4YPHx5jx46NefPmHfAVE1/60pfis5/9bJx//vkxatSoePPNN+OSSy4pGzN16tQYOnRonHbaadGnT59YvHjxPuf5vd/7vViwYEE8+eSTccopp8SXv/zlmDJlSvzVX/1Vsx4HAADwwVFRvPe10QAAAAAAAIeRV0wAAAAAAABphAkAAAAAACCNMAEAAAAAAKQRJgAAAAAAgDTCBAAAAAAAkEaYAAAAAAAA0ggTAAAAAABAGmECAAAAAABII0wAAAAAAABphAkAAAAAACCNMAEAAAAAAKQRJgAAAAAAgDT/B16GaNtwS0c/AAAAAElFTkSuQmCC
"
class="
"
>
</div>

</div>

</div>

</div>

</div><div class="jp-Cell jp-CodeCell jp-Notebook-cell jp-mod-noOutputs  ">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea">
<div class="jp-InputPrompt jp-InputArea-prompt">In&nbsp;[&nbsp;]:</div>
<div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline">
     <div class="CodeMirror cm-s-jupyter">
<div class=" highlight hl-python"><pre><span></span><span class="kn">import</span> <span class="nn">plotly.express</span> <span class="k">as</span> <span class="nn">px</span>
</pre></div>

     </div>
</div>
</div>
</div>

</div><div class="jp-Cell jp-CodeCell jp-Notebook-cell   ">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea">
<div class="jp-InputPrompt jp-InputArea-prompt">In&nbsp;[&nbsp;]:</div>
<div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline">
     <div class="CodeMirror cm-s-jupyter">
<div class=" highlight hl-python"><pre><span></span><span class="n">his</span> <span class="o">=</span> <span class="n">px</span><span class="o">.</span><span class="n">histogram</span><span class="p">(</span><span class="n">correlation_cleaned_1</span><span class="p">,</span> <span class="n">x</span><span class="o">=</span><span class="s2">"correlation"</span><span class="p">)</span>
<span class="n">his</span><span class="o">.</span><span class="n">show</span><span class="p">()</span>
</pre></div>

     </div>
</div>
</div>
</div>

<div class="jp-Cell-outputWrapper">
<div class="jp-Collapser jp-OutputCollapser jp-Cell-outputCollapser">
</div>


<div class="jp-OutputArea jp-Cell-outputArea">

<div class="jp-OutputArea-child">

    
    <div class="jp-OutputPrompt jp-OutputArea-prompt"></div>



<div class="jp-RenderedHTMLCommon jp-RenderedHTML jp-OutputArea-output " data-mime-type="text/html">
<html>
<head><meta charset="utf-8" /></head>
<body>
    <div>            <script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.5/MathJax.js?config=TeX-AMS-MML_SVG"></script><script type="text/javascript">if (window.MathJax && window.MathJax.Hub && window.MathJax.Hub.Config) {window.MathJax.Hub.Config({SVG: {font: "STIX-Web"}});}</script>                <script type="text/javascript">window.PlotlyConfig = {MathJaxConfig: 'local'};</script>
        <script charset="utf-8" src="https://cdn.plot.ly/plotly-2.24.1.min.js"></script>                <div id="eafdcd14-8725-4ecc-bac3-5bd13232f8a9" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("eafdcd14-8725-4ecc-bac3-5bd13232f8a9")) {                    Plotly.newPlot(                        "eafdcd14-8725-4ecc-bac3-5bd13232f8a9",                        [{"alignmentgroup":"True","bingroup":"x","hovertemplate":"correlation=%{x}\u003cbr\u003ecount=%{y}\u003cextra\u003e\u003c\u002fextra\u003e","legendgroup":"","marker":{"color":"#636efa","pattern":{"shape":""}},"name":"","offsetgroup":"","orientation":"v","showlegend":false,"x":[-2.459671071019196e-16,1.4033210043432363e-17,-9.03556809713034e-17,1.0696966606809828e-17,-1.612096590998897e-16,1.7187755710596515e-16,0.0,0.5255481254258932,-0.5758327474053667,-0.409886680865376,-0.7402636425828437,-0.5072510935797163,-0.5072510935797163,-0.7987026697916767,-0.26842819912727167,0.05267888071777816,-0.5738057640758938,-0.8755065505180941,-0.4772994971328174,-0.4595013734519867,-0.19665097370123671,-0.2568574027489347,-0.4595852889918056,-0.46038367030805644,0.2944801365093142,-0.7571460260084141,-0.8752366585831558,-0.24261146879315723,-0.18407066100001546,-0.18146350099395317,-0.17852542043270053,-0.18340891347407398,-0.23358829607557832,-0.15722782271371094,-0.15120650191292473,-0.1444000375739939,-0.13669332648557458,-0.19808849372200465,-0.09371055382753174,-0.07883567247064116,-0.22357372715928567,-0.22156468376279892,-0.9486832980505139,-0.8856716698613685,-0.18476543543111493,-0.6317328381028973,-0.6371415045475947,-0.784358511084447,-0.028806086788472237,-0.6763227850115382,-0.8320502943378437,0.0,0.6859136495928028,-0.36061240070350575,-0.6076436202502,-0.6167112603765267,0.25086956784290604,-0.7211102550927979,-0.6076436202502,0.0,-0.4300336902557983,0.6977645102488365,-0.9432176704893989,-0.47163405080053883,-0.9200320485671842,-0.8468371554719634,-0.4999556339160052,0.34712313581959187,-0.699651070144565,0.08733691911050154,-0.8320846345021716,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.30386330544433354,-0.41097275190813204,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.30618621784789724,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.3572172541558801,0.42008402520840293,0.0,0.0,0.0,0.0,0.30175637618632967,0.753836123399238,-0.5466987108167995,-0.5796845005411256,-0.6468586270022806,-0.4110640051812482,-0.46655905656046354,-0.4041428254758851,-0.8132826998251269,-0.19336589799714077,-0.17385718848961013,-0.1916516973954469,-0.6183959060074372,-0.7242777684983073,-0.040071169399731196,-0.08672514815303627,-0.06761893104033172,-0.7901064005586675,-0.5538590878588793,-0.7964209391609843,0.01803158834179388,0.0457391681174478,-0.4570861665896277,-0.7028488544107737,-0.7844149935661472,-0.17848733752484663,0.0457391681174478,-0.08380350995555515,-0.7092464497779751,-0.20830227647108812,-0.5072510935797163,0.07560687408872896,0.07560687408872896,0.07560687408872896,-0.7558932805774813,-0.04532067100150997,-0.5611824469375476,0.07475289757642294,0.07475289757642294,0.07475289757642294,-0.8711972402493054,-0.1921650927881063,-0.4955039914311546,-0.3093887744879017,-0.04532067100150997,0.12569520370686685,-0.9373369827536655,-0.6702691954545267,-0.20397302268110737,-0.04532067100150997,-0.04532067100150997,-0.04532067100150997,-0.771623196991334,-0.35531045098323033,-0.4453757647263919,-0.04532067100150997,-0.04532067100150997,-0.04532067100150997,-0.08705012322142966,-0.04532067100150997,-0.5072510935797163,-0.04532067100150997,-0.04532067100150997,-0.04532067100150997,-0.34532833369360527,-0.06487113796452927,-0.5072510935797163,-0.04532067100150997,-0.041027468117019086,-0.03657370946562567,-0.10548164196475006,-0.1824513943052656,-0.34617207128373007,-0.17739911193056052,-0.17553801543684555,-0.1735805146530606,-0.17152140523444018,-0.16935514040651103,0.31007070107738577,0.3253720292694837,0.4124965624732234,-0.6317328381028973,-0.6317328381028973,0.6584888643693478,-0.42426406871192845,-0.4121120667911454,-0.39629696195060854,-0.34893157889942655,-0.3137858162210945,-0.26726124191242445,-0.20483662259967564,-0.11952286093343938,0.0,0.0,0.2927700218845599,0.8660254037844386,0.8944271909999159,0.8660254037844387,-0.8532567040195683,-0.7224255476874464,-0.911108190579139,-0.6758600952678613,-0.8714946126426727,-0.8381796013187109,-0.824599296870297,-0.8299394128732913,-0.7609081953386038,0.30175637618632967,0.30175637618632967,0.30175637618632967,0.30175637618632967,-0.27850945371194413,-0.27850945371194413,-0.5072510935797163,-0.8427049149883191,-0.6687846174637712,-0.6687846174637712,-0.6317328381028973,-0.6317328381028973,-0.8320846345021716,-0.07687586598482582,-0.07687586598482582,-0.8159400690685238,0.2797583930775208,0.2797583930775208,-0.42426406871192845,-0.4121120667911454,-0.39629696195060854,-0.3137858162210945,-0.26726124191242445,-0.20483662259967564,-0.11952286093343938,0.0,0.0,0.2927700218845599,0.8660254037844386,0.8944271909999159,0.8660254037844387,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.3572172541558801,0.42008402520840293,0.0,0.0,0.0,0.0,0.8451542547285167,-0.03767759640882487,-0.032563837908042674,-0.0271319531127849,-0.02135741909902971,-0.015213330186454464,0.005747336627544263,0.013697231954607411,0.022198506204497863,0.0313000489163136,0.04105628503327913,0.07489776694980785,0.0879580235611868,0.10206071859169945,0.0051447747266863955,0.014063499420977474,-0.42426406871192845,-0.730612945216875,-0.8847248532800083,-0.34893157889942655,-0.3137858162210945,-0.26726124191242445,-0.20483662259967564,-0.5466567037632775,-0.5254842858368264,0.0,0.2927700218845599,-0.6063390625908323,-0.8660254037844386,0.24080732065382357,0.0,0.0,0.0,0.24080732065382357,0.24080732065382357,0.0,0.0,0.0,0.24080732065382357,0.24080732065382357,0.0,0.0,0.0,0.24080732065382357,0.24080732065382357,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.36220198612772353,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.7985959522630739,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.449472822097964,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.046538310889866305,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.34317491455179244,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.32945948714629425,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.30386330544433354,-0.3146242955624413,0.3572172541558801,0.42008402520840293,0.0,0.0,0.3872983346207418,0.0,-0.18638213806903792,-0.18407066100001546,-0.18146350099395317,-0.17852542043270053,-0.17521611247693536,-0.16256175514375495,-0.15722782271371094,-0.15120650191292473,-0.1444000375739939,-0.13669332648557458,-0.10667311465869792,-0.09371055382753174,-0.07883567247064116,-0.22357372715928567,-0.22156468376279892,-0.5466987108167995,-0.5307007495252439,-0.56236946948434,-0.2405965303143743,-0.4227907247760231,-0.3842496809105675,-0.31788386719839923,-0.47903480274707816,-0.07018238733199403,-0.1916516973954469,-0.1534962274924057,-0.023352354695650304,-0.1824513943052656,0.42608880537299676,-0.06761893104033172,-0.06761893104033172,0.0457391681174478,-0.1824513943052656,0.0457391681174478,0.0457391681174478,0.0457391681174478,-0.1824513943052656,-0.2405965303143743,0.0457391681174478,0.0457391681174478,0.0457391681174478,-0.1824513943052656,-0.048821133082794885,0.1817291430068192,0.1817291430068192,0.1817291430068192,0.09477128627851926,-0.021930425238224905,0.17791633745873886,0.17791633745873886,0.09477128627851926,0.09477128627851926,0.3491102318373655,0.09477128627851926,0.09477128627851926,0.09477128627851926,0.09477128627851926,-0.021930425238224905,0.09477128627851926,0.09477128627851926,0.09477128627851926,0.09477128627851926,-0.021930425238224905,0.09477128627851926,0.09477128627851926,0.09477128627851926,0.09477128627851926,-0.021930425238224905,0.09477128627851926,0.09477128627851926,0.09477128627851926,0.09477128627851926,-0.021930425238224905,0.10142120217782609,0.10833872430396445,-0.03905406833776175,-0.035957152877614404,-0.05497618301388856,-0.02205686216240914,-0.018158642745032025,-0.014070820410146904,-0.009781877077921202,-0.24261146879315723,-0.15949399325126223,-0.15669468699310854,-0.15374601382995673,-0.14935841877197628,-0.23358829607557832,-0.13642694962155083,-0.13237031683074144,-0.12808600973280163,-0.12355903070020725,0.29049648710093895,-0.10267783178188651,-0.09666383926917997,-0.1690154307368685,-0.16548119899755545,-0.8318802024491622,-0.6113145033649013,-0.8944888580814347,-0.4355531647676405,-0.7192786969912659,-0.763838803770407,0.18811421405793116,-0.48509570298149257,-0.7346538836973215,-0.648106670804399,-0.8102145495040506,-0.5544920194213856,-0.8059731519302274,-0.5138164607492066,-0.7973303467699564,-0.8988808375092886,0.4276101311725934,0.39844380571906424,-0.7005480711434865,-0.25566158798213956,-0.9207946507829299,-0.8686710478037943,-0.7562353446359651,-0.610043781975697,-0.8586019756651713,0.09925803939990518,-0.7897003597722038,-0.04532067100150997,-0.1768271467376723,0.04156785955627388,0.5610111075312871,-0.5691168088241053,-0.8001111245604021,-0.4032677305240133,-0.7257894973665957,-0.023804453317843297,0.5239205200731691,0.13400680285745686,-0.2577800760000893,-0.6400586571082341,-0.4463744811722532,-0.22123789544374733,0.575794446942915,0.05791196223644885,-0.3608951464308761,0.4776572230845337,-0.6239955214767249,-0.4995295730893446,0.43420415965943343,-0.8998722739953554,-0.8437982577173907,-0.2778471731666782,-0.4726072039852668,-0.513772329350717,0.6045668491889161,-0.46852415140348636,-0.8049637455150177,-0.615959419459787,-0.6037098755110591,-0.3362137181187624,0.4869544883397883,-0.6608599631204383,-0.843538754840017,0.7821129576121785,-0.6376967712347308,-0.4189407303849119,0.5603883599124352,-0.16321716982433798,-0.713011927429968,-0.48066943810675594,-0.7252627050229874,-0.47684739799726966,0.7745966692414834,-0.2633096041984289,-0.5412560194805698,0.586270743934678,-0.652293246697892,-0.8120661852080806,0.19010328192355885,-0.15063909005236367,-0.7100634797380653,-0.3981181744394125,-0.13642694962155083,-0.13237031683074144,-0.413134239427083,-0.12355903070020725,-0.7710051923877724,-0.10835253587315444,-0.10267783178188651,-0.09666383926917997,0.721365929864801,-0.16548119899755545,-0.6317328381028973,-0.6317328381028973,-0.6317328381028973,-0.564222841552055,-0.564222841552055,-0.18787577456684298,-0.17568187354729867,-0.1620492291825904,-0.11032749348962717,-0.08853906473527291,-0.06386718472761346,-0.035833399207393445,-0.0038569284319363998,-0.04184999018767307,-0.004913025817426305,0.03853996718248504,-0.06893529476616927,-0.20604670093634578,-0.008872032300814838,-0.40485708771149975,-0.41017145312333614,-0.41414141414141403,0.35094320882354124,-0.34874291623145787,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.03767759640882487,-0.032563837908042674,-0.0271319531127849,-0.02135741909902971,-0.015875480378819494,-1.6488864471291284e-15,0.005747336627544263,0.013697231954607411,0.022198506204497863,0.0313000489163136,0.04105628503327913,-1.6488864471291284e-15,0.07489776694980785,0.0879580235611868,0.10206071859169945,0.0051447747266863955,0.014063499420977474,-0.6317328381028973,-0.6317328381028973,-0.18831567956585557,0.0,0.0,0.0,-0.6122131207673417,-0.5829752480681659,0.8320502943378437,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.9773555548504418,0.0,0.0,0.0,0.0,0.0,0.005619972893020643,-0.032563837908042674,-0.0271319531127849,-0.02135741909902971,-0.015875480378819494,-0.7173829386238844,0.013697231954607411,0.022198506204497863,0.0313000489163136,0.04105628503327913,-0.4771698281291109,0.0879580235611868,0.10206071859169945,0.0051447747266863955,0.014063499420977474,-0.5072510935797163,-0.6317328381028973,-0.497669209366797,-0.45494892840427104,-0.564222841552055,-0.564222841552055,-0.564222841552055,-0.564222841552055,-0.564222841552055,-0.564222841552055,-0.564222841552055,-0.564222841552055,-0.564222841552055,-0.564222841552055,-0.564222841552055,-0.564222841552055,-0.564222841552055,-0.564222841552055,-0.564222841552055,-1.6488864471291284e-15,-0.3987494971033641,-0.3987494971033641,-0.3987494971033641,-0.3987494971033641,-0.3987494971033641,-0.3987494971033641,-0.3987494971033641,-0.3987494971033641,-0.3987494971033641,-0.3987494971033641,-0.3987494971033641,0.7020282012586665,-0.7905176369287903,-0.3987494971033641,0.37446896007919483,-0.3987494971033641,-0.40485708771149975,-0.4101714531233361,-0.41414141414141403,-0.41590019592802907,-0.34874291623145787,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.8125449006954456,-0.33668248767181036,-0.3784115038845798,-0.1738270303682741,0.5522049122097675,-0.1620492291825904,-0.11032749348962717,-0.407711203160731,-0.3704753283115795,0.6759413046758554,-0.3847102266368276,-0.04184999018767307,0.0459393927769468,0.5176919543678168,0.8227036552139989,-0.20604670093634578,-0.3987494971033641,-0.40485708771149975,0.8413556274043971,0.2881331947338557,0.35094320882354124,-0.34874291623145787,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,0.7712216513615849,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,0.0,0.0,0.0,-0.42426406871192845,-0.4121120667911454,-0.39629696195060854,-0.4177908080293783,-0.3137858162210945,-0.26726124191242445,-0.20483662259967564,-0.11952286093343938,0.0,0.0,0.2927700218845599,0.8660254037844386,0.8944271909999159,0.8660254037844387,0.691338153817062,-0.8109978430871709,-0.5072510935797163,-0.34643165865828407,-0.3228520682722359,-0.3995563398547124,-0.5340179792444578,-0.3865315635492028,-0.872222234355764,-0.7265180036922679,-0.547357862326405,-0.755625483568801,-0.6713364630536865,-0.6519794154396175,-0.6936699345524183,-0.18787577456684298,-0.52527084419849,-0.1620492291825904,-0.11032749348962717,-0.08853906473527291,-0.06386718472761346,-0.564122658713292,-0.0038569284319363998,-0.04184999018767307,-0.004913025817426305,0.8012419172031566,-0.7279467756558435,0.3892235788587853,-0.3987494971033641,-0.40485708771149975,0.8733170294265375,0.5615265176023098,0.5653012624592498,-0.34874291623145787,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,0.8657500188042037,-0.2581988897471611,-0.5611824469375476,-0.7368411161889257,0.0036870927259713246,-0.8267275285874656,-0.9057240429515035,-0.3876556197685335,-0.6763227850115382,-0.4256903152439407,-0.8583323475475115,-0.055659339401843276,-0.3171457346346632,-0.4070743469519496,-0.7039269394939001,-0.7894731357150289,-0.8843344298145671,-0.8562454159755216,-0.7251525315917058,-0.27684339381049916,-0.4722725318874888,-0.8659245840512073,-0.3966060350661918,0.524186102356398,0.524186102356398,-0.6905666938213713,-0.5318531146865967,-0.6725433837163121,0.41072039187951515,-0.7199554396151635,-0.42426406871192845,-0.7820332599506077,-0.39629696195060854,-0.5516874282271507,-0.3137858162210945,-0.26726124191242445,-0.7974166738540841,-0.8112281357074064,0.0,0.4140393356054125,0.8660254037844386,-0.18787577456684298,-0.4406894160437834,-0.44010404777142476,-0.8482387325636636,-0.11032749348962717,-0.08853906473527291,-0.06386718472761346,-0.3937585744654115,-0.33260241607323543,-0.5203569674668507,0.5239776044224742,-0.004913025817426305,0.03853996718248504,-0.06893529476616927,-0.20604670093634578,-0.3472789965251522,-0.4742438795056793,-0.40485708771149975,-0.41017145312333614,0.5646068349030373,0.35094320882354124,-0.34874291623145787,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.6680279285074198,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.8011949083906871,-0.771746456594677,-0.5917850914875249,-0.8392044170880598,-0.8044734482509702,-0.6584329501074256,-0.3176932875678042,-0.6421229289284462,0.27486462302597753,-0.78236673992914,-0.11457637372296056,-0.42426406871192845,-0.7820332599506077,-0.39629696195060854,-0.3757345746510897,-0.7313623049756413,-0.3137858162210945,-0.26726124191242445,-0.7189222884452698,0.0,0.0,0.2927700218845599,0.8660254037844386,-0.5546745852270812,-0.3339765610635413,0.036268046155524605,-0.8026541402239461,0.036268046155524605,0.896343503877875,-0.6220202267305106,0.036268046155524605,-0.8456412047003037,0.0,0.0,0.7792865001991967,0.0,0.0,0.0,0.0,-0.10888954438347671,0.0,-0.564222841552055,-0.564222841552055,-0.564222841552055,-0.564222841552055,-0.564222841552055,-0.564222841552055,-0.564222841552055,-0.564222841552055,-0.564222841552055,-0.564222841552055,-0.564222841552055,-0.564222841552055,-0.564222841552055,-0.564222841552055,-0.564222841552055,-0.564222841552055,-0.564222841552055,-0.8106951523080405,-0.6413325682207596,-0.7584970296175086,-0.7829073582000751,-0.7878731426089961,-0.4227907247760231,-0.3842496809105675,-0.21120417133238129,-0.6251630081071426,-0.24567881791743432,-0.6447178925807554,-0.4209797619050295,-0.24366302919979096,-0.510998669848118,-0.7175511726225487,-0.22396719807140641,-0.4238184722687099,-0.194519678392798,-0.7690512394018223,-0.6796611336531452,-0.7931799154222375,-0.6083341240543805,0.01803158834179388,-0.8225784323249414,-0.8171996435625536,-0.09878219139013399,-0.8600122658577627,-0.01845681290929272,0.01803158834179388,-0.7091727939728014,-0.8466513426198329,0.21806006231637132,-0.5546745852270812,-0.6346859827548845,-0.3339765610635413,-0.4765454629346102,-0.7404902499736904,-0.8522675493819228,-0.6905754078760737,0.036268046155524605,-0.5494905382447212,-0.564222841552055,-0.564222841552055,-0.5999129468136675,-0.6697644279240655,-0.6653490591836714,0.0,-0.554453083808078,-0.4227907247760231,-0.5948241179567804,-0.5306743857240219,-0.2842343890835119,-0.18400053823424192,-0.1916516973954469,-0.6391025431592287,-0.06311312308263925,-0.4045801395453808,0.0,0.0457391681174478,-0.06761893104033172,0.4772687591715035,-0.19509955267862555,-0.4046229980594938,0.0457391681174478,0.04053718749719146,0.12161139059635465,-0.32632673348205604,0.0,-0.1379100383138055,0.0457391681174478,0.0457391681174478,0.07676897587121659,-0.07543115208600575,0.07560687408872896,0.07560687408872896,0.07560687408872896,0.036420606927570986,-0.8630274620603751,0.07475289757642294,0.07475289757642294,-0.6886468250606907,0.5924653354305878,-0.14948214970062512,-0.10634889031996833,-0.04532067100150997,-0.2777346321977367,0.48829298793375314,0.15843609517800886,-0.04532067100150997,-0.04532067100150997,-0.04532067100150997,0.3526395486102408,0.1958213226658045,-0.04532067100150997,-0.04532067100150997,-0.17710771633348993,0.24219158568971477,0.09175628511385912,-0.04532067100150997,-0.04532067100150997,-0.04532067100150997,0.5517601843582048,0.16286511352363753,-0.21287584961218606,-0.041027468117019086,-0.03657370946562567,0.39795443127666774,-0.23096319695268583,-0.17739911193056052,-0.17553801543684555,-0.1735805146530606,0.38949192741360994,-0.5538848313456465,-0.1621522141856574,-0.15949399325126223,-0.15669468699310854,0.11390223407902385,-0.14935841877197628,-0.1402697716713447,-0.13642694962155083,-0.13237031683074144,-0.12808600973280163,-0.36151707699762486,-0.10835253587315444,-0.10267783178188651,-0.09666383926917997,-0.1690154307368685,-0.16548119899755545,-1.6488864471291284e-15,-1.6488864471291284e-15,-1.6488864471291284e-15,-1.6488864471291284e-15,-0.2499768869420293,-0.7198229044363504,-0.8076912583623738,-0.4444886457184703,-0.860767131793515,-0.5652518333295579,-0.7292390310596382,-0.8164288269613797,-0.7871436545340105,-0.130312384114833,-0.7683447297393942,0.29784137426640367,-0.9273695963839395,-0.5843444159759691,-0.56236946948434,-0.4355531647676405,-0.4227907247760231,-0.8422448717618063,-0.1964631063096171,-0.47903480274707816,-0.17385718848961013,-0.1916516973954469,-0.8717269669752675,-0.06756344446859418,-0.1824513943052656,0.0457391681174478,-0.06761893104033172,-0.4981601459644685,-0.08365586193581256,-0.1824513943052656,-0.6320793285708962,-0.8067922908129704,-0.8487112640022444,-0.15063909005236364,0.01803158834179388,0.01803158834179388,-0.13162583026382432,-0.4803778542252017,-0.15063909005236364,-0.6268204601833071,-1.6488864471291284e-15,0.17558579779133116,-1.6488864471291284e-15,-1.6488864471291284e-15,-0.011164998554411902,-0.6214001288779508,-0.22888247036544407,-0.18787577456684298,-0.17568187354729867,-0.1620492291825904,-0.3987494971033641,-0.11032749348962717,-0.08853906473527291,-0.06386718472761346,-0.035833399207393445,-0.0038569284319363998,-0.3987494971033641,-0.04184999018767307,-0.004913025817426305,0.03853996718248504,-0.06893529476616927,-0.20604670093634578,-0.3987494971033641,-0.3987494971033641,-0.40485708771149975,-0.41017145312333614,-0.41414141414141403,0.35094320882354124,-0.2688885926714207,-0.34874291623145787,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.7903872540206135,-0.8694725524516509,-0.3452167766486433,-0.7854688515952298,-0.4458783213171013,0.036268046155524605,0.11676820484505261,-0.6572194778360336,-1.6488864471291284e-15,-0.3094135162027918,-0.5600620281031974,-0.3784115038845798,-0.578578221262026,-0.6061264113283628,-0.8486927123130323,-0.4787706172685275,-0.4438507055469696,-0.400173209334939,-0.35667713047192573,-0.5112688735982868,-0.6531922948694863,-0.21221115682743802,-0.1686263257623422,-0.11492894270273496,-0.054703033084954916,-0.15063909005236364,-0.30161731836225897,-0.24379341300312848,0.01803158834179388,0.01803158834179388,0.01803158834179388,-0.15063909005236364,0.617434583943537,-0.15063909005236364,0.01803158834179388,0.01803158834179388,0.01803158834179388,-0.15063909005236364,0.036268046155524605,0.01803158834179388,0.01803158834179388,0.01803158834179388,0.01803158834179388,-0.15063909005236364,-0.08960114668558108,0.01803158834179388,0.01803158834179388,0.01803158834179388,-0.15063909005236364,0.036268046155524605,0.036268046155524605,0.036268046155524605,-0.15063909005236364,-0.15063909005236364,0.013736114931960178,-0.15063909005236364,-0.15063909005236364,-0.15063909005236364,-0.15063909005236364,-0.7721224987970026,-0.15063909005236364,-0.15063909005236364,-0.15063909005236364,-0.15063909005236364,-0.4380716182002235,-0.15063909005236364,-0.15063909005236364,-0.15063909005236364,-0.15063909005236364,-0.15063909005236364,-0.15063909005236364,-0.15063909005236364,-0.15063909005236364,-0.15063909005236364,-0.18859485648812843,-0.15063909005236367,-0.15063909005236367,-0.15063909005236367,-0.1945823674946614,-0.2937911015874425,-0.15063909005236367,-0.15063909005236367,-0.15063909005236367,-0.15063909005236367,-0.564222841552055,-0.564222841552055,-0.564222841552055,-0.564222841552055,-0.564222841552055,-0.564222841552055,-0.564222841552055,-0.564222841552055,-0.564222841552055,-0.564222841552055,-0.564222841552055,-0.564222841552055,-0.564222841552055,-0.564222841552055,-0.564222841552055,-0.49554598181104514,-0.5668875061407621,-0.56236946948434,-0.23908363720440012,-0.31804870897514975,-0.3995919608904714,-0.47903480274707816,-0.3530113934628655,0.2884665674932716,0.305041496436097,-0.1824513943052656,0.4568179376824804,0.16003886109897392,-0.3662001023181603,-0.1824513943052656,-0.685647606178376,-0.3655411745964387,0.06684948008500867,-0.15063909005236364,0.2347246402183407,-0.6289094808422796,-0.143264846660312,-0.15063909005236364,-0.18787577456684298,0.08880309760848579,-0.1620492291825904,-0.12962957161691033,-0.11032749348962717,-0.08853906473527291,-0.06386718472761346,-0.27466111742578053,-0.0038569284319363998,-0.3987494971033641,-0.04184999018767307,-0.004913025817426305,0.42348788817343747,-0.4268984450500482,0.2604912678625827,-0.3922063624015568,-0.3987494971033641,-0.40485708771149975,0.8779672695277168,-0.5267685613081697,-0.6043526629201531,-0.3876990572676328,-0.34874291623145787,-0.2581988897471611,-0.7726084304603636,0.903839307030387,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.3987494971033641,-0.3987494971033641,-0.3987494971033641,-0.3987494971033641,0.34571638874381266,-0.3987494971033641,-0.3987494971033641,0.42512064641057246,0.8399850670648771,-0.3987494971033641,-0.3987494971033641,0.776778974117323,-0.41017145312333614,-0.015076208503633951,-0.41590019592802907,-0.16408625224302628,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,0.7226824402144922,0.9150513403586291,0.40453555215536807,-0.2581988897471611,-0.3987494971033641,0.8207792427954428,-0.3987494971033641,-0.3987494971033641,-0.3987494971033641,0.34525260237182454,-0.3987494971033641,0.2771114349306469,-0.3987494971033641,-0.3987494971033641,-0.3987494971033641,0.5026199168679021,0.6047836038880235,-0.27763552744764336,0.8487146001182361,0.8747574049211727,-0.9063346479997262,-0.3876990572676327,0.43641380970896815,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,0.7226824402144922,-0.2581988897471611,-0.3987494971033641,-0.5976210687225585,-0.3987494971033641,0.8242295196086844,-0.3987494971033641,-0.3987494971033641,-0.3987494971033641,0.5895583624412873,-0.3987494971033641,-0.2814858176709156,-0.3987494971033641,-0.3987494971033641,-0.3987494971033641,0.8590220074410236,-0.03479249519479471,-0.3987494971033641,-0.3987494971033641,-0.40485708771149975,0.8413556274043971,-0.4246463090454151,-0.5455722965095927,-0.3876990572676328,-0.34874291623145787,-0.2581988897471611,-0.07032494875510756,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.42426406871192845,-0.4121120667911454,-0.39629696195060854,-0.3137858162210945,-0.26726124191242445,-0.20483662259967564,-0.11952286093343938,0.0,0.0,0.2927700218845599,0.8660254037844386,0.8944271909999159,0.8660254037844387,-0.36509451194791254,-0.564222841552055,-0.564222841552055,0.0,0.0,0.0,0.5397918963594132,0.0,0.8320502943378437,0.0,0.0,0.0,-0.08011021752161376,0.7923376125751668,-0.7438293036226136,-0.5434894424296203,-0.16903085094570333,-0.8652258233316383,-0.13118101904837842,-0.18787577456684298,-0.17568187354729867,-0.1620492291825904,-0.3757345746510897,-0.11032749348962717,-0.08853906473527291,-0.06386718472761346,-0.035833399207393445,-0.0038569284319363998,-0.27386127875258304,0.6194618279543184,-0.004913025817426305,0.03853996718248504,-0.06893529476616927,-0.20604670093634578,-0.3987494971033641,-0.40485708771149975,-0.41017145312333614,-0.41414141414141403,0.35094320882354124,-0.34874291623145787,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,0.14333625034837733,0.8210006675472517,0.30386330544433354,0.30386330544433354,0.8701214368921526,0.3038633054443336,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.7852049938547898,0.8866724102743619,0.4647785487156839,-0.09059437064070719,0.30386330544433354,0.30386330544433354,0.7838136063070197,0.46635980958929757,0.7985959522630739,-0.40540887784473467,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.8630632202908544,0.4647785487156839,0.48970305443947126,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.8785153812964257,0.30386330544433354,0.5067038549017634,0.30386330544433354,0.30386330544433354,0.48970305443947126,0.8710821558087153,0.30386330544433354,0.522970315991872,0.30386330544433354,0.3572172541558801,0.42008402520840293,0.7164878441809671,0.0,0.0,0.0,0.0,-0.01676387183827571,-0.03767759640882487,-0.20506802639839744,-0.0271319531127849,0.6798131943847705,-0.1711646854196799,-0.21692394645385385,0.005747336627544263,0.013697231954607411,-0.6098235056143564,0.7236492392655527,0.04105628503327913,0.06278302487360335,0.07489776694980785,0.0879580235611868,0.10206071859169945,0.7957260125568446,0.014063499420977474,-1.6488864471291284e-15,-1.6488864471291284e-15,-1.6488864471291284e-15,-1.6488864471291284e-15,-1.6488864471291284e-15,-1.6488864471291284e-15,-1.6488864471291284e-15,-1.6488864471291284e-15,-1.6488864471291284e-15,-1.6488864471291284e-15,-1.6488864471291284e-15,-1.6488864471291284e-15,-1.6488864471291284e-15,-1.6488864471291284e-15,-1.6488864471291284e-15,-0.5466987108167995,-0.7293546771316569,-0.7309748921657242,-0.4355531647676405,-0.4227907247760231,-0.3842496809105675,-0.7223708185422573,-0.5485962113224708,-0.567648954994561,-0.1916516973954469,-0.1534962274924057,-0.023352354695650304,-0.4730648417010925,0.15892925787971918,-0.06761893104033172,-0.06761893104033172,-0.7198266205159278,0.1978741753689927,0.3530509831901872,0.0457391681174478,0.0457391681174478,-0.1824513943052656,-0.07031470860138414,0.0457391681174478,0.0457391681174478,0.0457391681174478,-0.1824513943052656,-0.27125901131720703,0.07560687408872896,0.07560687408872896,0.07560687408872896,-0.04532067100150997,-0.18878209771359195,0.07475289757642294,0.07475289757642294,-0.04532067100150997,-0.04532067100150997,0.08912111486148737,-0.04532067100150997,-0.04532067100150997,-0.04532067100150997,-0.04532067100150997,-0.6317328381028973,-0.6317328381028973,-0.6317328381028973,-0.6317328381028973,0.5296806152156662,0.7544943974410387,-0.5834086708471007,-0.04532067100150997,-0.04532067100150997,-0.04532067100150997,-0.04532067100150997,-0.12539766653155682,-0.04532067100150997,-0.04532067100150997,-0.04532067100150997,-0.04532067100150997,-0.4191169937392447,-0.04532067100150997,-0.04532067100150997,-0.04532067100150997,-0.10858383174809631,0.2800929984135786,-0.041027468117019086,-0.03657370946562567,-0.183972902336745,-0.1824513943052656,-0.203843810946071,-0.17553801543684555,-0.1735805146530606,-0.17152140523444018,-0.16935514040651103,-0.1621522141856574,-0.15949399325126223,-0.15669468699310854,-0.15374601382995673,-0.15063909005236367,-0.1402697716713447,-0.13642694962155083,-0.13237031683074144,-0.12808600973280163,-0.12355903070020725,-0.10835253587315444,-0.10267783178188651,-0.09666383926917997,-0.1690154307368685,-0.16548119899755545,-0.11033053778467522,0.09052068921551112,0.01803158834179388,0.01803158834179388,0.6513263526286591,-0.22239839581183043,-0.18481314382545427,0.012050826905642374,0.036268046155524605,0.2193150861329479,-0.11424992591064455,-0.46430188667569194,-0.20179889231225878,-0.7044206622183066,-0.7100312400779075,0.24555242526759227,0.05967588107599302,-0.5271364627055349,-0.46038367030805644,0.2792603178828567,-0.16611965738963522,-0.2845734617631624,0.7749225600498861,-0.16464481246199647,-0.09514404918495449,-0.5091059979229511,-0.15063909005236364,-0.8295979570149006,0.586977814453559,-0.15063909005236364,-0.9009160299301671,-0.16464481246199647,-0.15063909005236364,-0.6026578278048279,-0.07110982663128396,-0.6319666318942355,-0.6902569259588438,-0.15063909005236367,-0.15063909005236367,-0.15063909005236367,0.6548075137083994,-0.15063909005236367,-0.2926631347407227,-0.11563282959003929,-0.15063909005236367,-0.35449096492770976,0.19221929737717788,-0.15063909005236367,-0.2926631347407227,-0.564222841552055,0.5477317794814032,-0.564222841552055,-0.07681012907059656,0.6691351699788968,-0.3739225959585437,-0.564222841552055,-0.564222841552055,0.573834251674276,0.6217780462617831,-0.564222841552055,-0.011004552881209212,-0.564222841552055,-0.564222841552055,-0.564222841552055,-0.564222841552055,-0.564222841552055,-0.3987494971033641,-0.3987494971033641,-0.3987494971033641,-0.3987494971033641,-0.3987494971033641,-0.3987494971033641,-0.3987494971033641,-0.3987494971033641,-0.2611854903944028,-0.3987494971033641,-0.3987494971033641,-0.3987494971033641,-0.3987494971033641,-0.8437576725567334,-0.40485708771149975,-0.41017145312333614,-0.41414141414141403,-0.41590019592802907,-0.34874291623145787,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,0.01803158834179388,0.01803158834179388,0.18201136161176915,-0.15063909005236364,0.036268046155524605,0.036268046155524605,-0.15063909005236364,-0.15063909005236364,0.3729272039836085,-0.47191562710342877,-0.15063909005236364,-0.15063909005236364,-0.3711537444790451,-0.15063909005236364,-0.15063909005236364,-0.5834571054543123,-0.15063909005236364,-0.15063909005236364,-0.15063909005236364,-0.15063909005236364,-0.15063909005236364,-0.15063909005236364,-0.15063909005236364,0.1530517828969796,-0.15063909005236364,-0.15063909005236367,-0.15063909005236367,-0.15063909005236367,-0.22711404219821452,-0.39279220242478624,-0.15063909005236367,-0.15063909005236367,-0.15063909005236367,-0.15063909005236367,-0.564222841552055,-0.564222841552055,-0.564222841552055,-0.564222841552055,-0.564222841552055,-0.564222841552055,-0.564222841552055,-0.564222841552055,-0.564222841552055,-0.564222841552055,-0.564222841552055,-0.564222841552055,-0.6731004175699443,-0.4113149557396053,-0.6596478404464505,-0.6652629493223531,-0.5319263604200568,-0.4227907247760231,-0.7117753084415308,-0.3172773867572252,-0.5383779909630537,-0.3683477394832712,-0.508305840444832,-0.3086612437595944,-0.6229498183720399,-0.16928770202300925,-0.49331350694752263,0.019162004466499898,0.0457391681174478,-0.7702453550720778,-0.8658282310992675,0.18373916537271767,-0.4893996593411651,-0.07114687632871237,-0.6178325403593193,0.0457391681174478,0.45245699913098797,-0.3225064561315765,-0.5706602123684298,-0.025730271519149854,-0.1353936931012144,-0.2673637061399449,0.49690975772282797,-0.1824513943052656,-0.11747791687562362,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.3855266522153873,0.30386330544433354,0.44518459494497387,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.7103331753761518,0.30386330544433354,0.2556526700717737,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.01429650977210236,0.30386330544433354,0.45071820854135963,0.30386330544433354,0.30386330544433354,0.30386330544433354,-0.325847008696705,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.7656738929113627,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.30386330544433354,0.46376142766027284,-0.3809470503610587,0.44518459494497387,0.30386330544433354,0.7335223519665467,0.42008402520840293,0.20431738720347908,0.0,0.0,0.0,0.0,-0.2524308329098654,-0.18638213806903792,-0.18407066100001546,-0.18146350099395317,-0.6259080288914842,-0.18340891347407398,-0.1672917362169404,-0.16256175514375495,-0.15722782271371094,-0.15120650191292473,-0.8391506692670068,-0.13669332648557458,-0.11800816042090448,-0.10667311465869792,-0.09371055382753174,-0.07883567247064116,-0.051435222711495296,-0.22156468376279892,-0.5466987108167995,-0.5307007495252439,-0.56236946948434,-0.4355531647676405,-0.4227907247760231,-0.3842496809105675,-0.31788386719839923,-0.47903480274707816,-0.17385718848961013,-0.1916516973954469,-0.1534962274924057,-0.023352354695650304,-0.1824513943052656,-0.22699238143814895,-0.06761893104033172,-0.06761893104033172,0.0457391681174478,-0.1824513943052656,0.0457391681174478,0.0457391681174478,0.0457391681174478,-0.1824513943052656,0.0457391681174478,0.0457391681174478,0.0457391681174478,0.0457391681174478,-0.1824513943052656,0.036268046155524605,0.07560687408872896,0.07560687408872896,0.07560687408872896,0.07560687408872896,-0.04532067100150997,0.036268046155524605,0.07475289757642294,0.07475289757642294,0.07475289757642294,-0.04532067100150997,-0.04532067100150997,0.0030054957209263433,-0.04532067100150997,-0.04532067100150997,-0.04532067100150997,-0.04532067100150997,-0.04532067100150997,-0.5072510935797163,-0.2581988897471611,0.4453337564336055,0.4453337564336055,0.7635419726212861,-0.9933434114252985,0.0,-0.4548791592483864,-0.04532067100150997,-0.04532067100150997,-0.04532067100150997,-0.04532067100150997,0.7066076503591069,-0.04532067100150997,-0.04532067100150997,-0.04532067100150997,-0.04532067100150997,-0.04532067100150997,0.6413087731013666,0.09021581570060534,-0.04532067100150997,-0.04532067100150997,-0.04532067100150997,-0.04532067100150997,0.669635945189395,0.18616443364228133,-0.041027468117019086,-0.03657370946562567,-0.183972902336745,-0.1824513943052656,0.09140894183510502,-0.17553801543684555,-0.1735805146530606,-0.17152140523444018,-0.16935514040651103,0.7238249114155114,-0.1621522141856574,-0.15949399325126223,-0.15669468699310854,-0.15374601382995673,-0.15063909005236367,0.29588976036065145,-0.47424073353145796,-0.13642694962155083,-0.13237031683074144,-0.12808600973280163,-0.12355903070020725,-0.564222841552055,-0.10835253587315444,-0.10267783178188651,-0.09666383926917997,-0.1690154307368685,-0.16548119899755545,-0.36941404124771415,-0.5466987108167995,-0.5307007495252439,-0.56236946948434,0.0,-0.4355531647676405,-0.4227907247760231,-0.3842496809105675,-0.31788386719839923,-0.47903480274707816,0.0,-0.1000886024224575,-0.1916516973954469,-0.1534962274924057,-0.023352354695650304,-0.1824513943052656,0.0,0.03861266135361052,-0.06761893104033172,-0.06761893104033172,0.0457391681174478,-0.1824513943052656,-0.06047215600018424,0.0457391681174478,0.0457391681174478,0.0457391681174478,-0.1824513943052656,0.0,0.0457391681174478,0.0457391681174478,0.0457391681174478,0.0457391681174478,-0.1824513943052656,0.30386330544433354,-0.25298342391870055,0.07560687408872896,0.07560687408872896,0.07560687408872896,-0.04532067100150997,0.30386330544433354,0.07475289757642294,0.07475289757642294,0.07475289757642294,-0.04532067100150997,-0.04532067100150997,0.15416880356223303,-0.04532067100150997,-0.04532067100150997,-0.04532067100150997,-0.04532067100150997,-0.04532067100150997,-0.2581988897471611,-0.2581988897471611,-0.04532067100150997,-0.04532067100150997,-0.04532067100150997,-0.04532067100150997,-0.04532067100150997,0.35640319640181223,-0.04532067100150997,-0.04532067100150997,-0.04532067100150997,-0.04532067100150997,-0.04532067100150997,0.32746003388037526,-0.04532067100150997,-0.04532067100150997,-0.04532067100150997,-0.04532067100150997,-0.04532067100150997,-0.17499719789788082,-0.04532067100150997,-0.041027468117019086,-0.03657370946562567,-0.183972902336745,-0.1824513943052656,0.35044088555411673,-0.17553801543684555,-0.1735805146530606,-0.17152140523444018,-0.16935514040651103,0.6546536707079772,-0.18638213806903792,-0.18407066100001546,-0.18146350099395317,-0.17852542043270053,-0.17521611247693536,0.21213011005766705,-0.16256175514375495,-0.15722782271371094,-0.15120650191292473,-0.1444000375739939,-0.13669332648557458,-0.12795011117184552,-0.10667311465869792,-0.09371055382753174,-0.07883567247064116,-0.22357372715928567,-0.22156468376279892,-0.6317328381028973,-0.6317328381028973,-0.6317328381028973,-0.6317328381028973,-0.6317328381028973,-0.6317328381028973,-0.6317328381028973,-0.6317328381028973,-0.6317328381028973,-0.6317328381028973,-0.6317328381028973,-0.6317328381028973,-0.6317328381028973,-0.6317328381028973,-0.6317328381028973,-0.6317328381028973,-0.6317328381028973,-0.6317328381028973,-0.6317328381028973,-0.6317328381028973,-0.6317328381028973,-0.6317328381028973,-0.6317328381028973,-0.6317328381028973,-0.6317328381028973,-0.6317328381028973,-0.6317328381028973,-0.6317328381028973,-0.6317328381028973,-0.6317328381028973,-0.6317328381028973,-0.6317328381028973,-0.6317328381028973,-0.3703984367781933,-0.5773502691896258,-0.5227957190733314,-0.6850291682041814,0.5246239405620043,0.8555788489060271,0.34525509593650283,-0.24543054723750118,-0.13118101904837842,-0.21849810548832826,0.8516432260899937,0.5059187617878242,-0.6802865778281049,-1.6488864471291284e-15,-1.6488864471291284e-15,-1.6488864471291284e-15,-1.6488864471291284e-15,-0.22888247036544407,-1.6488864471291284e-15,-1.6488864471291284e-15,-1.6488864471291284e-15,-1.6488864471291284e-15,-1.6488864471291284e-15,-1.6488864471291284e-15,-1.6488864471291284e-15,-1.6488864471291284e-15,-1.6488864471291284e-15,0.6029204140232494,0.41239304942116123,0.22618522947760833,0.0,-0.6791468092525431,0.8140957776597022,0.8591980509949275,0.07921121336586012,-0.415900195928029,-0.3987494971033641,-0.3987494971033641,-0.3987494971033641,0.7554978737975875,-0.913225195706057,0.4335219992717936,-0.3987494971033641,0.42512064641057246,-0.3987494971033641,-0.39874949710336427,-0.3664646332274489,0.18712814237528874,-0.3987494971033641,-0.40485708771149986,-0.3876990572676327,-0.9156678841264829,-0.3876990572676328,-0.34874291623145787,-0.2581988897471611,0.23398419466270645,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,0.7081114325781411,0.9201608746530634,-0.42426406871192845,-0.5791377313233398,-0.7797548251511839,-0.3137858162210945,-0.26726124191242445,-0.4100669189609455,-0.8389965506306178,-0.8647826800214309,0.0,0.2927700218845599,0.3871910943211306,-0.8660254037844386,0.0,-0.3987494971033641,-0.07100581900617511,-0.3987494971033641,0.39526506813260764,-0.008872032300814838,-0.3987494971033641,-0.3987494971033641,0.2031551598414122,-0.3987494971033641,-0.3987494971033641,0.8829568449678471,-0.3987494971033641,-0.3987494971033641,-0.3987494971033641,0.8591980509949275,-0.3987494971033641,-0.3987494971033641,0.8487146001182361,-0.7064207942381242,0.7565587547299412,-0.41590019592802907,0.4367767843584962,-0.34874291623145787,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.07032494875510756,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.3757345746510897,-0.27386127875258304,-0.18787577456684298,0.17742642836494768,0.18628935808392136,-0.1467771350840328,0.2523971152954124,0.41073322947561175,-0.07387527762382572,0.010702499710099725,-0.3987494971033641,0.4871986078927671,-0.06828924846593164,-0.3719387243003157,-0.37867269425938815,0.21617853424389524,-0.40485708771149986,-0.4101714531233361,0.41414141414141414,-0.41590019592802907,-0.41403933560541245,-0.34874291623145787,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.7716255537346433,-0.7672555064904834,0.4973937341375435,0.4050904595451127,0.13115097902172654,-0.011694481533811595,0.6331442977671515,0.5575118948580827,-0.6445900506591707,-0.015145550718808975,0.6487611796889349,0.2523172547063214,0.47011916326908876,0.5341989207133298,0.7214108386018933,0.7124599813352368,0.5239036015747474,0.6794781878120094,0.5222329678670935,0.4115912952716699,-0.22735850344927291,-1.8990053399109594e-15,-0.7392706673293937,0.29210430248181507,0.4188993663698834,0.24101181392740956,0.601335549761016,0.6970722414166489,0.5903402316085212,0.6209652636058696,0.7237319135777766,0.6091741958530595,0.365192764202453,0.38608734970730724,0.6023578517000759,0.7606237376645552,0.6714506558884277,0.27619469986728024,0.48193927452710617,0.57625854332257,0.45909856547905814,0.6287478829201151,0.8500501623122012,0.7982152258889608,0.6827763273368952,0.8219316105403108,0.7296794039658474,0.7143266763207261,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,0.5698412946808923,-0.2581988897471611,-0.2581988897471611,0.15403387690935835,-0.3397415643510531,0.5364957601076126,0.19585356586034472,0.5553953786938723,0.5966222915462555,0.511633748970903,0.5591285760244312,0.6218409366417251,0.7014494322192072,0.6655123173347547,0.85373880376977,0.2460073163630004,0.4362250810757991,0.3397194984698243,-0.25568872363709344,0.785016757996821,0.21576752076729797,0.6308700072177061,0.6665416721858395,0.6011376940165578,-0.20093842890501104,0.6339720823398132,-0.6741998624632419,0.5570945012794605,0.7161918492677288,0.34494340883946817,-0.14215061324096245,0.2980856643489816,-0.21046815357700846,0.5471099448012255,0.7149398999854754,0.6070822042445764,-0.36036794118334897,0.8834555763166566,0.47190677465184816,-0.34757271229249254,0.8432047463765684,0.7948170307929107,-0.6174502483729862,0.8262598025827652,0.8270900573327447,-0.12360786388213368,0.500064570966828,0.36990253506522575,-0.2902867181609514,-0.18856355053707796,-2.1067572693627353e-15,-0.3987494971033641,0.2771114349306469,-0.3987494971033641,0.32167087215378903,0.053851119743821695,-0.3987494971033641,-0.3987494971033641,0.28651856615944604,-0.3987494971033641,-0.3987494971033641,-0.3987494971033641,-0.3987494971033641,0.6815342877388249,-0.06262145796003474,-0.8724530429875448,-0.3987494971033641,-0.40485708771149975,0.8413556274043971,-0.2078398188625758,0.4834741572972518,0.5646969281788781,-0.3876990572676328,-0.34874291623145787,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.5590205815881214,0.38318248418230916,-0.6997831219459301,0.9871386834659432,0.663968757195036,0.7575058556945296,-0.16968285920445467,0.5222329678670935,0.14510913198686887,-0.39215876648529213,0.39299799625546794,-0.49766673211276996,-0.1824513943052656,0.7013480259960274,0.791348775882721,0.5222329678670935,-0.22529001406645166,0.583483743495018,0.7269126176509506,0.2667150269413303,0.522970315991872,0.2502288104901238,0.7075608521487483,0.8606629658238706,0.3377444994203227,0.652145648563052,0.30386330544433354,0.7075608521487483,0.30386330544433354,0.2502288104901238,0.2502288104901238,0.2502288104901238,0.7071067811865475,0.30386330544433354,0.2502288104901238,0.2502288104901238,0.6748843297383071,0.828937633574942,0.3377444994203227,0.2502288104901238,0.2502288104901238,0.4354023289937353,0.9819805060619655,0.3038633054443336,0.2502288104901238,0.8055923488706974,0.7308859758864101,0.44839487952283485,0.501280411827603,0.5329602720075294,0.6965950063642875,0.7905694150420949,0.5763881307414861,-0.2820843113945564,-0.1464739883301621,0.3644077854076786,-0.015213330186454464,-0.03779218723549438,0.013697231954607411,0.022198506204497863,-0.416404341752243,0.04105628503327913,0.07489776694980785,0.0879580235611868,0.5699943532181667,0.6164479937383854,0.014063499420977474,-0.7444521651555824,0.8983185203488657,0.340056789479878,0.14896597561474567,-0.1261990652982305,0.4368019095685179,0.7107876176851043,-0.14944195034215754,0.5644480751694719,-0.8354074805290808,0.5957898081318149,0.5726250021912261,0.8596526792110195,0.12238841642908221,0.5149087284839843,0.7193141733078063,0.6017147928212646,0.7296089189032144,-0.04132772432690941,0.5222329678670935,0.6976990939625164,0.4910749289193281,0.8440406535949496,-0.3014282288409797,0.7193141733078063,0.8038699606255385,0.5848130901389526,-0.42220831925134056,0.3141815902287707,-0.27330475522813474,-0.4220475174427087,-0.02233242975780076,-0.27850945371194413,-0.7257111546192215,0.5105163375202605,-0.768969120213557,-0.5004827630662619,0.31500784859025227,-0.5072510935797163,-0.7457272594939393,0.44414443031478223,-0.49002639016174465,-0.5061373993987192,-0.41015348342941926,-0.08421519210665189,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.07032494875510756,-0.865132133403576,-0.2581988897471611,0.5390552186484722,0.05441221662583772,0.3172974649522592,-0.6130085770917629,-0.8714843798304788,-0.3076491175834179,-0.5072510935797163,-0.6702511218497916,0.5422557926866342,-0.9026299009387185,-0.42552408740408554,0.34800636820982084,-0.5072510935797163,-0.5228635094419067,0.47943528411272945,-0.8977751021953126,0.11359785590850537,-0.114998144884092,-0.326628835329328,-0.7466645665621588,0.539930829409005,-0.8760831664208374,-0.5369766289866132,0.08567521171314105,0.6262571640389732,-0.6875847456422698,0.1555689470724524,-0.6259562147006503,0.49819830387622216,-0.09694565278339462,-0.4984568427529129,-0.6994587050829643,-0.07889237988105169,-0.9281513840324254,-0.8761204795963887,-0.7229637537504788,-0.7982191432764582,-0.19821659664428332,-0.6749647078741506,-0.3779073186713464,-0.35992061277708337,-0.822363778738718,-0.24492985541742804,-0.7390056712347041,-0.5916923564392493,-0.21975856137987265,0.3736345033684275,-0.3987494971033641,0.8591980509949275,-0.6848013510902594,0.8140957776597022,-0.3987494971033641,0.2323529175138685,-0.3987494971033641,-0.474828828307271,-0.3987494971033641,-0.3987494971033641,-0.3987494971033641,-0.31765344995077843,-0.3987494971033641,-0.40485708771149986,-0.404820452376368,-0.41590019592802907,0.7083488596509687,-0.34874291623145787,-0.2581988897471611,-0.2581988897471611,0.9146835659309641,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.7716255537346433,0.24955209741515963,-0.15063909005236364,-0.15500290506437245,-0.15063909005236364,-0.07326924375691923,-0.4954171502988514,-0.6155353210511683,0.0025359427516137593,-0.15063909005236364,0.02260933675776885,0.22931532401049767,-0.15063909005236364,0.5222329678670935,-0.1065718292942379,-0.15063909005236364,0.41817773764364746,0.5707617077029041,-0.5782045978774105,-0.2223983958118305,-0.49930405692637464,-0.5236559570312167,-0.15063909005236367,-0.6638079667350733,-0.7134747262078218,-0.564222841552055,0.2694817833602532,-0.564222841552055,-0.35355339059327373,-0.5642228415520548,-0.564222841552055,0.4143266503322231,-0.2882007063652172,0.47972495990150604,0.36670049941788,0.6443888937131195,0.2589352737716011,0.7853687937038254,0.38908565245369997,0.4833971260878605,0.34501125642064223,0.7897079595152389,0.6733223899252798,0.6070173069708492,0.8062717897069104,0.8438799592679641,0.9208437618529378,0.5222329678670935,0.714601978512659,0.3792485269688983,0.8823540779561784,0.32416452186921196,0.39762171477621133,0.8070079603155896,0.04377559361375008,0.7076447687329093,0.6746590281578205,0.8238340449830193,-1.6488864471291284e-15,-1.6488864471291284e-15,-1.6488864471291284e-15,-0.30145496096912144,-1.6488864471291284e-15,-1.6488864471291284e-15,-1.6488864471291284e-15,-1.6488864471291284e-15,-1.6488864471291284e-15,-1.6488864471291284e-15,-1.6488864471291284e-15,-0.30145496096912144,-0.6483793294347487,0.6158521717903326,-0.605963391687097,0.18551608887622295,-0.26951526428113926,0.6892920710671857,-0.13118101904837842,-0.13118101904837842,-1.6488864471291284e-15,-1.6488864471291284e-15,0.4968424356713299,-0.2791249638602976,-1.6488864471291284e-15,-1.6488864471291284e-15,-0.25067846108144604,-0.5307448924342754,-1.6488864471291284e-15,-1.6488864471291284e-15,-1.6488864471291284e-15,-0.8504318431867266,-0.30145496096912144,-1.6488864471291284e-15,0.6339586602717608,0.7812773630545633,0.01803158834179388,-0.026428147837914102,0.2367735581958848,0.747327177148012,0.5434362436759264,0.24762145585805195,0.12081305899142937,-0.31374224759685937,-0.14526429428035992,0.5212540765821773,0.34115268754983163,0.8506617533281674,0.23263582194388957,0.4299010386912012,0.03803243608724708,0.6183890514510807,-0.1377562920762257,0.7589067953723075,-0.15063909005236364,0.6060738648482267,-0.42135990972272597,0.38678247315782543,0.2824204042961989,0.796753287004886,-0.15063909005236364,0.04667940131643444,-0.14630677083050667,0.2857191288634955,-0.29148692212773336,0.774891903133432,-0.15063909005236364,0.09859971852594103,-0.13668548335073147,0.030157098823901433,-0.47389789276870703,0.48342926429394856,-0.15063909005236367,-0.29305060675831063,-0.7401431640989011,0.47243298532528216,-0.5145561467190507,0.6268844600756949,-0.15063909005236367,-0.573061807100708,-0.5550248347902534,0.5482685900384208,0.4338576293399021,0.26139784051252923,-0.564222841552055,-0.1558569283807099,-0.9040268657462739,-0.3419787423052318,-0.2579925496740655,-0.564222841552055,-0.42427287883232306,-0.8761978452936311,0.2105924830224224,-0.564222841552055,-0.564222841552055,-0.564222841552055,-0.8053254256812061,-0.5642228415520548,-0.6568101977692655,-0.5466987108167995,-0.6283259110337815,-0.6312289148179844,-0.0316767002695938,0.0,-0.4355531647676405,-0.4227907247760231,-0.3842496809105675,-0.5403324311778315,-0.5618521990360875,0.68022820087799,-0.7026714655399282,-0.8009936832301233,-0.1916516973954469,-0.1534962274924057,-0.09002966423792595,-0.4239296070052509,0.21026463239214319,0.17200522903844542,-0.18149040452068674,-0.06761893104033172,-0.06761893104033172,-0.6172944986611199,0.25022025330437336,0.8350832285920956,0.0,0.6139578169779777,0.2565316988366018,0.0457391681174478,0.0457391681174478,0.08762232938816727,0.46843589283226167,-0.1032031374230672,0.0457391681174478,0.0457391681174478,0.0457391681174478,0.0457391681174478,-0.1824513943052656,0.7693181999980983,0.36904306676211357,0.04354168742100644,0.07560687408872896,0.07560687408872896,0.07560687408872896,-0.10904469555765439,0.6230385955955511,0.22238054992291514,0.46341553592640256,0.07475289757642294,0.07475289757642294,-0.042465998245646126,0.5692463466357883,0.6334771854685621,0.4611589447600393,0.27259140133737453,-0.04532067100150997,-0.11275782589407786,-0.05136292892194579,0.11677688354641598,0.25819888974716115,-0.38549100861643393,-0.5072510935797163,-0.8193443487841316,-0.2581988897471611,0.09432627821624366,-0.329029351193377,-0.04532067100150997,-0.04532067100150997,0.588659365543387,0.92376531145061,0.7771375663994621,0.56795564258852,-0.6300564010253532,-0.3972956326651855,-0.0972766731190187,0.588659365543387,0.9139944031160441,0.6213917785298246,0.5379927583877,-0.5559022666292697,-0.04532067100150997,-0.04532067100150997,0.5974044848923618,0.8663081503853913,0.8498298497448412,0.538596027927386,-0.041027468117019086,-0.03657370946562567,-0.013795172973955893,0.49155826808739495,0.8301110911672399,0.4456483213292151,0.2177393714851163,-0.15872989052623873,-0.1735805146530606,-0.22017513027005897,-0.1434858753659309,0.9369477934407328,0.3282866720214143,-0.15344352612569975,-0.15949399325126223,-0.15669468699310854,-0.45155206128931863,-0.09412940011030059,0.7413224872833075,0.9044056564777085,-0.1598605362120501,-0.13642694962155083,-0.13237031683074144,-0.12808600973280163,-0.12355903070020725,0.7068415644697962,0.9034521780638287,-0.10835253587315444,-0.10267783178188651,-0.09666383926917997,-0.32650446894908025,-0.16548119899755545,0.03423941039258186,-0.25888132381726264,-0.08553196273979308,-0.7254271197463411,-0.5619705742858367,-0.02418241785501486,-0.5140281307633972,-0.6777350174446887,-0.6255416207652533,-0.7819981760942141,0.0,0.4859750672796192,-0.23209723229908719,-0.5857520434819997,-0.6203440181879718,-0.6082930467052517,0.716240576974811,-0.6021426117788576,-0.48065360895205095,-0.7549329833589107,-0.15063909005236376,-0.40516941810221957,-0.7625257546938122,-0.2245199225273262,-0.6824717451999959,-0.25310026848665,-0.2603873520025828,-0.15063909005236364,-0.15063909005236364,-0.7110984327033617,0.5880943941101258,-0.2401268604420312,-0.3847646606402961,-0.8075149854393946,0.5551153191723827,-0.024454174725810924,-0.2336793620205963,-0.15063909005236367,-0.7972199925838608,0.6315986696181949,-0.46723379817479394,-0.564222841552055,-0.564222841552055,-0.6673884419690734,-0.5071546333284233,-0.564222841552055,-0.564222841552055,0.17303826094377522,-0.564222841552055,-0.564222841552055,-0.564222841552055,0.7032739068950907,-0.5642228415520548,-0.5642228415520548,0.2041241452319315,-0.18787577456684298,0.13852544119842905,0.6868142016374293,0.568406633034678,-0.11032749348962717,-0.08853906473527291,0.2217098149398123,0.265892842748683,0.494488643822704,0.4603509467717908,0.2964998866308431,-0.004913025817426305,-0.14172930336233677,0.243179358492599,0.5967331652904216,0.07519969658043711,0.6423868245452719,-0.40485708771149975,-0.41017145312333614,-0.4141414141414141,-0.712322537660549,0.3772198973601749,-0.1644898799497595,-0.2581988897471611,-0.7715123348235795,-0.8763769335624741,0.7226824402144922,-0.2581988897471611,-0.2581988897471611,0.5698412946808923,-0.2581988897471611,-0.2581988897471611,-0.18787577456684298,-0.17568187354729867,-0.1620492291825904,0.3421631113363127,-0.12962957161691033,-0.11032749348962717,-0.08853906473527291,-0.06386718472761346,-0.035833399207393445,-0.0038569284319363998,-0.16006115626305584,0.1266334116369259,-0.04184999018767307,-0.004913025817426305,0.03853996718248504,-0.06893529476616927,-0.20604670093634578,0.47294226126806316,-0.33636767331766587,-0.42677340239846456,-0.40485708771149975,-0.41017145312333614,0.3256485349778145,0.5839684897976798,-0.41403933560541245,0.6815254919711036,-0.3876990572676327,-0.34874291623145787,-0.2581988897471611,-0.2581988897471611,0.903839307030387,-0.2581988897471611,0.5698412946808923,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.8596486697854687,-0.7459636472819602,-0.1620492291825904,-0.8257736991320194,-0.7426269237234371,-0.5149424971527916,0.4154649520325889,-0.059052215730911024,-0.37867269425938815,0.4954982240717923,-0.5578592085389683,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,0.0,0.03240775471459592,-0.42426406871192845,-0.4121120667911454,-0.3137858162210945,-0.26726124191242445,-0.20483662259967564,-0.11952286093343938,0.0,0.0,0.2927700218845599,0.8660254037844386,0.8944271909999159,0.8660254037844387,-0.442608342456849,-0.8198229763613265,-0.4821631217942352,0.3784115038845798,-0.7465124695134071,-0.42743806203754625,-0.3336109271593779,-0.2952633738999096,-0.3519000001423036,0.17235919596042173,0.8320502943378437,0.034034578581766416,-0.21277410449989748,-0.171844324519765,-0.4571706767282042,-0.4890710240290042,-0.4396042110223836,0.7615333941252301,-0.2614362368583126,-0.47097687366602276,-0.008172195485894611,-0.5350029196640769,-0.5024896790993076,-0.7215641043503535,0.17200522903844542,0.2089854644770769,-0.5335832557745382,-0.398580829785619,0.07560687408872896,-0.04532067100150997,0.3544587784792833,-0.21047585500020233,0.07560687408872896,0.07560687408872896,0.07560687408872896,0.20664711151492038,0.3516645634875688,-0.7271197406771477,0.07560687408872896,0.07560687408872896,0.07560687408872896,0.07560687408872896,-0.3655593990684132,-0.6892806640866397,0.07475289757642294,0.07475289757642294,0.07475289757642294,-0.3374330138441254,-0.4444955785386715,-0.0027095128927159128,-0.24828512438374598,-0.04532067100150997,-0.15775631660596676,0.8333870890922567,-0.44206666154682606,-0.2581988897471611,-0.7079366633601034,-0.04532067100150997,-0.04532067100150997,-0.04532067100150997,-0.4862732097915381,-0.04532067100150997,-0.4622420024478049,-0.16298200431634047,-0.04532067100150997,-0.10688400968695085,-0.04532067100150997,-0.04532067100150997,-0.9405949911265029,-0.04532067100150997,-0.04532067100150997,-0.04532067100150997,-0.26261953941767774,-0.04532067100150997,-0.6143542625236988,-0.04532067100150997,-0.041027468117019086,-0.03657370946562567,-0.21540928290830444,-0.1824513943052656,-0.5738746732015769,-0.17739911193056052,-0.17553801543684555,-0.1735805146530606,-0.34673032694970046,-0.3763290009606122,-0.5196943374038432,-0.1621522141856574,-0.15949399325126223,-0.15669468699310854,-0.15374601382995673,-0.5389607446137491,-0.3075731870721606,-0.1402697716713447,-0.13642694962155083,-0.13237031683074144,-0.6507576223541368,-0.12355903070020725,-0.3004418803919251,-0.10835253587315444,-0.10267783178188651,-0.09666383926917997,-0.1690154307368685,-0.16548119899755545,-0.18787577456684298,-0.38736320904863286,-0.1620492291825904,0.7623910503908392,-0.801838613099022,-0.11032749348962717,-0.08853906473527291,-0.06386718472761346,-0.035833399207393445,-0.0038569284319363998,0.4242374439204022,0.5136313060024328,-0.04184999018767307,-0.004913025817426305,0.03853996718248504,-0.06893529476616927,-0.47920824159988507,0.8562276291406854,-0.3922063624015569,-0.3987494971033641,-0.40485708771149975,-0.41017145312333614,0.47895105803143767,-0.5455722965095927,0.516974073860403,0.6580318658402194,0.8066842939482289,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,0.9426944006292399,-0.2581988897471611,-0.6737734200041539,-0.753917797514395,-0.5409710907553794,0.0,-0.6146373833706825,-0.2795800355712993,0.27392684979588633,-0.3196210057646308,0.0,0.19042185317450175,0.026653191911606564,0.5388817012906054,0.8772867845880139,0.5946201808703536,-0.17385744850052975,0.8599539773243247,0.8891238008942043,0.3309771685027867,0.6722641114227199,-0.05063696835418333,-0.15063909005236364,0.8495819717571138,0.8222420036312564,-0.2402881185471998,0.5630481376626479,0.0,0.7166633448553961,0.8474905612661254,0.70142736390844,0.6551199045266707,0.1838619773144781,0.5764502649186667,0.6597106246187974,0.484314417407003,0.7630283320484907,0.1838619773144781,0.7017867444945798,0.46085947891289886,0.7836901425027949,0.6788879687249699,-0.05791770190499765,0.7663518741405793,0.44349979474004414,0.6522283262197974,0.8477821721476698,-0.2581988897471611,-0.2581988897471611,-0.8710685902987654,0.874165538557734,0.4453337564336055,0.9903830503194893,-0.2581988897471611,0.6767290149627004,0.25551254686905794,0.8567112454824038,0.9231376262092561,0.049072117791499774,0.5117229881586742,0.6300206260321388,0.678814739764447,0.9067006486958774,0.1838619773144781,0.15627235674086848,0.6732362706188925,0.7341918288248175,0.7402366355281951,-0.772070596279192,0.2892228545398971,0.4610371961669787,0.6890302506952243,0.7259851326011755,-0.1446026502889495,0.39134109598806627,0.34255523222174783,0.09074258777368031,0.7019238386042318,-0.026615456534959992,0.5124656893698858,0.1692865433947731,0.5211033392089153,0.790813632214211,-0.17148931456966424,0.8398807670719817,0.5085579988090015,0.20973752100374848,0.671854862672283,-0.12795011117184552,0.271404203391708,0.49462778375239064,0.4741374879537219,0.8330870054150854,-0.8685419763757457,-0.6680705001742802,-0.3329399086544245,0.4242374439204022,0.4202497679913006,-0.3137543882717235,-0.7750939332025649,0.710693926059558,0.7758261549255033,-0.46407256372954386,-0.4591681342726519,0.718856362587474,-0.05718807560223826,0.4048911846337288,-0.11203185094095915,0.6375094980953537,0.38378769343293606,0.5489506493051143,-0.34874291623145787,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,0.7226824402144922,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.18787577456684298,-0.3656499324906277,0.5600217679103059,-0.1467771350840328,0.3961187316184272,-0.11032749348962717,-0.08853906473527291,0.5209991273981309,-0.5740287154022771,0.6211943982194768,-0.10059408357279488,-0.3202216893178903,0.45018597309486624,0.6175412411198894,-0.06828924846593164,-0.5128776445321725,-0.43641023062001394,-0.3854624923680711,-0.7375953153323836,-0.3987494971033641,0.8487146001182361,-0.41017145312333614,-0.535225406245433,-0.550774676521739,-0.41403933560541245,-0.3876990572676327,-0.8241087771506503,-0.07032494875510756,-0.2581988897471611,-0.27344740981088894,0.903839307030387,-0.020852348049245767,-0.2581988897471611,-0.07032494875510756,-0.2581988897471611,-0.18898223650461354,-0.2581988897471611,-0.8803756547934263,-0.16903085094570333,0.027137128654616198,-0.3938483785193999,-0.6535354975593528,0.13093073414159545,-0.15063909005236364,-0.41185384376226924,-0.849610052504186,-0.7400862007022051,-0.15063909005236364,-0.38532661414902675,-0.6913356328646345,-0.6941782753825193,-0.8043418986946769,-0.15063909005236364,-0.762070844012362,-0.6919445461315075,-0.4367817020489443,-0.3997032022061933,-0.15063909005236364,-0.3105902984519937,-0.48433815682818754,-0.5599069933360823,-0.48806037591001583,-0.4368178108850521,-0.5854579266959506,-0.5174830012225182,-0.6656540428517349,-0.8143104767703171,-0.15063909005236364,-0.41742249996392083,-0.7153890121985859,-0.16136155311749137,-0.39547758343326134,-0.2920833627064374,-0.5550769332020079,-0.4887559999845143,0.4672074777739124,-0.683421225275589,-0.5642228415520548,-0.8694681182333903,0.0,-0.564222841552055,-0.6501794664265751,-0.7225966793272202,0.828078671210825,-0.564222841552055,-0.564222841552055,-0.28566129435411175,-0.7480586188374232,-0.8053254256812061,-0.564222841552055,0.9030828858565617,0.0,0.0,-0.17104041835875172,-0.03221721896727008,0.769120486471833,-0.1754291706042626,-0.3931125429964079,-0.08853906473527291,-0.32916538612635354,0.6630274032406385,0.4242374439204022,-0.7695360628788057,-0.521191555636046,0.5349421229899514,0.596367208727514,-0.01594284156158165,-0.3987494971033641,-0.7397577403040587,-0.41017145312333614,-0.3876990572676328,0.43641380970896815,0.7710843782295296,-0.7696863397588491,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,0.5698412946808923,-0.2581988897471611,-0.2581988897471611,-0.7110596209919329,0.3302453849407488,-0.37827964816247317,0.12255389718076279,-0.7750939332025649,0.18819395327583233,-0.41583474180443625,-0.09715613106440363,-0.3653266821880202,0.2307856318228483,-0.08704393197144066,-0.40485708771149975,-0.4101714531233361,-0.5589132542862637,-0.3876990572676328,-0.34874291623145787,-0.2581988897471611,-0.8720797023735438,-0.8222959329087097,-0.2581988897471611,0.7081114325781411,-0.3987494971033641,-0.3987494971033641,-0.3987494971033641,-0.3987494971033641,-0.3987494971033641,-0.3987494971033641,-0.3987494971033641,0.48506109133897046,-0.3987494971033641,-0.3987494971033641,-0.3987494971033641,-0.3987494971033641,0.8562276291406854,-0.40485708771149975,-0.41017145312333614,-0.815235097274701,0.35094320882354124,-0.8241087771506503,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.5466987108167995,-0.8248876302346865,-0.6917511298674051,0.0,-0.7180384534993317,-0.631327552540518,-0.3842496809105675,-0.5050855085592779,-0.8432578433465827,-0.7334778544465346,-0.7377142249978557,-0.8100822143462597,0.29436719748174606,0.3359045662819291,0.5728918992315463,-0.1915794823137587,0.12042598301813655,0.5424512789811494,-0.3178254165988825,0.632180807661947,0.0,-0.6527941694582028,-0.6936204298713656,-0.77712430415979,-0.7721495302933901,0.7452413135250995,-0.663069992223704,0.0457391681174478,-0.4268606007115976,-0.025499723235443084,-0.042094639416775465,-0.7516149441800198,0.01803158834179388,0.01803158834179388,-0.130312384114833,-0.6790734041321197,-0.19756849679351882,0.036268046155524605,0.036268046155524605,-0.7179855804536036,-0.4877283867187915,-0.21637561222586763,-0.21676777717282952,-0.15063909005236364,0.6180133826919204,-0.2542559541052901,-0.7665924603863328,-0.15063909005236364,-0.15063909005236364,-0.674941873262412,0.4875060749904314,-0.5017422496035454,-0.15063909005236364,-0.1803830565678353,0.5476149271517711,-0.5568031476505497,0.10925517106309035,-0.15063909005236364,-0.15063909005236364,-0.15063909005236364,-0.058401601468353895,0.023841232870642215,-0.15063909005236367,-0.15063909005236367,-0.104769702396667,0.28937638184208425,0.023841232870642215,-0.15063909005236367,-0.15063909005236367,-0.027946847103778167,-0.3798842055136336,-0.564222841552055,-0.564222841552055,-0.564222841552055,0.8629498402259554,-0.564222841552055,-0.564222841552055,-0.564222841552055,-0.564222841552055,-0.564222841552055,-0.564222841552055,-0.564222841552055,-0.5642228415520548,-0.5477711792281285,-0.5466987108167995,-0.5307007495252439,-0.56236946948434,0.36199846614261094,-0.4227907247760231,-0.3663615743803931,-0.3740609214641422,-0.47903480274707816,0.6846804692688995,-0.1916516973954469,0.22467993440390688,-0.023352354695650304,-0.1824513943052656,0.7062826063813481,-0.06761893104033172,-0.06761893104033172,0.0457391681174478,-0.1824513943052656,-0.2595468934114277,0.0457391681174478,0.0457391681174478,-0.5338444179977011,0.6966740375450763,0.0457391681174478,0.0457391681174478,0.0457391681174478,-0.1824513943052656,-0.583045906199188,0.40654955243026003,0.01803158834179388,0.01803158834179388,0.01803158834179388,-0.15063909005236364,0.4136433828383639,0.036268046155524605,0.036268046155524605,-0.15063909005236364,-0.15063909005236364,0.5363523059946655,-0.15063909005236364,-0.15063909005236364,-0.15063909005236364,-0.15063909005236364,-0.4472135954999579,0.15323470644371398,-0.15063909005236364,-0.15063909005236364,-0.15063909005236364,-0.15063909005236364,-0.09630858168336641,-0.15063909005236364,-0.15063909005236364,-0.15063909005236364,-0.15063909005236364,0.3579429191613678,-0.15063909005236364,-0.15063909005236364,-0.15063909005236364,-0.15063909005236364,0.652636879895881,-0.15063909005236367,-0.15063909005236367,-0.15063909005236367,-0.15063909005236367,-0.138835501228536,0.2480135438450324,-0.15063909005236367,-0.15063909005236367,-0.15063909005236367,-0.28652902182663254,-0.564222841552055,-0.564222841552055,-0.564222841552055,-0.564222841552055,-0.7617914500821986,-0.564222841552055,-0.564222841552055,-0.564222841552055,-0.564222841552055,-0.564222841552055,-0.564222841552055,-0.564222841552055,-0.564222841552055,-0.564222841552055,3.421571735935038e-15,0.7075608521487483,0.7190465476314597,0.30386330544433354,-0.21976393602944547,0.7190465476314597,0.1824133877646348,0.49999999999999994,0.4561393005500253,0.0,-0.14141007932539418,0.8710207247439574,0.1690926064364921,0.46635980958929757,0.37711647092898554,0.886212293047897,0.30386330544433354,0.6766033179215207,0.2502288104901238,0.9061193778576649,0.1959618531981683,0.25167095976695175,0.2526597885285856,0.7706434991063845,0.8320502943378437,0.7216664274528833,0.0,-0.08247860988423225,0.16611132709585258,-0.5035630959991002,-0.4037770037903547,-0.015875480378819494,-0.9895372723259246,-0.5570228518038539,-0.4341251102705609,0.04105628503327913,-0.6603522818505452,-0.09064855835615018,-0.6010716830876792,0.014063499420977474,0.6813212506895922,0.14022642776386085,0.47664451119804807,-0.13118101904837842,-0.8415359810247868,-0.8604377964200752,0.028471133116259345,-0.5466987108167995,-0.6097263294926278,-0.75877568155558,0.0,0.0,-0.8006741036340659,-0.4227907247760231,-0.3842496809105675,-0.6509006113932522,-0.23981834500188004,0.0,0.41964978891536997,-0.4485720974274017,-0.7785490689972555,-0.2141653272374701,-0.168686621751501,-0.4450922764749255,0.3205965611347919,0.8320502943378437,-0.7080699372957274,-0.2647443788557585,-0.2597702018360601,-0.9095587747690996,-0.8941526037586578,-0.4731077790531205,0.0,0.8811527384026501,-0.6433008088698238,-0.7474323421104943,0.0457391681174478,-0.21256387890716602,0.0,0.7061878636037996,-0.3793565366656993,-0.23407077566040038,0.0457391681174478,-0.2337410818018806,-0.4986434648851077,0.25578147177404803,0.13647340712943126,0.07560687408872896,0.07560687408872896,0.07560687408872896,0.07560687408872896,-0.04532067100150997,-0.7126181830193375,-0.7408511360097544,0.07475289757642294,0.07475289757642294,0.07475289757642294,-0.6724257827157909,-0.2643540227227055,-0.6822040128734762,0.7173117486839621,-0.26319835105636247,-0.04532067100150997,0.6450975003948097,0.6244678122595303,-0.6848689257921343,0.7844166311294495,0.24911798420996084,0.5797205153308971,0.8518749815237426,0.2629693575231776,0.6546576366603885,0.6327411193168272,-0.7693376840149656,0.12511080380963047,-0.27824679204673974,-0.15930318246443556,-0.04532067100150997,0.8375511250172961,-0.12058784595685669,-0.6022457346986599,0.21215297571457103,-0.26738951169773945,-0.04532067100150997,-0.04532067100150997,-0.6610340950836681,-0.04532067100150997,-0.15991676274480182,0.1747699891144679,-0.04532067100150997,-0.04532067100150997,-0.04532067100150997,-0.14529832903171372,-0.07353757157526483,-0.26779253691085825,0.2837531835075372,-0.04532067100150997,-0.041027468117019086,-0.03657370946562567,-0.7453172462802662,-0.1824513943052656,0.09302595548255979,-0.7361857376945588,-0.17739911193056052,-0.17553801543684555,-0.1735805146530606,-0.353276963377108,-0.16935514040651103,0.08696088268923105,-0.7405437077334588,-0.1621522141856574,-0.15949399325126223,-0.15669468699310854,-0.15374601382995673,-0.556093191961947,-0.8775643415391566,-0.6873041341018947,-0.1402697716713447,-0.13642694962155083,-0.13237031683074144,-0.12808600973280163,-0.12355903070020725,0.6006696256834592,-0.8461587134693831,-0.10835253587315444,-0.10267783178188651,-0.09666383926917997,-0.1690154307368685,-0.16548119899755545,-0.4806428715799506,-0.5466987108167995,-0.5999253644590726,-0.6921906609239882,0.3544587784792833,-0.23749692644784484,-0.4227907247760231,-0.3842496809105675,-0.33081573600328396,-0.445280789228531,0.5222329678670937,-0.4352499555044999,-0.1916516973954469,0.22467993440390688,-0.023352354695650304,-0.5306842146382081,0.39380737297351964,-0.018889023101953457,-0.06761893104033172,-0.06761893104033172,-0.08753907524237553,-0.1824513943052656,0.5222329678670935,0.0457391681174478,0.0457391681174478,-0.3948210035602599,-0.1824513943052656,0.17200522903844542,-0.6431029749273197,0.0457391681174478,0.0457391681174478,0.0457391681174478,-0.1824513943052656,-0.0697754884490583,-0.38234222558016173,0.07560687408872896,0.12264084755539702,0.07560687408872896,-0.04532067100150997,0.6780646876525098,-0.7241348666086298,0.07475289757642294,0.24132574003418722,-0.04532067100150997,-0.04532067100150997,0.35008023879807415,-0.7058821545991929,-0.04532067100150997,-0.04532067100150997,-0.0472082055927109,-0.3951944356153767,-0.07032494875510756,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,0.85264712829343,-0.7696185628818999,-0.04532067100150997,-0.04532067100150997,-0.04532067100150997,-0.04532067100150997,-0.8392545940788332,-0.04532067100150997,-0.04532067100150997,-0.04532067100150997,-0.04532067100150997,0.15863520778762777,-0.8544189688055011,-0.04532067100150997,-0.04532067100150997,-0.04532067100150997,-0.04532067100150997,0.5320803295613357,-0.7801147434403706,-0.041027468117019086,-0.03657370946562567,-0.183972902336745,-0.1824513943052656,-0.5983952941395071,-0.17553801543684555,-0.1735805146530606,-0.3254880302705976,-0.16935514040651103,0.21194160877150944,-0.5499510996113715,-0.15949399325126223,-0.15669468699310854,-0.15374601382995673,-0.15063909005236367,0.2719788551496517,-0.3639798454739524,-0.13642694962155083,-0.13237031683074144,-0.12808600973280163,-0.12355903070020725,0.1762829084622232,0.6458135952776821,-0.10267783178188651,-0.09666383926917997,-0.1690154307368685,-0.16548119899755545,-0.5466987108167995,-0.5478521948343542,-0.6075149901556964,-0.7710842257338746,-0.4836720335393004,-0.3842496809105675,-0.22317702682434637,0.14725756298936815,-0.17385718848961013,-0.42907555977656336,-0.2819546901111011,-0.6451014439416874,-0.3597075191774593,-0.5733912930557157,-0.06761893104033172,-0.8148657636619226,-0.5561999741126811,-0.6585240843210676,0.0,-0.8627534409119868,-0.43777916307347287,-0.2493064016310529,-0.1824513943052656,-0.32597662905492925,0.0457391681174478,0.0457391681174478,0.10770675778546376,-0.19796122033901226,-0.19247881789883542,0.4706773781317832,0.07560687408872896,0.07560687408872896,0.07560687408872896,0.07560687408872896,-0.04532067100150997,0.56663232047707,0.28926790319069856,0.07475289757642294,0.07475289757642294,-0.2936617185640397,-0.4936568770109405,-0.2504374632583523,0.2502288104901238,-0.709572221549228,-0.2731117967741293,-0.04532067100150997,-0.5479405805860407,0.02743256796638764,-0.6914805881185937,-0.2581988897471611,0.9150513403586291,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,0.1682812647646685,0.11340367387360953,-0.6574418304223759,-0.04532067100150997,0.132027809607268,0.07688306386615501,-0.31237523902522385,0.3957342244624319,0.1109327823921727,-0.04532067100150997,-0.04532067100150997,-0.04532067100150997,-0.21700593337422175,-0.04532067100150997,0.14853822876198824,-0.3882580077612804,-0.04532067100150997,-0.04532067100150997,-0.04532067100150997,-0.22596974744757503,0.10865629037595878,0.4947380347329367,-0.7794265857317059,0.04486842099255871,-0.041027468117019086,-0.03657370946562567,-0.21861261491445388,-0.6316740310991649,0.7301936309941032,-0.31827047940799835,-0.007969437178795674,-0.17553801543684555,-0.1735805146530606,-0.32328455936721495,-0.16935514040651103,-0.5145017780069082,-0.4280703166570356,-0.15949399325126223,-0.15669468699310854,-0.15374601382995673,-0.26045702506797064,0.6822059011087515,-0.6933352431860277,-0.1402697716713447,-0.13642694962155083,-0.13237031683074144,-0.12808600973280163,-0.12355903070020725,-0.7430870277708619,-0.4950649989288269,-0.10835253587315444,-0.10267783178188651,-0.09666383926917997,-0.1690154307368685,-0.16548119899755545,-0.5466987108167995,-0.7146661743435729,0.60781925352533,-0.5593279433887912,-0.4227907247760231,-0.3842496809105675,-0.7651166704691484,-0.75391998622962,-0.5750613372519754,-0.1916516973954469,-0.20800215028779115,-0.023352354695650304,-0.529716807221482,0.40901247593432033,-0.06761893104033172,-0.09084276890016636,0.6242363456151926,-0.1824513943052656,0.6090746133187086,0.0457391681174478,-0.9544985272483226,0.9493121854813257,-0.07129728514467064,0.0457391681174478,0.0457391681174478,0.0457391681174478,-0.1824513943052656,-0.6365045164511142,0.2284843233117569,0.07560687408872896,0.07560687408872896,0.07560687408872896,-0.04532067100150997,0.39607289825658226,-0.48911468696623955,0.07475289757642294,0.07475289757642294,-0.5343633173904709,-0.3516935722760543,0.45782795140959465,-0.5137004215982655,-0.04532067100150997,-0.04532067100150997,0.343996978539516,-0.04532067100150997,-0.5072510935797163,0.5698612547964268,0.8371416255051412,-0.04532067100150997,-0.04532067100150997,-0.04532067100150997,-0.04532067100150997,-0.5220213925611141,-0.5477267744286147,-0.04532067100150997,-0.04532067100150997,-0.04532067100150997,-0.04532067100150997,-0.7080774510983578,-0.252295309648844,-0.18520019122612583,-0.04532067100150997,-0.04532067100150997,-0.15065098684084233,-0.7074898528267644,-0.5565047784041668,-0.041027468117019086,-0.03657370946562567,-0.183972902336745,-0.1824513943052656,-0.5278880309292224,-0.6632789308854609,-0.17553801543684555,-0.1735805146530606,-0.17152140523444018,-0.16935514040651103,-0.5501323557038352,-0.2666463648779354,-0.15949399325126223,-0.15669468699310854,-0.09070252104636524,-0.15063909005236367,-0.7958557154585915,-0.136161182865694,-0.13642694962155083,-0.13237031683074144,-0.12808600973280163,-0.12355903070020725,-0.4657959413003346,-0.10835253587315444,-0.10267783178188651,-0.09666383926917997,-0.1690154307368685,-0.16548119899755545,-0.5466987108167995,-0.6169885841876632,-0.6169598929983681,0.9989407374922168,0.3720474371046043,-0.4227907247760231,-0.3842496809105675,-0.14845910830382567,-0.5473011752278525,-0.17385718848961013,-0.5463399698860992,-0.2057384166802774,-0.2213042840941946,0.3992940551115348,0.8475968446231935,-0.2612482801611256,-0.06761893104033172,-0.2090892486899233,0.41260617352360324,-0.7788181317984112,-0.17249761888431278,-0.7462559908452336,-0.7643054471190905,-0.487264248558565,-0.40153969809677054,-0.1824513943052656,-0.3534464415090668,0.0457391681174478,-0.5375802622705922,-0.4939471265533115,-0.5390393407146907,-0.44435125325305974,-0.005660138016737811,0.07560687408872896,0.07560687408872896,0.07560687408872896,0.07560687408872896,-0.16465828453125486,0.057895070088068844,-0.5481343090655801,0.07475289757642294,0.07475289757642294,-0.24180525123745142,-0.12823377989058493,-0.4293706197831528,0.8567325747004054,-0.5543614193378522,-0.4293706197831528,-0.04532067100150997,-0.04532067100150997,-0.34140479413396435,-0.32105573273668675,0.5878803366822822,-0.6540542679642917,0.6375548612068146,0.9085847258363428,0.8609170164817194,0.5235509837747283,0.38627209446077226,0.7068951579965577,-0.4895999011010312,-0.6118684854635225,-0.050173612722461444,-0.4293706197831528,-0.8094960941184981,-0.6084142006128967,0.790384480986475,-0.2179837262286871,-0.04532067100150997,-0.5229524701354389,-0.04532067100150997,-0.8800167301729297,-0.3209478230033582,-0.018796726860848895,-0.25954630326750106,-0.30628311240914147,-0.04532067100150997,-0.04532067100150997,-0.5846176826567293,-0.19601745973884188,0.7017017395912717,-0.2576438312894311,-0.17133407937962555,-0.041027468117019086,-0.03657370946562567,-0.20767651813617216,-0.09819704172698053,0.6957161151206291,-0.32231196124534617,-0.33558235756918214,-0.17553801543684555,-0.1735805146530606,-0.17152140523444018,-0.29115685109214173,0.22144217094576038,-0.659166691954621,-0.1621522141856574,-0.15949399325126223,-0.45515739024060686,-0.15374601382995673,-0.14935841877197628,0.1779388986124473,-0.7791871530702177,-0.1402697716713447,-0.13642694962155083,-0.13237031683074144,-0.12808600973280163,-0.12355903070020725,0.5942552246053641,-0.4708865117418375,-0.10835253587315444,-0.10267783178188651,-0.09666383926917997,-0.7429426782844377,-0.18598722405991702,0.4091189608212065,0.16327125868596967,-0.7724916398777808,0.48399919858711193,-0.1444334049043461,-0.5755750188420583,0.3103033669450399,0.3857982968764932,-0.7144971717474636,0.0687961974323725,-0.3987494971033641,0.6816825453913443,0.541356666444039,0.5751454489375772,-0.023352354695650304,0.5297245260450095,0.32885684586812586,0.9140291110389854,0.35540980536666544,0.5240526833845974,-0.059235962060811866,0.9257186893153806,0.7615208020592567,0.12192238473477518,0.3645634563248907,-0.07692660025955747,0.7401496995961472,0.5676581505158352,0.3951070455497896,0.36054364583609744,0.3643503571610302,0.0457391681174478,0.3360619377683435,-0.40522484348440524,-0.6812020307478813,0.07560687408872896,0.26983981813998026,0.07560687408872896,0.46456250116419134,0.49692332470673783,-0.2806877222934817,0.07475289757642294,0.07475289757642294,-0.04532067100150997,0.6625455147716958,-0.20866829850675722,-0.3482195089361727,-0.04532067100150997,-0.04532067100150997,0.9354938061154568,0.4692642489800223,0.014772097150307958,-0.4794208395484583,-0.04532067100150997,-0.04532067100150997,-0.04532067100150997,0.5692463466357883,-0.22308298604205543,-0.29556392273940646,-0.33994271224699457,-0.04532067100150997,-0.04532067100150997,0.588659365543387,-0.48859304532223413,-0.02269741320203169,-0.47612941491985145,-0.04532067100150997,-0.04532067100150997,0.5692463466357883,-0.7860854885338121,0.1304015808208572,-0.07736024988073571,-0.686367389388236,-0.183972902336745,0.5464277699938906,0.04341678676129665,0.07594506087141428,-0.8360350589975089,-0.1735805146530606,-0.17152140523444018,0.5522465735611204,0.0844415200675816,-0.3052883762211028,-0.15949399325126223,-0.15669468699310854,-0.27784432486325367,0.43445551733552074,-0.4835342871698953,-0.6534901276735662,-0.13642694962155083,-0.13237031683074144,-0.12808600973280163,-0.12355903070020725,-0.6907300537883752,-0.10835253587315444,-0.10267783178188651,-0.09666383926917997,-0.1690154307368685,-0.16548119899755545,-0.0754013163555962,0.7887681445352901,0.4053630839214107,-0.06686366581155298,0.4636611081567347,0.2502288104901238,0.7495725530857474,0.5043761210159091,0.30386330544433354,-0.7156447513390316,0.7499331145870861,0.7075608521487483,0.5302262733767327,0.2502288104901238,-0.464699965987862,-0.6613797599567283,0.9049779585218485,0.7075608521487483,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.5040286124242029,0.7887681445352901,0.3038633054443336,0.2502288104901238,0.7593213613986659,0.5092255580079639,0.7075608521487483,0.6929394871697984,0.48970305443947126,0.2502288104901238,0.7451235802114864,0.2502288104901238,0.7075608521487483,0.4096516999917802,0.30386330544433354,0.2502288104901238,0.7451235802114864,0.5132839584277499,0.7075608521487483,0.7887681445352901,0.3572172541558801,0.17905237859760317,0.86563803853413,0.7214108386018933,0.2768283000971267,0.4917364211991423,0.0,0.8660254037844386,0.6540484013726188,-0.20412414523193154,-0.7071067811865475,-0.35655115700982565,-0.032563837908042674,-0.0271319531127849,-0.5240769759417159,-0.33586403639641954,-0.3136530883364908,-0.24281467663015305,0.013697231954607411,0.022198506204497863,-0.34212288090650883,-0.6316997308779438,-0.29082061425964323,0.07489776694980785,0.0879580235611868,0.10206071859169945,0.33653593135601256,-0.014132968141930538,-0.5513475150630468,-0.6886684447957291,-0.139210191922782,-0.12855771537051042,-0.4355531647676405,-0.4227907247760231,-0.3842496809105675,-0.47439338572891887,-0.20155796430934322,-0.24080732065382346,-0.2902503318458675,-0.1916516973954469,0.24546516081546965,-0.24925256621903144,-0.19696642059663136,0.7214108386018933,-0.004000622198367561,-0.4965231577120848,-0.06761893104033172,-0.43726121868955037,0.6085430179842322,0.5728918992315463,-0.4677125280524501,-0.028682843900628772,0.0457391681174478,-0.025730271519149854,-0.5259385648439721,0.17200522903844542,0.25556070362486055,0.0457391681174478,0.0457391681174478,0.5401777530653098,0.5175663845863315,-0.4122185616067168,0.5123309514673036,0.34804826443050074,0.06433147875236218,0.48708015118759757,0.36697839979111546,0.4869744325747215,-0.8063743950440542,0.33026355309775374,0.6244890647913814,0.36613951463484296,-0.4351558117462916,-0.04532067100150997,0.6817612146490665,-0.6397368316306271,0.5545939714335397,-0.20298159736496113,-0.04532067100150997,-0.04532067100150997,-0.052005493207478284,0.6121466907775731,-0.5439117078513773,0.1838619773144781,0.26859886787482246,-0.466514850647084,0.09477128627851926,-0.12162315100846814,-0.02424144053236631,-0.3797866872695304,0.746759858264755,0.39838416123474285,0.10042125487761402,-0.04532067100150997,-0.04532067100150997,0.5292205392168664,0.03573708449459316,0.8760119686804969,0.6739515297207773,0.41109397885656096,-0.04532067100150997,-0.04532067100150997,0.5745746331951919,-0.2831476694571611,0.8324511088472273,0.4552589274906373,0.2340330193614407,0.18616443364228133,-0.04532067100150997,0.6732716426481057,0.3150445060164079,0.8670769285936318,0.6675770589896074,0.2052633239543861,0.11156590207950169,-0.183972902336745,0.7192572085244276,-0.4724206195672624,0.7531813531426295,0.5453389145775729,0.22443918104077543,0.20221407808479122,-0.17152140523444018,0.6481928412888996,-0.8523429884016521,0.6989817828119161,0.25542442834501417,-0.15949399325126223,-0.15669468699310854,-0.15374601382995673,0.43445551733552074,0.17953316737068212,0.16479921696942687,-0.13642694962155083,-0.13237031683074144,-0.12808600973280163,-0.12355903070020725,0.5814424419542751,-0.10835253587315444,-0.10267783178188651,-0.09666383926917997,-0.1690154307368685,-0.16548119899755545,0.0,-0.508034425260854,0.31998772758079647,-0.5679199398719007,-0.523805505944013,-0.2719333814409514,-0.4275941873027329,0.3369850285515542,0.8305236096832691,0.46148614401421245,-0.49231163302332026,-0.005336939207515571,0.012691968690126959,-0.5110616502763334,0.4102157277061374,0.7214108386018933,-0.5376988723011474,0.7558074165190595,0.14883241828801091,-0.6225317562548818,0.7090807509948718,-0.6248837155657432,-0.251370292102028,-0.7418690439666864,0.7298854375883861,0.16755168234618428,0.8232281344254794,-0.6063763203547793,-0.5407058776101649,-0.40543456458461025,0.646759240970528,0.7234491925137776,0.172818063241586,0.412366141338448,-0.5346674555096809,0.7527921213577384,0.6373034665256408,0.2971108439968373,0.34701927227008644,-0.33831313902882876,0.37382141963355126,0.6917592857415031,0.7641820430031926,0.5074527928651222,-0.25864026776637217,0.7225348661664396,-0.2581988897471611,-0.012256252679182116,-0.18911982828559631,0.35196376247454353,-0.6209076127081518,0.0,0.7296563362733232,0.3585695185690019,0.4312687935598088,0.005674887504373819,-0.2434067961932013,0.7960029457578486,0.7731495637901735,0.4500014466763189,0.2555205132552462,-0.7067826944727917,0.0,0.7412840909066531,0.2848964997626105,0.4644729534183078,-0.041027468117019086,-0.8250714801580036,0.9258352155729361,0.6970478859757049,0.10303616663388532,-0.34295802517030083,-0.17553801543684555,-0.3718802411281219,0.6877561259913006,-0.4365762598996667,-0.23315215371962209,-0.26508587183117965,-0.15949399325126223,-0.7140093814662152,0.6210236118649777,0.24896764872145555,-0.15143935164407163,-0.33642961037723773,-0.7931306496027085,-0.23886174008636615,0.5065960176296648,0.27880381709316787,-0.10835253587315444,-0.10267783178188651,-0.8009235244351697,-0.3422308167702721,-0.4365131182560873,-0.8028629311004086,0.02937612973267827,-0.2579219200759754,0.4534251929419071,0.03996548472536015,-0.39818379948511534,-0.37378922896798994,-0.703309998982673,0.24441975793480522,-0.23952379697200213,-0.7026714655399282,-0.19709926309480386,0.6203595152364121,-0.12749759504753916,0.1668614455154995,-0.14477843977889213,0.5054300045307331,0.0,0.516995535720349,0.4818738544425089,-0.008172195485894611,-0.6829566069058776,0.6814159033493054,0.7315236395222119,0.10575755782114618,0.18976211361362594,-0.1462372527168079,0.07560687408872896,0.5930015466490844,0.5608243063442607,0.35805743701971643,-0.6269002631272599,-0.6394981489279145,-0.3329530043087854,0.3264049020439062,0.21199725831174435,-0.39487785395091035,0.8348127082972265,-0.6755623486733214,0.018918485952906385,0.5539914402632062,-0.2495023908600318,0.39397617767583515,-0.04624128377924923,-0.8635270462161392,-0.4927798033646488,0.413650580863107,0.2970929882967983,-0.5005246629467431,0.008944417649478785,0.14064895712598144,0.7788598137167748,-0.6748748308762853,-0.14756954127146468,0.1889905783325454,-0.21743225822111056,0.43894090855080836,0.3831298297940588,-0.8522042387591692,-0.2581988897471611,0.6967900728326434,-0.3996605464414898,-0.3760126718956251,-0.6074186193861709,-0.2525927288970137,0.6620666101026521,-0.8096647122755937,-0.06255142861059175,0.010591040114603089,-0.04532067100150997,0.4376113776925027,0.14569437472481575,0.47340125184236265,-0.8932021432103523,-0.7527041643355501,-0.3248047281371239,-0.5826238897009491,-0.42771519396386554,0.19111358280730492,-0.10003383992939643,0.6748843297383071,-0.7289476484142928,0.337470336110017,-0.18318502420226299,-0.4616700615548844,-0.48751842048901,-0.43395461264136215,0.723809246567694,-0.42637572548390357,-0.3354632054641395,-0.18201689465654797,-0.061707654295812576,0.46286647878171544,-0.2807412198598612,-0.320019710604268,-0.6019239561467427,-0.5690524522438349,-0.8329860904023427,-0.6306314866364706,-0.14701292333545035,-0.1566287698960718,-0.5456566228544204,-0.4010922093997746,-0.43404490572661064,-0.13642694962155083,-0.13237031683074144,-0.38924476148402226,-0.6929475581115431,-0.7577828917675923,-0.5417106199907742,-0.10835253587315444,-0.10267783178188651,0.2289732343228903,0.31663793936376344,-0.3676779301816985,0.11768228721187347,0.11771790427181061,0.032892014886254255,-0.41332166690028754,0.7966188809566351,0.842228174482794,-0.5596275216342407,0.859423804892011,0.7302786041377101,0.4011918927992478,-0.16408765824214636,0.752461062818548,0.8967669924085103,0.4473525568685494,0.0,0.5145759841274763,-0.13227788610455374,0.8526337691440546,-0.37662398882868653,-0.7122385077673044,0.7069676698829587,-0.2567582517699363,-0.11045048394898073,-0.30729833983707106,0.7252865737839016,0.3400214221508548,0.5348824040037005,0.6487113744549468,0.818756643742494,-0.8265464437256892,0.4097204449702105,-0.7888057157695038,0.6283388456990443,-0.42052576314402823,-0.6531851847855544,0.4691001634534436,0.632950797552351,0.3121794241395189,-0.8468307017252259,0.7779203178053162,0.8078426142674763,-0.5246363972676777,-0.7275823599715133,-0.8301188747765521,0.7077405855992223,-0.009865037218421798,-0.7334834101084725,0.7473817876322465,0.6642284277431045,-0.7882346502804469,-0.26784941360409936,-0.6748143029680006,0.8073392391686269,-0.24011675959291767,-0.6453094998495392,0.6847962829934104,-0.7506478296580354,-0.13642694962155083,-0.516332836823039,0.6709687375400868,-0.10835253587315444,0.5687805579947757,-0.03294644909599835,0.8406034786362148,-0.9086959512514401,0.3904408794676231,0.5525818726235717,0.0,-0.4395075720440213,-0.4355531647676405,-0.5576762486121578,-0.6216693526742193,0.25574037387779186,0.09307430795725237,0.0,0.22446912513818798,0.022083939661672207,-0.6297981791560173,-0.3772445622543865,0.5721544598631443,0.25006950657703847,0.0,0.21650881544431602,-0.5261134208741842,-0.7319241945917851,-0.16191941191907883,0.7529418479199456,0.16328197240521458,0.28822631305665164,0.5052911526399112,-0.6770736049753235,-0.6116264602675309,-0.5532518678145578,0.5290678458778626,-0.732735437360335,0.0,-0.6297098789526245,-0.8180300739210609,-0.7452191516713619,-0.5724237497646784,0.42772777895067826,-0.009883741351612635,0.2134073095529255,0.16535727493718647,0.4430900817496691,-0.7397050108740754,-0.40475087656291486,0.6340028557937223,0.7241414351211243,-0.0853793869414222,0.35027416082241736,0.5332063265292717,-0.18416652038970002,-0.6265055564821006,0.5755274824250777,0.32786397327795364,0.25653178119972564,-0.3279980907159822,0.5436045531789976,-0.7964920775124683,-0.5285220195093733,0.8763350086840396,0.03683161670143516,0.24732459338858184,-0.6317629439823612,-0.3362789812236649,0.7983262616202846,0.7384656481215935,-0.8369643961498217,0.46635980958929757,-0.10433386099202262,-0.3368888945039727,-0.8165774813780801,0.23863820151105208,-0.39989142624002544,0.6764544263504891,0.22153962581369002,0.3423877103349665,-0.870694974858564,-0.6793574560735568,0.6049258635710064,0.1513859311627826,0.35768559732518557,-0.15174856077510138,0.6948058278337365,-0.7557271060076381,-0.45442788614352164,0.7908258733750863,0.1354130943502699,0.2099838293460767,-0.42368723564005456,0.861629617315134,-0.7912428441391434,-0.11280334654892214,0.44896594396865364,-0.0184369726808223,-0.03042420259071307,0.5773502691896258,-0.22281985488557635,0.25932941367410905,0.9858869581095002,0.40513590347399075,-0.2623224671451949,0.08915859273547815,-0.469650285562026,-0.7850035703645288,-0.25050976885684473,-0.5209247152501538,0.19085258511646122,0.20042184114094372,0.8805935322110982,0.38694574910086654,-0.5880434246968841,-0.87422058971564,-0.05890248886190719,-0.22714447764627652,0.07364884187137727,0.5533094102155222,0.05633471432786838,-0.10835253587315444,-0.6865928819840844,-0.3945960031711431,-0.6551360900676527,0.1160013786120102,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,0.5469141909675319,-0.45742019910067166,-0.35806781427665135,0.8611731139145825,0.8577951299420694,0.34873398679797624,0.6866731276788394,0.5222329678670935,0.8454696840715094,-0.28098035149559003,0.661321483866089,0.7964991882001425,-0.4095418348104017,0.6205052279940229,0.08986111131013433,-0.8175113387802025,-0.5454023027839483,0.5222329678670935,0.41670916130512964,0.7171992557494655,0.0659578246691135,0.7786189843209607,0.5203193622714797,0.5251398537932878,-0.11168886886214965,0.4306414893115928,0.7144787929353293,-0.4961089940282442,0.26885717414804383,-0.10024590049850797,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,0.9308296160504307,-0.31350448613512344,0.64410735877456,0.5435020573542262,0.6370234295356537,-0.14321655610089523,0.718395767527796,0.6025561665458197,0.6899438970764984,-0.11124786897594822,0.29844400799971027,0.3255282156618946,0.7462873634616584,-0.3073267555321246,0.6389694774769906,0.40204954642087604,0.6317010100739405,0.002330044472121642,0.14799399452759893,-0.25007604120135946,-0.15900753686888555,-0.21087867713478514,-0.6160829556879275,-0.18058301992561007,0.11956792120438008,-0.11519932128227382,0.32207959355270965,0.24047328498731368,-0.4166654563232788,0.0711783918559574,-0.15102749953776967,0.7484352101849452,-0.7569671928885559,0.0,-0.4355531647676405,0.44867473293245036,0.7193141733078063,-0.3483635450869743,0.3221545719360085,0.4891666604081971,0.5222329678670935,-0.4192583277783588,0.6087521247140342,-0.7322463200227961,0.5780285658812839,0.5274808530246775,-0.2808419941807957,0.35575120137830496,0.9153187820658627,0.22447192314781922,-0.5768335138314167,0.7452413135250995,-0.27301815342074776,-0.2467076897294926,0.5222329678670935,-0.5041660098317663,0.3507388871951392,-0.292342030181766,0.1575517112163585,-0.8151185750527778,0.3882602161415942,0.7496942167073617,0.7653830108679803,-0.9031423918237256,0.39460018696382204,0.6416348190592419,0.7795535738541093,-0.8253296001945783,-0.33347770988328834,-0.4817833779156712,0.02792896396390355,-0.7621766577797727,0.5539500850894272,-0.795774275039998,0.7751053721830463,-0.8639706695458573,-0.4999713182655435,0.35391016775861517,0.7289906285932864,-0.6718453598074642,0.29991883566493355,-0.6863745522313701,0.765279667983568,-0.6888753302173587,-0.08122689004084395,0.8549819600709617,-0.7318511446448713,-0.20293055327940643,-0.698907500811567,-0.6430425910347556,-0.5432368871893779,-0.09753410011662822,-0.69214886049407,-0.7704981986465622,-0.34635567570792003,-0.11064874734972924,-0.10835253587315444,-0.8529303626945559,-0.7747150075089132,-0.6312648343369752,0.0,-0.18787577456684298,-0.17568187354729867,0.3645453092983775,-0.49096443270377865,0.3356647649556397,-0.08853906473527291,-0.06386718472761346,-0.035833399207393445,0.7006371682889228,-0.5258715896067695,0.8116681951497386,-0.004913025817426305,0.8291133496525602,-0.06893529476616927,0.41461450332459987,-0.39220636240155693,0.4242374439204022,-0.40485708771149975,-0.41017145312333614,0.3256485349778145,0.35094320882354124,-0.6210590034081187,-0.3876990572676327,-0.19666919344652806,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.07032494875510756,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.18787577456684298,-0.17568187354729867,0.6325621003530739,-0.7376570624710856,0.19824906542813714,-0.11032749348962717,-0.08853906473527291,-0.06386718472761346,-0.035833399207393445,0.5492749567266382,0.21935735174863982,0.11183399128268835,-0.26839155261289316,-0.004913025817426305,-0.3395876527190317,-0.16167410949698277,0.5152013863855462,-0.3854624923680711,-0.46048169688434726,0.42277584215457986,-0.4048570877115,-0.41017145312333614,0.3256485349778145,-0.5513226936603202,-0.41403933560541256,0.36928978380412253,-0.14465001372554434,-0.34874291623145787,-0.2581988897471611,-0.2581988897471611,-0.8716281484584735,-0.07032494875510756,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,0.7226824402144922,-0.2581988897471611,-0.18787577456684298,-0.4756089135285118,-0.1620492291825904,-0.6927327888214317,-0.08853906473527291,-0.06386718472761346,-0.035833399207393445,-0.0038569284319363998,-0.5273790107841475,-0.004913025817426305,0.7955223526694318,-0.1271013111802056,-0.20604670093634578,-0.3987494971033641,-0.40485708771149975,-0.41017145312333614,-0.41414141414141403,0.35094320882354124,0.43641380970896815,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.18787577456684298,-0.17568187354729867,-0.1620492291825904,0.3984142190833906,0.6576863653955292,-0.11032749348962717,-0.08853906473527291,-0.06386718472761346,-0.035833399207393445,-0.3767147092294649,0.6139309397661737,0.782379153975502,-0.04184999018767307,-0.004913025817426305,0.03853996718248504,0.6607299735367196,-0.2641601589592388,0.5439835260705577,0.4154220940388059,-0.3987494971033641,-0.40485708771149975,-0.14082999449351732,0.47895105803143767,-0.5455722965095927,0.35024313111902333,0.5834276208691653,-0.3876990572676327,-0.34874291623145787,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,0.9485759505765576,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,0.5698412946808923,-0.42426406871192845,-0.44939797670375853,0.27297067060947594,-0.8653578208347266,-0.8418394858524693,-0.10912915609253614,-0.5405689030937241,-0.9958705948858224,-0.5261837177996508,0.15811388300841897,0.31862881254500136,0.8660254037844385,0.0,-0.2704714592314516,-0.6240513739075659,0.8297419797915963,0.31422493638077376,-0.7970340793499889,-0.4198978187193777,0.4303206433256331,0.3995206748389057,-0.5516652784507498,-0.37867269425938815,0.5661402526663349,0.9589098921255875,-0.550774676521739,0.8810076369139084,-0.2581988897471611,0.7226824402144922,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2898754521821014,-0.31920887460879804,-0.39629696195060854,0.9449111825230678,-0.2266162753805409,0.944911182523068,0.0,-0.7071067811865475,-0.8103788741587159,-0.7956280102814204,0.27787161266532695,-0.8806088402725198,-0.09759000729485331,-0.5941032577494995,-0.6112603302119518,-0.8713917838497269,-0.6546536707079772,-0.8660254037844386,-0.6802865778281048,0.21666830941799442,-0.0573460798126111,-0.8315667549715154,-0.005076500556638316,0.6177912369472641,-0.05846143389488504,0.027295837003541094,-0.8315667549715154,-0.3949664444314453,-0.3949664444314453,-0.4036036763977875,-0.2691917628409274,-0.40295466789385004,0.0,-0.4036036763977875,-0.4036036763977875,-0.8052170868200578,0.06860753837801113,-0.3876990572676327,-0.3487429162314579,-0.2581988897471611,0.9154324272853895,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,0.5693549221212508,-0.7187817427740001,-0.736550942631332,-0.322682658599683,-0.40844025544631285,0.35686796227038303,0.6332684000291027,-0.061016958300200284,-0.303488489333442,-0.303488489333442,-0.6177283369584906,-0.6081771705869038,-0.6014811214173097,-0.6288136730208924,-0.5394570788606908,0.32956882094869044,-0.501646637970781,-0.482266077443566,-0.45112082022953404,0.3644482986430507,-0.5374280212842173,-0.839974354346304,-0.4853935883904147,-0.4681965040440347,-0.8100854064561956,0.08966634348845352,-0.6031363605571595,-0.14633485450076056,-0.3660002749536396,-0.4770433444377102,-0.4770433444377102,-0.6414304532268282,-0.21334027754207324,0.17126159006881012,0.5652258790353353,0.4708856197492654,-0.6224953653101111,-0.5927333353568489,-0.5927333353568489,-0.5927333353568489,-0.714319436412802,0.3470302260047773,0.3901393682797313,-0.5927333353568489,-0.5927333353568489,0.5970885653562694,0.1810824089104369,-0.43672836396031883,0.8785659163621241,0.23802971406424642,-0.619401591743338,-0.619401591743338,-0.619401591743338,-0.012480113859363163,-0.41386648017510086,0.4584033972093177,-0.772838640655272,-0.43540230071158603,-0.619401591743338,-0.619401591743338,-0.8350638636220225,-0.2116658882456921,-0.6986991446834462,0.029443731201613445,-0.619401591743338,-0.619401591743338,-0.7171883528369253,0.796246317124392,0.8948952718704826,0.2853080426955507,0.49651219718697776,-0.7043389497856123,-0.10860887238755518,-0.578690612563015,-0.6076436202501999,-0.18633775916184298,0.6140808035688312,-0.41209234783454485,-0.619401591743338,-0.619401591743338,-0.619401591743338,-0.11007031052852236,-0.6489252651250368,-0.3614885849612484,-0.2559056322008053,-0.619401591743338,-0.619401591743338,-0.619401591743338,0.06945766267013614,-0.08531034676318067,-0.09078687864106068,0.2870420500306796,-0.619401591743338,-0.619401591743338,-0.619401591743338,-0.060763913554255304,-0.7419744972314402,0.1828476932357486,0.505988366218464,-0.6194015917433381,-0.6194015917433381,-0.2730969390378505,-0.4905287788285498,-0.6592869843116022,-0.6785263275497022,0.7134192642711589,-0.23390912449269438,-0.23390912449269438,-0.23390912449269438,-0.3389206894957703,-0.5107208823672349,-0.7353165797421557,-0.6177283369584906,-0.17824640802311453,0.4765345021551105,-0.3671913494213893,-0.3343827935029986,-0.501646637970781,-0.482266077443566,-0.052204816174342104,0.0,-0.46715384355268924,0.6771002118265715,-0.4853935883904147,-0.27248528174725556,0.6327396972677966,-0.558309763113096,-0.1639760123172805,-0.5449962465196659,-0.48432266510229943,-0.4657449352244384,0.8944271909999159,0.9523574859836244,-0.22248549671220977,0.7927505158414726,0.32239812019888875,-0.5927333353568489,0.6960742232331466,-0.30512569748420804,0.11274487907565123,0.5402670881592707,-0.7328737264525266,0.13637176129778117,0.45076394091512767,-0.3127353843565695,0.8747159139227911,0.15487153430528575,0.010679079616249884,-0.29358649626789335,-0.4401768938697289,0.4133351656272694,-0.581508786580434,0.713275683629894,0.8255199027746102,-0.7974289618780472,-0.5566648585353445,-0.619401591743338,0.6124653489641694,0.05327144124706502,-0.8271243745130807,0.1684669078920251,0.7656329237430078,0.7665574065703559,-0.13869611745361946,0.5117891288210595,0.11824628692996977,0.5578448800782573,-0.6076436202501999,-0.6076436202501999,-0.46816557321257896,0.5364054221767299,0.4413360904642108,-0.619401591743338,-0.619401591743338,0.7167351544816436,0.7238652787502976,0.41357166204648527,0.43194983822854677,0.6232212200927821,-0.619401591743338,0.13719302598149744,0.7157351153101095,0.2512376951206842,0.22861880427089484,0.1033558987135656,-0.32542122406443164,-0.6406993529780132,-0.619401591743338,0.5375291019059516,0.40772139575654764,-0.5475845871894154,0.6562468857733535,-0.1734235737448191,0.19886456333394228,-0.35427531055988676,-0.4288107506818055,0.8580706860967519,0.8711150253672522,0.9359579552511783,0.27729976692510694,0.5682258941577587,0.06896716055335672,0.7884898772514476,0.9690178216792086,0.6493171795472215,0.06137850513887587,-0.30827169328651105,-0.7043389497856123,-0.5276179625130264,0.8057560061245255,0.5654902662927869,0.20349606878115362,-0.7043389497856123,0.19595700706686886,0.33677544243969293,0.4893330310304038,0.04146055248208383,0.32677847440673574,-0.006360926002804878,-0.31089404438685614,0.5475603984975762,0.9050420562578877,-0.016469125940120465,-0.21243486396674968,-0.7043389497856123,-0.7043389497856123,0.8355887826468165,0.41476920951214985,0.5696891207949514,-0.7043389497856123,-0.7043389497856123,-0.7043389497856123,0.22848888991682476,-0.05384339271732372,0.5373488493630996,-0.05384339271732372,-0.7043389497856123,-0.7043389497856123,0.43138359399024856,0.7560336574112689,0.22272797129913843,-0.7043389497856123,-0.7043389497856123,-0.7043389497856123,0.607774416394732,0.42966892442365967,0.0,0.453425192941907,0.453425192941907,0.453425192941907,0.447213595499958,0.14322297480788657,0.0,-0.6177283369584906,0.2858686611580389,0.29865966269345284,0.38598771789698527,-0.501646637970781,-0.482266077443566,0.8937975704983925,0.8657309785614021,-0.23153097560576646,0.3289019372554744,0.6220089107610198,0.751005021199655,0.7415711521284188,0.024184651841151223,-0.4770433444377102,0.5348936253340195,0.6745816007781246,0.4790399932758861,0.19708219286013337,0.6046765248479539,-0.4259753887706836,0.7661548216391397,0.4667320851690653,0.7035308064353126,-0.5927333353568489,0.744833981752435,0.8343670867311521,0.6956685566174615,0.9981190970069563,0.7994167264110766,-0.31275325198383835,0.7627313149838774,0.7514852028085955,0.7637251632943496,0.9774646584250225,-0.31275325198383835,0.3538007835789926,0.6133283880767196,0.6900220285492107,0.8426889349164391,0.7231962646079512,-0.6358556308331254,0.950553711449615,0.5816054255797853,0.36967145827615083,0.006918145204308467,-0.31275325198383835,0.6710414035160348,0.6133280799102636,0.8144547943050782,-0.31275325198383835,0.4552523889077164,0.09993297753740436,0.2995691869125334,0.7599091476212665,0.04786261821731679,-0.31275325198383835,0.12725796698357353,0.6711401548566042,0.8308238283906635,0.8332662680627771,0.2823225132155952,-0.0024137110273671795,0.4523552992596048,0.7766676372588126,0.6272566281984275,0.9294334978096132,0.1929429288582709,-0.31275325198383835,-0.03512345048951698,0.7613424918950052,0.3684738149602602,0.7718115141148991,-0.8315667549715154,-0.6302320007867649,-0.5801804715264876,-3.5252791857717644e-15,-0.08127995789508022,-0.8315667549715154,-0.8315667549715154,-0.4036036763977875,-0.37009074000180053,-0.394603415419413,-0.8315667549715154,-0.8315667549715154,-0.2924876757282291,0.8392386464957226,-0.8052170868200578,-0.7672080800443577,-0.27678168302754314,-0.16726505124870636,-0.2581988897471611,0.7156264473321342,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,0.9154324272853895,0.9154324272853895,-0.1643989873053573,-0.6177283369584906,-0.38128640533322417,0.697840229515112,0.6983591625929452,-0.5244509499152916,-0.501646637970781,-0.482266077443566,-0.31727626800140596,0.39470382889346023,0.8108616232054588,0.3403126603861606,-0.4853935883904147,-0.4681965040440347,0.377423643834138,0.12703533941239648,-0.01894606846999017,0.33301003734528467,-0.4770433444377102,-0.4770433444377102,0.05767891589836563,0.41892101987087027,0.5902939407530761,0.12295532096237789,-0.5927333353568489,0.28607428677318547,0.5226988017884089,0.10623759324126103,-0.5927333353568489,-0.5927333353568489,-0.5927333353568489,-0.0205814491669983,0.4813833943101643,0.3423296938586592,-0.17619031865607326,-0.619401591743338,-0.36287231425223937,-0.08090170335773329,0.5274015922842273,0.19270543559935596,-0.619401591743338,-0.619401591743338,-0.619401591743338,0.259984961009918,0.4351569764303974,0.6519240120704355,0.5211275546254541,-0.26838404691169077,-0.09128846178601183,0.7222017136030392,0.8846988042383974,0.0,-0.0637359495882753,-0.619401591743338,-0.619401591743338,0.7300370898821855,0.8932710967166501,0.7297307436603946,0.12430221552323681,-0.619401591743338,-0.01002973599876165,0.8762344500610179,-0.12760112028667397,0.6428230585044034,-0.5523864859594552,-0.619401591743338,0.1668349918058767,0.6937002910706719,0.3009144595099586,-0.11131895499815156,-0.6194015917433381,0.166137239951246,0.5417398572981313,-0.3607447156205402,0.7184745814205271,0.426243102478563,-0.23390912449269438,0.4282608771722994,0.6251310624948377,0.610837485670792,0.8077043082349259,-0.18156825980064076,-0.6800940691793416,0.05382735877629195,-0.6687341046641716,-0.6919890071051025,0.5549926990106763,-0.5307160536207052,-0.8315667549715154,-0.4036036763977875,-0.4036036763977875,-0.4036036763977875,-0.5828717043012237,-0.8315667549715154,-0.8315667549715154,-0.4036036763977875,-0.267743808496017,-0.554391512765193,-0.6692130101240628,-0.8315667549715154,0.3247886561723863,0.30180562597800165,-0.05846143389488504,-4.0718948572848944e-16,-0.8052170868200578,-0.7672080800443577,0.7839659348220407,-0.2268388384186525,-0.17722549332617152,-0.34874291623145787,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.8315667549715154,0.20727235653641105,-0.4036036763977875,-0.4036036763977875,-0.4036036763977875,-0.8315667549715154,-0.8315667549715154,-0.4036036763977875,-0.5828717043012237,0.4904133569608351,-0.8315667549715154,-0.8315667549715154,-0.4036036763977875,1.084268171655641e-15,0.26041911462267026,-0.8052170868200578,-0.41095381623455823,-0.3876990572676327,-0.16726505124870636,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,0.7406569751795089,0.0,-0.05710402407201607,-0.8884170527924956,0.5800029702940247,0.5567859929248509,0.7804214658417283,-0.5329916733164214,0.35637095004666897,0.6367860507691099,0.6826930225979305,0.0,0.7844645405527363,0.0,-0.16096872731710674,0.7844645405527361,0.24080732065382351,0.784464540552736,0.0,0.6782645105654813,0.0,-0.7784068086568319,0.7148390984373268,0.7844645405527361,0.784464540552736,0.0,-0.3784115038845798,0.04698571924573446,0.0,-0.971179392893021,0.5544021420581858,0.0,0.0,0.7844645405527361,0.7844645405527361,0.8889671970157156,-0.3348902210918905,0.7844645405527361,0.0,-0.23460201043956588,2.8230391056958538e-15,0.3096094122692016,0.914991592472124,-0.19017284379281307,0.8283495734029905,0.0,-0.16132763239437126,0.7844645405527361,0.599664623055931,0.784464540552736,0.0,0.7844645405527361,-0.15138925704901615,-0.028209146888372245,0.4924851260391455,0.7844645405527361,0.0,0.0,0.724568837309472,0.0,-0.6030751420522631,-0.17325260600803766,-0.029833824575772428,0.4788983496981684,0.3854199065563143,-0.27391677543189824,0.49408208774087514,0.23176425072622986,0.647083864756678,0.5144744586751024,0.5277986629117474,0.5833371893818499,0.5447047794019221,0.21878043238246966,0.7150556714337868,-0.5814524542228187,-0.5879572942861635,0.5950103083819183,-0.644807876446444,0.10368606861560252,0.0,-0.7694098843098921,-1.1915000418136931e-15,0.417230985423222,-0.12206438246699455,0.3784115038845798,0.7844645405527361,0.0,0.0,0.31484851571149697,0.0,0.0,0.0,0.0,0.0,0.0,0.0,1.6488864471291286e-15,-0.4090470714228113,0.06405510009067046,-0.09685485552825748,0.0,0.0,0.0,-0.0721967632343502,0.5,0.0,0.6123724356957946,-0.4956199125785412,-0.500458393275385,-0.505419854312669,0.806515389459356,0.38862719085564545,0.8531060411638095,0.663622150005551,-0.5379143536399191,-0.08420540843111307,0.5237412729660951,0.5360562674188973,0.4618802153517006,-0.5749661560353623,-0.5814524542228187,-0.5879572942861635,0.5950103083819182,-0.42905816516051654,0.7844645405527361,0.5944124802409158,0.0,0.0,0.0,-0.24402887326207381,0.7844645405527362,0.4615467470642054,0.7844645405527361,0.0,0.0,0.0,0.7468563574599022,0.6758862681841687,0.5829752480681663,0.6840838686503156,0.0,0.0,0.0,0.7844645405527362,0.0985187659973608,0.0,0.0,0.0,0.0,0.5026642643831538,0.7844645405527362,-0.4472135954999578,0.0,0.0,0.0,0.0,0.36615851043689523,0.04919832203859492,0.6995392475186418,0.7844645405527361,0.0,0.0,0.0,0.011139295750190353,0.7546556075998152,0.5424384851322468,0.4430383713324795,0.0,0.0,-0.12296333385037268,-0.10127393670836665,-0.4948716593053936,0.0,0.42801743093908906,0.0,-0.8660254037844387,-0.6546536707079771,-0.4956199125785412,-0.500458393275385,-0.505419854312669,-0.5105073560858459,-0.5157236706714444,-0.5817254100434042,-0.5265516045859981,0.4089968496654696,-0.5379143536399191,-0.5437951687821688,-0.549805160293821,-0.5559385652911958,-0.5621863674135879,-0.5685352436149611,-0.1074767915380524,-0.5814524542228187,-0.5879572942861635,-0.5944300934137098,-0.675902477363216,0.0,-0.4557327151876499,0.0,0.0,0.656293503084857,0.6890098957999242,0.0,0.28386409972640186,0.0,0.8349570688732537,0.7844645405527361,0.0,-0.22302704208965848,0.17200522903844528,0.2733344113491574,0.30382181012510007,-0.09245003270420486,-0.5383260945789446,-0.4472135954999581,0.0,0.0,0.47383090353650126,0.06477502756312956,-0.6076436202502001,0.0,0.0,0.0,0.19179959726524354,0.7844645405527361,-0.3348902210918905,0.0,0.0,0.0,0.6420280125354673,0.7844645405527361,0.0,0.0,0.0,-0.13218423623348546,0.5175424117754857,-0.16624349603272656,0.0,-0.6921481157136118,-0.500458393275385,-0.505419854312669,-0.16729899041956528,0.6221057624657332,0.8551315688243534,-0.6445171019552843,-0.5379143536399191,-0.5437951687821688,0.006585525773979201,0.2914554344995381,-0.5537749241945381,-0.5749661560353623,-0.5814524542228187,-0.5879572942861635,-0.36878214891289307,-0.4254398138323376,0.1317189571239352,-0.8388849077360018,-0.7560432565813523,-0.6803091740553736,-0.10255660764738253,-0.6395566292608688,0.1921082554114286,-0.7286698513881176,-0.7286698513881176,0.5841077139732719,0.6491869480684284,0.10114091647862654,0.4290200545434007,-0.7286698513881176,-0.7286698513881176,0.7576771321350049,-0.42116620329133597,0.8396154480280142,-0.7286698513881176,-0.7286698513881176,-0.7286698513881176,0.585943367288668,0.49379413718611054,-0.7286698513881176,-0.7286698513881176,-0.7286698513881176,-0.7286698513881176,-0.7286698513881176,-0.7286698513881176,-0.7286698513881176,0.47451716705713226,-0.7286698513881176,-0.7286698513881176,-0.7286698513881176,-0.7286698513881176,-0.7286698513881176,-0.7286698513881176,-0.7286698513881176,-0.7286698513881176,-0.7286698513881176,-0.7286698513881176,0.3058576782103798,-0.28945163000509494,0.15314781531075583,-0.09797202753701673,-0.322682658599683,-0.18759111843481838,0.6046011741413325,0.6568081256782783,0.12682710026788405,-1.6488864471291284e-15,0.011164998554411892,0.4801728268602346,-1.6488864471291284e-15,-1.6488864471291284e-15,-0.7199582268387873,0.46049915628601407,0.4968424356713296,0.4336324073897561,0.5932207470874876,-1.6488864471291284e-15,-1.6488864471291284e-15,-1.6488864471291284e-15,-0.6054168347136332,-1.6488864471291284e-15,-0.43427395370447686,-0.43427395370447686,-1.6488864471291284e-15,-0.43427395370447686,-0.43427395370447686,0.4336324073897561,-0.4242699450676523,0.6060734966104934,-0.6064154394402388,0.6428610420750422,-0.43427395370447686,-0.43427395370447686,0.15218254035972997,0.73635296327446,-1.6488864471291284e-15,-0.43427395370447686,-0.43427395370447686,-0.43427395370447686,0.1562457029094787,-0.43427395370447686,-0.43427395370447686,-0.43427395370447686,-0.43427395370447686,-0.43427395370447686,0.4336324073897561,-0.43427395370447686,-1.6488864471291284e-15,-0.43427395370447686,-0.43427395370447686,-0.43427395370447686,-0.43427395370447686,-0.43427395370447686,-0.43427395370447686,0.5879992692848225,-0.43427395370447686,-0.43427395370447686,0.4336324073897561,-0.6060734966104934,0.4968424356713296,-0.43427395370447686,0.36013538918810256,-1.6488864471291284e-15,-1.6488864471291284e-15,-0.5988304852019423,-1.6488864471291284e-15,-0.43427395370447686,-0.43427395370447686,-1.6488864471291284e-15,0.8754102944357351,0.2771829415983343,-0.43427395370447686,-0.43427395370447686,-0.43427395370447686,-1.6488864471291284e-15,-0.6060734966104934,-0.6301003003879385,-0.7304026235587497,-0.710987011899386,0.3483074472405143,0.2780601673153155,0.6225776298794916,-0.482266077443566,-0.4555727383364641,0.22064045403562826,0.6225776298794916,-0.6767936631777142,-0.8290379725557331,-0.7472870370393102,-0.6404996363054051,0.6225776298794916,-0.4770433444377102,0.14385227475720708,0.034819890101739616,-0.22787056562831373,0.1315804836023598,-0.09997751623558779,-0.5927333353568489,-0.5585044078372036,-0.2315593010182801,-1.9586744543430656e-16,-0.3891099848027567,0.0,-0.5927333353568489,-0.49101099285826266,-0.7114309056067295,0.7623177884199178,0.49553764877283235,0.17801924969212693,-0.31275325198383835,-0.31275325198383835,-0.2829993372371511,0.4438497996537436,0.7313914129809551,0.6605494117961558,-0.49503985695256186,-0.31275325198383835,-0.878669099337656,-0.004423628172107475,0.3197430339951497,0.7368076850680864,0.7272798643520201,-0.49069455862138545,-0.5220961781516251,-0.31275325198383835,-0.7359574788944443,0.420557216210395,0.06722711485772949,-0.31275325198383835,-0.31275325198383835,0.022404885295955992,0.07339330443961593,0.5462813581787765,0.5556208705513535,0.26020024466785013,-0.31275325198383835,-0.4492584743850474,-0.18369993225420916,0.17407501966502612,0.5462813581787765,0.8078736260367867,0.2576317062143426,-0.31275325198383835,-0.31275325198383835,-0.5703019528623292,0.15710225404974948,0.7520576081162283,0.07471367923659236,0.43422836997855324,-0.31275325198383835,-0.31275325198383835,-0.6549360151187277,0.6207888934178826,0.18117208295559048,-0.7536424666641384,0.22880630385102552,-0.31275325198383835,-0.31275325198383835,-0.7321770253938684,0.08235124930626929,0.7704742039602929,-0.5139573814977851,-0.5972140920199508,-0.5969747371959498,-0.6829612264764094,0.11901415692629894,-0.6395566292608688,0.0,0.7844645405527362,0.8550252774398882,0.0,0.0,-0.442896920482894,0.12940115777215383,0.0,0.299281673734186,0.0,0.0,0.0,0.0,0.7844645405527361,0.8189954117857369,0.26893099205232024,-0.38044771633786284,0.0,0.2699773471418208,0.30683998933315265,0.7844645405527362,-0.8329354158847951,0.6850285890384115,0.0,0.0,0.0,0.5596816369371859,0.0,0.7844645405527362,0.6204969104815937,0.0,0.0,-0.44437258975278365,0.5594311838120641,0.0,0.7844645405527362,0.7125253031944254,0.0,0.0,0.0,-0.06043309420860644,0.4472135954999578,0.08557996255200796,0.8249094433041266,0.0,0.0,0.0,-0.02637745300644317,0.42213209303575716,0.0,-0.14907119849998599,0.0,0.724568837309472,-0.24743582965269675,0.0,-0.83281608258222,-0.16146059742791027,-0.3989469149191077,-0.6252997692766742,0.45181966380417243,0.7952293575002705,-0.11961713748979344,-0.6986461658434497,-0.3851835427767263,-0.38168744708108987,-0.3777474245398947,-0.062234722073216,0.6063926036262143,0.18466867190408456,-0.09130117919728299,-0.3494446882587472,-0.34156003098191157,-0.3327154491651965,-0.571993343586912,-0.35869655921845534,-0.43427395370447686,-0.43427395370447686,-1.6488864471291284e-15,-1.6488864471291284e-15,-1.6488864471291284e-15,-0.6054168347136333,-1.6488864471291284e-15,-0.8095948696316225,-0.43427395370447686,-0.7788165300762653,0.5460667624672665,-0.207920933133975,0.34600688994340434,-0.08867216266007757,-0.43427395370447686,-0.43427395370447686,-0.39168015087072694,0.29250463742407184,0.07431574398296296,0.0,0.5944124802409158,0.0,0.0,-0.31491870887321555,-0.22051766723814362,-0.1838797812855611,-0.021591675854376522,0.7844645405527361,0.0,0.0,0.2129556586416961,0.7844645405527361,-0.07789050171365274,0.7844645405527362,0.5754646952551707,0.0,0.0,0.0,-0.3289938828048902,-0.03497001094741343,-0.3051306949487777,0.0,0.0,-0.6076436202502,0.7844645405527361,0.0,0.0,-0.1332337451304175,0.0,0.0,-0.1159153684604541,-0.6816561864488859,0.35805743701971643,0.45342519294190703,0.7844645405527361,0.0,0.0,-0.22381956974513112,0.7844645405527361,0.0,0.6612584238234283,0.7844645405527361,0.0,0.0,0.7844645405527361,-0.14907686154625827,4.479888993041075e-15,0.6477502756312958,0.21488604392103328,0.0,-0.6197797868009123,0.15811388300841897,-0.13093073414159545,0.3636740810733596,-0.500458393275385,-0.505419854312669,-0.685538449600993,0.5587607760564541,0.4128430026449095,0.8256937441576597,0.7925472267669249,-0.5379143536399191,-0.5437951687821688,-0.549805160293821,0.50201157349207,0.27751651417118256,0.5537749241945384,-0.14312564351916787,-0.5814524542228187,-0.5879572942861635,-0.6410425406260384,-0.42743574982337584,-0.6177283369584906,-0.6272373965567192,-0.44882379168866177,0.0,-0.543431891392146,0.6225776298794916,-0.558986170642741,0.3652752392712603,-0.500022510939462,0.38953218036027776,-0.3120805151894773,0.6225776298794916,-0.4681965040440347,-0.43311176063958146,-0.09787439362781357,0.198762219095561,-0.7339483316243832,0.6225776298794916,-0.4770433444377102,-0.17145614761334524,0.6070255742661241,-0.22633486846575654,0.3921021247022749,-0.5538679363243871,-0.5927333353568489,-0.5927333353568489,-0.1499302391934902,0.0,0.07996473880197424,0.0,-0.5927333353568489,-0.46591989140590195,-0.643706687498829,-0.619401591743338,0.3066829458136575,0.4602072876569784,-0.619401591743338,-0.619401591743338,-0.4255195318407148,-0.7733764417496355,-0.619401591743338,-0.10868000085988223,-0.6538422243081312,-0.619401591743338,-0.619401591743338,-0.619401591743338,-0.8083291337585635,-0.7452859177112892,-0.029795100506099997,0.45948714958064485,-0.619401591743338,-0.5154376120354333,0.015747547075119373,-0.06256040573216644,-0.48654136597022324,0.03695278087565514,-0.7179149816752202,-0.1624540683902062,0.453425192941907,0.0,-0.16431107185726337,0.15769360241480515,-0.6291982024916237,-0.619401591743338,-0.619401591743338,-0.619401591743338,0.44923095507483457,-0.619401591743338,0.36688456222816457,-0.040907343428657254,-0.619401591743338,-0.619401591743338,0.30388216747803615,-0.157906659938166,-0.4812239160218164,0.4893829428389145,-0.6005846160893202,-0.619401591743338,-0.619401591743338,-0.619401591743338,-0.5072606061898309,-0.801718591911999,0.2714170681429198,-0.13264434432764913,-0.6194015917433381,-0.6194015917433381,-0.6233312262535198,-0.2634074305798709,-0.445242115417213,0.6228721317009144,0.6038521961109629,0.5291913161717902,-0.6565714326994666,-0.6194015917433381,-0.17745144682019307,-0.09567645874669457,0.8720965259135245,0.6416675642180699,-0.7286698513881176,-0.7286698513881176,-0.07155751785282571,0.27947010550124063,-0.28635579956198653,0.934835256524872,0.5967831203404574,-0.7286698513881176,-0.7286698513881176,-0.7286698513881176,-0.5467989868153977,-0.7286698513881176,0.31924056349529956,0.4435574522679593,-0.7286698513881176,-0.7286698513881176,-0.7286698513881176,-0.5726562866782,0.24409065359764912,-0.11268181409346917,0.5365115217380095,-0.7043389497856123,-0.7043389497856123,0.08230083464281648,0.5259925179423811,0.5919253824637492,0.47308464283728663,0.5654902662927869,-0.3468939229402753,-0.8307790094987169,0.02327713182823845,-0.68429918930492,0.533544283208993,-0.42409149841041793,-0.556427723133795,0.40843771763055714,-0.3169415745101062,-0.5446315906447708,0.3703837860023442,-0.38449547786159755,-0.7132350219330628,0.4150842788785476,0.6192316453300375,0.6838479727123291,0.16127323536959343,-0.08825469695728136,-0.7043389497856123,-0.7043389497856123,-0.42423453699225283,-0.43368660492314093,0.834741976786939,0.5045912832807063,0.23192281940730186,-0.7043389497856123,-0.7579962366027759,-0.33540627735126327,0.0785668370449182,0.7310476504418107,0.4505781770143327,0.3184223504298316,-0.7043389497856123,-0.3246349137171035,-0.6138422478092181,-0.18156825980064076,0.3696234814197645,0.8101747611274231,-0.3266353471677526,-0.7043389497856123,-0.40292246305708457,-0.013064138552336353,0.5610052179853938,-0.2621649476468864,-0.18156825980064076,-0.6076436202501999,-0.7043389497856123,-0.7043389497856123,-0.4082585678097016,-0.23876694280657776,0.15966848722908106,-0.48654136597022324,-0.12868243666739113,-0.7286698513881176,-0.7286698513881176,-0.5726562866781999,-0.060874574699624706,0.7882890610639204,0.41424287333667376,-0.5726562866781999,0.018532973248883287,-0.7286698513881176,-0.2564870394750903,-0.30773917853833765,0.6682014377227015,0.2816518308690369,0.10798169546702992,-0.7286698513881176,-0.7286698513881176,-0.5781407118994097,-0.5726562866782,0.04055157443865159,-0.8315667549715154,-0.4036036763977875,-0.6302320007867649,-0.4036036763977875,0.4630294300183269,-0.8315667549715154,-0.7527852970187293,0.8392386464957226,-0.3949664444314453,-0.4799494522241494,-0.7041127724407689,-0.8315667549715154,-0.8315667549715154,-0.5791967204687432,-0.8547916897947561,-0.05846143389488504,0.8392386464957226,-0.8052170868200578,-0.7672080800443577,-0.7372010807774001,-0.41590019592802907,-0.41403933560541245,-0.3876990572676327,-0.34874291623145787,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,0.5129891760425771,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,0.7156264473321342,-0.303488489333442,-0.303488489333442,-0.28867513459481287,-0.2886751345948129,0.28284859908320203,-0.303488489333442,-0.303488489333442,0.33329681365824887,-0.7230088495575223,-0.8315667549715154,0.36710940932965913,0.3629909146998291,-1.2948719200468596e-15,0.2235350899064772,-0.8315667549715154,-0.8315667549715154,0.3409905372618307,0.8392386464957226,-0.8315667549715154,-0.6800176717858601,0.1318362059312687,0.8392386464957226,-0.8052170868200578,-0.7672080800443577,0.7251630089993119,0.8988065065157367,-0.406145710182372,-0.8067842963896242,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,0.7156264473321342,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,0.0,0.0,0.4792102259783263,0.7630954914412682,0.470095784057331,-0.652595566218312,0.295599546766101,0.3492267946354524,-0.17424412479050025,-0.03330619141602821,0.0,-0.3674452043805646,-0.303488489333442,0.2886751345948129,-0.5292186058053521,-0.4239770545082185,-0.6644105970267494,0.8263938705413374,-0.28867513459481287,-0.41598581776101917,-0.303488489333442,-0.303488489333442,-0.303488489333442,0.33329681365824887,0.3696953492131656,0.3735224769764122,-0.03642590462960279,-0.03642590462960279,-0.5687632688291246,-0.4834310993426827,0.45678815273165907,0.24338265354215455,-0.1490530251801781,0.02120949209919259,-0.7286698513881176,-0.7286698513881176,0.5859433672886679,-0.5726562866781999,-0.03642590462960279,-0.03642590462960279,0.1532071775121842,-0.7286698513881176,-0.7286698513881176,0.585943367288668,0.6724380256077493,-0.7286698513881176,-0.7286698513881176,-0.7286698513881176,0.2797576562710437,0.4162108408373745,-0.03642590462960279,-0.7107922636521499,-0.7286698513881176,-0.7286698513881176,0.40028796859424604,-0.28255960870498,0.052734475583125835,-0.7286698513881176,-0.7286698513881176,-0.7286698513881176,0.5939317140855739,0.5137411787222391,-0.303488489333442,-0.6427144893291011,0.1513757999929865,-0.303488489333442,-0.303488489333442,-0.303488489333442,-0.303488489333442,-0.303488489333442,-0.303488489333442,-0.21286964326267707,0.2421508803628691,-0.38643065086526585,-0.37871564830263355,0.35725210942669977,-0.3541780035180431,-0.6434774126957169,-0.3102711597585853,-0.27656699039023197,-0.2309434816008843,-0.7919328414182938,-0.0808717977914339,0.8235332745470355,0.3962029078465307,0.9486832980505139,0.9449111825230679,-0.38643065086526585,-0.37871564830263355,-0.36824194977451447,0.41425314679467756,-0.27656699039023197,-0.2309434816008843,-0.16836089801897483,-0.08087179779143394,0.3962029078465307,0.9486832980505139,0.9449111825230679,0.7156264473321345,-0.2581988897471611,-0.2581988897471611,0.11639789394328642,-0.2581988897471611,0.1348399724926484,0.11639789394328642,-0.2581988897471611,0.7156264473321342,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,0.5129891760425771,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,0.11639789394328642,-0.2581988897471611,0.7156264473321342,-0.2581988897471611,-0.15645958050901082,-0.2581988897471611,0.0,-0.0948947473596909,-0.3787156483026334,-0.6239753861131645,0.41425314679467756,-0.4968772264464329,-0.2309434816008843,-0.16836089801897483,-0.0808717977914339,-0.04011873063331422,0.9486832980505139,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,0.9154324272853895,0.11639789394328642,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,0.9175643839128836,-0.2581988897471611,-0.15645958050901082,0.9274495143273386,-0.2581988897471611,-0.2581988897471611,0.7216252726004873,0.7216252726004873,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,0.11639789394328642,0.7246536441906486,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,0.9154324272853895,-0.2581988897471611,0.7709385771594583,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,0.0,-0.2581988897471611,-0.7733094283532788,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.860795656933511,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.8684871746040875,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,0.9175643839128836,0.11639789394328642,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,0.0,-0.38643065086526585,0.3616303508597736,-0.36824194977451447,-0.3102711597585853,-0.27656699039023197,-0.2309434816008843,-0.16836089801897483,0.8722191392328227,0.3962029078465307,0.9486832980505139,0.9449111825230679,-0.38643065086526585,-0.5615332007886874,-0.36824194977451447,-0.4932606753487856,-0.27656699039023197,-0.2309434816008843,-0.16836089801897483,-0.6687338550904236,0.3962029078465307,0.9486832980505139,-0.6919731797614249,-0.3541780035180431,-0.2581988897471611,0.9175643839128834,-0.2581988897471611,-0.2581988897471611,-0.03419927840283847,-0.2581988897471611,0.7216252726004873,-0.15645958050901082,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,0.7156264473321345,-0.2581988897471611,0.2392567998081078,0.2389416924404502,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,0.11639789394328642,0.0,-0.2581988897471611,0.7156264473321342,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,0.2392567998081078,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,0.7081114325781411,-0.2581988897471611,-0.15645958050901082,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,0.0,-0.05563613711830076,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,0.22473328748774735,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,0.7156264473321345,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,0.685506162763685,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.03419927840283847,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,0.7700535410868202,-0.2581988897471611,0.7709385771594583,-0.2581988897471611,0.0,0.4716848065428443,-0.2581988897471611,0.9175643839128834,0.674199862463242,-0.2581988897471611,-0.2581988897471611,0.11639789394328642,-0.2581988897471611,-0.2581988897471611,0.674199862463242,0.5129891760425771,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,-0.7947847094927469,-0.2581988897471611,0.11639789394328642,-0.2581988897471611,0.9154324272853895,0.5129891760425771,0.7216252726004873,-0.021247759143566757,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,0.2392567998081078,-0.2581988897471611,-0.2581988897471611,-0.15645958050901082,-0.2581988897471611,-0.2581988897471611,0.7156264473321342,0.7559289460184544,-0.2581988897471611,-0.15645958050901082,-0.2581988897471611,-0.2581988897471611,-0.2581988897471611,0.2392567998081078,0.7429040726720494,-0.2581988897471611,0.7559289460184545,-0.2581988897471611,0.0,0.4523337897082036,-0.37871564830263355,-0.022181046693085155,-0.3201709400736422,-0.6521659817283186,-0.27656699039023197,-0.2309434816008843,-0.16836089801897483,-0.6581578772937773,0.3962029078465307,0.9486832980505139,0.9449111825230679,-1.1074327235849019e-16,0.6860089857477933,0.6860089857477933],"xaxis":"x","yaxis":"y","type":"histogram"}],                        {"template":{"data":{"histogram2dcontour":[{"type":"histogram2dcontour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"choropleth":[{"type":"choropleth","colorbar":{"outlinewidth":0,"ticks":""}}],"histogram2d":[{"type":"histogram2d","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmap":[{"type":"heatmap","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmapgl":[{"type":"heatmapgl","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"contourcarpet":[{"type":"contourcarpet","colorbar":{"outlinewidth":0,"ticks":""}}],"contour":[{"type":"contour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"surface":[{"type":"surface","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"mesh3d":[{"type":"mesh3d","colorbar":{"outlinewidth":0,"ticks":""}}],"scatter":[{"fillpattern":{"fillmode":"overlay","size":10,"solidity":0.2},"type":"scatter"}],"parcoords":[{"type":"parcoords","line":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolargl":[{"type":"scatterpolargl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"bar":[{"error_x":{"color":"#2a3f5f"},"error_y":{"color":"#2a3f5f"},"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"bar"}],"scattergeo":[{"type":"scattergeo","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolar":[{"type":"scatterpolar","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"histogram":[{"marker":{"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"histogram"}],"scattergl":[{"type":"scattergl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatter3d":[{"type":"scatter3d","line":{"colorbar":{"outlinewidth":0,"ticks":""}},"marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattermapbox":[{"type":"scattermapbox","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterternary":[{"type":"scatterternary","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattercarpet":[{"type":"scattercarpet","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"carpet":[{"aaxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"baxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"type":"carpet"}],"table":[{"cells":{"fill":{"color":"#EBF0F8"},"line":{"color":"white"}},"header":{"fill":{"color":"#C8D4E3"},"line":{"color":"white"}},"type":"table"}],"barpolar":[{"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"barpolar"}],"pie":[{"automargin":true,"type":"pie"}]},"layout":{"autotypenumbers":"strict","colorway":["#636efa","#EF553B","#00cc96","#ab63fa","#FFA15A","#19d3f3","#FF6692","#B6E880","#FF97FF","#FECB52"],"font":{"color":"#2a3f5f"},"hovermode":"closest","hoverlabel":{"align":"left"},"paper_bgcolor":"white","plot_bgcolor":"#E5ECF6","polar":{"bgcolor":"#E5ECF6","angularaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"radialaxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"ternary":{"bgcolor":"#E5ECF6","aaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"baxis":{"gridcolor":"white","linecolor":"white","ticks":""},"caxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"coloraxis":{"colorbar":{"outlinewidth":0,"ticks":""}},"colorscale":{"sequential":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"sequentialminus":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"diverging":[[0,"#8e0152"],[0.1,"#c51b7d"],[0.2,"#de77ae"],[0.3,"#f1b6da"],[0.4,"#fde0ef"],[0.5,"#f7f7f7"],[0.6,"#e6f5d0"],[0.7,"#b8e186"],[0.8,"#7fbc41"],[0.9,"#4d9221"],[1,"#276419"]]},"xaxis":{"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","automargin":true,"zerolinewidth":2},"yaxis":{"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","automargin":true,"zerolinewidth":2},"scene":{"xaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2},"yaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2},"zaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2}},"shapedefaults":{"line":{"color":"#2a3f5f"}},"annotationdefaults":{"arrowcolor":"#2a3f5f","arrowhead":0,"arrowwidth":1},"geo":{"bgcolor":"white","landcolor":"#E5ECF6","subunitcolor":"white","showland":true,"showlakes":true,"lakecolor":"white"},"title":{"x":0.05},"mapbox":{"style":"light"}}},"xaxis":{"anchor":"y","domain":[0.0,1.0],"title":{"text":"correlation"}},"yaxis":{"anchor":"x","domain":[0.0,1.0],"title":{"text":"count"}},"legend":{"tracegroupgap":0},"margin":{"t":60},"barmode":"relative"},                        {"responsive": true}                    ).then(function(){
                            
var gd = document.getElementById('eafdcd14-8725-4ecc-bac3-5bd13232f8a9');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                            </script>        </div>
</body>
</html>
</div>

</div>

</div>

</div>

</div><div class="jp-Cell jp-CodeCell jp-Notebook-cell jp-mod-noOutputs  ">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea">
<div class="jp-InputPrompt jp-InputArea-prompt">In&nbsp;[&nbsp;]:</div>
<div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline">
     <div class="CodeMirror cm-s-jupyter">
<div class=" highlight hl-python"><pre><span></span><span class="n">path</span><span class="o">=</span><span class="s2">"/content/drive/MyDrive/airfares/airfares_merged_stdgroup.csv"</span>
<span class="n">df</span><span class="o">=</span><span class="n">pd</span><span class="o">.</span><span class="n">read_csv</span><span class="p">(</span><span class="n">path</span><span class="p">)</span>
</pre></div>

     </div>
</div>
</div>
</div>

</div><div class="jp-Cell jp-CodeCell jp-Notebook-cell jp-mod-noOutputs  ">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea">
<div class="jp-InputPrompt jp-InputArea-prompt">In&nbsp;[&nbsp;]:</div>
<div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline">
     <div class="CodeMirror cm-s-jupyter">
<div class=" highlight hl-python"><pre><span></span><span class="c1"># Define the list of tuples for std ranges and their corresponding categories</span>
<span class="n">std_categories</span> <span class="o">=</span> <span class="p">[</span>
    <span class="p">((</span><span class="mi">0</span><span class="p">,</span> <span class="mf">1.03</span><span class="p">),</span> <span class="s1">'low'</span><span class="p">),</span>
    <span class="p">((</span><span class="mf">1.03</span><span class="p">,</span> <span class="mf">4.18</span><span class="p">),</span> <span class="s1">'mid'</span><span class="p">),</span>
    <span class="p">((</span><span class="mf">4.18</span><span class="p">,</span> <span class="mf">74.56</span><span class="p">),</span> <span class="s1">'high'</span><span class="p">)</span>
<span class="p">]</span>

<span class="c1"># Define the function to map age to categories using the list of tuples</span>
<span class="k">def</span> <span class="nf">categorize_std</span><span class="p">(</span><span class="n">std</span><span class="p">):</span>
    <span class="k">for</span> <span class="p">(</span><span class="n">start</span><span class="p">,</span> <span class="n">end</span><span class="p">),</span> <span class="n">category</span> <span class="ow">in</span> <span class="n">std_categories</span><span class="p">:</span>
        <span class="k">if</span> <span class="n">start</span> <span class="o">&lt;=</span> <span class="n">std</span> <span class="o">&lt;</span> <span class="n">end</span><span class="p">:</span>
            <span class="k">return</span> <span class="n">category</span>
    <span class="k">return</span> <span class="s1">'Unknown'</span>

<span class="c1"># Create a new column 'AgeGroup' by applying the function to the 'Age' column</span>
<span class="n">df</span><span class="p">[</span><span class="s1">'stdGroup'</span><span class="p">]</span> <span class="o">=</span> <span class="n">df</span><span class="p">[</span><span class="s1">'std'</span><span class="p">]</span><span class="o">.</span><span class="n">apply</span><span class="p">(</span><span class="n">categorize_std</span><span class="p">)</span>
</pre></div>

     </div>
</div>
</div>
</div>

</div><div class="jp-Cell jp-CodeCell jp-Notebook-cell jp-mod-noOutputs  ">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea">
<div class="jp-InputPrompt jp-InputArea-prompt">In&nbsp;[&nbsp;]:</div>
<div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline">
     <div class="CodeMirror cm-s-jupyter">
<div class=" highlight hl-python"><pre><span></span><span class="c1"># Define the list of tuples for std ranges and their corresponding categories</span>
<span class="n">corr_categories</span> <span class="o">=</span> <span class="p">[</span>
    <span class="p">((</span><span class="o">-</span><span class="mi">1</span><span class="p">,</span> <span class="mi">0</span><span class="p">),</span> <span class="s1">'negative'</span><span class="p">),</span>
    <span class="p">((</span><span class="mi">0</span><span class="p">,</span> <span class="mi">1</span><span class="p">),</span> <span class="s1">'positive'</span><span class="p">)</span>
<span class="p">]</span>

<span class="c1"># Define the function to map age to categories using the list of tuples</span>
<span class="k">def</span> <span class="nf">categorize_corr</span><span class="p">(</span><span class="n">correlation</span><span class="p">):</span>
    <span class="k">for</span> <span class="p">(</span><span class="n">start</span><span class="p">,</span> <span class="n">end</span><span class="p">),</span> <span class="n">category</span> <span class="ow">in</span> <span class="n">corr_categories</span><span class="p">:</span>
        <span class="k">if</span> <span class="n">start</span> <span class="o">&lt;=</span> <span class="n">correlation</span> <span class="o">&lt;</span> <span class="n">end</span><span class="p">:</span>
            <span class="k">return</span> <span class="n">category</span>
    <span class="k">return</span> <span class="s1">'Unknown'</span>

<span class="c1"># Create a new column 'trend' by applying the function to the 'correlation' column</span>
<span class="n">df</span><span class="p">[</span><span class="s1">'trend'</span><span class="p">]</span> <span class="o">=</span> <span class="n">df</span><span class="p">[</span><span class="s1">'correlation'</span><span class="p">]</span><span class="o">.</span><span class="n">apply</span><span class="p">(</span><span class="n">categorize_corr</span><span class="p">)</span>
</pre></div>

     </div>
</div>
</div>
</div>

</div><div class="jp-Cell jp-CodeCell jp-Notebook-cell   ">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea">
<div class="jp-InputPrompt jp-InputArea-prompt">In&nbsp;[&nbsp;]:</div>
<div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline">
     <div class="CodeMirror cm-s-jupyter">
<div class=" highlight hl-python"><pre><span></span><span class="n">df</span><span class="o">.</span><span class="n">head</span><span class="p">()</span>
</pre></div>

     </div>
</div>
</div>
</div>

<div class="jp-Cell-outputWrapper">
<div class="jp-Collapser jp-OutputCollapser jp-Cell-outputCollapser">
</div>


<div class="jp-OutputArea jp-Cell-outputArea">

<div class="jp-OutputArea-child">

    
    <div class="jp-OutputPrompt jp-OutputArea-prompt">Out[&nbsp;]:</div>



<div class="jp-RenderedHTMLCommon jp-RenderedHTML jp-OutputArea-output jp-OutputArea-executeResult" data-mime-type="text/html">

  <div id="df-2728d6aa-b753-4c90-9827-bd3775691148" class="colab-df-container">
    <div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>observation_date</th>
      <th>observation_time</th>
      <th>origin</th>
      <th>destination</th>
      <th>market</th>
      <th>carrier</th>
      <th>flight_number</th>
      <th>departure_date</th>
      <th>departure_time</th>
      <th>fare_basis</th>
      <th>...</th>
      <th>tax</th>
      <th>price</th>
      <th>currency</th>
      <th>departure_year</th>
      <th>departure_month</th>
      <th>days_remain</th>
      <th>correlation</th>
      <th>std</th>
      <th>stdGroup</th>
      <th>trend</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2019-01-12</td>
      <td>02:18:00</td>
      <td>RIC</td>
      <td>BOS</td>
      <td>BOSRIC</td>
      <td>C7</td>
      <td>682</td>
      <td>2019-05-05</td>
      <td>20:29:00</td>
      <td>EH0KUEN5</td>
      <td>...</td>
      <td>50.86</td>
      <td>538.3</td>
      <td>USD</td>
      <td>2019</td>
      <td>5</td>
      <td>113</td>
      <td>1.403321e-17</td>
      <td>0.0</td>
      <td>low</td>
      <td>positive</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2019-04-21</td>
      <td>01:19:00</td>
      <td>RIC</td>
      <td>BOS</td>
      <td>BOSRIC</td>
      <td>C7</td>
      <td>682</td>
      <td>2019-05-05</td>
      <td>20:29:00</td>
      <td>EH0AUEN5</td>
      <td>...</td>
      <td>50.86</td>
      <td>538.3</td>
      <td>USD</td>
      <td>2019</td>
      <td>5</td>
      <td>14</td>
      <td>1.403321e-17</td>
      <td>0.0</td>
      <td>low</td>
      <td>positive</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2019-01-11</td>
      <td>01:48:00</td>
      <td>RIC</td>
      <td>BOS</td>
      <td>BOSRIC</td>
      <td>C7</td>
      <td>682</td>
      <td>2019-05-05</td>
      <td>20:17:00</td>
      <td>EH0KUEN5</td>
      <td>...</td>
      <td>50.86</td>
      <td>538.3</td>
      <td>USD</td>
      <td>2019</td>
      <td>5</td>
      <td>114</td>
      <td>1.403321e-17</td>
      <td>0.0</td>
      <td>low</td>
      <td>positive</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2019-01-05</td>
      <td>01:42:00</td>
      <td>RIC</td>
      <td>BOS</td>
      <td>BOSRIC</td>
      <td>C7</td>
      <td>682</td>
      <td>2019-05-05</td>
      <td>20:17:00</td>
      <td>EH0KUEN5</td>
      <td>...</td>
      <td>50.86</td>
      <td>538.3</td>
      <td>USD</td>
      <td>2019</td>
      <td>5</td>
      <td>120</td>
      <td>1.403321e-17</td>
      <td>0.0</td>
      <td>low</td>
      <td>positive</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2019-04-14</td>
      <td>01:27:00</td>
      <td>RIC</td>
      <td>BOS</td>
      <td>BOSRIC</td>
      <td>C7</td>
      <td>682</td>
      <td>2019-05-05</td>
      <td>20:29:00</td>
      <td>EH0AUEN5</td>
      <td>...</td>
      <td>50.86</td>
      <td>538.3</td>
      <td>USD</td>
      <td>2019</td>
      <td>5</td>
      <td>21</td>
      <td>1.403321e-17</td>
      <td>0.0</td>
      <td>low</td>
      <td>positive</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 22 columns</p>
</div>
    <div class="colab-df-buttons">

  <div class="colab-df-container">
    <button class="colab-df-convert" onclick="convertToInteractive('df-2728d6aa-b753-4c90-9827-bd3775691148')"
            title="Convert this dataframe to an interactive table."
            style="display:none;">

  <svg xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 -960 960 960">
    <path d="M120-120v-720h720v720H120Zm60-500h600v-160H180v160Zm220 220h160v-160H400v160Zm0 220h160v-160H400v160ZM180-400h160v-160H180v160Zm440 0h160v-160H620v160ZM180-180h160v-160H180v160Zm440 0h160v-160H620v160Z"/>
  </svg>
    </button>

  <style>
    .colab-df-container {
      display:flex;
      gap: 12px;
    }

    .colab-df-convert {
      background-color: #E8F0FE;
      border: none;
      border-radius: 50%;
      cursor: pointer;
      display: none;
      fill: #1967D2;
      height: 32px;
      padding: 0 0 0 0;
      width: 32px;
    }

    .colab-df-convert:hover {
      background-color: #E2EBFA;
      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);
      fill: #174EA6;
    }

    .colab-df-buttons div {
      margin-bottom: 4px;
    }

    [theme=dark] .colab-df-convert {
      background-color: #3B4455;
      fill: #D2E3FC;
    }

    [theme=dark] .colab-df-convert:hover {
      background-color: #434B5C;
      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));
      fill: #FFFFFF;
    }
  </style>

    <script>
      const buttonEl =
        document.querySelector('#df-2728d6aa-b753-4c90-9827-bd3775691148 button.colab-df-convert');
      buttonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';

      async function convertToInteractive(key) {
        const element = document.querySelector('#df-2728d6aa-b753-4c90-9827-bd3775691148');
        const dataTable =
          await google.colab.kernel.invokeFunction('convertToInteractive',
                                                    [key], {});
        if (!dataTable) return;

        const docLinkHtml = 'Like what you see? Visit the ' +
          '<a target="_blank" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'
          + ' to learn more about interactive tables.';
        element.innerHTML = '';
        dataTable['output_type'] = 'display_data';
        await google.colab.output.renderOutput(dataTable, element);
        const docLink = document.createElement('div');
        docLink.innerHTML = docLinkHtml;
        element.appendChild(docLink);
      }
    </script>
  </div>


<div id="df-f037062a-17d3-4c2b-8ae0-9742efb53a3b">
  <button class="colab-df-quickchart" onclick="quickchart('df-f037062a-17d3-4c2b-8ae0-9742efb53a3b')"
            title="Suggest charts"
            style="display:none;">

<svg xmlns="http://www.w3.org/2000/svg" height="24px"viewBox="0 0 24 24"
     width="24px">
    <g>
        <path d="M19 3H5c-1.1 0-2 .9-2 2v14c0 1.1.9 2 2 2h14c1.1 0 2-.9 2-2V5c0-1.1-.9-2-2-2zM9 17H7v-7h2v7zm4 0h-2V7h2v10zm4 0h-2v-4h2v4z"/>
    </g>
</svg>
  </button>

<style>
  .colab-df-quickchart {
      --bg-color: #E8F0FE;
      --fill-color: #1967D2;
      --hover-bg-color: #E2EBFA;
      --hover-fill-color: #174EA6;
      --disabled-fill-color: #AAA;
      --disabled-bg-color: #DDD;
  }

  [theme=dark] .colab-df-quickchart {
      --bg-color: #3B4455;
      --fill-color: #D2E3FC;
      --hover-bg-color: #434B5C;
      --hover-fill-color: #FFFFFF;
      --disabled-bg-color: #3B4455;
      --disabled-fill-color: #666;
  }

  .colab-df-quickchart {
    background-color: var(--bg-color);
    border: none;
    border-radius: 50%;
    cursor: pointer;
    display: none;
    fill: var(--fill-color);
    height: 32px;
    padding: 0;
    width: 32px;
  }

  .colab-df-quickchart:hover {
    background-color: var(--hover-bg-color);
    box-shadow: 0 1px 2px rgba(60, 64, 67, 0.3), 0 1px 3px 1px rgba(60, 64, 67, 0.15);
    fill: var(--button-hover-fill-color);
  }

  .colab-df-quickchart-complete:disabled,
  .colab-df-quickchart-complete:disabled:hover {
    background-color: var(--disabled-bg-color);
    fill: var(--disabled-fill-color);
    box-shadow: none;
  }

  .colab-df-spinner {
    border: 2px solid var(--fill-color);
    border-color: transparent;
    border-bottom-color: var(--fill-color);
    animation:
      spin 1s steps(1) infinite;
  }

  @keyframes spin {
    0% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
      border-left-color: var(--fill-color);
    }
    20% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    30% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
      border-right-color: var(--fill-color);
    }
    40% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    60% {
      border-color: transparent;
      border-right-color: var(--fill-color);
    }
    80% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-bottom-color: var(--fill-color);
    }
    90% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
    }
  }
</style>

  <script>
    async function quickchart(key) {
      const quickchartButtonEl =
        document.querySelector('#' + key + ' button');
      quickchartButtonEl.disabled = true;  // To prevent multiple clicks.
      quickchartButtonEl.classList.add('colab-df-spinner');
      try {
        const charts = await google.colab.kernel.invokeFunction(
            'suggestCharts', [key], {});
      } catch (error) {
        console.error('Error during call to suggestCharts:', error);
      }
      quickchartButtonEl.classList.remove('colab-df-spinner');
      quickchartButtonEl.classList.add('colab-df-quickchart-complete');
    }
    (() => {
      let quickchartButtonEl =
        document.querySelector('#df-f037062a-17d3-4c2b-8ae0-9742efb53a3b button');
      quickchartButtonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';
    })();
  </script>
</div>

    </div>
  </div>

</div>

</div>

</div>

</div>

</div><div class="jp-Cell jp-CodeCell jp-Notebook-cell jp-mod-noOutputs  ">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea">
<div class="jp-InputPrompt jp-InputArea-prompt">In&nbsp;[&nbsp;]:</div>
<div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline">
     <div class="CodeMirror cm-s-jupyter">
<div class=" highlight hl-python"><pre><span></span><span class="n">merged_data</span> <span class="o">=</span> <span class="n">pd</span><span class="o">.</span><span class="n">merge</span><span class="p">(</span><span class="n">data</span><span class="p">,</span><span class="n">correlation_cleaned_1</span><span class="p">,</span> <span class="n">how</span><span class="o">=</span><span class="s1">'inner'</span><span class="p">,</span><span class="n">on</span><span class="o">=</span><span class="p">[</span><span class="s1">'carrier'</span><span class="p">,</span><span class="s1">'market'</span><span class="p">,</span><span class="s1">'flight_number'</span><span class="p">,</span><span class="s1">'origin'</span><span class="p">,</span><span class="s1">'destination'</span><span class="p">,</span><span class="s1">'departure_date'</span><span class="p">,</span><span class="s1">'booking_class'</span><span class="p">])</span>
</pre></div>

     </div>
</div>
</div>
</div>

</div><div class="jp-Cell jp-CodeCell jp-Notebook-cell jp-mod-noOutputs  ">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea">
<div class="jp-InputPrompt jp-InputArea-prompt">In&nbsp;[&nbsp;]:</div>
<div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline">
     <div class="CodeMirror cm-s-jupyter">
<div class=" highlight hl-python"><pre><span></span><span class="n">merged_data_new</span> <span class="o">=</span> <span class="n">pd</span><span class="o">.</span><span class="n">merge</span><span class="p">(</span><span class="n">merged_data</span><span class="p">,</span><span class="n">grouped_data</span><span class="p">,</span> <span class="n">how</span><span class="o">=</span><span class="s1">'inner'</span><span class="p">,</span><span class="n">on</span><span class="o">=</span><span class="p">[</span><span class="s1">'carrier'</span><span class="p">,</span><span class="s1">'market'</span><span class="p">,</span><span class="s1">'flight_number'</span><span class="p">,</span><span class="s1">'origin'</span><span class="p">,</span><span class="s1">'destination'</span><span class="p">,</span><span class="s1">'departure_date'</span><span class="p">,</span><span class="s1">'booking_class'</span><span class="p">])</span>
</pre></div>

     </div>
</div>
</div>
</div>

</div><div class="jp-Cell jp-CodeCell jp-Notebook-cell jp-mod-noOutputs  ">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea">
<div class="jp-InputPrompt jp-InputArea-prompt">In&nbsp;[&nbsp;]:</div>
<div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline">
     <div class="CodeMirror cm-s-jupyter">
<div class=" highlight hl-python"><pre><span></span><span class="n">merged_data_new</span><span class="o">.</span><span class="n">info</span><span class="p">()</span>
</pre></div>

     </div>
</div>
</div>
</div>

</div><div class="jp-Cell jp-CodeCell jp-Notebook-cell jp-mod-noOutputs  ">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea">
<div class="jp-InputPrompt jp-InputArea-prompt">In&nbsp;[&nbsp;]:</div>
<div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline">
     <div class="CodeMirror cm-s-jupyter">
<div class=" highlight hl-python"><pre><span></span><span class="n">merged_data_new</span> <span class="o">=</span> <span class="n">merged_data_new</span><span class="o">.</span><span class="n">rename</span><span class="p">(</span><span class="n">columns</span><span class="o">=</span><span class="p">{</span><span class="s1">'price_x'</span><span class="p">:</span> <span class="s1">'price'</span><span class="p">,</span> <span class="s1">'price_y'</span><span class="p">:</span> <span class="s1">'std'</span><span class="p">})</span>
</pre></div>

     </div>
</div>
</div>
</div>

</div><div class="jp-Cell jp-CodeCell jp-Notebook-cell jp-mod-noOutputs  ">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea">
<div class="jp-InputPrompt jp-InputArea-prompt">In&nbsp;[&nbsp;]:</div>
<div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline">
     <div class="CodeMirror cm-s-jupyter">
<div class=" highlight hl-python"><pre><span></span><span class="n">df</span><span class="o">.</span><span class="n">to_csv</span><span class="p">(</span><span class="s1">'/content/drive/MyDrive/airfares/airfares_merged_stdgroup.csv'</span><span class="p">,</span><span class="n">index</span><span class="o">=</span><span class="kc">False</span><span class="p">)</span>
</pre></div>

     </div>
</div>
</div>
</div>

</div><div class="jp-Cell jp-CodeCell jp-Notebook-cell jp-mod-noOutputs  ">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea">
<div class="jp-InputPrompt jp-InputArea-prompt">In&nbsp;[&nbsp;]:</div>
<div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline">
     <div class="CodeMirror cm-s-jupyter">
<div class=" highlight hl-python"><pre><span></span>
</pre></div>

     </div>
</div>
</div>
</div>

</div><div class="jp-Cell jp-CodeCell jp-Notebook-cell jp-mod-noOutputs  ">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea">
<div class="jp-InputPrompt jp-InputArea-prompt">In&nbsp;[&nbsp;]:</div>
<div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline">
     <div class="CodeMirror cm-s-jupyter">
<div class=" highlight hl-python"><pre><span></span><span class="n">path</span><span class="o">=</span><span class="s2">"/content/drive/MyDrive/airfares/airfares_merged_stdgroup.csv"</span>
<span class="n">df</span><span class="o">=</span><span class="n">pd</span><span class="o">.</span><span class="n">read_csv</span><span class="p">(</span><span class="n">path</span><span class="p">)</span>
</pre></div>

     </div>
</div>
</div>
</div>

</div><div class="jp-Cell jp-CodeCell jp-Notebook-cell jp-mod-noOutputs  ">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea">
<div class="jp-InputPrompt jp-InputArea-prompt">In&nbsp;[&nbsp;]:</div>
<div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline">
     <div class="CodeMirror cm-s-jupyter">
<div class=" highlight hl-python"><pre><span></span><span class="n">half_df</span> <span class="o">=</span> <span class="n">df</span><span class="o">.</span><span class="n">sample</span><span class="p">(</span><span class="n">frac</span><span class="o">=</span><span class="mf">0.5</span><span class="p">,</span> <span class="n">random_state</span><span class="o">=</span><span class="mi">1</span><span class="p">)</span>
</pre></div>

     </div>
</div>
</div>
</div>

</div><div class="jp-Cell jp-CodeCell jp-Notebook-cell jp-mod-noOutputs  ">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea">
<div class="jp-InputPrompt jp-InputArea-prompt">In&nbsp;[&nbsp;]:</div>
<div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline">
     <div class="CodeMirror cm-s-jupyter">
<div class=" highlight hl-python"><pre><span></span><span class="n">half_df</span><span class="o">.</span><span class="n">to_csv</span><span class="p">(</span><span class="s1">'/content/drive/MyDrive/airfares/airfares_dash_sample.csv'</span><span class="p">,</span><span class="n">index</span><span class="o">=</span><span class="kc">False</span><span class="p">)</span>
</pre></div>

     </div>
</div>
</div>
</div>

</div><div class="jp-Cell jp-CodeCell jp-Notebook-cell   ">
<div class="jp-Cell-inputWrapper">
<div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser">
</div>
<div class="jp-InputArea jp-Cell-inputArea">
<div class="jp-InputPrompt jp-InputArea-prompt">In&nbsp;[&nbsp;]:</div>
<div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline">
     <div class="CodeMirror cm-s-jupyter">
<div class=" highlight hl-python"><pre><span></span><span class="n">half_df</span><span class="o">.</span><span class="n">head</span><span class="p">()</span>
</pre></div>

     </div>
</div>
</div>
</div>

<div class="jp-Cell-outputWrapper">
<div class="jp-Collapser jp-OutputCollapser jp-Cell-outputCollapser">
</div>


<div class="jp-OutputArea jp-Cell-outputArea">

<div class="jp-OutputArea-child">

    
    <div class="jp-OutputPrompt jp-OutputArea-prompt">Out[&nbsp;]:</div>



<div class="jp-RenderedHTMLCommon jp-RenderedHTML jp-OutputArea-output jp-OutputArea-executeResult" data-mime-type="text/html">

  <div id="df-2c8a3dd2-fc56-4f8a-a80f-97b94649ec4c" class="colab-df-container">
    <div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>observation_date</th>
      <th>observation_time</th>
      <th>origin</th>
      <th>destination</th>
      <th>market</th>
      <th>carrier</th>
      <th>flight_number</th>
      <th>departure_date</th>
      <th>departure_time</th>
      <th>fare_basis</th>
      <th>...</th>
      <th>tax</th>
      <th>price</th>
      <th>currency</th>
      <th>departure_year</th>
      <th>departure_month</th>
      <th>days_remain</th>
      <th>correlation</th>
      <th>std</th>
      <th>stdGroup</th>
      <th>trend</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>168253</th>
      <td>2019-04-20</td>
      <td>04:09:00</td>
      <td>BOS</td>
      <td>LGA</td>
      <td>BOSLGA</td>
      <td>EM</td>
      <td>5910</td>
      <td>2019-05-31</td>
      <td>16:00:00</td>
      <td>XAVNA0BP</td>
      <td>...</td>
      <td>19.04</td>
      <td>82.3</td>
      <td>USD</td>
      <td>2019</td>
      <td>5</td>
      <td>41</td>
      <td>0.697699</td>
      <td>31.245866</td>
      <td>high</td>
      <td>positive</td>
    </tr>
    <tr>
      <th>77255</th>
      <td>2019-02-11</td>
      <td>04:45:00</td>
      <td>LGA</td>
      <td>BOS</td>
      <td>BOSLGA</td>
      <td>EM</td>
      <td>1969</td>
      <td>2019-05-03</td>
      <td>07:00:00</td>
      <td>VAVNA0BP</td>
      <td>...</td>
      <td>19.04</td>
      <td>82.3</td>
      <td>USD</td>
      <td>2019</td>
      <td>5</td>
      <td>81</td>
      <td>-0.562369</td>
      <td>18.280267</td>
      <td>high</td>
      <td>negative</td>
    </tr>
    <tr>
      <th>81246</th>
      <td>2019-03-02</td>
      <td>05:31:00</td>
      <td>BOS</td>
      <td>LGA</td>
      <td>BOSLGA</td>
      <td>EM</td>
      <td>5910</td>
      <td>2019-08-05</td>
      <td>16:00:00</td>
      <td>UA7UA0BP</td>
      <td>...</td>
      <td>23.51</td>
      <td>146.3</td>
      <td>USD</td>
      <td>2019</td>
      <td>8</td>
      <td>156</td>
      <td>0.155569</td>
      <td>18.849048</td>
      <td>high</td>
      <td>positive</td>
    </tr>
    <tr>
      <th>44932</th>
      <td>2019-02-23</td>
      <td>04:46:00</td>
      <td>LGA</td>
      <td>BOS</td>
      <td>BOSLGA</td>
      <td>EM</td>
      <td>5996</td>
      <td>2019-05-16</td>
      <td>08:59:00</td>
      <td>LA7VA0BP</td>
      <td>...</td>
      <td>25.25</td>
      <td>171.3</td>
      <td>USD</td>
      <td>2019</td>
      <td>5</td>
      <td>82</td>
      <td>0.877287</td>
      <td>35.374386</td>
      <td>high</td>
      <td>positive</td>
    </tr>
    <tr>
      <th>142664</th>
      <td>2019-03-23</td>
      <td>05:25:00</td>
      <td>BOS</td>
      <td>LGA</td>
      <td>BOSLGA</td>
      <td>EM</td>
      <td>1929</td>
      <td>2019-08-05</td>
      <td>06:00:00</td>
      <td>XAVNA0BP</td>
      <td>...</td>
      <td>19.04</td>
      <td>82.3</td>
      <td>USD</td>
      <td>2019</td>
      <td>8</td>
      <td>135</td>
      <td>-0.293791</td>
      <td>5.471051</td>
      <td>high</td>
      <td>negative</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 22 columns</p>
</div>
    <div class="colab-df-buttons">

  <div class="colab-df-container">
    <button class="colab-df-convert" onclick="convertToInteractive('df-2c8a3dd2-fc56-4f8a-a80f-97b94649ec4c')"
            title="Convert this dataframe to an interactive table."
            style="display:none;">

  <svg xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 -960 960 960">
    <path d="M120-120v-720h720v720H120Zm60-500h600v-160H180v160Zm220 220h160v-160H400v160Zm0 220h160v-160H400v160ZM180-400h160v-160H180v160Zm440 0h160v-160H620v160ZM180-180h160v-160H180v160Zm440 0h160v-160H620v160Z"/>
  </svg>
    </button>

  <style>
    .colab-df-container {
      display:flex;
      gap: 12px;
    }

    .colab-df-convert {
      background-color: #E8F0FE;
      border: none;
      border-radius: 50%;
      cursor: pointer;
      display: none;
      fill: #1967D2;
      height: 32px;
      padding: 0 0 0 0;
      width: 32px;
    }

    .colab-df-convert:hover {
      background-color: #E2EBFA;
      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);
      fill: #174EA6;
    }

    .colab-df-buttons div {
      margin-bottom: 4px;
    }

    [theme=dark] .colab-df-convert {
      background-color: #3B4455;
      fill: #D2E3FC;
    }

    [theme=dark] .colab-df-convert:hover {
      background-color: #434B5C;
      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));
      fill: #FFFFFF;
    }
  </style>

    <script>
      const buttonEl =
        document.querySelector('#df-2c8a3dd2-fc56-4f8a-a80f-97b94649ec4c button.colab-df-convert');
      buttonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';

      async function convertToInteractive(key) {
        const element = document.querySelector('#df-2c8a3dd2-fc56-4f8a-a80f-97b94649ec4c');
        const dataTable =
          await google.colab.kernel.invokeFunction('convertToInteractive',
                                                    [key], {});
        if (!dataTable) return;

        const docLinkHtml = 'Like what you see? Visit the ' +
          '<a target="_blank" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'
          + ' to learn more about interactive tables.';
        element.innerHTML = '';
        dataTable['output_type'] = 'display_data';
        await google.colab.output.renderOutput(dataTable, element);
        const docLink = document.createElement('div');
        docLink.innerHTML = docLinkHtml;
        element.appendChild(docLink);
      }
    </script>
  </div>


<div id="df-335ee4ef-61c7-472c-abb1-510fa3a55ad3">
  <button class="colab-df-quickchart" onclick="quickchart('df-335ee4ef-61c7-472c-abb1-510fa3a55ad3')"
            title="Suggest charts"
            style="display:none;">

<svg xmlns="http://www.w3.org/2000/svg" height="24px"viewBox="0 0 24 24"
     width="24px">
    <g>
        <path d="M19 3H5c-1.1 0-2 .9-2 2v14c0 1.1.9 2 2 2h14c1.1 0 2-.9 2-2V5c0-1.1-.9-2-2-2zM9 17H7v-7h2v7zm4 0h-2V7h2v10zm4 0h-2v-4h2v4z"/>
    </g>
</svg>
  </button>

<style>
  .colab-df-quickchart {
      --bg-color: #E8F0FE;
      --fill-color: #1967D2;
      --hover-bg-color: #E2EBFA;
      --hover-fill-color: #174EA6;
      --disabled-fill-color: #AAA;
      --disabled-bg-color: #DDD;
  }

  [theme=dark] .colab-df-quickchart {
      --bg-color: #3B4455;
      --fill-color: #D2E3FC;
      --hover-bg-color: #434B5C;
      --hover-fill-color: #FFFFFF;
      --disabled-bg-color: #3B4455;
      --disabled-fill-color: #666;
  }

  .colab-df-quickchart {
    background-color: var(--bg-color);
    border: none;
    border-radius: 50%;
    cursor: pointer;
    display: none;
    fill: var(--fill-color);
    height: 32px;
    padding: 0;
    width: 32px;
  }

  .colab-df-quickchart:hover {
    background-color: var(--hover-bg-color);
    box-shadow: 0 1px 2px rgba(60, 64, 67, 0.3), 0 1px 3px 1px rgba(60, 64, 67, 0.15);
    fill: var(--button-hover-fill-color);
  }

  .colab-df-quickchart-complete:disabled,
  .colab-df-quickchart-complete:disabled:hover {
    background-color: var(--disabled-bg-color);
    fill: var(--disabled-fill-color);
    box-shadow: none;
  }

  .colab-df-spinner {
    border: 2px solid var(--fill-color);
    border-color: transparent;
    border-bottom-color: var(--fill-color);
    animation:
      spin 1s steps(1) infinite;
  }

  @keyframes spin {
    0% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
      border-left-color: var(--fill-color);
    }
    20% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    30% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
      border-right-color: var(--fill-color);
    }
    40% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    60% {
      border-color: transparent;
      border-right-color: var(--fill-color);
    }
    80% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-bottom-color: var(--fill-color);
    }
    90% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
    }
  }
</style>

  <script>
    async function quickchart(key) {
      const quickchartButtonEl =
        document.querySelector('#' + key + ' button');
      quickchartButtonEl.disabled = true;  // To prevent multiple clicks.
      quickchartButtonEl.classList.add('colab-df-spinner');
      try {
        const charts = await google.colab.kernel.invokeFunction(
            'suggestCharts', [key], {});
      } catch (error) {
        console.error('Error during call to suggestCharts:', error);
      }
      quickchartButtonEl.classList.remove('colab-df-spinner');
      quickchartButtonEl.classList.add('colab-df-quickchart-complete');
    }
    (() => {
      let quickchartButtonEl =
        document.querySelector('#df-335ee4ef-61c7-472c-abb1-510fa3a55ad3 button');
      quickchartButtonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';
    })();
  </script>
</div>

    </div>
  </div>

</div>

</div>

</div>

</div>

</div>
</body>







</html>


{% endraw %}
