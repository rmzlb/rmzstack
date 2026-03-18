# Performance Review Checklist

Common antipatterns to catch during code review.

---

## Database

```
[ ] No N+1 queries (fetching related records in a loop)
[ ] New queries have appropriate indexes
[ ] No SELECT * — only fetch columns you need
[ ] Pagination on list endpoints (no unbounded queries)
[ ] Bulk operations use batch insert/update (not individual queries in loop)
[ ] Connection pooling configured (not new connection per request)
[ ] Transactions: short-lived, no external API calls inside transactions
```

## API / Network

```
[ ] Response payloads aren't bloated (no nested objects you don't need)
[ ] Expensive operations are async (email sending, image processing, PDF generation)
[ ] API calls to external services have timeouts set
[ ] Retry logic has exponential backoff (not infinite retry)
[ ] No waterfall API calls that could be parallel (Promise.all / join!)
[ ] Caching headers set on static/rarely-changing responses
[ ] Compression enabled (gzip/brotli) on responses > 1KB
```

## Frontend

```
[ ] Images optimized (WebP/AVIF, lazy loaded, sized correctly)
[ ] No render-blocking scripts in <head> without async/defer
[ ] Bundle size: new dependency justified? Check size with bundlephobia.com
[ ] No layout shift (CLS): images have width/height, fonts have fallbacks
[ ] Lists use virtualization if > 100 items (react-window, tanstack-virtual)
[ ] Memoization where needed (useMemo/useCallback for expensive computations)
[ ] No unnecessary re-renders (React.memo for pure components in hot paths)
```

## Memory & Compute

```
[ ] No memory leaks: event listeners removed on cleanup
[ ] No unbounded arrays/maps that grow forever
[ ] Large file processing: streamed, not loaded entirely in memory
[ ] CPU-intensive work: offloaded to worker thread or queue
[ ] Regex: no catastrophic backtracking patterns (test with large input)
[ ] Recursive functions: depth-limited or converted to iterative
```

## Red Flags (auto-flag in review)

| Pattern | Impact | Alternative |
|---------|--------|-------------|
| `await` in a for loop | N sequential round trips | `Promise.all()` or batch query |
| `JSON.parse(JSON.stringify(obj))` for deep clone | Slow for large objects | `structuredClone()` |
| `.filter().map()` chain | 2 array passes | Single `.reduce()` or for loop |
| `setTimeout(fn, 0)` for ordering | Unreliable, race-prone | Proper async/await flow |
| Fetching same data multiple times | Redundant network calls | Cache or lift to parent |
| `console.log` in production code | Perf + security concern | Proper logger with levels |
| Importing entire library for 1 function | Bundle bloat | Tree-shakeable import |
