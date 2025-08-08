# Infrastructure Sécurisée - Projet de groupe Docker Compose

## Objectif

Déployer une infrastructure sécurisée avec :
- **WAF** (ModSecurity)
- **Reverse Proxy** (Traefik)
- **IDS/IPS** (CrowdSec)
- **solution SIEM** (ELK Stack)
- **monitoring** (Prometheus + Grafana)
- **application métier** : Nextcloud

Le tout orchestré avec **Docker Compose**,

---

## Stack utilisée

| Composant     | Outil utilisé            |
|---------------|--------------------------|
| Reverse Proxy | Traefik                  |
| WAF           | Nginx + ModSecurity      |
| App métier    | Nextcloud                |
| Base de données | MySQL                  | 
| IDS/IPS       | CrowdSec + Bouncer       | 
| Monitoring    | Prometheus + cAdvisor    |
| Dashboard     | Grafana                  |
| Cache         | Redis                    |
| SIEM          | ELK (Elasticsearch, Logstash, Kibana)|

---

## Arborescence

.
├── config/
│   ├── crowdsec/
│   ├── logstash/
│   ├── prometheus/
│   └── waf/
├── dashboards/
│   ├── grafana_json/
│   └── kibana_export.ndjson
├── data/
│   ├── db/
│   └── nextcloud/
├── docs/
│   ├── architecture.png
│   └── rapport.md
├── tests/
│   ├── sqli.ps1
│   └── xss.ps1
├── docker-compose.yml
└── README.md


---

## Architecture simplifiée

              [ Client Web ]
                     │
          ┌──────────▼──────────┐
          │    Traefik Gateway  │
          └──────────┬──────────┘
                     │
             ┌───────▼───────┐
             │      WAF      │
             └───────┬───────┘
                     │
          ┌──────────▼──────────┐
          │   Nextcloud (App)   │
          └───────┬───────┬─────┘
                  │       │
             ┌────▼──┐ ┌──▼─────┐
             │ Redis │ │ MySQL  │
             └───┬────┘ └──┬────┘
                 │         │
             Logs▼         ▼Logs
             ┌────────────────────┐
             │  CrowdSec IDS/IPS  │
             └────────┬───────────┘
                      │
            ┌─────────▼─────────────┐
            │   Traefik Bouncer     │
            └─────────┬─────────────┘
                      │
          ┌───────────▼──────────────┐
          │     ELK Stack (SIEM)     │
          │ Elasticsearch + Logstash │
          │          + Kibana        │
          └───────────┬──────────────┘
                      │
         ┌────────────▼────────────┐
         │  Prometheus + Grafana   │
         └─────────────────────────┘


---

## Lancement

    docker-compose up -d

## Accès aux services :

    | Service | URL |
    |----------------|---------------------------|
    | Nextcloud | http://localhost:8080 |
    | Grafana | http://localhost:3000 |
    | Kibana | http://localhost:5601 |
    | Prometheus | http://localhost:9090 |
    | Nginx ProxyMgr | http://localhost:81 |


---

## Tests d'attaques

    - Les scripts dans le dossier tests/ simulent :

    - sqli.ps1 : Injection SQL

    - xss.ps1 : Cross-Site Scripting

    - Ils permettent de vérifier :

    - Blocage par le WAF

    - Détection par CrowdSec

    - Visualisation des alertes dans Kibana

    - Impact visible dans Grafana

---

## Monitoring

    - Prometheus : collecte les métriques de cadvisor et des services

    - Grafana : dashboards personnalisés sur l’utilisation CPU, mémoire, requêtes

    - Kibana : visualisation des logs (Nextcloud, WAF, IDS)

---

## Documentation
    - Architecture : docs/architecture.png

    - Rapport d’analyse : docs/rapport.md

    - Dashboards JSON : dashboards/


---

## Équipe

    - Sécurité : Djimi Romaric

    - Infrastructure : silvere Tchoudji

    - Monitoring : Silvere Tchoudji & Djimi Romaric

    - Tests & Validation : Silvere Tchoudji & Djimi Romaric


---

## État du livrable

    - Application métier sécurisée et opérationnelle

    - Logs centralisés dans Kibana

    - Détection d'attaques (SQLi, XSS)

    - Visualisation dans Grafana

    - Documentation complète et schéma d'architecture


---

## Licence
Projet réalisé dans le cadre d’un TP pédagogique par Djimi ROMARIC et SILVERE TCHOUDJI