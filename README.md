Perl XS Rosetta Stone
=====================

Scalars
-------

| Perl             | XS               |
| ----------------- | ---------------- |
| `my $v`    | `SV *mysv = newSV()` |
| `$v = undef`    | `mysv = &PL_sv_undef` |
| `mv $v = 1`   | `SV *mysv = newSVuv(1)` |
| `$v = 1`   | `sv_setuv(mysv, 1)` |

Packages/Namespaces
-------------------

| Perl             | XS               |
| ----------------- | ---------------- |
| `__PACKAGE__`    | `HvNAME( (HV*)CopSTASH(PL_curcop) )` |
