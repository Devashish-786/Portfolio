import React, { useState, useEffect, useRef, useMemo, useCallback } from 'react';
import {
ArrowUpRight,
Sliders,
Grid,
List,
Terminal,
Sparkles,
Volume2,
VolumeX,
Copy,
Check,
Eye,
Code2,
Cpu,
Globe,
Layers,
Plus,
Trash2,
Edit3,
RefreshCw,
ChevronRight,
Maximize2,
Minimize2,
Circle,
Command,
FileText,
GitBranch,
Activity,
Compass,
Sun,
Moon,
Zap,
X
} from 'lucide-react';

// ============================================================================
// WEB AUDIO API TACTILE SYNTHESIZER (Zero external audio files required)
// ============================================================================
class TactileAudioEngine {
constructor() {
this.ctx = null;
this.enabled = false;
}

init() {
if (!this.ctx && typeof window !== 'undefined') {
const AudioCtx = window.AudioContext || window.webkitAudioContext;
if (AudioCtx) this.ctx = new AudioCtx();
}
}

playTick(freq = 800, duration = 0.018, type = 'sine', volume = 0.035) {
if (!this.enabled) return;
try {
this.init();
if (!this.ctx) return;
if (this.ctx.state === 'suspended') this.ctx.resume();

```
  const osc = this.ctx.createOscillator();
  const gain = this.ctx.createGain();
  osc.type = type;
  osc.frequency.setValueAtTime(freq, this.ctx.currentTime);
  osc.frequency.exponentialRampToValueAtTime(freq * 0.4, this.ctx.currentTime + duration);

  gain.gain.setValueAtTime(volume, this.ctx.currentTime);
  gain.gain.exponentialRampToValueAtTime(0.0001, this.ctx.currentTime + duration);

  osc.connect(gain);
  gain.connect(this.ctx.destination);

  osc.start();
  osc.stop(this.ctx.currentTime + duration);
} catch {
  // Ignore audio errors if blocked by browser autoplay policy
}

```

}

hover() {
this.playTick(1100, 0.012, 'sine', 0.02);
}

click() {
this.playTick(520, 0.035, 'triangle', 0.05);
}

switchMode() {
this.playTick(740, 0.045, 'sine', 0.04);
}
}

const soundEngine = new TactileAudioEngine();

// ============================================================================
// DEFAULT PERSONALIZABLE CONFIG (Fresh Slate - No Old Projects)
// ============================================================================
const INITIAL_IDENTITY = {
name: 'AARAV SHARMA', // Click "Personalize" in top bar to put your exact name
monogram: 'AS—26',
role: 'Systems Architect & Creative Technologist',
tagline: 'Engineering silent, high-frequency digital interfaces where mathematical precision meets tactile minimalism.',
location: '22.7196° N, 75.8577° E // INDORE, IN',
timezone: 'IST (UTC+5:30)',
status: 'Open for Select Architecture & R&D',
email: 'hello@yourdomain.dev',
github: 'github.com/yourusername/kinetic-folio',
xHandle: '@yourusername',
showFreshSlateMode: false, // Toggle between 0 projects (Pure Fresh Slate) or Interactive Sandbox Modules
};

// Fresh architectural capabilities / experimental sandboxes (not copied projects)
const INITIAL_SANDBOXES = \[
{
id: 'SYS-01',
title: 'Verlet Kinetic Cloth & Field Engine',
category: 'GPU / Physics / GLSL',
year: '2026',
status: 'LIVE SANDBOX',
metrics: '120 FPS · 0.4ms frame',
summary: 'Zero-dependency spatial physics solver using continuous spring-damper constraints and reactive pointer velocity.',
stack: \['TypeScript', 'HTML5 Canvas', 'Verlet Physics', 'Tailwind'\],
demoType: 'wave'
},
{
id: 'SYS-02',
title: 'Sub-Millisecond Edge Telemetry Core',
category: 'Distributed Systems',
year: '2026',
status: 'ARCHITECTURE',
metrics: '99.99% Uptime · Edge',
summary: 'Deterministic state synchronization protocol built for ultra-low-latency collaborative canvas applications.',
stack: \['Next.js', 'WebSockets', 'Rust WASM', 'Redis'\],
demoType: 'telemetry'
},
{
id: 'SYS-03',
title: 'Swiss Grid Typographic Synthesizer',
category: 'Design Engineering',
year: '2026',
status: 'OPEN SOURCE',
metrics: 'Variable Axis · 4KB',
summary: 'Procedural layout and variable font optical-sizing engine driven by real-time scroll inertia and viewport geometry.',
stack: \['React 19', 'CSS Houdini', 'Spring Physics', 'SVG'\],
demoType: 'typography'
}
\];

const REPO_FILES = \[
{
path: 'src/app/page.tsx',
role: 'Primary Entry & Spatial Layout Controller',
lines: 248,
description: 'Orchestrates smooth spring scroll variables, view-mode transitions (Index / Bento / Blueprint), and keyboard shortcuts.'
},
{
path: 'src/components/canvas/VectorFieldCanvas.tsx',
role: '60–120FPS Hardware-Accelerated Field',
lines: 184,
description: 'Renders the reactive topological vector lattice and harmonic ripples using requestAnimationFrame and spatial hashing.'
},
{
path: 'src/lib/audio/tactileSynth.ts',
role: 'Procedural Web Audio Micro-Feedback',
lines: 76,
description: 'Synthesizes zero-latency sine/triangle micro-clicks on hover and state mutations without loading external MP3 assets.'
},
{
path: 'src/config/identity.config.ts',
role: 'Single-Source Identity & Slate Manifest',
lines: 62,
description: 'Typed configuration file allowing instant fork customization of name, coordinates, social links, and sandbox modules.'
}
\];

// ============================================================================
// INTERACTIVE MINI-CANVAS FOR BENTO CARDS
// ============================================================================
function MiniSandboxPreview({ type, theme }) {
const \[tick, setTick\] = useState(0);
const \[hovered, setHovered\] = useState(false);

useEffect(() => {
const id = setInterval(() => {
setTick((t) => (t + 1) % 100);
}, 50);
return () => clearInterval(id);
}, \[\]);

const strokeColor =
theme === 'light' ? 'rgba(15,15,15,0.65)' : theme === 'blueprint' ? 'rgba(56,189,248,0.8)' : 'rgba(255,255,255,0.75)';

if (type === 'wave') {
return (
<div
onMouseEnter={() => setHovered(true)}
onMouseLeave={() => setHovered(false)}
className="relative h-36 w-full overflow-hidden rounded-lg border border-current/10 bg-current/\[0.02\] flex items-center justify-center"
\>

{\[0, 1, 2, 3\].map((row) => {
const points = Array.from({ length: 24 }, (\_, i) => {
const x = i \* 10.5;
const amp = hovered ? 16 : 7;
const y =
45 +
(row - 1.5) \* 8 +
Math.sin((i \* 0.45) + (tick \* 0.18) + row \* 0.7) \* amp;
return `${x.toFixed(1)},${y.toFixed(1)}`;
}).join(' ');
return (
<polyline
key={row}
fill="none"
stroke={strokeColor}
strokeWidth={row === 1 ? '1.5' : '0.75'}
strokeOpacity={1 - row \* 0.2}
points={points}
/>
);
})}


{hovered ? 'AMPLITUDE: HIGH' : 'HOVER TO EXCITE'}


);
}

if (type === 'telemetry') {
return (
<div
onMouseEnter={() => setHovered(true)}
onMouseLeave={() => setHovered(false)}
className="relative h-36 w-full overflow-hidden rounded-lg border border-current/10 bg-current/\[0.02\] p-3 flex flex-col justify-between font-mono text-\[11px\]"
\>

NODE_CLUSTER // EDGE


{(0.28 + Math.sin(tick \* 0.2) \* 0.09).toFixed(2)}ms



{Array.from({ length: 12 }, (\_, i) => {
const h = Math.max(
18,
Math.min(100, Math.round(50 + Math.sin(tick \* 0.25 + i \* 0.9) \* (hovered ? 45 : 28)))
);
return (
<div
key={i}
className="w-full rounded-sm transition-all duration-75"
style={{
height: `${h}%`,
backgroundColor: strokeColor,
opacity: i % 3 === 0 ? 0.85 : 0.35
}}
/>
);
})}


PACKET_LOSS: 0.000%
SYNC: DETERMINISTIC


);
}

return (
<div
onMouseEnter={() => setHovered(true)}
onMouseLeave={() => setHovered(false)}
className="relative h-36 w-full overflow-hidden rounded-lg border border-current/10 bg-current/\[0.02\] flex flex-col items-center justify-center p-4"
\>
<div
className="font-serif italic transition-all duration-300 select-none"
style={{
fontSize: hovered ? '2.5rem' : '1.9rem',
letterSpacing: hovered ? '0.18em' : '-0.03em'
}}
\>
Aa Bb Σ Ω


OPTICAL AXIS · {hovered ? 'EXPANDED \[800\]' : 'COMPACT \[300\]'}


);
}

// ============================================================================
// MAIN APPLICATION COMPONENT
// ============================================================================
export default function App() {
// Personalization & Content State
const \[identity, setIdentity\] = useState(INITIAL_IDENTITY);
const \[sandboxes, setSandboxes\] = useState(INITIAL_SANDBOXES);
const \[isCustomizerOpen, setIsCustomizerOpen\] = useState(false);

// Interactive UI Modes
const \[viewMode, setViewMode\] = useState('index'); // 'index' | 'bento' | 'repo'
const \[canvasMode, setCanvasMode\] = useState('lattice'); // 'lattice' | 'topography' | 'orbital' | 'minimal'
const \[theme, setTheme\] = useState('obsidian'); // 'obsidian' | 'alabaster' | 'blueprint'
const \[soundEnabled, setSoundEnabled\] = useState(false);

// Hover & Telemetry State
const \[hoveredItem, setHoveredItem\] = useState(null);
const \[mousePos, setMousePos\] = useState({ x: 0, y: 0, vx: 0, vy: 0 });
const \[clock, setClock\] = useState('');
const \[fps, setFps\] = useState(60);
const \[copiedKey, setCopiedKey\] = useState(null);
const \[newSandboxForm, setNewSandboxForm\] = useState({
title: '',
category: 'Interactive System',
summary: '',
stack: 'Next.js, TypeScript, WebGL'
});
const \[showAddForm, setShowAddForm\] = useState(false);

// Refs for Canvas & Cursor Physics
const canvasRef = useRef(null);
const cursorRingRef = useRef(null);
const pointerRef = useRef({ x: 500, y: 350, tx: 500, ty: 350, vx: 0, vy: 0, down: false });

// Inject Google Fonts
useEffect(() => {
const link = document.createElement('link');
link.rel = 'stylesheet';
link.href =
'https://fonts.googleapis.com/css2?family=Instrument+Serif:ital@0;1&family=JetBrains+Mono:wght@300;400;500&family=Space+Grotesk:wght@300;400;500;600;700&display=swap';
document.head.appendChild(link);
}, \[\]);

// Live Clock Telemetry
useEffect(() => {
const updateClock = () => {
const now = new Date();
setClock(
now.toLocaleTimeString('en-US', {
hour12: false,
hour: '2-digit',
minute: '2-digit',
second: '2-digit'
}) + `.${String(now.getMilliseconds()).padStart(3, '0').slice(0, 2)}`
);
};
updateClock();
const timer = setInterval(updateClock, 80);
return () => clearInterval(timer);
}, \[\]);

// Toggle Sound
const toggleSound = () => {
const next = !soundEnabled;
soundEngine.enabled = next;
setSoundEnabled(next);
if (next) soundEngine.click();
};

// Copy Helper
const copyToClipboard = useCallback((text, key) => {
navigator.clipboard?.writeText(text);
soundEngine.click();
setCopiedKey(key);
setTimeout(() => setCopiedKey(null), 2000);
}, \[\]);

// Keyboard Shortcuts (1/2/3 for views, C for canvas, T for theme, P for Personalize)
useEffect(() => {
const onKeyDown = (e) => {
if (\['INPUT', 'TEXTAREA'\].includes(document.activeElement?.tagName)) return;
if (e.key === '1') {
soundEngine.switchMode();
setViewMode('index');
} else if (e.key === '2') {
soundEngine.switchMode();
setViewMode('bento');
} else if (e.key === '3') {
soundEngine.switchMode();
setViewMode('repo');
} else if (e.key.toLowerCase() === 'c') {
soundEngine.switchMode();
const modes = \['lattice', 'topography', 'orbital', 'minimal'\];
setCanvasMode((prev) => modes\[(modes.indexOf(prev) + 1) % modes.length\]);
} else if (e.key.toLowerCase() === 't') {
soundEngine.switchMode();
const themes = \['obsidian', 'alabaster', 'blueprint'\];
setTheme((prev) => themes\[(themes.indexOf(prev) + 1) % themes.length\]);
} else if (e.key.toLowerCase() === 'p') {
soundEngine.click();
setIsCustomizerOpen((prev) => !prev);
}
};
window.addEventListener('keydown', onKeyDown);
return () => window.removeEventListener('keydown', onKeyDown);
}, \[\]);

// ============================================================================
// HARDWARE-ACCELERATED INTERACTIVE BACKGROUND CANVAS + SPRING CURSOR
// ============================================================================
useEffect(() => {
const canvas = canvasRef.current;
if (!canvas) return;
const ctx = canvas.getContext('2d');
let animationFrameId;
let width = (canvas.width = window.innerWidth);
let height = (canvas.height = window.innerHeight);

```
const handleResize = () => {
  if (!canvas) return;
  width = canvas.width = window.innerWidth;
  height = canvas.height = window.innerHeight;
};

const handleMouseMove = (e) => {
  const p = pointerRef.current;
  p.vx = e.clientX - p.tx;
  p.vy = e.clientY - p.ty;
  p.tx = e.clientX;
  p.ty = e.clientY;
  setMousePos({
    x: Math.round(e.clientX),
    y: Math.round(e.clientY),
    vx: Math.round(Math.hypot(p.vx, p.vy))
  });
};

const handleMouseDown = () => {
  pointerRef.current.down = true;
};
const handleMouseUp = () => {
  pointerRef.current.down = false;
};

window.addEventListener('resize', handleResize);
window.addEventListener('mousemove', handleMouseMove);
window.addEventListener('mousedown', handleMouseDown);
window.addEventListener('mouseup', handleMouseUp);

let lastTime = performance.now();
let frameCount = 0;
let timePhase = 0;

const render = (now) => {
  frameCount++;
  if (now - lastTime >= 500) {
    setFps(Math.min(120, Math.round((frameCount * 1000) / (now - lastTime))));
    frameCount = 0;
    lastTime = now;
  }

  timePhase += 0.014;
  const p = pointerRef.current;
  // Spring lerp pointer
  p.x += (p.tx - p.x) * 0.14;
  p.y += (p.ty - p.y) * 0.14;

  // Update custom cursor ring DOM directly for zero React re-render lag
  if (cursorRingRef.current) {
    const speed = Math.min(35, Math.hypot(p.tx - p.x, p.ty - p.y));
    const scale = p.down ? 0.75 : 1 + speed * 0.012;
    cursorRingRef.current.style.transform = `translate3d(${p.x - 16}px, ${p.y - 16}px, 0) scale(${scale})`;
  }

  ctx.clearRect(0, 0, width, height);

  const rgb =
    theme === 'alabaster'
      ? '18, 18, 20'
      : theme === 'blueprint'
      ? '56, 189, 248'
      : '235, 235, 240';

  if (canvasMode === 'lattice') {
    const spacing = width < 768 ? 56 : 46;
    const cols = Math.ceil(width / spacing) + 1;
    const rows = Math.ceil(height / spacing) + 1;

    for (let r = 0; r < rows; r++) {
      for (let c = 0; c < cols; c++) {
        const gx = c * spacing;
        const gy = r * spacing;
        const dx = gx - p.x;
        const dy = gy - p.y;
        const dist = Math.hypot(dx, dy);
        const maxRadius = p.down ? 280 : 190;

        let ox = gx;
        let oy = gy;
        let alpha = 0.11;

        if (dist < maxRadius && dist > 1) {
          const force = Math.pow(1 - dist / maxRadius, 2);
          const pull = p.down ? -28 : 22;
          ox += (dx / dist) * force * pull;
          oy += (dy / dist) * force * pull;
          alpha = 0.12 + force * 0.55;
        }

        // Subtle harmonic breathing
        ox += Math.sin(timePhase + r * 0.35 + c * 0.2) * 1.8;
        oy += Math.cos(timePhase + c * 0.35) * 1.8;

        ctx.fillStyle = `rgba(${rgb}, ${alpha})`;
        ctx.beginPath();
        ctx.arc(ox, oy, dist < maxRadius ? 1.6 : 1.0, 0, Math.PI * 2);
        ctx.fill();

        if (dist < 115) {
          ctx.strokeStyle = `rgba(${rgb}, ${(1 - dist / 115) * 0.18})`;
          ctx.lineWidth = 0.7;
          ctx.beginPath();
          ctx.moveTo(ox, oy);
          ctx.lineTo(p.x, p.y);
          ctx.stroke();
        }
      }
    }
  } else if (canvasMode === 'topography') {
    const lines = 18;
    ctx.lineWidth = 1;
    for (let i = 0; i < lines; i++) {
      const baseY = (height / (lines + 1)) * (i + 1);
      ctx.strokeStyle = `rgba(${rgb}, ${0.06 + (i % 3) * 0.04})`;
      ctx.beginPath();
      for (let x = 0; x <= width; x += 24) {
        const dx = x - p.x;
        const dy = baseY - p.y;
        const dist = Math.hypot(dx, dy);
        const influence = Math.max(0, 1 - dist / 240);
        const wave =
          Math.sin(x * 0.006 + timePhase + i * 0.45) * 14 +
          Math.cos(x * 0.015 - timePhase * 0.8) * 6 +
          influence * Math.sin(dist * 0.05 - timePhase * 3) * 34;

        if (x === 0) ctx.moveTo(x, baseY + wave);
        else ctx.lineTo(x, baseY + wave);
      }
      ctx.stroke();
    }
  } else if (canvasMode === 'orbital') {
    const cx = width * 0.5 + (p.x - width * 0.5) * 0.12;
    const cy = height * 0.5 + (p.y - height * 0.5) * 0.12;
    for (let ring = 1; ring <= 7; ring++) {
      const radius = ring * Math.min(width, height) * 0.075;
      ctx.strokeStyle = `rgba(${rgb}, ${0.15 - ring * 0.015})`;
      ctx.lineWidth = 0.8;
      ctx.beginPath();
      ctx.arc(cx, cy, radius, timePhase * (ring % 2 ? 0.4 : -0.3), timePhase * (ring % 2 ? 0.4 : -0.3) + Math.PI * 1.45);
      ctx.stroke();

      // Satellite node on each ring
      const angle = timePhase * (0.9 - ring * 0.08) + ring;
      const sx = cx + Math.cos(angle) * radius;
      const sy = cy + Math.sin(angle) * radius;
      ctx.fillStyle = `rgba(${rgb}, 0.65)`;
      ctx.beginPath();
      ctx.arc(sx, sy, 2.2, 0, Math.PI * 2);
      ctx.fill();
    }
  }

  animationFrameId = requestAnimationFrame(render);
};

animationFrameId = requestAnimationFrame(render);

return () => {
  cancelAnimationFrame(animationFrameId);
  window.removeEventListener('resize', handleResize);
  window.removeEventListener('mousemove', handleMouseMove);
  window.removeEventListener('mousedown', handleMouseDown);
  window.removeEventListener('mouseup', handleMouseUp);
};

```

}, \[canvasMode, theme\]);

// Add a Custom Sandbox Module
const handleAddSandbox = (e) => {
e.preventDefault();
if (!newSandboxForm.title.trim()) return;
soundEngine.click();
const nextId = `SYS-0${sandboxes.length + 1}`;
const demoTypes = \['wave', 'telemetry', 'typography'\];
setSandboxes(\[
...sandboxes,
{
id: nextId,
title: newSandboxForm.title.trim(),
category: newSandboxForm.category.trim() || 'Architecture',
year: '2026',
status: 'ACTIVE',
metrics: '60+ FPS · Custom',
summary:
newSandboxForm.summary.trim() ||
'Custom interactive module engineered with minimal footprint and high-frequency feedback.',
stack: newSandboxForm.stack
.split(',')
.map((s) => s.trim())
.filter(Boolean),
demoType: demoTypes\[sandboxes.length % demoTypes.length\]
}
\]);
setNewSandboxForm({ title: '', category: 'Interactive System', summary: '', stack: 'Next.js, TypeScript, WebGL' });
setShowAddForm(false);
setIdentity((prev) => ({ ...prev, showFreshSlateMode: false }));
};

const removeSandbox = (id) => {
soundEngine.click();
setSandboxes((prev) => prev.filter((item) => item.id !== id));
};

// Theme Styles
const themeClasses = useMemo(() => {
if (theme === 'alabaster') {
return {
bg: 'bg-\[#F4F3EF\] text-\[#111113\]',
panel: 'bg-\[#EAE8E1\]/80 border-black/10',
pill: 'bg-black/\[0.04\] hover:bg-black/\[0.08\] border-black/10',
accent: 'text-emerald-700',
badge: 'bg-black text-\[#F4F3EF\]'
};
}
if (theme === 'blueprint') {
return {
bg: 'bg-\[#061320\] text-\[#E0F2FE\]',
panel: 'bg-\[#091D30\]/80 border-sky-400/20',
pill: 'bg-sky-400/\[0.07\] hover:bg-sky-400/\[0.14\] border-sky-400/20',
accent: 'text-sky-300',
badge: 'bg-sky-400 text-\[#061320\]'
};
}
return {
bg: 'bg-\[#09090B\] text-\[#F4F4F6\]',
panel: 'bg-\[#121216\]/80 border-white/10',
pill: 'bg-white/\[0.04\] hover:bg-white/\[0.09\] border-white/10',
accent: 'text-emerald-400',
badge: 'bg-white text-\[#09090B\]'
};
}, \[theme\]);

// Dynamic Config Export String for User's Repo
const configExportCode = useMemo(() => {
return \`// src/config/identity.config.ts
export const IDENTITY_CONFIG = \${JSON.stringify(identity, null, 2)};

export const SANDBOX_MODULES = \${JSON.stringify(
identity.showFreshSlateMode ? \[\] : sandboxes,
null,
2
)};\`;
}, \[identity, sandboxes\]);

return (
<div
className={`min-h-screen w-full transition-colors duration-500 selection:bg-emerald-400 selection:text-black relative overflow-x-hidden ${themeClasses.bg}`}
style={{ fontFamily: "'Space Grotesk', sans-serif" }}
>
{/\* HARDWARE-ACCELERATED INTERACTIVE CANVAS \*/}

```
  {/* CUSTOM SPRING PHYSICS CURSOR RING (Desktop) */}
  <div
    ref={cursorRingRef}
    className="fixed top-0 left-0 w-8 h-8 rounded-full border border-current/30 pointer-events-none z-50 hidden md:flex items-center justify-center transition-transform duration-75 ease-out"
  >
    <div className="w-1 h-1 rounded-full bg-current opacity-70" />
  </div>

  {/* ARCHITECTURAL HAIRLINE GRID OVERLAY */}
  <div className="fixed inset-0 pointer-events-none z-0 flex justify-between max-w-7xl mx-auto px-6 sm:px-10 opacity-[0.04]">
    <div className="w-px h-full bg-current" />
    <div className="w-px h-full bg-current hidden sm:block" />
    <div className="w-px h-full bg-current hidden lg:block" />
    <div className="w-px h-full bg-current" />
  </div>

  {/* =====================================================================
      TOP TELEMETRY & CONTROL HUD BAR
  ===================================================================== */}
  <header className="sticky top-0 z-40 backdrop-blur-md border-b border-current/10">
    <div className="max-w-7xl mx-auto px-5 sm:px-10 h-16 flex items-center justify-between gap-4">
      {/* Left: Monogram & Status */}
      <div className="flex items-center gap-4">
        <button
          onClick={() => {
            soundEngine.click();
            setIsCustomizerOpen(true);
          }}
          onMouseEnter={() => soundEngine.hover()}
          className="group flex items-center gap-2.5 text-left"
          title="Click to personalize your name & identity"
        >
          <span className={`px-2 py-0.5 text-xs font-mono font-medium rounded ${themeClasses.badge}`}>
            {identity.monogram}
          </span>
          <span className="font-semibold tracking-tight text-sm sm:text-base group-hover:underline">
            {identity.name}
          </span>
          <Edit3 className="w-3.5 h-3.5 opacity-40 group-hover:opacity-100 transition-opacity" />
        </button>

        <div className="hidden lg:flex items-center gap-2 pl-4 border-l border-current/10 text-xs font-mono opacity-65">
          <span className="h-2 w-2 rounded-full bg-emerald-400 animate-pulse" />
          <span>{identity.status}</span>
        </div>
      </div>

      {/* Center: Live Telemetry Readout */}
      <div className="hidden xl:flex items-center gap-6 font-mono text-[11px] opacity-55">
        <span>SYS_CLOCK: {clock}</span>
        <span>RENDER: {fps} FPS</span>
        <span>
          PTR: {String(mousePos.x).padStart(4, '0')}×{String(mousePos.y).padStart(4, '0')}
        </span>
      </div>

      {/* Right: Interactive Switches (Canvas, Theme, Audio, Personalize) */}
      <div className="flex items-center gap-2">
        {/* Canvas Physics Switcher */}
        <button
          onClick={() => {
            soundEngine.switchMode();
            const modes = ['lattice', 'topography', 'orbital', 'minimal'];
            setCanvasMode(modes[(modes.indexOf(canvasMode) + 1) % modes.length]);
          }}
          onMouseEnter={() => soundEngine.hover()}
          className={`px-2.5 py-1.5 rounded-md border text-xs font-mono flex items-center gap-1.5 transition-all ${themeClasses.pill}`}
          title="Cycle interactive background shader [Key: C]"
        >
          <Sparkles className="w-3.5 h-3.5" />
          <span className="hidden sm:inline uppercase">{canvasMode}</span>
        </button>

        {/* Theme Polarity Switcher */}
        <button
          onClick={() => {
            soundEngine.switchMode();
            const themes = ['obsidian', 'alabaster', 'blueprint'];
            setTheme(themes[(themes.indexOf(theme) + 1) % themes.length]);
          }}
          onMouseEnter={() => soundEngine.hover()}
          className={`p-1.5 rounded-md border text-xs font-mono transition-all ${themeClasses.pill}`}
          title="Switch visual polarity [Key: T]"
        >
          {theme === 'alabaster' ? (
            <Sun className="w-4 h-4" />
          ) : theme === 'blueprint' ? (
            <Compass className="w-4 h-4" />
          ) : (
            <Moon className="w-4 h-4" />
          )}
        </button>

        {/* Web Audio Synthesizer Toggle */}
        <button
          onClick={toggleSound}
          onMouseEnter={() => soundEngine.hover()}
          className={`p-1.5 rounded-md border text-xs font-mono transition-all ${themeClasses.pill}`}
          title="Toggle tactile Web Audio synthesizer"
        >
          {soundEnabled ? <Volume2 className="w-4 h-4 text-emerald-400" /> : <VolumeX className="w-4 h-4 opacity-50" />}
        </button>

        {/* Personalize / Identity Studio Drawer Trigger */}
        <button
          onClick={() => {
            soundEngine.click();
            setIsCustomizerOpen(!isCustomizerOpen);
          }}
          onMouseEnter={() => soundEngine.hover()}
          className={`px-3 py-1.5 rounded-md text-xs font-mono font-medium flex items-center gap-1.5 transition-all ${themeClasses.badge}`}
        >
          <Sliders className="w-3.5 h-3.5" />
          <span>Personalize</span>
        </button>
      </div>
    </div>
  </header>

  {/* =====================================================================
      LIVE IDENTITY & STUDIO CUSTOMIZER DRAWER (Put Your Name & Slate)
  ===================================================================== */}
  {isCustomizerOpen && (
    <div className="fixed inset-0 z-50 flex justify-end bg-black/60 backdrop-blur-sm animate-fadeIn">
      <div
        className={`w-full max-w-lg h-full overflow-y-auto border-l p-6 sm:p-8 flex flex-col justify-between ${themeClasses.bg} border-current/15 shadow-2xl`}
      >
        <div>
          <div className="flex items-center justify-between pb-5 border-b border-current/10">
            <div>
              <span className="font-mono text-[10px] uppercase tracking-widest opacity-50">
                LIVE STUDIO CONFIGURATOR // KEY: [P]
              </span>
              <h2 className="text-xl font-semibold tracking-tight mt-0.5">
                Personalize Your Identity & Slate
              </h2>
            </div>
            <button
              onClick={() => setIsCustomizerOpen(false)}
              className={`p-2 rounded-md border ${themeClasses.pill}`}
            >
              <X className="w-4 h-4" />
            </button>
          </div>

          {/* Form Controls */}
          <div className="space-y-4 mt-6 text-sm">
            <div className="grid grid-cols-3 gap-3">
              <div className="col-span-2">
                <label className="block font-mono text-[11px] uppercase opacity-60 mb-1">
                  Your Full Name / Studio Title
                </label>
                <input
                  type="text"
                  value={identity.name}
                  onChange={(e) => setIdentity({ ...identity, name: e.target.value })}
                  className="w-full px-3 py-2 rounded-md bg-current/[0.05] border border-current/15 font-medium focus:outline-none focus:border-emerald-400"
                />
              </div>
              <div>
                <label className="block font-mono text-[11px] uppercase opacity-60 mb-1">
                  Monogram
                </label>
                <input
                  type="text"
                  value={identity.monogram}
                  onChange={(e) => setIdentity({ ...identity, monogram: e.target.value })}
                  className="w-full px-3 py-2 rounded-md bg-current/[0.05] border border-current/15 font-mono focus:outline-none focus:border-emerald-400"
                />
              </div>
            </div>

            <div>
              <label className="block font-mono text-[11px] uppercase opacity-60 mb-1">
                Primary Role / Discipline
              </label>
              <input
                type="text"
                value={identity.role}
                onChange={(e) => setIdentity({ ...identity, role: e.target.value })}
                className="w-full px-3 py-2 rounded-md bg-current/[0.05] border border-current/15 focus:outline-none focus:border-emerald-400"
              />
            </div>

            <div>
              <label className="block font-mono text-[11px] uppercase opacity-60 mb-1">
                Architectural Manifesto / Tagline
              </label>
              <textarea
                rows={2}
                value={identity.tagline}
                onChange={(e) => setIdentity({ ...identity, tagline: e.target.value })}
                className="w-full px-3 py-2 rounded-md bg-current/[0.05] border border-current/15 focus:outline-none focus:border-emerald-400"
              />
            </div>

            <div className="grid grid-cols-1 sm:grid-cols-2 gap-3">
              <div>
                <label className="block font-mono text-[11px] uppercase opacity-60 mb-1">
                  Coordinates & City
                </label>
                <input
                  type="text"
                  value={identity.location}
                  onChange={(e) => setIdentity({ ...identity, location: e.target.value })}
                  className="w-full px-3 py-2 rounded-md bg-current/[0.05] border border-current/15 font-mono text-xs focus:outline-none focus:border-emerald-400"
                />
              </div>
              <div>
                <label className="block font-mono text-[11px] uppercase opacity-60 mb-1">
                  Contact Email
                </label>
                <input
                  type="text"
                  value={identity.email}
                  onChange={(e) => setIdentity({ ...identity, email: e.target.value })}
                  className="w-full px-3 py-2 rounded-md bg-current/[0.05] border border-current/15 font-mono text-xs focus:outline-none focus:border-emerald-400"
                />
              </div>
            </div>

            <div className="grid grid-cols-1 sm:grid-cols-2 gap-3">
              <div>
                <label className="block font-mono text-[11px] uppercase opacity-60 mb-1">
                  GitHub Repository Path
                </label>
                <input
                  type="text"
                  value={identity.github}
                  onChange={(e) => setIdentity({ ...identity, github: e.target.value })}
                  className="w-full px-3 py-2 rounded-md bg-current/[0.05] border border-current/15 font-mono text-xs focus:outline-none focus:border-emerald-400"
                />
              </div>
              <div>
                <label className="block font-mono text-[11px] uppercase opacity-60 mb-1">
                  Availability Status
                </label>
                <input
                  type="text"
                  value={identity.status}
                  onChange={(e) => setIdentity({ ...identity, status: e.target.value })}
                  className="w-full px-3 py-2 rounded-md bg-current/[0.05] border border-current/15 text-xs focus:outline-none focus:border-emerald-400"
                />
              </div>
            </div>

            {/* Fresh Zero-Project Slate Toggle */}
            <div className="pt-3 border-t border-current/10">
              <div className="flex items-center justify-between p-3.5 rounded-lg border border-current/15 bg-current/[0.03]">
                <div>
                  <div className="font-medium text-sm">Pure Zero-Project Slate Mode</div>
                  <div className="text-xs opacity-60 mt-0.5">
                    Hide all sample sandboxes and display a clean " Slate 00 // Ready for First Deployment" state.
                  </div>
                </div>
                <button
                  type="button"
                  onClick={() => {
                    soundEngine.click();
                    setIdentity({
                      ...identity,
                      showFreshSlateMode: !identity.showFreshSlateMode
                    });
                  }}
                  className={`px-3 py-1.5 rounded font-mono text-xs font-medium transition-all ${
                    identity.showFreshSlateMode ? themeClasses.badge : themeClasses.pill
                  }`}
                >
                  {identity.showFreshSlateMode ? 'ENABLED' : 'DISABLED'}
                </button>
              </div>
            </div>
          </div>
        </div>

        {/* Export Config Snippet */}
        <div className="mt-8 pt-4 border-t border-current/10 flex items-center justify-between gap-3">
          <button
            onClick={() => copyToClipboard(configExportCode, 'config')}
            className={`flex-1 py-2.5 px-4 rounded-md border font-mono text-xs flex items-center justify-center gap-2 ${themeClasses.pill}`}
          >
            {copiedKey === 'config' ? (
              <>
                <Check className="w-4 h-4 text-emerald-400" />
                <span>COPIED CONFIG TO CLIPBOARD</span>
              </>
            ) : (
              <>
                <Copy className="w-4 h-4" />
                <span>COPY IDENTITY CONFIG JSON</span>
              </>
            )}
          </button>
          <button
            onClick={() => setIsCustomizerOpen(false)}
            className={`py-2.5 px-5 rounded-md font-mono text-xs font-medium ${themeClasses.badge}`}
          >
            DONE
          </button>
        </div>
      </div>
    </div>
  )}

  {/* =====================================================================
      MAIN ARCHITECTURAL HERO SECTION
  ===================================================================== */}
  <main className="relative z-10 max-w-7xl mx-auto px-5 sm:px-10 pt-14 sm:pt-24 pb-24">
    {/* Top Coordinate Metadata Strip */}
    <div className="flex flex-wrap items-center justify-between gap-4 pb-8 border-b border-current/10 font-mono text-xs opacity-60">
      <div className="flex items-center gap-2">
        <Globe className="w-3.5 h-3.5" />
        <span>{identity.location}</span>
      </div>
      <div className="flex items-center gap-6">
        <span>EDITION // 2026.04</span>
        <span>ARCHITECTURE: ZERO-BLOAT GPU</span>
      </div>
    </div>

    {/* Kinetic Display Typography */}
    <section className="py-12 sm:py-20">
      <div className="max-w-5xl">
        <div className="inline-flex items-center gap-2 px-3 py-1 rounded-full border border-current/15 bg-current/[0.03] font-mono text-xs mb-6">
          <Zap className="w-3.5 h-3.5 text-emerald-400" />
          <span>{identity.role}</span>
        </div>

        <h1 className="text-4xl sm:text-6xl lg:text-7xl font-medium tracking-tight leading-[1.06]">
          {identity.tagline.split(' ').map((word, idx) => {
            const isItalic =
              word.toLowerCase().includes('silent') ||
              word.toLowerCase().includes('tactile') ||
              word.toLowerCase().includes('mathematical') ||
              word.toLowerCase().includes('minimalism');
            return (
              <span
                key={idx}
                onMouseEnter={() => soundEngine.hover()}
                className={`inline-block mr-[0.26em] transition-transform duration-300 hover:-translate-y-1 ${
                  isItalic ? 'font-serif italic font-normal tracking-normal' : ''
                }`}
                style={isItalic ? { fontFamily: "'Instrument Serif', serif" } : undefined}
              >
                {word}
              </span>
            );
          })}
        </h1>

        {/* Quick Action & Interactive Stats Row */}
        <div className="mt-10 sm:mt-14 flex flex-wrap items-center justify-between gap-6 pt-8 border-t border-current/10">
          <div className="flex flex-wrap items-center gap-3">
            <a
              href={`mailto:${identity.email}`}
              onMouseEnter={() => soundEngine.hover()}
              onClick={() => soundEngine.click()}
              className={`px-5 py-3 rounded-md font-mono text-xs font-medium flex items-center gap-2 transition-transform active:scale-95 ${themeClasses.badge}`}
            >
              <span>INITIATE DIALOGUE</span>
              <ArrowUpRight className="w-4 h-4" />
            </a>

            <button
              onClick={() => copyToClipboard(identity.email, 'email')}
              onMouseEnter={() => soundEngine.hover()}
              className={`px-4 py-3 rounded-md border font-mono text-xs flex items-center gap-2 transition-all ${themeClasses.pill}`}
            >
              {copiedKey === 'email' ? (
                <>
                  <Check className="w-3.5 h-3.5 text-emerald-400" />
                  <span>COPIED: {identity.email}</span>
                </>
              ) : (
                <>
                  <Copy className="w-3.5 h-3.5 opacity-60" />
                  <span>{identity.email}</span>
                </>
              )}
            </button>

            <button
              onClick={() => {
                soundEngine.switchMode();
                setViewMode('repo');
              }}
              onMouseEnter={() => soundEngine.hover()}
              className={`px-4 py-3 rounded-md border font-mono text-xs flex items-center gap-2 transition-all ${themeClasses.pill}`}
            >
              <GitBranch className="w-3.5 h-3.5 opacity-60" />
              <span>INSPECT GITHUB REPO</span>
            </button>
          </div>

          {/* Interactive Keyboard Hint */}
          <div className="hidden md:flex items-center gap-2 font-mono text-[11px] opacity-50">
            <Command className="w-3.5 h-3.5" />
            <span>KEYS: [1/2/3] VIEW · [C] SHADER · [T] THEME · [P] EDIT NAME</span>
          </div>
        </div>
      </div>
    </section>

    {/* ===================================================================
        VIEW CONTROLLER BAR (INDEX [01] / BENTO [02] / REPO & README [03])
    =================================================================== */}
    <section className="mt-6">
      <div className="flex flex-wrap items-center justify-between gap-4 pb-5 border-b border-current/15">
        <div className="flex items-center gap-3">
          <span className="font-mono text-xs uppercase tracking-widest opacity-50">
            SYSTEM DIRECTORY //
          </span>
          <span className="font-mono text-xs px-2 py-0.5 rounded border border-current/15">
            {identity.showFreshSlateMode ? '00 MODULES (FRESH SLATE)' : `0${sandboxes.length} ACTIVE MODULES`}
          </span>
        </div>

        {/* Multi-Perspective View Switcher */}
        <div className="flex items-center gap-1.5 p-1 rounded-lg border border-current/10 bg-current/[0.02]">
          <button
            onClick={() => {
              soundEngine.switchMode();
              setViewMode('index');
            }}
            onMouseEnter={() => soundEngine.hover()}
            className={`px-3 py-1.5 rounded-md font-mono text-xs flex items-center gap-1.5 transition-all ${
              viewMode === 'index' ? themeClasses.badge : 'opacity-60 hover:opacity-100'
            }`}
          >
            <List className="w-3.5 h-3.5" />
            <span>INDEX [01]</span>
          </button>

          <button
            onClick={() => {
              soundEngine.switchMode();
              setViewMode('bento');
            }}
            onMouseEnter={() => soundEngine.hover()}
            className={`px-3 py-1.5 rounded-md font-mono text-xs flex items-center gap-1.5 transition-all ${
              viewMode === 'bento' ? themeClasses.badge : 'opacity-60 hover:opacity-100'
            }`}
          >
            <Grid className="w-3.5 h-3.5" />
            <span>BENTO [02]</span>
          </button>

          <button
            onClick={() => {
              soundEngine.switchMode();
              setViewMode('repo');
            }}
            onMouseEnter={() => soundEngine.hover()}
            className={`px-3 py-1.5 rounded-md font-mono text-xs flex items-center gap-1.5 transition-all ${
              viewMode === 'repo' ? themeClasses.badge : 'opacity-60 hover:opacity-100'
            }`}
          >
            <Code2 className="w-3.5 h-3.5" />
            <span>REPO & CODE [03]</span>
          </button>
        </div>
      </div>

      {/* =================================================================
          FRESH ZERO-PROJECT SLATE STATE (If user toggles 0 projects)
      ================================================================= */}
      {identity.showFreshSlateMode && viewMode !== 'repo' ? (
        <div className="my-12 p-10 sm:p-16 rounded-xl border border-dashed border-current/20 bg-current/[0.015] text-center max-w-3xl mx-auto">
          <div className="w-10 h-10 rounded-full border border-current/20 mx-auto flex items-center justify-center mb-4">
            <Sparkles className="w-4 h-4 opacity-70" />
          </div>
          <p className="font-mono text-xs uppercase tracking-widest opacity-50">
            SLATE 00 // ZERO LEGACY ARTIFACTS
          </p>
          <h3 className="text-2xl sm:text-3xl font-medium mt-2">
            Fresh Architectural Canvas Ready for Your Works
          </h3>
          <p className="text-sm opacity-65 mt-3 max-w-lg mx-auto leading-relaxed">
            All previous projects have been stripped out. You can publish this clean minimalist state immediately, or add your own custom modules right now.
          </p>
          <div className="mt-7 flex flex-wrap items-center justify-center gap-3">
            <button
              onClick={() => {
                soundEngine.click();
                setShowAddForm(true);
              }}
              className={`px-4 py-2.5 rounded-md font-mono text-xs font-medium flex items-center gap-2 ${themeClasses.badge}`}
            >
              <Plus className="w-4 h-4" />
              <span>ADD YOUR FIRST PROJECT</span>
            </button>
            <button
              onClick={() => {
                soundEngine.click();
                setIdentity({ ...identity, showFreshSlateMode: false });
              }}
              className={`px-4 py-2.5 rounded-md border font-mono text-xs flex items-center gap-2 ${themeClasses.pill}`}
            >
              <RefreshCw className="w-3.5 h-3.5" />
              <span>LOAD 3 INTERACTIVE SANDBOX TEMPLATES</span>
            </button>
          </div>
        </div>
      ) : null}

      {/* =================================================================
          VIEW 01: KINETIC INDEX LEDGER VIEW
      ================================================================= */}
      {!identity.showFreshSlateMode && viewMode === 'index' && (
        <div className="divide-y divide-current/10">
          {sandboxes.map((item) => {
            const isHovered = hoveredItem === item.id;
            return (
              <div
                key={item.id}
                onMouseEnter={() => {
                  soundEngine.hover();
                  setHoveredItem(item.id);
                }}
                onMouseLeave={() => setHoveredItem(null)}
                className="group py-7 sm:py-9 transition-all duration-300 flex flex-col lg:flex-row lg:items-center justify-between gap-6 cursor-pointer"
              >
                <div className="flex items-start sm:items-center gap-5 sm:gap-8">
                  <span className="font-mono text-xs opacity-40 group-hover:opacity-100 transition-opacity pt-1 sm:pt-0">
                    {item.id}
                  </span>
                  <div>
                    <div className="flex items-center gap-3">
                      <h3 className="text-xl sm:text-3xl font-medium tracking-tight group-hover:translate-x-2 transition-transform duration-300">
                        {item.title}
                      </h3>
                      <span className="hidden sm:inline-block font-mono text-[10px] px-2 py-0.5 rounded border border-current/15 opacity-60">
                        {item.status}
                      </span>
                    </div>
                    <p className="text-sm opacity-60 mt-1.5 max-w-2xl leading-relaxed">
                      {item.summary}
                    </p>
                  </div>
                </div>

                {/* Right Specs & Live Micro-Preview */}
                <div className="flex flex-wrap sm:flex-nowrap items-center justify-between lg:justify-end gap-6 pl-10 lg:pl-0">
                  <div className="flex flex-wrap gap-1.5">
                    {item.stack.map((tech) => (
                      <span
                        key={tech}
                        className="font-mono text-[11px] px-2.5 py-1 rounded bg-current/[0.04] border border-current/10 opacity-75"
                      >
                        {tech}
                      </span>
                    ))}
                  </div>

                  <div className="flex items-center gap-3">
                    <span className="font-mono text-xs opacity-45 hidden xl:inline">
                      {item.metrics}
                    </span>
                    <button
                      onClick={(e) => {
                        e.stopPropagation();
                        removeSandbox(item.id);
                      }}
                      title="Remove this module for a cleaner slate"
                      className="p-2 rounded-md opacity-30 hover:opacity-100 hover:text-rose-400 transition-opacity"
                    >
                      <Trash2 className="w-4 h-4" />
                    </button>
                    <div
                      className={`w-9 h-9 rounded-full border border-current/15 flex items-center justify-center transition-all duration-300 ${
                        isHovered ? themeClasses.badge : ''
                      }`}
                    >
                      <ArrowUpRight className="w-4 h-4" />
                    </div>
                  </div>
                </div>
              </div>
            );
          })}
        </div>
      )}

      {/* =================================================================
          VIEW 02: ARCHITECTURAL BENTO SANDBOX VIEW
      ================================================================= */}
      {!identity.showFreshSlateMode && viewMode === 'bento' && (
        <div className="grid grid-cols-1 md:grid-cols-3 gap-6 py-8">
          {sandboxes.map((item) => (
            <div
              key={item.id}
              onMouseEnter={() => soundEngine.hover()}
              className={`group rounded-xl border p-6 flex flex-col justify-between transition-all duration-300 hover:-translate-y-1 ${themeClasses.panel}`}
            >
              <div>
                <div className="flex items-center justify-between font-mono text-xs opacity-60 mb-4">
                  <span>{item.id} // {item.category}</span>
                  <span>{item.metrics}</span>
                </div>

                {/* Interactive Live Canvas Widget Inside Card */}
                <MiniSandboxPreview type={item.demoType} theme={theme} />

                <h3 className="text-xl font-medium tracking-tight mt-5">
                  {item.title}
                </h3>
                <p className="text-xs sm:text-sm opacity-65 mt-2 leading-relaxed">
                  {item.summary}
                </p>
              </div>

              <div className="mt-6 pt-4 border-t border-current/10 flex items-center justify-between gap-2">
                <div className="flex flex-wrap gap-1.5">
                  {item.stack.slice(0, 3).map((t) => (
                    <span
                      key={t}
                      className="font-mono text-[10px] px-2 py-0.5 rounded border border-current/10 opacity-70"
                    >
                      {t}
                    </span>
                  ))}
                </div>
                <button
                  onClick={() => removeSandbox(item.id)}
                  className="text-xs font-mono opacity-40 hover:opacity-100 hover:text-rose-400 transition-opacity"
                  title="Delete module"
                >
                  <Trash2 className="w-3.5 h-3.5" />
                </button>
              </div>
            </div>
          ))}
        </div>
      )}

      {/* Add Custom Module Bar (In Index or Bento View) */}
      {viewMode !== 'repo' && (
        <div className="mt-6 pt-4 flex items-center justify-between">
          {!showAddForm ? (
            <button
              onClick={() => {
                soundEngine.click();
                setShowAddForm(true);
              }}
              onMouseEnter={() => soundEngine.hover()}
              className={`px-4 py-2 rounded-md border font-mono text-xs flex items-center gap-2 transition-all ${themeClasses.pill}`}
            >
              <Plus className="w-3.5 h-3.5" />
              <span>ADD YOUR OWN PROJECT / MODULE</span>
            </button>
          ) : (
            <form
              onSubmit={handleAddSandbox}
              className={`w-full p-5 rounded-xl border ${themeClasses.panel} space-y-4`}
            >
              <div className="flex items-center justify-between">
                <span className="font-mono text-xs uppercase opacity-70">
                  NEW ARCHITECTURAL MODULE ENTRY
                </span>
                <button
                  type="button"
                  onClick={() => setShowAddForm(false)}
                  className="text-xs font-mono opacity-50 hover:opacity-100"
                >
                  [CANCEL]
                </button>
              </div>
              <div className="grid grid-cols-1 sm:grid-cols-3 gap-3">
                <input
                  type="text"
                  placeholder="Module or Project Title *"
                  required
                  value={newSandboxForm.title}
                  onChange={(e) => setNewSandboxForm({ ...newSandboxForm, title: e.target.value })}
                  className="px-3 py-2 rounded bg-current/[0.04] border border-current/15 text-sm focus:outline-none"
                />
                <input
                  type="text"
                  placeholder="Category (e.g. Full-Stack / AI)"
                  value={newSandboxForm.category}
                  onChange={(e) => setNewSandboxForm({ ...newSandboxForm, category: e.target.value })}
                  className="px-3 py-2 rounded bg-current/[0.04] border border-current/15 text-sm focus:outline-none"
                />
                <input
                  type="text"
                  placeholder="Tech Stack (comma separated)"
                  value={newSandboxForm.stack}
                  onChange={(e) => setNewSandboxForm({ ...newSandboxForm, stack: e.target.value })}
                  className="px-3 py-2 rounded bg-current/[0.04] border border-current/15 text-sm font-mono focus:outline-none"
                />
              </div>
              <div className="flex flex-col sm:flex-row gap-3">
                <input
                  type="text"
                  placeholder="Concise one-line technical summary..."
                  value={newSandboxForm.summary}
                  onChange={(e) => setNewSandboxForm({ ...newSandboxForm, summary: e.target.value })}
                  className="flex-1 px-3 py-2 rounded bg-current/[0.04] border border-current/15 text-sm focus:outline-none"
                />
                <button
                  type="submit"
                  className={`px-5 py-2 rounded font-mono text-xs font-medium ${themeClasses.badge}`}
                >
                  APPEND TO SLATE
                </button>
              </div>
            </form>
          )}

          {!identity.showFreshSlateMode && sandboxes.length > 0 && (
            <button
              onClick={() => {
                soundEngine.click();
                setIdentity({ ...identity, showFreshSlateMode: true });
              }}
              className="font-mono text-xs opacity-45 hover:opacity-100 underline"
            >
              Clear All to Pure Zero-Project Slate
            </button>
          )}
        </div>
      )}

      {/* =================================================================
          VIEW 03: GITHUB REPOSITORY SHOWCASE & ARCHITECTURE VIEW
      ================================================================= */}
      {viewMode === 'repo' && (
        <div className="py-8 space-y-8">
          {/* Repo Header Banner */}
          <div className={`p-6 sm:p-8 rounded-xl border ${themeClasses.panel}`}>
            <div className="flex flex-wrap items-center justify-between gap-4 pb-6 border-b border-current/10">
              <div className="flex items-center gap-3">
                <GitBranch className="w-5 h-5 text-emerald-400" />
                <div>
                  <div className="font-mono text-sm sm:text-base font-medium">
                    {identity.github}
                  </div>
                  <div className="font-mono text-xs opacity-55">
                    MIT License · Zero External Assets · 100/100 Lighthouse Architecture
                  </div>
                </div>
              </div>
              <button
                onClick={() => copyToClipboard(configExportCode, 'repoConfig')}
                className={`px-4 py-2 rounded-md font-mono text-xs font-medium flex items-center gap-2 ${themeClasses.badge}`}
              >
                {copiedKey === 'repoConfig' ? (
                  <>
                    <Check className="w-3.5 h-3.5" />
                    <span>COPIED CONFIG</span>
                  </>
                ) : (
                  <>
                    <Copy className="w-3.5 h-3.5" />
                    <span>EXPORT PERSONALIZED CONFIG.TS</span>
                  </>
                )}
              </button>
            </div>

            {/* Directory Tree Breakdown */}
            <div className="grid grid-cols-1 lg:grid-cols-2 gap-6 mt-6">
              <div>
                <h4 className="font-mono text-xs uppercase tracking-widest opacity-55 mb-3">
                  REPOSITORY MODULE STRUCTURE
                </h4>
                <div className="space-y-2.5">
                  {REPO_FILES.map((file) => (
                    <div
                      key={file.path}
                      className="p-3.5 rounded-lg border border-current/10 bg-current/[0.02]"
                    >
                      <div className="flex items-center justify-between font-mono text-xs">
                        <span className="font-medium text-emerald-400">{file.path}</span>
                        <span className="opacity-45">{file.lines} LOC</span>
                      </div>
                      <div className="text-xs font-medium mt-1">{file.role}</div>
                      <div className="text-xs opacity-60 mt-1 leading-relaxed">
                        {file.description}
                      </div>
                    </div>
                  ))}
                </div>
              </div>

              {/* Live Exported Identity Config Code Preview */}
              <div className="flex flex-col">
                <div className="flex items-center justify-between mb-3">
                  <h4 className="font-mono text-xs uppercase tracking-widest opacity-55">
                    LIVE GENERATED MANIFEST (src/config/identity.config.ts)
                  </h4>
                  <span className="font-mono text-[10px] opacity-50">AUTO-SYNCED</span>
                </div>
                <pre className="flex-1 p-4 rounded-lg border border-current/10 bg-black/40 text-emerald-300 font-mono text-xs overflow-x-auto leading-relaxed">
                  {configExportCode}
                </pre>
              </div>
            </div>
          </div>
        </div>
      )}
    </section>

    {/* ===================================================================
        ARCHITECTURAL PRINCIPLES & STACK MATRIX
    =================================================================== */}
    <section className="mt-20 pt-14 border-t border-current/10 grid grid-cols-1 md:grid-cols-3 gap-8">
      <div>
        <span className="font-mono text-xs uppercase tracking-widest opacity-50">
          01 // ZERO-DOM-REFLOW PHYSICS
        </span>
        <h4 className="text-lg font-medium mt-2">
          GPU & Canvas Offloading
        </h4>
        <p className="text-sm opacity-65 mt-2 leading-relaxed">
          Pointer trajectories and vector field deformations are computed inside a single requestAnimationFrame loop, keeping React state unblocked at 60–120 FPS.
        </p>
      </div>

      <div>
        <span className="font-mono text-xs uppercase tracking-widest opacity-50">
          02 // PROCEDURAL SENSORY FEEDBACK
        </span>
        <h4 className="text-lg font-medium mt-2">
          Web Audio Micro-Synthesizer
        </h4>
        <p className="text-sm opacity-65 mt-2 leading-relaxed">
          Instead of loading external audio files, harmonic oscillators synthesize 12ms tactile clicks in real time whenever you inspect modules or toggle views.
        </p>
      </div>

      <div>
        <span className="font-mono text-xs uppercase tracking-widest opacity-50">
          03 // SWISS KINETIC TYPOGRAPHY
        </span>
        <h4 className="text-lg font-medium mt-2">
          High-Contrast Editorial Rhythm
        </h4>
        <p className="text-sm opacity-65 mt-2 leading-relaxed">
          Pairs geometric grotesks with Instrument Serif italics and tabular monospace telemetry so data density never compromises visual calm.
        </p>
      </div>
    </section>
  </main>

  {/* =====================================================================
      MINIMALIST TELEMETRY FOOTER
  ===================================================================== */}
  <footer className="relative z-10 border-t border-current/10 mt-12">
    <div className="max-w-7xl mx-auto px-5 sm:px-10 py-8 flex flex-col sm:flex-row items-center justify-between gap-4 font-mono text-xs opacity-60">
      <div>
        © {new Date().getFullYear()} {identity.name} — ALL RIGHTS RESERVED.
      </div>
      <div className="flex items-center gap-6">
        <button
          onClick={() => setIsCustomizerOpen(true)}
          className="hover:underline"
        >
          EDIT IDENTITY
        </button>
        <button
          onClick={() => setViewMode('repo')}
          className="hover:underline"
        >
          REPOSITORY ARCHITECTURE
        </button>
        <span>LATENCY: 0.3ms</span>
      </div>
    </div>
  </footer>
</div>

```

);
}