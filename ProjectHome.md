# Description #

Lalr-scm is yet another LALR(1) parser generator written in Scheme. In contrast to other such parser generators, this one implements an efficient algorithm for computing the lookahead sets. The algorithm is the same as used in Bison (GNU yacc) and is described in the following paper:

_Efficient Computation of LALR(1) Look-Ahead Set_, F. DeRemer and T. Pennello, TOPLAS, vol. 4, no. 4, october 1982.

As a consequence, it is not written in a fully functional style. In fact, much of the code is a direct translation from C to Scheme of the Bison sources.

## Source Code ##

Source code is hosted on [Github](http://github.com/schemeway/lalr-scm).

Some versions are also available from the [Downloads](http://code.google.com/p/lalr-scm/downloads/list) section.


## Documentation ##

  * [New Features](NewFeatures.md)
  * [Portability](Portability.md)
  * [Defining a parser](ParserDefinition.md)
  * [Parser syntax](ParserSyntax.md)
  * [GLR Parsing](GlrParsing.md)
  * [Symbol Index](SymbolIndex.md)
  * [Acknowledgments](Acknowledgments.md)
  * [Licensing](Licensing.md)