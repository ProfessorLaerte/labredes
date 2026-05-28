flowchart TB
    %% =========================
    %% AS 4100
    %% =========================
    subgraph AS4100["AS 4100"]
        direction TB
        R7["R7"]
        R6["R6<br/>Router ID 10.10.10.1"]
        R8["R8"]
        PC50["PC50"]

        R6 ---|"10.10.10.1/24 ↔ 10.10.10.2/24"| R7
        R7 ---|"10.10.20.1/24 ↔ 10.10.20.2/24"| R8
        R8 ---|"LAN 10.10.20.0/24"| PC50
    end

    %% =========================
    %% AS 3200
    %% =========================
    subgraph AS3200["AS 3200"]
        direction TB
        R1["R1<br/>Router ID 172.16.30.2"]
        R2["R2"]
        R3["R3"]
        SW1["SW1"]
        PC10["PC10"]

        R1 ---|"172.16.30.1/24 ↔ 172.16.30.2/24"| R2
        R2 ---|"172.16.20.1/24 ↔ 172.16.20.2/24"| R3
        R3 ---|"172.16.10.1/24"| SW1
        SW1 ---|"172.16.10.0/24"| PC10
    end

    %% =========================
    %% AS 1900
    %% =========================
    subgraph AS1900["AS 1900"]
        direction TB
        R9["R9<br/>Router ID 192.168.30.1"]
        R4["R4"]
        R5["R5"]
        PC40["PC40"]

        R9 ---|"192.168.30.1/24 ↔ 192.168.30.2/24"| R4
        R4 ---|"192.168.20.2/24 ↔ 192.168.20.1/24"| R5
        R5 ---|"192.168.10.1/24"| PC40
    end

    %% =========================
    %% Links entre AS
    %% =========================
    R1 ---|"172.31.10.1/24 ↔ 172.31.10.2/24"| R6
    R1 ---|"172.31.20.1/24 ↔ 172.31.20.2/24"| R9
    R6 ---|"172.31.30.1/24 ↔ 172.31.30.2/24"| R9

    %% =========================
    %% Estilo
    %% =========================
    classDef router fill:#dbeafe,stroke:#1d4ed8,color:#111827,stroke-width:2px;
    classDef pc fill:#fef3c7,stroke:#d97706,color:#111827,stroke-width:1.5px;
    classDef sw fill:#dcfce7,stroke:#16a34a,color:#111827,stroke-width:1.5px;

    class R1,R2,R3,R4,R5,R6,R7,R8,R9 router;
    class PC10,PC40,PC50 pc;
    class SW1 sw;

    style AS4100 fill:#fef9c3,stroke:#ca8a04,stroke-width:2px,stroke-dasharray: 8 6
    style AS3200 fill:#ede9fe,stroke:#7c3aed,stroke-width:2px,stroke-dasharray: 8 6
    style AS1900 fill:#dcfce7,stroke:#16a34a,stroke-width:2px,stroke-dasharray: 8 6
