# Computer Algorithms: Sorting in Linear Time

The first question when we see the phrase “sorting in linear time” should be: where’s the catch? Indeed there’s a catch. We can’t sort arbitrary data in linear time using only comparisons.

For general comparison sorting, algorithms such as [merge sort](./merge-sort.md), [quicksort](./quicksort.md), and [heapsort](./heapsort.md) run in `O(n log n)` time in their usual or guaranteed cases. To do better than that, the algorithm must use extra information about the input.

## Counting Sort

[Counting sort](./counting-sort.md) works when the input consists of integer keys in a known, reasonably dense range. It counts the occurrence of each key and then reconstructs the sorted output.

Its runtime is `O(n + k)`, where `n` is the number of input items and `k` is the key range. This can be excellent when `k` is close to `n`, but very wasteful when the values are sparse, such as `[2, 1, 10000000, 2]`.

## Radix Sort

[Radix sort](./radix-sort.md) sorts by digit or character position. It uses a stable sort at each position so that the ordering created by previous passes is preserved.

Its runtime is `O(d(n + b))`, where `d` is the number of digit positions and `b` is the radix or base. For a fixed base and bounded digit count, radix sort is linear in the number of input items.

## Bucket Sort

[Bucket sort](./bucket-sort.md) distributes values into buckets and sorts values inside each bucket. It can run in expected `O(n + k)` time when the input is well distributed and the number of buckets is chosen well.

Bucket sort is not automatically linear for every input. Its worst-case performance is `O(n^2)` when too many values land in the same bucket.

## Application

Linear-time sorting algorithms are powerful only when their input assumptions match the data. Counting sort depends on a suitable integer key range, radix sort depends on a bounded number of digit positions and stable passes, and bucket sort depends on distribution assumptions.

Any time we need to implement a sorting algorithm, we must carefully investigate the input data first.
