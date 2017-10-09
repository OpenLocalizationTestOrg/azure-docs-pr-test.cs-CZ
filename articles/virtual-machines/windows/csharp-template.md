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
# <a name="deploy-an-azure-virtual-machine-using-c-and-a-resource-manager-template"></a><span data-ttu-id="5eea6-103">Nasazení virtuálního počítače Azure pomocí jazyka C# a šablony Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="5eea6-103">Deploy an Azure Virtual Machine using C# and a Resource Manager template</span></span>
<span data-ttu-id="5eea6-104">Tento článek ukazuje, jak toodeploy šablonu Azure Resource Manager pomocí jazyka C#.</span><span class="sxs-lookup"><span data-stu-id="5eea6-104">This article shows you how toodeploy an Azure Resource Manager template using C#.</span></span> <span data-ttu-id="5eea6-105">Hello šablonu, kterou vytvoříte nasadí jednoho virtuálního počítače se systémem Windows Server v nové virtuální sítě s jedinou podsítí.</span><span class="sxs-lookup"><span data-stu-id="5eea6-105">hello template that you create deploys a single virtual machine running Windows Server in a new virtual network with a single subnet.</span></span>

<span data-ttu-id="5eea6-106">Podrobný popis hello prostředek virtuálního počítače naleznete v tématu [virtuálních počítačů v šablonu Azure Resource Manager](template-description.md).</span><span class="sxs-lookup"><span data-stu-id="5eea6-106">For a detailed description of hello virtual machine resource, see [Virtual machines in an Azure Resource Manager template](template-description.md).</span></span> <span data-ttu-id="5eea6-107">Další informace o všech hello prostředcích v šabloně najdete v tématu [názorný Průvodce šablonou Azure Resource Manager](../../azure-resource-manager/resource-manager-template-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="5eea6-107">For more information about all hello resources in a template, see [Azure Resource Manager template walkthrough](../../azure-resource-manager/resource-manager-template-walkthrough.md).</span></span>

<span data-ttu-id="5eea6-108">Trvá přibližně 10 minut toodo tyto kroky.</span><span class="sxs-lookup"><span data-stu-id="5eea6-108">It takes about 10 minutes toodo these steps.</span></span>

## <a name="create-a-visual-studio-project"></a><span data-ttu-id="5eea6-109">Vytvoření projektu ve Visual Studiu</span><span class="sxs-lookup"><span data-stu-id="5eea6-109">Create a Visual Studio project</span></span>

<span data-ttu-id="5eea6-110">V tomto kroku Ujistěte se, že je nainstalován nástroj Visual Studio a vytvoříte šablonu konzola aplikace používá toodeploy hello.</span><span class="sxs-lookup"><span data-stu-id="5eea6-110">In this step, you make sure that Visual Studio is installed and you create a console application used toodeploy hello template.</span></span>

1. <span data-ttu-id="5eea6-111">Pokud jste to ještě neudělali, nainstalujte [Visual Studio](https://docs.microsoft.com/visualstudio/install/install-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="5eea6-111">If you haven't already, install [Visual Studio](https://docs.microsoft.com/visualstudio/install/install-visual-studio).</span></span> <span data-ttu-id="5eea6-112">Vyberte **vývoj aplikací .NET** na hello zatížení stránky a potom klikněte na **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="5eea6-112">Select **.NET desktop development** on hello Workloads page, and then click **Install**.</span></span> <span data-ttu-id="5eea6-113">V hello souhrn, můžete uvidíte, že **nástrojů pro vývoj řešení pro rozhraní .NET Framework 4 4.6** je automaticky vybrána pro vás.</span><span class="sxs-lookup"><span data-stu-id="5eea6-113">In hello summary, you can see that **.NET Framework 4 - 4.6 development tools** is automatically selected for you.</span></span> <span data-ttu-id="5eea6-114">Pokud jste již nainstalovali Visual Studio, můžete přidat hello .NET zatížení pomocí hello Spouštěče Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5eea6-114">If you have already installed Visual Studio, you can add hello .NET workload using hello Visual Studio Launcher.</span></span>
2. <span data-ttu-id="5eea6-115">V sadě Visual Studio, klikněte na tlačítko **soubor** > **nový** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="5eea6-115">In Visual Studio, click **File** > **New** > **Project**.</span></span>
3. <span data-ttu-id="5eea6-116">V **šablony** > **Visual C#**, vyberte **konzolovou aplikaci (rozhraní .NET Framework)**, zadejte *myDotnetProject* pro název hello Hello projekt, vyberte hello umístění projektu hello a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="5eea6-116">In **Templates** > **Visual C#**, select **Console App (.NET Framework)**, enter *myDotnetProject* for hello name of hello project, select hello location of hello project, and then click **OK**.</span></span>

## <a name="install-hello-packages"></a><span data-ttu-id="5eea6-117">Instalovat balíčky hello</span><span class="sxs-lookup"><span data-stu-id="5eea6-117">Install hello packages</span></span>

<span data-ttu-id="5eea6-118">Balíčky NuGet jsou hello nejjednodušší způsob, jak tooinstall hello knihovny, je nutné toofinish tyto kroky.</span><span class="sxs-lookup"><span data-stu-id="5eea6-118">NuGet packages are hello easiest way tooinstall hello libraries that you need toofinish these steps.</span></span> <span data-ttu-id="5eea6-119">tooget hello knihovny, které je nutné v sadě Visual Studio, proveďte tyto kroky:</span><span class="sxs-lookup"><span data-stu-id="5eea6-119">tooget hello libraries that you need in Visual Studio, do these steps:</span></span>

1. <span data-ttu-id="5eea6-120">Klikněte na tlačítko **nástroje** > **Správce balíčků Nuget**a potom klikněte na **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="5eea6-120">Click **Tools** > **Nuget Package Manager**, and then click **Package Manager Console**.</span></span>
2. <span data-ttu-id="5eea6-121">V konzole hello zadejte tyto příkazy:</span><span class="sxs-lookup"><span data-stu-id="5eea6-121">Type these commands in hello console:</span></span>

    ```
    Install-Package Microsoft.Azure.Management.Fluent
    Install-Package WindowsAzure.Storage
    ```

## <a name="create-hello-files"></a><span data-ttu-id="5eea6-122">Vytvoření souborů hello</span><span class="sxs-lookup"><span data-stu-id="5eea6-122">Create hello files</span></span>

<span data-ttu-id="5eea6-123">V tomto kroku vytvoříte soubor šablony, která nasazuje hello prostředky a soubor parametrů, který poskytuje šablony toohello hodnoty parametru.</span><span class="sxs-lookup"><span data-stu-id="5eea6-123">In this step, you create a template file that deploys hello resources and a parameters file that supplies parameter values toohello template.</span></span> <span data-ttu-id="5eea6-124">Můžete také vytvořit soubor autorizace, který je použité tooperform Azure Resource Manager operace.</span><span class="sxs-lookup"><span data-stu-id="5eea6-124">You also create an authorization file that is used tooperform Azure Resource Manager operations.</span></span>

### <a name="create-hello-template-file"></a><span data-ttu-id="5eea6-125">Vytvoření souboru šablony hello</span><span class="sxs-lookup"><span data-stu-id="5eea6-125">Create hello template file</span></span>

1. <span data-ttu-id="5eea6-126">V Průzkumníku řešení klikněte pravým tlačítkem na *myDotnetProject* > **přidat** > **nová položka**a potom vyberte **textový soubor** v *Visual C# položky*.</span><span class="sxs-lookup"><span data-stu-id="5eea6-126">In Solution Explorer, right-click *myDotnetProject* > **Add** > **New Item**, and then select **Text File** in *Visual C# Items*.</span></span> <span data-ttu-id="5eea6-127">Název souboru hello *CreateVMTemplate.json*a potom klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="5eea6-127">Name hello file *CreateVMTemplate.json*, and then click **Add**.</span></span>
2. <span data-ttu-id="5eea6-128">Přidejte tento soubor toohello kódu JSON, který jste vytvořili:</span><span class="sxs-lookup"><span data-stu-id="5eea6-128">Add this JSON code toohello file that you created:</span></span>

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

3. <span data-ttu-id="5eea6-129">Uložte soubor CreateVMTemplate.json hello.</span><span class="sxs-lookup"><span data-stu-id="5eea6-129">Save hello CreateVMTemplate.json file.</span></span>

### <a name="create-hello-parameters-file"></a><span data-ttu-id="5eea6-130">Vytvoření souboru parametrů hello</span><span class="sxs-lookup"><span data-stu-id="5eea6-130">Create hello parameters file</span></span>

<span data-ttu-id="5eea6-131">toospecify hodnoty pro parametry hello prostředků, které jsou definované v šabloně hello, vytvoříte soubor parametrů, který obsahuje hodnoty hello.</span><span class="sxs-lookup"><span data-stu-id="5eea6-131">toospecify values for hello resource parameters that are defined in hello template, you create a parameters file that contains hello values.</span></span>

1. <span data-ttu-id="5eea6-132">V Průzkumníku řešení klikněte pravým tlačítkem na *myDotnetProject* > **přidat** > **nová položka**a potom vyberte **textový soubor** v *Visual C# položky*.</span><span class="sxs-lookup"><span data-stu-id="5eea6-132">In Solution Explorer, right-click *myDotnetProject* > **Add** > **New Item**, and then select **Text File** in *Visual C# Items*.</span></span> <span data-ttu-id="5eea6-133">Název souboru hello *Parameters.JSON tímto kódem*a potom klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="5eea6-133">Name hello file *Parameters.json*, and then click **Add**.</span></span>
2. <span data-ttu-id="5eea6-134">Přidejte tento soubor toohello kódu JSON, který jste vytvořili:</span><span class="sxs-lookup"><span data-stu-id="5eea6-134">Add this JSON code toohello file that you created:</span></span>

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

4. <span data-ttu-id="5eea6-135">Uložení souboru parameters.JSON tímto kódem hello.</span><span class="sxs-lookup"><span data-stu-id="5eea6-135">Save hello Parameters.json file.</span></span>

### <a name="create-hello-authorization-file"></a><span data-ttu-id="5eea6-136">Vytvoření souboru autorizace hello</span><span class="sxs-lookup"><span data-stu-id="5eea6-136">Create hello authorization file</span></span>

<span data-ttu-id="5eea6-137">Než bude možné nasadit šablonu, ujistěte se, zda máte přístup tooan [objektu služby Active Directory](../../resource-group-authenticate-service-principal.md).</span><span class="sxs-lookup"><span data-stu-id="5eea6-137">Before you can deploy a template, make sure that you have access tooan [Active Directory service principal](../../resource-group-authenticate-service-principal.md).</span></span> <span data-ttu-id="5eea6-138">Z hello instanční objekt můžete získat token pro ověřování požadavků tooAzure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="5eea6-138">From hello service principal, you acquire a token for authenticating requests tooAzure Resource Manager.</span></span> <span data-ttu-id="5eea6-139">Také byste měli zaznamenat ID aplikace hello hello ověřovací klíč a hello ID klienta, které je třeba v souboru autorizace hello.</span><span class="sxs-lookup"><span data-stu-id="5eea6-139">You should also record hello application ID, hello authentication key, and hello tenant ID that you need in hello authorization file.</span></span>

1. <span data-ttu-id="5eea6-140">V Průzkumníku řešení klikněte pravým tlačítkem na *myDotnetProject* > **přidat** > **nová položka**a potom vyberte **textový soubor** v *Visual C# položky*.</span><span class="sxs-lookup"><span data-stu-id="5eea6-140">In Solution Explorer, right-click *myDotnetProject* > **Add** > **New Item**, and then select **Text File** in *Visual C# Items*.</span></span> <span data-ttu-id="5eea6-141">Název souboru hello *azureauth.properties*a potom klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="5eea6-141">Name hello file *azureauth.properties*, and then click **Add**.</span></span>
2. <span data-ttu-id="5eea6-142">Přidejte tyto vlastnosti autorizace:</span><span class="sxs-lookup"><span data-stu-id="5eea6-142">Add these authorization properties:</span></span>

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

    <span data-ttu-id="5eea6-143">Nahraďte  **&lt;id předplatného&gt;**  s ID vašeho předplatného  **&lt;id aplikace&gt;**  s hello aplikace Active Directory identifikátor,  **&lt;ověřovací klíč&gt;**  klíčem aplikace hello a  **&lt;id klienta&gt;**  s klientem hello identifikátor.</span><span class="sxs-lookup"><span data-stu-id="5eea6-143">Replace **&lt;subscription-id&gt;** with your subscription identifier, **&lt;application-id&gt;** with hello Active Directory application identifier, **&lt;authentication-key&gt;** with hello application key, and **&lt;tenant-id&gt;** with hello tenant identifier.</span></span>

3. <span data-ttu-id="5eea6-144">Uložte soubor azureauth.properties hello.</span><span class="sxs-lookup"><span data-stu-id="5eea6-144">Save hello azureauth.properties file.</span></span>
4. <span data-ttu-id="5eea6-145">Nastavit proměnnou prostředí v systému Windows s názvem AZURE_AUTH_LOCATION s hello úplná cesta tooauthorization soubor, který jste vytvořili, například hello následující příkaz lze použít PowerShell:</span><span class="sxs-lookup"><span data-stu-id="5eea6-145">Set an environment variable in Windows named AZURE_AUTH_LOCATION with hello full path tooauthorization file that you created, for example hello following PowerShell command can be used:</span></span>

    ```
    [Environment]::SetEnvironmentVariable("AZURE_AUTH_LOCATION", "C:\Visual Studio 2017\Projects\myDotnetProject\myDotnetProject\azureauth.properties", "User")
    ```
    
## <a name="create-hello-management-client"></a><span data-ttu-id="5eea6-146">Vytvoření klienta pro správu hello</span><span class="sxs-lookup"><span data-stu-id="5eea6-146">Create hello management client</span></span>

1. <span data-ttu-id="5eea6-147">Otevřete soubor Program.cs hello pro hello projekt, který jste vytvořili a pak přidejte tyto příkazy toohello existující příkazy using v horní části souboru hello:</span><span class="sxs-lookup"><span data-stu-id="5eea6-147">Open hello Program.cs file for hello project that you created, and then add these using statements toohello existing statements at top of hello file:</span></span>

    ```
    using Microsoft.Azure.Management.Compute.Fluent;
    using Microsoft.Azure.Management.Compute.Fluent.Models;
    using Microsoft.Azure.Management.Fluent;
    using Microsoft.Azure.Management.ResourceManager.Fluent;
    using Microsoft.Azure.Management.ResourceManager.Fluent.Core;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```

2. <span data-ttu-id="5eea6-148">Klient správy hello toocreate, přidejte tento kód toohello metodu Main:</span><span class="sxs-lookup"><span data-stu-id="5eea6-148">toocreate hello management client, add this code toohello Main method:</span></span>

    ```
    var credentials = SdkContext.AzureCredentialsFactory
        .FromFile(Environment.GetEnvironmentVariable("AZURE_AUTH_LOCATION"));

    var azure = Azure
        .Configure()
        .WithLogLevel(HttpLoggingDelegatingHandler.Level.Basic)
        .Authenticate(credentials)
        .WithDefaultSubscription();
    ```

## <a name="create-a-resource-group"></a><span data-ttu-id="5eea6-149">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="5eea6-149">Create a resource group</span></span>

<span data-ttu-id="5eea6-150">toospecify hodnoty pro aplikace hello, přidejte metodu Main toohello kódu:</span><span class="sxs-lookup"><span data-stu-id="5eea6-150">toospecify values for hello application, add code toohello Main method:</span></span>

```
var groupName = "myResourceGroup";
var location = Region.USWest;

var resourceGroup = azure.ResourceGroups.Define(groupName)
    .WithRegion(location)
    .Create();
```

## <a name="create-a-storage-account"></a><span data-ttu-id="5eea6-151">vytvořit účet úložiště</span><span class="sxs-lookup"><span data-stu-id="5eea6-151">Create a storage account</span></span>

<span data-ttu-id="5eea6-152">Šablona Hello a parametry jsou nasazeny v účtu úložiště v Azure.</span><span class="sxs-lookup"><span data-stu-id="5eea6-152">hello template and parameters are deployed from a storage account in Azure.</span></span> <span data-ttu-id="5eea6-153">V tomto kroku vytvoříte účet hello a nahrání souborů hello.</span><span class="sxs-lookup"><span data-stu-id="5eea6-153">In this step, you create hello account and upload hello files.</span></span> 

<span data-ttu-id="5eea6-154">toocreate hello účet, přidejte tento kód toohello metodu Main:</span><span class="sxs-lookup"><span data-stu-id="5eea6-154">toocreate hello account, add this code toohello Main method:</span></span>

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

## <a name="deploy-hello-template"></a><span data-ttu-id="5eea6-155">Nasazení šablony hello</span><span class="sxs-lookup"><span data-stu-id="5eea6-155">Deploy hello template</span></span>

<span data-ttu-id="5eea6-156">Nasazení šablony hello a parametry z hello účet úložiště, který byl vytvořen.</span><span class="sxs-lookup"><span data-stu-id="5eea6-156">Deploy hello template and parameters from hello storage account that was created.</span></span> 

<span data-ttu-id="5eea6-157">Šablona hello toodeploy, přidejte tento kód toohello metodu Main:</span><span class="sxs-lookup"><span data-stu-id="5eea6-157">toodeploy hello template, add this code toohello Main method:</span></span>

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

## <a name="delete-hello-resources"></a><span data-ttu-id="5eea6-158">Odstraňte prostředky hello</span><span class="sxs-lookup"><span data-stu-id="5eea6-158">Delete hello resources</span></span>

<span data-ttu-id="5eea6-159">Vzhledem k tomu, že se vám účtovat prostředky využívané v Azure, vždycky je dobrým zvykem toodelete prostředky, které už nejsou potřeba.</span><span class="sxs-lookup"><span data-stu-id="5eea6-159">Because you are charged for resources used in Azure, it is always good practice toodelete resources that are no longer needed.</span></span> <span data-ttu-id="5eea6-160">Nepotřebujete toodelete každého prostředku nezávisle na skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="5eea6-160">You don’t need toodelete each resource separately from a resource group.</span></span> <span data-ttu-id="5eea6-161">Odstranit skupinu prostředků hello a všechny její prostředky se automaticky odstraní.</span><span class="sxs-lookup"><span data-stu-id="5eea6-161">Delete hello resource group and all its resources are automatically deleted.</span></span> 

<span data-ttu-id="5eea6-162">toodelete hello prostředků skupiny, přidejte tento kód toohello metodu Main:</span><span class="sxs-lookup"><span data-stu-id="5eea6-162">toodelete hello resource group, add this code toohello Main method:</span></span>

```
azure.ResourceGroups.DeleteByName(groupName);
```

## <a name="run-hello-application"></a><span data-ttu-id="5eea6-163">Spuštění aplikace hello</span><span class="sxs-lookup"><span data-stu-id="5eea6-163">Run hello application</span></span>

<span data-ttu-id="5eea6-164">Má trvat přibližně pět minut, než tato toorun aplikace konzoly zcela od počáteční toofinish.</span><span class="sxs-lookup"><span data-stu-id="5eea6-164">It should take about five minutes for this console application toorun completely from start toofinish.</span></span> 

1. <span data-ttu-id="5eea6-165">toorun hello konzolovou aplikaci, klikněte na tlačítko **spustit**.</span><span class="sxs-lookup"><span data-stu-id="5eea6-165">toorun hello console application, click **Start**.</span></span>

2. <span data-ttu-id="5eea6-166">Před stisknutím klávesy **Enter** toostart odstraňování prostředky, vám může trvat několik minut tooverify hello vytváření prostředků hello v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="5eea6-166">Before you press **Enter** toostart deleting resources, you could take a few minutes tooverify hello creation of hello resources in hello Azure portal.</span></span> <span data-ttu-id="5eea6-167">Klikněte na tlačítko hello nasazení toosee informace o stavu hello nasazení.</span><span class="sxs-lookup"><span data-stu-id="5eea6-167">Click hello deployment status toosee information about hello deployment.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5eea6-168">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5eea6-168">Next steps</span></span>
* <span data-ttu-id="5eea6-169">Pokud byly nějaké problémy s hello nasazení, je dalším krokem bude toolook na [odstraňování běžných chyb nasazení Azure pomocí Azure Resource Manageru](../../resource-manager-common-deployment-errors.md).</span><span class="sxs-lookup"><span data-stu-id="5eea6-169">If there were issues with hello deployment, a next step would be toolook at [Troubleshoot common Azure deployment errors with Azure Resource Manager](../../resource-manager-common-deployment-errors.md).</span></span>
* <span data-ttu-id="5eea6-170">Zjistěte, jak toodeploy virtuální počítač a jeho podpůrné prostředky kontrolou [nasazení služby Azure virtuálního počítače pomocí jazyka C#](csharp.md).</span><span class="sxs-lookup"><span data-stu-id="5eea6-170">Learn how toodeploy a virtual machine and its supporting resources by reviewing [Deploy an Azure Virtual Machine Using C#](csharp.md).</span></span>
