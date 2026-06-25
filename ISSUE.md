# Upstream issue draft

**Title:** Astro 7 build fails on JSX inside object literal in template expression (Rolldown PARSE_ERROR)

## Environment

- Astro 7.0.x
- Minimal repro: https://github.com/YOUR_USER/astro-jsx-object-literal-repro (after you push)

## Description

In Astro 6, this template expression builds successfully:

```astro
{(layer = 'main') => ({
  main: (
    <Widget />
  ),
  other: (
    <p>Other layer</p>
  ),
}[layer])}
```

In Astro 7, `npm run build` fails with:

```
[PARSE_ERROR] Expected `>` but found `/`
    ╭─[ src/pages/index.astro:25:16 ]
    │
 25 │     main: <Widget />,
    │                   ╰── `>` expected
```

It looks like Rolldown receives the object literal with JSX shorthand (`main: <Widget />`) rather than the parenthesized JSX Astro accepts in v6 (`main: (<Widget />)`).

## Expected behavior

Build succeeds, same as Astro 6.

## Reproduction

```bash
git clone …
cd astro-jsx-object-literal-repro
git checkout main && npm i && npm run build   # OK on Astro 6
git checkout astro-7 && npm i && npm run build # fails on Astro 7
```

## Workaround

Replace the object lookup with a ternary or `switch`.
