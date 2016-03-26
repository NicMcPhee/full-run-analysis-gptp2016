# Analysis of individual programs

Here's the general workflow for analyzing individual programs from this run:

* Pick an individual
* Figure out who its parents are
* For each parent (if there are more than one):
   * Diff (see below) the parent and child genomes (using `genome_diff_file.txt`)
   * Diff the parent and child programs (using `program_diff_file.txt`)
   * Diff the parent and child error vectors _twice_
      * Once with `error_vector_diff_file.txt`
      * Once with `error_vector_even_odd_file.txt`
* Write up your results in an appropriately named Markdown file (e.g., 0_126.md)

There's more detail on each of these below.

## Picking an individual

Feel free to just pick an individual from the printed graph we'll have in the
room. They're all labeled with names of the form "GEN_LOC" (e.g., "19_554").
The first number (19 in our example) is the generation that individual lived in
and the second number (554 here) is just it's ID number within that generation.

**Put your initials next to an individual when you claim it so we don't end
up with two people doing the same individual.**

## Figure out who its parents are

You should be able to just read that off the graph. If you're not sure, though,
**definitely ask**. We don't want people `diff`ing the wrong individuals as
that would be quite confusing.

If the edge from your individual to its parent is orange, then that individual
only had one parent, and was created via a mutation operator. These will
probably be easier to analyze, because the parent and child will be nearly
identical, with just a few changes.

If the edge(s) from your individual are black/grey, then that individual was
created via crossover and had two parents. It's possible, though, that the
graph (and our data files) will only include one parent because our filtering
tool decided that the other parent really didn't contribute very much to the
child. **This might be an incorrect assumption.** If you find that your child
seems to have gotten a sizable chunk of code from the other parent, then you
should definitely note that in big bold letters in your report so we can look
into it. This is _especially_ true if there are substantial differences in the
program as well as the genome, and _doubly especially_ true if there are
substantial differences in the _simplifed program_ (if we get that far).

## Ways to make children

There were four ways that a child can be made in the system that generated this
data:

 * `uniform-mutation`: solid orange
 * `uniform-close-mutation`: dashed orange
 * `alternation`: solid black/gray
 * `alternation-uniform-mutation`: dashed black/gray

### `uniform-mutation`

This randomly replaces a gene with a new, randomly generated gene. These should
show up as just random changes to (usually isolated) instructions when `diff`ing
the genomes. It's less clear how these changes will show up in the programs and
the error vectors. In many cases there may be no change in the program, or just
a few instructions replaced with the new random instructions. Similarly, the
error vector may not change at all, or it might just change in a few places, or
it might change totally. We'll have to see.

### `uniform-close-mutation`

Here _only_ the number part of a gene will change on a small number of genes.
This will often lead to no change in the program, and no change in the errors.
There may be cases, though, where this causes blocks in the program to grow or
shrink (essentially by moving where closing parentheses are), which could change
the error vector quite significantly.

## `Diff` tools

There are lots of tools out there to compute `diff`s between files, often used
to see what's changed in code when doing things like merges. I think that
`kdiff3` is a pretty nice GUI `diff` tool, but there are lots of others. I'm
totally open to suggestions, but I would tend to prefer that we all use the
same tool since sometimes different tools (or different flag settings) will
lead to different outputs, which could skew our results in weird ways.

**Whenever you have two parents make sure you use the 3-way diff capability
of the `diff` tool.** This will allow you to see differences between the
parent and both parents all at the same time, and makes things like crossover
much easier to analyze.

## `Diff`ing genomes

Using whatever `diff` tool we agree to use, `diff` the file
`genome_diff_file.txt` that is in the child's directory and in the one or two
parent directories. The genomes are sequences of genes, where each gene is
essentially a pair containing an instruction and what's called a "close count"
(you don't really care).
