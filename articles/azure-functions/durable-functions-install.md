---
title: "Nainstalujte rozšíření trvanlivý funkce a vzorky – Azure"
description: "Naučte se nainstalovat rozšíření trvanlivý funkce pro funkce Azure pro vývoj portálu nebo vývoj sady Visual Studio."
services: functions
author: cgillum
manager: cfowler
editor: 
tags: 
keywords: 
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/29/2017
ms.author: azfuncdf
ms.openlocfilehash: 58f23406f20f6b0e75ec65197e4cebb6e4e788ac
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/08/2017
---
# <a name="install-the-durable-functions-extension-and-samples-azure-functions"></a>Nainstalujte rozšíření trvanlivý funkce a ukázky (Azure Functions)

[Trvanlivý funkce](durable-functions-overview.md) rozšíření pro Azure Functions je součástí balíčku NuGet [Microsoft.Azure.WebJobs.Extensions.DurableTask](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DurableTask). Tento článek ukazuje, jak nainstalovat balíček a sady vzorků pro následující vývojových prostředí:

* Visual Studio 2017 (doporučeno) 

* portál Azure

## <a name="visual-studio-2017"></a>Visual Studio 2017

Visual Studio teď poskytuje dosažení co nejlepších výsledků pro vývoj aplikací, které používají trvanlivý funkce.  Funkce můžete spustit místně a také mohou být publikovány do služby Azure. Můžete spustit s prázdným projektem nebo sadu ukázkových funkcích.

### <a name="prerequisites"></a>Požadavky

* Nainstalujte [nejnovější verze sady Visual Studio](https://www.visualstudio.com/downloads/) (verze 15.3 nebo novější). Zahrnout **Azure development** zatížení v možnostech instalace.

### <a name="start-with-sample-functions"></a>Začněte s ukázkových funkcích

1. Stažení [soubor .zip ukázková aplikace pro sadu Visual Studio](https://azure.github.io/azure-functions-durable-extension/files/VSDFSampleApp.zip). Nepotřebujete přidat odkaz NuGet, protože ukázkový projekt již existuje.
2. Nainstalujte a spusťte [emulátoru úložiště Azure](https://docs.microsoft.com/azure/storage/storage-use-emulator) verze 5.2 nebo novější. Alternativně můžete aktualizovat *local.appsettings.json* soubor s skutečné připojovacích řetězců Azure Storage.
3. Otevřete projekt ve Visual Studio 2017. 
4. Pokyny ke spuštění ukázky začínat [funkce řetězení – Hello ukázka pořadí](durable-functions-sequence.md). Ukázku můžete spustit místně nebo publikované do Azure.

### <a name="start-with-an-empty-project"></a>Začít s prázdným projektem
 
Postupujte podle pokynů k stejné jako u počínaje ukázku, ale proveďte následující kroky místo stahování *.zip* souboru:

1. Vytvoření projektu funkce aplikace.
2. Přidejte následující odkaz balíčku NuGet pro vaše *.csproj* souboru:

   ```xml
   <PackageReference Include="Microsoft.Azure.WebJobs.Extensions.DurableTask" Version="1.0.0-beta" />
   ```
   
## <a name="visual-studio-code"></a>Visual Studio Code

Visual Studio Code poskytuje místní vývojové prostředí, který po sobě zakrývá všechny hlavní platformy – Windows, systému macOS a Linux.  Funkce můžete spustit místně a také mohou být publikovány do služby Azure. Můžete spustit s prázdným projektem nebo sadu ukázkových funkcích.

### <a name="prerequisites"></a>Požadavky

* Nainstalujte [nejnovější verzi Visual Studio Code](https://code.visualstudio.com/Download) 

* Postupujte podle pokynů v části "Instalace Azure funkce základní nástrojů" v [kódu a testování Azure Functions místně](https://docs.microsoft.com/azure/azure-functions/functions-run-local)

    >[!IMPORTANT]
    > Pokud již máte nástrojů pro platformy různé funkce Azure, můžete je aktualizujte na nejnovější dostupnou verzi.

*  Nainstalujte a spusťte [emulátoru úložiště Azure](https://docs.microsoft.com/azure/storage/storage-use-emulator) verze 5.2 nebo novější. Alternativně můžete aktualizovat *local.appsettings.json* soubor s skutečné připojení úložiště Azure. 


### <a name="start-with-sample-functions"></a>Začněte s ukázkových funkcích

1. Klon [úložiště odolné funkce](https://github.com/Azure/azure-functions-durable-extension.git).
2. Přejděte na vašem počítači, aby [C# skript Ukázky složky](https://github.com/Azure/azure-functions-durable-extension/tree/master/samples/csx). 
3. Nainstalujte rozšíření trvanlivý funkce Azure tak, že spustíte následující v příkazu výzvy a Terminálový okno:

    ```bash
    func extensions install -p Microsoft.Azure.WebJobs.Extensions.DurableTask -v 1.1.0-beta2
    ```
4. Spusťte emulátor úložiště Azure nebo aktualizace *local.appsettings.json* soubor s skutečné připojovacího řetězce úložiště Azure.
3. Otevřete projekt ve Visual Studio Code. 
5. Pokyny ke spuštění ukázky začínat [funkce řetězení – Hello ukázka pořadí](durable-functions-sequence.md). Ukázku můžete spustit místně nebo publikované do Azure.
6. Spusťte projekt tak, že spustíte v příkazu výzvy a Terminálový následující příkaz:
    ```bash
    func host start
    ```

### <a name="start-with-an-empty-project"></a>Začít s prázdným projektem
 
1. V příkazu výzvy a Terminálový přejděte do složky, který bude hostovat aplikaci funkce.
2. Nainstalujte rozšíření trvanlivý funkce Azure tak, že spustíte následující v příkazu výzvy a Terminálový okno:

    ```bash
    func extensions install -p Microsoft.Azure.WebJobs.Extensions.DurableTask -v 1.1.0-beta2
    ```
3. Vytvoření projektu funkce aplikace tak, že spustíte následující příkaz:

    ```bash
    func init
    ``` 
4. Spusťte emulátor úložiště Azure nebo aktualizace *local.appsettings.json* soubor s skutečné připojovacího řetězce úložiště Azure.
5. Dále vytvořte novou funkci tak, že spustíte následující příkaz a postupujte podle kroků průvodce:

    ```bash
    func new
    ```
    >[!IMPORTANT]
    > Aktuálně šablona trvanlivý funkce není k dispozici, ale můžete začít s jedním z podporovaných možností a potom upravte kód. Použít pro referenci ukázky pro [Orchestration klienta](https://github.com/Azure/azure-functions-durable-extension/tree/master/samples/csx/HttpStart), [aktivační událost Orchestration](https://github.com/Azure/azure-functions-durable-extension/tree/master/samples/csx/E1_HelloSequence), a [aktivační události aktivity](https://github.com/Azure/azure-functions-durable-extension/tree/master/samples/csx/E1_HelloSequence).

6. Otevřete složku projektu ve Visual Studio Code a pokračovat změnou kód šablony. 
7. Spusťte projekt tak, že spustíte v příkazu výzvy a Terminálový následující příkaz:
    ```bash
    func host start
    ```

## <a name="azure-portal"></a>portál Azure

Pokud dáváte přednost, můžete portál Azure pro vývoj trvanlivý funkce.

### <a name="create-an-orchestrator-function"></a>Vytvoření funkce produktu orchestrator

1. Vytvořit novou aplikaci funkce v [functions.azure.com](https://functions.azure.com/signin).

2. Konfigurace funkce aplikaci [použít verzi 2.0 runtime](functions-versions.md).

3. Vytvořte novou funkci tak, že vyberete **"Vytvoření vlastní funkce."** .

4. Změna **jazyk** k **C#**, **scénář** k **trvanlivý funkce** a vyberte **trvanlivý Starter funkce protokolu Http -C#** šablony.

5. V části **není nainstalovaná rozšíření**, klikněte na tlačítko **nainstalovat** ke stažení rozšíření od NuGet.org. 

6. Po dokončení instalace pokračujte vytvoření klienta funkce orchestration – **"HttpStart"** vytvořený výběrem **trvanlivý funkce protokolu Http Starter - C#** šablony.

7. Teď vytvořte funkce orchestration **"HelloSequence"** z **trvanlivý Orchestrator funkce – C#** šablony.

8. A bude se volat funkci naposledy **"Hello"** z **trvanlivý aktivita funkce – C#** šablony.

9. Přejděte na **"HttpStart"** funkce a zkopírujte jeho adresa URL.

10. Použijte Postman nebo adresu cURL k volání trvanlivý funkce. Před testováním, nahraďte v adrese URL **{funkce%{FunctionName/}** s názvem funkce orchestrator - **HelloSequence**.  Není třeba žádná data, stačí použít operaci POST. 

    ```bash
    curl -X POST https://{your function app name}.azurewebsites.net/api/orchestrators/HelloSequence
    ```

11. Potom zavolejte **"statusQueryGetUri"** koncový bod a můžete zobrazit aktuální stav trvanlivý funkce

    ```json
        {
            "runtimeStatus": "Running",
            "input": null,
            "output": null,
            "createdTime": "2017-12-01T05:37:33Z",
            "lastUpdatedTime": "2017-12-01T05:37:36Z"
        }
    ```

12. Pokračovat volání **"statusQueryGetUri"** koncový bod, dokud se stav změní na **"Dokončeno"** 

    ```json
    {
            "runtimeStatus": "Completed",
            "input": null,
            "output": [
                "Hello Tokyo!",
                "Hello Seattle!",
                "Hello London!"
            ],
            "createdTime": "2017-12-01T05:38:22Z",
            "lastUpdatedTime": "2017-12-01T05:38:28Z"
        }
    ```

Blahopřejeme! První trvanlivý funkce je spuštěná na portálu Azure!

## <a name="next-steps"></a>Další kroky

> [!div class="nextstepaction"]
> [Spuštění funkce řetězení ukázka](durable-functions-sequence.md)
