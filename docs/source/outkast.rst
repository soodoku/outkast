outkast: estimate caste by last name, year, and state
-----------------------------------------------------

.. image:: https://travis-ci.org/appeler/outkast.svg?branch=master
    :target: https://travis-ci.org/appeler/outkast
.. image:: https://ci.appveyor.com/api/projects/status/q4wr4clilf4samlk?svg=true
    :target: https://ci.appveyor.com/project/soodoku/outkast
.. image:: https://img.shields.io/pypi/v/outkast.svg
    :target: https://pypi.python.org/pypi/outkast
.. image:: https://pepy.tech/badge/outkast
    :target: https://pepy.tech/project/outkast


Using data on more than 420M Indians from the `Socio-Economic Caste Census <https://github.com/in-rolls/secc>`__ (parsed data `here <https://dataverse.harvard.edu/dataset.xhtml?persistentId=doi:10.7910/DVN/LIIBNB>`__), we estimate the proportion lower-caste for a particular last name, year, and state.

Why?
====

We provide this package so that people can find ways to assess, highlight, and fight unfairness. 

How is the underlying data produced?
====================================

We split name into first name and last name and then aggregated per state, year.

This is used to provide the base prediction.


Base Classifier
~~~~~~~~~~~~~~~

We start by providing a base model for last\_name that gives the Bayes
optimal solution providing the proportion of `SC, ST, and Other` with that last name. 
We also provide a series of base models where the state of
residence is known.

Installation
~~~~~~~~~~~~

We strongly recommend installing `outkast` inside a Python virtual environment (see `venv documentation <https://docs.python.org/3/library/venv.html#creating-virtual-environments>`__)

::

    pip install outkast


Usage
~~~~~

::

    usage: secc_caste [-h] -l LAST_NAME
                    [-s {arunachal pradesh,assam,bihar,chhattisgarh,gujarat,haryana,kerala,madhya pradesh,maharashtra,mizoram,odisha,nagaland,punjab,rajasthan,sikkim,tamilnadu,uttar pradesh,uttarakhand,west bengal}]
                    [-y YEAR] [-o OUTPUT]
                    input

    Appends SECC 2011 data columns for sc, st, and other by last name

    positional arguments:
    input                 Input file

    optional arguments:
    -h, --help            show this help message and exit
    -l LAST_NAME, --last-name LAST_NAME
                            Name or index location of column contains the last
                            name
    -s {arunachal pradesh,assam,bihar,chhattisgarh,gujarat,haryana,kerala,madhya pradesh,maharashtra,mizoram,odisha,nagaland,punjab,rajasthan,sikkim,tamilnadu,uttar pradesh,uttarakhand,west bengal}, --state {arunachal pradesh,assam,bihar,chhattisgarh,gujarat,haryana,kerala,madhya pradesh,maharashtra,mizoram,odisha,nagaland,punjab,rajasthan,sikkim,tamilnadu,uttar pradesh,uttarakhand,west bengal}
                            State name of SECC data (default=all)
    -y YEAR, --year YEAR  Birth year in SECC data (default=all)
    -o OUTPUT, --output OUTPUT
                            Output file with SECC data columns



Using outkast
~~~~~~~~~~~~~

::

    >>> import pandas as pd
    >>> from outkast import secc_caste
    >>>
    >>> names = [{'name': 'patel'},
    ...          {'name': 'kohli'},
    ...          {'name': 'lal'},
    ...          {'name': 'agarwal'}]
    >>>
    >>> df = pd.DataFrame(names)
    >>>
    >>> secc_caste(df, 'name')
        name     n_sc    n_st  n_other   prop_sc   prop_st  prop_other
    0    patel    17043  336909  1894248  0.007581  0.149857    0.842562
    1    kohli      468      57      552  0.434540  0.052925    0.512535
    2      lal  2111632  725713  3943494  0.311412  0.107024    0.581564
    3  agarwal      117      36    13125  0.008812  0.002711    0.988477
    >>>
    >>> help(secc_caste)
    Help on method secc_caste in module outkast.secc_caste_ln:

    secc_caste(df, namecol, state=None, year=None) method of builtins.type instance
        Appends additional columns from SECC data to the input DataFrame
        based on the last name.

        Removes extra space. Checks if the name is the SECC data.
        If it is, outputs data from that row.

        Args:
            df (:obj:`DataFrame`): Pandas DataFrame containing the last name
                column.
            namecol (str or int): Column's name or location of the name in
                DataFrame.
            state (str): The state name of SECC data to be used.
                (default is None for all states)
            year (int): The year of SECC data to be used.
                (default is None for all years)

        Returns:
            DataFrame: Pandas DataFrame with additional columns:-
                'n_sc', 'n_st', 'n_other',
                'prop_sc', 'prop_st', 'prop_other' by last name


Authors
~~~~~~~

Suriyan Laohaprapanon and Gaurav Sood

License
~~~~~~~

The package is released under the `MIT
License <https://opensource.org/licenses/MIT>`__.
