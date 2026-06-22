# darsi-veg-backend
⚙️ Firebase Cloud Functions backend for Darsi Vegetable Shop — daily WhatsApp summary reports, automated stock alerts, price history triggers and Firestore security rules.

## Security Rules

| File | Protects |
|------|----------|
| `firestore.rules` | All Firestore collections — field/type/range validation |
| `storage.rules` | Storage bucket — `receipts/{orderId}/*` (10 MB image cap), `vegetables/{vegId}/*` (5 MB image cap); all other paths denied |

Both use the shared-PIN model (no per-user auth yet) and validate by
path + content rather than `request.auth`.

### Deploy

`firebase.json` wires both rule files. When the Firebase CLI is available:

```bash
firebase deploy --only firestore:rules,storage --project darsi-veg-staging
firebase deploy --only firestore:rules,storage --project darsi-veg-shop
```

If the CLI is unavailable, paste the contents of each `.rules` file into the
Firebase Console (Firestore → Rules, Storage → Rules) for the target project.
