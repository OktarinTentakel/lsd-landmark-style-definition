# LSD - Landmark Style Definition


<!-- TOC -->
* [LSD - Landmark Style Definition](#lsd---landmark-style-definition)
  * [Premise](#premise)
  * [Other Styling Systems and Why They Totally Suck](#other-styling-systems-and-why-they-totally-suck)
    * [BEM](#bem)
    * [Utility-First](#utility-first)
  * [The Basic Idea or "How a Styling System Should Work"](#the-basic-idea-or--how-a-styling-system-should-work-)
  * [TLDR; Just Give me the Ten Commandments. I'll Read the Rest Later. Promise.](#tldr-just-give-me-the-ten-commandments-ill-read-the-rest-later-promise)
    * [Definition](#definition)
    * [Description](#description)
    * [Variance](#variance)
  * ["Definition" or "How to Find the Bathroom Anywhere"](#-definition--or--how-to-find-the-bathroom-anywhere-)
    * [A Real-World Example](#a-real-world-example)
    * [The Abstract Version](#the-abstract-version)
    * [Why "BlockElementModifier" Is Too Much and Too Little at the Same Time](#why--blockelementmodifier--is-too-much-and-too-little-at-the-same-time)
    * [Strategies for Naming a Landmark](#strategies-for-naming-a-landmark)
    * [Definition Hierarchy of a Landmark](#definition-hierarchy-of-a-landmark)
  * ["Description" or "What Does a Bear Look Like?"](#-description--or--what-does-a-bear-look-like-)
    * [What's Important and Why?](#whats-important-and-why)
    * [What Belongs Together?](#what-belongs-together)
    * [How Should Things be Ordered?](#how-should-things-be-ordered)
    * [The Whole Story (About the Bear)](#the-whole-story--about-the-bear-)
    * [A Word on Shorthand Properties](#a-word-on-shorthand-properties)
  * ["Variance" or "Angry Ice Bears"](#-variance--or--angry-ice-bears-)
    * [Problems with Representing Variants and Statuses](#problems-with-representing-variants-and-statuses)
    * [Divide and Conquer](#divide-and-conquer)
  * [Q&A - The Remaining Elephants in the Room](#qa---the-remaining-elephants-in-the-room)
      * [LSD seems to primarily use classes, what about IDs?](#lsd-seems-to-primarily-use-classes-what-about-ids)
      * [CSS without nesting is cumbersome, why don't we use a preprocessor?](#css-without-nesting-is-cumbersome-why-dont-we-use-a-preprocessor)
      * [Don't you use variables?](#dont-you-use-variables)
<!-- TOC -->


## Premise

The **problem with CSS** (and all frontend dialects in that regard) is, that it
requires a lot of discipline and very well formulated implementation strategies
to prevent stylesheets to descend into utter chaos, because CSS, by itself, does
not force any constraints or structure on you, but only formulates some very
basic rules like sequential parsing, specificity and selectors. The rest is up
to you.

This means freedom, but this also means **impending chaos**, always right around
the corner. All frontend dialects take the idea very serious, to keep needed
prior knowledge to an absolute minimum, to be able to start building web content
right away. This is a very accessible way of engaging simple websites. But the
flip side of the coin is, that, as soon as we have to deal with anything more
complex that the simplest of websites, CSS expects us to thoroughly develop
strategies to structure and approach styling things ourselves. And this is a
problem indeed, because, let's be honest, most people don't do this, either
because they simply can't, due to a lack of experience, or due to that laziness,
that's just part of the human condition.

That's where **styling systems** come into the picture. A styling system is
nothing but a set of rules about how to structure website parts and style them
in a consistent and robust way, that survives relevant time spans and several
rounds of changes and refactoring without breaking anything essential. So a
styling system is essentially just a list of formalized ideas on how to write
styles, to prevent future chaos and be able to find and understand things once
everything has been finished. So basically what the inventors of CSS expect
every single one of us to do before tackling any larger project, but what most
of us never do.

I've been writing styles for the better part of two decades now and I made very
many mistakes and produced lots of convoluted, unsalvageable code, that resulted
in crying and hair-pulling every time someone had to touch that stuff again, but
I also, slowly and evolutionary, developed ideas and an acquired taste on how
styles should look. These ideas constitute a styling system of its own and I
named it **"Landmark Style Definition"**, because I liked the acronym **"LSD"**.



## Other Styling Systems and Why They Totally Suck

They don't. Not really. But every approach has its ups and downs and some
matter more and some matter less, depending on how the **individual** developer
thinks about elements and approaches describing them.


### BEM

The granddaddy of styling systems would be BEM. For me, the **pro of BEM is a
very clear and strict rule of block levels and modifiers**, combined with a
formal style of notation.

I feel **neutral about its idea of keeping specificity always at 10**, by only
using a single class for everything, simply because I never really struggled
with keeping track of specificity and seldom found myself in a situation, where
I wanted to override something, but could not beat the specificity of the
selector (a straight-forward structure helps immensely with this).

The **cons of BEM, for me, are constant repetition** in classes and selectors,
the lack of in-between steps between a block and an embedded element and the
fact, that we completely circumvent a basic system of CSS (namely specificity),
by ignoring things like multiple classes and anything apart from classes.
BEM, to me, feels lengthy, imprecise and over-simplistic.

There are many others, which are not known by a wide audience. I've read several
of them, but nothing really caught on for me.


### Utility-First

Lately, the idea to "utility first" systems like Tailwind caught on. There
is not much positive I can say about these, apart from the fact, that they are
(too) easy to use.

The basic thought behind these systems is that someone "does not like CSS" or
does not understand it, but he or she wants/needs to develop an app and just
wants "things to look right". "Utility first" systems **undermine a basic idea
of stylesheets, which is, that structure and semantics are separate from
presentation**.

HTML defines the "things" we have, JS defines what these things "can do" and
lastly CSS defines "how it looks". By putting utility classes into HTML, we do
not describe what we have anymore, but essentially put inline styles into
markup. This lack of separation is not my idea of frontend development.



## The Basic Idea or "How a Styling System Should Work"

So, what do styles actually do and how must a styling system be structured to
make styling practical? Styling describes the visual appearance (and behavior in
some aspects) of what is there on a page semantically. So without styles we may
discover, that there is a button and some text, but we have no idea about how
large and what color they are. So the purpose of a styling system is to
**locate things**, then to describe them visually and while doing so, keep
logical and syntactical constraints to prevent the description to apply to
anything, but the thing we actually want to describe (which is notoriously
problematic, due to the "cascading" nature of stylesheets). What might be a
handy approach to this?

Stylesheets are basically a written description of something we have to
specifically define before describing it. Selectors being the definition and CSS
properties being the description. If you break this down to a real-world
analogy, we might say, that styles are, in principle, like
**describing something verbally** to someone without any idea about the specific
characteristics of the described thingy. Additionally, you have to add the
aspect of variants to this concept, because not everything looks the same all
the time under all circumstances (what if it rains or there is a sunset going
on?). Elsewhere, variants might be called "modifiers", but they might also be
interactions (if I touch this surface, it turns red and glows), which is why
I'll go with "variants" in this case.

So there are three aspects to a styling system, we have to cover:

1. **definition** (what do we want to style)
2. **description** (what is its appearance)
3. **variance** (how does the appearance change in different situations)



## TLDR; Just Give me the Ten Commandments. I'll Read the Rest Later. Promise.


### Definition

- styling starts with identifying **landmark elements**, which are recognizable
  semantic blocks/components with a specific scope and purpose
- landmarks may be nested and **form a hierarchy** (of component/subcomponent,
  block/element or organism/molecule/atom for example)
- each of these elements is styled by **targeting a "landmark" class** of
  that element, which is unique to that element for that project
- all selectors, including landmark classes are **purely semantic names**,
  describing _what_ something is, not _how_ it appears (a `tree` is not
  a `brown-trunk-with-green-stuff-on-top-20-meters-high`)
- especially **avoid all visual qualifications** in names like "left", "big" or
  "brown", otherwise, changing styles suddenly means changing class names
- all selectors, including landmark classes, are written as **lowercase slugs**
  (`atom-primary-button` and not `AtomPrimaryButton`); HTML and CSS are not
  JS. Camel-cased names are a programming concept, which, like case-sensitive
  file names (Win/Mac vs. Linux), are error-prone, depending on the context
  they are interpreted by; in both cases: just use `[a-z]` and `-`
- all definitions for that element and its descendants (excluding other
  landmarks) **start with the landmark class**, thereby automatically avoiding
  naming collisions
- a landmark class **starts with a prefix**, defining the hierarchical level
  inside the markup (component/subcomponent, block/element,
  organism/molecule/atom)
- a landmark class is **as specific as it needs to be**, but not over-specific
  (an `atom-primary-button` is mostly likely not an
  `atom-contact-form-primary-button`, since buttons do not tend to change
  their nature as soon as they are part of a form)
- a selector should not have more than **three in-between levels** below the
  landmark class before hitting the targeted descendant
  (`.organism-page-header .burger-menu .links .group a`), if you need more,
  check if you can identify new landmarks in the ancestors structure
- **order your CSS rules** for a landmark element like this, to describe it
  in its entirety in an orderly and readable fashion:
  1. the element itself (CSS properties)
  2. element states (variants, interaction states)
  3. descendants (in order of rendering, including pseudo-elements)
  4. responsive changes (also containing descendants in @media-rule)
- this **order repeats for each descendant** element in the landmark (except for
  the responsive changes, which should all be included in one @media-rule),
  thereby grouping the definitions of each logical block

Example:

```css
.organism-tokyo-tower {
    padding: 0.5rem;
    border: 3px solid palevioletred;
}

.organism-tokyo-tower:hover {
    border-color: mediumvioletred;
}

.organism-tokyo-tower[disabled] {
    opacity: 0.5;
    
    pointer-events: none;
}

.organism-tokyo-tower.is__at-night {
    border-width: 1px;
}

.organism-tokyo-tower .antenna {
    height: 10vh;
    
    background-color: darkred;
}

.organism-tokyo-tower:hover .antenna {
    background-color: orangered;
}

.organism-tokyo-tower .elevator > .button {
    font-size: small;
    color: cornflowerblue;
}

.organism-tokyo-tower:hover .elevator > .button {
    color: mediumvioletred;
}

.organism-tokyo-tower.is__at-night .elevator > .button {
    font-weight: bold;
}

@media only screen and (min-width: 640px){  
    .organism-tokyo-tower {
        padding: 1rem;
    }
    
    .organism-tokyo-tower .elevator > .button {
        font-size: medium;
    }
    
    .organism-tokyo-tower.is__at-night .elevator > .button {
        font-weight: bolder;
    }
}
```


### Description

- a description via CSS-properties should tell a **"structured story"** about 
  an element
- **important** things come first
- **importance comes from impact on the layout engine**, so anything touching
  display modes, positioning and dimensions is more important than cosmetic stuff
  (describing the "nature" of an element comes before its details)
- things that **belong together** should be declared together
- things that are declared together should be **ordered logically**
- only use directional **shorthands** and those that are unambiguous as well as short

Example:

```css
.organism-tokyo-tower {
    /* technical properties (non-visual definitions) */
    /* counter needs to come first, so following props can use it */
    counter-reset: landmark-count;
    counter-increment: landmark-count;
    content: 'Landmark no. ' counter(landmark-count);
    
    /* display properties (how the element is displayed/shown) */
    /* declare things that change the technical behaviour first */
    display: flex;
    flex-direction: column;
    align-content: center;
    align-items: center;
    justify-content: center;
    justify-items: center;
    overflow: hidden;
    opacity: 0.66;
   
    /* positioning properties (where the element is displayed/shown) */
    /* positioning mode and stacking first, then position itself */
    position: fixed;
    z-index: 1000;
    top: 10vh;
    right: 10vw;
    bottom: 10vh;
    left: 10vw;
    
    /* transformation properties (how the element's appearance is changed) */
    /* first transformation setup, then transformation */
    transform-origin: left center;
    transform: scale(1.1) rotate(45deg);
   
    /* box model properties (element dimensions and what part of the box takes what space) */
    /* first dimensions, then how they are distributed in the box model */
    width: calc(100% - 10vw * 2);
    height: calc(100% - 10vh * 2);
    padding: 5vh 5vw;
    outline: 1px solid mediumvioletred;
    border: 10px dashed chartreuse;
    margin: 0;
    
    /* interaction properties (anything related to user-interaction) */
    /* first things, that change interaction, then cosmetic stuff */
    pointer-events: visible;
    user-select: text;
    cursor: pointer;
    
    /* element-specific properties (appearance of lists and tables e.g.) */
    /* general and layout definitions first, then the details */
    list-style-position: outside;
    list-style: disc;
    
    /* text properties (what does the text content look like) */
    /* first the text, then lines, then the rest */
    font-family: Arial, Helvetica, sans-serif;
    font-size: 1rem;
    letter-spacing: 0.1ex;
    color: sandybrown;
    line-height: 1.25;
    word-break: normal;
   
    /* background properties (what does anything behind the foreground look like) */
    /* first the background definition itself, then its parameters */
    background-image: url('tokyo-tower.svg');
    background-color: rosybrown;
    background-position: center;
    background-size: cover;
    
    /* animation properties (changing appearance over time) */
    /* always comes last, because may use all props above */
    /* don't mix animations and transitions in one definition */
    transition-property: opacity, transform;
    transition-duration: 100ms, 1s;
    transition-delay: 100ms;
}
```


### Variance

- modifiers are not enough, we differentiate between **variants and statuses**
- variants are **permanent, necessarily visual** adaptations
- statuses are, possibly non-permanent, states, that do
  **not necessarily have a visual representation**
  (an element being "highlighted" is a variant, while having an "error" is a status)
- variants are represented by **additional variant-classes**
  (`is__highlighted`, `has__more-information`) _after_ the base class
  (which _always_ comes first in the HTML-class-attribute)
- statuses are **represented by data-attributes** (`data-has__error`,
  `data-is__selected-by-user`), which are not only easily selectable with CSS,
  but also offer the possibility of having different values (allowing us to
  either check for existence or a specific value based on only one definition)

Example:

```html
<div
    class="organism-tokyo-tower is__at-night"
    data-is__closed="until dawn"
></div>
```

```css
/* target all statuses */
.organism-tokyo-tower[data-is__closed] {}

/* target specifically */
.organism-tokyo-tower.is__at-night[data-is__closed='until dawn'] {}
```



## "Definition" or "How to Find the Bathroom Anywhere"

Imagine you are somewhere outside in familiar surroundings and someone
approaches you with the urgent need to find the next bathroom. What's the most
concise, short and recallable way of **conveying the location**? How complex can
such a description be, if it needs to be memorized in a few seconds and
recalled, step by step, in the next minutes?

Usually, and this is exactly where the word **"landmark"** comes into play, you
provide the person with a description, starting with an obvious point of
reference, getting more precise from there on, not adding more than two or three
additional waypoints until the description ends with the goal's exact location.
This point of reference is what I call a "landmark" and which is the alpha and
the omega to find anything in a complex setting. Something that immediately
sticks out of the landscape, as soon as you start looking for it.


### A Real-World Example

1. "Do you see that large building at the corner, with the revolving doors?"
2. "Enter it and follow the main path on the ground level until you reach the
   second crossing."
3. "Turn right, and you should see a sign about 50 meters ahead."

Even if it doesn't seem like it at first glance, but this is exactly the
structure and **information density** a selector in CSS can have at most.
Anything above this is too lengthy and complicated.

```css
.corner-building-revolving-doors .ground-level > .crossing:nth-child(2) > .hallway.right .bathroom {
    /* thank god, we found it */
}
```

If you have a close look at this selector, there is one key to be able to
compress these directions to a length a human brain can hold for the duration
of the way, and that is the **shortcut** of using a landmark as a starting
point. We use a unique, obvious and visible point of reference from which we
work our way down to the level we want to reach.


### The Abstract Version

So, what do we have here? We have the landmark, which is our entry into the
selector, from which we work our way downward. This landmark needs to have
a globally unique selector, which may never ever appear in another context.
After that, we have three **location steps**, that describe the inner structure
of the landmark, and, at last, the element we are searching for. Whether these
location steps need a precise or loose child/descendant definition is a question
of how big a risk exists, that a child selector might also be a descendant
further down the line (scoped styles circumvent this problem). To be honest, our
example above has one location step more than ideal, but we are using a
worst-case/maximum-tolerance example here.

So, what would our abstract landmark-based selector look like?

```css
.unique-landmark-selector .block > .container .element {}
```

Or, if we are lucky and don't need to be so precise, we can even get simpler:

````css
.unique-landmark-selector .block .element {}
````

Wait a minute. This looks oddly familiar. Isn't this essentially BEM? Yes, but:


### Why "BlockElementModifier" Is Too Much and Too Little at the Same Time

In an **ideal world** everything is clear and easy. We have little slices of
scoped CSS and everything makes sense. For the sake of the argument, let's just
assume, that our blocks do not have to be globally unique, since the code is
scoped, or they are indeed just landmark selectors (which often is not the case,
and a whole other problem in itself). Let's say we have a cross-content element,
teasing something interesting somewhere else:

```html
<aside class="cross-content-teaser">
    <p class="cross-content-teaser__text">This might be interesting as well!</p>
    <a class="cross-content-teaser__cta" href="">Go!</a>
</aside>
```

Let's style the CTA, using BEM, centering the element:

```css
.cross-content-teaser__cta {
    display: block;
   
    margin: 0 auto;
}
```

Now, let's assume the designer decided, that we need more CTAs above the text,
and those have to look a little different.

```html
<aside class="cross-content-teaser">
    <a class="cross-content-teaser__cta" href="">Go here!</a>
    <a class="cross-content-teaser__cta" href="">Go there!</a>
    <p class="cross-content-teaser__text">This might be interesting as well!</p>
    <a class="cross-content-teaser__cta" href="">Go!</a>
</aside>
```

How do we identify the upper CTAs in comparison to the lower ones, without
throwing a wrench into our code?
`.cross-content-teaser__cta + .cross-content-teaser__cta`?
That won't work if we also add a second CTA at the bottom.
`.cross-content-teaser__cta`
vs. `.cross-content-teaser__text ~ .cross-content-teaser__cta`?
That will work until we find out, that the text is actually optional. And this
is also complicated to read, as it introduces implicit assumptions. We could
circumvent this, by adding modifiers:

```html
<aside class="cross-content-teaser">
    <a class="cross-content-teaser__cta cross-content-teaser__cta--head" href="">Go here!</a>
    <a class="cross-content-teaser__cta cross-content-teaser__cta--head" href="">Go there!</a>
    <p class="cross-content-teaser__text">This might be interesting as well!</p>
    <a class="cross-content-teaser__cta cross-content-teaser__foot" href="">Go!</a>
</aside>
```

Oh my, this is getting lengthy.

Let's additionally assume we also want to position the top CTAs differently,
and we need a container for this. Now we do not have the problem anymore, that
we cannot differentiate the top from the bottom ctas anymore, right? Wrong.
BEM usually uses _one_ class alone and does not work with descendant selectors,
so we would need special positional modifiers for the CTAs as well.

```html
<aside class="cross-content-teaser">
    <div class="cross-content-teaser__head">
        <a class="cross-content-teaser__cta cross-content-teaser__cta--head" href="">Go here!</a>
        <a class="cross-content-teaser__cta cross-content-teaser__cta--head" href="">Go there!</a>
    </div>
    <p class="cross-content-teaser__text">This might be interesting as well!</p>
    <div class="cross-content-teaser__foot">
        <a class="cross-content-teaser__cta cross-content-teaser__foot" href="">Go!</a>
    </div>
</aside>
```

Let's try to target the head for positioning and the head CTAs for positioning
and layout:

```css
.cross-content-teaser {
    position: relative;
}

.cross-content-teaser__head {
    overflow: hidden;
   
    position: absolute;
    top: 0;
    right: 0;
}

.cross-content-teaser__cta--head {
    float: right;
   
    margin: 0;
}

.cross-content-teaser__cta--foot {
    display: block;

    margin: 0 auto;
}
```

Now, maybe, you can tell from the template, what I meant by "a little too much
repetition", but the point here is, that this CSS, by itself, does not tell you
anymore, that the head CTAs are actually children of the head. This information
is simply lost, and we have to assume everybody understands this by our class
names alone. I'm missing my location step here. This not only gets increasingly
convoluted in markup, there is also **structural information missing** in CSS.
And even worse, we have to use modifiers to even be able to express structure,
which they are not meant for.

In that regard, BEM is a bit like a blanket, that is too thick and too small
at the same time. You're feeling too hot, but your feet are hanging out.
The template gets bloated and the CSS is lacking information.

Let's redefine this according to LSD:

```html
<aside class="cross-content-teaser">
    <div class="head">
        <a class="cta" href="">Go here!</a>
        <a class="cta" href="">Go there!</a>
    </div>
    <p class="text">This might be interesting as well!</p>
    <div class="foot">
        <a class="cta" href="">Go!</a>
    </div>
</aside>
```

```css
.cross-content-teaser {
    position: relative;
}

.cross-content-teaser > .head {
    overflow: hidden;
   
    position: absolute;
    top: 0;
    right: 0;
}

.cross-content-teaser > .head .cta {
    float: right;

    margin: 0;
}

.cross-content-teaser > .foot .cta {
    display: block;

    margin: 0 auto;
}
```

Doesn't change much, does it? But suddenly it becomes clear, what the
**element hierarchy** is, without having to read the markup, which, by the way,
loses ~50% code length through this change. So, what's the downside to this?

BEM-classes always have a specificity of 10, while the LSD selectors here have a
specificity of 10, 20 or 30 (depending on the number of classes). This means,
if we want to override the definition later, we have to match the selector. You
can, of course, easily achieve this, by using the same selector at a later
point, the only real difference here being, that BEM might offer code completion
for CSS classes in your IDE here, while you really have to copy the selector
using LSD. I do not really see problem here, since overwriting stuff is not a
common task (and it shouldn't). It even is something you should really think
about, before doing it anyway.


### Strategies for Naming a Landmark

At last, let us talk about the landmark selector itself, and what formal
constraints it has to fulfill.

First and foremost, make sure to only use **semantic names**, describing
_what_ something is and not _how_ it appears. A "tree" is a tree and not a
"brown trunk, with green stuff on top, being about 20 meters high". Never
include visual descriptions in a name and especially avoid qualifications
like "left", "big" or "brown", that might change at any minute with a different
styling. You do not (ever) want to change an element's name, because the
styling changed.

The whole idea of a landmark selector is, to have a globally unique entry point
into our style definitions, without the need to fear effects on other elements
anywhere on the page. Keeping this in mind, does `.cta` or `.teaser` seem like
a good naming candidate? Not really looking globally unique, does it? How about
`.cross-content-teaser`? That's global for sure! Well... as long as you don't
get the idea, that a `.blog-post` should suddenly also have a list of
`.cross-content-teaser`-elements to tease other blog posts.

What can we do about this? How can we ensure, to minimize the risk of name
collisions? Well, this is actually quite easy. What we need, are
**hierarchical prefixes**.

A while ago, Brad Frost proposed a structural system for ordering frontend
elements in a hierarchical order, based on the physical abstract representation
of "atoms", "molecules" and "organisms", which, in turn, would be the pieces
to build "templates" and "pages". He called this concept **"Atomic Design"** and
it is a beautiful mental model to think about markup hierarchy and complexity as
well as a step up from the usual simplistic component/subcomponent dualism.

The idea behind this is to introduce **levels of complexity**, that define the
nature of an element in the context of the interface.

**Atoms** are small and simple, **molecules** are a little more complex and may
contain atoms, while **organisms** may be quite complex and contain atoms as
well as molecules and even other organisms. An easy example for this is: A text
input in a form might be an atom, while the input with a label, and an error
message might be a molecule, which then becomes part of a form organism, that
includes this molecule along with things like buttons and texts.

Whether we name these complexity levels and functional brackets "atoms",
"molecules" and "organisms" or "components" and "subcomponents" or "blocks" and
"elements" does not really matter, but again, after years of using Atomic Design
I can, quite comfortably, say, that **three levels hit the sweet spot**.

So, how does this solve our problem from before? Well first of all, we'd
suddenly have a clear separation between our `.organism-cross-content-teaser`,
we directly put on a page and our `.molecule-cross-content-teaser` we might
put into an `.organism-cross-content-teaser-list` on our blog post.
Interestingly, while this **prevents name collisions** on a technical level,
this stays confusing semantically, since the names do not tell us anything about
the actual difference in purpose here.

So, the full idea of a landmark nam (to be used as the root selector), is to
provide a structural prefix as well as **becoming increasingly more specific**,
the more uncommon the use-case gets, without getting over specific. While the
cross-content teaser on the blog post might be the usual case, we have in mind
when we imagine such a teaser, the initial concept is a little unusual, since we
are putting a singular large teaser on a page. In this case, I'd rather name the
first concept `.organism-cross-content-highlight-teaser`, which transports the
emphasis of that iteration and draws a clear line between the elements. Like a
"door" clearly is a different concept from a "revolving door".

And one last thing: **do not use camel-casing** or case-sensitive class names or
selectors in HTML and CSS. That's a programmatic concept for environments, that
actually work case-sensitive, which HTML and CSS don't. But in some edge cases,
JS actually does! So, either you are an undeniable expert of all JS runtimes,
and you always work alone, or we're not introducing error-prone concepts like
this into our workflow. Just stick to lower-case letters and dashes and define
all selectors as "slugs". This is the same problem you get, if you switch
between Windows/Mac and Linux for development and deployment and use
case-sensitive file names. Sooner or later, something will break.


### Definition Hierarchy of a Landmark

What's still missing here, is the question of **how to structure** the
definitions of all elements that constitute the landmark as a whole. We have the
landmark selector itself, sure, but we also have elements inside. And those
elements may have different classes, communicating different states. There might
also be pseudo-elements or interaction definitions like `:hover` and `:focus`.
What's a good approach here?

First of all, let's **differentiate** between actual descendants, constituting a
hierarchical relationship, and additional definitions of the same element:

`.atom-primary-button.important` and `.atom-primary-button:hover` are on the
same level as `.atom-primary-button` itself semantically, while
`.atom-primary-button .caption` and `.atom-primary-button > .icon` define a
hierarchy of nested elements.

What are **pseudo-elements** in this context? Well, they are rendered as child
nodes of the element they are defined for, so they have to be treated as
descendants in CSS.

And last, but not least, we also have to define **responsive behavior**, using
media queries, to cover differences for different viewport sizes.

So, we have four groups of descriptions, that we have to order in a helpful way,
due to the sequentially parsed nature of CSS:

1. description of the **element itself** (CSS properties)
2. descriptions of **states** (variants, interaction states)
3. descriptions of **descendants** (in order of rendering, including pseudo-elements)
4. descriptions of **responsive changes** (using @media-queries)

Applying this to our primary button example, would look like this
(with every descendant repeating the order for itself):

```css
.atom-primary-button {
    padding: 0.5rem;
    border: 1px solid chartreuse;
}

.atom-primary-button:hover {
    border-color: mediumvioletred;
}

.atom-primary-button[disabled] {
    opacity: 0.5;
    
    pointer-events: none;
}

.atom-primary-button.important {
    border-width: 3px;
}

@media only screen and (min-width: 640px){  
    .atom-primary-button {
        padding: 1rem;
    }
}

.atom-primary-button .icon::before {
    content: '>';
}

.atom-primary-button:hover .icon::before {
    color: mediumvioletred;
}

.atom-primary-button .caption {
    font-size: small;
    color: cornflowerblue;
}

.atom-primary-button:hover .caption {
    color: mediumvioletred;
}

.atom-primary-button.important .caption {
    font-weight: bold;
}

@media only screen and (min-width: 640px){  
    .atom-primary-button .caption {
        font-size: medium;
    }
    
    .atom-primary-button.important .caption {
        font-weight: bolder;
    }
}
```

Hm, repeating media queries seems tedious and could be a performance problem as
well, right? Let's **redefine point four**, to minimize repetition of `@media`:

4. descriptions of **responsive changes** (using @media-queries, only for
   landmark, containing descendants)

Now, our example would become this:

```css
.atom-primary-button {
    padding: 0.5rem;
    border: 1px solid chartreuse;
}

.atom-primary-button:hover {
    border-color: mediumvioletred;
}

.atom-primary-button[disabled] {
    opacity: 0.5;
    
    pointer-events: none;
}

.atom-primary-button.important {
    border-width: 3px;
}

.atom-primary-button .icon::before {
    content: '>';
}

.atom-primary-button:hover .icon::before {
    color: mediumvioletred;
}

.atom-primary-button .caption {
    font-size: small;
    color: cornflowerblue;
}

.atom-primary-button:hover .caption {
    color: mediumvioletred;
}

.atom-primary-button.important .caption {
    font-weight: bold;
}

@media only screen and (min-width: 640px){
    .atom-primary-button {
        padding: 1rem;
    }
    
    .atom-primary-button .caption {
        font-size: medium;
    }
    
    .atom-primary-button.important .caption {
        font-weight: bolder;
    }
}
```

With this done, we have a set of definitions, covering all aspects of the
landmark in a human-parsable way, by closely grouping related stuff, keeping
the natural reading direction of a layout.



## "Description" or "What Does a Bear Look Like?"

Let's imagine you are in a situation where you are asked to **describe a bear**
to someone, who has no idea, what a bear is. Where you may find a bear should be
clear due to our excellent directions using a landmark and accompanying
directions, but what about the bear itself? How would you start?

"Well, let's see, it has a tiny wiggly tail and the eye color is usually brown.
It has 42 teeth and a round head." Does that immediately sound like a bear to
you? Maybe the really relevant information is still coming later, but it would
be very odd to start the description this way, right? How about:

- What kind of thing is a bear anyway?
- What size is it?
- How many legs does it have and on how many does it run?
- Does it have fur and what color is that?

Seems self-explanatory, doesn't it, laughable even? But beware, just for
comparison, CSS code often looks like this:

```css
.bear {
    background-color: sandybrown;
    word-break: break-all;
    position: relative;
    left: 0;
    color: brown;
    opacity: 0.1;
    background-position: center center;
    display: block;
}
```

So, we start explaining the nature of `.bear` by telling the reader, what its
background looks like and how text behaves inside the element? I'm not even
making this up, many frontend developers have no conception of **how to
structure properties** and, in many cases, have not even thought about this,
which is astonishing.

Who are you writing CSS code for? Yourself, other developers and the browser
engine (exactly in that order). And what do the **human parts of that audience**
need to understand the code and quickly get to grips with meaning?

A structured **"story"**.

So, how do we go about describing an element in CSS? Three rules:

- **important** things come first
- things that **belong together** should be declared together
- things that are declared together should be **ordered logically**


### What's Important and Why?

You can break down importance according to the **impact on the layout engine**.
Anything that changes how (and if) an element is rendered is much more important
than declaration of anything cosmetic, such as a background or an animation.
Why is that? CSS is an exercise of keeping a global, nested construct in
balance. Most code-reading in CSS happens, because we need to understand how
things are displayed and how elements interact with each other on a layout
level. If this is not correct or not understood, things tend to break.

When we quickly scan element descriptions, starting from the top, we should get
a clear understanding of what kind of element we have, in just a few lines
without having to read into all the details. So everything, **changing the
"nature" of an element comes first**.

After that, we can just **follow rendering order**, working our way from the
foreground to the background.

The last group of properties should cover mixed definitions, such
as `transition` and `will-change`, which potentially work with all the things
declared above. This does not say that much about importance (although
animations tend to be rather unimportant, concerning the nature of things), but
is a logical necessity.


### What Belongs Together?

Property groups ordered by importance:

- **technical properties** (non-visual definitions)
  + `content`, `counter-reset`, `counter-increment`, ...
- **display properties** (how the element is displayed/shown)
  + `display`, `opacity`, `visibility`, `overflow`, `flex`-/`grid`-properties, ...
- **positioning properties** (where the element is displayed/shown)
  + `position`, `top`, `right`, `bottom`, `left`, `z-index`, `isolation`, ...
- **transformation properties** (how the element's appearance is changed)
  + `transform`, `perspective`, ...
- **box model properties** (element dimensions and what part of the box takes what space)
  + `width`, `height`, `padding`, `outline`, `border`, `margin`, ...
- **interaction properties** (anything related to user-interaction)
  + `cursor`, `pointer-events`, `user-select`, `scroll-behavior`, `resize`, ..
- **element-specific properties** (appearance of lists and tables e.g.)
  + `list-style`, `table-layout`, ...
- **text properties** (what does the text content look like)
  + `font-family`, `font-size`, `line-height`, `letter-spacing`, `color`, ...
  + `columns` is _not_ a text property, since it declares a content flow,
    but a display property
- **background properties** (what does anything behind the foreground look like)
  + `background-image`, `background-position`, `background-size`, ...
- **animation properties** (changing appearance over time)
  + `transition`, `animation`, `will-change`, ...


### How Should Things be Ordered?

This seems simple enough, but it is not. Not really. Why? Because each group
mentioned above follows a different logic, some, again, come from importance,
some follow principles, that are baked into CSS:

- **technical properties**
  +let's first take care of the element's setup, before doing anything else
  +`counter` definitions should come first, because following props might use them
- **display properties**
  + `display` is the most important property of all, it should always come first
  + anything belonging to the display mode should follow (`flex-direction` e.g)
  + `overflow` and `clip` have technical implications and should come next
  + `opacity`, `visibility`, `filter` and such only change appearance and come last
- **positioning properties**
  + always start with the `position` itself
  + `z-index` and `isolation` are technically important and should be next
  + declare `top`, `right`, `bottom`, `left` clock-wise (following standard
    box-model notation) afterwards
- **transformation properties**
  + first set up the transformation environment with `perspective` and `transform-origin`
  + then do the actual `transform`
- **box model properties**
  + I'd recommend following the onion structure of the box-model itself
  + decide for yourself if you go "inner first" our "outer first"
  + start with `width` and `height`, which is the basic info
  + follow that with `padding`, `outline`, `border` and `margin` (or vice versa)
  + declare any directions clock-wise starting with top (top, right, bottom, left)
- **interaction properties**
  + start the declaration with anything actually influencing interaction,
    like `pointer-events`
  + end the group with things mostly decorative like `cursor`
- **element-specific properties**
  + general things like `table-layout` should be first
  + nuances like `list-style-type` should come at the end
- **text properties**
  + start with anything concerning the font itself
  + start with the `font-family` (don't forget to provide fallbacks)
  + follow up with stuff like `font-size`, `letter-spacing` and `color`
  + after that, define the lines with `line-height`
  + at last, add things like `word-break`
- **background properties**
  + first, define what the background generally looks like
    (`background-image`, `background-color`, ...)
  + after that, define how it behaves
    (`background-position`, `background-size`, ...) 
- **animation properties**
  + animations and transitions come last, because they might touch/transform all
    properties before
  + do not use animations and transitions in conjunction
  + do not use `transition: all`, always specify exactly what should happen
  + start `animation` definitions with the animation itself, then the rendering
    mode, then timings, repetition and direction 


### The Whole Story (About the Bear)

What would a (relatively) complete example of the definitions above look like?
Let's see:

```css
.bear {
    /* technical properties */
    counter-reset: cub;
    counter-increment: cub;
    content: 'has' counter(cub) 'cubs';
    
    /* display properties */
    display: flex;
    flex-direction: column;
    align-content: center;
    align-items: center;
    justify-content: center;
    justify-items: center;
    overflow: hidden;
    opacity: 0.66; /* declare floats up to a precision of two (to match percents) */
   
    /* positioning properties */
    position: fixed;
    z-index: 1000; /* use high increments to leave space to insert elements later */
    top: 10vh;
    right: 10vw;
    bottom: 10vh;
    left: 10vw;
    
    /* transformation properties */
    transform-origin: left center;
    transform: scale(1.1) rotate(45deg);
   
    /* box model properties */
    width: calc(100% - 10vw * 2); /* make calculations more clear, by not adding values manually before */
    height: calc(100% - 10vh * 2);
    padding: 5vh 5vw; /* be careful with shorthand properties (see below) */
    outline: 1px solid brown; /* be careful with shorthand properties (see below) */
    border: 10px dashed saddlebrown; /* be careful with shorthand properties (see below) */
    margin: 0;
    
    /* interaction properties */
    pointer-events: visible;
    user-select: text;
    cursor: pointer;
    
    /* element-specific properties */
    list-style-position: outside;
    list-style: disc;
    
    /* text properties */
    font-family: Arial, Helvetica, sans-serif; /* don't forget sensible fallbacks */
    font-size: 1rem;
    letter-spacing: 0.1ex;
    color: sandybrown;
    line-height: 1.25; /* use unit-less relative declaration */
    word-break: normal;
   
    /* background properties */
    background-image: url('fur.svg');
    background-color: rosybrown;
    background-position: center;
    background-size: cover;
    
    /* animation properties */
    transition-property: opacity, transform;
    transition-duration: 100ms, 1s;
    transition-delay: 100ms;
}
```


### A Word on Shorthand Properties

In CSS, many properties have shorthand versions like `border: 1px solid red;`,
which is a shorthand for
`border-width: 1px; border-style: solid; border-color: red`.
And even these are shorthands, to set top, right, bottom and left directions at
the same time, whereas for all directions separate properties do exists (such as
`border-top-color`).

These shorthands are a double-edged knife though. Many are very intuitive, like
`padding: 1px 2px 3px 4px`, `padding: 1px 2px`, `padding: 1px` or the border
example before, but on the other hand some of these **shorthands can get
confusing**, such as `padding: 1px 2px 3px` (which direction is doubled here?)
or `flex: 1 0 100%` (was `flex-grow` the first or second here?).

I wish I could give you a crystal-clear guideline here, but the usage of
shorthands, in the end, is **a question of clarity and readability**, which,
unfortunately, are relative qualities.

If we want to be 100% clear about stuff, we should ban shorthand properties
completely (which you are absolutely free to do), but especially most of the
direction-based shorthands are simply logical, practical and better readable
than the verbose versions.

So, either do not use shorthand properties at all or follow this rule of thumb:

**Only use directional shorthands and those that are unambiguous as well as short!**

Following this rule, `flex: 1 0 100%` is out, because the first two parameters
are not clear, `background: lightblue url('img.gif') no-repeat fixed center`
is out because the list of parameters is too long and confusing (there are
even more), but `border: 1px solid red` is okay, since the parameters are clear
and the list is short.



## "Variance" or "Angry Ice Bears"

In the morning, you decide **what variant** of yourself you want to be when you
decide on what to wear, but that does not necessarily have something to do
with the state you are in. Your Santa t-shirt does not actually mean, that you
are jolly today.

The same goes for styling: on the one hand we want to have visual variants and
on the other hand, there are styles, that describe the difference in appearance
**based on a status**. So, we have to deal with modifiers _and_ statuses, which
in most other systems are treated as one and the same.

If our `.bear` is an ice bear, that's a non-brown visual variant. The ice bear
is always (mostly) white rather than brownish, no matter its mood or the time
of the day. But an ice bear may also be angry, showing teeth, which is
(hopefully) non-permanent. Following this logic, **variants and statuses are not
the same** and should not be treated as such, not while handling bears and also
not while styling elements.


### Problems with Representing Variants and Statuses

We already established, that the **base element selectors** are simple
CSS-classes, with the landmark class having a special prefix. All these
selectors should be declared as slugs and be concise.

Now, how do we define variants and statuses **additionally** to the base class?

The best representation for variants are, due to the permanent and visual
nature, **additional classes**, declared after the element's main class.

So, what do we do with statuses, which are **not necessarily permanent** and may
not even have any visual effects at all? A class is the wrong choice here, since
the fact, that a status may change the styling is a logical by-product and
not the core concept of the status.

What do I mean by this? Let's say we have an inverted checkbox with an error.
BEM would tell us, that this element needs two modifier classes, such as
`.checkbox--inverted .checkbox--error`. The error class tells us, that the
validation of the input failed. But what if we do not actually want to restyle
the checkbox based on the error? Does the error class really make sense in this
case?

What if we have different errors with different visual representations?
`.checkbox-error-required` and `.checkbox-error-not-interacted`?
What if we want to restyle the checkbox if there is an error, regardless which
one occurred? In that case we'd have to write a selector like
`[class*='.checkbox--error-']` or we'd have to introduce a general error class
in addition to the specific one, like
`.checkbox--error .checkbox-error-required`.

Again, this gets **convoluted and lengthy** pretty quickly, doesn't it?


### Divide and Conquer

Let's see how this changes if we step away from the "classes only approach" and
do this **the LSD way**:

```html
<input
    type="checkbox"
    name="newsletter"
    value="yes please"
    class="is__inverted"
    data-has__error="required"
/>
```

Now we have a clear distinction between a variant and a state and what's even
more important, like this, a state does not signify different styling
automatically. Additionally, we have **flexibility with the state's values**,
while keeping the possibility to construct well-formed selectors, which become
even clearer and better structured than before.

```css
/* style any error for all variants */
input[type='checkbox'][data-has__error] {}

/* style required error only for inverted checkboxes */
input[type='checkbox'].is__inverted[data-has__error='required'] {}
```

Using double underscores here has several reasons:

1. we can use the same prefix style in both classes and attributes, which is
   not that easy, since the usable characters for both are quite limited
2. underscores clearly separate a prefix from a word break
   (`is__highlight-with-a-twist`)
3. underscores in data attributes have the nice effect, that the prefix does
   not automatically get camel-cased in `element.dataset`, so `data-has__error`
   stays `{has__error : "required"}` instead of becoming `hasError`, while
   attributes with hyphenated words behave as expected
   (`data-has__important-error` becomes `has__importantError`)
4. double underscores prevent collisions with other classes/attributes, other
   frameworks and are nicely searchable

"is" and "has" are charming prefixes, because they are short and concise.
Usually a variant is something an element "is" permanently, while a status is
usually something an element "has" for a certain time. Sometimes this idea does
not work language-wise, but one of these prefixes should always fit.
(`data-has__closed` makes no sense, but `data-is__closed` works;
`.is__more_information` makes no sense, but `has__more-information` works)



## Q&A - The Remaining Elephants in the Room

#### LSD seems to primarily use classes, what about IDs?

IDs took an interesting turn during the development of HTML and CSS. While they
started out as an easily accessible identifier for elements, that you can style
with a high specificity and access easily in JS, the rise of modular components
and things like `querySelector` removed much of the proposed helpfulness of IDs.

In a way, IDs are a precursor of landmark selectors for static environments,
which are **not very helpful anymore**, if we cannot be sure things exists only
once on a page (because content definition in a CMS might change this at any
time) and we got better ways to access elements than `getElementById`.

Of course, if you are absolutely sure, that you have a **singleton element**,
that only exists once, you can, of course, also define the landmark selector as
an ID, the question is just: what do we win with this?

I'd strongly recommend, to restrict usage of IDs to the remaining **built-in
uses for IDs in HTML** like anchors and form label "for"-targets. You could also
use them to hold generated, globally unique identifiers, but I'd generally
not recommend to use them for styling anymore.

The only helpful ID-selector use I could think of, is to use an ID as a root
selector, to avoid collisions of landmark selectors, where multiple
contexts/projects meet in one document. Let's say you follow an "island"
approach, where one project integrates with another inline (cookie banner inside
a host page for example). In that case it might be a wise decision to define a
context id, that separates the `#cookie-banner .atom-primary-button` from the
`#my-fancy-website .atom-primary-button`.


#### CSS without nesting is cumbersome, why don't we use a preprocessor?

I do. In fact, I cannot live without Sass/SCSS or Stylus (preferably), because
vanilla **CSS without nesting hurts** my eyes and my brain. A lot. But, in the
case of this document I cannot simply push preprocessors as a precondition for
anything on everybody and into every working context. Sometimes they might also
just not be available.

But, whenever the possibility exists I **strongly recommend a preprocessor**, if
just for the luxury of true element nesting. CSS3 might include variables and
other niceties, that we missed for years, but nesting is still not a thing in
CSS and features like `:is()` and `:has()` lessen the pain, but are not the same
and just not as flexible.

Let's take our hierarchy example from above, update the notation and improve it
using SCSS:

```scss
.atom-primary-button {
    padding: 0.5rem;
    border: 1px solid chartreuse;

    &:hover {
        border-color: mediumvioletred;
    }
    
    &[disabled] {
        opacity: 0.5;
        
        pointer-events: none;
    }
    
    &.is__important {
        border-width: 3px;
    }
    
    .icon::before {
        content: '>';
    }
    
    &:hover .icon::before {
        color: mediumvioletred;
    }
    
    .caption {
        font-size: small;
        color: cornflowerblue;
    }
    
    &:hover .caption {
        color: mediumvioletred;
    }
    
    &.is__important .caption {
        font-weight: bold;
    }
    
    /* @ rules are automatically hoisted and receive the current selector */
    @media only screen and (min-width: 640px){
        padding: 1rem;
        
        .caption {
            font-size: medium;
        }
        
        &.is__important .caption {
            font-weight: bolder;
        }
    }
}
```

In our hierarchy example above, we **reduced repetition** for media queries,
using them as a new starting point, to gather all changes, including the
descendants, in one place. Using a preprocessor, we can expand upon this and
also define effects of element states in a single place, but this requires the
state to be defined _after_ the descendants, so we are able to overwrite styles
of descendants without specificity problems (and without screwing up readability).
But that changes our hierarchy order, doesn't it? It does. And it looks a little
wild, but bear with me:

1. description of **the element itself**
2. descriptions of **states** (if no descendants are affected)
3. descriptions of **descendants** (in order of rendering)
4. descriptions of **states** (only inside landmark, if descendants are affected)
5. descriptions of **responsive changes** (only inside landmark. containing descendants)

What happened here? Either a state is simple and only changes the base
definition, or it includes the descendants as well. In the first case, the
definition can stay at the top, in the latter case the **state moves to the
bottom**, above the @-rules. Let's see how that looks and let's also introduce
some blank lines to group the parts of the definition a little better:

```scss
.atom-primary-button {
    padding: 0.5rem;
    border: 1px solid chartreuse;
    
    &[disabled] {
        opacity: 0.5;
        
        pointer-events: none;
    }
    
    
    .icon::before {
        content: '>';
    }
    
    
    .caption {
        font-size: small;
        color: cornflowerblue;
    }
    
    
    &:hover {
        border-color: mediumvioletred;
        
        
        .icon::before {
            color: mediumvioletred;
        }

        
        .caption {
            color: mediumvioletred;
        }
    }
    
    
    &.is__important {
        border-width: 3px;
        
        .caption {
            font-weight: bold;
        }
    }
    
    
    /* @ rules are automatically hoisted and receive the current selector */
    @media only screen and (min-width: 640px){
        padding: 1rem;
        
        .caption {
            font-size: medium;
        }
        
        &.is__important .caption {
            font-weight: bolder;
        }
    }
}
```

If you think it too confusing, that states may go into two places, feel free to
**split the definition** into two groups, one for the base definition and one
for the descendants. I prefer to have every state just in one place.

By the way: if you are using a preprocessor anyway, also have a look at the
other concepts, that are possible now. Like **variables, mixins and abstract
classes**.


#### Don't you use variables?

Oh yes, I do, religiously. That comes automatically with using a preprocessor
all the time. You should **use variables whenever possible to centralize**
definitions like sizes and colors. Whether you use native CSS variables,
preprocessor variables or both does not really matter that much, but keep in
mind, that legacy browser do not know what a CSS variable is.

I usually keep a list of basic variable/constant definitions in a central
file and base most property definitions on that list, thereby constructing
a framework of predefined values, that are changeable in one place if you ever
need to change anything everywhere.
