# Simple Bank implemented in Go + PostgreSQL + Docker/Kubernetes + grpc

### DB Schema: https://dbdiagram.io/d/Simple-Bank-6589e4e389dea62799874f39

Note: In postgres Password is not required when you are connecting locally.

### Docker commands:
    
    - docker pull postgres:12-alpine

    - docker run --name <container_name> -e <environment_variable> -d <image>:<tag>

    - docker run --name postgres12 -p 5432:5432  -e POSTGRES_USER=root -e POSTGRES_PASSWORD=secret -d postgres:12-alpine

    - docker images
    - docker ps 
    - docker exec -it postgres12 psql -U root
    - docker logs -f postgres12
    - docker ps -a
    - docker stop postgres12
    - docker start postgres12
    - docker exec -it postgres12 /bin/sh
    - select now();
    - \q
    

### Install Golang On Ubuntu 22 (My Host Machine):

    - curl -OL https://go.dev/dl/go1.21.5.linux-amd64.tar.gz
    - sha256sum go1.21.5.linux-amd64.tar.gz
    - sudo tar -C /usr/local -xvf go1.21.5.linux-amd64.tar.gz
    - sudo nano ~/.profile
    - export PATH=$PATH:/usr/local/go/bin
    - source ~/.profile
    - go version

### Install sql on Ubuntu 22 (My Host Machine):

    - sudo snap install sqlc
    - Note: sqlc is used for creating and managing the migrations


### Install and use Golang migrate library on Ubuntu 22 (My Host Machine)

    - https://github.com/golang-migrate/migrate
    - Note: Database migrations written in Go. Use as CLI or import as library.
    - Installation documentation url : https://github.com/golang-migrate/migrate/tree/master/cmd/migrate
    - go to this address: https://github.com/golang-migrate/migrate/releases
    - Choose the right version
    - wget https://github.com/golang-migrate/migrate/releases/download/v4.17.0/migrate.linux-amd64.deb
    - sudo dpkg -i migrate.linux-amd64.deb
    - migrate --version


### Migration commands:

    - migrate create -ext sql -dir db/migration -seq init_schema
    - migrate --path db/migration --database "postgresql://root:secret@localhost:5432/simple_bank?sslmode=disable" --verbose up

### PostgreSQL commands:

    - docker exec -it postgres12 /bin/sh
    - createdb --username=root --owner=root simple_bank
    - psql simple_bank
    - \q
    - dropdb simple_bank
    - exit
    - docker exec -it postgres12 createdb --username=root --owner=root simple_bank
    - docker exec -it postgres12 psql -U root simple_bank
    - docker exec -it postgres12 dropdb simple_bank

### Make file usage:

    - make postgres
    - make createdb
    - make dropdb

### First migration:

    - migrate --path db/migration --database "postgresql://root:secret@localhost:5432/simple_bank?sslmode=disable" --verbose up


### SQLC commands:

    - sqlc init
    - sqlc generate

### Go mod commands:

    - go mod init github.com/mjmichael73/go-simple-bank
    - go mod tidy (to install all dependencies)

### Running First Test

    - go test --timeout 30s github.com/mjmichael73/go-simple-bank/db/sqlc
    - need to install this: https://github.com/lib/pq
    - To say to golang compiler that we want to import a library but not use it we need to add an _ before the import
    - if you install a go package it is indirect in go mod But when you run go mod tidy, the "indirect" will be removed
    - To check the test result it is recommended to use : testify library:  go get github.com/stretchr/testify
    - go test --cover --timeout 30s github.com/mjmichael73/go-simple-bank/db/sqlc
