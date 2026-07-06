# Convo Cave

A modular, real-time messaging application designed for scoped group channels, secure direct messaging, and advanced administrative logging.

Convo Cave is a highly responsive, multi-room chat application designed to deliver lightweight, secure, and visually striking real-time messaging. Built on a modular Flask backend and styled with custom neon-mint and deep-purple palettes, the platform provides direct peer-to-peer (DM) and group messaging, automated administrative tracking, and an animated, high-fidelity landing page.

---

## Key Features

### Core Chat Engine
* **WebSocket-Powered Core**: Blazing-fast message syncing across devices powered by Flask-SocketIO.
* **Direct Messaging (DMs)**: Click any registered resident on the sidebar to instantly provision a private, secure 1-to-1 transmission line.
* **Message Seen & Delivery Ticks**:
  * [Sent] (Single check) - Message sent to the server.
  * [Delivered] (Double check, gray) - Message delivered to the recipient's client.
  * [Read] (Double check, neon-blue) - Message read/seen by the recipient.
* **Rich Text & Markdown**: Dynamic client-side Markdown compiling via Marked.js paired with automatic code syntax highlighting via Highlight.js.
* **Reaction Metrics**: Hover over any chat message to apply real-time reaction counters.

### Drag-and-Drop File Sharing
* **Universal Uploads**: Share images, videos, audio clips, and documents.
* **Inline Previews**: Dynamic render handlers automatically construct responsive image thumbnails, local video players, and stylized file-download cards based on file types.
* **Live Progress Tracking**: Custom progressive upload card with live calculation metrics (0% to 100%) and loading bars.

### Customization and Design
* **Interactive Customizer**: Swap accent colors, change wallpapers (Solid Midnight vs. Deep Cave), and adjust interface typography scales instantly.
* **Lucide Icon Integration**: Fully outfitted with modern, lightweight vector symbols to completely replace standard system emojis in UI elements.
* **GSAP Landing Page**: Performance-optimized visual entrance with glowing interactive background spheres.

### Owner Administrative Control
* **System Metrics Dashboard**: Monitor real-time statistics, including active resident counts and overall transmission volumes.
* **Moderation Feed**: View global message logs and dynamically prune messages with one click.
* **Resident Table Directory**: Search the user base and ban/expel profiles from the database instantly.
* **Audit Trail Logs**: A terminal-style security display logging timestamps, actions (sign-ups, logins, logouts), and originating IP addresses.

---

## Technology Stack

| Layer | Technologies |
| :--- | :--- |
| **Backend Engine** | Python 3.8.10, Flask |
| **Realtime Sync** | Flask-SocketIO (WebSockets) |
| **Database ORM** | Flask-SQLAlchemy (SQLite) |
| **Security & Auth** | Flask-Login, Werkzeug Security (PBKDF2 Hashing) |
| **UI Styling** | Tailwind CSS, Custom CSS Variables |
| **Animations** | GSAP (GreenSock Animation Platform) |
| **Client Compilers** | Marked.js, Highlight.js, Lucide Icons CDN |

---

## Project Structure

```text
convo-cave/
├── app.py                   # Main Flask backend and WebSocket handlers
├── models.py                # Relational database models (12 tables)
├── requirements.txt         # Project package dependencies
├── vercel.json              # Vercel deployment configurations
├── static/                  
│   ├── logo.png             # Custom Convo Cave logo and Favicon
│   └── uploads/             # Locally stored user uploads (Images/Files)
└── templates/              
    ├── base.html            # Shared layout with Favicon and Glow styling
    ├── landing.html         # Interactive GSAP landing page
    ├── login.html           # Secure credential login page
    ├── signup.html          # Registration form (Country, Language, Bio)
    ├── dashboard.html       # Dual-panel WhatsApp-style chat interface
    └── admin.html           # Owner systems operations control panel
```

---

## Relational Database Schema (models.py)

The database relies on SQLite managed through SQLAlchemy. The tables are structured as follows:

* **users**: Manages primary credential logins, password hashes, and administration flags.
* **profiles**: Tracks metadata such as display names, avatars, bios, countries, and timezones.
* **user_settings**: Stores custom UI themes, accent colors, and privacy settings.
* **chats**: Handles both public group chats and private 1-to-1 rooms.
* **messages**: Logs transmissions, timestamps, file metadata (URLs, names, sizes), and seen/delivered tick statuses.
* **message_reactions**: Links emojis applied by users to messages.
* **contacts**: Handles secure user friendship lists and block lists.
* **calls**: Logs and routes peer-to-peer VoIP or video call sessions.
* **notifications**: Dispatches system updates, mentions, or calling alerts.
* **reports**: Moderation flag logs for spam, abuse, or copyright violations.
* **audit_logs**: Crucial terminal-trail tracking login histories, suspensions, and IP metadata.

---

## Local Installation Guide

To set up Convo Cave on your machine (optimized for Windows 7 with Python 3.8.10):

### 1. Clone the Repository
```cmd
git clone https://github.com/your-username/convo-cave.git
cd convo-cave
```

### 2. Initialize and Activate Virtual Environment
```cmd
python -m venv venv
venv\Scripts\activate
```

### 3. Install Dependencies
```cmd
pip install -r requirements.txt
```

### 4. Launch the Server
```cmd
python app.py
```

### 5. Access the Platform
Open your browser and navigate to: `http://127.0.0.1:5000`

---

## Deployment Configuration

Convo Cave is pre-configured with a `vercel.json` file to be compatible with serverless routing. 

### Serverless Hosting Note (Vercel)
Vercel runs on serverless architecture, which means the filesystem is read-only at runtime, and containers spin down frequently. Under this hosting model:
1. SQLite records will reset to default states when container instances recycle.
2. WebSockets will fallback to HTTP long-polling due to the short lifespan of serverless functions.

### Persistent Hosting (Recommended)
For full, persistent database storage and sustained real-time WebSocket connections, we recommend deploying Convo Cave to platforms with persistent server processes, such as:
* **Render.com**
* **Railway.app**
* **Fly.io**

To deploy to these services, connect your GitHub repository directly to their dashboard. They will automatically build the application using your `requirements.txt` and start the server using:
```cmd
gunicorn --worker-class eventlet -w 1 app:app
```

---

