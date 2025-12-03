Java的argv和C的差不多，都指的命令行参数。

```java
public class HelloGoodbye {  
    public static void main(String[] args) {  
        String name1 = args[0];  
        String name2 = args[1];  
  
        System.out.println("Hello " + name1 + " and " + name2 + ".");  
        System.out.println("Goodbye " + name2 + " and " + name1 + ".");  
    }  
}
```
```
javac HelloGoodbye
```
进行编译。
```
java HelloGoodbye Alice Bob
```
运行。

编写一个程序 `RandomWord.java`，从 **标准输入**（例如终端输入或重定向文件）读取一系列单词，然后 **均匀随机**地打印其中的一个单词。

### **Knuth 方法 / Reservoir Sampling 原理**

1. **当读取到第 i 个单词时**：
    - 以 **1/i 的概率**把它选为当前的“冠军”（champion），替换之前的冠军。
2. **继续读取下一个单词**：
    - 用同样的方法更新冠军。
3. **读取完所有单词后**：
    - 最终的“冠军”就是要打印的单词。

> 这个方法保证了：**每个单词被选中的概率都是均等的 1/n**，即完全随机。

```c
#include<string.h>
#include<stdio.h>
#include<stdlib.h>
#include <time.h>

int main()

{

    srand(time(NULL));  //初始化随机种子
    int r = rand();
    char champion[50]={0};
    char buff[50]={0};
    int count=0;
    while(fgets(buff,sizeof(buff),stdin)!=NULL)
    {
        buff[strcspn(buff, "\n")] = '\0';  // 去掉换行符
        count++;
        if(rand() % count == 0)//1/count的概率选中。
        {
            strcpy(champion,buff);
        }
    }
    printf("%s",champion);
    return 0;
}
```
- `rand()` 返回一个 **整数 `int`**
- 范围在 **0 到 RAND_MAX** 之间（RAND_MAX 是宏，通常是 32767 或更大，取决于实现
```
 if(rand() % count == 0)
        {
            strcpy(champion,buff);
        }
```
rand()%count会让生成的随机数变成[0,count-1]之间，等于0的概率是1/count。

```c
char *fgets(char *str, int n, FILE *stream);

fgets(buff,sizeof(buff),stdin)
```
fgets返回的是指针，读到末尾会返回NULL。

这段程序的输入是每行一个单词，末尾需要输入ctrl+z，回车后才能结束。

如果想要以空格为结束，那么在while循环里：
```c
while (fgets(buff, sizeof(buff), stdin) != NULL) {
        // 去掉换行符
        buff[strcspn(buff, "\n")] = '\0';
        // 如果输入空行，结束循环
        if (buff[0] == '\0') break;
        count++;
        // 以 1/count 概率替换 champion
        if (rand() % count == 0) {
            strcpy(champion, buff);
        }
    }
```
如果没有单词，可以加入判断：
```c
    if (count > 0) {
        printf("随机选中的单词：%s\n", champion);
    } else {
        printf("没有输入单词。\n");
    }

    return 0;
}
```
如果想输入一行，用空格分开的话，可用strtok：
```c
        // 使用 strtok 按空格拆分单词
        char *word = strtok(buff, " ");
        while (word != NULL) {
            count++;
            // 以 1/count 概率替换 champion
            if (rand() % count == 0) {
                strcpy(champion, word);
            }
            word = strtok(NULL, " ");  // 获取下一个单词
        }
```
