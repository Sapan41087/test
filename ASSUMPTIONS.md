# Assumptions & Constraints

## Business Assumptions

### BA-1: Market Demand
**Assumption:** There is significant demand for audiobooks in Indian languages
**Evidence:**
- Blind population in India: ~8 million (accessibility angle)
- Hindi speakers learning Sanskrit: 2+ million
- Gujarati diaspora: 10+ million globally
- Audiobook market growing 20% YoY

**If False:** Pivot to enterprise/educational licensing

### BA-2: PDFs as Primary Format
**Assumption:** Users have literary content in PDF format
**Evidence:**
- Most digital books distributed as PDF
- Educational materials are PDFs
- Legal documents, manuscripts are PDFs
- Easy for users to export from Word/Google Docs

**If False:** Add DOCX, EPUB, TXT support

### BA-3: Google TTS is Sufficient Quality
**Assumption:** Google Translate TTS is acceptable for literary texts
**Evidence:**
- Google TTS is free and relatively good quality
- Many successful products use Google TTS
- Users prefer "good enough" free over premium paid

**If False:** Integrate AI4Bharat or ElevenLabs (higher cost)

### BA-4: Browser-Based is Viable
**Assumption:** Browser-only approach (no native app) is acceptable for MVP
**Evidence:**
- Web apps are faster to build (no app store approval)
- Works on all devices (phone, tablet, desktop)
- Can upgrade to Electron/React Native later
- React works well in browsers

**If False:** Build native apps for iOS/Android (6+ months more)

### BA-5: Users Will Share Content
**Assumption:** Word-of-mouth and social sharing drive user acquisition
**Evidence:**
- Literary communities are engaged
- People share favorite texts with friends
- Accessibility features attract advocates
- Cultural content sparks discussions

**If False:** Need more paid marketing (higher CAC)

### BA-6: Privacy-First Approach Acceptable
**Assumption:** Users accept that PDFs are NOT stored (local processing only)
**Evidence:**
- Privacy-conscious users prefer local processing
- No GDPR/data compliance overhead
- Less infrastructure cost
- Trust advantage for cultural content

**If False:** Build cloud storage (adds cost + compliance)

---

## Technical Assumptions

### TA-1: pdf.js Sufficient for Text Extraction
**Assumption:** pdf.js can extract text accurately from most PDFs
**Evidence:**
- pdf.js is industry-standard
- Works with digital and OCR PDFs
- Handles multi-language texts
- Active maintenance

**Risk:** Scanned PDFs without good OCR may have poor text quality
**Mitigation:** Test on 50+ PDFs, identify failure patterns, document limitations

### TA-2: Google TTS Rate Limits Manageable
**Assumption:** Google TTS API allows ~500-1000 requests/day without payment
**Evidence:**
- API is officially documented
- Free tier supports reasonable usage
- Can batch requests to optimize

**Risk:** High user load → rate limit hits → conversion fails
**Mitigation:** Implement request queue, show user "waiting..." message, upgrade to paid if needed

### TA-3: Web Speech API Works Across Browsers
**Assumption:** SpeechSynthesisUtterance works in Chrome, Firefox, Safari, Edge
**Evidence:**
- Part of W3C standard
- Implemented in all modern browsers
- Fallback: Google TTS for download anyway

**Risk:** Some browsers have bugs or missing voices
**Mitigation:** Test on 5+ browsers, provide error messages, document browser support matrix

### TA-4: lamejs Sufficient for MP3 Encoding
**Assumption:** lamejs can encode audio into valid MP3 files
**Evidence:**
- lamejs is JavaScript port of LAME encoder
- Works in browsers
- Used by other web audio projects

**Risk:** Licensing uncertainty (LAME is LGPL)
**Mitigation:** Monitor licensing, plan fallback to WAV encoding or server-side encoding

### TA-5: Client-Side Performance Acceptable
**Assumption:** JavaScript can handle 20+ MB files and 50+ audio chunks
**Evidence:**
- Modern browsers support 200+ MB memory
- V8/SpiderMonkey engines are fast
- Web Workers available for heavy lifting

**Risk:** Very large documents (200+ pages) may be slow
**Mitigation:** Implement streaming, add progress indicators, warn user of large files

### TA-6: Coordinate Mapping Achievable (Phase 2)
**Assumption:** pdf.js text layer coordinates can be used to highlight text as audio plays
**Evidence:**
- pdf.js exposes textContent with bounding boxes
- Coordinate system is predictable (CSS pixels)
- Can create SVG overlay on canvas

**Risk:** OCR PDFs have poor coordinate accuracy
**Mitigation:** Test accuracy on 50 PDFs, fallback to condensed view if <70% match

---

## User Assumptions

### UA-1: Non-Technical Users
**Assumption:** Users can upload PDF without technical knowledge
**Evidence:**
- Drag-drop interface is intuitive
- No terminal/CLI needed
- No config files
- Clear error messages

**Risk:** Confusion around file formats, language selection
**Mitigation:** Add tooltips, provide examples, user guide

### UA-2: Users Have Internet Connection
**Assumption:** Users have reliable internet (except for final download)
**Evidence:**
- Target users have smartphones/computers
- TTS requires internet (Google API)
- Download-then-offline is supported

**Risk:** Offline-first users can't convert
**Mitigation:** Document requirement clearly, plan local TTS fallback for Phase 3

### UA-3: Users Value Accessibility
**Assumption:** Blind/low-vision users are motivated audience
**Evidence:**
- 8 million blind Indians
- Limited audiobooks in Indian languages
- Strong advocacy communities
- Emotional connection to literature

**Risk:** Accessibility bugs harm vulnerable population
**Mitigation:** Early beta with screen reader users, hire accessibility consultant

### UA-4: Users Know Their Language
**Assumption:** Users can identify language of their PDF
**Evidence:**
- Most users speak one language
- Auto-detection helps unsure users
- Language selector is simple

**Risk:** Confusion if PDF is mixed-language
**Mitigation:** Auto-detect per chunk, allow overrides

### UA-5: Users Want Full Document as One Audio
**Assumption:** Users prefer merged MP3 over 20+ separate files
**Evidence:**
- Similar to audiobook listening experience
- Easier to share
- Works in car audio systems
- Streaming-like experience

**Risk:** Very large merged files (200+ MB) may be slow to download
**Mitigation:** Offer both options, warn user of file size

---

## Product Assumptions

### PA-1: Market Size: 10,000+ Users in Year 1
**Assumption:** Can reach 10k users with organic growth + community outreach
**Evidence:**
- Indian language enthusiasts: 50+ million
- Accessibility community: 500k+
- Educational institutions: 50k+
- Low CAC with organic growth

**If False:** Pivot to enterprise/institutional sales

### PA-2: Pricing: $10/month sustainable
**Assumption:** Users willing to pay $10/month for premium features
**Evidence:**
- Audible charges $14.95/month
- Competitors charge similar
- Educational institutions have budgets

**If False:** Lower price to $5 or find B2B revenue instead

### PA-3: Retention: 60% month-over-month (Phase 3)
**Assumption:** Users come back regularly once they have projects
**Evidence:**
- Literary engagement is recurring
- Projects create "sticky" behavior
- Audio format encourages re-listening

**If False:** Need stronger engagement features (gamification, community)

### PA-4: B2B Opportunity
**Assumption:** Educational institutions will license platform
**Evidence:**
- Digital transformation in Indian schools
- Accessibility requirements (AODA)
- Teacher interest in multimedia materials
- Government tech initiatives

**If False:** Focus on consumer only, no enterprise sales team

---

## Risk Assumptions

### RA-1: No Google API Terms of Service Violation
**Assumption:** Using Google Translate TTS within ToS for non-commercial use
**Evidence:**
- TTS API is documented
- Non-commercial academic use is allowed
- MVP is free

**Risk:** Google could block IP, require auth, change ToS
**Mitigation:** Monitor ToS changes, implement fallback engines early, watch for API changes

### RA-2: No Copyright Infringement Risk
**Assumption:** Users provide their own PDFs or public domain texts
**Evidence:**
- We don't store PDFs
- Users responsible for content
- No distribution of copyrighted works

**Risk:** User uploads copyrighted material, holds us liable
**Mitigation:** Add ToS clause, monitor for abuse, DMCA process in Phase 3

### RA-3: No Licensing Issues with lamejs
**Assumption:** Using lamejs LGPL doesn't create problems
**Evidence:**
- lamejs is LGPL-2.1
- LGPL allows dynamic linking in web context
- Used by other web projects

**Risk:** LGPL compliance issue or patent claims
**Mitigation:** Monitor licensing forums, prepare fallback encoder, plan Phase 4 fallback

### RA-4: Browser APIs Remain Stable
**Assumption:** Web Speech API, pdf.js, Web Audio API won't break
**Evidence:**
- All are W3C standards or stable libraries
- Well-maintained projects
- Browser vendors committed

**Risk:** Browser deprecates Web Speech API
**Mitigation:** Have fallback plan (server-side TTS), watch browser roadmaps

---

## Dependency Assumptions

### DA-1: pdf.js Maintained
**Assumption:** pdf.js continues to receive updates and security fixes
**Evidence:**
- Maintained by Mozilla
- Used by Firefox
- Active community

### DA-2: Google TTS API Stays Free/Cheap
**Assumption:** Google continues to offer free TTS API
**Evidence:**
- Strategic product for Google
- Part of Translate ecosystem
- No shutdown announced

### DA-3: lamejs Works in All Browsers
**Assumption:** JavaScript MP3 encoding works across Chrome, Firefox, Safari, Edge
**Evidence:**
- Used in production by other projects
- Pure JavaScript (no native code)

### DA-4: Internet Availability
**Assumption:** Users have internet access during conversion
**Evidence:**
- 500+ million internet users in India
- Target users likely have smartphone/computer
- Can defer to Phase 3 for offline support

---

## Validation Plan

### Validate BA-1 (Market Demand)
**Method:** Launch to 100 beta users, measure NPS
**Timeline:** Week 1-4 of launch
**Success:** NPS ≥ 30, 80%+ retention after 1 week
**Action if False:** Pivot to different user segment or feature

### Validate TA-2 (Google TTS Rate Limits)
**Method:** Load test with 100 concurrent users
**Timeline:** Week 2-3 of Phase 1
**Success:** No rate limit errors, <5% failures
**Action if False:** Implement request queue, upgrade to paid API

### Validate TA-6 (Coordinate Mapping)
**Method:** Extract coordinates from 50 PDFs, measure accuracy
**Timeline:** Week 1-2 of Phase 2
**Success:** 95%+ successful mapping, ±200ms sync
**Action if False:** Use condensed text fallback for Phase 2 MVP+

### Validate PA-1 (10k Users Year 1)
**Method:** Track signups monthly, model growth
**Timeline:** Months 1-12
**Success:** Reach 10k users organically
**Action if False:** Increase marketing spend, reduce CAC, find partners

### Validate PA-3 (60% Retention)
**Method:** Cohort retention analysis
**Timeline:** After 3 months Phase 1
**Success:** 60%+ users return
**Action if False:** Improve UX, add engagement features

---

## Constraints & Limitations

### Technical Constraints
- **No server (MVP):** All processing in browser
- **PDF format:** Only PDF input (DOCX/EPUB later)
- **Storage:** Limited to browser storage (50 MB)
- **Offline:** Requires internet for TTS
- **License:** Using open-source projects (licensing compliance)

### Business Constraints
- **No paid TTS (MVP):** Only free APIs
- **No user data:** No tracking/analytics
- **No auth:** No user accounts (MVP)
- **Limited languages:** 4 languages (20+ available later)
- **Manual operation:** No automation/batch processing

### Legal Constraints
- **Copyright:** Users responsible for content
- **Terms of Service:** Google TTS compliance
- **Accessibility:** WCAG 2.1 AA target
- **Data Privacy:** GDPR-compliant (no data storage)

---

## Assumptions Changelog

| Date | Assumption | Status | Notes |
|------|-----------|--------|-------|
| May 22, 2026 | BA-1: Market demand exists | Active | Validating with MVP |
| May 22, 2026 | TA-2: Google TTS sufficient | Active | Monitoring rate limits |
| May 23, 2026 | TA-6: Coordinate mapping works | Unvalidated | Phase 2 work |

---

**Owner:** Product Manager
**Last Updated:** May 23, 2026
**Review Frequency:** Quarterly or when major pivot considered
