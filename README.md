# Apex Drop

Send a link, a snippet of text or a file from one of your devices to another, instantly.

**Status: not built.**

## What it should do

- Copy on the phone → paste on the laptop.
- Drop a file on the laptop → it appears on the phone.
- No account sharing, no third party reading it in transit.

## The constraint that shapes it

Realtime needs a socket, and the free options are limited:

- **Firebase Realtime Database:** 1 GB stored, 100 simultaneous connections. Plenty for
  your own devices.
- **Supabase Realtime:** 200 connections, 2 million messages a month, but the project
  pauses after a week of quiet — which is exactly what a drop service does when you are
  not using it.

Firebase is the better fit: it does not pause.

## The part that is actually hard

Android does not let a background app watch the system clipboard, and it should not.
Realistic options are a share-sheet target (you press Share → Apex Drop) or a small
foreground service with a notification. Files also need somewhere to sit: a short-lived
object in the B2 bucket, deleted after collection.

## What you need to do

1. Nothing new if you already made the Firebase project for `Apex-Auth`.

## First three steps

1. A web page that shows a text box and a live list, so two browsers share clipboard text.
2. A share target in the Android app that posts to the same place.
3. File drops: upload to B2, hand out a one-time link, delete on collection.
