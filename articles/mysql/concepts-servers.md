---
title: "Koncepty aaaServer ve službě Azure Database pro databázi MySQL | Microsoft Docs"
description: "Toto téma obsahuje důležité informace a pokyny pro práci s databází Azure pro servery, MySQL."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 07/06/2017
ms.openlocfilehash: 4d994cbdbde93ac5af3f4a2a7375b5851bebb1cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="server-concepts-in-azure-database-for-mysql"></a>Koncepty serveru ve službě Azure Database pro databázi MySQL
Toto téma obsahuje důležité informace a pokyny pro práci s databází Azure pro servery, MySQL.

## <a name="what-is-an-azure-database-for-mysql-server"></a>Co je Azure databáze MySQL serveru?

Databázi Azure pro server databáze MySQL je centrální administrativní bod pro více databází. Je stejný server konstruktor MySQL, který je pravděpodobně obeznámeni s místními hello, world hello. Konkrétně hello databáze Azure pro službu MySQL je spravovaný, poskytuje záruku výkonu, zpřístupní přístup a funkce na úrovni serveru.

Databáze MySQL serveru Azure:

- Je vytvořen v rámci předplatného Azure.
- Je hello nadřazený prostředek pro databáze.
- Poskytuje obor názvů pro databáze.
- Je kontejner s silné životnost sémantiku - odstranění serveru a odstraní hello obsažené databáze.
- Collocates prostředky v oblasti.
- Poskytuje koncového bodu připojení pro server a přístup k databázi.
- Poskytuje hello oboru pro zásady správy, které se vztahují tooits databází: přihlášení, brána firewall, uživatelů, rolí, konfigurace atd.
- Je k dispozici v několika verzích. Další informace najdete v tématu [podporované databáze Azure pro verze databáze MySQL](./concepts-supported-versions.md).

V rámci serveru Azure Database for MySQL můžete vytvořit jednu nebo několik databází. Můžete vyjádřit výslovný toocreate jednu databázi na serveru tooutilize všechny prostředky hello nebo vytvořit více databází tooshare hello prostředky. ceny Hello je strukturovaných na serveru, v závislosti na konfiguraci hello cenové úrovně výpočetní jednotky, úložiště (GB). Další informace najdete v tématu [cenové úrovně](./concepts-service-tiers.md).

## <a name="how-do-i-connect-and-authenticate-tooan-azure-database-for-mysql-server"></a>Jak připojit a ověřit tooan Azure databáze MySQL serveru?

Hello následující prvky pomoct zajistit bezpečný přístup tooyour databáze.

|||
| :-- | :-- |
| **Ověřování a autorizace** | Databáze MySQL serveru Azure podporuje nativní MySQL ověřování. Můžete se připojit a ověřit tooserver s přihlašovací jméno správce serveru hello. |
| **Protokol** | Služba Hello podporuje protokol na základě zpráv používané MySQL. |
| **TCP/IP** | protokol Hello je podporován přes TCP/IP a přes sockets Unix domény. |
| **Brána firewall** | toohelp chránit vaše data, pravidlo brány firewall brání všechny přístup tooyour databázový server nebo tooits databáze, dokud nezadáte počítače, které mají oprávnění. V tématu [databáze Azure pro pravidla brány firewall serveru MySQL](./concepts-firewall-rules.md). |
| **PROTOKOL SSL** | Služba Hello podporuje vynucení připojení SSL mezi aplikací a databázový server.  V tématu [připojení konfigurace protokolu SSL ve vaší aplikaci toosecurely connect tooAzure databáze pro databázi MySQL](./howto-configure-ssl.md). |
|||

## <a name="how-do-i-manage-a-server"></a>Jak lze spravovat server?
Můžete spravovat databáze Azure pro servery, MySQL pomocí hello portál Azure nebo hello rozhraní příkazového řádku Azure.

## <a name="next-steps"></a>Další kroky
- Přehled služby hello najdete v tématu [Azure databáze MySQL přehled](./overview.md)
- Informace o konkrétní prostředek kvóty a omezení na základě vaší **vrstvy služby**, najdete v části [úrovních služeb](./concepts-service-tiers.md)
- Informace o připojování toohello služby najdete v tématu [knihovny připojení pro databázi Azure pro databázi MySQL](./concepts-connection-libraries.md).
