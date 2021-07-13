# Get It Working

I once saw on Twitter (can't find proper attribution now 😞) a good mantra
for how to approach a programming problem: "Get it working, get it right, get
it fast," *in that order.* In other words, you shouldn't aim for perfection
straight away: you will have a hard time even getting started. Rather, start
by doing the "naive" approach, the first thing that pops in your head, and
get it running without errors. Don't worry about edge cases, or about whether
a specific line of code is correct or off by one. Just get it running.

Once it's running without errors, check the inputs and outputs for some known
values, and make sure they match what you expect. For example, if you write a
function that will rotate a 2D vector by 90 degrees between the x and y axes,
check that running [1, 0] through the function gives [0, 1]. Also, check that
[1, 1] gets you close to [-1, 1]. (It might not be perfect because floating
point arithmetic in computers is not exact.) And so on. If the output doesn't
match, *now* you start working through the code to try to figure out where
the calculations are going wrong. Importantly, [write tests](testing) so that
you know from then on, every time you change the code, whether it is still
working correctly.

Finally, when everything is working, *then* you can worry about making it run
faster. There are [lots of strategies](get-it-fast) for this, and,
importantly, you *absolutely must* have tests in place before doing this,
because it's a virtual certainty that in your speedups you will end up
breaking the functionality, sometimes in subtle ways. The tests are your
North Star that tell you when your speedups are actually correct.

So, in this section, we start with "get it working:" how to understand
errors when they happen ({ref}`understanding-errors`), and how to track them
down and fix them ({ref}`debugging`).
