Geo Distance Facet
==================

The geo_distance facet is a facet providing information for ranges of distances from a provided geo_point including count of the number of hits that fall within each range, and aggregation information (like total). 


Assuming the following sample doc:


.. code-block:: js


    {
        "pin" : {
            "location" : {
                "lat" : 40.12,
                "lon" : -71.34
            }
        }
    }


Here is an example that create a **geo_distance** facet from a **pin.location** of 40,-70, and a set of ranges:


.. code-block:: js


    {
        "query" : {
            "match_all" : {}
        },
        "facets" : {
            "geo1" : {
                "geo_distance" : {
                    "pin.location" : {
                        "lat" : 40,
                        "lon" : -70
                    },
                    "ranges" : [
                        { "to" : 10 },
                        { "from" : 10, "to" : 20 },
                        { "from" : 20, "to" : 100 },
                        { "from" : 100 }
                    ]
                }
            }
        }
    }


Accepted Formats
----------------

In much the same way the geo_point type can accept different representation of the geo point, the filter can accept it as well:


Lat Lon As Properties
"""""""""""""""""""""

.. code-block:: js


    {
        "query" : {
            "match_all" : {}
        },
        "facets" : {
            "geo1" : {
                "geo_distance" : {
                    "pin.location" : {
                        "lat" : 40,
                        "lon" : -70
                    },
                    "ranges" : [
                        { "to" : 10 },
                        { "from" : 10, "to" : 20 },
                        { "from" : 20, "to" : 100 },
                        { "from" : 100 }
                    ]
                }
            }
        }
    }


Lat Lon As Array
""""""""""""""""

.. code-block:: js


    {
        "query" : {
            "match_all" : {}
        },
        "facets" : {
            "geo1" : {
                "geo_distance" : {
                    "pin.location" : [40, -70],
                    "ranges" : [
                        { "to" : 10 },
                        { "from" : 10, "to" : 20 },
                        { "from" : 20, "to" : 100 },
                        { "from" : 100 }
                    ]
                }
            }
        }
    }


Lat Lon As String
"""""""""""""""""

.. code-block:: js


    {
        "query" : {
            "match_all" : {}
        },
        "facets" : {
            "geo1" : {
                "geo_distance" : {
                    "pin.location" : "40, -70",
                    "ranges" : [
                        { "to" : 10 },
                        { "from" : 10, "to" : 20 },
                        { "from" : 20, "to" : 100 },
                        { "from" : 100 }
                    ]
                }
            }
        }
    }


Geohash
"""""""

.. code-block:: js


    {
        "query" : {
            "match_all" : {}
        },
        "facets" : {
            "geo1" : {
                "geo_distance" : {
                    "pin.location" : "drm3btev3e86",
                    "ranges" : [
                        { "to" : 10 },
                        { "from" : 10, "to" : 20 },
                        { "from" : 20, "to" : 100 },
                        { "from" : 100 }
                    ]
                }
            }
        }
    }


Ranges
------

When a **to** or **from** are not set, they are assumed to be unbounded. Ranges are allowed to overlap, basically, each range is treated by itself.


Options
-------

===================  ===================================================================================================================
 Option               Description                                                                                                       
===================  ===================================================================================================================
**unit**             The unit the ranges are provided in. Defaults to **km**. Can also be **mi** or **miles**.                          
**distance_type**    How to compute the distance. Can either be **arc** (better precision) or **plane** (faster). Defaults to **arc**.  
===================  ===================================================================================================================

Value Options
-------------

On top of the count of hits falling within each range, aggregated data can be provided (total) as well. By default, the aggregated data will simply use the distance calculated, but the value can be extracted either using a different numeric field, or a script. Here is an example of using a different numeric field:


.. code-block:: js


    {
        "query" : {
            "match_all" : {}
        },
        "facets" : {
            "geo1" : {
                "geo_distance" : {
                    "pin.location" : "drm3btev3e86",
                    "value_field" : "num1",
                    "ranges" : [
                        { "to" : 10 },
                        { "from" : 10, "to" : 20 },
                        { "from" : 20, "to" : 100 },
                        { "from" : 100 }
                    ]
                }
            }
        }
    }


And here is an example of using a script:


.. code-block:: js


    {
        "query" : {
            "match_all" : {}
        },
        "facets" : {
            "geo1" : {
                "geo_distance" : {
                    "pin.location" : "drm3btev3e86",
                    "value_script" : "doc['num1'].value * factor",
                    "params" : {
                        "factor" : 5
                    }
                    "ranges" : [
                        { "to" : 10 },
                        { "from" : 10, "to" : 20 },
                        { "from" : 20, "to" : 100 },
                        { "from" : 100 }
                    ]
                }
            }
        }
    }


Note the params option, allowing to pass parameters to the script (resulting in faster script execution instead of providing the values within the script each time).


**geo_point** Type
----------------

The facet *requires* the **geo_point** type to be set on the relevant field.


Multi Location Per Document
---------------------------

The facet can work with multiple locations per document.

