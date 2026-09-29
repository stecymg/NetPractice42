# NetPractice

Résolution de dix scénarios de configuration réseau TCP/IP : adressage, masques de sous-réseau, tables de routage et passerelles par défaut, sur des topologies de complexité croissante.

Projet réalisé dans le cadre du cursus **École 42 Paris** (2023).

---

## Objectif

NetPractice est un exercice de diagnostic plutôt que de programmation. Chaque niveau présente une topologie partiellement configurée — machines, commutateurs, routeurs — et un objectif de connectivité à atteindre. Il faut déterminer les adresses, masques et routes manquants pour que les échanges demandés aboutissent, sans jamais modifier les éléments verrouillés par l'énoncé.

Les fichiers `levelN.json` de ce dépôt sont les configurations retenues pour chaque niveau.

---

## Notions travaillées

**Adressage et masques**

Un masque de sous-réseau partage une adresse IP en deux parties : celle qui identifie le réseau, et celle qui identifie la machine à l'intérieur. Deux machines ne peuvent communiquer directement que si elles appartiennent au même réseau — et ce calcul dépend autant du masque que de l'adresse. C'est la source d'erreur la plus fréquente des premiers niveaux : deux adresses visuellement proches, mais séparées par des masques incompatibles.

Deux adresses restent inutilisables dans chaque sous-réseau : celle qui désigne le réseau lui-même, et celle de diffusion. Un `/30` — masque `255.255.255.252` — ne laisse donc que deux adresses exploitables, juste assez pour une liaison point à point entre deux routeurs. Ce découpage revient à chaque niveau avancé.

**Commutateurs et routeurs**

Un commutateur relaie les trames à l'intérieur d'un même réseau et ne porte pas d'adresse IP. Un routeur relie des réseaux distincts et possède une adresse dans chacun d'eux, portée par une interface différente. C'est cette distinction qui détermine si un problème de connectivité relève de l'adressage ou du routage.

**Routage**

Chaque machine a besoin de savoir où envoyer les paquets destinés à l'extérieur de son réseau. La passerelle par défaut joue ce rôle, et elle doit impérativement se trouver dans le même sous-réseau que l'interface qui la déclare — contrainte qui élimine à elle seule une bonne partie des configurations erronées.

Les niveaux avancés introduisent des routes statiques explicites : plutôt qu'un unique itinéraire de sortie, on indique quel routeur atteindre pour chaque destination. Le routage y devient bidirectionnel — une réponse doit pouvoir revenir par un chemin valide, ce qui n'est pas automatique dès qu'on ajoute un second routeur.

---

## Progression

| Niveaux | Contenu |
|---|---|
| 1 – 3 | Adressage au sein d'un même réseau, rôle du commutateur |
| 4 – 6 | Masques de tailles variables, introduction de la passerelle par défaut |
| 7 – 9 | Routage entre plusieurs réseaux, cohérence aller-retour |
| 10 | Découpage en sous-réseaux, routes statiques, topologie multi-routeurs |

---

## Ce que le projet m'a apporté

- **Raisonner en binaire** — un masque ne se lit pas correctement en décimal. Convertir devient un réflexe, et les frontières de sous-réseau cessent d'être arbitraires.
- **Méthode de diagnostic** — partir de la liaison qui échoue, vérifier d'abord l'appartenance au même réseau, puis la route, plutôt que d'ajuster les valeurs au hasard.
- **Le routage est bidirectionnel** — une configuration peut permettre à un paquet de partir sans permettre à la réponse de revenir. Ce point n'apparaît qu'en testant les deux sens.
- **Des fondations réutilisables** — ces notions se retrouvent telles quelles dans la configuration d'un pare-feu, d'un VLAN ou d'un réseau Docker.
