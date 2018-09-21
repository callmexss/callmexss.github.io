---
title: Coursera-algorithm：配置algs4环境 
date: 2017-10-17 21:22:34
tags: 
- java
- DSA
- 环境配置
---

折腾了一天，总算是把测试的程序运行成功了，问题还是出在了环境变量上。

1. 要把` algs4.jar ` 和 ` stdlib.jar` 都添加到 ` class_path ` 才能使用jar包提供的功能。

2. 运行编译好的class文件时不加扩展名，例如Example.class

   ```
   java Example 
   ```

   否则会出现以下错误

   ```
   错误: 找不到或无法加载主类 Example.class
   ```

3. 运行测试程序

   数据文件 ` tinyUF.txt: ` 

   ```
   10
   4 3
   3 8
   6 5
   9 4
   2 1
   8 9
   5 0
   7 2
   6 1
   1 0
   6 7
   ```

   ​

   ```java
   import edu.princeton.cs.algs4.*;

   public class QuickFindUF {
   	private int[] id;
   	
   	public QuickFindUF(int N) {
   		id = new int[N];
   		for (int i = 0; i < N; i++)
   			id[i] = i;
   	}
   	
   	public boolean connected(int p, int q) {
   		return id[p] == id[q];
   	}
   	
   	public void union(int p, int q) {
   		int pid = id[p];
   		int qid = id[q];
   		for(int i = 0; i < id.length; i++) {
   			if (id[i] == pid) id[i] = qid;
   		}
   	}
   	
   	public int count() {
   		return id.length;
   	}
   	
   	public static void main(String[] args) {
           int n = StdIn.readInt();
           QuickFindUF qf = new QuickFindUF(n);
           while (!StdIn.isEmpty()) {
               int p = StdIn.readInt();
               int q = StdIn.readInt();
               if (qf.connected(p, q)) continue;
               qf.union(p, q);
               StdOut.println(p + " " + q);
           }
           StdOut.println(qf.count() + " components");
           StdOut.println(qf.connected(3, 4));
           StdOut.println(qf.connected(2, 5));
           StdOut.println(qf.connected(7, 4));
       }
   }

   ```

   编译运行：	

   ```
   >> javac QuickFindUF.java
   >> java QuickFindUF < tinyUF.txt
   >> output:
   4 3
   3 8
   6 5
   9 4
   2 1
   5 0
   7 2
   6 1
   10 components
   true
   true
   false
   ```

4. 关于怎么在IDE中添加参数执行的方法还没有找到，这里留到后续补充。
