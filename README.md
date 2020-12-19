## power_rankings
Python package used to create stats including power rankings for the Zogby group.
This package provides cli tools to create and customize statistics from
Zogby group round data.

### Install

**`pip install 'power_rankings@git+https://github.com/cswelton/power_rankings@master'`**

When installed, these two cli tools are available:

#### sync-golfgenius

```
usage: sync-golfgenius [-h] [--results-dir <PATH>] [--disable-screenshots] [--screenshots-dir <PATH>] [--filter <REGEX>] [--show-browser] [--quiet] [--logfile <PATH>] [--debug] <GGID-CODE>

positional arguments:
  <GGID-CODE>           GGID Code to parse. This can be any GGID code from a bbc round.

optional arguments:
  -h, --help            show this help message and exit
  --results-dir <PATH>  The directory to output result JSON files. (default: ./results)
  --disable-screenshots
                        Turn off screenshots (default: False)
  --screenshots-dir <PATH>
                        Directory to store screenshots (default: screenshots)
  --filter <REGEX>      A regular expression to filter round names to parse (default: .*)
  --show-browser        Show the browser as data is being scanned. (default: False)
  --quiet               Do not print logs to the screen. (default: False)
  --logfile <PATH>      Send logs to a file. (default: None)
  --debug               Turn on debug logging. (default: False)
```

#### power-rankings

```
usage: power-rankings [-h] [--results-directory <PATH>] [--weeks <integer>] [--min-rounds <integer>] [--weighted-rounds <integer>] [--outlier-distance <float>] [--weight-birdies <float>] [--weight-scoring <float>]
                      [--weight-pars <float>] [--dump] [--summary] [--player-filter <regex>]

optional arguments:
  -h, --help            show this help message and exit
  --results-directory <PATH>
                        Path to results directory (default: ./results)
  --weeks <integer>     Data range in weeks (default: 4)
  --min-rounds <integer>
                        Minimum number of rounds for player to count in rankings (default: 0)
  --weighted-rounds <integer>
                        Number of recent rounds to weight heavier (default: 2)
  --outlier-distance <float>
                        Distance from median to remove outliers. (default: None)
  --weight-birdies <float>
                        Relative weight to apply to birdies or better per round average. (default: 5)
  --weight-scoring <float>
                        Relative weight to apply to scoring average. (default: 4)
  --weight-pars <float>
                        Relative weight to apply to pars per round average. (default: 2)
  --dump                Dump player data as JSON (default: False)
  --summary             Print data summary before power rankings (default: False)
  --player-filter <regex>
                        Show data for player names that match filter (default: .*)
```

