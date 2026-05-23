# Product Roadmap & Timeline

## Release Schedule

### Phase 1: MVP (May - July 2026) ✅ IN PROGRESS
**Status:** ~90% complete
**Key Features:**
- PDF upload & text extraction
- 4 language support (Hindi, Gujarati, Sanskrit, English)
- TTS via Google Translate API + Web Speech API
- Play/stop controls with speed adjustment
- Download options: individual MP3, ZIP, merged MP3
- Condensed text captions

**Launch Date:** June 30, 2026
**Target Users:** 50-100 early adopters
**Deliverables:**
- ✅ indic-audiobook-test.html (working prototype)
- ⬜ User guide + getting started
- ⬜ Known limitations document
- ⬜ Feedback collection form

**Metrics to Track:**
- Avg. time to convert
- Success rate by PDF type
- Audio quality ratings (1-5 scale)
- Most-used language
- Device types (desktop vs mobile)

---

### Phase 2: MVP+ (August - October 2026) → NEXT PRIORITY
**Status:** Planning
**Goal:** Enhanced text synchronization with real PDF display

**Key Features:**
- **PDF Viewer Integration:** Display original PDF with highlighting
- **Text-Position Mapping:** Extract coordinates from PDF
- **Real-time Highlighting:** Current segment highlighted as audio plays
- **Auto-scroll:** PDF scrolls to follow audio
- **Dual-view Toggle:** Switch between PDF and condensed view
- **Mobile Optimization:** Responsive design for tablets
- **Performance:** Sub-50ms highlighting latency

**Architecture:**
See [ARCHITECTURE.md](ARCHITECTURE.md) for technical options

**Timeline:**
- Week 1-2: Prototype coordinate extraction
- Week 3-4: Build highlight rendering layer
- Week 5-6: Polish UI & performance testing
- Week 7-8: Mobile responsiveness
- Week 9-10: Bug fixes & documentation

**User Impact:**
- Preserves original PDF formatting
- Better for cross-referencing
- Differentiator vs. competitors
- Higher user satisfaction

**Metrics:**
- Sync accuracy: ±200ms tolerance
- 95% of PDFs coordinate-mapped successfully
- User preference: PDF view vs condensed (survey)
- Performance: Sub-1s PDF render time
- Mobile usage: 30%+ of traffic

**Budget:** 400 engineering hours
**Launch Date:** October 31, 2026

---

### Phase 3: Scalable Platform (Nov 2026 - Q2 2027)
**Status:** Planning
**Goal:** Move from browser-only to managed platform with persistence

**Key Features:**
- **User Accounts:** Email signup, login, profile
- **Project Management:** Save/organize conversions
- **Cloud Storage:** Upload & store projects
- **Multiple TTS Engines:** Google Cloud, Azure, Edge-TTS
- **Voice Selection:** Choose from 5+ voices per language
- **Batch Processing:** Convert 10+ PDFs at once
- **Analytics Dashboard:** Track usage, popular texts
- **Collaboration:** Share projects, comments, ratings

**Architecture Changes:**
- Backend: Node.js/Express (hosted on AWS)
- Database: PostgreSQL (user data, projects)
- Storage: S3 (PDFs, audio files)
- Cache: Redis (frequently accessed projects)

**Timeline:**
- Month 1: Backend setup, auth, database
- Month 2: Project management API
- Month 3: Cloud storage integration
- Month 4: TTS engine integration
- Month 5: Analytics & monitoring
- Month 6: Performance optimization

**User Impact:**
- No data loss after browser close
- Access projects from any device
- Faster audio generation (parallel processing)
- Premium voice options

**Metrics:**
- User retention: 60%+ (month-over-month)
- New projects per user: 5+
- TTS engine usage distribution
- Cloud storage utilization

**Budget:** 1200 engineering hours
**Launch Date:** June 30, 2027

---

### Phase 4: Monetization & Growth (Q3 2027+)
**Status:** Planning
**Goal:** Self-sustaining platform with revenue model

**Key Features:**
- **Freemium Model:**
  - Free: 2 conversions/month, 50 MB storage
  - Pro: $9.99/month (unlimited, priority TTS)
  - Enterprise: Custom pricing (bulk licensing)
- **API Access:** Developers can embed TTS
- **White-label:** Libraries/nonprofits can rebrand
- **Premium Voices:** Additional character voices
- **Batch API:** Automation for publishers

**Go-to-Market:**
- Month 1: Freemium pricing page
- Month 2: B2B sales outreach (educational institutions)
- Month 3: API documentation & SDK
- Month 4: Developer onboarding program
- Month 5: Brand partnerships (libraries, nonprofits)

**Revenue Projections:**
- Year 1: $50k ARR (500 users × $10/month avg)
- Year 2: $500k ARR (3000 users + B2B licensing)
- Year 3: $2m ARR (10k users + strong B2B)

**Metrics:**
- Paid conversion rate: 10-15%
- Churn rate: <5% monthly
- LTV:CAC ratio: >3:1
- Enterprise contracts: 5+

**Budget:** Ongoing (sales, marketing, support)
**Launch Date:** September 2027

---

## Dependency Map

```
Phase 1 (MVP) ✅
    ↓
Phase 2 (PDF Sync) → Requires Phase 1 stable
    ↓
Phase 3 (Platform) → Requires Phase 2 polish
    ↓
Phase 4 (Monetization) → Requires Phase 3 robust
```

### Critical Path
1. **Fix playback issues** (current)
2. **Stabilize TTS** (complete Phase 1)
3. **Implement PDF sync** (Phase 2, critical for differentiation)
4. **Launch v1.0** (public, June 2026)
5. **Collect user feedback** (June-July 2026)
6. **Release Phase 2** (October 2026)

---

## Quarterly View

| Quarter | Phase | Focus | Users | ARR |
|---------|-------|-------|-------|-----|
| Q2 2026 | MVP Launch | Stability, feedback | 100 | $0 |
| Q3 2026 | MVP Polish | Bugs, performance | 500 | $0 |
| Q4 2026 | MVP+ PDF Sync | Enhanced UX | 1,000 | $0 |
| Q1 2027 | Platform Beta | Accounts, persistence | 2,000 | $0 |
| Q2 2027 | Platform GA | Full features | 3,000 | $5k |
| Q3 2027 | Monetization | Freemium model | 3,500 | $35k |
| Q4 2027 | Growth | B2B sales | 5,000 | $80k |
| 2028 | Scale | Enterprise features | 10,000 | $500k+ |

---

## Feature Priority Matrix

### High Priority (Do First)
- ✅ PDF upload & conversion
- ✅ 4 language support
- ✅ Audio download
- 🔄 PDF viewer with highlighting (Phase 2)

### Medium Priority (Next)
- Mobile responsiveness
- User accounts
- Project management
- Voice selection

### Low Priority (Nice-to-Have)
- Translation features
- Video generation
- Community features
- Social sharing

---

## Risk Mitigation Timeline

| Risk | Phase 1 | Phase 2 | Phase 3 | Phase 4 |
|------|---------|---------|---------|---------|
| Google TTS rate limits | Monitor | Fallback engine ready | Multi-engine | Own TTS model |
| PDF coordinate extraction | Fallback to condensed | Improve accuracy | Handle edge cases | Perfect sync |
| Scale issues | Local processing | Pre-optimize | Load testing | Distributed system |
| User acquisition | Organic + communities | Partner outreach | Sales team | Enterprise sales |
| Churn | Track metrics | Improve UX | Premium features | Better support |

---

## Success Criteria by Phase

### Phase 1 Success
- ✅ 100+ users
- ✅ 85%+ conversion success rate
- ✅ NPS ≥30
- ✅ <5% critical bugs

### Phase 2 Success
- ✅ 500+ users
- ✅ 90%+ coordinate mapping success
- ✅ Sync ±200ms tolerance
- ✅ Users prefer PDF view (survey)
- ✅ NPS ≥40

### Phase 3 Success
- ✅ 3,000+ users
- ✅ 60% retention (month-over-month)
- ✅ 10% free→paid conversion
- ✅ 5+ enterprise pilot customers
- ✅ NPS ≥45

### Phase 4 Success
- ✅ $500k+ ARR
- ✅ <5% monthly churn
- ✅ 3+ established partnerships
- ✅ Top 3 search result for "PDF audiobook"
- ✅ NPS ≥50

---

## Resource Planning

### Phase 1 (Complete)
- 1 Full-stack dev (completed)
- ~200 hours

### Phase 2 (Next)
- 1 Full-stack dev (100%)
- 1 QA engineer (50%)
- 1 Product manager (50%)
- ~400 engineering hours
- **Timeline:** 10 weeks

### Phase 3 (Future)
- 2 Backend devs
- 1 Frontend dev
- 1 DevOps engineer
- 1 QA engineer
- 1 Product manager
- ~1200 engineering hours
- **Timeline:** 26 weeks

### Phase 4 (Future)
- Add sales/marketing
- Customer success
- Community management
- Ongoing support

---

## Testing & Launch Checklist

### Phase 1 Launch
- [ ] Convert 20+ test PDFs successfully
- [ ] Audio plays in Chrome, Firefox, Safari
- [ ] Mobile responsive (tested on iPhone)
- [ ] Download files work
- [ ] Error handling for edge cases
- [ ] User guide written
- [ ] Known issues documented

### Phase 2 Launch
- [ ] PDF highlights sync to ±200ms
- [ ] 95% of test PDFs coordinate-mapped
- [ ] Mobile swipe gestures work
- [ ] Performance: <50ms highlight latency
- [ ] Fallback to condensed view works
- [ ] Accessibility audit (WCAG AA)
- [ ] Video tutorial created

### Phase 3 Launch
- [ ] Load testing: 1000+ concurrent users
- [ ] Database scalability verified
- [ ] Auth system secure
- [ ] Payment processing tested
- [ ] GDPR compliance audit
- [ ] Security penetration test
- [ ] SLA documentation

---

**Next Step:** Complete Phase 2 PDF sync implementation in next sprint

**Approval:** 
- [ ] Product Manager
- [ ] Engineering Lead
- [ ] Stakeholder Sign-off
