# Hospital PM Management System v3 + WhatsApp Bot

This ZIP removes the Android/APK module and keeps the scope Preventive Maintenance only.

## Included
Web PM system, Supabase PostgreSQL, Auth foundation, equipment CRUD, PM1/PM2/PM3 scheduling, due/due-soon/due-today/overdue status, PM checklist completion/history, Excel export, WhatsApp interactive bot, search by inventory/equipment name/serial, PM Due/Completed/Overdue, Machine Details, Department/Location/Equipment Type/PM Status filters, notification log, automatic due/overdue sender, completed sender.

## Setup
1. Install Node.js LTS, Git, VS Code and Supabase CLI.
2. Create a Supabase project.
3. Run `supabase/schema.sql` in Supabase SQL Editor.
4. Create an Auth user in Supabase Authentication > Users.
5. Copy its UUID into `supabase/setup_first_admin.sql`, then run that SQL.
6. Copy `.env.example` to `.env.local` and fill the three NEXT_PUBLIC values.
7. Run `npm install`.
8. Run `npm run dev`.
9. Open http://localhost:3000 and sign in.
10. Add departments, equipment and PM schedules.

## WhatsApp
Configure an official Meta WhatsApp Business Cloud API application. Keep access tokens only in Edge Function secrets.

Set:
`supabase secrets set WHATSAPP_ACCESS_TOKEN="..." WHATSAPP_PHONE_NUMBER_ID="..." WHATSAPP_VERIFY_TOKEN="..." WHATSAPP_GRAPH_VERSION="v23.0"`

Deploy:
`supabase functions deploy whatsapp-bot`
`supabase functions deploy pm-notifications`
`supabase functions deploy pm-completed-notification`

Webhook:
`https://YOUR_PROJECT_ID.supabase.co/functions/v1/whatsapp-bot`

Use the same verify token in Meta. For proactive business-initiated messages, use approved WhatsApp templates when required by Meta's current rules.

## Recipients
Example SQL:
`insert into public.whatsapp_recipients(name,phone,recipient_type) values ('PM Admin','COUNTRYCODEPHONENUMBER','PM_ADMIN'),('Biomedical','COUNTRYCODEPHONENUMBER','BIOMED'),('Engineer','COUNTRYCODEPHONENUMBER','ENGINEER');`

## Automatic notifications
Schedule `pm-notifications` once per day with a trusted scheduler. It checks 30/14/7/3/1 days before due, due today, and overdue. It logs sends and avoids duplicate same-day sends.

## Completed notifications
Call `pm-completed-notification` after a successful PM completion transaction. In production, use a server-side/database webhook and authenticate the invocation.

## Compliance
PM Compliance = Completed PM tasks / Due PM tasks × 100. Due means active PM schedules with due date <= today.

## Production hardening
Before hospital production use, add Meta webhook signature validation, approved template sender, delivery-status webhook, retry UI, Excel staging/preview/mapping, PDF reports, calendar/charts, admin settings UI, user-management UI, true signature capture, backups/restore testing and UAT.

## Not included
Android APK, downtime, breakdown, corrective maintenance, service tickets.
