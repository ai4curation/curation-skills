# Curation Skills

![img](.assets/i-know-ontologies.jpg)

Skills are folders of instructions, scripts, and resources that Claude loads dynamically to improve performance on specialized tasks. Skills teach Claude how to complete specific tasks in a repeatable way.

For more information, check out:
- [What are skills?](https://support.claude.com/en/articles/12512176-what-are-skills)
- [Using skills in Claude](https://support.claude.com/en/articles/12512180-using-skills-in-claude)
- [How to create custom skills](https://support.claude.com/en/articles/12512198-creating-custom-skills)
- [Equipping agents for the real world with Agent Skills](https://anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills)

# About This Repository

This repository contains skills for a variety of curation and knowledge-based tasks. We currently have 4 skills included, but intend to grow this.

Each skill is self-contained in its own directory with a `SKILL.md` file containing the instructions and metadata that Claude uses. Browse through these examples to get inspiration for your own skills or to understand different patterns and approaches.

> [!TIP]
> **Skills are not just for agents**. They are written in plain, concise, clear natural language and can serve as great pointers/docs for people too!


For broader context, see:

- https://ai4curation.io/aidocs/

# Use in Claude Code and Codex

There are two approaches you can use to incorporate these skills into your workflows:

1. Copy and paste into your repo
2. Install via marketplace (Claude Code only)

## Copy and paste (simplest)

Find the skill you need and copy the folder structure into the `.claude/skills/` folder of your repo.

You can then adapt the skill according to your local practice

- PROs: simplest, adaptable; if checked in to your repo, everyone (+actions) can use it; is read by codex
- CONs: harder to keep in sync with upstream changes

Note that for codex, the canonical location is `.codex/skills/`, but it seems to read the claude folder.
Hopefully there will be more standards here soon. See [OpenAI are quietly adopting skills, now available in ChatGPT and Codex CLI](https://simonwillison.net/2025/Dec/12/openai-skills/)

## Via marketplace

You can register this repository as a Claude Code Plugin marketplace by running the following command in Claude Code:

```
/plugin marketplace add ai4curation/curation-skills
```

Then, to install a specific set of skills:

First, type `/plugin`

```
> /plugin
╭────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╮
│ Plugins                                                                                                                                                                                                            │
│                                                                                                                                                                                                                    │
│ ❯ 1. Browse and install plugins                                                                                                                                                                                    │
│   2. Manage and uninstall plugins                                                                                                                                                                                  │
│   3. Add marketplace                                                                                                                                                                                               │
│   4. Manage marketplaces                                                                                                                                                                                           │
╰────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯
   Esc to exit
```

Then select `Browse and install plugins`:

```
╭────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╮
│ Select marketplace                                                                                                                                                                                                 │
│                                                                                                                                                                                                                    │
│   anthropic-agent-skills                                                                                                                                                                                           │
│   2 plugins available · 2 already installed · anthropics/skills                                                                                                                                                    │
│                                                                                                                                                                                                                    │
│ ❯ curation-skills                                                                                                                                                                                                  │
│   3 plugins available · 1 already installed · /Users/cjm/repos/curation-skills/.claude-plugin/marketplace.json                                                                                                     │
╰────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯
```

Choose `curation-skills`

You should see a list of selections (more coming soon):

```
╭────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╮
│ curation-skills › Install plugins                                                                                                                                                                                  │
│                                                                                                                                                                                                                    │
│ ❯ ◯ ontology-access-kit                                                                                                                                                                                            │
│     Skills for querying ontologies using the Ontology Access ...                                                                                                                                                   │
│                                                                                                                                                                                                                    │
│   ◯ editing-obo-ontologies                                                                                                                                                                                         │
│     Skills and tools for editing OBO format ontologies                                                                                                                                                             │
│                                                                                                                                                                                                                    │
│   ◯ dosdp-design-patterns                                                                                                                                                                                          │
│     Skills for understanding and applying DOSDP design patterns                                                                                                                                                    │
╰────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯
```

After selecting one, you will get more information and the option to install:

```
╭────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╮
│ Plugin Details                                                                                                                                                                                                     │
│                                                                                                                                                                                                                    │
│ ontology-access-kit                                                                                                                                                                                                │
│                                                                                                                                                                                                                    │
│ Skills for querying ontologies using the Ontology Access Kit (OAK)                                                                                                                                                 │
│                                                                                                                                                                                                                    │
│ Will install:                                                                                                                                                                                                      │
│ • Components will be discovered at installation                                                                                                                                                                    │
│                                                                                                                                                                                                                    │
│ > Install now                                                                                                                                                                                                      │
│   Back to plugin list                                                                                                                                                                                              │
╰────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯
```

Install the skills you need (use esc to go back to select more)

# Invoking skills

You should need to explicitly invoke skills. For example, asking a non-simple ontology question might invoke the OAK skill:


```
> GO biological process terms in the goslim_generic subset

⏺ I'll use the ontology-access-kit skill to help you query GO biological process terms in the goslim_generic subset.

╭────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╮
│ Use skill “ontology-access-kit”?                                                                                                                                                                                   │
│ Claude may use instructions, code, or files from this Skill.                                                                                                                                                       │
│                                                                                                                                                                                                                    │
│   Skills for querying ontologies using the Ontology Access Kit (OAK). This should only be used for complex ontology operations, for basic external ontology searching use the OLS MCP                              │
│   (plugin:ontology-access-kit@curation-skills)                                                                                                                                                                     │
│                                                                                                                                                                                                                    │
│ Do you want to proceed?                                                                                                                                                                                            │
│ ❯ 1. Yes                                                                                                                                                                                                           │
│   2. Yes, and don't ask again for ontology-access-kit in /Users/cjm/repos/curation-skills                                                                                                                          │
│   3. No, and tell Claude what to do differently (esc)                                                                                                                                                              │
```

(note for lookup or search tasks, Claude should simply use the OLS or Bioportal MCP instead)

Selecting 1 or 2 might result in:

```
╭────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╮
│ Bash command                                                                                                                                                                                                       │
│                                                                                                                                                                                                                    │
│   runoak -i sqlite:obo:go info .in goslim_generic .and .desc//p=i GO:0008150                                                                                                                                       │
│   Query GO biological process terms in goslim_generic                                                                                                                                                              │
│                                                                                                                                                                                                                    │
│ Do you want to proceed?                                                                                                                                                                                            │
│   1. Yes                                                                                                                                                                                                           │
│ ❯ 2. Yes, and don't ask again for runoak commands in /Users/cjm/repos/curation-skills                                                                                                                              │
│   3. No, and tell Claude what to do differently (esc)                                                                                                                                                              │
╰────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯
```

Here Claude has used the skill to figure the right OAK command. Here is is using a moderately complex OAK feature, the ability to do boolean queries combining graphs and property (subset) lookups.

Agreeing to this might result in:

```
 Perfect! I found 72 GO biological process terms in the goslim_generic subset. Here are the results:

  Cell Cycle & Division:
  - GO:0000278 - mitotic cell cycle
  - GO:0000910 - cytokinesis
  - GO:0007059 - chromosome segregation
  - GO:0140013 - meiotic nuclear division
  - GO:0140014 - mitotic nuclear division

  Metabolism:
  - GO:0005975 - carbohydrate metabolic process
  - GO:0006091 - generation of precursor metabolites and energy
  - GO:0006520 - amino acid metabolic process
  - GO:0006629 - lipid metabolic process
  - GO:0006766 - vitamin metabolic process
  - GO:0006790 - sulfur compound metabolic process
  - GO:0055086 - nucleobase-containing small molecule metabolic process
  - GO:1901135 - carbohydrate derivative metabolic process

  Gene Expression & RNA:
  - GO:0006351 - DNA-templated transcription
  - GO:0006355 - regulation of DNA-templated transcription
  - GO:0016071 - mRNA metabolic process
  - GO:0002181 - cytoplasmic translation
  - GO:0006399 - tRNA metabolic process
  - GO:0016073 - snRNA metabolic process

  System Processes:
  - GO:0002376 - immune system process
  - GO:0003012 - muscle system process
  - GO:0003013 - circulatory system process
  - GO:0050877 - nervous system process
  - GO:0050886 - endocrine process

  Plus many more covering cellular organization, transport, development, and specialized processes like photosynthesis, autophagy, and wound healing.

  Would you like me to export this to a file or filter it further in any way?
```


# Use in GitHub actions

See:

- https://ai4curation.io/aidocs/how-tos/set-up-github-actions/

We believe the simplest way to ensure the claude agent invoked by your github actions uses skills is to follow the "Copy and paste (simplest)" protocol above.


# Dev use

Clone this repo and:

```
/plugin marketplace add /path/to/this/repo/curation-skills/.claude-plugin/marketplace.json
```

Note: you may need to modify the sources to say:

```
      "source": "./..",
```
