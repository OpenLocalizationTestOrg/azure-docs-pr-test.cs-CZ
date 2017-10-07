---
title: aaaManage Azure recovery services trezory a servery | Microsoft Docs
description: "Použijte tento kurz toolearn jak trezory služeb zotavení Azure toomanage a servery."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: tysonn
ms.assetid: 4eea984b-7ed6-4600-ac60-99d2e9cb6d8a
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: markgal
ms.openlocfilehash: b4c35c86faa0828b3c63a13b85c095c0cbaba50e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-and-manage-azure-recovery-services-vaults-and-servers-for-windows-machines"></a>Monitorování a správa serverů a trezorů služby Azure Recovery Services pro počítače s Windows
> [!div class="op_single_selector"]
> * [Resource Manager](backup-azure-manage-windows-server.md)
> * [Classic](backup-azure-manage-windows-server-classic.md)
>
>

V tomto článku najdete přehled hello zálohování monitorování a Správa úloh k dispozici prostřednictvím hello Azure portal a hello Microsoft Azure Backup agent. Tento článek předpokládá už máte předplatné Azure a vytvořili alespoň jeden trezor služeb zotavení.

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]


## <a name="open-a-recovery-services-vault"></a>Otevřete trezoru služeb zotavení

panelu trezoru služeb zotavení Hello se zobrazuje podrobnosti o hello nebo atributů trezoru služeb zotavení.

1. Přihlaste se toohello [portálu Azure](https://portal.azure.com/) pomocí svého předplatného Azure.
2. V nabídce centra hello, klikněte na tlačítko **více služeb**.

    ![Otevřete seznam krok trezory služeb zotavení 1](./media/backup-azure-manage-windows-server/open-rs-vault-list.png) <br/>

3. Chcete tooopen trezoru služeb zotavení. V dialogovém okně hello, začněte psát **služeb zotavení**. Během zadávání hello seznam bude filtrovat podle vašeho zadání. Klikněte na tlačítko **trezory služeb zotavení** toodisplay hello seznam služeb zotavení trezory ve vašem předplatném.

    ![Vytvoření trezoru Recovery Services – krok 1](./media/backup-azure-manage-windows-server/browse-to-rs-vaults-2.png) <br/>

    Otevře se Hello seznamu trezorů služeb zotavení.

    ![Vytvoření trezoru Recovery Services – krok 1](./media/backup-azure-manage-windows-server/list-of-rs-vaults.png) <br/>

4. Hello seznamu trezorů vyberte název hello hello chcete tooopen trezor služeb zotavení. Otevře se okno řídicího panelu trezoru služeb zotavení Hello.

    ![panelu trezoru služeb zotavení](./media/backup-azure-manage-windows-server/rs-vault-blade.png) <br/>

    Teď, když máte otevřený trezor služeb zotavení hello, vyzkoušejte některý z hello monitorování nebo Správa úloh.

## <a name="monitor-backup-jobs-and-alerts"></a>Úlohy zálohování monitorování a výstrahy

Sledování úloh a výstrahy z hello trezor služeb zotavení řídicí panel, kde uvidíte:

* Podrobnosti výstrahy zálohy
* Soubory a složky, jakož i virtuální počítače Azure chráněna v cloudu hello
* Celkový úložný využívat v Azure
* Stav úlohy zálohování

![Úlohy zálohování řídicí panel](./media/backup-azure-manage-windows-server/dashboard-tiles.png)

Kliknutím na hello informace v každé z těchto dlaždic otevřete hello přidružené okno, kde budete spravovat související úlohy.

Z hello horní části hello řídicí panel:

* Nastavení poskytuje přístup k dispozici úlohy zálohování.
* Zálohování – pomáhá zálohujete nové soubory a složky (nebo virtuálních počítačích Azure) služeb zotavení toohello trezoru.
* Odstranění – Pokud obnovení služeb trezoru se už používá, chcete-li odstranit toofree do prostoru úložiště. Odstranění je aktivní jenom po odstranění všech chráněných serverech z trezoru hello.

![Úlohy zálohování řídicí panel](./media/backup-azure-manage-windows-server/dashboard-tasks.png)

## <a name="alerts-for-backups-using-azure-backup-agent"></a>Výstrahy pro zálohování pomocí agenta Azure backup:
| Úroveň výstrahy | Zasílání upozornění |
| --- | --- |
| Kritické |Selhání zálohování, obnovení selhání |
| Upozornění |Zálohování dokončeno s varováním (při méně než sto soubory nejsou zálohovány z důvodu problémů toocorruption a více než jeden milión soubory jsou zálohovány úspěšně) |
| Informační |Žádný |

## <a name="manage-backup-alerts"></a>Správa výstrah zálohování
Klikněte na tlačítko hello **zálohování výstrahy** dlaždice tooopen hello **zálohování výstrahy** okno a spravovat výstrahy.

![Výstrahy zálohy](./media/backup-azure-manage-windows-server/manage-backup-alerts.png)

Hello zálohování výstrahy, které ukazuje, dlaždice hello počet:

* kritické výstrahy nepřeložené za posledních 24 hodin
* upozorňující výstrahy nepřeložené za posledních 24 hodin

Kliknutím na každý z těchto odkazů trvá toohello **zálohování výstrahy** okno s filtrované zobrazení tyto výstrahy (kritický nebo upozornění).

V okně Zálohování výstrahy hello můžete:

* Vyberte příslušné informace o tooinclude hello s oznámení.

    ![Zvolit sloupce](./media/backup-azure-manage-windows-server/choose-alerts-colunms.png)
* Filtrování výstrah pro závažnost, stavu a počáteční nebo koncové časy.

    ![Filtrování výstrah](./media/backup-azure-manage-windows-server/filter-alerts.png)
* Konfigurace oznámení pro závažností, četnosti a příjemce, a také zapnout výstrahy nebo vypnout.

    ![Filtrování výstrah](./media/backup-azure-manage-windows-server/configure-notifications.png)

Pokud **za výstrahy** je vybrán jako hello **oznámení** frekvence dojde k žádné seskupení nebo snížení e-mailů. Každá výstraha výsledkem 1 oznámení. Toto je výchozí nastavení hello a e-mailové řešení hello je také odesílat okamžitě.

Pokud **hodinové Digest** je vybrán jako hello **oznámení** nepřeložené frekvenci, jeden e-mail je odeslán toohello uživatele upozorněním, které nejsou nové výstrahy generované v hello poslední hodinu. E-mailu s řešení odeslání na konci hello hello hodina.

Můžete odesílat upozornění pro hello následující úrovně závažnosti:

* Kritické
* Upozornění
* Informace o

Deaktivovat výstrahy hello s hello **deaktivovat** tlačítka na okno Podrobnosti úlohy hello. Po kliknutí na tlačítko deaktivovat, můžete zadat poznámky k řešení.

Vyberte sloupce hello chcete tooappear jako součást hello výstraha s hello **zvolit sloupce** tlačítko.

> [!NOTE]
> Z hello **nastavení** okně Spravovat výstrahy zálohy výběrem **monitorování a sestavy > Výstrahy a události > výstrahy zálohy** a pak levým na **filtru** nebo  **Konfigurace oznámení**.
>
>

## <a name="manage-backup-items"></a>Správa zálohování položek
Správa místního zálohování je nyní k dispozici na portálu pro správu hello. V části zálohování hello řídicího panelu hello hello **položky zálohování** dlaždice ukazuje počet hello zálohování položek, které chráněné toohello trezoru.

Klikněte na tlačítko **složek souborů** v hello dlaždici zálohování položek.

![Dlaždice položky zálohování](./media/backup-azure-manage-windows-server/backup-items-tile.png)

Hello položky zálohování typu, otevře se okno s hello filtrovat sadu tooFile složku, kde uvidíte každé konkrétní zálohy položka v seznamu.

![Zálohování položek](./media/backup-azure-manage-windows-server/backup-item-list.png)

Pokud vyberete konkrétní zálohování položku ze seznamu hello, uvidíte hello základní podrobnosti pro tuto položku.

> [!NOTE]
> Z hello **nastavení** okně Spravovat soubory a složky tak, že vyberete **chráněné položky > Zálohování položek** a potom vyberete **složek souborů** z hello rozevírací nabídce.
>
>

![Zálohování položek z nastavení](./media/backup-azure-manage-windows-server/backup-files-and-folders.png)

## <a name="manage-backup-jobs"></a>Při správě úloh zálohování
Úlohy zálohování pro místní (při hello na místním serveru je zálohování tooAzure) a zálohování Azure se zobrazují na řídicím panelu hello.

Ve hello zálohování části hello řídicího panelu zobrazí dlaždice úlohy zálohování hello hello počet úloh:

* v průběhu
* hello posledních 24 hodin se nezdařilo.

toomanage vaše úlohy zálohování, klikněte na tlačítko hello **úlohy zálohování** dlaždici, což otevře okno úlohy zálohování hello.

![Zálohování položek z nastavení](./media/backup-azure-manage-windows-server/backup-jobs.png)

Upravte hello informace, které jsou k dispozici v okně úlohy zálohování hello s hello **zvolit sloupce** tlačítko hello horní části stránky hello.

Použití hello **filtru** tlačítko tooselect mezi soubory a složky a záloha virtuálního počítače Azure.

Pokud nevidíte zálohovaný soubory a složky, klikněte na tlačítko **filtru** tlačítka v horní části hello hello stránku a vyberte **soubory a složky** z hello typ položky nabídky.

> [!NOTE]
> Z hello **nastavení** okně Spravovat úlohy zálohování tak, že vyberete **monitorování a sestavy > úlohy > úlohy zálohování** a potom vyberete **složek souborů** rozevíracím hello nabídku.
>
>

## <a name="monitor-backup-usage"></a>Sledování využití zálohování
Ve hello zálohování části hello řídicího panelu zobrazí dlaždice využití zálohování hello hello úložiště využívat v Azure. Využití úložiště je k dispozici pro:

* Využití úložiště LRS cloudu přidružený k trezoru hello
* Využití úložiště GRS cloudu přidružený k trezoru hello

## <a name="manage-your-production-servers"></a>Spravovat vaše produkční servery
Klikněte na provozních serverů, toomanage **nastavení**.

V části Správa klikněte na **infrastruktura zálohování > produkční servery**.

Zobrazí okno Hello provozních serverů všechny dostupné provozních serverů. Klikněte na server v tooopen hello hello seznam-podrobnosti o serveru.

![Chráněné položky](./media/backup-azure-manage-windows-server/production-server-list.png)


## <a name="open-hello-azure-backup-agent"></a>Otevřete hello agenta Azure Backup
Otevřete hello **Microsoft Azure Backup agent** (můžete najít tak, že váš počítač pro *Microsoft Azure Backup*).

![Plán zálohování Windows serveru](./media/backup-azure-manage-windows-server/snap-in-search.png)

Z hello **akce** k dispozici na hello napravo od konzoly agenta zálohování hello provádět následující úlohy správy hello:

* Registrace serveru
* Plán zálohování
* Zálohovat nyní
* Změnit vlastnosti

![Microsoft Azure Backup agent konzoly akce](./media/backup-azure-manage-windows-server/console-actions.png)

> [!NOTE]
> příliš**obnovit Data**, najdete v části [obnovit soubory tooa Windows server nebo klientský počítač systému Windows](backup-azure-restore-windows-server.md).
>
>

## <a name="modify-hello-backup-schedule"></a>Upravte plán zálohování hello
1. V rámci Microsoft Azure Backup agent hello klikněte na tlačítko **plánem zálohování**.

    ![Plán zálohování Windows serveru](./media/backup-azure-manage-windows-server/schedule-backup.png)
2. V hello **Průvodce plánem zálohování** nechte hello **provádět změny toobackup položky nebo časy** možnost a klikněte na tlačítko **Další**.

    ![Plán zálohování Windows serveru](./media/backup-azure-manage-windows-server/modify-or-stop-a-scheduled-backup.png)
3. Pokud chcete tooadd nebo změnit položky na hello **vyberte položky tooBackup** obrazovce klikněte na tlačítko **přidat položky**.

    Můžete také nastavit **nastavení vyloučení** z této stránky v Průvodci hello. Pokud chcete tooexclude soubory nebo typy souborů číst hello postup pro přidání [nastavení vyloučení](#manage-exclusion-settings).
4. Vyberte hello soubory a složky tooback nahoru a klikněte na **nevadí**.

    ![Plán zálohování Windows serveru](./media/backup-azure-manage-windows-server/add-items-modify.png)
5. Zadejte hello **plán zálohování** a klikněte na tlačítko **Další**.

    Můžete naplánovat denní (probíhající maximálně 3krát denně) nebo týdenní zálohování.

    ![Položky k zálohování z Windows Serveru](./media/backup-azure-manage-windows-server/specify-backup-schedule-modify-close.png)

   > [!NOTE]
   > Zadat plán zálohování hello je podrobně popsány v tomto [článku](backup-azure-backup-cloud-as-tape.md).
   >

6. Vyberte hello **zásady uchovávání informací** pro záložní kopii hello a klikněte na tlačítko **Další**.

    ![Položky k zálohování z Windows Serveru](./media/backup-azure-manage-windows-server/select-retention-policy-modify.png)
7. Na hello **potvrzení** obrazovce zkontrolovat hello informace a klikněte na tlačítko **Dokončit**.
8. Po dokončení Průvodce hello vytváření hello **plán zálohování**, klikněte na tlačítko **Zavřít**.

    Po úpravě ochrany, můžete ověřit, zálohování, ke které spouštějí správně podle budete toohello **úlohy** kartě a potvrzující, že změny se projeví v hello zálohování úloh.

## <a name="enable-network-throttling"></a>Povolit omezení šířky pásma sítě

agent Azure Backup Hello poskytuje omezování karta, která vám umožní toocontrol použití šířky pásma sítě při přenosu dat. Tento ovládací prvek může být užitečné, pokud potřebujete tooback dat během pracovní doby, ale nechcete, aby proces zálohování toointerfere hello s ostatní internetový provoz. Omezování dat přenos platí tooback nahoru a obnovit aktivity.  

tooenable omezení:

1. V hello **agenta zálohování**, klikněte na tlačítko **změnit vlastnosti**.
2. Na hello ** omezení karty, vyberte **povolit využití šířky pásma Internetu u operací zálohování omezení**.

    ![Omezení šířky pásma sítě](./media/backup-azure-manage-windows-server/throttling-dialog.png)

    Jakmile jste povolili omezování, zadejte hello povolený šířky pásma pro přenos zálohovaných dat během **pracovní hodiny** a **nepracovní hodiny**.

    hodnoty šířky pásma Hello začít až 512 kilobitů za sekundu (Kbps) a můžete přejít do too1023 megabajtů za sekundu (Mbps). Můžete také určit hello zahájení a dokončení pro **pracovní hodiny**, a které dny v týdnu hello jsou považovány za pracovních dnů. čas Hello mimo určené pracovní hodiny hello je považovány toobe mimo pracovní dobu.
3. Klikněte na **OK**.

## <a name="manage-exclusion-settings"></a>Spravovat nastavení vyloučení
1. Otevřete hello **Microsoft Azure Backup agent** (najdete ho tak, že váš počítač pro *Microsoft Azure Backup*).

    ![Plán zálohování Windows serveru](./media/backup-azure-manage-windows-server/snap-in-search.png)
2. V rámci Microsoft Azure Backup agent hello klikněte na tlačítko **plánem zálohování**.

    ![Plán zálohování Windows serveru](./media/backup-azure-manage-windows-server/schedule-backup.png)
3. V Průvodci plánování zálohování hello nechte hello **provádět změny toobackup položky nebo časy** možnost a klikněte na tlačítko **Další**.

    ![Plán zálohování Windows serveru](./media/backup-azure-manage-windows-server/modify-or-stop-a-scheduled-backup.png)
4. Klikněte na tlačítko **nastavení vyloučení**.

    ![Plán zálohování Windows serveru](./media/backup-azure-manage-windows-server/exclusion-settings.png)
5. Klikněte na tlačítko **Přidat vyloučení**.

    ![Plán zálohování Windows serveru](./media/backup-azure-manage-windows-server/add-exclusion.png)
6. Vyberte umístění hello a potom klikněte na **OK**.

    ![Plán zálohování Windows serveru](./media/backup-azure-manage-windows-server/exclusion-location.png)
7. Přidat příponu souboru hello v hello **typ souboru** pole.

    ![Plán zálohování Windows serveru](./media/backup-azure-manage-windows-server/exclude-file-type.png)

    Přidání rozšíření MP3

    ![Plán zálohování Windows serveru](./media/backup-azure-manage-windows-server/exclude-mp3.png)

    tooadd jiné rozšíření, klikněte na tlačítko **Přidat vyloučení** a zadejte jinou příponu typu souboru (Přidání rozšíření .jpeg).

    ![Plán zálohování Windows serveru](./media/backup-azure-manage-windows-server/exclude-jpg.png)
8. Pokud jste přidali všechny hello rozšíření, klikněte na tlačítko **OK**.
9. Kliknutím na pokračovat prostřednictvím hello Průvodce plánem zálohování **Další** dokud hello **potvrzovací stránku**, pak klikněte na tlačítko **Dokončit**.

    ![Plán zálohování Windows serveru](./media/backup-azure-manage-windows-server/finish-exclusions.png)

## <a name="frequently-asked-questions"></a>Nejčastější dotazy
**OTÁZKA Č. 1. Stav úlohy zálohování Hello zobrazuje jako dokončené v hello Azure backup agent, proč není ho získat projeví okamžitě v portálu?**

Odpověď č. 1: Existuje v maximální zpoždění 15 minut mezi hello stav úlohy zálohování se v hello Azure backup agent a hello portálu Azure.

**Q.2 při selhání úlohy zálohování, jak dlouho trvá tooraise výstrahu?**

A.2 výstraha se vyvolá v rámci 20 minut hello Azure selhání zálohování.

**3. ČTVRTLETÍ. Je případ, kdy e-mailu neodešle, pokud jsou nakonfigurované oznámení?**

Odpověď 3. Níže jsou hello případech při hello upozornění nebude odesláno v pořadí tooreduce hello výstrahy nepůsobily:

* Pokud oznámení jsou nakonfigurovány každou hodinu a výstrahu, je vyvolána a vyřešené v rámci hello hodinu
* Úloha je zrušena.
* Druhý úloha zálohování se nezdařila, protože probíhá úloha původní zálohování.

## <a name="troubleshooting-monitoring-issues"></a>Monitorování řešení potíží
**Problém:** úlohy a výstrahy z agenta Azure Backup hello se nezobrazí na portálu hello.

**Řešení potíží:** hello procesu ```OBRecoveryServicesManagementAgent```, odešle hello úlohy a výstrahy toohello dat služby zálohování Azure. Příležitostně tento proces může zablokovat nebo vypnutí.

1. proces hello tooverify neběží, otevřete **Správce úloh** a zkontrolujte, jestli hello ```OBRecoveryServicesManagementAgent``` proces běží.
2. Za předpokladu, že proces hello neběží, otevřete **ovládací panely** a procházet hello seznam služeb. Spustit nebo restartovat **správy agenta služeb zotavení Microsoft Azure**.

    Další informace vyhledejte hello protokoly na:<br/>
   `<AzureBackup_agent_install_folder>\Microsoft Azure Recovery Services Agent\Temp\GatewayProvider*`Například:<br/>
   `C:\Program Files\Microsoft Azure Recovery Services Agent\Temp\GatewayProvider0.errlog`

## <a name="next-steps"></a>Další kroky
* [Obnovení systému Windows Server nebo klienta Windows z Azure](backup-azure-restore-windows-server.md)
* toolearn Další informace o zálohování Azure, najdete v části [Přehled zálohování Azure](backup-introduction-to-azure-backup.md)
* Navštivte hello [fóru služby Azure Backup](http://go.microsoft.com/fwlink/p/?LinkId=290933)
