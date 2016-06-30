---
layout: post
title: My Atom packages and stylesheet
date: '2015-12-20T04:44:05+08:00'
tags:
- atom
- package
- list
- customize
- stylesheet
tumblr_url: http://hi-tips.tumblr.com/post/135567258156/my-atom-packages-and-stylesheet
---
Here’s my atom packages.
The complete list will at the end. A lot of murmuring :p.


```
Sublime-Style-Column-Selection(sublime-style Alt + mouse drag multi-line selection, suprisingly not a built-in function in Atom)
Zen(content focus writing)
atom-notifier(for sending native notification)
atom-spotified(display spotify cover/artist info)
block-travel(traversing between code blocks)
color-picker(BJ4)
docblockr(for block commenting)
git-plus(using git command from command palette)
jumpy(easy motion alternative, which was deprecated and quicker in my thoughs)
merge-conflicts(a great interface for merge conflicts)
native-ui(my currently using theme, integrated greatly on OSX El Capitan)
nucleus-dark-ui(a great dark alternative theme)
oceanic-next(my favorite syntax theme)
pigments(for display colors)
set-syntax(Ctrl-P ss, we came from sublime :p)
terminal-plus(beautifully integrated shell into Atom)
trailing-spaces(for display trailing spaces)
vim-mode(BJ4)
minimap and its enhancements.
minimap
minimap-cursorline
minimap-find-and-replace
minimap-git-diff
minimap-highlight-selected
minimap-pigments
minimap-selection
A toolbar. Basically I use command palette more frequently. But it still looks great.
tool-bar
tool-bar-almighty
Not as good as AlignTab(sublime version with regex support), but still useful.
aligner
aligner-css
aligner-ruby
aligner-scss
And stylesheet as follows. Only slightly changes. 
```

```css
.tree-view {
  // config tree-view font
  font-family: InputSansCompressed-Regular !important;
  font-size: 12px;

  // tree view animations via https://github.com/atom/tree-view/issues/358
  .entry.expanded > .entries {
    -webkit-animation: tree-view-expanded .3s cubic-bezier(.15,.7,.3, 1);
  }
  @-webkit-keyframes tree-view-expanded {
    0% { opacity: 0; transform: translateY(-10px); }
    100% { opacity: 1; transform: translateY(0); }
  }
  .list-tree.has-collapsable-children .list-nested-item.collapsed > .list-tree {
    display: none;
  }

  // change repo font
  .name.icon.icon-repo {
    font-family: Oswald-Regular;
    letter-spacing: 1.5px;
  }
}


// always show foldable toggle icon
atom-text-editor::shadow .gutter .line-number.foldable .icon-right {
visibility: visible;
}

// change wrap-guide color and made it barely visible
atom-text-editor::shadow .wrap-guide {
  opacity: .12;
  background-color: cyan;
}


// customize status-bar git button
status-bar.status-bar .git-view {
  background-color: #cfcdcf;

  &:hover {
    background-color: #b3b1b3;
    cursor: pointer;
  }

  span.icon.icon-git-branch {
    color: #494949;
  }

  span.branch-label {
    color: #494949;
  }

  .status-modified {
    color: rgb(222, 138, 40);
  }
}

// change trailing spaces color
atom-text-editor::shadow .lines .line .trailing-whitespace.trailing-whitespace {
  background-color: #54626a;
}
```


A complete list of my packages. Use can use

  apm install package1 package2 package3

quickly install them.

```
  ├── Sublime-Style-Column-Selection@1.3.0
  ├── Zen@0.16.3
  ├── advanced-new-file@0.5.0
  ├── advanced-open-file@0.13.0
  ├── aligner@0.17.4
  ├── aligner-css@1.2.0
  ├── aligner-ruby@1.4.0
  ├── aligner-scss@1.1.0
  ├── atom-flat-ui@1.1.0
  ├── atom-material-ui@0.8.0
  ├── atom-notifier@0.4.5
  ├── atom-spotified@0.1.2
  ├── auto-detect-indentation@0.4.2
  ├── autocomplete-ruby@0.1.0
  ├── base16-ocean-dark-spacegray@0.12.0
  ├── block-travel@1.0.4
  ├── browser-plus@0.0.54
  ├── color-picker@2.0.14
  ├── dash@1.5.0
  ├── docblockr@0.7.3
  ├── file-icon-supplement@0.8.3
  ├── file-icons@1.6.13
  ├── file-types@0.5.1
  ├── flatty-syntax@0.1.0
  ├── git-plus@5.7.0
  ├── hyperclick@0.0.35
  ├── idiomatic-dark-syntax@0.1.3
  ├── js-hyperclick@1.4.1
  ├── jumpy@2.0.10
  ├── lambda-ui@0.11.1
  ├── language-markdown@0.7.1
  ├── language-swift@0.4.0
  ├── linter@1.11.3
  ├── linter-ruby@1.2.0
  ├── markdown-pdf@1.3.9
  ├── markdown-preview-plus@2.2.2
  ├── markdown-scroll-sync@2.0.3
  ├── markdown-table-formatter@2.7.1
  ├── markdown-toc@0.4.0
  ├── merge-conflicts@1.3.7
  ├── minimap@4.18.2
  ├── minimap-cursorline@0.1.0
  ├── minimap-find-and-replace@4.4.0
  ├── minimap-git-diff@4.1.8
  ├── minimap-highlight-selected@4.3.1
  ├── minimap-pigments@0.1.7
  ├── minimap-selection@4.3.1
  ├── monokai@0.18.0
  ├── native-ui@0.11.0
  ├── neutron-ui@0.4.0
  ├── nucleus-dark-ui@0.7.0
  ├── oceanic-next@0.1.2
  ├── oceanic-theme@0.1.2
  ├── pigments@0.19.3
  ├── project-manager@2.6.5
  ├── rails-latest-migration@1.1.4
  ├── rails-partials@0.8.1
  ├── rails-snippets@2.0.0
  ├── react@0.12.10
  ├── ruby-block@0.3.5
  ├── ruby-slim@0.2.0
  ├── set-syntax@0.3.0
  ├── seti-syntax@0.4.1
  ├── seti-ui@0.8.1
  ├── slime@1.4.0
  ├── spacegray-dark-neue-syntax@1.3.0
  ├── spacegray-dark-neue-ui@1.4.0
  ├── spacegray-eighties-ui@1.2.0
  ├── spacegray-mocha-ui@1.0.0
  ├── terminal-plus@0.14.5
  ├── tidy-markdown@1.0.0
  ├── tool-bar@0.1.9
  ├── tool-bar-almighty@0.5.0
  ├── trailing-spaces@0.3.2
  ├── unicorn-syntax@2.2.0
  ├── unity-dark-ui@2.0.9
  ├── unity-ui@2.1.3
  ├── vim-mode@0.64.0
  └── yosemite-unity-ui@0.3.13
```
