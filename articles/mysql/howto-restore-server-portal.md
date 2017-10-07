---
title: "aaaHow tooRestore serveru ve službě Azure Database pro databázi MySQL | Microsoft Docs"
description: "Tento článek popisuje, jak hello toorestore na serveru v Azure databázi MySQL pomocí portálu Azure."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: 4b990d5b37c5d4924de9571192b923e3c81094ce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toobackup-and-restore-a-server-in-azure-database-for-mysql-using-hello-azure-portal"></a>Jak hello tooBackup a obnovení na serveru v Azure databázi MySQL pomocí portálu Azure

## <a name="backup-happens-automatically"></a>Zálohování se automaticky stane
Při použití Azure databáze pro databázi MySQL, služba hello databáze automaticky provede zálohování služby hello každých 5 minut. 

Hello zálohy jsou k dispozici po dobu 7 dnů, při použití úroveň Basic a 35 dní při použití úrovně Standard. Další informace najdete v tématu [Azure databáze MySQL úrovně služeb](concepts-service-tiers.md)

Pomocí této funkce automatického zálohování můžete obnovit hello server a všechny jeho databáze do nový server tooan dříve v daném okamžiku.

## <a name="restore-in-hello-azure-portal"></a>Obnovit v hello portálu Azure
Databáze pro databázi MySQL Azure umožňuje toorestore hello server back tooa bod v čase a do tooa novou kopii hello server. Můžete použít tento nový server toorecover vaše data. 

Například pokud tabulka byla omylem vyřadit v poledne v současné době můžete obnovit toohello čas těsně před poledne a načíst hello chybějící tabulky a data z této novou kopii hello server.

Následující kroky obnovení hello ukázkový server tooa bod v čase Hello:

1. Přihlaste se k hello [portálu Azure](https://portal.azure.com/)

2. Vyhledejte vaší databázi Azure pro server databáze MySQL. V levém podokně hello vyberte **všechny prostředky**, pak vyberte svůj server ze seznamu hello.

3.  Na hello horní části okna Přehled hello serveru, klikněte na tlačítko **obnovení** na panelu nástrojů hello. Otevře se okno obnovení Hello.
![Klikněte na tlačítko Obnovit](./media/howto-restore-server-portal/click-restore-button.png)

4. Vyplňte formulář obnovení hello hello požadované informace:

- **Obnovení bodu (UTC)**: pomocí hello výběr data a času výběr, vyberte v okamžiku toorestore k. Zadaný čas Hello je ve formátu UTC, proto musíte pravděpodobně tooconvert hello místního času na čas UTC.
- **Obnovení serveru toonew**: Zadejte název toorestore hello existující server do nového serveru.
- **Umístění**: volba oblasti hello automaticky naplní s hello zdrojový server oblasti a nedá se změnit.
- **Cenová úroveň**: hello cenová úroveň volba automaticky naplní s hello stejné cenovou úroveň jako zdrojový server hello a nelze je zde změnit. 
![Možnosti PITR obnovení](./media/howto-restore-server-portal/pitr-restore.png)

5. Klikněte na tlačítko **OK** toorestore hello server toorestore tooa bod v čase. 

6. Po dokončení obnovení hello vyhledejte hello nový server, který byl vytvořen hello tooverify databáze byly obnoveny podle očekávání.

## <a name="next-steps"></a>Další kroky
- [Knihovny připojení pro databázi Azure pro databázi MySQL](concepts-connection-libraries.md)