# Union Find

Union find is an algorithm that stores information of disjoint sets, checking if 2 points are connected with each other.

We use an array `parents[]` to interpret the connection: if a is connected to b, parents[a] = parents[b].

For further optimization, we only want to store the root parent of each point, which needs path compression:
```
private int union(int x) {
  int root = parents[x];

  // The root parent's parent should be itself, so we iterate till we find the root parent.
  while (root != parents[root]) {
    root = parents[root];
  }

  // After we find x's root parent, we compress the path, which means we iteratively assign x as well as x's parents' parents to the root.
  while (parents[x] != root) {
    int p = parents[x];
    parents[x] = root;
    x = p;
  }

  return root;
}
```
Path compression is the core of Union Find. Then we need some basic operations of our UnionFind class, like connecting a and b, and checking if a and b are connected, etc..

```
class UnionFind {
  int[] parents;

  public UnionFind(int n) {
      parents = new int[n];
      for (int i = 0; i < n; i++) {
          parents[i] = i;
      }
  }

  public int union(int x) {
      int root = parents[x];
      while (parents[root] != root) {
          root = parents[root];
      }
      while (x != root) {
          int p = parents[x];
          parents[x] = root;
          x = p;
      }
      return root;
  }

  public boolean isConnected(int a, int b) {
      return union(a) == union(b);
  }

  public void connect(int a, int b) {
      parents[union(a)] = parents[union(b)];
      //We should ensure the path is best optimized at any time.
      union(a);
      union(b);
  }
}
```
