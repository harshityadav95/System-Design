# Low Level Design



### **LLD Questions** <a href="#d466" id="d466"></a>

![Image for post](https://miro.medium.com/max/60/0\*kzBjN9quuPgmxw7u?q=20)![Image for post](https://miro.medium.com/max/6000/0\*kzBjN9quuPgmxw7u)

All LLD Questions were supposed to be written in a clean, modular, and extensible manner, with proper test cases, log messages and exception handling. Concurrency handling depends from question to question. There was no restriction on the choice of language in any company.

1. Code a rate-limiting algorithm. The approach I proposed was similar to the leaky bucket algorithm or token bucket algorithm provided by the [Guava Library](https://guava.dev/releases/19.0/api/docs/index.html?com/google/common/util/concurrent/RateLimiter.html#:\~:text=A%20rate%20limiter.,permits%20need%20not%20be%20released.).
2. In Memory [Kafka](https://kafka.apache.org)
3. Build a coffee maker machine, which has multiple ingredients and a set of beverages, which can be prepared by using these ingredients. The preparation of a beverage should be done as a transaction, with rollbacks, if a certain ingredient is not available while preparing a beverage. Ingredients can be added, and beverages can be prepared, etc. in a concurrent fashion.

### Algo Questions <a href="#a4e0" id="a4e0"></a>

![Image for post](https://miro.medium.com/max/60/0\*3qV-WFxLvGbFNsny?q=20)![Image for post](https://miro.medium.com/max/6016/0\*3qV-WFxLvGbFNsny)

I am putting the questions which I can remember.

1. Given a directed graph, a starting point, and an ending point, find critical points from all the random walks. A critical point is one where the same node is visited twice.
2. Given a grid where points are either connected in an either horizontal or vertical manner, tell the total number of squares possible. These squares can be of multiple sizes and can be overlapping.
3. Given a grid where there are walls in certain blocks. There are k people with their positions and a block for the meeting point. What is the overall minimum distance required to travel by all the people to reach the meeting point? What is the optimal meeting point, so that sum of distance traveled is minimum?
4. [Maximum size rectangle binary sub-matrix with all 1s](https://www.geeksforgeeks.org/maximum-size-rectangle-binary-sub-matrix-1s/)
5. [Decode a string recursively encoded as count followed by substring](https://www.geeksforgeeks.org/decode-string-recursively-encoded-count-followed-substring/)
