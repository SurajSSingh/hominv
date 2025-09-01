# System Architecture

## Overview
Hominv is built as a desktop/mobile application using Tauri framework with a SvelteKit frontend and Rust backend. The architecture follows a client-server pattern where the frontend handles UI and user interactions, while the backend manages data persistence and system operations.

## High-Level Architecture

```
┌─────────────────┐    ┌─────────────────┐
│   SvelteKit     │    │     Tauri       │
│   Frontend      │◄──►│     Backend     │
│                 │    │                 │
│ - UI Components │    │ - Rust Commands │
│ - State Mgmt    │    │ - Data Storage  │
│ - API Calls     │    │ - File System   │
└─────────────────┘    └─────────────────┘
         │                       │
         └───────────────────────┘
              Local Storage
```

## Source Code Structure

### Frontend (`src/`)
- `routes/+page.svelte` - Main application page
- `routes/+layout.ts` - Layout configuration (SSR disabled for SPA mode)
- `app.html` - HTML template
- Static assets in `static/` directory

### Backend (`src-tauri/`)
- `src/main.rs` - Application entry point
- `src/lib.rs` - Tauri commands and application setup
- `Cargo.toml` - Rust dependencies
- `tauri.conf.json` - Tauri configuration

## Key Technical Decisions

### Frontend Architecture
- **SvelteKit with Static Adapter**: Chosen for SPA mode compatibility with Tauri
- **No SSR**: Disabled to work with Tauri's static file serving
- **TypeScript**: For type safety and better developer experience
- **Vite**: Fast development and build tooling

### Backend Architecture
- **Tauri Framework**: Cross-platform desktop/mobile app development
- **Rust**: System programming language for performance and safety
- **Command Pattern**: Frontend invokes backend commands via Tauri's invoke system

### Data Storage
- **Local Storage**: SQLite database or file-based storage for offline capability
- **No Cloud Dependency**: Core functionality works without internet connection

## Component Relationships

### Frontend-Backend Communication
- Frontend calls `invoke()` from `@tauri-apps/api/core`
- Backend exposes commands with `#[tauri::command]` macro
- Commands registered in `tauri::generate_handler![]`

### Data Flow
1. User interacts with Svelte components
2. Components call Tauri commands
3. Commands execute Rust logic
4. Results returned to frontend
5. UI updates based on response

## Critical Implementation Paths

### Item Management
- CRUD operations for inventory items
- Location and category management
- Search and filtering logic

### File Operations
- Photo attachment handling
- Export/import functionality
- Backup and restore features

### UI Components
- Item list and detail views
- Search and filter interfaces
- Photo upload and display
- Barcode scanning integration

## Design Patterns

- **Command Pattern**: For frontend-backend communication
- **Component-Based Architecture**: Svelte's component system
- **State Management**: Svelte 5's `$state` runes for reactive state
- **Repository Pattern**: For data access abstraction (to be implemented)

## Security Considerations

- Local data storage with user-controlled permissions
- No network communication for core features
- File system access limited to user directories
- Input validation on both frontend and backend