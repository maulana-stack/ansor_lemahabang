# Development Guide

## Getting Started

### Prerequisites
- Node.js (v14 or higher)
- npm or yarn
- MongoDB (local or cloud)
- Git

### Installation

1. **Clone the repository**
```bash
git clone https://github.com/maulana-stack/ansor_lemahabang.git
cd ansor_lemahabang
```

2. **Backend Setup**
```bash
cd backend
npm install

# Create .env file
cp .env.example .env

# Edit .env with your MongoDB URI and JWT secret
# MONGODB_URI=mongodb://localhost:27017/ansor_lemahabang
# JWT_SECRET=your_secret_key

# Start development server
npm run dev
```

3. **Frontend Setup** (in a new terminal)
```bash
cd frontend
npm install

# Create .env file
cp .env.example .env

# Start development server
npm run dev
```

The application will be available at `http://localhost:3000`

## Project Structure

### Backend
```
backend/
├── config/          # Database and configuration
├── models/          # MongoDB schemas
├── routes/          # API endpoints
├── middleware/      # Authentication and validation
├── controllers/     # Business logic (to be added)
├── server.js        # Entry point
└── package.json
```

### Frontend
```
frontend/
├── src/
│   ├── components/  # Reusable components
│   ├── pages/       # Page components
│   ├── context/     # Context API providers
│   ├── services/    # API services
│   ├── App.jsx      # Main app component
│   └── index.css    # Global styles
├── public/          # Static files
└── package.json
```

## API Endpoints

### Authentication
- `POST /api/v1/auth/register` - Register new user
- `POST /api/v1/auth/login` - User login
- `GET /api/v1/auth/me` - Get current user

### Projects
- `GET /api/v1/projects` - List projects
- `POST /api/v1/projects` - Create project
- `GET /api/v1/projects/:id` - Get project
- `PUT /api/v1/projects/:id` - Update project
- `DELETE /api/v1/projects/:id` - Delete project

### Tasks
- `GET /api/v1/tasks` - List tasks
- `POST /api/v1/tasks` - Create task
- `GET /api/v1/tasks/:id` - Get task
- `PUT /api/v1/tasks/:id` - Update task
- `DELETE /api/v1/tasks/:id` - Delete task

### Team Management
- `GET /api/v1/team/:projectId` - Get team members
- `POST /api/v1/team/:projectId/invite` - Invite member
- `DELETE /api/v1/team/:projectId/:memberId` - Remove member

### Users
- `GET /api/v1/users` - List users (admin only)
- `GET /api/v1/users/:id` - Get user
- `PUT /api/v1/users/:id` - Update user
- `GET /api/v1/users/search` - Search users

## Development Workflow

1. **Create a new branch for features**
```bash
git checkout -b feature/feature-name
```

2. **Make your changes and commit**
```bash
git add .
git commit -m "Add feature description"
```

3. **Push to repository**
```bash
git push origin feature/feature-name
```

4. **Create a Pull Request on GitHub**

## Testing

### Backend Testing
```bash
cd backend
npm test
```

### Frontend Testing
```bash
cd frontend
npm test
```

## Deployment

### Backend Deployment (Heroku example)
```bash
cd backend
heroku create ansor-lemahabang-api
git push heroku main
```

### Frontend Deployment (Vercel example)
```bash
cd frontend
npm run build
vercel --prod
```

## Troubleshooting

### MongoDB Connection Error
- Check MongoDB service is running
- Verify MONGODB_URI in .env
- Check network connectivity

### CORS Error
- Ensure FRONTEND_URL is correctly set in backend .env
- Check API requests are using correct base URL

### Port Already in Use
```bash
# Kill process on port 5000 (backend)
lsof -ti:5000 | xargs kill -9

# Kill process on port 3000 (frontend)
lsof -ti:3000 | xargs kill -9
```

## Contributing

1. Fork the repository
2. Create a feature branch
3. Commit your changes
4. Push to the branch
5. Create a Pull Request

## License

MIT License - See LICENSE file for details
