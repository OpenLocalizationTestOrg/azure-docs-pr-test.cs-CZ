---
title: "aaaUse stav virtuálního počítače Azure tooschedule formátu JSON značky | Microsoft Docs"
description: "Tento článek ukazuje, jak toouse JSON řetězce na značky tooautomate hello plánování virtuálních počítačů při spuštění a vypnutí."
services: automation
documentationcenter: 
author: MGoedtel
manager: jwhit
editor: tysonn
ms.assetid: 6afed5d2-e939-4749-8b2c-9312b4c16fb2
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: magoedte;paulomarquesc
ms.openlocfilehash: f6bbf1dea1c193e5d1010f12f3b1ed63562f9daf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-automation-scenario-using-json-formatted-tags-toocreate-a-schedule-for-azure-vm-startup-and-shutdown"></a>Azure scénář automatizace: použití formátu JSON značky toocreate plán pro virtuální počítač Azure spuštění a vypnutí
Zákazníci často mají tooschedule hello spuštění a vypnutí virtuálního počítače toohelp snížit náklady na předplatné nebo podporu obchodních a technických požadavků.

Hello následující scénáře vám umožní tooset automatické spuštění a vypnutí virtuální počítače pomocí značku plán na úrovni skupiny prostředků nebo úrovni virtuálního počítače v Azure. Tento plán lze konfigurovat v neděli tooSaturday času spuštění a čas ukončení.

Máme některé možnosti se na pole. Mezi ně patří:

* [Sady škálování virtuálního počítače](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) s nastavení automatického škálování, které umožňují tooscale příchozí nebo odchozí.
* [DevTest Labs](../devtest-lab/devtest-lab-overview.md) služby, která obsahuje integrovanou schopnost hello plánování operace spuštění a vypnutí.

Ale tyto možnosti podporují pouze konkrétní scénáře a nemůže být virtuální počítače použité tooinfrastructure jako služba (IaaS).

Při hello plán značky je použité tooa skupinu prostředků, je také použité tooall virtuální počítače v příslušné skupině prostředků. Plán se také přímo použité tooa virtuálních počítačů, poslední plán hello přednost v hello následující pořadí:

1. Skupina prostředků použité tooa plán
2. Naplánovat použité tooa skupinu prostředků a virtuální počítač ve skupině prostředků hello
3. Použít plán tooa virtuálního počítače

Tento scénář v podstatě přebírá řetězec formátu JSON s zadaný formát a přidá ji jako hello hodnotu pro značku plán. Potom runbook jsou uvedeny všechny skupiny prostředků a virtuální počítače a identifikuje hello plány pro každý virtuální počítač, na základě výše uvedených scénářů hello. V dalším projde hello virtuálních počítačů, které mají plány připojen a vyhodnotí, jakou akci by měla být provedena. Například určuje který virtuálních počítačů potřebovat toobe zastavit, vypnout nebo ignorovat.

Tyto sady runbook ověřit pomocí hello [účet spustit v Azure jako](automation-sec-configure-azure-runas-account.md).

## <a name="download-hello-runbooks-for-hello-scenario"></a>Stáhnout runbooky hello hello scénář
Tento scénář se skládá ze čtyř runbooky pracovních postupů Powershellu, které si můžete stáhnout z hello [Galerii TechNet](https://gallery.technet.microsoft.com/Azure-Automation-Runbooks-84f0efc7) nebo hello [Githubu](https://github.com/paulomarquesdacosta/azure-automation-scheduled-shutdown-and-startup) úložiště pro tento projekt.

| Runbook | Popis |
| --- | --- |
| Test ResourceSchedule |Zkontroluje každý plán virtuální počítač a provede vypínání nebo spouštění podle plánu hello. |
| Přidat ResourceSchedule |Přidá hello plán značky tooa virtuálního počítače nebo skupiny prostředků. |
| Aktualizace ResourceSchedule |Upravuje existující plán značky hello tak, že nahradíte novým připojením. |
| Odebrat ResourceSchedule |Odebere z virtuálního počítače nebo prostředku skupiny hello plán značky. |

## <a name="install-and-configure-this-scenario"></a>Instalace a konfigurace tohoto scénáře
### <a name="install-and-publish-hello-runbooks"></a>Instalace a publikování sad runbook hello
Po stažení hello sady runbook, můžete je importovat pomocí postupu hello v [vytvoření nebo import runbooku ve službě Azure Automation](automation-creating-importing-runbook.md#importing-a-runbook-from-a-file-into-azure-automation).  Každá sada runbook publikujte po byl úspěšně importován do vašeho účtu Automation.

### <a name="add-a-schedule-toohello-test-resourceschedule-runbook"></a>Přidat runbook toohello ResourceSchedule testovací plán
Postupujte podle těchto kroků tooenable hello plán pro testovací ResourceSchedule runbook hello. Je to hello runbooku, který ověřuje, které virtuální počítače by měla být spuštěna, vypnout, nebo vlevo, jako je.

1. Z hello portálu Azure, otevřete svůj účet Automation a pak klikněte na tlačítko hello **Runbooky** dlaždici.
2. Na hello **Test ResourceSchedule** okně klikněte na tlačítko hello **plány** dlaždici.
3. Na hello **plány** okně klikněte na tlačítko **přidat plán**.
4. Na hello **plány** vyberte **propojit plán tooyour runbook**. Potom vyberte **vytvořte nový plán**.
5. Na hello **nový plán** okno, zadejte název hello tento plán, například: *HourlyExecution*.
6. Pro plán hello **spustit**, nastavte hello počáteční čas tooan hodinu přírůstku.
7. Vyberte **opakování**a pak pro **opakovat každých interval**, vyberte **1 hodina**.
8. Ověřte, že **nastavit dobu platnosti** je nastaven příliš**ne**a potom klikněte na **vytvořit** toosave nový plán.
9. Na hello **plán Runbook** okno Možnosti, vyberte **nastavení parametrů a běhu**. V hello Test ResourceSchedule **parametry** okno, zadejte název hello předplatného v hello **Název_předplatného** pole.  Toto je pouze parametr hello, které je nutné pro sadu runbook hello.  Až budete hotovi, klikněte na tlačítko **OK**.

plán sad runbook Hello by měl vypadat jako následující hello, až se dokončí:

![Nakonfigurované ResourceSchedule testu runbooku](./media/automation-scenario-start-stop-vm-wjson-tags/automation-schedule-config.png)<br>

## <a name="format-hello-json-string"></a>Formát hello řetězce formátu JSON
Toto řešení v podstatě trvá řetězce s zadaný formát JSON a přidá ji jako hello hodnotu pro značku volá plán. Potom runbook jsou uvedeny všechny skupiny prostředků a virtuální počítače a identifikuje hello plány pro každý virtuální počítač.

Hello runbook cyklicky prochází přes hello virtuálních počítačů, které mají plány připojen a zkontroluje, jaké akce by měla být provedena. Hello následuje příklad formátování hello řešení:

```json
{
    "TzId": "Eastern Standard Time",
    "0": {
        "S": "11",
        "E": "17"
    },
    "1": {
        "S": "9",
        "E": "19"
    },
    "2": {
        "S": "9",
        "E": "19"
    },
}
```

Zde jsou některé podrobné informace o tuto strukturu:

1. Formát Hello této struktury JSON je optimalizované toowork obejít omezení 256 znaků hello jedinou značku hodnoty v Azure.
2. *TzId* představuje hello časové pásmo hello virtuálního počítače. Toto ID můžete získat pomocí třída TimeZoneInfo .NET hello v relaci prostředí PowerShell –**[System.TimeZoneInfo]:: GetSystemTimeZones()**.

   ![GetSystemTimeZones v prostředí PowerShell](./media/automation-scenario-start-stop-vm-wjson-tags/automation-get-timzone-powershell.png)

   * Dny v týdnu jsou reprezentované pomocí číselná hodnota nula toosix. Hello hodnota nula rovná neděli.
   * Hello čas spuštění je reprezentována pomocí hello **S** atribut a jeho hodnota je ve 24hodinovém formátu.
   * Hello čas end nebo ukončení je reprezentována pomocí hello **E** atribut a jeho hodnota je ve 24hodinovém formátu.

     Pokud hello **S** a **E** atributy každý mít hodnotu nula (0), hello virtuální počítač zůstane za současného stavu v době vyhodnocení hello.
3. Pokud chcete zkušební tooskip pro určitý den v týdnu hello, není pro daný den v týdnu hello přidat oddíl. Hello následující ukázka, pouze pondělí je vyhodnocena a hello jiných dny v týdnu hello se ignorují:

    ```json
    {
        "TzId": "Eastern Standard Time",
        "1": {
            "S": "11",
            "E": "17"
        }
    }
    ```

## <a name="tag-resource-groups-or-vms"></a>Skupiny prostředků značka nebo virtuální počítače
tooshut dolů virtuálních počítačů, je nutné tootag hello virtuální počítače nebo skupiny prostředků hello, ve kterých se nachází. Virtuální počítače, které nemají značku plán nevyhodnocují. Proto není spuštěna nebo vypnout.

Existují dva způsoby skupiny prostředků tootag nebo virtuální počítače s tímto řešením. Můžete provést přímo z portálu hello. Nebo můžete použít hello přidat ResourceSchedule, aktualizace ResourceSchedule a odebrat ResourceSchedule sady runbook.

### <a name="tag-through-hello-portal"></a>Značka prostřednictvím portálu hello
Postupujte podle těchto kroků tootag virtuální počítač nebo skupinu prostředků hello portálu:

1. Vyrovnání hello řetězce formátu JSON a ověřte, že nejsou žádné mezery.  Řetězec vašeho JSON by měl vypadat takto:

    ```json
    {"TzId":"Eastern Standard Time","0":{"S":"11","E":"17"},"1":{"S":"9","E":"19"},"2": {"S":"9","E":"19"},"3":{"S":"9","E":"19"},"4":{"S":"9","E":"19"},"5":{"S":"9","E":"19"},"6":{"S":"11","E":"17"}}
    ```

2. Vyberte hello **značky** ikonu pro virtuální počítač nebo prostředek skupiny tooapply tento plán.

   ![Možnosti značky virtuálních počítačů](./media/automation-scenario-start-stop-vm-wjson-tags/automation-vm-tag-option.png)

3. Značky jsou definovány následující dvojici klíč/hodnota. Typ **plán** v hello **klíč** pole a vložte hello řetězce formátu JSON do hello **hodnotu** pole. Klikněte na **Uložit**. Novou značku by se měla zobrazit v seznamu hello značky prostředku.

   ![Virtuální počítač plán značky](./media/automation-scenario-start-stop-vm-wjson-tags/automation-vm-schedule-tag.png)

### <a name="tag-from-powershell"></a>Značka z prostředí PowerShell
Všechny importované sady runbook obsahují informace nápovědy od začátku hello hello skriptu, který popisuje, jak tooexecute hello sady runbook přímo z prostředí PowerShell. Hello přidat ScheduleResource a ScheduleResource aktualizace sad runbook můžete volat z prostředí PowerShell. To provedete pomocí předání požadované parametry, které umožňují toocreate nebo aktualizace hello plán značky pro skupinu virtuálních počítačů nebo prostředek mimo portál hello.

toocreate, přidání a odstranění značky pomocí prostředí PowerShell, musíte nejprve příliš[nastavení prostředí PowerShell pro Azure](/powershell/azure/overview). Po dokončení nastavení hello, můžete pokračovat hello následující kroky.

### <a name="create-a-schedule-tag-with-powershell"></a>Vytvoření plánu značku pomocí prostředí PowerShell
1. Otevřete relaci prostředí PowerShell. Pak použijte následující příklad tooauthenticate se účet Spustit jako a toospecify předplatné hello:

    ```powershell
    $Conn = Get-AutomationConnection -Name AzureRunAsConnection
    Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID `
    -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

2. Definujte plán zatřiďovací tabulku. Tady je příklad, jak by měla zkonstruovat:

    ```powershell
    $schedule= @{ "TzId"="Eastern Standard Time"; "0"= @{"S"="11";"E"="17"};"1"= @{"S"="9";"E"="19"};"2"= @{"S"="9";"E"="19"};"3"= @{"S"="9";"E"="19"};"4"= @{"S"="9";"E"="19"};"5"= @{"S"="9";"E"="19"};"6"= @{"S"="11";"E"="17"}}
    ```

3. Definujte hello parametry, které jsou vyžadované hello runbook. V následujícím příkladu hello jsme cílení virtuálního počítače:

    ```powershell
    $params = @{"SubscriptionName"="MySubscription";"ResourceGroupName"="ResourceGroup01"; "VmName"="VM01";"Schedule"=$schedule}
    ```

    Pokud jste označování skupinu prostředků, odeberte hello *VMName* parametr z hello $params hash tabulky následujícím způsobem:

    ```powershell
    $params = @{"SubscriptionName"="MySubscription";"ResourceGroupName"="ResourceGroup01"; "Schedule"=$schedule}
    ```

4. Spusťte runbook hello přidat ResourceSchedule s hello následující parametry toocreate hello plán značky:

    ```powershell
    Start-AzureRmAutomationRunbook -Name "Add-ResourceSchedule" -Parameters $params `
    -AutomationAccountName "AutomationAccount" -ResourceGroupName "ResourceGroup01"
    ```

5. tooupdate značka virtuální počítač nebo skupinu prostředků provést hello **aktualizace ResourceSchedule** runbook s hello následující parametry:

    ```powershell
    Start-AzureRmAutomationRunbook -Name "Update-ResourceSchedule" -Parameters $params `
    -AutomationAccountName "AutomationAccount" -ResourceGroupName "ResourceGroup01"
    ```

### <a name="remove-a-schedule-tag-with-powershell"></a>Odebrání značky plánu pomocí prostředí PowerShell
1. Otevřete relaci prostředí PowerShell a spusťte následující tooauthenticate se účet Spustit jako a tooselect hello a zadejte předplatné:

    ```powershell
    Conn = Get-AutomationConnection -Name AzureRunAsConnection
    Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID `
    -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

2. Definujte hello parametry, které jsou vyžadované hello runbook. V následujícím příkladu hello jsme cílení virtuálního počítače:

    ```powershell
    $params = @{"SubscriptionName"="MySubscription";"ResourceGroupName"="ResourceGroup01";"VmName"="VM01"}
    ```

    Pokud odstraňujete značku ze skupiny prostředků, odeberte hello *VMName* parametr z hello $params hash tabulky následujícím způsobem:

    ```powershell
    $params = @{"SubscriptionName"="MySubscription";"ResourceGroupName"="ResourceGroup01"}
    ```

3. Spusťte hello odebrat ResourceSchedule runbook tooremove hello plán značky:

    ```powershell
    Start-AzureRmAutomationRunbook -Name "Remove-ResourceSchedule" -Parameters $params `
    -AutomationAccountName "AutomationAccount" -ResourceGroupName "ResourceGroup01"
    ```

4. tooupdate značka virtuální počítač nebo skupinu prostředků vykonat hello odebrat ResourceSchedule runbook s hello následující parametry:

    ```powershell
    Start-AzureRmAutomationRunbook -Name "Remove-ResourceSchedule" -Parameters $params `
    -AutomationAccountName "AutomationAccount" -ResourceGroupName "ResourceGroup01"
    ```

> [!NOTE]
> Doporučujeme proaktivnímu monitorování tyto sady runbook (a stavech virtuálních počítačů hello) vypnout tooverify, který bude vaše virtuální počítače a spustit odpovídajícím způsobem.
>

portál Azure, vyberte hello tooview hello podrobnosti sady runbook hello ResourceSchedule testovací úlohy hello **úlohy** dlaždice hello sady runbook. Hello úlohy Souhrn zobrazí hello vstupní parametry a výstup hello stream, kromě toogeneral informace o úloze hello a všechny výjimky, pokud k nim došlo.

Hello **Souhrn úlohy** zahrnuje zprávy z datových proudů hello výstup, upozornění a chyby. Vyberte hello **výstup** dlaždici tooview podrobné výsledky z hello spuštění sady runbook.

![Výstup testu ResourceSchedule](./media/automation-scenario-start-stop-vm-wjson-tags/automation-job-output.png)

## <a name="next-steps"></a>Další kroky
* tooget kroky s runbooky pracovních postupů Powershellu, najdete v části [Můj první runbook pracovního postupu Powershellu](automation-first-runbook-textual.md).
* toolearn Další informace o typech runbooků a jejich výhody a omezení, najdete v části [typy runbooků ve službě Azure Automation](automation-runbook-types.md).
* Další informace o funkcích podpory skript prostředí PowerShell najdete v tématu [nativní podpora Powershellových skriptů ve službě Azure Automation](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/).
* toolearn Další informace o protokolování sad runbook a výstup, najdete v části [Runbook výstupu a zpráv ve službě Azure Automation](automation-runbook-output-and-messages.md).
* Další informace o účtu spustit v Azure jako a jak tooauthenticate runbooků pomocí, najdete v části toolearn [ověření runbooků pomocí účtu spustit v Azure jako](automation-sec-configure-azure-runas-account.md).
