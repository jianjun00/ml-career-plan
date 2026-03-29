# Weeks 23-24: Transformer Implementation - Quiz & Projects

## 📋 **Bi-Weekly Assessment Overview**
**Duration**: Weeks 23-24 (Transformer Implementation)
**Focus**: Advanced transformer architecture, from-scratch implementation, mathematical reasoning applications
**Assessment Type**: Quiz + Practical Project

---

## 🧮 **Weeks 23-24 Quiz: Advanced Transformer Architecture Assessment**

### **Section 1: Transformer Architecture Deep Dive (40%)**

#### **Question 1: Advanced Attention Mechanisms**
**Problem**: Design and analyze:
1. Multi-head attention with mathematical analysis
2. Sparse attention mechanisms for efficiency
3. Linear attention approximations
4. Attention pattern analysis and interpretation

**Scoring**: 10 points (2.5 points each mechanism)

#### **Question 2: Transformer Architecture Variants**
**Problem**: Compare and implement:
1. Encoder-only vs encoder-decoder transformers
2. Hierarchical transformer architectures
3. Transformer variants for specific domains
4. Efficiency improvements and optimizations

**Scoring**: 10 points (2.5 points each variant)

#### **Question 3: Mathematical Transformer Specializations**
**Problem**: Design transformers for:
1. Mathematical sequence understanding
2. Symbolic computation integration
3. Multi-modal mathematical reasoning
4. Long-range dependency handling

**Scoring**: 10 points (2.5 points each specialization)

#### **Question 4: Training and Optimization**
**Problem**: Analyze and implement:
1. Advanced optimization techniques for transformers
2. Learning rate scheduling strategies
3. Regularization methods specific to transformers
4. Distributed training considerations

**Scoring**: 10 points (2.5 points each technique)

### **Section 2: Toolformer and External Integration (30%)**

#### **Question 5: Tool Learning Frameworks**
**Problem**: Design and implement:
1. Tool selection mechanisms for mathematical reasoning
2. Tool execution and result integration
3. Tool reliability and error handling
4. Multi-tool coordination strategies

**Scoring**: 10 points (2.5 points each framework)

#### **Question 6: Mathematical Tool Integration**
**Problem**: Implement integration with:
1. Symbolic mathematics libraries (SymPy, Mathematica)
2. Numerical computation tools (NumPy, SciPy)
3. Graph plotting and visualization tools
4. External APIs and databases

**Scoring**: 10 points (2.5 points each integration)

#### **Question 7: Advanced Tool-Augmented Reasoning**
**Problem**: Design systems for:
1. Dynamic tool discovery and learning
2. Tool composition and chaining
3. Verification and consistency checking
4. Uncertainty quantification in tool use

**Scoring**: 10 points (2.5 points each system)

### **Section 3: Performance Optimization (30%)**

#### **Question 8: Computational Efficiency**
**Problem**: Optimize transformer for:
1. Memory usage and computational complexity
2. Inference speed and latency optimization
3. Batch processing efficiency
4. Hardware-specific optimizations

**Scoring**: 10 points (2.5 points each optimization)

#### **Question 9: Scalability and Deployment**
**Problem**: Design systems for:
1. Model parallelism and distributed inference
2. Dynamic batching and serving optimization
3. Edge device deployment considerations
4. Monitoring and performance analysis

**Scoring**: 10 points (2.5 points each system)

#### **Question 10: Evaluation and Benchmarking**
**Problem**: Implement evaluation for:
1. Mathematical reasoning accuracy and correctness
2. Solution quality and interpretability
3. Efficiency and resource usage metrics
4. Comparative analysis with baseline methods

**Scoring**: 10 points (2.5 points each evaluation)

---

## 🚀 **Weeks 23-24 Project: Advanced Transformer Implementation

### **Project Overview**
**Objective**: Build advanced transformer from scratch with mathematical reasoning capabilities
**Duration**: 2 weeks
**Technologies**: PyTorch, CUDA, attention mechanisms, optimization techniques

### **Project Components**

#### **Component 1: Advanced Transformer Architecture (35%)**
**Requirements**:
- Complete transformer implementation from scratch
- Advanced attention mechanisms
- Mathematical reasoning specialization
- Performance optimization

**Deliverables**:
```python
# advanced_transformer.py
import torch
import torch.nn as nn
import torch.nn.functional as F
import math
from typing import Optional, Tuple, Dict
import numpy as np

class AdvancedTransformer(nn.Module):
    """Advanced transformer with mathematical reasoning capabilities"""
    
    def __init__(self, 
                 vocab_size: int,
                 d_model: int = 512,
                 n_heads: int = 8,
                 n_layers: int = 6,
                 d_ff: int = 2048,
                 max_seq_len: int = 1024,
                 dropout: float = 0.1,
                 use_sparse_attention: bool = True,
                 use_flash_attention: bool = True):
        super().__init__()
        
        self.d_model = d_model
        self.n_heads = n_heads
        self.n_layers = n_layers
        self.d_ff = d_ff
        self.max_seq_len = max_seq_len
        self.dropout = dropout
        
        # Embeddings and positional encoding
        self.token_embedding = nn.Embedding(vocab_size, d_model)
        self.pos_encoding = PositionalEncoding(d_model, max_seq_len)
        
        # Transformer layers
        self.layers = nn.ModuleList([
            TransformerBlock(d_model, n_heads, d_ff, dropout, 
                           use_sparse_attention, use_flash_attention)
            for _ in range(n_layers)
        ])
        
        # Mathematical reasoning heads
        self.math_reasoning_head = MathematicalReasoningHead(d_model)
        self.confidence_head = ConfidenceHead(d_model)
        
        # Output projection
        self.output_projection = nn.Linear(d_model, vocab_size)
        
        # Initialize weights
        self._init_weights()
    
    def _init_weights(self):
        """Initialize transformer weights"""
        for p in self.parameters():
            if p.dim() > 1:
                nn.init.xavier_uniform_(p)
            elif p.dim() == 1:
                nn.init.uniform_(p, -0.1, 0.1)
    
    def forward(self, 
                input_ids: torch.Tensor,
                attention_mask: Optional[torch.Tensor] = None,
                mathematical_context: Optional[Dict] = None) -> Dict[str, torch.Tensor]:
        """Forward pass with mathematical reasoning"""
        
        # Embeddings and positional encoding
        x = self.token_embedding(input_ids)
        x = x + self.pos_encoding(input_ids.size(1))
        
        # Apply attention mask if provided
        if attention_mask is not None:
            attention_mask = attention_mask.unsqueeze(1).unsqueeze(1)
            attention_mask = attention_mask.repeat(1, self.n_heads, 1, 1)
        else:
            attention_mask = None
        
        # Pass through transformer layers
        for layer in self.layers:
            x = layer(x, attention_mask)
        
        # Mathematical reasoning
        math_logits = self.math_reasoning_head(x, mathematical_context)
        confidence = self.confidence_head(x)
        
        # Output projection
        output_logits = self.output_projection(x)
        
        return {
            'output_logits': output_logits,
            'math_logits': math_logits,
            'confidence': confidence,
            'hidden_states': x
        }

class TransformerBlock(nn.Module):
    """Advanced transformer block with multiple attention types"""
    
    def __init__(self, 
                 d_model: int,
                 n_heads: int,
                 d_ff: int,
                 dropout: float = 0.1,
                 use_sparse_attention: bool = True,
                 use_flash_attention: bool = True):
        super().__init__()
        
        self.d_model = d_model
        self.n_heads = n_heads
        self.d_k = d_model // n_heads
        
        # Multi-head attention
        if use_flash_attention:
            self.attention = FlashMultiHeadAttention(d_model, n_heads)
        elif use_sparse_attention:
            self.attention = SparseMultiHeadAttention(d_model, n_heads)
        else:
            self.attention = StandardMultiHeadAttention(d_model, n_heads)
        
        # Feed-forward network
        self.ffn = PositionwiseFeedForward(d_model, d_ff, dropout)
        
        # Layer normalization and dropout
        self.norm1 = nn.LayerNorm(d_model)
        self.norm2 = nn.LayerNorm(d_model)
        self.dropout = nn.Dropout(dropout)
    
    def forward(self, 
                x: torch.Tensor,
                attention_mask: Optional[torch.Tensor] = None) -> torch.Tensor:
        """Forward pass through transformer block"""
        
        # Self-attention
        attn_output = self.attention(x, x, x, attention_mask)
        
        # Residual connection and layer norm
        x = self.norm1(x + self.dropout(attn_output))
        
        # Feed-forward network
        ffn_output = self.ffn(x)
        
        # Residual connection and layer norm
        x = self.norm2(x + self.dropout(ffn_output))
        
        return x

class StandardMultiHeadAttention(nn.Module):
    """Standard multi-head attention implementation"""
    
    def __init__(self, d_model: int, n_heads: int):
        super().__init__()
        self.d_model = d_model
        self.n_heads = n_heads
        self.d_k = d_model // n_heads
        
        self.w_q = nn.Linear(d_model, d_model)
        self.w_k = nn.Linear(d_model, d_model)
        self.w_v = nn.Linear(d_model, d_model)
        self.w_o = nn.Linear(d_model, d_model)
    
    def forward(self, 
                query: torch.Tensor,
                key: torch.Tensor,
                value: torch.Tensor,
                mask: Optional[torch.Tensor] = None) -> torch.Tensor:
        """Scaled dot-product attention"""
        
        batch_size, seq_len, d_model = query.size()
        
        # Linear projections
        Q = self.w_q(query).view(batch_size, seq_len, self.n_heads, self.d_k)
        K = self.w_k(key).view(batch_size, seq_len, self.n_heads, self.d_k)
        V = self.w_v(value).view(batch_size, seq_len, self.n_heads, self.d_k)
        
        # Transpose for attention computation
        Q = Q.transpose(1, 2)  # (batch_size, n_heads, seq_len, d_k)
        K = K.transpose(1, 2)  # (batch_size, n_heads, seq_len, d_k)
        V = V.transpose(1, 2)  # (batch_size, n_heads, seq_len, d_k)
        
        # Scaled dot-product attention
        scores = torch.matmul(Q, K.transpose(-2, -1)) / math.sqrt(self.d_k)
        
        # Apply mask if provided
        if mask is not None:
            scores = scores.masked_fill(mask == 0, -1e9)
        
        attention_weights = F.softmax(scores, dim=-1)
        
        # Attention-weighted values
        context = torch.matmul(attention_weights, V)
        
        # Concatenate heads and project
        context = context.transpose(1, 2).contiguous().view(
            batch_size, seq_len, self.d_model
        )
        output = self.w_o(context)
        
        return output

class SparseMultiHeadAttention(nn.Module):
    """Sparse multi-head attention for efficiency"""
    
    def __init__(self, d_model: int, n_heads: int):
        super().__init__()
        self.d_model = d_model
        self.n_heads = n_heads
        self.d_k = d_model // n_heads
        
        self.w_q = nn.Linear(d_model, d_model)
        self.w_k = nn.Linear(d_model, d_model)
        self.w_v = nn.Linear(d_model, d_model)
        self.w_o = nn.Linear(d_model, d_model)
        
        # Learnable sparsity parameters
        self.top_k = min(64, d_model // 4)
        self.sparse_threshold = nn.Parameter(torch.tensor(0.1))
    
    def forward(self, 
                query: torch.Tensor,
                key: torch.Tensor,
                value: torch.Tensor,
                mask: Optional[torch.Tensor] = None) -> torch.Tensor:
        """Sparse attention computation"""
        
        batch_size, seq_len, d_model = query.size()
        
        # Linear projections
        Q = self.w_q(query).view(batch_size, seq_len, self.n_heads, self.d_k)
        K = self.w_k(key).view(batch_size, seq_len, self.n_heads, self.d_k)
        V = self.w_v(value).view(batch_size, seq_len, self.n_heads, self.d_k)
        
        # Transpose for attention computation
        Q = Q.transpose(1, 2)
        K = K.transpose(-2, -1)
        V = V.transpose(1, 2)
        
        # Compute attention scores
        scores = torch.matmul(Q, K) / math.sqrt(self.d_k)
        
        # Apply mask if provided
        if mask is not None:
            scores = scores.masked_fill(mask == 0, -1e9)
        
        # Sparse attention: select top-k tokens
        top_k_scores, top_k_indices = torch.topk(scores, self.top_k, dim=-1)
        
        # Create sparse attention weights
        sparse_weights = torch.zeros_like(scores)
        sparse_weights.scatter_(-1, top_k_indices, 
                               torch.ones_like(top_k_scores))
        
        # Apply threshold
        sparse_weights = sparse_weights * (sparse_weights > self.sparse_threshold)
        
        # Normalize sparse weights
        sparse_weights = F.softmax(sparse_weights, dim=-1)
        
        # Attention-weighted values
        context = torch.matmul(sparse_weights, V)
        
        # Concatenate heads and project
        context = context.transpose(1, 2).contiguous().view(
            batch_size, seq_len, self.d_model
        )
        output = self.w_o(context)
        
        return output

class FlashMultiHeadAttention(nn.Module):
    """Flash attention for memory efficiency"""
    
    def __init__(self, d_model: int, n_heads: int):
        super().__init__()
        self.d_model = d_model
        self.n_heads = n_heads
        self.d_k = d_model // n_heads
        
        self.w_q = nn.Linear(d_model, d_model)
        self.w_k = nn.Linear(d_model, d_model)
        self.w_v = nn.Linear(d_model, d_model)
        self.w_o = nn.Linear(d_model, d_model)
        
        # Flash attention specific parameters
        self.block_size = 64
        self.window_size = 512
    
    def forward(self, 
                query: torch.Tensor,
                key: torch.Tensor,
                value: torch.Tensor,
                mask: Optional[torch.Tensor] = None) -> torch.Tensor:
        """Flash attention implementation"""
        
        # This would implement the actual flash attention algorithm
        # For now, provide a simplified version
        
        batch_size, seq_len, d_model = query.size()
        
        # Linear projections
        Q = self.w_q(query).view(batch_size, seq_len, self.n_heads, self.d_k)
        K = self.w_k(key).view(batch_size, seq_len, self.n_heads, self.d_k)
        V = self.w_v(value).view(batch_size, seq_len, self.n_heads, self.d_k)
        
        # Simplified flash attention (would need CUDA implementation)
        # For demonstration, use standard attention with memory optimization
        scores = torch.matmul(Q, K.transpose(-2, -1)) / math.sqrt(self.d_k)
        
        if mask is not None:
            scores = scores.masked_fill(mask == 0, -1e9)
        
        attention_weights = F.softmax(scores, dim=-1)
        context = torch.matmul(attention_weights, V)
        
        context = context.transpose(1, 2).contiguous().view(
            batch_size, seq_len, self.d_model
        )
        output = self.w_o(context)
        
        return output

class PositionalEncoding(nn.Module):
    """Positional encoding for transformer"""
    
    def __init__(self, d_model: int, max_len: int = 5000):
        super().__init__()
        self.d_model = d_model
        self.max_len = max_len
        
        pe = torch.zeros(max_len, d_model)
        position = torch.arange(0, max_len, dtype=torch.float).unsqueeze(1)
        
        div_term = torch.exp(torch.arange(0, d_model, 2, dtype=torch.float) *
                           -(math.log(10000.0) / d_model))
        
        pe[:, 0::2] = torch.sin(position * div_term)
        pe[:, 1::2] = torch.cos(position * div_term)
        
        self.register_buffer('pe', pe.unsqueeze(0))
    
    def forward(self, x: torch.Tensor) -> torch.Tensor:
        seq_len = x.size(1)
        return self.pe[:, :seq_len]

class PositionwiseFeedForward(nn.Module):
    """Position-wise feed-forward network"""
    
    def __init__(self, d_model: int, d_ff: int, dropout: float = 0.1):
        super().__init__()
        self.w_1 = nn.Linear(d_model, d_ff)
        self.w_2 = nn.Linear(d_ff, d_model)
        self.dropout = nn.Dropout(dropout)
        self.activation = nn.GELU()
    
    def forward(self, x: torch.Tensor) -> torch.Tensor:
        return self.w_2(self.dropout(self.activation(self.w_1(x))))

class MathematicalReasoningHead(nn.Module):
    """Mathematical reasoning head for transformer"""
    
    def __init__(self, d_model: int):
        super().__init__()
        self.d_model = d_model
        
        # Mathematical operation classifiers
        self.operation_classifier = nn.Linear(d_model, 10)  # 10 math operations
        self.number_classifier = nn.Linear(d_model, 1)  # numerical answer
        self.proof_generator = nn.Linear(d_model, d_model)  # proof steps
        self.confidence_estimator = nn.Linear(d_model, 1)
    
    def forward(self, x: torch.Tensor, context: Optional[Dict] = None) -> torch.Tensor:
        """Generate mathematical reasoning outputs"""
        
        # Global average pooling
        pooled = F.adaptive_avg_pool2d(x, (1, 1))
        pooled = pooled.view(pooled.size(0), -1)
        
        # Mathematical reasoning outputs
        operation_logits = self.operation_classifier(pooled)
        number_logits = self.number_classifier(pooled)
        proof_steps = self.proof_generator(pooled)
        confidence = torch.sigmoid(self.confidence_estimator(pooled))
        
        return torch.cat([operation_logits, number_logits, confidence], dim=-1)

class ConfidenceHead(nn.Module):
    """Confidence estimation head"""
    
    def __init__(self, d_model: int):
        super().__init__()
        self.d_model = d_model
        self.confidence_layers = nn.Sequential(
            nn.Linear(d_model, d_model // 2),
            nn.ReLU(),
            nn.Dropout(0.1),
            nn.Linear(d_model // 2, d_model // 4),
            nn.ReLU(),
            nn.Dropout(0.1),
            nn.Linear(d_model // 4, 1),
            nn.Sigmoid()
        )
    
    def forward(self, x: torch.Tensor) -> torch.Tensor:
        """Estimate confidence in solution"""
        # Global average pooling
        pooled = F.adaptive_avg_pool2d(x, (1, 1))
        pooled = pooled.view(pooled.size(0), -1)
        
        return self.confidence_layers(pooled)
```

**Success Criteria**:
- ✅ Complete transformer implementation
- ✅ Advanced attention mechanisms
- ✅ Mathematical reasoning capabilities
- ✅ Performance optimizations

#### **Component 2: Training and Optimization (30%)**
**Requirements**:
- Advanced training pipeline
- Mathematical reasoning loss functions
- Optimizers and learning rate scheduling
- Performance monitoring and analysis

**Deliverables**:
```python
# transformer_training.py
import torch
import torch.optim as optim
import torch.nn as nn
from torch.utils.data import DataLoader
import numpy as np
from typing import Dict, List, Optional
import wandb

class MathematicalReasoningLoss(nn.Module):
    """Custom loss function for mathematical reasoning"""
    
    def __init__(self, alpha: float = 0.7, beta: float = 0.3):
        super().__init__()
        self.alpha = alpha  # Weight for accuracy
        self.beta = beta    # Weight for reasoning quality
        self.mse_loss = nn.MSELoss()
        self.ce_loss = nn.CrossEntropyLoss()
    
    def forward(self, 
                predictions: Dict[str, torch.Tensor],
                targets: Dict[str, torch.Tensor]) -> Dict[str, torch.Tensor]:
        """Calculate combined loss for mathematical reasoning"""
        
        losses = {}
        
        # Accuracy loss for numerical answers
        if 'number_logits' in predictions and 'number_target' in targets:
            accuracy_loss = self.mse_loss(
                predictions['number_logits'], targets['number_target']
            )
            losses['accuracy'] = accuracy_loss * self.alpha
        
        # Cross-entropy loss for operations
        if 'operation_logits' in predictions and 'operation_target' in targets:
            operation_loss = self.ce_loss(
                predictions['operation_logits'], targets['operation_target']
            )
            losses['operation'] = operation_loss * self.beta
        
        # Confidence regularization
        if 'confidence' in predictions:
            confidence_penalty = torch.mean((predictions['confidence'] - 0.8) ** 2)
            losses['confidence_penalty'] = confidence_penalty * 0.1
        
        # Total loss
        total_loss = sum(losses.values())
        losses['total'] = total_loss
        
        return losses

class AdvancedTrainer:
    """Advanced trainer for mathematical reasoning transformer"""
    
    def __init__(self, 
                 model: nn.Module,
                 train_loader: DataLoader,
                 val_loader: DataLoader,
                 config: Dict):
        self.model = model
        self.train_loader = train_loader
        self.val_loader = val_loader
        self.config = config
        
        # Optimizer with different learning rates
        self.optimizer = optim.AdamW([
            {'params': model.token_embedding.parameters(), 'lr': config['embedding_lr']},
            {'params': model.pos_encoding.parameters(), 'lr': config['positional_lr']},
            {'params': model.layers.parameters(), 'lr': config['transformer_lr']},
            {'params': model.math_reasoning_head.parameters(), 'lr': config['reasoning_lr']},
            {'params': model.output_projection.parameters(), 'lr': config['output_lr']}
        ])
        
        # Learning rate scheduler
        self.scheduler = optim.lr_scheduler.ReduceLROnPlateau(
            self.optimizer, mode='min', factor=0.5, patience=5
        )
        
        # Loss function
        self.criterion = MathematicalReasoningLoss()
        
        # Training state
        self.current_epoch = 0
        self.best_val_loss = float('inf')
        self.training_history = []
        
        # Initialize wandb
        wandb.init(project="mathematical-reasoning-transformer", config=config)
    
    def train_epoch(self) -> Dict[str, float]:
        """Train for one epoch"""
        self.model.train()
        total_loss = 0
        num_batches = 0
        
        for batch in self.train_loader:
            self.optimizer.zero_grad()
            
            # Forward pass
            outputs = self.model(
                input_ids=batch['input_ids'],
                attention_mask=batch['attention_mask'],
                mathematical_context=batch.get('math_context')
            )
            
            # Calculate loss
            targets = {
                'number_target': batch['number_target'],
                'operation_target': batch['operation_target']
            }
            
            losses = self.criterion(outputs, targets)
            total_loss += losses['total']
            
            # Backward pass
            total_loss.backward()
            
            # Gradient clipping
            torch.nn.utils.clip_grad_norm_(self.model.parameters(), 1.0)
            
            self.optimizer.step()
            num_batches += 1
            
            # Log to wandb
            wandb.log({
                'batch_loss': losses['total'],
                'accuracy_loss': losses['accuracy'],
                'operation_loss': losses['operation'],
                'confidence_penalty': losses['confidence_penalty']
            })
        
        avg_loss = total_loss / num_batches
        return {'train_loss': avg_loss, 'num_batches': num_batches}
    
    def validate(self) -> Dict[str, float]:
        """Validate model"""
        self.model.eval()
        total_loss = 0
        correct_predictions = 0
        total_predictions = 0
        
        with torch.no_grad():
            for batch in self.val_loader:
                outputs = self.model(
                    input_ids=batch['input_ids'],
                    attention_mask=batch['attention_mask'],
                    mathematical_context=batch.get('math_context')
                )
                
                # Calculate predictions
                number_pred = torch.argmax(outputs['number_logits'], dim=-1)
                operation_pred = torch.argmax(outputs['operation_logits'], dim=-1)
                
                # Calculate accuracy
                number_correct = (number_pred == batch['number_target']).float().mean()
                operation_correct = (operation_pred == batch['operation_target']).float().mean()
                
                total_correct = number_correct + operation_correct
                total_predictions += 2
                
                # Calculate loss
                targets = {
                    'number_target': batch['number_target'],
                    'operation_target': batch['operation_target']
                }
                
                losses = self.criterion(outputs, targets)
                total_loss += losses['total']
        
        avg_loss = total_loss / len(self.val_loader)
        accuracy = total_correct / total_predictions
        
        return {
            'val_loss': avg_loss,
            'val_accuracy': accuracy
        }
    
    def train(self, num_epochs: int):
        """Complete training loop"""
        for epoch in range(num_epochs):
            self.current_epoch = epoch
            
            # Training
            train_metrics = self.train_epoch()
            
            # Validation
            val_metrics = self.validate()
            
            # Learning rate scheduling
            self.scheduler.step(val_metrics['val_loss'])
            
            # Save best model
            if val_metrics['val_loss'] < self.best_val_loss:
                self.best_val_loss = val_metrics['val_loss']
                torch.save({
                    'epoch': epoch,
                    'model_state_dict': self.model.state_dict(),
                    'optimizer_state_dict': self.optimizer.state_dict(),
                    'val_loss': val_metrics['val_loss'],
                    'val_accuracy': val_metrics['val_accuracy']
                }, 'best_model.pth')
            
            # Log epoch metrics
            wandb.log({
                'epoch': epoch,
                'train_loss': train_metrics['train_loss'],
                'val_loss': val_metrics['val_loss'],
                'val_accuracy': val_metrics['val_accuracy'],
                'learning_rate': self.optimizer.param_groups[0]['lr']
            })
            
            print(f"Epoch {epoch}: Train Loss = {train_metrics['train_loss']:.4f}, "
                  f"Val Loss = {val_metrics['val_loss']:.4f}, "
                  f"Val Accuracy = {val_metrics['val_accuracy']:.4f}")
```

**Success Criteria**:
- ✅ Advanced training pipeline
- ✅ Mathematical reasoning loss functions
- ✅ Performance monitoring
- ✅ Model optimization

#### **Component 3: Performance Analysis (20%)**
**Requirements**:
- Comprehensive performance evaluation
- Mathematical reasoning quality assessment
- Efficiency analysis and optimization
- Comparative benchmarking

**Deliverables**:
```python
# performance_analysis.py
import torch
import numpy as np
from typing import Dict, List, Tuple
import matplotlib.pyplot as plt
import seaborn as sns
from sklearn.metrics import classification_report, confusion_matrix
import json

class PerformanceAnalyzer:
    """Comprehensive performance analysis for mathematical reasoning transformer"""
    
    def __init__(self):
        self.metrics_history = []
        self.benchmark_results = {}
    
    def evaluate_model(self, 
                     model: nn.Module,
                     test_loader: torch.utils.data.DataLoader,
                     device: torch.device) -> Dict:
        """Comprehensive model evaluation"""
        
        model.eval()
        all_predictions = []
        all_targets = []
        all_confidences = []
        
        with torch.no_grad():
            for batch in test_loader:
                # Move batch to device
                input_ids = batch['input_ids'].to(device)
                attention_mask = batch['attention_mask'].to(device)
                
                # Forward pass
                outputs = model(input_ids, attention_mask)
                
                # Extract predictions
                number_pred = torch.argmax(outputs['number_logits'], dim=-1)
                operation_pred = torch.argmax(outputs['operation_logits'], dim=-1)
                confidence = outputs['confidence']
                
                # Collect results
                batch_size = input_ids.size(0)
                for i in range(batch_size):
                    all_predictions.append({
                        'number_pred': number_pred[i].item(),
                        'operation_pred': operation_pred[i].item(),
                        'confidence': confidence[i].item()
                    })
                    all_targets.append({
                        'number_target': batch['number_target'][i].item(),
                        'operation_target': batch['operation_target'][i].item()
                    })
                    all_confidences.append(confidence[i].item())
        
        # Calculate comprehensive metrics
        metrics = self._calculate_comprehensive_metrics(
            all_predictions, all_targets, all_confidences
        )
        
        return metrics
    
    def _calculate_comprehensive_metrics(self, 
                                    predictions: List[Dict],
                                    targets: List[Dict],
                                    confidences: List[float]) -> Dict:
        """Calculate comprehensive performance metrics"""
        
        # Extract predictions and targets
        number_preds = [p['number_pred'] for p in predictions]
        number_targets = [t['number_target'] for t in targets]
        operation_preds = [p['operation_pred'] for p in predictions]
        operation_targets = [t['operation_target'] for t in targets]
        
        # Accuracy metrics
        number_accuracy = sum(1 for p, t in zip(number_preds, number_targets) 
                           if p == t) / len(number_preds)
        operation_accuracy = sum(1 for p, t in zip(operation_preds, operation_targets) 
                             if p == t) / len(operation_preds)
        
        # Confidence analysis
        avg_confidence = np.mean(confidences)
        confidence_calibration = self._calculate_confidence_calibration(
            confidences, number_accuracy
        )
        
        # Error analysis
        number_errors = self._analyze_errors(number_preds, number_targets, 'number')
        operation_errors = self._analyze_errors(operation_preds, operation_targets, 'operation')
        
        # Mathematical reasoning quality
        reasoning_quality = self._assess_reasoning_quality(predictions, targets)
        
        return {
            'number_accuracy': number_accuracy,
            'operation_accuracy': operation_accuracy,
            'avg_confidence': avg_confidence,
            'confidence_calibration': confidence_calibration,
            'number_errors': number_errors,
            'operation_errors': operation_errors,
            'reasoning_quality': reasoning_quality,
            'total_samples': len(predictions)
        }
    
    def _calculate_confidence_calibration(self, 
                                   confidences: List[float],
                                   accuracy: float) -> float:
        """Calculate confidence calibration metrics"""
        # Expected Calibration Error (ECE)
        n_bins = 10
        bin_boundaries = np.linspace(0, 1, n_bins + 1)
        bin_lowers = bin_boundaries[:-1]
        bin_uppers = bin_boundaries[1:]
        
        ece = 0.0
        for i in range(n_bins):
            # Find samples in this bin
            bin_mask = (np.array(confidences) > bin_lowers[i]) & 
                       (np.array(confidences) <= bin_uppers[i]))
            
            if np.sum(bin_mask) > 0:
                accuracy_in_bin = np.mean(np.array(accuracy)[bin_mask])
                avg_confidence_in_bin = np.mean(np.array(confidences)[bin_mask])
                
                ece += np.abs(avg_confidence_in_bin - accuracy_in_bin) * np.sum(bin_mask)
        
        ece /= len(confidences)
        
        return ece
    
    def _analyze_errors(self, predictions: List[int], targets: List[int], error_type: str) -> Dict:
        """Analyze prediction errors"""
        errors = []
        error_types = {}
        
        for pred, target in zip(predictions, targets):
            if pred != target:
                error_type = self._classify_error(pred, target, error_type)
                errors.append(error_type)
                error_types[error_type] = error_types.get(error_type, 0) + 1
        
        return {
            'error_list': errors,
            'error_types': error_types,
            'error_rate': len(errors) / len(predictions)
        }
    
    def _classify_error(self, pred: int, target: int, error_type: str) -> str:
        """Classify type of error"""
        if error_type == 'number':
            if abs(pred - target) == 1:
                return 'off_by_one'
            elif abs(pred - target) <= 5:
                return 'small_error'
            elif abs(pred - target) <= 10:
                return 'medium_error'
            else:
                return 'large_error'
        return 'unknown'
    
    def _assess_reasoning_quality(self, predictions: List[Dict], targets: List[Dict]) -> Dict:
        """Assess quality of mathematical reasoning"""
        quality_scores = {
            'consistency': 0.0,
            'completeness': 0.0,
            'correctness': 0.0
        }
        
        for i, (pred, target) in enumerate(zip(predictions, targets)):
            # Consistency: does prediction follow from previous steps?
            # Completeness: are all necessary steps included?
            # Correctness: is final answer correct?
            
            # Simplified assessment
            if pred['number_pred'] == target['number_target']:
                quality_scores['correctness'] += 1
            
            # Would need actual reasoning steps to assess consistency and completeness
        
        total_predictions = len(predictions)
        
        return {
            'avg_consistency': quality_scores['consistency'] / total_predictions,
            'avg_completeness': quality_scores['completeness'] / total_predictions,
            'avg_correctness': quality_scores['correctness'] / total_predictions
        }
    
    def generate_performance_report(self, metrics: Dict) -> str:
        """Generate comprehensive performance report"""
        report = f"""
# Mathematical Reasoning Transformer Performance Report

## Overall Metrics
- **Number Accuracy**: {metrics['number_accuracy']:.2%}
- **Operation Accuracy**: {metrics['operation_accuracy']:.2%}
- **Average Confidence**: {metrics['avg_confidence']:.3f}
- **Confidence Calibration (ECE)**: {metrics['confidence_calibration']:.3f}

## Error Analysis
### Number Errors
{self._format_error_analysis(metrics['number_errors'])}

### Operation Errors
{self._format_error_analysis(metrics['operation_errors'])}

## Reasoning Quality
- **Consistency**: {metrics['reasoning_quality']['avg_consistency']:.2f}
- **Completeness**: {metrics['reasoning_quality']['avg_completeness']:.2f}
- **Correctness**: {metrics['reasoning_quality']['avg_correctness']:.2f}

## Recommendations
1. Improve confidence calibration through temperature scaling
2. Enhance error detection and correction mechanisms
3. Implement better step-by-step reasoning validation
4. Add more diverse training examples for edge cases
        """
        
        return report
    
    def _format_error_analysis(self, errors: List[str]) -> str:
        """Format error analysis for report"""
        if not errors:
            return "No errors detected"
        
        error_counts = {}
        for error in errors:
            error_counts[error] = error_counts.get(error, 0) + 1
        
        analysis = []
        for error_type, count in sorted(error_counts.items(), key=lambda x: x[1], reverse=True):
            analysis.append(f"- {error_type}: {count} occurrences")
        
        return "\n".join(analysis)
    
    def visualize_performance(self, metrics: Dict, save_path: str = 'performance_analysis.png'):
        """Create performance visualizations"""
        
        fig, axes = plt.subplots(2, 2, figsize=(15, 10))
        
        # Accuracy comparison
        categories = ['Number Accuracy', 'Operation Accuracy']
        values = [metrics['number_accuracy'], metrics['operation_accuracy']]
        
        bars = axes[0, 0].bar(categories, values)
        axes[0, 0].set_title('Accuracy Comparison')
        axes[0, 0].set_ylabel('Accuracy')
        axes[0, 0].set_ylim(0, 1)
        
        # Confidence distribution
        axes[0, 1].hist([p['confidence'] for p in metrics.get('confidence_history', [])], 
                           bins=20, alpha=0.7)
        axes[0, 1].set_title('Confidence Distribution')
        axes[0, 1].set_xlabel('Confidence')
        axes[0, 1].set_ylabel('Frequency')
        
        # Error analysis
        error_types = list(metrics['number_errors']['error_types'].keys())
        error_counts = list(metrics['number_errors']['error_types'].values())
        
        axes[1, 0].pie(error_counts, labels=error_types, autopct='%1.1f%%')
        axes[1, 0].set_title('Error Type Distribution')
        
        plt.tight_layout()
        plt.savefig(save_path, dpi=300, bbox_inches='tight')
        plt.close()
```

**Success Criteria**:
- ✅ Comprehensive performance evaluation
- ✅ Mathematical reasoning quality assessment
- ✅ Error analysis and visualization
- ✅ Performance optimization recommendations

#### **Component 4: Mathematical Reasoning Applications (15%)**
**Requirements**:
- Real-world mathematical problems
- Advanced reasoning chain generation
- Solution verification and validation
- Performance optimization

**Deliverables**:
```python
# math_reasoning_applications.py
import torch
import numpy as np
from typing import List, Dict, Optional
import json
import re

class MathematicalReasoningApplications:
    """Applications of advanced transformer to real-world problems"""
    
    def __init__(self, model, tokenizer):
        self.model = model
        self.tokenizer = tokenizer
        
        # Application domains
        self.domains = {
            'physics': self._physics_problems,
            'engineering': self._engineering_problems,
            'finance': self._finance_problems,
            'statistics': self._statistics_problems
        }
    
    def solve_domain_problems(self, domain: str, num_problems: int = 10) -> List[Dict]:
        """Solve problems from specific domain"""
        problems = self.domains.get(domain, [])
        
        results = []
        for problem in problems[:num_problems]:
            result = self._solve_single_problem(problem)
            results.append(result)
        
        return results
    
    def _solve_single_problem(self, problem: Dict) -> Dict:
        """Solve individual mathematical problem"""
        
        # Prepare input
        problem_text = problem['problem']
        inputs = self.tokenizer(
            problem_text, 
            return_tensors='pt', 
            padding=True, 
            truncation=True, 
            max_length=512
        )
        
        # Generate solution
        with torch.no_grad():
            outputs = self.model(
                input_ids=inputs['input_ids'],
                attention_mask=inputs['attention_mask']
            )
        
        # Extract solution
        solution_tokens = torch.argmax(outputs['output_logits'], dim=-1)
        solution_text = self.tokenizer.decode(solution_tokens)
        
        # Parse numerical answer
        numerical_answer = self._extract_numerical_answer(solution_text)
        
        # Verify solution
        is_correct = self._verify_solution(problem, numerical_answer)
        
        return {
            'problem': problem,
            'solution_text': solution_text,
            'numerical_answer': numerical_answer,
            'is_correct': is_correct,
            'confidence': outputs['confidence'].mean().item() if 'confidence' in outputs else 0.5
        }
    
    def _extract_numerical_answer(self, text: str) -> Optional[float]:
        """Extract numerical answer from solution text"""
        # Look for patterns like "The answer is X" or final numbers
        patterns = [
            r'answer is ([\d\.]+)',
            r'equals ([\d\.]+)',
            r'is ([\d\.]+)',
            r'([\d\.]+)\s*$'
        ]
        
        for pattern in patterns:
            match = re.search(pattern, text, re.IGNORECASE)
            if match:
                try:
                    return float(match.group(1))
                except ValueError:
                    continue
        
        return None
    
    def _verify_solution(self, problem: Dict, answer: float) -> bool:
        """Verify solution against known answer"""
        if 'answer' not in problem:
            return True  # Can't verify if no known answer
        
        try:
            expected_answer = float(problem['answer'])
            return abs(answer - expected_answer) < 1e-6
        except (ValueError, TypeError):
            return False
    
    def _physics_problems(self) -> List[Dict]:
        """Physics word problems"""
        return [
            {
                'id': 'phys_001',
                'problem': 'A ball is thrown vertically upward with an initial velocity of 20 m/s. What is its maximum height?',
                'answer': '20.4',  # Using h = v²/(2g)
                'difficulty': 'medium',
                'concepts': ['kinematics', 'energy_conservation', 'quadratic_equations']
            },
            {
                'id': 'phys_002',
                'problem': 'A block of mass m slides down an inclined plane at angle θ with coefficient of friction μ. Find the acceleration.',
                'answer': 'g(sinθ - μcosθ)',  # a = g(sinθ - μcosθ)
                'difficulty': 'hard',
                'concepts': ['newton_laws', 'friction', 'trigonometry']
            }
        ]
    
    def _engineering_problems(self) -> List[Dict]:
        """Engineering optimization problems"""
        return [
            {
                'id': 'eng_001',
                'problem': 'A rectangular beam of length L is simply supported at both ends. Find the maximum load it can support.',
                'answer': 'π²EI/(8L²)',  # Beam bending formula
                'difficulty': 'hard',
                'concepts': ['mechanics_of_materials', 'beam_bending', 'structural_analysis']
            },
            {
                'id': 'eng_002',
                'problem': 'A cylindrical tank of radius r and height h is filled with water. Find the time to empty through a hole at the bottom.',
                'answer': 'πr²h/(2A)',  # Torricelli's law
                'difficulty': 'medium',
                'concepts': ['fluid_dynamics', 'calculus', 'optimization']
            }
        ]
    
    def _finance_problems(self) -> List[Dict]:
        """Financial mathematics problems"""
        return [
            {
                'id': 'fin_001',
                'problem': 'An investment of $1000 earns 5% interest compounded annually. What is the value after 10 years?',
                'answer': '1628.89',  # 1000*(1.05)^10
                'difficulty': 'easy',
                'concepts': ['compound_interest', 'exponential_growth', 'financial_mathematics']
            },
            {
                'id': 'fin_002',
                'problem': 'A loan of $5000 has monthly payments of $450 for 3 years at 6% APR. What is the total interest paid?',
                'answer': '477.86',  # Total payment - principal
                'difficulty': 'medium',
                'concepts': ['loan_amortization', 'interest_calculation', 'time_value_of_money']
            }
        ]
    
    def _statistics_problems(self) -> List[Dict]:
        """Statistical analysis problems"""
        return [
            {
                'id': 'stat_001',
                'problem': 'A sample has mean μ and standard deviation σ. What is the probability that a randomly selected value is within 2σ of the mean?',
                'answer': '0.9544',  # Empirical rule
                'difficulty': 'easy',
                'concepts': ['normal_distribution', 'empirical_rule', 'probability_theory']
            },
            {
                'id': 'stat_002',
                'problem': 'In a hypothesis test, the null hypothesis is rejected with p-value 0.03. What conclusion can be drawn?',
                'answer': 'reject_null',
                'difficulty': 'medium',
                'concepts': ['hypothesis_testing', 'statistical_significance', 'p_value']
            }
        ]
```

**Success Criteria**:
- ✅ Real-world problem applications
- ✅ Domain-specific problem sets
- ✅ Solution verification and validation
- ✅ Performance analysis across domains

---

## 📊 **Assessment Rubric**

### **Quiz Scoring (100 points total)**
- **Transformer Architecture**: 40 points
- **Toolformer Integration**: 30 points
- **Performance Optimization**: 30 points

**Grade Scale**:
- **A**: 90-100 points (Excellent)
- **B**: 80-89 points (Good)
- **C**: 70-79 points (Satisfactory)
- **D**: 60-69 points (Needs Improvement)
- **F**: <60 points (Unsatisfactory)

### **Project Scoring (100 points total)**
- **Advanced Transformer Architecture**: 35 points
- **Training and Optimization**: 30 points
- **Performance Analysis**: 20 points
- **Mathematical Reasoning Applications**: 15 points

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
1. **Advanced Transformer Implementation**: Build optimized transformers from scratch
2. **Tool Integration**: Integrate external tools with LLMs
3. **Performance Optimization**: Optimize for efficiency and scalability
4. **Mathematical Applications**: Apply transformers to real-world problems
5. **Research Skills**: Conduct comprehensive performance analysis

### **Prerequisites for Next Phase**
- ✅ Advanced transformer architecture expertise
- ✅ Tool-augmented reasoning capabilities
- ✅ Performance optimization skills
- ✅ Real-world application experience
- ✅ Preparation for research-level projects

---

## 📅 **Timeline & Milestones**

### **Week 23**
- **Day 162-165**: Complete quiz preparation and assessment
- **Day 166-169**: Implement advanced transformer architecture
- **Day 170-172**: Develop training and optimization pipeline

### **Week 24**
- **Day 173-176**: Create performance analysis framework
- **Day 177-179**: Develop mathematical reasoning applications
- **Day 180-182**: Testing, optimization, and final submission

---

## 🚀 **Submission Guidelines**

### **Quiz Submission**
- **Format**: Written responses (PDF) + code solutions (Python files)
- **Deadline**: End of Week 23
- **Submission**: Upload to learning management system

### **Project Submission**
- **Format**: GitHub repository with complete implementation
- **Contents**: Source code, trained models, performance analysis, applications
- **Deadline**: End of Week 24
- **Submission**: Repository link + demonstration video (25 minutes)

---

**🎯 This bi-weekly assessment focuses on advanced transformer implementation with mathematical reasoning capabilities, preparing students for cutting-edge AI research and applications.**
