---
layout: post
title: "Gin-gonic e Swagger"
date: 2026-07-21 15:30:00 +0200
excerpt_separator: <!--more-->
---

Guida rapida per integrare Swagger (swaggo) in un'app Go con gin-gonic

<!--more-->

# Integrare Swagger (swaggo) in un'app Go con gin-gonic

## Dipendenze

```bash
$ go install github.com/swaggo/swag/cmd/swag@latest
$ go get -u github.com/swaggo/gin-swagger github.com/swaggo/files
```

## Documentazione di base

in main.go, sopra package main

```go
// @title My API
// @version 1.0
// @host localhost:8080
// @BasePath /
```

## Annotare DTO e handler

Esempio di dto:

```go   
// CreateUserDto rappresenta la richiesta di creazione utente
type CreateUserDto struct {
    Username string `json:"username" example:"mario"`
    Email string `json:"email" example:"mario@esempio.com"`
}
```

E nell'handler:

```go
// @Summary Crea utente
// @Tags users
// @Accept json
// @Produce json
// @Param payload body dto.CreateUserDto true "User"
// @Success 201 {object} dto.CreateUserDto
// @Router /users [post]
```

## Generare i file openapi

```bash
swag init -g ./cmd/main.go # (modificare il path a maing.go se necessario).
``` 

## Montare Swagger UI

```go
import (
    _ "<my-project-module>/docs"
    ginSwagger "github.com/swaggo/gin-swagger"
    swaggerFiles "github.com/swaggo/files"
)

```

## Aggiungere il middleware

```go
   r.GET("/swagger/*any", ginSwagger.WrapHandler(swaggerFiles.Handler))
```

## Eseguire

Eseguire l'applicazione go e visitare l'url http://localhost:8080/swagger/index.html

## Note

Adattare -g al file che contiene il main. Aggiungere tag `example` nei DTO (come sopra) per migliorare la UI.