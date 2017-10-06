---
title: "aaaCustomizing předkonfigurovanými řešeními | Microsoft Docs"
description: "Obsahuje pokyny k jak toocustomize hello Azure IoT Suite předkonfigurovaných řešení."
services: 
suite: iot-suite
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 4653ae53-4110-4a10-bd6c-7dc034c293a8
ms.service: iot-suite
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/15/2017
ms.author: corywink
ms.openlocfilehash: 1a8573f5ac6ed944c44459df495446f15174d513
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="customize-a-preconfigured-solution"></a>Přizpůsobení předkonfigurovaného řešení

Hello předkonfigurované řešení součástí hello Azure IoT Suite ukázka hello služby v rámci hello suite pracovní společně toodeliver-komplexní řešení. V tento počáteční bod existují různé míst, ve kterých můžete rozšířit a přizpůsobit hello řešení pro konkrétní scénáře. Hello následující části popisují tyto společné body přizpůsobení.

## <a name="find-hello-source-code"></a>Najít hello zdrojového kódu

zdrojový kód Hello hello předkonfigurované řešení je k dispozici na Githubu v hello následující úložiště:

* Vzdálené monitorování: [https://www.github.com/Azure/azure-iot-remote-monitoring](https://github.com/Azure/azure-iot-remote-monitoring)
* Prediktivní údržby: [https://github.com/Azure/azure-iot-predictive-maintenance](https://github.com/Azure/azure-iot-predictive-maintenance)
* Připojené factory: [https://github.com/Azure/azure-iot-connected-factory](https://github.com/Azure/azure-iot-connected-factory)

zdrojový kód Hello hello předkonfigurované řešení je zadaný toodemonstrate hello vzory a postupy používané funkce začátku do konce hello tooimplement řešení IoT pomocí sady Azure IoT Suite. Můžete najít další informace o toobuild a nasadit řešení hello v úložišť GitHub hello.

## <a name="change-hello-preconfigured-rules"></a>Změna pravidel hello předkonfigurované

Hello řešení vzdáleného monitorování obsahuje tři [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) úlohy toohandle informace o zařízení, telemetrie a logiku pravidla v řešení hello.

Hello tři stream analytics úloh a jejich syntaxe jsou popsány podrobněji v hello [vzdálené monitorování návod pro předkonfigurované řešení](iot-suite-remote-monitoring-sample-walkthrough.md). 

Tyto úlohy můžete upravit přímo tooalter hello logiku, nebo přidejte logiku tooyour konkrétní scénáře. Můžete najít hello úlohy Stream Analytics následujícím způsobem:

1. Přejděte příliš[portál Azure](https://portal.azure.com).
2. Přejděte toohello skupinu prostředků s hello stejný název jako řešení IoT. 
3. Vyberte úlohy Azure Stream Analytics hello chcete toomodify. 
4. Úlohu zastavení hello výběrem **Zastavit** v hello sadu příkazů. 
5. Upravte hello vstupy, dotazů a výstupy.
   
    Jednoduchých úprav je toochange hello dotazu pro hello **pravidla** toouse úlohy **"<"** místo **">"**. portál řešení Hello stále zobrazuje **">"** při upravit pravidlo, ale Všimněte si, jak je z důvodu změn toohello v hello základní úlohy obráceně hello chování.
6. Spuštění úlohy hello

> [!NOTE]
> vzdálené monitorování řídicí panel Hello závisí na konkrétní data, tak změnu hello úloh může způsobit toofail hello řídicího panelu.

## <a name="add-your-own-rules"></a>Přidání vlastních pravidel

Kromě toho toochanging hello předkonfigurované úlohy Azure Stream Analytics, můžete použít nové úlohy Azure portálu tooadd hello nebo přidat nové úlohy tooexisting dotazy.

## <a name="customize-devices"></a>Vlastní nastavení zařízení

Mezi nejběžnější aktivity rozšíření hello ve spolupráci s konkrétní tooyour scénář zařízení. Existuje několik metod pro práci s zařízení. Tyto metody zahrnují změny simulované zařízení toomatch váš scénář, nebo pomocí hello [sady SDK zařízení IoT] [ IoT Device SDK] tooconnect řešení toohello fyzického zařízení.

Podrobný průvodce tooadding zařízeních, najdete v části hello [Iot Suite připojení zařízení](iot-suite-connecting-devices.md) článek a hello [vzdálené monitorování C SDK – ukázka](https://github.com/Azure/azure-iot-sdk-c/tree/master/serializer/samples/remote_monitoring). Tato ukázka je navrženou toowork s hello předkonfigurovanému řešení vzdáleného monitorování.

### <a name="create-your-own-simulated-device"></a>Vytvoření simulovaného zařízení

Součástí hello [pro vzdálené monitorování řešení zdrojového kódu](https://github.com/Azure/azure-iot-remote-monitoring), je simulátoru .NET. Tato simulátoru je hello jeden zřízený jako součást řešení hello a můžete změnu toosend rozdílná metadata, telemetrických dat a odpovědět, toodifferent příkazy a metody.

předkonfigurované simulátoru Hello v hello předkonfigurovanému řešení vzdáleného monitorování simuluje chladič zařízení, který vysílá telemetrická data teploty a vlhkosti. Můžete upravit hello simulátoru v hello [Simulator.WebJob](https://github.com/Azure/azure-iot-remote-monitoring/tree/master/Simulator/Simulator.WebJob) projektu, když jste forked úložiště GitHub hello.

### <a name="available-locations-for-simulated-devices"></a>K dispozici umístění pro simulované zařízení

Sada Hello výchozí umístění je v Praze/Redmond, Washington, USA. Můžete změnit tato místa v [SampleDeviceFactory.cs][lnk-sample-device-factory].

### <a name="add-a-desired-property-update-handler-toohello-simulator"></a>Přidat simulátoru toohello obslužná rutina aktualizace požadované vlastnosti

Můžete nastavit hodnotu pro požadovanou vlastnost pro zařízení na portálu řešení hello. Pokud zařízení hello načte hodnotu vlastnosti hello potřeby je hello odpovědností hello zařízení toohandle hello vlastnost žádosti o změnu. Podpora tooadd pro změnu hodnoty vlastnosti prostřednictvím požadovanou vlastnost, je nutné tooadd simulátoru toohello obslužné rutiny.

Simulátor Hello obsahuje obslužné rutiny pro hello **SetPointTemp** a **TelemetryInterval** vlastnosti, které lze aktualizovat nastavením požadované hodnoty v portálu řešení hello.

Hello následující příklad ukazuje hello obslužné rutiny pro hello **SetPointTemp** potřeby vlastnost hello **CoolerDevice** třídy:

```csharp
protected async Task OnSetPointTempUpdate(object value)
{
    var telemetry = _telemetryController as ITelemetryWithSetPointTemperature;
    telemetry.SetPointTemperature = Convert.ToDouble(value);

    await SetReportedPropertyAsync(SetPointTempPropertyName, telemetry.SetPointTemperature);
}
```

Tato metoda hello telemetrie bod teploty a pak sestavy hello změnit back tooIoT rozbočovače nastavením hlášené vlastnosti aktualizace.

Pomocí následující vzor hello v předchozím příkladu hello můžete přidat svoje vlastní obslužné rutiny pro vlastní vlastnosti.

Je třeba také svázat obslužná rutina toohello hello požadovanou vlastnost, jak ukazuje následující příklad z hello hello **CoolerDevice** konstruktor:

```csharp
_desiredPropertyUpdateHandlers.Add(SetPointTempPropertyName, OnSetPointTempUpdate);
```

Všimněte si, že **SetPointTempPropertyName** konstanta definovaný jako "Config.SetPointTemp".

### <a name="add-support-for-a-new-method-toohello-simulator"></a>Přidat podporu pro nové simulátoru toohello – metoda

Můžete přizpůsobit hello simulátoru tooadd podporu pro nové [– metoda (přímá metoda)][lnk-direct-methods]. Existují dva klíče kroky:

- Simulátor Hello musíte upozornit centra IoT hello hello předkonfigurované řešení s podrobnostmi metody hello.
- Simulátor Hello musí obsahovat volání metody hello toohandle kód při vyvolání z hello **podrobnosti o zařízení** panely v Průzkumníku řešení hello nebo prostřednictvím úlohu.

Hello vzdálené monitorování předkonfigurované řešení používá *hlášené vlastnosti* toosend podrobnosti o podporovaných metod tooIoT rozbočovače. back-end Hello řešení udržuje seznam všech metod hello podporovaných každé zařízení společně s historie volání metod. Můžete zobrazit tyto informace o zařízeních a volat metody na portálu řešení hello.

toonotify hello Centrum IoT hub, zařízení podporuje metodu hello zařízení musíte přidat podrobnosti o hello metoda toohello **SupportedMethods** uzel v hello hlášené vlastnosti:

```json
"SupportedMethods": {
  "<method signature>": "<method description>",
  "<method signature>": "<method description>"
}
```

Hello podpis metody má hello následující formát: `<method name>--<parameter #0 name>-<parameter #1 type>-...-<parameter #n name>-<parameter #n type>`. Například toospecify hello **InitiateFirmwareUpdate** metoda očekává parametr řetězec s názvem **FwPackageURI**, použijte následující podpis metody hello:

```json
InitiateFirmwareUpate--FwPackageURI-string: "description of method"
```

Seznam typů podporovaných parametrů najdete v tématu hello **CommandTypes** třídy v projektu OMI hello.

toodelete metodu, nastavení hello metoda podpis příliš`null` v hello hlášené vlastnosti.

> [!NOTE]
> Hello back-end řešení aktualizuje pouze informace o podporovaných metod při přijetí *informace o zařízení* zpráv ze zařízení hello.

Následující ukázka kódu z hello Hello **SampleDeviceFactory** třídy v hello běžné projektu ukazuje, jak tooadd seznam metoda toohello z **SupportedMethods** v hello hlášené vlastnosti poslal hello zařízení:

```csharp
device.Commands.Add(new Command(
    "InitiateFirmwareUpdate",
    DeliveryType.Method,
    "Updates device Firmware. Use parameter 'FwPackageUri' toospecifiy hello URI of hello firmware file, e.g. https://iotrmassets.blob.core.windows.net/firmwares/FW20.bin",
    new[] { new Parameter("FwPackageUri", "string") }
));
```

Tento fragment kódu přidá podrobnosti o hello **InitiateFirmwareUpdate** metoda včetně toodisplay textu hello portál řešení a podrobnosti o hello požadované parametry metody.

Simulátor Hello odešle hlášené vlastnosti, včetně hello seznam podporovaných metod tooIoT centra, když se spustí simulátoru hello.

Přidejte kód simulátoru toohello obslužné rutiny pro každou metodu, kterou podporuje. Zobrazí se stávající obslužné rutiny hello v hello **CoolerDevice** – třída v projektu Simulator.WebJob hello. Hello následující příklad ukazuje hello obslužné rutiny pro **InitiateFirmwareUpdate** metoda:

```csharp
public async Task<MethodResponse> OnInitiateFirmwareUpdate(MethodRequest methodRequest, object userContext)
{
    if (_deviceManagementTask != null && !_deviceManagementTask.IsCompleted)
    {
        return await Task.FromResult(BuildMethodRespose(new
        {
            Message = "Device is busy"
        }, 409));
    }

    try
    {
        var operation = new FirmwareUpdate(methodRequest);
        _deviceManagementTask = operation.Run(Transport).ContinueWith(async task =>
        {
            // after firmware completed, we reset telemetry
            var telemetry = _telemetryController as ITelemetryWithTemperatureMeanValue;
            if (telemetry != null)
            {
                telemetry.TemperatureMeanValue = 34.5;
            }

            await UpdateReportedTemperatureMeanValue();
        });

        return await Task.FromResult(BuildMethodRespose(new
        {
            Message = "FirmwareUpdate accepted",
            Uri = operation.Uri
        }));
    }
    catch (Exception ex)
    {
        return await Task.FromResult(BuildMethodRespose(new
        {
            Message = ex.Message
        }, 400));
    }
}
```

Metoda obslužná rutina názvy musí začínat `On` následuje název hello hello metody. Hello **methodRequest** parametr obsahuje žádné parametry předané pomocí volání metody hello z back-end hello řešení. Hello návratová hodnota musí být typu **úloh&lt;MethodResponse&gt;**. Hello **BuildMethodResponse** nástroj metoda vám pomůže vytvořit hello návratovou hodnotu.

Uvnitř hello metoda obslužnou rutinu vám může:

- Spustí asynchronní úlohu.
- Načíst požadované vlastnosti z hello *dvojče zařízení* IoT hub.
- Aktualizovat vlastnosti jediné hlášené pomocí hello **SetReportedPropertyAsync** metoda v hello **CoolerDevice** třídy.
- Aktualizovat více hlášené vlastnosti tak, že vytvoříte **TwinCollection** instance a volání hello **Transport.UpdateReportedPropertiesAsync** metoda.

Hello předchozí příklad aktualizace firmwaru provádí hello následující kroky:

- Kontroly hello zařízení je možné tooaccept žádost o aktualizaci firmwaru hello.
- Asynchronně spustí operace aktualizace firmwaru hello a obnoví hello telemetrie při dokončení operace hello.
- Hned vrátí hello "FirmwareUpdate přijata" zpráva tooindicate hello žádost byla přijata hello zařízení.

### <a name="build-and-use-your-own-physical-device"></a>Vytvoření a použití si vlastní zařízení (fyzické)

Hello [SDK služby Azure IoT](https://github.com/Azure/azure-iot-sdks) poskytují knihovny pro připojení (jazyky a operační systémy) celou řadu typů zařízení do řešení IoT.

## <a name="modify-dashboard-limits"></a>Upravit omezení řídicí panel

### <a name="number-of-devices-displayed-in-dashboard-dropdown"></a>Počet zařízení, které jsou zobrazeny v rozevírací nabídce řídicí panel

Hello výchozí hodnota je 200. Můžete změnit toto číslo v [DashboardController.cs][lnk-dashboard-controller].

### <a name="number-of-pins-toodisplay-in-bing-map-control"></a>Počet kódů PIN toodisplay v ovládacím prvku mapy Bing

Hello výchozí hodnota je 200. Můžete změnit toto číslo v [TelemetryApiController.cs][lnk-telemetry-api-controller-01].

### <a name="time-period-of-telemetry-graph"></a>Časové období telemetrických dat grafu

Hello výchozí hodnota je 10 minut. Můžete změnit tuto hodnotu v [TelmetryApiController.cs][lnk-telemetry-api-controller-02].

## <a name="manually-set-up-application-roles"></a>Ručně nastavit aplikační role

Hello následující postup popisuje, jak tooadd **správce** a **jen pro čtení** aplikace role tooa předkonfigurované řešení. Předkonfigurovaná řešení, které jsou už zřízené z webu azureiotsuite.com hello obsahují hello **správce** a **jen pro čtení** role.

Členové hello **jen pro čtení** role můžete zobrazit seznam zařízení hello a hello řídicího panelu, ale nejsou povoleny tooadd zařízení, změny atributů zařízení nebo odesílání příkazů.  Členové hello **správce** role mají plný přístup tooall hello funkci v řešení hello.

1. Přejděte toohello [portál Azure classic][lnk-classic-portal].
2. Vyberte **služby Active Directory**.
3. Klikněte na název hello hello AAD klienta, který jste použili při zřízení řešení.
4. Klikněte na tlačítko **aplikace**.
5. Klikněte na název hello hello aplikace, která odpovídá názvu vašeho předkonfigurované řešení. Pokud nevidíte aplikaci v seznamu hello, vyberte **aplikace Moje společnost vlastní** v hello **zobrazit** rozevíracího seznamu a klikněte na tlačítko hello zaškrtnutí.
6. V dolní části hello hello stránky, klikněte na tlačítko **spravovat Manifest** a potom **stáhnout Manifest**.
7. Tento postup stáhne .json souboru tooyour místním počítači. Umožňuje otevřete tento soubor pro úpravy v textovém editoru podle svého výběru.
8. Na třetí řádek hello hello .json souboru se zobrazí:

   ```json
   "appRoles" : [],
   ```
   Nahraďte tento řádek hello následující kód:

   ```json
   "appRoles": [
   {
   "allowedMemberTypes": [
   "User"
   ],
   "description": "Administrator access toohello application",
   "displayName": "Admin",
   "id": "a400a00b-f67c-42b7-ba9a-f73d8c67e433",
   "isEnabled": true,
   "value": "Admin"
   },
   {
   "allowedMemberTypes": [
   "User"
   ],
   "description": "Read only access toodevice information",
   "displayName": "Read Only",
   "id": "e5bbd0f5-128e-4362-9dd1-8f253c6082d7",
   "isEnabled": true,
   "value": "ReadOnly"
   } ],
   ```

9. Uložte soubor .json aktualizované hello (můžete přepsat existující soubor hello).
10. V hello portál Azure classic, v dolní části hello hello stránky, vyberte **spravovat Manifest** pak **nahrát Manifest** soubor .json hello tooupload jste uložili v předchozím kroku hello.
11. Nyní jste přidali hello **správce** a **jen pro čtení** role tooyour aplikace.
12. tooassign jednu z těchto rolí uživatele tooa ve vašem adresáři, najdete v části [oprávnění na webu azureiotsuite.com hello][lnk-permissions].

## <a name="feedback"></a>Váš názor

Máte přizpůsobení chcete toosee zahrnuté v tomto dokumentu? Přidání funkce návrhy příliš[User Voice](https://feedback.azure.com/forums/321918-azure-iot), nebo komentář k tomuto článku. 

## <a name="next-steps"></a>Další kroky

toolearn Další informace o hello možností pro přizpůsobení hello předkonfigurované řešení, najdete v části:

* [Připojení aplikace logiky tooyour Azure IoT Suite vzdálené monitorování předkonfigurované řešení][lnk-logicapp]
* [Použití dynamické telemetrie s hello předkonfigurovanému řešení vzdáleného monitorování][lnk-dynamic]
* [Informace metadat zařízení v hello předkonfigurovanému řešení vzdáleného monitorování][lnk-devinfo]
* [Přizpůsobit způsob hello připojen objekt pro vytváření řešení zobrazí data z vašich serverů OPC UA][lnk-cf-customize]

[lnk-logicapp]: iot-suite-logic-apps-tutorial.md
[lnk-dynamic]: iot-suite-dynamic-telemetry.md
[lnk-devinfo]: iot-suite-remote-monitoring-device-info.md

[IoT Device SDK]: https://azure.microsoft.com/documentation/articles/iot-hub-sdks-summary/
[lnk-permissions]: iot-suite-permissions.md
[lnk-dashboard-controller]: https://github.com/Azure/azure-iot-remote-monitoring/blob/3fd43b8a9f7e0f2774d73f3569439063705cebe4/DeviceAdministration/Web/Controllers/DashboardController.cs#L27
[lnk-telemetry-api-controller-01]: https://github.com/Azure/azure-iot-remote-monitoring/blob/3fd43b8a9f7e0f2774d73f3569439063705cebe4/DeviceAdministration/Web/WebApiControllers/TelemetryApiController.cs#L27
[lnk-telemetry-api-controller-02]: https://github.com/Azure/azure-iot-remote-monitoring/blob/e7003339f73e21d3930f71ceba1e74fb5c0d9ea0/DeviceAdministration/Web/WebApiControllers/TelemetryApiController.cs#L25 
[lnk-sample-device-factory]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Common/Factory/SampleDeviceFactory.cs#L40
[lnk-classic-portal]: https://manage.windowsazure.com
[lnk-direct-methods]: ../iot-hub/iot-hub-devguide-direct-methods.md
[lnk-cf-customize]: iot-suite-connected-factory-customize.md