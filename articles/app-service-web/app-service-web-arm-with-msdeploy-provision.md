---
title: "aaaDeploy webovou aplikaci pomocí MSDeploy certifikátem název hostitele a ssl"
description: "Použijte toodeploy šablony Azure Resource Manager webovou aplikaci pomocí MSDeploy a nastavení vlastního názvu hostitele a certifikátu SSL"
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
ms.openlocfilehash: ac13f4a7d14ae182e8e7ced5adff30491422d1e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-web-app-with-msdeploy-custom-hostname-and-ssl-certificate"></a><span data-ttu-id="e7ac0-103">Nasazení webové aplikace s MSDeploy, vlastní název hostitele a certifikátu SSL</span><span class="sxs-lookup"><span data-stu-id="e7ac0-103">Deploy a web app with MSDeploy, custom hostname and SSL certificate</span></span>
<span data-ttu-id="e7ac0-104">Tento průvodce vás provede vytváření nasazení začátku do konce pro webové aplikace Azure, využití MSDeploy a také přidání vlastního názvu hostitele a šablony ARM toohello certifikát SSL.</span><span class="sxs-lookup"><span data-stu-id="e7ac0-104">This guide walks through creating an end-to-end deployment for an Azure Web App, leveraging MSDeploy as well as adding a custom hostname and an SSL certificate toohello ARM template.</span></span>

<span data-ttu-id="e7ac0-105">Další informace o vytváření šablon najdete v tématu [vytváření šablon Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="e7ac0-105">For more information about creating templates, see [Authoring Azure Resource Manager Templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>

### <a name="create-sample-application"></a><span data-ttu-id="e7ac0-106">Vytvoření ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="e7ac0-106">Create Sample Application</span></span>
<span data-ttu-id="e7ac0-107">Budete nasazovat webové aplikace ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e7ac0-107">You will be deploying an ASP.NET web application.</span></span> <span data-ttu-id="e7ac0-108">prvním krokem Hello je toocreate jednoduché webové aplikace (nebo toouse může zvolit existující – v takovém případě můžete tento krok přeskočit).</span><span class="sxs-lookup"><span data-stu-id="e7ac0-108">hello first step is toocreate a simple web application (or you could choose toouse an existing one - in which case you can skip this step).</span></span>

<span data-ttu-id="e7ac0-109">Otevřete Visual Studio 2015 a zvolte Soubor > Nový projekt.</span><span class="sxs-lookup"><span data-stu-id="e7ac0-109">Open Visual Studio 2015 and choose File > New Project.</span></span> <span data-ttu-id="e7ac0-110">V dialogovém okně hello, který se zobrazí vyberte Web > ASP.NET Web Application.</span><span class="sxs-lookup"><span data-stu-id="e7ac0-110">On hello dialog that appears choose Web > ASP.NET Web Application.</span></span> <span data-ttu-id="e7ac0-111">V části šablony vyberte Web a vyberte šablonu MVC hello.</span><span class="sxs-lookup"><span data-stu-id="e7ac0-111">Under Templates choose Web and choose hello MVC template.</span></span> <span data-ttu-id="e7ac0-112">Vyberte *změnit typ ověřování* příliš*bez ověřování*.</span><span class="sxs-lookup"><span data-stu-id="e7ac0-112">Select *Change authentication type* too*No Authentication*.</span></span> <span data-ttu-id="e7ac0-113">Toto je právě toomake hello ukázkovou aplikaci co nejjednodušší.</span><span class="sxs-lookup"><span data-stu-id="e7ac0-113">This is just toomake hello sample application as simple as possible.</span></span>

<span data-ttu-id="e7ac0-114">V tomto okamžiku budete mít základní ASP.Net webové aplikace připravené toouse jako součást procesu nasazení.</span><span class="sxs-lookup"><span data-stu-id="e7ac0-114">At this point you will have a basic ASP.Net web app ready toouse as part of your deployment process.</span></span>

### <a name="create-msdeploy-package"></a><span data-ttu-id="e7ac0-115">Vytvoření balíčku MSDeploy</span><span class="sxs-lookup"><span data-stu-id="e7ac0-115">Create MSDeploy package</span></span>
<span data-ttu-id="e7ac0-116">Dalším krokem je toocreate hello balíček toodeploy hello webové aplikace tooAzure.</span><span class="sxs-lookup"><span data-stu-id="e7ac0-116">Next step is toocreate hello package toodeploy hello web app tooAzure.</span></span> <span data-ttu-id="e7ac0-117">toodo toho uložte projekt a pak spusťte z příkazového řádku hello hello následující:</span><span class="sxs-lookup"><span data-stu-id="e7ac0-117">toodo this, save your project and then run hello following from hello command line:</span></span>

    msbuild yourwebapp.csproj /t:Package /p:PackageLocation="path\to\package.zip"

<span data-ttu-id="e7ac0-118">Tím se vytvoří balíček komprimované složce PackageLocation hello.</span><span class="sxs-lookup"><span data-stu-id="e7ac0-118">This will create a zipped package under hello PackageLocation folder.</span></span> <span data-ttu-id="e7ac0-119">Hello aplikace je nyní připraveni toobe nasazení, které teď můžete sestavit se toodo šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="e7ac0-119">hello application is now ready toobe deployed, which you can now build out an Azure Resource Manager template toodo that.</span></span>

### <a name="create-arm-template"></a><span data-ttu-id="e7ac0-120">Vytvoření šablony ARM</span><span class="sxs-lookup"><span data-stu-id="e7ac0-120">Create ARM Template</span></span>
<span data-ttu-id="e7ac0-121">Nejdříve začneme základní šablony ARM, který vytvoří webovou aplikaci a plán hostování (Všimněte si, že parametry a proměnné nejsou zobrazeny jako stručný výtah).</span><span class="sxs-lookup"><span data-stu-id="e7ac0-121">First, let's start with a basic ARM template that will create a web application and a hosting plan (note that parameters and variables are not shown for brevity).</span></span>

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

<span data-ttu-id="e7ac0-122">V dalším kroku budete potřebovat toomodify hello webové aplikace prostředků tootake vnořeného prostředku MSDeploy.</span><span class="sxs-lookup"><span data-stu-id="e7ac0-122">Next, you will need toomodify hello web app resource tootake a nested MSDeploy resource.</span></span> <span data-ttu-id="e7ac0-123">To bude povolit jste tooreference hello balíčku dříve vytvořili a sdělte Azure Resource Manager toouse MSDeploy toodeploy hello balíček toohello Azure WebApp.</span><span class="sxs-lookup"><span data-stu-id="e7ac0-123">This will allow you tooreference hello package created earlier and tell Azure Resource Manager toouse MSDeploy toodeploy hello package toohello Azure WebApp.</span></span> <span data-ttu-id="e7ac0-124">Následující Hello ukazuje hello Microsoft.Web/sites prostředek s hello vnořené MSDeploy prostředků:</span><span class="sxs-lookup"><span data-stu-id="e7ac0-124">hello following shows hello Microsoft.Web/sites resource with hello nested MSDeploy resource:</span></span>

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

<span data-ttu-id="e7ac0-125">Teď si všimnete, že hello MSDeploy prostředků trvá **packageUri** vlastnost, která je definován následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="e7ac0-125">Now you will notice that hello MSDeploy resource takes a **packageUri** property which is defined as follows:</span></span>

    "packageUri": "[concat(parameters('_artifactsLocation'), '/', parameters('webDeployPackageFolder'), '/', parameters('webDeployPackageFileName'), parameters('_artifactsLocationSasToken'))]"

<span data-ttu-id="e7ac0-126">To **packageUri** trvá hello identifikátor uri účtu, který účet úložiště toohello, kde bude nahrávat váš balíček zip do bodů.</span><span class="sxs-lookup"><span data-stu-id="e7ac0-126">This **packageUri** takes hello storage account uri which points toohello storage account where you will upload your package zip to.</span></span> <span data-ttu-id="e7ac0-127">Hello Azure Resource Manager bude využívat [sdílené přístupové podpisy](../storage/common/storage-dotnet-shared-access-signature-part-1.md) toopull hello balíček dolů místně z účtu úložiště hello při nasazování šablony hello.</span><span class="sxs-lookup"><span data-stu-id="e7ac0-127">hello Azure Resource Manager will leverage [Shared Access Signatures](../storage/common/storage-dotnet-shared-access-signature-part-1.md) toopull hello package down locally from hello storage account when you deploy hello template.</span></span> <span data-ttu-id="e7ac0-128">Tento proces bude možné automatizovat pomocí skriptu PowerShell, který nahrání balíčku hello a zavolá hello rozhraní API pro správu Azure toocreate hello klíče vyžadované a ty do šablony hello předat jako parametry (*_artifactsLocation* a *_artifactsLocationSasToken*).</span><span class="sxs-lookup"><span data-stu-id="e7ac0-128">This process will be automated via a PowerShell script that will upload hello package and call hello Azure Management API toocreate hello keys required and pass those into hello template as parameters (*_artifactsLocation* and *_artifactsLocationSasToken*).</span></span> <span data-ttu-id="e7ac0-129">Budete potřebovat toodefine parametry pro složku hello a filename hello balíček je kontejner úložiště nahrané toounder hello.</span><span class="sxs-lookup"><span data-stu-id="e7ac0-129">You will need toodefine parameters for hello folder and filename hello package is uploaded toounder hello storage container.</span></span>

<span data-ttu-id="e7ac0-130">Dále je třeba tooadd v jiné vnořeného prostředku toosetup hello hostname vazby tooleverage vlastní doménu.</span><span class="sxs-lookup"><span data-stu-id="e7ac0-130">Next you need tooadd in another nested resource toosetup hello hostname bindings tooleverage a custom domain.</span></span> <span data-ttu-id="e7ac0-131">Bude první nutné tooensure vlastní název hostitele hello a nastavit toobe ověřit pomocí Azure, že jste jejím vlastníkem – viz [konfigurace vlastního názvu domény v Azure App Service](app-service-web-tutorial-custom-domain.md).</span><span class="sxs-lookup"><span data-stu-id="e7ac0-131">You will first need tooensure that you own hello hostname and set it up toobe verified by Azure that you own it - see [Configure a custom domain name in Azure App Service](app-service-web-tutorial-custom-domain.md).</span></span> <span data-ttu-id="e7ac0-132">Po dokončení můžete přidat hello následující šablony tooyour části hello Microsoft.Web/sites prostředků:</span><span class="sxs-lookup"><span data-stu-id="e7ac0-132">Once that is done you can add hello following tooyour template under hello Microsoft.Web/sites resource section:</span></span>

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

<span data-ttu-id="e7ac0-133">Nakonec musíte tooadd jiný prostředek Microsoft.Web/certificates na nejvyšší úrovni.</span><span class="sxs-lookup"><span data-stu-id="e7ac0-133">Finally you need tooadd another top level resource, Microsoft.Web/certificates.</span></span> <span data-ttu-id="e7ac0-134">Tento prostředek bude obsahovat svůj certifikát SSL a bude existovat na stejné úrovni jako webové aplikace a hostování plánování hello.</span><span class="sxs-lookup"><span data-stu-id="e7ac0-134">This resource will contain your SSL certificate and will exist at hello same level as your web app and hosting plan.</span></span>

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

<span data-ttu-id="e7ac0-135">Budete potřebovat toohave platný certifikát SSL v pořadí tooset až tento prostředek.</span><span class="sxs-lookup"><span data-stu-id="e7ac0-135">You will need toohave a valid SSL certificate in order tooset up this resource.</span></span> <span data-ttu-id="e7ac0-136">Jakmile je tento certifikát budete potřebovat tooextract hello pfx bajtů jako řetězec base64.</span><span class="sxs-lookup"><span data-stu-id="e7ac0-136">Once you have that valid certificate then you need tooextract hello pfx bytes as a base64 string.</span></span> <span data-ttu-id="e7ac0-137">Jednou z možností tooextract jde toouse hello následující příkaz prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="e7ac0-137">One option tooextract this is toouse hello following PowerShell command:</span></span>

    $fileContentBytes = get-content 'C:\path\to\cert.pfx' -Encoding Byte

    [System.Convert]::ToBase64String($fileContentBytes) | Out-File 'pfx-bytes.txt'

<span data-ttu-id="e7ac0-138">To může pak předejte jako parametr tooyour ARM nasazení šablony.</span><span class="sxs-lookup"><span data-stu-id="e7ac0-138">You could then pass this as a parameter tooyour ARM deployment template.</span></span>

<span data-ttu-id="e7ac0-139">V tomto okamžiku je připraven šablony ARM hello.</span><span class="sxs-lookup"><span data-stu-id="e7ac0-139">At this point hello ARM template is ready.</span></span>

### <a name="deploy-template"></a><span data-ttu-id="e7ac0-140">Nasazení šablony</span><span class="sxs-lookup"><span data-stu-id="e7ac0-140">Deploy Template</span></span>
<span data-ttu-id="e7ac0-141">Hello závěrečné kroky jsou toopiece to všechny společně do nasazení úplného začátku do konce.</span><span class="sxs-lookup"><span data-stu-id="e7ac0-141">hello final steps are toopiece this all together into a full end-to-end deployment.</span></span> <span data-ttu-id="e7ac0-142">nasazení toomake jednodušší, můžete využít hello **nasadit AzureResourceGroup.ps1** Powershellový skript, který se přidá, když vytvoříte projekt skupiny prostředků Azure v sadě Visual Studio toohelp s artefakty potřebné v odesílání Šablona Hello.</span><span class="sxs-lookup"><span data-stu-id="e7ac0-142">toomake deployment easier you can leverage hello **Deploy-AzureResourceGroup.ps1** PowerShell script that is added when you create an Azure Resource Group project in Visual Studio toohelp with uploading of any artifacts required in hello template.</span></span> <span data-ttu-id="e7ac0-143">To vyžaduje toohave vytvořili účet úložiště, že který má toouse dopředu.</span><span class="sxs-lookup"><span data-stu-id="e7ac0-143">It requires you toohave created a storage account you want toouse ahead of time.</span></span> <span data-ttu-id="e7ac0-144">V tomto příkladu vytvoříme účet sdílené úložiště pro toobe package.zip hello nahrát.</span><span class="sxs-lookup"><span data-stu-id="e7ac0-144">For this example, I created a shared storage account for hello package.zip toobe uploaded.</span></span> <span data-ttu-id="e7ac0-145">skript Hello využije AzCopy tooupload hello balíček toohello účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="e7ac0-145">hello script will leverage AzCopy tooupload hello package toohello storage account.</span></span> <span data-ttu-id="e7ac0-146">Předat do umístění složky vašeho artefaktů a hello skript bude automaticky odeslat všechny soubory v rámci této toohello adresář s názvem kontejner úložiště.</span><span class="sxs-lookup"><span data-stu-id="e7ac0-146">You pass in your artifact folder location and hello script will automatically upload all files within that directory toohello named storage container.</span></span> <span data-ttu-id="e7ac0-147">Po volání metody nasazení AzureResourceGroup.ps1 máte toothen aktualizace hello protokolu SSL vazby toomap hello vlastní název hostitele certifikátem SSL.</span><span class="sxs-lookup"><span data-stu-id="e7ac0-147">After calling Deploy-AzureResourceGroup.ps1 you have toothen update hello SSL bindings toomap hello custom hostname with your SSL certificate.</span></span>

<span data-ttu-id="e7ac0-148">Hello následující prostředí PowerShell ukazuje hello dokončení nasazení volání hello nasadit-AzureResourceGroup.ps1:</span><span class="sxs-lookup"><span data-stu-id="e7ac0-148">hello following PowerShell shows hello complete deployment calling hello Deploy-AzureResourceGroup.ps1:</span></span>

    #Set resource group name
    $rgName = "Name-of-resource-group"

    #call deploy-azureresourcegroup script toodeploy web app

    .\Deploy-AzureResourceGroup.ps1 -ResourceGroupLocation "East US" `
                                    -ResourceGroupName $rgName `
                                    -UploadArtifacts `
                                    -StorageAccountName "name-of-storage-acct-for-package" `
                                    -StorageAccountResourceGroupName "resource-group-name-storage-acct" `
                                    -TemplateFile "web-app-deploy.json" `
                                    -TemplateParametersFile "web-app-deploy-parameters.json" `
                                    -ArtifactStagingDirectory "C:\path\to\packagefolder\"

    #update web app toobind ssl certificate toohostname. This has toobe done after creation above.

    $cert = Get-PfxCertificate -FilePath C:\path\to\certificate.pfx

    $ar = Get-AzureRmResource -Name nameofwebsite -ResourceGroupName $rgName -ResourceType Microsoft.Web/sites -ApiVersion 2014-11-01

    $props = $ar.Properties

    $props.HostNameSslStates[2].'SslState' = 1
    $props.HostNameSslStates[2].'thumbprint' = $cert.Thumbprint
    $props.hostNameSslStates[2].'toUpdate' = $true

    Set-AzureRmResource -ApiVersion 2014-11-01 -Name nameofwebsite -ResourceGroupName $rgName -ResourceType Microsoft.Web/sites -PropertyObject $props

<span data-ttu-id="e7ac0-149">V tomto okamžiku aplikace by měla mít nasazený a mělo by být možné toobrowse tooit prostřednictvím https://www.yourcustomdomain.com</span><span class="sxs-lookup"><span data-stu-id="e7ac0-149">At this point your application should have been deployed and you should be able toobrowse tooit via https://www.yourcustomdomain.com</span></span>

