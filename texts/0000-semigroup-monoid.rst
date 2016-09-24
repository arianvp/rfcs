- Feature Name: Semigroup ⇒ Monoid
- Start Date: September 23rd, 2016
- RFC PR: 
- Haskell Report Issue: 



#######
Summary
#######

::

	class Semigroup a where
	    -- laws: (x <> y) <> z = x <> (y <> z)
	    (<>) :: a -> a -> a
	
	class Semigroup a => Monoid a where
	    -- laws: x <> mempty = x
            --       mempty <> y = y
	    mempty :: a


##########
Motivation
##########

Some types form semigroups but not monoids. A simple example is ``Void``. Another example is this::

	newtype Min a = Min a
	instance Ord a => Semigroup (Min a) where Min x <> Min y = Min (min x y)
	instance (Ord a, Bounded a) => Monoid (Min a) where mempty = Min maxBound

Note no ``Bounded`` constraint on the ``Semigroup`` instance.


Too, we have some suboptimal instances, for example::

	instance Monoid a => Monoid (Maybe a)

could in fact be
::

	instance Semigroup a => Monoid (Maybe a)

if ``Semigroup`` were a superclass of ``Monoid``.



###############
Detailed design
###############

Add these definitions to the Report::

	class Semigroup a where
	    -- laws: (x <> y) <> z = x <> (y <> z)
	    (<>) :: a -> a -> a
	
	class Semigroup a => Monoid a where
	    -- laws: x <> mempty = x
            --       mempty <> y = y
	    mempty :: a


#########
Drawbacks
#########

This breaks code which defines instances of ``Monoid``, and code which defines its own ``(<>)``.



############
Alternatives
############

None known



####################
Unresolved questions
####################

- Where shall ``Semigroup`` and ``Monoid`` be defined?
