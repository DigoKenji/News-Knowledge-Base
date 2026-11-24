---
type: Article Summary
title: What 5,400 Malicious npm Packages Reveal about Software Supply Chain Risk in 2026
description: Once counted by campaign, malicious npm publishing is steady; rare compromises of legitimate packages carry nearly
  all downloads.
resource: https://www.databricks.com/blog/what-5400-malicious-npm-packages-reveal-about-software-supply-chain-risk-2026
tags:
- security
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-24T07:00:00-03:00'
sources:
- resource: https://www.databricks.com/blog/what-5400-malicious-npm-packages-reveal-about-software-supply-chain-risk-2026
  title: What 5,400 Malicious npm Packages Reveal about Software Supply Chain Risk in 2026
  author: Databricks Threat Research
status: stable
source_url: https://www.databricks.com/blog/what-5400-malicious-npm-packages-reveal-about-software-supply-chain-risk-2026
source: Databricks
source_type: blog
published: '2026-11-23'
domain: security
concepts:
- oidc
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/422baf74172d81f5a2b6c31fddec9a6a
---

## 🎯 Core thesis
Once malicious npm counts are normalized by campaign, publishing activity is steady, while the rare compromises of legitimate packages carry nearly all the download exposure and registry takedowns trail public advisories.

## 🧭 Why the author wrote it
The article is the first in a series on supply-chain threats. It starts from about 5,400 confirmed malicious npm packages published between December 2025 and October 2026, the first year after npm revoked long-lived authentication tokens. Raw monthly totals make the threat look volatile, but the author argues the volatility comes from a few campaigns; if that is right, defenders should track campaigns, compromises and removal lag rather than package counts.

## Scope and method
The dataset comes from a scanner that analyzes npm publish events. Only packages with at least one independent source are kept: a public advisory, a vendor campaign report, an npm security takedown or analyst review. Time is measured by npm publish date. The unit of record is the package version, and each package is dated by its first malicious version. Download counts are monthly snapshots taken at scan time, and registry status was checked on November 6, 2026.

> 🔎 Added context: The figures are referenced by caption but are not included in the extracted text, so this note relies on the prose. The article has no code blocks or checklists, so none are reproduced.

### Verified malicious package
A package enters the dataset only after an independent source confirms it, which filters out raw scanner alerts. The filter favors precision over coverage, so a suspicious package with no outside corroboration is excluded. The counts should therefore be read as confirmed cases, not as every suspicious publish.

### Unit of record
Counts are per package version, but each package takes the date of its first malicious version. A package with several bad versions therefore adds to volume figures several times while keeping one date. Keeping this in mind stops a single long-running package from being misread as a new wave.

## Activity over time, counted by campaign
Monthly counts of confirmed malicious packages vary by a factor of fifteen across the period. The author attributes this to a small number of campaigns: in August 2026 a single dependency-confusion campaign accounts for 78% of confirmed versions. Counting each campaign as one actor, activity is stable: several hundred new publisher accounts per month, a flat rate of malicious packages relative to total packages published, and spikes that match named campaigns. Three changes remain after this normalization. Compromise of legitimate packages is episodic. Dependency-confusion reconnaissance is continuous at a consistent level. Obfuscation increased: the share of actors whose packages trigger an obfuscation rule more than doubled between May and August 2026.

### Campaign normalization
Normalization means grouping packages by the actor or campaign behind them before reading a trend. Without it, one burst of hundreds of versions looks like a rising threat. With it, the underlying publishing rate can be seen separately from the bursts. The article uses this step to show that steady activity sits underneath the headline spikes.

### Dependency confusion
Dependency confusion relies on attackers publishing packages under names an organization uses internally, in the hope that builds fetch them from the public registry. The article reports reconnaissance for it in every month, and it drives the August spike. Its delivery is described as builds that are misconfigured, rather than users choosing the package.

> 🔎 Added context: Build tools can prefer a public registry version of a name over a private one, which is the gap this attack uses. The article does not explain the mechanism itself.

> ❌ **Anti-pattern — Unprotected internal names:** A build configuration that lets an unscoped internal package name resolve against the public npm registry. **Why people do it:** the article does not say; default resolution works without setup. **Consequence:** an attacker-registered package with the same name is installed automatically, which the article says is how most attacker-created packages reach victims. **Fix:** see the added context below.

> 🔎 Added context: The article gives no fix. A common mitigation is to pin internal names to a private registry or a scope the organization controls.

## Compromised legitimate packages
Compromised legitimate packages are the other side of the picture. Three waves of one worm family occurred in May, June and August 2026. The May and August waves each compromised more than one hundred packages, and the June wave republished a vendor's entire scope in one day. Detail on the family follows in its own section below.

### Compromised legitimate package
A compromised package is a real, popular package whose publisher account was taken over, so the malicious version is published through the trusted name. Users who already depend on it install the malicious version without any choice on their part. The article's point is that these events are rare but reach far more installs than newly created malicious packages.

## Credential targeting
Targeting of AI-developer credentials stayed between 1% and 4% of packages all year, though absolute counts rose with overall volume. Theft of npm authentication tokens peaked at 22% of packages in March 2026 and fell below 5% from May. Classic long-lived tokens were revoked on December 9, 2025. The author reasons that the tokens targeted afterward were plausibly granular access tokens and CI secrets.

### npm authentication token
An npm token lets a tool publish packages without an interactive login, so a stolen token can publish as the maintainer. The article's data shows token theft peaking in March 2026 and then staying below 5% after May. That drop follows the December 2025 revocation, though the article does not claim the two are causally linked.

> 🔎 Added context: A granular access token is a narrower token that can be limited to specific packages and permissions, unlike the classic long-lived tokens that were revoked.

## Publisher accounts
99% of malicious packages are published by single-maintainer accounts, and 55% of those accounts use a free-mail address. Two thirds of accounts publish exactly one malicious package. Among accounts that publish more than one, the median gap from first to last malicious publish is under one day. DPRK-nexus campaigns are the exception: several hundred accounts, each with one or two packages, 83% on free-mail addresses. Other named campaigns use free-mail in 15% of cases, which the author reads as consistent with procured account pools.

### Single-maintainer account
A single-maintainer account is the only identity with publish rights for a package, so whoever controls it controls every release. The article finds 99% of malicious packages come from such accounts. Most of these accounts are used once, since two thirds publish a single malicious package, which points to disposable identities rather than long-term maintainers.

### Procured account pool
A procured account pool is a set of accounts obtained in bulk and spread across campaigns. The article describes the 15% free-mail rate for other named campaigns as consistent with this. The DPRK-nexus pattern of hundreds of accounts, each used for one or two packages, fits the same picture.

> 🔎 Added context: DPRK-nexus refers to campaigns linked to North Korean state-affiliated actors. The article uses the label without elaboration.

## Package metadata
88% of packages were under thirty days old at their malicious publish, and 75% declared no repository. 66% had a single version. 65% kept npm init template values in the author, description or repository fields, and 54% declared a lifecycle script. 17% carried an internal-looking name such as core, utils, sdk or config. Typosquatting is a minority tactic: 13% embed a popular package name, and under 5% are within two character edits of one.

### Lifecycle script
A lifecycle script is a command in package.json that npm runs automatically at certain points, such as during install. Because it runs without anyone invoking it, it is an early place for malicious code to execute. The article finds 54% of malicious packages declare one, and it becomes a key compromise signal: 78% of compromised versions add or modify one.

> 🔎 Added context: The package.json file is the manifest that npm reads to describe and install a package.

### Typosquatting
Typosquatting publishes a package whose name imitates a popular one, relying on a typo to catch installs. The article finds it is a minority tactic here, with 13% of packages embedding a popular name and under 5% within two character edits. Most of the set therefore does not depend on misspelling a well-known name.

## Compromise indicators
Compromised legitimate packages differ from attacker-created ones on version-level change signals. 78% of compromised versions add or modify a lifecycle script, against 4% of attacker-created packages. 23% show a change of maintainer or publisher. Most compromises go through the legitimate account and leave ownership records unchanged.

### Ownership record
An ownership record shows who maintains a package and who published each version. A change in it is a natural alarm for defenders. The article's finding is that most compromises leave it unchanged, because the attacker publishes through the real maintainer account, so a clean ownership history does not show a version is safe.

## Behavior
72% of packages carry an obfuscation marker, and 30% retrieve a second stage. Credential and data theft is the predominant behavior in every month except August. In August some packages stole credentials but aimed to deliver a Sliver command-and-control implant to the victim machine, so their classification changed to Trojan / Downloader.

### Credential stealer
A credential stealer collects secrets from the machine where the package is installed. The article reports that 45% of stealers target a single credential class and 36% target three or more. Single-class stealers spread evenly across cloud credentials, SSH keys, AI-developer credentials, npm tokens and .env files. In multi-class stealers, cloud credentials, SSH keys and .env files co-occur, while wallet and browser-data theft occur separately. AI-developer credentials are the most specialized target: 80% of packages that target them target no other class.

### Second-stage payload
A second-stage payload is code that a first package fetches after it runs, so the package that gets installed can stay small. The article reports that 30% of packages retrieve one. For review, this means inspecting only the installed package can miss what it fetches later.

### Sliver implant
A command-and-control implant gives an attacker a persistent channel to a compromised machine. In August some stealers went further and tried to deliver one. The change in classification to Trojan / Downloader reflects that step beyond theft.

> 🔎 Added context: Sliver is an open-source command-and-control framework. The article names it only as the implant the August packages aimed to deliver.

## Platform
55% of packages contain no operating-system-specific code. Among the rest, droppers target Windows through PowerShell, scheduled tasks and executables. Credential stealers and backdoors target Linux through systemd, cron and shell configuration files. macOS-specific code appears in about one in eight stealers.

### Dropper
A dropper is a package whose job is to deliver and start another payload rather than to steal anything itself. The article's split shows droppers leaning toward Windows mechanisms, while stealers and backdoors lean toward Linux persistence. Platform-specific code is therefore a clue to the role a package plays in an attack.

## Lures
24% of packages are scoped, spread across more than three hundred distinct scopes. Most of those scopes hold a single malicious package, which the article describes as an organization's internal namespace registered once. 36% declare an author and 24% a repository, and about 80% of each refer to an identity other than the publishing account. 62% include a README, and 16% of those contain an installation command for a different package, in half of cases a popular one. Most attacker-created packages use no lure, consistent with the finding that they are retrieved by misconfigured builds rather than chosen by users.

### Scoped package
A scoped package has a name of the form @scope/name, where the scope is a namespace owned by a user or organization. Scopes normally keep an organization's internal names distinct. Here attackers registered scopes that resemble an organization's internal namespace, which can catch builds that resolve those names.

> 🔎 Added context: The @scope/name form is the npm naming convention for scoped packages.

### Lure
A lure is anything that makes a package look legitimate or tempting, such as an author, a repository link or a README. The article finds lures in only some packages, and most attacker-created packages carry none. That suggests delivery depends more on build configuration than on persuading a developer to pick the package.

## The worm family and trusted publishing
Compromised legitimate packages are 7% of the set and over 99% of its download exposure. All compromise waves in the period belong to one family: first the toolkit of a single threat actor, later open-sourced for free use in May. The May and August worms each compromised more than one hundred packages. In June the same kit republished a single vendor's entire scope in one day through trusted publishing, followed within days by a few unrelated packages. Outside these waves, compromise runs at a few packages per month.

Worm-compromised versions are distinguishable from attacker-created packages. All execute through a lifecycle script, one third read cloud credentials or the Kubernetes service-account token, and 44% trigger obfuscation rules. They receive public advisories within hours of publication. The August wave's highest-exposure victims were republished through OIDC trusted publishing from compromised CI environments rather than stolen tokens. Across the period, 85% of packages in the malicious set published via trusted publishing are compromises. The December 2025 token revocation directed maintainers toward trusted publishing, and later compromise activity targeted the publishing pipeline directly.

### Trusted publishing
Trusted publishing lets a CI workflow publish to npm using an OIDC identity instead of a stored long-lived token. The article's data shows that moving to it did not remove the attack surface; it moved the target to the CI environment that publishes. The December 2025 revocation pointed maintainers toward this path, which makes the pipeline the thing to protect.

> ❌ **Anti-pattern — Treating trusted publishing as the fix:** Moving to trusted publishing and assuming the pipeline is safe, without protecting the CI environment that holds publish rights. **Why people do it:** the revocation pushed maintainers toward it, so it looks like the end state. **Consequence:** compromised CI environments republished the highest-exposure packages in the August wave, and 85% of trusted-publishing packages in the set are compromises. **Fix:** see the added context below.

> 🔎 Added context: The article gives no fix. A common mitigation is to harden the CI environment and limit which workflows and branches can publish.

## Shared legitimate services
38% of packages, and 42% of those with a network indicator, communicate with a legitimate third-party service. Serverless hosting is the most common, and Cloudflare Workers alone serves several hundred packages from a comparable number of accounts. Code-hosting and paste sites follow, then Telegram, Discord and Slack webhooks. A small population retrieves its payload address from a smart contract through a public blockchain RPC endpoint; two thirds of these are heavily obfuscated and 42% ship a compiled binary. 8% of droppers carry no resolvable host, because the second-stage URL is built at runtime.

### Serverless hosting
Serverless hosting runs code on a cloud provider's platform without the attacker managing a server. The article reports it as the most common legitimate service used, and Cloudflare Workers alone serves several hundred packages. Traffic to a well-known provider is harder to block by reputation alone than traffic to an unknown host.

> 🔎 Added context: Serverless hosting here means functions run by a provider, so the attacker needs no server of its own.

### Blockchain-hosted payload pointer
Some packages read their payload address from a smart contract through a public blockchain RPC endpoint. The article does not say why attackers choose this, but it means the pointer lives on a public chain rather than on a server the attacker controls. Two thirds of these packages are heavily obfuscated, and 42% ship a compiled binary.

## Exposure
81% of attacker-created packages had no downloads at scan time. The ten most-downloaded packages in the set account for over 99% of recorded downloads, and all ten are compromised legitimate packages. Attacker-created packages are the majority by count, but compromised packages are where the exposure sits.

### Download exposure
Download exposure is how many installs a package has received, or could receive. In this set it is highly concentrated: a few compromised, popular packages hold over 99% of recorded downloads. Ranking by downloads therefore shows where the risk is, while counting packages shows mostly volume.

## Time to public advisory
87% of the set has an OSV advisory. Named worm and dependency-confusion campaigns receive advisories on the day of publication. Unattributed packages receive advisories after a median of 1.7 days, with 10% waiting more than a month. DPRK-nexus packages take a median of 4.5 days, with 10% waiting more than two months, the slowest of any developer-facing population.

### OSV advisory
An OSV advisory is a public record of a malicious or vulnerable package in the OSV format. It is the signal that tells consumers and tools a package is known to be bad.

> 🔎 Added context: OSV is an open vulnerability database format that aggregates advisories across ecosystems.

### Publish-to-advisory lag
The lag is the time between a malicious publish and its first public advisory. It is the window in which a package can be installed before anyone warns about it. Named campaigns close the window within a day, while unattributed packages can stay exposed for a month or more.

## Registry outcome
Each attacker-created package was checked against the registry at analysis time. 40% had been replaced by npm's security placeholder, at a median of 7.1 days after publication, with 10% after more than 59 days. 37% had been unpublished by their own publisher, at a median of 0.3 days. Attackers withdraw packages faster than the registry removes them, and the share of self-removal rose through the year. 7% had been deleted without a recoverable timestamp, and 16% remained installable.

Removal varies by actor. 64% of DPRK-nexus packages were removed by npm, at a median lag of 19.5 days, and 5% remain installable; individual DPRK-nexus campaigns range from 16.4 to 65.9 days. Of unattributed packages, 21% remain installable. From March 2026 onward, the median advisory preceded the median takedown in every month, so the registry is the slower control.

### npm security placeholder
The security placeholder is npm's stand-in for a removed package, which the article counts among the registry's outcomes. 40% of attacker-created packages ended this way, typically after about a week, with a tail beyond two months.

> 🔎 Added context: The placeholder is a registry-side replacement, distinct from the publisher withdrawing the package.

### Takedown lag
Takedown lag is the time between publication and registry removal. The article finds it is usually longer than the advisory lag, so public reporting tends to come first. Its conclusion is that takedown is a slower control than public reporting, so defenders cannot rely on the registry to clear malware before warnings go out.

## Outcomes by campaign
Outcomes differ sharply by campaign. Campaigns with same-day advisories had median takedown lags of a few days or less. DPRK-nexus campaigns received advisories within days but remained installable for weeks to months; every DPRK-nexus tag in the dataset has a median takedown lag above two weeks. A further group, including five crypto-wallet stealer families, was never removed by the registry. Their publishers withdrew every package themselves, typically within a day, and the registry recorded no security action. The article closes by saying the next report will cover emerging threats that expand the npm registry's role beyond traditional malware distribution.

## 🧠 Mental models
### Count campaigns, not packages
**The model:** Most monthly variation comes from a few campaigns, so one actor's burst looks like a trend unless it is counted once.
**How to apply it:** Group detections by campaign before reading a trend, and check whether a spike is one actor or many.

### Rare compromise, outsized exposure
**The model:** Compromises of trusted packages are episodic, yet they reach almost all downloads, so risk concentrates in a few popular names.
**How to apply it:** Rank packages by downloads and watch version-level changes such as new lifecycle scripts on the most-used ones first.

## 🔑 Key takeaways
- Monthly malicious-package counts vary fifteen-fold, and one dependency-confusion campaign accounts for 78% of confirmed versions in August 2026.
- Compromised legitimate packages are 7% of the set but over 99% of its download exposure.
- 78% of compromised versions add or modify a lifecycle script, against 4% of attacker-created packages.
- 99% of malicious packages come from single-maintainer accounts, and 85% of trusted-publishing packages in the set are compromises.
- 81% of attacker-created packages had no downloads when scanned, and 37% were unpublished by their own publishers, median 0.3 days after publication.
- From March 2026 the median advisory preceded the median takedown in every month, so public reporting outpaced the registry.
- 16% of attacker-created packages remained installable at analysis time, and 64% of DPRK-nexus packages were removed by npm at a median lag of 19.5 days.

## 🏛️ For Kenji
In an architect's work this applies to dependency governance: which registries a build may use, whether internal package names are protected from public resolution, and how CI publish rights are scoped and monitored. The article suggests that compromised trusted packages and publishing pipelines carry more risk than new malicious packages. One question it raises: which of our build and publish pipelines would still publish safely if an attacker controlled one job's environment? (This is your interpretation, not the author's.)

## 📚 Reading plan
### From the article
- [Original article](https://www.databricks.com/blog/what-5400-malicious-npm-packages-reveal-about-software-supply-chain-risk-2026) — the full dataset, its figures and the series this first part opens.
- GitHub (2025), “npm classic tokens revoked, session-based auth and CLI token management now available” — the December 2025 revocation that starts the study period. The article gives no link for it.
### To go further
- npm trusted publishing (OIDC) documentation — how CI identity replaces stored publish tokens.
- OSV advisory format — how public advisories are structured and aggregated across ecosystems.
- npm scoped packages and registry resolution — how build tools choose between private and public registries.

## 🔗 Concepts
- [oidc](/concepts/security/oidc.md)
