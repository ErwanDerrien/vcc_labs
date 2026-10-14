
flowchart TB
USER["Utilisateur / Internet"]
ROUTER["Routeur OpenStack<br/>routerdefault"]

    USER -->|"HTTP - TCP 50000"| ROUTER

    subgraph NETWORK["Réseau privé OpenStack - net-discovery - 192.168.10.0/24"]
        direction TB

        subgraph VM1["VM 1 - vm-discovery - insa-tiny"]
            CALC["Docker : calc-service<br/>Frontend Calculator<br/>192.168.10.232<br/>Port conteneur : 80"]
        end

        subgraph VM2["VM 2 - vm-addition - insa-tiny"]
            ADD["Docker : add-service<br/>Addition"]
        end

        subgraph VM3["VM 3 - vm-subtraction - insa-tiny"]
            SUB["Docker : sub-service<br/>Soustraction"]
        end

        subgraph VM4["VM 4 - vm-multiplication - insa-tiny"]
            MUL["Docker : mul-service<br/>Multiplication"]
        end

        subgraph VM5["VM 5 - vm-division - insa-tiny"]
            DIV["Docker : div-service<br/>Division"]
        end

        CALC -->|"HTTP - Réseau privé"| ADD
        CALC -->|"HTTP - Réseau privé"| SUB
        CALC -->|"HTTP - Réseau privé"| MUL
        CALC -->|"HTTP - Réseau privé"| DIV
    end

    ROUTER -->|"IP flottante : 194.199.106.34<br/>Vers frontend"| CALC

    classDef frontend fill:#dbeafe,stroke:#2563eb,color:#1e40af
    classDef internal fill:#dcfce7,stroke:#16a34a,color:#166534
    classDef network fill:#f3f4f6,stroke:#64748b,color:#111827

    class CALC frontend
    class ADD,SUB,MUL,DIV internal
    class ROUTER network
