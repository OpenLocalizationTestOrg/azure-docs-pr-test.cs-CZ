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
# <a name="deploy-a-web-app-with-msdeploy-custom-hostname-and-ssl-certificate"></a>Nasazení webové aplikace s MSDeploy, vlastní název hostitele a certifikátu SSL
Tento průvodce vás provede vytváření nasazení začátku do konce pro webové aplikace Azure, využití MSDeploy a také přidání vlastního názvu hostitele a certifikát SSL pro šablony ARM.

Další informace o vytváření šablon najdete v tématu [vytváření šablon Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md).

### <a name="create-sample-application"></a>Vytvoření ukázkové aplikace
Budete nasazovat webové aplikace ASP.NET. Prvním krokem je vytvoření jednoduché webové aplikace (nebo můžete rozhodnout použít existující – v takovém případě můžete tento krok přeskočit).

Otevřete Visual Studio 2015 a zvolte Soubor > Nový projekt. V zobrazeném dialogovém okně vyberte Web > ASP.NET Web Application. V části šablony vyberte Web a vyberte šablonu MVC. Vyberte *změnit typ ověřování* k *bez ověřování*. Toto je k tomu, aby ukázkovou aplikaci co nejjednodušší.

V tomto okamžiku budete mít základní ASP.Net webové aplikace připravené k použití jako součást procesu nasazení.

### <a name="create-msdeploy-package"></a>Vytvoření balíčku MSDeploy
Dalším krokem je vytvoření balíčku pro nasazení webové aplikace do Azure. Chcete-li to provést, uložte projekt a pak spusťte následující příkaz z příkazového řádku:

    msbuild yourwebapp.csproj /t:Package /p:PackageLocation="path\to\package.zip"

Tím se vytvoří ve složce PackageLocation komprimované balíček. Aplikace je nyní připravena k nasazení, které teď můžete sestavit si šablonu Azure Resource Manageru k tomu.

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

Dále bude nutné upravit prostředek webové aplikace provést vnořeného prostředku MSDeploy. To vám umožní k odkazu na balíček vytvořený a sdělte Azure Resource Manager pomocí MSDeploy balíček nasadit do webové aplikace Azure. Na obrázku Microsoft.Web/sites prostředek s vnořeného prostředku MSDeploy:

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

Teď si všimnete, že trvá MSDeploy prostředků **packageUri** vlastnost, která je definován následujícím způsobem:

    "packageUri": "[concat(parameters('_artifactsLocation'), '/', parameters('webDeployPackageFolder'), '/', parameters('webDeployPackageFileName'), parameters('_artifactsLocationSasToken'))]"

To **packageUri** trvá identifikátor uri účtu úložiště, který odkazuje na účet úložiště, kde bude nahrávat váš balíček zip do. Azure Resource Manager využije [sdílené přístupové podpisy](../storage/common/storage-dotnet-shared-access-signature-part-1.md) k stahují balíček místně z účtu úložiště při nasazování šablony. Tento proces bude možné automatizovat pomocí Powershellový skript, který bude nahrání balíčku a volání rozhraní API pro správu Azure k vytvoření klíčů vyžaduje a v šabloně předat jako parametry (*_artifactsLocation* a *_ artifactsLocationSasToken*). Je nutné zadat parametry pro složku a název souboru balíčku se nahraje v kontejneru úložiště.

Dále je nutné přidat v jiné vnořeného prostředku nastavení vazby názvů hostitelů využívat vlastní doménu. Nejdřív musíte zajistit, že vlastníte do názvu hostitele a nastavte ho nahoru ověřit pomocí Azure jeho - vlastníkem najdete v tématu [konfigurace vlastního názvu domény v Azure App Service](app-service-web-tutorial-custom-domain.md). Po dokončení můžete do šablony prostředků části Microsoft.Web/sites přidejte následující:

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

Nakonec budete muset přidat další nejvyšší úrovně prostředek, Microsoft.Web/certificates. Tento prostředek bude obsahovat svůj certifikát SSL a bude existovat na stejné úrovni jako webová aplikace a hostingový plán.

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

Musíte mít platný certifikát SSL, pokud chcete nastavit tento prostředek. Jakmile je tento certifikát budete potřebovat k extrakci bajtů pfx jako řetězec base64. Jednou z možností k extrakci to je použijte následující příkaz prostředí PowerShell:

    $fileContentBytes = get-content 'C:\path\to\cert.pfx' -Encoding Byte

    [System.Convert]::ToBase64String($fileContentBytes) | Out-File 'pfx-bytes.txt'

Potom toto může předávat do nasazení šablony ARM jako parametr.

Šablona ARM je nyní připraven.

### <a name="deploy-template"></a>Nasazení šablony
Závěrečné kroky se část všechny společně do nasazení úplného začátku do konce. Aby nasazení jednodušší můžete využít **nasadit AzureResourceGroup.ps1** Powershellový skript, který se přidá, když vytvoříte projekt skupiny prostředků Azure v sadě Visual Studio, které pomáhají při odesílání artefakty potřebné v Šablona. Se vyžaduje, abyste vytvořili účet úložiště, který chcete použít předem. V tomto příkladu vytvoříme účet sdílené úložiště pro package.zip k odeslání. Skript bude využívat AzCopy pro nahrání balíčku k účtu úložiště. Předat do umístění složky vašeho artefaktů a skript bude automaticky odeslat všechny soubory v tomto adresáři kontejner s názvem úložiště. Po volání metody nasazení AzureResourceGroup.ps1 budete muset aktualizovat vazby SSL k mapování vlastní název hostitele certifikátem SSL.

Následující PowerShell ukazuje dokončení nasazení volání nasadit-AzureResourceGroup.ps1:

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

V tomto okamžiku aplikace by měla mít nasazený a nyní byste měli mít vyhledejte prostřednictvím https://www.yourcustomdomain.com

