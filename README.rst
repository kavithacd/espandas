espandas
========

|PyPI version| |Build Status| |Coverage Status|

Reading and writing `pandas <http://pandas.pydata.org/>`__ DataFrames in
`ElasticSearch <https://www.elastic.co/>`__

Requirements.
-------------

This package should work on both python 2(>=2.7) and 3(>=3.4) but has
primarily been tested on python 2.7. ElasticSearch is of course required
and should be version 2.x.

Installation
------------

The package is hosted on PyPi and can be installed with pip:

::

    pip install espandas

Alternatively, the development version from Github can be installed:

::

    pip install git+git://github.com/dashaub/espandas.git

Tests
-----

Unit tests can be run with pytest or nosetests. Code coverage can be
established with pytest-cov from PyPi:

::

    py.test --cov=espandas

Usage
-----

This example assumes ElasticSearch is running on localhost on the
standard port. If different connection infromation needs to be
specified, it can be passed to the ``Espandas()`` constructor as keyward
arguments. The DataFrame to insert ***must*** have a column that will be
used for the unique identifier ``_id`` in ElasticSearch: the default
value is ``uid_name = 'indexId'``.

::

    import pandas as pd
    import numpy as np
    from espandas import Espandas

    # Example data frame
    df = (100 * pd.DataFrame(np.round(np.random.rand(100, 5), 2))).astype(int)
    df.columns = ['A', 'B', 'C', 'D', 'E']
    df['indexId'] = (df.index + 100).astype(str)

    # Create a client and write the DataFrame. If necessary, connection
    # information to the ES cluster can be passed in the espandas constructor
    # as keyword arguments.
    INDEX = 'foo_index'
    TYPE = 'bar_type'
    esp = Espandas()
    esp.es_write(df, INDEX, TYPE)


    # Query for the first ten rows and see that they match the original
    k = df.indexId[0:10]
    res = esp.es_read(k, INDEX, TYPE)
    res == df.iloc[0:10].astype('str')

License
-------

(c) 2017 David Shaub

This package is free software released under the
`GPL-3 <http://www.gnu.org/licenses/gpl-3.0.en.html>`__ license.

.. |PyPI version| image:: https://badge.fury.io/py/espandas.svg
   :target: https://badge.fury.io/py/espandas
.. |Build Status| image:: https://travis-ci.org/dashaub/espandas.svg?branch=master
   :target: https://travis-ci.org/dashaub/espandas
.. |Coverage Status| image:: https://coveralls.io/repos/github/dashaub/espandas/badge.svg?branch=master
   :target: https://coveralls.io/github/dashaub/espandas?branch=master
