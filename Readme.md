<div>
    <h1 align=center>
        <a href="https://git.io/typing-svg"><img src="https://readme-typing-svg.demolab.com?font=Fira+Code&size=22&pause=1000&random=false&width=435&lines=Azure+Security+Information+Model" alt="Typing SVG" /></a>
    </h1>
</div>

![header](/images/header.png)  

## About

We have a collection of Sentinel Blue maintained [Azure Security Information Model (ASIM)](https://learn.microsoft.com/en-us/azure/sentinel/normalization) normalization parsers.

Within the Azure Government region specifically we found need to maintain a stable baseline of ASIM for our Sentinels. This capability is something we want to share with whomever might find it useful. This repo will represent the upstream additions for consistency and usefulness within our SOC.

Normalizing data within Sentinel, or within any complex data environment, is advantageous for a number of reasons. Like..

- Enhance data analysis by normalized schemas
- Simplify alert creation by applying an alert to all data sources of a schema
- Documented sources by mapping to a well known schema definitions and format
- Move enrichments close to the dat a source and always apply enrichments where possible
- Parsers provide flexibility by placing normalization outside of extract-transform-load (ETL) pipelines

## Philosophy

🔵 Maintain consistency with Microsoft published schemas.

🔵 Additional parameters or content do not 'break' deployed alerting.

🔵 Utilize Semantic versions for our schemas/parsers to maintain versions.

## Highlights and Additions

> [!NOTE]
> We have made additions to extend the capability of the ASIM parsers. These additions will be 'on top' of the ASIM parsers provided by Microsoft.
> While utiliznig the ASIM parsers during analysis and alerting we saw the opportunity to expand the functionality.

Top 'extra' features:

✅ Pivot queries to copy-paste relevant next-hop or relevant queries.

✅ Additional parameters for the schema functions/parsers

✅ "Lookup" links in columns for relevant and well-known entities (e.g. abuse db IP lookup)

✅ Enrichments wherever possible (e.g. login codes mapped to relevant description)

### Notes

### File Types

We have found it useful to separate the raw KQL from the YAML definition files. You'll see the names of the files match except for the extensions. Automation will combine the files for ARM template creation or deployment.

### Add-On Global Parameters

> [!WARNING]
> Each schema has unique parameters added.

| New Parameter | Description |
| - | - |
| pivot_lookback | Sets the lookback time in the pivot queries provided in each event. Defaults to a value of 30 days.

## Development

Next steps ahead...

- [ ] Add Deploy to Azure buttons for all parsers
- [ ] Add all parsers
  - [ ] Audit
  - [x] Authentication
  - [ ] DNS
  - [ ] DHCP
  - [ ] File
  - [ ] Network
  - [ ] Process
  - [ ] Registry
  - [ ] User Management
  - [ ] Web Session

## Parser Directory

These are all of the ASIM parsers that are stored in this repository.

| Parser | Schema | Schema Version | Description | Deploy
| - | - | - | - | - |
| [Authentication Event](/AuthenticationEvent/README.md) | Authentication | 0.1.3 | Normalize authentication events across Entra ID data sources. | ![Deploy to Azure](https://aka.ms/deploytoazurebutton)