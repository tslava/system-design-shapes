# System Design Shapes - Developer Documentation

This file contains development context, architecture decisions, and technical notes for AI assistants and developers working on this project.

## Project Overview

**System Design Shapes** is an interactive educational tool that visualizes 20 common system design interview systems on a radar chart across 9 architectural dimensions. Built as a single standalone HTML file with no build dependencies.

## Architecture Decisions

### Single-File Architecture
**Decision**: Build the entire application as a single `index.html` file with inline JavaScript and CSS.

**Rationale**:
- Zero build step - works anywhere, instantly
- Easy to share and deploy (just one file)
- Perfect for educational/demo purposes
- GitHub Pages deployment is trivial
- Users can easily view source and learn from the code

**Trade-offs**:
- Large file size (~135KB, but acceptable)
- No module system or code splitting
- All dependencies loaded from CDN
- Limited scalability for massive feature additions

### Technology Stack

**React 18.3.1** (via unpkg CDN)
- Chosen for: Component-based UI, state management, familiar to developers
- **Why not React 19?**: React 19.2.4 doesn't exist yet on CDN (future-dated in original plan)
- Production build for optimal performance

**D3.js 7.9.0** (via unpkg CDN)
- Chosen for: Radar chart calculations (polar to cartesian coordinates)
- Only used for math utilities, not DOM manipulation
- Alternatives considered: Chart.js (too opinionated), raw trigonometry (D3 is cleaner)

**Babel Standalone 7.24.4** (via unpkg CDN)
- Chosen for: JSX transformation at runtime
- **Trade-off**: Runtime compilation cost vs. no build step
- Mitigated by: Using production React, minimizing re-renders

**No State Persistence**
- Theme preference resets on page reload
- System selection resets on page reload
- **Rationale**: Simplicity, no localStorage complications in embedded contexts

### Component Structure

```
App
├── Header (title, subtitle, theme toggle)
├── SystemSelector (multi-select pills with always-visible Clear All button)
├── Subtitle (instructional text, always visible)
├── RadarChart (D3-based radar visualization)
├── PatternsPanel (collapsible cards with architecture patterns)
└── Footer (attribution, GitHub link)
```

### Key Technical Patterns

#### 1. Theme System
- Detects OS color scheme preference on load: `window.matchMedia('(prefers-color-scheme: dark)')`
- Listens for system theme changes and auto-updates
- Manual toggle override available
- All colors defined in `THEMES` constant (no hardcoded colors)
- Smooth CSS transitions on theme changes

#### 2. Responsive Design
- Breakpoints: sm=480px, md=768px, lg=1024px
- Mobile-first approach
- `useWindowSize` hook for responsive calculations
- Horizontal scroll on mobile for system pills
- Radar chart size adjusts: 380px (mobile) to 700px (desktop)

#### 3. Radar Chart Mathematics
- 9-sided polygon (9 dimensions)
- D3 linear scale: `[0, 5]` → `[0, radius]`
- Angle calculation: `(2π / 9) * index - π/2` (start at top)
- Polar to cartesian: `x = center + r*cos(θ)`, `y = center + r*sin(θ)`
- Label positioning: 5.5 units from center (vs 5.0 max score)
- SVG `overflow: visible` to prevent label clipping

#### 4. Stable Button Layout
- "Clear All" button always rendered (not conditional)
- Disabled state when no selections: `opacity: 0.3`, `cursor: not-allowed`
- Prevents layout shift when selecting/deselecting systems
- Smooth opacity transition for visual feedback

## File Structure

```
/
├── index.html                      # Complete standalone application (2,302 lines)
├── .github/
│   └── workflows/
│       └── deploy.yml             # GitHub Actions auto-deploy to Pages
├── README.md                       # User-facing documentation
├── CLAUDE.md                       # This file - developer context
└── IMPLEMENTATION_PLAN.md         # Original implementation plan
```

## Data Structure

### SYSTEMS Array
20 systems, each with:
- `id`: Unique kebab-case identifier
- `name`: Display name
- `icon`: Emoji for visual identification
- `color`: Unique hex color (selected for visibility at low opacity)
- `examples`: Real-world examples (e.g., "Stripe, PayPal")
- `scores`: Object mapping dimension keys to values 1-5
- `patterns`: Array of 9 pattern objects (one per dimension)

### Pattern Object
Each pattern contains:
- `dimension`: Key matching one of the 9 dimensions
- `text`: 1-2 sentence architecture pattern description
- `tools`: Array of 1-3 recommended tools with:
  - `name`: Tool name
  - `url`: Official website/documentation
  - `rationale`: Why this tool fits this dimension/system

**Total**: 20 systems × 9 dimensions = 180 patterns, ~360 tool recommendations

### Color Palette Strategy
- 20 distinct colors chosen for visibility
- Colors work at low opacity (0.15-0.2) for overlapping shapes
- Tested in both light and dark themes
- Higher opacity in dark mode (0.2 vs 0.15) for better contrast

## Configuration Changes

### Default State
- **Pre-selected system**: URL Shortener (`'url-shortener'`)
  - Provides immediate visual example for new users
  - Shows tool functionality without requiring interaction
- **Theme**: Auto-detected from OS (`prefers-color-scheme`)

## Known Issues & Limitations

1. **Large File Size**: ~135KB (acceptable for single-page app, but monitor if adding more systems)
2. **No Data Persistence**: Selections and theme preference don't persist across sessions
3. **CDN Dependency**: Requires internet connection for initial load (React, D3, Babel)
4. **Runtime JSX Compilation**: Slight performance cost on initial render
5. **No Unit Tests**: Educational project, testing is manual/visual
6. **Fixed 9 Dimensions**: Adding/removing dimensions requires code changes to layout math

## Testing Checklist

When making changes, verify:

### Visual Tests
- [ ] All 20 systems render with correct colors
- [ ] Radar chart displays correctly with 1, 2, 5, and 10+ systems selected
- [ ] Labels visible at all breakpoints (mobile 375px, tablet 768px, desktop 1200px+)
- [ ] No text overflow or clipping on dimension labels
- [ ] Pattern cards expand/collapse smoothly
- [ ] Tool links open in new tabs

### Theme Tests
- [ ] Light mode: readable text, good contrast
- [ ] Dark mode: readable text, good contrast
- [ ] Theme toggle works bidirectionally
- [ ] System theme auto-detection works (test by changing OS theme)
- [ ] All colors transition smoothly (no jarring changes)

### Responsive Tests
- [ ] Mobile (375px): horizontal scroll, collapsed cards, no overflow
- [ ] Tablet (768px): appropriate layout, readable text
- [ ] Desktop (1200px): full multi-column layout, hover effects work
- [ ] Window resize: smooth adaptation, no layout breaks

### Interaction Tests
- [ ] Clicking system pills toggles selection
- [ ] Multiple systems can be selected simultaneously
- [ ] "Clear All" button works and is always visible
- [ ] "Clear All" is disabled when nothing selected
- [ ] Hover effects work on desktop (pills, radar shapes, tool chips)
- [ ] Touch feedback works on mobile (no sticky hover states)

### Browser Compatibility
- [ ] Chrome/Edge (Chromium)
- [ ] Firefox
- [ ] Safari (WebKit)
- [ ] Mobile Safari (iOS)
- [ ] Mobile Chrome (Android)

## Development Workflow

### Making Changes

1. **Edit `index.html`** directly (no build step)
2. **Test locally**:
   ```bash
   python3 -m http.server 8000
   # Open http://localhost:8000/index.html
   ```
3. **Hard refresh** after changes: Cmd+Shift+R (Mac) or Ctrl+Shift+R (Windows/Linux)
4. **Commit and push** to trigger GitHub Actions deployment

### Adding a New System

1. Add to `SYSTEMS` array (around line 200)
2. Choose unique color (test at 0.15 opacity in light mode, 0.2 in dark mode)
3. Provide scores for all 9 dimensions (1-5 scale)
4. Write 9 pattern objects (one per dimension) with tools
5. Test: Select new system, verify radar shape, check pattern display

### Modifying Dimensions

**Warning**: Changing dimension count requires updates to:
- `DIMENSIONS` array definition
- Radar chart math (`angleStep` calculation)
- All 20 systems' `patterns` arrays (must match dimension count)
- SVG sizing/padding may need adjustment for more/fewer axes

### Styling Changes

- All theme colors in `THEMES` constant (line ~60)
- Breakpoints in `BREAKPOINTS` constant (line ~80)
- Responsive logic in component code (check `isMobile` conditionals)
- CSS-in-JS style objects (inline in components)

## Performance Considerations

### Current Performance
- **Initial load**: <2s on 3G (mostly CDN downloads)
- **Interactions**: 60fps on modern devices
- **Memory**: ~50MB for React + D3 + app state (acceptable)

### Optimization Opportunities (if needed)
1. **Lazy load patterns**: Only render expanded card content
2. **Virtualize system list**: If expanding beyond 30 systems
3. **Memoize radar calculations**: `useMemo` on chart points
4. **Debounce window resize**: Currently updates on every resize event
5. **Code splitting**: Move to build system if file grows beyond 250KB

## Dependencies

All loaded from CDN (no `package.json`):

- **react@18.3.1**: UI framework
- **react-dom@18.3.1**: React DOM renderer
- **@babel/standalone@7.24.4**: JSX transformation
- **d3@7.9.0**: Math utilities for radar chart

**Why these versions?**
- React 18.3.1: Latest stable (19.x not yet available on unpkg)
- Babel 7.24.4: Latest stable for runtime JSX
- D3 7.9.0: Latest stable, only using scale/math utilities

## Future Enhancement Ideas

### High Priority
- [ ] Add search/filter for systems
- [ ] Export chart as PNG/SVG
- [ ] Permalink with selected systems in URL hash
- [ ] Keyboard navigation (arrow keys, Enter to select)

### Medium Priority
- [ ] Compare mode: side-by-side radar charts
- [ ] Custom systems: user-defined scores
- [ ] Dimension tooltips with detailed explanations
- [ ] Print-friendly stylesheet

### Low Priority
- [ ] Animation on chart transitions
- [ ] 3D radar chart option
- [ ] Historical data (how systems evolved over time)
- [ ] Quiz mode: guess the system from the shape

## Contributing Guidelines

When adding content:
- **Patterns**: Keep descriptions concise (1-2 sentences)
- **Tools**: Pick 1-3 most relevant tools per dimension
- **Rationales**: Explain *why*, not *what* (users can click links to learn what)
- **Examples**: Use well-known companies/products
- **Scores**: Justify your scoring (document in PR)

## Deployment

### GitHub Pages Setup
1. Go to repository Settings → Pages
2. Source: GitHub Actions
3. Push to `main` branch
4. Workflow in `.github/workflows/deploy.yml` handles deployment
5. Site available at: `https://[username].github.io/system-design-shapes/`

### Manual Deployment
Simply upload `index.html` to any static host:
- Netlify: Drag and drop
- Vercel: Deploy via CLI or GitHub integration
- AWS S3: Upload and enable static website hosting
- Any web server: Just serve the HTML file

## License & Attribution

- **Original Framework**: "9 Questions for System Design Interviews" by @rauljuncoV
- **Implementation**: Built by @tslava (https://github.com/tslava)
- **License**: MIT (see repository for full license)

## Questions?

For development questions or contributions, open an issue on GitHub: [https://github.com/tslava/system-design-shapes](https://github.com/tslava/system-design-shapes)

---

**Last Updated**: February 2026
**File Version**: 1.0
**Lines of Code**: ~2,300 (index.html)
