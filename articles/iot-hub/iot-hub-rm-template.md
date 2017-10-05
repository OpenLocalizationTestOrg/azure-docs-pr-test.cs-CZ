---
title: "Vytvoření služby Azure IoT Hub pomocí šablony (.NET) | Microsoft Docs"
description: "Postup vytvoření služby IoT Hub s programu v C# pomocí šablony Azure Resource Manager."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: a447b40c-c728-487e-875d-db554db5adc3
ms.service: iot-hub
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: 0f197a28e0c51b06d0b47a03c29fe1fde0c6b78d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-an-iot-hub-using-azure-resource-manager-template-net"></a><span data-ttu-id="08df8-103">Vytvoření služby IoT hub pomocí šablony Azure Resource Manageru (.NET)</span><span class="sxs-lookup"><span data-stu-id="08df8-103">Create an IoT hub using Azure Resource Manager template (.NET)</span></span>

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

<span data-ttu-id="08df8-104">Azure Resource Manager můžete použít k vytváření a správě Azure IoT hubs prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="08df8-104">You can use Azure Resource Manager to create and manage Azure IoT hubs programmatically.</span></span> <span data-ttu-id="08df8-105">V tomto kurzu se dozvíte, jak k vytvoření služby IoT hub z programu v C# pomocí šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="08df8-105">This tutorial shows you how to use an Azure Resource Manager template to create an IoT hub from a C# program.</span></span>

> [!NOTE]
> <span data-ttu-id="08df8-106">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Azure Resource Manager a klasický](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="08df8-106">Azure has two different deployment models for creating and working with resources:  [Azure Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="08df8-107">Tento článek se zabývá pomocí modelu nasazení Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="08df8-107">This article covers using the Azure Resource Manager deployment model.</span></span>

<span data-ttu-id="08df8-108">Pro absolvování tohoto kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="08df8-108">To complete this tutorial, you need the following:</span></span>

* <span data-ttu-id="08df8-109">Visual Studio 2015 nebo Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="08df8-109">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="08df8-110">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="08df8-110">An active Azure account.</span></span> <br/><span data-ttu-id="08df8-111">Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="08df8-111">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>
* <span data-ttu-id="08df8-112">[Účet úložiště Azure] [ lnk-storage-account] kam můžete ukládat soubory šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="08df8-112">An [Azure Storage account][lnk-storage-account] where you can store your Azure Resource Manager template files.</span></span>
* <span data-ttu-id="08df8-113">[Azure PowerShell 1.0] [ lnk-powershell-install] nebo novější.</span><span class="sxs-lookup"><span data-stu-id="08df8-113">[Azure PowerShell 1.0][lnk-powershell-install] or later.</span></span>

[!INCLUDE [iot-hub-prepare-resource-manager](../../includes/iot-hub-prepare-resource-manager.md)]

## <a name="prepare-your-visual-studio-project"></a><span data-ttu-id="08df8-114">Příprava projektu sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="08df8-114">Prepare your Visual Studio project</span></span>

1. <span data-ttu-id="08df8-115">V sadě Visual Studio vytvořit Visual C# Windows klasický desktopový projekt pomocí **konzolovou aplikaci (rozhraní .NET Framework)** šablona projektu.</span><span class="sxs-lookup"><span data-stu-id="08df8-115">In Visual Studio, create a Visual C# Windows Classic Desktop project using the **Console App (.NET Framework)** project template.</span></span> <span data-ttu-id="08df8-116">Název projektu **CreateIoTHub**.</span><span class="sxs-lookup"><span data-stu-id="08df8-116">Name the project **CreateIoTHub**.</span></span>

2. <span data-ttu-id="08df8-117">V Průzkumníku řešení klikněte pravým tlačítkem na projekt a pak klikněte na tlačítko **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="08df8-117">In Solution Explorer, right-click on your project and then click **Manage NuGet Packages**.</span></span>

3. <span data-ttu-id="08df8-118">Zkontrolujte v Správce balíčků NuGet **zahrnout předběžné verze**a na **Procházet** stránky hledání **Microsoft.Azure.Management.ResourceManager**.</span><span class="sxs-lookup"><span data-stu-id="08df8-118">In NuGet Package Manager, check **Include prerelease**, and on the **Browse** page search for **Microsoft.Azure.Management.ResourceManager**.</span></span> <span data-ttu-id="08df8-119">Vyberte balíček, klikněte na tlačítko **nainstalovat**v **zkontrolujte změny** klikněte na tlačítko **OK**, pak klikněte na tlačítko **souhlasím** tak, aby přijímal licence.</span><span class="sxs-lookup"><span data-stu-id="08df8-119">Select the package, click **Install**, in **Review Changes** click **OK**, then click **I Accept** to accept the licenses.</span></span>

4. <span data-ttu-id="08df8-120">Vyhledejte v Správce balíčků NuGet **Microsoft.IdentityModel.Clients.ActiveDirectory**.</span><span class="sxs-lookup"><span data-stu-id="08df8-120">In NuGet Package Manager, search for **Microsoft.IdentityModel.Clients.ActiveDirectory**.</span></span>  <span data-ttu-id="08df8-121">Klikněte na tlačítko **nainstalovat**v **zkontrolujte změny** klikněte na tlačítko **OK**, pak klikněte na tlačítko **souhlasím** tak, aby přijímal licence.</span><span class="sxs-lookup"><span data-stu-id="08df8-121">Click **Install**, in **Review Changes** click **OK**, then click **I Accept** to accept the license.</span></span>

5. <span data-ttu-id="08df8-122">V Program.cs místo existující **pomocí** příkazy následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="08df8-122">In Program.cs, replace the existing **using** statements with the following code:</span></span>

    ```csharp
    using System;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.ResourceManager.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.Rest;
    ```

6. <span data-ttu-id="08df8-123">V souboru Program.cs přidejte následující proměnné na statické nahraďte zástupný symbol hodnoty.</span><span class="sxs-lookup"><span data-stu-id="08df8-123">In Program.cs, add the following static variables replacing the placeholder values.</span></span> <span data-ttu-id="08df8-124">Učinit poznámku o **ApplicationId**, **SubscriptionId**, **TenantId**, a **heslo** výše v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="08df8-124">You made a note of **ApplicationId**, **SubscriptionId**, **TenantId**, and **Password** earlier in this tutorial.</span></span> <span data-ttu-id="08df8-125">**Název účtu úložiště Azure** je název účtu úložiště Azure, kde můžete ukládat soubory šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="08df8-125">**Your Azure Storage account name** is the name of the Azure Storage account where you store your Azure Resource Manager template files.</span></span> <span data-ttu-id="08df8-126">**Název skupiny prostředků** je název skupiny prostředků, můžete použít, když vytvoříte Centrum IoT.</span><span class="sxs-lookup"><span data-stu-id="08df8-126">**Resource group name** is the name of the resource group you use when you create the IoT hub.</span></span> <span data-ttu-id="08df8-127">Název může být existující nebo nové skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="08df8-127">The name can be a pre-existing or new resource group.</span></span> <span data-ttu-id="08df8-128">**Název nasazení** , jako je název pro nasazení, **Deployment_01**.</span><span class="sxs-lookup"><span data-stu-id="08df8-128">**Deployment name** is a name for the deployment, such as **Deployment_01**.</span></span>

    ```csharp
    static string applicationId = "{Your ApplicationId}";
    static string subscriptionId = "{Your SubscriptionId}";
    static string tenantId = "{Your TenantId}";
    static string password = "{Your application Password}";
    static string storageAddress = "https://{Your storage account name}.blob.core.windows.net";
    static string rgName = "{Resource group name}";
    static string deploymentName = "{Deployment name}";
    ```

[!INCLUDE [iot-hub-get-access-token](../../includes/iot-hub-get-access-token.md)]

## <a name="submit-a-template-to-create-an-iot-hub"></a><span data-ttu-id="08df8-129">Odeslat šablonu pro vytvoření služby IoT hub</span><span class="sxs-lookup"><span data-stu-id="08df8-129">Submit a template to create an IoT hub</span></span>

<span data-ttu-id="08df8-130">Použijte soubor JSON šablony a parametrů k vytvoření služby IoT hub ve vaší skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="08df8-130">Use a JSON template and parameter file to create an IoT hub in your resource group.</span></span> <span data-ttu-id="08df8-131">Šablonu Azure Resource Manager můžete také provést změny do stávající služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="08df8-131">You can also use an Azure Resource Manager template to make changes to an existing IoT hub.</span></span>

1. <span data-ttu-id="08df8-132">V Průzkumníku řešení klikněte pravým tlačítkem na projekt, klikněte na tlačítko **přidat**a potom klikněte na **novou položku**.</span><span class="sxs-lookup"><span data-stu-id="08df8-132">In Solution Explorer, right-click on your project, click **Add**, and then click **New Item**.</span></span> <span data-ttu-id="08df8-133">Přidejte soubor JSON s názvem **template.json** do projektu.</span><span class="sxs-lookup"><span data-stu-id="08df8-133">Add a JSON file called **template.json** to your project.</span></span>

2. <span data-ttu-id="08df8-134">Chcete-li přidat standardní IoT hub, která **východní USA** oblast, nahraďte obsah **template.json** s následující definice prostředků.</span><span class="sxs-lookup"><span data-stu-id="08df8-134">To add a standard IoT hub to the **East US** region, replace the contents of **template.json** with the following resource definition.</span></span> <span data-ttu-id="08df8-135">Aktuální seznam oblastí, které podporují služby IoT Hub naleznete v části [stavu Azure][lnk-status]:</span><span class="sxs-lookup"><span data-stu-id="08df8-135">For the current list of regions that support IoT Hub see [Azure Status][lnk-status]:</span></span>

    ```json
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "hubName": {
          "type": "string"
        }
      },
      "resources": [
      {
        "apiVersion": "2016-02-03",
        "type": "Microsoft.Devices/IotHubs",
        "name": "[parameters('hubName')]",
        "location": "East US",
        "sku": {
          "name": "S1",
          "tier": "Standard",
          "capacity": 1
        },
        "properties": {
          "location": "East US"
        }
      }
      ],
      "outputs": {
        "hubKeys": {
          "value": "[listKeys(resourceId('Microsoft.Devices/IotHubs', parameters('hubName')), '2016-02-03')]",
          "type": "object"
        }
      }
    }
    ```

3. <span data-ttu-id="08df8-136">V Průzkumníku řešení klikněte pravým tlačítkem na projekt, klikněte na tlačítko **přidat**a potom klikněte na **novou položku**.</span><span class="sxs-lookup"><span data-stu-id="08df8-136">In Solution Explorer, right-click on your project, click **Add**, and then click **New Item**.</span></span> <span data-ttu-id="08df8-137">Přidejte soubor JSON s názvem **Parameters.JSON tímto kódem** do projektu.</span><span class="sxs-lookup"><span data-stu-id="08df8-137">Add a JSON file called **parameters.json** to your project.</span></span>

4. <span data-ttu-id="08df8-138">Nahraďte obsah **Parameters.JSON tímto kódem** s následujícími informacemi parametr, který nastaví název nového centra IoT, jako **{vašimi iniciálami} mynewiothub**.</span><span class="sxs-lookup"><span data-stu-id="08df8-138">Replace the contents of **parameters.json** with the following parameter information that sets a name for the new IoT hub such as **{your initials}mynewiothub**.</span></span> <span data-ttu-id="08df8-139">Název centra IoT musí být globálně jedinečný, aby měl by obsahovat název nebo initials:</span><span class="sxs-lookup"><span data-stu-id="08df8-139">The IoT hub name must be globally unique so it should include your name or initials:</span></span>

    ```json
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "hubName": { "value": "mynewiothub" }
      }
    }
    ```
  [!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]

5. <span data-ttu-id="08df8-140">V **Průzkumníka serveru**, připojení k předplatnému Azure a v účtu úložiště Azure vytvořit kontejner s názvem **šablony**.</span><span class="sxs-lookup"><span data-stu-id="08df8-140">In **Server Explorer**, connect to your Azure subscription, and in your Azure Storage account create a container called **templates**.</span></span> <span data-ttu-id="08df8-141">V **vlastnosti** panelu, nastavte **veřejný přístup pro čtení** oprávnění pro **šablony** kontejner, aby **Blob**.</span><span class="sxs-lookup"><span data-stu-id="08df8-141">In the **Properties** panel, set the **Public Read Access** permissions for the **templates** container to **Blob**.</span></span>

6. <span data-ttu-id="08df8-142">V **Průzkumníka serveru**, klikněte pravým tlačítkem na **šablony** kontejneru a pak klikněte na tlačítko **kontejner objektů Blob zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="08df8-142">In **Server Explorer**, right-click on the **templates** container and then click **View Blob Container**.</span></span> <span data-ttu-id="08df8-143">Klikněte na tlačítko **nahrát objekt Blob** tlačítko, vyberte dva soubory **Parameters.JSON tímto kódem** a **templates.json**a potom klikněte na **otevřete** nahrát JSON souborů do **šablony** kontejneru.</span><span class="sxs-lookup"><span data-stu-id="08df8-143">Click the **Upload Blob** button, select the two files, **parameters.json** and **templates.json**, and then click **Open** to upload the JSON files to the **templates** container.</span></span> <span data-ttu-id="08df8-144">Adresy URL obsahující JSON data objekty BLOB jsou:</span><span class="sxs-lookup"><span data-stu-id="08df8-144">The URLs of the blobs containing the JSON data are:</span></span>

    ```csharp
    https://{Your storage account name}.blob.core.windows.net/templates/parameters.json
    https://{Your storage account name}.blob.core.windows.net/templates/template.json
    ```
7. <span data-ttu-id="08df8-145">Do souboru Program.cs přidejte následující metodu:</span><span class="sxs-lookup"><span data-stu-id="08df8-145">Add the following method to Program.cs:</span></span>

    ```csharp
    static void CreateIoTHub(ResourceManagementClient client)
    {

    }
    ```

8. <span data-ttu-id="08df8-146">Přidejte následující kód, který **CreateIoTHub** metoda odeslat soubory šablony a parametrů do Azure Resource Manager:</span><span class="sxs-lookup"><span data-stu-id="08df8-146">Add the following code to the **CreateIoTHub** method to submit the template and parameter files to the Azure Resource Manager:</span></span>

    ```csharp
    var createResponse = client.Deployments.CreateOrUpdate(
        rgName,
        deploymentName,
        new Deployment()
        {
          Properties = new DeploymentProperties
          {
            Mode = DeploymentMode.Incremental,
            TemplateLink = new TemplateLink
            {
              Uri = storageAddress + "/templates/template.json"
            },
            ParametersLink = new ParametersLink
            {
              Uri = storageAddress + "/templates/parameters.json"
            }
          }
        });
    ```

9. <span data-ttu-id="08df8-147">Přidejte následující kód, který **CreateIoTHub** metoda, která zobrazuje stav a klíče pro nového centra IoT:</span><span class="sxs-lookup"><span data-stu-id="08df8-147">Add the following code to the **CreateIoTHub** method that displays the status and the keys for the new IoT hub:</span></span>

    ```csharp
    string state = createResponse.Properties.ProvisioningState;
    Console.WriteLine("Deployment state: {0}", state);

    if (state != "Succeeded")
    {
      Console.WriteLine("Failed to create iothub");
    }
    Console.WriteLine(createResponse.Properties.Outputs);
    ```

## <a name="complete-and-run-the-application"></a><span data-ttu-id="08df8-148">Dokončení a spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="08df8-148">Complete and run the application</span></span>

<span data-ttu-id="08df8-149">Aplikace je nyní možné dokončit voláním **CreateIoTHub** metoda předtím, než můžete sestavit a spustit ho.</span><span class="sxs-lookup"><span data-stu-id="08df8-149">You can now complete the application by calling the **CreateIoTHub** method before you build and run it.</span></span>

1. <span data-ttu-id="08df8-150">Přidejte následující kód do konce **hlavní** metoda:</span><span class="sxs-lookup"><span data-stu-id="08df8-150">Add the following code to the end of the **Main** method:</span></span>

    ```csharp
    CreateIoTHub(client);
    Console.ReadLine();
    ```

2. <span data-ttu-id="08df8-151">Klikněte na tlačítko **sestavení** a potom **sestavení řešení**.</span><span class="sxs-lookup"><span data-stu-id="08df8-151">Click **Build** and then **Build Solution**.</span></span> <span data-ttu-id="08df8-152">Opravte všechny chyby.</span><span class="sxs-lookup"><span data-stu-id="08df8-152">Correct any errors.</span></span>

3. <span data-ttu-id="08df8-153">Klikněte na tlačítko **ladění** a potom **spustit ladění** ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="08df8-153">Click **Debug** and then **Start Debugging** to run the application.</span></span> <span data-ttu-id="08df8-154">Ho může trvat několik minut, než nasazení ke spuštění.</span><span class="sxs-lookup"><span data-stu-id="08df8-154">It may take several minutes for the deployment to run.</span></span>

4. <span data-ttu-id="08df8-155">Chcete-li ověřit, vaše aplikace přidat nového centra IoT, navštivte [portál Azure] [ lnk-azure-portal] a zobrazení seznamu prostředků.</span><span class="sxs-lookup"><span data-stu-id="08df8-155">To verify your application added the new IoT hub, visit the [Azure portal][lnk-azure-portal] and view your list of resources.</span></span> <span data-ttu-id="08df8-156">Můžete taky použít **Get-AzureRmResource** rutiny prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="08df8-156">Alternatively, use the **Get-AzureRmResource** PowerShell cmdlet.</span></span>

> [!NOTE]
> <span data-ttu-id="08df8-157">Tato ukázková aplikace přidá S1 Standard IoT Hub pro kterou se účtují.</span><span class="sxs-lookup"><span data-stu-id="08df8-157">This example application adds an S1 Standard IoT Hub for which you are billed.</span></span> <span data-ttu-id="08df8-158">Odstraněním služby IoT hub prostřednictvím [portál Azure] [ lnk-azure-portal] nebo pomocí **AzureRmResource odebrat** rutiny prostředí PowerShell po dokončení.</span><span class="sxs-lookup"><span data-stu-id="08df8-158">You can delete the IoT hub through the [Azure portal][lnk-azure-portal] or by using the **Remove-AzureRmResource** PowerShell cmdlet when you are finished.</span></span>

## <a name="next-steps"></a><span data-ttu-id="08df8-159">Další kroky</span><span class="sxs-lookup"><span data-stu-id="08df8-159">Next steps</span></span>
<span data-ttu-id="08df8-160">Nyní jste nasadili služby IoT hub pomocí šablony Azure Resource Manageru pomocí programu v C#, můžete chtít Další:</span><span class="sxs-lookup"><span data-stu-id="08df8-160">Now you have deployed an IoT hub using an Azure Resource Manager template with a C# program, you may want to explore further:</span></span>

* <span data-ttu-id="08df8-161">Přečtěte si informace o možnostech [zprostředkovatele prostředků služby IoT Hub REST API][lnk-rest-api].</span><span class="sxs-lookup"><span data-stu-id="08df8-161">Read about the capabilities of the [IoT Hub resource provider REST API][lnk-rest-api].</span></span>
* <span data-ttu-id="08df8-162">Čtení [přehled Azure Resource Manageru] [ lnk-azure-rm-overview] Další informace o funkcích nástroje Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="08df8-162">Read [Azure Resource Manager overview][lnk-azure-rm-overview] to learn more about the capabilities of Azure Resource Manager.</span></span>

<span data-ttu-id="08df8-163">Další informace o vývoji pro Centrum IoT, naleznete v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="08df8-163">To learn more about developing for IoT Hub, see the following articles:</span></span>

* <span data-ttu-id="08df8-164">[Úvod do jazyka C SDK][lnk-c-sdk]</span><span class="sxs-lookup"><span data-stu-id="08df8-164">[Introduction to C SDK][lnk-c-sdk]</span></span>
* <span data-ttu-id="08df8-165">[Sady SDK služby Azure IoT][lnk-sdks]</span><span class="sxs-lookup"><span data-stu-id="08df8-165">[Azure IoT SDKs][lnk-sdks]</span></span>

<span data-ttu-id="08df8-166">Pokud chcete prozkoumat další možnosti IoT Hub, najdete v části:</span><span class="sxs-lookup"><span data-stu-id="08df8-166">To further explore the capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="08df8-167">[Simulaci zařízení s Azure IoT Edge][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="08df8-167">[Simulating a device with Azure IoT Edge][lnk-iotedge]</span></span>

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-azure-portal]: https://portal.azure.com/
[lnk-status]: https://azure.microsoft.com/status/
[lnk-powershell-install]: https://docs.microsoft.com/powershell/azure/install-azurerm-ps
[lnk-rest-api]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-azure-rm-overview]: ../azure-resource-manager/resource-group-overview.md
[lnk-storage-account]:../storage/common/storage-create-storage-account.md

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
