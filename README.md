# FoodSharePoint

A food marketplace platform connecting vendors (food sellers) with customers. Built with a microservices architecture using separate repositories for each component.

## 🏗️ Project Structure

FoodSharePoint consists of **3 separate git repositories** organized as submodules:

```
foodSharePoint/
├── backend/     # Go API server
├── frontend/    # React web application  
└── mobileapp/  # Flutter mobile application
```

## 🚀 Tech Stack

### Backend (`backend/`)
- **Language**: Go 1.24.x
- **Framework**: Gin v1.11.0
- **ORM**: GORM v1.30.0
- **Database**: PostgreSQL 17
- **Cache/Queue**: Redis + Asynq
- **Auth**: Clerk (JWT)
- **External APIs**: AWS S3, Paystack, Mailjet, Tship, Livepeer, Mux

### Frontend (`frontend/`)
- **Framework**: React 19.2.4
- **Language**: TypeScript 5.9.x
- **Bundler**: Vite 7.3.1
- **Routing**: TanStack Router 1.161.x
- **Data Fetching**: TanStack Query 5.90.x
- **UI**: shadcn/ui + Tailwind CSS 4
- **Forms**: React Hook Form + Zod
- **Auth**: Clerk
- **State**: Zustand

### Mobile App (`mobileapp/`)
- **Framework**: Flutter
- **Language**: Dart
- **Platform**: iOS, Android, Web, Desktop

## 📦 Setup Instructions

### Prerequisites
- Git
- Go 1.24.x (for backend)
- Node.js 18+ (for frontend)
- Flutter SDK (for mobile app)
- Docker & Docker Compose (for database services)

### 1. Clone the Repository

```bash
git clone git@github.com:Playbits/foodSharePoint.git
cd foodSharePoint
```

### 2. Initialize Submodules

```bash
git submodule update --init --recursive
```

### 3. Setup Backend

```bash
cd backend

# Start database and redis
docker-compose -f docker-compose.yml up -d

# Install dependencies
go mod download

# Run migrations
go run main.go migrate

# Start the server
go run main.go
```

Backend will be available at `http://localhost:8080`

### 4. Setup Frontend

```bash
cd frontend

# Install dependencies
pnpm install

# Start development server
pnpm dev
```

Frontend will be available at `http://localhost:5173`

### 5. Setup Mobile App

```bash
cd mobileapp

# Install dependencies
flutter pub get

# Start development (choose platform)
flutter run
```

## 🔌 API Communication

- **Backend API**: `http://localhost:8080/api/v2/`
- **Frontend API**: Uses Axios with Clerk JWT authentication
- **Mobile API**: Uses Dio with Clerk JWT authentication

## 🔐 Authentication

All components use **Clerk** for authentication:
- Frontend: `<ClerkProvider>` + `useAuth()` hook
- Backend: JWT validation via JWKS in middleware
- Mobile: Clerk SDK with local authentication

## 📁 Component Details

### Backend (`backend/`)
- **Architecture**: Handler → Service → Repository pattern
- **Modules**: auth, vendors, sharepoint, bookings, livestreams, payments, logistics, subscriptions, reviews, addressbook, common
- **Entry Point**: `main.go`
- **Routes**: `routes/router.go`

### Frontend (`frontend/`)
- **Architecture**: Feature-based with TanStack Router
- **Routes**: File-based routes in `src/routes/`
- **Features**: Organized in `src/features/`
- **API Hooks**: TanStack Query wrappers in `src/hooks/`
- **Components**: shadcn/ui components in `src/components/ui/`

### Mobile App (`mobileapp/`)
- **Platform**: Cross-platform Flutter application
- **Architecture**: Feature-based modules
- **State Management**: Provider/Bloc pattern
- **Navigation**: GoRouter

## 🧪 Development Commands

### Backend
```bash
cd backend && go run main.go          # Start server
cd backend && go test ./...           # Run tests
cd backend && go mod tidy             # Clean dependencies
```

### Frontend
```bash
cd frontend && pnpm dev              # Start dev server
cd frontend && pnpm build            # Production build
cd frontend && pnpm test             # Run tests
cd frontend && pnpm lint             # Lint code
```

### Mobile App
```bash
cd mobileapp && flutter run         # Start app
cd mobileapp && flutter test        # Run tests
cd mobileapp && flutter analyze     # Analyze code
```

## 📋 Environment Variables

### Backend (`.env`)
```env
# Database
DB_HOST=localhost
DB_PORT=5432
DB_DATABASE=foodsharepoint
DB_USERNAME=postgres
DB_PASSWORD=your_password

# Redis
REDIS_HOST=localhost
REDIS_PORT=6379

# Auth
CLERK_SECRET_KEY=your_clerk_secret
CLERK_JWKS_URL=your_clerk_jwks_url

# External Services
AWS_BUCKET_NAME=your_bucket_name
LIVEPEER_API_KEY=your_livepeer_key
TSHIP_API_KEY=your_tship_key
PAYSTACK_SECRET_KEY=your_paystack_key
```

### Frontend (`.env`)
```env
VITE_API_BASE_URL=http://localhost:8080/api/v2/
VITE_CLERK_PUBLISHABLE_KEY=your_clerk_publishable_key
```

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Commit and push to your fork
5. Create a pull request

## 📄 License

This project is licensed under the MIT License.

## 📞 Support

For support and questions, please open an issue in the respective component repository:
- [Backend Issues](https://github.com/Playbits/foodSharePoint/issues)
- [Frontend Issues](https://github.com/Playbits/foodSharePoint/issues)
- [Mobile App Issues](https://github.com/Playbits/food_share_point_mobile/issues)