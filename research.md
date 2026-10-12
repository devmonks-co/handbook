# Research before you build

Most features have been built before, by us or by someone else. Find that first.

## When

Any feature or change bigger than about half a day of work.

Timebox it. An hour is usually enough. If it needs more, say so in the issue.

## Look in this order

Stop at the first one that fits.

1. **Ours.** [`blocks`](https://github.com/devmonks-co/blocks) (reusable packages), the project templates (`nextjs-copier-template`, `fastapi-copier-template`), and past projects that solved the same problem. Not sure? Ask the team "has anyone built X?". It's a two-minute question.
2. **The platform's built-in.** What the framework, database or host already gives you: auth in Supabase, caching in Next.js, the admin in Django. Read the docs before adding anything.
3. **A mature library or service.** It must pass all four:
   - **Maintained:** a release or commit in the last 6 months
   - **Licence:** MIT, Apache 2.0, BSD or similar. GPL or AGPL in client code needs your lead's OK first
   - **Used:** real adoption, not a weekend project
   - **Typed and documented**

   Then ask: does it do what we need, or will we fight it? A library you have to patch around costs more than the code it saved.
4. **Build it.** Only when 1 to 3 don't fit. Keep it small, with a clean boundary, so it can be reused later.

## Write it down

Before you start, add an "Options checked" note to the issue:

```
Options checked
- blocks/notify: has the core, no SMS adapter yet
- Supabase built-in: email only
- Novu: a full platform, far more than we need
Picked: blocks/notify, and write the SMS adapter there
```

This is what the team discusses in the "Discuss" step of [our mantra](principles.md#our-mantra). For bigger choices, your lead signs off here, before any code.

## Give back

- Built something a second project needs? Propose it for `blocks`.
- Found a gap in a library? Contribute upstream (see [GitHub conventions](github.md#contributing-to-third-party-libraries)).

## See also

- [Principles](principles.md): the mantra this is step one of
- [GitHub conventions](github.md): issues come before non-trivial work
- [Coding guidelines](coding-guidelines.md): DRY, and when to stop and rethink
