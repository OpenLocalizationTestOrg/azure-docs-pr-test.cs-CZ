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
# <a name="deploy-and-manage-notification-hubs-using-powershell"></a><span data-ttu-id="4b586-103">Nasazení a správa Notification Hubs pomocí PowerShellu</span><span class="sxs-lookup"><span data-stu-id="4b586-103">Deploy and Manage Notification Hubs using PowerShell</span></span>
## <a name="overview"></a><span data-ttu-id="4b586-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="4b586-104">Overview</span></span>
<span data-ttu-id="4b586-105">Tento článek ukazuje, jak toouse vytvořit a spravovat Azure Notification Hubs pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="4b586-105">This article shows you how toouse Create and Manage Azure Notification Hubs using PowerShell.</span></span> <span data-ttu-id="4b586-106">Hello následující běžné úlohy automatizace jsou uvedené v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="4b586-106">hello following common automation tasks are shown in this topic.</span></span>

* <span data-ttu-id="4b586-107">Vytvoření centra oznámení</span><span class="sxs-lookup"><span data-stu-id="4b586-107">Create a Notification Hub</span></span>
* <span data-ttu-id="4b586-108">Nastavení přihlašovacích údajů</span><span class="sxs-lookup"><span data-stu-id="4b586-108">Set Credentials</span></span>

<span data-ttu-id="4b586-109">Pokud pro vaše centra oznámení musíte taky toocreate nový obor názvů service bus, najdete v části [spravovat služby Service Bus pomocí prostředí PowerShell](../service-bus-messaging/service-bus-powershell-how-to-provision.md).</span><span class="sxs-lookup"><span data-stu-id="4b586-109">If you also need toocreate a new service bus namespace for your notification hubs, see [Manage Service Bus with PowerShell](../service-bus-messaging/service-bus-powershell-how-to-provision.md).</span></span>

<span data-ttu-id="4b586-110">Správa centra oznámení nepodporuje přímo hello rutiny zahrnuté v prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="4b586-110">Managing Notifications Hubs is not supported directly by hello cmdlets included with Azure PowerShell.</span></span> <span data-ttu-id="4b586-111">Nejlepším postupem Hello z prostředí PowerShell se tooreference hello Microsoft.Azure.NotificationHubs.dll sestavení.</span><span class="sxs-lookup"><span data-stu-id="4b586-111">hello best approach from PowerShell is tooreference hello Microsoft.Azure.NotificationHubs.dll assembly.</span></span> <span data-ttu-id="4b586-112">sestavení Hello je distribuován s hello [balíček Microsoft Azure Notification Hubs NuGet](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span><span class="sxs-lookup"><span data-stu-id="4b586-112">hello assembly is distributed with hello [Microsoft Azure Notification Hubs NuGet package](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4b586-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="4b586-113">Prerequisites</span></span>
<span data-ttu-id="4b586-114">Před zahájením tohoto článku, musíte mít následující hello:</span><span class="sxs-lookup"><span data-stu-id="4b586-114">Before you begin this article, you must have hello following:</span></span>

* <span data-ttu-id="4b586-115">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="4b586-115">An Azure subscription.</span></span> <span data-ttu-id="4b586-116">Azure je platforma, na základě předplatného.</span><span class="sxs-lookup"><span data-stu-id="4b586-116">Azure is a subscription-based platform.</span></span> <span data-ttu-id="4b586-117">Další informace o získání předplatného najdete v tématu [možnostech nákupu], [nabízí člen], nebo [bezplatné zkušební verze].</span><span class="sxs-lookup"><span data-stu-id="4b586-117">For more information about obtaining a subscription, see [Purchase Options], [Member Offers], or [Free Trial].</span></span>
* <span data-ttu-id="4b586-118">Počítač s prostředím Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="4b586-118">A computer with Azure PowerShell.</span></span> <span data-ttu-id="4b586-119">Pokyny najdete v tématu [nainstalovat a nakonfigurovat Azure PowerShell].</span><span class="sxs-lookup"><span data-stu-id="4b586-119">For instructions, see [Install and configure Azure PowerShell].</span></span>
* <span data-ttu-id="4b586-120">Obecné znalosti skriptů prostředí PowerShell, balíčků NuGet a hello rozhraní .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="4b586-120">A general understanding of PowerShell scripts, NuGet packages, and hello .NET Framework.</span></span>

## <a name="including-a-reference-toohello-net-assembly-for-service-bus"></a><span data-ttu-id="4b586-121">Včetně referenční sestavení .NET toohello pro Service Bus</span><span class="sxs-lookup"><span data-stu-id="4b586-121">Including a reference toohello .NET assembly for Service Bus</span></span>
<span data-ttu-id="4b586-122">Správa Azure Notification Hubs ještě není součástí hello rutiny prostředí PowerShell v prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="4b586-122">Managing Azure Notification Hubs is not yet included with hello PowerShell cmdlets in Azure PowerShell.</span></span> <span data-ttu-id="4b586-123">centra oznámení tooprovision, můžete použít klienta rozhraní .NET hello součástí hello [balíček Microsoft Azure Notification Hubs NuGet](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span><span class="sxs-lookup"><span data-stu-id="4b586-123">tooprovision notification hubs, you can use hello .NET client provided in hello [Microsoft Azure Notification Hubs NuGet package](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span></span>

<span data-ttu-id="4b586-124">Nejprve zkontrolujte, zda váš skript najdou hello **Microsoft.Azure.NotificationHubs.dll** sestavení, která je nainstalována jako balíčku NuGet v projektu sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4b586-124">First, make sure your script can locate hello **Microsoft.Azure.NotificationHubs.dll** assembly, which is installed as a NuGet package in a Visual Studio project.</span></span> <span data-ttu-id="4b586-125">V pořadí toobe flexibilní hello skript provede tyto kroky:</span><span class="sxs-lookup"><span data-stu-id="4b586-125">In order toobe flexible, hello script performs these steps:</span></span>

1. <span data-ttu-id="4b586-126">Určuje, kdy byl vyvolán cesta hello.</span><span class="sxs-lookup"><span data-stu-id="4b586-126">Determines hello path at which it was invoked.</span></span>
2. <span data-ttu-id="4b586-127">Traverses hello cestu, dokud nenajde složku s názvem `packages`.</span><span class="sxs-lookup"><span data-stu-id="4b586-127">Traverses hello path until it finds a folder named `packages`.</span></span> <span data-ttu-id="4b586-128">Tato složka se vytvoří při instalaci balíčků NuGet pro projekty v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4b586-128">This folder is created when you install NuGet packages for Visual Studio projects.</span></span>
3. <span data-ttu-id="4b586-129">Rekurzivní hledání hello `packages` složku pro sestavení s názvem **Microsoft.Azure.NotificationHubs.dll**.</span><span class="sxs-lookup"><span data-stu-id="4b586-129">Recursively searches hello `packages` folder for an assembly named **Microsoft.Azure.NotificationHubs.dll**.</span></span>
4. <span data-ttu-id="4b586-130">Odkazy na sestavení hello, takže hello typy jsou k dispozici pro pozdější použití.</span><span class="sxs-lookup"><span data-stu-id="4b586-130">References hello assembly so that hello types are available for later use.</span></span>

<span data-ttu-id="4b586-131">Zde je, jak tyto kroky jsou implementované ve skriptu prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="4b586-131">Here's how these steps are implemented in a PowerShell script:</span></span>

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

## <a name="create-hello-namespacemanager-class"></a><span data-ttu-id="4b586-132">Vytvořte třídu NamespaceManager hello</span><span class="sxs-lookup"><span data-stu-id="4b586-132">Create hello NamespaceManager class</span></span>
<span data-ttu-id="4b586-133">tooprovision Notification Hubs, vytvořte instanci hello [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.namespacemanager.aspx) třídy z hello SDK.</span><span class="sxs-lookup"><span data-stu-id="4b586-133">tooprovision Notification Hubs, create an instance of hello [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.namespacemanager.aspx) class from hello SDK.</span></span> 

<span data-ttu-id="4b586-134">Můžete použít hello [Get-AzureSBAuthorizationRule] rutiny zahrnuté do prostředí Azure PowerShell tooretrieve autorizační pravidlo, které se používá tooprovide připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="4b586-134">You can use hello [Get-AzureSBAuthorizationRule] cmdlet included with Azure PowerShell tooretrieve an authorization rule that's used tooprovide a connection string.</span></span> <span data-ttu-id="4b586-135">Uložíme odkaz toohello `NamespaceManager` instance v hello `$NamespaceManager` proměnné.</span><span class="sxs-lookup"><span data-stu-id="4b586-135">We'll store a reference toohello `NamespaceManager` instance in hello `$NamespaceManager` variable.</span></span> <span data-ttu-id="4b586-136">Budeme používat `$NamespaceManager` tooprovision centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="4b586-136">We will use `$NamespaceManager` tooprovision a notification hub.</span></span>

``` powershell
$sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
# Create hello NamespaceManager object toocreate hello hub
Write-Output "Creating a NamespaceManager object for hello [$Namespace] namespace..."
$NamespaceManager=[Microsoft.Azure.NotificationHubs.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
Write-Output "NamespaceManager object for hello [$Namespace] namespace has been successfully created."
```


## <a name="provisioning-a-new-notification-hub"></a><span data-ttu-id="4b586-137">Zřizování nového centra oznámení</span><span class="sxs-lookup"><span data-stu-id="4b586-137">Provisioning a new Notification Hub</span></span>
<span data-ttu-id="4b586-138">tooprovision nového centra oznámení, používat hello [rozhraní API .NET pro centra oznámení].</span><span class="sxs-lookup"><span data-stu-id="4b586-138">tooprovision a new notification hub, use hello [.NET API for Notification Hubs].</span></span>

<span data-ttu-id="4b586-139">V této části hello skriptu nastavíte čtyři lokální proměnné.</span><span class="sxs-lookup"><span data-stu-id="4b586-139">In this part of hello script you set up four local variables.</span></span> 

1. <span data-ttu-id="4b586-140">`$Namespace`: Nastavte tento toohello název oboru názvů hello místo toocreate centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="4b586-140">`$Namespace` : Set this toohello name of hello namespace where you want toocreate a notification hub.</span></span>
2. <span data-ttu-id="4b586-141">`$Path`: Nastavte tento název cesty toohello hello nového centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="4b586-141">`$Path` : Set this path toohello name of hello new notification hub.</span></span>  <span data-ttu-id="4b586-142">Například "MyHub".</span><span class="sxs-lookup"><span data-stu-id="4b586-142">For example, "MyHub".</span></span>    
3. <span data-ttu-id="4b586-143">`$WnsPackageSid`: Nastavte tento balíček toohello SID pro vás aplikace pro Windows z hello [Centrum vývojářů pro Windows](http://go.microsoft.com/fwlink/p/?linkid=266582&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="4b586-143">`$WnsPackageSid` : Set this toohello package SID for you Windows App from hello [Windows Dev Center](http://go.microsoft.com/fwlink/p/?linkid=266582&clcid=0x409).</span></span>
4. <span data-ttu-id="4b586-144">`$WnsSecretkey`: Nastavte tento toohello tajný klíč pro vás aplikace pro Windows z hello [Centrum vývojářů pro Windows](http://go.microsoft.com/fwlink/p/?linkid=266582&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="4b586-144">`$WnsSecretkey`: Set this toohello secret key for you Windows App from hello [Windows Dev Center](http://go.microsoft.com/fwlink/p/?linkid=266582&clcid=0x409).</span></span>

<span data-ttu-id="4b586-145">Tyto proměnné jsou použité tooconnect tooyour obor názvů a vytvoření nového centra oznámení, které jsou nakonfigurované toohandle oznámení služby oznámení Windows (WNS) s přihlašovacími údaji WNS pro aplikace pro Windows.</span><span class="sxs-lookup"><span data-stu-id="4b586-145">These variables are used tooconnect tooyour namespace and create a new Notification Hub configured toohandle Windows Notification Services (WNS) notifications with WNS credentials for a Windows App.</span></span> <span data-ttu-id="4b586-146">Informace o získání hello balíček SID a tajný klíč najdete hello [Začínáme s Notification Hubs](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="4b586-146">For information on obtaining hello package SID and secret key see, hello [Getting Started with Notification Hubs](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) tutorial.</span></span> 

* <span data-ttu-id="4b586-147">fragment skriptu Hello používá hello `NamespaceManager` objektu toocheck toosee Pokud hello Centrum oznámení se identifikovanou pomocí `$Path` existuje.</span><span class="sxs-lookup"><span data-stu-id="4b586-147">hello script snippet uses hello `NamespaceManager` object toocheck toosee if hello Notification Hub identified by `$Path` exists.</span></span>
* <span data-ttu-id="4b586-148">Pokud neexistuje, dojde k vytvoření skriptu hello `NotificationHubDescription` s WNS přihlašovací údaje a předat tuto toohello `NamespaceManager` třída `CreateNotificationHub` metoda.</span><span class="sxs-lookup"><span data-stu-id="4b586-148">If it does not exist, hello script will create an `NotificationHubDescription` with WNS credentials and pass that toohello `NamespaceManager` class `CreateNotificationHub` method.</span></span>

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




## <a name="additional-resources"></a><span data-ttu-id="4b586-149">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="4b586-149">Additional Resources</span></span>
* [<span data-ttu-id="4b586-150">Správa služby Service Bus pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="4b586-150">Manage Service Bus with PowerShell</span></span>](../service-bus-messaging/service-bus-powershell-how-to-provision.md)
* [<span data-ttu-id="4b586-151">Jak toocreate Service Bus fronty, témata a odběry pomocí skriptu prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="4b586-151">How toocreate Service Bus queues, topics and subscriptions using a PowerShell script</span></span>](http://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
* [<span data-ttu-id="4b586-152">Jak toocreate a Service Bus Namespace a Centrum událostí pomocí skriptu prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="4b586-152">How toocreate a Service Bus Namespace and an Event Hub using a PowerShell script</span></span>](http://blogs.msdn.com/b/paolos/archive/2014/12/01/how-to-create-a-service-bus-namespace-and-an-event-hub-using-a-powershell-script.aspx)

<span data-ttu-id="4b586-153">Některé připravených skripty jsou také k dispozici ke stažení:</span><span class="sxs-lookup"><span data-stu-id="4b586-153">Some ready-made scripts are also available for download:</span></span>

* [<span data-ttu-id="4b586-154">Skripty prostředí PowerShell služby Service Bus</span><span class="sxs-lookup"><span data-stu-id="4b586-154">Service Bus PowerShell Scripts</span></span>](https://code.msdn.microsoft.com/windowsazure/Service-Bus-PowerShell-a46b7059)

[možnostech nákupu]: http://azure.microsoft.com/pricing/purchase-options/
[nabízí člen]: http://azure.microsoft.com/pricing/member-offers/
[bezplatné zkušební verze]: http://azure.microsoft.com/pricing/free-trial/
[nainstalovat a nakonfigurovat Azure PowerShell]: /powershell/azureps-cmdlets-docs
[rozhraní API .NET pro centra oznámení]: https://msdn.microsoft.com/library/azure/mt414893.aspx
[Get-AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495122.aspx
[New-AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495165.aspx
[Get-AzureSBAuthorizationRule]: https://msdn.microsoft.com/library/azure/dn495113.aspx

