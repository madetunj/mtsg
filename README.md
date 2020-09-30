# Mutational Signatures

**Mutational Signatures** (abbreviated as **mtsg**) finds and quantifies [COSMIC
mutational signatures] across samples.

mtsg uses a base set of mutational signatures extracted by [SigProfiler] for
[single-base substitutions] (SBS), i.e., single-nucleotide variants (SNV),
using 2780 whole-genome variant calls from the ICGC/TCGA [Pan-Cancer Analysis
of Whole Genomes] (PCAWG) project.

[COSMIC mutational signatures]: https://cancer.sanger.ac.uk/cosmic/signatures
[SigProfiler]: https://cancer.sanger.ac.uk/cosmic/signatures/sigprofiler.tt
[single-base substitutions]: https://cancer.sanger.ac.uk/cosmic/signatures/SBS/index.tt
[Pan-Cancer Analysis of Whole Genomes]: https://dcc.icgc.org/pcawg

## Prerequisites

  * [Python] ^3.8
    * [sigproSS] ^0.0.0

[Python]: https://www.python.org/
[sigproSS]: https://github.com/AlexandrovLab/SigProfilerSingleSample

## Install

Use [Poetry] to install mtsg and its dependencies.

```
$ poetry install --no-dev
```

SigProfilerMatrixGenerator can then be used to install base mutational
matrices given a supported genome build.

```
$ poetry run python -c 'from SigProfilerMatrixGenerator.install import install; install("<genome-build>")'
```

Replace `<genome-build>` with one of `GRCh37`, `GRCh38` (Homo sapiens),
`mm9`, `mm10` (Mus musculus), or `rn6` (Rattus norvegicus).

[Poetry]: http://python-poetry.org/

## Usage

Mutational Signatures is installed as the executable `mtsg`. It has two
commands: `run` and `visualize`.

### Run

`run` is used to profile samples (as VCFs) using known mutational signatures.

```
usage: mtsg run [-h] [--dst-prefix DST_PREFIX] [--genome-build {GRCh37,GRCh38,mm9,mm10,rn6}] src-prefix

positional arguments:
  src-prefix

optional arguments:
  -h, --help            show this help message and exit
  --dst-prefix DST_PREFIX
  --genome-build {GRCh37,GRCh38,mm9,mm10,rn6}
```

### Visualize

`visualize` uses signature activities (generated by `run`) to create an
interactive plot.

```
usage: mtsg visualize [-h] [--output OUTPUT] src

positional arguments:
  src

optional arguments:
  -h, --help       show this help message and exit
  --output OUTPUT
```

## Docker

Mutational Signatures has a `Dockerfile` to create a Docker image, which sets
up and installs the required runtime and dependencies. To build and use this
image, [install Docker](https://docs.docker.com/install) for your platform.

### Build

In the Mutational Signatures project directory, build the container image.

```
$ docker image build --tag mtsg .
```

### Run

The image uses `mtsg` as its entrypoint, giving access to all commands.

```
$ docker container run mtsg <args...>
```

For example, to run the `run` command with samples in `$PWD/data` and an empty
results directory at `$PWD/results`:

```
$ docker container run \
  --rm \
  --mount type=bind,source=$PWD/data,target=/data \
  --mount type=bind,source=$PWD/results,target=/results \
  mtsg \
  run \
  --dst-prefix /results \
  /data
```

## References

  * Alexandrov, L.B., Kim, J., Haradhvala, N.J. _et al_. The repertoire of
    mutational signatures in human cancer. _Nature_ **578**, 94–101 (2020).
    https://doi.org/10.1038/s41586-020-1943-3

  * Bergstrom, E.N., Huang, M.N., Mahto, U. _et al_.
    SigProfilerMatrixGenerator: a tool for visualizing and exploring patterns
    of small mutational events. _BMC Genomics_ **20**, 685 (2019).
    https://doi.org/10.1186/s12864-019-6041-2
