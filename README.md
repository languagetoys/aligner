# Aligner

This is an implementation of a configurable [Damerau Levenshtein](https://en.wikipedia.org/wiki/Damerau%E2%80%93Levenshtein_distance) aligner.

I suggest catching up on Edit Distance in general if the reader requires some background before we dive into some examples.
Here are some recommended resources:
1. [Speech and Language Processing](https://web.stanford.edu/~jurafsky/slp3/2.pdf)
2. [Stanford](https://web.stanford.edu/class/cs124/lec/med.pdf)
3. [Wikipedia](https://en.wikipedia.org/wiki/Edit_distance)

## Use Cases

### Levenshtein

In this example String::equals is used as the equality function, i.e. what determines whether two elemnts are equal.

```
  List<String> source = List.of("one", "three", "two");
  List<String> target = List.of("one", "two", "three");
        
  Aligner<String> aligner = Aligner.levenshtein();
  
  Alignment<String> actual = aligner.align(source, target);
```

We can supply a custom equality function as such (though in this example both use String::equals).

```
  Aligner<String> aligner = Aligner.levenshtein((s, t) -> s.equals(t);
```

Let's take it a step further and configure our own cost function. We will use the AlignerBuilder for this, and reaplce the substituteCost with a char edit ratio, a static utility method supplied via the AlignerUtils class.

```
  Aligner<String> aligner = Aligner.<String>builder()
          .setSubstituteCost(AlignerUtils::charEditRatio)
          .build();
```
