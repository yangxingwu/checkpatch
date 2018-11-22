# Coding style for linux C

It's a coding style following [QEMU](https://github.com/qemu/qemu/)  

From [QEMU contributing guide](https://wiki.qemu.org/Contribute/SubmitAPatch):  

>  1. [QEMU Coding Style](http://git.qemu-project.org/?p=qemu.git;a=blob_plain;f=CODING_STYLE;hb=HEAD)
>  2. [QEMU Coding Guidelines](http://git.qemu-project.org/?p=qemu.git;a=blob_plain;f=HACKING;hb=HEAD)
>  3. [Automate a checkpatch run on commit](http://blog.vmsplice.net/2011/03/how-to-automatically-run-checkpatchpl.html)
>  4. [Spell check your patches](https://wiki.qemu.org/Contribute/SpellCheck)

## Usage

```bash
~$ ./checkpatch.pl --help
Usage:

    checkpatch.pl [OPTION]... [FILE]...
    checkpatch.pl [OPTION]... [GIT-REV-LIST]

Version: 0.31

Options:
  -q, --quiet                quiet
  --no-tree                  run without a kernel tree
  --no-signoff               do not check for 'Signed-off-by' line
  --patch                    treat FILE as patchfile
  --branch                   treat args as GIT revision list
  --emacs                    emacs compile window format
  --terse                    one line per report
  -f, --file                 treat FILE as regular source file
  --strict                   fail if only warnings are found
  --root=PATH                PATH to the kernel tree root
  --no-summary               suppress the per-file summary
  --mailback                 only produce a report in case of warnings/errors
  --summary-file             include the filename in summary
  --debug KEY=[0|1]          turn on/off debugging of KEY, where KEY is one of
                             'values', 'possible', 'type', and 'attr' (default
                             is all off)
  --test-only=WORD           report only warnings/errors containing WORD
                             literally
  -h, --help, --version      display this help and exit

When FILE is - read standard input.
```

### check a file

```bash
~$ ./checkpatch.pl --no-tree -q --file test.c
```

### check a patch

```bash
~$ ./checkpatch.pl --no-tree -q --patch test.diff
```

### run as a git commit hook

```bash
~$ cat >.git/hooks/pre-commit
#!/bin/bash
exec git diff --cached | scripts/checkpatch.pl --no-signoff -q -
^D

$ chmod 755 .git/hooks/pre-commit
```

Note: If you encounter a false positive because `checkpatch.pl` is complaining about code you didn't touch, use `git commit --no-verify` to override the pre-commit hook. Use this trick sparingly :-).
