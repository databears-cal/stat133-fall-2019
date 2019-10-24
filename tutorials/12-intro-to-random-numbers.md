Random Numbers and Simulations
================
Gaston Sanchez

> ### Learning Objectives
> 
>   - How to use R to simulate chance processes
>   - Getting to know the function `sample()`
>   - Simulate flipping a coin
>   - Visualize relative frequencies

-----

## Introduction

Random numbers have many applications in science and computer
programming, especially when there are significant uncertainties in a
phenomenon of interest. In this tutorial we’ll look at a basic problem
that involves working with random numbers and creating simulations.

More specifically, let’s see how to use R to simulate basic chance
processes like tossing a coin.

## Let’s flip a coin

Chance processes, also referred to as chance experiments, have to do
with actions in which the resulting outcome turns out to be different in
each occurrence.

Typical examples of basic chance processes are tossing one or more
coins, rolling one or more dice, selecting one or more cards from a deck
of cards, and in general, things that can be framed in terms of drawing
tickets out of a box (or any other type of container: bag, urn, etc.).

You can use your computer, and R in particular, to simulate chances
processes. In order to do that, the first step consists of learning how
to create a virtual coin, or die, or box-with-tickets.

### Creating a coin

The simplest way to create a coin with two sides, `"heads"` and
`"tails"`, is with an R character vector via the *combine* function
`c()`

``` r
# a (virtual) coin object
coin <- c("heads", "tails")
```

You can also create a *numeric* coin that shows `1` and `0` instead of
`"heads"` and `"tails"`:

``` r
num_coin <- c(0, 1)
```

Likewise, you can also create a *logical* coin that shows `TRUE` and
`FALSE` instead of `"heads"` and `"tails"`:

``` r
log_coin <- c(TRUE, FALSE)
```

## Tossing a coin

Once you have an object that represents the *coin*, the next step
involves learning how to simulate tossing the coin. One way to simulate
the action of tossing a coin in R is with the function `sample()` which
lets you draw random samples, with or without replacement, from an input
vector.

To toss the coin use `sample()` like this:

``` r
coin <- c('heads', 'tails')

# toss the coin
sample(coin, size = 1)
```

    ## [1] "tails"

with the argument `size =`, specifying that we want to take a sample of
size 1 from the input vector `coin`.

### Function `sample.int()`

Another function related to `sample()` is `sample.int()` which simulates
drawing random integers. The main argument is `n`, which represents the
maximum integer to sample from: `1, 2, 3, ..., n`

``` r
sample.int(10)
```

    ##  [1] 10  2  6  4  1  8  7  9  5  3

### Random Samples

By default, `sample()` draws each element in `coin` with the same
probability. In other words, each element is assigned the same
probability of being chosen. Another default behavior of `sample()` is
to take a sample of the specified `size` **without replacement**. If
`size = 1`, it does not really matter whether the sample is done with or
without replacement.

To draw two elements WITHOUT replacement, use `sample()` like this:

``` r
# draw 2 elements without replacement
sample(coin, size = 2)
```

    ## [1] "tails" "heads"

What if we try to toss the coin three or four times?

``` r
# trying to toss coin 3 times
sample(coin, size = 3)
```

    ## Error in sample.int(length(x), size, replace, prob): cannot take a sample larger than the population when 'replace = FALSE'

Notice that R produced an error message. This is because the default
behavior of `sample()` cannot draw more elements that the length of the
input vector.

To be able to draw more elements, we need to sample WITH replacement,
which is done by specifying the argument `replace = TRUE`, like this:

``` r
# draw 4 elements with replacement
sample(coin, size = 4, replace = TRUE)
```

    ## [1] "tails" "tails" "heads" "tails"

## The Random Seed

The way `sample()` works is by taking a random sample from the input
vector. This means that every time you invoke `sample()` you will likely
get a different output.

In order to make the examples replicable (so you can get the same output
as me), you need to specify what is called a **random seed**. This is
done with the function `set.seed()`. By setting a *seed*, every time you
use one of the random generator functions, like `sample()`, you will get
the same values.

``` r
# set random seed
set.seed(1257)

# toss a coin with replacement
sample(coin, size = 4, replace = TRUE)
```

    ## [1] "heads" "tails" "heads" "heads"

All computations of random numbers are based on deterministic
algorithms, so the sequence of numbers is not truly random. However, the
sequence of numbers appears to lack any systematic pattern, and we can
therefore regard the numbers as random.

Every time you use one of the random generator functions in R, the call
produces different numbers. For replication and debugging purposes, it
is useful to get the same sequence of random numebrs every time we run
the script. This functionality is obtained by setting a **seed** before
we start generating the numebrs. The seed is an integer and set by the
function `set.seed()`

``` r
set.seed(123)
runif(4)
```

    ## [1] 0.2875775 0.7883051 0.4089769 0.8830174

If we set the seed to `123` again, the sequence of uniform random
numbers is regenerated:

``` r
set.seed(123)
runif(4)
```

    ## [1] 0.2875775 0.7883051 0.4089769 0.8830174

If we don’t specify a seed, the random generator functions set a seed
based on the current time. That is, the seed will be different each time
we run the script and consequently the sequence of random numbers will
also be different.

## Sampling with different probabilities

Last but not least, `sample()` comes with the argument `prob` which
allows you to provide specific probabilities for each element in the
input vector.

By default, `prob = NULL`, which means that every element has the same
probability of being drawn. In the example of tossing a coin, the
command `sample(coin)` is equivalent to `sample(coin, prob =
c(0.5, 0.5))`. In the latter case we explicitly specify a probability of
50% chance of heads, and 50% chance of tails:

    ## [1] "tails" "heads"

    ## [1] "heads" "tails"

However, you can provide different probabilities for each of the
elements in the input vector. For instance, to simulate a **loaded**
coin with chance of heads 20%, and chance of tails 80%, set `prob =
c(0.2, 0.8)` like so:

``` r
# tossing a loaded coin (20% heads, 80% tails)
sample(coin, size = 5, replace = TRUE, prob = c(0.2, 0.8))
```

    ## [1] "tails" "tails" "heads" "tails" "tails"

-----

## Simulating tossing a coin

Now that we have all the elements to toss a coin with R, let’s simulate
flipping a coin 100 times, and use the function `table()` to count the
resulting number of `"heads"` and `"tails"`:

``` r
# number of flips
num_flips <- 100

# flips simulation
coin <- c('heads', 'tails')
flips <- sample(coin, size = num_flips, replace = TRUE)

# number of heads and tails
freqs <- table(flips)
freqs
```

    ## flips
    ## heads tails 
    ##    55    45

In my case, I got 55 heads and 45 tails. Your results will probably be
different than mine. Some of you will get more `"heads"`, some of you
will get more `"tails"`, and some will get exactly 50 `"heads"` and 50
`"tails"`.

Run another series of 100 flips, and find the frequency of `"heads"` and
`"tails"`:

``` r
# one more 100 flips
flips <- sample(coin, size = num_flips, replace = TRUE)
freqs <- table(flips)
freqs
```

    ## flips
    ## heads tails 
    ##    52    48

## Tossing function

Let’s make things a little bit more complex but also more interesting.
Instead of calling `sample()` every time we want to toss a coin, we can
write a `toss()` function:

``` r
#' @title coin toss function 
#' @description simulates tossing a coin a given number of times
#' @param x coin object (a vector)
#' @param times number of tosses
#' @return vector of tosses
toss <- function(x, times = 1) {
  sample(x, size = times, replace = TRUE)
}

# basic call
toss(coin)
```

    ## [1] "tails"

``` r
# toss 5 times
toss(coin, 5)
```

    ## [1] "heads" "tails" "heads" "heads" "tails"

We can make the function more versatile by adding a `prob` argument that
let us specify different probabilities for `heads` and `tails`

``` r
#' @title coin toss function 
#' @description simulates tossing a coin a given number of times
#' @param x coin object (a vector)
#' @param times number of tosses
#' @param prob vector of probabilities for each side of the coin
#' @return vector of tosses
toss <- function(x, times = 1, prob = NULL) {
  sample(x, size = times, replace = TRUE, prob = prob)
}

# toss a loaded coin 10 times
toss(coin, times = 10, prob = c(0.8, 0.2))
```

    ##  [1] "tails" "heads" "tails" "heads" "heads" "heads" "heads" "heads"
    ##  [9] "heads" "heads"

## Counting Frequencies

The next step is to toss a coin several times, and count the frequency
of `heads` and `tails`

``` r
# count frequencies
tosses <- toss(coin, times = 100)
table(tosses)
```

    ## tosses
    ## heads tails 
    ##    54    46

We can also count the relative frequencies:

``` r
# relative freqs (proportions)
table(tosses) / length(tosses)
```

    ## tosses
    ## heads tails 
    ##  0.54  0.46

To make things more interesting, let’s consider how the frequency of
`heads` evolves over a series of `n` tosses.

``` r
n <- 500
tosses <- toss(coin, times = n)
heads_freq <- cumsum(tosses == 'heads') / 1:n
```

In this case, we can make a plot of the relative frequencies:

``` r
plot(heads_freq, type = 'l', lwd = 2, col = 'tomato', las = 1,
     ylim = c(0, 1))
abline(h = 0.5, col = 'gray50')
```

![](12-images/head_freqs_plot-1.png)<!-- -->
