---
title: "aaaCreate a správě Azure databáze pro pravidla brány firewall PostgreSQL pomocí hello portálu Azure | Microsoft Docs"
description: "Vytvářet a spravovat databáze Azure pro pravidla brány firewall PostgreSQL pomocí hello portálu Azure"
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: 6a41a077168657769e442401e9df9931aa809240
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-azure-database-for-postgresql-firewall-rules-using-hello-azure-portal"></a>Vytvářet a spravovat databáze Azure pro pravidla brány firewall PostgreSQL pomocí hello portálu Azure
Pravidla brány firewall na úrovni serveru povolit správci tooaccess databázi Azure pro PostgreSQL Server ze zadané IP adresy nebo rozsahu IP adres. 

## <a name="prerequisites"></a>Požadavky
toostep prostřednictvím této jak tooguide, budete potřebovat:
- Server [vytvoření databáze Azure pro PostgreSQL](quickstart-create-server-database-portal.md)

## <a name="create-a-server-level-firewall-rule-in-hello-azure-portal"></a>Vytvoření pravidla brány firewall na úrovni serveru v hello portálu Azure
1. V okně serveru PostgreSQL hello, v části nastavení klikněte na **zabezpečení připojení** tooopen hello zabezpečení připojení okně hello databáze Azure pro PostgreSQL.

  ![Portál Azure – klikněte na možnost zabezpečení připojení](./media/howto-manage-firewall-using-portal/1-connection-security.png)

2. Klikněte na tlačítko **přidat Moje IP** na panelu nástrojů hello. Pravidlo tím se vytvoří automaticky s hello IP adresa vašeho počítače, který je dosahovaný podle hello systému Azure.

  ![Portál Azure – klikněte na tlačítko Přidat Moje IP](./media/howto-manage-firewall-using-portal/2-add-my-ip.png)

3. Před uložením hello konfigurace ověřte IP adresu. V některých situacích hello IP adresu zjištěnými pomocí portálu Azure se liší od hello IP adresu použitou při přístupu k hello Internetu a serverech Azure. Proto může být nutné toochange hello počáteční IP a koncová IP adresa toomake hello funkce pravidla podle očekávání.
Pomocí vyhledávacího webu nebo jiných online nástroj toocheck vlastní IP adresu (například Bing vyhledávání se "Jaký je můj IP").

  ![Co je můj IP Bing hledání](./media/howto-manage-firewall-using-portal/3-what-is-my-ip.png)

4. Přidáte další adresní rozsahy. V hello pravidla pro hello Azure databáze PostgreSQL brány firewall můžete zadat jednu IP adresu nebo rozsah adres. Pokud chcete, aby toolimit hello pravidlo tooone jednu IP adresu, typ hello stejné počáteční IP a koncová IP adresa v poli hello. Otevření brány firewall hello umožňuje správcům a uživatelům databáze tooany toologin na hello toowhich PostgreSQL serveru mají platné přihlašovací údaje.

  ![Portál Azure – pravidla brány firewall ](./media/howto-manage-firewall-using-portal/4-specify-addresses.png)

5. Klikněte na tlačítko **Uložit** na hello nástrojů toosave toto pravidlo brány firewall na úrovni serveru. Počkejte hello potvrzení, zda text hello pravidla brány firewall toohello aktualizace proběhla úspěšně.

  ![Portál Azure – klikněte na tlačítko Uložit](./media/howto-manage-firewall-using-portal/5-save-firewall-rule.png)


## <a name="manage-existing-server-level-firewall-rules-through-hello-azure-portal"></a>Spravovat existující pravidla brány firewall na úrovni serveru prostřednictvím hello portálu Azure
Opakujte kroky hello, toomanage pravidla brány firewall hello.
* tooadd hello aktuální počítač, klikněte na tlačítko hello příliš + **přidat Moje IP**. Klikněte na tlačítko **Uložit** toosave hello změny.
* tooadd další IP adresy, zadejte název pravidla a počáteční IP adresa, koncová IP adresa hello. Klikněte na tlačítko **Uložit** toosave hello změny.
* toomodify existující pravidlo, klikněte na libovolné hello pole v pravidle hello a upravit. Klikněte na tlačítko **Uložit** toosave hello změny.
* toodelete existující pravidlo, klikněte na tlačítko se třemi tečkami hello [...] a klikněte na tlačítko Odstranit odebrat hello pravidlo. Klikněte na tlačítko **Uložit** toosave hello změny.

## <a name="next-steps"></a>Další kroky
- Podobně můžete skriptovat příliš[vytvořit a spravovat databáze Azure pro pravidla brány firewall PostgreSQL pomocí rozhraní příkazového řádku Azure](howto-manage-firewall-using-cli.md)
- Pomoc při připojování tooan Azure databáze PostgreSQL serveru najdete v tématu [knihovny připojení pro databázi Azure pro PostgreSQL](concepts-connection-libraries.md)
