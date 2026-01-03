# DayFlow HRMS - Quick Setup Guide

## ✅ Current Status
**The project is now running successfully!**

🌐 **Application URL**: http://127.0.0.1:8000/
🔧 **Django Admin**: http://127.0.0.1:8000/django-admin/

## 📦 What Was Done

### 1. Dependencies Installed ✅
All required packages have been installed:
- Django 5.x
- mysqlclient
- django-environ
- Pillow

### 2. Database Migrated ✅
Database migrations have been applied successfully. The SQLite database is ready with all tables:
- CustomUser (users with employee IDs)
- Profile (employee profiles)
- Attendance (attendance tracking)
- LeaveRequest (leave management)
- Payroll (salary information)

### 3. Development Server Running ✅
The Django development server is running on port 8000.

## 🚀 Next Steps

### Create Your First Admin User

To access the admin panel and start using the system, you need to create an admin user:

```bash
# Navigate to the mysite directory
cd mysite

# Create a superuser
python manage.py createsuperuser
```

You'll be prompted to enter:
- **Username**: Choose a username
- **Email**: Your email address
- **Password**: Choose a secure password
- **Employee ID**: Format should be EMP1234 (EMP followed by 4-6 digits)
- **Role**: Choose ADMIN

### Access the Application

1. **Sign Up Page**: http://127.0.0.1:8000/signup/
   - Register as a new employee
   - Email verification will print to console (check terminal)

2. **Sign In Page**: http://127.0.0.1:8000/signin/
   - Login with your credentials
   - Redirects based on role (Admin → Admin Dashboard, Employee → Employee Dashboard)

3. **Django Admin Panel**: http://127.0.0.1:8000/django-admin/
   - Full Django admin interface
   - Manage all models directly

## 🎯 Testing the Application

### As an Employee:
1. Sign up at `/signup/`
2. Verify email (check console output)
3. Sign in at `/signin/`
4. Access features:
   - View dashboard
   - Update profile
   - Check-in/Check-out attendance
   - Request leave
   - View payroll

### As an Admin:
1. Create admin user via `createsuperuser` command
2. Sign in at `/signin/`
3. Access admin features:
   - View admin dashboard
   - Manage employees
   - View attendance records
   - Approve/reject leave requests
   - Manage salaries

## 📁 Project Structure Quick Reference

```
mysite/
├── manage.py              # Django management script
├── db.sqlite3            # SQLite database (created after migration)
├── mysite/               # Project settings
│   └── settings.py       # Configuration file
└── hrms/                 # Main application
    ├── models.py         # Database models
    ├── views.py          # View logic
    ├── urls.py           # URL routing
    ├── forms.py          # Form definitions
    ├── templates/        # HTML templates
    └── static/           # CSS, JS, images
```

## 🛠️ Useful Commands

### Running the Server
```bash
cd mysite
python manage.py runserver
```

### Making Database Changes
```bash
python manage.py makemigrations
python manage.py migrate
```

### Creating Superuser
```bash
python manage.py createsuperuser
```

### Collecting Static Files (for production)
```bash
python manage.py collectstatic
```

### Running Tests
```bash
python manage.py test
```

## 🔍 Troubleshooting

### Port Already in Use
If port 8000 is busy, run on a different port:
```bash
python manage.py runserver 8080
```

### Database Issues
If you encounter database errors, try:
```bash
# Delete the database
rm db.sqlite3

# Delete migrations (except __init__.py)
# Then recreate migrations
python manage.py makemigrations
python manage.py migrate
```

### Static Files Not Loading
Make sure you're in DEBUG mode (already set in settings.py):
```python
DEBUG = True
```

## 📧 Email Configuration

Currently, emails are printed to the console. To see verification emails:
1. Check the terminal where the server is running
2. Look for email output after user registration

For production, configure SMTP in `settings.py`:
```python
EMAIL_BACKEND = 'django.core.mail.backends.smtp.EmailBackend'
EMAIL_HOST = 'smtp.gmail.com'
EMAIL_PORT = 587
EMAIL_USE_TLS = True
EMAIL_HOST_USER = 'your_email@gmail.com'
EMAIL_HOST_PASSWORD = 'your_app_password'
```

## 🎨 Customization

### Changing Time Zone
Edit `mysite/settings.py`:
```python
TIME_ZONE = 'Asia/Kolkata'  # Change to your timezone
```

### Switching to MySQL
1. Uncomment MySQL configuration in `settings.py`
2. Comment out SQLite configuration
3. Create MySQL database
4. Update credentials
5. Run migrations

## 📊 Sample Data

To populate the system with sample data, you can:
1. Use Django admin to manually add data
2. Create a custom management command
3. Use Django fixtures

## 🔐 Security Notes

**Before deploying to production:**
1. Change `SECRET_KEY` to environment variable
2. Set `DEBUG = False`
3. Configure `ALLOWED_HOSTS`
4. Use proper database (PostgreSQL/MySQL)
5. Configure HTTPS
6. Set up proper email backend
7. Configure static/media file storage

## 📚 Additional Resources

- **Django Documentation**: https://docs.djangoproject.com/
- **Project Analysis**: See `PROJECT_ANALYSIS.md` for detailed documentation
- **Django Admin**: http://127.0.0.1:8000/django-admin/

## ✨ Features Overview

### Employee Features
- ✅ Personal dashboard
- ✅ Profile management
- ✅ Attendance tracking (check-in/out)
- ✅ Leave requests
- ✅ Payroll viewing

### Admin Features
- ✅ Admin dashboard with statistics
- ✅ Employee management
- ✅ Attendance monitoring
- ✅ Leave approval workflow
- ✅ Salary management

## 🎉 You're All Set!

The DayFlow HRMS is now running and ready to use. Start by creating an admin user and exploring the features!

**Happy Managing! 🚀**
