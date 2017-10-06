---
title: "aaaGet dat pomocí Azure AD Reporting rozhraní API s certifikáty hello | Microsoft Docs"
description: "Vysvětluje, jak toouse hello Azure AD Reporting rozhraní API s daty tooget certifikát přihlašovacích údajů z adresáře bez zásahu uživatele."
services: active-directory
documentationcenter: 
author: ramical
writer: v-lorisc
manager: kannar
ms.assetid: 
ms.service: active-directory
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/24/2017
ms.author: ramical
ms.openlocfilehash: 00ddfaefe32ea6ae48f276c974a17ddcf84f7894
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-data-using-hello-azure-ad-reporting-api-with-certificates"></a>Získání dat pomocí rozhraní API generování sestav Azure AD hello s certifikáty
Tento článek popisuje, jak toouse hello Azure AD Reporting rozhraní API s daty tooget certifikát přihlašovacích údajů z adresáře bez zásahu uživatele. 

## <a name="use-hello-azure-ad-reporting-api"></a>Použití hello rozhraní API pro vytváření sestav Azure AD 
Rozhraní API pro vytváření sestav Azure AD vyžaduje, že provedete následující kroky hello:
 *  Požadavky na instalaci
 *  Nastavte hello certifikát v aplikaci
 *  Získání přístupového tokenu
 *  Použití hello přístup tokenu toocall hello rozhraní Graph API

Informace o zdrojovém kódu najdete v tématu popisujícím [využití modulu rozhraní API pro sestavy](https://github.com/AzureAD/azure-activedirectory-powershell/tree/gh-pages/Modules/AzureADUtils). 

### <a name="install-prerequisites"></a>Požadavky na instalaci
Budete potřebovat toohave Azure AD PowerShell V2 a AzureADUtils modul nainstalovaný.

1. Stáhněte a nainstalujte Azure AD Powershell V2, pokynů hello v [Azure Active Directory PowerShell](https://github.com/Azure/azure-docs-powershell-azuread/blob/master/Azure AD Cmdlets/AzureAD/index.md).
2. Stáhnout modul Azure AD Utils hello z [AzureAD/azure-Active Directory powershell](https://github.com/AzureAD/azure-activedirectory-powershell/blob/gh-pages/Modules/AzureADUtils/AzureADUtils.psm1). 
  Tento modul poskytuje několik rutin nástrojů, jako jsou:
   * Hello nejnovější verzi pomocí nástroje Nuget ADAL
   * Přístupové tokeny od uživatele, klíče aplikace a certifikáty pomocí ADAL
   * Rozhraní Graph API zpracovávající stránkové výsledky

**modul Azure AD Utils tooinstall hello:**

1. Vytvořte modul directory toosave hello nástroje (například c:\azureAD) a modulu hello stáhnout z webu GitHub.
2. Otevřete relaci prostředí PowerShell a přejděte toohello adresář, který jste právě vytvořili. 
3. Naimportovat modul hello a nainstalujte ho v cestě modulu prostředí PowerShell hello pomocí rutiny Install-AzureADUtilsModule hello. 

relace Hello by měl vypadat podobně jako toothis obrazovky:

  ![Windows Powershell](./media/active-directory-report-api-with-certificates/windows-powershell.png)

### <a name="set-hello-certificate-in-your-app"></a>Nastavte hello certifikát v aplikaci
1. Pokud již máte aplikaci, získáte jeho ID objektu z hello portálu Azure. 

  ![portál Azure](./media/active-directory-report-api-with-certificates/azure-portal.png)

2. Otevřete relaci prostředí PowerShell a připojte tooAzure AD pomocí rutiny hello Connect-AzureAD.

  ![portál Azure](./media/active-directory-report-api-with-certificates/connect-azuaread-cmdlet.png)

3. Pomocí rutiny New-AzureADApplicationCertificateCredential hello z AzureADUtils tooadd tooit přihlašovacích údajů certifikátu. 

>[!Note]
>Budete potřebovat aplikace hello tooprovide ID objektu, kterou jste zachytili dříve, jakož i hello objekt certifikátu (získat této konfigurace pomocí hello Cert: jednotku).
>


  ![portál Azure](./media/active-directory-report-api-with-certificates/add-certificate-credential.png)
  
### <a name="get-an-access-token"></a>Získání přístupového tokenu

tooget přístupový token, použijte rutinu Get-AzureADGraphAPIAccessTokenFromCert hello z AzureADUtils. 

>[!NOTE]
>Je nutné toouse hello ID aplikace místo hello ID objektu, který jste použili v poslední části hello.
>

 ![portál Azure](./media/active-directory-report-api-with-certificates/application-id.png)

### <a name="use-hello-access-token-toocall-hello-graph-api"></a>Použití hello přístup tokenu toocall hello rozhraní Graph API

Nyní můžete vytvořit skript hello. Dole je příklad pomocí rutiny Invoke-AzureADGraphAPIQuery hello z hello AzureADUtils. Tato rutina zpracovává více stránkových výsledků a pak odešle tyto výsledky toohello prostředí PowerShell kanálu. 

 ![portál Azure](./media/active-directory-report-api-with-certificates/script-completed.png)

Jsou nyní připraven tooexport tooa sdíleného svazku clusteru a uložte tooa systému SIEM. Můžete také zabalit vašeho skriptu do tooget naplánované úlohy služby Azure AD data z vašeho klienta pravidelně bez nutnosti toostore aplikace klíče ve zdrojovém kódu hello. 

## <a name="next-steps"></a>Další kroky
[Hello základy Azure identity management.](https://docs.microsoft.com/en-us/azure/active-directory/fundamentals-identity)<br>



