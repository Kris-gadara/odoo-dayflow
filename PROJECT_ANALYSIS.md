# DayFlow HRMS - Project Analysis

## 📋 Project Overview

**DayFlow** is a comprehensive role-based Human Resource Management System (HRMS) built with Django. It streamlines employee onboarding, attendance tracking, leave management, and approval workflows through a secure and user-friendly web platform.

## 🏗️ Technology Stack

- **Framework**: Django 5.x
- **Database**: SQLite (default) / MySQL (configurable)
- **Python Version**: 3.11.9
- **Key Dependencies**:
  - Django >= 5.0, < 6.0
  - mysqlclient >= 2.2.0
  - django-environ >= 0.11.0
  - Pillow >= 10.0.0

## 📁 Project Structure

```
odoo-dayflow/
├── mysite/                      # Main Django project
│   ├── mysite/                  # Project settings
│   │   ├── settings.py          # Configuration
│   │   ├── urls.py              # URL routing
│   │   ├── wsgi.py              # WSGI config
│   │   └── asgi.py              # ASGI config
│   ├── hrms/                    # Main HRMS application
│   │   ├── models.py            # Database models
│   │   ├── views.py             # View logic
│   │   ├── forms.py             # Form definitions
│   │   ├── urls.py              # App URL patterns
│   │   ├── admin.py             # Admin configuration
│   │   ├── signals.py           # Django signals
│   │   ├── templates/           # HTML templates
│   │   │   ├── base.html        # Base template
│   │   │   ├── auth/            # Authentication templates
│   │   │   ├── employee/        # Employee templates
│   │   │   └── admin/           # Admin templates
│   │   ├── static/              # Static files (CSS, JS, images)
│   │   └── management/          # Custom management commands
│   ├── core/                    # Core application (minimal)
│   └── manage.py                # Django management script
├── requirements.txt             # Python dependencies
├── .gitignore                   # Git ignore rules
└── README.md                    # Project documentation
```

## 🗄️ Database Models

### 1. **CustomUser** (extends AbstractUser)
- **Purpose**: Custom user model with employee ID and role
- **Key Fields**:
  - `employee_id`: Unique identifier (format: EMP1234)
  - `role`: ADMIN or EMPLOYEE
  - `email_verified`: Email verification status
  - `verification_token`: For email verification
- **Methods**:
  - `generate_verification_token()`: Creates unique verification token

### 2. **Profile**
- **Purpose**: Employee profile with job and personal details
- **Key Fields**:
  - `user`: OneToOne with CustomUser
  - `designation`: Job title
  - `department`: Department name
  - `date_of_joining`: Employment start date
  - `employment_type`: FULL_TIME, PART_TIME, CONTRACT, INTERN
  - `phone_number`, `address`, `emergency_contact`: Personal info
  - `profile_picture`: Employee photo

### 3. **Attendance**
- **Purpose**: Employee attendance tracking
- **Key Fields**:
  - `user`: ForeignKey to CustomUser
  - `date`: Attendance date
  - `check_in_time`, `check_out_time`: Time tracking
  - `status`: PRESENT, ABSENT, HALF_DAY
  - `total_hours`: Calculated work hours
  - `notes`: Additional comments
- **Methods**:
  - `calculate_total_hours()`: Auto-calculates hours and status

### 4. **LeaveRequest**
- **Purpose**: Employee leave request management
- **Key Fields**:
  - `user`: ForeignKey to CustomUser
  - `leave_type`: PAID, SICK, UNPAID, CASUAL
  - `start_date`, `end_date`: Leave period
  - `status`: PENDING, APPROVED, REJECTED
  - `remarks`: Employee's reason
  - `admin_comment`: Admin's feedback
  - `approved_by`: Admin who approved/rejected
- **Properties**:
  - `total_days`: Calculated leave duration

### 5. **Payroll**
- **Purpose**: Employee payroll and salary structure
- **Key Fields**:
  - `user`: ForeignKey to CustomUser
  - `basic_salary`: Base salary
  - **Allowances**: HRA, transport, medical, other
  - **Deductions**: PF, professional tax, income tax, other
  - `effective_date`: When salary becomes active
- **Properties**:
  - `gross_salary`: Basic + all allowances
  - `total_deductions`: Sum of all deductions
  - `net_salary`: Gross - deductions

## 🎯 Key Features

### Authentication & Authorization
- ✅ User registration with email verification
- ✅ Role-based access control (Admin/Employee)
- ✅ Secure login/logout
- ✅ Custom user model with employee ID

### Employee Features
- ✅ Personal dashboard with quick stats
- ✅ Profile management (view/edit personal info)
- ✅ Attendance check-in/check-out
- ✅ Weekly attendance summary
- ✅ Leave request submission
- ✅ Leave request tracking
- ✅ Payroll viewing (read-only)

### Admin/HR Features
- ✅ Admin dashboard with overview statistics
- ✅ Employee management (list, search, edit)
- ✅ Attendance records viewing
- ✅ Leave approval/rejection workflow
- ✅ Salary management and updates
- ✅ Full CRUD operations on employee data

## 🔐 Security Features

1. **Authentication**:
   - Email verification required
   - Secure password validation
   - Login required decorators

2. **Authorization**:
   - Role-based access control
   - Admin-only views protected
   - User-specific data isolation

3. **Django Security**:
   - CSRF protection enabled
   - XSS protection via templates
   - SQL injection protection (ORM)
   - Secure session management

## 🌐 URL Structure

### Public URLs
- `/signup/` - User registration
- `/signin/` - User login
- `/verify-email/<token>/` - Email verification

### Employee URLs
- `/` - Employee dashboard
- `/employee/profile/` - Profile management
- `/employee/attendance/` - Attendance view
- `/employee/attendance/checkin/` - Check-in
- `/employee/attendance/checkout/` - Check-out
- `/employee/leave/` - Leave requests list
- `/employee/leave/create/` - Create leave request
- `/employee/payroll/` - Payroll details

### Admin URLs
- `/admin/dashboard/` - Admin dashboard
- `/admin/employees/` - Employee list
- `/admin/employees/<id>/edit/` - Edit employee
- `/admin/attendance/` - Attendance records
- `/admin/leave/` - Leave approvals
- `/admin/leave/<id>/<action>/` - Approve/reject leave
- `/admin/salary/` - Salary management
- `/admin/salary/<id>/update/` - Update salary

## ⚙️ Configuration

### Database
- **Default**: SQLite (development)
- **Production**: MySQL (commented out in settings)
- **Location**: `mysite/db.sqlite3`

### Static Files
- **URL**: `/static/`
- **Directory**: `mysite/hrms/static/`
- **Collected**: `mysite/staticfiles/`

### Media Files
- **URL**: `/media/`
- **Directory**: `mysite/media/`
- **Purpose**: Profile pictures, uploads

### Email
- **Development**: Console backend (prints to terminal)
- **Production**: SMTP (commented configuration available)

### Time Zone
- **Configured**: Asia/Kolkata (IST)

## 🚀 Setup & Running

### Prerequisites
1. Python 3.11+ installed
2. pip package manager
3. Virtual environment (recommended)

### Installation Steps
1. Create virtual environment:
   ```bash
   python -m venv venv
   venv\Scripts\activate  # Windows
   ```

2. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```

3. Run migrations:
   ```bash
   cd mysite
   python manage.py makemigrations
   python manage.py migrate
   ```

4. Create superuser (optional):
   ```bash
   python manage.py createsuperuser
   ```

5. Run development server:
   ```bash
   python manage.py runserver
   ```

6. Access the application:
   - Main app: http://127.0.0.1:8000/
   - Django admin: http://127.0.0.1:8000/django-admin/

## 📊 Business Logic Highlights

### Attendance System
- Automatic status calculation based on hours worked
- ≥8 hours = PRESENT
- ≥4 hours = HALF_DAY
- <4 hours = ABSENT
- Unique constraint: one attendance per user per day

### Leave Management
- Multi-step approval workflow
- Leave type categorization
- Automatic day calculation
- Admin comments and feedback
- Approval tracking

### Payroll System
- Flexible allowance structure
- Multiple deduction types
- Automatic gross/net salary calculation
- Historical salary records
- Effective date tracking

## 🎨 Frontend Templates

The application uses Django templates with:
- **Base template**: Common layout and navigation
- **Auth templates**: Sign-up, sign-in pages
- **Employee templates**: Dashboard, profile, attendance, leave, payroll
- **Admin templates**: Dashboard, employee management, approvals, salary

## 🔄 Workflow Examples

### Employee Onboarding
1. Admin creates employee account
2. Employee receives verification email
3. Employee verifies email and logs in
4. Employee completes profile information
5. Admin assigns salary structure

### Leave Request Flow
1. Employee submits leave request
2. Request appears in admin's approval queue
3. Admin reviews and approves/rejects
4. Employee sees updated status
5. Admin comment visible to employee

### Attendance Tracking
1. Employee checks in at start of day
2. System records check-in time
3. Employee checks out at end of day
4. System calculates hours and status
5. Weekly summary available to employee
6. Admin can view all attendance records

## 🛠️ Potential Enhancements

1. **Notifications**: Email/SMS alerts for approvals
2. **Reports**: PDF generation for payslips
3. **Analytics**: Dashboard charts and graphs
4. **Mobile App**: REST API for mobile access
5. **Biometric**: Integration with attendance devices
6. **Calendar**: Visual leave calendar
7. **Performance**: Employee performance tracking
8. **Documents**: Document management system

## 📝 Notes

- The project uses Django's built-in admin at `/django-admin/`
- Custom admin interface at `/admin/dashboard/`
- Email verification currently uses console backend
- Database is SQLite by default (easy for development)
- Ready for MySQL migration in production
- All sensitive data should be moved to environment variables

## 🐛 Known Considerations

1. **Email Verification**: Currently outputs to console, needs SMTP for production
2. **Security**: SECRET_KEY should be in environment variables
3. **Database**: SQLite not recommended for production
4. **Static Files**: Need to run `collectstatic` for production
5. **Media Files**: Need proper storage backend for production (e.g., S3)

## 📄 License & Credits

This is a custom-built HRMS solution for managing employee lifecycle operations.
