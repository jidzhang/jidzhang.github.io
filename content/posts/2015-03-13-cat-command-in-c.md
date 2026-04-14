---
title: "C 语言实现 Linux cat 命令"
date: 2015-03-13
draft: false
slug: "cat-command-in-c"
categories:
  - "Code"
---

用 C 语言模拟实现 Linux 的 `cat` 命令，支持查看单个或多个文件内容。

## 代码实现

```c
/*
 * 模拟 Linux cat 命令
 * 用法：./mycat file1 [file2 ...]
 */
#include <stdio.h>
#include <stdlib.h>

/* 将文件内容复制到输出流 */
void file_copy(FILE *in, FILE *out)
{
    int c;
    while ((c = getc(in)) != EOF)
        putc(c, out);
}

int main(int argc, char *argv[])
{
    FILE *fp;

    if (argc == 1) {
        /* 无参数时，从标准输入读取（与 cat 行为一致） */
        file_copy(stdin, stdout);
    } else {
        while (--argc > 0) {
            if ((fp = fopen(*++argv, "r")) == NULL) {
                fprintf(stderr, "cat: %s: 无法打开文件\n", *argv);
                return 1;
            }
            file_copy(fp, stdout);
            fclose(fp);
        }
    }

    return 0;
}
```

## 要点

- `getc` / `putc` 逐字符读写，适合小文件；大文件建议用 `fread` / `fwrite` 按块读写
- `argc == 1` 时从 stdin 读取，使程序支持管道：`echo hello | ./mycat`
- 错误信息输出到 `stderr`，不会混入正常输出

## 编译运行

```bash
gcc -o mycat mycat.c
./mycat file1.txt file2.txt
```
