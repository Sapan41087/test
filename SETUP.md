# Development Setup & Onboarding

## Quick Start (5 Minutes)

### Prerequisites
- Modern browser (Chrome, Firefox, Safari, Edge)
- No Node.js required for MVP
- No build step required

### Running Locally
```bash
# Clone or download the project
cd /path/to/vachana-audiobook-project

# Start a simple HTTP server (Python 3)
python3 -m http.server 8000

# Or using Node.js http-server
npx http-server

# Open browser
open http://localhost:8000
```

### Testing
1. Open `indic-audiobook-test.html` in browser
2. Upload a PDF (use `test-data/sample.pdf`)
3. Select language (Gujarati, Hindi, English, or Sanskrit)
4. Click "Start Conversion"
5. Play a chunk to test audio
6. Download merged MP3

---

## Detailed Setup

### For Frontend Developers

**Option A: No Build (Current MVP)**
```bash
# Files you'll edit:
indic-audiobook-test.html    # Single-file React app

# Libraries loaded from CDN:
- React 18
- Babel standalone
- pdf.js
- lamejs
- jszip

# No dependencies to install
```

**Option B: With Build (Phase 2+)**
```bash
# Initialize Node project
npm init -y

# Install dependencies
npm install react react-dom @babel/core @babel/preset-react
npm install pdf-parse pdf.js lamejs jszip

# Install dev dependencies
npm install -D webpack webpack-cli @babel/loader
npm install -D jest @testing-library/react

# Start dev server
npm start

# Run tests
npm test

# Build for production
npm run build
```

### For Backend Developers (Phase 3+)

**Backend Stack:**
- Node.js 18+
- Express.js
- PostgreSQL 13+
- Redis 7+
- AWS S3

```bash
# Initialize backend project
mkdir vachana-backend
cd vachana-backend
npm init -y

# Install dependencies
npm install express cors dotenv
npm install pg redis
npm install axios (for external APIs)

# Install dev dependencies
npm install -D jest supertest nodemon

# Environment variables
cp .env.example .env
# Edit .env with your credentials

# Start development server
npm run dev

# Run tests
npm test
```

### For DevOps / Deployment (Phase 3+)

```bash
# Docker setup
docker build -t vachana:1.0 .
docker run -p 3000:3000 vachana:1.0

# Kubernetes (if scaling)
kubectl apply -f k8s/deployment.yaml

# AWS deployment
aws s3 cp dist/ s3://vachana-frontend/
aws cloudfront create-invalidation --id XXXXX --paths "/*"
```

---

## Project Structure

```
vachana-audiobook-project/
├── README.md                          # Project overview
├── PRODUCT_VISION.md                  # PM strategy
├── REQUIREMENTS.md                    # Feature specs
├── ARCHITECTURE.md                    # Technical design
├── ROADMAP.md                         # Timeline & phases
├── DESIGN_STANDARDS.md                # Code style
├── TESTING_STRATEGY.md                # QA approach
├── ASSUMPTIONS.md                     # Key assumptions
├── FUTURE_CONSIDERATIONS.md           # Scalability plan
├── PRODUCTION_CHECKLIST.md            # Launch checklist
├── SETUP.md                           # This file
│
├── src/                               # Source code (Phase 2+)
│   ├── components/
│   ├── services/
│   ├── hooks/
│   └── styles/
│
├── indic-audiobook-test.html          # Current MVP
│
├── test-data/                         # Test PDFs
│   ├── sample-gujarati.pdf
│   ├── sample-hindi.pdf
│   └── sample-english.pdf
│
├── docs/                              # Additional documentation
│   ├── API.md                         # API reference (Phase 3)
│   ├── DEPLOYMENT.md                  # Deployment guide
│   └── TROUBLESHOOTING.md             # Common issues
│
├── .github/                           # GitHub configuration
│   └── workflows/                     # CI/CD pipelines
│       └── test.yml
│
└── package.json                       # Node dependencies (if using build)
```

---

## Development Workflow

### Daily Development (Current MVP)

**1. Edit the code:**
```bash
# Edit indic-audiobook-test.html in your editor
open -a "Visual Studio Code" indic-audiobook-test.html
```

**2. Test in browser:**
```bash
# Reload browser or use live reload extension
open http://localhost:8000
```

**3. Make changes:**
- Modify React component logic
- Add new functions
- Update UI styling

**4. Test locally:**
- Upload test PDF
- Verify audio plays
- Check mobile responsiveness

**5. Commit changes:**
```bash
git add indic-audiobook-test.html
git commit -m "Fix play button for English language"
git push origin main
```

### Code Review Process

1. Create feature branch: `git checkout -b feature/pdf-sync`
2. Make changes and test
3. Push to GitHub: `git push origin feature/pdf-sync`
4. Create Pull Request with description
5. Wait for code review (1+ reviewers)
6. Address feedback
7. Merge to main

### Testing Before Commit

```bash
# Manual testing checklist
- [ ] Tested on Chrome
- [ ] Tested on Firefox
- [ ] Tested on Safari
- [ ] Tested on mobile (iPhone)
- [ ] Tested on mobile (Android)
- [ ] No console errors
- [ ] No console warnings (except known)
- [ ] Accessibility check (NVDA or VoiceOver)
- [ ] File downloads work
- [ ] Audio plays correctly
```

---

## Environment Configuration

### MVP (No Configuration Needed)
Browser-based, all APIs called from client

### Phase 3+ (Backend Environment)

Create `.env` file:
```env
# Server
NODE_ENV=development
PORT=3000
DATABASE_URL=postgresql://user:pass@localhost:5432/vachana
REDIS_URL=redis://localhost:6379

# External APIs
GOOGLE_CLOUD_TTS_API_KEY=xxx
AZURE_TTS_API_KEY=xxx

# AWS
AWS_ACCESS_KEY_ID=xxx
AWS_SECRET_ACCESS_KEY=xxx
AWS_S3_BUCKET=vachana-prod

# Authentication
JWT_SECRET=your-secret-key

# Monitoring
SENTRY_DSN=https://xxx@sentry.io/xxx
```

**Never commit `.env` to version control!** Add to `.gitignore`:
```
.env
.env.local
*.log
node_modules/
dist/
build/
```

---

## Debugging & Troubleshooting

### Browser Console Errors

**"pdf.js library failed to load"**
- Check CDN link is accessible
- Try refreshing page
- Check browser console (F12)

**"Google TTS returned 403"**
- Rate limit exceeded
- Check internet connection
- Try again in 5 minutes

**"SpeechSynthesis not available"**
- Browser doesn't support Web Speech API
- Use download instead (external TTS)
- Try Chrome or Firefox

### Performance Issues

**Slow PDF extraction:**
```javascript
// Add timing logs
console.time('pdf-extract');
const text = await extractText(pdf);
console.timeEnd('pdf-extract'); // Shows elapsed time
```

**Memory leak detection:**
```javascript
// Take heap snapshots in Chrome DevTools
// Memory tab → Take snapshot → Compare between operations
// Look for growing references
```

### Common Fixes

| Issue | Fix |
|-------|-----|
| Page won't load | Hard refresh (Cmd+Shift+R) |
| PDF upload fails | Try different PDF or browser |
| Audio won't play | Check audio is enabled, try Chrome |
| Download broken | Check file size, try different format |
| Mobile layout broken | Check viewport meta tag, test responsive |

---

## Testing Your Changes

### Unit Testing (When Added)

```bash
# Run all tests
npm test

# Run specific test
npm test pdf.test.js

# Watch mode (rerun on change)
npm test -- --watch

# Coverage report
npm test -- --coverage
```

### Manual Testing Checklist

Before committing:
- [ ] Feature works as intended
- [ ] No new bugs introduced
- [ ] Error messages are helpful
- [ ] Mobile layout correct
- [ ] Accessibility maintained
- [ ] Performance acceptable
- [ ] No console errors/warnings

### Accessibility Testing

```javascript
// Test with screen reader (free options):
// Windows: NVDA (free)
// macOS: VoiceOver (built-in, Cmd+F5)
// iOS: VoiceOver (Settings > Accessibility)

// Chrome DevTools Accessibility Audit:
// F12 → Lighthouse → Accessibility → Run audit
```

---

## Git Workflow

```bash
# Clone the repository
git clone https://github.com/yourusername/vachana-audiobook.git
cd vachana-audiobook-project

# Create a feature branch
git checkout -b feature/pdf-with-highlighting

# Make changes
# ... edit files ...

# Stage changes
git add indic-audiobook-test.html

# Commit with descriptive message
git commit -m "Add PDF viewer with text highlighting for Phase 2

- Implement pdf.js overlay layer for highlighting
- Add coordinate mapping for text synchronization
- Fallback to condensed view for PDFs without coordinates
- Test on 10 sample PDFs with 95% accuracy"

# Push to GitHub
git push origin feature/pdf-with-highlighting

# Create Pull Request on GitHub
# - Describe changes
# - Link to related issues
# - Request reviewers
```

### Commit Message Convention

```
<type>: <subject>

<body>

<footer>

Types: feat, fix, docs, style, refactor, test, chore
Example:
feat: add PDF coordinate extraction
fix: resolve play button not working for English
docs: update REQUIREMENTS.md with Phase 2 features
```

---

## IDE Setup

### Recommended: Visual Studio Code

**Extensions:**
```
ES7+ React/Redux/React-Native snippets
Prettier (code formatter)
ESLint
GitLens
Thunder Client (REST API testing)
```

**Settings (.vscode/settings.json):**
```json
{
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.formatOnSave": true,
  "files.exclude": {
    "node_modules": true,
    ".git": true
  }
}
```

### Recommended: Browser DevTools

**Chrome/Edge:**
- F12 to open DevTools
- Elements tab: inspect HTML/CSS
- Console tab: JavaScript logs and debugging
- Network tab: API calls and performance
- Performance tab: profiling
- Lighthouse: audit performance/accessibility

**Firefox:**
- F12 to open DevTools
- Inspector tab: HTML/CSS
- Console tab: JavaScript
- Network tab: requests
- Storage tab: localStorage, cookies

---

## Contribution Guidelines

1. **Fork the repository** on GitHub
2. **Create a feature branch** (`feature/your-feature`)
3. **Make commits** with clear messages
4. **Push to your fork**
5. **Create a Pull Request** with description
6. **Wait for review** and address feedback
7. **Merge when approved** by maintainers

### Code Review Checklist (Reviewers)

- [ ] Code follows style guide (DESIGN_STANDARDS.md)
- [ ] Tests pass and coverage maintained
- [ ] Documentation updated if needed
- [ ] No hardcoded values or credentials
- [ ] Performance impact considered
- [ ] Accessibility not degraded
- [ ] Works on mobile and desktop

---

## Resources & Documentation

**Learning Resources:**
- [React 18 Docs](https://react.dev)
- [pdf.js Documentation](https://mozilla.github.io/pdf.js)
- [Web Audio API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Audio_API)
- [Web Speech API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Speech_API)

**Project Documentation:**
- [PRODUCT_VISION.md](PRODUCT_VISION.md) - Strategic direction
- [ARCHITECTURE.md](ARCHITECTURE.md) - Technical design
- [REQUIREMENTS.md](REQUIREMENTS.md) - Feature specifications
- [ROADMAP.md](ROADMAP.md) - Timeline and phases

**Useful Tools:**
- [Chrome DevTools](https://developer.chrome.com/docs/devtools/)
- [Lighthouse](https://developers.google.com/web/tools/lighthouse)
- [Accessibility Checker](https://www.deque.com/axe/devtools/)
- [Can I Use](https://caniuse.com) - Browser compatibility

---

## Getting Help

**Questions?**
1. Check relevant .md file (REQUIREMENTS, ARCHITECTURE, etc.)
2. Search GitHub issues
3. Create new issue with detailed question
4. Ask in discussions or email

**Report bugs:**
1. Describe steps to reproduce
2. Attach screenshots or screencast
3. Include browser/device info
4. Provide error logs from console

---

**Last Updated:** May 23, 2026
**Maintained by:** Vāchana Project Team
