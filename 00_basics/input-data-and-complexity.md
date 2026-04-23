# Friday Algorithms: Input Data and Complexity

## Size of ..

As we all know most of the cases a program execution time depends most from the input data. As I wrote in my [post](/2010/08/29/beginning-algorithm-complexity-and-estimation/) the degree of the input data estimates the algorithm complexity.

Of course 3*n² is a bit faster than 5*n², but in general both functions are similar and have the same complexity. In this case we tend to say that the size of the input data is n.

[![Size of the input data](/wp-content/uploads/2010/09/input-data.jpg)](/wp-content/uploads/2010/09/input-data.jpg)The size of the input data affects the complexity of the algorithm

It is easy to bind this to arrays or other simple data structures. For example when sorting an array with n elements, the size of the input data is n, but sometimes there is not such an obvious relation between an algorithm and the size of the input data.

For instance when we’ve to search a path into a graph, than maybe the best way to describe the input data is by summing both the number of vertices and the number of edges.

## Conclusion

[![Think, think, think](/wp-content/uploads/2010/09/think.jpg)](/wp-content/uploads/2010/09/think.jpg)Think, think, think

However this is important when dealing with algorithms, because a mistake in the estimation of the input data size will result in wrong algorithm estimation at all