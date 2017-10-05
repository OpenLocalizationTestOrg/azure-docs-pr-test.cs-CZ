---
title: "Nasazení webové aplikace, která je propojený s úložišti GitHub | Microsoft Docs"
description: "Pomocí šablony Azure Resource Manager nasadit webovou aplikaci, která obsahuje projekt z úložiště Githubu."
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: 32739607-85fe-43c8-a4dc-1feb46d93a4d
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2016
ms.author: cephalin
ms.openlocfilehash: 77064802814296d0c21f004534e4264d2f97252e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-a-web-app-linked-to-a-github-repository"></a><span data-ttu-id="17230-103">Nasazení webové aplikace propojené s úložišti GitHub</span><span class="sxs-lookup"><span data-stu-id="17230-103">Deploy a web app linked to a GitHub repository</span></span>
<span data-ttu-id="17230-104">V tomto tématu se dozvíte, jak vytvořit šablonu Azure Resource Manager, nasadí webové aplikace, který je propojený na projekt v úložišti GitHub.</span><span class="sxs-lookup"><span data-stu-id="17230-104">In this topic, you will learn how to create an Azure Resource Manager template that deploys a web app that is linked to a project in a GitHub repository.</span></span> <span data-ttu-id="17230-105">Se dozvíte, jak definovat prostředky, ke kterým nasazených a jak definovat parametry, které jsou zadané, když se spustí nasazení.</span><span class="sxs-lookup"><span data-stu-id="17230-105">You will learn how to define which resources are deployed and how to define parameters that are specified when the deployment is executed.</span></span> <span data-ttu-id="17230-106">Tuto šablonu můžete použít pro vlastní nasazení nebo ji upravit, aby splňovala vaše požadavky.</span><span class="sxs-lookup"><span data-stu-id="17230-106">You can use this template for your own deployments, or customize it to meet your requirements.</span></span>

<span data-ttu-id="17230-107">Další informace o vytváření šablon najdete v tématu [vytváření šablon Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="17230-107">For more information about creating templates, see [Authoring Azure Resource Manager Templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>

<span data-ttu-id="17230-108">Pro úplnou šablonu, najdete v části [webové aplikace propojené do šablony Githubu](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-github-deploy/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="17230-108">For the complete template, see [Web App Linked to GitHub template](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-github-deploy/azuredeploy.json).</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="what-you-will-deploy"></a><span data-ttu-id="17230-109">Co budete nasazovat</span><span class="sxs-lookup"><span data-stu-id="17230-109">What you will deploy</span></span>
<span data-ttu-id="17230-110">Pomocí této šablony nasadíte webovou aplikaci, která obsahuje kód z projektu na Githubu.</span><span class="sxs-lookup"><span data-stu-id="17230-110">With this template, you will deploy a web app that contains the code from a project in GitHub.</span></span>

<span data-ttu-id="17230-111">Pokud chcete nasazení spustit automaticky, klikněte na následující tlačítko:</span><span class="sxs-lookup"><span data-stu-id="17230-111">To run the deployment automatically, click the following button:</span></span>

<span data-ttu-id="17230-112">[![Nasazení do Azure](./media/app-service-web-arm-from-github-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-github-deploy%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="17230-112">[![Deploy to Azure](./media/app-service-web-arm-from-github-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-github-deploy%2Fazuredeploy.json)</span></span>

## <a name="parameters"></a><span data-ttu-id="17230-113">Parametry</span><span class="sxs-lookup"><span data-stu-id="17230-113">Parameters</span></span>
[!INCLUDE [app-service-web-deploy-web-parameters](../../includes/app-service-web-deploy-web-parameters.md)]

### <a name="repourl"></a><span data-ttu-id="17230-114">repoURL</span><span class="sxs-lookup"><span data-stu-id="17230-114">repoURL</span></span>
<span data-ttu-id="17230-115">Adresa URL pro úložiště GitHub obsahující projekt nasadit.</span><span class="sxs-lookup"><span data-stu-id="17230-115">The URL for GitHub repository that contains the project to deploy.</span></span> <span data-ttu-id="17230-116">Tento parametr obsahuje výchozí hodnotu, ale tato hodnota je určena pouze k ukazují, jak můžete zadat adresu URL pro úložiště.</span><span class="sxs-lookup"><span data-stu-id="17230-116">This parameter contains a default value but this value is only intended to show you how to provide the URL for repository.</span></span> <span data-ttu-id="17230-117">Tuto hodnotu lze použít při testování šablony, ale můžete zadat adresu URL vlastního úložiště při práci se šablonou.</span><span class="sxs-lookup"><span data-stu-id="17230-117">You can use this value when testing the template but you will want to provide the URL your own repository when working with the template.</span></span>

    "repoURL": {
        "type": "string",
        "defaultValue": "https://github.com/davidebbo-test/Mvc52Application.git"
    }

### <a name="branch"></a><span data-ttu-id="17230-118">Firemní pobočky</span><span class="sxs-lookup"><span data-stu-id="17230-118">branch</span></span>
<span data-ttu-id="17230-119">Větev úložiště k použití při nasazení aplikace.</span><span class="sxs-lookup"><span data-stu-id="17230-119">The branch of the repository to use when deploying the application.</span></span> <span data-ttu-id="17230-120">Výchozí hodnota je hlavní, ale můžete zadat název žádné větve v úložišti, který chcete nasadit.</span><span class="sxs-lookup"><span data-stu-id="17230-120">The default value is master, but you can provide the name of any branch in the repository that you wish to deploy.</span></span>

    "branch": {
        "type": "string",
        "defaultValue": "master"
    }

## <a name="resources-to-deploy"></a><span data-ttu-id="17230-121">Prostředky k nasazení</span><span class="sxs-lookup"><span data-stu-id="17230-121">Resources to deploy</span></span>
[!INCLUDE [app-service-web-deploy-web-host](../../includes/app-service-web-deploy-web-host.md)]

### <a name="web-app"></a><span data-ttu-id="17230-122">Webová aplikace</span><span class="sxs-lookup"><span data-stu-id="17230-122">Web app</span></span>
<span data-ttu-id="17230-123">Vytvoří webovou aplikaci, která je propojená do projektu na Githubu.</span><span class="sxs-lookup"><span data-stu-id="17230-123">Creates the web app that is linked to the project in GitHub.</span></span> 

<span data-ttu-id="17230-124">Zadejte název webové aplikace prostřednictvím **siteName** parametr a umístění webové aplikace prostřednictvím **siteLocation** parametr.</span><span class="sxs-lookup"><span data-stu-id="17230-124">You specify the name of the web app through the **siteName** parameter, and the location of the web app through the **siteLocation** parameter.</span></span> <span data-ttu-id="17230-125">V **dependsOn** elementu, že šablona definuje webové aplikace jako závislé na službě hostování plánu.</span><span class="sxs-lookup"><span data-stu-id="17230-125">In the **dependsOn** element, the template defines the web app as dependent on the service hosting plan.</span></span> <span data-ttu-id="17230-126">Protože je závislá na hostování plán, webové aplikace se nevytvoří, dokud nedokončí hostingový plán vytváří.</span><span class="sxs-lookup"><span data-stu-id="17230-126">Because it is dependent on the hosting plan, the web app is not created until the hosting plan has finished being created.</span></span> <span data-ttu-id="17230-127">**DependsOn** element slouží pouze k určení pořadí nasazení.</span><span class="sxs-lookup"><span data-stu-id="17230-127">The **dependsOn** element is only used to specify deployment order.</span></span> <span data-ttu-id="17230-128">Pokud webová aplikace jako závislé na hostování plán nezaškrtnete, Azure Resource Manager se pokusí vytvořit i prostředky ve stejnou dobu a může se zobrazit chyba, pokud webová aplikace je vytvořen před hostingový plán.</span><span class="sxs-lookup"><span data-stu-id="17230-128">If you do not mark the web app as dependent on the hosting plan, Azure Resource Mananger will attempt to create both resources at the same time and you may receive an error if the web app is created before the hosting plan.</span></span>

<span data-ttu-id="17230-129">Webové aplikace má také podřízený prostředek, který je definován v **prostředky** části níže.</span><span class="sxs-lookup"><span data-stu-id="17230-129">The web app also has a child resource which is defined in **resources** section below.</span></span> <span data-ttu-id="17230-130">Tento podřízený prostředek definuje zdrojového kódu pro projekt nasazen s webovou aplikací.</span><span class="sxs-lookup"><span data-stu-id="17230-130">This child resource defines source control for the project deployed with the web app.</span></span> <span data-ttu-id="17230-131">V této šabloně se správa zdrojového kódu propojí konkrétní úložiště GitHub.</span><span class="sxs-lookup"><span data-stu-id="17230-131">In this template, the source control is linked to a particular GitHub repository.</span></span> <span data-ttu-id="17230-132">Úložiště GitHub je definován s kódem **"RepoUrl": "https://github.com/davidebbo-test/Mvc52Application.git"** pravděpodobně pevně adresu URL úložiště Pokud chcete vytvořit šablonu, která opakovaně nasadí jedné Projekt současně vyžaduje minimální počet parametrů.</span><span class="sxs-lookup"><span data-stu-id="17230-132">The GitHub repository is defined with the code **"RepoUrl":"https://github.com/davidebbo-test/Mvc52Application.git"** You might hard-code the repository URL when you want to create a template that repeatedly deploys a single project while requiring the minimum number of parameters.</span></span>
<span data-ttu-id="17230-133">Místo pevně kódováno adresu URL úložiště, můžete přidat parametr pro adresu URL úložiště a používat pro tuto hodnotu **RepoUrl** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="17230-133">Instead of hard-coding the repository URL, you can add a parameter for the repository URL and use that value for the **RepoUrl** property.</span></span>

    {
      "apiVersion": "2015-08-01",
      "name": "[parameters('siteName')]",
      "type": "Microsoft.Web/sites",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[resourceId('Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
      ],
      "properties": {
        "serverFarmId": "[parameters('hostingPlanName')]"
      },
      "resources": [
        {
          "apiVersion": "2015-08-01",
          "name": "web",
          "type": "sourcecontrols",
          "dependsOn": [
            "[resourceId('Microsoft.Web/Sites', parameters('siteName'))]"
          ],
          "properties": {
            "RepoUrl": "[parameters('repoURL')]",
            "branch": "[parameters('branch')]",
            "IsManualIntegration": true
          }
        }
      ]
    }

## <a name="commands-to-run-deployment"></a><span data-ttu-id="17230-134">Příkazy pro spuštění nasazení</span><span class="sxs-lookup"><span data-stu-id="17230-134">Commands to run deployment</span></span>
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a><span data-ttu-id="17230-135">PowerShell</span><span class="sxs-lookup"><span data-stu-id="17230-135">PowerShell</span></span>
    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-github-deploy/azuredeploy.json -siteName ExampleSite -hostingPlanName ExamplePlan -ResourceGroupName ExampleDeployGroup

### <a name="azure-cli"></a><span data-ttu-id="17230-136">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="17230-136">Azure CLI</span></span>

    azure group deployment create -g {resource-group-name} --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-github-deploy/azuredeploy.json

### <a name="azure-cli-20"></a><span data-ttu-id="17230-137">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="17230-137">Azure CLI 2.0</span></span>

    az group deployment create -g {resource-group-name} --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-github-deploy/azuredeploy.json --parameters '@azuredeploy.parameters.json'

> [!NOTE] 
> <span data-ttu-id="17230-138">Obsah souboru JSON parametry, najdete v části [azuredeploy.parameters.json](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-github-deploy/azuredeploy.parameters.json).</span><span class="sxs-lookup"><span data-stu-id="17230-138">For content of the parameters JSON file, see [azuredeploy.parameters.json](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-github-deploy/azuredeploy.parameters.json).</span></span>
>
>

