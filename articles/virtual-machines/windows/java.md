---
title: "aaaCreate a spravovat služby Azure virtuálního počítače pomocí Java | Microsoft Docs"
description: "Virtuální počítač a všechny její Podpůrné prostředky, pomocí toodeploy Java a Azure Resource Manager."
services: virtual-machines-windows
documentationcenter: 
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: davidmu
ms.openlocfilehash: 31ac8d59f92ecff887e64906940933dd6fd50815
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-windows-vms-in-azure-using-java"></a><span data-ttu-id="27a0d-103">Vytvářet a spravovat virtuální počítače Windows v Azure pomocí Java</span><span class="sxs-lookup"><span data-stu-id="27a0d-103">Create and manage Windows VMs in Azure using Java</span></span>

<span data-ttu-id="27a0d-104">[Virtuální počítač Azure](overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (VM) vyžaduje několik podpůrných prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="27a0d-104">An [Azure Virtual Machine](overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (VM) needs several supporting Azure resources.</span></span> <span data-ttu-id="27a0d-105">Tento článek popisuje vytváření, správu a odstraňování prostředky virtuálních počítačů pomocí Java.</span><span class="sxs-lookup"><span data-stu-id="27a0d-105">This article covers creating, managing, and deleting VM resources using Java.</span></span> <span data-ttu-id="27a0d-106">Získáte informace o těchto tématech:</span><span class="sxs-lookup"><span data-stu-id="27a0d-106">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="27a0d-107">Vytvořte projekt Maven</span><span class="sxs-lookup"><span data-stu-id="27a0d-107">Create a Maven project</span></span>
> * <span data-ttu-id="27a0d-108">Přidat závislosti</span><span class="sxs-lookup"><span data-stu-id="27a0d-108">Add dependencies</span></span>
> * <span data-ttu-id="27a0d-109">Vytvořit přihlašovací údaje</span><span class="sxs-lookup"><span data-stu-id="27a0d-109">Create credentials</span></span>
> * <span data-ttu-id="27a0d-110">Vytvoření prostředků</span><span class="sxs-lookup"><span data-stu-id="27a0d-110">Create resources</span></span>
> * <span data-ttu-id="27a0d-111">Provádění úloh správy</span><span class="sxs-lookup"><span data-stu-id="27a0d-111">Perform management tasks</span></span>
> * <span data-ttu-id="27a0d-112">Odstraňte prostředky</span><span class="sxs-lookup"><span data-stu-id="27a0d-112">Delete resources</span></span>
> * <span data-ttu-id="27a0d-113">Spuštění aplikace hello</span><span class="sxs-lookup"><span data-stu-id="27a0d-113">Run hello application</span></span>

<span data-ttu-id="27a0d-114">Trvá přibližně 20 minut toodo tyto kroky.</span><span class="sxs-lookup"><span data-stu-id="27a0d-114">It takes about 20 minutes toodo these steps.</span></span>

## <a name="create-a-maven-project"></a><span data-ttu-id="27a0d-115">Vytvořte projekt Maven</span><span class="sxs-lookup"><span data-stu-id="27a0d-115">Create a Maven project</span></span>

1. <span data-ttu-id="27a0d-116">Pokud jste tak již neučinili, nainstalujte [Java](http://www.oracle.com/technetwork/java/javase/downloads/index.html).</span><span class="sxs-lookup"><span data-stu-id="27a0d-116">If you haven't already done so, install [Java](http://www.oracle.com/technetwork/java/javase/downloads/index.html).</span></span>
2. <span data-ttu-id="27a0d-117">Nainstalujte [Maven](http://maven.apache.org/download.cgi).</span><span class="sxs-lookup"><span data-stu-id="27a0d-117">Install [Maven](http://maven.apache.org/download.cgi).</span></span>
3. <span data-ttu-id="27a0d-118">Vytvoření nového projektu složku a hello:</span><span class="sxs-lookup"><span data-stu-id="27a0d-118">Create a new folder and hello project:</span></span>
    
    ```
    mkdir java-azure-test
    cd java-azure-test
    
    mvn archetype:generate -DgroupId=com.fabrikam -DartifactId=testAzureApp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

## <a name="add-dependencies"></a><span data-ttu-id="27a0d-119">Přidat závislosti</span><span class="sxs-lookup"><span data-stu-id="27a0d-119">Add dependencies</span></span>

1. <span data-ttu-id="27a0d-120">V části hello `testAzureApp` složku, otevřete hello `pom.xml` souboru a přidejte konfigurace sestavení hello příliš&lt;projektu&gt; tooenable hello sestavení vaší aplikace:</span><span class="sxs-lookup"><span data-stu-id="27a0d-120">Under hello `testAzureApp` folder, open hello `pom.xml` file and add hello build configuration too&lt;project&gt; tooenable hello building of your application:</span></span>

    ```xml
    <build>
      <plugins>
        <plugin>
            <groupId>org.codehaus.mojo</groupId>
            <artifactId>exec-maven-plugin</artifactId>
            <configuration>
                <mainClass>com.fabrikam.testAzureApp.App</mainClass>
            </configuration>
        </plugin>
      </plugins>
    </build>
    ```

2. <span data-ttu-id="27a0d-121">Přidá hello závislosti, které jsou potřebné tooaccess hello Azure Java SDK.</span><span class="sxs-lookup"><span data-stu-id="27a0d-121">Add hello dependencies that are needed tooaccess hello Azure Java SDK.</span></span>

    ```xml
    <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure</artifactId>
      <version>1.1.0</version>
    </dependency>
    <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-mgmt-compute</artifactId>
      <version>1.1.0</version>
    </dependency>
    <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-mgmt-resources</artifactId>
      <version>1.1.0</version>
    </dependency>
    <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-mgmt-network</artifactId>
      <version>1.1.0</version>
    </dependency>
    <dependency>
      <groupId>com.squareup.okio</groupId>
      <artifactId>okio</artifactId>
      <version>1.13.0</version>
    </dependency>
    <dependency> 
      <groupId>com.nimbusds</groupId>
      <artifactId>nimbus-jose-jwt</artifactId>
      <version>3.6</version>
    </dependency>
    <dependency>
      <groupId>net.minidev</groupId>
      <artifactId>json-smart</artifactId>
      <version>1.0.6.3</version>
    </dependency>
    <dependency>
      <groupId>javax.mail</groupId>
      <artifactId>mail</artifactId>
      <version>1.4.5</version>
    </dependency>
    ```

3. <span data-ttu-id="27a0d-122">Uložte soubor hello.</span><span class="sxs-lookup"><span data-stu-id="27a0d-122">Save hello file.</span></span>

## <a name="create-credentials"></a><span data-ttu-id="27a0d-123">Vytvořit přihlašovací údaje</span><span class="sxs-lookup"><span data-stu-id="27a0d-123">Create credentials</span></span>

<span data-ttu-id="27a0d-124">Než začnete tento krok, ujistěte se, zda máte přístup tooan [objektu služby Active Directory](../../azure-resource-manager/resource-group-create-service-principal-portal.md).</span><span class="sxs-lookup"><span data-stu-id="27a0d-124">Before you start this step, make sure that you have access tooan [Active Directory service principal](../../azure-resource-manager/resource-group-create-service-principal-portal.md).</span></span> <span data-ttu-id="27a0d-125">Také byste měli zaznamenat ID aplikace hello hello ověřovací klíč a hello ID klienta, který budete potřebovat později.</span><span class="sxs-lookup"><span data-stu-id="27a0d-125">You should also record hello application ID, hello authentication key, and hello tenant ID that you need in a later step.</span></span>

### <a name="create-hello-authorization-file"></a><span data-ttu-id="27a0d-126">Vytvoření souboru autorizace hello</span><span class="sxs-lookup"><span data-stu-id="27a0d-126">Create hello authorization file</span></span>

1. <span data-ttu-id="27a0d-127">Vytvořte soubor s názvem `azureauth.properties` a přidejte tyto vlastnosti tooit:</span><span class="sxs-lookup"><span data-stu-id="27a0d-127">Create a file named `azureauth.properties` and add these properties tooit:</span></span>

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

    <span data-ttu-id="27a0d-128">Nahraďte  **&lt;id předplatného&gt;**  s ID vašeho předplatného  **&lt;id aplikace&gt;**  s hello aplikace Active Directory identifikátor,  **&lt;ověřovací klíč&gt;**  klíčem aplikace hello a  **&lt;id klienta&gt;**  s klientem hello identifikátor.</span><span class="sxs-lookup"><span data-stu-id="27a0d-128">Replace **&lt;subscription-id&gt;** with your subscription identifier, **&lt;application-id&gt;** with hello Active Directory application identifier, **&lt;authentication-key&gt;** with hello application key, and **&lt;tenant-id&gt;** with hello tenant identifier.</span></span>

2. <span data-ttu-id="27a0d-129">Uložte soubor hello.</span><span class="sxs-lookup"><span data-stu-id="27a0d-129">Save hello file.</span></span>
3. <span data-ttu-id="27a0d-130">Nastavte proměnnou prostředí s názvem AZURE_AUTH_LOCATION ve vašem prostředí s hello úplná cesta toohello ověřování souboru.</span><span class="sxs-lookup"><span data-stu-id="27a0d-130">Set an environment variable named AZURE_AUTH_LOCATION in your shell with hello full path toohello authentication file.</span></span>

### <a name="create-hello-management-client"></a><span data-ttu-id="27a0d-131">Vytvoření klienta pro správu hello</span><span class="sxs-lookup"><span data-stu-id="27a0d-131">Create hello management client</span></span>

1. <span data-ttu-id="27a0d-132">Otevřete hello `App.java` souboru pod `src\main\java\com\fabrikam` a zajistěte, aby tento příkaz balíček je v horní části hello:</span><span class="sxs-lookup"><span data-stu-id="27a0d-132">Open hello `App.java` file under `src\main\java\com\fabrikam` and make sure this package statement is at hello top:</span></span>

    ```java
    package com.fabrikam.testAzureApp;
    ```

2. <span data-ttu-id="27a0d-133">V části prohlášení hello balíčku, přidejte tyto příkazy importu:</span><span class="sxs-lookup"><span data-stu-id="27a0d-133">Under hello package statement, add these import statements:</span></span>
   
    ```java
    import com.microsoft.azure.management.Azure;
    import com.microsoft.azure.management.compute.AvailabilitySet;
    import com.microsoft.azure.management.compute.AvailabilitySetSkuTypes;
    import com.microsoft.azure.management.compute.CachingTypes;
    import com.microsoft.azure.management.compute.InstanceViewStatus;
    import com.microsoft.azure.management.compute.DiskInstanceView;
    import com.microsoft.azure.management.compute.VirtualMachine;
    import com.microsoft.azure.management.compute.VirtualMachineSizeTypes;
    import com.microsoft.azure.management.network.PublicIPAddress;
    import com.microsoft.azure.management.network.Network;
    import com.microsoft.azure.management.network.NetworkInterface;
    import com.microsoft.azure.management.resources.ResourceGroup;
    import com.microsoft.azure.management.resources.fluentcore.arm.Region;
    import com.microsoft.azure.management.resources.fluentcore.model.Creatable;
    import com.microsoft.rest.LogLevel;
    import java.io.File;
    import java.util.Scanner;
    ```

2. <span data-ttu-id="27a0d-134">pověření služby Active Directory hello toocreate, je nutné, aby toomake požadavků, přidejte tento kód toohello hlavní metodu hello – třída aplikace:</span><span class="sxs-lookup"><span data-stu-id="27a0d-134">toocreate hello Active Directory credentials that you need toomake requests, add this code toohello main method of hello App class:</span></span>
   
    ```java
    try {    
        final File credFile = new File(System.getenv("AZURE_AUTH_LOCATION"));
        Azure azure = Azure.configure()
            .withLogLevel(LogLevel.BASIC)
            .authenticate(credFile)
            .withDefaultSubscription();
    } catch (Exception e) {
        System.out.println(e.getMessage());
        e.printStackTrace();
    }

    ```

## <a name="create-resources"></a><span data-ttu-id="27a0d-135">Vytvoření prostředků</span><span class="sxs-lookup"><span data-stu-id="27a0d-135">Create resources</span></span>

### <a name="create-hello-resource-group"></a><span data-ttu-id="27a0d-136">Vytvořte skupinu prostředků hello</span><span class="sxs-lookup"><span data-stu-id="27a0d-136">Create hello resource group</span></span>

<span data-ttu-id="27a0d-137">Musí být všechny prostředky obsažené v [skupiny prostředků](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="27a0d-137">All resources must be contained in a [Resource group](../../azure-resource-manager/resource-group-overview.md).</span></span>

<span data-ttu-id="27a0d-138">toospecify hodnoty pro hello aplikace a vytvořte skupinu prostředků hello, přidejte tento kód toohello zkuste blok v hlavní metoda hello:</span><span class="sxs-lookup"><span data-stu-id="27a0d-138">toospecify values for hello application and create hello resource group, add this code toohello try block in hello main method:</span></span>

```java
System.out.println("Creating resource group...");
ResourceGroup resourceGroup = azure.resourceGroups()
    .define("myResourceGroup")
    .withRegion(Region.US_EAST)
    .create();
```

### <a name="create-hello-availability-set"></a><span data-ttu-id="27a0d-139">Vytvořit skupinu dostupnosti hello</span><span class="sxs-lookup"><span data-stu-id="27a0d-139">Create hello availability set</span></span>

<span data-ttu-id="27a0d-140">[Skupiny dostupnosti](tutorial-availability-sets.md) bylo snazší pro vás toomaintain hello virtuální počítače používané vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="27a0d-140">[Availability sets](tutorial-availability-sets.md) make it easier for you toomaintain hello virtual machines used by your application.</span></span>

<span data-ttu-id="27a0d-141">dostupnost hello toocreate nastavit, přidejte tento kód toohello zkuste blok v hlavní metoda hello:</span><span class="sxs-lookup"><span data-stu-id="27a0d-141">toocreate hello availability set, add this code toohello try block in hello main method:</span></span>

```java
System.out.println("Creating availability set...");
AvailabilitySet availabilitySet = azure.availabilitySets()
    .define("myAvailabilitySet")
    .withRegion(Region.US_EAST)
    .withExistingResourceGroup("myResourceGroup")
    .withSku(AvailabilitySetSkuTypes.MANAGED)
    .create();
```
### <a name="create-hello-public-ip-address"></a><span data-ttu-id="27a0d-142">Vytvoření veřejné IP adresy hello</span><span class="sxs-lookup"><span data-stu-id="27a0d-142">Create hello public IP address</span></span>

<span data-ttu-id="27a0d-143">A [veřejnou IP adresu](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) je potřebné toocommunicate s hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="27a0d-143">A [Public IP address](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) is needed toocommunicate with hello virtual machine.</span></span>

<span data-ttu-id="27a0d-144">toocreate hello veřejnou IP adresu pro virtuální počítač hello, přidejte tento kód toohello zkuste blok v hlavní metoda hello:</span><span class="sxs-lookup"><span data-stu-id="27a0d-144">toocreate hello public IP address for hello virtual machine, add this code toohello try block in hello main method:</span></span>

```java
System.out.println("Creating public IP address...");
PublicIPAddress publicIPAddress = azure.publicIPAddresses()
    .define("myPublicIP")
    .withRegion(Region.US_EAST)
    .withExistingResourceGroup("myResourceGroup")
    .withDynamicIP()
    .create();
```

### <a name="create-hello-virtual-network"></a><span data-ttu-id="27a0d-145">Vytvoření virtuální sítě hello</span><span class="sxs-lookup"><span data-stu-id="27a0d-145">Create hello virtual network</span></span>

<span data-ttu-id="27a0d-146">Virtuální počítač musí být v podsíti [virtuální síť](../../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="27a0d-146">A virtual machine must be in a subnet of a [Virtual network](../../virtual-network/virtual-networks-overview.md).</span></span>

<span data-ttu-id="27a0d-147">toocreate a podsítě a virtuální síť, přidejte tento blok kódu toohello zkuste hello hlavní metoda:</span><span class="sxs-lookup"><span data-stu-id="27a0d-147">toocreate a subnet and a virtual network, add this code toohello try block in hello main method:</span></span>

```java
System.out.println("Creating virtual network...");
Network network = azure.networks()
    .define("myVN")
    .withRegion(Region.US_EAST)
    .withExistingResourceGroup("myResourceGroup")
    .withAddressSpace("10.0.0.0/16")
    .withSubnet("mySubnet","10.0.0.0/24")
    .create();
```

### <a name="create-hello-network-interface"></a><span data-ttu-id="27a0d-148">Vytvoření hello síťové rozhraní</span><span class="sxs-lookup"><span data-stu-id="27a0d-148">Create hello network interface</span></span>

<span data-ttu-id="27a0d-149">Virtuální počítač vyžaduje toocommunicate rozhraní sítě ve virtuální síti hello.</span><span class="sxs-lookup"><span data-stu-id="27a0d-149">A virtual machine needs a network interface toocommunicate on hello virtual network.</span></span>

<span data-ttu-id="27a0d-150">toocreate síťového rozhraní, přidejte tento kód toohello zkuste blok v hlavní metoda hello:</span><span class="sxs-lookup"><span data-stu-id="27a0d-150">toocreate a network interface, add this code toohello try block in hello main method:</span></span>

```java
System.out.println("Creating network interface...");
NetworkInterface networkInterface = azure.networkInterfaces()
    .define("myNIC")
    .withRegion(Region.US_EAST)
    .withExistingResourceGroup("myResourceGroup")
    .withExistingPrimaryNetwork(network)
    .withSubnet("mySubnet")
    .withPrimaryPrivateIPAddressDynamic()
    .withExistingPrimaryPublicIPAddress(publicIPAddress)
    .create();
```

### <a name="create-hello-virtual-machine"></a><span data-ttu-id="27a0d-151">Vytvoření virtuálního počítače hello</span><span class="sxs-lookup"><span data-stu-id="27a0d-151">Create hello virtual machine</span></span>

<span data-ttu-id="27a0d-152">Teď, když jste vytvořili všechny hello Podpora prostředků, můžete vytvořit virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="27a0d-152">Now that you created all hello supporting resources, you can create a virtual machine.</span></span>

<span data-ttu-id="27a0d-153">toocreate hello virtuálního počítače, přidejte tento kód toohello zkuste blok v hlavní metoda hello:</span><span class="sxs-lookup"><span data-stu-id="27a0d-153">toocreate hello virtual machine, add this code toohello try block in hello main method:</span></span>

```java
System.out.println("Creating virtual machine...");
VirtualMachine virtualMachine = azure.virtualMachines()
    .define("myVM")
    .withRegion(Region.US_EAST)
    .withExistingResourceGroup("myResourceGroup")
    .withExistingPrimaryNetworkInterface(networkInterface)
    .withLatestWindowsImage("MicrosoftWindowsServer", "WindowsServer", "2012-R2-Datacenter")
    .withAdminUsername("azureuser")
    .withAdminPassword("Azure12345678")
    .withComputerName("myVM")
    .withExistingAvailabilitySet(availabilitySet)
    .withSize("Standard_DS1")
    .create();
Scanner input = new Scanner(System.in);
System.out.println("Press enter tooget information about hello VM...");
input.nextLine();
```

> [!NOTE]
> <span data-ttu-id="27a0d-154">V tomto kurzu vytvoří virtuální počítač s verzí operačního systému Windows Server hello.</span><span class="sxs-lookup"><span data-stu-id="27a0d-154">This tutorial creates a virtual machine running a version of hello Windows Server operating system.</span></span> <span data-ttu-id="27a0d-155">toolearn Další informace o výběru ostatní Image, najdete v části [vyhledání a výběr imagí virtuálních počítačů Azure pomocí prostředí Windows PowerShell a rozhraní příkazového řádku Azure hello](../linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="27a0d-155">toolearn more about selecting other images, see [Navigate and select Azure virtual machine images with Windows PowerShell and hello Azure CLI](../linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
> 
>

<span data-ttu-id="27a0d-156">Pokud chcete toouse existující disk místo image pořízenou prostřednictvím marketplace, použijte tento kód:</span><span class="sxs-lookup"><span data-stu-id="27a0d-156">If you want toouse an existing disk instead of a marketplace image, use this code:</span></span> 

```java
ManagedDisk managedDisk = azure.disks.define("myosdisk") 
    .withRegion(Region.US_EAST) 
    .withExistingResourceGroup("myResourceGroup") 
    .withWindowsFromVhd("https://mystorage.blob.core.windows.net/vhds/myosdisk.vhd") 
    .withSizeInGB(128) 
    .withSku(DiskSkuTypes.PremiumLRS) 
    .create(); 

azure.virtualMachines.define("myVM") 
    .withRegion(Region.US_EAST) 
    .withExistingResourceGroup("myResourceGroup") 
    .withExistingPrimaryNetworkInterface(networkInterface) 
    .withSpecializedOSDisk(managedDisk, OperatingSystemTypes.Windows) 
    .withExistingAvailabilitySet(availabilitySet) 
    .withSize(VirtualMachineSizeTypes.StandardDS1) 
    .create(); 
``` 

## <a name="perform-management-tasks"></a><span data-ttu-id="27a0d-157">Provádění úloh správy</span><span class="sxs-lookup"><span data-stu-id="27a0d-157">Perform management tasks</span></span>

<span data-ttu-id="27a0d-158">Během životního cyklu hello virtuálního počítače můžete úlohy správy toorun například spuštění, zastavení nebo odstranění virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="27a0d-158">During hello lifecycle of a virtual machine, you may want toorun management tasks such as starting, stopping, or deleting a virtual machine.</span></span> <span data-ttu-id="27a0d-159">Kromě toho můžete toocreate kód tooautomate opakovaných nebo komplexní úlohy.</span><span class="sxs-lookup"><span data-stu-id="27a0d-159">Additionally, you may want toocreate code tooautomate repetitive or complex tasks.</span></span>

<span data-ttu-id="27a0d-160">Pokud budete potřebovat toodo nic s hello virtuálních počítačů, je potřeba tooget její instanci.</span><span class="sxs-lookup"><span data-stu-id="27a0d-160">When you need toodo anything with hello VM, you need tooget an instance of it.</span></span> <span data-ttu-id="27a0d-161">Přidejte tento blok kódu toohello zkuste hello hlavní metody:</span><span class="sxs-lookup"><span data-stu-id="27a0d-161">Add this code toohello try block of hello main method:</span></span>

```java
VirtualMachine vm = azure.virtualMachines().getByResourceGroup("myResourceGroup", "myVM");
```

### <a name="get-information-about-hello-vm"></a><span data-ttu-id="27a0d-162">Získání informací o hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="27a0d-162">Get information about hello VM</span></span>

<span data-ttu-id="27a0d-163">tooget informace o virtuálním počítači hello, přidejte tento kód toohello zkuste blok v hlavní metoda hello:</span><span class="sxs-lookup"><span data-stu-id="27a0d-163">tooget information about hello virtual machine, add this code toohello try block in hello main method:</span></span>

```java
System.out.println("hardwareProfile");
System.out.println("    vmSize: " + vm.size());
System.out.println("storageProfile");
System.out.println("  imageReference");
System.out.println("    publisher: " + vm.storageProfile().imageReference().publisher());
System.out.println("    offer: " + vm.storageProfile().imageReference().offer());
System.out.println("    sku: " + vm.storageProfile().imageReference().sku());
System.out.println("    version: " + vm.storageProfile().imageReference().version());
System.out.println("  osDisk");
System.out.println("    osType: " + vm.storageProfile().osDisk().osType());
System.out.println("    name: " + vm.storageProfile().osDisk().name());
System.out.println("    createOption: " + vm.storageProfile().osDisk().createOption());
System.out.println("    caching: " + vm.storageProfile().osDisk().caching());
System.out.println("osProfile");
System.out.println("    computerName: " + vm.osProfile().computerName());
System.out.println("    adminUserName: " + vm.osProfile().adminUsername());
System.out.println("    provisionVMAgent: " + vm.osProfile().windowsConfiguration().provisionVMAgent());
System.out.println("    enableAutomaticUpdates: " + vm.osProfile().windowsConfiguration().enableAutomaticUpdates());
System.out.println("networkProfile");
System.out.println("    networkInterface: " + vm.primaryNetworkInterfaceId());
System.out.println("vmAgent");
System.out.println("  vmAgentVersion: " + vm.instanceView().vmAgent().vmAgentVersion());
System.out.println("    statuses");
for(InstanceViewStatus status : vm.instanceView().vmAgent().statuses()) {
    System.out.println("    code: " + status.code());
    System.out.println("    displayStatus: " + status.displayStatus());
    System.out.println("    message: " + status.message());
    System.out.println("    time: " + status.time());
}
System.out.println("disks");
for(DiskInstanceView disk : vm.instanceView().disks()) {
    System.out.println("  name: " + disk.name());
    System.out.println("  statuses");
    for(InstanceViewStatus status : disk.statuses()) {
        System.out.println("    code: " + status.code());
        System.out.println("    displayStatus: " + status.displayStatus());
        System.out.println("    time: " + status.time());
    }
}
System.out.println("VM general status");
System.out.println("  provisioningStatus: " + vm.provisioningState());
System.out.println("  id: " + vm.id());
System.out.println("  name: " + vm.name());
System.out.println("  type: " + vm.type());
System.out.println("VM instance status");
for(InstanceViewStatus status : vm.instanceView().statuses()) {
    System.out.println("  code: " + status.code());
    System.out.println("  displayStatus: " + status.displayStatus());
}
System.out.println("Press enter toocontinue...");
input.nextLine();   
```

### <a name="stop-hello-vm"></a><span data-ttu-id="27a0d-164">Zastavit hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="27a0d-164">Stop hello VM</span></span>

<span data-ttu-id="27a0d-165">Můžete zastavit virtuální počítač a ponechat jeho nastavení, ale pokračovat toobe účtovat pro něj nebo můžete zastavit virtuální počítač a jeho navrácení.</span><span class="sxs-lookup"><span data-stu-id="27a0d-165">You can stop a virtual machine and keep all its settings, but continue toobe charged for it, or you can stop a virtual machine and deallocate it.</span></span> <span data-ttu-id="27a0d-166">Při zrušení jeho přidělení virtuálního počítače jsou končí deallocated a fakturace pro ni také všechny prostředky, které jsou s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="27a0d-166">When a virtual machine is deallocated, all resources associated with it are also deallocated and billing ends for it.</span></span>

<span data-ttu-id="27a0d-167">toostop hello virtuální počítač bez rušení přidělení, přidejte tento kód toohello zkuste blok v hlavní metoda hello:</span><span class="sxs-lookup"><span data-stu-id="27a0d-167">toostop hello virtual machine without deallocating it, add this code toohello try block in hello main method:</span></span>

```java
System.out.println("Stopping vm...");
vm.powerOff();
System.out.println("Press enter toocontinue...");
input.nextLine();
```

<span data-ttu-id="27a0d-168">Pokud chcete toodeallocate hello virtuální počítač, změňte hello PowerOff volání toothis kódu:</span><span class="sxs-lookup"><span data-stu-id="27a0d-168">If you want toodeallocate hello virtual machine, change hello PowerOff call toothis code:</span></span>

```java
vm.deallocate();
```

### <a name="start-hello-vm"></a><span data-ttu-id="27a0d-169">Hello spuštění virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="27a0d-169">Start hello VM</span></span>

<span data-ttu-id="27a0d-170">toostart hello virtuálního počítače, přidejte tento kód toohello zkuste blok v hlavní metoda hello:</span><span class="sxs-lookup"><span data-stu-id="27a0d-170">toostart hello virtual machine, add this code toohello try block in hello main method:</span></span>

```java
System.out.println("Starting vm...");
vm.start();
System.out.println("Press enter toocontinue...");
input.nextLine();
```

### <a name="resize-hello-vm"></a><span data-ttu-id="27a0d-171">Změnit velikost hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="27a0d-171">Resize hello VM</span></span>

<span data-ttu-id="27a0d-172">Mnoho aspektů nasazení měli zvážit při rozhodování o velikosti pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="27a0d-172">Many aspects of deployment should be considered when deciding on a size for your virtual machine.</span></span> <span data-ttu-id="27a0d-173">Další informace najdete v tématu [velikosti virtuálních počítačů](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="27a0d-173">For more information, see [VM sizes](sizes.md).</span></span>  

<span data-ttu-id="27a0d-174">velikost toochange hello virtuálního počítače, přidejte tento blok kódu toohello zkuste hlavní metoda hello:</span><span class="sxs-lookup"><span data-stu-id="27a0d-174">toochange size of hello virtual machine, add this code toohello try block in hello main method:</span></span>

```java
System.out.println("Resizing vm...");
vm.update()
    .withSize(VirtualMachineSizeTypes.STANDARD_DS2)
    .apply();
System.out.println("Press enter toocontinue...");
input.nextLine();
```

### <a name="add-a-data-disk-toohello-vm"></a><span data-ttu-id="27a0d-175">Přidání disku toohello data virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="27a0d-175">Add a data disk toohello VM</span></span>

<span data-ttu-id="27a0d-176">tooadd datového disku toohello virtuální počítač, který je 2 GB velikost, má logické jednotky 0 a typu ukládání do mezipaměti v režimu ReadWrite, přidejte tento kód toohello zkuste blok v hlavní metoda hello:</span><span class="sxs-lookup"><span data-stu-id="27a0d-176">tooadd a data disk toohello virtual machine that is 2 GB in size, has a LUN of 0, and a caching type of ReadWrite, add this code toohello try block in hello main method:</span></span>

```java
System.out.println("Adding data disk...");
vm.update()
    .withNewDataDisk(2, 0, CachingTypes.READ_WRITE)
    .apply();
System.out.println("Press enter toodelete resources...");
input.nextLine();
```

## <a name="delete-resources"></a><span data-ttu-id="27a0d-177">Odstraňte prostředky</span><span class="sxs-lookup"><span data-stu-id="27a0d-177">Delete resources</span></span>

<span data-ttu-id="27a0d-178">Vzhledem k tomu, že se vám účtovat prostředky využívané v Azure, vždycky je dobrým zvykem toodelete prostředky, které už nejsou potřeba.</span><span class="sxs-lookup"><span data-stu-id="27a0d-178">Because you are charged for resources used in Azure, it is always good practice toodelete resources that are no longer needed.</span></span> <span data-ttu-id="27a0d-179">Pokud chcete, aby toodelete hello virtuální počítače a všechny hello podpora prostředky, všechny máte toodo je odstranit skupinu prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="27a0d-179">If you want toodelete hello virtual machines and all hello supporting resources, all you have toodo is delete hello resource group.</span></span>

1. <span data-ttu-id="27a0d-180">toodelete hello prostředků skupiny, přidejte tento kód toohello zkuste blok v hlavní metoda hello:</span><span class="sxs-lookup"><span data-stu-id="27a0d-180">toodelete hello resource group, add this code toohello try block in hello main method:</span></span>
   
```java
System.out.println("Deleting resources...");
azure.resourceGroups().deleteByName("myResourceGroup");
```

2. <span data-ttu-id="27a0d-181">Uložte soubor App.java hello.</span><span class="sxs-lookup"><span data-stu-id="27a0d-181">Save hello App.java file.</span></span>

## <a name="run-hello-application"></a><span data-ttu-id="27a0d-182">Spuštění aplikace hello</span><span class="sxs-lookup"><span data-stu-id="27a0d-182">Run hello application</span></span>

<span data-ttu-id="27a0d-183">Má trvat přibližně pět minut, než tato toorun aplikace konzoly zcela od počáteční toofinish.</span><span class="sxs-lookup"><span data-stu-id="27a0d-183">It should take about five minutes for this console application toorun completely from start toofinish.</span></span>

1. <span data-ttu-id="27a0d-184">toorun hello aplikace, použijte tento příkaz Maven:</span><span class="sxs-lookup"><span data-stu-id="27a0d-184">toorun hello application, use this Maven command:</span></span>

    ```
    mvn compile exec:java
    ```

2. <span data-ttu-id="27a0d-185">Před stisknutím klávesy **Enter** toostart odstraňování prostředky, vám může trvat několik minut tooverify hello vytváření prostředků hello v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="27a0d-185">Before you press **Enter** toostart deleting resources, you could take a few minutes tooverify hello creation of hello resources in hello Azure portal.</span></span> <span data-ttu-id="27a0d-186">Klikněte na tlačítko hello nasazení toosee informace o stavu hello nasazení.</span><span class="sxs-lookup"><span data-stu-id="27a0d-186">Click hello deployment status toosee information about hello deployment.</span></span>


## <a name="next-steps"></a><span data-ttu-id="27a0d-187">Další kroky</span><span class="sxs-lookup"><span data-stu-id="27a0d-187">Next steps</span></span>
* <span data-ttu-id="27a0d-188">Další informace o používání hello [knihovny Azure libraries for Java](https://docs.microsoft.com/en-us/java/azure/java-sdk-azure-overview).</span><span class="sxs-lookup"><span data-stu-id="27a0d-188">Learn more about using hello [Azure libraries for Java](https://docs.microsoft.com/en-us/java/azure/java-sdk-azure-overview).</span></span>

