---
layout: "post"
title: "Hashset-stuides-2"
date: "2018-08-23 13:45"
tag:
- HashSet
comments: true
---

# EnumSet and EnumMap

* This article discusses
<a href="https://docs.oracle.com/javase/8/docs/api/java/util/EnumSet.html"><code>java.util.EnumSet</code></a>
and
<a href="https://docs.oracle.com/javase/8/docs/api/java/util/EnumMap.html"><code>java.util.EnumMap</code></a>
from Java's standard libraries.


## What are they?

<a href="https://docs.oracle.com/javase/8/docs/api/java/util/EnumSet.html"><code>EnumSet</code></a>
and
<a href="https://docs.oracle.com/javase/8/docs/api/java/util/EnumMap.html"><code>EnumMap</code></a> are compact, efficient implementations of the <a href="https://docs.oracle.com/javase/8/docs/api/java/util/Set.html"><code>Set</code></a> and <a href="https://docs.oracle.com/javase/8/docs/api/java/util/Map.html"><code>Map</code></a> interfaces.  They have the constraint that their elements/keys come from a single <a href="https://docs.oracle.com/javase/tutorial/java/javaOO/enum.html"><code>enum</code></a> type.

* Like <a href="https://docs.oracle.com/javase/8/docs/api/java/util/HashSet.html"><code>HashSet</code></a> and <a href="https://docs.oracle.com/javase/8/docs/api/java/util/HashMap.html"><code>HashMap</code></a>, they are modifiable.

* In contrast to <code>HashSet</code>, <code>EnumSet</code>:


- Consumes less memory, usually.
- Is faster at all the things a <code>Set</code> can do, usually
- Iterates over elements in a predictable order (the declaration order of the element type's <code>enum</code> constants).
- Rejects <code>null</code> elements.

* In contrast to <code>HashMap</code>, <code>EnumMap</code>:

- Consumes less memory, usually.
- Is faster at all the things a <code>Map</code> can do, usually.
- Iterates over entries in a predictable order (the declaration order of the key type's <code>enum</code> constants).
- Rejects <code>null</code> keys.

* If you're wondering how this is possible, I encourage you to look at the source code:

- <a href="http://hg.openjdk.java.net/jdk10/jdk10/jdk/file/72f33dbfcf3b/src/java.base/share/classes/java/util/EnumSet.java"><code>EnumSet</code></a>
    A bit vector of the <a href="https://docs.oracle.com/javase/8/docs/api/java/lang/Enum.html#ordinal--">ordinals</a> of the elements in the <code>Set</code>.  This is an abstract superclass of <code>RegularEnumSet</code> and <code>JumboEnumSet</code>.
- <a href="http://hg.openjdk.java.net/jdk10/jdk10/jdk/file/72f33dbfcf3b/src/java.base/share/classes/java/util/RegularEnumSet.java"><code>RegularEnumSet</code></a>
    <p>An <code>EnumSet</code> whose bit vector is a single primitive <code>long</code>, which is enough to handle all <code>enum</code> types having 64 or fewer constants.
- <a href="http://hg.openjdk.java.net/jdk10/jdk10/jdk/file/72f33dbfcf3b/src/java.base/share/classes/java/util/JumboEnumSet.java"><code>JumboEnumSet</code></a>
    <p>An <code>EnumSet</code> whose bit vector is a <code>long[]</code> array, which is allocated however many slots are necessary for the given <code>enum</code> type.  Two slots are allocated for 128 or fewer constants, three slots for 192 or fewer constants, etc.
- <a href="http://hg.openjdk.java.net/jdk10/jdk10/jdk/file/72f33dbfcf3b/src/java.base/share/classes/java/util/EnumMap.java"><code>EnumMap</code></a>
    A flat array of the <code>Map</code>'s values indexed by the ordinals of their keys.

* <code>EnumSet</code> and <code>EnumMap</code> cheat!  They use <a href="http://hg.openjdk.java.net/jdk10/jdk10/jdk/file/72f33dbfcf3b/src/java.base/share/classes/java/util/EnumSet.java#l405">privileged code like this</a>:

<pre class="enums">
<span class="enums com">/**</span>
<span class="enums com"> * Returns all of the values comprising E.</span>
<span class="enums com"> * The result is uncloned, cached, and shared by all callers.</span>
<span class="enums com"> */</span>
<span class="enums kwa">private static</span> <span class="enums opt">&lt;</span>E <span class="enums kwa">extends</span> Enum<span class="enums opt">&lt;</span>E<span class="enums opt">&gt;&gt;</span> E<span class="enums opt">[]</span> <span class="enums kwd">getUniverse</span><span class="enums opt">(</span>Class<span class="enums opt">&lt;</span>E<span class="enums opt">&gt;</span> elementType<span class="enums opt">) {</span>
  <span class="enums kwa">return</span> SharedSecrets<span class="enums opt">.</span><span class="enums kwd">getJavaLangAccess</span><span class="enums opt">()</span>
                      <span class="enums opt">.</span><span class="enums kwd">getEnumConstantsShared</span><span class="enums opt">(</span>elementType<span class="enums opt">);</span>
<span class="enums opt">}</span>
</pre>

* If you want all the <a href="https://docs.oracle.com/javase/8/docs/api/java/time/Month.html"><code>Month</code></a> constants, you might call <a href="https://docs.oracle.com/javase/8/docs/api/java/time/Month.html#values--"><code>Month.values()</code></a>, giving you a <code>Month[]</code> array.  There is a single backing array instance of those <code>Month</code> constants living in memory somewhere (<a href="http://hg.openjdk.java.net/jdk10/jdk10/jdk/file/72f33dbfcf3b/src/java.base/share/classes/java/lang/Class.java#l3447">a private field in the <code>Class</code> object</a> for <code>Month</code>), but it wouldn't be safe to pass that array directly to every caller of <code>values()</code>.  Imagine if someone modified that array!  Instead, <code>values()</code> creates a fresh clone of the array for each caller.

* <code>EnumSet</code> and <code>EnumMap</code> get to skip that cloning step.  They have direct access to the backing array.

* Effectively, no third-party versions of these classes can be as efficient.  Third-party libraries that provide <code>enum</code>-specialized collections tend to delegate to <code>EnumSet</code> and <code>EnumMap</code>.  It's not that the library authors are lazy or incapable; delegating is the correct choice for them.

## When should they be used?

* Historically, <code>Enum{Set,Map}</code> were recommended as a matter of safety, taking better advantage of Java's type system than the alternatives.

* <strong>Prefer <code>enum</code> types and <code>Enum{Set,Map}</code> over <code>int</code> flags.</strong>

* <a href="https://www.amazon.com/Effective-Java-2nd-Joshua-Bloch/dp/0321356683">Effective Java</a> goes into detail about this use case for <code>Enum{Set,Map}</code> and <code>enum</code> types in general.  If you write a lot of Java code, then you should read that book and follow its advice.

* Before <code>enum</code> types existed, people would declare flags as <code>int</code> constants.  Sometimes the flags would be powers of two and combined into sets using bitwise arithmetic:

<pre class="enums">
<span class="enums kwa">static final</span> <span class="enums kwb">int</span> OVERLAY_STREETS  <span class="enums opt">=</span> <span class="enums num">1</span> <span class="enums opt">&lt;&lt;</span> <span class="enums num">0</span><span class="enums opt">;</span>
<span class="enums kwa">static final</span> <span class="enums kwb">int</span> OVERLAY_ELECTRIC <span class="enums opt">=</span> <span class="enums num">1</span> <span class="enums opt">&lt;&lt;</span> <span class="enums num">1</span><span class="enums opt">;</span>
<span class="enums kwa">static final</span> <span class="enums kwb">int</span> OVERLAY_PLUMBING <span class="enums opt">=</span> <span class="enums num">1</span> <span class="enums opt">&lt;&lt;</span> <span class="enums num">2</span><span class="enums opt">;</span>
<span class="enums kwa">static final</span> <span class="enums kwb">int</span> OVERLAY_TERRAIN  <span class="enums opt">=</span> <span class="enums num">1</span> <span class="enums opt">&lt;&lt;</span> <span class="enums num">3</span><span class="enums opt">;</span>

<span class="enums kwb">void</span> <span class="enums kwd">drawCityMap</span><span class="enums opt">(</span><span class="enums kwb">int</span> overlays<span class="enums opt">) { ... }</span>

<span class="enums kwd">drawCityMap</span><span class="enums opt">(</span>OVERLAY_STREETS <span class="enums opt">|</span> OVERLAY_PLUMBING<span class="enums opt">);</span>
</pre>

* Other times the flags would start at zero and count up by one, and they would be used as array indexes:

<pre class="enums">
<span class="enums kwa">static final</span> <span class="enums kwb">int</span> MONSTER_SLIME    <span class="enums opt">=</span> <span class="enums num">0</span><span class="enums opt">;</span>
<span class="enums kwa">static final</span> <span class="enums kwb">int</span> MONSTER_GHOST    <span class="enums opt">=</span> <span class="enums num">1</span><span class="enums opt">;</span>
<span class="enums kwa">static final</span> <span class="enums kwb">int</span> MONSTER_SKELETON <span class="enums opt">=</span> <span class="enums num">2</span><span class="enums opt">;</span>
<span class="enums kwa">static final</span> <span class="enums kwb">int</span> MONSTER_GOLEM    <span class="enums opt">=</span> <span class="enums num">3</span><span class="enums opt">;</span>

<span class="enums kwb">int</span><span class="enums opt">[]</span> kills <span class="enums opt">=</span> <span class="enums kwd">getMonstersSlain</span><span class="enums opt">();</span>

<span class="enums kwa">if</span> <span class="enums opt">(</span>kills<span class="enums opt">[</span>MONSTER_SLIME<span class="enums opt">] &gt;=</span> <span class="enums num">10</span><span class="enums opt">) { ... }</span>
</pre>

* These approaches got the job done for many people, but they were somewhat error-prone and difficult to maintain.

* When <code>enum</code> types were introduced to the language, <code>Enum{Set,Map}</code> came with them.  Together they were meant to provide better tooling for problems previously solved with <code>int</code> flags.  We would say, "Don't use <code>int</code> flags, use <code>enum</code> constants.  Don't use bitwise arithmetic for sets of flags, use <code>EnumSet</code>.  Don't use arrays for mappings of flags, use <code>EnumMap</code>."  This was not because the <code>enum</code>-based solutions were faster than <code>int</code> flags &mdash; they were probably slower &mdash; but because the <code>enum</code>-based solutions were easier to understand and implement correctly.

* Fast forward to today, I don't see many people using <code>int</code> flags anymore (though there are <a href="https://docs.oracle.com/javase/8/docs/api/java/util/Spliterator.html#characteristics--">notable exceptions</a>).  We've had <code>enum</code> types in the language for more than a decade.  We're all using <code>enum</code> types here and there, we're all using the collections framework.  At this point, while <cite>Effective Java</cite>'s advice regarding <code>Enum{Set,Map}</code> is still valid, I think most people will never have a chance to put it into practice.

* Today, we're using <code>enum</code> types in the right places, but we're forgetting about the collection types that came with them.

* <strong>Prefer <code>Enum{Set,Map}</code> over <code>Hash{Set,Map}</code> as a performance optimization.</strong>

- Prefer <code>EnumSet</code> over <code>HashSet</code> when the elements come from a single <code>enum</code> type.
- Prefer <code>EnumMap</code> over <code>HashMap</code> when the keys come from a single <code>enum</code> type.

* Should you refactor all of your existing code to use <code>Enum{Set,Map}</code> instead of <code>Hash{Set,Map}</code>?  <strong>No.</strong>

* Your code that uses <code>Hash{Set,Map}</code> isn't wrong.  Migrating to <code>Enum{Set,Map}</code> might make it faster.  That's it.

* If you've ever used primitive collection libraries like <a href="http://fastutil.di.unimi.it/">fastutil</a> or <a href="http://trove.starlight-systems.com/">Trove</a>, then it may help to think of <code>Enum{Set,Map}</code> like those primitive collections.  The difference is that <code>Enum{Set,Map}</code> are specialized for <code>enum</code> types, not primitive types, and you can use them without depending on any third-party libraries.

* <code>Enum{Set,Map}</code> don't have identical semantics to <code>Hash{Set,Map}</code>, so please don't make blind, blanket replacements in your existing code.

* Instead, try to remember these classes for next time.  If you can make your code more efficient for free, then why not go ahead and do that, right?

* If you use <a href="https://www.jetbrains.com/idea/">IntelliJ IDEA</a>, you can have it remind you to use <code>Enum{Set,Map}</code> with inspections:

- Analyze - Run inspection by name - "Set replaceable with EnumSet" or "Map replaceable with EnumMap"

* ...or...

- File - Settings - Editor - Inspections - Java - Performance issues - "Set replaceable with EnumSet" or "Map replaceable with EnumMap"

* <a href="https://www.sonarqube.org/">SonarQube</a> can also remind you to use <code>Enum{Set,Map}</code>:

- <a href="https://github.com/SonarSource/sonar-java/blob/cb1baeadb6f9beaab8a04a639a33b90944f73f19/java-checks/src/main/java/org/sonar/java/checks/EnumSetCheck.java"><code>S1641</code></a>: "Sets with elements that are enum values should be replaced with EnumSet"
- <a href="https://github.com/SonarSource/sonar-java/blob/cb1baeadb6f9beaab8a04a639a33b90944f73f19/java-checks/src/main/java/org/sonar/java/checks/EnumMapCheck.java"><code>S1640</code></a>: "Maps with keys that are enum values should be replaced with EnumMap"

* For immutable versions of <code>Enum{Set,Map}</code>, see the following methods from <a href="https://github.com/google/guava">Guava</a>:

- Factory methods:

    - <a href="http://google.github.io/guava/releases/21.0/api/docs/com/google/common/collect/Sets.html#immutableEnumSet-E-E...-"><code>Sets.immutableEnumSet(first, rest...)</code></a>
    - <a href="http://google.github.io/guava/releases/21.0/api/docs/com/google/common/collect/Sets.html#immutableEnumSet-java.lang.Iterable-"><code>Sets.immutableEnumSet(iterable)</code></a>
    - <a href="http://google.github.io/guava/releases/21.0/api/docs/com/google/common/collect/Maps.html#immutableEnumMap-java.util.Map-"><code>Maps.immutableEnumMap(map)</code></a>

- Collectors:

    - <a href="http://google.github.io/guava/releases/21.0/api/docs/com/google/common/collect/Sets.html#toImmutableEnumSet--"><code>Sets.toImmutableEnumSet()</code></a>
    - <a href="http://google.github.io/guava/releases/21.0/api/docs/com/google/common/collect/Maps.html#toImmutableEnumMap-java.util.function.Function-java.util.function.Function-"><code>Maps.toImmutableEnumMap(keyMapper, valueMapper)</code></a>
    - <a href="http://google.github.io/guava/releases/21.0/api/docs/com/google/common/collect/Maps.html#toImmutableEnumMap-java.util.function.Function-java.util.function.Function-java.util.function.BinaryOperator-"><code>Maps.toImmutableEnumMap(keyMapper, valueMapper, mergeFunction)</code></a>


* If you don't want to use Guava, then wrap the modifiable <code>Enum{Set,Map}</code> instances in <a href="https://docs.oracle.com/javase/8/docs/api/java/util/Collections.html#unmodifiableSet-java.util.Set-"><code>Collections.unmodifiableSet(set)</code></a> or <a href="https://docs.oracle.com/javase/8/docs/api/java/util/Collections.html#unmodifiableMap-java.util.Map-"><code>Collections.unmodifiableMap(map)</code></a> and throw away the direct references to the modifiable collections.

* The resulting collections may be less efficient when it comes to operations like <code>containsAll</code> and <code>equals</code> than their counterparts in Guava, which may in turn be less efficient than the raw modifiable collections themselves.

## Could the implementations be improved?

* Since they can't be replaced by third-party libraries, <code>Enum{Set,Map}</code> had better be as good as possible!  They're good already, but they could be better.

* <code>Enum{Set,Map}</code> have missed out on potential upgrades since Java 8.  New methods were added in Java 8 to <code>Set</code> and <code>Map</code> (or higher-level interfaces like <a href="https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html"><code>Collection</code></a> and <a href="https://docs.oracle.com/javase/8/docs/api/java/lang/Iterable.html"><code>Iterable</code></a>).  While the default implementations of those methods are <em>correct</em>, we could do better with overrides in <code>Enum{Set,Map}</code>.

* This issue is tracked as <a href="https://bugs.openjdk.java.net/browse/JDK-8170826">JDK-8170826</a>.

* Specifically, these methods should be overridden:

- <code>{Regular,Jumbo}EnumSet.forEach(action)</code>
- <code>{Regular,Jumbo}EnumSet.iterator().forEachRemaining(action)</code>
- <code>{Regular,Jumbo}EnumSet.spliterator()</code>
- <code>EnumMap.forEach(action)</code>
- <code>EnumMap.{keySet,values,entrySet}().forEach(action)</code>
- <code>EnumMap.{keySet,values,entrySet}().iterator().forEachRemaining(action)</code>
- <code>EnumMap.{keySet,values,entrySet}().spliterator()</code>

* I put <a href="https://github.com/TechEmpower/misc/blob/master/enums/enum_collection_overrides.txt">sample implementations</a> on GitHub in case you're curious what these overrides might look like.  They're all pretty straightforward.

* Rather than walk through each implementation in detail, I'll share some high-level observations about them.

- The optimized <code>forEach</code> and <code>forEachRemaining</code> methods are roughly 50% better than the defaults (in terms of operations per second).
- <code>EnumMap.forEach(action)</code> benefits the most, becoming twice as fast as the default implementation.
- The <a href="https://docs.oracle.com/javase/8/docs/api/java/lang/Iterable.html"><code>iterable.forEach(action)</code></a> method is popular.  Optimizing it tends to affect a large audience, which increases the likelihood that the optimization (even if small) is worthwhile.  (I'd claim that <code>iterable.forEach(action)</code> is <em>too</em> popular, and I'd suggest that the traditional enhanced <code>for</code> loop should be preferred over <code>forEach</code> except when the argument to <code>forEach</code> can be written as a method reference.  That's a topic for another discussion, though.)
- The <a href="https://docs.oracle.com/javase/8/docs/api/java/util/Iterator.html#forEachRemaining-java.util.function.Consumer-"><code>iterator.forEachRemaining(action)</code></a> method is more important than it seems.  Few people use it directly, but many people use it indirectly through streams.  The default <code>spliterator()</code> delegates to the <code>iterator()</code>, and the default <code>stream()</code> delegates to the <code>spliterator()</code>.  In the end, stream traversal may delegate to <code>iterator().forEachRemaining(...)</code>.  Given the popularity of streams, optimizing this method is a good idea!
- The <a href="https://docs.oracle.com/javase/8/docs/api/java/lang/Iterable.html#spliterator--"><code>iterable.spliterator()</code></a> method is critical when it comes to stream performance, but writing a custom <a href="https://docs.oracle.com/javase/8/docs/api/java/util/Spliterator.html"><code>Spliterator</code></a> from scratch is a non-trivial task.  I recommend this approach:

    - Check whether the characteristics of the default spliterator are correct for your collection (often times the defaults are too conservative &mdash; for example, <code>EnumSet</code>'s spliterator is currently missing the <a href="https://docs.oracle.com/javase/8/docs/api/java/util/Spliterator.html#ORDERED"><code>ORDERED</code></a>, <a href="https://docs.oracle.com/javase/8/docs/api/java/util/Spliterator.html#SORTED"><code>SORTED</code></a>, and <a href="https://docs.oracle.com/javase/8/docs/api/java/util/Spliterator.html#NONNULL"><code>NONNULL</code></a> characteristics).  If they're not correct, then provide a trivial override of the spliterator that uses <a href="https://docs.oracle.com/javase/8/docs/api/java/util/Spliterators.html#spliterator-java.util.Collection-int-"><code>Spliterators.spliterator(collection, characteristics)</code></a> to define the correct characteristics.
    - Don't go further than that until you've read through <a href="http://hg.openjdk.java.net/jdk10/jdk10/jdk/file/72f33dbfcf3b/src/java.base/share/classes/java/util/Spliterators.java#l1691">the implementation of that spliterator</a>, and you understand how it works, and you're confident that you can do better.  In particular, your <a href="https://docs.oracle.com/javase/8/docs/api/java/util/Spliterator.html#tryAdvance-java.util.function.Consumer-"><code>tryAdvance(action)</code></a> and <a href="https://docs.oracle.com/javase/8/docs/api/java/util/Spliterator.html#trySplit--"><code>trySplit()</code></a> should both be better.  Write a benchmark afterwards to confirm your assumptions.

- The <a href="https://docs.oracle.com/javase/8/docs/api/java/util/Map.html#forEach-java.util.function.BiConsumer-"><code>map.forEach(action)</code></a> method is extremely popular and is almost always worth overriding.  This is especially true for maps like <code>EnumMap</code> that create their <a href="https://docs.oracle.com/javase/8/docs/api/java/util/Map.Entry.html"><code>Entry</code></a> objects on demand.
- It's usually possible to share code across the <code>forEach</code> and <code>forEachRemaining</code> methods.  If you override one, you're already most of the way there to overriding the others.
- I don't think it's worthwhile to override <a href="https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html#removeIf-java.util.function.Predicate-"><code>collection.removeIf(filter)</code></a> in any of these classes.  For <code>RegularEnumSet</code>, where it seemed most likely to be worthwhile, I couldn't come up with a faster implementation than the default.
- <code>Enum{Set,Map}</code> <em>could</em> provide faster <code>hashCode()</code> implementations than the ones they currently inherit from <a href="https://docs.oracle.com/javase/8/docs/api/java/util/AbstractSet.html"><code>AbstractSet</code></a> and <a href="https://docs.oracle.com/javase/8/docs/api/java/util/AbstractMap.html"><code>AbstractMap</code></a>, but I don't think that would be worthwhile.  In general, I don't think optimizing the <code>hashCode()</code> of collections is worthwhile unless it can somehow become a constant-time (<code>O(1)</code>) operation, and even then it is questionable.  Collection hash codes aren't used very often.

## Could the APIs be improved?

* The implementation-level changes I've described are purely beneficial.  There is no downside other than a moderate increase in lines of code, and the new lines of code aren't all that complicated.  (Even if they were complicated, this is <code>java.util</code>!  Bring on the micro-optimizations.)

* Since the existing code is already so good, though, changes of this nature have limited impact.  Cutting one third or one half of the execution time from an operation that's already measured in nanoseconds is a good thing but not game-changing.  I suspect that those changes will cause exactly zero users of the JDK to write their applications differently.

* The more tantalizing, meaningful, and dangerous changes are the realm of the APIs.

* I think that <code>Enum{Set,Map}</code> are chronically underused.  They have a bit of a PR problem.  Some developers don't know these classes exist.  Other developers know about these classes but don't bother to reach for them when the time comes.  It's just not a priority for them.  That's totally understandable, but...  There's avoiding premature optimization and then there's throwing away performance for no reason &mdash; performance nihilism?  Maybe we can win their hearts with API-level changes.

* No one should have to go out of their way to use <code>Enum{Set,Map}</code>.  <a href="https://blog.codinghorror.com/falling-into-the-pit-of-success/">Ideally it should be easier</a> than using <code>Hash{Set,Map}</code>.  The <a href="https://docs.oracle.com/javase/8/docs/api/java/util/EnumSet.html#allOf-java.lang.Class-"><code>EnumSet.allOf(elementType)</code></a> method is a great example.  If you want a <code>Set</code> containing all the <code>enum</code> constants of some type, then <code>EnumSet.allOf(elementType)</code> is the best solution <em>and</em> the easiest solution.

* The high-level <a href="https://bugs.openjdk.java.net/browse/JDK-8145048">JDK-8145048</a> tracks a couple of ideas for improvements in this area.  In the following sections, I expand on these ideas and discuss other API-level changes.

### Add immutable <code>Enum{Set,Map}</code> (maybe?)

* In a recent conversation on Twitter about <a href="http://openjdk.java.net/jeps/301">JEP 301: Enhanced Enums</a>, Joshua Bloch and Brian Goetz referred to theoretical immutable <code>Enum{Set,Map}</code> types in the JDK.

* <a href="https://twitter.com/joshbloch/status/806389443519270912">Joshua Bloch</a>:
<blockquote>"Not sure I see the point.  <strong>ImmutableEnumSet (a library change) seems far more valuable and easier to implement.</strong>  Am I missing something?"</blockquote>

* <a href="https://twitter.com/BrianGoetz/status/806509036413788162">Brian Goetz</a>:
<blockquote>"@joshbloch a) not either/or; b) not solving same prob; c) gen enum easier than you think; d) <strong>we'd gladly take contrib of IES!</strong>"</blockquote>

* <a href="https://twitter.com/BrianGoetz/status/806615225646678016">Brian Goetz</a>:
<blockquote>"@joshbloch <strong>Still open to IE{S,M} contributions!</strong>"</blockquote>

* Joshua Bloch also discussed the possibility of an immutable <code>EnumSet</code> in <cite>Effective Java</cite>:

<blockquote>"The one real disadvantage of EnumSet is that it is not, as of release 1.6, possible to create an immutable EnumSet, but <strong>this will likely be remedied in an upcoming release</strong>.  In the meantime, you can wrap an EnumSet with Collections.unmodifiableSet, but conciseness and performance will suffer."</blockquote>

* When he said "performance will suffer", he was probably referring to the fact that certain bulk operations of <code>EnumSet</code> won't execute as quickly when inside a wrapper collection (tracked as <a href="https://bugs.openjdk.java.net/browse/JDK-5039214">JDK-5039214</a>).  Consider <a href="http://hg.openjdk.java.net/jdk10/jdk10/jdk/file/72f33dbfcf3b/src/java.base/share/classes/java/util/RegularEnumSet.java#l295"><code>RegularEnumSet.equals(object)</code></a>:

<pre class="enums">
<span class="enums kwa">public</span> <span class="enums kwb">boolean</span> <span class="enums kwd">equals</span><span class="enums opt">(</span>Object o<span class="enums opt">) {</span>
  <span class="enums kwa">if</span> <span class="enums opt">(!(</span>o <span class="enums kwa">instanceof</span> RegularEnumSet<span class="enums opt">))</span>
    <span class="enums kwa">return super</span><span class="enums opt">.</span><span class="enums kwd">equals</span><span class="enums opt">(</span>o<span class="enums opt">);</span>

  RegularEnumSet<span class="enums opt">&lt;</span>?<span class="enums opt">&gt;</span> es <span class="enums opt">= (</span>RegularEnumSet<span class="enums opt">&lt;</span>?<span class="enums opt">&gt;)</span>o<span class="enums opt">;</span>
  <span class="enums kwa">if</span> <span class="enums opt">(</span>es<span class="enums opt">.</span>elementType <span class="enums opt">!=</span> elementType<span class="enums opt">)</span>
    <span class="enums kwa">return</span> elements <span class="enums opt">==</span> <span class="enums num">0</span> <span class="enums opt">&amp;&amp;</span> es<span class="enums opt">.</span>elements <span class="enums opt">==</span> <span class="enums num">0</span><span class="enums opt">;</span>

  <span class="enums kwa">return</span> es<span class="enums opt">.</span>elements <span class="enums opt">==</span> elements<span class="enums opt">;</span>
<span class="enums opt">}</span>
</pre>

* It's optimized for the case that the argument is another instance of <code>RegularEnumSet</code>.  In that case the equality check boils down to a comparison of two primitive <code>long</code> values.  Now that's fast!

* If the argument to <code>equals(object)</code> was not a <code>RegularEnumSet</code> but instead a <code>Collections.unmodifiableSet</code> wrapper, that code would fall back to its slow path.

* Guava's approach is similar to the <code>Collections.unmodifiableSet</code> one, although <a href="https://github.com/google/guava/commit/f1f674b0eff8f55d99e2ad6e0752c684294fc95f">Guava does a bit better</a> in terms of unwrapping the underlying <code>Enum{Set,Map}</code> and delegating to the super-fast optimized paths.

* If your application deals exclusively with Guava's immutable <code>Enum{Set,Map}</code> wrappers, you should get the full benefit of those optimized paths from the JDK.  If you mix and match Guava's collections with the JDK's though, the results won't be quite as good.  (<code>RegularEnumSet</code> doesn't know how to unwrap Guava's <code>ImmutableEnumSet</code>, so a comparison in that direction would invoke the slow path.)

* If immutable <code>Enum{Set,Map}</code> had full support in the JDK, however, it would not have those same limitations.  <code>RegularEnumSet</code> and friends can be changed.

* <strong>What should be done in the JDK?</strong>

* I spent a long time and tested a lot of code trying to come up with an answer to this.  Sadly the end result is:

* <strong>I don't know.</strong>

* Personally, I'm content to use Guava for this.  I'll share some observations I made along the way.

* <strong>Immutable <code>Enum{Set,Map}</code> won't be faster than mutable <code>Enum{Set,Map}</code>.</strong>

* The current versions of <code>Enum{Set,Map}</code> are really, really good.  They'll be even better once they override the defaults from Java 8.

* Sometimes, having to support mutability comes with a tax on efficiency.  I don't think this is the case with <code>Enum{Set,Map}</code>.  At <em>best</em>, immutable versions of these classes will be exactly as efficient as the mutable ones.

* The more likely outcome is that immutable versions will come with a small penalty to performance by expanding the <code>Enum{Set,Map}</code> ecosystem.

* Take <code>RegularEnumSet.equals(object)</code> for example.  Each time we create a new type of <code>EnumSet</code>, are we going to change that code to add a new <code>instanceof</code> check for our new type?  If we add the check, we make that code worse at handling everything <em>except</em> our new type.  If we don't add the check, we.... still make that code worse!  It's less effective than it used to be; more <code>EnumSet</code> instances trigger the slow path.

* Classes like <code>Enum{Set,Map}</code> have a userbase that is more sensitive to changes in performance than average users.  If adding a new type causes some call site to become <a href="http://insightfullogic.com/2014/May/12/fast-and-megamorphic-what-influences-method-invoca/">megamorphic</a>, we might have thrown their carefully-crafted assumptions regarding performance out the window.

* If we decide to add immutable <code>Enum{Set,Map}</code>, we should do so for reasons unrelated to performance.

* <strong>As an exception to the rule, an immutable <code>EnumSet</code> containing all constants of a single <code>enum</code> type would be <em>really</em> fast.</strong>

* <code>RegularEnumSet</code> sets such a high bar for efficiency.  There is almost no wiggle room in <code>Set</code> operations like <code>contains(element)</code> for anyone else to be faster.  Here's the source code for <a href="http://hg.openjdk.java.net/jdk10/jdk10/jdk/file/72f33dbfcf3b/src/java.base/share/classes/java/util/RegularEnumSet.java#l140"><code>RegularEnumSet.contains(element)</code></a>:

<pre class="enums">
<span class="enums kwa">public</span> <span class="enums kwb">boolean</span> <span class="enums kwd">contains</span><span class="enums opt">(</span>Object e<span class="enums opt">) {</span>
  <span class="enums kwa">if</span> <span class="enums opt">(</span>e <span class="enums opt">==</span> null<span class="enums opt">)</span>
    <span class="enums kwa">return</span> false<span class="enums opt">;</span>
  Class<span class="enums opt">&lt;</span>?<span class="enums opt">&gt;</span> eClass <span class="enums opt">=</span> e<span class="enums opt">.</span><span class="enums kwd">getClass</span><span class="enums opt">();</span>
  <span class="enums kwa">if</span> <span class="enums opt">(</span>eClass <span class="enums opt">!=</span> elementType <span class="enums opt">&amp;&amp;</span> eClass<span class="enums opt">.</span><span class="enums kwd">getSuperclass</span><span class="enums opt">() !=</span> elementType<span class="enums opt">)</span>
    <span class="enums kwa">return</span> false<span class="enums opt">;</span>

  <span class="enums kwa">return</span> <span class="enums opt">(</span>elements <span class="enums opt">&amp; (</span><span class="enums num">1L</span> <span class="enums opt">&lt;&lt; ((</span>Enum<span class="enums opt">&lt;</span>?<span class="enums opt">&gt;)</span>e<span class="enums opt">).</span><span class="enums kwd">ordinal</span><span class="enums opt">())) !=</span> <span class="enums num">0</span><span class="enums opt">;</span>
<span class="enums opt">}</span>
</pre>

* If you can't do <code>contains(element)</code> faster than that, you've already lost.  Your <code>EnumSet</code> is probably worthless.

* There is a worthy contender, which I'll call <code>FullEnumSet</code>.  It is an <code>EnumSet</code> that (always) contains every constant of a single <code>enum</code> type.  Here is one way to write that class:

<pre class="enums">
<span class="enums kwa">package</span> java<span class="enums opt">.</span>util<span class="enums opt">;</span>

<span class="enums kwa">import</span> java<span class="enums opt">.</span>util<span class="enums opt">.</span>function<span class="enums opt">.</span>Consumer<span class="enums opt">;</span>
<span class="enums kwa">import</span> java<span class="enums opt">.</span>util<span class="enums opt">.</span>function<span class="enums opt">.</span>Predicate<span class="enums opt">;</span>

<span class="enums kwa">class</span> FullEnumSet<span class="enums opt">&lt;</span>E <span class="enums kwa">extends</span> Enum<span class="enums opt">&lt;</span>E<span class="enums opt">&gt;&gt;</span> <span class="enums kwa">extends</span> EnumSet<span class="enums opt">&lt;</span>E<span class="enums opt">&gt; {</span>

  <span class="enums slc">// TODO: Add a static factory method somewhere.</span>
  <span class="enums kwd">FullEnumSet</span><span class="enums opt">(</span>Class<span class="enums opt">&lt;</span>E<span class="enums opt">&gt;</span> elementType<span class="enums opt">,</span> Enum<span class="enums opt">&lt;</span>?<span class="enums opt">&gt;[]</span> universe<span class="enums opt">) {</span>
    <span class="enums kwa">super</span><span class="enums opt">(</span>elementType<span class="enums opt">,</span> universe<span class="enums opt">);</span>
  <span class="enums opt">}</span>

  <span class="enums kwc">&#64;Override</span>
  <span class="enums kwc">&#64;SuppressWarnings</span><span class="enums opt">(</span><span class="enums str">&quot;unchecked&quot;</span><span class="enums opt">)</span>
  <span class="enums kwa">public</span> Iterator<span class="enums opt">&lt;</span>E<span class="enums opt">&gt;</span> <span class="enums kwd">iterator</span><span class="enums opt">() {</span>
    <span class="enums slc">// TODO: Avoid calling Arrays.asList.</span>
    <span class="enums slc">//       The iterator class can be shared and used directly.</span>
    <span class="enums kwa">return</span> Arrays<span class="enums opt">.</span><span class="enums kwd">asList</span><span class="enums opt">((</span>E<span class="enums opt">[])</span> universe<span class="enums opt">).</span><span class="enums kwd">iterator</span><span class="enums opt">();</span>
  <span class="enums opt">}</span>

  <span class="enums kwc">&#64;Override</span>
  <span class="enums kwa">public</span> Spliterator<span class="enums opt">&lt;</span>E<span class="enums opt">&gt;</span> <span class="enums kwd">spliterator</span><span class="enums opt">() {</span>
    <span class="enums kwa">return</span> Spliterators<span class="enums opt">.</span><span class="enums kwd">spliterator</span><span class="enums opt">(</span>
        universe<span class="enums opt">,</span>
        Spliterator<span class="enums opt">.</span>ORDERED <span class="enums opt">|</span>
            Spliterator<span class="enums opt">.</span>SORTED <span class="enums opt">|</span>
            Spliterator<span class="enums opt">.</span>IMMUTABLE <span class="enums opt">|</span>
            Spliterator<span class="enums opt">.</span>NONNULL <span class="enums opt">|</span>
            Spliterator<span class="enums opt">.</span>DISTINCT<span class="enums opt">);</span>
  <span class="enums opt">}</span>

  <span class="enums kwc">&#64;Override</span>
  <span class="enums kwa">public</span> <span class="enums kwb">int</span> <span class="enums kwd">size</span><span class="enums opt">() {</span>
    <span class="enums kwa">return</span> universe<span class="enums opt">.</span>length<span class="enums opt">;</span>
  <span class="enums opt">}</span>

  <span class="enums kwc">&#64;Override</span>
  <span class="enums kwa">public</span> <span class="enums kwb">boolean</span> <span class="enums kwd">contains</span><span class="enums opt">(</span>Object e<span class="enums opt">) {</span>
    <span class="enums kwa">if</span> <span class="enums opt">(</span>e <span class="enums opt">==</span> null<span class="enums opt">)</span>
      <span class="enums kwa">return</span> false<span class="enums opt">;</span>

   Class<span class="enums opt">&lt;</span>?<span class="enums opt">&gt;</span> eClass <span class="enums opt">=</span> e<span class="enums opt">.</span><span class="enums kwd">getClass</span><span class="enums opt">();</span>
    <span class="enums kwa">return</span> eClass <span class="enums opt">==</span> elementType <span class="enums opt">||</span> eClass<span class="enums opt">.</span><span class="enums kwd">getSuperclass</span><span class="enums opt">() ==</span> elementType<span class="enums opt">;</span>
  <span class="enums opt">}</span>

  <span class="enums kwc">&#64;Override</span>
  <span class="enums kwa">public</span> <span class="enums kwb">boolean</span> <span class="enums kwd">containsAll</span><span class="enums opt">(</span>Collection<span class="enums opt">&lt;</span>?<span class="enums opt">&gt;</span> c<span class="enums opt">) {</span>
    <span class="enums kwa">if</span> <span class="enums opt">(!(</span>c <span class="enums kwa">instanceof</span> EnumSet<span class="enums opt">))</span>
      <span class="enums kwa">return super</span><span class="enums opt">.</span><span class="enums kwd">containsAll</span><span class="enums opt">(</span>c<span class="enums opt">);</span>

   EnumSet<span class="enums opt">&lt;</span>?<span class="enums opt">&gt;</span> es <span class="enums opt">= (</span>EnumSet<span class="enums opt">&lt;</span>?<span class="enums opt">&gt;)</span> c<span class="enums opt">;</span>
    <span class="enums kwa">return</span> es<span class="enums opt">.</span>elementType <span class="enums opt">==</span> elementType <span class="enums opt">||</span> es<span class="enums opt">.</span><span class="enums kwd">isEmpty</span><span class="enums opt">();</span>
  <span class="enums opt">}</span>

  <span class="enums kwc">&#64;Override</span>
  <span class="enums kwc">&#64;SuppressWarnings</span><span class="enums opt">(</span><span class="enums str">&quot;unchecked&quot;</span><span class="enums opt">)</span>
  <span class="enums kwa">public</span> <span class="enums kwb">void</span> <span class="enums kwd">forEach</span><span class="enums opt">(</span>Consumer<span class="enums opt">&lt;</span>? <span class="enums kwa">super</span> E<span class="enums opt">&gt;</span> action<span class="enums opt">) {</span>
    <span class="enums kwb">int</span> i <span class="enums opt">=</span> <span class="enums num">0</span><span class="enums opt">,</span> n <span class="enums opt">=</span> universe<span class="enums opt">.</span>length<span class="enums opt">;</span>
    <span class="enums kwa">if</span> <span class="enums opt">(</span>i <span class="enums opt">&gt;=</span> n<span class="enums opt">) {</span>
      Objects<span class="enums opt">.</span><span class="enums kwd">requireNonNull</span><span class="enums opt">(</span>action<span class="enums opt">);</span>
      <span class="enums kwa">return</span><span class="enums opt">;</span>
    <span class="enums opt">}</span>
    <span class="enums kwa">do</span> action<span class="enums opt">.</span><span class="enums kwd">accept</span><span class="enums opt">((</span>E<span class="enums opt">)</span> universe<span class="enums opt">[</span>i<span class="enums opt">]);</span>
    <span class="enums kwa">while</span> <span class="enums opt">(++</span>i <span class="enums opt">&lt;</span> n<span class="enums opt">);</span>
  <span class="enums opt">}</span>

  <span class="enums kwc">&#64;Override</span> <span class="enums kwb">void</span> <span class="enums kwd">addAll</span><span class="enums opt">()               {</span><span class="enums kwa">throw</span> <span class="enums kwd">uoe</span><span class="enums opt">();}</span>
  <span class="enums kwc">&#64;Override</span> <span class="enums kwb">void</span> <span class="enums kwd">addRange</span><span class="enums opt">(</span>E from<span class="enums opt">,</span> E to<span class="enums opt">) {</span><span class="enums kwa">throw</span> <span class="enums kwd">uoe</span><span class="enums opt">();}</span>
  <span class="enums kwc">&#64;Override</span> <span class="enums kwb">void</span> <span class="enums kwd">complement</span><span class="enums opt">()           {</span><span class="enums kwa">throw</span> <span class="enums kwd">uoe</span><span class="enums opt">();}</span>

  <span class="enums kwc">&#64;Override</span> <span class="enums kwa">public</span> <span class="enums kwb">boolean</span> <span class="enums kwd">add</span><span class="enums opt">(</span>E e<span class="enums opt">)                          {</span><span class="enums kwa">throw</span> <span class="enums kwd">uoe</span><span class="enums opt">();}</span>
  <span class="enums kwc">&#64;Override</span> <span class="enums kwa">public</span> <span class="enums kwb">boolean</span> <span class="enums kwd">addAll</span><span class="enums opt">(</span>Collection<span class="enums opt">&lt;</span>? <span class="enums kwa">extends</span> E<span class="enums opt">&gt;</span> c<span class="enums opt">) {</span><span class="enums kwa">throw</span> <span class="enums kwd">uoe</span><span class="enums opt">();}</span>
  <span class="enums kwc">&#64;Override</span> <span class="enums kwa">public</span> <span class="enums kwb">void</span>    <span class="enums kwd">clear</span><span class="enums opt">()                           {</span><span class="enums kwa">throw</span> <span class="enums kwd">uoe</span><span class="enums opt">();}</span>
  <span class="enums kwc">&#64;Override</span> <span class="enums kwa">public</span> <span class="enums kwb">boolean</span> <span class="enums kwd">remove</span><span class="enums opt">(</span>Object e<span class="enums opt">)                  {</span><span class="enums kwa">throw</span> <span class="enums kwd">uoe</span><span class="enums opt">();}</span>
  <span class="enums kwc">&#64;Override</span> <span class="enums kwa">public</span> <span class="enums kwb">boolean</span> <span class="enums kwd">removeAll</span><span class="enums opt">(</span>Collection<span class="enums opt">&lt;</span>?<span class="enums opt">&gt;</span> c<span class="enums opt">)        {</span><span class="enums kwa">throw</span> <span class="enums kwd">uoe</span><span class="enums opt">();}</span>
  <span class="enums kwc">&#64;Override</span> <span class="enums kwa">public</span> <span class="enums kwb">boolean</span> <span class="enums kwd">removeIf</span><span class="enums opt">(</span>Predicate<span class="enums opt">&lt;</span>? <span class="enums kwa">super</span> E<span class="enums opt">&gt;</span> f<span class="enums opt">)  {</span><span class="enums kwa">throw</span> <span class="enums kwd">uoe</span><span class="enums opt">();}</span>
  <span class="enums kwc">&#64;Override</span> <span class="enums kwa">public</span> <span class="enums kwb">boolean</span> <span class="enums kwd">retainAll</span><span class="enums opt">(</span>Collection<span class="enums opt">&lt;</span>?<span class="enums opt">&gt;</span> c<span class="enums opt">)        {</span><span class="enums kwa">throw</span> <span class="enums kwd">uoe</span><span class="enums opt">();}</span>

  <span class="enums kwa">private static</span> UnsupportedOperationException <span class="enums kwd">uoe</span><span class="enums opt">() {</span>
    <span class="enums kwa">return new</span> <span class="enums kwd">UnsupportedOperationException</span><span class="enums opt">();</span>
  <span class="enums opt">}</span>

  <span class="enums slc">// TODO: Figure out serialization.</span>
  <span class="enums slc">//       Serialization should preserve these qualities:</span>
  <span class="enums slc">//         - Immutable</span>
  <span class="enums slc">//         - Full</span>
  <span class="enums slc">//         - Singleton?</span>
  <span class="enums slc">//       Maybe it&apos;s a bad idea to extend EnumSet?</span>
  <span class="enums kwa">private static final</span> <span class="enums kwb">long</span> serialVersionUID <span class="enums opt">=</span> <span class="enums num">0</span><span class="enums opt">;</span>
<span class="enums opt">}</span>
</pre>

* <code>FullEnumSet</code> has many desirable properties.  Of note:

- <code>contains(element)</code> only needs to check the type of the argument to know whether it's a member of the set.
- <code>containsAll(collection)</code> is extremely fast when the argument is an <code>EnumSet</code> (of any kind); it boils down to comparing the element types of the two sets.  It follows that <code>equals(object)</code> is just as fast in that case, since <code>equals</code> delegates the hard work to <code>containsAll</code>.
- Since all the elements are contained in one flat array with no empty spaces, conditions are ideal for iterating and for splitting (splitting efficiency is important in the context of parallel streams).
- It beats <code>RegularEnumSet</code> in <em>all</em> important metrics:

    - Query speed (<code>contains(element)</code>, etc.)
    - Iteration speed
    - Space consumed

* Asking for the full set of <code>enum</code> constants of some type is a very common operation.  See: every user of <code>values()</code>, <code>elementType.getEnumConstants()</code>, and <code>EnumSet.allOf(elementType)</code>.  I bet the vast majority of those users do not modify (their copy of) that set of constants.  A class that is specifically tailored to that use case has a good chance of being worthwhile.

* Since it's immutable, the <code>FullEnumSet</code> of each <code>enum</code> type could be a lazy-initialized singleton.

* <strong>Should immutable <code>Enum{Set,Map}</code> reuse existing code, or should they be rewritten from scratch?</strong>

* As I said earlier, the immutable versions of these classes aren't going to be any faster.  If they're built from scratch, that code is going to look near-identical to the existing code.  There would be a painful amount of copy and pasting, and I would not envy the people responsible for maintaining that code in the future.

* Suppose we want to reuse the existing code.  I see two general approaches:

1. Do what Guava did, basically.  Create unmodifiable wrappers around modifiable <code>Enum{Set,Map}</code>.  Both the wrappers and the modifiable collections should be able to unwrap intelligently to take advantage of the existing optimizations for particular <code>Enum{Set,Map}</code> types (as in <code>RegularEnumSet.equals(object)</code>).

2. Extend the modifiable <code>Enum{Set,Map}</code> classes with new classes that override modifier methods to throw <code>UnsupportedOperationException</code>.  Optimizations that sniff for particular <code>Enum{Set,Map}</code> types (as in <code>RegularEnumSet.equals(object)</code>) remain exactly as effective as before without changes.

* Of those two, I prefer the Guava-like approach.  Extending the existing classes raises some difficult questions about the public API, particularly with respect to serialization.

* <strong>What's the public API for immutable <code>Enum{Set,Map}</code>?  What's the immutable version of <code>EnumSet.of(e1, e2, e3)</code>?</strong>

* Here's where I gave up.

- Should we add public <code>java.util.ImmutableEnum{Set,Map}</code> classes?
- If not, where do we put the factory methods, and what do we name them?  <code>EnumSet.immutableOf(e1, e2, e3)</code>?  <code>EnumSet.immutableAllOf(Month.class)</code>?  Yuck!  (Clever synonyms like "having" and "universeOf" might be even worse.)
- Are the new classes instances of <code>Enum{Set,Map}</code> or do they exist in an unrelated class hierarchy?
- If the new classes do extend <code>Enum{Set,Map}</code>, how is serialization affected?  Do we add an "isImmutable" bit to the current serialized forms?  Can that be done without breaking backwards compatibility?

* Good luck to whoever has to produce the final answers to those questions.

* That's enough about this topic.  Let's move on.

### Add factory methods

* <a href="https://bugs.openjdk.java.net/browse/JDK-8145048">JDK-8145048</a> mentions the possibility of adding factory methods in <code>Enum{Set,Map}</code> to align them with <a href="http://openjdk.java.net/jeps/269">Java 9's <code>Set</code> and <code>Map</code> factories</a>.  <code>EnumSet</code> already has a varargs <code>EnumSet.of(...)</code> factory method, but <code>EnumMap</code> has nothing like that.

* It would be nice to be able to declare <code>EnumMap</code> instances like this, for some reasonable number of key-value pairs:

<pre class="enums">
Map<span class="enums opt">&lt;</span>DayOfWeek<span class="enums opt">,</span> String<span class="enums opt">&gt;</span> dayNames <span class="enums opt">=</span>
    EnumMap<span class="enums opt">.</span><span class="enums kwd">of</span><span class="enums opt">(</span>
        DayOfWeek<span class="enums opt">.</span>MONDAY<span class="enums opt">,</span>    <span class="enums str">&quot;lunes&quot;</span><span class="enums opt">,</span>
        DayOfWeek<span class="enums opt">.</span>TUESDAY<span class="enums opt">,</span>   <span class="enums str">&quot;martes&quot;</span><span class="enums opt">,</span>
        DayOfWeek<span class="enums opt">.</span>WEDNESDAY<span class="enums opt">,</span> <span class="enums str">&quot;miércoles&quot;</span><span class="enums opt">,</span>
        DayOfWeek<span class="enums opt">.</span>THURSDAY<span class="enums opt">,</span>  <span class="enums str">&quot;jueves&quot;</span><span class="enums opt">,</span>
        DayOfWeek<span class="enums opt">.</span>FRIDAY<span class="enums opt">,</span>    <span class="enums str">&quot;viernes&quot;</span><span class="enums opt">,</span>
        DayOfWeek<span class="enums opt">.</span>SATURDAY<span class="enums opt">,</span>  <span class="enums str">&quot;sábado&quot;</span><span class="enums opt">,</span>
        DayOfWeek<span class="enums opt">.</span>SUNDAY<span class="enums opt">,</span>    <span class="enums str">&quot;domingo&quot;</span><span class="enums opt">);</span>
</pre>

* Users could use <code>EnumMap</code>'s <a href="https://docs.oracle.com/javase/8/docs/api/java/util/EnumMap.html#EnumMap-java.util.Map-">copy constructor</a> in conjunction with Java 9's <code>Map</code> factory methods to achieve the same result less efficiently...

<pre class="enums">
Map<span class="enums opt">&lt;</span>DayOfWeek<span class="enums opt">,</span> String<span class="enums opt">&gt;</span> dayNames <span class="enums opt">=</span>
    <span class="enums kwa">new</span> EnumMap<span class="enums opt">&lt;&gt;(</span>
        Map<span class="enums opt">.</span><span class="enums kwd">of</span><span class="enums opt">(</span>
            DayOfWeek<span class="enums opt">.</span>MONDAY<span class="enums opt">,</span>    <span class="enums str">&quot;lunes&quot;</span><span class="enums opt">,</span>
            DayOfWeek<span class="enums opt">.</span>TUESDAY<span class="enums opt">,</span>   <span class="enums str">&quot;martes&quot;</span><span class="enums opt">,</span>
            DayOfWeek<span class="enums opt">.</span>WEDNESDAY<span class="enums opt">,</span> <span class="enums str">&quot;miércoles&quot;</span><span class="enums opt">,</span>
            DayOfWeek<span class="enums opt">.</span>THURSDAY<span class="enums opt">,</span>  <span class="enums str">&quot;jueves&quot;</span><span class="enums opt">,</span>
            DayOfWeek<span class="enums opt">.</span>FRIDAY<span class="enums opt">,</span>    <span class="enums str">&quot;viernes&quot;</span><span class="enums opt">,</span>
            DayOfWeek<span class="enums opt">.</span>SATURDAY<span class="enums opt">,</span>  <span class="enums str">&quot;sábado&quot;</span><span class="enums opt">,</span>
            DayOfWeek<span class="enums opt">.</span>SUNDAY<span class="enums opt">,</span>    <span class="enums str">&quot;domingo&quot;</span><span class="enums opt">));</span>
</pre>

* ...but the more we give up efficiency like that, the less <code>EnumMap</code> makes sense in the first place.  A reasonable person might start to question why they should bother with <code>EnumMap</code> at all &mdash; just get rid of the <code>new EnumMap&lt;&gt;(...)</code> wrapper and use <code>Map.of(...)</code> directly.

* Speaking of that <code>EnumMap(Map)</code> copy constructor, the fact that it may throw <code>IllegalArgumentException</code> when provided an empty <code>Map</code> leads people to use this pattern instead:

<pre class="enums">
Map<span class="enums opt">&lt;</span>DayOfWeek<span class="enums opt">,</span> String<span class="enums opt">&gt;</span> copy <span class="enums opt">=</span> <span class="enums kwa">new</span> EnumMap<span class="enums opt">&lt;&gt;(</span>DayOfWeek<span class="enums opt">.</span><span class="enums kwa">class</span><span class="enums opt">);</span>
copy<span class="enums opt">.</span><span class="enums kwd">putAll</span><span class="enums opt">(</span>otherMap<span class="enums opt">);</span>
</pre>

* We could give them a shortcut:

<pre class="enums">
Map<span class="enums opt">&lt;</span>DayOfWeek<span class="enums opt">,</span> String<span class="enums opt">&gt;</span> copy <span class="enums opt">=</span> <span class="enums kwa">new</span> EnumMap<span class="enums opt">&lt;&gt;(</span>DayOfWeek<span class="enums opt">.</span><span class="enums kwa">class</span><span class="enums opt">,</span> otherMap<span class="enums opt">);</span>
</pre>

* Similarly, to avoid an <code>IllegalArgumentException</code> from <a href="https://docs.oracle.com/javase/8/docs/api/java/util/EnumSet.html#copyOf-java.util.Collection-"><code>EnumSet.copyOf(collection)</code></a>, I see code like this:

<pre class="enums">
Set<span class="enums opt">&lt;</span>Month<span class="enums opt">&gt;</span> copy <span class="enums opt">=</span> EnumSet<span class="enums opt">.</span><span class="enums kwd">noneOf</span><span class="enums opt">(</span>Month<span class="enums opt">.</span><span class="enums kwa">class</span><span class="enums opt">);</span>
copy<span class="enums opt">.</span><span class="enums kwd">addAll</span><span class="enums opt">(</span>otherCollection<span class="enums opt">);</span>
</pre>

* We could give them a shortcut too:

<pre class="enums">
Set<span class="enums opt">&lt;</span>Month<span class="enums opt">&gt;</span> copy <span class="enums opt">=</span> EnumSet<span class="enums opt">.</span><span class="enums kwd">copyOf</span><span class="enums opt">(</span>Month<span class="enums opt">.</span><span class="enums kwa">class</span><span class="enums opt">,</span> otherCollection<span class="enums opt">);</span>
</pre>

* Existing code may define mappings from <code>enum</code> constants to values as standalone functions.  Maybe the users of that code would like to view those (function-based) mappings as <code>Map</code> objects.

* To that end, we could give people the means to generate an <code>EnumMap</code> from a <a href="https://docs.oracle.com/javase/8/docs/api/java/util/function/Function.html"><code>Function</code></a>:

<pre class="enums">
Locale locale <span class="enums opt">=</span> Locale<span class="enums opt">.</span><span class="enums kwd">forLanguageTag</span><span class="enums opt">(</span><span class="enums str">&quot;es-MX&quot;</span><span class="enums opt">);</span>

Map<span class="enums opt">&lt;</span>DayOfWeek<span class="enums opt">,</span> String<span class="enums opt">&gt;</span> dayNames <span class="enums opt">=</span>
    EnumMap<span class="enums opt">.</span><span class="enums kwd">map</span><span class="enums opt">(</span>DayOfWeek<span class="enums opt">.</span><span class="enums kwa">class</span><span class="enums opt">,</span>
                day <span class="enums opt">-&gt;</span> day<span class="enums opt">.</span><span class="enums kwd">getDisplayName</span><span class="enums opt">(</span>TextStyle<span class="enums opt">.</span>FULL<span class="enums opt">,</span> locale<span class="enums opt">));</span>

<span class="enums slc">// We could interpret the function returning null to mean that the</span>
<span class="enums slc">// key is not present.  That would allow this method to support</span>
<span class="enums slc">// more than the &quot;every constant is a key&quot; use case while dropping</span>
<span class="enums slc">// support for the &quot;there may be present null values&quot; use case,</span>
<span class="enums slc">// which is probably a good trade.</span>
</pre>

* We could provide a similar factory method for <code>EnumSet</code>, accepting a <a href="https://docs.oracle.com/javase/8/docs/api/java/util/function/Predicate.html"><code>Predicate</code></a> instead of a <code>Function</code>:

<pre class="enums">
Set<span class="enums opt">&lt;</span>Month<span class="enums opt">&gt;</span> shortMonths <span class="enums opt">=</span>
    EnumSet<span class="enums opt">.</span><span class="enums kwd">filter</span><span class="enums opt">(</span>Month<span class="enums opt">.</span><span class="enums kwa">class</span><span class="enums opt">,</span>
                   month <span class="enums opt">-&gt;</span> month<span class="enums opt">.</span><span class="enums kwd">minLength</span><span class="enums opt">() &lt;</span> <span class="enums num">31</span><span class="enums opt">);</span>
</pre>

* This functionality could be achieved less efficiently and more verbosely with streams.  Again, the more we give up efficiency like that, the less sense it makes to use <code>Enum{Set,Map}</code> in the first place.  I acknowledge that there is a cost to making API-level changes like the ones I'm discussing, but I feel that we are solidly in the "too little API-level support for <code>Enum{Set,Map}</code>" part of the spectrum and not even close to approaching the opposite "API bloat" end.

* I don't mean to belittle streams.  There should also be more support for <code>Enum{Set,Map}</code> in the stream API.

### Add collectors

* Code written for Java 8+ will often produce collections using <a href="https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html">streams</a> and <a href="https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collector.html">collectors</a> rather than invoking collection constructors or factory methods directly.  I don't think it would be outlandish to estimate that one third of collections are produced by collectors.  Some of these collections will be (or could be) <code>Enum{Set,Map}</code>, and more could be done to serve that use case.

* Collectors with these signatures should exist somewhere in the JDK:

<pre class="enums">
<span class="enums kwa">public static</span> <span class="enums opt">&lt;</span>T <span class="enums kwa">extends</span> Enum<span class="enums opt">&lt;</span>T<span class="enums opt">&gt;&gt;</span>
Collector<span class="enums opt">&lt;</span>T<span class="enums opt">,</span> ?<span class="enums opt">,</span> EnumSet<span class="enums opt">&lt;</span>T<span class="enums opt">&gt;&gt;</span> <span class="enums kwd">toEnumSet</span><span class="enums opt">(</span>
    Class<span class="enums opt">&lt;</span>T<span class="enums opt">&gt;</span> elementType<span class="enums opt">)</span>

<span class="enums kwa">public static</span> <span class="enums opt">&lt;</span>T<span class="enums opt">,</span> K <span class="enums kwa">extends</span> Enum<span class="enums opt">&lt;</span>K<span class="enums opt">&gt;,</span> U<span class="enums opt">&gt;</span>
Collector<span class="enums opt">&lt;</span>T<span class="enums opt">,</span> ?<span class="enums opt">,</span> EnumMap<span class="enums opt">&lt;</span>K<span class="enums opt">,</span> U<span class="enums opt">&gt;&gt;</span> <span class="enums kwd">toEnumMap</span><span class="enums opt">(</span>
    Class<span class="enums opt">&lt;</span>K<span class="enums opt">&gt;</span> keyType<span class="enums opt">,</span>
    Function<span class="enums opt">&lt;</span>? <span class="enums kwa">super</span> T<span class="enums opt">,</span> ? <span class="enums kwa">extends</span> K<span class="enums opt">&gt;</span> keyMapper<span class="enums opt">,</span>
    Function<span class="enums opt">&lt;</span>? <span class="enums kwa">super</span> T<span class="enums opt">,</span> ? <span class="enums kwa">extends</span> U<span class="enums opt">&gt;</span> valueMapper<span class="enums opt">)</span>

<span class="enums kwa">public static</span> <span class="enums opt">&lt;</span>T<span class="enums opt">,</span> K <span class="enums kwa">extends</span> Enum<span class="enums opt">&lt;</span>K<span class="enums opt">&gt;,</span> U<span class="enums opt">&gt;</span>
Collector<span class="enums opt">&lt;</span>T<span class="enums opt">,</span> ?<span class="enums opt">,</span> EnumMap<span class="enums opt">&lt;</span>K<span class="enums opt">,</span> U<span class="enums opt">&gt;&gt;</span> <span class="enums kwd">toEnumMap</span><span class="enums opt">(</span>
    Class<span class="enums opt">&lt;</span>K<span class="enums opt">&gt;</span> keyType<span class="enums opt">,</span>
    Function<span class="enums opt">&lt;</span>? <span class="enums kwa">super</span> T<span class="enums opt">,</span> ? <span class="enums kwa">extends</span> K<span class="enums opt">&gt;</span> keyMapper<span class="enums opt">,</span>
    Function<span class="enums opt">&lt;</span>? <span class="enums kwa">super</span> T<span class="enums opt">,</span> ? <span class="enums kwa">extends</span> U<span class="enums opt">&gt;</span> valueMapper<span class="enums opt">,</span>
    BinaryOperator<span class="enums opt">&lt;</span>U<span class="enums opt">&gt;</span> mergeFunction<span class="enums opt">)</span>
</pre>

* Similar collectors can be obtained from the existing collector factories in the <a href="https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html"><code>Collectors</code></a> class (specifically <a href="https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html#toCollection-java.util.function.Supplier-"><code>toCollection(collectionSupplier)</code></a> and <a href="https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html#toMap-java.util.function.Function-java.util.function.Function-java.util.function.BinaryOperator-java.util.function.Supplier-"><code>toMap(keyMapper, valueMapper, mergeFunction, mapSupplier)</code></a>) or by using <a href="https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collector.html#of-java.util.function.Supplier-java.util.function.BiConsumer-java.util.function.BinaryOperator-java.util.stream.Collector.Characteristics...-"><code>Collector.of(...)</code></a>, but that requires a little more effort on the users' part, adding a little bit of extra friction to using <code>Enum{Set,Map}</code> that we don't need.

* I referenced these collectors from Guava earlier in this article:

- <a href="http://google.github.io/guava/releases/21.0/api/docs/com/google/common/collect/Sets.html#toImmutableEnumSet--"><code>Sets.toImmutableEnumSet()</code></a>
- <a href="http://google.github.io/guava/releases/21.0/api/docs/com/google/common/collect/Maps.html#toImmutableEnumMap-java.util.function.Function-java.util.function.Function-"><code>Maps.toImmutableEnumMap(keyMapper, valueMapper)</code></a>
- <a href="http://google.github.io/guava/releases/21.0/api/docs/com/google/common/collect/Maps.html#toImmutableEnumMap-java.util.function.Function-java.util.function.Function-java.util.function.BinaryOperator-"><code>Maps.toImmutableEnumMap(keyMapper, valueMapper, mergeFunction)</code></a>

* They do not require the <code>Class</code> object argument, making them easier to use than the collectors that I proposed.  The reason the Guava collectors can do this is that they produce <a href="http://google.github.io/guava/releases/21.0/api/docs/com/google/common/collect/ImmutableSet.html"><code>ImmutableSet</code></a> and <a href="http://google.github.io/guava/releases/21.0/api/docs/com/google/common/collect/ImmutableMap.html"><code>ImmutableMap</code></a>, not <code>EnumSet</code> and <code>EnumMap</code>.  One cannot create an <code>Enum{Set,Map}</code> instance without having the <code>Class</code> object for that <code>enum</code> type.  In order to have a collector that reliably produces <code>Enum{Set,Map}</code> (even when the stream contains zero input elements to grab the <code>Class</code> object from), the <code>Class</code> object must be provided up front.

* We <em>could</em> provide similar collectors in the JDK that would produce immutable <code>Set</code> and <code>Map</code> instances.  For streams with no elements, the collectors would produce <a href="https://docs.oracle.com/javase/8/docs/api/java/util/Collections.html#emptySet--"><code>Collections.emptySet()</code></a> or <a href="https://docs.oracle.com/javase/8/docs/api/java/util/Collections.html#emptyMap--"><code>Collections.emptyMap()</code></a>.  For streams with at least one element, the collectors would produce an <code>Enum{Set,Map}</code> instance wrapped by <code>Collections.unmodifiable{Set,Map}</code>.

* The signatures would look like this:

<pre class="enums">
<span class="enums kwa">public static</span> <span class="enums opt">&lt;</span>T <span class="enums kwa">extends</span> Enum<span class="enums opt">&lt;</span>T<span class="enums opt">&gt;&gt;</span>
Collector<span class="enums opt">&lt;</span>T<span class="enums opt">,</span> ?<span class="enums opt">,</span> Set<span class="enums opt">&lt;</span>T<span class="enums opt">&gt;&gt;</span> <span class="enums kwd">toImmutableEnumSet</span><span class="enums opt">()</span>

<span class="enums kwa">public static</span> <span class="enums opt">&lt;</span>T<span class="enums opt">,</span> K <span class="enums kwa">extends</span> Enum<span class="enums opt">&lt;</span>K<span class="enums opt">&gt;,</span> U<span class="enums opt">&gt;</span>
Collector<span class="enums opt">&lt;</span>T<span class="enums opt">,</span> ?<span class="enums opt">,</span> Map<span class="enums opt">&lt;</span>K<span class="enums opt">,</span> U<span class="enums opt">&gt;&gt;</span> <span class="enums kwd">toImmutableEnumMap</span><span class="enums opt">(</span>
    Function<span class="enums opt">&lt;</span>? <span class="enums kwa">super</span> T<span class="enums opt">,</span> ? <span class="enums kwa">extends</span> K<span class="enums opt">&gt;</span> keyMapper<span class="enums opt">,</span>
    Function<span class="enums opt">&lt;</span>? <span class="enums kwa">super</span> T<span class="enums opt">,</span> ? <span class="enums kwa">extends</span> U<span class="enums opt">&gt;</span> valueMapper<span class="enums opt">)</span>

<span class="enums kwa">public static</span> <span class="enums opt">&lt;</span>T<span class="enums opt">,</span> K <span class="enums kwa">extends</span> Enum<span class="enums opt">&lt;</span>K<span class="enums opt">&gt;,</span> U<span class="enums opt">&gt;</span>
Collector<span class="enums opt">&lt;</span>T<span class="enums opt">,</span> ?<span class="enums opt">,</span> Map<span class="enums opt">&lt;</span>K<span class="enums opt">,</span> U<span class="enums opt">&gt;&gt;</span> <span class="enums kwd">toImmutableEnumMap</span><span class="enums opt">(</span>
    Function<span class="enums opt">&lt;</span>? <span class="enums kwa">super</span> T<span class="enums opt">,</span> ? <span class="enums kwa">extends</span> K<span class="enums opt">&gt;</span> keyMapper<span class="enums opt">,</span>
    Function<span class="enums opt">&lt;</span>? <span class="enums kwa">super</span> T<span class="enums opt">,</span> ? <span class="enums kwa">extends</span> U<span class="enums opt">&gt;</span> valueMapper<span class="enums opt">,</span>
    BinaryOperator<span class="enums opt">&lt;</span>U<span class="enums opt">&gt;</span> mergeFunction<span class="enums opt">)</span>
</pre>

* I'm not sure that those collectors are worthwhile.  I might never recommend them over their counterparts in Guava.

* The <a href="https://github.com/amaembo/streamex">StreamEx</a> library also provides a couple of interesting <code>enum</code>-specialized collectors:

- <a href="http://amaembo.github.io/streamex/javadoc/one/util/streamex/MoreCollectors.html#toEnumSet-java.lang.Class-"><code>MoreCollectors.toEnumSet(elementType)</code></a>
- <a href="http://amaembo.github.io/streamex/javadoc/one/util/streamex/MoreCollectors.html#groupingByEnum-java.lang.Class-java.util.function.Function-java.util.stream.Collector-"><code>MoreCollectors.groupingByEnum(keyType, classifier, downstreamCollector)</code></a>

* They're interesting because they are potentially short-circuiting.  With <code>MoreCollectors.toEnumSet(elementType)</code>, when the collector can determine that it has encountered all of the elements of that <code>enum</code> type (which is easy &mdash; the set of already-collected elements can be compared to <code>EnumSet.allOf(elementType)</code>), it stops collecting.  These collectors may be well-suited for streams having a huge number of elements (or having elements that are expensive to compute) mapping to a relatively small set of <code>enum</code> constants.

* I don't know how feasible it is to port these StreamEx collectors to the JDK.  As I understand it, the concept of short-circuiting collectors is not supported by the JDK.  Adding support may necessitate other changes to the stream and collector APIs.

### Be navigable?  (No)

* Over the years, many people have suggested that <code>Enum{Set,Map}</code> should implement the <a href="https://docs.oracle.com/javase/8/docs/api/java/util/NavigableSet.html"><code>NavigableSet</code></a> and <a href="https://docs.oracle.com/javase/8/docs/api/java/util/NavigableMap.html"><code>NavigableMap</code></a> interfaces.  Every <code>enum</code> type is <a href="https://docs.oracle.com/javase/8/docs/api/java/lang/Comparable.html"><code>Comparable</code></a>, so it's technically possible.  Why not?

* <strong>I think the <code>Navigable{Set,Map}</code> interfaces are a poor fit for <code>Enum{Set,Map}</code>.</strong>

* Those interfaces are huge!  Implementing <code>Navigable{Set,Map}</code> would bloat the size of <code>Enum{Set,Map}</code> by 2-4x (in terms of lines of code).  It would distract them from their core focus and strengths.  Supporting the navigable API would most likely come with a non-zero penalty to runtime performance.

* Have you ever looked closely at the specified behavior of methods like <a href="https://docs.oracle.com/javase/8/docs/api/java/util/NavigableSet.html#subSet-E-boolean-E-boolean-"><code>subSet</code></a> and <a href="https://docs.oracle.com/javase/8/docs/api/java/util/NavigableMap.html#subMap-K-boolean-K-boolean-"><code>subMap</code></a>, specifically when they might throw <code>IllegalArgumentException</code>?  Those contracts impose a great deal of complexity for what seems like undesirable behavior.  <code>Enum{Set,Map}</code> could take a stance on those methods similar to Guava's <a href="http://google.github.io/guava/releases/21.0/api/docs/com/google/common/collect/ImmutableSortedSet.html"><code>ImmutableSortedSet</code></a> and <a href="http://google.github.io/guava/releases/21.0/api/docs/com/google/common/collect/ImmutableSortedMap.html"><code>ImmutableSortedMap</code></a>:  acknowledge the contract of the interface but <a href="http://google.github.io/guava/releases/21.0/api/docs/com/google/common/collect/ImmutableSortedSet.html#subSet-E-E-">do something else</a> that is more reasonable instead...

* I say forget about it.  If you want navigable collections, use <a href="https://docs.oracle.com/javase/8/docs/api/java/util/TreeSet.html"><code>TreeSet</code></a> and <a href="https://docs.oracle.com/javase/8/docs/api/java/util/TreeMap.html"><code>TreeMap</code></a> (or their thread-safe cousins, <a href="https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ConcurrentSkipListSet.html"><code>ConcurrentSkipListSet</code></a> and <a href="https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ConcurrentSkipListMap.html"><code>ConcurrentSkipListMap</code></a>).  The cross-section of people who <em>need</em> the navigable API <em>and</em> the efficiency of <code>enum</code>-specialized collections must be very small.

* There are few cases  where the <code>Comparable</code> nature of <code>enum</code> types comes into play <em>at all</em>.  In practice, I expect that the ordering of most <code>enum</code> constants is arbitrary (with respect to intended behavior).

* I'll go further than that; <strong>I think that making all <code>enum</code> types <code>Comparable</code> in the first place was a mistake.</strong>

- Which ordering of <a href="https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collector.Characteristics.html"><code>Collector.Characteristics</code></a> is "natural", <code>[CONCURRENT,UNORDERED]</code> or <code>[UNORDERED,CONCURRENT]</code>?
- Which is the "greater" <a href="https://docs.oracle.com/javase/8/docs/api/java/lang/Thread.State.html"><code>Thread.State</code></a>, <code>WAITING</code> or <code>TIMED_WAITING</code>?
- <a href="https://docs.oracle.com/javase/8/docs/api/java/nio/file/FileVisitOption.html#FOLLOW_LINKS"><code>FileVisitOption.FOLLOW_LINKS</code></a> is "comparable" &mdash; to what?  (There is no other <code>FileVisitOption</code>.)
- How many instances of <a href="https://docs.oracle.com/javase/8/docs/api/java/math/RoundingMode.html"><code>RoundingMode</code></a> are in the "range" from <code>FLOOR</code> to <code>CEILING</code>?

<pre class="enums">
<span class="enums kwa">import</span> java<span class="enums opt">.</span>math<span class="enums opt">.</span>RoundingMode<span class="enums opt">;</span>
<span class="enums kwa">import</span> java<span class="enums opt">.</span>util<span class="enums opt">.</span>EnumSet<span class="enums opt">;</span>
<span class="enums kwa">import</span> java<span class="enums opt">.</span>util<span class="enums opt">.</span>Set<span class="enums opt">;</span>

<span class="enums kwa">class</span> RangeTest <span class="enums opt">{</span>
  <span class="enums kwa">public static</span> <span class="enums kwb">void</span> <span class="enums kwd">main</span><span class="enums opt">(</span>String<span class="enums opt">[]</span> args<span class="enums opt">) {</span>
    Set<span class="enums opt">&lt;</span>RoundingMode<span class="enums opt">&gt;</span> range <span class="enums opt">=</span>
        EnumSet<span class="enums opt">.</span><span class="enums kwd">range</span><span class="enums opt">(</span>RoundingMode<span class="enums opt">.</span>FLOOR<span class="enums opt">,</span>
                      RoundingMode<span class="enums opt">.</span>CEILING<span class="enums opt">);</span>
    System<span class="enums opt">.</span>out<span class="enums opt">.</span><span class="enums kwd">println</span><span class="enums opt">(</span>range<span class="enums opt">.</span><span class="enums kwd">size</span><span class="enums opt">());</span>
  <span class="enums opt">}</span>
<span class="enums opt">}</span>

<span class="enums slc">// java.lang.IllegalArgumentException: FLOOR &gt; CEILING</span>
</pre>

* There are other <code>enum</code> types where questions like that actually make sense, and those <em>should</em> be <code>Comparable</code>.

- Is <code>Month.JANUARY</code> "before" <code>Month.FEBRUARY</code>?  Yes.
- Is <code>TimeUnit.HOURS</code> "larger" than <code>TimeUnit.MINUTES</code>?  Yes.

* Implementing <code>Comparable</code> or not should have been a choice for authors of individual <code>enum</code> types.  To serve people who really did want to sort <code>enum</code> constants by declaration order for whatever reason, we could have automatically provided a static <code>Comparator</code> from each <code>enum</code> type:

<pre class="enums">
Comparator<span class="enums opt">&lt;</span>JDBCType<span class="enums opt">&gt;</span> c <span class="enums opt">=</span> JDBCType<span class="enums opt">.</span><span class="enums kwd">declarationOrder</span><span class="enums opt">();</span>
</pre>

* It's too late for that now.  Let's not double down on the original mistake by making <code>Enum{Set,Map}</code> navigable.

## Conclusion

<a href="https://docs.oracle.com/javase/8/docs/api/java/util/EnumSet.html"><code>EnumSet</code></a>
and
<a href="https://docs.oracle.com/javase/8/docs/api/java/util/EnumMap.html"><code>EnumMap</code></a> are cool collections, and you should use them!

They're already great, but they can become even better with changes to their private implementation details.  I propose some ideas here.  If you want to find out what happens in the JDK, the changes (if there are any) should be noted in <a href="https://bugs.openjdk.java.net/browse/JDK-8170826">JDK-8170826</a>.

API-level changes are warranted as well.  New factory methods and collectors would make it easier to obtain instances of <code>Enum{Set,Map}</code>, and immutable <code>Enum{Set,Map}</code> could be better-supported.  I propose some ideas here, but if there are any actual changes made then they should be noted in <a href="https://bugs.openjdk.java.net/browse/JDK-8145048">JDK-8145048</a>.
