# convert module to ScriptModule

### 需要注意的地方

- torch.jit.trace时，如果有某一层为null则报错，此处需要在__init__中过滤submodule为null的情况
- tracing(torch.jit.trace) and scripting(@torch.jit.script)
- forward输入输出只能是Tensor, List, tuple