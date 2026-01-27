
# Introduction: The Smart Labour Management System is an integrated web-based system made to help small and medium-sized businesses automate and simplify workforce management. An integrated platform that manages worker profiles, attendance monitoring, task management, salary processing, leave management, and performance evaluation is created by digitizing conventional manual labor processes for management.



# Frontend Development : 

  Built the frontend using HTML5, CSS3, and Bootstrap 5 for responsive design across all devices. Jinja2 templating engine (with Flask) dynamically generates pages by injecting Python data into HTML. Used vanilla JavaScript.


  Features:
  
  1. WORKER, MANAGER PROFILE 
  
  
    What the user sees:
  
      Clean dashboard with user's photo/avatar at top
      Personal information card: Name, Email, Contact, Address, Department
      Work information card: Joining Date, Status (Active/Inactive), Role
      Two buttons: "Edit Contact Info" and "Change Password"
      When editing, a pop-up form appears with current info pre-filled
      Success/error messages appear as colored alerts (green/red)
  
    Visual elements:
  
      Professional blue-themed design
      Profile picture placeholder with initials
      Information displayed in labeled boxes
      Responsive layout that works on mobile
  
  
  2. ADMIN CONTROL:
  
  
    What admin sees:
  
      Dashboard with statistics cards: Total Workers, Today's Attendance, Pending Approvals
      Sidebar navigation: Dashboard, All Workers, Attendance Reports, Salary Management, Approvals
      All Workers page shows table with: ID, Name, Department, Status, Actions
      Clicking a worker shows detailed modal with all their info
      Status toggle buttons: Active/Inactive/On Leave
      Color-coded status badges (green=Active, yellow=On Leave, red=Inactive)
  
    Visual elements:
  
      Admin-specific red/black theme
      Interactive data tables with search and filter
      Charts for attendance reports (bar charts, pie charts)
      Confirmation modals for critical actions
  
  
  3. SUBSTITUTE SYSTEM
  
  
    What workers see:
  
      "Request Substitute" button on their dashboard
      Form with: Date picker, Hours dropdown, Reason textarea, and "Available Substitutes" dropdown
      Shows list of pending requests with status badges
      Shows requests where they're the substitute with "Accept/Decline" buttons
      Admin sees separate "Approve Substitutes" page with all pending requests
      Each request shows: Requester, Substitute, Date, Hours, Status
  
    Visual elements:
  
      Calendar picker for selecting dates
      Dropdown showing available substitutes (same department)
      Status badges: Pending (yellow), Accepted (blue), Approved (green), Rejected (red)
      Table layout with action buttons
  
  
  4. ATTENDANCE SYSTEM
  
  
    What workers see:
  
      Large "Check In" button (green) when starting shift
      After check-in: Button changes to "Check Out" (red) with timer showing current shift duration
      Attendance history table: Date, Check-in Time, Check-out Time, Hours, Status
      Visual calendar showing attendance (green=present, red=absent, yellow=half-day)
      Today's attendance status prominently displayed
  
    Visual elements:
  
      Large prominent buttons with icons (clock icon for check-in)
      Live timer when checked in
      Color-coded calendar view
      Progress bar showing monthly attendance percentage
  
  
  5. MANAGER TASK ASSIGNMENT
  
    What managers see:
  
      "Assign New Task" button leading to a form
      Form has: Worker selection dropdown, Task description textarea, Deadline date picker
      View of all assigned tasks in a Kanban-style board (To Do, In Progress, Done)
      Each task card shows: Worker name, Task description, Deadline, Status
      Drag-and-drop to move tasks between columns (optional feature)
      Option to edit/delete tasks
  
    Visual elements:
  
      Kanban board with color-coded columns
      Task cards with priority colors (red=urgent, yellow=normal, green=low)
      Hover effects on task cards
      Date picker with calendar view
      Worker avatars on task cards
  
  
  6. SALARY CALCULATIONS 
  
  
    What workers see:
  
      Monthly salary slip cards: Month, Base Salary, Bonus, Total, Status (Paid/Pending)
      Breakdown showing: Basic pay, Overtime hours, Overtime pay, Deductions, Net pay
      Download/Print button for salary slips
      Visual chart showing salary trend over months
      Current month's expected salary preview
  
    What admin sees:
  
      Salary management table with all workers
      Edit buttons to adjust salary components
      "Finalize" button to lock salary for payment
      Status filters: Draft, Finalized, Paid
      Bulk actions for multiple workers
  
    Visual elements:
  
      Salary slip with professional formatting
      Pie chart showing salary composition
      Line chart showing salary history
      Color-coded status badges
  
  
  7. WORKER PERSONAL TASK PROGRESS 
  
  
    What workers see:
  
      Personal task board with their assigned tasks
      Each task shows: Task description, Assigned by, Deadline, Current status
      Status update dropdown on each task: Pending → In Progress → Completed
      Progress bar showing completed vs pending tasks
      Color-coded deadlines (red=overdue, yellow=due soon, green=on track)
      Option to add notes/comments to tasks
  
    Visual elements:
  
      Personal task dashboard with progress visualization
      Status badges with colors
      Deadline countdown timer for urgent tasks
      Checkmark animations when marking complete
  
  
  8. PERFORMANCE SYSTEM
  
  
    What workers see:
  
      Monthly performance card: Attendance %, Total Hours, Efficiency Score
      Manager feedback section with comments
      "Request Performance Report" button
      Visual charts: Attendance trend, Hours worked comparison
      Performance history table showing month-by-month data
  
    What managers see:
  
      Team performance overview
      Individual worker performance cards
      "Give Feedback" button that opens feedback form
      Performance comparison charts across team
  
    Visual elements:
  
      Gauge charts for attendance percentage
      Bar charts for hours comparison
      Star ratings for overall performance
      Printable performance report format
  
  
  9. LEAVE MANAGEMENT
  
  
    What workers see:
  
      "Apply for Leave" button opening a form
      Form has: Leave type dropdown (Casual/Sick/Annual), Date range picker, Reason textarea
      Leave balance display: Available leaves of each type
      Leave history table: Type, Dates, Status, Approval Date
      Calendar view showing approved leaves
      Status badges: Pending (yellow), Approved (green), Rejected (red)
  
    What admin sees:
  
      Leave approval dashboard with pending requests
      Each request shows: Employee, Leave type, Dates, Reason
      Approve/Reject buttons with optional comment
      Team leave calendar showing who's out when
  
    Visual elements:
  
      Visual calendar showing leave days
      Leave balance progress bars
      Date range selector with highlight
      Email-style interface for leave requests
      


# Backend Development:

  Python Flask Framework: Lightweight, flexible web framework for rapid development
  MySQL Database: Relational database using XAMPP stack


  Template Integration

    Jinja2 Templating: Dynamic HTML generation
    Custom Filters: Currency formatting, date formatting, ID padding
    Context Processors: Global template variables
    Template Inheritance: Base templates for consistency

  Deployment Configuration

    XAMPP Integration: Local MySQL database
    Environment Configuration: Separate config files
    Port Management: Standard web port (5000)
    Cross-platform Support: Windows/Linux compatible


    1. WORKER, MANAGER PROFILE
  
  
      # Single WORKER table stores ALL users
      # Role column determines access: 'worker', 'manager', 'admin'
      
      @app.route('/worker/profile')
      def worker_profile():
          # Get worker data using their session ID
          cursor.execute("SELECT * FROM WORKER WHERE worker_id=%s", (session['user_id'],))
          # Show in profile.html template
    
      How it works:
  
        One table WORKER with role column
        Same profile page adapted for all roles
        Users can edit only their contact/address
        Password change available for all
  
  
    2. ADMIN CONTROL
  
  
    @app.route('/admin/dashboard')
    def admin_dashboard():
        # Admin sees everything
        cursor.execute("SELECT COUNT(*) FROM WORKER WHERE role='worker'")
        cursor.execute("SELECT COUNT(*) FROM ATTENDANCE WHERE date=CURDATE()")
        # Shows stats on dashboard
    
    Admin Powers:
  
      View all workers/managers
      Update worker status (Active/Inactive/On Leave)
      Approve substitutes and leaves
      Finalize salary slips
      View attendance reports
  
  
    3. SUBSTITUTE SYSTEM
  
    
      # Worker requests substitute
      sql = "INSERT INTO SUBSTITUTE_REQUEST VALUES (requester_id, substitute_id, date, hours, 'Pending', FALSE)"
      
      # Substitute accepts
      UPDATE SUBSTITUTE_REQUEST SET status='Accepted'
      
      # Admin approves
      UPDATE SUBSTITUTE_REQUEST SET admin_approved=TRUE
      
      Flow:
      
        1. Worker requests → 2. Substitute accepts → 3. Admin approves
        Substitute gets bonus hours
        Ensures work continuity
  
  
    4. ATTENDANCE SYSTEM
  
  
      # Two-stage system
      # Check-in: Insert with 0.5 value
      INSERT INTO ATTENDANCE (worker_id, date, check_in, attendance_value) 
      VALUES (id, today, time, 0.5)
      
      # Check-out: Update to 1.0 + calculate hours
      UPDATE ATTENDANCE SET check_out=time, attendance_value=1.0, 
      working_hours=TIMEDIFF(check_out, check_in)
      
      Unique Feature:
    
        Check-in = 0.5 attendance
        Check-out = 1.0 attendance
        Auto-calculates working hours
        Prevents cheating (need both in/out)
  
  
    5. MANAGER TASK ASSIGNMENT
  
  
      # Manager assigns to their department only
      sql = "INSERT INTO TASK (worker_id, task_details, deadline, status) 
      VALUES (%s, %s, %s, 'Pending')"
      
      Features:
  
        Managers see only their department workers
        Assign tasks with deadlines
        Workers get notifications
        Tasks appear on worker's profile
  
  
    6. SALARY CALCULATIONS
  
  
      # Salary formula
      base_salary = 20000  # Fixed minimum
      extra_hours = get_from_database()
      bonus_amount = extra_hours * 50  # 50 taka per extra hour
      total_salary = base_salary + bonus_amount
      
      Process:
  
        System calculates from attendance/substitute hours
        Admin reviews and finalizes
        Status: Draft → Finalized → Paid
        Workers can view their salary slips
  
  
      
    7. WORKER PERSONAL TASK PROGRESS
  
  
        # Worker updates task status
        UPDATE TASK SET status=%s WHERE task_id=%s AND worker_id=%s
        # Status options: 'Pending', 'In Progress', 'Completed', 'Delayed'
        
        Features:  
  
          Workers see only their tasks
          Can update status
          Shows deadlines
          Color-coded by status
  
  
    8. PERFORMANCE SYSTEM
  
  
        # Monthly performance record
        INSERT INTO PERFORMANCE (worker_id, month, attendance_percentage, total_hours, manager_feedback)
        # Auto-calculated from attendance data
        SELECT AVG(attendance_value)*100, SUM(working_hours) FROM ATTENDANCE
        
        Components:
  
          Attendance percentage (auto-calculated)
          Total hours (auto-calculated)
          Manager feedback (manual input)
          Workers can request performance slip
  
  
    9. LEAVE MANAGEMENT
  
  
        # Worker applies
        INSERT INTO LEAVE_REQUEST (worker_id, leave_type, start_date, end_date, reason, status)
        VALUES (%s, %s, %s, %s, %s, 'Pending')
        
        # System blocks attendance during leave
        # When checking attendance:
        if worker_on_leave(date):
            block_check_in()
        
        Flow:
  
          Worker submits with dates/reason
          Admin approves/rejects
          System auto-updates worker status to "On Leave"
          Attendance blocked during leave period
          Auto-reverts to "Active" after leave
  

Conclusion: This Smart Labour Management System successfully integrates all essential workforce management features into a unified platform. The project demonstrates practical implementation of database management, user authentication, and role-based access control using Flask and MySQL. By combining attendance tracking, task management, salary calculation, and leave systems, it provides a complete solution for efficient workforce administration. The system's modular design allows easy scalability and maintenance, making it suitable for real-world deployment in small to medium organizations.


