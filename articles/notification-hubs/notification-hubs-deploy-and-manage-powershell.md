---
title: "Nasazení a správa Notification Hubs pomocí PowerShellu"
description: "Jak vytvořit a spravovat pomocí prostředí PowerShell pro automatizaci centra oznámení"
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
ms.openlocfilehash: 4db058e4bd91dc287b14e887abc6c378c65c4a2b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-and-manage-notification-hubs-using-powershell"></a><span data-ttu-id="e6ca8-103">Nasazení a správa Notification Hubs pomocí PowerShellu</span><span class="sxs-lookup"><span data-stu-id="e6ca8-103">Deploy and Manage Notification Hubs using PowerShell</span></span>
## <a name="overview"></a><span data-ttu-id="e6ca8-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="e6ca8-104">Overview</span></span>
<span data-ttu-id="e6ca8-105">Tento článek ukazuje, jak používat vytvořit a spravovat Azure Notification Hubs pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e6ca8-105">This article shows you how to use Create and Manage Azure Notification Hubs using PowerShell.</span></span> <span data-ttu-id="e6ca8-106">V tomto tématu jsou uvedeny následující běžné úlohy automatizace.</span><span class="sxs-lookup"><span data-stu-id="e6ca8-106">The following common automation tasks are shown in this topic.</span></span>

* <span data-ttu-id="e6ca8-107">Vytvoření centra oznámení</span><span class="sxs-lookup"><span data-stu-id="e6ca8-107">Create a Notification Hub</span></span>
* <span data-ttu-id="e6ca8-108">Nastavení přihlašovacích údajů</span><span class="sxs-lookup"><span data-stu-id="e6ca8-108">Set Credentials</span></span>

<span data-ttu-id="e6ca8-109">Pokud potřebujete vytvořit nový obor názvů sběrnice služby pro vaše centra oznámení, přečtěte si téma [spravovat služby Service Bus pomocí prostředí PowerShell](../service-bus-messaging/service-bus-powershell-how-to-provision.md).</span><span class="sxs-lookup"><span data-stu-id="e6ca8-109">If you also need to create a new service bus namespace for your notification hubs, see [Manage Service Bus with PowerShell](../service-bus-messaging/service-bus-powershell-how-to-provision.md).</span></span>

<span data-ttu-id="e6ca8-110">Správa centra oznámení není podporována přímo pomocí rutin obsažených v prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e6ca8-110">Managing Notifications Hubs is not supported directly by the cmdlets included with Azure PowerShell.</span></span> <span data-ttu-id="e6ca8-111">Nejlepším postupem z prostředí PowerShell se tak, aby odkazovaly Microsoft.Azure.NotificationHubs.dll sestavení.</span><span class="sxs-lookup"><span data-stu-id="e6ca8-111">The best approach from PowerShell is to reference the Microsoft.Azure.NotificationHubs.dll assembly.</span></span> <span data-ttu-id="e6ca8-112">Sestavení je distribuován s [balíček Microsoft Azure Notification Hubs NuGet](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span><span class="sxs-lookup"><span data-stu-id="e6ca8-112">The assembly is distributed with the [Microsoft Azure Notification Hubs NuGet package](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e6ca8-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e6ca8-113">Prerequisites</span></span>
<span data-ttu-id="e6ca8-114">Je nutné, abyste před zahájením tohoto článku měli tyto položky:</span><span class="sxs-lookup"><span data-stu-id="e6ca8-114">Before you begin this article, you must have the following:</span></span>

* <span data-ttu-id="e6ca8-115">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="e6ca8-115">An Azure subscription.</span></span> <span data-ttu-id="e6ca8-116">Azure je platforma, na základě předplatného.</span><span class="sxs-lookup"><span data-stu-id="e6ca8-116">Azure is a subscription-based platform.</span></span> <span data-ttu-id="e6ca8-117">Další informace o získání předplatného najdete v tématu [možnostech nákupu], [nabízí člen], nebo [bezplatné zkušební verze].</span><span class="sxs-lookup"><span data-stu-id="e6ca8-117">For more information about obtaining a subscription, see [Purchase Options], [Member Offers], or [Free Trial].</span></span>
* <span data-ttu-id="e6ca8-118">Počítač s prostředím Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e6ca8-118">A computer with Azure PowerShell.</span></span> <span data-ttu-id="e6ca8-119">Pokyny najdete v tématu [nainstalovat a nakonfigurovat Azure PowerShell].</span><span class="sxs-lookup"><span data-stu-id="e6ca8-119">For instructions, see [Install and configure Azure PowerShell].</span></span>
* <span data-ttu-id="e6ca8-120">Obecné znalosti skriptů prostředí PowerShell, balíčků NuGet a rozhraní .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="e6ca8-120">A general understanding of PowerShell scripts, NuGet packages, and the .NET Framework.</span></span>

## <a name="including-a-reference-to-the-net-assembly-for-service-bus"></a><span data-ttu-id="e6ca8-121">Včetně odkaz na sestavení rozhraní .NET pro Service Bus</span><span class="sxs-lookup"><span data-stu-id="e6ca8-121">Including a reference to the .NET assembly for Service Bus</span></span>
<span data-ttu-id="e6ca8-122">Správa Azure Notification Hubs ještě není součástí rutin prostředí PowerShell v prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e6ca8-122">Managing Azure Notification Hubs is not yet included with the PowerShell cmdlets in Azure PowerShell.</span></span> <span data-ttu-id="e6ca8-123">Pokud chcete zřídit centra oznámení, můžete použít klient .NET, které jsou součástí [balíček Microsoft Azure Notification Hubs NuGet](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span><span class="sxs-lookup"><span data-stu-id="e6ca8-123">To provision notification hubs, you can use the .NET client provided in the [Microsoft Azure Notification Hubs NuGet package](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span></span>

<span data-ttu-id="e6ca8-124">Nejprve zkontrolujte, zda váš skript můžete najít **Microsoft.Azure.NotificationHubs.dll** sestavení, která je nainstalována jako balíčku NuGet v projektu sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e6ca8-124">First, make sure your script can locate the **Microsoft.Azure.NotificationHubs.dll** assembly, which is installed as a NuGet package in a Visual Studio project.</span></span> <span data-ttu-id="e6ca8-125">Chcete-li být flexibilní, skript provede tyto kroky:</span><span class="sxs-lookup"><span data-stu-id="e6ca8-125">In order to be flexible, the script performs these steps:</span></span>

1. <span data-ttu-id="e6ca8-126">Určuje cestu, kde byla vyvolána.</span><span class="sxs-lookup"><span data-stu-id="e6ca8-126">Determines the path at which it was invoked.</span></span>
2. <span data-ttu-id="e6ca8-127">Prochází cestu, dokud nenajde složku s názvem `packages`.</span><span class="sxs-lookup"><span data-stu-id="e6ca8-127">Traverses the path until it finds a folder named `packages`.</span></span> <span data-ttu-id="e6ca8-128">Tato složka se vytvoří při instalaci balíčků NuGet pro projekty v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e6ca8-128">This folder is created when you install NuGet packages for Visual Studio projects.</span></span>
3. <span data-ttu-id="e6ca8-129">Rekurzivní hledání `packages` složku pro sestavení s názvem **Microsoft.Azure.NotificationHubs.dll**.</span><span class="sxs-lookup"><span data-stu-id="e6ca8-129">Recursively searches the `packages` folder for an assembly named **Microsoft.Azure.NotificationHubs.dll**.</span></span>
4. <span data-ttu-id="e6ca8-130">Odkazuje na sestavení tak, aby typy jsou k dispozici pro pozdější použití.</span><span class="sxs-lookup"><span data-stu-id="e6ca8-130">References the assembly so that the types are available for later use.</span></span>

<span data-ttu-id="e6ca8-131">Zde je, jak tyto kroky jsou implementované ve skriptu prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="e6ca8-131">Here's how these steps are implemented in a PowerShell script:</span></span>

``` powershell

try
{
    # WARNING: Make sure to reference the latest version of Microsoft.Azure.NotificationHubs.dll
    Write-Output "Adding the [Microsoft.Azure.NotificationHubs.dll] assembly to the script..."
    $scriptPath = Split-Path (Get-Variable MyInvocation -Scope 0).Value.MyCommand.Path
    $packagesFolder = (Split-Path $scriptPath -Parent) + "\packages"
    $assembly = Get-ChildItem $packagesFolder -Include "Microsoft.Azure.NotificationHubs.dll" -Recurse
    Add-Type -Path $assembly.FullName

    Write-Output "The [Microsoft.Azure.NotificationHubs.dll] assembly has been successfully added to the script."
}

catch [System.Exception]
{
    Write-Error("Could not add the Microsoft.Azure.NotificationHubs.dll assembly to the script. Make sure you build the solution before running the provisioning script.")
}
```

## <a name="create-the-namespacemanager-class"></a><span data-ttu-id="e6ca8-132">Vytvořte třídu NamespaceManager</span><span class="sxs-lookup"><span data-stu-id="e6ca8-132">Create the NamespaceManager class</span></span>
<span data-ttu-id="e6ca8-133">Pokud chcete zřídit Notification Hubs, vytvořte instanci [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.namespacemanager.aspx) třída ze sady SDK.</span><span class="sxs-lookup"><span data-stu-id="e6ca8-133">To provision Notification Hubs, create an instance of the [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.namespacemanager.aspx) class from the SDK.</span></span> 

<span data-ttu-id="e6ca8-134">Můžete použít [Get-AzureSBAuthorizationRule] rutiny zahrnuté do prostředí Azure PowerShell k načtení autorizační pravidlo, které slouží k poskytování připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="e6ca8-134">You can use the [Get-AzureSBAuthorizationRule] cmdlet included with Azure PowerShell to retrieve an authorization rule that's used to provide a connection string.</span></span> <span data-ttu-id="e6ca8-135">Odkaz na uložíme `NamespaceManager` instance v `$NamespaceManager` proměnné.</span><span class="sxs-lookup"><span data-stu-id="e6ca8-135">We'll store a reference to the `NamespaceManager` instance in the `$NamespaceManager` variable.</span></span> <span data-ttu-id="e6ca8-136">Budeme používat `$NamespaceManager` ke zřízení centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="e6ca8-136">We will use `$NamespaceManager` to provision a notification hub.</span></span>

``` powershell
$sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
# Create the NamespaceManager object to create the hub
Write-Output "Creating a NamespaceManager object for the [$Namespace] namespace..."
$NamespaceManager=[Microsoft.Azure.NotificationHubs.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
Write-Output "NamespaceManager object for the [$Namespace] namespace has been successfully created."
```


## <a name="provisioning-a-new-notification-hub"></a><span data-ttu-id="e6ca8-137">Zřizování nového centra oznámení</span><span class="sxs-lookup"><span data-stu-id="e6ca8-137">Provisioning a new Notification Hub</span></span>
<span data-ttu-id="e6ca8-138">Chcete-li zřídit nového centra oznámení, použijte [rozhraní API .NET pro centra oznámení].</span><span class="sxs-lookup"><span data-stu-id="e6ca8-138">To provision a new notification hub, use the [.NET API for Notification Hubs].</span></span>

<span data-ttu-id="e6ca8-139">V této části skriptu nastavíte čtyři lokální proměnné.</span><span class="sxs-lookup"><span data-stu-id="e6ca8-139">In this part of the script you set up four local variables.</span></span> 

1. <span data-ttu-id="e6ca8-140">`$Namespace`: Nastavením název oboru názvů, kde chcete vytvořit centrum oznámení.</span><span class="sxs-lookup"><span data-stu-id="e6ca8-140">`$Namespace` : Set this to the name of the namespace where you want to create a notification hub.</span></span>
2. <span data-ttu-id="e6ca8-141">`$Path`: Nastavte tuto cestu na název nového centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="e6ca8-141">`$Path` : Set this path to the name of the new notification hub.</span></span>  <span data-ttu-id="e6ca8-142">Například "MyHub".</span><span class="sxs-lookup"><span data-stu-id="e6ca8-142">For example, "MyHub".</span></span>    
3. <span data-ttu-id="e6ca8-143">`$WnsPackageSid`: Tuto možnost nastavíte na identifikátor SID balíčku pro vás aplikace pro Windows z [Centrum vývojářů pro Windows](http://go.microsoft.com/fwlink/p/?linkid=266582&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="e6ca8-143">`$WnsPackageSid` : Set this to the package SID for you Windows App from the [Windows Dev Center](http://go.microsoft.com/fwlink/p/?linkid=266582&clcid=0x409).</span></span>
4. <span data-ttu-id="e6ca8-144">`$WnsSecretkey`: Tuto možnost nastavíte na tajný klíč pro vás aplikace pro Windows z [Centrum vývojářů pro Windows](http://go.microsoft.com/fwlink/p/?linkid=266582&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="e6ca8-144">`$WnsSecretkey`: Set this to the secret key for you Windows App from the [Windows Dev Center](http://go.microsoft.com/fwlink/p/?linkid=266582&clcid=0x409).</span></span>

<span data-ttu-id="e6ca8-145">Tyto proměnné se používají k připojení k oboru názvů a vytvoření nového centra oznámení pro zpracování oznámení služby oznámení Windows (WNS) s přihlašovacími údaji WNS pro aplikaci systému Windows nakonfigurována.</span><span class="sxs-lookup"><span data-stu-id="e6ca8-145">These variables are used to connect to your namespace and create a new Notification Hub configured to handle Windows Notification Services (WNS) notifications with WNS credentials for a Windows App.</span></span> <span data-ttu-id="e6ca8-146">Informace o získání balíčku SID a tajný klíč najdete [Začínáme s Notification Hubs](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="e6ca8-146">For information on obtaining the package SID and secret key see, the [Getting Started with Notification Hubs](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) tutorial.</span></span> 

* <span data-ttu-id="e6ca8-147">Fragment skriptu používá `NamespaceManager` objekt, který chcete zkontrolovat, pokud centra oznámení identifikovaný `$Path` existuje.</span><span class="sxs-lookup"><span data-stu-id="e6ca8-147">The script snippet uses the `NamespaceManager` object to check to see if the Notification Hub identified by `$Path` exists.</span></span>
* <span data-ttu-id="e6ca8-148">Pokud neexistuje, vytvoří skript `NotificationHubDescription` s WNS přihlašovací údaje a předejte to `NamespaceManager` třída `CreateNotificationHub` metoda.</span><span class="sxs-lookup"><span data-stu-id="e6ca8-148">If it does not exist, the script will create an `NotificationHubDescription` with WNS credentials and pass that to the `NamespaceManager` class `CreateNotificationHub` method.</span></span>

``` powershell

$Namespace = "<Enter your namespace>"
$Path  = "<Enter a name for your notification hub>"
$WnsPackageSid = "<your package sid>"
$WnsSecretkey = "<enter your secret key>"

$WnsCredential = New-Object -TypeName Microsoft.Azure.NotificationHubs.WnsCredential -ArgumentList $WnsPackageSid,$WnsSecretkey

# Query the namespace
$CurrentNamespace = Get-AzureSBNamespace -Name $Namespace

# Check if the namespace already exists
if ($CurrentNamespace)
{
    Write-Output "The namespace [$Namespace] in the [$($CurrentNamespace.Region)] region was found."

    # Create the NamespaceManager object used to create a new notification hub
    $sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
    Write-Output "Creating a NamespaceManager object for the [$Namespace] namespace..."
    $NamespaceManager = [Microsoft.Azure.NotificationHubs.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
    Write-Output "NamespaceManager object for the [$Namespace] namespace has been successfully created."

    # Check to see if the Notification Hub already exists
    if ($NamespaceManager.NotificationHubExists($Path))
    {
        Write-Output "The [$Path] notification hub already exists in the [$Namespace] namespace."  
    }
    else
    {
        Write-Output "Creating the [$Path] notification hub in the [$Namespace] namespace."
        $NHDescription = New-Object -TypeName Microsoft.Azure.NotificationHubs.NotificationHubDescription -ArgumentList $Path;
        $NHDescription.WnsCredential = $WnsCredential;
        $NamespaceManager.CreateNotificationHub($NHDescription);
        Write-Output "The [$Path] notification hub was created in the [$Namespace] namespace."
    }
}
else
{
    Write-Host "The [$Namespace] namespace does not exist."
}
```




## <a name="additional-resources"></a><span data-ttu-id="e6ca8-149">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="e6ca8-149">Additional Resources</span></span>
* [<span data-ttu-id="e6ca8-150">Správa služby Service Bus pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="e6ca8-150">Manage Service Bus with PowerShell</span></span>](../service-bus-messaging/service-bus-powershell-how-to-provision.md)
* [<span data-ttu-id="e6ca8-151">Postup vytvoření fronty, témata a odběry pomocí skriptu prostředí PowerShell služby Service Bus</span><span class="sxs-lookup"><span data-stu-id="e6ca8-151">How to create Service Bus queues, topics and subscriptions using a PowerShell script</span></span>](http://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
* [<span data-ttu-id="e6ca8-152">Postup vytvoření Namespace Service Bus a centra událostí pomocí skriptu prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="e6ca8-152">How to create a Service Bus Namespace and an Event Hub using a PowerShell script</span></span>](http://blogs.msdn.com/b/paolos/archive/2014/12/01/how-to-create-a-service-bus-namespace-and-an-event-hub-using-a-powershell-script.aspx)

<span data-ttu-id="e6ca8-153">Některé připravených skripty jsou také k dispozici ke stažení:</span><span class="sxs-lookup"><span data-stu-id="e6ca8-153">Some ready-made scripts are also available for download:</span></span>

* [<span data-ttu-id="e6ca8-154">Skripty prostředí PowerShell služby Service Bus</span><span class="sxs-lookup"><span data-stu-id="e6ca8-154">Service Bus PowerShell Scripts</span></span>](https://code.msdn.microsoft.com/windowsazure/Service-Bus-PowerShell-a46b7059)

<span data-ttu-id="e6ca8-155">[možnostech nákupu]: http://azure.microsoft.com/pricing/purchase-options/</span><span class="sxs-lookup"><span data-stu-id="e6ca8-155">[Purchase Options]: http://azure.microsoft.com/pricing/purchase-options/</span></span>
<span data-ttu-id="e6ca8-156">[nabízí člen]: http://azure.microsoft.com/pricing/member-offers/</span><span class="sxs-lookup"><span data-stu-id="e6ca8-156">[Member Offers]: http://azure.microsoft.com/pricing/member-offers/</span></span>
<span data-ttu-id="e6ca8-157">[bezplatné zkušební verze]: http://azure.microsoft.com/pricing/free-trial/</span><span class="sxs-lookup"><span data-stu-id="e6ca8-157">[Free Trial]: http://azure.microsoft.com/pricing/free-trial/</span></span>
<span data-ttu-id="e6ca8-158">[nainstalovat a nakonfigurovat Azure PowerShell]: /powershell/azureps-cmdlets-docs</span><span class="sxs-lookup"><span data-stu-id="e6ca8-158">[Install and configure Azure PowerShell]: /powershell/azureps-cmdlets-docs</span></span>
<span data-ttu-id="e6ca8-159">[rozhraní API .NET pro centra oznámení]: https://msdn.microsoft.com/library/azure/mt414893.aspx</span><span class="sxs-lookup"><span data-stu-id="e6ca8-159">[.NET API for Notification Hubs]: https://msdn.microsoft.com/library/azure/mt414893.aspx</span></span>
[Get-AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495122.aspx
[New-AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495165.aspx
<span data-ttu-id="e6ca8-160">[Get-AzureSBAuthorizationRule]: https://msdn.microsoft.com/library/azure/dn495113.aspx</span><span class="sxs-lookup"><span data-stu-id="e6ca8-160">[Get-AzureSBAuthorizationRule]: https://msdn.microsoft.com/library/azure/dn495113.aspx</span></span>

