---
title: 'Azure Active Directory Domain Services: Funkce | Microsoft Docs'
description: "Funkce služby Azure Active Directory Domain Services"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 8d1c3eb3-1022-4add-a919-c98cc6584af1
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: maheshu
ms.openlocfilehash: 1a9417bafa35959d280eabf3db6ad8530db4ea45
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-domain-services"></a>Azure AD Domain Services
## <a name="features"></a>Funkce
Hello následující funkce jsou dostupné v Azure AD Domain Services spravované domény.

* **Jednoduché nasazení prostředí:** povolíte Azure AD Domain Services pro vašeho tenanta Azure AD pomocí několika kliknutí. Bez ohledu na to, jestli klientovi Azure AD je klienta cloudových nebo synchronizovány s místní adresář můžete rychle zřídit vaší spravované domény.
* **Podpora pro připojení k doméně:** můžete snadno připojení k doméně počítače v hello virtuální síť Azure je k dispozici ve vaší spravované domény. připojení k doméně prostředí Hello na Windows klient a Server operační systémy funguje bezproblémově proti domén, které jsou obsluhovány pomocí Azure AD Domain Services. Můžete také použít automatizované domény spojení tooling proti takové domény.
* **Jednu doménu instance pro každý adresář Azure AD:** můžete vytvořit jednu doménu služby Active Directory pro každý adresář Azure AD.
* **Vytvoření domény s vlastními názvy:** můžete vytvořit domény s vlastními názvy (například "contoso100.com") pomocí služby Azure AD Domain Services. Můžete použít buď názvy ověřené nebo neověřených domén. Volitelně můžete také vytvořit doménu s příponou domény předdefinované hello (to znamená, ' *. onmicrosoft.com ") nabízené adresáře Azure AD.
* **Integrované s Azure AD:** není nutné tooconfigure nebo Správa replikace tooAzure AD Domain Services. Uživatelské účty, členství ve skupinách a přihlašovací údaje uživatele (hesla) z adresáře služby Azure AD jsou automaticky dostupné v Azure AD Domain Services. Noví uživatelé, skupiny nebo tooattributes změny z vašeho klienta Azure AD nebo místním adresářem jsou automaticky synchronizované tooAzure AD Domain Services.
* **Ověřování NTLM a Kerberos:** s podporou pro ověřování NTLM a Kerberos, můžete nasazovat aplikace, které jsou závislé na integrované ověřování systému Windows.
* **Použít podnikové přihlašovací údaje a hesla:** hesla pro uživatele ve službě Azure AD klienta pracovat s Azure AD Domain Services. Uživatele můžete používat své podnikové přihlašovací údaje toodomain spojení počítače, přihlásit interaktivně nebo přes vzdálené plochy a ověřování na základě hello spravované domény.
* **Vazba LDAP & LDAP přečíst podpory:** používáte aplikace, které jsou závislé na vazby LDAP tooauthenticate uživatelé v doménách, které jsou obsluhovány pomocí Azure AD Domain Services. Kromě toho aplikace, které používají LDAP přečíst operace, které atributy uživatele nebo počítače tooquery z adresáře hello může spolupracovat taky s Azure AD Domain Services.
* **Zabezpečený LDAP (LDAPS):** můžete povolit přístup k adresáři toohello přes zabezpečený LDAP (LDAPS). Zabezpečený LDAP přístup je k dispozici v rámci virtuální sítě hello ve výchozím nastavení. Však také volitelně můžete umožnit zabezpečený přístup protokolu LDAP přes hello Internetu.
* **Zásady skupiny:** můžete použít jeden předdefinovaný objektu zásad skupiny každý hello uživatelů a počítačů kontejnery tooenforce souladu se zásadami zabezpečení požadovaná pro počítače připojené k doméně a uživatelské účty. Můžete také vytvořit vlastní vlastní objekty zásad skupiny a přiřadit je příliš organizační jednotky toocustom[správu zásad skupiny](active-directory-ds-admin-guide-administer-group-policy.md).
* **Umožňuje spravovat DNS:** členům skupiny hello 'Administrators AAD řadič domény, můžete spravovat DNS pro vaše spravované doméně pomocí známých DNS správy nástroje, například hello modul snap-in konzoly MMC Správa DNS.
* **Vytvořit vlastní organizačních jednotek (OU):** členům skupiny hello 'Administrators AAD řadič domény, můžete vytvořit vlastní organizační jednotky v hello spravované domény. Tito uživatelé jsou udělena úplná oprávnění správce přes vlastní organizační jednotky, takže se můžete přidat nebo odebrat účty služeb, počítačů, skupin atd. v rámci těchto vlastních organizačních jednotek.
* **K dispozici v několika oblastmi Azure:** najdete hello [služby Azure podle oblasti](https://azure.microsoft.com/regions/#services/) tooknow stránku hello oblastí Azure, ve kterých je dostupná služba Azure AD Domain Services.
* **Vysoká dostupnost:** Azure AD Domain Services nabízí vysokou dostupnost vaší domény. Tato funkce nabízí hello zárukou toho vyšší dostupnost a odolnost proti toofailures služby. Předdefinované stavu monitorování nabízí automatizovanou nápravu selhání roztočený nové instance služby se nezdařilo tooreplace instancí a tooprovide pokračování služby pro vaši doménu.
* **Pomocí nástroje pro správu známé:** známých nástrojů pro správu systému Windows Server Active Directory můžete použít například hello Centrum správy služby Active Directory nebo Active Directory PowerShell tooadminister spravované domény.
