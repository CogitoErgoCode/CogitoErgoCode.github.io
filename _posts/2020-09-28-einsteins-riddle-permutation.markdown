---
layout: post
title: "Einstein's Riddle, Permutation Based Solution 🐟"
date: 2020-10-19 12:32:35 -0700
categories: logic riddle einstein
published: true
---

Legend has it that this puzzle was created by Albert Einstein as a boy. Einstein said that only 2% of the world could solve it. Considering that there are 5!^5 or 24,883,200,000 combinations with only 1 correct solution, are you up for the challenge?

# **Riddle**

1. There are 5 houses in five different colors.
2. In each house lives a person with a different nationality.
3. These five owners drink a certain type of beverage, smoke a certain brand of cigar and keep a certain pet.
4. No owners have the same pet, smoke the same brand of cigar or drink the same beverage.

The question is: ***Who owns the fish?*** 🐟


# **Hints**

1. the `Brit` lives in the *<span style="color:red">red house</span>*
2. the `Swede` keeps dogs 🐕 as pets
3. the `Dane` drinks tea 🍵
4. the *<span style="color:green">green house</span>* is on the left of the *<span style="color: grey">white house</span>*
5. the *<span style="color:green">green house's</span>* owner drinks coffee ☕
6. the person who smokes Pall Mall 🚬 rears birds 🐦
7. the owner of the *<span style="color:gold">yellow house</span>* smokes Dunhill 🚬
8. the man living in the center house drinks milk 🍼
9. the `Norwegian` lives in the first house
10. the man who smokes blends 🚬 lives next to the one who keeps cats 😺
11. the man who keeps horses 🐴 lives next to the man who smokes Dunhill 🚬
12. the owner who smokes BlueMaster 🚬 drinks beer 🍺
13. the `German` smokes Prince 🚬
14. the `Norwegian` lives next to the *<span style="color:blue">blue house</span>*
15. the man who smokes blend 🚬 has a neighbor who drinks water 🌊

# **Programmatic Solution (Permutation)**

If we are going to solve a problem we need to recognize that it has variables and that those variables need 
a data structure to exist in so we can manipulate these variables into a proper solution. Lets store them in a dictionary of lists.

```python
variables = {
    'colors' : ['red'     , 'green'  , 'white' , 'yellow'    , 'blue'  ],
    'races'  : ['brit'    , 'swede'  , 'dane'  , 'norwegian' , 'german'],
    'drinks' : ['tea'     , 'coffee' , 'milk'  , 'beer'      , 'water' ],
    'smokes' : ['pallMall', 'dunhill', 'blends', 'blueMaster', 'prince'],
    'pets'   : ['dogs'    , 'birds'  , 'cats'  , 'horses'    , 'fish'  ]
}
```

Next we'll be defining our constraints, the basic rules that govern the position and associated items that we will use to bruteforce our solution. If two items are related to one another, for instance hint #1 which states that the brit lives in the red house, we say that they are equal mathematically. When we attempt to bruteforce the answer, we will be crunching number representations of each list. 

```python
equals  = lambda a,b: True if a == b else False
next_to = lambda a,b: True if a == b - 1 or a == b + 1 else False
left_of = lambda a,b: True if a == b - 1 else False
```

In order to fully comprehend the following snippet of code, we must first understand De Morgan's theorem. These laws are used in propositional logic, Boolean algebra, simplification of logical expressions in computer programs and digital circuit designs.

> "the negation of a disjunction is the conjunction of the negations; and <br>
the negation of a conjunction is the disjunction of the negations;" ― [Wikipedia](https://en.wikipedia.org/wiki/De_Morgan%27s_laws)


```python
( not True and not False ) == ( not (True or  False) )
( not True or  not False ) == ( not (True and False) )
```

We will use the aforementioned laws to reduce the verbosity of the resulting code below. In essence we are bruteforcing permutations against constraints defined in the lambda functions.

```python
perm = list(permutations(range(1, 5+1)))
for red, green, white, yellow, blue in perm:
    if not left_of(green, white):
        continue
    for brit, swede, dane, norwegian, german in perm:
        if not ( equals(brit, red) and next_to(norwegian, blue) ):
            continue
        for tea, coffee, milk, beer, water in perm:
            if not ( equals(dane, tea) and equals(green, coffee) and \
                     milk == 3         and norwegian == 1 ):
                continue
            for pallMall, dunhill, blends, blueMaster, prince in perm:
                if not ( equals(yellow, dunhill) and equals(blueMaster, beer) and \
                         equals(german, prince)  and next_to(blends, water) ):
                    continue
                for dogs, birds, cats, horses, fish in perm:
                    if not ( equals(swede, dogs)   and equals(pallMall, birds) and \
                             next_to(blends, cats) and next_to(horses, dunhill) ):
                        continue
                    ...
```

We could also negate the python keyword **all** to achieve the same effect. 

```python
perm = list(permutations(range(1, 5+1)))
for red, green, white, yellow, blue in perm:
    if not left_of(green, white):
        continue
    for brit, swede, dane, norwegian, german in perm:
        if not all([equals(brit, red), next_to(norwegian, blue)]):
            continue
        for tea, coffee, milk, beer, water in perm:
            if not all([ equals(dane, tea), equals(green, coffee), 
                         milk == 3, norwegian == 1 ]):
                continue
            for pallMall, dunhill, blends, blueMaster, prince in perm:
                if not all([ equals(yellow, dunhill), equals(blueMaster, beer), 
                             equals(german, prince), next_to(blends, water) ]):
                    continue
                for dogs, birds, cats, horses, fish in perm:
                    if not all([ equals(swede, dogs), equals(pallMall, birds), 
                                 next_to(blends, cats), next_to(horses, dunhill) ]):
                        continue
                    ...
```
It's important to realize why we can't just test the constraints under the final nested iterative loop.
If we attempted to do that, it could take a very long time to compute the answer. Imagine if `left_of(green, white)` was in the last loop instead of the first, we wouldn't have hit the continue in the first iterative loop and instead we would have had to go through all the permutations of each subsequent loop after the first just to check the constraint that should have ended all the unnecessary iterations. By placing the constraints in the highest loop that supports their variables, we save massive amounts of time.

When we reach a solution by bruteforcing all the permutations above, we will have arrived at our solution. This solutions data structure will look something like this if it were printed out:

```python
print(
    [red     , green  , white , yellow    , blue  ],
    [brit    , swede  , dane  , norwegian , german],
    [tea     , coffee , milk  , beer      , water ],
    [pallMall, dunhill, blends, blueMaster, prince],
    [dogs    , birds  , cats  , horses    , fish  ]
)

[3, 4, 5, 1, 2] [3, 5, 2, 1, 4] [2, 4, 3, 5, 1] [3, 1, 2, 5, 4] [5, 3, 1, 2, 4]
```

Our next step is to associate the solved positions with their string counterparts into something that looks more like this:

```python
(
    [ ('red'     , 3), ('green'  , 4), ('white' , 5), ('yellow'    , 1), ('blue'  , 2) ]
    [ ('brit'    , 3), ('swede'  , 5), ('dane'  , 2), ('norwegian' , 1), ('german', 4) ]
    [ ('tea'     , 2), ('coffee' , 4), ('milk'  , 3), ('beer'      , 5), ('water' , 1) ]
    [ ('pallMall', 3), ('dunhill', 1), ('blends', 2), ('blueMaster', 5), ('prince', 4) ]
    [ ('dogs'    , 5), ('birds'  , 3), ('cats'  , 1), ('horses'    , 2), ('fish'  , 4) ]
)
```
We can accomplish this with a simple lambda expression which combines the strings found in the corresponding variables dictionary in their old order configuration with the newly ordered values we just bruteforced. The reason we do this is so we can sort the list into its proper arrangement, we then immediately discard the position as it is no longer needed once the string is sorted.

```python
g = lambda oldLst,newPos: [i for i,j in sorted(zip(oldLst, newPos), key=lambda x: x[1])]
```
The resulting data structure from the list comprehension will produce a list of tuples filled with the strings in their proper order. We just simply need to print them out by iterating over the solved positions.

```python
for (k,v), new in zip(variables.items(), (
    [red     , green  , white , yellow    , blue  ],
    [brit    , swede  , dane  , norwegian , german],
    [tea     , coffee , milk  , beer      , water ],
    [pallMall, dunhill, blends, blueMaster, prince],
    [dogs    , birds  , cats  , horses    , fish  ]
)):
    print("{:6s}: {:10s} {:10s} {:10s} {:10s} {:10s}".format(k, *g(v, new)))
```

Full program & output

{% highlight python %}
from itertools import permutations

def main():
    variables = {
        'colors' : ['red'     , 'green'  , 'white' , 'yellow'    , 'blue'  ],
        'races'  : ['brit'    , 'swede'  , 'dane'  , 'norwegian' , 'german'],
        'drinks' : ['tea'     , 'coffee' , 'milk'  , 'beer'      , 'water' ],
        'smokes' : ['pallMall', 'dunhill', 'blends', 'blueMaster', 'prince'],
        'pets'   : ['dogs'    , 'birds'  , 'cats'  , 'horses'    , 'fish'  ]
    }

    equals  = lambda a,b: a == b
    next_to = lambda a,b: a == b - 1 or a == b + 1
    left_of = lambda a,b: a == b - 1
                        
    perm = list(permutations(range(1, 5+1)))
    for red, green, white, yellow, blue in perm:
        if not left_of(green, white):
            continue
        for brit, swede, dane, norwegian, german in perm:
            # if not ( equals(brit, red) and next_to(norwegian, blue) ):
            if not all([equals(brit, red), next_to(norwegian, blue)]):
                continue
            for tea, coffee, milk, beer, water in perm:
                # if not ( equals(dane, tea) and equals(green, coffee) and \
                #          milk == 3         and norwegian == 1 ):
                if not all([ equals(dane, tea), equals(green, coffee), 
                             milk == 3, norwegian == 1 ]):
                    continue
                for pallMall, dunhill, blends, blueMaster, prince in perm:
                    # if not ( equals(yellow, dunhill) and equals(blueMaster, beer) and \
                    #          equals(german, prince)  and next_to(blends, water) ):
                    if not all([ equals(yellow, dunhill), equals(blueMaster, beer), 
                                 equals(german, prince), next_to(blends, water) ]):
                        continue
                    for dogs, birds, cats, horses, fish in perm:
                        # if not ( equals(swede, dogs)   and equals(pallMall, birds) and \
                        #          next_to(blends, cats) and next_to(horses, dunhill) ):
                        if not all([ equals(swede, dogs), equals(pallMall, birds), 
                                     next_to(blends, cats), next_to(horses, dunhill) ]):
                            continue

                        g = lambda oldLst,newPos: [i for i,j in sorted(zip(oldLst, newPos), key=lambda x: x[1])]
                        
                        # iterate through the solved positions sorting each tuple of solutions as we print
                        for (k,v), new in zip(variables.items(), (
                            [red     , green  , white , yellow    , blue  ],
                            [brit    , swede  , dane  , norwegian , german],
                            [tea     , coffee , milk  , beer      , water ],
                            [pallMall, dunhill, blends, blueMaster, prince],
                            [dogs    , birds  , cats  , horses    , fish  ]
                        )):
                            print("{:6s}: {:10s} {:10s} {:10s} {:10s} {:10s}".format(k, *g(v, new)))

if __name__ == "__main__":
    main()
{% endhighlight %}

```
colors: yellow     blue       red        green      white
races : norwegian  dane       brit       german     swede
drinks: water      tea        milk       coffee     beer
smokes: dunhill    blends     pallMall   prince     blueMaster
pets  : cats       horses     birds      fish       dogs
```