Client
======

Obtaining an elasticsearch Groovy **GClient** (a **GClient** is a simple wrapper on top of the Java **Client**) is simple. The most common way to get a client is by starting an embedded **Node** which acts as a node within the cluster.


Node Client
-----------

A Node based client is the simplest form to get a **GClient** to start executing operations against elasticsearch.


.. code-block:: js

    import org.elasticsearch.groovy.node.GNode
    import org.elasticsearch.groovy.node.GNodeBuilder
    import static org.elasticsearch.groovy.node.GNodeBuilder.*
    
    // on startup
    
    GNode node = nodeBuilder().node();
    GClient client = node.client();
    
    // on shutdown
    
    node.close();


Since elasticsearch allows to configure it using JSON based settings, the configuration itself can be done using a closure that represent the JSON:


.. code-block:: js

    import org.elasticsearch.groovy.node.GNode
    import org.elasticsearch.groovy.node.GNodeBuilder
    import static org.elasticsearch.groovy.node.GNodeBuilder.*
    
    // on startup
    
    GNodeBuilder nodeBuilder = nodeBuilder();
    nodeBuilder.settings {
        node {
            client = true
        }
        cluster {
            name = "test"
        }
    }
    
    GNode node = nodeBuilder.node()
    
    // on shutdown
    
    node.stop().close()

