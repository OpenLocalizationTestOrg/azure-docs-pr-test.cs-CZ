---
title: "resetování hesla pomocí samoobslužné služby aaaAzure AD přehled | Microsoft Docs"
description: "Co můžete dělat samoobslužné služby heslo resetovat ve službě Azure AD pro vaši organizaci?"
services: active-directory
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: gahug
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 0efc291b1eeec0b7ae33ff5a7d9ed38e70c8be6d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-self-service-password-reset-for-hello-it-professional"></a>Azure AD samoobslužného resetování hesla pro odborníky v oblasti IT hello

"Samoobslužné služby" je vyvolána kolem uvnitř řadu IT oddělení mezi hello, world s různými významy buzzword. Hello trhu je přenášeny s produkty, které vám umožňují toomanage místní skupiny, hesla nebo profilů uživatelů z hello cloudu nebo místně.

Azure Active Directory (Azure AD) samoobslužné resetování hesla (SSPR) nastaví sám od sebe s snadné použití a nasazení. Heslo samoobslužné služby Azure AD resetování kombinuje sadu funkcí, které:

* Povolit vaši uživatelé toomanage své vlastní heslo
  * Z libovolného zařízení
  * V žádném z umístění
  * Kdykoli
* Udržovat kompatibilitu se zásadami, které je jako správce definovat

Pokud je všechno připravené, abyste mohli začít s Azure AD SSPR pomocí našich [rychlý start pokyny](active-directory-passwords-getting-started.md) a rychle uživatelům resetovat vlastní hesla.

## <a name="what-is-possible"></a>Co je to možné

* **Změna hesla pomocí samoobslužné služby** umožňuje koncoví uživatelé nebo správci toochange bez pomoci správce hesel
* **Odemknutí účtu samoobslužné služby** umožňuje koncovým uživatelům toounlock svůj vlastní účet bez pomoci správce
* **Resetování hesla pomocí samoobslužné služby** umožňuje koncoví uživatelé nebo správci tooreset hesel automaticky bez pomoci správce. Samoobslužné resetování hesla vyžaduje Azure AD Premium nebo Basic - [edice služby Azure Active Directory](active-directory-editions.md).
* **Resetování hesla iniciované správcem** umožňuje tooreset správce koncového uživatele nebo jiný správce heslo z aplikace hello [portálu Azure](https://docs.microsoft.com/azure/azure-portal-overview)
* **Sestavy aktivit správy heslo** udělte správci přehledy aktivita resetování a registraci hesla ke kterým dochází ve své organizaci - [sestavy správy](active-directory-passwords-reporting.md)
* **Zpětný zápis hesla** umožňuje správu místních hesel z cloudu hello tak, aby všechny předcházející hello scénáře lze provést, nebo v zastoupení hello, federovaný nebo heslo synchronizované uživatele. Zpětný zápis hesla vyžaduje [Azure AD Premium](active-directory-get-started-premium.md).

## <a name="why-choose-azure-ad-self-service-password-reset"></a>Proč zvolit Azure AD samoobslužného resetování hesel

* **Snížení nákladů** jako helpdesk a podporu resetování hesla odbornou je obvykle 20 % IT organizace potřebují
* **Vylepšení prostředí pro koncové uživatele** a **snížit helpdesk svazku** tím, že koncoví uživatelé hello power tooresolve své vlastní heslo problémy najednou bez volání technickou podporu nebo otevření žádosti o podporu.
* **Jednotka mobility** jako uživatelům můžete resetovat vlastní hesla ze kdekoli jsou.

## <a name="azure-ad-self-service-password-reset-availability"></a>Dostupnost resetování hesel samoobslužné služby Azure AD

Heslo samoobslužné služby Azure AD vynulování je k dispozici ve třech úrovních v závislosti na vaše předplatné.

* **Azure AD volné** – jenom pro Cloud správci můžou resetovat vlastní hesla
* **Azure AD Basic** nebo jakoukoli **placené předplatné Office 365** – jenom pro Cloud uživatelům a správcům jenom pro cloud, můžete resetovat vlastní hesla
* **Azure AD Premium** – všechny uživatele nebo správce, včetně jenom pro cloud, federovaný, nebo heslo synchronizované uživatele, můžete resetovat vlastní hesla. Místních hesel vyžadují heslo toobe zpětný zápis povolen.

## <a name="azure-ad-self-service-password-reset-a-sum-of-hello-parts"></a>Azure AD samoobslužného resetování hesel, součet částí hello

Samoobslužné služby ve službě Azure AD pro vytvoření nového hesla se skládá z hello následující součásti:

* **Konfigurace portálu pro správu hesel** kde můžete řídit možnosti pro způsob správy hesel v klientovi prostřednictvím hello portálu Azure
* **Portál pro registraci a resetování hesla** které uživatelé mohou registrovat pro resetování hesla
* **Portál pro resetování hesla** zadali kde uživatelům můžete resetovat heslo pomocí hello výzvy definované správcem hello a hello odpovědi uživatele
* **Uživatel heslo změnit portál** které mohou uživatelé změnit jejich hesla tak, že zadáte svoje staré heslo a poskytuje nové heslo
* **Sestavy správy heslo** kde správci mohou zobrazit a analyzovat aktivita hesla sestav pro své klienty v hello portálu Azure
* **Heslo zpětný zápis tooon místní pomocí služby Azure AD Connect** umožňuje vám tooenable správu místně federovaný, nebo heslo synchronizovat uživatele z cloudu hello

## <a name="azure-ad-pricing-sla-updates-and-roadmap"></a>Azure AD ceny, smlouvy o úrovni služeb, aktualizace a plán

Další podrobnosti o těchto tématech naleznete na následujících stránkách hello

* [**Podrobnosti o cenách Azure AD**](https://azure.microsoft.com/pricing/details/active-directory/)
* [**Ceny Office 365**](https://products.office.com/compare-all-microsoft-office-products?tab=2)
* [**Smlouvy o úrovni služeb Azure**](https://azure.microsoft.com/support/legal/sla/)
* [**Dohoda o úrovni služeb pro Microsoft Online Services**](http://go.microsoft.com/fwlink/?LinkID=272026&clcid=0x409)
* [**Azure aktualizace**](https://azure.microsoft.com/updates/)
* [**Přehled Azure**](https://www.microsoft.com/cloud-platform/roadmap-recently-available)

## <a name="next-steps"></a>Další kroky

Hello následující odkazy obsahují další informace o resetování hesla pomocí služby Azure AD

* [**Rychlý Start**](active-directory-passwords-getting-started.md) – Zprovozněte samoobslužné resetování hesla Azure AD. 
* [**Správa licencí**](active-directory-passwords-licensing.md) – Konfigurujte licencování Azure AD.
* [**Data** ](active-directory-passwords-data.md) – pochopit hello data, která je požadována a jak se používají pro správu hesel
* [**Zavedení** ](active-directory-passwords-best-practices.md) -plánování a nasazení SSPR tooyour uživatelů podle pokynů hello je zde uveden
* [**Přizpůsobení** ](active-directory-passwords-customize.md) -přizpůsobit hello vzhledu a chování hello SSPR prostředí pro vaši společnost.
* [**Vytváření sestav**](active-directory-passwords-reporting.md) – Zjistěte, jestli, kdy a kde vaši uživatelé používají funkci samoobslužného resetování hesla.
* [**Podrobné technické informace** ](active-directory-passwords-how-it-works.md) -přejděte za hello závěsem toounderstand, jak to funguje
* [**Nejčastější dotazy**](active-directory-passwords-faq.md) – Jak? Proč? Co? Kde? Kdo? Kdy? -Odpovědi tooquestions vždy chtěli tooask
* [**Řešení potíží s** ](active-directory-passwords-troubleshoot.md) – zjistěte, jak tooresolve běžné problémy, že vidíte s SSPR

