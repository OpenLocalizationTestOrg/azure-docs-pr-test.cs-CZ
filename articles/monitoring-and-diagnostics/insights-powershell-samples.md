---
title: "Ukázky úvodní aaaAzure monitorování PowerShell. | Dokumentace Microsoftu"
description: "Pomocí prostředí PowerShell tooaccess Azure monitorování funkce, jako je automatické škálování, výstrahy, webhooky a hledání protokoly aktivity."
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
ms.openlocfilehash: 6eece0b0227e0bbf08225bd330d0601169911f55
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-monitor-powershell-quick-start-samples"></a>Ukázek Azure PowerShell monitorování rychlý start
Tento článek ukazuje ukázkové toohelp příkazy prostředí PowerShell měli přístup k funkcím monitorování Azure. Azure monitorování umožňuje tooAutoScale cloudové služby, virtuální počítače a webové aplikace a toosend oznámení výstrah nebo volání webové adresy URL založené na hodnotách nakonfigurované telemetrická data.

> [!NOTE]
> Azure monitorování je hello nový název pro co byla volána "Statistika Azure" až 25 září 2016. Ale hello obory názvů a proto hello následující příkazy stále obsahovat hello "insights".
> 
> 

## <a name="set-up-powershell"></a>Nastavení prostředí PowerShell
Pokud jste to ještě neudělali, nastavte toorun prostředí PowerShell v počítači. Další informace najdete v tématu [jak tooInstall a konfigurace prostředí PowerShell](/powershell/azure/overview).

## <a name="examples-in-this-article"></a>Příklady v tomto článku
Příklady Hello v článku hello ilustrují, jak můžete použít rutiny Azure monitorování. Můžete také zkontrolovat hello celý seznam rutin Azure Powershellu monitorování v [rutiny Azure monitorování (přehled)](https://msdn.microsoft.com/library/azure/mt282452#40v=azure.200#41.aspx).

## <a name="sign-in-and-use-subscriptions"></a>Přihlásit a používat odběrů
Nejdřív přihlaste tooyour předplatného Azure.

```PowerShell
Login-AzureRmAccount
```

To vyžaduje toosign v. Až to uděláte, váš účet, zobrazí se TenantID a výchozí ID předplatného. Všechny hello pracovní rutiny Azure v kontextu hello předplatného výchozí. tooview hello seznam předplatných, budete mít přístup k, použijte následující příkaz hello.

```PowerShell
Get-AzureRmSubscription
```

toochange pracovní kontext tooa jiné předplatné, hello použijte následující příkaz.

```PowerShell
Set-AzureRmContext -SubscriptionId <subscriptionid>
```


## <a name="retrieve-activity-log-for-a-subscription"></a>Načtení protokol aktivit pro předplatné
Použití hello `Get-AzureRmLog` rutiny.  Hello následuje několik běžných příkladů.

Získání položky protokolu z této toopresent času a data:

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

Následující příkaz načte Hello hello posledních 1000 události z protokolu aktivit hello:

```PowerShell
Get-AzureRmLog -MaxEvents 1000
```

`Get-AzureRmLog`podporuje mnoho dalších parametrů. V tématu hello `Get-AzureRmLog` pro další informace.

> [!NOTE]
> `Get-AzureRmLog`poskytuje pouze 15 dní historie. Pomocí hello **- MaxEvents** parametr vám umožní tooquery hello posledních N události, nad rámec 15 dnů. tooaccess události starší než 15 dny, použijte hello REST API nebo sady SDK (C# ukázku pomocí hello SDK). Pokud neuvedete **StartTime**, pak hello výchozí hodnota je **EndTime** minus jednu hodinu. Pokud neuvedete **EndTime**, pak hello výchozí hodnota je aktuální čas. Všechny časy jsou ve formátu UTC.
> 
> 

## <a name="retrieve-alerts-history"></a>Načtení historie výstrah
všechny výstrahy událostí, můžete dát dotaz na tooview hello protokolů Azure Resource Manager pomocí hello následující příklady.

```PowerShell
Get-AzureRmLog -Caller "Microsoft.Insights/alertRules" -DetailedOutput -StartTime 2015-03-01
```

pravidlo tooview hello historie pro konkrétní výstrahu, můžete použít hello `Get-AzureRmAlertHistory` rutiny předávání v ID prostředku hello hello pravidlo výstrahy.

```PowerShell
Get-AzureRmAlertHistory -ResourceId /subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/alertrules/myalert -StartTime 2016-03-1 -Status Activated
```

Hello `Get-AzureRmAlertHistory` rutina podporuje různé parametry. Další informace najdete v tématu [Get-AlertHistory](https://msdn.microsoft.com/library/mt282453.aspx).

## <a name="retrieve-information-on-alert-rules"></a>Načíst informace o pravidla výstrah
Všechny následující příkazy hello fungují na skupinu prostředků s názvem "montest".

Zobrazte všechny vlastnosti hello hello pravidlo výstrahy:

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
Můžete použít hello `Add-AlertRule` toocreate rutiny, aktualizovat nebo zakázat pravidlo výstrahy.

Můžete vytvořit e-mailu a webhooku vlastností pomocí `New-AzureRmAlertRuleEmail` a `New-AzureRmAlertRuleWebhook`, v uvedeném pořadí. V rutině hello pravidlo výstrahy, přiřadit jako akce toohello **akce** vlastnost hello pravidlo výstrahy.

Hello následující tabulka popisuje hello parametry a hodnoty používané toocreate výstrahu pomocí metriky.

| Parametr | hodnota |
| --- | --- |
| Name (Název) |simpletestdiskwrite |
| Umístění tohoto pravidla výstrah |Východ USA |
| ResourceGroup |montest |
| TargetResourceId |/subscriptions/S1/resourceGroups/montest/providers/Microsoft.COMPUTE/virtualMachines/testconfig |
| MetricName hello výstrahy, která je vytvořena |\PhysicalDisk (_celkem) \Disk zápisů za sekundu. V tématu hello `Get-MetricDefinitions` rutiny o tom, jak tooretrieve hello přesný metriky názvy |
| Operátor |GreaterThan |
| Prahová hodnota (počet za sekundu za pro tato metrika) |1 |
| Velikost_okna (ve formátu hh: mm:) |00:05:00 |
| agregátoru (statistiky hello metriku, která používá průměrný počet, v takovém případě) |Průměr |
| vlastní e-mailů (řetězec pole) |'foo@example.com','bar@example.com' |
| odesílání e-mailu tooowners, přispěvatelé a čtenáři |-SendToServiceOwners |

Vytvoří akci, e-mailu

```PowerShell
$actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
```

Vytvoření Webhooku akce

```PowerShell
$actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://example.com?token=mytoken
```

Vytvořit pravidlo výstrahy hello na hello procesoru % metriky na klasické virtuální počítač

```PowerShell
Add-AzureRmMetricAlertRule -Name vmcpu_gt_1 -Location "East US" -ResourceGroup myrg1 -TargetResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.ClassicCompute/virtualMachines/my_vm1 -MetricName "Percentage CPU" -Operator GreaterThan -Threshold 1 -WindowSize 00:05:00 -TimeAggregationOperator Average -Actions $actionEmail, $actionWebhook -Description "alert on CPU > 1%"
```

Načtení pravidlo výstrahy hello

```PowerShell
Get-AzureRmAlertRule -Name vmcpu_gt_1 -ResourceGroup myrg1 -DetailedOutput
```

výstrahy rutiny Hello přidat také aktualizuje pravidlo hello, pokud se pravidlo výstrahy pro hello zadané vlastnosti již existuje. pravidlo výstrahy toodisable zahrnout parametr hello **- DisableRule**.

## <a name="get-a-list-of-available-metrics-for-alerts"></a>Získat seznam dostupné metriky pro výstrahy
Můžete použít hello `Get-AzureRmMetricDefinition` rutiny tooview hello seznamu všechny metriky pro konkrétní zdroje.

```PowerShell
Get-AzureRmMetricDefinition -ResourceId <resource_id>
```

Hello následující příklad vygeneruje tabulku s hello metrika název a hello jednotky pro ni.

```PowerShell
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

Úplný seznam dostupných možností pro `Get-AzureRmMetricDefinition` je k dispozici na [Get-MetricDefinitions](https://msdn.microsoft.com/library/mt282458.aspx).

## <a name="create-and-manage-autoscale-settings"></a>Vytvořit a spravovat nastavení automatického škálování
Prostředek, například webovou aplikaci, virtuální počítač, Cloudová služba nebo sadu škálování virtuálního počítače může mít pouze jeden nastavení automatického škálování pro něj nakonfigurovali.
Každé nastavení automatického škálování však může mít několik profilů. Například jeden pro profil na základě výkonu škálování a druhý pro profil na základě plánu. Každý profil může mít víc pravidel, které jsou nakonfigurované na něm. Další informace o škálování najdete v tématu [jak tooAutoscale aplikace](../cloud-services/cloud-services-how-to-scale.md).

Zde jsou hello kroky, které budeme používat:

1. Vytvoření pravidel.
2. Vytvořte profily mapování hello pravidla, které jste vytvořili dříve toohello profily.
3. Volitelné: Konfigurace vlastností webhooku a e-mailu vytvořte oznámení o škálování.
4. Vytvoření nastavení automatického škálování s názvem hello cílovému prostředku tak, že mapuje hello profily a oznámení, které jste vytvořili v předchozích krocích hello.

Hello následující příklady ukazují můžete jak můžete vytvořit na nastavení automatického škálování pro sadu škálování virtuálního počítače pro operační systém Windows na základě pomocí metriky využití hello procesoru.

Nejprve vytvořte pravidlo tooscale-out, větší počet instancí.

```PowerShell
$rule1 = New-AzureRmAutoscaleRule -MetricName "Percentage CPU" -MetricResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -Operator GreaterThan -MetricStatistic Average -Threshold 60 -TimeGrain 00:01:00 -TimeWindow 00:10:00 -ScaleActionCooldown 00:10:00 -ScaleActionDirection Increase -ScaleActionValue 1
```        

Dále vytvořte pravidlo tooscale v s snížení počtu instancí.

```PowerShell
$rule2 = New-AzureRmAutoscaleRule -MetricName "Percentage CPU" -MetricResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -Operator GreaterThan -MetricStatistic Average -Threshold 30 -TimeGrain 00:01:00 -TimeWindow 00:10:00 -ScaleActionCooldown 00:10:00 -ScaleActionDirection Decrease -ScaleActionValue 1
```

Pak vytvořte profil pro pravidla hello.

```PowerShell
$profile1 = New-AzureRmAutoscaleProfile -DefaultCapacity 2 -MaximumCapacity 10 -MinimumCapacity 2 -Rules $rule1,$rule2 -Name "My_Profile"
```

Vytvoření vlastnosti webhooku.

```PowerShell
$webhook_scale = New-AzureRmAutoscaleWebhook -ServiceUri "https://example.com?mytoken=mytokenvalue"
```

Vytvořit hello oznámení vlastnost pro nastavení automatického škálování hello, včetně e-mailů a hello webhooku, který jste vytvořili dříve.

```PowerShell
$notification1= New-AzureRmAutoscaleNotification -CustomEmails ashwink@microsoft.com -SendEmailToSubscriptionAdministrators SendEmailToSubscriptionCoAdministrators -Webhooks $webhook_scale
```

Nakonec vytvořte hello škálování nastavení tooadd hello profil, který jste vytvořili výše.

```PowerShell
Add-AzureRmAutoscaleSetting -Location "East US" -Name "MyScaleVMSSSetting" -ResourceGroup big2 -TargetResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -AutoscaleProfiles $profile1 -Notifications $notification1
```

Další informace o správě nastavení automatického škálování najdete v tématu [Get-AutoscaleSetting](https://msdn.microsoft.com/library/mt282461.aspx).

## <a name="autoscale-history"></a>Historie škálování
Hello následující příklad ukazuje, jak zobrazit nedávné škálování a výstraha události. Použijte automatické škálování historie hello vyhledávání tooview hello aktivity protokolu.

```PowerShell
Get-AzureRmLog -Caller "Microsoft.Insights/autoscaleSettings" -DetailedOutput -StartTime 2015-03-01
```

Můžete použít hello `Get-AzureRmAutoScaleHistory` rutiny tooretrieve historie škálování.

```PowerShell
Get-AzureRmAutoScaleHistory -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/microsoft.insights/autoscalesettings/myScaleSetting -StartTime 2016-03-15 -DetailedOutput
```

Další informace najdete v tématu [Get-AutoscaleHistory](https://msdn.microsoft.com/library/mt282464.aspx).

### <a name="view-details-for-an-autoscale-setting"></a>Zobrazit podrobnosti o nastavení automatického škálování
Můžete použít hello `Get-Autoscalesetting` rutiny tooretrieve Další informace o nastavení automatického škálování hello.

Hello následující příklad ukazuje údaje o všechna nastavení automatického škálování v myrg1' hello prostředku skupiny".

```PowerShell
Get-AzureRmAutoscalesetting -ResourceGroup myrg1 -DetailedOutput
```

Hello následující příklad zobrazuje podrobnosti o všech nastaveních škálování v myrg1' hello prostředku skupiny"a konkrétně hello nastavení automatického škálování s názvem 'MyScaleVMSSSetting'.

```PowerShell
Get-AzureRmAutoscalesetting -ResourceGroup myrg1 -Name MyScaleVMSSSetting -DetailedOutput
```

### <a name="remove-an-autoscale-setting"></a>Odebrat nastavení automatického škálování
Můžete použít hello `Remove-Autoscalesetting` toodelete rutiny nastavení automatického škálování.

```PowerShell
Remove-AzureRmAutoscalesetting -ResourceGroup myrg1 -Name MyScaleVMSSSetting
```

## <a name="manage-log-profiles-for-activity-log"></a>Správa profilů protokolu pro protokol aktivit
Můžete vytvořit *protokolu profil* a exportu dat z vašeho účtu úložiště tooa protokolu aktivity a vy můžete nakonfigurovat uchovávání dat pro ni. Volitelně můžete také stream hello data tooyour centra událostí. Všimněte si, že tato funkce je aktuálně ve verzi Preview a je lze vytvořit pouze jeden profil protokolu podle předplatného. Můžete použít následující rutiny s vaší aktuální předplatné toocreate hello a spravovat profily protokolu. Můžete také určitý odběr. I když prostředí PowerShell výchozí toohello aktuální předplatné, můžete kdykoliv změnit této pomocí `Set-AzureRmContext`. V rámci tohoto předplatného můžete nakonfigurovat účet úložiště aktivity protokolu tooroute data tooany nebo Centrum událostí. Data se zapisují jako objekt blob soubory ve formátu JSON.

### <a name="get-a-log-profile"></a>Získat profil protokolu
toofetch existujících protokolu profilů, použijte hello `Get-AzureRmLogProfile` rutiny.

### <a name="add-a-log-profile-without-data-retention"></a>Přidat profil protokolu bez uchovávání dat
```PowerShell
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia
```

### <a name="remove-a-log-profile"></a>Odebrat profil protokolu
```PowerShell
Remove-AzureRmLogProfile -name my_log_profile_s1
```

### <a name="add-a-log-profile-with-data-retention"></a>Přidat profil protokolu s uchovávání dat
Můžete zadat hello **- RetentionInDays** vlastnost s hello počet dní, jako kladné celé číslo, které mají být uchována hello data.

```PowerShell
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia -RetentionInDays 90
```

### <a name="add-log-profile-with-retention-and-eventhub"></a>Přidat profil protokolu s uchovávání a EventHub
V toorouting přidání účtu toostorage dat je můžete také Streamovat ho tooan centra událostí. Poznámka: v této verzi preview verze a hello konfigurace účtu úložiště je povinná však Konfigurace centra událostí je volitelné.

```PowerShell
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia -RetentionInDays 90
```

## <a name="configure-diagnostics-logs"></a>Konfigurace protokolů diagnostiky
Mnoho služeb Azure poskytují další protokoly a telemetrii, kterou může být nakonfigurované toosave data ve vašem účtu úložiště Azure odeslat tooEvent centra nebo odeslat pracovní prostor analýzy protokolů OMS tooan. Tuto operaci může provést pouze na úrovni prostředků a hello úložiště účet nebo event hub by měl být součástí hello stejné oblasti jako hello cílový prostředek, kde je nakonfigurované nastavení diagnostiky hello.

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
