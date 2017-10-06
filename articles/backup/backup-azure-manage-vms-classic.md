---
title: "zálohování virtuálního počítače Azure aaaManage a monitorování | Microsoft Docs"
description: "Zjistěte, jak toomanage a monitorování Azure virtuální počítač zálohy"
services: backup
documentationcenter: 
author: trinadhk
manager: shreeshd
editor: 
ms.assetid: 4372944e-d33a-4f6a-bd5f-d04af2842713
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: trinadhk;markgal;
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: cb95e0b3760c96f7fd563c42cb4c635553f48957
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-common-azure-backup-jobs-and-trigger-alerts-in-hello-classic-portal"></a>Spravovat běžné úlohy zálohování Azure a aktivační události upozornění v portálu classic hello
> [!div class="op_single_selector"]
> * [Spravovat zálohy virtuálního počítače Azure](backup-azure-manage-vms.md)
> * [Spravovat zálohy Classic virtuálních počítačů](backup-azure-manage-vms-classic.md)
>
>

Tento článek obsahuje informace o běžné úlohy správy a sledování pro Classic model virtuální počítače chráněné v Azure.  

> [!NOTE]
> Azure obsahuje dva modely nasazení pro vytváření a práci s prostředky: [Resource Manager a Classic](../azure-resource-manager/resource-manager-deployment-model.md). V tématu [připravit vaše prostředí tooback zálohu virtuálních počítačích Azure](backup-azure-vms-prepare.md) podrobnosti o práci s klasického nasazení modelu virtuálních počítačů.
>
> [!IMPORTANT]
>Od března 2017 se už můžete trezory Backup portálu classic toocreate hello.
>
> Teď můžete upgradovat vaše trezory služeb tooRecovery trezory Backup. Podrobnosti najdete v tématu hello článku [upgradu tooa trezoru zálohování trezor služeb zotavení](backup-azure-upgrade-backup-to-recovery-services.md). Společnost Microsoft doporučuje tooupgrade zálohování trezory tooRecovery trezory služeb.<br/> Po 15 říjen 2017 nemůžete použít trezory Backup toocreate prostředí PowerShell. **Do 1. listopadu 2017:**
>- Všechny zbývající trezory Backup bude automaticky upgradovaný tooRecovery trezory služeb.
>- Můžete nebudou moct tooaccess zálohovaných dat na portálu classic hello. Místo toho použijte hello Azure portálu tooaccess zálohovaných dat v trezory služeb zotavení.

## <a name="manage-protected-virtual-machines"></a>Správa chráněných virtuálních počítačů
toomanage chráněné virtuální počítače:

1. tooview a spravovat nastavení zálohování pro virtuální počítač, klikněte na tlačítko hello **chráněné položky** kartě.
2. Klikněte na název hello chráněné položky toosee hello **zálohování podrobnosti** kartu, která vám zobrazí informace o hello poslední zálohy.

    ![Záloha virtuálního počítače](./media/backup-azure-manage-vms/backup-vmdetails.png)
3. tooview a spravovat nastavení zásad zálohování pro virtuální počítač, klikněte na tlačítko hello **zásady** kartě.

    ![Virtuální počítač zásad](./media/backup-azure-manage-vms/manage-policy-settings.png)

    Hello **zásady zálohování** kartě ukazuje hello existující zásady. Můžete upravit podle potřeby. Pokud potřebujete toocreate nových zásad klikněte na tlačítko **vytvořit** na hello **zásady** stránky. Všimněte si, že pokud chcete, aby tooremove zásady by neměl mít všechny virtuální počítače s ním spojená.

    ![Virtuální počítač zásad](./media/backup-azure-manage-vms/backup-vmpolicy.png)
4. Můžete získat další informace o stavu nebo akce pro virtuální počítač na hello **úlohy** stránky. Další podrobnosti nebo filtr úlohy pro konkrétní virtuální počítač, klikněte na úlohu v seznamu tooget hello.

    ![Úlohy](./media/backup-azure-manage-vms/backup-job.png)

## <a name="on-demand-backup-of-a-virtual-machine"></a>Zálohování na vyžádání virtuálního počítače
Na vyžádání můžete provést zálohování virtuálního počítače, jakmile je nakonfigurován pro ochranu. Pokud hello prvotní zálohování čeká na vyřízení pro hello virtuálního počítače, bude zálohování na vyžádání vytvořit úplnou kopii hello virtuálního počítače v záložním trezoru Azure. Pokud se první zálohování je dokončeno, bude zálohování na vyžádání jen odeslání změn z předchozí zálohy zálohování tooAzure trezoru tj ho je vždy přírůstkové.

> [!NOTE]
> Rozsah uchování zálohu na vyžádání je nastavena hodnota tooretention zadaná pro denní doba uchování ve toohello odpovídající zásady zálohování virtuálních počítačů.  
>
>

tootake na vyžádání zálohování virtuálního počítače:

1. Přejděte toohello **chráněné položky** a vyberte **virtuální počítač Azure** jako **typ** (Pokud již není vybrána) a klikněte na **vyberte**tlačítko.

    ![Typ virtuálního počítače](./media/backup-azure-manage-vms/vm-type.png)
2. Vyberte hello virtuální počítač, na kterém chcete zálohování tootake na vyžádání a klikněte na **zálohování** tlačítko v hello dolní části stránky hello.

    ![Zálohovat nyní](./media/backup-azure-manage-vms/backup-now.png)

    Tím se vytvoří úlohu zálohování na hello vybraný virtuální počítač. Rozsah uchování bodu obnovení, které jsou vytvořené pomocí této úlohy bude stejné jako příkaz, který je uveden v zásadách hello přidruženého hello virtuálnímu počítači.

    ![Vytvoření úlohy zálohování](./media/backup-azure-manage-vms/creating-job.png)

   > [!NOTE]
   > zásady hello tooview spojené s virtuálním počítačem, procházení dolů do virtuálního počítače v hello **chráněné položky** stránku a přejděte toobackup kartu zásad.
   >
   >
3. Jakmile bude vytvořena úloha hello, můžete kliknutím na **zobrazit úlohy** tlačítka na hello informační panel toosee hello odpovídající úlohy na stránce úloh hello.

    ![Vytvořit úlohu zálohování](./media/backup-azure-manage-vms/created-job.png)
4. Po úspěšném dokončení úlohy hello bodu obnovení se vytvoří který můžete použít toorestore hello virtuálního počítače. Hodnota sloupce bodu obnovení hello to také se zvýší o 1 v **chráněné položky** stránky.

## <a name="stop-protecting-virtual-machines"></a>Zastavte ochranu virtuálních počítačů
Můžete toostop hello budoucí zálohy virtuálního počítače s hello následující možnosti:

* Zachovat zálohovaná data související s virtuálním počítačem v trezoru zálohování Azure
* Odstranit záložní data související s virtuálním počítačem

Pokud jste vybrali tooretain zálohovaná data související s virtuálním počítačem, můžete použít hello zálohovaná data toorestore hello virtuální počítač. O cenách podrobnosti takových virtuálních počítačů, klikněte na tlačítko [zde](https://azure.microsoft.com/pricing/details/backup/).

tooStop ochranu pro virtuální počítač:

1. Přejděte příliš**chráněné položky** a vyberte **virtuální počítač Azure** jako typ filtru hello (Pokud již není vybrána) a klikněte na **vyberte** tlačítko.

    ![Typ virtuálního počítače](./media/backup-azure-manage-vms/vm-type.png)
2. Vyberte hello virtuální počítač a klikněte na **zastavit ochranu** v hello dolní části stránky hello.

    ![Zastavení ochrany](./media/backup-azure-manage-vms/stop-protection.png)
3. Ve výchozím nastavení zálohování Azure nedojde k odstranění hello zálohovaná data související s virtuálním počítačem hello.

    ![Potvrďte zastavení ochrany](./media/backup-azure-manage-vms/confirm-stop-protection.png)

    Pokud chcete toodelete zálohování dat, zaškrtněte políčko hello.

    ![Zaškrtávací políčko](./media/backup-azure-manage-vms/checkbox.png)

    Vyberte důvod zastavení zálohování hello. Zatímco tato položka je nepovinná, poskytuje příslušný důvod. bude pomoci toowork Azure Backup na zpětnou vazbu hello a určit jejich prioritu hello zákazníka scénáře.
4. Klikněte na **odeslání** tlačítko toosubmit hello **zastavit ochranu** úlohy. Klikněte na **zobrazit úlohy** toosee hello odpovídající úloha hello v **úlohy** stránky.

    ![Zastavení ochrany](./media/backup-azure-manage-vms/stop-protect-success.png)

    Pokud nevyberete **odstranit související zálohovaná data** možnost během **zastavit ochranu** ochrany průvodce a po dokončení úlohy post, stav se změní příliš**ochrana zastavena**. Hello data zůstávají s Azure Backup, dokud je explicitně odstranit. Hello data můžete odstranit vždy výběrem hello virtuálního počítače v hello **chráněné položky** stránky a kliknutím na **odstranit**.

    ![Ukončeno ochrany](./media/backup-azure-manage-vms/protection-stopped-status.png)

    Pokud jste vybrali hello **odstranit související zálohovaná data** možnost, hello virtuální počítač nebude součástí hello **chráněné položky** stránky.

## <a name="re-protect-virtual-machine"></a>Znovu nastavit ochranu virtuálního počítače
Pokud nevyberete hello **odstranění přidružení zálohovaná data** možnost **zastavit ochranu**, můžete znovu nastavit ochranu hello virtuálního počítače pomocí následujících hello kroky podobné toobacking si zaregistrovat virtuální počítače. Jakmile chráněný, tento virtuální počítač bude mít zálohovaná data zachována předchozí toostop ochrany a vytvoří body obnovení po se znovu nastavit ochranu.

Po znovu nastavit ochranu, stav ochrany hello virtuálního počítače se změní příliš**chráněné** Pokud jsou body obnovení předchozí příliš**zastavit ochranu**.

  ![Vytvořit ochranu virtuálního počítače](./media/backup-azure-manage-vms/reprotected-status.png)

> [!NOTE]
> Při ochranu znovu hello virtuálního počítače, můžete zvolit jinou zásadu než hello zásada, ke které byl virtuální počítač původně chráněný.
>
>

## <a name="unregister-virtual-machines"></a>Zrušit registraci virtuálních počítačů
Pokud chcete, aby tooremove hello virtuální počítač z trezoru záloh hello:

1. Klikněte na hello **UNREGISTER** tlačítko v hello dolní části stránky hello.

    ![Zakažte ochranu](./media/backup-azure-manage-vms/unregister-button.png)

    Informační zpráva se zobrazí v hello dolní části obrazovky hello žádají o potvrzení. Klikněte na tlačítko **Ano** toocontinue.

    ![Zakažte ochranu](./media/backup-azure-manage-vms/confirm-unregister.png)

## <a name="delete-backup-data"></a>Odstranění dat zálohy
Odstraněním hello zálohovaná data související s virtuálním počítačem, buď:

* Během úlohu zastavení ochrany
* Po zastavení ochrany dokončení úlohy na virtuálním počítači

toodelete zálohovaná data na virtuálním počítači, který je v hello *ukončena ochrana* stavu post úspěšné dokončení **zastavení zálohování** úlohy:

1. Přejděte toohello **chráněné položky** a vyberte **virtuální počítač Azure** jako *typ* a klikněte na tlačítko hello **vyberte** tlačítko.

    ![Typ virtuálního počítače](./media/backup-azure-manage-vms/vm-type.png)
2. Vyberte virtuální počítač hello. Hello virtuální počítač bude v **ukončena ochrana** stavu.

    ![Zastavení ochrany](./media/backup-azure-manage-vms/protection-stopped-b.png)
3. Klikněte na tlačítko hello **odstranit** tlačítko v hello dolní části stránky hello.

    ![Odstranit zálohování](./media/backup-azure-manage-vms/delete-backup.png)
4. V hello **odstranit záložní data** průvodce, vyberte důvod pro odstraňují se záložní data (důrazně doporučujeme) a klikněte na **odeslání**.

    ![Odstranit záložní data](./media/backup-azure-manage-vms/delete-backup-data.png)
5. Tím se vytvoří záložní data úlohy toodelete vybraného virtuálního počítače. Klikněte na tlačítko **zobrazit úlohy** toosee odpovídající úlohy na stránce úloh.

    ![Úspěšné odstranění dat](./media/backup-azure-manage-vms/delete-data-success.png)

    Po dokončení úlohy hello hello záznam odpovídající toohello virtuální počítač se odebere z **chráněné položky** stránky.

## <a name="dashboard"></a>Řídicí panel
Na hello **řídicí panel** stránce můžete zkontrolovat informace o virtuálních počítačích Azure, jejich úložiště a úlohy související s nimi v hello posledních 24 hodin. Můžete zobrazit stav zálohování a všechny související chyby zálohování.

![Řídicí panel](./media/backup-azure-manage-vms/dashboard-protectedvms.png)

> [!NOTE]
> Hodnoty v řídicím panelu hello se aktualizuje každých 24 hodin.
>
>

## <a name="auditing-operations"></a>Auditování operací
Azure backup poskytuje kontrolu hello "protokoly operací" operace zálohování aktivovány hello zákazníků, takže je easy toosee přesně jaké operace správy, které byly provedeny v trezoru záloh hello. Protokoly operací povolit skvělé postmortální a auditování podpory u operací zálohování hello.

protokoly operací přihlášeni Hello následující operace:

* Registrace
* Zrušit registraci
* Konfigurace ochrany
* Zálohování (obě plánované i zálohování na vyžádání prostřednictvím BackupNow)
* Obnovení
* Zastavení ochrany
* Odstranit záložní data
* Přidání zásad
* Odstranit zásadu
* Aktualizovat zásady
* Zrušení úlohy

operace tooview protokolů odpovídajícího úložiště záloh tooa:

1. Přejděte příliš**služby pro** na portálu Azure a potom klikněte na hello **protokoly operací** kartě.

    ![Protokoly operací](./media/backup-azure-manage-vms/ops-logs.png)
2. V hello filtry, vyberte **zálohování** jako *typ* a zadejte název úložiště záloh hello v *název služby* a klikněte na **odeslání**.

    ![Operace protokoly filtru](./media/backup-azure-manage-vms/ops-logs-filter.png)
3. V protokolech operací hello, vyberte všechny operace a klikněte na **podrobnosti** toosee podrobnosti odpovídající tooan operaci.

    ![Podrobnosti operace načítání protokolů](./media/backup-azure-manage-vms/ops-logs-details.png)

    Hello **podrobnosti průvodce** obsahuje informace o aktivaci, operace hello úloha s Id prostředku, na které se aktivuje tuto operaci a čas zahájení operace hello.

    ![Podrobnosti o operaci](./media/backup-azure-manage-vms/ops-logs-details-window.png)

## <a name="alert-notifications"></a>Oznámení o výstrahách
Vlastní oznámení výstrah pro úlohy hello můžete získat na portálu. Toho dosáhnete tak, že definujete pravidla výstrah pomocí prostředí PowerShell operační protokoly událostí. Doporučujeme používat *prostředí PowerShell, verze 1.3.0 nebo vyšší*.

toodefine tooalert vlastní oznámení selhání zálohování, ukázka příkazu bude vypadat jako:

```
PS C:\> $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail contoso@microsoft.com
PS C:\> Add-AzureRmLogAlertRule -Name backupFailedAlert -Location "East US" -ResourceGroup RecoveryServices-DP2RCXUGWS3MLJF4LKPI3A3OMJ2DI4SRJK6HIJH22HFIHZVVELRQ-East-US -OperationName Microsoft.Backup/backupVault/Backup -Status Failed -TargetResourceId /subscriptions/86eeac34-eth9a-4de3-84db-7a27d121967e/resourceGroups/RecoveryServices-DP2RCXUGWS3MLJF4LKPI3A3OMJ2DI4SRJK6HIJH22HFIHZVVELRQ-East-US/providers/microsoft.backupbvtd2/BackupVault/trinadhVault -Actions $actionEmail
```

**ResourceId**: vám to z protokolů Operations místní podle popisu ve výše části. ResourceUri v automaticky otevíraném okně Podrobnosti operace je toobe ResourceId hello zadaný pro tuto rutinu.

**OperationName**: to bude mít formát hello "Microsoft.Backup/backupvault/<EventName>" kde EventName je jedním z registru, Unregister, ConfigureProtection, zálohování, obnovení, StopProtection, DeleteBackupData, UpdateProtectionPolicy CreateProtectionPolicy, DeleteProtectionPolicy,

**Stav**: podporované hodnoty jsou – Začínáme, úspěšná a se nezdařilo.

**ResourceGroup**: ResourceGroup hello prostředku, na kterém se spustí operaci. To můžete získat od ResourceId hodnota. Hodnota mezi poli */resourceGroups/* a */providers/* v ResourceId hodnota je hodnota hello ResourceGroup.

**Název**: název hello pravidlo výstrahy.

**CustomEmail**: Zadejte hello vlastní e-mailové adresy toowhich chcete toosend oznámení výstrah

**SendToServiceOwners**: tuto možnost odesílá oznámení výstrah tooall správci a spolusprávci předplatného hello. Je možné v **New-AzureRmAlertRuleEmail** rutiny

### <a name="limitations-on-alerts"></a>Omezení výstrahy
Na základě událostí výstrahy jsou vystaveno toohello následující omezení:

1. Výstrahy se spouštějí na všechny virtuální počítače v trezoru záloh hello. Nelze přizpůsobit ho výstrah tooget pro konkrétní sadu virtuálních počítačů v trezoru záloh.
2. Tato funkce je ve verzi Preview. [Další informace](../monitoring-and-diagnostics/insights-powershell-samples.md#create-metric-alerts)
3. Zobrazí se výstrahy z "alerts-noreply@mail.windowsazure.com". Momentálně nelze upravit hello odesílatelem e-mailu.

## <a name="next-steps"></a>Další kroky
* [Obnovení virtuálních počítačů Azure](backup-azure-restore-vms.md)
