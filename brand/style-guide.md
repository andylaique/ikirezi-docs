# Ikirezi

**Ikirezi** is Kinyarwanda for *a precious pearl*. It is a sexual and reproductive health (SRH) education, wellbeing and opportunity platform for girls and young women, built by TeleMedAfrica. The products are a Flutter app for girls, a Next.js admin console and a Next.js professionals portal. All three share this system.

Tagline: **Know your body. Own your future.** · *Menya umubiri wawe. Tegura ejo hawe.*

## The idea behind the brand

A pearl grows slowly and in private before it is shown. Ikirezi treats each girl the same way. She learns at her own pace, shares anonymously, gets checked advice, and is reminded that she has value. The mark puts the pearl in a pair of open hands. The left petal is plum, for knowledge and dignity. The right petal is coral, for warmth and action. A gold ring marks the pearl as worth celebrating.

## Voice and content

- **Warm, factual, never shaming.** Say "Your period is late — here's what can cause that", not "Are you pregnant?". The AI never diagnoses and never confirms a pregnancy.
- **Every health fact carries its source.** Tips and AI answers end with tappable citations (WHO, UNFPA, UNICEF, CDC, NHS, Rwanda Biomedical Centre). If no source supports an answer, the app says so and offers a professional.
- **Bilingual by default.** English and Kinyarwanda strings are written together, never machine-translated after the fact. The language toggle sits on onboarding, in Settings and on the login screen.
- **Anonymity is visible.** Stories show "Shared anonymously", never an alias or avatar. Professionals see an alias, avatar and age band only.
- **Short sentences, sentence case.** Buttons say what happens: "Request a professional", "Mark my calendar", "Submit for review".

## Visual foundations

**Colour.** Plum (`plum`) carries the brand. Use it for headings, primary buttons and the active nav. Coral (`coral`) is for action and warmth: the SOS button, highlights and one hero accent per screen. Put `ink` text on coral, or white only at 24px and above. Teal (`teal`, `teal-deep`) means care and health: the cycle tracker, verified professionals and success states. Gold (`gold`) is for celebration only: stars, women's-day banners and the pearl ring. Pages sit on `pearl`, cards on `card`, and quiet tints use `lilac`, `coral-soft`, `teal-soft` and `gold-soft`. `alert` is reserved for errors, strikes and destructive confirmations.

**Type.** *Fraunces* (`display`) sets screen titles, card headlines and big numbers. It is a soft, editorial serif that feels like a diary rather than a clinic. *Plus Jakarta Sans* (`sans`) sets everything else. Keep display text to one or two lines per block. Uppercase `eyebrow` labels get 0.08em tracking.

**Shape.** Rounded and soft. Cards use `radius-lg`, buttons and inputs use `radius-md` on every surface. Badges, chips and avatars are pills. Cards get one `shadow-card` lift and a `line` border. Never stack shadows.

**Spacing.** 4px base. Mobile screens use a 16px gutter (`space-4`) and web pages 24–32px. Separate sibling groups with gaps, not margins.

**Imagery.** No photographs of real people, ever. People appear as illustrated avatars drawn in code: diverse skin tones and hair (afro puff, braids, headwrap, bun) on soft tinted discs. Professionals get a teal disc and a stethoscope arc.

**Motion.** Gentle. Use 200–300ms ease-out for sheets and cards. Confetti appears only when a weekly challenge is completed. Respect reduced-motion settings.

**Type scale** (identical in Flutter `AppTheme`, Tailwind and these tokens): `display-xl` 40/44, `display` 32/37, `headline` 28/34 and `title` 22/28 in Fraunces 600. Then `heading` 17/24 bold, `body` 15/23, `label` 13/18 semibold, `caption` 12/16 and `eyebrow` 11/14 bold uppercase in Plus Jakarta Sans.

## Buttons

Labels start with a verb and say exactly what happens ("Mark my calendar", "Submit for review", "Request a professional"). Use one primary button per view.

| Variant | Fill / text | Use |
| --- | --- | --- |
| Primary | `plum` / white | The main action on a screen |
| Secondary | `card` with `line` border / `plum` | The alternative action (Log in, Cancel) |
| Accent | `coral` / `ink` | One floating or hero CTA (Share my story, Read posts) |
| Care | `teal-deep` / white | Health and success actions (Start session, Done) |
| Danger | `alert` / white | Only inside a confirmation (Log out, Delete account) |
| Text link | none / `coral-text` bold | "See all", "Resend code" |
| Icon button | `card` + `line`, 44×44, radius 14 | Back, close, notifications; always has an aria-label |
| SOS | `alert` / white, 44px tall | In the Home header; opens Emergency |

Sizes: `button-lg` 54px (bottom of a mobile screen), `button-md` 44px (default and the minimum touch target), `button-sm` 36px (inline row actions). Every button uses `radius-md` (16px). Small buttons use `radius-sm`.

States: pressed darkens the fill (plum becomes #3A1449). Focus adds a 2px `orchid` ring offset by 2px. Disabled drops to 40% opacity. Loading replaces the icon with a spinner and changes the label to "-ing…" ("Verifying…"). Destructive actions always open a confirmation sheet or dialog first. This includes logout on every surface.

## Iconography

Icons are rounded line icons on a 24px grid, with a 2px stroke and round caps and joins, outline only. The filled version marks the selected state only: the active tab, a liked heart, a rated star. Web portals and designs use **Lucide**. Flutter uses **Material Symbols Rounded**, mapped one-to-one so each concept keeps the same glyph:

| Concept | Lucide | Flutter |
| --- | --- | --- |
| Home / Learn / Stories | house / book / message-square | home_rounded / menu_book_rounded / forum_rounded |
| Care / Grow / Cycle | heart / sprout / droplet | favorite_rounded / spa_rounded / water_drop_rounded |
| Professional / Calendar | stethoscope / calendar | medical_services_rounded / event_rounded |
| Call / Privacy / Log out | phone / shield-check / log-out | call_rounded / verified_user_rounded / logout_rounded |

Sizes: `icon-sm` 16px inline, `icon-md` 20px in buttons and nav, `icon-lg` 24px in feature tiles. Each feature tile is 40–52px with radius 12–14, a soft tint ground and the deep tone for the icon:

| Area | Ground | Icon |
| --- | --- | --- |
| Learn | `lilac` | `orchid` |
| Stories | `coral-soft` | `coral-text` |
| Care | `teal-soft` | `teal-deep` |
| Grow | `gold-soft` | #8A5A00 |
| Emergency | `alert` | white |

Icons are never decorative emoji.

## Components

- **Inputs:** 50px tall, `radius-md`, 1.5px `line` border. Focus is a 2px `orchid` border. An error state uses a 2px `alert` border with the message underneath. The email OTP uses six 46×54 boxes, and a `coral` border marks the next digit.
- **Chips and status pills:** pill radius. The selected chip is `plum` with white text. Status pills use the tint and text pairs: Published (teal), Pending (gold), In session (coral), AI rejected (alert), Contributor (lilac).
- **Cards:** `card` ground, `line` border, `radius-lg`, one `shadow-card`.
- **Bottom nav:** 5 tabs. The active tab has a `lilac` pill behind its icon and a bold `plum` label.

## Reference images

The **Guides** asset group holds the full style guide sheet (`ikirezi-style-guide.png`) and every designed screen (`ikirezi-all-screens.png`).

## Logos

See the **Logos** group. `ikirezi-mark.svg` is the primary mark. `ikirezi-wordmark.svg` puts the mark beside the lowercase "ikirezi" set in Fraunces SemiBold. `ikirezi-mark-reverse.svg` is the app icon on a plum tile. `ikirezi-mark-mono.svg` is for one-colour print. Keep clear space equal to the pearl's radius around the mark. Never recolour the petals or place the full-colour mark on coral.

## Product surfaces

| Surface | Ground | Accent | Notes |
| --- | --- | --- | --- |
| Girls' app (Flutter) | `pearl` | `plum` + `coral` | Bottom nav: Home, Learn, Stories, Care, Grow; floating SOS |
| Admin console (Next.js) | `pearl` | `plum` sidebar | Data tables, filters, CSV export, audit history |
| Professionals portal (Next.js) | `pearl` | teal → plum sidebar | Today's members (max 2/day), session notes, posts |
