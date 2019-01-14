---
layout : post
date : "2019-01-13 20:20"
title : "알고리즘 학습에 대한 조언"
tag : "algorithm"
comments : true
project : true
---

# 알고리즘 학습에 대한 조언

> 출처 : The post 알고리즘 학습에 대한 조언 appeared first on Edward Kim.

> 원문 : http://shlegeris.com/2016/08/14/algorithms

Software engineering interviews often ask whiteboard algorithms questions. Here’s my advice on how to study for them. (My credentials on this topic are: I have passed a lot of whiteboard interviews, including at Google and Apple; as part of my job I prepare programmers for these algorithm interviews; I have conducted more than 200 technical interviews on programmers with a very wide variety of backgrounds.)

* 소프트웨어공학 면접에서는 화이트보드 알고리즘 질문을 종종 냅니다. 이런 질문을 어떻게 공부해야 하는지 조언을 하려고 합니다. (저는 구글과 애플을 포함한 수많은 화이트보드 면접을 통과했습니다. 그리고 프로그래머가 이런 알고리즘 면접을 준비하도록 돕는 일이 제 직업의 일부입니다. 게다가 다양한 분야의 개발자를 대상으로 200회 이상의 기술 면접을 치뤘습니다.)

I’m speaking for myself, not for Triplebyte, in this post.
Other topics than algorithms are also covered in interviews. Triplebyte has a blog post which has useful information on studying for those. I primarily intend this post to be a supplement to section 2 of that post.

* 이 글은 Triplebyte이 아닌 제 자신으로서 쓰는 글입니다.
알고리즘 외에도 면접에 관한 여러 주제가 있습니다. 이런 주제는 Triplebyte의 포스트에서 잘 다루고 있습니다. 이 글에서 중요하게 다루려고 하는 내용은 Triplebyte의 포스트에서 2번 항목에 해당합니다.

## Background: why do companies ask algorithms questions? (배경: 왜 회사는 알고리즘 문제를 낼까요?)
In real life, programmers spend almost none of their time implementing binary search trees or graph search algorithms. So why do companies ask so many questions about them?
* 실생활에서 프로그래머가 이진트리 검색이나 그래프 탐색 알고리즘을 구현하는 시간은 거의 존재하지 않습니다. 그런데 왜 회사는 알고리즘에 대해 많은 질문을 낼까요?

There’s both a Watsonian and a Doylist interpretation of this question–“why are algorithms questions useful to companies to ask” has one answer, and “what is the actual causal mechanism by which companies decided to ask algorithms questions” has another.

* 이 질문을 존 왓슨과 코난 도일의 관점으로 해석할 수 있습니다. “회사가 알고리즘 문제를 내는 것이 왜 유용한가?”, 그리고 “어떤 회사가 알고리즘 질문을 하는 실제 일반적 원리가 무엇인가?”가 그 관점입니다.

I’ll start out by explaining the reasons to ask algorithms questions, and then move on to more cynical explanations for their prevalence.

* 먼저 알고리즘을 물어보는 이유를 설명하려고 합니다. 그리고 더 나아가 이 유행에 대한 냉소적인 입장에서 설명합니다.

To start with, a lot of professional programmers aren’t able to do very basic things. For example, I might have a list of Customer objects, each of whom has an array of Purchase objects, and I want to get the names of the five customers who have spent the most over the last week. My guess is that maybe 50% of professional programmers would not solve this problem within 30 minutes. You don’t want to hire such people by mistake.

* 먼저 직업 프로그래머 대다수가 아주 기초적인 일을 수행하지 못합니다. 예를 들어 고객 객체 목록이 있고 각 고객 객체에 구입 객체 배열이 존재합니다. 지난 주에 가장 많이 구입한 고객 다섯 명의 이름을 찾으려고 합니다. 제 예상에는 직업 프로그래머의 50%가 이 문제를 30분 이내에 풀지 못합니다. 이런 사람들을 실수로라도 채용하고 싶지 않을겁니다.

Less pessimistically, when you’re interviewing someone for a programming job, you’re trying to find out how good they are at solving hard and confusing programming problems where they need to keep lots of details in their head at once. In real life, confusing complicated problems happen because you’re working on projects that are large enough to take weeks, and you need to think about a lot of different parts of software at once. But interviews don’t normally have enough time to let you get that deep into a programming problem. So instead of asking you a problem that is only difficult because of how large it is, they ask you questions which are short and difficult.

* 조금 덜 비관적으로 가정해봅시다. 프로그래밍 일을 위해 누군가 면접을 볼 때 어렵고 혼란스러운 문제를 잘 풀어낼 수 있는지 알아내려고 할겁니다. 모든 세세한 내용을 머리에 담고 있어야 풀 수 있는 것을 말이죠. 실생활에서 혼란스럽고 복잡한 문제가 존재하는 이유는 그 프로젝트를 몇 주 동안 봐야 할 만큼 큰 규모고 소프트웨어의 여러 부분을 동시에 고려해야 하기 때문입니다. 하지만 면접은 일반적으로 그렇게 깊은 프로그래밍 문제를 다룰 만큼 시간이 넉넉하지 않습니다. 그래서 규모가 크기 때문에 복잡한 문제를 물어보는 것보다 짧고 복잡한 질문을 물어보는 것입니다.

How can you come up with complicated but short coding problems which are also simple to describe? I think algorithmics is a good choice here. Algorithmics is the most complicated area of computer science that almost all software engineers know about, and it allows lots of easy-to-describe, tricky-to-implement problems.

* 그럼 어떻게 복잡하지만 쉽게 설명할 수 있는 짧은 코딩 문제를 낼 수 있을까요? 그런 관점으로 생각해보면 제 생각에 여기서는 알고리즘이 좋은 선택입니다. 알고리즘은 컴퓨터 과학에서 대부분의 소프트웨어 엔지니어가 알고 있는 복잡한 분야며 쉽게 설명할 수 있고 구현하기 힘든 문제가 많이 존재합니다.

Now here’s are some more cynical historical notes. Interview processes are awkwardly sticky. Engineering teams are comprised entirely of people who passed the technical interview of that team. So everyone is personally incentivised to believe and say that their interview process is extremely accurate at measuring software engineering ability. So once a culture of algorithmic interviewing gets established at a company, it’s hard to change it.

* 여기 조금 냉소적인 설명이 따라옵니다. 면접 프로세스는 이상할 정도로 끈적합니다. 엔지니어링팀은 팀의 기술 면접을 통과한 사람으로만 구성되어 있습니다. 그래서 모두가 면접이 옳은 방식이라고 믿게 되고 면접 과정이 소프트웨어 엔지니어링 능력을 측정하는데 매우 정확하다고 생각하게 됩니다. 그래서 회사에 알고리즘 면접 문화가 생기고 나면 그 이후로 바꾸기가 어려워집니다.

Also, everyone knows that Google had an amazing team 10 years ago (and to a lesser extent still today), and Google asked algorithmic interview questions back then. Most companies are slightly insecure about not being Google (because they’re used to losing their best candidates to Google), so they are tempted to imitate Google’s interview process.

* 또한 모두가 알듯 구글은 10년 전에 놀라운 팀이 있었습니다. (지금은 그 당시보다 적습니다.) 그 당시에 구글은 알고리즘 면접 질문을 했습니다. 대부분의 회사가 자신들이 구글이 아니라는 점에 조금 불안했는지 (알다시피 최고의 지원자는 구글에 다 잃었으니까요), 이 회사도 구글 면접 과정을 따라하기 시작했습니다.

At worst, algorithms interviews can turn into some weird hazing process. Sometimes companies get totally convinced that some random brainteaser is the secret sauce to finding great candidates, and you can never change their mind about it.

* 최악의 경우로 보면 알고리즘 면접은 기괴하고 못살게 구는 절차로 바뀌어 버립니다. 가끔 회사에서 무작위 난제를 내는 것이 위대한 지원자를 찾는 비밀 병기라고 단단히 착각해버려서 그런 생각을 바꾸는 일이 불가능한 경우가 있습니다.

Overall, I wouldn’t ask traditional hard algorithms questions in interviews. At worst, algorithms questions are extremely bad interview questions. I’ve written on occasion about questions I particularly hate. Algorithms questions are particularly bad when they’re more like brainteasers and require more leaps of insight. (If you want to build an algorithm interview process, feel free to email me and I can give you more detailed opinions on how to ask questions that don’t have these problems.)

* 종합해서 말하자면, 저는 이런 전통적이고 어려운 알고리즘 문제를 면접에서 안냈으면 합니다. 최악으로는 알고리즘 문제가 극단적으로 나쁜 면접 질문이 될겁니다. 제가 특히 싫어하는 질문은 이런 상황을 위해 따로 적어뒀습니다. 알고리즘 질문은 난제나 여러 통찰을 요구하는 경우에 특히 나쁜 질문입니다. (만약 알고리즘 면접 과정을 만들고 싶다면 언제든지 이메일을 보내시기 바랍니다. 이런 문제가 없는 질문을 하는 방법에 대해 더 자세한 의견을 드릴 수 있습니다.)


## How to study (어떻게 공부하나요?)

EDIT: I just read Haseeb Qureshi’s blog post about this again; he and I agree and I think he goes into slightly more detail. Read the “General Study Strategy” and “Programming Interview Study Guide” sections.

* 추가: Haseeb Qureshi의 블로그 포스트를 읽었는데 이 글에 동의합니다. 그리고 이 글이 좀 더 상세하다고 생각합니다. “일반 학습 전략”과 “프로그래밍 면접 공부 가이드” 부분을 읽으세요.

> 링크 : https://haseebq.com/how-to-break-into-tech-job-hunting-and-interviews/#general-study

I think there’s sort of two different skills involved in answering algorithms questions. Firstly, you need to know all of the classic algorithm and data structure material. Secondly, you need to be able to quickly produce algorithmic logic on a whiteboard under pressure. I’m going to discuss these topics separately.

* 저는 알고리즘 문제에 답하려면 두 가지 다른 기술이 필요로 하다고 생각합니다. 첫째로 모든 대표적 알고리즘과 자료 구조 문제를 알아야 합니다. 둘째로는 부담되는 상황에서 알고리즘 논리를 화이트보드에 빠르게 풀어나갈 수 있어야 합니다. 이 두 주제를 나눠서 얘기해보려고 합니다.

### Canonical algorithms material (표준 알고리즘 자료)

There’s a pretty consistent set of core algorithms knowledge to acquire that companies will test you on. Companies are incentivised not to ask questions relying on material which isn’t in this list, because many good programmers don’t know it and the companies will end up failing good people.

* 회사의 시험을 준비하는데 있어 습득해야 하는 거의 필수적인 핵심 알고리즘 모음이 있습니다. 회사에서는 이런 목록에 들어있지 않은 질문은 하지 않으려고 합니다. 좋은 프로그래머 다수가 이 목록에 없는 질문에 대해서는 답을 모르기 때문이며 그래서 목록 외 질문을 냈다가 회사는 좋은 사람을 뽑는데 실패하게 됩니다.

Here’s a list of the data structures material you should know:
알아야 할 자료구조는 다음과 같습니다.
+ List structures: arrays, dynamic arrays, linked lists
* list 구조: 배열, 동적 배열, 링크드 리스트(linked list)
+ Set and map structures: hash maps, binary search trees, heaps
* set과 map 구조: 해시맵, 이진 검색 트리, 힙

For each of the data structures, you should know how all of its essential methods are implemented and their runtimes. (Essential methods for lists are set, get, pushAtEnd, popAtEnd, insertByIndex, removeByIndex. Essential methods for sets are insert, remove, contains?). You should know how to write code that uses the implementation of the data structures; for example, you should be able to implement a getNearestElementTo(x) method which takes a binary search tree and searches for the closest value to x in the tree.

* 여기서 언급한 자료구조는 필수 메소드가 어떻게 구현되어 있는지, 런타임은 어떻게 동작하는지 알아야 합니다. (list의 필수 메소드는 set, get, pushAtEnd, popAtEnd, insertByIndex, removeByIndex, set의 필수 메소드는 insert, remove, contains? 입니다.) 자료구조 구현을 어떻게 사용하는지 알아야 합니다. 예를 들면 getNearestElementTo(x) 메소드를 구현할 수 있어야 합니다. 이 메소드 즉, x와 가장 가까운 값을 찾는 구현을 하려면 이진 검색트리를 알아야 합니다.

Other notes on this:
이 문제를 해결하는데 이런 내용을 알아야 합니다.
+ You should know that your binary search tree implementations need to have logic for balancing, but it’s okay if you don’t know the details. (Optional material: if you want to quickly learn an implementation of self balancing BSTs check out treaps. If you want to understand how red-black trees work, learn about left-leaning red-black trees or 2-3-4 trees instead.)
* 이진 검색트리 구현에 균형을 맞추는 코드가 필요하다는 점을 알아야 하지만 세부 내용은 몰라도 괜찮습니다. (선택 자료: 자기 균형 BST을 어떻게 구현하는지 빠르게 배우고 싶다면 이 트립을 참조하세요. 어떻게 레드블랙 트리가 동작하는지 이해하고 싶다면 좌편향 레드블랙 트리 또는 2-3-4 트리를 배우세요.)
+ You should probably know that a queue can be implemented with two stacks
* 큐를 스택 두 개로 구현할 수 있다는 점을 알아야 합니다.

You should be able to implement all of the following algorithms:
- 다음 알고리즘은 어떻게 구현하는지 알아야 합니다.
+ Graph algorithms: Breadth first search, depth first search, Dijkstra’s algorithm
* 그래프 알고리즘: 너비 우선 탐색(breadth first search), 깊이 우선 탐색(depth first search), 다익스트라 알고리즘 (dikstra’s algorithm)
+ One fast sorting algorithm; I recommend mergesort or quicksort
* 빠른 정렬 알고리즘 하나. 병합 정렬(mergesort) 또는 퀵 정렬(quicksort)
+ Binary search on an array. This one is super fiddly to get right and it’s worth writing the code even if you roughly understand the algorithm.
* 배열에서 수행하는 이진 검색. 이 알고리즘은 제대로 작성하기 매우 까다롭고 대략적으로 알고리즘을 이해하고 있더라도 코드로 작성해볼 가치가 있습니다.

You should be roughly comfortable with Big O notation.
* 그리고 Big O 표기법도 대충이라도 편하게 사용할 수 있어야 합니다.

How should you learn all this? My favorite resource is Skiena’s Algorithm Design Manual. All the above material is covered in chapters 2-6. I like it because the writing style is really engaging and it focuses on the parts of the material which I think are the most important. It’s available free on the internet here. One downside of this book is that it gives code examples in C, which makes it less accessible to programmers who can’t read C. If you read this book, I think it’s worth reading chapters 1-6 and chapter 12. This reading will cover quite a bit of material which is extremely unlikely to come up in interviews, but I think that the unnecessary material does a good job of reinforcing the really core stuff.

* 이 모든 내용을 어떻게 배워야 하나요? 제가 가장 좋아하는 자료는 Skiena의 Algorithm Design Manual입니다. 위에서 언급한 모든 내용을 챕터 2~6에서 다룹니다. 이 책을 좋아하는 이유는 저술 방식이 참여를 유도하고 각 부분에서 중요한 자료에 잘 초점을 맞추고 다루는데 이런 방식은 중요하다고 생각합니다. 이 책은 인터넷에서 무료로 찾을 수 있습니다. 이 책의 단점은 예제가 C로 작성되었다는 점인데 C를 읽지 못하는 개발자라면 접근성이 좋지 않습니다. 저는 챕터 1~6, 12는 꼭 읽어야 한다고 생각합니다. 이 부분은 인터뷰에서 나올 가능성이 극히 낮지만 필요 없다고 생각하는 부분이 진정 핵심적인 부분을 잘 보강한다고 생각하기 때문입니다.

If you want a briefer explanation, I like the explanations in Craking the Coding Interview and on InterviewCake.com. I think Skiena’s book is way better than the famous CLRS textbook, which is extremely dry and somewhat pedantic. I have a few notes on graph algorithms here that might be a useful resource.

* 이런 부분에 대략적인 설명을 보고 싶다면 Craking the Coding Interview와 InterviewCake.com의 설명이 좋습니다. 저는 Skiena의 책이 극단적일 정도로 건조하고 딱딱한 유명 CLRS 교재보다 낫다고 생각합니다. 그래프 알고리즘에 대한 글을 쓴 적이 있는데 참고가 되었으면 좋겠습니다.

### Canonical algorithmic skills (표준 알고리즘 기술)

So that’s the core knowledge required for interviews. Here are the different kinds of programming skills that are tested, together with my favorite resource for learning them. Cracking the Coding Interview is an extremely useful resource for all of this. I wrote some notes on it here.

* 여기까지 인터뷰에 핵심적으로 필요한 부분을 확인했습니다. 이제 다른 종류의 프로그래밍 기술로 무엇을 테스트하는지 확인하고 제가 선호하는 학습 자료도 함께 확인합니다. 이런 기술에 있어서는 Cracking the Coding Interview(이하 CtCI) 책이 가장 유용합니다. 이 책에 대해서 작성한 글입니다.

> 링크 : http://shlegeris.com/2016/06/22/ctci

Here are the most common central components of algorithms interview problems:
- 알고리즘 면접 문제 중 가장 일반적이고 중점적으로 다뤄지는 요소는 다음과 같습니다.
+ Dynamic programming: learn it by reading Skiena chapter 8, or by reading the Cracking the Coding Interview chapter on this topic.
* 동적 프로그래밍: Skiena 책의 챕터 8 또는 CtCI에서 이 주제의 챕터에서 학습합니다.
+ Recursion: Cracking the Coding Interview has a great chapter on this.
* 재귀: CtCI에 이 주제에 대한 멋진 챕터가 있습니다.
+ Iterating around and over the famous data structures. CtCI has good questions about this for each of the individual data structures. Eg for BSTs, you could refer to the CtCI tree chapter.
* 유명 자료 구조를 반복(iterating)하는 문제: CtCI에서 각각의 자료 구조를 다룰 때 이 문제도 함께 다룹니다. 예를 들어 BST에서는 CtCI 트리 챕터를 참고할 수 있습니다.
+ Composing fast data structures for a problem. Here are some examples of this kind of problem.
* 문제 해결을 위해 빠른 자료 구조를 조합하기: 이런 문제에 대한 예제는 이 글에서 확인할 수 있습니다.

My main advice is to do a bunch of problems from Cracking the Coding Interview. I listed in my notes on it the problems that I thought were most important.
Here’s a general note on studying these problems: I think it’s probably fine to “cheat” by reading the answers. I think you’re better off giving up on the problems and reading the solutions than giving up totally on doing practice interview problems.

* CtCI에서 살펴볼 수 있는 많은 문제를 살펴보는 방법이 제가 드리는 가장 주요한 조언입니다. 이 문제에서 가장 중요하다고 생각하는 부분은 위 목록과 같습니다.
이런 부류의 문제를 어떻게 학습하는지에 대해 일반적인 생각은 이렇습니다. 제 생각엔 답안을 “훔쳐보는” 일은 그래도 괜찮다고 생각합니다. 면접 문제 푸는 일을 내던지고 아예 포기하는 것보다는 문제 풀다가 막히면 해결책을 보는 방법이 차라리 나은 접근이기 때문입니다.

### Non-technical aspects of succeeding at algorithm interviews (알고리즘 면접에서 성공하기 위한 비기술적 측면)

It’s worth practicing answering these questions under pressure, with a real human asking them to you. For more advice on this, see the Triplebyte blog post, particularly points 2, 3, and 7.

* 이런 질문은 실제로 부담되는 환경에서 답하는 연습을 해야 합니다. 진짜 사람이 질문하는 상황에서 말이죠. 이 부분에 대해서는 Triplebyte의 블로그 포스트에서 다루고 있고 2, 3, 7번을 읽어보기 바랍니다.

### Learning further about algorithms and data structures (알고리즘과 자료구조에 대해 더 배우기)

Suppose you’re excited about algorithms and data structures for their own sake, as opposed to for the mercentile purpose of getting a job. How should you learn more?

* 취업 목적 학습을 넘어서 본인을 위해 즐겁게 알고리즘과 자료구조를 배우고 싶다고 가정해봅시다. 어떻게 더 배워야 할까요?

The easiest things to learn more about are relatively simple data structures that for whatever reason aren’t part of the core data structure canon I described above. For example, treaps, skip lists, augmented BSTs, and the Disjoint set data structure are all pretty easy to understand and I think they’re all really cool. There are some data structure topics which were harder to understand but well worth the effort. For example, this set of slides which explains BTrees and 2-3-4 trees.

* 가장 쉬운 방법은 위에서 필수로 배워야 한다고 한 핵심 자료 구조에 포함되지 않는 자료 구조 중 상대적으로 간단한 자료 구조를 학습하는 방법입니다. 트립, 스킵 목록, 증강 이진검색트리, 서로소 집합 자료구조가 그 예로 모두 쉽게 이해할 수 있는 편이며 모두 멋진 알고리즘입니다. 자료구조 주제 중 이해하기 어렵지만 노력해서 이해하면 좋은 주제도 있습니다. 예를 들어 이 슬라이드에서는 이진트리와 2-3-4 트리를 설명합니다.

For more interesting data structures material, my favorite resources are:
- 흥미로운 자료 구조를 배울 수 있는, 제가 좋아하는 자료는 다음과 같습니다.
+　Chapter 12 of Skiena, and other later chapters of Skiena
* Skiena의 챕터 12와 이후 챕터
+ CS166, an awesome Stanford class. The slides are awesome and quite readable. I also enjoyed doing some of the problem sets. For further data structures fun, I recommend the project ideas handout from the class.
* 스탠포드의 멋진 강의인 CS166. 이 강의의 슬라이드는 멋지고 읽기 좋은 편입니다. 저는 여기서 다룬 문제가 즐거웠습니다. 자료구조와 더 놀고 싶다면 이 프로젝트 아이디어 핸드아웃을 추천합니다.
> 링크 : http://web.stanford.edu/class/cs166/

+ I’m quite proud of the work I did here on a particular data structure problem which isn’t actually that hard but was fun as an amateur activity. My solution to that problem involves several ideas from advanced data structures and I think it’s quite neat.
* 저는 이런 작업처럼 그다지 어렵지 않은 자료 구조 문제를 아마추어 활동으로 재미삼아 한다는 점이 자랑스럽습니다. 이 문제를 풀기 위한 해결책으로 고급 자료구조에서 얻은 몇 아이디어를 적용했다는 점이 멋지지 않나 생각합니다.

### Resources
+ Triplebyte, How To Pass A Programming Interview. I endorse most of this content.
	1. Be enthusiastic
	2. Study common interview concepts
	3. Get help from your interviewer
	4. Talk about trade-offs
	5. Highlight results
	6. Use a dynamic language, but mention C
	7. Practice, practice, practice
	8. Mention credentials
	9. Line up offers
* Triplebyte의 프로그래밍 면접 통과 방법을 확인하세요. 저는 이 글 내용 대부분을 지지합니다.(링크 : https://triplebyte.com/blog/how-to-pass-a-programming-interview)
+ Steve Yegge: some old posts like this one, Get That Job At Google. I think a lot of his ideas are wrong, but they’re interesting to read.
	- Algorithm Complexity, Sorting, Hashtables, Trees, Graphs, Other data structures(NP-complete), Math!, Operating Systems, Other Stuff
* Steve Yegge의 글 5가지 필수 폰스크린 질문, 구글에 자리 얻기는 좀 오래된 글이기도 하고 많은 부분에 문제가 있다고 생각하지만 흥미롭게 읽을 수 있습니다.(링크 : http://steve-yegge.blogspot.com/2008/03/get-that-job-at-google.html)
+ InterviewCake.com is a more fun and engaging self-study tool than Cracking the Coding Interview.
* InterviewCake.com[https://www.interviewcake.com/]은 Cracking the Coding Interview보다 더 재미있는 자습 도구입니다.

### 기타
+ MIT algorithm 강좌

> https://www.youtube.com/watch?v=JPyuH4qXLZ0&list=PL8B24C31197EC371C

+ Stanford 강좌

> https://www.youtube.com/watch?v=yRM3sc57q0c&list=PLXFMmlk03Dt7Q0xr1PIAriY5623cKiH7V

+ 숙명여대 snow

> 망했음(https://www.snow.or.kr/field/infoandcommunication/algorithm/)

+ 포프TV(POCU)

> https://www.udemy.com/cpp-unmanaged-programming-by-pocu/learn/v4/t/lecture/12660726?start=0