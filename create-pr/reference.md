# Reference

## Tool Detection Priority

### JavaScript / TypeScript

1. pnpm → `pnpm-lock.yaml`
2. yarn → `yarn.lock`
3. bun → `bun.lockb` or `bun.lock`
4. npm → fallback

---

## Script Priority

### Lint

1. lint:fix
2. lint
3. check

### Tests

1. test
2. test:ci
3. test:unit

### Build

1. build
2. compile
3. typecheck

---

## Conventional Commit Types

- feat
- fix
- chore
- refactor
- test
- docs
- perf
- ci

---

## Failure Handling

Always report:

1. command
2. error
3. impact
4. next step

Never hide failures.