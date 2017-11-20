![](paper/mordecai_geoparsing.png)

Full text geoparsing as a Python library. Extract the place names from a piece of
text, resolve them to the correct place, and return their coordinates and
structured geographic information.

Example usage
-------------

```
>>> from mordecai import Geoparse
>>> geo = Geoparse()
>>> geo.geoparse("I traveled from Oxford to Lima.")

[{'country_conf': 0.96474487,
  'country_predicted': 'GBR',
  'geo': {'admin1': 'to do',
   'country_code3': 'GBR',
   'feature_class': 'P',
   'feature_code': 'PPLA2',
   'geonameid': '2640729',
   'lat': '51.75222',
   'lon': '-1.25596',
   'place_name': 'Oxford'},
  'spans': [{'end': 22, 'start': 16}],
  'word': 'Oxford'},
 {'country_conf': 0.99259007,
  'country_predicted': 'PER',
  'geo': {'admin1': 'to do',
   'country_code3': 'PER',
   'feature_class': 'P',
   'feature_code': 'PPLC',
   'geonameid': '3936456',
   'lat': '-12.04318',
   'lon': '-77.02824',
   'place_name': 'Lima'},
  'spans': [{'end': 30, 'start': 26}],
  'word': 'Lima'}]
```

`mordecai` requires a running Elasticsearch service with Geonames in it. See
"Installation" below for instructions.


Installation and Use
--------------------

Mordecai is on PyPI and can be installed with pip:

```
pip install mordecai
```

In order to work, Mordecai needs access to a Geonames gazetteer running in
Elasticsearch. The easiest way to set it up is by running the following
commands (you must have [Docker](https://docs.docker.com/engine/installation/)
installed first).

```
docker pull elasticsearch:5.5.2
wget https://s3.amazonaws.com/ahalterman-geo/geonames_index.tar.gz --output-file=wget_log.txt
tar -xzf geonames_index.tar.gz
docker run -d -p 127.0.0.1:9200:9200 -v $(pwd)/geonames_index/:/usr/share/elasticsearch/data elasticsearch:5.5.2
```

You can then run Mordecai as above.

How does it work?
-----------------

`Mordecai` accepts text and returns structured geographic information extracted
from it. 

- It uses [spaCy](https://github.com/explosion/spaCy/)'s named entity recognition to
  extract placenames from the text.

- It uses a the [geonames](http://www.geonames.org/)
  gazetteer in an [Elasticsearch](https://www.elastic.co/products/elasticsearch) index 
  (with some custom logic) to find potential the potential coordinates of
  extracted place names.

- It uses neural networks implemented in [Keras](https://keras.io/) and trained on new annotated
  data to infer the correct country and gazetteer entries for each
  placename. 

See `paper/Mordecai_whitepaper.pdf` for more details.

The training data for the two models includes copyrighted text so cannot be
shared freely, but get in touch with me if you're interested in it.

Tests
-----

`mordecai` includes a few unit tests. To run the tests:

```
py.test
```

The tests require access to a running Elastic/Geonames service to
complete. 


Acknowledgements
----------------

An earlier verion of this software was donated to the Open Event Data Alliance
by Caerus Associates.  See [Releases](https://github.com/openeventdata/mordecai/releases) for the
2015-2016 and the 2016-2017 production versions of Mordecai.

This work was funded in part by DARPA's XDATA program, the U.S. Army Research
Laboratory and the U.S. Army Research Office through the Minerva Initiative
under grant number W911NF-13-0332, and the National Science Foundation under
award number SBE-SMA-1539302. Any opinions, findings, and conclusions or
recommendations expressed in this material are those of the authors and do not
necessarily reflect the views of DARPA, ARO, Minerva, NSF, or the U.S.
government.

Citing
------

If you use this software in academic work, please cite as 

Andrew Halterman, (2017). Mordecai: Full Text Geoparsing and Event Geocoding. *Journal of Open Source
Software*, 2(9), 91, doi:10.21105/joss.00091

```
@article{halterman2017mordecai,
  title={Mordecai: Full Text Geoparsing and Event Geocoding},
  author={Halterman, Andrew},
  journal={The Journal of Open Source Software},
  volume={2},
  number={9},
  year={2017},
  doi={10.21105/joss.00091}
}
```

Send a note if you use Mordecai! It's always interesting to hear what people
are doing with it and whether it's doing what they want it to.

Contributing
------------

Contributions via pull requests are welcome. Please make sure that changes
pass the unit tests. Any bugs and problems can be reported
on the repo's [issues page](https://github.com/openeventdata/mordecai/issues).

