---
title: "Centrum Azure IoT pomocí aaaCreate hello poskytovatele prostředků REST API | Microsoft Docs"
description: "Jak toouse hello toocreate REST API zprostředkovatele prostředků služby IoT Hub."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 52814ee5-bc10-4abe-9eb2-f8973096c2d8
ms.service: iot-hub
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: 98d240ccce47dec13a255bce28943b40f5354ecf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-iot-hub-using-hello-resource-provider-rest-api-net"></a><span data-ttu-id="c5acf-103">Vytvoření služby IoT hub pomocí zprostředkovatele prostředků hello rozhraní API REST (.NET)</span><span class="sxs-lookup"><span data-stu-id="c5acf-103">Create an IoT hub using hello resource provider REST API (.NET)</span></span>

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

<span data-ttu-id="c5acf-104">Můžete použít hello [zprostředkovatele prostředků služby IoT Hub REST API] [ lnk-rest-api] toocreate a spravovat Azure IoT hubs prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="c5acf-104">You can use hello [IoT Hub resource provider REST API][lnk-rest-api] toocreate and manage Azure IoT hubs programmatically.</span></span> <span data-ttu-id="c5acf-105">Tento kurz ukazuje, jak toouse hello toocreate REST API poskytovatele prostředků Centrum IoT Centrum IoT z programu v C#.</span><span class="sxs-lookup"><span data-stu-id="c5acf-105">This tutorial shows you how toouse hello IoT Hub resource provider REST API toocreate an IoT hub from a C# program.</span></span>

> [!NOTE]
> <span data-ttu-id="c5acf-106">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Azure Resource Manager a klasický](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="c5acf-106">Azure has two different deployment models for creating and working with resources:  [Azure Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="c5acf-107">Tento článek se zabývá pomocí modelu nasazení Azure Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="c5acf-107">This article covers using hello Azure Resource Manager deployment model.</span></span>

<span data-ttu-id="c5acf-108">toocomplete tohoto kurzu budete potřebovat hello následující:</span><span class="sxs-lookup"><span data-stu-id="c5acf-108">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="c5acf-109">Visual Studio 2015 nebo Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="c5acf-109">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="c5acf-110">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="c5acf-110">An active Azure account.</span></span> <br/><span data-ttu-id="c5acf-111">Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="c5acf-111">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>
* <span data-ttu-id="c5acf-112">[Azure PowerShell 1.0] [ lnk-powershell-install] nebo novější.</span><span class="sxs-lookup"><span data-stu-id="c5acf-112">[Azure PowerShell 1.0][lnk-powershell-install] or later.</span></span>

[!INCLUDE [iot-hub-prepare-resource-manager](../../includes/iot-hub-prepare-resource-manager.md)]

## <a name="prepare-your-visual-studio-project"></a><span data-ttu-id="c5acf-113">Příprava projektu sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c5acf-113">Prepare your Visual Studio project</span></span>

1. <span data-ttu-id="c5acf-114">V sadě Visual Studio vytvořte projekt Visual C# Windows klasický desktopový pomocí hello **konzolovou aplikaci (rozhraní .NET Framework)** šablona projektu.</span><span class="sxs-lookup"><span data-stu-id="c5acf-114">In Visual Studio, create a Visual C# Windows Classic Desktop project using hello **Console App (.NET Framework)** project template.</span></span> <span data-ttu-id="c5acf-115">Název projektu hello **CreateIoTHubREST**.</span><span class="sxs-lookup"><span data-stu-id="c5acf-115">Name hello project **CreateIoTHubREST**.</span></span>

2. <span data-ttu-id="c5acf-116">V Průzkumníku řešení klikněte pravým tlačítkem na projekt a pak klikněte na tlačítko **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="c5acf-116">In Solution Explorer, right-click on your project and then click **Manage NuGet Packages**.</span></span>

3. <span data-ttu-id="c5acf-117">Zkontrolujte v Správce balíčků NuGet **zahrnout předběžné verze**a na hello **Procházet** stránky hledání **Microsoft.Azure.Management.ResourceManager**.</span><span class="sxs-lookup"><span data-stu-id="c5acf-117">In NuGet Package Manager, check **Include prerelease**, and on hello **Browse** page search for **Microsoft.Azure.Management.ResourceManager**.</span></span> <span data-ttu-id="c5acf-118">Vyberte hello balíčku, klikněte na **nainstalovat**v **zkontrolujte změny** klikněte na tlačítko **OK**, pak klikněte na tlačítko **souhlasím** tooaccept hello licence.</span><span class="sxs-lookup"><span data-stu-id="c5acf-118">Select hello package, click **Install**, in **Review Changes** click **OK**, then click **I Accept** tooaccept hello licenses.</span></span>

4. <span data-ttu-id="c5acf-119">Vyhledejte v Správce balíčků NuGet **Microsoft.IdentityModel.Clients.ActiveDirectory**.</span><span class="sxs-lookup"><span data-stu-id="c5acf-119">In NuGet Package Manager, search for **Microsoft.IdentityModel.Clients.ActiveDirectory**.</span></span>  <span data-ttu-id="c5acf-120">Klikněte na tlačítko **nainstalovat**v **zkontrolujte změny** klikněte na tlačítko **OK**, pak klikněte na tlačítko **souhlasím** tooaccept hello licence.</span><span class="sxs-lookup"><span data-stu-id="c5acf-120">Click **Install**, in **Review Changes** click **OK**, then click **I Accept** tooaccept hello license.</span></span>

5. <span data-ttu-id="c5acf-121">V Program.cs místo existující hello **pomocí** příkazy s hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="c5acf-121">In Program.cs, replace hello existing **using** statements with hello following code:</span></span>

    ```csharp
    using System;
    using System.Net.Http;
    using System.Net.Http.Headers;
    using System.Text;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.ResourceManager.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Newtonsoft.Json;
    using Microsoft.Rest;
    using System.Linq;
    using System.Threading;
    ```

6. <span data-ttu-id="c5acf-122">V souboru Program.cs přidejte následující statické proměnné nahraďte zástupný symbol hodnoty hello hello.</span><span class="sxs-lookup"><span data-stu-id="c5acf-122">In Program.cs, add hello following static variables replacing hello placeholder values.</span></span> <span data-ttu-id="c5acf-123">Učinit poznámku o **ApplicationId**, **SubscriptionId**, **TenantId**, a **heslo** výše v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="c5acf-123">You made a note of **ApplicationId**, **SubscriptionId**, **TenantId**, and **Password** earlier in this tutorial.</span></span> <span data-ttu-id="c5acf-124">**Název skupiny prostředků** je hello název skupiny prostředků hello použijete při vytváření centra IoT hello.</span><span class="sxs-lookup"><span data-stu-id="c5acf-124">**Resource group name** is hello name of hello resource group you use when you create hello IoT hub.</span></span> <span data-ttu-id="c5acf-125">Můžete použít existující nebo novou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="c5acf-125">You can use a pre-existing or a new resource group.</span></span> <span data-ttu-id="c5acf-126">**Název centra IoT** je název hello hello IoT Hub, které vytvoříte, jako například **MyIoTHub**.</span><span class="sxs-lookup"><span data-stu-id="c5acf-126">**IoT Hub name** is hello name of hello IoT Hub you create, such as **MyIoTHub**.</span></span> <span data-ttu-id="c5acf-127">Hello název služby IoT hub musí být globálně jedinečný.</span><span class="sxs-lookup"><span data-stu-id="c5acf-127">hello name of your IoT hub must be globally unique.</span></span> <span data-ttu-id="c5acf-128">**Název nasazení** , jako je název pro nasazení hello **Deployment_01**.</span><span class="sxs-lookup"><span data-stu-id="c5acf-128">**Deployment name** is a name for hello deployment, such as **Deployment_01**.</span></span>

    ```csharp
    static string applicationId = "{Your ApplicationId}";
    static string subscriptionId = "{Your SubscriptionId}";
    static string tenantId = "{Your TenantId}";
    static string password = "{Your application Password}";

    static string rgName = "{Resource group name}";
    static string iotHubName = "{IoT Hub name including your initials}";
    ```
[!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]

[!INCLUDE [iot-hub-get-access-token](../../includes/iot-hub-get-access-token.md)]

## <a name="use-hello-resource-provider-rest-api-toocreate-an-iot-hub"></a><span data-ttu-id="c5acf-129">Použít hello prostředků poskytovatele rozhraní REST API toocreate služby IoT hub</span><span class="sxs-lookup"><span data-stu-id="c5acf-129">Use hello resource provider REST API toocreate an IoT hub</span></span>

<span data-ttu-id="c5acf-130">Použití hello [zprostředkovatele prostředků služby IoT Hub REST API] [ lnk-rest-api] toocreate služby IoT hub ve vaší skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="c5acf-130">Use hello [IoT Hub resource provider REST API][lnk-rest-api] toocreate an IoT hub in your resource group.</span></span> <span data-ttu-id="c5acf-131">Můžete také použít hello prostředků poskytovatele rozhraní REST API toomake změny tooan stávající služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="c5acf-131">You can also use hello resource provider REST API toomake changes tooan existing IoT hub.</span></span>

1. <span data-ttu-id="c5acf-132">Přidejte následující metodu tooProgram.cs hello:</span><span class="sxs-lookup"><span data-stu-id="c5acf-132">Add hello following method tooProgram.cs:</span></span>

    ```csharp
    static void CreateIoTHub(string token)
    {

    }
    ```

2. <span data-ttu-id="c5acf-133">Přidejte následující kód toohello hello **CreateIoTHub** metoda.</span><span class="sxs-lookup"><span data-stu-id="c5acf-133">Add hello following code toohello **CreateIoTHub** method.</span></span> <span data-ttu-id="c5acf-134">Tento kód vytvoří **HttpClient** objekt s hello ověřovací token v hlavičkách hello:</span><span class="sxs-lookup"><span data-stu-id="c5acf-134">This code creates an **HttpClient** object with hello authentication token in hello headers:</span></span>

    ```csharp
    HttpClient client = new HttpClient();
    client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", token);
    ```

3. <span data-ttu-id="c5acf-135">Přidejte následující kód toohello hello **CreateIoTHub** metoda.</span><span class="sxs-lookup"><span data-stu-id="c5acf-135">Add hello following code toohello **CreateIoTHub** method.</span></span> <span data-ttu-id="c5acf-136">Tento kód popisuje hello IoT hub toocreate a generuje reprezentaci JSON.</span><span class="sxs-lookup"><span data-stu-id="c5acf-136">This code describes hello IoT hub toocreate and generates a JSON representation.</span></span> <span data-ttu-id="c5acf-137">Hello aktuální seznam umístění, které podporují služby IoT Hub naleznete v části [stavu Azure][lnk-status]:</span><span class="sxs-lookup"><span data-stu-id="c5acf-137">For hello current list of locations that support IoT Hub see [Azure Status][lnk-status]:</span></span>

    ```csharp
    var description = new
    {
      name = iotHubName,
      location = "East US",
      sku = new
      {
        name = "S1",
        tier = "Standard",
        capacity = 1
      }
    };

    var json = JsonConvert.SerializeObject(description, Formatting.Indented);
    ```

4. <span data-ttu-id="c5acf-138">Přidejte následující kód toohello hello **CreateIoTHub** metoda.</span><span class="sxs-lookup"><span data-stu-id="c5acf-138">Add hello following code toohello **CreateIoTHub** method.</span></span> <span data-ttu-id="c5acf-139">Tento kód odešle tooAzure požadavku REST hello.</span><span class="sxs-lookup"><span data-stu-id="c5acf-139">This code submits hello REST request tooAzure.</span></span> <span data-ttu-id="c5acf-140">Hello kód pak zkontroluje odpověď hello a načte hello adresu URL můžete použít stav hello toomonitor úloha nasazení hello:</span><span class="sxs-lookup"><span data-stu-id="c5acf-140">hello code then checks hello response and retrieves hello URL you can use toomonitor hello state of hello deployment task:</span></span>

    ```csharp
    var content = new StringContent(JsonConvert.SerializeObject(description), Encoding.UTF8, "application/json");
    var requestUri = string.Format("https://management.azure.com/subscriptions/{0}/resourcegroups/{1}/providers/Microsoft.devices/IotHubs/{2}?api-version=2016-02-03", subscriptionId, rgName, iotHubName);
    var result = client.PutAsync(requestUri, content).Result;

    if (!result.IsSuccessStatusCode)
    {
      Console.WriteLine("Failed {0}", result.Content.ReadAsStringAsync().Result);
      return;
    }

    var asyncStatusUri = result.Headers.GetValues("Azure-AsyncOperation").First();
    ```

5. <span data-ttu-id="c5acf-141">Přidejte následující kód toohello konec hello hello **CreateIoTHub** metoda.</span><span class="sxs-lookup"><span data-stu-id="c5acf-141">Add hello following code toohello end of hello **CreateIoTHub** method.</span></span> <span data-ttu-id="c5acf-142">Tento kód používá hello **asyncStatusUri** získat adresu v toowait hello předchozí krok pro nasazení toocomplete hello:</span><span class="sxs-lookup"><span data-stu-id="c5acf-142">This code uses hello **asyncStatusUri** address retrieved in hello previous step toowait for hello deployment toocomplete:</span></span>

    ```csharp
    string body;
    do
    {
      Thread.Sleep(10000);
      HttpResponseMessage deploymentstatus = client.GetAsync(asyncStatusUri).Result;
      body = deploymentstatus.Content.ReadAsStringAsync().Result;
    } while (body == "{\"status\":\"Running\"}");
    ```

6. <span data-ttu-id="c5acf-143">Přidejte následující kód toohello konec hello hello **CreateIoTHub** metoda.</span><span class="sxs-lookup"><span data-stu-id="c5acf-143">Add hello following code toohello end of hello **CreateIoTHub** method.</span></span> <span data-ttu-id="c5acf-144">Tento kód načte hello klíče hello služby IoT hub jste vytvořili a vypíše je toohello konzoly:</span><span class="sxs-lookup"><span data-stu-id="c5acf-144">This code retrieves hello keys of hello IoT hub you created and prints them toohello console:</span></span>

    ```csharp
    var listKeysUri = string.Format("https://management.azure.com/subscriptions/{0}/resourceGroups/{1}/providers/Microsoft.Devices/IotHubs/{2}/IoTHubKeys/listkeys?api-version=2016-02-03", subscriptionId, rgName, iotHubName);
    var keysresults = client.PostAsync(listKeysUri, null).Result;

    Console.WriteLine("Keys: {0}", keysresults.Content.ReadAsStringAsync().Result);
    ```

## <a name="complete-and-run-hello-application"></a><span data-ttu-id="c5acf-145">Hello úplný a spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="c5acf-145">Complete and run hello application</span></span>

<span data-ttu-id="c5acf-146">Hello aplikace je nyní možné dokončit pomocí volání hello **CreateIoTHub** metoda předtím, než můžete sestavit a spustit ho.</span><span class="sxs-lookup"><span data-stu-id="c5acf-146">You can now complete hello application by calling hello **CreateIoTHub** method before you build and run it.</span></span>

1. <span data-ttu-id="c5acf-147">Přidejte následující kód toohello konec hello hello **hlavní** metoda:</span><span class="sxs-lookup"><span data-stu-id="c5acf-147">Add hello following code toohello end of hello **Main** method:</span></span>

    ```csharp
    CreateIoTHub(token.AccessToken);
    Console.ReadLine();
    ```

2. <span data-ttu-id="c5acf-148">Klikněte na tlačítko **sestavení** a potom **sestavení řešení**.</span><span class="sxs-lookup"><span data-stu-id="c5acf-148">Click **Build** and then **Build Solution**.</span></span> <span data-ttu-id="c5acf-149">Opravte všechny chyby.</span><span class="sxs-lookup"><span data-stu-id="c5acf-149">Correct any errors.</span></span>

3. <span data-ttu-id="c5acf-150">Klikněte na tlačítko **ladění** a potom **spustit ladění** toorun hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="c5acf-150">Click **Debug** and then **Start Debugging** toorun hello application.</span></span> <span data-ttu-id="c5acf-151">Ho může trvat několik minut, než toorun nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="c5acf-151">It may take several minutes for hello deployment toorun.</span></span>

4. <span data-ttu-id="c5acf-152">tooverify, kterou vaše aplikace přidali hello novou službu IoT hub, navštivte hello [portál Azure] [ lnk-azure-portal] a zobrazení seznamu prostředků.</span><span class="sxs-lookup"><span data-stu-id="c5acf-152">tooverify that your application added hello new IoT hub, visit hello [Azure portal][lnk-azure-portal] and view your list of resources.</span></span> <span data-ttu-id="c5acf-153">Můžete taky použít hello **Get-AzureRmResource** rutiny prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="c5acf-153">Alternatively, use hello **Get-AzureRmResource** PowerShell cmdlet.</span></span>

> [!NOTE]
> <span data-ttu-id="c5acf-154">Tato ukázková aplikace přidá S1 Standard IoT Hub pro kterou se účtují.</span><span class="sxs-lookup"><span data-stu-id="c5acf-154">This example application adds an S1 Standard IoT Hub for which you are billed.</span></span> <span data-ttu-id="c5acf-155">Jakmile budete hotovi, můžete odstranit hello IoT hub prostřednictvím hello [portál Azure] [ lnk-azure-portal] nebo pomocí hello **odebrat AzureRmResource** rutiny prostředí PowerShell po dokončení.</span><span class="sxs-lookup"><span data-stu-id="c5acf-155">When you are finished, you can delete hello IoT hub through hello [Azure portal][lnk-azure-portal] or by using hello **Remove-AzureRmResource** PowerShell cmdlet when you are finished.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c5acf-156">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c5acf-156">Next steps</span></span>
<span data-ttu-id="c5acf-157">Nyní jste nasadili služby IoT hub pomocí zprostředkovatele prostředků hello REST API, může být vhodné tooexplore dále:</span><span class="sxs-lookup"><span data-stu-id="c5acf-157">Now you have deployed an IoT hub using hello resource provider REST API, you may want tooexplore further:</span></span>

* <span data-ttu-id="c5acf-158">Přečtěte si informace o možnostech hello hello [zprostředkovatele prostředků služby IoT Hub REST API][lnk-rest-api].</span><span class="sxs-lookup"><span data-stu-id="c5acf-158">Read about hello capabilities of hello [IoT Hub resource provider REST API][lnk-rest-api].</span></span>
* <span data-ttu-id="c5acf-159">Čtení [přehled Azure Resource Manageru] [ lnk-azure-rm-overview] toolearn více informací o hello možnosti nástroje Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="c5acf-159">Read [Azure Resource Manager overview][lnk-azure-rm-overview] toolearn more about hello capabilities of Azure Resource Manager.</span></span>

<span data-ttu-id="c5acf-160">toolearn Další informace o vývoji pro IoT Hub, najdete v části hello následující články:</span><span class="sxs-lookup"><span data-stu-id="c5acf-160">toolearn more about developing for IoT Hub, see hello following articles:</span></span>

* <span data-ttu-id="c5acf-161">[Úvod tooC SDK][lnk-c-sdk]</span><span class="sxs-lookup"><span data-stu-id="c5acf-161">[Introduction tooC SDK][lnk-c-sdk]</span></span>
* <span data-ttu-id="c5acf-162">[Sady SDK služby Azure IoT][lnk-sdks]</span><span class="sxs-lookup"><span data-stu-id="c5acf-162">[Azure IoT SDKs][lnk-sdks]</span></span>

<span data-ttu-id="c5acf-163">toofurther prozkoumat hello služby IoT Hub, najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="c5acf-163">toofurther explore hello capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="c5acf-164">[Simulaci zařízení s Azure IoT Edge][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="c5acf-164">[Simulating a device with Azure IoT Edge][lnk-iotedge]</span></span>

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-azure-portal]: https://portal.azure.com/
[lnk-status]: https://azure.microsoft.com/status/
[lnk-powershell-install]: https://docs.microsoft.com/powershell/azure/install-azurerm-ps
[lnk-rest-api]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-azure-rm-overview]: ../azure-resource-manager/resource-group-overview.md

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
