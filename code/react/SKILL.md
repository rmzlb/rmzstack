---
name: react
description: |
  React/Next.js performance optimization, re-render prevention, bundle size reduction,
  and data fetching patterns. Use when: writing React components, reviewing code,
  fixing performance issues, implementing new features, or auditing for best practices.
  Covers 50+ rules across 8 categories from CRITICAL to LOW priority.
allowed-tools:
  - Read
  - Grep
  - Glob
  - Edit
---

# React Best Practices

**Version:** 1.0.0 | **Updated:** January 2026

> 50+ rules across 8 categories, prioritized from CRITICAL to LOW impact.
> Extended with Helmai-specific patterns (Dark Mode, TanStack Query, Zustand).

## When to Use This Skill

- Writing new React/Next.js components
- Reviewing code for performance issues
- Fixing re-render or bundle size problems
- Implementing data fetching patterns
- Auditing components for best practices
- Debugging slow renders or waterfalls

## Quick Reference

| Priority | Category | Rules | Reference |
|----------|----------|-------|-----------|
| 🔴 CRITICAL | Waterfalls | 6 | [waterfalls.md](references/waterfalls.md) |
| 🔴 CRITICAL | Bundle Size | 5 | [bundle-size.md](references/bundle-size.md) |
| 🟠 HIGH | Server-Side | 5 | [server-side.md](references/server-side.md) |
| 🟡 MEDIUM-HIGH | Client Fetching | 3 | [client-fetching.md](references/client-fetching.md) |
| 🟡 MEDIUM | Re-renders | 7 | [re-renders.md](references/re-renders.md) |
| 🟡 MEDIUM | Rendering | 7 | [rendering.md](references/rendering.md) |
| 🟢 LOW-MEDIUM | JavaScript | 12 | [javascript.md](references/javascript.md) |
| 🟢 LOW | Advanced | 2 | [advanced.md](references/advanced.md) |
| 🔵 PROJECT | Helmai-Specific | 5 | [helmai.md](references/helmai.md) |

## Critical Patterns (Always Check)

### Waterfall Prevention
```tsx
// ❌ Sequential - 3 round trips
const user = await fetchUser()
const posts = await fetchPosts()

// ✅ Parallel - 1 round trip
const [user, posts] = await Promise.all([fetchUser(), fetchPosts()])
```

### Bundle Size
```tsx
// ❌ Loads 1,583 modules
import { Check } from 'lucide-react'

// ✅ Next.js config
// next.config.js: optimizePackageImports: ['lucide-react']
```

### Dark Mode (Helmai MANDATORY)
```tsx
// ❌ Light only
className="bg-white text-gray-700"

// ✅ With dark variants
className="bg-white dark:bg-gray-900 text-gray-700 dark:text-gray-200"
```

### TanStack Query Pattern
```tsx
// Use query key factories
import { patientKeys } from '@/hooks/queryKeys'

const { data } = useQuery({
  queryKey: patientKeys.detail(patientId),
  queryFn: () => fetchPatient(patientId),
})
```

### Toast Notifications
```tsx
import { toast } from 'sonner'

// ❌ Never use alert()
// ✅ Always use Sonner
toast.error('Something went wrong')
toast.success('Saved successfully')
```

## Decision Tree

```
Performance Issue?
├── Slow initial load → Check [bundle-size.md](references/bundle-size.md)
├── Slow data fetch → Check [waterfalls.md](references/waterfalls.md)
├── UI stuttering → Check [re-renders.md](references/re-renders.md)
├── Long lists slow → Check [rendering.md](references/rendering.md)
└── API route slow → Check [server-side.md](references/server-side.md)
```

## Checklist Before PR

- [ ] No sequential awaits (use Promise.all)
- [ ] Dynamic imports for heavy components
- [ ] Dark mode variants on all UI elements
- [ ] TanStack Query with key factories
- [ ] Sonner for notifications (not alert)
- [ ] Error boundaries around async components

## Feedback Loop

When reviewing or fixing performance issues:

1. **Identify** → Run `pnpm build` or check React DevTools
2. **Fix** → Apply patterns from reference files
3. **Validate** → Re-run build, verify no new warnings
4. **Repeat** → If issues remain, continue loop

## External Resources

- [React Documentation](https://react.dev)
- [Next.js Documentation](https://nextjs.org)
- [TanStack Query](https://tanstack.com/query)
- [Vercel Performance Guide](https://vercel.com/docs/speed-insights)
