---
title: Use of start() of sequences in UVM
summary: Procedure of start method of sequences in UVM.
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

categories:
- tools

tags:
- UVM
---
{{< figure src="sequence_flow.png" caption="Sequence Flow" numbered="true" height="75%" width="75%" >}}
The prototype of `start()` is:

```verilog
virtual task start ( uvm_sequencer_base sequencer,
                     uvm_sequene_base   parent_sequence = null,
                     int                this_priority = -1,
                     bit                call_pre_post = 1       );
```

The first argument must be set, others being optional. After calling `seq.start()`, following methods will execute in order and in demand:

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