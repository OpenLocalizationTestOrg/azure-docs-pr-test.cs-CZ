---
title: "Připojení aplikace k virtuální síti pomocí PowerShellu"
description: "Pokyny o tom, jak připojit a pracovat s virtuálními sítěmi pomocí prostředí PowerShell"
services: app-service
documentationcenter: 
author: ccompy
manager: erikre
editor: cephalin
ms.assetid: a5c76e77-972a-431c-b14b-3611dae1631b
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/29/2016
ms.author: ccompy
ms.openlocfilehash: 6fae6a6c162fa326161d2b47a259b3151d6e3dd0
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="connect-your-app-to-your-virtual-network-by-using-powershell"></a><span data-ttu-id="212f4-103">Připojení aplikace k virtuální síti pomocí PowerShellu</span><span class="sxs-lookup"><span data-stu-id="212f4-103">Connect your app to your virtual network by using PowerShell</span></span>
## <a name="overview"></a><span data-ttu-id="212f4-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="212f4-104">Overview</span></span>
<span data-ttu-id="212f4-105">Ve službě Azure App Service můžete připojit aplikace (webové, mobilní nebo rozhraní API) pro virtuální sítě Azure (VNet) v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="212f4-105">In Azure App Service, you can connect your app (web, mobile, or API) to an Azure virtual network (VNet) in your subscription.</span></span> <span data-ttu-id="212f4-106">Tato funkce je volána integrace virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="212f4-106">This feature is called VNet Integration.</span></span> <span data-ttu-id="212f4-107">Funkce integrace virtuální sítě Nezaměňovat s funkci App Service Environment, která umožňuje spustit instanci služby Azure App Service ve virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="212f4-107">The VNet Integration feature should not be confused with the App Service Environment feature, which allows you to run an instance of Azure App Service in your virtual network.</span></span>

<span data-ttu-id="212f4-108">Funkce integrace virtuální sítě má uživatelské rozhraní (UI) v nové portálu, který můžete použít k integraci s virtuální sítě, které jsou nasazeny pomocí modelu nasazení classic nebo modelu nasazení Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="212f4-108">The VNet Integration feature has a user interface (UI) in the new portal that you can use to integrate with virtual networks that are deployed by using either the classic deployment model or the Azure Resource Manager deployment model.</span></span> <span data-ttu-id="212f4-109">Pokud chcete získat další informace o funkci, přečtěte si téma [integraci aplikace s virtuální sítě Azure](web-sites-integrate-with-vnet.md).</span><span class="sxs-lookup"><span data-stu-id="212f4-109">If you want to learn more about the feature, see [Integrate your app with an Azure virtual network](web-sites-integrate-with-vnet.md).</span></span>

<span data-ttu-id="212f4-110">Tento článek ne tom, jak pomocí uživatelského rozhraní, ale spíš o tom, jak povolit integraci pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="212f4-110">This article is not about how to use the UI but rather about how to enable integration by using PowerShell.</span></span> <span data-ttu-id="212f4-111">Protože příkazy pro každý model nasazení se liší, tento článek obsahuje oddíl pro každý model nasazení.</span><span class="sxs-lookup"><span data-stu-id="212f4-111">Because the commands for each deployment model are different, this article has a section for each deployment model.</span></span>  

<span data-ttu-id="212f4-112">Než budete pokračovat v tomto článku, ujistěte se, zda máte:</span><span class="sxs-lookup"><span data-stu-id="212f4-112">Before you continue with this article, ensure that you have:</span></span>

* <span data-ttu-id="212f4-113">Nejnovější nainstalovat Azure PowerShell SDK.</span><span class="sxs-lookup"><span data-stu-id="212f4-113">The latest Azure PowerShell SDK installed.</span></span> <span data-ttu-id="212f4-114">To můžete nainstalovat pomocí instalačního programu webové platformy.</span><span class="sxs-lookup"><span data-stu-id="212f4-114">You can install this with the Web Platform Installer.</span></span>
* <span data-ttu-id="212f4-115">Aplikace v Azure App Service spuštěna v Standard nebo Premium SKU.</span><span class="sxs-lookup"><span data-stu-id="212f4-115">An app in Azure App Service running in a Standard or Premium SKU.</span></span>

## <a name="classic-virtual-networks"></a><span data-ttu-id="212f4-116">Klasické virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="212f4-116">Classic virtual networks</span></span>
<span data-ttu-id="212f4-117">Tato část popisuje tři úlohy pro virtuální sítě, které používají model nasazení classic:</span><span class="sxs-lookup"><span data-stu-id="212f4-117">This section explains three tasks for virtual networks that use the classic deployment model:</span></span>

1. <span data-ttu-id="212f4-118">Připojení aplikace k dříve existující virtuální síť, která má bránu a je nakonfigurován pro připojení point-to-site.</span><span class="sxs-lookup"><span data-stu-id="212f4-118">Connect your app to a preexisting virtual network that has a gateway and is configured for point-to-site connectivity.</span></span>
2. <span data-ttu-id="212f4-119">Aktualizujte informace integrace virtuální sítě pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="212f4-119">Update your virtual network integration information for your app.</span></span>
3. <span data-ttu-id="212f4-120">Aplikace se odpojte od vaší virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="212f4-120">Disconnect your app from your virtual network.</span></span>

### <a name="connect-an-app-to-a-classic-vnet"></a><span data-ttu-id="212f4-121">Připojení aplikace k klasické virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="212f4-121">Connect an app to a classic VNet</span></span>
<span data-ttu-id="212f4-122">Chcete-li aplikaci připojit k virtuální síti, postupujte tyto tři kroky:</span><span class="sxs-lookup"><span data-stu-id="212f4-122">To connect an app to a virtual network, follow these three steps:</span></span>

1. <span data-ttu-id="212f4-123">Do webové aplikace deklarujte, že se připojení k virtuální síti konkrétní.</span><span class="sxs-lookup"><span data-stu-id="212f4-123">Declare to the web app that it will be joining a particular virtual network.</span></span> <span data-ttu-id="212f4-124">Aplikace vygeneruje certifikát, který bude mu udělená do virtuální sítě pro připojení point-to-site.</span><span class="sxs-lookup"><span data-stu-id="212f4-124">The app will generate a certificate that will be given to the virtual network for point-to-site connectivity.</span></span>
2. <span data-ttu-id="212f4-125">Nahrajte certifikát webové aplikace do virtuální sítě a následně načíst balíček point-to-site VPN identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="212f4-125">Upload the web app certificate to the virtual network, and then retrieve the point-to-site VPN package URI.</span></span>
3. <span data-ttu-id="212f4-126">Aktualizujte připojení virtuální sítě do webové aplikace s tímto balíčkem point-to-site identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="212f4-126">Update the web app's virtual network connection with the point-to-site package URI.</span></span>

<span data-ttu-id="212f4-127">První a třetí kroky jsou plně skriptovatelnou, ale druhý krok vyžaduje jednorázové, ruční akce prostřednictvím portálu nebo přístup k provedení **PUT** nebo **oprava** akce v koncovém bodě virtuální sítě Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="212f4-127">The first and third steps are fully scriptable, but the second step requires a one-time, manual action through the portal, or access to perform **PUT** or **PATCH** actions on the virtual network Azure Resource Manager endpoint.</span></span> <span data-ttu-id="212f4-128">Kontaktujte podporu Azure tak, aby měl tuto funkci povolíte.</span><span class="sxs-lookup"><span data-stu-id="212f4-128">Contact Azure Support to have this enabled.</span></span> <span data-ttu-id="212f4-129">Než začnete, ujistěte se, že máte klasickou virtuální síť, který má připojení point-to-site již povolené a nasazené brány.</span><span class="sxs-lookup"><span data-stu-id="212f4-129">Before you start, make sure that you have a classic virtual network that has point-to-site connectivity already enabled and a deployed gateway.</span></span> <span data-ttu-id="212f4-130">K vytvoření brány a povolíte připojení point-to-site, budete muset použít na portálu, jak je popsáno v [vytvoření brány VPN][createvpngateway].</span><span class="sxs-lookup"><span data-stu-id="212f4-130">To create the gateway and enable point-to-site connectivity, you need to use the portal as described at [Creating a VPN gateway][createvpngateway].</span></span>

<span data-ttu-id="212f4-131">Klasickou virtuální síť musí být ve stejném předplatném jako plán služby App Service, který obsahuje aplikace, budete nástroj integrovat.</span><span class="sxs-lookup"><span data-stu-id="212f4-131">The classic virtual network needs to be in the same subscription as your App Service plan that holds the app that you are integrating with.</span></span>

##### <a name="set-up-azure-powershell-sdk"></a><span data-ttu-id="212f4-132">Nastavení Azure PowerShell SDK</span><span class="sxs-lookup"><span data-stu-id="212f4-132">Set up Azure PowerShell SDK</span></span>
<span data-ttu-id="212f4-133">Otevřete okno prostředí PowerShell a nastavit váš účet a předplatné Azure pomocí:</span><span class="sxs-lookup"><span data-stu-id="212f4-133">Open a PowerShell window and set up your Azure account and subscription by using:</span></span>

    Login-AzureRmAccount

<span data-ttu-id="212f4-134">Tento příkaz se otevře okno s výzvou k získání přihlašovacích údajů Azure.</span><span class="sxs-lookup"><span data-stu-id="212f4-134">That command will open a prompt to get your Azure credentials.</span></span> <span data-ttu-id="212f4-135">Po přihlášení, použijte některý z následujících příkazů a vyberte předplatné, které chcete použít.</span><span class="sxs-lookup"><span data-stu-id="212f4-135">After you sign in, use either of the following commands to select the subscription that you want to use.</span></span> <span data-ttu-id="212f4-136">Ujistěte se, že používáte předplatné, které virtuální sítě a plán služby App Service jsou v.</span><span class="sxs-lookup"><span data-stu-id="212f4-136">Make sure that you are using the subscription that your virtual network and App Service plan are in.</span></span>

    Select-AzureRmSubscription –SubscriptionName [WebAppSubscriptionName]

<span data-ttu-id="212f4-137">nebo</span><span class="sxs-lookup"><span data-stu-id="212f4-137">or</span></span>

    Select-AzureRmSubscription –SubscriptionId [WebAppSubscriptionId]

##### <a name="variables-used-in-this-article"></a><span data-ttu-id="212f4-138">Proměnné používané v tomto článku</span><span class="sxs-lookup"><span data-stu-id="212f4-138">Variables used in this article</span></span>
<span data-ttu-id="212f4-139">Pro zjednodušení příkazy, se nastaví **$Configuration** proměnnou prostředí PowerShell s konkrétní konfigurací.</span><span class="sxs-lookup"><span data-stu-id="212f4-139">To simplify commands, we will set a **$Configuration** PowerShell variable with the specific configuration.</span></span>

<span data-ttu-id="212f4-140">Nastavte proměnnou následujícím způsobem v prostředí PowerShell s následujícími parametry:</span><span class="sxs-lookup"><span data-stu-id="212f4-140">Set a variable as follows in PowerShell with the following parameters:</span></span>

    $Configuration = @{}
    $Configuration.WebAppResourceGroup = "[Your web app resource group]"
    $Configuration.WebAppName = "[Your web app name]"
    $Configuration.VnetSubscriptionId = "[Your vnet subscription id]"
    $Configuration.VnetResourceGroup = "[Your vnet resource group]"
    $Configuration.VnetName = "[Your vnet name]"

<span data-ttu-id="212f4-141">Umístění aplikace musí být umístění bez žádné mezery.</span><span class="sxs-lookup"><span data-stu-id="212f4-141">The app location should be the location without any spaces.</span></span> <span data-ttu-id="212f4-142">Westus je například západní USA.</span><span class="sxs-lookup"><span data-stu-id="212f4-142">For example, West US is westus.</span></span>

    $Configuration.WebAppLocation = "[Your web app Location]"

<span data-ttu-id="212f4-143">Na další položku je zápis certifikátu.</span><span class="sxs-lookup"><span data-stu-id="212f4-143">The next item is where the certificate should be written.</span></span> <span data-ttu-id="212f4-144">By mělo být zapisovatelnou cestu v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="212f4-144">It should be a writable path on your local computer.</span></span> <span data-ttu-id="212f4-145">Nezapomeňte zahrnout .cer na konci.</span><span class="sxs-lookup"><span data-stu-id="212f4-145">Make sure to include .cer at the end.</span></span>

    $Configuration.GeneratedCertificatePath = "[C:\Path\To\Certificate.cer]"

<span data-ttu-id="212f4-146">Pokud chcete zobrazit, co nastavíte, zadejte **$Configuration**.</span><span class="sxs-lookup"><span data-stu-id="212f4-146">To see what you set, type **$Configuration**.</span></span>

    > $Configuration

    Name                           Value
    ----                           -----
    GeneratedCertificatePath       C:\vnetcert.cer
    VnetSubscriptionId             efc239a4-88f9-2c5e-a9a1-3034c21ad496
    WebAppResourceGroup            vnetdemo-rg
    VnetResourceGroup              testase1-rg
    VnetName                       TestNetwork
    WebAppName                     vnetintdemoapp
    WebAppLocation                 centralus

<span data-ttu-id="212f4-147">Zbývající část tohoto oddílu předpokládá, že máte proměnné vytvořené jako právě popsané.</span><span class="sxs-lookup"><span data-stu-id="212f4-147">The rest of this section assumes that you have a variable created as just described.</span></span>

##### <a name="declare-the-virtual-network-to-the-app"></a><span data-ttu-id="212f4-148">Deklarovat virtuální sítě do aplikace</span><span class="sxs-lookup"><span data-stu-id="212f4-148">Declare the virtual network to the app</span></span>
<span data-ttu-id="212f4-149">Použijte následující příkaz aplikace říct, že bude pomocí této konkrétní virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="212f4-149">Use the following command to tell the app that it will be using this particular virtual network.</span></span> <span data-ttu-id="212f4-150">To způsobí, že aplikace ke generování potřebné certifikáty:</span><span class="sxs-lookup"><span data-stu-id="212f4-150">This will cause the app to generate necessary certificates:</span></span>

    $vnet = New-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -PropertyObject @{"VnetResourceId" = "/subscriptions/$($Configuration.VnetSubscriptionId)/resourceGroups/$($Configuration.VnetResourceGroup)/providers/Microsoft.ClassicNetwork/virtualNetworks/$($Configuration.VnetName)"} -Location $Configuration.WebAppLocation -ApiVersion 2015-07-01

<span data-ttu-id="212f4-151">Pokud tento příkaz úspěšné, **$vnet** by měl mít **vlastnosti** proměnné v ní.</span><span class="sxs-lookup"><span data-stu-id="212f4-151">If this command succeeds, **$vnet** should have a **Properties** variable in it.</span></span> <span data-ttu-id="212f4-152">**Vlastnosti** proměnná musí obsahovat kryptografický otisk certifikátu a data certifikátu.</span><span class="sxs-lookup"><span data-stu-id="212f4-152">The **Properties** variable should contain both a certificate thumbprint and the certificate data.</span></span>

##### <a name="upload-the-web-app-certificate-to-the-virtual-network"></a><span data-ttu-id="212f4-153">Nahrajte certifikát webové aplikace do virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="212f4-153">Upload the web app certificate to the virtual network</span></span>
<span data-ttu-id="212f4-154">Ruční, jednorázové krok je vyžadován pro každé předplatné a kombinace virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="212f4-154">A manual, one-time step is required for each subscription and virtual network combination.</span></span> <span data-ttu-id="212f4-155">To znamená pokud se připojujete aplikací v rámci předplatného A do virtuální sítě A, musíte provést tento krok jenom po bez ohledu na to, kolik aplikací je nakonfigurovat.</span><span class="sxs-lookup"><span data-stu-id="212f4-155">That is, if you are connecting apps in Subscription A to Virtual Network A, you will need to do this step only once regardless of how many apps you configure.</span></span> <span data-ttu-id="212f4-156">Chcete-li přidat novou aplikaci s jinou virtuální sítí, budete muset udělat to znova.</span><span class="sxs-lookup"><span data-stu-id="212f4-156">If you are adding a new app to another virtual network, you'll need to do this again.</span></span> <span data-ttu-id="212f4-157">Důvodem je, že sadu certifikáty se generuje na úrovni předplatného v Azure App Service a sada je generována jednou pro každou virtuální síť, která aplikace se budou připojovat k.</span><span class="sxs-lookup"><span data-stu-id="212f4-157">The reason for this is that a set of certificates is generated at a subscription level in Azure App Service, and the set is generated once for each virtual network that the apps will connect to.</span></span>

<span data-ttu-id="212f4-158">Certifikáty budou mít již byla nastavena Pokud jste postupovali podle těchto kroků, nebo pokud je spojen s stejné virtuální síti pomocí portálu.</span><span class="sxs-lookup"><span data-stu-id="212f4-158">The certificates will have already been set if you followed these steps or if you integrated with the same virtual network by using the portal.</span></span>

<span data-ttu-id="212f4-159">Prvním krokem je generovat soubor .cer.</span><span class="sxs-lookup"><span data-stu-id="212f4-159">The first step is to generate the .cer file.</span></span> <span data-ttu-id="212f4-160">Druhým krokem je pro nahrání souboru .cer k virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="212f4-160">The second step is to upload the .cer file to your virtual network.</span></span> <span data-ttu-id="212f4-161">Vygenerujte soubor .cer z volání rozhraní API v předchozím kroku, spusťte následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="212f4-161">To generate the .cer file from the API call in the earlier step, run the following commands.</span></span>

    $certBytes = [System.Convert]::FromBase64String($vnet.Properties.certBlob)
    [System.IO.File]::WriteAllBytes("$($Configuration.GeneratedCertificatePath)", $certBytes)

<span data-ttu-id="212f4-162">Certifikát se budou nacházet v umístění, **$Configuration.GeneratedCertificatePath** určuje.</span><span class="sxs-lookup"><span data-stu-id="212f4-162">The certificate will be found in the location that **$Configuration.GeneratedCertificatePath** specifies.</span></span>

<span data-ttu-id="212f4-163">Chcete-li nahrát na server certifikát ručně, použijte [portál Azure] [ azureportal] a **procházet virtuální sítě (klasické)** > **připojení k síti VPN** > **Point-to-site** > **spravovat certifikáty**.</span><span class="sxs-lookup"><span data-stu-id="212f4-163">To upload the certificate manually, use the [Azure portal][azureportal] and **Browse Virtual Network (classic)** > **VPN connections** > **Point-to-site** > **Manage certificates**.</span></span> <span data-ttu-id="212f4-164">Tady odeslání vašeho certifikátu.</span><span class="sxs-lookup"><span data-stu-id="212f4-164">From here, upload your certificate.</span></span>

##### <a name="get-the-point-to-site-package"></a><span data-ttu-id="212f4-165">Načtení balíčku point-to-site</span><span class="sxs-lookup"><span data-stu-id="212f4-165">Get the point-to-site package</span></span>
<span data-ttu-id="212f4-166">Dalším krokem při nastavování připojení virtuální sítě ve webové aplikaci je získat balíček point-to-site a poskytnout ho do vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="212f4-166">The next step in setting up a virtual network connection on a web app is to get the point-to-site package and provide it to your web app.</span></span>

<span data-ttu-id="212f4-167">Uložte následující šablonu do souboru s názvem GetNetworkPackageUri.json někde v počítači, například C:\Azure\Templates\GetNetworkPackageUri.json.</span><span class="sxs-lookup"><span data-stu-id="212f4-167">Save the following template to a file called GetNetworkPackageUri.json somewhere on your computer, for example, C:\Azure\Templates\GetNetworkPackageUri.json.</span></span>

    {
        "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "certData": {
                "type": "string"
            },
            "certThumbprint": {
                "type": "string"
            },
            "networkName": {
                "type": "string"
            }
        },
        "variables": {
            "legacyVnetName": "[concat('Group ', resourceGroup().name, ' ', parameters('networkName'))]"
            },
            "resources": [
            ],
        "outputs" : {
            "PackageUri" :
            {
            "value" : "[listPackage(resourceId('Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRootCertificates', parameters('networkName'), 'primary', parameters('certThumbprint')), '2014-06-01').packageUri]", "type" : "string"
            }
        }
    }


<span data-ttu-id="212f4-168">Nastavte vstupní parametry:</span><span class="sxs-lookup"><span data-stu-id="212f4-168">Set input parameters:</span></span>

    $parameters = @{"certData" = $vnet.Properties.certBlob ;
    certThumbprint = $vnet.Properties.certThumbprint ;
    "networkName" = $Configuration.VnetName }

<span data-ttu-id="212f4-169">Volání skript:</span><span class="sxs-lookup"><span data-stu-id="212f4-169">Call the script:</span></span>

    $output = New-AzureRmResourceGroupDeployment -Name unused -ResourceGroupName $Configuration.VnetResourceGroup -TemplateParameterObject $parameters -TemplateFile C:\PATH\TO\GetNetworkPackageUri.json


<span data-ttu-id="212f4-170">Proměnná **$output. Outputs.packageUri** bude teď obsahovat balíček URI má být poskytnut do vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="212f4-170">The variable **$output.Outputs.packageUri** will now contain the package URI to be given to your web app.</span></span>

##### <a name="upload-the-point-to-site-package-to-your-app"></a><span data-ttu-id="212f4-171">Nahrání balíčku point-to-site do vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="212f4-171">Upload the point-to-site package to your app</span></span>
<span data-ttu-id="212f4-172">V posledním kroku je zajistit aplikace s tímto balíčkem.</span><span class="sxs-lookup"><span data-stu-id="212f4-172">The final step is to provide the app with this package.</span></span> <span data-ttu-id="212f4-173">Jednoduše spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="212f4-173">Simply run the next command:</span></span>

    $vnet = New-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)/primary" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections/gateways" -ApiVersion 2015-07-01 -PropertyObject @{"VnetName" = $Configuration.VnetName ; "VpnPackageUri" = $($output.Outputs.packageUri).Value } -Location $Configuration.WebAppLocation

<span data-ttu-id="212f4-174">Pokud se výzva k potvrzení, že se přepsal existující prostředek, zajistěte, aby tak, aby ji.</span><span class="sxs-lookup"><span data-stu-id="212f4-174">If a message asks you to confirm that you are overwriting an existing resource, make sure to allow it.</span></span>

<span data-ttu-id="212f4-175">Po úspěšném provedení tohoto příkazu, aplikace by teď připojený k virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="212f4-175">After this command succeeds, your app should now be connected to the virtual network.</span></span> <span data-ttu-id="212f4-176">Pokud chcete ověřit úspěch, přejděte do konzoly aplikace a zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="212f4-176">To confirm success, go to your app console, and type the following:</span></span>

    SET WEBSITE_

<span data-ttu-id="212f4-177">Pokud je proměnná prostředí s názvem WEBSITE_VNETNAME, který má hodnotu, která odpovídá názvu cílové virtuální síti, všechny konfigurace proběhlo úspěšně.</span><span class="sxs-lookup"><span data-stu-id="212f4-177">If there is an environment variable called WEBSITE_VNETNAME that has a value that matches the name of the target virtual network, all configurations have succeeded.</span></span>

### <a name="update-classic-vnet-integration-information"></a><span data-ttu-id="212f4-178">Aktualizovat informace integrace klasické virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="212f4-178">Update classic VNet integration information</span></span>
<span data-ttu-id="212f4-179">Aktualizace nebo nové synchronizace vaše informace, jednoduše zopakujte kroky, které jste udělali, když vytvoříte integraci na prvním místě.</span><span class="sxs-lookup"><span data-stu-id="212f4-179">To update or resync your information, simply repeat the steps that you followed when you created the integration in the first place.</span></span> <span data-ttu-id="212f4-180">Tyto kroky jsou:</span><span class="sxs-lookup"><span data-stu-id="212f4-180">Those steps are:</span></span>

1. <span data-ttu-id="212f4-181">Zadejte informace o konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="212f4-181">Define your configuration information.</span></span>
2. <span data-ttu-id="212f4-182">Deklarujte virtuální sítě do aplikace.</span><span class="sxs-lookup"><span data-stu-id="212f4-182">Declare the virtual network to the app.</span></span>
3. <span data-ttu-id="212f4-183">Načtení balíčku point-to-site.</span><span class="sxs-lookup"><span data-stu-id="212f4-183">Get the point-to-site package.</span></span>
4. <span data-ttu-id="212f4-184">Nahrání balíčku point-to-site do vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="212f4-184">Upload the point-to-site package to your app.</span></span>

### <a name="disconnect-your-app-from-a-classic-vnet"></a><span data-ttu-id="212f4-185">Odpojit aplikace od klasické virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="212f4-185">Disconnect your app from a classic VNet</span></span>
<span data-ttu-id="212f4-186">Chcete-li odpojit aplikace, musíte informace o konfiguraci, které bylo nastaveno během integrace virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="212f4-186">To disconnect the app, you need the configuration information that was set during virtual network integration.</span></span> <span data-ttu-id="212f4-187">Tyto informace používat, je pak jeden příkaz k odpojení vaší aplikace z vaší virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="212f4-187">Using that information, there is then one command to disconnect your app from your virtual network.</span></span>

    $vnet = Remove-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-07-01

## <a name="resource-manager-virtual-networks"></a><span data-ttu-id="212f4-188">Virtuální sítě Resource Manager</span><span class="sxs-lookup"><span data-stu-id="212f4-188">Resource Manager virtual networks</span></span>
<span data-ttu-id="212f4-189">Virtuální sítě Resource Manager mají rozhraní API Správce Azure Resource Manager, které zjednodušují některé procesy ve srovnání s klasické virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="212f4-189">Resource Manager virtual networks have Azure Resource Manager APIs, which simplify some processes when compared with classic virtual networks.</span></span> <span data-ttu-id="212f4-190">Máme skript, který vám pomůže dokončit následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="212f4-190">We have a script that will help you complete the following tasks:</span></span>

* <span data-ttu-id="212f4-191">Vytvoření virtuální sítě Resource Manager a integrovat vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="212f4-191">Create a Resource Manager virtual network and integrate your app with it.</span></span>
* <span data-ttu-id="212f4-192">Vytvořit bránu, nakonfigurujte připojení point-to-site v dříve existující virtuální sítě Resource Manager a pak zajistit integraci aplikace.</span><span class="sxs-lookup"><span data-stu-id="212f4-192">Create a gateway, configure point-to-site connectivity in a preexisting Resource Manager virtual network, and then integrate your app with it.</span></span>
* <span data-ttu-id="212f4-193">Integrate dříve existující virtuální sítě Resource Manager, která má brány a point-to-site povoleno připojení k vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="212f4-193">Integrate your app with a preexisting Resource Manager virtual network that has a gateway and point-to-site connectivity enabled.</span></span>
* <span data-ttu-id="212f4-194">Aplikace se odpojte od vaší virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="212f4-194">Disconnect your app from your virtual network.</span></span>

### <a name="resource-manager-vnet-app-service-integration-script"></a><span data-ttu-id="212f4-195">Správce prostředků virtuální sítě App Service integrace skriptu</span><span class="sxs-lookup"><span data-stu-id="212f4-195">Resource Manager VNet App Service integration script</span></span>
<span data-ttu-id="212f4-196">Zkopírujte následující skript a uložte ho do souboru.</span><span class="sxs-lookup"><span data-stu-id="212f4-196">Copy the following script and save it to a file.</span></span> <span data-ttu-id="212f4-197">Pokud nechcete použít skript, klidně si další informace z něj chcete zjistit, jak nastavit věcí s virtuální sítí Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="212f4-197">If you don’t want to use the script, feel free to learn from it to see how to set things up with a Resource Manager virtual network.</span></span>

    function ReadHostWithDefault($message, $default)
    {
        $result = Read-Host "$message [$default]"
        if($result -eq "")
        {
            $result = $default
        }
            return $result
        }

    function PromptCustom($title, $optionValues, $optionDescriptions)
    {
        Write-Host $title
        Write-Host
        $a = @()
        for($i = 0; $i -lt $optionValues.Length; $i++){
            Write-Host "$($i+1))" $optionDescriptions[$i]
        }
        Write-Host

        while($true)
        {
            Write-Host "Choose an option: "
            $option = Read-Host
            $option = $option -as [int]

            if($option -ge 1 -and $option -le $optionValues.Length)
            {
                return $optionValues[$option-1]
            }
        }
    }

    function PromptYesNo($title, $message, $default = 0)
    {
        $yes = New-Object System.Management.Automation.Host.ChoiceDescription "&Yes", ""
        $no = New-Object System.Management.Automation.Host.ChoiceDescription "&No", ""
        $options = [System.Management.Automation.Host.ChoiceDescription[]]($yes, $no)
        $result = $host.ui.PromptForChoice($title, $message, $options, $default)
        return $result
    }

    function CreateVnet($resourceGroupName, $vnetName, $vnetAddressSpace, $vnetGatewayAddressSpace, $location)
    {
        Write-Host "Creating a new VNET"
        $gatewaySubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix $vnetGatewayAddressSpace
        New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $resourceGroupName -Location $location -AddressPrefix $vnetAddressSpace -Subnet $gatewaySubnet
    }

    function CreateVnetGateway($resourceGroupName, $vnetName, $vnetIpName, $location, $vnetIpConfigName, $vnetGatewayName, $certificateData, $vnetPointToSiteAddressSpace)
    {
        $vnet = Get-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $resourceGroupName
        $subnet=Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet

        Write-Host "Creating a public IP address for this VNET"
        $pip = New-AzureRmPublicIpAddress -Name $vnetIpName -ResourceGroupName $resourceGroupName -Location $location -AllocationMethod Dynamic
        $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $vnetIpConfigName -Subnet $subnet -PublicIpAddress $pip

        Write-Host "Adding a root certificate to this VNET"
        $root = New-AzureRmVpnClientRootCertificate -Name "AppServiceCertificate.cer" -PublicCertData $certificateData

        Write-Host "Creating Azure VNET Gateway. This may take up to an hour."
        New-AzureRmVirtualNetworkGateway -Name $vnetGatewayName -ResourceGroupName $resourceGroupName -Location $location -IpConfigurations $ipconf -GatewayType Vpn -VpnType RouteBased -EnableBgp $false -GatewaySku Basic -VpnClientAddressPool $vnetPointToSiteAddressSpace -VpnClientRootCertificates $root
    }

    function AddNewVnet($subscriptionId, $webAppResourceGroup, $webAppName)
    {
        Write-Host "Adding a new Vnet"
        Write-Host
        $vnetName = Read-Host "Specify a name for this Virtual Network"

        $vnetGatewayName="$($vnetName)-gateway"
        $vnetIpName="$($vnetName)-ip"
        $vnetIpConfigName="$($vnetName)-ipconf"

        # Virtual Network settings
        $vnetAddressSpace="10.0.0.0/8"
        $vnetGatewayAddressSpace="10.5.0.0/16"
        $vnetPointToSiteAddressSpace="172.16.0.0/16"

        $changeRequested = 0
        $resourceGroupName = $webAppResourceGroup

        while($changeRequested -eq 0)
        {
            Write-Host
            Write-Host "Currently, I will create a VNET with the following settings:"
            Write-Host
            Write-Host "Virtual Network Name: $vnetName"
            Write-Host "Resource Group Name:  $resourceGroupName"
            Write-Host "Gateway Name: $vnetGatewayName"
            Write-Host "Vnet IP name: $vnetIpName"
            Write-Host "Vnet IP config name:  $vnetIpConfigName"
            Write-Host "Address Space:$vnetAddressSpace"
            Write-Host "Gateway Address Space:$vnetGatewayAddressSpace"
            Write-Host "Point-To-Site Address Space:  $vnetPointToSiteAddressSpace"
            Write-Host
            $changeRequested = PromptYesNo "" "Do you wish to change these settings?" 1

            if($changeRequested -eq 0)
            {
                $vnetName = ReadHostWithDefault "Virtual Network Name" $vnetName
                $resourceGroupName = ReadHostWithDefault "Resource Group Name" $resourceGroupName
                $vnetGatewayName = ReadHostWithDefault "Vnet Gateway Name" $vnetGatewayName
                $vnetIpName = ReadHostWithDefault "Vnet IP name" $vnetIpName
                $vnetIpConfigName = ReadHostWithDefault "Vnet IP configuration name" $vnetIpConfigName
                $vnetAddressSpace = ReadHostWithDefault "Vnet Address Space" $vnetAddressSpace
                $vnetGatewayAddressSpace = ReadHostWithDefault "Vnet Gateway Address Space" $vnetGatewayAddressSpace
                $vnetPointToSiteAddressSpace = ReadHostWithDefault "Vnet Point-to-site Address Space" $vnetPointToSiteAddressSpace
            }
        }

        $ErrorActionPreference = "Stop";

        # We create the virtual network and add it here. The way this works is:
        # 1) Add the VNET association to the App. This allows the App to generate certificates, etc. for the VNET.
        # 2) Create the VNET and VNET gateway, add the certificates, create the public IP, etc., required for the gateway
        # 3) Get the VPN package from the gateway and pass it back to the App.

        $webApp = Get-AzureRmResource -ResourceName $webAppName -ResourceType "Microsoft.Web/sites" -ApiVersion 2015-08-01 -ResourceGroupName $webAppResourceGroup
        $location = $webApp.Location

        Write-Host "Creating App association to VNET"
        $propertiesObject = @{
         "vnetResourceId" = "/subscriptions/$($subscriptionId)/resourceGroups/$($resourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($vnetName)"
        }
        $virtualNetwork = New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnetName)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $webAppResourceGroup -Force

        CreateVnet $resourceGroupName $vnetName $vnetAddressSpace $vnetGatewayAddressSpace $location

        CreateVnetGateway $resourceGroupName $vnetName $vnetIpName $location $vnetIpConfigName $vnetGatewayName $virtualNetwork.Properties.CertBlob $vnetPointToSiteAddressSpace

        Write-Host "Retrieving VPN Package and supplying to App"
        $packageUri = Get-AzureRmVpnClientPackage -ResourceGroupName $resourceGroupName -VirtualNetworkGatewayName $vnetGatewayName -ProcessorArchitecture Amd64
        
        # $packageUri may contain literal double-quotes at the start and the end of the URL
        if($packageUri.Length -gt 0 -and $packageUri.Substring(0, 1) -eq '"' -and $packageUri.Substring($packageUri.Length - 1, 1) -eq '"')
        {
            $packageUri = $packageUri.Substring(1, $packageUri.Length - 2)
        }

        # Put the VPN client configuration package onto the App
        $PropertiesObject = @{
        "vnetName" = $VirtualNetworkName; "vpnPackageUri" = $packageUri
        }

        New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnetName)/primary" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections/gateways" -ApiVersion 2015-08-01 -ResourceGroupName $webAppResourceGroup -Force

        Write-Host "Finished!"
    }

    function AddExistingVnet($subscriptionId, $resourceGroupName, $webAppName)
    {
        $ErrorActionPreference = "Stop";

        # At this point, the gateway should be able to be joined to an App, but may require some minor tweaking. We will declare to the App now to use this VNET
        Write-Host "Getting App information"
        $webApp = Get-AzureRmResource -ResourceName $webAppName -ResourceType "Microsoft.Web/sites" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        $location = $webApp.Location

        $webAppConfig = Get-AzureRmResource -ResourceName "$($webAppName)/web" -ResourceType "Microsoft.Web/sites/config" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        $currentVnet = $webAppConfig.Properties.VnetName
        if($currentVnet -ne $null -and $currentVnet -ne "")
        {
            Write-Host "Currently connected to VNET $currentVnet"
        }

        # Display existing vnets
        $vnets = Get-AzureRmVirtualNetwork
        $vnetNames = @()
        foreach($vnet in $vnets)
        {
            $vnetNames += $vnet.Name
        }

        Write-Host
        $vnet = PromptCustom "Select a VNET to integrate with" $vnets $vnetNames

        # We need to check if this VNET is able to be joined to a App, based on following criteria
            # If there is no gateway, we can create one.
            # If there is a gateway:
                # It must be of type Vpn
                # It must be of VpnType RouteBased
                # If it doesn't have the right certificate, we will need to add it.
                # If it doesn't have a point-to-site range, we will need to add it.

        $gatewaySubnet = $vnet.Subnets | Where-Object { $_.Name -eq "GatewaySubnet" }

        if($gatewaySubnet -eq $null -or $gatewaySubnet.IpConfigurations -eq $null -or $gatewaySubnet.IpConfigurations.Count -eq 0)
        {
            $ErrorActionPreference = "Continue";
            # There is no gateway. We need to create one.
            Write-Host "This Virtual Network has no gateway. I will need to create one."

            $vnetName = $vnet.Name
            $vnetGatewayName="$($vnetName)-gateway"
            $vnetIpName="$($vnetName)-ip"
            $vnetIpConfigName="$($vnetName)-ipconf"

            # Virtual Network settings
            $vnetAddressSpace="10.0.0.0/8"
            $vnetGatewayAddressSpace="10.5.0.0/16"
            $vnetPointToSiteAddressSpace="172.16.0.0/16"

            $changeRequested = 0

            Write-Host "Your VNET is in the address space $($vnet.AddressSpace.AddressPrefixes), with the following Subnets:"
            foreach($subnet in $vnet.Subnets)
            {
                Write-Host "$($subnet.Name): $($subnet.AddressPrefix)"
            }

            $vnetGatewayAddressSpace = Read-Host "Please choose a GatewaySubnet address space"

            while($changeRequested -eq 0)
            {
                Write-Host
                Write-Host "Currently, I will create a VNET gateway with the following settings:"
                Write-Host
                Write-Host "Virtual Network Name: $vnetName"
                Write-Host "Resource Group Name:  $($vnet.ResourceGroupName)"
                Write-Host "Gateway Name: $vnetGatewayName"
                Write-Host "Vnet IP name: $vnetIpName"
                Write-Host "Vnet IP config name:  $vnetIpConfigName"
                Write-Host "Address Space:$($vnet.AddressSpace.AddressPrefixes)"
                Write-Host "Gateway Address Space:$vnetGatewayAddressSpace"
                Write-Host "Point-To-Site Address Space:  $vnetPointToSiteAddressSpace"
                Write-Host
                $changeRequested = PromptYesNo "" "Do you wish to change these settings?" 1

                if($changeRequested -eq 0)
                {
                    $vnetGatewayName = ReadHostWithDefault "Vnet Gateway Name" $vnetGatewayName
                    $vnetIpName = ReadHostWithDefault "Vnet IP name" $vnetIpName
                    $vnetIpConfigName = ReadHostWithDefault "Vnet IP configuration name" $vnetIpConfigName
                    $vnetGatewayAddressSpace = ReadHostWithDefault "Vnet Gateway Address Space" $vnetGatewayAddressSpace
                    $vnetPointToSiteAddressSpace = ReadHostWithDefault "Vnet Point-to-site Address Space" $vnetPointToSiteAddressSpace
                }
            }

            $ErrorActionPreference = "Stop";

            Write-Host "Creating App association to VNET"
            $propertiesObject = @{
             "vnetResourceId" = "/subscriptions/$($subscriptionId)/resourceGroups/$($vnet.ResourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($vnetName)"
            }

            $virtualNetwork = New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnet.Name)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName -Force

            # If there is no gateway subnet, we need to create one.
            if($gatewaySubnet -eq $null)
            {
                $gatewaySubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix $vnetGatewayAddressSpace
                $vnet.Subnets.Add($gatewaySubnet);
                Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
            }

            CreateVnetGateway $vnet.ResourceGroupName $vnetName $vnetIpName $location $vnetIpConfigName $vnetGatewayName $virtualNetwork.Properties.CertBlob $vnetPointToSiteAddressSpace

            $gateway = Get-AzureRmVirtualNetworkGateway -ResourceGroupName $vnet.ResourceGroupName -Name $vnetGatewayName
        }
        else
        {
            $uriParts = $gatewaySubnet.IpConfigurations[0].Id.Split('/')
            $gatewayResourceGroup = $uriParts[4]
            $gatewayName = $uriParts[8]

            $gateway = Get-AzureRmVirtualNetworkGateway -ResourceGroupName $vnet.ResourceGroupName -Name $gatewayName

            # validate gateway types, etc.
            if($gateway.GatewayType -ne "Vpn")
            {
                Write-Error "This gateway is not of the Vpn type. It cannot be joined to an App."
                return
            }

            if($gateway.VpnType -ne "RouteBased")
            {
                Write-Error "This gateways Vpn type is not RouteBased. It cannot be joined to an App."
                return
            }

            if($gateway.VpnClientConfiguration -eq $null -or $gateway.VpnClientConfiguration.VpnClientAddressPool -eq $null)
            {
                Write-Host "This gateway does not have a Point-to-site Address Range. Please specify one in CIDR notation, e.g. 10.0.0.0/8"
                $pointToSiteAddress = Read-Host "Point-To-Site Address Space"
                Set-AzureRmVirtualNetworkGatewayVpnClientConfig -VirtualNetworkGateway $gateway.Name -VpnClientAddressPool $pointToSiteAddress
            }

            Write-Host "Creating App association to VNET"
            $propertiesObject = @{
             "vnetResourceId" = "/subscriptions/$($subscriptionId)/resourceGroups/$($vnet.ResourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($vnet.Name)"
            }

            $virtualNetwork = New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnet.Name)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName -Force

            # We need to check if the certificate here exists in the gateway.
            $certificates = $gateway.VpnClientConfiguration.VpnClientRootCertificates

            $certFound = $false
            foreach($certificate in $certificates)
            {
                if($certificate.PublicCertData -eq $virtualNetwork.Properties.CertBlob)
                {
                    $certFound = $true
                    break
                }
            }

            if(-not $certFound)
            {
                Write-Host "Adding certificate"
                Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName "AppServiceCertificate.cer" -PublicCertData $virtualNetwork.Properties.CertBlob -VirtualNetworkGatewayName $gateway.Name
            }
        }

        # Now finish joining by getting the VPN package and giving it to the App
        Write-Host "Retrieving VPN Package and supplying to App"
        $packageUri = Get-AzureRmVpnClientPackage -ResourceGroupName $vnet.ResourceGroupName -VirtualNetworkGatewayName $gateway.Name -ProcessorArchitecture Amd64
        
        # $packageUri may contain literal double-quotes at the start and the end of the URL
        if($packageUri.Length -gt 0 -and $packageUri.Substring(0, 1) -eq '"' -and $packageUri.Substring($packageUri.Length - 1, 1) -eq '"')
        {
            $packageUri = $packageUri.Substring(1, $packageUri.Length - 2)
        }

        # Put the VPN client configuration package onto the App
        $PropertiesObject = @{
        "vnetName" = $vnet.Name; "vpnPackageUri" = $packageUri
        }

        New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnet.Name)/primary" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections/gateways" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName -Force

        Write-Host "Finished!"
    }

    function RemoveVnet($subscriptionId, $resourceGroupName, $webAppName)
    {
        $webAppConfig = Get-AzureRmResource -ResourceName "$($webAppName)/web" -ResourceType "Microsoft.Web/sites/config" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        $currentVnet = $webAppConfig.Properties.VnetName
        if($currentVnet -ne $null -and $currentVnet -ne "")
        {
            Write-Host "Currently connected to VNET $currentVnet"

            Remove-AzureRmResource -ResourceName "$($webAppName)/$($currentVnet)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        }
            else
        {
            Write-Host "Not connected to a VNET."
        }
    }

    Write-Host "Please Login"
    Login-AzureRmAccount

    # Choose subscription. If there's only one we will choose automatically

    $subs = Get-AzureRmSubscription
    $subscriptionId = ""

    if($subs.Length -eq 0)
    {
        Write-Error "No subscriptions bound to this account."
        return
    }

    if($subs.Length -eq 1)
    {
        $subscriptionId = $subs[0].SubscriptionId
    }
    else
    {
        $subscriptionChoices = @()
        $subscriptionValues = @()

        foreach($subscription in $subs){
        $subscriptionChoices += "$($subscription.SubscriptionName) ($($subscription.SubscriptionId))";
        $subscriptionValues += ($subscription.SubscriptionId);
        }

        $subscriptionId = PromptCustom "Choose a subscription" $subscriptionValues $subscriptionChoices
    }

    Select-AzureRmSubscription -SubscriptionId $subscriptionId

    $resourceGroup = Read-Host "Please enter the Resource Group of your App"

    $appName = Read-Host "Please enter the Name of your App"

    $options = @("Add a NEW Virtual Network to an App", "Add an EXISTING Virtual Network to an App", "Remove a Virtual Network from an App");
    $optionValues = @(0, 1, 2)
    $option = PromptCustom "What do you want to do?" $optionValues $options

    if($option -eq 0)
    {
        AddNewVnet $subscriptionId $resourceGroup $appName
    }
    if($option -eq 1)
    {
        AddExistingVnet $subscriptionId $resourceGroup $appName
    }
    if($option -eq 2)
    {
        RemoveVnet $subscriptionId $resourceGroup $appName
    }

<span data-ttu-id="212f4-198">Uložte kopii tohoto skriptu.</span><span class="sxs-lookup"><span data-stu-id="212f4-198">Save a copy of the script.</span></span> <span data-ttu-id="212f4-199">V tomto článku se označuje jako V2VnetAllinOne.ps1, ale můžete použít jiný název.</span><span class="sxs-lookup"><span data-stu-id="212f4-199">In this article, it is called V2VnetAllinOne.ps1, but you can use another name.</span></span> <span data-ttu-id="212f4-200">Neexistují žádné argumenty pro tento skript.</span><span class="sxs-lookup"><span data-stu-id="212f4-200">There are no arguments for this script.</span></span> <span data-ttu-id="212f4-201">Můžete jednoduše spusťte ji.</span><span class="sxs-lookup"><span data-stu-id="212f4-201">You simply run it.</span></span> <span data-ttu-id="212f4-202">První věcí, kterou skript provede je výzva, abyste se přihlásili.</span><span class="sxs-lookup"><span data-stu-id="212f4-202">The first thing the script will do is prompt you to sign in.</span></span> <span data-ttu-id="212f4-203">Po přihlášení, skript získá informace o vašem účtu a vrátí seznam předplatných.</span><span class="sxs-lookup"><span data-stu-id="212f4-203">After you sign in, the script gets details about your account and returns a list of subscriptions.</span></span> <span data-ttu-id="212f4-204">Není počítání žádost o zadání přihlašovacích údajů, provádění skriptu počáteční vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="212f4-204">Not counting the request for your credentials, the initial script execution looks like this:</span></span>

    PS C:\Users\ccompy\Documents\VNET> .\V2VnetAllInOne.ps1
    Please Login

    Environment           : AzureCloud
    Account               : ccompy@microsoft.com
    TenantId              : 722278f-fef1-499f-91ab-2323d011db47
    SubscriptionId        : af5358e1-acac-2c90-a9eb-722190abf47a
    CurrentStorageAccount :

    Choose a subscription

    1) <span data-ttu-id="212f4-205">Ukázkový předplatné (af5358e1-acac-2c90-a9eb-722190abf47a)</span><span class="sxs-lookup"><span data-stu-id="212f4-205">Demo Subscription (af5358e1-acac-2c90-a9eb-722190abf47a)</span></span>
    2) <span data-ttu-id="212f4-206">Test MS (a5350f55-dd5a-41ec-2ddb-ff7b911bb2ef)</span><span class="sxs-lookup"><span data-stu-id="212f4-206">MS Test (a5350f55-dd5a-41ec-2ddb-ff7b911bb2ef)</span></span>
    3) <span data-ttu-id="212f4-207">Předplatné fialové ukázku (2d4c99a4-57f9-4d5e-a0a1-0034c52db59d)</span><span class="sxs-lookup"><span data-stu-id="212f4-207">Purple Demo Subscription (2d4c99a4-57f9-4d5e-a0a1-0034c52db59d)</span></span>

    <span data-ttu-id="212f4-208">Vyberte možnost: 3</span><span class="sxs-lookup"><span data-stu-id="212f4-208">Choose an option: 3</span></span>

    <span data-ttu-id="212f4-209">Účet: ccompy@microsoft.com prostředí: AzureCloud předplatné: 2d4c99a4-57f9-4d5e-a0a1-0034c52db59d klienta: 722278f-fef1-499f-91ab-2323d011db47</span><span class="sxs-lookup"><span data-stu-id="212f4-209">Account      : ccompy@microsoft.com Environment  : AzureCloud Subscription : 2d4c99a4-57f9-4d5e-a0a1-0034c52db59d Tenant       : 722278f-fef1-499f-91ab-2323d011db47</span></span>

    <span data-ttu-id="212f4-210">Zadejte prosím skupinu prostředků vaší aplikace: hcdemo-rg, zadejte název vaší aplikace: v2vnetpowershell co chcete udělat?</span><span class="sxs-lookup"><span data-stu-id="212f4-210">Please enter the Resource Group of your App: hcdemo-rg Please enter the Name of your App: v2vnetpowershell What do you want to do?</span></span>

    1) <span data-ttu-id="212f4-211">Přidejte nové virtuální sítě do aplikace</span><span class="sxs-lookup"><span data-stu-id="212f4-211">Add a NEW Virtual Network to an App</span></span>
    2) <span data-ttu-id="212f4-212">Přidejte existující virtuální sítě do aplikace</span><span class="sxs-lookup"><span data-stu-id="212f4-212">Add an EXISTING Virtual Network to an App</span></span>
    3) <span data-ttu-id="212f4-213">Odebrat virtuální sítě z aplikace.</span><span class="sxs-lookup"><span data-stu-id="212f4-213">Remove a Virtual Network from an App</span></span>

<span data-ttu-id="212f4-214">Zbývající část Tato část vysvětluje, každá z těchto tří možností.</span><span class="sxs-lookup"><span data-stu-id="212f4-214">The rest of this section explains each of those three options.</span></span>

### <a name="create-a-resource-manager-vnet-and-integrate-with-it"></a><span data-ttu-id="212f4-215">Vytvoření virtuální sítě Resource Manager a integrovat s ním</span><span class="sxs-lookup"><span data-stu-id="212f4-215">Create a Resource Manager VNet and integrate with it</span></span>
<span data-ttu-id="212f4-216">Chcete-li vytvořit novou virtuální síť, která používá model nasazení Resource Manager a integrovat s vaší aplikací, vyberte **1) přidejte nové virtuální sítě do aplikace**.</span><span class="sxs-lookup"><span data-stu-id="212f4-216">To create a new virtual network that uses the Resource Manager deployment model and integrate it with your app, select **1) Add a NEW Virtual Network to an App**.</span></span> <span data-ttu-id="212f4-217">To vás vyzve k zadání názvu virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="212f4-217">This will prompt you for the name of the virtual network.</span></span> <span data-ttu-id="212f4-218">V mé případu jak můžete vidět v následujícím nastavení, byl použit název v2pshell.</span><span class="sxs-lookup"><span data-stu-id="212f4-218">In my case, as you can see in the following settings, I used the name, v2pshell.</span></span>

<span data-ttu-id="212f4-219">Skript poskytuje podrobnosti o virtuální síť, která je právě vytvářena.</span><span class="sxs-lookup"><span data-stu-id="212f4-219">The script gives the details about the virtual network that's being created.</span></span> <span data-ttu-id="212f4-220">Pokud chcete, můžete změnit všechny hodnoty</span><span class="sxs-lookup"><span data-stu-id="212f4-220">If I want, I can change any of the values.</span></span> <span data-ttu-id="212f4-221">Při provádění tohoto příkladu vytvořené virtuální síti, která má následující nastavení:</span><span class="sxs-lookup"><span data-stu-id="212f4-221">In this example execution, I created a virtual network that has the following settings:</span></span>

    Virtual Network Name:         v2pshell
    Resource Group Name:          hcdemo-rg
    Gateway Name:                 v2pshell-gateway
    Vnet IP name:                 v2pshell-ip
    Vnet IP config name:          v2pshell-ipconf
    Address Space:                10.0.0.0/8
    Gateway Address Space:        10.5.0.0/16
    Point-To-Site Address Space:  172.16.0.0/12

    Do you wish to change these settings?
    [Y] Yes  [N] No  [?] Help (default is "N"):

<span data-ttu-id="212f4-222">Pokud chcete změnit hodnoty, zadejte **Y** a proveďte požadované změny.</span><span class="sxs-lookup"><span data-stu-id="212f4-222">If you want to change any of the values, type **Y** and make the changes.</span></span> <span data-ttu-id="212f4-223">Pokud jste radostí s nastavení virtuální sítě, zadejte **N** nebo jednoduše stiskněte klávesu Enter při zobrazení výzvy týkající se změna nastavení.</span><span class="sxs-lookup"><span data-stu-id="212f4-223">When you are happy with the virtual network settings, type **N** or simply press Enter when prompted about changing the settings.</span></span> <span data-ttu-id="212f4-224">Z tohoto místa na až do dokončení skriptu, která vám sdělí některé co IT oddělení "i's provádění, dokud není zahájena vytvořit bránu virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="212f4-224">From there on until completion, the script will tell you some of what it' i's doing until it starts to create the virtual network gateway.</span></span> <span data-ttu-id="212f4-225">Tento krok může trvat až jednu hodinu.</span><span class="sxs-lookup"><span data-stu-id="212f4-225">That step can take up to an hour.</span></span> <span data-ttu-id="212f4-226">V průběhu této fáze neexistuje žádné indikátor průběhu, ale skript vám umožní vědět, kdy byla vytvořena brány.</span><span class="sxs-lookup"><span data-stu-id="212f4-226">There is no progress indicator during this phase, but the script will let you know when the gateway has been created.</span></span>

<span data-ttu-id="212f4-227">Po dokončení skriptu se dozvíte **dokončeno**.</span><span class="sxs-lookup"><span data-stu-id="212f4-227">When the script finishes, it will say **Finished**.</span></span> <span data-ttu-id="212f4-228">V tuto chvíli máte virtuální sítě Resource Manager, která má název a nastavení, které jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="212f4-228">At this point, you will have a Resource Manager virtual network that has the name and settings that you selected.</span></span> <span data-ttu-id="212f4-229">Toto nové virtuální sítě se také integrovat s vaší aplikací.</span><span class="sxs-lookup"><span data-stu-id="212f4-229">This new virtual network will also be integrated with your app.</span></span>

### <a name="integrate-your-app-with-a-preexisting-resource-manager-vnet"></a><span data-ttu-id="212f4-230">Integrace aplikace s dříve existující virtuální sítě Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="212f4-230">Integrate your app with a preexisting Resource Manager VNet</span></span>
<span data-ttu-id="212f4-231">Pokud se integrace s dříve existující virtuální sítě, pokud jste zadali virtuální sítě Resource Manager, která nemá brána nebo připojení point-to-site, skript bude tomuto nastavení.</span><span class="sxs-lookup"><span data-stu-id="212f4-231">When you're integrating with a preexisting virtual network, if you provide a Resource Manager virtual network that doesn’t have a gateway or point-to-site connectivity, the script will set that up.</span></span> <span data-ttu-id="212f4-232">Pokud síť VNET už má tyto věci nastavit, skript přejde přímo na integraci aplikací.</span><span class="sxs-lookup"><span data-stu-id="212f4-232">If the VNET already has those things set up, the script goes straight to the app integration.</span></span> <span data-ttu-id="212f4-233">Tento proces zahájíte jednoduše vybrat **2) přidejte existující virtuální síť do aplikace**.</span><span class="sxs-lookup"><span data-stu-id="212f4-233">To start this process, simply select **2) Add an EXISTING Virtual Network to an App**.</span></span>

<span data-ttu-id="212f4-234">Tato možnost funguje jenom v případě, že máte dříve existující virtuální síť Resource Manager, který je ve stejném předplatném jako aplikace.</span><span class="sxs-lookup"><span data-stu-id="212f4-234">This option works only if you have a preexisting Resource Manager virtual network that is in the same subscription as your app.</span></span> <span data-ttu-id="212f4-235">Jakmile vyberete možnost, zobrazí se seznam virtuálních sítí Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="212f4-235">After you select the option, you will be presented with a list of your Resource Manager virtual networks.</span></span>   

    Select a VNET to integrate with

    1) <span data-ttu-id="212f4-236">v2demonetwork</span><span class="sxs-lookup"><span data-stu-id="212f4-236">v2demonetwork</span></span>
    2) <span data-ttu-id="212f4-237">v2pshell</span><span class="sxs-lookup"><span data-stu-id="212f4-237">v2pshell</span></span>
    3) <span data-ttu-id="212f4-238">v2vnetintdemo</span><span class="sxs-lookup"><span data-stu-id="212f4-238">v2vnetintdemo</span></span>
    4) <span data-ttu-id="212f4-239">v2asenetwork</span><span class="sxs-lookup"><span data-stu-id="212f4-239">v2asenetwork</span></span>
    5) <span data-ttu-id="212f4-240">v2pshell2</span><span class="sxs-lookup"><span data-stu-id="212f4-240">v2pshell2</span></span>

    <span data-ttu-id="212f4-241">Vyberte možnost: 5</span><span class="sxs-lookup"><span data-stu-id="212f4-241">Choose an option: 5</span></span>

<span data-ttu-id="212f4-242">Jednoduše vyberte virtuální síť, který chcete integrovat.</span><span class="sxs-lookup"><span data-stu-id="212f4-242">Simply select the virtual network that you want to integrate with.</span></span> <span data-ttu-id="212f4-243">Pokud již máte bránu, která má povolené připojení point-to-site, skript aplikace jednoduše integrovat s vaší virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="212f4-243">If you already have a gateway that has point-to-site connectivity enabled, the script will simply integrate your app with your virtual network.</span></span> <span data-ttu-id="212f4-244">Pokud jste bránu, budete muset zadat podsíť brány.</span><span class="sxs-lookup"><span data-stu-id="212f4-244">If you do not have a gateway, you will need to specify the gateway subnet.</span></span> <span data-ttu-id="212f4-245">Podsíť brány musí být v adresní prostor vaší virtuální sítě a nemůže být v jiné podsíti.</span><span class="sxs-lookup"><span data-stu-id="212f4-245">Your gateway subnet must be in your virtual network address space, and it cannot be in any other subnet.</span></span> <span data-ttu-id="212f4-246">Pokud máte virtuální síť bez brány a spusťte tento krok, věcí vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="212f4-246">If you have a virtual network without a gateway and run this step, things look like this:</span></span>

    This Virtual Network has no gateway. I will need to create one.
    Your VNET is in the address space 172.16.0.0/16, with the following Subnets:
    default: 172.16.0.0/24
    Please choose a GatewaySubnet address space: 172.16.1.0/26

<span data-ttu-id="212f4-247">V tomto příkladu vytvoříme bránu virtuální sítě, který má následující nastavení:</span><span class="sxs-lookup"><span data-stu-id="212f4-247">In this example, I created a virtual network gateway that has the following settings:</span></span>

    Virtual Network Name:         v2pshell2
    Resource Group Name:          vnetdemo-rg
    Gateway Name:                 v2pshell2-gateway
    Vnet IP name:                 v2pshell2-ip
    Vnet IP config name:          v2pshell2-ipconf
    Address Space:                172.16.0.0/16
    Gateway Address Space:        172.16.1.0/26
    Point-To-Site Address Space:  172.16.0.0/12

    Do you wish to change these settings?
    [Y] Yes  [N] No  [?] Help (default is "N"):
    Creating App association to VNET

<span data-ttu-id="212f4-248">Pokud chcete některé z těchto nastavení změnit, můžete tak učinit.</span><span class="sxs-lookup"><span data-stu-id="212f4-248">If you want to change any of those settings, you can do so.</span></span> <span data-ttu-id="212f4-249">Jinak, stiskněte klávesu Enter a skript vytvoří brány a připojení aplikace k virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="212f4-249">Otherwise, press Enter and the script will create your gateway and attach your app to your virtual network.</span></span> <span data-ttu-id="212f4-250">Čas vytvoření brány je stále hodinu, přestože, proto se ujistěte, že byste, mít na paměti.</span><span class="sxs-lookup"><span data-stu-id="212f4-250">The gateway creation time is still an hour, though, so make sure you keep that in mind.</span></span> <span data-ttu-id="212f4-251">Po dokončení všechno, co se dozvíte skript **dokončeno**.</span><span class="sxs-lookup"><span data-stu-id="212f4-251">When everything is finished, the script will say **Finished**.</span></span>

### <a name="disconnect-your-app-from-a-resource-manager-vnet"></a><span data-ttu-id="212f4-252">Odpojit vaší aplikace z virtuální sítě Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="212f4-252">Disconnect your app from a Resource Manager VNet</span></span>
<span data-ttu-id="212f4-253">Aplikace se odpojuje od vaší virtuální sítě vypnout bránu nebo zakázat připojení point-to-site.</span><span class="sxs-lookup"><span data-stu-id="212f4-253">Disconnecting your app from your virtual network does not take down the gateway or disable point-to-site connectivity.</span></span> <span data-ttu-id="212f4-254">Může po všech používáte jej na něco jiného.</span><span class="sxs-lookup"><span data-stu-id="212f4-254">You might, after all, be using it for something else.</span></span> <span data-ttu-id="212f4-255">Je také neodpojí ji z jiných aplikací než ten, který jste zadali.</span><span class="sxs-lookup"><span data-stu-id="212f4-255">It also does not disconnect it from any other apps other than the one you provided.</span></span> <span data-ttu-id="212f4-256">K provedení této akce, vyberte **3) odebrat virtuální sítě z aplikace**.</span><span class="sxs-lookup"><span data-stu-id="212f4-256">To perform this action, select **3) Remove a Virtual Network from an App**.</span></span> <span data-ttu-id="212f4-257">Pokud tak učiníte, zobrazí se přibližně toto:</span><span class="sxs-lookup"><span data-stu-id="212f4-257">When you do so, you will see something like this:</span></span>

    Currently connected to VNET v2pshell

    Confirm
    Are you sure you want to delete the following resource:
    /subscriptions/edcc99a4-b7f9-4b5e-a9a1-3034c51db496/resourceGroups/hcdemo-rg/providers/Microsoft.Web/sites/v2vnetpowers
    hell/virtualNetworkConnections/v2pshell
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):

<span data-ttu-id="212f4-258">I když skript uvádí odstranit, ale neodstraníte virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="212f4-258">Although the script says delete, it does not delete the virtual network.</span></span> <span data-ttu-id="212f4-259">Právě odebírá integrace.</span><span class="sxs-lookup"><span data-stu-id="212f4-259">It’s just removing the integration.</span></span> <span data-ttu-id="212f4-260">Jakmile potvrdíte, že toto je co chcete udělat, příkaz je zpracován poměrně rychle a zjistíte **True** když se provádí.</span><span class="sxs-lookup"><span data-stu-id="212f4-260">After you confirm that this is what you want to do, the command is processed quite quickly and tells you **True** when it is done.</span></span>

<!--Links-->
[createvpngateway]: http://azure.microsoft.com/documentation/articles/vpn-gateway-point-to-site-create/
[azureportal]: http://portal.azure.com
