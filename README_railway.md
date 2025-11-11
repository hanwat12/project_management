# Railway Deployment Guide for SLRDX Project Management App

## Prerequisites
- Railway account (https://railway.app)
- Git repository with your project

## Step-by-Step Deployment Process

### 1. Create Railway Project
1. Go to https://railway.app and sign in
2. Click "New Project"
3. Choose "Deploy from GitHub repo" or "Empty Project"
4. If using GitHub, connect your repository

### 2. Add MySQL Database
1. In your Railway project dashboard, click "Add Plugin"
2. Search for "MySQL" and add it
3. Railway will automatically create a MySQL database and provide connection details

### 3. Configure Environment Variables
In your Railway project settings, add these environment variables:

```
DATABASE_URL=mysql+pymysql://username:password@host:port/database_name
SESSION_SECRET=your-super-secret-session-key-change-in-production
FLASK_DEBUG=False
```

**Important:** Get the DATABASE_URL from Railway's MySQL plugin variables section.

### 4. Deploy the Application
1. Push your code to your Git repository (if using GitHub integration, it will auto-deploy)
2. Railway will automatically build and deploy your app using the Procfile
3. The app will be available at the generated Railway URL

### 5. Database Migration
After deployment, you need to initialize the database:

1. Connect to your Railway app's terminal
2. Run: `python -c "from app import db; db.create_all()"`
3. Or create a migration script if you have one

### 6. Create Admin User
Create your first admin user by running in the Railway terminal:

```python
from app import app, db
from models import User

with app.app_context():
    # Create admin user
    admin = User(username='admin', email='admin@example.com', role='Admin')
    admin.set_password('your-admin-password')
    db.session.add(admin)
    db.session.commit()
    print("Admin user created!")
```

## File Uploads
- File uploads will work automatically
- Files are stored in the `uploads/` directory
- Railway provides persistent storage for your app

## Troubleshooting

### Common Issues:
1. **Database Connection Error**: Double-check your DATABASE_URL format
2. **Port Issues**: Railway handles PORT automatically, don't hardcode it
3. **Static Files**: Flask will serve static files automatically
4. **Debug Mode**: Keep FLASK_DEBUG=False in production

### Logs:
- Check Railway logs in the project dashboard
- Use Railway's terminal for debugging

## Free Tier Limitations
- Railway's free tier includes:
  - 512MB RAM
  - 1GB disk space
  - Limited bandwidth
- MySQL database is also free with limitations

## Scaling
When ready to scale:
1. Upgrade to paid plan
2. Consider using Railway's persistent volumes for file storage
3. Set up proper backup strategies for MySQL

## Security Notes
- Change SESSION_SECRET to a strong, random string
- Use HTTPS (Railway provides SSL automatically)
- Regularly update dependencies
- Monitor database usage

## Support
- Railway Documentation: https://docs.railway.app/
- Flask Documentation: https://flask.palletsprojects.com/
