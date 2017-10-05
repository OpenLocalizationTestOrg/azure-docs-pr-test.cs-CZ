---
title: "Ukázek Azure PowerShell monitorování rychlý start. | Dokumentace Microsoftu"
description: "Pomocí prostředí PowerShell pro přístup k Azure monitorování funkce, jako je automatické škálování, výstrahy, webhooky a hledání protokoly aktivity."
author: kamathashwin
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: c0761814-7148-4ab5-8c27-a2c9fa4cfef5
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: ashwink
ms.openlocfilehash: 48f064884c2a6d0a55cc58a44169ed03c62de46d
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="azure-monitor-powershell-quick-start-samples"></a>Ukázek Azure PowerShell monitorování rychlý start
Tento článek ukazuje ukázkové příkazy prostředí PowerShell, abyste měli přístup k funkcím Azure monitorování. Azure monitorování umožňuje škálování cloudové služby, virtuální počítače a webových aplikací a odesílat oznámení o výstrahách nebo volání webové adresy URL založené na hodnotách nakonfigurované telemetrická data.

> [!NOTE]
> Azure monitorování je nový název pro co byla volána "Statistika Azure" až 25 září 2016. Však obory názvů a proto následující příkazy stále obsahovat "insights".
> 
> 

## <a name="set-up-powershell"></a>Nastavení prostředí PowerShell
Pokud jste to ještě neudělali, nastavte prostředí PowerShell pro spouštění v počítači. Další informace najdete v tématu [postup instalace a konfigurace prostředí PowerShell](/powershell/azure/overview).

## <a name="examples-in-this-article"></a>Příklady v tomto článku
V příkladech v článku znázorňují, jak můžete použít rutiny Azure monitorování. Můžete také zkontrolovat celý seznam rutin Azure Powershellu monitorování v [rutiny Azure monitorování (přehled)](https://msdn.microsoft.com/library/azure/mt282452#40v=azure.200#41.aspx).

## <a name="sign-in-and-use-subscriptions"></a>Přihlásit a používat odběrů
Nejdřív přihlaste k vašemu předplatnému Azure.

```PowerShell
Login-AzureRmAccount
```

To vyžaduje, abyste se přihlásili. Až to uděláte, váš účet, zobrazí se TenantID a výchozí ID předplatného. Všechny rutiny Azure fungovat v rámci vašeho předplatného výchozí. Chcete-li zobrazit seznam odběrů máte přístup, použijte následující příkaz.

```PowerShell
Get-AzureRmSubscription
```

Chcete-li změnit váš pracovní kontext do jiného předplatného, použijte následující příkaz.

```PowerShell
Set-AzureRmContext -SubscriptionId <subscriptionid>
```


## <a name="retrieve-activity-log-for-a-subscription"></a>Načtení protokol aktivit pro předplatné
Použití `Get-AzureRmLog` rutiny.  Následuje několik běžných příkladů.

Získání položky protokolu z tohoto času a data k dispozici:

```PowerShell
Get-AzureRmLog -StartTime 2016-03-01T10:30
```

Získáte položky protokolu mezi rozsahem času a data:

```PowerShell
Get-AzureRmLog -StartTime 2015-01-01T10:30 -EndTime 2015-01-01T11:30
```

Získání položky protokolu z určité skupiny zdrojů:

```PowerShell
Get-AzureRmLog -ResourceGroup 'myrg1'
```

Získáte položky protokolu od zprostředkovatele konkrétní prostředků mezi rozsahem času a data:

```PowerShell
Get-AzureRmLog -ResourceProvider 'Microsoft.Web' -StartTime 2015-01-01T10:30 -EndTime 2015-01-01T11:30
```

Získání všech položek protokolů s konkrétní volající:

```PowerShell
Get-AzureRmLog -Caller 'myname@company.com'
```

Tento příkaz načte posledních 1000 události z protokolu aktivit:

```PowerShell
Get-AzureRmLog -MaxEvents 1000
```

`Get-AzureRmLog`podporuje mnoho dalších parametrů. Najdete v článku `Get-AzureRmLog` pro další informace.

> [!NOTE]
> `Get-AzureRmLog`poskytuje pouze 15 dní historie. Pomocí **- MaxEvents** parametr umožňuje dotazování poslední události N, nad rámec 15 dnů. Pro přístup k události starší než 15 dní pomocí rozhraní REST API nebo SDK (C# ukázka pomocí sady SDK). Pokud neuvedete **StartTime**, pak výchozí hodnota je **EndTime** minus jednu hodinu. Pokud neuvedete **EndTime**, pak výchozí hodnota je aktuální čas. Všechny časy jsou ve formátu UTC.
> 
> 

## <a name="retrieve-alerts-history"></a>Načtení historie výstrah
Chcete-li zobrazit všechny výstrahy událostí, se můžete dotazovat protokolů Azure Resource Manager pomocí následující příklady.

```PowerShell
Get-AzureRmLog -Caller "Microsoft.Insights/alertRules" -DetailedOutput -StartTime 2015-03-01
```

Chcete-li zobrazit historii pro konkrétní pravidlo výstrahy, můžete použít `Get-AzureRmAlertHistory` rutiny předávání v ID prostředku pravidlo výstrahy.

```PowerShell
Get-AzureRmAlertHistory -ResourceId /subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/alertrules/myalert -StartTime 2016-03-1 -Status Activated
```

`Get-AzureRmAlertHistory` Rutina podporuje různé parametry. Další informace najdete v tématu [Get-AlertHistory](https://msdn.microsoft.com/library/mt282453.aspx).

## <a name="retrieve-information-on-alert-rules"></a>Načíst informace o pravidla výstrah
Všechny z následujících příkazů fungují na skupinu prostředků s názvem "montest".

Zobrazte všechny vlastnosti pravidlo výstrahy:

```PowerShell
Get-AzureRmAlertRule -Name simpletestCPU -ResourceGroup montest -DetailedOutput
```

Načtěte všechny výstrahy pro skupinu prostředků:

```PowerShell
Get-AzureRmAlertRule -ResourceGroup montest
```

Načtěte všechny výstrahy pravidla nastavená pro cílový prostředek. Všechna pravidla výstrah je třeba nastavit na virtuálním počítači.

```PowerShell
Get-AzureRmAlertRule -ResourceGroup montest -TargetResourceId /subscriptions/s1/resourceGroups/montest/providers/Microsoft.Compute/virtualMachines/testconfig
```

`Get-AzureRmAlertRule`podporuje další parametry. V tématu [Get-AlertRule](https://msdn.microsoft.com/library/mt282459.aspx) Další informace.

## <a name="create-metric-alerts"></a>Vytvoření metriky výstrahy
Můžete použít `Add-AlertRule` rutiny vytvářet, aktualizovat nebo zakázat pravidlo výstrahy.

Můžete vytvořit e-mailu a webhooku vlastností pomocí `New-AzureRmAlertRuleEmail` a `New-AzureRmAlertRuleWebhook`, v uvedeném pořadí. V rutině pravidlo výstrahy, přiřadit jako akce **akce** vlastnost pravidlo výstrahy.

Následující tabulka popisuje parametry a hodnoty použité k vytvoření výstrahy pomocí metriky.

| Parametr | hodnota |
| --- | --- |
| Name (Název) |simpletestdiskwrite |
| Umístění tohoto pravidla výstrah |Východ USA |
| ResourceGroup |montest |
| TargetResourceId |/subscriptions/S1/resourceGroups/montest/providers/Microsoft.COMPUTE/virtualMachines/testconfig |
| MetricName výstrahy, která je vytvořena |\PhysicalDisk (_celkem) \Disk zápisů za sekundu. Najdete v článku `Get-MetricDefinitions` rutiny o tom, jak získat přesné metriky názvy |
| Operátor |GreaterThan |
| Prahová hodnota (počet za sekundu za pro tato metrika) |1 |
| Velikost_okna (ve formátu hh: mm:) |00:05:00 |
| agregátoru (statistiky metriky, které používá průměrný počet, v takovém případě) |Průměr |
| vlastní e-mailů (řetězec pole) |'foo@example.com','bar@example.com' |
| poslat vlastníci, přispěvatelé a čtenáři e-mailu |-SendToServiceOwners |

Vytvoří akci, e-mailu

```PowerShell
$actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
```

Vytvoření Webhooku akce

```PowerShell
$actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://example.com?token=mytoken
```

Vytvořit pravidlo výstrahy na metrika % využití procesoru na virtuálním počítači classic

```PowerShell
Add-AzureRmMetricAlertRule -Name vmcpu_gt_1 -Location "East US" -ResourceGroup myrg1 -TargetResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.ClassicCompute/virtualMachines/my_vm1 -MetricName "Percentage CPU" -Operator GreaterThan -Threshold 1 -WindowSize 00:05:00 -TimeAggregationOperator Average -Actions $actionEmail, $actionWebhook -Description "alert on CPU > 1%"
```

Načtení pravidlo výstrahy

```PowerShell
Get-AzureRmAlertRule -Name vmcpu_gt_1 -ResourceGroup myrg1 -DetailedOutput
```

Výstrahy rutinu přidat také aktualizuje pravidlo, pokud se pravidlo výstrahy již existuje pro dané vlastnosti. Chcete-li zakázat pravidlo výstrahy, zahrňte parametr **- DisableRule**.

## <a name="get-a-list-of-available-metrics-for-alerts"></a>Získat seznam dostupné metriky pro výstrahy
Můžete použít `Get-AzureRmMetricDefinition` rutiny zobrazíte seznam všechny metriky pro konkrétní zdroje.

```PowerShell
Get-AzureRmMetricDefinition -ResourceId <resource_id>
```

Následující příklad vytvoří tabulku s metrika název a jednotky pro ni.

```PowerShell
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

Úplný seznam dostupných možností pro `Get-AzureRmMetricDefinition` je k dispozici na [Get-MetricDefinitions](https://msdn.microsoft.com/library/mt282458.aspx).

## <a name="create-and-manage-autoscale-settings"></a>Vytvořit a spravovat nastavení automatického škálování
Prostředek, například webovou aplikaci, virtuální počítač, Cloudová služba nebo sadu škálování virtuálního počítače může mít pouze jeden nastavení automatického škálování pro něj nakonfigurovali.
Každé nastavení automatického škálování však může mít několik profilů. Například jeden pro profil na základě výkonu škálování a druhý pro profil na základě plánu. Každý profil může mít víc pravidel, které jsou nakonfigurované na něm. Další informace o škálování najdete v tématu [postup škálování aplikace](../cloud-services/cloud-services-how-to-scale.md).

Zde jsou kroky, které budeme používat:

1. Vytvoření pravidel.
2. Vytvořte profily mapování pravidla, které jste vytvořili dříve na profily.
3. Volitelné: Konfigurace vlastností webhooku a e-mailu vytvořte oznámení o škálování.
4. Vytvoření nastavení automatického škálování s názvem na cílového prostředku tak, že mapuje profily a oznámení, které jste vytvořili v předchozích krocích.

Následující příklady ukazují, jak můžete vytvořit na nastavení automatického škálování pro sadu škálování virtuálního počítače pro operační systém Windows na základě pomocí metriky využití procesoru.

Nejprve vytvořte pravidlo pro škálovatelnou větší počet instancí.

```PowerShell
$rule1 = New-AzureRmAutoscaleRule -MetricName "Percentage CPU" -MetricResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -Operator GreaterThan -MetricStatistic Average -Threshold 60 -TimeGrain 00:01:00 -TimeWindow 00:10:00 -ScaleActionCooldown 00:10:00 -ScaleActionDirection Increase -ScaleActionValue 1
```        

Dále vytvořte pravidlo pro škálování v, s snížení počtu instancí.

```PowerShell
$rule2 = New-AzureRmAutoscaleRule -MetricName "Percentage CPU" -MetricResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -Operator GreaterThan -MetricStatistic Average -Threshold 30 -TimeGrain 00:01:00 -TimeWindow 00:10:00 -ScaleActionCooldown 00:10:00 -ScaleActionDirection Decrease -ScaleActionValue 1
```

Pak vytvořte profil pro pravidla.

```PowerShell
$profile1 = New-AzureRmAutoscaleProfile -DefaultCapacity 2 -MaximumCapacity 10 -MinimumCapacity 2 -Rules $rule1,$rule2 -Name "My_Profile"
```

Vytvoření vlastnosti webhooku.

```PowerShell
$webhook_scale = New-AzureRmAutoscaleWebhook -ServiceUri "https://example.com?mytoken=mytokenvalue"
```

Vytvořte oznámení vlastnost pro nastavení automatického škálování, včetně e-mailu a webhooku, který jste vytvořili dříve.

```PowerShell
$notification1= New-AzureRmAutoscaleNotification -CustomEmails ashwink@microsoft.com -SendEmailToSubscriptionAdministrators SendEmailToSubscriptionCoAdministrators -Webhooks $webhook_scale
```

Nakonec vytvořte nastavení automatického škálování přidat profil, který jste vytvořili výše.

```PowerShell
Add-AzureRmAutoscaleSetting -Location "East US" -Name "MyScaleVMSSSetting" -ResourceGroup big2 -TargetResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -AutoscaleProfiles $profile1 -Notifications $notification1
```

Další informace o správě nastavení automatického škálování najdete v tématu [Get-AutoscaleSetting](https://msdn.microsoft.com/library/mt282461.aspx).

## <a name="autoscale-history"></a>Historie škálování
Následující příklad ukazuje, jak zobrazit nedávné škálování a události výstrah. Pomocí vyhledávání protokolu aktivity a zobrazte historii škálování.

```PowerShell
Get-AzureRmLog -Caller "Microsoft.Insights/autoscaleSettings" -DetailedOutput -StartTime 2015-03-01
```

Můžete použít `Get-AzureRmAutoScaleHistory` rutiny načíst historii škálování.

```PowerShell
Get-AzureRmAutoScaleHistory -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/microsoft.insights/autoscalesettings/myScaleSetting -StartTime 2016-03-15 -DetailedOutput
```

Další informace najdete v tématu [Get-AutoscaleHistory](https://msdn.microsoft.com/library/mt282464.aspx).

### <a name="view-details-for-an-autoscale-setting"></a>Zobrazit podrobnosti o nastavení automatického škálování
Můžete použít `Get-Autoscalesetting` rutiny získat další informace o nastavení automatického škálování.

Následující příklad ukazuje údaje o všechna nastavení škálování v prostředku skupiny "myrg1'.

```PowerShell
Get-AzureRmAutoscalesetting -ResourceGroup myrg1 -DetailedOutput
```

Následující příklad ukazuje údaje o všech nastavení škálování v prostředku skupiny "myrg1' a konkrétně nastavení automatického škálování s názvem 'MyScaleVMSSSetting'.

```PowerShell
Get-AzureRmAutoscalesetting -ResourceGroup myrg1 -Name MyScaleVMSSSetting -DetailedOutput
```

### <a name="remove-an-autoscale-setting"></a>Odebrat nastavení automatického škálování
Můžete použít `Remove-Autoscalesetting` rutiny odstranit nastavení automatického škálování.

```PowerShell
Remove-AzureRmAutoscalesetting -ResourceGroup myrg1 -Name MyScaleVMSSSetting
```

## <a name="manage-log-profiles-for-activity-log"></a>Správa profilů protokolu pro protokol aktivit
Můžete vytvořit *protokolu profil* a exportu dat z aktivity protokolu do účtu úložiště a vy můžete nakonfigurovat uchovávání dat pro ni. Volitelně můžete také Streamovat data do vašeho centra událostí. Všimněte si, že tato funkce je aktuálně ve verzi Preview a je lze vytvořit pouze jeden profil protokolu podle předplatného. Následující rutiny s vaším aktuálním předplatným slouží k vytvoření a Správa profilů protokolu. Můžete také určitý odběr. I když k aktuálním předplatném výchozí nastavení prostředí PowerShell, můžete kdykoliv změnit této pomocí `Set-AzureRmContext`. Můžete nakonfigurovat protokolu aktivity a data trasy k žádnému účtu úložiště nebo Centrum událostí v rámci tohoto předplatného. Data se zapisují jako objekt blob soubory ve formátu JSON.

### <a name="get-a-log-profile"></a>Získat profil protokolu
Chcete-li načíst vaše stávající protokolu profilů, použijte `Get-AzureRmLogProfile` rutiny.

### <a name="add-a-log-profile-without-data-retention"></a>Přidat profil protokolu bez uchovávání dat
```PowerShell
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia
```

### <a name="remove-a-log-profile"></a>Odebrat profil protokolu
```PowerShell
Remove-AzureRmLogProfile -name my_log_profile_s1
```

### <a name="add-a-log-profile-with-data-retention"></a>Přidat profil protokolu s uchovávání dat
Můžete zadat **- RetentionInDays** vlastnost s počet dní, jako kladné celé číslo, které se uchovávají data.

```PowerShell
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia -RetentionInDays 90
```

### <a name="add-log-profile-with-retention-and-eventhub"></a>Přidat profil protokolu s uchovávání a EventHub
Kromě směrování dat do účtu úložiště, můžete také Streamovat ho do centra událostí. Nezapomeňte, že v této verzi preview a úložiště konfigurace účtu je povinná, ale konfigurace centra událostí je volitelné.

```PowerShell
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia -RetentionInDays 90
```

## <a name="configure-diagnostics-logs"></a>Konfigurace protokolů diagnostiky
Mnoho služeb Azure poskytují další protokoly a telemetrie, které můžete nakonfigurovat tak, aby uložení dat v účtu úložiště Azure, odesílat do centra událostí nebo odeslat k pracovnímu prostoru analýzy protokolů OMS. Tuto operaci může provést pouze na úrovni prostředků a úložiště účet nebo události rozbočovače musí být ve stejné oblasti jako cílový prostředek, kde je nakonfigurované nastavení diagnostiky.

### <a name="get-diagnostic-setting"></a>Získat nastavení diagnostiky
```PowerShell
Get-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp
```

Zakažte nastavení diagnostiky

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $false
```

Povolit nastavení diagnostiky bez uchování

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $true
```

Povolit nastavení diagnostiky s dobou uchování

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $true -RetentionEnabled $true -RetentionInDays 90
```

Povolit nastavení diagnostiky se zachováním pro konkrétní kategorii

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Network/networkSecurityGroups/viruela1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/sakteststorage -Categories NetworkSecurityGroupEvent -Enable $true -RetentionEnabled $true -RetentionInDays 90
```

Povolit nastavení diagnostiky pro službu Event Hubs

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Network/networkSecurityGroups/viruela1 -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Enable $true
```

Povolit nastavení diagnostiky pro OMS

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Network/networkSecurityGroups/viruela1 -WorkspaceId 76d785fd-d1ce-4f50-8ca3-858fc819ca0f -Enabled $true

```
