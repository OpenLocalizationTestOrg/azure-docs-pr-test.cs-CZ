---
title: "zálohy virtuálních počítačů nasazených Resource Managerem aaaMonitor | Microsoft Docs"
description: "Monitorování události a upozornění ze záloh virtuálních počítačů nasazených Resource Managerem. Odeslání e-mailu na základě výstrah."
services: backup
documentationcenter: dev-center-name
author: markgalioto
manager: carmonm
editor: 
ms.assetid: fed32015-2db2-44f8-b204-d89f6fd1bea2
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/21/2016
ms.author: markgal;trinadhk;giridham;
ms.openlocfilehash: bf45cbaa05621b2365c26bafa1bd8223a444c1fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-alerts-for-azure-virtual-machine-backups"></a>Správa výstrah pro virtuální počítače Azure
Výstrahy jsou odpovědí z hello služby, aby prahová hodnota události byla splněna nebo překročení. Znalost, při spuštění problémů může být důležité tookeeping obchodní náklady dolů. Výstrahy obvykle nedojde k podle plánu, a proto je užitečné tooknow co nejdříve po generována výstraha. Například pokud se nezdaří úlohy zálohování nebo obnovení, zobrazení výstrahy do pěti minut hello selhání. V panelu trezoru hello hello dlaždice výstrah zálohování zobrazuje události kritický a úroveň pro upozornění. V nastavení hello zálohování výstrah můžete zobrazit všechny události. Ale co dělat v případě výstrahu při práci na samostatné problém? Pokud si nejste jisti, když se stane hello výstraha, může to být méně závažné potíže, nebo ho mohl ohrozit zabezpečení dat. Pokud k ní dojde, že hello správné osoby víte výstrahy - toomake nakonfigurovat hello služby toosend oznámení o výstrahách e-mailem. Podrobnosti o nastavení e-mailová oznámení najdete v tématu [konfigurace oznámení](backup-azure-monitor-vms.md#configure-notifications).

## <a name="how-do-i-find-information-about-hello-alerts"></a>Jak se najít informace o výstrahách hello?
tooview informace o hello událost, která způsobila výstrahu, je nutné otevřít okno zálohování výstrahy hello. Existují dva způsoby tooopen hello výstrahy zálohování okno: z hello výstrahy zálohování dlaždici na řídicím panelu trezoru hello nebo v okně hello výstrahy a události.

okno zálohování výstrahy hello tooopen z výstrah zálohování dlaždice:

* Na hello **zálohování výstrahy** dlaždici na řídicím panelu trezoru hello, klikněte na tlačítko **kritický** nebo **upozornění** tooview hello provozní události pro tuto úroveň závažnosti.

    ![Dlaždice výstrahy zálohy](./media/backup-azure-monitor-vms/backup-alerts-tile.png)

okno zálohování výstrahy tooopen hello v okně hello výstrahy a události:

1. Na panelu trezoru hello, klikněte na **všechna nastavení**. ![Tlačítko všechna nastavení](./media/backup-azure-monitor-vms/all-settings-button.png)
2. Na hello **nastavení** okně klikněte na tlačítko **výstrahy a události**. ![Tlačítko výstrahy a události](./media/backup-azure-monitor-vms/alerts-and-events-button.png)
3. Na hello **výstrahy a události** okně klikněte na tlačítko **zálohování výstrahy**. ![Zálohování tlačítko výstrahy](./media/backup-azure-monitor-vms/backup-alerts.png)

    Hello **zálohování výstrahy** otevře se okno a zobrazí hello filtrované výstrahy.

    ![Dlaždice výstrahy zálohy](./media/backup-azure-monitor-vms/backup-alerts-critical.png)
4. tooview podrobné informace o určité výstraze, z hello seznam událostí, klikněte na výstrahu tooopen hello jeho **podrobnosti** okno.

    ![Podrobnosti události](./media/backup-azure-monitor-vms/audit-logs-event-detail.png)

    najdete v části atributy hello toocustomize zobrazí v seznamu hello [zobrazit další událost atributy](backup-azure-monitor-vms.md#view-additional-event-attributes)

## <a name="configure-notifications"></a>Konfigurace oznámení
 Můžete nakonfigurovat hello služby toosend e-mailová oznámení pro výstrahy hello, které došlo v průběhu hello za hodinu nebo pokud dojde k určité typy událostí.

tooset si e-mailová oznámení pro výstrahy

1. V nabídce hello výstrahy zálohování, klikněte na tlačítko **konfigurace oznámení**

    ![Zálohování nabídky výstrahy](./media/backup-azure-monitor-vms/backup-alerts-menu.png)

    Otevře se okno oznámení konfigurovat Hello.

    ![Konfigurace oznámení okno](./media/backup-azure-monitor-vms/configure-notifications.png)
2. V okně oznámení konfigurovat hello, e-mailových oznámení, klikněte na tlačítko **na**.

    Hello příjemce a dialogová okna závažnost mají hvězdičkami další toothem, protože tyto informace se vyžaduje. Zadejte aspoň jednu e-mailovou adresu a vyberte alespoň jeden závažnosti.
3. V hello **příjemce (e-mailu)** dialogové okno, typ hello e-mailové adresy pro kdo dostávat oznámení hello. Použijte formát hello: username@domainname.com. Jednotlivé e-mailové adresy oddělte středníkem (;).
4. V hello **oznámení** oblasti, zvolte **za výstraha** toosend oznámení, když hello zadaný výstrahy, nebo **hodinové Digest** toosend souhrnné hello poslední hodinu.
5. V hello **závažnost** dialogovém okně, vyberte jeden nebo více úrovní, které chcete tootrigger e-mailové oznámení.
6. Klikněte na **Uložit**.

   ### <a name="what-alert-types-are-available-for-azure-iaas-vm-backup"></a>Jaké typy výstrah jsou k dispozici pro zálohování virtuálních počítačů Azure IaaS?
   | Úroveň výstrahy | Zasílání upozornění |
   | --- | --- |
   | Kritické |Selhání zálohování, obnovení selhání |
   | Upozornění |Žádný |
   | Informační |Žádný |

### <a name="are-there-situations-where-email-isnt-sent-even-if-notifications-are-configured"></a>Dochází k situacím, že se e-mail neodešle, i když jsou oznámení nakonfigurovaná?
Existují situacích, kde se neposílají výstrahu, i když hello oznámení správně nakonfigurovaný. V hello neodešlou následující situace e-mailová oznámení výstrahy nepůsobily tooavoid:

* Pokud oznámení jsou nakonfigurované tooHourly ověřování algoritmem Digest, výstraha je vyvolána a vyřešené v rámci hello hodinu.
* Úloha Hello je zrušena.
* Úloha zálohování se aktivuje a pak se nezdaří a probíhá další úloha zálohování.
* Spustí naplánované úlohy zálohování pro virtuální počítač povolena Resource Manager, ale hello virtuálního počítače již existuje.

## <a name="customize-your-view-of-events"></a>Přizpůsobení zobrazení událostí
Hello **protokoly auditu** nastavení obsahuje předem definovaná sadu filtrů a sloupce, které zobrazuje informace o provozní události. Hello zobrazení můžete přizpůsobit tak, aby při hello **události** otevře se okno, zobrazuje hello informace, které chcete.

1. V hello [panelu trezoru](backup-azure-manage-vms.md#open-a-recovery-services-vault-in-the-dashboard), klikněte na Procházet tooand **protokoly auditu** tooopen hello **události** okno.

    ![Protokoly auditu](./media/backup-azure-monitor-vms/audit-logs-1606-1.png)

    Hello **události** toohello provozní události filtrované právě pro hello aktuální trezor otevře se okno.

    ![Filtr protokolů auditu](./media/backup-azure-monitor-vms/audit-logs-filter.png)

    okno Hello zobrazuje seznam hello kritický, chyby, upozornění a informační události, které došlo k chybě v hello minulého týdne. Hello časové období je výchozí hodnotu nastavenou v hello **filtru**. Hello **události** okno také ukazuje pruhový graf sledování při hello události došlo k chybě. Pokud nechcete, aby toosee hello pruhový graf, v hello **události** nabídky, klikněte na tlačítko **skrýt graf** tootoggle vypnout hello grafu. Hello výchozí zobrazení událostí zobrazuje informace o operaci, úroveň, stav, prostředků a času. Informace o vystavení další atributy, které události, najdete v tématu hello [rozšíření informací o události](backup-azure-monitor-vms.md#view-additional-event-attributes).
2. Další informace o provozních událostí, v hello **operace** sloupce, klikněte na tlačítko provozních událostí tooopen její okno. Hello okno obsahuje podrobné informace o událostech hello. Události jsou seskupené podle jejich ID korelace a seznam hello událostí, které nastaly v hello časové rozpětí.

    ![Podrobnosti o operaci](./media/backup-azure-monitor-vms/audit-logs-details-window.png)
3. tooview podrobné informace o konkrétní události z hello seznam událostí, klikněte na tlačítko hello událostí tooopen jeho **podrobnosti** okno.

    ![Podrobnosti události](./media/backup-azure-monitor-vms/audit-logs-details-window-deep.png)

    Hello úroveň události informace jsou také podrobné informace získá hello. Pokud raději prohlížet tomto velkého množství informací o jednotlivých událostí a chtěli tooadd to mnohem podrobností toohello **události** okně najdete v tématu hello [rozšíření informací o události](backup-azure-monitor-vms.md#view-additional-event-attributes).

## <a name="customize-hello-event-filter"></a>Přizpůsobení filtr událostí hello
Použití hello **filtru** tooadjust nebo zvolte hello informace, které se zobrazí v konkrétní okno. informace o toofilter hello události:

1. V hello [panelu trezoru](backup-azure-manage-vms.md#open-a-recovery-services-vault-in-the-dashboard), klikněte na Procházet tooand **protokoly auditu** tooopen hello **události** okno.

    ![Protokoly auditu](./media/backup-azure-monitor-vms/audit-logs-1606-1.png)

    Hello **události** toohello provozní události filtrované právě pro hello aktuální trezor otevře se okno.

    ![Filtr protokolů auditu](./media/backup-azure-monitor-vms/audit-logs-filter.png)
2. Na hello **události** nabídky, klikněte na tlačítko **filtru** tooopen tohoto okna.

    ![Otevřete okno filtru](./media/backup-azure-monitor-vms/audit-logs-filter-button.png)
3. Na hello **filtru** okně Upravit hello **úroveň**, **časové rozpětí**, a **volající** filtry. Hello ostatní filtry nejsou k dispozici vzhledem k tomu, že byly nastavené tooprovide hello aktuální informace pro trezor služeb zotavení hello.

    ![Podrobnosti o dotazu protokoly auditu](./media/backup-azure-monitor-vms/filter-blade.png)

    Můžete zadat hello **úroveň** události: kritická, chyba, varování nebo informační. Můžete zvolit libovolnou kombinaci úrovní událostí, ale musí mít nejméně jedna vybraná úroveň. Hello úroveň zapnout nebo vypnout. Hello **časové rozpětí** filtru lze toospecify hello časový interval pro zachycení událostí. Pokud používáte vlastní časové období, můžete nastavit hello spuštění a ukončení.
4. Až se připravené tooquery hello operations protokolů pomocí filtru, klikněte na tlačítko **aktualizace**. Zobrazit výsledky Hello v hello **události** okno.

    ![Podrobnosti o operaci](./media/backup-azure-monitor-vms/edited-list-of-events.png)

### <a name="view-additional-event-attributes"></a>Zobrazení událostí další atributy
Pomocí hello **sloupce** tlačítko, můžete povolit další událost tooappear atributy v seznamu hello na hello **události** okno. Hello výchozí seznam událostí zobrazí informace o operaci, úroveň, stav, prostředků a času. Další atributy tooenable:

1. Na hello **události** okně klikněte na tlačítko **sloupce**.

    ![Otevřete sloupců](./media/backup-azure-monitor-vms/audi-logs-column-button.png)

    Hello **zvolit sloupce** otevře se okno.

    ![Okno sloupce](./media/backup-azure-monitor-vms/columns-blade.png)
2. tooselect hello atribut, klikněte na zaškrtávací políčko hello. zaškrtávací políčko atribut Hello přepne zapnout a vypnout.
3. Klikněte na tlačítko **resetovat** tooreset hello seznamem atributů v hello **události** okno. Po přidání nebo odebrání atributů hello seznamu, použijte **resetovat** tooview hello nový seznam událostí atributy.
4. Klikněte na tlačítko **aktualizace** tooupdate hello data v atributech událostí hello. Hello následující tabulka obsahuje informace o každý atribut.

| Název sloupce | Popis |
| --- | --- |
| Operace |Název Hello operace hello |
| Úroveň |Hello úroveň hello operace hodnoty mohou být: informační, upozornění, chyby nebo kritický |
| Status |Popisný stavu operace hello |
| Prostředek |Adresa URL, který identifikuje prostředek hello; také označované jako ID prostředku hello |
| Čas |Čas, měřená od hello aktuální čas, kdy došlo k události hello |
| Volající |Kdo nebo co názvem nebo aktivuje událost hello. může být hello systému nebo uživatele |
| časové razítko |Hello čas, kdy hello událost byla spuštěna |
| Skupina prostředků |Hello související skupiny prostředků |
| Typ prostředku |Hello interní typ prostředku používaný správcem prostředků |
| ID předplatného |Hello přidružené ID předplatného |
| Kategorie |Kategorie události hello |
| ID korelace |ID společné souvisejících událostí |

## <a name="use-powershell-toocustomize-alerts"></a>Pomocí prostředí PowerShell toocustomize výstrahy
Vlastní oznámení výstrah pro úlohy hello můžete získat na portálu hello. Tyto úlohy, tooget definovat pravidla na hello provozní protokoluje události výstrahy založené na prostředí PowerShell. Použití *prostředí PowerShell, verze 1.3.0 nebo novější*.

toodefine tooalert vlastní oznámení selhání zálohování, použijte příkaz jako hello následující skript:

```
PS C:\> $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail contoso@microsoft.com
PS C:\> Add-AzureRmLogAlertRule -Name backupFailedAlert -Location "East US" -ResourceGroup RecoveryServices-DP2RCXUGWS3MLJF4LKPI3A3OMJ2DI4SRJK6HIJH22HFIHZVVELRQ-East-US -OperationName Microsoft.RecoveryServices/recoveryServicesVault/Backup -Status Failed -TargetResourceId /subscriptions/86eeac34-eth9a-4de3-84db-7a27d121967e/resourceGroups/RecoveryServices-DP2RCXUGWS3MLJF4LKPI3A3OMJ2DI4SRJK6HIJH22HFIHZVVELRQ-East-US/providers/Microsoft.RecoveryServices/vaults/trinadhVault -Actions $actionEmail
```

**ResourceId** : můžete získat ResourceId hello protokoly auditu. Hello ResourceId je adresa URL zadané ve sloupci prostředků hello hello operaci protokolů.

**OperationName** : OperationName je ve formátu hello "Microsoft.RecoveryServices/recoveryServicesVault/*EventName*" kde *EventName* může být:<br/>

* Registrace <br/>
* Zrušit registraci <br/>
* ConfigureProtection <br/>
* Zálohování <br/>
* Obnovení <br/>
* StopProtection <br/>
* DeleteBackupData <br/>
* CreateProtectionPolicy <br/>
* DeleteProtectionPolicy <br/>
* UpdateProtectionPolicy <br/>

**Stav** : podporované hodnoty jsou Začínáme, bylo úspěšné nebo neúspěšné.

**ResourceGroup** : Toto je hello skupinu prostředků, prostředků hello toowhich patří. Můžete přidat hello skupiny prostředků sloupec toohello generuje protokoly. Skupina prostředků je jeden z dostupných typů hello informací o události.

**Název** : název hello pravidlo výstrahy.

**CustomEmail** : Zadejte hello vlastní e-mailové adresy toowhich chcete toosend oznámení výstrahy

**SendToServiceOwners** : tuto možnost odesílá oznámení výstrah tooall správci a spolusprávci předplatného hello. Je možné v **New-AzureRmAlertRuleEmail** rutiny

### <a name="limitations-on-alerts"></a>Omezení výstrahy
Na základě událostí výstrahy jsou subjektu toohello následující omezení:

1. Výstrahy se spouštějí na všechny virtuální počítače v trezoru služeb zotavení hello. Nelze přizpůsobit hello výstrahy pro podmnožinu virtuálních počítačů v trezoru služeb zotavení.
2. Tato funkce je ve verzi Preview. [Další informace](../monitoring-and-diagnostics/insights-powershell-samples.md#create-metric-alerts)
3. Výstrahy jsou odesílány z "alerts-noreply@mail.windowsazure.com". Momentálně nelze upravit hello odesílatelem e-mailu.

## <a name="next-steps"></a>Další kroky
Protokoly událostí povolit skvělé postmortální a auditování podpory u operací zálohování hello. jsou zaznamenány Hello následující operace:

* Registrace
* Zrušit registraci
* Konfigurace ochrany
* Zálohování (obě plánované i zálohování na vyžádání)
* Obnovení
* Zastavení ochrany
* Odstranit záložní data
* Přidání zásad
* Odstranit zásadu
* Aktualizovat zásady
* Zrušení úlohy

Široká vysvětlení události, operace a protokoly auditu napříč hello služby Azure, najdete v článku hello, [zobrazení událostí a protokolů auditování](../monitoring-and-diagnostics/insights-debugging-with-events.md).

Informace o opětovné vytvoření virtuálního počítače z bodu obnovení, podívejte se na [obnovení virtuálních počítačů Azure](backup-azure-restore-vms.md). Pokud potřebujete informace o ochraně virtuálních počítačů, přečtěte si [první pohled: zálohování virtuálních počítačů tooa trezor služeb zotavení](backup-azure-vms-first-look-arm.md). Další informace o hello úlohy správy pro zálohování virtuálních počítačů v článku hello [záloh virtuálních počítačů Azure spravovat](backup-azure-manage-vms.md).
