---
title: "aaaDeploy webovou aplikaci, která je propojená úložiště GitHub tooa | Microsoft Docs"
description: "Použijte toodeploy šablony Azure Resource Manager webovou aplikaci, která obsahuje projekt z úložiště Githubu."
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
ms.openlocfilehash: 8b23416c4c06a60991517e6ee4cd82bebc5a9d73
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-web-app-linked-tooa-github-repository"></a><span data-ttu-id="98472-103">Nasazení webové aplikace propojené tooa Githubu úložiště</span><span class="sxs-lookup"><span data-stu-id="98472-103">Deploy a web app linked tooa GitHub repository</span></span>
<span data-ttu-id="98472-104">V tomto tématu se dozvíte, jak toocreate šablonu Azure Resource Manager, která nasazuje webovou aplikaci, která je propojená tooa projekt v úložišti GitHub.</span><span class="sxs-lookup"><span data-stu-id="98472-104">In this topic, you will learn how toocreate an Azure Resource Manager template that deploys a web app that is linked tooa project in a GitHub repository.</span></span> <span data-ttu-id="98472-105">Se dozvíte, jak toodefine prostředky, ke kterým jsou nasazené a jak toodefine parametry, které jsou zadané, když se spustí nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="98472-105">You will learn how toodefine which resources are deployed and how toodefine parameters that are specified when hello deployment is executed.</span></span> <span data-ttu-id="98472-106">Můžete tuto šablonu použít pro vlastní nasazení, nebo si ji přizpůsobit toomeet vašim požadavkům.</span><span class="sxs-lookup"><span data-stu-id="98472-106">You can use this template for your own deployments, or customize it toomeet your requirements.</span></span>

<span data-ttu-id="98472-107">Další informace o vytváření šablon najdete v tématu [vytváření šablon Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="98472-107">For more information about creating templates, see [Authoring Azure Resource Manager Templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>

<span data-ttu-id="98472-108">Hello úplnou šablonu, najdete v části [šablony webové aplikace propojené tooGitHub](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-github-deploy/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="98472-108">For hello complete template, see [Web App Linked tooGitHub template](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-github-deploy/azuredeploy.json).</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="what-you-will-deploy"></a><span data-ttu-id="98472-109">Co budete nasazovat</span><span class="sxs-lookup"><span data-stu-id="98472-109">What you will deploy</span></span>
<span data-ttu-id="98472-110">Pomocí této šablony nasadíte webovou aplikaci, která obsahuje kód hello z projektu na Githubu.</span><span class="sxs-lookup"><span data-stu-id="98472-110">With this template, you will deploy a web app that contains hello code from a project in GitHub.</span></span>

<span data-ttu-id="98472-111">toorun hello nasazení automaticky, klikněte na následující tlačítko hello:</span><span class="sxs-lookup"><span data-stu-id="98472-111">toorun hello deployment automatically, click hello following button:</span></span>

<span data-ttu-id="98472-112">[![Nasazení tooAzure](./media/app-service-web-arm-from-github-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-github-deploy%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="98472-112">[![Deploy tooAzure](./media/app-service-web-arm-from-github-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-github-deploy%2Fazuredeploy.json)</span></span>

## <a name="parameters"></a><span data-ttu-id="98472-113">Parametry</span><span class="sxs-lookup"><span data-stu-id="98472-113">Parameters</span></span>
[!INCLUDE [app-service-web-deploy-web-parameters](../../includes/app-service-web-deploy-web-parameters.md)]

### <a name="repourl"></a><span data-ttu-id="98472-114">repoURL</span><span class="sxs-lookup"><span data-stu-id="98472-114">repoURL</span></span>
<span data-ttu-id="98472-115">Adresa URL Hello pro úložiště GitHub obsahující toodeploy projektu hello.</span><span class="sxs-lookup"><span data-stu-id="98472-115">hello URL for GitHub repository that contains hello project toodeploy.</span></span> <span data-ttu-id="98472-116">Tento parametr obsahuje výchozí hodnotu, ale tato hodnota je jenom zamýšlený tooshow můžete jak tooprovide hello adresu URL pro úložiště.</span><span class="sxs-lookup"><span data-stu-id="98472-116">This parameter contains a default value but this value is only intended tooshow you how tooprovide hello URL for repository.</span></span> <span data-ttu-id="98472-117">Tuto hodnotu můžete použít při testování hello šablony, ale bude vhodné tooprovide hello URL vlastního úložiště při práci s hello šablony.</span><span class="sxs-lookup"><span data-stu-id="98472-117">You can use this value when testing hello template but you will want tooprovide hello URL your own repository when working with hello template.</span></span>

    "repoURL": {
        "type": "string",
        "defaultValue": "https://github.com/davidebbo-test/Mvc52Application.git"
    }

### <a name="branch"></a><span data-ttu-id="98472-118">Firemní pobočky</span><span class="sxs-lookup"><span data-stu-id="98472-118">branch</span></span>
<span data-ttu-id="98472-119">větev Hello hello úložiště toouse při nasazování aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="98472-119">hello branch of hello repository toouse when deploying hello application.</span></span> <span data-ttu-id="98472-120">Hello výchozí hodnota je hlavní, ale můžete zadat název hello žádné větve v úložišti hello chcete toodeploy.</span><span class="sxs-lookup"><span data-stu-id="98472-120">hello default value is master, but you can provide hello name of any branch in hello repository that you wish toodeploy.</span></span>

    "branch": {
        "type": "string",
        "defaultValue": "master"
    }

## <a name="resources-toodeploy"></a><span data-ttu-id="98472-121">Toodeploy prostředky</span><span class="sxs-lookup"><span data-stu-id="98472-121">Resources toodeploy</span></span>
[!INCLUDE [app-service-web-deploy-web-host](../../includes/app-service-web-deploy-web-host.md)]

### <a name="web-app"></a><span data-ttu-id="98472-122">Webová aplikace</span><span class="sxs-lookup"><span data-stu-id="98472-122">Web app</span></span>
<span data-ttu-id="98472-123">Vytvoří hello webovou aplikaci, která je propojená toohello projektu na Githubu.</span><span class="sxs-lookup"><span data-stu-id="98472-123">Creates hello web app that is linked toohello project in GitHub.</span></span> 

<span data-ttu-id="98472-124">Zadejte název hello hello webové aplikace prostřednictvím hello **siteName** parametr a umístění hello hello webové aplikace prostřednictvím hello **siteLocation** parametr.</span><span class="sxs-lookup"><span data-stu-id="98472-124">You specify hello name of hello web app through hello **siteName** parameter, and hello location of hello web app through hello **siteLocation** parameter.</span></span> <span data-ttu-id="98472-125">V hello **dependsOn** elementu hello Šablona definuje hello webové aplikace jako závisí na službě hello hostování plánu.</span><span class="sxs-lookup"><span data-stu-id="98472-125">In hello **dependsOn** element, hello template defines hello web app as dependent on hello service hosting plan.</span></span> <span data-ttu-id="98472-126">Vzhledem k tomu, že je závislá na hello hostování plán, hello webové aplikace se nevytvoří, dokud nedokončí hello hostování plán vytváří.</span><span class="sxs-lookup"><span data-stu-id="98472-126">Because it is dependent on hello hosting plan, hello web app is not created until hello hosting plan has finished being created.</span></span> <span data-ttu-id="98472-127">Hello **dependsOn** element je pouze použité toospecify pořadí nasazení.</span><span class="sxs-lookup"><span data-stu-id="98472-127">hello **dependsOn** element is only used toospecify deployment order.</span></span> <span data-ttu-id="98472-128">Pokud webová aplikace hello jako závislé na hostování plán hello nezaškrtnete, Azure Resource Manager pokusí toocreate i prostředky v hello stejnou dobu a může se zobrazit chyba Pokud hello webové aplikace se vytvoří před hello hostování plánu.</span><span class="sxs-lookup"><span data-stu-id="98472-128">If you do not mark hello web app as dependent on hello hosting plan, Azure Resource Mananger will attempt toocreate both resources at hello same time and you may receive an error if hello web app is created before hello hosting plan.</span></span>

<span data-ttu-id="98472-129">Hello webové aplikace se také podřízený prostředek, který je definován v **prostředky** části níže.</span><span class="sxs-lookup"><span data-stu-id="98472-129">hello web app also has a child resource which is defined in **resources** section below.</span></span> <span data-ttu-id="98472-130">Tento podřízený prostředek definuje zdrojového kódu pro nasazení s hello webové aplikace projektu hello.</span><span class="sxs-lookup"><span data-stu-id="98472-130">This child resource defines source control for hello project deployed with hello web app.</span></span> <span data-ttu-id="98472-131">V této šabloně je hello zdrojového kódu propojené tooa konkrétní úložiště GitHub.</span><span class="sxs-lookup"><span data-stu-id="98472-131">In this template, hello source control is linked tooa particular GitHub repository.</span></span> <span data-ttu-id="98472-132">úložiště GitHub Hello je definován s kódem hello **"RepoUrl": "https://github.com/davidebbo-test/Mvc52Application.git"** může adresu URL úložiště pevně hello, pokud chcete toocreate šablonu, která opakovaně nasadí jeden projektu při vyžadování hello minimální počet parametrů.</span><span class="sxs-lookup"><span data-stu-id="98472-132">hello GitHub repository is defined with hello code **"RepoUrl":"https://github.com/davidebbo-test/Mvc52Application.git"** You might hard-code hello repository URL when you want toocreate a template that repeatedly deploys a single project while requiring hello minimum number of parameters.</span></span>
<span data-ttu-id="98472-133">Místo pevně kódováno hello adresu URL úložiště, můžete přidat parametr pro adresu URL úložiště hello a používat tuto hodnotu pro hello **RepoUrl** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="98472-133">Instead of hard-coding hello repository URL, you can add a parameter for hello repository URL and use that value for hello **RepoUrl** property.</span></span>

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

## <a name="commands-toorun-deployment"></a><span data-ttu-id="98472-134">Příkazy toorun nasazení</span><span class="sxs-lookup"><span data-stu-id="98472-134">Commands toorun deployment</span></span>
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a><span data-ttu-id="98472-135">PowerShell</span><span class="sxs-lookup"><span data-stu-id="98472-135">PowerShell</span></span>
    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-github-deploy/azuredeploy.json -siteName ExampleSite -hostingPlanName ExamplePlan -ResourceGroupName ExampleDeployGroup

### <a name="azure-cli"></a><span data-ttu-id="98472-136">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="98472-136">Azure CLI</span></span>

    azure group deployment create -g {resource-group-name} --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-github-deploy/azuredeploy.json

### <a name="azure-cli-20"></a><span data-ttu-id="98472-137">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="98472-137">Azure CLI 2.0</span></span>

    az group deployment create -g {resource-group-name} --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-github-deploy/azuredeploy.json --parameters '@azuredeploy.parameters.json'

> [!NOTE] 
> <span data-ttu-id="98472-138">Obsah souboru JSON hello parametry, najdete v části [azuredeploy.parameters.json](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-github-deploy/azuredeploy.parameters.json).</span><span class="sxs-lookup"><span data-stu-id="98472-138">For content of hello parameters JSON file, see [azuredeploy.parameters.json](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-github-deploy/azuredeploy.parameters.json).</span></span>
>
>

