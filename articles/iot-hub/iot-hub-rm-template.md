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
# <a name="create-an-iot-hub-using-azure-resource-manager-template-net"></a>Vytvoření služby IoT hub pomocí šablony Azure Resource Manageru (.NET)

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

Můžete použít Azure Resource Manager toocreate a spravovat Azure IoT hubs prostřednictvím kódu programu. Tento kurz ukazuje, jak toouse toocreate šablony Azure Resource Manager Centrum IoT z programu v C#.

> [!NOTE]
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Azure Resource Manager a klasický](../azure-resource-manager/resource-manager-deployment-model.md).  Tento článek se zabývá pomocí modelu nasazení Azure Resource Manager hello.

toocomplete tohoto kurzu budete potřebovat hello následující:

* Visual Studio 2015 nebo Visual Studio 2017.
* Aktivní účet Azure. <br/>Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].
* [Účet úložiště Azure] [ lnk-storage-account] kam můžete ukládat soubory šablony Azure Resource Manager.
* [Azure PowerShell 1.0] [ lnk-powershell-install] nebo novější.

[!INCLUDE [iot-hub-prepare-resource-manager](../../includes/iot-hub-prepare-resource-manager.md)]

## <a name="prepare-your-visual-studio-project"></a>Příprava projektu sady Visual Studio

1. V sadě Visual Studio vytvořte projekt Visual C# Windows klasický desktopový pomocí hello **konzolovou aplikaci (rozhraní .NET Framework)** šablona projektu. Název projektu hello **CreateIoTHub**.

2. V Průzkumníku řešení klikněte pravým tlačítkem na projekt a pak klikněte na tlačítko **spravovat balíčky NuGet**.

3. Zkontrolujte v Správce balíčků NuGet **zahrnout předběžné verze**a na hello **Procházet** stránky hledání **Microsoft.Azure.Management.ResourceManager**. Vyberte hello balíčku, klikněte na **nainstalovat**v **zkontrolujte změny** klikněte na tlačítko **OK**, pak klikněte na tlačítko **souhlasím** tooaccept hello licence.

4. Vyhledejte v Správce balíčků NuGet **Microsoft.IdentityModel.Clients.ActiveDirectory**.  Klikněte na tlačítko **nainstalovat**v **zkontrolujte změny** klikněte na tlačítko **OK**, pak klikněte na tlačítko **souhlasím** tooaccept hello licence.

5. V Program.cs místo existující hello **pomocí** příkazy s hello následující kód:

    ```csharp
    using System;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.ResourceManager.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.Rest;
    ```

6. V souboru Program.cs přidejte následující statické proměnné nahraďte zástupný symbol hodnoty hello hello. Učinit poznámku o **ApplicationId**, **SubscriptionId**, **TenantId**, a **heslo** výše v tomto kurzu. **Název účtu úložiště Azure** je název hello hello účet úložiště Azure, kde můžete ukládat soubory šablony Azure Resource Manager. **Název skupiny prostředků** je hello název skupiny prostředků hello použijete při vytváření centra IoT hello. Název Hello může být existující nebo nové skupiny prostředků. **Název nasazení** , jako je název pro nasazení hello **Deployment_01**.

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

## <a name="submit-a-template-toocreate-an-iot-hub"></a>Odeslání toocreate šablony služby IoT hub

Použijte JSON šablony a parametr souboru toocreate služby IoT hub ve vaší skupině prostředků. Můžete použít také Azure Resource Manager šablony toomake změny tooan stávající služby IoT hub.

1. V Průzkumníku řešení klikněte pravým tlačítkem na projekt, klikněte na tlačítko **přidat**a potom klikněte na **novou položku**. Přidejte soubor JSON s názvem **template.json** tooyour projektu.

2. tooadd standardní toohello centra IoT **východní USA** oblast, nahraďte obsah hello **template.json** s hello následující definici prostředků. Hello aktuální seznam oblastí, které podporují služby IoT Hub naleznete v části [stavu Azure][lnk-status]:

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

3. V Průzkumníku řešení klikněte pravým tlačítkem na projekt, klikněte na tlačítko **přidat**a potom klikněte na **novou položku**. Přidejte soubor JSON s názvem **Parameters.JSON tímto kódem** tooyour projektu.

4. Nahraďte obsah hello **Parameters.JSON tímto kódem** s hello následující informace o parametrech, které nastavuje název hello novou službu IoT hub, jako **{vašimi iniciálami} mynewiothub**. název centra IoT Hello musí být globálně jedinečný, aby měl by obsahovat název nebo initials:

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

5. V **Průzkumníka serveru**, připojit tooyour předplatného Azure a ve službě Azure Storage účet vytvořit kontejner s názvem **šablony**. V hello **vlastnosti** panelu, sada hello **veřejný přístup pro čtení** oprávnění pro hello **šablony** kontejneru příliš**Blob**.

6. V **Průzkumníka serveru**, klikněte pravým tlačítkem na hello **šablony** kontejneru a pak klikněte na tlačítko **kontejner objektů Blob zobrazení**. Klikněte na tlačítko hello **nahrát objekt Blob** tlačítko, vyberte hello dva soubory, **Parameters.JSON tímto kódem** a **templates.json**a potom klikněte na **otevřete** tooupload hello JSON soubory toohello **šablony** kontejneru. adresy URL Hello objektů BLOB hello obsahující hello JSON data jsou:

    ```csharp
    https://{Your storage account name}.blob.core.windows.net/templates/parameters.json
    https://{Your storage account name}.blob.core.windows.net/templates/template.json
    ```
7. Přidejte následující metodu tooProgram.cs hello:

    ```csharp
    static void CreateIoTHub(ResourceManagementClient client)
    {

    }
    ```

8. Přidejte následující kód toohello hello **CreateIoTHub** metoda toosubmit hello šablony a parametr soubory toohello Azure Resource Manager:

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

9. Přidejte následující kód toohello hello **CreateIoTHub** metoda, která se zobrazuje stav hello a hello klíče pro hello novou službu IoT hub:

    ```csharp
    string state = createResponse.Properties.ProvisioningState;
    Console.WriteLine("Deployment state: {0}", state);

    if (state != "Succeeded")
    {
      Console.WriteLine("Failed toocreate iothub");
    }
    Console.WriteLine(createResponse.Properties.Outputs);
    ```

## <a name="complete-and-run-hello-application"></a>Hello úplný a spuštění aplikace

Hello aplikace je nyní možné dokončit pomocí volání hello **CreateIoTHub** metoda předtím, než můžete sestavit a spustit ho.

1. Přidejte následující kód toohello konec hello hello **hlavní** metoda:

    ```csharp
    CreateIoTHub(client);
    Console.ReadLine();
    ```

2. Klikněte na tlačítko **sestavení** a potom **sestavení řešení**. Opravte všechny chyby.

3. Klikněte na tlačítko **ladění** a potom **spustit ladění** toorun hello aplikace. Ho může trvat několik minut, než toorun nasazení hello.

4. tooverify přidat aplikace hello novou službu IoT hub, navštivte hello [portál Azure] [ lnk-azure-portal] a zobrazení seznamu prostředků. Můžete taky použít hello **Get-AzureRmResource** rutiny prostředí PowerShell.

> [!NOTE]
> Tato ukázková aplikace přidá S1 Standard IoT Hub pro kterou se účtují. Odstraněním hello IoT hub prostřednictvím hello [portál Azure] [ lnk-azure-portal] nebo pomocí hello **odebrat AzureRmResource** rutiny prostředí PowerShell po dokončení.

## <a name="next-steps"></a>Další kroky
Nyní jste nasadili služby IoT hub pomocí šablony Azure Resource Manageru pomocí programu v C#, můžete další tooexplore:

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
[lnk-storage-account]:../storage/common/storage-create-storage-account.md

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
