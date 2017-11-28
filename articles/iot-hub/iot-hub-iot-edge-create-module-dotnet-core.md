---
title: "Modul služby Azure IoT Edge vytvořit pomocí jazyka C# | Microsoft Docs"
description: "V tomto kurzu umožňující prezentovat jak napsat modulu převaděč dat zakázat pomocí nejnovější balíčky Azure IoT Edge NuGet, Visual Studio Code a C#."
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
ms.openlocfilehash: 7175ffc8de2c043593d61143b402484d33e4a8cc
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="create-an-azure-iot-edge-module-with-cx23"></a><span data-ttu-id="ffba8-104">Vytvořte modul Azure IoT Edge s C & #x23;</span><span class="sxs-lookup"><span data-stu-id="ffba8-104">Create an Azure IoT Edge Module with C&#x23;</span></span>

<span data-ttu-id="ffba8-105">V tomto kurzu umožňující prezentovat vytvoření modulu pro `Azure IoT Edge` pomocí `Visual Studio Code` a `C#`.</span><span class="sxs-lookup"><span data-stu-id="ffba8-105">This tutorial showcases how to create a module for `Azure IoT Edge` using `Visual Studio Code` and `C#`.</span></span>

<span data-ttu-id="ffba8-106">V tomto kurzu jsme provede procesem nastavení prostředí a jak napsat [lit](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) modulu převaděč data pomocí poslední `Azure IoT Edge NuGet` balíčky.</span><span class="sxs-lookup"><span data-stu-id="ffba8-106">In this tutorial, we walk through environment set-up and how to write a [BLE](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) data converter module using the latest `Azure IoT Edge NuGet` packages.</span></span> 

>[!NOTE]
<span data-ttu-id="ffba8-107">Tento kurz používá `.NET Core SDK`, který podporuje mezi různými platformami.</span><span class="sxs-lookup"><span data-stu-id="ffba8-107">This tutorial is using the `.NET Core SDK`, which supports cross-platform compatibility.</span></span> <span data-ttu-id="ffba8-108">Následující kurz je napsán pomocí `Windows 10` operačního systému.</span><span class="sxs-lookup"><span data-stu-id="ffba8-108">The following tutorial is written using the `Windows 10` operating system.</span></span> <span data-ttu-id="ffba8-109">Některé příkazy v tomto kurzu se můžou lišit v závislosti na vaší `development environment`.</span><span class="sxs-lookup"><span data-stu-id="ffba8-109">Some of the commands in this tutorial may be different depending on your `development environment`.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="ffba8-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ffba8-110">Prerequisites</span></span>

<span data-ttu-id="ffba8-111">V této části jsme nastavení prostředí pro `Azure IoT Edge` vývoj modulu.</span><span class="sxs-lookup"><span data-stu-id="ffba8-111">In this section, we set-up your environment for `Azure IoT Edge` module development.</span></span> <span data-ttu-id="ffba8-112">Platí pro obě **64bitová verze Windows** a **64-bit Linux (8 Ubuntu/Debian)** operační systémy.</span><span class="sxs-lookup"><span data-stu-id="ffba8-112">It applies to both **64-bit Windows** and **64-bit Linux (Ubuntu/Debian 8)** operating systems.</span></span>

<span data-ttu-id="ffba8-113">Je vyžadován následující software:</span><span class="sxs-lookup"><span data-stu-id="ffba8-113">The following software is required:</span></span>

- [<span data-ttu-id="ffba8-114">Git klienta</span><span class="sxs-lookup"><span data-stu-id="ffba8-114">Git Client</span></span>](https://git-scm.com/downloads)
- [<span data-ttu-id="ffba8-115">Sada .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="ffba8-115">.NET Core SDK</span></span>](https://www.microsoft.com/net/core#windowscmd)
- [<span data-ttu-id="ffba8-116">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ffba8-116">Visual Studio Code</span></span>](https://code.visualstudio.com/)

<span data-ttu-id="ffba8-117">Není potřeba naklonujte úložiště pro tuto ukázku, ale všechny ukázkový kód popsané v tomto kurzu se nachází v úložišti následující:</span><span class="sxs-lookup"><span data-stu-id="ffba8-117">You do not need to clone the repo for this sample, however all of the sample code discussed in this tutorial is located in the following repository:</span></span>

- <span data-ttu-id="ffba8-118">`git clone https://github.com/Azure-Samples/iot-edge-samples.git`.</span><span class="sxs-lookup"><span data-stu-id="ffba8-118">`git clone https://github.com/Azure-Samples/iot-edge-samples.git`.</span></span>
- `cd iot-edge-samples/dotnetcore/simulated_ble`

## <a name="getting-started"></a><span data-ttu-id="ffba8-119">Začínáme</span><span class="sxs-lookup"><span data-stu-id="ffba8-119">Getting started</span></span>

1. <span data-ttu-id="ffba8-120">Nainstalujte `.NET Core SDK`.</span><span class="sxs-lookup"><span data-stu-id="ffba8-120">Install `.NET Core SDK`.</span></span>
2. <span data-ttu-id="ffba8-121">Nainstalujte `Visual Studio Code` a `C# extension` v sadě Visual Studio Marketplace kódu.</span><span class="sxs-lookup"><span data-stu-id="ffba8-121">Install `Visual Studio Code` and the `C# extension` from the Visual Studio Code Marketplace.</span></span>

<span data-ttu-id="ffba8-122">Zobrazit [rychlý kurz video](https://channel9.msdn.com/Blogs/dotnet/Get-started-VSCode-Csharp-NET-Core-Windows) o tom, jak začít používat `Visual Studio Code` a `.NET Core SDK`.</span><span class="sxs-lookup"><span data-stu-id="ffba8-122">View this [quick video tutorial](https://channel9.msdn.com/Blogs/dotnet/Get-started-VSCode-Csharp-NET-Core-Windows) about how to get started using `Visual Studio Code` and the `.NET Core SDK`.</span></span>

## <a name="creating-the-azure-iot-edge-converter-module"></a><span data-ttu-id="ffba8-123">Vytváření modulu převaděč Azure IoT Edge</span><span class="sxs-lookup"><span data-stu-id="ffba8-123">Creating the Azure IoT Edge converter module</span></span>

1. <span data-ttu-id="ffba8-124">Inicializace novou `.NET Core` projektu knihovny jazyka C# – třída:</span><span class="sxs-lookup"><span data-stu-id="ffba8-124">Initialize a new `.NET Core` class library C# project:</span></span>
    - <span data-ttu-id="ffba8-125">Otevřete příkazový řádek (`Windows + R` -> `cmd` -> `enter`).</span><span class="sxs-lookup"><span data-stu-id="ffba8-125">Open a command prompt (`Windows + R` -> `cmd` -> `enter`).</span></span>
    - <span data-ttu-id="ffba8-126">Přejděte do složky, kde chcete vytvořit `C#` projektu.</span><span class="sxs-lookup"><span data-stu-id="ffba8-126">Navigate to the folder where you'd like to create the `C#` project.</span></span>
    - <span data-ttu-id="ffba8-127">Typ **dotnet nové classlib -o IoTEdgeConverterModule -f netstandard1.3**.</span><span class="sxs-lookup"><span data-stu-id="ffba8-127">Type **dotnet new classlib -o IoTEdgeConverterModule -f netstandard1.3**.</span></span> 
    - <span data-ttu-id="ffba8-128">Tento příkaz vytvoří prázdný třídu s názvem `Class1.cs` ve vašem adresáři projektů.</span><span class="sxs-lookup"><span data-stu-id="ffba8-128">This command creates an empty class called `Class1.cs` in your projects directory.</span></span>
2. <span data-ttu-id="ffba8-129">Přejděte do složky, kde právě vytvořený projektu knihovny tříd zadáním **cd IoTEdgeConverterModule**.</span><span class="sxs-lookup"><span data-stu-id="ffba8-129">Navigate to the folder where we just created the class library project by typing **cd IoTEdgeConverterModule**.</span></span>
3. <span data-ttu-id="ffba8-130">Otevřete projekt ve `Visual Studio Code` zadáním **kód.**.</span><span class="sxs-lookup"><span data-stu-id="ffba8-130">Open the project in `Visual Studio Code` by typing **code .**.</span></span>
4. <span data-ttu-id="ffba8-131">Po otevření projektu v `Visual Studio Code`, klikněte na **IoTEdgeConverterModule.csproj** k otevření souboru, jak je znázorněno na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="ffba8-131">Once the project is opened in `Visual Studio Code`, click on the **IoTEdgeConverterModule.csproj** to open the file as shown in the following image:</span></span>

    ![Visual Studio Code okno úprav.](media/iot-hub-iot-edge-create-module/vscode-edit-csproj.png)

5. <span data-ttu-id="ffba8-133">Vložit `XML` blob uvedené v následující fragment kódu mezi ukončovací `PropertyGroup` značky a zavření `Project` značky; řádek šest na předchozím obrázku a uložte soubor stisknutím `Ctrl`  +  `S`.</span><span class="sxs-lookup"><span data-stu-id="ffba8-133">Insert the `XML` blob shown in the following code snippet between the closing `PropertyGroup` tag and the closing `Project` tag; line six in the preceding image and save the file by pressing `Ctrl` + `S`.</span></span>

   ```xml
     <ItemGroup>
       <PackageReference Include="Microsoft.Azure.Devices.Gateway.Module.NetStandard" Version="1.0.5" />
       <PackageReference Include="System.Threading.Thread" Version="4.3.0" />
       <PackageReference Include="Newtonsoft.Json" Version="10.0.2" />
     </ItemGroup> 
   ```

6. <span data-ttu-id="ffba8-134">Po uložení `.csproj` souboru `Visual Studio Code` by se zobrazit výzva s `unresolved dependencies` dialogové okno, jak je vidět na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="ffba8-134">Once you save the `.csproj` file, `Visual Studio Code` should prompt you with an `unresolved dependencies` dialog as seen in the following image:</span></span> 

    ![Visual Studio Code obnovení závislosti dialogové okno](media/iot-hub-iot-edge-create-module/vscode-restore.png)

    <span data-ttu-id="ffba8-136">(a) klikněte na tlačítko `Restore` chcete obnovit všechny odkazy v projektech `.csproj` včetně souborů `PackageReferences` jsme přidali.</span><span class="sxs-lookup"><span data-stu-id="ffba8-136">a) Click `Restore` to restore all of the references in the projects `.csproj` file including the `PackageReferences` we have added.</span></span> 

    <span data-ttu-id="ffba8-137">b) `Visual Studio Code` automaticky vytvoří `project.assets.json` soubor ve vašich projektů `obj` složky.</span><span class="sxs-lookup"><span data-stu-id="ffba8-137">b) `Visual Studio Code` automatically creates the `project.assets.json` file in your projects `obj` folder.</span></span> <span data-ttu-id="ffba8-138">Tento soubor obsahuje informace o závislostech vašeho projektu aby následné obnovení rychlejší.</span><span class="sxs-lookup"><span data-stu-id="ffba8-138">This file contains information about your project's dependencies to make subsequent restores quicker.</span></span>
 
    >[!NOTE]
    <span data-ttu-id="ffba8-139">`.NET Core Tools`jsou nyní využívající MSBuild.</span><span class="sxs-lookup"><span data-stu-id="ffba8-139">`.NET Core Tools` are now MSBuild-based.</span></span> <span data-ttu-id="ffba8-140">To znamená `.csproj` místo k vytvoření souboru projektu `project.json`.</span><span class="sxs-lookup"><span data-stu-id="ffba8-140">Which means a `.csproj` project file is created instead of a `project.json`.</span></span>

    - <span data-ttu-id="ffba8-141">Pokud `Visual Studio Code` nezobrazuje výzvy k zadání, který je v pořádku, jsme můžete provést ručně.</span><span class="sxs-lookup"><span data-stu-id="ffba8-141">If `Visual Studio Code` does not prompt you that is ok, we can do it manually.</span></span> <span data-ttu-id="ffba8-142">Otevřete `Visual Studio Code` integrované okno terminálu stisknutím `Ctrl`  +  `backtick` klíčů nebo používat nabídky `View`  ->  `Integrated Terminal`.</span><span class="sxs-lookup"><span data-stu-id="ffba8-142">Open the `Visual Studio Code` integrated terminal window by pressing the `Ctrl` + `backtick` keys or using the menus `View` -> `Integrated Terminal`.</span></span>
    - <span data-ttu-id="ffba8-143">V `Integrated Terminal` okno – typ **dotnet obnovení**.</span><span class="sxs-lookup"><span data-stu-id="ffba8-143">In the `Integrated Terminal` window type **dotnet restore**.</span></span>
    
7. <span data-ttu-id="ffba8-144">Přejmenujte `Class1.cs` do souboru `BleConverterModule.cs`.</span><span class="sxs-lookup"><span data-stu-id="ffba8-144">Rename the `Class1.cs` file to `BleConverterModule.cs`.</span></span> 

    <span data-ttu-id="ffba8-145">(a) Chcete-li přejmenovat soubor nejdřív kliknutím na soubor stiskněte `F2` klíč.</span><span class="sxs-lookup"><span data-stu-id="ffba8-145">a) To rename the file first click on the file then press the `F2` key.</span></span>
    
    <span data-ttu-id="ffba8-146">b) zadejte nový název **BleConverterModule**, jak je vidět na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="ffba8-146">b) Type in the new name **BleConverterModule**, as seen in the following image:</span></span>

    ![Visual Studio Code přejmenování třídy](media/iot-hub-iot-edge-create-module/vscode-rename.png)

8. <span data-ttu-id="ffba8-148">Nahraďte stávající kód v `BleConverterModule.cs` souboru zkopírováním a vložením následující fragment kódu do vaší `BleConverterModule.cs` souboru.</span><span class="sxs-lookup"><span data-stu-id="ffba8-148">Replace the existing code in the `BleConverterModule.cs` file by copying and pasting the following code snippet into your `BleConverterModule.cs` file.</span></span>

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

9. <span data-ttu-id="ffba8-149">Uložte soubor stisknutím `Ctrl`  +  `S`.</span><span class="sxs-lookup"><span data-stu-id="ffba8-149">Save the file by pressing `Ctrl` + `S`.</span></span>

10. <span data-ttu-id="ffba8-150">Vytvořte nový soubor s názvem `Untitled-1` stisknutím `Ctrl`  +  `N` klíče, jak je vidět na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="ffba8-150">Create a new file called `Untitled-1` by pressing the `Ctrl` + `N` keys as seen in the following image:</span></span>

    ![Visual Studio Code nový soubor](media/iot-hub-iot-edge-create-module/vscode-new-file.png)

11. <span data-ttu-id="ffba8-152">K deserializaci `JSON` objekt, který jsme obdrželi od simulované `BLE` zařízení, zkopírujte následující kód do `Untitled-1` okno editoru kódu souboru.</span><span class="sxs-lookup"><span data-stu-id="ffba8-152">To deserialize the `JSON` object that we receive from the simulated `BLE` device, copy the following code into the `Untitled-1` file code editor window.</span></span> 

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

12. <span data-ttu-id="ffba8-153">Uložte soubor jako `BleData.cs` stisknutím `Ctrl`  +  `Shift`  +  `S` klíče.</span><span class="sxs-lookup"><span data-stu-id="ffba8-153">Save the file as `BleData.cs` by pressing `Ctrl` + `Shift` + `S` keys.</span></span>
    - <span data-ttu-id="ffba8-154">Na Uložit jako dialogové okno v `Save as Type` rozevírací nabídce vyberte `C# (*.cs;*.csx)` jak je vidět na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="ffba8-154">On the save as dialog box, in the `Save as Type` dropdown menu, select `C# (*.cs;*.csx)` as seen in the following image:</span></span>

    ![Visual Studio Code uložit jako dialogové okno](media/iot-hub-iot-edge-create-module/vscode-save-as.png)

13. <span data-ttu-id="ffba8-156">Vytvořte nový soubor s názvem `Untitled-1` stisknutím `Ctrl`  +  `N` klíče.</span><span class="sxs-lookup"><span data-stu-id="ffba8-156">Create a new file called `Untitled-1` by pressing the `Ctrl` + `N` keys.</span></span>

14. <span data-ttu-id="ffba8-157">Zkopírujte a vložte následující fragment kódu do `Untitled-1` souboru.</span><span class="sxs-lookup"><span data-stu-id="ffba8-157">Copy and paste the following code snippet into the `Untitled-1` file.</span></span> <span data-ttu-id="ffba8-158">Tato třída je `Azure IoT Edge` modul, který používáme k vypsání data přijatá z našich `BleConverterModule`.</span><span class="sxs-lookup"><span data-stu-id="ffba8-158">This class is a `Azure IoT Edge` module, which we use to output the data received from our `BleConverterModule`.</span></span>

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

15. <span data-ttu-id="ffba8-159">Uložte soubor jako `DotNetPrinterModule.cs` stisknutím `Ctrl`  +  `Shift`  +  `S`.</span><span class="sxs-lookup"><span data-stu-id="ffba8-159">Save the file as `DotNetPrinterModule.cs` by pressing `Ctrl` + `Shift` + `S`.</span></span>
    - <span data-ttu-id="ffba8-160">Na Uložit jako dialogové okno v `Save as Type` rozevírací nabídce vyberte `C# (*.cs;*.csx)`.</span><span class="sxs-lookup"><span data-stu-id="ffba8-160">On the save as dialog box, in the `Save as Type` dropdown menu, select `C# (*.cs;*.csx)`.</span></span>

16. <span data-ttu-id="ffba8-161">Vytvořte nový soubor s názvem `Untitled-1` stisknutím `Ctrl`  +  `N` klíče.</span><span class="sxs-lookup"><span data-stu-id="ffba8-161">Create a new file called `Untitled-1` by pressing the `Ctrl` + `N` keys.</span></span>

17. <span data-ttu-id="ffba8-162">K deserializaci `JSON` objekt, který jsme obdrželi od `BleConverterModule`, kopírování a vkládání následující fragment kódu do kódu `Untitled-1` souboru.</span><span class="sxs-lookup"><span data-stu-id="ffba8-162">To deserialize the `JSON` object that we receive from the `BleConverterModule`, Copy and paste the following code snippet into the `Untitled-1` file.</span></span> 

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

18. <span data-ttu-id="ffba8-163">Uložte soubor jako `BleConverterData.cs` stisknutím `Ctrl`  +  `Shift`  +  `S`.</span><span class="sxs-lookup"><span data-stu-id="ffba8-163">Save the file as `BleConverterData.cs` by pressing `Ctrl` + `Shift` + `S`.</span></span>
    - <span data-ttu-id="ffba8-164">Na Uložit jako dialogové okno v `Save as Type` rozevírací nabídce vyberte `C# (*.cs;*.csx)`.</span><span class="sxs-lookup"><span data-stu-id="ffba8-164">On the save as dialog box, in the `Save as Type` dropdown menu, select `C# (*.cs;*.csx)`.</span></span>

19. <span data-ttu-id="ffba8-165">Vytvořte nový soubor s názvem `Untitled-1` stisknutím `Ctrl`  +  `N` klíče.</span><span class="sxs-lookup"><span data-stu-id="ffba8-165">Create a new file called `Untitled-1` by pressing the `Ctrl` + `N` keys.</span></span>

20. <span data-ttu-id="ffba8-166">Zkopírujte a vložte následující fragment kódu do `Untitled-1` souboru.</span><span class="sxs-lookup"><span data-stu-id="ffba8-166">Copy and paste the following code snippet into the `Untitled-1` file.</span></span>

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

21. <span data-ttu-id="ffba8-167">Uložte soubor jako `gw-config.json` stisknutím `Ctrl`  +  `Shift`  +  `S`.</span><span class="sxs-lookup"><span data-stu-id="ffba8-167">Save the file as `gw-config.json` by pressing `Ctrl` + `Shift` + `S`.</span></span>
    - <span data-ttu-id="ffba8-168">Na Uložit jako dialogové okno v `Save as Type` rozevírací nabídce vyberte `JSON (*.json;*.bowerrc;*.jshintrc;*.jscsrc;*.eslintrc;*.babelrc;*webmanifest)`.</span><span class="sxs-lookup"><span data-stu-id="ffba8-168">On the save as dialog box, in the `Save as Type` dropdown menu, select `JSON (*.json;*.bowerrc;*.jshintrc;*.jscsrc;*.eslintrc;*.babelrc;*webmanifest)`.</span></span>

22. <span data-ttu-id="ffba8-169">Povolit kopírování konfiguračního souboru do výstupního adresáře, aktualizovat `IoTEdgeConverterModule.csproj` s objektem blob následující XML:</span><span class="sxs-lookup"><span data-stu-id="ffba8-169">To enable copying of the configuration file to the output directory, update the `IoTEdgeConverterModule.csproj` with the following XML blob:</span></span>

   ```xml
     <ItemGroup>
       <None Update="gw-config.json" CopyToOutputDirectory="PreserveNewest" />
     </ItemGroup>
   ```
    
   - <span data-ttu-id="ffba8-170">Aktualizovaný `IoTEdgeConverterModule.csproj` by měl vypadat jako na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="ffba8-170">The updated `IoTEdgeConverterModule.csproj` should look like the following image:</span></span>

    ![Visual Studio Code aktualizovat souboru .csproj](media/iot-hub-iot-edge-create-module/vscode-update-csproj.png)

23. <span data-ttu-id="ffba8-172">Vytvořte nový soubor s názvem `Untitled-1` stisknutím `Ctrl`  +  `N` klíče.</span><span class="sxs-lookup"><span data-stu-id="ffba8-172">Create a new file called `Untitled-1` by pressing the `Ctrl` + `N` keys.</span></span>

24. <span data-ttu-id="ffba8-173">Zkopírujte a vložte následující fragment kódu do `Untitled-1` souboru.</span><span class="sxs-lookup"><span data-stu-id="ffba8-173">Copy and paste the following code snippet into the `Untitled-1` file.</span></span>

   ```powershell
   Copy-Item -Path $env:userprofile\.nuget\packages\microsoft.azure.devices.gateway.native.windows.x64\1.1.3\runtimes\win-x64\native\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\system.runtime.serialization.formatters\4.3.0\lib\netstandard1.4\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\system.runtime.serialization.primitives\4.3.0\lib\netstandard1.3\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\newtonsoft.json\10.0.2\lib\netstandard1.3\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\system.componentmodel.typeconverter\4.3.0\lib\netstandard1.5\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\system.collections.nongeneric\4.3.0\lib\netstandard1.3\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\system.collections.specialized\4.3.0\lib\netstandard1.3\* -Destination .\bin\Debug\netstandard1.3
   ```

25. <span data-ttu-id="ffba8-174">Uložte soubor jako `binplace.ps1` stisknutím `Ctrl`  +  `Shift`  +  `S`.</span><span class="sxs-lookup"><span data-stu-id="ffba8-174">Save the file as `binplace.ps1` by pressing `Ctrl` + `Shift` + `S`.</span></span>
    - <span data-ttu-id="ffba8-175">Na Uložit jako dialogové okno v `Save as Type` rozevírací nabídce vyberte `PowerShell (*.ps1;*.psm1;*.psd1;*.pssc;*.psrc)`.</span><span class="sxs-lookup"><span data-stu-id="ffba8-175">On the save as dialog box, in the `Save as Type` dropdown menu, select `PowerShell (*.ps1;*.psm1;*.psd1;*.pssc;*.psrc)`.</span></span>

26. <span data-ttu-id="ffba8-176">Sestavení projektu stisknutím `Ctrl`  +  `Shift`  +  `B` klíče.</span><span class="sxs-lookup"><span data-stu-id="ffba8-176">Build the project by pressing the `Ctrl` + `Shift` + `B` keys.</span></span> <span data-ttu-id="ffba8-177">Při sestavování projektu poprvé, `Visual Studio Code` vyzve vás s `No build task defined.` dialogové okno, jak je vidět na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="ffba8-177">When you build the project for the first time, `Visual Studio Code` prompts you with the `No build task defined.` dialog as seen in the following image:</span></span>

    ![Dialogové okno úloha sestavení Visual Studio Code](media/iot-hub-iot-edge-create-module/vscode-build-task.png)

    <span data-ttu-id="ffba8-179">a) klikněte na tlačítko `Configure Build Task` tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ffba8-179">a) Click the `Configure Build Task` button.</span></span>

    <span data-ttu-id="ffba8-180">b) v `Select a Task Runner` dialogové okno rozevírací nabídce.</span><span class="sxs-lookup"><span data-stu-id="ffba8-180">b) In the `Select a Task Runner` dialog dropdown menu.</span></span> <span data-ttu-id="ffba8-181">Vyberte `.NET Core` jak je vidět na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="ffba8-181">Select `.NET Core` as seen in the following image:</span></span> 

    ![Visual Studio Code vyberte dialogové okno úloha](media/iot-hub-iot-edge-create-module/vscode-build-task-runner.png)

    <span data-ttu-id="ffba8-183">c) kliknutím `.NET Core` vytvoří položku `tasks.json` ve vaší `.vscode` adresáře a otevře soubor v `code editor` okno.</span><span class="sxs-lookup"><span data-stu-id="ffba8-183">c) Clicking the `.NET Core` item creates the `tasks.json` file in your `.vscode` directory and opens the file in the `code editor` window.</span></span> <span data-ttu-id="ffba8-184">Není potřeba pro úpravu tohoto souboru, zavřete kartu.</span><span class="sxs-lookup"><span data-stu-id="ffba8-184">There is no need to modify this file, close the tab.</span></span>

27.  <span data-ttu-id="ffba8-185">Otevřete `Visual Studio Code` integrované okno terminálu stisknutím `Ctrl`  +  `backtick` klíčů nebo používat nabídky `View`  ->  `Integrated Terminal` a typ **.\binplace.ps1** na `PowerShell` příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="ffba8-185">Open the `Visual Studio Code` integrated terminal window by pressing the `Ctrl` + `backtick` keys or using the menus `View` -> `Integrated Terminal` and type **.\binplace.ps1** into the `PowerShell` command prompt.</span></span> <span data-ttu-id="ffba8-186">Tento příkaz zkopíruje všechny naše závislosti do výstupního adresáře.</span><span class="sxs-lookup"><span data-stu-id="ffba8-186">This command copies all our dependencies to the output directory.</span></span>

28. <span data-ttu-id="ffba8-187">Přejděte do výstupního adresáře projektů v `Integrated Terminal` okno zadáním **cd.\bin\Debug\netstandard1.3**.</span><span class="sxs-lookup"><span data-stu-id="ffba8-187">Navigate to the projects output directory in the `Integrated Terminal` window by typing **cd .\bin\Debug\netstandard1.3**.</span></span>

29. <span data-ttu-id="ffba8-188">Spustit ukázkový projekt zadáním **. \gw.exe gw-config.json** do `Integrated Terminal` okno řádku.</span><span class="sxs-lookup"><span data-stu-id="ffba8-188">Run the sample project by typing **.\gw.exe gw-config.json** into the `Integrated Terminal` window prompt.</span></span> 
    - <span data-ttu-id="ffba8-189">Pokud jste postupovali podle kroků v tomto kurzu úzce, zda nyní je spuštěna `Azure IoT Edge BLE Data Converter Module` ukázkový projekt, jak je vidět na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="ffba8-189">If you have followed the steps in this tutorial closely, you should now be running the `Azure IoT Edge BLE Data Converter Module` sample project as seen in the following image:</span></span>
    
        ![Příklad Simulovaná zařízení spuštěná v kódu Visual Studio](media/iot-hub-iot-edge-create-module/vscode-run.png)
    
    - <span data-ttu-id="ffba8-191">Pokud chcete aplikaci ukončit, stiskněte `<Enter>` klíč.</span><span class="sxs-lookup"><span data-stu-id="ffba8-191">If you want to terminate the application, press the `<Enter>` key.</span></span>

>[!IMPORTANT]
<span data-ttu-id="ffba8-192">Nedoporučuje se používat `Ctrl`  +  `C` ukončit `IoT Edge` brány aplikace (tedy **gw.exe**).</span><span class="sxs-lookup"><span data-stu-id="ffba8-192">It is not recommended to use `Ctrl` + `C` to terminate the `IoT Edge` gateway application (that is, **gw.exe**).</span></span> <span data-ttu-id="ffba8-193">Protože tato akce může způsobit neobvyklým způsobem ukončení procesu.</span><span class="sxs-lookup"><span data-stu-id="ffba8-193">As this action may cause the process to terminate abnormally.</span></span>

