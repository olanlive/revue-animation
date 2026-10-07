# Pépites Animation

Repo : `revue-animation`

Veille **animation** en français — open source d’abord (gratuit / payant signalé).

Site statique (GitHub Pages) : **https://olanlive.github.io/revue-animation/**

Flux RSS 2.0 : **https://olanlive.github.io/revue-animation/feed.xml** (un item par découverte, lien vers l’ancre du bloc)

Cadence : **tous les jours**, week-end compris, à **9h51 Europe/Paris** (cron `51 9 * * *`, décalé de la revue 3D de 9h21) — 3 à 7 pépites par passage, **rien publié s’il n’y a rien de neuf**.

## Priorités

1. Outils et logiciels d’animation (open source mis en avant)
2. Add-ons Blender d’animation et de rigging
3. Rigging, pickers, retargeting
4. Mocap / capture vidéo (markerless, vidéo → animation)
5. Animation 2D : Grease Pencil, OpenToonz, Tahoma2D, Krita, Synfig, Pencil2D…
6. Nouveautés notables des releases (Blender, Krita, OpenToonz…)

Chaque pépite doit avoir une **vraie actualité** (release récente, projet découvert récemment), une **URL officielle vérifiée** et une **source réelle** (release notes, annonce, post forum/X/Reddit, article). Ne rien inventer.

Avant d’ajouter : vérifier [`COVERED.md`](./COVERED.md) (généré) et les `data/discoveries.json` des autres revues Pépites pour éviter les doublons. Hors périmètre : 3D/VFX généraliste (`olanlive/revue-oss-3d`), audio/vidéo (`olanlive/revue-audio-video`), outils IT (`olanlive/revue-outils-it`).

## Résumés

**Résumé court et percutant : 1 à 2 phrases, 280 caractères max** (le build avertit au-delà). Ce que fait l’outil ou ce qui est neuf dans cette version, avec la date de l’actu. Mention très courte (« bêta », « payant », « usage non commercial ») seulement si essentielle.

## Ajouter une découverte

1. Éditer [`data/discoveries.json`](./data/discoveries.json) : ajouter un objet (en tête de préférence ; le build trie par date décroissante, ordre conservé à date égale) :

```json
{
  "date": "2026-10-09",
  "name": "Nom de l’outil 1.2.3",
  "summary": "Ce que fait l’outil / ce qui est neuf (sortie le 8 oct). 1–2 phrases, 280 caractères max.",
  "url": "https://site-officiel…",
  "tags": ["blender-addon", "rigging"],
  "source": { "label": "Release GitHub — owner/repo 1.2.3", "url": "https://github.com/…/releases/tag/…" }
}
```

2. Rebuild (régénère `docs/`, dont `docs/feed.xml`, et `COVERED.md`) :

```bash
python3 scripts/build.py
```

3. Commit + push :

```bash
git add -A && git commit -m "Pépites du AAAA-MM-JJ : …" && git push origin main
```

GitHub Pages (branche `main`, dossier `/docs`) se redéploie tout seul.

## Tags (vocabulaire)

Tags kebab-case, sans accents, à réutiliser de façon cohérente :

| Tag | Usage |
|-----|--------|
| `blender` | Nouveauté de Blender lui-même (release notes) |
| `blender-addon` | Add-on / extension Blender |
| `grease-pencil` | Grease Pencil |
| `2d` | Animation 2D (dessin, cut-out, Xsheet) |
| `krita` | Krita |
| `opentoonz` | OpenToonz / Tahoma2D |
| `rigging` | Rigging, contraintes, rotations, skinning |
| `picker` | Pickers / sélection de contrôleurs |
| `keyframing` | Keyframes, courbes, timing, playback |
| `retargeting` | Retargeting d’animation |
| `mocap` | Motion capture |
| `capture-video` | Capture depuis la vidéo (markerless) |
| `stop-motion` | Stop motion |
| `open-source` | Projet open source (à mettre en avant) |
| `payant` | Outil payant / freemium |
| `beta` | Bêta / RC / préversion |
| `emerging` | Projet jeune / early |

## Architecture

- `data/discoveries.json` — fil plat de découvertes (éditable ; champs `date`, `name`, `summary`, `url`, `tags`, `source {label, url}`)
- `scripts/build.py` — génère le HTML dans `docs/` et `COVERED.md`
- `docs/` — site publié (Pages depuis `main` / dossier `/docs`) : `index.html` (fil unique, plus récent en haut, un séparateur par jour), `tags/*.html`, `feed.xml` (RSS 2.0, 100 dernières découvertes)
- `COVERED.md` — liste générée des outils déjà couverts (anti-doublons)

Chaque bloc `<article class="discovery">` a une ancre stable `id="AAAA-MM-JJ-nom-version"` (lien direct, utilisé par le flux RSS). Chaque jour a son séparateur `<h2 class="day-sep" id="jour-AAAA-MM-JJ">`.

Pas de npm. Python 3 standard library uniquement.

## Thème

Thème **clair jaune** (bandeau d’en-tête jaune `#ffd60a`, fond beurre) — volontairement distinct du bleu de la revue 3D, du vert de la revue Audio & Vidéo et de l’ambre sombre de la revue Outils IT. Contrastes WCAG AA vérifiés :

| Couleur | Fond | Ratio |
|---|---|---|
| Texte `#1d1a0e` | fond `#fff7cc` / carte `#fffdf2` | 16.11:1 / 17.06:1 |
| Texte secondaire `#57491a` | fond / carte | 8.19:1 / 8.67:1 |
| Liens `#665000` | fond / carte | 7.16:1 / 7.59:1 |
| Liens survol `#3d3000` | fond / carte | 11.98:1 / 12.70:1 |
| Titre `#1d1a0e` | bandeau `#ffd60a` | 12.33:1 |
| Accroche `#3b3000` | bandeau `#ffd60a` | 9.25:1 |
| Tags `#3b2f00` | `#ffe066` | 10.11:1 |
| Tags survol `#2a2100` | `#ffd21f` | 11.01:1 |
| Traits / séparateurs `#9c7a00` (non-texte) | fond | 3.74:1 |

Les liens dans du texte courant (source, footer) sont soulignés pour ne pas reposer sur la seule couleur.

## Source d’une découverte

Champ `source` : où la pépite a été repérée, affiché sous le résumé (« Trouvé via : … »).

Libellés usuels : « Release GitHub — owner/repo x.y.z », « Blender Extensions — nom », « Devtalk — titre », « Forum — projet », « Fil X — @compte », « arXiv — id », « Reddit — r/sub », « Web — site ».

## Licence

Notes de veille (liens vers les projets upstream, chacun avec sa propre licence).
