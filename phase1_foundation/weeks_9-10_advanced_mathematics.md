# Weeks 9-10: Advanced Mathematics (June 1-14, 2026)

## 🎯 **Learning Objectives**
- Start MIT ML mathematical foundations course
- Begin Hugging Face Transformers course
- Review linear algebra for attention mechanisms
- Continue statistics learning with hypothesis testing
- Apply to Inspirit AI (deadline April 15 - follow-up)
- Research backup summer programs

---

## 📚 **Content & Resources**

### **MIT ML Mathematical Foundations**
**Resource**: [MIT 6.036 Introduction to Machine Learning](https://ocw.mit.edu/courses/6-036-introduction-to-machine-learning-fall-2020/)
- **Specific Lectures**:
  - [Lecture 1: Introduction](https://ocw.mit.edu/courses/6-036-introduction-to-machine-learning-fall-2020/resources/lecture-1-introduction-and-overview/)
  - [Lecture 2: Linear Classification](https://ocw.mit.edu/courses/6-036-introduction-to-machine-learning-fall-2020/resources/lecture-2-linear-classification/)
  - [Lecture 3: Perceptron Algorithm](https://ocw.mit.edu/courses/6-036-introduction-to-machine-learning-fall-2020/resources/lecture-3-perceptron/)

**Course Materials**:
- [Lecture Notes](https://ocw.mit.edu/courses/6-036-introduction-to-machine-learning-fall-2020/pages/lecture-notes/)
- [Problem Sets](https://ocw.mit.edu/courses/6-036-introduction-to-machine-learning-fall-2020/pages/assignments/)
- [Exams](https://ocw.mit.edu/courses/6-036-introduction-to-machine-learning-fall-2020/pages/exams/)

**Time Commitment**: 2 hours/day, 4 days/week
**Focus**: Mathematical foundations of machine learning, linear algebra applications

### **Hugging Face Transformers Course**
**Resource**: [Hugging Face NLP Course](https://huggingface.co/course/chapter1/1)
- **Specific Chapters**:
  - [Chapter 1: Transformer Models](https://huggingface.co/course/chapter1/1)
  - [Chapter 2: Using Transformers](https://huggingface.co/course/chapter2/1)
  - [Chapter 3: Fine-tuning Models](https://huggingface.co/course/chapter3/1)

**Course Components**:
- [Video lectures](https://www.youtube.com/playlist?list=PLqTA7I_2s9Z_5a9dXm0aFw9X8Q7z6Q7Z)
- [Interactive notebooks](https://github.com/huggingface/course)
- [Exercises and solutions](https://github.com/huggingface/course/tree/main/chapters)

**Time Commitment**: 1.5 hours/day, 3 days/week
**Focus**: Practical transformer implementation and fine-tuning

### **Linear Algebra for Attention Mechanisms**
**Resource**: [3Blue1Brown Linear Algebra](https://www.youtube.com/playlist?list=PLZHQObOWTQDPD3MizzM2xVFitgF8hE_ab)
- **Key Videos**:
  - [Chapter 6: Inverse matrices, column space and null space](https://www.youtube.com/watch?v=uQhTuRlWMxw)
  - [Chapter 7: Nonsquare matrices](https://www.youtube.com/watch?v=vXr2os-jQck)
  - [Chapter 8: Dot products and duality](https://www.youtube.com/watch?v=LyGKycYT2v0)

**Additional Resources**:
- [Linear Algebra for Deep Learning](https://pabloinsente.github.io/intro-linear-algebra/)
- [Mathematics for Machine Learning](https://mml-book.github.io/)

**Time Commitment**: 45 minutes/day, 3 days/week

### **Statistics: Hypothesis Testing**
**Resource**: [StatQuest Hypothesis Testing](https://www.youtube.com/watch?v=0oc49DyA3hU)
- **Key Videos**:
  - [Hypothesis Testing](https://www.youtube.com/watch?v=0oc49DyA3hU)
  - [p-values](https://www.youtube.com/watch?v=eyknGvncKLw)
  - [Type I and Type II Errors](https://www.youtube.com/watch?v=a1iDuQjzS8U)

**Additional Resources**:
- [Khan Academy Significance Tests](https://www.khanacademy.org/math/statistics-probability/significance-tests)
- [Statistical Significance in ML](https://arxiv.org/abs/1811.12808)

**Time Commitment**: 30 minutes/day, 3 days/week

### **Summer Program Applications**
**Primary**: [Inspirit AI](https://www.inspiritai.com/) - Follow-up on application
**Backup Options**:
- [Google CSSI](https://buildyourfuture.withgoogle.com/programs/computer-science-summer-institute/)
- [Microsoft Discovery Program](https://careers.microsoft.com/students/us/en/discoveryprogram)
- [AI4ALL](https://ai-4-all.org/programs/)
- [Summer Math Programs](https://www.ams.org/programs/math-camps)

---

## 🧪 **Evaluation & Assessment**

### **MIT ML Course Evaluation**
**Problem Sets**:
- [Problem Set 1: Introduction](https://ocw.mit.edu/courses/6-036-introduction-to-machine-learning-fall-2020/resources/mit6_036f20_ps1/)
- [Problem Set 2: Linear Classification](https://ocw.mit.edu/courses/6-036-introduction-to-machine-learning-fall-2020/resources/mit6_036f20_ps2/)

**Quiz Implementation**:
```python
# MIT ML Quiz Helper
import numpy as np
from sklearn.linear_model import Perceptron
from sklearn.datasets import make_classification

def test_perceptron():
    """Test perceptron algorithm understanding"""
    # Create sample data
    X, y = make_classification(n_samples=100, n_features=2, n_redundant=0, random_state=42)
    
    # Implement perceptron
    perceptron = Perceptron(random_state=42)
    perceptron.fit(X, y)
    
    # Test accuracy
    accuracy = perceptron.score(X, y)
    print(f"Perceptron accuracy: {accuracy:.2f}")
    
    return accuracy > 0.8

def test_linear_classification():
    """Test linear classification concepts"""
    # Create linearly separable data
    np.random.seed(42)
    class1 = np.random.randn(50, 2) + [2, 2]
    class2 = np.random.randn(50, 2) + [-2, -2]
    
    X = np.vstack([class1, class2])
    y = np.array([1]*50 + [0]*50)
    
    # Simple linear classifier
    perceptron = Perceptron()
    perceptron.fit(X, y)
    
    # Test on training data
    accuracy = perceptron.score(X, y)
    print(f"Linear classification accuracy: {accuracy:.2f}")
    
    return accuracy > 0.9

# Run tests
if __name__ == "__main__":
    print("Testing MIT ML concepts...")
    perceptron_test = test_perceptron()
    linear_test = test_linear_classification()
    
    print(f"Perceptron test passed: {perceptron_test}")
    print(f"Linear classification test passed: {linear_test}")
```

**Success Criteria**:
- [ ] Complete first 3 MIT lectures with notes
- [ ] Solve Problem Set 1 with 80%+ accuracy
- [ ] Implement perceptron algorithm from scratch
- [ ] Explain mathematical foundations of ML

### **Hugging Face Course Evaluation**
**Course Exercises**:
```python
# Hugging Face Course Exercises
from transformers import pipeline, AutoTokenizer, AutoModelForSequenceClassification
import torch

def test_transformer_pipeline():
    """Test basic transformer pipeline"""
    # Create sentiment analysis pipeline
    classifier = pipeline("sentiment-analysis")
    
    # Test with sample text
    result = classifier("I love learning about transformers!")
    print(f"Sentiment analysis result: {result}")
    
    return result[0]['label'] in ['POSITIVE', 'NEGATIVE']

def test_tokenizer_and_model():
    """Test tokenizer and model loading"""
    # Load tokenizer and model
    tokenizer = AutoTokenizer.from_pretrained("distilbert-base-uncased")
    model = AutoModelForSequenceClassification.from_pretrained("distilbert-base-uncased")
    
    # Test tokenization
    text = "Mathematical reasoning is important for AI"
    inputs = tokenizer(text, return_tensors="pt")
    
    # Test model inference
    outputs = model(**inputs)
    predictions = torch.nn.functional.softmax(outputs.logits, dim=-1)
    
    print(f"Input shape: {inputs['input_ids'].shape}")
    print(f"Output predictions: {predictions}")
    
    return inputs['input_ids'].shape[1] > 0

# Run tests
if __name__ == "__main__":
    print("Testing Hugging Face concepts...")
    pipeline_test = test_transformer_pipeline()
    tokenizer_test = test_tokenizer_and_model()
    
    print(f"Pipeline test passed: {pipeline_test}")
    print(f"Tokenizer test passed: {tokenizer_test}")
```

**Success Criteria**:
- [ ] Complete Chapters 1-3 of Hugging Face course
- [ ] Run all example notebooks successfully
- [ ] Fine-tune a simple model
- [ ] Explain transformer architecture components

### **Linear Algebra Evaluation**
**Attention Mechanism Quiz**:
```python
# Linear Algebra for Attention
import numpy as np
import matplotlib.pyplot as plt

def test_attention_linear_algebra():
    """Test linear algebra concepts for attention"""
    # Create Q, K, V matrices
    seq_len = 4
    d_k = 8
    
    Q = np.random.randn(seq_len, d_k)
    K = np.random.randn(seq_len, d_k)
    V = np.random.randn(seq_len, d_k)
    
    # Compute attention scores (Q @ K^T)
    scores = np.dot(Q, K.T)
    
    # Scale by sqrt(d_k)
    scaled_scores = scores / np.sqrt(d_k)
    
    # Apply softmax
    def softmax(x):
        exp_x = np.exp(x - np.max(x, axis=-1, keepdims=True))
        return exp_x / np.sum(exp_x, axis=-1, keepdims=True)
    
    attention_weights = softmax(scaled_scores)
    
    # Compute output
    output = np.dot(attention_weights, V)
    
    print(f"Q shape: {Q.shape}")
    print(f"K shape: {K.shape}")
    print(f"V shape: {V.shape}")
    print(f"Attention weights shape: {attention_weights.shape}")
    print(f"Output shape: {output.shape}")
    
    # Test properties
    assert attention_weights.shape == (seq_len, seq_len)
    assert output.shape == (seq_len, d_k)
    
    # Check if attention weights sum to 1
    row_sums = np.sum(attention_weights, axis=1)
    assert np.allclose(row_sums, 1.0)
    
    return True

# Run test
if __name__ == "__main__":
    print("Testing linear algebra for attention...")
    test_passed = test_attention_linear_algebra()
    print(f"Linear algebra test passed: {test_passed}")
```

**Success Criteria**:
- [ ] Complete 3Blue1Brown videos 6-8
- [ ] Implement attention mechanism from scratch
- [ ] Explain matrix operations in attention
- [ ] Visualize attention weights correctly

### **Statistics Evaluation**
**Hypothesis Testing Quiz**:
```python
# Hypothesis Testing Implementation
import numpy as np
from scipy import stats

def test_hypothesis_testing():
    """Test hypothesis understanding"""
    # Generate sample data
    np.random.seed(42)
    control_group = np.random.normal(100, 15, 100)
    treatment_group = np.random.normal(105, 15, 100)
    
    # Perform t-test
    t_stat, p_value = stats.ttest_ind(control_group, treatment_group)
    
    print(f"T-statistic: {t_stat:.3f}")
    print(f"P-value: {p_value:.3f}")
    
    # Test interpretation
    alpha = 0.05
    reject_null = p_value < alpha
    
    print(f"Reject null hypothesis: {reject_null}")
    print(f"Significant difference: {reject_null}")
    
    return p_value < 0.05

def test_p_value_calculation():
    """Test p-value understanding"""
    # Calculate p-value for observed statistic
    observed_stat = 2.5
    df = 98  # degrees of freedom
    
    # Two-tailed test
    p_value = 2 * (1 - stats.t.cdf(abs(observed_stat), df))
    
    print(f"Observed statistic: {observed_stat}")
    print(f"P-value: {p_value:.3f}")
    
    # Test significance
    is_significant = p_value < 0.05
    print(f"Statistically significant: {is_significant}")
    
    return p_value < 0.05

# Run tests
if __name__ == "__main__":
    print("Testing hypothesis testing concepts...")
    hypothesis_test = test_hypothesis_testing()
    pvalue_test = test_p_value_calculation()
    
    print(f"Hypothesis test passed: {hypothesis_test}")
    print(f"P-value test passed: {pvalue_test}")
```

**Success Criteria**:
- [ ] Complete StatQuest hypothesis testing videos
- [ ] Implement t-test from scratch
- [ ] Explain p-value interpretation
- [ ] Apply hypothesis testing to model evaluation

---

## 🛠️ **Projects & Applications**

### **Project 1: Mathematical Foundation for ML**
**Objective**: Implement core ML algorithms from mathematical principles

**Implementation**:
```python
# Mathematical ML Implementation
import numpy as np
from sklearn.datasets import make_classification
from sklearn.model_selection import train_test_split

class MathematicalML:
    def __init__(self):
        self.weights = None
        self.bias = None
    
    def perceptron_learning_rule(self, X, y, learning_rate=0.01, epochs=100):
        """Implement perceptron learning rule"""
        n_samples, n_features = X.shape
        
        # Initialize weights and bias
        self.weights = np.zeros(n_features)
        self.bias = 0
        
        # Training loop
        for epoch in range(epochs):
            for i in range(n_samples):
                # Forward pass
                linear_output = np.dot(X[i], self.weights) + self.bias
                prediction = 1 if linear_output > 0 else 0
                
                # Update weights if misclassified
                if prediction != y[i]:
                    update = learning_rate * (y[i] - prediction)
                    self.weights += update * X[i]
                    self.bias += update
        
        return self.weights, self.bias
    
    def predict(self, X):
        """Make predictions"""
        linear_output = np.dot(X, self.weights) + self.bias
        return np.where(linear_output > 0, 1, 0)
    
    def calculate_margin(self, X, y):
        """Calculate margin for linear classifier"""
        margins = []
        for i in range(len(X)):
            margin = y[i] * (np.dot(X[i], self.weights) + self.bias)
            margins.append(margin)
        return np.array(margins)

# Test implementation
def test_mathematical_ml():
    """Test mathematical ML implementation"""
    # Generate linearly separable data
    X, y = make_classification(n_samples=200, n_features=2, n_redundant=0, 
                              n_clusters_per_class=1, random_state=42)
    
    # Split data
    X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)
    
    # Train perceptron
    ml_model = MathematicalML()
    weights, bias = ml_model.perceptron_learning_rule(X_train, y_train)
    
    # Make predictions
    y_pred = ml_model.predict(X_test)
    
    # Calculate accuracy
    accuracy = np.mean(y_pred == y_test)
    
    print(f"Perceptron accuracy: {accuracy:.3f}")
    print(f"Weights: {weights}")
    print(f"Bias: {bias}")
    
    # Calculate margins
    margins = ml_model.calculate_margin(X_test, y_test)
    print(f"Average margin: {np.mean(margins):.3f}")
    
    return accuracy > 0.8

# Run test
if __name__ == "__main__":
    print("Testing mathematical ML implementation...")
    test_passed = test_mathematical_ml()
    print(f"Mathematical ML test passed: {test_passed}")
```

**Deliverables**:
- [ ] Working perceptron implementation
- [ ] Mathematical explanation of learning rule
- [ ] Performance comparison with sklearn
- [ ] Margin analysis and visualization

### **Project 2: Transformer Mathematics**
**Objective**: Implement transformer components with mathematical explanations

**Implementation**:
```python
# Transformer Mathematics Implementation
import numpy as np
import matplotlib.pyplot as plt

class TransformerMathematics:
    def __init__(self, d_model=512, n_heads=8):
        self.d_model = d_model
        self.n_heads = n_heads
        self.d_k = d_model // n_heads
    
    def scaled_dot_product_attention(self, Q, K, V, mask=None):
        """Implement scaled dot-product attention"""
        # Q, K, V shape: (batch_size, n_heads, seq_len, d_k)
        
        # Calculate attention scores
        scores = np.matmul(Q, K.transpose(-2, -1)) / np.sqrt(self.d_k)
        
        # Apply mask if provided
        if mask is not None:
            scores = np.where(mask == 0, -1e9, scores)
        
        # Apply softmax
        def softmax(x, axis=-1):
            exp_x = np.exp(x - np.max(x, axis=axis, keepdims=True))
            return exp_x / np.sum(exp_x, axis=axis, keepdims=True)
        
        attention_weights = softmax(scores)
        
        # Calculate output
        output = np.matmul(attention_weights, V)
        
        return output, attention_weights
    
    def multi_head_attention(self, Q, K, V):
        """Implement multi-head attention"""
        batch_size, seq_len, d_model = Q.shape
        
        # Linear projections (simplified)
        W_q = np.random.randn(d_model, d_model)
        W_k = np.random.randn(d_model, d_model)
        W_v = np.random.randn(d_model, d_model)
        
        # Project to Q, K, V
        Q_proj = np.dot(Q, W_q)
        K_proj = np.dot(K, W_k)
        V_proj = np.dot(V, W_v)
        
        # Reshape for multi-head
        Q_heads = Q_proj.reshape(batch_size, seq_len, self.n_heads, self.d_k).transpose(0, 2, 1, 3)
        K_heads = K_proj.reshape(batch_size, seq_len, self.n_heads, self.d_k).transpose(0, 2, 1, 3)
        V_heads = V_proj.reshape(batch_size, seq_len, self.n_heads, self.d_k).transpose(0, 2, 1, 3)
        
        # Apply attention to each head
        attention_outputs = []
        attention_weights_list = []
        
        for i in range(self.n_heads):
            attn_output, attn_weights = self.scaled_dot_product_attention(
                Q_heads[:, i], K_heads[:, i], V_heads[:, i]
            )
            attention_outputs.append(attn_output)
            attention_weights_list.append(attn_weights)
        
        # Concatenate heads
        concatenated = np.concatenate(attention_outputs, axis=-1)
        
        # Final linear projection
        W_o = np.random.randn(d_model, d_model)
        output = np.dot(concatenated, W_o)
        
        return output, attention_weights_list
    
    def positional_encoding(self, seq_len, d_model):
        """Generate positional encoding"""
        position = np.arange(seq_len)[:, np.newaxis]
        div_term = np.exp(np.arange(0, d_model, 2) * (-np.log(10000.0) / d_model))
        
        pos_encoding = np.zeros((seq_len, d_model))
        pos_encoding[:, 0::2] = np.sin(position * div_term)
        pos_encoding[:, 1::2] = np.cos(position * div_term)
        
        return pos_encoding

# Test implementation
def test_transformer_math():
    """Test transformer mathematics"""
    batch_size = 2
    seq_len = 10
    d_model = 512
    
    # Create sample inputs
    Q = np.random.randn(batch_size, seq_len, d_model)
    K = np.random.randn(batch_size, seq_len, d_model)
    V = np.random.randn(batch_size, seq_len, d_model)
    
    # Test transformer mathematics
    transformer = TransformerMathematics(d_model, n_heads=8)
    
    # Test multi-head attention
    output, attention_weights = transformer.multi_head_attention(Q, K, V)
    
    print(f"Input shape: {Q.shape}")
    print(f"Output shape: {output.shape}")
    print(f"Number of attention heads: {len(attention_weights)}")
    
    # Test positional encoding
    pos_encoding = transformer.positional_encoding(seq_len, d_model)
    print(f"Positional encoding shape: {pos_encoding.shape}")
    
    # Visualize attention weights for first head
    plt.figure(figsize=(10, 6))
    plt.imshow(attention_weights[0][0], cmap='Blues')
    plt.title('Attention Weights (Head 1)')
    plt.xlabel('Key Position')
    plt.ylabel('Query Position')
    plt.colorbar()
    plt.show()
    
    return output.shape == (batch_size, seq_len, d_model)

# Run test
if __name__ == "__main__":
    print("Testing transformer mathematics...")
    test_passed = test_transformer_math()
    print(f"Transformer math test passed: {test_passed}")
```

**Deliverables**:
- [ ] Complete transformer mathematics implementation
- [ ] Mathematical explanations for each component
- [ ] Attention weight visualizations
- [ ] Positional encoding analysis

---

## 📊 **Progress Tracking**

### **Daily Checklist**
- [ ] 2 hours MIT ML course
- [ ] 1.5 hours Hugging Face course
- [ ] 45 minutes linear algebra review
- [ ] 30 minutes statistics
- [ ] 15 minutes summer program research
- [ ] 15 minutes Inspirit AI follow-up

### **Weekly Milestones**
**Week 9**:
- [ ] Complete MIT Lectures 1-2
- [ ] Complete Hugging Face Chapters 1-2
- [ ] Finish 3Blue1Brown videos 6-7
- [ ] Complete StatQuest hypothesis testing
- [ ] Submit summer program applications
- [ ] Start mathematical ML project

**Week 10**:
- [ ] Complete MIT Lecture 3 + Problem Set 1
- [ ] Complete Hugging Face Chapter 3
- [ ] Finish 3Blue1Brown video 8
- [ ] Complete transformer math project
- [ ] Follow up on summer program applications
- [ ] Complete statistics evaluation

### **End-of-Period Assessment**
**Success Metrics**:
- [ ] MIT ML course: Lectures 1-3 completed, PS1 solved
- [ ] Hugging Face: Chapters 1-3 completed with exercises
- [ ] Linear Algebra: Attention mechanism implemented
- [ ] Statistics: Hypothesis testing mastered
- [ ] Projects: 2 completed with documentation
- [ ] Summer Programs: 3+ applications submitted

---

## 🚀 **Next Period Preparation**

### **Weeks 11-12 Preview**
- Continue MIT ML course (Problem Set 2)
- Build transformer components from scratch
- Master Hugging Face Transformers basics
- Start mathematical reasoning concept study
- Accept/reject summer program decisions

### **Resources to Preview**:
- [MIT Problem Set 2](https://ocw.mit.edu/courses/6-036-introduction-to-machine-learning-fall-2020/resources/mit6_036f20_ps2/)
- [PyTorch Transformer Tutorial](https://pytorch.org/tutorials/beginner/transformer_tutorial.html)
- [GSM8K Paper](https://arxiv.org/abs/2110.14168)
- [Summer Program Decision Matrix](https://docs.google.com/spreadsheets/d/1abc123)

---

## 📞 **Support & Resources**

### **Help Channels**:
- MIT OCW Discussion Forums
- Hugging Face Discord: #transformers and #course
- OpenAI Discord: #technical-discussions
- Summer Program Admissions Offices

### **Additional Resources**:
- [Mathematics for Machine Learning Book](https://mml-book.github.io/)
- [Transformer Blog Posts](https://blog.openai.com/transformers/)
- [Statistical Learning Theory](https://www.statlearning.com/)
- [Summer Program Reviews](https://www.reddit.com/r/ApplyingToCollege/)

---

*This 2-week plan provides comprehensive advanced mathematics foundation with MIT ML course, Hugging Face transformers, and statistical evaluation methods for Math LLM development.*
