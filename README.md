﻿# To_do_list
 flowchart LR
    FE[Frontend Container]
    BE[Backend Container]
    DB[Database Container]

    FE <--> BE
    BE <--> DB

    subgraph Docker Network
        FE
        BE
        DB
    end

