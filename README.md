# -
# 基础算法之平衡二叉树的部分理论


# 一些概念：

假设一棵平衡二叉树，根节点是10，左右子节点分别是9和11，节点9的左右子节点分别是8和9.1，节点11的左右子节点分别是10.5和11.2

在数组中可表示为：

10_9_11_8_9.1_10.5_11.2 

高度：子节点到根节点的代数，例如10的第一代左子节点是9，第二代左子节点是8；

偏移：子节点距离同一代的最左子节点的节点数，例如根节点10的第二代子节点有8_9.1_10.5_11.2，节点8的偏移为0，
节点9.1的偏移为1，依次类推；

pow(2, n)：表示2的n次方，例如pow(2, 0) = 1，pow(2, 4) = 16，pow(2, 0) * 2 = pow(2, 1)；

int(x)：表示对数字x取整，例如2.8取整后得2。


平衡二叉树部分公式：

将平衡二叉树的节点以根节点开始，从左到右，从上到下按顺序存储在数组中（例如根节点为10的上述例子），那么有


## 公式一：

对于任意节点序号 i ，其左右节点的序号分别为

     l(i) = 2 * i + 1；

     r(i) = 2 * i + 2


证明：

根据平衡二叉树的定义，因为每个节点都有两个子节点，所以从第0层开始的每一层的节点数量是pow(2, h)，h是层的高度，假设任意处于高度是h的层最左端的节点p，
该节点的序号是pi = int(pow(2, 0) + pow(2, 1) + pow(2, 2) + … + pow(2, h - 1))，其左子节点必然位于高度为h+1的层的最左端，其左子节点的序号为
piL =  pow(2, 0) + pow(2, 1) + pow(2, 2) + … + pow(2, h)；
所以piL =  1 + 2 * int(pow(2, 1) + pow(2, 2) + … + pow(2, h - 1)) = 2 * pi + 1；(注意：pow(2, 0) = 1)

对于高度是h的层的其他任意节点，假设piAny = pi + d，d是其他节点距离pi的距离

根据定义，每个节点在下一层都有连续的2个子节点，那么在h层相邻的两个节点在h+1层有连续的4个子节点，
所以对应偏移d，在下一层对应为2 * d,
有piAnyL = piL + 2 * d = 2 * pi + 1 + 2d = 2 * (pi + d) + 1 = 2 * piAny + 1;

同理可证任意节点的右节点有 piAnyR = 2 * piAny + 2。


# 公式二：

对于任意节点序号i，其高度和偏移满足

i = int(pow(2, 0) + pow(2, 1) + pow(2, 2) + … + pow(2, h - 1)) + d；

d >= 0 ;

或

i = pow(2, 0) + pow(2, 1) + pow(2, 2) + … + pow(2, h - 1)，当 h > 0 ；
i = 0，当 h = 0 。

其中，d 为节点在高度h的层中的偏移，仅当h = 0时，需要取整。


证明可参考公式一。



# 公式三：

我们将对于形如

i = int(pow(2, 0) + … + pow(2, h - 1)) + d 的表述用下式代替：

i = (h, d)

其中，h 是高度，d 是节点在高度h的层中的偏移，根节点可表示为(0, 0)



# 公式四：

对于任意节点序号i = (h, d)，其左右子节点可表示为

l(i) = (h + 1, 2 * d);

r(i) = (h + 1, 2 * d + 1)


证明：

根据公式三有

l(i) = 2 * i + 1 = 2 * (pow(2, 0) + … + pow(2, h - 1) + d) + 1 = 1 + pow(2, 1) + … + pow(2, h) + 2 * d 
= pow(2, 0) + … + pow(2, h) + 2 * d
= (h + 1, 2 * d)；

同理 r(i) = (h + 1, 2 * d + 1)。



# 公式五：

对于任意平衡二叉树的子树，假设其节点为 i = (f(x), f(y))，以此为根节点的子树的节点(x, y)满足

f(x) = x + a;

f(y) = b * pow(2, x) + y;

其中，a = f(x = 0)，b = f(y = 0)


证明：

考虑一棵子树，其根节点在原树中的位置是(a, b)，在子树中的位置是(0, 0)，那么子树中的位置(x, y)相对于其根节点(0, 0)，

显然其高度差（节点(0, 0)和(x, y)）为x，所以这两个节点在原树中的高度差也是x，那么有

f(x) = a + x；

根据公式四可得，在平衡二叉树中，

任意节点(h, d)的左子节l(i) = (h + 1, 2 * d)，即该左子节点的偏移2 * d总是其直接父节点偏移d的2倍，

所以对于父节点(a, b)和其最左端的子节点(a + x, f(y))，有

f(y) = b * pow(2, x) ，

由于在子树中，对应节点(x, y)相对于最左节点有y的右向偏移量，所以有

f(y) = b * pow(2, x) + y;


# 一般地，

# 以(a, b)为根节点的任意子树中，对于子树节点(x, y)，有(a + x, b * pow(2, x) + y) 是子树节点(x, y)在原树中的位置。


# 公式六 

在以(a, b)为根节点的子树中，判断原树中的任意节点(f(x), f(y))是否是子树根节点的子节点的方法：

根据公式五，可以得到：

x = f(x) - a;

f(y) = b * pow(2, f(x) - a) + y;

y = f(y) - b * pow(2, f(x) - a);

因为y是第x代子节点的偏移，所以不能超过第x代的子节点数量，若 y 满足不等式，

0 <= y < pow(2, x)，即

如果不等式 0 <=  f(y) - b * pow(2, f(x) - a) < pow(2, f(x) - a)成立，那么原树节点(f(x), f(y))是子树根节点的子节点。

上述不等式即

0 <=  f(y) - b * pow(2, f(x) - a) < pow(2, f(x) - a)，

b * pow(2, f(x) - a) <= f(y) < pow(2, f(x) - a) + b * pow(2, f(x) - a)，

# b * pow(2, f(x) - a) <= f(y) < pow(2, f(x) - a) * (1 + b)。
