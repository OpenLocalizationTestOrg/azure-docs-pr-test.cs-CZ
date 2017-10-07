---
title: "brány sítě VPN aaaMonitor při řešení problémů sledovací proces sítě Azure | Microsoft Docs"
description: "Tento článek popisuje, jak diagnostikovat místní připojení Azure Automation a sledovací proces sítě"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: a607d0c862ea1be63c687717f0c5dc137db58a43
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-vpn-gateways-with-network-watcher-troubleshooting"></a>Sledování brány sítě VPN s řešení potíží s sledovací proces sítě

Získání hlubšímu porozumění na výkonu sítě je důležité tooprovide toocustomers spolehlivé služby. Je proto důležité toodetect síťové podmínky výpadku rychle a trvat stav výpadku hello toomitigate opravné akce. Automatizace Azure umožňuje tooimplement a spusťte úlohu programový způsobem prostřednictvím sady runbook. Pomocí Azure Automation vytvoří ideální recepturách pro provádění sítě nepřetržitý a Proaktivní monitorování a výstrahy.

## <a name="scenario"></a>Scénář

scénář Hello v hello následující obrázek je víceúrovňových aplikace, s na připojení místní navázat pomocí brány sítě VPN a tunelové propojení. Zajištění hello VPN Gateway je zapnutý a spuštění je důležité toohello výkonu aplikace.

Runbook se vytvoří s toocheck skript pro stav připojení hello tunelového připojení sítě VPN, pomocí toocheck hello prostředků řešení potíží s rozhraní API pro stav připojení tunelové propojení. Pokud hello stav není v pořádku, odešle aktivační procedury e-mailu tooadministrators.

![Příklad scénáře][scenario]

Tento scénář se:

- Vytváření volání hello runbook `Start-AzureRmNetworkWatcherResourceTroubleshooting` stav připojení tootroubleshoot rutiny
- Propojit plán toohello runbooku

## <a name="before-you-begin"></a>Než začnete

Než začnete tento scénář, musíte mít hello následující požadavky:

- Účet Azure automation v Azure. Zajistěte, aby účet automation hello hello nejnovější modulů a také obsahuje modul AzureRM.Network hello. Hello AzureRM.Network modulu je k dispozici v galerii modulů hello tooadd potřebujete-li ji tooyour účet automation.
- Musíte mít sadu přihlašovacích údajů nakonfigurovat ve službě Azure Automation. Další informace na [zabezpečení Azure Automation.](../automation/automation-security-overview.md)
- Platný server SMTP (Office 365, místní e-mailu nebo jiné) a přihlašovací údaje definované ve službě Azure Automation
- Nakonfigurované virtuální bránu sítě v Azure.
- Přihlásí se existující toostore hello kontejneru stávající účet úložiště.

> [!NOTE]
> Hello infrastruktury znázorněný v hello předcházející bitové kopie je pro účely obrázku a nejsou vytvořeny s hello kroky obsažené v tomto článku.

### <a name="create-hello-runbook"></a>Vytvoření sady runbook hello

Příklad hello tooconfiguring první krok Hello je toocreate hello runbook. Tento příklad používá účet Spustit jako. toolearn informace o účtech spustit jako, navštivte [ověření Runbooků pomocí účtu spustit v Azure jako](../automation/automation-sec-configure-azure-runas-account.md)

### <a name="step-1"></a>Krok 1

Přejděte tooAzure automatizace v hello [portál Azure](https://portal.azure.com) a klikněte na tlačítko **sady Runbook**

![Přehled účtu Automation][1]

### <a name="step-2"></a>Krok 2

Klikněte na tlačítko **přidat runbook** toostart hello proces vytvoření sady runbook hello.

![okno sady runbook][2]

### <a name="step-3"></a>Krok 3

V části **rychle vytvořit**, klikněte na tlačítko **vytvořit nový runbook** toocreate hello runbook.

![runbook okně Přidat][3]

### <a name="step-4"></a>Krok 4

V tomto kroku jsme pojmenujte hello runbook, v příkladu hello se označuje jako **Get-VPNGatewayStatus**. Je důležité toogive hello runbook popisný název a doporučuje pod názvem, který následuje standardní standardy pro vytváření názvů prostředí PowerShell. Typ runbooku Hello v tomto příkladu je **prostředí PowerShell**, hello ostatní možnosti patří grafický pracovního postupu Powershellu a pracovní postup grafické prostředí PowerShell.

![okno sady runbook][4]

### <a name="step-5"></a>Krok 5

V tomto kroku hello sady runbook se vytvoří, hello následující ukázka kódu obsahuje že všechny hello kód potřebný například hello. Hello položky v hello kódu, které obsahují \<hodnotu\> potřebovat toobe nahradit hello hodnotami ze svého předplatného.

Použití hello následující kód jako klikněte na tlačítko **uložit**

```PowerShell
# Set these variables toohello proper values for your environment
$o365AutomationCredential = "<Office 365 account>"
$fromEmail = "<from email address>"
$toEmail = "<tooemail address>"
$smtpServer = "<smtp.office365.com>"
$smtpPort = 587
$runAsConnectionName = "<AzureRunAsConnection>"
$subscriptionId = "<subscription id>"
$region = "<Azure region>"
$vpnConnectionName = "<vpn connection name>"
$vpnConnectionResourceGroup = "<resource group name>"
$storageAccountName = "<storage account name>"
$storageAccountResourceGroup = "<resource group name>"
$storageAccountContainer = "<container name>"

# Get credentials for Office 365 account
$cred = Get-AutomationPSCredential -Name $o365AutomationCredential

# Get hello connection "AzureRunAsConnection "
$servicePrincipalConnection=Get-AutomationConnection -Name $runAsConnectionName

"Logging in tooAzure..."
Add-AzureRmAccount `
    -ServicePrincipal `
    -TenantId $servicePrincipalConnection.TenantId `
    -ApplicationId $servicePrincipalConnection.ApplicationId `
    -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint
"Setting context tooa specific subscription"
Set-AzureRmContext -SubscriptionId $subscriptionId

$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq $region }
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName
$connection = Get-AzureRmVirtualNetworkGatewayConnection -Name $vpnConnectionName -ResourceGroupName $vpnConnectionResourceGroup
$sa = Get-AzureRmStorageAccount -Name $storageAccountName -ResourceGroupName $storageAccountResourceGroup 
$storagePath = "$($sa.PrimaryEndpoints.Blob)$($storageAccountContainer)"
$result = Start-AzureRmNetworkWatcherResourceTroubleshooting -NetworkWatcher $networkWatcher -TargetResourceId $connection.Id -StorageId $sa.Id -StoragePath $storagePath

if($result.code -ne "Healthy")
    {
        $body = "Connection for $($connection.name) is: $($result.code) `n$($result.results[0].summary) `nView hello logs at $($storagePath) toolearn more."
        Write-Output $body
        $subject = "$($connection.name) Status"
        Send-MailMessage `
        -too$toEmail `
        -Subject $subject `
        -Body $body `
        -UseSsl `
        -Port $smtpPort `
        -SmtpServer $smtpServer `
        -From $fromEmail `
        -BodyAsHtml `
        -Credential $cred
    }
else
    {
    Write-Output ("Connection Status is: $($result.code)")
    }
```

### <a name="step-6"></a>Krok 6

Po uložení hello runbook plánu musí být propojená tooit tooautomate hello spuštění sady runbook hello. proces hello toostart, klikněte na tlačítko **plán**.

![Krok 6][6]

## <a name="link-a-schedule-toohello-runbook"></a>Propojit plán toohello runbooku

Je nutné vytvořit nový plán. Klikněte na tlačítko **propojit plán tooyour runbook**.

![Krok 7][7]

### <a name="step-1"></a>Krok 1

Na hello **plán** okně klikněte na tlačítko **vytvoření nového plánu**

![Krok 8][8]

### <a name="step-2"></a>Krok 2

Na hello **nový plán** výplně okno se informace o plánu hello. v následujícím seznamu hello jsou Hello hodnoty, které lze nastavit:

- **Název** -hello popisný název plánu hello.
- **Popis** – popis hello plánu.
- **Spustí** – tato hodnota je kombinací datum, čas a časové pásmo, které tvoří hello čas hello plán aktivační události.
- **Opakování** – tato hodnota určuje hello plány opakování.  Platné hodnoty jsou **jednou** nebo **opakovaná**.
- **Opakovat každý** – interval opakování hello hello plánu v hodiny, dny, týdny nebo měsíce.
- **Nastavit dobu platnosti** -hello hodnota určuje, pokud by měla vypršet hello plánu, nebo ne. Můžete nastavit příliš**Ano** nebo **ne**. Platné datum a čas se toobe zadán, pokud ano je vybrán.

> [!NOTE]
> Pokud potřebujete toohave sady runbook spustit častěji než každou hodinu, je nutné vytvořit více plánů v různých intervalech (to znamená, 15, 30, 45 minut po hodině hello)

![Krok 9][9]

### <a name="step-3"></a>Krok 3

Klikněte na Uložit toosave hello plán toohello runbook.

![Krok 10][10]

## <a name="next-steps"></a>Další kroky

Teď, když máte představu o tom, toointegrate sledovací proces sítě řešení potíží s Azure Automation, zjistěte, jak zaznamená tootrigger paketů na virtuální počítač výstrahy navštivte stránky [vytvořit zaznamenání výstrahy spouštěná paketů s sledovací proces sítě Azure](network-watcher-alert-triggered-packet-capture.md).

<!-- images -->
[scenario]: ./media/network-watcher-monitor-with-azure-automation/scenario.png
[1]: ./media/network-watcher-monitor-with-azure-automation/figure1.png
[2]: ./media/network-watcher-monitor-with-azure-automation/figure2.png
[3]: ./media/network-watcher-monitor-with-azure-automation/figure3.png
[4]: ./media/network-watcher-monitor-with-azure-automation/figure4.png
[5]: ./media/network-watcher-monitor-with-azure-automation/figure5.png
[6]: ./media/network-watcher-monitor-with-azure-automation/figure6.png
[7]: ./media/network-watcher-monitor-with-azure-automation/figure7.png
[8]: ./media/network-watcher-monitor-with-azure-automation/figure8.png
[9]: ./media/network-watcher-monitor-with-azure-automation/figure9.png
[10]: ./media/network-watcher-monitor-with-azure-automation/figure10.png
