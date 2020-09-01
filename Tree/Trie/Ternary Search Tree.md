# Ternary Search Tree

We can integrate internal Binary Search Trees into the main trie structure:

![](/pictures/tst.png)

When search in a TST, for each character:
1. If less, take left link; if greater, take right link.
2. If equal, take the middle link and move to the next key character.

**Search hit**. Final node is blue(isKey == true).

**Search miss**. Reach a null link or final node is white(isKey == false).

For example, there are 6 keys in the picture above. They are corresponding to:
1. AC;
2. ACC;
3. CAC;
4. CAGC;
5. CC;
6. CGC;

Here is the implementation of a TST class:
```java
class Node {
  char val;
  boolean isKey;
  Node left, right, next;

  public Node(char val, boolean isKey) {
    this.val = val;
    this.isKey = isKey;
  }
}

class Trie {
  Node root;
  Set<String> result_set = new HashSet<>();
  List<String> result_list = new ArrayList<>();

  public void insert(String word) {
    root = insert_helper(word, root, 0);
  }

  private Node insert_helper(String word, Node node, int idx) {
    if (idx >= word.length()) return node;
    if (node == null) {
      node = new Node(word.charAt(idx), false);
    }
    if (word.charAt(idx) < node.val) {
      node.left = insert_helper(word, node.left, idx);
    } else if (word.charAt(idx) > node.val) {
      node.right = insert_helper(word, node.right, idx);
    } else {
      node.isKey = idx == word.length() - 1? true : node.isKey;
      node.next = insert_helper(word, node.next, idx+1);
    }
    return node;
  }

  public boolean search(String word) {
    return search_helper(word, root, 0, false);
  }

  public boolean isPrefix(String prefix) {
    return search_helper(prefix, root, 0, true);
  }

  private boolean search_helper(String word, Node node, int idx, boolean isPrefix) {
    if (node == null) return false;
    if (word.charAt(idx) < node.val) return search_helper(word, node.left, idx, isPrefix);
    if (word.charAt(idx) > node.val) return search_helper(word, ndoe.right, idx, isPrefix);
    if (idx == word.length() - 1) return isPrefix? true : node.isKey;
    return search_helper(word, node.next, ++idx, isPrefix);
  }

  public Node findNode(String word) {
    return findNode(word, root, 0);
  }

  private Node findNode(String word, Node node, int idx) {
    if (node == null) return node;
    if (word.charAt(idx) < node.val) return findNode(word, node.left, idx);
    if (word.charAt(idx) > node.val) return findNode(word, node.right, idx);
    if (idx == word.length() - 1) return node.next;
    return findNode(word, node.next, ++idx);
  }

  public List<String> prefixSearch(String prefix) {
    Node node = findNode(prefix);
    result_set = new HashSet<>();
    result_list = new ArrayList<>();
    result_set.add(prefix);
    result_list.add(prefix);
    return prefixSearch(prefix, node);
  }

  private List<String> prefixSearch(String prefix, Node node) {
    if (node != null) {
      StringBuilder sb = new StringBuilder(prefix);

      prefixSearch(prefix, node.left);
      sb.append(node.val);
      if (node.iskey && !result_set.contains(sb.toString())) {
        result_set.add(sb.toString());
        result_list.add(sb.toString());
      }
      prefixSearch(sb.toString(), node.next);
      prefixSearch(prefix, node.right);
    }

    return result_list;
  }
}
```
