# Introduction to probability

## 1: Probability Basics

We covered a bit of probability in the last mission, but we'll go more into depth here and build a strong foundation. Before we do that, let's introduce our dataset. Our dataset contains information on flags of countries around the world. Each row is a country. Here are the relevant columns:

- `name` -- name of the country
- `landmass` -- which continent the country is in (1=N.America, 2=S.America, 3=Europe, 4=Africa, 4=Asia, 6=Oceania)
- `area` -- country area, in thousands of square kilometers
- `population` -- rounded to the nearest million
- `bars` -- Number of vertical bars in the flag
- `stripes` -- Number of horizontal stripes in the flag
- `colors` -- Number of different colours in the flag
- `red`, `green`, `blue`, `gold`, `white`, `black`, `orange` -- 0 if color absent, 1 if color present in the flag

This data was collected from `Collins Gem Guide to Flags`. It was written in 1986, so some flag information may be out of date!

### Instructions

- Find the country with the most bars in its flag. Assign the `name` of the country to `most_bars_country`.
- Find the country with the highest population (as of 1986). Assign the name of the country to `highest_population_country`.

```
# Print the first two rows of the data.
print(flags[:2])
bars_sorted = flags.sort_values("bars", ascending=[0])
most_bars_country = bars_sorted["name"].iloc[0]

population_sorted = flags.sort_values("population", ascending=[0])
highest_population_country = population_sorted["name"].iloc[0]
```

## 2: Calculating Probability

Now that we've explored the data a bit, let's get back to probability. Probability can roughly be described as "the percentage chance of an event or sequence of events occurring".

If you think about a coin flip intuitively, there's a 50% chance of getting heads, and a 50% chance of getting tails. This is because there are only two possible outcomes, and each event is equally likely.

We can apply the same principle to find how likely it is to select an element with certain characteristics from a sample. In this case, our sample is all the country flags. With a coin flip, it's already known that only two outcomes exist -- we need to compute the probability ourselves for the flags.

We could compute the probability of a country flag having a certain characteristic by dividing how many flags have that characteristic by the total number of flags.

### Instructions

- Determine the probability of a country having a flag with the color orange in it. Assign the result to `orange_probability`.
- Determine the probability of a country having a flag with more than 1 stripe in it. Assign the result to `stripe_probability`.

```
total_countries = flags.shape[0]
orange_probability = flags[flags["orange"] == 1].shape[0] / total_countries

stripe_probability = flags[flags["stripes"] > 1].shape[0] / total_countries
```

## 3: Conjunctive Probabilities

We just found the probability of a country having a flag with more than one stripe. This was fairly straightforward, as it only involved one probability.

But let's say we have a coin that we flip 5 times, and we want to find the probability that it will come up heads every time. This is called a **conjunctive probability**, because it involves a sequence of events. We want to find the probability that the first flip is heads and the second flip is heads, and so on.

Each event in this sequence is independent, as the outcome of the first flip won't have an impact on the outcome of the last flip. All we have to do to compute the probability of this sequence is multiply the individual probabilities of each event together. This is `.5 * .5 * .5 * .5 * .5`, which equals `.03125`, giving us a `3.125%` chance that all 5 coin flips result in heads.

### Instructions

- Find the probability that `10` flips in a row will all turn out heads. Assign the probability to `ten_heads`.
- Find the probability that `100` flips in a row will all turn out heads. Assign the probability to `hundred_heads`.

```
five_heads = .5 ** 5
ten_heads = .5 ** 10
hundred_heads = .5 ** 100
```

## 4: Dependent Probabilities

Let's say that we're picking countries from the sample, and removing them when we pick. Each time we pick a country, we reduce the sample size for the next pick. The events are dependent -- the number of countries available to pick depends on the previous pick. We can't just calculate the probability upfront and take a power in this case -- we need to recompute the probability after each selection happens.

Let's simplify the example a bit by saying that we're eating some M&Ms. There are 10 M&Ms left in the bag: 5 are green, and 5 are blue. What are the odds of getting 3 blue candies in a row? The probability of getting the first blue candy is `5/10`, or `1/2`. When we pick a blue candy, though, we remove it from the bag, so the probability of getting another is `4/9`. The probability of picking a third blue candy is `3/8`. This means our final probability is `1/2 * 4/9 * 3/8`, or `.0833`. So, there is an 8.3% chance of picking three blue candies in a row.

### Instructions

- Let's say that we're picking countries from our dataset, and removing each one that we pick.
- What are the odds of picking three countries with red in their flags in a row? Assign the resulting probability to `three_red`.

```
# Remember that whether a flag has red in it or not is in the `red` column.
total_count = flags.shape[0]
red_count = flags[flags["red"] == 1].shape[0]
one_red = (red_count / total_count) 
two_red = one_red * ((red_count - 1) / (total_count - 1))
three_red = two_red * ((red_count - 2) / (total_count - 2))
```

## 5: Disjunctive Probability

Conjunctive probability is when something happens *and* something else happens. But sometimes, we want to know the probability of some event occurring or another event occurring. Let's say we're rolling a six-sided die -- the probability of rolling a 2 is `1/6`.

What if we want to know the probability of rolling a 2 *or* the probability of rolling a three? We actually can just add the probabilities, because both events are independent. Rolling a 2 doesn't change my odds of rolling a three next time around. Thus, the probability is `1/6 + 1/6`, or `1/3`.

### Instructions

- Let's say we have a random number generator that generates numbers from `1` to `18000`.
- What are the odds of getting a number evenly divisible by `100`, with no remainder? (ie `100`, `200`, `300`, etc). Assign the result to `hundred_prob`.
- What are the odds of getting a number evenly divisible by `70`, with no remainder? (ie `70`, `140`, `210`, etc). Assign the result to `seventy_prob`.

```
start = 1
end = 18000
def count_evenly_divisible(start, end, div):
    divisible = 0
    for i in range(start, end+1):
        if (i % div) == 0:
            divisible += 1
    return divisible

hundred_prob = count_evenly_divisible(start, end, 100) / end
seventy_prob = count_evenly_divisible(start, end, 70) / end
```

## 6: Disjunctive Dependent Probabilities

So we've covered disjunctive probabilities in the neat case where everything is mututally exclusive, and we can just add them up.

But, let's think about a slightly more complex case with dependencies. What if we have a set of 10 cars -- 5 are red and 5 are blue. 5 of the 10 are convertibles, and 5 are sport utility vehicles.

If we wanted to find cars that were red or were convertibles, we might try to add the probability of the car being red to the probability of the car being a convertible. This would give us `1/2 + 1/2 == 1`. But, this is wrong, as it tells us that all 10 cars are either red or convertibles.

It's wrong because it assumes that the two traits (color and vehicle type) are independent, when in fact they aren't. Some of the cars are red and convertibles. If we don't account for this overlap, we end up with a vastly inflated count.

Let's say that we have `3` cars that are red and convertibles. Our probability for red or convertible then comes out to `(1/2 + 1/2) - 3/10`. We subtract `3/10` to account for the cars we double counted when we computed `(1/2 + 1/2)`. This gives us a `.7` probability of a car being a convertible or red.

### Instructions

- Find the probability of a flag having `red` or `orange` as a `color`. Assign the result to `red_or_orange`.
- Find the probability of a flag having at least one `stripes`or at least one `bars`. Assign the result to `stripes_or_bars`.

```
stripes_or_bars = None
red_or_orange = None
red = flags[flags["red"] == 1].shape[0] / flags.shape[0]
orange = flags[flags["orange"] == 1].shape[0] / flags.shape[0]
red_and_orange = flags[(flags["red"] == 1) & (flags["orange"] == 1)].shape[0] / flags.shape[0]

red_or_orange = red + orange - red_and_orange

stripes = flags[flags["stripes"] > 0].shape[0] / flags.shape[0]
bars = flags[flags["bars"] > 0].shape[0] / flags.shape[0]
stripes_and_bars = flags[(flags["stripes"] > 0) & (flags["bars"] > 0)].shape[0] / flags.shape[0]

stripes_or_bars = stripes + bars - stripes_and_bars
```

## 7: Disjunctive Probabilities With Multiple Conditions

We've looked at disjunctive probabilities in cases where there are only two conditions (A *or* B). But what if we have three or more conditions?

Let's say we have 10 cars again. 5 are red and 5 are blue. 5 are convertibles and 5 are sport utility vehicles. 5 have a top speed of 130mph, and 5 have a top speed of 110mph.

Let's say we want to find all cars that are red or convertibles or have a top speed of 130mph. Let's say 2 cars meet all three criteria. We would end up with `1/2 + 1/2 + 1/2 - 1/5`, or a `1.3` probability if we tried to apply the formula from before. This is clearly false, as we can't have a probability greater than `1`.

One easy way to solve for cases like this is to find everything that doesn't match our criteria first. In this case, we'd look for blue sport utility vehicles with a top speed of 110mph. We would then subtract that probability from 1 to get the probability of red *or* convertible *or* 130mph top speed. Let's say there are 2 vehicles that are blue *and* sport utility vehicles *and* have a 110mph top speed. We would get a `1 - .2` or `.8` probability for red *or* convertible *or* 130mph top speed.

### Instructions

- Let's say we have a coin that we're flipping. Find the probability that at least one of the first three flips comes up heads. Assign the result to `heads_or`.

```
heads_or = None
all_three_tails = (1/2 * 1/2 * 1/2)
heads_or = 1 - all_three_tails
```

# Calculating probabilities

## 1: The Dataset

In the last mission, we looked at probability, and the ideas of *disjunctive*, *dependent*, and *conjunctive* probabilities.

We'll dive more into probability in this mission, and calculate more complex probabilities. But first, we'll look at the dataset we'll be using.

In many countries, there are bikesharing programs where anyone can rent a bike from a depot, and return it at other depots throughout a city. There is one such program in Washington, D.C., in the US. We'll be looking at the number of bikes that were rented by day. Here are the relevant columns:

- `dteday` -- the date that we're looking at.
- `casual` -- the number of casual riders (people who hadn't previously signed up with the bikesharing program) that rented bikes on the day.
- `registered` -- the number of registered riders (people who signed up previously) that rented bikes.
- `cnt` -- the total number of bikes rented.

This data was collected by `Hadi Fanaee-T` at the `University of Porto`, and can be downloaded [here](http://archive.ics.uci.edu/ml/datasets/Bike+Sharing+Dataset).

## 2: Probability Of Renting Bikes

Let's explore our data a bit, first by finding the probability that more than `2000` bikes will be rented on any given day.

### Instructions

- Find the probability that more than `4000` bikes were rented on any given day.Assign the result to `probability_over_4000`.

```
import pandas
bikes = pandas.read_csv("bike_rental_day.csv")

# Find the number of days the bikes rented exceeded the threshold.
days_over_threshold = bikes[bikes["cnt"] > 2000].shape[0]
# Find the total number of days we have data for.
total_days = bikes.shape[0]

# Get the probability that more than 2000 bikes were rented for any given day.
probability_over_2000 = days_over_threshold / total_days
print(probability_over_2000)
days_over_threshold = bikes[bikes["cnt"] > 4000].shape[0]
probability_over_4000 = days_over_threshold / total_days
```

## 3: Up To Or Greater

Let's say we flip three coins, and we want to know the probability of getting `2` or more heads. In order to do this, we'd need to add the probability of getting exactly `2` heads with the probability of getting exactly `3`heads. The probability that any single coin will be heads is `.5` (the probability that the coin will be tails is the same, `.5`).

The probability of `3` heads is easy to calculate -- this can only happen in one situation, where all three coins are heads, or `.5 * .5 * .5`, which equals `.125`.

The probability of `2` heads is a little trickier -- there are three different combinations that the three coins can configure themselves in to end up with `2` heads. We show this in the table below, using `H` for heads, and `T`for tails.

```
Coin 1    Coin 2    Coin 3
H         H         T
T         H         H
H         T         H
```

Each one of these has a probability of `.5 * .5 * .5`, so we just multiply `3 * .125` to get `.375`, the probability that we'll get `2` heads.

We then just have to add up the probability of getting `2` heads to the probability of getting `3` heads to get `.5`, the probability of getting `2` or more heads when we flip 3 coins.

## 4: Calculating Probabilities

Now that we know how to calculate probabilities for coins, let's calculate the probability that `1` coin out of `3` is heads.

### Instructions

- Find the probability that `1` coin out of `3` is heads.Assign the result to `coin_1_prob`.

```
# Enter your code here.
# There are three combinations in which we can have one coin heads.
# HTT, THT, TTH
# Each combination's probability is (.5 * .5 * .5)
combination_prob = (.5 * .5 * .5) 
# The probability for one combination is in combination_prob -- multiply by the three possible combinations.
coin_1_prob = 3 * combination_prob
```

## 5: Number Of Combinations

What we found in the last screen was that there were exactly `3` combinations of coins to get `2` out of the `3`coins to be heads. There was exactly `1` combination to get all three coins to be heads.

Let's scale this example up a little bit. Let's say that we live in Los Angeles, CA, and the chance of any single day being sunny is `.7`. The chance of a day not being sunny is `.3`.

If we have a sample of `5` days, and we want to find the chance that all `5` of them will be sunny, there's only one combination that allows this to happen -- the sunny outcome has to occur on all `5` days:

```
Day 1    Day 2    Day 3    Day 4    Day 5
S        S        S        S        S
```

If we want to find the probability that only `4` days will be sunny, there are `5` possible combinations.

```
Day 1    Day 2    Day 3    Day 4    Day 5
S        S        S        S        N
S        S        S        N        S
S        S        N        S        S
S        N        S        S        S
N        S        S        S        S
```

You may notice a pattern here. The most extreme cases -- a given outcome happening all the time or none of the time, can only occur in one combination. The next step lower, a given outcome happening every time except once, or a given outcome only happening once, can happen in as many combinations as there are total events.

## 6: Calculating The Number Of Combinations

Now that we've worked out some tables, let's practice a bit.

### Instructions

- Find the number of combinations in which `1` day will be sunny.Assign the result to `sunny_1_combinations`.

```
sunny_1_combinations = None
# There are 5 combinations in which one day can be sunny.
# SNNNN
# NSNNN
# NNSNN
# NNNSN
# NNNNS

sunny_1_combinations = 5
```

## 7: Number Of Combinations Formula

In fact, there's an easily quantifiable pattern with the number of combinations. We can calculate the number of combinations in which an outcome can occur `k` times in a set of events with a formula:

N!k!(N−k)!

In this formula, N is the total number of events we have, and k is the target number of times we want our desired outcome to occur. So if we wanted to find the number of combinations in which `4` out of `5` days can be sunny, we'd set N to `5`, and k to `4`. The ! symbol means [factorial](https://en.wikipedia.org/wiki/Factorial). A factorial means "multiply every number from 1 to this number together". So 4! is `1*2*3*4`, which is `24`.

Plugging `4` and `5` into this formula gives us:

5!4!(5−4)!=5!4!(5−4)!=1∗2∗3∗4∗51∗2∗3∗4(1!)=12024=5

This matches our intuitive answer that we got earlier!

## 8: Finding The Number Of Combinations

We can calculate probabilities greater than or equal to a threshold with our bike sharing data. We found that the probability of having more riders than `4000` is about `.6`. We can use this to find the probability that in `10` days, `7`or more days have more than `4000`riders.

But first, let's find the number of combinations in which `7` days out of `10`have more than `4000` rentals. We can repeat this process for `8` and `9` days.

### Instructions

- Find the number of combinations where `8` days out of `10`have more than `4000` rentals. Assign the result to `combinations_8`.
- Find the number of combinations where `9` days out of `10`have more than `4000` rentals. Assign the result to `combinations_9`.

```
import math
def find_outcome_combinations(N, k):
    # Calculate the numerator of our formula.
    numerator = math.factorial(N)
    # Calculate the denominator.
    denominator = math.factorial(k) * math.factorial(N - k)
    # Divide them to get the final value.
    return numerator / denominator

combinations_7 = find_outcome_combinations(10, 7)
combinations_8 = find_outcome_combinations(10, 8)
combinations_9 = find_outcome_combinations(10, 9)
```

## 9: The Probability For Each Combination

Let's go back to our example about the number of sunny days in Los Angeles. We can call the probability that a day will be sunny p, and the probability that a day won't be sunny q.

p is the probability that an outcome will occur, and q is the complementary probability that the outcome won't happen -- 1−p=q.

When we calculate the number of combinations in which a given outcome can occur `k` times in `N` events, each of those combinations has a certain probability of occurring.

Let's say that for sunny days in Los Angeles, p is `.7`, and q is `.3`. If we look at `5` days, there is one combination in which every day is sunny -- the probability for this combination is `.7 * .7 * .7 * .7 * .7`, which equals `.168`.

There are `5` combinations in which only `4` days are sunny -- you can see our table earlier for a closer look. We can calculate the probability of the first combination with `.7 * .7 * .7 * .7 * .3`, which equals `.072`. The probability of the second combination is `.7 * .7 * .7 * .3 * .7`, which equals `.072`. We're multiplying all the same numbers, just in a different order, so this combination has the same probability as the first combination. The probability for each combination in which k of the same outcome can happen in N events is always the same.

## 10: Calculating The Probability Of One Combination

Now that we calculated the probability for a single combination occurring, let's practice the calculation a bit more.

### Instructions

- Find the probability of a single combination for finding `3`days out of `5` are sunny.The combination is `Sunny, Sunny, Sunny, Not Sunny, Not Sunny`.Assign the result to `prob_combination_3`.

```
prob_combination_3 = None
prob_combination_3 = .7 * .7 * .7 * .3 * .3
```

## 11: Per Combination Probability Formula

As we learned earlier, the probability for each combination in which k of the same outcome can happen in N events is always the same. Given this, we can calculate the probability of a given outcome happening k times in N events by multiplying the number of combinations in which our result can occur by the probability of a single combination occurring.

The probability of a single combination occurring is given by pk∗qN−k. We can verify this with our sunny days example. First, let's find the probability of one combination in which there are `5` sunny days out of `5`:

.75∗.35−5=.168∗.30=.168∗1=.168

Now, let's find the probability of one combination in which there are `4` sunny days out of `5`:

.74∗.35−4=.24∗.31=.24∗.3=.072

This matches up perfectly with what we did earlier. To find the overall probabilty of `4` days out of `5` being sunny, we just multiply the number of combinations by the probability of any single combination occurring. This gives us `.36`.

## 12: Function To Calculate The Probability Of A Single Combination

Now we know enough to find the probability that within `10` days, `7` or more days have more than `4000` riders. The probability of having more than `4000`riders on any single day is about `.6`. This means that p is `.6`, and q is `.4`.

### Instructions

- Write a function to find the probability of a single combination occurring.
- Use the function to calculate the probability of `8` days out of `10` having more than `4000` riders.
  - Assign the result to `prob_8`.
- Use the function to calculate the probability of `9` days out of `10` having more than `4000` riders.
  - Assign the result to `prob_9`.
- Use the function to calculate the probability of `10` days out of `10` having more than `4000` riders.
  - Assign the result to `prob_10`.

```
p = .6
q = .4
def find_combination_probability(N, k, p, q):
    # Take p to the power k, and get the first term.
    term_1 = p ** k
    # Take q to the power N-k, and get the second term.
    term_2 = q ** (N-k)
    # Multiply the terms out.
    return term_1 * term_2

prob_8 = find_outcome_combinations(10, 8) * find_combination_probability(10, 8, p, q)
prob_9 = find_outcome_combinations(10, 9) * find_combination_probability(10, 9, p, q)
prob_10 = find_outcome_combinations(10, 10) * find_combination_probability(10, 10, p, q)
```

## 13: Statistical Significance

Let's say we've invented a weather control device that can make the weather sunny (if only!), and we decide to test it on Los Angeles. The device isn't perfect, and can't make every single day sunny -- it can only increase the chance that a day is sunny. We turn it on for `10` days, and notice that the weather is sunny in `8` of those.

We touched on the question of statistical significance before -- it's the question of whether a result happened as the result of something we changed, or whether a result is a matter of random chance.

Typically, researchers will use `5%` as a significance threshold -- if an event would only happen `5%` or less of the time by random chance, then it is statistically significant. If an event would happen more than `5%` of the time by random chance, then it isn't statistically significant.

In order to determine statistical significance, we need to determine the percentage chance that the number of outcomes we saw or greater could happen by random chance.

In our case, there is `12%` chance that the weather would be sunny `8` days out of `10` by random chance. We add this to `4%` for `9` days out of `10`, and `.6%` for `10` days out of `10` to get a `16.6%` total chance of the sunny outcome happening `8` or more time in our `10` days. Our result isn't statistically significant, so we'd have to go back to the lab and spend some time adding more flux capacitors to our weather control device.

Let's say we recalibrate our weather control device successfully, and observe for `10` more days, of which `9` of them are sunny. This only has a `4.6%` chance of happening randomly (probability of `9` plus probability of `10`). This is a statistically significant result, but it isn't a slam-dunk. It would require more investigation, including collecting results for more days, to get a more conclusive result.

In practice, setting statistical significance thresholds is tricky, and can be highly variable. We'll be touching on how to set thresholds later on.

# Probability distributions

## 1: The Dataset

In the last mission, we looked at calculating probabilities. In this mission, we'll construct probability distributions. But first, let's look at the dataset we'll be using.

In many countries, there are bikesharing programs where anyone can rent a bike from a depot, and return it at other depots throughout a city. There is one such program in Washington, D.C., in the US. We'll be looking at the number of bikes that were rented by day. Here are the relevant columns:

- `dteday` -- the date that we're looking at.
- `cnt` -- the total number of bikes rented.

This data was collected by `Hadi Fanaee-T` at the `University of Porto`, and can be downloaded [here](http://archive.ics.uci.edu/ml/datasets/Bike+Sharing+Dataset).

## 2: Binomial Distributions

In the last mission, we defined p as the probability of an outcome occurring, and q as the probability of it not occurring, where q=1−p. These types of probabilites are known as binomial -- there are two values, which add to `1` collectively. There's a `100%` chance of one outcome or the other occurring.

Many commonly occurring events can be expressed in terms of binomial outcomes -- a coin flip, winning a football game, the stock market going up, and more.

When we deal with binomial probabilities, we're often interested in the chance of a certain outcome happening in a sequence. We want to know what the chances are of our favorite football team winning 5 of its next 6 games, and the stock market going up in 4 of the next 6 days.

The same interest applies when we're analyzing data. Companies and researchers conduct experiments every day. These can range from testing whether changing the button color on your webpage increases conversion rate to seeing if a new drug increases patient recovery rate.

The core of these tests is the idea of a binomial distribution -- we want to know how many visitors out of 100 would normally sign up for our website, and we want to know if changing our button color affected that probability.

One easy way to visualize binomials is a binomial distribution. Given `N` events, it plots the probabilities of getting different numbers of successful outcomes.

## 3: Bikesharing Distribution

Let's say we're working for the mayor of Washington, DC, [Muriel Bowser](https://en.wikipedia.org/wiki/Mayor_of_the_District_of_Columbia). She wants to know on how many days out of the next `30` we can expect more than `5000` riders.

Rather than give her an exact number, which may not be accurate, we can hedge our bets with a probability distribution. This will show her all the possibilities, along with probabilities for each.

First, we have to find the probability of there being more than `5000` riders in a single day.

### Instructions

- Find the probability of there being more than `5000` riders in a single day (using the `cnt` column).Assign the result to `prob_over_5000`.

```
import pandas
bikes = pandas.read_csv("bike_rental_day.csv")
prob_over_5000 = bikes[bikes["cnt"] > 5000].shape[0] / bikes.shape[0]
```

## 4: Computing The Distribution

We now know that the probability is about `.39` that there are more than `5000`riders in a single day. In the last mission, we figured out how to find the probability of `k` outcomes out of `N` events occurring. We'll need to use this to build up a list of probabilities.

The formula we used in the last mission was:

(pk∗qN−k)∗N!k!(N−k)!

### Instructions

- Using the knowledge from the last mission, create a function that can compute the probability of `k` outcomes out of `N` events occurring.
- Use the function to find the probability of each number of outcomes in `outcome_counts` occurring.
  - An outcome is a day where there are more than `5000`riders, with p=.39.
  - You should have a list with 31 items, where the first item is the probability of `0` days out of `30` with more than `5000` riders, the second is the probability of `1`day out of `30`, and so on, up to `30` days out of `30`.
  - Assign the list to `outcome_probs`.

```
import math

# Each item in this list represents one k, starting from 0 and going up to and including 30.
outcome_counts = list(range(31))
def find_probability(N, k, p, q):
    # Find the probability of any single combination.
    term_1 = p ** k
    term_2 = q ** (N-k)
    combo_prob = term_1 * term_2
    
    # Find the number of combinations.
    numerator = math.factorial(N)
    denominator = math.factorial(k) * math.factorial(N - k)
    combo_count = numerator / denominator
    
    return combo_prob * combo_count

outcome_probs = [find_probability(30, i, .39, .61) for i in outcome_counts]
```

## 5: Plotting The Distribution

You may have noticed that `outcome_counts` in the previous screen was `31` items long when `N` was only `30`. This is because we need to account for `0`. There's a chance of having k=0, where the outcome we want doesn't happen at all. This data point needs to be on our charts. We'll always want to add `1` to `N` when figuring out how many points to plot.

Our data is in terms of whole days. Either `1` day has more than `5000` riders, or `2`days have more than `5000` riders. It doesn't make sense to talk about the probability of `1.5` days having more than `5000` riders. The points in our data are [discrete](https://en.wikipedia.org/wiki/Probability_distribution#Discrete_probability_distribution) and not [continuous](https://en.wikipedia.org/wiki/Continuous_and_discrete_variables), so we use a bar chart when plotting.

Now that we've computed the distribution, we can easily plot it out using *matplotlib*. This will show us a nice distribution of our probabilities, along with the most likely outcomes.

## 6: Simplifying The Computation

To construct our distribution, we had to write our own custom function, and a decent amount of code. We can instead use the [binom.pmf](http://docs.scipy.org/doc/scipy-0.16.1/reference/generated/scipy.stats.binom.html) function from [SciPy](http://docs.scipy.org/doc/scipy/reference/index.html) to do this faster.

Here's a usage example:

```
from scipy import linspace
from scipy.stats import binom

# Create a range of numbers from 0 to 30, with 31 elements (each number has one entry).
outcome_counts = linspace(0,30,31)

# Create the binomial probabilities, one for each entry in outcome_counts.
dist = binom.pmf(outcome_counts,30,0.39)
```

The `pmf` function in SciPy is an implementation of the mathematical [probability mass function](https://en.wikipedia.org/wiki/Probability_mass_function). The `pmf` will give us the probability of each `k` in our `outcome_counts` list occurring.

A binomial distribution only needs two parameters. A **parameter** is the statistical term for a number that summarizes data for the entire population. For a binomial distribution, the parameters are:

- `N`, the total number of events,
- `p`, the probability of the outcome we're interested in seeing.

The SciPy function `pmf` matches this and takes in the following parameters:

- `x`: the *list* of outcomes,
- `n`: the total number of events,
- `p`: the probability of the outcome we're interested in seeing.

Because we only need two parameters to describe a distribution, it doesn't matter whether we want to know if it will be sunny `5` days out of `5`, or if `5` out of `5` coin flips will turn up heads. As long as the outcome that we care about has the *same probability* (p), and N is the same, the binomial distribution will look the same.

### Instructions

- Generate a binomial distribution, and then find the probabilities for each value in `outcome_counts`.
  - Use `N=30`, and `p=.39`, as we're doing this for the bikesharing data.
- Plot the resulting data as a bar chart.

```
import scipy
from scipy import linspace
from scipy.stats import binom

# Create a range of numbers from 0 to 30, with 31 elements (each number has one entry).
outcome_counts = linspace(0,30,31)
# Create the binomial distribution.
outcome_probs = binom.pmf(outcome_counts,30,0.39)
plt.bar(outcome_counts, outcome_probs)
plt.show()
```

## 7: How To Think About A Probability Distribution

Looking at a probability distribution might not be extremely intuitive. One way to think about it is that "if we repeatedly look at samples, the expected number of outcomes will follow the probability distribution".

If we repeatedly look at 30 days of bikesharing data, we'll find that `10` of the days had more than `5000` riders about `12.4%` of the time. We'll find that `12` of the days had more than `5000` riders about `14.6%` of the time.

A probability distribution is a great way to visualize data, but bear in mind that it's not dealing in absolute values. A probability distribution can only tell us which values are likely, and how likely they are.

## 8: Computing The Mean Of A Probability Distribution

Sometimes we'll want to be able to tell people the expected value of a probability distribution -- the most likely result of a single sample that we look at.

To compute this, we just multiply N by p.

### Instructions

- Compute the mean for the bikesharing data, where N=30, and p=.39.Assign the result to `dist_mean`.

```
dist_mean = None
dist_mean = 30 * .39
```

## 9: Computing The Standard Deviation

Just as we can compute the mean, we can also compute the standard deviation of a probability distribution. This helps us find how much the actual values will vary from the mean when we take a sample.

Going back to the bikesharing example, we know that the actual values will be around `11.7` (from the last screen). But, we'll need a standard deviation to explain how much the actual values can vary from this expectation.

The formula for standard deviation of a probability distribution is:

N∗p∗q

### Instructions

- Compute the standard deviation for the bikesharing data, where N=30, and p=.39.Assign the result to `dist_stdev`.

```
dist_stdev = None
import math
dist_stdev = math.sqrt(30 * .39 * .61)
```

## 10: A Different Plot

Just like we did with histograms and sampling a few missions ago, we can vary the parameters to change the distribution. Let's see what the plot would look like with only `10` events, or `100` events.

### Instructions

- Generate a binomial distribution, with `N=10`, and `p=.39`.
  - Find the probabilities for each value in `outcome_counts`.
  - Plot the resulting data as a bar chart.
- Generate a binomial distribution, with `N=100`, and `p=.39`.
  - Find the probabilities for each value in `outcome_counts`.
  - Plot the resulting data as a bar chart.

```
# Enter your answer here.
outcome_counts = linspace(0,10,11)
outcome_probs = binom.pmf(outcome_counts,10,0.39)
plt.bar(outcome_counts, outcome_probs)
plt.show()

outcome_counts = linspace(0,100,101)
outcome_probs = binom.pmf(outcome_counts,100,0.39)
plt.bar(outcome_counts, outcome_probs)
plt.show()
```

## 12: Cumulative Density Function

So far, we've looked at the probability that single values of `k` will occur. What we can look at instead is the probability that `k` or less will occur. These probabilities can be generated by the [cumulative density function](https://en.wikipedia.org/wiki/Binomial_distribution).

Let's say we flip a coin `3` times -- N=3, and p=.5. When this happens, here are the probabilities:

```
k    probability
0    .125
1    .375
2    .375
3    .125
```

A cumulative distribution would look like this:

```
k    probability
0    .125
1    .5
2    .875
3    1
```

For each `k`, we fill in the probability that we'll see `k` outcomes *or less*. By the end of the distribution, we should get `1`, because all the probabilities add to `1` (if we flip `3` coins, either `0`, `1`, `2`, or `3`of them must be heads).

We can calculate this with `binom.cdf` in scipy.

```
from scipy import linspace
from scipy.stats import binom

# Create a range of numbers from 0 to 30, with 31 elements (each number has one entry).
outcome_counts = linspace(0,30,31)
# Create the cumulative binomial probabilities, one for each entry in outcome_counts.
dist = binom.cdf(outcome_counts,30,0.39)
```

### Instructions

- Create a cumulative distribution where N=30 and p=.39 and generate a line plot of the distribution.

```
outcome_counts = linspace(0,30,31)
outcome_probs = binom.cdf(outcome_counts,30,0.39)
plt.plot(outcome_counts, outcome_probs)
plt.show()
```

## 13: Calculating Z-Scores

We can calculate z-scores (the number of standard deviations away from the mean a probability is) fairly easily. These z-scores can then be used how we used z-scores earlier -- to find the percentage of values to the left and right of the value we're looking at.

To make this more concrete, say we had `16` days where we observed more than `5000` riders. Is this likely? Unlikely? Using a z-score, we can figure out exactly how common this event is.

This is because every normal distribution, as we learned in an earlier mission, has the same properties when it comes to what percentage of the data is within a certain number of standard deviations of the mean. You can look these up in a [standard normal table](https://en.wikipedia.org/wiki/Standard_normal_table). About `68%` of the data is within `1` standard deviation of the mean, `95%` is within `2`, and `99%` is within `3`.

We can calculate the mean (μ) and standard deviation (σ) of a binomial probability distribution using the formulas from earlier:

μ=N∗p

σ=N∗p∗q

If we want to figure out the number of standard deviations from the mean a value is, we just do:

k−μσ

If we wanted to find how many standard deviations from the mean `16` days is:

16−μσ=16−(30∗.39)16∗.39∗.61=4.31.95=2.2

This tells us that `97.22%` of the data is within `2.2` standard deviations of the mean, so so a result to be as different from the mean as this, there is a `2.78%` probability that it occurred by chance.

Note that this means both "sides" of the distribution. There's a `1.39%` chance that a value is `2.2` standard deviations or more above the mean (to the right of the mean), and there's a `1.39%` chance that a value is `2.2`standard devitation below the mean (to the left).

## 14: Faster Way To Calculate Likelihood

We don't want to have to use a z-score table every time we want to see how likely or unlikely a probability is. A much faster way is to use the cumulative distribution fuction (`cdf`) like we did earlier. This won't give us the exact same values as using z-scores, because the distribution isn't exactly normal, but it will give us the actual amount of probability in a distribution to the left of a given `k`.

To use it, we can run:

```
# The sum of all the probabilities to the left of k, including k.
left = binom.cdf(k,N,p)
# The sum of all probabilities to the right of k.
right = 1 - left
```

This will return the sum of all the probabilities to the left of and including `k`. If we subtract this value from `1`, we get the sum of all the probabilities to the right of `k`.

### Instructions

- Find the probability to the left of k=16 (including `16`) when N=30 and p=.39.
  - Assign the result to `left_16`.
- Find the probability to the right of k=16 when N=30 and p=.39.
  - Assign the result to `right_16`.

```
left_16 = None
right_16 = None
left_16 = binom.cdf(16,30,0.39)
right_16 = 1 - left_16
```

# Significance Testing

## 1: Hypothesis Testing

In this mission, we'll learn about hypothesis testing and statistical significance. A hypothesis is a pattern or rule about a process in the world that can be tested. We use hypothesis testing to determine if a change we made had a meaningful impact or not.

You can use hypothesis testing to help you determine:

- if a new banner ad on a website caused a meaningful drop in the user engagement,
- if raising the price of a product caused a meaningful drop in sales,
- if a new weight loss pill helped people lose more weight.

Observing a decrease in user engagement or sales after instituting a change doesn't automatically imply that the change was the cause. Hypothesis testing allows us to calculate the probability that random chance was actually responsible for the difference in outcome. Every process has some inherent amount of randomness that we can't measure and understanding the role of chance helps us reach a conclusion that's more likely to be correct.

We first set up a **null hypothesis** that describes the status quo. We then state an **alternative hypothesis**, which we used to compare with the null hypothesis to decide which describes the data better. In the end, we either need to:

- reject the null hypothesis and accept the alternative hypothesis or
- accept the null hypothesis and reject the alternative hypothesis.

We can frame each of the studies above as these rival pairs of hypotheses:

- if a new banner ad on a website caused a meaningful drop in the user engagement:
  - null hypothesis: users who were exposed to the banner ad spent the same amount of time on the website than those who weren't.
  - alternative hypothesis: users who were exposed to the banner ad spent less time on the website than those who weren't.
- if raising the price of a product caused a meaningful drop in sales:
  - null hypothesis: the number of purchases of the product was the same at the lower price than it was at the higher price.
  - alternative hypothesis: the number of purchases of the product was lower at the higher price than it was at the lower price.
- if a new weight loss pill helped people lose more weight:
  - null hypothesis: patients who went on the weight loss pill lost no more weight than those who didn't.
  - alternative hypothesis: patients who went on the weight loss pill lost more weight than those who didn't.

In the rest of this mission, we'll focus on the third scenario and use data to determine if a weight loss pill helped people lose weight.

## 2: Research Design

To help us determine if the weight loss pill was effective, we conducted a study where we invited 100 volunteers and split them into 2 even groups randomly:

- Group A was given a placebo, or fake, pill and instructed to consume it on a daily basis.
- Group B was given the actual weight loss pill and instructed to consume it on a daily basis.

Both groups were instructed to change nothing else about their diets. Group A is referred to as the control group while group B is referred to as the treatment group. This type of study is called a [blind experiment](https://en.wikipedia.org/wiki/Blind_experiment) since the participants didn't know which pill they were receiving. This helps us reduce the potential bias that is introduced when participants know which pill they were given. For example, participants who are aware they were given the weight loss pill may try to add healthier foods to their diet to help them lose more weight. Both groups were weighed before the study began and a month later, after the study ended.

Understanding the **research design** for a study is an important first step that informs the rest of your analysis. It helps us uncover potential flaws in the study that we need to keep in mind as we dive deeper. The weight loss pill study we conducted is known as an experimental study. Experimental studies usually involve bringing in participants, instructing them to perform some tasks, and observing them. A key part of running an experimental study is random assignment, which involves assigning participants in the study to random groups without revealing which group each participant is in. Before exploring and analyzing a dataset, it's important to understand how the study was conducted. Flaws in how the study was run can lead you to reach the wrong conclusions.

## 3: Statistical Significance

Statistics helps us determine if the difference in the weight lost between the 2 groups is because of random chance or because of an actual difference in the outcomes. If there is a meaningful difference, we say that the results are **statistically significant**. We'll dive into what this means exactly over the course of this mission.

Now that we're familiar with the study, let's state our null and alternative hypotheses more precisely. Our null hypothesis should describe the default position of skepticism, which is that there's no statistically significant difference between the outcomes of the 2 groups. Put another way, it should state that any difference is due to random chance. Our alternative hypothesis should state that there is in fact a statistically significant difference between the outcomes of the 2 groups.

- Null hypothesis: participants who consumed the weight loss pills lost the same amount of weight as those who didn't take the pill.
- Alternative hypothesis: participants who consumed the weight loss pills lost more weight than those who didn't take the pill.

The *lists* `weight_lost_a` and `weight_lost_b` contain the amount of weight (in pounds) that the participants in each group lost. Let's now explore the data to become more familiar with it.

### Instructions

- Use the [NumPy function `mean()`](http://docs.scipy.org/doc/numpy-1.10.0/reference/generated/numpy.mean.html) to calculate:
  - The mean of the weight lost by participants in group A. Assign to `mean_group_a` and display it using the `print()` function.
  - The mean of the weight lost by participants in group B. Assign to `mean_group_b` and display it using the `print()` function.
- Use the [Matplotlib function `hist()`](http://matplotlib.org/api/pyplot_api.html#matplotlib.pyplot.hist) to plot histograms for both `weight_lost_a` and `weight_lost_b`.

```
import numpy as np
import matplotlib.pyplot as plt
mean_group_a = np.mean(weight_lost_a)
mean_group_b = np.mean(weight_lost_b)

plt.hist(weight_lost_a)
plt.show()
plt.hist(weight_lost_b)
plt.show()
```

## 4: Test Statistic

To decide which hypothesis more accurately describes the data, we need to frame the hypotheses more quantitatively. The first step is to decide on a **test statistic**, which is a numerical value that summarizes the data and we can use in statistical formulas. We use this test statistic to run a statistical test that will determine how likely the difference between the groups were due to random chance.

Since we want to know if the amount of weight lost between the groups is meaningfully different, we will use the difference in the means, also known as the mean difference, of the amount of weight lost for each group as the test statistic.

The following symbol is used to represent the sample mean in statistics:

x¯

We'll use:

x¯a

to denote the mean of group A and

x¯b

to denote the mean of group B. For the mean difference, we'll subtract the mean of group A from group B:

x¯b−x¯a

Now that we have decided on a test statistic, we can rewrite our hypotheses to be more precise:

Null hypothesis: x¯b−x¯a=0

Alternative hypothesis: x¯b−x¯a>0

Note that while we've stated our hypotheses as equations, we're not simply calculating the difference and matching the result to hypothesis. We're instead using a statistical test to determine which of these statements better describes the data.

### Instructions

- Calculate the observed test statistic by subtracting `mean_group_a` from `mean_group_b` and assign to `mean_difference`.
- Display `mean_difference` using the `print()` function.

```
mean_difference = mean_group_b - mean_group_a
print(mean_difference)
```

## 5: Permutation Test

Now that we have a test statistic, we need to decide on a statistical test. The purpose of a statistical test is to work out the likelihood that the result we achieved was due to random chance.

The **permutation test** is a statistical test that involves simulating rerunning the study many times and recalculating the test statistic for each iteration. The goal is to calculate a distribution of the test statistics over these many iterations. This distribution is called the **sampling distribution** and it approximates the full range of possible test statistics under the null hypothesis. We can then benchmark the test statistic we observed in the data (a mean difference of `2.52`) to determine how likely it is to observe this mean difference under the null hypothesis. If the null hypothesis is true, that the weight loss pill doesn't help people lose more weight, than the observed mean difference of `2.52` should be quite common in the sampling distribution. If it's instead extremely rare, then we accept the alternative hypothesis instead.

To simulate rerunning the study, we randomly reassign each data point (weight lost) to either group A or group B. We keep track of the recalculated test statistics as a separate *list*. By re-randomizing the groups that the weight loss values belong to, we're simulating what randomly generated groupings of these weight loss values would look like. We then use these randomly generated groupings to understand how rare the groupings in our actual data were.

Ideally, the number of times we re-randomize the groups that each data point belongs to matches the total number of possible permutations. Usually, the number of total permutations is too high for even powerful supercomputers to calculate within a reasonable time frame. While we'll use 1000 iterations for now since we'll get the results back quickly, in later missions we'll learn how to quantify the tradeoff we make between accuracy and speed to determine the optimal number of iterations.

Since we'll be randomizing the groups each value belongs to, we created a *list* named `all_values` that contains just the weight loss values.

### Instructions

- Create an empty *list* named `mean_differences`.
- Inside a `for` loop that repeats `1000` times:Assign empty *lists* to the variables `group_a` and `group_b`.Inside a `for` loop that iterates over `all_values`:Use the [`numpy.random.rand()` function](http://docs.scipy.org/doc/numpy/reference/generated/numpy.random.rand.html#numpy.random.rand) to generate a value between `0` and `1`.If the random value is larger than or equal to `0.5`, assign that weight loss value to group A by using the `append()` method to append it to `group_a`.If the random value is less than `0.5`, assign that weight loss value to group B by using the `append()` method to append it to `group_b`.Outside the `for` loop that iterates over `all_values`:Use the [`numpy.mean()` function](http://docs.scipy.org/doc/numpy-1.10.0/reference/generated/numpy.mean.html) to calculate the means of `group_a` and `group_b`.Subtract the mean of group A from group B and assign the result to `iteration_mean_difference`.Append `iteration_mean_difference` to `mean_differences` using the `append()`method.
- Use `plt.hist()` to generate a histogram of `mean_differences`.

```
mean_difference = 2.52
print(all_values)
mean_differences = []
for i in range(1000):
    group_a = []
    group_b = []
    for value in all_values:
        assignment_chance = np.random.rand()
        if assignment_chance >= 0.5:
            group_a.append(value)
        else:
            group_b.append(value)
    iteration_mean_difference = np.mean(group_b) - np.mean(group_a)
    mean_differences.append(iteration_mean_difference)
    
plt.hist(mean_differences)
plt.show()
```

## 6: Sampling Distribution

By randomly assigning participants to group A or group B, we account for the effect of random chance. Someone in group B who just happened to lose more weight (but not because of the pills) makes the results look better than they were. By creating many permutations, we're able to see all possible configurations of this error. Creating a histogram enables us to see how likely different values of our test statistic are if we repeated our experiment many times.

The histogram we generated in the previous step using Matplotlib is a visual representation of the sampling distribution. Let's now create a *dictionary* that contains the values in the sampling distribution so we can benchmark our observed test statistic against it.

The keys in the *dictionary* should be the test statistic and the values should be their frequency:

```
{
    0.34943639291465356: 3,
    -0.55702280912364888: 2, 
    -0.14942528735632177: 1
    ....
}
```

We need to first count up how frequently each value in the *list*, `mean_differences`, occurs. As we loop over `mean_differences`, we need a way to check if the test statistic is already in our *dictionary*:

- If it is, we look up the value at that key, add `1` to it, and assign the new value to the key.
- If it isn't, we add the key to the dictinoary and assign the value `1` to it.

We'll dive more into how to accomplish this in the next step.

## 7: Dictionary Representation Of A Distribution

To check if a key exists in a *dictionary*, we need to use the [`get()` method](https://docs.python.org/3/library/stdtypes.html#dict.get) to:

- return the value at the specified key if it exists in the dictionary or
- return another value we specify instead.

Here are the parameters the method takes in:

- the required parameter is the key we want to look up,
- the optional parameter is the default value we want returned if the key is not found.

In the following code block, we use the `get` method and set the default value to `False`:

```
empty = {}

# Since "a" isn't a key in empty, the value False is returned.
key_a = empty.get("a", False):

empty["b"] = "boat"

# key_b is the value for the key "b" in empty.
key_b = empty.get("b", False):
# "boat" is assigned to key_b.
```

We can use the value returned from the `get()` method in an `if` statement to express our logic:

```
empty = {"c": 1}
if empty.get("c", False):
    # If in the dictionary, grab the value, increment by 1, reassign.
    val = empty.get("c")
    inc = val + 1
    empty["c"] = inc
else:
    # If not in the dictionary, assign `1` as the value to that key.
    empty["c"] = 1
```

### Instructions

- Create an empty *dictionary* called `sampling_distribution`whose keys will be the test statistics and whose values are the frequencies of the test statistics.Inside a `for` loop that iterates over `mean_differences`, check if each value exists as a key in `sampling_distribution`:Use the [dictionary method `get()`](https://docs.python.org/3/library/stdtypes.html#dict.get) with a default condition of `False` to check if the current iteration's value is already in `sampling_distribution`.If it is, increment the existing value in `sampling_distribution` for that key by `1`.If it isn't, add it to `sampling_distribution` as a key and assign `1` as the value.

```
sampling_distribution = {}
for df in mean_differences:
    if sampling_distribution.get(df, False):
        sampling_distribution[df] = sampling_distribution[df] + 1
    else:
        sampling_distribution[df] = 1
```

## 8: P Value

In the sampling distribution we generated, most of the values are closely centered around the mean difference of `0`. This means that if it were purely up to chance, both groups would have lost the same amount of weight (the null hypothesis). But since the observed test statistic is not near `0`, it could mean that the weight loss pills could be responsible for the mean difference in the study.

We can now use the sampling distribution to determine the number of times a value of `2.52` or higher appeared in our simulations. If we then divide that frequency by `1000`, we'll have the probability of observing a mean difference of `2.52` or higher purely due to random chance.

This probability is called the **p value**. If this value is high, it means that the difference in the amount of weight both groups lost could have *easily* happened randomly and the weight loss pills probably didn't play a role. On the other hand, a low p value implies that there's an incredibly small probability that the mean difference we observed was because of random chance.

In general, it's good practice to set the **p value threshold** before conducting the study:

- if the p value is less than the threshold, we:
  - reject the null hypothesis that there's no difference in mean amount of weight lost by participants in both groups,
  - accept the alternative hypothesis that the people who consumed the weight loss pill lost more weight,
  - conclude that the weight loss pill does affect the amount of weight people lost.
- if the p value is greater than the threshold, we:
  - accept the null hypothesis that there's no difference in the mean amount of weight lost by participants in both groups,
  - reject the alternative hypothesis that the people who consumed the weight loss pill lost more weight,
  - conclude that the weight loss pill doesn't seem to be effective in helping people lose more weight.

The most common p value threshold is `0.05` or 5%, which is what we'll use in this mission. Although .05 is an arbitrary threshold, it means that there's only a 5% chance that the results are due to random chance, which most researchers are comfortable with.

### Instructions

- Create an empty *list* named `frequencies`.
- Inside a `for` loop that iterates over the keys in `sampling_distribution`:If the key is `2.52` or larger, add its value to `frequencies` (and do nothing if it isn't).
- Outside the `for` loop, use the [NumPy function `sum()`](http://docs.scipy.org/doc/numpy-1.10.1/reference/generated/numpy.sum.html) to calculate the sum of the values in `frequencies`.
- Divide the sum by `1000` and assign to `p_value`.

```
frequencies = []
for sp in sampling_distribution.keys():
    if sp >= 2.52:
        frequencies.append(sampling_distribution[sp])
p_value = np.sum(frequencies) / 1000
```

## 9: Caveats

Since the p value of `0` is less than the threshold we set of `0.05`, we conclude that the difference in weight lost can't be attributed to random chance alone. We therefore reject the null hypothesis and accept the alternative hypothesis. A few caveats:

- Research design is incredibly important and can bias your results. For example, if the participants in group A realized they were given placebo sugar pills, they may modify their behavior and affect the outcome.
- The p value threshold you set can also affect the conclusion you reach.
  - If you set too high of a p value threshold, you may accept the alternative hypothesis incorrectly and reject the null hypothesis. This is known as a [**type I error**](https://en.wikipedia.org/wiki/Type_I_and_type_II_errors).
  - If you set too low of a p value threshold, you may reject the alternative hypothesis incorrectly in favor of accepting the null hypothesis. This is known as a [**type II error**](https://en.wikipedia.org/wiki/Type_I_and_type_II_errors).

## 10: Next Steps

In this mission, we explored hypothesis testing and the basics of statistical significance using a study that closely represents real world studies. In the next mission, we'll learn about the chi squared distribution and the chi squared test.

# Chi-squared tests

## 1: Observed And Expected Frequencies

In this mission, we'll be learning about the chi-squared test for categorical data. This test enables us to determine the statistical significance of observing a set of categorical values.

We'll be working with data on US income and demographics throughout this mission. Here are the first few rows of the data, in *csv* format:

```
age,workclass,fnlwgt,education,education_num,marital_status,occupation,relationship,race,sex,capital_gain,capital_loss,hours_per_week,native_country,high_income
39, State-gov, 77516, Bachelors, 13, Never-married, Adm-clerical, Not-in-family, White, Male, 2174, 0, 40, United-States, <=50K
50, Self-emp-not-inc, 83311, Bachelors, 13, Married-civ-spouse, Exec-managerial, Husband, White, Male, 0, 0, 13, United-States, <=50K
38, Private, 215646, HS-grad, 9, Divorced, Handlers-cleaners, Not-in-family, White, Male, 0, 0, 40, United-States, <=50K
```

Each row represents a single person who was counted in the `1990` US Census, and contains information about their income and demograpics. Here are some of the relevant columns:

- `age` -- how old the person is
- `workclass` -- the type of sector the person is employed in.
- `race` -- the race of the person.
- `sex` -- the gender of the person, either `Male` or `Female`.

The entire dataset has `32561` rows, and is a sample of the full Census. Of the rows, `10771` are `Female`, and `21790` are `Male`. These numbers look a bit off, because the full Census shows that the US is about `50%` Male and `50%` Female. So our expected values for number of Males and Females would be `16280.5` each.

Here's a diagram:

|          | Male    | Female  | Total |
| -------- | ------- | ------- | ----- |
| Observed | 21790   | 10771   | 32561 |
| Expected | 16280.5 | 16280.5 | 32561 |

We know that something looks off, but we don't quite know how to quantify how different the observed and expected values are. We also don't have any way to determine if there's a statistically significant difference between the two groups, and if we need to investigate further.

This is where a chi-squared test can help. The chi-squared test enables us to quantify the difference between sets of observed and expected categorical values.

## 2: Calculating Differences

One way that we can determine the differences between observed and expected values is to compute simple proportional differences.

Let's say an expected value is `1000`, and the observed value is `1100`. We can compute the proportional difference with:

(observed−expected)/expected=(1100−1000)/1000=.1

So there's a `.1`, or `10%`, difference between our observed and expected values.

### Instructions

In the last screen, our observed values were `10771` `Females`, and `21790` `Males`. Our expected values were `16280.5` `Females`and `16280.5` `Males`.

- Compute the proportional difference in number of observed `Females` vs number of expected `Females`. Assign the result to `female_diff`.
- Compute the proportional difference in number of observed `Males` vs number of expected `Males`. Assign the result to `male_diff`.

```

female_diff = (10771 - 16280.5) / 16280.5
male_diff = (21790 - 16280.5) / 16280.5
```

## 3: Updating The Formula

In the last screen, we got `-0.338` for the `Female` difference, and `0.338` for the `Male` difference. These are great for finding individual differences for each category, but since both values add up to `0`, they don't give us a meaningful measure of how our overall observed counts deviate from the expected counts.

You may recall this diagram from earlier:

|          | Male    | Female  | Total |
| -------- | ------- | ------- | ----- |
| Observed | 21790   | 10771   | 32561 |
| Expected | 16280.5 | 16280.5 | 32561 |

No matter what numbers you plug in for observed `Male` or `Female` counts, the differences between observed and expected will always add to `0`, because the total observed count for `Male` and `Female`items always comes out to `32561`. If the observed count of `Females` is high, the count of `Males` has to be low to compensate, and vice versa.

What we really want to find is one number that can tell us how much all of our observed counts deviate from all of their expected counterparts. This will let us figure out if our difference in counts is statistically significant. We can get one step closer to this by squaring the top term in our difference formula:

(observed−expected)^2/expected=(1100−1000)^2/1000=10

Squaring the difference will ensure that all the differences don't sum to zero (you can't have negative squares), giving us a non-zero number we can use to assess statistical significance.

We can calculate χ2, the chi-squared value, by adding up all of the squared differences between observed and expected values.

### Instructions

In the last screen, our observed values were `10771` `Females`, and `21790` `Males`. Our expected values were `16280.5` `Females`and `16280.5` `Males`.

- Compute the difference in number of observed `Females` vs number of expected `Females` using the updated technique. Assign the result to `female_diff`.
- Compute the difference in number of observed `Males` vs number of expected `Males` using the updated technique. Assign the result to `male_diff`.
- Add `male_diff` and `female_diff` together and assign to the variable `gender_chisq`.

```

female_diff = (10771 - 16280.5) ** 2 / 16280.5
male_diff = (21790 - 16280.5) ** 2 / 16280.5
gender_chisq = female_diff + male_diff
```

## 4: Generating A Distribution

Now that we have a chi-squared value for our observed and expected gender counts, we need a way to figure out what the chi-squared value represents. We can translate a chi-squared value into a statistical significance value using a chi-squared sampling distribution. If you recall, we covered statistical significance and p-values in the last mission. A p-value allows us to determine whether the difference between two values is due to chance, or due to an underlying difference.

We can generate a chi-squared sampling distribution using our expected probabilities. If we repeatedly generate random samples that contain `32561`samples, and graph the chi-squared value of each sample, we'll be able to generate a distribution. Here's a rough algorithm:

- Randomly generate `32561` numbers that range from `0-1`.
- Based on the expected probabilities, assign `Male` or `Female` to each number.
- Compute the observed frequences of `Male` and `Female`.
- Compute the chi-squared value and save it.
- Repeat several times.
- Create a histogram of all the chi-squared values.

By comparing our chi-squared value to the distribution, and seeing what percentage of the distribution is greater than our value, we'll get a p-value. For instance, if `5%` of the values in the distribution are greater than our chi-squared value, the p-value is `.05`.

### Instructions

Inside a for loop that repeats `1000` times:

- Use the [numpy.random.random](http://docs.scipy.org/doc/numpy/reference/generated/numpy.random.random.html#numpy.random.random) function to generate `32561` numbers between `0.0` and `1.0`.
- Pass `(32561,)` into the [numpy.random.random](http://docs.scipy.org/doc/numpy/reference/generated/numpy.random.random.html#numpy.random.random) function to get a vector with `32561` elements.
  - For each of the numbers, if it is less than `.5`, replace it with `0`, otherwise replace it with `1`.
  - Count up how many times `0` occurs (`Male`frequency), and how many times `1` occurs (`Female`frequency).
  - Use the expected frequencies from earlier to compute the chi-squared value.
    - Compute `male_diff` by subtracting the expected `Male` count from the observed `Male`count, squaring it, and dividing by the expected `Male` count.
    - Compute `female_diff` by subtracting the expected `Female` count from the observed `Female` count, squaring it, and dividing by the expected `Female` count.
    - Add up `male_diff` and `female_diff` to get the chi-squared vlaue.
- Append the chi-squared value to `chi_squared_values`.
- Create a histogram with `chi_squared_values` using the [plt.hist](http://matplotlib.org/api/pyplot_api.html#matplotlib.pyplot.hist) method.

```
chi_squared_values = []
from numpy.random import random
import matplotlib.pyplot as plt

for i in range(1000):
    sequence = random((32561,))
    sequence[sequence < .5] = 0
    sequence[sequence >= .5] = 1
    male_count = len(sequence[sequence == 0])
    female_count = len(sequence[sequence == 1])
    male_diff = (male_count - 16280.5) ** 2 / 16280.5
    female_diff = (female_count - 16280.5) ** 2 / 16280.5
    chi_squared = male_diff + female_diff
    chi_squared_values.append(chi_squared)

plt.hist(chi_squared_values)
```

## 5: Statistical Significance

In the last screen, our calculated chi-squared value is greater than all the values in the distribution, so our p-value is `0`, indicating that our result is statistically significant. You may recall from the last mission that `.05`is the typical threshold for statistical significance, and anything below it is considered significant.

A significant value indicates that something is different between the observed and expected values, but it doesn't indicate what is different.

Now that we have a chi-squared sampling distribution, we can compare the chi-squared value we calculated for our data to it to see if our result is statistically significant. The chi-squared value we calculated was `3728.95`. The highest value in the chi-squared sampling distribution was about `12`. This means that our chi-squared value is higher than `100%` of all the values in the sampling distribution, so we get a p-value of `0`. This means that there is a `0%` chance that we could get such a result randomly.

This would indicate that we need to investigate our data collection techniques more closely to figure out why such a result occurred.

Because a chi-squared value has no sign (all chi-squared values are positive), it doesn't tell us anything about the direction of the statistical significance. If we had `10771` `Females`, and `21790` `Males`, or `10771` `Males`, and `21790` `Females`, we'd get the same chi-squared value. It's important to look at the data and see how the data is unbalanced after calculating a chi-squared value and getting a significant result.

## 6: Smaller Samples

One interesting thing about chi-squared values is that they get smaller as the sample size decreases. For example, with our `Male` and `Female` example, let's say we only have `100` rows, but the same observed and expected proportions:

MaleFemaleTotalObserved66.9233.08100Expected5050100

We can compute the chi-squared value for this:

(observedM−expectedM)2expectedM+(observedF−expectedF)2expectedF=(66.92−50)250+(33−50)250=5.726+5.726=11.4522

`32561` (our original number of rows) divided by `100` (our new number of rows) is `325.61`. If we multiply `11.4522` by `325.61`, we get `3728.95`, which is the exact same chi-squared value that we got two screens ago.

So as sample size changes, the chi-squared value changes proportionally.

### Instructions

Let's say our observed values are `107.71` `Females`, and `217.90``Males`. Our expected values are `162.805` `Females` and `162.805``Males`.

- Compute the difference in number of observed `Females` vs number of expected `Females` using the new formula. Assign the result to `female_diff`.
- Compute the difference in number of observed `Males` vs number of expected `Males` using the new formula. Assign the result to `male_diff`.
- Add `male_diff` and `female_diff` together and assign to the variable `gender_chisq`.

```
female_diff = (107.71 - 162.805) ** 2 / 162.805
male_diff = (217.90 - 162.805) ** 2 / 162.805
gender_chisq = female_diff + male_diff
```

## 7: Sampling Distribution Equality

As sample sizes get larger, seeing large deviations from the expected probabilities gets less and less likely. For example, if you're flipping a coin `10` times, you wouldn't be surprised to see this:

HeadsTailsTotalObserved8210Expected5510

This is a fairly skewed result, but in a small sample size, random chance can create effects like this. It would be very surprising to see this after flipping a coin `1000` times, though:

HeadsTailsTotalObserved8002001000Expected5005001000

A result like this would probably make you check the coin to see if it's a trick coin or weighted improperly.

The chi-squared value follows the same principle. Chi-squared values for the same sized effect increase as sample size increases, but the chance of getting a high chi-squared value decreases as the sample gets larger.

These two effects offset each other, and a chi-squared sampling distribution constructed when sampling `200` items for each iteration will look identical to one sampling `1000` items.

This enables us to easily compare any chi-squared value to a master sampling distribution to determine statistical significance, no matter what sample size the chi-squared value was created with.

### Instructions

Inside a for loop that repeats `1000` times:

- Use the [numpy.random.random](http://docs.scipy.org/doc/numpy/reference/generated/numpy.random.random.html#numpy.random.random) function to generate `300`numbers between `0.0` and `1.0`.
- Pass `(300,)` into the [numpy.random.random](http://docs.scipy.org/doc/numpy/reference/generated/numpy.random.random.html#numpy.random.random) function to get a vector with `300` elements.For each of the numbers, if it is less than `.5`, replace it with `0`, otherwise replace it with `1`.Count up how many times `0` occurs (`Male`frequency), and how many times `1` occurs (`Female`frequency).Use the expected frequencies from earlier to compute the chi-squared value.Compute `male_diff` by subtracting the expected `Male` count (`150`) from the observed `Male` count, squaring it, and dividing by the expected `Male` count.Compute `female_diff` by subtracting the expected `Female` count (`150`) from the observed `Female` count, squaring it, and dividing by the expected `Female` count.Add up `male_diff` and `female_diff` to get the chi-squared vlaue.Append the chi-squared value to `chi_squared_values`.
- Create a histogram with `chi_squared_values` using the [plt.hist](http://matplotlib.org/api/pyplot_api.html#matplotlib.pyplot.hist) method.
- This plot should look identical to the one you generated earlier.

```
chi_squared_values = []
from numpy.random import random
import matplotlib.pyplot as plt

for i in range(1000):
    sequence = random((300,))
    sequence[sequence < .5] = 0
    sequence[sequence >= .5] = 1
    male_count = len(sequence[sequence == 0])
    female_count = len(sequence[sequence == 1])
    male_diff = (male_count - 150) ** 2 / 150
    female_diff = (female_count - 150) ** 2 / 150
    chi_squared = male_diff + female_diff
    chi_squared_values.append(chi_squared)

plt.hist(chi_squared_values)
```

## 8: Degrees Of Freedom

When we were computing the chi-squared value earlier, we were working with `2` values that could be vary, the number of `Males` and the number of `Females`. But actually, only `1` of the values could vary. Since we already know the total number of values, `32561`, if we set one of the values, the other has to be the difference between `32561` and the value we set.

The diagram from earlier might clarify this:

MaleFemaleTotalObserved217901077132561Expected16280.516280.532561

If we set a count for `Male` or `Female`, we know what the other value has to be, because they both need to add up to `32561`.

A degree of freedom is the number of values that can vary without the other values being "locked in". In the case of our two categories, there is actually only one degree of freedom. Degrees of freedom are an important statistical concept that will come up repeatedly, both in this mission and after.

## 9: Increasing Degrees Of Freedom

So far, we've only calculated chi-squared values for `2` categories and `1` degree of freedom. We can actually work with any number of categories, and any number of degrees of freedom. We can accomplish this using largely the same formula we've been using, but we will need to generate new sampling distributions for each number of degrees of freedom.

If we look at the `race` column of the income data, the possible values are `White`, `Black`, `Asian-Pac-Islander`, `Amer-Indian-Eskimo`, and `Other`.

We can get our expected proportions straight from the full 1990 US Census:

- `White` -- `80.3%`
- `Black` -- `12.1%`
- `Asian-Pac-Islander` -- `2.9%`
- `Amer-Indian-Eskimo` -- `.8%`
- `Other` -- `3.9%`

Here's a table showing expected and actual values for our income dataset:

AmerWhiteBlackAsianIndianOtherTotalObserved278163124103931127132561Expected26146.53939.9944.3260.51269.832561

It looks like there's a discrepancy between the `White` and `Other` counts, but let's dig in a bit more and calculate the chi-squared value.

### Instructions

- For each category (`White`, `Black`, `Asian-Pac-Islander`, `Amer-Indian-Eskimo`, and `Other`):compute the difference between the expected and observed counts,square the difference,divide by the expected value,append each result to a list,sum the values in the list and assign the result to `race_chisq`.

```

diffs = []
observed = [27816, 3124, 1039, 311, 271]
expected = [26146.5, 3939.9, 944.3, 260.5, 1269.8]

for i, obs in enumerate(observed):
    exp = expected[i]
    diff = (obs - exp) ** 2 / exp
    diffs.append(diff)
    
race_chisq = sum(diffs)
```

## 10: Using SciPy

Rather than constructing another chi-squared sampling distribution for `4`degrees of freedom, we can use a function from the [SciPy](http://www.scipy.org/) library to do it more quickly.

The [scipy.stats.chisquare](http://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.mstats.chisquare.html) function takes in an array of observed frequences, and an array of expected frequencies, and returns a tuple containing both the chi-squared value and the matching p-value that we can use to check for statistical significance.

Here's a usage example:

```
import numpy as np
from scipy.stats import chisquare

observed = np.array([5, 10, 15])
expected = np.array([7, 11, 12])
chisquare_value, pvalue = chisquare(observed, expected)
```

The [scipy.stats.chisquare](http://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.mstats.chisquare.html) function returns a list, so we can assign each item in the list to a separate variable using 2 variable names separated with a comma, like you see above.

### Instructions

- Use the [scipy.stats.chisquare](http://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.mstats.chisquare.html) function to calculate the p-value for the following table:

AmerWhiteBlackAsianIndianOtherTotalObserved278163124103931127132561Expected26146.53939.9944.3260.51269.832561

- Assign the result to `race_pvalue`.

```
from scipy.stats import chisquare
import numpy as np
observed = np.array([27816, 3124, 1039, 311, 271])
expected = np.array([26146.5, 3939.9, 944.3, 260.5, 1269.8])

chisquare_value, race_pvalue = chisquare(observed, expected)
```

## 11: Next Steps

In this mission, we introduced the chi-squared test, and showed how to use it to tell if observed and expected categorical frequency data differs significantly. In the next mission, we'll explore how to determine significance in the case of multiple categories.

# Multi category chi-squared tests

## 1: Multiple Categories

In the last mission, we looked at the gender frequencies of people included in a data set on US income. The dataset consists of `32561` rows, and here are the first few:

```
age,workclass,fnlwgt,education,education_num,marital_status,occupation,relationship,race,sex,capital_gain,capital_loss,hours_per_week,native_country,high_income
39, State-gov, 77516, Bachelors, 13, Never-married, Adm-clerical, Not-in-family, White, Male, 2174, 0, 40, United-States, <=50K
50, Self-emp-not-inc, 83311, Bachelors, 13, Married-civ-spouse, Exec-managerial, Husband, White, Male, 0, 0, 13, United-States, <=50K
38, Private, 215646, HS-grad, 9, Divorced, Handlers-cleaners, Not-in-family, White, Male, 0, 0, 40, United-States, <=50K
```

Each row represents a single person who was counted in the `1990` US Census, and contains information about their income and demograpics. Here are some of the relevant columns:

- `age` -- how old the person is
- `workclass` -- the type of sector the person is employed in.
- `race` -- the race of the person.
- `sex` -- the gender of the person, either `Male` or `Female`.
- `high_income` -- if the person makes more the `50k` or not.

In the last mission, we calculated a chi-squared value indicating how the observed frequencies in a single categorical column, such as `sex`, varied from the US population as a whole.

In this mission, we'll look how to make this same technique applicable to cross tables, that show how two categorical columns interact. For instance, here's a table showing the relationship between `sex` and `high_income`:

SexMaleFemaleTotals>50k666211797841Income<=50k15128959224720Totals217901077132561

On looking at this diagram, you might see a pattern between `sex` and `high_income`. But it's hard to immediately quantify that pattern, and tell if it's significant. We can apply the chi-squared test (also known as the [chi-squared test of association](https://en.wikipedia.org/wiki/Chi-squared_test)) to figure out if there's a statistically significant correlation between two categorical columns.

## 2: Calculating Expected Values

In the single category chi-squared test, we find expected values from other data sets, and then compare with our own observed values. In a multiple category chi-squared test, we calculate expected values across our whole dataset. We'll illustrate this by converting our chart from last screen into proportions:

SexMaleFemaleTotals>50k.205.036.241Income<=50k.465.294.759Totals.669.3311

Each cell represents the proportion of people in the data set that fall into the specified categories.

- `20.5%` of `Males` in the whole data set earn `>50k` in income.
- `33.1%` of the whole dataset is `Female`
- `75.9%` of the whole dataset earns `<=50k`.

We can use our total proportions to calculate expected values. `24.1%` of all people in `income` earn `>50k`, and `33.1%`of all people in `income` are `Female`, so we'd expect the proportion of people who are female and earn `>50k` to be `.241 * .331`, which is `.0799771`. We have this expectation based on the proportions of `Females` and `>50k` earners across the whole dataset. Instead, we see that the observed proportion is `.036`, which indicates that there may be some correlation between the `sex` and `high_income` columns.

We can convert our expected proportion to an expected value by multiplying by `32561`, the total number of rows in the data set, which gives us `32561 * .0799771`, or `2597.4`.

### Instructions

Using the expected proportions in the table above, calculate the expected values for each of the `4` cells in the table.

- Calculate the expected value for `Males` who earn `>50k`, and assign to `males_over50k`.
- Calculate the expected value for `Males` who earn `<=50k`, and assign to `males_under50k`.
- Calculate the expected value for `Females` who earn `>50k`, and assign to `females_over50k`.
- Calculate the expected value for `Females` who earn `<=50k`, and assign to `females_under50k`.

```
males_over50k = .669 * .241 * 32561
males_under50k = .669 * .759 * 32561
females_over50k = .331 * .241 * 32561
females_under50k = .331 * .759 * 32561
```

## 3: Calculating Chi-Squared

In the last screen, you should have ended up with a table like this:

SexMaleFemale>50k5249.82597.4Income<=50k16533.58180.3

Now that we have our expected values, we can calculate the chi-squared value by using the same principle from the previous mission. Here are the steps:

- Subtract the expected value from the observed value.
- Square the difference.
- Divide the squared difference by the expected value.
- Repeat for all the observed and expected values and add up the values.

Here's the formula:

∑(observed−expected)2expected

Here's the table of our observed values for reference:

SexMaleFemale>50k66621179Income<=50k151289592

### Instructions

- Compute the chi-squared value for the observed values above and the expected values above.
  - Assign the result to `chisq_gender_income`.

```
observed = [6662, 1179, 15128, 9592]
expected = [5249.8, 2597.4, 16533.5, 8180.3]
values = []

for i, obs in enumerate(observed):
    exp = expected[i]
    value = (obs - exp) ** 2 / exp
    values.append(value)

chisq_gender_income = sum(values)
```

## 4: Finding Statistical Significance

Now that we've found our chi-squared value, `1517.6`, we can use the same technique with the chi-squared sampling distribution from the last mission to find a p-value associated with the chi-squared value. The p-value will tell us whether the difference between the observed and expected values is statistically significant or not.

Rather than construct a sampling distribution again manually, we'll use the [scipy.stats.chisquare](http://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.mstats.chisquare.html) function that we covered in the last mission.

If we had a table of expected values that looked like this:

551010

And a table of observed values that looked like this:

101055

We could find the chi-squared value and the p-value using the [scipy.stats.chisquare](http://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.mstats.chisquare.html) function like this:

```
import numpy as np
from scipy.stats import chisquare

observed = np.array([10, 10, 5, 5])
expected = np.array([5, 5, 10, 10])
chisquare_value, pvalue = chisquare(observed, expected)
```

### Instructions

Here are our expected values from the last screen:

SexMaleFemale>50k5249.82597.4Income<=50k16533.58180.3

And here are our observed values:

SexMaleFemale>50k66621179Income<=50k151289592

- Use the [scipy.stats.chisquare](http://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.mstats.chisquare.html) function to find the chi-squared value and p-value for the above observed and expected counts.Assign the p-value to `pvalue_gender_income`.

```
import numpy as np
from scipy.stats import chisquare

observed = np.array([6662, 1179, 15128, 9592])
expected = np.array([5249.8, 2597.4, 16533.5, 8180.3])

chisq_value, pvalue_gender_income = chisquare(observed, expected)
```

## 5: Cross Tables

We can also scale up the chi-squared test to cases where each category contains more than two possibilities. We'll illustrate this with an example where we look at `sex` vs `race`.

Before we can find the chi-squared value, we need to find the observed frequency counts. We can do this using the [pandas.crosstab](http://pandas.pydata.org/pandas-docs/version/0.17.0/generated/pandas.crosstab.html) function. The crosstab function will print a table that shows frequency counts for two or more columns. Here's how you could use the [pandas.crosstab](http://pandas.pydata.org/pandas-docs/version/0.17.0/generated/pandas.crosstab.html) function:

```
import pandas

table = pandas.crosstab(income["sex"], [income["high_income"]])
print(table)
```

The above code will print a table showing how many people from `income` fall into each category of `sex` and `high_income`.

The second parameter passed into [pandas.crosstab](http://pandas.pydata.org/pandas-docs/version/0.17.0/generated/pandas.crosstab.html) is actually a list -- this parameter can contain more than one item.

```
import pandas
table = pandas.crosstab(income["sex"], [income["race"]])
print(table)
```

## 6: Finding Expected Values

Now that we have the observed frequency counts, we can generate the expected values. We can use the [scipy.stats.chi2_contingency](http://docs.scipy.org/doc/scipy-0.15.1/reference/generated/scipy.stats.chi2_contingency.html) function to do this. The function takes in a cross table of observed counts, and returns the chi-squared value, the p-value, the degrees of freedom, and the expected frequencies. Let's say we have the following observed counts:

551010

Here's how we could use the [scipy.stats.chi2_contingency](http://docs.scipy.org/doc/scipy-0.15.1/reference/generated/scipy.stats.chi2_contingency.html) function:

```
import numpy as np
from scipy.stats import chi2_contingency
observed = np.array([[5, 5], [10, 10]])

chisq_value, pvalue, df, expected = chi2_contingency(observed)
```

You can also directly pass the result of the [pandas.crosstab](http://pandas.pydata.org/pandas-docs/version/0.17.0/generated/pandas.crosstab.html) function into the [scipy.stats.chi2_contingency](http://docs.scipy.org/doc/scipy-0.15.1/reference/generated/scipy.stats.chi2_contingency.html) function, which makes it extremely easy to do perform a chi-squared test.

### Instructions

- Use the [scipy.stats.chi2_contingency](http://docs.scipy.org/doc/scipy-0.15.1/reference/generated/scipy.stats.chi2_contingency.html) function to calculate the pvalue for the `sex` and `race` columns of `income`.Assign the result to `pvalue_gender_race`.

```
import pandas
from scipy.stats import chi2_contingency

table = pandas.crosstab(income["sex"], [income["race"]])
chisq_value, pvalue_gender_race, df, expected = chi2_contingency(table)
```

## 7: Caveats

Now that we've learned the chi-squared test, you should be able to figure out if the association between two columns of categorical data is statistically significant or not. There are a few caveats to using the chi-squared test that are important to cover, though:

- Finding that a result isn't significant doesn't mean that no association between the columns exists. For instance, if we found that the chi-squared test between the `sex` and `race` columns returned a p-value of `.1`, it wouldn't mean that there is no relationship between `sex` and `race`. It just means that there isn't a statistically significant relationship.
- Finding a statistically significant result doesn't imply anything about what the correlation is. For instance, finding that a chi-squared test between `sex` and `race` results in a p-value of `.01` doesn't mean that the dataset contains too many `Females` who are `White` (or too few). A statistically significant finding means that some evidence of a relationship between the variables exists, but needs to be investigated further.
- Chi-squared tests can only be applied in the case where each possibility within a category is independent. For instance, the Census counts individuals as either `Male` or `Female`, not both.
- Chi-squared tests are more valid when the numbers in each cell of the cross table are larger. So if each number is `100`, great -- if each number is `1`, you may need to gather more data.

## 8: Next Steps

In this mission, we covered chi-squared tests for multiple categories, and learned how to quickly perform chi-squared tests. We learned when to apply and when not to apply chi-squared tests. Chi-squared tests can be a powerful tool to discover correlations and figure out when anomalies in your data should be investigated further.

# Guided Project: Winning Jeopardy

## 1: Jeopardy Questions

Jeopardy is a popular TV show in the US where participants answer questions to win money. It's been running for a few decades, and is a major force in popular culture. If you need help at any point, you can consult our solution notebook [here](https://github.com/dataquestio/solutions/blob/master/Mission210Solution.ipynb).

Let's say you want to compete on Jeopardy, and you're looking for any edge you can get to win. In this project, you'll work with a dataset of Jeopardy questions to figure out some patterns in the questions that could help you win.

The dataset is named `jeopardy.csv`, and contains `20000` rows from the beginning of a full dataset of Jeopardy questions, which you can download [here](https://www.reddit.com/r/datasets/comments/1uyd0t/200000_jeopardy_questions_in_a_json_file). Here's the beginning of the file:

![Imgur](https://dq-content.s3.amazonaws.com/Nlfu13A.png)

As you can see, each row in the dataset represents a single question on a single episode of Jeopardy. Here are explanations of each column:

- `Show Number` -- the Jeopardy episode number of the show this question was in.
- `Air Date` -- the date the episode aired.
- `Round` -- the round of Jeopardy that the question was asked in. Jeopardy has several rounds as each episode progresses.
- `Category` -- the category of the question.
- `Value` -- the number of dollars answering the question correctly is worth.
- `Question` -- the text of the question.
- `Answer` -- the text of the answer.

### Instructions

- Read the dataset into a Dataframe called `jeopardy` using [Pandas](http://pandas.pydata.org/).
- Print out the first `5` rows of `jeopardy`.
- Print out the columns of `jeopardy` using `jeopardy.columns`.
- Some of the column names have spaces in front.
  - Remove the spaces in each item in `jeopardy.columns`.
  - Assign the result back to `jeopardy.columns` to fix the column names in `jeopardy`.
- Make sure you pay close attention to the format of each column.

## 2: Normalizing Text

Before you can start doing analysis on the Jeopardy questions, you need to normalize all of the text columns (the `Question` and `Answer` columns). We covered normalization before, but the idea is to ensure that you lowercase words and remove punctuation so `Don't` and `don't`aren't considered to be different words when you compare them.

### Instructions

- Write a function to normalize questions and answers. It should:
  - Take in a string.
  - Convert the string to lowercase.
  - Remove all punctuation in the string.
  - Return the string.
- Normalize the `Question` column.Use the Pandas [apply](http://pandas.pydata.org/pandas-docs/version/0.17.0/generated/pandas.Series.apply.html) method to apply the function to each item in the `Question` column.Assign the result to the `clean_question` column.
- Normalize the `Answer` column.Use the Pandas [apply](http://pandas.pydata.org/pandas-docs/version/0.17.0/generated/pandas.Series.apply.html) method to apply the function to each item in the `Answer` column.Assign the result to the `clean_answer` column.

## 3: Normalizing Columns

Now that you've normalized the text columns, there are also some other columns to normalize.

The `Value` column should also be numeric, to allow you to manipulate it more easily. You'll need to remove the dollar sign from the beginning of each value and convert the column from text to numeric.

The `Air Date` column should also be a datetime, not a string, to enable you to work with it more easily.

### Instructions

- Write a function to normalize dollar values. It should:
  - Take in a string.
  - Remove any punctuation in the string.
  - Convert the string to an integer.
  - If the conversion has an error, assign `0` instead.
  - Return the integer.
- Normalize the `Value` column.Use the Pandas [apply](http://pandas.pydata.org/pandas-docs/version/0.17.0/generated/pandas.Series.apply.html) method to apply the function to each item in the `Value` column.Assign the result to the `clean_value` column.
- Use the [pandas.to_datetime](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.to_datetime.html) function to convert the `Air Date` column to a datetime column.

## 4: Answers In Questions

In order to figure out whether to study past questions, study general knowledge, or not study it all, it would be helpful to figure out two things:

- How often the answer is deducible from the question.
- How often new questions are repeats of older questions.

You can answer the second question by seeing how often complex words (> 6 characters) reoccur. You can answer the first question by seeing how many times words in the answer also occur in the question. We'll work on the first question now, and come back to the second.

### Instructions

- Write a function that takes in a row in `jeopardy`, as a Series. It should:Split the `clean_answer` column on the space character (``), and assign to the variable `split_answer`.Split the `clean_question` column on the space character (``), and assign to the variable `split_question`.Create a variable called `match_count`, and set it to `0`.If `the` is in `split_answer`, remove it using the [remove](https://docs.python.org/2/tutorial/datastructures.html) method on lists. `The` is commonly found in answers and questions, but doesn't have any meaningful use in finding the answer.If the length of `split_answer` is `0`, return `0`. This prevents a division by zero error later.Loop through each item in `split_answer`, and see if it occurs in `split_question`. If it does, add `1` to `match_count`.Divide `match_count` by the length of `split_answer`, and return the result.
- Count how many times terms in `clean_answer` occur in `clean_question`.Use the Pandas [apply](http://pandas.pydata.org/pandas-docs/version/0.17.0/generated/pandas.DataFrame.apply.html) method on Dataframes to apply the function to each row in `jeopardy`.Pass the `axis=1` argument to apply the function across each row.Assign the result to the `answer_in_question`column.
- Find the mean of the `answer_in_question` column using the [mean](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.Series.mean.html) method on Series.
- Write up a markdown cell with a short explanation of how finding this mean might influence your studying strategy for Jeopardy.

## 5: Recycled Questions

Let's say you want to investigate how often new questions are repeats of older ones. You can't completely answer this, because you only have about `10%` of the full Jeopardy question dataset, but you can investigate it at least.

To do this, you can:

- Sort `jeopardy` in order of ascending air date.
- Maintain a *set* called `terms_used` that will be empty initially.
- Iterate through each row of `jeopardy`.
- Split `clean_question` into words, remove any word shorter than `6`characters, and check if each word occurs in `terms_used`.If it does, increment a counter.Add each word to `terms_used`.

This will enable you to check if the terms in questions have been used previously or not. Only looking at words greater than `6` characters enables you to filter out words like `the` and `than`, which are commonly used, but don't tell you a lot about a question.

### Instructions

- Create an empty list called `question_overlap`.
- Create an empty set called `terms_used`.
- Use the [iterrows](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.iterrows.html) Dataframe method to loop through each row of `jeopardy`.Split the `clean_question` column of the row on the space character (``), and assign to `split_question`.Remove any words in `split_question` that are less than `6` characters long.Set `match_count` to `0`.Loop through each word in `split_question`.If the term occurs in `terms_used`, add `1` to `match_count`.Add each word in `split_question` to `terms_used`using the [add](https://docs.python.org/2/library/sets.html) method on sets.If the length of `split_question` is greater than `0`, divide `match_count` by the length of `split_question`.Append `match_count` to `question_overlap`.
- Assign `question_overlap` to the `question_overlap`column of `jeopardy`.
- Find the mean of the `question_overlap` column and print it.
- Look at the value, and think about what this might mean for questions being recycled. Write up your thoughts in a markdown cell.

## 6: Low Value Vs High Value Questions

Let's say you only want to study questions that pertain to high value questions instead of low value questions. This will help you earn more money when you're on Jeopardy.

You can actually figure out which terms correspond to high-value questions using a chi-squared test. You'll first need to narrow down the questions into two categories:

- Low value -- Any row where `Value`is less than `800`.
- High value -- Any row where `Value`is greater than `800`.

You'll then be able to loop through each of the terms from the last screen, `terms_used`, and:

- Find the number of low value questions the word occurs in.
- Find the number of high value questions the word occurs in.
- Find the percentage of questions the word occurs in.
- Based on the percentage of questions the word occurs in, find expected counts.
- Compute the chi squared value based on the expected counts and the observed counts for high and low value questions.

You can then find the words with the biggest differences in usage between high and low value questions, by selecting the words with the highest associated chi-squared values. Doing this for all of the words would take a very long time, so we'll just do it for a small sample now.

### Instructions

- Create a function that takes in a row from a Dataframe, and:
  - If the `clean_value` column is greater than `800`, assign `1` to `value`.
  - Otherwise, assign `0` to `value`.
  - Return `value`.
- Determine which questions are high and low value.
  - Use the Pandas [apply](http://pandas.pydata.org/pandas-docs/version/0.17.0/generated/pandas.DataFrame.apply.html) method on Dataframes to apply the function to each row in `jeopardy`.
  - Pass the `axis=1` argument to apply the function across each row.
  - Assign the result to the `high_value` column.
- Create a function that takes in a word, and:
  - Assigns `0` to `low_count`.
  - Assigns `0` to `high_count`.
  - Loops through each row in `jeopardy` using the [iterrows](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.iterrows.html) method.Split the `clean_question` column on the space character (``).If the word is in the split question:If the `high_value` column is `1`, add `1` to `high_count`.Else, add `1` to `low_count`.
  - Returns `high_count` and `low_count`. You can return multiple values by separating them with a comma.
- Create an empty list called `observed_expected`.
- Convert `terms_used` into a list using the [list](https://docs.python.org/3/library/functions.html#func-list) function, and assign the first `5` elements to `comparison_terms`.
- Loop through each term in `comparison_terms`, and:Run the function on the term to get the high value and low value counts.Append the result of running the function (which will be a list) to `observed_expected`.

## 7: Applying The Chi-Squared Test

Now that you've found the observed counts for a few terms, you can compute the expected counts and the chi-squared value.

### Instructions

- Find the number of rows in `jeopardy` where `high_value`is `1`, and assign to `high_value_count`.
- Find the number of rows in `jeopardy` where `high_value`is `0`, and assign to `low_value_count`.
- Create an empty list called `chi_squared`.
- Loop through each list in `observed_expected`.Add up both items in the list (high and low counts) to get the total count, and assign to `total`.Divide `total` by the number of rows in `jeopardy` to get the proportion across the dataset. Assign to `total_prop`.Multiply `total_prop` by `high_value_count` to get the expected term count for high value rows.Multiply `total_prop` by `low_value_count` to get the expected term count for low value rows.Use the [scipy.stats.chisquare](http://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.mstats.chisquare.html) function to compute the chi-squared value and p-value given the expected and observed counts.Append the results to `chi_squared`.
- Look over the chi-squared values and the associated p-values. Are there any statistically significant results? Write up your thoughts in a markdown cell.

## 8: Next Steps

That's it for the guided steps! We recommend exploring the data more on your own.

Here are some potential next steps:

- Find a better way to eliminate non-informative words than just removing words that are less than `6` characters long. Some ideas:Manually create a list of words to remove, like `the`, `than`, etc.Find a list of stopwords to remove.Remove words that occur in more than a certain percentage (like `5%`) of questions.
- Perform the chi-squared test across more terms to see what terms have larger differences. This is hard to do currently because the code is slow, but here are some ideas:
  - Use the [apply](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.apply.html) method to make the code that calculates frequencies more efficient.
  - Only select terms that have high frequencies across the dataset, and ignore the others.
- Look more into the `Category` column and see if any interesting analysis can be done with it. Some ideas:See which categories appear the most often.Find the probability of each category appearing in each round.
- Use the whole Jeopardy dataset (available [here](https://www.reddit.com/r/datasets/comments/1uyd0t/200000_jeopardy_questions_in_a_json_file)) instead of the subset we used in this mission.
- Use phrases instead of single words when seeing if there's overlap between questions. Single words don't capture the whole context of the question well.

We recommend creating a [Github](https://github.com/) repository and placing this project there. It will help other people, including employers, see your work. As you start to put multiple projects on Github, you'll have the beginnings of a strong portfolio.

You're welcome to keep working on the project here, but we recommend downloading it to your computer using the download icon above and working on it there.

We hope this guided project has been a good experience, and please email us at hello@dataquest.io if you want to share your work!