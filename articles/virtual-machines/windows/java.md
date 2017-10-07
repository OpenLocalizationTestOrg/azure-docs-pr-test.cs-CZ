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
# <a name="create-and-manage-windows-vms-in-azure-using-java"></a>Vytvářet a spravovat virtuální počítače Windows v Azure pomocí Java

[Virtuální počítač Azure](overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (VM) vyžaduje několik podpůrných prostředků Azure. Tento článek popisuje vytváření, správu a odstraňování prostředky virtuálních počítačů pomocí Java. Získáte informace o těchto tématech:

> [!div class="checklist"]
> * Vytvořte projekt Maven
> * Přidat závislosti
> * Vytvořit přihlašovací údaje
> * Vytvoření prostředků
> * Provádění úloh správy
> * Odstraňte prostředky
> * Spuštění aplikace hello

Trvá přibližně 20 minut toodo tyto kroky.

## <a name="create-a-maven-project"></a>Vytvořte projekt Maven

1. Pokud jste tak již neučinili, nainstalujte [Java](http://www.oracle.com/technetwork/java/javase/downloads/index.html).
2. Nainstalujte [Maven](http://maven.apache.org/download.cgi).
3. Vytvoření nového projektu složku a hello:
    
    ```
    mkdir java-azure-test
    cd java-azure-test
    
    mvn archetype:generate -DgroupId=com.fabrikam -DartifactId=testAzureApp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

## <a name="add-dependencies"></a>Přidat závislosti

1. V části hello `testAzureApp` složku, otevřete hello `pom.xml` souboru a přidejte konfigurace sestavení hello příliš&lt;projektu&gt; tooenable hello sestavení vaší aplikace:

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

2. Přidá hello závislosti, které jsou potřebné tooaccess hello Azure Java SDK.

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

3. Uložte soubor hello.

## <a name="create-credentials"></a>Vytvořit přihlašovací údaje

Než začnete tento krok, ujistěte se, zda máte přístup tooan [objektu služby Active Directory](../../azure-resource-manager/resource-group-create-service-principal-portal.md). Také byste měli zaznamenat ID aplikace hello hello ověřovací klíč a hello ID klienta, který budete potřebovat později.

### <a name="create-hello-authorization-file"></a>Vytvoření souboru autorizace hello

1. Vytvořte soubor s názvem `azureauth.properties` a přidejte tyto vlastnosti tooit:

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

    Nahraďte  **&lt;id předplatného&gt;**  s ID vašeho předplatného  **&lt;id aplikace&gt;**  s hello aplikace Active Directory identifikátor,  **&lt;ověřovací klíč&gt;**  klíčem aplikace hello a  **&lt;id klienta&gt;**  s klientem hello identifikátor.

2. Uložte soubor hello.
3. Nastavte proměnnou prostředí s názvem AZURE_AUTH_LOCATION ve vašem prostředí s hello úplná cesta toohello ověřování souboru.

### <a name="create-hello-management-client"></a>Vytvoření klienta pro správu hello

1. Otevřete hello `App.java` souboru pod `src\main\java\com\fabrikam` a zajistěte, aby tento příkaz balíček je v horní části hello:

    ```java
    package com.fabrikam.testAzureApp;
    ```

2. V části prohlášení hello balíčku, přidejte tyto příkazy importu:
   
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

2. pověření služby Active Directory hello toocreate, je nutné, aby toomake požadavků, přidejte tento kód toohello hlavní metodu hello – třída aplikace:
   
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

## <a name="create-resources"></a>Vytvoření prostředků

### <a name="create-hello-resource-group"></a>Vytvořte skupinu prostředků hello

Musí být všechny prostředky obsažené v [skupiny prostředků](../../azure-resource-manager/resource-group-overview.md).

toospecify hodnoty pro hello aplikace a vytvořte skupinu prostředků hello, přidejte tento kód toohello zkuste blok v hlavní metoda hello:

```java
System.out.println("Creating resource group...");
ResourceGroup resourceGroup = azure.resourceGroups()
    .define("myResourceGroup")
    .withRegion(Region.US_EAST)
    .create();
```

### <a name="create-hello-availability-set"></a>Vytvořit skupinu dostupnosti hello

[Skupiny dostupnosti](tutorial-availability-sets.md) bylo snazší pro vás toomaintain hello virtuální počítače používané vaší aplikace.

dostupnost hello toocreate nastavit, přidejte tento kód toohello zkuste blok v hlavní metoda hello:

```java
System.out.println("Creating availability set...");
AvailabilitySet availabilitySet = azure.availabilitySets()
    .define("myAvailabilitySet")
    .withRegion(Region.US_EAST)
    .withExistingResourceGroup("myResourceGroup")
    .withSku(AvailabilitySetSkuTypes.MANAGED)
    .create();
```
### <a name="create-hello-public-ip-address"></a>Vytvoření veřejné IP adresy hello

A [veřejnou IP adresu](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) je potřebné toocommunicate s hello virtuálního počítače.

toocreate hello veřejnou IP adresu pro virtuální počítač hello, přidejte tento kód toohello zkuste blok v hlavní metoda hello:

```java
System.out.println("Creating public IP address...");
PublicIPAddress publicIPAddress = azure.publicIPAddresses()
    .define("myPublicIP")
    .withRegion(Region.US_EAST)
    .withExistingResourceGroup("myResourceGroup")
    .withDynamicIP()
    .create();
```

### <a name="create-hello-virtual-network"></a>Vytvoření virtuální sítě hello

Virtuální počítač musí být v podsíti [virtuální síť](../../virtual-network/virtual-networks-overview.md).

toocreate a podsítě a virtuální síť, přidejte tento blok kódu toohello zkuste hello hlavní metoda:

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

### <a name="create-hello-network-interface"></a>Vytvoření hello síťové rozhraní

Virtuální počítač vyžaduje toocommunicate rozhraní sítě ve virtuální síti hello.

toocreate síťového rozhraní, přidejte tento kód toohello zkuste blok v hlavní metoda hello:

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

### <a name="create-hello-virtual-machine"></a>Vytvoření virtuálního počítače hello

Teď, když jste vytvořili všechny hello Podpora prostředků, můžete vytvořit virtuální počítač.

toocreate hello virtuálního počítače, přidejte tento kód toohello zkuste blok v hlavní metoda hello:

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
> V tomto kurzu vytvoří virtuální počítač s verzí operačního systému Windows Server hello. toolearn Další informace o výběru ostatní Image, najdete v části [vyhledání a výběr imagí virtuálních počítačů Azure pomocí prostředí Windows PowerShell a rozhraní příkazového řádku Azure hello](../linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
> 
>

Pokud chcete toouse existující disk místo image pořízenou prostřednictvím marketplace, použijte tento kód: 

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

## <a name="perform-management-tasks"></a>Provádění úloh správy

Během životního cyklu hello virtuálního počítače můžete úlohy správy toorun například spuštění, zastavení nebo odstranění virtuálního počítače. Kromě toho můžete toocreate kód tooautomate opakovaných nebo komplexní úlohy.

Pokud budete potřebovat toodo nic s hello virtuálních počítačů, je potřeba tooget její instanci. Přidejte tento blok kódu toohello zkuste hello hlavní metody:

```java
VirtualMachine vm = azure.virtualMachines().getByResourceGroup("myResourceGroup", "myVM");
```

### <a name="get-information-about-hello-vm"></a>Získání informací o hello virtuálních počítačů

tooget informace o virtuálním počítači hello, přidejte tento kód toohello zkuste blok v hlavní metoda hello:

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

### <a name="stop-hello-vm"></a>Zastavit hello virtuálních počítačů

Můžete zastavit virtuální počítač a ponechat jeho nastavení, ale pokračovat toobe účtovat pro něj nebo můžete zastavit virtuální počítač a jeho navrácení. Při zrušení jeho přidělení virtuálního počítače jsou končí deallocated a fakturace pro ni také všechny prostředky, které jsou s ním spojená.

toostop hello virtuální počítač bez rušení přidělení, přidejte tento kód toohello zkuste blok v hlavní metoda hello:

```java
System.out.println("Stopping vm...");
vm.powerOff();
System.out.println("Press enter toocontinue...");
input.nextLine();
```

Pokud chcete toodeallocate hello virtuální počítač, změňte hello PowerOff volání toothis kódu:

```java
vm.deallocate();
```

### <a name="start-hello-vm"></a>Hello spuštění virtuálního počítače

toostart hello virtuálního počítače, přidejte tento kód toohello zkuste blok v hlavní metoda hello:

```java
System.out.println("Starting vm...");
vm.start();
System.out.println("Press enter toocontinue...");
input.nextLine();
```

### <a name="resize-hello-vm"></a>Změnit velikost hello virtuálních počítačů

Mnoho aspektů nasazení měli zvážit při rozhodování o velikosti pro virtuální počítač. Další informace najdete v tématu [velikosti virtuálních počítačů](sizes.md).  

velikost toochange hello virtuálního počítače, přidejte tento blok kódu toohello zkuste hlavní metoda hello:

```java
System.out.println("Resizing vm...");
vm.update()
    .withSize(VirtualMachineSizeTypes.STANDARD_DS2)
    .apply();
System.out.println("Press enter toocontinue...");
input.nextLine();
```

### <a name="add-a-data-disk-toohello-vm"></a>Přidání disku toohello data virtuálního počítače

tooadd datového disku toohello virtuální počítač, který je 2 GB velikost, má logické jednotky 0 a typu ukládání do mezipaměti v režimu ReadWrite, přidejte tento kód toohello zkuste blok v hlavní metoda hello:

```java
System.out.println("Adding data disk...");
vm.update()
    .withNewDataDisk(2, 0, CachingTypes.READ_WRITE)
    .apply();
System.out.println("Press enter toodelete resources...");
input.nextLine();
```

## <a name="delete-resources"></a>Odstraňte prostředky

Vzhledem k tomu, že se vám účtovat prostředky využívané v Azure, vždycky je dobrým zvykem toodelete prostředky, které už nejsou potřeba. Pokud chcete, aby toodelete hello virtuální počítače a všechny hello podpora prostředky, všechny máte toodo je odstranit skupinu prostředků hello.

1. toodelete hello prostředků skupiny, přidejte tento kód toohello zkuste blok v hlavní metoda hello:
   
```java
System.out.println("Deleting resources...");
azure.resourceGroups().deleteByName("myResourceGroup");
```

2. Uložte soubor App.java hello.

## <a name="run-hello-application"></a>Spuštění aplikace hello

Má trvat přibližně pět minut, než tato toorun aplikace konzoly zcela od počáteční toofinish.

1. toorun hello aplikace, použijte tento příkaz Maven:

    ```
    mvn compile exec:java
    ```

2. Před stisknutím klávesy **Enter** toostart odstraňování prostředky, vám může trvat několik minut tooverify hello vytváření prostředků hello v hello portálu Azure. Klikněte na tlačítko hello nasazení toosee informace o stavu hello nasazení.


## <a name="next-steps"></a>Další kroky
* Další informace o používání hello [knihovny Azure libraries for Java](https://docs.microsoft.com/en-us/java/azure/java-sdk-azure-overview).

