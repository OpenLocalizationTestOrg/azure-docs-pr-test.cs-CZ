---
title: "aaaCreate první funkce z hello rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Zjistěte, jak hello toocreate vaše první Azure fungovat pro provádění bez serveru pomocí rozhraní příkazového řádku Azure."
services: functions
keywords: 
author: ggailey777
ms.author: glenga
ms.assetid: 674a01a7-fd34-4775-8b69-893182742ae0
ms.date: 08/22/2017
ms.topic: hero-article
ms.service: functions
ms.custom: mvc
ms.devlang: azure-cli
manager: erikre
ms.openlocfilehash: 5feed0045d4998b88b0e1bb50996cb7bb42b0822
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-function-using-hello-azure-cli"></a>Vytvoření první funkce pomocí hello rozhraní příkazového řádku Azure

Tento rychlý úvodní kurz vás provede jak toouse Azure Functions toocreate svoji první funkci. Můžete použít rozhraní příkazového řádku Azure toocreate hello funkce aplikace, která je hello bez serveru infrastruktury, který je hostitelem funkce. samotný kód Funkce Hello je nasazena z úložiště Githubu ukázka.    

Můžete provést kroky hello níže použití počítače Mac, Windows nebo Linux. 

## <a name="prerequisites"></a>Požadavky 

Před spuštěním této ukázce, musíte mít následující hello:

+ Aktivní účet [GitHub](https://github.com) 
+ Aktivní předplatné Azure.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Pokud zvolte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto tématu vyžaduje, že používáte hello Azure CLI verze 2.0 nebo novější. Spustit `az --version` toofind hello verze. Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli). 


## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Vytvořte skupinu prostředků s hello [vytvořit skupinu az](/cli/azure/group#create). Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure, jako například aplikace Function App, databáze a účty úložiště.

Hello následující příklad vytvoří skupinu prostředků s názvem `myResourceGroup`.  
Pokud nepoužíváte cloudové prostředí, musíte se nejdřív přihlásit pomocí `az login`.

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```


## <a name="create-an-azure-storage-account"></a>Vytvoření účtu služby Azure Storage

Funkce používá stav toomaintain účtu úložiště Azure a další informace o funkcí. Vytvořit účet úložiště ve skupině prostředků hello jste vytvořili pomocí hello [vytvořit účet úložiště az](/cli/azure/storage/account#create) příkaz.

V hello následující příkaz, nahraďte váš vlastní název účtu globálně jedinečný úložiště, kde uvidíte hello `<storage_name>` zástupný symbol. Názvy účtů úložiště musí mít od 3 do 24 znaků a můžou obsahovat jenom číslice a malá písmena.

```azurecli-interactive
az storage account create --name <storage_name> --location westeurope --resource-group myResourceGroup --sku Standard_LRS
```

Po vytvoření účtu úložiště hello hello rozhraní příkazového řádku Azure zobrazuje informace podobné toohello následující ukázka:

```json
{
  "creationTime": "2017-04-15T17:14:39.320307+00:00",
  "id": "/subscriptions/bbbef702-e769-477b-9f16-bc4d3aa97387/resourceGroups/myresourcegroup/...",
  "kind": "Storage",
  "location": "westeurope",
  "name": "myfunctionappstorage",
  "primaryEndpoints": {
    "blob": "https://myfunctionappstorage.blob.core.windows.net/",
    "file": "https://myfunctionappstorage.file.core.windows.net/",
    "queue": "https://myfunctionappstorage.queue.core.windows.net/",
    "table": "https://myfunctionappstorage.table.core.windows.net/"
  },
     ....
    // Remaining output has been truncated for readability.
}
```

## <a name="create-a-function-app"></a>Vytvoření Function App

Musíte mít funkce aplikace toohost hello provádění funkcí. funkce aplikace Hello poskytuje prostředí pro bez serveru provádění kódu funkce. Umožňuje seskupit funkce jako logickou jednotku pro snadnější správu, nasazování a sdílení prostředků. Vytvořit aplikaci function app pomocí hello [vytvořit az functionapp](/cli/azure/functionapp#create) příkaz. 

V hello následující příkaz, nahraďte název vlastní jedinečné funkce aplikace, kde uvidíte hello `<app_name>` zástupný symbol a hello název účtu úložiště pro `<storage_name>`. Hello `<app_name>` slouží jako hello výchozí doménu DNS hello funkce aplikace a Ano název hello musí toobe jedinečný mezi všechny aplikace v Azure. 

```azurecli-interactive
az functionapp create --name <app_name> --storage-account  <storage_name>  --resource-group myResourceGroup \
--consumption-plan-location westeurope
```
Ve výchozím nastavení je funkce aplikace vytvoří s plánem spotřeba hostování hello, což znamená, že se prostředky dynamicky podle požadavků vaší funkce přidají a platíte jenom při funkce fungují. Další informace najdete v tématu [zvolte hello správné hostingový plán](functions-scale.md). 

Po vytvoření aplikace hello funkce hello rozhraní příkazového řádku Azure zobrazuje informace podobné toohello následující ukázka:

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "containerSize": 1536,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "quickstart.azurewebsites.net",
  "enabled": true,
  "enabledHostNames": [
    "quickstart.azurewebsites.net",
    "quickstart.scm.azurewebsites.net"
  ],
   ....
    // Remaining output has been truncated for readability.
}
```

Teď, když máte aplikaci function app, můžete nasadit hello funkce skutečného kód z úložiště ukázkové hello GitHub.

## <a name="deploy-your-function-code"></a>Nasazení kódu funkce  

Existují několik způsobů toocreate funkce kódu v aplikaci nové funkce. Toto téma se připojí tooa Ukázka úložiště Githubu. Jako předtím v hello následující kód nahraďte hello `<app_name>` zástupný symbol hello název aplikace hello funkce jste vytvořili. 

```azurecli-interactive
az functionapp deployment source config --name <app_name> --resource-group myResourceGroup --branch master \
--repo-url https://github.com/Azure-Samples/functions-quickstart \
--manual-integration 
```
Po nasazení hello zdroj byl nastaven, hello rozhraní příkazového řádku Azure zobrazuje následující příklad (hodnoty null Odebrat čitelnější) podobné toohello informace:

```json
{
  "branch": "master",
  "deploymentRollbackEnabled": false,
  "id": "/subscriptions/bbbef702-e769-477b-9f16-bc4d3aa97387/resourceGroups/myResourceGroup/...",
  "isManualIntegration": true,
  "isMercurial": false,
  "location": "West Europe",
  "name": "quickstart",
  "repoUrl": "https://github.com/Azure-Samples/functions-quickstart",
  "resourceGroup": "myResourceGroup",
  "type": "Microsoft.Web/sites/sourcecontrols"
}
```

## <a name="test-hello-function"></a>Testování funkce hello

Funkci cURL tootest hello nasazený na počítači se systémem Mac nebo Linux nebo pomocí Bash v systému Windows. Spustit následující příkaz cURL, nahraďte hello hello `<app_name>` zástupný symbol hello název vaší aplikace funkce. Připojit řetězec dotazu hello `&name=<yourname>` toohello adresy URL.

```bash
curl http://<app_name>.azurewebsites.net/api/HttpTriggerJS1?name=<yourname>
```  

![Odpověď funkce se zobrazí v prohlížeči.](./media/functions-create-first-azure-function-azure-cli/functions-azure-cli-function-test-curl.png)  

Pokud nemáte cURL k dispozici v příkazovém řádku, zadejte text hello stejnou adresu URL v adrese hello webového prohlížeče. Znovu, nahraďte hello `<app_name>` zástupný symbol hello název funkce aplikace a připojte řetězec dotazu hello `&name=<yourname>` toohello adresy URL a provedení hello požadavku. 

    http://<app_name>.azurewebsites.net/api/HttpTriggerJS1?name=<yourname>
   
![Odpověď funkce se zobrazí v prohlížeči.](./media/functions-create-first-azure-function-azure-cli/functions-azure-cli-function-test-browser.png)  

## <a name="clean-up-resources"></a>Vyčištění prostředků

Další rychlé starty v této kolekci jsou postavené na tomto rychlém startu. Pokud máte v plánu toocontinue na toowork následné – elementy QuickStart nebo s hello kurzy, to není vyčištění hello prostředky vytvořené v tento rychlý start. Pokud neplánujete toocontinue, použijte následující příkaz toodelete hello všechny prostředky, které jsou vytvořené tento rychlý start:

```azurecli-interactive
az group delete --name myResourceGroup
```
Po zobrazení výzvy zadejte `y`.

## <a name="next-steps"></a>Další kroky

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]
