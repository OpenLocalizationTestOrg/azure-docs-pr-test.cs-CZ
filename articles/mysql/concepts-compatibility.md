---
title: "Ovladače MySQL a Správa nástroje kompatibility | Microsoft Docs"
description: "Ovladače MySQL a nástroje pro správu kompatibilní s Azure Database pro databázi MySQL"
services: mysql
author: seanli1988
ms.author: seanli
editor: jasonwhowell
manager: jhubbard
ms.service: mysql-database
ms.topic: article
ms.date: 10/27/2017
ms.openlocfilehash: 7578ae710a3d6c81fdfa2952c53a20c2cdccb6d0
ms.sourcegitcommit: 0e1c4b925c778de4924c4985504a1791b8330c71
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/06/2018
---
# <a name="mysql-drivers-and-management-tools-compatible-with-azure-database-for-mysql"></a>Ovladače MySQL a nástroje pro správu kompatibilní s Azure Database pro databázi MySQL
Tento článek popisuje ovladače a nástroje pro správu, které jsou kompatibilní s Azure Database pro databázi MySQL.

## <a name="mysql-drivers"></a>Ovladače MySQL
Azure databáze pro databázi MySQL používá na světě nejoblíbenější edice community databáze MySQL. Proto je kompatibilní s širokou škálu programovací jazyky a ovladače. Cílem je podpora tři nejnovější verze ovladače MySQL a pokračovat ve snaze s autory open source Community o neustále zlepšovat funkce a použitelnost ovladače MySQL. Seznam ovladačů, které byly testovány a zjistí, že je kompatibilní s Azure databáze MySQL 5.6 a 5.7 je uvedený v následující tabulce:

| **Ovladače** | **Odkazy** | **Kompatibilní verze** | **Nekompatibilní verze** | **Poznámky k** |
| :-------- | :------------------------ | :----------- | :---------------------- | :--------------------------------------- |
| PHP | http://PHP.NET/downloads.php | 5.5 5.6 7.x | 5.3 | Pro jazyk PHP 7.0 připojení pomocí protokolu SSL MySQLi přidejte MYSQLI_CLIENT_SSL_DONT_VERIFY_SERVER_CERT v připojovacím řetězci. <br> ```mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306, NULL, MYSQLI_CLIENT_SSL_DONT_VERIFY_SERVER_CERT);```<br> Sada PDO: ```PDO::MYSQL_ATTR_SSL_VERIFY_SERVER_CERT``` možnost na hodnotu false.|
| .Net | [MySqlConnector na Githubu](https://github.com/mysql-net/MySqlConnector) <br> [Instalační balíček z Nuget](https://www.nuget.org/packages/MySqlConnector/) | 0.27 a po | 0.26.5 a před | |
| Nodejs |  [MySQLjs na Githubu](https://github.com/mysqljs/mysql/releases) <br> Instalační balíček z NPM:<br> Spustit `npm install mysql` z NPM | 2.15 | 2.14.1 a před | |
| PŘEJÍT | https://github.com/go-SQL-Driver/MySQL/releases | 1.3 | 1.2 a před | Použít allowNativePasswords = true v připojovacím řetězci |
| Python | https://pypi.Python.org/pypi/MySQL-Connector-Python | 1.2.3, 2.0, 2.1, 2.2 | 1.2.2 a před | |
| Java | https://downloads.mariadb.org/Connector-Java/ | 2.1 2.0 1.6 | 1.5.5 a před | |

## <a name="management-tools"></a>Nástroje pro správu
Výhoda kompatibility rozšiřuje na nástroje pro správu databáze také. Existující nástroje by měla fungovat s Azure Database pro databázi MySQL, tak dlouho, dokud manipulaci databáze pracuje v hranicích uživatelských oprávnění. Tři běžné nástroje pro správu databáze, které byly testovány a zjistí, že je kompatibilní s Azure databáze MySQL 5.6 a 5.7 jsou uvedené v následující tabulce:

|                                     | **MySQL Workbench 6.x a vyšší** | **Navicat 12** | **PHPMyAdmin 4.x a vyšší** |
| :---------------------------------- | :----------------------------- | :------------- | :-------------------------|
| Vytvoření, aktualizace, číst, zapisovat, odstranit | X | X | X |
| Připojení protokolem SSL | X | X | X |
| Automatické dokončování dotazů SQL | X | X |  |
| Import a Export dat | X | X | X |
| Exportovat do více formátů | X | X | X |
| Zálohování a obnovení |  | X |  |
| Zobrazit parametry serveru | X | X | X |
| Zobrazit připojení klientů | X | X | X |
