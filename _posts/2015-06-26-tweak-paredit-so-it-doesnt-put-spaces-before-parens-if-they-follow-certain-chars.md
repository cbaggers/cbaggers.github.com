---
layout: post
title: Tweak paredit so it doesnt put spaces before parens if they follow certain chars
description:
date: 2015-06-26 16:47:58
category:
tags: ['lisp', 'cepl']
---

Quickly hacked together to fix fixes a fustraction where trying to write `#a(1 2 3)` gets 'corrected' by paredit to `#a (1 2 3)`

    (defvar paredit-dont-add-space-after
      '((?s ?#) (?a ?#) (?λ) (?- ?#) (?+ ?#) (?c ?#) (?o ?#) (?p ?#) (?r ?#) (?s ?#) (?x ?#)))

    (add-hook 'paredit-mode-hook
              (lambda ()
                (setq paredit-space-for-delimiter-predicates
                      (list (lambda (endp delimiter)
                              (if (not endp)
                                  (save-excursion
                                    (let ((1-back (char-before))
                                          (2-back (char-before (- (point) 1))))
                                      (null
                                       (cl-loop for (1b 2b) in paredit-dont-add-space-after thereis
                                                (and 1-back (char-equal 1b 1-back)
                                                     (if 2b
                                                         (when 2-back
                                                           (char-equal 2b 2-back))
                                                       t))))))
                                t))))))
