---
name: foodshare-frontend
description: Frontend development skill for FoodSharePoint - React 19/TypeScript with Vite, shadcn/ui, TanStack Query, and Clerk Auth
allowed-tools:
  - "Bash"
  - "Read"
  - "Write"
  - "glob"
  - "grep"
  - "edit"
---

# Frontend Development Skill for FoodSharePoint

## Overview

FoodSharePoint is a food marketplace frontend built with **React 19**, **TypeScript**, and **Vite**. It uses **TanStack Router** for routing, **TanStack Query** for server state, **shadcn/ui** + **Tailwind CSS 4** for UI, and **Clerk** for authentication.

## Technology Stack

| Component | Technology | Version |
|-----------|------------|---------|
| Framework | React | 19.2.4 |
| Language | TypeScript | 5.9.x |
| Bundler | Vite | 7.3.1 |
| Routing | TanStack Router | 1.161.x |
| Data Fetching | TanStack Query | 5.90.x |
| UI Components | shadcn/ui + Radix UI | - |
| Styling | Tailwind CSS | 4.2.x |
| Forms | React Hook Form + Zod | 7.71.x / 4.3.x |
| Auth | Clerk | 5.60.x |
| State | Zustand | 5.0.x |
| Package Manager | pnpm | 10.30.x |
| Icons | Lucide React | - |
| Maps | Mapbox GL | - |

## Project Structure

```
frontend/
├── src/
│   ├── main.tsx                    # App entry point
│   ├── router.tsx                 # TanStack Router + Query Client setup
│   ├── routeTree.gen.ts           # Auto-generated route tree
│   ├── appRouter.tsx              # Router component
│   ├── components/
│   │   ├── ui/                     # shadcn/ui components
│   │   │   ├── button.tsx
│   │   │   ├── card.tsx
│   │   │   ├── dialog.tsx
│   │   │   ├── input.tsx
│   │   │   └── ... (Radix-based)
│   │   ├── layout/                 # Layout components
│   │   │   ├── header.tsx
│   │   │   ├── sidebar.tsx
│   │   │   └── footer.tsx
│   │   └── *.tsx                   # Shared components
│   ├── features/                   # Feature-based modules
│   │   ├── auth/                   # Sign-in, sign-up, OTP, forgot-password
│   │   │   ├── sign-in/
│   │   │   ├── sign-up/
│   │   │   ├── otp/
│   │   │   └── forgot-password/
│   │   ├── dashboard/             # Authenticated dashboard
│   │   │   ├── index/             # Dashboard home
│   │   │   ├── bookings/          # Booking management
│   │   │   ├── share-points/      # SharePoint management
│   │   │   ├── subscribers/       # Subscription management
│   │   │   ├── reviews/           # Review management
│   │   │   ├── settings/          # User settings
│   │   │   └── ...
│   │   ├── vendor/                # Vendor-facing features
│   │   ├── sharepoints/           # Public sharepoint browsing
│   │   ├── bookings/              # Booking features
│   │   ├── livestream/            # Live streaming UI
│   │   ├── subscription/          # Subscription features
│   │   ├── profile/               # User profiles
│   │   ├── chats/                 # Messaging
│   │   ├── settings/              # Settings pages
│   │   └── users/                 # User management
│   ├── hooks/                     # Custom hooks
│   │   ├── api.tsx                # Axios instance with Clerk JWT
│   │   ├── vendor.tsx             # Vendor API hooks
│   │   ├── booking.tsx            # Booking API hooks
│   │   ├── sharepoint.tsx         # SharePoint API hooks
│   │   ├── livestream-interaction.tsx
│   │   ├── subscription.tsx
│   │   ├── address-book.tsx
│   │   ├── profile.tsx
│   │   ├── category.tsx
│   │   ├── me.tsx                 # Current user hooks
│   │   └── ...
│   ├── stores/                     # Zustand stores
│   │   ├── use-cart-store.ts      # Shopping cart
│   │   └── __tests__/             # Store tests
│   ├── context/                   # React Context providers
│   │   ├── clerk-provider.tsx    # Clerk provider wrapper
│   │   ├── theme-provider.tsx   # Dark/light theme
│   │   ├── direction-provider.tsx # RTL support
│   │   ├── font-provider.tsx     # Font customization
│   │   └── livestream-context.tsx # Livestream state
│   ├── lib/                        # Utilities
│   │   ├── utils.ts               # cn() utility for Tailwind
│   │   ├── handle-server-error.ts # Error handling
│   │   ├── auth.ts                # Auth helpers
│   │   └── cookies.ts             # Cookie helpers
│   ├── types/                      # TypeScript definitions
│   ├── routes/                     # Route components (TanStack Router)
│   │   ├── __root.tsx             # Root route
│   │   ├── (auth)/                # Auth pages (sign-in, sign-up)
│   │   ├── (common)/             # Public pages (home, vendor, sharepoints)
│   │   ├── _authenticated/        # Protected routes layout
│   │   ├── clerk/                 # Clerk pages
│   │   └── errors/               # Error pages (401, 403, 404, 500)
│   └── styles/                    # CSS files
│       ├── index.css              # Tailwind imports
│       └── common.css             # Global styles
├── public/                         # Static assets
├── components.json                # shadcn/ui config
├── vite.config.ts                 # Vite configuration
├── tsconfig.json                  # TypeScript config
└── package.json
```

### Key Directories

| Directory | Purpose |
|-----------|---------|
| `src/features/` | Feature-based UI components organized by domain |
| `src/hooks/` | TanStack Query hooks wrapping API calls |
| `src/components/ui/` | shadcn/ui component library |
| `src/stores/` | Zustand global state stores |
| `src/context/` | React Context providers |
| `src/routes/` | TanStack Router route components |
| `src/lib/` | Utilities (cn(), API client) |

## API Communication

### API Client (`src/hooks/api.tsx`)

The frontend uses **axios** with Clerk JWT authentication:

```tsx
import axios from 'axios'
import { useAuth } from '@clerk/clerk-react'

const useApi = (type?: string) => {
  const { getToken } = useAuth()

  const api = axios.create({
    baseURL: import.meta.env.VITE_API_BASE_URL, // http://localhost:8080/api/v2/
  })

  // Set content type based on request type
  switch (type) {
    case 'multipart':
      api.defaults.headers.common['Content-Type'] = 'multipart/form-data'
      break
    default:
      api.defaults.headers.common['Content-Type'] = 'application/json'
  }

  // Add Clerk JWT to requests
  api.interceptors.request.use(async (config) => {
    const token = await getToken()
    if (token) {
      config.headers.Authorization = `Bearer ${token}`
    }
    return config
  })

  return api
}
```

### API Hook Pattern

Each feature has dedicated hooks in `src/hooks/`:

```tsx
// src/hooks/vendor.tsx
import { useMutation, useQuery, useQueryClient } from '@tanstack/react-query'
import useApi from './api'

export function useVendor() {
  const api = useApi()
  const queryClient = useQueryClient()

  const list = useQuery({
    queryKey: ['vendors'],
    queryFn: async () => {
      const { data } = await api.get('/vendor')
      return data
    },
  })

  const onboard = useMutation({
    mutationFn: async (payload: VendorRequest) => {
      const { data } = await api.post('/vendor', payload)
      return data
    },
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ['vendor-status'] })
    },
  })

  return { list, onboard }
}
```

## Routing (TanStack Router)

Routes are defined in `src/routes/` and auto-generated into `routeTree.gen.ts`:

### Route Structure

```
src/routes/
├── __root.tsx                 # Root route
├── (auth)/                    # Auth layout group
│   ├── index.tsx             # /
│   ├── sign-in/
│   ├── sign-up/
│   ├── otp/
│   └── forgot-password/
├── (common)/                  # Public pages
│   ├── index.tsx             # Home
│   ├── vendor.$vendorId.tsx  # Vendor public profile
│   ├── share-points/
│   └── checkout/
├── _authenticated/            # Protected routes (requires auth)
│   ├── route.tsx             # Auth layout
│   ├── dashboard/
│   ├── vendor/
│   ├── bookings/
│   └── settings/
└── errors/                   # Error pages
    ├── 401.tsx
    ├── 403.tsx
    ├── 404.tsx
    └── 500.tsx
```

### Adding a New Route

1. Create file in `src/routes/`:
```tsx
// src/routes/_authenticated/dashboard/my-page.tsx
import { createFileRoute } from '@tanstack/react-router'

export const Route = createFileRoute('/_authenticated/dashboard/my-page')({
  component: MyPage,
})

function MyPage() {
  return <div>My Page</div>
}
```

2. Routes are auto-generated - run `pnpm dev` to regenerate `routeTree.gen.ts`

## shadcn/ui Components

Components are stored in `src/components/ui/` and use **Radix UI** primitives with **Tailwind CSS**.

### Available Components

Located in `src/components/ui/`:
- `button.tsx` - Button with variants (default, destructive, outline, secondary, ghost, link)
- `card.tsx` - Card, CardHeader, CardContent, CardTitle, CardDescription, CardFooter
- `dialog.tsx` - Modal dialogs
- `input.tsx` - Form inputs
- `label.tsx` - Form labels
- `select.tsx` - Select dropdowns
- `tabs.tsx` - Tab navigation
- `avatar.tsx` - User avatars
- `switch.tsx` - Toggle switches
- `tooltip.tsx` - Tooltips
- `popover.tsx` - Popover menus
- `dropdown-menu.tsx` - Dropdown menus
- `checkbox.tsx` - Checkboxes
- `radio-group.tsx` - Radio buttons
- `scroll-area.tsx` - Custom scrollable areas
- And more...

### Adding a New Component

```bash
npx shadcn@latest add button
# or
npx shadcn@latest add button card dialog input
```

### Using Components

```tsx
import { Button } from "@/components/ui/button"
import { Card, CardContent, CardHeader, CardTitle } from "@/components/ui/card"

export function MyComponent() {
  return (
    <Card>
      <CardHeader>
        <CardTitle>Title</CardTitle>
      </CardHeader>
      <CardContent>
        <Button variant="default">Click me</Button>
      </CardContent>
    </Card>
  )
}
```

## State Management

### TanStack Query (Server State)

Used for API data fetching with caching:

```tsx
const { data, isLoading, error } = useQuery({
  queryKey: ['vendors'],
  queryFn: () => api.get('/vendor'),
})
```

### Zustand (Client State)

Used for global client state:

```tsx
// src/stores/use-cart-store.ts
import { create } from 'zustand'

interface CartItem {
  id: string
  quantity: number
}

interface CartState {
  items: CartItem[]
  addItem: (item: CartItem) => void
  removeItem: (id: string) => void
  clearCart: () => void
}

export const useCartStore = create<CartState>((set) => ({
  items: [],
  addItem: (item) => set((state) => ({ items: [...state.items, item] })),
  removeItem: (id) => set((state) => ({ items: state.items.filter(i => i.id !== id) })),
  clearCart: () => set({ items: [] }),
}))
```

## Forms (React Hook Form + Zod)

```tsx
import { useForm } from "react-hook-form"
import { zodResolver } from "@hookform/resolvers/zod"
import { z } from "zod"

const schema = z.object({
  name: z.string().min(1, "Name is required"),
  email: z.string().email("Invalid email"),
})

export function MyForm() {
  const form = useForm({
    resolver: zodResolver(schema),
  })

  return (
    <form onSubmit={form.handleSubmit(onSubmit)}>
      <Input {...form.register("name")} />
      <Button type="submit">Submit</Button>
    </form>
  )
}
```

## Authentication (Clerk)

Clerk handles all authentication:

1. **Sign In/Sign Up**: Use Clerk's `<SignIn />` and `<SignUp />` components
2. **Protected Routes**: Use `_authenticated` route group
3. **User Info**: Access via `useAuth()` hook

### Clerk Provider Setup

```tsx
// src/context/clerk-provider.tsx
import { ClerkProvider } from '@clerk/clerk-react'

export default function AppClerkProvider({ children }) {
  return (
    <ClerkProvider publishableKey={import.meta.env.VITE_CLERK_PUBLISHABLE_KEY}>
      {children}
    </ClerkProvider>
  )
}
```

## Development Commands

```bash
# Install dependencies
cd frontend && pnpm install

# Start development server (port 5173)
cd frontend && pnpm dev

# Build for production
cd frontend && pnpm build

# Preview production build
cd frontend && pnpm preview

# Run tests
cd frontend && pnpm test

# Run tests with UI
cd frontend && pnpm test:ui

# Run linting
cd frontend && pnpm lint

# Format code
cd frontend && pnpm format

# Check formatting
cd frontend && pnpm format:check
```

## Testing

Tests use **Vitest** + **React Testing Library**:

```bash
# Run tests
pnpm test

# Run in watch mode
pnpm test -- --watch

# Run with coverage
pnpm test -- --coverage

# Run tests UI
pnpm test:ui
```

Example test:
```tsx
// src/hooks/vendor.test.tsx
import { renderHook, waitFor } from '@testing-library/react'
import { useVendor } from './vendor'

vi.mock('./api', () => ({
  default: vi.fn(() => ({
    get: vi.fn(),
    post: vi.fn(),
  })),
}))

test('useVendor returns vendors', async () => {
  const { result } = renderHook(() => useVendor())
  await waitFor(() => expect(result.current.list.isSuccess).toBe(true))
})
```

## Code Patterns

### File Naming Conventions

| Type | Convention | Example |
|------|------------|---------|
| Components | PascalCase | `VendorCard.tsx`, `BookingForm.tsx` |
| Hooks | use prefix | `useBookings.ts`, `useVendor.ts` |
| Utilities | camelCase | `formatDate.ts`, `apiClient.ts` |
| Types | PascalCase | `Booking.ts`, `User.ts` |
| Routes | kebab-case | `my-page.tsx`, `vendor-$vendorId.tsx` |
| Stores | use prefix | `useCartStore.ts` |

### Component Pattern

```tsx
// src/features/vendor/components/VendorCard.tsx
interface VendorCardProps {
  vendor: Vendor
  onClick?: () => void
}

export function VendorCard({ vendor, onClick }: VendorCardProps) {
  return (
    <Card onClick={onClick}>
      <CardHeader>
        <CardTitle>{vendor.business_name}</CardTitle>
      </CardHeader>
    </Card>
  )
}
```

### Styling with Tailwind CSS

```tsx
// Use cn() utility for conditional classes
import { cn } from "@/lib/utils"

<div className={cn(
  "base-class",
  isActive && "active-class",
  variant === "primary" ? "primary-class" : "secondary-class"
)}>
```

## Conventions

1. **Component naming**: PascalCase (e.g., `VendorCard.tsx`)
2. **Hook naming**: `use` prefix (e.g., `useVendor.ts`)
3. **UI Components**: Use shadcn/ui from `@/components/ui/`
4. **Styling**: Tailwind CSS with `cn()` utility
5. **Icons**: Lucide React (`import { Icon } from "lucide-react"`)
6. **Data fetching**: TanStack Query via `useApi()` hook
7. **API Base URL**: Configured in `.env` (`VITE_API_BASE_URL`)
8. **Auth**: Clerk JWT, tokens added automatically via axios interceptor
9. **Forms**: React Hook Form + Zod validation
10. **Type safety**: Define interfaces for all props and API responses
11. **Routing**: TanStack Router with file-based routes
12. **State**: TanStack Query (server) + Zustand (client)

## Common Tasks

| Task | Location | Action |
|------|----------|--------|
| Add a new page | `src/routes/` | Create route file |
| Add a UI component | `src/components/` | Create PascalCase component |
| Add a shadcn/ui component | `src/components/ui/` | Run `npx shadcn@latest add <component>` |
| Add data fetching | `src/hooks/` | Create `useXxx` hook with TanStack Query |
| Add global state | `src/stores/` | Create Zustand store |
| Add API endpoint hook | `src/hooks/` | Create hook using `useApi()` |
| Add feature | `src/features/` | Create feature folder with components |
| Add context | `src/context/` | Create context + provider |

## Environment Variables

Required in `frontend/.env`:
- `VITE_API_BASE_URL` - Backend URL (e.g., `http://localhost:8080/api/v2/`)
- `VITE_CLERK_PUBLISHABLE_KEY` - Clerk publishable key
- `VITE_PAYSTACK_PUBLIC_KEY` - Paystack public key (payments)
- `VITE_MAPBOX_ACCESS_TOKEN` - Mapbox token (location features)
- `VITE_GOOGLE_MAPS_API_KEY` - Google Maps API key

## API Endpoints (v2)

The frontend communicates with these backend endpoints:

| Feature | Endpoints |
|---------|-----------|
| Auth | `/auth/vendor_status`, `/auth/me` |
| Vendors | `/vendor`, `/vendor/:id`, `/vendor/view/:id`, `/vendor/profile` |
| SharePoints | `/sharepoint`, `/sharepoint/:id`, `/sharepoint/:id/occurrence` |
| Bookings | `/booking`, `/booking/:id`, `/booking/:id/status` |
| Payments | `/payment/initiate`, `/payment/verify` |
| Logistics | `/logistics/initiate`, `/logistics/track/:id` |
| Livestreams | `/livestream`, `/livestream/:id`, `/livestream/:id/view` |
| Subscriptions | `/subscription`, `/subscription/:id/status` |
| Reviews | `/review/vendor/:id`, `/review` |
| AddressBook | `/addressbook`, `/addressbook/:id` |
