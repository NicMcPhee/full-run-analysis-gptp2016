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

## Genomes, programs, and error vectors

For the rest to make sense it's useful to quickly go over the key pieces of the system:

* Genomes: Linear sequences of genes
* Programs: Generated from the genomes (which is a little complicated)
* Error vectors: The integer errors for this program's output for each of the 200 test cases

The genomes are linear sequences of genes, where each gene is
essentially a pair containing an instruction and what's called a _close count_. The
close count is a little complicated, but there are certain instructions (things like _if_ and _while_ statements) that come with "built-in" open parens that start blocks (like the body of an _if_). The close count in a gene specifies how many close parentheses will follow the instruction in the gene, up to the number of open parens at that point in the program. So if there are 4 open parentheses and the close count is 2, then 2 close parentheses would be inserted after that instruction. If 4 were open and the close count was 9, then the 4 open parentheses would be closed and the "extra" 5 would be ignored.

Programs are essentially Clojure-esque programs, but with really odd instructions that were designed for the purposes of evolution rather than human understandability. I'm not going to go over all those instructions here, but we might go through a program or two together.

Each program is tested by being run on 200 different test cases, generating 200 integer errors. The test problem being used for this run requires that the evolved program both print something and return an integer value. This means that we have two different kinds of test cases, 100 to check the accuracy of the printing part and 100 to check the accuracy of the integer return values. These are interleaved, so items 0 and 1 of the error vector are the error on the printing for the first test case and (respectively) the error on the return value. The `error_vector_diff_file.txt`s are the errors in their "regular" interleaved order, where the `error_vector_even_odd_file.txt`s are rearranged so that all the printing errors come together first, followed then by the return errors. We're particularly interested in changes where error going down, and _especially_ particularly interested in cases where the error goes from non-zero to zero.

### Ways to make children

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

Here _only_ the close count part of a gene will change on a small number of genes.
This will often lead to no change in the program, and no change in the errors.
There may be cases, though, where this causes blocks in the program to grow or
shrink (essentially by moving where closing parentheses are), which could change
the error vector quite significantly.

### `alternation`

This is essentially the crossover operator in this system. The child is constructed by
assembling alternating chunks from the two parents. This should be pretty apparent when diffing the genomes – you should see the alternating sections pretty clearly there. The exception is that when the two parents are very similar (or identical), then the result of alternation may look kind of mutation-y. There may be a few small deletions, where one or two genes are removed, or places where a few genes are duplicated (so, for example, AB gets turned into ABAB). The programs _may_ show alternating program sections, but it may not; it'll depend a lot on the how that genome translates to a program. The error vectors may be (nearly) the same (especially if there's very little change to the genome), but they could be wildly different.

### `alternation-uniform-mutation`

The same as the previous operation, but it's followed by a `uniform-mutation` step.

## `Diff`ing and `Diff` tools

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

### `Diff`ing genomes

Using whatever `diff` tool we agree to use, `diff` the file
`genome_diff_file.txt` that is in the child's directory and in the one or two
parent directories. Then document the differences you see. That might be small changes like mutations of single genes, or big chunks coming from different parent genes. Make sure to turn the line numbers on so you can be specific in your reports.

### `Diff`ing programs

`Diff`ing the `program_diff_file.txt`s may be harder to figure out. There might be "obvious" changes like a mutation of a single instruction or swaps of chunks of programs. It may be more complicated, though, because of weird things like the way the close parentheses work. Again, make sure you have line numbers on and document what comes from which parent.

### `Diff`ing error vectors

My guess is that `diff`ing the `error_vector_even_odd_file.txt` files will be more informative than `error_vector_diff_file.txt`, but I don't really know that until we dig around. You don't need to document every little change in error values (they're often going to be pretty random), but it would nice to see when errors go from non-zero to zero (or the reverse), and especially imporant to know when there are blocks of error values that change to zero.

## An example: Individual 10:473

This individual was constructed via alternation with mutation, but only has one parent 
in the graph (9:109).

### Diffing the genomes

This suggests that we did want to include the other parent, as lines 1-43 all seemed to come from that parent. All the rest came from 9:109, though, with lines 44-136 all coming exactly from lines 57-149 of 9:109. There were no mutations in that large block, which was potentially interesting.

### Diffing the programs

The first 20 "lines" of the program also came from the parent not included in the filtered graph, further suggesting that we do need to include that parent. 

Lines 21-41 are mostly copied exactly from lines 27-51 of 9:109, with two exceptions. Lines 32 and 37 of 9:109 seem to have been deleted, possibly through alternation shifts, or due to differences between the two parents.

### Diffing the error vectors

The error vectors are different in almost every location. Many of the even numbered cases got slightly worse (e.g., cases 0, 2, and 4 went from 0 to 1). Odd cases 1-27 and 51-63 stayed the same, but those were the only coherent block that didn't change.

