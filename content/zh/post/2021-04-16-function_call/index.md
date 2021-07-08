---
title: SystemVerilog的fork-join语句中的函数调用
summary: 解释并解决使用SV时出现的一种变量异常赋值。

date: ""
publishDate: "2021-04-16T14:33:00+08:00"
lastmod: ""

view: 2

comments: true
profile: true
share: false

featured: false
headless: false
draft: false
private: false

gitment: true
slug: sv-fork-join

tags:
- technical issue
---
当使用SystemVerilog时，在fork语句中调用函数时需要注意，例如：
```verilog
module test1;
    initial begin
        for (int j = 0; j < 3; j++)
            begin
                int k;
                k = j;
                fork
                    $write(k);
                join_none
            end
            #0
            $display("end\n");
        end
endmodule
```
这段程序可能很多人认为会输出`0, 1, 2`，但是实际的输出是`2, 2, 2`。原因是这段程序**只给静态变量k分配一次内存空间**，而fork join语句中的程序**只有当父线程执行结束之后才执行**，所以所有的子线程拿到的k值都是2。

要解决这个问题可以使用`automatic`类型的变量：`automatic int k = j`。`automatic`类型的变量在所有过程语句之前创建，并且与父线程同时进行。