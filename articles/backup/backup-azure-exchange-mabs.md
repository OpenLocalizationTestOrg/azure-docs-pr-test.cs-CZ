---
title: "aaaBack nahoru systému Exchange server tooAzure zálohování pomocí serveru Azure Backup | Microsoft Docs"
description: "Zjistěte, jak tooback nahoru systému Exchange server tooAzure zálohování pomocí serveru Azure Backup"
services: backup
documentationcenter: 
author: pvrk
manager: shivamg
editor: 
ms.assetid: e46557e8-2eaf-4ee0-99ea-00fbb8687dca
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: pullabhk
ms.openlocfilehash: db874161151fc57c5b79c41531e18d577f567f66
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-an-exchange-server-tooazure-backup-with-azure-backup-server"></a>Zálohování systému Exchange server tooAzure zálohování pomocí serveru Azure Backup
Tento článek popisuje, jak Microsoft Azure Backup Server (MABS) tooback tooconfigure až tooAzure Microsoft Exchange server.  

## <a name="prerequisites"></a>Požadavky
Než budete pokračovat, ujistěte se, zda je Azure Backup Server [nainstalované a připravené](backup-azure-microsoft-azure-backup.md).

## <a name="mabs-protection-agent"></a>Agent ochrany MABS
tooinstall hello MABS agenta ochrany na hello systému Exchange server, postupujte takto:

1. Ujistěte se, že jsou správně nakonfigurované brány firewall hello. V tématu [konfigurace výjimek brány firewall pro agenta hello](https://technet.microsoft.com/library/Hh758204.aspx).
2. Nainstalujte agenta hello na serveru Exchange hello kliknutím **správy > agenty > nainstalujte** v konzole pro správu MABS. V tématu [agenta ochrany MABS hello instalace](https://technet.microsoft.com/library/hh758186.aspx?f=255&MSPPError=-2147217396) podrobné pokyny.

## <a name="create-a-protection-group-for-hello-exchange-server"></a>Vytvořte skupinu ochrany pro hello Exchange server
1. V konzole pro správu MABS hello, klikněte na **ochrany**a potom klikněte na **nový** na hello tooopen pásu karet nástroje hello **vytvořením nové skupiny ochrany** průvodce.
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

    Když vyberete tuto možnost, kontrola konzistence zálohovaných souborů se spustí na MABS tooavoid hello vstupně-výstupní provoz, který je generovaný systémem hello **eseutil** příkaz hello systém Exchange Server.

   > [!NOTE]
   > toouse, tato možnost, musíte zkopírovat hello Ese.dll a Eseutil.exe soubory toohello C:\Program Files\Microsoft Azure Backup\DPM\DPM\bin adresáře na serveru MAB hello. Jinak se aktivuje hello následující chybě:  
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
12. Vyberte hello dobu, na které hello MAB Server vytvoří hello počáteční replikace a pak klikněte na tlačítko **Další**.
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
1. Klikněte na tlačítko toorecover databáze serveru Exchange, **obnovení** v konzole pro správu MABS hello.
2. Vyhledejte hello databáze systému Exchange, které chcete toorecover.
3. Vyberte bod obnovení online z hello *čas obnovení* rozevíracího seznamu.
4. Klikněte na tlačítko **obnovit** toostart hello **Průvodce obnovením**.

Pro body obnovení online se pět typů obnovení:

* **Obnovit umístění serveru Exchange toooriginal:** hello data budou obnovené toohello původní systému Exchange server.
* **Obnovení databáze tooanother na serveru Exchange Server:** hello data budou tooanother obnovenou databázi na jiném serveru Exchange.
* **Obnovit tooa obnovení databáze:** hello data budou obnovené tooan Exchange obnovení databáze (RDB).
* **Kopírování tooa síťové složky:** hello data budou obnovené tooa síťové složky.
* **Zkopírujte tootape:** Pokud máte knihovny pásků nebo samostatné páskové jednotky připojené a nakonfigurovaná v MABS, hello bod obnovení bude zkopírován tooa volnou pásku.

    ![Zvolte online replikace](./media/backup-azure-backup-exchange-server/choose-online-replication.png)

## <a name="next-steps"></a>Další kroky
* [Azure Backup – nejčastější dotazy](backup-azure-backup-faq.md)
