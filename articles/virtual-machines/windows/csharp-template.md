---
title: "aaaDeploy a virtuálního počítače pomocí C# a šablony Resource Manageru | Microsoft Docs"
description: "Zjistěte, toohow toouse C# a toodeploy správce prostředků šablony virtuálního počítače Azure."
services: virtual-machines-windows
documentationcenter: 
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: bfba66e8-c923-4df2-900a-0c2643b81240
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: davidmu
ms.openlocfilehash: 91d470228cfeed72336b488ffef4dfbb25bc3a40
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-an-azure-virtual-machine-using-c-and-a-resource-manager-template"></a>Nasazení virtuálního počítače Azure pomocí jazyka C# a šablony Resource Manageru
Tento článek ukazuje, jak toodeploy šablonu Azure Resource Manager pomocí jazyka C#. Hello šablonu, kterou vytvoříte nasadí jednoho virtuálního počítače se systémem Windows Server v nové virtuální sítě s jedinou podsítí.

Podrobný popis hello prostředek virtuálního počítače naleznete v tématu [virtuálních počítačů v šablonu Azure Resource Manager](template-description.md). Další informace o všech hello prostředcích v šabloně najdete v tématu [názorný Průvodce šablonou Azure Resource Manager](../../azure-resource-manager/resource-manager-template-walkthrough.md).

Trvá přibližně 10 minut toodo tyto kroky.

## <a name="create-a-visual-studio-project"></a>Vytvoření projektu ve Visual Studiu

V tomto kroku Ujistěte se, že je nainstalován nástroj Visual Studio a vytvoříte šablonu konzola aplikace používá toodeploy hello.

1. Pokud jste to ještě neudělali, nainstalujte [Visual Studio](https://docs.microsoft.com/visualstudio/install/install-visual-studio). Vyberte **vývoj aplikací .NET** na hello zatížení stránky a potom klikněte na **nainstalovat**. V hello souhrn, můžete uvidíte, že **nástrojů pro vývoj řešení pro rozhraní .NET Framework 4 4.6** je automaticky vybrána pro vás. Pokud jste již nainstalovali Visual Studio, můžete přidat hello .NET zatížení pomocí hello Spouštěče Visual Studio.
2. V sadě Visual Studio, klikněte na tlačítko **soubor** > **nový** > **projektu**.
3. V **šablony** > **Visual C#**, vyberte **konzolovou aplikaci (rozhraní .NET Framework)**, zadejte *myDotnetProject* pro název hello Hello projekt, vyberte hello umístění projektu hello a potom klikněte na **OK**.

## <a name="install-hello-packages"></a>Instalovat balíčky hello

Balíčky NuGet jsou hello nejjednodušší způsob, jak tooinstall hello knihovny, je nutné toofinish tyto kroky. tooget hello knihovny, které je nutné v sadě Visual Studio, proveďte tyto kroky:

1. Klikněte na tlačítko **nástroje** > **Správce balíčků Nuget**a potom klikněte na **Konzola správce balíčků**.
2. V konzole hello zadejte tyto příkazy:

    ```
    Install-Package Microsoft.Azure.Management.Fluent
    Install-Package WindowsAzure.Storage
    ```

## <a name="create-hello-files"></a>Vytvoření souborů hello

V tomto kroku vytvoříte soubor šablony, která nasazuje hello prostředky a soubor parametrů, který poskytuje šablony toohello hodnoty parametru. Můžete také vytvořit soubor autorizace, který je použité tooperform Azure Resource Manager operace.

### <a name="create-hello-template-file"></a>Vytvoření souboru šablony hello

1. V Průzkumníku řešení klikněte pravým tlačítkem na *myDotnetProject* > **přidat** > **nová položka**a potom vyberte **textový soubor** v *Visual C# položky*. Název souboru hello *CreateVMTemplate.json*a potom klikněte na **přidat**.
2. Přidejte tento soubor toohello kódu JSON, který jste vytvořili:

    ```json
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "adminUsername": { "type": "string" },
        "adminPassword": { "type": "securestring" }
      },
      "variables": {
        "vnetID": "[resourceId('Microsoft.Network/virtualNetworks','myVNet')]", 
        "subnetRef": "[concat(variables('vnetID'),'/subnets/mySubnet')]", 
      },
      "resources": [
        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/publicIPAddresses",
          "name": "myPublicIPAddress",
          "location": "[resourceGroup().location]",
          "properties": {
            "publicIPAllocationMethod": "Dynamic",
            "dnsSettings": {
              "domainNameLabel": "myresourcegroupdns1"
            }
          }
        },
        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/virtualNetworks",
          "name": "myVNet",
          "location": "[resourceGroup().location]",
          "properties": {
            "addressSpace": { "addressPrefixes": [ "10.0.0.0/16" ] },
            "subnets": [
              {
                "name": "mySubnet",
                "properties": { "addressPrefix": "10.0.0.0/24" }
              }
            ]
          }
        },
        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/networkInterfaces",
          "name": "myNic",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[resourceId('Microsoft.Network/publicIPAddresses/', 'myPublicIPAddress')]",
            "[resourceId('Microsoft.Network/virtualNetworks/', 'myVNet')]"
          ],
          "properties": {
            "ipConfigurations": [
              {
                "name": "ipconfig1",
                "properties": {
                  "privateIPAllocationMethod": "Dynamic",
                  "publicIPAddress": { "id": "[resourceId('Microsoft.Network/publicIPAddresses','myPublicIPAddress')]" },
                  "subnet": { "id": "[variables('subnetRef')]" }
                }
              }
            ]
          }
        },
        {
          "apiVersion": "2016-04-30-preview",
          "type": "Microsoft.Compute/virtualMachines",
          "name": "myVM",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[resourceId('Microsoft.Network/networkInterfaces/', 'myNic')]"
          ],
          "properties": {
            "hardwareProfile": { "vmSize": "Standard_DS1" },
            "osProfile": {
              "computerName": "myVM",
              "adminUsername": "[parameters('adminUsername')]",
              "adminPassword": "[parameters('adminPassword')]"
            },
            "storageProfile": {
              "imageReference": {
                "publisher": "MicrosoftWindowsServer",
                "offer": "WindowsServer",
                "sku": "2012-R2-Datacenter",
                "version": "latest"
              },
              "osDisk": {
                "name": "myManagedOSDisk",
                "caching": "ReadWrite",
                "createOption": "FromImage"
              }
            },
            "networkProfile": {
              "networkInterfaces": [
                {
                  "id": "[resourceId('Microsoft.Network/networkInterfaces','myNic')]"
                }
              ]
            }
          }
        }
      ]
    }
    ```

3. Uložte soubor CreateVMTemplate.json hello.

### <a name="create-hello-parameters-file"></a>Vytvoření souboru parametrů hello

toospecify hodnoty pro parametry hello prostředků, které jsou definované v šabloně hello, vytvoříte soubor parametrů, který obsahuje hodnoty hello.

1. V Průzkumníku řešení klikněte pravým tlačítkem na *myDotnetProject* > **přidat** > **nová položka**a potom vyberte **textový soubor** v *Visual C# položky*. Název souboru hello *Parameters.JSON tímto kódem*a potom klikněte na **přidat**.
2. Přidejte tento soubor toohello kódu JSON, který jste vytvořili:

    ```json
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "adminUserName": { "value": "azureuser" },
        "adminPassword": { "value": "Azure12345678" }
      }
    }
    ```

4. Uložení souboru parameters.JSON tímto kódem hello.

### <a name="create-hello-authorization-file"></a>Vytvoření souboru autorizace hello

Než bude možné nasadit šablonu, ujistěte se, zda máte přístup tooan [objektu služby Active Directory](../../resource-group-authenticate-service-principal.md). Z hello instanční objekt můžete získat token pro ověřování požadavků tooAzure Resource Manager. Také byste měli zaznamenat ID aplikace hello hello ověřovací klíč a hello ID klienta, které je třeba v souboru autorizace hello.

1. V Průzkumníku řešení klikněte pravým tlačítkem na *myDotnetProject* > **přidat** > **nová položka**a potom vyberte **textový soubor** v *Visual C# položky*. Název souboru hello *azureauth.properties*a potom klikněte na **přidat**.
2. Přidejte tyto vlastnosti autorizace:

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

3. Uložte soubor azureauth.properties hello.
4. Nastavit proměnnou prostředí v systému Windows s názvem AZURE_AUTH_LOCATION s hello úplná cesta tooauthorization soubor, který jste vytvořili, například hello následující příkaz lze použít PowerShell:

    ```
    [Environment]::SetEnvironmentVariable("AZURE_AUTH_LOCATION", "C:\Visual Studio 2017\Projects\myDotnetProject\myDotnetProject\azureauth.properties", "User")
    ```
    
## <a name="create-hello-management-client"></a>Vytvoření klienta pro správu hello

1. Otevřete soubor Program.cs hello pro hello projekt, který jste vytvořili a pak přidejte tyto příkazy toohello existující příkazy using v horní části souboru hello:

    ```
    using Microsoft.Azure.Management.Compute.Fluent;
    using Microsoft.Azure.Management.Compute.Fluent.Models;
    using Microsoft.Azure.Management.Fluent;
    using Microsoft.Azure.Management.ResourceManager.Fluent;
    using Microsoft.Azure.Management.ResourceManager.Fluent.Core;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```

2. Klient správy hello toocreate, přidejte tento kód toohello metodu Main:

    ```
    var credentials = SdkContext.AzureCredentialsFactory
        .FromFile(Environment.GetEnvironmentVariable("AZURE_AUTH_LOCATION"));

    var azure = Azure
        .Configure()
        .WithLogLevel(HttpLoggingDelegatingHandler.Level.Basic)
        .Authenticate(credentials)
        .WithDefaultSubscription();
    ```

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

toospecify hodnoty pro aplikace hello, přidejte metodu Main toohello kódu:

```
var groupName = "myResourceGroup";
var location = Region.USWest;

var resourceGroup = azure.ResourceGroups.Define(groupName)
    .WithRegion(location)
    .Create();
```

## <a name="create-a-storage-account"></a>vytvořit účet úložiště

Šablona Hello a parametry jsou nasazeny v účtu úložiště v Azure. V tomto kroku vytvoříte účet hello a nahrání souborů hello. 

toocreate hello účet, přidejte tento kód toohello metodu Main:

```
string storageAccountName = SdkContext.RandomResourceName("st", 10);

Console.WriteLine("Creating storage account...");
var storage = azure.StorageAccounts.Define(storageAccountName)
    .WithRegion(Region.USWest)
    .WithExistingResourceGroup(resourceGroup)
    .Create();

var storageKeys = storage.GetKeys();
string storageConnectionString = "DefaultEndpointsProtocol=https;"
    + "AccountName=" + storage.Name
    + ";AccountKey=" + storageKeys[0].Value
    + ";EndpointSuffix=core.windows.net";

var account = CloudStorageAccount.Parse(storageConnectionString);
var serviceClient = account.CreateCloudBlobClient();

Console.WriteLine("Creating container...");
var container = serviceClient.GetContainerReference("templates");
container.CreateIfNotExistsAsync().Wait();
var containerPermissions = new BlobContainerPermissions()
    { PublicAccess = BlobContainerPublicAccessType.Container };
container.SetPermissionsAsync(containerPermissions).Wait();

Console.WriteLine("Uploading template file...");
var templateblob = container.GetBlockBlobReference("CreateVMTemplate.json");
templateblob.UploadFromFile("..\\..\\CreateVMTemplate.json");

Console.WriteLine("Uploading parameters file...");
var paramblob = container.GetBlockBlobReference("Parameters.json");
paramblob.UploadFromFile("..\\..\\Parameters.json");
```

## <a name="deploy-hello-template"></a>Nasazení šablony hello

Nasazení šablony hello a parametry z hello účet úložiště, který byl vytvořen. 

Šablona hello toodeploy, přidejte tento kód toohello metodu Main:

```
var templatePath = "https://" + storageAccountName + ".blob.core.windows.net/templates/CreateVMTemplate.json";
var paramPath = "https://" + storageAccountName + ".blob.core.windows.net/templates/Parameters.json";
var deployment = azure.Deployments.Define("myDeployment")
    .WithExistingResourceGroup(groupName)
    .WithTemplateLink(templatePath, "1.0.0.0")
    .WithParametersLink(paramPath, "1.0.0.0")
    .WithMode(Microsoft.Azure.Management.ResourceManager.Fluent.Models.DeploymentMode.Incremental)
    .Create();
Console.WriteLine("Press enter toodelete hello resource group...");
Console.ReadLine();
```

## <a name="delete-hello-resources"></a>Odstraňte prostředky hello

Vzhledem k tomu, že se vám účtovat prostředky využívané v Azure, vždycky je dobrým zvykem toodelete prostředky, které už nejsou potřeba. Nepotřebujete toodelete každého prostředku nezávisle na skupinu prostředků. Odstranit skupinu prostředků hello a všechny její prostředky se automaticky odstraní. 

toodelete hello prostředků skupiny, přidejte tento kód toohello metodu Main:

```
azure.ResourceGroups.DeleteByName(groupName);
```

## <a name="run-hello-application"></a>Spuštění aplikace hello

Má trvat přibližně pět minut, než tato toorun aplikace konzoly zcela od počáteční toofinish. 

1. toorun hello konzolovou aplikaci, klikněte na tlačítko **spustit**.

2. Před stisknutím klávesy **Enter** toostart odstraňování prostředky, vám může trvat několik minut tooverify hello vytváření prostředků hello v hello portálu Azure. Klikněte na tlačítko hello nasazení toosee informace o stavu hello nasazení.

## <a name="next-steps"></a>Další kroky
* Pokud byly nějaké problémy s hello nasazení, je dalším krokem bude toolook na [odstraňování běžných chyb nasazení Azure pomocí Azure Resource Manageru](../../resource-manager-common-deployment-errors.md).
* Zjistěte, jak toodeploy virtuální počítač a jeho podpůrné prostředky kontrolou [nasazení služby Azure virtuálního počítače pomocí jazyka C#](csharp.md).
