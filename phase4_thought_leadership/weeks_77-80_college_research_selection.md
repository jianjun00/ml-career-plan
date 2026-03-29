# Weeks 77-80: College Research & Selection (September 1-28, 2027)

## 🎯 **Learning Objectives**
- Research and evaluate 15+ target universities
- Create comprehensive college comparison spreadsheet
- Identify academic requirements for math/CS programs
- Establish application timeline and strategy
- Connect with current students at target schools

---

## 📚 **Content & Resources**

### **College Research Methodology**
**Resource**: [College Board BigFuture](https://bigfuture.collegeboard.org/)
- **Research Components**:
  - [Academic Programs](https://www.usnews.com/best-colleges/rankings/computer-science)
  - [Mathematics Departments](https://www.ams.org/profession/data/annual-survey)
  - [Research Opportunities](https://www.nsf.gov/funding/pgm_summ.jsp?pims_id=5517)
  - [Campus Culture](https://www.niche.com/colleges/)

**Research Criteria**:
- Math/CS program strength and faculty
- Undergraduate research opportunities
- Internship and co-op programs
- Graduation outcomes and career placement
- Student satisfaction and retention rates

**Time Commitment**: 6 hours/week

### **Target University Categories**
**Reach Schools (3-4)**:
- MIT, Stanford, Carnegie Mellon, Caltech
- Requirements: 3.9+ GPA, 1550+ SAT, significant research

**Match Schools (4-5)**:
- UC Berkeley, Georgia Tech, University of Washington, Cornell
- Requirements: 3.7+ GPA, 1450+ SAT, strong projects

**Safety Schools (3-4)**:
- University of Maryland, Virginia Tech, Purdue, Texas A&M
- Requirements: 3.5+ GPA, 1350+ SAT, good extracurriculars

### **Academic Requirements Analysis**
**Resource**: [Naviance](https://www.naviance.com/)
- **Requirements to Research**:
  - GPA distributions for admitted students
  - SAT/ACT score ranges
  - Required high school coursework
  - AP/IB credit policies
  - Application deadlines and requirements

**Documentation Method**:
```python
# College Research Tracker
college_data = {
    'university': '',
    'program': 'Computer Science/Mathematics',
    'gpa_range': {'25th': '', '50th': '', '75th': ''},
    'sat_range': {'25th': '', '50th': '', '75th': ''},
    'acceptance_rate': '',
    'research_opportunities': [],
    'notable_faculty': [],
    'career_outcomes': {},
    'application_deadline': '',
    'required_courses': []
}
```

**Time Commitment**: 4 hours/week

### **Student Network Development**
**Resource**: [LinkedIn Alumni](https://www.linkedin.com/alumni/)
- **Networking Strategy**:
  - Find current students in math/CS programs
  - Request informational interviews
  - Ask about research opportunities
  - Learn about campus culture
  - Get application tips

**Contact Template**:
```
Subject: Question about [University] Math/CS Program

Dear [Student Name],
I found your profile through LinkedIn and was impressed by your work in [specific area]. 
I'm a high school student interested in mathematical reasoning and AI research, 
and I'm considering applying to [University]. 

Would you have 15-20 minutes to share your experience with:
- Research opportunities for undergraduates
- Quality of math/CS faculty
- Campus culture for STEM students
- Application tips for the program

Thank you for your time!
```

**Time Commitment**: 2 hours/week

---

## 🧪 **Evaluation & Assessment**

### **College Research Completion**
**Success Criteria**:
- [ ] Research 15+ universities with detailed notes
- [ ] Create comparison spreadsheet with all criteria
- [ ] Identify 3-4 reach, 4-5 match, 3-4 safety schools
- [ ] Document all academic requirements
- [ ] Establish application timeline with deadlines

### **Network Development**
**Success Criteria**:
- [ ] Connect with 5+ current students at target schools
- [ ] Complete 3+ informational interviews
- [ ] Get insights on research opportunities
- [ ] Receive application advice from current students
- [ ] Build ongoing relationships for future questions

### **Research Quality**
**Success Criteria**:
- [ ] All schools have complete academic profiles
- [ ] Research opportunities documented for each school
- [ ] Faculty research areas identified and matched
- [ ] Career outcomes data collected
- [ ] Application requirements clearly understood

---

## 🛠️ **Projects & Applications**

### **Project 1: College Comparison Dashboard**
**Objective**: Create interactive dashboard for college comparison

**Implementation**:
```python
import pandas as pd
import plotly.express as px
from dash import Dash, dcc, html, Input, Output

# College comparison data
colleges = [
    {
        'name': 'MIT',
        'gpa_25th': 3.9, 'gpa_50th': 4.0, 'gpa_75th': 4.0,
        'sat_25th': 1520, 'sat_50th': 1550, 'sat_75th': 1580,
        'acceptance_rate': 0.07, 'research_rank': 1,
        'cs_rank': 1, 'math_rank': 1
    },
    # Add more colleges...
]

df = pd.DataFrame(colleges)

# Create dashboard
app = Dash(__name__)

app.layout = html.Div([
    html.H1("College Comparison Dashboard"),
    dcc.Graph(id='gpa-comparison'),
    dcc.Graph(id='sat-comparison'),
    dcc.Graph(id='acceptance-rates')
])

@app.callback(
    [Output('gpa-comparison', 'figure'),
     Output('sat-comparison', 'figure'),
     Output('acceptance-rates', 'figure')],
    [Input('college-selector', 'value')]
)
def update_graphs(selected_colleges):
    filtered_df = df[df['name'].isin(selected_colleges)]
    
    gpa_fig = px.bar(filtered_df, x='name', y=['gpa_25th', 'gpa_50th', 'gpa_75th'],
                     title='GPA Ranges by College')
    
    sat_fig = px.bar(filtered_df, x='name', y=['sat_25th', 'sat_50th', 'sat_75th'],
                    title='SAT Score Ranges by College')
    
    accept_fig = px.scatter(filtered_df, x='acceptance_rate', y='cs_rank',
                         size='research_rank', hover_name='name',
                         title='Acceptance Rate vs CS Ranking')
    
    return gpa_fig, sat_fig, accept_fig

if __name__ == '__main__':
    app.run_server(debug=True)
```

**Deliverables**:
- [ ] Interactive college comparison dashboard
- [ ] Data for 15+ target universities
- [ ] Visualizations for academic metrics
- [ ] Filtering and ranking capabilities

### **Project 2: Application Timeline Manager**
**Objective**: Create comprehensive application tracking system

**Implementation**:
```python
from datetime import datetime, timedelta
import json

class ApplicationTracker:
    def __init__(self):
        self.applications = []
        self.deadlines = {}
        self.requirements = {}
    
    def add_college(self, college_name, deadline, requirements):
        """Add college with deadline and requirements"""
        self.applications.append({
            'college': college_name,
            'deadline': deadline,
            'requirements': requirements,
            'status': 'not_started',
            'progress': 0
        })
        
    def update_progress(self, college_name, task_completed):
        """Update application progress"""
        for app in self.applications:
            if app['college'] == college_name:
                app['progress'] += 10
                if app['progress'] >= 100:
                    app['status'] = 'completed'
                    
    def get_upcoming_deadlines(self):
        """Get sorted list of upcoming deadlines"""
        today = datetime.now()
        upcoming = []
        
        for app in self.applications:
            deadline = datetime.strptime(app['deadline'], '%Y-%m-%d')
            days_left = (deadline - today).days
            if days_left > 0:
                upcoming.append({
                    'college': app['college'],
                    'days_left': days_left,
                    'progress': app['progress']
                })
                
        return sorted(upcoming, key=lambda x: x['days_left'])
    
    def generate_checklist(self, college_name):
        """Generate application checklist"""
        checklist = {
            'research': False,
            'essay_1': False,
            'essay_2': False,
            'recommendations': False,
            'portfolio': False,
            'test_scores': False,
            'transcripts': False,
            'submission': False
        }
        return checklist

# Usage example
tracker = ApplicationTracker()
tracker.add_college('MIT', '2028-01-01', {
    'essays': 5,
    'recommendations': 3,
    'test_scores': True,
    'portfolio': True
})
```

**Deliverables**:
- [ ] Application tracking system
- [ ] Deadline alerts and reminders
- [ ] Progress visualization
- [ ] Checklist generation for each college

---

## 📊 **Progress Tracking**

### **Weekly Checklist**
- [ ] 2 hours college research (3-4 new schools)
- [ ] 1 hour spreadsheet updates and analysis
- [ ] 1 hour networking outreach
- [ ] 1 hour application timeline planning
- [ ] 1 hour portfolio updates
- [ ] 1 hour test preparation

### **Weekly Milestones**
**Week 77**:
- [ ] Research MIT, Stanford, Carnegie Mellon
- [ ] Create initial spreadsheet structure
- [ ] Connect with 2 current students
- [ ] Document application requirements

**Week 78**:
- [ ] Research UC Berkeley, Georgia Tech, Cornell
- [ ] Complete academic requirements analysis
- [ ] Schedule 3 informational interviews
- [ ] Update comparison dashboard

**Week 79**:
- [ ] Research safety schools
- [ ] Finalize college categories
- [ ] Complete informational interviews
- [ ] Refine application strategy

**Week 80**:
- [ ] Complete college research (15+ schools)
- [ ] Finalize application timeline
- [ ] Get feedback on college list
- [ ] Prepare for application writing phase

### **End-of-Period Assessment**
**Success Metrics**:
- [ ] College Research: 15+ universities researched and documented
- [ ] Analysis Quality: Complete academic and career data for all schools
- [ ] Network: 5+ student connections made
- [ ] Strategy: Clear application plan with timeline
- [ ] Tools: Functional tracking and comparison systems

---

## 🚀 **Next Period Preparation**

### **Phase 4 Weeks 81-84 Preview**
- Begin college essay writing and refinement
- Develop personal narrative and themes
- Create essay templates for common prompts
- Start recommendation letter requests
- Prepare application materials

### **Resources to Preview**:
- [Common App Essay Prompts](https://www.commonapp.org/apply/essay-prompts/)
- [College Essay Examples](https://www.collegeessayguy.com/)
- [Recommendation Letter Guide](https://www.usnews.com/education/best-colleges/articles/teacher-recommendation-letters)

---

## 📞 **Support & Resources**

### **Help Channels**:
- School college counselor
- English teachers for essay feedback
- Math/CS teachers for recommendations
- College admissions workshops
- Peer review groups

### **Additional Resources**:
- [College Board](https://www.collegeboard.org/)
- [US News College Rankings](https://www.usnews.com/best-colleges)
- [Niche College Reviews](https://www.niche.com/colleges/)
- [Reddit r/ApplyingToCollege](https://www.reddit.com/r/ApplyingToCollege/)

---

*This 4-week plan provides concrete, actionable steps for college research and selection with measurable outcomes and realistic timelines.*
