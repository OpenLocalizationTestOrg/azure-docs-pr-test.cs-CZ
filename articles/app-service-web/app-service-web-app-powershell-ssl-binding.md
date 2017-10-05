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
# <a name="azure-app-service-ssl-certificate-binding-using-powershell"></a>Azure App Service vazba certifikátu protokolu SSL pomocí prostředí PowerShell
Verze služby Microsoft Azure PowerShell verze 1.1.0 byl přidán novou rutinu, která by uživateli přidělit možnost vytvořit vazbu existující nebo nové certifikáty SSL na stávající webovou aplikaci.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

Další informace o použití prostředí Azure PowerShell rutiny pro správu zkontrolujte vaší webové aplikace založené na Azure Resource Manager [Azure Resource Manager na základě příkazy prostředí PowerShell pro webové aplikace Azure](app-service-web-app-azure-resource-manager-powershell.md)

## <a name="uploading-and-binding-a-new-ssl-certificate"></a>Odesílání a vytvoření vazby nový certifikát SSL
Scénář: Uživatel se má vytvořit vazbu certifikátu protokolu SSL na jeden z jeho webové aplikace.

Znalost, název skupiny prostředků, který obsahuje webové aplikace, název webové aplikace, cesta k souboru certifikátu .pfx v počítači uživatele, heslo pro certifikát a vlastní název hostitele, jsme použijte následující příkaz prostředí PowerShell k vytvoření této vazby SSL:

    New-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -CertificateFilePath PathToPfxFile -CertificatePassword PlainTextPwd -Name www.contoso.com

Všimněte si, že před přidáním vazbu SSL pro webovou aplikaci, musí mít název hostitele (vlastní domény) už nakonfigurovaná. Pokud není nakonfigurovaný název hostitele, pak bude dojde k chybě 'název hostitele, při spuštění New-AzureRmWebAppSSLBinding neexistuje. Můžete přidat název hostitele přímo z portálu nebo pomocí prostředí Azure PowerShell. Následující fragment kódu prostředí PowerShell lze nakonfigurovat před spuštěním AzureRmWebAppSSLBinding nový název hostitele.   

    $webApp = Get-AzureRmWebApp -Name mytestapp -ResourceGroupName myresourcegroup  
    $hostNames = $webApp.HostNames  
    $HostNames.Add("www.contoso.com")  
    Set-AzureRmWebApp -Name mytestapp -ResourceGroupName myresourcegroup -HostNames $HostNames   

Je důležité si uvědomit, že rutinu Set-AzureRmWebApp přepíše názvy hostitelů pro webovou aplikaci. Proto výše uvedeném fragmentu prostředí PowerShell je připojení k existující seznam názvy hostitelů pro webové aplikace.  

## <a name="uploading-and-binding-an-existing-ssl-certificate"></a>Odesílání a vazbu existujícího certifikátu SSL
Scénář: Uživatel by chtěl dříve odeslaný certifikát SSL vazbu na jeden z jeho webové aplikace.

Můžete se nám získat seznam certifikátů, které jsou již odeslány do určité skupiny zdrojů pomocí následujícího příkazu

    Get-AzureRmWebAppCertificate -ResourceGroupName myresourcegroup

Všimněte si, že certifikáty jsou místní pro konkrétní umístění a skupině prostředků, uživatel bude muset znovu nahrát na server certifikát, pokud je nakonfigurované webové aplikace v jiném umístění a skupina prostředků jiného, než který potřebný certifikát 

Znalost, název skupiny prostředků, který obsahuje webové aplikace, název webové aplikace, kryptografický otisk certifikátu a vlastní název hostitele, jsme použijte následující příkaz prostředí PowerShell k vytvoření této vazby SSL:

    New-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Thumbprint <certificate thumbprint> -Name www.contoso.com

## <a name="deleting-an-existing-ssl-binding"></a>Odstranění stávající vazbu SSL
Scénář: Uživatel chcete odstranit stávající vazbu SSL.

Zároveň budete vědět, název skupiny prostředků, který obsahuje webové aplikace, název webové aplikace a vlastní název hostitele, jsme odebrat tuto vazbu SSL pomocí následujícího příkazu Powershellu:

    Remove-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Name www.contoso.com

Všimněte si, že pokud byla odebrána vazba SSL poslední vazbu pomocí certifikátu v tomto umístění, ve výchozím nastavení certifikátu se odstraní, pokud chcete zachovat certifikátu, že kterou může použít možnost DeleteCertificate zachovat certifikát uživatele

    Remove-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Name www.contoso.com -DeleteCertificate $false

### <a name="references"></a>Odkazy
* [Azure Resource Manager na základě příkazy prostředí PowerShell pro webové aplikace Azure](app-service-web-app-azure-resource-manager-powershell.md)
* [Úvod do prostředí App Service](app-service-app-service-environment-intro.md)
* [Použití Azure PowerShellu s Azure Resource Managerem](../powershell-azure-resource-manager.md)

