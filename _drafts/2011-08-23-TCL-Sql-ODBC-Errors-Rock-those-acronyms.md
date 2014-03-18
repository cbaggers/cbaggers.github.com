---
layout: post
title: TCL Sql ODBC Errors... Rock those acronyms
description: 
category:
tags: ['']
---

As I'm employed in the business of creative self harm I have been trying to get tcl to talk to one of our other systems using odbc. 
If get the wonderfully informative: SQL_ERROR when you try and connect and aren't psychic try the following:
<pre class="brush: python;"> 
set res_state ""
set res_num 0
set res_text ""
set res_buf_int 0
odbc SQLGetDiagRec SQL_HANDLE_DBC $hdbc 1 state res_num res_text 200 res_buf_int
</pre>
You can then echo res_state and res_text to see if there is anything useful.
The '1' in the SQLGetDiagRec command is the error record you want to pull, there can be any number of these. Note that the first record is 1...why?..who knows!