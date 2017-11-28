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
# <a name="create-your-first-function-using-hello-azure-cli"></a><span data-ttu-id="902c9-103">Vytvoření první funkce pomocí hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="902c9-103">Create your first function using hello Azure CLI</span></span>

<span data-ttu-id="902c9-104">Tento rychlý úvodní kurz vás provede jak toouse Azure Functions toocreate svoji první funkci.</span><span class="sxs-lookup"><span data-stu-id="902c9-104">This quickstart tutorial walks through how toouse Azure Functions toocreate your first function.</span></span> <span data-ttu-id="902c9-105">Můžete použít rozhraní příkazového řádku Azure toocreate hello funkce aplikace, která je hello bez serveru infrastruktury, který je hostitelem funkce.</span><span class="sxs-lookup"><span data-stu-id="902c9-105">You use hello Azure CLI toocreate a function app, which is hello serverless infrastructure that hosts your function.</span></span> <span data-ttu-id="902c9-106">samotný kód Funkce Hello je nasazena z úložiště Githubu ukázka.</span><span class="sxs-lookup"><span data-stu-id="902c9-106">hello function code itself is deployed from a GitHub sample repository.</span></span>    

<span data-ttu-id="902c9-107">Můžete provést kroky hello níže použití počítače Mac, Windows nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="902c9-107">You can follow hello steps below using a Mac, Windows, or Linux computer.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="902c9-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="902c9-108">Prerequisites</span></span> 

<span data-ttu-id="902c9-109">Před spuštěním této ukázce, musíte mít následující hello:</span><span class="sxs-lookup"><span data-stu-id="902c9-109">Before running this sample, you must have hello following:</span></span>

+ <span data-ttu-id="902c9-110">Aktivní účet [GitHub](https://github.com)</span><span class="sxs-lookup"><span data-stu-id="902c9-110">An active [GitHub](https://github.com) account.</span></span> 
+ <span data-ttu-id="902c9-111">Aktivní předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="902c9-111">An active Azure subscription.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="902c9-112">Pokud zvolte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto tématu vyžaduje, že používáte hello Azure CLI verze 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="902c9-112">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="902c9-113">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="902c9-113">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="902c9-114">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="902c9-114">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 


## <a name="create-a-resource-group"></a><span data-ttu-id="902c9-115">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="902c9-115">Create a resource group</span></span>

<span data-ttu-id="902c9-116">Vytvořte skupinu prostředků s hello [vytvořit skupinu az](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="902c9-116">Create a resource group with hello [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="902c9-117">Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure, jako například aplikace Function App, databáze a účty úložiště.</span><span class="sxs-lookup"><span data-stu-id="902c9-117">An Azure resource group is a logical container into which Azure resources like function apps, databases, and storage accounts are deployed and managed.</span></span>

<span data-ttu-id="902c9-118">Hello následující příklad vytvoří skupinu prostředků s názvem `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="902c9-118">hello following example creates a resource group named `myResourceGroup`.</span></span>  
<span data-ttu-id="902c9-119">Pokud nepoužíváte cloudové prostředí, musíte se nejdřív přihlásit pomocí `az login`.</span><span class="sxs-lookup"><span data-stu-id="902c9-119">If you are not using Cloud Shell, you must first sign in using `az login`.</span></span>

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```


## <a name="create-an-azure-storage-account"></a><span data-ttu-id="902c9-120">Vytvoření účtu služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="902c9-120">Create an Azure Storage account</span></span>

<span data-ttu-id="902c9-121">Funkce používá stav toomaintain účtu úložiště Azure a další informace o funkcí.</span><span class="sxs-lookup"><span data-stu-id="902c9-121">Functions uses an Azure Storage account toomaintain state and other information about your functions.</span></span> <span data-ttu-id="902c9-122">Vytvořit účet úložiště ve skupině prostředků hello jste vytvořili pomocí hello [vytvořit účet úložiště az](/cli/azure/storage/account#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="902c9-122">Create a storage account in hello resource group you created by using hello [az storage account create](/cli/azure/storage/account#create) command.</span></span>

<span data-ttu-id="902c9-123">V hello následující příkaz, nahraďte váš vlastní název účtu globálně jedinečný úložiště, kde uvidíte hello `<storage_name>` zástupný symbol.</span><span class="sxs-lookup"><span data-stu-id="902c9-123">In hello following command, substitute your own globally unique storage account name where you see hello `<storage_name>` placeholder.</span></span> <span data-ttu-id="902c9-124">Názvy účtů úložiště musí mít od 3 do 24 znaků a můžou obsahovat jenom číslice a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="902c9-124">Storage account names must be between 3 and 24 characters in length and may contain numbers and lowercase letters only.</span></span>

```azurecli-interactive
az storage account create --name <storage_name> --location westeurope --resource-group myResourceGroup --sku Standard_LRS
```

<span data-ttu-id="902c9-125">Po vytvoření účtu úložiště hello hello rozhraní příkazového řádku Azure zobrazuje informace podobné toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="902c9-125">After hello storage account has been created, hello Azure CLI shows information similar toohello following example:</span></span>

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

## <a name="create-a-function-app"></a><span data-ttu-id="902c9-126">Vytvoření Function App</span><span class="sxs-lookup"><span data-stu-id="902c9-126">Create a function app</span></span>

<span data-ttu-id="902c9-127">Musíte mít funkce aplikace toohost hello provádění funkcí.</span><span class="sxs-lookup"><span data-stu-id="902c9-127">You must have a function app toohost hello execution of your functions.</span></span> <span data-ttu-id="902c9-128">funkce aplikace Hello poskytuje prostředí pro bez serveru provádění kódu funkce.</span><span class="sxs-lookup"><span data-stu-id="902c9-128">hello function app provides an environment for serverless execution of your function code.</span></span> <span data-ttu-id="902c9-129">Umožňuje seskupit funkce jako logickou jednotku pro snadnější správu, nasazování a sdílení prostředků.</span><span class="sxs-lookup"><span data-stu-id="902c9-129">It lets you group functions as a logic unit for easier management, deployment, and sharing of resources.</span></span> <span data-ttu-id="902c9-130">Vytvořit aplikaci function app pomocí hello [vytvořit az functionapp](/cli/azure/functionapp#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="902c9-130">Create a function app by using hello [az functionapp create](/cli/azure/functionapp#create) command.</span></span> 

<span data-ttu-id="902c9-131">V hello následující příkaz, nahraďte název vlastní jedinečné funkce aplikace, kde uvidíte hello `<app_name>` zástupný symbol a hello název účtu úložiště pro `<storage_name>`.</span><span class="sxs-lookup"><span data-stu-id="902c9-131">In hello following command, substitute your own unique function app name where you see hello `<app_name>` placeholder and hello storage account name for  `<storage_name>`.</span></span> <span data-ttu-id="902c9-132">Hello `<app_name>` slouží jako hello výchozí doménu DNS hello funkce aplikace a Ano název hello musí toobe jedinečný mezi všechny aplikace v Azure.</span><span class="sxs-lookup"><span data-stu-id="902c9-132">hello `<app_name>` is used as hello default DNS domain for hello function app, and so hello name needs toobe unique across all apps in Azure.</span></span> 

```azurecli-interactive
az functionapp create --name <app_name> --storage-account  <storage_name>  --resource-group myResourceGroup \
--consumption-plan-location westeurope
```
<span data-ttu-id="902c9-133">Ve výchozím nastavení je funkce aplikace vytvoří s plánem spotřeba hostování hello, což znamená, že se prostředky dynamicky podle požadavků vaší funkce přidají a platíte jenom při funkce fungují.</span><span class="sxs-lookup"><span data-stu-id="902c9-133">By default, a function app is created with hello Consumption hosting plan, which means that resources are added dynamically as required by your functions and you only pay when functions are running.</span></span> <span data-ttu-id="902c9-134">Další informace najdete v tématu [zvolte hello správné hostingový plán](functions-scale.md).</span><span class="sxs-lookup"><span data-stu-id="902c9-134">For more information, see [Choose hello correct hosting plan](functions-scale.md).</span></span> 

<span data-ttu-id="902c9-135">Po vytvoření aplikace hello funkce hello rozhraní příkazového řádku Azure zobrazuje informace podobné toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="902c9-135">After hello function app has been created, hello Azure CLI shows information similar toohello following example:</span></span>

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

<span data-ttu-id="902c9-136">Teď, když máte aplikaci function app, můžete nasadit hello funkce skutečného kód z úložiště ukázkové hello GitHub.</span><span class="sxs-lookup"><span data-stu-id="902c9-136">Now that you have a function app, you can deploy hello actual function code from hello GitHub sample repository.</span></span>

## <a name="deploy-your-function-code"></a><span data-ttu-id="902c9-137">Nasazení kódu funkce</span><span class="sxs-lookup"><span data-stu-id="902c9-137">Deploy your function code</span></span>  

<span data-ttu-id="902c9-138">Existují několik způsobů toocreate funkce kódu v aplikaci nové funkce.</span><span class="sxs-lookup"><span data-stu-id="902c9-138">There are several ways toocreate your function code in your new function app.</span></span> <span data-ttu-id="902c9-139">Toto téma se připojí tooa Ukázka úložiště Githubu.</span><span class="sxs-lookup"><span data-stu-id="902c9-139">This topic connects tooa sample repository in GitHub.</span></span> <span data-ttu-id="902c9-140">Jako předtím v hello následující kód nahraďte hello `<app_name>` zástupný symbol hello název aplikace hello funkce jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="902c9-140">As before, in hello following code replace hello `<app_name>` placeholder with hello name of hello function app you created.</span></span> 

```azurecli-interactive
az functionapp deployment source config --name <app_name> --resource-group myResourceGroup --branch master \
--repo-url https://github.com/Azure-Samples/functions-quickstart \
--manual-integration 
```
<span data-ttu-id="902c9-141">Po nasazení hello zdroj byl nastaven, hello rozhraní příkazového řádku Azure zobrazuje následující příklad (hodnoty null Odebrat čitelnější) podobné toohello informace:</span><span class="sxs-lookup"><span data-stu-id="902c9-141">After hello deployment source been set, hello Azure CLI shows information similar toohello following example (null values removed for readability):</span></span>

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

## <a name="test-hello-function"></a><span data-ttu-id="902c9-142">Testování funkce hello</span><span class="sxs-lookup"><span data-stu-id="902c9-142">Test hello function</span></span>

<span data-ttu-id="902c9-143">Funkci cURL tootest hello nasazený na počítači se systémem Mac nebo Linux nebo pomocí Bash v systému Windows.</span><span class="sxs-lookup"><span data-stu-id="902c9-143">Use cURL tootest hello deployed function on a Mac or Linux computer or using Bash on Windows.</span></span> <span data-ttu-id="902c9-144">Spustit následující příkaz cURL, nahraďte hello hello `<app_name>` zástupný symbol hello název vaší aplikace funkce.</span><span class="sxs-lookup"><span data-stu-id="902c9-144">Execute hello following cURL command, replacing hello `<app_name>` placeholder with hello name of your function app.</span></span> <span data-ttu-id="902c9-145">Připojit řetězec dotazu hello `&name=<yourname>` toohello adresy URL.</span><span class="sxs-lookup"><span data-stu-id="902c9-145">Append hello query string `&name=<yourname>` toohello URL.</span></span>

```bash
curl http://<app_name>.azurewebsites.net/api/HttpTriggerJS1?name=<yourname>
```  

![Odpověď funkce se zobrazí v prohlížeči.](./media/functions-create-first-azure-function-azure-cli/functions-azure-cli-function-test-curl.png)  

<span data-ttu-id="902c9-147">Pokud nemáte cURL k dispozici v příkazovém řádku, zadejte text hello stejnou adresu URL v adrese hello webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="902c9-147">If you don't have cURL available in your command line, enter hello same URL in hello address of your web browser.</span></span> <span data-ttu-id="902c9-148">Znovu, nahraďte hello `<app_name>` zástupný symbol hello název funkce aplikace a připojte řetězec dotazu hello `&name=<yourname>` toohello adresy URL a provedení hello požadavku.</span><span class="sxs-lookup"><span data-stu-id="902c9-148">Again, replace hello `<app_name>` placeholder with hello name of your function app, and append hello query string `&name=<yourname>` toohello URL and execute hello request.</span></span> 

    http://<app_name>.azurewebsites.net/api/HttpTriggerJS1?name=<yourname>
   
![Odpověď funkce se zobrazí v prohlížeči.](./media/functions-create-first-azure-function-azure-cli/functions-azure-cli-function-test-browser.png)  

## <a name="clean-up-resources"></a><span data-ttu-id="902c9-150">Vyčištění prostředků</span><span class="sxs-lookup"><span data-stu-id="902c9-150">Clean up resources</span></span>

<span data-ttu-id="902c9-151">Další rychlé starty v této kolekci jsou postavené na tomto rychlém startu.</span><span class="sxs-lookup"><span data-stu-id="902c9-151">Other quickstarts in this collection build upon this quickstart.</span></span> <span data-ttu-id="902c9-152">Pokud máte v plánu toocontinue na toowork následné – elementy QuickStart nebo s hello kurzy, to není vyčištění hello prostředky vytvořené v tento rychlý start.</span><span class="sxs-lookup"><span data-stu-id="902c9-152">If you plan toocontinue on toowork with subsequent quickstarts or with hello tutorials, do not clean up hello resources created in this quickstart.</span></span> <span data-ttu-id="902c9-153">Pokud neplánujete toocontinue, použijte následující příkaz toodelete hello všechny prostředky, které jsou vytvořené tento rychlý start:</span><span class="sxs-lookup"><span data-stu-id="902c9-153">If you do not plan toocontinue, use hello following command toodelete all resources created by this quickstart:</span></span>

```azurecli-interactive
az group delete --name myResourceGroup
```
<span data-ttu-id="902c9-154">Po zobrazení výzvy zadejte `y`.</span><span class="sxs-lookup"><span data-stu-id="902c9-154">Type `y` when prompted.</span></span>

## <a name="next-steps"></a><span data-ttu-id="902c9-155">Další kroky</span><span class="sxs-lookup"><span data-stu-id="902c9-155">Next steps</span></span>

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]
