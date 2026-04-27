## Making b-files for OEIS with PARI/GP

OK, I'm taking up the posts here and will post more regularly my musings, math & CS activities,
with some hopefully useful notes for myself and maybe others.

### General remarks

IMO, OEIS' "b-files" should be signed **and dated** as well in the LINKS section (usually not dated there) 
as in a short header of the file itself.
That header should also inform about how "precisely" the terms were computed: formula, brute force scan, ...?
If the code used to compute it is given in the sequence, it's enough to refer to that,
since the history function allows to know which code was there at the moment where the data was computed,
even if the program is changed later on.

### The code

I use the following code of just two lines to make the b-files. (Somewhere in earlier space-time I had a script
`make_bfile(seq, fct_or_data, offset)`, where seq could be just the number (*i.e.*, the digits in the A-number),
or a string or name of the form "Annn" or "bnnn") and all other arguments were optional --- 
required only if the program could or should not guess and/or compute them on itself. 
But finally, the following two lines are that short it might often take longer to load the script & execute the function,
than to type it out directly (using "autocomplete" with the "TAB" key for the path & filename).

[Side note, for *reading* b-files I do use `\r /temp/b2v.gp` and then `#Axx=b2v("/temp/bxxx.txt")`
(again using [Tab] to autocomplete as much as possible!), which is really faster than doing it by hand,
due to slightly painful parsing of the b-file if it has comments, spaces and empty lines in all possible places.]

Here's the code:
```
write("/temp/b012345.txt","# OEIS.org/A012345(o..N) computed Apr 29 2026 by M.F.H. using PARI/GP code given there.")
write("/temp/b012345.txt",strjoin([Str(n" "A012345(n)) | n<-[o..N]], "\n")
```
Of course, if we have the data in a list, we replace A012345(n) by A012345[n] or [n+1] if offset is zero.

It is indeed very **(very)** much more efficient to concatenate the lines of the file and use only one single  
`write()` command, rather than `write()` each line separately.

For the special case of recurrent sequences (such as [Conway's "fraction game" sequences](../26/PRIMEGAME.html), *e.g.*, 
[PRIMEGAME = A203907](http://oeis.org/A203907) with trajectories of 2 ([A007542](http://oeis.org/A007542))
[or 3 (A185242)](http://oeis.org/A185242)), 
we can create the sequence "on the run" with
```
write("/temp/b007542.txt",strjoin([Str(k" ", a=if(k,A203907(a),2)) | k<-[0..8102]], "\n"))
```
