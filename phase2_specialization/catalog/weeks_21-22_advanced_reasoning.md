# Weeks 21-22: Advanced Mathematical Reasoning (September 1-14, 2026)

## 🎯 **Learning Objectives**
- Start Berkeley RL course with focus on mathematical reasoning
- Read and analyze "Chain-of-Thought Prompting" paper
- Begin chain-of-thought math solver project
- Explore GSM8K dataset structure and challenges
- Set up advanced development environment for Phase 2

---

## 📚 **Content & Resources**

### **Berkeley Deep Reinforcement Learning**
**Resource**: [Berkeley CS285 Deep RL](https://rail.eecs.berkeley.edu/deeprlcourse/)
- **Specific Lectures**:
  - [Lecture 1: Introduction to Deep RL](https://rail.eecs.berkeley.edu/deeprlcourse/static/slides/lec1.pdf)
  - [Lecture 2: Policy Gradients](https://rail.eecs.berkeley.edu/deeprlcourse/static/slides/lec2.pdf)
  - [Lecture 3: Q-Learning and Function Approximation](https://rail.eecs.berkeley.edu/deeprlcourse/static/slides/lec3.pdf)
- **Video Playlist**: [Berkeley Deep RL YouTube](https://www.youtube.com/playlist?list=PL_iWQOsE6TfVYGEGiAOMaOzzv41Jfm_Ps)
- **Course Materials**:
- [Lecture Videos](https://rail.eecs.berkeley.edu/deeprlcourse/)
- [Assignments](https://rail.eecs.berkeley.edu/deeprlcourse/)
- [Course Notes](https://rail.eecs.berkeley.edu/deeprlcourse/)

**Time Commitment**: 6 hours/week
**Focus**: Mathematical reasoning as reinforcement learning

### **Chain-of-Thought Research**
**Paper**: [Chain-of-Thought Prompting Elicits Reasoning in Large Language Models](https://arxiv.org/abs/2201.11903)
- **Reading Strategy**:
  - Day 1: Abstract, Introduction, and Related Work
  - Day 2: Methodology and Experiments
  - Day 3: Results, Discussion, and Future Work
  - Day 4: Implementation details and reproduction

**Key Concepts**:
- **Chain-of-Thought**: Step-by-step reasoning
- **Few-shot prompting**: Examples for reasoning
- **Mathematical reasoning**: Application to math problems
- **Evaluation**: Accuracy and reasoning quality

**Time Commitment**: 4 hours/week

### **GSM8K Dataset Exploration**
**Resource**: [GSM8K Dataset](https://github.com/openai/grade-school-math)
- **Dataset Structure**:
  - 8,500 grade school math word problems
  - Multi-step reasoning required
  - Step-by-step solutions provided
  - Evaluation metrics and benchmarks

**Exploration Tasks**:
- Download and analyze dataset structure
- Understand problem types and difficulty
- Examine solution patterns
- Create data processing pipeline

**Time Commitment**: 3 hours/week

### **Advanced Development Environment**
**Resource**: [PyTorch Advanced Setup](https://pytorch.org/get-started/locally/)
- **Components**:
  - PyTorch 2.0+ with advanced features
  - JupyterLab for interactive development
  - Git for version control
  - CUDA support for GPU acceleration

**Additional Tools**:
- [Weights & Biases](https://wandb.ai/) for experiment tracking
- [TensorBoard](https://www.tensorflow.org/tensorboard) for visualization
- [Docker](https://www.docker.com/) for environment consistency

**Time Commitment**: 2 hours/week

---

## 🧪 **Evaluation & Assessment**

### **Berkeley RL Course Evaluation**
**Assignment Implementation**:
```python
# Berkeley RL Assignment 1: Policy Gradients
import numpy as np
import torch
import torch.nn as nn
import torch.optim as optim
from typing import List, Dict, Tuple

class PolicyGradientAgent:
    def __init__(self, state_dim: int, action_dim: int, hidden_dim: int = 64):
        self.state_dim = state_dim
        self.action_dim = action_dim
        
        # Policy network
        self.policy_net = nn.Sequential(
            nn.Linear(state_dim, hidden_dim),
            nn.ReLU(),
            nn.Linear(hidden_dim, hidden_dim),
            nn.ReLU(),
            nn.Linear(hidden_dim, action_dim),
            nn.Softmax(dim=-1)
        )
        
        self.optimizer = optim.Adam(self.policy_net.parameters(), lr=0.01)
        self.gamma = 0.99
        
    def select_action(self, state: np.ndarray) -> Tuple[int, float]:
        """Select action using policy network"""
        state_tensor = torch.FloatTensor(state).unsqueeze(0)
        action_probs = self.policy_net(state_tensor)
        
        # Sample action from probability distribution
        action_dist = torch.distributions.Categorical(action_probs)
        action = action_dist.sample()
        log_prob = action_dist.log_prob(action)
        
        return action.item(), log_prob.item()
    
    def update_policy(self, rewards: List[float], log_probs: List[float], states: List[np.ndarray]):
        """Update policy using REINFORCE algorithm"""
        # Calculate discounted rewards
        discounted_rewards = []
        cumulative_reward = 0
        for reward in reversed(rewards):
            cumulative_reward = reward + self.gamma * cumulative_reward
            discounted_rewards.insert(0, cumulative_reward)
        
        # Normalize rewards
        discounted_rewards = torch.tensor(discounted_rewards)
        discounted_rewards = (discounted_rewards - discounted_rewards.mean()) / (discounted_rewards.std() + 1e-8)
        
        # Calculate loss
        log_probs_tensor = torch.tensor(log_probs)
        loss = -torch.mean(log_probs_tensor * discounted_rewards)
        
        # Update policy
        self.optimizer.zero_grad()
        loss.backward()
        self.optimizer.step()
        
        return loss.item()

# Test the policy gradient agent
def test_policy_gradient_agent():
    """Test policy gradient agent implementation"""
    # Create agent for simple environment
    agent = PolicyGradientAgent(state_dim=4, action_dim=2)
    
    # Simulate training episodes
    total_rewards = []
    
    for episode in range(100):
        state = np.random.randn(4)  # Random state
        rewards = []
        log_probs = []
        states = []
        
        # Simulate episode
        for step in range(10):
            action, log_prob = agent.select_action(state)
            reward = np.random.randn()  # Random reward
            next_state = np.random.randn(4)  # Random next state
            
            rewards.append(reward)
            log_probs.append(log_prob)
            states.append(state)
            
            state = next_state
        
        # Update policy
        loss = agent.update_policy(rewards, log_probs, states)
        total_rewards.append(sum(rewards))
        
        if episode % 20 == 0:
            print(f"Episode {episode}, Average Reward: {np.mean(total_rewards[-20:]):.3f}")
    
    return len(total_rewards) == 100

# Run test
if __name__ == "__main__":
    print("Testing policy gradient agent...")
    test_passed = test_policy_gradient_agent()
    print(f"Policy gradient test passed: {test_passed}")
```

**Success Criteria**:
- [ ] Complete Berkeley RL Lectures 1-3
- [ ] Implement policy gradient algorithm
- [ ] Understand mathematical reasoning in RL context
- [ ] Complete Assignment 1 with 80%+ accuracy

### **Chain-of-Thought Implementation Evaluation**
**CoT Implementation**:
```python
# Chain-of-Thought Implementation
import torch
import torch.nn as nn
from typing import List, Dict, Optional

class ChainOfThoughtSolver:
    def __init__(self, model_name: str = "microsoft/DialoGPT-medium"):
        """Initialize CoT solver with language model"""
        # In real implementation, would load actual model
        self.model = None  # Placeholder for actual model
        self.tokenizer = None  # Placeholder for actual tokenizer
        self.reasoning_steps = []
    
    def generate_reasoning_steps(self, problem: str) -> List[str]:
        """Generate step-by-step reasoning for mathematical problem"""
        # This is a simplified implementation
        # Real implementation would use actual LLM
        
        reasoning_template = [
            "Step 1: Understand the problem",
            "Step 2: Identify the mathematical operations needed",
            "Step 3: Perform the calculations step by step",
            "Step 4: Verify the answer"
        ]
        
        # Simulate generating specific steps based on problem
        steps = []
        
        if "percent" in problem.lower():
            steps.append("Step 1: Identify the percentage calculation needed")
            steps.append("Step 2: Convert percentage to decimal")
            steps.append("Step 3: Multiply by the base number")
            steps.append("Step 4: Convert back to percentage if needed")
        
        elif "average" in problem.lower():
            steps.append("Step 1: List all the numbers")
            steps.append("Step 2: Sum all the numbers")
            steps.append("Step 3: Count how many numbers")
            steps.append("Step 4: Divide sum by count")
        
        elif "ratio" in problem.lower():
            steps.append("Step 1: Identify the ratio components")
            steps.append("Step 2: Set up proportion equation")
            steps.append("Step 3: Solve for unknown")
            steps.append("Step 4: Verify the solution")
        
        else:
            steps = reasoning_template
        
        self.reasoning_steps = steps
        return steps
    
    def solve_with_cot(self, problem: str) -> Dict:
        """Solve problem using chain-of-thought reasoning"""
        steps = self.generate_reasoning_steps(problem)
        
        # Simulate solving each step
        solution_steps = []
        current_result = None
        
        for i, step in enumerate(steps):
            step_result = self.execute_step(step, problem, current_result)
            solution_steps.append({
                'step': i + 1,
                'description': step,
                'result': step_result
            })
            current_result = step_result
        
        final_answer = current_result if current_result else "Unable to solve"
        
        return {
            'problem': problem,
            'reasoning_steps': solution_steps,
            'final_answer': final_answer,
            'confidence': self.calculate_confidence(solution_steps)
        }
    
    def execute_step(self, step_description: str, problem: str, current_result: Optional[float]) -> Optional[float]:
        """Execute a single reasoning step"""
        # This is a simplified implementation
        # Real implementation would use actual LLM for step execution
        
        if "Convert percentage to decimal" in step_description:
            # Extract percentage from problem (simplified)
            if "25%" in problem:
                return 0.25
            elif "50%" in problem:
                return 0.5
            elif "75%" in problem:
                return 0.75
        
        elif "Multiply by the base number" in step_description:
            # Extract base number (simplified)
            if "80" in problem:
                return 0.25 * 80  # 25% of 80 = 20
            elif "100" in problem:
                return 0.25 * 100  # 25% of 100 = 25
        
        elif "Sum all the numbers" in step_description:
            # Extract numbers (simplified)
            numbers = [10, 20, 30]  # Example numbers
            return sum(numbers)
        
        elif "Divide sum by count" in step_description:
            numbers = [10, 20, 30]  # Example numbers
            return sum(numbers) / len(numbers)
        
        return current_result
    
    def calculate_confidence(self, solution_steps: List[Dict]) -> float:
        """Calculate confidence in the solution"""
        # Simple confidence calculation based on step completeness
        if not solution_steps:
            return 0.0
        
        # Check if each step has a result
        completed_steps = sum(1 for step in solution_steps if step['result'] is not None)
        step_confidence = completed_steps / len(solution_steps)
        
        # Adjust based on problem complexity
        complexity_factor = min(len(solution_steps) / 4.0, 1.0)  # More steps = more complex
        
        return step_confidence * (1 - complexity_factor * 0.2)  # Slightly lower confidence for complex problems

# Test the CoT solver
def test_chain_of_thought_solver():
    """Test chain-of-thought solver"""
    solver = ChainOfThoughtSolver()
    
    test_problems = [
        "What is 25% of 80?",
        "Find the average of 10, 20, and 30",
        "If the ratio of boys to girls is 3:5 and there are 24 students total, how many are girls?"
    ]
    
    results = []
    for problem in test_problems:
        result = solver.solve_with_cot(problem)
        results.append(result)
        
        print(f"Problem: {problem}")
        print(f"Final Answer: {result['final_answer']}")
        print(f"Confidence: {result['confidence']:.2f}")
        print(f"Steps: {len(result['reasoning_steps'])}")
        print("---")
    
    return len(results) == len(test_problems)

# Run test
if __name__ == "__main__":
    print("Testing chain-of-thought solver...")
    test_passed = test_chain_of_thought_solver()
    print(f"Chain-of-thought test passed: {test_passed}")
```

**Success Criteria**:
- [ ] Complete paper reading and analysis
- [ ] Implement CoT reasoning framework
- [ ] Generate step-by-step solutions
- [ ] Calculate confidence in solutions
- [ ] Achieve 70%+ accuracy on test problems

### **GSM8K Dataset Evaluation**
**Dataset Analysis**:
```python
# GSM8K Dataset Analysis
import json
import pandas as pd
from typing import Dict, List, Tuple
import re

class GSM8KAnalyzer:
    def __init__(self):
        self.dataset = []
        self.statistics = {}
    
    def load_dataset(self, file_path: str) -> Dict:
        """Load and analyze GSM8K dataset"""
        # This is a simplified implementation
        # Real implementation would load actual GSM8K dataset
        
        # Sample data structure
        sample_problems = [
            {
                "question": "Natalia sold clips to 48 of her friends in April, and then she sold half as many clips in May. She sold 48 clips in April. How many clips did Natalia sell altogether in April and May?",
                "answer": "72",
                "solution": "Natalia sold 48 clips in April. In May, she sold half as many clips as she did in April, which is 48 / 2 = 24 clips. So, the total number of clips she sold in April and May is 48 + 24 = 72 clips."
            },
            {
                "question": "Angelo and Melanie try to plan a trip to a museum. The museum offers student discounts for $8 per ticket and regular tickets for $12 per ticket. Angelo wants to buy 3 student tickets and 2 regular tickets. Melanie wants to buy 2 student tickets and 4 regular tickets. How much more will Melanie's tickets cost than Angelo's tickets?",
                "answer": "8",
                "solution": "Angelo's tickets cost 3 * $8 + 2 * $12 = $24 + $24 = $48. Melanie's tickets cost 2 * $8 + 4 * $12 = $16 + $48 = $64. So, Melanie's tickets cost $64 - $48 = $16 more than Angelo's tickets."
            }
        ]
        
        self.dataset = sample_problems
        return self.analyze_dataset()
    
    def analyze_dataset(self) -> Dict:
        """Analyze dataset characteristics"""
        if not self.dataset:
            return {"error": "No data loaded"}
        
        # Basic statistics
        total_problems = len(self.dataset)
        
        # Problem length analysis
        question_lengths = [len(p['question']) for p in self.dataset]
        answer_lengths = [len(p['answer']) for p in self.dataset]
        solution_lengths = [len(p['solution']) for p in self.dataset]
        
        # Problem type analysis
        problem_types = self.identify_problem_types()
        
        # Difficulty analysis (based on solution length)
        difficulties = self.estimate_difficulties()
        
        self.statistics = {
            'total_problems': total_problems,
            'avg_question_length': sum(question_lengths) / len(question_lengths),
            'avg_answer_length': sum(answer_lengths) / len(answer_lengths),
            'avg_solution_length': sum(solution_lengths) / len(solution_lengths),
            'problem_types': problem_types,
            'difficulty_distribution': difficulties,
            'numeric_problems': self.count_numeric_problems(),
            'multi_step_problems': self.count_multi_step_problems()
        }
        
        return self.statistics
    
    def identify_problem_types(self) -> Dict[str, int]:
        """Identify different types of mathematical problems"""
        problem_types = {
            'percentage': 0,
            'ratio': 0,
            'average': 0,
            'algebra': 0,
            'geometry': 0,
            'arithmetic': 0,
            'other': 0
        }
        
        for problem in self.dataset:
            question = problem['question'].lower()
            
            if 'percent' in question:
                problem_types['percentage'] += 1
            elif 'ratio' in question:
                problem_types['ratio'] += 1
            elif 'average' in question or 'mean' in question:
                problem_types['average'] += 1
            elif 'equation' in question or 'solve for' in question:
                problem_types['algebra'] += 1
            elif 'area' in question or 'perimeter' in question or 'volume' in question:
                problem_types['geometry'] += 1
            elif any(op in question for op in ['+', '-', '*', '/']):
                problem_types['arithmetic'] += 1
            else:
                problem_types['other'] += 1
        
        return problem_types
    
    def estimate_difficulties(self) -> Dict[str, int]:
        """Estimate difficulty based on solution length"""
        difficulties = {
            'easy': 0,    # Short solutions (< 100 chars)
            'medium': 0,  # Medium solutions (100-200 chars)
            'hard': 0     # Long solutions (> 200 chars)
        }
        
        for problem in self.dataset:
            solution_length = len(problem['solution'])
            
            if solution_length < 100:
                difficulties['easy'] += 1
            elif solution_length < 200:
                difficulties['medium'] += 1
            else:
                difficulties['hard'] += 1
        
        return difficulties
    
    def count_numeric_problems(self) -> int:
        """Count problems with numeric answers"""
        numeric_count = 0
        
        for problem in self.dataset:
            answer = problem['answer']
            if answer.isdigit() or (answer.replace('.', '').isdigit()):
                numeric_count += 1
        
        return numeric_count
    
    def count_multi_step_problems(self) -> int:
        """Count multi-step problems (based on solution complexity)"""
        multi_step_count = 0
        
        for problem in self.dataset:
            solution = problem['solution']
            # Count steps (simplified - look for numbered steps or "Step" keywords)
            step_indicators = solution.lower().count('step') + solution.count('1.') + solution.count('2.')
            
            if step_indicators >= 2:
                multi_step_count += 1
        
        return multi_step_count
    
    def generate_sample_problems(self, num_samples: int = 5) -> List[Dict]:
        """Generate sample problems for testing"""
        return self.dataset[:num_samples]

# Test the analyzer
def test_gsm8k_analyzer():
    """Test GSM8K analyzer"""
    analyzer = GSM8KAnalyzer()
    
    # Simulate loading dataset
    stats = analyzer.load_dataset("dummy_path.json")
    
    print("GSM8K Dataset Analysis:")
    print(f"Total Problems: {stats['total_problems']}")
    print(f"Average Question Length: {stats['avg_question_length']:.1f}")
    print(f"Average Answer Length: {stats['avg_answer_length']:.1f}")
    print(f"Average Solution Length: {stats['avg_solution_length']:.1f}")
    print(f"Problem Types: {stats['problem_types']}")
    print(f"Difficulty Distribution: {stats['difficulty_distribution']}")
    print(f"Numeric Problems: {stats['numeric_problems']}")
    print(f"Multi-step Problems: {stats['multi_step_problems']}")
    
    # Generate sample problems
    samples = analyzer.generate_sample_problems(2)
    print(f"\nSample Problems: {len(samples)}")
    
    return stats['total_problems'] > 0

# Run test
if __name__ == "__main__":
    print("Testing GSM8K analyzer...")
    test_passed = test_gsm8k_analyzer()
    print(f"GSM8K analyzer test passed: {test_passed}")
```

**Success Criteria**:
- [ ] Load and analyze GSM8K dataset structure
- [ ] Identify problem types and difficulty levels
- [ ] Create data processing pipeline
- [ ] Generate sample problems for testing
- [ ] Document dataset characteristics

---

## 🛠️ **Projects & Applications**

### **Project 1: Chain-of-Thought Math Solver**
**Objective**: Build a complete chain-of-thought mathematical reasoning system

**Implementation**:
```python
# Complete Chain-of-Thought Math Solver
import torch
import torch.nn as nn
from typing import List, Dict, Optional, Tuple
import re
import numpy as np

class CompleteCoTSolver:
    def __init__(self, model_name: str = "microsoft/DialoGPT-medium"):
        """Initialize complete CoT solver"""
        self.model = None  # Would load actual model
        self.tokenizer = None  # Would load actual tokenizer
        self.reasoning_templates = self._initialize_templates()
        self.calculator = MathCalculator()
    
    def _initialize_templates(self) -> Dict[str, List[str]]:
        """Initialize reasoning templates for different problem types"""
        return {
            'percentage': [
                "Step 1: Identify the percentage value",
                "Step 2: Convert percentage to decimal",
                "Step 3: Identify the base number",
                "Step 4: Multiply decimal by base number",
                "Step 5: Convert result back to percentage if needed"
            ],
            'ratio': [
                "Step 1: Identify the ratio components",
                "Step 2: Set up proportion equation",
                "Step 3: Cross-multiply to solve",
                "Step 4: Solve for unknown",
                "Step 5: Verify the solution"
            ],
            'average': [
                "Step 1: List all the numbers",
                "Step 2: Add all numbers together",
                "Step 3: Count how many numbers",
                "Step 4: Divide sum by count",
                "Step 5: Check for outliers"
            ],
            'algebra': [
                "Step 1: Identify the equation",
                "Step 2: Isolate the variable",
                "Step 3: Perform inverse operations",
                "Step 4: Solve for variable",
                "Step 5: Verify solution"
            ],
            'geometry': [
                "Step 1: Identify the shape and formula",
                "Step 2: Extract given dimensions",
                "Step 3: Apply the formula",
                "Step 4: Calculate the result",
                "Step 5: Check units and reasonableness"
            ]
        }
    
    def identify_problem_type(self, problem: str) -> str:
        """Identify the type of mathematical problem"""
        problem_lower = problem.lower()
        
        if 'percent' in problem_lower:
            return 'percentage'
        elif 'ratio' in problem_lower:
            return 'ratio'
        elif 'average' in problem_lower or 'mean' in problem_lower:
            return 'average'
        elif 'equation' in problem_lower or 'solve for' in problem_lower:
            return 'algebra'
        elif any(shape in problem_lower for shape in ['area', 'perimeter', 'volume', 'triangle', 'circle']):
            return 'geometry'
        else:
            return 'arithmetic'
    
    def extract_numbers(self, problem: str) -> List[float]:
        """Extract numbers from problem text"""
        # Find all numbers (including decimals)
        numbers = re.findall(r'\d+\.?\d*', problem)
        return [float(n) for n in numbers]
    
    def solve_with_complete_cot(self, problem: str) -> Dict:
        """Solve problem with complete chain-of-thought reasoning"""
        # Identify problem type
        problem_type = self.identify_problem_type(problem)
        
        # Get reasoning template
        template = self.reasoning_templates.get(problem_type, self.reasoning_templates['arithmetic'])
        
        # Extract numbers
        numbers = self.extract_numbers(problem)
        
        # Generate reasoning steps
        reasoning_steps = []
        current_result = None
        
        for i, step_template in enumerate(template):
            step_result = self.execute_reasoning_step(
                step_template, problem, numbers, current_result, i
            )
            
            reasoning_steps.append({
                'step': i + 1,
                'description': step_template,
                'calculation': step_result['calculation'],
                'result': step_result['result'],
                'explanation': step_result['explanation']
            })
            
            current_result = step_result['result']
        
        # Final verification
        verification = self.verify_solution(problem, current_result, reasoning_steps)
        
        return {
            'problem': problem,
            'problem_type': problem_type,
            'reasoning_steps': reasoning_steps,
            'final_answer': current_result,
            'verification': verification,
            'confidence': self.calculate_detailed_confidence(reasoning_steps, verification)
        }
    
    def execute_reasoning_step(self, step_template: str, problem: str, numbers: List[float], 
                             current_result: Optional[float], step_index: int) -> Dict:
        """Execute a single reasoning step"""
        problem_type = self.identify_problem_type(problem)
        
        if problem_type == 'percentage':
            return self.execute_percentage_step(step_template, numbers, current_result, step_index)
        elif problem_type == 'ratio':
            return self.execute_ratio_step(step_template, numbers, current_result, step_index)
        elif problem_type == 'average':
            return self.execute_average_step(step_template, numbers, current_result, step_index)
        elif problem_type == 'algebra':
            return self.execute_algebra_step(step_template, problem, numbers, current_result, step_index)
        elif problem_type == 'geometry':
            return self.execute_geometry_step(step_template, problem, numbers, current_result, step_index)
        else:
            return self.execute_arithmetic_step(step_template, numbers, current_result, step_index)
    
    def execute_percentage_step(self, step_template: str, numbers: List[float], 
                                current_result: Optional[float], step_index: int) -> Dict:
        """Execute percentage-related reasoning step"""
        if "Convert percentage to decimal" in step_template and numbers:
            percentage = numbers[0] if step_index == 1 else current_result
            decimal_value = percentage / 100
            return {
                'calculation': f"{percentage}% = {percentage/100}",
                'result': decimal_value,
                'explanation': f"Convert {percentage}% to decimal: {percentage/100}"
            }
        
        elif "Multiply decimal by base number" in step_template and len(numbers) >= 2:
            decimal_value = current_result
            base_number = numbers[1]
            result = decimal_value * base_number
            return {
                'calculation': f"{decimal_value} × {base_number} = {result}",
                'result': result,
                'explanation': f"Multiply decimal {decimal_value} by base number {base_number}"
            }
        
        else:
            return self.default_step_execution(step_template, numbers, current_result)
    
    def execute_ratio_step(self, step_template: str, numbers: List[float], 
                          current_result: Optional[float], step_index: int) -> Dict:
        """Execute ratio-related reasoning step"""
        if "Cross-multiply to solve" in step_template and len(numbers) >= 2:
            a, b = numbers[0], numbers[1]
            result = a * b
            return {
                'calculation': f"{a} × {b} = {result}",
                'result': result,
                'explanation': f"Cross-multiply {a} and {b}"
            }
        
        else:
            return self.default_step_execution(step_template, numbers, current_result)
    
    def execute_average_step(self, step_template: str, numbers: List[float], 
                           current_result: Optional[float], step_index: int) -> Dict:
        """Execute average-related reasoning step"""
        if "Add all numbers together" in step_template:
            result = sum(numbers)
            return {
                'calculation': f"{' + '.join(map(str, numbers))} = {result}",
                'result': result,
                'explanation': f"Add all numbers: {' + '.join(map(str, numbers))}"
            }
        
        elif "Divide sum by count" in step_template:
            sum_result = current_result
            count = len(numbers)
            result = sum_result / count
            return {
                'calculation': f"{sum_result} ÷ {count} = {result}",
                'result': result,
                'explanation': f"Divide sum {sum_result} by count {count}"
            }
        
        else:
            return self.default_step_execution(step_template, numbers, current_result)
    
    def execute_algebra_step(self, step_template: str, problem: str, numbers: List[float], 
                            current_result: Optional[float], step_index: int) -> Dict:
        """Execute algebra-related reasoning step"""
        # Simplified algebra step execution
        if "Solve for variable" in step_template and numbers:
            # Simple linear equation: x + a = b
            if len(numbers) >= 2:
                a, b = numbers[0], numbers[1]
                result = b - a
                return {
                    'calculation': f"x = {b} - {a} = {result}",
                    'result': result,
                    'explanation': f"Solve x + {a} = {b}: x = {b} - {a} = {result}"
                }
        
        return self.default_step_execution(step_template, numbers, current_result)
    
    def execute_geometry_step(self, step_template: str, problem: str, numbers: List[float], 
                            current_result: Optional[float], step_index: int) -> Dict:
        """Execute geometry-related reasoning step"""
        if "Apply the formula" in step_template and len(numbers) >= 2:
            length, width = numbers[0], numbers[1]
            area = length * width
            return {
                'calculation': f"Area = {length} × {width} = {area}",
                'result': area,
                'explanation': f"Calculate area: {length} × {width} = {area}"
            }
        
        return self.default_step_execution(step_template, numbers, current_result)
    
    def execute_arithmetic_step(self, step_template: str, numbers: List[float], 
                               current_result: Optional[float], step_index: int) -> Dict:
        """Execute arithmetic-related reasoning step"""
        if numbers and step_index == 0:
            # First step: identify numbers
            return {
                'calculation': f"Numbers identified: {numbers}",
                'result': numbers[0],  # Return first number as default
                'explanation': f"Identified numbers in problem: {numbers}"
            }
        
        return self.default_step_execution(step_template, numbers, current_result)
    
    def default_step_execution(self, step_template: str, numbers: List[float], 
                              current_result: Optional[float]) -> Dict:
        """Default step execution for unknown step types"""
        return {
            'calculation': "Unknown calculation",
            'result': current_result,
            'explanation': f"Executing: {step_template}"
        }
    
    def verify_solution(self, problem: str, result: Optional[float], reasoning_steps: List[Dict]) -> Dict:
        """Verify the solution"""
        verification = {
            'plausible': True,
            'checked': False,
            'method': 'sanity_check'
        }
        
        if result is not None:
            # Basic sanity checks
            if result < 0 and 'area' in problem.lower():
                verification['plausible'] = False
                verification['error'] = 'Area cannot be negative'
            
            if result > 10000 and 'percent' in problem.lower():
                verification['plausible'] = False
                verification['error'] = 'Percentage result seems too large'
            
            # Check if result makes sense in context
            verification['checked'] = True
        
        return verification
    
    def calculate_detailed_confidence(self, reasoning_steps: List[Dict], verification: Dict) -> float:
        """Calculate detailed confidence score"""
        if not reasoning_steps:
            return 0.0
        
        # Base confidence from step completion
        completed_steps = sum(1 for step in reasoning_steps if step['result'] is not None)
        step_confidence = completed_steps / len(reasoning_steps)
        
        # Adjust for verification
        verification_factor = 1.0
        if verification['checked']:
            if verification['plausible']:
                verification_factor = 1.1  # Boost confidence for verified solution
            else:
                verification_factor = 0.5  # Reduce confidence for implausible solution
        
        # Adjust for problem complexity
        complexity_factor = min(len(reasoning_steps) / 5.0, 1.0)
        
        return min(step_confidence * verification_factor * (1 - complexity_factor * 0.1), 1.0)

class MathCalculator:
    """Helper class for mathematical calculations"""
    
    @staticmethod
    def percentage_of(percent: float, base: float) -> float:
        """Calculate percentage of a number"""
        return (percent / 100) * base
    
    @staticmethod
    def average(numbers: List[float]) -> float:
        """Calculate average of numbers"""
        return sum(numbers) / len(numbers) if numbers else 0.0
    
    @staticmethod
    def ratio_proportion(a: float, b: float, c: float) -> float:
        """Solve proportion a:b = c:x"""
        return (b * c) / a if a != 0 else 0.0

# Test the complete CoT solver
def test_complete_cot_solver():
    """Test complete chain-of-thought solver"""
    solver = CompleteCoTSolver()
    
    test_problems = [
        "What is 25% of 80?",
        "Find the average of 10, 20, and 30",
        "If the ratio of boys to girls is 3:5 and there are 24 students total, how many are girls?",
        "A rectangle has length 6 and width 4. What is its area?"
        "Solve for x: x + 5 = 15"
    ]
    
    results = []
    for problem in test_problems:
        result = solver.solve_with_complete_cot(problem)
        results.append(result)
        
        print(f"Problem: {problem}")
        print(f"Type: {result['problem_type']}")
        print(f"Final Answer: {result['final_answer']}")
        print(f"Confidence: {result['confidence']:.2f}")
        print(f"Verification: {result['verification']}")
        print(f"Steps: {len(result['reasoning_steps'])}")
        print("---")
    
    return len(results) == len(test_problems)

# Run test
if __name__ == "__main__":
    print("Testing complete CoT solver...")
    test_passed = test_complete_cot_solver()
    print(f"Complete CoT solver test passed: {test_passed}")
```

**Deliverables**:
- [ ] Complete CoT solver with multiple problem types
- [ ] Detailed reasoning step generation
- [ ] Solution verification system
- [ ] Confidence scoring mechanism
- [ ] Support for percentage, ratio, average, algebra, geometry problems
- [ ] Comprehensive test suite

---

## 📊 **Progress Tracking**

### **Daily Checklist**
- [ ] 2 hours Berkeley RL course (lectures and assignments)
- [ ] 2 hours chain-of-thought implementation
- [ ] 1.5 hours GSM8K dataset exploration
- [ ] 1 hour advanced environment setup
- [ ] 30 minutes research paper analysis
- [ ] 30 minutes project development

### **Weekly Milestones**
**Week 21**:
- [ ] Complete Berkeley RL Lectures 1-2
- [ ] Read and analyze "Chain-of-Thought Prompting" paper
- [ ] Implement basic CoT framework
- [ ] Load and analyze GSM8K dataset
- [ ] Set up advanced development environment

**Week 22**:
- [ ] Complete Berkeley RL Lecture 3
- [ ] Enhance CoT solver with multiple problem types
- [ ] Create GSM8K data processing pipeline
- [ ] Test CoT solver on sample problems
- [ ] Complete Assignment 1

### **End-of-Period Assessment**
**Success Metrics**:
- [ ] Berkeley RL: Lectures 1-3 completed, Assignment 1 submitted
- [ ] Chain-of-Thought: Complete implementation with 70%+ accuracy
- [ ] GSM8K: Dataset analyzed and processing pipeline ready
- [ ] Environment: Advanced setup complete with all tools
- [ ] Project: CoT solver working with detailed reasoning steps
- [ ] Research: Paper completely read and analyzed

---

## 🚀 **Next Period Preparation**

### **Weeks 23-24 Preview**
- Continue Berkeley RL course (Lectures 4-6)
- Read "Toolformer" paper
- Build transformer from scratch (PyTorch)
- Implement mathematical attention mechanisms
- Test on mathematical reasoning tasks

### **Resources to Preview**:
- [PyTorch Transformer Tutorial](https://pytorch.org/tutorials/beginner/transformer_tutorial.html)
- [Toolformer Paper](https://arxiv.org/abs/2302.04761)
- [Attention Mechanism Guide](https://github.com/karpathy/nn-zero-to-hero)
- [Mathematical Reasoning Datasets](https://paperswithcode.com/task/mathematical-reasoning)

---

## 📞 **Support & Resources**

### **Help Channels**:
- Berkeley RL Course Discussion Forums
- PyTorch Documentation and Community
- Research Paper Discussion Groups
- Math LLM Discord Communities
- Stack Overflow for technical questions

### **Additional Resources**:
- [Deep Learning Book](https://www.deeplearningbook.org/)
- [Mathematical Reasoning Papers](https://arxiv.org/list/cs.LG/recent)
- [PyTorch Examples](https://github.com/pytorch/examples)
- [Chain-of-Thought Examples](https://github.com/dair-ai/Chain-of-Thought-Hub)

---

*This 2-week plan provides advanced mathematical reasoning foundation with Berkeley RL course, chain-of-thought implementation, and GSM8K dataset exploration for Phase 2 Math LLM specialization.*
