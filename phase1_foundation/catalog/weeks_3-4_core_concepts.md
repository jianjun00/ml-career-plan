# Weeks 3-4: Core Concepts (March 15-28, 2026)

## 🎯 **Learning Objectives**
- Complete linear algebra fundamentals
- Master Python basics for mathematical computing
- Advance to probability theory in statistics
- Begin mathematical expression parser development
- Read and understand key Math LLM research papers

---

## 📚 **Content & Resources**

### **Advanced Linear Algebra**
**Resource**: [Khan Academy Linear Algebra](https://www.khanacademy.org/math/linear-algebra)
- **Specific Sections**:
  - [Alternate coordinate systems (bases)](https://www.khanacademy.org/math/linear-algebra/alternate-bases)
  - [Orthogonal complements](https://www.khanacademy.org/math/linear-algebra/orthogonality)
- **Time Commitment**: 1 hour/day, 3 days/week
- **Focus Areas**: Eigenvalues, eigenvectors, basis transformations

### **Harvard CS50 Programming**
**Resource**: [Harvard CS50](https://cs50.harvard.edu/x/2023/)
- **Specific Weeks**:
  - [Week 2: Arrays](https://cs50.harvard.edu/x/2023/weeks/2/)
  - [Week 3: Algorithms](https://cs50.harvard.edu/x/2023/weeks/3/)
- **Time Commitment**: 2 hours/day, 4 days/week
- **Focus**: Data structures, basic algorithms, memory management

### **Probability Theory**
**Resource**: [Khan Academy Statistics and Probability](https://www.khanacademy.org/math/statistics-probability)
- **Specific Sections**:
  - [Probability](https://www.khanacademy.org/math/statistics-probability/probability)
  - [Compound events and probability](https://www.khanacademy.org/math/statistics-probability/compound-events-and-probability)
- **Time Commitment**: 45 minutes/day, 3 days/week

### **Python for Mathematical Computing**
**Resource**: [Python Official Tutorial](https://docs.python.org/3/tutorial/)
- **Key Sections**:
  - [Data Structures](https://docs.python.org/3/tutorial/datastructures.html)
  - [Modules and Packages](https://docs.python.org/3/tutorial/modules.html)
  - [Input and Output](https://docs.python.org/3/tutorial/inputoutput.html)

**Mathematical Python Resources**:
- [Python for Scientific Computing](https://scipy-lectures.org/)
- [NumPy for Linear Algebra](https://numpy.org/doc/stable/user/numpy-for-matlab-users.html)
- [SymPy Tutorial](https://docs.sympy.org/latest/tutorials/index.html)

### **Research Papers**
**Paper 1**: [Toolformer: Language Models Can Teach Themselves to Use Tools](https://arxiv.org/abs/2302.04761)
- **Focus**: Tool integration in LLMs
- **Reading Schedule**: 
  - Day 1: Abstract, Introduction, Related Work
  - Day 2: Methodology, Experiments
  - Day 3: Results, Discussion, Future Work

**Paper 2**: [MathCoder: Seamless Code Integration in LLMs for Mathematical Reasoning](https://arxiv.org/abs/2410.01985)
- **Focus**: Mathematical reasoning capabilities
- **Reading Schedule**: Same 3-day approach

---

## 🧪 **Evaluation & Assessment**

### **Linear Algebra Evaluation**
**Quiz Links**:
- [Eigenvalues and Eigenvectors Quiz](https://www.khanacademy.org/math/linear-algebra/alternate-bases/eigen-everything/quiz/eigenvalues-eigenvectors-quiz)
- [Orthogonality Quiz](https://www.khanacademy.org/math/linear-algebra/orthogonality/quiz/orthogonality-quiz)

**Practical Assessment**:
- [ ] Compute eigenvalues for 3x3 matrix
- [ ] Find eigenvectors and verify properties
- [ ] Explain geometric interpretation
- [ ] Apply eigenvalues to real problems

**Success Criteria**:
- [ ] Score 85%+ on eigenvalues quiz
- [ ] Score 85%+ on orthogonality quiz
- [ ] Successfully compute eigenvalues for 3x3 matrix
- [ ] Explain geometric interpretation of eigenvectors

### **CS50 Programming Evaluation**
**Problem Sets**:
- [Problem Set 2: Arrays](https://cs50.harvard.edu/x/2023/psets/2/)
- [Problem Set 3: Algorithms](https://cs50.harvard.edu/x/2023/psets/3/)

**Success Criteria**:
- [ ] Complete Problem Set 2 with 90%+ score
- [ ] Complete Problem Set 3 with 85%+ score
- [ ] Implement bubble sort algorithm from scratch
- [ ] Create linked list data structure

### **Probability Theory Evaluation**
**Quiz Links**:
- [Probability Quiz](https://www.khanacademy.org/math/statistics-probability/probability/quiz/probability-quiz)
- [Compound Events Quiz](https://www.khanacademy.org/math/statistics-probability/compound-events-and-probability/quiz/compound-events-quiz)

**Practical Assessment**:
- [ ] Simulate coin flips and calculate probabilities
- [ ] Create probability distributions
- [ ] Test compound events
- [ ] Visualize probability outcomes

**Success Criteria**:
- [ ] Score 85%+ on probability quiz
- [ ] Score 85%+ on compound events quiz
- [ ] Simulate probability experiments
- [ ] Explain probability concepts
```

**Success Criteria**:
- [ ] Score 85%+ on probability quiz
- [ ] Score 80%+ on compound events quiz
- [ ] Simulate and analyze probability distributions
- [ ] Calculate conditional probabilities correctly

### **Research Comprehension Evaluation**
**Assessment Method**:
- Create comparison table of both papers
- Identify key techniques for mathematical reasoning
- Design simple experiment based on paper methodology

**Success Criteria**:
- [ ] Submit 300-word comparison to Discord
- [ ] Identify 5 key techniques total
- [ ] Receive feedback from 2+ community members

---

## 🛠️ **Projects & Applications**

### **Project 1: Mathematical Expression Parser**
**Objective**: Build basic parser for mathematical expressions using SymPy

**Steps**:
1. Study SymPy expression parsing
2. Implement basic arithmetic parser (+, -, *, /)
3. Add support for variables and functions
4. Create evaluation and simplification methods

**Code Structure**:
```python
from sympy import symbols, simplify, parse_expr
import numpy as np

class MathExpressionParser:
    def __init__(self):
        self.variables = {}
    
    def parse_expression(self, expr_str):
        """Parse mathematical expression string"""
        return parse_expr(expr_str, evaluate=False)
    
    def evaluate_expression(self, expr_str, variable_values=None):
        """Evaluate expression with given variable values"""
        expr = self.parse_expression(expr_str)
        if variable_values:
            subs_dict = {symbols(k): v for k, v in variable_values.items()}
            return expr.subs(subs_dict)
        return expr
    
    def simplify_expression(self, expr_str):
        """Simplify mathematical expression"""
        expr = self.parse_expression(expr_str)
        return simplify(expr)

# Test the parser
parser = MathExpressionParser()
result = parser.evaluate_expression("2*x + 3", {"x": 5})
print(f"2*5 + 3 = {result}")
```

**Deliverables**:
- [ ] Working parser class with basic operations
- [ ] Test suite with 10+ expressions
- [ ] Documentation of parser capabilities
- [ ] GitHub commit with project files

### **Project 2: Probability Simulation Tool**
**Objective**: Create interactive probability simulations for mathematical concepts

**Steps**:
1. Implement common probability distributions
2. Create visualization functions
3. Add interactive parameter controls
4. Include statistical analysis tools

**Code Structure**:
```python
import numpy as np
import matplotlib.pyplot as plt
from scipy import stats

class ProbabilitySimulator:
    def __init__(self):
        self.distributions = {
            'normal': stats.norm,
            'binomial': stats.binom,
            'poisson': stats.poisson
        }
    
    def simulate_distribution(self, dist_type, params, n_samples=1000):
        """Simulate probability distribution"""
        dist = self.distributions[dist_type]
        if dist_type == 'normal':
            samples = dist.rvs(params['mean'], params['std'], size=n_samples)
        elif dist_type == 'binomial':
            samples = dist.rvs(params['n'], params['p'], size=n_samples)
        elif dist_type == 'poisson':
            samples = dist.rvs(params['mu'], size=n_samples)
        return samples
    
    def plot_distribution(self, samples, title="Distribution"):
        """Plot probability distribution"""
        plt.figure(figsize=(10, 6))
        plt.hist(samples, bins=30, density=True, alpha=0.7)
        plt.title(title)
        plt.xlabel('Value')
        plt.ylabel('Probability Density')
        **Deliverables**:
- [ ] Complete simulator class
- [ ] Interactive Jupyter notebook
- [ ] Documentation of distributions
- [ ] Statistical analysis examples

---

## 📊 **Progress Tracking**

### **Daily Checklist**
- [ ] 45 minutes advanced linear algebra
- [ ] 1 hour CS50 programming
- [ ] 30 minutes probability theory
- [ ] 30 minutes research paper reading
- [ ] 15 minutes project development

### **Weekly Milestones**
**Week 3**:
- [ ] Complete alternate coordinate systems unit
- [ ] Complete CS50 Week 2 (Arrays)
- [ ] Complete basic probability unit
- [ ] Read Toolformer paper completely
- [ ] Start expression parser development

**Week 4**:
- [ ] Complete orthogonal complements unit
- [ ] Complete CS50 Week 3 (Algorithms)
- [ ] Complete compound events unit
- [ ] Read MathCoder paper completely
- [ ] Complete expression parser project

### **End-of-Period Assessment**
**Learning Outcomes**:
Upon successful completion, students will be able to:
1. **Advanced Mathematics**: Master alternate coordinate systems, orthogonal complements, and compound events
2. **Programming**: Complete CS50 algorithms and data structures with high performance
3. **Probability Theory**: Apply probability distributions, statistical inference, and simulation
4. **Mathematical Parsing**: Build expression parsers with symbolic computation capabilities
5. **Research**: Understand Toolformer and AutoMath methodologies
6. **Integration**: Combine mathematical knowledge with programming skills

### **Prerequisites for Next Phase**
- ✅ Strong foundation in advanced mathematics
- ✅ Proficiency in algorithms and data structures
- ✅ Experience with probability and statistics
- ✅ Mathematical programming and parsing skills
- ✅ Understanding of modern LLM research trends

---

## 🚀 **Next Period Preparation**

### **Weeks 5-6 Preview**
- Complete Harvard CS50 Week 4-5
- Build advanced tokenizer implementation
- Start StatQuest statistics videos
- Join additional LLM community
- Read AutoMath research paper

### **Resources to Preview**:
- [Hugging Face Tokenizers](https://huggingface.co/docs/tokenizers/index)
- [StatQuest YouTube Channel](https://www.youtube.com/c/statquest)
- [AutoMath Paper](https://arxiv.org/abs/2402.07145)
- [Reddit r/LocalLLaMA](https://www.reddit.com/r/LocalLLaMA)

---

## 📞 **Support & Resources**

### **Help Channels**:
- CS50 Ed Discussion Forum
- OpenAI Discord: #research and #technical-help
- Learn AI Together: #math-help and #programming
- Stack Overflow for specific programming questions

### **Additional Resources**:
- [Linear Algebra Done Right](https://linear.axler.net/) - Reference book
- [Algorithm Design Manual](https://www.algorist.com/) - Algorithm reference
- [Think Stats](https://greenteapress.com/wp/think-stats-2e/) - Probability reference
- [Effective Python](https://effectivepython.com/) - Python best practices

---

*This 2-week plan advances core concepts with comprehensive resources, hands-on projects, and rigorous evaluation methods for Math LLM development.*
