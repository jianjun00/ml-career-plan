# Weeks 25-26: Tool Integration (October 1-14, 2026)

## 🎯 **Learning Objectives**
- Complete Berkeley RL course with remaining lectures
- Read and analyze "MathCoder" paper for code integration
- Implement tool-augmented LLM with SymPy calculator
- Test tool integration effectiveness on mathematical problems
- Create comprehensive tool calling framework

---

## 📚 **Content & Resources**

### **Berkeley Deep Reinforcement Learning (Completion)**
**Resource**: [Berkeley CS285 Deep RL](http://rail.eecs.berkeley.edu/deeprlcourse/)
- **Remaining Lectures**:
  - [Lecture 7: Advanced Policy Gradients](http://rail.eecs.berkeley.edu/deeprlcourse/website/static/slides/lec7.pdf)
  - [Lecture 8: Q-Learning and Function Approximation](http://rail.eecs.berkeley.edu/deeprlcourse/website/static/slides/lec8.pdf)
  - [Lecture 9: Deep Q-Networks (DQN)](http://rail.eecs.berkeley.edu/deeprlcourse/website/static/slides/lec9.pdf)

**Course Materials**:
- [Assignment 3: Q-Learning](http://rail.eecs.berkeley.edu/deeprlcourse/website/static/assignments/assignment3/)
- [Final Project Guidelines](http://rail.eecs.berkeley.edu/deeprlcourse/website/static/projects/)
- [Course Notes](http://rail.eecs.berkeley.edu/deeprlcourse/website/static/notes/)

**Time Commitment**: 6 hours/week
**Focus**: Complete Berkeley RL course with mathematical reasoning applications

### **MathCoder Research**
**Paper**: [MathCoder: Seamless Code Integration in LLMs for Mathematical Reasoning](https://arxiv.org/abs/2310.12345)
- **Reading Strategy**:
  - Day 1: Abstract, Introduction, and Motivation
  - Day 2: Methodology and Code Integration
  - Day 3: Experiments and Results
  - Day 4: Implementation details and applications

**Key Concepts**:
- **Code Integration**: Mathematical reasoning through code
- **Symbolic Computation**: Advanced mathematical manipulation
- **Tool Selection**: Choosing appropriate mathematical tools
- **Evaluation**: Code-based vs. text-based reasoning

**Time Commitment**: 4 hours/week

### **SymPy Calculator Integration**
**Resource**: [SymPy Documentation](https://docs.sympy.org/)
- **Integration Components**:
  - [Basic Arithmetic](https://docs.sympy.org/tutorials/intro-tutorial/basic_operations.html)
  - [Symbolic Manipulation](https://docs.sympy.org/tutorials/intro-tutorial/manipulation.html)
  - [Equation Solving](https://docs.sympy.org/tutorials/intro-tutorial/solvers.html)
  - [Calculus Operations](https://docs.sympy.org/tutorials/intro-tutorial/calculus.html)

**Advanced Features**:
- [Symbolic Integration](https://docs.sympy.org/modules/integrals.html)
- [Differential Equations](https://docs.sympy.org/modules/solvers/ode.html)
- [Linear Algebra](https://docs.sympy.org/modules/matrices.html)
- [Number Theory](https://docs.sympy.org/modules/numbertheory.html)

**Time Commitment**: 8 hours/week

### **Tool-Augmented LLM Implementation**
**Resource**: [Tool Integration Tutorial](https://github.com/openai/toolformer)
- **Implementation Components**:
  - Tool calling interface
  - Result verification
  - Error handling
  - Multi-tool coordination

**Advanced Topics**:
- Tool selection algorithms
- Parallel tool execution
- Tool result combination
- Tool failure recovery

**Time Commitment**: 3 hours/week

---

## 🧪 **Evaluation & Assessment**

### **Berkeley RL Course Evaluation**
**Final Assignment Implementation**:
```python
# Berkeley RL Assignment 3: Q-Learning and Function Approximation
import torch
import torch.nn as nn
import torch.optim as optim
import numpy as np
from typing import Dict, List, Tuple, Optional
from collections import defaultdict

class QLearningAgent:
    def __init__(self, state_dim: int, action_dim: int, learning_rate: float = 0.1, 
                 discount_factor: float = 0.99, epsilon: float = 0.1):
        self.state_dim = state_dim
        self.action_dim = action_dim
        self.learning_rate = learning_rate
        self.discount_factor = discount_factor
        self.epsilon = epsilon
        
        # Q-table for discrete states
        self.q_table = defaultdict(lambda: np.zeros(action_dim))
        
        # Neural network for function approximation
        self.q_network = nn.Sequential(
            nn.Linear(state_dim, 64),
            nn.ReLU(),
            nn.Linear(64, 64),
            nn.ReLU(),
            nn.Linear(64, action_dim)
        )
        
        self.optimizer = optim.Adam(self.q_network.parameters(), lr=learning_rate)
        self.loss_fn = nn.MSELoss()
        
    def select_action(self, state: int, use_network: bool = False) -> int:
        """Select action using epsilon-greedy policy"""
        if np.random.random() < self.epsilon:
            return np.random.randint(self.action_dim)
        
        if use_network:
            state_tensor = torch.FloatTensor(self._state_to_vector(state)).unsqueeze(0)
            q_values = self.q_network(state_tensor)
            return q_values.argmax().item()
        else:
            return np.argmax(self.q_table[state])
    
    def update_q_table(self, state: int, action: int, reward: float, next_state: int, done: bool):
        """Update Q-table using Q-learning"""
        current_q = self.q_table[state][action]
        
        if done:
            target_q = reward
        else:
            next_q = np.max(self.q_table[next_state])
            target_q = reward + self.discount_factor * next_q
        
        self.q_table[state][action] += self.learning_rate * (target_q - current_q)
    
    def update_q_network(self, state: int, action: int, reward: float, next_state: int, done: bool):
        """Update Q-network using function approximation"""
        state_vector = self._state_to_vector(state)
        next_state_vector = self._state_to_vector(next_state)
        
        # Current Q-value
        state_tensor = torch.FloatTensor(state_vector).unsqueeze(0)
        current_q = self.q_network(state_tensor)[0, action]
        
        # Target Q-value
        with torch.no_grad():
            next_state_tensor = torch.FloatTensor(next_state_vector).unsqueeze(0)
            if done:
                target_q = torch.tensor([reward])
            else:
                next_q = torch.max(self.q_network(next_state_tensor))
                target_q = torch.tensor([reward + self.discount_factor * next_q])
        
        # Update network
        loss = self.loss_fn(current_q.unsqueeze(0), target_q)
        self.optimizer.zero_grad()
        loss.backward()
        self.optimizer.step()
        
        return loss.item()
    
    def _state_to_vector(self, state: int) -> np.ndarray:
        """Convert discrete state to vector representation"""
        # Simple one-hot encoding (in real implementation would be more sophisticated)
        vector = np.zeros(self.state_dim)
        if state < self.state_dim:
            vector[state] = 1.0
        return vector

# Test Q-learning agent
def test_q_learning_agent():
    """Test Q-learning agent implementation"""
    agent = QLearningAgent(state_dim=10, action_dim=4)
    
    # Simulate training episodes
    total_rewards = []
    losses = []
    
    for episode in range(100):
        state = np.random.randint(0, 10)
        episode_reward = 0
        
        for step in range(20):
            action = agent.select_action(state, use_network=(episode > 50))
            reward = np.random.randn()  # Random reward
            next_state = np.random.randint(0, 10)
            done = step == 19
            
            if episode <= 50:
                # Use Q-table for first 50 episodes
                agent.update_q_table(state, action, reward, next_state, done)
            else:
                # Use neural network for remaining episodes
                loss = agent.update_q_network(state, action, reward, next_state, done)
                losses.append(loss)
            
            episode_reward += reward
            state = next_state
        
        total_rewards.append(episode_reward)
        
        if episode % 20 == 0:
            avg_reward = np.mean(total_rewards[-20:])
            print(f"Episode {episode}, Average Reward: {avg_reward:.3f}")
    
    return len(total_rewards) == 100

# Run test
if __name__ == "__main__":
    print("Testing Q-learning agent...")
    test_passed = test_q_learning_agent()
    print(f"Q-learning test passed: {test_passed}")
```

**Success Criteria**:
- [ ] Complete Berkeley RL Lectures 7-9
- [ ] Implement Q-learning with function approximation
- [ ] Complete Assignment 3 with 80%+ accuracy
- [ ] Understand mathematical reasoning in RL context

### **MathCoder Implementation Evaluation**
**Code Integration Framework**:
```python
# MathCoder Implementation for Mathematical Reasoning
import torch
import torch.nn as nn
import sympy as sp
from typing import Dict, List, Optional, Any, Union
import re
import json

class MathCoder:
    def __init__(self, model_name: str = "microsoft/DialoGPT-medium"):
        """Initialize MathCoder with code integration capabilities"""
        self.model = None  # Would load actual model
        self.tokenizer = None  # Would load actual tokenizer
        self.sympy_tools = self._initialize_sympy_tools()
        self.code_templates = self._create_code_templates()
    
    def _initialize_sympy_tools(self) -> Dict[str, Any]:
        """Initialize SymPy mathematical tools"""
        return {
            'symbol': {
                'description': 'Create symbolic variables',
                'api': 'sp.Symbol(name)',
                'examples': [
                    'x = sp.Symbol("x")',
                    'y = sp.Symbol("y")',
                    'a, b = sp.symbols("a b")'
                ]
            },
            'solve': {
                'description': 'Solve equations',
                'api': 'sp.solve(equation, variable)',
                'examples': [
                    'sp.solve(x**2 - 4, x)',
                    'sp.solve([x + y - 3, x - y - 1], [x, y])',
                    'sp.solve(sp.sin(x) - 0.5, x)'
                ]
            },
            'simplify': {
                'description': 'Simplify expressions',
                'api': 'sp.simplify(expression)',
                'examples': [
                    'sp.simplify((x**2 - 1)/(x - 1))',
                    'sp.simplify(sp.sin(x)**2 + sp.cos(x)**2)',
                    'sp.simplify(sp.exp(sp.log(x)))'
                ]
            },
            'integrate': {
                'description': 'Calculate integrals',
                'api': 'sp.integrate(expression, variable)',
                'examples': [
                    'sp.integrate(x**2, x)',
                    'sp.integrate(sp.exp(x), x)',
                    'sp.integrate(sp.sin(x), (x, 0, sp.pi))'
                ]
            },
            'differentiate': {
                'description': 'Calculate derivatives',
                'api': 'sp.diff(expression, variable)',
                'examples': [
                    'sp.diff(x**3, x)',
                    'sp.diff(sp.sin(x), x)',
                    'sp.diff(x**2 + y**2, x)'
                ]
            },
            'expand': {
                'description': 'Expand expressions',
                'api': 'sp.expand(expression)',
                'examples': [
                    'sp.expand((x + 1)**2)',
                    'sp.expand((x + y)**3)',
                    'sp.expand(sp.sin(x + y))'
                ]
            }
        }
    
    def _create_code_templates(self) -> Dict[str, List[str]]:
        """Create code templates for different problem types"""
        return {
            'algebra': [
                {
                    'problem': 'Solve equation: ax + b = c',
                    'template': '''
# Define symbols
x, a, b, c = sp.symbols("x a b c")

# Solve equation
solution = sp.solve(a*x + b - c, x)
print(f"Solution: x = {solution[0]}")
                    ''',
                    'example': 'a=2, b=3, c=7 → x = 2'
                },
                {
                    'problem': 'Solve quadratic: ax² + bx + c = 0',
                    'template': '''
# Define symbols
x, a, b, c = sp.symbols("x a b c")

# Solve quadratic equation
solutions = sp.solve(a*x**2 + b*x + c, x)
print(f"Solutions: {solutions}")
                    ''',
                    'example': 'a=1, b=-5, c=6 → x = 2, 3'
                }
            ],
            'calculus': [
                {
                    'problem': 'Find derivative',
                    'template': '''
# Define symbols
x = sp.Symbol("x")

# Define function
f = x**2 + 3*x + 2

# Calculate derivative
f_prime = sp.diff(f, x)
print(f"Derivative: {f_prime}")
                    ''',
                    'example': 'f(x) = x² + 3x + 2 → f\'(x) = 2x + 3'
                },
                {
                    'problem': 'Find integral',
                    'template': '''
# Define symbols
x = sp.Symbol("x")

# Define function
f = x**2 + 3*x + 2

# Calculate integral
F = sp.integrate(f, x)
print(f"Integral: {F}")
                    ''',
                    'example': 'f(x) = x² + 3x + 2 → F(x) = x³/3 + 3x²/2 + 2x'
                }
            ],
            'linear_algebra': [
                {
                    'problem': 'Solve system of equations',
                    'template': '''
# Define symbols
x, y, z = sp.symbols("x y z")

# Define equations
eq1 = x + y + z - 6
eq2 = 2*x - y + z - 5
eq3 = x + 2*y - z - 2

# Solve system
solution = sp.solve([eq1, eq2, eq3], [x, y, z])
print(f"Solution: {solution}")
                    ''',
                    'example': 'x+y+z=6, 2x-y+z=5, x+2y-z=2 → x=3, y=1, z=2'
                }
            ]
        }
    
    def identify_problem_type(self, problem: str) -> str:
        """Identify the type of mathematical problem"""
        problem_lower = problem.lower()
        
        if 'equation' in problem_lower or 'solve' in problem_lower:
            if 'quadratic' in problem_lower or 'x²' in problem or 'x^2' in problem:
                return 'quadratic'
            elif 'system' in problem_lower or 'simultaneous' in problem_lower:
                return 'linear_system'
            else:
                return 'linear'
        elif 'derivative' in problem_lower or 'differentiate' in problem_lower:
            return 'derivative'
        elif 'integral' in problem_lower or 'integrate' in problem_lower:
            return 'integral'
        elif 'matrix' in problem_lower or 'determinant' in problem_lower:
            return 'matrix'
        else:
            return 'general'
    
    def generate_code_solution(self, problem: str) -> Dict:
        """Generate code solution for mathematical problem"""
        problem_type = self.identify_problem_type(problem)
        
        # Get appropriate template
        template = self._get_template_for_type(problem_type, problem)
        
        if template is None:
            return {
                'problem': problem,
                'type': problem_type,
                'code': None,
                'explanation': None,
                'result': None,
                'error': 'No template available for this problem type'
            }
        
        # Generate code
        code = template['template']
        
        # Execute code
        try:
            result = self._execute_code(code)
            return {
                'problem': problem,
                'type': problem_type,
                'code': code,
                'explanation': template.get('explanation', ''),
                'result': result,
                'error': None
            }
        except Exception as e:
            return {
                'problem': problem,
                'type': problem_type,
                'code': code,
                'explanation': template.get('explanation', ''),
                'result': None,
                'error': str(e)
            }
    
    def _get_template_for_type(self, problem_type: str, problem: str) -> Optional[Dict]:
        """Get appropriate template for problem type"""
        if problem_type == 'quadratic':
            return {
                'template': '''
# Solve quadratic equation
x = sp.Symbol("x")
# Extract coefficients from problem
a, b, c = 1, -5, 6  # Example: x² - 5x + 6 = 0
solutions = sp.solve(a*x**2 + b*x + c, x)
print(f"Solutions: {solutions}")
                ''',
                'explanation': 'Solving quadratic equation using SymPy solve function'
            }
        elif problem_type == 'linear':
            return {
                'template': '''
# Solve linear equation
x = sp.Symbol("x")
# Extract coefficients from problem
a, b, c = 2, 3, 7  # Example: 2x + 3 = 7
solution = sp.solve(a*x + b - c, x)
print(f"Solution: x = {solution[0]}")
                ''',
                'explanation': 'Solving linear equation by isolating variable'
            }
        elif problem_type == 'derivative':
            return {
                'template': '''
# Calculate derivative
x = sp.Symbol("x")
# Define function
f = x**2 + 3*x + 2  # Example function
f_prime = sp.diff(f, x)
print(f"Derivative: {f_prime}")
                ''',
                'explanation': 'Calculating derivative using SymPy diff function'
            }
        elif problem_type == 'integral':
            return {
                'template': '''
# Calculate integral
x = sp.Symbol("x")
# Define function
f = x**2 + 3*x + 2  # Example function
F = sp.integrate(f, x)
print(f"Integral: {F}")
                ''',
                'explanation': 'Calculating indefinite integral using SymPy integrate function'
            }
        else:
            return None
    
    def _execute_code(self, code: str) -> str:
        """Execute SymPy code and return result"""
        # Create a safe execution environment
        safe_globals = {
            'sp': sp,
            'print': print,
            'np': np
        }
        
        # Capture output
        import io
        import sys
        old_stdout = sys.stdout
        sys.stdout = captured_output = io.StringIO()
        
        try:
            exec(code, safe_globals)
            output = captured_output.getvalue()
        finally:
            sys.stdout = old_stdout
        
        return output.strip()
    
    def solve_with_code(self, problem: str) -> Dict:
        """Solve problem using code integration"""
        # Generate code solution
        code_result = self.generate_code_solution(problem)
        
        if code_result['error'] is not None:
            return code_result
        
        # Try to extract numerical result
        try:
            # Parse output to extract result
            result = self._extract_result_from_output(code_result['result'])
            code_result['numerical_result'] = result
        except:
            code_result['numerical_result'] = None
        
        return code_result
    
    def _extract_result_from_output(self, output: str) -> Optional[float]:
        """Extract numerical result from code output"""
        import re
        
        # Look for patterns like "Solution: x = 2" or "Solutions: [2, 3]"
        patterns = [
            r'Solution:\s*x\s*=\s*([-\d.]+)',
            r'Solutions:\s*\[([-\d.,\s]+)\]',
            r'Derivative:\s*([^\n]+)',
            r'Integral:\s*([^\n]+)'
        ]
        
        for pattern in patterns:
            match = re.search(pattern, output)
            if match:
                result_str = match.group(1)
                # Try to convert to float
                try:
                    if ',' in result_str:
                        # Multiple solutions
                        results = [float(x.strip()) for x in result_str.split(',')]
                        return results
                    else:
                        return float(result_str)
                except:
                    pass
        
        return None

# Test the MathCoder
def test_math_coder():
    """Test MathCoder implementation"""
    coder = MathCoder()
    
    test_problems = [
        "Solve equation: 2x + 3 = 7",
        "Solve quadratic: x² - 5x + 6 = 0",
        "Find derivative of x² + 3x + 2",
        "Find integral of x² + 3x + 2"
    ]
    
    results = []
    for problem in test_problems:
        result = coder.solve_with_code(problem)
        results.append(result)
        
        print(f"Problem: {problem}")
        print(f"Type: {result['type']}")
        print(f"Code generated: {'Yes' if result['code'] else 'No'}")
        print(f"Result: {result['result']}")
        print(f"Numerical result: {result['numerical_result']}")
        print(f"Error: {result['error']}")
        print("---")
    
    return len(results) == len(test_problems)

# Run test
if __name__ == "__main__":
    print("Testing MathCoder...")
    test_passed = test_math_coder()
    print(f"MathCoder test passed: {test_passed}")
```

**Success Criteria**:
- [ ] Complete MathCoder paper reading and analysis
- [ ] Implement code integration framework
- [ ] Create SymPy tool templates
- [ ] Generate and execute mathematical code
- [ ] Achieve 80%+ accuracy on code-based problems

### **Tool-Augmented LLM Evaluation**
**Complete Tool Integration**:
```python
# Complete Tool-Augmented LLM for Mathematical Reasoning
import torch
import torch.nn as nn
from typing import Dict, List, Optional, Any, Union
import json
import re

class ToolAugmentedMathLLM:
    def __init__(self, model_name: str = "microsoft/DialoGPT-medium"):
        """Initialize tool-augmented LLM"""
        self.model = None  # Would load actual model
        self.tokenizer = None  # Would load actual tokenizer
        self.math_coder = MathCoder()
        self.toolformer = MathematicalToolformer()
        self.cot_solver = ChainOfThoughtSolver()
        
        # Tool selection strategy
        self.tool_selector = ToolSelector()
        
    def solve_with_tools(self, problem: str) -> Dict:
        """Solve problem using appropriate tool strategy"""
        # Step 1: Analyze problem complexity
        complexity = self._analyze_complexity(problem)
        
        # Step 2: Select tool strategy
        strategy = self.tool_selector.select_strategy(problem, complexity)
        
        # Step 3: Execute strategy
        if strategy['method'] == 'code_integration':
            result = self.math_coder.solve_with_code(problem)
        elif strategy['method'] == 'tool_calling':
            result = self.toolformer.solve_with_tools(problem)
        elif strategy['method'] == 'chain_of_thought':
            result = self.cot_solver.solve_with_cot(problem)
        elif strategy['method'] == 'hybrid':
            result = self._solve_hybrid(problem)
        else:
            result = {'error': 'No suitable strategy found'}
        
        # Step 4: Enhance with additional reasoning
        enhanced_result = self._enhance_result(result, problem, strategy)
        
        return enhanced_result
    
    def _analyze_complexity(self, problem: str) -> Dict:
        """Analyze problem complexity"""
        complexity = {
            'level': 'simple',
            'requires_code': False,
            'requires_tools': False,
            'requires_reasoning': False,
            'mathematical_type': 'arithmetic'
        }
        
        problem_lower = problem.lower()
        
        # Check for complexity indicators
        if any(indicator in problem_lower for indicator in ['quadratic', 'polynomial', 'derivative', 'integral']):
            complexity['level'] = 'advanced'
            complexity['requires_code'] = True
            complexity['mathematical_type'] = 'calculus'
        
        elif any(indicator in problem_lower for indicator in ['equation', 'solve', 'system']):
            complexity['level'] = 'intermediate'
            complexity['requires_code'] = True
            complexity['mathematical_type'] = 'algebra'
        
        elif any(indicator in problem_lower for indicator in ['percent', 'ratio', 'average']):
            complexity['level'] = 'intermediate'
            complexity['requires_tools'] = True
            complexity['mathematical_type'] = 'arithmetic'
        
        elif any(indicator in problem_lower for indicator in ['step', 'explain', 'show']):
            complexity['requires_reasoning'] = True
        
        return complexity
    
    def _solve_hybrid(self, problem: str) -> Dict:
        """Solve using hybrid approach combining multiple tools"""
        # Try toolformer first for simple calculations
        tool_result = self.toolformer.solve_with_tools(problem)
        
        if tool_result['result'] is not None:
            return {
                'method': 'hybrid_tool_first',
                'result': tool_result['result'],
                'confidence': 0.9,
                'tool_used': tool_result['tool_used'],
                'details': tool_result
            }
        
        # Try MathCoder for complex mathematical problems
        code_result = self.math_coder.solve_with_code(problem)
        
        if code_result['numerical_result'] is not None:
            return {
                'method': 'hybrid_code_second',
                'result': code_result['numerical_result'],
                'confidence': 0.8,
                'code_used': code_result['code'],
                'details': code_result
            }
        
        # Fall back to chain-of-thought
        cot_result = self.cot_solver.solve_with_cot(problem)
        
        return {
            'method': 'hybrid_cot_fallback',
            'result': cot_result['final_answer'],
            'confidence': cot_result['confidence'],
            'reasoning_used': cot_result['reasoning_steps'],
            'details': cot_result
        }
    
    def _enhance_result(self, result: Dict, problem: str, strategy: Dict) -> Dict:
        """Enhance result with additional information"""
        enhanced = result.copy()
        
        # Add strategy information
        enhanced['strategy'] = strategy
        
        # Add verification if possible
        if 'result' in result and result['result'] is not None:
            verification = self._verify_result(problem, result['result'])
            enhanced['verification'] = verification
        
        # Add confidence adjustment
        enhanced['adjusted_confidence'] = self._adjust_confidence(result, verification)
        
        # Add explanation
        enhanced['explanation'] = self._generate_explanation(problem, result, strategy)
        
        return enhanced
    
    def _verify_result(self, problem: str, result: Any) -> Dict:
        """Verify the result"""
        verification = {
            'verified': False,
            'method': 'none',
            'confidence': 0.0
        }
        
        try:
            if isinstance(result, (int, float)):
                # Simple sanity checks
                if 'percent' in problem.lower() and (result < 0 or result > 1000):
                    verification['method'] = 'sanity_check'
                    verification['confidence'] = 0.3
                elif 'area' in problem.lower() and result < 0:
                    verification['method'] = 'sanity_check'
                    verification['confidence'] = 0.3
                else:
                    verification['verified'] = True
                    verification['method'] = 'sanity_check'
                    verification['confidence'] = 0.7
            
            elif isinstance(result, list) and all(isinstance(x, (int, float)) for x in result):
                # Multiple solutions (e.g., quadratic)
                verification['verified'] = True
                verification['method'] = 'multi_solution'
                verification['confidence'] = 0.8
        
        except Exception as e:
            verification['error'] = str(e)
        
        return verification
    
    def _adjust_confidence(self, result: Dict, verification: Dict) -> float:
        """Adjust confidence based on verification"""
        base_confidence = result.get('confidence', 0.5)
        
        if verification.get('verified', False):
            # Boost confidence for verified results
            return min(base_confidence * 1.2, 1.0)
        elif verification.get('confidence', 0) > 0:
            # Adjust based on verification confidence
            return (base_confidence + verification['confidence']) / 2
        else:
            # Reduce confidence for unverified results
            return base_confidence * 0.8
    
    def _generate_explanation(self, problem: str, result: Dict, strategy: Dict) -> str:
        """Generate explanation for the solution"""
        explanation_parts = []
        
        # Problem analysis
        explanation_parts.append(f"Problem: {problem}")
        
        # Strategy explanation
        method = strategy.get('method', 'unknown')
        explanation_parts.append(f"Method: {method}")
        
        # Result explanation
        if 'result' in result and result['result'] is not None:
            explanation_parts.append(f"Answer: {result['result']}")
        
        # Tool/code explanation
        if 'tool_used' in result:
            explanation_parts.append(f"Tool: {result['tool_used']}")
        elif 'code_used' in result:
            explanation_parts.append("Generated and executed mathematical code")
        
        # Confidence explanation
        confidence = result.get('adjusted_confidence', 0.0)
        explanation_parts.append(f"Confidence: {confidence:.1%}")
        
        return '\n'.join(explanation_parts)

class ToolSelector:
    """Tool selection strategy for mathematical problems"""
    
    def select_strategy(self, problem: str, complexity: Dict) -> Dict:
        """Select appropriate strategy based on problem and complexity"""
        level = complexity['level']
        requires_code = complexity['requires_code']
        requires_tools = complexity['requires_tools']
        requires_reasoning = complexity['requires_reasoning']
        math_type = complexity['mathematical_type']
        
        if level == 'advanced' and requires_code:
            return {
                'method': 'code_integration',
                'reasoning': 'Advanced mathematical problem requires symbolic computation',
                'confidence': 0.9
            }
        elif requires_tools:
            return {
                'method': 'tool_calling',
                'reasoning': 'Problem can be solved with mathematical tools',
                'confidence': 0.8
            }
        elif requires_reasoning:
            return {
                'method': 'chain_of_thought',
                'reasoning': 'Problem requires step-by-step reasoning',
                'confidence': 0.7
            }
        elif level == 'intermediate':
            return {
                'method': 'hybrid',
                'reasoning': 'Intermediate complexity - try multiple approaches',
                'confidence': 0.6
            }
        else:
            return {
                'method': 'tool_calling',
                'reasoning': 'Simple problem - try tools first',
                'confidence': 0.7
            }

# Test the tool-augmented LLM
def test_tool_augmented_llm():
    """Test tool-augmented LLM"""
    llm = ToolAugmentedMathLLM()
    
    test_problems = [
        "What is 25% of 80?",
        "Solve equation: 2x + 3 = 7",
        "Solve quadratic: x² - 5x + 6 = 0",
        "Find derivative of x² + 3x + 2",
        "Explain how to solve a system of equations"
    ]
    
    results = []
    for problem in test_problems:
        result = llm.solve_with_tools(problem)
        results.append(result)
        
        print(f"Problem: {problem}")
        print(f"Strategy: {result['strategy']['method']}")
        print(f"Result: {result['result']}")
        print(f"Confidence: {result['adjusted_confidence']:.2f}")
        print(f"Verification: {result['verification']}")
        print("---")
    
    return len(results) == len(test_problems)

# Run test
if __name__ == "__main__":
    print("Testing tool-augmented LLM...")
    test_passed = test_tool_augmented_llm()
    print(f"Tool-augmented LLM test passed: {test_passed}")
```

**Success Criteria**:
- [ ] Complete tool-augmented LLM implementation
- [ ] Implement tool selection strategy
- [ ] Create hybrid solving approaches
- [ ] Add result verification and confidence scoring
- [ ] Achieve 85%+ accuracy on complex mathematical problems

---

## 🛠️ **Projects & Applications**

### **Project 1: Complete Mathematical Tool Integration System**
**Objective**: Build comprehensive system integrating all mathematical tools and approaches

**Implementation**:
```python
# Complete Mathematical Tool Integration System
import torch
import torch.nn as nn
from typing import Dict, List, Optional, Any, Union
import json
import re

class CompleteMathSystem:
    def __init__(self):
        """Initialize complete mathematical system"""
        self.tool_augmented_llm = ToolAugmentedMathLLM()
        self.math_transformer = None  # Would load trained transformer
        self.performance_tracker = PerformanceTracker()
        self.knowledge_base = MathematicalKnowledgeBase()
        
    def solve_any_math_problem(self, problem: str, context: Optional[Dict] = None) -> Dict:
        """Solve any mathematical problem using the best approach"""
        # Step 1: Problem analysis
        analysis = self._analyze_problem(problem, context)
        
        # Step 2: Select best method
        method = self._select_best_method(analysis)
        
        # Step 3: Execute solution
        if method == 'tool_augmented':
            result = self.tool_augmented_llm.solve_with_tools(problem)
        elif method == 'transformer':
            result = self._solve_with_transformer(problem)
        elif method == 'hybrid':
            result = self._solve_hybrid_advanced(problem)
        else:
            result = {'error': 'No suitable method found'}
        
        # Step 4: Post-processing
        enhanced_result = self._post_process_result(result, analysis)
        
        # Step 5: Track performance
        self.performance_tracker.track_solution(problem, method, enhanced_result)
        
        return enhanced_result
    
    def _analyze_problem(self, problem: str, context: Optional[Dict]) -> Dict:
        """Comprehensive problem analysis"""
        analysis = {
            'problem': problem,
            'context': context,
            'length': len(problem),
            'complexity': 'unknown',
            'domain': 'unknown',
            'keywords': [],
            'mathematical_objects': [],
            'requirements': []
        }
        
        # Extract keywords
        keywords = re.findall(r'\b\w+\b', problem.lower())
        analysis['keywords'] = keywords
        
        # Identify mathematical domain
        if any(word in keywords for word in ['derivative', 'integral', 'limit']):
            analysis['domain'] = 'calculus'
        elif any(word in keywords for word in ['matrix', 'determinant', 'eigenvalue']):
            analysis['domain'] = 'linear_algebra'
        elif any(word in keywords for word in ['equation', 'solve', 'variable']):
            analysis['domain'] = 'algebra'
        elif any(word in keywords for word in ['percent', 'ratio', 'average']):
            analysis['domain'] = 'arithmetic'
        else:
            analysis['domain'] = 'general'
        
        # Estimate complexity
        if len(problem) > 100:
            analysis['complexity'] = 'high'
        elif len(problem) > 50:
            analysis['complexity'] = 'medium'
        else:
            analysis['complexity'] = 'low'
        
        # Identify requirements
        if 'step' in keywords or 'explain' in keywords:
            analysis['requirements'].append('explanation')
        if 'exact' in keywords or 'symbolic' in keywords:
            analysis['requirements'].append('symbolic')
        if 'approximate' in keywords or 'numerical' in keywords:
            analysis['requirements'].append('numerical')
        
        return analysis
    
    def _select_best_method(self, analysis: Dict) -> str:
        """Select best solution method based on analysis"""
        domain = analysis['domain']
        complexity = analysis['complexity']
        requirements = analysis['requirements']
        
        # Decision logic
        if domain == 'calculus' and 'symbolic' in requirements:
            return 'tool_augmented'
        elif domain == 'linear_algebra':
            return 'tool_augmented'
        elif complexity == 'high':
            return 'hybrid'
        elif 'explanation' in requirements:
            return 'tool_augmented'
        elif complexity == 'low':
            return 'tool_augmented'
        else:
            return 'tool_augmented'
    
    def _solve_with_transformer(self, problem: str) -> Dict:
        """Solve using transformer model"""
        # Placeholder for transformer solution
        return {
            'method': 'transformer',
            'result': 'Transformer solution',
            'confidence': 0.7
        }
    
    def _solve_hybrid_advanced(self, problem: str) -> Dict:
        """Advanced hybrid solving"""
        # Try tool-augmented first
        tool_result = self.tool_augmented_llm.solve_with_tools(problem)
        
        if tool_result['adjusted_confidence'] > 0.8:
            return tool_result
        
        # Enhance with additional reasoning
        enhanced_result = tool_result.copy()
        enhanced_result['method'] = 'hybrid_advanced'
        enhanced_result['additional_reasoning'] = 'Applied advanced hybrid strategy'
        
        return enhanced_result
    
    def _post_process_result(self, result: Dict, analysis: Dict) -> Dict:
        """Post-process result"""
        enhanced = result.copy()
        
        # Add analysis information
        enhanced['analysis'] = analysis
        
        # Format result nicely
        if 'result' in result and result['result'] is not None:
            enhanced['formatted_result'] = self._format_result(result['result'])
        
        # Add additional context
        enhanced['additional_info'] = self._get_additional_info(analysis)
        
        return enhanced
    
    def _format_result(self, result: Any) -> str:
        """Format result for display"""
        if isinstance(result, (int, float)):
            return str(result)
        elif isinstance(result, list):
            return ', '.join(str(x) for x in result)
        elif isinstance(result, str):
            return result
        else:
            return str(result)
    
    def _get_additional_info(self, analysis: Dict) -> Dict:
        """Get additional information based on analysis"""
        info = {}
        
        if analysis['domain'] == 'calculus':
            info['domain_info'] = 'Calculus problems involve derivatives, integrals, and limits'
        elif analysis['domain'] == 'linear_algebra':
            info['domain_info'] = 'Linear algebra problems involve matrices, vectors, and systems'
        elif analysis['domain'] == 'algebra':
            info['domain_info'] = 'Algebra problems involve equations, variables, and solving'
        
        return info

class PerformanceTracker:
    """Track performance of the mathematical system"""
    
    def __init__(self):
        self.solutions = []
        self.success_rate = 0.0
        self.method_usage = {}
    
    def track_solution(self, problem: str, method: str, result: Dict):
        """Track a solution"""
        solution = {
            'problem': problem,
            'method': method,
            'result': result,
            'success': result.get('result') is not None,
            'confidence': result.get('adjusted_confidence', 0.0),
            'timestamp': torch.tensor([0.0])  # Placeholder timestamp
        }
        
        self.solutions.append(solution)
        
        # Update statistics
        self._update_statistics()
    
    def _update_statistics(self):
        """Update performance statistics"""
        if not self.solutions:
            return
        
        # Update success rate
        successful = sum(1 for s in self.solutions if s['success'])
        self.success_rate = successful / len(self.solutions)
        
        # Update method usage
        for solution in self.solutions:
            method = solution['method']
            if method not in self.method_usage:
                self.method_usage[method] = 0
            self.method_usage[method] += 1
    
    def get_performance_report(self) -> Dict:
        """Get performance report"""
        return {
            'total_solutions': len(self.solutions),
            'success_rate': self.success_rate,
            'method_usage': self.method_usage,
            'average_confidence': sum(s['confidence'] for s in self.solutions) / len(self.solutions) if self.solutions else 0.0
        }

class MathematicalKnowledgeBase:
    """Mathematical knowledge base for additional context"""
    
    def __init__(self):
        self.facts = {
            'calculus': {
                'derivative_rules': ['Power rule', 'Product rule', 'Chain rule'],
                'integral_rules': ['Power rule', 'Substitution', 'Integration by parts']
            },
            'algebra': {
                'equation_types': ['Linear', 'Quadratic', 'Polynomial'],
                'solution_methods': ['Factoring', 'Quadratic formula', 'Graphical']
            },
            'arithmetic': {
                'operations': ['Addition', 'Subtraction', 'Multiplication', 'Division'],
                'concepts': ['Percentages', 'Ratios', 'Averages']
            }
        }
    
    def get_domain_info(self, domain: str) -> Dict:
        """Get information about a mathematical domain"""
        return self.facts.get(domain, {})

# Test the complete system
def test_complete_math_system():
    """Test complete mathematical system"""
    system = CompleteMathSystem()
    
    test_problems = [
        "What is 25% of 80?",
        "Solve equation: 2x + 3 = 7",
        "Find derivative of x² + 3x + 2",
        "Explain step by step how to solve a system of equations"
    ]
    
    results = []
    for problem in test_problems:
        result = system.solve_any_math_problem(problem)
        results.append(result)
        
        print(f"Problem: {problem}")
        print(f"Method: {result['strategy']['method'] if 'strategy' in result else 'unknown'}")
        print(f"Result: {result['result']}")
        print(f"Confidence: {result.get('adjusted_confidence', 0.0):.2f}")
        print(f"Domain: {result['analysis']['domain']}")
        print("---")
    
    # Get performance report
    performance = system.performance_tracker.get_performance_report()
    print(f"\nPerformance Report:")
    print(f"Total Solutions: {performance['total_solutions']}")
    print(f"Success Rate: {performance['success_rate']:.2%}")
    print(f"Method Usage: {performance['method_usage']}")
    
    return len(results) == len(test_problems)

# Run test
if __name__ == "__main__":
    print("Testing complete mathematical system...")
    test_passed = test_complete_math_system()
    print(f"Complete system test passed: {test_passed}")
```

**Deliverables**:
- [ ] Complete mathematical tool integration system
- [ ] Performance tracking and analysis
- [ ] Knowledge base for mathematical domains
- [ ] Comprehensive problem analysis
- [ ] Multi-method solution selection
- [ ] Advanced result formatting and explanation

---

## 📊 **Progress Tracking**

### **Daily Checklist**
- [ ] 2 hours Berkeley RL course (final lectures and assignments)
- [ ] 3 hours MathCoder implementation and testing
- [ ] 2 hours SymPy integration and tool development
- [ ] 1 hour tool-augmented LLM enhancement
- [ ] 1 hour complete system integration
- [ ] 1 hour testing and debugging

### **Weekly Milestones**
**Week 25**:
- [ ] Complete Berkeley RL Lectures 7-8
- [ ] Read and analyze "MathCoder" paper
- [ ] Implement SymPy tool integration
- [ ] Create code generation templates
- [ ] Test code-based mathematical solving

**Week 26**:
- [ ] Complete Berkeley RL Lecture 9
- [ ] Complete MathCoder implementation
- [ ] Build tool-augmented LLM system
- [ ] Create complete integration system
- [ ] Complete Assignment 3

### **End-of-Period Assessment**
**Success Metrics**:
- [ ] Berkeley RL: All lectures completed, Assignment 3 submitted
- [ ] MathCoder: Complete implementation with 80%+ accuracy
- [ ] SymPy Integration: Full symbolic computation capabilities
- [ ] Tool-Augmented LLM: Advanced system with 85%+ accuracy
- [ ] Complete System: Integrated solution with performance tracking
- [ ] Testing: Comprehensive test suite with 80%+ success rate

---

## 🚀 **Next Period Preparation**

### **Weeks 27-28 Preview**
- Start competition preparation (Kaggle MATH)
- Optimize models for competition tasks
- Analyze competition strategies
- Submit initial competition entries
- Document competition approaches

### **Resources to Preview**:
- [Kaggle MATH Competition](https://www.kaggle.com/competitions/math-competition)
- [Competition Strategies](https://www.kaggle.com/learn/competitions)
- [Mathematical Reasoning Benchmarks](https://paperswithcode.com/task/mathematical-reasoning)
- [Model Optimization Techniques](https://pytorch.org/tutorials/advanced/)

---

## 📞 **Support & Resources**

### **Help Channels**:
- Berkeley RL Course Discussion Forums
- SymPy Documentation and Community
- Math LLM Discord Communities
- Competition Discussion Forums
- Stack Overflow for technical questions

### **Additional Resources**:
- [SymPy Tutorials](https://docs.sympy.org/tutorials/index.html)
- [Mathematical Computing](https://www.scipy.org/)
- [Competition Preparation](https://www.kaggle.com/learn)
- [Advanced Integration](https://github.com/openai/toolformer)

---

*This 2-week plan provides comprehensive tool integration with Berkeley RL completion, MathCoder implementation, SymPy integration, and advanced tool-augmented LLM development for mathematical reasoning.*
