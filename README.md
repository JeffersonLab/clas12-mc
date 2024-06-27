This repository is the collection of generators available in the CLAS12 Docker/Singularity containers for offsite (e.g. OSG) simulation jobs.  These generators are also available for use on JLab machines via the [CLAS12 environment](https://clasweb.jlab.org/wiki/index.php/CLAS12_Software_Environment_@_JLab) module `mcgen`.

# Current Generators 

name                 | description                                                    | maintainer
-------------------- | -------------------------------------------------------------- | ------------------
[clasdis]            | SIDIS MC based on PEPSI LUND MC                                | Harut Avakian
[claspyth]           | SIDIS full event generator based on PYTHIA                     | Harut Avakian
[clas-stringspinner] | SIDIS PYTHIA with hadronization spin effects                   | Christopher Dilks
[dvcsgen]            | DVCS/pi0/eta generator based on GPD and PDF parameterizations  | Harut Avakian
[genKYandOnePion]    | KY, pi0P and pi+N                                              | Valerii Klimenko
[inclusive-dis-rad]  | Inclusive electron and optionally radiative photon using PDFs  | Harut Avakian
[tcsgen]             | Timelike Compton Scattering                                    | Rafayel Paremuzyan
[jpsigen]            | J/Psi photoproduction                                          | Rafayel Paremuzyan
[twopeg]             | pi+pi- electroproduction off protons                           | Iuliia Skorodumina
[clas12-elspectro]   | General electroproduction final states                         | Derek Glazier
[MCEGENpiN_radcorr]  | Exclusive single pion electroproduction based on MAID          | Maksim Davydov
[deep-pipi-gen]      | Deep double pion production                                    | Dilini Bulumulla
[genepi]             | Photon and meson electroproduction                             | Noémie Pilleuxi
[onepigen]           | Single charged pion production based on AO/Daresbury/MAID      | Nick Tyler
[GiBUU]              | Quark and hadron propagation in nuclear media                  | Ahmed El Alaoui 
[GENIE]              | Not ready for OSG |

# Adding or Modifying a Generator

1. Create a github repository for your source code, ideally inside https://github.com/JeffersonLab
2. Make sure to include the README.md describing the generator, its options, and requirements
3. Have a working build system (for example a Makefile)
4. Satisfy the additional requirements described below
5. Send email to Mauri (`ungaro at jlab.org`) or Nathan (`baltzell at jlab.org`) with the repository address and the git tag to use

# Requirements

- C/C++/Fortran/python3, with a working GNU make or cmake build system compliant with GCC no less than 9.0
- The top level README file should contain:
  - The location of the executable(s) and any shared libraries produced and required at runtime
  - Required environment variables
  - Documentation on all command-line options
- The executable to be used on OSG should have the same name as the github repository name and be runnable from any current working directory
- The default output LUND filename should be the same as the exectuable + `.dat`. For example, the output of clasdis must be clasdis.dat
- The follow command-line arguments are always passed to all generators on OSG:
  - `--trig #` must be honored and used to specify the number of events to generate.
  - `--docker` must be accepted as a valid argument and can be ignored or used for setting conditions for OSG.
  - `--seed #` must be accepted as a valid argument and can be ignored or used to initialize the event generator's RNG.  Its value is a 32-bit RNG seed based on system clock with microsecond precision.  If `--seed` is ignored, the generator is responsible for choosing unique random seeds, without preserving state between jobs, which can be done from a millisecond or better precision system clock.
- A git tag to reference for including the generator as a submodule into this repository.  Note [versions.json](versions.json) stores the current verisions for insertion into the data stream.
- Note, currently CLAS12's OSG web submission portal does not support configuration files for event generators, so users' runtime options must be supported via command-line options.
  -  Some generators do that via a wrapper script that generates a configuration file on-the-fly.
  -  If there's popular demand for it, support for user-defined configuration files could be added.

### Test of Requirements

We use this command line to check if the runtime requirements are met:

```
GENERATOR_NAME --trig 10 --docker --seed 1448577483
```

That should produce a file `GENERATOR_NAME.dat` in the current working directory.

The script `requirements.zsh` will compile all the generators, check for their executable names, run them with their environment and the above options, and check for the output file, and then output the table in the next section.

### Requirements Summary

name | compilation and executable name | CLI options | runs in container w/ output
---- | ------------------------------- | --------------------- | -----------------
clasdis | :white_check_mark: | :white_check_mark: | :white_check_mark: |
claspyth | :white_check_mark: | :white_check_mark: | :white_check_mark: |
classtringspinner | :white_check_mark: | :white_check_mark: | :white_check_mark: |
dvcsgen | :white_check_mark: | :white_check_mark: | :white_check_mark: |
genKYandOnePion | :white_check_mark: | :white_check_mark: | :white_check_mark: |
inclusive-dis-rad | :white_check_mark: | :white_check_mark: | :white_check_mark: |
JPsiGen | :white_check_mark: | :white_check_mark: | :white_check_mark: |
TCSGen | :white_check_mark: | :white_check_mark: | :white_check_mark: |
twopeg | :white_check_mark: | :white_check_mark: | :white_check_mark: |
clas12-elSpectro | :white_check_mark: | :white_check_mark: | :white_check_mark: |
MCEGENpiN_radcorr | :white_check_mark: | :white_check_mark: | :white_check_mark: |
deep-pipi-gen | :white_check_mark: | :white_check_mark: | :white_check_mark: |
genepi | :white_check_mark: | :white_check_mark: | :white_check_mark: |
gibuu | :white_check_mark: | :white_check_mark: | :white_check_mark: |
genie | :white_check_mark: | :white_check_mark: | :red_circle: |

### [Maintenance](./doc/maintenance.md)

[clasdis]: https://github.com/jeffersonlab/clasdis 
[claspyth]: https://github.com/jeffersonlab/claspyth
[clas-stringspinner]: https://github.com/JeffersonLab/clas-stringspinner.git
[dvcsgen]: https://github.com/jeffersonlab/dvcsgen
[genKYandOnePion]: https://github.com/ValeriiKlimenko/genKYandOnePion
[inclusive-dis-rad]: https://github.com/jeffersonlab/inclusive-dis-rad
[tcsgen]: https://github.com/jeffersonlab/tcsgen
[jpsigen]: https://github.com/jeffersonlab/jpsigen
[twopeg]: https://github.com/skorodumina/twopeg
[clas12-elspectro]: https://github.com/dglazier/clas12-elspectro/
[MCEGENpiN_radcorr]: https://github.com/Maksaska/MCEGENpiN_radcorr 
[deep-pipi-gen]: https://github.com/jeffersonlab/deep-pipi-gen
[genepi]: https://github.com/N-Plx/genepi
[onepigen]: https://github.com/tylern4/onepigen
[GENIE]: genie-util
[GiBUU]: gibuu
