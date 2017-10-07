---
title: "aaaAzure zálohování pro úlohy SQL serveru pomocí aplikace DPM | Microsoft Docs"
description: "Toobacking Úvod do databáze SQL serveru pomocí služby Azure Backup hello"
services: backup
documentationcenter: 
author: adigan
manager: Nkolli
editor: 
ms.assetid: 59df5bec-d959-457d-8731-7b20f7f1013e
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/27/2016
ms.author: adigan;giridham;jimpark;markgal;trinadhk
ms.openlocfilehash: ba78dbf1c7934a259a7bd0bdb7d4467ac75d05a3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-sql-server-tooazure-as-a-dpm-workload"></a>Zálohování serveru SQL Server tooAzure jako zatížení aplikace DPM
Tento článek vás provede kroky konfigurace hello pro zálohování databází systému SQL Server pomocí služby Azure Backup.

tooback až tooAzure databáze systému SQL Server, potřebujete účet Azure. Pokud nemáte účet, můžete jenom několika minut vytvořit Bezplatný zkušební účet. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).

Správa Hello tooAzure zálohování databáze systému SQL Server a obnovení z Azure zahrnuje tři kroky:

1. Vytvořte tooAzure databáze systému SQL Server tooprotect zásady zálohování.
2. Vytvořte tooAzure záložní kopie na vyžádání.
3. Obnovte databázi hello z Azure.

## <a name="before-you-start"></a>Než začnete
Než začnete, ujistěte se, že všechny hello [požadavky](backup-azure-dpm-introduction.md#prerequisites) pro pomocí služby Microsoft Azure Backup byly splněny tooprotect úlohy. požadavky Hello zahrnují úlohy, jako: Vytvoření úložiště záloh, stáhnout přihlašovací údaje trezoru, instalace hello Azure Backup Agent a registraci hello serveru s úložištěm hello.

## <a name="create-a-backup-policy-tooprotect-sql-server-databases-tooazure"></a>Vytvoření tooAzure databáze systému SQL Server tooprotect zásady zálohování
1. Na serveru DPM hello, klikněte na tlačítko hello **ochrany** pracovního prostoru.
2. Na pásu karet nástroje hello, klikněte na tlačítko **nový** toocreate nové skupiny ochrany.

    ![Vytvořit skupinu ochrany](./media/backup-azure-backup-sql/protection-group.png)
3. Aplikace DPM zobrazuje hello úvodní obrazovce s hello pokyny k vytváření **skupiny ochrany**. Klikněte na **Další**.
4. Vyberte **servery**.

    ![Vybrat typ skupiny ochrany – 'servery.](./media/backup-azure-backup-sql/pg-servers.png)
5. Rozbalte počítači serveru SQL Server hello, kde jsou přítomny toobe databáze hello zálohovat. Aplikace DPM zobrazuje různé zdroje dat, které lze zálohovat z tohoto serveru. Rozbalte hello **všechny sdílené složky SQL** a vyberte hello databáze (v tomto případě jsme vybrali ReportServer$ MSDPM2012 a ReportServer$ MSDPM2012TempDB) toobe zálohovat. Klikněte na **Další**.

    ![Vyberte databázi SQL](./media/backup-azure-backup-sql/pg-databases.png)
6. Zadejte název pro skupinu ochrany hello a vyberte hello **chci online ochranu** zaškrtávací políčko.

    ![Způsob ochrany dat – krátkodobou disku & Online Azure](./media/backup-azure-backup-sql/pg-name.png)
7. V hello **zadat krátkodobé cíle** obrazovky, zahrnují hello nezbytné vstupy toocreate body zálohy toodisk.

    Tady vidíte, **rozsah uchování** je nastaven příliš*5 dní*, **četnost synchronizace** nastavena tooonce každých *15 minut* tedy hello frekvence, při které zálohování se. **Expresní úplná záloha** je nastaven příliš*východního*.

    ![Krátkodobé cíle](./media/backup-azure-backup-sql/pg-shortterm.png)

   > [!NOTE]
   > V 8:00 PM (podle toohello obrazovky vstup) zálohování vytváří bod obnovení každý den přenosu hello data, která byla změněna z hello bodu zálohy 8:00 PM předchozí den. Tento proces se nazývá **expresní úplné zálohování**. Při protokoly transakcí hello se synchronizují každých 15 minut, pokud je nutné toorecover hello databáze na 21:00:00 – pak vytváří bod hello přehrání hello protokoly z hello poslední expresní úplné zálohování bod (20: 00 v tomto případě).
   >
   >

8. Klikněte na **Další**

    Aplikace DPM zobrazuje hello celkového prostoru úložiště k dispozici a využití místa na disku potenciální hello.

    ![Přidělení disku](./media/backup-azure-backup-sql/pg-storage.png)

    Ve výchozím nastavení aplikace DPM vytvoří jeden svazek na zdroj dat (databázi systému SQL Server), který se používá pro hello kopie prvotní zálohy. Tento přístup hello Správce logických disků (LDM) omezení zdrojů dat too300 ochrany aplikace DPM (databáze systému SQL Server). toowork obejít toto omezení, vyberte hello **společně umísťovat data do fondu úložiště DPM**, možnost. Pokud použijete tuto možnost, aplikace DPM používá na jednom svazku více zdrojů dat, což umožňuje DPM tooprotect až too2000 databází SQL.

    Pokud **automaticky rozšiřovat svazky hello** je vybraná možnost, aplikace DPM můžete účet pro hello vyšší zálohování svazku s růstem hello provozními daty. Pokud **automaticky rozšiřovat svazky hello** možnost není vybrána, DPM omezuje hello používá úložiště záloh toohello zdroje dat ve skupině ochrany hello.
9. Správci mají hello volba přenosu tato počáteční zálohování ručně (vypnuto síť) tooavoid zahlcení šířky pásma nebo přes síť hello. Mohou také nakonfigurovat hello čas, na které hello může dojít počáteční přenos. Klikněte na **Další**.

    ![Metodu počáteční replikace](./media/backup-azure-backup-sql/pg-manual.png)

    kopie prvotní zálohy Hello vyžaduje přenos hello celého datového zdroje (databázi systému SQL Server) ze serveru DPM pro toohello provozním serveru (počítači serveru SQL Server). Tato data mohou být velký a přenášení hello dat přes síť hello může překročit šířky pásma. Z tohoto důvodu mohou správci tootransfer hello prvotní zálohování: **ručně** (pomocí vyměnitelného média) tooavoid zahlcení šířky pásma, nebo **automaticky přes síť hello** (na zadané čas).

    Po dokončení prvotní zálohování hello hello zbytek hello zálohy jsou přírůstkových záloh v hello kopie prvotní zálohy. Přírůstkové zálohování mívají toobe malé a snadno přenést přes síť hello.
10. Vyberte, když chcete toorun Kontrola konzistence hello a klikněte na tlačítko **Další**.

    ![Kontrola konzistence](./media/backup-azure-backup-sql/pg-consistent.png)

    Aplikaci DPM můžete provést konzistence kontrola toocheck hello integrity hello bodu zálohy. Vypočte kontrolního součtu hello hello záložního souboru na provozním serveru hello (počítači serveru SQL Server v tomto scénáři) a hello zálohovaná data pro tento soubor v aplikaci DPM. V případě hello jsou v konfliktu se předpokládá, že hello soubor zálohy v aplikaci DPM je poškozený. Aplikace DPM rectifies hello zálohovaná data odesláním hello bloky odpovídající neshoda toohello kontrolního součtu. Kontrola konzistence hello je operace náročné na výkon, Správci mají možnost hello plánovaná kontrola konzistence hello nebo spuštěním automaticky.
11. toospecify online ochrany hello zdrojů dat, vyberte hello databáze toobe chráněné tooAzure a klikněte na tlačítko **Další**.

    ![Vyberte zdroje dat](./media/backup-azure-backup-sql/pg-sqldatabases.png)
12. Správci mohou plánů zálohování a zásady uchovávání informací, která vyhovuje své zásady organizace.

    ![Plán a jejich uchovávání](./media/backup-azure-backup-sql/pg-schedule.png)

    V tomto příkladu jsou provedeny zálohy jednou za den v 12:00 PM a 20: 00 (dolní část úvodní obrazovka)

    > [!NOTE]
    > Je dobrým zvykem toohave několik krátkodobé body obnovení na disku pro rychlé obnovení. Tyto body obnovení se používají pro "provozní obnovení". Azure slouží jako dobrý mimo pracoviště s vyšší SLA a záruku dostupnosti.
    >
    >

    **Nejvhodnější**: Ujistěte se, že zálohy Azure jsou naplánovány po dokončení hello místního disku zálohování pomocí aplikace DPM. To umožňuje hello tooAzure zálohování toobe zkopírovat nejnovější disku.

13. Zvolte plán zásad uchovávání informací hello. Hello informace o tom, jak funguje hello zásady uchovávání informací jsou k dispozici v [pomocí Azure Backup tooreplace váš článek infrastruktury pásky](backup-azure-backup-cloud-as-tape.md).

    ![Zásady uchovávání informací](./media/backup-azure-backup-sql/pg-retentionschedule.png)

    V tomto příkladu:

    * Zálohování se pořídí jednou denně na 12:00 PM a 20: 00 (dolní část úvodní obrazovka) a jsou uchovány na 180 dní.
    * Hello zálohování na sobotu ve 12:00 se zachovává kvůli 104 týdny
    * Hello zálohování na poslední sobotu ve 12:00 se uchovávají po dobu 60 měsíců
    * Hello zálohy na poslední sobota dne ve 12:00 se uchovávají 10 let.
14. Klikněte na tlačítko **Další** a hello vyberte příslušnou možnost pro přenos počáteční kopie zálohy tooAzure hello. Můžete zvolit **automaticky přes síť hello** nebo **Offline zálohování**.

    * **Automaticky přes síť hello** přenosy hello tooAzure zálohovaná data podle plánu hello zvolené pro zálohování.
    * Jak **Offline zálohování** funguje je vysvětleno v [pracovní postup Offline zálohování v Azure Backup](backup-azure-backup-import-export.md).

    Vyberte relevantní přenos hello mechanismus toosend hello kopie prvotní zálohy tooAzure a klikněte na tlačítko **Další**.
15. Po prostudování hello podrobnosti zásady v hello **Souhrn** obrazovky, klikněte na hello **vytvořit skupinu** tlačítko toocomplete hello pracovního postupu. Můžete kliknout na hello **Zavřít** tlačítko a monitorování průběhu úlohy hello v pracovním prostoru monitorování.

    ![Vytvoření skupiny ochrany v průběhu](./media/backup-azure-backup-sql/pg-summary.png)

## <a name="on-demand-backup-of-a-sql-server-database"></a>Zálohování na vyžádání databáze systému SQL Server
Při předchozí kroky hello vytvořili zásadu zálohování, "bodu obnovení" se vytvoří jenom v případě, že dochází k zálohování první hello. Místo čekání hello tookick plánovače v, bodu hello kroků vytváření aktivační události hello obnovení ručně.

1. Počkejte, až se zobrazí stav skupiny ochrany hello **OK** hello databáze před vytvořením bodu obnovení hello.

    ![Členové skupiny ochrany](./media/backup-azure-backup-sql/sqlbackup-recoverypoint.png)
2. Klikněte pravým tlačítkem na hello databáze a vyberte **vytvořit bod obnovení**.

    ![Vytvořit bod obnovení Online](./media/backup-azure-backup-sql/sqlbackup-createrp.png)
3. Zvolte **Online ochrany** hello rozevírací nabídky a klikněte na **OK**. Spustí se hello vytvoření bodu obnovení v Azure.

    ![Vytvoření bodu obnovení](./media/backup-azure-backup-sql/sqlbackup-azure.png)
4. Hello průběh úlohy můžete zobrazit v hello **monitorování** prostoru, kde najdete v průběhu úlohy jako jeden znázorňuje následující obrázek hello hello.

    ![Konzola monitorování](./media/backup-azure-backup-sql/sqlbackup-monitoring.png)

## <a name="recover-a-sql-server-database-from-azure"></a>Obnovení databáze systému SQL Server z Azure
Hello následující kroky jsou požadované toorecover chráněná entita (databázi systému SQL Server) ze služby Azure.

1. Otevřete konzolu pro správu serveru aplikace DPM hello. Přejděte příliš**obnovení** prostoru, kde můžete sledovat servery hello zálohované aplikací DPM. Procházejte hello požadovaná databáze (v této rozlišují ReportServer$ MSDPM2012). Vyberte **obnovení z** čas, který končí **Online**.

    ![Vyberte bod obnovení](./media/backup-azure-backup-sql/sqlbackup-restorepoint.png)
2. Klikněte pravým tlačítkem na název databáze hello a klikněte na tlačítko **obnovit**.

    ![Obnovení z Azure](./media/backup-azure-backup-sql/sqlbackup-recover.png)
3. Aplikace DPM zobrazí podrobnosti o hello hello bodu obnovení. Klikněte na **Další**. toooverwrite hello databáze, typ obnovení vyberte hello **obnovit toooriginal instance systému SQL Server**. Klikněte na **Další**.

    ![Obnovit tooOriginal umístění](./media/backup-azure-backup-sql/sqlbackup-recoveroriginal.png)

    V tomto příkladu aplikace DPM umožňuje obnovení instance systému SQL Server tooanother hello databáze nebo tooa samostatné síťové složky.
4. V hello **možnosti obnovení zadejte** obrazovce můžete vybrat možnosti obnovení hello jako toothrottle hello pásma sítě používané při obnovení omezení využití šířky pásma. Klikněte na **Další**.
5. V hello **Souhrn** obrazovky, uvidíte všechny konfigurace hello obnovení dosavadní zadat. Klikněte na tlačítko **obnovit**.

    Hello stav obnovení zobrazí hello databáze obnovení. Můžete kliknout na **Zavřít** tooclose hello průvodce a zobrazení hello průběh hello **monitorování** pracovního prostoru.

    ![Zahájení obnovení](./media/backup-azure-backup-sql/sqlbackup-recoverying.png)

    Po dokončení obnovení hello hello obnovit databáze je aplikace, které jsou konzistentní.

### <a name="next-steps"></a>Další kroky:
• [Azure Backup – nejčastější dotazy](backup-azure-backup-faq.md)
