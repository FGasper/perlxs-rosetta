Perl XS Rosetta Stone
=====================

The following is meant as a quick-and-dirty “cheat sheet” reference for XSUB authors.
Many of the “translations” below can take multiple forms; the below is not a substitute for
reading and digesting the relevant Perl documentation: `perlxs`, `perlxstut`, `perlapi`, `perlguts`, etc.

The following assumes knowledge of C, including use of pointers.

Scalars: assignment
-------

| Perl             | XS               |
| ----------------- | ---------------- |
| `my $v`    | `SV *mysv = newSV()` |
| `$v = undef`    | `mysv = &PL_sv_undef` |
| `mv $v = 1`   | `SV *mysv = newSVuv(1)` |
| `$v = 1`   | `sv_setuv(mysv, 1)` |
| `$v = -1`  | `sv_setiv(mysv, -1)` |
| `$v = q<>`  | `sv_setpv(mysv, "")` |
| `$v = "\0\1\2"`  | `sv_setpvn(mysv, "\0\1\2", 3)` |

Scalars: concatenation
-------

| Perl             | XS               |
| ----------------- | ---------------- |
| `$v .= 'a'`  | `sv_catpv(mysv, "a")` |
| `$v .= "\0\1\2"`  | `sv_catpvn(mysv, "\0\1\2", 3)` |

Arrays
------

| Perl             | XS               |
| ----------------- | ---------------- |
| `my @a`        | `AV *myav = newAV()` |
| `@a = ()`      | `av_clear(myav)` |
| `push @a, $foo` | `av_push(myav, foo)` |
| `$foo = pop @a` | `SV *foo = av_pop(myav)` |
| `$foo = shift @a` | `SV *foo = av_shift(myav)` |
| `($old) = splice(@a, 2, 1, $foo)` | `SV **old = av_store(myav, 2, foo)` |
| `my ($el) = splice(@, 2, 1)` | `SV *myel = av_delete(myav, 2, 0)` |
| `() = splice(@, 2, 1)` | `av_delete(myav, 2, G_DISCARD)` |


Packages/Namespaces
-------------------

| Perl             | XS               |
| ----------------- | ---------------- |
| `__PACKAGE__`    | `HvNAME( (HV*)CopSTASH(PL_curcop) )` |

Global State
------------

| Perl             | XS             |
| ---------------- | -------------- |
| `${^GLOBAL_PHASE}` | `PL_phase`/`PL_dirty` |
