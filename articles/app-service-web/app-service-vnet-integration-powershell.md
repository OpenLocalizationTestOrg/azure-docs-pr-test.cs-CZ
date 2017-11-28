---
title: "aaaConnect aplikace tooyour virtuální sítě pomocí prostředí PowerShell"
description: "Pokyny, jak fungují tooconnect tooand s virtuálními sítěmi pomocí prostředí PowerShell"
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
ms.openlocfilehash: c9d0fa99d02cab7b2c7211a1b2f7b7d0cd27ee8e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-app-tooyour-virtual-network-by-using-powershell"></a><span data-ttu-id="6cf23-103">Připojit virtuální síť tooyour aplikace pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="6cf23-103">Connect your app tooyour virtual network by using PowerShell</span></span>
## <a name="overview"></a><span data-ttu-id="6cf23-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="6cf23-104">Overview</span></span>
<span data-ttu-id="6cf23-105">Ve službě Azure App Service se můžete připojit aplikace (webové, mobilní nebo rozhraní API) tooan virtuální sítě Azure (VNet) v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="6cf23-105">In Azure App Service, you can connect your app (web, mobile, or API) tooan Azure virtual network (VNet) in your subscription.</span></span> <span data-ttu-id="6cf23-106">Tato funkce je volána integrace virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="6cf23-106">This feature is called VNet Integration.</span></span> <span data-ttu-id="6cf23-107">funkce integrace virtuální sítě Hello Nezaměňovat s hello App Service Environment funkci, která vám umožní toorun instanci služby Azure App Service ve virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="6cf23-107">hello VNet Integration feature should not be confused with hello App Service Environment feature, which allows you toorun an instance of Azure App Service in your virtual network.</span></span>

<span data-ttu-id="6cf23-108">funkce integrace virtuální sítě Hello má uživatelské rozhraní (UI) v hello nového portálu, které můžete použít toointegrate s virtuálními sítěmi, které jsou nasazeny pomocí modelu nasazení classic hello nebo hello modelu nasazení Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="6cf23-108">hello VNet Integration feature has a user interface (UI) in hello new portal that you can use toointegrate with virtual networks that are deployed by using either hello classic deployment model or hello Azure Resource Manager deployment model.</span></span> <span data-ttu-id="6cf23-109">Pokud chcete další informace o funkci hello toolearn, najdete v části [integraci aplikace s virtuální sítě Azure](web-sites-integrate-with-vnet.md).</span><span class="sxs-lookup"><span data-stu-id="6cf23-109">If you want toolearn more about hello feature, see [Integrate your app with an Azure virtual network](web-sites-integrate-with-vnet.md).</span></span>

<span data-ttu-id="6cf23-110">Tento článek není o tom, jak toouse hello uživatelského rozhraní, ale spíš o tooenable integrace pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="6cf23-110">This article is not about how toouse hello UI but rather about how tooenable integration by using PowerShell.</span></span> <span data-ttu-id="6cf23-111">Protože hello příkazy pro každý model nasazení se liší, tento článek obsahuje oddíl pro každý model nasazení.</span><span class="sxs-lookup"><span data-stu-id="6cf23-111">Because hello commands for each deployment model are different, this article has a section for each deployment model.</span></span>  

<span data-ttu-id="6cf23-112">Než budete pokračovat v tomto článku, ujistěte se, zda máte:</span><span class="sxs-lookup"><span data-stu-id="6cf23-112">Before you continue with this article, ensure that you have:</span></span>

* <span data-ttu-id="6cf23-113">Hello nainstalované nejnovější sadu Azure PowerShell SDK.</span><span class="sxs-lookup"><span data-stu-id="6cf23-113">hello latest Azure PowerShell SDK installed.</span></span> <span data-ttu-id="6cf23-114">To můžete nainstalovat s hello instalačního programu webové platformy.</span><span class="sxs-lookup"><span data-stu-id="6cf23-114">You can install this with hello Web Platform Installer.</span></span>
* <span data-ttu-id="6cf23-115">Aplikace v Azure App Service spuštěna v Standard nebo Premium SKU.</span><span class="sxs-lookup"><span data-stu-id="6cf23-115">An app in Azure App Service running in a Standard or Premium SKU.</span></span>

## <a name="classic-virtual-networks"></a><span data-ttu-id="6cf23-116">Klasické virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="6cf23-116">Classic virtual networks</span></span>
<span data-ttu-id="6cf23-117">Tato část popisuje tři úlohy pro virtuální sítě, které používají model nasazení classic hello:</span><span class="sxs-lookup"><span data-stu-id="6cf23-117">This section explains three tasks for virtual networks that use hello classic deployment model:</span></span>

1. <span data-ttu-id="6cf23-118">Připojte vaše aplikace tooa dříve existující virtuální síť, která má bránu a je nakonfigurován pro připojení point-to-site.</span><span class="sxs-lookup"><span data-stu-id="6cf23-118">Connect your app tooa preexisting virtual network that has a gateway and is configured for point-to-site connectivity.</span></span>
2. <span data-ttu-id="6cf23-119">Aktualizujte informace integrace virtuální sítě pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6cf23-119">Update your virtual network integration information for your app.</span></span>
3. <span data-ttu-id="6cf23-120">Aplikace se odpojte od vaší virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="6cf23-120">Disconnect your app from your virtual network.</span></span>

### <a name="connect-an-app-tooa-classic-vnet"></a><span data-ttu-id="6cf23-121">Připojit aplikaci tooa klasické virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="6cf23-121">Connect an app tooa classic VNet</span></span>
<span data-ttu-id="6cf23-122">tooconnect aplikace tooa virtuální síť, proveďte tyto kroky:</span><span class="sxs-lookup"><span data-stu-id="6cf23-122">tooconnect an app tooa virtual network, follow these three steps:</span></span>

1. <span data-ttu-id="6cf23-123">Deklarujte toohello webovou aplikaci, bude připojení k konkrétní virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="6cf23-123">Declare toohello web app that it will be joining a particular virtual network.</span></span> <span data-ttu-id="6cf23-124">aplikace Hello vygeneruje certifikát, který bude mu udělená toohello virtuální sítě pro připojení point-to-site.</span><span class="sxs-lookup"><span data-stu-id="6cf23-124">hello app will generate a certificate that will be given toohello virtual network for point-to-site connectivity.</span></span>
2. <span data-ttu-id="6cf23-125">Nahrání hello webové aplikace certifikát toohello virtuální sítě a následně načíst balíček hello point-to-site VPN identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="6cf23-125">Upload hello web app certificate toohello virtual network, and then retrieve hello point-to-site VPN package URI.</span></span>
3. <span data-ttu-id="6cf23-126">Aktualizujte připojení virtuální sítě hello webové aplikace s balíčkem point-to-site hello identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="6cf23-126">Update hello web app's virtual network connection with hello point-to-site package URI.</span></span>

<span data-ttu-id="6cf23-127">Hello první a třetí kroky jsou plně skriptovatelnou, ale druhý krok hello vyžaduje jednorázové, ruční akce prostřednictvím portálu hello nebo přístup tooperform **PUT** nebo **oprava** akce ve virtuální síti hello Koncový bod Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="6cf23-127">hello first and third steps are fully scriptable, but hello second step requires a one-time, manual action through hello portal, or access tooperform **PUT** or **PATCH** actions on hello virtual network Azure Resource Manager endpoint.</span></span> <span data-ttu-id="6cf23-128">Požádejte podporu Azure toohave tuto funkci povolíte.</span><span class="sxs-lookup"><span data-stu-id="6cf23-128">Contact Azure Support toohave this enabled.</span></span> <span data-ttu-id="6cf23-129">Než začnete, ujistěte se, že máte klasickou virtuální síť, který má připojení point-to-site již povolené a nasazené brány.</span><span class="sxs-lookup"><span data-stu-id="6cf23-129">Before you start, make sure that you have a classic virtual network that has point-to-site connectivity already enabled and a deployed gateway.</span></span> <span data-ttu-id="6cf23-130">toocreate hello brány a povolit point-to-site připojení, je nutné, aby portál hello toouse jak je popsáno v [vytvoření brány VPN][createvpngateway].</span><span class="sxs-lookup"><span data-stu-id="6cf23-130">toocreate hello gateway and enable point-to-site connectivity, you need toouse hello portal as described at [Creating a VPN gateway][createvpngateway].</span></span>

<span data-ttu-id="6cf23-131">Hello klasickou virtuální síť musí toobe v hello stejnému předplatnému jako App Service plánování tuto aplikaci hello blokování, který budete nástroj integrovat.</span><span class="sxs-lookup"><span data-stu-id="6cf23-131">hello classic virtual network needs toobe in hello same subscription as your App Service plan that holds hello app that you are integrating with.</span></span>

##### <a name="set-up-azure-powershell-sdk"></a><span data-ttu-id="6cf23-132">Nastavení Azure PowerShell SDK</span><span class="sxs-lookup"><span data-stu-id="6cf23-132">Set up Azure PowerShell SDK</span></span>
<span data-ttu-id="6cf23-133">Otevřete okno prostředí PowerShell a nastavit váš účet a předplatné Azure pomocí:</span><span class="sxs-lookup"><span data-stu-id="6cf23-133">Open a PowerShell window and set up your Azure account and subscription by using:</span></span>

    Login-AzureRmAccount

<span data-ttu-id="6cf23-134">Tento příkaz otevřete příkazový řádek tooget vaše přihlašovací údaje Azure.</span><span class="sxs-lookup"><span data-stu-id="6cf23-134">That command will open a prompt tooget your Azure credentials.</span></span> <span data-ttu-id="6cf23-135">Po přihlášení, použijte jednu z následujících příkazů tooselect hello předplatné, které chcete toouse hello.</span><span class="sxs-lookup"><span data-stu-id="6cf23-135">After you sign in, use either of hello following commands tooselect hello subscription that you want toouse.</span></span> <span data-ttu-id="6cf23-136">Ujistěte se, že používáte hello předplatné, které virtuální sítě a plán služby App Service jsou v.</span><span class="sxs-lookup"><span data-stu-id="6cf23-136">Make sure that you are using hello subscription that your virtual network and App Service plan are in.</span></span>

    Select-AzureRmSubscription –SubscriptionName [WebAppSubscriptionName]

<span data-ttu-id="6cf23-137">nebo</span><span class="sxs-lookup"><span data-stu-id="6cf23-137">or</span></span>

    Select-AzureRmSubscription –SubscriptionId [WebAppSubscriptionId]

##### <a name="variables-used-in-this-article"></a><span data-ttu-id="6cf23-138">Proměnné používané v tomto článku</span><span class="sxs-lookup"><span data-stu-id="6cf23-138">Variables used in this article</span></span>
<span data-ttu-id="6cf23-139">příkazy toosimplify se nastaví **$Configuration** proměnnou prostředí PowerShell s konkrétní konfigurací hello.</span><span class="sxs-lookup"><span data-stu-id="6cf23-139">toosimplify commands, we will set a **$Configuration** PowerShell variable with hello specific configuration.</span></span>

<span data-ttu-id="6cf23-140">Nastavte proměnnou následujícím způsobem v prostředí PowerShell s hello následující parametry:</span><span class="sxs-lookup"><span data-stu-id="6cf23-140">Set a variable as follows in PowerShell with hello following parameters:</span></span>

    $Configuration = @{}
    $Configuration.WebAppResourceGroup = "[Your web app resource group]"
    $Configuration.WebAppName = "[Your web app name]"
    $Configuration.VnetSubscriptionId = "[Your vnet subscription id]"
    $Configuration.VnetResourceGroup = "[Your vnet resource group]"
    $Configuration.VnetName = "[Your vnet name]"

<span data-ttu-id="6cf23-141">umístění aplikace Hello by měl být hello umístění bez žádné mezery.</span><span class="sxs-lookup"><span data-stu-id="6cf23-141">hello app location should be hello location without any spaces.</span></span> <span data-ttu-id="6cf23-142">Westus je například západní USA.</span><span class="sxs-lookup"><span data-stu-id="6cf23-142">For example, West US is westus.</span></span>

    $Configuration.WebAppLocation = "[Your web app Location]"

<span data-ttu-id="6cf23-143">Další položky Hello je zápis certifikátu hello.</span><span class="sxs-lookup"><span data-stu-id="6cf23-143">hello next item is where hello certificate should be written.</span></span> <span data-ttu-id="6cf23-144">By mělo být zapisovatelnou cestu v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="6cf23-144">It should be a writable path on your local computer.</span></span> <span data-ttu-id="6cf23-145">Ujistěte se, že .cer tooinclude na konci hello.</span><span class="sxs-lookup"><span data-stu-id="6cf23-145">Make sure tooinclude .cer at hello end.</span></span>

    $Configuration.GeneratedCertificatePath = "[C:\Path\To\Certificate.cer]"

<span data-ttu-id="6cf23-146">toosee co nastavíte, typ **$Configuration**.</span><span class="sxs-lookup"><span data-stu-id="6cf23-146">toosee what you set, type **$Configuration**.</span></span>

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

<span data-ttu-id="6cf23-147">Hello zbývající část tohoto oddílu předpokládá, že máte proměnné vytvořené jako právě popsané.</span><span class="sxs-lookup"><span data-stu-id="6cf23-147">hello rest of this section assumes that you have a variable created as just described.</span></span>

##### <a name="declare-hello-virtual-network-toohello-app"></a><span data-ttu-id="6cf23-148">Deklarovat hello virtuální sítě toohello aplikace</span><span class="sxs-lookup"><span data-stu-id="6cf23-148">Declare hello virtual network toohello app</span></span>
<span data-ttu-id="6cf23-149">Hello použijte následující příkaz tootell hello aplikaci, bude pomocí této konkrétní virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="6cf23-149">Use hello following command tootell hello app that it will be using this particular virtual network.</span></span> <span data-ttu-id="6cf23-150">To způsobí, že toogenerate aplikace hello potřebné certifikáty:</span><span class="sxs-lookup"><span data-stu-id="6cf23-150">This will cause hello app toogenerate necessary certificates:</span></span>

    $vnet = New-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -PropertyObject @{"VnetResourceId" = "/subscriptions/$($Configuration.VnetSubscriptionId)/resourceGroups/$($Configuration.VnetResourceGroup)/providers/Microsoft.ClassicNetwork/virtualNetworks/$($Configuration.VnetName)"} -Location $Configuration.WebAppLocation -ApiVersion 2015-07-01

<span data-ttu-id="6cf23-151">Pokud tento příkaz úspěšné, **$vnet** by měl mít **vlastnosti** proměnné v ní.</span><span class="sxs-lookup"><span data-stu-id="6cf23-151">If this command succeeds, **$vnet** should have a **Properties** variable in it.</span></span> <span data-ttu-id="6cf23-152">Hello **vlastnosti** proměnná by měl obsahovat obě kryptografický otisk a hello certifikát data certifikátu.</span><span class="sxs-lookup"><span data-stu-id="6cf23-152">hello **Properties** variable should contain both a certificate thumbprint and hello certificate data.</span></span>

##### <a name="upload-hello-web-app-certificate-toohello-virtual-network"></a><span data-ttu-id="6cf23-153">Nahrát hello webové aplikace certifikát toohello virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="6cf23-153">Upload hello web app certificate toohello virtual network</span></span>
<span data-ttu-id="6cf23-154">Ruční, jednorázové krok je vyžadován pro každé předplatné a kombinace virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="6cf23-154">A manual, one-time step is required for each subscription and virtual network combination.</span></span> <span data-ttu-id="6cf23-155">To znamená pokud se připojujete aplikace v předplatném A tooVirtual sítě A, budete potřebovat toodo tento krok jenom po bez ohledu na to, kolik aplikací je nakonfigurovat.</span><span class="sxs-lookup"><span data-stu-id="6cf23-155">That is, if you are connecting apps in Subscription A tooVirtual Network A, you will need toodo this step only once regardless of how many apps you configure.</span></span> <span data-ttu-id="6cf23-156">Pokud přidáváte nové virtuální sítě tooanother aplikace, budete potřebovat toodo znovu.</span><span class="sxs-lookup"><span data-stu-id="6cf23-156">If you are adding a new app tooanother virtual network, you'll need toodo this again.</span></span> <span data-ttu-id="6cf23-157">Hello důvodem je, že sadu certifikáty se generuje na úrovni předplatného v Azure App Service a sadu hello se vygeneruje jednou pro každou virtuální síť, která hello aplikace se budou připojovat k.</span><span class="sxs-lookup"><span data-stu-id="6cf23-157">hello reason for this is that a set of certificates is generated at a subscription level in Azure App Service, and hello set is generated once for each virtual network that hello apps will connect to.</span></span>

<span data-ttu-id="6cf23-158">Hello certifikáty budou již byly nastaveny Pokud jste postupovali podle těchto kroků, nebo pokud je spojen s hello stejné virtuální sítě pomocí portálu hello.</span><span class="sxs-lookup"><span data-stu-id="6cf23-158">hello certificates will have already been set if you followed these steps or if you integrated with hello same virtual network by using hello portal.</span></span>

<span data-ttu-id="6cf23-159">prvním krokem Hello je soubor .cer toogenerate hello.</span><span class="sxs-lookup"><span data-stu-id="6cf23-159">hello first step is toogenerate hello .cer file.</span></span> <span data-ttu-id="6cf23-160">druhým krokem Hello je tooupload hello .cer souboru tooyour virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="6cf23-160">hello second step is tooupload hello .cer file tooyour virtual network.</span></span> <span data-ttu-id="6cf23-161">soubor .cer hello toogenerate z volání hello rozhraní API v hello z předchozích kroků, spusťte následující příkazy hello.</span><span class="sxs-lookup"><span data-stu-id="6cf23-161">toogenerate hello .cer file from hello API call in hello earlier step, run hello following commands.</span></span>

    $certBytes = [System.Convert]::FromBase64String($vnet.Properties.certBlob)
    [System.IO.File]::WriteAllBytes("$($Configuration.GeneratedCertificatePath)", $certBytes)

<span data-ttu-id="6cf23-162">certifikát Hello budou nacházet v umístění hello který **$Configuration.GeneratedCertificatePath** určuje.</span><span class="sxs-lookup"><span data-stu-id="6cf23-162">hello certificate will be found in hello location that **$Configuration.GeneratedCertificatePath** specifies.</span></span>

<span data-ttu-id="6cf23-163">certifikát hello tooupload ručně, použijte hello [portál Azure] [ azureportal] a **procházet virtuální sítě (klasické)** > **připojení k síti VPN**  >  **Point-to-site** > **spravovat certifikáty**.</span><span class="sxs-lookup"><span data-stu-id="6cf23-163">tooupload hello certificate manually, use hello [Azure portal][azureportal] and **Browse Virtual Network (classic)** > **VPN connections** > **Point-to-site** > **Manage certificates**.</span></span> <span data-ttu-id="6cf23-164">Tady odeslání vašeho certifikátu.</span><span class="sxs-lookup"><span data-stu-id="6cf23-164">From here, upload your certificate.</span></span>

##### <a name="get-hello-point-to-site-package"></a><span data-ttu-id="6cf23-165">Načtení balíčku pro hello point-to-site</span><span class="sxs-lookup"><span data-stu-id="6cf23-165">Get hello point-to-site package</span></span>
<span data-ttu-id="6cf23-166">dalším krokem Hello k nastavení připojení virtuální sítě ve webové aplikaci je tooget hello point-to-site balíček a poskytnout ho tooyour webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="6cf23-166">hello next step in setting up a virtual network connection on a web app is tooget hello point-to-site package and provide it tooyour web app.</span></span>

<span data-ttu-id="6cf23-167">Uložte následující soubor šablony tooa názvem GetNetworkPackageUri.json někde v počítači, například C:\Azure\Templates\GetNetworkPackageUri.json hello.</span><span class="sxs-lookup"><span data-stu-id="6cf23-167">Save hello following template tooa file called GetNetworkPackageUri.json somewhere on your computer, for example, C:\Azure\Templates\GetNetworkPackageUri.json.</span></span>

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


<span data-ttu-id="6cf23-168">Nastavte vstupní parametry:</span><span class="sxs-lookup"><span data-stu-id="6cf23-168">Set input parameters:</span></span>

    $parameters = @{"certData" = $vnet.Properties.certBlob ;
    certThumbprint = $vnet.Properties.certThumbprint ;
    "networkName" = $Configuration.VnetName }

<span data-ttu-id="6cf23-169">Volání hello skriptu:</span><span class="sxs-lookup"><span data-stu-id="6cf23-169">Call hello script:</span></span>

    $output = New-AzureRmResourceGroupDeployment -Name unused -ResourceGroupName $Configuration.VnetResourceGroup -TemplateParameterObject $parameters -TemplateFile C:\PATH\TO\GetNetworkPackageUri.json


<span data-ttu-id="6cf23-170">Proměnná Hello **$output. Outputs.packageUri** bude teď obsahovat hello balíček URI toobe zadané tooyour webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="6cf23-170">hello variable **$output.Outputs.packageUri** will now contain hello package URI toobe given tooyour web app.</span></span>

##### <a name="upload-hello-point-to-site-package-tooyour-app"></a><span data-ttu-id="6cf23-171">Nahrání aplikace tooyour hello balíček point-to-site</span><span class="sxs-lookup"><span data-stu-id="6cf23-171">Upload hello point-to-site package tooyour app</span></span>
<span data-ttu-id="6cf23-172">posledním krokem Hello je tooprovide hello aplikace s tímto balíčkem.</span><span class="sxs-lookup"><span data-stu-id="6cf23-172">hello final step is tooprovide hello app with this package.</span></span> <span data-ttu-id="6cf23-173">Jednoduše spusťte další příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="6cf23-173">Simply run hello next command:</span></span>

    $vnet = New-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)/primary" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections/gateways" -ApiVersion 2015-07-01 -PropertyObject @{"VnetName" = $Configuration.VnetName ; "VpnPackageUri" = $($output.Outputs.packageUri).Value } -Location $Configuration.WebAppLocation

<span data-ttu-id="6cf23-174">Pokud se zobrazí se dotaz tooconfirm přepisování existující prostředek, ujistěte se, že tooallow ho.</span><span class="sxs-lookup"><span data-stu-id="6cf23-174">If a message asks you tooconfirm that you are overwriting an existing resource, make sure tooallow it.</span></span>

<span data-ttu-id="6cf23-175">Po úspěšném provedení tohoto příkazu, vaše aplikace by teď měly být připojené toohello virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="6cf23-175">After this command succeeds, your app should now be connected toohello virtual network.</span></span> <span data-ttu-id="6cf23-176">tooconfirm úspěch, přejděte tooyour aplikaci konzoly, zadejte následující hello:</span><span class="sxs-lookup"><span data-stu-id="6cf23-176">tooconfirm success, go tooyour app console, and type hello following:</span></span>

    SET WEBSITE_

<span data-ttu-id="6cf23-177">Pokud je proměnná prostředí s názvem WEBSITE_VNETNAME, který má hodnotu, která odpovídá názvu hello hello cílový virtuální sítě, všechny konfigurace proběhlo úspěšně.</span><span class="sxs-lookup"><span data-stu-id="6cf23-177">If there is an environment variable called WEBSITE_VNETNAME that has a value that matches hello name of hello target virtual network, all configurations have succeeded.</span></span>

### <a name="update-classic-vnet-integration-information"></a><span data-ttu-id="6cf23-178">Aktualizovat informace integrace klasické virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="6cf23-178">Update classic VNet integration information</span></span>
<span data-ttu-id="6cf23-179">tooupdate nebo nové synchronizace vaše informace, jednoduše zopakujte hello kroky, které jste udělali, když vytvoříte hello integrace na prvním místě hello.</span><span class="sxs-lookup"><span data-stu-id="6cf23-179">tooupdate or resync your information, simply repeat hello steps that you followed when you created hello integration in hello first place.</span></span> <span data-ttu-id="6cf23-180">Tyto kroky jsou:</span><span class="sxs-lookup"><span data-stu-id="6cf23-180">Those steps are:</span></span>

1. <span data-ttu-id="6cf23-181">Zadejte informace o konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="6cf23-181">Define your configuration information.</span></span>
2. <span data-ttu-id="6cf23-182">Deklarujte aplikace toohello hello virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="6cf23-182">Declare hello virtual network toohello app.</span></span>
3. <span data-ttu-id="6cf23-183">Načtení balíčku pro hello point-to-site.</span><span class="sxs-lookup"><span data-stu-id="6cf23-183">Get hello point-to-site package.</span></span>
4. <span data-ttu-id="6cf23-184">Nahrání aplikace tooyour hello balíček point-to-site.</span><span class="sxs-lookup"><span data-stu-id="6cf23-184">Upload hello point-to-site package tooyour app.</span></span>

### <a name="disconnect-your-app-from-a-classic-vnet"></a><span data-ttu-id="6cf23-185">Odpojit aplikace od klasické virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="6cf23-185">Disconnect your app from a classic VNet</span></span>
<span data-ttu-id="6cf23-186">toodisconnect hello aplikace, musíte hello informace o konfiguraci, které bylo nastaveno během integrace virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="6cf23-186">toodisconnect hello app, you need hello configuration information that was set during virtual network integration.</span></span> <span data-ttu-id="6cf23-187">Tyto informace používat, je pak jeden příkaz toodisconnect aplikace z vaší virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="6cf23-187">Using that information, there is then one command toodisconnect your app from your virtual network.</span></span>

    $vnet = Remove-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-07-01

## <a name="resource-manager-virtual-networks"></a><span data-ttu-id="6cf23-188">Virtuální sítě Resource Manager</span><span class="sxs-lookup"><span data-stu-id="6cf23-188">Resource Manager virtual networks</span></span>
<span data-ttu-id="6cf23-189">Virtuální sítě Resource Manager mají rozhraní API Správce Azure Resource Manager, které zjednodušují některé procesy ve srovnání s klasické virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="6cf23-189">Resource Manager virtual networks have Azure Resource Manager APIs, which simplify some processes when compared with classic virtual networks.</span></span> <span data-ttu-id="6cf23-190">Máme skript, který vám pomůžou dokončit hello následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="6cf23-190">We have a script that will help you complete hello following tasks:</span></span>

* <span data-ttu-id="6cf23-191">Vytvoření virtuální sítě Resource Manager a integrovat vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6cf23-191">Create a Resource Manager virtual network and integrate your app with it.</span></span>
* <span data-ttu-id="6cf23-192">Vytvořit bránu, nakonfigurujte připojení point-to-site v dříve existující virtuální sítě Resource Manager a pak zajistit integraci aplikace.</span><span class="sxs-lookup"><span data-stu-id="6cf23-192">Create a gateway, configure point-to-site connectivity in a preexisting Resource Manager virtual network, and then integrate your app with it.</span></span>
* <span data-ttu-id="6cf23-193">Integrate dříve existující virtuální sítě Resource Manager, která má brány a point-to-site povoleno připojení k vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="6cf23-193">Integrate your app with a preexisting Resource Manager virtual network that has a gateway and point-to-site connectivity enabled.</span></span>
* <span data-ttu-id="6cf23-194">Aplikace se odpojte od vaší virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="6cf23-194">Disconnect your app from your virtual network.</span></span>

### <a name="resource-manager-vnet-app-service-integration-script"></a><span data-ttu-id="6cf23-195">Správce prostředků virtuální sítě App Service integrace skriptu</span><span class="sxs-lookup"><span data-stu-id="6cf23-195">Resource Manager VNet App Service integration script</span></span>
<span data-ttu-id="6cf23-196">Zkopírujte následující skript a uložte ho tooa soubor hello.</span><span class="sxs-lookup"><span data-stu-id="6cf23-196">Copy hello following script and save it tooa file.</span></span> <span data-ttu-id="6cf23-197">Pokud nechcete, aby toouse hello skriptu, myslíte, že volné toolearn z něj toosee jak tooset věcí až s virtuální sítí Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="6cf23-197">If you don’t want toouse hello script, feel free toolearn from it toosee how tooset things up with a Resource Manager virtual network.</span></span>

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

        Write-Host "Adding a root certificate toothis VNET"
        $root = New-AzureRmVpnClientRootCertificate -Name "AppServiceCertificate.cer" -PublicCertData $certificateData

        Write-Host "Creating Azure VNET Gateway. This may take up tooan hour."
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
            Write-Host "Currently, I will create a VNET with hello following settings:"
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
            $changeRequested = PromptYesNo "" "Do you wish toochange these settings?" 1

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

        # We create hello virtual network and add it here. hello way this works is:
        # 1) Add hello VNET association toohello App. This allows hello App toogenerate certificates, etc. for hello VNET.
        # 2) Create hello VNET and VNET gateway, add hello certificates, create hello public IP, etc., required for hello gateway
        # 3) Get hello VPN package from hello gateway and pass it back toohello App.

        $webApp = Get-AzureRmResource -ResourceName $webAppName -ResourceType "Microsoft.Web/sites" -ApiVersion 2015-08-01 -ResourceGroupName $webAppResourceGroup
        $location = $webApp.Location

        Write-Host "Creating App association tooVNET"
        $propertiesObject = @{
         "vnetResourceId" = "/subscriptions/$($subscriptionId)/resourceGroups/$($resourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($vnetName)"
        }
        $virtualNetwork = New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnetName)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $webAppResourceGroup -Force

        CreateVnet $resourceGroupName $vnetName $vnetAddressSpace $vnetGatewayAddressSpace $location

        CreateVnetGateway $resourceGroupName $vnetName $vnetIpName $location $vnetIpConfigName $vnetGatewayName $virtualNetwork.Properties.CertBlob $vnetPointToSiteAddressSpace

        Write-Host "Retrieving VPN Package and supplying tooApp"
        $packageUri = Get-AzureRmVpnClientPackage -ResourceGroupName $resourceGroupName -VirtualNetworkGatewayName $vnetGatewayName -ProcessorArchitecture Amd64
        
        # $packageUri may contain literal double-quotes at hello start and hello end of hello URL
        if($packageUri.Length -gt 0 -and $packageUri.Substring(0, 1) -eq '"' -and $packageUri.Substring($packageUri.Length - 1, 1) -eq '"')
        {
            $packageUri = $packageUri.Substring(1, $packageUri.Length - 2)
        }

        # Put hello VPN client configuration package onto hello App
        $PropertiesObject = @{
        "vnetName" = $VirtualNetworkName; "vpnPackageUri" = $packageUri
        }

        New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnetName)/primary" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections/gateways" -ApiVersion 2015-08-01 -ResourceGroupName $webAppResourceGroup -Force

        Write-Host "Finished!"
    }

    function AddExistingVnet($subscriptionId, $resourceGroupName, $webAppName)
    {
        $ErrorActionPreference = "Stop";

        # At this point, hello gateway should be able toobe joined tooan App, but may require some minor tweaking. We will declare toohello App now toouse this VNET
        Write-Host "Getting App information"
        $webApp = Get-AzureRmResource -ResourceName $webAppName -ResourceType "Microsoft.Web/sites" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        $location = $webApp.Location

        $webAppConfig = Get-AzureRmResource -ResourceName "$($webAppName)/web" -ResourceType "Microsoft.Web/sites/config" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        $currentVnet = $webAppConfig.Properties.VnetName
        if($currentVnet -ne $null -and $currentVnet -ne "")
        {
            Write-Host "Currently connected tooVNET $currentVnet"
        }

        # Display existing vnets
        $vnets = Get-AzureRmVirtualNetwork
        $vnetNames = @()
        foreach($vnet in $vnets)
        {
            $vnetNames += $vnet.Name
        }

        Write-Host
        $vnet = PromptCustom "Select a VNET toointegrate with" $vnets $vnetNames

        # We need toocheck if this VNET is able toobe joined tooa App, based on following criteria
            # If there is no gateway, we can create one.
            # If there is a gateway:
                # It must be of type Vpn
                # It must be of VpnType RouteBased
                # If it doesn't have hello right certificate, we will need tooadd it.
                # If it doesn't have a point-to-site range, we will need tooadd it.

        $gatewaySubnet = $vnet.Subnets | Where-Object { $_.Name -eq "GatewaySubnet" }

        if($gatewaySubnet -eq $null -or $gatewaySubnet.IpConfigurations -eq $null -or $gatewaySubnet.IpConfigurations.Count -eq 0)
        {
            $ErrorActionPreference = "Continue";
            # There is no gateway. We need toocreate one.
            Write-Host "This Virtual Network has no gateway. I will need toocreate one."

            $vnetName = $vnet.Name
            $vnetGatewayName="$($vnetName)-gateway"
            $vnetIpName="$($vnetName)-ip"
            $vnetIpConfigName="$($vnetName)-ipconf"

            # Virtual Network settings
            $vnetAddressSpace="10.0.0.0/8"
            $vnetGatewayAddressSpace="10.5.0.0/16"
            $vnetPointToSiteAddressSpace="172.16.0.0/16"

            $changeRequested = 0

            Write-Host "Your VNET is in hello address space $($vnet.AddressSpace.AddressPrefixes), with hello following Subnets:"
            foreach($subnet in $vnet.Subnets)
            {
                Write-Host "$($subnet.Name): $($subnet.AddressPrefix)"
            }

            $vnetGatewayAddressSpace = Read-Host "Please choose a GatewaySubnet address space"

            while($changeRequested -eq 0)
            {
                Write-Host
                Write-Host "Currently, I will create a VNET gateway with hello following settings:"
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
                $changeRequested = PromptYesNo "" "Do you wish toochange these settings?" 1

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

            Write-Host "Creating App association tooVNET"
            $propertiesObject = @{
             "vnetResourceId" = "/subscriptions/$($subscriptionId)/resourceGroups/$($vnet.ResourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($vnetName)"
            }

            $virtualNetwork = New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnet.Name)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName -Force

            # If there is no gateway subnet, we need toocreate one.
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
                Write-Error "This gateway is not of hello Vpn type. It cannot be joined tooan App."
                return
            }

            if($gateway.VpnType -ne "RouteBased")
            {
                Write-Error "This gateways Vpn type is not RouteBased. It cannot be joined tooan App."
                return
            }

            if($gateway.VpnClientConfiguration -eq $null -or $gateway.VpnClientConfiguration.VpnClientAddressPool -eq $null)
            {
                Write-Host "This gateway does not have a Point-to-site Address Range. Please specify one in CIDR notation, e.g. 10.0.0.0/8"
                $pointToSiteAddress = Read-Host "Point-To-Site Address Space"
                Set-AzureRmVirtualNetworkGatewayVpnClientConfig -VirtualNetworkGateway $gateway.Name -VpnClientAddressPool $pointToSiteAddress
            }

            Write-Host "Creating App association tooVNET"
            $propertiesObject = @{
             "vnetResourceId" = "/subscriptions/$($subscriptionId)/resourceGroups/$($vnet.ResourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($vnet.Name)"
            }

            $virtualNetwork = New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnet.Name)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName -Force

            # We need toocheck if hello certificate here exists in hello gateway.
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

        # Now finish joining by getting hello VPN package and giving it toohello App
        Write-Host "Retrieving VPN Package and supplying tooApp"
        $packageUri = Get-AzureRmVpnClientPackage -ResourceGroupName $vnet.ResourceGroupName -VirtualNetworkGatewayName $gateway.Name -ProcessorArchitecture Amd64
        
        # $packageUri may contain literal double-quotes at hello start and hello end of hello URL
        if($packageUri.Length -gt 0 -and $packageUri.Substring(0, 1) -eq '"' -and $packageUri.Substring($packageUri.Length - 1, 1) -eq '"')
        {
            $packageUri = $packageUri.Substring(1, $packageUri.Length - 2)
        }

        # Put hello VPN client configuration package onto hello App
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
            Write-Host "Currently connected tooVNET $currentVnet"

            Remove-AzureRmResource -ResourceName "$($webAppName)/$($currentVnet)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        }
            else
        {
            Write-Host "Not connected tooa VNET."
        }
    }

    Write-Host "Please Login"
    Login-AzureRmAccount

    # Choose subscription. If there's only one we will choose automatically

    $subs = Get-AzureRmSubscription
    $subscriptionId = ""

    if($subs.Length -eq 0)
    {
        Write-Error "No subscriptions bound toothis account."
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

    $resourceGroup = Read-Host "Please enter hello Resource Group of your App"

    $appName = Read-Host "Please enter hello Name of your App"

    $options = @("Add a NEW Virtual Network tooan App", "Add an EXISTING Virtual Network tooan App", "Remove a Virtual Network from an App");
    $optionValues = @(0, 1, 2)
    $option = PromptCustom "What do you want toodo?" $optionValues $options

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

<span data-ttu-id="6cf23-198">Uložení kopie hello skriptu.</span><span class="sxs-lookup"><span data-stu-id="6cf23-198">Save a copy of hello script.</span></span> <span data-ttu-id="6cf23-199">V tomto článku se označuje jako V2VnetAllinOne.ps1, ale můžete použít jiný název.</span><span class="sxs-lookup"><span data-stu-id="6cf23-199">In this article, it is called V2VnetAllinOne.ps1, but you can use another name.</span></span> <span data-ttu-id="6cf23-200">Neexistují žádné argumenty pro tento skript.</span><span class="sxs-lookup"><span data-stu-id="6cf23-200">There are no arguments for this script.</span></span> <span data-ttu-id="6cf23-201">Můžete jednoduše spusťte ji.</span><span class="sxs-lookup"><span data-stu-id="6cf23-201">You simply run it.</span></span> <span data-ttu-id="6cf23-202">Hello nejprve thing hello skript provede je výzva toosign v.</span><span class="sxs-lookup"><span data-stu-id="6cf23-202">hello first thing hello script will do is prompt you toosign in.</span></span> <span data-ttu-id="6cf23-203">Po přihlášení, skript hello získá informace o vašem účtu a vrátí seznam předplatných.</span><span class="sxs-lookup"><span data-stu-id="6cf23-203">After you sign in, hello script gets details about your account and returns a list of subscriptions.</span></span> <span data-ttu-id="6cf23-204">Není počítání hello žádost o zadání přihlašovacích údajů, provedení počáteční skriptu hello vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="6cf23-204">Not counting hello request for your credentials, hello initial script execution looks like this:</span></span>

    PS C:\Users\ccompy\Documents\VNET> .\V2VnetAllInOne.ps1
    Please Login

    Environment           : AzureCloud
    Account               : ccompy@microsoft.com
    TenantId              : 722278f-fef1-499f-91ab-2323d011db47
    SubscriptionId        : af5358e1-acac-2c90-a9eb-722190abf47a
    CurrentStorageAccount :

    Choose a subscription

    1) <span data-ttu-id="6cf23-205">Ukázkový předplatné (af5358e1-acac-2c90-a9eb-722190abf47a)</span><span class="sxs-lookup"><span data-stu-id="6cf23-205">Demo Subscription (af5358e1-acac-2c90-a9eb-722190abf47a)</span></span>
    2) <span data-ttu-id="6cf23-206">Test MS (a5350f55-dd5a-41ec-2ddb-ff7b911bb2ef)</span><span class="sxs-lookup"><span data-stu-id="6cf23-206">MS Test (a5350f55-dd5a-41ec-2ddb-ff7b911bb2ef)</span></span>
    3) <span data-ttu-id="6cf23-207">Předplatné fialové ukázku (2d4c99a4-57f9-4d5e-a0a1-0034c52db59d)</span><span class="sxs-lookup"><span data-stu-id="6cf23-207">Purple Demo Subscription (2d4c99a4-57f9-4d5e-a0a1-0034c52db59d)</span></span>

    <span data-ttu-id="6cf23-208">Vyberte možnost: 3</span><span class="sxs-lookup"><span data-stu-id="6cf23-208">Choose an option: 3</span></span>

    <span data-ttu-id="6cf23-209">Účet: ccompy@microsoft.com prostředí: AzureCloud předplatné: 2d4c99a4-57f9-4d5e-a0a1-0034c52db59d klienta: 722278f-fef1-499f-91ab-2323d011db47</span><span class="sxs-lookup"><span data-stu-id="6cf23-209">Account      : ccompy@microsoft.com Environment  : AzureCloud Subscription : 2d4c99a4-57f9-4d5e-a0a1-0034c52db59d Tenant       : 722278f-fef1-499f-91ab-2323d011db47</span></span>

    <span data-ttu-id="6cf23-210">Zadejte prosím skupinu prostředků aplikace hello: hcdemo-rg, zadejte název vaší aplikace hello: v2vnetpowershell, co můžete dělat má toodo?</span><span class="sxs-lookup"><span data-stu-id="6cf23-210">Please enter hello Resource Group of your App: hcdemo-rg Please enter hello Name of your App: v2vnetpowershell What do you want toodo?</span></span>

    1) <span data-ttu-id="6cf23-211">Přidat nové virtuální sítě tooan aplikace</span><span class="sxs-lookup"><span data-stu-id="6cf23-211">Add a NEW Virtual Network tooan App</span></span>
    2) <span data-ttu-id="6cf23-212">Přidat existující virtuální síť tooan aplikace</span><span class="sxs-lookup"><span data-stu-id="6cf23-212">Add an EXISTING Virtual Network tooan App</span></span>
    3) <span data-ttu-id="6cf23-213">Odebrat virtuální sítě z aplikace.</span><span class="sxs-lookup"><span data-stu-id="6cf23-213">Remove a Virtual Network from an App</span></span>

<span data-ttu-id="6cf23-214">Hello zbytek Tato část vysvětluje, každá z těchto tří možností.</span><span class="sxs-lookup"><span data-stu-id="6cf23-214">hello rest of this section explains each of those three options.</span></span>

### <a name="create-a-resource-manager-vnet-and-integrate-with-it"></a><span data-ttu-id="6cf23-215">Vytvoření virtuální sítě Resource Manager a integrovat s ním</span><span class="sxs-lookup"><span data-stu-id="6cf23-215">Create a Resource Manager VNet and integrate with it</span></span>
<span data-ttu-id="6cf23-216">Vyberte nové virtuální sítě, že používá hello modelu nasazení Resource Manager a integraci s vaší aplikací, toocreate **1) přidat nové virtuální sítě tooan aplikace**.</span><span class="sxs-lookup"><span data-stu-id="6cf23-216">toocreate a new virtual network that uses hello Resource Manager deployment model and integrate it with your app, select **1) Add a NEW Virtual Network tooan App**.</span></span> <span data-ttu-id="6cf23-217">Toto zobrazí výzvu k zadání názvu hello hello virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="6cf23-217">This will prompt you for hello name of hello virtual network.</span></span> <span data-ttu-id="6cf23-218">V mé případ jak můžete vidět v hello následující nastavení, byl použit název hello, v2pshell.</span><span class="sxs-lookup"><span data-stu-id="6cf23-218">In my case, as you can see in hello following settings, I used hello name, v2pshell.</span></span>

<span data-ttu-id="6cf23-219">Hello skript obsahuje hello údaje o hello virtuální síť, která je právě vytvářena.</span><span class="sxs-lookup"><span data-stu-id="6cf23-219">hello script gives hello details about hello virtual network that's being created.</span></span> <span data-ttu-id="6cf23-220">Pokud chcete, můžete změnit všechny hodnoty hello</span><span class="sxs-lookup"><span data-stu-id="6cf23-220">If I want, I can change any of hello values.</span></span> <span data-ttu-id="6cf23-221">Při provádění tohoto příkladu vytvořené virtuální síti, která má hello následující nastavení:</span><span class="sxs-lookup"><span data-stu-id="6cf23-221">In this example execution, I created a virtual network that has hello following settings:</span></span>

    Virtual Network Name:         v2pshell
    Resource Group Name:          hcdemo-rg
    Gateway Name:                 v2pshell-gateway
    Vnet IP name:                 v2pshell-ip
    Vnet IP config name:          v2pshell-ipconf
    Address Space:                10.0.0.0/8
    Gateway Address Space:        10.5.0.0/16
    Point-To-Site Address Space:  172.16.0.0/12

    Do you wish toochange these settings?
    [Y] Yes  [N] No  [?] Help (default is "N"):

<span data-ttu-id="6cf23-222">Pokud chcete, toochange hello hodnot, zadejte **Y** a proveďte změny hello.</span><span class="sxs-lookup"><span data-stu-id="6cf23-222">If you want toochange any of hello values, type **Y** and make hello changes.</span></span> <span data-ttu-id="6cf23-223">Pokud jste s nastavení virtuální sítě hello spokojeni, zadejte **N** nebo jednoduše stiskněte klávesu Enter při zobrazení výzvy týkající se změna nastavení hello.</span><span class="sxs-lookup"><span data-stu-id="6cf23-223">When you are happy with hello virtual network settings, type **N** or simply press Enter when prompted about changing hello settings.</span></span> <span data-ttu-id="6cf23-224">Z tohoto místa na až do dokončení skriptu hello zjistíte některé co IT oddělení "i's provádění, dokud není zahájena brány virtuální sítě toocreate hello.</span><span class="sxs-lookup"><span data-stu-id="6cf23-224">From there on until completion, hello script will tell you some of what it' i's doing until it starts toocreate hello virtual network gateway.</span></span> <span data-ttu-id="6cf23-225">Tento krok může trvat až hodinu tooan.</span><span class="sxs-lookup"><span data-stu-id="6cf23-225">That step can take up tooan hour.</span></span> <span data-ttu-id="6cf23-226">V průběhu této fáze neexistuje žádné indikátor průběhu, ale skript hello vám umožní vědět, kdy byla vytvořena hello brány.</span><span class="sxs-lookup"><span data-stu-id="6cf23-226">There is no progress indicator during this phase, but hello script will let you know when hello gateway has been created.</span></span>

<span data-ttu-id="6cf23-227">Po dokončení skriptu hello se dozvíte **dokončeno**.</span><span class="sxs-lookup"><span data-stu-id="6cf23-227">When hello script finishes, it will say **Finished**.</span></span> <span data-ttu-id="6cf23-228">V tomto okamžiku budete mít virtuální sítě Resource Manager, která má název hello a nastavení, které jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="6cf23-228">At this point, you will have a Resource Manager virtual network that has hello name and settings that you selected.</span></span> <span data-ttu-id="6cf23-229">Toto nové virtuální sítě se také integrovat s vaší aplikací.</span><span class="sxs-lookup"><span data-stu-id="6cf23-229">This new virtual network will also be integrated with your app.</span></span>

### <a name="integrate-your-app-with-a-preexisting-resource-manager-vnet"></a><span data-ttu-id="6cf23-230">Integrace aplikace s dříve existující virtuální sítě Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="6cf23-230">Integrate your app with a preexisting Resource Manager VNet</span></span>
<span data-ttu-id="6cf23-231">Pokud se integrace s dříve existující virtuální sítě, pokud jste zadali virtuální sítě Resource Manager, která nemá brána nebo připojení point-to-site, hello skript bude tomuto nastavení.</span><span class="sxs-lookup"><span data-stu-id="6cf23-231">When you're integrating with a preexisting virtual network, if you provide a Resource Manager virtual network that doesn’t have a gateway or point-to-site connectivity, hello script will set that up.</span></span> <span data-ttu-id="6cf23-232">Pokud hello virtuální síť už má tyto věci nastavit, přejde hello skriptu přímých toohello integrace aplikací.</span><span class="sxs-lookup"><span data-stu-id="6cf23-232">If hello VNET already has those things set up, hello script goes straight toohello app integration.</span></span> <span data-ttu-id="6cf23-233">toostart tento proces, jednoduše vyberte **2) přidejte existující virtuální síť tooan aplikace**.</span><span class="sxs-lookup"><span data-stu-id="6cf23-233">toostart this process, simply select **2) Add an EXISTING Virtual Network tooan App**.</span></span>

<span data-ttu-id="6cf23-234">Tato možnost funguje jenom v případě, že máte dříve existující virtuální sítě Resource Manager, který je v hello stejnému předplatnému jako aplikace.</span><span class="sxs-lookup"><span data-stu-id="6cf23-234">This option works only if you have a preexisting Resource Manager virtual network that is in hello same subscription as your app.</span></span> <span data-ttu-id="6cf23-235">Když vyberete možnost hello, zobrazí se seznam virtuálních sítí Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="6cf23-235">After you select hello option, you will be presented with a list of your Resource Manager virtual networks.</span></span>   

    Select a VNET toointegrate with

    1) <span data-ttu-id="6cf23-236">v2demonetwork</span><span class="sxs-lookup"><span data-stu-id="6cf23-236">v2demonetwork</span></span>
    2) <span data-ttu-id="6cf23-237">v2pshell</span><span class="sxs-lookup"><span data-stu-id="6cf23-237">v2pshell</span></span>
    3) <span data-ttu-id="6cf23-238">v2vnetintdemo</span><span class="sxs-lookup"><span data-stu-id="6cf23-238">v2vnetintdemo</span></span>
    4) <span data-ttu-id="6cf23-239">v2asenetwork</span><span class="sxs-lookup"><span data-stu-id="6cf23-239">v2asenetwork</span></span>
    5) <span data-ttu-id="6cf23-240">v2pshell2</span><span class="sxs-lookup"><span data-stu-id="6cf23-240">v2pshell2</span></span>

    <span data-ttu-id="6cf23-241">Vyberte možnost: 5</span><span class="sxs-lookup"><span data-stu-id="6cf23-241">Choose an option: 5</span></span>

<span data-ttu-id="6cf23-242">Stačí vyberte hello virtuální síť, kterou chcete toointegrate s.</span><span class="sxs-lookup"><span data-stu-id="6cf23-242">Simply select hello virtual network that you want toointegrate with.</span></span> <span data-ttu-id="6cf23-243">Pokud již máte bránu, která má povolené připojení point-to-site, skript hello jednoduše integraci aplikace s vaší virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="6cf23-243">If you already have a gateway that has point-to-site connectivity enabled, hello script will simply integrate your app with your virtual network.</span></span> <span data-ttu-id="6cf23-244">Pokud jste bránu, budete potřebovat podsíť brány toospecify hello.</span><span class="sxs-lookup"><span data-stu-id="6cf23-244">If you do not have a gateway, you will need toospecify hello gateway subnet.</span></span> <span data-ttu-id="6cf23-245">Podsíť brány musí být v adresní prostor vaší virtuální sítě a nemůže být v jiné podsíti.</span><span class="sxs-lookup"><span data-stu-id="6cf23-245">Your gateway subnet must be in your virtual network address space, and it cannot be in any other subnet.</span></span> <span data-ttu-id="6cf23-246">Pokud máte virtuální síť bez brány a spusťte tento krok, věcí vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="6cf23-246">If you have a virtual network without a gateway and run this step, things look like this:</span></span>

    This Virtual Network has no gateway. I will need toocreate one.
    Your VNET is in hello address space 172.16.0.0/16, with hello following Subnets:
    default: 172.16.0.0/24
    Please choose a GatewaySubnet address space: 172.16.1.0/26

<span data-ttu-id="6cf23-247">V tomto příkladu vytvoříme bránu virtuální sítě, který má hello následující nastavení:</span><span class="sxs-lookup"><span data-stu-id="6cf23-247">In this example, I created a virtual network gateway that has hello following settings:</span></span>

    Virtual Network Name:         v2pshell2
    Resource Group Name:          vnetdemo-rg
    Gateway Name:                 v2pshell2-gateway
    Vnet IP name:                 v2pshell2-ip
    Vnet IP config name:          v2pshell2-ipconf
    Address Space:                172.16.0.0/16
    Gateway Address Space:        172.16.1.0/26
    Point-To-Site Address Space:  172.16.0.0/12

    Do you wish toochange these settings?
    [Y] Yes  [N] No  [?] Help (default is "N"):
    Creating App association tooVNET

<span data-ttu-id="6cf23-248">Pokud chcete, toochange některé z těchto nastavení, můžete tak učinit.</span><span class="sxs-lookup"><span data-stu-id="6cf23-248">If you want toochange any of those settings, you can do so.</span></span> <span data-ttu-id="6cf23-249">Jinak, stiskněte klávesu Enter a skript hello vytvořte bránu a připojení virtuální sítě tooyour aplikace.</span><span class="sxs-lookup"><span data-stu-id="6cf23-249">Otherwise, press Enter and hello script will create your gateway and attach your app tooyour virtual network.</span></span> <span data-ttu-id="6cf23-250">čas vytvoření brány Hello je stále hodinu, přestože, proto se ujistěte, že byste, mít na paměti.</span><span class="sxs-lookup"><span data-stu-id="6cf23-250">hello gateway creation time is still an hour, though, so make sure you keep that in mind.</span></span> <span data-ttu-id="6cf23-251">Po dokončení všechno, co se dozvíte hello skriptu **dokončeno**.</span><span class="sxs-lookup"><span data-stu-id="6cf23-251">When everything is finished, hello script will say **Finished**.</span></span>

### <a name="disconnect-your-app-from-a-resource-manager-vnet"></a><span data-ttu-id="6cf23-252">Odpojit vaší aplikace z virtuální sítě Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="6cf23-252">Disconnect your app from a Resource Manager VNet</span></span>
<span data-ttu-id="6cf23-253">Aplikace se odpojuje od vaší virtuální sítě vypnout hello brány nebo zakázat připojení point-to-site.</span><span class="sxs-lookup"><span data-stu-id="6cf23-253">Disconnecting your app from your virtual network does not take down hello gateway or disable point-to-site connectivity.</span></span> <span data-ttu-id="6cf23-254">Může po všech používáte jej na něco jiného.</span><span class="sxs-lookup"><span data-stu-id="6cf23-254">You might, after all, be using it for something else.</span></span> <span data-ttu-id="6cf23-255">Je také neodpojí ji z jiných aplikací než hello jeden, které jste zadali.</span><span class="sxs-lookup"><span data-stu-id="6cf23-255">It also does not disconnect it from any other apps other than hello one you provided.</span></span> <span data-ttu-id="6cf23-256">tooperform tuto akci, vyberte **3) odebrat virtuální sítě z aplikace**.</span><span class="sxs-lookup"><span data-stu-id="6cf23-256">tooperform this action, select **3) Remove a Virtual Network from an App**.</span></span> <span data-ttu-id="6cf23-257">Pokud tak učiníte, zobrazí se přibližně toto:</span><span class="sxs-lookup"><span data-stu-id="6cf23-257">When you do so, you will see something like this:</span></span>

    Currently connected tooVNET v2pshell

    Confirm
    Are you sure you want toodelete hello following resource:
    /subscriptions/edcc99a4-b7f9-4b5e-a9a1-3034c51db496/resourceGroups/hcdemo-rg/providers/Microsoft.Web/sites/v2vnetpowers
    hell/virtualNetworkConnections/v2pshell
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):

<span data-ttu-id="6cf23-258">I když hello skriptu uvádí odstranit, ale neodstraníte hello virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="6cf23-258">Although hello script says delete, it does not delete hello virtual network.</span></span> <span data-ttu-id="6cf23-259">Právě odebírá hello integrace.</span><span class="sxs-lookup"><span data-stu-id="6cf23-259">It’s just removing hello integration.</span></span> <span data-ttu-id="6cf23-260">Jakmile potvrdíte, že toto je co chcete toodo, příkaz hello zpracování poměrně rychle a dozvíte **True** když se provádí.</span><span class="sxs-lookup"><span data-stu-id="6cf23-260">After you confirm that this is what you want toodo, hello command is processed quite quickly and tells you **True** when it is done.</span></span>

<!--Links-->
[createvpngateway]: http://azure.microsoft.com/documentation/articles/vpn-gateway-point-to-site-create/
[azureportal]: http://portal.azure.com
