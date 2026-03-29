# Weeks 3-4: Core Concepts - Quiz & Projects

## 📋 **Bi-Weekly Assessment Overview**
**Duration**: Weeks 3-4 (Core Concepts)
**Focus**: Advanced mathematics, data structures, algorithmic thinking
**Assessment Type**: Quiz + Practical Project

---

## 🧮 **Weeks 3-4 Quiz: Core Concepts Assessment**

### **Section 1: Advanced Mathematics (40%)**

#### **Question 1: Calculus Fundamentals**
**Problem**: For the function f(x) = x³ - 6x² + 9x + 1:
1. Find the first and second derivatives
2. Determine critical points and classify them
3. Find points of inflection
4. Sketch the complete function

**Scoring**: 10 points (2.5 points each)

#### **Question 2: Probability Theory**
**Problem**: A bag contains 5 red balls, 3 blue balls, and 2 green balls. Calculate:
1. Probability of drawing 2 red balls without replacement
2. Probability of drawing 1 red and 1 blue ball
3. Expected value of red balls in 3 draws
4. Variance of red balls in 3 draws

**Scoring**: 10 points (2.5 points each)

#### **Question 3: Linear Algebra Applications**
**Problem**: Given the system of equations:
2x + 3y - z = 7
x - y + 2z = 4
3x + 2y + z = 9

1. Solve using matrix methods
2. Find the inverse of the coefficient matrix
3. Calculate the determinant
4. Explain the geometric interpretation

**Scoring**: 10 points (2.5 points each)

#### **Question 4: Discrete Mathematics**
**Problem**: Prove by mathematical induction that:
1 + 2 + 3 + ... + n = n(n+1)/2

Also provide a combinatorial proof for the same formula.

**Scoring**: 10 points (5 points each proof)

### **Section 2: Data Structures & Algorithms (30%)**

#### **Question 5: Algorithm Analysis**
**Problem**: Analyze the time complexity of:
1. Binary search algorithm
2. Merge sort algorithm
3. Quick sort algorithm (average and worst case)
4. Dijkstra's shortest path algorithm

Provide Big-O notation for each and explain your reasoning.

**Scoring**: 10 points (2.5 points each)

#### **Question 6: Data Structure Design**
**Problem**: Design a data structure that supports:
1. Insert elements in O(1) time
2. Find minimum element in O(1) time
3. Delete minimum element in O(log n) time
4. Search for an element in O(log n) time

Explain your design and analyze all operations.

**Scoring**: 10 points

#### **Question 7: Graph Theory**
**Problem**: For the graph with vertices A, B, C, D, E and edges:
A-B (weight 3), A-C (weight 1), B-D (weight 4), C-D (weight 2), C-E (weight 5), D-E (weight 1)

1. Find the minimum spanning tree
2. Find the shortest path from A to E
3. Determine if the graph is Eulerian
4. Find the graph's chromatic number

**Scoring**: 10 points (2.5 points each)

### **Section 3: Problem Solving & Applications (30%)**

#### **Question 8: Mathematical Modeling**
**Problem**: A population of bacteria grows according to the model P(t) = P₀e^(rt), where P₀ = 1000 and r = 0.02 per hour.
1. Find the population after 24 hours
2. When will the population reach 1,000,000?
3. Calculate the doubling time
4. Graph the population growth

**Scoring**: 10 points (2.5 points each)

#### **Question 9: Optimization Problem**
**Problem**: A company produces two products A and B. Product A requires 2 hours of labor and 1 unit of material, selling for $10. Product B requires 1 hour of labor and 2 units of material, selling for $12. The company has 100 labor hours and 80 units of material available.
1. Formulate the linear programming problem
2. Solve using graphical method
3. Determine the optimal production quantities
4. Calculate maximum profit

**Scoring**: 10 points (2.5 points each)

#### **Question 10: Cryptography Application**
**Problem**: Implement and explain the Caesar cipher:
1. Write the encryption algorithm
2. Write the decryption algorithm
3. Analyze its security weaknesses
4. Suggest improvements

**Scoring**: 10 points (2.5 points each)

---

## 🚀 **Weeks 3-4 Project: Data Analysis Pipeline**

### **Project Overview**
**Objective**: Build comprehensive data analysis pipeline with statistical analysis and visualization
**Duration**: 2 weeks
**Technologies**: Python, Pandas, NumPy, Matplotlib, Seaborn, Scikit-learn

### **Project Components**

#### **Component 1: Data Collection & Cleaning (25%)**
**Requirements**:
- Import data from multiple sources (CSV, JSON, API)
- Handle missing values and outliers
- Data validation and quality checks
- Data transformation and normalization

**Deliverables**:
```python
class DataCollector:
    def __init__(self):
        self.data_sources = {}
        self.cleaning_pipeline = {}
    
    def import_data(self, source, format_type):
        # Implementation for CSV, JSON, API
        pass
    
    def clean_data(self, data, cleaning_rules):
        # Implementation for missing values, outliers
        pass
    
    def validate_data(self, data, schema):
        # Implementation for data validation
        pass
    
    def transform_data(self, data, transformations):
        # Implementation for normalization, encoding
        pass
```

**Success Criteria**:
- ✅ Support for multiple data formats
- ✅ Robust error handling for data issues
- ✅ Configurable cleaning rules
- ✅ Data quality reports

#### **Component 2: Statistical Analysis (30%)**
**Requirements**:
- Descriptive statistics and exploratory analysis
- Hypothesis testing and confidence intervals
- Correlation and regression analysis
- Distribution analysis and fitting

**Deliverables**:
```python
class StatisticalAnalyzer:
    def __init__(self):
        self.analysis_results = {}
    
    def descriptive_analysis(self, data):
        # Implementation for mean, median, std, etc.
        pass
    
    def hypothesis_testing(self, data1, data2, test_type):
        # Implementation for t-test, chi-square, etc.
        pass
    
    def correlation_analysis(self, data):
        # Implementation for correlation matrix
        pass
    
    def regression_analysis(self, X, y):
        # Implementation for linear regression
        pass
```

**Success Criteria**:
- ✅ Comprehensive statistical analysis
- ✅ Proper statistical tests with p-values
- ✅ Visualization of statistical results
- ✅ Interpretation of statistical significance

#### **Component 3: Data Visualization (25%)**
**Requirements**:
- Interactive dashboards with multiple plot types
- Customizable styling and themes
- Real-time data updates
- Export capabilities for reports

**Deliverables**:
```python
class DataVisualizer:
    def __init__(self):
        self.dashboard = {}
        self.plots = {}
    
    def create_dashboard(self, data, layout):
        # Implementation for interactive dashboard
        pass
    
    def statistical_plots(self, data, plot_types):
        # Implementation for histograms, box plots, etc.
        pass
    
    def correlation_heatmap(self, data):
        # Implementation for correlation visualization
        pass
    
    def time_series_plot(self, data, time_col):
        # Implementation for time series visualization
        pass
```

**Success Criteria**:
- ✅ Professional-looking visualizations
- ✅ Interactive features (zoom, filter, hover)
- ✅ Multiple plot types and layouts
- ✅ Responsive design for different screen sizes

#### **Component 4: Machine Learning Integration (20%)**
**Requirements**:
- Basic predictive modeling
- Model evaluation and validation
- Feature engineering and selection
- Model interpretation and explanation

**Deliverables**:
```python
class MLAnalyzer:
    def __init__(self):
        self.models = {}
        self.evaluations = {}
    
    def train_model(self, X, y, model_type):
        # Implementation for basic ML models
        pass
    
    def evaluate_model(self, model, X_test, y_test):
        # Implementation for model evaluation
        pass
    
    def feature_engineering(self, data):
        # Implementation for feature creation
        pass
    
    def interpret_model(self, model, feature_names):
        # Implementation for model interpretation
        pass
```

**Success Criteria**:
- ✅ Working predictive models
- ✅ Proper model evaluation metrics
- ✅ Feature importance analysis
- ✅ Model explanation capabilities

---

## 📊 **Assessment Rubric**

### **Quiz Scoring (100 points total)**
- **Advanced Mathematics**: 40 points
- **Data Structures & Algorithms**: 30 points
- **Problem Solving & Applications**: 30 points

**Grade Scale**:
- **A**: 90-100 points (Excellent)
- **B**: 80-89 points (Good)
- **C**: 70-79 points (Satisfactory)
- **D**: 60-69 points (Needs Improvement)
- **F**: <60 points (Unsatisfactory)

### **Project Scoring (100 points total)**
- **Data Collection & Cleaning**: 25 points
- **Statistical Analysis**: 30 points
- **Data Visualization**: 25 points
- **Machine Learning Integration**: 20 points

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
1. **Advanced Mathematics**: Apply calculus, probability, and linear algebra
2. **Data Structures**: Analyze and design efficient algorithms
3. **Data Analysis**: Process and analyze real-world datasets
4. **Statistical Thinking**: Apply statistical methods to data problems
5. **Machine Learning**: Build and evaluate basic predictive models

### **Prerequisites for Next Phase**
- ✅ Strong mathematical foundation
- ✅ Data analysis skills
- ✅ Algorithmic thinking
- ✅ Preparation for mathematical computing

---

## 📅 **Timeline & Milestones**

### **Week 3**
- **Day 15-16**: Complete quiz preparation and assessment
- **Day 17-19**: Implement data collection and cleaning
- **Day 20-21**: Develop statistical analysis components

### **Week 4**
- **Day 22-24**: Create data visualization tools
- **Day 25-26**: Integrate machine learning components
- **Day 27-28**: Testing, documentation, and final submission

---

## 🚀 **Submission Guidelines**

### **Quiz Submission**
- **Format**: Written responses (PDF) + code solutions (Python files)
- **Deadline**: End of Week 3
- **Submission**: Upload to learning management system

### **Project Submission**
- **Format**: GitHub repository with complete pipeline
- **Contents**: Source code, documentation, sample data, reports
- **Deadline**: End of Week 4
- **Submission**: Repository link + demonstration video (10 minutes)

---

**🎯 This bi-weekly assessment builds on foundational concepts to develop advanced mathematical thinking and data analysis skills.**
