## power_rankings
This package provides 2 scripts: **power-rankings** and **refresh-bbc-data**.

### Installation
To create power rankings you need to install 2 packages, both of them
are included in this repo.

First install *golfgenius*:
```shell script
cd golfgenius
python setup.py install
cd ..
```
Once that is installed you can install *power_rankings*:

```shell script
cd power_rankings
python setup.py install
cd ..
```
After that you should be able to run ``bbc-refresh-data --help`` and ``power-rankings --help`` to verify the 
installatiopn was successful. If you get ``command not found`` running either of those then the installation failed.

### Creating Power Rankings
This tutorial will create a power ranking using these settings:

1. Rounds in the last 2 weeks
2. Players must have played atleast 1 round to be included
3. Weight the most recent round heavier
4. Consider birdies twice as important as scoring which is twice as important as pars.

The first step we need to do is collect the data we need from Golf Genius.

We need to use a GGID code, this can be ANY GGID code from ANY round in the BBC group.

The example below tells the **refresh-bbc-data** tool to collect the score data 
from rounds that have November or December in their name and store the results in a directory named *results*:

```shell script
refresh-bbc-data --results-dir='./results' --filter='.*(November|December).*'  <ggid>
```
**NOTE:** Don't forget to substitue ``<ggid>`` in the command above with a valid bbc ggid!

Output:

```shell script
Fri Dec 18 19:13:58 <root:refresh_bbc_data[66]> INFO: Refreshing BBC Results from GGID ***** to ./results using filter .*(November|December).*
Fri Dec 18 19:14:01 <golfgenius.parser:parser[170]> INFO: Logging into https://www.golfgenius.com/golfgenius
Fri Dec 18 19:14:34 <golfgenius.parser:parser[191]> INFO: Discovered 11 rounds matching pattern .*(November|December).*. (53 total)
Fri Dec 18 19:14:46 <golfgenius.parser:parser[210]> INFO: Stored Round 57 (Sun, December 13) (12 players)
Fri Dec 18 19:14:58 <golfgenius.parser:parser[210]> INFO: Stored Round 56 (Fri, December 11) (12 players)
Fri Dec 18 19:15:10 <golfgenius.parser:parser[210]> INFO: Stored Round 55 (Fri, December  4) (16 players)
Fri Dec 18 19:15:22 <golfgenius.parser:parser[210]> INFO: Stored Round 54 (Sun, November 29) (12 players)
Fri Dec 18 19:15:34 <golfgenius.parser:parser[210]> INFO: Stored Round 53 (Fri, November 27) (16 players)
Fri Dec 18 19:15:46 <golfgenius.parser:parser[210]> INFO: Stored Round 52 (Wed, November 25) (12 players)
Fri Dec 18 19:15:57 <golfgenius.parser:parser[210]> INFO: Stored Round 51 (Tue, November 24) (8 players)
Fri Dec 18 19:16:10 <golfgenius.parser:parser[210]> INFO: Stored Round 50 (Fri, November 20) (16 players)
Fri Dec 18 19:16:21 <golfgenius.parser:parser[210]> INFO: Stored Round 49 (Sat, November 14) (12 players)
Fri Dec 18 19:16:32 <golfgenius.parser:parser[210]> INFO: Stored Round 48 (Tue, November 10) (12 players)
Fri Dec 18 19:16:44 <golfgenius.parser:parser[210]> INFO: Stored Round 47 (Fri, November  6) (12 players)
Fri Dec 18 19:16:45 <root:refresh_bbc_data[79]> INFO: Finished refreshing GGID *****. Results have been stored in ./results
```

We now have our score data in *results*:

```shell script
$ ls results
Round 47 (Fri, November  6).json	Round 50 (Fri, November 20).json	Round 53 (Fri, November 27).json	Round 56 (Fri, December 11).json
Round 48 (Tue, November 10).json	Round 51 (Tue, November 24).json	Round 54 (Sun, November 29).json	Round 57 (Sun, December 13).json
Round 49 (Sat, November 14).json	Round 52 (Wed, November 25).json	Round 55 (Fri, December  4).json
$ 
```

The next step is to use **power-rankings** to compute the rankings.
We will run **power-rankings** using the following options:

- ``--weeks=2``
  - Evaluate the last 2 weeks of rounds
- ``min-rounds=1``
  - Only consider players who have played at least 1 round
- ``weighted-rounds=1``
  - Consider the most recent round more important and weigh stats heavier
- ``weight-birdies=4``
  - We want birdies to be the most important stat so these are weighted the highest.
- ``weight-scoring=2``
  - Scoring should be half as important as birdies
- ``weight-pars=1``
  - Pars should be half as important as scoring
- ``--summary``
  - Tells **power-rankings** to print out a summary of additional stats above the power rankings table
- ``--results-directory``
  - This is where **power-rankings** loads the scoring data from. We collected this earlier into *results/*

Here is the full command:

```shell script
power-rankings --weeks=2 --min-rounds=1 --weighted-rounds=1 --weight-birdies=4 --weight-scoring=2 --weight-pars=1 --summary --results-directory=results
```

Output:
```shell script
┌─────────┐
│ Players │
├─────────┤
│ 18      │
└─────────┘
┌Round Counts────────────────┬────────┐
│ Rank │ Player              │ Rounds │
├──────┼─────────────────────┼────────┤
│ 1    │ Hilliard, Tj        │ 2      │
│ 2    │ Zogby, Kevin        │ 2      │
│ 3    │ Smith, Brian        │ 2      │
│ 4    │ Perry, Robbie       │ 2      │
│ 5    │ McDougald, Kevin    │ 2      │
│ 6    │ Fish, Tony          │ 2      │
│ 7    │ Welton, Craig       │ 1      │
│ 8    │ Ellzey, Matt        │ 1      │
│ 9    │ Rogers, Ken         │ 1      │
│ 10   │ Samuel, Matthew     │ 1      │
│ 11   │ Alford, Sumner      │ 1      │
│ 12   │ James, Brian        │ 1      │
│ 13   │ Parker, Shane       │ 1      │
│ 14   │ Corcoran, Scott     │ 1      │
│ 15   │ Shoffner, Chris     │ 1      │
│ 16   │ Stefanacci, Michael │ 1      │
│ 17   │ Beaird, Ray         │ 1      │
│ 18   │ Capwell, Robert     │ 1      │
└──────┴─────────────────────┴────────┘
┌Scoring Average─────────────┬─────────┬────────┐
│ Rank │ Player              │ Average │ Trend  │
├──────┼─────────────────────┼─────────┼────────┤
│ 1    │ Hilliard, Tj        │ 69.000  │ 0.000  │
│ 2    │ Parker, Shane       │ 71.000  │ 0.000  │
│ 3    │ Perry, Robbie       │ 72.000  │ -1.000 │
│ 4    │ Smith, Brian        │ 72.000  │ 0.000  │
│ 5    │ Capwell, Robert     │ 74.000  │ 0.000  │
│ 6    │ Rogers, Ken         │ 74.000  │ 0.000  │
│ 7    │ Fish, Tony          │ 75.500  │ -0.500 │
│ 8    │ Samuel, Matthew     │ 75.000  │ 0.000  │
│ 9    │ Stefanacci, Michael │ 75.000  │ 0.000  │
│ 10   │ Beaird, Ray         │ 76.000  │ 0.000  │
│ 11   │ Corcoran, Scott     │ 76.000  │ 0.000  │
│ 12   │ Alford, Sumner      │ 77.000  │ 0.000  │
│ 13   │ Welton, Craig       │ 79.000  │ 0.000  │
│ 14   │ Zogby, Kevin        │ 80.000  │ -0.333 │
│ 15   │ McDougald, Kevin    │ 78.500  │ 1.833  │
│ 16   │ Shoffner, Chris     │ 82.000  │ 0.000  │
│ 17   │ Ellzey, Matt        │ 83.000  │ 0.000  │
│ 18   │ James, Brian        │ 85.000  │ 0.000  │
└──────┴─────────────────────┴─────────┴────────┘
┌Birdies per Round Average───┬─────────┬────────┐
│ Rank │ Player              │ Average │ Trend  │
├──────┼─────────────────────┼─────────┼────────┤
│ 1    │ Parker, Shane       │ 6.000   │ 0.000  │
│ 2    │ Perry, Robbie       │ 3.500   │ 0.500  │
│ 3    │ Hilliard, Tj        │ 4.000   │ -0.333 │
│ 4    │ Smith, Brian        │ 4.000   │ -0.333 │
│ 5    │ Corcoran, Scott     │ 3.000   │ 0.000  │
│ 6    │ Rogers, Ken         │ 3.000   │ 0.000  │
│ 7    │ Stefanacci, Michael │ 3.000   │ 0.000  │
│ 8    │ Alford, Sumner      │ 2.000   │ 0.000  │
│ 9    │ Capwell, Robert     │ 2.000   │ 0.000  │
│ 10   │ Samuel, Matthew     │ 2.000   │ 0.000  │
│ 11   │ Welton, Craig       │ 2.000   │ 0.000  │
│ 12   │ Zogby, Kevin        │ 1.000   │ 0.333  │
│ 13   │ Beaird, Ray         │ 1.000   │ 0.000  │
│ 14   │ Ellzey, Matt        │ 1.000   │ 0.000  │
│ 15   │ Fish, Tony          │ 1.000   │ 0.000  │
│ 16   │ McDougald, Kevin    │ 1.500   │ -0.500 │
│ 17   │ Shoffner, Chris     │ 1.000   │ 0.000  │
│ 18   │ James, Brian        │ 0.000   │ 0.000  │
└──────┴─────────────────────┴─────────┴────────┘
┌Pars per Round Average──────┬─────────┬────────┐
│ Rank │ Player              │ Average │ Trend  │
├──────┼─────────────────────┼─────────┼────────┤
│ 1    │ Hilliard, Tj        │ 13.500  │ 0.500  │
│ 2    │ Fish, Tony          │ 13.000  │ 0.333  │
│ 3    │ Beaird, Ray         │ 13.000  │ 0.000  │
│ 4    │ Capwell, Robert     │ 12.000  │ 0.000  │
│ 5    │ Samuel, Matthew     │ 12.000  │ 0.000  │
│ 6    │ Smith, Brian        │ 10.500  │ 0.833  │
│ 7    │ Perry, Robbie       │ 11.000  │ 0.000  │
│ 8    │ Alford, Sumner      │ 10.000  │ 0.000  │
│ 9    │ Rogers, Ken         │ 10.000  │ 0.000  │
│ 10   │ McDougald, Kevin    │ 10.000  │ -0.333 │
│ 11   │ Corcoran, Scott     │ 9.000   │ 0.000  │
│ 12   │ Stefanacci, Michael │ 9.000   │ 0.000  │
│ 13   │ Zogby, Kevin        │ 9.000   │ -0.333 │
│ 14   │ Parker, Shane       │ 8.000   │ 0.000  │
│ 15   │ James, Brian        │ 7.000   │ 0.000  │
│ 16   │ Shoffner, Chris     │ 7.000   │ 0.000  │
│ 17   │ Welton, Craig       │ 7.000   │ 0.000  │
│ 18   │ Ellzey, Matt        │ 6.000   │ 0.000  │
└──────┴─────────────────────┴─────────┴────────┘
┌Power Rankings──────────────┬───────────────┬─────────────────┬───────────────────┬─────────────────┬────────┐
│ Rank │ Player              │ Power Ranking │ Scoring         │ Birdies or Better │ Pars            │ Rounds │
├──────┼─────────────────────┼───────────────┼─────────────────┼───────────────────┼─────────────────┼────────┤
│ 1    │ Hilliard, Tj        │ 46.667        │ 69.000          │ 3.667 (-0.333)    │ 14.000 (+0.500) │ 2      │
│ 2    │ Perry, Robbie       │ 36.842        │ 71.000 (-1.000) │ 4.000 (+0.500)    │ 11.000          │ 2      │
│ 3    │ Parker, Shane       │ 31.818        │ 71.000          │ 6.000             │ 8.000           │ 1      │
│ 4    │ Smith, Brian        │ 26.923        │ 72.000          │ 3.667 (-0.333)    │ 11.333 (+0.833) │ 2      │
│ 5    │ Rogers, Ken         │ 18.421        │ 74.000          │ 3.000             │ 10.000          │ 1      │
│ 6    │ Stefanacci, Michael │ 15.556        │ 75.000          │ 3.000             │ 9.000           │ 1      │
│ 7    │ Capwell, Robert     │ 15.217        │ 74.000          │ 2.000             │ 12.000          │ 1      │
│ 8    │ Samuel, Matthew     │ 14.000        │ 75.000          │ 2.000             │ 12.000          │ 1      │
│ 9    │ Corcoran, Scott     │ 13.725        │ 76.000          │ 3.000             │ 9.000           │ 1      │
│ 10   │ Alford, Sumner      │ 10.938        │ 77.000          │ 2.000             │ 10.000          │ 1      │
│ 11   │ Fish, Tony          │ 10.294        │ 75.000 (-0.500) │ 1.000             │ 13.333 (+0.333) │ 2      │
│ 12   │ Welton, Craig       │ 9.589         │ 79.000          │ 2.000             │ 7.000           │ 1      │
│ 13   │ Beaird, Ray         │ 9.333         │ 76.000          │ 1.000             │ 13.000          │ 1      │
│ 14   │ Zogby, Kevin        │ 7.865         │ 79.667 (-0.333) │ 1.333 (+0.333)    │ 8.667 (-0.333)  │ 2      │
│ 15   │ McDougald, Kevin    │ 7.609         │ 80.333 (+1.833) │ 1.000 (-0.500)    │ 9.667 (-0.333)  │ 2      │
│ 16   │ Shoffner, Chris     │ 7.071         │ 82.000          │ 1.000             │ 7.000           │ 1      │
│ 17   │ Ellzey, Matt        │ 6.731         │ 83.000          │ 1.000             │ 6.000           │ 1      │
│ 18   │ James, Brian        │ 5.691         │ 85.000          │ 0.000             │ 7.000           │ 1      │
└──────┴─────────────────────┴───────────────┴─────────────────┴───────────────────┴─────────────────┴────────┘
```
In addition to the output printed to the screen, **power-rankings** also creates 
an excel file named ``PowerRankings-MM-DD-YYY.xlsx`` in the current working directory.

For more information, see the full usage options for **refresh-bbc-data** and **power-rankings** below:

#### refresh-bbc-data
```shell script
usage: refresh-bbc-data [-h] [--results-dir <PATH>] [--disable-screenshots] [--screenshots-dir <PATH>] [--filter <REGEX>] [--show-browser] [--quiet] [--logfile <PATH>] [--debug] <GGID-CODE>

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
```shell script
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

