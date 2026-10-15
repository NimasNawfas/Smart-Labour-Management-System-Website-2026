# 🏢 Smart Labour Management System (SLMS)
> **CSE370: Database Management Systems Course Project**

An enterprise-grade, web-based workforce management platform designed to automate and streamline operations for small and medium-sized enterprises (SMEs). SLMS digitizes traditional manual workflows, offering robust management tools for worker profiles, attendance tracking, shift substitutions, task delegation, payroll calculation, leave requests, and performance evaluation.

---

## 🛠️ Tech Stack & Architecture

* **Frontend:** HTML5, CSS3, Bootstrap 5 (Responsive UI), Vanilla JavaScript
* **Backend:** Python Flask (Lightweight Web Framework)
* **Database & Environment:** MySQL (Relational Database Management via XAMPP)
* **Template Engine:** Jinja2 (Dynamic HTML rendering with layout inheritance, custom filters, and global context processors)

---

## 🌟 Core Features & Module Breakdown

### 1. Unified Profile Management (`/worker/profile`)
* **Role-Based Access:** A single normalized `WORKER` table handles all users. Access levels (`worker`, `manager`, `admin`) are governed by the `role` column.
* **Self-Service Portal:** Workers and managers can update personal contact info and securely change passwords via dynamic modal forms with instant color-coded feedback alerts.

### 2. Administrative Control Center (`/admin/dashboard`)
* **Live Analytics:** Real-time statistics cards tracking Total Active Workers, Today's Check-ins, and Pending Approvals.
* **Global Oversight:** Filterable data tables for managing all personnel, updating status (`Active`, `Inactive`, `On Leave`), reviewing reports, and locking payroll.

### 3. Shift Substitution System
* **Peer-to-Peer Workflow:** Workers can request coverage for specific shifts by selecting an available peer from the same department.
* **Approval Pipeline:** Requests follow a strict workflow: 
  $$\text{Worker Requests} \longrightarrow \text{Peer Accepts} \longrightarrow \text{Admin Approves}$$
  Approved substitutes automatically log bonus hours to ensure seamless business continuity.

### 4. Two-Stage Attendance System
* **Anti-Cheating Mechanism:** 
  * **Check-In:** Logs an initial attendance value of `0.5`.
  * **Check-Out:** Updates the record to `1.0`, closes the live shift timer, and automatically computes total working hours using SQL `TIMEDIFF()`.
* **Visual Dashboards:** Color-coded monthly calendars displaying present (`green`), absent (`red`), and half-day (`yellow`) states alongside percentage progress bars.

### 5. Manager Task Assignment & Kanban Boards
* **Department-Scoped Delegation:** Managers assign tasks with custom deadlines exclusively to workers within their assigned department.
* **Interactive Kanban Boards:** Real-time task tracking columns (*To Do*, *In Progress*, *Done*) with urgency priority indicators (`red = urgent`, `yellow = normal`, `green = low`).

### 6. Automated Salary Processing
* **Dynamic Calculations:** Computes net payout using base salary paired with overtime compensation earned through extra or substitute hours:
  $$\text{Total Salary} = \text{Base Salary} + (\text{Extra Hours} \times \text{Hourly Bonus Rate})$$
* **Payroll States:** Workflow tracking moving from `Draft` $\rightarrow$ `Finalized` $\rightarrow$ `Paid` with downloadable pay slips.

### 7. Performance Evaluation Engine
* **Automated Metrics:** System-generated monthly evaluations calculating average attendance percentages and aggregate work hours directly from database logs.
* **Qualitative Feedback:** Integrated manager review sections providing structured performance summaries and historical trends.

### 8. Leave Management & Status Synchronization
* **Automated Leave Hooks:** Workers apply for Casual, Sick, or Annual leaves specifying date ranges. 
* **Database Interlock:** Upon admin approval, the system automatically shifts the worker's operational status to `On Leave`, temporarily blocking attendance check-ins for those dates and auto-reverting status upon expiry.

---

## 💻 Backend Implementation Highlights

### Unified User Routing & Profile Query
```python
@app.route('/worker/profile')
def worker_profile():
    # Fetch worker data using session identifier
    cursor.execute("SELECT * FROM WORKER WHERE worker_id = %s", (session['user_id'],))
    user_data = cursor.fetchone()
    return render_template('profile.html', user=user_data)
