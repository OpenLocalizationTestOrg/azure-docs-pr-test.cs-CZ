---
title: "Certifikáty aaaSSL vazby pomocí prostředí PowerShell"
description: "Zjistěte, jak toobind SSL certifikáty tooyour webovou aplikaci pomocí prostředí PowerShell."
services: app-service\web
documentationcenter: 
author: ahmedelnably
manager: stefsch
editor: 
ms.assetid: 8ce32508-e982-48d3-b878-0e526afda537
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/13/2016
ms.author: aelnably
ms.openlocfilehash: 82f0e7c796da99ab50f69f3638ef64d55a94fc8e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-app-service-ssl-certificate-binding-using-powershell"></a><span data-ttu-id="f20b6-103">Azure App Service vazba certifikátu protokolu SSL pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="f20b6-103">Azure App Service SSL Certificate Binding using PowerShell</span></span>
<span data-ttu-id="f20b6-104">Hello verze služby Microsoft Azure PowerShell verze 1.1.0 byla přidána nová rutina, která získáte hello uživatele hello možnost toobind stávajícího nebo nového SSL certifikáty tooan stávající webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f20b6-104">With hello release of Microsoft Azure PowerShell version 1.1.0 a new cmdlet has been added that would give hello user hello ability toobind existing or new SSL certificates tooan existing Web App.</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

<span data-ttu-id="f20b6-105">toolearn o pomocí Azure Resource Manager na základě toomanage rutin prostředí Azure PowerShell vaší webové aplikace zkontrolujte [Azure Resource Manager na základě příkazy prostředí PowerShell pro webové aplikace Azure](app-service-web-app-azure-resource-manager-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="f20b6-105">toolearn about using Azure Resource Manager based Azure PowerShell cmdlets toomanage your Web Apps check [Azure Resource Manager based PowerShell commands for Azure Web App](app-service-web-app-azure-resource-manager-powershell.md)</span></span>

## <a name="uploading-and-binding-a-new-ssl-certificate"></a><span data-ttu-id="f20b6-106">Odesílání a vytvoření vazby nový certifikát SSL</span><span class="sxs-lookup"><span data-stu-id="f20b6-106">Uploading and Binding a new SSL certificate</span></span>
<span data-ttu-id="f20b6-107">Scénář: hello uživatele chcete toobind tooone certifikát SSL jeho webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="f20b6-107">Scenario: hello user would like toobind an SSL certificate tooone of his web apps.</span></span>

<span data-ttu-id="f20b6-108">Znalost hello název skupiny prostředků, který obsahuje hello webové aplikace, název webové aplikace hello, hello certifikátu .pfx cesta k souboru na počítači uživatele hello hello heslo pro certifikát hello a hello vlastní název hostitele, můžeme použít hello následující toocreate příkaz prostředí PowerShell, Vazba SSL:</span><span class="sxs-lookup"><span data-stu-id="f20b6-108">Knowing hello resource group name that contains hello web app, hello web app name, hello certificate .pfx file path on hello user machine, hello password for hello certificate, and hello custom hostname, we can use hello following PowerShell command toocreate that SSL binding:</span></span>

    New-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -CertificateFilePath PathToPfxFile -CertificatePassword PlainTextPwd -Name www.contoso.com

<span data-ttu-id="f20b6-109">Všimněte si, že před přidáním protokolu SSL vazby tooa webovou aplikaci, musí mít název hostitele (vlastní domény) už nakonfigurovaná.</span><span class="sxs-lookup"><span data-stu-id="f20b6-109">Note that before adding a SSL binding tooa web app, you must have a host name (custom domain) already configured.</span></span> <span data-ttu-id="f20b6-110">Pokud není nakonfigurovaný název hostitele hello, pak bude dojde k chybě 'název hostitele, při spuštění New-AzureRmWebAppSSLBinding neexistuje.</span><span class="sxs-lookup"><span data-stu-id="f20b6-110">If hello host name is not configured , then you will get an error 'hostname' does not exist while running  New-AzureRmWebAppSSLBinding.</span></span> <span data-ttu-id="f20b6-111">Můžete přidat název hostitele přímo z hello portálu nebo pomocí prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="f20b6-111">You can add a hostname directly from hello portal or using Azure PowerShell.</span></span> <span data-ttu-id="f20b6-112">Hello následující fragment kódu prostředí PowerShell může být název hostitele hello tooconfigure před spuštěním AzureRmWebAppSSLBinding nový.</span><span class="sxs-lookup"><span data-stu-id="f20b6-112">hello following PowerShell snippet can be tooconfigure hello hostname before running New-AzureRmWebAppSSLBinding.</span></span>   

    $webApp = Get-AzureRmWebApp -Name mytestapp -ResourceGroupName myresourcegroup  
    $hostNames = $webApp.HostNames  
    $HostNames.Add("www.contoso.com")  
    Set-AzureRmWebApp -Name mytestapp -ResourceGroupName myresourcegroup -HostNames $HostNames   

<span data-ttu-id="f20b6-113">Je důležité, že toounderstand, který hello rutiny Set-AzureRmWebApp přepíše hello názvy hostitelů pro hello webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f20b6-113">It is important toounderstand that hello Set-AzureRmWebApp cmdlet overwrites hello hostnames for hello web app.</span></span> <span data-ttu-id="f20b6-114">Proto je hello výše fragmentu kódu Powershellu připojování toohello existující seznam hello názvy hostitelů pro hello webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f20b6-114">Hence hello above PowerShell snippet is appending toohello existing list of hello host names for hello web app.</span></span>  

## <a name="uploading-and-binding-an-existing-ssl-certificate"></a><span data-ttu-id="f20b6-115">Odesílání a vazbu existujícího certifikátu SSL</span><span class="sxs-lookup"><span data-stu-id="f20b6-115">Uploading and Binding an existing SSL certificate</span></span>
<span data-ttu-id="f20b6-116">Scénář: hello uživatele chcete toobind dříve odeslaný tooone certifikát SSL jeho webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="f20b6-116">Scenario: hello user would like toobind a previously uploaded SSL certificate tooone of his web apps.</span></span>

<span data-ttu-id="f20b6-117">Nemůžeme získat hello seznam certifikátů již nahrané tooa určité skupiny zdrojů pomocí hello následující příkaz</span><span class="sxs-lookup"><span data-stu-id="f20b6-117">We can get hello list of certificates already uploaded tooa specific resource group by using hello following command</span></span>

    Get-AzureRmWebAppCertificate -ResourceGroupName myresourcegroup

<span data-ttu-id="f20b6-118">Všimněte si, že jsou certifikáty hello místní tooa konkrétní umístění nebo skupině prostředků hello stačit, když uživatel toore – nahrání certifikátu hello Pokud hello nakonfigurována webová aplikace je v jiném umístění a skupina prostředků jiného, než který hello potřeby certifikátu</span><span class="sxs-lookup"><span data-stu-id="f20b6-118">Note that hello certificates are local tooa specific location and resource group, hello user need toore-upload hello certificate if hello configured web app is in a different location and resource group other that that of hello needed certificate</span></span> 

<span data-ttu-id="f20b6-119">Znalost hello název skupiny prostředků, který obsahuje hello webové aplikace, hello název webové aplikace, hello kryptografický otisk certifikátu a hello vlastní název hostitele, můžeme hello následující toocreate příkaz prostředí PowerShell použít tuto vazbu SSL:</span><span class="sxs-lookup"><span data-stu-id="f20b6-119">Knowing hello resource group name that contains hello web app, hello web app name, hello certificate thumbprint, and hello custom hostname, we can use hello following PowerShell command toocreate that SSL binding:</span></span>

    New-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Thumbprint <certificate thumbprint> -Name www.contoso.com

## <a name="deleting-an-existing-ssl-binding"></a><span data-ttu-id="f20b6-120">Odstranění stávající vazbu SSL</span><span class="sxs-lookup"><span data-stu-id="f20b6-120">Deleting an existing SSL binding</span></span>
<span data-ttu-id="f20b6-121">Scénář: hello uživatele chcete toodelete stávající vazbu SSL.</span><span class="sxs-lookup"><span data-stu-id="f20b6-121">Scenario: hello user would like toodelete an existing SSL binding.</span></span>

<span data-ttu-id="f20b6-122">Znalost hello název skupiny prostředků, který obsahuje hello webové aplikace, hello název webové aplikace a hello vlastní název hostitele, můžeme hello následující tooremove příkaz prostředí PowerShell použít tuto vazbu SSL:</span><span class="sxs-lookup"><span data-stu-id="f20b6-122">Knowing hello resource group name that contains hello web app, hello web app name, and hello custom hostname, we can use hello following PowerShell command tooremove that SSL binding:</span></span>

    Remove-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Name www.contoso.com

<span data-ttu-id="f20b6-123">Všimněte si, že pokud hello odebrána vazbu SSL byl hello poslední vazbu pomocí certifikátu v tomto umístění výchozí hello certifikátem budou odstraněny, pokud hello uživatele, aby certifikát hello tookeep může použít hello DeleteCertificate možnost tookeep hello certifikátu</span><span class="sxs-lookup"><span data-stu-id="f20b6-123">Note that if hello removed SSL binding was hello last binding using that certificate in that location, by default hello certificate will be deleted, if hello user want tookeep hello certificate he can use hello DeleteCertificate option tookeep hello certificate</span></span>

    Remove-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Name www.contoso.com -DeleteCertificate $false

### <a name="references"></a><span data-ttu-id="f20b6-124">Odkazy</span><span class="sxs-lookup"><span data-stu-id="f20b6-124">References</span></span>
* [<span data-ttu-id="f20b6-125">Azure Resource Manager na základě příkazy prostředí PowerShell pro webové aplikace Azure</span><span class="sxs-lookup"><span data-stu-id="f20b6-125">Azure Resource Manager based PowerShell commands for Azure Web App</span></span>](app-service-web-app-azure-resource-manager-powershell.md)
* [<span data-ttu-id="f20b6-126">Úvod tooApp Service Environment</span><span class="sxs-lookup"><span data-stu-id="f20b6-126">Introduction tooApp Service Environment</span></span>](app-service-app-service-environment-intro.md)
* [<span data-ttu-id="f20b6-127">Použití Azure PowerShellu s Azure Resource Managerem</span><span class="sxs-lookup"><span data-stu-id="f20b6-127">Using Azure PowerShell with Azure Resource Manager</span></span>](../powershell-azure-resource-manager.md)

