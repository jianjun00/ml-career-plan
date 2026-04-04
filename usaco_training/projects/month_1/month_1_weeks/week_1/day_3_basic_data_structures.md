# Day 3: Basic Data Structures

## 🎯 **Daily Learning Objectives**
- Learn MIT 6.006 Lecture 2 on Models of Computation
- Master basic data structures (arrays, vectors, strings)
- Understand container operations and efficiency
- Practice with 10 data structure problems

## 📚 **Learning Materials**

### **MIT 6.006 Lecture 2: Models of Computation, Document Distance**
- **Video**: https://ocw.mit.edu/courses/6-006-introduction-to-algorithms-fall-2011/lecture-videos/
- **Specific Lecture**: "Models of Computation, Document Distance" (Lecture 2, 51:49)
- **Focus**: Data structure selection, efficiency analysis
- **Key Concepts**:
  - Array operations and complexity (O(1) access, O(n) operations)
  - String processing techniques (character operations, substring)
  - Algorithm efficiency analysis (time vs space tradeoffs)
  - Document distance problem (real-world application)
- **Learning Tasks**:
  1. Watch the full lecture (51 minutes)
  2. Understand the document distance example completely
  3. Write examples of array operations with their complexities
  4. Implement a simple string processing algorithm
  5. Analyze time/space tradeoffs for different data structures

### **USACO Guide: Sorting and Searching**
- **Resource**: https://usaco.guide/current/bronze/sort-search/
- **Specific Sections**:
  - "Introduction to Sorting"
  - "Sorting Algorithms" (Selection, Bubble, Insertion)
  - "Binary Search"
  - "Applications of Sorting and Searching"
- **Learning Tasks**:
  1. Read all sorting sections thoroughly
  2. Learn `std::sort()` with default and custom comparators — do NOT implement sorting from scratch
  3. Understand binary search concept; learn `lower_bound`/`upper_bound`
  4. Practice with USACO sorting problems
  5. Know when O(n log n) vs O(n²) is needed based on constraints

> **Note**: At Bronze level you use `std::sort()`. Implementing bubble sort, selection sort etc. from scratch is not tested and wastes contest time. Understanding *why* O(n log n) is faster than O(n²) is sufficient conceptual background.

### **C++ STL Tutorial**
- **Resource**: https://en.cppreference.com/w/cpp/container
- **Specific Sections**:
  - "std::vector" (all member functions)
  - "std::string" (all operations and methods)
  - "Container operations" (begin, end, size, empty)
  - "Iterator basics" (iterator types and operations)
- **Learning Tasks**:
  1. Study all vector operations (push_back, pop_back, resize, reserve)
  2. Master string methods (substr, find, length, empty)
  3. Understand iterator usage (begin(), end(), range-based loops)
  4. Practice container efficiency analysis
  5. Write programs using different STL containers

## 🔧 **Data Structures to Master**

### **Arrays and Vectors**
- **Declaration**: Different ways to declare arrays and vectors
- **Initialization**: Initialize with values
- **Access**: Element access and bounds checking
- **Operations**: Common operations (push, pop, resize)
- **Efficiency**: Time complexity analysis

### **Strings**
- **Creation**: String creation and initialization
- **Manipulation**: Common string operations
- **Character Operations**: Individual character access
- **String Functions**: Built-in string methods
- **Applications**: String-based problem solving

### **Basic Sorting Concepts**
- **Why Sorting**: Importance in problem solving
- **Simple Sorting**: Basic sorting algorithms
- **Sorting Applications**: When and how to use sorting
- **Efficiency**: Sorting complexity considerations

## 💻 **Practice Problems (12 problems)**

### **USACO Bronze Data Structure Problems**
1. **Stuck in a Rut** - Array processing
   - **Problem URL**: https://usaco.guide/bronze/simulation/
   - **Skills**: Array manipulation, coordinate processing
   - **Focus**: Array operations and coordinate geometry
   - **Learning Steps**:
     1. Read coordinate input and understand movement patterns
     2. Implement array-based coordinate tracking
     3. Process movement instructions step by step
     4. Output final positions
   - **Expected Time**: 20 minutes
   - **Solution Approach**: Array-based simulation with coordinate updates

2. **Cowntagion** - Mathematical simulation
   - **Problem URL**: https://usaco.guide/bronze/ad-hoc/
   - **Skills**: Array-based simulation, mathematical modeling
   - **Focus**: Simulation with array storage
   - **Learning Steps**:
     1. Understand exponential growth pattern
     2. Read farm structure and implement simulation
     3. Use array to track infection spread
     4. Calculate minimum days needed
   - **Expected Time**: 25 minutes
   - **Solution Approach**: Array-based simulation with mathematical calculations

3. **Milk Visits** - Graph basics
   - **Problem URL**: https://usaco.guide/bronze/intro-graphs/
   - **Skills**: Basic graph representation, array usage
   - **Focus**: Graph concepts with array storage
   - **Learning Steps**:
     1. Read farm connections and build graph structure
     2. Implement graph traversal using arrays
     3. Process milk visit queries
     4. Output visit results
   - **Expected Time**: 30 minutes
   - **Solution Approach**: Array-based graph representation and traversal

4. **Air Cownditioning** - Array manipulation
   - **Problem URL**: https://usaco.guide/bronze/intro-complete/
   - **Skills**: Array operations, range processing
   - **Focus**: Array manipulation and range updates
   - **Learning Steps**:
     1. Read temperature requirements and air conditioner capacities
     2. Implement array-based range updates
     3. Calculate minimum air conditioners needed
     4. Output optimal solution
   - **Expected Time**: 25 minutes
   - **Solution Approach**: Array manipulation with range operations

5. **Game of Thrones** - String processing
   - **Problem URL**: https://usaco.guide/bronze/intro-ds/
   - **Skills**: String manipulation, character counting
   - **Focus**: String operations and character analysis
   - **Learning Steps**:
     1. Read string input and understand palindrome requirements
     2. Implement character counting using arrays
     3. Check palindrome formation possibility
     4. Output result
   - **Expected Time**: 15 minutes
   - **Solution Approach**: String processing with character frequency analysis

### **Codeforces 900 Difficulty**
6. **Sorting Problems**: Various sorting applications
   - **Problem URL**: https://codeforces.com/problemset/problem/1791/A
   - **Skills**: Sorting implementation, custom comparators
   - **Focus**: Sorting algorithms and applications
   - **Learning Steps**:
     1. Read input and understand sorting requirements
     2. Implement appropriate sorting algorithm
     3. Apply custom sorting rules if needed
     4. Output sorted result
   - **Expected Time**: 15 minutes
   - **Solution Approach**: Standard sorting with custom rules

7. **Array Operations**: Basic array manipulations
   - **Problem URL**: https://codeforces.com/problemset/problem/1791/B
   - **Skills**: Array processing, element operations
   - **Focus**: Array manipulation techniques
   - **Learning Steps**:
     1. Read array input and understand operations
     2. Implement required array transformations
     3. Process array elements efficiently
     4. Output transformed array
   - **Expected Time**: 15 minutes
   - **Solution Approach**: Array processing with specific operations

8. **String Processing**: String manipulation problems
   - **Problem URL**: https://codeforces.com/problemset/problem/1791/C
   - **Skills**: String operations, character processing
   - **Focus**: String manipulation and analysis
   - **Learning Steps**:
     1. Read string input and understand processing rules
     2. Implement string transformation algorithm
     3. Process characters according to requirements
     4. Output processed string
   - **Expected Time**: 20 minutes
   - **Solution Approach**: String manipulation with character operations

9. **Implementation**: Simple implementation tasks
   - **Problem URL**: https://codeforces.com/problemset/problem/1791/D
   - **Skills**: Careful coding, edge case handling
   - **Focus**: Implementation accuracy and efficiency
   - **Learning Steps**:
     1. Understand problem requirements thoroughly
     2. Implement solution with attention to details
     3. Handle edge cases properly
     4. Test with sample cases
   - **Expected Time**: 20 minutes
   - **Solution Approach**: Careful implementation with edge case handling

10. **Mathematical**: Basic mathematical computations
    - **Problem URL**: https://codeforces.com/problemset/problem/1791/E
    - **Skills**: Mathematical operations, precision
    - **Focus**: Mathematical accuracy and efficiency
    - **Learning Steps**:
      1. Read mathematical input and understand operations
      2. Implement required calculations
      3. Ensure precision and accuracy
      4. Output mathematical results
    - **Expected Time**: 15 minutes
    - **Solution Approach**: Mathematical computation with precision handling

### **Kattis Problems**
11. **Basic Data Structures**: Container operations
    - **Problem URL**: https://open.kattis.com/problems/apaxiaaans
    - **Skills**: Container manipulation, efficiency
    - **Focus**: Data structure selection and usage
    - **Learning Steps**:
      1. Read input and understand data structure needs
      2. Choose appropriate container (vector, string, etc.)
      3. Implement efficient operations
      4. Output results
    - **Expected Time**: 15 minutes
    - **Solution Approach**: Container-based solution with efficiency

12. **Sorting Applications**: Real-world sorting
    - **Problem URL**: https://open.kattis.com/problems/apaxiaapax
    - **Skills**: Sorting implementation, optimization
    - **Focus**: Practical sorting applications
    - **Learning Steps**:
      1. Read input and understand sorting requirements
      2. Implement sorting algorithm
      3. Optimize for efficiency
      4. Output sorted result
    - **Expected Time**: 20 minutes
    - **Solution Approach**: Practical sorting with optimization

### **Data Structure Problem-Solving Strategy**
1. **Analyze Requirements**: Determine data structure needs
2. **Choose Structure**: Select optimal container (array, vector, string)
3. **Implement Operations**: Write efficient operations
4. **Test Edge Cases**: Verify with boundary conditions
5. **Optimize**: Improve time/space complexity
6. **Validate**: Ensure correctness and efficiency

## 🎯 **Daily Goals**
- **MIT Lecture**: Complete Lecture 2 understanding
- **Data Structures**: Master arrays, vectors, strings
- **Container Operations**: Understand STL container usage
- **Problem Solving**: Complete 12 problems successfully

## 📊 **Success Metrics**
- **Problems Completed**: 12/12
- **Understanding**: Data structure concepts and efficiency
- **Implementation**: Efficient container usage
- **Time Management**: < 20 minutes per problem

## 📝 **Notes & Reflection**
- **Key Learnings**: 
- **Difficult Concepts**: 
- **Improvement Areas**: 
- **Tomorrow's Focus**: 

---

## 🔧 **Implementation Checklist**
- [ ] Watch MIT 6.006 Lecture 2
- [ ] Understand array operations and complexity
- [ ] Master vector operations and methods
- [ ] Learn string manipulation techniques
- [ ] Understand container efficiency
- [ ] Solve Stuck in a Rut problem
- [ ] Solve Cowntagion problem
- [ ] Solve Milk Visits problem
- [ ] Solve Air Cownditioning problem
- [ ] Solve Game of Thrones problem
- [ ] Solve Codeforces sorting problems
- [ ] Solve Codeforces array operations
- [ ] Solve Codeforces string processing
- [ ] Solve Codeforces implementation problems
- [ ] Solve Codeforces mathematical problems
- [ ] Solve Kattis basic data structures
- [ ] Solve Kattis sorting applications
- [ ] Review all solutions
- [ ] Document data structure insights
- [ ] Prepare for Day 4 topics

## 🎯 **Data Structure Selection Guide**
1. **Array**: Fixed size, random access needed
2. **Vector**: Dynamic size, frequent insertions/deletions
3. **String**: Character manipulation, text processing
4. **Consider**: Time complexity, space requirements
5. **Choose**: Based on problem requirements

## 📚 **Key Concepts to Remember**
- **Time Complexity**: O(1) access, O(n) traversal
- **Space Complexity**: Memory usage considerations
- **STL Methods**: push_back(), pop_back(), resize()
- **String Methods**: length(), substr(), find()
- **Iterator Usage**: Begin(), end(), range-based loops
