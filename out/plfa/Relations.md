---
src       : src/plfa/Relations.lagda
title     : "Relations: Inductive definition of relations"
layout    : page
prev      : /Induction/
permalink : /Relations/
next      : /Equality/
---

<pre class="Agda">{% raw %}<a id="170" class="Keyword">module</a> <a id="177" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}" class="Module">plfa.Relations</a> <a id="192" class="Keyword">where</a>{% endraw %}</pre>

After having defined operations such as addition and multiplication,
the next step is to define relations, such as _less than or equal_.

## Imports

<pre class="Agda">{% raw %}<a id="373" class="Keyword">import</a> <a id="380" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html" class="Module">Relation.Binary.PropositionalEquality</a> <a id="418" class="Symbol">as</a> <a id="421" class="Module">Eq</a>
<a id="424" class="Keyword">open</a> <a id="429" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html" class="Module">Eq</a> <a id="432" class="Keyword">using</a> <a id="438" class="Symbol">(</a><a id="439" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Equality.html#83" class="Datatype Operator">_≡_</a><a id="442" class="Symbol">;</a> <a id="444" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Equality.html#140" class="InductiveConstructor">refl</a><a id="448" class="Symbol">;</a> <a id="450" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html#1056" class="Function">cong</a><a id="454" class="Symbol">;</a> <a id="456" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.Core.html#838" class="Function">sym</a><a id="459" class="Symbol">)</a>
<a id="461" class="Keyword">open</a> <a id="466" class="Keyword">import</a> <a id="473" href="https://agda.github.io/agda-stdlib/Data.Nat.html" class="Module">Data.Nat</a> <a id="482" class="Keyword">using</a> <a id="488" class="Symbol">(</a><a id="489" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="490" class="Symbol">;</a> <a id="492" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#115" class="InductiveConstructor">zero</a><a id="496" class="Symbol">;</a> <a id="498" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a><a id="501" class="Symbol">;</a> <a id="503" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">_+_</a><a id="506" class="Symbol">;</a> <a id="508" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#433" class="Primitive Operator">_*_</a><a id="511" class="Symbol">;</a> <a id="513" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#320" class="Primitive Operator">_∸_</a><a id="516" class="Symbol">)</a>
<a id="518" class="Keyword">open</a> <a id="523" class="Keyword">import</a> <a id="530" href="https://agda.github.io/agda-stdlib/Data.Nat.Properties.html" class="Module">Data.Nat.Properties</a> <a id="550" class="Keyword">using</a> <a id="556" class="Symbol">(</a><a id="557" href="https://agda.github.io/agda-stdlib/Data.Nat.Properties.html#8011" class="Function">+-comm</a><a id="563" class="Symbol">;</a> <a id="565" href="https://agda.github.io/agda-stdlib/Data.Nat.Properties.html#7575" class="Function">+-suc</a><a id="570" class="Symbol">)</a>
<a id="572" class="Keyword">open</a> <a id="577" class="Keyword">import</a> <a id="584" href="https://agda.github.io/agda-stdlib/Data.List.html" class="Module">Data.List</a> <a id="594" class="Keyword">using</a> <a id="600" class="Symbol">(</a><a id="601" href="https://agda.github.io/agda-stdlib/Agda.Builtin.List.html#80" class="Datatype">List</a><a id="605" class="Symbol">;</a> <a id="607" href="https://agda.github.io/agda-stdlib/Data.List.Base.html#8019" class="InductiveConstructor">[]</a><a id="609" class="Symbol">;</a> <a id="611" href="https://agda.github.io/agda-stdlib/Agda.Builtin.List.html#132" class="InductiveConstructor Operator">_∷_</a><a id="614" class="Symbol">)</a>
<a id="616" class="Keyword">open</a> <a id="621" class="Keyword">import</a> <a id="628" href="https://agda.github.io/agda-stdlib/Function.html" class="Module">Function</a> <a id="637" class="Keyword">using</a> <a id="643" class="Symbol">(</a><a id="644" href="https://agda.github.io/agda-stdlib/Function.html#1068" class="Function">id</a><a id="646" class="Symbol">;</a> <a id="648" href="https://agda.github.io/agda-stdlib/Function.html#769" class="Function Operator">_∘_</a><a id="651" class="Symbol">)</a>
<a id="653" class="Keyword">open</a> <a id="658" class="Keyword">import</a> <a id="665" href="https://agda.github.io/agda-stdlib/Relation.Nullary.html" class="Module">Relation.Nullary</a> <a id="682" class="Keyword">using</a> <a id="688" class="Symbol">(</a><a id="689" href="https://agda.github.io/agda-stdlib/Relation.Nullary.html#464" class="Function Operator">¬_</a><a id="691" class="Symbol">)</a>
<a id="693" class="Keyword">open</a> <a id="698" class="Keyword">import</a> <a id="705" href="https://agda.github.io/agda-stdlib/Data.Empty.html" class="Module">Data.Empty</a> <a id="716" class="Keyword">using</a> <a id="722" class="Symbol">(</a><a id="723" href="https://agda.github.io/agda-stdlib/Data.Empty.html#243" class="Datatype">⊥</a><a id="724" class="Symbol">;</a> <a id="726" href="https://agda.github.io/agda-stdlib/Data.Empty.html#360" class="Function">⊥-elim</a><a id="732" class="Symbol">)</a>{% endraw %}</pre>


## Defining relations

The relation _less than or equal_ has an infinite number of
instances.  Here are a few of them:

    0 ≤ 0     0 ≤ 1     0 ≤ 2     0 ≤ 3     ...
              1 ≤ 1     1 ≤ 2     1 ≤ 3     ...
                        2 ≤ 2     2 ≤ 3     ...
                                  3 ≤ 3     ...
                                            ...

And yet, we can write a finite definition that encompasses
all of these instances in just a few lines.  Here is the
definition as a pair of inference rules:

    z≤n --------
        zero ≤ n

        m ≤ n
    s≤s -------------
        suc m ≤ suc n

And here is the definition in Agda:
<pre class="Agda">{% raw %}<a id="1409" class="Keyword">data</a> <a id="_≤_"></a><a id="1414" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1414" class="Datatype Operator">_≤_</a> <a id="1418" class="Symbol">:</a> <a id="1420" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a> <a id="1422" class="Symbol">→</a> <a id="1424" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a> <a id="1426" class="Symbol">→</a> <a id="1428" class="PrimitiveType">Set</a> <a id="1432" class="Keyword">where</a>

  <a id="_≤_.z≤n"></a><a id="1441" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1441" class="InductiveConstructor">z≤n</a> <a id="1445" class="Symbol">:</a> <a id="1447" class="Symbol">∀</a> <a id="1449" class="Symbol">{</a><a id="1450" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1450" class="Bound">n</a> <a id="1452" class="Symbol">:</a> <a id="1454" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="1455" class="Symbol">}</a>
      <a id="1463" class="Comment">--------</a>
    <a id="1476" class="Symbol">→</a> <a id="1478" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#115" class="InductiveConstructor">zero</a> <a id="1483" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1414" class="Datatype Operator">≤</a> <a id="1485" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1450" class="Bound">n</a>

  <a id="_≤_.s≤s"></a><a id="1490" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1490" class="InductiveConstructor">s≤s</a> <a id="1494" class="Symbol">:</a> <a id="1496" class="Symbol">∀</a> <a id="1498" class="Symbol">{</a><a id="1499" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1499" class="Bound">m</a> <a id="1501" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1501" class="Bound">n</a> <a id="1503" class="Symbol">:</a> <a id="1505" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="1506" class="Symbol">}</a>
    <a id="1512" class="Symbol">→</a> <a id="1514" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1499" class="Bound">m</a> <a id="1516" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1414" class="Datatype Operator">≤</a> <a id="1518" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1501" class="Bound">n</a>
      <a id="1526" class="Comment">-------------</a>
    <a id="1544" class="Symbol">→</a> <a id="1546" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="1550" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1499" class="Bound">m</a> <a id="1552" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1414" class="Datatype Operator">≤</a> <a id="1554" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="1558" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1501" class="Bound">n</a>{% endraw %}</pre>
Here `z≤n` and `s≤s` (with no spaces) are constructor names, while
`zero ≤ n`, and `m ≤ n` and `suc m ≤ suc n` (with spaces) are types.
This is our first use of an _indexed_ datatype, where the type `m ≤ n`
is indexed by two naturals, `m` and `n`.  In Agda any line beginning
with two or more dashes is a comment, and here we have exploited that
convention to write our Agda code in a form that resembles the
corresponding inference rules, a trick we will use often from now on.

Both definitions above tell us the same two things:

* _Base case_: for all naturals `n`, the proposition `zero ≤ n` holds
* _Inductive case_: for all naturals `m` and `n`, if the proposition
  `m ≤ n` holds, then the proposition `suc m ≤ suc n` holds.

In fact, they each give us a bit more detail:

* _Base case_: for all naturals `n`, the constructor `z≤n`
  produces evidence that `zero ≤ n` holds.
* _Inductive case_: for all naturals `m` and `n`, the constructor
  `s≤s` takes evidence that `m ≤ n` holds into evidence that
  `suc m ≤ suc n` holds.

For example, here in inference rule notation is the proof that
`2 ≤ 4`:

      z≤n -----
          0 ≤ 2
     s≤s -------
          1 ≤ 3
    s≤s ---------
          2 ≤ 4

And here is the corresponding Agda proof:
<pre class="Agda">{% raw %}<a id="2835" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#2835" class="Function">_</a> <a id="2837" class="Symbol">:</a> <a id="2839" class="Number">2</a> <a id="2841" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1414" class="Datatype Operator">≤</a> <a id="2843" class="Number">4</a>
<a id="2845" class="Symbol">_</a> <a id="2847" class="Symbol">=</a> <a id="2849" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1490" class="InductiveConstructor">s≤s</a> <a id="2853" class="Symbol">(</a><a id="2854" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1490" class="InductiveConstructor">s≤s</a> <a id="2858" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1441" class="InductiveConstructor">z≤n</a><a id="2861" class="Symbol">)</a>{% endraw %}</pre>




## Implicit arguments

This is our first use of implicit arguments.  In the definition of
inequality, the two lines defining the constructors use `∀`, very
similar to our use of `∀` in propositions such as:

    +-comm : ∀ (m n : ℕ) → m + n ≡ n + m

However, here the declarations are surrounded by curly braces `{ }`
rather than parentheses `( )`.  This means that the arguments are
_implicit_ and need not be written explicitly; instead, they are
_inferred_ by Agda's typechecker. Thus, we write `+-comm m n` for the
proof that `m + n ≡ n + m`, but `z≤n` for the proof that `zero ≤ n`,
leaving `n` implicit.  Similarly, if `m≤n` is evidence that `m ≤ n`,
we write `s≤s m≤n` for evidence that `suc m ≤ suc n`, leaving both `m`
and `n` implicit.

If we wish, it is possible to provide implicit arguments explicitly by
writing the arguments inside curly braces.  For instance, here is the
Agda proof that `2 ≤ 4` repeated, with the implicit arguments made
explicit:
<pre class="Agda">{% raw %}<a id="3856" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#3856" class="Function">_</a> <a id="3858" class="Symbol">:</a> <a id="3860" class="Number">2</a> <a id="3862" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1414" class="Datatype Operator">≤</a> <a id="3864" class="Number">4</a>
<a id="3866" class="Symbol">_</a> <a id="3868" class="Symbol">=</a> <a id="3870" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1490" class="InductiveConstructor">s≤s</a> <a id="3874" class="Symbol">{</a><a id="3875" class="Number">1</a><a id="3876" class="Symbol">}</a> <a id="3878" class="Symbol">{</a><a id="3879" class="Number">3</a><a id="3880" class="Symbol">}</a> <a id="3882" class="Symbol">(</a><a id="3883" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1490" class="InductiveConstructor">s≤s</a> <a id="3887" class="Symbol">{</a><a id="3888" class="Number">0</a><a id="3889" class="Symbol">}</a> <a id="3891" class="Symbol">{</a><a id="3892" class="Number">2</a><a id="3893" class="Symbol">}</a> <a id="3895" class="Symbol">(</a><a id="3896" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1441" class="InductiveConstructor">z≤n</a> <a id="3900" class="Symbol">{</a><a id="3901" class="Number">2</a><a id="3902" class="Symbol">}))</a>{% endraw %}</pre>
One may also identify implicit arguments by name:
<pre class="Agda">{% raw %}<a id="3980" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#3980" class="Function">_</a> <a id="3982" class="Symbol">:</a> <a id="3984" class="Number">2</a> <a id="3986" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1414" class="Datatype Operator">≤</a> <a id="3988" class="Number">4</a>
<a id="3990" class="Symbol">_</a> <a id="3992" class="Symbol">=</a> <a id="3994" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1490" class="InductiveConstructor">s≤s</a> <a id="3998" class="Symbol">{</a><a id="3999" class="Argument">m</a> <a id="4001" class="Symbol">=</a> <a id="4003" class="Number">1</a><a id="4004" class="Symbol">}</a> <a id="4006" class="Symbol">{</a><a id="4007" class="Argument">n</a> <a id="4009" class="Symbol">=</a> <a id="4011" class="Number">3</a><a id="4012" class="Symbol">}</a> <a id="4014" class="Symbol">(</a><a id="4015" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1490" class="InductiveConstructor">s≤s</a> <a id="4019" class="Symbol">{</a><a id="4020" class="Argument">m</a> <a id="4022" class="Symbol">=</a> <a id="4024" class="Number">0</a><a id="4025" class="Symbol">}</a> <a id="4027" class="Symbol">{</a><a id="4028" class="Argument">n</a> <a id="4030" class="Symbol">=</a> <a id="4032" class="Number">2</a><a id="4033" class="Symbol">}</a> <a id="4035" class="Symbol">(</a><a id="4036" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1441" class="InductiveConstructor">z≤n</a> <a id="4040" class="Symbol">{</a><a id="4041" class="Argument">n</a> <a id="4043" class="Symbol">=</a> <a id="4045" class="Number">2</a><a id="4046" class="Symbol">}))</a>{% endraw %}</pre>
In the latter format, you may only supply some implicit arguments:
<pre class="Agda">{% raw %}<a id="4141" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#4141" class="Function">_</a> <a id="4143" class="Symbol">:</a> <a id="4145" class="Number">2</a> <a id="4147" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1414" class="Datatype Operator">≤</a> <a id="4149" class="Number">4</a>
<a id="4151" class="Symbol">_</a> <a id="4153" class="Symbol">=</a> <a id="4155" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1490" class="InductiveConstructor">s≤s</a> <a id="4159" class="Symbol">{</a><a id="4160" class="Argument">n</a> <a id="4162" class="Symbol">=</a> <a id="4164" class="Number">3</a><a id="4165" class="Symbol">}</a> <a id="4167" class="Symbol">(</a><a id="4168" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1490" class="InductiveConstructor">s≤s</a> <a id="4172" class="Symbol">{</a><a id="4173" class="Argument">n</a> <a id="4175" class="Symbol">=</a> <a id="4177" class="Number">2</a><a id="4178" class="Symbol">}</a> <a id="4180" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1441" class="InductiveConstructor">z≤n</a><a id="4183" class="Symbol">)</a>{% endraw %}</pre>
It is not permitted to swap implicit arguments, even when named.


## Precedence

We declare the precedence for comparison as follows:
<pre class="Agda">{% raw %}<a id="4344" class="Keyword">infix</a> <a id="4350" class="Number">4</a> <a id="4352" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1414" class="Datatype Operator">_≤_</a>{% endraw %}</pre>
We set the precedence of `_≤_` at level 4, so it binds less tightly
that `_+_` at level 6 and hence `1 + 2 ≤ 3` parses as `(1 + 2) ≤ 3`.
We write `infix` to indicate that the operator does not associate to
either the left or right, as it makes no sense to parse `1 ≤ 2 ≤ 3` as
either `(1 ≤ 2) ≤ 3` or `1 ≤ (2 ≤ 3)`.


## Decidability

Given two numbers, it is straightforward to compute whether or not the
first is less than or equal to the second.  We don't give the code for
doing so here, but will return to this point in
Chapter [Decidable]({{ site.baseurl }}{% link out/plfa/Decidable.md%}).


## Properties of ordering relations

Relations pop up all the time, and mathematicians have agreed
on names for some of the most common properties.

* _Reflexive_. For all `n`, the relation `n ≤ n` holds.
* _Transitive_. For all `m`, `n`, and `p`, if `m ≤ n` and
`n ≤ p` hold, then `m ≤ p` holds.
* _Anti-symmetric_. For all `m` and `n`, if both `m ≤ n` and
`n ≤ m` hold, then `m ≡ n` holds.
* _Total_. For all `m` and `n`, either `m ≤ n` or `n ≤ m`
holds.

The relation `_≤_` satisfies all four of these properties.

There are also names for some combinations of these properties.

* _Preorder_. Any relation that is reflexive and transitive.
* _Partial order_. Any preorder that is also anti-symmetric.
* _Total order_. Any partial order that is also total.

If you ever bump into a relation at a party, you now know how
to make small talk, by asking it whether it is reflexive, transitive,
anti-symmetric, and total. Or instead you might ask whether it is a
preorder, partial order, or total order.

Less frivolously, if you ever bump into a relation while reading a
technical paper, this gives you a way to orient yourself, by checking
whether or not it is a preorder, partial order, or total order.  A
careful author will often call out these properties---or their
lack---for instance by saying that a newly introduced relation is a
partial order but not a total order.


#### Exercise `orderings` {#orderings}

Give an example of a preorder that is not a partial order.

Give an example of a partial order that is not a total order.


## Reflexivity

The first property to prove about comparison is that it is reflexive:
for any natural `n`, the relation `n ≤ n` holds.  We follow the
convention in the standard library and make the argument implicit,
as that will make it easier to invoke reflexivity:
<pre class="Agda">{% raw %}<a id="≤-refl"></a><a id="6753" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#6753" class="Function">≤-refl</a> <a id="6760" class="Symbol">:</a> <a id="6762" class="Symbol">∀</a> <a id="6764" class="Symbol">{</a><a id="6765" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#6765" class="Bound">n</a> <a id="6767" class="Symbol">:</a> <a id="6769" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="6770" class="Symbol">}</a>
    <a id="6776" class="Comment">-----</a>
  <a id="6784" class="Symbol">→</a> <a id="6786" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#6765" class="Bound">n</a> <a id="6788" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1414" class="Datatype Operator">≤</a> <a id="6790" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#6765" class="Bound">n</a>
<a id="6792" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#6753" class="Function">≤-refl</a> <a id="6799" class="Symbol">{</a><a id="6800" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#115" class="InductiveConstructor">zero</a><a id="6804" class="Symbol">}</a>   <a id="6808" class="Symbol">=</a>  <a id="6811" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1441" class="InductiveConstructor">z≤n</a>
<a id="6815" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#6753" class="Function">≤-refl</a> <a id="6822" class="Symbol">{</a><a id="6823" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="6827" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#6827" class="Bound">n</a><a id="6828" class="Symbol">}</a>  <a id="6831" class="Symbol">=</a>  <a id="6834" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1490" class="InductiveConstructor">s≤s</a> <a id="6838" class="Symbol">(</a><a id="6839" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#6753" class="Function">≤-refl</a> <a id="6846" class="Symbol">{</a><a id="6847" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#6827" class="Bound">n</a><a id="6848" class="Symbol">})</a>{% endraw %}</pre>
The proof is a straightforward induction on the implicit argument `n`.
In the base case, `zero ≤ zero` holds by `z≤n`.  In the inductive
case, the inductive hypothesis `≤-refl {n}` gives us a proof of `n ≤
n`, and applying `s≤s` to that yields a proof of `suc n ≤ suc n`.

It is a good exercise to prove reflexivity interactively in Emacs,
using holes and the `C-c C-c`, `C-c C-,`, and `C-c C-r` commands.


## Transitivity

The second property to prove about comparison is that it is
transitive: for any naturals `m`, `n`, and `p`, if `m ≤ n` and `n ≤ p`
hold, then `m ≤ p` holds.  Again, `m`, `n`, and `p` are implicit:
<pre class="Agda">{% raw %}<a id="≤-trans"></a><a id="7497" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#7497" class="Function">≤-trans</a> <a id="7505" class="Symbol">:</a> <a id="7507" class="Symbol">∀</a> <a id="7509" class="Symbol">{</a><a id="7510" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#7510" class="Bound">m</a> <a id="7512" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#7512" class="Bound">n</a> <a id="7514" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#7514" class="Bound">p</a> <a id="7516" class="Symbol">:</a> <a id="7518" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="7519" class="Symbol">}</a>
  <a id="7523" class="Symbol">→</a> <a id="7525" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#7510" class="Bound">m</a> <a id="7527" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1414" class="Datatype Operator">≤</a> <a id="7529" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#7512" class="Bound">n</a>
  <a id="7533" class="Symbol">→</a> <a id="7535" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#7512" class="Bound">n</a> <a id="7537" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1414" class="Datatype Operator">≤</a> <a id="7539" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#7514" class="Bound">p</a>
    <a id="7545" class="Comment">-----</a>
  <a id="7553" class="Symbol">→</a> <a id="7555" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#7510" class="Bound">m</a> <a id="7557" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1414" class="Datatype Operator">≤</a> <a id="7559" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#7514" class="Bound">p</a>
<a id="7561" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#7497" class="Function">≤-trans</a> <a id="7569" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1441" class="InductiveConstructor">z≤n</a>       <a id="7579" class="Symbol">_</a>          <a id="7590" class="Symbol">=</a>  <a id="7593" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1441" class="InductiveConstructor">z≤n</a>
<a id="7597" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#7497" class="Function">≤-trans</a> <a id="7605" class="Symbol">(</a><a id="7606" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1490" class="InductiveConstructor">s≤s</a> <a id="7610" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#7610" class="Bound">m≤n</a><a id="7613" class="Symbol">)</a> <a id="7615" class="Symbol">(</a><a id="7616" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1490" class="InductiveConstructor">s≤s</a> <a id="7620" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#7620" class="Bound">n≤p</a><a id="7623" class="Symbol">)</a>  <a id="7626" class="Symbol">=</a>  <a id="7629" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1490" class="InductiveConstructor">s≤s</a> <a id="7633" class="Symbol">(</a><a id="7634" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#7497" class="Function">≤-trans</a> <a id="7642" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#7610" class="Bound">m≤n</a> <a id="7646" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#7620" class="Bound">n≤p</a><a id="7649" class="Symbol">)</a>{% endraw %}</pre>
Here the proof is by induction on the _evidence_ that `m ≤ n`.  In the
base case, the first inequality holds by `z≤n` and must show `zero ≤
p`, which follows immediately by `z≤n`.  In this case, the fact that
`n ≤ p` is irrelevant, and we write `_` as the pattern to indicate
that the corresponding evidence is unused.

In the inductive case, the first inequality holds by `s≤s m≤n`
and the second inequality by `s≤s n≤p`, and so we are given
`suc m ≤ suc n` and `suc n ≤ suc p`, and must show `suc m ≤ suc p`.
The inductive hypothesis `≤-trans m≤n n≤p` establishes
that `m ≤ p`, and our goal follows by applying `s≤s`.

The case `≤-trans (s≤s m≤n) z≤n` cannot arise, since the first
inequality implies the middle value is `suc n` while the second
inequality implies that it is `zero`.  Agda can determine that such a
case cannot arise, and does not require (or permit) it to be listed.

Alternatively, we could make the implicit parameters explicit:
<pre class="Agda">{% raw %}<a id="≤-trans′"></a><a id="8626" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#8626" class="Function">≤-trans′</a> <a id="8635" class="Symbol">:</a> <a id="8637" class="Symbol">∀</a> <a id="8639" class="Symbol">(</a><a id="8640" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#8640" class="Bound">m</a> <a id="8642" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#8642" class="Bound">n</a> <a id="8644" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#8644" class="Bound">p</a> <a id="8646" class="Symbol">:</a> <a id="8648" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="8649" class="Symbol">)</a>
  <a id="8653" class="Symbol">→</a> <a id="8655" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#8640" class="Bound">m</a> <a id="8657" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1414" class="Datatype Operator">≤</a> <a id="8659" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#8642" class="Bound">n</a>
  <a id="8663" class="Symbol">→</a> <a id="8665" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#8642" class="Bound">n</a> <a id="8667" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1414" class="Datatype Operator">≤</a> <a id="8669" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#8644" class="Bound">p</a>
    <a id="8675" class="Comment">-----</a>
  <a id="8683" class="Symbol">→</a> <a id="8685" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#8640" class="Bound">m</a> <a id="8687" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1414" class="Datatype Operator">≤</a> <a id="8689" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#8644" class="Bound">p</a>
<a id="8691" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#8626" class="Function">≤-trans′</a> <a id="8700" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#115" class="InductiveConstructor">zero</a>    <a id="8708" class="Symbol">_</a>       <a id="8716" class="Symbol">_</a>       <a id="8724" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1441" class="InductiveConstructor">z≤n</a>       <a id="8734" class="Symbol">_</a>          <a id="8745" class="Symbol">=</a>  <a id="8748" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1441" class="InductiveConstructor">z≤n</a>
<a id="8752" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#8626" class="Function">≤-trans′</a> <a id="8761" class="Symbol">(</a><a id="8762" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="8766" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#8766" class="Bound">m</a><a id="8767" class="Symbol">)</a> <a id="8769" class="Symbol">(</a><a id="8770" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="8774" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#8774" class="Bound">n</a><a id="8775" class="Symbol">)</a> <a id="8777" class="Symbol">(</a><a id="8778" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="8782" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#8782" class="Bound">p</a><a id="8783" class="Symbol">)</a> <a id="8785" class="Symbol">(</a><a id="8786" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1490" class="InductiveConstructor">s≤s</a> <a id="8790" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#8790" class="Bound">m≤n</a><a id="8793" class="Symbol">)</a> <a id="8795" class="Symbol">(</a><a id="8796" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1490" class="InductiveConstructor">s≤s</a> <a id="8800" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#8800" class="Bound">n≤p</a><a id="8803" class="Symbol">)</a>  <a id="8806" class="Symbol">=</a>  <a id="8809" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1490" class="InductiveConstructor">s≤s</a> <a id="8813" class="Symbol">(</a><a id="8814" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#8626" class="Function">≤-trans′</a> <a id="8823" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#8766" class="Bound">m</a> <a id="8825" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#8774" class="Bound">n</a> <a id="8827" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#8782" class="Bound">p</a> <a id="8829" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#8790" class="Bound">m≤n</a> <a id="8833" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#8800" class="Bound">n≤p</a><a id="8836" class="Symbol">)</a>{% endraw %}</pre>
One might argue that this is clearer or one might argue that the extra
length obscures the essence of the proof.  We will usually opt for
shorter proofs.

The technique of induction on evidence that a property holds (e.g.,
inducting on evidence that `m ≤ n`)---rather than induction on 
values of which the property holds (e.g., inducting on `m`)---will turn
out to be immensely valuable, and one that we use often.

Again, it is a good exercise to prove transitivity interactively in Emacs,
using holes and the `C-c C-c`, `C-c C-,`, and `C-c C-r` commands.


## Anti-symmetry

The third property to prove about comparison is that it is
antisymmetric: for all naturals `m` and `n`, if both `m ≤ n` and `n ≤
m` hold, then `m ≡ n` holds:
<pre class="Agda">{% raw %}<a id="≤-antisym"></a><a id="9598" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#9598" class="Function">≤-antisym</a> <a id="9608" class="Symbol">:</a> <a id="9610" class="Symbol">∀</a> <a id="9612" class="Symbol">{</a><a id="9613" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#9613" class="Bound">m</a> <a id="9615" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#9615" class="Bound">n</a> <a id="9617" class="Symbol">:</a> <a id="9619" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="9620" class="Symbol">}</a>
  <a id="9624" class="Symbol">→</a> <a id="9626" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#9613" class="Bound">m</a> <a id="9628" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1414" class="Datatype Operator">≤</a> <a id="9630" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#9615" class="Bound">n</a>
  <a id="9634" class="Symbol">→</a> <a id="9636" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#9615" class="Bound">n</a> <a id="9638" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1414" class="Datatype Operator">≤</a> <a id="9640" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#9613" class="Bound">m</a>
    <a id="9646" class="Comment">-----</a>
  <a id="9654" class="Symbol">→</a> <a id="9656" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#9613" class="Bound">m</a> <a id="9658" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Equality.html#83" class="Datatype Operator">≡</a> <a id="9660" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#9615" class="Bound">n</a>
<a id="9662" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#9598" class="Function">≤-antisym</a> <a id="9672" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1441" class="InductiveConstructor">z≤n</a>       <a id="9682" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1441" class="InductiveConstructor">z≤n</a>        <a id="9693" class="Symbol">=</a>  <a id="9696" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Equality.html#140" class="InductiveConstructor">refl</a>
<a id="9701" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#9598" class="Function">≤-antisym</a> <a id="9711" class="Symbol">(</a><a id="9712" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1490" class="InductiveConstructor">s≤s</a> <a id="9716" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#9716" class="Bound">m≤n</a><a id="9719" class="Symbol">)</a> <a id="9721" class="Symbol">(</a><a id="9722" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1490" class="InductiveConstructor">s≤s</a> <a id="9726" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#9726" class="Bound">n≤m</a><a id="9729" class="Symbol">)</a>  <a id="9732" class="Symbol">=</a>  <a id="9735" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html#1056" class="Function">cong</a> <a id="9740" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="9744" class="Symbol">(</a><a id="9745" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#9598" class="Function">≤-antisym</a> <a id="9755" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#9716" class="Bound">m≤n</a> <a id="9759" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#9726" class="Bound">n≤m</a><a id="9762" class="Symbol">)</a>{% endraw %}</pre>
Again, the proof is by induction over the evidence that `m ≤ n`
and `n ≤ m` hold.

In the base case, both inequalities hold by `z≤n`, and so we are given
`zero ≤ zero` and `zero ≤ zero` and must show `zero ≡ zero`, which
follows by reflexivity.  (Reflexivity of equality, that is, not
reflexivity of inequality.)

In the inductive case, the first inequality holds by `s≤s m≤n` and the
second inequality holds by `s≤s n≤m`, and so we are given `suc m ≤ suc n`
and `suc n ≤ suc m` and must show `suc m ≡ suc n`.  The inductive
hypothesis `≤-antisym m≤n n≤m` establishes that `m ≡ n`, and our goal
follows by congruence.

#### Exercise `≤-antisym-cases` {#leq-antisym-cases} 

The above proof omits cases where one argument is `z≤n` and one
argument is `s≤s`.  Why is it ok to omit them?


## Total

The fourth property to prove about comparison is that it is total:
for any naturals `m` and `n` either `m ≤ n` or `n ≤ m`, or both if
`m` and `n` are equal.

We specify what it means for inequality to be total:
<pre class="Agda">{% raw %}<a id="10796" class="Keyword">data</a> <a id="Total"></a><a id="10801" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10801" class="Datatype">Total</a> <a id="10807" class="Symbol">(</a><a id="10808" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10808" class="Bound">m</a> <a id="10810" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10810" class="Bound">n</a> <a id="10812" class="Symbol">:</a> <a id="10814" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="10815" class="Symbol">)</a> <a id="10817" class="Symbol">:</a> <a id="10819" class="PrimitiveType">Set</a> <a id="10823" class="Keyword">where</a>

  <a id="Total.forward"></a><a id="10832" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10832" class="InductiveConstructor">forward</a> <a id="10840" class="Symbol">:</a>
      <a id="10848" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10808" class="Bound">m</a> <a id="10850" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1414" class="Datatype Operator">≤</a> <a id="10852" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10810" class="Bound">n</a>
      <a id="10860" class="Comment">---------</a>
    <a id="10874" class="Symbol">→</a> <a id="10876" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10801" class="Datatype">Total</a> <a id="10882" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10808" class="Bound">m</a> <a id="10884" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10810" class="Bound">n</a>

  <a id="Total.flipped"></a><a id="10889" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10889" class="InductiveConstructor">flipped</a> <a id="10897" class="Symbol">:</a>
      <a id="10905" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10810" class="Bound">n</a> <a id="10907" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1414" class="Datatype Operator">≤</a> <a id="10909" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10808" class="Bound">m</a>
      <a id="10917" class="Comment">---------</a>
    <a id="10931" class="Symbol">→</a> <a id="10933" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10801" class="Datatype">Total</a> <a id="10939" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10808" class="Bound">m</a> <a id="10941" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10810" class="Bound">n</a>{% endraw %}</pre>
Evidence that `Total m n` holds is either of the form
`forward m≤n` or `flipped n≤m`, where `m≤n` and `n≤m` are
evidence of `m ≤ n` and `n ≤ m` respectively.

(For those familiar with logic, the above definition
could also be written as a disjunction. Disjunctions will
be introduced in Chapter [Connectives]({{ site.baseurl }}{% link out/plfa/Connectives.md%}).)

This is our first use of a datatype with _parameters_,
in this case `m` and `n`.  It is equivalent to the following
indexed datatype:
<pre class="Agda">{% raw %}<a id="11431" class="Keyword">data</a> <a id="Total′"></a><a id="11436" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#11436" class="Datatype">Total′</a> <a id="11443" class="Symbol">:</a> <a id="11445" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a> <a id="11447" class="Symbol">→</a> <a id="11449" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a> <a id="11451" class="Symbol">→</a> <a id="11453" class="PrimitiveType">Set</a> <a id="11457" class="Keyword">where</a>

  <a id="Total′.forward′"></a><a id="11466" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#11466" class="InductiveConstructor">forward′</a> <a id="11475" class="Symbol">:</a> <a id="11477" class="Symbol">∀</a> <a id="11479" class="Symbol">{</a><a id="11480" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#11480" class="Bound">m</a> <a id="11482" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#11482" class="Bound">n</a> <a id="11484" class="Symbol">:</a> <a id="11486" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="11487" class="Symbol">}</a>
    <a id="11493" class="Symbol">→</a> <a id="11495" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#11480" class="Bound">m</a> <a id="11497" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1414" class="Datatype Operator">≤</a> <a id="11499" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#11482" class="Bound">n</a>
      <a id="11507" class="Comment">----------</a>
    <a id="11522" class="Symbol">→</a> <a id="11524" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#11436" class="Datatype">Total′</a> <a id="11531" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#11480" class="Bound">m</a> <a id="11533" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#11482" class="Bound">n</a>

  <a id="Total′.flipped′"></a><a id="11538" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#11538" class="InductiveConstructor">flipped′</a> <a id="11547" class="Symbol">:</a> <a id="11549" class="Symbol">∀</a> <a id="11551" class="Symbol">{</a><a id="11552" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#11552" class="Bound">m</a> <a id="11554" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#11554" class="Bound">n</a> <a id="11556" class="Symbol">:</a> <a id="11558" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="11559" class="Symbol">}</a>
    <a id="11565" class="Symbol">→</a> <a id="11567" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#11554" class="Bound">n</a> <a id="11569" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1414" class="Datatype Operator">≤</a> <a id="11571" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#11552" class="Bound">m</a>
      <a id="11579" class="Comment">----------</a>
    <a id="11594" class="Symbol">→</a> <a id="11596" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#11436" class="Datatype">Total′</a> <a id="11603" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#11552" class="Bound">m</a> <a id="11605" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#11554" class="Bound">n</a>{% endraw %}</pre>
Each parameter of the type translates as an implicit parameter of each
constructor.  Unlike an indexed datatype, where the indexes can vary
(as in `zero ≤ n` and `suc m ≤ suc n`), in a parameterised datatype
the parameters must always be the same (as in `Total m n`).
Parameterised declarations are shorter, easier to read, and
occasionally aid Agda's termination checker, so we will use them in
preference to indexed types when possible.

With that preliminary out of the way, we specify and prove totality:
<pre class="Agda">{% raw %}<a id="≤-total"></a><a id="12140" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#12140" class="Function">≤-total</a> <a id="12148" class="Symbol">:</a> <a id="12150" class="Symbol">∀</a> <a id="12152" class="Symbol">(</a><a id="12153" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#12153" class="Bound">m</a> <a id="12155" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#12155" class="Bound">n</a> <a id="12157" class="Symbol">:</a> <a id="12159" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="12160" class="Symbol">)</a> <a id="12162" class="Symbol">→</a> <a id="12164" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10801" class="Datatype">Total</a> <a id="12170" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#12153" class="Bound">m</a> <a id="12172" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#12155" class="Bound">n</a>
<a id="12174" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#12140" class="Function">≤-total</a> <a id="12182" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#115" class="InductiveConstructor">zero</a>    <a id="12190" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#12190" class="Bound">n</a>                         <a id="12216" class="Symbol">=</a>  <a id="12219" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10832" class="InductiveConstructor">forward</a> <a id="12227" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1441" class="InductiveConstructor">z≤n</a>
<a id="12231" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#12140" class="Function">≤-total</a> <a id="12239" class="Symbol">(</a><a id="12240" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="12244" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#12244" class="Bound">m</a><a id="12245" class="Symbol">)</a> <a id="12247" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#115" class="InductiveConstructor">zero</a>                      <a id="12273" class="Symbol">=</a>  <a id="12276" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10889" class="InductiveConstructor">flipped</a> <a id="12284" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1441" class="InductiveConstructor">z≤n</a>
<a id="12288" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#12140" class="Function">≤-total</a> <a id="12296" class="Symbol">(</a><a id="12297" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="12301" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#12301" class="Bound">m</a><a id="12302" class="Symbol">)</a> <a id="12304" class="Symbol">(</a><a id="12305" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="12309" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#12309" class="Bound">n</a><a id="12310" class="Symbol">)</a> <a id="12312" class="Keyword">with</a> <a id="12317" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#12140" class="Function">≤-total</a> <a id="12325" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#12301" class="Bound">m</a> <a id="12327" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#12309" class="Bound">n</a>
<a id="12329" class="Symbol">...</a>                        <a id="12356" class="Symbol">|</a> <a id="12358" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10832" class="InductiveConstructor">forward</a> <a id="12366" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#12366" class="Bound">m≤n</a>  <a id="12371" class="Symbol">=</a>  <a id="12374" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10832" class="InductiveConstructor">forward</a> <a id="12382" class="Symbol">(</a><a id="12383" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1490" class="InductiveConstructor">s≤s</a> <a id="12387" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#12366" class="Bound">m≤n</a><a id="12390" class="Symbol">)</a>
<a id="12392" class="Symbol">...</a>                        <a id="12419" class="Symbol">|</a> <a id="12421" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10889" class="InductiveConstructor">flipped</a> <a id="12429" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#12429" class="Bound">n≤m</a>  <a id="12434" class="Symbol">=</a>  <a id="12437" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10889" class="InductiveConstructor">flipped</a> <a id="12445" class="Symbol">(</a><a id="12446" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1490" class="InductiveConstructor">s≤s</a> <a id="12450" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#12429" class="Bound">n≤m</a><a id="12453" class="Symbol">)</a>{% endraw %}</pre>
In this case the proof is by induction over both the first
and second arguments.  We perform a case analysis:

* _First base case_: If the first argument is `zero` and the
  second argument is `n` then the forward case holds,
  with `z≤n` as evidence that `zero ≤ n`.

* _Second base case_: If the first argument is `suc m` and the
  second argument is `zero` then the flipped case holds, with
  `z≤n` as evidence that `zero ≤ suc m`.

* _Inductive case_: If the first argument is `suc m` and the
  second argument is `suc n`, then the inductive hypothesis
  `≤-total m n` establishes one of the following:

  + The forward case of the inductive hypothesis holds with `m≤n` as
    evidence that `m ≤ n`, from which it follows that the forward case of the
    proposition holds with `s≤s m≤n` as evidence that `suc m ≤ suc n`.

  + The flipped case of the inductive hypothesis holds with `n≤m` as
    evidence that `n ≤ m`, from which it follows that the flipped case of the
    proposition holds with `s≤s n≤m` as evidence that `suc n ≤ suc m`.

This is our first use of the `with` clause in Agda.  The keyword
`with` is followed by an expression and one or more subsequent lines.
Each line begins with an ellipsis (`...`) and a vertical bar (`|`),
followed by a pattern to be matched against the expression
and the right-hand side of the equation.

Every use of `with` is equivalent to defining a helper function.  For
example, the definition above is equivalent to the following:
<pre class="Agda">{% raw %}<a id="≤-total′"></a><a id="13961" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#13961" class="Function">≤-total′</a> <a id="13970" class="Symbol">:</a> <a id="13972" class="Symbol">∀</a> <a id="13974" class="Symbol">(</a><a id="13975" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#13975" class="Bound">m</a> <a id="13977" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#13977" class="Bound">n</a> <a id="13979" class="Symbol">:</a> <a id="13981" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="13982" class="Symbol">)</a> <a id="13984" class="Symbol">→</a> <a id="13986" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10801" class="Datatype">Total</a> <a id="13992" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#13975" class="Bound">m</a> <a id="13994" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#13977" class="Bound">n</a>
<a id="13996" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#13961" class="Function">≤-total′</a> <a id="14005" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#115" class="InductiveConstructor">zero</a>    <a id="14013" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#14013" class="Bound">n</a>        <a id="14022" class="Symbol">=</a>  <a id="14025" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10832" class="InductiveConstructor">forward</a> <a id="14033" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1441" class="InductiveConstructor">z≤n</a>
<a id="14037" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#13961" class="Function">≤-total′</a> <a id="14046" class="Symbol">(</a><a id="14047" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="14051" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#14051" class="Bound">m</a><a id="14052" class="Symbol">)</a> <a id="14054" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#115" class="InductiveConstructor">zero</a>     <a id="14063" class="Symbol">=</a>  <a id="14066" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10889" class="InductiveConstructor">flipped</a> <a id="14074" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1441" class="InductiveConstructor">z≤n</a>
<a id="14078" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#13961" class="Function">≤-total′</a> <a id="14087" class="Symbol">(</a><a id="14088" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="14092" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#14092" class="Bound">m</a><a id="14093" class="Symbol">)</a> <a id="14095" class="Symbol">(</a><a id="14096" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="14100" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#14100" class="Bound">n</a><a id="14101" class="Symbol">)</a>  <a id="14104" class="Symbol">=</a>  <a id="14107" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#14139" class="Function">helper</a> <a id="14114" class="Symbol">(</a><a id="14115" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#13961" class="Function">≤-total′</a> <a id="14124" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#14092" class="Bound">m</a> <a id="14126" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#14100" class="Bound">n</a><a id="14127" class="Symbol">)</a>
  <a id="14131" class="Keyword">where</a>
  <a id="14139" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#14139" class="Function">helper</a> <a id="14146" class="Symbol">:</a> <a id="14148" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10801" class="Datatype">Total</a> <a id="14154" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#14092" class="Bound">m</a> <a id="14156" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#14100" class="Bound">n</a> <a id="14158" class="Symbol">→</a> <a id="14160" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10801" class="Datatype">Total</a> <a id="14166" class="Symbol">(</a><a id="14167" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="14171" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#14092" class="Bound">m</a><a id="14172" class="Symbol">)</a> <a id="14174" class="Symbol">(</a><a id="14175" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="14179" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#14100" class="Bound">n</a><a id="14180" class="Symbol">)</a>
  <a id="14184" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#14139" class="Function">helper</a> <a id="14191" class="Symbol">(</a><a id="14192" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10832" class="InductiveConstructor">forward</a> <a id="14200" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#14200" class="Bound">m≤n</a><a id="14203" class="Symbol">)</a>  <a id="14206" class="Symbol">=</a>  <a id="14209" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10832" class="InductiveConstructor">forward</a> <a id="14217" class="Symbol">(</a><a id="14218" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1490" class="InductiveConstructor">s≤s</a> <a id="14222" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#14200" class="Bound">m≤n</a><a id="14225" class="Symbol">)</a>
  <a id="14229" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#14139" class="Function">helper</a> <a id="14236" class="Symbol">(</a><a id="14237" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10889" class="InductiveConstructor">flipped</a> <a id="14245" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#14245" class="Bound">n≤m</a><a id="14248" class="Symbol">)</a>  <a id="14251" class="Symbol">=</a>  <a id="14254" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10889" class="InductiveConstructor">flipped</a> <a id="14262" class="Symbol">(</a><a id="14263" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1490" class="InductiveConstructor">s≤s</a> <a id="14267" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#14245" class="Bound">n≤m</a><a id="14270" class="Symbol">)</a>{% endraw %}</pre>
This is also our first use of a `where` clause in Agda.  The keyword `where` is
followed by one or more definitions, which must be indented.  Any variables
bound of the left-hand side of the preceding equation (in this case, `m` and
`n`) are in scope within the nested definition, and any identifiers bound in the
nested definition (in this case, `helper`) are in scope in the right-hand side
of the preceding equation.

If both arguments are equal, then both cases hold and we could return evidence
of either.  In the code above we return the forward case, but there is a
variant that returns the flipped case:
<pre class="Agda">{% raw %}<a id="≤-total″"></a><a id="14908" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#14908" class="Function">≤-total″</a> <a id="14917" class="Symbol">:</a> <a id="14919" class="Symbol">∀</a> <a id="14921" class="Symbol">(</a><a id="14922" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#14922" class="Bound">m</a> <a id="14924" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#14924" class="Bound">n</a> <a id="14926" class="Symbol">:</a> <a id="14928" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="14929" class="Symbol">)</a> <a id="14931" class="Symbol">→</a> <a id="14933" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10801" class="Datatype">Total</a> <a id="14939" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#14922" class="Bound">m</a> <a id="14941" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#14924" class="Bound">n</a>
<a id="14943" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#14908" class="Function">≤-total″</a> <a id="14952" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#14952" class="Bound">m</a>       <a id="14960" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#115" class="InductiveConstructor">zero</a>                      <a id="14986" class="Symbol">=</a>  <a id="14989" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10889" class="InductiveConstructor">flipped</a> <a id="14997" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1441" class="InductiveConstructor">z≤n</a>
<a id="15001" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#14908" class="Function">≤-total″</a> <a id="15010" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#115" class="InductiveConstructor">zero</a>    <a id="15018" class="Symbol">(</a><a id="15019" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="15023" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15023" class="Bound">n</a><a id="15024" class="Symbol">)</a>                   <a id="15044" class="Symbol">=</a>  <a id="15047" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10832" class="InductiveConstructor">forward</a> <a id="15055" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1441" class="InductiveConstructor">z≤n</a>
<a id="15059" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#14908" class="Function">≤-total″</a> <a id="15068" class="Symbol">(</a><a id="15069" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="15073" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15073" class="Bound">m</a><a id="15074" class="Symbol">)</a> <a id="15076" class="Symbol">(</a><a id="15077" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="15081" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15081" class="Bound">n</a><a id="15082" class="Symbol">)</a> <a id="15084" class="Keyword">with</a> <a id="15089" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#14908" class="Function">≤-total″</a> <a id="15098" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15073" class="Bound">m</a> <a id="15100" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15081" class="Bound">n</a>
<a id="15102" class="Symbol">...</a>                        <a id="15129" class="Symbol">|</a> <a id="15131" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10832" class="InductiveConstructor">forward</a> <a id="15139" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15139" class="Bound">m≤n</a>   <a id="15145" class="Symbol">=</a>  <a id="15148" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10832" class="InductiveConstructor">forward</a> <a id="15156" class="Symbol">(</a><a id="15157" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1490" class="InductiveConstructor">s≤s</a> <a id="15161" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15139" class="Bound">m≤n</a><a id="15164" class="Symbol">)</a>
<a id="15166" class="Symbol">...</a>                        <a id="15193" class="Symbol">|</a> <a id="15195" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10889" class="InductiveConstructor">flipped</a> <a id="15203" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15203" class="Bound">n≤m</a>   <a id="15209" class="Symbol">=</a>  <a id="15212" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#10889" class="InductiveConstructor">flipped</a> <a id="15220" class="Symbol">(</a><a id="15221" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1490" class="InductiveConstructor">s≤s</a> <a id="15225" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15203" class="Bound">n≤m</a><a id="15228" class="Symbol">)</a>{% endraw %}</pre>
It differs from the original code in that it pattern
matches on the second argument before the first argument.


## Monotonicity

If one bumps into both an operator and an ordering at a party, one may ask if
the operator is _monotonic_ with regard to the ordering.  For example, addition
is monotonic with regard to inequality, meaning:

    ∀ {m n p q : ℕ} → m ≤ n → p ≤ q → m + p ≤ n + q

The proof is straightforward using the techniques we have learned, and is best
broken into three parts. First, we deal with the special case of showing
addition is monotonic on the right:
<pre class="Agda">{% raw %}<a id="+-monoʳ-≤"></a><a id="15833" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15833" class="Function">+-monoʳ-≤</a> <a id="15843" class="Symbol">:</a> <a id="15845" class="Symbol">∀</a> <a id="15847" class="Symbol">(</a><a id="15848" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15848" class="Bound">m</a> <a id="15850" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15850" class="Bound">p</a> <a id="15852" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15852" class="Bound">q</a> <a id="15854" class="Symbol">:</a> <a id="15856" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="15857" class="Symbol">)</a>
  <a id="15861" class="Symbol">→</a> <a id="15863" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15850" class="Bound">p</a> <a id="15865" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1414" class="Datatype Operator">≤</a> <a id="15867" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15852" class="Bound">q</a>
    <a id="15873" class="Comment">-------------</a>
  <a id="15889" class="Symbol">→</a> <a id="15891" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15848" class="Bound">m</a> <a id="15893" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="15895" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15850" class="Bound">p</a> <a id="15897" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1414" class="Datatype Operator">≤</a> <a id="15899" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15848" class="Bound">m</a> <a id="15901" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="15903" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15852" class="Bound">q</a>
<a id="15905" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15833" class="Function">+-monoʳ-≤</a> <a id="15915" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#115" class="InductiveConstructor">zero</a>    <a id="15923" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15923" class="Bound">p</a> <a id="15925" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15925" class="Bound">q</a> <a id="15927" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15927" class="Bound">p≤q</a>  <a id="15932" class="Symbol">=</a>  <a id="15935" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15927" class="Bound">p≤q</a>
<a id="15939" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15833" class="Function">+-monoʳ-≤</a> <a id="15949" class="Symbol">(</a><a id="15950" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="15954" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15954" class="Bound">m</a><a id="15955" class="Symbol">)</a> <a id="15957" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15957" class="Bound">p</a> <a id="15959" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15959" class="Bound">q</a> <a id="15961" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15961" class="Bound">p≤q</a>  <a id="15966" class="Symbol">=</a>  <a id="15969" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1490" class="InductiveConstructor">s≤s</a> <a id="15973" class="Symbol">(</a><a id="15974" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15833" class="Function">+-monoʳ-≤</a> <a id="15984" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15954" class="Bound">m</a> <a id="15986" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15957" class="Bound">p</a> <a id="15988" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15959" class="Bound">q</a> <a id="15990" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15961" class="Bound">p≤q</a><a id="15993" class="Symbol">)</a>{% endraw %}</pre>
The proof is by induction on the first argument.

* _Base case_: The first argument is `zero` in which case
  `zero + p ≤ zero + q` simplifies to `p ≤ q`, the evidence
  for which is given by the argument `p≤q`.

* _Inductive case_: The first argument is `suc m`, in which case
  `suc m + p ≤ suc m + q` simplifies to `suc (m + p) ≤ suc (m + q)`.
  The inductive hypothesis `+-monoʳ-≤ m p q p≤q` establishes that
  `m + p ≤ m + q`, and our goal follows by applying `s≤s`.

Second, we deal with the special case of showing addition is
monotonic on the left. This follows from the previous
result and the commutativity of addition:
<pre class="Agda">{% raw %}<a id="+-monoˡ-≤"></a><a id="16649" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16649" class="Function">+-monoˡ-≤</a> <a id="16659" class="Symbol">:</a> <a id="16661" class="Symbol">∀</a> <a id="16663" class="Symbol">(</a><a id="16664" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16664" class="Bound">m</a> <a id="16666" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16666" class="Bound">n</a> <a id="16668" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16668" class="Bound">p</a> <a id="16670" class="Symbol">:</a> <a id="16672" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="16673" class="Symbol">)</a>
  <a id="16677" class="Symbol">→</a> <a id="16679" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16664" class="Bound">m</a> <a id="16681" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1414" class="Datatype Operator">≤</a> <a id="16683" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16666" class="Bound">n</a>
    <a id="16689" class="Comment">-------------</a>
  <a id="16705" class="Symbol">→</a> <a id="16707" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16664" class="Bound">m</a> <a id="16709" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="16711" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16668" class="Bound">p</a> <a id="16713" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1414" class="Datatype Operator">≤</a> <a id="16715" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16666" class="Bound">n</a> <a id="16717" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="16719" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16668" class="Bound">p</a>
<a id="16721" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16649" class="Function">+-monoˡ-≤</a> <a id="16731" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16731" class="Bound">m</a> <a id="16733" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16733" class="Bound">n</a> <a id="16735" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16735" class="Bound">p</a> <a id="16737" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16737" class="Bound">m≤n</a>  <a id="16742" class="Keyword">rewrite</a> <a id="16750" href="https://agda.github.io/agda-stdlib/Data.Nat.Properties.html#8011" class="Function">+-comm</a> <a id="16757" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16731" class="Bound">m</a> <a id="16759" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16735" class="Bound">p</a> <a id="16761" class="Symbol">|</a> <a id="16763" href="https://agda.github.io/agda-stdlib/Data.Nat.Properties.html#8011" class="Function">+-comm</a> <a id="16770" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16733" class="Bound">n</a> <a id="16772" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16735" class="Bound">p</a>  <a id="16775" class="Symbol">=</a> <a id="16777" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15833" class="Function">+-monoʳ-≤</a> <a id="16787" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16735" class="Bound">p</a> <a id="16789" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16731" class="Bound">m</a> <a id="16791" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16733" class="Bound">n</a> <a id="16793" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16737" class="Bound">m≤n</a>{% endraw %}</pre>
Rewriting by `+-comm m p` and `+-comm n p` converts `m + p ≤ n + p` into
`p + m ≤ p + n`, which is proved by invoking `+-monoʳ-≤ p m n m≤n`.

Third, we combine the two previous results:
<pre class="Agda">{% raw %}<a id="+-mono-≤"></a><a id="17007" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17007" class="Function">+-mono-≤</a> <a id="17016" class="Symbol">:</a> <a id="17018" class="Symbol">∀</a> <a id="17020" class="Symbol">(</a><a id="17021" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17021" class="Bound">m</a> <a id="17023" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17023" class="Bound">n</a> <a id="17025" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17025" class="Bound">p</a> <a id="17027" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17027" class="Bound">q</a> <a id="17029" class="Symbol">:</a> <a id="17031" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="17032" class="Symbol">)</a>
  <a id="17036" class="Symbol">→</a> <a id="17038" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17021" class="Bound">m</a> <a id="17040" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1414" class="Datatype Operator">≤</a> <a id="17042" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17023" class="Bound">n</a>
  <a id="17046" class="Symbol">→</a> <a id="17048" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17025" class="Bound">p</a> <a id="17050" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1414" class="Datatype Operator">≤</a> <a id="17052" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17027" class="Bound">q</a>
    <a id="17058" class="Comment">-------------</a>
  <a id="17074" class="Symbol">→</a> <a id="17076" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17021" class="Bound">m</a> <a id="17078" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="17080" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17025" class="Bound">p</a> <a id="17082" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#1414" class="Datatype Operator">≤</a> <a id="17084" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17023" class="Bound">n</a> <a id="17086" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="17088" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17027" class="Bound">q</a>
<a id="17090" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17007" class="Function">+-mono-≤</a> <a id="17099" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17099" class="Bound">m</a> <a id="17101" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17101" class="Bound">n</a> <a id="17103" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17103" class="Bound">p</a> <a id="17105" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17105" class="Bound">q</a> <a id="17107" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17107" class="Bound">m≤n</a> <a id="17111" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17111" class="Bound">p≤q</a>  <a id="17116" class="Symbol">=</a>  <a id="17119" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#7497" class="Function">≤-trans</a> <a id="17127" class="Symbol">(</a><a id="17128" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#16649" class="Function">+-monoˡ-≤</a> <a id="17138" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17099" class="Bound">m</a> <a id="17140" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17101" class="Bound">n</a> <a id="17142" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17103" class="Bound">p</a> <a id="17144" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17107" class="Bound">m≤n</a><a id="17147" class="Symbol">)</a> <a id="17149" class="Symbol">(</a><a id="17150" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#15833" class="Function">+-monoʳ-≤</a> <a id="17160" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17101" class="Bound">n</a> <a id="17162" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17103" class="Bound">p</a> <a id="17164" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17105" class="Bound">q</a> <a id="17166" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17111" class="Bound">p≤q</a><a id="17169" class="Symbol">)</a>{% endraw %}</pre>
Invoking `+-monoˡ-≤ m n p m≤n` proves `m + p ≤ n + p` and invoking
`+-monoʳ-≤ n p q p≤q` proves `n + p ≤ n + q`, and combining these with
transitivity proves `m + p ≤ n + q`, as was to be shown.


#### Exercise `*-mono-≤` (stretch)

Show that multiplication is monotonic with regard to inequality.


## Strict inequality {#strict-inequality}

We can define strict inequality similarly to inequality:
<pre class="Agda">{% raw %}<a id="17595" class="Keyword">infix</a> <a id="17601" class="Number">4</a> <a id="17603" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17613" class="Datatype Operator">_&lt;_</a>

<a id="17608" class="Keyword">data</a> <a id="_&lt;_"></a><a id="17613" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17613" class="Datatype Operator">_&lt;_</a> <a id="17617" class="Symbol">:</a> <a id="17619" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a> <a id="17621" class="Symbol">→</a> <a id="17623" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a> <a id="17625" class="Symbol">→</a> <a id="17627" class="PrimitiveType">Set</a> <a id="17631" class="Keyword">where</a>

  <a id="_&lt;_.z&lt;s"></a><a id="17640" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17640" class="InductiveConstructor">z&lt;s</a> <a id="17644" class="Symbol">:</a> <a id="17646" class="Symbol">∀</a> <a id="17648" class="Symbol">{</a><a id="17649" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17649" class="Bound">n</a> <a id="17651" class="Symbol">:</a> <a id="17653" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="17654" class="Symbol">}</a>
      <a id="17662" class="Comment">------------</a>
    <a id="17679" class="Symbol">→</a> <a id="17681" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#115" class="InductiveConstructor">zero</a> <a id="17686" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17613" class="Datatype Operator">&lt;</a> <a id="17688" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="17692" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17649" class="Bound">n</a>

  <a id="_&lt;_.s&lt;s"></a><a id="17697" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17697" class="InductiveConstructor">s&lt;s</a> <a id="17701" class="Symbol">:</a> <a id="17703" class="Symbol">∀</a> <a id="17705" class="Symbol">{</a><a id="17706" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17706" class="Bound">m</a> <a id="17708" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17708" class="Bound">n</a> <a id="17710" class="Symbol">:</a> <a id="17712" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="17713" class="Symbol">}</a>
    <a id="17719" class="Symbol">→</a> <a id="17721" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17706" class="Bound">m</a> <a id="17723" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17613" class="Datatype Operator">&lt;</a> <a id="17725" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17708" class="Bound">n</a>
      <a id="17733" class="Comment">-------------</a>
    <a id="17751" class="Symbol">→</a> <a id="17753" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="17757" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17706" class="Bound">m</a> <a id="17759" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17613" class="Datatype Operator">&lt;</a> <a id="17761" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="17765" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#17708" class="Bound">n</a>{% endraw %}</pre>
The key difference is that zero is less than the successor of an
arbitrary number, but is not less than zero.

Clearly, strict inequality is not reflexive. However it is
_irreflexive_ in that `n < n` never holds for any value of `n`.
Like inequality, strict inequality is transitive.
Strict inequality is not total, but satisfies the closely related property of
_trichotomy_: for any `m` and `n`, exactly one of `m < n`, `m ≡ n`, or `m > n`
holds (where we define `m > n` to hold exactly when `n < m`).
It is also monotonic with regards to addition and multiplication.

Most of the above are considered in exercises below.  Irreflexivity
requires negation, as does the fact that the three cases in
trichotomy are mutually exclusive, so those points are deferred to
Chapter [Negation]({{ site.baseurl }}{% link out/plfa/Negation.md%}).

It is straightforward to show that `suc m ≤ n` implies `m < n`,
and conversely.  One can then give an alternative derivation of the
properties of strict inequality, such as transitivity, by directly
exploiting the corresponding properties of inequality.

#### Exercise `<-trans` (recommended) {#less-trans}

Show that strict inequality is transitive.

#### Exercise `trichotomy` {#trichotomy}

Show that strict inequality satisfies a weak version of trichotomy, in
the sense that for any `m` and `n` that one of the following holds:
  * `m < n`,
  * `m ≡ n`, or
  * `m > n`
Define `m > n` to be the same as `n < m`.
You will need a suitable data declaration,
similar to that used for totality.
(We will show that the three cases are exclusive after we introduce
[negation]({{ site.baseurl }}{% link out/plfa/Negation.md%}).)

#### Exercise `+-mono-<` {#plus-mono-less}

Show that addition is monotonic with respect to strict inequality.
As with inequality, some additional definitions may be required.

#### Exercise `≤-iff-<` (recommended) {#leq-iff-less}

Show that `suc m ≤ n` implies `m < n`, and conversely.

#### Exercise `<-trans-revisited` {#less-trans-revisited}

Give an alternative proof that strict inequality is transitive,
using the relating between strict inequality and inequality and
the fact that inequality is transitive.


## Even and odd

As a further example, let's specify even and odd numbers.  Inequality
and strict inequality are _binary relations_, while even and odd are
_unary relations_, sometimes called _predicates_:
<pre class="Agda">{% raw %}<a id="20106" class="Keyword">data</a> <a id="even"></a><a id="20111" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20111" class="Datatype">even</a> <a id="20116" class="Symbol">:</a> <a id="20118" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a> <a id="20120" class="Symbol">→</a> <a id="20122" class="PrimitiveType">Set</a>
<a id="20126" class="Keyword">data</a> <a id="odd"></a><a id="20131" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20131" class="Datatype">odd</a>  <a id="20136" class="Symbol">:</a> <a id="20138" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a> <a id="20140" class="Symbol">→</a> <a id="20142" class="PrimitiveType">Set</a>

<a id="20147" class="Keyword">data</a> <a id="20152" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20111" class="Datatype">even</a> <a id="20157" class="Keyword">where</a>

  <a id="even.zero"></a><a id="20166" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20166" class="InductiveConstructor">zero</a> <a id="20171" class="Symbol">:</a>
      <a id="20179" class="Comment">---------</a>
      <a id="20195" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20111" class="Datatype">even</a> <a id="20200" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#115" class="InductiveConstructor">zero</a>

  <a id="even.suc"></a><a id="20208" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20208" class="InductiveConstructor">suc</a>  <a id="20213" class="Symbol">:</a> <a id="20215" class="Symbol">∀</a> <a id="20217" class="Symbol">{</a><a id="20218" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20218" class="Bound">n</a> <a id="20220" class="Symbol">:</a> <a id="20222" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="20223" class="Symbol">}</a>
    <a id="20229" class="Symbol">→</a> <a id="20231" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20131" class="Datatype">odd</a> <a id="20235" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20218" class="Bound">n</a>
      <a id="20243" class="Comment">------------</a>
    <a id="20260" class="Symbol">→</a> <a id="20262" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20111" class="Datatype">even</a> <a id="20267" class="Symbol">(</a><a id="20268" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="20272" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20218" class="Bound">n</a><a id="20273" class="Symbol">)</a>

<a id="20276" class="Keyword">data</a> <a id="20281" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20131" class="Datatype">odd</a> <a id="20285" class="Keyword">where</a>

  <a id="odd.suc"></a><a id="20294" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20294" class="InductiveConstructor">suc</a>   <a id="20300" class="Symbol">:</a> <a id="20302" class="Symbol">∀</a> <a id="20304" class="Symbol">{</a><a id="20305" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20305" class="Bound">n</a> <a id="20307" class="Symbol">:</a> <a id="20309" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="20310" class="Symbol">}</a>
    <a id="20316" class="Symbol">→</a> <a id="20318" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20111" class="Datatype">even</a> <a id="20323" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20305" class="Bound">n</a>
      <a id="20331" class="Comment">-----------</a>
    <a id="20347" class="Symbol">→</a> <a id="20349" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20131" class="Datatype">odd</a> <a id="20353" class="Symbol">(</a><a id="20354" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="20358" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20305" class="Bound">n</a><a id="20359" class="Symbol">)</a>{% endraw %}</pre>
A number is even if it is zero or the successor of an odd number,
and odd if it is the successor of an even number.

This is our first use of a mutually recursive datatype declaration.
Since each identifier must be defined before it is used, we first
declare the indexed types `even` and `odd` (omitting the `where`
keyword and the declarations of the constructors) and then
declare the constructors (omitting the signatures `ℕ → Set`
which were given earlier).

This is also our first use of _overloaded_ constructors,
that is, using the same name for constructors of different types.
Here `suc` means one of three constructors:

    suc : ℕ → ℕ

    suc : ∀ {n : ℕ}
      → odd n
        ------------
      → even (suc n)

    suc : ∀ {n : ℕ}
      → even n
        -----------
      → odd (suc n)

Similarly, `zero` refers to one of two constructors. Due to how it
does type inference, Agda does not allow overloading of defined names,
but does allow overloading of constructors.  It is recommended that
one restrict overloading to related meanings, as we have done here,
but it is not required.

We show that the sum of two even numbers is even:
<pre class="Agda">{% raw %}<a id="e+e≡e"></a><a id="21535" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21535" class="Function">e+e≡e</a> <a id="21541" class="Symbol">:</a> <a id="21543" class="Symbol">∀</a> <a id="21545" class="Symbol">{</a><a id="21546" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21546" class="Bound">m</a> <a id="21548" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21548" class="Bound">n</a> <a id="21550" class="Symbol">:</a> <a id="21552" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="21553" class="Symbol">}</a>
  <a id="21557" class="Symbol">→</a> <a id="21559" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20111" class="Datatype">even</a> <a id="21564" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21546" class="Bound">m</a>
  <a id="21568" class="Symbol">→</a> <a id="21570" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20111" class="Datatype">even</a> <a id="21575" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21548" class="Bound">n</a>
    <a id="21581" class="Comment">------------</a>
  <a id="21596" class="Symbol">→</a> <a id="21598" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20111" class="Datatype">even</a> <a id="21603" class="Symbol">(</a><a id="21604" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21546" class="Bound">m</a> <a id="21606" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="21608" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21548" class="Bound">n</a><a id="21609" class="Symbol">)</a>

<a id="o+e≡o"></a><a id="21612" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21612" class="Function">o+e≡o</a> <a id="21618" class="Symbol">:</a> <a id="21620" class="Symbol">∀</a> <a id="21622" class="Symbol">{</a><a id="21623" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21623" class="Bound">m</a> <a id="21625" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21625" class="Bound">n</a> <a id="21627" class="Symbol">:</a> <a id="21629" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="21630" class="Symbol">}</a>
  <a id="21634" class="Symbol">→</a> <a id="21636" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20131" class="Datatype">odd</a> <a id="21640" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21623" class="Bound">m</a>
  <a id="21644" class="Symbol">→</a> <a id="21646" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20111" class="Datatype">even</a> <a id="21651" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21625" class="Bound">n</a>
    <a id="21657" class="Comment">-----------</a>
  <a id="21671" class="Symbol">→</a> <a id="21673" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20131" class="Datatype">odd</a> <a id="21677" class="Symbol">(</a><a id="21678" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21623" class="Bound">m</a> <a id="21680" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#230" class="Primitive Operator">+</a> <a id="21682" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21625" class="Bound">n</a><a id="21683" class="Symbol">)</a>

<a id="21686" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21535" class="Function">e+e≡e</a> <a id="21692" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20166" class="InductiveConstructor">zero</a>     <a id="21701" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21701" class="Bound">en</a>  <a id="21705" class="Symbol">=</a>  <a id="21708" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21701" class="Bound">en</a>
<a id="21711" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21535" class="Function">e+e≡e</a> <a id="21717" class="Symbol">(</a><a id="21718" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20208" class="InductiveConstructor">suc</a> <a id="21722" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21722" class="Bound">om</a><a id="21724" class="Symbol">)</a> <a id="21726" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21726" class="Bound">en</a>  <a id="21730" class="Symbol">=</a>  <a id="21733" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20208" class="InductiveConstructor">suc</a> <a id="21737" class="Symbol">(</a><a id="21738" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21612" class="Function">o+e≡o</a> <a id="21744" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21722" class="Bound">om</a> <a id="21747" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21726" class="Bound">en</a><a id="21749" class="Symbol">)</a>

<a id="21752" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21612" class="Function">o+e≡o</a> <a id="21758" class="Symbol">(</a><a id="21759" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20294" class="InductiveConstructor">suc</a> <a id="21763" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21763" class="Bound">em</a><a id="21765" class="Symbol">)</a> <a id="21767" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21767" class="Bound">en</a>  <a id="21771" class="Symbol">=</a>  <a id="21774" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#20294" class="InductiveConstructor">suc</a> <a id="21778" class="Symbol">(</a><a id="21779" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21535" class="Function">e+e≡e</a> <a id="21785" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21763" class="Bound">em</a> <a id="21788" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Relations.md %}{% raw %}#21767" class="Bound">en</a><a id="21790" class="Symbol">)</a>{% endraw %}</pre>
Corresponding to the mutually recursive types, we use two mutually recursive
functions, one to show that the sum of two even numbers is even, and the other
to show that the sum of an odd and an even number is odd.

This is our first use of mutually recursive functions.  Since each identifier
must be defined before it is used, we first give the signatures for both
functions and then the equations that define them.

To show that the sum of two even numbers is even, consider the
evidence that the first number is even. If it is because it is zero,
then the sum is even because the second number is even.  If it is
because it is the successor of an odd number, then the result is even
because it is the successor of the sum of an odd and an even number,
which is odd.

To show that the sum of an odd and even number is odd, consider the
evidence that the first number is odd. If it is because it is the
successor of an even number, then the result is odd because it is the
successor of the sum of two even numbers, which is even.

#### Exercise `o+o≡e` (stretch) {#odd-plus-odd}

Show that the sum of two odd numbers is even.

#### Exercise `Bin-predicates` (stretch) {#Bin-predicates}

Recall that 
Exercise [Bin]({{ site.baseurl }}{% link out/plfa/Naturals.md%}#Bin)
defines a datatype `Bin` of bitstrings representing natural numbers.
Representations are not unique due to leading zeros.
Hence, eleven may be represented by both of the following:

    x1 x1 x0 x1 nil
    x1 x1 x0 x1 x0 x0 nil

Define a predicate

    Can : Bin → Set

over all bitstrings that holds if the bitstring is canonical, meaning
it has no leading zeros; the first representation of eleven above is
canonical, and the second is not.  To define it, you will need an
auxiliary predicate

    One : Bin → Set

that holds only if the bistring has a leading one.  A bitstring is
canonical if it has a leading one (representing a positive number) or
if it consists of a single zero (representing zero).

Show that increment preserves canonical bitstrings:

    Can x
    ------------
    Can (inc x)

Show that converting a natural to a bitstring always yields a
canonical bitstring:

    ----------
    Can (to n)

Show that converting a canonical bitstring to a natural
and back is the identity:

    Can x
    ---------------
    to (from x) ≡ x

(Hint: For each of these, you may first need to prove related
properties of `One`.)

## Standard prelude

Definitions similar to those in this chapter can be found in the standard library:
<pre class="Agda">{% raw %}<a id="24294" class="Keyword">import</a> <a id="24301" href="https://agda.github.io/agda-stdlib/Data.Nat.html" class="Module">Data.Nat</a> <a id="24310" class="Keyword">using</a> <a id="24316" class="Symbol">(</a><a id="24317" href="https://agda.github.io/agda-stdlib/Data.Nat.Base.html#742" class="Datatype Operator">_≤_</a><a id="24320" class="Symbol">;</a> <a id="24322" href="https://agda.github.io/agda-stdlib/Data.Nat.Base.html#765" class="InductiveConstructor">z≤n</a><a id="24325" class="Symbol">;</a> <a id="24327" href="https://agda.github.io/agda-stdlib/Data.Nat.Base.html#807" class="InductiveConstructor">s≤s</a><a id="24330" class="Symbol">)</a>
<a id="24332" class="Keyword">import</a> <a id="24339" href="https://agda.github.io/agda-stdlib/Data.Nat.Properties.html" class="Module">Data.Nat.Properties</a> <a id="24359" class="Keyword">using</a> <a id="24365" class="Symbol">(</a><a id="24366" href="https://agda.github.io/agda-stdlib/Data.Nat.Properties.html#1804" class="Function">≤-refl</a><a id="24372" class="Symbol">;</a> <a id="24374" href="https://agda.github.io/agda-stdlib/Data.Nat.Properties.html#1997" class="Function">≤-trans</a><a id="24381" class="Symbol">;</a> <a id="24383" href="https://agda.github.io/agda-stdlib/Data.Nat.Properties.html#1854" class="Function">≤-antisym</a><a id="24392" class="Symbol">;</a> <a id="24394" href="https://agda.github.io/agda-stdlib/Data.Nat.Properties.html#2109" class="Function">≤-total</a><a id="24401" class="Symbol">;</a>
                                  <a id="24437" href="https://agda.github.io/agda-stdlib/Data.Nat.Properties.html#10952" class="Function">+-monoʳ-≤</a><a id="24446" class="Symbol">;</a> <a id="24448" href="https://agda.github.io/agda-stdlib/Data.Nat.Properties.html#10862" class="Function">+-monoˡ-≤</a><a id="24457" class="Symbol">;</a> <a id="24459" href="https://agda.github.io/agda-stdlib/Data.Nat.Properties.html#10706" class="Function">+-mono-≤</a><a id="24467" class="Symbol">)</a>{% endraw %}</pre>
In the standard library, `≤-total` is formalised in terms of
disjunction (which we define in
Chapter [Connectives]({{ site.baseurl }}{% link out/plfa/Connectives.md%})),
and `+-monoʳ-≤`, `+-monoˡ-≤`, `+-mono-≤` are proved differently than here,
and more arguments are implicit.


## Unicode

This chapter uses the following unicode:

    ≤  U+2264  LESS-THAN OR EQUAL TO (\<=, \le)
    ≥  U+2265  GREATER-THAN OR EQUAL TO (\>=, \ge)
    ˡ  U+02E1  MODIFIER LETTER SMALL L (\^l)
    ʳ  U+02B3  MODIFIER LETTER SMALL R (\^r)

The commands `\^l` and `\^r` give access to a variety of superscript
leftward and rightward arrows in addition to superscript letters `l` and `r`.
