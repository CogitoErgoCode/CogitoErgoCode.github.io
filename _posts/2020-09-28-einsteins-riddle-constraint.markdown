---
layout: post
title: "Einstein's Riddle, Constraint Based Solution 🐟"
date: 2020-10-19 12:33:32 -0700
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


# **Programmatic Solution (Constraint Based)**

Lets attempt to write a constraint based python program to solve Einstein's Riddle.
Constraint programming is a `declarative programming paradigm` that focuses on `what` to execute
rather than `how` to execute (imperative). 

Firstly, install python-constraint module.

```
$ pip install python-constraint
```

Import the necessary requirements and create the solver object, assigning it to a variable call problem.

```python
from constraint import *

problem = Problem()
```

Using the same variables dictionary structure as before, we iterate over each list, and associate a value 1-5 subsequently to each item of the list using the built in `addVariables()` method. We apply exclusivity to each list by applying the constraint `AllDifferentConstraint()` with `addConstraint()`.

```python
for values in variables.values():
    problem.addVariables(values, range(1,5+1))
    problem.addConstraint(AllDifferentConstraint(), values)
```

Our next goal is to apply the logic of the constraints via lambda functions to each association.

```python
# constraints: equal
for i in (
    ["brit"      , "red"  ], ["swede" , "dogs"   ],
    ["dane"      , "tea"  ], ["green" , "coffee" ],
    ["pallMall"  , "birds"], ["yellow", "dunhill"],
    ["blueMaster", "beer" ], ["german", "prince" ]
):
    problem.addConstraint(lambda a, b: a == b, i)

# constraints: next_to
for i in (
    ["blends"   , "cats"], ["horses", "dunhill"],
    ["norwegian", "blue"], ["blends", "water"  ]
):
    problem.addConstraint(lambda a, b: a == b - 1 or a == b + 1, i)

# constraints: left_of, middle, first
problem.addConstraint(lambda a, b: a == b - 1, ["green"    , "white"])
problem.addConstraint(lambda a: a == 3       , ["milk"              ])
problem.addConstraint(lambda a: a == 1       , ["norwegian"         ])
```

To retrieve the solution all that needs to be done is to call the method `getSolution()` upon the problem object.

```python
solution = problem.getSolution()
```

The data structure returned is in dictionary form, we will need to format the output.

```python
{
    'blends'    : 2, 'dunhill': 1, 'green' : 4, 'coffee': 4, 'horses'    : 2, 
    'norwegian' : 1, 'blue'   : 2, 'white' : 5, 'yellow': 1, 'red'       : 3, 
    'brit'      : 3, 'cats'   : 1, 'water' : 1, 'beer'  : 5, 'blueMaster': 5, 
    'pallMall'  : 3, 'birds'  : 3, 'prince': 4, 'german': 4, 'dane'      : 2, 
    'swede'     : 5, 'dogs'   : 5, 'tea'   : 2, 'fish'  : 4, 'milk'      : 3
}
```

This solution has given us the groups by number. For instance group 1 contains dunhill, norwegian, yellow, cats, and water; all we have to do is place the strings in each group in an order we decide.

```python
def order(key):
    for position, array in enumerate(variables.values()):
        if key in array:
            return position

ordered = [ 
    sorted([ k for k,v in solution.items() if v == i], key=lambda x: order(x) ) 
        for i in range(1, max(solution.values())+1)
]
```

Now that we have the data organized into their respective groups, all we have to do now is swap columns for rows.

```python
[
    ['yellow', 'norwegian', 'water' , 'dunhill'   , 'cats'  ], 
    ['blue'  , 'dane'     , 'tea'   , 'blends'    , 'horses'], 
    ['red'   , 'brit'     , 'milk'  , 'pallMall'  , 'birds' ], 
    ['green' , 'german'   , 'coffee', 'prince'    , 'fish'  ], 
    ['white' , 'swede'    , 'beer'  , 'blueMaster', 'dogs'  ]
]
```

There's a few ways we can accomplish this, one way is to use numpy `np.array(ordered).T.tolist()`, the other is to use a pandas `DataFrame()`.

```python
df = pd.DataFrame(
    ordered,
    index   = ["first" , "second", "third"  , "fourth" , "fifth"],
    columns = ["color:", "race:" , "smokes:", "drinks:", "pets:"]
).transpose()
```

Personally i'd like to avoid added overhead by creating my own solution to transpose.

```python
# transpose & format output
for i in map(list, zip(*ordered)):
    print("{:10s} {:10s} {:10s} {:10s} {:10s}".format(*i))
```

Full program & output

{% highlight python %}
from constraint import *

def main():
    problem = Problem()

    variables = {
        'houses' : ['red'     , 'green'  , 'white' , 'yellow'    , 'blue'  ],
        'races'  : ['brit'    , 'swede'  , 'dane'  , 'norwegian' , 'german'],
        'drinks' : ['tea'     , 'coffee' , 'milk'  , 'beer'      , 'water' ],
        'smokes' : ['pallMall', 'dunhill', 'blends', 'blueMaster', 'prince'],
        'pets'   : ['dogs'    , 'birds'  , 'cats'  , 'horses'    , 'fish'  ]
    }

    # addVariables: each list gets numbered 1 - 5
    # addConstraint: apply exclusivity
    for values in variables.values():
        problem.addVariables(values, range(1,5+1))
        problem.addConstraint(AllDifferentConstraint(), values)

    # constraints: equal
    for i in (
        ["brit"      , "red"  ], ["swede" , "dogs"   ],
        ["dane"      , "tea"  ], ["green" , "coffee" ],
        ["pallMall"  , "birds"], ["yellow", "dunhill"],
        ["blueMaster", "beer" ], ["german", "prince" ]
    ):
        problem.addConstraint(lambda a, b: a == b, i)

    # constraints: next_to
    for i in (
        ["blends"   , "cats"], ["horses", "dunhill"],
        ["norwegian", "blue"], ["blends", "water"  ]
    ):
        problem.addConstraint(lambda a, b: a == b - 1 or a == b + 1, i)

    # constraints: left_of, middle, first
    problem.addConstraint(lambda a, b: a == b - 1, ["green"    , "white"])
    problem.addConstraint(lambda a: a == 3       , ["milk"              ])
    problem.addConstraint(lambda a: a == 1       , ["norwegian"         ])

    solution = problem.getSolution()

    def order(key):
        for position, array in enumerate(variables.values()):
            if key in array:
                return position

    ordered = [ 
        sorted([ k for k,v in solution.items() if v == i], key=lambda x: order(x) ) 
            for i in range(1, max(solution.values())+1)
    ]

    # transpose & format output
    for i in map(list, zip(*ordered)):
        print("{:10s} {:10s} {:10s} {:10s} {:10s}".format(*i))


if __name__ == "__main__":
    main()
{% endhighlight %}

```
yellow     blue       red        green      white
norwegian  dane       brit       german     swede
water      tea        milk       coffee     beer
dunhill    blends     pallMall   prince     blueMaster
cats       horses     birds      fish       dogs
```