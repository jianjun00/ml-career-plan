# Weeks 5-6: Project Development (March 29 - April 11, 2026)

## 🎯 **Learning Objectives**
- Master advanced programming concepts
- Build mathematical expression parser
- Implement tokenization for LLMs
- Advance statistics understanding
- Read AutoMath research paper

---

## 📚 **Core Resources**

### **Programming**
**Focus Areas**:
- Memory management and pointers
- Data structures (linked lists, trees, hash tables)
- Advanced algorithms

### **Mathematical Computing**
**Key Topics**:
- Expression parsing with SymPy
- Symbolic mathematics
- Statistical analysis

### **Research**
**Primary Paper**: AutoMath (automated mathematical reasoning)

---

## 🛠️ **Core Projects**

### **Project 1: Advanced Expression Parser**
**Objective**: Build parser for complex expressions

**Core Features**:
- Handle variables and functions
- Support symbolic evaluation
- Create simplification methods

### **Project 2: Tokenization Implementation**
**Objective**: Build subword tokenizer for LLMs

**Core Components**:
- Text preprocessing
- Vocabulary building
- Token encoding/decoding

        
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
