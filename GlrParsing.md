# GRL Parsing #

## Introduction ##

GLR parsing (which stands for Generalized LR parsing) is an extension of the traditional LR parsing technique to handle highly ambiguous grammars. It is especially interesting for natural language processing, the context in which the technique has been devised.

## Declaration ##

To generate a GLR parser instead of a regular LALR parser, simply put the following option in the options section of the grammar declaration:

```
  (driver: glr)
```

## Running the parser ##

GLR parsers are run in exactly the same way as regular LALR parsers. The only difference is that the result of the parsing is a (possibly empty) list of parses instead of a single parse.

## Limitations ##

Since the parsing of a phrase can lead to many potential parses, errors cannot be detected as easily as with deterministic LR parsing. For this reason, it is advised to not put error productions in the grammar (they will be ignored anyway). Moreover, GLR parsers are usually not meant for interactive parsers (like the calculator example that comes with the distribution).