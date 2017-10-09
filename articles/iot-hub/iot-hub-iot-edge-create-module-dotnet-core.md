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
# <a name="create-an-azure-iot-edge-module-with-cx23"></a><span data-ttu-id="93c6f-104">Vytvořte modul Azure IoT Edge s C & #x23;</span><span class="sxs-lookup"><span data-stu-id="93c6f-104">Create an Azure IoT Edge Module with C&#x23;</span></span>

<span data-ttu-id="93c6f-105">V tomto kurzu umožňující prezentovat jak toocreate a modul pro `Azure IoT Edge` pomocí `Visual Studio Code` a `C#`.</span><span class="sxs-lookup"><span data-stu-id="93c6f-105">This tutorial showcases how toocreate a module for `Azure IoT Edge` using `Visual Studio Code` and `C#`.</span></span>

<span data-ttu-id="93c6f-106">V tomto kurzu jsme provede procesem nastavení prostředí a jak toowrite [lit](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) dat převaděč modulu pomocí nejnovější hello `Azure IoT Edge NuGet` balíčky.</span><span class="sxs-lookup"><span data-stu-id="93c6f-106">In this tutorial, we walk through environment set-up and how toowrite a [BLE](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) data converter module using hello latest `Azure IoT Edge NuGet` packages.</span></span> 

>[!NOTE]
<span data-ttu-id="93c6f-107">Tento kurz používá hello `.NET Core SDK`, který podporuje mezi různými platformami.</span><span class="sxs-lookup"><span data-stu-id="93c6f-107">This tutorial is using hello `.NET Core SDK`, which supports cross-platform compatibility.</span></span> <span data-ttu-id="93c6f-108">Hello následující kurz je napsán pomocí hello `Windows 10` operačního systému.</span><span class="sxs-lookup"><span data-stu-id="93c6f-108">hello following tutorial is written using hello `Windows 10` operating system.</span></span> <span data-ttu-id="93c6f-109">Některé příkazy hello v tomto kurzu se můžou lišit v závislosti na vaší `development environment`.</span><span class="sxs-lookup"><span data-stu-id="93c6f-109">Some of hello commands in this tutorial may be different depending on your `development environment`.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="93c6f-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="93c6f-110">Prerequisites</span></span>

<span data-ttu-id="93c6f-111">V této části jsme nastavení prostředí pro `Azure IoT Edge` vývoj modulu.</span><span class="sxs-lookup"><span data-stu-id="93c6f-111">In this section, we set-up your environment for `Azure IoT Edge` module development.</span></span> <span data-ttu-id="93c6f-112">Platí tooboth **64bitová verze Windows** a **64-bit Linux (8 Ubuntu/Debian)** operační systémy.</span><span class="sxs-lookup"><span data-stu-id="93c6f-112">It applies tooboth **64-bit Windows** and **64-bit Linux (Ubuntu/Debian 8)** operating systems.</span></span>

<span data-ttu-id="93c6f-113">je potřeba Hello následující software:</span><span class="sxs-lookup"><span data-stu-id="93c6f-113">hello following software is required:</span></span>

- [<span data-ttu-id="93c6f-114">Git klienta</span><span class="sxs-lookup"><span data-stu-id="93c6f-114">Git Client</span></span>](https://git-scm.com/downloads)
- [<span data-ttu-id="93c6f-115">Sada .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="93c6f-115">.NET Core SDK</span></span>](https://www.microsoft.com/net/core#windowscmd)
- [<span data-ttu-id="93c6f-116">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="93c6f-116">Visual Studio Code</span></span>](https://code.visualstudio.com/)

<span data-ttu-id="93c6f-117">Tato ukázka nepotřebujete tooclone hello úložiště, ale všechny hello ukázkový kód, které jsou popsané v tomto kurzu se nachází v hello následující úložiště:</span><span class="sxs-lookup"><span data-stu-id="93c6f-117">You do not need tooclone hello repo for this sample, however all of hello sample code discussed in this tutorial is located in hello following repository:</span></span>

- <span data-ttu-id="93c6f-118">`git clone https://github.com/Azure-Samples/iot-edge-samples.git`.</span><span class="sxs-lookup"><span data-stu-id="93c6f-118">`git clone https://github.com/Azure-Samples/iot-edge-samples.git`.</span></span>
- `cd iot-edge-samples/dotnetcore/simulated_ble`

## <a name="getting-started"></a><span data-ttu-id="93c6f-119">Začínáme</span><span class="sxs-lookup"><span data-stu-id="93c6f-119">Getting started</span></span>

1. <span data-ttu-id="93c6f-120">Nainstalujte `.NET Core SDK`.</span><span class="sxs-lookup"><span data-stu-id="93c6f-120">Install `.NET Core SDK`.</span></span>
2. <span data-ttu-id="93c6f-121">Nainstalujte `Visual Studio Code` a hello `C# extension` z hello Visual Studio Code Marketplace.</span><span class="sxs-lookup"><span data-stu-id="93c6f-121">Install `Visual Studio Code` and hello `C# extension` from hello Visual Studio Code Marketplace.</span></span>

<span data-ttu-id="93c6f-122">Zobrazit [rychlý kurz video](https://channel9.msdn.com/Blogs/dotnet/Get-started-VSCode-Csharp-NET-Core-Windows) o tom, jak začít tooget pomocí `Visual Studio Code` a hello `.NET Core SDK`.</span><span class="sxs-lookup"><span data-stu-id="93c6f-122">View this [quick video tutorial](https://channel9.msdn.com/Blogs/dotnet/Get-started-VSCode-Csharp-NET-Core-Windows) about how tooget started using `Visual Studio Code` and hello `.NET Core SDK`.</span></span>

## <a name="creating-hello-azure-iot-edge-converter-module"></a><span data-ttu-id="93c6f-123">Vytváření modulu převaděč Azure IoT Edge hello</span><span class="sxs-lookup"><span data-stu-id="93c6f-123">Creating hello Azure IoT Edge converter module</span></span>

1. <span data-ttu-id="93c6f-124">Inicializace novou `.NET Core` projektu knihovny jazyka C# – třída:</span><span class="sxs-lookup"><span data-stu-id="93c6f-124">Initialize a new `.NET Core` class library C# project:</span></span>
    - <span data-ttu-id="93c6f-125">Otevřete příkazový řádek (`Windows + R` -> `cmd` -> `enter`).</span><span class="sxs-lookup"><span data-stu-id="93c6f-125">Open a command prompt (`Windows + R` -> `cmd` -> `enter`).</span></span>
    - <span data-ttu-id="93c6f-126">Přejděte toohello složku, kam chcete toocreate hello `C#` projektu.</span><span class="sxs-lookup"><span data-stu-id="93c6f-126">Navigate toohello folder where you'd like toocreate hello `C#` project.</span></span>
    - <span data-ttu-id="93c6f-127">Typ **dotnet nové classlib -o IoTEdgeConverterModule -f netstandard1.3**.</span><span class="sxs-lookup"><span data-stu-id="93c6f-127">Type **dotnet new classlib -o IoTEdgeConverterModule -f netstandard1.3**.</span></span> 
    - <span data-ttu-id="93c6f-128">Tento příkaz vytvoří prázdný třídu s názvem `Class1.cs` ve vašem adresáři projektů.</span><span class="sxs-lookup"><span data-stu-id="93c6f-128">This command creates an empty class called `Class1.cs` in your projects directory.</span></span>
2. <span data-ttu-id="93c6f-129">Přejděte toohello složku, kde jsme právě vytvořili projektu knihovny tříd hello zadáním **cd IoTEdgeConverterModule**.</span><span class="sxs-lookup"><span data-stu-id="93c6f-129">Navigate toohello folder where we just created hello class library project by typing **cd IoTEdgeConverterModule**.</span></span>
3. <span data-ttu-id="93c6f-130">Hello otevřete projekt v `Visual Studio Code` zadáním **kód.**.</span><span class="sxs-lookup"><span data-stu-id="93c6f-130">Open hello project in `Visual Studio Code` by typing **code .**.</span></span>
4. <span data-ttu-id="93c6f-131">Po otevření projektu hello v `Visual Studio Code`, klikněte na hello **IoTEdgeConverterModule.csproj** tooopen hello souboru, jak ukazuje následující obrázek hello:</span><span class="sxs-lookup"><span data-stu-id="93c6f-131">Once hello project is opened in `Visual Studio Code`, click on hello **IoTEdgeConverterModule.csproj** tooopen hello file as shown in hello following image:</span></span>

    ![Visual Studio Code okno úprav.](media/iot-hub-iot-edge-create-module/vscode-edit-csproj.png)

5. <span data-ttu-id="93c6f-133">Vložit hello `XML` blob uvedené v následující fragment kódu mezi hello uzavírací hello `PropertyGroup` značky a hello zavřením `Project` značky; řádek šest v hello předcházející bitové kopie a uložte soubor hello stisknutím `Ctrl`  +  `S`.</span><span class="sxs-lookup"><span data-stu-id="93c6f-133">Insert hello `XML` blob shown in hello following code snippet between hello closing `PropertyGroup` tag and hello closing `Project` tag; line six in hello preceding image and save hello file by pressing `Ctrl` + `S`.</span></span>

   ```xml
     <ItemGroup>
       <PackageReference Include="Microsoft.Azure.Devices.Gateway.Module.NetStandard" Version="1.0.5" />
       <PackageReference Include="System.Threading.Thread" Version="4.3.0" />
       <PackageReference Include="Newtonsoft.Json" Version="10.0.2" />
     </ItemGroup> 
   ```

6. <span data-ttu-id="93c6f-134">Po uložení hello `.csproj` souboru `Visual Studio Code` by se zobrazit výzva s `unresolved dependencies` dialogové okno, jak je vidět v hello následující bitové kopie:</span><span class="sxs-lookup"><span data-stu-id="93c6f-134">Once you save hello `.csproj` file, `Visual Studio Code` should prompt you with an `unresolved dependencies` dialog as seen in hello following image:</span></span> 

    ![Visual Studio Code obnovení závislosti dialogové okno](media/iot-hub-iot-edge-create-module/vscode-restore.png)

    <span data-ttu-id="93c6f-136">a) klikněte na tlačítko `Restore` toorestore všechny hello odkazuje v projektech hello `.csproj` souboru včetně hello `PackageReferences` jsme přidali.</span><span class="sxs-lookup"><span data-stu-id="93c6f-136">a) Click `Restore` toorestore all of hello references in hello projects `.csproj` file including hello `PackageReferences` we have added.</span></span> 

    <span data-ttu-id="93c6f-137">b) `Visual Studio Code` automaticky vytvoří hello `project.assets.json` soubor ve vašich projektů `obj` složky.</span><span class="sxs-lookup"><span data-stu-id="93c6f-137">b) `Visual Studio Code` automatically creates hello `project.assets.json` file in your projects `obj` folder.</span></span> <span data-ttu-id="93c6f-138">Tento soubor obsahuje informace o projektu na závislosti toomake následné obnovení rychlejší.</span><span class="sxs-lookup"><span data-stu-id="93c6f-138">This file contains information about your project's dependencies toomake subsequent restores quicker.</span></span>
 
    >[!NOTE]
    <span data-ttu-id="93c6f-139">`.NET Core Tools`jsou nyní využívající MSBuild.</span><span class="sxs-lookup"><span data-stu-id="93c6f-139">`.NET Core Tools` are now MSBuild-based.</span></span> <span data-ttu-id="93c6f-140">To znamená `.csproj` místo k vytvoření souboru projektu `project.json`.</span><span class="sxs-lookup"><span data-stu-id="93c6f-140">Which means a `.csproj` project file is created instead of a `project.json`.</span></span>

    - <span data-ttu-id="93c6f-141">Pokud `Visual Studio Code` nezobrazuje výzvy k zadání, který je v pořádku, jsme můžete provést ručně.</span><span class="sxs-lookup"><span data-stu-id="93c6f-141">If `Visual Studio Code` does not prompt you that is ok, we can do it manually.</span></span> <span data-ttu-id="93c6f-142">Otevřete hello `Visual Studio Code` integrované okno terminálu pomocí klávesy hello `Ctrl`  +  `backtick` klíče nebo pomocí nabídky hello `View`  ->  `Integrated Terminal`.</span><span class="sxs-lookup"><span data-stu-id="93c6f-142">Open hello `Visual Studio Code` integrated terminal window by pressing hello `Ctrl` + `backtick` keys or using hello menus `View` -> `Integrated Terminal`.</span></span>
    - <span data-ttu-id="93c6f-143">V hello `Integrated Terminal` okno – typ **dotnet obnovení**.</span><span class="sxs-lookup"><span data-stu-id="93c6f-143">In hello `Integrated Terminal` window type **dotnet restore**.</span></span>
    
7. <span data-ttu-id="93c6f-144">Přejmenujte hello `Class1.cs` souboru příliš`BleConverterModule.cs`.</span><span class="sxs-lookup"><span data-stu-id="93c6f-144">Rename hello `Class1.cs` file too`BleConverterModule.cs`.</span></span> 

    <span data-ttu-id="93c6f-145">a) soubor hello toorename nejprve klikněte na soubor hello stiskněte klávesu hello `F2` klíč.</span><span class="sxs-lookup"><span data-stu-id="93c6f-145">a) toorename hello file first click on hello file then press hello `F2` key.</span></span>
    
    <span data-ttu-id="93c6f-146">b) zadejte nový název hello **BleConverterModule**, jak je vidět v hello následující bitové kopie:</span><span class="sxs-lookup"><span data-stu-id="93c6f-146">b) Type in hello new name **BleConverterModule**, as seen in hello following image:</span></span>

    ![Visual Studio Code přejmenování třídy](media/iot-hub-iot-edge-create-module/vscode-rename.png)

8. <span data-ttu-id="93c6f-148">Nahradit existující kód hello v hello `BleConverterModule.cs` souboru tak, že kopírování a vkládání hello následující fragment kódu do vaší `BleConverterModule.cs` souboru.</span><span class="sxs-lookup"><span data-stu-id="93c6f-148">Replace hello existing code in hello `BleConverterModule.cs` file by copying and pasting hello following code snippet into your `BleConverterModule.cs` file.</span></span>

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

9. <span data-ttu-id="93c6f-149">Uložte soubor hello stisknutím `Ctrl`  +  `S`.</span><span class="sxs-lookup"><span data-stu-id="93c6f-149">Save hello file by pressing `Ctrl` + `S`.</span></span>

10. <span data-ttu-id="93c6f-150">Vytvořte nový soubor s názvem `Untitled-1` pomocí klávesy hello `Ctrl`  +  `N` klíče, jak je vidět v hello následující bitové kopie:</span><span class="sxs-lookup"><span data-stu-id="93c6f-150">Create a new file called `Untitled-1` by pressing hello `Ctrl` + `N` keys as seen in hello following image:</span></span>

    ![Visual Studio Code nový soubor](media/iot-hub-iot-edge-create-module/vscode-new-file.png)

11. <span data-ttu-id="93c6f-152">toodeserialize hello `JSON` objekt, který jsme obdrželi od hello simulated `BLE` zařízení, kopie hello následující kód do hello `Untitled-1` okno editoru kódu souboru.</span><span class="sxs-lookup"><span data-stu-id="93c6f-152">toodeserialize hello `JSON` object that we receive from hello simulated `BLE` device, copy hello following code into hello `Untitled-1` file code editor window.</span></span> 

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

12. <span data-ttu-id="93c6f-153">Uložte soubor hello jako `BleData.cs` stisknutím `Ctrl`  +  `Shift`  +  `S` klíče.</span><span class="sxs-lookup"><span data-stu-id="93c6f-153">Save hello file as `BleData.cs` by pressing `Ctrl` + `Shift` + `S` keys.</span></span>
    - <span data-ttu-id="93c6f-154">Na hello uložit jako dialogové okno, v hello `Save as Type` rozevírací nabídce vyberte `C# (*.cs;*.csx)` jak je vidět v hello následující bitové kopie:</span><span class="sxs-lookup"><span data-stu-id="93c6f-154">On hello save as dialog box, in hello `Save as Type` dropdown menu, select `C# (*.cs;*.csx)` as seen in hello following image:</span></span>

    ![Visual Studio Code uložit jako dialogové okno](media/iot-hub-iot-edge-create-module/vscode-save-as.png)

13. <span data-ttu-id="93c6f-156">Vytvořte nový soubor s názvem `Untitled-1` pomocí klávesy hello `Ctrl`  +  `N` klíče.</span><span class="sxs-lookup"><span data-stu-id="93c6f-156">Create a new file called `Untitled-1` by pressing hello `Ctrl` + `N` keys.</span></span>

14. <span data-ttu-id="93c6f-157">Zkopírujte a vložte následující fragment kódu do hello hello `Untitled-1` souboru.</span><span class="sxs-lookup"><span data-stu-id="93c6f-157">Copy and paste hello following code snippet into hello `Untitled-1` file.</span></span> <span data-ttu-id="93c6f-158">Tato třída je `Azure IoT Edge` modul, který používáme toooutput hello data přijatá z našich `BleConverterModule`.</span><span class="sxs-lookup"><span data-stu-id="93c6f-158">This class is a `Azure IoT Edge` module, which we use toooutput hello data received from our `BleConverterModule`.</span></span>

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

15. <span data-ttu-id="93c6f-159">Uložte soubor hello jako `DotNetPrinterModule.cs` stisknutím `Ctrl`  +  `Shift`  +  `S`.</span><span class="sxs-lookup"><span data-stu-id="93c6f-159">Save hello file as `DotNetPrinterModule.cs` by pressing `Ctrl` + `Shift` + `S`.</span></span>
    - <span data-ttu-id="93c6f-160">Na hello uložit jako dialogové okno, v hello `Save as Type` rozevírací nabídce vyberte `C# (*.cs;*.csx)`.</span><span class="sxs-lookup"><span data-stu-id="93c6f-160">On hello save as dialog box, in hello `Save as Type` dropdown menu, select `C# (*.cs;*.csx)`.</span></span>

16. <span data-ttu-id="93c6f-161">Vytvořte nový soubor s názvem `Untitled-1` pomocí klávesy hello `Ctrl`  +  `N` klíče.</span><span class="sxs-lookup"><span data-stu-id="93c6f-161">Create a new file called `Untitled-1` by pressing hello `Ctrl` + `N` keys.</span></span>

17. <span data-ttu-id="93c6f-162">toodeserialize hello `JSON` objekt, který jsme obdrželi od hello `BleConverterModule`, kopírování a vložení hello následující fragment kódu do hello `Untitled-1` souboru.</span><span class="sxs-lookup"><span data-stu-id="93c6f-162">toodeserialize hello `JSON` object that we receive from hello `BleConverterModule`, Copy and paste hello following code snippet into hello `Untitled-1` file.</span></span> 

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

18. <span data-ttu-id="93c6f-163">Uložte soubor hello jako `BleConverterData.cs` stisknutím `Ctrl`  +  `Shift`  +  `S`.</span><span class="sxs-lookup"><span data-stu-id="93c6f-163">Save hello file as `BleConverterData.cs` by pressing `Ctrl` + `Shift` + `S`.</span></span>
    - <span data-ttu-id="93c6f-164">Na hello uložit jako dialogové okno, v hello `Save as Type` rozevírací nabídce vyberte `C# (*.cs;*.csx)`.</span><span class="sxs-lookup"><span data-stu-id="93c6f-164">On hello save as dialog box, in hello `Save as Type` dropdown menu, select `C# (*.cs;*.csx)`.</span></span>

19. <span data-ttu-id="93c6f-165">Vytvořte nový soubor s názvem `Untitled-1` pomocí klávesy hello `Ctrl`  +  `N` klíče.</span><span class="sxs-lookup"><span data-stu-id="93c6f-165">Create a new file called `Untitled-1` by pressing hello `Ctrl` + `N` keys.</span></span>

20. <span data-ttu-id="93c6f-166">Zkopírujte a vložte následující fragment kódu do hello hello `Untitled-1` souboru.</span><span class="sxs-lookup"><span data-stu-id="93c6f-166">Copy and paste hello following code snippet into hello `Untitled-1` file.</span></span>

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

21. <span data-ttu-id="93c6f-167">Uložte soubor hello jako `gw-config.json` stisknutím `Ctrl`  +  `Shift`  +  `S`.</span><span class="sxs-lookup"><span data-stu-id="93c6f-167">Save hello file as `gw-config.json` by pressing `Ctrl` + `Shift` + `S`.</span></span>
    - <span data-ttu-id="93c6f-168">Na hello uložit jako dialogové okno, v hello `Save as Type` rozevírací nabídce vyberte `JSON (*.json;*.bowerrc;*.jshintrc;*.jscsrc;*.eslintrc;*.babelrc;*webmanifest)`.</span><span class="sxs-lookup"><span data-stu-id="93c6f-168">On hello save as dialog box, in hello `Save as Type` dropdown menu, select `JSON (*.json;*.bowerrc;*.jshintrc;*.jscsrc;*.eslintrc;*.babelrc;*webmanifest)`.</span></span>

22. <span data-ttu-id="93c6f-169">tooenable kopírování hello konfigurační soubor toohello výstupní adresář, aktualizace hello `IoTEdgeConverterModule.csproj` s hello následující XML blob:</span><span class="sxs-lookup"><span data-stu-id="93c6f-169">tooenable copying of hello configuration file toohello output directory, update hello `IoTEdgeConverterModule.csproj` with hello following XML blob:</span></span>

   ```xml
     <ItemGroup>
       <None Update="gw-config.json" CopyToOutputDirectory="PreserveNewest" />
     </ItemGroup>
   ```
    
   - <span data-ttu-id="93c6f-170">Hello aktualizovat `IoTEdgeConverterModule.csproj` by měl vypadají hello následující bitové kopie:</span><span class="sxs-lookup"><span data-stu-id="93c6f-170">hello updated `IoTEdgeConverterModule.csproj` should look like hello following image:</span></span>

    ![Visual Studio Code aktualizovat souboru .csproj](media/iot-hub-iot-edge-create-module/vscode-update-csproj.png)

23. <span data-ttu-id="93c6f-172">Vytvořte nový soubor s názvem `Untitled-1` pomocí klávesy hello `Ctrl`  +  `N` klíče.</span><span class="sxs-lookup"><span data-stu-id="93c6f-172">Create a new file called `Untitled-1` by pressing hello `Ctrl` + `N` keys.</span></span>

24. <span data-ttu-id="93c6f-173">Zkopírujte a vložte následující fragment kódu do hello hello `Untitled-1` souboru.</span><span class="sxs-lookup"><span data-stu-id="93c6f-173">Copy and paste hello following code snippet into hello `Untitled-1` file.</span></span>

   ```powershell
   Copy-Item -Path $env:userprofile\.nuget\packages\microsoft.azure.devices.gateway.native.windows.x64\1.1.3\runtimes\win-x64\native\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\system.runtime.serialization.formatters\4.3.0\lib\netstandard1.4\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\system.runtime.serialization.primitives\4.3.0\lib\netstandard1.3\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\newtonsoft.json\10.0.2\lib\netstandard1.3\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\system.componentmodel.typeconverter\4.3.0\lib\netstandard1.5\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\system.collections.nongeneric\4.3.0\lib\netstandard1.3\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\system.collections.specialized\4.3.0\lib\netstandard1.3\* -Destination .\bin\Debug\netstandard1.3
   ```

25. <span data-ttu-id="93c6f-174">Uložte soubor hello jako `binplace.ps1` stisknutím `Ctrl`  +  `Shift`  +  `S`.</span><span class="sxs-lookup"><span data-stu-id="93c6f-174">Save hello file as `binplace.ps1` by pressing `Ctrl` + `Shift` + `S`.</span></span>
    - <span data-ttu-id="93c6f-175">Na hello uložit jako dialogové okno, v hello `Save as Type` rozevírací nabídce vyberte `PowerShell (*.ps1;*.psm1;*.psd1;*.pssc;*.psrc)`.</span><span class="sxs-lookup"><span data-stu-id="93c6f-175">On hello save as dialog box, in hello `Save as Type` dropdown menu, select `PowerShell (*.ps1;*.psm1;*.psd1;*.pssc;*.psrc)`.</span></span>

26. <span data-ttu-id="93c6f-176">Sestavení projektu hello pomocí klávesy hello `Ctrl`  +  `Shift`  +  `B` klíče.</span><span class="sxs-lookup"><span data-stu-id="93c6f-176">Build hello project by pressing hello `Ctrl` + `Shift` + `B` keys.</span></span> <span data-ttu-id="93c6f-177">Když vytváříte projekt hello pro hello poprvé, `Visual Studio Code` vyzve vás s hello `No build task defined.` dialogové okno, jak je vidět v hello následující bitové kopie:</span><span class="sxs-lookup"><span data-stu-id="93c6f-177">When you build hello project for hello first time, `Visual Studio Code` prompts you with hello `No build task defined.` dialog as seen in hello following image:</span></span>

    ![Dialogové okno úloha sestavení Visual Studio Code](media/iot-hub-iot-edge-create-module/vscode-build-task.png)

    <span data-ttu-id="93c6f-179">(a) klikněte na tlačítko hello `Configure Build Task` tlačítko.</span><span class="sxs-lookup"><span data-stu-id="93c6f-179">a) Click hello `Configure Build Task` button.</span></span>

    <span data-ttu-id="93c6f-180">b) v hello `Select a Task Runner` dialogové okno rozevírací nabídce.</span><span class="sxs-lookup"><span data-stu-id="93c6f-180">b) In hello `Select a Task Runner` dialog dropdown menu.</span></span> <span data-ttu-id="93c6f-181">Vyberte `.NET Core` jak je vidět v hello následující bitové kopie:</span><span class="sxs-lookup"><span data-stu-id="93c6f-181">Select `.NET Core` as seen in hello following image:</span></span> 

    ![Visual Studio Code vyberte dialogové okno úloha](media/iot-hub-iot-edge-create-module/vscode-build-task-runner.png)

    <span data-ttu-id="93c6f-183">hello c) kliknutím `.NET Core` položky vytvoří hello `tasks.json` ve vaší `.vscode` adresář a otevře se okno hello souboru v hello `code editor` okno.</span><span class="sxs-lookup"><span data-stu-id="93c6f-183">c) Clicking hello `.NET Core` item creates hello `tasks.json` file in your `.vscode` directory and opens hello file in hello `code editor` window.</span></span> <span data-ttu-id="93c6f-184">Neexistuje žádné toomodify nutné tento soubor zavřít hello karta.</span><span class="sxs-lookup"><span data-stu-id="93c6f-184">There is no need toomodify this file, close hello tab.</span></span>

27.  <span data-ttu-id="93c6f-185">Otevřete hello `Visual Studio Code` integrované okno terminálu pomocí klávesy hello `Ctrl`  +  `backtick` klíče nebo pomocí nabídky hello `View`  ->  `Integrated Terminal` a typ **.\binplace.ps1**do hello `PowerShell` příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="93c6f-185">Open hello `Visual Studio Code` integrated terminal window by pressing hello `Ctrl` + `backtick` keys or using hello menus `View` -> `Integrated Terminal` and type **.\binplace.ps1** into hello `PowerShell` command prompt.</span></span> <span data-ttu-id="93c6f-186">Tento příkaz zkopíruje všechny naše závislosti toohello výstupního adresáře.</span><span class="sxs-lookup"><span data-stu-id="93c6f-186">This command copies all our dependencies toohello output directory.</span></span>

28. <span data-ttu-id="93c6f-187">Přejděte toohello projekty výstupního adresáře v hello `Integrated Terminal` okno zadáním **cd.\bin\Debug\netstandard1.3**.</span><span class="sxs-lookup"><span data-stu-id="93c6f-187">Navigate toohello projects output directory in hello `Integrated Terminal` window by typing **cd .\bin\Debug\netstandard1.3**.</span></span>

29. <span data-ttu-id="93c6f-188">Spustit ukázkový projekt hello zadáním **. \gw.exe gw-config.json** do hello `Integrated Terminal` okno řádku.</span><span class="sxs-lookup"><span data-stu-id="93c6f-188">Run hello sample project by typing **.\gw.exe gw-config.json** into hello `Integrated Terminal` window prompt.</span></span> 
    - <span data-ttu-id="93c6f-189">Pokud jste postupovali podle kroků hello v tomto kurzu úzce, zda nyní je spuštěna hello `Azure IoT Edge BLE Data Converter Module` ukázkový projekt, jak je vidět v hello následující bitové kopie:</span><span class="sxs-lookup"><span data-stu-id="93c6f-189">If you have followed hello steps in this tutorial closely, you should now be running hello `Azure IoT Edge BLE Data Converter Module` sample project as seen in hello following image:</span></span>
    
        ![Příklad Simulovaná zařízení spuštěná v kódu Visual Studio](media/iot-hub-iot-edge-create-module/vscode-run.png)
    
    - <span data-ttu-id="93c6f-191">Pokud chcete aplikace hello tooterminate, stiskněte klávesu hello `<Enter>` klíč.</span><span class="sxs-lookup"><span data-stu-id="93c6f-191">If you want tooterminate hello application, press hello `<Enter>` key.</span></span>

>[!IMPORTANT]
<span data-ttu-id="93c6f-192">Není doporučeno toouse `Ctrl`  +  `C` tooterminate hello `IoT Edge` brány aplikace (tedy **gw.exe**).</span><span class="sxs-lookup"><span data-stu-id="93c6f-192">It is not recommended toouse `Ctrl` + `C` tooterminate hello `IoT Edge` gateway application (that is, **gw.exe**).</span></span> <span data-ttu-id="93c6f-193">Protože tato akce může způsobit hello proces tooterminate neobvyklým způsobem.</span><span class="sxs-lookup"><span data-stu-id="93c6f-193">As this action may cause hello process tooterminate abnormally.</span></span>

