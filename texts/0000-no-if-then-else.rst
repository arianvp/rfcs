- Feature Name: No ``if-then-else``
- Start Date: September 23rd, 2016
- RFC PR: 
- Haskell Report Issue: 



#######
Summary
#######

Scrub the syntactic keywords ``if``, ``then``, and ``else``, and their semantics, from the Report.



##########
Motivation
##########

This syntax is vain in a language with non-strict evaluation, as a mere function ``bool :: a -> a -> Bool -> a`` will do.

Its semantics and interaction with other syntax has been a surprise to some, for examples:

- trying to use ``if`` and ``then`` alone like ``when :: Applicative f => Bool -> m () -> m ()``
- not indenting ``then`` or ``else`` further than ``if`` in a ``do`` block, making the parser choke

Unlike a mere function, it is not readily used as an argument to such functions as ``liftA3`` and ``zipWith3``.

In many cases, pattern guards can rather be used::

	x = if p then a else b

vs
::

	x | p = a | otherwise = b


###############
Detailed design
###############

Full proposal given in Summary.


#########
Drawbacks
#########

- This breaks much code.
- ``if-then-else`` means fewer parentheses in some cases.



############
Alternatives
############

None



####################
Unresolved questions
####################

None known
