# Weeks 7-8: Integration & Application - Quiz & Projects

## 📋 **Bi-Weekly Assessment Overview**
**Duration**: Weeks 7-8 (Integration & Application)
**Focus**: LLM fundamentals, attention mechanisms, neural networks
**Assessment Type**: Quiz + Practical Project

---

## 🧮 **Weeks 7-8 Quiz: LLM Fundamentals Assessment**

### **Section 1: Neural Network Foundations (40%)**

#### **Question 1: Neural Network Architecture**
**Problem**: Design a neural network for mathematical reasoning with:
1. Input layer for mathematical expressions
2. Hidden layers with appropriate activation functions
3. Output layer for mathematical results
4. Explain the choice of architecture and activation functions

**Scoring**: 10 points (2.5 points each component)

#### **Question 2: Backpropagation Mathematics**
**Problem**: Derive the backpropagation algorithm for:
1. Single hidden layer network
2. Multi-layer network with chain rule
3. Gradient computation for each layer
4. Weight update rules with learning rate

**Scoring**: 10 points (2.5 points each component)

#### **Question 3: Activation Functions**
**Problem**: Analyze and compare:
1. Sigmoid function and its derivatives
2. ReLU function and its variants
3. Tanh function properties
4. Suitability for mathematical reasoning tasks

**Scoring**: 10 points (2.5 points each function)

#### **Question 4: Loss Functions**
**Problem**: Design and analyze loss functions for:
1. Mathematical expression evaluation
2. Equation solving accuracy
3. Step-by-step reasoning quality
4. Multi-task learning scenarios

**Scoring**: 10 points (2.5 points each loss function)

### **Section 2: Attention Mechanisms (30%)**

#### **Question 5: Self-Attention Mathematics**
**Problem**: Derive the self-attention mechanism:
1. Query, Key, Value transformations
2. Attention weight computation
3. Context vector calculation
4. Computational complexity analysis

**Scoring**: 10 points (2.5 points each component)

#### **Question 6: Multi-Head Attention**
**Problem**: Explain multi-head attention:
1. Multiple attention heads design
2. Concatenation and linear transformation
3. Benefits over single-head attention
4. Implementation considerations

**Scoring**: 10 points (2.5 points each aspect)

#### **Question 7: Positional Encoding**
**Problem**: Design and analyze positional encoding:
1. Sinusoidal positional encoding
2. Learned positional embeddings
3. Comparison of approaches
4. Importance for mathematical sequences

**Scoring**: 10 points (2.5 points each approach)

### **Section 3: Mathematical Reasoning Applications (30%)**

#### **Question 8: Sequence-to-Sequence Models**
**Problem**: Design a seq2seq model for:
1. Mathematical problem solving
2. Step-by-step solution generation
3. Teacher forcing during training
4. Beam search for inference

**Scoring**: 10 points (2.5 points each component)

#### **Question 9: Transformer Architecture**
**Problem**: Analyze transformer components:
1. Encoder-decoder architecture
2. Feed-forward networks
3. Layer normalization and residual connections
4. Mathematical reasoning applications

**Scoring**: 10 points (2.5 points each component)

#### **Question 10: Training Strategies**
**Problem**: Design training strategies for:
1. Curriculum learning for mathematics
2. Data augmentation for math problems
3. Transfer learning from general text
4. Evaluation metrics for mathematical reasoning

**Scoring**: 10 points (2.5 points each strategy)

---

## 🚀 **Weeks 7-8 Project: Attention Mechanism Visualization**

### **Project Overview**
**Objective**: Implement and visualize transformer attention mechanisms for mathematical reasoning
**Duration**: 2 weeks
**Technologies**: PyTorch, NumPy, Matplotlib, Plotly, Streamlit

### **Project Components**

#### **Component 1: Scaled Dot-Product Attention (30%)**
**Requirements**:
- Implement scaled dot-product attention from scratch
- Support for batch processing
- Masking capabilities for variable lengths
- Gradient computation and backpropagation

**Deliverables**:
```python
import torch
import torch.nn as nn
import torch.nn.functional as F

class ScaledDotProductAttention(nn.Module):
    def __init__(self, d_model, dropout=0.1):
        super().__init__()
        self.d_model = d_model
        self.dropout = nn.Dropout(dropout)
        self.scale = 1.0 / (d_model ** 0.5)
    
    def forward(self, Q, K, V, mask=None):
        # Q, K, V: [batch_size, seq_len, d_model]
        # mask: [batch_size, seq_len, seq_len]
        
        # Compute attention scores
        scores = torch.matmul(Q, K.transpose(-2, -1)) * self.scale
        
        # Apply mask if provided
        if mask is not None:
            scores = scores.masked_fill(mask == 0, -1e9)
        
        # Compute attention weights
        attention_weights = F.softmax(scores, dim=-1)
        attention_weights = self.dropout(attention_weights)
        
        # Compute context vector
        context = torch.matmul(attention_weights, V)
        
        return context, attention_weights
```

**Success Criteria**:
- ✅ Correct attention computation
- ✅ Proper masking implementation
- ✅ Efficient batch processing
- ✅ Gradient flow verification

#### **Component 2: Multi-Head Attention (25%)**
**Requirements**:
- Implement multi-head attention mechanism
- Support for multiple attention heads
- Concatenation and output projection
- Position-wise feed-forward networks

**Deliverables**:
```python
class MultiHeadAttention(nn.Module):
    def __init__(self, d_model, num_heads, dropout=0.1):
        super().__init__()
        assert d_model % num_heads == 0
        
        self.d_model = d_model
        self.num_heads = num_heads
        self.d_k = d_model // num_heads
        
        self.W_q = nn.Linear(d_model, d_model)
        self.W_k = nn.Linear(d_model, d_model)
        self.W_v = nn.Linear(d_model, d_model)
        self.W_o = nn.Linear(d_model, d_model)
        
        self.attention = ScaledDotProductAttention(self.d_k, dropout)
        self.dropout = nn.Dropout(dropout)
    
    def forward(self, query, key, value, mask=None):
        batch_size = query.size(0)
        
        # Linear projections
        Q = self.W_q(query)  # [batch_size, seq_len, d_model]
        K = self.W_k(key)    # [batch_size, seq_len, d_model]
        V = self.W_v(value)  # [batch_size, seq_len, d_model]
        
        # Reshape for multi-head attention
        Q = Q.view(batch_size, -1, self.num_heads, self.d_k).transpose(1, 2)
        K = K.view(batch_size, -1, self.num_heads, self.d_k).transpose(1, 2)
        V = V.view(batch_size, -1, self.num_heads, self.d_k).transpose(1, 2)
        
        # Apply attention
        context, attention_weights = self.attention(Q, K, V, mask)
        
        # Concatenate heads
        context = context.transpose(1, 2).contiguous().view(
            batch_size, -1, self.d_model
        )
        
        # Output projection
        output = self.W_o(context)
        
        return output, attention_weights
```

**Success Criteria**:
- ✅ Correct multi-head implementation
- ✅ Proper head concatenation
- ✅ Efficient computation
- ✅ Gradient flow verification

#### **Component 3: Positional Encoding (20%)**
**Requirements**:
- Implement sinusoidal positional encoding
- Support for learned positional embeddings
- Visualization of positional patterns
- Analysis of encoding properties

**Deliverables**:
```python
import math

class PositionalEncoding(nn.Module):
    def __init__(self, d_model, max_len=5000):
        super().__init__()
        
        pe = torch.zeros(max_len, d_model)
        position = torch.arange(0, max_len, dtype=torch.float).unsqueeze(1)
        div_term = torch.exp(torch.arange(0, d_model, 2).float() * 
                           (-math.log(10000.0) / d_model))
        
        pe[:, 0::2] = torch.sin(position * div_term)
        pe[:, 1::2] = torch.cos(position * div_term)
        pe = pe.unsqueeze(0).transpose(0, 1)
        
        self.register_buffer('pe', pe)
    
    def forward(self, x):
        # x: [seq_len, batch_size, d_model]
        return x + self.pe[:x.size(0), :]

class LearnedPositionalEncoding(nn.Module):
    def __init__(self, d_model, max_len=5000):
        super().__init__()
        self.position_embeddings = nn.Embedding(max_len, d_model)
    
    def forward(self, x):
        # x: [batch_size, seq_len, d_model]
        positions = torch.arange(x.size(1), device=x.device)
        return x + self.position_embeddings(positions)
```

**Success Criteria**:
- ✅ Correct positional encoding
- ✅ Support for both approaches
- ✅ Visualization capabilities
- ✅ Analysis of encoding properties

#### **Component 4: Visualization Tools (25%)**
**Requirements**:
- Interactive attention weight visualization
- Multi-head attention comparison
- Sequence alignment visualization
- Real-time computation and updates

**Deliverables**:
```python
import plotly.graph_objects as go
import plotly.express as px
from plotly.subplots import make_subplots

class AttentionVisualizer:
    def __init__(self):
        self.figures = {}
    
    def attention_heatmap(self, attention_weights, tokens, layer_name=""):
        """Create interactive heatmap of attention weights"""
        fig = go.Figure(data=go.Heatmap(
            z=attention_weights.numpy(),
            x=tokens,
            y=tokens,
            colorscale='Blues',
            showscale=True,
            hoverongaps=False
        ))
        
        fig.update_layout(
            title=f'Attention Weights - {layer_name}',
            xaxis_title='Key Tokens',
            yaxis_title='Query Tokens',
            width=800,
            height=600
        )
        
        return fig
    
    def multi_head_comparison(self, attention_weights, tokens, num_heads):
        """Compare attention across multiple heads"""
        fig = make_subplots(
            rows=2, cols=num_heads//2,
            subplot_titles=[f'Head {i+1}' for i in range(num_heads)]
        )
        
        for i in range(num_heads):
            row = i // (num_heads // 2) + 1
            col = i % (num_heads // 2) + 1
            
            fig.add_trace(
                go.Heatmap(
                    z=attention_weights[i].numpy(),
                    x=tokens,
                    y=tokens,
                    colorscale='Blues',
                    showscale=False
                ),
                row=row, col=col
            )
        
        fig.update_layout(
            title='Multi-Head Attention Comparison',
            height=600
        )
        
        return fig
    
    def attention_flow(self, attention_weights, tokens):
        """Create attention flow visualization"""
        # Implementation for attention flow diagram
        pass
```

**Streamlit Interface**:
```python
import streamlit as st

def main():
    st.title("Attention Mechanism Visualizer")
    
    # Sidebar for configuration
    st.sidebar.header("Configuration")
    d_model = st.sidebar.slider("Model Dimension", 64, 512, 256)
    num_heads = st.sidebar.slider("Number of Heads", 2, 16, 8)
    seq_len = st.sidebar.slider("Sequence Length", 5, 50, 20)
    
    # Input text
    text_input = st.text_area("Enter Mathematical Expression", 
                             "2 + 3 * 4 - 5 / 2")
    
    if st.button("Analyze Attention"):
        # Tokenize and process
        tokens = text_input.split()
        
        # Create attention visualization
        visualizer = AttentionVisualizer()
        
        # Generate attention weights (mock for demo)
        attention_weights = torch.randn(num_heads, len(tokens), len(tokens))
        attention_weights = F.softmax(attention_weights, dim=-1)
        
        # Display visualizations
        fig = visualizer.multi_head_comparison(attention_weights, tokens, num_heads)
        st.plotly_chart(fig, use_container_width=True)

if __name__ == "__main__":
    main()
```

**Success Criteria**:
- ✅ Interactive visualizations
- ✅ Multi-head attention comparison
- ✅ Real-time computation
- ✅ User-friendly interface

---

## 📊 **Assessment Rubric**

### **Quiz Scoring (100 points total)**
- **Neural Network Foundations**: 40 points
- **Attention Mechanisms**: 30 points
- **Mathematical Reasoning Applications**: 30 points

**Grade Scale**:
- **A**: 90-100 points (Excellent)
- **B**: 80-89 points (Good)
- **C**: 70-79 points (Satisfactory)
- **D**: 60-69 points (Needs Improvement)
- **F**: <60 points (Unsatisfactory)

### **Project Scoring (100 points total)**
- **Scaled Dot-Product Attention**: 30 points
- **Multi-Head Attention**: 25 points
- **Positional Encoding**: 20 points
- **Visualization Tools**: 25 points

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
1. **Attention Mechanisms**: Implement and understand transformer attention
2. **Neural Networks**: Design and train neural architectures
3. **Mathematical Reasoning**: Apply neural networks to mathematical problems
4. **Visualization**: Create interactive visualizations of neural computations
5. **Deep Learning**: Understand modern transformer architectures

### **Prerequisites for Next Phase**
- ✅ Strong neural network understanding
- ✅ Experience with attention mechanisms
- ✅ Visualization skills for complex systems
- ✅ Preparation for advanced transformer implementation

---

## 📅 **Timeline & Milestones**

### **Week 7**
- **Day 43-44**: Complete quiz preparation and assessment
- **Day 45-47**: Implement scaled dot-product attention
- **Day 48-49**: Develop multi-head attention

### **Week 8**
- **Day 50-51**: Implement positional encoding
- **Day 52-54**: Create visualization tools
- **Day 55-56**: Testing, documentation, and final submission

---

## 🚀 **Submission Guidelines**

### **Quiz Submission**
- **Format**: Written responses (PDF) + code solutions (Python files)
- **Deadline**: End of Week 7
- **Submission**: Upload to learning management system

### **Project Submission**
- **Format**: GitHub repository with complete implementation
- **Contents**: Source code, documentation, visualizations, Streamlit app
- **Deadline**: End of Week 8
- **Submission**: Repository link + live demo URL + video (10 minutes)

---

**🎯 This bi-weekly assessment introduces transformer architectures and attention mechanisms, fundamental to modern mathematical reasoning systems.**
