---
layout: post
title: Common Lisp on the Nexus 7
description:
date: 2012-10-28 16:45:38
category:
tags: ['arm', 'nexus7', 'ubuntu', 'lisp', 'common lisp']
---

Right now I've got Ubuntu on this Nexus, I'm going to want to code something for it. It's already set up for C and python dev but not a drop of Lisp in sight (*gasp*)! 

In this post I'm going to get two versions up and running on the Nexus. ECL and Clozure.

To make this easier for yourself make sure you have installed ssh so you can do all of this from a real keyboard!

    sudo apt-get install ssh


## ECL
ECL is a great fit for this platform as it compiles down to C code so the executables are small and the tight integration with the FFI will make this very useful. Also it is super easy to install as it is in the repos.

    sudo apt-get install ecl

and your done! Obviously your going to want to have more than just the repl so further down in this post we will look at installing Emacs and Slime.


## Clozure
Next is Clozure. Now I'm usually working in SBCL when I'm on a regular machine (mainly as that's what my mate set me up with when I started learning!) but Clozure (I believe) has had an Arm port for a while longer and also SBCL's install process for Arm was a bit hairy for a beginner like me.

Setting up Clozure involves a little bit more work than ECL, but not by much.

Right so once you are ssh'd into yout tablet, you need to pull the latest version of Clozure from the repository. 

       svn co http://svn.clozure.com/publicsvn/openmcl/trunk/linuxarm/ccl

       Note: I'm using the build from trunk which is not guarenteed
       stable, it may be worth trying the release version instead 
       but as I have not tested this yet I can't show the steps here.
       If you do try it, please let me know how it went!


Once that has finished we need to make a quick change to the config as, as default, Clozure for Arm7 is set up for soft floating point. We need hard. So:

     cd ccl/lisp-kernel/linuxarm/
     make clean

Now you need to open up the 'float_abi.mk' file in your favourite editor and uncomment the hard float line and comment out the soft float one. After the change the file should look like this:

      # This file should define FLOAT_ABI as one of "softfp" or "hard".
	# If you change this, do 'make clean' to remove any object files
	# compiled for the other ABI.
	#FLOAT_ABI = softfp
	FLOAT_ABI = hard

We are almost done! now run the following:
   make
   cd ../..
   armcl --not-init

At this point you should get the clozure repl!

Now run:
    
    (rebuild-ccl :full t)

And clozure will be rebuilt. This will take a few minutes so keep yourself busy with a little victory dance!


## Editor
Wonderful, so now I want an editor to play around with. Unfortunately I am not versed in Vim so I can't provide and info for you Vim fans but I do use Emacs so here goes!:

Well the first order of buisness is to get Emacs. Now it may seem overkill to have this on your device, you may prefer just to install swank and use your desktop but for me I'd like to ssh in an use Emacs from the command line.

     sudo apt-get install Emacs24 emacs-goodies-el slime

Now we just need to add the following to the .emacs file:
    
    (eval-after-load "slime"
      '(progn
	 (setq slime-lisp-implementations
	       '((ccl ("/home/ubuntu/.ccl/armcl"))
		 (ecl ("/usr/bin/ecl"))))
	 (slime-setup '(
			slime-asdf
			slime-editing-commands
			slime-fancy-inspector
			slime-fontifying-fu
			slime-fuzzy
			slime-indentation
			slime-mdot-fu
			slime-package-fu
			slime-references
			slime-repl
			slime-sbcl-exts
			slime-scratch
			slime-xref-browser
			))
	 (slime-autodoc-mode)
	 (setq slime-complete-symbol*-fancy t)
	 (setq slime-complete-symbol-function
      'slime-fuzzy-complete-symbol)))

    (require 'slime)

## Done!
Well thats all done! I'll try and keep this updated with any progress I make on the tablet. I'm very excited to see what happens next.
Ciao