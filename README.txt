Dielectric constant

let's say we've done the calculations to the most stable structure.
To begin with we need to perform our non self consistent field(nscf) calculation.
so get ready the filess as follows:

nscf.in
Fe.upf(This depends on what pseudopotential you use)
S.upf(This depends on what pseudopotential you use)


	nscf.in
------------------------------------------------------------
&CONTROL
    calculation='nscf',
    etot_conv_thr = 1.0d-4,
    forc_conv_thr = 1.0d-3,
    nstep = 100,
    max_seconds = 3500,
    prefix='FeS-194-AFM-ONCV',
    pseudo_dir='./',
    outdir='./tmp',
    verbosity='high'
    !restart_mode='restart'
/

&SYSTEM
    ibrav = 4,
    a = 3.3555455208,
    c = 5.6003770828,
    cosAB = -0.5,
    nat =  4,
    ntyp = 3,
    nbnd = 52,
    nspin= 2,
    nosym=.true.
    noinv=.true.
    ecutwfc = 80.0,
    ecutrho = 640.0
    starting_magnetization(1) =  0.5,
    starting_magnetization(2) = -0.5,
    input_dft = 'vdw-df2-b86r',
    occupations = 'smearing',
    smearing = 'gauss',
    degauss = 0.01,
/

&ELECTRONS
    startingwfc = 'atomic+random'
    diagonalization = 'cg'
    mixing_beta = 0.7
    conv_thr = 1.0d-9
    electron_maxstep = 1000
/

&IONS
    ion_dynamics = 'bfgs'
/

&CELL
    cell_dynamics = 'bfgs'
/

ATOMIC_SPECIES
    Fe1   55.845 Fe.upf
    Fe2   55.845 Fe.upf
    S    32.06  S.upf

ATOMIC_POSITIONS {crystal}
Fe1     0.000000000         0.000000000         0.000000000
Fe2     0.000000000         0.000000000         0.500000000
S       0.333330154         0.666669846         0.750000060
S       0.666669846         0.333330154         0.250000000

K_POINTS automatic
   24 24 12   1 1 1
------------------------------------------------------------


Especially, notice the changes:

  nbnd = 52
  nosym = .true.
  noinv = .true.

You have to turn off the automatic reduction of k-points that pw.x does by utilizing crystal symmetries (nosym = .true. and noinv = .true.). This is because epsilon.x does not recognize crystal symmetries, therefore the whole of k-points in the grid is required. Furthermore, you have to calculate a bigger number of bands(in this case we increase the number of bands/Kohn Sham State from 26 to 52), since to see a few bands that are not show in case using small number of bands.

after you get the results of the nscf calculation, you'll be able proceed the process to the epsilon calculation. in here, we need the input for epsilon:

eps.in

	eps.in
------------------------------------------------------------
&inputpp
 outdir='./tmp'
 prefix='FeS-194-AFM-PBENC'
 calculation='eps'
/
&energy_grid
 smeartype='gauss'
 intersmear=0.01
 intrasmear=0.01
 wmax=30.1
 wmin=0.1
 nw=601
 shift=0
------------------------------------------------------------
