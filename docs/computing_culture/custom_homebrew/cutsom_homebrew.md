---
geometry: margin=1in
author: Yehowshua Immanuel
date: January 1, 2020
title: Custom Tap and Bottles with Homebrew
---
## Some Terminology and Background

Homebrew is a fantastic package manager written in ruby. A package in Homebrew
is built with a formula, and all formula-based packages can be and must be 
capable of being built from source[^1].
Since many FOSS projects use git these days, you can even specify 
the git repository and how to build from it in the homebrew formula.

For example:

    $ brew install --HEAD gcc

would git clone the latest gcc sources and build gcc using the 
formula provided in Homebrew.

Homebrew is also very flexible and secure as it doesn't need root privileges
for its packages.

Homebrew can turn a build into a re-distributable **Bottle**. Homebrew works on both
Linux and Mac and hosts its core repository at https://github.com/Homebrew/homebrew-core.

Lastly, Homebrew has taps which are third party formula repositories that a user can
install from.

## Why Make a Homebrew Tap?
Recently, Bluespec open sourced their BlueSpec Verilog Compiler(bsc) [^2].
I wanted to build a homebrew formula and submit it to homebrew-core.
Homebrew has strict requirements for formula they accept to homebrew-core.

The one that got me in trouble is:

>  If the source for the software to be built is hosted on GitHub, a canonical [^3]
>  versioned/git-tagged Github release tarball must be used as the source for the 
>  build of the formula.

Basically, as of the time of the writing of this article, the BSC maintainers do not
have a Github release.

If I can't contribute a formula to homebrew core, I can still create my own tap.

## Making a Tap and Bottle
I created my own tap at [bracketmaster/homebrew-rtl](https://github.com/BracketMaster/homebrew-rtl)
which contains some formulas for building FOSS RTL and FPGA tools such as NextPnr and bsc.

To install these formulas, I do:

``` bash
$ brew tap bracketmaster/rtl
$ brew install bsc
==> Installing bsc from bracketmaster/rtl
==> Downloading https://github.com/BracketMaster/homebrew-rtl/releases/download/
==> Downloading from https://github-production-release-asset-2e65be.s3.amazonaws
######################################################################## 100.0%
==> Pouring bsc-2017.07.A.catalina.bottle.tar.gz
🍺  /usr/local/Cellar/bsc/2017.07.A: 334 files, 137.1MB
```

You may notice that homebrew poured bottles instead of building bsc from source.
You may also notice the bottles are hosted using [Github Releases](https://github.com/BracketMaster/homebrew-rtl/releases).

Basically, I created a versioned release on Github with all the tarred sources for various formulas[^4]
I also uploaded the bottles brew generated when building my formulas the first go round.

Generating your first brew bottle is easy. For example wit NextPnr:

    $ brew install --build-bottle nextpnr
    $ brew bottle --root-url="https://github.com/BracketMaster/homebrew-rtl/releases/download/v1.0" --no-rebuild nextpnr

      bottle do
        root_url "https://github.com/BracketMaster/homebrew-rtl/releases/download/v1.0"
        cellar :any
        sha256 "779cbbee58781c95459c155c6a574267f8d9d9817779252f13defef51d01a4f2" => :catalina
      end

Where ``root-url`` is the Github Releases url you plan to upload the bottle to.

You should see a file in your current directory that looks something along the lines of
``nextpnr-1.0.catalina.bottle.tar.gz``.

Go ahead and upload that to your Github Releases and paste the ``bottle do/end`` stanza
into your homebrew ruby formula.


[^1]: I specify formula-based packages because Hombrew also supports the installation of 
Casks which in MacOS are MacOS native "apps" that end in ``.app`` such as ``Chrome.app``.
[^2]: Anounced 
[here](https://bluespec.com/2020/01/06/bluespec-inc-to-open-source-its-proven-bsv-high-level-hdl-tools/)
to be open sourced on January 31, 2020. Interestingly enough, at the time of the writing of this article,
https://bluespec.com still does not have a link on their website pointing to the source code location.
But, after scouring around on the internet,
[this Reddit post](https://www.reddit.com/r/FPGA/comments/el26iv/bluespec_inc_to_open_source_its_proven_bsv/)
notes that the source is located [here](https://github.com/B-Lang-org/bsc).
[^3]: Canonical basically means that the Github Repo is not a fork.
[^4]: Worth noting that as of this writing, Github does provide a URL API that
allows you to download a repository as a tar. But the provided tarball doesn't 
contain submodules or the ``.git`` which is a problem for software that builds
version info from the git commit. Also worth noting that Github releases also
use this same URL API.