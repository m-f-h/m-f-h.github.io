## Conway's "fraction game" and special cases

A few days ago I rediscovered Conway's PRIMEGAME and related stuff.

Idea: a given **fraction game** is defined by a list of fractions $F = (f_1,...,f_k)$.

This defines a *successor map* $\hat F:ℤ\toℤ; n \mapsto n\cdot f_{i(n)}$, 
where $i(n) = \min\lbrace i \in [1..k]: n\cdot f_i \in ℤ\rbrace$.\
Equivalently, $i(n) = \min\lbrace i\in[1..k]: d_i \mid n \rbrace$, where $d_i$ is the denominator of the $i$-th fraction.

If there is no such $i(n)$ (or $i(n)=-\infty$ if "min" is replaced with "inf"), $\hat F(n)$ is undefined.\
Often, the last fraction is actually an integer, $f_k\in ℤ$. Then $\hat F$ is defined for all $n\in ℤ$.

**Remark:** In GitHub pages, LaTeX's `\mathbb` doesn't work, so you have to copy-paste the UTF characters, *e.g.*,
[from Wikipedia](https://en.wikipedia.org/wiki/Blackboard_bold).

The successor map defines a recurrent sequence $N=(N_n)_ {n\in ℕ}$ for any starting value $N_0\in ℤ$, with $N_{n+1}=\hat F(N_n)$.

### PRIMEGAME

The fractions, successor function and further references for PRIMEGAME can be found in [oeis:A203907](http://oeis.org/A203907).

The PRIMEGAME is such that $(N_n)$ with $N_0=2$ contains all $2^{p_n}$ in order, where $p_n$ is the $n$-th prime,
and no other power of two except for $N_0=2$. 
Otherwise said, it produces all primes in increasing order as $\log_2$ of the powers of 2 appearing in the sequence.

The method is of course quite inefficient: the first few primes appear as\
&nbsp; &nbsp; $N_{19} = 4 = 2^2$, $N_{69} = 8 = 2^3$, $N_{281} = 32 = 2^5$, $N_{710} = 128 = 2^7$, $N_{2375} = 2^{11}$, ...\
So it takes 2375 iterations of the PRIMEGAME successor function to "produce" the 5th prime $p_5=11$.

R. K. Guy (1983) explaines very well how and why this "prime producing" PRIMEGAME works.

PRIMEGAME is actually just an illustration of the fact that one can realize any computable  as a *fraction game*,
as Conway showed in [his FRACTRAN article from 1987](https://dx.doi.org/10.1007/978-1-4612-4808-8_2).

### PIGAME

Conway also gives the example PIGAME (with 40 fractions), which "produces" the decimal digits of &pi;, $d=(3,1,4,1,5,...)$, 
here as $2^{d_n}$ being the first power of two in the sequence starting with $N_0=89\cdot2^n$.
[This is incorrectly stated in Conway (1987), cf. [Kaushik *et al.*, arxiv:2412.16185](https://arxiv.org/abs/2412.16185).]
However, it takes 774 iterations to get the initial digit as 2^3 in the trajectory of 89,
and I was not patient enough to find the second digit as 2^1 in the trajectory of $89\cdot2$.\
(See also [oeis:A350555](https://oeis.org/A350555), [A350556](https://oeis.org/A350556) 
and hopefully soon [A395539](https://oeis.org/A395539).)

References:
* John H. Conway: [FRACTRAN: a simple universal programming language for arithmetic](https://dx.doi.org/10.1007/978-1-4612-4808-8_2).
  in T. M. Cover and Gopinath, eds.: Open Problems in Communication and Computation, Springer, NY, 1987, pp. 4-26.
  doi:10.1007/978-1-4612-4808-8_2
* Richard K. Guy, <a href="https://doi.org/10.2307/2690263">Conway's Prime Producing Machine</a>,
  Mathematics Magazine, Vol. 56, No. 1 (1983), pp. 26-33, available at JSTOR.org/stable/2690263.
* Khushi Kaushik, Tommy Murphy, David Weed: [Computing &Sqrt;2 with FRACTRAN. arxiv:2412.16185](https://arxiv.org/abs/2412.16185)
* Wikipedia, <a href="https://en.wikipedia.org/wiki/FRACTRAN">FRACTRAN</a>, created Sep. 23, 2007.
