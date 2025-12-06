## Overview

This project uses the following tech stack:
- Vite
- Typescript
- React Router v7 (all imports from `react-router` instead of `react-router-dom`)
- React 19 (for frontend components)
- Tailwind v4 (for styling)
- Shadcn UI (for UI components library)
- Lucide Icons (for icons)
- Convex (for backend & database)
- Convex Auth (for authentication)
- Framer Motion (for animations)
- Three js (for 3d models)

All relevant files live in the `src` directory.

Use pnpm for the package manager.

## Setup

This project is set up already and running on a cloud environment, as well as a Convex deployment in the sandbox.

To run the project locally:

1. **Clone and install**

git clone https://github.com/Varshaisnice/vate-glow-final-1.git
cd vate-glow-final-1
pnpm install


2. **Configure environment variables**

Create `.env.local` in the project root (or update it if it already exists):

VITE_CONVEX_URL=<your Convex deployment public URL>
CONVEX_DEPLOYMENT=<your Convex deployment name>


The Convex server has its own environment variables configured in the Convex dashboard (e.g. `JWKS`, `JWT_PRIVATE_KEY`, `SITE_URL`) and they are not loaded from this repo.

3. **Start Convex dev server**

In one terminal:

npx convex dev

text

4. **Start the Vite dev server**

In another terminal:

pnpm dev

text

The app will be available at `http://localhost:5173`.

5. **Admin access**

Authentication is available at `/auth`. To access the admin dashboard locally, sign in with the configured admin account (currently `mvarsha4306@gmail.com`) and then navigate to `/admin`.

## Environment Variables

The project is set up with project specific `CONVEX_DEPLOYMENT` and `VITE_CONVEX_URL` environment variables on the client side.

The Convex server has a separate set of environment variables that are accessible by the Convex backend.

Currently, these variables include auth-specific keys: `JWKS`, `JWT_PRIVATE_KEY`, and `SITE_URL`.

# Using Authentication (Important!)

You must follow these conventions when using authentication.

## Auth is already set up.

All Convex authentication functions are already set up. The auth currently uses email OTP and anonymous users, but can support more.

The email OTP configuration is defined in `src/convex/auth/emailOtp.ts`. DO NOT MODIFY THIS FILE.

Also, DO NOT MODIFY THESE AUTH FILES: `src/convex/auth.config.ts` and `src/convex/auth.ts`.

## Using Convex Auth on the backend

On the `src/convex/authHelpers.ts` file, you can use the `getCurrentUser` function to get the current user's data.

## Using Convex Auth on the frontend

The `/auth` page is already set up to use auth. Navigate to `/auth` for all log in / sign up sequences.

You MUST use this hook to get user data. Never do this yourself without the hook:

import { useAuth } from "@/hooks/use-auth";

const { isLoading, isAuthenticated, user, signIn, signOut } = useAuth();

text

## Protected Routes

When protecting a page, use the auth hooks to check for authentication and redirect to `/auth`.

## Auth Page

The auth page is defined in `src/pages/Auth.tsx`. Redirect authenticated pages and sign in / sign up to `/auth`.

## Authorization

You can perform authorization checks on the frontend and backend.

On the frontend, you can use the `useAuth` hook to get the current user's data and authentication state.

You should also be protecting queries, mutations, and actions at the base level, checking for authorization securely.

## Adding a redirect after auth

In `src/main.tsx`, you must add a redirect-after-auth URL to redirect to the correct dashboard/profile/page that should be created after authentication.

# Frontend Conventions

You will be using the Vite frontend with React 19, Tailwind v4, and Shadcn UI.

Generally, pages should be in the `src/pages` folder, and components should be in the `src/components` folder.

Shadcn primitives are located in the `src/components/ui` folder and should be used by default.

## Page routing

Your page component should go under the `src/pages` folder.

When adding a page, update the React Router configuration in `src/main.tsx` to include the new route you just added.

## Shad CN conventions

Follow these conventions when using Shad CN components, which you should use by default.
- Remember to use `cursor-pointer` to make the element clickable.
- For title text, use the `tracking-tight font-bold` class to make the text more readable.
- Always make apps **mobile responsive**.
- Avoid nested cards; nested borders/components add clutter.
- Avoid shadows; use thin borders without shadows.
- Avoid skeletons; instead, use the `loader2` component to show a spinning loading state when loading data.

## Landing Pages

You must always create good-looking designer-level styles for your application. 
- Make it well animated and fit a certain theme (neo brutalist, retro, neumorphism, glass morphism, etc).

Use known images and emojis from online.

If the user is logged in already, show the Get Started button as “Dashboard” or “Profile” instead and take them there.

## Responsiveness and formatting

Make sure pages are wrapped in a container to prevent the width stretching out on wide screens. Always make sure they are center aligned.

Always make sure that your designs are mobile responsive. Verify formatting for correct max/min widths.

- Always create sidebars for protected dashboard pages and navigate between pages.
- Always create navbars for landing pages.
- The logo should be clickable and redirect to the index page.

## Animating with Framer Motion

You must add animations to components using Framer Motion. It is already installed and configured.

To use it, import the `motion` component from `framer-motion` and wrap the component you want to animate.

Things to animate:
- Fade in / fade out
- Slide in / slide out
- Render animations
- Button clicks and UI elements

Animate for all components, including landing and app pages.

## Three JS Graphics

Your app comes with Three.js by default. You can use it to create 3D graphics for landing pages, games, etc.

## Colors

You can override colors in `src/index.css`.

This uses the `oklch` color format for Tailwind v4.

Always use these color variable names.

Make sure all UI components are mobile responsive and compatible with both light and dark mode.

Set theme using `dark` or `light` variables at the parent `className`.

## Styling and Theming

When changing the theme, always change the underlying theme of the Shadcn components app-wide under `src/components/ui` and the colors in `index.css`.

Avoid hardcoding colors unless necessary, and implement themes through the Shadcn UI components.

When styling, ensure buttons and clickable items have `cursor-pointer`.

Always follow a consistent theme style and ensure it matches the app’s look.

## Toasts

You should always use toasts to display results to the user (confirmations, results, errors, etc.).

Use the Shadcn Sonner component as the toaster:

import { toast } from "sonner";
import { Button } from "@/components/ui/button";

export function SonnerDemo() {
return (
<Button
variant="outline"
onClick={() =>
toast("Event has been created", {
description: "Sunday, December 03, 2023 at 9:00 AM",
action: {
label: "Undo",
onClick: () => console.log("Undo"),
},
})
}
>
Show Toast
</Button>
);
}

text

Remember to `import { toast } from "sonner"`. Usage: `toast("Event has been created.")`.

## Dialogs

Always ensure your larger dialogs have scrollable content so everything fits on screen.

Ideally, instead of using a new page, use a Dialog instead.

# Using the Convex backend

You will be implementing the Convex backend. Follow your knowledge of Convex and the documentation.

## The Convex Schema

The schema is defined in `src/convex/schema.ts`.

- Do not include the `_id` and `_creationTime` fields in your queries (they are included by default).
- Do not index `_creationTime` (it is indexed for you).
- Never create duplicate indexes.

## Convex Actions: Using CRUD operations

When running anything that involves external connections, you must use a Convex **action** with `"use node"` at the top of the file.

You cannot have queries or mutations in the same file as a `"use node"` action file. Use pre-built queries and mutations in other files.

You can also use the pre-installed internal CRUD functions for the database:

// in convex/users.ts
import { crud } from "convex-helpers/server/crud";
import schema from "./schema";

export const { create, read, update, destroy } = crud(schema, "users");

// in some file, in an action:
const user = await ctx.runQuery(internal.users.read, { id: userId });

await ctx.runMutation(internal.users.update, {
id: userId,
patch: {
status: "inactive",
},
});

text

## Common Convex Mistakes To Avoid

When using Convex, make sure:

- Document IDs are referenced as `_id`, not `id`.
- Document ID types are `Id<"TableName">`, not `string`.
- Document object types are `Doc<"TableName">`.
- Keep `schemaValidation` to `false` in the schema file.
- Type your code so it passes the type checker.
- Handle `null` / `undefined` cases of your Convex queries on both frontend and backend.
- Always use the `@/` alias path, e.g. `@/convex/_generated/server`, `@/convex/_generated/api`.
- Import hooks like `useQuery`, `useMutation`, `useAction` from `convex/react`.
- Never use return type validators.

