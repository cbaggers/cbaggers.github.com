---
layout: post
title: Eval into file
description:
date: 2015-01-08 00:28:51
category:
tags: ['lisp', 'slime']
---

Just wrote a handy function for working with slime.

* It takes the top level form at that point e.g. `(make-instance 'test)`
* Wraps it in a defparameter with a new var name `(defparameter <iv-0> (make-instance 'test))`
* evals it using slime and injects the var name (in this case `<iv-0>`) into the file just after the toplevel form

This is useful when sketching out ideas in a file and you want to compile something but also capture the result in a global for messing with later

running it 3 times would give you something like 

    (make-instance 'test)
    <iv-2> <iv-1> <iv-0>

which makes it very easy to wrap a list around and throw it where you see fit.


    (defvar eval-into-file-count 0)
    (defun slime-eval-into-file ()
      "Evaluate the current toplevel form.
    store the result in a new global and insert the 
    var into the code"
      (interactive)
      (let* ((form (slime-defun-at-point))
             (var-name (concat "<iv-" (number-to-string eval-into-file-count) ">"))
             (form-with-var (concat "(defparameter " var-name form ")")))
        (setq eval-into-file-count (+ eval-into-file-count 1))
        (end-of-defun)
        (slime-eval-async `(swank:eval-and-grab-output ,form-with-var)
          (lambda (result)
            (cl-destructuring-bind (output value) result
              (push-mark)
              (insert value " "))))))

Ciao