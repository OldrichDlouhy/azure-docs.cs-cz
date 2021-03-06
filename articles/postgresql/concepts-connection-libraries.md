---
title: Knihovny připojení – Azure Database for PostgreSQL – jeden server
description: Tento článek popisuje několik knihoven a ovladačů, které můžete použít při kódování aplikací pro připojení a dotazování Azure Database for PostgreSQL-Single server.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: 2305a02e5b094ec9e98f39363ddbd0c39221ab0b
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "74768890"
---
# <a name="connection-libraries-for-azure-database-for-postgresql---single-server"></a>Knihovny připojení pro Azure Database for PostgreSQL – jeden server
Tento článek obsahuje seznam knihoven a ovladačů, které můžou vývojáři použít k vývoji aplikací pro připojení k Azure Database for PostgreSQL a dotazování na ně.

## <a name="client-interfaces"></a>Klientská rozhraní
Většina jazykových klientských knihoven používaných pro připojení k serveru PostgreSQL je externí projekty a distribuuje se nezávisle. Uvedené knihovny jsou podporované na platformách Windows, Linux a Mac pro připojení k Azure Database for PostgreSQL. V části Další kroky jsou uvedeny některé příklady rychlých startů.

| **Jazyk** | **Rozhraní klienta** | **Další informace** | **Stáhnout** |
|--------------|----------------------------------------------------------------|-------------------------------------|--------------------------------------------------------------------|
| Python | [psycopg](http://initd.org/psycopg/) | DB API 2,0 – kompatibilní | [Stáhnout](http://initd.org/psycopg/download/) |
| PHP | [php – PgSQL](https://secure.php.net/manual/en/book.pgsql.php) | Rozšíření databáze | [Instalace](https://secure.php.net/manual/en/pgsql.installation.php) |
| Node.js | [Balíček pg npm](https://www.npmjs.com/package/pg) | Čistý JavaScript bez blokování klienta | [Instalace](https://www.npmjs.com/package/pg) |
| Java | [JDBC](https://jdbc.postgresql.org/) | Typ 4 ovladač JDBC | [Stáhnout](https://jdbc.postgresql.org/download.html)  |
| Ruby | [PG Gem](https://deveiate.org/code/pg/) | Rozhraní Ruby | [Stáhnout](https://rubygems.org/downloads/pg-0.20.0.gem) |
| Přejít | [PQ balíčku](https://godoc.org/github.com/lib/pq) | Ovladač čistého přechodu na Postgres | [Instalace](https://github.com/lib/pq/blob/master/README.md) |
| C \# /.NET | [Npgsql](https://www.npgsql.org/) | ADO.NET Zprostředkovatel dat | [Stáhnout](https://www.microsoft.com/net/) |
| ODBC | [psqlODBC](https://odbc.postgresql.org/) | Ovladač ODBC | [Stáhnout](https://www.postgresql.org/ftp/odbc/versions/) |
| C | [libpq](https://www.postgresql.org/docs/9.6/static/libpq.html) | Primární rozhraní jazyka C | Zahrnuje |
| C++ | [libpqxx](http://pqxx.org/) | Rozhraní C++ pro nové styly | [Stáhnout](http://pqxx.org/download/software/) |

## <a name="next-steps"></a>Další kroky
Přečtěte si tyto rychlé starty, jak se připojit k a dotazovat Azure Database for PostgreSQL pomocí vašeho jazyka podle vlastního výběru:

[Python](./connect-python.md)  |  [Node.JS](./connect-nodejs.md)  |  [Jazyk Java](./connect-java.md)  |  [Ruby](./connect-ruby.md)  |  [Php](./connect-php.md)  |  [.NET (C#)](./connect-csharp.md)  |  [Přejít](./connect-go.md)
