# package.json research

## Goal

A research analysis on the usage of `package.json` throughout the entire JavaScript ecosystem.

## Research/Contribution Process

> Contributions welcome! Reach out in the [Slack channel][slack-channel] for help!

Within the `research` directory are a collection of markdown files each containing the analysis for one or more top-level `package.json` properties. The file, [package-json.md][package-json], contains a property research index.

Research should primarily focus on documenting the rules of a property's value. The analysis should avoid details regarding _how_ the value will be used by various tools. For example, the `dependencies` property has a number of ways a dependency can be included. Beyond the standard SemVer value, some tools also allow for various workspace specific syntax. The research for this property should include details regarding all the different ways a dependency could be listed, but not how a tool will necessarily resolve the dependency.

### Contributing New Research:
- Make sure that an analysis has not already been conducted on the property or properties you are analyzing.
  - Check the index in [package-json.md][package-json].
  - Check in progress pull requests.
    - If a PR seems stale, politely reach out to the author on the PR or within the [Slack channel][slack-channel].
- Open a draft PR including the following details:
  - Create a new file for your research in the `research` directory.
    - The file title should be short and representative of the property or properties being analyzed.
    - The file title should use [kebab-case](https://www.freecodecamp.org/news/programming-naming-conventions-explained/#what-is-kebab-case).
  - Add the property or properties and the new file to the index in [package-json.md][package-json].
    - Use reference links so they can be reused throughout the file reliably.
  - The PR title should follow the format: `[research] Property: <property>` or `[research] Properties: <property-1>, <property-2>`.
- Once your research is complete, mark the PR as "ready to review".

### Contributing to Existing Research:
- Open a PR with the title `[research] Edit: <file-name.md>`.

[package-json]: <./research/package-json.md>
[slack-channel]: <https://openjs-foundation.slack.com/archives/C05AWQH5E4R>