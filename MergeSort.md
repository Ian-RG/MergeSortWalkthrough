# **Merge Sort**

Merge sort is a sorting algorithm which employs a divide and conquer strategy to organise a list of values.  

Merge sort is also an example of a recursive function, meaning it makes calls to itself. This results in a “nesting” of one or more instances of itself inside the first call to the function. More information on recursion can be found [here](https://www.khanacademy.org/computing/computer-science/algorithms/recursive-algorithms/a/recursion)   

The first part of this article will go through the divide steps, then the conquer steps with illustrated examples.  

Part two will walk through implementing the algorithm with a pseudocode example. 

# **Part One: Concepts**
## Divide  
First, let’s look at the divide aspect of merge sort.  

Let’s start with the following array, which is to be sorted:  
![Array tree image 1](https://raw.githubusercontent.com/Ian-RG/MergeSortWalkthrough/master/Array_1.png)  

The first step of merge sort is to split the array in half, and move the left half into its own array:  
![Array tree image 2](https://raw.githubusercontent.com/Ian-RG/MergeSortWalkthrough/master/Array_2.png)  

This step is repeated on the new left array, until we reach array size one as shown:  
![Array tree image 3](https://raw.githubusercontent.com/Ian-RG/MergeSortWalkthrough/master/Array_3.png)  

When the left branch is exhausted, we move back up one level and go right:  
![Array tree image 4](https://raw.githubusercontent.com/Ian-RG/MergeSortWalkthrough/master/Array_4.png)  

Following this pattern of going left whenever we can, and going right when left moves are exhausted we end up with the following:  
![Array tree image 5](https://raw.githubusercontent.com/Ian-RG/MergeSortWalkthrough/master/Array_5.png)    

## Conquer  
The conquer aspect works as follows:  

We take each pair of size 1 arrays at the bottom of the tree and compare their values:  
![Array tree image 6](https://raw.githubusercontent.com/Ian-RG/MergeSortWalkthrough/master/Array_6.png)  

First, the lesser value is inserted into the array above it, followed by the larger value. This “merges” the two smaller arrays into one larger array.  
![Array tree image 7](https://raw.githubusercontent.com/Ian-RG/MergeSortWalkthrough/master/Array_7.png)  

Lets move up to the next level in the tree. Here we’re just performing the same steps as before, ensuring we’re comparing the two lowest values in each array and inserting the lesser value into the array above. First we compare 1 and 6. 1 is less than 6, so it is inserted.  
![Array tree image 8](https://raw.githubusercontent.com/Ian-RG/MergeSortWalkthrough/master/Array_8.png)  

Then we compare 5 and 6. 5 is less than 6, so 5 is inserted.  
![Array tree image 9](https://raw.githubusercontent.com/Ian-RG/MergeSortWalkthrough/master/Array_9.png)  

Our left array is now depleted, so we can simply insert all values in our right array in order.  
![Array tree image 10](https://raw.githubusercontent.com/Ian-RG/MergeSortWalkthrough/master/Array_10.png)

Following this pattern all the way back up the tree, we finally reach a level where all values are back together in one array, sorted by value:  
![Array tree image 11](https://raw.githubusercontent.com/Ian-RG/MergeSortWalkthrough/master/Array_11.png)  

In broad strokes, that is how merge sort works.  

In reality, the “divide” and “conquer” elements aren’t done entirely one after the other like this. The merging of arrays begins as soon as we have our first pair of arrays of size 1, so we are swapping between “dividing” and “conquering” depending on what stage we are at. Here we will look at how this works with a pseudocode example.

# **Part Two: Implementation**
This implementation of merge sort will involve two functions: One for dividing the current array (merge_sort), and one for merging our left and right arrays (merge).  


## Function 1 - merge_sort  
First let’s look at the merge_sort function, which will split our arrays.  
merge_sort will only need one parameter - the array to be split. Let’s name it “values”:  
```c++
merge_sort (values[])
```  

Our first variable inside the function will be for storing the length of array values, named values_len.  
```c++
values_len = length of values array
```

Now the first thing we want to do next is check to see if the length of array values is less than 2 (ie, we have an array of length one and are at the bottom of the tree illustrated in the first section). If the length is less than 2, we can return - ending our recursive calls:  
```c++
if (values_len < 2)
        return
```

If the program continues past this point we need to divide the array, as it is larger than length 1. So let’s store the middle index of the array (The actual index, not the value stored in it) in a variable named mid:  
```c++
mid = (values_len / 2)
```

Next we will create 2 new arrays. An array named left of length [mid], and an array named right of length [values_len – mid]. It’s important that the right array is given the length [values_len – mid], otherwise you are going to run into trouble if values_len is an odd number. Here we have:  
```c++
left[mid]
right[values_len - mid]
```

Next we need to fill our left and right arrays. We can do this with a for loop for each array:  
```c++
// Fill left array
for i = 0 to mid - 1
    left[i] = values[i]
    
// Fill right array
for i = mid to values_len - 1
    right[i - mid] = values[i]
```

Now that we have our arrays filled, we can start to split those into even smaller arrays. First we make a recursive call to merge_sort, sending our left array as a parameter:  
```c++
merge_sort (left)
```  

This will result in the left branch being followed all the way down in successive calls to merge_sort, until we reach array length 1 at which point the recursive calls will end due to our if statement at the start of the function.  
The next step once we bounce back from our left array of size one, is to send our right branch:  
```c++
merge_sort (right)
```

Now that we have a pair of sorted arrays to merge, we can call our merge function to insert our left and right arrays back into our values array which we started with.  
```c++
merge (left, right, values)
```

## Function 2 - merge  

Now let’s take a look at the merge function. This function will be called by merge_sort when there are arrays to be merged. As such, we will need to pass three arrays to this function. The left array to be inserted (L_array), the right array to be inserted (R_array), and the final merged array (M_array).

```c++
merge (L_array[], R_array[], M_array[])
```

Inside merge, we will need two variables to store the length of the left and right arrays (one each), and three more variables to track which index we are working on inside each array (x for L_array, y for R_array, z for M_array).  

```c++
left_len    = length of left array  
right_len   = length of right array  
x           = 0  
y           = 0       
z           = 0
```

Next, we are going to start a while loop. We are going to be iterating through our smaller arrays, and will want to stop when we hit the end of one of them. So our while loop will be as follows:  
```c++
while (x < left_len and y < right_len)
```

Inside our loop, we want to start comparing elements of the left and right arrays, and inserting the lesser value into our sorted array. We will also increment the index tracker of whichever array we inserted from. We will also increment z, the index tracker of M_array, once a value has been inserted. Now we have:   
```c++
while (x < left_len and y < right_len)  
{
    // If the value in the left array is lower, insert left value  
    if (L_array[x] < R_array[y])  
    {  
        M_array[z] = L_array[x]  
        x = x + 1  
    }  
    // Otherwise, insert right value 
    else  
    {  
        M_array[z] = R_array[y]  
        y = y+1  
    }  
    // Move to next index in M_array  
    z = z+1  
}  
```

This will loop until one of the arrays is exhausted. Once we reach this point, whatever values are left in the other array can simply be inserted into the sorted array. We can use two more while loops, one for the left array and one for the right, to check if the index tracker is less than the length of the array. Inside each loop we will insert the next value of that array into the sorted array, and increment both the index tracker for that array, and our M_array index tracker (z).  
```c++  
// Insert remaining left values
while (x < left_len)
{
    M_array[z] = L_array[x]
    x = x + 1
    z = z + 1
}

// Insert remaining right values
while (y < right_len)
{
    M_array[z] = R_array[y]
    y = y + 1
    z = z + 1
}
```
At the end of these while loops, both of our smaller arrays should be merged into one larger array. This ends the merge function.

Once our first call of merge is complete, we are at the point of having our leftmost array of size 2 sorted in the array tree we looked at in the first section. 

And that’s it! This will continue to run until you have one, sorted array.  

I strongly recommend doing a desk check using a small array of length 4 or so, and tracking the arrays in a tree structure as you go. This can be a great way to help internalise what’s really going on.  

Below you can find the pseudocode for each function put together. Best of luck!  

```c++
merge (L_array[], R_array[], M_array[])
{
    left_len    = length of left array
    right_len   = length of right array
    x           = 0       
    y           = 0       
    z           = 0      

    while (x < left_len and y < right_len)
    {
        // If the value in the left array is lower, insert left value
        if (L_array[x] < R_array[y])
        {
            M_array[z] = L_array[x]
            x = x + 1
        }
        // Otherwise, insert right value
        else
        {
            M_array[z] = R_array[y]
            y = y+1
        }   
        // Move to next index in M_array     
        z = z+1
    }

    // Insert remaining left values
    while (x < left_len)
    {
        M_array[z] = L_array[x]
        x = x + 1
        z = z + 1
    }

    // Insert remaining right values
    while (y < right_len)
    {
        M_array[z] = R_array[y]
        y = y + 1
        z = z + 1
    }
}  

merge_sort (values[])
{
    values_len = length of values array

    if (values_len < 2)
        return
    
    mid = (values_len / 2)

    left[mid]
    right[values_len - mid]

    // Fill left array
    for i = 0 to mid - 1
        left[i] = values[i]
    
    // Fill right array
    for i = mid to values_len - 1
        right[i - mid] = values[i]
    

    merge_sort (left)
    merge_sort (right)
    merge (left, right, values)
}
```
