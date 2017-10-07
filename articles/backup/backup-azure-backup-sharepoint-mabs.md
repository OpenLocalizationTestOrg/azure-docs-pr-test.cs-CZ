---
title: "aaaUse Azure Backup server tooback nahoru tooAzure farmy služby SharePoint | Microsoft Docs"
description: "Pomocí serveru Azure Backup tooback a obnovení dat služby SharePoint. Tento článek obsahuje informace o tooconfigure hello farmy služby SharePoint tak, že požadovaná data se uloží v Azure. Chráněná data služby SharePoint můžete obnovit z disku nebo z Azure."
services: backup
documentationcenter: 
author: pvrk
manager: shivamg
editor: 
ms.assetid: 34ba87a4-91f1-4054-a4a1-272af1e15496
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: pullabhk
ms.openlocfilehash: 350c1ac0f3518f400062f3b586bbe9710a79915a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-a-sharepoint-farm-tooazure"></a>Zálohování tooAzure farmy služby SharePoint
Zálohování služby SharePoint farmu tooMicrosoft Azure pomocí služby Microsoft Azure Backup Server (MABS) v mnohem hello stejným způsobem, který zálohujete jiných zdrojů dat. Azure Backup poskytuje flexibilitu při hello plán zálohování toocreate denní, týdenní, měsíční nebo roční body zálohy a poskytuje možnosti zásad uchovávání informací pro různé body zálohy. Poskytuje také hello schopností toostore místního disku kopie pro rychlé cíle doba obnovení (RTO) a toostore zkopíruje tooAzure pro ekonomické, dlouhodobé uchovávání.

## <a name="sharepoint-supported-versions-and-related-protection-scenarios"></a>Podporované verze služby SharePoint a související scénáře ochrany
Zálohování Azure pro DPM podporuje hello následující scénáře:

| Úloha | Verze | Nasazení služby SharePoint | Ochrana a obnovení |
| --- | --- | --- | --- | --- | --- |
| SharePoint |SharePoint 2013, SharePoint 2010 a SharePoint 2007 SharePoint 3.0 |SharePoint nasazená jako fyzický server nebo virtuální počítač technologie Hyper-V nebo VMware <br> -------------- <br> Technologie AlwaysOn serveru SQL | Ochrana farmy služby SharePoint možnosti obnovení: farma obnovení, databáze a soubor nebo položka seznamu z bodů obnovení disku.  Obnovení z bodů obnovení Azure farmy a databáze. |

## <a name="before-you-start"></a>Než začnete
Existuje několik možností, potřebujete tooconfirm před zálohováním tooAzure farmy služby SharePoint.

### <a name="prerequisites"></a>Požadavky
Než budete pokračovat, ujistěte se, že máte [nainstalované a připravené hello serveru Azure Backup](backup-azure-microsoft-azure-backup.md) tooprotect úlohy.

### <a name="protection-agent"></a>Agent ochrany
agent ochrany Hello musí být nainstalován na server hello se systémem SharePoint, hello serverech se systémem SQL Server a všechny ostatní servery, které jsou součástí farmy služby SharePoint hello. Další informace o tom, tooset až hello agenta ochrany, najdete v části [instalace agenta ochrany](https://technet.microsoft.com/library/hh758034\(v=sc.12\).aspx).  Hello jedinou výjimkou je, že instalujete agenta hello pouze na jednom webovém serveru front-end (WFE). Aplikace DPM vyžaduje hello agenta na jednu pouze tooserve serveru WFE jako hello vstupní bod pro ochranu.

### <a name="sharepoint-farm"></a>Farmy služby SharePoint
Pro každých 10 milionů položek ve farmě hello musí být alespoň 2 GB místa na svazku hello, kde je umístěna složka MABS hello. Tento prostor je nezbytné pro generování katalogu. Pro MABS toorecover konkrétní položky (kolekce webů, weby, seznamy, knihovny dokumentů, složky, jednotlivé dokumenty a položky seznamu) generování katalogu vytvoří seznam hello adres URL, které jsou obsaženy v jednotlivých databázích obsahu. Hello seznam adres URL můžete zobrazit v podokně hello obnovitelných položek v hello **obnovení** oblasti konzoly pro správu MABS úlohy.

### <a name="sql-server"></a>SQL Server
MABS běží pod účtem LocalSystem. tooback zálohu databáze systému SQL Server, MABS potřebuje oprávnění sysadmin na tento účet pro hello serveru se systémem SQL Server. Nastavit NT AUTHORITY\SYSTEM příliš*sysadmin* na hello serveru, který je spuštěn SQL Server předtím, než ho zálohovat.

Pokud hello farmy služby SharePoint databáze systému SQL Server, které jsou nakonfigurovány s aliasy systému SQL Server, nainstalujte na hello front-end webový server, který bude chránit MABS hello součásti klienta systému SQL Server.

### <a name="sharepoint-server"></a>SharePoint Server
Při výkonu závislá na mnoha faktorech, jako je například velikost farmy služby SharePoint, jako obecné pokyny jeden MABS můžete chránit 25 TB farmu služby SharePoint.

### <a name="whats-not-supported"></a>Co není podporováno
* MABS, který chrání farmy služby SharePoint nechrání indexy hledání nebo databáze aplikace služby. Tooconfigure hello ochranu těchto databází, budete potřebovat samostatně.
* MABS neposkytuje zálohování databází serveru SQL služby SharePoint, které jsou hostované na sdílených složkách škálovatelného souborového serveru (SOFS).

## <a name="configure-sharepoint-protection"></a>Konfigurace ochrany Sharepointu
Než budete moct použít MABS tooprotect služby SharePoint, musíte nakonfigurovat hello SharePoint VSS Writer (služby WSS Writer) pomocí **ConfigureSharePoint.exe**.

Můžete najít **ConfigureSharePoint.exe** v složky \bin hello [MABS Instalační cesta] na hello front-end webovém serveru. Tento nástroj poskytuje hello agenta ochrany s přihlašovacími údaji hello pro farmu služby SharePoint hello. Spustíte ji na jeden server WFE. Pokud máte více serverů WFE, vyberte pouze jeden, když konfigurujete skupinu ochrany.

### <a name="tooconfigure-hello-sharepoint-vss-writer-service"></a>tooconfigure hello SharePoint VSS Writer service
1. Na serveru WFE hello na příkazovém řádku přejděte příliš \bin\ [umístění instalace MABS]
2. Zadejte ConfigureSharePoint - EnableSharePointProtection.
3. Zadejte přihlašovací údaje správce farmy hello. Tento účet by měl být členem místní skupiny správců hello na serveru WFE hello. Pokud není správcem farmy hello hello místního správce udělit následující oprávnění na serveru WFE hello:
   * Úplné řízení skupině WSS_Admin_WPG hello grant DPM toohello složky (% Program Files%\Microsoft Azure Backup\DPM).
   * Udělte přístup pro čtení skupině WSS_Admin_WPG hello toohello klíč registru aplikace DPM (HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft Data Protection Manager).

> [!NOTE]
> Vždy, když dojde ke změně v hello přihlašovací údaje správce farmy služby SharePoint, budete potřebovat toorerun ConfigureSharePoint.exe.
>
>

## <a name="back-up-a-sharepoint-farm-by-using-mabs"></a>Zálohování farmy služby SharePoint pomocí MABS
Po nakonfigurování MABS a hello farmy služby SharePoint, jak je popsáno dříve, můžete pomocí MABS chráněný službou SharePoint.

### <a name="tooprotect-a-sharepoint-farm"></a>tooprotect farmy služby SharePoint
1. Z hello **ochrany** klikněte na kartě hello konzole pro správu MABS **nový**.
    ![Nové karty ochrana](./media/backup-azure-backup-sharepoint/dpm-new-protection-tab.png)
2. Na hello **vybrat typ skupiny ochrany** stránku hello **vytvořením nové skupiny ochrany** průvodce, vyberte **servery**a potom klikněte na **Další** .

    ![Typ skupiny ochrany vyberte](./media/backup-azure-backup-sharepoint/select-protection-group-type.png)
3. Na hello **vybrat členy skupiny** obrazovku, vyberte hello zaškrtávací políčko pro server SharePoint hello tooprotect a klikněte na **Další**.

    ![Vybrat členy skupiny](./media/backup-azure-backup-sharepoint/select-group-members2.png)

   > [!NOTE]
   > S nainstalovaným agentem ochrany hello uvidíte hello serveru v Průvodci hello. MABS také ukazuje jeho strukturu. Protože jste spustili ConfigureSharePoint.exe, MABS komunikuje s hello SharePoint VSS Writer service a její odpovídající databáze systému SQL Server a rozpozná hello strukturu farmy služby SharePoint, hello přidružených databází obsahu a všechny odpovídající položky.
   >
   >
4. Na hello **vyberte způsob ochrany dat** stránky, zadejte název hello hello **skupiny ochrany**a vyberte upřednostňovanou *metody ochrany*. Klikněte na **Další**.

    ![Vyberte způsob ochrany dat](./media/backup-azure-backup-sharepoint/select-data-protection-method1.png)

   > [!NOTE]
   > způsob ochrany disku Hello pomáhá toomeet krátkou dobu obnovení cíle.
   >
   >
5. Na hello **zadat krátkodobé cíle** vyberte upřednostňovanou **rozsah uchování** a zjistíte, kdy chcete toooccur zálohy.

    ![Určení krátkodobých cílů](./media/backup-azure-backup-sharepoint/specify-short-term-goals2.png)

   > [!NOTE]
   > Protože obnovení je nejčastěji požadované pro data, která je menší než pět dní, jsme vybrali rozsahem uchování 5 dní na disku a zajistit, že během mimo provozní hodiny, v tomto příkladu se stane hello zálohování.
   >
   >
6. Zkontrolujte hello úložiště fondu místo na disku přidělené pro skupinu ochrany hello a pak klikněte na **Další**.
7. Pro každou skupinu ochrany MABS přiděluje toostore místa na disku a spravovat repliky. V tomto okamžiku MABS, musíte vytvořit kopii hello vybraná data. Vyberte, jak a kdy chcete hello repliky vytvořit a pak klikněte na tlačítko **Další**.

    ![Vyberte způsob vytvoření repliky](./media/backup-azure-backup-sharepoint/choose-replica-creation-method.png)

   > [!NOTE]
   > toomake se, že není uskutečněn síťový provoz, vyberte čas mimo pracovní hodiny.
   >
   >
8. MABS zajistíte integritu dat provedením kontroly konzistence na replice hello. Existují dvě možnosti k dispozici. Můžete definovat kontroly konzistence toorun plán nebo vždy, když se stane nekonzistentní se DPM dá spustit kontrolu konzistence automaticky na hello repliky. Vyberte požadovanou možnost a pak klikněte na tlačítko **Další**.

    ![Kontrola konzistence](./media/backup-azure-backup-sharepoint/consistency-check.png)
9. Na hello **zadat Data Online ochrany** vyberte hello farmy služby SharePoint má tooprotect a pak klikněte na tlačítko **Další**.

    ![Aplikace DPM Protection1 služby SharePoint](./media/backup-azure-backup-sharepoint/select-online-protection1.png)
10. Na hello **zadejte plán Online zálohování** vyberte upřednostňovaný plán a pak klikněte na tlačítko **Další**.

    ![Online_backup_schedule](./media/backup-azure-backup-sharepoint/specify-online-backup-schedule.png)

    > [!NOTE]
    > MABS poskytuje maximálně dvě tooAzure denní zálohy z hello pak k dispozici nejnovější bod zálohy disku. Zálohování Azure můžete také ovládat hello množství šířky pásma sítě WAN, který lze použít pro zálohování ve špičce a špičku pomocí [omezení sítě Azure Backup](https://azure.microsoft.com/documentation/articles/backup-configure-vault/#enable-network-throttling).
    >
    >
11. V závislosti na hello plán zálohování, který jste vybrali, na hello **zadejte zásady uchovávání Online** stránky, vyberte hello zásady uchovávání informací pro denní, týdenní, měsíční a roční body zálohy.

    ![Online_retention_policy](./media/backup-azure-backup-sharepoint/specify-online-retention.png)

    > [!NOTE]
    > MABS používá schéma uchovávání historických. otec SYN ve které je možné vybrat rozdílné zásady pro různé body zálohy.
    >
    >
12. Podobně jako toodisk repliku bodu počáteční odkaz musí toobe vytvoří v Azure. Vyberte vaší toocreate upřednostňovanou možnost ověřování tooAzure kopie prvotní zálohy a pak klikněte na tlačítko **Další**.

    ![Online_replica](./media/backup-azure-backup-sharepoint/online-replication.png)
13. Zkontrolujte vybrané nastavení na hello **Souhrn** a pak klikněte na tlačítko **vytvořit skupinu**. Zobrazí se zpráva o úspěšném provedení a po vytvoření skupiny ochrany hello.

    ![Souhrn](./media/backup-azure-backup-sharepoint/summary.png)

## <a name="restore-a-sharepoint-item-from-disk-by-using-mabs"></a>Obnovení položky služby SharePoint z disku pomocí MABS
V následujícím příkladu hello, hello *položky obnovení služby SharePoint* omylem odstraněný a je třeba toobe obnovit.
![Protection4 MABS služby SharePoint](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection5.png)

1. Otevřete hello **konzole správce aplikace DPM**. Všechny farmy služby SharePoint, které jsou chráněné službou DPM se zobrazují v hello **ochrany** kartě.

    ![Protection3 MABS služby SharePoint](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection4.png)
2. toobegin toorecover hello položku, vyberte hello **obnovení** kartě.

    ![Protection5 MABS služby SharePoint](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection6.png)
3. Můžete hledat SharePoint pro *položky obnovení služby SharePoint* pomocí vyhledávání na základě zástupný znak v rámci obnovení bodu rozsahu.

    ![Protection6 MABS služby SharePoint](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection7.png)
4. Vyberte bod obnovení odpovídající hello z výsledků hledání hello, klikněte pravým tlačítkem na položku hello a pak vyberte **obnovit**.
5. Můžete také procházet různé body obnovení a vyberte databázi nebo položky toorecover. Vyberte **datum > čas obnovení**a potom vyberte správné hello **databáze > farmy služby SharePoint > bod obnovení > položky**.

    ![Protection7 MABS služby SharePoint](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection8.png)
6. Klikněte pravým tlačítkem na položku hello a potom vyberte **obnovit** tooopen hello **Průvodce obnovením**. Klikněte na **Další**.

    ![Revidovat výběr obnovení](./media/backup-azure-backup-sharepoint/review-recovery-selection.png)
7. Vyberte typ hello obnovení má tooperform a pak klikněte na tlačítko **Další**.

    ![Typ obnovení](./media/backup-azure-backup-sharepoint/select-recovery-type.png)

   > [!NOTE]
   > Hello výběr **obnovit toooriginal** v hello příklad obnoví hello položky toohello původní web služby SharePoint.
   >
   >
8. Vyberte hello **proces obnovení** , které chcete toouse.

   * Vyberte **obnovit bez použití farmy obnovení** Pokud nebylo změněno hello farmy služby SharePoint a je stejné jako obnovení hello bod, který je hello obnovena.
   * Vyberte **obnovit použití farmy obnovení** Pokud od vytvoření bodu obnovení hello změnila hello farmy služby SharePoint.

     ![Proces obnovení](./media/backup-azure-backup-sharepoint/recovery-process.png)
9. Dočasně zadat pracovní toorecover hello databázi umístění instance SQL serveru a poskytnout pracovní sdílené složky na MABS a hello serveru se systémem SharePoint toorecover hello položky.

    ![Pracovní Location1](./media/backup-azure-backup-sharepoint/staging-location1.png)

    MABS připojí hello databáze obsahu, který je hostitelem hello SharePoint položky toohello dočasné instance systému SQL Server. Z databáze obsahu hello obnoví hello položky a vloží ho hello pracovní umístění souboru na MABS. Hello obnovit položku, která je na hello pracovního umístění teď toohello toobe exportovat potřeb pracovního umístění na hello farmy služby SharePoint.

    ![Pracovní Location2](./media/backup-azure-backup-sharepoint/staging-location2.png)
10. Vyberte **nastavte možnosti obnovení**a použít toohello nastavení zabezpečení farmy služby SharePoint nebo použít nastavení zabezpečení hello hello bodu obnovení. Klikněte na **Další**.

    ![Možnosti obnovení](./media/backup-azure-backup-sharepoint/recovery-options.png)

    > [!NOTE]
    > Můžete zvolit využití šířky pásma sítě toothrottle hello. Tím se minimalizují dopad toohello provozním serveru během pracovní doby.
    >
    >
11. Zkontrolujte hello souhrnné informace a pak klikněte na tlačítko **obnovit** toobegin obnovení souboru hello.

    ![Obnovení souhrn](./media/backup-azure-backup-sharepoint/recovery-summary.png)
12. Nyní vyberte hello **monitorování** ve hello **konzoly pro správu MABS** tooview hello **stav** hello obnovení.

    ![Stav obnovení](./media/backup-azure-backup-sharepoint/recovery-monitoring.png)

    > [!NOTE]
    > soubor Hello je nyní obnovit. Můžete obnovit hello SharePoint lokality toocheck hello obnovit soubor.
    >
    >

## <a name="restore-a-sharepoint-database-from-azure-by-using-dpm"></a>Obnovení databáze služby SharePoint z Azure pomocí aplikace DPM
1. toorecover databázi obsahu služby SharePoint, procházet různé body obnovení (jak je uvedeno výše) a vyberte bod obnovení hello, které chcete toorestore.

    ![Protection8 MABS služby SharePoint](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection9.png)
2. Dvakrát klikněte na hello SharePoint bodu tooshow hello k dispozici SharePoint katalogu informace pro obnovení.

   > [!NOTE]
   > Protože pro dlouhodobé uchovávání v Azure je chráněn hello farmy služby SharePoint, nejsou dostupné na MABS žádné informace katalogu (metadata). V důsledku toho vždy, když databáze obsahu služby SharePoint v daném okamžiku je toobe obnovit, musíte farmy služby SharePoint hello toocatalog znovu.
   >
   >
3. Klikněte na tlačítko **opětovného zařazení do katalogu**.

    ![Protection10 MABS služby SharePoint](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection12.png)

    Hello **provést novou katalogizaci cloudu** otevře se okno stav.

    ![Protection11 MABS služby SharePoint](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection13.png)

    Po dokončení katalogizaci hello stav změní příliš*úspěch*. Klikněte na **Zavřít**.

    ![Protection12 MABS služby SharePoint](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection14.png)
4. Klikněte na objekt služby SharePoint hello ukazuje hello MABS **obnovení** kartě struktura databáze obsahu tooget hello. Klikněte pravým tlačítkem na položku hello a pak klikněte na **obnovit**.

    ![Protection13 MABS služby SharePoint](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection15.png)
5. V tomto okamžiku postupujte podle hello [kroky obnovení dříve v tomto článku](#restore-a-sharepoint-item-from-disk-using-dpm) toorecover databázi obsahu služby SharePoint z disku.

## <a name="faqs"></a>Nejčastější dotazy
Otázka: je možné obnovit původní umístění toohello položky služby SharePoint, pokud je služba SharePoint nakonfigurována pomocí technologie AlwaysOn serveru SQL (s ochrany na disku)?<br>
Odpověď: Ano hello položka může být obnovené toohello původní web služby SharePoint.

Otázka: je možné obnovit původní umístění toohello databáze služby SharePoint, pokud je služba SharePoint nakonfigurována pomocí technologie AlwaysOn serveru SQL?<br>
A:, protože databáze služby SharePoint jsou konfigurované v SQL AlwaysOn, je nelze změnit, pokud je skupina dostupnosti hello odebrána. V důsledku toho MABS nelze obnovit původní umístění toohello databáze. Můžete obnovit instanci systému SQL Server tooanother databáze systému SQL Server.

## <a name="next-steps"></a>Další kroky
* Další informace o MABS ochrany služby SharePoint – viz [řady Video - DPM ochrany služby SharePoint](http://channel9.msdn.com/Series/Azure-Backup/Microsoft-SCDPM-Protection-of-SharePoint-1-of-2-How-to-create-a-SharePoint-Protection-Group)
