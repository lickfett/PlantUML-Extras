# Auth0 Widget — Design Spec

**Date:** 2026-04-12  
**Status:** Approved

## Overview

Add an Auth0 entity widget to PlantUML-Extras following the Dreamhost-style pattern: two macro overloads — a color PNG variant and a monochrome sprite variant. Auth0 is a third-party identity provider, so it lives under `dist/misc/` alongside Zuplo and Dreamhost.

## File Structure

| File | Change |
|------|--------|
| `dist/common.puml` | Add `ExtrasMonoEntity` macro |
| `dist/misc/misc-extras.puml` | Add `Auth0EntityColoring` macro |
| `dist/misc/auth0/auth0.png` | New — PNG copied from `lickfett/gilbarbara-plantuml-sprites` |
| `dist/misc/auth0/all.puml` | New — `Auth0` macro definitions (color + mono overloads) |
| `samples/extras-test.puml` | Add Auth0 include and two usage examples |

## `common.puml` — `ExtrasMonoEntity`

Mirrors `AzureExtrasMonoEntity` from `azure-extras.puml`, but in `common.puml` so any misc widget can use it without pulling in Azure dependencies.

```plantuml
!definelong ExtrasMonoEntity(e_alias, e_label, e_techn, e_color, e_sprite, e_stereo)
rectangle "==e_label\n//<size:TECHN_FONT_SIZE>e_stereo</size>//\n<color:e_color><$e_sprite{scale=0.8}></color>\n//<size:TECHN_FONT_SIZE>e_techn</size>//" <<e_stereo>> <<extra>> as e_alias
!enddefinelong
```

## `misc-extras.puml` — `Auth0EntityColoring`

Uses Auth0's brand orange (`#EB5424`) for the border, consistent with how Zuplo uses its brand pink (`#FF007F`).

```plantuml
!definelong Auth0EntityColoring(e_stereo)
skinparam rectangle<<e_stereo>> {
    BackgroundColor DEFAULT_BG_COLOR
    BorderColor #EB5424
}
!enddefinelong
```

## `dist/misc/auth0/all.puml`

Sprite is generated from `auth0.png` using `plantuml -encodesprite 16 auth0.png`. Sprite name follows the `<Brand>Sprite` convention (cf. `AzureDNSSprite` in Dreamhost).

```plantuml
sprite $Auth0Sprite [WxH/16z] {
  <encoded sprite data — dimensions and content produced by plantuml -encodesprite 16 auth0.png>
}

Auth0EntityColoring(Auth0)

!definelong Auth0(e_alias, e_label, e_techn)
ExtrasEntity(e_alias, e_label, e_techn, Extras/misc/auth0/auth0.png, Auth0)
!enddefinelong

!definelong Auth0(e_alias, e_label, e_techn, e_color)
ExtrasMonoEntity(e_alias, e_label, e_techn, e_color, Auth0Sprite, Auth0)
!enddefinelong
```

### Usage

```plantuml
Auth0(auth0color, "auth0-tenant", "Identity Provider")           ' color PNG variant
Auth0(auth0mono,  "auth0-tenant", "Identity Provider", #EB5424)  ' mono sprite variant
```

## `samples/extras-test.puml` — Updates

Add to the includes block:
```plantuml
!includeurl Extras/misc/auth0/all.puml
```

Add before `@enduml`:
```plantuml
Auth0(auth0color, "auth0-tenant", "Identity Provider")
Auth0(auth0mono, "auth0-tenant", "Identity Provider", #EB5424)
```

## Implementation Notes

- **Sprite generation** requires the PlantUML CLI: `plantuml -encodesprite 16 auth0.png`. Run from `dist/misc/auth0/` after placing the PNG.
- The PNG source is `https://raw.githubusercontent.com/lickfett/gilbarbara-plantuml-sprites/master/pngs/auth0.png` — download and store at `dist/misc/auth0/auth0.png`.
- The `Extras` prefix in macro bodies resolves via the `!define Extras ...` at the top of consuming diagrams, same as Zuplo.
