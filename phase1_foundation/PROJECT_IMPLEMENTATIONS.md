# Phase 1 Projects - Detailed Implementation Guide

## 🚀 **Project 1: Mathematical Foundation Toolkit - Implementation**

### **Detailed Implementation Plan**

#### **1. Linear Algebra Library Implementation**

```python
# linear_algebra.py
import numpy as np
from typing import Tuple, List, Union

class LinearAlgebraLibrary:
    """Comprehensive linear algebra operations with performance optimization"""
    
    def __init__(self):
        self.cache = {}  # Cache for expensive operations
    
    def matrix_multiply(self, A: np.ndarray, B: np.ndarray) -> np.ndarray:
        """Optimized matrix multiplication with error checking"""
        if A.shape[1] != B.shape[0]:
            raise ValueError(f"Cannot multiply {A.shape} by {B.shape}")
        
        # Use NumPy's optimized implementation
        result = np.dot(A, B)
        
        # Cache result for repeated computations
        cache_key = f"mul_{A.shape}_{B.shape}_{hash(A.tobytes())}"
        self.cache[cache_key] = result
        
        return result
    
    def compute_eigenvalues(self, matrix: np.ndarray) -> Tuple[np.ndarray, np.ndarray]:
        """Compute eigenvalues and eigenvectors with numerical stability"""
        if matrix.shape[0] != matrix.shape[1]:
            raise ValueError("Matrix must be square for eigenvalue computation")
        
        # Use NumPy's robust eigenvalue computation
        eigenvalues, eigenvectors = np.linalg.eig(matrix)
        
        # Sort by eigenvalue magnitude for consistency
        idx = np.argsort(np.abs(eigenvalues))[::-1]
        eigenvalues = eigenvalues[idx]
        eigenvectors = eigenvectors[:, idx]
        
        return eigenvalues, eigenvectors
    
    def matrix_inverse(self, matrix: np.ndarray) -> np.ndarray:
        """Compute matrix inverse with singularity check"""
        if matrix.shape[0] != matrix.shape[1]:
            raise ValueError("Matrix must be square for inversion")
        
        # Check condition number for numerical stability
        cond_num = np.linalg.cond(matrix)
        if cond_num > 1e12:
            print(f"Warning: Matrix is ill-conditioned (cond={cond_num:.2e})")
        
        return np.linalg.inv(matrix)
    
    def matrix_determinant(self, matrix: np.ndarray) -> float:
        """Compute determinant with optimization"""
        if matrix.shape[0] != matrix.shape[1]:
            raise ValueError("Matrix must be square for determinant")
        
        return np.linalg.det(matrix)
    
    def solve_linear_system(self, A: np.ndarray, b: np.ndarray) -> np.ndarray:
        """Solve Ax = b using least squares for robustness"""
        return np.linalg.lstsq(A, b, rcond=None)[0]

# Performance benchmarking
def benchmark_linear_algebra():
    """Benchmark linear algebra operations"""
    lib = LinearAlgebraLibrary()
    
    # Test matrix sizes
    sizes = [100, 500, 1000]
    results = {}
    
    for size in sizes:
        A = np.random.randn(size, size)
        B = np.random.randn(size, size)
        
        import time
        start_time = time.time()
        result = lib.matrix_multiply(A, B)
        end_time = time.time()
        
        results[size] = end_time - start_time
        print(f"Matrix multiplication {size}x{size}: {results[size]:.4f}s")
    
    return results
```

#### **2. Statistics Calculator Implementation**

```python
# statistics_calculator.py
import numpy as np
from scipy import stats
from typing import Dict, List, Tuple, Optional

class StatisticsCalculator:
    """Comprehensive statistical analysis with hypothesis testing"""
    
    def __init__(self):
        self.data_cache = {}
    
    def descriptive_stats(self, data: np.ndarray) -> Dict[str, float]:
        """Compute comprehensive descriptive statistics"""
        return {
            'mean': np.mean(data),
            'median': np.median(data),
            'std': np.std(data, ddof=1),
            'variance': np.var(data, ddof=1),
            'min': np.min(data),
            'max': np.max(data),
            'q25': np.percentile(data, 25),
            'q75': np.percentile(data, 75),
            'skewness': stats.skew(data),
            'kurtosis': stats.kurtosis(data),
            'n': len(data)
        }
    
    def correlation_analysis(self, x: np.ndarray, y: np.ndarray) -> Dict[str, float]:
        """Comprehensive correlation analysis"""
        pearson_r, pearson_p = stats.pearsonr(x, y)
        spearman_r, spearman_p = stats.spearmanr(x, y)
        
        return {
            'pearson_r': pearson_r,
            'pearson_p': pearson_p,
            'spearman_r': spearman_r,
            'spearman_p': spearman_p,
            'covariance': np.cov(x, y)[0, 1]
        }
    
    def t_test_one_sample(self, data: np.ndarray, mu: float = 0) -> Dict[str, float]:
        """One-sample t-test"""
        t_stat, p_value = stats.ttest_1samp(data, mu)
        
        return {
            't_statistic': t_stat,
            'p_value': p_value,
            'degrees_of_freedom': len(data) - 1,
            'mean': np.mean(data),
            'std_error': stats.sem(data),
            'significant': p_value < 0.05
        }
    
    def t_test_two_sample(self, group1: np.ndarray, group2: np.ndarray) -> Dict[str, float]:
        """Two-sample t-test"""
        t_stat, p_value = stats.ttest_ind(group1, group2)
        
        return {
            't_statistic': t_stat,
            'p_value': p_value,
            'degrees_of_freedom': len(group1) + len(group2) - 2,
            'mean1': np.mean(group1),
            'mean2': np.mean(group2),
            'significant': p_value < 0.05
        }
    
    def confidence_interval(self, data: np.ndarray, confidence: float = 0.95) -> Tuple[float, float]:
        """Calculate confidence interval for mean"""
        mean = np.mean(data)
        sem = stats.sem(data)
        df = len(data) - 1
        
        # Use t-distribution for small samples
        t_critical = stats.t.ppf((1 + confidence) / 2, df)
        margin_error = t_critical * sem
        
        return (mean - margin_error, mean + margin_error)
    
    def chi_square_test(self, observed: np.ndarray, expected: Optional[np.ndarray] = None) -> Dict[str, float]:
        """Chi-square goodness of fit test"""
        if expected is None:
            expected = np.full_like(observed, np.mean(observed))
        
        chi2_stat, p_value = stats.chisquare(observed, expected)
        
        return {
            'chi2_statistic': chi2_stat,
            'p_value': p_value,
            'degrees_of_freedom': len(observed) - 1,
            'significant': p_value < 0.05
        }

# Usage example
def demo_statistics():
    """Demonstrate statistical analysis"""
    calc = StatisticsCalculator()
    
    # Generate sample data
    np.random.seed(42)
    data1 = np.random.normal(100, 15, 1000)
    data2 = np.random.normal(105, 15, 1000)
    
    # Descriptive statistics
    stats1 = calc.descriptive_stats(data1)
    print("Data 1 Statistics:", stats1)
    
    # Correlation analysis
    corr = calc.correlation_analysis(data1, data2)
    print("Correlation Analysis:", corr)
    
    # Two-sample t-test
    t_test = calc.t_test_two_sample(data1, data2)
    print("Two-sample t-test:", t_test)
    
    # Confidence interval
    ci = calc.confidence_interval(data1)
    print(f"95% CI: [{ci[0]:.2f}, {ci[1]:.2f}]")
```

#### **3. Visualization Tools Implementation**

```python
# visualization_tools.py
import matplotlib.pyplot as plt
import seaborn as sns
import numpy as np
from typing import List, Tuple, Optional
import plotly.graph_objects as go
import plotly.express as px

class VisualizationTools:
    """Advanced visualization tools for mathematical concepts"""
    
    def __init__(self):
        plt.style.use('seaborn-v0_8')
        sns.set_palette("husl")
    
    def plot_function_2d(self, func, x_range: Tuple[float, float], 
                        title: str = "Function Plot") -> plt.Figure:
        """Plot 2D mathematical function"""
        x = np.linspace(x_range[0], x_range[1], 1000)
        y = func(x)
        
        fig, ax = plt.subplots(figsize=(10, 6))
        ax.plot(x, y, linewidth=2, color='blue')
        ax.set_title(title, fontsize=14, fontweight='bold')
        ax.set_xlabel('x', fontsize=12)
        ax.set_ylabel('f(x)', fontsize=12)
        ax.grid(True, alpha=0.3)
        
        return fig
    
    def plot_3d_surface(self, func, x_range: Tuple[float, float], 
                        y_range: Tuple[float, float], 
                        title: str = "3D Surface Plot") -> plt.Figure:
        """Plot 3D surface"""
        x = np.linspace(x_range[0], x_range[1], 100)
        y = np.linspace(y_range[0], y_range[1], 100)
        X, Y = np.meshgrid(x, y)
        Z = func(X, Y)
        
        fig = plt.figure(figsize=(12, 8))
        ax = fig.add_subplot(111, projection='3d')
        
        surface = ax.plot_surface(X, Y, Z, cmap='viridis', alpha=0.8)
        ax.set_title(title, fontsize=14, fontweight='bold')
        ax.set_xlabel('x', fontsize=12)
        ax.set_ylabel('y', fontsize=12)
        ax.set_zlabel('f(x, y)', fontsize=12)
        
        fig.colorbar(surface, ax=ax, shrink=0.5)
        return fig
    
    def plot_eigenvectors(self, matrix: np.ndarray, title: str = "Eigenvectors") -> plt.Figure:
        """Visualize eigenvectors of 2x2 matrix"""
        eigenvalues, eigenvectors = np.linalg.eig(matrix)
        
        fig, ax = plt.subplots(figsize=(8, 8))
        
        # Plot original vectors
        ax.quiver(0, 0, 1, 0, angles='xy', scale_units='xy', scale=1, 
                 color='blue', alpha=0.6, width=0.005, label='Standard basis')
        ax.quiver(0, 0, 0, 1, angles='xy', scale_units='xy', scale=1, 
                 color='blue', alpha=0.6, width=0.005)
        
        # Plot eigenvectors
        colors = ['red', 'green']
        for i, (eigval, eigvec) in enumerate(zip(eigenvalues, eigenvectors.T)):
            ax.quiver(0, 0, eigvec[0], eigvec[1], angles='xy', scale_units='xy', scale=1,
                     color=colors[i], alpha=0.8, width=0.008, 
                     label=f'Eigenvector {i+1} (λ={eigval:.2f})')
        
        ax.set_xlim(-2, 2)
        ax.set_ylim(-2, 2)
        ax.set_aspect('equal')
        ax.grid(True, alpha=0.3)
        ax.legend()
        ax.set_title(title, fontsize=14, fontweight='bold')
        
        return fig
    
    def plot_distribution_comparison(self, data: List[np.ndarray], 
                                  labels: List[str]) -> plt.Figure:
        """Compare multiple distributions"""
        fig, axes = plt.subplots(2, 2, figsize=(15, 10))
        
        # Histograms
        axes[0, 0].set_title('Histograms')
        for i, (d, label) in enumerate(zip(data, labels)):
            axes[0, 0].hist(d, alpha=0.6, bins=30, label=label)
        axes[0, 0].legend()
        
        # Box plots
        axes[0, 1].set_title('Box Plots')
        axes[0, 1].boxplot(data, labels=labels)
        
        # Q-Q plots
        axes[1, 0].set_title('Q-Q Plots')
        for i, (d, label) in enumerate(zip(data, labels)):
            stats.probplot(d, dist="norm", plot=axes[1, 0])
        axes[1, 0].set_title('Q-Q Plots (Normal)')
        
        # Violin plots
        axes[1, 1].set_title('Violin Plots')
        axes[1, 1].violinplot(data)
        axes[1, 1].set_xticks(range(1, len(labels) + 1), labels)
        
        plt.tight_layout()
        return fig
    
    def interactive_attention_heatmap(self, attention_weights: np.ndarray, 
                                   tokens: List[str]) -> go.Figure:
        """Create interactive attention heatmap"""
        fig = go.Figure(data=go.Heatmap(
            z=attention_weights,
            x=tokens,
            y=tokens,
            colorscale='Blues',
            showscale=True,
            hoverongaps=False
        ))
        
        fig.update_layout(
            title='Attention Weights Heatmap',
            xaxis_title='Key Tokens',
            yaxis_title='Query Tokens',
            width=800,
            height=600
        )
        
        return fig

# Demo functions
def demo_visualizations():
    """Demonstrate visualization capabilities"""
    viz = VisualizationTools()
    
    # Function plotting
    fig1 = viz.plot_function_2d(lambda x: x**2 - 4*x + 3, (-2, 6), "Quadratic Function")
    
    # 3D surface
    fig2 = viz.plot_3d_surface(lambda x, y: x**2 + y**2, (-3, 3), (-3, 3), "Paraboloid")
    
    # Eigenvector visualization
    matrix = np.array([[2, 1], [1, 2]])
    fig3 = viz.plot_eigenvectors(matrix, "Eigenvectors of [[2,1],[1,2]]")
    
    plt.show()
```

---

## 📊 **Project 2: Real-World Data Analysis - Implementation**

### **Detailed Implementation Plan**

#### **1. Data Collection Pipeline**

```python
# data_pipeline.py
import pandas as pd
import numpy as np
from typing import Dict, List, Tuple, Optional
from sklearn.preprocessing import StandardScaler, LabelEncoder
from sklearn.impute import SimpleImputer
import warnings
warnings.filterwarnings('ignore')

class DataPipeline:
    """Complete data processing pipeline for real-world datasets"""
    
    def __init__(self):
        self.scaler = StandardScaler()
        self.label_encoders = {}
        self.imputers = {}
        self.processing_log = []
    
    def load_data(self, file_path: str, target_column: str = None) -> pd.DataFrame:
        """Load and initial data inspection"""
        try:
            # Try different file formats
            if file_path.endswith('.csv'):
                df = pd.read_csv(file_path)
            elif file_path.endswith('.xlsx'):
                df = pd.read_excel(file_path)
            else:
                raise ValueError("Unsupported file format")
            
            self.processing_log.append(f"Loaded data: {df.shape}")
            self.processing_log.append(f"Columns: {list(df.columns)}")
            
            if target_column and target_column not in df.columns:
                raise ValueError(f"Target column '{target_column}' not found")
            
            return df
            
        except Exception as e:
            self.processing_log.append(f"Error loading data: {str(e)}")
            raise
    
    def analyze_data_quality(self, df: pd.DataFrame) -> Dict:
        """Comprehensive data quality analysis"""
        quality_report = {
            'total_rows': len(df),
            'total_columns': len(df.columns),
            'missing_values': df.isnull().sum().to_dict(),
            'duplicate_rows': df.duplicated().sum(),
            'data_types': df.dtypes.to_dict(),
            'memory_usage': df.memory_usage(deep=True).sum()
        }
        
        # Missing value percentage
        missing_pct = (df.isnull().sum() / len(df) * 100).round(2)
        quality_report['missing_percentage'] = missing_pct.to_dict()
        
        # Numeric columns statistics
        numeric_cols = df.select_dtypes(include=[np.number]).columns
        if len(numeric_cols) > 0:
            quality_report['numeric_summary'] = df[numeric_cols].describe().to_dict()
        
        self.processing_log.append(f"Data quality analysis completed")
        return quality_report
    
    def clean_data(self, df: pd.DataFrame, 
                   missing_threshold: float = 0.05,
                   duplicate_action: str = 'drop') -> pd.DataFrame:
        """Clean data with configurable strategies"""
        original_shape = df.shape
        df_clean = df.copy()
        
        # Handle duplicates
        if duplicate_action == 'drop':
            df_clean = df_clean.drop_duplicates()
            self.processing_log.append(f"Dropped {original_shape[0] - len(df_clean)} duplicate rows")
        
        # Handle missing values
        missing_pct = df_clean.isnull().sum() / len(df_clean)
        
        # Drop columns with too many missing values
        cols_to_drop = missing_pct[missing_pct > missing_threshold].index.tolist()
        if cols_to_drop:
            df_clean = df_clean.drop(columns=cols_to_drop)
            self.processing_log.append(f"Dropped columns with >{missing_threshold*100}% missing: {cols_to_drop}")
        
        # Impute remaining missing values
        for col in df_clean.columns:
            if df_clean[col].isnull().sum() > 0:
                if df_clean[col].dtype in ['object', 'category']:
                    # Categorical: use mode
                    imputer = SimpleImputer(strategy='most_frequent')
                    df_clean[col] = imputer.fit_transform(df_clean[[col]]).ravel()
                else:
                    # Numerical: use median
                    imputer = SimpleImputer(strategy='median')
                    df_clean[col] = imputer.fit_transform(df_clean[[col]]).ravel()
                
                self.imputers[col] = imputer
                self.processing_log.append(f"Imputed missing values in {col}")
        
        final_shape = df_clean.shape
        data_loss = (original_shape[0] - final_shape[0]) / original_shape[0] * 100
        self.processing_log.append(f"Data loss due to cleaning: {data_loss:.2f}%")
        
        return df_clean
    
    def detect_outliers(self, df: pd.DataFrame, 
                       method: str = 'iqr',
                       threshold: float = 1.5) -> Dict:
        """Outlier detection using multiple methods"""
        outliers_info = {}
        numeric_cols = df.select_dtypes(include=[np.number]).columns
        
        for col in numeric_cols:
            if method == 'iqr':
                Q1 = df[col].quantile(0.25)
                Q3 = df[col].quantile(0.75)
                IQR = Q3 - Q1
                lower_bound = Q1 - threshold * IQR
                upper_bound = Q3 + threshold * IQR
                
                outliers = df[(df[col] < lower_bound) | (df[col] > upper_bound)]
                outliers_info[col] = {
                    'count': len(outliers),
                    'percentage': len(outliers) / len(df) * 100,
                    'bounds': (lower_bound, upper_bound)
                }
            
            elif method == 'zscore':
                z_scores = np.abs((df[col] - df[col].mean()) / df[col].std())
                outliers = df[z_scores > threshold]
                outliers_info[col] = {
                    'count': len(outliers),
                    'percentage': len(outliers) / len(df) * 100,
                    'threshold': threshold
                }
        
        return outliers_info
    
    def normalize_features(self, df: pd.DataFrame, 
                          method: str = 'standard') -> pd.DataFrame:
        """Feature normalization/standardization"""
        df_normalized = df.copy()
        numeric_cols = df_normalized.select_dtypes(include=[np.number]).columns
        
        if method == 'standard':
            df_normalized[numeric_cols] = self.scaler.fit_transform(df_normalized[numeric_cols])
        elif method == 'minmax':
            from sklearn.preprocessing import MinMaxScaler
            scaler = MinMaxScaler()
            df_normalized[numeric_cols] = scaler.fit_transform(df_normalized[numeric_cols])
        
        self.processing_log.append(f"Normalized {len(numeric_cols)} numeric columns using {method} method")
        return df_normalized
    
    def encode_categorical(self, df: pd.DataFrame) -> pd.DataFrame:
        """Encode categorical variables"""
        df_encoded = df.copy()
        categorical_cols = df_encoded.select_dtypes(include=['object', 'category']).columns
        
        for col in categorical_cols:
            if col not in self.label_encoders:
                self.label_encoders[col] = LabelEncoder()
                df_encoded[col] = self.label_encoders[col].fit_transform(df_encoded[col].astype(str))
            else:
                df_encoded[col] = self.label_encoders[col].transform(df_encoded[col].astype(str))
        
        self.processing_log.append(f"Encoded {len(categorical_cols)} categorical columns")
        return df_encoded
    
    def get_processing_summary(self) -> Dict:
        """Get complete processing summary"""
        return {
            'processing_steps': self.processing_log,
            'imputers_used': list(self.imputers.keys()),
            'encoders_used': list(self.label_encoders.keys()),
            'total_steps': len(self.processing_log)
        }

# Usage example
def demo_data_pipeline():
    """Demonstrate complete data pipeline"""
    pipeline = DataPipeline()
    
    # Load data (example with student performance dataset)
    # df = pipeline.load_data('student_performance.csv', target_column='score')
    
    # For demonstration, create sample data
    np.random.seed(42)
    sample_data = {
        'study_hours': np.random.normal(10, 3, 1000),
        'previous_score': np.random.normal(75, 10, 1000),
        'attendance': np.random.uniform(60, 100, 1000),
        'gender': np.random.choice(['male', 'female'], 1000),
        'score': np.random.normal(80, 15, 1000)
    }
    df = pd.DataFrame(sample_data)
    
    # Add some missing values
    df.loc[np.random.choice(1000, 50), 'study_hours'] = np.nan
    df.loc[np.random.choice(1000, 30), 'previous_score'] = np.nan
    
    # Process data
    quality_report = pipeline.analyze_data_quality(df)
    print("Data Quality Report:")
    print(f"Shape: {quality_report['total_rows']} x {quality_report['total_columns']}")
    print(f"Missing values: {quality_report['missing_values']}")
    
    df_clean = pipeline.clean_data(df)
    outliers = pipeline.detect_outliers(df_clean)
    print(f"Outliers detected: {outliers}")
    
    df_normalized = pipeline.normalize_features(df_clean)
    df_encoded = pipeline.encode_categorical(df_normalized)
    
    summary = pipeline.get_processing_summary()
    print(f"Processing completed: {summary['total_steps']} steps")
    
    return df_encoded
```

#### **2. Statistical Analysis Implementation**

```python
# statistical_analysis.py
import pandas as pd
import numpy as np
from scipy import stats
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression, LogisticRegression
from sklearn.metrics import r2_score, mean_squared_error, accuracy_score, classification_report
import matplotlib.pyplot as plt
import seaborn as sns
from typing import Dict, List, Tuple

class StatisticalAnalyzer:
    """Comprehensive statistical analysis for real-world data"""
    
    def __init__(self):
        self.models = {}
        self.results = {}
    
    def descriptive_analysis(self, df: pd.DataFrame) -> Dict:
        """Complete descriptive statistics analysis"""
        analysis = {}
        
        # Numerical variables
        numeric_cols = df.select_dtypes(include=[np.number]).columns
        if len(numeric_cols) > 0:
            analysis['numeric_summary'] = df[numeric_cols].describe().to_dict()
            
            # Distribution analysis for each numeric variable
            for col in numeric_cols:
                analysis[f'{col}_distribution'] = {
                    'skewness': stats.skew(df[col].dropna()),
                    'kurtosis': stats.kurtosis(df[col].dropna()),
                    'normality_test': stats.shapiro(df[col].dropna()) if len(df[col].dropna()) > 3 else None
                }
        
        # Categorical variables
        categorical_cols = df.select_dtypes(include=['object', 'category']).columns
        if len(categorical_cols) > 0:
            analysis['categorical_summary'] = {}
            for col in categorical_cols:
                analysis['categorical_summary'][col] = df[col].value_counts().to_dict()
        
        return analysis
    
    def correlation_analysis(self, df: pd.DataFrame) -> Dict:
        """Comprehensive correlation analysis"""
        numeric_df = df.select_dtypes(include=[np.number])
        
        if len(numeric_df.columns) < 2:
            return {'error': 'Need at least 2 numeric columns for correlation analysis'}
        
        # Pearson correlation
        pearson_corr = numeric_df.corr(method='pearson')
        
        # Spearman correlation
        spearman_corr = numeric_df.corr(method='spearman')
        
        # Significant correlations (p < 0.05)
        significant_correlations = {}
        for i, col1 in enumerate(numeric_df.columns):
            for j, col2 in enumerate(numeric_df.columns):
                if i < j:  # Avoid duplicates
                    corr, p_value = stats.pearsonr(numeric_df[col1].dropna(), numeric_df[col2].dropna())
                    if p_value < 0.05:
                        significant_correlations[f'{col1}_vs_{col2}'] = {
                            'correlation': corr,
                            'p_value': p_value,
                            'strength': self._interpret_correlation(abs(corr))
                        }
        
        return {
            'pearson_correlation': pearson_corr.to_dict(),
            'spearman_correlation': spearman_corr.to_dict(),
            'significant_correlations': significant_correlations
        }
    
    def _interpret_correlation(self, corr_value: float) -> str:
        """Interpret correlation strength"""
        abs_corr = abs(corr_value)
        if abs_corr < 0.1:
            return 'negligible'
        elif abs_corr < 0.3:
            return 'weak'
        elif abs_corr < 0.5:
            return 'moderate'
        elif abs_corr < 0.7:
            return 'strong'
        else:
            return 'very strong'
    
    def hypothesis_testing(self, df: pd.DataFrame, 
                          target_col: str,
                          group_col: str = None) -> Dict:
        """Comprehensive hypothesis testing"""
        results = {}
        
        if group_col and group_col in df.columns:
            # Compare groups
            groups = df[group_col].unique()
            if len(groups) == 2:
                # Two-sample t-test
                group1_data = df[df[group_col] == groups[0]][target_col].dropna()
                group2_data = df[df[group_col] == groups[1]][target_col].dropna()
                
                t_stat, p_value = stats.ttest_ind(group1_data, group2_data)
                results['two_sample_t_test'] = {
                    't_statistic': t_stat,
                    'p_value': p_value,
                    'significant': p_value < 0.05,
                    'group1_mean': group1_data.mean(),
                    'group2_mean': group2_data.mean(),
                    'effect_size': (group2_data.mean() - group1_data.mean()) / np.sqrt(((len(group1_data)-1)*group1_data.var() + (len(group2_data)-1)*group2_data.var()) / (len(group1_data)+len(group2_data)-2))
                }
            
            elif len(groups) > 2:
                # ANOVA
                group_data = [df[df[group_col] == group][target_col].dropna() for group in groups]
                f_stat, p_value = stats.f_oneway(*group_data)
                results['anova'] = {
                    'f_statistic': f_stat,
                    'p_value': p_value,
                    'significant': p_value < 0.05,
                    'groups': len(groups)
                }
        
        # One-sample t-test against population mean
        population_mean = df[target_col].mean()
        sample_data = df[target_col].dropna()
        t_stat, p_value = stats.ttest_1samp(sample_data, population_mean)
        
        results['one_sample_t_test'] = {
            't_statistic': t_stat,
            'p_value': p_value,
            'population_mean': population_mean,
            'sample_mean': sample_data.mean(),
            'sample_std': sample_data.std()
        }
        
        return results
    
    def regression_analysis(self, df: pd.DataFrame, 
                           target_col: str,
                           test_size: float = 0.2) -> Dict:
        """Linear regression analysis with model evaluation"""
        # Prepare data
        feature_cols = [col for col in df.columns if col != target_col and df[col].dtype in ['int64', 'float64']]
        
        if len(feature_cols) == 0:
            return {'error': 'No suitable feature columns found'}
        
        X = df[feature_cols]
        y = df[target_col]
        
        # Split data
        X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=test_size, random_state=42)
        
        # Train model
        model = LinearRegression()
        model.fit(X_train, y_train)
        
        # Make predictions
        y_train_pred = model.predict(X_train)
        y_test_pred = model.predict(X_test)
        
        # Evaluate
        results = {
            'model_coefficients': dict(zip(feature_cols, model.coef_)),
            'intercept': model.intercept_,
            'training_metrics': {
                'r2_score': r2_score(y_train, y_train_pred),
                'rmse': np.sqrt(mean_squared_error(y_train, y_train_pred)),
                'mae': np.mean(np.abs(y_train - y_train_pred))
            },
            'test_metrics': {
                'r2_score': r2_score(y_test, y_test_pred),
                'rmse': np.sqrt(mean_squared_error(y_test, y_test_pred)),
                'mae': np.mean(np.abs(y_test - y_test_pred))
            },
            'feature_importance': dict(zip(feature_cols, np.abs(model.coef_)))
        }
        
        # Store model
        self.models['linear_regression'] = model
        self.results['regression'] = results
        
        return results
    
    def classification_analysis(self, df: pd.DataFrame,
                              target_col: str,
                              test_size: float = 0.2) -> Dict:
        """Logistic regression classification analysis"""
        # Check if target is binary
        unique_values = df[target_col].unique()
        if len(unique_values) != 2:
            return {'error': 'Target must be binary for classification'}
        
        # Prepare data
        feature_cols = [col for col in df.columns if col != target_col and df[col].dtype in ['int64', 'float64']]
        
        if len(feature_cols) == 0:
            return {'error': 'No suitable feature columns found'}
        
        X = df[feature_cols]
        y = df[target_col]
        
        # Split data
        X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=test_size, random_state=42, stratify=y)
        
        # Train model
        model = LogisticRegression(random_state=42, max_iter=1000)
        model.fit(X_train, y_train)
        
        # Make predictions
        y_train_pred = model.predict(X_train)
        y_test_pred = model.predict(X_test)
        
        # Evaluate
        results = {
            'model_coefficients': dict(zip(feature_cols, model.coef_[0])),
            'intercept': model.intercept_[0],
            'training_metrics': {
                'accuracy': accuracy_score(y_train, y_train_pred),
                'classification_report': classification_report(y_train, y_train_pred, output_dict=True)
            },
            'test_metrics': {
                'accuracy': accuracy_score(y_test, y_test_pred),
                'classification_report': classification_report(y_test, y_test_pred, output_dict=True)
            },
            'feature_importance': dict(zip(feature_cols, np.abs(model.coef_[0])))
        }
        
        # Store model
        self.models['logistic_regression'] = model
        self.results['classification'] = results
        
        return results
    
    def generate_report(self, df: pd.DataFrame, target_col: str) -> str:
        """Generate comprehensive statistical report"""
        report = []
        report.append("STATISTICAL ANALYSIS REPORT")
        report.append("=" * 50)
        
        # Descriptive analysis
        desc_analysis = self.descriptive_analysis(df)
        report.append("\n1. DESCRIPTIVE STATISTICS")
        report.append(f"Dataset shape: {df.shape}")
        report.append(f"Numeric columns: {len(df.select_dtypes(include=[np.number]).columns)}")
        report.append(f"Categorical columns: {len(df.select_dtypes(include=['object', 'category']).columns)}")
        
        # Correlation analysis
        corr_analysis = self.correlation_analysis(df)
        if 'significant_correlations' in corr_analysis:
            report.append(f"\n2. CORRELATION ANALYSIS")
            report.append(f"Significant correlations found: {len(corr_analysis['significant_correlations'])}")
            for corr_name, corr_info in corr_analysis['significant_correlations'].items():
                report.append(f"  {corr_name}: {corr_info['correlation']:.3f} ({corr_info['strength']})")
        
        # Regression analysis (if applicable)
        if df[target_col].dtype in ['int64', 'float64']:
            reg_results = self.regression_analysis(df, target_col)
            if 'test_metrics' in reg_results:
                report.append(f"\n3. REGRESSION ANALYSIS")
                report.append(f"Test R²: {reg_results['test_metrics']['r2_score']:.3f}")
                report.append(f"Test RMSE: {reg_results['test_metrics']['rmse']:.3f}")
        
        return "\n".join(report)

# Usage example
def demo_statistical_analysis():
    """Demonstrate statistical analysis"""
    # Create sample data
    np.random.seed(42)
    data = {
        'study_hours': np.random.normal(10, 3, 1000),
        'previous_score': np.random.normal(75, 10, 1000),
        'attendance': np.random.uniform(60, 100, 1000),
        'score': np.random.normal(80, 15, 1000)
    }
    df = pd.DataFrame(data)
    
    # Add relationship
    df['score'] = 50 + 2*df['study_hours'] + 0.3*df['previous_score'] + 0.1*df['attendance'] + np.random.normal(0, 5, 1000)
    
    analyzer = StatisticalAnalyzer()
    
    # Descriptive analysis
    desc_results = analyzer.descriptive_analysis(df)
    print("Descriptive Analysis:")
    print(f"Score mean: {desc_results['numeric_summary']['score']['mean']:.2f}")
    print(f"Score std: {desc_results['numeric_summary']['score']['std']:.2f}")
    
    # Correlation analysis
    corr_results = analyzer.correlation_analysis(df)
    print(f"\nSignificant correlations: {len(corr_results['significant_correlations'])}")
    
    # Regression analysis
    reg_results = analyzer.regression_analysis(df, 'score')
    print(f"\nRegression R²: {reg_results['test_metrics']['r2_score']:.3f}")
    
    # Generate report
    report = analyzer.generate_report(df, 'score')
    print(f"\n{report}")
    
    return analyzer
```

---

## 🔢 **Project 3: Mathematical Expression Parser - Implementation**

### **Detailed Implementation Plan**

#### **1. Expression Parser Implementation**

```python
# expression_parser.py
import re
import numpy as np
from typing import Dict, List, Tuple, Optional, Union
from dataclasses import dataclass
from enum import Enum
import sympy as sp
import json

class TokenType(Enum):
    NUMBER = "NUMBER"
    VARIABLE = "VARIABLE"
    OPERATOR = "OPERATOR"
    FUNCTION = "FUNCTION"
    LEFT_PAREN = "LEFT_PAREN"
    RIGHT_PAREN = "RIGHT_PAREN"
    COMMA = "COMMA"

@dataclass
class Token:
    type: TokenType
    value: str
    position: int

class ExpressionParser:
    """Mathematical expression parser with comprehensive error handling"""
    
    def __init__(self):
        self.operators = {
            '+': {'precedence': 1, 'associativity': 'left', 'function': np.add},
            '-': {'precedence': 1, 'associativity': 'left', 'function': np.subtract},
            '*': {'precedence': 2, 'associativity': 'left', 'function': np.multiply},
            '/': {'precedence': 2, 'associativity': 'left', 'function': np.divide},
            '^': {'precedence': 3, 'associativity': 'right', 'function': np.power},
            '**': {'precedence': 3, 'associativity': 'right', 'function': np.power},
            '%': {'precedence': 2, 'associativity': 'left', 'function': np.mod}
        }
        
        self.functions = {
            'sin': np.sin,
            'cos': np.cos,
            'tan': np.tan,
            'log': np.log,
            'ln': np.log,
            'exp': np.exp,
            'sqrt': np.sqrt,
            'abs': np.abs,
            'floor': np.floor,
            'ceil': np.ceil,
            'round': np.round
        }
        
        self.variables = {}
        self.errors = []
    
    def tokenize(self, expression: str) -> List[Token]:
        """Tokenize mathematical expression"""
        tokens = []
        i = 0
        n = len(expression)
        
        while i < n:
            # Skip whitespace
            if expression[i].isspace():
                i += 1
                continue
            
            # Numbers (integers and decimals)
            if expression[i].isdigit() or expression[i] == '.':
                start = i
                while i < n and (expression[i].isdigit() or expression[i] == '.'):
                    i += 1
                tokens.append(Token(TokenType.NUMBER, expression[start:i], start))
                continue
            
            # Variables (single letters or multi-letter starting with letter)
            if expression[i].isalpha():
                start = i
                while i < n and (expression[i].isalnum() or expression[i] == '_'):
                    i += 1
                value = expression[start:i]
                
                # Check if it's a function
                if value in self.functions:
                    tokens.append(Token(TokenType.FUNCTION, value, start))
                else:
                    tokens.append(Token(TokenType.VARIABLE, value, start))
                continue
            
            # Operators
            if expression[i] in '+-*/^%':
                # Check for ** operator
                if i + 1 < n and expression[i] == '*' and expression[i + 1] == '*':
                    tokens.append(Token(TokenType.OPERATOR, '**', i))
                    i += 2
                else:
                    tokens.append(Token(TokenType.OPERATOR, expression[i], i))
                    i += 1
                continue
            
            # Parentheses
            if expression[i] == '(':
                tokens.append(Token(TokenType.LEFT_PAREN, '(', i))
                i += 1
                continue
            elif expression[i] == ')':
                tokens.append(Token(TokenType.RIGHT_PAREN, ')', i))
                i += 1
                continue
            
            # Comma (for function arguments)
            if expression[i] == ',':
                tokens.append(Token(TokenType.COMMA, ',', i))
                i += 1
                continue
            
            # Unknown character
            self.errors.append(f"Unknown character '{expression[i]}' at position {i}")
            i += 1
        
        return tokens
    
    def shunting_yard(self, tokens: List[Token]) -> List[Token]:
        """Convert infix to postfix notation using Shunting Yard algorithm"""
        output = []
        operator_stack = []
        
        for token in tokens:
            if token.type == TokenType.NUMBER or token.type == TokenType.VARIABLE:
                output.append(token)
            elif token.type == TokenType.FUNCTION:
                operator_stack.append(token)
            elif token.type == TokenType.OPERATOR:
                while (operator_stack and 
                       operator_stack[-1].type == TokenType.OPERATOR and
                       ((self.operators[token.value]['precedence'] < self.operators[operator_stack[-1].value]['precedence']) or
                        (self.operators[token.value]['precedence'] == self.operators[operator_stack[-1].value]['precedence'] and
                         self.operators[token.value]['associativity'] == 'left'))):
                    output.append(operator_stack.pop())
                operator_stack.append(token)
            elif token.type == TokenType.LEFT_PAREN:
                operator_stack.append(token)
            elif token.type == TokenType.RIGHT_PAREN:
                while operator_stack and operator_stack[-1].type != TokenType.LEFT_PAREN:
                    output.append(operator_stack.pop())
                if not operator_stack:
                    self.errors.append("Mismatched parentheses")
                    return []
                operator_stack.pop()  # Remove left parenthesis
                
                # If function is on top of stack, pop it to output
                if operator_stack and operator_stack[-1].type == TokenType.FUNCTION:
                    output.append(operator_stack.pop())
            elif token.type == TokenType.COMMA:
                while operator_stack and operator_stack[-1].type != TokenType.LEFT_PAREN:
                    output.append(operator_stack.pop())
                if not operator_stack:
                    self.errors.append("Misplaced comma or mismatched parentheses")
                    return []
        
        # Pop remaining operators
        while operator_stack:
            if operator_stack[-1].type in [TokenType.LEFT_PAREN, TokenType.RIGHT_PAREN]:
                self.errors.append("Mismatched parentheses")
                return []
            output.append(operator_stack.pop())
        
        return output
    
    def evaluate_postfix(self, postfix_tokens: List[Token], variable_values: Dict[str, float] = None) -> Union[float, None]:
        """Evaluate postfix expression"""
        if variable_values is None:
            variable_values = {}
        
        stack = []
        
        for token in postfix_tokens:
            if token.type == TokenType.NUMBER:
                stack.append(float(token.value))
            elif token.type == TokenType.VARIABLE:
                if token.value in variable_values:
                    stack.append(float(variable_values[token.value]))
                elif token.value in self.variables:
                    stack.append(float(self.variables[token.value]))
                else:
                    self.errors.append(f"Unknown variable '{token.value}'")
                    return None
            elif token.type == TokenType.FUNCTION:
                if len(stack) < 1:
                    self.errors.append(f"Insufficient arguments for function '{token.value}'")
                    return None
                
                arg = stack.pop()
                try:
                    result = self.functions[token.value](arg)
                    stack.append(result)
                except Exception as e:
                    self.errors.append(f"Error in function '{token.value}': {str(e)}")
                    return None
            elif token.type == TokenType.OPERATOR:
                if len(stack) < 2:
                    self.errors.append(f"Insufficient operands for operator '{token.value}'")
                    return None
                
                right = stack.pop()
                left = stack.pop()
                
                try:
                    if token.value == '/' and right == 0:
                        self.errors.append("Division by zero")
                        return None
                    
                    result = self.operators[token.value]['function'](left, right)
                    stack.append(result)
                except Exception as e:
                    self.errors.append(f"Error in operation '{token.value}': {str(e)}")
                    return None
        
        if len(stack) != 1:
            self.errors.append("Invalid expression format")
            return None
        
        return stack[0]
    
    def parse_and_evaluate(self, expression: str, variable_values: Dict[str, float] = None) -> Dict:
        """Parse and evaluate expression with comprehensive error handling"""
        self.errors = []
        
        # Tokenize
        tokens = self.tokenize(expression)
        if self.errors:
            return {'error': self.errors, 'tokens': tokens}
        
        # Convert to postfix
        postfix_tokens = self.shunting_yard(tokens)
        if self.errors:
            return {'error': self.errors, 'postfix_tokens': postfix_tokens}
        
        # Evaluate
        result = self.evaluate_postfix(postfix_tokens, variable_values)
        if self.errors:
            return {'error': self.errors, 'postfix_tokens': postfix_tokens}
        
        return {
            'result': result,
            'tokens': tokens,
            'postfix_tokens': postfix_tokens,
            'expression': expression
        }
    
    def set_variable(self, name: str, value: float):
        """Set variable value"""
        self.variables[name] = value
    
    def get_variables(self) -> Dict[str, float]:
        """Get all variables"""
        return self.variables.copy()

# Usage example
def demo_expression_parser():
    """Demonstrate expression parser"""
    parser = ExpressionParser()
    
    # Test expressions
    test_expressions = [
        "2 + 3 * 4",
        "sin(0.5) + cos(0.5)",
        "x^2 + 2*x + 1",
        "log(10) + exp(1)",
        "(2 + 3) * (4 - 1)"
    ]
    
    # Set variables
    parser.set_variable('x', 2.5)
    parser.set_variable('pi', np.pi)
    
    for expr in test_expressions:
        print(f"\nExpression: {expr}")
        result = parser.parse_and_evaluate(expr)
        
        if 'error' in result:
            print(f"Error: {result['error']}")
        else:
            print(f"Result: {result['result']:.6f}")
            print(f"Tokens: {[t.value for t in result['tokens']]}")
    
    return parser
```

#### **2. Equation Solver Implementation**

```python
# equation_solver.py
import numpy as np
import sympy as sp
from typing import Dict, List, Tuple, Union, Optional
from dataclasses import dataclass
import json

@dataclass
class EquationSolution:
    equation: str
    solution: Union[float, List[float], None]
    method: str
    steps: List[str]
    error: Optional[str] = None

class EquationSolver:
    """Comprehensive equation solver with step-by-step solutions"""
    
    def __init__(self):
        self.solutions = []
        self.methods = {
            'linear': self.solve_linear_equation,
            'quadratic': self.solve_quadratic_equation,
            'system': self.solve_system_of_equations,
            'polynomial': self.solve_polynomial_equation
        }
    
    def solve_linear_equation(self, equation: str) -> EquationSolution:
        """Solve linear equation ax + b = c"""
        steps = []
        
        try:
            # Parse equation using sympy
            x = sp.symbols('x')
            
            # Handle different equation formats
            if '=' in equation:
                left, right = equation.split('=', 1)
                eq = sp.Eq(sp.parse_expr(left.strip()), sp.parse_expr(right.strip()))
            else:
                # Assume equation = 0
                eq = sp.Eq(sp.parse_expr(equation), 0)
            
            steps.append(f"Original equation: {eq}")
            
            # Solve
            solution = sp.solve(eq, x)
            
            if solution:
                sol_value = float(solution[0])
                steps.append(f"Solution: x = {sol_value}")
                
                # Verification
                verification = eq.subs(x, sol_value).simplify()
                steps.append(f"Verification: {verification} = {verification.evalf()}")
                
                return EquationSolution(
                    equation=equation,
                    solution=sol_value,
                    method="linear_algebraic",
                    steps=steps
                )
            else:
                return EquationSolution(
                    equation=equation,
                    solution=None,
                    method="linear_algebraic",
                    steps=steps + ["No solution found"],
                    error="No solution exists"
                )
                
        except Exception as e:
            return EquationSolution(
                equation=equation,
                solution=None,
                method="linear_algebraic",
                steps=[f"Error: {str(e)}"],
                error=str(e)
            )
    
    def solve_quadratic_equation(self, equation: str) -> EquationSolution:
        """Solve quadratic equation ax² + bx + c = 0"""
        steps = []
        
        try:
            x = sp.symbols('x')
            
            if '=' in equation:
                left, right = equation.split('=', 1)
                eq = sp.Eq(sp.parse_expr(left.strip()), sp.parse_expr(right.strip()))
            else:
                eq = sp.Eq(sp.parse_expr(equation), 0)
            
            steps.append(f"Original equation: {eq}")
            
            # Convert to standard form ax² + bx + c = 0
            expanded = sp.expand(eq.lhs - eq.rhs)
            coeffs = sp.Poly(expanded, x).all_coeffs()
            
            if len(coeffs) != 3:
                return EquationSolution(
                    equation=equation,
                    solution=None,
                    method="quadratic_formula",
                    steps=steps + ["Not a quadratic equation"],
                    error="Not a quadratic equation"
                )
            
            a, b, c = [float(coeff) for coeff in coeffs]
            steps.append(f"Standard form: {a}x² + {b}x + {c} = 0")
            
            # Calculate discriminant
            discriminant = b**2 - 4*a*c
            steps.append(f"Discriminant: Δ = b² - 4ac = {b}² - 4({a})({c}) = {discriminant}")
            
            if discriminant > 0:
                # Two real solutions
                x1 = (-b + np.sqrt(discriminant)) / (2*a)
                x2 = (-b - np.sqrt(discriminant)) / (2*a)
                steps.append(f"Two real solutions: x₁ = {x1:.4f}, x₂ = {x2:.4f}")
                solution = [x1, x2]
            elif discriminant == 0:
                # One real solution
                x1 = -b / (2*a)
                steps.append(f"One real solution: x = {x1:.4f}")
                solution = x1
            else:
                # Complex solutions
                real_part = -b / (2*a)
                imag_part = np.sqrt(-discriminant) / (2*a)
                steps.append(f"Complex solutions: x = {real_part:.4f} ± {imag_part:.4f}i")
                solution = [complex(real_part, imag_part), complex(real_part, -imag_part)]
            
            return EquationSolution(
                equation=equation,
                solution=solution,
                method="quadratic_formula",
                steps=steps
            )
            
        except Exception as e:
            return EquationSolution(
                equation=equation,
                solution=None,
                method="quadratic_formula",
                steps=[f"Error: {str(e)}"],
                error=str(e)
            )
    
    def solve_system_of_equations(self, equations: List[str]) -> EquationSolution:
        """Solve system of linear equations"""
        steps = []
        
        try:
            if len(equations) != 2:
                return EquationSolution(
                    equation=str(equations),
                    solution=None,
                    method="linear_system",
                    steps=["System must have exactly 2 equations"],
                    error="Invalid system size"
                )
            
            x, y = sp.symbols('x y')
            eqs = []
            
            for i, eq_str in enumerate(equations):
                if '=' in eq_str:
                    left, right = eq_str.split('=', 1)
                    eq = sp.Eq(sp.parse_expr(left.strip()), sp.parse_expr(right.strip()))
                else:
                    eq = sp.Eq(sp.parse_expr(eq_str), 0)
                eqs.append(eq)
                steps.append(f"Equation {i+1}: {eq}")
            
            # Solve system
            solution = sp.solve(eqs, (x, y))
            
            if solution:
                sol_dict = {str(var): float(val) for var, val in solution.items()}
                steps.append(f"Solution: {sol_dict}")
                
                # Verification
                for i, eq in enumerate(eqs):
                    verification = eq.subs(solution).simplify()
                    steps.append(f"Verification {i+1}: {verification} = {verification.evalf()}")
                
                return EquationSolution(
                    equation=str(equations),
                    solution=sol_dict,
                    method="linear_system",
                    steps=steps
                )
            else:
                return EquationSolution(
                    equation=str(equations),
                    solution=None,
                    method="linear_system",
                    steps=steps + ["No solution found"],
                    error="No solution exists"
                )
                
        except Exception as e:
            return EquationSolution(
                equation=str(equations),
                solution=None,
                method="linear_system",
                steps=[f"Error: {str(e)}"],
                error=str(e)
            )
    
    def solve_polynomial_equation(self, equation: str) -> EquationSolution:
        """Solve polynomial equation"""
        steps = []
        
        try:
            x = sp.symbols('x')
            
            if '=' in equation:
                left, right = equation.split('=', 1)
                eq = sp.Eq(sp.parse_expr(left.strip()), sp.parse_expr(right.strip()))
            else:
                eq = sp.Eq(sp.parse_expr(equation), 0)
            
            steps.append(f"Original equation: {eq}")
            
            # Convert to polynomial form
            polynomial = sp.expand(eq.lhs - eq.rhs)
            degree = sp.degree(polynomial, x)
            steps.append(f"Polynomial degree: {degree}")
            
            if degree > 4:
                steps.append("High-degree polynomial (degree > 4) - using numerical methods")
                # Use numerical methods
                coeffs = sp.Poly(polynomial, x).all_coeffs()
                roots = np.roots(coeffs)
                steps.append(f"Numerical roots: {roots}")
                solution = roots.tolist()
            else:
                # Use symbolic methods
                roots = sp.solve(polynomial, x)
                if roots:
                    solution = [float(root) if root.is_real else complex(root) for root in roots]
                    steps.append(f"Symbolic roots: {solution}")
                else:
                    solution = None
            
            return EquationSolution(
                equation=equation,
                solution=solution,
                method="polynomial_symbolic" if degree <= 4 else "polynomial_numerical",
                steps=steps
            )
            
        except Exception as e:
            return EquationSolution(
                equation=equation,
                solution=None,
                method="polynomial",
                steps=[f"Error: {str(e)}"],
                error=str(e)
            )
    
    def solve(self, equation: Union[str, List[str]], method: str = None) -> EquationSolution:
        """Main solve method"""
        if isinstance(equation, list):
            return self.solve_system_of_equations(equation)
        
        if method:
            if method in self.methods:
                return self.methods[method](equation)
            else:
                return EquationSolution(
                    equation=equation,
                    solution=None,
                    method="unknown",
                    steps=[f"Unknown method: {method}"],
                    error=f"Method {method} not supported"
                )
        
        # Auto-detect equation type
        x = sp.symbols('x')
        
        try:
            if '=' in equation:
                left, right = equation.split('=', 1)
                expr = sp.parse_expr(left.strip()) - sp.parse_expr(right.strip())
            else:
                expr = sp.parse_expr(equation)
            
            degree = sp.degree(expr, x)
            
            if degree == 1:
                return self.solve_linear_equation(equation)
            elif degree == 2:
                return self.solve_quadratic_equation(equation)
            else:
                return self.solve_polynomial_equation(equation)
                
        except:
            return EquationSolution(
                equation=equation,
                solution=None,
                method="auto_detection",
                steps=["Could not determine equation type"],
                error="Equation parsing failed"
            )
    
    def get_solution_history(self) -> List[EquationSolution]:
        """Get history of all solutions"""
        return self.solutions

# Usage example
def demo_equation_solver():
    """Demonstrate equation solver"""
    solver = EquationSolver()
    
    # Test equations
    test_equations = [
        ("2x + 3 = 7", "linear"),
        ("x^2 - 4 = 0", "quadratic"),
        ("x^2 + 3x + 2 = 0", "quadratic"),
        (["2x + y = 5", "x - y = 1"], "system"),
        ("x^3 - 2x^2 + x - 2 = 0", "polynomial")
    ]
    
    for equation, method in test_equations:
        print(f"\n{'='*50}")
        print(f"Equation: {equation}")
        print(f"Method: {method}")
        
        solution = solver.solve(equation, method)
        
        print("Steps:")
        for i, step in enumerate(solution.steps, 1):
            print(f"  {i}. {step}")
        
        if solution.error:
            print(f"Error: {solution.error}")
        else:
            print(f"Solution: {solution.solution}")
        
        solver.solutions.append(solution)
    
    return solver
```

#### **3. Web Interface Implementation**

```python
# web_interface.py
from flask import Flask, render_template, request, jsonify
import json
from expression_parser import ExpressionParser
from equation_solver import EquationSolver
import numpy as np

app = Flask(__name__)

# Initialize solvers
expr_parser = ExpressionParser()
eq_solver = EquationSolver()

@app.route('/')
def index():
    """Main calculator interface"""
    return render_template('calculator.html')

@app.route('/api/evaluate', methods=['POST'])
def evaluate_expression():
    """API endpoint for expression evaluation"""
    data = request.get_json()
    
    expression = data.get('expression', '')
    variables = data.get('variables', {})
    
    if not expression:
        return jsonify({'error': 'No expression provided'})
    
    # Parse and evaluate
    result = expr_parser.parse_and_evaluate(expression, variables)
    
    if 'error' in result:
        return jsonify({'error': result['error']})
    
    # Generate step-by-step explanation
    steps = generate_expression_steps(expression, result['tokens'])
    
    return jsonify({
        'result': result['result'],
        'steps': steps,
        'tokens': [t.value for t in result['tokens']],
        'variables': expr_parser.get_variables()
    })

@app.route('/api/solve', methods=['POST'])
def solve_equation():
    """API endpoint for equation solving"""
    data = request.get_json()
    
    equation = data.get('equation', '')
    method = data.get('method', None)
    
    if not equation:
        return jsonify({'error': 'No equation provided'})
    
    # Solve equation
    solution = eq_solver.solve(equation, method)
    
    return jsonify({
        'solution': solution.solution,
        'steps': solution.steps,
        'method': solution.method,
        'error': solution.error
    })

@app.route('/api/calculate', methods=['POST'])
def calculate_derivative():
    """API endpoint for derivative calculation"""
    data = request.get_json()
    
    expression = data.get('expression', '')
    variable = data.get('variable', 'x')
    
    if not expression:
        return jsonify({'error': 'No expression provided'})
    
    try:
        import sympy as sp
        x = sp.symbols(variable)
        expr = sp.parse_expr(expression)
        derivative = sp.diff(expr, x)
        
        return jsonify({
            'derivative': str(derivative),
            'simplified': str(sp.simplify(derivative)),
            'latex': sp.latex(derivative)
        })
    
    except Exception as e:
        return jsonify({'error': str(e)})

def generate_expression_steps(expression: str, tokens: list) -> list:
    """Generate step-by-step explanation for expression evaluation"""
    steps = []
    steps.append(f"Step 1: Parse expression '{expression}'")
    steps.append(f"Step 2: Tokenize into {len(tokens)} tokens: {[t.value for t in tokens]}")
    
    # Add more detailed steps based on expression complexity
    if any(t.type.value == 'FUNCTION' for t in tokens):
        steps.append("Step 3: Identify mathematical functions")
    
    if any(t.value in ['^', '**'] for t in tokens):
        steps.append("Step 4: Handle exponentiation operations")
    
    steps.append("Step 5: Apply order of operations (PEMDAS)")
    steps.append("Step 6: Compute final result")
    
    return steps

if __name__ == '__main__':
    app.run(debug=True, host='0.0.0.0', port=5000)

# HTML template (templates/calculator.html)
HTML_TEMPLATE = """
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mathematical Expression Calculator</title>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
            background-color: #f5f5f5;
        }
        .container {
            background: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
        }
        .calculator {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 20px;
        }
        .input-section, .result-section {
            padding: 20px;
            border: 1px solid #ddd;
            border-radius: 8px;
        }
        .input-group {
            margin-bottom: 15px;
        }
        label {
            display: block;
            margin-bottom: 5px;
            font-weight: bold;
        }
        input, textarea, select {
            width: 100%;
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: 4px;
            font-size: 16px;
        }
        button {
            background-color: #007bff;
            color: white;
            padding: 12px 24px;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            font-size: 16px;
            margin-right: 10px;
        }
        button:hover {
            background-color: #0056b3;
        }
        .result {
            background-color: #f8f9fa;
            padding: 15px;
            border-radius: 4px;
            margin-top: 15px;
        }
        .steps {
            background-color: #e9ecef;
            padding: 15px;
            border-radius: 4px;
            margin-top: 15px;
        }
        .error {
            background-color: #f8d7da;
            color: #721c24;
            padding: 15px;
            border-radius: 4px;
            margin-top: 15px;
        }
        .examples {
            background-color: #d1ecf1;
            padding: 15px;
            border-radius: 4px;
            margin-top: 15px;
        }
        .example {
            cursor: pointer;
            color: #007bff;
            text-decoration: underline;
            margin-right: 15px;
        }
        .example:hover {
            color: #0056b3;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Mathematical Expression Calculator</h1>
        
        <div class="calculator">
            <div class="input-section">
                <h2>Expression Evaluator</h2>
                
                <div class="input-group">
                    <label for="expression">Mathematical Expression:</label>
                    <input type="text" id="expression" placeholder="e.g., 2*x + 3*sin(x)">
                </div>
                
                <div class="input-group">
                    <label for="variables">Variables (JSON format):</label>
                    <input type="text" id="variables" placeholder='{"x": 2.5, "y": 1.0}'>
                </div>
                
                <button onclick="evaluateExpression()">Evaluate</button>
                <button onclick="clearResults()">Clear</button>
                
                <div class="examples">
                    <strong>Examples:</strong><br>
                    <span class="example" onclick="setExample('2 + 3 * 4')">2 + 3 * 4</span>
                    <span class="example" onclick="setExample('sin(0.5) + cos(0.5)')">sin(0.5) + cos(0.5)</span>
                    <span class="example" onclick="setExample('x^2 + 2*x + 1')">x^2 + 2*x + 1</span>
                </div>
            </div>
            
            <div class="result-section">
                <h2>Results</h2>
                <div id="result"></div>
                <div id="steps"></div>
                <div id="error"></div>
            </div>
        </div>
        
        <div class="calculator" style="margin-top: 20px;">
            <div class="input-section">
                <h2>Equation Solver</h2>
                
                <div class="input-group">
                    <label for="equation">Equation:</label>
                    <input type="text" id="equation" placeholder="e.g., 2*x + 3 = 7">
                </div>
                
                <div class="input-group">
                    <label for="method">Method:</label>
                    <select id="method">
                        <option value="">Auto-detect</option>
                        <option value="linear">Linear</option>
                        <option value="quadratic">Quadratic</option>
                        <option value="polynomial">Polynomial</option>
                    </select>
                </div>
                
                <button onclick="solveEquation()">Solve</button>
                
                <div class="examples">
                    <strong>Examples:</strong><br>
                    <span class="example" onclick="setEquation('2*x + 3 = 7')">2*x + 3 = 7</span>
                    <span class="example" onclick="setEquation('x^2 - 4 = 0')">x^2 - 4 = 0</span>
                    <span class="example" onclick="setEquation('x^2 + 3*x + 2 = 0')">x^2 + 3*x + 2 = 0</span>
                </div>
            </div>
            
            <div class="result-section">
                <h2>Solution</h2>
                <div id="solution"></div>
                <div id="solution-steps"></div>
                <div id="solution-error"></div>
            </div>
        </div>
    </div>

    <script>
        async function evaluateExpression() {
            const expression = document.getElementById('expression').value;
            const variablesStr = document.getElementById('variables').value;
            
            let variables = {};
            if (variablesStr) {
                try {
                    variables = JSON.parse(variablesStr);
                } catch (e) {
                    showError('Invalid JSON format for variables');
                    return;
                }
            }
            
            try {
                const response = await fetch('/api/evaluate', {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json',
                    },
                    body: JSON.stringify({
                        expression: expression,
                        variables: variables
                    })
                });
                
                const data = await response.json();
                
                if (data.error) {
                    showError(data.error);
                } else {
                    showResult(data.result, data.steps, data.tokens);
                }
            } catch (error) {
                showError('Network error: ' + error.message);
            }
        }
        
        async function solveEquation() {
            const equation = document.getElementById('equation').value;
            const method = document.getElementById('method').value;
            
            try {
                const response = await fetch('/api/solve', {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json',
                    },
                    body: JSON.stringify({
                        equation: equation,
                        method: method
                    })
                });
                
                const data = await response.json();
                
                if (data.error) {
                    showSolutionError(data.error);
                } else {
                    showSolution(data.solution, data.steps, data.method);
                }
            } catch (error) {
                showSolutionError('Network error: ' + error.message);
            }
        }
        
        function showResult(result, steps, tokens) {
            document.getElementById('result').innerHTML = `
                <div class="result">
                    <h3>Result: ${result}</h3>
                    <p><strong>Tokens:</strong> ${tokens.join(', ')}</p>
                </div>
            `;
            
            document.getElementById('steps').innerHTML = `
                <div class="steps">
                    <h3>Steps:</h3>
                    <ol>
                        ${steps.map(step => `<li>${step}</li>`).join('')}
                    </ol>
                </div>
            `;
            
            document.getElementById('error').innerHTML = '';
        }
        
        function showSolution(solution, steps, method) {
            document.getElementById('solution').innerHTML = `
                <div class="result">
                    <h3>Solution (${method}): ${solution}</h3>
                </div>
            `;
            
            document.getElementById('solution-steps').innerHTML = `
                <div class="steps">
                    <h3>Steps:</h3>
                    <ol>
                        ${steps.map(step => `<li>${step}</li>`).join('')}
                    </ol>
                </div>
            `;
            
            document.getElementById('solution-error').innerHTML = '';
        }
        
        function showError(message) {
            document.getElementById('error').innerHTML = `
                <div class="error">
                    <h3>Error:</h3>
                    <p>${message}</p>
                </div>
            `;
            
            document.getElementById('result').innerHTML = '';
            document.getElementById('steps').innerHTML = '';
        }
        
        function showSolutionError(message) {
            document.getElementById('solution-error').innerHTML = `
                <div class="error">
                    <h3>Error:</h3>
                    <p>${message}</p>
                </div>
            `;
            
            document.getElementById('solution').innerHTML = '';
            document.getElementById('solution-steps').innerHTML = '';
        }
        
        function clearResults() {
            document.getElementById('result').innerHTML = '';
            document.getElementById('steps').innerHTML = '';
            document.getElementById('error').innerHTML = '';
            document.getElementById('solution').innerHTML = '';
            document.getElementById('solution-steps').innerHTML = '';
            document.getElementById('solution-error').innerHTML = '';
        }
        
        function setExample(expr) {
            document.getElementById('expression').value = expr;
        }
        
        function setEquation(eq) {
            document.getElementById('equation').value = eq;
        }
    </script>
</body>
</html>
"""

# Save HTML template
def save_html_template():
    """Save HTML template to file"""
    with open('templates/calculator.html', 'w') as f:
        f.write(HTML_TEMPLATE)
    print("HTML template saved to templates/calculator.html")

# Usage example
def demo_web_interface():
    """Demonstrate web interface"""
    save_html_template()
    print("Web interface ready! Run 'python web_interface.py' to start the server.")
    print("Open http://localhost:5000 in your browser to use the calculator.")

if __name__ == '__main__':
    demo_web_interface()
```

---

## 🤖 **Project 4: Attention Mechanism Visualization - Implementation**

### **Detailed Implementation Plan**

#### **1. Attention Mechanism Implementation**

```python
# attention_mechanism.py
import numpy as np
import torch
import torch.nn as nn
import torch.nn.functional as F
import matplotlib.pyplot as plt
import seaborn as sns
from typing import Tuple, Optional
import plotly.graph_objects as go
import plotly.express as px

class ScaledDotProductAttention(nn.Module):
    """Scaled Dot-Product Attention implementation"""
    
    def __init__(self, d_k: int):
        super().__init__()
        self.d_k = d_k
        self.scale = d_k ** -0.5
    
    def forward(self, Q: torch.Tensor, K: torch.Tensor, V: torch.Tensor, 
                mask: Optional[torch.Tensor] = None) -> Tuple[torch.Tensor, torch.Tensor]:
        """
        Args:
            Q: Query tensor [batch_size, seq_len, d_k]
            K: Key tensor [batch_size, seq_len, d_k]
            V: Value tensor [batch_size, seq_len, d_v]
            mask: Optional mask [batch_size, seq_len, seq_len]
        
        Returns:
            output: [batch_size, seq_len, d_v]
            attention_weights: [batch_size, seq_len, seq_len]
        """
        # Compute attention scores
        scores = torch.matmul(Q, K.transpose(-2, -1)) * self.scale
        
        # Apply mask if provided
        if mask is not None:
            scores = scores.masked_fill(mask == 0, -1e9)
        
        # Apply softmax
        attention_weights = F.softmax(scores, dim=-1)
        
        # Apply attention to values
        output = torch.matmul(attention_weights, V)
        
        return output, attention_weights

class MultiHeadAttention(nn.Module):
    """Multi-Head Attention implementation"""
    
    def __init__(self, d_model: int, num_heads: int, dropout: float = 0.1):
        super().__init__()
        assert d_model % num_heads == 0, "d_model must be divisible by num_heads"
        
        self.d_model = d_model
        self.num_heads = num_heads
        self.d_k = d_model // num_heads
        self.d_v = d_model // num_heads
        
        self.w_q = nn.Linear(d_model, d_model, bias=False)
        self.w_k = nn.Linear(d_model, d_model, bias=False)
        self.w_v = nn.Linear(d_model, d_model, bias=False)
        self.w_o = nn.Linear(d_model, d_model, bias=False)
        
        self.attention = ScaledDotProductAttention(self.d_k)
        self.dropout = nn.Dropout(dropout)
        self.layer_norm = nn.LayerNorm(d_model)
        
    def forward(self, query: torch.Tensor, key: torch.Tensor, value: torch.Tensor,
                mask: Optional[torch.Tensor] = None) -> Tuple[torch.Tensor, torch.Tensor]:
        batch_size, seq_len, _ = query.size()
        
        # Linear projections
        Q = self.w_q(query).view(batch_size, seq_len, self.num_heads, self.d_k).transpose(1, 2)
        K = self.w_k(key).view(batch_size, seq_len, self.num_heads, self.d_k).transpose(1, 2)
        V = self.w_v(value).view(batch_size, seq_len, self.num_heads, self.d_v).transpose(1, 2)
        
        # Apply attention
        attention_output, attention_weights = self.attention(Q, K, V, mask)
        
        # Concatenate heads
        attention_output = attention_output.transpose(1, 2).contiguous().view(
            batch_size, seq_len, self.d_model
        )
        
        # Final linear projection and dropout
        output = self.dropout(self.w_o(attention_output))
        
        # Residual connection and layer norm
        output = self.layer_norm(output + query)
        
        return output, attention_weights

class PositionalEncoding(nn.Module):
    """Positional Encoding for Transformer"""
    
    def __init__(self, d_model: int, max_len: int = 5000, dropout: float = 0.1):
        super().__init__()
        self.dropout = nn.Dropout(dropout)
        
        pe = torch.zeros(max_len, d_model)
        position = torch.arange(0, max_len, dtype=torch.float).unsqueeze(1)
        
        div_term = torch.exp(torch.arange(0, d_model, 2).float() * 
                           -(np.log(10000.0) / d_model))
        
        pe[:, 0::2] = torch.sin(position * div_term)
        pe[:, 1::2] = torch.cos(position * div_term)
        
        pe = pe.unsqueeze(0).transpose(0, 1)
        self.register_buffer('pe', pe)
    
    def forward(self, x: torch.Tensor) -> torch.Tensor:
        x = x + self.pe[:x.size(0), :]
        return self.dropout(x)

class TransformerEncoderLayer(nn.Module):
    """Single Transformer Encoder Layer"""
    
    def __init__(self, d_model: int, num_heads: int, d_ff: int, dropout: float = 0.1):
        super().__init__()
        
        self.self_attention = MultiHeadAttention(d_model, num_heads, dropout)
        self.feed_forward = nn.Sequential(
            nn.Linear(d_model, d_ff),
            nn.ReLU(),
            nn.Dropout(dropout),
            nn.Linear(d_ff, d_model),
            nn.Dropout(dropout)
        )
        self.layer_norm1 = nn.LayerNorm(d_model)
        self.layer_norm2 = nn.LayerNorm(d_model)
        self.dropout = nn.Dropout(dropout)
    
    def forward(self, x: torch.Tensor, mask: Optional[torch.Tensor] = None) -> Tuple[torch.Tensor, torch.Tensor]:
        # Self-attention
        attn_output, attention_weights = self.self_attention(x, x, x, mask)
        x = self.layer_norm1(x + self.dropout(attn_output))
        
        # Feed-forward
        ff_output = self.feed_forward(x)
        x = self.layer_norm2(x + self.dropout(ff_output))
        
        return x, attention_weights

# Usage example and testing
def test_attention_mechanism():
    """Test attention mechanism implementation"""
    # Parameters
    batch_size = 2
    seq_len = 10
    d_model = 512
    num_heads = 8
    
    # Create sample input
    x = torch.randn(batch_size, seq_len, d_model)
    
    # Test multi-head attention
    mha = MultiHeadAttention(d_model, num_heads)
    output, attention_weights = mha(x, x, x)
    
    print(f"Input shape: {x.shape}")
    print(f"Output shape: {output.shape}")
    print(f"Attention weights shape: {attention_weights.shape}")
    
    # Test positional encoding
    pos_encoding = PositionalEncoding(d_model)
    x_with_pos = pos_encoding(x)
    print(f"Positional encoded shape: {x_with_pos.shape}")
    
    # Test transformer encoder layer
    encoder_layer = TransformerEncoderLayer(d_model, num_heads, d_ff=2048)
    encoder_output, attn_weights = encoder_layer(x_with_pos)
    print(f"Encoder output shape: {encoder_output.shape}")
    
    return mha, encoder_layer

# Compare with PyTorch implementation
def compare_with_pytorch():
    """Compare our implementation with PyTorch's built-in"""
    batch_size = 2
    seq_len = 10
    d_model = 512
    num_heads = 8
    
    # Our implementation
    our_mha = MultiHeadAttention(d_model, num_heads)
    
    # PyTorch implementation
    pytorch_mha = nn.MultiheadAttention(d_model, num_heads, batch_first=True)
    
    # Create sample input
    x = torch.randn(batch_size, seq_len, d_model)
    
    # Our implementation
    our_output, our_weights = our_mha(x, x, x)
    
    # PyTorch implementation
    pytorch_output, pytorch_weights = pytorch_mha(x, x, x)
    
    # Compare shapes
    print(f"Our output shape: {our_output.shape}")
    print(f"PyTorch output shape: {pytorch_output.shape}")
    print(f"Our attention weights shape: {our_weights.shape}")
    print(f"PyTorch attention weights shape: {pytorch_weights.shape}")
    
    # Numerical comparison (should be similar but not identical due to different implementations)
    diff = torch.abs(our_output - pytorch_output).mean()
    print(f"Mean absolute difference: {diff.item():.6f}")
    
    return our_mha, pytorch_mha
```

#### **2. Visualization Tools**

```python
# attention_visualization.py
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
import plotly.graph_objects as go
import plotly.express as px
from plotly.subplots import make_subplots
import torch
from typing import List, Tuple, Optional
import pandas as pd

class AttentionVisualizer:
    """Comprehensive attention visualization tools"""
    
    def __init__(self):
        plt.style.use('seaborn-v0_8')
        sns.set_palette("husl")
    
    def plot_attention_heatmap(self, attention_weights: np.ndarray, 
                            tokens: List[str],
                            title: str = "Attention Weights",
                            figsize: Tuple[int, int] = (10, 8)) -> plt.Figure:
        """Create attention heatmap visualization"""
        fig, ax = plt.subplots(figsize=figsize)
        
        # Create heatmap
        sns.heatmap(attention_weights, 
                   xticklabels=tokens, 
                   yticklabels=tokens,
                   annot=True, 
                   fmt='.3f',
                   cmap='Blues',
                   ax=ax,
                   cbar_kws={'label': 'Attention Weight'})
        
        ax.set_title(title, fontsize=14, fontweight='bold')
        ax.set_xlabel('Key Tokens', fontsize=12)
        ax.set_ylabel('Query Tokens', fontsize=12)
        
        # Rotate labels for better readability
        plt.setp(ax.get_xticklabels(), rotation=45, ha='right')
        plt.setp(ax.get_yticklabels(), rotation=0)
        
        plt.tight_layout()
        return fig
    
    def plot_multi_head_attention(self, attention_weights: np.ndarray,
                                tokens: List[str],
                                num_heads: int,
                                title: str = "Multi-Head Attention") -> plt.Figure:
        """Visualize multi-head attention"""
        fig, axes = plt.subplots(2, 4, figsize=(20, 10))
        axes = axes.flatten()
        
        for head in range(num_heads):
            ax = axes[head]
            head_weights = attention_weights[head] if attention_weights.ndim == 3 else attention_weights
            
            sns.heatmap(head_weights,
                       xticklabels=tokens,
                       yticklabels=tokens,
                       annot=True,
                       fmt='.2f',
                       cmap='Blues',
                       ax=ax,
                       cbar_kws={'label': 'Attention Weight'} if head == 0 else {})
            
            ax.set_title(f'Head {head + 1}', fontsize=12, fontweight='bold')
            
            # Only show labels on first row and last column
            if head >= 4:
                ax.set_xlabel('Key Tokens', fontsize=10)
            else:
                ax.set_xlabel('')
            
            if head % 4 == 0:
                ax.set_ylabel('Query Tokens', fontsize=10)
            else:
                ax.set_ylabel('')
            
            plt.setp(ax.get_xticklabels(), rotation=45, ha='right')
            plt.setp(ax.get_yticklabels(), rotation=0)
        
        fig.suptitle(title, fontsize=16, fontweight='bold')
        plt.tight_layout()
        return fig
    
    def interactive_attention_plot(self, attention_weights: np.ndarray,
                               tokens: List[str],
                               title: str = "Interactive Attention Visualization") -> go.Figure:
        """Create interactive attention visualization using Plotly"""
        fig = go.Figure(data=go.Heatmap(
            z=attention_weights,
            x=tokens,
            y=tokens,
            colorscale='Blues',
            showscale=True,
            hoverongaps=False,
            hovertemplate='Query: %{y}<br>Key: %{x}<br>Attention: %{z:.4f}<extra></extra>'
        ))
        
        fig.update_layout(
            title=title,
            xaxis_title='Key Tokens',
            yaxis_title='Query Tokens',
            width=800,
            height=600,
            font=dict(size=12)
        )
        
        return fig
    
    def plot_attention_patterns(self, attention_weights_list: List[np.ndarray],
                              tokens_list: List[List[str]],
                              titles: List[str],
                              figsize: Tuple[int, int] = (15, 5)) -> plt.Figure:
        """Compare attention patterns across different examples"""
        n_examples = len(attention_weights_list)
        fig, axes = plt.subplots(1, n_examples, figsize=figsize)
        
        if n_examples == 1:
            axes = [axes]
        
        for i, (weights, tokens, title) in enumerate(zip(attention_weights_list, tokens_list, titles)):
            sns.heatmap(weights,
                       xticklabels=tokens,
                       yticklabels=tokens,
                       annot=True,
                       fmt='.3f',
                       cmap='Blues',
                       ax=axes[i],
                       cbar_kws={'label': 'Attention Weight'} if i == n_examples - 1 else {})
            
            axes[i].set_title(title, fontsize=12, fontweight='bold')
            axes[i].set_xlabel('Key Tokens', fontsize=10)
            axes[i].set_ylabel('Query Tokens', fontsize=10)
            
            plt.setp(axes[i].get_xticklabels(), rotation=45, ha='right')
            plt.setp(axes[i].get_yticklabels(), rotation=0)
        
        plt.tight_layout()
        return fig
    
    def plot_attention_statistics(self, attention_weights: np.ndarray,
                                tokens: List[str]) -> plt.Figure:
        """Plot attention statistics and distributions"""
        fig, axes = plt.subplots(2, 2, figsize=(15, 10))
        
        # Flatten attention weights for statistics
        flat_weights = attention_weights.flatten()
        
        # 1. Histogram of attention weights
        axes[0, 0].hist(flat_weights, bins=50, alpha=0.7, edgecolor='black')
        axes[0, 0].set_title('Distribution of Attention Weights')
        axes[0, 0].set_xlabel('Attention Weight')
        axes[0, 0].set_ylabel('Frequency')
        axes[0, 0].grid(True, alpha=0.3)
        
        # 2. Average attention per token
        avg_attention = np.mean(attention_weights, axis=0)
        axes[0, 1].bar(range(len(tokens)), avg_attention)
        axes[0, 1].set_title('Average Attention per Key Token')
        axes[0, 1].set_xlabel('Token Index')
        axes[0, 1].set_ylabel('Average Attention')
        axes[0, 1].set_xticks(range(len(tokens)))
        axes[0, 1].set_xticklabels(tokens, rotation=45, ha='right')
        axes[0, 1].grid(True, alpha=0.3)
        
        # 3. Maximum attention per token
        max_attention = np.max(attention_weights, axis=0)
        axes[1, 0].bar(range(len(tokens)), max_attention, color='red', alpha=0.7)
        axes[1, 0].set_title('Maximum Attention per Key Token')
        axes[1, 0].set_xlabel('Token Index')
        axes[1, 0].set_ylabel('Maximum Attention')
        axes[1, 0].set_xticks(range(len(tokens)))
        axes[1, 0].set_xticklabels(tokens, rotation=45, ha='right')
        axes[1, 0].grid(True, alpha=0.3)
        
        # 4. Attention entropy per query
        attention_entropy = []
        for i in range(attention_weights.shape[0]):
            row = attention_weights[i]
            entropy = -np.sum(row * np.log(row + 1e-10))
            attention_entropy.append(entropy)
        
        axes[1, 1].plot(range(len(tokens)), attention_entropy, marker='o')
        axes[1, 1].set_title('Attention Entropy per Query Token')
        axes[1, 1].set_xlabel('Query Token Index')
        axes[1, 1].set_ylabel('Entropy')
        axes[1, 1].grid(True, alpha=0.3)
        
        plt.tight_layout()
        return fig
    
    def create_attention_animation(self, attention_weights_sequence: List[np.ndarray],
                                  tokens: List[str],
                                  title: str = "Attention Evolution") -> go.Figure:
        """Create animated attention visualization"""
        frames = []
        
        for i, weights in enumerate(attention_weights_sequence):
            frame = go.Frame(
                data=[go.Heatmap(
                    z=weights,
                    x=tokens,
                    y=tokens,
                    colorscale='Blues',
                    showscale=True if i == 0 else False,
                    hoverongaps=False
                )],
                name=f'Step {i+1}'
            )
            frames.append(frame)
        
        # Create initial plot
        fig = go.Figure(
            data=[go.Heatmap(
                z=attention_weights_sequence[0],
                x=tokens,
                y=tokens,
                colorscale='Blues',
                showscale=True
            )],
            frames=frames
        )
        
        # Add animation controls
        fig.update_layout(
            title=title,
            xaxis_title='Key Tokens',
            yaxis_title='Query Tokens',
            width=800,
            height=600,
            updatemenus=[
                dict(
                    type="buttons",
                    buttons=[
                        dict(label="Play", method="animate", args=[None, {"frame": {"duration": 500}, "fromcurrent": True}]),
                        dict(label="Pause", method="animate", args=[[None], {"frame": {"duration": 0}, "mode": "immediate", "transition": {"duration": 0}}]),
                    ],
                    direction="left",
                    pad={"r": 10, "t": 87},
                    showactive=False,
                    x=0.011,
                    xanchor="right",
                    y=0,
                    yanchor="top"
                ),
            ],
            sliders=[
                dict(
                    active=0,
                    yanchor="top",
                    xanchor="left",
                    currentvalue={"font": {"size": 20}, "prefix": "Step: "},
                    transition={"duration": 300, "easing": "cubic-in-out"},
                    pad={"b": 10, "t": 50},
                    len=0.9,
                    x=0.1,
                    y=0,
                    steps=[
                        dict(
                            args=[[f"Step {i+1}"], {"frame": {"duration": 0, "redraw": True}}],
                            label=f"Step {i+1}",
                            method="animate"
                        )
                        for i in range(len(attention_weights_sequence))
                    ]
                )
            ]
        )
        
        return fig

# Demo functions
def demo_attention_visualization():
    """Demonstrate attention visualization capabilities"""
    visualizer = AttentionVisualizer()
    
    # Create sample attention weights
    np.random.seed(42)
    tokens = ["The", "cat", "sat", "on", "the", "mat"]
    attention_weights = np.random.rand(len(tokens), len(tokens))
    
    # Normalize rows
    attention_weights = attention_weights / attention_weights.sum(axis=1, keepdims=True)
    
    # Create visualizations
    fig1 = visualizer.plot_attention_heatmap(attention_weights, tokens, "Sample Attention Weights")
    fig2 = visualizer.plot_attention_statistics(attention_weights, tokens)
    
    # Interactive plot
    fig3 = visualizer.interactive_attention_plot(attention_weights, tokens, "Interactive Attention")
    
    plt.show()
    
    return visualizer, fig1, fig2, fig3

def demo_multi_head_visualization():
    """Demonstrate multi-head attention visualization"""
    visualizer = AttentionVisualizer()
    
    # Create sample multi-head attention weights
    np.random.seed(42)
    tokens = ["Hello", "world", "how", "are", "you"]
    num_heads = 8
    multi_head_weights = np.random.rand(num_heads, len(tokens), len(tokens))
    
    # Normalize each head
    for head in range(num_heads):
        multi_head_weights[head] = multi_head_weights[head] / multi_head_weights[head].sum(axis=1, keepdims=True)
    
    # Create visualization
    fig = visualizer.plot_multi_head_attention(multi_head_weights, tokens, num_heads, "Multi-Head Attention Pattern")
    
    plt.show()
    
    return visualizer, fig
```

#### **3. Demo Application**

```python
# demo_app.py
import streamlit as st
import numpy as np
import torch
import plotly.graph_objects as go
from attention_mechanism import MultiHeadAttention, TransformerEncoderLayer
from attention_visualization import AttentionVisualizer
import seaborn as sns

class AttentionDemoApp:
    """Streamlit demo application for attention mechanisms"""
    
    def __init__(self):
        self.visualizer = AttentionVisualizer()
        self.models = {}
    
    def run(self):
        """Run the Streamlit app"""
        st.set_page_config(
            page_title="Attention Mechanism Demo",
            page_icon="🤖",
            layout="wide"
        )
        
        st.title("🤖 Attention Mechanism Visualization Demo")
        st.markdown("""
        This interactive demo showcases how attention mechanisms work in transformer models.
        Explore different attention patterns and see how the model "focuses" on different parts of the input.
        """)
        
        # Sidebar for configuration
        st.sidebar.header("Configuration")
        
        # Model parameters
        seq_len = st.sidebar.slider("Sequence Length", min_value=4, max_value=20, value=8)
        d_model = st.sidebar.selectbox("Model Dimension", [128, 256, 512], index=1)
        num_heads = st.sidebar.selectbox("Number of Heads", [2, 4, 8], index=1)
        
        # Input options
        st.sidebar.header("Input Options")
        input_type = st.sidebar.radio("Input Type", ["Random", "Custom Text", "Predefined Examples"])
        
        # Generate or get input
        if input_type == "Random":
            tokens = [f"token_{i}" for i in range(seq_len)]
            x = torch.randn(1, seq_len, d_model)
        elif input_type == "Custom Text":
            text = st.sidebar.text_input("Enter text (tokens separated by spaces)", 
                                       "The quick brown fox jumps over the lazy dog")
            tokens = text.split()[:seq_len]
            # Pad or truncate to seq_len
            while len(tokens) < seq_len:
                tokens.append("<pad>")
            tokens = tokens[:seq_len]
            x = torch.randn(1, seq_len, d_model)
        else:
            examples = {
                "Short sentence": ["The", "cat", "sat", "on", "the", "mat"],
                "Question": ["What", "is", "the", "meaning", "of", "life"],
                "Math problem": ["Two", "plus", "two", "equals", "four"]
            }
            example = st.sidebar.selectbox("Choose example", list(examples.keys()))
            tokens = examples[example]
            x = torch.randn(1, len(tokens), d_model)
        
        # Create or get model
        model_key = f"{d_model}_{num_heads}"
        if model_key not in self.models:
            self.models[model_key] = MultiHeadAttention(d_model, num_heads)
        
        model = self.models[model_key]
        
        # Forward pass
        with torch.no_grad():
            output, attention_weights = model(x, x, x)
        
        # Convert to numpy for visualization
        attention_np = attention_weights[0].cpu().numpy()  # Remove batch dimension
        
        # Main visualization area
        col1, col2 = st.columns([2, 1])
        
        with col1:
            st.subheader("Attention Heatmap")
            
            # Create interactive heatmap
            fig = go.Figure(data=go.Heatmap(
                z=attention_np,
                x=tokens,
                y=tokens,
                colorscale='Blues',
                showscale=True,
                hoverongaps=False,
                hovertemplate='Query: %{y}<br>Key: %{x}<br>Attention: %{z:.4f}<extra></extra>'
            ))
            
            fig.update_layout(
                title="Attention Weights Visualization",
                xaxis_title="Key Tokens",
                yaxis_title="Query Tokens",
                width=700,
                height=500
            )
            
            st.plotly_chart(fig, use_container_width=True)
        
        with col2:
            st.subheader("Statistics")
            
            # Calculate statistics
            flat_weights = attention_np.flatten()
            avg_attention = np.mean(attention_np, axis=0)
            max_attention = np.max(attention_np, axis=0)
            
            st.metric("Mean Attention", f"{np.mean(flat_weights):.4f}")
            st.metric("Max Attention", f"{np.max(flat_weights):.4f}")
            st.metric("Min Attention", f"{np.min(flat_weights):.4f}")
            
            st.subheader("Token Analysis")
            token_data = []
            for i, token in enumerate(tokens):
                token_data.append({
                    'Token': token,
                    'Avg Attention': avg_attention[i],
                    'Max Attention': max_attention[i]
                })
            
            st.dataframe(token_data, use_container_width=True)
        
        # Multi-head visualization
        if num_heads > 1:
            st.subheader("Multi-Head Attention Patterns")
            
            # Get multi-head weights (reshape from our implementation)
            multi_head_weights = attention_weights.cpu().numpy()  # [batch, heads, seq, seq]
            multi_head_weights = multi_head_weights[0]  # Remove batch dimension
            
            # Create subplots
            cols = st.columns(min(num_heads, 4))
            for head in range(num_heads):
                with cols[head % 4]:
                    fig_head = go.Figure(data=go.Heatmap(
                        z=multi_head_weights[head],
                        x=tokens,
                        y=tokens,
                        colorscale='Blues',
                        showscale=False
                    ))
                    
                    fig_head.update_layout(
                        title=f"Head {head + 1}",
                        width=300,
                        height=250,
                        margin=dict(l=20, r=20, t=40, b=20)
                    )
                    
                    st.plotly_chart(fig_head, use_container_width=True)
        
        # Analysis section
        st.subheader("Attention Analysis")
        
        col1, col2 = st.columns(2)
        
        with col1:
            # Attention distribution
            fig_dist = go.Figure(data=[go.Histogram(
                x=flat_weights,
                nbinsx=30,
                name="Attention Distribution"
            )])
            
            fig_dist.update_layout(
                title="Distribution of Attention Weights",
                xaxis_title="Attention Weight",
                yaxis_title="Frequency",
                width=500,
                height=300
            )
            
            st.plotly_chart(fig_dist, use_container_width=True)
        
        with col2:
            # Token importance
            fig_imp = go.Figure(data=[go.Bar(
                x=tokens,
                y=avg_attention,
                name="Average Attention"
            )])
            
            fig_imp.update_layout(
                title="Average Attention per Token",
                xaxis_title="Tokens",
                yaxis_title="Average Attention",
                width=500,
                height=300
            )
            
            st.plotly_chart(fig_imp, use_container_width=True)
        
        # Explanation section
        st.subheader("How to Interpret This Visualization")
        
        st.markdown("""
        ### Understanding the Heatmap
        - **Rows (Query Tokens)**: Which token is "looking" for information
        - **Columns (Key Tokens)**: Which token is being "looked at"
        - **Color Intensity**: How much attention is paid (darker = more attention)
        
        ### Key Insights
        - **Self-Attention**: Tokens often attend to themselves (diagonal)
        - **Context**: Tokens attend to related words in the sequence
        - **Patterns**: Different heads learn different attention patterns
        
        ### Multi-Head Attention
        - Each head learns to focus on different aspects
        - Some heads may focus on syntactic relationships
        - Others may focus on semantic relationships
        """)
        
        # Performance metrics
        st.subheader("Performance Metrics")
        
        col1, col2, col3 = st.columns(3)
        
        with col1:
            st.metric("Model Parameters", f"{d_model * d_model * num_heads * 4:,}")
        
        with col2:
            st.metric("Sequence Length", seq_len)
        
        with col3:
            st.metric("Number of Heads", num_heads)
