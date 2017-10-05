---
title: "Vazby certifikátů SSL pomocí prostředí PowerShell"
description: "Postup vytvoření vazby certifikátů SSL do webové aplikace pomocí prostředí PowerShell."
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
ms.openlocfilehash: a1fcc618fb0c68778e39cc227368a60b008f9401
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-app-service-ssl-certificate-binding-using-powershell"></a><span data-ttu-id="07ccc-103">Azure App Service vazba certifikátu protokolu SSL pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="07ccc-103">Azure App Service SSL Certificate Binding using PowerShell</span></span>
<span data-ttu-id="07ccc-104">Verze služby Microsoft Azure PowerShell verze 1.1.0 byl přidán novou rutinu, která by uživateli přidělit možnost vytvořit vazbu existující nebo nové certifikáty SSL na stávající webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="07ccc-104">With the release of Microsoft Azure PowerShell version 1.1.0 a new cmdlet has been added that would give the user the ability to bind existing or new SSL certificates to an existing Web App.</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

<span data-ttu-id="07ccc-105">Další informace o použití prostředí Azure PowerShell rutiny pro správu zkontrolujte vaší webové aplikace založené na Azure Resource Manager [Azure Resource Manager na základě příkazy prostředí PowerShell pro webové aplikace Azure](app-service-web-app-azure-resource-manager-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="07ccc-105">To learn about using Azure Resource Manager based Azure PowerShell cmdlets to manage your Web Apps check [Azure Resource Manager based PowerShell commands for Azure Web App](app-service-web-app-azure-resource-manager-powershell.md)</span></span>

## <a name="uploading-and-binding-a-new-ssl-certificate"></a><span data-ttu-id="07ccc-106">Odesílání a vytvoření vazby nový certifikát SSL</span><span class="sxs-lookup"><span data-stu-id="07ccc-106">Uploading and Binding a new SSL certificate</span></span>
<span data-ttu-id="07ccc-107">Scénář: Uživatel se má vytvořit vazbu certifikátu protokolu SSL na jeden z jeho webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="07ccc-107">Scenario: The user would like to bind an SSL certificate to one of his web apps.</span></span>

<span data-ttu-id="07ccc-108">Znalost, název skupiny prostředků, který obsahuje webové aplikace, název webové aplikace, cesta k souboru certifikátu .pfx v počítači uživatele, heslo pro certifikát a vlastní název hostitele, jsme použijte následující příkaz prostředí PowerShell k vytvoření této vazby SSL:</span><span class="sxs-lookup"><span data-stu-id="07ccc-108">Knowing the resource group name that contains the web app, the web app name, the certificate .pfx file path on the user machine, the password for the certificate, and the custom hostname, we can use the following PowerShell command to create that SSL binding:</span></span>

    New-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -CertificateFilePath PathToPfxFile -CertificatePassword PlainTextPwd -Name www.contoso.com

<span data-ttu-id="07ccc-109">Všimněte si, že před přidáním vazbu SSL pro webovou aplikaci, musí mít název hostitele (vlastní domény) už nakonfigurovaná.</span><span class="sxs-lookup"><span data-stu-id="07ccc-109">Note that before adding a SSL binding to a web app, you must have a host name (custom domain) already configured.</span></span> <span data-ttu-id="07ccc-110">Pokud není nakonfigurovaný název hostitele, pak bude dojde k chybě 'název hostitele, při spuštění New-AzureRmWebAppSSLBinding neexistuje.</span><span class="sxs-lookup"><span data-stu-id="07ccc-110">If the host name is not configured , then you will get an error 'hostname' does not exist while running  New-AzureRmWebAppSSLBinding.</span></span> <span data-ttu-id="07ccc-111">Můžete přidat název hostitele přímo z portálu nebo pomocí prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="07ccc-111">You can add a hostname directly from the portal or using Azure PowerShell.</span></span> <span data-ttu-id="07ccc-112">Následující fragment kódu prostředí PowerShell lze nakonfigurovat před spuštěním AzureRmWebAppSSLBinding nový název hostitele.</span><span class="sxs-lookup"><span data-stu-id="07ccc-112">The following PowerShell snippet can be to configure the hostname before running New-AzureRmWebAppSSLBinding.</span></span>   

    $webApp = Get-AzureRmWebApp -Name mytestapp -ResourceGroupName myresourcegroup  
    $hostNames = $webApp.HostNames  
    $HostNames.Add("www.contoso.com")  
    Set-AzureRmWebApp -Name mytestapp -ResourceGroupName myresourcegroup -HostNames $HostNames   

<span data-ttu-id="07ccc-113">Je důležité si uvědomit, že rutinu Set-AzureRmWebApp přepíše názvy hostitelů pro webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="07ccc-113">It is important to understand that the Set-AzureRmWebApp cmdlet overwrites the hostnames for the web app.</span></span> <span data-ttu-id="07ccc-114">Proto výše uvedeném fragmentu prostředí PowerShell je připojení k existující seznam názvy hostitelů pro webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="07ccc-114">Hence the above PowerShell snippet is appending to the existing list of the host names for the web app.</span></span>  

## <a name="uploading-and-binding-an-existing-ssl-certificate"></a><span data-ttu-id="07ccc-115">Odesílání a vazbu existujícího certifikátu SSL</span><span class="sxs-lookup"><span data-stu-id="07ccc-115">Uploading and Binding an existing SSL certificate</span></span>
<span data-ttu-id="07ccc-116">Scénář: Uživatel by chtěl dříve odeslaný certifikát SSL vazbu na jeden z jeho webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="07ccc-116">Scenario: The user would like to bind a previously uploaded SSL certificate to one of his web apps.</span></span>

<span data-ttu-id="07ccc-117">Můžete se nám získat seznam certifikátů, které jsou již odeslány do určité skupiny zdrojů pomocí následujícího příkazu</span><span class="sxs-lookup"><span data-stu-id="07ccc-117">We can get the list of certificates already uploaded to a specific resource group by using the following command</span></span>

    Get-AzureRmWebAppCertificate -ResourceGroupName myresourcegroup

<span data-ttu-id="07ccc-118">Všimněte si, že certifikáty jsou místní pro konkrétní umístění a skupině prostředků, uživatel bude muset znovu nahrát na server certifikát, pokud je nakonfigurované webové aplikace v jiném umístění a skupina prostředků jiného, než který potřebný certifikát</span><span class="sxs-lookup"><span data-stu-id="07ccc-118">Note that the certificates are local to a specific location and resource group, the user need to re-upload the certificate if the configured web app is in a different location and resource group other that that of the needed certificate</span></span> 

<span data-ttu-id="07ccc-119">Znalost, název skupiny prostředků, který obsahuje webové aplikace, název webové aplikace, kryptografický otisk certifikátu a vlastní název hostitele, jsme použijte následující příkaz prostředí PowerShell k vytvoření této vazby SSL:</span><span class="sxs-lookup"><span data-stu-id="07ccc-119">Knowing the resource group name that contains the web app, the web app name, the certificate thumbprint, and the custom hostname, we can use the following PowerShell command to create that SSL binding:</span></span>

    New-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Thumbprint <certificate thumbprint> -Name www.contoso.com

## <a name="deleting-an-existing-ssl-binding"></a><span data-ttu-id="07ccc-120">Odstranění stávající vazbu SSL</span><span class="sxs-lookup"><span data-stu-id="07ccc-120">Deleting an existing SSL binding</span></span>
<span data-ttu-id="07ccc-121">Scénář: Uživatel chcete odstranit stávající vazbu SSL.</span><span class="sxs-lookup"><span data-stu-id="07ccc-121">Scenario: The user would like to delete an existing SSL binding.</span></span>

<span data-ttu-id="07ccc-122">Zároveň budete vědět, název skupiny prostředků, který obsahuje webové aplikace, název webové aplikace a vlastní název hostitele, jsme odebrat tuto vazbu SSL pomocí následujícího příkazu Powershellu:</span><span class="sxs-lookup"><span data-stu-id="07ccc-122">Knowing the resource group name that contains the web app, the web app name, and the custom hostname, we can use the following PowerShell command to remove that SSL binding:</span></span>

    Remove-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Name www.contoso.com

<span data-ttu-id="07ccc-123">Všimněte si, že pokud byla odebrána vazba SSL poslední vazbu pomocí certifikátu v tomto umístění, ve výchozím nastavení certifikátu se odstraní, pokud chcete zachovat certifikátu, že kterou může použít možnost DeleteCertificate zachovat certifikát uživatele</span><span class="sxs-lookup"><span data-stu-id="07ccc-123">Note that if the removed SSL binding was the last binding using that certificate in that location, by default the certificate will be deleted, if the user want to keep the certificate he can use the DeleteCertificate option to keep the certificate</span></span>

    Remove-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Name www.contoso.com -DeleteCertificate $false

### <a name="references"></a><span data-ttu-id="07ccc-124">Odkazy</span><span class="sxs-lookup"><span data-stu-id="07ccc-124">References</span></span>
* [<span data-ttu-id="07ccc-125">Azure Resource Manager na základě příkazy prostředí PowerShell pro webové aplikace Azure</span><span class="sxs-lookup"><span data-stu-id="07ccc-125">Azure Resource Manager based PowerShell commands for Azure Web App</span></span>](app-service-web-app-azure-resource-manager-powershell.md)
* [<span data-ttu-id="07ccc-126">Úvod do prostředí App Service</span><span class="sxs-lookup"><span data-stu-id="07ccc-126">Introduction to App Service Environment</span></span>](app-service-app-service-environment-intro.md)
* [<span data-ttu-id="07ccc-127">Použití Azure PowerShellu s Azure Resource Managerem</span><span class="sxs-lookup"><span data-stu-id="07ccc-127">Using Azure PowerShell with Azure Resource Manager</span></span>](../powershell-azure-resource-manager.md)

