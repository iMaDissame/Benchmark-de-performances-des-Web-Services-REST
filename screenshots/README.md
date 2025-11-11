# 📸 Screenshots Directory

Ce dossier contient toutes les captures d'écran utilisées dans le README.md principal.

## 📋 Images Requises

### Dashboards Grafana

1. **grafana-dashboard.png**
   - Dashboard principal avec RPS, Response Time Percentiles, Active Threads, Error Rate
   - Source: http://localhost:3000/d/jmeter-benchmark-complete

2. **grafana-jvm-metrics.png**
   - CPU Usage (process vs system)
   - Heap Usage (used vs max)
   - GC Pause (ms/s)
   - HikariCP Connections
   - Live Threads
   - Source: http://localhost:3000/d/jmeter-benchmark-complete

### Graphiques InfluxDB

3. **influxdb-read-heavy.png**
   - Data Explorer InfluxDB
   - Query: variant-read-heavy, measurement=jmeter
   - Graphique montrant les pics de latence 20-140ms

4. **influxdb-heavy-body.png**
   - Data Explorer InfluxDB
   - Query: variant-heavy-body, measurement=jmeter
   - Graphique montrant le pic catastrophique ~1100ms (Spring MVC)

5. **influxdb-mixed.png**
   - Data Explorer InfluxDB
   - Query: variant-mixed, measurement=jmeter
   - Graphique montrant latences 30-95ms

6. **influxdb-join-filter.png**
   - Data Explorer InfluxDB
   - Query: variant-join-filter, measurement=jmeter
   - Graphique montrant latences stables 10-90ms avec tooltip p95=30.07ms

## 📸 Comment Capturer les Screenshots

### 1. Grafana Dashboard

```bash
# Accéder au dashboard
http://localhost:3000/d/jmeter-benchmark-complete

# Capturer:
# - Vue d'ensemble complète (grafana-dashboard.png)
# - Section JVM Metrics (grafana-jvm-metrics.png)
```

**Résolution recommandée:** 1920x1080 ou plus large

### 2. InfluxDB Data Explorer

```bash
# Accéder au Data Explorer
http://localhost:8086/data-explorer

# Pour chaque scénario:
FROM: jmeter
FILTER: 
  - _measurement = jmeter
  - application = variant-[scenario-name]
  - status = all
AGGREGATE: mean(value) grouped by time
```

**Queries exactes:**

#### influxdb-read-heavy.png
```
from(bucket: "jmeter")
  |> range(start: -24h)
  |> filter(fn: (r) => r["_measurement"] == "jmeter")
  |> filter(fn: (r) => r["application"] == "variant-read-heavy")
  |> aggregateWindow(every: 10s, fn: mean)
```

#### influxdb-heavy-body.png
```
from(bucket: "jmeter")
  |> range(start: -24h)
  |> filter(fn: (r) => r["_measurement"] == "jmeter")
  |> filter(fn: (r) => r["application"] == "variant-heavy-body")
  |> aggregateWindow(every: 10s, fn: mean)
```

#### influxdb-mixed.png
```
from(bucket: "jmeter")
  |> range(start: -24h)
  |> filter(fn: (r) => r["_measurement"] == "jmeter")
  |> filter(fn: (r) => r["application"] == "variant-mixed")
  |> aggregateWindow(every: 10s, fn: mean)
```

#### influxdb-join-filter.png
```
from(bucket: "jmeter")
  |> range(start: -24h)
  |> filter(fn: (r) => r["_measurement"] == "jmeter")
  |> filter(fn: (r) => r["application"] == "variant-join-filter")
  |> aggregateWindow(every: 10s, fn: mean)
```

## 🖼️ Formats et Qualité

- **Format:** PNG (recommandé) ou JPG
- **Résolution minimale:** 1280x720
- **Résolution recommandée:** 1920x1080
- **Qualité:** Haute (85-100%)
- **Taille maximale:** 5MB par image

## 📝 Noms de Fichiers

Les noms DOIVENT correspondre exactement à ceux référencés dans le README.md:

```
screenshots/
├── grafana-dashboard.png
├── grafana-jvm-metrics.png
├── influxdb-read-heavy.png
├── influxdb-heavy-body.png
├── influxdb-mixed.png
└── influxdb-join-filter.png
```

## ✅ Checklist

Avant de commit:

- [ ] Toutes les 6 images sont présentes
- [ ] Les noms de fichiers sont corrects (lowercase, avec tirets)
- [ ] Les images sont en format PNG
- [ ] Les graphiques sont lisibles (texte non flou)
- [ ] Les tooltips/légendes sont visibles si nécessaire
- [ ] Les timestamps sont cohérents
- [ ] Taille totale < 30MB

## 🚫 .gitignore

Si vous voulez exclure temporairement certaines images:

```gitignore
# Dans .gitignore
screenshots/*.tmp
screenshots/*-draft.png
```

## 🔄 Mise à Jour

Si vous refaites des tests:

1. Re-capturer les screenshots avec les nouvelles données
2. Remplacer les anciennes images
3. Vérifier que les métriques dans le README.md correspondent
4. Commit avec message: `docs: update benchmark screenshots`

---

**Note:** Ces screenshots sont essentiels pour la visualisation des résultats dans le README.md. Sans eux, GitHub affichera des images cassées (404).

