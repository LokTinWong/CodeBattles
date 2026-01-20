# CodeBattles

A real-time multiplayer game built with React (Vite) frontend and Flask backend using Socket.IO for real-time communication.

Think of [Leetcode](https://leetcode.com/), but as a party game.

Lok Tin made this game during a hackathon, along with [moromorad](https://github.com/moromorad) and [CrimsonBlade7](https://github.com/CrimsonBlade7), who are his real-life friends. Credits go to them as well. They had a fourth teammate, but he did not contribute at all, hence he receives no credit and will not be named (even if he puts this game on his LinkedIn). To see the full commit history during the making of the game, go [here](https://github.com/moromorad/NewCodeBattles). This repo (not the linked one) is Lok Tin's solo expansion on the game they made during the hackathon.

**IMPORTANT**: As of now, all players, including the host, must be connected to the same WiFi.

See the **Current Bugs or Additional Information** section for other important notes.

## Tech Stack

### Frontend
- **React** (via Vite): Fast development server and build tool
- **Tailwind CSS**: Utility-first CSS framework
- **Zustand**: Lightweight global state management
- **Socket.io-client**: Real-time WebSocket communication

### Backend
- **Python Flask**: Lightweight web framework
- **Flask-SocketIO**: WebSocket support for Flask
- **In-memory state**: No database needed for demo

### Networking
- **Local IP (0.0.0.0)**: Connect devices on same Wi-Fi network

## Setup Instructions

Some of the instructions below may be more tailored for Windows users, but note that you can host or play **CodeBattles** on other operating systems.

### Prerequisites

- **Node.js** (v18 or higher) - [Download](https://nodejs.org/)
- **Python** (v3.8 or higher) - [Download](https://www.python.org/)

>To check your **Node.js** version and **Python** versions, run `node --version` and `python --version` on Command Prompt respectively.

### Frontend Setup

1. Navigate to the frontend directory:
   ```bash
   cd frontend
   ```

2. Install dependencies:
   ```bash
   npm install
   ```

   >If you are given the error "cannot be loaded because running scripts is disabled on this system", run `Set-ExecutionPolicy RemoteSigned` on Command Prompt, then try `npm install` again.

3. Create a `.env` file directly within `frontend`:
   ```bash
   cp .env.example .env
   ```
   Edit `.env` and set `VITE_SOCKET_URL` to your backend URL if needed.

   Inside your `.env` file, you should have:
   ```bash
   VITE_SOCKET_URL=http://host_ip_address:5000
   ```

   > To find the host's IP address, run `ipconfig` on Command Prompt on the hosting device and find the line that says `IPv4 Address`.
   >
   > It is important that the `VITE_SOCKET_URL` of the host and all connecting players are the same in their `.env` files. You must use the host's IP address for everyone.

   Note that it is normal for different devices to have different IPv4 IP addresses, even if they are connected to the same WiFi.

   Note that Git does not track the `.env` file, so each time you clone the repository on a new device, you need to create a new `.env` file.

### Backend Setup

1. Navigate to the backend directory:
   ```bash
   cd backend
   ```

2. Create a virtual environment (recommended):
   ```bash
   python -m venv venv
   
   # On macOS/Linux:
   source venv/bin/activate
   
   # On Windows:
   venv\Scripts\activate
   ```

3. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```

4. Create a `.env` file (optional, defaults are fine for local development):
   ```bash
   cp .env.example .env
   ```

5. Start the Flask server (only for the host):
   ```bash
   python app.py
   ```

   The backend will be available at `http://localhost:5000` (or `http://0.0.0.0:5000` for network access)

## Running the Application

### Local Development

1. The host starts the backend server (in `backend/` directory):
   ```bash
   python app.py
   ```

2. Start the frontend server (in `frontend/` directory):
   ```bash
   npm run dev
   ```

   Every player needs to do this. The host is not necessarily the player, but if the hosting device is also playing, then open a second terminal and run `npm run dev` as well.

3. Open your browser to `http://localhost:5173`

### Network Access (Same Wi-Fi)

If you play **CodeBattles** under a different WiFi connection, remember to update the `.env` file (located in `frontend/.env`) for all devices.

Here is a reminder of what your `.env` file should look like:
```bash
   VITE_SOCKET_URL=http://host_ip_address:5000
   ```

It is extremely important that `host_ip_address` is the hosting device's IPv4 address, not the player's own address. In this context, "host" means the device hosting the backend server, not the game host.

If you can't find your `.env` file (since some IDEs hide it), change directory to `frontend/` with Command Prompt and run `.env`. This will open the `.env` file. To check if you have `.env` in `frontend/`, change directory to `frontend/` with Command Prompt and run `ls`. This will display all files under the current directory.

## Troubleshooting

### Frontend can't connect to backend
- Verify backend is running on port 5000
- Check `VITE_SOCKET_URL` in `.env` matches your backend URL
- Ensure CORS is enabled on backend (already configured)
- Check firewall settings if using a local IP

### Host can connect but non-host devices cannot
- Ensure that the WiFi network profile of the host is set to private. Select the WiFi you are connected to, click Properties, and change Network profile to Private.

### Multiple players not syncing
- Verify all clients are connected to the same backend URL
- Check browser console for Socket.IO errors
- Ensure `broadcast=True` in backend event handlers

### "cannot be loaded because running scripts is disabled on this system" error after running `npm install`

Run `Set-ExecutionPolicy RemoteSigned` on Command Prompt, then try `npm install` again.

## Current Bugs or Additional Information

- **Eventlet** is currently used (not recommended), needs to be migrated to something like `asyncio`.
- Do not reload the page if a game is ongoing. It interrupts the game for everyone and progress is completely lost. You may safely close your own tab though.
- Room code does not currently matter. Rooms are not yet implemented.
- The problem *Container With Most Water* is currently broken.
- There are currently 9 different problems.
- The game host (not the device hosting the backend server, but the player who gets to decide when to start in the game lobby) is the player who types a player name and presses **Join Game** first.
- There is a **Skip** button on each problem for debugging. Pressing it assumes that the problem is done correctly.
- There is a **Debug Menu** on the lower right corner. It allows adjusting time remaining or using any rewards/attacks.

## Development Tips

This section is AI generated. Not sponsored by Lok Tin.

- **Hot Reload**: Both frontend (Vite) and backend (Flask debug mode) support hot reload
- **State Management**: Player positions/actions are stored in Zustand store and synced via Socket.IO
- **CORS**: Backend has CORS enabled for all origins (adjust in production)
- **Debugging**: Check browser console and terminal for Socket.IO connection status