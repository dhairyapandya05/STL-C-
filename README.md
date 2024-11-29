# STL-C++

- writing comparator function for priority_queue
```c++
Using lambda function

auto comp = [](int &a, int &b) {
    return a<b; //max-heap
    return a>b; //min-heap 
};

priority_queue<int, vector<int>, decltype(comp) > pq(comp);   //usage
```
### :memo: When and why to use std::move() :arrow_left:
```c++
/*
    To efficiently transfer the resources from source to target.
    By efficient, I mean no usage of extra space and time for creating copy.
*/
Examples :
    string source = "MIK";
    string target = "";
    target = std::move(source);
    cout << " source = " << source << endl;
    cout << "target = "  << target << endl;
    /*
        output :
        source = 
        target = "MIK"
    */
    
    vector<string> v;
    string str = "example";
    v.push_back(std::move(str));
    /*
    After this, str becomes empty i.e. ""
    And while moving str inside v, no extra copy of str was done implicitly.
    */

    vector<int> temp{1, 2, 3};
    vector<vector<int>> result;
    result.push_back(std::move(temp));
    /*
    This allows no copy of "temp" being created.
    It ensures that the contents of "temp"
    will be moved into the "result".  This is less
    expensive, also means temp will now be empty.
    */
```
### :memo: std::accumulate(begin_iterator, end_iterator, initial_sum) :heavy_plus_sign:
```c++
int sum = 0;
vector<int> nums{1, 3, 2, 5};
sum = accumulate(begin(nums), end(nums), 0);

cout << sum; //11

Benefit : You didn't have to write for loop to find the sum
```

### :memo: std::accumulate(begin_iterator, end_iterator, initial_sum, lambda) :heavy_plus_sign:
```c++
lambda : Binary operation taking an element of type <initial_sum> as first argument and an
            element in the range as second, and which returns a value that can be assigned to type T.

Example-1 : 

auto lambda = [&](int s, long n) {
    return s + n*n; //sums the square of numbers
    //You can call any other function inside as well
};

int sum = 0;
vector<int> nums{1, 3, 2, 5};
sum = accumulate(begin(nums), end(nums), 0, lambda);

cout << sum; //39

Example-2 : Handling 2-D matrix
//Summming all elements row by row
auto lambda = [&](int sum, vector<int> vec) {
    sum = sum + accumulate(begin(vec), end(vec), 0);
    return sum;
};

int result =  accumulate(matrix.begin(), matrix.end(), 0, lambda);


Beautiful example and usage :
Leetcode-1577 (My Approach - https://leetcode.com/problems/number-of-ways-where-square-of-number-is-equal-to-product-of-two-numbers/discuss/1305961/C%2B-(A-very-simple-Two-Sum-like-approach)

Leetcode-1572 (My Approach - https://leetcode.com/problems/matrix-diagonal-sum/discuss/3498479/Using-C%2B%2B-STL-%3A-accumulate)

```

### :memo: min_element(begin_iterator, end_iterator), max_element(begin_iterator, end_iterator), minmax_element(begin_iterator, end_iterator) :astonished:
```c++
vector<int> nums{1, 3, 2, 5};

int minimumValue = *min_element(begin(nums), end(nums)); //1
int maximumValue = *max_element(begin(nums), end(nums)); //5
                OR,
        auto itr  = minmax_element(begin(nums), end(nums));
int minimumValue  = *itr.first;  //remember, first is minimum  //1
int maximumValue  = *itr.second; //remember, second is maximum //5


Benefit : You didn't have to write for loop to find the max or min element
```

### :memo: upper_bound(), lower_bound() in sorted vector, ordered set, ordered map :outbox_tray:
```c++

For vector:
vector<int> vec{10,20,30,30,20,10,10,20};
vector<int>::iterator up  = upper_bound(begin(vec), end(vec), 35);//returns iterator to first element "greater" than 35
vector<int>::iterator low = lower_bound(begin(vec), end(vec), 35);//returns iterator to first element "greater or equal" to 35
cout << "upper_bound at position " << (up - vec.begin()) << '\n';
cout << "lower_bound at position " << (low- vec.begin()) << '\n';

For set:
st.upper_bound(35); //returns iterator to first element "greater" than 35
st.lower_bound(35); //returns iterator to first element "greater or equal" than 35

For map:
mp.upper_bound(35); //returns iterator to first element "greater" than 35
mp.lower_bound(35); //returns iterator to first element "greater or equal" than 35

Benefit : You didn't have to write binary search (in case of vector),
JAVA's tree_map equivalent in C++ (in case of map or set)
There are amazing applications or problems that can be solved using the above concepts.
Example : My Calendar I (Leetcode - 729) -
         You can find it in my interview_ds_algo repository as well B-)
```
### :memo: std::rotate 🌀
```c++
vector<int> vec{1, 2, 3, 4};
int n = vec.size();
int k = 2;

rotate(vec.begin(), vec.begin()+k, vec.end());   //Left Rotate by K times using "<<"

rotate(vec.begin(), vec.begin()+n-k, vec.end()); //Right Rotate by K times using ">>"

```
### :memo: To check if some rotation of string s can become string t🌀
```c++

string s = "abcde";
string t = "cdeab";

cout << (s.length() == t.length() && (s+s).find(t) != string::npos) << endl;
This string::npos is vvvvvvv.Important
```

### :memo: std::next_permutation ➡️
```c++
It gives the next lexicographically greater permutation.
So, if the container is already the greatest permutation (descending order), it returns nothing.

vector<int> vec{1, 2, 3, 4};
    
if(next_permutation(begin(vec), end(vec)))
    cout << "Next permutation available" << endl;

for(int &x : vec)
    cout << x << " ";
    
//Output : 1, 2, 4, 3

Also see : std::prev_permutation() - It gives just the previous lexicographically smaller permutation.
But I have never encountered any question where it's required till now. So you can skip it.
    Leetcode - 31  : Next Permutation
    etc.
```

### :memo: std::stringstream :fast_forward:
```c++
Usage:
1) Converting string to number
2) Count number of words in a string

Example-1
    string s = "12345";
    stringstream ss(s);
 
    // The object has the value 12345 and stream
    // it to the integer x
    int x = 0;
    ss >> x;
    cout << x;
    
Exmaple-2
    stringstream s(ss);
    string word; // to store individual words
  
    int count = 0;
    while (s >> word)
        count++;
    cout << count;
    NOTE: It will tokenize words on the basis of ' ' (space by default) characters
Example-3
    It can be used very well to extract numbers from string.
    string complex = "1+1i";
    stringstream ss(complex);
    char justToSkip;
    int real, imag;
    ss >> real >> justToSkip >> imag >> justToSkip;
    cout << real << ", " << imag; //output : 1, 1
    
    Other application on this STL :
    Leetcode - 151  : Reverse Words in a String
    Leetcode - 186  : Reverse Words in a String II
    Leetcode - 557  : Reverse Words in a String III
    Leetcode - 1108 : Defanging an IP Address
    Leetcode - 1816 : Truncate Sentence
    Leetcode - 884  : Uncommon Words from Two Sentences
    Leetcode - 537  : Complex Number Multiplication (Example-3 above)
    Leetcode - 165  : Compare Version Numbers
    etc.
```


### :memo: std::transform(InputIterator first1, InputIterator last1, OutputIterator result, UnaryOperation op) :robot:
```c++
Applies an operation sequentially to the elements of one (1) or
two (2) ranges and stores the result in the range that begins at result.
Uage :
1) Convert all letters of a string to lower case
2) Convert all letters of a string to upper case

Example : 
    string line = "Hello world, this is MIK";

    transform(begin(line), end(line), begin(line), ::tolower);

    cout << line << endl;

    transform(begin(line), end(line), begin(line), ::toupper);

    cout << line << endl;

```
The std::transform function with a unary operation (applying an operation on each element of a single range) is incredibly flexible. Apart from converting cases as shown in the example, you can use other unary operations depending on the desired transformation.

Here’s a breakdown of unary operations you can apply:

1. Mathematical Operations
Apply arithmetic or mathematical operations to elements in a range:

Increment each element:

c++
Copy code
vector<int> nums = {1, 2, 3, 4};
transform(begin(nums), end(nums), begin(nums), [](int x) { return x + 1; });
// nums becomes {2, 3, 4, 5}
Square each element:

c++
Copy code
vector<int> nums = {1, 2, 3, 4};
transform(begin(nums), end(nums), begin(nums), [](int x) { return x * x; });
// nums becomes {1, 4, 9, 16}
Apply a mathematical function (e.g., std::abs, std::sqrt):

c++
Copy code
vector<int> nums = {-1, -2, 3, 4};
transform(begin(nums), end(nums), begin(nums), [](int x) { return abs(x); });
// nums becomes {1, 2, 3, 4}
2. Logical/Conditional Operations
Transform elements based on logical conditions:

Clamp values within a range:

c++
Copy code
vector<int> nums = {-1, 5, 10, 15};
transform(begin(nums), end(nums), begin(nums), [](int x) { return clamp(x, 0, 10); });
// nums becomes {0, 5, 10, 10}
Set all negative numbers to zero:

c++
Copy code
vector<int> nums = {-3, -1, 4, 6};
transform(begin(nums), end(nums), begin(nums), [](int x) { return x < 0 ? 0 : x; });
// nums becomes {0, 0, 4, 6}
3. Character/String Manipulation
Toggle case for characters:

c++
Copy code
string str = "HeLLo WoRLd";
transform(begin(str), end(str), begin(str), [](char c) {
    return islower(c) ? toupper(c) : tolower(c);
});
// str becomes "hEllO wOrlD"
Remove non-alphanumeric characters:

c++
Copy code
string str = "H3ll0 W@rld!";
transform(begin(str), end(str), begin(str), [](char c) {
    return isalnum(c) ? c : ' ';
});
// str becomes "H3ll0 W rld "
4. Bitwise Operations
Transform elements with bitwise operations:

Invert bits:

c++
Copy code
vector<int> nums = {0b1010, 0b1100};
transform(begin(nums), end(nums), begin(nums), [](int x) { return ~x; });
// nums becomes {~0b1010, ~0b1100}
Shift bits:

c++
Copy code
vector<int> nums = {1, 2, 4};
transform(begin(nums), end(nums), begin(nums), [](int x) { return x << 1; });
// nums becomes {2, 4, 8}
5. Type Conversion
Transform one data type to another:

Convert int to float:

c++
Copy code
vector<int> nums = {1, 2, 3};
vector<float> floats(nums.size());
transform(begin(nums), end(nums), begin(floats), [](int x) { return float(x); });
// floats becomes {1.0, 2.0, 3.0}
Convert characters to ASCII values:

c++
Copy code
string str = "ABC";
vector<int> ascii(str.size());
transform(begin(str), end(str), begin(ascii), [](char c) { return int(c); });
// ascii becomes {65, 66, 67}
6. Data Transformation
Normalize numbers:

c++
Copy code
vector<int> nums = {2, 4, 6, 8};
int max_val = *max_element(begin(nums), end(nums));
transform(begin(nums), end(nums), begin(nums), [max_val](int x) {
    return x / float(max_val);
});
// nums becomes {0.25, 0.5, 0.75, 1.0}
Map to a range (e.g., compress to 0-100):

c++
Copy code
vector<int> nums = {5, 10, 15};
transform(begin(nums), end(nums), begin(nums), [](int x) { return x * 10; });
// nums becomes {50, 100, 150}
General Pattern
The fourth argument to std::transform is any callable object (function, lambda, functor) that takes one input and returns one output. This allows you to implement virtually any transformation.

By leveraging lambdas and STL functions, std::transform becomes a powerful tool to apply transformations in a concise and efficient manner.



