🛡️ Supervision Réseau avec NetFlow/sFlow couplée à ELK et NtopNG


Mise en place d'une solution de supervision réseau open-source pour l'analyse et la sécurisation du trafic, combinant deux chaînes de traitement complémentaires : ELK et NtopNG.


[Status](https://img.shields.io/badge/status-completed-brightgreen)
[License](https://img.shields.io/badge/license-MIT-blue)
[Made with](https://img.shields.io/badge/made%20with-Ubuntu%20%7C%20ELK%20%7C%20NtopNG-orange)


📌 Contexte

Ce projet a été réalisé dans le cadre de mon année académique 2025-2026 à l'Institut Supérieur d'Informatique (ISI), sous la direction de M. Ahmed Ould KHALIFA.

L'objectif est de concevoir et déployer une architecture de supervision réseau capable de collecter, centraliser, analyser et visualiser en quasi temps réel les flux NetFlow/sFlow d'une infrastructure, en combinant les forces complémentaires d'une solution généraliste (ELK) et d'une solution spécialisée dans l'analyse de trafic (NtopNG).


🎯 Objectifs


Présenter le cadre théorique des protocoles NetFlow, sFlow et IPFIX
Concevoir une architecture de collecte, stockage et visualisation robuste et évolutive
Déployer un laboratoire fonctionnel de bout en bout
Détecter des comportements suspects (scans, anomalies) via l'analyse de flux
Comparer la solution open-source aux outils commerciaux



🏗️ Architecture

Routeurs/Switch (NetFlow) ──UDP/2055──┐
                                        ├──► netflow2ng ──ZMQ──► NtopNG (UI :3000)
Switch/Pare-feu (sFlow) ──UDP/6343────┘

Flux dupliqués ──► Logstash ──► Elasticsearch ──► Kibana (UI :5601)

Les deux chaînes sont alimentées par les mêmes flux, dupliqués à la source, et fonctionnent de manière totalement indépendante : la panne de l'une n'affecte pas l'autre.


🛠️ Stack technique

ComposantRôleNetFlow / sFlowProtocoles d'export de flux (routeurs, switches)softflowdGénération de flux NetFlow depuis une machine clientenetflow2ngCollecteur NetFlow/IPFIX open-source (alternative gratuite à nProbe)NtopNGVisualisation temps réel + identification applicative (nDPI)LogstashIngestion et décodage des fluxElasticsearchIndexation et stockage des fluxKibanaTableaux de bord interactifs et alertingGNS3Virtualisation d'un routeur Cisco IOS et d'un switch Open vSwitch


🖥️ Topologie du laboratoire

VMRôleIPRessourcesntop-srvnetflow2ng + ntopng + ClickHouse192.168.10.102 vCPU / 3 Go RAMelk-srvElasticsearch + Logstash + Kibana192.168.10.202-3 vCPU / 6-7 Go RAMclient-testGénération de trafic et export simulé192.168.10.501 vCPU / 1-2 Go RAM


⚔️ Tests et scénarios réalisés


📊 Trafic volumétrique (iperf3) — flux visibles en moins de 60s dans ntopng
🌐 Identification applicative (HTTP, HTTPS/SNI, DNS, SSH) via nDPI
🔍 Balayage de ports (Nmap) — détection en temps réel : alerte "possible port scan"
🔁 Comparaison NetFlow vs sFlow — comptage exact vs approché par échantillonnage
💥 Test de résilience — panne simulée de Logstash sans impact sur ntopng



🔐 Sécurité et détection d'anomalies

L'analyse de flux permet de détecter, sans inspection du contenu des paquets :

ComportementSignature réseauBalayage de portsFlux courts et nombreux vers des ports distinctsExfiltration de donnéesVolume sortant anormal vers destination rareBeaconing C2Connexions régulières, faible volume, même destinationAttaque DDoSPic soudain de flux depuis de nombreuses sources


📈 Indicateurs de performance observés


Latence d'apparition d'un flux dans ntopng : quelques secondes
Charge CPU (softflowd/ntopng) : < 5% sur un cœur en régime nominal
Volume Elasticsearch : ~1,2 Go / 48h de trafic de test modéré



📂 Contenu du dépôt

├── docs/               # Rapport complet (PDF)
├── configs/            # Fichiers de configuration (netflow2ng, logstash, ntopng)
├── scripts/            # Scripts d'installation et de test
└── README.md


🚀 Perspectives


Intégration à un SIEM/SOC pour une détection avancée
Automatisation de la réponse aux incidents (SOAR)
Détection d'anomalies par Machine Learning
Migration progressive vers IPFIX (standard ouvert)



📄 Rapport complet

Le rapport détaillé (théorie, mise en œuvre pas-à-pas, résultats, recommandations) est disponible dans le dossier docs/.


🙏 Remerciements

Un grand merci à mon mentor Massamba LO pour son soutien, ses orientations et la confiance qu’il nous a accordée tout au long de ce projet, nous permettant d’aller plus loin dans notre progression.

Un grand merci également à mon encadrant M. Ahmed Ould KHALIFA pour son accompagnement, ses conseils avisés et sa disponibilité tout au long de ce projet,


👤 Auteur

Ayoub Awwale BILE
Étudiant à l'Institut Supérieur d'Informatique (ISI) — Année Académique 2025-2026

📫 N'hésitez pas à me contacter sur LinkedIn pour toute question sur ce projet.
