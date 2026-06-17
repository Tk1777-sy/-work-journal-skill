# Work Journal · 2026-06-17

**Project:** Datarun — KOL Influencer Marketing SaaS Platform
**Stack:** Next.js 14 · TypeScript · Tailwind CSS · Framer Motion · GSAP · Spline · Lenis
**Session length:** Long (~3 hours)

---

## 1. Overview

Focused entirely on the landing page interactive experience. At session start, the hero had a non-functional SVG robot, invisible background lines, and no KOL showcase section. By session end: the background paths are clearly visible, the hero shows clean single-column layout, a 12-card GSAP-animated KOL portrait wall was added between Features and Workflow, and the AI section now features a fully interactive 3D Spline robot whose head tracks the mouse cursor.

---

## 2. Requirements & Requests

### Request 1: Fix robot eye tracking

**User's exact words:**
> "机器人那个视觉化效果 interactive 3D 没有做好呀 我鼠标移动的时候机器人眼睛不会跟着我鼠标转动：重新给你发一遍 你弄一下"

**Desired outcome:** Moving the mouse causes the robot's head/eyes to follow the cursor in real time with a 3D feel.

**Reference material sent:** Spline component code + scene URL `https://prod.spline.design/kZDDjO5HuC9GJUM2/scene.splinecode`

---

### Request 2: Background flowing lines

**User's exact words:**
> "我想要的主页的背景线条流动化效果呢？"

**Desired outcome:** Diagonal blue animated SVG lines flowing across the hero background, creating a tech-forward motion effect.

**Reference material sent:** BackgroundPaths component code (36 SVG paths, Framer Motion)

---

### Request 3: KOL portrait wall on scroll

**User's exact words:**
> "当我往下滑动这个网站时候 想添加一个这个效果"

**Desired outcome:** Scrolling down reveals a grid of KOL influencer portrait cards that scale up as they enter the viewport — a "growing in" reveal effect.

**Reference material sent:** ScrollPortraitWall concept with GSAP + ScrollTrigger

---

### Request 4: Only one robot

**User's exact words:**
> "机器人有一个就可以了呀 为什么上来就是机器人 下面还有机器人 保留下面的机器人"

**Desired outcome:** Remove robot from hero (first screen). Keep only the robot in the AI section below.

---

## 3. Execution — Methods & Tools

### Task 1: Fix BackgroundPaths visibility

**Files modified:** `src/components/ui/background-paths.tsx`

**Approach:** The SVG paths existed but were nearly invisible due to extremely low strokeOpacity values. No library changes needed — just adjusted the animation parameters.

**Key code change:**
```tsx
// Before
strokeWidth={path.width}           // 0.5 + i * 0.03
strokeOpacity={0.05 + path.id * 0.016}
opacity: [0.2, 0.5, 0.2]

// After
strokeWidth={path.width + 0.4}     // made thicker
strokeOpacity={0.18 + path.id * 0.02}
opacity: [0.5, 1, 0.5]
```

**Decision log:** Kept sky-blue (`rgba(56,189,248,1)`) rather than white — white paths would fight with the DATARUN text. Blue reads as "tech infrastructure" and complements the brand color.

---

### Task 2: Install @gsap/react and create ScrollPortraitWall

**Commands run:**
```bash
npm install @gsap/react --save
```

**Files created:** `src/components/ui/scroll-portrait-wall.tsx`

**Approach:** GSAP `ScrollTrigger` with per-card triggers rather than a single pinned section. This gives each card its own entrance timing based on scroll position, which feels more organic than a synchronized reveal.

```tsx
gsap.fromTo(card, 
  { scale: 0.6, opacity: 0, y: 40 },
  {
    scale: 1, opacity: 1, y: 0,
    scrollTrigger: {
      trigger: card,
      start: "top 88%",
      end: "top 45%",
      scrub: 0.6,
    }
  }
)
```

**Decision log:** `scrub: 0.6` (not `true`) gives a slight lag that feels physical without being sluggish. `scrub: true` felt too directly mechanical.

---

### Task 3: Replace SVG robot with Spline 3D robot

**Files modified:** `src/components/landing/LandingPage.tsx`

**Approach:** `@splinetool/react-spline` was already installed. The Spline scene has built-in cursor interaction — no custom mouse tracking code needed. Just render the component inside a sized container.

**Files deleted (deprecated):** `src/components/ui/eye-tracking-robot.tsx` (SVG robot — kept on disk but import removed)

**Decision log:** The SVG robot required manual DOM manipulation (`style.transform = translate(${ox}px)`) to move pupils. This approach breaks in SSR and requires precise coordinate math. Spline handles this natively in the 3D scene.

---

### Task 4: Fix Spline mouse interaction

**Files modified:** `src/components/landing/LandingPage.tsx` (AI section)

**Root cause found:** Two `absolute`-positioned divs sat on top of the Spline canvas. Without `pointer-events-none`, they intercepted all mouse events before they reached the canvas.

```tsx
// Fix 1 — gradient overlay
<div className="absolute inset-0 bg-[radial-gradient(...)] pointer-events-none" />

// Fix 2 — activity badge overlay  
<div className="absolute bottom-4 ... backdrop-blur-sm pointer-events-none">
```

---

## 4. Bugs & Solutions

| Bug | Root cause | Fix | Prevention |
|-----|-----------|-----|------------|
| Preview viewport 3px wide | Screenshot tool reset after server restart | `preview_resize(1280×800)` | Always resize after restart |
| CSS 404 during hot reload | Next.js chunk version mismatch during HMR | Restart preview server | Expected behavior during rapid edits |
| npm ENOENT can't find package.json | User ran `npm run dev` from `~` home dir | `cd ~/Desktop/Datarun\ web && npm run dev` | Always cd to project first |
| Spline robot eyes not tracking | Overlay divs missing `pointer-events-none` | Added to both overlay divs | Always add to any absolute div over interactive canvas |

---

## 5. File Change Log

| Action | Path |
|--------|------|
| Created | `src/components/ui/scroll-portrait-wall.tsx` |
| Modified | `src/components/ui/background-paths.tsx` |
| Modified | `src/components/landing/LandingPage.tsx` |
| Deprecated | `src/components/ui/eye-tracking-robot.tsx` |

---

## 6. Final Output

The landing page now flows as: Hero (text + flowing lines) → Features → KOL Portrait Wall → AI Robot Section → Workflow → Testimonials → CTA → Footer.

The hero is clean and fast — no heavy 3D load on the first screen. The KOL portrait wall gives an immediate sense of the platform's scale (12 cards with platform badges, follower counts, and hover reveal). The AI section robot is visually impressive and genuinely interactive — moving the mouse causes the robot's head to rotate in 3D space.

---

## 7. Follow-up & Known Issues

### Must do next session
- [ ] Delete `eye-tracking-robot.tsx` — it's unused and clutters the component directory

### Should do soon
- [ ] Set upper limit on "品牌客户" counter — currently counts to infinity

### Known limitations
- [ ] Spline scene loads from external URL — in production, evaluate self-hosting the `.splinecode` file
- [ ] ScrollPortraitWall uses Unsplash placeholder images — needs real KOL data pipeline

---

## Suggested git commit message

```
feat(landing): add KOL portrait wall, fix Spline interaction, enhance bg paths

- Add ScrollPortraitWall with 12 KOL cards (GSAP ScrollTrigger, scale 0.6→1 scrub)
- Fix Spline robot mouse tracking (pointer-events-none on overlay divs)
- Increase BackgroundPaths visibility (strokeOpacity 0.05→0.18, opacity 0.2→1.0)
- Remove EyeTrackingRobot from hero, keep Spline robot in AI section only
- Install @gsap/react
```

---

*Generated: 2026-06-17 · work-journal Claude Code skill*
