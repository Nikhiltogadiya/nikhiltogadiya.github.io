# German i18n (Internationalization) — Design Spec
**Date:** 2026-03-16
**Status:** Approved

---

## Overview

Add German language support to the Nikhil Togadiya portfolio site. Users will be able to toggle between English (default) and German using a flag+text button in the navbar. Language preference is persisted via `localStorage`.

---

## Architecture

### New File: `assets/js/i18n.js`
Contains all translations as a plain JS object:

```js
const translations = {
  en: {
    "nav.about": "About",
    "hero.greeting": "Hello, I'm",
    // ...all keys
  },
  de: {
    "nav.about": "Über mich",
    "hero.greeting": "Hallo, ich bin",
    // ...all keys
  }
}
```

### HTML Changes
Every translatable element gets a `data-i18n="key"` attribute:
```html
<a href="#about" data-i18n="nav.about">About</a>
```

### JS Changes (main.js)
- `setLanguage(lang)` function: queries all `[data-i18n]` elements, replaces `textContent` with the translation for the given key, saves `lang` to `localStorage`.
- Re-initializes the typing animation strings when language switches.
- Flash-prevention inline script in `<head>` reads `localStorage` and sets `data-lang` attribute on `<html>` before paint.

---

## Language Toggle Button

**Location:** Navbar, immediately to the right of the existing theme toggle button.

**Appearance:**
- English active: `🇬🇧 EN`
- German active: `🇩🇪 DE`

**HTML:**
```html
<button class="lang-toggle" id="lang-toggle" aria-label="Toggle language">
  🇬🇧 EN
</button>
```

**Behavior:** Single click cycles EN → DE → EN. Button label updates to reflect the current language.

---

## Flash Prevention

Inline script in `<head>` (immediately after the existing theme flash-prevention script):
```html
<script>
  (function() {
    var l = localStorage.getItem('lang');
    if (l === 'de') document.documentElement.setAttribute('data-lang', 'de');
  })();
</script>
```

---

## Translation Scope

### Translated (EN + DE)
| Section | Content |
|---|---|
| Navbar | Nav link labels |
| Hero | Greeting, description, button labels, stat labels, typing strings |
| About | Section title, subtitle, paragraph text, highlight card titles & descriptions |
| Experience | Section title, subtitle, role titles, bullet points |
| Projects | Section title, subtitle, card subtitles, bullet points |
| Skills | Section title, subtitle, category headings |
| Education | Section title, subtitle, degree names |
| Contact | Section title, subtitle, all body text, button labels |
| Footer | Tagline, copyright text |

### NOT Translated (stays in English)
- Company/university proper names (Volkswagen AG, Ostfalia Hochschule, etc.)
- Technology tags (Python, LangChain, Docker, RAG, LLM, LoRA, etc.)
- Name, email address, social links
- Technical acronyms that are proper nouns

---

## File Changes Summary

| File | Change |
|---|---|
| `index.html` | Add `data-i18n` attributes to all translatable elements; add lang toggle button; add flash-prevention script |
| `assets/js/i18n.js` | **New file** — all EN + DE translation strings |
| `assets/js/main.js` | Add `setLanguage()`, lang toggle click handler, re-init typing on language switch |
| `assets/css/style.css` | Style the `.lang-toggle` button to match existing `.theme-toggle` |

---

## Non-Goals
- No URL-based language routing (e.g., `/de/`)
- No third-party i18n library
- No additional languages beyond EN and DE
- No server-side rendering
