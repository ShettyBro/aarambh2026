# UI/UX & Background Photo Animation Blueprint
> **Source Project:** `dandiya-reg` (`F:\GitHub Live\dandiya-reg`)  
> **Target Project:** `aarambh2026` (`F:\GitHub Live\aarambh2026`)  
> **Author:** ShettyBro / DeepMind Antigravity  
> **Specification Version:** 1.0.0 (Production Verified)

---

## Table of Contents
1. [Visual Language & Core UI/UX Philosophy](#1-visual-language--core-uiux-philosophy)
2. [Technology Stack & Dependencies](#2-technology-stack--dependencies)
3. [Design System Tokens & Tailwind Configuration](#3-design-system-tokens--tailwind-configuration)
4. [The Signature "Bamboo Style" Background Scrolling Gallery](#4-the-signature-bamboo-style-background-scrolling-gallery)
5. [Image Asset Optimization Pipeline (Sharp & WebP)](#5-image-asset-optimization-pipeline-sharp--webp)
6. [Interactive Memory Gallery (Sine-Wave LED Fairy Lights)](#6-interactive-memory-gallery-sine-wave-led-fairy-lights)
7. [3D Flip-Clock Countdown Timer](#7-3d-flip-clock-countdown-timer)
8. [Glassmorphism Design Primitives (GlassPanel, Button, Container)](#8-glassmorphism-design-primitives-glasspanel-button-container)
9. [Bento-Grid Event Highlights](#9-bento-grid-event-highlights)
10. [Floating Dynamic Pill Navigation & Mobile Drawer](#10-floating-dynamic-pill-navigation--mobile-drawer)
11. [Step-by-Step Replication Blueprint for Aarambh 2026](#11-step-by-step-replication-blueprint-for-aarambh-2026)
12. [Theme Customization Matrix for Aarambh 2026](#12-theme-customization-matrix-for-aarambh-2026)

---

## 1. Visual Language & Core UI/UX Philosophy

The `dandiya-reg` frontend delivers an immersive, high-energy festival atmosphere designed to convert visitors into registered attendees while setting an electric mood.

The core visual pillars are:

1. **Ambient Infinite Movement (The "Bamboo" Gallery):** An 8-column, 6-degree tilted grid of square photos that endlessly glide up and down in alternating rhythm behind the entire page. A dark translucent wash (`#07080f/60`) ensures all foreground text and cards stay crystal clear while keeping the kinetic festival energy alive in the background.
2. **Deep Midnight & Festive Gold Palette:** High contrast palette built on ultra-dark midnight blues (`#07080f`, `#0f1122`), vibrant festival golds (`#ecc07a`), and festival magenta accents (`#d63aa8`).
3. **Glassmorphism with Tactile Borders:** Cards float with semi-transparent frosted backgrounds, subtle white borders (`border-white/10` to `border-white/15`), and glowing radial drop shadows (`boxShadow: glow` / `goldGlow`).
4. **Physical Festival Lighting:** An interactive sine-wave LED fairy light string rendered in SVG with blurred light bloom, pulsing amber bulbs, hanging triangular pennants, and floating mascot dancers.
5. **Physical 3D Flip Clock:** Countdown digits that physically flip in half using 3D CSS perspective transforms and cubic bezier easing.

---

## 2. Technology Stack & Dependencies

To replicate the exact look, feel, and 60fps animations:

### Core Dependencies
```bash
npm install react react-dom react-router-dom motion clsx tailwind-merge @phosphor-icons/react @fontsource-variable/outfit
```

### Dev Dependencies (Build & Image Optimization)
```bash
npm install -D tailwindcss postcss autoprefixer sharp @types/react @types/react-dom vite
```

### Package Roles
| Package | Role in UI/UX |
|---|---|
| `motion` (`^11.15.0`) | Spring hover micro-interactions, scroll reveals, floating dancer levitation |
| `tailwindcss` (`^3.4.17`) | Styling system and custom keyframe engine for infinite background scrolling |
| `@phosphor-icons/react` (`^2.1.7`) | Consistent icon system (Duotone and Fill weights) |
| `@fontsource-variable/outfit` | Modern geometric sans-serif typeface across all headings and copy |
| `sharp` (`^0.35.4`) | Batch image optimizer resizing raw photos into 640x640 WebPs |
| `clsx` + `tailwind-merge` | Dynamic class string merging utility |

---

## 3. Design System Tokens & Tailwind Configuration

### `tailwind.config.ts`
Add these custom tokens to your Tailwind config. The keyframe animations for the background scroll and 3D flip leaves must be registered here:

```ts
import type { Config } from "tailwindcss";

export default {
  content: ["./index.html", "./src/**/*.{ts,tsx}"],
  theme: {
    extend: {
      colors: {
        midnight: {
          950: "#07080f",
          900: "#0f1122",
          800: "#161a33",
          700: "#1f2444"
        },
        indigo: {
          400: "#8b8cf7",
          500: "#6d6ef4",
          600: "#5648e0"
        },
        festival: {
          magenta: "#d63aa8",
          gold: "#ecc07a",
          goldDim: "#b99457"
        }
      },
      fontFamily: {
        display: ["'Outfit Variable'", "system-ui", "sans-serif"],
        body: ["'Outfit Variable'", "system-ui", "sans-serif"]
      },
      borderRadius: {
        card: "18px",
        pill: "999px"
      },
      boxShadow: {
        glow: "0 0 60px -12px rgba(109, 110, 244, 0.45)",
        goldGlow: "0 0 40px -10px rgba(236, 192, 122, 0.4)"
      },
      maxWidth: {
        page: "1400px"
      },
      keyframes: {
        "scroll-up": {
          "0%": { transform: "translateY(0)" },
          "100%": { transform: "translateY(-50%)" }
        },
        "scroll-down": {
          "0%": { transform: "translateY(-50%)" },
          "100%": { transform: "translateY(0)" }
        },
        "flip-leaf-top": {
          "0%": { transform: "rotateX(0deg)" },
          "100%": { transform: "rotateX(-90deg)" }
        },
        "flip-leaf-bottom": {
          "0%": { transform: "rotateX(90deg)" },
          "100%": { transform: "rotateX(0deg)" }
        }
      },
      animation: {
        "scroll-up-slow": "scroll-up 8s ease-in-out infinite alternate",
        "scroll-up-slower": "scroll-up 10s ease-in-out infinite alternate",
        "scroll-down-slow": "scroll-down 9s ease-in-out infinite alternate",
        "scroll-down-slower": "scroll-down 11s ease-in-out infinite alternate",
        "flip-leaf-top": "flip-leaf-top 0.3s cubic-bezier(0.4,0,1,1) forwards",
        "flip-leaf-bottom": "flip-leaf-bottom 0.3s cubic-bezier(0,0,0.2,1) 0.3s forwards"
      }
    }
  },
  plugins: []
} satisfies Config;
```

### Global Styles (`src/index.css`)
```css
@import "@fontsource-variable/outfit";

@tailwind base;
@tailwind components;
@tailwind utilities;

html,
body,
#root {
  height: 100%;
}

body {
  background-color: #07080f;
  color: #f4f3fb;
  font-family: "Outfit Variable", system-ui, sans-serif;
}

::selection {
  background-color: rgba(109, 110, 244, 0.35);
  color: #ffffff;
}
```

### Class Merger Helper (`src/lib/cn.ts`)
```ts
import { clsx, type ClassValue } from "clsx";
import { twMerge } from "tailwind-merge";

export function cn(...inputs: ClassValue[]): string {
  return twMerge(clsx(inputs));
}
```

---

## 4. The Signature "Bamboo Style" Background Scrolling Gallery

The **Bamboo Gallery** (`BambooGallery.tsx`) forms the kinetic canvas behind the entire website.

### Visual Architecture & Geometry
- **Fixed Fullscreen Underlay:** `fixed inset-0 -z-10 h-dvh w-full overflow-hidden pointer-events-none`.
- **Canvas Expansion & Angle:** To rotate without exposing black corners, the container is scaled to `132%` width and height, translated `-16%` on X and Y, and tilted `-6deg`:
  ```html
  <div className="flex h-[132%] w-[132%] -translate-x-[16%] -translate-y-[16%] -rotate-6 gap-2 sm:gap-3">
  ```
- **8 Parallel Columns:** Width is `w-[12.5%]` per column.
- **Alternating Asynchronous Durations:** The columns use 4 alternating animation speeds (`8s`, `11s`, `10s`, `9s`). Because each alternates direction (`infinite alternate`) and has a different period, the overall pattern never appears to repeat.
- **10x Virtual Array Duplication:** Images in each column are cloned (`Array.from({ length: 10 }, () => column).flat()`) so that translating `-50%` vertically maintains continuous visible imagery.
- **Contrast Shield:** An absolute overlay `bg-midnight-950/60` sits on top to dim the photos, keeping white foreground typography readable.

### Component Code (`src/components/gallery/BambooGallery.tsx`)
```tsx
import { BACKGROUND_GALLERY_FILENAMES, galleryImageUrl, splitIntoColumns } from "../../lib/gallery-manifest";
import { cn } from "../../lib/cn";

const COLUMN_ANIMATIONS = [
  "animate-scroll-up-slow",
  "animate-scroll-down-slower",
  "animate-scroll-up-slower",
  "animate-scroll-down-slow",
  "animate-scroll-up-slow",
  "animate-scroll-down-slower",
  "animate-scroll-up-slower",
  "animate-scroll-down-slow"
];

export function BambooGallery({ className }: { className?: string }) {
  const columns = splitIntoColumns(BACKGROUND_GALLERY_FILENAMES, 8);

  return (
    <div
      className={cn("fixed inset-0 -z-10 h-dvh w-full overflow-hidden pointer-events-none", className)}
      aria-hidden="true"
    >
      <div className="flex h-[132%] w-[132%] -translate-x-[16%] -translate-y-[16%] -rotate-6 gap-2 sm:gap-3">
        {columns.map((column, columnIndex) => {
          if (column.length === 0) return null;
          const loopImages = Array.from({ length: 10 }, () => column).flat();
          return (
            <div
              key={columnIndex}
              className={cn(
                "flex w-[12.5%] shrink-0 flex-col gap-2 sm:gap-3",
                COLUMN_ANIMATIONS[columnIndex % COLUMN_ANIMATIONS.length]
              )}
            >
              {loopImages.map((filename, imageIndex) => (
                <img
                  key={`${filename}-${imageIndex}`}
                  src={galleryImageUrl(filename)}
                  alt=""
                  loading="eager"
                  decoding="async"
                  className="aspect-square w-full rounded-2xl object-cover"
                />
              ))}
            </div>
          );
        })}
      </div>
      <div className="absolute inset-0 bg-midnight-950/60" />
    </div>
  );
}
```

### Manifest Helper (`src/lib/gallery-manifest.ts`)
```ts
export const GALLERY_IMAGE_FILENAMES = [
  "IMG_0813.webp",
  "IMG_0814.webp",
  "IMG_0823.webp",
  "IMG_0858.webp",
  "IMG_0863.webp",
  "IMG_0896.webp",
  "IMG_0899.webp",
  "IMG_1071.webp",
  "IMG_1078.webp",
  "IMG_1095.webp",
  "IMG_1101.webp",
  "IMG20241010201923.webp",
  "IMG20241010205844.webp",
  "IMG20241010205858.webp",
  "IMG20241010220611.webp",
  "IMG20241010220614.webp",
  "IMG20241010220738.webp",
  "PXL_20241010_133407886.NIGHT.webp",
  "PXL_20241010_134325281.NIGHT.webp",
  "PXL_20241010_141638031.NIGHT.webp",
  "PXL_20241010_142112537.NIGHT.webp",
  "PXL_20241010_142212565.NIGHT.webp",
  "PXL_20241010_142757667.NIGHT.webp",
  "PXL_20241010_142823915.NIGHT.webp",
  "PXL_20241010_163319387.NIGHT.webp"
];

const BANNER_HEAVY_FILENAMES = new Set([
  "IMG_0813.webp",
  "IMG_0814.webp",
  "PXL_20241010_142212565.NIGHT.webp"
]);

export const BACKGROUND_GALLERY_FILENAMES = GALLERY_IMAGE_FILENAMES.filter(
  (filename) => !BANNER_HEAVY_FILENAMES.has(filename)
);

export function galleryImageUrl(filename: string): string {
  return `/images/gallery/${filename}`;
}

export function splitIntoColumns<T>(items: T[], columnCount: number): T[][] {
  const columns: T[][] = Array.from({ length: columnCount }, () => []);
  items.forEach((item, index) => {
    columns[index % columnCount]?.push(item);
  });
  return columns;
}
```

---

## 5. Image Asset Optimization Pipeline (Sharp & WebP)

Because the Bamboo Gallery mounts over 80 image elements simultaneously, uncompressed DSLR or mobile photos (2MB-8MB each) will freeze low-end mobile devices and exhaust browser memory.

The pipeline converts all source photos in `public/images/` to 640x640 WebP squares at 68% quality (~35KB each).

### `scripts/optimize-images.mjs`
```javascript
import { readdir, mkdir } from "node:fs/promises";
import { join, extname, basename } from "node:path";
import sharp from "sharp";

const SOURCE_DIR = join(process.cwd(), "public", "images");
const OUTPUT_DIR = join(SOURCE_DIR, "gallery");

async function main() {
  await mkdir(OUTPUT_DIR, { recursive: true });
  const entries = await readdir(SOURCE_DIR, { withFileTypes: true });
  const files = entries.filter(
    (entry) => entry.isFile() && [".webp", ".jpg", ".jpeg", ".png"].includes(extname(entry.name).toLowerCase())
  );

  console.log(`Found ${files.length} images to optimize...`);

  for (const file of files) {
    const inputPath = join(SOURCE_DIR, file.name);
    const outputName = `${basename(file.name, extname(file.name))}.webp`;
    const outputPath = join(OUTPUT_DIR, outputName);

    await sharp(inputPath)
      .resize({ width: 640, height: 640, fit: "cover" })
      .webp({ quality: 68 })
      .toFile(outputPath);

    console.log(`✓ Optimized ${file.name} -> gallery/${outputName}`);
  }

  console.log(`Done! All optimized images saved to ${OUTPUT_DIR}`);
}

main().catch((error) => {
  console.error(error);
  process.exitCode = 1;
});
```

Run via: `node scripts/optimize-images.mjs` (add to `package.json` scripts).

---

## 6. Interactive Memory Gallery (Sine-Wave LED Fairy Lights)

The Memory Gallery (`MemoryGallery.tsx`) displays Polaroid photo cards suspended from an undulating fairy-light wire.

### Architectural Innovations
1. **SVG Sine Wave Wire Calculation:**
   Using the formula $y(t) = \frac{H}{2} - A \cdot \sin(2\pi \cdot f \cdot t)$:
   - `STRIP_WIDTH = 1000`
   - `STRIP_HEIGHT = 100`
   - `STRIP_AMPLITUDE = 32`
   - `STRIP_CYCLES = 2.5`
   An under-layer SVG path uses `filter: blur(6px)` to produce warm amber neon diffusion, while the top path uses `vectorEffect="non-scaling-stroke"` for crisp vector edges.
2. **Dynamic Hanging Drop Alignment:**
   Each of the 6 card slots calculates its top hanging drop margin from the wire's current sine height so that the connecting wire strings touch the SVG wire perfectly:
   ```ts
   function posterMarginTop(slotIndex: number): number {
     const rowT = (slotIndex + 0.5) / SLOT_COUNT;
     const wireT = (rowT - 0.09) / (1 - 2 * 0.09);
     const waveFraction = waveY(wireT) / STRIP_HEIGHT;
     return MIN_DROP_PX + (WAVE_ZONE_PX - waveFraction * WAVE_ZONE_PX);
   }
   ```
3. **Pulsing Golden LED Bulbs & Pennants:**
   16 bulbs pulse in scale and glow opacity with staggered phase delays. 9 triangular pennants hang between bulbs using CSS `clipPath: polygon(0 0, 100% 0, 50% 100%)`.
4. **Floating Mascot Dancers:**
   Two dancer graphics anchor both ends of the light wire, smoothly floating with an infinite `y: [0, -14, 0, 8, 0]` spring-like bounce.
5. **Periodic Photo Shuffle with Blackout Fade:**
   Every 4000ms, slots fade to pitch black in a staggered wave (`slotIndex * 300ms`), swap the active photo, and fade back in.
6. **Breathing Gold Box Shadow:**
   Cards continuously pulse their gold halo shadow between 18px and 34px spread.

### Component Implementation (`src/routes/landing/MemoryGallery.tsx`)
```tsx
import { useEffect, useRef, useState } from "react";
import { motion } from "motion/react";
import { Container } from "../../components/ui/Container";
import { GALLERY_IMAGE_FILENAMES, galleryImageUrl } from "../../lib/gallery-manifest";

const SLOT_COUNT = 6;
const BASE_INTERVAL_MS = 4000;
const FADE_MS = 380;
const BLACKOUT_HOLD_MS = 100;

const STRIP_WIDTH = 1000;
const STRIP_HEIGHT = 100;
const STRIP_AMPLITUDE = 32;
const STRIP_CYCLES = 2.5;
const BULB_COUNT = 16;
const PENNANT_COUNT = 9;
const WAVE_ZONE_PX = 80;
const MIN_DROP_PX = 26;
const WIRE_INSET = 0.09;

function rowTToWireT(rowT: number): number {
  return (rowT - WIRE_INSET) / (1 - 2 * WIRE_INSET);
}

function waveY(t: number): number {
  return STRIP_HEIGHT / 2 - STRIP_AMPLITUDE * Math.sin(t * Math.PI * 2 * STRIP_CYCLES);
}

function buildWavePathD(sampleCount: number): string {
  const points = Array.from({ length: sampleCount + 1 }, (_, i) => {
    const t = i / sampleCount;
    const x = t * STRIP_WIDTH;
    const y = waveY(t);
    return `${x.toFixed(1)},${y.toFixed(1)}`;
  });
  return `M ${points.join(" L ")}`;
}

const WAVE_PATH_D = buildWavePathD(80);

const BULBS = Array.from({ length: BULB_COUNT }, (_, i) => {
  const t = i / (BULB_COUNT - 1);
  return { leftPercent: t * 100, topPercent: (waveY(t) / STRIP_HEIGHT) * 100, delay: i * 0.12 };
});

const PENNANTS = Array.from({ length: PENNANT_COUNT }, (_, i) => {
  const t = (i + 0.5) / PENNANT_COUNT;
  return {
    leftPercent: t * 100,
    topPercent: (waveY(t) / STRIP_HEIGHT) * 100,
    color: i % 2 === 0 ? "#ecc07a" : "#d63aa8"
  };
});

function posterMarginTop(slotIndex: number): number {
  const rowT = (slotIndex + 0.5) / SLOT_COUNT;
  const wireT = rowTToWireT(rowT);
  const waveFraction = waveY(wireT) / STRIP_HEIGHT;
  return MIN_DROP_PX + (WAVE_ZONE_PX - waveFraction * WAVE_ZONE_PX);
}

function FloatingDancers({ flip = false }: { flip?: boolean }) {
  return (
    <div
      className="absolute top-1/2 z-10 h-24 w-24 -translate-y-1/2 sm:h-28 sm:w-28"
      style={flip ? { left: "calc(100% + 0.25rem)" } : { right: "calc(100% + 0.25rem)" }}
    >
      <motion.img
        src="/dandiya-dancers.png"
        alt=""
        aria-hidden="true"
        loading="lazy"
        decoding="async"
        className="h-full w-full object-contain drop-shadow-[0_10px_24px_rgba(0,0,0,0.6)]"
        style={flip ? { transform: "scaleX(-1)" } : undefined}
        animate={{ y: [0, -14, 0, 8, 0] }}
        transition={{ duration: 4.2, repeat: Infinity, ease: "easeInOut" }}
      />
    </div>
  );
}

function LedStrip() {
  return (
    <div className="relative hidden h-20 sm:block">
      <div className="absolute inset-y-0 left-[9%] right-[9%]">
        <svg
          viewBox={`0 0 ${STRIP_WIDTH} ${STRIP_HEIGHT}`}
          preserveAspectRatio="none"
          className="absolute inset-0 h-full w-full overflow-visible"
        >
          <path
            d={WAVE_PATH_D}
            fill="none"
            stroke="rgba(236,192,122,0.55)"
            strokeWidth="6"
            strokeLinecap="round"
            style={{ filter: "blur(6px)" }}
          />
          <path
            d={WAVE_PATH_D}
            fill="none"
            stroke="#ecc07a"
            strokeWidth="2.5"
            strokeLinecap="round"
            vectorEffect="non-scaling-stroke"
          />
        </svg>

        {PENNANTS.map((pennant, i) => (
          <span
            key={i}
            className="absolute h-3 w-3 -translate-x-1/2"
            style={{
              left: `${pennant.leftPercent}%`,
              top: `${pennant.topPercent}%`,
              marginTop: "3px",
              backgroundColor: pennant.color,
              clipPath: "polygon(0 0, 100% 0, 50% 100%)"
            }}
          />
        ))}

        {BULBS.map((bulb, i) => (
          <motion.span
            key={i}
            className="absolute h-3.5 w-3.5 -translate-x-1/2 -translate-y-1/2 rounded-full bg-festival-gold"
            style={{
              left: `${bulb.leftPercent}%`,
              top: `${bulb.topPercent}%`,
              boxShadow: "0 0 16px 4px rgba(236,192,122,0.9)"
            }}
            animate={{ opacity: [0.4, 1, 0.4], scale: [0.85, 1.15, 0.85] }}
            transition={{ duration: 2.2, repeat: Infinity, ease: "easeInOut", delay: bulb.delay }}
          />
        ))}

        <FloatingDancers />
        <FloatingDancers flip />
      </div>
    </div>
  );
}

function HangingPoster({
  slotIndex,
  slotStep,
  generation,
  staggerMs,
  marginTop
}: {
  slotIndex: number;
  slotStep: number;
  generation: number;
  staggerMs: number;
  marginTop: number;
}) {
  const total = GALLERY_IMAGE_FILENAMES.length;
  const [displayIndex, setDisplayIndex] = useState((slotIndex * slotStep) % total);
  const [blackout, setBlackout] = useState(false);
  const mountedRef = useRef(false);

  useEffect(() => {
    if (!mountedRef.current) {
      mountedRef.current = true;
      return;
    }
    const timer = window.setTimeout(() => setBlackout(true), staggerMs);
    return () => window.clearTimeout(timer);
  }, [generation, staggerMs]);

  const filename = GALLERY_IMAGE_FILENAMES[displayIndex] ?? "";

  return (
    <div
      className="flex flex-1 flex-col items-center px-2.5 sm:mt-[var(--card-drop)] sm:px-4"
      style={{ "--card-drop": `${marginTop}px` } as React.CSSProperties}
    >
      <div className="hidden h-5 w-px bg-white/25 sm:block" />
      <div className="hidden h-2 w-2 shrink-0 rounded-full bg-festival-gold/80 sm:block" />
      
      <motion.div
        className="relative aspect-[3/4] w-full overflow-hidden rounded-2xl border-[3px] border-festival-gold/70 bg-midnight-900"
        animate={{
          boxShadow: [
            "0 0 18px -2px rgba(236,192,122,0.45), 0 22px 48px -16px rgba(0,0,0,0.75)",
            "0 0 34px 4px rgba(236,192,122,0.85), 0 22px 48px -16px rgba(0,0,0,0.75)",
            "0 0 18px -2px rgba(236,192,122,0.45), 0 22px 48px -16px rgba(0,0,0,0.75)"
          ]
        }}
        transition={{ duration: 3, repeat: Infinity, ease: "easeInOut" }}
      >
        <img
          src={galleryImageUrl(filename)}
          alt=""
          loading="eager"
          decoding="async"
          className="absolute inset-0 h-full w-full object-cover"
        />
        <motion.div
          className="pointer-events-none absolute inset-0 bg-black"
          animate={{ opacity: blackout ? 1 : 0 }}
          transition={{ duration: FADE_MS / 1000, ease: "easeInOut" }}
          onAnimationComplete={() => {
            if (blackout) {
              window.setTimeout(() => {
                setDisplayIndex((generation + slotIndex * slotStep) % total);
                setBlackout(false);
              }, BLACKOUT_HOLD_MS);
            }
          }}
        />
      </motion.div>
    </div>
  );
}

export function MemoryGallery() {
  const slotStep = Math.max(1, Math.floor(GALLERY_IMAGE_FILENAMES.length / SLOT_COUNT));
  const [generation, setGeneration] = useState(0);

  useEffect(() => {
    const timer = setInterval(() => setGeneration((g) => g + 1), BASE_INTERVAL_MS);
    return () => clearInterval(timer);
  }, []);

  return (
    <section className="py-20 sm:py-28">
      <Container>
        <motion.h2
          initial={{ opacity: 0, y: 20 }}
          whileInView={{ opacity: 1, y: 0 }}
          viewport={{ once: true, amount: 0.4 }}
          transition={{ duration: 0.5 }}
          className="mb-10 font-display text-3xl font-semibold tracking-tight text-white [text-shadow:0_1px_3px_rgba(0,0,0,0.85),0_4px_18px_rgba(0,0,0,0.55)] sm:text-4xl"
        >
          Last year, in frames
        </motion.h2>

        <LedStrip />

        <div className="grid grid-cols-2 gap-4 sm:flex sm:items-start sm:gap-0">
          {Array.from({ length: SLOT_COUNT }, (_, slotIndex) => (
            <HangingPoster
              key={slotIndex}
              slotIndex={slotIndex}
              slotStep={slotStep}
              generation={generation}
              staggerMs={slotIndex * 300}
              marginTop={posterMarginTop(slotIndex)}
            />
          ))}
        </div>
      </Container>
    </section>
  );
}
```

---

## 7. 3D Flip-Clock Countdown Timer

The countdown clock uses genuine 3D perspective transforms to fold numbers like a split-flap display board.

### Mechanics
- `perspective: 300px` creates the depth field.
- `HalfDigit` clips the top 50% and bottom 50% of the number.
- When the second changes, two animations fire simultaneously:
  1. Top Leaf: folds down forward from `0deg` to `-90deg` (`animate-flip-leaf-top`).
  2. Bottom Leaf: unrolls forward from `90deg` to `0deg` (`animate-flip-leaf-bottom`).
- If the user has OS reduced motion enabled (`prefers-reduced-motion: reduce`), it falls back to instant numeric changes.

### Component Implementation (`src/components/Countdown.tsx`)
```tsx
import { useEffect, useRef, useState } from "react";

interface CountdownParts {
  days: number;
  hours: number;
  minutes: number;
  seconds: number;
}

function computeParts(target: Date, now: Date): CountdownParts | null {
  const remainingMs = target.getTime() - now.getTime();
  if (remainingMs <= 0) return null;

  const days = Math.floor(remainingMs / (24 * 60 * 60 * 1000));
  const hours = Math.floor((remainingMs % (24 * 60 * 60 * 1000)) / (60 * 60 * 1000));
  const minutes = Math.floor((remainingMs % (60 * 1000)) / (60 * 1000));
  const seconds = Math.floor((remainingMs % (60 * 1000)) / 1000);

  return { days, hours, minutes, seconds };
}

function pad(value: number): string {
  return String(value).padStart(2, "0");
}

function prefersReducedMotion(): boolean {
  return typeof window !== "undefined" && window.matchMedia("(prefers-reduced-motion: reduce)").matches;
}

function HalfDigit({ text, position }: { text: string; position: "top" | "bottom" }) {
  return (
    <div
      className={`absolute inset-x-0 overflow-hidden ${position === "top" ? "top-0" : "bottom-0"}`}
      style={{ height: "calc(var(--flip-h) / 2)" }}
    >
      <div
        className="absolute inset-x-0 flex items-center justify-center font-display font-bold tabular-nums text-white"
        style={{
          height: "var(--flip-h)",
          [position]: 0,
          fontSize: "calc(var(--flip-h) * 0.44)"
        }}
      >
        {text}
      </div>
    </div>
  );
}

function FlipUnit({ value, label }: { value: number; label: string }) {
  const text = pad(value);
  const [displayed, setDisplayed] = useState(text);
  const [previous, setPrevious] = useState(text);
  const [flipping, setFlipping] = useState(false);
  const timeoutRef = useRef<number | undefined>(undefined);

  useEffect(() => {
    if (text === displayed) return;
    if (prefersReducedMotion()) {
      setDisplayed(text);
      return;
    }
    setPrevious(displayed);
    setFlipping(true);
    window.clearTimeout(timeoutRef.current);
    timeoutRef.current = window.setTimeout(() => {
      setDisplayed(text);
      setFlipping(false);
    }, 600);
    return () => window.clearTimeout(timeoutRef.current);
  }, [text, displayed]);

  return (
    <div className="flex flex-col items-center gap-2">
      <div
        className="relative w-14 rounded-xl bg-midnight-800 shadow-[0_8px_20px_-6px_rgba(0,0,0,0.6)] ring-1 ring-white/10 sm:w-[4.5rem]"
        style={{ height: "var(--flip-h)", perspective: "300px" }}
      >
        <HalfDigit text={text} position="top" />
        <HalfDigit text={flipping ? previous : text} position="bottom" />

        <div className="absolute inset-x-0 top-1/2 z-20 h-px -translate-y-1/2 bg-black/50" />

        {flipping && (
          <>
            <div
              className="absolute inset-x-0 top-0 z-10 origin-bottom animate-flip-leaf-top overflow-hidden rounded-t-xl [transform-style:preserve-3d]"
              style={{ height: "calc(var(--flip-h) / 2)" }}
            >
              <div
                className="absolute inset-x-0 top-0 flex items-center justify-center rounded-t-xl bg-midnight-800 font-display font-bold tabular-nums text-white"
                style={{ height: "var(--flip-h)", fontSize: "calc(var(--flip-h) * 0.44)" }}
              >
                {previous}
              </div>
            </div>
            <div
              className="absolute inset-x-0 bottom-0 z-10 origin-top animate-flip-leaf-bottom overflow-hidden rounded-b-xl [transform-style:preserve-3d]"
              style={{ height: "calc(var(--flip-h) / 2)" }}
            >
              <div
                className="absolute inset-x-0 bottom-0 flex items-center justify-center rounded-b-xl bg-midnight-800 font-display font-bold tabular-nums text-white"
                style={{ height: "var(--flip-h)", fontSize: "calc(var(--flip-h) * 0.44)" }}
              >
                {text}
              </div>
            </div>
          </>
        )}
      </div>
      <span className="text-[10px] uppercase tracking-[0.2em] text-festival-gold">{label}</span>
    </div>
  );
}

export function Countdown({ targetIso }: { targetIso: string }) {
  const target = new Date(targetIso);
  const [parts, setParts] = useState<CountdownParts | null>(() => computeParts(target, new Date()));

  useEffect(() => {
    const interval = setInterval(() => {
      setParts(computeParts(target, new Date()));
    }, 1000);
    return () => clearInterval(interval);
  }, [targetIso]);

  return (
    <div
      className={`overflow-hidden transition-[max-height,opacity] duration-700 motion-reduce:transition-none ${
        parts ? "max-h-40 opacity-100" : "max-h-0 opacity-0"
      }`}
      aria-hidden={!parts}
    >
      {parts && (
        <div className="flex items-start justify-center gap-2.5 [--flip-h:3.5rem] sm:gap-4 sm:[--flip-h:4.5rem]">
          <FlipUnit value={parts.days} label="Days" />
          <FlipUnit value={parts.hours} label="Hours" />
          <FlipUnit value={parts.minutes} label="Minutes" />
          <FlipUnit value={parts.seconds} label="Seconds" />
        </div>
      )}
    </div>
  );
}
```

---

## 8. Glassmorphism Design Primitives (GlassPanel, Button, Container)

### `GlassPanel.tsx`
Two distinct variations:
- `variant="glass"`: Light translucent frosted pane (`bg-white/[0.15]`) with inset specular highlight.
- `variant="solid"`: Deep midnight surface (`bg-midnight-900/95`) for heavy contrast and form inputs.

```tsx
import type { ReactNode } from "react";
import { cn } from "../../lib/cn";

export function GlassPanel({
  className,
  children,
  variant = "glass"
}: {
  className?: string;
  children: ReactNode;
  variant?: "glass" | "solid";
}) {
  return (
    <div
      className={cn(
        "rounded-card border",
        variant === "solid"
          ? "border-white/10 bg-midnight-900/95 shadow-[0_20px_50px_-20px_rgba(0,0,0,0.85)]"
          : "border-white/15 bg-white/[0.15] shadow-[inset_0_1px_0_rgba(255,255,255,0.12)]",
        className
      )}
    >
      {children}
    </div>
  );
}
```

### `Button.tsx` & `LinkButton.tsx`
Interactive physics using Motion with spring parameters `{ stiffness: 400, damping: 20 }`:

```tsx
import type { ButtonHTMLAttributes, ReactNode } from "react";
import { Link } from "react-router-dom";
import { motion } from "motion/react";
import { cn } from "../../lib/cn";

type Variant = "primary" | "secondary" | "ghost";

const VARIANT_CLASSES: Record<Variant, string> = {
  primary: "bg-festival-gold text-midnight-950 font-semibold hover:bg-festival-goldDim shadow-goldGlow",
  secondary: "border border-white/20 bg-white/5 text-white hover:bg-white/10",
  ghost: "text-white/80 hover:text-white"
};

const BASE_CLASSES =
  "inline-flex items-center justify-center gap-2 rounded-pill px-6 py-3 text-sm transition-all whitespace-nowrap disabled:cursor-not-allowed disabled:opacity-40 disabled:grayscale disabled:shadow-none";

const HOVER_TRANSITION = { type: "spring", stiffness: 400, damping: 20 } as const;

interface CommonProps {
  variant?: Variant;
  className?: string;
  children: ReactNode;
}

export function Button({
  variant = "primary",
  className,
  children,
  disabled,
  ...rest
}: CommonProps & Omit<ButtonHTMLAttributes<HTMLButtonElement>, "onDrag">) {
  return (
    <motion.button
      whileHover={disabled ? undefined : { scale: 1.04 }}
      whileTap={disabled ? undefined : { scale: 0.96 }}
      transition={HOVER_TRANSITION}
      disabled={disabled}
      className={cn(BASE_CLASSES, VARIANT_CLASSES[variant], className)}
      {...rest}
    >
      {children}
    </motion.button>
  );
}

const MotionLink = motion.create(Link);

export function LinkButton({
  to,
  variant = "primary",
  className,
  children,
  disabled = false
}: CommonProps & { to: string; disabled?: boolean }) {
  if (disabled) {
    return (
      <span
        aria-disabled="true"
        className={cn(
          BASE_CLASSES,
          VARIANT_CLASSES[variant],
          "pointer-events-none cursor-not-allowed opacity-40 grayscale shadow-none",
          className
        )}
      >
        {children}
      </span>
    );
  }

  return (
    <MotionLink
      to={to}
      whileHover={{ scale: 1.04 }}
      whileTap={{ scale: 0.96 }}
      transition={HOVER_TRANSITION}
      className={cn(BASE_CLASSES, VARIANT_CLASSES[variant], className)}
    >
      {children}
    </MotionLink>
  );
}
```

### `Container.tsx`
```tsx
import type { ReactNode } from "react";
import { cn } from "../../lib/cn";

export function Container({ className, children }: { className?: string; children: ReactNode }) {
  return <div className={cn("mx-auto w-full max-w-page px-5 sm:px-8 lg:px-12", className)}>{children}</div>;
}
```

---

## 9. Bento-Grid Event Highlights

Displays festival specifics across an asymmetric 3-column Bento layout.

```tsx
import { motion } from "motion/react";
import { CalendarBlank, Gift, MapPin, TShirt } from "@phosphor-icons/react";
import { Container } from "../../components/ui/Container";
import { GlassPanel } from "../../components/ui/GlassPanel";

const REVEAL = {
  initial: { opacity: 0, y: 20 },
  whileInView: { opacity: 1, y: 0 },
  viewport: { once: true, amount: 0.4 },
  transition: { duration: 0.5, ease: [0.16, 1, 0.3, 1] as const }
};

export function Highlights() {
  return (
    <section id="highlights" className="py-20 sm:py-28">
      <Container>
        <motion.h2
          {...REVEAL}
          className="font-display text-3xl font-semibold tracking-tight text-white [text-shadow:0_1px_3px_rgba(0,0,0,0.85),0_4px_18px_rgba(0,0,0,0.55)] sm:text-4xl"
        >
          The essentials
        </motion.h2>

        <div className="mt-10 grid grid-cols-1 gap-4 md:grid-cols-3 md:grid-rows-2">
          {/* Main Hero Card (Spans 2 columns and 2 rows) */}
          <motion.div {...REVEAL} className="md:col-span-2 md:row-span-2">
            <GlassPanel variant="solid" className="flex h-full flex-col gap-6 bg-midnight-800/80 p-8">
              <CalendarBlank size={36} weight="duotone" className="text-festival-gold" />
              <div className="flex flex-col gap-3">
                <p className="text-xs uppercase tracking-[0.14em] text-white/50">When and where</p>
                <p className="font-display text-4xl font-semibold leading-tight text-white sm:text-5xl">
                  October 15, 2026
                </p>
                <p className="flex items-center gap-2 text-base text-festival-gold/90 font-medium">
                  <MapPin size={18} weight="fill" /> Acharya Stadium
                </p>
                <div className="mt-2 flex flex-col gap-1.5 border-t border-white/10 pt-4 text-sm text-white/60">
                  <p>Entry: <span className="text-white/85">3:00 PM – 5:00 PM</span> (gate closes 5:00 PM)</p>
                  <p>Event: <span className="text-white/85">4:00 PM – 9:00 PM</span></p>
                  <p className="mt-1 text-xs text-white/40">Entry is one-time only — once you exit, re-entry is prohibited.</p>
                </div>
              </div>
            </GlassPanel>
          </motion.div>

          {/* Kit Card */}
          <motion.div {...REVEAL}>
            <GlassPanel variant="solid" className="flex h-full flex-col justify-between gap-6 bg-midnight-800/80 p-6">
              <Gift size={28} weight="duotone" className="text-indigo-400" />
              <div>
                <p className="text-xs uppercase tracking-[0.14em] text-white/50">Celebration Kit</p>
                <p className="mt-1 font-display text-2xl font-semibold text-white">₹151.00</p>
                <p className="mt-2 text-xs text-white/50 leading-relaxed">
                  Receive your Dandiya Celebration Kit directly at the venue entrance.
                </p>
              </div>
            </GlassPanel>
          </motion.div>

          {/* Dress Code Card */}
          <motion.div {...REVEAL}>
            <GlassPanel variant="solid" className="flex h-full flex-col justify-between gap-6 bg-midnight-800/80 p-6">
              <TShirt size={28} weight="duotone" className="text-festival-magenta" />
              <div>
                <p className="text-xs uppercase tracking-[0.14em] text-white/50">Dress code</p>
                <p className="mt-1 text-sm text-white/75">
                  Traditional and festive. Kurtas, chaniya cholis, and comfortable footwear.
                </p>
              </div>
            </GlassPanel>
          </motion.div>
        </div>
      </Container>
    </section>
  );
}
```

---

## 10. Floating Dynamic Pill Navigation & Mobile Drawer

The header floats cleanly above the content:
- When scroll position <= 10px, background is `bg-transparent`.
- When scrolled > 10px, it blurs into `bg-midnight-950/90 backdrop-blur-md border-b border-white/10`.
- Desktop menu items sit inside a sleek capsule pill (`rounded-pill border border-white/10 bg-white/5 p-1.5`).

```tsx
import { useEffect, useState } from "react";
import { Link } from "react-router-dom";
import { InstagramLogo, List, X } from "@phosphor-icons/react";
import { motion, AnimatePresence } from "motion/react";
import { Container } from "../ui/Container";
import { LinkButton } from "../ui/Button";

const NAV_LINKS = [
  { label: "Event", href: "#highlights" },
  { label: "Rules", href: "#rules" },
  { label: "Check Status", to: "/registration/status" }
];

export function SiteNav({ minimal = false }: { minimal?: boolean }) {
  const [open, setOpen] = useState(false);
  const [scrolled, setScrolled] = useState(false);

  useEffect(() => {
    function onScroll() {
      setScrolled(window.scrollY > 10);
    }
    window.addEventListener("scroll", onScroll, { passive: true });
    return () => window.removeEventListener("scroll", onScroll);
  }, []);

  const navBg = scrolled
    ? "bg-midnight-950/90 backdrop-blur-md border-b border-white/10 shadow-[0_1px_20px_rgba(0,0,0,0.4)]"
    : "bg-transparent";

  return (
    <header className={`sticky top-0 z-40 transition-all duration-300 ${navBg}`}>
      <Container className="flex h-16 items-center justify-between">
        <Link to="/" className="flex items-center gap-2.5">
          <img src="/acharya-mark.png" alt="Logo" className="h-9 w-auto sm:h-10" />
          <span className="font-display text-lg font-semibold tracking-tight text-white">
            Aarambh <span className="text-festival-gold">2026</span>
          </span>
        </Link>

        {/* Desktop Menu */}
        <div className="hidden items-center gap-3 md:flex">
          <nav className="flex items-center gap-2 rounded-pill border border-white/10 bg-white/5 p-1.5">
            {NAV_LINKS.map((link) =>
              link.to ? (
                <Link
                  key={link.label}
                  to={link.to}
                  className="rounded-pill px-4 py-1.5 text-sm text-white/75 transition-colors hover:bg-white/10 hover:text-white"
                >
                  {link.label}
                </Link>
              ) : (
                <a
                  key={link.label}
                  href={link.href}
                  className="rounded-pill px-4 py-1.5 text-sm text-white/75 transition-colors hover:bg-white/10 hover:text-white"
                >
                  {link.label}
                </a>
              )
            )}
            <LinkButton to="/register" className="px-5 py-2 text-sm">
              Register Now
            </LinkButton>
          </nav>
        </div>

        {/* Mobile Hamburger */}
        <button
          type="button"
          className="flex h-10 w-10 items-center justify-center rounded-pill border border-white/10 bg-white/5 text-white md:hidden"
          onClick={() => setOpen((value) => !value)}
        >
          {open ? <X size={22} /> : <List size={22} />}
        </button>
      </Container>

      {/* Mobile Animated Dropdown */}
      <AnimatePresence>
        {open && (
          <motion.nav
            initial={{ height: 0, opacity: 0 }}
            animate={{ height: "auto", opacity: 1 }}
            exit={{ height: 0, opacity: 0 }}
            transition={{ duration: 0.25, ease: [0.16, 1, 0.3, 1] }}
            className="overflow-hidden border-t border-white/10 bg-midnight-950/90 md:hidden"
          >
            <Container className="flex flex-col gap-4 py-5">
              {NAV_LINKS.map((link) =>
                link.to ? (
                  <Link key={link.label} to={link.to} onClick={() => setOpen(false)} className="text-sm text-white/80">
                    {link.label}
                  </Link>
                ) : (
                  <a key={link.label} href={link.href} onClick={() => setOpen(false)} className="text-sm text-white/80">
                    {link.label}
                  </a>
                )
              )}
              <LinkButton to="/register" className="w-full">
                Register Now
              </LinkButton>
            </Container>
          </motion.nav>
        )}
      </AnimatePresence>
    </header>
  );
}
```

---

## 11. Step-by-Step Replication Blueprint for Aarambh 2026

To construct the frontend for **Aarambh 2026** with this identical architecture:

1. **Initialize Project & Structure:**
   ```bash
   npm create vite@latest aarambh2026-web -- --template react-ts
   cd aarambh2026-web
   npm install react-router-dom motion clsx tailwind-merge @phosphor-icons/react @fontsource-variable/outfit
   npm install -D tailwindcss postcss autoprefixer sharp
   npx tailwindcss init -p
   ```
2. **Inject Design Tokens:**
   - Replace `tailwind.config.ts` with the config from Section 3.
   - Replace `src/index.css` with the global Outfit font import and midnight body style.
   - Add `src/lib/cn.ts`.
3. **Set Up Image Pipeline:**
   - Create `scripts/optimize-images.mjs` and add `"optimize-images": "node scripts/optimize-images.mjs"` to `package.json`.
   - Put 20-30 festival/event photos in `public/images/`.
   - Run `npm run optimize-images`.
   - Generate `src/lib/gallery-manifest.ts` listing the WebP filenames.
4. **Mount Visual Foundations:**
   - Create `src/components/gallery/BambooGallery.tsx`.
   - Create `src/components/ui/GlassPanel.tsx`, `Button.tsx`, and `Container.tsx`.
   - Create `src/components/Countdown.tsx`.
5. **Assemble Landing Page:**
   ```tsx
   import { BambooGallery } from "./components/gallery/BambooGallery";
   import { SiteNav } from "./components/layout/SiteNav";
   import { Hero } from "./routes/landing/Hero";
   import { Highlights } from "./routes/landing/Highlights";
   import { MemoryGallery } from "./routes/landing/MemoryGallery";
   import { Rules } from "./routes/landing/Rules";
   import { FinalCta } from "./routes/landing/FinalCta";
   import { SiteFooter } from "./components/layout/SiteFooter";

   export function LandingPage() {
     return (
       <div className="relative min-h-screen">
         {/* Background photo animation covers whole site */}
         <BambooGallery />
         
         <SiteNav />
         <main className="relative z-10">
           <Hero />
           <Highlights />
           <MemoryGallery />
           <Rules />
           <FinalCta />
         </main>
         <SiteFooter />
       </div>
     );
   }
   ```

---

## 12. Theme Customization Matrix for Aarambh 2026

Customize the color palette in `tailwind.config.ts` depending on the exact theme of Aarambh 2026:

| Theme Identity | Midnight Base (`midnight-950`) | Primary Accent (`festival-gold`) | Secondary Accent (`festival-magenta`) | Atmosphere |
|---|---|---|---|---|
| **Dandiya / Raas (Current)** | `#07080f` | `#ecc07a` (Warm Gold) | `#d63aa8` (Vibrant Magenta) | Traditional festive night |
| **Aarambh College Freshers** | `#090919` | `#38bdf8` (Electric Sky Blue) | `#f43f5e` (Neon Rose) | Youthful, energetic neon |
| **Tech Fest / Hackathon** | `#030712` | `#10b981` (Cyber Emerald) | `#8b5cf6` (Deep Violet) | Futuristic dark-mode |
| **Cultural Sunburst** | `#0c0a1a` | `#f59e0b` (Solar Amber) | `#ef4444` (Crimson Flame) | Vibrant celebratory stage |

To change colors, update the hex codes in `tailwind.config.ts` — all glowing shadows, borders, and buttons will adapt automatically.
