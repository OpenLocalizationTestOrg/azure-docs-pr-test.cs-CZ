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
# <a name="deploy-a-web-app-with-msdeploy-custom-hostname-and-ssl-certificate"></a>Nasazení webové aplikace s MSDeploy, vlastní název hostitele a certifikátu SSL
Tento průvodce vás provede vytváření nasazení začátku do konce pro webové aplikace Azure, využití MSDeploy a také přidání vlastního názvu hostitele a šablony ARM toohello certifikát SSL.

Další informace o vytváření šablon najdete v tématu [vytváření šablon Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md).

### <a name="create-sample-application"></a>Vytvoření ukázkové aplikace
Budete nasazovat webové aplikace ASP.NET. prvním krokem Hello je toocreate jednoduché webové aplikace (nebo toouse může zvolit existující – v takovém případě můžete tento krok přeskočit).

Otevřete Visual Studio 2015 a zvolte Soubor > Nový projekt. V dialogovém okně hello, který se zobrazí vyberte Web > ASP.NET Web Application. V části šablony vyberte Web a vyberte šablonu MVC hello. Vyberte *změnit typ ověřování* příliš*bez ověřování*. Toto je právě toomake hello ukázkovou aplikaci co nejjednodušší.

V tomto okamžiku budete mít základní ASP.Net webové aplikace připravené toouse jako součást procesu nasazení.

### <a name="create-msdeploy-package"></a>Vytvoření balíčku MSDeploy
Dalším krokem je toocreate hello balíček toodeploy hello webové aplikace tooAzure. toodo toho uložte projekt a pak spusťte z příkazového řádku hello hello následující:

    msbuild yourwebapp.csproj /t:Package /p:PackageLocation="path\to\package.zip"

Tím se vytvoří balíček komprimované složce PackageLocation hello. Hello aplikace je nyní připraveni toobe nasazení, které teď můžete sestavit se toodo šablony Azure Resource Manager.

### <a name="create-arm-template"></a>Vytvoření šablony ARM
Nejdříve začneme základní šablony ARM, který vytvoří webovou aplikaci a plán hostování (Všimněte si, že parametry a proměnné nejsou zobrazeny jako stručný výtah).

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

V dalším kroku budete potřebovat toomodify hello webové aplikace prostředků tootake vnořeného prostředku MSDeploy. To bude povolit jste tooreference hello balíčku dříve vytvořili a sdělte Azure Resource Manager toouse MSDeploy toodeploy hello balíček toohello Azure WebApp. Následující Hello ukazuje hello Microsoft.Web/sites prostředek s hello vnořené MSDeploy prostředků:

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

Teď si všimnete, že hello MSDeploy prostředků trvá **packageUri** vlastnost, která je definován následujícím způsobem:

    "packageUri": "[concat(parameters('_artifactsLocation'), '/', parameters('webDeployPackageFolder'), '/', parameters('webDeployPackageFileName'), parameters('_artifactsLocationSasToken'))]"

To **packageUri** trvá hello identifikátor uri účtu, který účet úložiště toohello, kde bude nahrávat váš balíček zip do bodů. Hello Azure Resource Manager bude využívat [sdílené přístupové podpisy](../storage/common/storage-dotnet-shared-access-signature-part-1.md) toopull hello balíček dolů místně z účtu úložiště hello při nasazování šablony hello. Tento proces bude možné automatizovat pomocí skriptu PowerShell, který nahrání balíčku hello a zavolá hello rozhraní API pro správu Azure toocreate hello klíče vyžadované a ty do šablony hello předat jako parametry (*_artifactsLocation* a *_artifactsLocationSasToken*). Budete potřebovat toodefine parametry pro složku hello a filename hello balíček je kontejner úložiště nahrané toounder hello.

Dále je třeba tooadd v jiné vnořeného prostředku toosetup hello hostname vazby tooleverage vlastní doménu. Bude první nutné tooensure vlastní název hostitele hello a nastavit toobe ověřit pomocí Azure, že jste jejím vlastníkem – viz [konfigurace vlastního názvu domény v Azure App Service](app-service-web-tutorial-custom-domain.md). Po dokončení můžete přidat hello následující šablony tooyour části hello Microsoft.Web/sites prostředků:

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

Nakonec musíte tooadd jiný prostředek Microsoft.Web/certificates na nejvyšší úrovni. Tento prostředek bude obsahovat svůj certifikát SSL a bude existovat na stejné úrovni jako webové aplikace a hostování plánování hello.

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

Budete potřebovat toohave platný certifikát SSL v pořadí tooset až tento prostředek. Jakmile je tento certifikát budete potřebovat tooextract hello pfx bajtů jako řetězec base64. Jednou z možností tooextract jde toouse hello následující příkaz prostředí PowerShell:

    $fileContentBytes = get-content 'C:\path\to\cert.pfx' -Encoding Byte

    [System.Convert]::ToBase64String($fileContentBytes) | Out-File 'pfx-bytes.txt'

To může pak předejte jako parametr tooyour ARM nasazení šablony.

V tomto okamžiku je připraven šablony ARM hello.

### <a name="deploy-template"></a>Nasazení šablony
Hello závěrečné kroky jsou toopiece to všechny společně do nasazení úplného začátku do konce. nasazení toomake jednodušší, můžete využít hello **nasadit AzureResourceGroup.ps1** Powershellový skript, který se přidá, když vytvoříte projekt skupiny prostředků Azure v sadě Visual Studio toohelp s artefakty potřebné v odesílání Šablona Hello. To vyžaduje toohave vytvořili účet úložiště, že který má toouse dopředu. V tomto příkladu vytvoříme účet sdílené úložiště pro toobe package.zip hello nahrát. skript Hello využije AzCopy tooupload hello balíček toohello účet úložiště. Předat do umístění složky vašeho artefaktů a hello skript bude automaticky odeslat všechny soubory v rámci této toohello adresář s názvem kontejner úložiště. Po volání metody nasazení AzureResourceGroup.ps1 máte toothen aktualizace hello protokolu SSL vazby toomap hello vlastní název hostitele certifikátem SSL.

Hello následující prostředí PowerShell ukazuje hello dokončení nasazení volání hello nasadit-AzureResourceGroup.ps1:

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

V tomto okamžiku aplikace by měla mít nasazený a mělo by být možné toobrowse tooit prostřednictvím https://www.yourcustomdomain.com

