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
# <a name="azure-app-service-ssl-certificate-binding-using-powershell"></a>Azure App Service vazba certifikátu protokolu SSL pomocí prostředí PowerShell
Hello verze služby Microsoft Azure PowerShell verze 1.1.0 byla přidána nová rutina, která získáte hello uživatele hello možnost toobind stávajícího nebo nového SSL certifikáty tooan stávající webovou aplikaci.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

toolearn o pomocí Azure Resource Manager na základě toomanage rutin prostředí Azure PowerShell vaší webové aplikace zkontrolujte [Azure Resource Manager na základě příkazy prostředí PowerShell pro webové aplikace Azure](app-service-web-app-azure-resource-manager-powershell.md)

## <a name="uploading-and-binding-a-new-ssl-certificate"></a>Odesílání a vytvoření vazby nový certifikát SSL
Scénář: hello uživatele chcete toobind tooone certifikát SSL jeho webových aplikací.

Znalost hello název skupiny prostředků, který obsahuje hello webové aplikace, název webové aplikace hello, hello certifikátu .pfx cesta k souboru na počítači uživatele hello hello heslo pro certifikát hello a hello vlastní název hostitele, můžeme použít hello následující toocreate příkaz prostředí PowerShell, Vazba SSL:

    New-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -CertificateFilePath PathToPfxFile -CertificatePassword PlainTextPwd -Name www.contoso.com

Všimněte si, že před přidáním protokolu SSL vazby tooa webovou aplikaci, musí mít název hostitele (vlastní domény) už nakonfigurovaná. Pokud není nakonfigurovaný název hostitele hello, pak bude dojde k chybě 'název hostitele, při spuštění New-AzureRmWebAppSSLBinding neexistuje. Můžete přidat název hostitele přímo z hello portálu nebo pomocí prostředí Azure PowerShell. Hello následující fragment kódu prostředí PowerShell může být název hostitele hello tooconfigure před spuštěním AzureRmWebAppSSLBinding nový.   

    $webApp = Get-AzureRmWebApp -Name mytestapp -ResourceGroupName myresourcegroup  
    $hostNames = $webApp.HostNames  
    $HostNames.Add("www.contoso.com")  
    Set-AzureRmWebApp -Name mytestapp -ResourceGroupName myresourcegroup -HostNames $HostNames   

Je důležité, že toounderstand, který hello rutiny Set-AzureRmWebApp přepíše hello názvy hostitelů pro hello webovou aplikaci. Proto je hello výše fragmentu kódu Powershellu připojování toohello existující seznam hello názvy hostitelů pro hello webovou aplikaci.  

## <a name="uploading-and-binding-an-existing-ssl-certificate"></a>Odesílání a vazbu existujícího certifikátu SSL
Scénář: hello uživatele chcete toobind dříve odeslaný tooone certifikát SSL jeho webových aplikací.

Nemůžeme získat hello seznam certifikátů již nahrané tooa určité skupiny zdrojů pomocí hello následující příkaz

    Get-AzureRmWebAppCertificate -ResourceGroupName myresourcegroup

Všimněte si, že jsou certifikáty hello místní tooa konkrétní umístění nebo skupině prostředků hello stačit, když uživatel toore – nahrání certifikátu hello Pokud hello nakonfigurována webová aplikace je v jiném umístění a skupina prostředků jiného, než který hello potřeby certifikátu 

Znalost hello název skupiny prostředků, který obsahuje hello webové aplikace, hello název webové aplikace, hello kryptografický otisk certifikátu a hello vlastní název hostitele, můžeme hello následující toocreate příkaz prostředí PowerShell použít tuto vazbu SSL:

    New-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Thumbprint <certificate thumbprint> -Name www.contoso.com

## <a name="deleting-an-existing-ssl-binding"></a>Odstranění stávající vazbu SSL
Scénář: hello uživatele chcete toodelete stávající vazbu SSL.

Znalost hello název skupiny prostředků, který obsahuje hello webové aplikace, hello název webové aplikace a hello vlastní název hostitele, můžeme hello následující tooremove příkaz prostředí PowerShell použít tuto vazbu SSL:

    Remove-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Name www.contoso.com

Všimněte si, že pokud hello odebrána vazbu SSL byl hello poslední vazbu pomocí certifikátu v tomto umístění výchozí hello certifikátem budou odstraněny, pokud hello uživatele, aby certifikát hello tookeep může použít hello DeleteCertificate možnost tookeep hello certifikátu

    Remove-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Name www.contoso.com -DeleteCertificate $false

### <a name="references"></a>Odkazy
* [Azure Resource Manager na základě příkazy prostředí PowerShell pro webové aplikace Azure](app-service-web-app-azure-resource-manager-powershell.md)
* [Úvod tooApp Service Environment](app-service-app-service-environment-intro.md)
* [Použití Azure PowerShellu s Azure Resource Managerem](../powershell-azure-resource-manager.md)

