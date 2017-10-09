---
title: "aaaFederating více Azure AD s jedné služby AD FS | Microsoft Docs"
description: "V tomto dokumentu se dozvíte, jak toofederate více Azure AD s jedné služby AD FS."
keywords: "vytvoření federace, ADFS, AD FS, více klientů, jedna služba AD FS, jedna služba ADFS, federace s více klienty, ADFS s více doménovými strukturami, připojení AAD, federace, federace mezi klienty"
services: active-directory
documentationcenter: 
author: anandyadavmsft
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/17/2017
ms.author: anandy; billmath
ms.openlocfilehash: 442192896b3b13f7bf9388396cd3769e194329d4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
#<a name="federate-multiple-instances-of-azure-ad-with-single-instance-of-ad-fs"></a>Vytvoření federace několika instancí Azure AD s jednou instancí AD FS

Jedna farma AD FS s vysokou dostupností může federovat několik doménových struktur, pokud mezi nimi existuje obousměrný vztah důvěryhodnosti. Tyto několik doménových struktur může nebo nemusí odpovídat toohello stejné Azure Active Directory. Tento článek obsahuje pokyny jak tooconfigure federace mezi jedno nasazení služby AD FS a více doménových strukturách toodifferent této synchronizace Azure AD.

![Federace více klientů s jednou službou AD FS](media/active-directory-aadconnectfed-single-adfs-multitenant-federation/concept.png)
 
> [!NOTE]
> Zpětný zápis zařízení a automatické připojení zařízení nejsou v tomto scénáři podporovány.

> [!NOTE]
> Azure AD Connect nelze použít tooconfigure federation v tomto scénáři, protože Azure AD Connect můžete konfiguraci federace domén v jednom Azure AD.

##<a name="steps-for-federating-ad-fs-with-multiple-azure-ad"></a>Kroky pro vytvoření federace AD FS s více službami Azure AD

Vezměte v úvahu, že doménu contoso.com v Azure Active Directory contoso.onmicrosoft.com je již sdružených se službou místně nainstalován v prostředí služby Active Directory v místě contoso.com hello služby AD FS. Fabrikam.com je doména ve službě Azure Active Directory fabrikam.onmicrosoft.com.

##<a name="step-1-establish-a-two-way-trust"></a>Krok 1: Vytvoření obousměrného vztahu důvěryhodnosti
 
Pro službu AD FS v contoso.com toobe možné tooauthenticate uživatelé v fabrikam.com je potřeba obousměrný vztah důvěryhodnosti mezi contoso.com a fabrikam.com. Postupujte podle obecných zásad hello v tomto [článku](https://technet.microsoft.com/library/cc816590.aspx) toocreate hello obousměrný vztah důvěryhodnosti.
 
##<a name="step-2-modify-contosocom-federation-settings"></a>Krok 2: Úprava nastavení federace contoso.com 
 
Vystavitel Hello výchozí nastavení pro jednu doménu federovaný tooAD FS je "http://ADFSServiceFQDN/adfs/services/trust", například "http://fs.contoso.com/adfs/services/trust". Azure Active Directory vyžaduje jedinečného vystavitele pro každou federovanou doménu. Hodnota vystavitele hello hello stejné služby AD FS bude toofederate dvě domény, musí toobe upravit tak, aby je jedinečný pro každou doménu, kterou federates služby AD FS se službou Azure Active Directory. 
 
Na serveru hello služby AD FS otevřete Azure AD PowerShell a provádět hello následující kroky:
 
Připojit toohello Azure Active Directory, která obsahuje hello domény contoso.com Connect-MsolService hello federační nastavení aktualizace pro doménu contoso.com aktualizace MsolFederatedDomain - DomainName contoso.com – SupportMultipleDomain
 
Změní vystavitele v nastavení federace domén hello příliš "http://contoso.com/adfs/services/trust" a vystavování deklarace identity přidá se pravidlo pro hello Azure AD vztah důvěryhodnosti předávající strany tooissue hello správná hodnota issuerId založená na příponu UPN hello.
 
##<a name="step-3-federate-fabrikamcom-with-ad-fs"></a>Krok 3: Vytvoření federace domény fabrikam.com se službou AD FS
 
V Azure AD powershell relace provést následující kroky hello: připojení tooAzure služby Active Directory, který obsahuje fabrikam.com domény hello

    Connect-MsolService
Převeďte hello fabrikam.com spravované domény toofederated:

    Convert-MsolDomainToFederated -DomainName anandmsft.com -Verbose -SupportMultipleDomain
 
Hello výše operace bude federaci hello domény fabrikam.com s hello stejné služby AD FS. Nastavení domény hello můžete ověřit pomocí příkazu Get-MsolDomainFederationSettings pro obě domény.

## <a name="next-steps"></a>Další kroky
[Připojení Active Directory s Azure Active Directory](active-directory-aadconnect.md)
