# GSA Digital Innovation Hackathon - Fall 2015

Files in the [hackathon](/hackathon) directory represent hackathon rules and guidance [provided by the GSA](https://github.com/GSA/open.gsa.gov/tree/4a6296d66df4e313ac4901c025c5f5338d271152/Digital-Innovation-Hackathon-Fall2015).

Files in the [data](/data) directory represent machine-readable versions of the data provided by GSA. The [gge_to_ghg_conversions.csv](/data/gge_to_ghg_conversions.csv) file needed manual manipulation to make it machine-readable, but the other files have not been altered in any way.

Files in the root directory comprise a solution to the [Greenhouse Gas (GHG) hackathon challenge](GHG.md).

## Usage

View the tool live at http://s2t2.github.io/gsa-hackathon-2015/

## Development

Download source.

```` sh
git clone git@github.com:s2t2/gsa-hackathon-2015.git
````

Start a local web server.

```` sh
cd gsa-hackathon-2015/
python -m SimpleHTTPServer 8888 &
````

Visit http://localhost:8888/ in a browser.
