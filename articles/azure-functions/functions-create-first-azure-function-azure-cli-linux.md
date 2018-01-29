---
title: "Vytvoření první funkce v Linuxu pomocí Azure CLI (Preview)| Dokumentace Microsoftu"
description: "Zjistěte, jak pomocí Azure CLI vytvořit první funkci Azure Functions spuštěnou ve výchozí imagi Linuxu."
services: functions
keywords: 
author: ggailey777
ms.author: glenga
ms.date: 11/15/2017
ms.topic: quickstart
ms.service: functions
ms.custom: mvc
ms.devlang: azure-cli
manager: cfowler
ms.openlocfilehash: d04e2000f2043e8bb11e15f6b9d7fd06ef5b9da3
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/16/2017
---
# <a name="create-your-first-function-running-on-linux-using-the-azure-cli-preview"></a>Vytvoření první funkce spuštěné v Linuxu pomocí Azure CLI (Preview)

Služba Azure Functions umožňuje hostovat funkce v Linuxu ve výchozím kontejneru Azure App Service. Tato funkce je aktuálně ve verzi Preview. Můžete také [použít vlastní kontejner](functions-create-function-linux-custom-image.md). 

Toto téma Rychlý start vás provede použitím služby Azure Functions s Azure CLI k vytvoření první aplikace funkcí v Linuxu hostované ve výchozím kontejneru App Service. Samotný kód funkce se do image nasadí z ukázkového úložiště GitHubu.    

Následující kroky se podporují na počítačích se systémem Mac, Windows a Linux. 

## <a name="prerequisites"></a>Požadavky 

K dokončení tohoto rychlého startu je potřeba:

+ Aktivní předplatné Azure.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku místně, musíte mít verzi Azure CLI 2.0.21 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 

[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-linux-app-service-plan"></a>Vytvoření plánu služby App Service pro Linux

Hostování služby Functions v Linuxu se aktuálně podporuje pouze v rámci plánu služby App Service. Hostování v rámci plánu Consumption se zatím nepodporuje. Další informace o hostování najdete v [porovnání plánů hostování služby Azure Functions](functions-scale.md). 

[!INCLUDE [app-service-plan-no-h](../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

## <a name="create-a-function-app-on-linux"></a>Vytvoření aplikace funkcí v Linuxu

K hostování provádění funkcí v Linuxu musíte mít aplikaci funkcí. Aplikace funkcí poskytuje prostředí pro provádění kódu funkce. Umožňuje seskupit funkce jako logickou jednotku pro snadnější správu, nasazování a sdílení prostředků. Aplikaci funkcí vytvoříte pomocí příkazu [az functionapp create](/cli/azure/functionapp#create) s použitím plánu služby App Service pro Linux. 

V následujícím příkazu nahraďte zástupný symbol `<app_name>` jedinečným názvem vaší aplikace funkcí a `<storage_name>` názvem účtu úložiště. Jako výchozí doména DNS pro příslušnou aplikaci Function App se použije `<app_name>`, a proto musí být název mezi všemi aplikacemi v Azure jedinečný. Parametr _deployment-source-url_ představuje ukázkové úložiště na GitHubu, které obsahuje funkci Hello World aktivovanou protokolem HTTP.

```azurecli-interactive
az functionapp create --name <app_name> --storage-account  <storage_name>  --resource-group myResourceGroup \
--plan myAppServicePlan --deployment-source-url https://github.com/Azure-Samples/functions-quickstart-linux
```
Po vytvoření a nasazení aplikace funkcí se v Azure CLI zobrazí podobné informace jako v následujícím příkladu:

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
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

Vzhledem k tomu, že `myAppServicePlan` je plán pro Linux, k vytvoření kontejneru, ve kterém se spouští aplikace funkcí v Linuxu, se použije integrovaná image Dockeru. 

>[!NOTE]  
>Ukázkové úložiště aktuálně obsahuje dva skriptovací soubory – [deploy.sh](https://github.com/Azure-Samples/functions-quickstart-linux/blob/master/deploy.sh) a [.deployment](https://github.com/Azure-Samples/functions-quickstart-linux/blob/master/.deployment). Soubor .deployment sděluje procesu nasazení, že se má jako [skript vlastního nasazení](https://github.com/projectkudu/kudu/wiki/Custom-Deployment-Script) použít soubor deploy.sh. V aktuální vydané verzi Preview se k nasazení aplikace funkcí do image Linuxu vyžadují skripty.  

[!INCLUDE [functions-test-function-code](../../includes/functions-test-function-code.md)]

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

[!INCLUDE [functions-quickstart-next-steps-cli](../../includes/functions-quickstart-next-steps-cli.md)]
