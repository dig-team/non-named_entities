# Non-Named Entities – The Silent Majority

This is a joint work between [Pierre-Henri Paris](https://phparis.net) and 
<a href="https://suchanek.name" target="_blank">Fabian M. Suchanek</a> (both at <a href="https://www.telecom-paris.fr/en/home" target="_blank">Télécom Paris</a>,
<a href="https://dig.telecom-paris.fr/blog/" target="_blank">DIG
team</a>).

<br>

## Non-Named Entities?
            
Human texts usually contain a lot of <a
href="https://en.wikipedia.org/wiki/Noun_phrase#:~:text=A%20noun%20phrase%2C%20or%20nominal,most%20frequently%20occurring%20phrase%20type."
target="_blank">noun phrases</a> that often function as verb subjects and objects, as predicative
expressions and as the complements of prepositions.
When building knowledge bases, proper nouns in noun phrases are extracted and used as entities equipped
with facts to populate knowledge bases.
During this process, <span class="fw-bolder">noun phrases that are not proper nouns are left
unmapped</span>, thus ignoring a vast
amount of information.
Consider the following example which contains a <span style="color:red">named entity</span> and two
<span style="color:blue">non-named entities</span>:

<blockquote><i class="e">« <span style="color:red" data-bs-toggle="tooltip" data-bs-placement="top"
data-bs-html="true" data-bs-customClass="fs-high"
title="<span style='font-size: 5.8em;'>This is a <span class='fw-bolder'>named entity</span>, e.g. on Wikidata.</span>">The
<a href="https://www.wikidata.org/wiki/Q33761" target="_blank" style="color:red">Arab
Spring</a></span> resulted in
<span style="color:blue" data-bs-toggle="tooltip" data-bs-placement="top"
title="This is a non-named entity.">contentious battle between a consolidation of power by
religious elites</span> and <span style="color:blue" data-bs-toggle="tooltip"
data-bs-placement="top" title="This is another non-named entity.">the growing support for
democracy</span>. »</i>
</blockquote>

<br>

### The Problem

The previous example contains <span class="fw-bolder">a lot of information that will not be extracted despite
its importance</span>.
Indeed, the two non-named entities explain the Arab Spring event, but will not be mapped to the
knowledge bases.

In this position paper:

1. We <span class="fw-bolder">manually analyzed this phenomenon</span> in 30 Wikipedia abstracts,
allowing us to quantify the information lost,
1. We also proposed <span class="fw-bolder">a partial solution</span> for simple non-named entities,
1. Finally, we discuss <span class="fw-bolder">the remaining challenges</span> to represent all
non-named entities.

<br>

## Manual Study of Non-Named Entities in Wikipedia

We conducted a manual analysis of the noun phrases in Wikipedia articles.
Our choice is motivated by the fact that Wikipedia is <span class="fw-bolder">a widely used standard
reference</span>, in both research and industry applications.

We focus on the Wikipedia articles with highest quality (the “<a
href="https://en.wikipedia.org/wiki/Wikipedia:Featured_articles" target="_blank">featured
articles</a>”).

<br>

<p><a name="fig1"></a>
<figure class="figure">
<img src="img/workflow.svg" class="figure-img img-fluid rounded"
alt="Annotation workflow.">
<figcaption class="figure-caption"><span class="fw-bolder">Figure 1:</span> Annotation workflow.
</figcaption>
</figure></a>
</p>

<br>

We choose <span class="fw-bolder">one article from each of the 30 topics</span>, and automatically
extract (and manually verify) noun
phrases from the abstract of the article.
We consider noun phrases that are sequences of nouns, adjectives, adverbs, determiners, and
prepositions (see <a href="#fig1">Figure 1</a>).

Overall, we annotated <span class="fw-bolder">1925 noun phrases</span>, at an inter-annotator agreement
(<a href="https://en.wikipedia.org/wiki/Cohen%27s_kappa" target="_blank">Cohen’s kappa</a>) of 0.88,
which is considered excellent.

<br>

<div class="container">
<div class="row">
<div class="col-md">
<a name="fig2">
<figure class="figure">
<img src="img/plural.svg" class="figure-img img-fluid rounded"
alt="Repartition of named and non-named entities.">
<figcaption class="figure-caption"><span class="fw-bolder">Figure 2:</span> Repartition
of
named and non-named entities.</figcaption>
</figure>
</a>
</div>
<div class="col-md">
<a name="fig3">
<figure class="figure">
<img src="img/yago.svg" class="figure-img img-fluid rounded"
alt="Non-named entities by Yago class.">
<figcaption class="figure-caption"><span class="fw-bolder">Figure 3:</span> Non-named
entities by Yago class.</figcaption>
</figure>
</a>
</div>
</div>
</div>

<br>

<a href="#fig2">Figure 2</a> shows the repartition between named and non-named entities. We found that
78% of the entities are non-named, and thus left alone.
Most of the non-named entities are singular (63%), and thus refer to a single entity (<i
class="e">« <span class="fst-italic">a bus</span> »</i>, <i class="e">« <span class="fst-italic">the
tall girl</span> »</i>).
The plural non-named entities refer to ad-hoc concepts (<i class="e">« <span class="fst-italic">German
scientists</span> »</i>) or groups of entities
(<i class="e">« <span class="fst-italic">the four horses</span> »</i>).

<a href="#fig3">Figure 3</a> shows the repartition of non-named entities between classes from <a
href="https://schema.org">schema.org</a> and <a href="https://bioschema.org">BioSchema.org</a>.
As expected, <span class="fst-italic">intangible</span> and <span class="fst-italic">creative
work</span> entities are ubiquitous.

<a name="fig4">
<figure class="figure">
<img src="img/nature.svg" class="figure-img img-fluid rounded"
alt="Non-named entities by nature and modifiers.">
<figcaption class="figure-caption"><span class="fw-bolder">Figure 4:</span> Non-named entities
by nature
and modifiers.</figcaption>
</figure>
</a>

<br>

<a href="#fig4">Figure 4</a> shows the repartition of non-named entities by nature and modifiers.
32% of non-named entities are determined (<i class="e">« <span class="fst-italic">the man</span> »</i>),
which makes it
more likely that they are central
to
the text.
Only a few entities are anaphoras or qualified by anaphoras.

<br>

## A Partial Solution
As a solution for the simplest non-named entities, we propose the following steps:

1. Replace anaphoras and determined noun phrases with their referent: <i class="e">« <span
class="fst-italic">the
coffee brand</span> »</i> becomes the <span class="fst-italic">G. Washington Coffee
Company</span> 
entity in <a href="https://en.wikipedia.org/wiki/George_Washington_(inventor)"
target="_blank">this</a> context.
1. Simple nested noun phrases are linked together: <i class="e">« a mansion in Brooklyn »</i> becomes
an anonymous
entity link to the  <span class="fst-italic">Brooklyn</span> entity.
1. Use of ad-hoc classes for plural noun phrases based on the head noun : <i class="e">« <span
class="fst-italic">German scientists</span> »</i> becomes a subclass of the <span
class="fst-italic">scientist</span> class.
1. Make singular noun phrases anonymous instances of ad-hoc classes or top classes (as in <a
href="#fig3">Figure 3</a>): <i class="e">« <span class="fst-italic">a German
scientist</span> »</i> becomes an
anonymous instance of the <span class="fst-italic">German scientist</span> class.
1. Replication of numbered noun phrases: <i class="e">« <span class="fst-italic">the four royal
houses</span> »</i>
becomes four instances of the <span class="fst-italic">royal house</span> class.
1. Mass nouns became classes: <i class="e">« <span class="fst-italic">the propagation of
light</span> »</i> becomes an
instance of the <span class="fst-italic">propagation</span> class or <i class="e">« <span
class="fst-italic">the
fame that Elvis achieved</span> »</i> becomes an
instance of the <span class="fst-italic">fame</span> class.

<br>

## The Gap that remains

However, many pitfalls remain before we can represent all non-named entities in knowledge bases.

### Knowledge representation

- Some knowledge bases contain mainly classes, others mix classes and instances, and others duplicate
them (like <a href="http://dbpedia.org/resource/Book">dbr:Book</a> and <a
href="http://dbpedia.org/ontology/Book">dbo:Book</a>).
These different models are not all usable as is and may need to be adapted before adding non-named
entities.
- Plural noun phrases like <i class="e">« <span class="fst-italic">hundreds of soldiers</span> »</i>
needs axioms to
express <i class="e">« <span class="fst-italic">hundreds</span> »</i>. OWL 2 could help for this
task.
- Vague noun phrases like <i class="e">« <span class="fst-italic">large-scale settlement</span> »</i>
cannot be represented in current knowledge bases 
unless using Generalized Quantifiers or Fuzzy Logic.
- Nested entities like <i class="e">« <span class="fst-italic">the growing support for democracy in
many Muslim-majority states</span> »</i> or <i class="e">«
<span class="fst-italic">contentious battle between a consolidation of power by religious
elites</span> »</i>.

### Canonicalization

- Similar non-named entities in different contexts, e.g. two different rises of the same stock market.
- Distinct entities must be kept apart, e.g. two rises of different stock markets.

### Facts

- Comparatives, superlatives, and temporal comparisons, <i class="e">« <span class="fst-italic">A rose
is more
beautiful than a daisy</span> »</i> need elaborate axioms.
- Complex statements about classes: How to express statements for non-named entities such that <i
class="e">« dormant volcano »</i> or <i class="e">« his characteristic surrealist style »</i> ?

<br>

<br>

#### To cite this work

<blockquote>Pierre-Henri Paris, Fabian M. Suchanek. <span class="fst-italic">Non-named entities - the silent
majority</span>. In ESWC 2021.</blockquote>

<br>

#### Additional materials

- The paper is available <a href="https://openreview.net/pdf?id=bdC-s5cjrm6" target="_blank">here</a>,
- the text annotation guideline [here](annotation_guideline.md),
- all Wikipedia articles we processed are in the [articles](articles/) directory,
- and all annotations are in the [annotations](annotations/) directory.

[![Alt text](https://img.youtube.com/vi/PSaTgx6fvwM/0.jpg)](https://www.youtube.com/watch?v=PSaTgx6fvwM)