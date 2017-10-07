---
title: "Jak tooRestore na Server v Azure databáze PostgreSQL | Microsoft Docs"
description: "Tento článek popisuje, jak hello toorestore na serveru v Azure databázi PostgreSQL pomocí portálu Azure."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 07/20/2017
ms.openlocfilehash: bc7351f384607397806d837afd3e1d7a26575e0e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toobackup-and-restore-a-server-in-azure-database-for-postgresql-using-hello-azure-portal"></a>Jak hello tooBackup a obnovení na serveru v Azure databázi PostgreSQL pomocí portálu Azure

## <a name="backup-happens-automatically"></a>Zálohování se automaticky stane
Při použití Azure databáze PostgreSQL, služba hello databáze automaticky provede zálohování služby hello každých 5 minut. 

Hello zálohy jsou k dispozici po dobu 7 dnů, při použití úroveň Basic a 35 dní při použití úrovně Standard. Další informace najdete v tématu [databáze Azure pro PostgreSQL úrovně služeb](concepts-service-tiers.md)

Pomocí této funkce automatického zálohování můžete obnovit hello server a všechny jeho databáze do nový server tooan dříve v daném okamžiku.

## <a name="restore-in-hello-azure-portal"></a>Obnovit v hello portálu Azure
Azure databázi PostgreSQL umožňuje toorestore hello server back tooa bod v čase a do tooa novou kopii hello server. Můžete použít tento nový server toorecover vaše data. 

Například pokud tabulka byla omylem vyřadit v poledne v současné době můžete obnovit toohello čas těsně před poledne a načíst hello chybějící tabulky a data z této novou kopii hello server.

Následující kroky obnovení hello ukázkový server tooa bod v čase Hello:
1. Přihlaste se k hello [portálu Azure](https://portal.azure.com/)
2. Vyhledejte vaší databázi Azure pro PostgreSQL server. V hello portálu Azure, klikněte na **všechny prostředky** z nabídky na levé straně hello a zadejte název hello, jako například **mypgserver 20170401**, toosearch pro existující server. Klikněte na název serveru hello uvedené v výsledek hledání hello. Hello **přehled** stránky pro váš server otevře a poskytuje možnosti pro další konfiguraci.

   ![Portál Azure – prohledávání toolocate vašeho serveru](media/postgresql-howto-restore-server-portal/1-locate.png)

3. Na hello horní části okna Přehled hello serveru, klikněte na tlačítko **obnovení** na panelu nástrojů hello. Otevře se okno obnovení Hello.

   ![Azure databáze pro obnovení PostgreSQL – přehled – tlačítko](./media/postgresql-howto-restore-server-portal/2_server.png)

4. Vyplňte formulář obnovení hello hello požadované informace:

   ![Azure databázi PostgreSQL - informace o obnovení ](./media/postgresql-howto-restore-server-portal/3_restore.png)
  - **Bod obnovení**: Vyberte bodu v čase, k níž dojde před hello server byl změněn
  - **Cílový server**: Zadejte nový název serveru, který chcete toorestore k
  - **Umístění**: nelze vybrat hello oblast, ve výchozím nastavení je stejný jako zdrojový server hello
  - **Cenová úroveň**: tuto hodnotu nelze změnit, při obnovení serveru. Je stejný jako zdrojový server hello. 

5. Klikněte na tlačítko **OK** toorestore hello server toorestore tooa bod v čase. 

6. Po dokončení obnovení hello vyhledejte hello nový server, který se vytvoří hello tooverify data byla obnovena podle očekávání.

## <a name="next-steps"></a>Další kroky
- [Knihovny připojení pro databázi Azure pro PostgreSQL](concepts-connection-libraries.md)
