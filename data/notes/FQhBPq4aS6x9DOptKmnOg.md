
## Dendron Usage

```
.
└── workspace
    ├── vault.main
    │   ├── foo.md
    │   ├── foo.one.md
    │   └── foo.two.md
    └── vault.secret (hypothetical)
        ├── secret.one.md
        └── secret.two.md
```


## Workspace
Workspace level means the directory that holds `dendron.yml` and/or `dendron.code-workspace`

## Vault
Vault level is the directory that holds `root.md` and possibly holds every other md files
* to add vault -> `C-S-P`[^keynotation] Dendron:Vault Add
## Naming Convention
Markdown note file naming convention: kebab case
* `dendron.docs.mash-potato.md -> title: Mash Potato`

[^keynotation]: [[key notation|key.notation]]


## Schema
It is like a rule for folder structure. It describes your notes hierachy in a parent-children relations
The rule can be used for auto-completion when creating/finding new notes for the specific file path
e.g. `recipe.vege.potato.mash-potato.md`
it is under the schema of:
* `recipe.`
* `recipe.vege.`
* `recipe.vege.potato.`
* `recipe.vege.potato.mash-potato.`
With Schema, you can add templates with matching schema
And in xxx.schema.yml file, you must define every group. Cannot list the children id without defining its entry.
`namespace: true` means it has arbitrary children. Meaning it has another middle '*' level before defined `children` level.
```yml
version: 1
imports: [] # can import other schema file (with filename)
schemas:
  - id: media # the name that has to match with the file name
    title: media # a title you give to this schema (can be diff from id)
    parent: root # parent of this file. at least one has to set `root`
    children: # children of this file
      - video
      - audio
  - id: video # you can separate each group by each item
    title: video
    children:
      - codec
  - id: audio
    title: audio
    children:
      - codec
  - id: codec
    namespace: true # this matches the immediate level of children -> codec.xxx, (but not codec.xxx.abc)
    template:
      id: templates.media.codec
      type: note
```

