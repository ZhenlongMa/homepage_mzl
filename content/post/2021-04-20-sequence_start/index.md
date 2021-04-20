---
title: Use of start() of sequences in UVM
summary: Procedure of start method of sequences in UVM.
publishDate: "2020-04-20T12:29:00Z"

view: 2

comments: true
profile: true
share: false

# Optional header image (relative to `static/img/` folder).
header:
  caption: ""
  image: ""
---
{{< figure src="sequence_flow.png" caption="Sequence Flow" numbered="true" height="75%" width="75%" >}}
The prototype of `start()` is:

```verilog
virtual task start ( uvm_sequencer_base sequencer,
                     uvm_sequene_base   parent_sequence = null,
                     int                this_priority = -1,
                     bit                call_pre_post = 1       );
```

The first argument must be set, others being optional.





Reference:

[1] https://www.chipverify.com/uvm/how-to-execute-sequences-via-start-method