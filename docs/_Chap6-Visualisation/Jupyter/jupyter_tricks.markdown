---
title: Jupyter Notebook Tricks
author: Lei Lei
category: notes
layout: post
---

## Hotkeys

### Command mode (press Esc to enable)

| Enter | enter edit mode |
| ----- | ----- |
| Shift+­Enter | run cell, select below |
| Ctrl+Enter | run cell |
| Alt+Enter | run cell, insert below |
| Y | to code |
| M | to markdown |
| R | to raw |
| 1 | to heading 1 |
| 2,3,4,5,6 | to heading 2,3,4,5,6 |
| Up/K | select cell above |
| Down/J | select cell below |
| A/B | insert cell above/­below |
| X | cut selected cell |
| C | copy selected cell |
| Shift+V | paste cell above |
| V | paste cell below |
| Z | undo last cell deletion |
| D | delete selected cell |
| Shift+M | merge cell below |
| Ctrl+S | Save and Checkpoint |
| L | toggle line numbers |
| O | toggle output |
| Shift+O | toggle output scrolling |
| Esc | close pager |
| H | show keyboard shortcut help dialog |
| I | interrupt kernel |
| 0 | restart kernel |
| Space | scroll down |
| Shift+­Space | scroll up |
| Shift | ignore |

### Edit Mode (press Enter to enable)

| Tab | code completion or indent |
| ----- | ----- |
| Shift+Tab | tooltip |
| Ctrl+\] | indent |
| Ctrl+\[ | dedent |
| Ctrl+A | select all |
| Ctrl+Z | undo |
| Ctrl+S­hift+Z | redo |
| Ctrl+Y | redo |
| Ctrl+Home | go to cell start |
| Ctrl+Up | go to cell start |
| Ctrl+End | go to cell end |
| Ctrl+Down | go to cell end |
| Ctrl+Left | go one word left |
| Ctrl+Right | go one word right |
| Ctrl+B­ack­space | delete word before |
| Ctrl+D­elete | delete word after |
| Esc | command mode |
| Ctrl+M | command mode |
| Ctrl+S­hift+minus | split cell |
| Ctrl+S | Save and Checkpoint |
| Up | move cursor up or previous cell |
| Down | move cursor down or next cell |
| Ctrl+/ | toggle comment on current or selected lines |

## Recover deleted notebooks

Retrieve your Ipython history by writting into a file:

```
%history -g -f filename 
```

Check (would be the end part if you just deleted it) the file: `filename` contains all scripts you've run.