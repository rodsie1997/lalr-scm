# Parser syntax #

The grammar is specified by first giving the list of terminals and the list of non-terminal definitions. Each non-terminal definition is a list where the first element is the non-terminal and the other elements are the right-hand sides (lists of grammar symbols). In addition to this, each rhs can be followed by a semantic action.

For example, consider the following (yacc) grammar for a very simple expression language:
```
  e : e '+' t
    | e '-' t
    | t
    ;
  t : t '*' f
    : t '/' f
    | f
    ;
  f : ID
    ;
```
The same grammar, written for the scheme parser generator, would look like this (with semantic actions)
```
(define expr-parser
  (lalr-parser
   ; Terminal symbols
   (ID + - * /)
   ; Productions
   (e (e + t)    : (+ $1 $3)
      (e - t)    : (- $1 $3)
      (t)        : $1)
   (t (t * f)    : (* $1 $3)
      (t / f)    : (/ $1 $3)
      (f)        : $1)
   (f (ID)       : $1)))
```

In semantic actions, the symbol `$n` refers to the synthesized attribute value of the nth symbol in the production. The value associated with the non-terminal on the left is the result of evaluating the semantic action (it defaults to `#f`).

## Operator precedence and associativity ##

The above grammar implicitly handles operator precedences. It is also possible to explicitly assign precedences and associativity to terminal symbols and productions à la Yacc. Here is a modified (and augmented) version of the grammar:

```
(define expr-parser
 (lalr-parser
  ; Terminal symbols
  (ID
   (left: + -)
   (left: * /)
   (nonassoc: uminus))
  (e (e + e)              : (+ $1 $3)
     (e - e)              : (- $1 $3)
     (e * e)              : (* $1 $3)
     (e / e)              : (/ $1 $3)
     (- e (prec: uminus)) : (- $2)
     (ID)                 : $1)))
```

The `left:` directive is used to specify a set of left-associative operators of the same precedence level, the `right:` directive for right-associative operators, and `nonassoc:` for operators that are not associative. Note the use of the (apparently) useless terminal uminus. It is only defined in order to assign to the penultimate rule a precedence level higher than that of `*` and `/`. The `prec:` directive can only appear as the last element of a rule. Finally, note that precedence levels are incremented from left to right, i.e. the precedence level of `+` and `-` is less than the precedence level of `*` and `/` since the formers appear first in the list of terminal symbols (token definitions).

## Options ##

The following options are available.

  * `(output: name filename)` - copies the parser to the given file. The parser is given the name _name_.
  * `(out-table: filename)` - outputs the parsing tables in filename in a more readable format.
  * `(expect: n)` - don't warn about conflicts if there are _n_ or less conflicts.
  * `(driver: glr)` - generates a GLR parser instead of a regular LALR parser.


## Error recovery ##

lalr-scm implements a very simple error recovery strategy. A production can be of the form

```
   (rulename
      ...
      (error TERMINAL) : action-code)
```

(There can be several such productions for a single rulename.) This will cause the parser to skip all the tokens produced by the lexer that are different than the given `TERMINAL`. For a C-like language, one can synchronize on semicolons and closing curly brackets by writing error rules like these:

```
   (stmt
      (expression SEMICOLON) : ...
      (LBRACKET stmt RBRACKET) : ...
      (error SEMICOLON)
      (error RBRACKET))
```

## A final note on conflict resolution ##

Conflicts in the grammar are handled in a conventional way. In the absence of precedence directives, Shift/Reduce conflicts are resolved by shifting, and Reduce/Reduce conflicts are resolved by choosing the rule listed first in the grammar definition.