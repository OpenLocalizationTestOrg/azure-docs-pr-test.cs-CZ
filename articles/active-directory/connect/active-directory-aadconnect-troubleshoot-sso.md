---
title: "Azure AD Connect: Řešení potíží s bezproblémové jednotné přihlašování | Microsoft Docs"
description: "Toto téma popisuje, jak tootroubleshoot Azure Active Directory bezproblémové jednotné přihlašování (Azure AD bezproblémové jednotné přihlašování)."
services: active-directory
keywords: "Co je Azure AD Connect, instalace služby Active Directory, požadované součásti pro Azure AD, jednotné přihlašování, jednotné přihlašování"
documentationcenter: 
author: swkrish
manager: femila
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2017
ms.author: billmath
ms.openlocfilehash: f1c1c11522f22d5bc742c126fff483c5b06e1f06
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-azure-active-directory-seamless-single-sign-on"></a>Řešení potíží s Azure Active Directory bezproblémové jednotné přihlašování

Tento článek vám pomůže najít informace o běžných problémech týkající se Azure AD bezproblémové jednotného přihlašování k řešení potíží.

## <a name="known-issues"></a>Známé problémy

- Pokud se synchronizace 30 nebo více doménových struktur služby AD, nelze povolit bezproblémové jednotného přihlašování pomocí služby Azure AD Connect. Jako alternativní řešení můžete [ručně povolit](#manual-reset-of-azure-ad-seamless-sso) hello funkce na klientovi.
- Přidání Azure AD služba adresy URL (https://autologon.microsoftazuread-sso.com, https://aadg.windows.net.nsatc.net) toohello "zóny Důvěryhodné servery" namísto zóny "Místní intranet" hello **blokuje uživatelům v přihlašování**.
- Bezproblémové SSO nefunguje v privátním režimu procházení na Firefox a okraj. A také v Internet Exploreru po zapnutí režimu rozšířené ochrany.

>[!IMPORTANT]
>Jsme nedávno vrácena podporu pro hraniční tooinvestigate zákazníka hlášené problémy.

## <a name="check-status-of-hello-feature"></a>Zkontrolujte stav funkce hello

Ujistěte se, tato funkce bezproblémové SSO hello je stále **povoleno** na klientovi. Stav můžete zkontrolovat pomocí budete toohello **Azure AD Connect** okně hello [centra pro správu Azure Active Directory](https://aad.portal.azure.com/).

![Azure Centrum pro správu služby Active Directory – okno Azure AD Connect](./media/active-directory-aadconnect-sso/sso10.png)

## <a name="sign-in-failure-reasons-on-hello-azure-active-directory-admin-center"></a>Chyba přihlášení důvodů v Centru pro správu Azure Active Directory hello

Dobrým místem toostart, řešení potíží s uživatele přihlásit pomocí bezproblémové jednotného přihlašování je toolook v hello [přihlašovací aktivita sestavy](../active-directory-reporting-activity-sign-ins.md) na hello [centra pro správu Azure Active Directory](https://aad.portal.azure.com/).

![Azure Active Directory Centrum pro správu – sestava přihlášení](./media/active-directory-aadconnect-sso/sso9.png)

Přejděte příliš**Azure Active Directory** -> **přihlášení** na hello [centra pro správu Azure Active Directory](https://aad.portal.azure.com/) a klikněte na aktivitu přihlášení příslušného uživatele. Vyhledejte hello **kód chyby SIGN-IN** pole. Mapování hello hodnotu tohoto pole důvodem selhání tooa a řešení pomocí hello následující tabulka:

|Kód chyby přihlášení|Důvod selhání přihlášení|Řešení
| --- | --- | ---
| 81001 | Lístek Kerberos uživatele je příliš velký. | Snižte členství uživatele ve skupině a zkuste to znovu.
| 81002 | Uživatele nelze toovalidate lístek protokolu Kerberos. | V tématu [řešení potíží s kontrolní seznam](#troubleshooting-checklist).
| 81003 | Uživatele nelze toovalidate lístek protokolu Kerberos. | V tématu [řešení potíží s kontrolní seznam](#troubleshooting-checklist).
| 81004 | Pokus o ověření protokolu Kerberos selhal. | V tématu [řešení potíží s kontrolní seznam](#troubleshooting-checklist).
| 81008 | Uživatele nelze toovalidate lístek protokolu Kerberos. | V tématu [řešení potíží s kontrolní seznam](#troubleshooting-checklist).
| 81009 | "Nelze toovalidate uživatele lístek protokolu Kerberos. | V tématu [řešení potíží s kontrolní seznam](#troubleshooting-checklist).
| 81010 | Bezproblémové jednotné přihlašování se nezdařilo, protože lístek protokolu Kerberos hello uživatele vypršela nebo je neplatný. | Uživatel musí toosign v ze zařízení připojeného k doméně uvnitř firemní sítě.
| 81011 | Objekt nelze toofind uživatele na základě informací v lístku protokolu Kerberos hello uživatele. | Použijte Azure AD Connect toosynchronize uživatelské informace do služby Azure AD.
| 81012 | Hello uživatele při toosign v tooAzure AD se liší od uživatele hello přihlásili hello zařízení. | Přihlásit z jiného zařízení.
| 81013 | Objekt nelze toofind uživatele na základě informací v lístku protokolu Kerberos hello uživatele. |Použijte Azure AD Connect toosynchronize uživatelské informace do služby Azure AD. 

## <a name="troubleshooting-checklist"></a>Řešení potíží s kontrolní seznam

Použijte následující kontrolní seznam tootroubleshoot bezproblémové SSO problémy hello:

- Zkontrolujte, zda je povolena funkce hello bezproblémové jednotného přihlašování k v Azure AD Connect. Pokud nelze povolit funkci hello (například z důvodu tooa blokované port), ujistěte se, že máte všechny hello [předpoklady](active-directory-aadconnect-sso-quick-start.md#step-1-check-prerequisites) na místě.
- Zkontrolujte, zda jsou obě tyto Azure AD adresy URL (https://autologon.microsoftazuread-sso.com a https://aadg.windows.net.nsatc.net) součástí nastavení zóny intranetu hello uživatele.
- Zajistěte, aby hello podnikových zařízení je připojené k toohello AD domény.
- Zkontrolujte, zda je přihlášený uživatel hello toohello zařízení pomocí účtu domény AD.
- Zkontrolujte, že hello uživatelský účet z doménové struktury služby AD, kde byl bezproblémové SSO nastavená.
- Zkontrolujte, zda hello podnikové síti připojeno zařízení hello.
- Zajistěte, aby čas hello zařízení je synchronizován s časem hello služby Active Directory a řadiče domény hello a během pěti minut vzájemně.
- Seznam existujících lístků protokolu Kerberos na zařízení hello pomocí hello **klist** příkazu z příkazového řádku. Zkontrolujte, pokud lístky vydané pro hello `AZUREADSSOACCT` účet počítače jsou k dispozici. Lístky protokolu Kerberos uživatelů jsou obvykle platné po dobu 12 hodin. Můžete mít různá nastavení ve službě Active Directory.
- Vymazání existujících lístků protokolu Kerberos z hello zařízení pomocí hello **klist mazání** příkaz a akci opakujte.
- toodetermine, pokud existují problémy související s JavaScript, zkontrolujte protokoly konzoly hello hello prohlížeče (v části "Developer Tools").
- Zkontrolujte hello [řadič domény](#domain-controller-logs) také.

### <a name="domain-controller-logs"></a>Řadič domény

Pokud je auditování úspěšných je povoleno na vašem řadiči domény, pak pokaždé, když se uživatel přihlásí pomocí bezproblémové jednotného přihlašování k položku zabezpečení se zaznamená do protokolu událostí hello. Můžete najít těchto událostí zabezpečení pomocí hello následující dotaz (vyhledejte událost **4769** přidružené k účtu počítače hello **AzureADSSOAcc$**):

```
    <QueryList>
      <Query Id="0" Path="Security">
    <Select Path="Security">*[EventData[Data[@Name='ServiceName'] and (Data='AZUREADSSOACC$')]]</Select>
      </Query>
    </QueryList>
```

## <a name="manual-reset-of-hello-feature"></a>Ruční vynulování hello funkce

Pokud vám nepomohly řešení potíží, můžete ručně obnovit hello funkce na klientovi. Pomocí těchto kroků hello na místní server, kde je spuštěn nástroj Azure AD Connect:

### <a name="step-1-import-hello-seamless-sso-powershell-module"></a>Krok 1: Naimportovat modul hello bezproblémové prostředí PowerShell pro jednotné přihlašování

1. Nejprve stáhnout a nainstalovat hello [Microsoft Online Services Sign-In Assistant](http://go.microsoft.com/fwlink/?LinkID=286152).
2. Potom stáhněte a nainstalujte hello [64-bit modulu Azure Active Directory pro prostředí Windows PowerShell](http://go.microsoft.com/fwlink/p/?linkid=236297).
3. Přejděte toohello `%programfiles%\Microsoft Azure Active Directory Connect` složky.
4. Import hello bezproblémové prostředí PowerShell jednotného přihlašování k modulu pomocí tohoto příkazu: `Import-Module .\AzureADSSO.psd1`.

### <a name="step-2-get-hello-list-of-ad-forests-on-which-seamless-sso-has-been-enabled"></a>Krok 2: Získání seznamu hello doménových struktur služby AD, u kterých je povolena bezproblémové jednotného přihlašování

1. Spusťte prostředí PowerShell jako správce. V prostředí PowerShell, zavolejte `New-AzureADSSOAuthenticationContext`. Po zobrazení výzvy zadejte přihlašovací údaje globálního správce vašeho klienta.
2. Volání `Get-AzureADSSOStatus`. Tento příkaz zjistí, zda že text hello seznam doménových struktur AD (podívejte se na seznam domén"hello") na které tato funkce povolená.

### <a name="step-3-disable-seamless-sso-for-each-ad-forest-that-it-was-set-it-up-on"></a>Krok 3: Zakázat bezproblémové jednotného přihlašování pro každou doménovou strukturu AD, který ho se ho nastavit na

1. Volání `$creds = Get-Credential`. Po zobrazení výzvy zadejte přihlašovací údaje správce domény hello pro hello určené doménové struktuře Active Directory.
2. Volání `Disable-AzureADSSOForest -OnPremCredentials $creds`. Tento příkaz odebere hello `AZUREADSSOACCT` účet počítače z hello místní řadič domény pro tento konkrétní doménovou strukturu AD.
3. Opakováním předchozích kroků pro každou doménovou strukturu AD, který jste nastavili hello funkce na hello.

### <a name="step-4-enable-seamless-sso-for-each-ad-forest"></a>Krok 4: Povolení bezproblémové jednotného přihlašování pro každou doménovou strukturu AD

1. Volání `Enable-AzureADSSOForest`. Po zobrazení výzvy zadejte přihlašovací údaje správce domény hello pro hello určené doménové struktuře Active Directory.
2. Opakováním předchozích kroků pro každou doménovou strukturu AD, které chcete tooset až hello funkce na hello.

### <a name="step-5-enable-hello-feature-on-your-tenant"></a>Krok 5. Povolit funkci hello na vašeho klienta

Volání `Enable-AzureADSSO` a zadejte hodnotu "true" v hello `Enable: ` výzva tooturn hello funkce ve vašem klientovi.
