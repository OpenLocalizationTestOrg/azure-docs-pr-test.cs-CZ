---
title: "Nasazení webové aplikace pomocí MSDeploy certifikátem název hostitele a ssl"
description: "Nasazení webové aplikace pomocí MSDeploy a nastavení vlastního názvu hostitele a certifikátu SSL pomocí šablony Azure Resource Manager"
services: app-service\web
manager: erikre
documentationcenter: 
author: jodehavi
ms.assetid: 66366a72-cef7-4d75-8779-f4d32ed33cf7
ms.service: app-service-web
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/31/2016
ms.author: jodehavi
ms.openlocfilehash: a0e944d0d74ecb72a919538d54db330cbbdeef64
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="deploy-a-web-app-with-msdeploy-custom-hostname-and-ssl-certificate"></a><span data-ttu-id="5aed5-103">Nasazení webové aplikace s MSDeploy, vlastní název hostitele a certifikátu SSL</span><span class="sxs-lookup"><span data-stu-id="5aed5-103">Deploy a web app with MSDeploy, custom hostname and SSL certificate</span></span>
<span data-ttu-id="5aed5-104">Tento průvodce vás provede vytváření nasazení začátku do konce pro webové aplikace Azure, využití MSDeploy a také přidání vlastního názvu hostitele a certifikát SSL pro šablony ARM.</span><span class="sxs-lookup"><span data-stu-id="5aed5-104">This guide walks through creating an end-to-end deployment for an Azure Web App, leveraging MSDeploy as well as adding a custom hostname and an SSL certificate to the ARM template.</span></span>

<span data-ttu-id="5aed5-105">Další informace o vytváření šablon najdete v tématu [vytváření šablon Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="5aed5-105">For more information about creating templates, see [Authoring Azure Resource Manager Templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>

### <a name="create-sample-application"></a><span data-ttu-id="5aed5-106">Vytvoření ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="5aed5-106">Create Sample Application</span></span>
<span data-ttu-id="5aed5-107">Budete nasazovat webové aplikace ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="5aed5-107">You will be deploying an ASP.NET web application.</span></span> <span data-ttu-id="5aed5-108">Prvním krokem je vytvoření jednoduché webové aplikace (nebo můžete rozhodnout použít existující – v takovém případě můžete tento krok přeskočit).</span><span class="sxs-lookup"><span data-stu-id="5aed5-108">The first step is to create a simple web application (or you could choose to use an existing one - in which case you can skip this step).</span></span>

<span data-ttu-id="5aed5-109">Otevřete Visual Studio 2015 a zvolte Soubor > Nový projekt.</span><span class="sxs-lookup"><span data-stu-id="5aed5-109">Open Visual Studio 2015 and choose File > New Project.</span></span> <span data-ttu-id="5aed5-110">V zobrazeném dialogovém okně vyberte Web > ASP.NET Web Application.</span><span class="sxs-lookup"><span data-stu-id="5aed5-110">On the dialog that appears choose Web > ASP.NET Web Application.</span></span> <span data-ttu-id="5aed5-111">V části šablony vyberte Web a vyberte šablonu MVC.</span><span class="sxs-lookup"><span data-stu-id="5aed5-111">Under Templates choose Web and choose the MVC template.</span></span> <span data-ttu-id="5aed5-112">Vyberte *změnit typ ověřování* k *bez ověřování*.</span><span class="sxs-lookup"><span data-stu-id="5aed5-112">Select *Change authentication type* to *No Authentication*.</span></span> <span data-ttu-id="5aed5-113">Toto je k tomu, aby ukázkovou aplikaci co nejjednodušší.</span><span class="sxs-lookup"><span data-stu-id="5aed5-113">This is just to make the sample application as simple as possible.</span></span>

<span data-ttu-id="5aed5-114">V tomto okamžiku budete mít základní ASP.Net webové aplikace připravené k použití jako součást procesu nasazení.</span><span class="sxs-lookup"><span data-stu-id="5aed5-114">At this point you will have a basic ASP.Net web app ready to use as part of your deployment process.</span></span>

### <a name="create-msdeploy-package"></a><span data-ttu-id="5aed5-115">Vytvoření balíčku MSDeploy</span><span class="sxs-lookup"><span data-stu-id="5aed5-115">Create MSDeploy package</span></span>
<span data-ttu-id="5aed5-116">Dalším krokem je vytvoření balíčku pro nasazení webové aplikace do Azure.</span><span class="sxs-lookup"><span data-stu-id="5aed5-116">Next step is to create the package to deploy the web app to Azure.</span></span> <span data-ttu-id="5aed5-117">Chcete-li to provést, uložte projekt a pak spusťte následující příkaz z příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="5aed5-117">To do this, save your project and then run the following from the command line:</span></span>

    msbuild yourwebapp.csproj /t:Package /p:PackageLocation="path\to\package.zip"

<span data-ttu-id="5aed5-118">Tím se vytvoří ve složce PackageLocation komprimované balíček.</span><span class="sxs-lookup"><span data-stu-id="5aed5-118">This will create a zipped package under the PackageLocation folder.</span></span> <span data-ttu-id="5aed5-119">Aplikace je nyní připravena k nasazení, které teď můžete sestavit si šablonu Azure Resource Manageru k tomu.</span><span class="sxs-lookup"><span data-stu-id="5aed5-119">The application is now ready to be deployed, which you can now build out an Azure Resource Manager template to do that.</span></span>

### <a name="create-arm-template"></a><span data-ttu-id="5aed5-120">Vytvoření šablony ARM</span><span class="sxs-lookup"><span data-stu-id="5aed5-120">Create ARM Template</span></span>
<span data-ttu-id="5aed5-121">Nejdříve začneme základní šablony ARM, který vytvoří webovou aplikaci a plán hostování (Všimněte si, že parametry a proměnné nejsou zobrazeny jako stručný výtah).</span><span class="sxs-lookup"><span data-stu-id="5aed5-121">First, let's start with a basic ARM template that will create a web application and a hosting plan (note that parameters and variables are not shown for brevity).</span></span>

    {
        "name": "[parameters('appServicePlanName')]",
        "type": "Microsoft.Web/serverfarms",
        "location": "[resourceGroup().location]",
        "apiVersion": "2014-06-01",
        "dependsOn": [ ],
        "tags": {
            "displayName": "appServicePlan"
        },
        "properties": {
            "name": "[parameters('appServicePlanName')]",
            "sku": "[parameters('appServicePlanSKU')]",
            "workerSize": "[parameters('appServicePlanWorkerSize')]",
            "numberOfWorkers": 1
        }
    },
    {
        "name": "[variables('webAppName')]",
        "type": "Microsoft.Web/sites",
        "location": "[resourceGroup().location]",
        "apiVersion": "2015-08-01",
        "dependsOn": [
            "[concat('Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]"
        ],
        "tags": {
            "[concat('hidden-related:', resourceGroup().id, '/providers/Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]": "Resource",
            "displayName": "webApp"
        },
        "properties": {
            "name": "[variables('webAppName')]",
            "serverFarmId": "[resourceId('Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]"
        }
    }

<span data-ttu-id="5aed5-122">Dále bude nutné upravit prostředek webové aplikace provést vnořeného prostředku MSDeploy.</span><span class="sxs-lookup"><span data-stu-id="5aed5-122">Next, you will need to modify the web app resource to take a nested MSDeploy resource.</span></span> <span data-ttu-id="5aed5-123">To vám umožní k odkazu na balíček vytvořený a sdělte Azure Resource Manager pomocí MSDeploy balíček nasadit do webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="5aed5-123">This will allow you to reference the package created earlier and tell Azure Resource Manager to use MSDeploy to deploy the package to the Azure WebApp.</span></span> <span data-ttu-id="5aed5-124">Na obrázku Microsoft.Web/sites prostředek s vnořeného prostředku MSDeploy:</span><span class="sxs-lookup"><span data-stu-id="5aed5-124">The following shows the Microsoft.Web/sites resource with the nested MSDeploy resource:</span></span>

    {
        "name": "[variables('webAppName')]",
        "type": "Microsoft.Web/sites",
        "location": "[resourceGroup().location]",
        "apiVersion": "2015-08-01",
        "dependsOn": [
            "[concat('Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]"
        ],
        "tags": {
            "[concat('hidden-related:', resourceGroup().id, '/providers/Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]": "Resource",
            "displayName": "webApp"
        },
        "properties": {
            "name": "[variables('webAppName')]",
            "serverFarmId": "[resourceId('Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]"
        },
        "resources": [
            {
                "name": "MSDeploy",
                "type": "extensions",
                "location": "[resourceGroup().location]",
                "apiVersion": "2015-08-01",
                "dependsOn": [
                    "[concat('Microsoft.Web/sites/', variables('webAppName'))]"
                ],
                "tags": {
                    "displayName": "webDeploy"
                },
                "properties": {
                    "packageUri": "[concat(parameters('_artifactsLocation'), '/', parameters('webDeployPackageFolder'), '/', parameters('webDeployPackageFileName'), parameters('_artifactsLocationSasToken'))]",
                    "dbType": "None",
                    "connectionString": "",
                    "setParameters": {
                        "IIS Web Application Name": "[variables('webAppName')]"
                    }
                }
            }
        ]
    }

<span data-ttu-id="5aed5-125">Teď si všimnete, že trvá MSDeploy prostředků **packageUri** vlastnost, která je definován následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="5aed5-125">Now you will notice that the MSDeploy resource takes a **packageUri** property which is defined as follows:</span></span>

    "packageUri": "[concat(parameters('_artifactsLocation'), '/', parameters('webDeployPackageFolder'), '/', parameters('webDeployPackageFileName'), parameters('_artifactsLocationSasToken'))]"

<span data-ttu-id="5aed5-126">To **packageUri** trvá identifikátor uri účtu úložiště, který odkazuje na účet úložiště, kde bude nahrávat váš balíček zip do.</span><span class="sxs-lookup"><span data-stu-id="5aed5-126">This **packageUri** takes the storage account uri which points to the storage account where you will upload your package zip to.</span></span> <span data-ttu-id="5aed5-127">Azure Resource Manager využije [sdílené přístupové podpisy](../storage/common/storage-dotnet-shared-access-signature-part-1.md) k stahují balíček místně z účtu úložiště při nasazování šablony.</span><span class="sxs-lookup"><span data-stu-id="5aed5-127">The Azure Resource Manager will leverage [Shared Access Signatures](../storage/common/storage-dotnet-shared-access-signature-part-1.md) to pull the package down locally from the storage account when you deploy the template.</span></span> <span data-ttu-id="5aed5-128">Tento proces bude možné automatizovat pomocí Powershellový skript, který bude nahrání balíčku a volání rozhraní API pro správu Azure k vytvoření klíčů vyžaduje a v šabloně předat jako parametry (*_artifactsLocation* a *_ artifactsLocationSasToken*).</span><span class="sxs-lookup"><span data-stu-id="5aed5-128">This process will be automated via a PowerShell script that will upload the package and call the Azure Management API to create the keys required and pass those into the template as parameters (*_artifactsLocation* and *_artifactsLocationSasToken*).</span></span> <span data-ttu-id="5aed5-129">Je nutné zadat parametry pro složku a název souboru balíčku se nahraje v kontejneru úložiště.</span><span class="sxs-lookup"><span data-stu-id="5aed5-129">You will need to define parameters for the folder and filename the package is uploaded to under the storage container.</span></span>

<span data-ttu-id="5aed5-130">Dále je nutné přidat v jiné vnořeného prostředku nastavení vazby názvů hostitelů využívat vlastní doménu.</span><span class="sxs-lookup"><span data-stu-id="5aed5-130">Next you need to add in another nested resource to setup the hostname bindings to leverage a custom domain.</span></span> <span data-ttu-id="5aed5-131">Nejdřív musíte zajistit, že vlastníte do názvu hostitele a nastavte ho nahoru ověřit pomocí Azure jeho - vlastníkem najdete v tématu [konfigurace vlastního názvu domény v Azure App Service](app-service-web-tutorial-custom-domain.md).</span><span class="sxs-lookup"><span data-stu-id="5aed5-131">You will first need to ensure that you own the hostname and set it up to be verified by Azure that you own it - see [Configure a custom domain name in Azure App Service](app-service-web-tutorial-custom-domain.md).</span></span> <span data-ttu-id="5aed5-132">Po dokončení můžete do šablony prostředků části Microsoft.Web/sites přidejte následující:</span><span class="sxs-lookup"><span data-stu-id="5aed5-132">Once that is done you can add the following to your template under the Microsoft.Web/sites resource section:</span></span>

    {
        "apiVersion": "2015-08-01",
        "type": "hostNameBindings",
        "name": "www.yourcustomdomain.com",
        "dependsOn": [
            "[concat('Microsoft.Web/sites/', variables('webAppName'))]"
        ],
        "properties": {
            "domainId": null,
            "hostNameType": "Verified",
            "siteName": "variables('webAppName')"
        }
    }

<span data-ttu-id="5aed5-133">Nakonec budete muset přidat další nejvyšší úrovně prostředek, Microsoft.Web/certificates.</span><span class="sxs-lookup"><span data-stu-id="5aed5-133">Finally you need to add another top level resource, Microsoft.Web/certificates.</span></span> <span data-ttu-id="5aed5-134">Tento prostředek bude obsahovat svůj certifikát SSL a bude existovat na stejné úrovni jako webová aplikace a hostingový plán.</span><span class="sxs-lookup"><span data-stu-id="5aed5-134">This resource will contain your SSL certificate and will exist at the same level as your web app and hosting plan.</span></span>

    {
        "name": "[parameters('certificateName')]",
        "apiVersion": "2014-04-01",
        "type": "Microsoft.Web/certificates",
        "location": "[resourceGroup().location]",
        "properties": {
            "pfxBlob": "pfx base64 blob",
            "password": "some pass"
        }
    }

<span data-ttu-id="5aed5-135">Musíte mít platný certifikát SSL, pokud chcete nastavit tento prostředek.</span><span class="sxs-lookup"><span data-stu-id="5aed5-135">You will need to have a valid SSL certificate in order to set up this resource.</span></span> <span data-ttu-id="5aed5-136">Jakmile je tento certifikát budete potřebovat k extrakci bajtů pfx jako řetězec base64.</span><span class="sxs-lookup"><span data-stu-id="5aed5-136">Once you have that valid certificate then you need to extract the pfx bytes as a base64 string.</span></span> <span data-ttu-id="5aed5-137">Jednou z možností k extrakci to je použijte následující příkaz prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="5aed5-137">One option to extract this is to use the following PowerShell command:</span></span>

    $fileContentBytes = get-content 'C:\path\to\cert.pfx' -Encoding Byte

    [System.Convert]::ToBase64String($fileContentBytes) | Out-File 'pfx-bytes.txt'

<span data-ttu-id="5aed5-138">Potom toto může předávat do nasazení šablony ARM jako parametr.</span><span class="sxs-lookup"><span data-stu-id="5aed5-138">You could then pass this as a parameter to your ARM deployment template.</span></span>

<span data-ttu-id="5aed5-139">Šablona ARM je nyní připraven.</span><span class="sxs-lookup"><span data-stu-id="5aed5-139">At this point the ARM template is ready.</span></span>

### <a name="deploy-template"></a><span data-ttu-id="5aed5-140">Nasazení šablony</span><span class="sxs-lookup"><span data-stu-id="5aed5-140">Deploy Template</span></span>
<span data-ttu-id="5aed5-141">Závěrečné kroky se část všechny společně do nasazení úplného začátku do konce.</span><span class="sxs-lookup"><span data-stu-id="5aed5-141">The final steps are to piece this all together into a full end-to-end deployment.</span></span> <span data-ttu-id="5aed5-142">Aby nasazení jednodušší můžete využít **nasadit AzureResourceGroup.ps1** Powershellový skript, který se přidá, když vytvoříte projekt skupiny prostředků Azure v sadě Visual Studio, které pomáhají při odesílání artefakty potřebné v Šablona.</span><span class="sxs-lookup"><span data-stu-id="5aed5-142">To make deployment easier you can leverage the **Deploy-AzureResourceGroup.ps1** PowerShell script that is added when you create an Azure Resource Group project in Visual Studio to help with uploading of any artifacts required in the template.</span></span> <span data-ttu-id="5aed5-143">Se vyžaduje, abyste vytvořili účet úložiště, který chcete použít předem.</span><span class="sxs-lookup"><span data-stu-id="5aed5-143">It requires you to have created a storage account you want to use ahead of time.</span></span> <span data-ttu-id="5aed5-144">V tomto příkladu vytvoříme účet sdílené úložiště pro package.zip k odeslání.</span><span class="sxs-lookup"><span data-stu-id="5aed5-144">For this example, I created a shared storage account for the package.zip to be uploaded.</span></span> <span data-ttu-id="5aed5-145">Skript bude využívat AzCopy pro nahrání balíčku k účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="5aed5-145">The script will leverage AzCopy to upload the package to the storage account.</span></span> <span data-ttu-id="5aed5-146">Předat do umístění složky vašeho artefaktů a skript bude automaticky odeslat všechny soubory v tomto adresáři kontejner s názvem úložiště.</span><span class="sxs-lookup"><span data-stu-id="5aed5-146">You pass in your artifact folder location and the script will automatically upload all files within that directory to the named storage container.</span></span> <span data-ttu-id="5aed5-147">Po volání metody nasazení AzureResourceGroup.ps1 budete muset aktualizovat vazby SSL k mapování vlastní název hostitele certifikátem SSL.</span><span class="sxs-lookup"><span data-stu-id="5aed5-147">After calling Deploy-AzureResourceGroup.ps1 you have to then update the SSL bindings to map the custom hostname with your SSL certificate.</span></span>

<span data-ttu-id="5aed5-148">Následující PowerShell ukazuje dokončení nasazení volání nasadit-AzureResourceGroup.ps1:</span><span class="sxs-lookup"><span data-stu-id="5aed5-148">The following PowerShell shows the complete deployment calling the Deploy-AzureResourceGroup.ps1:</span></span>

    #Set resource group name
    $rgName = "Name-of-resource-group"

    #call deploy-azureresourcegroup script to deploy web app

    .\Deploy-AzureResourceGroup.ps1 -ResourceGroupLocation "East US" `
                                    -ResourceGroupName $rgName `
                                    -UploadArtifacts `
                                    -StorageAccountName "name-of-storage-acct-for-package" `
                                    -StorageAccountResourceGroupName "resource-group-name-storage-acct" `
                                    -TemplateFile "web-app-deploy.json" `
                                    -TemplateParametersFile "web-app-deploy-parameters.json" `
                                    -ArtifactStagingDirectory "C:\path\to\packagefolder\"

    #update web app to bind ssl certificate to hostname. This has to be done after creation above.

    $cert = Get-PfxCertificate -FilePath C:\path\to\certificate.pfx

    $ar = Get-AzureRmResource -Name nameofwebsite -ResourceGroupName $rgName -ResourceType Microsoft.Web/sites -ApiVersion 2014-11-01

    $props = $ar.Properties

    $props.HostNameSslStates[2].'SslState' = 1
    $props.HostNameSslStates[2].'thumbprint' = $cert.Thumbprint
    $props.hostNameSslStates[2].'toUpdate' = $true

    Set-AzureRmResource -ApiVersion 2014-11-01 -Name nameofwebsite -ResourceGroupName $rgName -ResourceType Microsoft.Web/sites -PropertyObject $props

<span data-ttu-id="5aed5-149">V tomto okamžiku aplikace by měla mít nasazený a nyní byste měli mít vyhledejte prostřednictvím https://www.yourcustomdomain.com</span><span class="sxs-lookup"><span data-stu-id="5aed5-149">At this point your application should have been deployed and you should be able to browse to it via https://www.yourcustomdomain.com</span></span>

