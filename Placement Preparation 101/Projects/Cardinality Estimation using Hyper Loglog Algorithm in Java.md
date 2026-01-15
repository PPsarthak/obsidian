#project #placement-preparation 
ref - 
https://www.youtube.com/watch?v=MOkm__T2jUg
https://www.youtube.com/watch?v=lJYufx0bfpw (This is the best video!!)
https://youtu.be/eV1haPUt0NU?si=Cr9gC6BTrICnWisS (Gaurav Sen)

**Basic Intro**
Think of counting the number of unique visitors to a website? 
The problem is simple and can be solved using a data structure like - hashmap or hashset
But problem comes with scalability, when you have billions of data points, O(n) can become costlier

So, let's approach the constraints of the problem? 
We have a very large dataset, perhaps billions, then do we really need a really accurate count (till thousands/hundreds place should be sufficient)? 
If the answer to this is YES, then you can work with Hyper Loglog algorithm

**Rough Steps**
- Convert the unique key (say IP address) into its binary form
- Now look at the number of zeros at the end of each binary number
- ![[HyperLogLog Example 1.png|175]]
- Here we see the maximum number of trailing zeros = 2 (so, n=2)
- Now, think of this - how many binary number do we need to go over to get this number ? - 2^(n=2)
- ![[HyperLogLog Example 2.png|175]]
- So if you have 'n' zeros at the end, you must have gone over 2^n numbers to get n-zeros at the end right?
- So you are very much confident to say that you have ==atleast== 2^n users - which can be a good guess
- So there is a big room of error and we know why. Here it is -
- ![[HyperLogLog Example 3.png|275]]
- 2^n graph is a exponential graph, so there is a steep curve and hence there is great chance to miss the correct answer because of the nature of equation
- So, let's try to mitigate this - let's see what happens when bring hashing into the picture
- Let's create 'm' buckets (m is arbitrary, here it is 4) and take first few bits(here first 2 bits are taken) and use them to perform hashing
> Remember, more the buckets, more closer to accurate, but more resources as well!
- ![[HyperLogLog Example 4.png|200]]
- So the first 2 bits are used to determine the bucket index, and in that bucket that number will be hashed. This is fair enough way to hash
- Now for each bucket we will repeat the above process - get the maximum number of trailing zeros in each bucket and take the mean of all buckets
- ![[HyperLogLog Example 5.png|250]]
- So, now we would have got 2^a (where a is our arithmetic mean, here 1) as our answer
- Now clearly there is deviation from the answer and the research we referred for our project, experimentally came up with this approximation to determine unique entries
- ![[HyperLogLog Example 6.png|250]]
- We will multiply our answer with a constant β = number of buckets (m) x 0.79 (experimentally taken from trail and error)

*This was the Loglog algorithm*
The problem with this is that it is sensitive to outliers - when 1 bucket has a very high number of trailing zeros while the rest are much smaller. And the reason for this sensitivity is using AM which is sensitive to outliers

*In Hyper Loglog HM is used instead of AM to account for such outliers*