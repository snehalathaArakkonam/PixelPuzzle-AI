# 🧩 Prompt to Puzzle

<p align="center">
  <img src="https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB"/>
  <img src="https://img.shields.io/badge/TypeScript-007ACC?style=for-the-badge&logo=typescript&logoColor=white"/>
  <img src="https://img.shields.io/badge/Tailwind_CSS-38B2AC?style=for-the-badge&logo=tailwind-css&logoColor=white"/>
  <img src="https://img.shields.io/badge/Appwrite-F02E65?style=for-the-badge&logo=appwrite&logoColor=white"/>
</p>

A web application that leverages multi-modal AI to dynamically generate playable **"Spot the Difference"** games from a single text prompt.

---

## 🌟 Inspiration

The goal was to create a truly interactive and generative experience using a multi-modal AI workflow. Instead of just generating an image or text, this project uses AI as a creative partner in a two-step process: first to create a world (the base image), and then to subtly alter it (the modified image). The challenge of programmatically finding those differences without further AI calls led to an interesting blend of AI generation and classic computer vision techniques.

---

## ✨ Key Features

| Feature | Description |
|--------|-------------|
| 🎨 AI-Powered Game Creation | Describe any scene and the AI generates a unique game for you |
| 🔍 Dynamic Difference Generation | Multi-modal AI intelligently adds or removes elements from a base image |
| ⚡ Client-Side Analysis | Differences detected mathematically in the browser via JavaScript & Canvas APIs |
| ✏️ Manual Editing | Add, remove, or resize differences after automated analysis |
| 💾 Database Persistence | Manually curated games saved to Appwrite for future use or sharing |
| 🎮 Interactive Gameplay | Timer, scoring, and clickable regions to find the differences |
| 🐛 Debug Mode | Visualizes mathematically-found differences before starting the game |
| 📱 Responsive Design | Playable on both desktop and mobile devices |

---

## 🤖 How It Works

### Tech Stack

| Layer | Technologies |
|-------|-------------|
| Frontend | React, TypeScript, Tailwind CSS |
| AI Models | Imagen 4, Gemini 2.5 Flash Image Preview |
| Analysis | JavaScript, HTML Canvas API |
| Backend | Appwrite (Storage + Database) |

### Generation & Analysis Pipeline

```
User Prompt
    │
    ▼
┌─────────────────────────┐
│  Base Image Generation  │  ← Imagen 4
└─────────────────────────┘
    │
    ▼
┌─────────────────────────┐
│  Difference Generation  │  ← Gemini 2.5 Flash Image Preview
│  (3-5 subtle changes)   │
└─────────────────────────┘
    │
    ▼
┌─────────────────────────┐
│   Image Storage         │  ← Appwrite Storage
└─────────────────────────┘
    │
    ▼
┌─────────────────────────┐
│  Pixel-by-Pixel         │
│  Mathematical Analysis  │  ← Canvas API
└─────────────────────────┘
    │
    ▼
┌─────────────────────────┐
│  Region Finding &       │
│  Noise Filtering        │
└─────────────────────────┘
    │
    ▼
┌─────────────────────────┐
│  Manual Curation &      │
│  Database Save          │  ← Appwrite Database
└─────────────────────────┘
    │
    ▼
🎮 Game Ready!
```

1. **Prompt** — User enters a theme (e.g. *"A whimsical fantasy library with floating books"*)
2. **Base Image** — Imagen 4 generates the original high-quality image
3. **Difference Generation** — Gemini 2.5 Flash introduces 3–5 structural changes (object additions/removals), explicitly avoiding simple color or brightness shifts
4. **Storage** — Both images uploaded to Appwrite Storage
5. **Analysis** — Client-side pixel-by-pixel canvas comparison
6. **Region Grouping** — Custom algorithm groups differing pixels into connected, filtered, merged regions
7. **Manual Curation** — User can add, delete, or resize auto-detected differences
8. **Save & Play** — Final game data saved to Appwrite Database, ready to play!

---

## 🚀 Running the Application

### Environment Variables

| Variable | Description |
|----------|-------------|
| `API_KEY` | Your AI API key for Gemini models |
| `APPWRITE_ENDPOINT` | Appwrite API endpoint (e.g. `https://cloud.appwrite.io/v1`) |
| `APPWRITE_PROJECT_ID` | Project ID from Appwrite console |
| `APPWRITE_BUCKET_ID` | Storage Bucket ID for generated images |
| `APPWRITE_DATABASE_ID` | Database ID for the `games` collection |

> **Note:** The app accesses these from the global scope via `(self as any).environment.API_KEY` — not `process.env`.

---

### Appwrite Platform Setup

To allow local development, register your environment as a Web Platform in Appwrite:

1. Go to your project in the **Appwrite Console**
2. Click **Platforms** → **Add Platform** → **New Web App**
3. Set **Hostname** to `localhost`
4. Click **Create**

> ⚠️ Without this step, you'll encounter authentication errors that prevent images from being saved.

---

### Database Setup

**1. Create a Database** in Appwrite Console → Databases. Note the **Database ID**.

**2. Create an API Key** with `databases.write` permission. Note the **Secret**.

**3. Run the setup script:**

```bash
# Install dependencies
npm install ts-node node-appwrite

# Run migration
APPWRITE_ENDPOINT="https://cloud.appwrite.io/v1" \
APPWRITE_PROJECT_ID="<YOUR_PROJECT_ID>" \
APPWRITE_API_KEY="<YOUR_SECRET_API_KEY>" \
APPWRITE_DATABASE_ID="<YOUR_DATABASE_ID>" \
npx ts-node migrations/setup-appwrite.ts
```

This creates the `games` collection with all required attributes.

---

### Local Setup

```bash
# 1. Clone the repository
git clone https://github.com/seehiong/gemini-spot-the-diff-generator.git
cd gemini-spot-the-diff-generator

# 2. Set your environment variables

# 3. Serve the application
# All dependencies load via CDN — no npm install needed for the web app!
```

---

## 🔮 Future Improvements

- [ ] **Game Loading** — Load previously saved games from the database
- [ ] **Difficulty Levels** — Adjust subtlety of AI changes and number of differences
- [ ] **Zen Mode** — A relaxing game mode with no timer
- [ ] **Shareable Games** — Unique URLs for each saved game to challenge friends
- [ ] **Leaderboards** — Global leaderboard for fastest completion times

---

## 📁 Project Structure

```
gemini-spot-the-diff-generator/
├── migrations/
│   └── setup-appwrite.ts    # One-time DB setup script
├── src/
│   ├── components/          # React components
│   └── ...
├── index.html
└── README.md
```

---

## 📄 License

This project is open source. Feel free to fork, extend, and build upon it!
