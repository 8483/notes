# Premature Optimization

> _"Premature optimization is the root of all evil"_ - Donald Knuth

You're writing code to solve a real world problem. Getting to a solution faster is more important than a fast solution.

-   Macro performance (Design level) - system wide decisions ex. sql vs nosql
-   Micro performance (Fine tuning) - modules, functions...

1. Have a **real** performance problem.
2. Make 80% moves (what change leads to an 80% reduction? usually data structures).
3. Use a profiler to fix hot spots.
4. Get under the hood (meomry).

# Quotes

> A sentence I stole online and sometimes tell my classmates is "If your code doesn't work, we don't care how fast it doesn't work". I think it's a great piece of advice.

> My data structures and algorithms lecturer said something similar, an anecdote about him replacing a broken algorithm. The exchange was along the lines of 'But your algorithm takes 3 times as long as my algorithm!' 'Yes, but my algorithm gets the right answer'

> Correctness comes before efficiency. Sometimes people also say “I can make it run a bit faster if it doesn’t have to handle all these edge cases”. Or, in other words, if it doesn’t have to be correct.

> That's a clever adaptation of the shorter one they use often when training devs: "It's easier to make working code go faster than it is to make fast code work"

> One lesson that always stuck with me since college was that software engineering doesn’t just have one time-cost. It has three: runtime cost, development time cost, and repair/maintenance cost. Which happen to be a perfect allegory to the performance, velocity, adaptability triangle
