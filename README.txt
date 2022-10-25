Dielectric constant

let's say we've done the calculations to the most stable structure.
To begin with we need to perform our non self consistent field(nscf) calculation.
so get ready the filess as follows:

antrian.sh
nscf.in
Fe.upf(This depends on what pseudopotential you use)
S.upf(This depends on what pseudopotential you use)

Especially, notice the changes:

  nbnd = ...
  nosym = .true.
  noinv = .true.

You have to turn off the automatic reduction of k-points that pw.x does by utilizing crystal symmetries (nosym = .true. and noinv = .true.).
This is because epsilon.x does not recognize crystal symmetries, therefore the whole of k-points in the grid is required.
Furthermore, you have to calculate a bigger number of bands(in this case we increase the number of bands/Kohn Sham State from ... to ...),
since to see a few bands that are not show in case using small number of bands.

