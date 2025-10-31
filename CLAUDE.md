# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a cryptocurrency ranking dashboard built with Next.js 15.2.4 and React 19. It displays information about the top cryptocurrencies including price, market cap, volume, and 24h change. Currently uses mock data; real-time API integration is planned.

## Coding Standards

**Variable naming**: Use camelCase for all variables (e.g., `coinData`, `marketCap`, `change24h`)

## Development Commands

```bash
# Install dependencies (must use pnpm)
pnpm install

# Start development server (http://localhost:3000)
pnpm dev

# Build for production
pnpm build

# Start production server
pnpm start

# Linting (currently disabled during builds)
pnpm lint

# Testing (Jest configured, no tests written yet)
pnpm test                    # Run all tests
pnpm test -- --watch         # Watch mode
pnpm test -- path/to/file    # Single file
pnpm test -- --coverage      # With coverage
```

## Architecture

### Component Structure
```
crypto-ranking-board.tsx    # Main dashboard component (root level - should move to components/)
├── app/
│   ├── layout.tsx          # Root layout with Korean metadata
│   ├── page.tsx            # Entry point (renders crypto-ranking-board)
│   └── globals.css         # Global styles + CSS variables for theming
├── components/ui/          # 49 shadcn/ui components (Card, Badge, etc.)
├── lib/utils.ts            # cn() utility for merging Tailwind classes
└── public/coin/            # Cryptocurrency logos (8 PNG/WEBP files)
```

### Data Flow
The app currently uses **mock data only** (lines 16-97 in `crypto-ranking-board.tsx`). Data structure:
```typescript
interface CoinData {
  rank: number
  name: string      // Korean names (비트코인, 이더리움, etc.)
  symbol: string    // BTC, ETH, etc.
  price: number
  change24h: number // Percentage change
  marketCap: number
  volume: number
  logo: string      // Path to /public/coin/
}
```

**Important**: Footer claims "updates every 60 seconds" but no refresh mechanism exists. When implementing real-time data:
- Consider SWR or React Query for data fetching
- Implement error boundaries for API failures
- Add loading states during fetch/refresh

### UI System (shadcn/ui)

This project uses shadcn/ui, which provides **copy-paste components** (not npm packages). Key patterns:

1. **Component imports**: Always from `@/components/ui/[name]`
2. **Styling utility**: Use `cn()` from `@/lib/utils` to merge Tailwind classes
3. **Adding new components**: Use `npx shadcn@latest add [component-name]` (configured in `components.json`)
4. **Theme customization**: Modify CSS variables in `app/globals.css` (lines 16-84)

**Available components** (49 total): Card, Badge, Button, Dialog, Dropdown, Form, Input, Select, Table, Toast, etc.

### Responsive Design
Mobile-first with breakpoints:
- `hidden md:table-cell` - Market cap column (hidden on mobile)
- `hidden lg:table-cell` - Volume column (hidden on mobile/tablet)

### Path Aliases (tsconfig.json)
```typescript
"@/*" → project root
// Examples:
import { cn } from "@/lib/utils"
import { Card } from "@/components/ui/card"
import Component from "@/crypto-ranking-board"
```

## Technical Configuration

### Build Settings (next.config.mjs)
**Critical**: The following are currently **disabled** for rapid development but should be enabled before production:
```javascript
eslint: { ignoreDuringBuilds: true }      // Skips ESLint checks
typescript: { ignoreBuildErrors: true }   // Skips type checking
images: { unoptimized: true }             // Disables image optimization
```

### Testing Setup
Jest 30 + Testing Library configured but **no tests exist**. Config uses:
- `ts-jest` preset for TypeScript
- `jsdom` environment for React components
- Path mapping from tsconfig.json
- Test pattern: `**/*.{spec,test}.{js,jsx,ts,tsx}` or `**/__tests__/**`

### Language & Localization
- **UI language**: Korean (metadata, coin names, table headers)
- **Number formatting**: Uses `toLocaleString()` for prices
- **Currency**: USD ($) for all prices

## Common Development Tasks

### Adding a New Cryptocurrency
1. Add logo to `public/coin/[coinname].png` (or .webp)
2. Add entry to `mockCoinData` array in `crypto-ranking-board.tsx` (lines 16-97)
3. Ensure Korean name translation for `name` field

### Working with UI Components
```bash
# Add a new shadcn component
npx shadcn@latest add [component-name]

# Example: Add tooltip
npx shadcn@latest add tooltip
# Then import: import { Tooltip } from "@/components/ui/tooltip"
```

### Modifying Styles
- **Theme colors**: Edit CSS variables in `app/globals.css` (lines 16-84)
- **Component styles**: All components use Tailwind utility classes (no CSS modules)
- **Format utilities**: `formatNumber()` and `formatPrice()` in `crypto-ranking-board.tsx` (lines 99-120)

### TossPayments MCP Integration
Project includes `.mcp.json` with TossPayments integration guide MCP server. This may be for future payment features.

### GitHub CLI for PR Workflows
PR-related slash commands (`/review`, `/pr-comments`) require GitHub CLI:
```bash
brew install gh    # Install
gh auth login      # Authenticate (one-time setup)
```

## Current State & Limitations

1. **Mock Data Only**: No API integration despite UI suggesting real-time updates
2. **Build Validations Disabled**: Production builds skip TypeScript/ESLint checks
3. **No Tests**: Testing framework ready but 0% coverage
4. **Component Organization**: Main component in root instead of `components/` directory
5. **No Error Handling**: Missing error boundaries, loading states, fallbacks