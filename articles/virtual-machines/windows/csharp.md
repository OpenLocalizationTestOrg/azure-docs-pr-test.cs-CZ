---
title: "Vytvářet a spravovat virtuální počítač Azure pomocí jazyka C# | Microsoft Docs"
description: "Použití jazyka C# a Azure Resource Manager k nasazení virtuálního počítače a všechny její Podpůrné prostředky."
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
ms.openlocfilehash: 5d9021c2f65b70e36d5ea82992c9fb9d2d6d394a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="create-and-manage-windows-vms-in-azure-using-c"></a><span data-ttu-id="794a6-103">Vytvářet a spravovat virtuální počítače Windows v Azure pomocí jazyka C#</span><span class="sxs-lookup"><span data-stu-id="794a6-103">Create and manage Windows VMs in Azure using C#</span></span> #

<span data-ttu-id="794a6-104">[Virtuální počítač Azure](overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (VM) vyžaduje několik podpůrných prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="794a6-104">An [Azure Virtual Machine](overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (VM) needs several supporting Azure resources.</span></span> <span data-ttu-id="794a6-105">Tento článek popisuje vytváření, správu a odstraňování prostředky virtuálních počítačů pomocí jazyka C#.</span><span class="sxs-lookup"><span data-stu-id="794a6-105">This article covers creating, managing, and deleting VM resources using C#.</span></span> <span data-ttu-id="794a6-106">Získáte informace o těchto tématech:</span><span class="sxs-lookup"><span data-stu-id="794a6-106">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="794a6-107">Vytvoření projektu ve Visual Studiu</span><span class="sxs-lookup"><span data-stu-id="794a6-107">Create a Visual Studio project</span></span>
> * <span data-ttu-id="794a6-108">Instalovat balíček</span><span class="sxs-lookup"><span data-stu-id="794a6-108">Install the package</span></span>
> * <span data-ttu-id="794a6-109">Vytvořit přihlašovací údaje</span><span class="sxs-lookup"><span data-stu-id="794a6-109">Create credentials</span></span>
> * <span data-ttu-id="794a6-110">Vytvoření prostředků</span><span class="sxs-lookup"><span data-stu-id="794a6-110">Create resources</span></span>
> * <span data-ttu-id="794a6-111">Provádění úloh správy</span><span class="sxs-lookup"><span data-stu-id="794a6-111">Perform management tasks</span></span>
> * <span data-ttu-id="794a6-112">Odstraňte prostředky</span><span class="sxs-lookup"><span data-stu-id="794a6-112">Delete resources</span></span>
> * <span data-ttu-id="794a6-113">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="794a6-113">Run the application</span></span>

<span data-ttu-id="794a6-114">Proveďte tyto kroky trvá asi 20 minut.</span><span class="sxs-lookup"><span data-stu-id="794a6-114">It takes about 20 minutes to do these steps.</span></span>

## <a name="create-a-visual-studio-project"></a><span data-ttu-id="794a6-115">Vytvoření projektu ve Visual Studiu</span><span class="sxs-lookup"><span data-stu-id="794a6-115">Create a Visual Studio project</span></span>

1. <span data-ttu-id="794a6-116">Pokud jste to ještě neudělali, nainstalujte [Visual Studio](https://docs.microsoft.com/visualstudio/install/install-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="794a6-116">If you haven't already, install [Visual Studio](https://docs.microsoft.com/visualstudio/install/install-visual-studio).</span></span> <span data-ttu-id="794a6-117">Vyberte **vývoj aplikací .NET** na stránce úlohy a pak klikněte na tlačítko **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="794a6-117">Select **.NET desktop development** on the Workloads page, and then click **Install**.</span></span> <span data-ttu-id="794a6-118">V souhrnu, můžete uvidíte, že **nástrojů pro vývoj řešení pro rozhraní .NET Framework 4 4.6** je automaticky vybrána pro vás.</span><span class="sxs-lookup"><span data-stu-id="794a6-118">In the summary, you can see that **.NET Framework 4 - 4.6 development tools** is automatically selected for you.</span></span> <span data-ttu-id="794a6-119">Pokud jste již nainstalovali Visual Studio, můžete přidat pomocí Spouštěče sady Visual Studio .NET zatížení.</span><span class="sxs-lookup"><span data-stu-id="794a6-119">If you have already installed Visual Studio, you can add the .NET workload using the Visual Studio Launcher.</span></span>
2. <span data-ttu-id="794a6-120">V sadě Visual Studio, klikněte na tlačítko **soubor** > **nový** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="794a6-120">In Visual Studio, click **File** > **New** > **Project**.</span></span>
3. <span data-ttu-id="794a6-121">V **šablony** > **Visual C#**, vyberte **konzolovou aplikaci (rozhraní .NET Framework)**, zadejte *myDotnetProject* pro název projektu a vyberte umístění projektu a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="794a6-121">In **Templates** > **Visual C#**, select **Console App (.NET Framework)**, enter *myDotnetProject* for the name of the project, select the location of the project, and then click **OK**.</span></span>

## <a name="install-the-package"></a><span data-ttu-id="794a6-122">Instalovat balíček</span><span class="sxs-lookup"><span data-stu-id="794a6-122">Install the package</span></span>

<span data-ttu-id="794a6-123">Balíčky NuGet jsou nejjednodušší způsob, jak nainstalovat knihovny, které potřebujete k dokončení těchto kroků.</span><span class="sxs-lookup"><span data-stu-id="794a6-123">NuGet packages are the easiest way to install the libraries that you need to finish these steps.</span></span> <span data-ttu-id="794a6-124">Chcete-li získat knihovny, které je nutné v sadě Visual Studio, proveďte tyto kroky:</span><span class="sxs-lookup"><span data-stu-id="794a6-124">To get the libraries that you need in Visual Studio, do these steps:</span></span>

1. <span data-ttu-id="794a6-125">Klikněte na tlačítko **nástroje** > **Správce balíčků Nuget**a potom klikněte na **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="794a6-125">Click **Tools** > **Nuget Package Manager**, and then click **Package Manager Console**.</span></span>
2. <span data-ttu-id="794a6-126">V konzole zadejte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="794a6-126">Type this command in the console:</span></span>

    ```
    Install-Package Microsoft.Azure.Management.Fluent
    ```

## <a name="create-credentials"></a><span data-ttu-id="794a6-127">Vytvořit přihlašovací údaje</span><span class="sxs-lookup"><span data-stu-id="794a6-127">Create credentials</span></span>

<span data-ttu-id="794a6-128">Než začnete tento krok, ujistěte se, zda máte přístup k [objektu služby Active Directory](../../azure-resource-manager/resource-group-create-service-principal-portal.md).</span><span class="sxs-lookup"><span data-stu-id="794a6-128">Before you start this step, make sure that you have access to an [Active Directory service principal](../../azure-resource-manager/resource-group-create-service-principal-portal.md).</span></span> <span data-ttu-id="794a6-129">Také byste měli zaznamenat ID aplikace, ověřovací klíč a ID klienta, který budete potřebovat později.</span><span class="sxs-lookup"><span data-stu-id="794a6-129">You should also record the application ID, the authentication key, and the tenant ID that you need in a later step.</span></span>

### <a name="create-the-authorization-file"></a><span data-ttu-id="794a6-130">Vytvoření souboru autorizace</span><span class="sxs-lookup"><span data-stu-id="794a6-130">Create the authorization file</span></span>

1. <span data-ttu-id="794a6-131">V Průzkumníku řešení klikněte pravým tlačítkem na *myDotnetProject* > **přidat** > **nová položka**a potom vyberte **textový soubor** v *Visual C# položky*.</span><span class="sxs-lookup"><span data-stu-id="794a6-131">In Solution Explorer, right-click *myDotnetProject* > **Add** > **New Item**, and then select **Text File** in *Visual C# Items*.</span></span> <span data-ttu-id="794a6-132">Název souboru *azureauth.properties*a potom klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="794a6-132">Name the file *azureauth.properties*, and then click **Add**.</span></span>
2. <span data-ttu-id="794a6-133">Přidejte tyto vlastnosti autorizace:</span><span class="sxs-lookup"><span data-stu-id="794a6-133">Add these authorization properties:</span></span>

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

    <span data-ttu-id="794a6-134">Nahraďte  **&lt;id předplatného&gt;**  s ID vašeho předplatného  **&lt;id aplikace&gt;**  s identifikátor aplikace služby Active Directory  **&lt;ověřovací klíč&gt;**  s klíč aplikace a  **&lt;id klienta&gt;**  s identifikátorem klienta.</span><span class="sxs-lookup"><span data-stu-id="794a6-134">Replace **&lt;subscription-id&gt;** with your subscription identifier, **&lt;application-id&gt;** with the Active Directory application identifier, **&lt;authentication-key&gt;** with the application key, and **&lt;tenant-id&gt;** with the tenant identifier.</span></span>

3. <span data-ttu-id="794a6-135">Uložte soubor azureauth.properties.</span><span class="sxs-lookup"><span data-stu-id="794a6-135">Save the azureauth.properties file.</span></span> 
4. <span data-ttu-id="794a6-136">Nastavte proměnnou prostředí v systému Windows s názvem AZURE_AUTH_LOCATION s úplnou cestu k souboru autorizace, který jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="794a6-136">Set an environment variable in Windows named AZURE_AUTH_LOCATION with the full path to authorization file that you created.</span></span> <span data-ttu-id="794a6-137">Například můžete použít následující příkaz prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="794a6-137">For example, the following PowerShell command can be used:</span></span>

    ```
    [Environment]::SetEnvironmentVariable("AZURE_AUTH_LOCATION", "C:\Visual Studio 2017\Projects\myDotnetProject\myDotnetProject\azureauth.properties", "User")
    ```

### <a name="create-the-management-client"></a><span data-ttu-id="794a6-138">Vytvoření klienta správy</span><span class="sxs-lookup"><span data-stu-id="794a6-138">Create the management client</span></span>

1. <span data-ttu-id="794a6-139">Otevřete soubor Program.cs pro projekt, který jste vytvořili a poté přidejte tyto příkazy using do existující příkazy v horní části souboru:</span><span class="sxs-lookup"><span data-stu-id="794a6-139">Open the Program.cs file for the project that you created, and then add these using statements to the existing statements at top of the file:</span></span>

    ```
    using Microsoft.Azure.Management.Compute.Fluent;
    using Microsoft.Azure.Management.Compute.Fluent.Models;
    using Microsoft.Azure.Management.Fluent;
    using Microsoft.Azure.Management.ResourceManager.Fluent;
    using Microsoft.Azure.Management.ResourceManager.Fluent.Core;
    ```

2. <span data-ttu-id="794a6-140">Pro vytvoření klienta správy, přidejte do metody Main tento kód:</span><span class="sxs-lookup"><span data-stu-id="794a6-140">To create the management client, add this code to the Main method:</span></span>

    ```
    var credentials = SdkContext.AzureCredentialsFactory
        .FromFile(Environment.GetEnvironmentVariable("AZURE_AUTH_LOCATION"));

    var azure = Azure
        .Configure()
        .WithLogLevel(HttpLoggingDelegatingHandler.Level.Basic)
        .Authenticate(credentials)
        .WithDefaultSubscription();
    ```

## <a name="create-resources"></a><span data-ttu-id="794a6-141">Vytvoření prostředků</span><span class="sxs-lookup"><span data-stu-id="794a6-141">Create resources</span></span>

### <a name="create-the-resource-group"></a><span data-ttu-id="794a6-142">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="794a6-142">Create the resource group</span></span>

<span data-ttu-id="794a6-143">Musí být všechny prostředky obsažené v [skupiny prostředků](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="794a6-143">All resources must be contained in a [Resource group](../../azure-resource-manager/resource-group-overview.md).</span></span>

<span data-ttu-id="794a6-144">Zadejte hodnoty pro aplikaci a vytvořte skupinu prostředků, přidejte tento kód do metody Main:</span><span class="sxs-lookup"><span data-stu-id="794a6-144">To specify values for the application and create the resource group, add this code to the Main method:</span></span>

```
var groupName = "myResourceGroup";
var vmName = "myVM";
var location = Region.USWest;
    
Console.WriteLine("Creating resource group...");
var resourceGroup = azure.ResourceGroups.Define(groupName)
    .WithRegion(location)
    .Create();
```

### <a name="create-the-availability-set"></a><span data-ttu-id="794a6-145">Vytvořit sadu dostupnosti</span><span class="sxs-lookup"><span data-stu-id="794a6-145">Create the availability set</span></span>

<span data-ttu-id="794a6-146">[Skupiny dostupnosti](tutorial-availability-sets.md) umožňují snadnější zachování virtuálních počítačů, které používá vaše aplikace.</span><span class="sxs-lookup"><span data-stu-id="794a6-146">[Availability sets](tutorial-availability-sets.md) make it easier for you to maintain the virtual machines used by your application.</span></span>

<span data-ttu-id="794a6-147">Vytvořit skupinu dostupnosti, přidejte tento kód do metody Main:</span><span class="sxs-lookup"><span data-stu-id="794a6-147">To create the availability set, add this code to the Main method:</span></span>

```
Console.WriteLine("Creating availability set...");
var availabilitySet = azure.AvailabilitySets.Define("myAVSet")
    .WithRegion(location)
    .WithExistingResourceGroup(groupName)
    .WithSku(AvailabilitySetSkuTypes.Managed)
    .Create();
```

### <a name="create-the-public-ip-address"></a><span data-ttu-id="794a6-148">Vytvoření veřejné IP adresy</span><span class="sxs-lookup"><span data-stu-id="794a6-148">Create the public IP address</span></span>

<span data-ttu-id="794a6-149">A [veřejnou IP adresu](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) je potřeba ke komunikaci s virtuálním počítačem.</span><span class="sxs-lookup"><span data-stu-id="794a6-149">A [Public IP address](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) is needed to communicate with the virtual machine.</span></span>

<span data-ttu-id="794a6-150">Pokud chcete vytvořit veřejnou IP adresu pro virtuální počítač, přidejte do metodu Main tento kód:</span><span class="sxs-lookup"><span data-stu-id="794a6-150">To create the public IP address for the virtual machine, add this code to the Main method:</span></span>
   
```
Console.WriteLine("Creating public IP address...");
var publicIPAddress = azure.PublicIPAddresses.Define("myPublicIP")
    .WithRegion(location)
    .WithExistingResourceGroup(groupName)
    .WithDynamicIP()
    .Create();
```

### <a name="create-the-virtual-network"></a><span data-ttu-id="794a6-151">Vytvoření virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="794a6-151">Create the virtual network</span></span>

<span data-ttu-id="794a6-152">Virtuální počítač musí být v podsíti [virtuální síť](../../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="794a6-152">A virtual machine must be in a subnet of a [Virtual network](../../virtual-network/virtual-networks-overview.md).</span></span>

<span data-ttu-id="794a6-153">Pokud chcete vytvořit podsíť a virtuální síť, tento kód vložte do hlavní metoda:</span><span class="sxs-lookup"><span data-stu-id="794a6-153">To create a subnet and a virtual network, add this code to the Main method:</span></span>

```
Console.WriteLine("Creating virtual network...");
var network = azure.Networks.Define("myVNet")
    .WithRegion(location)
    .WithExistingResourceGroup(groupName)
    .WithAddressSpace("10.0.0.0/16")
    .WithSubnet("mySubnet", "10.0.0.0/24")
    .Create();
```

### <a name="create-the-network-interface"></a><span data-ttu-id="794a6-154">Vytvořit rozhraní sítě</span><span class="sxs-lookup"><span data-stu-id="794a6-154">Create the network interface</span></span>

<span data-ttu-id="794a6-155">Virtuální počítač vyžaduje síťové rozhraní pro komunikaci ve virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="794a6-155">A virtual machine needs a network interface to communicate on the virtual network.</span></span>

<span data-ttu-id="794a6-156">Pokud chcete vytvořit síťové rozhraní, tento kód vložte do hlavní metoda:</span><span class="sxs-lookup"><span data-stu-id="794a6-156">To create a network interface, add this code to the Main method:</span></span>

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

### <a name="create-the-virtual-machine"></a><span data-ttu-id="794a6-157">Vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="794a6-157">Create the virtual machine</span></span>

<span data-ttu-id="794a6-158">Teď, když jste vytvořili doprovodné materiály, můžete vytvořit virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="794a6-158">Now that you created all the supporting resources, you can create a virtual machine.</span></span>

<span data-ttu-id="794a6-159">Pokud chcete vytvořit virtuální počítač, přidejte do metodu Main tento kód:</span><span class="sxs-lookup"><span data-stu-id="794a6-159">To create the virtual machine, add this code to the Main method:</span></span>

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
> <span data-ttu-id="794a6-160">V tomto kurzu vytvoří virtuální počítač s verzí operačního systému Windows Server.</span><span class="sxs-lookup"><span data-stu-id="794a6-160">This tutorial creates a virtual machine running a version of the Windows Server operating system.</span></span> <span data-ttu-id="794a6-161">Další informace o výběru ostatní Image, najdete v tématu [vyhledání a výběr imagí virtuálních počítačů Azure pomocí prostředí Windows PowerShell a rozhraní příkazového řádku Azure](../linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="794a6-161">To learn more about selecting other images, see [Navigate and select Azure virtual machine images with Windows PowerShell and the Azure CLI](../linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
> 
>

<span data-ttu-id="794a6-162">Pokud chcete použít stávající disk, místo image pořízenou prostřednictvím marketplace, použijte tento kód:</span><span class="sxs-lookup"><span data-stu-id="794a6-162">If you want to use an existing disk instead of a marketplace image, use this code:</span></span>

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

## <a name="perform-management-tasks"></a><span data-ttu-id="794a6-163">Provádění úloh správy</span><span class="sxs-lookup"><span data-stu-id="794a6-163">Perform management tasks</span></span>

<span data-ttu-id="794a6-164">Během životního cyklu virtuálního počítače můžete spustit úlohy správy, jako je například spuštění, zastavení nebo odstranění virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="794a6-164">During the lifecycle of a virtual machine, you may want to run management tasks such as starting, stopping, or deleting a virtual machine.</span></span> <span data-ttu-id="794a6-165">Kromě toho můžete vytvořit kód pro automatizaci úloh opakovaných nebo komplexní.</span><span class="sxs-lookup"><span data-stu-id="794a6-165">Additionally, you may want to create code to automate repetitive or complex tasks.</span></span>

<span data-ttu-id="794a6-166">Když potřebujete udělat nic s virtuálním Počítačem, budete muset získat instanci ho:</span><span class="sxs-lookup"><span data-stu-id="794a6-166">When you need to do anything with the VM, you need to get an instance of it:</span></span>

```
var vm = azure.VirtualMachines.GetByResourceGroup(groupName, vmName);
```

### <a name="get-information-about-the-vm"></a><span data-ttu-id="794a6-167">Získat informace o virtuálním počítači</span><span class="sxs-lookup"><span data-stu-id="794a6-167">Get information about the VM</span></span>

<span data-ttu-id="794a6-168">Chcete-li získat informace o virtuálním počítači, přidejte do metodu Main tento kód:</span><span class="sxs-lookup"><span data-stu-id="794a6-168">To get information about the virtual machine, add this code to the Main method:</span></span>

```
Console.WriteLine("Getting information about the virtual machine...");
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
Console.WriteLine("Press enter to continue...");
Console.ReadLine();
```

### <a name="stop-the-vm"></a><span data-ttu-id="794a6-169">Zastavení virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="794a6-169">Stop the VM</span></span>

<span data-ttu-id="794a6-170">Můžete zastavit virtuální počítač a ponechat jeho nastavení, ale nadále účtovat poplatek za ho nebo můžete zastavit virtuální počítač a jeho navrácení.</span><span class="sxs-lookup"><span data-stu-id="794a6-170">You can stop a virtual machine and keep all its settings, but continue to be charged for it, or you can stop a virtual machine and deallocate it.</span></span> <span data-ttu-id="794a6-171">Při zrušení jeho přidělení virtuálního počítače jsou končí deallocated a fakturace pro ni také všechny prostředky, které jsou s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="794a6-171">When a virtual machine is deallocated, all resources associated with it are also deallocated and billing ends for it.</span></span>

<span data-ttu-id="794a6-172">Zastavte virtuální počítač bez rušení přidělení ho, přidejte tento kód do metody Main:</span><span class="sxs-lookup"><span data-stu-id="794a6-172">To stop the virtual machine without deallocating it, add this code to the Main method:</span></span>

```
Console.WriteLine("Stopping vm...");
vm.PowerOff();
Console.WriteLine("Press enter to continue...");
Console.ReadLine();
```

<span data-ttu-id="794a6-173">Pokud chcete zrušit přidělení virtuálního počítače, změna volání PowerOff tento kód:</span><span class="sxs-lookup"><span data-stu-id="794a6-173">If you want to deallocate the virtual machine, change the PowerOff call to this code:</span></span>

```
vm.Deallocate();
```

### <a name="start-the-vm"></a><span data-ttu-id="794a6-174">Spusťte virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="794a6-174">Start the VM</span></span>

<span data-ttu-id="794a6-175">Spuštění virtuálního počítače, přidejte tento kód do metody Main:</span><span class="sxs-lookup"><span data-stu-id="794a6-175">To start the virtual machine, add this code to the Main method:</span></span>

```
Console.WriteLine("Starting vm...");
vm.Start();
Console.WriteLine("Press enter to continue...");
Console.ReadLine();
```

### <a name="resize-the-vm"></a><span data-ttu-id="794a6-176">Změnit velikost virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="794a6-176">Resize the VM</span></span>

<span data-ttu-id="794a6-177">Mnoho aspektů nasazení měli zvážit při rozhodování o velikosti pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="794a6-177">Many aspects of deployment should be considered when deciding on a size for your virtual machine.</span></span> <span data-ttu-id="794a6-178">Další informace najdete v tématu [velikosti virtuálních počítačů](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="794a6-178">For more information, see [VM sizes](sizes.md).</span></span>  

<span data-ttu-id="794a6-179">Chcete-li změnit velikost virtuálního počítače, přidejte do metody Main tento kód:</span><span class="sxs-lookup"><span data-stu-id="794a6-179">To change size of the virtual machine, add this code to the Main method:</span></span>

```
Console.WriteLine("Resizing vm...");
vm.Update()
    .WithSize(VirtualMachineSizeTypes.StandardDS2) 
    .Apply();
Console.WriteLine("Press enter to continue...");
Console.ReadLine();
```

### <a name="add-a-data-disk-to-the-vm"></a><span data-ttu-id="794a6-180">Přidat datový disk k virtuálnímu počítači</span><span class="sxs-lookup"><span data-stu-id="794a6-180">Add a data disk to the VM</span></span>

<span data-ttu-id="794a6-181">Chcete-li přidat datový disk k virtuálnímu počítači, přidejte do metody Main přidat datový disk, který je 2 GB velikostí Hanu logické jednotce LUN 0 a typu ukládání do mezipaměti v režimu ReadWrite tento kód:</span><span class="sxs-lookup"><span data-stu-id="794a6-181">To add a data disk to the virtual machine, add this code to the Main method to add a data disk that is 2 GB in size, han a LUN of 0 and a caching type of ReadWrite:</span></span>

```
Console.WriteLine("Adding data disk to vm...");
vm.Update()
    .WithNewDataDisk(2, 0, CachingTypes.ReadWrite) 
    .Apply();
Console.WriteLine("Press enter to delete resources...");
Console.ReadLine();
```

## <a name="delete-resources"></a><span data-ttu-id="794a6-182">Odstraňte prostředky</span><span class="sxs-lookup"><span data-stu-id="794a6-182">Delete resources</span></span>

<span data-ttu-id="794a6-183">Vzhledem k tomu, že se vám účtovat prostředky využívané v Azure, vždycky je dobrým zvykem odstranit prostředky, které už nejsou potřeba.</span><span class="sxs-lookup"><span data-stu-id="794a6-183">Because you are charged for resources used in Azure, it is always good practice to delete resources that are no longer needed.</span></span> <span data-ttu-id="794a6-184">Pokud chcete odstranit virtuální počítače a všechny podpůrné prostředky, je vše, co musíte udělat, odstraňte skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="794a6-184">If you want to delete the virtual machines and all the supporting resources, all you have to do is delete the resource group.</span></span>

<span data-ttu-id="794a6-185">Pokud chcete odstranit skupinu prostředků, přidejte do metody Main tento kód:</span><span class="sxs-lookup"><span data-stu-id="794a6-185">To delete the resource group, add this code to the Main method:</span></span>

```
azure.ResourceGroups.DeleteByName(groupName);
```

## <a name="run-the-application"></a><span data-ttu-id="794a6-186">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="794a6-186">Run the application</span></span>

<span data-ttu-id="794a6-187">Dokončit má trvat přibližně pět minut, než tato Konzolová aplikace spustit úplně od začátku.</span><span class="sxs-lookup"><span data-stu-id="794a6-187">It should take about five minutes for this console application to run completely from start to finish.</span></span> 

1. <span data-ttu-id="794a6-188">Chcete-li spustit aplikaci konzoly, klikněte na tlačítko **spustit**.</span><span class="sxs-lookup"><span data-stu-id="794a6-188">To run the console application, click **Start**.</span></span>

2. <span data-ttu-id="794a6-189">Před stisknutím klávesy **Enter** zahájíte odstranění prostředků, může trvat několik minut na ověření vytváření prostředků na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="794a6-189">Before you press **Enter** to start deleting resources, you could take a few minutes to verify the creation of the resources in the Azure portal.</span></span> <span data-ttu-id="794a6-190">Klikněte na tlačítko Stav nasazení zobrazíte informace o tomto nasazení.</span><span class="sxs-lookup"><span data-stu-id="794a6-190">Click the deployment status to see information about the deployment.</span></span>

## <a name="next-steps"></a><span data-ttu-id="794a6-191">Další kroky</span><span class="sxs-lookup"><span data-stu-id="794a6-191">Next steps</span></span>
* <span data-ttu-id="794a6-192">Využít výhod pomocí šablony pro vytvoření virtuálního počítače pomocí informací v [nasadit virtuální počítač Azure pomocí jazyka C# a šablony Resource Manageru](csharp-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="794a6-192">Take advantage of using a template to create a virtual machine by using the information in [Deploy an Azure Virtual Machine using C# and a Resource Manager template](csharp-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
* <span data-ttu-id="794a6-193">Další informace o používání [knihovny Azure pro .NET](https://docs.microsoft.com/dotnet/azure/?view=azure-dotnet).</span><span class="sxs-lookup"><span data-stu-id="794a6-193">Learn more about using the [Azure libraries for .NET](https://docs.microsoft.com/dotnet/azure/?view=azure-dotnet).</span></span>

