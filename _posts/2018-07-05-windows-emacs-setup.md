---
layout: post
title: Windows emacs setup
description:
date: 2018-07-05 12:40:04
category:
tags: ['lisp', 'cepl']
---

My mate was looking at using emacs on windows and so I wrote a little 'how to'. I then added my own opinions of stuff to start with so it got bigger. Made sense to just dump it here:

## Note

I'm probably out of date! I just saw you can install emacs via pacman in msys which might handle a bunch of the 'path' & environment variable shit for you. I'm going to look into this and then update this guide.


### Install
- download this chap <http://ftp.gnu.org/gnu/emacs/windows/emacs-26/emacs-26.1-x86_64.zip>
- There is no install process so just extract it to c:\ so you have a folder like c:\emacs-25-1
- open the c:\emacs-**-*\bin folder
- copy the path to that directory
- add it to the PATH environment variable
- make a folder called 'home' folder in c:\ (or if you have one for msys skip this step). I do this as twice I've had windows shit up after updates and tell me my account isnt mine anymore and I hate fighting that shit
- Add a environemnt varable called HOME and set it to your new (or mysys) 'home' directory
- Basic setup is done but there are some things you will want.

### Package manager

Dont download packages yourself, keeping them current is a pain, emacs comes with a package manager but the official package source is a little behind the times so we will add one that is much more used by the community 'melpa'

See this video for how to use and install it: <https://www.youtube.com/watch?v=Cf6tRBPbWKs>

Short version is:
- hit 'M-x' [0] which will open the minibuffer at the bottom of emacs, type 'customize' and hit return.
- Type 'package' in the search bar and hit return.
- Scoot your cursor down to the arrow next to 'package archives' and hit return to open that subtree
- hit return on the 'INS' button and add 'melpa' as the archive name and 'http://melpa.org/packages/' as the url
- 'C-x C-s' to save (which means) hold down control and press 'x' then 's'

This will have edited your .emacs file in your 'home' directory, this is nice as lots of packages use the customize system and its often friendlier than searching docs for the right thing to edit.

### Magit (reticently optional)
- restart emacs (rarely neccessary but I want to be sure evertyhing is fresh) and hit 'M-x' and type 'list-packages' and hit return
- give it a second to pull the package list and then install 'magit' (see that video for a guide of how to do that)
- This is the nicest damn git client, I'd install emacs for this even if I wasnt using it as a text editor.

### Biased Baggers .emacs file additions (optional)

Open your .emacs file and paste the following at the top of the file

It looks like a lot just its just shit I've slowly accrued whilst using emacs.

```
;; so package manager is always good to go
(package-initialize)

;; utf8 is a good default
(setenv "LANG" "en_US.UTF-8")

;; when you start looking for files its nice to start in home
(setq default-directory "~/")

;; Using msys?
;; to make sure we can use git and stuff like that from emacs
(setenv "PATH"
  (concat
   ;; Change this with your path to MSYS bin directory
   "C:\\msys64\\usr\\bin;"
   (getenv "PATH")))

;; Fuck that bell
(setq ring-bell-function #'ignore)

;; Turn off the menu bars, embrace the keys :p
(tool-bar-mode -1)
(menu-bar-mode -1)

;; Dont need emacs welcome in your face at every start
(setq inhibit-splash-screen t)

;; typing out 'yes' and 'no' sucks, use 'y' and 'n'
(fset `yes-or-no-p `y-or-n-p)

;; Highlight matching paren
(show-paren-mode t)

;; This might be out of date now, need to ask kristian
(setq column-number-mode t)

;; You can now used meta+arrow-keys to move between split windows
(windmove-default-keybindings)

;; Kill that bloody insert key
(global-set-key [insert] 'ignore)

;; Stop shift mouse click opening the font window
(global-set-key [(shift down-mouse-1)] 'ignore)
(global-set-key [(control down-mouse-1)] 'ignore)

;; C-c C-g now runs 'git status' in magit. Super handy
(global-set-key (kbd "\C-c \C-g") `magit-status)

;; Jump to matching paren. A touch hacky but I nabbed it from somewhere and has worked well enough for my stuff
;; probably something better out there though (this is language independent though).
;; Move to one bracket and hit 'Control )' to jump to the other bracket
(defun goto-match-paren (arg)
  "Go to the matching  if on (){}[], similar to vi style of %"
  (interactive "p")
  ;; first, check for "outside of bracket" positions expected by forward-sexp, etc.
  (cond ((looking-at "[\[\(\{]") (forward-sexp))
        ((looking-back "[\]\)\}]" 1) (backward-sexp))
        ;; now, try to succeed from inside of a bracket
        ((looking-at "[\]\)\}]") (forward-char) (backward-sexp))
        ((looking-back "[\[\(\{]" 1) (backward-char) (forward-sexp))
        (t nil)))
(global-set-key (kbd "C-c )") `goto-match-paren)
```

### ssh-agent hack (only needed if you have the issue I did)

Windows is a pita with some of this stuff. I have emacs on machine using the bash provided with git rather than a dedicate msys or whatever install and I got confused some ssh-agent issues. So I just made a windows shortcut that runs emacs from git bash and that helped. I get propted for my ssh-agent password on launch (which I do once a day) and then im free to work.

The shortcut just pointed to: `"C:\Program Files\Git\git-bash.exe" -c "emacs-25.2"`

### Control Key (optional)

The control key is in the wrong place and it's not good for the hand to keep having to reach for it. Let's make capslock an extra control key.

make a file called `control.reg` and paste this in it

```
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Keyboard Layout]
"Scancode Map"=hex:00,00,00,00,00,00,00,00,02,00,00,00,1d,00,3a,00,00,00,00,00
```

Save it, run it and restart windows for it to take effect.

### Done (optional :p)

Save and restart emacs again. Hopefully there are no errors (bug me if there are)

Remember that emacs is your editor, nothing is too stupid if it makes your experience better. For example I kept mistyping certain key combos so I bound the things I kept hitting instead to the same functions, it's small but it makes me faster.

Thats all for now, seeya!

[0] (M stands for meta and is the alt key [the naming comes from the old [lisp machine keyboards iirc](https://pbs.twimg.com/media/C942BKSU0AE7p0q.jpg))
