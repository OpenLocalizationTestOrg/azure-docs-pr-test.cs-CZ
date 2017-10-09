---
title: "aaaCreate Azure IoT Hub pomocí šablony (.NET) | Microsoft Docs"
description: "Jak toouse toocreate šablony Azure Resource Manager Centrum IoT se programu v C#."
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
ms.openlocfilehash: 6140deff3553701f994502fd4a60178f874e27cf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-iot-hub-using-azure-resource-manager-template-net"></a><span data-ttu-id="72bbc-103">Vytvoření služby IoT hub pomocí šablony Azure Resource Manageru (.NET)</span><span class="sxs-lookup"><span data-stu-id="72bbc-103">Create an IoT hub using Azure Resource Manager template (.NET)</span></span>

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

<span data-ttu-id="72bbc-104">Můžete použít Azure Resource Manager toocreate a spravovat Azure IoT hubs prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="72bbc-104">You can use Azure Resource Manager toocreate and manage Azure IoT hubs programmatically.</span></span> <span data-ttu-id="72bbc-105">Tento kurz ukazuje, jak toouse toocreate šablony Azure Resource Manager Centrum IoT z programu v C#.</span><span class="sxs-lookup"><span data-stu-id="72bbc-105">This tutorial shows you how toouse an Azure Resource Manager template toocreate an IoT hub from a C# program.</span></span>

> [!NOTE]
> <span data-ttu-id="72bbc-106">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Azure Resource Manager a klasický](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="72bbc-106">Azure has two different deployment models for creating and working with resources:  [Azure Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="72bbc-107">Tento článek se zabývá pomocí modelu nasazení Azure Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="72bbc-107">This article covers using hello Azure Resource Manager deployment model.</span></span>

<span data-ttu-id="72bbc-108">toocomplete tohoto kurzu budete potřebovat hello následující:</span><span class="sxs-lookup"><span data-stu-id="72bbc-108">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="72bbc-109">Visual Studio 2015 nebo Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="72bbc-109">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="72bbc-110">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="72bbc-110">An active Azure account.</span></span> <br/><span data-ttu-id="72bbc-111">Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="72bbc-111">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>
* <span data-ttu-id="72bbc-112">[Účet úložiště Azure] [ lnk-storage-account] kam můžete ukládat soubory šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="72bbc-112">An [Azure Storage account][lnk-storage-account] where you can store your Azure Resource Manager template files.</span></span>
* <span data-ttu-id="72bbc-113">[Azure PowerShell 1.0] [ lnk-powershell-install] nebo novější.</span><span class="sxs-lookup"><span data-stu-id="72bbc-113">[Azure PowerShell 1.0][lnk-powershell-install] or later.</span></span>

[!INCLUDE [iot-hub-prepare-resource-manager](../../includes/iot-hub-prepare-resource-manager.md)]

## <a name="prepare-your-visual-studio-project"></a><span data-ttu-id="72bbc-114">Příprava projektu sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="72bbc-114">Prepare your Visual Studio project</span></span>

1. <span data-ttu-id="72bbc-115">V sadě Visual Studio vytvořte projekt Visual C# Windows klasický desktopový pomocí hello **konzolovou aplikaci (rozhraní .NET Framework)** šablona projektu.</span><span class="sxs-lookup"><span data-stu-id="72bbc-115">In Visual Studio, create a Visual C# Windows Classic Desktop project using hello **Console App (.NET Framework)** project template.</span></span> <span data-ttu-id="72bbc-116">Název projektu hello **CreateIoTHub**.</span><span class="sxs-lookup"><span data-stu-id="72bbc-116">Name hello project **CreateIoTHub**.</span></span>

2. <span data-ttu-id="72bbc-117">V Průzkumníku řešení klikněte pravým tlačítkem na projekt a pak klikněte na tlačítko **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="72bbc-117">In Solution Explorer, right-click on your project and then click **Manage NuGet Packages**.</span></span>

3. <span data-ttu-id="72bbc-118">Zkontrolujte v Správce balíčků NuGet **zahrnout předběžné verze**a na hello **Procházet** stránky hledání **Microsoft.Azure.Management.ResourceManager**.</span><span class="sxs-lookup"><span data-stu-id="72bbc-118">In NuGet Package Manager, check **Include prerelease**, and on hello **Browse** page search for **Microsoft.Azure.Management.ResourceManager**.</span></span> <span data-ttu-id="72bbc-119">Vyberte hello balíčku, klikněte na **nainstalovat**v **zkontrolujte změny** klikněte na tlačítko **OK**, pak klikněte na tlačítko **souhlasím** tooaccept hello licence.</span><span class="sxs-lookup"><span data-stu-id="72bbc-119">Select hello package, click **Install**, in **Review Changes** click **OK**, then click **I Accept** tooaccept hello licenses.</span></span>

4. <span data-ttu-id="72bbc-120">Vyhledejte v Správce balíčků NuGet **Microsoft.IdentityModel.Clients.ActiveDirectory**.</span><span class="sxs-lookup"><span data-stu-id="72bbc-120">In NuGet Package Manager, search for **Microsoft.IdentityModel.Clients.ActiveDirectory**.</span></span>  <span data-ttu-id="72bbc-121">Klikněte na tlačítko **nainstalovat**v **zkontrolujte změny** klikněte na tlačítko **OK**, pak klikněte na tlačítko **souhlasím** tooaccept hello licence.</span><span class="sxs-lookup"><span data-stu-id="72bbc-121">Click **Install**, in **Review Changes** click **OK**, then click **I Accept** tooaccept hello license.</span></span>

5. <span data-ttu-id="72bbc-122">V Program.cs místo existující hello **pomocí** příkazy s hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="72bbc-122">In Program.cs, replace hello existing **using** statements with hello following code:</span></span>

    ```csharp
    using System;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.ResourceManager.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.Rest;
    ```

6. <span data-ttu-id="72bbc-123">V souboru Program.cs přidejte následující statické proměnné nahraďte zástupný symbol hodnoty hello hello.</span><span class="sxs-lookup"><span data-stu-id="72bbc-123">In Program.cs, add hello following static variables replacing hello placeholder values.</span></span> <span data-ttu-id="72bbc-124">Učinit poznámku o **ApplicationId**, **SubscriptionId**, **TenantId**, a **heslo** výše v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="72bbc-124">You made a note of **ApplicationId**, **SubscriptionId**, **TenantId**, and **Password** earlier in this tutorial.</span></span> <span data-ttu-id="72bbc-125">**Název účtu úložiště Azure** je název hello hello účet úložiště Azure, kde můžete ukládat soubory šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="72bbc-125">**Your Azure Storage account name** is hello name of hello Azure Storage account where you store your Azure Resource Manager template files.</span></span> <span data-ttu-id="72bbc-126">**Název skupiny prostředků** je hello název skupiny prostředků hello použijete při vytváření centra IoT hello.</span><span class="sxs-lookup"><span data-stu-id="72bbc-126">**Resource group name** is hello name of hello resource group you use when you create hello IoT hub.</span></span> <span data-ttu-id="72bbc-127">Název Hello může být existující nebo nové skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="72bbc-127">hello name can be a pre-existing or new resource group.</span></span> <span data-ttu-id="72bbc-128">**Název nasazení** , jako je název pro nasazení hello **Deployment_01**.</span><span class="sxs-lookup"><span data-stu-id="72bbc-128">**Deployment name** is a name for hello deployment, such as **Deployment_01**.</span></span>

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

## <a name="submit-a-template-toocreate-an-iot-hub"></a><span data-ttu-id="72bbc-129">Odeslání toocreate šablony služby IoT hub</span><span class="sxs-lookup"><span data-stu-id="72bbc-129">Submit a template toocreate an IoT hub</span></span>

<span data-ttu-id="72bbc-130">Použijte JSON šablony a parametr souboru toocreate služby IoT hub ve vaší skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="72bbc-130">Use a JSON template and parameter file toocreate an IoT hub in your resource group.</span></span> <span data-ttu-id="72bbc-131">Můžete použít také Azure Resource Manager šablony toomake změny tooan stávající služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="72bbc-131">You can also use an Azure Resource Manager template toomake changes tooan existing IoT hub.</span></span>

1. <span data-ttu-id="72bbc-132">V Průzkumníku řešení klikněte pravým tlačítkem na projekt, klikněte na tlačítko **přidat**a potom klikněte na **novou položku**.</span><span class="sxs-lookup"><span data-stu-id="72bbc-132">In Solution Explorer, right-click on your project, click **Add**, and then click **New Item**.</span></span> <span data-ttu-id="72bbc-133">Přidejte soubor JSON s názvem **template.json** tooyour projektu.</span><span class="sxs-lookup"><span data-stu-id="72bbc-133">Add a JSON file called **template.json** tooyour project.</span></span>

2. <span data-ttu-id="72bbc-134">tooadd standardní toohello centra IoT **východní USA** oblast, nahraďte obsah hello **template.json** s hello následující definici prostředků.</span><span class="sxs-lookup"><span data-stu-id="72bbc-134">tooadd a standard IoT hub toohello **East US** region, replace hello contents of **template.json** with hello following resource definition.</span></span> <span data-ttu-id="72bbc-135">Hello aktuální seznam oblastí, které podporují služby IoT Hub naleznete v části [stavu Azure][lnk-status]:</span><span class="sxs-lookup"><span data-stu-id="72bbc-135">For hello current list of regions that support IoT Hub see [Azure Status][lnk-status]:</span></span>

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

3. <span data-ttu-id="72bbc-136">V Průzkumníku řešení klikněte pravým tlačítkem na projekt, klikněte na tlačítko **přidat**a potom klikněte na **novou položku**.</span><span class="sxs-lookup"><span data-stu-id="72bbc-136">In Solution Explorer, right-click on your project, click **Add**, and then click **New Item**.</span></span> <span data-ttu-id="72bbc-137">Přidejte soubor JSON s názvem **Parameters.JSON tímto kódem** tooyour projektu.</span><span class="sxs-lookup"><span data-stu-id="72bbc-137">Add a JSON file called **parameters.json** tooyour project.</span></span>

4. <span data-ttu-id="72bbc-138">Nahraďte obsah hello **Parameters.JSON tímto kódem** s hello následující informace o parametrech, které nastavuje název hello novou službu IoT hub, jako **{vašimi iniciálami} mynewiothub**.</span><span class="sxs-lookup"><span data-stu-id="72bbc-138">Replace hello contents of **parameters.json** with hello following parameter information that sets a name for hello new IoT hub such as **{your initials}mynewiothub**.</span></span> <span data-ttu-id="72bbc-139">název centra IoT Hello musí být globálně jedinečný, aby měl by obsahovat název nebo initials:</span><span class="sxs-lookup"><span data-stu-id="72bbc-139">hello IoT hub name must be globally unique so it should include your name or initials:</span></span>

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

5. <span data-ttu-id="72bbc-140">V **Průzkumníka serveru**, připojit tooyour předplatného Azure a ve službě Azure Storage účet vytvořit kontejner s názvem **šablony**.</span><span class="sxs-lookup"><span data-stu-id="72bbc-140">In **Server Explorer**, connect tooyour Azure subscription, and in your Azure Storage account create a container called **templates**.</span></span> <span data-ttu-id="72bbc-141">V hello **vlastnosti** panelu, sada hello **veřejný přístup pro čtení** oprávnění pro hello **šablony** kontejneru příliš**Blob**.</span><span class="sxs-lookup"><span data-stu-id="72bbc-141">In hello **Properties** panel, set hello **Public Read Access** permissions for hello **templates** container too**Blob**.</span></span>

6. <span data-ttu-id="72bbc-142">V **Průzkumníka serveru**, klikněte pravým tlačítkem na hello **šablony** kontejneru a pak klikněte na tlačítko **kontejner objektů Blob zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="72bbc-142">In **Server Explorer**, right-click on hello **templates** container and then click **View Blob Container**.</span></span> <span data-ttu-id="72bbc-143">Klikněte na tlačítko hello **nahrát objekt Blob** tlačítko, vyberte hello dva soubory, **Parameters.JSON tímto kódem** a **templates.json**a potom klikněte na **otevřete** tooupload hello JSON soubory toohello **šablony** kontejneru.</span><span class="sxs-lookup"><span data-stu-id="72bbc-143">Click hello **Upload Blob** button, select hello two files, **parameters.json** and **templates.json**, and then click **Open** tooupload hello JSON files toohello **templates** container.</span></span> <span data-ttu-id="72bbc-144">adresy URL Hello objektů BLOB hello obsahující hello JSON data jsou:</span><span class="sxs-lookup"><span data-stu-id="72bbc-144">hello URLs of hello blobs containing hello JSON data are:</span></span>

    ```csharp
    https://{Your storage account name}.blob.core.windows.net/templates/parameters.json
    https://{Your storage account name}.blob.core.windows.net/templates/template.json
    ```
7. <span data-ttu-id="72bbc-145">Přidejte následující metodu tooProgram.cs hello:</span><span class="sxs-lookup"><span data-stu-id="72bbc-145">Add hello following method tooProgram.cs:</span></span>

    ```csharp
    static void CreateIoTHub(ResourceManagementClient client)
    {

    }
    ```

8. <span data-ttu-id="72bbc-146">Přidejte následující kód toohello hello **CreateIoTHub** metoda toosubmit hello šablony a parametr soubory toohello Azure Resource Manager:</span><span class="sxs-lookup"><span data-stu-id="72bbc-146">Add hello following code toohello **CreateIoTHub** method toosubmit hello template and parameter files toohello Azure Resource Manager:</span></span>

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

9. <span data-ttu-id="72bbc-147">Přidejte následující kód toohello hello **CreateIoTHub** metoda, která se zobrazuje stav hello a hello klíče pro hello novou službu IoT hub:</span><span class="sxs-lookup"><span data-stu-id="72bbc-147">Add hello following code toohello **CreateIoTHub** method that displays hello status and hello keys for hello new IoT hub:</span></span>

    ```csharp
    string state = createResponse.Properties.ProvisioningState;
    Console.WriteLine("Deployment state: {0}", state);

    if (state != "Succeeded")
    {
      Console.WriteLine("Failed toocreate iothub");
    }
    Console.WriteLine(createResponse.Properties.Outputs);
    ```

## <a name="complete-and-run-hello-application"></a><span data-ttu-id="72bbc-148">Hello úplný a spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="72bbc-148">Complete and run hello application</span></span>

<span data-ttu-id="72bbc-149">Hello aplikace je nyní možné dokončit pomocí volání hello **CreateIoTHub** metoda předtím, než můžete sestavit a spustit ho.</span><span class="sxs-lookup"><span data-stu-id="72bbc-149">You can now complete hello application by calling hello **CreateIoTHub** method before you build and run it.</span></span>

1. <span data-ttu-id="72bbc-150">Přidejte následující kód toohello konec hello hello **hlavní** metoda:</span><span class="sxs-lookup"><span data-stu-id="72bbc-150">Add hello following code toohello end of hello **Main** method:</span></span>

    ```csharp
    CreateIoTHub(client);
    Console.ReadLine();
    ```

2. <span data-ttu-id="72bbc-151">Klikněte na tlačítko **sestavení** a potom **sestavení řešení**.</span><span class="sxs-lookup"><span data-stu-id="72bbc-151">Click **Build** and then **Build Solution**.</span></span> <span data-ttu-id="72bbc-152">Opravte všechny chyby.</span><span class="sxs-lookup"><span data-stu-id="72bbc-152">Correct any errors.</span></span>

3. <span data-ttu-id="72bbc-153">Klikněte na tlačítko **ladění** a potom **spustit ladění** toorun hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="72bbc-153">Click **Debug** and then **Start Debugging** toorun hello application.</span></span> <span data-ttu-id="72bbc-154">Ho může trvat několik minut, než toorun nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="72bbc-154">It may take several minutes for hello deployment toorun.</span></span>

4. <span data-ttu-id="72bbc-155">tooverify přidat aplikace hello novou službu IoT hub, navštivte hello [portál Azure] [ lnk-azure-portal] a zobrazení seznamu prostředků.</span><span class="sxs-lookup"><span data-stu-id="72bbc-155">tooverify your application added hello new IoT hub, visit hello [Azure portal][lnk-azure-portal] and view your list of resources.</span></span> <span data-ttu-id="72bbc-156">Můžete taky použít hello **Get-AzureRmResource** rutiny prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="72bbc-156">Alternatively, use hello **Get-AzureRmResource** PowerShell cmdlet.</span></span>

> [!NOTE]
> <span data-ttu-id="72bbc-157">Tato ukázková aplikace přidá S1 Standard IoT Hub pro kterou se účtují.</span><span class="sxs-lookup"><span data-stu-id="72bbc-157">This example application adds an S1 Standard IoT Hub for which you are billed.</span></span> <span data-ttu-id="72bbc-158">Odstraněním hello IoT hub prostřednictvím hello [portál Azure] [ lnk-azure-portal] nebo pomocí hello **odebrat AzureRmResource** rutiny prostředí PowerShell po dokončení.</span><span class="sxs-lookup"><span data-stu-id="72bbc-158">You can delete hello IoT hub through hello [Azure portal][lnk-azure-portal] or by using hello **Remove-AzureRmResource** PowerShell cmdlet when you are finished.</span></span>

## <a name="next-steps"></a><span data-ttu-id="72bbc-159">Další kroky</span><span class="sxs-lookup"><span data-stu-id="72bbc-159">Next steps</span></span>
<span data-ttu-id="72bbc-160">Nyní jste nasadili služby IoT hub pomocí šablony Azure Resource Manageru pomocí programu v C#, můžete další tooexplore:</span><span class="sxs-lookup"><span data-stu-id="72bbc-160">Now you have deployed an IoT hub using an Azure Resource Manager template with a C# program, you may want tooexplore further:</span></span>

* <span data-ttu-id="72bbc-161">Přečtěte si informace o možnostech hello hello [zprostředkovatele prostředků služby IoT Hub REST API][lnk-rest-api].</span><span class="sxs-lookup"><span data-stu-id="72bbc-161">Read about hello capabilities of hello [IoT Hub resource provider REST API][lnk-rest-api].</span></span>
* <span data-ttu-id="72bbc-162">Čtení [přehled Azure Resource Manageru] [ lnk-azure-rm-overview] toolearn více informací o hello možnosti nástroje Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="72bbc-162">Read [Azure Resource Manager overview][lnk-azure-rm-overview] toolearn more about hello capabilities of Azure Resource Manager.</span></span>

<span data-ttu-id="72bbc-163">toolearn Další informace o vývoji pro IoT Hub, najdete v části hello následující články:</span><span class="sxs-lookup"><span data-stu-id="72bbc-163">toolearn more about developing for IoT Hub, see hello following articles:</span></span>

* <span data-ttu-id="72bbc-164">[Úvod tooC SDK][lnk-c-sdk]</span><span class="sxs-lookup"><span data-stu-id="72bbc-164">[Introduction tooC SDK][lnk-c-sdk]</span></span>
* <span data-ttu-id="72bbc-165">[Sady SDK služby Azure IoT][lnk-sdks]</span><span class="sxs-lookup"><span data-stu-id="72bbc-165">[Azure IoT SDKs][lnk-sdks]</span></span>

<span data-ttu-id="72bbc-166">toofurther prozkoumat hello služby IoT Hub, najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="72bbc-166">toofurther explore hello capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="72bbc-167">[Simulaci zařízení s Azure IoT Edge][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="72bbc-167">[Simulating a device with Azure IoT Edge][lnk-iotedge]</span></span>

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
