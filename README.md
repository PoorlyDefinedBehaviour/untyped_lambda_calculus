```console
--- grammar ---
term ::= terms:
x variable
\x. term abstraction
term term application

true = \t. \f. t
false = \t. \f. f

if = \condition. \consequence. \alternative. condition consequence alternative

if true v w
-> (\condition. \consequence. \alternative. condition consequence alternative) true v w
-> (\consequence. \alternative. true consequence alternative) v w
-> (\alternative. true v alternative) w
-> true v w
-> (\f. v) w
-> v

and = \a. \b. a b false

and true true
-> (\b. true b false) true
-> true true false
-> true

or = \a. \b. a true b

or false true
-> (\a. \b. a true b) false true
-> (\b. false true b) true
-> (false true true)
-> (\t. \f. f) true true
-> (\f. f) true
-> true

not = \a. if a false true

not true
-> (\a. if a false true) true
-> if true false true
-> (\condition. \consequence. \alternative. condition consequence alternative) true false true
-> (\consequence. \alternative. true consequence alternative) false true
-> (\alternative. true false alternative) true
-> true false true
-> (\t. \f. t) false true
-> (\f. false) true
-> false

pair = \first. \second. \selector. selector first second
fst = \pair. pair true
snd = \pair. pair false

fst (pair v w)
-> fst ((\first. \second. \selector. selector first second) v w)
-> fst ((\second. \selector. selector v second) w)
-> fst (\selector. selector v w)
-> (\pair. pair true)(\selector. selector v w)
-> ((\selector. selector v w) true)
-> true v w
-> (\t. \f. t) v w
-> (\f. v) w
-> v

zero = \successor. \zero. zero
one = \successor. \zero. successor zero
two = \successor. \zero. successor (successor zero)
three = \successor. \zero. successor (successor (successor zero))

succ = \numeral. \successor. \zero. successor (numeral successor zero)

plus = \m. \n. \successor. \zero. m successor (n successor z)

times = \m. \n. m (plus n) zero

is_zero = \numeral. numeral (\_. false) true

is_zero zero
-> (\numeral. numeral (\_. false) true) zero
-> zero (\_. false) true
-> (\successor. \zero. zero) (\_. false) true
-> (\zero. zero) true
-> true

is_zero one
-> (\numeral. numeral (\_. false) true) one
-> one (\_. false) true
-> (\successor. \zero. successor zero) (\_. false) true
-> (\zero. (\_. false) zero) true
-> (\_. false) true
-> false

zero_pair = pair zero zero
next_state = \p. pair (snd p) (plus one (snd p))
pred = \numeral. fst (numeral next_state zero_pair)

sub = \m. \n. \successor. \zero. n pred (m successor zero)

to_real_boolean = \church_boolean. church_boolean true false

to_church_boolean = \real_boolean. if real_boolean then true else false

omega = (\x. x x) (\x. x x) // divergent combinator

fix = \f. (\g. f (\y. g g y)) (\g. f (\y. g g y))

Y = \f. (\g. f(g g)) (\g. f(g g)) // simpler call-by-name fixed point combinator
```
