# AI Coach - Project Structure & Documentation

## Project Overview

**AI Coach** is a Next.js-based web application that provides AI-powered career coaching services. It helps users with resume building, cover letter generation, interview preparation, and skill assessments.

---

## 📁 Folder Structure & Purpose

### Root Level Files

| File                 | Purpose                                              |
| -------------------- | ---------------------------------------------------- |
| `package.json`       | Project dependencies and npm scripts configuration   |
| `next.config.mjs`    | Next.js configuration settings                       |
| `tailwind.config.js` | Tailwind CSS configuration for styling               |
| `postcss.config.mjs` | PostCSS configuration for CSS processing             |
| `eslint.config.mjs`  | ESLint configuration for code linting                |
| `jsconfig.json`      | JavaScript path aliases and compiler options         |
| `components.json`    | UI component configuration (likely for shadcn/ui)    |
| `middleware.js`      | Clerk authentication middleware for route protection |
| `.env`               | Environment variables (not committed to git)         |
| `.gitignore`         | Git ignore rules                                     |
| `README.md`          | Original project readme                              |

---

## 📂 Main Directories

### `/app` - Next.js App Router Pages

The main application structure using Next.js 15 with App Router.

```
app/
├── page.jsx                      # Landing/home page
├── layout.js                     # Root layout wrapper
├── globals.css                   # Global CSS styles
├── (auth)/                       # Authentication routes (sign-in/sign-up)
│   ├── layout.js
│   ├── sign-in/[[...sign-in]]/   # Clerk sign-in page
│   └── sign-up/[[...sign-up]]/   # Clerk sign-up page
│
├── (main)/                       # Protected main application routes
│   ├── layout.js
│   ├── dashboard/                # User dashboard
│   │   ├── page.jsx
│   │   ├── layout.js
│   │   └── _components/          # Dashboard sub-components
│   │
│   ├── resume/                   # Resume builder feature
│   │   ├── page.jsx
│   │   ├── layout.js
│   │   ├── new/                  # Create new resume
│   │   ├── [id]/                 # View/edit specific resume
│   │   └── _components/          # Resume-related components
│   │
│   ├── ai-cover-letter/          # AI cover letter generator
│   │   ├── page.jsx
│   │   ├── new/                  # Create new cover letter
│   │   ├── [id]/                 # View/edit specific cover letter
│   │   └── _components/          # Cover letter components
│   │
│   ├── interview/                # Interview preparation
│   │   ├── page.jsx
│   │   ├── layout.js
│   │   ├── mock/                 # Mock interview feature
│   │   └── _components/          # Interview components
│   │
│   └── onboarding/               # User onboarding flow
│       ├── page.jsx
│       └── _components/          # Onboarding components
│
└── api/
    └── inngest/
        └── route.js              # Inngest event handling API route
```

**Purpose:** Contains all page routes and server-side logic for the application.

---

### `/components` - Reusable React Components

```
components/
├── header.jsx                    # Navigation header component
├── hero.jsx                      # Hero section on landing page
├── theme-provider.jsx            # Theme context provider
└── ui/                           # Reusable UI components (shadcn/ui)
    ├── accordion.jsx
    ├── alert-dialog.jsx
    ├── badge.jsx
    ├── button.jsx
    ├── card.jsx
    ├── dialog.jsx
    ├── dropdown-menu.jsx
    ├── input.jsx
    ├── label.jsx
    ├── progress.jsx
    ├── radio-group.jsx
    ├── select.jsx
    ├── sonner.jsx               # Toast notifications
    ├── tabs.jsx
    └── textarea.jsx
```

**Purpose:** Centralized location for reusable React components used across the application.

---

### `/lib` - Utility Functions & Configuration

```
lib/
├── utils.js                      # General utility functions
├── helper.js                     # Helper functions
├── checkUser.js                  # User validation/check functions
├── prisma.js                     # Prisma client singleton
├── schema.js                     # Database schema helpers
└── generated/prisma/             # Auto-generated Prisma client
    ├── client.js                 # Prisma client (auto-generated)
    ├── client.d.ts               # TypeScript definitions
    ├── query_engine-*.node       # Binary query engines for different OS
    └── ...other generated files
```

**Purpose:** Contains utility functions, database configuration, and generated Prisma client code.

---

### `/actions` - Server Actions

```
actions/
├── user.js                       # User-related server actions
├── resume.js                     # Resume CRUD operations
├── cover-letter.js               # Cover letter operations
├── interview.js                  # Interview session actions
└── dashboard.js                  # Dashboard data operations
```

**Purpose:** Next.js server actions for backend logic (replaces traditional API routes in many cases).

---

### `/hooks` - Custom React Hooks

```
hooks/
└── use-fetch.js                  # Custom hook for data fetching
```

**Purpose:** Custom React hooks for shared stateful logic across components.

---

### `/data` - Static Data & Constants

```
data/
├── faqs.js                       # FAQ items for landing page
├── features.js                   # Feature list for marketing
├── howItWorks.js                 # How the app works section data
├── industries.js                 # Industry/subindustry categories
└── testimonial.js                # User testimonials data
```

**Purpose:** Static data and configuration constants used throughout the application.

---

### `/prisma` - Database Configuration

```
prisma/
├── schema.prisma                 # Database schema definition
│   - User model (with Clerk integration)
│   - Assessment model (quiz results)
│   - Resume model (user resumes)
│   - CoverLetter model (generated cover letters)
│   - IndustryInsight model (industry-specific data)
│
└── migrations/                   # Database migration history
    ├── migration_lock.toml
    ├── 20250908185649_init/      # Initial schema
    └── 20250913023906_fix_keytrends_array/  # Schema updates
```

**Purpose:** Prisma ORM configuration and PostgreSQL database schema.

**Key Models:**

- **User**: Stores user profile, skills, experience (linked with Clerk auth)
- **Resume**: Stores user resumes with content
- **CoverLetter**: Stores generated cover letters with job context
- **Assessment**: Tracks quiz scores and improvement areas
- **IndustryInsight**: Industry-specific career insights

---

### `/public` - Static Assets

```
public/
└── [static files like images, fonts, icons]
```

**Purpose:** Static files served directly by Next.js (images, favicons, etc.).

---

### `/inngest` - Event Processing

```
lib/inngest/
├── client.js                     # Inngest client configuration
└── functions.js                  # Background job definitions
```

**Purpose:** Inngest event handling for asynchronous tasks (background jobs, notifications, etc.).

---

## 🔐 Authentication & Security

- **Auth Provider**: Clerk (`@clerk/nextjs`)
- **Protected Routes**: Middleware in `middleware.js` protects:
  - `/dashboard`
  - `/resume-builder`
  - `/cover-letter`
  - `/interview`
  - `/onboarding`
- **Route Groups**:
  - `(auth)` - Public authentication routes
  - `(main)` - Protected application routes

---

## 🗄️ Database Schema Overview

### User

- Stores user profile linked with Clerk authentication
- Fields: `id`, `clerkUserId`, `email`, `name`, `imageUrl`, `industry`, `bio`, `experience`, `skills[]`
- Relations: Has many `Assessments`, `Resumes`, `CoverLetters`

### Resume

- One resume per user
- Fields: `id`, `userId`, `content` (Text), `createdAt`, `updatedAt`

### CoverLetter

- Multiple cover letters per user (for different job applications)
- Fields: `id`, `userId`, `content`, `jobDescription`, `companyName`, `jobTitle`, `createdAt`, `updatedAt`

### Assessment

- Tracks quiz/interview performance
- Fields: `id`, `userId`, `quizScore`, `questions[]`, `category`, `improvementTip`, `createdAt`, `updatedAt`

### IndustryInsight

- Industry-specific data and insights
- Linked to User model via `industry` field

---

## 🚀 Key Features

1. **Resume Builder** - Create and edit resumes with AI assistance
2. **Cover Letter Generator** - Generate AI-powered cover letters
3. **Interview Preparation** - Mock interviews and assessment questions
4. **Skill Assessment** - Track and improve professional skills
5. **Dashboard** - User profile and progress tracking
6. **Onboarding** - Industry and skill selection flow

---

## 🛠️ Tech Stack

- **Framework**: Next.js 15 with App Router
- **Authentication**: Clerk
- **Database**: PostgreSQL with Prisma ORM
- **AI**: Google Generative AI (@google/generative-ai)
- **Styling**: Tailwind CSS
- **UI Components**: Radix UI + shadcn/ui
- **Forms**: React Hook Form + Zod validation
- **Background Jobs**: Inngest
- **Notifications**: Sonner (toast notifications)
- **State Management**: React Hook Form for forms
- **Charts**: Recharts

---

## 📝 Key Configuration Files

### `middleware.js`

Handles Clerk authentication and route protection using the `isProtectedRoute` matcher.

### `next.config.mjs`

Next.js configuration for the application.

### `tailwind.config.js`

Tailwind CSS customization and theme setup.

### `jsconfig.json`

Path aliases:

- `@/` → root directory (for easier imports)

---

## 🔄 Development Workflow

```bash
# Install dependencies
npm install

# Run development server with Turbopack
npm run dev

# Build for production
npm run build

# Start production server
npm start

# Run linting
npm run lint
```

---

## 📦 Important Dependencies

| Package                 | Purpose                          |
| ----------------------- | -------------------------------- |
| `@clerk/nextjs`         | Authentication & user management |
| `@prisma/client`        | Database ORM                     |
| `@google/generative-ai` | AI model integration             |
| `next`                  | React framework                  |
| `react-hook-form`       | Form state management            |
| `zod`                   | Schema validation                |
| `@radix-ui/*`           | Accessible UI components         |
| `tailwindcss`           | CSS framework                    |
| `inngest`               | Background job processing        |
| `sonner`                | Toast notifications              |

---

## 🎯 Folder Naming Conventions

- **`_components/`** - Private components used only in that specific feature (Next.js convention)
- **`[id]/`** - Dynamic route segments for specific resource IDs
- **`[[...slug]]/`** - Optional catch-all routes (used for Clerk auth pages)
- **`(groupName)/`** - Route groups for logical organization without affecting URL

---

## 📌 Notes

- All database migrations are tracked in `/prisma/migrations/`
- Prisma client is auto-generated in `/lib/generated/prisma/`
- Environment variables are loaded from `.env` file (not committed)
- ESLint is configured for code quality checks
- The app uses Tailwind CSS with PostCSS for styling

---

**Last Updated**: May 5, 2026
