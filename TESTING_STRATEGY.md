# Testing Strategy & QA Plan

## Test Pyramid

```
                    ╱╲
                   ╱  ╲         E2E Tests (5%)
                  ╱────╲        • User workflows
                 ╱      ╲       • Production-like
                ╱────────╲
               ╱          ╲     Integration Tests (20%)
              ╱            ╲    • API calls
             ╱──────────────╲   • Audio processing
            ╱                ╲
           ╱                  ╲ Unit Tests (75%)
          ╱────────────────────╲• Functions
         ╱                      ╲• Components
        ╱                        ╲
```

---

## Unit Tests (75% Coverage)

### Frontend Components
```javascript
// App.test.jsx
describe('App Component', () => {
  test('renders Upload step on initial load', () => {
    // Assert step === 1
  });
  
  test('moves to Language step after PDF upload', () => {
    // Upload PDF → assert step === 2
  });
  
  test('disables Convert button if no language selected', () => {
    // Assert button disabled when selectedLang === null
  });
});

// LanguageCard.test.jsx
describe('LanguageCard', () => {
  test('renders all 4 languages', () => {
    // Assert 4 cards rendered
  });
  
  test('calls setSelectedLang on click', () => {
    // Click → verify callback
  });
  
  test('highlights selected language', () => {
    // Assert className includes "selected"
  });
});
```

### PDF Processing Functions
```javascript
// pdf.test.js
describe('PDF Processing', () => {
  test('extractText returns string', async () => {
    const text = await extractText(testPdfBlob);
    expect(typeof text).toBe('string');
  });
  
  test('chunkText splits by word boundaries', () => {
    const text = "word1 word2 word3 word4";
    const chunks = chunkText(text, 10);
    expect(chunks).toEqual(["word1 word2", "word3 word4"]);
  });
  
  test('detectIndicLanguage identifies Gujarati', () => {
    const lang = detectIndicLanguage("ગુજરાતી");
    expect(lang).toBe('gu');
  });
});
```

### Audio Functions
```javascript
// audio.test.js
describe('Audio Processing', () => {
  test('encodeMp3 produces valid MP3 blob', () => {
    const samples = new Float32Array([...]);
    const blob = encodeMp3(samples, 44100);
    expect(blob.type).toBe('audio/mp3');
  });
  
  test('mergeAudioBuffers concatenates buffers', () => {
    const buf1 = new Float32Array([1, 2, 3]);
    const buf2 = new Float32Array([4, 5, 6]);
    const merged = mergeAudioBuffers([buf1, buf2], 44100);
    expect(merged.samples.length).toBe(6);
  });
  
  test('getRemoteTTS returns audio blob', async () => {
    const blob = await getRemoteTTS("test", { code: 'gu' });
    expect(blob.type).toMatch(/audio/);
  });
});
```

### Test Data
```javascript
// fixtures/testData.js
export const testPdfs = {
  gujarati: { /* Gujarati PDF fixture */ },
  hindi: { /* Hindi PDF fixture */ },
  english: { /* English PDF fixture */ },
  scanned: { /* OCR-scanned PDF fixture */ },
  largeDocument: { /* 100+ page PDF */ },
  malformed: { /* Invalid PDF */ }
};

export const testLanguages = [
  { code: 'gu', sample: 'ગુજરાતી' },
  { code: 'hi', sample: 'हिन्दी' },
  // ...
];
```

---

## Integration Tests (20% Coverage)

### PDF Upload → Conversion Flow
```javascript
describe('PDF Upload & Conversion', () => {
  test('Full workflow: Upload → Language → Convert → Play', async () => {
    // 1. Upload PDF
    const pdf = await uploadPdf(testPdfs.gujarati);
    
    // 2. Extract text
    const text = await extractText(pdf);
    expect(text.length).toBeGreaterThan(0);
    
    // 3. Select language
    const selectedLang = LANGUAGES.find(l => l.code === 'gu');
    
    // 4. Convert to audio chunks
    const chunks = await convertToAudio(text, selectedLang);
    expect(chunks.length).toBeGreaterThan(0);
    
    // 5. Verify playback
    expect(chunks[0].text).toBeDefined();
    expect(chunks[0].blob).toBeDefined();
  });
  
  test('TTS API integration: Request → Response', async () => {
    const blob = await getRemoteTTS("test text", { code: 'gu' });
    expect(blob.size).toBeGreaterThan(1000); // Valid MP3 has size
  });
  
  test('Download flow: Generate → Create blob → Download', async () => {
    const blob = await generateMergedMp3(audioChunks);
    expect(blob.type).toBe('audio/mp3');
    // Verify downloadable
  });
});
```

### Language-Specific Tests
```javascript
describe('Language Support', () => {
  test.each([
    ['gu', 'ગુજરાતી'],
    ['hi', 'हिन्दी'],
    ['sa', 'संस्कृतम्'],
    ['en', 'English']
  ])('Supports %s language', async (code, sample) => {
    const chunks = await convertToAudio(sample, 
      LANGUAGES.find(l => l.code === code));
    expect(chunks.length).toBeGreaterThan(0);
  });
});
```

---

## E2E Tests (5% Coverage)

### Critical User Journeys (Playwright/Cypress)
```javascript
// e2e/audiobook.spec.js
describe('User Journey: Convert PDF to Audiobook', () => {
  
  it('User 1: Upload Gujarati poetry, play, download MP3', async () => {
    // 1. Load page
    await page.goto('http://localhost:3000');
    
    // 2. Drag-drop PDF
    await page.setInputFiles('input[type="file"]', 
      'fixtures/gujarati-poetry.pdf');
    
    // 3. See extracted text
    await expect(page.locator('text=/extracted/i')).toBeVisible();
    
    // 4. Select Gujarati
    await page.click('text=ગુજરાતી');
    
    // 5. Click Convert
    await page.click('button:has-text("Start Conversion")');
    
    // 6. Wait for chunks
    await page.waitForSelector('button:has-text("▶ Play")');
    
    // 7. Click Play
    await page.click('button:has-text("▶ Play")');
    
    // 8. Verify audio plays
    await expect(page.locator('text=/READING NOW/i')).toBeVisible();
    
    // 9. Download MP3
    const downloadPromise = page.waitForEvent('download');
    await page.click('button:has-text("⬇ Download full MP3")');
    const download = await downloadPromise;
    expect(download.suggestedFilename()).toContain('.mp3');
  });
  
  it('User 2: English document, fast speed, zip download', async () => {
    // Similar flow with English PDF
    // Adjust speed slider to 1.5x
    // Download as ZIP
  });
  
  it('User 3: Large 100-page document, verify performance', async () => {
    // Upload large PDF
    // Measure conversion time (<5 min)
    // Verify merged MP3 file is complete
  });
});
```

---

## Test Coverage by Module

| Module | Current | Target | Status |
|--------|---------|--------|--------|
| PDF extraction | 80% | 90% | Good |
| Text chunking | 75% | 85% | Needs work |
| Language detection | 70% | 90% | Priority |
| TTS integration | 60% | 85% | Critical |
| Audio encoding | 50% | 80% | Critical |
| Download functions | 80% | 95% | Good |
| UI components | 40% | 75% | Later |

---

## Manual Testing Checklist

### Pre-Release Testing (Phase 1)

#### PDF Upload
- [ ] Single-page PDF uploads
- [ ] Multi-page PDF (20+ pages)
- [ ] Large PDF (50+ MB)
- [ ] Scanned/OCR PDF
- [ ] Corrupted/invalid PDF shows error
- [ ] Very small file (1KB) handled
- [ ] Progress indicator visible during upload

#### Language Support
- [ ] Gujarati text detected and processed
- [ ] Hindi text detected and processed
- [ ] Sanskrit text detected and processed
- [ ] English text processed
- [ ] Mixed language PDF (auto-detects per chunk)
- [ ] Language selection persists

#### Audio Playback
- [ ] Play button works in Chrome
- [ ] Play button works in Firefox
- [ ] Play button works in Safari
- [ ] Play button works on iPad
- [ ] Play button works on Android
- [ ] Pause works
- [ ] Stop works
- [ ] Speed control (0.5x, 1.5x, 2.0x)
- [ ] Volume control
- [ ] Caption text updates in real-time

#### Download Functions
- [ ] Individual MP3 downloads (chunk 1)
- [ ] ZIP download (all chunks)
- [ ] Merged MP3 download (combined)
- [ ] Downloaded files are valid MP3 (playable in VLC)
- [ ] Filenames are descriptive
- [ ] Files not corrupted (file size reasonable)

#### Error Handling
- [ ] Invalid PDF shows error message
- [ ] Network error (TTS unavailable) shows fallback
- [ ] Browser doesn't have speech synthesis → shows warning
- [ ] Out of memory handled gracefully
- [ ] User cancels midway → cleanup works

#### Mobile Testing
- [ ] Page loads on iPhone 12
- [ ] Page loads on Android 11+
- [ ] Buttons are touch-friendly (44px min)
- [ ] Scrolling is smooth
- [ ] Orientation change (portrait ↔ landscape) works
- [ ] No horizontal scroll on mobile

#### Accessibility
- [ ] Screen reader (NVDA) announces buttons
- [ ] Keyboard navigation works (Tab through controls)
- [ ] Color contrast ≥4.5:1 (WCAG AA)
- [ ] Form labels properly associated
- [ ] Error messages announced
- [ ] Works without sound (captions visible)

#### Performance
- [ ] Page loads in <3 seconds
- [ ] PDF extraction <5 min for 20-page doc
- [ ] Audio playback starts in <1 second
- [ ] No jank during caption updates
- [ ] Memory doesn't leak after repeated uploads

---

## Bug Severity Levels

| Level | Definition | Action | Examples |
|-------|-----------|--------|----------|
| **Critical** | Feature unusable, data loss, security | Fix immediately | PDF upload broken, audio won't play, crash |
| **High** | Feature significantly degraded | Fix in current sprint | Highlighting doesn't sync, wrong language detected |
| **Medium** | Feature works but suboptimal | Fix in next sprint | Slow audio download, UI misalignment |
| **Low** | Minor cosmetic/UX issue | Fix when time allows | Button spacing, typo in text |

---

## Test Environment Setup

### Local Testing
```bash
# Start dev server
npm start

# Run unit tests
npm test

# Run E2E tests
npx playwright test

# Check coverage
npm run test:coverage
```

### Staging Environment
- Pre-release testing with actual users
- Use staging Google TTS API credentials
- Monitor performance metrics
- Collect user feedback via survey

### Production Monitoring
- Sentry for error tracking
- DataDog for performance monitoring
- User session replay (PostHog)
- Analytics dashboard

---

## Regression Test Suite

**Run before each release:**
- [ ] Convert 5 PDFs (different languages)
- [ ] Download all 3 formats (chunk, zip, merged)
- [ ] Play on 3 browsers (Chrome, Firefox, Safari)
- [ ] Test on mobile (iOS + Android)
- [ ] Verify error messages display
- [ ] Check accessibility (screen reader, keyboard)

---

## Performance Benchmarks

| Operation | Target | Current | Status |
|-----------|--------|---------|--------|
| PDF upload | <30s | 15s | ✅ |
| Text extraction | <2min | 1.5min | ✅ |
| Audio generation | <5min | 4min | ✅ |
| Download start | <1s | 0.8s | ✅ |
| Highlight update | <50ms | 200ms | ⚠️ (Phase 2) |

---

**Next Step:** Set up CI/CD pipeline with automatic test runs on each commit
