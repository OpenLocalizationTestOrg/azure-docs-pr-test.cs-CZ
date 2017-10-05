---
title: "Vytvoření první funkce z rozhraní Azure CLI| Dokumentace Microsoftu"
description: "Naučíte se postup vytvoření první funkce Azure Function pro provádění pomocí Azure CLI bez serveru."
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
ms.openlocfilehash: 8bd3e4bb7423db44c48b04f25edcf1074e6ea0bd
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-your-first-function-using-the-azure-cli"></a><span data-ttu-id="a6010-103">Vytvoření první funkce pomocí Azure CLI</span><span class="sxs-lookup"><span data-stu-id="a6010-103">Create your first function using the Azure CLI</span></span>

<span data-ttu-id="a6010-104">Tento rychlý start vám ukáže, jak používat Azure Functions k vytvoření první funkce.</span><span class="sxs-lookup"><span data-stu-id="a6010-104">This quickstart tutorial walks through how to use Azure Functions to create your first function.</span></span> <span data-ttu-id="a6010-105">Pomocí Azure CLI vytvoříte aplikaci Function App, což je infrastruktura bez serveru, která je hostitelem funkce.</span><span class="sxs-lookup"><span data-stu-id="a6010-105">You use the Azure CLI to create a function app, which is the serverless infrastructure that hosts your function.</span></span> <span data-ttu-id="a6010-106">Samotný kód funkce se nasadí z ukázkového úložiště Githubu.</span><span class="sxs-lookup"><span data-stu-id="a6010-106">The function code itself is deployed from a GitHub sample repository.</span></span>    

<span data-ttu-id="a6010-107">Následující kroky můžete provést v počítačích se systémem Mac, Windows nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="a6010-107">You can follow the steps below using a Mac, Windows, or Linux computer.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="a6010-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a6010-108">Prerequisites</span></span> 

<span data-ttu-id="a6010-109">Před spuštěním této ukázky musíte mít následující:</span><span class="sxs-lookup"><span data-stu-id="a6010-109">Before running this sample, you must have the following:</span></span>

+ <span data-ttu-id="a6010-110">Aktivní účet [GitHub](https://github.com)</span><span class="sxs-lookup"><span data-stu-id="a6010-110">An active [GitHub](https://github.com) account.</span></span> 
+ <span data-ttu-id="a6010-111">Aktivní předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="a6010-111">An active Azure subscription.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="a6010-112">Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="a6010-112">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="a6010-113">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="a6010-113">Run `az --version` to find the version.</span></span> <span data-ttu-id="a6010-114">Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="a6010-114">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 


## <a name="create-a-resource-group"></a><span data-ttu-id="a6010-115">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="a6010-115">Create a resource group</span></span>

<span data-ttu-id="a6010-116">Vytvořte skupinu prostředků pomocí příkazu [az group create](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="a6010-116">Create a resource group with the [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="a6010-117">Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure, jako například aplikace Function App, databáze a účty úložiště.</span><span class="sxs-lookup"><span data-stu-id="a6010-117">An Azure resource group is a logical container into which Azure resources like function apps, databases, and storage accounts are deployed and managed.</span></span>

<span data-ttu-id="a6010-118">Následující příklad vytvoří skupinu prostředků s názvem `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="a6010-118">The following example creates a resource group named `myResourceGroup`.</span></span>  
<span data-ttu-id="a6010-119">Pokud nepoužíváte cloudové prostředí, musíte se nejdřív přihlásit pomocí `az login`.</span><span class="sxs-lookup"><span data-stu-id="a6010-119">If you are not using Cloud Shell, you must first sign in using `az login`.</span></span>

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```


## <a name="create-an-azure-storage-account"></a><span data-ttu-id="a6010-120">Vytvoření účtu služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="a6010-120">Create an Azure Storage account</span></span>

<span data-ttu-id="a6010-121">Aplikace Functions používá účet Azure Storage k zachování stavu a dalších informací o vašich funkcích.</span><span class="sxs-lookup"><span data-stu-id="a6010-121">Functions uses an Azure Storage account to maintain state and other information about your functions.</span></span> <span data-ttu-id="a6010-122">Ve skupině prostředků, kterou jste vytvořili vytvořte účet úložiště pomocí příkazu [az storage account create](/cli/azure/storage/account#create).</span><span class="sxs-lookup"><span data-stu-id="a6010-122">Create a storage account in the resource group you created by using the [az storage account create](/cli/azure/storage/account#create) command.</span></span>

<span data-ttu-id="a6010-123">V následujícím příkazu nahraďte zástupný symbol `<storage_name>` vlastním globálním jedinečným názvem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="a6010-123">In the following command, substitute your own globally unique storage account name where you see the `<storage_name>` placeholder.</span></span> <span data-ttu-id="a6010-124">Názvy účtů úložiště musí mít od 3 do 24 znaků a můžou obsahovat jenom číslice a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="a6010-124">Storage account names must be between 3 and 24 characters in length and may contain numbers and lowercase letters only.</span></span>

```azurecli-interactive
az storage account create --name <storage_name> --location westeurope --resource-group myResourceGroup --sku Standard_LRS
```

<span data-ttu-id="a6010-125">Po vytvoření účtu úložiště se v rozhraní Azure CLI zobrazí podobné informace jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="a6010-125">After the storage account has been created, the Azure CLI shows information similar to the following example:</span></span>

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

## <a name="create-a-function-app"></a><span data-ttu-id="a6010-126">Vytvoření Function App</span><span class="sxs-lookup"><span data-stu-id="a6010-126">Create a function app</span></span>

<span data-ttu-id="a6010-127">K hostování provádění funkcí musíte mít aplikaci Function App.</span><span class="sxs-lookup"><span data-stu-id="a6010-127">You must have a function app to host the execution of your functions.</span></span> <span data-ttu-id="a6010-128">Function App poskytuje prostředí pro provádění kódu funkce bez serveru.</span><span class="sxs-lookup"><span data-stu-id="a6010-128">The function app provides an environment for serverless execution of your function code.</span></span> <span data-ttu-id="a6010-129">Umožňuje seskupit funkce jako logickou jednotku pro snadnější správu, nasazování a sdílení prostředků.</span><span class="sxs-lookup"><span data-stu-id="a6010-129">It lets you group functions as a logic unit for easier management, deployment, and sharing of resources.</span></span> <span data-ttu-id="a6010-130">Aplikaci Function App vytvoříte pomocí příkazu [az functionapp create](/cli/azure/functionapp#create).</span><span class="sxs-lookup"><span data-stu-id="a6010-130">Create a function app by using the [az functionapp create](/cli/azure/functionapp#create) command.</span></span> 

<span data-ttu-id="a6010-131">V následujícím příkazu nahraďte zástupný symbol `<app_name>` a účet úložiště pro `<storage_name>` vlastním jedinečným názvem aplikace Function App.</span><span class="sxs-lookup"><span data-stu-id="a6010-131">In the following command, substitute your own unique function app name where you see the `<app_name>` placeholder and the storage account name for  `<storage_name>`.</span></span> <span data-ttu-id="a6010-132">Jako výchozí doména DNS pro příslušnou aplikaci Function App se použije `<app_name>`, a proto musí být název mezi všemi aplikacemi v Azure jedinečný.</span><span class="sxs-lookup"><span data-stu-id="a6010-132">The `<app_name>` is used as the default DNS domain for the function app, and so the name needs to be unique across all apps in Azure.</span></span> 

```azurecli-interactive
az functionapp create --name <app_name> --storage-account  <storage_name>  --resource-group myResourceGroup \
--consumption-plan-location westeurope
```
<span data-ttu-id="a6010-133">Ve výchozím nastavení se aplikace Function App vytvoří s plánem hostování Consumption, což znamená, že se prostředky přidávají dynamicky podle požadavků vašich funkcí a platíte, jenom když funkce běží.</span><span class="sxs-lookup"><span data-stu-id="a6010-133">By default, a function app is created with the Consumption hosting plan, which means that resources are added dynamically as required by your functions and you only pay when functions are running.</span></span> <span data-ttu-id="a6010-134">Další informace najdete v tématu [Výběr správného plánu hostování](functions-scale.md).</span><span class="sxs-lookup"><span data-stu-id="a6010-134">For more information, see [Choose the correct hosting plan](functions-scale.md).</span></span> 

<span data-ttu-id="a6010-135">Po vytvoření aplikace Function App se v Azure CLI zobrazí podobné informace jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="a6010-135">After the function app has been created, the Azure CLI shows information similar to the following example:</span></span>

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

<span data-ttu-id="a6010-136">Teď, když máte aplikaci Function App, můžete nasadit samotný kód funkce z ukázkového úložiště GitHub.</span><span class="sxs-lookup"><span data-stu-id="a6010-136">Now that you have a function app, you can deploy the actual function code from the GitHub sample repository.</span></span>

## <a name="deploy-your-function-code"></a><span data-ttu-id="a6010-137">Nasazení kódu funkce</span><span class="sxs-lookup"><span data-stu-id="a6010-137">Deploy your function code</span></span>  

<span data-ttu-id="a6010-138">Existuje několik způsobů vytvoření kódu funkce v nové aplikaci Function App.</span><span class="sxs-lookup"><span data-stu-id="a6010-138">There are several ways to create your function code in your new function app.</span></span> <span data-ttu-id="a6010-139">V tomto tématu se připojíte k ukázkovému úložišti v GitHubu.</span><span class="sxs-lookup"><span data-stu-id="a6010-139">This topic connects to a sample repository in GitHub.</span></span> <span data-ttu-id="a6010-140">Tak jako předtím nahraďte v následujícím kódu zástupný symbol `<app_name>` názvem aplikace Function App, který jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="a6010-140">As before, in the following code replace the `<app_name>` placeholder with the name of the function app you created.</span></span> 

```azurecli-interactive
az functionapp deployment source config --name <app_name> --resource-group myResourceGroup --branch master \
--repo-url https://github.com/Azure-Samples/functions-quickstart \
--manual-integration 
```
<span data-ttu-id="a6010-141">Po nastavení zdroje nasazení zobrazí Azure CLI informace podobně jako v následujícím příkladu (hodnoty null byly odebrány pro zachování přehlednosti):</span><span class="sxs-lookup"><span data-stu-id="a6010-141">After the deployment source been set, the Azure CLI shows information similar to the following example (null values removed for readability):</span></span>

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

## <a name="test-the-function"></a><span data-ttu-id="a6010-142">Testování funkce</span><span class="sxs-lookup"><span data-stu-id="a6010-142">Test the function</span></span>

<span data-ttu-id="a6010-143">Na počítačích se systémem Mac nebo Linux otestujete nasazenou funkci pomocí cURL nebo v systému Windows pomocí skriptu Bash.</span><span class="sxs-lookup"><span data-stu-id="a6010-143">Use cURL to test the deployed function on a Mac or Linux computer or using Bash on Windows.</span></span> <span data-ttu-id="a6010-144">Proveďte následující příkaz cURL, který nahradí zástupný symbol `<app_name>` názvem vaší aplikace Function App.</span><span class="sxs-lookup"><span data-stu-id="a6010-144">Execute the following cURL command, replacing the `<app_name>` placeholder with the name of your function app.</span></span> <span data-ttu-id="a6010-145">Připojte řetězec dotazu `&name=<yourname>` k adrese URL.</span><span class="sxs-lookup"><span data-stu-id="a6010-145">Append the query string `&name=<yourname>` to the URL.</span></span>

```bash
curl http://<app_name>.azurewebsites.net/api/HttpTriggerJS1?name=<yourname>
```  

![Odpověď funkce se zobrazí v prohlížeči.](./media/functions-create-first-azure-function-azure-cli/functions-azure-cli-function-test-curl.png)  

<span data-ttu-id="a6010-147">Pokud nemáte cURL k dispozici v příkazovém řádku, zadejte stejnou adresu URL do pole adresy ve webovém prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="a6010-147">If you don't have cURL available in your command line, enter the same URL in the address of your web browser.</span></span> <span data-ttu-id="a6010-148">Znovu nahraďte zástupný symbol `<app_name>` názvem aplikace Function App, připojte řetězec dotazu `&name=<yourname>` k adrese URL a proveďte požadavek.</span><span class="sxs-lookup"><span data-stu-id="a6010-148">Again, replace the `<app_name>` placeholder with the name of your function app, and append the query string `&name=<yourname>` to the URL and execute the request.</span></span> 

    http://<app_name>.azurewebsites.net/api/HttpTriggerJS1?name=<yourname>
   
![Odpověď funkce se zobrazí v prohlížeči.](./media/functions-create-first-azure-function-azure-cli/functions-azure-cli-function-test-browser.png)  

## <a name="clean-up-resources"></a><span data-ttu-id="a6010-150">Vyčištění prostředků</span><span class="sxs-lookup"><span data-stu-id="a6010-150">Clean up resources</span></span>

<span data-ttu-id="a6010-151">Další rychlé starty v této kolekci jsou postavené na tomto rychlém startu.</span><span class="sxs-lookup"><span data-stu-id="a6010-151">Other quickstarts in this collection build upon this quickstart.</span></span> <span data-ttu-id="a6010-152">Pokud chcete pokračovat v práci s dalšími rychlými starty nebo kurzy, nevyčišťujte prostředky vytvořené v rámci tohoto rychlého startu.</span><span class="sxs-lookup"><span data-stu-id="a6010-152">If you plan to continue on to work with subsequent quickstarts or with the tutorials, do not clean up the resources created in this quickstart.</span></span> <span data-ttu-id="a6010-153">Pokud pokračovat nechcete, pomocí následujícího příkazu odstraňte všechny prostředky vytvořené tímto rychlým startem:</span><span class="sxs-lookup"><span data-stu-id="a6010-153">If you do not plan to continue, use the following command to delete all resources created by this quickstart:</span></span>

```azurecli-interactive
az group delete --name myResourceGroup
```
<span data-ttu-id="a6010-154">Po zobrazení výzvy zadejte `y`.</span><span class="sxs-lookup"><span data-stu-id="a6010-154">Type `y` when prompted.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a6010-155">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a6010-155">Next steps</span></span>

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]
