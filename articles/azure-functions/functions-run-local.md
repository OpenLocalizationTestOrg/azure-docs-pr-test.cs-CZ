---
title: "aaaDevelop a spuštění Azure funkce místně | Microsoft Docs"
description: "Zjistěte, jak toocode a testování Azure funkce v místním počítači před spuštěním na Azure Functions."
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
ms.openlocfilehash: 342ed4d6df41a2d2df9067948e19e347bb52c844
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="code-and-test-azure-functions-locally"></a>Kód a testovat místně na Azure functions

Při hello [portál Azure] poskytuje úplnou sadu nástrojů pro vývoj a testování Azure Functions se celá řada vývojářů raději místní vývojové prostředí. Azure Functions umožňuje snadno toouse své oblíbené kódu editoru a toodevelop nástroje pro místní vývoj a testování funkcí v místním počítači. Funkce můžete aktivovat pro události v Azure a při ladění vašich C# a funkce jazyka JavaScript v místním počítači. 

Pokud jste Azure Functions Visual Studio C# vývojář, také [se integruje s Visual Studio 2017](functions-develop-vs.md).

## <a name="install-hello-azure-functions-core-tools"></a>Instalace nástroje základní funkce Azure hello

Nástroje Azure základní funkce je místní verzi modulu runtime Azure Functions hello, který můžete spustit na místním počítači s Windows. Není emulátor ani simulátor. Má hello stejné runtime této zajišťuje funkce v Azure.

Hello [nástroje základní funkce Azure] je k dispozici jako balíčku npm. Je nutné nejprve [nainstalovat NodeJS](https://docs.npmjs.com/getting-started/installing-node), což zahrnuje npm.  

>[!NOTE]
>V tomto okamžiku můžete balíček nástroje základní funkce Azure hello nainstalovat pouze na počítačích s Windows. Toto omezení je kvůli dočasné omezení tooa v hostiteli funkce hello.

[nástroje základní funkce Azure] přidá hello následující aliasy příkazů:
* **Func**
* **azfun**
* **azurefunctions**

Všechny tyto alias lze použít místo `func` hello příkladů v tomto tématu.

## <a name="create-a-local-functions-project"></a>Vytvoření projektu místní funkce

Při místním spuštění projektu funkce je adresář, který má hello soubory host.json a local.settings.json. Tento adresář je hello ekvivalentní funkce aplikace v Azure. toolearn Další informace o hello struktura složek Azure Functions, najdete v části hello [Příručka pro vývojáře Azure Functions](functions-reference.md#folder-structure).

Na příkazovém řádku spusťte následující příkaz hello:

```
func init MyFunctionProj
```

výstup Hello vypadá hello následující ukázka:

```
Writing .gitignore
Writing host.json
Writing local.settings.json
Created launch.json
Initialized empty Git repository in D:/Code/Playground/MyFunctionProj/.git/
```

tooopt mimo vytvoření úložiště Git, možnost použití hello `--no-source-control [-n]`.

<a name="local-settings"></a>

## <a name="local-settings-file"></a>Nastavení místního souboru

Hello souboru local.settings.json ukládá nastavení aplikace, řetězce připojení a nastavení nástroje základní funkce Azure. Má hello strukturu:

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
| **IsEncrypted** | Pokud nastavíte příliš**true**, všechny hodnoty jsou šifrované pomocí klíče místního počítače. Použít s `func settings` příkazy. Výchozí hodnota je **false**. |
| **Hodnoty** | Kolekce nastavení aplikace používá při místním spuštění. Přidejte objekt toothis nastavení vaší aplikace.  |
| **AzureWebJobsStorage** | Nastaví hello připojovací řetězec toohello účet úložiště Azure, který používá se interně pomocí modulu runtime Azure Functions hello. účet úložiště Hello podporuje vaše funkce aktivační události. Toto nastavení připojení účtu úložiště je vyžadována pro všechny funkce, s výjimkou funkce protokolu HTTP aktivované.  |
| **AzureWebJobsDashboard** | Nastaví hello připojení řetězec toohello Azure Storage účtu, který je použité toostore hello funkce protokoly. Tato volitelná hodnota zpřístupní hello protokoly portálu hello.|
| **Hostitele** | Nastavení v této části přizpůsobit hello funkce hostitelský proces, při místním spuštění. | 
| **LocalHttpPort** | Nastaví hello výchozí port použitý při spuštění místního hostitele funkce hello (`func host start` a `func run`). Hello `--port` možnost příkazového řádku má přednost před tuto hodnotu. |
| **CORS** | Definuje hello zdroje povolené pro [(CORS) pro sdílení prostředků různého původu](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing). Zdroje se zadávají jako seznam oddělený čárkami bez mezer. Hello hodnota zástupného znaku (**\***) je podporováno, což umožňuje požadavky od jakýkoli původ. |
| **ConnectionStrings** | Obsahuje hello databázové připojovací řetězce pro funkcí. Připojovací řetězce v tento objekt se přidají toohello prostředí s typem zprostředkovatele hello **System.Data.SqlClient**.  | 

Většina triggerů a vazeb mít **připojení** vlastnost, která mapuje toohello název nastavení aplikace nebo proměnné prostředí. Pro každou vlastnost připojení musí být definována v souboru local.settings.json nastavení aplikace. 

Tato nastavení lze také přečíst v kódu jako proměnné prostředí. V jazyce C#, použijte [System.Environment.GetEnvironmentVariable](https://msdn.microsoft.com/library/system.environment.getenvironmentvariable(v=vs.110).aspx) nebo [ConfigurationManager.AppSettings](https://msdn.microsoft.com/library/system.configuration.configurationmanager.appsettings%28v=vs.110%29.aspx). V jazyce JavaScript, použijte `process.env`. Bylo nastaveno jako systémové proměnné prostředí mají přednost před hodnot v souboru local.settings.json hello. 

Nastavení v souboru local.settings.json hello používají funkce nástroje jenom, při místním spuštění. Ve výchozím nastavení tato nastavení se nemigrují automaticky publikované tooAzure po hello projektu. Použití hello `--publish-local-settings` přepínač [při publikování](#publish) toomake, že tato nastavení jsou přidaný toohello funkce aplikace v Azure.

Když je nastavená žádný platný úložiště připojovací řetězec pro **AzureWebJobsStorage**, se zobrazí následující chybová zpráva hello:  

>Chybí hodnota pro AzureWebJobsStorage v local.settings.json. To je potřeba pro všechny aktivační události než HTTP. Můžete spustit ' func azure functionary načítání app nastavení <functionAppName>, nebo zadejte připojovací řetězec v local.settings.json.
  
[!INCLUDE [Note toonot use local storage](../../includes/functions-local-settings-note.md)]

### <a name="configure-app-settings"></a>Konfigurace nastavení aplikace

tooset hodnotu pro připojovací řetězce, můžete provést jednu z následujících akcí hello:
* Zadejte připojovací řetězec hello z [Azure Storage Explorer](http://storageexplorer.com/).
* Použijte jednu z hello následující příkazy:

    ```
    func azure functionapp fetch-app-settings <FunctionAppName>
    ```
    ```
    func azure functionapp storage fetch-connection-string <StorageAccountName>
    ```
    Oba příkazy vyžadují toofirst přihlášení je tooAzure.

## <a name="create-a-function"></a>Vytvoření funkce

toocreate funkci, spusťte následující příkaz hello:

```
func new
``` 
`func new`podporuje následující volitelné argumenty hello:

| Argument     | Popis                            |
| ------------ | -------------------------------------- |
| **`--language -l`** | Šablona Hello programovací jazyk, například C#, F # nebo JavaScript. |
| **`--template -t`** | Název šablony Hello. |
| **`--name -n`** | název funkce Hello. |

Například toocreate aktivační událost jazyka JavaScript HTTP, spusťte:

```
func new --language JavaScript --template HttpTrigger --name MyHttpTrigger
```

toocreate funkce aktivované fronty, spusťte:

```
func new --language JavaScript --template QueueTrigger --name QueueTriggerJS
```

## <a name="run-functions-locally"></a>Místní spuštění funkce

toorun funkce projektu, spusťte hello funkce hostitelů. Hello hostitele umožňuje aktivačních událostí pro všechny funkce v projektu hello:

```
func host start
```

`func host start`hello podporuje následující možnosti:

| Možnost     | Popis                            |
| ------------ | -------------------------------------- |
|**`--port -p`** | Hello toolisten místního portu na. Výchozí hodnota: 7071. |
| **`--debug <type>`** | Možnosti Hello jsou `VSCode` a `VS`. |
| **`--cors`** | Seznam původů CORS, bez mezer oddělených čárkami. |
| **`--nodeDebugPort -n`** | Hello port pro toouse ladicího programu hello uzlu. Výchozí hodnota: Hodnota z launch.json nebo 5858. |
| **`--debugLevel -d`** | úroveň trasování Hello konzoly (vypnuto, podrobné nastavení, informace, varování nebo chyba). Výchozí: informace.|
| **`--timeout -t`** | Hello vypršení časového limitu pro hello funkce hostitele t o spuštění, v sekundách. Výchozí hodnota: 20 sekund.|
| **`--useHttps`** | Vytvoření vazby toohttps://localhost: {port} místo toohttp://localhost: {port}. Ve výchozím nastavení tato možnost vytvoří důvěryhodný certifikát v počítači.|
| **`--pause-on-error`** | Pozastavení další vstupní před ukončením hello procesu. Užitečné při spuštění nástroje základní funkce Azure z integrované vývojové prostředí (IDE).|

Při spuštění hostitelů funkce hello výstupy funkce aktivované protokolem URL HTTP hello:

```
Found hello following functions:
Host.Functions.MyHttpTrigger

ob host started
Http Function MyHttpTrigger: http://localhost:7071/api/MyHttpTrigger
```

### <a name="debug-in-vs-code-or-visual-studio"></a>Ladění ve VS Code nebo Visual Studio

ladicí program, tooattach předat hello `--debug` argument. funkce jazyka JavaScript toodebug, použijte Visual Studio Code. Pro C# funkce použijte sadu Visual Studio.

pomocí funkce toodebug C#, `--debug vs`. Můžete také použít [Azure funkce 2017 nástroje sady Visual Studio](https://blogs.msdn.microsoft.com/webdev/2017/05/10/azure-function-tools-for-visual-studio-2017/). 

toolaunch hello hostitelů a nastavení ladění jazyka JavaScript, spusťte:

```
func host start --debug vscode
```

Potom můžete v sadě Visual Studio Code hello **ladění** zobrazit, vyberte možnost **připojte tooAzure funkce**. Můžete připojit zarážky, zkontrolujte proměnné a krok prostřednictvím kódu.

![Ladění jazyka JavaScript s kódem jazyka Visual Studio](./media/functions-run-local/vscode-javascript-debugging.png)

### <a name="passing-test-data-tooa-function"></a>Předávání testů data tooa funkce

Můžete také vyvolat funkci přímo pomocí `func run <FunctionName>` a zadejte vstupní data pro funkce hello. Tento příkaz je podobné toorunning funkce pomocí hello **Test** ve hello portálu Azure. Tento příkaz spustí hello celý funkce hostitele.

`func run`hello podporuje následující možnosti:

| Možnost     | Popis                            |
| ------------ | -------------------------------------- |
| **`--content -c`** | Vložený obsah. |
| **`--debug -d`** | Připojte ladicí toohello hostitelský proces před spuštěním funkce hello.|
| **`--timeout -t`** | Toowait čas (v sekundách) až do místního hostitele funkce hello je připraven.|
| **`--file -f`** | Hello toouse název souboru jako obsah.|
| **`--no-interactive`** | Nezobrazí výzvu pro vstup. Tato možnost je užitečná pro scénáře automatizace.|

Například toocall funkce aktivované protokolem HTTP a text obsahu pass, spusťte následující příkaz hello:

```
func run MyHttpTrigger -c '{\"name\": \"Azure\"}'
```

## <a name="publish"></a>Publikování tooAzure

toopublish aplikaci funkce projektu tooa funkce v Azure, použijte hello `publish` příkaz:

```
func azure functionapp publish <FunctionAppName>
```

Můžete použít hello následující možnosti:

| Možnost     | Popis                            |
| ------------ | -------------------------------------- |
| **`--publish-local-settings -i`** |  Nastavení publikování v local.settings.json tooAzure, výzvy toooverwrite Pokud hello nastavení již existuje.|
| **`--overwrite-settings -y`** | Musí být použit s `-i`. Pokud se liší, přepíše místní hodnotou AppSettings v Azure. Výchozí hodnota je výzva.|

Hello `publish` příkaz odešle hello obsah adresáře projektu funkce hello. Pokud odstraníte soubory lokálně, hello `publish` příkaz nedojde k jejich odstranění z Azure. Můžete odstranit soubory v Azure pomocí hello [Kudu nástroj](functions-how-to-use-azure-function-app-settings.md#kudu) v hello [portál Azure].

## <a name="next-steps"></a>Další kroky

Nástroje Azure základní funkce je [otevřít zdroj a hostované na Githubu](https://github.com/azure/azure-functions-cli).  
toofile žádost chyb nebo funkce [otevřete potíže Githubu](https://github.com/azure/azure-functions-cli/issues). 

<!-- LINKS -->

[nástroje základní funkce Azure]: https://www.npmjs.com/package/azure-functions-core-tools
[portál Azure]: https://portal.azure.com 
