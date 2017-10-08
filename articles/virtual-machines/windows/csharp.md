---
title: "aaaCreate a spravovat služby Azure virtuálního počítače pomocí jazyka C# | Microsoft Docs"
description: "Virtuální počítač a všechny její Podpůrné prostředky, pomocí toodeploy C# a Azure Resource Manager."
services: virtual-machines-windows
documentationcenter: 
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 87524373-5f52-4f4b-94af-50bf7b65c277
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: davidmu
ms.openlocfilehash: 8beeabde731bbaa25e68d2b9c5abbf71acbe377f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-windows-vms-in-azure-using-c"></a><span data-ttu-id="6d4ed-103">Vytvářet a spravovat virtuální počítače Windows v Azure pomocí jazyka C#</span><span class="sxs-lookup"><span data-stu-id="6d4ed-103">Create and manage Windows VMs in Azure using C#</span></span> #

<span data-ttu-id="6d4ed-104">[Virtuální počítač Azure](overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (VM) vyžaduje několik podpůrných prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="6d4ed-104">An [Azure Virtual Machine](overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (VM) needs several supporting Azure resources.</span></span> <span data-ttu-id="6d4ed-105">Tento článek popisuje vytváření, správu a odstraňování prostředky virtuálních počítačů pomocí jazyka C#.</span><span class="sxs-lookup"><span data-stu-id="6d4ed-105">This article covers creating, managing, and deleting VM resources using C#.</span></span> <span data-ttu-id="6d4ed-106">Získáte informace o těchto tématech:</span><span class="sxs-lookup"><span data-stu-id="6d4ed-106">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="6d4ed-107">Vytvoření projektu ve Visual Studiu</span><span class="sxs-lookup"><span data-stu-id="6d4ed-107">Create a Visual Studio project</span></span>
> * <span data-ttu-id="6d4ed-108">Instalovat balíček hello</span><span class="sxs-lookup"><span data-stu-id="6d4ed-108">Install hello package</span></span>
> * <span data-ttu-id="6d4ed-109">Vytvořit přihlašovací údaje</span><span class="sxs-lookup"><span data-stu-id="6d4ed-109">Create credentials</span></span>
> * <span data-ttu-id="6d4ed-110">Vytvoření prostředků</span><span class="sxs-lookup"><span data-stu-id="6d4ed-110">Create resources</span></span>
> * <span data-ttu-id="6d4ed-111">Provádění úloh správy</span><span class="sxs-lookup"><span data-stu-id="6d4ed-111">Perform management tasks</span></span>
> * <span data-ttu-id="6d4ed-112">Odstraňte prostředky</span><span class="sxs-lookup"><span data-stu-id="6d4ed-112">Delete resources</span></span>
> * <span data-ttu-id="6d4ed-113">Spuštění aplikace hello</span><span class="sxs-lookup"><span data-stu-id="6d4ed-113">Run hello application</span></span>

<span data-ttu-id="6d4ed-114">Trvá přibližně 20 minut toodo tyto kroky.</span><span class="sxs-lookup"><span data-stu-id="6d4ed-114">It takes about 20 minutes toodo these steps.</span></span>

## <a name="create-a-visual-studio-project"></a><span data-ttu-id="6d4ed-115">Vytvoření projektu ve Visual Studiu</span><span class="sxs-lookup"><span data-stu-id="6d4ed-115">Create a Visual Studio project</span></span>

1. <span data-ttu-id="6d4ed-116">Pokud jste to ještě neudělali, nainstalujte [Visual Studio](https://docs.microsoft.com/visualstudio/install/install-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="6d4ed-116">If you haven't already, install [Visual Studio](https://docs.microsoft.com/visualstudio/install/install-visual-studio).</span></span> <span data-ttu-id="6d4ed-117">Vyberte **vývoj aplikací .NET** na hello zatížení stránky a potom klikněte na **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="6d4ed-117">Select **.NET desktop development** on hello Workloads page, and then click **Install**.</span></span> <span data-ttu-id="6d4ed-118">V hello souhrn, můžete uvidíte, že **nástrojů pro vývoj řešení pro rozhraní .NET Framework 4 4.6** je automaticky vybrána pro vás.</span><span class="sxs-lookup"><span data-stu-id="6d4ed-118">In hello summary, you can see that **.NET Framework 4 - 4.6 development tools** is automatically selected for you.</span></span> <span data-ttu-id="6d4ed-119">Pokud jste již nainstalovali Visual Studio, můžete přidat hello .NET zatížení pomocí hello Spouštěče Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6d4ed-119">If you have already installed Visual Studio, you can add hello .NET workload using hello Visual Studio Launcher.</span></span>
2. <span data-ttu-id="6d4ed-120">V sadě Visual Studio, klikněte na tlačítko **soubor** > **nový** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="6d4ed-120">In Visual Studio, click **File** > **New** > **Project**.</span></span>
3. <span data-ttu-id="6d4ed-121">V **šablony** > **Visual C#**, vyberte **konzolovou aplikaci (rozhraní .NET Framework)**, zadejte *myDotnetProject* pro název hello Hello projekt, vyberte hello umístění projektu hello a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="6d4ed-121">In **Templates** > **Visual C#**, select **Console App (.NET Framework)**, enter *myDotnetProject* for hello name of hello project, select hello location of hello project, and then click **OK**.</span></span>

## <a name="install-hello-package"></a><span data-ttu-id="6d4ed-122">Instalovat balíček hello</span><span class="sxs-lookup"><span data-stu-id="6d4ed-122">Install hello package</span></span>

<span data-ttu-id="6d4ed-123">Balíčky NuGet jsou hello nejjednodušší způsob, jak tooinstall hello knihovny, je nutné toofinish tyto kroky.</span><span class="sxs-lookup"><span data-stu-id="6d4ed-123">NuGet packages are hello easiest way tooinstall hello libraries that you need toofinish these steps.</span></span> <span data-ttu-id="6d4ed-124">tooget hello knihovny, které je nutné v sadě Visual Studio, proveďte tyto kroky:</span><span class="sxs-lookup"><span data-stu-id="6d4ed-124">tooget hello libraries that you need in Visual Studio, do these steps:</span></span>

1. <span data-ttu-id="6d4ed-125">Klikněte na tlačítko **nástroje** > **Správce balíčků Nuget**a potom klikněte na **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="6d4ed-125">Click **Tools** > **Nuget Package Manager**, and then click **Package Manager Console**.</span></span>
2. <span data-ttu-id="6d4ed-126">V konzole hello zadejte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="6d4ed-126">Type this command in hello console:</span></span>

    ```
    Install-Package Microsoft.Azure.Management.Fluent
    ```

## <a name="create-credentials"></a><span data-ttu-id="6d4ed-127">Vytvořit přihlašovací údaje</span><span class="sxs-lookup"><span data-stu-id="6d4ed-127">Create credentials</span></span>

<span data-ttu-id="6d4ed-128">Než začnete tento krok, ujistěte se, zda máte přístup tooan [objektu služby Active Directory](../../azure-resource-manager/resource-group-create-service-principal-portal.md).</span><span class="sxs-lookup"><span data-stu-id="6d4ed-128">Before you start this step, make sure that you have access tooan [Active Directory service principal](../../azure-resource-manager/resource-group-create-service-principal-portal.md).</span></span> <span data-ttu-id="6d4ed-129">Také byste měli zaznamenat ID aplikace hello hello ověřovací klíč a hello ID klienta, který budete potřebovat později.</span><span class="sxs-lookup"><span data-stu-id="6d4ed-129">You should also record hello application ID, hello authentication key, and hello tenant ID that you need in a later step.</span></span>

### <a name="create-hello-authorization-file"></a><span data-ttu-id="6d4ed-130">Vytvoření souboru autorizace hello</span><span class="sxs-lookup"><span data-stu-id="6d4ed-130">Create hello authorization file</span></span>

1. <span data-ttu-id="6d4ed-131">V Průzkumníku řešení klikněte pravým tlačítkem na *myDotnetProject* > **přidat** > **nová položka**a potom vyberte **textový soubor** v *Visual C# položky*.</span><span class="sxs-lookup"><span data-stu-id="6d4ed-131">In Solution Explorer, right-click *myDotnetProject* > **Add** > **New Item**, and then select **Text File** in *Visual C# Items*.</span></span> <span data-ttu-id="6d4ed-132">Název souboru hello *azureauth.properties*a potom klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="6d4ed-132">Name hello file *azureauth.properties*, and then click **Add**.</span></span>
2. <span data-ttu-id="6d4ed-133">Přidejte tyto vlastnosti autorizace:</span><span class="sxs-lookup"><span data-stu-id="6d4ed-133">Add these authorization properties:</span></span>

    ```
    subscription=<subscription-id>
    client=<application-id>
    key=<authentication-key>
    tenant=<tenant-id>
    managementURI=https://management.core.windows.net/
    baseURL=https://management.azure.com/
    authURL=https://login.windows.net/
    graphURL=https://graph.windows.net/
    ```

    <span data-ttu-id="6d4ed-134">Nahraďte  **&lt;id předplatného&gt;**  s ID vašeho předplatného  **&lt;id aplikace&gt;**  s hello aplikace Active Directory identifikátor,  **&lt;ověřovací klíč&gt;**  klíčem aplikace hello a  **&lt;id klienta&gt;**  s klientem hello identifikátor.</span><span class="sxs-lookup"><span data-stu-id="6d4ed-134">Replace **&lt;subscription-id&gt;** with your subscription identifier, **&lt;application-id&gt;** with hello Active Directory application identifier, **&lt;authentication-key&gt;** with hello application key, and **&lt;tenant-id&gt;** with hello tenant identifier.</span></span>

3. <span data-ttu-id="6d4ed-135">Uložte soubor azureauth.properties hello.</span><span class="sxs-lookup"><span data-stu-id="6d4ed-135">Save hello azureauth.properties file.</span></span> 
4. <span data-ttu-id="6d4ed-136">Nastavte proměnnou prostředí v systému Windows s názvem AZURE_AUTH_LOCATION s hello úplná cesta tooauthorization soubor, který jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="6d4ed-136">Set an environment variable in Windows named AZURE_AUTH_LOCATION with hello full path tooauthorization file that you created.</span></span> <span data-ttu-id="6d4ed-137">Například můžete použít hello následující příkaz prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="6d4ed-137">For example, hello following PowerShell command can be used:</span></span>

    ```
    [Environment]::SetEnvironmentVariable("AZURE_AUTH_LOCATION", "C:\Visual Studio 2017\Projects\myDotnetProject\myDotnetProject\azureauth.properties", "User")
    ```

### <a name="create-hello-management-client"></a><span data-ttu-id="6d4ed-138">Vytvoření klienta pro správu hello</span><span class="sxs-lookup"><span data-stu-id="6d4ed-138">Create hello management client</span></span>

1. <span data-ttu-id="6d4ed-139">Otevřete soubor Program.cs hello pro hello projekt, který jste vytvořili a pak přidejte tyto příkazy toohello existující příkazy using v horní části souboru hello:</span><span class="sxs-lookup"><span data-stu-id="6d4ed-139">Open hello Program.cs file for hello project that you created, and then add these using statements toohello existing statements at top of hello file:</span></span>

    ```
    using Microsoft.Azure.Management.Compute.Fluent;
    using Microsoft.Azure.Management.Compute.Fluent.Models;
    using Microsoft.Azure.Management.Fluent;
    using Microsoft.Azure.Management.ResourceManager.Fluent;
    using Microsoft.Azure.Management.ResourceManager.Fluent.Core;
    ```

2. <span data-ttu-id="6d4ed-140">Klient správy hello toocreate, přidejte tento kód toohello metodu Main:</span><span class="sxs-lookup"><span data-stu-id="6d4ed-140">toocreate hello management client, add this code toohello Main method:</span></span>

    ```
    var credentials = SdkContext.AzureCredentialsFactory
        .FromFile(Environment.GetEnvironmentVariable("AZURE_AUTH_LOCATION"));

    var azure = Azure
        .Configure()
        .WithLogLevel(HttpLoggingDelegatingHandler.Level.Basic)
        .Authenticate(credentials)
        .WithDefaultSubscription();
    ```

## <a name="create-resources"></a><span data-ttu-id="6d4ed-141">Vytvoření prostředků</span><span class="sxs-lookup"><span data-stu-id="6d4ed-141">Create resources</span></span>

### <a name="create-hello-resource-group"></a><span data-ttu-id="6d4ed-142">Vytvořte skupinu prostředků hello</span><span class="sxs-lookup"><span data-stu-id="6d4ed-142">Create hello resource group</span></span>

<span data-ttu-id="6d4ed-143">Musí být všechny prostředky obsažené v [skupiny prostředků](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6d4ed-143">All resources must be contained in a [Resource group](../../azure-resource-manager/resource-group-overview.md).</span></span>

<span data-ttu-id="6d4ed-144">toospecify hodnoty pro hello aplikace a vytvořte skupinu prostředků hello, přidejte tento kód toohello metodu Main:</span><span class="sxs-lookup"><span data-stu-id="6d4ed-144">toospecify values for hello application and create hello resource group, add this code toohello Main method:</span></span>

```
var groupName = "myResourceGroup";
var vmName = "myVM";
var location = Region.USWest;
    
Console.WriteLine("Creating resource group...");
var resourceGroup = azure.ResourceGroups.Define(groupName)
    .WithRegion(location)
    .Create();
```

### <a name="create-hello-availability-set"></a><span data-ttu-id="6d4ed-145">Vytvořit skupinu dostupnosti hello</span><span class="sxs-lookup"><span data-stu-id="6d4ed-145">Create hello availability set</span></span>

<span data-ttu-id="6d4ed-146">[Skupiny dostupnosti](tutorial-availability-sets.md) bylo snazší pro vás toomaintain hello virtuální počítače používané vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="6d4ed-146">[Availability sets](tutorial-availability-sets.md) make it easier for you toomaintain hello virtual machines used by your application.</span></span>

<span data-ttu-id="6d4ed-147">dostupnost hello toocreate nastavit, přidejte tento kód toohello metodu Main:</span><span class="sxs-lookup"><span data-stu-id="6d4ed-147">toocreate hello availability set, add this code toohello Main method:</span></span>

```
Console.WriteLine("Creating availability set...");
var availabilitySet = azure.AvailabilitySets.Define("myAVSet")
    .WithRegion(location)
    .WithExistingResourceGroup(groupName)
    .WithSku(AvailabilitySetSkuTypes.Managed)
    .Create();
```

### <a name="create-hello-public-ip-address"></a><span data-ttu-id="6d4ed-148">Vytvoření veřejné IP adresy hello</span><span class="sxs-lookup"><span data-stu-id="6d4ed-148">Create hello public IP address</span></span>

<span data-ttu-id="6d4ed-149">A [veřejnou IP adresu](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) je potřebné toocommunicate s hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="6d4ed-149">A [Public IP address](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) is needed toocommunicate with hello virtual machine.</span></span>

<span data-ttu-id="6d4ed-150">toocreate hello veřejnou IP adresu pro virtuální počítač hello, přidejte tento kód toohello metodu Main:</span><span class="sxs-lookup"><span data-stu-id="6d4ed-150">toocreate hello public IP address for hello virtual machine, add this code toohello Main method:</span></span>
   
```
Console.WriteLine("Creating public IP address...");
var publicIPAddress = azure.PublicIPAddresses.Define("myPublicIP")
    .WithRegion(location)
    .WithExistingResourceGroup(groupName)
    .WithDynamicIP()
    .Create();
```

### <a name="create-hello-virtual-network"></a><span data-ttu-id="6d4ed-151">Vytvoření virtuální sítě hello</span><span class="sxs-lookup"><span data-stu-id="6d4ed-151">Create hello virtual network</span></span>

<span data-ttu-id="6d4ed-152">Virtuální počítač musí být v podsíti [virtuální síť](../../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6d4ed-152">A virtual machine must be in a subnet of a [Virtual network](../../virtual-network/virtual-networks-overview.md).</span></span>

<span data-ttu-id="6d4ed-153">toocreate a podsítě a virtuální síti, přidejte tento kód toohello metodu Main:</span><span class="sxs-lookup"><span data-stu-id="6d4ed-153">toocreate a subnet and a virtual network, add this code toohello Main method:</span></span>

```
Console.WriteLine("Creating virtual network...");
var network = azure.Networks.Define("myVNet")
    .WithRegion(location)
    .WithExistingResourceGroup(groupName)
    .WithAddressSpace("10.0.0.0/16")
    .WithSubnet("mySubnet", "10.0.0.0/24")
    .Create();
```

### <a name="create-hello-network-interface"></a><span data-ttu-id="6d4ed-154">Vytvoření hello síťové rozhraní</span><span class="sxs-lookup"><span data-stu-id="6d4ed-154">Create hello network interface</span></span>

<span data-ttu-id="6d4ed-155">Virtuální počítač vyžaduje toocommunicate rozhraní sítě ve virtuální síti hello.</span><span class="sxs-lookup"><span data-stu-id="6d4ed-155">A virtual machine needs a network interface toocommunicate on hello virtual network.</span></span>

<span data-ttu-id="6d4ed-156">toocreate síťového rozhraní, přidejte tento kód toohello metodu Main:</span><span class="sxs-lookup"><span data-stu-id="6d4ed-156">toocreate a network interface, add this code toohello Main method:</span></span>

```
Console.WriteLine("Creating network interface...");
var networkInterface = azure.NetworkInterfaces.Define("myNIC")
    .WithRegion(location)
    .WithExistingResourceGroup(groupName)
    .WithExistingPrimaryNetwork(network)
    .WithSubnet("mySubnet")
    .WithPrimaryPrivateIPAddressDynamic()
    .WithExistingPrimaryPublicIPAddress(publicIPAddress)
    .Create();
 ```

### <a name="create-hello-virtual-machine"></a><span data-ttu-id="6d4ed-157">Vytvoření virtuálního počítače hello</span><span class="sxs-lookup"><span data-stu-id="6d4ed-157">Create hello virtual machine</span></span>

<span data-ttu-id="6d4ed-158">Teď, když jste vytvořili všechny hello Podpora prostředků, můžete vytvořit virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="6d4ed-158">Now that you created all hello supporting resources, you can create a virtual machine.</span></span>

<span data-ttu-id="6d4ed-159">toocreate hello virtuálního počítače, přidejte tento kód toohello metodu Main:</span><span class="sxs-lookup"><span data-stu-id="6d4ed-159">toocreate hello virtual machine, add this code toohello Main method:</span></span>

```
Console.WriteLine("Creating virtual machine...");
azure.VirtualMachines.Define(vmName)
    .WithRegion(location)
    .WithExistingResourceGroup(groupName)
    .WithExistingPrimaryNetworkInterface(networkInterface)
    .WithLatestWindowsImage("MicrosoftWindowsServer", "WindowsServer", "2012-R2-Datacenter")
    .WithAdminUsername("azureuser")
    .WithAdminPassword("Azure12345678")
    .WithComputerName(vmName)
    .WithExistingAvailabilitySet(availabilitySet)
    .WithSize(VirtualMachineSizeTypes.StandardDS1)
    .Create();
```

> [!NOTE]
> <span data-ttu-id="6d4ed-160">V tomto kurzu vytvoří virtuální počítač s verzí operačního systému Windows Server hello.</span><span class="sxs-lookup"><span data-stu-id="6d4ed-160">This tutorial creates a virtual machine running a version of hello Windows Server operating system.</span></span> <span data-ttu-id="6d4ed-161">toolearn Další informace o výběru ostatní Image, najdete v části [vyhledání a výběr imagí virtuálních počítačů Azure pomocí prostředí Windows PowerShell a rozhraní příkazového řádku Azure hello](../linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="6d4ed-161">toolearn more about selecting other images, see [Navigate and select Azure virtual machine images with Windows PowerShell and hello Azure CLI](../linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
> 
>

<span data-ttu-id="6d4ed-162">Pokud chcete toouse existující disk místo image pořízenou prostřednictvím marketplace, použijte tento kód:</span><span class="sxs-lookup"><span data-stu-id="6d4ed-162">If you want toouse an existing disk instead of a marketplace image, use this code:</span></span>

```
var managedDisk = azure.Disks.Define("myosdisk")
    .WithRegion(location)
    .WithExistingResourceGroup(groupName)
    .WithWindowsFromVhd("https://mystorage.blob.core.windows.net/vhds/myosdisk.vhd")
    .WithSizeInGB(128)
    .WithSku(DiskSkuTypes.PremiumLRS)
    .Create();

azure.VirtualMachines.Define("myVM")
    .WithRegion(location)
    .WithExistingResourceGroup(groupName)
    .WithExistingPrimaryNetworkInterface(networkInterface)
    .WithSpecializedOSDisk(managedDisk, OperatingSystemTypes.Windows)
    .WithExistingAvailabilitySet(availabilitySet)
    .WithSize(VirtualMachineSizeTypes.StandardDS1)
    .Create();
```

## <a name="perform-management-tasks"></a><span data-ttu-id="6d4ed-163">Provádění úloh správy</span><span class="sxs-lookup"><span data-stu-id="6d4ed-163">Perform management tasks</span></span>

<span data-ttu-id="6d4ed-164">Během životního cyklu hello virtuálního počítače můžete úlohy správy toorun například spuštění, zastavení nebo odstranění virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="6d4ed-164">During hello lifecycle of a virtual machine, you may want toorun management tasks such as starting, stopping, or deleting a virtual machine.</span></span> <span data-ttu-id="6d4ed-165">Kromě toho můžete toocreate kód tooautomate opakovaných nebo komplexní úlohy.</span><span class="sxs-lookup"><span data-stu-id="6d4ed-165">Additionally, you may want toocreate code tooautomate repetitive or complex tasks.</span></span>

<span data-ttu-id="6d4ed-166">Pokud budete potřebovat toodo nic s hello virtuálních počítačů, je třeba tooget její instanci:</span><span class="sxs-lookup"><span data-stu-id="6d4ed-166">When you need toodo anything with hello VM, you need tooget an instance of it:</span></span>

```
var vm = azure.VirtualMachines.GetByResourceGroup(groupName, vmName);
```

### <a name="get-information-about-hello-vm"></a><span data-ttu-id="6d4ed-167">Získání informací o hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="6d4ed-167">Get information about hello VM</span></span>

<span data-ttu-id="6d4ed-168">tooget informace o virtuálním počítači hello, přidejte tento kód toohello metodu Main:</span><span class="sxs-lookup"><span data-stu-id="6d4ed-168">tooget information about hello virtual machine, add this code toohello Main method:</span></span>

```
Console.WriteLine("Getting information about hello virtual machine...");
Console.WriteLine("hardwareProfile");
Console.WriteLine("   vmSize: " + vm.Size);
Console.WriteLine("storageProfile");
Console.WriteLine("  imageReference");
Console.WriteLine("    publisher: " + vm.StorageProfile.ImageReference.Publisher);
Console.WriteLine("    offer: " + vm.StorageProfile.ImageReference.Offer);
Console.WriteLine("    sku: " + vm.StorageProfile.ImageReference.Sku);
Console.WriteLine("    version: " + vm.StorageProfile.ImageReference.Version);
Console.WriteLine("  osDisk");
Console.WriteLine("    osType: " + vm.StorageProfile.OsDisk.OsType);
Console.WriteLine("    name: " + vm.StorageProfile.OsDisk.Name);
Console.WriteLine("    createOption: " + vm.StorageProfile.OsDisk.CreateOption);
Console.WriteLine("    caching: " + vm.StorageProfile.OsDisk.Caching);
Console.WriteLine("osProfile");
Console.WriteLine("  computerName: " + vm.OSProfile.ComputerName);
Console.WriteLine("  adminUsername: " + vm.OSProfile.AdminUsername);
Console.WriteLine("  provisionVMAgent: " + vm.OSProfile.WindowsConfiguration.ProvisionVMAgent.Value);
Console.WriteLine("  enableAutomaticUpdates: " + vm.OSProfile.WindowsConfiguration.EnableAutomaticUpdates.Value);
Console.WriteLine("networkProfile");
foreach (string nicId in vm.NetworkInterfaceIds)
{
    Console.WriteLine("  networkInterface id: " + nicId);
}
Console.WriteLine("vmAgent");
Console.WriteLine("  vmAgentVersion" + vm.InstanceView.VmAgent.VmAgentVersion);
Console.WriteLine("    statuses");
foreach (InstanceViewStatus stat in vm.InstanceView.VmAgent.Statuses)
{
    Console.WriteLine("    code: " + stat.Code);
    Console.WriteLine("    level: " + stat.Level);
    Console.WriteLine("    displayStatus: " + stat.DisplayStatus);
    Console.WriteLine("    message: " + stat.Message);
    Console.WriteLine("    time: " + stat.Time);
}
Console.WriteLine("disks");
foreach (DiskInstanceView disk in vm.InstanceView.Disks)
{
    Console.WriteLine("  name: " + disk.Name);
    Console.WriteLine("  statuses");
    foreach (InstanceViewStatus stat in disk.Statuses)
    {
        Console.WriteLine("    code: " + stat.Code);
        Console.WriteLine("    level: " + stat.Level);
        Console.WriteLine("    displayStatus: " + stat.DisplayStatus);
        Console.WriteLine("    time: " + stat.Time);
    }
}
Console.WriteLine("VM general status");
Console.WriteLine("  provisioningStatus: " + vm.ProvisioningState);
Console.WriteLine("  id: " + vm.Id);
Console.WriteLine("  name: " + vm.Name);
Console.WriteLine("  type: " + vm.Type);
Console.WriteLine("  location: " + vm.Region);
Console.WriteLine("VM instance status");
foreach (InstanceViewStatus stat in vm.InstanceView.Statuses)
{
    Console.WriteLine("  code: " + stat.Code);
    Console.WriteLine("  level: " + stat.Level);
    Console.WriteLine("  displayStatus: " + stat.DisplayStatus);
}
Console.WriteLine("Press enter toocontinue...");
Console.ReadLine();
```

### <a name="stop-hello-vm"></a><span data-ttu-id="6d4ed-169">Zastavit hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="6d4ed-169">Stop hello VM</span></span>

<span data-ttu-id="6d4ed-170">Můžete zastavit virtuální počítač a ponechat jeho nastavení, ale pokračovat toobe účtovat pro něj nebo můžete zastavit virtuální počítač a jeho navrácení.</span><span class="sxs-lookup"><span data-stu-id="6d4ed-170">You can stop a virtual machine and keep all its settings, but continue toobe charged for it, or you can stop a virtual machine and deallocate it.</span></span> <span data-ttu-id="6d4ed-171">Při zrušení jeho přidělení virtuálního počítače jsou končí deallocated a fakturace pro ni také všechny prostředky, které jsou s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="6d4ed-171">When a virtual machine is deallocated, all resources associated with it are also deallocated and billing ends for it.</span></span>

<span data-ttu-id="6d4ed-172">toostop hello virtuální počítač bez rušení přidělení, přidejte tento kód toohello metodu Main:</span><span class="sxs-lookup"><span data-stu-id="6d4ed-172">toostop hello virtual machine without deallocating it, add this code toohello Main method:</span></span>

```
Console.WriteLine("Stopping vm...");
vm.PowerOff();
Console.WriteLine("Press enter toocontinue...");
Console.ReadLine();
```

<span data-ttu-id="6d4ed-173">Pokud chcete toodeallocate hello virtuální počítač, změňte hello PowerOff volání toothis kódu:</span><span class="sxs-lookup"><span data-stu-id="6d4ed-173">If you want toodeallocate hello virtual machine, change hello PowerOff call toothis code:</span></span>

```
vm.Deallocate();
```

### <a name="start-hello-vm"></a><span data-ttu-id="6d4ed-174">Hello spuštění virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="6d4ed-174">Start hello VM</span></span>

<span data-ttu-id="6d4ed-175">toostart hello virtuálního počítače, přidejte tento kód toohello metodu Main:</span><span class="sxs-lookup"><span data-stu-id="6d4ed-175">toostart hello virtual machine, add this code toohello Main method:</span></span>

```
Console.WriteLine("Starting vm...");
vm.Start();
Console.WriteLine("Press enter toocontinue...");
Console.ReadLine();
```

### <a name="resize-hello-vm"></a><span data-ttu-id="6d4ed-176">Změnit velikost hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="6d4ed-176">Resize hello VM</span></span>

<span data-ttu-id="6d4ed-177">Mnoho aspektů nasazení měli zvážit při rozhodování o velikosti pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="6d4ed-177">Many aspects of deployment should be considered when deciding on a size for your virtual machine.</span></span> <span data-ttu-id="6d4ed-178">Další informace najdete v tématu [velikosti virtuálních počítačů](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="6d4ed-178">For more information, see [VM sizes](sizes.md).</span></span>  

<span data-ttu-id="6d4ed-179">velikost toochange hello virtuálního počítače, přidejte tento kód toohello metodu Main:</span><span class="sxs-lookup"><span data-stu-id="6d4ed-179">toochange size of hello virtual machine, add this code toohello Main method:</span></span>

```
Console.WriteLine("Resizing vm...");
vm.Update()
    .WithSize(VirtualMachineSizeTypes.StandardDS2) 
    .Apply();
Console.WriteLine("Press enter toocontinue...");
Console.ReadLine();
```

### <a name="add-a-data-disk-toohello-vm"></a><span data-ttu-id="6d4ed-180">Přidání disku toohello data virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="6d4ed-180">Add a data disk toohello VM</span></span>

<span data-ttu-id="6d4ed-181">tooadd datového disku toohello virtuálního počítače, přidejte tento kód toohello hlavní metoda tooadd datový disk, který je 2 GB velikostí Hanu logické jednotce LUN 0 a typu ukládání do mezipaměti v režimu ReadWrite:</span><span class="sxs-lookup"><span data-stu-id="6d4ed-181">tooadd a data disk toohello virtual machine, add this code toohello Main method tooadd a data disk that is 2 GB in size, han a LUN of 0 and a caching type of ReadWrite:</span></span>

```
Console.WriteLine("Adding data disk toovm...");
vm.Update()
    .WithNewDataDisk(2, 0, CachingTypes.ReadWrite) 
    .Apply();
Console.WriteLine("Press enter toodelete resources...");
Console.ReadLine();
```

## <a name="delete-resources"></a><span data-ttu-id="6d4ed-182">Odstraňte prostředky</span><span class="sxs-lookup"><span data-stu-id="6d4ed-182">Delete resources</span></span>

<span data-ttu-id="6d4ed-183">Vzhledem k tomu, že se vám účtovat prostředky využívané v Azure, vždycky je dobrým zvykem toodelete prostředky, které už nejsou potřeba.</span><span class="sxs-lookup"><span data-stu-id="6d4ed-183">Because you are charged for resources used in Azure, it is always good practice toodelete resources that are no longer needed.</span></span> <span data-ttu-id="6d4ed-184">Pokud chcete, aby toodelete hello virtuální počítače a všechny hello podpora prostředky, všechny máte toodo je odstranit skupinu prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="6d4ed-184">If you want toodelete hello virtual machines and all hello supporting resources, all you have toodo is delete hello resource group.</span></span>

<span data-ttu-id="6d4ed-185">toodelete hello prostředků skupiny, přidejte tento kód toohello metodu Main:</span><span class="sxs-lookup"><span data-stu-id="6d4ed-185">toodelete hello resource group, add this code toohello Main method:</span></span>

```
azure.ResourceGroups.DeleteByName(groupName);
```

## <a name="run-hello-application"></a><span data-ttu-id="6d4ed-186">Spuštění aplikace hello</span><span class="sxs-lookup"><span data-stu-id="6d4ed-186">Run hello application</span></span>

<span data-ttu-id="6d4ed-187">Má trvat přibližně pět minut, než tato toorun aplikace konzoly zcela od počáteční toofinish.</span><span class="sxs-lookup"><span data-stu-id="6d4ed-187">It should take about five minutes for this console application toorun completely from start toofinish.</span></span> 

1. <span data-ttu-id="6d4ed-188">toorun hello konzolovou aplikaci, klikněte na tlačítko **spustit**.</span><span class="sxs-lookup"><span data-stu-id="6d4ed-188">toorun hello console application, click **Start**.</span></span>

2. <span data-ttu-id="6d4ed-189">Před stisknutím klávesy **Enter** toostart odstraňování prostředky, vám může trvat několik minut tooverify hello vytváření prostředků hello v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="6d4ed-189">Before you press **Enter** toostart deleting resources, you could take a few minutes tooverify hello creation of hello resources in hello Azure portal.</span></span> <span data-ttu-id="6d4ed-190">Klikněte na tlačítko hello nasazení toosee informace o stavu hello nasazení.</span><span class="sxs-lookup"><span data-stu-id="6d4ed-190">Click hello deployment status toosee information about hello deployment.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6d4ed-191">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6d4ed-191">Next steps</span></span>
* <span data-ttu-id="6d4ed-192">Využít výhod pomocí toocreate šablony virtuálního počítače pomocí informací o hello v [nasadit virtuální počítač Azure pomocí jazyka C# a šablony Resource Manageru](csharp-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="6d4ed-192">Take advantage of using a template toocreate a virtual machine by using hello information in [Deploy an Azure Virtual Machine using C# and a Resource Manager template](csharp-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
* <span data-ttu-id="6d4ed-193">Další informace o používání hello [knihovny Azure pro .NET](https://docs.microsoft.com/dotnet/azure/?view=azure-dotnet).</span><span class="sxs-lookup"><span data-stu-id="6d4ed-193">Learn more about using hello [Azure libraries for .NET](https://docs.microsoft.com/dotnet/azure/?view=azure-dotnet).</span></span>

