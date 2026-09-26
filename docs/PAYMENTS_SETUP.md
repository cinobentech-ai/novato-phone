# Novato Payments + Plans — Setup (Production)

**Project:** `buzmxatbmiqtiysmwcya`  
**Do not** run this on other Supabase projects.

## 1. SQL (required first)

1. Open Supabase → project **buzmxatbmiqtiysmwcya** → SQL Editor  
2. Paste and run the migration from the `novato` repo:  
   `supabase/migrations/20260926_payments_and_plans.sql`  
3. Confirm tables: `payment_requests`, `payment_attempts`, `subscription_plans`, `organization_subscriptions`

## 2. Paystack (test mode)

1. https://dashboard.paystack.com → **Test mode**  
2. Copy **Secret Key** (`sk_test_...`)  
3. Webhook URL:  
   `https://buzmxatbmiqtiysmwcya.supabase.co/functions/v1/paystack-webhook`  
4. Events: `charge.success`, `charge.failed`

## 3. Edge function secrets

| Name | Value |
|------|--------|
| `PAYSTACK_SECRET_KEY` | `sk_test_...` |
| `PAYSTACK_CALLBACK_URL` | `https://chimerical-lollipop-eddfad.netlify.app/` |

## 4. Deploy functions

- `paystack-initiate` — JWT **required**  
- `paystack-webhook` — JWT **not** required  

Source files are under `supabase/functions/` in this repo.

## 5. Packages

| Plan | Notes |
|------|--------|
| Starter | Free, up to 50 members |
| Growth | ₦15,000/mo, up to 200 members |
| Enterprise | Custom price/notes for large orgs |

## 6. Test card

Success: `4084084084084081` (Paystack test)
