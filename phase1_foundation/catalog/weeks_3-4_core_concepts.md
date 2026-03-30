# Weeks 3-4: Core Concepts (March 15-28, 2026)

## 🎯 **Learning Objectives**
- Master advanced linear algebra concepts
- Develop Python programming fundamentals
- Learn probability theory foundations
- Build mathematical expression parser
- Understand key Math LLM research papers

---

## 📚 **Core Resources**

### **Mathematics**
**Primary Resources**:
- [MIT 18.06 Linear Algebra (Advanced Topics)](https://ocw.mit.edu/courses/18-06sc-linear-algebra-spring-2010/)
- [Stanford CS229 Machine Learning](https://see.stanford.edu/course/cs229)
- [3Blue1Brown Linear Algebra Series](https://www.3blue1brown.com/topics/linear-algebra)

**Specific Topics with Links**:
- **Eigenvalues and Eigenvectors**: [MIT Lecture 21](https://ocw.mit.edu/courses/18-06sc-linear-algebra-spring-2010/video_galleries/lecture-videos/lecture-21/)
- **Orthogonal Complements**: [MIT Lecture 22](https://ocw.mit.edu/courses/18-06sc-linear-algebra-spring-2010/video_galleries/lecture-videos/lecture-22/)
- **Alternate Coordinate Systems**: [MIT Lecture 23](https://ocw.mit.edu/courses/18-06sc-linear-algebra-spring-2010/video_galleries/lecture-videos/lecture-23/)
- **Matrix Transformations**: [MIT Lecture 24](https://ocw.mit.edu/courses/18-06sc-linear-algebra-spring-2010/video_galleries/lecture-videos/lecture-24/)

### **Programming**
**Primary Resources**:
- [Python for Data Science Handbook](https://jakevdp.github.io/PythonDataScienceHandbook/)
- [Harvard CS50](https://cs50.harvard.edu/x/2023/)
- [MIT 6.006 Introduction to Algorithms](https://ocw.mit.edu/courses/6-006-introduction-to-algorithms-fall-2011/)

**Specific Topics with Links**:
- **Data Structures**: [CS50 Week 2: Arrays](https://cs50.harvard.edu/x/2023/weeks/2/)
- **Algorithms**: [MIT 6.006 Problem Sets](https://ocw.mit.edu/courses/6-006-introduction-to-algorithms-fall-2011/pages/assignments/)
- **Memory Management**: [CS50 Week 4: Memory](https://cs50.harvard.edu/x/2023/weeks/4/)

### **Probability Theory**
**Primary Resources**:
- [MIT 18.05 Introduction to Probability and Statistics](https://ocw.mit.edu/courses/18-05-introduction-to-probability-and-statistics-spring-2022/)
- [Stanford Stats 116 Introduction to Probability](https://web.stanford.edu/class/stats116/syllabus.html)
- [StatQuest YouTube Channel](https://www.youtube.com/c/statquest)

**Specific Topics with Links**:
- **Basic Probability**: [MIT Lecture Notes](https://ocw.mit.edu/courses/18-05-introduction-to-probability-and-statistics-spring-2022/pages/classes-reading-and-in-class-materials/)
- **Probability Axioms**: [Stanford Lecture Notes](https://web.stanford.edu/class/stats116/Notes/01.pdf)
- **Conditional Probability**: [Stanford Lecture Notes](https://web.stanford.edu/class/stats116/Notes/03.pdf)
- **Random Variables**: [Stanford Lecture Notes](https://web.stanford.edu/class/stats116/Notes/04.pdf)
- **Statistical Reasoning**: [StatQuest Statistical Reasoning](https://www.youtube.com/watch?v=0oc49DyA3hU)

### **Research**
**Primary Resources**:
- [Toolformer Paper](https://arxiv.org/abs/2302.04761)
- [MathCoder Paper](https://arxiv.org/abs/2410.01985)
- [Papers with Code](https://paperswithcode.com/task/mathematical-reasoning)

**Reading Strategy**:
- **Day 1**: Abstract, Introduction, Related Work
- **Day 2**: Methodology, Experiments
- **Day 3**: Results, Discussion, Future Work

---

## 🛠️ **Core Projects**

### **Project 1: Mathematical Expression Parser**
**Objective**: Build parser for mathematical expressions

**Core Features**:
- Parse arithmetic expressions (+, -, *, /)
- Handle variables and functions
- Symbolic evaluation and simplification

**Basic Implementation**:
```python
from sympy import symbols, simplify, parse_expr

class MathExpressionParser:
    def parse_expression(self, expr_str):
        return parse_expr(expr_str, evaluate=False)
    
    def evaluate_expression(self, expr_str, variables=None):
        expr = self.parse_expression(expr_str)
        if variables:
            return expr.subs(variables)
        return expr
```

### **Project 2: Probability Simulator**
**Objective**: Demonstrate probability concepts

**Core Functions**:
- Simulate basic probability experiments
- Calculate theoretical vs. experimental probabilities
- Visualize probability distributions

---

## 📊 **Learning Outcomes**

Upon completing weeks 3-4, you will be able to:
1. **Advanced Mathematics**: Apply eigenvalues, orthogonal complements
2. **Programming**: Implement data structures and algorithms
3. **Probability**: Understand and simulate probability events
4. **Parsing**: Build mathematical expression processors
5. **Research**: Analyze tool integration papers

---

## 🚀 **Next Steps**

**Weeks 5-6 Preview**:
- Advanced tokenizer implementation
- Statistical hypothesis testing
- Open source contribution strategies
