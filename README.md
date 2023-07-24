# system-overview

## Overall Achitecture

System Overview of my own projects.

<img src="system-overview-20230624.png" height="600px">

## Modules Overview

<img src="modules-overview-20230527.png" height="600px">

## Event Pipelines Overview

### Upload File Event Pipelines

```mermaid
sequenceDiagram

    participant b as browser
    participant ev as event-pump
    participant vfm as vfm
    participant ham as hammer
    participant fan as fantahsea
    participant mini as mini-fstore


    b->>mini:1. upload file
    b->>vfm:2. create file recrod
    ev--)vfm:3. (MQ, Binlog) file saved event
    vfm--)ham:4. (MQ) trigger image compression
    ham->>mini:5. Upload compressed image
    ham--)vfm:6. Notifiy file_id of compressed image
    vfm-->vfm:7. Update thumbnail
    ev--)vfm: 8. (MQ, Binlog) thumbnail updated event
    vfm--)fan:9. Create gallery image
```

### Delete File Event Pipelines

```mermaid
sequenceDiagram

    participant b as browser
    participant ev as event-pump
    participant vfm as vfm
    participant fan as fantahsea
    participant mini as mini-fstore

    b->>vfm:1. delete file
    vfm->>mini:2. mark file deleted
    mini--)mini:3. peridical job deletes actual files
    ev--)vfm:4. (MQ, Binlog) file deleted event
    vfm--)fan:5. (MQ) delete gallery image

```