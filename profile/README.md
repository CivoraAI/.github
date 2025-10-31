In an era where misinformation and bias have clouded the truth and polarized American society, Unspun AI makes the dream of easy, impartial news a reality. The algorithm fetches the latest political news articles from several polarized and central news agencies. It then utilizes advanced NLP algorithms and our novel bias quantification package to measure ideological lean in each outlet. It then extracts core facts, balances all viewpoints, neutralizes sensational language, and uses an LLM to present short, neutral briefs in an interactive, scrollable feed. 
Behind the scenes, Unspun collects 80–90 major articles from outlets across the spectrum—from CNN to Reuters to FOX News, and groups similar articles about the same story using embeddings and linear algebra. From each group, the system derives a factbank - a list of facts agreed upon by a quota of articles and a net bias of |0.5| - plus liberal and conservative claims. We integrated a novel bias quantifier API that analyzes metrics like Fact Coverage, Opposition Coverage, Source Diversity, Loaded Intensity, and sentiment, for a final bias output between ±1. These scores are determined through cosine similarity of embeddings, sentiment analysis, and fact/quote diversity, ensuring transparency and clear logic in the pipeline. 
Unspun AI is unique in the sense that it not only flags bias and splits claims, but rather that it measures it numerically and provides readers with the full picture. Additionally, the pipeline is fully automated, so humans don’t introduce personal bias (they cannot match the neutrality of math). The rewrite LLM is only given the articles in the group as context, so no outside background info from the web is brought into the rewrites, as is the case with many existing LLMS like Chat GPT or Gemini. This bias-removal layer, combined with generative rewriting, allows Unspun to rewrite the core facts in a way that retains accuracy while minimizing spin, slant, sensationalism, and omission; an issue no mainstream aggregator currently solves.
The user sees a clean, scrollable feed of easy-to-read bullet points optimized to engage younger generations like Gen Z and Gen Alpha. We prioritize transparency by providing the user with each original article that was used in the creation of the rewrite and its exact bias metrics, allowing users to verify how bias was measured and mitigated. Unspun AI ultimately transforms the way the next generation consumes information, rebuilding trust in the news and empowering readers with the truth.

# Backend Setup Instructions

## Prerequisites

- Python 3.10+ (Python 3.12 recommended for main backend)
- Python 3.10 (required for civai_bias metrics module)
- Git
- Anaconda/Miniconda (optional, for civai_bias metrics)

### 1. Clone the repository

```bash
git clone https://github.com/CivoraAI/backend.git
cd backend
```

### 2. Create and activate venv

```bash
python3.12 -m venv .venv
source .venv/bin/activate  # On Windows: .venv\Scripts\activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Configure environment variables

Create a `config/.env` file:

```bash
mkdir -p config
cat > config/.env << EOF
APP_ENV=dev
APP_PORT=8000
LOG_LEVEL=info
ALLOW_ORIGINS=http://localhost:5173,http://localhost:3000
DATABASE_URL=postgresql://user:password@localhost:5432/appdb
OPENROUTER_API_KEY=your_openrouter_api_key_here
EOF
```

### 5. Run the development server

```bash
./scripts/run_dev.sh
```

The API will be available at `http://localhost:8000`


## API Endpoints

- `GET /health` - Health check
- `GET /briefs` - Get all news briefs with metadata
- `POST /metrics/score` - Score article bias (body: `{"articles": ["text..."]}`)

## Testing

```bash
pytest tests/
```

## Development

The codebase follows this structure:


# Frontend Setup Instructions

### Prerequisites
- Node.js (v18 or later recommended)
- npm or yarn
- Expo CLI (`npm install -g expo-cli` or use `npx`)
- Expo Go app installed on your iOS/Android device (for physical device testing)
- Backend API running and accessible

### Installation

1. Clone the repository:
```bash
git clone <repository-url>
cd frontend-mobile
```

2. Install dependencies:
```bash
npm install
```

### Configuration

#### API Base URL Setup

The app needs to connect to your backend API. Configure the base URL based on where you're running:

**For iOS Simulator:**
- Uses `http://127.0.0.1:8000` by default (no configuration needed)

**Example:**
```bash
EXPO_PUBLIC_API_BASE=http://192.168.1.19:8000 npx expo start --lan
```

### Running the App

#### Development Server

Start the Expo development server:
```bash
npm start
# or
npx expo start
```

#### For Simulators/Emulators

1. Start the development server
2. Press `i` for iOS Simulator or `a` for Android Emulator
3. Or scan the QR code with your device camera (iOS) or Expo Go app (Android)

#### For Physical Devices

1. Ensure your phone and computer are on the **same Wi-Fi network**
2. Start the server with your LAN IP:
```bash
EXPO_PUBLIC_API_BASE=http://<YOUR_MAC_IP>:8000 npx expo start --lan
```
3. Scan the QR code with:
   - **iOS**: Camera app (will prompt to open in Expo Go)
   - **Android**: Expo Go app

#### Using Tunnel Mode (if LAN doesn't work)

```bash
EXPO_PUBLIC_API_BASE=http://<YOUR_MAC_IP>:8000 npx expo start --tunnel
```

### Backend Requirements

- Backend must be running on port `8000` (or update API base accordingly)
- Backend must bind to `0.0.0.0` (not `127.0.0.1`) for device access:
  - **FastAPI/Uvicorn**: `uvicorn app:app --host 0.0.0.0 --port 8000`
  - **Flask**: `flask run --host 0.0.0.0 --port 8000`
  - **Node/Express**: `app.listen(8000, '0.0.0.0')`
- macOS Firewall: Allow incoming connections on port 8000 (System Settings → Network → Firewall)


# Contact Info
Backend API + business logic for Unspun AI app Point to src/, tests/, scripts/ Contact: Arav Dharnikota(adharnikota@gmail.com; Discord: aravd80) 

Note: Org name remains Civora; however, due to copyright issues, the name was changed to Unspun.
