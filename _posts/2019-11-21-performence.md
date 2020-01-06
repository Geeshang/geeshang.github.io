SPEED PERFORMANCE

Using .append (NPE's answer)
Using .loc (fred's answer)
Using .loc with preallocating (FooBar's answer)
Using dict and create DataFrame in the end (ShikharDua's answer)
Results (in secs):

|------------|-------------|-------------|-------------|
|  Approach  |  1000 rows  |  5000 rows  | 10 000 rows |
|------------|-------------|-------------|-------------|
| .append    |    0.69     |    3.39     |    6.78     |
|------------|-------------|-------------|-------------|
| .loc w/o   |    0.74     |    3.90     |    8.35     |
| prealloc   |             |             |             |
|------------|-------------|-------------|-------------|
| .loc with  |    0.24     |    2.58     |    8.70     |
| prealloc   |             |             |             |
|------------|-------------|-------------|-------------|
|  dict      |    0.012    |   0.046     |   0.084     |
|------------|-------------|-------------|-------------|
