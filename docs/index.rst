.. _index:

#####################
CrateDB Python Client
#####################

.. rubric:: Table of contents

.. contents::
    :local:
    :depth: 1


************
Introduction
************

The Python client library for `CrateDB`_ implements the Python Database API
Specification v2.0 (`PEP 249`_), and also includes the :ref:`CrateDB dialect
<using-sqlalchemy>` for `SQLAlchemy`_.

The Python driver can be used to connect to both `CrateDB`_ and `CrateDB
Cloud`_, and is verified to work on Linux, macOS, and Windows. It is used by
the `Crash CLI`_, as well as other libraries and applications connecting to
CrateDB from the Python ecosystem. It is verified to work with CPython, but
it has also been tested successfully with `PyPy`_.

Please make sure to also visit the section about :ref:`other-options`, using
the :ref:`crate-reference:interface-postgresql` interface of `CrateDB`_.


*************
Documentation
*************

For general help about the Python Database API, or SQLAlchemy, please consult
`PEP 249`_, the `SQLAlchemy tutorial`_, and the general `SQLAlchemy
documentation`_.

For more detailed information about how to install the client driver, how to
connect to a CrateDB cluster, and how to run queries, consult the resources
referenced below. A dedicated section demonstrates how to use the :ref:`blob
storage capabilities <crate-reference:blob_support>` of CrateDB.

.. toctree::
    :titlesonly:

    getting-started
    connect
    query
    blobs


DB API
======

Install package from PyPI.

.. code-block:: shell

    pip install crate

Connect to CrateDB instance running on ``localhost``.

.. code-block:: python

    # Connect using DB API.
    from crate import client
    from pprint import pp

    query = "SELECT country, mountain, coordinates, height FROM sys.summits ORDER BY country;"
    
    with client.connect("localhost:4200", username="crate") as connection:
        cursor = connection.cursor()
        cursor.execute(query)
        pp(cursor.fetchall())
        cursor.close()

Connect to `CrateDB Cloud`_.

.. code-block:: python

    # Connect using DB API.
    from crate import client
    connection = client.connect(
        servers="https://example.aks1.westeurope.azure.cratedb.net:4200",
        username="admin",
        password="<PASSWORD>")


SQLAlchemy
==========

The CrateDB dialect for `SQLAlchemy`_ offers convenient ORM access and supports
CrateDB's ``OBJECT``, ``ARRAY``, and geospatial data types using `GeoJSON`_,
supporting different kinds of `GeoJSON geometry objects`_.

.. toctree::
    :maxdepth: 2

    sqlalchemy

Install package from PyPI with DB API and SQLAlchemy support.

.. code-block:: shell

    pip install 'crate[sqlalchemy]' pandas

Connect to CrateDB instance running on ``localhost``.

.. code-block:: python

    # Connect using SQLAlchemy Core.
    import pkg_resources
    import sqlalchemy as sa
    from pprint import pp

    pkg_resources.require("sqlalchemy>=2.0")
    
    dburi = "crate://localhost:4200"
    query = "SELECT country, mountain, coordinates, height FROM sys.summits ORDER BY country;"
    
    engine = sa.create_engine(dburi, echo=True)
    with engine.connect() as connection:
        with connection.execute(sa.text(query)) as result:
            pp(result.mappings().fetchall())

Connect to `CrateDB Cloud`_.

.. code-block:: python

    # Connect using SQLAlchemy Core.
    import sqlalchemy as sa
    dburi = "crate://admin:<PASSWORD>@example.aks1.westeurope.azure.cratedb.net:4200?ssl=true"
    engine = sa.create_engine(dburi, echo=True)

Load results into `pandas`_ DataFrame.

.. code-block:: python

    # Connect using SQLAlchemy Core and pandas.
    import pandas as pd
    import sqlalchemy as sa
    
    dburi = "crate://localhost:4200"
    query = "SELECT * FROM sys.summits ORDER BY country;"
    
    engine = sa.create_engine(dburi, echo=True)
    with engine.connect() as connection:
        df = pd.read_sql(sql=sa.text(query), con=connection)
        df.info()
        print(df)


Data types
==========

The DB API driver and the SQLAlchemy dialect support :ref:`CrateDB's data types
<crate-reference:data-types>` to different degrees. For more information,
please consult the :ref:`data-types` and :ref:`SQLAlchemy extension types
<using-extension-types>` documentation pages.

Examples
========

- The :ref:`by-example` section enumerates concise examples demonstrating the
  different API interfaces of the CrateDB Python client library. Those are
  DB API, HTTP, and BLOB interfaces, and the SQLAlchemy dialect.
- The `sample application`_ and the corresponding `sample application
  documentation`_ demonstrate the use of the driver on behalf of an example
  "guestbook" application.
- `Use CrateDB with pandas`_ has corresponding code snippets about how to
  connect to CrateDB using `pandas`_, and how to load and export data.
- The `Apache Superset`_ and `FIWARE QuantumLeap data historian`_ projects.


*******************
Project information
*******************

Resources
=========
- `Source code <https://github.com/crate/crate-python>`_
- `Documentation <https://crate.io/docs/python/>`_
- `Python Package Index (PyPI) <https://pypi.org/project/crate/>`_

Contributions
=============
The CrateDB Python client library is an open source project, and is `managed on
GitHub`_.
Every kind of contribution, feedback, or patch, is much welcome. `Create an
issue`_ or submit a patch if you think we should include a new feature, or to
report or fix a bug.

Development
===========
In order to setup a development environment on your workstation, please head
over to the `development sandbox`_ documentation. When you see the software
tests succeed, you should be ready to start hacking.

Page index
==========
The full index for all documentation pages can be inspected at :ref:`index-all`.

License
=======
The project is licensed under the terms of the Apache 2.0 license, like
`CrateDB itself <CrateDB source_>`_, see `LICENSE`_.


.. _Apache Superset: https://github.com/apache/superset
.. _Crash CLI: https://crate.io/docs/crate/crash/
.. _CrateDB: https://crate.io/products/cratedb
.. _CrateDB Cloud: https://console.cratedb.cloud/
.. _CrateDB source: https://github.com/crate/crate
.. _Create an issue: https://github.com/crate/crate-python/issues
.. _development sandbox: https://github.com/crate/crate-python/blob/master/DEVELOP.rst
.. _FIWARE QuantumLeap data historian: https://github.com/orchestracities/ngsi-timeseries-api
.. _GeoJSON: https://geojson.org/
.. _GeoJSON geometry objects: https://tools.ietf.org/html/rfc7946#section-3.1
.. _LICENSE: https://github.com/crate/crate-python/blob/master/LICENSE
.. _managed on GitHub: https://github.com/crate/crate-python
.. _pandas: https://pandas.pydata.org/
.. _PEP 249: https://peps.python.org/pep-0249/
.. _PyPy: https://www.pypy.org/
.. _sample application: https://github.com/crate/crate-sample-apps/tree/main/python-flask
.. _sample application documentation: https://github.com/crate/crate-sample-apps/blob/main/python-flask/documentation.md
.. _SQLAlchemy: https://www.sqlalchemy.org/
.. _SQLAlchemy documentation: https://docs.sqlalchemy.org/
.. _SQLAlchemy tutorial: https://docs.sqlalchemy.org/en/latest/tutorial/
.. _Use CrateDB with pandas: https://github.com/crate/crate-qa/pull/246
