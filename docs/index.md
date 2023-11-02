# home

## Documentation under development!

Welcome the the documentation for the [gist-geckos pipeline!](https://github.com/geckos-survey/gist-geckos)
gist-geckos is the extension of the [gist pipeline](https://abittner.gitlab.io/thegistpipeline/), originally written by [Adrian Bittner](https://ui.adsabs.harvard.edu/abs/2019A%26A...628A.117B/abstract).

gist-geckos is a modular, python-based pipeline for the analysis of integral field data. The pipeline has been developed initially for use with the ESO-VLT/MUSE [GECKOS](https://geckos-survey.org/) and [MAUVE](https://mauve.icrar.org/) large programmes, but can be adapted to any MUSE data (and any IFS data in the future).

gist-geckos reads a MUSE cube, spatially masks it, Voronoi bins (if rquired), and then performs scientific analysis on it. Current modules perform stellar kinematic extraction, emission lines measures, stellar populations, and line strength measures. The outputs are easy-to-read 2D maps files, which may be fed straight into your favourite plotting script, or used with mapviewer. 

:smile: :smile: :smile: