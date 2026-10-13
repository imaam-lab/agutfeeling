# AGUTFEELING website

A three-page static clothing website built with **HTML5 and CSS3 only**. There is no JavaScript anywhere, no framework and no build step.

## Files

```
agutfeeling/
├── index.html          Home page
├── product.html        Products page (catalogue + product pop-ups)
├── order.html          Order page (WhatsApp order message)
├── style.css           Shared styling for all three pages
├── favicon.ico         Favicon (the "a" of the logo)
├── README.md           This file
└── assets/
    ├── logo.png                 Logo, transparent background
    ├── favicon.png              Favicon, 64 px
    ├── favicon-512.png          Favicon, 512 px
    ├── apple-touch-icon.png     Home-screen icon
    ├── product-01.jpg           Product 1 photo (full size)
    ├── product-01-640.jpg       Product 1 photo (small, for cards)
    ├── product-02.jpg           Product 2 photo (full size)
    └── product-02-640.jpg       Product 2 photo (small, for cards)
    ├── product-03.jpeg           Product 3 photo (full size)
    └── product-03-640.jpeg       Product 3 photo (small, for cards)
```

## Your logo and photos

They are already in `assets/`. To replace one, save the new file under the **same name** (keep the logo a transparent PNG).

If you replace a product photo with one of a different shape, also update its `width` and `height` attributes in the HTML so the page does not shift while loading.

## What pure HTML + CSS can and cannot do

Everything below works without JavaScript, using modern browser features:

| Feature | How it works |
|---|---|
| Mobile menu | Native HTML `popover` (Escape and outside-click close it) |
| Product details pop-up | Native HTML `popover` (Escape, outside click and the close button close it; background scroll is locked) |
| Category filter and live product count | Hidden radio buttons + CSS `:has()` and CSS counters |
| Order page opens with the right product | The link `order.html#order-product-01` is matched with CSS `:target` |
| WhatsApp redirect | A normal HTML form sent with `GET` to `https://wa.me/94764013480` |
| Scroll reveal, parallax, page transitions | CSS scroll-driven animations and view transitions (these are bonuses: browsers that lack them show the same content without the effect) |
| Custom cursor | CSS cursor images (static; a trailing cursor needs JavaScript) |

What is **not possible** without JavaScript, and what was done instead:

- **Text search box.** CSS cannot read what a visitor types. The page has a working category filter and a live count instead. Real text search needs JavaScript or a store platform.
- **Several separate order fields combined into one message.** Plain HTML can only send one field to WhatsApp, so the order page gives the customer one pre-filled message to complete (see below).
- **Field-by-field validation and colour/size dropdowns.** The message box is validated for being empty or too short. Colour and size are written in the message because no confirmed options exist yet.
- **Automatic copyright year.** The footer says 2026 as plain text. Edit the footer line in each HTML file when the year changes.

Browser support: a recent browser is needed (Chrome / Edge 114+, Safari 17+, Firefox 125+). In much older browsers the menu button and product pop-ups will not open.

## Editing products

**No prices, sizes, stock levels or colour options were supplied, so none are shown.** Pages say "Contact us for price", "Contact us to confirm availability" and "Contact us to confirm available sizes".

Each product exists in a few places. Search for its ID (for example `product-01`) and update every match:

| Page | What to edit |
|---|---|
| `index.html` | The `<article class="feature">` in "Featured pieces": name, note |
| `product.html` | `PRODUCT CARD` (name, price, availability) and `PRODUCT DETAILS POP-UP` (name, price, description, colour, sizes, availability) |
| `order.html` | The product in the left-hand list, and the `ORDER PANEL` (name, price, and the message template inside the `<textarea>`) |

To show a real price, replace the text `Contact us for price` with, for example, `Rs. 8,500`, in every place for that product. In the order message template also change `Price: Please confirm` to the same amount.

### Adding a new product

1. Put the photos in `assets/` (for example `product-03.jpg` and a 640 px wide `product-03-640.jpg`).
2. In `product.html`, copy a whole `PRODUCT CARD` block and its `PRODUCT DETAILS POP-UP` block, then change the ID (`product-03`) everywhere in the copy, including `popovertarget`, `id`, `aria-labelledby` and the `order.html#order-product-03` link.
3. In `order.html`, copy one list item of the product picker and one `ORDER PANEL`, and change the ID the same way (`order-product-03`, `msg-product-03`, `hint-…`, `err-…`).
4. Optionally add a featured card in `index.html`.
5. If it belongs to a new category, in `product.html` copy the radio button and the filter chip for Shirts, then in `style.css` copy every rule that mentions `#f-shirts` (the selected-chip rules and the hide rule) and rename it to the new category.

## How the WhatsApp order works

1. The customer picks a product (or arrives from a product pop-up with it already selected).
2. The order page shows a message with the product name and ID filled in, and bracketed gaps such as `[your full name]`.
3. The customer completes it and presses **Send order on WhatsApp**.
4. The browser opens `https://wa.me/94764013480?text=…` (your number, with the message safely encoded) and WhatsApp shows the chat with the message ready.
5. The order is only real once the customer presses send in WhatsApp and you confirm it. The website never claims an order was placed.

Delivery fees, timing, availability and payment are agreed in the WhatsApp chat. None are promised on the site.

## Testing locally

Open a terminal in the project folder and run:

```
python3 -m http.server 8000
```

Then visit <http://localhost:8000>. (Double-clicking `index.html` also works for most things.) The Google Fonts and WhatsApp links need an internet connection.

Quick manual check list:

- Menu button opens and closes on a phone-width window; Escape closes it.
- On Products, tap a card: the pop-up opens, Escape / outside click / the X close it, the page behind does not scroll.
- In the pop-up, **Place Order** opens `order.html` with that product's panel showing.
- On Order, press **Send order on WhatsApp**: WhatsApp (web or app) opens with the full message and number `94764013480`.
- Try Jackets / Shirts filters and watch the count change.

## Putting it online

It is plain static files, so any static host works. Upload the whole folder (keep the structure):

- **Netlify Drop**: drag the folder onto app.netlify.com/drop.
- **Cloudflare Pages / GitHub Pages / Vercel**: connect a repository containing these files; no build command, publish directory is the root.
- **Any web host**: upload the folder contents to the site's public folder.

After you have a domain, you can add `og:image` and canonical tags to each page head for nicer link previews.

## Limits of a website without a backend

Orders are not stored, stock is not tracked, and payments are not taken or confirmed by the website. Everything goes through WhatsApp.
