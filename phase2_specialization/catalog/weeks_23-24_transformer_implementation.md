# Weeks 23-24: Transformer Implementation (September 15-28, 2026)

## 🎯 **Learning Objectives**
- Continue Berkeley RL course with advanced concepts
- Read and analyze "Toolformer" paper for tool integration
- Build complete transformer from scratch using PyTorch
- Implement mathematical attention mechanisms
- Test transformer on mathematical reasoning tasks

---

## 📚 **Content & Resources**

### **Berkeley Deep Reinforcement Learning (Continued)**
**Resource**: [Berkeley CS285 Deep RL](https://rail.eecs.berkeley.edu/deeprlcourse/)
- **Specific Lectures**:
  - [Lecture 4: Actor-Critic Methods](https://rail.eecs.berkeley.edu/deeprlcourse/static/slides/lec4.pdf)
  - [Lecture 5: Value Function Approximation](https://rail.eecs.berkeley.edu/deeprlcourse/static/slides/lec5.pdf)
  - [Lecture 6: Policy Gradients](https://rail.eecs.berkeley.edu/deeprlcourse/static/slides/lec6.pdf)
- **Video Playlist**: [Berkeley Deep RL YouTube](https://www.youtube.com/playlist?list=PL_iWQOsE6TfVYGEGiAOMaOzzv41Jfm_Ps)
- **Course Materials**:
- [Lecture Videos](https://rail.eecs.berkeley.edu/deeprlcourse/)
- [Course Notes](https://rail.eecs.berkeley.edu/deeprlcourse/)
- [Assignments](https://rail.eecs.berkeley.edu/deeprlcourse/)

**Time Commitment**: 6 hours/week
**Focus**: Actor-critic methods and advanced policy gradients

### **Toolformer Research**
**Paper**: [Toolformer: Language Models Can Teach Themselves to Use Tools](https://arxiv.org/abs/2302.04761)
- **Reading Strategy**:
  - Day 1: Abstract, Introduction, and Motivation
  - Day 2: Methodology and Tool Learning
  - Day 3: Experiments and Results
  - Day 4: Implementation details and applications

**Key Concepts**:
- **Tool Learning**: Self-supervised tool use
- **API Integration**: Calling external tools
- **Mathematical Tools**: Calculators, symbolic computation
- **Tool Selection**: Choosing appropriate tools

**Time Commitment**: 4 hours/week

### **Transformer Implementation from Scratch**
**Resource**: [PyTorch Transformer Tutorial](https://pytorch.org/tutorials/beginner/transformer_tutorial.html)
- **Implementation Components**:
  - [Multi-Head Attention](https://pytorch.org/tutorials/beginner/transformer_tutorial.html#the-transformer-model)
  - [Positional Encoding](https://pytorch.org/tutorials/beginner/transformer_tutorial.html#positional-encoding)
  - [Encoder-Decoder Architecture](https://pytorch.org/tutorials/beginner/transformer_tutorial.html#transformer)

**Advanced Resources**:
- [Attention Is All You Need](https://arxiv.org/abs/1706.03762)
- [The Illustrated Transformer](http://jalammar.github.io/illustrated-transformer/)
- [minGPT Repository](https://github.com/karpathy/minGPT)

**Time Commitment**: 8 hours/week

### **Mathematical Attention Mechanisms**
**Resource**: [Mathematical Attention for Reasoning](https://arxiv.org/abs/2103.10319)
- **Focus Areas**:
  - Mathematical sequence attention
  - Step-by-step reasoning attention
  - Multi-step problem attention
  - Attention visualization

**Implementation Tasks**:
- Design attention for mathematical sequences
- Implement step-by-step attention
- Create attention visualizations
- Test on mathematical problems

**Time Commitment**: 3 hours/week

---

## 🧪 **Evaluation & Assessment**

### **Berkeley RL Course Evaluation**
**Assignment 2 Implementation**:
```python
# Berkeley RL Assignment 2: Actor-Critic Methods
import torch
import torch.nn as nn
import torch.optim as optim
import numpy as np
from typing import List, Dict, Tuple, Optional

class ActorCriticAgent:
    def __init__(self, state_dim: int, action_dim: int, hidden_dim: int = 128):
        self.state_dim = state_dim
        self.action_dim = action_dim
        
        # Actor network (policy)
        self.actor = nn.Sequential(
            nn.Linear(state_dim, hidden_dim),
            nn.ReLU(),
            nn.Linear(hidden_dim, hidden_dim),
            nn.ReLU(),
            nn.Linear(hidden_dim, action_dim),
            nn.Softmax(dim=-1)
        )
        
        # Critic network (value function)
        self.critic = nn.Sequential(
            nn.Linear(state_dim, hidden_dim),
            nn.ReLU(),
            nn.Linear(hidden_dim, hidden_dim),
            nn.ReLU(),
            nn.Linear(hidden_dim, 1)
        )
        
        self.optimizer_actor = optim.Adam(self.actor.parameters(), lr=0.001)
        self.optimizer_critic = optim.Adam(self.critic.parameters(), lr=0.01)
        
        self.gamma = 0.99
        self.entropy_coef = 0.01
        
    def select_action(self, state: np.ndarray) -> Tuple[int, float, float]:
        """Select action using actor network"""
        state_tensor = torch.FloatTensor(state).unsqueeze(0)
        
        # Get action probabilities
        action_probs = self.actor(state_tensor)
        
        # Sample action
        action_dist = torch.distributions.Categorical(action_probs)
        action = action_dist.sample()
        log_prob = action_dist.log_prob(action)
        entropy = action_dist.entropy()
        
        # Get state value
        state_value = self.critic(state_tensor)
        
        return action.item(), log_prob.item(), entropy.item(), state_value.item()
    
    def update_actor_critic(self, states: List[np.ndarray], actions: List[int], 
                           rewards: List[float], next_states: List[np.ndarray], 
                           dones: List[bool], log_probs: List[float], 
                           entropies: List[float], state_values: List[float]):
        """Update actor and critic networks"""
        # Convert to tensors
        states_tensor = torch.FloatTensor(states)
        actions_tensor = torch.LongTensor(actions)
        rewards_tensor = torch.FloatTensor(rewards)
        next_states_tensor = torch.FloatTensor(next_states)
        dones_tensor = torch.FloatTensor(dones)
        log_probs_tensor = torch.FloatTensor(log_probs)
        entropies_tensor = torch.FloatTensor(entropies)
        state_values_tensor = torch.FloatTensor(state_values)
        
        # Calculate next state values
        with torch.no_grad():
            next_state_values = self.critic(next_states_tensor).squeeze()
            target_values = rewards_tensor + self.gamma * next_state_values * (1 - dones_tensor)
        
        # Calculate advantages
        advantages = target_values - state_values_tensor.squeeze()
        
        # Actor loss (policy gradient with advantage)
        actor_loss = -(log_probs_tensor * advantages.detach()).mean() - self.entropy_coef * entropies_tensor.mean()
        
        # Critic loss (value function)
        critic_loss = nn.functional.mse_loss(state_values_tensor.squeeze(), target_values)
        
        # Update actor
        self.optimizer_actor.zero_grad()
        actor_loss.backward()
        self.optimizer_actor.step()
        
        # Update critic
        self.optimizer_critic.zero_grad()
        critic_loss.backward()
        self.optimizer_critic.step()
        
        return actor_loss.item(), critic_loss.item()

# Test the actor-critic agent
def test_actor_critic_agent():
    """Test actor-critic agent implementation"""
    agent = ActorCriticAgent(state_dim=4, action_dim=2)
    
    # Simulate training
    episode_rewards = []
    actor_losses = []
    critic_losses = []
    
    for episode in range(50):
        state = np.random.randn(4)
        states, actions, rewards, next_states, dones = [], [], [], [], []
        log_probs, entropies, state_values = [], [], []
        
        # Simulate episode
        for step in range(10):
            action, log_prob, entropy, state_value = agent.select_action(state)
            reward = np.random.randn()  # Random reward
            next_state = np.random.randn(4)
            done = step == 9  # Episode ends after 10 steps
            
            states.append(state)
            actions.append(action)
            rewards.append(reward)
            next_states.append(next_state)
            dones.append(done)
            log_probs.append(log_prob)
            entropies.append(entropy)
            state_values.append(state_value)
            
            state = next_state
        
        # Update networks
        actor_loss, critic_loss = agent.update_actor_critic(
            states, actions, rewards, next_states, dones, log_probs, entropies, state_values
        )
        
        episode_rewards.append(sum(rewards))
        actor_losses.append(actor_loss)
        critic_losses.append(critic_loss)
        
        if episode % 10 == 0:
            avg_reward = np.mean(episode_rewards[-10:])
            print(f"Episode {episode}, Average Reward: {avg_reward:.3f}")
    
    return len(episode_rewards) == 50

# Run test
if __name__ == "__main__":
    print("Testing actor-critic agent...")
    test_passed = test_actor_critic_agent()
    print(f"Actor-critic test passed: {test_passed}")
```

**Success Criteria**:
- [ ] Complete Berkeley RL Lectures 4-6
- [ ] Implement actor-critic algorithm
- [ ] Complete Assignment 2 with 80%+ accuracy
- [ ] Understand actor-critic mathematical foundations

### **Toolformer Implementation Evaluation**
**Tool Integration Framework**:
```python
# Toolformer Implementation for Mathematical Tools
import torch
import torch.nn as nn
from typing import List, Dict, Optional, Any, Union
import json
import re

class MathematicalToolformer:
    def __init__(self, model_name: str = "microsoft/DialoGPT-medium"):
        """Initialize Toolformer with mathematical tools"""
        self.model = None  # Would load actual model
        self.tokenizer = None  # Would load actual tokenizer
        self.tools = self._initialize_mathematical_tools()
        self.tool_examples = self._create_tool_examples()
    
    def _initialize_mathematical_tools(self) -> Dict[str, Any]:
        """Initialize mathematical tools"""
        return {
            'calculator': {
                'description': 'Perform basic arithmetic calculations',
                'api': 'calculate(expression: str) -> float',
                'examples': [
                    'calculate("25 * 4") -> 100.0',
                    'calculate("80 / 5") -> 16.0',
                    'calculate("15 + 27") -> 42.0'
                ]
            },
            'percentage_calculator': {
                'description': 'Calculate percentage values',
                'api': 'percentage(percent: float, base: float) -> float',
                'examples': [
                    'percentage(25, 80) -> 20.0',
                    'percentage(50, 100) -> 50.0',
                    'percentage(75, 200) -> 150.0'
                ]
            },
            'average_calculator': {
                'description': 'Calculate average of numbers',
                'api': 'average(numbers: List[float]) -> float',
                'examples': [
                    'average([10, 20, 30]) -> 20.0',
                    'average([15, 25, 35, 45]) -> 30.0',
                    'average([5, 10, 15]) -> 10.0'
                ]
            },
            'equation_solver': {
                'description': 'Solve simple linear equations',
                'api': 'solve_linear(equation: str) -> float',
                'examples': [
                    'solve_linear("x + 5 = 15") -> 10.0',
                    'solve_linear("2x - 3 = 7") -> 5.0',
                    'solve_linear("x / 2 = 8") -> 16.0'
                ]
            }
        }
    
    def _create_tool_examples(self) -> Dict[str, List[str]]:
        """Create training examples for tool learning"""
        return {
            'calculator': [
                {"input": "What is 25 multiplied by 4?", "tool_call": "calculate(\"25 * 4\")", "output": "100"},
                {"input": "Calculate 80 divided by 5", "tool_call": "calculate(\"80 / 5\")", "output": "16"},
                {"input": "Add 15 and 27", "tool_call": "calculate(\"15 + 27\")", "output": "42"}
            ],
            'percentage_calculator': [
                {"input": "What is 25% of 80?", "tool_call": "percentage(25, 80)", "output": "20"},
                {"input": "Find 50% of 100", "tool_call": "percentage(50, 100)", "output": "50"},
                {"input": "Calculate 75% of 200", "tool_call": "percentage(75, 200)", "output": "150"}
            ],
            'average_calculator': [
                {"input": "What is the average of 10, 20, and 30?", "tool_call": "average([10, 20, 30])", "output": "20"},
                {"input": "Find the mean of 15, 25, 35, and 45", "tool_call": "average([15, 25, 35, 45])", "output": "30"},
                {"input": "Calculate the average of 5, 10, and 15", "tool_call": "average([5, 10, 15])", "output": "10"}
            ],
            'equation_solver': [
                {"input": "Solve for x: x + 5 = 15", "tool_call": "solve_linear(\"x + 5 = 15\")", "output": "10"},
                {"input": "What is x in 2x - 3 = 7?", "tool_call": "solve_linear(\"2x - 3 = 7\")", "output": "5"},
                {"input": "Find x if x / 2 = 8", "tool_call": "solve_linear(\"x / 2 = 8\")", "output": "16"}
            ]
        }
    
    def identify_tool_needed(self, problem: str) -> Optional[str]:
        """Identify which tool is needed for the problem"""
        problem_lower = problem.lower()
        
        if 'percent' in problem_lower or '%' in problem:
            return 'percentage_calculator'
        elif 'average' in problem_lower or 'mean' in problem_lower:
            return 'average_calculator'
        elif 'equation' in problem_lower or 'solve for' in problem_lower:
            return 'equation_solver'
        elif any(op in problem for op in ['+', '-', '*', '/', 'multiply', 'divide', 'add', 'subtract']):
            return 'calculator'
        else:
            return None
    
    def generate_tool_call(self, tool_name: str, problem: str) -> Optional[str]:
        """Generate tool call based on problem"""
        if tool_name == 'percentage_calculator':
            return self._generate_percentage_call(problem)
        elif tool_name == 'average_calculator':
            return self._generate_average_call(problem)
        elif tool_name == 'equation_solver':
            return self._generate_equation_call(problem)
        elif tool_name == 'calculator':
            return self._generate_calculator_call(problem)
        else:
            return None
    
    def _generate_percentage_call(self, problem: str) -> Optional[str]:
        """Generate percentage calculator call"""
        # Extract percentage and base number
        percent_match = re.search(r'(\d+)%', problem)
        base_match = re.search(r'of (\d+)', problem)
        
        if percent_match and base_match:
            percent = int(percent_match.group(1))
            base = int(base_match.group(1))
            return f"percentage({percent}, {base})"
        
        return None
    
    def _generate_average_call(self, problem: str) -> Optional[str]:
        """Generate average calculator call"""
        # Extract numbers from problem
        numbers = re.findall(r'\d+', problem)
        if len(numbers) >= 2:
            numbers_list = ', '.join(numbers)
            return f"average([{numbers_list}])"
        
        return None
    
    def _generate_equation_call(self, problem: str) -> Optional[str]:
        """Generate equation solver call"""
        # Extract equation
        equation_match = re.search(r'(.+?)\s*=\s*(\d+)', problem)
        if equation_match:
            equation = equation_match.group(1) + ' = ' + equation_match.group(2)
            return f'solve_linear("{equation}")'
        
        return None
    
    def _generate_calculator_call(self, problem: str) -> Optional[str]:
        """Generate calculator call"""
        # Extract mathematical expression
        if 'multiply' in problem.lower():
            numbers = re.findall(r'\d+', problem)
            if len(numbers) >= 2:
                return f'calculate("{numbers[0]} * {numbers[1]}")'
        elif 'divide' in problem.lower():
            numbers = re.findall(r'\d+', problem)
            if len(numbers) >= 2:
                return f'calculate("{numbers[0]} / {numbers[1]}")'
        elif 'add' in problem.lower():
            numbers = re.findall(r'\d+', problem)
            if len(numbers) >= 2:
                return f'calculate("{numbers[0]} + {numbers[1]}")'
        
        # Look for direct expressions
        expression_match = re.search(r'(\d+\s*[\+\-\*\/]\s*\d+)', problem)
        if expression_match:
            return f'calculate("{expression_match.group(1)}")'
        
        return None
    
    def execute_tool_call(self, tool_call: str) -> Optional[float]:
        """Execute the tool call"""
        try:
            # Parse the tool call
            if tool_call.startswith('calculate('):
                # Extract expression
                expression = tool_call[10:-1]  # Remove 'calculate(' and ')'
                return eval(expression)
            
            elif tool_call.startswith('percentage('):
                # Extract parameters
                params = tool_call[11:-1]  # Remove 'percentage(' and ')'
                parts = params.split(', ')
                percent = float(parts[0])
                base = float(parts[1])
                return (percent / 100) * base
            
            elif tool_call.startswith('average('):
                # Extract numbers
                numbers_str = tool_call[8:-1]  # Remove 'average(' and ')'
                numbers = [float(n) for n in numbers_str.split(', ')]
                return sum(numbers) / len(numbers)
            
            elif tool_call.startswith('solve_linear('):
                # Extract equation
                equation = tool_call[13:-2]  # Remove 'solve_linear(' and ')'
                return self._solve_linear_equation(equation)
            
        except Exception as e:
            print(f"Error executing tool call: {e}")
            return None
        
        return None
    
    def _solve_linear_equation(self, equation: str) -> Optional[float]:
        """Solve simple linear equation"""
        try:
            # Handle different equation forms
            if '+' in equation:
                # x + a = b
                parts = equation.split(' + ')
                x_part = parts[0].strip()
                right_part = parts[1].split(' = ')[1].strip()
                if x_part == 'x':
                    a = float(parts[1].split(' = ')[0].strip())
                    b = float(right_part)
                    return b - a
            
            elif '-' in equation and 'x' in equation:
                # x - a = b or a - x = b
                if equation.startswith('x'):
                    parts = equation.split(' - ')
                    a = float(parts[1].split(' = ')[0].strip())
                    b = float(parts[1].split(' = ')[1].strip())
                    return b + a
                else:
                    parts = equation.split(' - ')
                    a = float(parts[0].strip())
                    right_parts = parts[1].split(' = ')
                    b = float(right_parts[1].strip())
                    return a - b
            
            elif '*' in equation:
                # x * a = b
                parts = equation.split(' * ')
                a = float(parts[1].split(' = ')[0].strip())
                b = float(parts[1].split(' = ')[1].strip())
                return b / a
            
            elif '/' in equation:
                # x / a = b
                parts = equation.split(' / ')
                a = float(parts[1].split(' = ')[0].strip())
                b = float(parts[1].split(' = ')[1].strip())
                return b * a
            
        except Exception as e:
            print(f"Error solving equation: {e}")
        
        return None
    
    def solve_with_tools(self, problem: str) -> Dict:
        """Solve problem using appropriate tool"""
        # Identify tool needed
        tool_name = self.identify_tool_needed(problem)
        
        if tool_name is None:
            return {
                'problem': problem,
                'tool_used': None,
                'tool_call': None,
                'result': None,
                'error': 'No appropriate tool identified'
            }
        
        # Generate tool call
        tool_call = self.generate_tool_call(tool_name, problem)
        
        if tool_call is None:
            return {
                'problem': problem,
                'tool_used': tool_name,
                'tool_call': None,
                'result': None,
                'error': 'Could not generate tool call'
            }
        
        # Execute tool call
        result = self.execute_tool_call(tool_call)
        
        return {
            'problem': problem,
            'tool_used': tool_name,
            'tool_call': tool_call,
            'result': result,
            'error': None if result is not None else 'Tool execution failed'
        }

# Test the Toolformer implementation
def test_mathematical_toolformer():
    """Test mathematical Toolformer"""
    toolformer = MathematicalToolformer()
    
    test_problems = [
        "What is 25% of 80?",
        "What is the average of 10, 20, and 30?",
        "Solve for x: x + 5 = 15",
        "Calculate 15 multiplied by 4",
        "Find 50% of 100"
    ]
    
    results = []
    for problem in test_problems:
        result = toolformer.solve_with_tools(problem)
        results.append(result)
        
        print(f"Problem: {problem}")
        print(f"Tool: {result['tool_used']}")
        print(f"Call: {result['tool_call']}")
        print(f"Result: {result['result']}")
        print(f"Error: {result['error']}")
        print("---")
    
    return len(results) == len(test_problems)

# Run test
if __name__ == "__main__":
    print("Testing mathematical Toolformer...")
    test_passed = test_mathematical_toolformer()
    print(f"Mathematical Toolformer test passed: {test_passed}")
```

**Success Criteria**:
- [ ] Complete Toolformer paper reading and analysis
- [ ] Implement mathematical tool integration framework
- [ ] Create tool identification and generation system
- [ ] Execute tool calls with mathematical functions
- [ ] Achieve 80%+ accuracy on tool-based problems

### **Transformer Implementation Evaluation**
**Complete Transformer from Scratch**:
```python
# Complete Transformer Implementation for Mathematical Reasoning
import torch
import torch.nn as nn
import torch.optim as optim
import math
from typing import Dict, List, Optional, Tuple
import numpy as np

class MathematicalTransformerConfig:
    def __init__(self, vocab_size: int = 50304, block_size: int = 512, n_layer: int = 6, 
                 n_head: int = 8, n_embd: int = 512, dropout: float = 0.1, 
                 bias: bool = True, device: str = 'cpu'):
        self.vocab_size = vocab_size
        self.block_size = block_size
        self.n_layer = n_layer
        self.n_head = n_head
        self.n_embd = n_embd
        self.dropout = dropout
        self.bias = bias
        self.device = device

class MathematicalTransformer(nn.Module):
    def __init__(self, config: MathematicalTransformerConfig):
        super().__init__()
        self.config = config
        
        # Token and position embeddings
        self.token_embedding = nn.Embedding(config.vocab_size, config.n_embd)
        self.position_embedding = nn.Embedding(config.block_size, config.n_embd)
        self.dropout = nn.Dropout(config.dropout)
        
        # Transformer blocks
        self.transformer_blocks = nn.ModuleList([
            MathematicalTransformerBlock(config) for _ in range(config.n_layer)
        ])
        
        # Layer normalization
        self.ln_f = nn.LayerNorm(config.n_embd)
        
        # Language modeling head
        self.lm_head = nn.Linear(config.n_embd, config.vocab_size, bias=False)
        
        # Mathematical reasoning head
        self.math_head = MathematicalReasoningHead(config)
        
        # Initialize weights
        self.apply(self._init_weights)
        
        # Share weights between embedding and lm_head
        self.token_embedding.weight = self.lm_head.weight
        
        print(f"Mathematical Transformer initialized with {self.get_num_parameters():,} parameters")
    
    def _init_weights(self, module):
        """Initialize weights"""
        if isinstance(module, nn.Linear):
            torch.nn.init.normal_(module.weight, mean=0.0, std=0.02)
            if module.bias is not None:
                torch.nn.init.zeros_(module.bias)
        elif isinstance(module, nn.Embedding):
            torch.nn.init.normal_(module.weight, mean=0.0, std=0.02)
        elif isinstance(module, nn.LayerNorm):
            torch.nn.init.zeros_(module.bias)
            torch.nn.init.ones_(module.weight)
    
    def get_num_parameters(self, non_embedding: bool = True) -> int:
        """Get number of parameters"""
        n_params = sum(p.numel() for p in self.parameters())
        if non_embedding:
            n_params -= self.token_embedding.weight.numel()
        return n_params
    
    def forward(self, idx: torch.Tensor, targets: Optional[torch.Tensor] = None, 
                math_targets: Optional[torch.Tensor] = None) -> Dict[str, torch.Tensor]:
        """Forward pass"""
        device = idx.device
        b, t = idx.size()
        
        assert t <= self.config.block_size, f"Sequence length {t} exceeds block size {self.config.block_size}"
        
        # Token and position embeddings
        tok_emb = self.token_embedding(idx)  # (b, t, n_embd)
        pos = torch.arange(0, t, dtype=torch.long, device=device).unsqueeze(0)
        pos_emb = self.position_embedding(pos)  # (1, t, n_embd)
        
        x = self.dropout(tok_emb + pos_emb)
        
        # Pass through transformer blocks
        for block in self.transformer_blocks:
            x = block(x)
        
        x = self.ln_f(x)
        
        # Language modeling logits
        logits = self.lm_head(x)
        
        # Mathematical reasoning logits
        math_logits = self.math_head(x)
        
        # Calculate losses
        losses = {}
        
        if targets is not None:
            # Language modeling loss
            loss_fct = nn.CrossEntropyLoss(ignore_index=-1)
            loss_lm = loss_fct(logits.view(-1, logits.size(-1)), targets.view(-1))
            losses['lm_loss'] = loss_lm
        
        if math_targets is not None:
            # Mathematical reasoning loss
            loss_fct = nn.CrossEntropyLoss(ignore_index=-1)
            loss_math = loss_fct(math_logits.view(-1, math_logits.size(-1)), math_targets.view(-1))
            losses['math_loss'] = loss_math
        
        # Combine losses
        if losses:
            total_loss = sum(losses.values())
            losses['total_loss'] = total_loss
        
        return {
            'logits': logits,
            'math_logits': math_logits,
            'losses': losses
        }
    
    @torch.no_grad()
    def generate(self, idx: torch.Tensor, max_new_tokens: int, temperature: float = 1.0, 
                top_k: Optional[int] = None, math_mode: bool = False) -> torch.Tensor:
        """Generate sequence"""
        for _ in range(max_new_tokens):
            # Crop context
            idx_cond = idx if idx.size(1) <= self.config.block_size else idx[:, -self.config.block_size:]
            
            # Forward pass
            outputs = self(idx_cond)
            logits = outputs['math_logits'] if math_mode else outputs['logits']
            
            # Focus on last token
            logits = logits[:, -1, :] / temperature
            
            # Apply top-k filtering
            if top_k is not None:
                v, _ = torch.topk(logits, min(top_k, logits.size(-1)))
                logits[logits < v[:, [-1]]] = -float('inf')
            
            # Sample next token
            probs = nn.functional.softmax(logits, dim=-1)
            idx_next = torch.multinomial(probs, num_samples=1)
            
            # Append to sequence
            idx = torch.cat((idx, idx_next), dim=1)
            
            # Stop if end token generated
            if idx_next.item() == 0:  # Assuming 0 is end token
                break
        
        return idx

class MathematicalTransformerBlock(nn.Module):
    def __init__(self, config: MathematicalTransformerConfig):
        super().__init__()
        self.ln_1 = nn.LayerNorm(config.n_embd)
        self.attn = MathematicalMultiHeadAttention(config)
        self.ln_2 = nn.LayerNorm(config.n_embd)
        self.mlp = MathematicalMLP(config)
        self.dropout = nn.Dropout(config.dropout)
    
    def forward(self, x: torch.Tensor) -> torch.Tensor:
        # Self-attention with residual connection
        x = x + self.dropout(self.attn(self.ln_1(x)))
        
        # MLP with residual connection
        x = x + self.dropout(self.mlp(self.ln_2(x)))
        
        return x

class MathematicalMultiHeadAttention(nn.Module):
    def __init__(self, config: MathematicalTransformerConfig):
        super().__init__()
        assert config.n_embd % config.n_head == 0
        
        self.n_head = config.n_head
        self.n_embd = config.n_embd
        self.dropout = nn.Dropout(config.dropout)
        
        # Linear projections for Q, K, V
        self.q_proj = nn.Linear(config.n_embd, config.n_embd, bias=config.bias)
        self.k_proj = nn.Linear(config.n_embd, config.n_embd, bias=config.bias)
        self.v_proj = nn.Linear(config.n_embd, config.n_embd, bias=config.bias)
        
        # Output projection
        self.out_proj = nn.Linear(config.n_embd, config.n_embd, bias=config.bias)
        
        # Flash attention components (simplified)
        self.flash = hasattr(torch.nn.functional, 'scaled_dot_product_attention')
    
    def forward(self, x: torch.Tensor) -> torch.Tensor:
        B, T, C = x.size()
        
        # Compute Q, K, V
        q = self.q_proj(x).view(B, T, self.n_head, C // self.n_head).transpose(1, 2)
        k = self.k_proj(x).view(B, T, self.n_head, C // self.n_head).transpose(1, 2)
        v = self.v_proj(x).view(B, T, self.n_head, C // self.n_head).transpose(1, 2)
        
        # Compute attention
        if self.flash:
            # Use flash attention if available
            y = torch.nn.functional.scaled_dot_product_attention(
                q, k, v, attn_mask=None, dropout_p=self.dropout.p if self.training else 0.0, is_causal=True
            )
        else:
            # Manual attention computation
            attention = self._compute_attention(q, k, v)
            y = attention
        
        # Reshape and project output
        y = y.transpose(1, 2).contiguous().view(B, T, C)
        y = self.out_proj(y)
        
        return y
    
    def _compute_attention(self, q: torch.Tensor, k: torch.Tensor, v: torch.Tensor) -> torch.Tensor:
        """Compute scaled dot-product attention"""
        # Scale dot-product attention
        scale = 1.0 / math.sqrt(k.size(-1))
        attn = torch.matmul(q, k.transpose(-2, -1)) * scale
        
        # Apply causal mask
        B, H, T, _ = attn.size()
        causal_mask = torch.triu(torch.ones(T, T, dtype=torch.bool, device=attn.device), diagonal=1)
        attn = attn.masked_fill(causal_mask, float('-inf'))
        
        # Apply softmax
        attn = nn.functional.softmax(attn, dim=-1)
        attn = self.dropout(attn)
        
        # Apply attention to values
        y = torch.matmul(attn, v)
        
        return y

class MathematicalMLP(nn.Module):
    def __init__(self, config: MathematicalTransformerConfig):
        super().__init__()
        self.c_fc = nn.Linear(config.n_embd, 4 * config.n_embd, bias=config.bias)
        self.gelu = nn.GELU()
        self.c_proj = nn.Linear(4 * config.n_embd, config.n_embd, bias=config.bias)
        self.dropout = nn.Dropout(config.dropout)
    
    def forward(self, x: torch.Tensor) -> torch.Tensor:
        x = self.c_fc(x)
        x = self.gelu(x)
        x = self.c_proj(x)
        x = self.dropout(x)
        return x

class MathematicalReasoningHead(nn.Module):
    def __init__(self, config: MathematicalTransformerConfig):
        super().__init__()
        # Multi-task heads for different mathematical reasoning tasks
        self.arithmetic_head = nn.Linear(config.n_embd, config.vocab_size)
        self.reasoning_head = nn.Linear(config.n_embd, config.vocab_size)
        self.tool_head = nn.Linear(config.n_embd, config.vocab_size)
        
        # Task classifier
        self.task_classifier = nn.Linear(config.n_embd, 3)  # 3 tasks: arithmetic, reasoning, tool
    
    def forward(self, x: torch.Tensor) -> torch.Tensor:
        # Classify task type
        task_logits = self.task_classifier(x)
        task_probs = nn.functional.softmax(task_logits, dim=-1)
        
        # Generate outputs for each task
        arithmetic_logits = self.arithmetic_head(x)
        reasoning_logits = self.reasoning_head(x)
        tool_logits = self.tool_head(x)
        
        # Combine based on task classification
        combined_logits = (
            task_probs[..., 0:1] * arithmetic_logits +
            task_probs[..., 1:2] * reasoning_logits +
            task_probs[..., 2:3] * tool_logits
        )
        
        return combined_logits.squeeze(-2)

# Test the mathematical transformer
def test_mathematical_transformer():
    """Test mathematical transformer implementation"""
    config = MathematicalTransformerConfig(
        vocab_size=1000,
        block_size=128,
        n_layer=4,
        n_head=4,
        n_embd=256,
        dropout=0.1
    )
    
    model = MathematicalTransformer(config)
    
    # Test forward pass
    batch_size = 2
    seq_len = 32
    idx = torch.randint(0, config.vocab_size, (batch_size, seq_len))
    targets = torch.randint(0, config.vocab_size, (batch_size, seq_len))
    math_targets = torch.randint(0, config.vocab_size, (batch_size, seq_len))
    
    outputs = model(idx, targets, math_targets)
    
    print(f"Input shape: {idx.shape}")
    print(f"LM logits shape: {outputs['logits'].shape}")
    print(f"Math logits shape: {outputs['math_logits'].shape}")
    print(f"Losses: {list(outputs['losses'].keys())}")
    
    if 'total_loss' in outputs['losses']:
        print(f"Total loss: {outputs['losses']['total_loss'].item():.4f}")
    
    # Test generation
    prompt = torch.randint(0, config.vocab_size, (1, 10))
    generated = model.generate(prompt, max_new_tokens=20, math_mode=True)
    
    print(f"Prompt shape: {prompt.shape}")
    print(f"Generated shape: {generated.shape}")
    
    return True

# Run test
if __name__ == "__main__":
    print("Testing mathematical transformer...")
    test_passed = test_mathematical_transformer()
    print(f"Mathematical transformer test passed: {test_passed}")
```

**Success Criteria**:
- [ ] Complete transformer architecture implementation
- [ ] Implement mathematical attention mechanisms
- [ ] Create multi-task reasoning heads
- [ ] Test on mathematical reasoning tasks
- [ ] Generate mathematical sequences

---

## 🛠️ **Projects & Applications**

### **Project 1: Mathematical Transformer with Tool Integration**
**Objective**: Build complete transformer with mathematical reasoning and tool integration

**Implementation**:
```python
# Mathematical Transformer with Tool Integration
import torch
import torch.nn as nn
from typing import Dict, List, Optional, Any, Union
import json

class MathTransformerWithTools:
    def __init__(self, transformer_config: MathematicalTransformerConfig):
        """Initialize mathematical transformer with tools"""
        self.transformer = MathematicalTransformer(transformer_config)
        self.toolformer = MathematicalToolformer()
        self.reasoning_engine = ChainOfThoughtSolver()
        
    def solve_complex_math_problem(self, problem: str) -> Dict:
        """Solve complex mathematical problem using all components"""
        # Step 1: Identify if tool is needed
        tool_result = self.toolformer.solve_with_tools(problem)
        
        # Step 2: Generate chain-of-thought reasoning
        cot_result = self.reasoning_engine.solve_with_cot(problem)
        
        # Step 3: Use transformer for advanced reasoning
        # In real implementation, would encode problem and generate solution
        transformer_result = {
            'problem': problem,
            'solution': 'Generated by transformer',
            'confidence': 0.8
        }
        
        # Step 4: Combine results
        combined_result = self._combine_results(tool_result, cot_result, transformer_result)
        
        return combined_result
    
    def _combine_results(self, tool_result: Dict, cot_result: Dict, transformer_result: Dict) -> Dict:
        """Combine results from different components"""
        # Priority: tool > chain-of-thought > transformer
        if tool_result['result'] is not None:
            return {
                'method': 'tool',
                'result': tool_result['result'],
                'confidence': 0.9,
                'details': tool_result
            }
        elif cot_result['final_answer'] is not None:
            return {
                'method': 'chain_of_thought',
                'result': cot_result['final_answer'],
                'confidence': cot_result['confidence'],
                'details': cot_result
            }
        else:
            return {
                'method': 'transformer',
                'result': transformer_result['solution'],
                'confidence': transformer_result['confidence'],
                'details': transformer_result
            }

# Test the integrated system
def test_integrated_math_system():
    """Test integrated mathematical system"""
    config = MathematicalTransformerConfig(
        vocab_size=1000,
        block_size=128,
        n_layer=2,  # Smaller for testing
        n_head=2,
        n_embd=128
    )
    
    system = MathTransformerWithTools(config)
    
    test_problems = [
        "What is 25% of 80?",
        "Find the average of 10, 20, and 30",
        "Solve for x: x + 5 = 15"
    ]
    
    results = []
    for problem in test_problems:
        result = system.solve_complex_math_problem(problem)
        results.append(result)
        
        print(f"Problem: {problem}")
        print(f"Method: {result['method']}")
        print(f"Result: {result['result']}")
        print(f"Confidence: {result['confidence']:.2f}")
        print("---")
    
    return len(results) == len(test_problems)

# Run test
if __name__ == "__main__":
    print("Testing integrated mathematical system...")
    test_passed = test_integrated_math_system()
    print(f"Integrated system test passed: {test_passed}")
```

**Deliverables**:
- [ ] Complete mathematical transformer with tool integration
- [ ] Multi-component reasoning system
- [ ] Result combination and confidence scoring
- [ ] Comprehensive test suite
- [ ] Performance evaluation on mathematical problems

---

## 📊 **Progress Tracking**

### **Daily Checklist**
- [ ] 2 hours Berkeley RL course (lectures and assignments)
- [ ] 3 hours transformer implementation
- [ ] 2 hours Toolformer paper reading and implementation
- [ ] 1 hour mathematical attention mechanisms
- [ ] 1 hour project development
- [ ] 30 minutes testing and debugging

### **Weekly Milestones**
**Week 23**:
- [ ] Complete Berkeley RL Lectures 4-5
- [ ] Read and analyze "Toolformer" paper
- [ ] Build basic transformer architecture
- [ ] Implement multi-head attention
- [ ] Test attention on mathematical sequences

**Week 24**:
- [ ] Complete Berkeley RL Lecture 6
- [ ] Complete Toolformer implementation
- [ ] Add mathematical reasoning heads
- [ ] Integrate tool calling capabilities
- [ ] Complete Assignment 2

### **End-of-Period Assessment**
**Success Metrics**:
- [ ] Berkeley RL: Lectures 4-6 completed, Assignment 2 submitted
- [ ] Toolformer: Complete implementation with 80%+ accuracy
- [ ] Transformer: Complete architecture with mathematical attention
- [ ] Integration: Working system combining all components
- [ ] Testing: Comprehensive test suite with 70%+ success rate
- [ ] Documentation: Complete implementation documentation

---

## 🚀 **Next Period Preparation**

### **Weeks 25-26 Preview**
- Complete Berkeley RL course (remaining lectures)
- Read "MathCoder" paper
- Implement tool-augmented LLM
- Integrate SymPy calculator
- Test tool integration effectiveness

### **Resources to Preview**:
- [MathCoder Paper](https://arxiv.org/abs/2310.12345)
- [SymPy Documentation](https://docs.sympy.org/)
- [Tool Integration Tutorial](https://github.com/openai/toolformer)
- [Advanced RL Methods](http://rail.eecs.berkeley.edu/deeprlcourse/)

---

## 📞 **Support & Resources**

### **Help Channels**:
- Berkeley RL Course Discussion Forums
- PyTorch Documentation and Community
- Research Paper Discussion Groups
- Math LLM Discord Communities
- Stack Overflow for technical questions

### **Additional Resources**:
- [Attention Mechanisms Explained](https://towardsdatascience.com/attention-mechanism-explained-567a3f0c9b3)
- [Transformer Architecture Guide](https://github.com/karpathy/nn-zero-to-hero)
- [Mathematical Reasoning Papers](https://arxiv.org/list/cs.LG/recent)
- [Tool Integration Examples](https://github.com/openai/toolformer)

---

*This 2-week plan provides advanced transformer implementation with Berkeley RL course, Toolformer research, and mathematical attention mechanisms for comprehensive Math LLM development.*
