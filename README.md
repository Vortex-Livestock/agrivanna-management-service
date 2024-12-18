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
CREATE TABLE `farms` (
  `id` CHAR(36) NOT NULL,
  `name` VARCHAR(255) NOT NULL,
  `location` VARCHAR(255),
  `owner_id` CHAR(36) NOT NULL,
  `metadata` JSON,
  `created_at` TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  `updated_at` TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`),
  FOREIGN KEY (`owner_id`) REFERENCES `users`(`id`) ON DELETE CASCADE
);
```

- `user_farm` schema

```sql
CREATE TABLE `user_farm` (
  `user_id` CHAR(36) NOT NULL,
  `farm_id` CHAR(36) NOT NULL,
  `role_in_farm` ENUM('OWNER','MANAGER','WORKER') NOT NULL,
  `permissions` JSON,
  PRIMARY KEY (`user_id`, `farm_id`),
  FOREIGN KEY (`user_id`) REFERENCES `users`(`id`) ON DELETE CASCADE,
  FOREIGN KEY (`farm_id`) REFERENCES `farms`(`id`) ON DELETE CASCADE
);
```

- `reports` schema

```sql
CREATE TABLE `reports` (
  `id` CHAR(36) NOT NULL,
  `farm_id` CHAR(36) NOT NULL,
  `type` ENUM('DAILY', 'WEEKLY', 'MONTHLY', 'CUSTOM') NOT NULL,
  `file_url` VARCHAR(255) NOT NULL,
  `generated_at` DATETIME NOT NULL,
  `metadata` JSON,
  `created_at` TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  `updated_at` TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`),
  FOREIGN KEY (`farm_id`) REFERENCES `farms`(`id`) ON DELETE CASCADE
);
```

## Dependencies

- [Fiber](https://docs.gofiber.io/)
- [GORM](https://gorm.io/)
- [MySQL Driver](https://github.com/go-gorm/mysql)
