# Hyrox AI Training Planner

A full-stack web application that generates personalised AI-driven training plans for [Hyrox](https://hyrox.com) athletes.

## What is Hyrox?

Hyrox is a global fitness race that combines 8 km of running with 8 functional workout stations. This app helps athletes of all levels prepare for race day by producing smart, adaptive training schedules powered by AI.

## Features

- **AI-generated training plans** – receive a periodized plan tailored to your current fitness level, race date, and weekly availability.
- **Progressive overload** – plans automatically scale in volume and intensity as you approach race day.
- **Clean, responsive UI** – built with React and TypeScript for a fast, mobile-friendly experience.

## Tech Stack

| Layer | Technology |
|-------|------------|
| Frontend | React 19, TypeScript, Vite |
| Backend | .NET 10, ASP.NET Core Web API |
| Architecture | Clean Architecture (Domain / Application / Infrastructure / WebApi) |

## Getting Started

### Prerequisites

- [.NET 10 SDK](https://dotnet.microsoft.com/download)
- [Node.js](https://nodejs.org) (LTS recommended)

### Backend

```bash
cd backend
dotnet restore
dotnet build AIAgentDemo.slnx
dotnet run --project src/AIAgentDemo.WebApi
```

The API will be available at `https://localhost:5001` by default.

### Frontend

```bash
cd frontend
npm install
npm run dev
```

The app will be available at `http://localhost:5173` by default.

## Running Tests

```bash
cd backend
dotnet test AIAgentDemo.slnx
```

## Project Structure

```
.
├── backend/
│   └── src/
│       ├── AIAgentDemo.Domain/        # Entities & business rules
│       ├── AIAgentDemo.Application/   # Use cases & interfaces
│       ├── AIAgentDemo.Infrastructure/# Persistence & external services
│       └── AIAgentDemo.WebApi/        # API controllers & DI wiring
└── frontend/
    └── src/                           # React + TypeScript source
```

## License

MIT
