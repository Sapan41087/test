# Design Standards & Code Style Guide

## UI/UX Design System

### Color Palette

**Primary Colors (Language-Mapped):**
- **Hindi:** `#FF6B35` (Saffron) - Brand primary
- **Gujarati:** `#4ECDC4` (Teal) - Accent
- **Sanskrit:** `#FFD93D` (Gold) - Premium
- **English:** `#A2D2FF` (Sky Blue) - Secondary

**Neutral Colors:**
- **Background:** `#0A0A0F` (Dark grey, accessibility contrast)
- **Text:** `#E8E8F0` (Off-white, eye-friendly)
- **Border:** `rgba(255,255,255,0.06)` (Subtle dividers)
- **Hover:** `rgba(255,255,255,0.05)` (Interactive feedback)

**Status Colors:**
- **Success:** `#4ECDC4` (Teal) - Completed, ready
- **Error:** `#FF6B6B` (Red) - Failures, warnings
- **Info:** `rgba(255,255,255,0.5)` (Muted white) - Neutral info
- **Warning:** `#FFD93D` (Gold) - Caution, review needed

### Typography

**Font Family:**
```css
/* Literary, elegant */
font-family: 'Crimson Pro', 'Georgia', serif;

/* Technical, code */
font-family: 'JetBrains Mono', monospace;
```

**Font Sizes:**
- **H1:** 2.4rem (240% of 16px base)
- **H2:** 1.8rem
- **H3:** 1.4rem
- **Body:** 1rem
- **Small:** 0.88rem
- **Tiny:** 0.72rem

**Font Weights:**
- Light: 300 (headings)
- Regular: 400 (body text)
- Medium: 500 (emphasis)
- Bold: 600 (labels, highlights)

### Spacing System

**Consistent 8px grid:**
```
4px:   2 × unit
8px:   1 × unit (base)
12px:  1.5 × unit
16px:  2 × unit
24px:  3 × unit
32px:  4 × unit
```

**Usage:**
- **Padding:** 8px, 16px, 24px, 32px
- **Margins:** 12px, 24px, 32px, 48px
- **Gap:** 8px, 12px, 16px

### Buttons

**Primary Button:**
```css
.btn-primary {
  background: linear-gradient(135deg, #FF6B35, #FF8C61);
  color: white;
  padding: 14px 32px;
  font-size: 1.1rem;
  border-radius: 4px;
  transition: all 0.2s;
  border: none;
  cursor: pointer;
}

.btn-primary:hover {
  transform: translateY(-1px);
  box-shadow: 0 8px 24px rgba(255, 107, 53, 0.4);
}
```

**Ghost Button (Secondary):**
```css
.btn-ghost {
  background: transparent;
  border: 1px solid rgba(255, 255, 255, 0.15);
  color: rgba(255, 255, 255, 0.6);
  padding: 10px 24px;
  font-size: 1rem;
  border-radius: 4px;
  transition: all 0.2s;
  cursor: pointer;
}

.btn-ghost:hover {
  border-color: rgba(255, 255, 255, 0.4);
  color: white;
}
```

### Input Fields

**Text Input:**
```css
input[type="text"],
input[type="range"] {
  background: rgba(255, 255, 255, 0.02);
  border: 1px solid rgba(255, 255, 255, 0.08);
  color: #E8E8F0;
  padding: 10px 12px;
  border-radius: 4px;
  font-family: 'Crimson Pro', serif;
}

input[type="range"] {
  -webkit-appearance: none;
  width: 100%;
  height: 4px;
  border-radius: 2px;
  background: rgba(255, 255, 255, 0.1);
  outline: none;
  accentColor: currentColor; /* language.color */
}
```

### Cards

```css
.lang-card {
  border: 1px solid rgba(255, 255, 255, 0.08);
  border-radius: 8px;
  padding: 24px;
  background: rgba(255, 255, 255, 0.02);
  transition: all 0.25s;
  cursor: pointer;
}

.lang-card:hover {
  transform: translateY(-2px);
  background: rgba(255, 255, 255, 0.05);
}

.lang-card.selected {
  border-color: var(--lang-color);
  background: rgba(255, 255, 255, 0.05);
}
```

### Layout System

**Max width:** 860px (optimal reading width)
**Sidebar:** 240px (if added later)
**Margins:** 24px padding on main container
**Grid:** 12-column responsive

---

## JavaScript Code Style

### File Organization

```
src/
├── components/
│   ├── App.jsx
│   ├── LanguageCard.jsx
│   ├── PdfViewer.jsx
│   └── AudioPlayer.jsx
├── services/
│   ├── pdf.js (text extraction, coordinate mapping)
│   ├── audio.js (TTS, MP3 encoding)
│   ├── language.js (language detection)
│   └── download.js (file download helpers)
├── hooks/
│   ├── usePdfDocument.js
│   ├── useAudioPlayback.js
│   └── useLocalStorage.js
├── fixtures/
│   ├── languages.js
│   ├── testData.js
│   └── constants.js
├── styles/
│   └── global.css
└── tests/
    ├── pdf.test.js
    ├── audio.test.js
    └── components.test.jsx
```

### Naming Conventions

**Variables:**
```javascript
// camelCase for variables
const chunkSize = 500;
const audioChunks = [];
const isConverting = false;

// UPPER_SNAKE_CASE for constants
const MAX_FILE_SIZE = 100 * 1024 * 1024; // 100 MB
const LANGUAGES = [/* ... */];
const CHUNK_SIZE_MIN = 100;
```

**Functions:**
```javascript
// camelCase for functions
const extractTextFromPdf = async (pdfBlob) => { /* ... */ };
const detectLanguage = (text) => { /* ... */ };
const mergeAudioBuffers = (buffers) => { /* ... */ };

// Convention: 'get' for fetching, 'fetch' for API, 'set' for updating
const getRemoteTTS = async (text, lang) => { /* ... */ };
const fetchGoogleTTS = async (text, langCode) => { /* ... */ };
const setProgress = (percent) => { /* ... */ };
```

**React Components:**
```javascript
// PascalCase for components
const App = () => { /* ... */ };
const LanguageCard = ({ language, isSelected }) => { /* ... */ };
const PdfViewer = ({ pdfUrl }) => { /* ... */ };

// Props: camelCase
<LanguageCard language={lang} onSelect={setSelectedLang} />

// Event handlers: on* prefix
const onLanguageSelect = (lang) => { setSelectedLang(lang); };
const onClick = () => { /* ... */ };
```

### Code Quality

**Formatting:**
```javascript
// 2-space indentation
const obj = {
  key: "value",
  nested: {
    foo: "bar"
  }
};

// Lines: max 100 characters (except URLs)
const veryLongVariableName = someFunction(
  argumentOne,
  argumentTwo,
  argumentThree
);

// Consistent semicolons (required)
const x = 5;
const fn = () => {};
```

**Comments:**
```javascript
// Use JSDoc for functions with complex logic
/**
 * Extract text from PDF and return coordinate map
 * @param {Blob} pdfBlob - PDF file blob
 * @returns {Promise<{text: string, coordinates: Object[]}>}
 */
const extractTextWithCoordinates = async (pdfBlob) => {
  // Implementation...
};

// Single-line comments for clarification
// Google TTS API has 200-char limit, split longer text
const parts = chunkLongText(text, 200);

// TODO comments for future work
// TODO: Implement coordinate caching in Phase 2
```

**Error Handling:**
```javascript
// Always provide meaningful error messages
try {
  const audio = await getRemoteTTS(text, lang);
} catch (err) {
  addLog(`⚠️ TTS failed: ${err.message}`, 'error');
  console.error('TTS Error:', err); // Also log for debugging
}

// Avoid generic errors
throw new Error('Failed'); // ❌ Too vague
throw new Error('PDF extraction failed: Page 3 has no text'); // ✅ Specific
```

**Async/Await:**
```javascript
// Use async/await (not .then)
const convertToAudio = async () => {
  try {
    const text = await extractText(pdf);
    const chunks = chunkText(text);
    const audio = await synthesizeSpeech(chunks);
    return audio;
  } catch (err) {
    console.error(err);
  }
};

// Avoid callback nesting
// ❌
pdf.then(p => chunks(p).then(c => audio(c)));

// ✅
const pdf = await extractText();
const chunks = chunkText(pdf);
const audio = await synthesizeSpeech(chunks);
```

### React Patterns

**Hooks (Functional Components):**
```javascript
// Always at top level, no loops/conditionals
const App = () => {
  const [step, setStep] = useState(1);
  const [pdf, setPdf] = useState(null);
  const fileRef = useRef(null);

  useEffect(() => {
    // Side effects here
  }, [pdf]); // Include dependencies

  return ( /* JSX */ );
};
```

**Props & Destructuring:**
```javascript
// Destructure props in function signature
const LanguageCard = ({ language, isSelected, onSelect }) => {
  return (
    <div onClick={() => onSelect(language)}>
      {language.name}
    </div>
  );
};

// Use object destructuring for state
const { step, pdf, selectedLang } = component.state;
```

**Conditional Rendering:**
```javascript
// Use ternary for single condition
{isLoading ? <Spinner /> : <Content />}

// Use logical && for true-only
{error && <ErrorMessage msg={error} />}

// Use switch for multiple conditions
{
  status === 'idle' && <Button onClick={start} />
}
{
  status === 'running' && <ProgressBar />
}
{
  status === 'done' && <Success />
}
```

---

## Mobile-First Responsive Design

**Breakpoints:**
```css
/* Mobile-first approach */
@media (min-width: 768px) {
  /* Tablet styles */
}

@media (min-width: 1024px) {
  /* Desktop styles */
}
```

**Example:**
```css
/* Mobile */
.chunk-list {
  display: block;
  padding: 12px;
}

/* Tablet */
@media (min-width: 768px) {
  .chunk-list {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 16px;
  }
}

/* Desktop */
@media (min-width: 1024px) {
  .chunk-list {
    grid-template-columns: 1fr 1fr 1fr;
  }
}
```

**Touch-Friendly Sizes:**
- Min button size: 44px × 44px
- Min touch target padding: 8px
- Swipe gestures: 40px minimum swipe area

---

## Accessibility Standards (WCAG 2.1 AA)

**Color Contrast:**
- Text: ≥4.5:1 contrast ratio
- Large text (18pt+): ≥3:1
- Current implementation: Exceeds standard

**Keyboard Navigation:**
```javascript
// All interactive elements keyboard-accessible
<button tabIndex={0} onKeyDown={(e) => {
  if (e.key === 'Enter' || e.key === ' ') {
    handleClick();
  }
}} />
```

**ARIA Labels:**
```javascript
// Semantic HTML + ARIA
<button
  aria-label="Play audio chunk 1"
  onClick={playChunk}
>
  ▶ Play
</button>

<div role="progressbar" aria-valuenow={50} aria-valuemin={0} aria-valuemax={100}>
  {/* Visual progress bar */}
</div>
```

**Testing:**
- Run axe DevTools (Chrome extension)
- Test with NVDA (Windows screen reader)
- Keyboard-only navigation (no mouse)
- VoiceOver (macOS/iOS)

---

## Performance Standards

**Target Metrics:**
- Page load: <3s
- First interaction: <2s
- PDF render: <1s for 50 pages
- Audio playback start: <1s
- Caption update: <50ms (Phase 2)

**Optimization:**
- Code splitting for components
- Lazy load heavy libraries
- Cache PDF coordinates
- Debounce resize events
- Use requestAnimationFrame for highlights

---

## Documentation

**README for each module:**
```
# pdf.js

Handles PDF text extraction and coordinate mapping

## Usage
```javascript
const text = await extractText(pdfBlob);
const coords = await extractCoordinates(pdfBlob);
```

## Architecture
Uses pdf.js library to render and extract text...

## Edge Cases
- Scanned PDFs may have poor OCR
- Mixed-language PDFs detected per chunk
```

---

**Owner:** Engineering Lead
**Version:** 1.0
**Last Updated:** May 23, 2026
