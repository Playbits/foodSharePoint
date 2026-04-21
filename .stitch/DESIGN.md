# FoodSharePoint Design System

## 1. Brand Identity
- **Keywords**: Farm-to-Table, Community, Organic, Vibrant, Earthy.
- **Color Palette**: 
  - Primaries: Forest Green (Growth), Rich Terracotta/Orange (Sun-ripened), Wood Brown.
  - Secondary: Cream/Off-white background (Warmth).
- **Typography**: Friendly yet professional Sans-serif (Work Sans/Inter), large and bold headers.

## 2. Components (shadcn/ui based)
- **Cards**: Soft shadows, generous padding, clean borders.
- **Buttons**: Rounded-full or xl, strong distinctive colors (Dark Blue/Emerald Green).
- **Images**: High-quality, vibrant photography of markets, fresh produce, and cooking.

## 6. Design System Notes for Stitch Generation
[STITCH_PROMPT_START]
**STITCH DESIGN SYSTEM:**
- **AESTHETIC**: Vibrant "Farm to Table" Marketplace (NOT Fintech).
- **Color Mode**: Light mode with a warm, organic feel.
- **Background**: Soft cream or off-white.
- **Primary Color**: Forest Green (`#14532d`) or Emerald (`#10b981`).
- **Accent Color**: Earthy Orange/Terracotta (`#c2410c`).
- **Currency**: Nigerian Naira (₦).
- **Typography**: Bold, confident headlines with high contrast.
- **Imagery**: Use lush, vibrant Nigerian market scenery. Use local assets:
  - Header/Logo: `/images/logo.png`
  - Hero/Banners: `/images/banner.png`
  - Auth Pages: `/images/sign-in.png`
- **Layout**: Spacious 12-column grid, responsive containers.
- **DESKTOP HEADER CONSISTENCY**: All desktop pages must use a consistent top navigation bar with:
  - Left: `/images/logo.png` (link to home).
  - Center: Nav links (Browse, Pricing, My Bookings, Dashboard).
  - Right: User Profile/Cart icons.
- **MOBILE FOOTER CONSISTENCY**: All mobile pages must use a consistent bottom navigation bar (Tab Bar) with:
  - Icons for: Home, Search, Bookings, Dashboard.
  - Active states using Forest Green (#14532d).
[STITCH_PROMPT_END]
