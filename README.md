# 🗄️ Conception d'une architecture DFS

> Serveurs de fichiers redondés, répliqués via DFS Replication, avec ACL NTFS alignées sur les groupes de sécurité Active Directory.

## Architecture retenue

Deux VM **Windows Server 25**, hébergées sur l'hyperviseur ACE, jouent le rôle de filers répliqués via **DFS Replication**. Le gestionnaire DFS est implanté sur `h-winserv-2`. Cette architecture garantit une haute disponibilité du service de fichiers et une bascule transparente côté client en cas d'indisponibilité de l'un des deux serveurs `a-winserv-2` et `a-winserv-3` .

## Arborescence de partage

L'arborescence respecte l'organigramme RAID-A-PORTER : chaque service métier dispose d'un dossier racine dont les ACL NTFS sont alignées sur les groupes `_R` (lecture), `_M` (modification) et `_G` (gestion) définis côté Active Directory (voir `ad-structuration-annuaire`).

## Mise en production progressive

La mise en production a démarré par le service **Direction Générale**, permettant de vérifier le bon fonctionnement de la réplication DFS et de la résolution des chemins UNC (`\\raidaporter.local\<service>`) avant généralisation aux autres services.

## Aperçu

<img width="1746" height="1004" alt="dfs" src="https://github.com/user-attachments/assets/e065a534-2eae-4360-9ffc-d1c506933fbe" />

## Pourquoi DFS plutôt qu'un filer unique

Un filer unique constitue un point de défaillance unique (SPOF). La réplication DFS entre deux VM sur le même hyperviseur physique n'élimine pas tous les risques (panne de l'hôte), mais valide la mécanique de réplication et de bascule côté client avant une éventuelle extension multi-hôtes.

## Repos liés

- `ad-structuration-annuaire` — source des groupes de sécurité
- `dfs-habilitations-powershell` — automatisation des ACL
- `cups-scan-to-folder` — consommateur de cette arborescence

## Auteur

**Lilian Vertueux** — [LinkedIn](https://www.linkedin.com/in/lilian-vertueux/)
