# Hudaia — Lessons Learned

## 2026-10-17: Native fullscreen for the Quran popup reader

### Problem
The Quran popup expanded to fill the webpage, but the mobile browser address/search bar remained visible. The fullscreen toolbar button did not reliably enter the browser's native fullscreen mode.

### Root cause
The original implementation requested fullscreen on the popup `<dialog>` element (`previewDialog.requestFullscreen()`). A later workaround only toggled the dialog's `.immersive` CSS class, which resized content **inside** the browser rather than activating native fullscreen.

### Working fix
Request native fullscreen on the **document root** instead of the dialog:

```js
const target = document.documentElement;
await target.requestFullscreen({ navigationUI: "hide" });
```

For exit, use `document.exitFullscreen()`. Listen for `fullscreenchange` and update the fullscreen button/immersive state from `Boolean(document.fullscreenElement)`. Retain the CSS immersive fallback for browsers that reject the native request. The `navigationUI: "hide"` option is a hint, not a guarantee; browser support and permissions vary.

### Related landscape layout fix
The Quran text previously overlapped the toolbar in landscape. The fix assigned a dedicated **64px toolbar column** and a separate `minmax(0,1fr)` reader column with CSS Grid, including in fullscreen. Keep the toolbar and text in separate layout tracks; do not use absolute positioning that lets content slide underneath controls.

### Regression-prevention checklist
- Open the Quran popup in portrait and landscape.
- Tap fullscreen: verify **browser-level fullscreen**, not merely a larger popup within the browser.
- Tap again and verify exit.
- Check that the toolbar and Quran text do not overlap in landscape.
- Check that text scrolls, controls remain usable, and single/two-page selection persists.
- Test on target mobile browsers; fullscreen support can vary.

### Implementation reference
- Source: `quran/index.html`
- Native fullscreen correction: commit `a1f8571`
- Landscape toolbar/grid separation: commit `4841aeb`
