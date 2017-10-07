---
title: "aaaDeploy a spravovat Notification Hubs pomocí prostředí PowerShell"
description: "Jak tooCreate a spravovat Notification Hubs pomocí prostředí PowerShell pro automatizaci"
services: notification-hubs
documentationcenter: 
author: ysxu
manager: erikre
editor: 
ms.assetid: 7c58f2c8-0399-42bc-9e1e-a7f073426451
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: powershell
ms.devlang: na
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: 8835bdefa0d360354263eab8040259ad08281771
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-manage-notification-hubs-using-powershell"></a>Nasazení a správa Notification Hubs pomocí PowerShellu
## <a name="overview"></a>Přehled
Tento článek ukazuje, jak toouse vytvořit a spravovat Azure Notification Hubs pomocí prostředí PowerShell. Hello následující běžné úlohy automatizace jsou uvedené v tomto tématu.

* Vytvoření centra oznámení
* Nastavení přihlašovacích údajů

Pokud pro vaše centra oznámení musíte taky toocreate nový obor názvů service bus, najdete v části [spravovat služby Service Bus pomocí prostředí PowerShell](../service-bus-messaging/service-bus-powershell-how-to-provision.md).

Správa centra oznámení nepodporuje přímo hello rutiny zahrnuté v prostředí Azure PowerShell. Nejlepším postupem Hello z prostředí PowerShell se tooreference hello Microsoft.Azure.NotificationHubs.dll sestavení. sestavení Hello je distribuován s hello [balíček Microsoft Azure Notification Hubs NuGet](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

## <a name="prerequisites"></a>Požadavky
Před zahájením tohoto článku, musíte mít následující hello:

* Předplatné Azure. Azure je platforma, na základě předplatného. Další informace o získání předplatného najdete v tématu [možnostech nákupu], [nabízí člen], nebo [bezplatné zkušební verze].
* Počítač s prostředím Azure PowerShell. Pokyny najdete v tématu [nainstalovat a nakonfigurovat Azure PowerShell].
* Obecné znalosti skriptů prostředí PowerShell, balíčků NuGet a hello rozhraní .NET Framework.

## <a name="including-a-reference-toohello-net-assembly-for-service-bus"></a>Včetně referenční sestavení .NET toohello pro Service Bus
Správa Azure Notification Hubs ještě není součástí hello rutiny prostředí PowerShell v prostředí Azure PowerShell. centra oznámení tooprovision, můžete použít klienta rozhraní .NET hello součástí hello [balíček Microsoft Azure Notification Hubs NuGet](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

Nejprve zkontrolujte, zda váš skript najdou hello **Microsoft.Azure.NotificationHubs.dll** sestavení, která je nainstalována jako balíčku NuGet v projektu sady Visual Studio. V pořadí toobe flexibilní hello skript provede tyto kroky:

1. Určuje, kdy byl vyvolán cesta hello.
2. Traverses hello cestu, dokud nenajde složku s názvem `packages`. Tato složka se vytvoří při instalaci balíčků NuGet pro projekty v sadě Visual Studio.
3. Rekurzivní hledání hello `packages` složku pro sestavení s názvem **Microsoft.Azure.NotificationHubs.dll**.
4. Odkazy na sestavení hello, takže hello typy jsou k dispozici pro pozdější použití.

Zde je, jak tyto kroky jsou implementované ve skriptu prostředí PowerShell:

``` powershell

try
{
    # WARNING: Make sure tooreference hello latest version of Microsoft.Azure.NotificationHubs.dll
    Write-Output "Adding hello [Microsoft.Azure.NotificationHubs.dll] assembly toohello script..."
    $scriptPath = Split-Path (Get-Variable MyInvocation -Scope 0).Value.MyCommand.Path
    $packagesFolder = (Split-Path $scriptPath -Parent) + "\packages"
    $assembly = Get-ChildItem $packagesFolder -Include "Microsoft.Azure.NotificationHubs.dll" -Recurse
    Add-Type -Path $assembly.FullName

    Write-Output "hello [Microsoft.Azure.NotificationHubs.dll] assembly has been successfully added toohello script."
}

catch [System.Exception]
{
    Write-Error("Could not add hello Microsoft.Azure.NotificationHubs.dll assembly toohello script. Make sure you build hello solution before running hello provisioning script.")
}
```

## <a name="create-hello-namespacemanager-class"></a>Vytvořte třídu NamespaceManager hello
tooprovision Notification Hubs, vytvořte instanci hello [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.namespacemanager.aspx) třídy z hello SDK. 

Můžete použít hello [Get-AzureSBAuthorizationRule] rutiny zahrnuté do prostředí Azure PowerShell tooretrieve autorizační pravidlo, které se používá tooprovide připojovací řetězec. Uložíme odkaz toohello `NamespaceManager` instance v hello `$NamespaceManager` proměnné. Budeme používat `$NamespaceManager` tooprovision centra oznámení.

``` powershell
$sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
# Create hello NamespaceManager object toocreate hello hub
Write-Output "Creating a NamespaceManager object for hello [$Namespace] namespace..."
$NamespaceManager=[Microsoft.Azure.NotificationHubs.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
Write-Output "NamespaceManager object for hello [$Namespace] namespace has been successfully created."
```


## <a name="provisioning-a-new-notification-hub"></a>Zřizování nového centra oznámení
tooprovision nového centra oznámení, používat hello [rozhraní API .NET pro centra oznámení].

V této části hello skriptu nastavíte čtyři lokální proměnné. 

1. `$Namespace`: Nastavte tento toohello název oboru názvů hello místo toocreate centra oznámení.
2. `$Path`: Nastavte tento název cesty toohello hello nového centra oznámení.  Například "MyHub".    
3. `$WnsPackageSid`: Nastavte tento balíček toohello SID pro vás aplikace pro Windows z hello [Centrum vývojářů pro Windows](http://go.microsoft.com/fwlink/p/?linkid=266582&clcid=0x409).
4. `$WnsSecretkey`: Nastavte tento toohello tajný klíč pro vás aplikace pro Windows z hello [Centrum vývojářů pro Windows](http://go.microsoft.com/fwlink/p/?linkid=266582&clcid=0x409).

Tyto proměnné jsou použité tooconnect tooyour obor názvů a vytvoření nového centra oznámení, které jsou nakonfigurované toohandle oznámení služby oznámení Windows (WNS) s přihlašovacími údaji WNS pro aplikace pro Windows. Informace o získání hello balíček SID a tajný klíč najdete hello [Začínáme s Notification Hubs](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) kurzu. 

* fragment skriptu Hello používá hello `NamespaceManager` objektu toocheck toosee Pokud hello Centrum oznámení se identifikovanou pomocí `$Path` existuje.
* Pokud neexistuje, dojde k vytvoření skriptu hello `NotificationHubDescription` s WNS přihlašovací údaje a předat tuto toohello `NamespaceManager` třída `CreateNotificationHub` metoda.

``` powershell

$Namespace = "<Enter your namespace>"
$Path  = "<Enter a name for your notification hub>"
$WnsPackageSid = "<your package sid>"
$WnsSecretkey = "<enter your secret key>"

$WnsCredential = New-Object -TypeName Microsoft.Azure.NotificationHubs.WnsCredential -ArgumentList $WnsPackageSid,$WnsSecretkey

# Query hello namespace
$CurrentNamespace = Get-AzureSBNamespace -Name $Namespace

# Check if hello namespace already exists
if ($CurrentNamespace)
{
    Write-Output "hello namespace [$Namespace] in hello [$($CurrentNamespace.Region)] region was found."

    # Create hello NamespaceManager object used toocreate a new notification hub
    $sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
    Write-Output "Creating a NamespaceManager object for hello [$Namespace] namespace..."
    $NamespaceManager = [Microsoft.Azure.NotificationHubs.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
    Write-Output "NamespaceManager object for hello [$Namespace] namespace has been successfully created."

    # Check toosee if hello Notification Hub already exists
    if ($NamespaceManager.NotificationHubExists($Path))
    {
        Write-Output "hello [$Path] notification hub already exists in hello [$Namespace] namespace."  
    }
    else
    {
        Write-Output "Creating hello [$Path] notification hub in hello [$Namespace] namespace."
        $NHDescription = New-Object -TypeName Microsoft.Azure.NotificationHubs.NotificationHubDescription -ArgumentList $Path;
        $NHDescription.WnsCredential = $WnsCredential;
        $NamespaceManager.CreateNotificationHub($NHDescription);
        Write-Output "hello [$Path] notification hub was created in hello [$Namespace] namespace."
    }
}
else
{
    Write-Host "hello [$Namespace] namespace does not exist."
}
```




## <a name="additional-resources"></a>Další zdroje
* [Správa služby Service Bus pomocí prostředí PowerShell](../service-bus-messaging/service-bus-powershell-how-to-provision.md)
* [Jak toocreate Service Bus fronty, témata a odběry pomocí skriptu prostředí PowerShell](http://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
* [Jak toocreate a Service Bus Namespace a Centrum událostí pomocí skriptu prostředí PowerShell](http://blogs.msdn.com/b/paolos/archive/2014/12/01/how-to-create-a-service-bus-namespace-and-an-event-hub-using-a-powershell-script.aspx)

Některé připravených skripty jsou také k dispozici ke stažení:

* [Skripty prostředí PowerShell služby Service Bus](https://code.msdn.microsoft.com/windowsazure/Service-Bus-PowerShell-a46b7059)

[možnostech nákupu]: http://azure.microsoft.com/pricing/purchase-options/
[nabízí člen]: http://azure.microsoft.com/pricing/member-offers/
[bezplatné zkušební verze]: http://azure.microsoft.com/pricing/free-trial/
[nainstalovat a nakonfigurovat Azure PowerShell]: /powershell/azureps-cmdlets-docs
[rozhraní API .NET pro centra oznámení]: https://msdn.microsoft.com/library/azure/mt414893.aspx
[Get-AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495122.aspx
[New-AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495165.aspx
[Get-AzureSBAuthorizationRule]: https://msdn.microsoft.com/library/azure/dn495113.aspx

