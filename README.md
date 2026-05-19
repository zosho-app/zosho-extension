# Zosho Extensions Marketplace

Community extensions for the Zosho manga reader.

## Structure

```
marketplace/
├── marketplace.json              # Catalog (fetched by the app)
└── sources/
    └── mangakakalot/
        └── mangakakalot.json     # Full manifest with extension code
```

## How It Works

1. **`marketplace.json`** — A lightweight array of `MarketplaceListing` entries. The app fetches this file to display available extensions in the Extension Hub.

2. **`sources/<id>/<id>.json`** — Full `ExtensionManifest` JSON containing the serialized JavaScript source code in the `code` field. Downloaded on-demand when the user taps **Install**.

## Writing an Extension

Your extension code must be a single JavaScript string that:

1. Uses `__fetchText(url)` for all HTTP requests (injected by the runtime)
2. Returns an `ExtensionSource` object at the end via `return MySource;`

### ExtensionSource Contract

```javascript
{
  name: 'MySource',
  version: '1.0.0',
  language: 'en',
  baseUrl: 'https://example.com',

  getPopularManga(page)         // → { manga: [...], hasNextPage: bool }
  getLatestManga(page)          // → { manga: [...], hasNextPage: bool }
  searchManga(query, page)      // → { manga: [...], hasNextPage: bool }
  getMangaDetails(mangaId)      // → Manga object
  getChapterList(mangaId)       // → Chapter[]
  getPageList(chapterId)        // → string[] (image URLs)
}
```

### Manga Shape
```javascript
{
  id, title, coverUrl, author, artist,
  description, genres: [], status, url
}
```

### Chapter Shape
```javascript
{
  id, mangaId, title, chapterNumber,
  scanlator, url, dateUploaded, pageUrls: []
}
```

## Hosting

Upload this directory to a GitHub repository and update the `MARKETPLACE_URL` constant in the app's `marketplace-api.ts` with your raw GitHub URL:

```
https://raw.githubusercontent.com/<user>/zosho-extensions/main/marketplace.json
```
