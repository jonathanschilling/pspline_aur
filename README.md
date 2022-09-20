# pspline_aur
This is an Arch Linux PKGBUILD script for the Princeton University [NTCC PSPLINE library](https://w3.pppl.gov/ntcc/PSPLINE/).


## TODO

* One cannot use `-j` to parallelize the build because something is broken in the Makefiles.
  Thus, also a global setting of `MAKEFLAGS="-j"` in `/etc/makepkg.conf` breaks this package.
