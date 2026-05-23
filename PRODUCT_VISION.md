# Product Vision & Lifecycle Plan

## Strategic Vision

**Mission:** Make literary content in Indic languages accessible through high-quality audiobooks that preserve cultural authenticity while enabling modern consumption patterns.

**Target Users:**
- Blind/low-vision readers seeking literary works
- Language learners (Hindi, Gujarati, Sanskrit)
- Busy professionals preferring audio over reading
- Cultural institutions preserving texts
- Educational institutions teaching classical literature

## Product Lifecycle Stages

### Stage 1: MVP (Proof of Concept) ✅ CURRENT
**Timeline:** Months 1-3
**Goal:** Validate core value proposition with single-language, single-user flow

**Deliverables:**
- Single-language PDF→MP3 conversion
- Basic TTS playback
- Manual text sync (condensed display)
- Limited to browser, no persistence

**Success Metrics:**
- ✅ Users can convert a 20-page PDF to audio in <5 min
- ✅ Audio quality acceptable for literary texts
- ✅ Users can download files for offline use

### Stage 2: MVP+ (Enhanced Text Sync) → NEXT PRIORITY
**Timeline:** Months 4-6
**Goal:** Integrate real PDF rendering with synchronized captions

**Key Feature:** PDF Viewer with Live Highlighting
- Display original PDF alongside audio playback
- Highlight current sentence/paragraph in real PDF
- Scroll PDF automatically to follow audio
- Show both original formatting and audio transcript

**Why This Matters:**
- Users see actual typography, formatting, layout
- Maintains cultural authenticity of texts
- Better for scanning, cross-referencing
- Differentiates from condensed text view

**Success Metrics:**
- Sync accuracy: ±200ms between audio and PDF highlight
- 95%+ of PDFs render correctly
- Users prefer PDF sync over condensed view

### Stage 3: Scalable Platform (Multi-User, Cloud Backend)
**Timeline:** Months 7-12
**Goal:** Move from browser-only to full platform

**Key Features:**
- User accounts & project persistence
- Batch processing for large documents
- Multiple TTS engine support
- Advanced audio editing
- Collaboration features

### Stage 4: Production & Monetization
**Timeline:** Month 13+
**Goal:** Self-sustaining platform with revenue model

**Revenue Options:**
1. **Freemium:** Free tier (2 conversions/month), paid tier (unlimited)
2. **B2B:** License to publishers, educational institutions
3. **API:** Developers can embed TTS functionality
4. **White-label:** Libraries/nonprofits rebrand solution

---

## Product Lifecycle: Daily Use Case

### Day 1: User Discovers Vāchana
- Sees landing page explaining feature: "Listen to PDFs while reading"
- Tries uploading a classic Gujarati poem (PDF)
- Selects Gujarati + sees waveform of predicted audio

### Day 2-7: Regular Use
- **Morning:** User plays PDF during commute
  - Listens to audio via Bluetooth
  - PDF highlights follow on tablet when at desk
  - Notes key passages
  
- **Afternoon:** Downloads merged MP3 for offline listening
  
- **Evening:** Shares favorite segments with friends
  - "Listen to this verse in authentic Gujarati"
  - Friends can preview before downloading

### Month 1: Power User Behaviors Emerge
- User converts entire collection (50+ books)
- Expects project organization & playlists
- Wants to adjust voice/speed per book
- Seeks community (share collections, ratings)

### Year 1: Sticky Product
- User has consumed 100+ hours of content
- Has converted personal collection
- Recommends to 5+ friends
- Willing to pay for premium features

---

## Go-to-Market Strategy

### Phase 1: Early Adopters (Months 1-3)
- Target Indian literary enthusiasts on Reddit, Twitter
- Focus: Gujarati + Sanskrit (cultural angle)
- Free with public PDF samples
- Goal: 100 users, 10k downloads

### Phase 2: Expansion (Months 4-9)
- Add Hindi support
- Partner with digital libraries
- Target accessibility advocates
- Goal: 5k users, 500k downloads

### Phase 3: Monetization (Month 10+)
- Introduce freemium model
- B2B licensing to education
- Goal: $10k ARR by month 18

---

## Success Definition by Stage

| Metric | MVP | MVP+ | Platform | Production |
|--------|-----|------|----------|-----------|
| Monthly Users | 50 | 500 | 5k | 50k |
| Audio Quality | Good | Excellent | Excellent | Premium |
| PDF Sync | No | ±200ms | ±100ms | ±50ms |
| User Retention | 20% | 40% | 60% | 70%+ |
| Revenue | $0 | $0 | $1k/mo | $20k+/mo |

---

## Key Dependencies & Risks

| Risk | Impact | Mitigation |
|------|--------|-----------|
| Google TTS rate limits | High | Implement fallback TTS engines early |
| PDF OCR quality varies | High | Pre-flight validation, user feedback loop |
| Browser limitations (audio sync) | Medium | Evaluate Electron/React Native for mobile |
| Licensing (MP3 encoding) | Medium | Monitor lamejs licensing, plan alternative |
| Language-specific bugs | Medium | Early partnership with native speakers |

---

## Success Stories (Target Personas)

### Persona 1: Anaya (Blind Student)
*"I finally can study Sanskrit texts independently"*
- Uses screen reader + audio playback
- Downloads for offline study
- Recommends to 3 classmates

### Persona 2: Rajesh (Heritage Enthusiast)
*"My kids hear Gujarati literature, not just textbooks"*
- Converts family collection
- Shares via WhatsApp
- Suggests feature requests monthly

### Persona 3: Dr. Patel (Educator)
*"My students engage differently with audio texts"*
- Licenses platform for class usage
- Integrates with LMS
- Becomes institutional customer

---

**Next Step:** Implement Stage 2 (PDF Viewer Sync) in next 8 weeks
