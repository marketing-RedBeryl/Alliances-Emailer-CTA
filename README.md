# RedBeryl Partner Communications Platform

This static web application serves as the exclusive portal for RedBeryl Red and Black members to view their monthly partner privileges and initiate WhatsApp concierge bookings.

## 📅 How to Add a New Month & Partners

Adding a new month involves duplicating the structure from the previous month and updating the content.

### Step 1: Create the Folder Structure
1. In the `red` directory, create a new folder for the month (e.g., `may-2026`).
2. In the `black` directory, create a corresponding folder (e.g., `may-2026`).
3. Copy `index.html` from `red/april-2026/` into your new `red/may-2026/` folder.
4. Copy `index.html` from `black/april-2026/` into your new `black/may-2026/` folder.

### Step 2: Add Partner Images
1. Save the new partner images in `16:9` aspect ratio.
2. Place these images inside the `assets/images/` folder (e.g., `new-restaurant.png`).

### Step 3: Update the Partner Accordion Content
Open the new `index.html` you just copied into `may-2026/`. Find the `<article class="partner-card">` HTML block. To add a new partner, simply copy an existing `<article>` block and paste it inside the `<section class="partners-list">`. 

Make sure to update the following for each new partner:
- **IDs**: Change `id="partner-soka"`, `aria-controls="body-soka"`, and `id="body-soka"` to a unique name indicating the venue.
- **Name & Meta**: Update the `<h2>` text, the location badge, and the offer count badge.
- **Image**: Update the `<img src="../../assets/images/...">` path.
- **Description & Offers**: Update the paragraph text and the `<li>` list items.
- **WhatsApp Message**: Update the `data-message="..."` attribute on the `<a>` tag with the new venue name (this is the pre-filled message sent to the Concierge).

### Step 4: Update the Month Listing Page
Finally, link the new month so users can find it.
1. Open up `red/index.html`.
2. Find the `<section class="month-list">`.
3. Copy the existing `<a class="month-card">` block and paste a new one above it.
4. Update the `href="./may-2026/"`, the month name text, and the offer count.
5. Repeat this process for `black/index.html`.

## 🚀 Deep-linking to Sections

You can share a URL that automatically opens a specific partner's section. Simply append the partner's `id` to the end of the URL with a `#`.

**Example:**
`https://yourdomain.com/red/april-2026/#partner-ekaa`

The page will load, identify the section with `id="partner-ekaa"`, and automatically expand it for the user.


---


## 🎨 How to Change Colors & Design Language

The entire platform is built with a single centralized design system located in `assets/style.css`. There is no build step required.

### Global CSS Variables
Open `assets/style.css` and look at the top under `:root`. This is where all the master colors, fonts, and spacing variables live. If you change a color here, it updates across the entire site instantly.

```css
:root {
  /* Brand palette */
  --rb-red: #C62828;           /* Main primary accent */
  --rb-red-light: #EF5350;
  --rb-red-dark: #8E0000;
  --rb-red-glow: rgba(198, 40, 40, 0.25);

  --rb-black: #1A1A1A;
  --rb-black-card: #232323;

  --rb-gold: #D4AF37;          /* Used for Black tier elements */
  --rb-gold-light: #F5D060;
  
  /* WhatsApp button color */
  --rb-whatsapp: #25D366;
  
  /* Typography */
  --font-display: 'Playfair Display', Georgia, serif;
  --font-body: 'Inter', -apple-system, sans-serif;
  ...
}
```

### Changing Colors
- If you want a different "Red", just change `--rb-red` to a new hex code. 
- The gold accents for the Black tier can be updated by changing `--rb-gold`.
- The gradients on the root index page buttons use these variables, so changing the hex codes here will automatically update those gradients.

### Changing Fonts
The fonts are loaded via Google Fonts at the very top of `assets/style.css`:
```css
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&family=Playfair+Display:wght@400;500;600;700&display=swap');
```
If you want to use a different font (like "Roboto" or "Outfit"), swap out that `@import` URL (you can get the URL from Google Fonts) and then change `--font-display` and/or `--font-body` to the new font family name.


