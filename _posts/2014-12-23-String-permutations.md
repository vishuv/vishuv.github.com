---
layout: post
title: Strings Permutations
tags: Interview geeksforgeeks
published: true
---

Today I have learnt computing the [permutations of a string](http://www.geeksforgeeks.org/write-a-c-program-to-print-all-permutations-of-a-given-string/).
Using recursion produces a succinct solution to this problem.
The intution is that you pick a character from the string and permute the rest of the string.
Permuataion of an empty string is empty string

Say our string is "ABCD"

Permutations are
{% highlight java %}
A + permute(BCD)

B + permute(ACD)

C + permute(ABD)

D + permute(ABC) 
{% endhighlight %}

Similarly to permute(BCD)
{% highlight java %}
B + permute(CD)

C + permute(BD)

D + permute(BC)
{% endhighlight %}

and so on.

Following is the java code implementing the algorithm

{% highlight java %}
public static ArrayList<String> permute(String input) {
	ArrayList<String> permutations = new ArrayList<>();
	_permute(permutations, "", input);
	return permutations;
}

private static void _permute(ArrayList<String> permutations, String prefix, String str) {
  // prefix holds the part of the string that is chosen so far like AB in AB + permute(CD)
	int n = str.length();
	if (n == 0)
		permutations.add(prefix);
	else
		for (int i = 0; i < n; i++) {
			_permute(permutations, prefix + str.charAt(i),
					str.substring(0, i) + str.substring(i + 1, n));
		}

}

{% endhighlight %}

The time complexity of this apporach is O(N * N!)
