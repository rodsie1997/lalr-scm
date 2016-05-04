# New Features #

Version **2.4.1**

  * A comprehensive test suite has been added, thanks to [mrc.mgg](http://code.google.com/u/mrc.mgg).
  * Not packaged as a Snowball anymore.

Version **2.4.0**

  * Tokens are now either symbols or records created by `make-lexical-token`.

Version **2.3.0**

  * It is now packaged as a Snowball for the Scheme Now! framework for greater portability.
  * An experimental GLR driver.
  * A bug fix that makes all shift/reduce conflicts to be reported.
  * Better support for interactive parsers (no lookahead needed when the default action is to reduce).
  * Simple error recovery à la Yacc (`error` productions).
  * Parsers can be saved in files (`output:` option).
  * The parsing tables can be output in a human-readable format (`out-table:` option).
  * Suppression of conflicts warnings (`expect:` option).