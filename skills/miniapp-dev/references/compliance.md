# Compliance Checklist

- [ ] All API domains use HTTPS and are registered in MP Admin → Development → request domain
- [ ] `app.json` declares `permission` for location, camera, record, etc.
- [ ] Privacy Policy published in MP Admin
- [ ] ICP filing completed for all backend domains used by the Mini Program
- [ ] Mini Program itself has ICP filing if required by current regulations
- [ ] Payment features only for enterprise entities with merchant binding
- [ ] No hardcoded `appid` / `secret` in client code
- [ ] `sitemap.json` configured for search indexing

For the latest requirements, check the official WeChat docs or MP Admin console.

## Performance Tips

- Use `lazyCodeLoading`: `"requiredComponents"` in `app.json`.
- Split large pages into sub-packages (`subpackages` in `app.json`).
- Image: use `webp` format, specify `lazy-load` on `<image>`.
- List: use `recycle-view` from `@miniprogram-component-plus` for long lists.
- Avoid heavy computation in `onLoad`; defer to `onReady` or workers.
