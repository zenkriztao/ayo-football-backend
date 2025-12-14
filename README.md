# AYO Football Backend API

A RESTful API for managing football teams, players, matches, and generating reports. Built with Go (Golang) using the Gin framework, following **Clean Architecture** and **SOLID principles**.

## Live API

**Production URL:** https://ayo-football-api-production.up.railway.app

| Endpoint | URL |
|----------|-----|
| Base URL | `https://ayo-football-api-production.up.railway.app` |
| API v1 | `https://ayo-football-api-production.up.railway.app/api/v1` |
| Health Check | `https://ayo-football-api-production.up.railway.app/health` |

<img width="1510" height="817" alt="Screenshot 2025-12-14 at 11 33 41" src="https://github.com/user-attachments/assets/bab9f198-936e-4ccd-9a1b-475a75756a24" />

### Quick Test
```bash
# Health Check
curl https://ayo-football-api-production.up.railway.app/health

# Login
curl -X POST https://ayo-football-api-production.up.railway.app/api/v1/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email":"admin@ayofootball.com","password":"Admin@123"}'
```

### Default Admin Credentials
```
Email: admin@ayofootball.com
Password: Admin@123
```

## Features

- **Team Management**: CRUD operations for football teams
- **Player Management**: CRUD operations for players with jersey number validation
- **Match Scheduling**: Create and manage match schedules
- **Match Results**: Record match results with goal scorers
- **Reports**: Generate match reports with statistics, top scorers, and win counts
- **Authentication**: JWT-based authentication with role-based access control
- **Soft Delete**: All deletions are soft deletes for data integrity

## Technology Stack

- **Language**: Go 1.23+
- **Framework**: Gin Web Framework
- **Database**: PostgreSQL (with MySQL support)
- **ORM**: GORM
- **Authentication**: JWT (JSON Web Token)
- **Architecture**: Clean Architecture / Hexagonal Architecture
- **Containerization**: Docker & Docker Compose
- **Deployment**: Railway.app

## Project Structure

```
ayo-football-backend/
├── cmd/
│   └── api/
│       └── main.go                 # Application entry point
├── internal/
│   ├── config/
│   │   └── config.go               # Configuration management
│   ├── domain/
│   │   ├── entity/                 # Domain entities
│   │   │   ├── base.go
│   │   │   ├── user.go
│   │   │   ├── team.go
│   │   │   ├── player.go
│   │   │   ├── match.go
│   │   │   └── goal.go
│   │   ├── repository/             # Repository interfaces
│   │   │   ├── user_repository.go
│   │   │   ├── team_repository.go
│   │   │   ├── player_repository.go
│   │   │   ├── match_repository.go
│   │   │   └── goal_repository.go
│   │   └── usecase/                # Business logic
│   │       ├── auth_usecase.go
│   │       ├── team_usecase.go
│   │       ├── player_usecase.go
│   │       ├── match_usecase.go
│   │       └── report_usecase.go
│   ├── delivery/
│   │   └── http/
│   │       ├── handler/            # HTTP handlers
│   │       ├── middleware/         # HTTP middlewares
│   │       ├── dto/                # Data transfer objects
│   │       └── router.go           # Route definitions
│   └── infrastructure/
│       ├── database/               # Database implementations
│       └── security/               # JWT service
├── pkg/
│   └── response/                   # Response helpers
├── docs/
│   ├── API_DOCUMENTATION.md        # API documentation
│   └── postman_collection.json     # Postman collection
├── .env.example                    # Environment variables template
├── docker-compose.yml              # Docker compose configuration
├── Dockerfile                      # Docker build file
├── Makefile                        # Build commands
├── go.mod
└── README.md
```

## Getting Started

### Prerequisites

- Go 1.23 or higher
- PostgreSQL 13+ or MySQL 8+
- Docker & Docker Compose (optional)

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/Ayo-Indonesia-sport/ayo-football-backend.git
   cd ayo-football-backend
   ```

2. **Copy environment file**
   ```bash
   cp .env.example .env
   ```

3. **Configure environment variables**
   ```env
   SERVER_PORT=8080
   GIN_MODE=debug

   DB_DRIVER=postgres
   DB_HOST=localhost
   DB_PORT=5432
   DB_USER=postgres
   DB_PASSWORD=password
   DB_NAME=ayo_football
   DB_SSLMODE=disable

   JWT_SECRET=your-super-secret-jwt-key-change-in-production
   JWT_EXPIRATION_HOURS=24

   ADMIN_EMAIL=admin@ayofootball.com
   ADMIN_PASSWORD=Admin@123
   ```

4. **Install dependencies**
   ```bash
   go mod download
   ```

5. **Run the application**
   ```bash
   go run cmd/api/main.go
   ```

### Using Docker

1. **Start with Docker Compose**
   ```bash
   docker-compose up -d
   ```

2. **Stop containers**
   ```bash
   docker-compose down
   ```

### Using Makefile

```bash
# Download dependencies
make deps

# Run the application
make run

# Build the application
make build

# Run tests
make test

# Build Docker image
make docker-build
```

## API Documentation

See the full API documentation in [docs/API_DOCUMENTATION.md](docs/API_DOCUMENTATION.md).

### Quick Start

1. **Login to get JWT token**
   ```bash
   curl -X POST http://localhost:8080/api/v1/auth/login \
     -H "Content-Type: application/json" \
     -d '{"email": "admin@ayofootball.com", "password": "Admin@123"}'
   ```

2. **Create a team**
   ```bash
   curl -X POST http://localhost:8080/api/v1/teams \
     -H "Authorization: Bearer <your_token>" \
     -H "Content-Type: application/json" \
     -d '{
       "name": "Persija Jakarta",
       "logo": "https://example.com/logo.png",
       "founded_year": 1928,
       "address": "Jl. Casablanca No.1",
       "city": "Jakarta"
     }'
   ```

3. **Create a player**
   ```bash
   curl -X POST http://localhost:8080/api/v1/players \
     -H "Authorization: Bearer <your_token>" \
     -H "Content-Type: application/json" \
     -d '{
       "team_id": "<team_uuid>",
       "name": "Marko Simic",
       "height": 185.5,
       "weight": 82.0,
       "position": "forward",
       "jersey_number": 9
     }'
   ```

### Available Endpoints

| Method | Endpoint | Description | Auth |
|--------|----------|-------------|------|
| GET | /health | Health check | No |
| POST | /api/v1/auth/login | Login | No |
| POST | /api/v1/auth/register | Register | No |
| GET | /api/v1/auth/profile | Get profile | Yes |
| GET | /api/v1/teams | Get all teams | No |
| GET | /api/v1/teams/:id | Get team | No |
| POST | /api/v1/teams | Create team | Admin |
| PUT | /api/v1/teams/:id | Update team | Admin |
| DELETE | /api/v1/teams/:id | Delete team | Admin |
| GET | /api/v1/players | Get all players | No |
| GET | /api/v1/players/:id | Get player | No |
| POST | /api/v1/players | Create player | Admin |
| PUT | /api/v1/players/:id | Update player | Admin |
| DELETE | /api/v1/players/:id | Delete player | Admin |
| GET | /api/v1/matches | Get all matches | No |
| GET | /api/v1/matches/:id | Get match | No |
| POST | /api/v1/matches | Create match | Admin |
| PUT | /api/v1/matches/:id | Update match | Admin |
| DELETE | /api/v1/matches/:id | Delete match | Admin |
| POST | /api/v1/matches/:id/result | Record result | Admin |
| GET | /api/v1/reports/matches | Get reports | No |
| GET | /api/v1/reports/matches/:id | Get report | No |
| GET | /api/v1/reports/top-scorers | Get top scorers | No |

## Player Positions

| Position | Indonesian | English |
|----------|------------|---------|
| forward | Penyerang | Forward |
| midfielder | Gelandang | Midfielder |
| defender | Bertahan | Defender |
| goalkeeper | Penjaga Gawang | Goalkeeper |

## Business Rules

1. **Jersey Number**: Each player's jersey number must be unique within their team (1-99)
2. **Team Membership**: A player can only belong to one team at a time
3. **Match Teams**: Home team and away team must be different
4. **Soft Delete**: All deletions use soft delete mechanism for data integrity
5. **Authentication**: Admin role required for create, update, and delete operations

## Testing

```bash
# Run all tests
go test ./...

# Run tests with coverage
go test -coverprofile=coverage.out ./...
go tool cover -html=coverage.out
```

## Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## Deployment

This API is deployed on **Railway.app** with:
- Automatic deployment from GitHub
- PostgreSQL database
- Free tier hosting

### Deploy Your Own
1. Fork this repository
2. Create account at [Railway.app](https://railway.app)
3. Create new project → Deploy from GitHub
4. Add PostgreSQL database
5. Set environment variables (see `.env.example`)
6. Deploy!

## License

This project is licensed under the MIT License.

## Author

Developed by **AYO Indonesia Sport**

- GitHub: [@Ayo-Indonesia-sport](https://github.com/Ayo-Indonesia-sport)
- API: https://ayo-football-api-production.up.railway.app
