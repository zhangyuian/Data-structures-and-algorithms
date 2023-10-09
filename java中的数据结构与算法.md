# Data-structures-and-algorithms

使用STAR法则进行描述，Situation、Task、Action、Result

## day14 布隆过滤器

布隆过滤器是一个基于数组和哈希散列元素的结构，很像HashMap的哈希桶。

其作用是用于检测一个元素是否在集合中

优点是空间效率和查询时间比一般的算法好很多，但也有一定概率的误判性（哈希碰撞）。

1.布隆过滤器的使用场景？

*   用于快速检测元素是否在集合内。
*   缓存系统：快速判断一个数据是否在缓存中，避免不必要的数据库查询或计算。
*   网络爬虫：布隆过滤器可以用于网络爬虫中，用于快速判断一个URL是否已经被爬取过，避免重复爬取相同的页面。
*   垃圾邮件过滤：布隆过滤器可以用于垃圾邮件过滤中，用于快速判断一个邮件是否是垃圾邮件，避免用户收到大量的垃圾邮件。
*   分布式系统：布隆过滤器可以用于分布式系统中，用于快速判断一个元素是否存在于分布式系统的某个节点中，避免不必要的网络通信。

2.布隆过滤器的实现原理和方式？

*   布隆过滤器通过位数组BitSet存储二进制元素，并有多个哈希计算函数。通过哈希计算函数计算哈希值查找其在BitSet中的位置，多个哈希计算函数的结果来判断该元素是否在集合内。

3.如何提高布隆过滤器的准确性？

*   增加哈希计算函数的分散性
*   增大BitSet的值
*   多个哈希计算函数进行多重认证

4.有哪些中哈希计算方式？

*   增加散列的方法：字符哈希值叠加、移位、绝对值、与运算等
*   算法：MD5、SHA-1、SHA-256、SHA-3，后两者的安全性较高。

5.都有哪些类型的布隆过滤器实现？*Google 开源的 Guava 中自带的布隆过滤器、Redis 中的布隆过滤器([https://github.com/RedisBloom/RedisBloom (opens new window)](https://github.com/RedisBloom/RedisBloom))*

目前常见的布隆过滤器实现有以下几种类型：

1.  标准布隆过滤器（Standard Bloom Filter）：最基本的布隆过滤器实现，使用一个位数组和多个哈希函数。
2.  可变布隆过滤器（Mutable Bloom Filter）：在标准布隆过滤器的基础上，添加了删除操作。通过引入计数器，可以在删除元素时将对应位置的计数器减少，避免误判。
3.  计数布隆过滤器（Counting Bloom Filter）：在可变布隆过滤器的基础上，将位数组中的每个位置改为计数器。可以实现更精确的元素计数，但空间占用也会增加。
4.  压缩布隆过滤器（Compressed Bloom Filter）：通过使用压缩算法，减少位数组的空间占用。常见的压缩算法有差分编码（Delta Encoding）、前缀编码（Prefix Encoding）等。
5.  可扩展布隆过滤器（Scalable Bloom Filter）：解决了标准布隆过滤器固定大小的问题。当位数组满时，会创建一个新的布隆过滤器，并将元素添加到新的布隆过滤器中。
6.  布谷鸟过滤器（Cuckoo Filter）：使用哈希表代替位数组，可以实现更高的查找效率。同时，布谷鸟过滤器也支持删除操作。

## day13 图

![image.png](https://note.youdao.com/yws/res/29116/WEBRESOURCE4ee6a64fb831374bf8be58fddf3babb3)

```java
/* ----图的数据结构----*/
// 图的顶点数
protected int v;
// 图的边个数
protected int e;

// 图的矩阵【数组】
protected int[][] table;
// 图的矩阵【链表】
protected LinkedList<Integer[]>[] table;
// 图的矩阵【红黑树】
private TreeSet<Integer>[] table;
```

邻接矩阵：表示节点之间两两的关系

![image.png](https://note.youdao.com/yws/res/29122/WEBRESOURCEe44d1deee1a46d9a0e8f2b9e3605e7ea)

邻接表：链表里面是单个节点所有的相邻节点

![image.png](https://note.youdao.com/yws/res/29124/WEBRESOURCE6451038e813411b262d8d44cc5693831)

遍历：广度优先 & 深度优先

![image.png](https://note.youdao.com/yws/res/29130/WEBRESOURCE77ab9091dc30c82c69f4d75ea4fd313b)

1.图的使用场景是什么？

*   图是由顶点和连接顶点之间的边组成的集合。一个节点可以与多个节点之间相互连接，可以表示多对多的关系，地铁线路网、微信好友关系链、计算机中的状态执行等都可以抽象为图的结构。

2.图有的分类？

*   无向&无权图(U/U)、有向&无权图(D/U)、无向&有权图(U/W)、有向&有权图(D/W)。

3.图怎么存放权重值？

*   可以通过邻接矩阵：table\[x]\[y] = weight
*   也可以通过邻接表：table\[x] = new Integer\[] {y, weight}

4.图的广度遍历

*   先遍历当前节点相邻的节点，再遍历相邻节点的相邻节点。
*   在算法上，将遍历的节点放入队列中，从队列中取出节点，记录，取出当前节点的所有邻边并放入队列中，并设定已访问的标志位。继续从队列中取出节点进行记录，重复上述过程。

5.图的深度遍历

*   优先遍历离当前节点最远的节点。按照先序和后序遍历有不一样的顺序
*   算法上，通过递归实现。对当前节点进行深度遍历，然后递归调用当前节点的所有相邻节点。先序和后序则是通过节点在递归算法中记录的位置来决定。

## day12 并查集

![image](https://bugstack.cn/images/article/algorithm/disjoint-set-06.png?raw=true)

并查集中的元素可以方便找到自己的父节点，也能够快速合并。

合并的方式：union（parent， child）

*   默认合并：将child所在集合的所有节点都指向parent节点的根节点，如上图
*   粗暴合并：将child节点的根节点指向parent节点的根节点
*   数量合并：比较child和parent两个集合的数量count，将数量小的节点指向数量大的节点
*   排序合并：比较child和parent两个集合的树高rank，将矮的节点指向高的节点
*   压缩路径：对树高进行优化，将树高降低到2，如下图

![image](https://bugstack.cn/images/article/algorithm/disjoint-set-10.png?raw=true)

1.并查集叙述？

*   并查集是基于数组实现的一种跟踪元素的数据结构，这些元素被划分为多个不相交的子集

2.并查集的使用场景？

*   其提供了近乎恒定的时间操作来添加新集合、合并现有集合以及确定元素是否在同一个集合中。可以用于推荐算法、好友关系链、族谱等

3.并查集怎么合并元素？

先找到当前两个元素的根节点，之后合并的方法如下

*   默认合并：将child所在集合的所有节点都指向parent节点的根节点
*   粗暴合并：将child节点的根节点指向parent节点的根节点
*   数量合并：比较child和parent两个集合的数量count，将数量小的节点指向数量大的节点
*   排序合并：比较child和parent两个集合的树高rank，将矮的节点指向高的节点
*   压缩路径：在排序合并的基础上，对树高进行优化，在寻找两个元素根节点的过程中降低树的高度

4.并查集合并元素的优化策略？

*   默认合并，需要遍历数组，速度较慢
*   粗暴合并，只将元素的根节点进行链接合并，子节点不变，解决了数组遍历的问题
*   数量合并，将数量少的集合根节点链接在多的集合上，减少因为数量多（树变高）带来的时间开销
*   排序合并，数量多的集合其树高并不一定高，通过树高来决定链接关系，将树矮的链接到树高的集合上
*   压缩路径，树高的会导致查询根节点的数量变多，因此削减树的高度来减少查询时间

5.如何压缩路径？

*   在寻找根节点的过程中，将当前元素的父节点变更为父节点的父节点来压缩路径

## day11 红黑树

1.红黑树都有哪些使用场景？

*   红黑树不是严格平衡的二叉树，其只对黑色节点的树高进行平衡，因此相对于AVL树其插入和删除的效率较高，查找的效率较低。这也让其成为Java中HashMap的元素碰撞后的转换、Linux的CFS进行调度算法、多路复用技术的Epoll等各类底层的数据结构实现

2.相比于BST树，红黑树有什么用途？

*   BST树在最坏的情况下会变成一条链表，查找和插入的时间复杂度为O(n)，而红黑树在各种情况下的时间复杂度都比较好，为O（logn)。因此其除了可以用于搜索外，还能用于插入和删除操作较多的情况。

3.B-树是什么意思，都包括哪些？

*   B-树是自平衡树的统称（balanced tree）。有B-树有B+树、B\*树、2-3树、红黑树、AVL树等

4.新增加一个节点后，什么情况下需要染色、什么情况要左旋、什么情况要左旋+右旋？

*   染色情况：在叔叔节点是红时需要进行染色，当叔叔节点是黑色的时候，则需要进行旋转+染色调整
*   左旋情况：当新插入节点是父亲节点的右节点，并且其父亲节点是爷爷节点的右节点时，需要进行左旋
*   左旋+右旋情况：当新插入节点是父亲节点的右节点，并且其父亲节点是爷爷节点的左节点时，需要进行左旋+右旋的操作

5.红黑树的特点是什么？

*   根节点是黑色的
*   没有连续的红色节点
*   当前节点到任意NIL节点（叶子节点）的经过的黑色节点数量相同
*   所有NIL节点（叶子节点）都是黑色的

## day10 2-3树

1.2-3树的数据结构描述

*   2-3树的每个节点有自身元素 items 数组，孩子节点 children 数组，自身元素长度 number，父亲节点 parent，节点有用于插入元素的方法 insert
*   2-3树自身包含 root 节点，插入元素的操作 insert，拆分节点的方法 split，替换孩子节点的方法 replaceChild
*   2-3树的每个节点只能含有3个元素，并且元素按照大小从左往右依次排列。
*   当元素的大小等于3个时，会进行拆分，处于中间的元素会插入到父节点中，剩下的两个元素分裂成两个孩子节点
*   新元素只能插入到叶子节点中（但是可能经过拆分进入到父节点中）

2.2-3树一个节点最多可以存放几个元素

*   3个，当等于3个时，会进行拆分。

3.2-3树插入节点时间复杂度

*   log(n)，n为节点数量

4.2-3树一个节点有3个元素，如何迁移。*需要旋转吗*

*   位于中间大小的节点进入父节点中
*   剩下的两个节点变成父节点的两个子节点
*   不需要旋转

5.2-3树，你能手写一下吗？

```java
package tree.mycode;

public class Tree_2_3 {

    private Node_2_3 root;

    public void insert(int e) {
        if (this.root == null) {
            this.root = new Node_2_3(e);
        } else {
            root = insert(e, root);
            if (root.number == 3) {
                root = this.split(root, null);
            }
        }
    }

    public Node_2_3 insert(int e, Node_2_3 parent) {
        if (parent.isLeaf()) {
            parent.insert(e);
            return parent;
        }

        Node_2_3 child = null;
        if (parent.number == 1) {
            if (e < parent.getMinItem()) {
                child = insert(e, parent.getLeft());
            } else {
                child = insert(e, parent.getMiddle());
            }
        } else {
            if (e < parent.getMinItem()) {
                child = insert(e, parent.getLeft());
            } else if (e > parent.getMiddleItem()) {
                child = insert(e, parent.getRight());
            } else {
                child = insert(e, parent.getMiddle());
            }
        }

        if (child.number == 3) {
            return this.split(child, parent);
        }
        return parent;
    }

    public Node_2_3 split(Node_2_3 node, Node_2_3 parent) {
        if (parent == null) {
            parent = new Node_2_3(node);
        }
        parent.insert(node.getMiddleItem());
        Node_2_3[] newNode = this.triangle(node);
        this.replaceChild(parent, node, newNode[0], newNode[1]);
        return parent;
    }

    public Node_2_3[] triangle(Node_2_3 node) {
        Node_2_3[] newNode = new Node_2_3[2];

        newNode[0] = new Node_2_3(node.items[0]);
        newNode[1] = new Node_2_3(node.items[1]);

        if (!node.isLeaf()) {
            newNode[0].children[0] = node.children[0];
            newNode[0].children[1] = node.children[1];

            newNode[1].children[0] = node.children[2];
            newNode[1].children[1] = node.children[3];
        }

        return newNode;
    }

    public void replaceChild(Node_2_3 parent, Node_2_3 oldChild, Node_2_3 newNode1, Node_2_3 newNode2) {
        if (oldChild == parent.children[0]) {
            parent.children[3] = parent.children[2];
            parent.children[2] = parent.children[1];
            parent.children[1] = newNode2;
            parent.children[0] = newNode1;
        } else if (oldChild == parent.children[1]) {
            parent.children[3] = parent.children[2];
            parent.children[2] = newNode2;
            parent.children[1] = newNode1;
        } else {
            parent.children[3] = newNode2;
            parent.children[2] = newNode1;
        }
    }


    static class Node_2_3 {

        public int[] items;

        public Node_2_3[] children;

        public Node_2_3 parent;

        public int number;

        public Node_2_3() {
            this.items = new int[3];
            this.children = new Node_2_3[4];
            this.parent = null;
            this.number = 0;
        }

        public Node_2_3(int item) {
            this();
            this.items[0] = item;
        }

        public Node_2_3(Node_2_3 child) {
            this();
            this.children[0] = child;
        }

        public boolean isLeaf() {
            return this.children[0] == null;
        }

        public void insert(int e) {
            int idx = this.number - 1;
            while (idx >= 0) {
                if (e > this.items[idx]) {
                    break;
                }
                items[idx + 1] = items[idx];
            }
            items[idx + 1] = e;
            this.number++;
        }

        public int getMinItem() {
            return this.items[0];
        }

        public int getMiddleItem() {
            return this.items[1];
        }

        public Node_2_3 getLeft() {
            return this.children[0];
        }

        public Node_2_3 getMiddle() {
            return this.children[1];
        }

        public Node_2_3 getRight() {
            return this.children[2];
        }
    }
}

```

## day09 平衡二叉树（AVL树）

### 命名

*   AVL 树以其两位苏联发明家Georgy Adelson-Velsky和 Evgenii Landis的名字命名

### 插入 insert 操作

*   插入之后，对插入的节点自下往上进行平衡操作，平衡操作有四种：左旋、右旋、左旋+右旋、右旋+左旋

### 常见面试题

1.AVL 树平衡因子怎么计算？

*   factor = 左子树的高度 - 右子树的高度

2.AVL 树左旋操作的目的是什么？

*   二叉树处于不平衡的状态，左边子树的高度比右边低，因此需要左旋来增加左子树高度，降低右子树高度，使二叉树达到平衡

3.AVL 树左旋操作的流程是什么？

*   将右节点的父节点设置为当前节点的父亲节点，即 temp.parent = node.parent， 建立单边联系
*   将当前节点的右节点设置为右节点的左子节点，即 node.right = temp.left，并更新父节点 node.right.parent = node
*   将右节点的左节点设置为当前节点，即 temp.left = node, 并更新父节点 node.parent = temp&#x20;
*   设置右节点的父节点的子节点，判断是左子树还是右子树，再赋值，即 temp.parent.left = temp  或者 temp.parent.right = temp 建立双边联系

![image](https://bugstack.cn/images/article/algorithm/tree-avl-06.png?raw=true)

4.AVL 树什么情况下要左旋+右旋？

*   当出现当前节点的左子树高度比右子树高度高2，但是左节点的右子树比左子树的高度高时，需要先对左节点进行一次左旋，再对节点进行一次右旋

5.AVL 树的插入和读取的时间复杂度？

*   插入和读取的时间复杂度都为 O(logn)

## day08 二叉搜索树

1.二叉搜索树结构简述&变T的可能也让手写

*   二叉搜索树是一棵二叉树的左节点小于根节点，右节点大于根节点，并且右节点的所有子节点的值都大于左节点。
*   节点包含值，父节点，左子节点，右子结点
*   基本的操作有插入操作、搜索操作、删除操作

2.二叉搜索树的插入、删除、索引的时间复杂度

*   当二叉树是平衡的时候：插入、删除、索引的时间复杂度为O(log n)
*   当最坏的情况出现时，即：插入、删除、索引的时间复杂度为O(n)&#x20;
*   其中n是树中节点的数量

3.二叉搜索树删除含有双子节点的元素过程叙述

*   先对需要删除的元素进行搜索，搜索到待删除节点
*   判断待删除节点的左孩子、右孩子是否为空，双子节点不为空
*   双子节点不为空时，先找到大于当前节点的最小节点，也就是该节点右子结点的最左边的节点
*   找到目标节点之后，将目标节点的右子结点顶替其位置
*   将目标节点顶替待删除节点的位置（另外这一过程还需要更改目标节点的左右子节点）

4.二叉搜索树的节点都包括了哪些信息

*   节点包含值，父节点，左子节点，右子结点

5.为什么Java HashMap 中说过红黑树而不使用二叉搜索树

*   因为二叉搜索树最差的可能是变成一条链表，这样子搜索的效率会很低，因此需要使用平衡二叉树

## day07 字典树

1.简述字典树的数据结构

*   字典树的每个节点存储了一个类型为自身节点的数组，数组的长度为26，对应26个英文字符，通过下标与26个英文字母的ASCII码对应。
*   节点除了存储类型为自身的数组，还会当前节点代表的字符c，当前节点是否能构成一个单词的标志位isWord，当前节点构成的单词word，前缀长度prefix，单词解释explain
*   在搜索单词的时候，会通过单词下标的找到对应的节点，进而查找到单词。

2.叙述你怎么来实现一个字典树

*   字典树的节点TrieNode：包含一个26长度，类型为自身节点的数组，当前节点代表的字符c，当前节点是否能构成一个单词isWord，当前节点构成的单词word，前缀长度prefix，单词解释explain
*   字典树本身Trie：

    *   包含一个根节点
    *   insert方法：通过单词每个字符的下标存入字典树中
    *   search方法：根据前缀搜索匹配的单词。先精准匹配前缀，再进行模糊匹配，从前缀所在的节点中遍历后面的节点，如果后面的节点存在isWord为true的节点，则将当前节点组成的词记录下来

3.字典树的实际业务场景举例【排序、全文搜索、网络搜索引擎、生物信息】

*   排序：可对字符串按照字典顺序进行排序。遍历一次所有关键字，将它们全部插入trie树，树的每个结点的所有儿子很显然地按照字母表排序，然后先序遍历输出Trie树中所有关键字即可
*   全文搜索：将字符串从根节点开始一个一个进行比较
*   网络搜索引擎：搜索关键字提示功能
*   生物信息：dna序列模糊匹配
*   词频统计：可以在节点中加入count来计数

4.字典树的存入和检索的时间复杂度

*   存入和检索的事件复杂度都为O(k), k为字符串的长度

5.还有哪些字典树的实现方式

*   后缀树：一个字符串构建一棵树，树包含了所有可能的后缀，可用于字符串匹配
*   哈希树：将哈希值装换为key值

![image.png](https://note.youdao.com/yws/res/28712/WEBRESOURCE72f84ffd818f3fa79f4d3c54494e3a6e)

# day06 堆

1.堆的数据结构是什么样？

*   堆的结构是树形结构，分为最大堆和最小堆。最大堆每个节点的元素都比子节点要大，根节点的元最大。最小堆的每个节点都比子节点要小，根节点的元素最小。

2.堆的数据结构使用场景？

*   常用于优先队列、排列算法等场景，因为其具有高效的插入、删除和查找操作。

3.堆的数据结构实现方式有哪些？

*   通常使用数组来实现。也有通过链表实现的。

4.最小堆和最大堆的区别是什么？

*   最大堆每个节点的元素都比子节点要大，根节点的元素最大。
*   最小堆的每个节点都比子节点要小，根节点的元素最小。

5.有了解斐波那契堆吗？

*   斐波那契堆是一种多叉树的结构，由多个最小堆组成。每个最小堆称为一个根链表，根链表中的节点按照从左到右的顺序连接起来。除了根链表外，斐波那契堆还包含一个指向最小节点的指针。
*   每个节点包含一下几个属性：1. 键值 2. 子节点指针 3. 父节点指针 4. 兄弟节点指针 5. 标记（是否失去子节点）
*   斐波那契堆的根链表中的节点按照键值的大小进行排列，且每个节点的键值都小于其子节点的键值。最小节点是根链表中键值最小的节点，通过指针可以直接访问到。
*   斐波那契堆拥有理论上的最佳性能，平摊时间复杂度为O(1)的插入、删除的操作

# day05-2 哈希表拓展

1.简单的HashMap

2.简单的HashMap存在的问题：

*   这里所有的元素存放都需要获取一个索引位置，而如果元素的位置不够散列碰撞严重，那么就失去了散列表存放的意义，没有达到预期的性能。
*   在获取索引ID的计算公式中，需要数组长度是2的幂次方，那么怎么进行初始化这个数组大小。
*   数组越小碰撞的越大，数组越大碰撞的越小，时间与空间如何取舍。
*   目前存放7个元素，已经有两个位置都存放了2个字符串，那么链表越来越长怎么优化。
*   随着元素的不断添加，数组长度不足扩容时，怎么把原有的元素，拆分到新的位置上去。

3.扰动函数

*   用于优化散列效果，使用扰动函数就是为了增加随机性，让数据元素更加均衡的散列，减少碰撞

4.初始化容量和负载因子

*   初始化容量必须为2的n次幂，因此在初始化容量的时候需要找到比初始值大，最小的2进制数值，这个值也称为阈值 threshold
*   负载因子：负载因子决定了数据量多少了以后进行扩容

5.扩容元素拆分

*   数组长度不够，就会进行扩容。扩容的最直接问题，就是把元素拆分到新的数组中。
*   拆分元素的过程中，原jdk1.7中会需要重新计算哈希值，但是到jdk1.8中已经进行优化，不再需要重新计算，提升了拆分的性能

# day05 哈希表

1.哈希碰撞:产生索引冲突

索引冲突的解决办法

2.拉链寻址：在冲突处通过链表相连

3.开放寻址：寻找新位置解决冲突

4.合并散列：寻找新位置解决冲突，但是将碰撞元素链接起来，避免因为需要寻找碰撞元素所发生的循环遍历

5.杜鹃散列：使用2组key哈希表，将冲突元素推到另一个key哈希表中对应的位置

6.跳房子散列：跳房子散列是一种基于开放寻址的算法，它结合了杜鹃散列、线性探测和链接的元素，通过桶邻域的概念——任何给定占用桶周围的后续桶，也称为“虚拟”桶

7.罗宾汉哈希：罗宾汉哈希是一种基于开放寻址的冲突解决算法；冲突是通过偏向从其“原始位置”（即项目被散列到的存储桶）最远或最长探测序列长度（PSL）的元素的位移来解决的

# day04 堆栈

有点好奇为什么要通过这种奇怪的扩容方式来扩容，不能直接创建一个两倍大的数组然后直接复制过去呢？看起来通过先复制前一半再复制后一半，和直接全部复制到新数组的时间复杂度以及空间复杂度都是一样的

Q1：堆栈的使用场景？

满足后入先出的场景，如程序的递归

Q2：为什么不是用 Stack 类？

开发时间久远，开发得比较粗糙，效率低下

Q3：ArrayDeque 是基于什么实现的？

数组

Q4：ArrayDeque 数据结构使用过程叙述。

通过维护一个 element 数组，并通过两个指针 head 与 tail 完成操作。

\- 入栈的时候，通过 (head - 1) & (element.length - 1) 可以计算入栈元素的索引值，并把元素添加到该位置。其实 head 就是不停自减，直到 head 等于 tail 之后扩容，然后 head 移动到数组的尾部，再继续自减。

\- 出栈的时候，直接通过 head 得到出栈元素。再通过 (h + 1) & (elements.length - 1) 得到下一个出栈元素的位置。本质也是 head 不停自增，直到 head 大于数组长度后变成 0，再继续自增。

Q5：ArrayDeque 为什么要初始化2的n次幂个长度？

为了实现二进制的运算，提高计算效率。

*   Integer.toBinaryString( ) 可以将数字转换成二进制，方便查看

# day03 队列

*   Task：学习队列数据结构，Java源码实现

# day02 数组

## 课后问题：

1.数据结构中有哪些是线性表数据结构？

*   顺序表：数组、队列、栈
*   链式表：链表

2.数组的元素删除和获取，时间复杂度是多少？

*   O(n), O(1)

3.ArrayList 中默认的初始化长度是多少？

*   由 DEFAULT\_CAPACITY 定义，长度为10

4.ArrayList 中扩容的范围是多大一次？

*   int newCapacity = oldCapacity + (oldCapacity >> 1); 每次扩容是当前容量的一半

5.ArrayList 是如何完成扩容的，System.arraycopy 各个入参的作用是什么？

*   如果需要扩容，会调用 Arrays.copyOf(elementData, newCapacity); 其内部新建一个长度为 newCapacity 的数组 copy，然后调用 System.arraycopy 将原始数组 elementData 复制到 copy 数组上，最后将 copy 的引用赋给 elementData 完成扩容.
*   System.arraycopy(src, srcPos, dest, destPos, length)
    *   src 为原始数组
    *   srcPos 为原始数组从哪个位置开始复制的索引
    *   dest 为目标数组，原始数组会被复制到该数组中
    *   destPos 复制到目标数组中的第一个位置索引
    *   length 为原始数组复制的长度

## problem:

1.elementData为什么设置为 transient ？

*   比较靠谱的说法：
    elementData数组相当于容器，当容器不足时就会再扩充容量，但是容器的容量往往都是大于或者等于ArrayList所存元素的个数。比如，现在实际有了8个元素，那么elementData数组的容量可能是8x1.5=12，如果直接序列化elementData数组，那么就会浪费4个元素的空间，特别是当元素个数非常多时，这种浪费是非常不合算的。所以ArrayList的设计者将elementData设计为transient，然后在writeObject方法中手动将其序列化，并且只序列化了实际存储的那些元素，而不是整个数组。

## 基本设计

*   设计成自动扩容：数组是一个固定的、连续的、线性的数据结构，那么想把它作为一个自动扩展容量的数组列表，则需要做一些扩展

```java
/**
 * 默认初始化空间
 */
private static final int DEFAULT_CAPACITY = 10;
/**
 * 空元素
 */
private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};
/**
 * ArrayList 元素数组缓存区
 */
transient Object[] elementData;
```

1.  添加元素

```java
public boolean add(E e) {
    // 确保内部容量
    int minCapacity = size + 1;
    if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {
        minCapacity = Math.max(DEFAULT_CAPACITY, minCapacity);
    }
    // 判断扩容操作
    if (minCapacity - elementData.length > 0) {
        int oldCapacity = elementData.length;
        int newCapacity = oldCapacity + (oldCapacity >> 1);
         if (newCapacity - minCapacity < 0) {
            newCapacity = minCapacity;
        }
        elementData = Arrays.copyOf(elementData, newCapacity);
    }
    // 添加元素
    elementData[size++] = e;
    return true;
}
```

1.  移除元素

```java
```

# day01 链表

## 一、前言

*   1955-1956年由德兰公司的Allen Newell、Cliff Shaw 和 Herbert A. Simon开发

## 二、链表数据结构

![image.png](https://note.youdao.com/yws/res/28316/WEBRESOURCE8278946dcb520d10d604a2b245c66fa2)

*   通过指针连接起来的线性元素集合，元素与元素之间在内存上不一定连续。其由一组节点构成，每个元素指向下一个元素，这些节点一起，表示线性序列。
*   每个节点由数据和指针两部分组成
*   链表的数据结构通过链的连接方式，不需要扩容空间就更高效的插入和删除元素的操作。但在一些需要遍历、指定位置操作、或者访问任意元素下，是需要循环遍历的，这将导致时间复杂度的提升。

## 三、链表分类类型

*   单向链表
*   双向链表
*   循环链表

## 四、实现一个链表

### 1. 链表节点

```java
private static class Node<E> {
	E item;
	Node<E> next;
	Node<E> prev;
	
	public Node(Node<E> prev, E element, Node<E> next) {
		this.item = element;
        this.next = next;
        this.prev = prev;
    }
}
```

### 2. 头插节点

```java
void linkFirst(E e) {
    final Node<E> f = first;
    final Node<E> newNode = new Node<>(null, e, f);
    first = newNode;
    if (f == null) 
        last == newNode;
    else 
        f.prev = newNode;
    size++;
}
```

### 3. 尾插节点

```java
void linkLast(E e) {
    final Node<E> l = last;
    final Node<E> newNode = new Node<>(l, e, null);
    last = newNode;
    if (l == null)
        first = newNode;
    else
        l.last = newNode;
    size++;
}
```

### 4. 拆链操作

```java
E unlink(Node<E> x) {
    final E element = x.item;
    final Node<E> next = x.next;
    final Node<E> prev = x.prev;
    if (prev == null) {
        first = next;
    } else {
        prev.next = next;
        x.prev = null;
    }
    if (next == null) {
        last = prev;
    } else {
        next.prev = prev;
        x.next = null;
    }
    x.item = null;
    size--;
    return element;
}
```

### 5. 删除节点

```java
public boolean remove(Object o) {
    if (o == null) {
        for (Node<E> x = first; x != null; x = x.next) {
            if (x.item == null) {
                unlink(x);
                return true;
            }
        }
    } else {
        for (Node<E> x = first; x != null; x = x.next) {
            if (o.equals(x.item)) {
                unlink(x);
                return true;
            }
        }
    }
    return false;
}
```

## 六、常见面试问题

*   描述一下链表的数据结构？
    *   链表是一种线性元素的集合，其通常由一个一个的节点构成，节点包含指针和元素两个部分，通过指针指向下一个节点。由于是通过指针来寻找下一个节点，节点与节点之间在物理内存中并不一定是连续的。
    *   通常链表分为单链表、双链表、循环链表等
*   Java 中 LinkedList 使用的是单向链表、双向链表还是循环链表？
    *   LinkedList是双向链表，通过next和prev两个指针指向下一个节点和上一个节点
*   链表中数据的插入、删除、获取元素，时间复杂度是多少？
    *   分别为O(1), O(1), O(n)
*   什么场景下使用链表更合适？
    *   插入和删除操作多使用链表更合适
    *   需要遍历的操作，如查找等不适合使用链表（相对于数组来说）

