# Contributing

Thanks for helping make the Orbismo Playbook better. Contributions of all sizes are welcome: fixing a typo, tightening a rule that caused confusion, or adding a whole new template.

## What we're looking for

- **New templates and mini apps**: starters for archetypes not yet covered. Open an issue first to check the idea fits the catalog before writing it in full.
- **Improvements to existing starters**: clearer wording, rules that prevent real failure modes you've hit, better customization guidance.
- **Fixes**: typos, broken links, inaccuracies against current Orbismo behavior.

## Ground rules

- **Test in a real world.** Before submitting a starter or a behavioral change, install it in an Orbismo world and exercise it with a connected assistant. Note in the PR what you tested.
- **Follow the layout.** The [world-templates](world-templates/README.md) and [mini-apps](mini-apps/README.md) READMEs document the structure and design conventions every starter follows. Match them.
- **The library is canonical.** When a mini app exists in [mini-apps/](mini-apps/), that copy is the source of truth. If a template bundles it, change the library package first and re-sync the bundled copy in the same PR.
- **Keep starters generic.** No personal data, and nothing tied to one specific world. Anything the user must customize is called out in the starter's README.
- **Write for a first-time reader.** Every starter's README should let someone new to Orbismo install it without outside help.

## Submitting changes

1. Fork the repository and create a branch.
2. Make your changes, keeping each pull request focused on one template or fix.
3. Open a pull request describing what changed and, for behavioral changes, how you tested it.

## Questions

If you're unsure whether something belongs here, open an issue and ask. That's cheaper for everyone than a PR that has to be reworked.
