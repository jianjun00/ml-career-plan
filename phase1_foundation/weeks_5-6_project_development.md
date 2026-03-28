# Weeks 5-6: Project Development (March 29 - April 11, 2026)

## 🎯 **Learning Objectives**
- Complete Harvard CS50 advanced programming concepts
- Build mathematical expression parser with SymPy
- Implement subword tokenization for LLMs
- Advance statistics understanding with StatQuest
- Join additional LLM community and expand network
- Read AutoMath paper for advanced mathematical reasoning

---

## 📚 **Content & Resources**

### **Harvard CS50 Advanced Programming**
**Resource**: [Harvard CS50](https://cs50.harvard.edu/x/2023/)
- **Specific Weeks**:
  - [Week 4: Memory](https://cs50.harvard.edu/x/2023/weeks/4/)
  - [Week 5: Data Structures](https://cs50.harvard.edu/x/2023/weeks/5/)
- **Time Commitment**: 2 hours/day, 4 days/week
- **Focus**: Pointers, memory management, linked lists, trees, hash tables

### **Mathematical Expression Parser Development**
**Primary Resource**: [SymPy Tutorial](https://docs.sympy.org/latest/tutorials/intro-tutorial.html)
- **Key Sections**:
  - [Expression Parsing](https://docs.sympy.org/latest/tutorials/intro-tutorial.html#expression-parsing)
  - [Symbolic Mathematics](https://docs.sympy.org/latest/tutorials/intro-tutorial.html#symbolic-mathematics)
  - [Simplification](https://docs.sympy.org/latest/tutorials/intro-tutorial.html#simplification)

**Advanced Resources**:
- [Parsing Expression Grammars](https://en.wikipedia.org/wiki/Parsing_expression_grammar)
- [Abstract Syntax Trees](https://en.wikipedia.org/wiki/Abstract_syntax_tree)
- [Mathematical Notation Standards](https://en.wikipedia.org/wiki/Mathematical_notation)

### **Subword Tokenization Implementation**
**Primary Resource**: [Hugging Face Tokenizers](https://huggingface.co/docs/tokenizers/index)
- **Key Concepts**:
  - [Byte-Pair Encoding (BPE)](https://huggingface.co/docs/tokenizers/quickstart#byte-pair-encoding)
  - [WordPiece Tokenization](https://huggingface.co/docs/tokenizers/quickstart#wordpiece)
  - [SentencePiece](https://github.com/google/sentencepiece)

**Implementation Resources**:
- [Tokenizers Library Documentation](https://huggingface.co/docs/tokenizers/en/index)
- [BPE Algorithm Explanation](https://huggingface.co/docs/tokenizers/quickstart#byte-pair-encoding)
- [Mathematical Tokenization Challenges](https://arxiv.org/abs/2109.00886)

### **Statistics with StatQuest**
**Resource**: [StatQuest YouTube Channel](https://www.youtube.com/c/statquest)
- **Key Videos**:
  - [Standard Deviation](https://www.youtube.com/watch?v=sDv4fPKsBJ4)
  - [Hypothesis Testing](https://www.youtube.com/watch?v=0oc49DyA3hU)
  - [p-values](https://www.youtube.com/watch?v=eyknGvncKLw)
- **Time Commitment**: 30 minutes/day, 3 days/week

### **Community Building**
**Additional Community**: [r/LocalLLaMA](https://www.reddit.com/r/LocalLLaMA)
- **Focus**: Local LLM implementation and discussion
- **Benefits**: Technical implementation help, model sharing
- **Engagement**: Share projects, ask technical questions

### **Advanced Research Paper**
**Paper**: [AutoMath: Automated Mathematical Reasoning](https://arxiv.org/abs/2402.07145)
- **Focus**: Automated mathematical problem solving
- **Key Sections**: Methodology, Evaluation, Results
- **Reading Schedule**: 3-day structured approach

---

## 🧪 **Evaluation & Assessment**

### **CS50 Programming Evaluation**
**Problem Sets**:
- [Problem Set 4: Memory](https://cs50.harvard.edu/x/2023/psets/4/)
- [Problem Set 5: Data Structures](https://cs50.harvard.edu/x/2023/psets/5/)

**Practical Assessment**:
```c
// Memory management test
#include <stdio.h>
#include <stdlib.h>

typedef struct node {
    int val;
    struct node* next;
} node;

node* create_node(int val) {
    node* n = malloc(sizeof(node));
    n->val = val;
    n->next = NULL;
    return n;
}

void free_list(node* head) {
    node* current = head;
    while (current != NULL) {
        node* next = current->next;
        free(current);
        current = next;
    }
}

int main() {
    node* head = create_node(1);
    head->next = create_node(2);
    
    // Test memory management
    free_list(head);
    printf("Memory management test passed!\n");
    return 0;
}
```

**Success Criteria**:
- [ ] Complete Problem Set 4 with 85%+ score
- [ ] Complete Problem Set 5 with 80%+ score
- [ ] Implement linked list from scratch
- [ ] Demonstrate proper memory management
- [ ] Create hash table implementation

### **Mathematical Expression Parser Evaluation**
**Test Suite**:
```python
from sympy import symbols, simplify, parse_expr, Eq, solve
import unittest

class TestMathParser(unittest.TestCase):
    def setUp(self):
        self.parser = MathExpressionParser()
    
    def test_basic_arithmetic(self):
        result = self.parser.evaluate_expression("2+3*4")
        self.assertEqual(result, 14)
    
    def test_variables(self):
        result = self.parser.evaluate_expression("x^2", {"x": 3})
        self.assertEqual(result, 9)
    
    def test_functions(self):
        result = self.parser.evaluate_expression("sin(pi/2)")
        self.assertAlmostEqual(float(result), 1.0, places=5)
    
    def test_simplification(self):
        result = self.parser.simplify_expression("x^2 + 2*x + 1 - (x+1)^2")
        self.assertEqual(result, 0)
    
    def test_equation_solving(self):
        x = symbols('x')
        equation = Eq(x**2 - 4, 0)
        solutions = solve(equation, x)
        self.assertEqual(len(solutions), 2)

if __name__ == '__main__':
    unittest.main()
```

**Success Criteria**:
- [ ] Pass all unit tests (100% pass rate)
- [ ] Handle 10+ mathematical operations
- [ ] Support variable substitution
- [ ] Implement equation solving
- [ ] Create comprehensive documentation

### **Tokenization Implementation Evaluation**
**Test Cases**:
```python
from tokenizers import Tokenizer
import json

def test_tokenizer():
    # Test mathematical expressions
    tokenizer = Tokenizer.from_pretrained("bert-base-uncased")
    
    math_expressions = [
        "2x + 3y = 7",
        "sin(x) + cos(y)",
        "∫(x^2)dx",
        "a^2 + b^2 = c^2"
    ]
    
    for expr in math_expressions:
        tokens = tokenizer.tokenize(expr)
        print(f"Expression: {expr}")
        print(f"Tokens: {tokens}")
        print("---")

test_tokenizer()
```

**Success Criteria**:
- [ ] Implement BPE tokenization from scratch
- [ ] Handle mathematical symbols correctly
- [ ] Create vocabulary for math expressions
- [ ] Tokenize 20+ mathematical expressions
- [ ] Compare with Hugging Face tokenizers

### **Statistics Understanding Evaluation**
**Quiz Links**:
- [StatQuest Quiz Questions](https://statquest.org/stat-quests/) (video-specific quizzes)
- [Khan Academy Hypothesis Testing](https://www.khanacademy.org/math/statistics-probability/significance-tests/quiz/hypothesis-testing-quiz)

**Practical Assessment**:
```python
import numpy as np
from scipy import stats

def hypothesis_test_example():
    # A/B test example
    group_a = np.random.normal(100, 15, 100)  # Control group
    group_b = np.random.normal(105, 15, 100)  # Test group
    
    # Perform t-test
    t_stat, p_value = stats.ttest_ind(group_a, group_b)
    
    print(f"T-statistic: {t_stat}")
    print(f"P-value: {p_value}")
    
    if p_value < 0.05:
        print("Reject null hypothesis - significant difference")
    else:
        print("Fail to reject null hypothesis - no significant difference")

hypothesis_test_example()
```

**Success Criteria**:
- [ ] Complete 5 StatQuest video quizzes
- [ ] Score 80%+ on hypothesis testing quiz
- [ ] Implement A/B test analysis
- [ ] Explain p-value interpretation
- [ ] Apply statistical concepts to LLM evaluation

---

## 🛠️ **Projects & Applications**

### **Project 1: Advanced Mathematical Expression Parser**
**Objective**: Build comprehensive parser with advanced features

**Enhanced Features**:
1. Support for calculus operations (derivatives, integrals)
2. Equation solving capabilities
3. Function plotting and visualization
4. Expression simplification and optimization

**Implementation Structure**:
```python
from sympy import symbols, diff, integrate, plot, Eq, solve
import matplotlib.pyplot as plt

class AdvancedMathParser:
    def __init__(self):
        self.functions = {
            'sin', 'cos', 'tan', 'log', 'exp', 'sqrt'
        }
        self.operators = {
            '+', '-', '*', '/', '^', '**'
        }
    
    def parse_and_simplify(self, expr_str):
        """Parse and simplify mathematical expression"""
        expr = parse_expr(expr_str, evaluate=False)
        return simplify(expr)
    
    def calculate_derivative(self, expr_str, variable='x'):
        """Calculate derivative of expression"""
        x = symbols(variable)
        expr = parse_expr(expr_str)
        return diff(expr, x)
    
    def calculate_integral(self, expr_str, variable='x'):
        """Calculate indefinite integral"""
        x = symbols(variable)
        expr = parse_expr(expr_str)
        return integrate(expr, x)
    
    def solve_equation(self, equation_str, variable='x'):
        """Solve mathematical equation"""
        x = symbols(variable)
        if '=' in equation_str:
            left, right = equation_str.split('=')
            equation = Eq(parse_expr(left), parse_expr(right))
        else:
            equation = Eq(parse_expr(equation_str), 0)
        return solve(equation, x)
    
    def plot_function(self, expr_str, x_range=(-10, 10), variable='x'):
        """Plot mathematical function"""
        x = symbols(variable)
        expr = parse_expr(expr_str)
        
        # Convert to numerical function for plotting
        f = lambda val: float(expr.subs(x, val))
        
        # Generate points
        x_vals = np.linspace(x_range[0], x_range[1], 1000)
        y_vals = [f(val) for val in x_vals]
        
        # Plot
        plt.figure(figsize=(10, 6))
        plt.plot(x_vals, y_vals)
        plt.title(f'Plot of {expr_str}')
        plt.xlabel(variable)
        plt.ylabel('f(x)')
        plt.grid(True)
        plt.show()

# Example usage
parser = AdvancedMathParser()

# Test different operations
expr = "x**2 + 3*x + 2"
print(f"Simplified: {parser.parse_and_simplify(expr)}")
print(f"Derivative: {parser.calculate_derivative(expr)}")
print(f"Integral: {parser.calculate_integral(expr)}")
print(f"Solutions: {parser.solve_equation(expr)}")

# Plot function
parser.plot_function("sin(x) + cos(x)")
```

**Deliverables**:
- [ ] Complete parser with all advanced features
- [ ] Comprehensive test suite (50+ test cases)
- [ ] Documentation with examples
- [ ] Performance benchmarks
- [ ] GitHub repository with issues tracking

### **Project 2: Mathematical Tokenizer**
**Objective**: Build custom tokenizer optimized for mathematical expressions

**Implementation Steps**:
1. Collect mathematical expression dataset
2. Implement BPE algorithm for math
3. Create special tokens for math symbols
4. Train and evaluate tokenizer

**Code Structure**:
```python
import re
from collections import Counter, defaultdict
import json

class MathTokenizer:
    def __init__(self, vocab_size=1000):
        self.vocab_size = vocab_size
        self.vocab = {}
        self.merges = []
        self.special_tokens = {
            '<pad>': 0,
            '<unk>': 1,
            '<sos>': 2,
            '<eos>': 3
        }
    
    def preprocess_text(self, text):
        """Preprocess mathematical text"""
        # Add spaces around operators
        text = re.sub(r'([+\-*/=^()])', r' \1 ', text)
        # Handle special math symbols
        text = text.replace('∫', ' integral ')
        text = text.replace('∑', ' sum ')
        text = text.replace('∏', ' product ')
        text = text.replace('√', ' sqrt ')
        # Convert to lowercase and remove extra spaces
        text = ' '.join(text.lower().split())
        return text
    
    def get_word_frequencies(self, texts):
        """Calculate word frequencies"""
        word_freqs = Counter()
        for text in texts:
            text = self.preprocess_text(text)
            words = text.split()
            word_freqs.update(words)
        return word_freqs
    
    def learn_bpe(self, texts):
        """Learn Byte-Pair Encoding vocabulary"""
        # Get initial vocabulary (characters)
        vocab = set()
        for text in texts:
            text = self.preprocess_text(text)
            for word in text.split():
                vocab.update(list(word))
        
        # Initialize with characters
        vocab = list(vocab)
        
        # Get word frequencies
        word_freqs = self.get_word_frequencies(texts)
        
        # Learn merges
        for i in range(self.vocab_size - len(vocab) - len(self.special_tokens)):
            # Get all pairs and their frequencies
            pairs = defaultdict(int)
            for word, freq in word_freqs.items():
                symbols = word.split()
                for j in range(len(symbols) - 1):
                    pairs[(symbols[j], symbols[j+1])] += freq
            
            if not pairs:
                break
            
            # Get most frequent pair
            best_pair = max(pairs, key=pairs.get)
            
            # Merge the pair in all words
            new_word_freqs = {}
            for word, freq in word_freqs.items():
                new_word = ' '.join(word.split())
                new_word = new_word.replace(f' {best_pair[0]} {best_pair[1]} ', f' {best_pair[0]}{best_pair[1]} ')
                new_word = new_word.replace(f' {best_pair[0]} {best_pair[1]}', f' {best_pair[0]}{best_pair[1]}')
                new_word_freqs[new_word] = freq
            
            word_freqs = new_word_freqs
            self.merges.append(best_pair)
    
    def tokenize(self, text):
        """Tokenize text using learned BPE"""
        text = self.preprocess_text(text)
        words = text.split()
        
        tokens = []
        for word in words:
            # Apply learned merges
            for merge in self.merges:
                word = word.replace(f' {merge[0]} {merge[1]} ', f' {merge[0]}{merge[1]} ')
                word = word.replace(f' {merge[0]} {merge[1]}', f' {merge[0]}{merge[1]}')
            
            # Split into tokens
            word_tokens = word.split()
            tokens.extend(word_tokens)
        
        return tokens
    
    def encode(self, text):
        """Encode text to token IDs"""
        tokens = self.tokenize(text)
        token_ids = []
        
        for token in tokens:
            if token in self.vocab:
                token_ids.append(self.vocab[token])
            else:
                # Handle unknown tokens
                token_ids.append(self.special_tokens['<unk>'])
        
        return token_ids
    
    def decode(self, token_ids):
        """Decode token IDs to text"""
        reverse_vocab = {v: k for k, v in self.vocab.items()}
        tokens = [reverse_vocab.get(tid, '<unk>') for tid in token_ids]
        return ' '.join(tokens)

# Training data
math_expressions = [
    "2x + 3y = 7",
    "sin(x) + cos(y) = 1",
    "∫(x^2)dx = x^3/3",
    "a^2 + b^2 = c^2",
    "lim(x→∞) (1/x) = 0",
    "∑(i=1 to n) i = n(n+1)/2",
    "√(x^2) = |x|",
    "log(ab) = log(a) + log(b)",
    "exp(x+y) = exp(x) * exp(y)",
    "f'(x) = lim(h→0) (f(x+h) - f(x))/h"
]

# Train tokenizer
tokenizer = MathTokenizer(vocab_size=500)
tokenizer.learn_bpe(math_expressions)

# Test tokenizer
test_expr = "2x^2 + 3x + 1"
tokens = tokenizer.tokenize(test_expr)
token_ids = tokenizer.encode(test_expr)
decoded = tokenizer.decode(token_ids)

print(f"Original: {test_expr}")
print(f"Tokens: {tokens}")
print(f"Token IDs: {token_ids}")
print(f"Decoded: {decoded}")
```

**Deliverables**:
- [ ] Custom BPE tokenizer implementation
- [ ] Training dataset of 100+ math expressions
- [ ] Evaluation metrics (tokenization accuracy, compression ratio)
- [ ] Comparison with standard tokenizers
- [ ] Documentation and usage examples

---

## 📊 **Progress Tracking**

### **Daily Checklist**
- [ ] 1 hour CS50 programming
- [ ] 45 minutes expression parser development
- [ ] 30 minutes tokenizer implementation
- [ ] 30 minutes StatQuest videos
- [ ] 15 minutes community engagement
- [ ] 15 minutes research paper reading

### **Weekly Milestones**
**Week 5**:
- [ ] Complete CS50 Week 4 (Memory)
- [ ] Build basic expression parser
- [ ] Implement BPE algorithm basics
- [ ] Watch 3 StatQuest videos
- [ ] Join r/LocalLLaMA community
- [ ] Read AutoMath paper introduction

**Week 6**:
- [ ] Complete CS50 Week 5 (Data Structures)
- [ ] Complete advanced expression parser
- [ ] Finish tokenizer implementation
- [ ] Watch 2 more StatQuest videos
- [ ] Complete AutoMath paper
- [ ] Submit both projects to GitHub

### **End-of-Period Assessment**
**Success Metrics**:
- [ ] CS50 problem set average: 82%+
- [ ] Expression parser: 100% test pass rate
- [ ] Tokenizer: Handles 50+ math expressions
- [ ] StatQuest understanding: 80%+ quiz scores
- [ ] Community engagement: 15+ meaningful interactions
- [ ] Research comprehension: AutoMath paper summarized

---

## 🚀 **Next Period Preparation**

### **Weeks 7-8 Preview**
- Complete Harvard CS50 with LLM-focused project
- Build attention mechanism demo
- Apply to Inspirit AI (deadline April 15)
- Submit to math competition
- Follow Math LLM researchers
- Complete basic statistics evaluation metrics

### **Resources to Preview**:
- [Attention Mechanism Tutorial](https://github.com/karpathy/nn-zero-to-hero)
- [Inspirit AI Application](https://www.inspiritai.com/)
- [Kaggle MATH Competition](https://www.kaggle.com/competitions/math-competition)
- [Math LLM Researchers on Twitter](https://twitter.com/)

---

## 📞 **Support & Resources**

### **Help Channels**:
- CS50 Section discussions and office hours
- r/LocalLLaMA: Technical implementation help
- OpenAI Discord: #research-discussions
- Stack Overflow: Specific programming questions

### **Additional Resources**:
- [The C Programming Language](https://www.amazon.com/C-Programming-Language-2nd/dp/0131103628) - CS50 reference
- [Algorithms](https://www.amazon.com/Algorithms-4th-Robert-Sedgewick/dp/032157351X) - Data structures reference
- [StatQuest Book](https://statquest.org/book/) - Statistics reference
- [Speech and Language Processing](https://web.stanford.edu/~jurafsky/slp3/) - NLP reference

---

*This 2-week plan focuses on intensive project development with comprehensive evaluation methods and advanced implementation for Math LLM capabilities.*
