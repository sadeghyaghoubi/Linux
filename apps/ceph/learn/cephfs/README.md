# نمودار کامل مسیر داده در CephFS

```mermaid
graph TD
    A[Client] -->|1. Metadata Request| B[MDS]
    B -->|2. Permission Check| C[Metadata Pool]
    C -->|3. Return Inode| A
    A -->|4. Data Request| D[Data Pool]
    D --> E[PG 1]
    D --> F[PG 2]
    D --> G[PG ...]
    E --> H[OSD 1]
    E --> I[OSD 2]
    E --> J[OSD 3]
    F --> K[OSD 4]
    F --> L[OSD 5]
    F --> M[OSD 6]
    G --> N[OSD ...]
    H --> O[SSD Pool]
    I --> O
    K --> P[HDD Pool]
    L --> P
