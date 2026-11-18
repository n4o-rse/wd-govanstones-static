# PRIMER — wd-govanstones-static

Arbeitsplan für `n4o-rse/wd-govanstones-static`. Wird zu Beginn jedes Chats hochgeladen (oder per Clone geholt) und am Ende zurückgeschrieben. Stand: 2026-11-18 (S0: Skeleton).

Sprache: dieses Dokument ist deutsch, alles andere im Repo (Code, Kommentare, README) ist britisches Englisch.

---

## Teil A — Immer gültig

### A1 Ausgangslage

| Repo | Rolle | Stand |
|---|---|---|
| `n4o-rse/wikibase-static` | Paket (ab 0.27.0): `seeds-from-sparql`, `import-wikidata --generic`, Seiten, RDF, SPARQL | vorhanden, S14a erledigt |
| `n4o-rse/wd-govanstones-static` | dieses Repo: die Govan Stones aus Wikidata (WikiProject GovanStones, Q141962030) | S0 |
| `n4o-rse/wd-ogham-static` | Schwester-Site für Ogham-Steine aus Wikidata | leer, S14c im Paket-PRIMER |
| `n4o-rse/chublets-wikibase-demo` | Vorbild des Wikidata-Imports (S7d) | vorhanden |

### A2 Beschlusslage

| Thema | Beschluss | Datum |
|---|---|---|
| IDs | Eigene IDs (Items und Klassen `Q100` ff., Properties `P100` ff.); die Wikidata-ID steht als Zwilling `P4` (Entscheidung von Flo) | 2026-11-14 |
| RDF | Präfix `gvs` (`gvs:`, `gvst:`, `gvsp:` …), `rdf_base` `https://n4o-rse.github.io/wd-govanstones-static`; `owl:sameAs` auf Wikidata ist noch nicht umgesetzt (Paket-PRIMER, S14d) | 2026-11-14 |
| Lizenz | Daten CC0 (Standard des Pakets, Wikidata ist CC0), Code MIT; Bilder werden nicht kopiert, nur von Commons verlinkt | 2026-11-18 |
| Quellen | Query `data/seeds/wikidata-govanstones.rq` (die Sammlung Q42887850 und ihre Teile über `P527`/`P361`; die WikiProject-Query über `P6104` fand nur die Sammlung) plus `data/seeds/wikidata-qids.txt`; beide über `seeds-from-sparql` | 2026-11-18 |
| Umfang | Alle Steine des WikiProjects; für den CAA-UK-Vortrag werden höchstens drei Steine gezeigt, nicht alle | 2026-11-01 |
| Modell | Keins eigenes: `import-wikidata --generic` legt die Properties aus den Wikidata-Properties an (`main.py` hängt `--generic` selbst an) | 2026-11-18 |
| Später | Sketchfab-Modelle als FDOx (mit Rückverweis), Grabsteine des Friedhofs; beides nicht jetzt | 2026-11-01 |

### A3 Arbeitsweise

Änderungen kommen als Patch-ZIP; Flo spielt sie selbst ein und pusht. Wikidata und der Query Service sind aus der Sandbox nicht erreichbar: die Läufe mit Netz macht Flo auf Windows `cmd`, die Ergebnisse (`dist/wikidata-import.txt`, `dist/wikidata-gaps.csv`, die Ausgabe der Befehle) kommen in den nächsten Chat.

---

## Teil B — Schritte

| Schritt | Inhalt | Stand |
|---|---|---|
| S0 | Skeleton: `main.py` mit den Wikidata-Verben, `site.yaml`, Query, Pin auf v0.27.0, Workflows (`deploy: false`), leer gebaut | erledigt 2026-11-18 |
| S1 | Erster Lauf: `seeds-from-sparql`, `import-wikidata`, Bericht lesen, Properties prüfen (`--skip-property`), `--strict` bauen, `deploy: true`, Pages einschalten | offen (bei Flo) |
| S2 | CRM-Overlay `data/crm.csv` (Klassen der Steine → CIDOC CRM), `check-crm` | offen |
| S3 | Beschreibungen, Logo, `data_as_of`, `queries.yaml` für die Steine (Bild, Karte, Inschrift, Standort) | Logo und `data_as_of` erledigt 2026-11-18; Rest offen |
| S4 | Auswahl von drei Steinen für CAA UK; Verbindung zu FDOx und Sketchfab | später |

---

## Teil D — Offene Punkte

- **Query (1. Lauf 2026-11-18):** `P6104 = Q141962030` fand nur die Sammlung „The Govan Stones“ (Q42887850); ihre 31 Steine standen nur als Stubs da. Die Query nimmt jetzt die Sammlung selbst und alles, was `P527` (hat Teil) der Sammlung ist oder `P361` (Teil von) auf sie setzt. Fehlt danach ein Stein, seine Id in `data/seeds/wikidata-qids.txt` eintragen.
- **Noch nicht entschieden:** `P9905` (Datum außerhalb des Gregorianischen Kalenders) wird nicht importiert; Klassen-Stubs (Sammlung, archäologisches Artefakt, archäologische Stätte) brauchen Zeilen in `data/crm.csv`, bevor `check-crm` durchläuft (S2).
- **Zahl der Properties:** `--generic` legt jede tragbare Property an, auch Identifikatoren (Google-KG, VIAF …). Sind es zu viele, `--skip-property P…,P…` am `import-wikidata`-Aufruf (Paket-PRIMER, Teil D).
- **GitHub Pages:** einmal Settings, Pages, Source „GitHub Actions“; erst nach S1 `deploy: true` in `.github/workflows/site.yml`.
- **Update-Workflow:** einmal Settings, Actions, General, „Allow GitHub Actions to create and approve pull requests“.
