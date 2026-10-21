# PostHog in this Next.js app

From https://posthog.com/docs/libraries/next-js (checked 2026-10-21).

## Install

1. Install posthog-js with the project's package manager, e.g. `npm install --save posthog-js`.
   - Correct: the package manager adds posthog-js to package.json.
   - Incorrect: hand-editing package.json.

## Initialize

2. Initialize once, in `instrumentation-client.ts` (or `.js`) at the app root. This covers both the app and pages router.
   - Correct:

     ```ts
     import posthog from 'posthog-js'

     posthog.init(process.env.NEXT_PUBLIC_POSTHOG_PROJECT_TOKEN!, {
       api_host: process.env.NEXT_PUBLIC_POSTHOG_HOST,
       defaults: '2026-05-30'
     })
     ```

   - Incorrect: a PostHogProvider or `posthog.init` in `app/layout.tsx`, or a second init anywhere.

## Configure

3. Environment variables are `NEXT_PUBLIC_POSTHOG_PROJECT_TOKEN` and `NEXT_PUBLIC_POSTHOG_HOST`.
   - Incorrect: `NEXT_PUBLIC_POSTHOG_KEY` or `NEXT_PUBLIC_POSTHOG_API_KEY`.
4. `defaults` is `'2026-05-30'`.
   - Incorrect: any other date, or leaving `defaults` out.
5. Token and host come only from the environment.
   - Incorrect: a literal `phc_` token or a literal ingestion host as a fallback.

## Use

6. Elsewhere, `import posthog from 'posthog-js'` and call methods on it. Never initialize again.
7. posthog-js is browser-only. Server components, route handlers, and middleware use `posthog-node`, never posthog-js.
