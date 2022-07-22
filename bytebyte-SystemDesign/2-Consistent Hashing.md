# Consistent Hashing

To achive horizontal scaling, requests should be distributed efficiently and evenly across servers.

## Rehashing problem

if there n cache servers, common way to load balance is to use

> serverIndex = hash(key)%n
> with this client can find the server where the key is stored.

if a new server is added or existing server is removed, everything goes for a toss.

## Consistent hashing

    - when a hash table is re-sized and consistent hashing is used, only k/n keys need to be remapped on average.
    - if we are using SHA-1, output range of hash function is x0, x1,.. xn and the hash space goes from 0 to 2^160-1.
    - hash ring is created by joining x0 and xn.
    - using the same hash fn, servers are mapped to hash ring.
    - then hash keys are mapped to the ring.
    - to check the location of a key, go clockwise until a server is found.
    - when a new server is added, you need to go anti-clockwise until a server is found and redistribute only those keys to new server.
    - when a server is removed, go anti-clockwise and get all the keys and add it to the next server.
    - Issues
        - impossible to keep same size partitions(hash space) for all servers
        - non-uniform key distribtuion on the ring.
    - workarounds
        - add vitrual nodes- each server has multiple nodes on the ring. As the number of nodes increases, redistribution becomes more balanced.
    - Real world
        - partitioning component of Dynamo database
        - Data partitioning in Apache Cassandra
        - Discord chat app
