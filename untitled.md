# Audio Search Engine

Algorithm:

1. Graph the frequency and amplitude of each point in time in the song.
2. Find the points having high amplitude and frequency variation in this graph.
3. Take all k consecutive points to get a set of chunks.&#x20;
4. Take the combinatorial hash of each chunk.
5. Store these hashes in the DB.

Resources:\
\- https://medium.com/@treycoopermusic/how-shazam-works-d97135fb4582\
\- http://coding-geek.com/how-shazam-works/\
\- https://www.ee.columbia.edu/\~dpwe/papers/Wang03-shazam.pdf

![](<.gitbook/assets/image (12).png>)

Storage Estimation: Total storage => Number of point\_pairs \* STORAGE\_COST\_PER\_PAIR

STORAGE\_COST\_PER\_PAIR = cost of storing a pair((f1, t1), (f2, t2))​ = (f1 : f2 : delta\_time) = 3 integers = 3 \* 4 bytes = 12 bytes.

Total storage => Number of point\_pairs per song _total number of songs_ 12Bytes Assume the total number of songs in the database to be 1 million.

Total storage => Number of point\_pairs per song _1M_ 12Bytes

Total storage => NUMBER\_OF\_PAIRS\_PER\_CHUNK _AVERAGE\_NUMBER\_OF\_CHUNKS\_PER\_SONG_ 1M \* 12Bytes

AVERAGE\_NUMBER\_OF\_CHUNKS\_PER\_SONG length of chunk =avg length of song / divided by length of chunk

Assume average length to be around 3 minutes = 200, and each chunk to be around 10 seconds.

![](<.gitbook/assets/image (13).png>)

![](<.gitbook/assets/image (14).png>)

![](<.gitbook/assets/image (15).png>)

**2. Why does this video only talk about the sound search algorithm? Where is an explanation of the architecture?**\
\
_This video is about analysing an algorithm in depth. The considerations of server load, fault tolerance, etc… are discussed in detail in the other videos._

\
**3. Is this algorithm capable of handling millions of requests searching amongst millions of songs?**\
\
_The database has only insert and select operations. We do not require any transactional guarantees during querying._\
_The requests will be processed by the mobile clients and sent as a set of interesting points to the server._\
_This means there is little left to do on the server except matching a point map to the database. Even for millions of requests a day, this system should be scalable._

\
**4. What if two chunks have very similar signatures?**\
\
_As long as the hash function is chosen appropriately, this is highly unlikely._

\
**5. What if the song has no interesting points in it? Similarly, what happens if the clipping has no interesting points?**\
****\
****_The first scenario is highly unlikely, to the point of being assumed impossible. If there are no interesting points in the clip, it usually means one or more of the following:_\
a) The clip is too short\
b) The audio is too faint\
c) The noise is too much.

\
**6. Is it possible that two servers give different results for the same audio clip?**\
\
_If the process is deterministic, then no. If the algorithm changes, a new deployment will be needed. This might lead to multiple versions of code in the production environment._\
_When this happens, the same song clip can give different song match results._

__\
__**7. What happens if the API has to return the most likely matches?**\
__\
___We can modify the response to return the songs having the most hash matches._

__

{% embed url="https://youtu.be/bt5UkeMpXRU" %}

__

__
