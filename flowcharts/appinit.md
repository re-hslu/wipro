flowchart TD
    Start([Start syncData]) --> CheckNetwork{Netzwerkverbindung?}
    
    CheckNetwork -->|JA| GetRemoteTS[Hole Remote-Timestamp]
    CheckNetwork -->|NEIN| SetEmpty[remoteUpdateTS bleibt leer]
    
    GetRemoteTS --> GetLocalTS
    SetEmpty --> GetLocalTS[Hole lokalen Timestamp]
    
    GetLocalTS --> Compare[Vergleiche Timestamps]
    
    Compare --> Decision{lokalerZeitstempel != remoteZeitstempel}
    
    Decision -->|JA| LoadRemote[Lade Module von Backend und Speichere in Room-DB]
    Decision -->|NEIN| UseLocal[Hole lokale Module aus Room-DB]
    
    LoadRemote --> Success1[RETURN<br/>'Getting Remote Data']
    
    UseLocal --> CheckLocal{Lokale Modulliste vorhanden?}
    CheckLocal -->|JA| Success2[RETURN<br/>'Getting Cached Data']
    CheckLocal -->|NEIN| Fallback{Fallback<br/>möglich? #91;Wenn Netzwerk vorhanden -> Versuche Download#93;}
    
    Fallback -->|JA| FallbackLoad[Lade Module von Backend und Speichere in Room-DB]
    Fallback -->|NEIN| Error[RETURN<br/>'Fehler']
    
    FallbackLoad --> Success3[RETURN<br/>'Getting Remote Data']
    
    Success1 --> End([Ende])
    Success2 --> End
    Success3 --> End
    Error --> End
    
    style Start fill:#90EE90
    style End fill:#FFB6C1
    style Success1 fill:#77C5D8
    style Success2 fill:#EDEDF6
    style Success3 fill:#77C5D8
    style Error fill:#EC5A7A