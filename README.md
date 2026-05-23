# Vāchana: Indic PDF → Audiobook Platform

**Transform literary texts into accessible audio experiences in Hindi, Gujarati, Sanskrit, and English.**

## Project Overview

Vāchana is a browser-based tool that converts PDF documents into high-quality audiobooks with real-time text synchronization. Users can:
- Upload PDFs (digital or OCR-scanned)
- Select language and audio preferences
- Play audio with synchronized captions
- Download individual chunks or merged MP3 files

## Quick Start

See [SETUP.md](SETUP.md) for development environment setup.

## Documentation Structure (14 Files)

### Strategy & Planning
- **[PRODUCT_VISION.md](PRODUCT_VISION.md)** - Mission, lifecycle, personas, go-to-market
- **[ROADMAP.md](ROADMAP.md)** - 4 phases with timeline, metrics, resource planning

### Technical Design
- **[ARCHITECTURE.md](ARCHITECTURE.md)** ⭐ **Phase 2 Plan** - 4 PDF display options analyzed, Option 1 (Overlay) chosen
- **[REQUIREMENTS.md](REQUIREMENTS.md)** - Functional & non-functional specs
- **[DESIGN_STANDARDS.md](DESIGN_STANDARDS.md)** - Code style, accessibility, performance targets

### Development & Quality
- **[DEVELOPER_HANDOFF.md](DEVELOPER_HANDOFF.md)** - Code walkthrough, debugging guide, testing checklist
- **[TESTING_STRATEGY.md](TESTING_STRATEGY.md)** - Test pyramid, coverage targets, QA approach
- **[SETUP.md](SETUP.md)** - Development environment, git workflow, debugging

### Launch & Operations
- **[PRODUCTION_CHECKLIST.md](PRODUCTION_CHECKLIST.md)** - Pre-launch, go-live, post-launch
- **[ASSUMPTIONS.md](ASSUMPTIONS.md)** - Risks, assumptions, validation plans
- **[FUTURE_CONSIDERATIONS.md](FUTURE_CONSIDERATIONS.md)** - Scaling, monetization, expansion

### Decision History & Context
- **[CONVERSATION_HISTORY.md](CONVERSATION_HISTORY.md)** - All decisions made, rationale, learnings
- **[IMPLEMENTATION_SUMMARY.md](IMPLEMENTATION_SUMMARY.md)** - What's built, metrics, next steps

## Current Status

- ✅ MVP: Browser-based PDF→MP3 converter (95% complete)
- ✅ Languages: Hindi, Gujarati, Sanskrit, English
- ✅ TTS: Google Translate API + Web Speech API
- ✅ Downloads: Individual chunks + merged MP3
- ✅ Captions: Text display (condensed view)
- ✅ **Decision Made:** Phase 2 → PDF.js Overlay (Option 1)
- 🔄 Phase 2: Real PDF with text highlights (July-October 2026, 8 weeks)
- 🎯 Target: Sync ±200ms, 95% coordinate accuracy, Oct 31 launch

## Next Phase: PDF Display with Text Synchronization

**Decision:** We're proceeding with **Option 1 - PDF.js Overlay** for Phase 2

**Why This Option?**
- No new dependencies (pdf.js already used)
- Preserves original PDF formatting
- Works for any PDF type (digital or scanned)
- Graceful fallback for edge cases
- Best UX/effort tradeoff

**Timeline:** 8 weeks (July-October 2026)
- Weeks 1-2: Build text-position extraction
- Weeks 3-4: Implement SVG highlight rendering
- Weeks 5-6: UI polish & mobile optimization
- Weeks 7-8: Performance tuning & launch prep

See [ARCHITECTURE.md](ARCHITECTURE.md) for complete technical plan.

---

## How to Get Started

**As a PM/Product Owner:**
1. Read [PRODUCT_VISION.md](PRODUCT_VISION.md) (5 min)
2. Review [CONVERSATION_HISTORY.md](CONVERSATION_HISTORY.md) (10 min) - See all decisions made
3. Check [ROADMAP.md](ROADMAP.md) for 18-month timeline

**As a Developer:**
1. Read [DEVELOPER_HANDOFF.md](DEVELOPER_HANDOFF.md) (15 min) - Code walkthrough
2. Review [ARCHITECTURE.md](ARCHITECTURE.md) (20 min) - Phase 2 implementation plan
3. Run local setup: `python3 -m http.server 8000` then open http://localhost:8000/indic-audiobook-test.html

**As a Stakeholder:**
1. Read [PRODUCT_VISION.md](PRODUCT_VISION.md) (mission & vision)
2. Check [ROADMAP.md](ROADMAP.md) (timeline & targets)
3. Review [IMPLEMENTATION_SUMMARY.md](IMPLEMENTATION_SUMMARY.md) (what's been built)

## Key Technologies

| Layer | Technology |
|-------|-------------|
| Frontend | React 18 + Babel (browser-based) |
| PDF Processing | pdf.js |
| TTS | Google Translate API, Web Speech API |
| Audio Encoding | lamejs (MP3 encoding) |
| Packaging | JSZip |
| UI Framework | Custom CSS (Crimson Pro typography) |

---

**Last Updated:** May 23, 2026
