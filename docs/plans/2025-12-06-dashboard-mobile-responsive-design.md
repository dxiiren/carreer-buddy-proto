# Dashboard Mobile Responsive Design

## Overview

Full mobile-responsive overhaul of the dashboard to provide optimal UX across phone, tablet, and desktop devices.

## Breakpoints

| Device | Width | Tailwind Prefix |
|--------|-------|-----------------|
| Phone | < 640px | (default) |
| Tablet | 640px - 1024px | `sm:` and `md:` |
| Desktop | > 1024px | `lg:` |

---

## 1. Sidebar & Navigation

### Desktop (> 1024px)
- Fixed sidebar, collapsible via toggle button (current behavior)
- Width: `w-64` expanded, `w-16` collapsed

### Tablet & Phone (< 1024px)
- Sidebar hidden by default (`hidden lg:flex`)
- Hamburger button in header opens slide-out drawer
- Drawer slides in from left as overlay
- Dark backdrop behind drawer (`bg-black/50`)
- Close via X button or tapping backdrop
- Main content has no left margin on mobile

### Implementation
- Add `isMobileMenuOpen` ref state
- Add `hidden lg:flex` to sidebar
- Create mobile drawer component with transitions
- Remove `ml-64` / `ml-16` on mobile (`ml-0 lg:ml-64`)

---

## 2. Top Header

### Desktop (> 1024px)
- Current layout preserved
- Toggle + Search bar + Theme + Bell + Avatar with name/email

### Tablet (640px - 1024px)
- `[Hamburger] [Logo] ---spacer--- [Search icon] [Theme] [Bell] [Avatar]`
- Search icon expands to search bar overlay when tapped
- Hide user name/email text, show avatar only
- Padding: `px-5`

### Phone (< 640px)
- Same as tablet but tighter padding: `px-4`
- Search expands as larger overlay for easier mobile typing

### Implementation
- Add `isSearchOpen` ref state
- Hamburger: `lg:hidden` (only show on mobile/tablet)
- Sidebar toggle: `hidden lg:flex` (only show on desktop)
- Search bar: `hidden lg:flex` on desktop, icon-only on mobile
- User info text: `hidden lg:block`
- Logo in header: `lg:hidden` (only on mobile since sidebar has logo)

---

## 3. Dashboard Grid Layout

### Phone (< 640px)
```
[Welcome Header     ] - full width
[Progress Modules   ] - full width
[Quick Actions      ] - full width
[Daily Tasks        ] - full width
[Recent Activity    ] - full width
[Motivational       ] - full width
[Resume Preview     ] - full width
```
- Single column: `grid-cols-1`
- Gap: `gap-4`
- Main padding: `p-4`

### Tablet (640px - 1024px)
```
[Welcome Header - full width        ]
[Progress Modules - full width      ]
[Quick Actions    ] [Daily Tasks    ]
[Recent Activity  ] [Motivational   ]
[Resume Preview - full width        ]
```
- Two-column grid: `sm:grid-cols-2`
- Gap: `gap-5`
- Main padding: `p-5`

### Desktop (> 1024px)
- Current bento 12-column layout preserved
- `lg:grid-cols-12` with col-span classes
- Gap: `gap-6`
- Main padding: `p-6`

### Implementation
Change `pages/dashboard.vue` grid:
```vue
<div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-12 gap-4 sm:gap-5 lg:gap-6">
```

Update col-spans:
- Welcome Header: `sm:col-span-2 lg:col-span-12`
- Progress Modules: `sm:col-span-2 lg:col-span-7`
- Quick Actions: `lg:col-span-5` (uses sm:grid-cols-2 cell on tablet)
- Daily Tasks: stays in right column flow
- Recent Activity: `lg:col-span-6`
- Motivational: `lg:col-span-6`
- Resume Preview: `sm:col-span-2 lg:col-span-12`

---

## 4. Card Internals

### WelcomeHeader.vue
- Phone: Stack greeting text + progress bar vertically
- Tablet/Desktop: Side by side layout
- Responsive padding: `p-4 sm:p-5 lg:p-6`
- Responsive text: `text-xl sm:text-2xl lg:text-3xl` for heading

### ProgressModules.vue
- Phone: 1-column module list (`grid-cols-1`)
- Tablet: 2-column module grid (`sm:grid-cols-2`)
- Desktop: 2-column (current)
- Responsive padding: `p-4 sm:p-5 lg:p-6`

### QuickActions.vue
- Phone: 2x2 grid with smaller icons
- Tablet/Desktop: current layout
- Responsive icon size: `h-5 w-5 sm:h-6 sm:w-6`
- Responsive padding: `p-4 sm:p-5 lg:p-6`

### DailyTasks.vue
- Responsive padding: `p-4 sm:p-5 lg:p-6`
- Responsive text: `text-sm sm:text-base`
- Checkbox items stack nicely (already works)

### RecentActivity.vue
- Responsive padding: `p-4 sm:p-5 lg:p-6`
- Responsive text sizes
- Activity items flow naturally

### MotivationalSection.vue
- Responsive padding: `p-4 sm:p-5 lg:p-6`
- Responsive quote text size

### ResumePreview.vue
- Phone: Simplify - show core info only, hide secondary details
- Use `hidden sm:block` for less critical info
- Responsive padding and text sizes

---

## 5. Files to Modify

1. `layouts/dashboard.vue` - Sidebar drawer, header responsive
2. `pages/dashboard.vue` - Grid layout breakpoints
3. `components/dashboard/WelcomeHeader.vue` - Responsive internals
4. `components/dashboard/ProgressModules.vue` - Module grid responsive
5. `components/dashboard/QuickActions.vue` - Icon/button sizing
6. `components/dashboard/DailyTasks.vue` - Padding/text responsive
7. `components/dashboard/RecentActivity.vue` - Padding/text responsive
8. `components/dashboard/MotivationalSection.vue` - Padding/text responsive
9. `components/dashboard/ResumePreview.vue` - Simplified mobile view

---

## 6. Testing Checklist

- [ ] Phone (390px width): All cards stack, drawer works, header compact
- [ ] Tablet (768px width): Two-column grid, drawer works, search icon
- [ ] Desktop (1440px width): Current bento layout preserved, sidebar fixed
- [ ] Drawer opens/closes smoothly with backdrop
- [ ] Search expands/collapses properly on mobile
- [ ] No horizontal scroll on any device
- [ ] Touch targets are at least 44px on mobile
