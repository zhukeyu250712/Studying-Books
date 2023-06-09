## DP

```
面试中设计暴力递归过程的原则
1) 每一个可变参数的类型，一定不要比int类型更加复杂
2) 原则1)可以违反，让类型突破到一维线性结构，那必须是单一可变参数
3) 如果发现原则1)被违反，但不违反原则2)，只需要做到记忆化搜索即可(leetcde贴纸问题)
4) 可变参数的个数，能少则少
```

```
知道了面试中设计暴力递归过程的原则，然后呢？
一定要逼自己找到不违反原则情况下的暴力尝试！
如果你找到的暴力尝试，不符合原则，马上舍弃！找新的！
如果某个题目突破了设计原则，一定极难极难，面试中出现概率低于5%！
```

```
常见的4种尝试模型
1)从左往右的尝试模型
2)范围上的尝试模型
3)多样本位置全对应的尝试模型（根据最后一个字符划分方案）
4)寻找业务限制的尝试模型
```

```
暴力递归到动态规划的套路
1) 你已经有了一个不违反原则的暴力递归，而且的确存在解的重复调用
2) 找到哪些参数的变化会影响返回值，对每一个列出变化范围
3) 参数间的所有的组合数量，意味着表大小
4) 记忆化搜索的方法就是傻缓存，非常容易得到
5) 规定好严格表的大小，分析位置的依赖顺序，然后从基础填写到最终解
6) 对于有枚举行为的决策过程，进一步优化
```

```
动态规划的进一步优化
1) 空间压缩
2) 状态化简
3) 四边形不等式
4) 其他优化技巧
```



### 1、经典递归

#### （1）汉诺塔问题

**题目描述：**

​		汉诺塔问题是指：一块板上有三根针 A、B、C。A 针上套有 64 个大小不等的圆盘，按照大的在下、小的在上的顺序排列，要把这 64 个圆盘从 A 针移动到 C 针上，每次只能移动一个圆盘，移动过程可以借助 B  针。但在任何时候，任何针上的圆盘都必须保持大盘在下，小盘在上。从键盘输入需移动的圆盘个数，给出移动的过程。

**算法思想：**

​		对于汉诺塔问题，当只移动一个圆盘时，直接将圆盘从 A 针移动到 C 针。若移动的圆盘为 n(n>1)，则分成几步走：把 (n-1)  个圆盘从 A 针移动到 B 针（借助 C 针）；A 针上的最后一个圆盘移动到 C 针；B 针上的 (n-1) 个圆盘移动到 C 针（借助 A  针）。每做一遍，移动的圆盘少一个，逐次递减，最后当 n 为 1 时，完成整个移动过程。

 因此，解决汉诺塔问题可设计一个递归函数，利用递归实现圆盘的整个移动过程，问题的解决过程是对实际操作的模拟。

**题解：**

```java
package class18;

import java.util.HashSet;
import java.util.Stack;
/*
question : 打印 N 层汉诺塔从最左边移动到最右边的全部过程
 */
public class Code02_Hanoi {

    public static void hanoi1(int n) {
        leftToRight(n);
    }

    // 请把1~N层圆盘 从左 -> 右
    public static void leftToRight(int n) {
        if (n == 1) { // base case
            System.out.println("Move 1 from left to right");
            return;
        }
        leftToMid(n - 1);
        System.out.println("Move " + n + " from left to right");
        midToRight(n - 1);
    }

    // 请把1~N层圆盘 从左 -> 中
    public static void leftToMid(int n) {
        if (n == 1) {
            System.out.println("Move 1 from left to mid");
            return;
        }
        leftToRight(n - 1);
        System.out.println("Move " + n + " from left to mid");
        rightToMid(n - 1);
    }

    public static void rightToMid(int n) {
        if (n == 1) {
            System.out.println("Move 1 from right to mid");
            return;
        }
        rightToLeft(n - 1);
        System.out.println("Move " + n + " from right to mid");
        leftToMid(n - 1);
    }

    public static void midToRight(int n) {
        if (n == 1) {
            System.out.println("Move 1 from mid to right");
            return;
        }
        midToLeft(n - 1);
        System.out.println("Move " + n + " from mid to right");
        leftToRight(n - 1);
    }

    public static void midToLeft(int n) {
        if (n == 1) {
            System.out.println("Move 1 from mid to left");
            return;
        }
        midToRight(n - 1);
        System.out.println("Move " + n + " from mid to left");
        rightToLeft(n - 1);
    }

    public static void rightToLeft(int n) {
        if (n == 1) {
            System.out.println("Move 1 from right to left");
            return;
        }
        rightToMid(n - 1);
        System.out.println("Move " + n + " from right to left");
        midToLeft(n - 1);
    }


    public static void hanoi2(int n) {
        if (n > 0) {
            func(n, "left", "right", "mid");
        }
    }

    /*
    1~N
    在:from
    到: to
    另一个: other
    */
    public static void func(int N, String from, String to, String other) {
        if (N == 1) { // base
            System.out.println("Move 1 from " + from + " to " + to);
        } else {
            func(N - 1, from, other, to);
            System.out.println("Move " + N + " from " + from + " to " + to);
            func(N - 1, other, to, from);
        }
    }

    public static class Record {
        public int level;
        public String from;
        public String to;
        public String other;

        public Record(int l, String f, String t, String o) {
            level = l;
            from = f;
            to = t;
            other = o;
        }
    }

    // 之前的迭代版本，很多同学表示看不懂
    // 所以我换了一个更容易理解的版本
    // 看注释吧！好懂！
    // 你把汉诺塔问题想象成二叉树
    // 比如当前还剩i层，其实打印这个过程就是：
    // 1) 去打印第一部分 -> 左子树
    // 2) 打印当前的动作 -> 当前节点
    // 3) 去打印第二部分 -> 右子树
    // 那么你只需要记录每一个任务 : 有没有加入过左子树的任务
    // 就可以完成迭代对递归的替代了
    public static void hanoi3(int N) {
        if (N < 1) {
            return;
        }
        // 每一个记录进栈
        Stack<Record> stack = new Stack<>();
        // 记录每一个记录有没有加入过左子树的任务
        HashSet<Record> finishLeft = new HashSet<>();
        // 初始的任务，认为是种子
        stack.add(new Record(N, "left", "right", "mid"));
        while (!stack.isEmpty()) {
            // 弹出当前任务
            Record cur = stack.pop();
            if (cur.level == 1) {
                // 如果层数只剩1了
                // 直接打印
                System.out.println("Move 1 from " + cur.from + " to " + cur.to);
            } else {
                // 如果不只1层
                if (!finishLeft.contains(cur)) {
                    // 如果当前任务没有加入过左子树的任务
                    // 现在就要加入了！
                    // 把当前的任务重新压回去，因为还不到打印的时候
                    // 再加入左子树任务！
                    finishLeft.add(cur);
                    stack.push(cur);
                    stack.push(new Record(cur.level - 1, cur.from, cur.other, cur.to));
                } else {
                    // 如果当前任务加入过左子树的任务
                    // 说明此时已经是第二次弹出了！
                    // 说明左子树的所有打印任务都完成了
                    // 当前可以打印了！
                    // 然后加入右子树的任务
                    // 当前的任务可以永远的丢弃了！
                    // 因为完成了左子树、打印了自己、加入了右子树
                    // 再也不用回到这个任务了
                    System.out.println("Move " + cur.level + " from " + cur.from + " to " + cur.to);
                    stack.push(new Record(cur.level - 1, cur.other, cur.to, cur.from));
                }
            }
        }
    }

    public static void main(String[] args) {
        int n = 3;
        hanoi1(n);
        System.out.println("============");
        // 一个递归函数可以通过增加参数，来增加可能性
        hanoi2(n);
        System.out.println("============");
        hanoi3(n);
    }
}
```

#### （2）子序列问题

**问题描述：**

打印一个字符串的全部子序列(可以不连续)
打印一个字符串的全部子序列，要求不要出现重复字面值的子序列

**题解：**

```java
package class18;

import java.util.ArrayList;
import java.util.HashSet;
import java.util.List;

/*
question1: 打印一个字符串的全部子序列(可以不连续)
question2: 打印一个字符串的全部子序列，要求不要出现重复字面值的子序列
* 递归：
* （1）递归函数的意义
* （2）base case
* */
public class Code03_PrintAllSubsquences {

    public static List<String> subs(String s){
        char[] str = s.toCharArray();
        String path = "";
        List<String> ans = new ArrayList<>();
        dfs(str, 0, ans, path);
        return ans;
    }

    //str 固定参数
    //来到str[u]字符, u是位置
    // str[0~u-1]已经走过了，之前都遍历好了，存在path里面，之前的决定已经不能改变了，就是path
    //str[u....]还未确定，u前面已经确定了，u之后还能自己选择
    //所有的子序列path当前路径的最终答案，都放在ans里面
    public static void dfs(char[] str, int u, List<String> ans, String path){
        if(u==str.length){
            ans.add(path);
            return;
        }
        //不选择u位置的字符
        dfs(str,u+1,ans,path);

        //选择u位置的字符
        dfs(str, u+1,ans, path + String.valueOf(str[u]));
    }


    public static List<String> subsNoRepeat(String s){
        char[] str = s.toCharArray();
        String path = "";
        HashSet<String> set = new HashSet<>();
        dfs2(str, 0, set, path);
        List<String> ans = new ArrayList<>();
        for(String cur :set){
            ans.add(cur);
        }
        return ans;
    }

    public static void dfs2(char[] str,int u, HashSet<String> set, String path){
        if(u == str.length){
            set.add(path);
            return;
        }
        //不选择u位置
        dfs2(str, u+1, set, path);

        //选择u位置
        dfs2(str, u+1, set, path + String.valueOf(str[u]));
    }

    public static void main(String[] args) {
        String str="abc";
        System.out.println("*****打印一个字符串的全部子序列(可以不连续)*********");
        List<String> ans1 = subs(str);
        for(String s:ans1){
            System.out.println(s);
        }
        System.out.println("*********************");

        System.out.println("*****打印一个字符串的全部子序列，要求不要出现重复字面值的子序列********");
        List<String> ans2 = subsNoRepeat(str);
        for(String s:ans2){
            System.out.println(s);
        }
        System.out.println("*********************");
    }
}
```

#### （3）全排列

**问题描述：**

打印一个字符串的全排列
打印一个字符串的全排列,要求不重复

**题解：**

```java
package class18;

/*
question1: 打印一个字符串的全排列
question2: 打印一个字符串的全排列,要求不重复
* 清除线程
* */
import java.util.ArrayList;
import java.util.List;

public class Code04_PrintAllPermutations {

    public static List<String> permutation1(String s){
        List<String> ans = new ArrayList<>();
        if (s == null || s.length() == 0){
            return ans;
        }
        char[] str = s.toCharArray();
        ArrayList<Character> rest = new ArrayList<>();
        for(char ch : str) {
            rest.add(ch);
        }
        String path = "";
        f(rest, path, ans);
        return ans;
    }

    public static void f(ArrayList<Character> rest, String path, List<String> ans){
        if (rest.isEmpty()){    //base case
            ans.add(path);
        }else {
            for(int i=0; i < rest.size(); i++){
                char cur = rest.get(i);
                rest.remove(i);
                f(rest, path+cur, ans);
                rest.add(i ,cur);   // 回溯恢复现场
            }
        }
    }

    public static List<String> permutation2(String s){
        List<String> ans = new ArrayList<>();
        if (s == null || s.length() == 0){
            return ans;
        }
        char[] str = s.toCharArray();
        g1(str, 0, ans);
        return ans;
    }

    public static void g1(char[] str, int index, List<String> ans){
        if (index == str.length){
            ans.add(String.valueOf(str));
        } else {
            for(int i = index; i<str.length; i++) {
                swap(str, index, i);
                g1(str, index+1, ans);
                swap(str, index, i);
            }
        }
    }

    //去重复
    public static List<String> permutation3(String s){
        List<String> ans = new ArrayList<>();
        if (s == null || s.length() == 0){
            return ans;
        }
        char[] str = s.toCharArray();
        g2(str, 0, ans);
        return ans;
    }

    public static void g2(char[] str, int index, List<String> ans){
        if (index == str.length){
            ans.add(String.valueOf(str));
        } else {
            boolean[] visited = new boolean[256];
            for(int i = index; i<str.length; i++) {
                if(!visited[str[i]]){
                    visited[str[i]] = true;
                    swap(str, index, i);
                    g2(str, index+1, ans);
                    swap(str, index, i);
                }
            }
        }
    }

    public static void swap(char[] chs, int i, int j) {
        char tmp = chs[i];
        chs[i] = chs[j];
        chs[j] = tmp;
    }

    public static void main(String[] args) {
        String str = "acc";
        List<String> ans1 = permutation1(str);
        for(String cur : ans1){
            System.out.println(cur);
        }
        System.out.println("=======");
        List<String> ans2 = permutation2(str);
        for (String c : ans2) {
            System.out.println(c);
        }
        System.out.println("=======");
        List<String> ans3 = permutation3(str);
        for(String c: ans3){
            System.out.println(c);
        }
    }
}
```



#### （4）递归实现栈的反转

**题目描述：**

给你一个栈，请你逆序这个栈，
不能申请额外的数据结构，
只能使用递归函数。如何实现？

**题解：**

```java
package class18;

import java.util.Stack;

/*
仰望好的尝试？
给你一个栈，请你逆序这个栈，
不能申请额外的数据结构，
只能使用递归函数。如何实现？
 */
public class Code05_ReverseStackUsingRecursive {
    public static void reverse(Stack<Integer> stack) {
        if (stack.isEmpty()) {
            return;
        }
        int i = f(stack);
        reverse(stack);
        stack.push(i);
    }

    // 栈底元素移除掉
    // 上面的元素盖下来
    // 返回移除掉的栈底元素
    public static int f(Stack<Integer> stack) {
        int result = stack.pop();
        if (stack.isEmpty()) {
            return result;
        } else {
            int last = f(stack);
            stack.push(result);
            return last;
        }
    }

    public static void main(String[] args) {
        Stack<Integer> test = new Stack<Integer>();
        test.push(1);
        test.push(2);
        test.push(3);
        test.push(4);
        test.push(5);
        reverse(test);
        while (!test.isEmpty()) {
            System.out.println(test.pop());
        }

    }
}
```



### 2、基础DP

#### （1）有边界的杨辉三角类型

**题目描述：**

假设有排成一行的N个位置，记为1~N,N一定大于或等于2
开始时机器人在其中的M位置上(M一定是1~N中的一个)
如果机器人来到1位置，那么下一步只能往右来到2位置；
如果机器人来到N位置，那么下一步只能往左来到N-1位置；
如果机器人来到中间位置，那么下一步可以往左走或者往右走；
规定机器人必须走K步，最终能来到P位置(P也是1~N中的一个)的方法有多少种
给定四个参数N、M、K、P,返回方法数。

**题解：**

```java
package class19;

/*
假设有排成一行的N个位置，记为1~N,N一定大于或等于2
开始时机器人在其中的M位置上(M一定是1~N中的一个)
如果机器人来到1位置，那么下一步只能往右来到2位置；
如果机器人来到N位置，那么下一步只能往左来到N-1位置；
如果机器人来到中间位置，那么下一步可以往左走或者往右走；
规定机器人必须走K步，最终能来到P位置(P也是1~N中的一个)的方法有多少种
给定四个参数N、M、K、P,返回方法数。
* */
public class Code01_RobotWalk {

    public static int ways1(int N, int start, int aim, int K){
        if (N < 2 || start <1 || start > N || aim <1 || aim > N || K<1){
            return -1;
        }
        return process1(start, K, aim, N);
    }

    /*
    * cur:机器人当前来到的位置是cur
    * rest:机器人还有rest步需要走
    * aim:最终的目标
    * N:有哪些位置?1~N
    * 返回：机器人从cur出发，走过rest步之后，最终停在aim的方法数是多少
    * */
    public static int process1(int cur, int rest, int aim, int N){
        if(rest == 0) { //如果已经不需要走了，走完了
            return cur == aim ? 1 : 0;
        }
        //rest > 0 ,还有步数需要走！
        if (cur == 1){ //1~2
            return process1(2,rest-1, aim, N);
        }
        if(cur == N){ //N-->N-1
            return process1(N-1, rest-1, aim, N);
        }
        //中间位置
        return process1(cur - 1, rest-1, aim, N) + process1(cur + 1, rest-1, aim, N);
    }

    //加缓存:自定向下dp(记忆化搜索)n
    public static int ways2(int N, int start, int aim, int K){
        if (N < 2 || start <1 || start > N || aim <1 || aim > N || K<1){
            return -1;
        }
        /*
         * dp是缓存表
         * dp[cur][rest] == -1  表示process2(cur, rest)之前没有算过
         * dp[cur][rest] != -1  表示process2(cur, rest)之前算过，返回值dp[cur][rest]
         * */
        int[][] dp = new int[N+1][K+1];
        for (int i = 0; i <=N; i++) {
            for(int j = 0; j <=K; j++){
                dp[i][j] = -1;
            }
        }
        return process2(start, K, aim, N, dp);
    }

    /*
     * cur范围：1~N
     * rest范围：0~K
     * */
    public static int process2(int cur, int rest, int aim, int N, int[][] dp){
        //之前算过了
        if (dp[cur][rest] != -1) return dp[cur][rest];
        //之前没算过
        int ans = 0;

        if(rest == 0) { //如果已经不需要走了，走完了
            ans = cur == aim ? 1 : 0;
        }else if (cur == 1){ //1~2   //rest > 0 ,还有步数需要走！
            ans = process2(2,rest-1, aim, N,dp);
        }else if(cur == N){ //N-->N-1
            ans = process2(N-1, rest-1, aim, N,dp);
        }
        else{
            ans = process2(cur - 1, rest-1, aim, N, dp) + process2(cur + 1, rest-1, aim, N, dp);
        }
        dp[cur][rest] = ans;
        return ans;
    }

    //加缓存:自定向下dp(记忆化搜索)n
    public static int ways3(int N, int start, int aim, int K){
        //最前面可以增加无效参数的检查
        if (N < 2 || start <1 || start > N || aim <1 || aim > N || K<1){
            return -1;
        }
        int[][] dp = new int[N+1][K+1];
        dp[aim][0] = 1; //dp[...][0] = 0;

        for(int rest = 1; rest <= K; rest++) { //列
            dp[1][rest] = dp[2][rest-1]; //第一行,只依赖于左下角
            for(int cur = 2; cur < N; cur++) {  //行
                dp[cur][rest] = dp[cur-1][rest-1] + dp[cur+1][rest-1];
            }
            dp[N][rest] = dp[N-1][rest-1]; //最后一行，只依赖于左上角
        }
        return dp[start][K];
    }

    /*
     * cur范围：1~N
     * rest范围：0~K
     * */
    public static int process3(int cur, int rest, int aim, int N, int[][] dp){
        //之前算过了
        if (dp[cur][rest] != -1) return dp[cur][rest];
        //之前没算过
        int ans = 0;

        if(rest == 0) { //如果已经不需要走了，走完了
            ans = cur == aim ? 1 : 0;
        }else if (cur == 1){ //1~2   //rest > 0 ,还有步数需要走！
            ans = process2(2,rest-1, aim, N,dp);
        }else if(cur == N){ //N-->N-1
            ans = process2(N-1, rest-1, aim, N,dp);
        }
        else{
            ans = process2(cur - 1, rest-1, aim, N, dp) + process2(cur + 1, rest-1, aim, N, dp);
        }
        dp[cur][rest] = ans;
        return ans;
    }


    public static void main(String[] args) {
        int ans1 = ways1(4, 2, 4, 4);
        int ans2 = ways1(4, 2, 4, 4);
        int ans3 = ways3(4, 2, 4, 4);
        System.out.println(ans2);
        System.out.println(ans3);
    }
}
```

#### （2）范围题型

####   博弈论先后手

**题目描述：**

给定一个整型数组arr,代表数值不同的纸牌排成一条线
玩家A和玩家B依次拿走每张纸牌
规定玩家A先拿，玩家B后拿
但是每个玩家每次只能拿走最左或最右的纸牌
玩家A和玩家B都绝顶聪明
请返回最后获胜者的分数。

**题解：**

```java
package class19;

/*
给定一个整型数组arr,代表数值不同的纸牌排成一条线
玩家A和玩家B依次拿走每张纸牌
规定玩家A先拿，玩家B后拿
但是每个玩家每次只能拿走最左或最右的纸牌
玩家A和玩家B都绝顶聪明
请返回最后获胜者的分数。
* */
public class Code02_CardsInLine {

    //根据规则，返回获胜者的分数
    public static int win1(int[] arr) {
        if (arr == null || arr.length ==0) return 0;
        int first  = f1(arr,0,arr.length-1);
        int second  = g1(arr,0,arr.length-1);
        return Math.max(first,second);
    }

    //arr[l,r],先手在[l, r]获得的最大分数
    public static int f1(int[] arr, int l, int r){
        if (l == r) return arr[l];
        int p1 = arr[l] + g1(arr,l+1, r);
        int p2 = arr[r] + g1(arr, l, r-1);
        return Math.max(p1,p2);
    }

    ////arr[l,r],后手在[l, r]获得的最大分数
    public static int g1(int[] arr, int l, int r){
        if (l == r) return 0;   //只有最后一章牌了，你是后手，则得到0
        //对方拿走了l位置的数
        int p1 = f1(arr,l+1,r);
        //对方拿走了r位置的数
        int p2 = f1(arr,l,r-1);
        return Math.min(p1,p2);
    }

    //根据规则，返回获胜者的分数
    /*
    f(0,7)
    g(1,7)              g(0,6)
    f(1,6) f(2,7)       f(0,5) f(1,6)
    * */
    public static int win2(int[] arr) {
        if (arr == null || arr.length ==0) return 0;

        int N = arr.length;
        int[][] fmap = new int[N][N];
        int[][] gmap = new int[N][N];

        for(int i=0;i<N;i++){
            for(int j=0;j<N;j++){
                fmap[i][j]=-1;
                gmap[i][j]=-1;
            }
        }
        int first  = f2(arr,0,arr.length-1,fmap,gmap);
        int second  = g2(arr,0,arr.length-1,fmap,gmap);
        return Math.max(first,second);
    }

    //arr[l,r],先手在[l, r]获得的最大分数
    public static int f2(int[] arr, int l, int r,int[][] fmap, int[][] gmap){
        //如果已经求出了直接使用
        if(fmap[l][r] != -1) return fmap[l][r];

        //还没有求出
        int ans = 0;
        if (l == r) ans = arr[l];
        else {
            int p1 = arr[l] + g2(arr,l+1, r,fmap,gmap);
            int p2 = arr[r] + g2(arr, l, r-1,fmap,gmap);
            ans = Math.max(p1,p2);
        }
        fmap[l][r] = ans;
        return ans;
    }

    ////arr[l,r],后手在[l, r]获得的最大分数
    public static int g2(int[] arr, int l, int r,int[][] fmap, int[][] gmap){
        if (gmap[l][r] != -1) return gmap[l][r];

        int ans = 0;
//        if (l == r) return 0;   //只有最后一章牌了，你是后手，则得到0
        if (l != r){
            //对方拿走了l位置的数
            int p1 = f2(arr,l+1,r,fmap,gmap);
            //对方拿走了r位置的数
            int p2 = f2(arr,l,r-1,fmap,gmap);
            ans = Math.min(p1,p2);
        }
        gmap[l][r] = ans;
        return ans;
    }

    public static int win3(int[] arr) {
        if (arr == null || arr.length ==0) return 0;

        int N = arr.length;
        int[][] fmap = new int[N][N];
        int[][] gmap = new int[N][N];

        //初始化fmap对角线,gmap对角线默认就是0不管
        for(int i=0;i<N;i++){
            fmap[i][i] = arr[i];
        }

        //右上三角
        for(int startR = 1; startR < N; startR++) {  //列
            int L = 0;
            int R = startR;
            //对角线填充
            while(R < N) {
                fmap[L][R] = Math.max(arr[L]+gmap[L+1][R],arr[R]+gmap[L][R-1]);
                gmap[L][R] = Math.min(fmap[L+1][R],fmap[L][R-1]);
                L ++;
                R ++;
            }
        }
        return Math.max(fmap[0][N-1],gmap[0][N-1]);
    }


    public static void main(String[] args) {
        int[] arr = {1,100,1};
        int result1 = win1(arr);
        int result2 = win2(arr);
        int result3 = win3(arr);
        System.out.println(result3);
    }
}
```

#### （3）从左往右题型

####   01背包问题

**题解：**

```java
package class20;

/*
* 背包问题
* 从左往右
* */
public class Code01_Knapsack {

    // 所有的货，重量和价值，都在w和v数组里
    // 为了方便，其中没有负数
    // rest背包容量，不能超过这个载重
    // 返回：不超重的情况下，能够得到的最大价值
    public static int maxValue (int[] w, int[] v, int rest) {
        if (w == null || v == null || w.length != v.length || w.length ==0){
            return 0;
        }

        //尝试函数
        return process(w, v, 0, rest);
    }

    /*
    * index之前的已经考虑过了，不再需要考虑，index之后的货物可以自由选择
    * 做的选择不能超过背包最大容量
    * 返回最大价值
    * */
    public static int process(int[] w, int[] v, int index, int rest){
        if (rest < 0){
//            return 0;
            return -1;
        }
        if (index == w.length) return 0;

        /*
        目前还有货物，index位置的货物
        rest有空间，0
        不要当前的货物
         */
        int p1 = process(w,v,index+1,rest);

        //要当前的货物
//        intp2 = v[index] + process(w, v, index+1,rest - w[index]);
        int p2 = 0;
        int next = process(w, v, index+1,rest - w[index]);
        if(next != -1){
            //有效的时候再加
            p2 = v[index] + next;
        }
        return Math.max(p1, p2);
    }

    /*
     * index范围：0~N
     * rest范围: 负数 ~ bag
     * */
    public static int dp (int[] w, int[] v, int bag) {
        if (w == null || v == null || w.length != v.length || w.length ==0){
            return 0;
        }
        int N = w.length;
        //index 0~N
        //rest 0~bag
        int[][] dp = new int[N+1][bag+1];
        //dp[N][0~N]=0  N行全0，数组默认全0
        //从N-1行开始填dp,下往上
        for(int index = N-1;index >=0; index--){
            for(int rest = 0;rest <= bag; rest++){
                //不选
                int p1 = dp[index+1][rest];
                //选择
                int p2 = 0;
//                int next = rest - w[index] <0 ? -1 : dp[index+1][rest-w[index]];
                if(rest - w[index] >= 0){
                    p2 = v[index] + dp[index+1][rest - w[index]];
                }
                dp[index][rest] = Math.max(p1, p2);
            }
        }
        return dp[0][bag];
    }

    public static void main(String[] args) {
        int[] weights = { 3, 2, 4, 7, 3, 1, 7 };
        int[] values = { 5, 6, 3, 19, 12, 4, 2 };
        int rest = 15;
        System.out.println(maxValue(weights, values, rest));
        System.out.println(dp(weights, values, rest));
    }
}
```

#### （4）从右向左题型

#### 字符串转化相关

**题目描述：**

规定1和A对应、2和B对应、3和C对应.......26和Z对应
那么一个数字字符串比如"111”就可以转化为：
"AAA"、"KA"和"AK"看成11+1
给定一个只有数字字符组成的字符串str,返回有多少种转化结果

**题解：**

```java
package class20;

/*
规定1和A对应、2和B对应、3和C对应.......26和Z对应
那么一个数字字符串比如"111”就可以转化为：
"AAA"、"KA"和"AK"看成11+1
给定一个只有数字字符组成的字符串str,返回有多少种转化结果
* */
public class Code02_ConvertToLetterString {
    //str只含有数字字符0~9
    // 返回多少种转化方案
    public static int number(String str) {
        if (str == null || str.length() ==0 ) return 0;
        return process(str.toCharArray(),0);
    }

    //str[0.....i-1]转化不需要了解
    //str[i,....]去转化，返回需要多少种转化方案
    public static int process(char[] str, int u) {
        //u到了最后一步了，前面u-1已经转化结束了，前面已经转化了，此时u结束，但是需要收集前面的结果
        if (u == str.length) return 1;

        //u没有到最后，说明有字符
        if (str[u] == '0'){ //之前的决定有问题
            return 0;
        }

        //str[u] != '0'
        //(1) u位置单独转化
        int ways = process(str, u+1);
        //(2) u位置不能单独自己转化，需要和u+1一起转化
        if (u+1 < str.length && (str[u]-'0') * 10 +str[u+1]-'0' < 27){
            ways += process(str, u+2);
        }
        return ways;
    }

    public static int dp(String s) {
        if (s == null || s.length() == 0) return 0;
        char[] str = s.toCharArray();
        int N = str.length;
        int[] dp = new int[N+1];
        dp[N] = 1;
        for(int i = N-1; i >= 0; i--){
            if (str[i] != '0'){
                int ways = dp[i+1];
                if (i+1 < str.length && (str[i]-'0') * 10 +str[i+1]-'0' < 27){
                    ways += process(str, i+2);
                }
                dp[i] = ways;
            }
        }
        return dp[0];
    }

    public static void main(String[] args) {
        String str = "123455623";
        System.out.println(dp(str));
        System.out.println(number(str));
    }
}
```



#### 贴纸问题  （记忆化搜索）

**题目描述：Leetcode[691. 贴纸拼词](https://leetcode.cn/problems/stickers-to-spell-word/)**

给定一个字符串str,给定一个字符串类型的数组arr,出现的字符都是小写英文
arr每一个字符串，代表一张贴纸，你可以把单个字符剪开使用，目的是拼出str来
返回需要至少多少张贴纸可以完成这个任务。
例子：str="babac'",arr={"ba","c","abcd"}
至少需要两张贴纸"ba"和"abcd",因为使用这两张贴纸，把每一个字符单独剪开，含
有2个a、2个b、1个c。是可以拼出str的。所以返回2。

**题解：**

```java
package class20;

import sun.util.resources.cldr.zh.CalendarData_zh_Hans_HK;

import java.util.HashMap;
import java.util.stream.Stream;

//测试链接（leetcode 691）：https://leetcode.cn/problems/stickers-to-spell-word/
public class Code03_StickersToSpellWord {

    public int minStickers(String[] stickers, String target) {
        int  ans = process1(stickers,target);
        return ans == Integer.MAX_VALUE ? -1 : ans;
    }

    //所有贴纸stickers,每种贴纸都有无穷张
    //target
    //return:最小张数
    public static int process1(String[] stickers, String target) {
        //边界，已经没有了，还需要0张
        if (target.length() == 0) return 0;
        int min = Integer.MAX_VALUE;
        for(String first : stickers) {
            //每张贴纸作为第一张，还能剩余的情况
            String rest = minus(target, first);
            //剪完first之后和原来字符串不一样
            if(rest.length() != target.length()) {
                min = Math.min(min, process1(stickers, rest));
            }
        }
        // 1为first
        return min + (min == Integer.MAX_VALUE ? 0 : 1);
    }

    public static String minus(String s1, String s2) {
        char[] str1 = s1.toCharArray();
        char[] str2 = s2.toCharArray();
        int[] count = new int[26];
        for (char cha : str1) {
            count[cha - 'a']++;
        }
        for (char cha : str2) {
            count[cha - 'a']--;
        }
        StringBuilder builder = new StringBuilder();
        for (int i = 0; i < 26; i++) {
            if (count[i] > 0) {
                for (int j = 0; j < count[i]; j++) {
                    builder.append((char) (i + 'a'));
                }
            }
        }
        return builder.toString();
    }

    public int minStickers2(String[] stickers, String target) {
        int N = stickers.length;
        //优化
        int[][] counts = new int[N][26];
        for(int i = 0; i < N; i++){
            char[] str = stickers[i].toCharArray();
            for(char cha : str){
                counts[i][cha - 'a']++;
            }
        }
        int ans = process2(counts, target);
        return ans == Integer.MAX_VALUE ? -1 : ans;
    }

    // stickers[i] 数组，当初i号贴纸的字符统计 int[][] stickers -> 所有的贴纸
    // 每一种贴纸都有无穷张
    // 返回搞定target的最少张数
    // 最少张数
    public static int process2(int[][] stickers, String t) {
        if (t.length() == 0) return 0;
        //target做出词频统计
        //target aabbc  2 2 1
        //              0 1 2
        char[] target  = t.toCharArray();
        int[] tcounts = new int[26];
        for(char cha : target){
            tcounts[cha - 'a']++;
        }
        int N = stickers.length;
        int min = Integer.MAX_VALUE;
        for(int i = 0; i < N; i++){
            //尝试第一张贴纸
            int[] sticker = stickers[i];
            //关键性优化：剪枝（贪心）
            //看sticker里面是否存在target的第一个字符，有的化才执行逻辑操作
            if(sticker[target[0] - 'a'] > 0) {
                StringBuilder builder  = new StringBuilder();
                for(int j = 0; j < 26; j++){
                    if(tcounts[j] > 0){
                        int sum = tcounts[j] - sticker[j];
                        for(int k = 0; k < sum; k++){
                            builder.append((char)(j + 'a'));
                        }
                    }
                }
                String rest = builder.toString();
                min = Math.min(min, process2(stickers,rest));
            }
        }
        return min + (min == Integer.MAX_VALUE ? 0 : 1);
    }

    public static int minStickers3(String[] stickers, String target) {
        int N = stickers.length;
        //优化
        int[][] counts = new int[N][26];
        for(int i = 0; i < N; i++){
            char[] str = stickers[i].toCharArray();
            for(char ch : str) {
                counts[i][ch - 'a'] ++;
            }
        }
        HashMap<String, Integer> dp = new HashMap<>();
        dp.put("",0);
        int ans = process3(counts, target, dp);
        return ans == Integer.MAX_VALUE ? -1 : ans;
    }

    public static int process3(int[][] stickers, String t, HashMap<String, Integer> dp) {
        if (dp.containsKey(t)){
            return dp.get(t);
        }
        char[] target = t.toCharArray();
        int[] tcounts = new int[26];
        for(char cha : target) {
            tcounts[cha - 'a'] ++;
        }
        int N = stickers.length;
        int min = Integer.MAX_VALUE;
        for(int i = 0; i < N; i++) {
            int[] sticker = stickers[i];
            if(sticker[target[0] - 'a'] > 0){
                StringBuilder builder = new StringBuilder();
                for(int j = 0; j < 26; j++) {
                    if(tcounts[j] > 0){
                        int sum = tcounts[j] - sticker[j];
                        for(int k = 0; k < sum; k++){
                            builder.append((char)(j + 'a'));
                        }
                    }
                }
                String rest = builder.toString();
                min = Math.min(min, process3(stickers, rest, dp));
            }
        }
        int ans = min + (min == Integer.MAX_VALUE ? 0 : 1);
        dp.put(t,ans);
        return ans;
    }

    public static void main(String[] args) {

    }
}
```

#### （5）样本对应题型

#### 最长公共子序列

[Leetcode 1143. 最长公共子序列](https://leetcode.cn/problems/longest-common-subsequence/)

给定两个字符串 text1 和 text2，返回这两个字符串的最长 公共子序列 的长度。如果不存在 公共子序列 ，返回 0 。

一个字符串的 子序列 是指这样一个新的字符串：它是由原字符串在不改变字符的相对顺序的情况下删除某些字符（也可以不删除任何字符）后组成的新字符串。

    例如，"ace" 是 "abcde" 的子序列，但 "aec" 不是 "abcde" 的子序列。

两个字符串的 公共子序列 是这两个字符串所共同拥有的子序列。

**示例 1：**

```
输入：text1 = "abcde", text2 = "ace" 
输出：3  
解释：最长公共子序列是 "ace" ，它的长度为 3 。
```

**示例 2：**

```
输入：text1 = "abc", text2 = "abc"
输出：3
解释：最长公共子序列是 "abc" ，它的长度为 3 。
```

**示例 3：**

```
输入：text1 = "abc", text2 = "def"
输出：0
解释：两个字符串没有公共子序列，返回 0 。
```

**提示：**

$1 <= text1.length, text2.length <= 1000$,
$text1$ 和 $text2$ 仅由小写英文字符组成。

**题解：**

```java
package class20;

public class Code04_LongestCommonSubsequence {
    public int longestCommonSubsequence(String s1, String s2) {
        if (s1.length() == 0 || s2.length() == 0 || s1 == null || s2 == null) return 0;
        char[] str1 = s1.toCharArray();
        char[] str2 = s2.toCharArray();
        //尝试
        return process1(str1, str2, str1.length - 1, str2.length - 1);
    }

    //str1[0....i]和str2[0....j]最长公共子序列最长
    //返回
    /*
     str1[0...i]和str2[0...j]，这个范围上最长公共子序列长度是多少？
     可能性分类:
     a) 最长公共子序列，一定不以str1[i]字符结尾、也一定不以str2[j]字符结尾
     b) 最长公共子序列，可能以str1[i]字符结尾、但是一定不以str2[j]字符结尾
     c) 最长公共子序列，一定不以str1[i]字符结尾、但是可能以str2[j]字符结尾
     d) 最长公共子序列，必须以str1[i]字符结尾、也必须以str2[j]字符结尾
     注意：a)、b)、c)、d)并不是完全互斥的，他们可能会有重叠的情况
     但是可以肯定，答案不会超过这四种可能性的范围
     那么我们分别来看一下，这几种可能性怎么调用后续的递归。
     a) 最长公共子序列，一定不以str1[i]字符结尾、也一定不以str2[j]字符结尾
        如果是这种情况，那么有没有str1[i]和str2[j]就根本不重要了，因为这两个字符一定没用啊
        所以砍掉这两个字符，最长公共子序列 = str1[0...i-1]与str2[0...j-1]的最长公共子序列长度(后续递归)
     b) 最长公共子序列，可能以str1[i]字符结尾、但是一定不以str2[j]字符结尾
        如果是这种情况，那么我们可以确定str2[j]一定没有用，要砍掉；但是str1[i]可能有用，所以要保留
        所以，最长公共子序列 = str1[0...i]与str2[0...j-1]的最长公共子序列长度(后续递归)
     c) 最长公共子序列，一定不以str1[i]字符结尾、但是可能以str2[j]字符结尾
        跟上面分析过程类似，最长公共子序列 = str1[0...i-1]与str2[0...j]的最长公共子序列长度(后续递归)
     d) 最长公共子序列，必须以str1[i]字符结尾、也必须以str2[j]字符结尾
        同时可以看到，可能性d)存在的条件，一定是在str1[i] == str2[j]的情况下，才成立的
        所以，最长公共子序列总长度 = str1[0...i-1]与str2[0...j-1]的最长公共子序列长度(后续递归) + 1(共同的结尾)
     综上，四种情况已经穷尽了所有可能性。四种情况中取最大即可
     其中b)、c)一定参与最大值的比较，
     当str1[i] == str2[j]时，a)一定比d)小，所以d)参与
     当str1[i] != str2[j]时，d)压根不存在，所以a)参与
     但是再次注意了！
     a)是：str1[0...i-1]与str2[0...j-1]的最长公共子序列长度
     b)是：str1[0...i]与str2[0...j-1]的最长公共子序列长度
     c)是：str1[0...i-1]与str2[0...j]的最长公共子序列长度
     a)中str1的范围 < b)中str1的范围，a)中str2的范围 == b)中str2的范围
     所以a)不用求也知道，它比不过b)啊，因为有一个样本的范围比b)小啊！
     a)中str1的范围 == c)中str1的范围，a)中str2的范围 < c)中str2的范围
     所以a)不用求也知道，它比不过c)啊，因为有一个样本的范围比c)小啊！
     至此，可以知道，a)就是个垃圾，有它没它，都不影响最大值的决策
     所以，当str1[i] == str2[j]时，b)、c)、d)中选出最大值
     当str1[i] != str2[j]时，b)、c)中选出最大值
    */
    public static int process1(char[] str1, char[] str2, int i, int j) {
        if (i == 0 && j == 0) {
            // str1[0..0]和str2[0..0]，都只剩一个字符了
            // 那如果字符相等，公共子序列长度就是1，不相等就是0
            // 这显而易见
            return str1[i] == str2[j] ? 1 : 0;
        } else if (i == 0) { //str1[0...i = 0] 只有一个字符的时候
            // 这里的情况为：
            // str1[0...0]和str2[0...j]，str1只剩1个字符了，但是str2不只一个字符
            // 因为str1只剩一个字符了，所以str1[0...0]和str2[0...j]公共子序列最多长度为1
            // 如果str1[0] == str2[j]，那么此时相等已经找到了！公共子序列长度就是1，也不可能更大了
            // 如果str1[0] != str2[j]，只是此时不相等而已，
            // 那么str2[0...j-1]上有没有字符等于str1[0]呢？不知道，所以递归继续找
            if (str1[i] == str2[j]) {
                return 1;
            } else {
                return process1(str1, str2, i, j - 1);
            }
        } else if (j == 0) {
            // 和上面的else if同理
            // str1[0...i]和str2[0...0]，str2只剩1个字符了，但是str1不只一个字符
            // 因为str2只剩一个字符了，所以str1[0...i]和str2[0...0]公共子序列最多长度为1
            // 如果str1[i] == str2[0]，那么此时相等已经找到了！公共子序列长度就是1，也不可能更大了
            // 如果str1[i] != str2[0]，只是此时不相等而已，
            // 那么str1[0...i-1]上有没有字符等于str2[0]呢？不知道，所以递归继续找
            if (str1[i] == str2[j]) {
                return 1;
            } else {
                return process1(str1, str2, i - 1, j);
            }
        } else {  // i != 0 && j != 0
            //不能以i结尾，可以j
            // 这里的情况为：
            // str1[0...i]和str2[0...i]，str1和str2都不只一个字符
            // 看函数开始之前的注释部分
            // p1就是可能性c)
            int p1 = process1(str1, str2, i - 1, j);
            //不能以j结尾，可以i
            // p2就是可能性b)
            int p2 = process1(str1, str2, i, j - 1);
            //需要i结尾，j结尾
            // p3就是可能性d)，如果可能性d)存在，即str1[i] == str2[j]，那么p3就求出来，参与pk
            // 如果可能性d)不存在，即str1[i] != str2[j]，那么让p3等于0，然后去参与pk，反正不影响
            int p3 = str1[i] == str2[j] ? (1 + process1(str1, str2, i - 1, j - 1)) : Integer.MIN_VALUE;
            return Math.max(p1, Math.max(p2, p3));
        }
    }

    public int longestCommonSubsequence2(String s1, String s2) {
        if (s1.length() == 0 || s2.length() == 0 || s1 == null || s2 == null) return 0;
        char[] str1 = s1.toCharArray();
        char[] str2 = s2.toCharArray();
        //尝试
        int N = str1.length;
        int M = str2.length;
        int[][] dp = new int[N][M];
        //递归里面的base case
        dp[0][0] = str1[0] == str2[0] ? 1 : 0;
        //0行
        for(int j = 1; j < M; j++){
            dp[0][j] = str1[0] == str2[j] ? 1 : dp[0][j-1];
        }
        //0列
        for(int i = 1; i < N; i++){
            dp[i][0] = str1[i] == str2[0] ? 1 : dp[i-1][0];
        }
        for(int i = 1; i < N; i++) {
            for(int j = 1; j < M; j++){
                //不能以i结尾，可以j
                int p1 = dp[i - 1][j];
                //不能以j结尾，可以i
                int p2 = dp[i][j - 1];
                //需要i结尾，j结尾
                int p3 = str1[i] == str2[j] ? (1 + dp[i - 1][j - 1]) : 0;
                dp[i][j] = Math.max(p1, Math.max(p2, p3));
            }
        }
        return dp[N-1][M-1];
    }
}
```

#### **最长回文子序列**

[516. 最长回文子序列](https://leetcode.cn/problems/longest-palindromic-subsequence/)

给你一个字符串 s ，找出其中最长的回文子序列，并返回该序列的长度。

子序列定义为：不改变剩余字符顺序的情况下，删除某些字符或者不删除任何字符形成的一个序列。

**示例 1：**

```
输入：s = "bbbab"
输出：4
解释：一个可能的最长回文子序列为 "bbbb" 。
```

**示例 2：**

```
输入：s = "cbbd"
输出：2
解释：一个可能的最长回文子序列为 "bb" 。
```

**提示：**

- `1 <= s.length <= 1000`
- `s` 仅由小写英文字母组成

**题解：**

```java
package class21;

/*
leetcode 516. 最长回文子序列
给定一个字符串str,返回这个字符串的最长回文子序列长度
比如：str=“a12b3c43def2ghi1kpm"
最长回文子序列是“1234321”或者“123c321”，
返回长度 7
* */
public class Code01_PalindromeSubsequence {
    //1、一个字符串及其逆序串的最长公共子序列，就是原始字符串的最长回文子序列

    public static int lpsl1(String s) {
        if (s == null || s.length() == 0) {
            return 0;
        }
        char[] str = s.toCharArray();
        return f(str, 0, str.length - 1);
    }

    //str[L,R]最长回文子序列长度
    public static int f(char[] str,int l, int r) {
        //base case
        if (l == r) return 1;
        if ( l == r - 1){ //两个字符的情况
            return str[l] == str[r] ? 2 : 1;
        }
        //1、l,r不包含
        int p1 = f(str, l+1, r-1);
        //2、l包含，不包含r
        int p2 = f(str, l, r-1);
        //3、l不包含，r包含
        int p3 = f(str, l+1, r);
        //4、l，r包含
        int p4 =str[l] != str[r] ? 0 : ( 2 + f(str, l+1, r-1));
        return Math.max(Math.max(p1,p2),Math.max(p3,p4));
    }

    public static int lpsl2(String s) {
        if (s == null || s.length() == 0) {
            return 0;
        }
        char[] str = s.toCharArray();
        int N = str.length;
        //L 0....N-1  行
        //R 0....N-1  列
        int[][] dp = new int[N][N];
        //先填对角线的最后一个格子，方便下面填对角线及相邻对角线
        dp[N-1][N-1] = 1;
        for(int i = 0; i < N-1; i ++) {
            dp[i][i] = 1;
            //两个字符的情况str[l] == str[r-1]
            dp[i][i+1] = str[i] == str[i+1] ? 2 : 1;
        }
        //上三角按照从下到上，从左向右顺序填，看f的几个依赖关系可以得到
        //从N-3行填
        for(int L = N-1; L >=0; L--) {
            for(int R = L + 2; R < N; R++) {
                //1、l,r不包含
                int p1 = dp[L+1][R-1];
                //2、l包含，不包含r
                int p2 = dp[L][R-1];
                //3、l不包含，r包含
                int p3 = dp[L+1][R];
                //4、l，r包含
                int p4 =str[L] != str[R] ? 0 : ( 2 + dp[L+1][R-1]);
                dp[L][R] = Math.max(Math.max(p1,p2),Math.max(p3,p4));
            }
        }
        return dp[0][N-1];
    }

    public static int lpsl3(String s) {
        if (s == null || s.length() == 0) {
            return 0;
        }
        char[] str = s.toCharArray();
        int N = str.length;
        //L 0....N-1  行
        //R 0....N-1  列
        int[][] dp = new int[N][N];
        //先填对角线的最后一个格子，方便下面填对角线及相邻对角线
        dp[N-1][N-1] = 1;
        for(int i = 0; i < N-1; i ++) {
            dp[i][i] = 1;
            //两个字符的情况str[l] == str[r-1]
            dp[i][i+1] = str[i] == str[i+1] ? 2 : 1;
        }
        //上三角按照从下到上，从左向右顺序填，看f的几个依赖关系可以得到
        //从N-3行填
        for(int L = N-1; L >=0; L--) {
            for(int R = L + 2; R < N; R++) {
                //dp[L][R]的结果是取决于左边，左下，下面三个位置
                // 可以发现，左边位置又依赖于左边位置的左边，左下，下面三个位置，则对于dp[L][R]左下可以不考虑，左边包含了
                dp[L][R] = Math.max(dp[L][R-1],dp[L+1][R]); //求左边和下面
                if (str[L] == str[R]) {  //可能性4存在
                    dp[L][R] = Math.max(dp[L][R], 2 + dp[L+1][R-1]);
                }
            }
        }
        return dp[0][N-1];
    }

}
```

#### 跳马问题

**题目：**

```
请同学们自行搜索或者想象一个象棋的棋盘
然后把整个棋盘放入第一象限，棋盘的最左下角是(O,0)位置
那么整个棋盘就是横坐标上9条线、纵坐标上10条线的区域
给你三个参数 x,y,k
返回“马”从(O,0)位置出发，必须走k步
最后落在（x,y）上的方法数有多少种？
```

**解答：**

```java
package class21;

/*
请同学们自行搜索或者想象一个象棋的棋盘
然后把整个棋盘放入第一象限，棋盘的最左下角是(O,0)位置
那么整个棋盘就是横坐标上9条线、纵坐标上10条线的区域
给你三个参数 x,y,k
返回“马”从(O,0)位置出发，必须走k步
最后落在（x,y）上的方法数有多少种？
* */
public class Code02_HorseJump {

    //当前来到的位置是(x,y)
    //还剩余rest步需要跳
    //跳完rest步，正好跳到了(a,b)的方法数是多少？
    public static int jump(int a, int b, int k){
        return process(0, 0, k, a, b);
    }

    public static int process (int x, int y, int rest, int a, int b) {
        //越界了
        if(x < 0 || x > 9 || y < 0 || y > 8) {
            return 0;
        }
        //base case
        if (rest == 0){
            return (x == a && y == b) ? 1 : 0;
        }
        int ways = process(x + 2, y + 1, rest - 1, a, b);
        ways += process(x + 1, y + 2, rest - 1, a, b);
        ways += process(x - 1, y + 2, rest - 1, a, b);
        ways += process(x - 2, y + 1, rest - 1, a, b);
        ways += process(x - 2, y - 1, rest - 1, a, b);
        ways += process(x - 1, y - 2, rest - 1, a, b);
        ways += process(x + 1, y - 2, rest - 1, a, b);
        ways += process(x + 2, y - 1, rest - 1, a, b);
        return ways;
    }

    public static int check(int[][][] dp, int x, int y, int rest) {
        if(x < 0 || x > 9 || y < 0 || y > 8) return 0;
        return dp[x][y][rest];
    }

    //当前来到的位置是(x,y)
    //还剩余rest步需要跳
    //跳完rest步，正好跳到了(a,b)的方法数是多少？
    public static int dp(int a, int b, int k){
        int[][][] dp = new int[10][9][k+1];
        dp[a][b][0] = 1;
        for(int rest = 1;rest <= k;rest++){
            for(int x = 0; x < 10; x++){
                for(int y = 0; y < 9; y++){
                    int ways = check(dp ,x + 2, y + 1, rest - 1);
                    ways += check(dp,x + 1, y + 2, rest - 1);
                    ways += check(dp,x - 1, y + 2, rest - 1);
                    ways += check(dp,x - 2, y + 1, rest - 1);
                    ways += check(dp,x - 2, y - 1, rest - 1);
                    ways += check(dp,x - 1, y - 2, rest - 1);
                    ways += check(dp,x + 1, y - 2, rest - 1);
                    ways += check(dp,x + 2, y - 1, rest - 1);
                    dp[x][y][rest] = ways;
                }
            }
        }
        return dp[0][0][k];
    }

    public static void main(String[] args) {
        int x = 7;
        int y = 7;
        int k = 10;
        System.out.println(jump(x, y, k));
        System.out.println(dp(7,7,10));
    }
}
```



#### （6）业务限制题型

#### 咖啡问题-京东面试

**题目：**

```
给定一个数组arr,arr[i]代表第i号咖啡机泡一杯咖啡的时间
给定一个正数N,表示N个人等着咖啡机泡咖啡，每台咖啡机只能轮流泡咖啡
只有一台咖啡机，一次只能洗一个杯子，时间耗费a,洗完才能洗下一杯
每个咖啡杯也可以自己挥发干净，时间耗费b,咖啡杯可以并行挥发
假设所有人拿到咖啡之后立刻喝干净，
返回从开始等到所有咖啡机变干净的最短时间
三个参数：int[] arr、int N,int a、int b
```

**解答：**

```java
package class21;

import java.util.Arrays;
import java.util.Comparator;
import java.util.PriorityQueue;

/*
给定一个数组arr,arr[i]代表第i号咖啡机泡一杯咖啡的时间
给定一个正数N,表示N个人等着咖啡机泡咖啡，每台咖啡机只能轮流泡咖啡
只有一台咖啡机，一次只能洗一个杯子，时间耗费a,洗完才能洗下一杯
每个咖啡杯也可以自己挥发干净，时间耗费b,咖啡杯可以并行挥发
假设所有人拿到咖啡之后立刻喝干净，
返回从开始等到所有咖啡机变干净的最短时间
三个参数：int[] arr、int N,int a、int b
 */
public class Code03_Coffee {
    /*
    * 小根堆{再用，多久}
    * 使用再用+多久来排序
    * arr={3， 1， 7}
    * (0,1)、(0,3)、(0,7) 弹出(0,1),则服务完之后立马喝完，喝完后能继续使用咖啡机时间是(1,1)入堆
    * (1,1)、(0,3)、(0,7) 弹出(1,1),则服务完之后立马喝完，喝完后能继续使用咖啡机时间是(2,1)入堆
    * (2,1)、(0,3)、(0,7) 弹出(2,1),则服务完之后立马喝完，喝完后能继续使用咖啡机时间是(3,1)入堆
    * (0,3)、(3,1)、(0,7) 弹出(0,3),则服务完之后立马喝完，喝完后能继续使用咖啡机时间是(3,3)入堆
    * (3,1)、(3,3)、(0,7) 弹出(3,1),则服务完之后立马喝完，喝完后能继续使用咖啡机时间是(4,1)入堆
    * (4,1)、(3,3)、(0,7) ......
    * */

    public static class Machine {
        public int timePoint; //再次使用的时间点
        public int workTime;  //工作一次的时间

        public Machine (int t, int w) {
            timePoint = t;
            workTime = w;
        }
    }

    //排序
    public static  class MachineComparator implements Comparator<Machine> {
        @Override
        public int compare(Machine o1, Machine o2) {
            return (o1.timePoint + o1.workTime) - (o2.timePoint + o2.workTime);
        }
    }
    /**
     *
     * @param drinks :所有杯子开始洗的时间
     * @param wash ：单杯洗干净的时间（串行）
     * @param air ：挥发干净的时间（并行）
     * @param index ： 当前位置
     * @param free ： 洗的机器什么时候可用
     * @return  drinks[index ...]都变干净，最早的结束时间
     */
    public static int bestTime(int[] drinks, int wash, int air, int index, int free) {
        //没有杯子了，返回剩余0个杯子需要的时间就是0
        if(index == drinks.length) return 0;

        //index号杯子  决定洗的时间：是由喝完的时间和咖啡机开始能使用的时间决定的，求最大值
        int selfClean1 = Math.max(drinks[index], free) + wash;  //洗自己杯子的时间点
        int restClean1 = bestTime(drinks, wash, air, index + 1, selfClean1);  //剩余杯子变干净的时间点
        int p1 = Math.max(selfClean1, restClean1);

        //index号杯子 决定挥发的时间,此时没有使用洗咖啡机
        int selfClean2 = drinks[index] + air;
        int restClean2 = bestTime(drinks, wash, air, index + 1, free);
        int p2 = Math.max(selfClean2, restClean2);

        return Math.min(p1, p2);
    }

    //方法2：每个人暴力尝试用每个咖啡机给自己做咖啡，优化成贪心
    public static int minTime1(int[] arr, int n, int a, int b) {
        PriorityQueue<Machine> heap = new PriorityQueue<Machine>(new MachineComparator());
        for (int i = 0; i < arr.length; i++) {
            heap.add(new Machine(0, arr[i]));
        }
        //n个人的喝完时间
        int[] drinks = new int[n];
        for (int i = 0; i < n; i++) {
            Machine cur = heap.poll();
            cur.timePoint += cur.workTime;
            drinks[i] = cur.timePoint;
            heap.add(cur);
        }
        return bestTime(drinks, a, b,0,0);
    }

    //方法2：每个人暴力尝试用每个咖啡机给自己做咖啡，优化成贪心
    public static int minTime2(int[] arr, int n, int a, int b) {
        PriorityQueue<Machine> heap = new PriorityQueue<Machine>(new MachineComparator());
        for (int i = 0; i < arr.length; i++) {
            heap.add(new Machine(0, arr[i]));
        }
        //n个人的喝完时间
        int[] drinks = new int[n];
        for (int i = 0; i < n; i++) {
            Machine cur = heap.poll();
            cur.timePoint += cur.workTime;
            drinks[i] = cur.timePoint;
            heap.add(cur);
        }
        return bestTimeDp(drinks, a, b);
    }

    public static int bestTimeDp(int[] drinks, int wash, int air) {
        int N = drinks.length;
        //目前最早能开始洗的时间
        int maxFree = 0;
        for (int i = 0; i < drinks.length; i++) {
            //1 4 20 45      wash=3
            //1+3 = 4        maxFree = 1 -->更新为1+3=4
            //4+3 = 7        maxFree = 4 -->更新为4+3=7
            //20+3=23        maxFree = 7 -->更新为20+3=23
            maxFree = Math.max(maxFree, drinks[i]) + wash;
        }

        int[][] dp = new int[N+1][maxFree + 1];
        //dp[N][...] = 0;
        for (int index = N -1; index >=0; index--) {
            for(int free = 0; free <=maxFree; free++){
                //index号杯子  决定洗的时间：是由喝完的时间和咖啡机开始能使用的时间决定的，求最大值
                int selfClean1 = Math.max(drinks[index], free) + wash;  //洗自己杯子的时间点
                //越界检查
                if (selfClean1 > maxFree) continue;
                //dp[index + 1][selfClean1],这里selfClean1可能越界
                int restClean1 = dp[index + 1][selfClean1];   //剩余杯子变干净的时间点
                int p1 = Math.max(selfClean1, restClean1);

                //index号杯子 决定挥发的时间,此时没有使用洗咖啡机
                int selfClean2 = drinks[index] + air;
                int restClean2 = dp[index + 1][free];
                int p2 = Math.max(selfClean2, restClean2);
                dp[index][free] = Math.min(p1, p2);
            }
        }

        //从0开始，咖啡机从0开始
        return dp[0][0];
    }


    // 验证的方法
    // 彻底的暴力
    // 很慢但是绝对正确
    public static int right(int[] arr, int n, int a, int b) {
        int[] times = new int[arr.length];
        int[] drink = new int[n];
        return forceMake(arr, times, 0, drink, n, a, b);
    }

    // 每个人暴力尝试用每一个咖啡机给自己做咖啡
    public static int forceMake(int[] arr, int[] times, int kth, int[] drink, int n, int a, int b) {
        if (kth == n) {
            int[] drinkSorted = Arrays.copyOf(drink, kth);
            Arrays.sort(drinkSorted);
            return forceWash(drinkSorted, a, b, 0, 0, 0);
        }
        int time = Integer.MAX_VALUE;
        for (int i = 0; i < arr.length; i++) {
            int work = arr[i];
            int pre = times[i];
            drink[kth] = pre + work;
            times[i] = pre + work;
            time = Math.min(time, forceMake(arr, times, kth + 1, drink, n, a, b));
            drink[kth] = 0;
            times[i] = pre;
        }
        return time;
    }

    public static int forceWash(int[] drinks, int a, int b, int index, int washLine, int time) {
        if (index == drinks.length) {
            return time;
        }
        // 选择一：当前index号咖啡杯，选择用洗咖啡机刷干净
        int wash = Math.max(drinks[index], washLine) + a;
        int ans1 = forceWash(drinks, a, b, index + 1, wash, Math.max(wash, time));

        // 选择二：当前index号咖啡杯，选择自然挥发
        int dry = drinks[index] + b;
        int ans2 = forceWash(drinks, a, b, index + 1, washLine, Math.max(dry, time));
        return Math.min(ans1, ans2);
    }

    // for test
    public static int[] randomArray(int len, int max) {
        int[] arr = new int[len];
        for (int i = 0; i < len; i++) {
            arr[i] = (int) (Math.random() * max) + 1;
        }
        return arr;
    }

    // for test
    public static void printArray(int[] arr) {
        System.out.print("arr : ");
        for (int j = 0; j < arr.length; j++) {
            System.out.print(arr[j] + ", ");
        }
        System.out.println();
    }

    public static void main(String[] args) {
        int len = 10;
        int max = 10;
        int testTime = 10;
        System.out.println("测试开始");
        for (int i = 0; i < testTime; i++) {
            int[] arr = randomArray(len, max);
            int n = (int) (Math.random() * 7) + 1;
            int a = (int) (Math.random() * 7) + 1;
            int b = (int) (Math.random() * 10) + 1;
            int ans1 = right(arr, n, a, b);
            int ans2 = minTime1(arr, n, a, b);
            int ans3 = minTime2(arr, n, a, b);
            if (ans1 != ans2 || ans2 != ans3) {
                printArray(arr);
                System.out.println("n : " + n);
                System.out.println("a : " + a);
                System.out.println("b : " + b);
                System.out.println(ans1 + " , " + ans2 + " , " + ans3);
                System.out.println("===============");
                break;
            }
        }
        System.out.println("测试结束");
    }
}
```

#### （7）空间压缩技巧-滚动数组

#### 	最小路径和问题

**题目：**

```
给定一个二维数组matrⅸ，一个人必须从左上角出发，最后到达右下角。
沿途只可以向下或者向右走，沿途的数字都累加就是距离累加和。
返回最小距离累加和
```

**解答:**

```java
package class21;

/**
 * 数组压缩技巧(滚动数组)：
 * （1）只依赖左上角和上方（从右向左更新，从下向上更新）---如果从左向右，从上到下的话，第一行数据没法得到更新
 * （2）只依赖左边和上方（从左向右，从上到下更新）---第一行的第一个位置可以当作左边，推导得到第一行
 * （3）只依赖左边，左上角，上方（从左向右，从上到下）
 * （4）N 行 M列的数组，4行100000000列的数组，则准备长度为4的数组

 给定一个二维数组matrⅸ，一个人必须从左上角出发，最后到达右下角。
 沿途只可以向下或者向右走，沿途的数字都累加就是距离累加和。
 返回最小距离累加和

 解答：
 定义dp[i][j]表示从(i,j)位置到达最下面的(n,m)的最小的值，则答案就是返回dp[0][0];
 */
public class Code01_MinPathSum {

	public static int minPathSum1(int[][] m) {
		if (m == null || m.length == 0 || m[0] == null || m[0].length == 0) {
			return 0;
		}
		int row = m.length;
		int col = m[0].length;
		int[][] dp = new int[row][col];
		dp[0][0] = m[0][0];
		for (int i = 1; i < row; i++) {
			dp[i][0] = dp[i - 1][0] + m[i][0];
		}
		for (int j = 1; j < col; j++) {
			dp[0][j] = dp[0][j - 1] + m[0][j];
		}
		for (int i = 1; i < row; i++) {
			for (int j = 1; j < col; j++) {
				dp[i][j] = Math.min(dp[i - 1][j], dp[i][j - 1]) + m[i][j];
			}
		}
		return dp[row - 1][col - 1];
	}

	public static int minPathSum2(int[][] m) {
		if (m == null || m.length == 0 || m[0] == null || m[0].length == 0) {
			return 0;
		}
		int row = m.length;
		int col = m[0].length;
		int[] dp = new int[col];
		dp[0] = m[0][0];
		//第一行的值
		for (int j = 1; j < col; j++) {
			dp[j] = dp[j - 1] + m[0][j];
		}
		for (int i = 1; i < row; i++) {
			//每行的第一个位置，只取决于上一行的第一个位置的值
			dp[0] += m[i][0];
			for (int j = 1; j < col; j++) {
				//此时dp[j]位置左边的dp[j-1]已经更新了,dp[j]还是上一行的值，没有更新
				dp[j] = Math.min(dp[j - 1], dp[j]) + m[i][j];
			}
		}
		return dp[col - 1];
	}

	// for test
	public static int[][] generateRandomMatrix(int rowSize, int colSize) {
		if (rowSize < 0 || colSize < 0) {
			return null;
		}
		int[][] result = new int[rowSize][colSize];
		for (int i = 0; i != result.length; i++) {
			for (int j = 0; j != result[0].length; j++) {
				result[i][j] = (int) (Math.random() * 100);
			}
		}
		return result;
	}

	// for test
	public static void printMatrix(int[][] matrix) {
		for (int i = 0; i != matrix.length; i++) {
			for (int j = 0; j != matrix[0].length; j++) {
				System.out.print(matrix[i][j] + " ");
			}
			System.out.println();
		}
	}

	public static void main(String[] args) {
		int rowSize = 10;
		int colSize = 10;
		int[][] m = generateRandomMatrix(rowSize, colSize);
		System.out.println(minPathSum1(m));
		System.out.println(minPathSum2(m));

	}
}
```

#### 货币问题1（货币不重复，数量有限）

**题目：**

```
arr是货币数组，其中的值都是正数。再给定一个正数aim。
每个值都认为是一张货币，
即便是值相同的货币也认为每一张都是不同的，
返回组成aim的方法数
例如：arr={1,1,1},aim=2
第0个和第1个能组成2，第1个和第2个能组成2，第0个和第2个能组成2
一共就3种方法，所以返回3
```

**解答：**

```java
package class22;

/**
 * arr是货币数组，其中的值都是正数。再给定一个正数aim。
 * 每个值都认为是一张货币，
 * 即便是值相同的货币也认为每一张都是不同的，
 * 返回组成aim的方法数
 * 例如：arr={1,1,1},aim=2
 * 第0个和第1个能组成2，第1个和第2个能组成2，第0个和第2个能组成2
 * 一共就3种方法，所以返回3
 */
public class Code02_CoinsWayEveryPaperDifferent {

	public static int coinWays(int[] arr, int aim) {
		return process(arr, 0, aim);
	}

	// arr[index....] 组成正好rest这么多的钱，有几种方法
	public static int process(int[] arr, int index, int rest) {
		if (rest < 0) {
			return 0;
		}
		if (index == arr.length) { // 没钱了！
			return rest == 0 ? 1 : 0;
		} else {
			return process(arr, index + 1, rest) + process(arr, index + 1, rest - arr[index]);
		}
	}

	public static int dp(int[] arr, int aim) {
		if (aim == 0) {
			return 1;
		}
		int N = arr.length;
		int[][] dp = new int[N + 1][aim + 1];
		dp[N][0] = 1;
		//从N-1行开始递推
		for (int index = N - 1; index >= 0; index--) {
			for (int rest = 0; rest <= aim; rest++) {
				dp[index][rest] = dp[index + 1][rest] + (rest - arr[index] >= 0 ? dp[index + 1][rest - arr[index]] : 0);
			}
		}
		return dp[0][aim];
	}

	// 为了测试
	public static int[] randomArray(int maxLen, int maxValue) {
		int N = (int) (Math.random() * maxLen);
		int[] arr = new int[N];
		for (int i = 0; i < N; i++) {
			arr[i] = (int) (Math.random() * maxValue) + 1;
		}
		return arr;
	}

	// 为了测试
	public static void printArray(int[] arr) {
		for (int i = 0; i < arr.length; i++) {
			System.out.print(arr[i] + " ");
		}
		System.out.println();
	}

	// 为了测试
	public static void main(String[] args) {
		int maxLen = 20;
		int maxValue = 30;
		int testTime = 1000000;
		System.out.println("测试开始");
		for (int i = 0; i < testTime; i++) {
			int[] arr = randomArray(maxLen, maxValue);
			int aim = (int) (Math.random() * maxValue);
			int ans1 = coinWays(arr, aim);
			int ans2 = dp(arr, aim);
			if (ans1 != ans2) {
				System.out.println("Oops!");
				printArray(arr);
				System.out.println(aim);
				System.out.println(ans1);
				System.out.println(ans2);
				break;
			}
		}
		System.out.println("测试结束");
	}

}
```

#### 货币问题2（货币不重复，数量无限）

**题目：**

```
 arr是面值数组，其中的值都是正数且没有重复。再给定一个正数aim。
 每个值都认为是一种面值，且认为张数是无限的。
 返回组成aim的方法数
 例如：arr={1,2},aim=4
 方法如下：1+1+1+1、1+1+2、2：2
 一共就3种方法，所以返回3
```

**解答：**

```java
package class22;

/**
 * arr是面值数组，其中的值都是正数且没有重复。再给定一个正数aim。
 * 每个值都认为是一种面值，且认为张数是无限的。
 * 返回组成aim的方法数
 * 例如：arr={1,2},aim=4
 * 方法如下：1+1+1+1、1+1+2、2：2
 * 一共就3种方法，所以返回3
 */
public class Code03_CoinsWayNoLimit {

	public static int coinsWay(int[] arr, int aim) {
		if (arr == null || arr.length == 0 || aim < 0) {
			return 0;
		}
		return process(arr, 0, aim);
	}

	// arr[index....] 所有的面值，每一个面值都可以任意选择张数，组成正好rest这么多钱，方法数多少？
	public static int process(int[] arr, int index, int rest) {
		if (index == arr.length) { // 没钱了
			return rest == 0 ? 1 : 0;
		}
		int ways = 0;
		for (int zhang = 0; zhang * arr[index] <= rest; zhang++) {
			ways += process(arr, index + 1, rest - (zhang * arr[index]));
		}
		return ways;
	}

	public static int dp1(int[] arr, int aim) {
		if (arr == null || arr.length == 0 || aim < 0) {
			return 0;
		}
		int N = arr.length;
		int[][] dp = new int[N + 1][aim + 1];
		dp[N][0] = 1;
		for (int index = N - 1; index >= 0; index--) {
			for (int rest = 0; rest <= aim; rest++) {
				int ways = 0;
				for (int zhang = 0; zhang * arr[index] <= rest; zhang++) {
					ways += dp[index + 1][rest - (zhang * arr[index])];
				}
				dp[index][rest] = ways;
			}
		}
		return dp[0][aim];
	}

	public static int dp2(int[] arr, int aim) {
		if (arr == null || arr.length == 0 || aim < 0) {
			return 0;
		}
		int N = arr.length;
		int[][] dp = new int[N + 1][aim + 1];
		dp[N][0] = 1;
		for (int index = N - 1; index >= 0; index--) {
			for (int rest = 0; rest <= aim; rest++) {
				//通过表观察，index行的依赖index + 1行的数
				//也就是dp[index][rest] = dp[index+1][rest] + dp[index][rest-arr[index]];
				//dp[index][rest-2 * arr[index]]         dp[index][rest-arr[index]]              <------ dp[index][rest]
				//																								|
				//dp[index+1][rest-2 * arr[index]]   dp[index+1][rest- 1 * arr[index]]     dp[index+1][rest- 0 * arr[index]]=dp[index][rest]                                    dp[index+1][rest]
				dp[index][rest] = dp[index + 1][rest];
				if (rest - arr[index] >= 0) {
					dp[index][rest] += dp[index][rest - arr[index]];
				}
			}
		}
		return dp[0][aim];
	}

	// 为了测试
	public static int[] randomArray(int maxLen, int maxValue) {
		int N = (int) (Math.random() * maxLen);
		int[] arr = new int[N];
		boolean[] has = new boolean[maxValue + 1];
		for (int i = 0; i < N; i++) {
			do {
				arr[i] = (int) (Math.random() * maxValue) + 1;
			} while (has[arr[i]]);
			has[arr[i]] = true;
		}
		return arr;
	}

	// 为了测试
	public static void printArray(int[] arr) {
		for (int i = 0; i < arr.length; i++) {
			System.out.print(arr[i] + " ");
		}
		System.out.println();
	}

	// 为了测试
	public static void main(String[] args) {
		int maxLen = 10;
		int maxValue = 30;
		int testTime = 1000000;
		System.out.println("测试开始");
		for (int i = 0; i < testTime; i++) {
			int[] arr = randomArray(maxLen, maxValue);
			int aim = (int) (Math.random() * maxValue);
			int ans1 = coinsWay(arr, aim);
			int ans2 = dp1(arr, aim);
			int ans3 = dp2(arr, aim);
			if (ans1 != ans2 || ans1 != ans3) {
				System.out.println("Oops!");
				printArray(arr);
				System.out.println(aim);
				System.out.println(ans1);
				System.out.println(ans2);
				System.out.println(ans3);
				break;
			}
		}
		System.out.println("测试结束");
	}
}
```

#### 货币问题3（货币重复，数量有限）

**题目：**

```
arr是货币数组，其中的值都是正数。再给定一个正数aim。
每个值都认为是一张货币，
认为值相同的货币没有任何不同，
返回组成aim的方法数
例如：arr={1,2,1,1,2,1,2},aim=4
方法：1+1+1+1、1+1+2、2+2
一共就3种方法，所以返回3
```

**解答：**

```java
package class22;

import java.util.HashMap;
import java.util.Map.Entry;

/*
arr是货币数组，其中的值都是正数。再给定一个正数aim。
每个值都认为是一张货币，
认为值相同的货币没有任何不同，
返回组成aim的方法数
例如：arr={1,2,1,1,2,1,2,aim=4
方法：1+1+1+1、1+1+2、2+2
一共就3种方法，所以返回3
* */
public class Code04_CoinsWaySameValueSamePapper {

	public static class Info {
		public int[] coins;
		public int[] zhangs;

		public Info(int[] c, int[] z) {
			coins = c;
			zhangs = z;
		}
	}

	public static Info getInfo(int[] arr) {
		HashMap<Integer, Integer> counts = new HashMap<>();
		for (int value : arr) {
			if (!counts.containsKey(value)) {
				counts.put(value, 1);
			} else {
				counts.put(value, counts.get(value) + 1);
			}
		}
		int N = counts.size();
		int[] coins = new int[N];
		int[] zhangs = new int[N];
		int index = 0;
		for (Entry<Integer, Integer> entry : counts.entrySet()) {
			coins[index] = entry.getKey();
			zhangs[index++] = entry.getValue();
		}
		return new Info(coins, zhangs);
	}

	public static int coinsWay(int[] arr, int aim) {
		if (arr == null || arr.length == 0 || aim < 0) {
			return 0;
		}
		Info info = getInfo(arr);
		return process(info.coins, info.zhangs, 0, aim);
	}

	// coins 面值数组，正数且去重
	// zhangs 每种面值对应的张数
	//[2, 5, 10]
	//[4, 6, 3]
	public static int process(int[] coins, int[] zhangs, int index, int rest) {
		if (index == coins.length) {
			return rest == 0 ? 1 : 0;
		}
		int ways = 0;
		for (int zhang = 0; zhang * coins[index] <= rest && zhang <= zhangs[index]; zhang++) {
			ways += process(coins, zhangs, index + 1, rest - (zhang * coins[index]));
		}
		return ways;
	}

	public static int dp1(int[] arr, int aim) {
		if (arr == null || arr.length == 0 || aim < 0) {
			return 0;
		}
		Info info = getInfo(arr);
		int[] coins = info.coins;
		int[] zhangs = info.zhangs;
		int N = coins.length;
		int[][] dp = new int[N + 1][aim + 1];
		//按照上面的递归版本填充递归边界,由于java初始化的时候默认为0,则dp[N][！0] = 0；
		dp[N][0] = 1;
		//从N - 1行开始
		for (int index = N - 1; index >= 0; index--) {
			//列
			for (int rest = 0; rest <= aim; rest++) {
				//做记忆化搜索
				int ways = 0;
				//两个限制条件,rest还没使用完以及张数还够
				for (int zhang = 0; zhang * coins[index] <= rest && zhang <= zhangs[index]; zhang++) {
					//依赖关系
					ways += dp[index + 1][rest - (zhang * coins[index])];
				}
				dp[index][rest] = ways;
			}
		}
		return dp[0][aim];
	}

	public static int dp2(int[] arr, int aim) {
		if (arr == null || arr.length == 0 || aim < 0) {
			return 0;
		}
		Info info = getInfo(arr);
		int[] coins = info.coins;
		int[] zhangs = info.zhangs;
		int N = coins.length;
		int[][] dp = new int[N + 1][aim + 1];
		dp[N][0] = 1;
		for (int index = N - 1; index >= 0; index--) {
			for (int rest = 0; rest <= aim; rest++) {
				//先获取index下面的一行对应的位置
				dp[index][rest] = dp[index + 1][rest];
				//index左边位置
				if (rest - coins[index] >= 0) {
					dp[index][rest] += dp[index][rest - coins[index]];
				}
				//减去index左边位置多加的数值
				if (rest - coins[index] * (zhangs[index] + 1) >= 0) {
					dp[index][rest] -= dp[index + 1][rest - coins[index] * (zhangs[index] + 1)];
				}
			}
		}
		return dp[0][aim];
	}

	// 为了测试
	public static int[] randomArray(int maxLen, int maxValue) {
		int N = (int) (Math.random() * maxLen);
		int[] arr = new int[N];
		for (int i = 0; i < N; i++) {
			arr[i] = (int) (Math.random() * maxValue) + 1;
		}
		return arr;
	}

	// 为了测试
	public static void printArray(int[] arr) {
		for (int i = 0; i < arr.length; i++) {
			System.out.print(arr[i] + " ");
		}
		System.out.println();
	}

	// 为了测试
	public static void main(String[] args) {
		int maxLen = 10;
		int maxValue = 20;
		int testTime = 1000000;
		System.out.println("测试开始");
		for (int i = 0; i < testTime; i++) {
			int[] arr = randomArray(maxLen, maxValue);
			int aim = (int) (Math.random() * maxValue);
			int ans1 = coinsWay(arr, aim);
			int ans2 = dp1(arr, aim);
			int ans3 = dp2(arr, aim);
			if (ans1 != ans2 || ans1 != ans3) {
				System.out.println("Oops!");
				printArray(arr);
				System.out.println(aim);
				System.out.println(ans1);
				System.out.println(ans2);
				System.out.println(ans3);
				break;
			}
		}
		System.out.println("测试结束");
	}

}
```

#### 马走日类型问题

**题目：**

```
给定5个参数，N,M,row,col,k
表示在N*M的区域上，醉汉Bob初始在(row,col)位置
Bob一共要迈出k步，且每步都会等概率向上下左右四个方向走一个单位
任何时候Bob只要离开N*M的区域，就直接死亡
返回k步之后，Bob还在N*M的区域的概率
```

**解答：**

```java
package class22;

/*
给定5个参数，N,M,row,col,k
表示在N*M的区域上，醉汉Bob初始在(row,col)位置
Bob一共要迈出k步，且每步都会等概率向上下左右四个方向走一个单位
任何时候Bob只要离开N*M的区域，就直接死亡
返回k步之后，Bob还在N*M的区域的概率
* */
public class Code05_BobDie {

	public static double livePosibility1(int row, int col, int k, int N, int M) {
		return (double) process(row, col, k, N, M) / Math.pow(4, k);
	}

	// 目前在row，col位置，还有rest步要走，走完了如果还在棋盘中就获得1个生存点，返回总的生存点数
	public static long process(int row, int col, int rest, int N, int M) {
		//越界了
		if (row < 0 || row == N || col < 0 || col == M) {
			return 0;
		}
		// 还在棋盘中！
		if (rest == 0) {
			return 1;
		}
		// 还在棋盘中！还有步数要走
		int ans = 0;
		int[] dx = {-1,0,1,0};
		int[] dy = {0,1,0,-1};
		for(int i = 0; i < 4; i++) {
			int a = row + dx[i], b = col + dy[i];
			ans += process(a, b, rest - 1, N, M);
		}
		return ans;
//		long up = process(row - 1, col, rest - 1, N, M);
//		long down = process(row + 1, col, rest - 1, N, M);
//		long left = process(row, col - 1, rest - 1, N, M);
//		long right = process(row, col + 1, rest - 1, N, M);
//		return up + down + left + right;
	}

	public static double livePosibility2(int row, int col, int k, int N, int M) {
		long[][][] dp = new long[N][M][k + 1];
		for (int i = 0; i < N; i++) {
			for (int j = 0; j < M; j++) {
				dp[i][j][0] = 1;
			}
		}
		for (int rest = 1; rest <= k; rest++) {
			for (int r = 0; r < N; r++) {
				for (int c = 0; c < M; c++) {
					dp[r][c][rest] = pick(dp, N, M, r - 1, c, rest - 1);
					dp[r][c][rest] += pick(dp, N, M, r + 1, c, rest - 1);
					dp[r][c][rest] += pick(dp, N, M, r, c - 1, rest - 1);
					dp[r][c][rest] += pick(dp, N, M, r, c + 1, rest - 1);
				}
			}
		}
		return (double) dp[row][col][k] / Math.pow(4, k);
	}

	public static long pick(long[][][] dp, int N, int M, int r, int c, int rest) {
		if (r < 0 || r == N || c < 0 || c == M) {
			return 0;
		}
		return dp[r][c][rest];
	}

	public static void main(String[] args) {
		System.out.println(livePosibility1(6, 6, 10, 50, 50));
		System.out.println(livePosibility2(6, 6, 10, 50, 50));
	}

}
```

#### （8）斜率优化

#### 字节面试题目-样本对应模型

**题目：**

```
字节跳动面试题目:
给定3个参数，N,M,K
怪兽有 N 滴血，等着英雄来砍自己
英雄每一次打击，都会让怪兽流失[0~M]的血量
到底流失多少？每一次在[O~M]上等概率的获得一个值
求 K 次打击之后，英雄把怪兽砍死的概率

时间复杂度(M+1)^K
一直展开不剪枝，收集所有的点数
```

**解答：**

```java
package class23;

/*
字节跳动面试题目:
给定3个参数，N,M,K
怪兽有 N 滴血，等着英雄来砍自己
英雄每一次打击，都会让怪兽流失[0~M]的血量
到底流失多少？每一次在[O~M]上等概率的获得一个值
求 K 次打击之后，英雄把怪兽砍死的概率

时间复杂度(M+1)^K
一直展开不剪枝，收集所有的点数
* */
public class Code01_KillMonster {

    public static double right1(int N, int M, int K) {
        if (N < 1 || M < 1 || K < 1) return 0;
        long all = (long) Math.pow(M + 1, K);
        long kill = process1(K, M, N);
        return (double) ((double) kill / (double) all);
    }

    /**
     * 两个变化的参数：N 和 K
     *
     * @param hp 怪兽还有hp点血
     * @param M 每次的伤害在[0, M]范围上
     * @param times 还有times次可以砍
     * @return 返回砍死的情况数
     */
    public static long process1(int times, int M, int hp) {
        if (times == 0) {
            return hp <= 0 ? 1 : 0;
        }
        //当刀数为负数的时候，还剩余的kills的点数
        if(hp <= 0) {
            return (long) Math.pow(M+1,times);
        }
        long ways = 0;
        for (int i = 0; i <= M; i++) {
            //每一行K都依赖于K-1行
            ways += process1(times - 1, M, hp - i);
        }
        return ways;
    }

    public static double dp1(int N, int M, int K) {
        if (N < 1 || M < 1 || K < 1) return 0;
        long all = (long) Math.pow(M + 1, K);
        //小标从0开始
        long[][] dp = new long[K+1][N+1];
        //特别注意：dp[0][j]既表示原含义，也表示M+1的 j 次方的值
        dp[0][0] = 1; //dp[0][非0] = 0；
        for(int times = 1; times <= K; times++) {
            //第0列 : 当刀数为负数的时候，还剩余的kills的点数
            dp[times][0] = (long) Math.pow(M+1,times);
            for(int hp =1; hp <= N; hp++) {
                long ways = 0;
                for (int i = 0; i <= M; i++) {
                    //每一行K都依赖于K-1行
                    if(hp - i >= 0) {
                        ways += dp[times - 1][hp - i];
                    }else {
                        //刀数为负数，则次数为times - 1
                        ways += (long) Math.pow(M+1,times-1);
                    }
                }
                dp[times][hp] = ways;
            }
        }
        long kill = dp[K][N];
        return (double) ((double) kill / (double) all);
    }

    public static double right2(int N, int M, int K) {
        if (N < 1 || M < 1 || K < 1) return 0;
        long all = (long) Math.pow(M + 1, K);
        long kill = process2(N, M, K);
        return (double) ((double) kill / (double) all);
    }

    /**
     * 两个变化的参数：N 和 K
     * @param N 怪兽还有N点血
     * @param M 每次的伤害在[0, M]范围上
     * @param K 还有K次可以砍
     * @return 返回砍死的情况数
     */
    public static long process2(int N, int M, int K) {
        if (K == 0) {
            return N <= 0 ? 1 : 0;
        }
        if (N <= 0) {
            return (long) (Math.pow(M + 1, K));
        }
        return process2(N, M, K - 1) + process2(N - 1, M, K) - process2(N - M - 1, M, K - 1);
    }

    public static double dp2(int N, int M, int K) {
        if (N < 1 || M < 1 || K < 1) {
            return 0;
        }
        long all = (long) Math.pow(M + 1, K);
        long[][] dp = new long[K + 1][N + 1];
        dp[0][0] = 1;
        for (int times = 1; times <= K; times++) {
            dp[times][0] = (long) Math.pow(M + 1, times);
            for (int hp = 1; hp <= N; hp++) {
                dp[times][hp] = dp[times][hp - 1] + dp[times - 1][hp];
                if (hp - 1 - M >= 0) {
                    dp[times][hp] -= dp[times - 1][hp - 1 - M];
                } else {
                    //如果为负数的时候也需要直接计算，不能直接返回
                    dp[times][hp] -= Math.pow(M + 1, times - 1);
                }
            }
        }
        long kill = dp[K][N];
        return (double) ((double) kill / (double) all);
    }


    public static void main(String[] args) {
        int NMax = 10;
        int MMax = 10;
        int KMax = 10;
        int testTime = 200;
        System.out.println("测试开始");
        for (int i = 0; i < testTime; i++) {
            int N = (int) (Math.random() * NMax);
            int M = (int) (Math.random() * MMax);
            int K = (int) (Math.random() * KMax);
            double ans1 = right1(N, M, K);
            double ans2 = dp1(N, M, K);
            double ans3 = dp2(N, M, K);
            if (ans1 != ans2 || ans1 != ans3) {
                System.out.println("Oops!");
                break;
            }
        }
        System.out.println("测试结束");
    }
}
```



#### 货币问题4（货币不重复，数量无线，数量最少）

**题目：**

```
 arr是面值数组，其中的值都是正数且没有重复。再给定一个正数aim。
 每个值都认为是一种面值，且认为张数是无限的。
 返回组成aim的最少货币数
```

**解答：**

```java
package class23;

/**
 * arr是面值数组，其中的值都是正数且没有重复。再给定一个正数aim。
 * 每个值都认为是一种面值，且认为张数是无限的。
 * 返回组成aim的最少货币数
 */
public class Code02_MinCoinsNoLimit {
    public static int minCoins(int[] arr, int aim) {
        return process(arr, 0, aim);
    }

    /**
     *
     * @param arr :arr[index]面值，每种面值张数自由选择
     * @param index ： 位置
     * @param rest : 凑出rest正好这么多钱，返回最小张数
     * @return
     */
    public static int process(int[] arr, int index, int rest) {
        if(rest < 0) {
            return Integer.MAX_VALUE;
        }
        if(index == arr.length) {
            //rest为0，位置没有了,则需要的答案为0
            return rest == 0 ? 0 : Integer.MAX_VALUE;
        } else {
            int ans = Integer.MAX_VALUE;
            for (int zhang = 0; zhang * arr[index] <= rest; zhang++) {
                int next = process(arr,index + 1, rest - zhang * arr[index]);
                if(next != Integer.MAX_VALUE) {
                    ans = Math.min(ans, next + zhang);
                }
            }
            return ans;
        }
    }

    public static int dp1(int[] arr,int aim) {
        if(aim == 0) {
            return 0;
        }
        int N = arr.length;
        //rest范围从0 ~ aim+1
        //index 从 0 ~ N+1
        int[][] dp = new int[N+1][aim +1];
        dp[N][0] = 0;
        for(int j = 1; j <= aim; j++) {
            dp[N][j] = Integer.MAX_VALUE;
        }
        for(int index = N - 1; index >= 0; index--) {
            for(int rest = 0; rest <= aim; rest++) {
                int ans = Integer.MAX_VALUE;
                for (int zhang = 0; zhang * arr[index] <= rest; zhang++) {
                    //next表示rest还需要多少的张数
                    int next = dp[index + 1][rest - zhang * arr[index]];
                    if(next != Integer.MAX_VALUE) {
                        //最后更新答案的时候需要加上zhang
                        ans = Math.min(ans, next + zhang);
                    }
                }
                dp[index][rest] = ans;
            }
        }
        return dp[0][aim];
    }

    public static int dp2(int[] arr,int aim) {
        if(aim == 0) {
            return 0;
        }
        int N = arr.length;
        //rest范围从0 ~ aim+1
        //index 从 0 ~ N+1
        int[][] dp = new int[N+1][aim +1];
        dp[N][0] = 0;
        for(int j = 1; j <= aim; j++) {
            dp[N][j] = Integer.MAX_VALUE;
        }
        for(int index = N - 1; index >= 0; index--) {
            for(int rest = 0; rest <= aim; rest++) {
                //rest下面的格子
                dp[index][rest] = dp[index+1][rest];
                //rest - arr[index] >= 0不越界
                //并且对应的位置的数值是有效值
                if(rest - arr[index] >= 0 && dp[index][rest - arr[index]] != Integer.MAX_VALUE) {
                    dp[index][rest] = Math.min(dp[index][rest],dp[index][rest - arr[index]]+1);
                }
            }
        }
        return dp[0][aim];
    }

    // 为了测试
    public static int[] randomArray(int maxLen, int maxValue) {
        int N = (int) (Math.random() * maxLen);
        int[] arr = new int[N];
        boolean[] has = new boolean[maxValue + 1];
        for (int i = 0; i < N; i++) {
            do {
                arr[i] = (int) (Math.random() * maxValue) + 1;
            } while (has[arr[i]]);
            has[arr[i]] = true;
        }
        return arr;
    }

    // 为了测试
    public static void printArray(int[] arr) {
        for (int i = 0; i < arr.length; i++) {
            System.out.print(arr[i] + " ");
        }
        System.out.println();
    }

    // 为了测试
    public static void main(String[] args) {
        int maxLen = 20;
        int maxValue = 30;
        int testTime = 300000;
        System.out.println("功能测试开始");
        for (int i = 0; i < testTime; i++) {
            int N = (int) (Math.random() * maxLen);
            int[] arr = randomArray(N, maxValue);
            int aim = (int) (Math.random() * maxValue);
            int ans1 = minCoins(arr, aim);
            int ans2 = dp1(arr, aim);
            int ans3 = dp2(arr, aim);
            if (ans1 != ans2 || ans1 != ans3) {
                System.out.println("Oops!");
                printArray(arr);
                System.out.println(aim);
                System.out.println(ans1);
                System.out.println(ans2);
                break;
            }
        }
        System.out.println("功能测试结束");
    }
}
```



#### 数的划分

**题目：**

```
给定不正数1，裂开的方法有一种，(1)
给定一个正数2，裂开的方法有两种，(1和1)、(2)
给定一个正数3，裂开的方法有三种，(1、1、1)、(1、2)、(3)
给定一个正数4，裂开的方法有五种，(1、1、1、1)、(1、1、2)、(1、3)、(2、2)、(4)
给定一个正数，求裂开的方法数,拆分数，满足构成该数的序列是递增的序列。
动态规划优化状态依赖的技巧
```

**解答：**

```java
package class23;
/*
给定不正数1，裂开的方法有一种，(1)
给定一个正数2，裂开的方法有两种，(1和1)、(2)
给定一个正数3，裂开的方法有三种，(1、1、1)、(1、2)、(3)
给定一个正数4，裂开的方法有五种，(1、1、1、1)、(1、1、2)、(1、3)、(2、2)、(4)
给定一个正数，求裂开的方法数,拆分数，满足构成该数的序列是递增的序列。
动态规划优化状态依赖的技巧
eg1:
  3 = 1 + 1 + 1
    = 1 + 2
    = 3
  ！= 2 +1

eg2:
  5 = process(1, 4)
    = process(2, 3)
    = process(3, 2)    3 > 2 不满足题目意思
    = process(4, 1)    4 > 1 不满足题目意思
    = process(5, 0)    5 > 0 满足题目意思  或者这是特殊情况，如果不再划分了，那么(pre,0)也是合法的

eg3:
  6 = process(1, 5)
    = process(2, 4)
    = process(3, 3)    3 == 3 满足题目意思,一种方法
    = process(4, 2)    4 > 2 不满足题目意思
    = process(5, 1)    5 > 1 不满足题目意思
    = process(6, 0)    6 > 0 满足题目意思  或者这是特殊情况，如果不再划分了，那么(pre,0)也是合法的

* */
public class Code03_SplitNumber {

    //n 为正数
    public static int ways(int n) {
        if (n < 0) {
            return 0;
        }
        if( n == 1) {
            return 1;
        }
        return process(1, n);
    }

    /**
     *
     * @param pre 上一个拆出来的数是pre
     * @param rest 还剩余rest需要的拆
     * @return  返回拆解的方法数
     */
    public static int process(int pre, int rest) {
//        if(pre == rest) return 1;
        if(rest == 0) return 1;
        if(pre > rest) return 0;
        // pre < rest
        int ways = 0;

        for (int first = pre; first <= rest; first++) {
            ways += process(first,rest - first);
        }

//        for (int first = pre; first < rest; first++) {
//            ways += process(first,rest - first);
//        }
//        ways ++;

        return ways;
    }

    public static int dp1(int n) {
        if (n < 0) {
            return 0;
        }
        if( n == 1) {
            return 1;
        }
        // pre: 1 ~ n
        // rest: 0 ~ n
        int[][] dp = new int[n+1][n+1];

        //(1) if(rest == 0) return 1;
        //(2)对角线为1，（1，1），（2，2），（3，3），（4，4）.....
        for (int pre = 1; pre <= n; pre++) {
            dp[pre][0] = 1;
            dp[pre][pre] = 1;
        }

        //第一行不需要填
        for(int pre = n-1; pre >= 1; pre--) {
            //对角线已经填了
            for(int rest = pre + 1; rest <= n; rest++) {
                int ways = 0;
                for (int first = pre; first <= rest; first++) {
                    ways += dp[first][rest - first];
                }
                dp[pre][rest] = ways;
            }
        }
        return dp[1][n];
    }

    public static int dp2(int n) {
        if (n < 0) {
            return 0;
        }
        if( n == 1) {
            return 1;
        }
        // pre: 1 ~ n
        // rest: 0 ~ n
        int[][] dp = new int[n+1][n+1];

        //(1) if(rest == 0) return 1;
        //(2)对角线为1，（1，1），（2，2），（3，3），（4，4）.....
        for (int pre = 1; pre <= n; pre++) {
            dp[pre][0] = 1;
            dp[pre][pre] = 1;
        }

        //第一行不需要填
        for(int pre = n-1; pre >= 1; pre--) {
            //对角线已经填了
            for(int rest = pre + 1; rest <= n; rest++) {
                dp[pre][rest] = dp[rest+1][rest];
                dp[pre][rest] += dp[pre][rest-pre];
            }
        }
        return dp[1][n];
    }

    public static void main(String[] args) {
        int test = 39;
        System.out.println(ways(test));
        System.out.println(dp1(test));
        System.out.println(dp2(test));
    }
}
```



#### 划分两个集合，使得集合和相近

**题目：**

```
  给定一个正数数组arr,
  请把arr中所有的数分成两个集合，尽量让两个集合的累加和接近
  返回：
  最接近的情况下，较小集合的累加和
```

**解答：**

```java
package class24;

/**
 * 给定一个正数数组arr,
 * 请把arr中所有的数分成两个集合，尽量让两个集合的累加和接近
 * 返回：
 * 最接近的情况下，较小集合的累加和
 */
public class Code01_SplitSumClosed {
    public static int right(int[] arr) {
        if(arr == null || arr.length < 2) {
            return 0;
        }
        int sum  = 0;
        for(int num : arr) {
            sum += num;
        }
        return process(arr, 0, sum >> 1);
    }

    /**
     * @param arr
     * @param u    arr[u....]可以自由选择
     * @param rest
     * @return 返回尽量接近rest，但是不能超过rest的情况下，最接近的累加和是多少
     */
    public static int process(int[] arr, int u, int rest) {
        if (u == arr.length) {
            return 0;
        }else { //还有数，arr[u]这个数
            //（1）不使用arr[u]
            int p1 = process(arr,u+1,rest);
            //（2）使用arr[u]
            int p2 = 0;
            if(arr[u] <= rest){
                //需要加上arr[u]
                p2 =arr[u] + process(arr,u+1,rest - arr[u]);
            }
            return Math.max(p1, p2);
        }
    }

    public static int dp(int[] arr) {
        if(arr == null || arr.length < 2) {
            return 0;
        }
        int N = arr.length;
        int sum = 0;
        for(int item : arr) {
            sum += item;
        }
        sum /= 2;
        //这里都需要多开一个位置 0
        int[][] dp = new int[N+1][sum+1];
        //dp[N][....] = 0
        for(int i = N-1; i >= 0; i--) {
            for(int rest = sum; rest >= 0; rest--) {
                //（1）不使用arr[u]
                int p1 = dp[i+1][rest];
                //（2）使用arr[u]
                int p2 = 0;
                if(arr[i] <= rest){
                    //需要加上arr[u]
                    p2 =arr[i] + dp[i+1][rest - arr[i]];
                }
                dp[i][rest] = Math.max(p1, p2);
            }
        }
        return dp[0][sum];
    }

    public static int[] randomArray(int len, int value) {
        int[] arr = new int[len];
        for (int i = 0; i < arr.length; i++) {
            arr[i] = (int) (Math.random() * value);
        }
        return arr;
    }

    public static void printArray(int[] arr) {
        for (int num : arr) {
            System.out.print(num + " ");
        }
        System.out.println();
    }

    public static void main(String[] args) {
        int maxLen = 20;
        int maxValue = 50;
        int testTime = 10000;
        System.out.println("测试开始");
        for (int i = 0; i < testTime; i++) {
            int len = (int) (Math.random() * maxLen);
            int[] arr = randomArray(len, maxValue);
            int ans1 = right(arr);
            int ans2 = dp(arr);
            if (ans1 != ans2) {
                printArray(arr);
                System.out.println(ans1);
                System.out.println(ans2);
                System.out.println("Oops!");
                break;
            }
        }
        System.out.println("测试结束");
    }
}
```



#### 考虑长度奇偶的划分集合-字节面试

**题目：**

```
  字节面试题目：
  给定一个正数数组arr,请把arr中所有的数分成两个集合
  如果arr长度为偶数，两个集合包含数的个数要一样多
  如果arr长度为奇数，两个集合包含数的个数必须只差一个
  请尽量让两个集合的累加和接近
  返回：
  最接近的情况下，较小集合的累加和
```

**解答：**

```java
package class24;

/**
 * 字节面试题目：
 * 给定一个正数数组arr,请把arr中所有的数分成两个集合
 * 如果arr长度为偶数，两个集合包含数的个数要一样多
 * 如果arr长度为奇数，两个集合包含数的个数必须只差一个
 * 请尽量让两个集合的累加和接近
 * 返回：
 * 最接近的情况下，较小集合的累加和
 */
public class Code02_SplitSumClosedSizeHalf {

    public static int right(int[] arr) {
        if(arr == null || arr.length < 2) {
            return 0;
        }
        int sum  = 0;
        for(int num : arr) {
            sum += num;
        }
        if((arr.length & 1) == 0){
            return process(arr, 0, arr.length / 2, sum / 2);
        }else{
            return Math.max(process(arr, 0, arr.length / 2, sum / 2),process(arr, 0, arr.length / 2 + 1, sum / 2));
        }
    }

    /**
     * @param arr
     * @param i   arr[i....]自由选择，个数picks，累加和<= rest 离rest最近的返回
     * @param picks  挑选个数一定要是picks个
     * @param rest
     * @return
     */
    public static int process(int[] arr, int i, int picks, int rest) {
        //如果没有数了
        if (i == arr.length) {
            return picks == 0 ? 0 : -1;
        }else { // 还有数arr[i]
            //(1) 不要arr[i]
            int p1 = process(arr, i + 1, picks, rest);
            //(2) 使用arr[i]
            int p2 = -1;
            int next = -1;
            if(arr[i] <= rest) {
                next = process(arr, i + 1, picks - 1, rest - arr[i]);
            }
            if(next != -1) {
                p2 = arr[i] + next;
            }
            return Math.max(p1, p2);
        }
    }

    public static int dp(int[] arr) {
        if(arr == null || arr.length < 2) {
            return 0;
        }
        int sum  = 0;
        for(int num : arr) {
            sum += num;
        }
        sum /= 2;
        int N = arr.length;
        int M = (N + 1) / 2;  //向上取整
        int[][][] dp = new int[N+1][M+1][sum + 1];
        for(int i = 0; i <= N; i++) {
            for(int j = 0; j <= M; j++) {
                for(int k = 0; k <= sum; k++) {
                    dp[i][j][k] = -1;
                }
            }
        }
        for(int rest = 0; rest <= sum; rest++) {
            dp[N][0][rest] = 0;
        }
        for(int i = N-1; i >= 0; i--) {
            for(int picks = 0; picks <= M; picks++) {
                for(int rest = 0;rest <= sum; rest++) {
                    //(1) 不要arr[i]
                    int p1 = dp[i + 1][picks][rest];
                    //(2) 使用arr[i]
                    int p2 = -1;
                    int next = -1;
                    if(arr[i] <= rest && picks-1 >= 0) {
                        next = dp[i + 1][picks - 1][rest - arr[i]];
                    }
                    if(next != -1) {
                        p2 = arr[i] + next;
                    }
                    dp[i][picks][rest] = Math.max(p1, p2);
                }
            }
        }

        if((arr.length & 1) == 0){
            return dp[0][arr.length / 2][sum];
        }else{
            return Math.max(dp[0][arr.length / 2][sum],dp[0][arr.length / 2 + 1][sum]);
        }
    }

    //test
    public static int dp2(int[] arr) {
        if (arr == null || arr.length < 2) {
            return 0;
        }
        int sum = 0;
        for (int num : arr) {
            sum += num;
        }
        sum >>= 1;
        int N = arr.length;
        int M = (arr.length + 1) >> 1;
        int[][][] dp = new int[N][M + 1][sum + 1];
        for (int i = 0; i < N; i++) {
            for (int j = 0; j <= M; j++) {
                for (int k = 0; k <= sum; k++) {
                    dp[i][j][k] = Integer.MIN_VALUE;
                }
            }
        }
        for (int i = 0; i < N; i++) {
            for (int k = 0; k <= sum; k++) {
                dp[i][0][k] = 0;
            }
        }
        for (int k = 0; k <= sum; k++) {
            dp[0][1][k] = arr[0] <= k ? arr[0] : Integer.MIN_VALUE;
        }
        for (int i = 1; i < N; i++) {
            for (int j = 1; j <= Math.min(i + 1, M); j++) {
                for (int k = 0; k <= sum; k++) {
                    dp[i][j][k] = dp[i - 1][j][k];
                    if (k - arr[i] >= 0) {
                        dp[i][j][k] = Math.max(dp[i][j][k], dp[i - 1][j - 1][k - arr[i]] + arr[i]);
                    }
                }
            }
        }
        return Math.max(dp[N - 1][M][sum], dp[N - 1][N - M][sum]);
    }

    // for test
    public static int[] randomArray(int len, int value) {
        int[] arr = new int[len];
        for (int i = 0; i < arr.length; i++) {
            arr[i] = (int) (Math.random() * value);
        }
        return arr;
    }

    // for test
    public static void printArray(int[] arr) {
        for (int num : arr) {
            System.out.print(num + " ");
        }
        System.out.println();
    }

    // for test
    public static void main(String[] args) {
        int maxLen = 10;
        int maxValue = 50;
        int testTime = 10000;
        System.out.println("测试开始");
        for (int i = 0; i < testTime; i++) {
            int len = (int) (Math.random() * maxLen);
            int[] arr = randomArray(len, maxValue);
            int ans1 = right(arr);
            int ans2 = dp(arr);
            int ans3 = dp2(arr);
            if (ans1 != ans2 || ans1 != ans3) {
                printArray(arr);
                System.out.println(ans1);
                System.out.println(ans2);
                System.out.println(ans3);
                System.out.println("Oops!");
                break;
            }
        }
        System.out.println("测试结束");
    }
}
```



### 3、DP优化 - 四边形不等式技巧



### 4、DP优化 - 状态压缩



### 5、DP优化 - 其他优化