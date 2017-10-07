---
title: "aaaBack nahoru systému Exchange server tooAzure zálohování pomocí System Center 2012 R2 DPM | Microsoft Docs"
description: "Zjistěte, jak tooback nahoru systému Exchange server tooAzure zálohování pomocí System Center 2012 R2 DPM"
services: backup
documentationcenter: 
author: MaanasSaran
manager: NKolli1
editor: 
ms.assetid: 13f32256-888e-416e-a78b-40c2a26a5939
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2016
ms.author: masaran;jimpark;delhan;trinadhk;markgal
ms.openlocfilehash: fa99296d095c180333474b6d419ebc5ec727547a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-an-exchange-server-tooazure-backup-with-system-center-2012-r2-dpm"></a>Zálohování systému Exchange server tooAzure zálohování pomocí System Center 2012 R2 DPM
Tento článek popisuje, jak tooconfigure System Center 2012 R2 Data Protection Manager (DPM) tooback serveru Microsoft Exchange Server příliš Azure Backup.  

## <a name="updates"></a>Aktualizace
toosuccessfully registrace hello serveru DPM pomocí zálohování Azure, je nutné nainstalovat hello nejnovější kumulativní aktualizace pro System Center 2012 R2 DPM a hello nejnovější verzi hello Azure Backup Agent. Získat nejnovější kumulativní aktualizaci hello z hello [Microsoft Catalog](http://catalog.update.microsoft.com/v7/site/Search.aspx?q=System%20Center%202012%20R2%20Data%20protection%20manager).

> [!NOTE]
> Hello příklady v tomto článku je nainstalována verze 2.0.8719.0 hello Azure Backup Agent a kumulativní aktualizací 6 je nainstalovaná na System Center 2012 R2 DPM.
>
>

## <a name="prerequisites"></a>Požadavky
Než budete pokračovat, ujistěte se, že všechny hello [požadavky](backup-azure-dpm-introduction.md#prerequisites) pro pomocí služby Microsoft Azure Backup byly splněny tooprotect úlohy. Tyto součásti zahrnují hello následující:

* Trezor záloh na hello Azure lokality byl vytvořen.
* Agent a přihlašovací údaje trezoru byly stažené toohello serveru aplikace DPM.
* Hello agent je nainstalován na serveru DPM hello.
* přihlašovací údaje trezoru Hello byly použité tooregister hello DPM server.
* Pokud chráníte Exchange 2016, upgradujte prosím tooDPM 2012 R2 UR9 nebo novější

## <a name="dpm-protection-agent"></a>Agent ochrany aplikace DPM
agent ochrany DPM hello tooinstall v hello systému Exchange server, postupujte takto:

1. Ujistěte se, že jsou správně nakonfigurované brány firewall hello. V tématu [konfigurace výjimek brány firewall pro agenta hello](https://technet.microsoft.com/library/Hh758204.aspx).
2. Nainstalujte agenta hello na serveru Exchange hello kliknutím **správy > agenty > nainstalujte** v konzoli správce aplikace DPM. V tématu [agenta ochrany aplikace DPM nainstalujte hello](https://technet.microsoft.com/library/hh758186.aspx?f=255&MSPPError=-2147217396) podrobné pokyny.

## <a name="create-a-protection-group-for-hello-exchange-server"></a>Vytvořte skupinu ochrany pro hello Exchange server
1. V konzole správce aplikace DPM hello, klikněte na **ochrany**a potom klikněte na **nový** na hello tooopen pásu karet nástroje hello **vytvořením nové skupiny ochrany** průvodce.
2. Na hello **úvodní** obrazovky hello průvodce klikněte na tlačítko **Další**.
3. Na hello **vybrat typ skupiny ochrany** vyberte **servery** a klikněte na tlačítko **Další**.
4. Databáze serveru Exchange vyberte hello má tooprotect a klikněte na tlačítko **Další**.

   > [!NOTE]
   > Pokud chráníte Exchange 2013, zkontrolujte hello [požadavky serveru Exchange 2013](https://technet.microsoft.com/library/dn751029.aspx).
   >
   >

    V následujícím příkladu hello je vybraná databáze hello Exchange 2010.

    ![Vybrat členy skupiny](./media/backup-azure-backup-exchange-server/select-group-members.png)
5. Vyberte způsob ochrany dat hello.

    Název skupiny ochrany hello a pak vyberte oba hello následující možnosti:

   * Chci krátkodobou ochranu pomocí disku.
   * Chci online ochranu.
6. Klikněte na **Další**.
7. Vyberte hello **integritu dat toocheck spuštěním programu Eseutil** možnost, pokud chcete, aby toocheck hello integritu databáze systému Exchange Server hello.

    Když vyberete tuto možnost, kontrola konzistence zálohovaných souborů se spustí na hello DPM serveru tooavoid hello vstupně-výstupní provoz, který se vygeneruje spuštěním hello **eseutil** příkaz hello systém Exchange Server.

   > [!NOTE]
   > toouse, tato možnost, musíte zkopírovat hello Ese.dll a Eseutil.exe soubory toohello C:\Program Files\Microsoft System Center 2012 R2\DPM\DPM\bin adresáře na serveru aplikace DPM hello. Jinak se aktivuje hello následující chybě:  
   > ![Chyba eseutil](./media/backup-azure-backup-exchange-server/eseutil-error.png)
   >
   >
8. Klikněte na **Další**.
9. Vyberte hello databáze pro **zálohování kopírováním**a potom klikněte na **Další**.

   > [!NOTE]
   > Pokud nevyberete "Úplného zálohování" pro alespoň jednu DAG kopii databáze, nebudou zkráceny protokoly.
   >
   >
10. Konfigurace cíle hello **krátkodobé zálohování**a potom klikněte na **Další**.
11. Zkontrolujte hello volného místa na disku a pak klikněte na tlačítko **Další**.
12. Vyberte hello dobu, na které hello DPM server bude hello počáteční replikace a potom klikněte na **Další**.
13. Vyberte možnosti kontroly konzistence hello a pak klikněte na tlačítko **Další**.
14. Zvolte hello databáze má tooback až tooAzure a pak klikněte na tlačítko **Další**. Například:

    ![Zadejte data pro online ochranu](./media/backup-azure-backup-exchange-server/specify-online-protection-data.png)
15. Definujte plán hello pro **Azure Backup**a potom klikněte na **Další**. Například:

    ![Zadejte plán online zálohování](./media/backup-azure-backup-exchange-server/specify-online-backup-schedule.png)

    > [!NOTE]
    > Všimněte si, body obnovení Online jsou založené na expresní úplné body obnovení. Proto musíte naplánovat bodu obnovení online hello po dobu hello, která je určená pro hello expresní úplné obnovení bodu.
    >
    >
16. Konfigurace zásad uchovávání hello **Azure Backup**a potom klikněte na **Další**.
17. Zvolte možnost online replikace a klikněte na tlačítko **Další**.

    Pokud máte velké databáze, může trvat dlouhou dobu pro počáteční zálohování toobe hello vytvořili přes síť hello. tooavoid tento problém, můžete vytvořit zálohu do offline režimu.  

    ![Zadejte zásady online uchovávání dat.](./media/backup-azure-backup-exchange-server/specify-online-retention-policy.png)
18. Potvrďte nastavení hello a pak klikněte na tlačítko **vytvořit skupinu**.
19. Klikněte na **Zavřít**.

## <a name="recover-hello-exchange-database"></a>Obnovení databáze systému Exchange hello
1. Klikněte na tlačítko toorecover databáze serveru Exchange, **obnovení** v hello konzole správce aplikace DPM.
2. Vyhledejte hello databáze systému Exchange, které chcete toorecover.
3. Vyberte bod obnovení online z hello *čas obnovení* rozevíracího seznamu.
4. Klikněte na tlačítko **obnovit** toostart hello **Průvodce obnovením**.

Pro body obnovení online se pět typů obnovení:

* **Obnovit umístění serveru Exchange toooriginal:** hello data budou obnovené toohello původní systému Exchange server.
* **Obnovení databáze tooanother na serveru Exchange Server:** hello data budou tooanother obnovenou databázi na jiném serveru Exchange.
* **Obnovit tooa obnovení databáze:** hello data budou obnovené tooan Exchange obnovení databáze (RDB).
* **Kopírování tooa síťové složky:** hello data budou obnovené tooa síťové složky.
* **Zkopírujte tootape:** Pokud máte páskovou knihovnu nebo samostatnou páskovou jednotku hello připojené a nakonfigurované na serveru aplikace DPM, hello bod obnovení bude zkopírován tooa volnou pásku.

    ![Zvolte online replikace](./media/backup-azure-backup-exchange-server/choose-online-replication.png)

## <a name="next-steps"></a>Další kroky
* [Azure Backup – nejčastější dotazy](backup-azure-backup-faq.md)
