---
title: "Vývoj a spustit místně na Azure functions | Microsoft Docs"
description: "Zjistěte, jak kód a testovat Azure functions v místním počítači před spuštěním na Azure Functions."
services: functions
documentationcenter: na
author: lindydonna
manager: erikre
editor: 
ms.assetid: 242736be-ec66-4114-924b-31795fd18884
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: multiple
ms.topic: article
ms.date: 07/12/2017
ms.author: glenga
ms.openlocfilehash: bbe03973dbd7c70463caa6d1a45efae2ec722c05
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="code-and-test-azure-functions-locally"></a>Kód a testovat místně na Azure functions

Když [portál Azure] poskytuje úplnou sadu nástrojů pro vývoj a testování Azure Functions se celá řada vývojářů raději místní vývojové prostředí. Azure Functions umožňuje snadno použít editor Oblíbené kódu a místní vývojové nástroje pro vývoj a testování funkcí v místním počítači. Funkce můžete aktivovat pro události v Azure a při ladění vašich C# a funkce jazyka JavaScript v místním počítači. 

Pokud jste Azure Functions Visual Studio C# vývojář, také [se integruje s Visual Studio 2017](functions-develop-vs.md).

## <a name="install-the-azure-functions-core-tools"></a>Instalace nástroje Azure Functions jádra

Nástroje Azure základní funkce je místní verzi modulu runtime Azure Functions, který můžete spustit na místním počítači s Windows. Není emulátor ani simulátor. Je stejný modul runtime, který zajišťuje funkce v Azure.

[Nástroje základní funkce Azure] je k dispozici jako balíčku npm. Je nutné nejprve [nainstalovat NodeJS](https://docs.npmjs.com/getting-started/installing-node), což zahrnuje npm.  

>[!NOTE]
>V tomto okamžiku můžete balíček nástroje základní funkce Azure nainstalovat pouze na počítačích s Windows. Toto omezení je kvůli dočasné omezení na hostiteli funkce.

[Nástroje základní funkce Azure] přidá následující aliasy příkazů:
* **Func**
* **azfun**
* **azurefunctions**

Všechny tyto alias lze použít místo `func` zobrazeno v příkladech v tomto tématu.

## <a name="create-a-local-functions-project"></a>Vytvoření projektu místní funkce

Při místním spuštění projektu funkce je adresář, který má soubory host.json a local.settings.json. Tento adresář je ekvivalentem funkce aplikace v Azure. Další informace o struktuře složky Azure Functions najdete v tématu [Příručka pro vývojáře Azure Functions](functions-reference.md#folder-structure).

Na příkazovém řádku spusťte následující příkaz:

```
func init MyFunctionProj
```

Výstup bude vypadat jako v následujícím příkladu:

```
Writing .gitignore
Writing host.json
Writing local.settings.json
Created launch.json
Initialized empty Git repository in D:/Code/Playground/MyFunctionProj/.git/
```

Odhlásit se od vytvoření úložiště Git, použijte možnost `--no-source-control [-n]`.

<a name="local-settings"></a>

## <a name="local-settings-file"></a>Nastavení místního souboru

Soubor local.settings.json ukládá nastavení aplikace, řetězce připojení a nastavení nástroje základní funkce Azure. Má následující strukturu:

```json
{
  "IsEncrypted": false,   
  "Values": {
    "AzureWebJobsStorage": "<connection string>", 
    "AzureWebJobsDashboard": "<connection string>", 
  },
  "Host": {
    "LocalHttpPort": 7071, 
    "CORS": "*" 
  },
  "ConnectionStrings": {
    "SQLConnectionString": "Value"
  }
}
```
| Nastavení      | Popis                            |
| ------------ | -------------------------------------- |
| **IsEncrypted** | Pokud nastavíte hodnotu **true**, všechny hodnoty jsou šifrované pomocí klíče místního počítače. Použít s `func settings` příkazy. Výchozí hodnota je **false**. |
| **Hodnoty** | Kolekce nastavení aplikace používá při místním spuštění. Přidáte nastavení aplikace s tímto objektem.  |
| **AzureWebJobsStorage** | Nastaví připojovací řetězec k účtu Azure Storage, který se používá interně modulem runtime Azure Functions. Účet úložiště podporuje vaše funkce aktivační události. Toto nastavení připojení účtu úložiště je vyžadována pro všechny funkce, s výjimkou funkce protokolu HTTP aktivované.  |
| **AzureWebJobsDashboard** | Nastaví připojovací řetězec k účtu úložiště Azure, který se používá k ukládání protokolů funkce. Tato volitelná hodnota zpřístupní protokoly na portálu.|
| **Hostitele** | Nastavení v této části přizpůsobit funkce hostitelský proces, při místním spuštění. | 
| **LocalHttpPort** | Nastaví výchozí port použitý při spuštění místního hostitele funkce (`func host start` a `func run`). `--port` Možnost příkazového řádku má přednost před tuto hodnotu. |
| **CORS** | Definuje zdroje povolené pro [(CORS) pro sdílení prostředků různého původu](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing). Zdroje se zadávají jako seznam oddělený čárkami bez mezer. Hodnota zástupného znaku (**\***) je podporováno, což umožňuje požadavky od jakýkoli původ. |
| **ConnectionStrings** | Obsahuje databázové připojovací řetězce pro funkcí. Připojovací řetězce v tento objekt se přidají do prostředí s typem zprostředkovatele **System.Data.SqlClient**.  | 

Většina triggerů a vazeb mít **připojení** vlastnost, která se mapuje na název nastavení aplikace nebo proměnné prostředí. Pro každou vlastnost připojení musí být definována v souboru local.settings.json nastavení aplikace. 

Tato nastavení lze také přečíst v kódu jako proměnné prostředí. V jazyce C#, použijte [System.Environment.GetEnvironmentVariable](https://msdn.microsoft.com/library/system.environment.getenvironmentvariable(v=vs.110).aspx) nebo [ConfigurationManager.AppSettings](https://msdn.microsoft.com/library/system.configuration.configurationmanager.appsettings%28v=vs.110%29.aspx). V jazyce JavaScript, použijte `process.env`. Bylo nastaveno jako systémové proměnné prostředí mají přednost před hodnot v souboru local.settings.json. 

Nastavení v souboru local.settings.json používají funkce nástroje jenom, při místním spuštění. Ve výchozím nastavení tato nastavení se nemigrují automaticky při publikování projektu do Azure. Použití `--publish-local-settings` přepínač [při publikování](#publish) a ujistěte se, tato nastavení jsou přidány do aplikaci funkce v Azure.

Když je nastavená žádný platný úložiště připojovací řetězec pro **AzureWebJobsStorage**, se zobrazí následující chybová zpráva:  

>Chybí hodnota pro AzureWebJobsStorage v local.settings.json. To je potřeba pro všechny aktivační události než HTTP. Můžete spustit ' func azure functionary načítání app nastavení <functionAppName>, nebo zadejte připojovací řetězec v local.settings.json.
  
[!INCLUDE [Note to not use local storage](../../includes/functions-local-settings-note.md)]

### <a name="configure-app-settings"></a>Konfigurace nastavení aplikace

Pokud chcete nastavit hodnotu pro připojovací řetězce, můžete provést jednu z těchto možností:
* Zadejte připojovací řetězec z [Azure Storage Explorer](http://storageexplorer.com/).
* Použijte jednu z následujících příkazů:

    ```
    func azure functionapp fetch-app-settings <FunctionAppName>
    ```
    ```
    func azure functionapp storage fetch-connection-string <StorageAccountName>
    ```
    Oba příkazy vyžadovat, abyste první přihlášení do Azure.

## <a name="create-a-function"></a>Vytvoření funkce

Pokud chcete vytvořit funkci, spusťte následující příkaz:

```
func new
``` 
`func new`podporuje následující volitelné argumenty:

| Argument     | Popis                            |
| ------------ | -------------------------------------- |
| **`--language -l`** | Šablona programovací jazyk, například C#, F # nebo JavaScript. |
| **`--template -t`** | Název šablony. |
| **`--name -n`** | Název funkce. |

Například pokud chcete vytvořit aktivační událost jazyka JavaScript HTTP, spusťte tento příkaz:

```
func new --language JavaScript --template HttpTrigger --name MyHttpTrigger
```

Chcete-li vytvořit funkci aktivovaného fronty, spusťte:

```
func new --language JavaScript --template QueueTrigger --name QueueTriggerJS
```

## <a name="run-functions-locally"></a>Místní spuštění funkce

Pokud chcete spustit funkce projektu, spusťte hostiteli funkce. Hostitel umožňuje aktivačních událostí pro všechny funkce v projektu:

```
func host start
```

`func host start`podporuje následující možnosti:

| Možnost     | Popis                            |
| ------------ | -------------------------------------- |
|**`--port -p`** | Místní port pro naslouchání. Výchozí hodnota: 7071. |
| **`--debug <type>`** | Možnosti jsou `VSCode` a `VS`. |
| **`--cors`** | Seznam původů CORS, bez mezer oddělených čárkami. |
| **`--nodeDebugPort -n`** | Port pro uzel ladicí program používat. Výchozí hodnota: Hodnota z launch.json nebo 5858. |
| **`--debugLevel -d`** | Úroveň trasování konzoly (vypnuto, podrobné nastavení, informace, varování nebo chyba). Výchozí: informace.|
| **`--timeout -t`** | Časový limit pro o spuštění t hostitele na funkce v sekundách. Výchozí hodnota: 20 sekund.|
| **`--useHttps`** | Vytvořit vazbu na https://localhost: {port} místo na http://localhost: {port}. Ve výchozím nastavení tato možnost vytvoří důvěryhodný certifikát v počítači.|
| **`--pause-on-error`** | Pozastavení další vstupní před ukončení procesu. Užitečné při spuštění nástroje základní funkce Azure z integrované vývojové prostředí (IDE).|

Při spuštění funkce hostitele výstupy funkce aktivované protokolem URL HTTP:

```
Found the following functions:
Host.Functions.MyHttpTrigger

ob host started
Http Function MyHttpTrigger: http://localhost:7071/api/MyHttpTrigger
```

### <a name="debug-in-vs-code-or-visual-studio"></a>Ladění ve VS Code nebo Visual Studio

Chcete-li připojit ladicí program, předat `--debug` argument. Chcete-li ladit funkce jazyka JavaScript, použijte Visual Studio Code. Pro C# funkce použijte sadu Visual Studio.

Chcete-li ladit funkce jazyka C#, použijte `--debug vs`. Můžete také použít [Azure funkce 2017 nástroje sady Visual Studio](https://blogs.msdn.microsoft.com/webdev/2017/05/10/azure-function-tools-for-visual-studio-2017/). 

Spusťte hostiteli a nastavte ladění jazyka JavaScript, spusťte příkaz:

```
func host start --debug vscode
```

Potom v sadě Visual Studio Code v **ladění** zobrazit, vyberte možnost **připojit k Azure Functions**. Můžete připojit zarážky, zkontrolujte proměnné a krok prostřednictvím kódu.

![Ladění jazyka JavaScript s kódem jazyka Visual Studio](./media/functions-run-local/vscode-javascript-debugging.png)

### <a name="passing-test-data-to-a-function"></a>Předávání testů dat na funkci

Můžete také vyvolat funkci přímo pomocí `func run <FunctionName>` a zadejte vstupní data pro funkce. Tento příkaz je podobná spuštění funkce pomocí **Test** na portálu Azure. Tento příkaz spustí celý funkce hostitele.

`func run`podporuje následující možnosti:

| Možnost     | Popis                            |
| ------------ | -------------------------------------- |
| **`--content -c`** | Vložený obsah. |
| **`--debug -d`** | Připojte ladicí program k hostitelskému procesu před spuštěním funkce.|
| **`--timeout -t`** | Doba čekání (v sekundách), dokud místního hostitele funkce není připravena.|
| **`--file -f`** | Název souboru, který chcete použít jako obsah.|
| **`--no-interactive`** | Nezobrazí výzvu pro vstup. Tato možnost je užitečná pro scénáře automatizace.|

Například volání funkce aktivované protokolem HTTP a předat obsahu, spusťte následující příkaz:

```
func run MyHttpTrigger -c '{\"name\": \"Azure\"}'
```

## <a name="publish"></a>Publikování v Azure

K publikování funkce projektu do aplikace pro funkce v Azure, použijte `publish` příkaz:

```
func azure functionapp publish <FunctionAppName>
```

Můžete použít následující možnosti:

| Možnost     | Popis                            |
| ------------ | -------------------------------------- |
| **`--publish-local-settings -i`** |  Nastavení publikování v local.settings.json do Azure, výzvy přepsat, pokud nastavení již existuje.|
| **`--overwrite-settings -y`** | Musí být použit s `-i`. Pokud se liší, přepíše místní hodnotou AppSettings v Azure. Výchozí hodnota je výzva.|

`publish` Příkaz odešle obsah adresáře projektu funkce. Pokud odstraníte soubory místně, `publish` příkaz nedojde k jejich odstranění z Azure. Můžete odstranit soubory v Azure pomocí [Kudu nástroj](functions-how-to-use-azure-function-app-settings.md#kudu) v [portál Azure].

## <a name="next-steps"></a>Další kroky

Nástroje Azure základní funkce je [otevřít zdroj a hostované na Githubu](https://github.com/azure/azure-functions-cli).  
Do souboru žádost chyb nebo funkce [otevřete potíže Githubu](https://github.com/azure/azure-functions-cli/issues). 

<!-- LINKS -->

[Nástroje základní funkce Azure]: https://www.npmjs.com/package/azure-functions-core-tools
[portál Azure]: https://portal.azure.com 
