# Tree: Must do coding questions

#### Level order traversal

  1. Calculate tree height
 
  2. Print level
 
```java
  private static int getHeight(Node root){
        if(root==null) return 0;
        return 1+Math.max(getHeight(root.left), getHeight(root.right));
    }

    //Iterative
    private static void leftView(Node root) {
        int h = getHeight(root);
        for (int i = 0; i <=h; i++) {
            printLevel(root,i);
            System.out.println();
        }
    }

    private static void printLevel(Node node, int i) {
        if(i==0)
            return;
        if(i==1)
            System.out.print(node.data+" ");
        if(i>1){
            printLevel(node.left,i-1);
            printLevel(node.right,i-1);
        }
    }
```

#### 1) Print Left View of Binary Tree

Left view of the tree can be printed using level order traversal.

<p>
https://practice.geeksforgeeks.org/problems/left-view-of-binary-tree/1
  
</details>

<summary>code (using level order traversal (BFS) store first element at each level)</summary>    
      
```java
    class Tree
{
    //Function to return list containing elements of left view of binary tree.
    ArrayList<Integer> leftView(Node root)
    {
        ArrayList<Integer> res = new  ArrayList<>();
        leftView(root,res);
        return res;
    }
    
    //Iterative
    private static void leftView(Node root,ArrayList<Integer> res) {
        int h = getHeight(root);
        int[] value = new int[1];
        
        for (int i = 0; i <=h; i++) {
            value[0]=0;
            printLevel(root,i,value);
            if(value[0]!=0)
                res.add(value[0]);
        }
    }

    private static void printLevel(Node node, int i,int[] value) {
        if(i==0)
            return;
            
        if(i==1){
            if(value[0] == 0)
                value[0] = node.data;
        }
           
        if(i>1){
            if(node.left!=null)
                printLevel(node.left,i-1,value);
            if(node.right!=null)
                printLevel(node.right,i-1,value);
        }
    }
    
    private static int getHeight(Node root){
        if(root==null) return 0;
            return 1+Math.max(getHeight(root.left), getHeight(root.right));
    }

}
```
    
</details>

Time complecity is same in both O(n) but space complexity is O(h) for aux stack, in case of DFS, whereas in BFS queus is storing each level so ~O(N/2) for queue size.
</details>

<summary>code (DFS - R'LR - use a data structure to store first value at each level)</summary>    
      
```java
 
     //Function to return list containing elements of left view of binary tree.
    ArrayList<Integer> leftView(Node root)
    {
        ArrayList<Integer> res = new  ArrayList<>();
        leftViewUtil(root,res,0);
        return res;
    }
    
    //R' L R
    
    public void leftViewUtil(Node root, ArrayList<Integer> res, int level){
        
        if(root==null)
            return;
        
        if(level==res.size())
            res.add(root.data);
        
        leftViewUtil(root.left,res,level+1);
        leftViewUtil(root.right,res,level+1);
        
    }
 
```
    
</details>

2. Check for BST

https://practice.geeksforgeeks.org/problems/check-for-bst/1

BST property is left should be less than root and right should be greater then root.

It is is BST in-order travers should give us sorted array, so we just need to check each number is in accending order! 

  if(root.data<min || root.data>max)
            return false;
            


```java
public class Solution
{
    //Function to check whether a Binary Tree is BST or not.
    boolean isBST(Node root)
    {
        int min = Integer.MIN_VALUE;
        int max = Integer.MAX_VALUE;
        return checkBST(root,min,max);
    }
    
    boolean checkBST(Node root, int min, int max){
        if(root==null)
            return true;
        
        if(root.data<min || root.data>max)
            return false;
          
       boolean leftTrue = checkBST(root.left, min, root.data-1 );
       boolean rightTrue = checkBST(root.right, root.data+1, max);
        
       if(leftTrue && rightTrue)
            return true;
        else
            return false;
    }
}
```

3. Print Bottom View of Binary Tree

https://practice.geeksforgeeks.org/problems/bottom-view-of-binary-tree/1

DO BFS traversal with horizontal distance, horizondal distance  -

if horizontal distance property is not present in Node, then create node wrapper

```java
class NodeW{
  Node node;
  int hd
}

||

class Node{
  int data;
  int hd
}

```


```java
class Solution {
    public ArrayList<Integer> bottomView(Node root) {
        ArrayList<Integer> res = new ArrayList<>(0);
        if (root == null)
            return res;
        int hd = 0;
        Queue<Node> queue = new LinkedList<>();
        root.hd = hd;
        queue.add(root);
        //store the hd and values pair (last value is bottom view first value is top view)
        TreeMap<Integer, Integer> map = new TreeMap<>();
        while (!queue.isEmpty()) {
            Node temp = queue.poll();
            hd = temp.hd;
            map.put(hd, temp.data);
            if (temp.left != null) {
                temp.left.hd = hd - 1;
                queue.add(temp.left);
            }
            if (temp.right != null) {
                temp.right.hd = hd + 1;
                queue.add(temp.right);
            }
        }
        map.entrySet().stream().forEach(e -> res.add(e.getValue()));
        return res;
    }
}
```

4. Print a Binary Tree in Vertical Order

This question is similar to above, here we need to collect all the values instead of first value.

```java
class Solution
{
    //Function to find the vertical order traversal of Binary Tree.
    static ArrayList<Integer> verticalOrder(Node root) {
        ArrayList<Integer> res = new ArrayList<>();
        if (root == null)
            return res;
        TreeMap<Integer, ArrayList<Integer>> map = new TreeMap<>();
        Queue<NodeW> queue = new LinkedList<>();
        NodeW rootW = new NodeW(root, 0); // root hd=0;
        queue.add(rootW);
        while (!queue.isEmpty()) {
            NodeW temp = queue.poll();
            int hd = temp.hd;
            ArrayList<Integer> tempQueue = map.get(hd);
            if (tempQueue != null) {
                tempQueue.add(temp.node.data);
                map.put(hd, tempQueue);
            } else {
                tempQueue = new ArrayList<Integer>();
                tempQueue.add(temp.node.data);
                map.put(hd, tempQueue);
            }
            if (temp.node.left != null) {
                queue.add(new NodeW(temp.node.left, hd - 1));
            }
            if (temp.node.right != null) {
                queue.add(new NodeW(temp.node.right, hd + 1));
            }
        }// while 

        map.entrySet().stream().forEach(list -> {
            res.addAll(list.getValue());
        });
        return res;
    }
}

class NodeW {

    Node node;
    int hd;

    NodeW(Node node, int hd) {
        this.node = node;
        this.hd = hd;
    }
}
```

5. Level order traversal in spiral form

#### 6. Lowest Common Ancestor in a BST

  https://practice.geeksforgeeks.org/problems/lowest-common-ancestor-in-a-binary-tree/1

  If we get null from left tree then all three nodes are present in right tree, and vise versa.
  
  but if left is true and right is true then the node it self is LCA.
  
  So the idea is we will search for left node in left, if it is not present then both would be right tree and vise versa.
  
  ```java
    class Solution
    {
        //Function to return the lowest common ancestor in a Binary Tree.
      Node lca(Node root, int n1,int n2)
      {
             if(root==null)
              return null;

             if(root.data==n1 || root.data==n2)
                  return root;

             Node leftLca = lca(root.left, n1, n2);     
             Node rightLca = lca(root.right, n1, n2);

             if(leftLca != null && rightLca != null){
                 return root;
             }else if (leftLca != null){
                 return leftLca;
             }else {
                 return rightLca;
             }
      }
    }
  ```
  
#### 7. Maximum Path Sum (VII) 

```java
    //Take max of below 3 values for each node 
   
         //max path sum for parent call of root. 
         //This path must include at-most one child of root.    
        int max_single = Math.max(node.data, node.data + Math.max(l ,r));
        
        //path via self | when node it self is result
        //max_top represents the sum when the node under consideration 
        int max_top = Math.max(max_single, l + r + node.data);
        
        //storing the maximum result.
        res.val = Math.max(res.val, max_top);
```
https://practice.geeksforgeeks.org/problems/maximum-path-sum-from-any-node/1      
      
```java
class Res{
    public int val;
}

class Solution
{
    //Function to return maximum path sum from any node in a tree.
    //Function to return maximum path sum from any node in a tree.
    
     int findMaxSum(Node node){
          Res res = new Res();
          res.val = Integer.MIN_VALUE;
          findMaxUtil(node, res);
          return res.val;
     }
    
    int findMaxUtil(Node node, Res res){

        int l = 0, r = 0;
        
        if(node==null)
            return 0;
            
        if(node.left !=null) 
            l = findMaxUtil(node.left,res);
            
        if(node.right !=null) 
            r = findMaxUtil(node.right,res);
           
        int max_single = Math.max(node.data, node.data + Math.max(l ,r));
        
        int max_top = Math.max(max_single, l + r + node.data);
        
        res.val = Math.max(res.val, max_top);
        
        return max_single;
        
    }
}
```


#### 8. Diameter of a Binary Tree

    Take max of below 2 values for each node 
 
      - Math.max(leftHeight, rightHeight) + 1
      - leftHeight+rightHeight + 1
      
      
  9. Connect Nodes at Same Level

10. Convert a given Binary Tree to Doubly Linked List

11. Write Code to Determine if Two Trees are Identical or Not

12. Given a binary tree, check whether it is a mirror of itself

13. Number of leaf nodes

14. Check if given Binary Tree is Height Balanced or Not

15. Serialize and Deserialize a Binary Tree

#### 16. Height of Binary Tree

```java
   private static int getHeight(Node root){
        if(root==null) return 0;
            return 1+Math.max(getHeight(root.left), getHeight(root.right));
    }
```
