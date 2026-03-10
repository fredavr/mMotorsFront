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
    
    subgraph PUBLIC["public subnet"]
        subgraph US1["🖥️ Service (S3)"]
            FE1["Frontend <br> (Angular)"]
        end
    end

    subgraph PRIVE["private submet"]
        subgraph EDGE[" "]
            GW["Gateway"]
            LB["Load Balancer"]
        end
        subgraph VS1["🖥️ Server 1 (EC2 AZ1)"]
            BE1["Backend <br> (Node.js/NestJS)"]
        end

        subgraph VS2["🖥️ Server 2 (EC2 AZ2)"]
            BE2["Backend <br> (Node.js/NestJS)"]
            DB2["Primary Database <br> (PostgreSQL)"]
        end

        subgraph VS3["🖥️ Server 3 (EC2 AZ3)"]
            DB3["Secondary Database (réplica) <br> (PostgreSQL)"]
        end

        subgraph STO["📦 Storage (S3)"]
            CSTO["contracts pdf"]
            BK["backups DB <br> (PostgreSQL)"]
        end
    end


    subgraph EXT["☁️ external service"]
        DS["API Sign"]
        DP["API payment"]
        DA["API Auth"]
    end


    Client -->|"HTTPS"| FE1
    FE1 <-->|"HTTPS"| GW
    GW --> LB
    LB --> BE1
    LB --> BE2

    BE1 <-->|"CRUD"| DB2
    BE1 <-->|"read"| DB3
    BE1 <--> CSTO
    BE2 <-->|"CRUD"| DB2
    BE2 <-->|"read"| DB3
    BE2 <--> CSTO
    DB2 <-->|"Replica"| DB3
    DB3 --> BK

    BE1 <--> EXT
    BE2 <--> EXT

    
    style Client fill:#1a1a2e,stroke:#94a3b8,color:#fff

    style PUBLIC fill:#183E0C,stroke:#A1FA4F,color:#fff
    style US1 fill:#404040,stroke:#A1FA4F,color:#fff
    style FE1 fill:#757427,stroke:#183E0C,color:#fff

    style PRIVE fill:#3A0603,stroke:#EB3324,color:#fff
    style EDGE fill:#404040,stroke:#EB3324,color:#fff
    style GW fill:#1e3a5f,stroke:#3A0603,color:#fff
    style LB fill:#1e3a5f,stroke:#3A0603,color:#fff
    style VS1 fill:#404040,stroke:#EB3324,color:#fff
    style BE1 fill:#78350F,stroke:#3A0603,color:#fff
    style VS2 fill:#404040,stroke:#EB3324,color:#fff
    style BE2 fill:#78350f,stroke:#3A0603,color:#fff
    style DB2 fill:#3B0764,stroke:#3A0603,color:#fff
    style VS3 fill:#404040,stroke:#EB3324,color:#fff
    style DB3 fill:#3b0764,stroke:#3A0603,color:#fff
    style STO fill:#404040,stroke:#EB3324,color:#fff

    style EXT fill:#183E0C,stroke:#A1FA4F,color:#fff
    style DS fill:#831843,stroke:#f472b6,color:#fff
    style DP fill:#831843,stroke:#f472b6,color:#fff
    style DA fill:#831843,stroke:#f472b6,color:#fff
```
