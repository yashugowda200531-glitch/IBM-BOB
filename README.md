# Floating AI Developer Assistant

An ultra-lightweight, cross-platform (macOS/Windows/Linux) frameless Electron application that floats non-intrusively over any IDE and acts as an ambient, real-time context-aware risk and architecture monitor.

## 🎯 Features

- **Ultra-Lightweight**: < 250MB RAM, < 2% CPU idle
- **Non-Intrusive**: Floating bubble with smart opacity and positioning
- **Real-Time Monitoring**: File changes, Git status, IDE context
- **Risk Assessment**: Visual indicators for code quality and merge conflicts
- **WebSocket Integration**: Persistent connection with fault tolerance
- **Cross-Platform**: Works on macOS, Windows, and Linux

## 🏗️ Architecture

### Core Components

1. **Electron Main Process** (`electron/main.ts`)
   - Secure frameless window management
   - IPC bridge with strict security boundaries
   - Auto-updater integration

2. **Preload Script** (`electron/preload.ts`)
   - Hardened IPC bridge using contextBridge
   - Sanitized event-driven communication
   - No Node.js API exposure to renderer

3. **Local Agent** (`agent/watcher.ts`)
   - Throttled file system watcher (Chokidar)
   - Git context harvester
   - IDE focus detection

4. **WebSocket Manager** (`agent/socket-manager.ts`)
   - Resilient connection with exponential backoff
   - Message queue for offline scenarios
   - Type-safe event handling

5. **React Frontend** (`src/`)
   - Framer Motion animations
   - Zustand state management
   - TailwindCSS styling

## 📦 Installation

```bash
# Install dependencies
npm install

# Development mode
npm run dev

# Build for production
npm run build

# Package for distribution
npm run package        # All platforms
npm run package:win    # Windows
npm run package:mac    # macOS
npm run package:linux  # Linux
```

## 🚀 Usage

### Development

1. Start the development server:
```bash
npm run dev
```

2. The floating bubble will appear in the top-left corner
3. Click to expand and view risk assessment
4. Drag to reposition

### Production

1. Build and package:
```bash
npm run build
npm run package
```

2. Install the generated package from the `release/` directory

## 🔧 Configuration

Configuration is stored in `electron-store` and persists between sessions:

- `windowPosition`: Last window position
- `opacity`: Window opacity (0.1 - 1.0)
- `alwaysOnTop`: Keep window on top
- `serverUrl`: WebSocket server URL

## 🔒 Security

- **Context Isolation**: Enabled
- **Node Integration**: Disabled
- **Sandbox Mode**: Enabled
- **IPC Whitelist**: Only approved channels
- **Data Sanitization**: All incoming data is sanitized

## 📊 Performance Targets

- **RAM Usage**: < 250MB under full load
- **CPU Idle**: < 2% utilization
- **Responsiveness**: 0ms UI blocking
- **Startup Time**: < 2 seconds

## 🎨 UI States

### Collapsed (Bubble)
- 90x90px circular bubble
- Dynamic pulse rings indicating risk level
- Colors: Green (safe), Yellow (warning), Red (critical)
- Smart opacity based on mouse proximity

### Expanded (Panel)
- 400x500px panel with glassmorphism effect
- Risk Center with current status
- Git information (branch, changes, conflicts)
- Recent alerts and warnings
- Active IDE detection

## 🔌 WebSocket Events

### Outgoing (Client → Server)
- `FILE_UPDATED`: File change notification
- `USER_CONTEXT`: Current user context
- `LOCAL_DEPENDENCIES`: Dependency information
- `CURRENT_TASK`: Active task description
- `GIT_STATUS`: Git repository status
- `IDE_CONTEXT`: Active IDE information

### Incoming (Server → Client)
- `API_CHANGED`: API modification alert
- `MERGE_RISK`: Merge conflict warning
- `TEAM_OVERLAP`: Parallel work detection
- `ARCH_WARNING`: Architecture issue
- `DEPENDENCY_UPDATE`: Dependency updates
- `SECURITY_ALERT`: Security vulnerability

## 🛠️ Development

### Project Structure

```
floating-ai-assistant/
├── electron/           # Electron main process
│   ├── main.ts        # Main window management
│   └── preload.ts     # IPC bridge
├── agent/             # Local monitoring agents
│   ├── watcher.ts     # File & Git watcher
│   └── socket-manager.ts  # WebSocket client
├── src/               # React frontend
│   ├── components/    # UI components
│   ├── store/         # Zustand state
│   ├── types/         # TypeScript definitions
│   ├── App.tsx        # Main app component
│   └── main.tsx       # Entry point
├── package.json       # Dependencies
├── tsconfig.json      # TypeScript config
├── vite.config.ts     # Vite config
└── tailwind.config.js # Tailwind config
```

### Adding New Features

1. **New Alert Type**: Add to `SocketEvents` in `socket-manager.ts`
2. **New UI Component**: Create in `src/components/`
3. **New State**: Extend `AppState` in `useAppStore.ts`
4. **New IPC Channel**: Add to whitelist in `preload.ts`

## 🐛 Troubleshooting

### Window Not Appearing
- Check if another instance is running
- Verify screen bounds in electron-store

### High CPU Usage
- Check file watcher throttling settings
- Verify no infinite loops in event handlers

### Connection Issues
- Verify WebSocket server URL
- Check network connectivity
- Review reconnection attempts in logs

## 📝 License

MIT

## 🤝 Contributing

Contributions are welcome! Please follow the existing code style and add tests for new features.

## 📧 Support

For issues and questions, please open a GitHub issue.