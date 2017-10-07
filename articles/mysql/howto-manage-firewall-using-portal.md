---
title: "aaaCreate a správě Azure databáze pro pravidla brány firewall MySQL pomocí hello portálu Azure | Microsoft Docs"
description: "Vytvářet a spravovat databáze Azure pro pravidla brány firewall MySQL pomocí hello portálu Azure"
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 06/13/2017
ms.openlocfilehash: 30dbdde4299df564fbf6581419e908186fe3b823
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-azure-database-for-mysql-firewall-rules-using-hello-azure-portal"></a>Vytvářet a spravovat databáze Azure pro pravidla brány firewall MySQL pomocí hello portálu Azure
Pravidla brány firewall na úrovni serveru povolit správci tooaccess databázi Azure pro Server databáze MySQL ze zadané IP adresy nebo rozsahu IP adres. 

## <a name="create-a-server-level-firewall-rule-in-hello-azure-portal"></a>Vytvoření pravidla brány firewall na úrovni serveru v hello portálu Azure

1. V okně serveru MySQL hello, v části nastavení klikněte na **zabezpečení připojení** tooopen hello zabezpečení připojení okně hello Azure Database pro databázi MySQL.

   ![Portál Azure – klikněte na možnost zabezpečení připojení](./media/howto-manage-firewall-using-portal/1-connection-security.png)

2. Klikněte na tlačítko **přidat Moje IP** na panelu nástrojů toocreate hello pravidlo s hello IP adresa počítače, kterou posuzuje hello systému Azure.

   ![Portál Azure – klikněte na tlačítko Přidat Moje IP](./media/howto-manage-firewall-using-portal/2-add-my-ip.png)

3. Před uložením hello konfigurace ověřte IP adresu. V některých situacích hello IP adresu zjištěnými pomocí portálu Azure se liší od hello IP adresu použitou při přístupu k hello Internetu a serverech Azure. Proto může být nutné toochange hello počáteční IP a koncová IP adresa toomake hello funkce pravidla podle očekávání.

   Pomocí vyhledávacího webu nebo jiných online nástroj toocheck vlastní IP adresu (například hledání "co je adresa IP").

   ![Bing co je můj IP](./media/howto-manage-firewall-using-portal/3-what-is-my-ip.png)

4. Přidáte další adresní rozsahy. V hello pravidla pro hello Azure databáze MySQL brány firewall můžete zadat jednu IP adresu nebo rozsah adres. Pokud chcete, aby toolimit hello pravidlo tooone jednu IP adresu, typ hello stejné počáteční IP a koncová IP adresa v poli hello. Otevření brány firewall hello umožňuje správcům a uživatelům tooaccess všechny databáze na hello toowhich MySQL serveru mají platné přihlašovací údaje.

   ![Portál Azure – pravidla brány firewall ](./media/howto-manage-firewall-using-portal/5-specify-addresses.png)


5. Klikněte na tlačítko **Uložit** na hello nástrojů toosave toto pravidlo brány firewall na úrovni serveru. Počkejte hello potvrzení, zda text hello pravidla brány firewall toohello aktualizace proběhla úspěšně.

   ![Portál Azure – klikněte na tlačítko Uložit](./media/howto-manage-firewall-using-portal/4-save-firewall-rule.png)

## <a name="manage-existing-server-level-firewall-rules-through-hello-azure-portal"></a>Spravovat existující pravidla brány firewall na úrovni serveru prostřednictvím hello portálu Azure
Opakujte kroky hello, toomanage pravidla brány firewall hello.
* tooadd hello aktuální počítač, klikněte na tlačítko **+ přidat Moje IP**.
* tooadd další IP adresy, zadejte v hello **název pravidla**, **počáteční IP**, a **KONCOVÁ IP adresa**.
* toomodify existující pravidlo, klikněte na libovolné hello pole v pravidle hello a upravit.
* toodelete existující pravidlo, klikněte na tlačítko se třemi tečkami hello [...] a klikněte na tlačítko **odstranit**.
* Klikněte na tlačítko **Uložit** toosave hello změny.

## <a name="next-steps"></a>Další kroky
- Pomoc při připojování tooan Azure databáze MySQL serveru najdete v tématu [knihovny připojení pro databázi Azure pro databázi MySQL](./concepts-connection-libraries.md)
