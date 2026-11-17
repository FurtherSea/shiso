# 食序 Shiso website

The website of Shiso (食序 in Chinese), an app for iPhone: home, privacy policy and support, in English and Chinese. It's plain HTML, CSS and a little JavaScript with no build step. GitHub Pages serves the files as they are from the `main` branch.

| | English | Chinese |
| --- | --- | --- |
| Home | `index.html` | `zh/index.html` |
| Privacy policy | `privacy/index.html` | `zh/privacy/index.html` |
| Support | `support/index.html` | `zh/support/index.html` |
| A shared dish | `r/index.html`, both languages | |

`assets/site.css` holds the styles. Its colors, type and components come from the app (Design Direction in the app repo's `agents.md`). `assets/site.js` remembers the language and the light or dark choice, puts the contact address together (see Contact below), runs the plate palette on the home page (every plate's place comes from one number, so moving, swiping and spinning all glide along the arc), and moves the features' rows of screens. English is the default: a visitor who picks Chinese is sent to the Chinese page from then on, and the choice stays in that browser.

To preview, serve the folder with any static server, for example `python3 -m http.server`.

## A shared dish

`r/` is the page a dish card's QR code opens where Shiso isn't installed: `https://furthersea.org/shiso/r/?k=<key>`, the key being 16 letters and digits. Where Shiso is installed, iOS opens the app instead, because `.well-known/apple-app-site-association` in [`FurtherSea/furthersea.github.io`](https://github.com/FurtherSea/furthersea.github.io) claims `/shiso/r/` for it.

- **It only offers the app.** It shows the brand, the line for a shared dish (记在食序里的一道菜 / A dish kept in Shiso) and the App Store button. It doesn't read the dish: CloudKit's web service needs an API token in the page, and the owner chose on 2026-11-16 not to put one there. The dish comes into the app once it's installed.
- **Safari's Smart App Banner.** A script in the head writes `apple-itunes-app` with the dish's link as `app-argument`, so once Shiso is installed from the banner, the banner's Open hands the link to the app. The line under the button that says so shows only in iOS Safari.
- **WeChat** opens links in its own browser, where neither the banner nor the app's link works. There a dimmed layer points to ··· and 在浏览器打开, and a tap puts it away. `?wechat=1` shows it anywhere, for review.
- **Language** follows the browser: Chinese or English.
- The page has `noindex`, keeps nothing in the browser and follows the light or dark choice made on the other pages, with no button of its own.

## Names

The app is `食序` in Chinese and `Shiso` in English, with nothing after the name: the page header, the home page's `<title>`, `og:site_name` and `og:title`, the link preview image and the privacy policy's first line all say just `Shiso`, and the other pages' titles pair it with the page, as in `Support · Shiso`. Chinese pages say `食序` everywhere; only the privacy policy's first line and its note on the name change name the English too. The slogans are `Cooking that grows with you` and `陪你记录的个性化食谱书`.

The English name was `Mise` until November 8, 2026. The `mise.lang` and `mise.theme` keys in the browser's storage keep that name, so saved choices still work.

## The first screen

The slogan stays put as the page's `h1`, set small. Under it a big title and a line take turns, one turn every four seconds. A turn is one `.hero-title`, one `.lede` and one dot, matched by `data-turn` and `data-pick`; to add one, add all three in both languages. The theme green marks the part of the title to remember, the second line as a rule (`em`). The three turns are the three features below, in their order.

## The features

The home page shows three features, in this order of importance: the recipe library (`#library`), dish photos (`#photos`) and sharing (`#share`). As on the first site, each is a row with the text on one side and the app on the other, the sides taking turns (`.feat`, `.feat.flip`).

- **The text:** an eyebrow, a heading and a line, then a note for each screen (`.notes`).
- **The screens:** cards of the app's real 2.1 screens, in a row that swipes on its own while the text stays put. The card in the middle is bright, and its neighbours peek in at the sides.
- **Moving the row:** the note and the dot for the middle card are marked. Picking a note, a dot or an arrow brings its card to the middle (`createShots` in `assets/site.js`).
- **On phones** the row runs edge to edge and is swiped, with the dots under it.

The cards are `assets/shots/<en|zh>/<slot>.webp`, 800 × 1040 at quality 84, cut from the app's 2.1 screenshots (iPhone 17 Pro Max, 1320 × 2868). Each shows a 1320 × 1716 part of its screen, the part the card is about, scaled down. On 拍一道菜 the keyboard was replaced with the sheet's blank background, as with it dismissed. The captures come from the app repo's screenshot pipeline (`TEST_RUNNER_SITE_SCREENSHOTS=en,zh-Hans`).

| Slot | Shows | Top of the crop |
| --- | --- | --- |
| `library-recipes` | 食谱库, the dishes on their plates | 144 |
| `library-kitchen` | 厨房, ingredients with their units and stores | 144 |
| `library-recipe` | 编辑食谱, the ingredients and the steps with their photos | 144 |
| `library-plan` | 餐食, the days ahead and their meals | 144 |
| `library-shopping` | 采购, the list grouped by store | 144 |
| `photos-camera` | The plate camera | 560 |
| `photos-album` | 成品照, a dish's photos over time | 1152 |
| `photos-snap` | 拍一道菜, then 是哪道菜？ or a new dish | 190 |
| `share-lyric` | 分享这道菜 with the lyric card and its QR code | 216 |
| `share-cards` | The numbered cards a long lyric card runs on over | 216 |
| `share-import` | 导入这道菜, a shared dish opening in the app | 192 |

## Changing text

Change both languages. The Chinese font is a subset, so after adding Chinese text run this, pointing at the fonts in the app repo:

```bash
python3 -I tools/subset_fonts.py <mise-app>/mise/Resources/Fonts
```

It needs `fonttools` and `brotli`, and it lists any character the source font lacks.

## The privacy policy

The policy states facts about the app, so keep it true. Before a version that sends new data, or adds ads, accounts or online features, ships, update `privacy/` and `zh/privacy/`, the effective date on both, and the App Store privacy label. What the app sends to Google Analytics is listed in `docs/usage-analytics.md` in the app repo; the policy names each action, so a new event changes the policy too.

## Fonts

- Chinese: Resource Han Rounded, subset in `assets/fonts`, SIL OFL 1.1 (`OFL-ResourceHanRounded.txt`).
- Latin and numbers: the system's rounded font through `ui-rounded` where there is one, otherwise Nunito, SIL OFL 1.1 (`OFL-Nunito.txt`). SF Pro Rounded is never hosted here.

## App Store badge

The files in `assets/badges` are Apple's own artwork, unmodified, from the [App Store Marketing Tools](https://toolbox.marketingtools.apple.com/app-store/). Apple's [guidelines](https://developer.apple.com/app-store/marketing/guidelines/) allow one badge per layout, at least 40 px high, with clear space of a quarter of its height, and ask for the trademark line that's in the home pages' footers.

## The address

The site lives at https://furthersea.org/shiso/. The path is this repository's name: the domain is bound in [`FurtherSea/furthersea.github.io`](https://github.com/FurtherSea/furthersea.github.io), the organization's own site, and GitHub Pages serves every other repository's site under that domain. So this repository has no `CNAME` file and no custom domain of its own. DNS is at Cloudflare in DNS-only mode so GitHub can issue the certificate. `www.furthersea.org/shiso/` and `furthersea.github.io/shiso/` redirect here.

Pages link to each other with relative paths, so they work at any address. A new address, which includes renaming this repository, needs changes in these places: the `canonical`, `alternate` and `og:` tags in each page's head, and the root-relative paths in `404.html`.

The site was at `/mise-homepage/` until November 2026, and copies of the app already installed open their privacy and support pages there. So `404.html` in `FurtherSea/furthersea.github.io`, which GitHub Pages serves for any address no repository covers, sends every address under `/mise-homepage/` to the same page under `/shiso/`. Keep that redirect.

## Contact

Email comes first: the support page and the privacy policy's Contact section have `Email us` and `Show address`. The address is never in the HTML in one piece, so a scraper reading the source finds no `name@domain` address and no `mailto:` link. Each `.mail` block holds it backwards in `data-mail`, with `/` in place of `@`, and `assets/site.js` puts it together only for someone who reaches for it: `Email us` gets its `mailto:` link as the pointer or the focus comes to it, and `Show address` puts the address on the page with a `Copy` button. Without JavaScript the block is hidden and the page spells the address out with at and dot. To change the address, write the new one backwards with `/` for `@` in each `data-mail`, and change the spelled-out line under it.

This repository's issues stay as the public alternative: those two pages and the footers link to them. Keep them turned on.
