---
title: "aaaAzure databáze pro pravidla brány firewall serveru MySQL | Microsoft Docs"
description: "Popisuje pravidla brány firewall pro vaši databázi Azure pro server databáze MySQL."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: 1f85310385da947b6c492aa6aa21c1b885c2a97d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-database-for-mysql-server-firewall-rules"></a>Databáze Azure pro pravidla brány firewall serveru MySQL
Brány firewall zabránit všechny přístup tooyour databázový server, dokud nezadáte, které počítače mají oprávnění. brány firewall Hello uděluje přístup k serveru toohello podle hello pocházející IP adresu každého požadavku.

tooconfigure brány firewall, vytvořte pravidla firewallu, která zadejte rozsahy IP adres, přijatelný. Můžete vytvořit pravidla brány firewall na úrovni serveru hello.

**Pravidla brány firewall:** tato pravidla povolit klienty tooaccess celou databázi Azure pro server databáze MySQL, tedy hello všechny hello databází v rámci stejného logického serveru. Pravidla brány firewall na úrovni serveru se dá nakonfigurovat pomocí hello portál Azure nebo pomocí rozhraní příkazového řádku Azure. brány firewall na úrovni serveru toocreate pravidla, musí být Přispěvatel předplatné nebo vlastníka předplatného hello.

## <a name="firewall-overview"></a>Přehled brány firewall
Všechny databáze tooyour přístup k databázi Azure pro server databáze MySQL blokováno bránou hello firewall. ve výchozím nastavení. toobegin pomocí serveru z jiného počítače, je nutné toospecify jeden nebo více úrovni serveru brány firewall pravidla tooenable přístup tooyour serveru. Použijte toospecify pravidla brány firewall hello rozsahy IP adresu, která z Internetu tooallow hello. Toohello přístup k webu Azure portal, samotné nebude ovlivněné hello pravidla brány firewall.

Hello pokusy o připojení z Internetu a Azure musí nejdřív projít přes bránu firewall hello než dosáhnou Azure databáze pro databázi MySQL, jak je znázorněno v následujícím diagramu hello:

![Tok příklad toho, jak funguje hello brány firewall](./media/concepts-firewall-rules/1-firewall-concept.png)

## <a name="connecting-from-hello-internet"></a>Připojení z Internetu hello
Pravidla brány firewall na úrovni serveru použít tooall databází na hello Azure databáze pro server databáze MySQL.

Pokud IP adresa hello hello požadavku je do jedné z hello rozsahů určených v hello pravidla brány firewall na úrovni serveru, jsou udělena hello připojení.

Pokud IP adresa hello hello požadavku není v rámci hello rozsahy zadanému v některém z hello úroveň databáze nebo pravidla brány firewall na úrovni serveru, hello žádost o připojení selže.

## <a name="programmatically-managing-firewall-rules"></a>Programová správa pravidel brány firewall
Kromě toho toohello portál Azure, brána firewall, který může být pravidla spravované programově pomocí rozhraní příkazového řádku Azure. Viz také [vytvořit a spravovat databáze Azure pro pravidla brány firewall MySQL pomocí rozhraní příkazového řádku Azure](./howto-manage-firewall-using-cli.md)

## <a name="troubleshooting-hello-database-firewall"></a>Řešení potíží s hello databáze brány firewall
Mějte na paměti následující body při přístupu toohello databáze Microsoft Azure pro službu MySQL server tak, jak očekáváte, že hello:

* **Seznam povolených změny toohello ještě nevstoupilo v platnost:** může být co nejvíce pět minut zpoždění pro změny toohello databáze Azure pro efekt tootake konfigurace brány firewall serveru MySQL.

* **Hello přihlášení nemá oprávnění nebo nesprávné heslo byl použit:** Pokud přihlášení nemá oprávnění na hello Azure databáze MySQL server nebo hello heslo použité je nesprávné, hello toohello připojení databáze Azure pro MySQL serveru byl odepřen. Vytvoření nastavení brány firewall pouze poskytuje klientům tooattempt možnost připojení serveru tooyour; Každý klient musí poskytnout hello potřebná zabezpečovací pověření.

* **Dynamickou IP adresu:** Pokud máte připojení k Internetu pomocí dynamické IP adresy a máte potíže získávání přes bránu firewall hello, mohou zkuste jednu z následujících řešení hello:

* Zeptejte se, že poskytovatele služeb Internetu (ISP) pro rozsah IP adres hello přiřadili tooyour klientské počítače tohoto přístupu hello Azure databáze pro server databáze MySQL a poté přidejte hello rozsah IP adres jako pravidlo brány firewall.

* Získat statické IP adresy místo pro klientské počítače a poté přidejte hello IP adresy jako pravidla brány firewall.

## <a name="next-steps"></a>Další kroky

[Vytvářet a spravovat databáze Azure pro pravidla brány firewall MySQL pomocí portálu Azure hello](./howto-manage-firewall-using-portal.md)
[vytvořit a spravovat databáze Azure pro pravidla brány firewall MySQL pomocí rozhraní příkazového řádku Azure](./howto-manage-firewall-using-cli.md)
