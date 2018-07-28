---
layout: post
title:  "Counting-words"
date:   2018-06-20 7:30:00 +0900
categories: coding
tag:
- java
comments : true
---

# 여러가지 방법으로 코딩해보기

 Lambda Function coding practice

 Class & Interface coding practice

## coding

interface

```java

    import java.util.List;
    import java.util.Map;
    import java.util.Map.Entry;

    public interface WordCounter{

        void setText (String text);

        String getText ();

        Map<String, Long> getWordCounts ();

        List<Map.Entry<String, Long>> getWordCountsSorted ();

        List<Map.Entry<String, Long>> sortWordCounts (Map<String, Long> orig);

        void printWordCounts ();

        void printWordCountsSorted ();
    }

---------------------------------------------------------------------------

import java.io.PrintStream;
import java.util.Map;

public class WordCounterImpl implements WordCounter {
   private String text = null;
   private Map<String, Long> dict = null;

    @Override
    public void setText(String text) {
        this.text = text;
    }

    @Override
    public String getText() {
        return this.text;
    }

    @Override
    public Map<String, Long> getWordCounts() {
        if (this.text == null)
            throw new IllegalStateException();

        if (this.dict == null)
            this.dict = this.countWords(this.text);

        return new HashMap<String, Long>(this.dict);
    }

    private Map<String, Long> countWords(String text) {
        Map<String, Long> dict = new HashMap<String, Long>();

        for (String word : text.split("[ \\t\\n]+")) {
            word = word.toLowerCase();
            if (word.matches("<.*>") || word.length() == 0)
                continue;

            Long counter = dict.get(word);
            if (counter == null)
                counter = new Long(1);
            else
                counter = counter + 1;

            dict.put(word, counter);
        }

        return dict;
    }

    @Override
    public List<Map.Entry<String, Long>> getWordCountsSorted() {
        if (this.text == null)
            throw new IllegalStateException();

        return this.sortWordCounts(this.getWordCounts());
    }

    @Override
    public List<Map.Entry<String, Long>> sortWordCounts(Map<String, Long> orig) {
        List<Map.Entry<String, Long>> newList = new ArrayList<Map.Entry<String, Long>>();

        for (Map.Entry<String, Long> entry : orig.entrySet()) {
            newList.add(new AbstractMap.SimpleEntry<String, Long>(entry));
        }

        Collections.sort(newList, new Comparator<Map.Entry<String, Long>>() {
            @Override
            public int compare(Map.Entry<String, Long> o1, Map.Entry<String, Long> o2) {
                if (o2.getValue() > o1.getValue())
                    return 1;
                else if (o2.getValue() < o1.getValue())
                    return -1;
                else
                    return o1.getKey().compareTo(o2.getKey());
            }
        });

        return newList;
    }

    @Override
    public void printWordCounts() {
        if (this.text == null)
            throw new IllegalStateException();

        Map<String, Long> dict = this.getWordCounts();
        for (Map.Entry<String, Long> entry : dict.entrySet()) {
            System.out.printf("%s %d\n", entry.getKey(), entry.getValue());
        }
    }

    @Override
    public void printWordCountsSorted() {
        if (this.text == null)
            throw new IllegalStateException();

        List<Map.Entry<String, Long>> entries = this.sortWordCounts(this.getWordCounts());

        for (Map.Entry<String, Long> entry : entries) {
            System.out.printf("%s %d\n", entry.getKey(), entry.getValue());
        }
    }

    public stataic void main(String[] args){
        WordCounterImpl wordCounter = new WordCounterImpl();
        Scanner scan = new Scanner(System.in);
        String text = scan.nextLine();

        wordCounter.setText(text);
        wordCounter.printWordCounts();
        wordCounter.printWordCountsSorted();

    }
}

```
