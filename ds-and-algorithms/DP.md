Dynamic Programming
==================

### Kadane's Algorithm
* arr: list of int
* Let K be solution of arr[:i]
* K[i] = max(K[i-1] + arr[i], arr[i])
* See also: [here](https://hwan-shell.tistory.com/m/117?category=771708)