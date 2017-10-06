---
title: "aspekty návrhu rozšíření aaaAzure služby Active Directory hybridní identity - stanovit požadavky na identity | Microsoft Docs"
description: "Identifikace obchodních potřeb hello společnosti, které povede toodefine hello požadavky hello hybridní identity návrh."
documentationcenter: 
services: active-directory
author: billmath
manager: femila
editor: 
ms.assetid: de690978-84ef-41ad-9dfe-785722d343a1
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: b2f1cad923b0f08ededa0d8f9a4ea8e799956e54
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="determine-identity-requirements-for-your-hybrid-identity-solution"></a>Stanovení požadavků na identit pro hybridní řešení identit
Hello prvním krokem při navrhování řešení hybridní identity je toodetermine hello požadavky pro organizaci hello firmy, která bude mít využívání tohoto řešení.  Hybridní identita se spustí jako podpůrné role (podporuje jiných řešení cloud tím, že poskytuje ověřování) a přejde na tooprovide nové a zajímavé funkce, které odemknutí nové úlohy pro uživatele.  Tyto úlohy nebo služby, které chcete tooadopt pro vaše uživatele, bude určovat hello požadavky hello hybridní identity návrh.  Tyto služby a úlohy potřebují tooleverage hybridní identita jak místně a v cloudu hello.  

Toogo potřebují přes tyto klíčové aspekty hello firmy toounderstand co je nyní požadavek a co hello společnosti plánuje budoucí hello. Pokud nemáte hello viditelnost hello dlouhodobou strategii pro návrhu hybridní identity, je pravděpodobné, že vaše řešení nebude škálovatelné podle hello podnikání růst a měnit.   T mu obrázek níže znázorňuje příklad hybridní identity architektura a hello úloh, které jsou odemykají pro uživatele. Toto je jenom jako příklad všechny nové možnosti hello, které můžete odemknout a doručit s strategie plnou hybridní identity. 

Některé součásti, které jsou součástí architektury hello hybridní identity![](./media/hybrid-id-design-considerations/hybrid-identity-architechture.png)

## <a name="determine-business-needs"></a>Určení obchodních potřeb
Každá společnost má jiné požadavky, i když jsou tyto společnosti součástí hello stejného odvětví, skutečné obchodní požadavky hello mohou lišit. Vždy můžete využít osvědčené postupy hello odvětví, ale výsledku jsou to obchodní potřeby společnosti hello, které povede toodefine hello požadavky hello hybridní identity návrh. 

Zajistěte, aby tooanswer hello následující otázky tooidentify vaší firmě musí:

* Je vaše společnost hledáte toocut IT provozní náklady?
* Je vaše společnost hledáte toosecure cloudové prostředky (aplikace SaaS, infrastruktury)?
* Je vaše společnost vyhledávání toomodernize IT?
  * Jsou vaši uživatelé více mobilních a náročné IT toocreate výjimky do vaší hraniční sítě tooallow jiný typ provozu tooaccess různých prostředků?
  * Má vaše společnost nějaké starší verze aplikace, které potřeby toobe publikovaná toothese moderní ale nebudou se uživatelé snadno toorewrite?
  * Nepodporuje vaše společnost třeba tooaccomplish všechny tyto úkoly a ho převést pod řízení na hello současně?
* Je vaše společnost vyhledávání identity toosecure uživatelů a snížení rizika tak, že převedou nové nástroje, které využívají hello znalosti společnosti Microsoft Azure zabezpečení znalosti místního?
* Je vaší společnosti při tooget zbavit hello dreaded "externí" účty místně a přesuňte je tam, kde jsou už spících hrozeb v místním prostředí cloudu toohello?

## <a name="analyze-on-premises-identity-infrastructure"></a>Analýza na místní infrastrukturu identity
Teď, když máte představu o obchodních požadavků vaší společnosti, musíte tooevaluate na místní infrastruktuře identity. Toto testování je důležité pro váš aktuální identitu řešení toohello cloudové identity systém správy toointegrate technické požadavky hello. Ujistěte se, zda text hello tooanswer následující otázky:

* Jaké řešení ověřování a autorizace nemá vaše společnost používat místní? 
* Vaše společnost aktuálně nemá žádné místní služby synchronizace?
* Používá vaše společnost všech poskytovatelů identit třetích stran (IdP)?

Budete také potřebovat toobe vědět hello cloudové služby, které vaše společnost může mít. Provádění assessment toounderstand hello aktuální integrační s SaaS, je velmi důležité IaaS nebo PaaS modely ve vašem prostředí. Ujistěte se, zda tooanswer hello během hodnocení následující otázky:

* Má vaše společnost žádnou integraci s poskytovatele cloudové služby?
* Pokud ano, které služby jsou používány?
* Tato integrace je aktuálně v produkčním prostředí, nebo je pilotní nasazení?

> [!NOTE]
> Pokud nemáte přesné mapování u všech aplikací a cloudové služby, můžete použít nástroj hello Cloud App Discovery. Tento nástroj můžete poskytnout přehled o všech vaší organizace a příjemce cloudových aplikací vaše IT oddělení. Tím je jednodušší než kdy toodiscover stínové IT ve vaší organizaci, včetně podrobností o způsobu použití a všechny uživatele, kteří používají cloudové aplikace. najdete v části tooget spuštění [Cloud app discovery.](active-directory-cloudappdiscovery-whatis.md).
> 
> 

## <a name="evaluate-identity-integration-requirements"></a>Vyhodnotit požadavky na integraci identity
Dále je nutné požadavky na integraci identity tooevaluate hello. Toto testování je důležité toodefine hello technické požadavky na tom, jak budou uživatelé ověřovat, vzhled přítomnosti hello organizace v cloudu hello, jak hello organizaci umožní autorizace a jaké hello uživatelské prostředí je toobe probíhající. Ujistěte se, zda text hello tooanswer následující otázky:

* Bude vaše organizace používat federační, standardní ověřování nebo obojí?
* Je federation požadavek?  Z důvodu hello následující:
  * Jednotné přihlašování založené na protokolu Kerberos
  * Má vaše společnost místní aplikace (buď vytvořen interní nebo 3. stran), která používá SAML nebo podobné možnosti federace.
  * Vícefaktorové ověřování pomocí čipové karty. RSA SecurID atd.
  * Pravidla klientské přístupu, které řeší hello níže uvedené otázky:
    1. Můžete blokovat všechny externí přístup tooOffice 365 na základě IP adresy hello hello klienta?
    2. Můžete blokovat všechny externí přístup tooOffice 365, kromě Exchange ActiveSync?
    3. Můžete blokovat všechny externí přístup tooOffice 365, s výjimkou aplikace založené na prohlížeči (OWA, SPO)
    4. Můžete blokovat všechny externí přístup tooOffice 365 pro členy skupiny určené AD
* Otázky zabezpečení nebo auditování
* Stávající investice do federovaného ověřování
* Zadejte název bude naše organizace používat pro naše doménu v cloudu hello?
* Má hello organizace vlastní doménu?
  1. Veřejné a snadno ověřitelný pomocí DNS, je této domény?
  2. Pokud není, pak máte veřejné domény, kterou lze použít tooregister alternativní UPN v AD?
* Jsou identifikátory uživatele hello konzistentní pro reprezentaci cloudu? 
* Má organizace hello aplikací, které vyžadují integraci s cloudovými službami?
* Hello organizace mají několik domén a použije všechny standardní nebo federovaného ověřování?

## <a name="evaluate-applications-that-run-in-your-environment"></a>Hodnocení aplikace, které běží ve vašem prostředí
Teď, když máte představu o místní a cloudové infrastruktury, musíte tooevaluate hello aplikace, které běží v těchto prostředích. Toto testování je důležité toodefine hello toointegrate technické požadavky na systém správy identit tyto aplikace toohello cloudu. Ujistěte se, zda text hello tooanswer následující otázky:

* Bydlišti bude naše aplikace
* Budou uživatelé získávat přístup k místním aplikacím?  V cloudu hello? Nebo obojí?
* Existují plány tootake hello existující úlohy aplikací a přesuňte je toohello cloudu?
* Existují plány toodevelop nové aplikace, které se nacházejí místně nebo v hello cloudu, který bude používat cloudové ověřování?

## <a name="evaluate-user-requirements"></a>Vyhodnocení požadavků uživatele
Máte také tooevaluate hello uživatelské požadavky. Toto testování je důležité toodefine hello kroky, které budou potřebovat pro Startovní a jak jejich přechod toohello cloudu, které uživatelé. Ujistěte se, zda text hello tooanswer následující otázky:

* Budou uživatelé získávat přístup k aplikacím v místě?
* Budou uživatelé získávat přístup k aplikacím v cloudu hello?
* Jak se uživatelé obvykle přihlášení tootheir místní prostředí?
* Jak budou uživatelé přihlášení toohello cloudové?

> [!NOTE]
> Ujistěte se, že tootake poznámky u každé odpovědi a pochopit hello důvody, které hello odpovědí. [Stanovení požadavků na reakce na incidenty](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md) budou přenášeny po hello dostupných možností a profesionálové/nevýhody jednotlivých možností.  Po zodpovězení těchto otázek, které vyberete která možnost nejlépe vyhovuje vaší firmě potřebuje.
> 
> 

## <a name="next-steps"></a>Další kroky
[Určení požadavků na synchronizaci adresáře](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md)

## <a name="see-also"></a>Viz také
[Přehled aspektů návrhu](active-directory-hybrid-identity-design-considerations-overview.md)

