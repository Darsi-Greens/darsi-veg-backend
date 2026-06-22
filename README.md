# darsi-veg-backend
⚙️ Firebase Cloud Functions backend for Darsi Vegetable Shop — daily WhatsApp summary reports, automated stock alerts, price history triggers and Firestore security rules.

## Security Rules

| File | Protects | Status |
|------|----------|--------|
| `firestore.rules` | All Firestore collections — field/type/range validation | **Active** |
| `storage.rules` | Storage bucket — `receipts/{orderId}/*` (10 MB cap), `vegetables/{vegId}/*` (5 MB cap) | **Dormant** — see note |

Both use the shared-PIN model (no per-user auth yet) and validate by
path + content rather than `request.auth`.

> **Note — Storage is not in use.** Firebase Storage requires the Blaze
> (billing-card) plan, which this project does not have. **Order receipts are
> uploaded to Cloudinary's free tier instead** (see the app repo's
> `OrdersScreen.js` + `EXPO_PUBLIC_CLOUDINARY_*` env vars). `storage.rules` /
> `firebase.json` are kept only for the future case where Blaze is enabled for
> the planned vegetable-photo feature — do not rely on them today.

### Deploy

`firebase.json` wires both rule files. When the Firebase CLI is available:

```bash
# Firestore rules only (Storage skipped — no Blaze plan)
firebase deploy --only firestore:rules --project darsi-veg-staging
firebase deploy --only firestore:rules --project darsi-veg-shop
```

If the CLI is unavailable, paste `firestore.rules` into the Firebase Console
(Firestore → Rules) for the target project.
