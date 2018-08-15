---
layout : post
title : "Hashmap-studies #4"
date : 2018-08-14 10:00
tag : HashMap
comment : true
---

# How To Convert HashMap To ArrayList In Java?

<a href="https://javaconceptoftheday.com/how-to-convert-hashmap-to-arraylist-in-java/#comments" class="comments-link" itemprop="discussionURL">4</a>
<em>HashMap</em> and <em>ArrayList</em> are two most used data structures in java. Both classes inherit from different hierarchies. 
<em>HashMap</em> is inherited from <em>Map</em> interface which represents the data in the form of key-value pairs. <em>ArrayList</em> is inherited from <em>List</em> interface which arranges the data in the sequential manner. Conversion of <em>HashMap</em> to <em>ArrayList</em> has also become a regular question in the java interviews as there is no direct methods in <em>HashMap</em> which converts the <em>HashMap</em> to <em>ArrayList</em>. 

* In this post, we will see how to convert HashMap to ArrayList in java with examples.

<img class="aligncenter" src="https://javaconceptoftheday.com/wp-content/uploads/2016/01/HashMapToArrayList.png?x70034" alt="HashMap To ArrayList In Java" />

* As <em>HashMap</em> contains key-value pairs, there are three ways you can convert given <em>HashMap</em> to <em>ArrayList</em>. 
You can convert <em>HashMap</em> keys into <em>ArrayList</em> or you can convert <em>HashMap</em> values into <em>ArrayList</em> or you can convert key-value pairs into <em>ArrayList</em>. 

* Let's see these three methods in detail.

<strong>a) Conversion Of HashMap Keys Into ArrayList :</strong>

For this, we use <em>keySet()</em> method of <em>HashMap</em> which returns the <em>Set</em> containing all keys of the <em>HashMap</em>. And then we pass this <em>Set</em> while constructing the <em>ArrayList</em>.


//Creating a HashMap object
		
HashMap&lt;String, String&gt; map = new HashMap&lt;String, String&gt;();

//Getting Set of keys from HashMap
		
Set&lt;String&gt; keySet = map.keySet();
		
//Creating an ArrayList of keys by passing the keySet
		
ArrayList&lt;String&gt; listOfKeys = new ArrayList&lt;String&gt;(keySet);

<strong>b) Conversion Of HashMap Values Into ArrayList :</strong>

For this, we use <em>values()</em> method of <em>HashMap</em> which returns the <em>Collection</em> containing all values of the <em>HashMap</em>. Then we use this <em>Collection</em> to create the <em>ArrayList </em>of values.

//Creating a HashMap object
		
HashMap&lt;String, String&gt; map = new HashMap&lt;String, String&gt;();

//Getting Collection of values from HashMap
		
Collection&lt;String&gt; values = map.values();
		
//Creating an ArrayList of values
		
ArrayList&lt;String&gt; listOfValues = new ArrayList&lt;String&gt;(values);

<strong>c) Conversion Of HashMap&#8217;s Key-Value Pairs Into ArrayList :</strong>

For this, we use <em>entrySet()</em> method of <em>HashMap</em> which returns the <em>Set</em> of <em>Entry&lt;K, V&gt;</em> objects where each <em>Entry</em> object represents one key-value pair. We pass this <em>Set</em> to create the <em>ArrayList</em> of key-value pairs.


//Creating a HashMap object
		
HashMap&lt;String, String&gt; map = new HashMap&lt;String, String&gt;();

//Getting the Set of entries
		
Set&lt;Entry&lt;String, String&gt;&gt; entrySet = map.entrySet();
		
//Creating an ArrayList Of Entry objects
		
ArrayList&lt;Entry&lt;String, String&gt;&gt; listOfEntry = new ArrayList&lt;Entry&lt;String,String&gt;&gt;(entrySet);

Below example converts the <em>studentPerformanceMap</em> to <em>listOfKeys</em>, <em>listOfValues</em> and <em>listOfEntry</em>.

import java.util.ArrayList;
import java.util.Collection;
import java.util.HashMap;
import java.util.Map.Entry;
import java.util.Set;

public class HashMapToArrayListExample 
{   	
	public static void main(String[] args) 
	{
	    	HashMap<String, String> studentPerformanceMap = new HashMap<String, String>();
    		//Adding elements to HashMap
    		studentPerformanceMap.put("John Kevin", "Average");
    	studentPerformanceMap.put("Rakesh Sharma", "Good");
    	studentPerformanceMap.put("Prachi D", "Very Good");
    	studentPerformanceMap.put("Ivan Jose", "Very Bad");
    	studentPerformanceMap.put("Smith Jacob", "Very Good");
    	studentPerformanceMap.put("Anjali N", "Bad");
		//Getting Set of keys
		Set<String> keySet = studentPerformanceMap.keySet();
		//Creating an ArrayList of keys
		ArrayList<String> listOfKeys = new ArrayList<String>(keySet);
		System.out.println("ArrayList Of Keys :");
		for (String key : listOfKeys)
		{
			System.out.println(key);
		}
		System.out.println("--------------------------");
		//Getting Collection of values
		Collection<String> values = studentPerformanceMap.values();
		//Creating an ArrayList of values
		ArrayList<String> listOfValues = new ArrayList<String>(values);
		System.out.println("ArrayList Of Values :");
		for (String value : listOfValues)
		{
			System.out.println(value);
		}
		//Getting the Set of entries
		Set<Entry<String, String>> entrySet = studentPerformanceMap.entrySet();
		//Creating an ArrayList Of Entry objects
		ArrayList<Entry<String, String>> listOfEntry = new ArrayList<Entry<String,String>>(entrySet);
		System.out.println("ArrayList of Key-Values :");
		System.out.println("--------------------------");
		for (Entry<String, String> entry : listOfEntry)
		{
			System.out.println(entry.getKey()+" : "+entry.getValue());
		}
	}	
}


<strong>Output :</strong>
ArrayList Of Keys :<br /> Rakesh Sharma<br /> Anjali N<br /> Smith Jacob<br /> John Kevin<br /> Ivan Jose<br /> Prachi D<br /> &#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8211;<br /> ArrayList Of Values :<br /> Good<br /> Bad<br /> Very Good<br /> Average<br /> Very Bad<br /> Very Good<br /> &#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8211;<br /> ArrayList of Key-Values :<br /> Rakesh Sharma : Good<br /> Anjali N : Bad<br /> Smith Jacob : Very Good<br /> John Kevin : Average<br /> Ivan Jose : Very Bad<br /> Prachi D : Very Good

* <strong>References :</strong>

<ul><li><a href="https://docs.oracle.com/javase/8/docs/api/java/util/HashMap.html" target="_blank">HashMap</a></li><li><a href="https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html" target="_blank">ArrayList</a></li></ul>

