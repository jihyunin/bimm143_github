# Class06: Writing functions in R
Jihyun In (PID: A16955363

## Introduction

R code

``` r
add <- function(x, y=10, z=0){
  x + y + z
}
```

I can just use this funciton

``` r
add (1,100)
```

    [1] 101

``` r
add(x=c(1,2,3,4), y=100)
```

    [1] 101 102 103 104

Functions can have “required” input arguments and “optional” input
arguments. The optional arguments are defined with an equals default
value (`y=0`) in the function definition.

``` r
add(x=1, y=100, z=10)
```

    [1] 111

## Generate DNA sequence

> Q. Write a function to return a DNA sequence of a user specifed
> length. Call it `generate_dna()`

The `sample()` can help here.

``` r
#generate_dna() <- function(size=5){ }

students <- c("jeff", "jeremy", "peter")

sample(students, size = 5, replace=TRUE)
```

    [1] "jeremy" "jeff"   "jeremy" "jeremy" "peter" 

Now work with `bases` rather than `students`

``` r
bases <- c("A", "C", "G", "T")
sample(bases, size=10, replace=TRUE)
```

     [1] "A" "C" "T" "T" "C" "T" "T" "A" "G" "G"

Now I have a working ‘snippet’ of code, so I can use this as the body of
my first function version here:

``` r
generate_dna <- function(size=5){
  bases <- c("A", "C", "G", "T")
  sample(bases, size=size, replace=TRUE)
}
```

``` r
generate_dna(size=100)
```

      [1] "T" "A" "A" "C" "C" "T" "A" "G" "T" "G" "T" "T" "C" "C" "C" "A" "A" "G"
     [19] "A" "G" "T" "C" "A" "T" "G" "C" "T" "G" "A" "C" "C" "C" "T" "C" "G" "C"
     [37] "C" "T" "G" "T" "A" "T" "T" "T" "T" "C" "G" "A" "A" "G" "G" "G" "C" "T"
     [55] "A" "G" "C" "C" "A" "T" "G" "T" "T" "A" "T" "A" "T" "T" "A" "A" "C" "C"
     [73] "T" "G" "G" "A" "A" "G" "C" "T" "A" "G" "G" "T" "C" "T" "A" "C" "T" "A"
     [91] "A" "A" "G" "A" "C" "T" "A" "A" "T" "G"

``` r
generate_dna()
```

    [1] "A" "T" "T" "A" "A"

I want the ability to return a sequence like “AGTACCTG” string, i.e. a
one element vector where the bases are all together.

``` r
generate_dna <- function(size=5, together=TRUE){
  bases <- c("A", "C", "G", "T")
  sequence <- sample(bases, size=size, replace=TRUE)
  if (together){
    sequence <- paste(sequence, collapse = "")
  }
  return(sequence)
  
}
```

``` r
generate_dna(together=FALSE)
```

    [1] "A" "A" "C" "C" "C"

## 3. Generate Protein function

we can get the set of 20 natural amino-acids from the **bio3d** package

> 17. Write a protein sequence generating function that will return
>     sequences of a user-specified length?

``` r
generate_protein <- function(size=5, together=TRUE){
 aa <- bio3d::aa.table$aa1[1:20]
 sequence <- sample(aa, size=size, replace=TRUE)
 
  ## Optionally return a single string 
  if (together){
    sequence <- paste(sequence, collapse = "")
  }
  return(sequence)
}
```

> Q. Generate random protein sequences of length 6 to 12 amino acids.

``` r
#errored code
#generate_protein(size=6:12)
```

We can fix this inability to generate multiple sequences by either
editing and adding to the function body code (eg. for a for loop) or by
using the R **apply** family of utility functions

``` r
ans <- sapply(6:12, generate_protein)
ans
```

    [1] "VAHPYA"       "AHRSFHA"      "LETSHGFH"     "GHLKWEARR"    "MLGGNRLLSG"  
    [6] "MVCPKNWHKIF"  "DLFDFVFIAPGE"

It would be cool and useful if I could get FASTA format output.

``` r
cat(ans, sep="\n")
```

    VAHPYA
    AHRSFHA
    LETSHGFH
    GHLKWEARR
    MLGGNRLLSG
    MVCPKNWHKIF
    DLFDFVFIAPGE

I want this to look like

    >ID.6
    CNSTRV
    >ID.7
    NGCVYMV
    >ID.8
    QATSNMQI

``` r
with.id <- paste(">ID.",6:12,"\n", ans, sep="")
cat(with.id, sep="\n")
```

    >ID.6
    VAHPYA
    >ID.7
    AHRSFHA
    >ID.8
    LETSHGFH
    >ID.9
    GHLKWEARR
    >ID.10
    MLGGNRLLSG
    >ID.11
    MVCPKNWHKIF
    >ID.12
    DLFDFVFIAPGE

> Q. Determine if these sequences can be found in nature or are they
> unique? Why or why not?

I BLASTp searched by FAFST format sequences against the sequences with
length 6, 7, 8, were not unique with 100% coverage and 100% identity.

sequences with length 9,10,11,12 are unique and can’t be found in the
databases.
