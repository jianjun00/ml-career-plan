# Weeks 11-12: Implementation Skills - Quiz & Projects

## 📋 **Bi-Weekly Assessment Overview**
**Duration**: Weeks 11-12 (Implementation Skills)
**Focus: Machine learning implementation, model training, evaluation
**Assessment Type**: Quiz + Practical Project

---

## 🧮 **Weeks 11-12 Quiz: Machine Learning Implementation Assessment**

### **Section 1: Machine Learning Fundamentals (40%)**

#### **Question 1: Linear Models**
**Problem**: Implement and analyze:
1. Linear regression with gradient descent
2. Ridge and Lasso regularization
3. Logistic regression for classification
4. Multi-class classification strategies

**Scoring**: 10 points (2.5 points each model)

#### **Question 2: Neural Networks**
**Problem**: Design and implement:
1. Feedforward neural network architecture
2. Backpropagation algorithm
3. Activation functions and their derivatives
4. Weight initialization strategies

**Scoring**: 10 points (2.5 points each component)

#### **Question 3: Optimization Algorithms**
**Problem**: Compare and implement:
1. Stochastic gradient descent (SGD)
2. Momentum and Nesterov acceleration
3. Adam and RMSprop optimizers
4. Learning rate scheduling strategies

**Scoring**: 10 points (2.5 points each optimizer)

#### **Question 4: Model Evaluation**
**Problem**: Design evaluation metrics for:
1. Regression tasks (MSE, MAE, R²)
2. Classification tasks (accuracy, precision, recall, F1)
3. Multi-class classification (confusion matrix, macro/micro averaging)
4. Cross-validation and model selection

**Scoring**: 10 points (2.5 points each metric type)

### **Section 2: Advanced ML Concepts (30%)**

#### **Question 5: Decision Trees and Ensembles**
**Problem**: Implement and analyze:
1. Decision tree learning algorithm
2. Random forest ensemble method
3. Gradient boosting machines
4. Feature importance analysis

**Scoring**: 10 points (2.5 points each method)

#### **Question 6: Support Vector Machines**
**Problem**: Derive and implement:
1. Linear SVM formulation
2. Kernel trick and non-linear SVMs
3. Soft margin SVM
4. Multi-class SVM strategies

**Scoring**: 10 points (2.5 points each component)

#### **Question 7: Clustering Algorithms**
**Problem**: Implement and compare:
1. K-means clustering algorithm
2. Hierarchical clustering
3. DBSCAN density-based clustering
4. Clustering evaluation metrics

**Scoring**: 10 points (2.5 points each algorithm)

### **Section 3: Practical Applications (30%)**

#### **Question 8: Feature Engineering**
**Problem**: Design feature engineering for:
1. Text data (TF-IDF, word embeddings)
2. Image data (features, dimensionality reduction)
3. Time series data (lags, trends, seasonality)
4. Feature selection methods

**Scoring**: 10 points (2.5 points each data type)

#### **Question 9: Model Deployment**
**Problem**: Design deployment pipeline for:
1. Model serialization and loading
2. API development for ML models
3. Model monitoring and logging
4. A/B testing for model updates

**Scoring**: 10 points (2.5 points each component)

#### **Question 10: ML Pipeline**
**Problem**: Design complete ML pipeline for:
1. Data preprocessing and cleaning
2. Feature engineering and selection
3. Model training and validation
4. Model deployment and monitoring

**Scoring**: 10 points (2.5 points each pipeline stage)

---

## 🚀 **Weeks 11-12 Project: Simple Transformer Implementation

### **Project Overview**
**Objective**: Build complete transformer model for text classification from scratch
**Duration**: 2 weeks
**Technologies**: PyTorch, NumPy, Matplotlib, Hugging Face Datasets

### **Project Components**

#### **Component 1: Transformer Architecture (30%)**
**Requirements**:
- Complete encoder-only transformer
- Multi-head self-attention mechanism
- Positional encoding
- Feed-forward networks with layer normalization

**Deliverables**:
```python
import torch
import torch.nn as nn
import torch.nn.functional as F
import math

class TransformerEncoder(nn.Module):
    def __init__(self, vocab_size, d_model, n_heads, n_layers, d_ff, dropout=0.1):
        super().__init__()
        self.d_model = d_model
        self.n_heads = n_heads
        self.n_layers = n_layers
        
        # Embedding layers
        self.token_embedding = nn.Embedding(vocab_size, d_model)
        self.positional_encoding = PositionalEncoding(d_model)
        
        # Transformer layers
        self.layers = nn.ModuleList([
            TransformerLayer(d_model, n_heads, d_ff, dropout)
            for _ in range(n_layers)
        ])
        
        self.dropout = nn.Dropout(dropout)
    
    def forward(self, x, mask=None):
        # x: [batch_size, seq_len]
        
        # Token and positional embeddings
        token_emb = self.token_embedding(x)  # [batch_size, seq_len, d_model]
        pos_emb = self.positional_encoding(x)
        x = self.dropout(token_emb + pos_emb)
        
        # Pass through transformer layers
        for layer in self.layers:
            x = layer(x, mask)
        
        return x

class TransformerLayer(nn.Module):
    def __init__(self, d_model, n_heads, d_ff, dropout=0.1):
        super().__init__()
        self.self_attention = MultiHeadAttention(d_model, n_heads, dropout)
        self.feed_forward = FeedForward(d_model, d_ff, dropout)
        self.norm1 = nn.LayerNorm(d_model)
        self.norm2 = nn.LayerNorm(d_model)
        self.dropout = nn.Dropout(dropout)
    
    def forward(self, x, mask=None):
        # Self-attention with residual connection
        attn_output, _ = self.self_attention(x, x, x, mask)
        x = self.norm1(x + self.dropout(attn_output))
        
        # Feed-forward with residual connection
        ff_output = self.feed_forward(x)
        x = self.norm2(x + self.dropout(ff_output))
        
        return x

class MultiHeadAttention(nn.Module):
    def __init__(self, d_model, n_heads, dropout=0.1):
        super().__init__()
        assert d_model % n_heads == 0
        
        self.d_model = d_model
        self.n_heads = n_heads
        self.d_k = d_model // n_heads
        
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
        K = self.W_k(key)
        V = self.W_v(value)
        
        # Reshape for multi-head attention
        Q = Q.view(batch_size, -1, self.n_heads, self.d_k).transpose(1, 2)
        K = K.view(batch_size, -1, self.n_heads, self.d_k).transpose(1, 2)
        V = V.view(batch_size, -1, self.n_heads, self.d_k).transpose(1, 2)
        
        # Apply attention
        context, attention_weights = self.attention(Q, K, V, mask)
        
        # Concatenate heads
        context = context.transpose(1, 2).contiguous().view(
            batch_size, -1, self.d_model
        )
        
        # Output projection
        output = self.W_o(context)
        
        return output, attention_weights

class ScaledDotProductAttention(nn.Module):
    def __init__(self, d_k, dropout=0.1):
        super().__init__()
        self.d_k = d_k
        self.dropout = nn.Dropout(dropout)
        self.scale = 1.0 / math.sqrt(d_k)
    
    def forward(self, Q, K, V, mask=None):
        # Q, K, V: [batch_size, n_heads, seq_len, d_k]
        
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
        # x: [batch_size, seq_len]
        seq_len = x.size(1)
        return self.pe[:seq_len, :].transpose(0, 1)

class FeedForward(nn.Module):
    def __init__(self, d_model, d_ff, dropout=0.1):
        super().__init__()
        self.linear1 = nn.Linear(d_model, d_ff)
        self.linear2 = nn.Linear(d_ff, d_model)
        self.dropout = nn.Dropout(dropout)
        self.activation = nn.ReLU()
    
    def forward(self, x):
        x = self.linear1(x)
        x = self.activation(x)
        x = self.dropout(x)
        x = self.linear2(x)
        return x
```

**Success Criteria**:
- ✅ Correct transformer architecture implementation
- ✅ Proper attention mechanism
- ✅ Efficient computation with masking
- ✅ Gradient flow verification

#### **Component 2: Training Pipeline (25%)**
**Requirements**:
- Data loading and preprocessing
- Training loop with loss tracking
- Validation and early stopping
- Model checkpointing and resumption

**Deliverables**:
```python
import torch
import torch.optim as optim
from torch.utils.data import DataLoader, Dataset
from datasets import load_dataset
from transformers import AutoTokenizer
import numpy as np
from typing import Dict, List, Tuple

class TextClassificationDataset(Dataset):
    def __init__(self, texts, labels, tokenizer, max_length=256):
        self.texts = texts
        self.labels = labels
        self.tokenizer = tokenizer
        self.max_length = max_length
    
    def __len__(self):
        return len(self.texts)
    
    def __getitem__(self, idx):
        text = self.texts[idx]
        label = self.labels[idx]
        
        encoding = self.tokenizer(
            text,
            truncation=True,
            padding='max_length',
            max_length=self.max_length,
            return_tensors='pt'
        )
        
        return {
            'input_ids': encoding['input_ids'].flatten(),
            'attention_mask': encoding['attention_mask'].flatten(),
            'labels': torch.tensor(label, dtype=torch.long)
        }

class TransformerTrainer:
    def __init__(self, model, train_loader, val_loader, device='cuda'):
        self.model = model
        self.train_loader = train_loader
        self.val_loader = val_loader
        self.device = device
        self.model.to(device)
        
        # Training components
        self.criterion = nn.CrossEntropyLoss()
        self.optimizer = optim.AdamW(model.parameters(), lr=1e-4)
        self.scheduler = optim.lr_scheduler.ReduceLROnPlateau(
            self.optimizer, mode='min', patience=2, factor=0.5
        )
        
        # Training history
        self.train_losses = []
        self.val_losses = []
        self.val_accuracies = []
        self.best_val_loss = float('inf')
        self.patience_counter = 0
        self.max_patience = 5
    
    def train_epoch(self):
        self.model.train()
        total_loss = 0
        num_batches = 0
        
        for batch in self.train_loader:
            input_ids = batch['input_ids'].to(self.device)
            attention_mask = batch['attention_mask'].to(self.device)
            labels = batch['labels'].to(self.device)
            
            self.optimizer.zero_grad()
            
            # Forward pass
            outputs = self.model(input_ids, attention_mask)
            loss = self.criterion(outputs, labels)
            
            # Backward pass
            loss.backward()
            torch.nn.utils.clip_grad_norm_(self.model.parameters(), 1.0)
            self.optimizer.step()
            
            total_loss += loss.item()
            num_batches += 1
        
        return total_loss / num_batches
    
    def validate(self):
        self.model.eval()
        total_loss = 0
        correct_predictions = 0
        total_predictions = 0
        
        with torch.no_grad():
            for batch in self.val_loader:
                input_ids = batch['input_ids'].to(self.device)
                attention_mask = batch['attention_mask'].to(self.device)
                labels = batch['labels'].to(self.device)
                
                outputs = self.model(input_ids, attention_mask)
                loss = self.criterion(outputs, labels)
                
                total_loss += loss.item()
                
                # Calculate accuracy
                predictions = torch.argmax(outputs, dim=1)
                correct_predictions += (predictions == labels).sum().item()
                total_predictions += labels.size(0)
        
        avg_loss = total_loss / len(self.val_loader)
        accuracy = correct_predictions / total_predictions
        
        return avg_loss, accuracy
    
    def train(self, num_epochs, save_path='best_model.pth'):
        for epoch in range(num_epochs):
            # Training
            train_loss = self.train_epoch()
            
            # Validation
            val_loss, val_accuracy = self.validate()
            
            # Update learning rate
            self.scheduler.step(val_loss)
            
            # Record history
            self.train_losses.append(train_loss)
            self.val_losses.append(val_loss)
            self.val_accuracies.append(val_accuracy)
            
            print(f'Epoch {epoch+1}/{num_epochs}:')
            print(f'  Train Loss: {train_loss:.4f}')
            print(f'  Val Loss: {val_loss:.4f}')
            print(f'  Val Accuracy: {val_accuracy:.4f}')
            
            # Early stopping and model saving
            if val_loss < self.best_val_loss:
                self.best_val_loss = val_loss
                self.patience_counter = 0
                torch.save({
                    'epoch': epoch,
                    'model_state_dict': self.model.state_dict(),
                    'optimizer_state_dict': self.optimizer.state_dict(),
                    'val_loss': val_loss,
                    'val_accuracy': val_accuracy,
                }, save_path)
                print(f'  New best model saved!')
            else:
                self.patience_counter += 1
                if self.patience_counter >= self.max_patience:
                    print(f'Early stopping at epoch {epoch+1}')
                    break
    
    def load_best_model(self, load_path='best_model.pth'):
        checkpoint = torch.load(load_path, map_location=self.device)
        self.model.load_state_dict(checkpoint['model_state_dict'])
        return checkpoint['val_loss'], checkpoint['val_accuracy']
```

**Success Criteria**:
- ✅ Complete training pipeline
- ✅ Proper data handling
- ✅ Early stopping implementation
- ✅ Model checkpointing

#### **Component 3: Text Classification Head (20%)**
**Requirements**:
- Classification head for sentiment analysis
- Multi-class classification support
- Confidence scoring and calibration
- Error analysis and interpretability

**Deliverables**:
```python
class TextClassificationTransformer(nn.Module):
    def __init__(self, encoder, num_classes, dropout=0.1):
        super().__init__()
        self.encoder = encoder
        self.dropout = nn.Dropout(dropout)
        self.classifier = nn.Linear(encoder.d_model, num_classes)
    
    def forward(self, input_ids, attention_mask=None):
        # Get encoder outputs
        encoder_outputs = self.encoder(input_ids, attention_mask)
        
        # Use [CLS] token representation (first token)
        cls_representation = encoder_outputs[:, 0, :]
        
        # Apply dropout and classification
        x = self.dropout(cls_representation)
        logits = self.classifier(x)
        
        return logits

class ModelEvaluator:
    def __init__(self, model, device='cuda'):
        self.model = model
        self.device = device
    
    def predict(self, text, tokenizer, max_length=256):
        self.model.eval()
        
        # Tokenize input
        encoding = tokenizer(
            text,
            truncation=True,
            padding='max_length',
            max_length=max_length,
            return_tensors='pt'
        )
        
        input_ids = encoding['input_ids'].to(self.device)
        attention_mask = encoding['attention_mask'].to(self.device)
        
        with torch.no_grad():
            logits = self.model(input_ids, attention_mask)
            probabilities = F.softmax(logits, dim=1)
            prediction = torch.argmax(probabilities, dim=1)
        
        return {
            'prediction': prediction.item(),
            'probabilities': probabilities.cpu().numpy()[0],
            'confidence': probabilities.max().item()
        }
    
    def evaluate_dataset(self, test_loader, class_names=None):
        self.model.eval()
        all_predictions = []
        all_labels = []
        all_probabilities = []
        
        with torch.no_grad():
            for batch in test_loader:
                input_ids = batch['input_ids'].to(self.device)
                attention_mask = batch['attention_mask'].to(self.device)
                labels = batch['labels'].to(self.device)
                
                logits = self.model(input_ids, attention_mask)
                probabilities = F.softmax(logits, dim=1)
                predictions = torch.argmax(probabilities, dim=1)
                
                all_predictions.extend(predictions.cpu().numpy())
                all_labels.extend(labels.cpu().numpy())
                all_probabilities.extend(probabilities.cpu().numpy())
        
        # Calculate metrics
        from sklearn.metrics import accuracy_score, classification_report, confusion_matrix
        
        accuracy = accuracy_score(all_labels, all_predictions)
        report = classification_report(all_labels, all_predictions, 
                                   target_names=class_names, output_dict=True)
        cm = confusion_matrix(all_labels, all_predictions)
        
        return {
            'accuracy': accuracy,
            'classification_report': report,
            'confusion_matrix': cm,
            'predictions': all_predictions,
            'labels': all_labels,
            'probabilities': all_probabilities
        }
```

**Success Criteria**:
- ✅ Accurate text classification
- ✅ Proper confidence scoring
- ✅ Comprehensive evaluation metrics
- ✅ Error analysis capabilities

#### **Component 4: Performance Analysis (25%)**
**Requirements**:
- Learning curves visualization
- Hyperparameter optimization
- Model comparison with baselines
- Ablation studies

**Deliverables**:
```python
import matplotlib.pyplot as plt
import seaborn as sns
from sklearn.model_selection import ParameterGrid
import pandas as pd

class PerformanceAnalyzer:
    def __init__(self):
        self.results = {}
        self.figures = {}
    
    def plot_learning_curves(self, train_losses, val_losses, val_accuracies):
        """Plot training and validation curves"""
        fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(15, 5))
        
        # Loss curves
        ax1.plot(train_losses, label='Training Loss')
        ax1.plot(val_losses, label='Validation Loss')
        ax1.set_xlabel('Epoch')
        ax1.set_ylabel('Loss')
        ax1.set_title('Training and Validation Loss')
        ax1.legend()
        ax1.grid(True)
        
        # Accuracy curve
        ax2.plot(val_accuracies, label='Validation Accuracy', color='green')
        ax2.set_xlabel('Epoch')
        ax2.set_ylabel('Accuracy')
        ax2.set_title('Validation Accuracy')
        ax2.legend()
        ax2.grid(True)
        
        plt.tight_layout()
        return fig
    
    def hyperparameter_search(self, model_class, train_loader, val_loader, 
                          param_grid, search_type='grid'):
        """Perform hyperparameter optimization"""
        results = []
        
        if search_type == 'grid':
            param_combinations = list(ParameterGrid(param_grid))
        else:
            # Random search implementation
            param_combinations = self._random_search(param_grid, n_trials=20)
        
        for params in param_combinations:
            print(f"Testing parameters: {params}")
            
            # Create model with current parameters
            model = model_class(**params)
            trainer = TransformerTrainer(model, train_loader, val_loader)
            
            # Train model
            trainer.train(num_epochs=10)
            
            # Evaluate
            val_loss, val_accuracy = trainer.load_best_model()
            
            results.append({
                'params': params,
                'val_loss': val_loss,
                'val_accuracy': val_accuracy
            })
        
        # Convert to DataFrame for analysis
        df = pd.DataFrame(results)
        
        # Find best parameters
        best_idx = df['val_accuracy'].idxmax()
        best_params = df.loc[best_idx, 'params']
        best_accuracy = df.loc[best_idx, 'val_accuracy']
        
        return df, best_params, best_accuracy
    
    def compare_models(self, model_results, model_names):
        """Compare multiple models"""
        fig, axes = plt.subplots(2, 2, figsize=(15, 10))
        
        # Accuracy comparison
        accuracies = [result['accuracy'] for result in model_results]
        axes[0, 0].bar(model_names, accuracies)
        axes[0, 0].set_title('Model Accuracy Comparison')
        axes[0, 0].set_ylabel('Accuracy')
        
        # Training loss comparison
        for i, (result, name) in enumerate(zip(model_results, model_names)):
            if 'train_losses' in result:
                axes[0, 1].plot(result['train_losses'], label=name)
        axes[0, 1].set_title('Training Loss Comparison')
        axes[0, 1].set_xlabel('Epoch')
        axes[0, 1].set_ylabel('Loss')
        axes[0, 1].legend()
        
        # Validation loss comparison
        for i, (result, name) in enumerate(zip(model_results, model_names)):
            if 'val_losses' in result:
                axes[1, 0].plot(result['val_losses'], label=name)
        axes[1, 0].set_title('Validation Loss Comparison')
        axes[1, 0].set_xlabel('Epoch')
        axes[1, 0].set_ylabel('Loss')
        axes[1, 0].legend()
        
        # Confusion matrices
        for i, (result, name) in enumerate(zip(model_results, model_names)):
            if 'confusion_matrix' in result:
                sns.heatmap(result['confusion_matrix'], 
                          annot=True, fmt='d', ax=axes[1, 1])
                axes[1, 1].set_title(f'Confusion Matrix - {name}')
                break  # Show first model's confusion matrix
        
        plt.tight_layout()
        return fig
    
    def ablation_study(self, base_model, ablation_configs, train_loader, val_loader):
        """Perform ablation study"""
        results = []
        
        for config_name, config in ablation_configs.items():
            print(f"Running ablation: {config_name}")
            
            # Create model with ablation configuration
            model = base_model(**config)
            trainer = TransformerTrainer(model, train_loader, val_loader)
            
            # Train and evaluate
            trainer.train(num_epochs=10)
            val_loss, val_accuracy = trainer.load_best_model()
            
            results.append({
                'config': config_name,
                'val_loss': val_loss,
                'val_accuracy': val_accuracy
            })
        
        # Plot results
        fig, ax = plt.subplots(figsize=(10, 6))
        config_names = [r['config'] for r in results]
        accuracies = [r['val_accuracy'] for r in results]
        
        bars = ax.bar(config_names, accuracies)
        ax.set_title('Ablation Study Results')
        ax.set_ylabel('Validation Accuracy')
        ax.set_xlabel('Configuration')
        
        # Add value labels on bars
        for bar, acc in zip(bars, accuracies):
            height = bar.get_height()
            ax.text(bar.get_x() + bar.get_width()/2., height,
                   f'{acc:.3f}', ha='center', va='bottom')
        
        plt.xticks(rotation=45)
        plt.tight_layout()
        
        return fig, results
```

**Success Criteria**:
- ✅ Comprehensive performance analysis
- ✅ Hyperparameter optimization
- ✅ Model comparison capabilities
- ✅ Alation study implementation

---

## 📊 **Assessment Rubric**

### **Quiz Scoring (100 points total)**
- **Machine Learning Fundamentals**: 40 points
- **Advanced ML Concepts**: 30 points
- **Practical Applications**: 30 points

**Grade Scale**:
- **A**: 90-100 points (Excellent)
- **B**: 80-89 points (Good)
- **C**: 70-79 points (Satisfactory)
- **D**: 60-69 points (Needs Improvement)
- **F**: <60 points (Unsatisfactory)

### **Project Scoring (100 points total)**
- **Transformer Architecture**: 30 points
- **Training Pipeline**: 25 points
- **Text Classification Head**: 20 points
- **Performance Analysis**: 25 points

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
1. **Transformer Implementation**: Build complete transformer models
2. **Machine Learning**: Understand and implement ML algorithms
3. **Model Training**: Train and evaluate deep learning models
4. **Performance Analysis**: Analyze and optimize model performance
5. **Text Processing**: Apply transformers to NLP tasks

### **Prerequisites for Next Phase**
- ✅ Strong machine learning foundation
- ✅ Transformer architecture understanding
- ✅ Model training experience
- ✅ Preparation for advanced NLP applications

---

## 📅 **Timeline & Milestones**

### **Week 11**
- **Day 71-72**: Complete quiz preparation and assessment
- **Day 73-75**: Implement transformer architecture
- **Day 76-77**: Develop training pipeline

### **Week 12**
- **Day 78-80**: Create text classification head
- **Day 81-83**: Implement performance analysis tools
- **Day 84**: Testing, documentation, and final submission

---

## 🚀 **Submission Guidelines**

### **Quiz Submission**
- **Format**: Written responses (PDF) + code solutions (Python files)
- **Deadline**: End of Week 11
- **Submission**: Upload to learning management system

### **Project Submission**
- **Format**: GitHub repository with complete implementation
- **Contents**: Source code, trained models, evaluation results, documentation
- **Deadline**: End of Week 12
- **Submission**: Repository link + demonstration video (15 minutes)

---

**🎯 This bi-weekly assessment focuses on machine learning implementation and transformer architecture, preparing students for advanced NLP applications.**
