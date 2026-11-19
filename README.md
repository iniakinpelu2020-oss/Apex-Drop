# Apex Drop

Send a link, a snippet of text or a small file from one of your devices to another.

**Status: step 1 is built.** This repo is now the web page the plan asked for first: two
browsers with the same room code and passphrase share text and small files in a live list.

## What works

- **Room code + passphrase.** Both are combined with PBKDF2-SHA256 (100 000 iterations) into
  an AES-GCM-256 key. Encryption happens in the browser before anything is sent, so the
  database only ever holds ciphertext. The passphrase is never transmitted.
- **Text drops** with Copy, and **file drops** up to 600 KB with Download.
- **Live list**, newest first, kept to the last 40 drops and cleared after 24 hours.
- **Same-browser tabs** sync instantly over BroadcastChannel, even if the network is down.
- **Cross-device sync** through Firestore REST every 2.5 seconds (no SDK, no build step).
- Installable PWA with an offline shell.

## Run it

It is plain static files. Any static host works:

```sh
python3 -m http.server 8080
```

Open the page on both devices, type the same room code and passphrase, press **Connect**.

## One-time setup: Firestore rules

Writes to the `drops` collection need permission. In the Firebase console for
`apexaiautomation` → Firestore → Rules, add the block from [`firestore.rules`](firestore.rules)
and publish. Until you do, the page says the database refused the write.

## Limits worth knowing

- **600 KB per file** because a Firestore document caps at 1 MiB and base64 inflates by a third.
- **Polling, not a socket.** 2.5 seconds is effectively instant for moving a link; it is not
  a realtime database. The README's Firebase Realtime Database route is still the upgrade if
  you want true push, and it would remove the polling interval.
- **No account.** The room code is the only address. Anyone who guesses both the room code and
  the passphrase can read the room, so use a passphrase you do not reuse.
- **Files are not copied to B2.** They live in the encrypted room document until they age out.
  The one-time-link-plus-delete flow from the plan is not built.

## Next steps, in order

1. Swap polling for Firebase Realtime Database so drops appear without the delay.
2. Android share-sheet target that posts into the same room document.
3. File drops over B2 with a one-time link and deletion on collection.
