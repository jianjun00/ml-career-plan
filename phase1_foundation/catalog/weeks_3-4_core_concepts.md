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
**Primary Topics**:
- Eigenvalues, eigenvectors, and basis transformations
- Orthogonal complements and coordinate systems
- Probability theory and compound events

**Key Resources**:
- MIT 18.06 Linear Algebra (advanced topics)
- Stanford CS229 Machine Learning
- 3Blue1Brown visualizations

### **Programming**
**Focus Areas**:
- Data structures and algorithms
- Memory management in C/Python
- Mathematical computing with NumPy/SymPy

### **Research**
**Key Papers**:
- Toolformer (tool integration in LLMs)
- MathCoder (mathematical reasoning)

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
