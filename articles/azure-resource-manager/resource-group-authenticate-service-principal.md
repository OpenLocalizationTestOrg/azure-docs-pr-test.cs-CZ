---
title: "aaaCreate identitu pro aplikace Azure pomocí prostředí PowerShell | Microsoft Docs"
description: "Popisuje, jak řídit toouse prostředí Azure PowerShell toocreate aplikaci Azure Active Directory a objektu služby a udělte ho tooresources přístup prostřednictvím přístupu na základě rolí. Ukazuje, jak tooauthenticate aplikace pomocí hesla nebo certifikátu."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: d2caf121-9fbe-4f00-bf9d-8f3d1f00a6ff
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/15/2017
ms.author: tomfitz
ms.openlocfilehash: c534360799b590054a051e4426e5e27dccb559b7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-powershell-toocreate-a-service-principal-tooaccess-resources"></a>Použití Azure PowerShell toocreate hlavní tooaccess prostředky služby

Pokud máte aplikace nebo skript, který potřebuje tooaccess prostředků, můžete nastavit identity aplikace hello a ověřit hello aplikaci s svoje vlastní přihlašovací údaje. Tato identita se označuje jako hlavní název služby. Tento přístup umožňuje:

* Přiřazení oprávnění toohello identita aplikace, která se liší od vlastní oprávnění. Obvykle se tato oprávnění se s omezeným přístupem tooexactly jaké aplikace hello musí toodo.
* Při provádění bezobslužného skriptu, použijte certifikát pro ověřování.

Toto téma ukazuje, jak toouse [prostředí Azure PowerShell](/powershell/azure/overview) tooset až všechno, co potřebujete pro toorun aplikaci v části vlastní pověření a identity.

## <a name="required-permissions"></a>Požadovaná oprávnění
toocomplete tohoto tématu, musíte mít dostatečná oprávnění v Azure Active Directory a vašeho předplatného Azure. Konkrétně musí být schopný toocreate aplikace v hello Azure Active Directory a přiřadit role hlavní tooa služby hello. 

Hello bylo jestli váš účet má odpovídající oprávnění je prostřednictvím portálu hello nejjednodušší toocheck způsobem. V tématu [zkontrolujte požadované oprávnění](resource-group-create-service-principal-portal.md#required-permissions).

Nyní pokračujte tooa části k ověřování:

* [heslo](#create-service-principal-with-password)
* [certifikát podepsaný svým držitelem](#create-service-principal-with-self-signed-certificate)
* [certifikát od certifikační autority](#create-service-principal-with-certificate-from-certificate-authority)

## <a name="powershell-commands"></a>Příkazy prostředí PowerShell

tooset až objekt služby, použijte:

| Příkaz | Popis |
| ------- | ----------- | 
| [Nové AzureRmADServicePrincipal](/powershell/module/azurerm.resources/new-azurermadserviceprincipal) | Vytvoří objekt služby Azure Active Directory |
| [New-AzureRmRoleAssignment](/powershell/module/azurerm.resources/new-azurermroleassignment) | Přiřadí hello zadaný RBAC role toohello zadaný objekt, na hello zadaný obor. |


## <a name="create-service-principal-with-password"></a>Vytvoření instančního objektu s heslem

toocreate objekt služby s hello role Přispěvatel pro vaše předplatné, použijte: 

```powershell
Login-AzureRmAccount
$sp = New-AzureRmADServicePrincipal -DisplayName exampleapp -Password "{provide-password}"
Sleep 20
New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $sp.ApplicationId
```

Příklad Hello v režimu spánku 20 sekund tooallow nějakou dobu hello nové služby hlavní toopropagate v rámci Azure Active Directory. Pokud váš skript nečeká dost dlouho, zobrazí chybová zpráva: "PrincipalNotFound: hlavní {id} neexistuje v adresáři hello."

Hello následující skript vám umožní toospecify obor než hello výchozí předplatné a opakování hello přiřazení role, pokud dojde k chybě:

```powershell
Param (

 # Use tooset scope tooresource group. If no value is provided, scope is set toosubscription.
 [Parameter(Mandatory=$false)]
 [String] $ResourceGroup,

 # Use tooset subscription. If no value is provided, default subscription is used. 
 [Parameter(Mandatory=$false)]
 [String] $SubscriptionId,

 [Parameter(Mandatory=$true)]
 [String] $ApplicationDisplayName,

 [Parameter(Mandatory=$true)]
 [String] $Password
)

 Login-AzureRmAccount
 Import-Module AzureRM.Resources

 if ($SubscriptionId -eq "") 
 {
    $SubscriptionId = (Get-AzureRmContext).Subscription.Id
 }
 else
 {
    Set-AzureRmContext -SubscriptionId $SubscriptionId
 }

 if ($ResourceGroup -eq "")
 {
    $Scope = "/subscriptions/" + $SubscriptionId
 }
 else
 {
    $Scope = (Get-AzureRmResourceGroup -Name $ResourceGroup -ErrorAction Stop).ResourceId
 }

 
 # Create Service Principal for hello AD app
 $ServicePrincipal = New-AzureRMADServicePrincipal -DisplayName $ApplicationDisplayName -Password $Password
 Get-AzureRmADServicePrincipal -ObjectId $ServicePrincipal.Id 

 $NewRole = $null
 $Retries = 0;
 While ($NewRole -eq $null -and $Retries -le 6)
 {
    # Sleep here for a few seconds tooallow hello service principal application toobecome active (should only take a couple of seconds normally)
    Sleep 15
    New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $ServicePrincipal.ApplicationId -Scope $Scope | Write-Verbose -ErrorAction SilentlyContinue
    $NewRole = Get-AzureRMRoleAssignment -ServicePrincipalName $ServicePrincipal.ApplicationId -ErrorAction SilentlyContinue
    $Retries++;
 }
```

Několik položek toonote o hello skriptu:

* toogrant hello identity přístup toohello výchozí předplatné, není nutné tooprovide parametry ResourceGroup nebo ID předplatného.
* Zadejte parametr ResourceGroup hello pouze v případě, že chcete toolimit hello obor skupiny prostředků tooa přiřazení role hello.
*  V tomto příkladu přidáte roli Přispěvatel hlavní toohello služby hello. Jiné role, naleznete v části [RBAC: předdefinované role](../active-directory/role-based-access-built-in-roles.md).
* Hello skriptu v režimu spánku 15 sekund tooallow nějakou dobu hello nové služby hlavní toopropagate v rámci Azure Active Directory. Pokud váš skript nečeká dost dlouho, zobrazí chybová zpráva: "PrincipalNotFound: hlavní {id} neexistuje v adresáři hello."
* Pokud potřebujete toogrant hello služby hlavní přístup toomore odběry nebo skupiny prostředků, spusťte hello `New-AzureRMRoleAssignment` rutinu znovu s různými obory.


### <a name="provide-credentials-through-powershell"></a>Zadejte přihlašovací údaje pomocí prostředí PowerShell
Nyní musíte toolog v jako operace tooperform aplikace hello. Pro hello uživatelské jméno, použijte hello `ApplicationId` kterou jste vytvořili pro aplikace hello. Hello heslo použijte hello jeden, který jste zadali při vytváření účtu hello. 

```powershell   
$creds = Get-Credential
Login-AzureRmAccount -Credential $creds -ServicePrincipal -TenantId {tenant-id}
```

ID klienta Hello nerozlišuje malá písmena, takže jej můžete vložit přímo ve vašem skriptu. Pokud potřebujete ID klienta hello tooretrieve, použijte:

```powershell
(Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId
```

## <a name="create-service-principal-with-self-signed-certificate"></a>Vytvoření instančního objektu se certifikát podepsaný svým držitelem

toocreate hlavní název služby pomocí certifikátu podepsaného svým držitelem a hello role Přispěvatel pro vaše předplatné, použijte: 

```powershell
Login-AzureRmAccount
$cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=exampleappScriptCert" -KeySpec KeyExchange
$keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())

$sp = New-AzureRMADServicePrincipal -DisplayName exampleapp -CertValue $keyValue -EndDate $cert.NotAfter -StartDate $cert.NotBefore
Sleep 20
New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $sp.ApplicationId
```

Příklad Hello v režimu spánku 20 sekund tooallow nějakou dobu hello nové služby hlavní toopropagate v rámci Azure Active Directory. Pokud váš skript nečeká dost dlouho, zobrazí chybová zpráva: "PrincipalNotFound: hlavní {id} neexistuje v adresáři hello."

Hello následující skript vám umožní toospecify obor než hello výchozí předplatné a opakování hello přiřazení role, pokud dojde k chybě. Pokud nemáte Azure PowerShell 2.0 na Windows 10 nebo Windows Server 2016.

```powershell
Param (

 # Use tooset scope tooresource group. If no value is provided, scope is set toosubscription.
 [Parameter(Mandatory=$false)]
 [String] $ResourceGroup,

 # Use tooset subscription. If no value is provided, default subscription is used. 
 [Parameter(Mandatory=$false)]
 [String] $SubscriptionId,

 [Parameter(Mandatory=$true)]
 [String] $ApplicationDisplayName
 )

 Login-AzureRmAccount
 Import-Module AzureRM.Resources

 if ($SubscriptionId -eq "") 
 {
    $SubscriptionId = (Get-AzureRmContext).Subscription.Id
 }
 else
 {
    Set-AzureRmContext -SubscriptionId $SubscriptionId
 }

 if ($ResourceGroup -eq "")
 {
    $Scope = "/subscriptions/" + $SubscriptionId
 }
 else
 {
    $Scope = (Get-AzureRmResourceGroup -Name $ResourceGroup -ErrorAction Stop).ResourceId
 }

 $cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=exampleappScriptCert" -KeySpec KeyExchange
 $keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())

 $ServicePrincipal = New-AzureRMADServicePrincipal -DisplayName $ApplicationDisplayName -CertValue $keyValue -EndDate $cert.NotAfter -StartDate $cert.NotBefore
 Get-AzureRmADServicePrincipal -ObjectId $ServicePrincipal.Id 

 $NewRole = $null
 $Retries = 0;
 While ($NewRole -eq $null -and $Retries -le 6)
 {
    # Sleep here for a few seconds tooallow hello service principal application toobecome active (should only take a couple of seconds normally)
    Sleep 15
    New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $ServicePrincipal.ApplicationId -Scope $Scope | Write-Verbose -ErrorAction SilentlyContinue
    $NewRole = Get-AzureRMRoleAssignment -ServicePrincipalName $ServicePrincipal.ApplicationId -ErrorAction SilentlyContinue
    $Retries++;
 }
```

Několik položek toonote o hello skriptu:

* toogrant hello identity přístup toohello výchozí předplatné, není nutné tooprovide parametry ResourceGroup nebo ID předplatného.
* Zadejte parametr ResourceGroup hello pouze v případě, že chcete toolimit hello obor skupiny prostředků tooa přiřazení role hello.
* V tomto příkladu přidáte roli Přispěvatel hlavní toohello služby hello. Jiné role, naleznete v části [RBAC: předdefinované role](../active-directory/role-based-access-built-in-roles.md).
* Hello skriptu v režimu spánku 15 sekund tooallow nějakou dobu hello nové služby hlavní toopropagate v rámci Azure Active Directory. Pokud váš skript nečeká dost dlouho, zobrazí chybová zpráva: "PrincipalNotFound: hlavní {id} neexistuje v adresáři hello."
* Pokud potřebujete toogrant hello služby hlavní přístup toomore odběry nebo skupiny prostředků, spusťte hello `New-AzureRMRoleAssignment` rutinu znovu s různými obory.

Pokud jste **nemají Windows 10 nebo Windows Server 2016 Technical Preview**, budete potřebovat toodownload hello [certifikát podepsaný svým držitelem generátor](https://gallery.technet.microsoft.com/scriptcenter/Self-signed-certificate-5920a7c6/) z webu Microsoft Script Center. Rozbalte obsah a importovat hello rutiny, které potřebujete.

```powershell  
# Only run if you could not use New-SelfSignedCertificate
Import-Module -Name c:\ExtractedModule\New-SelfSignedCertificateEx.ps1
```
  
Ve skriptu hello nahraďte hello následující dva řádky toogenerate hello certifikátu.
  
```powershell
New-SelfSignedCertificateEx  -StoreLocation CurrentUser -StoreName My -Subject "CN=exampleapp" -KeySpec "Exchange" -FriendlyName "exampleapp"
$cert = Get-ChildItem -path Cert:\CurrentUser\my | where {$PSitem.Subject -eq 'CN=exampleapp' }
```

### <a name="provide-certificate-through-automated-powershell-script"></a>Zadejte certifikát pomocí automatizované skript prostředí PowerShell
Vždy, když se přihlásíte jako hlavní název služby, musíte pro vaši aplikaci AD id klienta hello tooprovide hello adresáře. Klient je instance služby Azure Active Directory. Pokud máte pouze jedno předplatné, můžete použít:

```powershell
Param (
 
 [Parameter(Mandatory=$true)]
 [String] $CertSubject,
 
 [Parameter(Mandatory=$true)]
 [String] $ApplicationId,

 [Parameter(Mandatory=$true)]
 [String] $TenantId
 )

 $Thumbprint = (Get-ChildItem cert:\CurrentUser\My\ | Where-Object {$_.Subject -match $CertSubject }).Thumbprint
 Login-AzureRmAccount -ServicePrincipal -CertificateThumbprint $Thumbprint -ApplicationId $ApplicationId -TenantId $TenantId
```

aplikace Hello ID a ID klienta nejsou písmena, takže je můžete vložit přímo ve vašem skriptu. Pokud potřebujete ID klienta hello tooretrieve, použijte:

```powershell
(Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId
```

Pokud potřebujete ID aplikace hello tooretrieve, použijte:

```powershell
(Get-AzureRmADApplication -DisplayNameStartWith {display-name}).ApplicationId
```

## <a name="create-service-principal-with-certificate-from-certificate-authority"></a>Vytvoření instančního objektu pomocí certifikátu od certifikační autority
toouse certifikátu vydaného certifikační autority toocreate hlavní název služby, použijte hello následující skript:

```powershell
Param (
 [Parameter(Mandatory=$true)]
 [String] $ApplicationDisplayName,

 [Parameter(Mandatory=$true)]
 [String] $SubscriptionId,

 [Parameter(Mandatory=$true)]
 [String] $CertPath,

 [Parameter(Mandatory=$true)]
 [String] $CertPlainPassword
 )

 Login-AzureRmAccount
 Import-Module AzureRM.Resources
 Set-AzureRmContext -SubscriptionId $SubscriptionId

 $KeyId = (New-Guid).Guid
 $CertPassword = ConvertTo-SecureString $CertPlainPassword -AsPlainText -Force

 $PFXCert = New-Object -TypeName System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList @($CertPath, $CertPassword)
 $KeyValue = [System.Convert]::ToBase64String($PFXCert.GetRawCertData())

 $KeyCredential = New-Object  Microsoft.Azure.Commands.Resources.Models.ActiveDirectory.PSADKeyCredential
 $KeyCredential.StartDate = $PFXCert.NotBefore
 $KeyCredential.EndDate= $PFXCert.NotAfter
 $KeyCredential.KeyId = $KeyId
 $KeyCredential.CertValue = $KeyValue

 $ServicePrincipal = New-AzureRMADServicePrincipal -DisplayName $ApplicationDisplayName -KeyCredentials $keyCredential
 Get-AzureRmADServicePrincipal -ObjectId $ServicePrincipal.Id 

 $NewRole = $null
 $Retries = 0;
 While ($NewRole -eq $null -and $Retries -le 6)
 {
    # Sleep here for a few seconds tooallow hello service principal application toobecome active (should only take a couple of seconds normally)
    Sleep 15
    New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $ServicePrincipal.ApplicationId | Write-Verbose -ErrorAction SilentlyContinue
    $NewRole = Get-AzureRMRoleAssignment -ServicePrincipalName $ServicePrincipal.ApplicationId -ErrorAction SilentlyContinue
    $Retries++;
 }
 
 $NewRole
```

Několik položek toonote o hello skriptu:

* Přístup je vymezená toohello předplatné.
* V tomto příkladu přidáte roli Přispěvatel hlavní toohello služby hello. Jiné role, naleznete v části [RBAC: předdefinované role](../active-directory/role-based-access-built-in-roles.md).
* Hello skriptu v režimu spánku 15 sekund tooallow nějakou dobu hello nové služby hlavní toopropagate v rámci Azure Active Directory. Pokud váš skript nečeká dost dlouho, zobrazí chybová zpráva: "PrincipalNotFound: hlavní {id} neexistuje v adresáři hello."
* Pokud potřebujete toogrant hello služby hlavní přístup toomore odběry nebo skupiny prostředků, spusťte hello `New-AzureRMRoleAssignment` rutinu znovu s různými obory.

### <a name="provide-certificate-through-automated-powershell-script"></a>Zadejte certifikát pomocí automatizované skript prostředí PowerShell
Vždy, když se přihlásíte jako hlavní název služby, musíte pro vaši aplikaci AD id klienta hello tooprovide hello adresáře. Klient je instance služby Azure Active Directory.

```powershell
Param (
 
 [Parameter(Mandatory=$true)]
 [String] $CertPath,

 [Parameter(Mandatory=$true)]
 [String] $CertPlainPassword,
 
 [Parameter(Mandatory=$true)]
 [String] $ApplicationId,

 [Parameter(Mandatory=$true)]
 [String] $TenantId
 )

 $CertPassword = ConvertTo-SecureString $CertPlainPassword -AsPlainText -Force
 $PFXCert = New-Object -TypeName System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList @($CertPath, $CertPassword)
 $Thumbprint = $PFXCert.Thumbprint

 Login-AzureRmAccount -ServicePrincipal -CertificateThumbprint $Thumbprint -ApplicationId $ApplicationId -TenantId $TenantId
```

aplikace Hello ID a ID klienta nejsou písmena, takže je můžete vložit přímo ve vašem skriptu. Pokud potřebujete ID klienta hello tooretrieve, použijte:

```powershell
(Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId
```

Pokud potřebujete ID aplikace hello tooretrieve, použijte:

```powershell
(Get-AzureRmADApplication -DisplayNameStartWith {display-name}).ApplicationId
```

## <a name="change-credentials"></a>Změnit pověření

přihlašovací údaje hello toochange pro aplikaci AD, buď kvůli ohrožení zabezpečení nebo vypršení platnosti pověření použít hello [odebrat AzureRmADAppCredential](/powershell/resourcemanager/azurerm.resources/v3.3.0/remove-azurermadappcredential) a [New-AzureRmADAppCredential](/powershell/module/azurerm.resources/new-azurermadappcredential) rutiny.

tooremove všechny hello přihlašovací údaje pro aplikaci, použijte:

```powershell
Remove-AzureRmADAppCredential -ApplicationId 8bc80782-a916-47c8-a47e-4d76ed755275 -All
```

tooadd heslo, použijte:

```powershell
New-AzureRmADAppCredential -ApplicationId 8bc80782-a916-47c8-a47e-4d76ed755275 -Password p@ssword!
```

tooadd hodnotu certifikát vytvořit certifikát podepsaný svým držitelem, jak je znázorněno v tomto tématu. Pak použijte:

```powershell
New-AzureRmADAppCredential -ApplicationId 8bc80782-a916-47c8-a47e-4d76ed755275 -CertValue $keyValue -EndDate $cert.NotAfter -StartDate $cert.NotBefore
```

## <a name="save-access-token-toosimplify-log-in"></a>Uložit přístup tokenu toosimplify přihlášení
tooavoid poskytnete hello služby hlavní přihlašovací údaje pokaždé, když je nutné toolog v, můžete uložit hello přístupový token.

toouse hello aktuální přístupový token v jiné relaci, uložte profil hello.
   
```powershell
Save-AzureRmProfile -Path c:\Users\exampleuser\profile\exampleSP.json
```
   
Otevřete profil hello a zkontrolujte jeho obsah. Všimněte si, že obsahuje přístupový token. Místo ručně přihlášení znovu načíst jednoduše hello profilu.
   
```powershell
Select-AzureRmProfile -Path c:\Users\exampleuser\profile\exampleSP.json
```

> [!NOTE]
> Hello přístupovému tokenu vyprší platnost, takže pomocí uloženého profilu funguje výhradně u tak dlouho, dokud hello token je platný.
>  

Alternativně můžete vyvolat operace REST z prostředí PowerShell toolog v. Z hello odpověď ověřování můžete načíst hello přístupový token pro použití s dalšími operacemi. Příklad získání tokenu přístupu hello vyvoláním operace REST, naleznete v části [generování tokenu přístupu](resource-manager-rest-api.md#generating-an-access-token).

## <a name="debug"></a>Ladění

Můžete setkat s následujícím chybám při vytváření objektu služby hello:

* **"Authentication_Unauthorized"** nebo **"žádné předplatné nalezena v kontextu hello".** -Se zobrazí tato chyba, pokud váš účet nemá hello [požadovaná oprávnění](#required-permissions) na Azure Active Directory tooregister hello aplikace. Obvykle se zobrazí tato chyba při jenom správci ve vašem Azure Active Directory můžete zaregistrovat aplikace a váš účet není správce. Požádejte vašeho správce tooeither přiřadit můžete tooan roli správce, nebo tooenable uživatelé tooregister aplikace.

* Váš účet **"nemá autorizace tooperform akce 'Microsoft.Authorization/roleAssignments/write' u rozsahu: /subscriptions/ {guid}'."**  -Zobrazí tato chyba, pokud se váš účet nemá dostatečná oprávnění tooassign identitu tooan role. Požádejte správce tooadd vaše předplatné můžete tooUser přístupu k roli správce.

## <a name="sample-applications"></a>Ukázkové aplikace
Informace o protokolování jako aplikace hello prostřednictvím různých platforem najdete v tématu:

* [.NET](/dotnet/azure/dotnet-sdk-azure-authenticate?view=azure-dotnet)
* [Java](/java/azure/java-sdk-azure-authenticate)
* [Node.js](/nodejs/azure/node-sdk-azure-get-started?view=azure-node-2.0.0)
* [Python](/python/azure/python-sdk-azure-authenticate?view=azure-python)
* [Ruby](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/)

## <a name="next-steps"></a>Další kroky
* Podrobné pokyny k integraci aplikace do Azure pro správu prostředků najdete v tématu [tooauthorization Příručka pro vývojáře s hello rozhraní API služby Azure Resource Manager](resource-manager-api-authentication.md).
* Podrobnější vysvětlení aplikací a objekty služby najdete v tématu [objekty aplikací a hlavní objekty služeb](../active-directory/active-directory-application-objects.md). 
* Další informace o ověřování Azure Active Directory najdete v tématu [scénáře ověřování pro Azure AD](../active-directory/active-directory-authentication-scenarios.md).
* Seznam dostupných akcí, které může být povolen nebo odepřen toousers najdete v tématu [poskytovatel prostředků Azure Resource Manager operations](../active-directory/role-based-access-control-resource-provider-operations.md).

