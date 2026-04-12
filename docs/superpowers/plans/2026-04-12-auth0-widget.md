# Auth0 Widget Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add an Auth0 entity widget to PlantUML-Extras with a color PNG variant and a monochrome sprite variant, following the established Dreamhost pattern.

**Architecture:** `ExtrasMonoEntity` is added to `common.puml` as the generic mono rendering macro. An `Auth0EntityColoring` macro (brand orange border) is added to `misc-extras.puml`. The `dist/misc/auth0/` directory holds the PNG asset and `all.puml` macro definitions. The sprite is embedded inline in `all.puml`, generated from the PNG using the PlantUML CLI.

**Tech Stack:** PlantUML macros (`!definelong`/`!enddefinelong`), PlantUML CLI (`plantuml -encodesprite`), `curl` for asset download.

---

## File Map

| File | Action |
|------|--------|
| `dist/common.puml` | Modify — add `ExtrasMonoEntity` |
| `dist/misc/misc-extras.puml` | Modify — add `Auth0EntityColoring` |
| `dist/misc/auth0/auth0.png` | Create — downloaded from gilbarbara fork |
| `dist/misc/auth0/all.puml` | Create — `Auth0` macro definitions |
| `samples/extras-test.puml` | Modify — add include + two usage examples |

---

### Task 1: Add `ExtrasMonoEntity` to `common.puml`

**Files:**
- Modify: `dist/common.puml`

- [ ] **Step 1: Open `dist/common.puml` and append the new macro**

The file currently ends after the `ExtrasEntity` definelong block. Add `ExtrasMonoEntity` immediately after it. The final file should read:

```plantuml
!define TECHN_FONT_SIZE 12
!define DEFAULT_BG_COLOR #f2f2f2
!define DEFAULT_BORDER_COLOR #000000

hide <<extra>> stereotype

skinparam defaultTextAlignment center

!definelong ExtrasEntityColoring(e_stereo)
skinparam rectangle<<e_stereo>> {
    BackgroundColor DEFAULT_BG_COLOR
    BorderColor DEFAULT_BORDER_COLOR
}
!enddefinelong

!definelong ExtrasEntity(e_alias, e_label, e_techn, e_img, e_stereo)
rectangle "==e_label\n//<size:TECHN_FONT_SIZE>e_stereo</size>//\n<img:e_img{scale=0.8}>\n//<size:TECHN_FONT_SIZE>e_techn</size>//" <<e_stereo>> <<extra>> as e_alias
!enddefinelong

!definelong ExtrasMonoEntity(e_alias, e_label, e_techn, e_color, e_sprite, e_stereo)
rectangle "==e_label\n//<size:TECHN_FONT_SIZE>e_stereo</size>//\n<color:e_color><$e_sprite{scale=0.8}></color>\n//<size:TECHN_FONT_SIZE>e_techn</size>//" <<e_stereo>> <<extra>> as e_alias
!enddefinelong
```

- [ ] **Step 2: Commit**

```bash
git add dist/common.puml
git commit -m "feat: add ExtrasMonoEntity to common.puml"
```

---

### Task 2: Add `Auth0EntityColoring` to `misc-extras.puml`

**Files:**
- Modify: `dist/misc/misc-extras.puml`

- [ ] **Step 1: Append `Auth0EntityColoring` to `misc-extras.puml`**

The file currently contains only `ZuploEntityColoring`. Add `Auth0EntityColoring` after it. The final file should read:

```plantuml
!definelong ZuploEntityColoring(e_stereo)
skinparam rectangle<<e_stereo>> {
    BackgroundColor AZURE_BG_COLOR
    BorderColor #FF007F
}
!enddefinelong

!definelong Auth0EntityColoring(e_stereo)
skinparam rectangle<<e_stereo>> {
    BackgroundColor DEFAULT_BG_COLOR
    BorderColor #EB5424
}
!enddefinelong
```

- [ ] **Step 2: Commit**

```bash
git add dist/misc/misc-extras.puml
git commit -m "feat: add Auth0EntityColoring to misc-extras.puml"
```

---

### Task 3: Download auth0.png

**Files:**
- Create: `dist/misc/auth0/auth0.png`

- [ ] **Step 1: Create the directory and download the PNG**

```bash
mkdir -p dist/misc/auth0
curl -L -o dist/misc/auth0/auth0.png \
  https://raw.githubusercontent.com/lickfett/gilbarbara-plantuml-sprites/master/pngs/auth0.png
```

- [ ] **Step 2: Verify the file downloaded correctly**

```bash
file dist/misc/auth0/auth0.png
```

Expected output contains: `PNG image data`

- [ ] **Step 3: Commit**

```bash
git add dist/misc/auth0/auth0.png
git commit -m "feat: add auth0.png asset"
```

---

### Task 4: Generate the Auth0 sprite

**Files:**
- Read: `dist/misc/auth0/auth0.png` (input)
- Will be used in: `dist/misc/auth0/all.puml` (next task)

> **Prerequisite:** Task 3 must be complete. The PlantUML CLI must be available as `plantuml` in your PATH (or `java -jar /path/to/plantuml.jar`).

- [ ] **Step 1: Generate the sprite from the PNG**

Run from the repo root:

```bash
plantuml -encodesprite 16 dist/misc/auth0/auth0.png
```

- [ ] **Step 2: Capture the output**

The command prints to stdout. It will look like this (dimensions and data will differ):

```
sprite $auth0 [64x64/16z] {
xSe...
...many lines of encoded data...
}
```

Copy the full output. You will use it in Task 5, with two changes:
1. Rename `$auth0` → `$Auth0Sprite`
2. Keep the `[WxH/16z]` dimensions exactly as printed

No commit in this task — the sprite data flows directly into Task 5.

---

### Task 5: Create `dist/misc/auth0/all.puml`

**Files:**
- Create: `dist/misc/auth0/all.puml`

> **Prerequisite:** Task 4 must be complete. You need the sprite data on your clipboard/in a scratch buffer.

- [ ] **Step 1: Create `dist/misc/auth0/all.puml`**

Replace the `sprite` block below with the actual output from Task 4 (renaming `$auth0` to `$Auth0Sprite`):

```plantuml
sprite $Auth0Sprite [WxH/16z] {
<paste encoded sprite lines here — copy from plantuml -encodesprite output, rename $auth0 to $Auth0Sprite>
}

Auth0EntityColoring(Auth0)

!definelong Auth0(e_alias, e_label, e_techn)
ExtrasEntity(e_alias, e_label, e_techn, Extras/misc/auth0/auth0.png, Auth0)
!enddefinelong

!definelong Auth0(e_alias, e_label, e_techn, e_color)
ExtrasMonoEntity(e_alias, e_label, e_techn, e_color, Auth0Sprite, Auth0)
!enddefinelong
```

- [ ] **Step 2: Commit**

```bash
git add dist/misc/auth0/all.puml
git commit -m "feat: add Auth0 widget macros"
```

---

### Task 6: Update `samples/extras-test.puml` and verify rendering

**Files:**
- Modify: `samples/extras-test.puml`

- [ ] **Step 1: Add the Auth0 include to the includes block**

In `samples/extras-test.puml`, add the Auth0 include alongside the other misc includes (after `!includeurl Extras/misc/dreamhost/all.puml`):

```plantuml
!includeurl Extras/misc/auth0/all.puml
```

- [ ] **Step 2: Add Auth0 usage examples before `@enduml`**

Add these two lines before the closing `@enduml`:

```plantuml
Auth0(auth0color, "auth0-tenant", "Identity Provider")
Auth0(auth0mono, "auth0-tenant", "Identity Provider", #EB5424)
```

- [ ] **Step 3: Render the sample diagram to verify no errors**

Run from the repo root (the `!define Extras dist` line in the file makes PlantUML resolve includes locally):

```bash
plantuml samples/extras-test.puml
```

Expected: exits with no errors, produces `samples/extras-test.png`.

Open `samples/extras-test.png` and confirm:
- An `auth0-tenant` rectangle appears with the Auth0 logo PNG (color variant)
- A second `auth0-tenant` rectangle appears with an orange monochrome sprite (mono variant)
- Both have an orange (`#EB5424`) border

- [ ] **Step 4: Commit**

```bash
git add samples/extras-test.puml
git commit -m "feat: add Auth0 widget to extras-test sample"
```
