---
layout: post
title: Finding minimum window containing all characters of a query in a given string
tags: Interview geeksforgeeks strings algorithms
published: false
---


Following is the java code implementing the algorithm

{% highlight java %}
public static String minWindow(String input, String query){
	Map<Character, Integer> queryCounts = new HashMap<>();
	//load query character counts
	//these are the character counts to be found in the input string
	for(int i=0;i<query.length();i++){
		Character c = query.charAt(i);
		if(queryCounts.containsKey(c))
			queryCounts.put(c, queryCounts.get(c)+1);
		else
			queryCounts.put(c, 1);
	}
	int startIndex = 0;
	int endIndex = 0;
	int queryCharsFound=0;
	int totalQueryChars = query.length();
	int minLength = input.length() + 1;
	int minStartIndex = -1;
	
	for(endIndex=0;endIndex<input.length();endIndex++){
		Character c = input.charAt(endIndex);
		//skip characters that are not in the query
		if(!queryCounts.containsKey(c))
			continue;
		int temp = queryCounts.get(c);
		//decrease the character count 
		//the character count could be negative when our window has 
		//more no of this character than the number of ones present 
		//in the actual query
		queryCounts.put(c, temp-1);
		
		//If the query counts are positive we have NOT found any redundant
		//just the no of ones found in query
		if(temp-1>=0)
			queryCharsFound++;
		
		if(queryCharsFound==totalQueryChars){
			//We have a window now, lets try to compact the window
			Character ch = input.charAt(startIndex);
			Integer charCount = queryCounts.get(ch);
			while( charCount==null || charCount<0){
				//since we removed unwanted extra characters increment the count
				if(charCount!=null)
					queryCounts.put(ch, charCount+1);
				startIndex++;
				ch = input.charAt(startIndex);
				charCount = queryCounts.get(ch);
			}
			//Check if this window is the minimal window so far
			if(minLength > endIndex-startIndex+1){
				minStartIndex = startIndex;
				minLength = endIndex - startIndex + 1;
			}
			//go to the next window
			queryCharsFound--;
			startIndex++;
			queryCounts.put(ch, queryCounts.get(ch)+1);
		}
	}
	System.out.println(minStartIndex +","+ endIndex);
	return minStartIndex == -1 ? "" : input.substring(minStartIndex, minStartIndex + minLength);
}
{% endhighlight %}

The time complexity of this apporach is O(N * N!)
