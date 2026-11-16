# Progress — Mayra's Handi Home website

Last updated: 2026-11-16

## Status

**Live:** https://mayras-handi-home.vercel.app (Vercel, production)

| Area | State |
| --- | --- |
| One-page site (hero, about, menu/services, reviews, hours, location, footer) | Done |
| Hero video (seamless loop, 1080p, poster frame) | Done |
| Layered scroll parallax + reduced-motion support | Done |
| Mobile layout, tap-to-call, Google Maps directions | Done |
| SEO: meta, canonical, OG/Twitter image, JSON-LD, sitemap, robots | Done — all point at Vercel |
| Deployment | Vercel (primary). Old Higgsfield copy redirects here |

## Business facts used (from the Google Maps listing)

- Name: Mayra's Handi Home — Restaurant
- Address: Street No. 7, Narain Nagar, Guru Nanak Colony, Faridkot, Punjab 151203 (plus code MPMW+35)
- Phone: +91 96393 00042
- Hours: opens 11 a.m. (closing time unknown, so the site asks visitors to call)
- Price: ₹200–400 per person
- Services: dine-in, takeout, no-contact delivery
- Menu: food-order.in (exact menu page unknown; links to the site's homepage)
- Rating: 3.9 from 153 Google reviews
- Reviews quoted verbatim: "Wonderful place..superb food quality and amazing atmosphere..", "Good quality food & service", "Good ambience" (techiedoc, Local Guide)

Nothing else was invented: no prices per dish, no dish list, no awards or history. The food photos are AI-generated illustrations, not photos of the restaurant's food.

## Design

- **Identity:** "copper and ember" colours — charcoal `#16110d`, copper `#c26d3c`, saffron `#e8b25a`, cream `#f4e9d8`. Logo/favicon: a handi pot with three steam curls (`site/favicon.svg`).
- **Type:** Fraunces (headings, italic accents) + Manrope (body), from Google Fonts, loaded without blocking rendering.
- **Motion:** hero has three parallax depths (video, spice drawings, headline); section images and spices drift at their own speeds; scroll-in reveals. All transform-based, and switched off for users who prefer reduced motion.

## Media pipeline

1. Hero start frame and section photos generated on Higgsfield with Z-Image.
2. Hero video generated on Higgsfield with Wan 3.0 at 720p, using the same image as first and last frame.
3. Local ffmpeg: crossfaded the last second into the first for a seamless 4-second loop (join difference ≈ 1.6, same as neighbouring frames), upscaled to 1920×1080 with Lanczos and light sharpening.
4. Encoded WebM (VP9) + MP4 (H.264) at 1920 and 1280 widths; poster frames in WebP. Mobile loads the 1280 version.
5. All media lives in `site/media` and `site/assets`; nothing loads from Higgsfield servers.

Limits hit: the Higgsfield account is on the free plan (≈0.8 credits left), so Kling video and the Topaz/ByteDance upscalers were not available.

## Verification (live Vercel site)

- All pages and media return 200; media cached for 1 year (`site/vercel.json`).
- Video autoplays muted, loops, and has a poster; desktop gets 1080p, mobile 720p.
- Parallax moves on scroll on desktop and mobile; with reduced motion the video is replaced by the poster and nothing moves.
- No console errors, no sideways scroll on mobile.
- Lighthouse (mobile): Performance 98, Accessibility 100, Best Practices 100, SEO 100; LCP 1.9 s.

## Timeline

1. Built the static site in `site/`, generated imagery and hero video.
2. Deployed to Higgsfield (`mayras-handi-home.higgsfield.app`); it had to be published to the Higgsfield community feed to become public.
3. Performance pass: hero headline no longer waits for the scroll-reveal, fonts made non-blocking, footer contrast fixed, mobile call bar moved above the platform badge. Lighthouse went from 80 to 100 (mobile performance).
4. Audit for external media URLs: none found; every image and video was already local.
5. Deployed to Vercel (`mayras-handi-home.vercel.app`) with cache headers.
6. Switched canonical, OG, Twitter, JSON-LD, sitemap and robots URLs to Vercel; confirmed no Higgsfield text, keys, env files or SDK packages remain in this repo.
7. Higgsfield has no tool to unpublish a website, so the Higgsfield copy was replaced with a `noindex` redirect to Vercel and its media removed.

## Open items

- [ ] Remove the Higgsfield community-feed listing by hand: https://higgsfield.ai/supercomputer/apps/bb457513-0d6c-433c-94b1-9424a0573ffa/view (no tool exists for it).
- [ ] Optionally delete the Higgsfield website entirely from the Higgsfield dashboard.
- [ ] Confirm the exact menu URL on food-order.in and the closing time, then update the site.
- [ ] Optional: self-host the two fonts so the site loads nothing from other servers.
- [ ] Optional: custom domain on Vercel; if added, update canonical/OG/sitemap/robots URLs.
- [ ] Remove the Vercel token from this cloud environment's settings when deploys from here are no longer needed; consider enabling 2FA on the Vercel account.

## How to deploy

```bash
cd site
npx vercel deploy --prod --yes   # needs a Vercel token or `vercel login`
```

The site is plain static files; there is no build step.
