# Weeks 1-2: Foundation Setup - Quiz & Projects

## 📋 **Bi-Weekly Assessment Overview**
**Duration**: Weeks 1-2 (Foundation Setup)
**Focus**: Mathematical foundations, programming environment, basic linear algebra
**Assessment Type**: Quiz + Practical Project

---

## 🧮 **Weeks 1-2 Quiz: Foundation Assessment**

### **Section 1: Mathematical Foundations (40%)**

#### **Question 1: Linear Algebra Basics**
**Problem**: Given vectors u = [3, 4, 5] and v = [1, 2, 3], calculate:
1. Dot product u · v
2. Cross product u × v
3. Angle between vectors
4. Projection of u onto v

Show all calculations and explain the geometric meaning of each operation.

**Scoring**: 10 points (2.5 points each)

#### **Question 2: Matrix Operations**
**Problem**: For matrix A = [[2, 1], [3, 4]] and B = [[1, 0], [2, 1]]:
1. Calculate A + B
2. Calculate A × B
3. Find det(A)
4. Find A⁻¹ (inverse of A)

Explain the significance of determinants and matrix inverses in mathematical computations.

**Scoring**: 10 points (2.5 points each)

#### **Question 3: Function Properties**
**Problem**: For the function f(x) = 2x² - 4x + 1:
1. Find the vertex
2. Determine if it's convex or concave
3. Find the roots
4. Sketch the graph

**Scoring**: 10 points (2.5 points each)

#### **Question 4: Statistical Concepts**
**Problem**: Explain with examples:
1. Mean vs Median vs Mode
2. Standard deviation and variance
3. Normal distribution properties
4. Correlation vs Causation

**Scoring**: 10 points (2.5 points each)

### **Section 2: Programming & Environment (30%)**

#### **Question 5: Python Setup**
**Problem**: Write a Python script that:
1. Creates a 3x3 NumPy array with random values
2. Calculates basic statistics (mean, std, min, max)
3. Visualizes the array as a heatmap
4. Saves the plot as PNG

**Scoring**: 15 points

#### **Question 6: Git Workflow**
**Problem**: Explain the Git workflow for:
1. Creating a new repository
2. Making changes and committing
3. Creating and merging branches
4. Pushing to remote repository

**Scoring**: 15 points

### **Section 3: Problem Solving (30%)**

#### **Question 7: Mathematical Problem**
**Problem**: A rectangular garden has perimeter 40 meters and area 75 square meters. Find the dimensions of the garden using algebraic methods.

**Scoring**: 15 points

#### **Question 8: Algorithm Design**
**Problem**: Design an algorithm to find the greatest common divisor (GCD) of two numbers. Provide pseudocode and explain its time complexity.

**Scoring**: 15 points

---

## 🚀 **Weeks 1-2 Project: Mathematical Foundation Toolkit**

### **Project Overview**
**Objective**: Build essential mathematical computing toolkit with Python
**Duration**: 2 weeks
**Technologies**: Python, NumPy, SciPy, Matplotlib, Jupyter

### **Project Components**

#### **Component 1: Linear Algebra Library (35%)**
**Requirements**:
- Matrix operations (add, multiply, transpose, inverse, determinant)
- Vector operations (dot product, cross product, projection)
- Eigenvalue and eigenvector computation
- Matrix decomposition (LU, QR, SVD)

**Deliverables**:
```python
class LinearAlgebraToolkit:
    def __init__(self):
        self.matrix_operations = {}
        self.vector_operations = {}
    
    def matrix_multiply(self, A, B):
        # Implementation
        pass
    
    def eigen_decomposition(self, matrix):
        # Implementation
        pass
    
    # ... other methods
```

**Success Criteria**:
- ✅ All operations work correctly with test cases
- ✅ Performance: <1 second for 100x100 matrix operations
- ✅ Documentation: Clear docstrings and examples
- ✅ Error handling: Proper exception handling for edge cases

#### **Component 2: Statistics Calculator (30%)**
**Requirements**:
- Descriptive statistics (mean, median, mode, std, variance)
- Probability distributions (normal, binomial, poisson)
- Hypothesis testing (t-test, chi-square)
- Correlation analysis

**Deliverables**:
```python
class StatisticsCalculator:
    def __init__(self):
        self.data = None
    
    def descriptive_stats(self, data):
        # Implementation
        pass
    
    def hypothesis_test(self, data1, data2, test_type):
        # Implementation
        pass
    
    # ... other methods
```

**Success Criteria**:
- ✅ Correct statistical calculations
- ✅ Support for different data types
- ✅ Visualization of statistical results
- ✅ Performance: <0.5 seconds for 10,000 data points

#### **Component 3: Visualization Tools (20%)**
**Requirements**:
- Function plotting (2D and 3D)
- Data visualization (scatter, histogram, box plot)
- Statistical plots (QQ plots, confidence intervals)
- Interactive visualizations

**Deliverables**:
```python
class VisualizationTools:
    def __init__(self):
        self.figures = {}
    
    def plot_function(self, func, x_range, title=None):
        # Implementation
        pass
    
    def statistical_plot(self, data, plot_type):
        # Implementation
        pass
    
    # ... other methods
```

**Success Criteria**:
- ✅ Professional-looking plots with proper labels
- ✅ Support for customization (colors, styles, annotations)
- ✅ Export capabilities (PNG, PDF, SVG)
- ✅ Interactive features (zoom, pan, tooltips)

#### **Component 4: Documentation & Testing (15%)**
**Requirements**:
- Comprehensive documentation (README, API docs)
- Unit tests for all functions
- Usage examples and tutorials
- Performance benchmarks

**Deliverables**:
- README.md with installation and usage instructions
- API documentation with examples
- Test suite with >90% code coverage
- Performance benchmark report

**Success Criteria**:
- ✅ Clear, professional documentation
- ✅ All tests passing
- ✅ Examples working correctly
- ✅ Performance meets specified requirements

---

## 📊 **Assessment Rubric**

### **Quiz Scoring (100 points total)**
- **Mathematical Foundations**: 40 points
- **Programming & Environment**: 30 points
- **Problem Solving**: 30 points

**Grade Scale**:
- **A**: 90-100 points (Excellent)
- **B**: 80-89 points (Good)
- **C**: 70-79 points (Satisfactory)
- **D**: 60-69 points (Needs Improvement)
- **F**: <60 points (Unsatisfactory)

### **Project Scoring (100 points total)**
- **Linear Algebra Library**: 35 points
- **Statistics Calculator**: 30 points
- **Visualization Tools**: 20 points
- **Documentation & Testing**: 15 points

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
1. **Mathematical Computing**: Implement linear algebra and statistical operations
2. **Programming Skills**: Write clean, documented Python code
3. **Problem Solving**: Apply mathematical concepts to practical problems
4. **Visualization**: Create professional mathematical visualizations
5. **Software Engineering**: Follow best practices for documentation and testing

### **Prerequisites for Next Phase**
- ✅ Understanding of basic linear algebra concepts
- ✅ Proficiency with Python and NumPy
- ✅ Experience with mathematical visualization
- ✅ Foundation for advanced mathematical computing

---

## 📅 **Timeline & Milestones**

### **Week 1**
- **Day 1-2**: Complete quiz preparation and assessment
- **Day 3-4**: Set up development environment
- **Day 5-7**: Implement linear algebra library

### **Week 2**
- **Day 8-10**: Implement statistics calculator
- **Day 11-12**: Create visualization tools
- **Day 13-14**: Documentation, testing, and final submission

---

## 🚀 **Submission Guidelines**

### **Quiz Submission**
- **Format**: Written responses (PDF) + code solutions (Python files)
- **Deadline**: End of Week 1
- **Submission**: Upload to learning management system

### **Project Submission**
- **Format**: GitHub repository with complete code
- **Contents**: Source code, documentation, tests, examples
- **Deadline**: End of Week 2
- **Submission**: Repository link + demonstration video (5 minutes)

---

**🎯 This bi-weekly assessment provides comprehensive evaluation of mathematical foundations and programming skills, preparing students for advanced concepts in subsequent weeks.**
