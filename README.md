# Hashing-1
Explain your approach in **three sentences only** at top of your code
//TC: O(N);
//SC: O(N*K); //where k = length of each string

//Approach:
1. We will create a hash map in which the key represents the unique product of each character;
2. Anagrams will have the same product and hence they will be grouped together in a form of list in the value part of hashmap
3. We will return the values of the hashmap.

## Problem 1:
Given an array of strings, group anagrams together.

Example:
Input: ["eat", "tea", "tan", "ate", "nat", "bat"],
Output:
[
  ["ate","eat","tea"],
  ["nat","tan"],
  ["bat"]
]

class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        HashMap<Double, List<String>> map = new HashMap<>();

        for (String str : strs) {
            double hash = getHash(str);

            if (!map.containsKey(hash)) {
                map.put(hash, new ArrayList<>());
            }

            map.get(hash).add(str);
        }

        return new ArrayList<>(map.values());
    }

    private double getHash(String str) {
        int[] primes = {
            2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 
            41, 43, 47, 53, 59, 61, 67, 71, 73, 79, 83, 89, 97, 101
        };// each character is assigned a prime number because this is a fact that prime factorization is always unique

        double hash = 1;

        for (char c : str.toCharArray()) {
            hash *= primes[c - 'a'];
        }

        return hash;
    }
}


Note:
All inputs will be in lowercase.
The order of your output does not matter.

## Problem 2:
//TC: O(N);
//SC: O(N); 

//Approach:
1. We will create a hash map and we will map each character of s string to each character of t string
2. But if the mapping of character os s string already exists to a different character in t string then we will return false

Given two strings s and t, determine if they are isomorphic.
Two strings are isomorphic if the characters in s can be replaced to get t.
All occurrences of a character must be replaced with another character while preserving the order of characters. No two characters may map to the same character but a character may map to itself.

Example 1:
Input: s = "egg", t = "add"
Output: true

Example 2:
Input: s = "foo", t = "bar"
Output: false

Example 3:
Input: s = "paper", t = "title"
Output: true
Note:
You may assume both s and t have the same length.

class Solution {
    public boolean isIsomorphic(String s, String t) {
        HashMap<Character, Character> hm = new HashMap<>();
        HashSet<Character> mapped = new HashSet<>(); // To avoid reverse mapping collision

        for (int i = 0; i < s.length(); i++) {
            char c1 = s.charAt(i);
            char c2 = t.charAt(i);

            if (hm.containsKey(c1)) {
                if (hm.get(c1) != c2) return false;
            } else {
                if (mapped.contains(c2)) return false; 
                hm.put(c1, c2);
                mapped.add(c2);
            }
        }

        return true;
    }
}

## Problem 3:
//TC: O(N);
//SC: O(N); 

//Approach:
1. Split the string into words and check if the count matches the pattern length.
2. Create two hash maps to store character-to-word and word-to-character mappings.
3. Iterate through both, and return false if any existing mapping conflicts; otherwise, return true.

Given a pattern and a string str, find if str follows the same pattern.
Here follow means a full match, such that there is a bijection between a letter in pattern and a non-empty word in str.

Example 1:
Input: pattern = "abba", str = "dog cat cat dog"
Output: true

Example 2:
Input:pattern = "abba", str = "dog cat cat fish"
Output: false

Example 3:
Input: pattern = "aaaa", str = "dog cat cat dog"
Output: false

Example 4:
Input: pattern = "abba", str = "dog dog dog dog"
Output: false
Notes:
You may assume pattern contains only lowercase letters, and str contains lowercase letters that may be separated by a single space.
class Solution {

    public boolean wordPattern(String pattern, String str) {

        String[] arr = str.split(" ");
        int n = arr.length;
        int k = pattern.length();

        if (n != k) return false;

        HashMap<Character, String> pMap = new HashMap<>();
        HashMap<String, Character> sMap = new HashMap<>();

        for (int i = 0; i < n; i++) {

            char a = pattern.charAt(i);

            if (!pMap.containsKey(a)) {
                pMap.put(a, arr[i]);
            } else {
                if (!pMap.get(a).equals(arr[i])) return false;
            }

            if (!sMap.containsKey(arr[i])) {
                sMap.put(arr[i], a);
            } else {
                if (!sMap.get(arr[i]).equals(a)) return false;
            }
        }

        return true;
    }
}
