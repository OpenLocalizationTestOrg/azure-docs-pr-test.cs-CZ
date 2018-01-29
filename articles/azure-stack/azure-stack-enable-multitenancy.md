---
title: "Povolit víceklientský v zásobníku Azure | Microsoft Docs"
description: "Zjistěte, jak podporovat více adresáře služby Azure Active Directory v Azure zásobníku"
services: azure-stack
documentationcenter: 
author: HeathL17
manager: byronr
editor: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: helaw
ms.openlocfilehash: a2cba85a553f20040d2fb118b35859b05870e361
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
# <a name="enable-multi-tenancy-in-azure-stack"></a>Povolit víceklientský v Azure zásobníku

*Platí pro: Azure zásobníku integrované systémy a Azure zásobníku Development Kit*

Můžete nakonfigurovat zásobník Azure pro podporu uživatelů od víc klientů služby Azure Active Directory (Azure AD) pro službu v Azure zásobníku. Jako příklad zvažte následující scénáře:

 - Jste správcem služby contoso.onmicrosoft.com, kde je nainstalován zásobník Azure.
 - Marie je správce adresáře fabrikam.onmicrosoft.com, kde se nachází uživatele typu Host. 
 - Její společnosti přijímá IaaS a PaaS služby z vaší společnosti a je potřeba povolit uživatelům z adresáře hosta (fabrikam.onmicrosoft.com) přihlásit a používat prostředky Azure zásobníku v contoso.onmicrosoft.com.

Tato příručka obsahuje kroky potřebné v kontextu tohoto scénáře konfigurace víceklientský v zásobníku Azure.  Tento scénář vám a Marie, musíte provést kroky, aby uživatelé ze společnosti Fabrikam přihlásit a využívání služeb z Azure zásobníku nasazení ve společnosti Contoso.  

## <a name="before-you-begin"></a>Než začnete
Existuje několik předpoklady, aby se zohlednily před konfigurací víceklientský v zásobníku Azure:
  
 - Jste a Marie musí koordinovat kroků pro správu ve adresáře, který je nainstalován zásobník Azure v (Contoso) a adresář hosta (Fabrikam).  
 - Ujistěte se, když jste [nainstalován](azure-stack-powershell-install.md) a [nakonfigurované](azure-stack-powershell-configure-admin.md) prostředí PowerShell pro Azure zásobníku.
 - [Stažení nástroje Azure zásobníku](azure-stack-powershell-download.md)a naimportovat moduly Connect a Identity:

    ````PowerShell
        Import-Module .\Connect\AzureStack.Connect.psm1
        Import-Module .\Identity\AzureStack.Identity.psm1
    ```` 
 - Bude vyžadovat Marie [VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn) přístup k Azure zásobníku. 

## <a name="configure-azure-stack-directory"></a>Nakonfigurujte adresář Azure zásobníku
V této části nakonfigurujete Azure zásobníku povolit přihlášení od klientů directory Fabrikam Azure AD.

### <a name="onboard-guest-directory-tenant"></a>Zařadit hosta directory klienta
Dál se zařadit klienta Directory hosta (Fabrikam) do protokolů Azure.  Tento krok nakonfiguruje Azure Resource Manager tak, aby přijímal uživatelů a objekty služby z klienta directory hosta.

````PowerShell
$adminARMEndpoint = "https://adminmanagement.local.azurestack.external"

## Replace the value below with the Azure Stack directory
$azureStackDirectoryTenant = "contoso.onmicrosoft.com"

## Replace the value below with the guest tenant directory. 
$guestDirectoryTenantToBeOnboarded = "fabrikam.onmicrosoft.com"

Register-AzSGuestDirectoryTenant -AdminResourceManagerEndpoint $adminARMEndpoint `
 -DirectoryTenantName $azureStackDirectoryTenant `
 -GuestDirectoryTenantName $guestDirectoryTenantToBeOnboarded `
 -Location "local"
````



## <a name="configure-guest-directory"></a>Nakonfigurujte adresář hosta
Po dokončení kroků v adresáři Azure zásobníku Marie musí poskytovat souhlasu zásobník Azure přístup k adresáři hosta a zaregistrujte zásobník Azure s adresářem hosta. 

### <a name="registering-azure-stack-with-the-guest-directory"></a>Registrace Azure zásobníku adresáři hosta
Jakmile správce adresáře hostovaného poskytl souhlas pro zásobník Azure pro přístup k adresáři společnosti Fabrikam, se musí zaregistrovat zásobník Azure s klientem adresář společnosti Fabrikam.

````PowerShell
$tenantARMEndpoint = "https://management.local.azurestack.external"
    
## Replace the value below with the guest tenant directory. 
$guestDirectoryTenantName = "fabrikam.onmicrosoft.com"

Register-AzSWithMyDirectoryTenant `
 -TenantResourceManagerEndpoint $tenantARMEndpoint `
 -DirectoryTenantName $guestDirectoryTenantName `
 -Verbose 
````
## <a name="direct-users-to-sign-in"></a>Přímé přihlášení uživatelů k
Teď, když jste a Marie jste dokončili postup zařadit Marie adresáře, můžete nastavit Marie Fabrikam uživatelům přihlášení.  Společnost Fabrikam uživatele (to znamená, uživatelé s příponou fabrikam.onmicrosoft.com) přihlásit návštěvou https://portal.local.azurestack.external.  

Marie budou směrovat všechny [cizí objekty zabezpečení](../active-directory/active-directory-understanding-resource-access.md) v adresáři společnosti Fabrikam (to znamená, uživatelé v adresáři společnosti Fabrikam bez přípona fabrikam.onmicrosoft.com) se přihlásit pomocí https://portal.local.azurestack.external/fabrikam.onmicrosoft.com.  Pokud se nepoužívají tuto adresu URL, posílají se na jejich výchozí adresář (Fabrikam) a zobrazí chybová zpráva s upozorněním, že jejich správce nedala souhlas.

## <a name="next-steps"></a>Další kroky

- [Správa delegovaného zprostředkovatelů](azure-stack-delegated-provider.md)
- [Klíčové koncepty Azure zásobníku](azure-stack-key-features.md)
