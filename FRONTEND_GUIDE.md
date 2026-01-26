# Frontend Salon Project - Complete Learning Guide

## Table of Contents
1. [Project Overview & Architecture](#project-overview--architecture)
2. [Dependencies & Setup](#dependencies--setup)
3. [Next.js Core Concepts](#nextjs-core-concepts)
4. [React Fundamentals](#react-fundamentals)
5. [Tailwind CSS](#tailwind-css)
6. [Project-Specific Concepts (Salon Project)](#project-specific-concepts-salon-project)
7. [Development Workflow](#development-workflow)

---

## Project Overview & Architecture

### What is Next.js?

Next.js is a React framework that provides:
- **Server-Side Rendering (SSR)**: Pages are rendered on the server for better SEO and initial load performance
- **Static Site Generation (SSG)**: Pre-render pages at build time
- **File-based Routing**: Automatic routing based on your file structure
- **API Routes**: Build backend endpoints alongside your frontend
- **Image Optimization**: Automatic image optimization and lazy loading
- **Built-in CSS Support**: Works seamlessly with CSS, SCSS, and Tailwind

### Why Use Next.js for a Salon Project?

- **Fast Performance**: Critical for booking systems where users expect quick responses
- **SEO Friendly**: Important for salon websites to rank in search results
- **Great Developer Experience**: Hot reloading, error messages, and TypeScript support
- **Production Ready**: Optimized builds and deployment options

### Project Structure

```
frontend/
├── app/                    # App Router directory (Next.js 13+)
│   ├── layout.tsx         # Root layout (wraps all pages)
│   ├── page.tsx           # Home page (/)
│   ├── about/
│   │   └── page.tsx       # About page (/about)
│   ├── services/
│   │   └── page.tsx       # Services page (/services)
│   └── api/               # API routes
│       └── appointments/
│           └── route.ts   # API endpoint
├── components/            # Reusable React components
│   ├── Header.tsx
│   ├── Footer.tsx
│   └── BookingForm.tsx
├── lib/                   # Utility functions
│   └── utils.ts
├── public/                # Static assets (images, icons)
│   └── images/
├── styles/                # Global styles
│   └── globals.css
├── tailwind.config.js     # Tailwind configuration
├── next.config.js         # Next.js configuration
└── package.json           # Dependencies
```

### Server Components vs Client Components

**Server Components (Default in Next.js 13+)**
- Run on the server
- Can directly access databases and APIs
- Smaller bundle size (code stays on server)
- Cannot use browser APIs or React hooks like `useState`, `useEffect`

```tsx
// app/services/page.tsx (Server Component)
// This runs on the server
export default async function ServicesPage() {
  // Can directly fetch data
  const services = await fetch('http://api/services').then(r => r.json())
  
  return (
    <div>
      <h1>Our Services</h1>
      {services.map(service => (
        <div key={service.id}>{service.name}</div>
      ))}
    </div>
  )
}
```

**Client Components**
- Run in the browser
- Can use React hooks and browser APIs
- Interactive features (forms, buttons, animations)
- Must be marked with `'use client'` directive

```tsx
// components/BookingForm.tsx (Client Component)
'use client'

import { useState } from 'react'

export default function BookingForm() {
  const [selectedDate, setSelectedDate] = useState('')
  
  return (
    <form>
      <input 
        type="date" 
        value={selectedDate}
        onChange={(e) => setSelectedDate(e.target.value)}
      />
    </form>
  )
}
```

---

## Dependencies & Setup

### Required Packages

#### Core Dependencies
- **next**: The Next.js framework
- **react**: React library
- **react-dom**: React DOM renderer

#### Styling
- **tailwindcss**: Utility-first CSS framework
- **postcss**: CSS processor
- **autoprefixer**: Adds vendor prefixes to CSS

#### Development Dependencies
- **typescript**: Type safety (optional but recommended)
- **@types/react**: TypeScript types for React
- **@types/node**: TypeScript types for Node.js
- **eslint**: Code linting
- **eslint-config-next**: Next.js ESLint configuration

### Installation Steps

#### 1. Initialize Next.js Project

```bash
# Navigate to frontend directory
cd frontend

# Create Next.js app with TypeScript and Tailwind
# When prompted about React Compiler, choose "No" (recommended for learning)
npx create-next-app@latest . --typescript --tailwind --app --no-src-dir --import-alias "@/*"

# Or if you prefer JavaScript
npx create-next-app@latest . --tailwind --app --no-src-dir --import-alias "@/*"
```

**Note on React Compiler**: When running `create-next-app`, you'll be asked if you want to use React Compiler. For this learning project, **choose "No"** because:
- It's still experimental and can complicate debugging
- You'll learn React fundamentals better without it
- Salon projects typically don't need its optimizations
- You can add it later if needed

#### 2. Install Additional Dependencies (if needed)

```bash
# For form handling
npm install react-hook-form zod @hookform/resolvers

# For date/time handling
npm install date-fns

# For icons
npm install lucide-react

# For HTTP requests (if not using fetch)
npm install axios
```

### Configuration Files

#### package.json

```json
{
  "name": "salon-frontend",
  "version": "0.1.0",
  "private": true,
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "next lint"
  },
  "dependencies": {
    "next": "^14.0.0",
    "react": "^18.2.0",
    "react-dom": "^18.2.0"
  },
  "devDependencies": {
    "@types/node": "^20.0.0",
    "@types/react": "^18.2.0",
    "@types/react-dom": "^18.2.0",
    "autoprefixer": "^10.4.16",
    "eslint": "^8.0.0",
    "eslint-config-next": "^14.0.0",
    "postcss": "^8.4.31",
    "tailwindcss": "^3.3.5",
    "typescript": "^5.2.2"
  }
}
```

#### tailwind.config.js

```javascript
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: [
    './pages/**/*.{js,ts,jsx,tsx,mdx}',
    './components/**/*.{js,ts,jsx,tsx,mdx}',
    './app/**/*.{js,ts,jsx,tsx,mdx}',
  ],
  theme: {
    extend: {
      colors: {
        // Custom salon brand colors
        primary: {
          50: '#fef2f2',
          100: '#fee2e2',
          500: '#ef4444',
          600: '#dc2626',
          700: '#b91c1c',
        },
        secondary: {
          500: '#8b5cf6',
          600: '#7c3aed',
        },
      },
      fontFamily: {
        sans: ['Inter', 'sans-serif'],
        display: ['Playfair Display', 'serif'],
      },
    },
  },
  plugins: [],
}
```

#### next.config.js

```javascript
/** @type {import('next').NextConfig} */
const nextConfig = {
  // Enable experimental features if needed
  // experimental: {
  //   appDir: true,
  // },
  
  // Image domains for external images
  images: {
    domains: ['example.com'],
  },
  
  // Environment variables
  env: {
    CUSTOM_KEY: process.env.CUSTOM_KEY,
  },
}

module.exports = nextConfig
```

#### tsconfig.json (if using TypeScript)

```json
{
  "compilerOptions": {
    "target": "es5",
    "lib": ["dom", "dom.iterable", "esnext"],
    "allowJs": true,
    "skipLibCheck": true,
    "strict": true,
    "noEmit": true,
    "esModuleInterop": true,
    "module": "esnext",
    "moduleResolution": "bundler",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "jsx": "preserve",
    "incremental": true,
    "plugins": [
      {
        "name": "next"
      }
    ],
    "paths": {
      "@/*": ["./*"]
    }
  },
  "include": ["next-env.d.ts", "**/*.ts", "**/*.tsx", ".next/types/**/*.ts"],
  "exclude": ["node_modules"]
}
```

### Environment Variables

Create a `.env.local` file in the frontend directory:

```env
# API endpoints
NEXT_PUBLIC_API_URL=http://localhost:3001/api

# Public keys (must start with NEXT_PUBLIC_ to be accessible in browser)
NEXT_PUBLIC_STRIPE_PUBLIC_KEY=pk_test_...

# Server-only variables (no NEXT_PUBLIC_ prefix)
DATABASE_URL=postgresql://...
SECRET_KEY=your-secret-key
```

**Important**: Variables prefixed with `NEXT_PUBLIC_` are exposed to the browser. Never put secrets in these!

### React Compiler: Should You Use It?

**Short Answer: No, not for this learning project.**

The React Compiler is an experimental optimization tool that automatically memoizes React components and values. Here's what you need to know:

#### What It Does
- Automatically optimizes React code
- Reduces need for manual `useMemo` and `useCallback`
- Can improve performance in complex applications

#### Why Skip It for Now
1. **Still Experimental**: It's relatively new and may have edge cases
2. **Learning Focus**: You'll understand React better by learning when to use `useMemo`/`useCallback` manually
3. **Debugging Complexity**: Can make debugging harder, especially when learning
4. **Unnecessary for Salon Projects**: Booking systems and service listings don't typically need these optimizations
5. **Can Add Later**: Easy to enable later if you identify performance needs

#### When You Might Want It
- Large-scale applications with complex state
- Performance-critical real-time features
- After you're comfortable with React fundamentals
- When you have specific performance bottlenecks

#### How to Enable Later (If Needed)
If you decide to use it later, you can install and configure it:

```bash
npm install babel-plugin-react-compiler
```

Then configure it in `next.config.js`. But for now, **skip it** and focus on learning React fundamentals!

---

## Next.js Core Concepts

### App Router (Next.js 13+)

The App Router uses a file-system based routing system. The `app` directory contains your routes.

#### File-Based Routing

```
app/
├── page.tsx              → / (home page)
├── about/
│   └── page.tsx         → /about
├── services/
│   ├── page.tsx         → /services
│   └── [id]/
│       └── page.tsx     → /services/:id (dynamic route)
└── booking/
    └── page.tsx         → /booking
```

#### Creating Pages

```tsx
// app/page.tsx (Home page)
export default function HomePage() {
  return (
    <main>
      <h1>Welcome to Our Salon</h1>
      <p>Book your appointment today!</p>
    </main>
  )
}
```

```tsx
// app/services/page.tsx
export default function ServicesPage() {
  return (
    <div>
      <h1>Our Services</h1>
      <ul>
        <li>Haircut</li>
        <li>Hair Coloring</li>
        <li>Manicure</li>
        <li>Pedicure</li>
      </ul>
    </div>
  )
}
```

#### Dynamic Routes

```tsx
// app/services/[id]/page.tsx
// Access via: /services/123, /services/haircut, etc.

interface PageProps {
  params: {
    id: string
  }
}

export default async function ServiceDetailPage({ params }: PageProps) {
  const { id } = params
  
  // Fetch service data based on id
  const service = await getServiceById(id)
  
  return (
    <div>
      <h1>{service.name}</h1>
      <p>{service.description}</p>
      <p>Price: ${service.price}</p>
    </div>
  )
}
```

### Layouts

Layouts wrap pages and persist across navigation.

```tsx
// app/layout.tsx (Root layout - wraps all pages)
import Header from '@/components/Header'
import Footer from '@/components/Footer'
import './globals.css'

export const metadata = {
  title: 'Salon Booking System',
  description: 'Book your salon appointment online',
}

export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="en">
      <body>
        <Header />
        <main>{children}</main>
        <Footer />
      </body>
    </html>
  )
}
```

```tsx
// app/dashboard/layout.tsx (Nested layout - only for /dashboard/*)
export default function DashboardLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <div className="flex min-h-screen">
      <aside className="w-64 bg-gray-100 p-6">Dashboard Sidebar</aside>
      <section className="flex-1 p-6">{children}</section>
    </div>
  )
}
```

### Navigation

#### Using `next/link`

```tsx
'use client'

import Link from 'next/link'

export default function Navigation() {
  return (
    <nav>
      <Link href="/">Home</Link>
      <Link href="/services">Services</Link>
      <Link href="/about">About</Link>
      <Link href="/booking">Book Appointment</Link>
    </nav>
  )
}
```

#### Programmatic Navigation

```tsx
'use client'

import { useRouter } from 'next/navigation'

export default function BookingButton() {
  const router = useRouter()
  
  const handleBooking = () => {
    // Navigate programmatically
    router.push('/booking')
  }
  
  return <button onClick={handleBooking}>Book Now</button>
}
```

### Data Fetching

#### Server Components (Recommended)

```tsx
// app/services/page.tsx
// This runs on the server - no 'use client' needed

async function getServices() {
  const res = await fetch('http://localhost:3001/api/services', {
    cache: 'no-store', // Always fetch fresh data
    // or
    // next: { revalidate: 3600 } // Revalidate every hour
  })
  
  if (!res.ok) {
    throw new Error('Failed to fetch services')
  }
  
  return res.json()
}

export default async function ServicesPage() {
  const services = await getServices()
  
  return (
    <div>
      <h1>Our Services</h1>
      {services.map(service => (
        <div key={service.id}>
          <h2>{service.name}</h2>
          <p>{service.description}</p>
        </div>
      ))}
    </div>
  )
}
```

#### Client-Side Data Fetching

```tsx
'use client'

import { useState, useEffect } from 'react'

export default function ServicesList() {
  const [services, setServices] = useState([])
  const [loading, setLoading] = useState(true)
  
  useEffect(() => {
    async function fetchServices() {
      const res = await fetch('/api/services')
      const data = await res.json()
      setServices(data)
      setLoading(false)
    }
    
    fetchServices()
  }, [])
  
  if (loading) return <div>Loading...</div>
  
  return (
    <div>
      {services.map(service => (
        <div key={service.id}>{service.name}</div>
      ))}
    </div>
  )
}
```

### API Routes

Create API endpoints in the `app/api` directory.

```tsx
// app/api/appointments/route.ts

import { NextRequest, NextResponse } from 'next/server'

// GET /api/appointments
export async function GET() {
  const appointments = await fetchAppointments()
  return NextResponse.json(appointments)
}

// POST /api/appointments
export async function POST(request: NextRequest) {
  const body = await request.json()
  
  // Validate and create appointment
  const appointment = await createAppointment(body)
  
  return NextResponse.json(appointment, { status: 201 })
}
```

### Image Optimization

```tsx
import Image from 'next/image'

export default function ServiceCard({ service }) {
  return (
    <div>
      <Image
        src={service.image}
        alt={service.name}
        width={400}
        height={300}
        // Optional: priority for above-the-fold images
        priority
      />
      <h2>{service.name}</h2>
    </div>
  )
}
```

---

## React Fundamentals

### Functional Components

```tsx
// Simple component
export default function ServiceCard() {
  return (
    <div>
      <h2>Haircut</h2>
      <p>$50</p>
    </div>
  )
}
```

### JSX Syntax

JSX (JavaScript XML) lets you write HTML-like syntax in JavaScript.

```tsx
// JSX expressions
const name = "Salon Name"
const price = 50

return (
  <div>
    <h1>{name}</h1>
    <p>Price: ${price}</p>
    <p>Total: ${price * 1.1}</p> {/* Can use expressions */}
  </div>
)
```

**JSX Rules:**
- Must return a single parent element (or use fragments `<>...</>`)
- Use `className` instead of `class`
- Use `htmlFor` instead of `for` in labels
- Self-closing tags must have `/` (e.g., `<img />`)

### Props

Props pass data from parent to child components.

```tsx
// Parent component
export default function ServicesList() {
  const services = [
    { id: 1, name: 'Haircut', price: 50 },
    { id: 2, name: 'Coloring', price: 120 },
  ]
  
  return (
    <div>
      {services.map(service => (
        <ServiceCard 
          key={service.id}
          name={service.name}
          price={service.price}
        />
      ))}
    </div>
  )
}

// Child component
interface ServiceCardProps {
  name: string
  price: number
}

export default function ServiceCard({ name, price }: ServiceCardProps) {
  return (
    <div>
      <h2>{name}</h2>
      <p>${price}</p>
    </div>
  )
}
```

### React Hooks

Hooks let you use state and other React features in functional components.

#### useState

Manages component state.

```tsx
'use client'

import { useState } from 'react'

export default function BookingForm() {
  const [selectedService, setSelectedService] = useState('')
  const [selectedDate, setSelectedDate] = useState('')
  const [customerName, setCustomerName] = useState('')
  
  return (
    <form>
      <select 
        value={selectedService}
        onChange={(e) => setSelectedService(e.target.value)}
      >
        <option value="">Select a service</option>
        <option value="haircut">Haircut</option>
        <option value="coloring">Coloring</option>
      </select>
      
      <input
        type="date"
        value={selectedDate}
        onChange={(e) => setSelectedDate(e.target.value)}
      />
      
      <input
        type="text"
        placeholder="Your name"
        value={customerName}
        onChange={(e) => setCustomerName(e.target.value)}
      />
    </form>
  )
}
```

#### useEffect

Handles side effects (API calls, subscriptions, DOM manipulation).

```tsx
'use client'

import { useState, useEffect } from 'react'

export default function AvailableSlots({ selectedDate }) {
  const [slots, setSlots] = useState([])
  const [loading, setLoading] = useState(true)
  
  useEffect(() => {
    // Runs after component mounts and when selectedDate changes
    if (!selectedDate) return
    
    async function fetchSlots() {
      setLoading(true)
      const res = await fetch(`/api/slots?date=${selectedDate}`)
      const data = await res.json()
      setSlots(data)
      setLoading(false)
    }
    
    fetchSlots()
  }, [selectedDate]) // Dependency array - effect runs when selectedDate changes
  
  if (loading) return <div>Loading slots...</div>
  
  return (
    <div>
      {slots.map(slot => (
        <button key={slot.id}>{slot.time}</button>
      ))}
    </div>
  )
}
```

**useEffect Cleanup:**

```tsx
useEffect(() => {
  const timer = setInterval(() => {
    console.log('Timer tick')
  }, 1000)
  
  // Cleanup function runs when component unmounts
  return () => {
    clearInterval(timer)
  }
}, [])
```

#### useContext

Shares state across components without prop drilling.

```tsx
'use client'

import { createContext, useContext, useState } from 'react'

// Create context
const BookingContext = createContext()

// Provider component
export function BookingProvider({ children }) {
  const [booking, setBooking] = useState({
    service: null,
    date: null,
    time: null,
  })
  
  return (
    <BookingContext.Provider value={{ booking, setBooking }}>
      {children}
    </BookingContext.Provider>
  )
}

// Custom hook to use context
export function useBooking() {
  const context = useContext(BookingContext)
  if (!context) {
    throw new Error('useBooking must be used within BookingProvider')
  }
  return context
}

// Usage in components
export default function ServiceSelector() {
  const { booking, setBooking } = useBooking()
  
  return (
    <select 
      value={booking.service || ''}
      onChange={(e) => setBooking({ ...booking, service: e.target.value })}
    >
      <option value="haircut">Haircut</option>
      <option value="coloring">Coloring</option>
    </select>
  )
}
```

### Event Handling

```tsx
'use client'

export default function BookingButton() {
  const handleClick = () => {
    alert('Booking clicked!')
  }
  
  const handleSubmit = (e) => {
    e.preventDefault() // Prevent form submission
    console.log('Form submitted')
  }
  
  return (
    <div>
      <button onClick={handleClick}>Book Now</button>
      
      <form onSubmit={handleSubmit}>
        <input type="text" />
        <button type="submit">Submit</button>
      </form>
    </div>
  )
}
```

### Conditional Rendering

```tsx
export default function AppointmentStatus({ appointment }) {
  // Using if statements
  if (appointment.status === 'confirmed') {
    return <div className="text-green-500">Confirmed</div>
  }
  
  if (appointment.status === 'pending') {
    return <div className="text-yellow-500">Pending</div>
  }
  
  return <div className="text-red-500">Cancelled</div>
}

// Using ternary operator
export default function ServiceCard({ service }) {
  return (
    <div>
      <h2>{service.name}</h2>
      {service.onSale ? (
        <p className="text-red-500">Sale: ${service.salePrice}</p>
      ) : (
        <p>${service.price}</p>
      )}
    </div>
  )
}

// Using logical AND
export default function BookingForm({ user }) {
  return (
    <form>
      {user && <p>Welcome, {user.name}!</p>}
      {user?.isAdmin && <button>Admin Panel</button>}
    </form>
  )
}
```

### Lists and Keys

```tsx
export default function ServicesList({ services }) {
  return (
    <ul>
      {services.map(service => (
        <li key={service.id}>
          <h3>{service.name}</h3>
          <p>${service.price}</p>
        </li>
      ))}
    </ul>
  )
}
```

**Important**: Always use a unique `key` prop when rendering lists. Keys help React identify which items changed.

---

## Tailwind CSS

### Utility-First Approach

Instead of writing custom CSS, Tailwind provides utility classes you compose together.

**Traditional CSS:**
```css
.service-card {
  padding: 1.5rem;
  background-color: white;
  border-radius: 0.5rem;
  box-shadow: 0 1px 3px rgba(0,0,0,0.1);
}
```

**Tailwind CSS:**
```tsx
<div className="p-6 bg-white rounded-lg shadow-md">
  Service Card
</div>
```

### Common Utility Classes

#### Spacing

```tsx
// Padding
<div className="p-4">Padding all sides</div>
<div className="px-4 py-2">Padding x and y</div>
<div className="pt-4 pb-2 pl-3 pr-5">Individual padding</div>

// Margin
<div className="m-4">Margin all sides</div>
<div className="mx-auto">Center horizontally</div>
<div className="mt-8 mb-4">Top and bottom margin</div>

// Spacing scale: 0, 1 (0.25rem), 2 (0.5rem), 4 (1rem), 6 (1.5rem), 8 (2rem), etc.
```

#### Colors

```tsx
// Background colors
<div className="bg-white">White background</div>
<div className="bg-gray-100">Light gray</div>
<div className="bg-blue-500">Blue background</div>
<div className="bg-primary-500">Custom primary color</div>

// Text colors
<p className="text-gray-900">Dark text</p>
<p className="text-blue-600">Blue text</p>
<p className="text-red-500">Red text</p>

// Border colors
<div className="border border-gray-300">Gray border</div>
<div className="border-2 border-blue-500">Blue border</div>
```

#### Typography

```tsx
// Font sizes
<h1 className="text-4xl">Large heading</h1>
<h2 className="text-2xl">Medium heading</h2>
<p className="text-base">Body text</p>
<p className="text-sm">Small text</p>

// Font weights
<p className="font-normal">Normal weight</p>
<p className="font-bold">Bold</p>
<p className="font-semibold">Semi-bold</p>

// Text alignment
<p className="text-left">Left aligned</p>
<p className="text-center">Center aligned</p>
<p className="text-right">Right aligned</p>

// Text decoration
<p className="underline">Underlined</p>
<p className="line-through">Strikethrough</p>
```

#### Layout

```tsx
// Flexbox
<div className="flex">Flex container</div>
<div className="flex items-center">Vertical center</div>
<div className="flex justify-between">Space between</div>
<div className="flex gap-4">Gap between items</div>

// Grid
<div className="grid grid-cols-3 gap-4">3 column grid</div>
<div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3">Responsive grid</div>

// Display
<div className="hidden">Hidden</div>
<div className="block">Block</div>
<div className="inline-block">Inline block</div>
```

#### Borders & Rounded Corners

```tsx
<div className="border">Border</div>
<div className="border-2 border-gray-300">Thick border</div>
<div className="rounded">Rounded corners</div>
<div className="rounded-lg">Large rounded</div>
<div className="rounded-full">Full circle</div>
```

#### Shadows

```tsx
<div className="shadow-sm">Small shadow</div>
<div className="shadow-md">Medium shadow</div>
<div className="shadow-lg">Large shadow</div>
<div className="shadow-xl">Extra large shadow</div>
```

### Responsive Design

Tailwind uses mobile-first breakpoints:

```tsx
// Base (mobile): text-sm
// md (768px+): text-base
// lg (1024px+): text-lg
// xl (1280px+): text-xl
// 2xl (1536px+): text-2xl

<div className="
  text-sm 
  md:text-base 
  lg:text-lg 
  xl:text-xl
">
  Responsive text
</div>

<div className="
  grid 
  grid-cols-1 
  md:grid-cols-2 
  lg:grid-cols-3
">
  Responsive grid
</div>

<div className="
  p-4 
  md:p-6 
  lg:p-8
">
  Responsive padding
</div>
```

### Hover, Focus, and Active States

```tsx
<button className="
  bg-blue-500 
  hover:bg-blue-600 
  active:bg-blue-700 
  focus:outline-none 
  focus:ring-2 
  focus:ring-blue-500
">
  Interactive Button
</button>

<a className="
  text-blue-500 
  hover:text-blue-700 
  hover:underline
">
  Link
</a>
```

### Component Patterns with Tailwind

#### Card Component

```tsx
export default function ServiceCard({ service }) {
  return (
    <div className="
      bg-white 
      rounded-lg 
      shadow-md 
      p-6 
      hover:shadow-lg 
      transition-shadow
    ">
      <h3 className="text-xl font-bold mb-2">{service.name}</h3>
      <p className="text-gray-600 mb-4">{service.description}</p>
      <p className="text-2xl font-semibold text-primary-600">
        ${service.price}
      </p>
      <button className="
        mt-4 
        w-full 
        bg-primary-500 
        text-white 
        py-2 
        rounded 
        hover:bg-primary-600 
        transition-colors
      ">
        Book Now
      </button>
    </div>
  )
}
```

#### Form Input

```tsx
export default function FormInput({ label, type = 'text', ...props }) {
  return (
    <div className="mb-4">
      <label className="block text-sm font-medium text-gray-700 mb-1">
        {label}
      </label>
      <input
        type={type}
        className="
          w-full 
          px-4 
          py-2 
          border 
          border-gray-300 
          rounded-lg 
          focus:outline-none 
          focus:ring-2 
          focus:ring-primary-500 
          focus:border-transparent
        "
        {...props}
      />
    </div>
  )
}
```

### Custom Configuration

Extend Tailwind in `tailwind.config.js`:

```javascript
module.exports = {
  theme: {
    extend: {
      colors: {
        salon: {
          primary: '#ef4444',
          secondary: '#8b5cf6',
        },
      },
      spacing: {
        '128': '32rem',
      },
      fontFamily: {
        sans: ['Inter', 'sans-serif'],
      },
    },
  },
}
```

---

## Project-Specific Concepts (Salon Project)

### Component Structure for Salon Features

#### Service Listing Component

```tsx
// components/ServiceList.tsx
'use client'

import { useState } from 'react'
import ServiceCard from './ServiceCard'

interface Service {
  id: string
  name: string
  description: string
  price: number
  duration: number
}

export default function ServiceList({ services }: { services: Service[] }) {
  const [selectedCategory, setSelectedCategory] = useState('all')
  
  const filteredServices = selectedCategory === 'all'
    ? services
    : services.filter(s => s.category === selectedCategory)
  
  return (
    <div>
      {/* Category Filter */}
      <div className="flex gap-4 mb-6">
        <button
          onClick={() => setSelectedCategory('all')}
          className={selectedCategory === 'all' ? 'font-bold' : ''}
        >
          All
        </button>
        <button onClick={() => setSelectedCategory('hair')}>Hair</button>
        <button onClick={() => setSelectedCategory('nails')}>Nails</button>
      </div>
      
      {/* Service Grid */}
      <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
        {filteredServices.map(service => (
          <ServiceCard key={service.id} service={service} />
        ))}
      </div>
    </div>
  )
}
```

#### Booking Form Component

```tsx
// components/BookingForm.tsx
'use client'

import { useState } from 'react'

export default function BookingForm() {
  const [formData, setFormData] = useState({
    service: '',
    date: '',
    time: '',
    name: '',
    email: '',
    phone: '',
  })
  
  const [errors, setErrors] = useState({})
  
  const handleChange = (e) => {
    const { name, value } = e.target
    setFormData(prev => ({
      ...prev,
      [name]: value
    }))
    // Clear error when user types
    if (errors[name]) {
      setErrors(prev => ({
        ...prev,
        [name]: ''
      }))
    }
  }
  
  const validate = () => {
    const newErrors = {}
    
    if (!formData.service) newErrors.service = 'Please select a service'
    if (!formData.date) newErrors.date = 'Please select a date'
    if (!formData.time) newErrors.time = 'Please select a time'
    if (!formData.name) newErrors.name = 'Name is required'
    if (!formData.email) newErrors.email = 'Email is required'
    
    setErrors(newErrors)
    return Object.keys(newErrors).length === 0
  }
  
  const handleSubmit = async (e) => {
    e.preventDefault()
    
    if (!validate()) return
    
    try {
      const response = await fetch('/api/appointments', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(formData),
      })
      
      if (response.ok) {
        alert('Booking confirmed!')
        // Reset form or redirect
      }
    } catch (error) {
      console.error('Booking failed:', error)
    }
  }
  
  return (
    <form onSubmit={handleSubmit} className="max-w-md mx-auto">
      <div className="mb-4">
        <label className="block mb-1">Service</label>
        <select
          name="service"
          value={formData.service}
          onChange={handleChange}
          className="w-full px-4 py-2 border rounded"
        >
          <option value="">Select a service</option>
          <option value="haircut">Haircut</option>
          <option value="coloring">Coloring</option>
        </select>
        {errors.service && <p className="text-red-500 text-sm">{errors.service}</p>}
      </div>
      
      <div className="mb-4">
        <label className="block mb-1">Date</label>
        <input
          type="date"
          name="date"
          value={formData.date}
          onChange={handleChange}
          className="w-full px-4 py-2 border rounded"
        />
        {errors.date && <p className="text-red-500 text-sm">{errors.date}</p>}
      </div>
      
      <div className="mb-4">
        <label className="block mb-1">Time</label>
        <select
          name="time"
          value={formData.time}
          onChange={handleChange}
          className="w-full px-4 py-2 border rounded"
        >
          <option value="">Select a time</option>
          <option value="09:00">9:00 AM</option>
          <option value="10:00">10:00 AM</option>
          <option value="11:00">11:00 AM</option>
        </select>
        {errors.time && <p className="text-red-500 text-sm">{errors.time}</p>}
      </div>
      
      <div className="mb-4">
        <label className="block mb-1">Name</label>
        <input
          type="text"
          name="name"
          value={formData.name}
          onChange={handleChange}
          className="w-full px-4 py-2 border rounded"
        />
        {errors.name && <p className="text-red-500 text-sm">{errors.name}</p>}
      </div>
      
      <div className="mb-4">
        <label className="block mb-1">Email</label>
        <input
          type="email"
          name="email"
          value={formData.email}
          onChange={handleChange}
          className="w-full px-4 py-2 border rounded"
        />
        {errors.email && <p className="text-red-500 text-sm">{errors.email}</p>}
      </div>
      
      <button
        type="submit"
        className="w-full bg-primary-500 text-white py-2 rounded hover:bg-primary-600"
      >
        Book Appointment
      </button>
    </form>
  )
}
```

### State Management Patterns

#### Local State (useState)

For component-specific state:

```tsx
const [selectedService, setSelectedService] = useState('')
```

#### Context API

For shared state across multiple components:

```tsx
// contexts/BookingContext.tsx
'use client'

import { createContext, useContext, useState } from 'react'

const BookingContext = createContext()

export function BookingProvider({ children }) {
  const [booking, setBooking] = useState({
    service: null,
    date: null,
    time: null,
    customer: null,
  })
  
  return (
    <BookingContext.Provider value={{ booking, setBooking }}>
      {children}
    </BookingContext.Provider>
  )
}

export const useBooking = () => useContext(BookingContext)
```

#### Server State (for data fetching)

Use Server Components or fetch in `useEffect`:

```tsx
// Server Component (preferred)
async function ServicesPage() {
  const services = await fetchServices()
  return <ServiceList services={services} />
}
```

### API Integration Patterns

#### Fetching Data

```tsx
// In Server Component
async function getAppointments() {
  const res = await fetch(`${process.env.NEXT_PUBLIC_API_URL}/appointments`, {
    cache: 'no-store',
  })
  return res.json()
}

// In Client Component
useEffect(() => {
  async function fetchData() {
    const res = await fetch('/api/appointments')
    const data = await res.json()
    setAppointments(data)
  }
  fetchData()
}, [])
```

#### Creating Data

```tsx
const handleBooking = async (bookingData) => {
  try {
    const response = await fetch('/api/appointments', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify(bookingData),
    })
    
    if (!response.ok) {
      throw new Error('Booking failed')
    }
    
    const result = await response.json()
    return result
  } catch (error) {
    console.error('Error:', error)
    throw error
  }
}
```

### Best Practices for Salon Booking System

1. **Date/Time Handling**
   - Use a date library like `date-fns` or `dayjs`
   - Validate dates are in the future
   - Check business hours
   - Prevent double bookings

2. **Form Validation**
   - Validate on both client and server
   - Show clear error messages
   - Disable submit button while processing

3. **User Experience**
   - Show loading states
   - Provide confirmation messages
   - Allow cancellation/modification
   - Send email confirmations (backend)

4. **Performance**
   - Use Server Components for data fetching
   - Implement pagination for long lists
   - Optimize images
   - Lazy load components

---

## Development Workflow

### Running the Development Server

```bash
# Start development server (usually on http://localhost:3000)
npm run dev

# Or with yarn
yarn dev
```

The server will:
- Hot reload on file changes
- Show compilation errors in the browser
- Provide helpful error messages

### Building for Production

```bash
# Create optimized production build
npm run build

# Start production server
npm start
```

### Common Commands

```bash
# Install dependencies
npm install

# Run linter
npm run lint

# Type checking (if using TypeScript)
npx tsc --noEmit
```

### Project Structure Best Practices

```
frontend/
├── app/                    # Next.js app directory
│   ├── (routes)/          # Route groups (optional)
│   ├── api/               # API routes
│   └── globals.css        # Global styles
├── components/            # Reusable components
│   ├── ui/                # Basic UI components
│   └── features/          # Feature-specific components
├── lib/                   # Utility functions
│   ├── utils.ts
│   └── api.ts
├── hooks/                 # Custom React hooks
├── contexts/              # React contexts
├── types/                 # TypeScript types (if using TS)
└── public/                # Static files
```

### Debugging Tips

1. **Browser DevTools**
   - Use React DevTools extension
   - Check Network tab for API calls
   - Use Console for debugging

2. **Next.js Error Overlay**
   - Errors appear automatically in browser
   - Click to see file and line number

3. **Console Logging**
   ```tsx
   console.log('Debug value:', value)
   console.table(arrayData) // Nice for arrays
   ```

4. **TypeScript Errors**
   - Read error messages carefully
   - Hover over variables to see types
   - Use `any` sparingly (only when necessary)

### Common Issues and Solutions

**Issue: "Module not found"**
- Check import paths
- Verify file exists
- Check `tsconfig.json` paths configuration

**Issue: "Hydration error"**
- Server and client HTML don't match
- Usually caused by using browser-only APIs in Server Components
- Move to Client Component with `'use client'`

**Issue: "Cannot read property of undefined"**
- Add optional chaining: `data?.property`
- Add default values: `data || {}`
- Check if data is loaded before rendering

**Issue: Styles not applying**
- Check Tailwind classes are spelled correctly
- Verify `tailwind.config.js` content paths
- Restart dev server after config changes

---

## Next Steps

1. **Set up the project** using the installation steps
2. **Create your first page** in `app/page.tsx`
3. **Build components** one at a time
4. **Connect to your backend** API
5. **Style with Tailwind** as you build
6. **Test thoroughly** before deploying

Remember: Start simple, then add complexity. Build one feature at a time, and test as you go!

---

## Additional Resources

- [Next.js Documentation](https://nextjs.org/docs)
- [React Documentation](https://react.dev)
- [Tailwind CSS Documentation](https://tailwindcss.com/docs)
- [TypeScript Handbook](https://www.typescriptlang.org/docs/)

Happy coding! 🎨

