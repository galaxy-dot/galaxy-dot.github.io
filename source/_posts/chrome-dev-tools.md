---
title: Chrome Dev Tools
date: 2022-08-13
categories: productivity
tags:
  - chrome dev tools
---

Some notes on Chrome Dev Tools. <!-- more -->

## CSS

`computed attributes` can check all css rules applied to one element and is overwritten, and the highest specificity.

## Add Break Point to HTML

Right click on an html element -> break on ->

```
subtree
attribute
node removal
```

## Event Listeners

Check any JS/React event listeners to an element.

## Workspace

Connect local files to `source` tab, make sure files added have `green dot` to persist changes between page reloads.

Click `sources`, open `Filesystem`, click `Add folder to workplace`, choose the folder where to save local changes, allow Chrome access. Make change to the css and it will reflect the changes after page refresh.

## Debug

Several different types of breakpoints.

- Breakpoints
- XHR/fetch Breakpoints, break on only some specific network calls
- Scope
  - Local, where breakpoint lives
  - Global
- Callback 回调
- Blackbox script 不显示某些 callback
- Watch: watch some variables, variables value will be updated.

## Network Performance

1. Click `shift` when hover over networks calls, will highlight `red` or `green` on which triggers and which being triggered
2. Waterfall

## Performance API

[Performance](https://developer.mozilla.org/en-US/docs/Web/API/Performance)

- performance.start()
- performance.end()
- page jank
- Rail rule: Response, Animation, Idle and Load
- Use Webworker to run heavy calculations

## Chrome Task Manager

Right click `More tools`, right click and choose `JavaScript Memory`

## Extra selection to console

Right click `three dots`, choose `Rendering`, click `Paint flashing`, `Frame Rendering Stats`

## node

`node --inspect cmd cmd1`

## Refs

1. [Chrome Dev Tools Doc](https://developer.chrome.com/docs/devtools/)
