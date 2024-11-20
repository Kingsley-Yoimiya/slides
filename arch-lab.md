---
# You can also start simply with 'default'
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://cover.sli.dev
# some information about your slides (markdown enabled)
title: Arch Lab 小班回课
info: |
  ## Arch lab
# apply unocss classes to the current slide
class: text-center
# https://sli.dev/features/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations.html#slide-transitions
transition: slide-left
# enable MDC Syntax: https://sli.dev/features/mdc
mdc: true
# take snapshot for each slide in the overview
overviewSnapshots: true
---

# Arch Lab

2300012929 尹锦润

<div class="pt-12">
  <span @click="$slidev.nav.next" class="px-2 py-1 rounded cursor-pointer" hover="bg-white bg-opacity-10">
    Press Space for next page <carbon:arrow-right class="inline"/>
  </span>
</div>

<div class="abs-br m-6 flex gap-2">
  <button @click="$slidev.nav.openInEditor()" title="Open in Editor" class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-white">
    <carbon:edit />
  </button>
  <a href="https://github.com/slidevjs/slidev" target="_blank" alt="GitHub" title="Open in GitHub"
    class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-logo-github />
  </a>
</div>

<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->

---
transition: fade-out
level: 2
---
# PartA


---
transition: fade-out
level: 2
---

# PartA-sum_list

根据 `archlab-project/misc/y86-code` 中代码作为基础模板，然后就是 Y86-64 翻译了。
<div grid="~ cols-2 gap-2" m="t-2">

```cpp
/* sum_list - Sum the elements of a linked list */
long sum_list(list_ptr ls)
{
    long val = 0;
    while (ls) {
      val += ls->val;
      ls = ls->next;
    }
    return val;
}
```

```asm
main:	irmovq ele1,%rdi
	call sum_list		# sum_list(ele1)
	ret
# long sum_list(long *start)
sum_list: xorq %rax,%rax # sum = 0
  jmp     test         # Goto test
loop:	mrmovq (%rdi),%rsi
    addq %rsi,%rax
    mrmovq 8(%rdi),%rdi
test:	andq %rdi,%rdi
    jne loop
    ret                  # Return
```
</div>
<br>
<br>

<!--
You can have `style` tag in markdown to override the style for the current page.
Learn more: https://sli.dev/features/slide-scope-style
-->

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

<!--
Here is another comment.
-->

---
transition: slide-up
level: 2
---

# PartA-rsum_list

根据 `archlab-project/misc/y86-code` 中代码作为基础模板，然后就是 Y86-64 翻译了。
<div grid="~ cols-2 gap-2" m="t-2">

```cpp
/* rsum_list - Recursive version of sum_list */
long rsum_list(list_ptr ls) {
  if (!ls) return 0;
  else {
    long val = ls->val;
    long rest = rsum_list(ls->next);
    return val + rest;
  }
}
```

```asm
main: irmovq ele1,%rdi
 call rsum_list  # sum_list(ele1)
 ret

# long rsum_list(long *start)
# start in %rdi
rsum_list: xorq %rax,%rax      # sum = 0
  andq %rdi, %rdi
  je zero
  mrmovq (%rdi), %rsi
  addq %rsi, %rax
  pushq %rax
  mrmovq 8(%rdi), %rdi
  call rsum_list
  popq %rsi
  addq %rsi, %rax
zero:
 ret                  # Return
```
</div>
<br>
<br>

<!--
You can have `style` tag in markdown to override the style for the current page.
Learn more: https://sli.dev/features/slide-scope-style
-->

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

<!--
Here is another comment.
-->

---
transition: slide-up
level: 2
---


# PartA-bubble

根据 `archlab-project/misc/y86-code` 中代码作为基础模板，然后就是 Y86-64 翻译了。
<div grid="~ cols-3 gap-3" m="t-2">

```cpp
/* rsum_list - Recursive version of sum_list */
void bubble_sort(long *data, long count) {
  long *i, *last;
  for(last = data + count - 1; last > data; last--) {
      for(i = data; i < last; i++) {
          if(*(i + 1) < *i) {
              long t = *(i + 1);
              *(i + 1) = *i;
              *i = t;
          }
      }
  }
}
```

```asm
bubble_a: xorq %rcx, %rcx # last = 0
 addq %rdi, %rcx
 rrmovq %rsi, %r8
 addq %rsi, %r8 # 2 * count
 addq %r8, %r8 # 4 * count
 addq %r8, %r8 # 8 * count
 addq %r8, %rcx 
 irmovq $8, %r8
 subq %r8, %rcx # last = data + count - 1
 jmp test1
loop1: rrmovq %rdi, %rdx # i = data
 jmp test2
loop2: mrmovq (%rdx), %r8
 mrmovq 8(%rdx), %r9
 subq %r9, %r8
 jle failed
 mrmovq (%rdx), %r8
 mrmovq 8(%rdx), %r9
 rmmovq %r8, 8(%rdx)
 rmmovq %r9, (%rdx)
```

```asm
failed:
 irmovq $8, %r8
 addq %r8, %rdx
test2: rrmovq %rcx, %r8
 subq %rdx, %r8
 jg loop2
 irmovq $8, %r8
 subq %r8, %rcx
test1: rrmovq %rcx, %r8
 subq %rdi, %r8
 jg loop1
 ret

main: irmovq array, %rdi
 irmovq $6, %rsi
 call bubble_a
 ret
```

</div>
<br>
<br>

<!--
You can have `style` tag in markdown to override the style for the current page.
Learn more: https://sli.dev/features/slide-scope-style
-->

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

<!--
Here is another comment.
-->

---
transition: slide-up
level: 1
---

# PartB

---
transition: slide-up
level: 2
---

# PartB - seq_full

现在要对于 `seq_full` 增加 `IOPQ` 操作。

````md magic-move {lines: true}
```rs
bool instr_valid = icode in { ..., OPQ, ...,  };
bool need_regids =
    icode in { CMOVX, OPQ, PUSHQ, POPQ, IRMOVQ, RMMOVQ, MRMOVQ };
bool need_valC = icode in { IRMOVQ, RMMOVQ, MRMOVQ, JX, CALL };
u8 srcB = [
    icode in { OPQ, RMMOVQ, MRMOVQ } : ialign.rB;
];
u8 dstE = [
    icode in { IRMOVQ, OPQ } : ialign.rB;
];
// Select input A to ALU
u64 aluA = [
    icode in { IRMOVQ, RMMOVQ, MRMOVQ } : ialign.valC;
];
u8 alufun = [
    icode in { OPQ } : ifun;
    true : ADD;
];
bool set_cc = icode in { OPQ };
```

```rs
bool instr_valid = icode in { ..., OPQ, ..., IOPQ };
bool need_regids =
    icode in { CMOVX, OPQ, IOPQ, PUSHQ, POPQ, IRMOVQ, RMMOVQ, MRMOVQ };
bool need_valC = icode in { IRMOVQ, RMMOVQ, MRMOVQ, JX, CALL, IOPQ };
u8 srcB = [
    icode in { OPQ, RMMOVQ, MRMOVQ, IOPQ } : ialign.rB;
];
u8 dstE = [
    icode in { IRMOVQ, OPQ, IOPQ } : ialign.rB;
];
// Select input A to ALU
u64 aluA = [
    icode in { IRMOVQ, RMMOVQ, MRMOVQ, IOPQ } : ialign.valC;
];
u8 alufun = [
    icode in { OPQ, IOPQ } : ifun;
    true : ADD;
];
bool set_cc = icode in { OPQ, IOPQ };
```

````

---
transition: slide-up
level: 2
---

# PartB - pipe_s3a
3-stage pipelines, containing instruction **fetch stage, decode stage** and all the rest part as **execute stage**.

本质上还是对照注释加深对于流水线的理解。

````md magic-move {lines: true}
```rs 
u64 f_pc = [
    D.icode == CALL : D.valC;
    // Taken branch.  Use instruction constant
    U8_PLACEHOLDER == JX && e_cnd : U64_PLACEHOLDER;
    // Completion of RET instruction.  Use value from stack
    // valM is from DEMW stage, thus the current cycle
    U8_PLACEHOLDER == RET : e_valM;
    // Default: Use incremented PC
    1 : F.valP;
];
u8 e_dstE = [
    U8_PLACEHOLDER == CMOVX && !BOOL_PLACEHOLDER : RNONE;
    1 : E.dstE;
];
bool data_harzard = d_srcA != RNONE && d_srcA in { e_dstE, e_dstM }
    || U8_PLACEHOLDER != RNONE && BOOL_PLACEHOLDER;
bool f_stall = D.icode in { JX, RET } || BOOL_PLACEHOLDER;
bool d_stall = BOOL_PLACEHOLDER;
```

```rs
u64 f_pc = [
    D.icode == CALL : D.valC;
    // Taken branch.  Use instruction constant
    E.icode == JX && e_cnd : E.valC;
    // Completion of RET instruction.  Use value from stack
    // valM is from DEMW stage, thus the current cycle
    E.icode == RET : e_valM;
    // Default: Use incremented PC
    1 : F.valP;
];
u8 e_dstE = [
    E.icode == CMOVX && !e_cnd : RNONE;
    1 : E.dstE;
];
bool data_harzard = d_srcA != RNONE && d_srcA in { e_dstE, e_dstM }
    || d_srcB != RNONE && d_srcB in { e_dstE, e_dstM };
bool f_stall = D.icode in { JX, RET } || data_harzard;
bool d_stall = data_harzard;
```
````

---
transition: slide-up
level: 2
---

# PartB - `pipe_s3b`

在 `pipe_s3a` 基础上加入转发，对照书本即可。而在情况处理阶段，就是用转发把 `data_hazard` 删除了。

````md magic-move {lines: true}
```rs 
u64 d_valA = [
    d_srcA == U8_PLACEHOLDER : e_valE;
    d_srcA == U8_PLACEHOLDER : e_valM;
    1: reg_read.valA;
];
u64 d_valB = [
    BOOL_PLACEHOLDER : U64_PLACEHOLDER;
    BOOL_PLACEHOLDER : U64_PLACEHOLDER;
    1: reg_read.valB;
];
bool f_stall = D.icode in { U8_PLACEHOLDER, U8_PLACEHOLDER };
bool d_bubble = D.icode in { U8_PLACEHOLDER, U8_PLACEHOLDER };
```

```rs
u64 d_valA = [
    d_srcA == e_dstE : e_valE;
    d_srcA == e_dstM : e_valM;
    1: reg_read.valA;
];
u64 d_valB = [
    d_srcB == e_dstE : e_valE;
    d_srcB == e_dstM : e_valM;
    1: reg_read.valB;
];
bool f_stall = D.icode in { JX, RET };
bool d_bubble = D.icode in { JX, RET };
```
````

---
transition: slide-up
level: 2
---

# PartB - `pipe_s3c`

We can implement **branch prediction**, since both branch options are known in the Decode stage. It's just that we have to wait until the **Execute** stage to **determine** which branch to take.

````md magic-move {lines: true}
```rs 
u64 f_pc = [ // pipe_s3b
    D.icode == CALL : D.valC;
    // Taken branch.  Use instruction constant
    E.icode == JX && e_cnd : E.valC;
    E.icode == RET : e_valM;
    1 : F.valP;
];
bool f_stall = D.icode in { JX, RET };
bool d_stall = false;
bool d_bubble = D.icode in { JX, RET };
bool e_bubble = false;
```

```rs
u64 f_pc = [ // pipe_s3c
    D.icode == CALL : D.valC;
    // Branch misprediction.  Use incremental PC
    E.icode == JX && !e_cnd : E.valP;
    E.icode == RET : e_valM;
    1 : F.pred_pc;
];
bool branch_mispred = E.icode == JX && !e_cnd;
bool ret_harzard = D.icode == RET;
bool f_stall = ret_harzard && !branch_mispred;
bool d_stall = false;
bool d_bubble = ret_harzard && !branch_mispred;
bool e_bubble = branch_mispred;
```
````

当然要注意在这里我们的处理逻辑是和书上不一样的，对于 `branch_mispred` 情况，我们只有在 E 的时候进行了 bubble，在 D（和书上不同）,F 时候是 normal，效果是等价的（也可以看注释）。

---
transition: slide-up
level: 2
---

# PartB - `pipe_s3d`
We have merged register reading and writing into a single device called `reg_file`, and the operations are performed in the order of write first, then read. The purpose of this change is to avoid structural hazards.

其实没有多大的改动，就是把 `pipe_s3c` 的内容复制过来即可。

````md magic-move {lines: true}
```rs 
u64 d_valA = [
    d_srcA == e_dstE : e_valE;
    d_srcA == e_dstM : e_valM;
    1: reg_read.valA;
];
u64 d_valB = [
    d_srcB == e_dstE : e_valE;
    d_srcB == e_dstM : e_valM;
    1: reg_read.valB;
];
@set_input(reg_write, {
    dstE: e_dstE,
    dstM: e_dstM,
    valM: e_valM,
    valE: e_valE,
});
```

```rs
u64 d_valA = [
    d_srcA == e_dstE : e_valE;
    d_srcA == e_dstM : e_valM;
    1: reg_file.valA;
];
u64 d_valB = [
    d_srcB == e_dstE : e_valE;
    d_srcB == e_dstM : e_valM;
    1: reg_file.valB;
];
@set_input(reg_file, {
    srcA: d_srcA,
    srcB: d_srcB,
    dstE: e_dstE,
    dstM: e_dstM,
    valM: e_valM,
    valE: e_valE,
});
```
````

---
transition: slide-up
level: 2
---

# PartB - `pipe_s4a`
4-stage pipelines, containing instruction **fetch stage, decode stage, execute stage** and all the rest part as **memory stage**.

这里利用了 `||` 的优先级。

````md magic-move {lines: true}
```rs 
u64 f_pc = [
    E.icode == RET : e_valM;
];
u64 d_valA = [
    d_srcA == e_dstE : e_valE;
    d_srcA == e_dstM : e_valM;
    1: reg_file.valA;
]; // d_valB similar
bool branch_mispred = E.icode == JX && !e_cnd;
bool ret_harzard = D.icode == RET;
bool f_stall = ret_harzard && !branch_mispred;
bool d_stall = false;
bool d_bubble = ret_harzard && !branch_mispred;
bool e_bubble = branch_mispred;
```

```rs
u64 f_pc = [
    M.icode == RET : m_valM;
];
u64 d_valA = [
    d_srcA == e_dstE : e_valE;
    d_srcA == m_dstM : m_valM;
    1: reg_file.valA;
]; // d_valB similar
bool branch_mispred = E.icode == JX && !e_cnd;
bool ret_harzard = RET in { D.icode, E.icode };
bool load_use_harzard = E.icode in { MRMOVQ, POPQ } && E.dstM in { d_srcA, d_srcB };
bool f_stall = ret_harzard && !branch_mispred || load_use_harzard;
bool d_stall = load_use_harzard;
bool d_bubble = ret_harzard && !branch_mispred  && !d_stall;
bool e_bubble = branch_mispred || load_use_harzard;
```
````

---
transition: slide-up
level: 2
---

# PartB - `pipe_s4b`
We can store `e_cnd` in the Memory Stage, register and replace it with `M.cnd`.

这个可以通过将信号传递下去减少硬件依赖，可以启示 PartC 里面优化 ac 的方式。

````md magic-move {lines: true}
```rs 
u64 f_pc = [
    M.icode == JX && !e_cnd : M.valP;
];
@set_stage(m, {
    ...
});
bool f_stall = ret_harzard && !branch_mispred || load_use_harzard;
bool d_stall = load_use_harzard;
bool d_bubble = ret_harzard && !branch_mispred  && !d_stall;
bool e_bubble = branch_mispred || load_use_harzard;
```

```rs
u64 f_pc = [
    M.icode == JX && !M.cnd : M.valP;
];
@set_stage(m, {
    ...
    cnd: e_cnd,
});
bool f_stall = ret_harzard || load_use_harzard;
bool d_stall = load_use_harzard;
bool d_bubble = branch_mispred || ret_harzard && !d_stall;
bool e_bubble = branch_mispred || load_use_harzard;
```
````

注意这里的控制逻辑已经和书上的一样了，因为将 `e_cnd` 往后挪了。 


---
transition: slide-up
level: 2
---

# PartB - `pipe_s4c`
教材中提到的特殊修改，将 `val_P` 和 `val_A` 合并。

(这里不列出将 `val_P` 替换成 `val_A` 的部分了)

````md magic-move {lines: true}
```rs 
u64 d_valA = [
    d_srcA == e_dstE : e_valE;
    d_srcA == m_dstM : m_valM;
    1: reg_file.valA;
];
```

```rs
u64 d_valA = [
    D.icode in { CALL, JX } : D.valP;
    d_srcA == e_dstE : e_valE;
    d_srcA == m_dstM : m_valM;
    1: reg_file.valA;
];
```
````

---
transition: slide-up
level: 1
---

# PartC

---
transition: slide-up
level: 2
---

# PartC 优化 CPE

引用 [A 神 Blog](https://arthals.ink/blog/arch-lab)。

关键点：

1. 手写循环展开
2. 把 `IOPQ` 融入 `ncopy.ns` 中。
3. 戳气泡：利用 CC 可以延迟存在，从中间插入其他内容，比如 `iaddq` 和 `je` 中间可以加入 `mrmovq`。

通过这些操作可以基本上在 Part_C 里面拿到 57 分，继续卷循环展开的方法（8 路 / 9 路 / etc？）理论上可以拿到满分，但是通过 ac 优化可以更快拿到。

---
transition: slide-up
level: 2
---

# PartC 优化 ac

给出很简单的实现，首先我们 copy `pipe_std.rs`。

## Architecture Cost

根据 `cargo run --bin ysim -- -A [arch_name] -I` 我们可以得到一张依赖图（或者直接看层级），蓝色的代表硬件写入，红色的代表信号，AC 定义就是 蓝色最长路径 +1。

## 优化关键

我们可以看到就在 `cond` 上面。

```
lv.4: cond e_cnd d_bubble e_bubble e_dstE d_valA d_valB
```

这是最长的一条硬件依赖链，重点就是优化 `e_cnd` 的获取行为。

```
dmem ->  reg_cc -> cond
```

---
transition: slide-up
level: 2
---

# PartC 优化 ac-Cond

观察行为找到优化方式。

我们注意到，`e_cnd` 需要通过 `Cond`, `Cond` 需要先 `Set CC`， 而 `Set CC` 前面必须先执行完成 E 阶段的 ALU 计算，进而形成了依赖链条。

那么能不能不 Set CC 呢？能！

需要 `Cond` 时候的 `CMOVXX` 等等不需要 `set CC`，于是可以在 D 阶段把 `CC` 取出来，参考提示设置为 `CC_INIT` 然后传到 `E.dcc` 即可减少 `Cond` 的硬件依赖。 

````md magic-move {lines: true}
```rs 
ExecuteStage e {
  stat: Stat = Bub, icode: u8 = NOP, ifun: u8 = 0, ...
}
Stat d_stat = D.stat;
@set_stage(e, {
}
ConditionCode cc = reg_cc.cc;
```

```rs
ExecuteStage e {
  stat: Stat = Bub, icode: u8 = NOP, ifun: u8 = 0, dcc: ConditionCode = CC_INIT, ...
}
Stat d_stat = D.stat;
ConditionCode dcc = reg_cc.cc;
@set_stage(e, {
  dcc: dcc,
}
ConditionCode cc = E.dcc;
```
````

改动四行即可收工，但是对于流水线的理解要非常透彻才行。
