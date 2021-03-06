More Like This
==============

The more like this (mlt) API allows to get documents that are "like" a specified document. Here is an example:


.. code-block:: js

    $ curl -XGET 'http://localhost:9200/twitter/tweet/1/_mlt?fields=tag,content&min_doc_freq=1'


The API simply results in executing a search request with :doc:`moreLikeThis <./../query_dsl/more_like_this_query/index>` query (http parameters match the parameters to the **more_like_this** query). This means that the body of the request can optionally include all the request body options in the :doc:`search API <./../search/index>` (facets, from/to and so on).


Rest parameters relating to search are also allowed, including **search_type**, **search_indices**, **search_types**, and **search_scroll**.


When no **fields** are specified, all the fields of the document will be used in the **more_like_this** query generated.

