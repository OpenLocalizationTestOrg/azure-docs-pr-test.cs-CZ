---
title: "Koncepty aaaServer v databázi Azure pro PostgreSQL | Microsoft Docs"
description: "Toto téma obsahuje důležité informace a pokyny pro práci s databází Azure pro servery PostgreSQL."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 07/06/2017
ms.openlocfilehash: 9cc7816992f2ddedd76fdf906075a723b97720a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-database-for-postgresql-servers"></a>Databáze Azure pro servery PostgreSQL
Toto téma obsahuje důležité informace a pokyny pro práci s databází Azure pro servery PostgreSQL.

## <a name="what-is-an-azure-database-for-postgresql-server"></a>Co je Azure databáze PostgreSQL serveru?
Databázi Azure pro PostgreSQL server je centrální administrativní bod pro více databází. Je stejný konstruktor PostgreSQL serveru, který je pravděpodobně obeznámeni s místními hello, world hello. Konkrétně hello PostgreSQL služby je spravovaný, poskytuje záruku výkonu, zpřístupní přístup a funkce na úrovni serveru.

Databázi Azure pro PostgreSQL server:

- Je vytvořen v rámci předplatného Azure.
- Je hello nadřazený prostředek pro databáze.
- Poskytuje obor názvů pro databáze.
- Je kontejner s silné životnost sémantiku - odstranění serveru a odstraní hello obsažené databáze.
- Collocates prostředky v oblasti.
- Poskytuje koncového bodu připojení pro přístup k serveru a databází (. postgresql.database.azure.com).
- Poskytuje hello oboru pro zásady správy, které se vztahují tooits databází: přihlášení, brána firewall, uživatelů, rolí, konfigurace atd.
- Je k dispozici v několika verzích. Další informace najdete v tématu [verze databáze podporována PostgreSQL](concepts-supported-versions.md).
- Je rozšiřitelný uživatelé. Další informace najdete v tématu [PostgreSQL rozšíření](concepts-extensions.md).

V rámci Azure Database pro PostgreSQL server můžete vytvořit jeden nebo více databází. Můžete vyjádřit výslovný toocreate jednu databázi na serveru tooutilize všechny prostředky hello nebo vytvořit více databází tooshare hello prostředky. ceny Hello je strukturovaných na serveru, v závislosti na konfiguraci hello cenové úrovně výpočetní jednotky, úložiště (GB). Další podrobnosti najdete v tématu [cenové úrovně](./concepts-service-tiers.md).

## <a name="how-do-i-connect-and-authenticate-tooan-azure-database-for-postgresql-server"></a>Jak připojit a ověřit tooan Azure databáze PostgreSQL serveru?
Hello následující prvky pomoct zajistit bezpečný přístup tooyour databáze.

|||
| :-- | :-- |
| **Ověřování a autorizace** | Azure databázi PostgreSQL serveru podporuje nativní PostgreSQL ověřování. Můžete se připojit a ověřit tooserver s přihlašovací jméno správce serveru hello. |
| **Protokol** | Služba Hello podporuje protokol na základě zpráv používá PostgreSQL. |
| **TCP/IP** | protokol Hello je podporován přes TCP/IP a přes sockets Unix domény. |
| **Brána firewall** | toohelp chránit vaše data, pravidlo brány firewall brání všechny přístup tooyour databázový server nebo tooits databáze, dokud nezadáte počítače, které mají oprávnění. V tématu [databáze Azure pro pravidla brány firewall serveru PostgreSQL](concepts-firewall-rules.md). |
|||

## <a name="how-do-i-manage-a-server"></a>Jak lze spravovat server?
Můžete spravovat databáze Azure pro hello PostgreSQL serverů pomocí portálu Azure nebo hello [rozhraní příkazového řádku Azure](/cli/azure/postgres).

## <a name="next-steps"></a>Další kroky
- Přehled služby hello najdete v tématu [databáze Azure pro PostgreSQL – přehled](overview.md)
- Informace o konkrétní prostředek kvóty a omezení na základě vaší **vrstvy služby**, najdete v části [úrovních služeb](concepts-service-tiers.md)
- Informace o připojování toohello služby, najdete v části [knihovny připojení pro databázi Azure pro PostgreSQL](concepts-connection-libraries.md).
