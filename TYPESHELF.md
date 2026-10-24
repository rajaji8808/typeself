# TypeShelf
A cozy typing-practice application built with React, TypeScript, Vinext, Cloudflare D1 and R2.

## Important source files
- `app/page.tsx`: library, typing engine, stats, PDF extraction, original SVG mascot and admin interface.
- `app/globals.css`: responsive cartoon design, animated mascot and themes.
- `app/api/books/route.ts`: persistent PDF upload and library listing.
- `app/api/books/[id]/route.ts`: text retrieval, publication and deletion.
- `lib/server.ts`: server-side administrator authorization and origin checking.
- `db/schema.ts` and `drizzle/`: book database schema and migrations.

## Upload books
Open `/admin` or choose **Upload a book** from the library. No sign-in is required. Upload a text-based PDF (up to 25 MB), review its extracted sections, and add it directly to the shared library. PDFs are stored in R2 and metadata in D1. There is no application book-count cap; storage quotas still apply. Extracted text is limited to 8 million characters. Scanned PDFs require OCR first. Because uploads are public, keep origin checks and size/content validation enabled.

## Practice
Four original sample stories are bundled. Type directly on the displayed passage. Correct characters become dark, incorrect characters red. Backspace corrects displayed errors but preserves cumulative mistake counts. Timer pauses on tab hiding and supports timed or untimed practice. Guest statistics and section position are browser-local, not synced across devices. Partial in-section input is not restored. Book deletions/reuploads get new IDs and do not inherit stale progress.

## Development
Use the supplied project scripts. `npm run dev` starts local development; `npm run build` creates the Worker bundle. `npm run db:generate` creates schema migrations. D1 and R2 must be configured in `.openai/hosting.json`. The PDF worker is served locally from `public/pdf.worker.min.mjs`; update it from `node_modules/pdfjs-dist/build/` whenever upgrading pdfjs-dist.

Sites provides authenticated identity headers. Do not host behind an untrusted proxy that lets clients forge `oai-authenticated-user-*` headers. For non-Sites hosting, replace this integration with a verified session system.

## Current scope
Single PDF uploads with review, publish/unpublish, delete, search/category filters, typing controls, browser-local results and settings. No OCR, user accounts, cross-device progress, batch uploads, or metadata editing after upload. Uploaded books are immediately available to visitors who have Site access. Changing the website audience is a separate owner action.
