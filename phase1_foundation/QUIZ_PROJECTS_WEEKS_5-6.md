# Weeks 5-6: Project Development - Quiz & Projects

## 📋 **Bi-Weekly Assessment Overview**
**Duration**: Weeks 5-6 (Project Development)
**Focus**: Mathematical expression parsing, symbolic computation, web development
**Assessment Type**: Quiz + Practical Project

---

## 🧮 **Weeks 5-6 Quiz: Mathematical Computing Assessment**

### **Section 1: Symbolic Mathematics (40%)**

#### **Question 1: Expression Parsing**
**Problem**: Design a parser for mathematical expressions that handles:
1. Arithmetic operations (+, -, *, /, ^)
2. Parentheses and operator precedence
3. Functions (sin, cos, log, sqrt)
4. Variables and constants

Provide the grammar rules and parsing algorithm.

**Scoring**: 10 points (2.5 points each component)

#### **Question 2: Symbolic Differentiation**
**Problem**: Implement symbolic differentiation for:
1. Polynomials
2. Trigonometric functions
3. Exponential and logarithmic functions
4. Chain rule application

Show the derivative rules and provide examples.

**Scoring**: 10 points (2.5 points each function type)

#### **Question 3: Equation Solving**
**Problem**: Design algorithms to solve:
1. Linear equations (ax + b = c)
2. Quadratic equations (ax² + bx + c = 0)
3. Systems of linear equations
4. Simple differential equations

**Scoring**: 10 points (2.5 points each equation type)

#### **Question 4: Numerical Methods**
**Problem**: Explain and implement:
1. Newton-Raphson method for root finding
2. Numerical integration (Simpson's rule)
3. Numerical differentiation
4. Error analysis and convergence

**Scoring**: 10 points (2.5 points each method)

### **Section 2: Algorithm Design (30%)**

#### **Question 5: Tree Structures**
**Problem**: Design expression tree data structures for:
1. Binary expression trees
2. Tree traversal algorithms (in-order, pre-order, post-order)
3. Tree simplification and optimization
4. Tree evaluation algorithms

**Scoring**: 10 points (2.5 points each component)

#### **Question 6: Parsing Algorithms**
**Problem**: Compare and implement:
1. Recursive descent parsing
2. Shunting-yard algorithm
3. Operator precedence parsing
4. Error handling and recovery

**Scoring**: 10 points (2.5 points each algorithm)

#### **Question 7: Computational Complexity**
**Problem**: Analyze the time and space complexity of:
1. Expression parsing algorithms
2. Tree operations
3. Symbolic differentiation
4. Numerical methods

**Scoring**: 10 points (2.5 points each analysis)

### **Section 3: Web Development & Integration (30%)**

#### **Question 8: Web Interface Design**
**Problem**: Design a web interface for mathematical tools with:
1. Input validation and sanitization
2. Real-time computation and results display
3. Error handling and user feedback
4. Responsive design principles

**Scoring**: 10 points (2.5 points each component)

#### **Question 9: API Design**
**Problem**: Design RESTful API endpoints for:
1. Expression evaluation
2. Equation solving
3. Graph plotting
4. History and session management

**Scoring**: 10 points (2.5 points each endpoint)

#### **Question 10: Performance Optimization**
**Problem**: Optimize mathematical computation for:
1. Caching strategies
2. Parallel computation
3. Memory management
4. Response time optimization

**Scoring**: 10 points (2.5 points each optimization)

---

## 🚀 **Weeks 5-6 Project: Mathematical Expression Parser**

### **Project Overview**
**Objective**: Build comprehensive mathematical expression parser with web interface
**Duration**: 2 weeks
**Technologies**: Python, SymPy, Flask, JavaScript, HTML/CSS

### **Project Components**

#### **Component 1: Expression Parser (30%)**
**Requirements**:
- Support for arithmetic operations (+, -, *, /, ^)
- Handle parentheses and operator precedence
- Support for mathematical functions (sin, cos, tan, log, exp, sqrt)
- Variable substitution and evaluation
- Syntax error handling and meaningful error messages

**Deliverables**:
```python
class ExpressionParser:
    def __init__(self):
        self.operators = {'+': 1, '-': 1, '*': 2, '/': 2, '^': 3}
        self.functions = {'sin', 'cos', 'tan', 'log', 'exp', 'sqrt'}
        self.variables = {}
    
    def tokenize(self, expression):
        # Tokenize mathematical expression
        pass
    
    def parse(self, tokens):
        # Parse tokens into expression tree
        pass
    
    def evaluate(self, expression, variables=None):
        # Evaluate expression with optional variable substitution
        pass
    
    def validate_syntax(self, expression):
        # Validate expression syntax
        pass
```

**Success Criteria**:
- ✅ Correct parsing of complex expressions
- ✅ Proper operator precedence handling
- ✅ Support for nested parentheses
- ✅ Comprehensive error handling

#### **Component 2: Symbolic Computation (25%)**
**Requirements**:
- Symbolic differentiation
- Equation solving (linear, quadratic)
- Expression simplification
- Substitution and evaluation

**Deliverables**:
```python
class SymbolicComputer:
    def __init__(self):
        self.parser = ExpressionParser()
        self.simplification_rules = {}
    
    def differentiate(self, expression, variable):
        # Symbolic differentiation
        pass
    
    def solve_equation(self, equation, variable):
        # Solve equations analytically
        pass
    
    def simplify(self, expression):
        # Simplify mathematical expressions
        pass
    
    def substitute(self, expression, substitutions):
        # Variable substitution
        pass
```

**Success Criteria**:
- ✅ Correct symbolic differentiation
- ✅ Accurate equation solving
- ✅ Expression simplification
- ✅ Variable substitution

#### **Component 3: Numerical Methods (20%)**
**Requirements**:
- Numerical root finding (Newton-Raphson)
- Numerical integration (Simpson's rule)
- Numerical differentiation
- Error estimation and convergence

**Deliverables**:
```python
class NumericalMethods:
    def __init__(self):
        self.tolerance = 1e-10
        self.max_iterations = 1000
    
    def newton_raphson(self, f, df, x0):
        # Newton-Raphson root finding
        pass
    
    def simpson_integration(self, f, a, b, n):
        # Numerical integration using Simpson's rule
        pass
    
    def numerical_derivative(self, f, x, h=1e-8):
        # Numerical differentiation
        pass
    
    def estimate_error(self, method, *args):
        # Error estimation for numerical methods
        pass
```

**Success Criteria**:
- ✅ Accurate numerical computations
- ✅ Convergence to correct solutions
- ✅ Proper error estimation
- ✅ Robust handling of edge cases

#### **Component 4: Web Interface (25%)**
**Requirements**:
- Interactive input field with syntax highlighting
- Real-time computation and results display
- Step-by-step solution visualization
- Graph plotting capabilities
- History and session management

**Deliverables**:
```python
# Flask Backend
from flask import Flask, render_template, request, jsonify

app = Flask(__name__)

@app.route('/')
def index():
    return render_template('index.html')

@app.route('/evaluate', methods=['POST'])
def evaluate_expression():
    # Handle expression evaluation
    pass

@app.route('/solve', methods=['POST'])
def solve_equation():
    # Handle equation solving
    pass

@app.route('/plot', methods=['POST'])
def plot_function():
    # Handle function plotting
    pass
```

**Frontend (JavaScript)**:
```javascript
class MathInterface {
    constructor() {
        this.parser = new ExpressionParser();
        this.history = [];
        this.setupEventListeners();
    }
    
    setupEventListeners() {
        // Setup input validation and real-time computation
    }
    
    evaluateExpression(expression) {
        // Send expression to backend for evaluation
    }
    
    displayResults(results) {
        // Display computation results with step-by-step solutions
    }
    
    plotFunction(expression, range) {
        // Generate function plot
    }
}
```

**Success Criteria**:
- ✅ Intuitive user interface
- ✅ Real-time computation feedback
- ✅ Step-by-step solution display
- ✅ Interactive function plotting
- ✅ Responsive design

---

## 📊 **Assessment Rubric**

### **Quiz Scoring (100 points total)**
- **Symbolic Mathematics**: 40 points
- **Algorithm Design**: 30 points
- **Web Development & Integration**: 30 points

**Grade Scale**:
- **A**: 90-100 points (Excellent)
- **B**: 80-89 points (Good)
- **C**: 70-79 points (Satisfactory)
- **D**: 60-69 points (Needs Improvement)
- **F**: <60 points (Unsatisfactory)

### **Project Scoring (100 points total)**
- **Expression Parser**: 30 points
- **Symbolic Computation**: 25 points
- **Numerical Methods**: 20 points
- **Web Interface**: 25 points

**Grade Scale**:
- **A**: 90-100 points (Professional Quality)
- **B**: 80-89 points (Good Implementation)
- **C**: 70-79 points (Basic Implementation)
- **D**: 60-69 points (Minimal Implementation)
- **F**: <60 points (Incomplete)

---

## 🎯 **Success Criteria & Learning Outcomes**

### **Minimum Requirements for Passing**
- **Quiz**: 70% or higher (70+ points)
- **Project**: 70% or higher (70+ points)
- **Both components must be completed**

### **Learning Outcomes**
Upon successful completion, students will be able to:
1. **Symbolic Computing**: Implement mathematical expression parsing and evaluation
2. **Algorithm Design**: Design efficient parsing and computation algorithms
3. **Web Development**: Create interactive mathematical tools with web interfaces
4. **Numerical Methods**: Apply numerical techniques to mathematical problems
5. **Software Engineering**: Build complete, user-friendly applications

### **Prerequisites for Next Phase**
- ✅ Strong programming and algorithmic skills
- ✅ Understanding of symbolic computation
- ✅ Web development experience
- ✅ Preparation for advanced mathematical applications

---

## 📅 **Timeline & Milestones**

### **Week 5**
- **Day 29-30**: Complete quiz preparation and assessment
- **Day 31-33**: Implement expression parser
- **Day 34-35**: Develop symbolic computation components

### **Week 6**
- **Day 36-38**: Implement numerical methods
- **Day 39-41**: Create web interface
- **Day 42**: Testing, documentation, and final submission

---

## 🚀 **Submission Guidelines**

### **Quiz Submission**
- **Format**: Written responses (PDF) + code solutions (Python files)
- **Deadline**: End of Week 5
- **Submission**: Upload to learning management system

### **Project Submission**
- **Format**: GitHub repository with complete application
- **Contents**: Source code, documentation, tests, deployment instructions
- **Deadline**: End of Week 6
- **Submission**: Repository link + live demo URL + video (10 minutes)

---

**🎯 This bi-weekly assessment focuses on mathematical computing and web development, creating practical tools for mathematical problem solving.**
