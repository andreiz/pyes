RabbitMQ River
==============

RabbitMQ River allows to automatically index a rabbitmq queue. The format of the messages follows the bulk api format:


.. code-block:: js


    { "index" : { "index" : "twitter", "type" : "tweet", "id" : "1" } }
    { "tweet" : { "text" : "this is a tweet" } }
    { "delete" : { "index" : "twitter", "type" : "tweet", "id" : "2" } }
    { "create" : { "index" : "twitter", "type" : "tweet", "id" : "1" } }
    { "tweet" : { "text" : "another tweet" } }    


Creating the rabbitmq river is as simple as:


.. code-block:: js

    curl -XPUT 'localhost:9200/_river/my_river/_meta' -d '{
        "type" : "rabbitmq",
        "rabbitmq" : {
            "host" : "localhost", 
            "port" : 5672,
            "user" : "guest",
            "pass" : "guest",
            "vhost" : "/",
            "queue" : "elasticsearch",
            "exchange" : "elasticsearch",
            "routing_key" : "elasticsearch"
        },
        "index" : {
            "bulk_size" : 100,
            "bulk_timeout" : "10ms"
        }
    }'


The river is automatically bulking queue messages if the queue is overloaded, allowing for faster catchup with the messages streamed into the queue.


The rabbitmq river is provided as a plugin and can be installed using **plugin -install river-rabbitmq**.


