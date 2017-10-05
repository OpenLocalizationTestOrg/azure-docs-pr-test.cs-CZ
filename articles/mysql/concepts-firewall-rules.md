---
title: "Databáze Azure pro pravidla brány firewall serveru MySQL | Microsoft Docs"
description: "Popisuje pravidla brány firewall pro vaši databázi Azure pro server databáze MySQL."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: 7456cef7a5da665ee3d70df64265b8186a79f9b3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-database-for-mysql-server-firewall-rules"></a>Databáze Azure pro pravidla brány firewall serveru MySQL
Brány firewall zabránit veškerý přístup k databázovému serveru, dokud nezadáte, které počítače mají oprávnění. Brána firewall uděluje přístup k serveru na základě původní IP adresy každého požadavku.

Bránu firewall nakonfigurujete tak, že vytvoříte pravidla brány firewall určující rozsahy přípustných IP adres. Můžete vytvořit pravidla brány firewall na úrovni serveru.

**Pravidla brány firewall:** tato pravidla povolit klientům přístup k vaší celou databázi Azure pro server databáze MySQL, to znamená, všechny databáze v rámci stejného logického serveru. Pravidla brány firewall na úrovni serveru se dá nakonfigurovat pomocí portálu Azure nebo pomocí rozhraní příkazového řádku Azure. Pokud chcete vytvořit pravidla brány firewall na úrovni serveru, musí být vlastník předplatného nebo jeho přispěvatelem předplatného.

## <a name="firewall-overview"></a>Přehled brány firewall
Všechny databáze přístup k vaší databázi Azure pro server databáze MySQL je blokován bránou firewall ve výchozím nastavení. Pokud chcete začít používat server z jiného počítače, je třeba zadat jeden nebo více pravidel brány firewall na úrovni serveru pro povolení přístupu k serveru. Použití pravidel brány firewall můžete určit IP rozsahy adres z Internetu, a povolit. Přístup k webu portálu Azure, samotné nebude ovlivněné pravidla brány firewall.

Pokusy o připojení z Internetu a Azure musí nejdřív projít přes bránu firewall než dosáhnou Azure databáze pro databázi MySQL, jak je znázorněno v následujícím diagramu:

![Příklad toku fungování brány firewall](./media/concepts-firewall-rules/1-firewall-concept.png)

## <a name="connecting-from-the-internet"></a>Připojení z Internetu
Pravidla brány firewall na úrovni serveru platí pro všechny databáze na databázi Azure pro server databáze MySQL.

Pokud je IP adresa požadavku v jednom z rozsahů určených v pravidlech brány firewall na úrovni serveru, připojení je povoleno.

Pokud IP adresa požadavku není v rámci rozsahů určených v žádné z pravidel brány firewall na úrovni databáze nebo úrovni serveru, požadavek na připojení se nezdaří.

## <a name="programmatically-managing-firewall-rules"></a>Programová správa pravidel brány firewall
Kromě portálu Azure lze spravovat pravidla brány firewall programově pomocí rozhraní příkazového řádku Azure. Viz také [vytvořit a spravovat databáze Azure pro pravidla brány firewall MySQL pomocí rozhraní příkazového řádku Azure](./howto-manage-firewall-using-cli.md)

## <a name="troubleshooting-the-database-firewall"></a>Řešení potíží s branou firewall databáze
Při přístupu k databázi Microsoft Azure pro službu MySQL server tak, jak očekáváte, zvažte následující body:

* **Změny v seznamu povolených ještě nevstoupilo v platnost:** může být co nejvíce pět minut zpoždění pro změny databáze Azure pro konfiguraci brány firewall serveru MySQL vstoupily v platnost.

* **Přihlášení nemá oprávnění nebo nesprávné heslo byl použit:** Pokud přihlášení nemá oprávnění v databázi Azure pro server databáze MySQL nebo je chybné heslo použít, připojení k databázi Azure MySQL serveru byl odepřen. Vytvoření nastavení brány firewall klientům pouze poskytuje možnost pokusit se o připojení k vašemu serveru – každý klient musí dodat potřebné zabezpečené přihlašovací údaje.

* **Dynamická IP adresa:** Pokud vaše internetové připojení používá dynamické přidělování IP adres a máte problémy dostat se přes bránu firewall, můžete zkusit jedno z následujících řešení:

* Požádejte poskytovatele služeb Internetu (ISP) pro rozsah IP adres přiřazené v klientských počítačích, které přístup k databázi Azure pro server databáze MySQL a pak přidat rozsah IP adres jako pravidlo brány firewall.

* Získejte pro své klientské počítače statické přidělování IP adres a následně přidejte tyto IP adresy jako pravidla brány firewall.

## <a name="next-steps"></a>Další kroky

[Vytvářet a spravovat databáze Azure pro pravidla brány firewall MySQL pomocí portálu Azure](./howto-manage-firewall-using-portal.md)
[vytvořit a spravovat databáze Azure pro pravidla brány firewall MySQL pomocí rozhraní příkazového řádku Azure](./howto-manage-firewall-using-cli.md)
