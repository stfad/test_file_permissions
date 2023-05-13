# Testing file permissions tracked by the Git

## The problem
I found really interesting behaviour of file permissions:
```bash
user@pc:~/projects/test_file_permissions$ ls -la test.sh 
-rwxr-xr-x 1 user user 34 мая 13 14:17 test.sh
user@pc:~/projects/test_file_permissions$ chmod 740 test.sh 
user@pc:~/projects/test_file_permissions$ git diff
diff --git a/test.sh b/test.sh
old mode 100644
new mode 100755

``` 
It seems that filesystem works fine while git sees incorrect file permissions with enabled `core.filemode` property. Also git ignore chmod of the form 644->640 or 755->700.

## Why this happens?

Full explanation can be found [here.](https://medium.com/@tahteche/how-git-treats-changes-in-file-permissions-f71874ca239d)

Short explanation:
> Git Tracks ONLY the Executable Bit of the Permissions for the User Who Owns the File. This means the only change in file permissions Git notices is whether the file’s owner (u when using chmod in symbolic mode, the first digit when using chmod in octal mode with 3 digits or the second digit when using chmod in octal mode with 4 digits) has executable permission or not. This means the following permissions are NOT tracked:
> * Other permissions (write and read) for the file's owner are not tracked
> * All permissions (execute, write and read) for other users in the file’s group (g) and other users not in the files group (o) are not tracked.

Conclusion:
> 100755 for executable and 100644 for non-executable are the only file modes Git records.

Happy coding!:metal:

