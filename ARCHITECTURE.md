# Technical Architecture & Design Options

## Problem Statement

**Current State:** Text is condensed in a sidebar, losing original PDF formatting (fonts, layout, images)

**Goal:** Display real PDF with synchronized captions (highlighting) as audio plays

---

## Option 1: PDF.js Viewer with Overlay Highlights ⭐ RECOMMENDED

### How It Works
1. Display PDF using pdf.js render layer
2. Create invisible text layer from pdf.js text extraction
3. Map chunk text to PDF coordinates
4. Highlight matching text regions as audio plays

### Pros
- ✅ No external dependencies (pdf.js already in use)
- ✅ Preserves original formatting, fonts, images
- ✅ Works for any PDF type
- ✅ Responsive design
- ✅ Fast highlighting (client-side)
- ✅ Good UX (shows original while playing)

### Cons
- ⚠️ Text position matching can be imprecise (~10-15% misalignment on OCR PDFs)
- ⚠️ Complex math to map coordinates across pages
- ⚠️ Requires rebuilding text-to-coordinate index

### Implementation Effort: **3-4 weeks**

### Code Sketch
```javascript
// Step 1: Extract PDF text + coordinates
const pageText = await extractPageTextWithCoordinates(pdfDoc, pageNum);

// Step 2: Match chunk to coordinates
const positions = findTextPositionInPDF(chunk.text, pageText);

// Step 3: Render highlight overlay
<canvas style="position: absolute; top: pdf.top; left: pdf.left;">
  {/* Draw colored rectangles at positions */}
</canvas>
```

**Recommendation:** Start here for MVP+

---

## Option 2: Two-Panel Layout (PDF + Transcript)

### How It Works
- Left panel: PDF (non-interactive)
- Right panel: Transcript with clickable chunks
- Scroll both panels in sync
- Highlight current chunk in transcript

### Pros
- ✅ Simplest to implement (1 week)
- ✅ No coordinate mapping needed
- ✅ Clear UI, easy to understand
- ✅ Works on mobile (stacked vertically)
- ✅ Users can read transcript instead of PDF

### Cons
- ⚠️ Loses PDF formatting (fonts, layout)
- ⚠️ Wider screen needed for desktop
- ⚠️ Same as current "condensed" but side-by-side

### Implementation Effort: **1 week**

**When to Use:** If PDF coordinate mapping proves too difficult

---

## Option 3: Browser PDF Extension + External Sync Service

### How It Works
1. User opens PDF in browser (Chrome/Firefox)
2. Vāchana extension injects sync marker
3. Server maintains sync state
4. When audio plays, extension highlights PDF
5. Same highlight positioning as Option 1, but via extension

### Pros
- ✅ Works with any PDF viewer
- ✅ No dependency on pdf.js
- ✅ Simpler coordinate math

### Cons
- ⚠️ Requires extension (adds friction)
- ⚠️ Not available on iOS/tablets
- ⚠️ Higher complexity (extension + server)
- ⚠️ Browser compatibility issues

### Implementation Effort: **6-8 weeks**

**When to Use:** For future desktop app version

---

## Option 4: Document Restructuring (PDF → Structured HTML)

### How It Works
1. Convert PDF to structured HTML (semantic markup)
2. Annotate each element with audio timestamp
3. Render HTML with synchronized highlighting
4. Style to match original PDF appearance

### Pros
- ✅ Perfect sync accuracy (100%)
- ✅ Accessibility-friendly (proper semantics)
- ✅ Searchable content
- ✅ Mobile-optimized

### Cons
- ⚠️ Requires manual markup for each PDF type
- ⚠️ Very expensive for scanned/OCR PDFs
- ⚠️ Not scalable to arbitrary PDFs
- ⚠️ Requires human review

### Implementation Effort: **10+ weeks** (not scalable)

**When to Use:** For premium/editorial content (curated classics)

---

## Recommended Architecture: Option 1 + Option 2 Hybrid

### Phase 2 Implementation Plan

#### Week 1-2: Enhance PDF Text Extraction
```
Goal: Build precise text-to-coordinate mapping

Steps:
1. Upgrade pdf.js to v4.x (better text layer)
2. Create TextPositionMap class
   - Stores: text, pageNum, x, y, width, height
   - Index all chunk boundaries
3. Test on 10 sample PDFs (digital + OCR)
   - Measure alignment accuracy
   - Identify misalignment patterns
```

#### Week 3: Build PDF Sync Layer
```
Goal: Render PDF with highlight overlay

Steps:
1. Modify renderPDF() to:
   - Keep canvas layer (visual PDF)
   - Add SVG overlay layer (highlights)
   - Sync scroll position
2. Create Highlighter class
   - drawHighlight(coordinates, color)
   - clearHighlight()
   - updateSync() on audio time change
3. Add auto-scroll logic
   - Center current highlight in viewport
   - Smooth scroll transitions
```

#### Week 4: UI Polish & Fallback
```
Goal: Complete MVP+ with graceful degradation

Steps:
1. Add toggle: "Show PDF" vs "Show Transcript"
2. For PDFs that fail coordinate mapping:
   - Fall back to Option 2 (condensed view)
   - Log to Sentry for investigation
3. Mobile responsiveness
   - Stack PDF over transcript
   - Gesture controls
4. Testing on 20+ PDFs
```

#### Week 5: Performance Optimization
```
Goal: <50ms highlight latency

Steps:
1. Profile rendering (Chrome DevTools)
2. Batch SVG updates (don't redraw per frame)
3. Implement virtual scrolling for long docs
4. Cache coordinate maps
```

---

## Current Architecture (As of May 2026)

```
┌─────────────────────────────────────┐
│        Browser (React)              │
├─────────────────────────────────────┤
│ • File Input (PDF)                  │
│ • Language Selection                │
│ • PDF Text Extraction (pdf.js)       │
│ • Text Chunking (500 chars)         │
├─────────────────────────────────────┤
│ Audio Generation                    │
│ ├─ Web Speech API (live preview)    │
│ └─ Google Translate TTS (download)  │
├─────────────────────────────────────┤
│ Audio Playback & Sync               │
│ ├─ SpeechSynthesis (play)           │
│ └─ Captions (condensed text)        │
├─────────────────────────────────────┤
│ Download Options                    │
│ ├─ Individual MP3 (per chunk)       │
│ ├─ ZIP (all chunks)                 │
│ └─ Merged MP3 (lamejs)              │
└─────────────────────────────────────┘

External Dependencies:
├─ pdf.js (CDN) - PDF rendering
├─ Google TTS (API) - TTS source
├─ lamejs (CDN) - MP3 encoding
├─ jszip (CDN) - ZIP packaging
└─ React 18 (CDN) - UI framework
```

---

## Proposed Architecture (Phase 2)

```
┌──────────────────────────────────────────────────────┐
│          Browser (React + Enhanced)                  │
├──────────────────────────────────────────────────────┤
│  Two-View Layout:                                    │
│  ├─ PDF Viewer (pdf.js canvas)                       │
│  │  └─ SVG Overlay (highlight rectangles)           │
│  └─ Transcript Panel (condensed backup)             │
├──────────────────────────────────────────────────────┤
│  Text Coordinate Mapping                             │
│  ├─ TextPositionMap class                            │
│  │  └─ { text, pageNum, x, y, w, h }               │
│  └─ Chunk → Coordinates resolver                    │
├──────────────────────────────────────────────────────┤
│  Real-time Sync Engine                              │
│  ├─ Audio playback time → text position            │
│  ├─ Highlight manager (SVG updates)                │
│  └─ Auto-scroll with smooth transitions            │
├──────────────────────────────────────────────────────┤
│  [Rest of audio generation / download remain same]  │
└──────────────────────────────────────────────────────┘

New Decision Points:
├─ Can extract coordinates? → Use Option 1
├─ Coordinates unreliable? → Fall back to Option 2
└─ User prefers transcript? → Show Option 2 only
```

---

## Technology Evaluation Matrix

| Feature | Option 1 | Option 2 | Option 3 | Option 4 |
|---------|----------|----------|----------|----------|
| PDF Fidelity | 95% | 60% | 95% | 100% |
| Implementation | 3-4w | 1w | 6-8w | 10w+ |
| Sync Accuracy | ±200ms | Perfect* | ±200ms | Perfect |
| Mobile Support | ✅ | ✅ | ❌ | ✅ |
| Scalable | ✅ | ✅ | ⚠️ | ❌ |
| Cost | $0 | $0 | Low | High |

*Perfect because user sees transcript anyway

---

## Risk Mitigation for Option 1

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|-----------|
| Coordinate extraction fails (OCR) | High | Medium | Fallback to condensed view |
| Sync drift over long documents | Medium | Medium | Reset coordinates every 5 min |
| Performance (large PDFs) | Low | High | Virtual scroll + canvas optimization |
| Browser compatibility | Low | Medium | Polyfill SVG overlays |

---

## Success Criteria for Phase 2

1. ✅ 90% of PDFs show highlight within ±300ms of audio
2. ✅ Users prefer PDF view over condensed (survey)
3. ✅ No performance degradation for PDFs >100 pages
4. ✅ Works on tablet (iPad) in landscape mode
5. ✅ Mobile fallback (Option 2) loads <2s

---

**Recommendation:** Proceed with Option 1 (PDF.js overlay) as primary path, Option 2 as graceful fallback.

**Next Step:** Create TextPositionMap prototype in Week 1 of Phase 2
