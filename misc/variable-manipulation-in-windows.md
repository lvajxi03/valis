# Variable manipulation in Windows


Of course, it would be too simple to implement expressions like this:

```VARIABLE=`command output` ```

in the win32 command processor. Microsoft wants us to do some weird and tricky patterns here. Following example uses the sed utility to manipulate the value:

```cmd
for /f „delims=” %e in (‚echo %VARIABLE% ^| sed -e ‚s/ABC/abc/g’) do @set VARIABLE=%e
```

Can you see the beauty of the pattern used?
(I can't)

Please remember about:

* `%%` instead of `%` if you prefer a script file rather than command line
* yup, there is no typo in `"delims="`, quotes are mandatory
* yup, there is no typo in `^|`, both characters are mandatory (otherwise you will get something like _"| was not expected at this time."_)

Good luck!

You will need it.
