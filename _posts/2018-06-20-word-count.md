---
layout: post
title:  "wordCounter"
date:   2018-06-20
excerpt: "wordCounter"
project: true
tag:
- wordCounter 
- 글자세기
comments: true
---

<center><b>wordCounter</b> is a basic-programming, one small application.</center>
     
This is my Java programming practice. There are many "Counting words in different programming languages" but it is in different coding style for counting words by Java language.
    
## 여러가지 방법으로 코딩해보기

#### Lambda Function coding practice
#### Class & Interface coding practice


    import java.io.PrintStream;
    import java.util.List;
    import java.util.Map;
    import java.util.Map.Entry;
    / **
     * OBJECTIVE - to understand the interface of java.util.Map and its implementations <br/>
     * Optional: sort out the API for sorting <br/>
     * Optional: understand the use of regular expressions in Java <br/>
     *
     * ASSIGNMENT <br/>
     * Calculate the number of occurrences of words in the text without regard to the case of characters. <br/>
     *
     * REQUIREMENTS <br/>
     * A word is any sequence of characters allocated by an arbitrary
     * the number of spaces, tab characters, and line transfers. <br/>
     * It is necessary to calculate the number of occurrences of words in the text without regard for the case of characters
     * (the words 'Program' and 'program' are considered the same word!). <br/>
     * ADDITIONALLY it is possible to arrange the counting results in the order
     * decrease in the number of occurrences of a word,
     * as well as exclusion from consideration of the words enclosed inside & lt; & gt ;. <br/>
     *
     * /
    public interface WordCounter
        {
        void setText (String text);

        String getText ();

        Map<String, Long> getWordCounts ();

        List<Map.Entry<String, Long>> getWordCountsSorted ();

        List<Map.Entry<String, Long>> sortWordCounts (Map<String, Long> orig);

        void printWordCounts (PrintStream ps);

        void printWordCountsSorted (PrintStream ps);
        }

    import java.io.PrintStream;
    import java.util.*;

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
    public void printWordCounts(PrintStream ps) {
        if (this.text == null)
            throw new IllegalStateException();

        Map<String, Long> dict = this.getWordCounts();
        for (Map.Entry<String, Long> entry : dict.entrySet()) {
            ps.format("%s %d\n", entry.getKey(), entry.getValue());
        }
    }

    @Override
    public void printWordCountsSorted(PrintStream ps) {
        if (this.text == null)
            throw new IllegalStateException();

        List<Map.Entry<String, Long>> entries = this.sortWordCounts(this.getWordCounts());

        for (Map.Entry<String, Long> entry : entries) {
            ps.format("%s %d\n", entry.getKey(), entry.getValue());
        }
    }
    }
