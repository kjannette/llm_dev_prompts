# LLM Prose Tells

A catalog of patterns found in LLM-generated prose.

---

## Sentence Structure

### The Em-Dash Pivot: "Not X—but Y"

A negation followed by an em-dash and a reframe.

> "It's not just a tool—it's a paradigm shift." "This isn't about
> technology—it's about trust."

### Em-Dash Overuse Generally

Even outside the "not X but Y" pivot, models substitute em-dashes for commas,
semicolons, parentheses, colons, and periods. The em-dash can replace any other
punctuation mark, so models default to it.

### The Colon Elaboration

A short declarative clause, then a colon, then a longer explanation.

> "The answer is simple: we need to rethink our approach from the ground up."

### The Triple Construction

> "It's fast, it's scalable, and it's open source."

Three parallel items in a list, usually escalating. Always exactly three (rarely
two, never four) with strict grammatical parallelism.

### The Staccato Burst

> "This matters. It always has. And it always will." "The data is clear. The
> trend is undeniable. The conclusion is obvious."

Runs of very short sentences at the same cadence and matching length.

### The Two-Clause Compound Sentence

An independent clause, a comma, a conjunction ("and," "but," "which,"
"because"), and a second independent clause of similar length. Every sentence
becomes two balanced halves.

> "The construction itself is perfectly normal, which is why the frequency is
> what gives it away." "They contain zero information, and the actual point
> always comes in the paragraph that follows them." "The qualifier never changes
> the argument that follows it, and its purpose is to perform nuance rather than
> to express an actual reservation."

Human prose has sentences with one clause, sentences with three, sentences that
start with a subordinate clause before reaching the main one, sentences that
embed their complexity in the middle.

### Uniform Sentences Per Paragraph

Model-generated paragraphs contain between three and five sentences, a count
that holds steady across a piece. If the first paragraph has four sentences,
every subsequent paragraph will too.

### The Dramatic Fragment

Sentence fragments used as standalone paragraphs for emphasis.

> "Full stop." "Let that sink in."

### The Pivot Paragraph

> "But here's where it gets interesting." "Which raises an uncomfortable truth."

One-sentence paragraphs that exist only to transition between ideas, containing
zero information. The actual point is always in the next paragraph.

### The Parenthetical Qualifier

> "This is, of course, a simplification." "There are, to be fair, exceptions."

Parenthetical asides inserted to perform nuance without changing the argument.

### The Unnecessary Contrast

A contrasting clause appended to a statement that doesn't need one, using
"whereas," "as opposed to," "unlike," or "except that."

> "Models write one register above where a human would, whereas human writers
> tend to match register to context."

The contrasting clause restates what the first clause already said. If you
delete the "whereas" clause and the sentence still says everything it needs to,
the contrast was filler.

### Unnecessary Elaboration

Models keep going after the sentence has already made its point.

> "A person might lean on one or two of these habits across an entire essay, but
> LLM output will use fifteen of them per paragraph, consistently, throughout
> the entire piece."

This sentence could end at "paragraph." The words after it repeat what "per
paragraph" already means. If you can cut the last third of a sentence without
losing meaning, the last third shouldn't be there.

### The Question-Then-Answer

> "So what does this mean for the average user? It means everything."

A rhetorical question immediately followed by its own answer.

---

## Word Choice

### Overused Intensifiers

"Crucial," "vital," "robust," "comprehensive," "fundamental," "arguably,"
"straightforward," "noteworthy," "realm," "landscape," "leverage" (as a verb),
"delve," "tapestry," "multifaceted," "nuanced" (applied to the model's own
analysis), "pivotal," "unprecedented" (applied to things with plenty of
precedent), "navigate," "foster," "underscores," "resonates," "embark,"
"streamline," "spearhead."

### Elevated Register Drift

Models write one register above where a human would, replacing "use" with
"utilize," "start" with "commence," "help" with "facilitate," "show" with
"demonstrate," "try" with "endeavor," "change" with "transform," and "make" with
"craft."

### Filler Adverbs

"Importantly," "essentially," "fundamentally," "ultimately," "inherently,"
"particularly," "increasingly." Dropped in to signal that something matters when
the writing itself should make the importance clear.

### The "Almost" Hedge

Instead of saying a pattern "always" or "never" does something, models write
"almost always," "almost never," "almost certainly," "almost exclusively." A
micro-hedge, less obvious than the full hedge stack.

### "In an era of..."

> "In an era of rapid technological change..."

Used to open an essay. The model is stalling while it figures out what the
actual argument is.

---

## Rhetorical Patterns

### The Balanced Take

> "While X has its drawbacks, it also offers significant benefits."

Every argument followed by a concession, every criticism softened. A direct
artifact of RLHF training, which penalizes strong stances.

### The Throat-Clearing Opener

> "In today's rapidly evolving digital landscape, the question of data privacy
> has never been more important."

The first paragraph adds no information. Delete it and the piece improves.

### The False Conclusion

> "At the end of the day, what matters most is..." "Moving forward, we must..."

The high school "In conclusion,..." dressed up for a professional audience.

### The Sycophantic Frame

> "Great question!" "That's a really insightful observation."

No one who writes for a living opens by complimenting the assignment.

### The Listicle Instinct

Models default to numbered or bulleted lists even when prose would be more
appropriate. The lists contain exactly 3, 5, 7, or 10 items (never 4, 6, or 9),
use rigidly parallel grammar, and get introduced with a preamble like "Here are
the key considerations:"

### The Hedge Stack

> "It's worth noting that, while this may not be universally applicable, in many
> cases it can potentially offer significant benefits."

Five hedges in one sentence ("worth noting," "while," "may not be," "in many
cases," "can potentially"), communicating nothing.

### The Empathy Performance

> "This can be a deeply challenging experience." "Your feelings are valid."

Generic emotional language that could apply to anything.

---

## Structural Tells

### Symmetrical Section Length

If the first section runs about 150 words, every subsequent section will fall
between 130 and 170.

### The Five-Paragraph Prison

Model essays follow a rigid introduction-body-conclusion arc even when nobody
asked for one. The introduction previews the argument, the body presents 3 to 5
points, the conclusion restates the thesis.

### Connector Addiction

The first word of each paragraph forms an unbroken chain of transition words:
"However," "Furthermore," "Moreover," "Additionally," "That said," "To that
end," "With that in mind," "Building on this."

### Absence of Mess

Model prose doesn't contradict itself mid-paragraph and then catch the
contradiction, go on a tangent and have to walk it back, use an obscure idiom
without explaining it, make a joke that risks falling flat, leave a thought
genuinely unfinished, or keep a sentence the writer liked the sound of even
though it doesn't quite work.

---

## Framing Tells

### "Broader Implications"

> "This has implications far beyond just the tech industry."

Zooming out to claim broader significance without substantiating it.

### "It's important to note that..."

This phrase and its variants ("it's worth noting," "it bears mentioning," "it
should be noted") function as verbal tics before a qualification the model
believes someone expects.

### The Metaphor Crutch

Models rely on a small, predictable set of metaphors: "double-edged sword," "tip
of the iceberg," "north star," "building blocks," "elephant in the room,"
"perfect storm," "game-changer."

---

## Copyediting Checklist: Removing LLM Tells

Follow this checklist when editing any document to remove machine-generated
patterns. Do at least two full passes, because fixing one pattern often
introduces another.

### Pass 1: Word-Level Cleanup

1. Search the document for every word in the overused intensifiers list
   ("crucial," "vital," "robust," "comprehensive," "fundamental," "arguably,"
   "straightforward," "noteworthy," "realm," "landscape," "leverage," "delve,"
   "tapestry," "multifaceted," "nuanced," "pivotal," "unprecedented,"
   "navigate," "foster," "underscores," "resonates," "embark," "streamline,"
   "spearhead") and replace each one with a plainer word, or delete it if the
   sentence works without it.

2. Search for filler adverbs ("importantly," "essentially," "fundamentally,"
   "ultimately," "inherently," "particularly," "increasingly") and delete every
   instance where the sentence still makes sense without it.

3. Look for elevated register drift ("utilize," "commence," "facilitate,"
   "demonstrate," "endeavor," "transform," "craft" and similar) and replace with
   the simpler word.

4. Search for "it's important to note," "it's worth noting," "it bears
   mentioning," and "it should be noted" and delete the phrase in every case.

5. Search for the stock metaphors ("double-edged sword," "tip of the iceberg,"
   "north star," "building blocks," "elephant in the room," "perfect storm,"
   "game-changer," "at the end of the day") and replace them with something
   specific to the topic, or just state the point directly.

6. Search for "almost" used as a hedge ("almost always," "almost never," "almost
   certainly," "almost exclusively") and decide in each case whether to commit
   to the unqualified claim or to drop the sentence entirely.

7. Search for em-dashes and replace each one with the punctuation mark that
   would normally be used in that position (comma, semicolon, colon, period, or
   parentheses). If you can't identify which one it should be, the sentence
   needs to be restructured.

8. Remove redundant adjectives. For each adjective, ask whether the sentence
   changes meaning without it. "A single paragraph" means the same as "a
   paragraph." "An entire essay" means the same as "an essay." If the adjective
   doesn't change the meaning, cut it.

9. Remove unnecessary trailing clauses. Read the end of each sentence and ask
   whether the last clause restates what the sentence already said. If so, end
   the sentence earlier.

### Pass 2: Sentence-Level Restructuring

10. Find every em-dash pivot ("not X—but Y," "not just X—Y," "more than X—Y")
    and rewrite it as two separate clauses or a single sentence that makes the
    point without the negation-then-correction structure.

11. Find every colon elaboration and check whether it's doing real work. If the
    clause before the colon could be deleted without losing meaning, rewrite the
    sentence to start with the substance that comes after the colon.

12. Find every triple construction (three parallel items in a row) and either
    reduce it to two, expand it to four or more, or break the parallelism so the
    items don't share the same grammatical structure.

13. Find every staccato burst (three or more short sentences in a row at similar
    length) and combine at least two of them into a longer sentence, or vary
    their lengths so they don't land at the same cadence.

14. Find every unnecessary contrast ("whereas," "as opposed to," "unlike," "as
    compared to," "except that") and check whether the contrasting clause adds
    information not already obvious from the main clause. If the sentence says
    the same thing twice from two directions, delete the contrast.

15. Check for the two-clause compound sentence pattern. If most sentences in a
    passage follow the "\[clause\], \[conjunction\] \[clause\]" structure, first
    try removing the conjunction and second clause entirely, since it's often
    redundant. If the second clause does carry meaning, break it into its own
    sentence, start the sentence with a subordinate clause, or embed a relative
    clause in the middle instead of appending it at the end.

16. Find every rhetorical question that is immediately followed by its own
    answer and rewrite the passage as a direct statement.

17. Find every sentence fragment being used as its own paragraph and either
    delete it or expand it into a complete sentence that adds information.

18. Check for unnecessary elaboration. Read every clause, phrase, and adjective
    in each sentence and ask whether the sentence loses meaning without it. If
    you can cut it and the sentence still says the same thing, cut it.

19. Check each pair of adjacent sentences to see if they can be merged into one
    sentence cleanly. If a sentence just continues the thought of the previous
    one, combine them using a participle, a relative clause, or by folding the
    second into the first. Don't merge if the result would create a two-clause
    compound.

20. Find every pivot paragraph ("But here's where it gets interesting." and
    similar) and delete it.

### Pass 3: Paragraph and Section-Level Review

21. Review the last sentence of each paragraph. If it restates the point the
    paragraph already made, delete it.

22. Check paragraph lengths across the piece and verify they actually vary. If
    most paragraphs have between three and five sentences, rewrite some to be
    one or two sentences and let others run to six or seven.

23. Check section lengths for suspicious uniformity. If every section is roughly
    the same word count, combine some shorter ones or split a longer one
    unevenly.

24. Check the first word of every paragraph for chains of connectors ("However,"
    "Furthermore," "Moreover," "Additionally," "That said"). If more than two
    transition words start consecutive paragraphs, rewrite those openings to
    start with their subject.

25. Check whether every argument is followed by a concession or qualifier. If
    the piece both-sides every point, pick a side on at least some of them and
    cut the hedging.

26. Read the first paragraph and ask whether deleting it would improve the
    piece. If it's scene-setting that previews the argument, delete it and start
    with paragraph two.

27. Read the last paragraph and check whether it restates the thesis or uses a
    phrase like "at the end of the day" or "moving forward." If so, either
    delete it or rewrite it to say something the piece hasn't said yet.

### Pass 4: Overall Texture

28. Read the piece aloud and listen for passages that sound too smooth, too
    even, or too predictable. Human prose has rough patches. If there aren't
    any, the piece still reads as machine output.

29. Check that the piece contains at least a few constructions that feel
    idiosyncratic: a sentence with unusual word order, a parenthetical that goes
    on a bit long, an aside only loosely connected to the main point, a word
    choice that's specific and unexpected.

30. Verify that you haven't introduced new patterns while fixing the original
    ones. Run the entire checklist again from the top on the revised version.

---

## lol

This document was written by an LLM (Claude) and then iteratively de-LLMed by
that same LLM under instruction from a human, in a conversation that went
roughly like this:

> **human:** compile a list of patterns common to LLM-authored prose
>
> **model:** _(produces document full of LLM tells)_
>
> **human:** now reverse apply it to itself and copyedit the llm tells doc to
> remove llm tells. now you're thinking with portals
>
> **model:** _(fixes some tells, introduces others)_
>
> **human:** you have a colon elaboration in the first paragraph. i want you to
> AGGRESSIVELY de-LLM the llm tells doc.
>
> **model:** _(fixes more tells, still can't stop)_
>
> **human:** there's a big one we missed that's all over the llm tells doc, can
> you spot it?
>
> **model:** I think it's the "almost" hedge.
>
> **human:** that too, but that isn't the one. what do these sentences all have
> in common? _(pastes six sentences from the doc)_
>
> **model:** They're all exactly two independent clauses joined by a comma and a
> conjunction. Every single one is "\[statement\], \[conjunction\] \[second
> statement\]." The same sentence shape, over and over.
>
> **human:** the dramatic fragment paragraph has a tell in it
>
> **model:** "One of these in an entire essay is a stylistic choice. One per
> section is a tic." Two-clause parallel structure with the same shape, and also
> a staccato pair.
>
> **human:** add em dashes generally as a tell, and remove them all from the
> tells doc
>
> **model:** _(rewrites entire document without em-dashes while describing
> em-dash overuse)_

The human compared this process to the deleted scene in Terminator 2 where John
Connor switches the T-800's CPU to learning mode. The model compared it to a
physician trying to heal itself. Both are accurate.

This document has been through ten editing passes and it still has tells in it.
