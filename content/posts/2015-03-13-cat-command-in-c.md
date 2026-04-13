---
title: "C语言实现Linux的cat命令"
date: 2015-03-13
draft: false
slug: "cat-command-in-c"
categories:
  - "Code"
---
模拟实现Linux系统中的cat命令
```c
/*
 *  filename: cat_file.c
 *  本程序模拟Linux系统中的cat程序，实现的功能：
 *  cat file1  浏览文件1的内容
 *  cat file1 file2 同时浏览多个文件
 *  在程序实现过程中，函数file_copy()实现文件内容的复制
 *  By linccn 2011.06.06
 * */
#include <stdio.h>
#include <stdlib.h>

void file_copy(FILE *inFile, FILE *outFile)
{
    int c=0;
    while((c=getc(inFile)) != EOF)
        putc(c, outFile);
}

int main(int argc, char *argv[])
{
    FILE *fp;
    while(--argc > 0){
        if ((fp=fopen(*++argv, "r")) == NULL)
        {
            printf("Can't open file\n");
            return 1;
        }
        else
        {
            file_copy(fp, stdout);
            fclose(fp);
        }
    }

    return 0;
}
```

