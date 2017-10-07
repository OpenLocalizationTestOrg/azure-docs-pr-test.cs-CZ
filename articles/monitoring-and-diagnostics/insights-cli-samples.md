---
title: "Ukázky úvodní aaaAzure monitorování 1.0 rozhraní příkazového řádku. | Dokumentace Microsoftu"
description: "Vzorové příkazy rozhraní příkazového řádku 1.0 pro Azure monitorování funkce. Azure monitorování je služba Microsoft Azure, což vám umožní toosend oznámení o výstrahách, volání webové adresy URL založené na hodnotách nakonfigurované telemetrická data a škálování cloudové služby, virtuální počítače a webové aplikace."
author: kamathashwin
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 1653aa81-0ee6-4622-9c65-d4801ed9006f
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: ashwink
ms.openlocfilehash: 6cd9cd62b3a1977276563f5e43f5384ccca66247
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-monitor--cross-platform-cli-10-quick-start-samples"></a>Ukázek Azure CLI monitorování napříč platformami 1.0 rychlý start
Tento článek ukazuje ukázkové toohelp příkazy rozhraní příkazového řádku (CLI) měli přístup k funkcím monitorování Azure. Azure monitorování umožňuje tooAutoScale cloudové služby, virtuální počítače a webové aplikace a toosend oznámení výstrah nebo volání webové adresy URL založené na hodnotách nakonfigurované telemetrická data.

> [!NOTE]
> Azure monitorování je hello nový název pro co byla volána "Statistika Azure" až 25 září 2016. Ale hello obory názvů a proto níže uvedené příkazy hello stále obsahovat hello "insights".
> 
> 

## <a name="prerequisites"></a>Požadavky
Pokud jste ještě nenainstalovali hello příkazového řádku Azure CLI, přečtěte si téma [hello instalace rozhraní příkazového řádku Azure](../cli-install-nodejs.md). Pokud jste obeznámeni s rozhraní příkazového řádku Azure, můžete přečíst další informace naleznete na [hello použití Azure CLI pro Mac, Linux a Windows pomocí Azure Resource Manageru](../xplat-cli-azure-resource-manager.md).

V systému Windows, nainstalujte npm z hello [Node.js webu](https://nodejs.org/). Po dokončení instalace hello CMD.exe pomocí oprávnění spustit jako správce, spusťte následující hello ze složky hello nainstalovanou npm:

```console
npm install azure-cli --global
```

Dále přejděte tooany složku nebo umístění chcete a zadejte hello příkazového řádku:

```console
azure help
```

## <a name="log-in-tooazure"></a>Přihlaste se tooAzure
prvním krokem Hello je toologin tooyour účet Azure.

```console
azure login
```

Po spuštění tohoto příkazu, máte toosign v prostřednictvím hello pokyny na obrazovce hello. Potom zobrazí váš účet, TenantId a výchozí ID předplatného. Všechny příkazy lze použít v kontextu hello předplatného výchozí.

toolist hello podrobnosti o vaším aktuálním předplatným, použijte následující příkaz hello.

```console
azure account show
```

toochange pracovní kontext tooa jiného předplatného, hello použijte následující příkaz.

```console
azure account set "subscription ID or subscription name"
```

toouse Azure Resource Manager a monitorování Azure příkazů, je nutné toobe v režimu Azure Resource Manager.

```console
azure config mode arm
```

tooview seznam všechny podporované příkazy Azure monitorování, proveďte následující hello.

```console
azure insights
```

## <a name="view-activity-log-for-a-subscription"></a>Zobrazit protokol aktivity pro předplatné
tooview seznam aktivity protokolu událostí, proveďte následující hello.

```console
azure insights logs list [options]
```

Zkuste hello následující tooview všechny dostupné možnosti.

```console
azure insights logs list -help
```

Tady je příklad toolist protokoly podle resourceGroup

```console
azure insights logs list --resourceGroup "myrg1"
```

Příklad toolist protokoly volající

```console
azure insights logs list --caller "myname@company.com"
```

Příklad toolist protokoly volající na typ prostředku, v rámci počáteční a koncové datum

```console
azure insights logs list --resourceProvider "Microsoft.Web" --caller "myname@company.com" --startTime 2016-03-08T00:00:00Z --endTime 2016-03-16T00:00:00Z
```

## <a name="work-with-alerts"></a>Práce s výstrahami
V části toowork hello s výstrahami, můžete použít informace hello.

### <a name="get-alert-rules-in-a-resource-group"></a>Získat pravidla výstrah ve skupině prostředků
```console
azure insights alerts rule list abhingrgtest123
azure insights alerts rule list abhingrgtest123 --ruleName andy0323
```

### <a name="create-a-metric-alert-rule"></a>Vytvoření metriky pravidlo výstrahy
```console
azure insights alerts actions email create --customEmails foo@microsoft.com
azure insights alerts actions webhook create https://someuri.com
azure insights alerts rule metric set andy0323 eastus abhingrgtest123 PT5M GreaterThan 2 /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/Default-Web-EastUS/providers/Microsoft.Web/serverfarms/Default1 BytesReceived Total
```

### <a name="create-webtest-alert-rule"></a>Vytvořit pravidlo výstrahy testu webu
```console
azure insights alerts rule webtest set leowebtestr1-webtestr1 eastus Default-Web-WestUS PT5M 1 GSMT_AvRaw /subscriptions/b67f7fec-69fc-4974-9099-a26bd6ffeda3/resourcegroups/Default-Web-WestUS/providers/microsoft.insights/webtests/leowebtestr1-webtestr1
```

### <a name="delete-an-alert-rule"></a>Odstranit pravidlo výstrahy
```console
azure insights alerts rule delete abhingrgtest123 andy0323
```

## <a name="log-profiles"></a>Profily protokolu
Použijte hello informace v této části toowork pomocí protokolu profilů.

### <a name="get-a-log-profile"></a>Získat profil protokolu
```console
azure insights logprofile list
azure insights logprofile get -n default
```


### <a name="add-a-log-profile-without-retention"></a>Přidat profil protokolu bez uchování
```console
azure insights logprofile add --name default --storageId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/insightsintegration7777 --locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia
```

### <a name="remove-a-log-profile"></a>Odebrat profil protokolu
```console
azure insights logprofile delete --name default
```

### <a name="add-a-log-profile-with-retention"></a>Přidat profil protokolu s dobou uchování
```console
azure insights logprofile add --name default --storageId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/insightsintegration7777 --locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia --retentionInDays 90
```

### <a name="add-a-log-profile-with-retention-and-eventhub"></a>Přidat profil protokolu s uchovávání a EventHub
```console
azure insights logprofile add --name default --storageId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/insightsintegration7777 --serviceBusRuleId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/testshoeboxeastus/authorizationrules/RootManageSharedAccessKey --locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia --retentionInDays 90
```


## <a name="diagnostics"></a>Diagnostika
Použijte hello informace v této části toowork se nastavení diagnostiky.

### <a name="get-a-diagnostic-setting"></a>Získat nastavení diagnostiky
```console
azure insights diagnostic get --resourceId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/andyrg/providers/Microsoft.Logic/workflows/andy0315logicapp
```

### <a name="disable-a-diagnostic-setting"></a>Zakažte nastavení diagnostiky
```console
azure insights diagnostic set --resourceId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/andyrg/providers/Microsoft.Logic/workflows/andy0315logicapp --storageId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/shibanitesting --enabled false
```

### <a name="enable-a-diagnostic-setting-without-retention"></a>Povolit nastavení diagnostiky bez uchování
```console
azure insights diagnostic set --resourceId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/andyrg/providers/Microsoft.Logic/workflows/andy0315logicapp --storageId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/shibanitesting --enabled true
```


## <a name="autoscale"></a>Automatické škálování
Použijte hello informace v této části toowork s nastavení automatického škálování. Je nutné toomodify tyto příklady.

### <a name="get-autoscale-settings-for-a-resource-group"></a>Získat nastavení automatického škálování pro skupinu prostředků.
```console
azure insights autoscale setting list montest2
```

### <a name="get-autoscale-settings-by-name-in-a-resource-group"></a>Získat nastavení automatického škálování podle názvu ve skupině prostředků
```console
azure insights autoscale setting list montest2 -n setting2
```


### <a name="set-auotoscale-settings"></a>Nastavení auotoscale
```console
azure insights autoscale setting set montest2 -n setting2 --settingSpec
```
