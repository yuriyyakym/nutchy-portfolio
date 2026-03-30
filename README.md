# Nuchy Portfolio

Minimal SvelteKit shell with prerendering enabled and `normalize.css` applied globally.

## Commands

```sh
pnpm dev
pnpm check
pnpm build
```

## Structure

- `src/routes/+layout.svelte`
  Imports `normalize.css` and renders the route content.
- `src/routes/+page.svelte`
  Empty home route.
- `src/routes/about/+page.svelte`
  Empty about route.
- `src/routes/gallery/+page.svelte`
  Empty gallery route.
- `src/routes/contact/+page.svelte`
  Empty contact route.

## Notes

- The project uses `@sveltejs/adapter-static`, so routes are prerendered.
- There is no custom global stylesheet or site content layer in this version.
- If you rebuild the actual site later, start from the routes and add only the content and styles you want to keep.
