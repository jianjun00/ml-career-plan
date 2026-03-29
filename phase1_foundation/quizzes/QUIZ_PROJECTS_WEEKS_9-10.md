# Weeks 9-10: Advanced Mathematics - Quiz & Projects

## 📋 **Bi-Weekly Assessment Overview**
**Duration**: Weeks 9-10 (Advanced Mathematics)
**Focus**: Advanced mathematical concepts, optimization, statistical inference
**Assessment Type**: Quiz + Practical Project

---

## 🧮 **Weeks 9-10 Quiz: Advanced Mathematics Assessment**

### **Section 1: Advanced Calculus (40%)**

#### **Question 1: Multivariable Calculus**
**Problem**: For the function f(x,y) = x²y + xy² - 2x - 3y:
1. Find partial derivatives ∂f/∂x and ∂f/∂y
2. Find critical points and classify them
3. Find the gradient and Hessian matrix
4. Determine if the function is convex

**Scoring**: 10 points (2.5 points each component)

#### **Question 2: Vector Calculus**
**Problem**: For the vector field F(x,y,z) = (xy, yz, zx):
1. Compute the divergence ∇·F
2. Compute the curl ∇×F
3. Find the potential function if it exists
4. Evaluate the line integral along a specified path

**Scoring**: 10 points (2.5 points each component)

#### **Question 3: Differential Equations**
**Problem**: Solve the following differential equations:
1. dy/dx = 2x + 3y (first-order linear)
2. d²y/dx² + 4dy/dx + 4y = 0 (second-order homogeneous)
3. dy/dx = y(1-y) (logistic equation)
4. System of differential equations with matrix methods

**Scoring**: 10 points (2.5 points each equation)

#### **Question 4: Optimization Theory**
**Problem**: Apply optimization methods to:
1. Find minimum of f(x) = x⁴ - 3x² + 2 using gradient descent
2. Apply Lagrange multipliers to constrained optimization
3. Implement Newton's method for optimization
4. Compare convergence rates of different methods

**Scoring**: 10 points (2.5 points each method)

### **Section 2: Advanced Statistics (30%)**

#### **Question 5: Statistical Inference**
**Problem**: Design and analyze:
1. Confidence intervals for population parameters
2. Hypothesis testing procedures
3. Power analysis for statistical tests
4. Bayesian inference methods

**Scoring**: 10 points (2.5 points each component)

#### **Question 6: Probability Distributions**
**Problem**: Analyze and apply:
1. Multivariate normal distributions
2. Exponential family distributions
3. Sampling methods (Monte Carlo, MCMC)
4. Distribution fitting and parameter estimation

**Scoring**: 10 points (2.5 points each distribution)

#### **Question 7: Regression Analysis**
**Problem**: Implement and evaluate:
1. Multiple linear regression with matrix methods
2. Polynomial regression and regularization
3. Logistic regression for classification
4. Model selection and validation techniques

**Scoring**: 10 points (2.5 points each regression type)

### **Section 3: Mathematical Applications (30%)**

#### **Question 8: Information Theory**
**Problem**: Apply information theory concepts:
1. Calculate entropy and mutual information
2. Implement KL divergence calculations
3. Design cross-entropy loss functions
4. Apply to machine learning optimization

**Scoring**: 10 points (2.5 points each concept)

#### **Question 9: Graph Theory**
**Problem**: Solve graph theory problems:
1. Find shortest paths using Dijkstra's algorithm
2. Apply minimum spanning tree algorithms
3. Solve network flow problems
4. Analyze graph properties and connectivity

**Scoring**: 10 points (2.5 points each problem)

#### **Question 10: Numerical Analysis**
**Problem**: Implement numerical methods:
1. Interpolation and extrapolation methods
2. Numerical integration techniques
3. Root finding algorithms
4. Error analysis and convergence

**Scoring**: 10 points (2.5 points each method)

---

## 🚀 **Weeks 9-10 Project: Advanced Mathematical Computing**

### **Project Overview**
**Objective**: Build advanced mathematical computing toolkit with optimization and statistical analysis
**Duration**: 2 weeks
**Technologies**: Python, NumPy, SciPy, Matplotlib, Scikit-learn

### **Project Components**

#### **Component 1: Optimization Library (30%)**
**Requirements**:
- Gradient descent with adaptive learning rates
- Newton's method and quasi-Newton methods
- Constrained optimization with Lagrange multipliers
- Global optimization methods (simulated annealing, genetic algorithms)

**Deliverables**:
```python
import numpy as np
from scipy.optimize import minimize
from typing import Callable, Tuple, Dict, Any

class OptimizationLibrary:
    def __init__(self):
        self.methods = {}
        self.convergence_history = {}
    
    def gradient_descent(self, f: Callable, df: Callable, x0: np.ndarray, 
                       learning_rate: float = 0.01, max_iter: int = 1000,
                       tol: float = 1e-6) -> Tuple[np.ndarray, Dict]:
        """Gradient descent optimization with adaptive learning rate"""
        x = x0.copy()
        history = {'x': [x.copy()], 'f': [f(x)], 'grad_norm': []}
        
        for i in range(max_iter):
            grad = df(x)
            grad_norm = np.linalg.norm(grad)
            history['grad_norm'].append(grad_norm)
            
            # Adaptive learning rate
            if i > 0 and history['f'][-1] > history['f'][-2]:
                learning_rate *= 0.5
            
            x = x - learning_rate * grad
            
            history['x'].append(x.copy())
            history['f'].append(f(x))
            
            if grad_norm < tol:
                break
        
        return x, history
    
    def newton_method(self, f: Callable, df: Callable, d2f: Callable, 
                     x0: np.ndarray, max_iter: int = 100,
                     tol: float = 1e-6) -> Tuple[np.ndarray, Dict]:
        """Newton's method for optimization"""
        x = x0.copy()
        history = {'x': [x.copy()], 'f': [f(x)]}
        
        for i in range(max_iter):
            grad = df(x)
            hess = d2f(x)
            
            # Newton step
            try:
                step = np.linalg.solve(hess, grad)
            except np.linalg.LinAlgError:
                # Add regularization if Hessian is singular
                step = np.linalg.solve(hess + 1e-6 * np.eye(len(x)), grad)
            
            x = x - step
            
            history['x'].append(x.copy())
            history['f'].append(f(x))
            
            if np.linalg.norm(grad) < tol:
                break
        
        return x, history
    
    def constrained_optimization(self, f: Callable, constraints: list, 
                             x0: np.ndarray, method: str = 'SLSQP') -> Dict:
        """Constrained optimization using scipy"""
        result = minimize(f, x0, method=method, constraints=constraints)
        return {
            'x': result.x,
            'f': result.fun,
            'success': result.success,
            'message': result.message,
            'iterations': result.nit
        }
    
    def global_optimization(self, f: Callable, bounds: list, 
                         method: str = 'differential_evolution') -> Dict:
        """Global optimization methods"""
        if method == 'differential_evolution':
            from scipy.optimize import differential_evolution
            result = differential_evolution(f, bounds)
        elif method == 'basinhopping':
            from scipy.optimize import basinhopping
            result = basinhopping(f, x0=(bounds[0][1] + bounds[0][0]) / 2)
        
        return {
            'x': result.x,
            'f': result.fun,
            'success': result.success
        }
```

**Success Criteria**:
- ✅ Correct implementation of optimization algorithms
- ✅ Convergence to correct solutions
- ✅ Proper handling of edge cases
- ✅ Performance comparison and analysis

#### **Component 2: Statistical Analysis Suite (25%)**
**Requirements**:
- Advanced statistical inference methods
- Bayesian analysis and MCMC sampling
- Distribution fitting and parameter estimation
- Hypothesis testing with power analysis

**Deliverables**:
```python
import numpy as np
from scipy import stats
from scipy.stats import norm, ttest_ind, chi2_contingency
import matplotlib.pyplot as plt

class AdvancedStatistics:
    def __init__(self):
        self.distributions = {}
        self.inference_results = {}
    
    def confidence_interval(self, data, confidence=0.95):
        """Calculate confidence interval for population parameters"""
        n = len(data)
        mean = np.mean(data)
        std_err = stats.sem(data)
        
        # t-distribution for small samples
        if n < 30:
            t_val = stats.t.ppf((1 + confidence) / 2, n - 1)
        else:
            t_val = stats.norm.ppf((1 + confidence) / 2)
        
        margin_error = t_val * std_err
        
        return {
            'mean': mean,
            'lower': mean - margin_error,
            'upper': mean + margin_error,
            'confidence': confidence
        }
    
    def hypothesis_test(self, data1, data2, test_type='two_sample_t'):
        """Perform hypothesis testing with power analysis"""
        if test_type == 'two_sample_t':
            statistic, p_value = ttest_ind(data1, data2)
            effect_size = (np.mean(data1) - np.mean(data2)) / np.sqrt(
                (np.var(data1) + np.var(data2)) / 2
            )
        
        # Power analysis
        from statsmodels.stats.power import TTestIndPower
        power_analysis = TTestIndPower()
        power = power_analysis.power(
            effect_size=effect_size,
            nobs1=len(data1),
            alpha=0.05,
            ratio=len(data2)/len(data1)
        )
        
        return {
            'statistic': statistic,
            'p_value': p_value,
            'effect_size': effect_size,
            'power': power,
            'significant': p_value < 0.05
        }
    
    def bayesian_inference(self, data, prior_type='conjugate'):
        """Bayesian inference with conjugate priors"""
        if prior_type == 'conjugate':
            # Normal-normal conjugate
            prior_mean = 0
            prior_var = 1
            
            n = len(data)
            sample_mean = np.mean(data)
            sample_var = np.var(data)
            
            # Posterior parameters
            posterior_mean = (prior_mean/prior_var + n*sample_mean/sample_var) / \
                           (1/prior_var + n/sample_var)
            posterior_var = 1 / (1/prior_var + n/sample_var)
            
            return {
                'posterior_mean': posterior_mean,
                'posterior_var': posterior_var,
                'posterior_std': np.sqrt(posterior_var)
            }
    
    def mcmc_sampling(self, log_likelihood, log_prior, initial_params, 
                     n_samples=10000, burn_in=1000):
        """MCMC sampling using Metropolis-Hastings"""
        params = initial_params.copy()
        samples = []
        
        for i in range(n_samples + burn_in):
            # Propose new parameters
            proposal = params + np.random.normal(0, 0.1, size=params.shape)
            
            # Calculate acceptance ratio
            log_proposal = log_likelihood(proposal) + log_prior(proposal)
            log_current = log_likelihood(params) + log_prior(params)
            
            alpha = min(1, np.exp(log_proposal - log_current))
            
            # Accept or reject
            if np.random.random() < alpha:
                params = proposal
            
            if i >= burn_in:
                samples.append(params.copy())
        
        return np.array(samples)
```

**Success Criteria**:
- ✅ Correct statistical computations
- ✅ Proper uncertainty quantification
- ✅ Efficient MCMC sampling
- ✅ Clear interpretation of results

#### **Component 3: Information Theory Tools (20%)**
**Requirements**:
- Entropy and mutual information calculations
- KL divergence and cross-entropy computations
- Information bottleneck analysis
- Applications to machine learning

**Deliverables**:
```python
import numpy as np
from scipy.stats import entropy

class InformationTheory:
    def __init__(self):
        self.distributions = {}
    
    def entropy(self, p):
        """Calculate Shannon entropy"""
        p = np.array(p)
        p = p[p > 0]  # Remove zero probabilities
        return -np.sum(p * np.log2(p))
    
    def mutual_information(self, p_xy):
        """Calculate mutual information from joint distribution"""
        p_x = np.sum(p_xy, axis=1)
        p_y = np.sum(p_xy, axis=0)
        
        mi = 0
        for i in range(len(p_x)):
            for j in range(len(p_y)):
                if p_xy[i, j] > 0:
                    mi += p_xy[i, j] * np.log2(p_xy[i, j] / (p_x[i] * p_y[j]))
        
        return mi
    
    def kl_divergence(self, p, q):
        """Calculate Kullback-Leibler divergence"""
        p = np.array(p)
        q = np.array(q)
        
        # Ensure no zero probabilities in denominator
        q = np.where(q == 0, 1e-10, q)
        
        return np.sum(p * np.log2(p / q))
    
    def cross_entropy(self, p, q):
        """Calculate cross-entropy"""
        p = np.array(p)
        q = np.array(q)
        
        q = np.where(q == 0, 1e-10, q)
        
        return -np.sum(p * np.log2(q))
    
    def information_bottleneck(self, X, Y, n_clusters):
        """Information bottleneck analysis"""
        from sklearn.cluster import KMeans
        
        # Cluster X to create compressed representation
        kmeans = KMeans(n_clusters=n_clusters)
        Z = kmeans.fit_predict(X)
        
        # Calculate mutual informations
        I_XZ = self.mutual_information_xy(X, Z)
        I_ZY = self.mutual_information_xy(Z, Y)
        
        return {
            'I(X;Z)': I_XZ,
            'I(Z;Y)': I_ZY,
            'bottleneck_ratio': I_ZY / I_XZ if I_XZ > 0 else 0
        }
```

**Success Criteria**:
- ✅ Correct information theory calculations
- ✅ Efficient computation for large datasets
- ✅ Applications to machine learning problems
- ✅ Visualization of information measures

#### **Component 4: Mathematical Modeling (25%)**
**Requirements**:
- Differential equation solvers
- Graph algorithms and network analysis
- Numerical analysis methods
- Mathematical modeling applications

**Deliverables**:
```python
import numpy as np
from scipy.integrate import odeint
from scipy.sparse import csr_matrix
from scipy.sparse.csgraph import shortest_path

class MathematicalModeling:
    def __init__(self):
        self.models = {}
        self.solutions = {}
    
    def ode_solver(self, f, y0, t, method='RK45'):
        """Solve ordinary differential equations"""
        from scipy.integrate import solve_ivp
        
        if method == 'RK45':
            solution = solve_ivp(f, [t[0], t[-1]], y0, 
                              t_eval=t, method='RK45')
        elif method == 'BDF':
            solution = solve_ivp(f, [t[0], t[-1]], y0, 
                              t_eval=t, method='BDF')
        
        return {
            't': solution.t,
            'y': solution.y,
            'success': solution.success,
            'message': solution.message
        }
    
    def graph_algorithms(self, adjacency_matrix):
        """Implement graph algorithms"""
        # Convert to sparse matrix for efficiency
        sparse_adj = csr_matrix(adjacency_matrix)
        
        # Shortest paths
        distances = shortest_path(sparse_adj, method='dijkstra')
        
        # Graph properties
        n_nodes = adjacency_matrix.shape[0]
        degree = np.sum(adjacency_matrix, axis=1)
        clustering_coeff = self.clustering_coefficient(adjacency_matrix)
        
        return {
            'distances': distances,
            'degree': degree,
            'clustering_coefficient': clustering_coeff,
            'n_nodes': n_nodes
        }
    
    def clustering_coefficient(self, adjacency_matrix):
        """Calculate clustering coefficient"""
        n = adjacency_matrix.shape[0]
        clustering = np.zeros(n)
        
        for i in range(n):
            neighbors = np.where(adjacency_matrix[i] > 0)[0]
            k = len(neighbors)
            
            if k >= 2:
                # Count edges between neighbors
                neighbor_edges = 0
                for j in range(k):
                    for l in range(j+1, k):
                        if adjacency_matrix[neighbors[j], neighbors[l]] > 0:
                            neighbor_edges += 1
                
                clustering[i] = 2 * neighbor_edges / (k * (k - 1))
        
        return clustering
    
    def numerical_integration(self, f, a, b, n=1000, method='simpson'):
        """Numerical integration methods"""
        x = np.linspace(a, b, n+1)
        y = f(x)
        
        if method == 'simpson':
            # Simpson's rule
            h = (b - a) / n
            integral = y[0] + y[-1] + 4 * np.sum(y[1:-1:2]) + 2 * np.sum(y[2:-2:2])
            integral *= h / 3
        elif method == 'trapezoid':
            # Trapezoidal rule
            integral = np.trapz(y, x)
        
        return integral
```

**Success Criteria**:
- ✅ Accurate numerical solutions
- ✅ Efficient graph algorithms
- ✅ Proper error handling
- ✅ Applications to real-world problems

---

## 📊 **Assessment Rubric**

### **Quiz Scoring (100 points total)**
- **Advanced Calculus**: 40 points
- **Advanced Statistics**: 30 points
- **Mathematical Applications**: 30 points

**Grade Scale**:
- **A**: 90-100 points (Excellent)
- **B**: 80-89 points (Good)
- **C**: 70-79 points (Satisfactory)
- **D**: 60-69 points (Needs Improvement)
- **F**: <60 points (Unsatisfactory)

### **Project Scoring (100 points total)**
- **Optimization Library**: 30 points
- **Statistical Analysis Suite**: 25 points
- **Information Theory Tools**: 20 points
- **Mathematical Modeling**: 25 points

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
1. **Advanced Mathematics**: Apply multivariable calculus and optimization
2. **Statistical Inference**: Perform advanced statistical analysis
3. **Information Theory**: Apply information-theoretic concepts
4. **Mathematical Modeling**: Build and solve mathematical models
5. **Computational Methods**: Implement numerical algorithms efficiently

### **Prerequisites for Next Phase**
- ✅ Strong mathematical foundation
- ✅ Advanced computational skills
- ✅ Statistical analysis expertise
- ✅ Preparation for machine learning applications

---

## 📅 **Timeline & Milestones**

### **Week 9**
- **Day 57-58**: Complete quiz preparation and assessment
- **Day 59-61**: Implement optimization library
- **Day 62-63**: Develop statistical analysis suite

### **Week 10**
- **Day 64-66**: Create information theory tools
- **Day 67-69**: Implement mathematical modeling components
- **Day 70**: Testing, documentation, and final submission

---

## 🚀 **Submission Guidelines**

### **Quiz Submission**
- **Format**: Written responses (PDF) + code solutions (Python files)
- **Deadline**: End of Week 9
- **Submission**: Upload to learning management system

### **Project Submission**
- **Format**: GitHub repository with complete toolkit
- **Contents**: Source code, documentation, examples, benchmarks
- **Deadline**: End of Week 10
- **Submission**: Repository link + demonstration video (15 minutes)

---

**🎯 This bi-weekly assessment develops advanced mathematical computing skills, preparing students for machine learning and AI applications.**
