
<h1>lecture 1:</h1>

In this lecture we are trying to find the solutions of the tractable problems

We will see various algorithms to find the n^{th} fibonacci number F_{n} from the naive to the more optimized ones

1. Defintion of fibonacci:

  The Fibonacci numbers are a sequence F_{n} of integers in which every number after the first two, 0 and 1, is the sum of the two preceding numbers: 0,1,1,2,3,5,8,13,21,... 

                F_{n-1}+F_{n-2} if n>1
      F_{n} =          1        if n=1
                       0        if n=0

    In this problem the input of the program is n and the output is F_{n}

2. 1: Recursive approach for solving
    <pre>
    fib(n):
       if n<=1 : return n
       else return F_{n-1}+F_{n-2}
    </pre>
    This algorithm is straight forward and we can say that it is correct according to the definition. Now if we talk about the time complexity

    T(n)<=2  for n<=1 because the values of F_{0} and F_{1} are pre-defined by definition.
                                 
    if n>1 then we can write that   T(n)=T(n-1)+T(n-2)+3   where 3 is the additional workdonne outside the recursion.
    If we see the above equation and compare it with the definition of the fibonacci there is an extra constant 3 so we can definitely say that T(n) >= F_{n}
    Also we know the fact that n^{th} fibonacci number has approximate value of ![eight](https://latex.codecogs.com/gif.latex?2%5E%7B0.694n%7D) and number of digits would be ome constant times n (say c*n, number of digits in the number is equal to mantissa of ![seven](https://latex.codecogs.com/gif.latex?log_%7B10%7D%5E%7B2%5E%7B0.694n%7D%7D%3D%200.694*log_%7B10%7D%7B2%7D*n%20%3D%20c*n))
     
    Hence, T(n) >= 2^{0.694n}  so the Time complexity grows exponentially......

    We can do better than this algorithm because in the recurrence relation F_{n}=F_{n-1}+F_{n-2} we are calculating same values multiple times, Example: F_{n}=F_{n-1}+F_{n-2} next F_{n-1}=F_{n-2}+F_{n-3} and F_{n-2}=F_{n-3}+F_{n-4} that is we are calculating F_{n-2},F_{n-3} two times which is clearly a waste of time. So, next approach is to avoid this

3. 2: Iterative approch for solving
   <pre>
   fib2(n):
     array f[n+1];
     f(0)=0, f[1]=1
     for i in range(2,n+1):
       f[n]=f[n-1]+f[n-2];
     return f[n];
    </pre>
  This approach uses memoization that is storing the already computed values in a array and using them just by refering to index
  It appears to be an algorithm with linear time complexity but actually it is not. We found that the number of digits in n^{th} fibonacci number is c*n where c is a constant.

        So, each addition (F_{n-1}+F_{n-2}) takes time of O(n) so the total time comlexity = O(n^{2})

  The next step is to think whether can we achieve better time complexity than this.

4. 3: Using matrix matrix multiplications

   There is a interesting relationship between the fibonacci numbers using matrices.

     ![nine](https://latex.codecogs.com/gif.latex?%5Cbegin%7Bequation*%7D%20%5Cbegin%7Bbmatrix%7D%20F_%7B1%7D%5C%5CF_%7B2%7D%20%5Cend%7Bbmatrix%7D%3D%20%5Cbegin%7Bbmatrix%7D%200%20%26%201%5C%5C1%20%261%20%5Cend%7Bbmatrix%7D%20%5Cbegin%7Bbmatrix%7D%20F_%7B0%7D%5C%5C%20F_%7B1%7D%20%5Cend%7Bbmatrix%7D%20%5Cend%7Bequation*%7D)

     We can use this and get the equation which maps n, n+1 fibonacci numbers to F_{0},F_{1} using a matrix

     ![ten](https://latex.codecogs.com/gif.latex?%5Cbegin%7Bequation*%7D%20%5Cbegin%7Bbmatrix%7D%20F_%7Bn%7D%5C%5CF_%7Bn&plus;1%7D%20%5Cend%7Bbmatrix%7D%3D%20%5Cbegin%7Bbmatrix%7D%200%20%26%201%5C%5C1%20%261%20%5Cend%7Bbmatrix%7D%5E%7Bn%7D%20%5Cbegin%7Bbmatrix%7D%20F_%7B0%7D%5C%5C%20F_%7B1%7D%20%5Cend%7Bbmatrix%7D%20%5Cend%7Bequation*%7D)

   In this algorithm time complexity depends on the time taken for for mutiplying two n-bit integers so the time complexity is given by O(M(n)logn) where we can calculate the constant matrix to the power of n by using binary exponentiation approach
  <pre>
   binpow(a, b):
    if (b == 0)
        return 1;
    res = binpow(a, b / 2);
    if (b % 2)
        return res * res * a;
    else
        return res * res;
    </pre>
5. Algorithm for multiplying two n-bit integers(Karatsuba Algorithm)
   
   The naive approach for multiplying is taking one-bit of an integer and multiplying it with all n bits of another integer(0*0=0*1=1*0=0 and 1*1=1) so a total of order of n^{2} single digit multiplications, additions and shift operations will be performed.

   Therefore time complexity of this naive approach = O(n^{2})
   Now, we have to check whether we can do better than this or not.

   If we see the multiplication of two complex numbers a+ib, c+id
     (a+ib)*(c+id) = (ac-bd)+i(bc+ad)
   That is we have to perform four multiplications. But we can do better than this by performing only 3 multiplications
   To find (bc+ad) we can compute ac, bd, (a+b)(c+d) and do (a+b)(c+d)-ac-bd. Here we are performing only 3 multiplications
   So, if we want to do the multiplications of two n-bit integers, we can do this way
   1. Add two n/2 integers
   2. Multiply three n/2 integers
   3. Add, subtract, and shift n/2 digit integers to obtain result

   ![first eqn](https://latex.codecogs.com/gif.latex?x%20%3D%202%5E%7B%5Cfrac%7Bn%7D%7B2%7D%7Dx_%7B1%7D%20&plus;%20x_%7B0%7D)

   ![one](https://latex.codecogs.com/gif.latex?y%20%3D%202%5E%7B%5Cfrac%7Bn%7D%7B2%7D%7Dy_%7B1%7D%20&plus;%20y_%7B0%7D)

   ![two](https://latex.codecogs.com/gif.latex?x.y%20%3D%20%282%5E%7B%5Cfrac%7Bn%7D%7B2%7D%7Dx_%7B1%7D%20&plus;%20x_%7B0%7D%29.%282%5E%7B%5Cfrac%7Bn%7D%7B2%7D%7Dy_%7B1%7D%20&plus;%20y_%7B0%7D%29)

   ![three](https://latex.codecogs.com/gif.latex?%3D%202%5E%7Bn%7D.x_%7B1%7D.y_%7B1%7D%20&plus;%202%5E%7B%5Cfrac%7Bn%7D%7B2%7D%7D.%28%28x_%7B0%7D&plus;x_%7B1%7D%29.%28y_%7B0%7D&plus;y_%7B1%7D%29-%28x_%7B1%7D.y_%7B1%7D&plus;x_%7B0%7D.y_%7B0%7D%29%29&plus;x_%7B0%7D.y_%7B0%7D)

   So the equation for time complexity can be written as

   ![four](https://latex.codecogs.com/gif.latex?T%28n%29%3DT%28%5Cfrac%7Bn%7D%7B2%7D%29&plus;T%28%5Cfrac%7Bn%7D%7B2%7D%29&plus;T%28%5Cfrac%7Bn%7D%7B2%7D%20&plus;%201%29%20&plus;%5Ctheta%28n%29)

Using master theorem 3rd case we get ![five](https://latex.codecogs.com/gif.latex?O%28n%5E%7Blog_2%7B3%7D%7D%29) Therefore it is equal to ![six](https://latex.codecogs.com/gif.latex?O%28n%5E%7B1.585%7D%29)




Lecture2:

This lecture is about the algorithms in which divide and conquer method is used.
The algorithms and theorems discussed in class are:
   1. MergeSort
   2. Strassen’s Matrix Multiplication​
   3. Master Theorem​
   4. Find the median

1. Master Theorem:
          
   Master theorem is a theorem which we use to solve the recurrence relations of the form

      ![a](https://latex.codecogs.com/gif.latex?T%28n%29%20%3D%20a*T%28n/b%29&plus;O%28n%5E%7Bd%7D%29)  and the solution is shown as below                    -->(1)

Here the meaning is: cost of the work done for the problem of size n is equal cost of workdone of (a) number of subproblems of size n/b(assuming n/b as integer otherwise take a ceil of it) plus the additional workdone outside the recursive call which includes the cost of dividing the problem and merging.


<hr>          O(![b](https://latex.codecogs.com/gif.latex?n%5E%7Bd%7D)) if d>![c](https://latex.codecogs.com/gif.latex?log_%7Bb%7D%5E%7Ba%7D)

   T(n)=  O(![d](https://latex.codecogs.com/gif.latex?n%5E%7Bd%7D)logn) if d=![e](https://latex.codecogs.com/gif.latex?log_%7Bb%7D%5E%7Ba%7D)

<hr>          O(![f](https://latex.codecogs.com/gif.latex?n%5E%7Blog_%7Bb%7D%5E%7Ba%7D%7D)) if d<![g](https://latex.codecogs.com/gif.latex?log_%7Bb%7D%5E%7Ba%7D)
         

   If we draw the tree generated by the recurrence relation whose depth is ![h](https://latex.codecogs.com/gif.latex?log_%7Bb%7D%5E%7Ba%7D) and branching factor a.
   If we check the tree at the k^{th} level there will be a^{k} nodes and each is a subproblem of size ![i](https://latex.codecogs.com/gif.latex?%5Cfrac%7Ba%7D%7Bb%5E%7Bk%7D%7D) so

             workdone at the k^{th} level is ![j](https://latex.codecogs.com/gif.latex?O%28%5Cfrac%7Ba%7D%7Bb%5E%7Bk%7D%7D%29%5E%7Bd%7D*a%5E%7Bk%7D)

(Explanation: There are a^{k} nodes and each costs a constant time O(n^{d}) but here n is ![k](https://latex.codecogs.com/gif.latex?%5Cfrac%7Ba%7D%7Bb%5E%7Bk%7D%7D) at kth level,see equation 1)
 
 Therefore workdone at the k^{th} level can also be written as ![l](https://latex.codecogs.com/gif.latex?O%28n%5E%7Bd%7D%29.%28%5Cfrac%7Ba%7D%7Bb%5E%7Bd%7D%7D%29%5E%7Bk%7D)
 It is summation of geometric series with ratio ![o](https://latex.codecogs.com/gif.latex?%5Cfrac%7Ba%7D%7Bb%5E%7Bd%7D%7D) and first term n^{d}

 So we have three cases when the ratio ![n](https://latex.codecogs.com/gif.latex?%5Cfrac%7Ba%7D%7Bb%5E%7Bd%7D%7D) is 

 a) greater than 1, then the series is increasing and its sum is given by its last term ![p](https://latex.codecogs.com/gif.latex?O%28n%5E%7Blog_b%5E%7Ba%7D%7D%29)

![m](https://latex.codecogs.com/gif.latex?n%5E%7Bd%7Dn%5E%7Blog_b%5E%7Ba%7D%7D%20%3D%20n%5E%7Bd%7D%28%5Cfrac%7Ba%7D%7Bb%5E%7Bd%7D%7D%29%5E%7Blog_b%5E%7Bn%7D%7D%20%3D%20n%5E%7Bd%7D%28%5Cfrac%7Ba%5E%7Blog_b%5E%7Bn%7D%7D%7D%7Bb%5E%7Bd*log_b%5E%7Bn%7D%7D%7D%29%20%3D%20a%5E%7Blog_b%5E%7Bn%7D%7D%3D%20n%5E%7Blog_b%5E%7Ba%7D%7D)

b) less than 1, then the series is decreasing and its sum is given by the first term 1[n](https://latex.codecogs.com/gif.latex?O%28n%5E%7Bd%7D%29)

c) equal to 1, in this case all O(logn) terms of the series are equal to n^{d}.Hence cost of work done = ![q](https://latex.codecogs.com/gif.latex?O%28n%5E%7Bd%7Dlogn%29)

There are several applications of the master theorem in different algorithms which uses divide and conquer method.It helps us to calculate the time complexity in simple and quick way.

2. Mergesort Algorithm:

   It is a O(nlogn) comparision sort
   In this algorithm we split the list into two equal halves, recursively sort each half and merge the two sorted sub-lists.
   Merge sort is one of the most efficient sorting algorithms. It works on the principle of Divide and Conquer. Merge sort repeatedly breaks down a list into several sublists until each sublist consists of a single element and merging those sublists in a manner that results into a sorted list.

<pre>
   mergesort(array a[],1, n):
       return merge(mergesort(array a[],0, n/2), mergesort(array a[],n/2+1, n))
       else return a

   merge(x[1,2...k], y[1,2,..l]):
       if k=0: return x[1,2,...k]
       if l=0: return y[1,2,...l]
       if(x[1]<=y[1])  return x[1] o merge(x[2..k],y[1,2,..l])  (here 'o' means concatenaion)
       else return x[1] o merge(x[2..k],y[1,2,..l])
</pre>
  Time complexity eqn is give by T(n)=2T(n/2)+O(n) because outside the recursive call(in merge) we doing k+l operations which is of order n.
  Using the master theorem second case we get, Time complexity = O(nlogn)    
     

   function iterative approch:
 
   We insert all the elements of the array as singleton arrays first in the queue ,then remove two arrays, merge two and insert back again till we get only one array in the queue.

<pre>
    Q=[] (first there is a empty queue)
    for i=1 to n:
        inject(Q,[a[i]])    (injecting as singleton arrays)
    which |Q|>1                        
        inject(Q,merge(eject(Q),eject(Q)))   (ejecting two arrays and combining and injecting again until we get one)
    return eject(Q)    
</pre>

   Comaprision based sorting:

   With n numbers we can have n! number of permutations and out of those one permuation is the required ascending or descending order permutation.

   For each (unique) input of length n, there is some permutation that gives us the sorted order
   
   for eg, for a = [6, 5, 30] the correct permutation is ['2', '1', '3'] (using quotes for 1-based indexing)

   Then, there are n! possible answers, and using k comparisons (which are booleans, true or false),we managed to uniquely identify, or index all n! permutations

  So with k booleans, we indexed n! things, which means k must be log(n!) which happens to be Ω(n logn)

3. Strassen's Matrix Mutiplication:
   
   To multiply two n cross n matrices we can write divide the matrix into 4, n/2 cross n/2 size sub-matrices and can write the original matrix as
![thir](https://latex.codecogs.com/gif.latex?%5Cbegin%7Bequation%7D%20X%3D%20%5Cbegin%7Bbmatrix%7D%20A%20%26%20B%5C%5CC%20%26%20D%20%5Cend%7Bbmatrix%7D%20%5Cend%7Bequation%7D)

<pre>    Here A, B, C, D are of n/2 cross n/2 size sub-matrices 
Multiplication of two matrices X,Y are given by </pre>

![foutt](https://latex.codecogs.com/gif.latex?%5Cbegin%7Bequation%7D%20X.Y%3D%20%5Cbegin%7Bbmatrix%7D%20A%20%26%20B%5C%5CC%20%26%20D%20%5Cend%7Bbmatrix%7D%20%5Cbegin%7Bbmatrix%7D%20E%20%26%20F%5C%5CG%20%26%20H%20%5Cend%7Bbmatrix%7D%3D%20%5Cbegin%7Bbmatrix%7D%20AE&plus;BG%20%26%20AF&plus;BH%5C%5CCE&plus;DG%20%26%20CF&plus;DH%20%5Cend%7Bbmatrix%7D%20%5Cend%7Bequation%7D)

There are eight n/2-sized products: AE,BG,AF,BH,CE,DG,CF and DH 

Time complexity of this is given by T(n)=8T(n/2)+O(n^2) where O(n^2) comes from the addition of numbers.

Using master theorem third case we get ![fift](https://latex.codecogs.com/gif.latex?T%28n%29%3DO%28n%5E%7Blog_2%7B8%7D%7D%29)

We can do better than this by the following approach

![sixt](https://latex.codecogs.com/gif.latex?%5Cbegin%7Bequation%7D%20X.Y%3D%20%5Cbegin%7Bbmatrix%7D%20P_%7B5%7D&plus;P_%7B4%7D-P%7B2%7D&plus;P_%7B6%7D%20%26%20P_%7B1%7D&plus;P_%7B2%7D%20%5C%5C%20P_%7B3%7D&plus;P_%7B4%7D%20%26%20P_%7B1%7D&plus;P_%7B5%7D-P_%7B3%7D-P_%7B7%7D%20%5Cend%7Bbmatrix%7D%20%5Cend%7Bequation%7D)

where P1 = A(F-H) , P2=(A+B)H , P3= (C+D)E, P4=D(G-E), P5=(A+D)(E+H) P6=(B-D)(G+H) P7=(A-C)(E+F)
    

4. Find the median:

This is a linear search algorithm..
One method is to sort the entire array and obtain the middle one. This algorithm takes O(nlogn) time

Median-finding algorithms use a divide and conquer approach to efficiently and quickly compute the ith smallest number in an unsorted array of size n, where i is an integer between 1 and n. 

If we can find ith smallest number, we can also find median by setting i=n/2. So, lets solve this problem

Divide the array into subarrays each of length five (if less than five elements are available for the last subarray, it is fine as it is a constant).

Sort each subarray and find the median. Sorting very small arrays takes linear time since these subarrays have five elements, and this takes O(n) time.

Determine the median of the set of all the medians by using recursive approach.Use this number obtained as the pivot element. The pivot here  is an approximate median of the array

Write Array such that all elements less than pivot are to the left of pivot, and all elements of Array that are greater than pivot are to the right.  The elements are not in sorted order once after placing them on either side of x. Example, if pivot is 3 the array the right of pivot maybe of this type [7,4,5,12] . This takes linear time O(n) since comparisons are between elements of array and pivot.

recursive algorithm is like:

<pre>
              Select(S_l,k)                if k<|S_l|  where S_l is the list of elements less than x
Select(S,k) = x                            if |S_l|<k<=|S_l|+|S_v|  where S_l is the list of elements equal to x
              SElect(S_r,k-|S_l|-|S_v|)    if k>|S_l|+|S_v|  where S_v is the list of elements greater than x
</pre>
   



