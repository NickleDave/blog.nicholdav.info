---
title:  "Reading Gibson's Ecological Approach to Visual Perception"
post_url: 2022-12-19-reading-gibsons-ecological-approach
---

## and why it gave me permission to reject functionalist modular cognitivism

> It is not true that “the laboratory can never be like life.” The laboratory must be like life!

<cite> James Gibson
{: .small}

##TODO: write this intro as a disclaimer that this is an *exegesis* and include links to later posts

I want to write a series of posts on
James Gibson's book "Ecological Approach to Visual Perception".
I just started reading it, after hearing
[Michael Anderson mention it on the Brain Inspired podcast](https://braininspired.co/podcast/152/).
It felt revelatory.
I have started reading the book over so my mind can grapple with the big ideas (like you do),
so as long as I'm here I want to capture why it feels so revelatory.

Part of the reason I was so interested to read this book
is because it sounded like it would help me make sense
of my own unanswered questions about how we study vision and behavior.
I did a post-doc funded by a DARPA program
whose stated goal was to
develop models of continual learning
that adapted mechanisms from biology.
If you, like me, are a jaded neuroscientist,
you might ask "What mechanisms?
We haven't figured out any mechanisms.
We're borrowing mechanism from artifical intelligence right now."
My part of the project was focused on goal-driven perception,
and before I even came into the picture,
it had already been decided that one of the supposed mechanisms
we'd adapt to achieve goal-driven perception would be attention,
specifically visual attention,
which as we all know is a long-solved problem in neuroscience,
which is why in machine learning it's All You Need (this is sarcasm).
So I was handed this project, and I began by trying to give myself a crash course
in visual neuroscience and visual attention.
From the very beginning it all felt very wrong to me.
The tasks that people in the lab used to study vision and attention
seemed completely divorced from reality.
In the real world, I wander around seeing natural images,
but in the lab, you force me to hold my head still
and to fixate my eyes while you flash a 2-D array of lines and shapes at me?
Then on top of that, the supposed mechanisms
that these tasks were meant to study
seemed to be making gigantic assumptions
about how the brain even operates.
In a lot of these papers, "attention" is always shown
coming in magically, as an arrow from the upper right,
to modulate activity in some lower-level brain area,
as we commented on in meetings at the start of the project.
("Module" is a weasel word if ever I heard one.)
This top-down modulation by a putative attention area
in the brain sounds good until you try to think about
what controls the attention area,
and you end up with an infinite regress of
homunculi.
These kinds of issues always happen when we study
the brain in a way that assumes
it operates on sensory inputs in a bottom-up manner,
constructing a representation from low-level features
that it can then hand off to some higher-level
brain area. You know, to do some cognitive things.
We can better define cognitive things later;
I have to spend centuries figuring out how
we build representations from low-level features first.
This happens even when you are studying
perception in a "simple" animal.
In a fly you get a "Flycunclus", "the little fly sitting in the fly's brain
trying to fly the fly" (from
[the introduction to Spikes](https://www.princeton.edu/~wbialek/our_papers/spikes_intro.pdf).)

![Flynculus](/assets/images/spikes-figure-1.7-flynculus.png)

What I'm trying to tell you is that none
of the received wisdom about attention made any sense to me.
I didn't have a very good reason to feel this way.
My Ph.D. was in neuroscience,
but I focused on motor learning,
specifically studying songbirds
and the neuroanatomy of
the special system of brain areas
they have evolved to learn their song.
My entry into neuroscience
was really from a comparative neuroanatomy and cognition lab.
I don't come from what I now know is the functionalist,
cognitivist tradition,
which in its extreme form asserts that the brain
is composed of a set of discrete areas,
each computing a specific function,
with these functions are composed to produce cognition,
operating on inputs to produce outputs.
Now in retrospect, I feel like I don't come from any tradition.
I was able to get ground through the biomedical science sausage mill
without ever being inculcated with a philosophy of science.
(I had a brief encounter with Theory of Mind in
a class taught by Daniel Weiskopf in undergrad,
but I took that class of my own volition
and I hadn't yet spent a decade
marinating in neuroscience.)
Not that I blame anyone.
Based on my experience in academia,
the principal route for gaining exposure to philosophy of science
as a scientist is to be the child
of a philosopher of science in academia.
Maybe we should change that some.
Scientists can have a little philosophy, as a treat.

So anyway, I worked really, really hard at doing a modeling study,
even though it was totally outside of anything I knew about,
and I was grappling alone with the ancient history of a gigantic field
that had only begun to dawn on me.
The experiments that I came up with,
the simulations that I did,
were still painted into a corner
by the assumptions of that field.
Please send any medals you wish to award me to the address that can be found on this site.
Eventually I did manage to squeeze a paper out about it,
but still I figured the main reason
I felt like the entire field of visual science
was trying to cram a square peg into a round hole
was probably just on account of my contrarian nature.

Unlike me, James Gibson had *very* good reasons
to feel like he doubted the conventional wisdom
in vision science.
As he explains in the introduction the
Ecological Approach to Visual Perception,
he helped build the foundation of vision science
as it is still practiced today,
writing an entire book
centered around the idea of
vision based on the *retinal image*.
This bottom-up study of vision,
will be familiar to anyone with
even secondhand knowlefge of
David Marr's *Vision*.
(I confess I still have never managed to read it,
and only know of it from other authors reacting to it.
See for example the introduction to Active Vision,
which similarly reacts to the Marr-like conception
of vision as painting in each moment
a still picture in the head,
constructed feature by feature.)
In the Ecological Approach to Visual Perception,
he instead argues that

> ... natural vision depends on the eyes in the head on a body supported by the ground,
> the brain being only the central organ of a complete visual system.
> When no constraints are put on this visual system, we look around,
> walk up to something interesting and move around it so as to see it from all sides,
> and go from one vista to another. That's what this book is about.

Gibson calls these *ambient vision* and *ambulatory vision*,
in contrast with what we might think of as the Marr approach,
what he terms *snapshot vision* and *aperature vision*,
the concept of vision as a series of pictures taken through a pinhole camera.

To redefine vision this way,
as a continuous process of extracting invariant information
from an environment in flux,
Gibson has to start from zero,
because the standard science of vision has little room for such ideas.
He has to first define an environment,
and then define information,
that infamous Gibsonian information that is
not to be confused with Shannon information,
and then finally he can dare to say how we might perceive.

As the preface to the new edition I'm reading makes clear,
Gibson really did reject the edifice he'd helped build.
We're talking about someone
who knew intimately the metrics of psychometrics,
who fought to isolate the perceptual quanta of psychophysics.
And in the end he said, "no, perception can not be this.
It won't work.
We must stop trying to
force reductionist

So, basically, reading what he wrote,
I now feel justified
in being skeptical of the whole endeavor
to localize attention to any brain areas,
and to identify those areas
by telling someone to hold their head very still
while we record neural activity and flash some Gabor filters at them.
It's helping me think about vision as a motor skill,
as a behavior,
which really makes sense to me.
Even just looking at the brain,
we can see the visual system as a motor loop.
What looks like attention from the point-of-view of V1
might just be efference copies from the frontal eye fields
as they figure out where to saccade next.
Insert refs to premotor atttention here,
the mirror neuron dudes + the microsaccade stuff.

I am hoping reading the book will help me find the way to think about other behaviors too,
to figure out how to find the path when you start over from scratch.
James Gibson has given me permission to feel this way.
If what I say below in these posts resonates with you,
then you can read more about Gibson
here on the monoskop wiki:
<https://monoskop.org/James_J._Gibson>.
They share the book I'll discuss,
"The ecological approach to vision", here:
<http://monoskop.org/log/?p=13821>.
