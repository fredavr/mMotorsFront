# MMotorsFront

This project was generated using [Angular CLI](https://github.com/angular/angular-cli) version 21.2.0.

## Development server

To start a local development server, run:

```bash
ng serve
```

Once the server is running, open your browser and navigate to `http://localhost:4200/`. The application will automatically reload whenever you modify any of the source files.

## Code scaffolding

Angular CLI includes powerful code scaffolding tools. To generate a new component, run:

```bash
ng generate component component-name
```

For a complete list of available schematics (such as `components`, `directives`, or `pipes`), run:

```bash
ng generate --help
```

## Building

To build the project run:

```bash
ng build
```

This will compile your project and store the build artifacts in the `dist/` directory. By default, the production build optimizes your application for performance and speed.

## Running unit tests

To execute unit tests with the [Vitest](https://vitest.dev/) test runner, use the following command:

```bash
ng test
```

## Running end-to-end tests

For end-to-end (e2e) testing, run:

```bash
ng e2e
```

Angular CLI does not come with an end-to-end testing framework by default. You can choose one that suits your needs.

## Additional Resources

For more information on using the Angular CLI, including detailed command references, visit the [Angular CLI Overview and Command Reference](https://angular.dev/tools/cli) page.


## Architecture logicielle
```mermaid
graph TB
    Client(["👤 Client / Navigateur"])

    subgraph EDGE["🌐 Couche d'entrée"]
        GW["🔒 Gateway"]
        LB["⚖️ Load Balancer"]
    end

    subgraph S1["🖥️ Serveur 1 (EC2)"]
        FE1["🎨 Frontend <br> Interface utilisateur (Angular)"]
        BE1["⚙️ Backend <br> API REST <br> (Node.js/NestJS)"]
    end

    subgraph S2["🖥️ Serveur 2 (EC2)"]
        BE2["⚙️ Backend <br> API REST <br> (Node.js/NestJS)"]
        DB2["📀 Base de données Primary <br> (PostgreSQL)"]
    end

    subgraph S3["🖥️ Serveur 3 (EC2)"]
        FE3["🎨 Frontend <br> Interface utilisateur (Angular)"]
        DB3["📀 Base de données Secondary (réplica) <br> (PostgreSQL)"]
    end

    subgraph STO["📦 Stockage (S3)"]
        CSTO["📝 Contrats pdf"]
        BK["📀 Backups BDD <br> (PostgreSQL)"]
    end

    subgraph EXT["☁️ Services externes"]
        DS["📝 API Signatures"]
        DP["🔶 API Paiement"]
        DA["🔒 API Auth"]
    end


    Client -->|"HTTPS"| GW
    GW --> LB
    LB --> FE1
    LB --> BE1
    LB --> BE2
    LB --> FE3

    FE1 <-->|"HTTP"| BE1
    FE1 <-->|"HTTP"| BE2
    FE3 <-->|"HTTP"| BE1
    FE3 <-->|"HTTP"| BE2
    BE1 <-->|"Lecture/Ecriture"| DB2
    BE1 <-->|"Lecture"| DB3
    BE1 <--> CSTO
    BE2 <-->|"Lecture/Ecriture"| DB2
    BE2 <-->|"Lecture"| DB3
    BE2 <--> CSTO
    DB2 <-->|"Replica"| DB3
    DB3 --> BK

    BE1 <--> DS
    BE1 <--> DP
    BE1 <--> DA
    BE2 <--> DS
    BE2 <--> DP
    BE2 <--> DA

    style EDGE fill:#1a1a2e,stroke:#4a9eff,color:#fff
    style S1 fill:#16213e,stroke:#4ade80,color:#fff
    style S2 fill:#16213e,stroke:#f59e0b,color:#fff
    style S3 fill:#16213e,stroke:#a78bfa,color:#fff
    style EXT fill:#1a1a2e,stroke:#f472b6,color:#fff

    style GW fill:#1e3a5f,stroke:#4a9eff,color:#fff
    style LB fill:#1e3a5f,stroke:#4a9eff,color:#fff
    style FE1 fill:#14532d,stroke:#4ade80,color:#fff
    style BE1 fill:#14532d,stroke:#4ade80,color:#fff
    style BE2 fill:#78350f,stroke:#f59e0b,color:#fff
    style DB2 fill:#78350f,stroke:#f59e0b,color:#fff
    style FE3 fill:#3b0764,stroke:#a78bfa,color:#fff
    style DB3 fill:#3b0764,stroke:#a78bfa,color:#fff
    style DS fill:#831843,stroke:#f472b6,color:#fff
    style DP fill:#831843,stroke:#f472b6,color:#fff
    style DA fill:#831843,stroke:#f472b6,color:#fff
    style Client fill:#1a1a2e,stroke:#94a3b8,color:#fff
```
