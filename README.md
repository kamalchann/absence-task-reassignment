# Sudden Absence Task Reassignment System

A single-page web application for intelligently managing task reassignment when employees are suddenly absent. The system uses competency scoring and workload capacity analysis to automatically reassign tasks to the best-fit teammate or flag them for manager review.

## 🎯 Features

### 1. **Employee Management**
- Display all employees with their roles and current task count
- Real-time "Mark Absent" toggle for each employee
- Visual indicators for absent employees (red border and background)
- Display competency score for each employee

### 2. **Competency Scoring**
The system calculates a competency score for each employee based on:
- **On-Time Rate**: Percentage of tasks completed on time
- **Efficiency Score**: Performance metric from task history
- **Formula**: (On-Time Rate × 0.5) + (Efficiency Score × 0.5)
- **Threshold**: Minimum 0.85 competency score required for reassignment

### 3. **Intelligent Task Reassignment**
When an employee is marked absent:
1. All their assigned tasks are evaluated for reassignment
2. The system finds candidates who:
   - Are **not absent**
   - Have the **required role** for the task
   - Have **available capacity** (< 3 tasks assigned)
   - Meet the **minimum competency threshold** (≥ 0.85)
3. Tasks are reassigned to the best-fit teammate with:
   - Highest competency score
   - Most available capacity

### 4. **Manager Review Flagging**
Tasks are flagged for manager review if:
- **No qualified teammate exists** (no one with required role and minimum competency)
- **All potential teammates are at capacity** (already have 3 tasks)
- **No one meets the minimum competency threshold**

Flagged tasks display:
- Original assignee name
- Task title and description
- Reason for flagging
- Priority level
- Required role

### 5. **Task Board**
- Visual display of all open tasks
- Color-coded by status and priority:
  - **Urgent (Red)**: High-priority tasks
  - **Warning (Orange)**: Tasks being reassigned
  - **Normal (White)**: Standard priority
- Shows task details, assigned employee, competency score, and available capacity
- Separate section for tasks flagged for manager review

### 6. **Statistics Dashboard**
Real-time counters showing:
- Total number of employees
- Currently absent employees
- Open tasks (not flagged)
- Tasks flagged for manager review

## 📊 Data Model

### Employee Object
```javascript
{
  id: number,
  name: string,
  role: string,
  absent: boolean,
  tasks: array of task IDs,
  completedTasks: number,
  onTimeCount: number,
  efficiencyScore: number (0-1)
}
```

### Task Object
```javascript
{
  id: string,
  title: string,
  assignedTo: number (employee ID),
  status: 'open' | 'reassigning' | 'flagged',
  priority: 'high' | 'medium' | 'low',
  requiredRole: string,
  description: string
}
```

## 🔧 Configuration

Edit these constants in the JavaScript to customize behavior:

```javascript
const WORKLOAD_THRESHOLD = 3;        // Max tasks per employee
const MIN_COMPETENCY_SCORE = 0.85;   // Minimum acceptable competency
```

## 🎨 User Interface

The application uses a modern, responsive design with:
- **Purple gradient header** with system title
- **Employee cards** with toggle switches for absence management
- **Task cards** with color-coded priority and status
- **Manager review section** with warning styling
- **Stats bar** showing key metrics at a glance
- **Responsive grid layout** (adapts from 2 columns to 1 on smaller screens)
- **Smooth transitions and hover effects**

## 🚀 How to Use

1. **Open the application** in your web browser (open `index.html`)
2. **View all employees** in the left panel with their competency scores
3. **Mark an employee as absent** by toggling the switch next to their name
4. **Observe automatic reassignment**:
   - Tasks are reassigned to best-fit teammates
   - The task board updates in real-time
   - Status changes to "Reassigning" for moved tasks
5. **Check manager review section** for flagged tasks that need manual review
6. **Mark employee as present** by toggling the switch again to restore normal state

## 📈 Example Scenario

**Initial State:**
- Alice (Developer): Tasks T001, T003
- Bob (Designer): Task T002
- Carol (Developer): No tasks
- David (QA): Tasks T004, T005
- Emma (Frontend): Task T006

**Alice Marks Absent:**
1. T001 (Fix Login Bug) → Reassigned to Carol (highest competency, available)
2. T003 (Database Optimization) → Also reassigned to Carol
3. Status updates to "Reassigning"

**If No Qualified Teammate:**
- Task is marked with "Flagged" status
- Added to manager review section
- Includes reason: "No teammate qualifies: insufficient competency score or workload at capacity"

## 🔐 Competency Calculation Example

```
Employee: Alice Johnson
- Completed Tasks: 18
- On-Time Tasks: 17
- Efficiency Score: 0.94

On-Time Rate = 17 / 18 = 0.944
Competency = (0.944 × 0.5) + (0.94 × 0.5) = 0.472 + 0.47 = 0.942
Display: 0.94 (rounded to 2 decimals)
```

## 💡 Key Algorithms

### Best-Fit Selection Algorithm
1. Filter candidates by: not absent, required role, has capacity
2. Sort by: competency score (descending), then capacity (descending)
3. Check if top candidate meets MIN_COMPETENCY_SCORE threshold
4. Return best fit or null if no one qualifies

### Absence Toggle Flow
1. Mark employee as absent
2. Collect all their assigned tasks
3. For each task: find best-fit teammate or flag for review
4. Update task status and assignments
5. Clear tasks from absent employee
6. Re-render UI

## 🎯 Color Scheme

- **Primary**: #667eea (Purple) - Headers, buttons, primary accents
- **Secondary**: #764ba2 (Dark Purple) - Gradient background
- **Success**: #27ae60 (Green) - Open/available status
- **Warning**: #f39c12 (Orange) - Reassigning status
- **Danger**: #e74c3c (Red) - Flagged/urgent status
- **Background**: #f8f9fa (Light Gray) - Cards and sections

## 📱 Responsive Design

The layout automatically adapts to different screen sizes:
- **Desktop (1200px+)**: 2-column layout (employees + tasks)
- **Tablet (768px - 1199px)**: 1-column layout with stacked sections
- **Mobile**: Single column with optimized spacing

## 🔄 Real-Time Updates

The UI uses a centralized `render()` function that updates:
- Employee list
- Task board
- Manager review section
- Statistics dashboard

All changes are reflected immediately when:
- Employee absence status toggles
- Tasks are reassigned
- New tasks are flagged

## 🛠️ Future Enhancements

Potential improvements:
- Persistent data storage (localStorage or backend)
- Manual task assignment override
- Task completion marking
- Historical reassignment logs
- Email notifications for managers
- Performance analytics and trends
- Multi-department support
- Task priority customization
- Advanced filtering and search

## 📄 License

This project is open source and available for educational and commercial use.

## 👨‍💼 Support

For questions or feature requests, please refer to the GitHub repository issues section.

---

**Last Updated:** September 2026
