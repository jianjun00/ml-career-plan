# Phase 1: Authentic Assessment & Project Evaluation

## 🎯 **Assessment Philosophy**

**Replace fake evaluation with real project-based assessment that measures actual skills and understanding.**

---

## 📋 **Assessment 1: Mathematical Computing Project**

### **Project: Mathematical Expression Evaluator**

**Problem Statement**:
> Create a Python function that can parse, evaluate, and simplify mathematical expressions. Your function should handle:
> - Basic arithmetic operations (+, -, *, /, **)
> - Parentheses and order of operations
> - Variable handling (x, y, z, etc.)
> - Error handling for invalid expressions
> - Simplification of complex expressions

**Requirements**:
1. **Function Signature**: `def evaluate_expression(expr: str) -> Tuple[float, str]`
2. **Return Values**: (result, explanation)
3. **Error Handling**: Raise `ValueError` for invalid expressions
4. **Documentation**: Complete docstring with examples
5. **Test Cases**: Include 5 test cases demonstrating functionality

**Example Test Cases**:
```python
# Test cases your function should handle:
test_cases = [
    ("2 + 3 * 4", 14),           # Order of operations
    ("(2 + 3) * 4", 20),         # Parentheses
    ("x^2 + 2*x + 1", None),       # Variables (should return None)
    ("sqrt(16) + 3", 7),           # Functions (should handle sqrt)
    ("2 / (1 - 1)", None),           # Division by zero (should raise error)
]
```

**Evaluation Criteria**:
- **Correctness** (40%): Function produces correct results for valid expressions
- **Error Handling** (20%): Properly handles invalid expressions and edge cases
- **Code Quality** (20%): Clean, readable, well-documented code
- **Documentation** (10%): Complete docstring with examples
- **Test Coverage** (10%): Comprehensive test cases included

---

## 📋 **Assessment 2: Statistical Analysis Project**

### **Project: Real-World Data Analysis**

**Dataset**: [Download this CSV with real student performance data](https://example.com/student_performance.csv)

**Problem Statement**:
> Analyze the provided student performance dataset and answer these questions:
> 1. Calculate descriptive statistics (mean, median, mode, std dev) for math scores
> 2. Identify correlation between study hours and test scores
> 3. Determine if there's a statistically significant difference between two teaching methods
> 4. Create visualizations that clearly communicate your findings
> 5. Provide recommendations for improving student performance

**Requirements**:
1. **Data Loading**: Load and clean the CSV dataset
2. **Statistical Analysis**: Use NumPy/SciPy for calculations
3. **Hypothesis Testing**: Perform appropriate statistical tests
4. **Visualization**: Use Matplotlib/Seaborn for clear charts
5. **Report**: Write a 2-page analysis report with findings

**Specific Analysis Questions**:
```python
# Your analysis must answer:
questions = [
    "What is the average math score and what does it tell us about overall performance?",
    "Is there a correlation between study hours and test scores? Calculate Pearson r.",
    "Do students taught with Method A perform significantly better than Method B? Use t-test.",
    "What are the main factors influencing student performance based on the data?",
    "Based on your analysis, what 3 specific recommendations would you make to improve performance?"
]
```

**Evaluation Criteria**:
- **Statistical Accuracy** (35%): Correct application of statistical methods
- **Data Analysis Quality** (25%): Depth of insights and pattern recognition
- **Visualization Clarity** (20%): Clear, informative charts with proper labels
- **Report Quality** (15%): Well-structured, actionable recommendations
- **Code Organization** (5%): Clean, reproducible analysis code

---

## 📋 **Assessment 3: LLM Concepts Implementation**

### **Project: Simple Attention Mechanism**

**Problem Statement**:
> Implement a simplified attention mechanism from scratch using only NumPy. Your implementation should:
> - Take input sequences, queries, keys, and values
> - Compute attention scores using scaled dot-product attention
> - Apply softmax to get attention weights
> - Return weighted sum of values
> - Include proper documentation and explanation

**Technical Requirements**:
```python
def simple_attention(Q, K, V):
    """
    Simplified attention mechanism implementation.
    
    Args:
        Q: Query matrix of shape (seq_len, d_k)
        K: Key matrix of shape (seq_len, d_k)  
        V: Value matrix of shape (seq_len, d_v)
    
    Returns:
        attention_output: Weighted sum of values
        attention_weights: Attention weight matrix
    """
    # Your implementation here
```

**Specific Implementation Tasks**:
1. **Scaled Dot-Product**: Implement attention score calculation
2. **Softmax**: Apply softmax to get attention weights
3. **Weighted Sum**: Multiply weights by values and sum
4. **Dimension Handling**: Proper matrix operations and broadcasting
5. **Numerical Stability**: Handle potential numerical issues

**Test Your Implementation**:
```python
# Test with simple inputs
Q = np.array([[1, 0], [0, 1]])  # 2x2
K = np.array([[1, 0], [0, 1]])  # 2x2  
V = np.array([[1, 2], [3, 4]])  # 2x2

output, weights = simple_attention(Q, K, V)

# Expected: output should be different from input V
# Expected: weights should sum to 1 across appropriate dimensions
```

**Evaluation Criteria**:
- **Correct Implementation** (40%): Attention mechanism works as specified
- **Numerical Accuracy** (25%): Correct matrix operations and softmax
- **Code Quality** (20%): Clean, efficient, well-commented code
- **Understanding** (15%): Clear explanation of how attention works
- **Testing** (10%): Comprehensive test cases and validation

---

## 📋 **Assessment 4: Research Comprehension**

### **Project: Paper Analysis & Implementation**

**Research Paper**: [Chain-of-Thought Prompting](https://arxiv.org/abs/2201.11903)

**Comprehension Tasks**:
> Read the paper and demonstrate understanding by:
> 1. Explaining the core concept in your own words
> 2. Implementing a simplified chain-of-thought example
> 3. Identifying 3 key limitations mentioned in the paper
> 4. Proposing 1 improvement or extension to the method
> 5. Creating a visualization of how chain-of-thought works

**Specific Deliverables**:
```python
# Your submission should include:
deliverables = [
    "2-page summary of chain-of-thought concept",
    "Python implementation of simplified CoT prompting",
    "List of 3 limitations with explanations",
    "1 proposed improvement with rationale",
    "Visualization showing step-by-step reasoning process"
]
```

**Implementation Example**:
```python
def chain_of_thought_example(problem):
    """
    Demonstrate chain-of-thought with a simple math problem.
    
    Args:
        problem: String representing a math problem
        
    Returns:
        reasoning_steps: List of reasoning steps
        final_answer: Final computed answer
    """
    # Example: Problem: "A store sells apples for $2 each. 
    # If they sell 15 apples and make $25, how many bananas did they sell?"
    
    reasoning_steps = [
        "Step 1: Calculate total apple revenue: 15 × $2 = $30",
        "Step 2: Calculate banana revenue: $25 - $30 = -$5 (impossible)",
        "Step 3: Identify problem inconsistency - negative revenue impossible",
        "Step 4: Conclude problem has insufficient information"
    ]
    
    return reasoning_steps, "Insufficient information"
```

**Evaluation Criteria**:
- **Comprehension Accuracy** (35%): Correct understanding of paper's main concepts
- **Implementation Quality** (25%): Working code demonstrating the concept
- **Critical Analysis** (20%): Thoughtful identification of limitations and improvements
- **Communication Clarity** (15%): Clear explanations and visualizations
- **Originality** (5%): Novel insights or extensions beyond the paper

---


## 📋 **Submission Guidelines**

### **What to Submit**:
1. **All 4 Projects**: Complete implementations with documentation
2. **Test Results**: Screenshots or outputs showing your code working
3. **Analysis Reports**: Written reports for data analysis and paper comprehension
4. **Reflection**: 1-page reflection on what you learned and challenges faced

### **How to Submit**:
- **GitHub Repository**: Upload all code and documentation
- **Written Reports**: Submit as PDF or Markdown files
- **Screenshots**: Include test results and visualizations
- **Reflection**: Submit as separate document

---

## 🏆 **Success Metrics**

### **What Success Looks Like**:
- **Project Completion**: All 4 projects working and documented
- **Correct Implementation**: Code produces expected outputs
- **Clear Explanations**: Can explain your approach and decisions
- **Problem-Solving**: Demonstrates logical thinking and debugging
- **Critical Analysis**: Shows understanding of limitations and improvements

### **What Needs Improvement**:
- **Incomplete Projects**: Missing functionality or documentation
- **Incorrect Results**: Code doesn't produce expected outputs
- **Poor Documentation**: Unclear explanations or missing examples
- **No Testing**: Code not validated with test cases
- **Superficial Analysis**: Surface-level understanding without depth

