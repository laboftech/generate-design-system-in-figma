# Heroicons SVG Reference for Figma

> All icons are from [Heroicons v2](https://heroicons.com/) (MIT license), 24x24 outline variant.
> Every SVG string below is ready to pass directly to `figma.createNodeFromSvg()`.
> `stroke="currentColor"` has been replaced with `stroke="black"` for Figma compatibility — recolor after import via the Plugin API.

---

## Helper Functions

Embed these in every `use_figma` call that creates icons.

### Create a single icon node

```javascript
function createHeroicon(svgString, name, size) {
  const node = figma.createNodeFromSvg(svgString);
  node.name = name;
  // Only resize if a non-default size is requested. The SVG frame
  // is already 24x24 from the viewBox — resizing at 24 is a no-op
  // but resizing to other values scales the frame uniformly.
  if (size && size !== 24) {
    node.resize(size, size);
  }
  return node;
}
```

### Create an icon as a reusable Component (for INSTANCE_SWAP)

```javascript
function createHeroiconComponent(svgString, name, size) {
  const sz = size || 24;
  const comp = figma.createComponent();
  comp.name = 'Icon/' + name;
  comp.resize(sz, sz);

  // CRITICAL: Keep the SVG frame intact — do NOT unwrap children.
  // createNodeFromSvg() produces a frame sized to the viewBox (24x24).
  // Unwrapping individual vectors loses the viewBox coordinate space,
  // causing multi-path icons (eye, cog, tag, etc.) to stretch or
  // misalign. Keeping the frame preserves all internal path positions.
  const svgNode = figma.createNodeFromSvg(svgString);
  svgNode.name = 'svg';
  svgNode.x = 0;
  svgNode.y = 0;
  // Do NOT call svgNode.resize() — the frame is already 24x24 from
  // the viewBox. Resizing can distort children non-uniformly.
  comp.appendChild(svgNode);
  svgNode.constraints = { horizontal: 'SCALE', vertical: 'SCALE' };

  return comp;
}
```

### Recolor all strokes in an imported SVG node

```javascript
function recolorIcon(node, r, g, b) {
  function walk(n) {
    if ('strokes' in n && n.strokes.length > 0) {
      n.strokes = n.strokes.map(s => ({...s, color: {r, g, b}}));
    }
    if ('fills' in n && n.fills.length > 0) {
      n.fills = n.fills.map(f => ({...f, color: {r, g, b}}));
    }
    if ('children' in n) {
      for (const child of n.children) walk(child);
    }
  }
  walk(node);
}
```

---

## Icon Catalog

### check

Used by: **Checkbox** (checked state), **Toggle** (on-state thumb indicator), **Dropdown** (selected item mark)

```
<svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="black" stroke-width="1.5" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" d="m4.5 12.75 6 6 9-13.5"/></svg>
```

---

### minus

Used by: **Checkbox** (indeterminate state)

```
<svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="black" stroke-width="1.5" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" d="M5 12h14"/></svg>
```

---

### chevron-down

Used by: **Accordion** (closed state), **Dropdown** (trigger chevron)

```
<svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="black" stroke-width="1.5" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" d="m19.5 8.25-7.5 7.5-7.5-7.5"/></svg>
```

---

### chevron-up

Used by: **Accordion** (open state — or rotate chevron-down 180deg)

```
<svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="black" stroke-width="1.5" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" d="m4.5 15.75 7.5-7.5 7.5 7.5"/></svg>
```

---

### chevron-right

Used by: **Navigation** (breadcrumb separator), **Card** (image pagination forward)

```
<svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="black" stroke-width="1.5" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" d="m8.25 4.5 7.5 7.5-7.5 7.5"/></svg>
```

---

### chevron-left

Used by: **Card** (image pagination backward)

```
<svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="black" stroke-width="1.5" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" d="M15.75 19.5 8.25 12l7.5-7.5"/></svg>
```

---

### x-mark

Used by: **Tooltip** (close button), **Chip** (remove/close)

```
<svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="black" stroke-width="1.5" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" d="M6 18 18 6M6 6l12 12"/></svg>
```

---

### magnifying-glass

Used by: **Input** (search variant trailing icon), **Navigation** (search action)

```
<svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="black" stroke-width="1.5" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" d="m21 21-5.197-5.197m0 0A7.5 7.5 0 1 0 5.196 5.196a7.5 7.5 0 0 0 10.607 10.607Z"/></svg>
```

---

### eye

Used by: **Input** (password show)

```
<svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="black" stroke-width="1.5" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" d="M2.036 12.322a1.012 1.012 0 0 1 0-.639C3.423 7.51 7.36 4.5 12 4.5c4.638 0 8.573 3.007 9.963 7.178.07.207.07.431 0 .639C20.577 16.49 16.64 19.5 12 19.5c-4.638 0-8.573-3.007-9.963-7.178Z"/><path stroke-linecap="round" stroke-linejoin="round" d="M15 12a3 3 0 1 1-6 0 3 3 0 0 1 6 0Z"/></svg>
```

---

### eye-slash

Used by: **Input** (password hide)

```
<svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="black" stroke-width="1.5" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" d="M3.98 8.223A10.477 10.477 0 0 0 1.934 12C3.226 16.338 7.244 19.5 12 19.5c.993 0 1.953-.138 2.863-.395M6.228 6.228A10.451 10.451 0 0 1 12 4.5c4.756 0 8.773 3.162 10.065 7.498a10.522 10.522 0 0 1-4.293 5.774M6.228 6.228 3 3m3.228 3.228 3.65 3.65m7.894 7.894L21 21m-3.228-3.228-3.65-3.65m0 0a3 3 0 1 0-4.243-4.243m4.242 4.242L9.88 9.88"/></svg>
```

---

### exclamation-circle

Used by: **Input** (error state icon)

```
<svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="black" stroke-width="1.5" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" d="M12 9v3.75m9-.75a9 9 0 1 1-18 0 9 9 0 0 1 18 0Zm-9 3.75h.008v.008H12v-.008Z"/></svg>
```

---

### exclamation-triangle

Used by: **Callout** (warning variant)

```
<svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="black" stroke-width="1.5" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" d="M12 9v3.75m-9.303 3.376c-.866 1.5.217 3.374 1.948 3.374h14.71c1.73 0 2.813-1.874 1.948-3.374L13.949 3.378c-.866-1.5-3.032-1.5-3.898 0L2.697 16.126ZM12 15.75h.007v.008H12v-.008Z"/></svg>
```

---

### information-circle

Used by: **Callout** (info variant)

```
<svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="black" stroke-width="1.5" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" d="m11.25 11.25.041-.02a.75.75 0 0 1 1.063.852l-.708 2.836a.75.75 0 0 0 1.063.853l.041-.021M21 12a9 9 0 1 1-18 0 9 9 0 0 1 18 0Zm-9-3.75h.008v.008H12V8.25Z"/></svg>
```

---

### check-circle

Used by: **Callout** (success variant)

```
<svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="black" stroke-width="1.5" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" d="M9 12.75 11.25 15 15 9.75M21 12a9 9 0 1 1-18 0 9 9 0 0 1 18 0Z"/></svg>
```

---

### x-circle

Used by: **Input** (clear/reset button)

```
<svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="black" stroke-width="1.5" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" d="m9.75 9.75 4.5 4.5m0-4.5-4.5 4.5M21 12a9 9 0 1 1-18 0 9 9 0 0 1 18 0Z"/></svg>
```

---

### bars-3

Used by: **Navigation** (hamburger menu / mobile toggle)

```
<svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="black" stroke-width="1.5" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" d="M3.75 6.75h16.5M3.75 12h16.5m-16.5 5.25h16.5"/></svg>
```

---

### bell

Used by: **Navigation** (notifications action)

```
<svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="black" stroke-width="1.5" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" d="M14.857 17.082a23.848 23.848 0 0 0 5.454-1.31A8.967 8.967 0 0 1 18 9.75V9A6 6 0 0 0 6 9v.75a8.967 8.967 0 0 1-2.312 6.022c1.733.64 3.56 1.085 5.455 1.31m5.714 0a24.255 24.255 0 0 1-5.714 0m5.714 0a3 3 0 1 1-5.714 0"/></svg>
```

---

### cog-6-tooth

Used by: **Navigation** (settings action), **Tile** (settings tile)

```
<svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="black" stroke-width="1.5" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" d="M9.594 3.94c.09-.542.56-.94 1.11-.94h2.593c.55 0 1.02.398 1.11.94l.213 1.281c.063.374.313.686.645.87.074.04.147.083.22.127.325.196.72.257 1.075.124l1.217-.456a1.125 1.125 0 0 1 1.37.49l1.296 2.247a1.125 1.125 0 0 1-.26 1.431l-1.003.827c-.293.241-.438.613-.43.992a7.723 7.723 0 0 1 0 .255c-.008.378.137.75.43.991l1.004.827c.424.35.534.955.26 1.43l-1.298 2.247a1.125 1.125 0 0 1-1.369.491l-1.217-.456c-.355-.133-.75-.072-1.076.124a6.47 6.47 0 0 1-.22.128c-.331.183-.581.495-.644.869l-.213 1.281c-.09.543-.56.94-1.11.94h-2.594c-.55 0-1.019-.398-1.11-.94l-.213-1.281c-.062-.374-.312-.686-.644-.87a6.52 6.52 0 0 1-.22-.127c-.325-.196-.72-.257-1.076-.124l-1.217.456a1.125 1.125 0 0 1-1.369-.49l-1.297-2.247a1.125 1.125 0 0 1 .26-1.431l1.004-.827c.292-.24.437-.613.43-.991a6.932 6.932 0 0 1 0-.255c.007-.38-.138-.751-.43-.992l-1.004-.827a1.125 1.125 0 0 1-.26-1.43l1.297-2.247a1.125 1.125 0 0 1 1.37-.491l1.216.456c.356.133.751.072 1.076-.124.072-.044.146-.086.22-.128.332-.183.582-.495.644-.869l.214-1.28Z"/><path stroke-linecap="round" stroke-linejoin="round" d="M15 12a3 3 0 1 1-6 0 3 3 0 0 1 6 0Z"/></svg>
```

---

### user-circle

Used by: **Navigation** (profile/avatar action)

```
<svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="black" stroke-width="1.5" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" d="M17.982 18.725A7.488 7.488 0 0 0 12 15.75a7.488 7.488 0 0 0-5.982 2.975m11.963 0a9 9 0 1 0-11.963 0m11.963 0A8.966 8.966 0 0 1 12 21a8.966 8.966 0 0 1-5.982-2.275M15 9.75a3 3 0 1 1-6 0 3 3 0 0 1 6 0Z"/></svg>
```

---

### home

Used by: **Navigation** (home link), **Tile** (home tile)

```
<svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="black" stroke-width="1.5" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" d="m2.25 12 8.954-8.955c.44-.439 1.152-.439 1.591 0L21.75 12M4.5 9.75v10.125c0 .621.504 1.125 1.125 1.125H9.75v-4.875c0-.621.504-1.125 1.125-1.125h2.25c.621 0 1.125.504 1.125 1.125V21h4.125c.621 0 1.125-.504 1.125-1.125V9.75M8.25 21h8.25"/></svg>
```

---

### heart

Used by: **Card** (favorite/save action overlay)

```
<svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="black" stroke-width="1.5" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" d="M21 8.25c0-2.485-2.099-4.5-4.688-4.5-1.935 0-3.597 1.126-4.312 2.733-.715-1.607-2.377-2.733-4.313-2.733C5.1 3.75 3 5.765 3 8.25c0 7.22 9 12 9 12s9-4.78 9-12Z"/></svg>
```

---

### share

Used by: **Card** (share action overlay)

```
<svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="black" stroke-width="1.5" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" d="M7.217 10.907a2.25 2.25 0 1 0 0 2.186m0-2.186c.18.324.283.696.283 1.093s-.103.77-.283 1.093m0-2.186 9.566-5.314m-9.566 7.5 9.566 5.314m0 0a2.25 2.25 0 1 0 3.935 2.186 2.25 2.25 0 0 0-3.935-2.186Zm0-12.814a2.25 2.25 0 1 0 3.933-2.185 2.25 2.25 0 0 0-3.933 2.185Z"/></svg>
```

---

### ellipsis-horizontal

Used by: **Card** (overflow menu action)

```
<svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="black" stroke-width="1.5" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" d="M6.75 12a.75.75 0 1 1-1.5 0 .75.75 0 0 1 1.5 0ZM12.75 12a.75.75 0 1 1-1.5 0 .75.75 0 0 1 1.5 0ZM18.75 12a.75.75 0 1 1-1.5 0 .75.75 0 0 1 1.5 0Z"/></svg>
```

---

### calendar

Used by: **Input** (date picker trailing icon)

```
<svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="black" stroke-width="1.5" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" d="M6.75 3v2.25M17.25 3v2.25M3 18.75V7.5a2.25 2.25 0 0 1 2.25-2.25h13.5A2.25 2.25 0 0 1 21 7.5v11.25m-18 0A2.25 2.25 0 0 0 5.25 21h13.5A2.25 2.25 0 0 0 21 18.75m-18 0v-7.5A2.25 2.25 0 0 1 5.25 9h13.5A2.25 2.25 0 0 1 21 11.25v7.5"/></svg>
```

---

### tag

Used by: **Callout** (category/tag icon)

```
<svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="black" stroke-width="1.5" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" d="M9.568 3H5.25A2.25 2.25 0 0 0 3 5.25v4.318c0 .597.237 1.17.659 1.591l9.581 9.581c.699.699 1.78.872 2.607.33a18.095 18.095 0 0 0 5.223-5.223c.542-.827.369-1.908-.33-2.607L11.16 3.66A2.25 2.25 0 0 0 9.568 3Z"/><path stroke-linecap="round" stroke-linejoin="round" d="M6 6h.008v.008H6V6Z"/></svg>
```

---

### clock

Used by: **Callout** (time/schedule icon)

```
<svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="black" stroke-width="1.5" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" d="M12 6v6h4.5m4.5 0a9 9 0 1 1-18 0 9 9 0 0 1 18 0Z"/></svg>
```

---

### currency-dollar

Used by: **Callout** (value/pricing icon)

```
<svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="black" stroke-width="1.5" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" d="M12 6v12m-3-2.818.879.659c1.171.879 3.07.879 4.242 0 1.172-.879 1.172-2.303 0-3.182C13.536 12.219 12.768 12 12 12c-.725 0-1.45-.22-2.003-.659-1.106-.879-1.106-2.303 0-3.182s2.9-.879 4.006 0l.415.33M21 12a9 9 0 1 1-18 0 9 9 0 0 1 18 0Z"/></svg>
```

---

### arrow-path

Used by: **Button** (loading/spinner state)

```
<svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="black" stroke-width="1.5" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" d="M16.023 9.348h4.992v-.001M2.985 19.644v-4.992m0 0h4.992m-4.993 0 3.181 3.183a8.25 8.25 0 0 0 13.803-3.7M4.031 9.865a8.25 8.25 0 0 1 13.803-3.7l3.181 3.182m0-4.991v4.99"/></svg>
```

---

### star

Used by: **Tile** (favorites/rating tile), **Card** (rating)

```
<svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="black" stroke-width="1.5" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" d="M11.48 3.499a.562.562 0 0 1 1.04 0l2.125 5.111a.563.563 0 0 0 .475.345l5.518.442c.499.04.701.663.321.988l-4.204 3.602a.563.563 0 0 0-.182.557l1.285 5.385a.562.562 0 0 1-.84.61l-4.725-2.885a.562.562 0 0 0-.586 0L6.982 20.54a.562.562 0 0 1-.84-.61l1.285-5.386a.562.562 0 0 0-.182-.557l-4.204-3.602a.562.562 0 0 1 .321-.988l5.518-.442a.563.563 0 0 0 .475-.345L11.48 3.5Z"/></svg>
```

---

### chart-bar

Used by: **Tile** (analytics/dashboard tile)

```
<svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="black" stroke-width="1.5" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" d="M3 13.125C3 12.504 3.504 12 4.125 12h2.25c.621 0 1.125.504 1.125 1.125v6.75C7.5 20.496 6.996 21 6.375 21h-2.25A1.125 1.125 0 0 1 3 19.875v-6.75ZM9.75 8.625c0-.621.504-1.125 1.125-1.125h2.25c.621 0 1.125.504 1.125 1.125v11.25c0 .621-.504 1.125-1.125 1.125h-2.25a1.125 1.125 0 0 1-1.125-1.125V8.625ZM16.5 4.125c0-.621.504-1.125 1.125-1.125h2.25C20.496 3 21 3.504 21 4.125v15.75c0 .621-.504 1.125-1.125 1.125h-2.25a1.125 1.125 0 0 1-1.125-1.125V4.125Z"/></svg>
```

---

### document-text

Used by: **Tile** (documents tile)

```
<svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="black" stroke-width="1.5" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" d="M19.5 14.25v-2.625a3.375 3.375 0 0 0-3.375-3.375h-1.5A1.125 1.125 0 0 1 13.5 7.125v-1.5a3.375 3.375 0 0 0-3.375-3.375H8.25m0 12.75h7.5m-7.5 3H12M10.5 2.25H5.625c-.621 0-1.125.504-1.125 1.125v17.25c0 .621.504 1.125 1.125 1.125h12.75c.621 0 1.125-.504 1.125-1.125V11.25a9 9 0 0 0-9-9Z"/></svg>
```

---

### globe-alt

Used by: **Tabs** (icon+label variant example), **Tile** (web/global tile)

```
<svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="black" stroke-width="1.5" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" d="M12 21a9.004 9.004 0 0 0 8.716-6.747M12 21a9.004 9.004 0 0 1-8.716-6.747M12 21c2.485 0 4.5-4.03 4.5-9S14.485 3 12 3m0 18c-2.485 0-4.5-4.03-4.5-9S9.515 3 12 3m0 0a8.997 8.997 0 0 1 7.843 4.582M12 3a8.997 8.997 0 0 0-7.843 4.582m15.686 0A11.953 11.953 0 0 1 12 10.5c-2.998 0-5.74-1.1-7.843-2.918m15.686 0A8.959 8.959 0 0 1 21 12c0 .778-.099 1.533-.284 2.253m0 0A17.919 17.919 0 0 1 12 16.5c-3.162 0-6.133-.815-8.716-2.247m0 0A9.015 9.015 0 0 1 3 12c0-1.605.42-3.113 1.157-4.418"/></svg>
```

---

### bookmark

Used by: **Card** (save/bookmark action), **Tabs** (icon+label variant example)

```
<svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="black" stroke-width="1.5" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" d="M17.593 3.322c1.1.128 1.907 1.077 1.907 2.185V21L12 17.25 4.5 21V5.507c0-1.108.806-2.057 1.907-2.185a48.507 48.507 0 0 1 11.186 0Z"/></svg>
```

---

### photo

Used by: **Card** (image placeholder)

```
<svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="black" stroke-width="1.5" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" d="m2.25 15.75 5.159-5.159a2.25 2.25 0 0 1 3.182 0l5.159 5.159m-1.5-1.5 1.409-1.409a2.25 2.25 0 0 1 3.182 0l2.909 2.909m-18 3.75h16.5a1.5 1.5 0 0 0 1.5-1.5V6a1.5 1.5 0 0 0-1.5-1.5H3.75A1.5 1.5 0 0 0 2.25 6v12a1.5 1.5 0 0 0 1.5 1.5Zm10.5-11.25h.008v.008h-.008V8.25Zm.375 0a.375.375 0 1 1-.75 0 .375.375 0 0 1 .75 0Z"/></svg>
```

---

### squares-2x2

Used by: **Tabs** (icon+label variant example), **Navigation** (dashboard link)

```
<svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="black" stroke-width="1.5" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" d="M3.75 6A2.25 2.25 0 0 1 6 3.75h2.25A2.25 2.25 0 0 1 10.5 6v2.25a2.25 2.25 0 0 1-2.25 2.25H6a2.25 2.25 0 0 1-2.25-2.25V6ZM3.75 15.75A2.25 2.25 0 0 1 6 13.5h2.25a2.25 2.25 0 0 1 2.25 2.25V18a2.25 2.25 0 0 1-2.25 2.25H6A2.25 2.25 0 0 1 3.75 18v-2.25ZM13.5 6a2.25 2.25 0 0 1 2.25-2.25H18A2.25 2.25 0 0 1 20.25 6v2.25A2.25 2.25 0 0 1 18 10.5h-2.25a2.25 2.25 0 0 1-2.25-2.25V6ZM13.5 15.75a2.25 2.25 0 0 1 2.25-2.25H18a2.25 2.25 0 0 1 2.25 2.25V18A2.25 2.25 0 0 1 18 20.25h-2.25A2.25 2.25 0 0 1 13.5 18v-2.25Z"/></svg>
```

---

## Quick Lookup: Component → Icons

| Component | Required Icons |
|---|---|
| Accordion | `chevron-down` (rotate 180deg for open state) |
| Button | `arrow-path` (loading spinner) |
| Callout | `information-circle`, `exclamation-triangle`, `check-circle`, `tag`, `clock`, `currency-dollar` |
| Card | `heart`, `share`, `ellipsis-horizontal`, `chevron-left`, `chevron-right`, `bookmark`, `photo` |
| Checkbox | `check` (checked), `minus` (indeterminate) |
| Chip | `x-mark` (removable variant) |
| Dropdown | `chevron-down` (trigger), `check` (selected item) |
| Input | `eye`, `eye-slash`, `exclamation-circle`, `magnifying-glass`, `x-circle`, `calendar` |
| Navigation | `bars-3`, `magnifying-glass`, `bell`, `cog-6-tooth`, `user-circle`, `home`, `chevron-right` |
| Radio Button | _(none — uses a filled Ellipse node for the inner dot)_ |
| Tabs | `home`, `globe-alt`, `bookmark`, `squares-2x2` (representative set for icon+label variant) |
| Tile | `home`, `cog-6-tooth`, `chart-bar`, `document-text`, `star`, `globe-alt` |
| Toggle | `check` (on-state thumb indicator) |
| Tooltip | `x-mark` (close button) |