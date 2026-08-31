# ProdIA — Audit de productivité IA pour PME

SaaS d'audit de productivité IA destiné aux PME françaises. **ProdIA mesure ce que les outils IA (ChatGPT, Copilot, Midjourney, automations…) rapportent réellement — en heures, en euros, et chaque mois.**

Cabinet de productivité IA : audit en ligne gratuit, formules d'abonnement pour le pilotage durable, suivi mensuel du score et du ROI.

---

## Offres SaaS

| Formule | Prix | Contenu |
|---|---|---|
| **Starter** | **19 €/mois** | Audit complet d'1 service IA, score 0-100, gains estimés en euros, plan d'action 30-60-90 jours, export PDF, support email |
| **Pro** ⭐ *Le plus choisi* | **39 €/mois** | Audit complet de **5 services IA**, **suivi mensuel du score et du ROI**, mesure du temps gagné chaque mois, recommandations couper / garder / développer, point mensuel avec un conseiller |
| **Business** | **69 €/mois** | Tout le contenu Pro, audit **multi-sites** (jusqu'à 5 sites), **dashboard temps réel** consolidé, **rapport PDF personnalisé** (logo, mise en page), formation des équipes incluse |

- **Essai de 14 jours offert** sur toutes les formules — sans carte bancaire, sans engagement, résiliable en un clic.
- Paiement par virement ou lien sécurisé, après validation de la commande. Facture fournie pour chaque règlement.
- Toutes les formules sont sans engagement (résiliation par simple email).

---

## Fichiers du livrable

| Fichier | Rôle |
|---|---|
| `index.html` | Landing page — hero « Vos outils IA coûtent. Que vous rapportent-ils ? », 3 douleurs, méthode en 3 étapes (Auditer → Mesurer → Piloter), bénéfices, 3 offres, formulaire d'essai 14 jours (EmailJS), FAQ, footer. |
| `outil-audit.html` | **Outil réel** : audit de productivité IA — 15 questions sur 5 axes (usage réel, fréquence, gain estimé, coût, adoption), score 0-100, gains annuels en euros (heures × 35 €/h), verdict, plan d'action 30-60-90 jours, export PDF (`window.print`), persistance `localStorage`, envoi du rapport par email. |
| `chatbot-config.js` | Configuration du chatbot (nom ProdIA, accent or #d4a017, FAQ business, EmailJS). |
| `chatbot.js` | Widget chatbot autonome (réponses préprogrammées + capture de leads EmailJS). |
| `README.md` | Ce document. |

---

## Fonctionnement de l'outil d'audit

1. **Auditer** — 15 questions, 5 axes, 4 options chacune (valeurs 0 à 3). Environ 10 minutes, tout en ligne, sans inscription.
2. **Mesurer** — calculs instantanés :
   - **Score global 0-100** (somme des points / 45 × 100), score par axe (/100).
   - **Gains annuels en €** : heures gagnées par semaine (déclarées) × nombre d'utilisateurs × 44 semaines travaillées × **35 €/h** (taux horaire de référence).
   - **Coût annuel des outils** estimé à partir du budget mensuel déclaré ; **ROI** = gains / coût.
3. **Piloter** — verdict par palier (IA optimisée ≥ 80 / consolidée 65-79 / sous-exploitée 50-64 / à risque < 50) + plan d'action généré par axe et par palier : **Urgences 30 jours** (axes < 50), **Consolidation 60 jours** (50-74), **Excellence 90 jours** (≥ 75).

### Persistance et export

- Réponses sauvegardées en `localStorage` (`prodia_answers`, `prodia_entreprise`) à chaque clic : rechargement accidentel sans perte, boîte « Reprendre l'audit » sur l'écran d'accueil.
- **Export PDF** : bouton « Exporter le rapport » → `window.print()` avec rapport dédié (en-tête, score, tableau des 5 axes, chiffres clés, plan d'action, mentions).
- **Recevoir le rapport par email** : formulaire nom + email → EmailJS (template `template_xpo58cv`).

## Suivi mensuel maintenant réel (offre Pro)

La lacune critique « suivi mensuel promis par l'offre Pro » est **comblée** via le
portail client multi-business (`agentia-portail`, mission V6 + ProdIA) :

1. **Compte client** : chaque client a un espace sécurisé (auth JWT) dans le portail,
   business `prodia` (couleur or `#d4a017`, module `audits`).
2. **Outil connecté** : l'outil d'audit est servi par le portail sur `/audit` (même origine,
   token partagé). Après le score, le client connecté voit **« Enregistrer dans mon espace »**
   → sa snapshot (score, axes, gains €, coût, ROI, plan 30-60-90) est sauvegardée en base
   (`audit_snapshots`) via `POST /api/audits`. Les visiteurs sans compte gardent le formulaire
   EmailJS inchangé.
3. **Courbe d'évolution réelle** : pages « Mes audits » + « Évolution » — courbe du score /100,
   barres des gains €, ROI, et delta vs le mois précédent (Chart.js, données 100 % base).
4. **Multi-sites** (Business 69 €) : champ `site_name` par audit + sélecteur de site dans le
   portail (`GET /api/audits/history?site=`).
5. **Paiement** : bloc d'offres 19/39/69 € prêt pour Stripe Billing, **non activé** (feu vert requis).

Démo : `sophie@atelier-dupont.fr` / `client1234` dans le portail local
(`http://127.0.0.1:8000`) — 3 audits réels à des dates différentes (01/08, 15/08, 31/08)
→ courbe montante 54 → 71 → 78, gains 8 960 → 16 800 €/an, ROI ×6,2 → ×10.
Capture vente : `screenshots/evolution-score.png`.

---

## Intégrations

- **EmailJS (réel)** : service `service_cy1ytdb`, template `template_xpo58cv`, clé publique `8Pui4ZEqxW2jRVF7h`. Payload `{site, name, email, question}`.
  - Formulaire d'essai (index) : « Demande d'essai 14 jours ProdIA — Formule souhaitée : … — Situation : … ».
  - Envoi du rapport (outil) : « Demande du rapport d'audit ProdIA — Entreprise : … — Score : …/100 — Gains annuels estimés : … € ».
  - Chatbot : capture de leads quand une question n'a pas de réponse préprogrammée.
- **Chatbot** : widget autonome, accent or `#d4a017` (texte foncé `#0f3d4d` pour la lisibilité), nom « ProdIA », 6 FAQ business (suivi mensuel, offre Pro, prix, essai 14 jours, déroulé de l'audit, résiliation).
- **SEO** : Open Graph, Twitter Card, JSON-LD (`SoftwareApplication` + `FAQPage` sur la landing, `WebApplication` sur l'outil), balises meta françaises.
- **Accessibilité / confort** : animations `reveal` au scroll avec `prefers-reduced-motion`, clavier (boutons), `aria-pressed` sur les options.

---

## Design

- **Identité** : bleu pétrole `#0f3d4d` + or `#d4a017` + ivoire `#faf7f0` — ambiance cabinet SaaS analytics (grille fine or sur fonds pétrole, carte-score façon app, bande or en haut de page).
- **Typographie** : titres serif **Fraunces**, texte **Inter**, accents **JetBrains Mono** (étiquettes, chiffres, kickers).
- **Responsive** : breakpoints 1024 / 900 / 560 px, grilles repliables, aucun débordement horizontal.

---

## Déploiement

Statique, aucun build : déposer les fichiers sur n'importe quel hébergement statique (GitHub Pages, Netlify, Vercel…). Ne pas publier sur GitHub sans validation du propriétaire.

```bash
# Test local rapide
cd ~/Documents/livrables/audit-prod-ia
python3 -m http.server 8000
# → http://localhost:8000/index.html
```

---

## Contact

- Email : prodia.audit@gmail.com — réponse sous 24 h ouvrées.
- Cabinet français — audit à distance partout en France.
