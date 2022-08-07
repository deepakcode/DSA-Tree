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

3. Print Bottom View of Binary Tree

4. Print a Binary Tree in Vertical Order

5. Level order traversal in spiral form

6. Connect Nodes at Same Level

7. Lowest Common Ancestor in a BST

8. Convert a given Binary Tree to Doubly Linked List

9. Write Code to Determine if Two Trees are Identical or Not

10. Given a binary tree, check whether it is a mirror of itself

11. Height of Binary Tree

12. Maximum Path Sum

13. Diameter of a Binary Tree

14. Number of leaf nodes

15. Check if given Binary Tree is Height Balanced or Not

16. Serialize and Deserialize a Binary Tree

</p>
