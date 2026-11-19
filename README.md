# 🛒 Calendrier Traouquet — diffusion hebdo des permanences

> **Publication hebdo des permanences** — fini les captures d'écran de
> Framagenda à refaire sur le bon ordinateur : la page génère le tableau
> coloré **et** le message WhatsApp du week-end, à copier-coller en deux clics.

## ✅ Récapitulatif : ce qui est en place

| | |
|---|---|
| **Application en ligne** | https://fmoreau78470.github.io/Calendrier_Traouquet/ (ordi **et** mobile) |
| **Code** | https://github.com/fmoreau78470/Calendrier_Traouquet |
| **Fenêtre affichée** | mardi → lundi, par défaut **la semaine à publier** (qui commence le mardi suivant) |
| **Tableau** | cellules colorées 🟢 Permanent.e · 🟡 Joker s'est proposé.e · 🔴 Joker recherché.e (orthographe inclusive) |
| **Image PNG** | bouton « 🖼️ Copier le tableau en image » — prêt pour WhatsApp (netteté 2×, remplace la capture d'écran) |
| **Message WhatsApp** | bouton « 📋 Copier le message » — bonjour, planning de la semaine (arrêté au dernier jour ouvert), un 🔴 par joker recherché, formule inclusive (`Un.e joker est recherché.e…`), gras WhatsApp inclus |
| **Mise à jour des données** | automatique : Action GitHub **vendredi + samedi ~10 h 30 (Paris)** → cache ICS commité → Pages redéployé en ~1 min |
| **Droits** | lecture seule via le lien de partage Framagenda — toi seul modifies le calendrier |
| **Coût / serveur** | 0 €, aucun serveur dédié (Pages + Actions gratuits) |

## 💡 Recommandations d'usage

1. **Marque-pages sur les deux appareils** : l'URL Pages sur l'ordi *et* le mobile.
2. **iPhone : « Sur l'écran d'accueil »** (Partager → Ajouter à l'écran d'accueil) → la page s'ouvre comme une mini-app, tu n'auras plus jamais besoin de capture d'écran.
3. **⌘⇧R** (rechargement forcé) si la page semble en retard — c'est presque toujours le cache de Firefox ; VSC lui ne cache rien.
4. **Changement de planning tardif dans Framagenda** avant publication : clic sur **🔄 Récupérer l'ICS** pour rafraîchir tout de suite, sans attendre l'Action.
5. **Test rapide après un changement** : bouton image → ⌘V dans un message WhatsApp vierge pour constater que le PNG arrive bien.
6. **Publie samedi idéalement** : le message part au week-end, les jokers ont tout le temps de réagir avant le mardi ; semaine tout couverte → le message est positif et sans demande (🎉).

## Comment ça marche (architecture sans serveur)

```
Framagenda (lecture seule)
   │  export ICS anonyme :
   │  /remote.php/dav/public-calendars/<token>/?export
   ▼
GitHub Action (ven + sam 09:30 UTC) → met à jour le cache ics/perma.ics
   ▼
Page GitHub Pages (index.html) : parse, tableau coloré + message
   ▼
« 🖼️ Copier l'image » et/ou « 📋 Copier le message » → WhatsApp
```

Framagenda n'envoie pas d'en-tête CORS : la page ne peut pas lire l'ICS en
direct. La chaîne de secours automatique (direct → proxys publics →
**cache du dépôt**) garantit au moins une source ; depuis que le cache est
hébergé sur github.io, il est servi avec `access-control-allow-origin: *`
et suffit à lui seul.

## Droits et sécurité

- Lecture seule via le lien de partage ; les adhérents ne peuvent rien modifier.
- Couleurs Framagenda mappées **configurables** dans ⚙️ Réglages
  (`cadetblue` → Vert, `khaki` → Jaune, `palevioletred` → Rouge).
- Le dépôt est **public** : les prénoms du planning y sont visibles, à
  l'image du lien de partage Framagenda. Reverser en privé désactive
  Pages en gratuit.

## Contenu

- `index.html` — l'application (1 fichier, zéro dépendance, zéro build)
- `ics/perma.ics` — cache de l'ICS (à jour par l'Action)
- `.github/workflows/update-ics.yml` — mise à jour ven/sam + déclenchement manuel

## Publier une modification

```bash
git add -A && git commit -m "…" && git push   # Pages redéploie en ~1 min
```
