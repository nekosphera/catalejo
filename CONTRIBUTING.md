# Contributing to Catalejo

Contributions are welcome. Read the next section first: this repository does
not work the way most repositories work, and knowing that before you write a
patch will save you the annoyance of watching it disappear.

## This tree is generated

Catalejo is exported from a private repository where the catalogue runs in
production. Every file here is written by that export, so the two cannot
drift apart and the code you read is the code that runs.

The consequence is blunt: **a pull request merged into this repository would
be erased by the next export.** So they are not merged here. A maintainer
takes your patch, applies it upstream, and the next export publishes it — with
your authorship and your sign-off intact in the commit that lands here.

What this means for you:

- Open the pull request here anyway. This is where the discussion happens.
- Expect it to be closed rather than merged, with the commit that carries your
  change named in the closing comment.
- If it is not published within a reasonable time, say so on the pull request.
  Silence is a maintainer failure, not a rejection.

Issues, questions and discussions belong here too, and those behave normally.

## Before opening a change

1. Open an issue describing the problem and the expected outcome.
2. Keep the change small and scoped to one thing.
3. Include no credentials, personal data, production data, certificates or
   private infrastructure details.
4. Run `./smoke.sh`. It stands up the catalogue store against a stub data
   space and validates a record against the profile. If it fails, the
   quickstart in the README is fiction and so is your change.

A pull request should explain what changed and why, what you ran to validate
it, and what you know it does not cover.

## Developer Certificate of Origin

Contributions use the [Developer Certificate of Origin 1.1](https://developercertificate.org/).
By signing off a commit you certify that you have the right to submit it under
this repository's licences.

Sign every commit:

```
git commit -s -m "..."
```

which appends `Signed-off-by: Your Name <your@email>` using your `git config`
identity. The sign-off travels with the commit when the change is published,
which is what makes the chain of provenance survive the export.

## Licences

Code is [Apache-2.0](LICENSE). Documentation is
[CC BY 4.0](DOCUMENTATION-LICENSE.md). Names and logos are not licensed by
either: see [TRADEMARKS.md](TRADEMARKS.md).

Contributions are accepted under those same licences and no others. There is
no contributor licence agreement and no copyright assignment: you keep your
copyright, and the project receives the rights the licences grant.

## Reporting a vulnerability

Do not open a public issue. See [SECURITY.md](SECURITY.md).

## Conduct

[CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md) applies here.
