C is pretty much a macro language for assembly.

Assembly languages are low-level programming languages where you are giving instructiins like

"load from memory location 123" "load from memory location 124" "add those together" "store the result at memory location 124" "compare to memory 77" "if that location equals 4, jump to instruction 60"

Different processors have different assembly languages, so you can't run the same assembly programs on, say, x86 and ARM machines.


But which assembly to choose?

Whatever processor you're writing for? Different architectures use different instruction sets

Let’s say I want one for an amd rizen 3200. Should I pick masm? Gas? Nasm? Yasm?

They’re all x86_64 assemblers. There isn’t “assembly language” so much as a whole family of assembly languages. It’s not like saying it’s programmed in C is all I’m saying.

For x86 there are two main dialects of assembly: AT&T and Intel. Personally I prefer Intel, I think it looks cleaner with less sigils, and putting the destination operator first makes more sense with instructions like `sub` and especially with comparison and jump. `sub eax 4` computes eax-4 and stores the result in eax, on AT&T syntax this is written `sub $4 %eax` so you have to read "subtract 4 from eax". And `cmp 4 eax; jl label` is simply "jump if 4 < eax" on Intel syntax, but in AT&T syntax the comparison is swapped to `cmp %eax $4`, but `jl` still means "jump if 4 < eax". It's very easy to get confused.

Beyond this, the assemblers are mostly different in whether they support macros and what kind. So the choice isn't too important.