
Lish will be a Lisp shell (it's already pretty nice)

https://github.com/nibbula/lish

Oh my, it also has a Lisp REPL.

## Lish shell

Build it: https://github.com/nibbula/yew/tree/master/lish

My notes:

- `help`
- docs are in lish/docs/doc.org
- re-load your lish file with `load ~/.lishrc`
- **mix** shell commands and Lisp code:

```
$ echo ,*package*
#<PACKAGE "LISH-USER">

$ (defun hello (name) (format t "hello ~a!!~&" name))
$ (hello "me")
hello me!!
NIL
$ (hello "me") | wc -w
=> 2 #<STRING-INPUT-STREAM {100532FEA3}>
```

when you write Lisp, use TAB to see the function's signature.

- a **document/tree viewer**, including for .org files: `view`. Use `C-n` and `C-p`, type any character to search, `C-l`… `q`, on a file use `C-x C-f`. Impressive.
- a **dired** (directory mode) emulation: open a directory with `view`. You get a warning message. I feel this and Lem are doing a similar job.

Of course, `vim` or `emacs- nw` work inside Lish. Not all interactive programs though, like `fzf`.

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

### Example commands

In the .lishrc file:

- create commands to listen internet radios (run `mpv`)

```lisp
(uiop:run-program (list "mpv"
                             ,url)
                       :output t
                       :input :interactive)
```

---

## Lisp REPL and debugger

https://github.com/nibbula/yew/blob/master/tools/tiny-repl.asd

Inside the Lish shell, call it:

    $ (tiny-repl:tiny-repl)

But we don't even *have* to start it, since Lish understand all Common Lisp.

It does:

- symbol completion
- multiline editing
- parens matching highlighting
- inline suggestions from past commands à la Fish shell (like Lish does)
- more?

It has a debugger (`deblarg`). It shows the condition and restarts, with a debug prompt.


~~~lisp
SBCL:LU> (/ 3 0)
Entering the debugger. Blarg.

Condition: DIVISION-BY-ZERO
arithmetic error DIVISION-BY-ZERO signalled
Operation was (/ 3 0).
Restarts are:
0: ABORT Return to TOP command loop.
1: ABORT Return to Lish TOP level.
2: LISH::RETRY Retry Lish command TOP level.
Debug 1>
~~~

It has a stepper?
