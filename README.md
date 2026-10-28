# Elite EA Marketplace

A Next.js marketplace for selling and licensing products (automation bots and similar digital goods), with a customer dashboard, an admin area, and M-Pesa and Paystack payment flows. Data and auth are handled by Supabase, and purchases generate license records that can be validated through an API route.

Status: work in progress. Payment and license code paths need end-to-end testing against sandbox credentials before going live.

## Features

- Public pages: landing, bot catalogue and bot detail pages, pricing, performance.
- Auth: login and registration with Supabase SSR sessions, with middleware guarding routes.
- Customer area: dashboard and licenses.
- Admin area: product list, create and edit product forms, orders.
- Payments: M-Pesa STK push with a callback handler, and Paystack initialise, verify and webhook routes.
- Licensing: `POST /api/licenses/validate`.
- Charts for performance data (Recharts).

## Stack

- Next.js 15 (App Router) with React 19 release candidate
- TypeScript
- Tailwind CSS 3
- Supabase (`@supabase/ssr`, `@supabase/supabase-js`)
- Framer Motion, Lucide React, Recharts, react-hook-form with Zod, Sonner, date-fns, axios

## Setup

Requires Node.js 20 or newer and npm.

    git clone https://github.com/Manu-del-source/elite-ea-marketplace.git
    cd elite-ea-marketplace
    npm install
    # create .env.local with the variables listed below
    npm run dev

Then open http://localhost:3000.

### Environment variables

Create `.env.local` with the following (values come from your Supabase project, M-Pesa Daraja portal and Paystack dashboard):

    NEXT_PUBLIC_SUPABASE_URL=
    NEXT_PUBLIC_SUPABASE_ANON_KEY=
    SUPABASE_SERVICE_ROLE_KEY=
    NEXT_PUBLIC_APP_URL=http://localhost:3000
    MPESA_CONSUMER_KEY=
    MPESA_CONSUMER_SECRET=
    MPESA_SHORTCODE=
    MPESA_PASSKEY=
    MPESA_CALLBACK_URL=
    PAYSTACK_SECRET_KEY=

Never commit `.env.local` or the service-role key.

### Database

SQL migrations live in `src/supabase/migrations/` and create the initial schema, seed products, license activations, and the M-Pesa columns. Apply them in order with the Supabase CLI or the SQL editor.

## Scripts

    npm run dev      start the dev server
    npm run build    production build
    npm run start    serve the production build
    npm run lint     run ESLint

## Structure

    src/
      app/
        (public)/       landing, bots, pricing, performance
        (customer)/     dashboard, licenses
        admin/          products, orders
        auth/           login, register
        api/            licenses/validate, payments (M-Pesa, Paystack)
        layout.tsx, globals.css
      components/       admin, charts, dashboard, landing, marketplace, ui
      lib/              supabase clients, mpesa.ts, paystack.ts, mockData.ts, utils
      middleware.ts     session and route protection
      supabase/migrations/   SQL schema and seed files
    public/             static SVG assets

## Notes

- `src/lib/mockData.ts` contains demo data used by some pages.
- Payment callbacks and webhooks need a publicly reachable URL during testing (for example a tunnel), and the M-Pesa callback URL must match what is registered with Daraja.
- Build and lint status were not verified as part of this README update.

## License

No license file is included in this repository yet.
