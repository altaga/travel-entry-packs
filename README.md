# travel-entry-packs

JSON schema and validator for country entry-rule packs. A rule is valid only when its quote appears verbatim in the cited source text.

MIT license. This is a small standalone package. [Waypoint](https://github.com/altaga/Waypoint) uses the same pack shape for its travel concierge.

Not legal advice. Sample packs are machine-extracted and not reviewed by a person. Travelers must confirm every rule with the official source.

## What is in a pack

Each `XX.json` file is one country (ISO 3166-1 alpha-2). It holds:

- local rules, each with a short summary, a verbatim quote, a source id, a verification date, and a confidence level
- source records (title, URL, license, retrieval date)
- a tier: `A` human-reviewed, `B` machine-extracted, `C` live fallback
- essentials such as currency, plug types, and emergency numbers

The validator checks four things:

1. The file matches the schema (`schema/pack.schema.json`, also enforced in code with Zod).
2. Every quote is found, after light normalization, inside the cached text of a source it cites.
3. Every source id on a rule exists on the pack.
4. Verification and retrieval dates are within 90 days of the reference date. Tier `A` also requires a named human reviewer.

## Run

Node.js 22 or newer.

```bash
npm ci
npm test
npm run validate
```

`npm run validate` checks `examples/JP.json` and `examples/SG.json` against `examples/sources-text/`.

To check your own packs:

```bash
npx tsx bin/validate.ts path/to/packs --sources path/to/sources-text --reference-date 2026-10-21
```

Each pack `XX.json` needs a sibling source cache `sources-text/XX.json`: an object whose keys are source ids and whose values are the plain text those quotes were taken from.

## Samples

| File | Country | Tier |
|---|---|---|
| `examples/JP.json` | Japan | B, machine-extracted |
| `examples/SG.json` | Singapore | B, machine-extracted |

## Schema

`schema/pack.schema.json`

The schema id is `https://github.com/altaga/travel-entry-packs/schema/pack.schema.json`.
