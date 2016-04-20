# psh
Perl shell from the 90s



psh (c) 1998 Marcel Turcotte and Bob MacCallum 

Description
^^^^^^^^^^^
Simple perl shell, uses AUTOLOAD to make system calls for commands which
are not defined.  Multiple line entry is accomplished by adding a
tailing \.  or by leaving an unbalanced (, { or [ (or <<EOF).
Force execution (the balancing check isn't very clever) by terminating
a line with ;; (two semicolons)

2016 comment update: probably should use
http://perldoc.perl.org/5.8.8/Shell.html
nowadays?
Or perhaps see this other project https://gnp.github.io/psh/

If you need a unix command instead of its perl namesake (eg. grep) then 
prepend an underscore, eg: _grep('pattern', 'file') or you could use
system() or backticks 

.pshrc   ($ENV{HOME}/.pshrc)
^^^^^^
You can put your own functions and other startup code in your .pshrc

To use unix `ls' (for example) without having to write ls() put prototypes
in your .pshrc like this
  sub ls;
  sub emacs;

Aliases are done like this:
  sub ll { ls('-l', @_) };

changing the behaviour in .pshrc
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
the $prompt string is eval'ed to make the prompt: for example
$prompt = '"psh<".($readline_attribs->{history_base}
                   +$readline_attribs->{history_length}).">% "';

set this variable like this:
$psh_keywords_h = '/usr/local/src/perl5.004_01/keywords.h';
to get all the perl keywords completed (grep foreach etc)

and $psh_history_size = 50
to change the size of the history between invocations (default 100)

Miscellaneous
^^^^^^^^^^^^^
If you need to `source' a perl program use perl's `do' function
or if it's a module, use `use' of course.

line editing/file/command/variable completion by the Gnu ReadLine library

plus... Control-g : kill-whole-line

add your own changes using .inputrc or in your .pshrc using 
  $readline_terminal->parse_and_bind("Control-g: kill-whole-line");
  (see the GNU Readline user manual)

some `built-in' variables that you can use

$lr is the last result
to echo $lr after each command set
$echolr = 1 

Job control (backgrounding)
^^^^^^^^^^^
Control-z will background a running job (not suspend it like csh)
***this does not work for unix/system commands, use bg (see below) ***
the `listjobs' command will list process ids (pid) of running jobs 
to kill a job use: kill 9, pid  (the perl function)

to background perl code or other commands from the outset use:
bg { code }

