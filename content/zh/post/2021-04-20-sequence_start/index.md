---
title: UVM sequence启动过程
summary: UVM中启动sequence时各个task或function的执行顺序
publishDate: "2021-04-20T12:29:00Z"

view: 2

comments: true
profile: true
share: false

headless: false
# Optional header image (relative to `static/img/` folder).
header:
  caption: ""
  image: ""
---

在UVM中，可以通过`start()`方法启动一个sequence，`start()`任务的原型如下：

```verilog
virtual task start ( uvm_sequencer_base sequencer,
                     uvm_sequene_base   parent_sequence = null,
                     int                this_priority = -1,
                     bit                call_pre_post = 1       );
```

其中第一个参数sequencer必须设置，其他三个都是可选项。在调用`seq.start()`之后，将会按顺序执行：`seq.prestert()` -> `seq.pre_body()` -> `parent_seq.pre_do()` -> `parent_seq.mid_do(this)` -> `seq.body()` -> `parent_seq.post_do(this)` -> `seq.post_body()` -> `sub_seq.post_start()`。

其中`pre_start()`、`body()`、`post_start()`函数无条件执行，`pre_body()`和`post_body()`函数只有在启动当前sequence时将`call_pre_post`设为1才会执行，而将parent_sequence设置为特定的父sequence时在子sequence的body()函数前后将会执行这个特定的父sequence中的`pre_do()`、`mid_do()`和`post_do()`函数。

例如，在seq1的`body()`函数中启动了seq2，并把自己设为seq2的父sequence，那么整体执行流程如下图所示：


{{< figure src="sequence_flow.png" caption="Sequence执行过程" numbered="true" height="75%" width="75%" >}}


Reference:

[1] https://www.chipverify.com/uvm/how-to-execute-sequences-via-start-method