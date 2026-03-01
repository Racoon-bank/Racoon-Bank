# Racoon-Bank
Design patterns project
To clone use
```
git clone --recurse-submodules [git@github.com:Dark-Type/<meta-repo>.git](https://github.com/Racoon-bank/Racoon-Bank/)
```
To update use

```
git pull

git submodule update --init --recursive

git submodule update --remote --merge

#then commit

```
```mermaid
graph LR

Client["iOS Apps: Client and Employee"] -->|HTTPS| DNS["Public domains: core / info / credit"]

subgraph Host["VPS Host"]
  subgraph Docker["Docker Engine"]
    subgraph Net["bridge network: racoon-net"]
      Core["racoon-core (.NET) port 5001"]
      Info["racoon-info (.NET) port 5002"]
      Credit["racoon-credit (Spring) port 8080"]

      SQL["sqlserver (MSSQL) port 1433"]
      PG["postgres (PostgreSQL) port 5432"]
    end

    SQLVol["volume: sqlserver_data"]
    PGVol["volume: postgres_data"]

    SQL --- SQLVol
    PG --- PGVol
  end
end

DNS -->|TLS routing| RP["Reverse proxy (nginx)"]

RP -->|"127.0.0.1:5001"| Core
RP -->|"127.0.0.1:5002"| Info
RP -->|"127.0.0.1:8090 -> 8080"| Credit

Core -->|"TCP 1433"| SQL
Info -->|"TCP 1433"| SQL
Credit -->|"TCP 5432"| PG

Credit -->|"HTTPS + service key"| Core
```
