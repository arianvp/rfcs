- Feature Name: Scrub most partial functions from Prelude
- Start Date: September 16th, 2016
- RFC PR: 
- Haskell Report Issue: 



#######
Summary
#######

Scrub the partial functions from the prelude, and rather define functions returning ``Maybe`` if needed.
Numeric functions are specifically excepted, as the author feels the trade-offs are unlike enough to consider them separately.

These are the functions in question:

- ``head``
- ``tail``
- ``init``
- ``last``
- ``(!!)``
- ``foldl1``
- ``foldr1``
- ``cycle``
- ``maximum``
- ``minimum``
- ``read``



##########
Motivation
##########

Having these functions in Prelude with such convenient names encourages partiality and frustrates efforts to encode invariants in the type system.
A message like "``*** Exception: Prelude.head: empty list``" is seldom helpful, while a more appropriate and useful message can often be chosen at the use site, if the function must be partial.



###############
Detailed design
###############

Delete the definitions of all the given functions from the Report.
Potentially (see "Unresolved Questions") redefine each to return a value of type ``Maybe`` whatever the partial function returned, for example::

	head :: [a] -> Maybe a
	head [] = Nothing
	head (x:_) = Just x
	
	(!!) :: [a] -> Int -> Maybe a
	xs !! n | n < 0 = Nothing
	[] !! _ = Nothing
	(x:_) !! 0 = Just x
	(_:xs) !! n = xs !! (n-1)
	
	read :: Read a => String -> Maybe a
	read s = case [x | (x,t) <- reads s, ("","") <- lex t] of
	    [x] -> Just x
	    _ -> Nothing



#########
Drawbacks
#########

- This is a breaking change; these functions are widely-known and widely-used.
- These functions are sometimes convenient, and at some use sites they are statically known to be defined at the given argument.



############
Alternatives
############

- Status quo

  The grievances with the status quo are given in "Motivation".

- Dependent types

  This would be a major change and would be worth its own RFC at very least.



####################
Unresolved questions
####################

- Are class methods in scope? What happens if `Foldable`, for example, is included in the report?
- Ought these to be redefined to return `Maybe` or not defined at all?
- If they are redefined, ought they to have the same names?
