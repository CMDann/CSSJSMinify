CSSJSMinify
===========

This shell script will minify Javascript and CSS files. This may cause problems in situations that have code in quotations. This will not know the difference between commented out code and coe that is in use.

How it works
============

1. This script  replaces /**/ and /*\*/ with /~~/ and /~\~/ 
2. Then it removes /*...*/ style comments on single lines 
3. Then it removes // style comments from Javascript files. 
4. Then it replaces all newlines with spaces and removes /*...*/ style comments again (this time over multiple lines). 
5. Then it puts the /**/ and /*\*/ CSS hacks back in. 
6. Then it replaces multiple spaces with single spaces. 
7. Then it removes spaces before [{;:,] and then spaces after [{:;,].

The command
===========

The command is split over multiple lines. You can remove the \ to put it onto a single line.

sed -e "s|/\*\(\\\\\)\?\*/|/~\1~/|g" -e "s|/\*[^*]*\*\+\([^/][^*]*\*\+\)*/||g" \
  -e "s|\([^:/]\)//.*$|\1|" -e "s|^//.*$||" | tr '\n' ' ' | \
  sed -e "s|/\*[^*]*\*\+\([^/][^*]*\*\+\)*/||g" -e "s|/\~\(\\\\\)\?\~/|/*\1*/|g" \
  -e "s|\s\+| |g" -e "s| \([{;:,]\)|\1|g" -e "s|\([{;:,]\) |\1|g" 
