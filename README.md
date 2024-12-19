# Agrivanna Farm Service

## Table of Contents

- [Contributors](#contributors)
- [Description](#description)
- [Tech Stack](#tech-stack)
- [Setup](#setup)
- [Quick Start](#quick-start)
- [Database Schemas](#database-schemas)

## Contributors

- [Axel Sanchez](https://github.com/Axeloooo)

## Description

Agrivanna Farm Service is a RESTful API service that manages user data for the Agrivanna platform. It is built using Go and MySQL.

## Tech Stack

![Go](https://img.shields.io/badge/Go-00ADD8.svg?style=for-the-badge&logo=Go&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-4479A1.svg?style=for-the-badge&logo=MySQL&logoColor=white)
![Auth0](https://img.shields.io/badge/Auth0-EB5424.svg?style=for-the-badge&logo=Auth0&logoColor=white)

## Setup

The following programs should be installed:

- [Go](https://golang.org/)

## Quick Start

1. Clone the repository

```bash
$ git clone git@github.com:Vortex-Livestock/agrivanna-farm-service.git
```

2. Change directory to the project folder

```bash
$ cd agrivanna-farm-service
```

3. Run the application

```bash
$ go run main.go
```

## Database Schemas

- `farms` schema

```sql
CREATE TABLE IF NOT EXISTS farms (
  `id` varchar(36) NOT NULL PRIMARY KEY,
  `name` varchar(255) NOT NULL,
  `address` varchar(255) NOT NULL,
  `city` varchar(255) NOT NULL,
  `province` varchar(255) NOT NULL,
  `country` varchar(255) NOT NULL,
  `postal_code` varchar(255) NOT NULL,
  `owner_id` varchar(36) NOT NULL UNIQUE,
  `metadata` json,
  `created_at` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP,
  `updated_at` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  FOREIGN KEY (`owner_id`) REFERENCES users(`id`)
);
```

- `user_farm` schema

```sql
CREATE TABLE IF NOT EXISTS user_farm (
  `id` varchar(36) NOT NULL PRIMARY KEY,
  `user_id` varchar(36) NOT NULL,
  `farm_id` varchar(36) NOT NULL,
  `role` enum('ADMIN', 'EDITOR', 'VIEWER') NOT NULL,
  `created_at` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP,
  `updated_at` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  FOREIGN KEY (`user_id`) REFERENCES users(`id`),
  FOREIGN KEY (`farm_id`) REFERENCES farms(`id`)
);
```

- `reports` schema

```sql
CREATE TABLE IF NOT EXISTS reports (
  `id` varchar(36) NOT NULL PRIMARY KEY,
  `farm_id` varchar(36) NOT NULL,
  `type` enum('WEEKLY', 'MONTHLY', 'YEARLY') NOT NULL,
  `file_url` varchar(255) NOT NULL,
  `generated_at` date NOT NULL,
  `metadata` json,
  `created_at` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP,
  `updated_at` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  FOREIGN KEY (`farm_id`) REFERENCES farms(`id`)
);
```

## Dependencies

- [Fiber](https://docs.gofiber.io/)
- [GORM](https://gorm.io/)
- [MySQL Driver](https://github.com/go-gorm/mysql)
