---
title: UVM sequence启动过程
summary: 
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
{{< figure src="sequence_flow.png" caption="Sequence执行过程" numbered="true" height="75%" width="75%" >}}
`start()`任务的原型如下：

```verilog
virtual task start ( uvm_sequencer_base sequencer,
                     uvm_sequene_base   parent_sequence = null,
                     int                this_priority = -1,
                     bit                call_pre_post = 1       );
```

第一个参数sequencer必须设置，其他三个都是可选项。在调用`seq.start()`之后，将会按顺序执行以下函数：

```verilog
seq.pre_start();
seq.pre_body();                 // if call_pre_post == 1
    parent_seq.pre_do();        // if parent_seq != null
    parent_seq_mid_do(this);    // if parent_seq != null
seq.bosy();
    parent_seq_post_do(this);   // if parent_seq != null
seq.post_body();                // if call_pre_post == 1
sub_seq.post_start();
```





Reference:

[1] https://www.chipverify.com/uvm/how-to-execute-sequences-via-start-method