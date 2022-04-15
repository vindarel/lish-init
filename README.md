
Lish will be a Lisp shell (it's already pretty nice)

https://github.com/nibbula/lish

Build it: https://github.com/nibbula/yew/tree/master/lish


In this .lishrc file:

- create commands to listen internet radios (run `mpv`)

```lisp
(uiop:run-program (list "mpv"
                             ,url)
                       :output t
                       :input :interactive)
```

---

My notes:

- doc in lish/docs/doc.org
- re-load your lish file with `load ~/.lishrc`
- mix shell commands and Lisp code:

```
$ echo ,*package*
#<PACKAGE "LISH-USER">

$ (defun hello () (print "hello !!"))
```

when you write Lisp, use TAB to see the function's signature.

- a document/tree viewer, including for .org files: `view`. Use `n` and `p`, TAB, `C-l`… `q`. Impressive. Of course, `vim` or `emacs- nw` work.

```
$ help differences
Lish is very different from a POSIX shell. The most notable differences are:

- Parentheses switch to Lisp syntax, and don't mean run in sub-shell.
  Lisp inside parentheses is evaluated and substituted in the current line.
- String quoting is done only with double quote ". Single quote ' and back
  quote `, are not special to avoid confusion with Lisp.
- The prefix VAR=value isn't supported. Use the ‘env’ command instead.
- Redirection syntax is different, e.g "2>&1" doesn't work.
- Commands can be Lish commands and Lisp functions, as well as executables
  in your PATH. Lish commands can be searched for and automatically loaded
  from ASDF places, manipulated by the ‘ldirs’ command.
- Most scripting related shell commands are missing, e.g. if, test, case.
  Scripting parameter expansion like $1 $* ${} are missing. Use Lisp instead.
- Shell expansions are different. Many expansions can be done by Lish functions
  starting with ! , such as (!_ "ss") expands to a list of strings of the
  lines of output, (!? "grep fuse /proc/filesystems") returns a boolean status.
  Comma can be used to substitute a Lisp value, e.g. "echo ,*package*".
- Comments start with ; not #

For more detail see the section ‘Differences from POSIX shells’ in docs/doc.org
```
