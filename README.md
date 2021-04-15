# cscope_symbolic_link_fix
cscope_symbolic_link_fix
Here is a simple patch which re-enables cscope to work with symlinks. Really you were already 99% of the way there to a fix.

IMHO, this should be a command line option (-l) to enable including symlinks. There are many projects which use symlinks and it's perfectly valid in many cases. It's been messing me up for years trying to use the Cavium SDKs.

Here's the patch (this is against 15.8)

--- dir.c.orig 2010-06-28 19:16:50.000000000 -0300
+++ dir.c 2012-08-30 21:56:44.047742152 -0300
@@ -652,7 +652,7 @@ accessible_file(char *file)
struct stat stats;

if (lstat(file, &stats) == 0
- && S_ISREG(stats.st_mode)) {
+ && (S_ISREG(stats.st_mode) || S_ISLNK(stats.st_mode))) {
return YES;
}
}
