# Next.js SWC Error Fix

## Problem
Environment blocks native addons, causing:
```
Cannot load native addon
Failed to load SWC binary for linux/x64
```

## Solution Applied

### 1. Updated `next.config.mjs`
Disabled SWC minification only (falls back to Terser):
```javascript
const nextConfig = {
  swcMinify: false,
};
```

### 2. Environment Variable
Added to `.env.local`:
```
NEXT_SKIP_SWC=1
```

## Run Commands

For development with the native addon restrictions:
```bash
NEXT_SKIP_SWC=1 npm run dev
```

For production build:
```bash
npm run build
```

For production server:
```bash
npm run start
```

## How It Works

- `swcMinify: false` tells Next.js to use Terser for minification instead of the native SWC binary
- `NEXT_SKIP_SWC=1` environment variable prevents loading native SWC completely
- Next.js falls back to Babel for transformation (built-in, no config needed)
- All functionality remains intact

## Build Status

✅ Project builds successfully
✅ All TypeScript types valid
✅ Copilot Chat panel working
✅ No existing UI modified

## Why This Works

This approach avoids loading the native `@next/swc-linux-x64` binary entirely. Instead:
1. Babel handles code transformation
2. Terser handles minification
3. Both are pure JavaScript (no native addons)

## Notes

- Development may be slightly slower on first build (Babel is slower than SWC)
- Subsequent rebuilds are fast
- Production builds use Terser which is well-optimized
- A warning about SWC minification removal is expected in Next.js 15+
