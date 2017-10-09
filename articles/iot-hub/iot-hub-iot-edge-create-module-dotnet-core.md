---
title: "aaaCreate modul Azure IoT Edge pomocí C# | Microsoft Docs"
description: "V tomto kurzu hodnotí, jak zakázat datovou převaděč modulu pomocí toowrite hello nejnovější balíčky Azure IoT Edge NuGet, Visual Studio Code a C#."
services: iot-hub
author: jeffreyCline
manager: timlt
keywords: Azure iot, kurz, modul, nuget, vscode, csharp, edge
ms.service: iot-hub
ms.devlang: csharp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2017
ms.author: jcline
ms.openlocfilehash: b104609c05d1613e21acc7d7bed547f311179151
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-iot-edge-module-with-cx23"></a>Vytvořte modul Azure IoT Edge s C & #x23;

V tomto kurzu umožňující prezentovat jak toocreate a modul pro `Azure IoT Edge` pomocí `Visual Studio Code` a `C#`.

V tomto kurzu jsme provede procesem nastavení prostředí a jak toowrite [lit](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) dat převaděč modulu pomocí nejnovější hello `Azure IoT Edge NuGet` balíčky. 

>[!NOTE]
Tento kurz používá hello `.NET Core SDK`, který podporuje mezi různými platformami. Hello následující kurz je napsán pomocí hello `Windows 10` operačního systému. Některé příkazy hello v tomto kurzu se můžou lišit v závislosti na vaší `development environment`. 

## <a name="prerequisites"></a>Požadavky

V této části jsme nastavení prostředí pro `Azure IoT Edge` vývoj modulu. Platí tooboth **64bitová verze Windows** a **64-bit Linux (8 Ubuntu/Debian)** operační systémy.

je potřeba Hello následující software:

- [Git klienta](https://git-scm.com/downloads)
- [Sada .NET Core SDK](https://www.microsoft.com/net/core#windowscmd)
- [Visual Studio Code](https://code.visualstudio.com/)

Tato ukázka nepotřebujete tooclone hello úložiště, ale všechny hello ukázkový kód, které jsou popsané v tomto kurzu se nachází v hello následující úložiště:

- `git clone https://github.com/Azure-Samples/iot-edge-samples.git`.
- `cd iot-edge-samples/dotnetcore/simulated_ble`

## <a name="getting-started"></a>Začínáme

1. Nainstalujte `.NET Core SDK`.
2. Nainstalujte `Visual Studio Code` a hello `C# extension` z hello Visual Studio Code Marketplace.

Zobrazit [rychlý kurz video](https://channel9.msdn.com/Blogs/dotnet/Get-started-VSCode-Csharp-NET-Core-Windows) o tom, jak začít tooget pomocí `Visual Studio Code` a hello `.NET Core SDK`.

## <a name="creating-hello-azure-iot-edge-converter-module"></a>Vytváření modulu převaděč Azure IoT Edge hello

1. Inicializace novou `.NET Core` projektu knihovny jazyka C# – třída:
    - Otevřete příkazový řádek (`Windows + R` -> `cmd` -> `enter`).
    - Přejděte toohello složku, kam chcete toocreate hello `C#` projektu.
    - Typ **dotnet nové classlib -o IoTEdgeConverterModule -f netstandard1.3**. 
    - Tento příkaz vytvoří prázdný třídu s názvem `Class1.cs` ve vašem adresáři projektů.
2. Přejděte toohello složku, kde jsme právě vytvořili projektu knihovny tříd hello zadáním **cd IoTEdgeConverterModule**.
3. Hello otevřete projekt v `Visual Studio Code` zadáním **kód.**.
4. Po otevření projektu hello v `Visual Studio Code`, klikněte na hello **IoTEdgeConverterModule.csproj** tooopen hello souboru, jak ukazuje následující obrázek hello:

    ![Visual Studio Code okno úprav.](media/iot-hub-iot-edge-create-module/vscode-edit-csproj.png)

5. Vložit hello `XML` blob uvedené v následující fragment kódu mezi hello uzavírací hello `PropertyGroup` značky a hello zavřením `Project` značky; řádek šest v hello předcházející bitové kopie a uložte soubor hello stisknutím `Ctrl`  +  `S`.

   ```xml
     <ItemGroup>
       <PackageReference Include="Microsoft.Azure.Devices.Gateway.Module.NetStandard" Version="1.0.5" />
       <PackageReference Include="System.Threading.Thread" Version="4.3.0" />
       <PackageReference Include="Newtonsoft.Json" Version="10.0.2" />
     </ItemGroup> 
   ```

6. Po uložení hello `.csproj` souboru `Visual Studio Code` by se zobrazit výzva s `unresolved dependencies` dialogové okno, jak je vidět v hello následující bitové kopie: 

    ![Visual Studio Code obnovení závislosti dialogové okno](media/iot-hub-iot-edge-create-module/vscode-restore.png)

    a) klikněte na tlačítko `Restore` toorestore všechny hello odkazuje v projektech hello `.csproj` souboru včetně hello `PackageReferences` jsme přidali. 

    b) `Visual Studio Code` automaticky vytvoří hello `project.assets.json` soubor ve vašich projektů `obj` složky. Tento soubor obsahuje informace o projektu na závislosti toomake následné obnovení rychlejší.
 
    >[!NOTE]
    `.NET Core Tools`jsou nyní využívající MSBuild. To znamená `.csproj` místo k vytvoření souboru projektu `project.json`.

    - Pokud `Visual Studio Code` nezobrazuje výzvy k zadání, který je v pořádku, jsme můžete provést ručně. Otevřete hello `Visual Studio Code` integrované okno terminálu pomocí klávesy hello `Ctrl`  +  `backtick` klíče nebo pomocí nabídky hello `View`  ->  `Integrated Terminal`.
    - V hello `Integrated Terminal` okno – typ **dotnet obnovení**.
    
7. Přejmenujte hello `Class1.cs` souboru příliš`BleConverterModule.cs`. 

    a) soubor hello toorename nejprve klikněte na soubor hello stiskněte klávesu hello `F2` klíč.
    
    b) zadejte nový název hello **BleConverterModule**, jak je vidět v hello následující bitové kopie:

    ![Visual Studio Code přejmenování třídy](media/iot-hub-iot-edge-create-module/vscode-rename.png)

8. Nahradit existující kód hello v hello `BleConverterModule.cs` souboru tak, že kopírování a vkládání hello následující fragment kódu do vaší `BleConverterModule.cs` souboru.

   ```csharp
   using System;
   using System.Collections.Generic;
   using System.Globalization;
   using System.Linq;
   using System.Text;
   using System.Threading;
   using System.Threading.Tasks;
   using Microsoft.Azure.Devices.Gateway;
   using Newtonsoft.Json;
   
   namespace IoTEdgeConverterModule
   {
       public class BleConverterModule : IGatewayModule, IGatewayModuleStart
       {
           private Broker broker;
           private string configuration;
           private int messageCount;

           public void Create(Broker broker, byte[] configuration)
           {
               this.broker = broker;
               this.configuration = System.Text.Encoding.UTF8.GetString(configuration);
           }
   
           public void Start()
           {
           }

           public void Destroy()
           {
           }

           public void Receive(Message received_message)
           {
               string recMsg = Encoding.UTF8.GetString(received_message.Content, 0, received_message.Content.Length);
               BleData receivedData = JsonConvert.DeserializeObject<BleData>(recMsg);

               float temperature = float.Parse(receivedData.Temperature, CultureInfo.InvariantCulture.NumberFormat); 
               Dictionary<string, string> receivedProperties = received_message.Properties;
            
               Dictionary<string, string> properties = new Dictionary<string, string>();
               properties.Add("source", receivedProperties["source"]);
               properties.Add("macAddress", receivedProperties["macAddress"]);
               properties.Add("temperatureAlert", temperature > 30 ? "true" : "false");
   
               String content = String.Format("{0} \"deviceId\": \"Intel NUC Gateway\", \"messageId\": {1}, \"temperature\": {2} {3}", "{", ++this.messageCount, temperature, "}");
               Message messageToPublish = new Message(content, properties);
   
               this.broker.Publish(messageToPublish);
           }
       }
   }
   ```

9. Uložte soubor hello stisknutím `Ctrl`  +  `S`.

10. Vytvořte nový soubor s názvem `Untitled-1` pomocí klávesy hello `Ctrl`  +  `N` klíče, jak je vidět v hello následující bitové kopie:

    ![Visual Studio Code nový soubor](media/iot-hub-iot-edge-create-module/vscode-new-file.png)

11. toodeserialize hello `JSON` objekt, který jsme obdrželi od hello simulated `BLE` zařízení, kopie hello následující kód do hello `Untitled-1` okno editoru kódu souboru. 

   ```csharp
   using System;
   using Newtonsoft.Json;

   namespace IoTEdgeConverterModule
   {
       public class BleData
       {
           [JsonProperty(PropertyName = "temperature")]
           public string Temperature { get; set; }
       }
   }
   ```

12. Uložte soubor hello jako `BleData.cs` stisknutím `Ctrl`  +  `Shift`  +  `S` klíče.
    - Na hello uložit jako dialogové okno, v hello `Save as Type` rozevírací nabídce vyberte `C# (*.cs;*.csx)` jak je vidět v hello následující bitové kopie:

    ![Visual Studio Code uložit jako dialogové okno](media/iot-hub-iot-edge-create-module/vscode-save-as.png)

13. Vytvořte nový soubor s názvem `Untitled-1` pomocí klávesy hello `Ctrl`  +  `N` klíče.

14. Zkopírujte a vložte následující fragment kódu do hello hello `Untitled-1` souboru. Tato třída je `Azure IoT Edge` modul, který používáme toooutput hello data přijatá z našich `BleConverterModule`.

   ```csharp
   using System;
   using System.Collections.Generic;
   using System.Linq;
   using System.Text;
   using System.Threading.Tasks;
   using Microsoft.Azure.Devices.Gateway;
   using Newtonsoft.Json;
   
   namespace PrinterModule
   {
       public class DotNetPrinterModule : IGatewayModule
       {
           private string configuration;
           public void Create(Broker broker, byte[] configuration)
           {
               this.configuration = System.Text.Encoding.UTF8.GetString(configuration);
           }
   
           public void Destroy()
           {
           }
   
           public void Receive(Message received_message)
           {
               string recMsg = System.Text.Encoding.UTF8.GetString(received_message.Content, 0, received_message.Content.Length);
               Dictionary<string, string> receivedProperties = received_message.Properties;
               
               BleConverterData receivedData = JsonConvert.DeserializeObject<BleConverterData>(recMsg);
   
               Console.WriteLine();
               Console.WriteLine("Module           : [DotNetPrinterModule]");
               Console.WriteLine("received_message : {0}", recMsg);
   
               if(received_message.Properties["source"] == "bleTelemetry")
               {
                   Console.WriteLine("Source           : {0}", receivedProperties["source"]);
                   Console.WriteLine("MAC address      : {0}", receivedProperties["macAddress"]);
                   Console.WriteLine("Temperature Alert: {0}", receivedProperties["temperatureAlert"]);
                   Console.WriteLine("Deserialized Obj : [BleConverterData]");
                   Console.WriteLine("  DeviceId       : {0}", receivedData.DeviceId);
                   Console.WriteLine("  MessageId      : {0}", receivedData.MessageId);
                   Console.WriteLine("  Temperature    : {0}", receivedData.Temperature);
               }
   
               Console.WriteLine();
           }
       }
   }
   ```

15. Uložte soubor hello jako `DotNetPrinterModule.cs` stisknutím `Ctrl`  +  `Shift`  +  `S`.
    - Na hello uložit jako dialogové okno, v hello `Save as Type` rozevírací nabídce vyberte `C# (*.cs;*.csx)`.

16. Vytvořte nový soubor s názvem `Untitled-1` pomocí klávesy hello `Ctrl`  +  `N` klíče.

17. toodeserialize hello `JSON` objekt, který jsme obdrželi od hello `BleConverterModule`, kopírování a vložení hello následující fragment kódu do hello `Untitled-1` souboru. 

   ```csharp
   using System;
   using Newtonsoft.Json;

   namespace PrinterModule
   {
       public class BleConverterData
       {
           [JsonProperty(PropertyName = "deviceId")]
           public string DeviceId { get; set; }
   
           [JsonProperty(PropertyName = "messageId")]
           public string MessageId { get; set; }
   
           [JsonProperty(PropertyName = "temperature")]
           public string Temperature { get; set; }
       }
   }
   ```

18. Uložte soubor hello jako `BleConverterData.cs` stisknutím `Ctrl`  +  `Shift`  +  `S`.
    - Na hello uložit jako dialogové okno, v hello `Save as Type` rozevírací nabídce vyberte `C# (*.cs;*.csx)`.

19. Vytvořte nový soubor s názvem `Untitled-1` pomocí klávesy hello `Ctrl`  +  `N` klíče.

20. Zkopírujte a vložte následující fragment kódu do hello hello `Untitled-1` souboru.

   ```json
   {
       "loaders": [
           {
               "type": "dotnetcore",
               "name": "dotnetcore",
               "configuration": {
                   "binding.path": "dotnetcore.dll",
                   "binding.coreclrpath": "C:\\Program Files\\dotnet\\shared\\Microsoft.NETCore.App\\1.1.1\\coreclr.dll",
                   "binding.trustedplatformassemblieslocation": "C:\\Program Files\\dotnet\\shared\\Microsoft.NETCore.App\\1.1.1\\"
               }
           }
       ],
          "modules": [
           {
               "name": "simulated_device",
               "loader": {
                   "name": "native",
                   "entrypoint": {
                       "module.path": "simulated_device.dll"
                   }
               },
               "args": {
                   "macAddress": "01:02:03:03:02:01",
                   "messagePeriod": 500
               }
           },
           {
               "name": "ble_converter_module",
               "loader": {
                   "name": "dotnetcore",
                   "entrypoint": {
                       "assembly.name": "IoTEdgeConverterModule",
                       "entry.type": "IoTEdgeConverterModule.BleConverterModule"
                   }
               },
               "args": ""
           },
           {
               "name": "printer_module",
               "loader": {
                   "name": "dotnetcore",
                   "entrypoint": {
                       "assembly.name": "IoTEdgeConverterModule",
                       "entry.type": "PrinterModule.DotNetPrinterModule"
                   }
               },
               "args": ""
           }
       ],
       "links": [
           {
               "source": "simulated_device", "sink": "ble_converter_module"
           },
           {
               "source": "ble_converter_module", "sink": "printer_module"
           }
   ]
   }
   ```

21. Uložte soubor hello jako `gw-config.json` stisknutím `Ctrl`  +  `Shift`  +  `S`.
    - Na hello uložit jako dialogové okno, v hello `Save as Type` rozevírací nabídce vyberte `JSON (*.json;*.bowerrc;*.jshintrc;*.jscsrc;*.eslintrc;*.babelrc;*webmanifest)`.

22. tooenable kopírování hello konfigurační soubor toohello výstupní adresář, aktualizace hello `IoTEdgeConverterModule.csproj` s hello následující XML blob:

   ```xml
     <ItemGroup>
       <None Update="gw-config.json" CopyToOutputDirectory="PreserveNewest" />
     </ItemGroup>
   ```
    
   - Hello aktualizovat `IoTEdgeConverterModule.csproj` by měl vypadají hello následující bitové kopie:

    ![Visual Studio Code aktualizovat souboru .csproj](media/iot-hub-iot-edge-create-module/vscode-update-csproj.png)

23. Vytvořte nový soubor s názvem `Untitled-1` pomocí klávesy hello `Ctrl`  +  `N` klíče.

24. Zkopírujte a vložte následující fragment kódu do hello hello `Untitled-1` souboru.

   ```powershell
   Copy-Item -Path $env:userprofile\.nuget\packages\microsoft.azure.devices.gateway.native.windows.x64\1.1.3\runtimes\win-x64\native\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\system.runtime.serialization.formatters\4.3.0\lib\netstandard1.4\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\system.runtime.serialization.primitives\4.3.0\lib\netstandard1.3\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\newtonsoft.json\10.0.2\lib\netstandard1.3\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\system.componentmodel.typeconverter\4.3.0\lib\netstandard1.5\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\system.collections.nongeneric\4.3.0\lib\netstandard1.3\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\system.collections.specialized\4.3.0\lib\netstandard1.3\* -Destination .\bin\Debug\netstandard1.3
   ```

25. Uložte soubor hello jako `binplace.ps1` stisknutím `Ctrl`  +  `Shift`  +  `S`.
    - Na hello uložit jako dialogové okno, v hello `Save as Type` rozevírací nabídce vyberte `PowerShell (*.ps1;*.psm1;*.psd1;*.pssc;*.psrc)`.

26. Sestavení projektu hello pomocí klávesy hello `Ctrl`  +  `Shift`  +  `B` klíče. Když vytváříte projekt hello pro hello poprvé, `Visual Studio Code` vyzve vás s hello `No build task defined.` dialogové okno, jak je vidět v hello následující bitové kopie:

    ![Dialogové okno úloha sestavení Visual Studio Code](media/iot-hub-iot-edge-create-module/vscode-build-task.png)

    (a) klikněte na tlačítko hello `Configure Build Task` tlačítko.

    b) v hello `Select a Task Runner` dialogové okno rozevírací nabídce. Vyberte `.NET Core` jak je vidět v hello následující bitové kopie: 

    ![Visual Studio Code vyberte dialogové okno úloha](media/iot-hub-iot-edge-create-module/vscode-build-task-runner.png)

    hello c) kliknutím `.NET Core` položky vytvoří hello `tasks.json` ve vaší `.vscode` adresář a otevře se okno hello souboru v hello `code editor` okno. Neexistuje žádné toomodify nutné tento soubor zavřít hello karta.

27.  Otevřete hello `Visual Studio Code` integrované okno terminálu pomocí klávesy hello `Ctrl`  +  `backtick` klíče nebo pomocí nabídky hello `View`  ->  `Integrated Terminal` a typ **.\binplace.ps1**do hello `PowerShell` příkazového řádku. Tento příkaz zkopíruje všechny naše závislosti toohello výstupního adresáře.

28. Přejděte toohello projekty výstupního adresáře v hello `Integrated Terminal` okno zadáním **cd.\bin\Debug\netstandard1.3**.

29. Spustit ukázkový projekt hello zadáním **. \gw.exe gw-config.json** do hello `Integrated Terminal` okno řádku. 
    - Pokud jste postupovali podle kroků hello v tomto kurzu úzce, zda nyní je spuštěna hello `Azure IoT Edge BLE Data Converter Module` ukázkový projekt, jak je vidět v hello následující bitové kopie:
    
        ![Příklad Simulovaná zařízení spuštěná v kódu Visual Studio](media/iot-hub-iot-edge-create-module/vscode-run.png)
    
    - Pokud chcete aplikace hello tooterminate, stiskněte klávesu hello `<Enter>` klíč.

>[!IMPORTANT]
Není doporučeno toouse `Ctrl`  +  `C` tooterminate hello `IoT Edge` brány aplikace (tedy **gw.exe**). Protože tato akce může způsobit hello proces tooterminate neobvyklým způsobem.

