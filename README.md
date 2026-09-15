# space-missions-dw

Data warehouse sulle missioni di lancio spaziale dal 1957 al 2020.

## Obiettivo

Costruire un data mart sulle missioni di lancio spaziale mondiali integrando due dataset pubblici, e pubblicare un cubo OLAP navigabile per analizzare affidabilità, costi e cadenza dei lanci lungo le dimensioni tempo, razzo, agenzia, sito di lancio e astronauta.

## Stack

| Livello | Tecnologia | Versione |
|---|---|---|
| Runtime | Eclipse Temurin JDK | 11 |
| Database | PostgreSQL | 18.6 |
| ETL | Pentaho Data Integration (Kettle) | 9.4.0.0-343 |
| Progettazione del cubo | Pentaho Schema Workbench | 9.4.0.0-343 |
| Motore OLAP | Mondrian su Pentaho Server | 9.4.0.0-343 |
| Front-end di analisi | Saiku Analytics | 9.4 |

## Fonti dati

| Fonte | Contenuto |
|---|---|
| [All Space Missions from 1957](https://www.kaggle.com/datasets/agirlcoding/all-space-missions-from-1957/data) | 4.324 missioni di lancio, 1957 - agosto 2020 |
| [Astronaut Database](https://data.mendeley.com/datasets/86tsnnbv2w/1) | 1.277 partecipazioni astronauta-missione, fino a gennaio 2020 |

I file sorgente sono versionati in `data/`.

## Requisiti

- **JDK 11** (Eclipse Temurin). Versioni successive non sono supportate da Pentaho 9.4.
- **PostgreSQL 18.6**
- **Pentaho Data Integration 9.4.0.0-343**, dalle release del repository [`ambientelivre/legacy-pentaho-ce`](https://github.com/ambientelivre/legacy-pentaho-ce)

## Setup

### 1. Java

Installare il JDK 11 e impostare la variabile d'ambiente utente, puntando alla cartella radice del JDK:

```
PENTAHO_JAVA_HOME = C:\Program Files\Eclipse Adoptium\jdk-11.x.x-hotspot
```

### 2. PostgreSQL

Installare PostgreSQL con i componenti Server, pgAdmin e Command Line Tools, mantenendo la porta predefinita 5432.

Creare il database:

```sql
CREATE DATABASE space_dw;
```

### 3. Pentaho Data Integration

Scompattare `pdi-ce-9.4.0.0-343.zip` in un percorso privo di spazi e caratteri accentati, ad esempio `C:\pentaho\`. Avviare con `Spoon.bat`.

### 4. Connessione al database

In Spoon, creare una nuova trasformazione e configurare la connessione dalla scheda *View > Database connections*:

| Parametro | Valore |
|---|---|
| Connection name | `space_dw` |
| Connection type | PostgreSQL |
| Access | Native (JDBC) |
| Host Name | `localhost` |
| Database Name | `space_dw` |
| Port Number | `5432` |
| User Name | `postgres` |

## Struttura del repository

```
docs/      documentazione di progetto (pitch, DFM, schema logico, ore)
sql/       script DDL del data mart
etl/       trasformazioni e job Kettle
data/      dataset sorgente
schema/    schema Mondrian del cubo
test/      query di verifica del caricamento
```

## Nota sull'uso di AI generativa

L'uso di strumenti di AI generativa è tracciato in `docs/ai-tracklist.md`, come previsto dalle linee guida del corso.