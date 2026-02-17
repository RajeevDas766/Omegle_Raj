# 🎥 Omegle - Video Chat Application

A modern, real-time video chat application that connects random users for peer-to-peer video conversations. Built with Next.js, Node.js, and ZegoCloud for seamless video communication.

## 📋 Table of Contents

- [Features](#features)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Environment Variables](#environment-variables)
- [Running the Application](#running-the-application)
- [How It Works](#how-it-works)
- [Project Components](#project-components)
- [API & WebSocket Events](#api--websocket-events)
- [Deployment](#deployment)
- [Contributing](#contributing)

## ✨ Features

- **Real-time Matching**: Instantly connect with random users via WebSocket
- **Video & Audio Communication**: High-quality video and audio calls powered by ZegoCloud
- **Text Chat**: Send messages during video conversations
- **Random Pairing**: Automatic matching algorithm that pairs waiting users
- **Next Conversation**: Skip to the next random user with one click
- **Responsive Design**: Beautiful UI with Tailwind CSS and Lucide icons
- **Live Status Indicators**: Visual feedback for connection status (idle, waiting, matched)

## 🛠 Tech Stack

### Frontend
- **Next.js 16.1.2** - React framework with App Router
- **React 19.2.3** - UI library
- **TypeScript** - Type-safe JavaScript
- **Tailwind CSS** - Utility-first CSS framework
- **Motion** - Animation library
- **Socket.io Client** - Real-time communication
- **ZegoCloud UIKit** - Video call SDK

### Backend
- **Node.js** - JavaScript runtime
- **Express/HTTP Server** - Web server
- **Socket.io** - Real-time bidirectional communication
- **UUID** - Unique room ID generation
- **Dotenv** - Environment variable management

## 📁 Project Structure

```
Omegle_Raj/
├── web/                          # Frontend (Next.js)
│   ├── src/
│   │   └── app/
│   │       ├── globals.css        # Global styles
│   │       ├── layout.tsx         # Root layout
│   │       ├── page.tsx           # Main page (matching logic)
│   │       └── components/
│   │           ├── Navbar.tsx     # Navigation header
│   │           ├── Footer.tsx     # Footer component
│   │           └── VideoRoom.tsx  # ZegoCloud video integration
│   ├── public/                    # Static assets
│   ├── package.json               # Frontend dependencies
│   ├── tsconfig.json              # TypeScript config
│   ├── next.config.ts             # Next.js configuration
│   └── postcss.config.mjs          # PostCSS configuration
│
└── socket/                        # Backend (Node.js + Socket.io)
    ├── index.js                   # Socket server (matching & events)
    └── package.json               # Backend dependencies
```

## 📦 Prerequisites

Before you begin, ensure you have installed:

- **Node.js** (v16 or higher)
- **npm** or **yarn** package manager
- **ZegoCloud Account** (for video call credentials)

## 🚀 Installation

### Step 1: Clone the Repository

```bash
git clone https://github.com/RajeevDas766/Omegle_Raj.git
cd Omegle_Raj
```

### Step 2: Install Frontend Dependencies

```bash
cd web
npm install
```

### Step 3: Install Backend Dependencies

```bash
cd ../socket
npm install
```

## 🔑 Environment Variables

### Frontend (web/.env.local)

Create a `.env.local` file in the `web/` directory:

```env
NEXT_PUBLIC_SOCKET_URL=http://localhost:5000
NEXT_PUBLIC_ZEGO_APP_ID=your_zego_app_id_here
NEXT_PUBLIC_ZEGO_SERVER_SECRET=your_zego_server_secret_here
```

### Backend (socket/.env)

Create a `.env` file in the `socket/` directory:

```env
PORT=5000
```

### How to Get ZegoCloud Credentials

1. Sign up at [ZegoCloud](https://console.zegocloud.com/)
2. Create a new project
3. Get your **App ID** and **Server Secret**
4. Add them to your frontend `.env.local`

## ▶️ Running the Application

### Development Mode

#### Terminal 1: Start the Socket Server

```bash
cd socket
npm run dev
```

The server will run on `http://localhost:5000`

#### Terminal 2: Start the Next.js Frontend

```bash
cd web
npm run dev
```

The application will be available at `http://localhost:3000`

### Production Build

#### Frontend Build

```bash
cd web
npm run build
npm start
```

#### Backend Production

```bash
cd socket
node index.js
```

## 🔄 How It Works

### User Flow

1. **User Opens App**: User visits the website and lands on the home page
2. **Start Chat**: User clicks "Start Chat" button
3. **Waiting State**: 
   - If no one is available, user enters a waiting queue
   - Server emits "waiting" event
4. **Matched**: 
   - When another user is waiting, they are paired
   - Server generates a unique room ID
   - Both users receive "matched" event with room ID
5. **Video Room**: 
   - Users are redirected to video component
   - ZegoCloud initializes with the room ID
   - Video/audio call begins
   - Text chat is available
6. **Next User**: 
   - User clicks "Next" to skip
   - Current call ends
   - Connection returns to home page
   - User can start a new chat

### Matching Algorithm Flow

```
User Presses Start
    ↓
Is someone waiting?
    ├─ YES → Create Room ID → Emit "matched" to both → Start Call
    └─ NO → Add to waiting queue → Emit "waiting"
```

## 🔌 Project Components

### Frontend Components

#### **page.tsx** (Main Page)
- Manages application state (idle, waiting, matched)
- Handles Socket.io connection
- Emits "start" and "next" events
- Displays UI based on current status

#### **VideoRoom.tsx** (Video Component)
- Integrates ZegoCloud UIKit
- Generates unique user ID
- Creates kit token for authentication
- Manages video room lifecycle
- Handles call cleanup

#### **Navbar.tsx** (Header)
- Navigation and branding
- App name/logo display

#### **Footer.tsx** (Footer)
- Footer information
- Additional links/credits

### Backend Components

#### **socket/index.js** (Socket Server)
- Manages WebSocket connections
- Maintains waiting queue
- Tracks active peer pairs
- Handles events:
  - `start`: User requests to chat
  - `next`: User skips to next user
  - `disconnect`: User leaves

```javascript
// Event Handler Flow
connection
├── start → Check queue → Pair users OR Add to queue
├── next → Cleanup & Notify partner
└── disconnect → Cleanup connections
```

## 📡 API & WebSocket Events

### Frontend Events (Client → Server)

| Event | Payload | Description |
|-------|---------|-------------|
| `start` | - | User initiates a chat session |
| `next` | - | User skips to next conversation |

### Backend Events (Server → Client)

| Event | Payload | Description |
|-------|---------|-------------|
| `waiting` | - | User is in waiting queue |
| `matched` | `{ roomId: string }` | Two users matched, ready for call |
| `partner_left` | - | Connected user disconnected |

## 🌐 Deployment

### Deploy Frontend (Vercel)

```bash
cd web
npm run build
# Push to GitHub and connect to Vercel
```

1. Push code to GitHub
2. Go to [Vercel](https://vercel.com)
3. Import repository
4. Add environment variables in Settings
5. Deploy

### Deploy Backend (Heroku, Railway, or Replit)

**Example: Heroku**

```bash
cd socket
heroku create your-app-name
git push heroku main
```

Set environment variables on hosting platform:
```
PORT=5000
```

**Update Frontend Socket URL** after deployment:
```env
NEXT_PUBLIC_SOCKET_URL=https://your-backend-url.com
```

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📄 License

This project is open source and available under the ISC License.

## 🙋‍♂️ Support

For issues, questions, or suggestions, please open an issue on [GitHub](https://github.com/RajeevDas766/Omegle_Raj/issues).

---

**Made with ❤️ by Rajeev Das**
