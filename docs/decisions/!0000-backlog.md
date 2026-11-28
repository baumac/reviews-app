--- 
This file is not a decision.
Instead, it is a backlog of decisions for the project that still need to be made.
Each sub-bullet point contains notes about the incomplete decision.
---

# Backlog

## For claude

## For Me
- redesign or rename table concept
  - Decision 00013 proposes a weekly digest with memories.
- next prod deploy
  - set the PocketBase Application name and mail sender name to "Olive"
  - find way to apply ip whitelisting in prod
  - deploy latest code

## For the Future
- Replace the public Photon server of Komoot and the tile server of OpenStreetMap with self-hosted options
  - `REVIEWS_PLACES_URL` and `REVIEWS_MAP_TILE_URL` already change the servers. The move needs no code change, only the servers on the home server.
  - The tile server of OpenStreetMap has no guarantee and can block heavy use. See decision 00008.
- in the UI add ability to auto-fill a new restaurant from a Google Maps link
  - The Terms of Service of Google forbid scraping Google Maps. The app must not read the address, phone, website, or opening hours from Google.
  - The Places API is legal, but its terms forbid storing its data except the `place_id`. They also forbid showing it on a map that is not from Google. See decision 00008.
  - A link holds only the name, the point, or a search text. The app can get the address and the `place_id` from Photon with them. A short `maps.app.goo.gl` link needs the server to follow its redirect.

## Won't do
- Allow directly uploading photos to restaurants or recipes: all photos must be uploaded via a review to force photos being associated to a review
- Expose operations that can be worked around by having an admin use the PocketBase dashboard
  - No email change. UserPatch has only name; a new address needs the dashboard.
  - No bulk import and no recipe import from a URL. A superuser adds old restaurants, recipes, and reviews in the dashboard or with the PocketBase API. The create limit of decision 00003 is only on the Public API.
    - Add photos only through the Public API. A photo from the dashboard keeps its metadata, such as the GPS position.
- Add IOS app: won't do for now as this requires a mac laptop and side-loading the app from that device, or a 99$ per year subscription to host the app in the app store
- Add load tests: not needed as the number of users are quite low
- Support oauth2 logins: not needed since email/password based authentication is sufficient
- Support inviting users via email
  - Note: A person can still make the sign up of an owner fail for one hour. The person signs up with the address until its email limit. The owner must then reset the password. An invite removes this.
- Social features: not needed since this is intended to be used by a low number of trusted users that communicate with each other via other means
  - Reviews have no owner or reader reply
  - Comments or replies on review
  - Helpful votes or likes on reviews
  - Following users, or a feed of followed users (the home of decision 00009 lists all reviews by date, with no follows)
  - Messages or notifications between users
  - Profile pictures or bios (the avatar field was removed)
  - Private or collaborative lists
  - A share button in the UI (sharing works by copying the page link)
  - Content moderation and a way for users to report content
    - a superuser must find it and delete it in the dashboard. Decision 00007 accepts this for photos.
