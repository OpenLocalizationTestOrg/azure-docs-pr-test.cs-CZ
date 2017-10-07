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
# <a name="create-an-iot-hub-using-hello-resource-provider-rest-api-net"></a>Vytvoření služby IoT hub pomocí zprostředkovatele prostředků hello rozhraní API REST (.NET)

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

Můžete použít hello [zprostředkovatele prostředků služby IoT Hub REST API] [ lnk-rest-api] toocreate a spravovat Azure IoT hubs prostřednictvím kódu programu. Tento kurz ukazuje, jak toouse hello toocreate REST API poskytovatele prostředků Centrum IoT Centrum IoT z programu v C#.

> [!NOTE]
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Azure Resource Manager a klasický](../azure-resource-manager/resource-manager-deployment-model.md).  Tento článek se zabývá pomocí modelu nasazení Azure Resource Manager hello.

toocomplete tohoto kurzu budete potřebovat hello následující:

* Visual Studio 2015 nebo Visual Studio 2017.
* Aktivní účet Azure. <br/>Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].
* [Azure PowerShell 1.0] [ lnk-powershell-install] nebo novější.

[!INCLUDE [iot-hub-prepare-resource-manager](../../includes/iot-hub-prepare-resource-manager.md)]

## <a name="prepare-your-visual-studio-project"></a>Příprava projektu sady Visual Studio

1. V sadě Visual Studio vytvořte projekt Visual C# Windows klasický desktopový pomocí hello **konzolovou aplikaci (rozhraní .NET Framework)** šablona projektu. Název projektu hello **CreateIoTHubREST**.

2. V Průzkumníku řešení klikněte pravým tlačítkem na projekt a pak klikněte na tlačítko **spravovat balíčky NuGet**.

3. Zkontrolujte v Správce balíčků NuGet **zahrnout předběžné verze**a na hello **Procházet** stránky hledání **Microsoft.Azure.Management.ResourceManager**. Vyberte hello balíčku, klikněte na **nainstalovat**v **zkontrolujte změny** klikněte na tlačítko **OK**, pak klikněte na tlačítko **souhlasím** tooaccept hello licence.

4. Vyhledejte v Správce balíčků NuGet **Microsoft.IdentityModel.Clients.ActiveDirectory**.  Klikněte na tlačítko **nainstalovat**v **zkontrolujte změny** klikněte na tlačítko **OK**, pak klikněte na tlačítko **souhlasím** tooaccept hello licence.

5. V Program.cs místo existující hello **pomocí** příkazy s hello následující kód:

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

6. V souboru Program.cs přidejte následující statické proměnné nahraďte zástupný symbol hodnoty hello hello. Učinit poznámku o **ApplicationId**, **SubscriptionId**, **TenantId**, a **heslo** výše v tomto kurzu. **Název skupiny prostředků** je hello název skupiny prostředků hello použijete při vytváření centra IoT hello. Můžete použít existující nebo novou skupinu prostředků. **Název centra IoT** je název hello hello IoT Hub, které vytvoříte, jako například **MyIoTHub**. Hello název služby IoT hub musí být globálně jedinečný. **Název nasazení** , jako je název pro nasazení hello **Deployment_01**.

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

## <a name="use-hello-resource-provider-rest-api-toocreate-an-iot-hub"></a>Použít hello prostředků poskytovatele rozhraní REST API toocreate služby IoT hub

Použití hello [zprostředkovatele prostředků služby IoT Hub REST API] [ lnk-rest-api] toocreate služby IoT hub ve vaší skupině prostředků. Můžete také použít hello prostředků poskytovatele rozhraní REST API toomake změny tooan stávající služby IoT hub.

1. Přidejte následující metodu tooProgram.cs hello:

    ```csharp
    static void CreateIoTHub(string token)
    {

    }
    ```

2. Přidejte následující kód toohello hello **CreateIoTHub** metoda. Tento kód vytvoří **HttpClient** objekt s hello ověřovací token v hlavičkách hello:

    ```csharp
    HttpClient client = new HttpClient();
    client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", token);
    ```

3. Přidejte následující kód toohello hello **CreateIoTHub** metoda. Tento kód popisuje hello IoT hub toocreate a generuje reprezentaci JSON. Hello aktuální seznam umístění, které podporují služby IoT Hub naleznete v části [stavu Azure][lnk-status]:

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

4. Přidejte následující kód toohello hello **CreateIoTHub** metoda. Tento kód odešle tooAzure požadavku REST hello. Hello kód pak zkontroluje odpověď hello a načte hello adresu URL můžete použít stav hello toomonitor úloha nasazení hello:

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

5. Přidejte následující kód toohello konec hello hello **CreateIoTHub** metoda. Tento kód používá hello **asyncStatusUri** získat adresu v toowait hello předchozí krok pro nasazení toocomplete hello:

    ```csharp
    string body;
    do
    {
      Thread.Sleep(10000);
      HttpResponseMessage deploymentstatus = client.GetAsync(asyncStatusUri).Result;
      body = deploymentstatus.Content.ReadAsStringAsync().Result;
    } while (body == "{\"status\":\"Running\"}");
    ```

6. Přidejte následující kód toohello konec hello hello **CreateIoTHub** metoda. Tento kód načte hello klíče hello služby IoT hub jste vytvořili a vypíše je toohello konzoly:

    ```csharp
    var listKeysUri = string.Format("https://management.azure.com/subscriptions/{0}/resourceGroups/{1}/providers/Microsoft.Devices/IotHubs/{2}/IoTHubKeys/listkeys?api-version=2016-02-03", subscriptionId, rgName, iotHubName);
    var keysresults = client.PostAsync(listKeysUri, null).Result;

    Console.WriteLine("Keys: {0}", keysresults.Content.ReadAsStringAsync().Result);
    ```

## <a name="complete-and-run-hello-application"></a>Hello úplný a spuštění aplikace

Hello aplikace je nyní možné dokončit pomocí volání hello **CreateIoTHub** metoda předtím, než můžete sestavit a spustit ho.

1. Přidejte následující kód toohello konec hello hello **hlavní** metoda:

    ```csharp
    CreateIoTHub(token.AccessToken);
    Console.ReadLine();
    ```

2. Klikněte na tlačítko **sestavení** a potom **sestavení řešení**. Opravte všechny chyby.

3. Klikněte na tlačítko **ladění** a potom **spustit ladění** toorun hello aplikace. Ho může trvat několik minut, než toorun nasazení hello.

4. tooverify, kterou vaše aplikace přidali hello novou službu IoT hub, navštivte hello [portál Azure] [ lnk-azure-portal] a zobrazení seznamu prostředků. Můžete taky použít hello **Get-AzureRmResource** rutiny prostředí PowerShell.

> [!NOTE]
> Tato ukázková aplikace přidá S1 Standard IoT Hub pro kterou se účtují. Jakmile budete hotovi, můžete odstranit hello IoT hub prostřednictvím hello [portál Azure] [ lnk-azure-portal] nebo pomocí hello **odebrat AzureRmResource** rutiny prostředí PowerShell po dokončení.

## <a name="next-steps"></a>Další kroky
Nyní jste nasadili služby IoT hub pomocí zprostředkovatele prostředků hello REST API, může být vhodné tooexplore dále:

* Přečtěte si informace o možnostech hello hello [zprostředkovatele prostředků služby IoT Hub REST API][lnk-rest-api].
* Čtení [přehled Azure Resource Manageru] [ lnk-azure-rm-overview] toolearn více informací o hello možnosti nástroje Azure Resource Manager.

toolearn Další informace o vývoji pro IoT Hub, najdete v části hello následující články:

* [Úvod tooC SDK][lnk-c-sdk]
* [Sady SDK služby Azure IoT][lnk-sdks]

toofurther prozkoumat hello služby IoT Hub, najdete v tématu:

* [Simulaci zařízení s Azure IoT Edge][lnk-iotedge]

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
