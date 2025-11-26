# DOB Performance Tracker

## Overview
This project is a secure performance tracking application designed for DOB team members. Its primary purpose is to provide a platform for tracking content performance, managing employee data, and monitoring workflow progress, including monthly rankings and analytics. The application aims to enhance team efficiency and accountability through comprehensive data insights.

## User Preferences
I prefer iterative development with clear, concise explanations for each step. Please ask for confirmation before implementing major changes or architectural shifts. I value detailed documentation within the code for complex logic. Ensure that the application prioritizes security and data privacy, especially for sensitive employee and performance data.

## System Architecture
The application is built with a modern web stack:
- **Frontend**: React 18 with TypeScript, using Vite for tooling. Wouter is used for routing, and TanStack Query for state management.
- **Backend**: Express.js with TypeScript.
- **UI/UX**: Tailwind CSS is used for styling, complemented by shadcn/ui components for a consistent design system.
- **Data Storage**: Client-side data persistence is handled via LocalStorage.
- **Authentication**: Features a secure login system with user ID and password, enhanced by IP-based access restrictions.
- **Authorization**: Role-based access control (Admin, Super Moderator, Moderator, User) with granular permissions for Super Moderators.
- **Real-time Communication**: Integrated real-time chat functionality using WebSockets, including online status, file sharing (images and videos with unlimited upload size), pinned messages, upload progress tracking, and admin moderation controls.
- **Core Features**: Includes a performance dashboard, voice artist portal, attendance tracking with auto-late detection and employee auto-attendance on login/logout, workflow management, video upload time tracking, employee data management with auto-generated employee credentials, Jela reporter data, performance rankings, and assignment management with file attachments.
- **Admin Panel**: Provides administrative settings, Super Moderator creation, IP Access Control (using ipapi.co), comprehensive app customization controls, feature toggles for modules, customizable page headers for Requisition/Expense sheets, and configurable office start time for attendance tracking.
- **App Customization**: Full branding customization system allowing admins to:
  - Customize app name and logos (header logo, login logo, favicon)
  - Configure theme colors (primary, secondary, background, foreground)
  - Personalize login page (background image, welcome text, developer credit toggle)
  - All customizations persist in localStorage and apply instantly across the application
  - Theme colors automatically convert from hex to HSL for Tailwind CSS compatibility
  - Reset to defaults option available
- **Deployment**: Configured for Replit Autoscale deployment, serving both API and frontend on port 5000.
- **File Upload System**: Multipart file upload support with multer middleware for handling large video files (up to 500MB limit)

## Recent Changes
- **2025-11-21 (Production Hardening)**: Implemented comprehensive production-ready infrastructure
  - **Security Middleware**: Helmet with CSP, CORS with whitelist, rate limiting, compression
  - **Logging Infrastructure**: Winston with daily rotating files, structured JSON logging, separate error logs
  - **Environment Configuration**: Typed config loader with dotenv, .env.example template, validation
  - **Database Optimization**: WAL mode, SQLite optimizations, connection pooling configuration
  - **Health Checks**: /healthz and /readyz endpoints for monitoring and load balancers
  - **Graceful Shutdown**: Proper cleanup of HTTP server, WebSocket connections, and database
  - **Error Handling**: Centralized error middleware, operational error class, uncaught exception handlers
  - **Production Build**: Optimized Vite configuration with code splitting, terser minification
  - **Database Backups**: Automated backup utilities with 30-day retention, scheduled via cron
  - **Process Management**: PM2 ecosystem configuration for cluster mode, auto-restart, health monitoring
  - **Deployment Documentation**: Comprehensive DEPLOYMENT.md with step-by-step production setup
  - **Production Scripts**: npm run build, start, backup-db, pm2:start/stop/restart/logs
  - Replaced all console.log/error with Winston logger throughout server codebase
  - Updated upload directory to use environment configuration
  - Created README.md with quick start and production deployment overview
  
- **2025-11-21 (Initial Setup)**: Successfully set up fresh GitHub import in Replit environment
  - Installed all npm dependencies (470 packages)
  - Created comprehensive .gitignore file for Node.js projects (excluding node_modules, dist, uploads, database temp files, etc.)
  - Configured "Start application" workflow running on port 5000 with webview output
  - Verified application runs successfully with login page displaying correctly
  - Set up Replit Autoscale deployment configuration:
    - Build command: `npm run build`
    - Run command: `tsx server/index.ts`
  - Confirmed frontend properly configured with:
    - Host: 0.0.0.0 (required for Replit proxy)
    - Port: 5000 (frontend only port)
    - allowedHosts: true (required for iframe proxy)
  - Backend Express server configured correctly:
    - Listening on 0.0.0.0:5000
    - Serving both API routes and static frontend in production
  - SQLite database initialized with employee_users and attendance_records tables
  - File upload directory (uploads/chat-files) created with proper gitignore exclusion
  
- **2025-11-20**: Implemented two major features for enhanced employee management and communication
  - **Employee ID Auto-Sync + Auto Attendance System**: Employee users can now log in with auto-generated credentials (8-character alphanumeric passwords). Attendance is automatically recorded on login (check-in) and logout (check-out) with timestamps. Admins retain exclusive editing rights over attendance records. Employee credentials are generated when employee data is updated with name and employee ID.
  - **Chat Box Video Upload + Download Feature**: Supports unlimited video uploads (up to 500MB per file) via REST API endpoints (/api/chat/upload, /api/chat/files/:filename). Features include:
    - Real-time upload progress tracking with visual progress bar
    - In-browser video playback with HTML5 video player
    - Direct download capability with proper MIME type detection
    - Uploaded files stored in server/uploads directory (gitignored)
    - Support for multiple video formats (MP4, WebM, OGG, MOV, AVI)
  - Extended schema with EmployeeUser type for employee authentication
  - Created employeeUserService and attendanceService for data management
  - Updated AuthContext to support employee role and auto-attendance hooks
  - Enhanced ChatContext with file upload API integration and progress callbacks
  
- **2025-11-15**: Implemented three major feature enhancements
  - **Assignment Module**: Complete CRUD system with role-based permissions, priority/status/category management, and file attachment support (upload, download, remove) using base64 encoding for localStorage persistence. Placed beside Chat Box in navigation with feature toggle control.
  - **Editable Page Headers**: Admin-configurable headers for Requisition and Expense sheets including customizable title (H1), subtitle (H3 with month), and "Prepared by" field. Changes persist in localStorage and apply to both display and exports.
  - **Auto-Late Detection**: Intelligent attendance tracking that automatically marks employees as late based on configurable office start time (default 8:00 AM). Late status is recalculated automatically when times change and cannot be manually overridden. Status editing restricted to Admin role only with visible Late indicator badges.
  - Extended shared schema with Assignment type, AdminSettings fields, and SuperModeratorPermissions
  - Updated DashboardHeader with assignment navigation and feature toggle support
  
- **2025-11-15 (Initial)**: Successfully imported from GitHub and configured for Replit environment
  - Installed all npm dependencies
  - Configured development workflow to run on port 5000 with proper host settings
  - Set up autoscale deployment configuration (build: `npm run build`, run: `tsx server/index.ts`)
  - Added comprehensive .gitignore file for Node.js projects
  - Verified application loads correctly with login page functioning

## External Dependencies
- **YouTube Data API v3**: Integrated via the `googleapis` library for fetching video information.
- **WebSocket (ws library)**: Used for real-time chat functionality.
- **Zod**: For schema validation.
- **xlsx**: For Excel export functionality.
- **Radix UI**: Underlying component library for shadcn/ui.
- **Multer**: For handling multipart/form-data file uploads in the chat system.

## Production Deployment
- **Database**: SQLite with WAL mode for better concurrency, automatic backups to `backups/` directory
- **Security**: Helmet, CORS, rate limiting, secure session management, input validation
- **Logging**: Winston with daily rotating files in `logs/` directory (14-day retention for app, 30-day for errors)
- **Process Management**: PM2 cluster mode recommended for production with auto-restart on failure
- **Health Monitoring**: `/healthz` for liveness checks, `/readyz` for readiness probes
- **Environment Variables**: All configuration via .env file (see .env.example)
- **Build Process**: `npm run build` compiles both frontend (Vite) and backend (TypeScript)
- **Deployment**: See DEPLOYMENT.md for comprehensive production deployment guide
- **Backups**: Database backups automated via `npm run backup-db` (schedule with cron)
- **Reverse Proxy**: Nginx configuration included in DEPLOYMENT.md for SSL, WebSocket support

## Important Notes
- **Data Storage**: Application now uses SQLite database (better-sqlite3) for persistent server-side storage
  - Employee credentials and attendance records stored securely in database
  - Database file location configurable via DB_PATH environment variable
  - Automatic WAL mode for better concurrency and crash recovery
- **Security**: Production-ready security implemented:
  - Helmet middleware for security headers
  - Rate limiting to prevent abuse
  - CORS whitelist for trusted domains
  - Session secrets must be configured in production
  - Graceful shutdown ensures data integrity
- **File Uploads**: Unlimited video uploads supported (default 500MB limit configurable)
  - Files stored in UPLOAD_DIR (configurable via environment)
  - Proper MIME type detection and serving
- **WebSocket**: Real-time chat uses WebSocket with proper graceful shutdown handling