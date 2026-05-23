# Requirements Document

## Functional Requirements

### MVP Features (Complete ✅)

#### FR-1: PDF Upload & Text Extraction
- **Input:** User selects local PDF file
- **Processing:** Extract all text via pdf.js
- **Output:** Plain text corpus
- **Constraints:**
  - Max file size: 100 MB
  - Supported: Digital PDFs, OCR-scanned PDFs
  - Handles multi-page documents

#### FR-2: Language Selection
- **Languages Supported:**
  - Hindi (हिन्दी)
  - Gujarati (ગુજરાતી)
  - Sanskrit (संस्कृतम्)
  - English
- **Auto-detection:** Option to auto-detect language per chunk
- **Display:** Sample text for each language

#### FR-3: Text-to-Speech Conversion
- **Primary Engine:** Google Translate API
- **Fallback:** Web Speech API (browser native)
- **Constraints:**
  - Max 200 chars per TTS request
  - Auto-chunk longer text

#### FR-4: Audio Playback
- **Web Speech API:** Live preview in browser
- **Controls:** Play, Pause, Stop
- **Sync:** Text caption display
- **Speed Control:** 0.5x to 2.0x

#### FR-5: Download Options
- **Individual Chunks:** MP3 per segment
- **Batch:** ZIP of all chunks
- **Merged:** Single MP3 file (combined)
- **Format:** MP3 (128 kbps)

#### FR-6: Captions (Current)
- **Display:** Condensed text view
- **Update:** Real-time as audio plays
- **Limitation:** Text formatting lost

---

### Phase 2 Features (Planned for MVP+)

#### FR-7: PDF Viewer with Synchronized Highlighting ⭐
- **Display:** Original PDF rendered alongside audio
- **Sync Method:** Text-position coordinate mapping
- **Highlight:** Current sentence/chunk highlighted in real PDF
- **Auto-scroll:** PDF scrolls to follow audio
- **Fallback:** Use condensed text if coordinate matching fails

#### FR-8: Text Position Mapping
- **Extract:** PDF coordinates (x, y, width, height) for each text segment
- **Index:** Map chunk boundaries to PDF locations
- **Accuracy:** ±200ms sync tolerance
- **Validation:** Test on 20+ PDFs (digital + OCR)

#### FR-9: Dual-View Toggle
- **Option A:** Full PDF view with overlay highlights
- **Option B:** Condensed transcript (current)
- **Toggle:** User can switch views at runtime

#### FR-10: Mobile Responsiveness (Phase 2)
- **Layout:** Stack PDF above transcript on mobile
- **Gestures:** Swipe to next segment
- **Accessibility:** Compatible with screen readers

---

### Phase 3+ Features (Future)

#### FR-11: Project Management
- **Persistence:** Save conversion projects locally (IndexedDB)
- **Collections:** Group multiple books
- **History:** Recent conversions

#### FR-12: Advanced TTS Options
- **Engine Selection:** Google, Azure, Edge-TTS
- **Voice Selection:** Multiple voices per language
- **Prosody:** Adjust pitch, rate per segment

#### FR-13: Collaboration
- **Share:** Public/private link sharing
- **Comments:** Annotate highlights
- **Playlists:** Community-curated collections

#### FR-14: User Accounts
- **Auth:** Email/social login
- **Cloud Storage:** Save projects to server
- **Sync:** Access from multiple devices

---

## Non-Functional Requirements

### Performance
- **NFR-1:** PDF upload → audio ready: <5 min for 20-page doc
- **NFR-2:** Highlight update latency: <50ms from audio play
- **NFR-3:** PDF render time: <1s for 50-page document
- **NFR-4:** Memory usage: <200 MB for typical workflow
- **NFR-5:** Cache coordinates: Reuse for same PDF

### Reliability
- **NFR-6:** 99% uptime for TTS API (failover required)
- **NFR-7:** Graceful degradation if Google TTS unavailable
- **NFR-8:** No data loss on browser refresh (session save)
- **NFR-9:** Error recovery: Clear messaging for failures

### Accessibility (WCAG 2.1 AA)
- **NFR-10:** Screen reader compatible
- **NFR-11:** Keyboard navigation for all controls
- **NFR-12:** Color contrast ratio ≥4.5:1
- **NFR-13:** Audio controls have text labels
- **NFR-14:** Works without JavaScript? (graceful fallback)

### Security
- **NFR-15:** PDFs processed locally (no server upload)
- **NFR-16:** No storing user PDFs
- **NFR-17:** API keys for TTS not exposed in client
- **NFR-18:** Content Security Policy headers enforced

### Compatibility
- **NFR-19:** Modern browsers (Chrome 90+, Firefox 88+, Safari 14+)
- **NFR-20:** Mobile (iOS 14+, Android 10+)
- **NFR-21:** Tablet-friendly (iPad, Android tablets)
- **NFR-22:** Network: Works on 4G, degrades gracefully on 3G

### Scalability
- **NFR-23:** Support 1000+ concurrent users (MVP)
- **NFR-24:** Support 100k+ PDFs in library (Phase 3)
- **NFR-25:** Batch processing for bulk conversions

---

## Business Requirements

### BR-1: Language Support Priority
**Priority Order:**
1. Gujarati (primary market)
2. Hindi (largest user base)
3. Sanskrit (cultural significance)
4. English (accessibility + learning)
5. Marathi, Bengali (future)

### BR-2: Content Quality
- **Audio:** Literary texts must sound natural
- **Sync:** Text highlighting must stay in sync
- **Accuracy:** Preserve original meaning

### BR-3: Monetization (Phase 3)
- **Free Tier:** 2 conversions/month
- **Pro Tier:** Unlimited + API access
- **Enterprise:** Bulk licensing

### BR-4: User Acquisition
- **Organic:** SEO for "PDF audiobook", "Gujarati text-to-speech"
- **Partnerships:** Literary communities, accessibility nonprofits
- **Referral:** Share via email/social

---

## User Stories

### Story 1: Blind Student Using Accessibility
**As a** visually impaired student
**I want to** convert my university PDFs to audio
**So that** I can study independently without relying on peer readers

**Acceptance Criteria:**
- ✅ Upload PDF in <30 seconds
- ✅ Audio ready in <5 minutes
- ✅ Download MP3 for offline use
- ✅ Works with screen reader (NVDA)

### Story 2: Heritage Enthusiast Exploring Culture
**As a** Gujarati heritage enthusiast
**I want to** listen to classical poetry while reading original text
**So that** I can appreciate language nuances and pronunciation

**Acceptance Criteria:**
- ✅ PDF displays with proper Gujarati fonts
- ✅ Current line highlighted as audio plays
- ✅ Audio sounds like natural Gujarati (not robotic)
- ✅ Can adjust speed for learning

### Story 3: Educator Creating Course Materials
**As an** Indian literature teacher
**I want to** create multimodal learning materials (audio + text)
**So that** my students can engage with classical texts differently

**Acceptance Criteria:**
- ✅ Batch convert 10 PDFs to audio
- ✅ Download all as merged MP3 files
- ✅ Share links with students
- ✅ Track usage analytics (future)

### Story 4: Commuter Consuming Content
**As a** commuter with limited reading time
**I want to** listen to literary texts during my commute
**So that** I can stay mentally engaged during travel

**Acceptance Criteria:**
- ✅ Download full audiobook as single MP3
- ✅ Works offline on mobile
- ✅ Maintains reading position (future)
- ✅ Supports car audio + Bluetooth

---

## Data Model

```
PDF Document
├─ id: string
├─ filename: string
├─ uploadedAt: timestamp
├─ fileSize: number (bytes)
├─ language: enum (hi|gu|sa|en|auto)
├─ totalPages: number
└─ extractedText: string (raw)

TextChunk
├─ id: number (sequence)
├─ text: string
├─ pageNum: number
├─ language: enum
├─ audioUrl: string (blob URL)
├─ audioBlob: Blob
├─ pdfCoordinates: {
│  ├─ pageNum: number
│  ├─ regions: [
│  │  ├─ x: number (px)
│  │  ├─ y: number (px)
│  │  ├─ width: number (px)
│  │  ├─ height: number (px)
│  │  └─ confidence: 0.0-1.0
│  │ ]
│ }
└─ duration: number (seconds)

Project (Phase 3)
├─ id: string (UUID)
├─ userId: string
├─ name: string
├─ description: string
├─ PDFs: PDF[]
├─ createdAt: timestamp
├─ updatedAt: timestamp
└─ isPublic: boolean
```

---

## Constraints & Assumptions

### Technical Constraints
- **Client-side only:** No server for MVP
- **Browser storage:** Max 50 MB (typical browser limit)
- **Google TTS:** Rate limited (~1000 requests/day)
- **PDF.js:** Limited to what it can extract (no semantic PDFs guaranteed)

### Business Constraints
- **No licensing:** Can only use public/user-provided PDFs
- **No authentication:** MVP doesn't track users
- **No monetization:** Free-only in MVP
- **No SLA:** Best-effort service

### User Constraints
- **Technical skill:** Minimal (non-technical users)
- **Connectivity:** Requires internet for TTS
- **Device:** Desktop/laptop primary (tablet secondary)

---

## Success Metrics

| Metric | MVP Target | MVP+ Target | Platform Target |
|--------|-----------|-----------|-----------------|
| Time to conversion | <5 min | <3 min | <1 min |
| Audio quality (1-5) | 3.5 | 4.5 | 4.8 |
| Sync accuracy | ±500ms | ±200ms | ±50ms |
| User satisfaction | 3.5/5 | 4.2/5 | 4.5/5 |
| Conversion success rate | 85% | 95% | 98% |
| Supports file types | 2 (digital, basic OCR) | 3+ (complex PDFs) | 5+ (images, scans) |

---

## Out of Scope (MVP)

- ❌ User authentication / accounts
- ❌ Cloud storage
- ❌ Batch processing API
- ❌ Video generation
- ❌ Translation features
- ❌ Premium voice synthesis
- ❌ Real-time collaboration
- ❌ Mobile apps (web-responsive only)

---

**Version:** 1.0
**Last Updated:** May 23, 2026
**Owner:** Product Manager
