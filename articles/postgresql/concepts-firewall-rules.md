---
title: "Databáze Azure pro pravidla brány firewall serveru PostgreSQL | Microsoft Docs"
description: "Popisuje pravidla brány firewall pro vaši databázi Azure pro PostgreSQL server."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: 1d46a4434c483c3612a9a7b4cdef718d6dc3e765
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-database-for-postgresql-server-firewall-rules"></a>Databáze Azure pro pravidla brány firewall serveru PostgreSQL
Brány firewall zabránit všechny přístup tooyour databázový server, dokud nezadáte, které počítače mají oprávnění. brány firewall Hello uděluje přístup k serveru toohello podle hello pocházející IP adresu každého požadavku.
tooconfigure brány firewall, vytvořte pravidla firewallu, která zadejte rozsahy IP adres, přijatelný. Můžete vytvořit pravidla brány firewall na úrovni serveru hello.

**Pravidla brány firewall:** tato pravidla povolit klienty tooaccess celou databázi Azure pro PostgreSQL Server, tedy hello všechny hello databází v rámci stejného logického serveru. Pravidla brány firewall na úrovni serveru se dá nakonfigurovat pomocí hello portál Azure nebo pomocí rozhraní příkazového řádku Azure. brány firewall na úrovni serveru toocreate pravidla, musí být Přispěvatel předplatné nebo vlastníka předplatného hello.

## <a name="firewall-overview"></a>Přehled brány firewall
Všechny databáze tooyour přístup k databázi Azure pro PostgreSQL server je blokován bránou hello firewall ve výchozím nastavení. toobegin pomocí serveru z jiného počítače, je nutné toospecify jeden nebo více úrovni serveru brány firewall pravidla tooenable přístup tooyour serveru. Použijte toospecify pravidla brány firewall hello rozsahy IP adresu, která z Internetu tooallow hello. Toohello přístup k webu Azure portal, samotné nebude ovlivněné hello pravidla brány firewall.
Hello pokusy o připojení z Internetu a Azure musí nejdřív projít přes bránu firewall hello než dosáhnou databáze PostgreSQL, jak je znázorněno v následujícím diagramu hello:

![Tok příklad toho, jak funguje hello brány firewall](media/concepts-firewall-rules/1-firewall-concept.png)

## <a name="connecting-from-hello-internet"></a>Připojení z Internetu hello
Pravidla brány firewall na úrovni serveru použít tooall databází na hello Azure databáze pro PostgreSQL server. Pokud IP adresa hello hello požadavku je do jedné z hello rozsahů určených v hello pravidla brány firewall na úrovni serveru, jsou udělena hello připojení.
Pokud IP adresa hello hello požadavku není v rámci hello rozsahy zadanému v některém z hello úroveň databáze nebo pravidla brány firewall na úrovni serveru, hello žádost o připojení selže.
Například pokud vaše aplikace připojí k ovladač JDBC pro PostgreSQL, se může zobrazit tuto chybu pokus tooconnect při hello brána firewall blokuje připojení hello.
> java.util.concurrent.ExecutionException: java.lang.RuntimeException: org.postgresql.util.PSQLException: závažná: žádné pg\_hba.conf položka pro uživatele "adminuser" hostitel "123.45.67.890", databáze "postgresql", SSL

## <a name="programmatically-managing-firewall-rules"></a>Programová správa pravidel brány firewall
Kromě toho toohello portál Azure, brána firewall, který může být pravidla spravované programově pomocí rozhraní příkazového řádku Azure.
Viz také [vytvořit a spravovat databáze Azure pro pravidla brány firewall PostgreSQL pomocí rozhraní příkazového řádku Azure](howto-manage-firewall-using-cli.md)

## <a name="troubleshooting-hello-database-firewall"></a>Řešení potíží s hello databáze brány firewall
Mějte na paměti následující body při přístupu toohello databáze Microsoft Azure pro službu PostgreSQL Server tak, jak očekáváte, že hello:

* **Seznam povolených změny toohello ještě nevstoupilo v platnost:** může být co nejvíce pět minut zpoždění pro změny toohello databáze Azure pro efekt tootake konfigurace brány firewall serveru PostgreSQL.

* **Hello přihlášení nemá oprávnění nebo nesprávné heslo byl použit:** Pokud přihlášení nemá oprávnění na hello databáze Azure pro PostgreSQL serveru nebo hello heslo používané je nesprávná, hello toohello připojení databáze Azure pro PostgreSQL server je byl odepřen. Vytvoření nastavení brány firewall pouze poskytuje klientům tooattempt možnost připojení serveru tooyour; Každý klient musí poskytnout hello potřebná zabezpečovací pověření.

Například pomocí klienta JDBC, může zobrazit hello následující chyba.
> java.util.concurrent.ExecutionException: java.lang.RuntimeException: org.postgresql.util.PSQLException: závažná: selhalo ověřování hesla pro uživatele "uživatelské_jméno"

* **Dynamickou IP adresu:** Pokud máte připojení k Internetu pomocí dynamické IP adresy a máte potíže získávání přes bránu firewall hello, mohou zkuste jednu z následujících řešení hello:

* Zeptejte se, že poskytovatele služeb Internetu (ISP) pro rozsah IP adres hello hello tento přístup k databázi Azure přiřadili tooyour klientské počítače pro PostgreSQL Server a poté přidejte hello rozsah IP adres jako pravidlo brány firewall.

* Získat statické IP adresy místo pro klientské počítače a poté přidejte hello IP adresy jako pravidla brány firewall.

## <a name="next-steps"></a>Další kroky
Články o vytvoření pravidla brány firewall serveru úroveň a databáze naleznete na stránce:
* [Vytvářet a spravovat databáze Azure pro pravidla brány firewall PostgreSQL pomocí hello portálu Azure](howto-manage-firewall-using-portal.md)
* [Vytvářet a spravovat databáze Azure pro pravidla brány firewall PostgreSQL pomocí rozhraní příkazového řádku Azure](howto-manage-firewall-using-cli.md)