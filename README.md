# Calendrier Traouquet — diffusion hebdo des permanences

Génère, chaque week-end, le message WhatsApp de la semaine suivante (tableau
avec cellules colorées) à partir du calendrier Framagenda, **sans serveur
dédié et en 100 % gratuit**.

## Scénario (architecture sans serveur)

```
Framagenda (CalDAV public, lecture seule)
        │  export ICS anonyme :
        │  /remote.php/dav/public-calendars/<token>/?export
        ▼
1) Un fichier HTML unique (index.html) hébergé gratuitement
   (GitHub Pages, Netlify Drop ou Cloudflare Pages)
   → accessible depuis l'ordi ET le mobile, plus besoin de capture d'écran.
        │
        ▼
2) Le navigateur télécharge l'ICS, le parse et rend :
   • le tableau de la semaine (Vert / Jaune / Rouge), copiable en
     image PNG d'un clic (bouton « 🖼️ Copier le tableau en image »)
   • le message WhatsApp listant uniquement les jokers recherchés
        │
        ▼
3) Tu colles image et/ou message dans le groupe WhatsApp (seuls gestes manuels)
```

### Le point bloquant et sa solution

Framagenda n'envoie pas d'en-tête CORS : un script exécuté sur une page
github.io ne peut pas lire l'ICS en direct. Options gratuites, de la plus
robuste à la plus simple :

| Source | Mise en place | Fiabilité |
|---|---|---|
| **Cache dans le dépôt + GitHub Action** (retenu) | `.github/workflows/update-ics.yml` : tous les samedis 10 h (ET génération du message), une Action télécharge l'ICS et le commite ; la page github.io lit ce cache **same-origin** (CORS OK) | ★★★ |
| Proxy CORS public (api.cors.lol, allorigins…) | Aucune | ★ (instable, mais en secours automatique) |
| Copier/coller ou dépôt manuel du fichier .ics dans l'app | Aucune (secours en local) | ★★ |

La première fois, une action manuelle « 🔄 Récupérer l'ICS » utilise la chaîne
direct → proxy → cache ; ensuite l'Action GitHub maintient le cache seule.

### Droits et sécurité

- L'app utilise **uniquement le lien de partage public** du calendrier, en
  lecture seule → les adhérents ne peuvent pas modifier le calendrier (tu
  restes seul modificateur sur Framagenda ; il suffira de partager la
  maintenance de Framagenda avec l'autre personne, rien à changer ici).
- Le code couleur vient des `COLOR` des événements Framagenda, mappés
  **configurables** dans l'app (`cadetblue` → Vert, `khaki` → Jaune,
  `palevioletred` → Rouge).
- Le statut actuel du calendrier est bien : `???` rouge = joker recherché,
  événement kaki (recurrence exception) = joker qui s'est proposé,
  bleu-vert = permanent.

## Contenu du prototype

- `index.html` — l'application (1 fichier, zéro dépendance, zéro build)
- `ics/perma.ics` — cache de l'ICS (mis à jour par l'Action ou manuellement)
- `.github/workflows/update-ics.yml` — mise à jour hebdo automatique
- `perma.ics` — copie de travail d'analyse (peut être supprimée)

## Utilisation

1. Ouvrir `index.html` (ou l'URL GitHub Pages après push).
2. La semaine affichée par défaut est **la semaine suivante** (le message
   est envoyé le week-end qui précède) ; naviguer avec ‹ › si besoin.
3. **🖼️ Copier le tableau en image** et/ou **📋 Copier le message**
   (le message liste uniquement les jokers recherchés).
4. Coller dans le groupe WhatsApp ; ajouter éventuellement un commentaire.

Depuis un mobile, la page marche aussi : plus de capture d'écran à faire
depuis l'ordinateur.

Jours de fermeture (le lundi chez Traouquet) : la colonne correspondante
est masquée dans le tableau et l'image ; la case est cochable dans
⚙️ Réglages si les horaires changent.
