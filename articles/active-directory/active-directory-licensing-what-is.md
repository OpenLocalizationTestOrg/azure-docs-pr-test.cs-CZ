---
title: "Licence uživatelů Azure Active Directory na portálu Azure classic | Microsoft Docs"
description: "Popis služby Microsoft Azure Active Directory licencování, jak to funguje, jak začít pracovat a osvědčených postupů, včetně služeb Office 365, Microsoft Intune a Azure Active Directory Premium a Basic edice"
services: active-directory
keywords: "Licencování Azure AD"
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: d769e8c6-7581-43f5-a3b4-de4b1dca2344
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/24/2017
ms.author: curtand
ms.custom: oldportal;it-pro;H1Hack27Feb2017
ms.openlocfilehash: 9da5bb6987a9eb3398abe0d6066f1945620df9a4
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="what-is-microsoft-azure-active-directory-licensing-in-the-azure-classic-portal"></a>Co je licencování služby Microsoft Azure Active Directory na portálu Azure classic?

> [!div class="op_single_selector"]
> * [Získají pokyny k portálu Azure](active-directory-licensing-get-started-azure-portal.md)
> * [Azure classic portálu informace](active-directory-licensing-what-is.md)
>
>

Azure Active Directory (Azure AD) je společnosti Microsoft Identity jako služby (IDaaS) řešení a platforma. Azure AD je k dispozici v několika verzích funkční a technické od Azure AD Free, která je k dispozici žádné službou Microsoft, jako je například Office 365, Dynamics, Microsoft Intune a Azure (Azure AD negeneruje žádné poplatky za využívání v tomto režimu), do Azure AD placené verze například Enterprise Mobility Suite (EMS), Azure AD Premium a Basic, jakož i Azure Multi-Factor Authentication (MFA). Podobně jako mnoho dalších služeb Microsoft online services nejvíce Azure AD placené verze se doručují prostřednictvím uživatelská oprávnění jsou v Office 365, Microsoft Intune a Azure AD. V těchto případech je reprezentována zakoupení služby s jeden nebo více odběrů a každé předplatné zahrnuje několik před nákupem licencí ve vašem klientovi. Uživatelská oprávnění se dosahuje prostřednictvím přiřazení licence, vytváření propojení mezi uživatelem a produktu, povolení součásti služby pro uživatele a využívání jedna ze záloh licencí.

> [!IMPORTANT]
> Společnost Microsoft doporučuje při správě služby Azure AD používat [centrum pro správu Azure AD](https://aad.portal.azure.com) na webu Azure Portal namísto používání portálu Azure Classic, na který odkazuje tento článek. Pro přiřazení rolí správce v Centru správy služby Azure AD najdete v tématu [licence vy a vaši uživatelé ve službě Azure AD](active-directory-licensing-get-started-azure-portal.md).

[Vyzkoušejte Azure AD premium.](https://portal.office.com/Signup/Signup.aspx?OfferId=01824d11-5ad8-447f-8523-666b0848b381&ali=1#0)

> [!NOTE]
> Portálu pro správu Azure AD je součástí portálu Azure classic. Při používání služby Azure AD nevyžaduje žádný Azure nákup, přístup k tomuto portálu vyžaduje aktivní předplatné Azure nebo [zkušební předplatné](https://azure.microsoft.com/pricing/free-trial/).
>
>

Komplexní přehled o možnosti služby Azure AD, najdete v části [co je Azure AD](active-directory-whatis.md).
[Další informace o úrovních služby Azure AD](https://azure.microsoft.com/support/legal/sla/)

> [!NOTE]
> Předplatná Azure platím průběžně se liší: během také reprezentované ve vašem adresáři, tato předplatná povolit vytváření prostředků Azure a jejich namapování na váš způsob platby. V tomto případě jsou žádné počty licencí, které jsou přidružené k předplatnému. Přidružení uživatelů k předplatnému přístup uživatelů k správě prostředky předplatného, se dosáhne je udělení oprávnění k provozu na prostředky Azure, které jsou namapované na předplatné.
>
>

## <a name="how-does-azure-ad-licensing-work"></a>Jak funguje licencování Azure AD
Na základě licencí (nárok na základě) Azure AD služby pracovní podle aktivaci předplatného v klientovi directory nebo služby Azure AD. Jakmile bude předplatné aktivní možnosti služby můžete spravují správci nástroje directory nebo služby a používá licencovaní uživatelé.

Pokud jste je zakoupili nebo aktivovat Enterprise Mobility Suite, Azure AD Premium nebo Azure AD Basic, adresáře je aktualizován předplatného, včetně jeho platnosti a předplacenou licence. Informace o vašem předplatném, včetně stavu, následující události životního cyklu a počet licencí, přiřazené nebo k dispozici je k dispozici prostřednictvím portálu Azure classic na kartě licence pro konkrétní adresář. Toto je také k nejlepší místo k spravovat vaše přiřazení licence.

Každé předplatné se skládá z jedné nebo více plánů služeb každý mapování zahrnuté úroveň funkčnosti typu služby; například Azure AD, Azure MFA, Microsoft Intune, Exchange Online nebo SharePoint Online. Správa licencí Azure AD nevyžaduje správy úrovně služeb plánu. To se liší od Office 365, která závisí na tomto režimu pokročilou konfiguraci můžete spravovat přístup k obsažených služeb. Azure AD spoléhá na v konfiguraci služby k povolení funkcí a spravovat jednotlivé oprávnění.

Informace o předplatném Azure AD je obecně spravovat prostřednictvím portálu Azure classic, na kartě licence pro konkrétní adresář. Předplatná Azure AD, s výjimkou Azure AD Premium, se nezobrazí na portálu Office.

> [!IMPORTANT]
> Azure AD Premium a Basic, a také odběry Enterprise Mobility Suite, jsou omezen na jejich zřízené adresáře nebo klienta. Odběry nelze rozdělit mezi adresáře nebo lze získat oprávnění uživatelé ostatních adresářů. Přesun předplatného mezi adresáře je možné, ale vyžaduje odesláním lístku podpory nebo zrušení a znovu zakoupit v případě přímé nákupy.
>
> Při nákupu Azure AD nebo Enterprise Mobility Suite prostřednictvím multilicenčních Aktivace předplatného se stane automaticky po smlouvu zahrnuje jiných služeb Microsoft Online, například Office 365.
>
>

Placené Azure AD funkce span spektra adresáře. Příklady obsahují:

* Na základě skupiny přiřazování k aplikacím, které jsou povoleny v konkrétní aplikaci, kterou spravujete.
* Pokročilé a možnosti správy samoobslužné služby skupiny jsou k dispozici v části konfigurace adresáře nebo v určité skupině.
* Sestavy zabezpečení Premium jsou na kartě generování sestav
* Zjišťování cloudové aplikace se zobrazí na portálu Azure pod identitou.

### <a name="assigning-licenses"></a>Přiřazování licencí
Při získávání předplatné je vše, budete muset nakonfigurovat placené funkce, pomocí služby Azure AD placené funkce vyžaduje distribuci licence správné jednotlivcům. Obecně platí každý uživatel, který mají mít přístup k nebo který je spravován prostřednictvím služby Azure AD placené funkce musí být přiřazena licence. Přiřazení licence je mapování mezi uživateli a zakoupené služby, například Azure AD Premium, Basic nebo Enterprise Mobility Suite.

Správa, které uživatelé v adresáři by měl mít licenci je jednoduché. Lze provést přiřazení do skupiny k vytvoření pravidla přiřazení prostřednictvím portálu pro správu Azure AD nebo přiřazování licencí přímo na pravém jednotlivce prostřednictvím portálu, prostředí PowerShell nebo rozhraní API. Při přiřazování licencí pro skupinu, všechny členy skupiny přiřazenou licenci. Je-li přidat nebo odebrat ze skupiny uživatelů se přiřadí nebo odebrat příslušné licence. Přiřazení skupiny můžete využívat správu všechny skupiny, která je k dispozici a je konzistentní s přiřazením na základě skupiny aplikací. Tento přístup může nastavit pravidla tak, aby všichni uživatelé v adresáři se automaticky přiřazují, zajistěte, aby všichni s názvem příslušné úlohy licenci nebo i delegovat rozhodnutí jiné správce v organizaci.

Pomocí přiřazení na základě skupiny licencí zdědí všechny uživatele chybí umístění využití umístěním adresáře při přiřazení. Toto umístění může správce změnit kdykoli. V případech, kdy automatické přiřazení se nezdařila kvůli chybě informace o uživateli v rámci tohoto typu licencí se projeví tomto stavu.

## <a name="getting-started-with-azure-ad-licensing"></a>Začínáme s licencováním Azure AD
Začínáme s Azure AD je snadné; vždy můžete vytvořit adresáře jako součást registrací k bezplatné zkušební verzi Azure. [Další informace o registraci organizace](sign-up-organization.md). Následující vám může pomoct Ujistěte se, že je adresáře nejlépe souladu s jinými službami Microsoftu, může být využívání nebo plánujete používat a cílů při získávání službu.

Tady je několik osvědčených postupů:

* Pokud už používáte některé z organizace služeb společnosti Microsoft, už máte adresář služby Azure AD. V takovém případě můžete by měly být nadále používat stejný adresář pro jiné služby, aby základní správu identit, včetně zřizování a hybridní jednotné přihlašování, můžete použít rámci služeb. Vaši uživatelé budou mít v jedné přihlášení rozhraní a budou využívat bohatší možnosti rámci služeb. Výsledkem je pokud se rozhodnete koupit Azure AD placené služby pro pracovníky, doporučujeme to udělat pomocí stejného adresáře.
* Pokud máte v úmyslu použít pro jinou sadu uživatelů (partnery, zákazníky a tak dále), nebo pokud chcete vyhodnotit služby Azure AD a chcete to udělat v izolaci produkční služby Azure AD, nebo pokud chcete nastavit prostředí izolovaného prostoru pro y Naše služby, doporučujeme, abyste nejdřív vytvořili nový adresář prostřednictvím portálu Azure Azure classic. [Další informace o vytvoření nové adresář Azure AD na portálu Azure classic](active-directory-licensing-directory-independence.md). Nový adresář bude vytvořen s vaším účtem, jako externí uživatel s oprávněními globálního správce. Při přihlášení k portálu Azure classic s tímto účtem, nebudete moci zobrazit tento adresář a přístup všechny úlohy pro správu adresáře. Doporučujeme vytvořit místní účet s náležitými oprávněními ke správě jiných služeb společnosti Microsoft, (ty není k dispozici prostřednictvím portálu Azure classic). [Další informace o vytváření uživatelských účtů ve službě Azure AD](active-directory-create-users.md).

> [!NOTE]
> Azure AD podporuje "externí uživatele,", které jsou uživatelské účty v instanci Azure AD, které byly vytvořené pomocí Microsoft účtu (MSA) nebo Azure AD identity z jiného adresáře. Když jsme se zaneprázdněn rozšíření tato funkce do celé organizace služby společnosti Microsoft, nyní tyto účty nejsou podporovány v některé z uvedených služeb prostředí; například portálu pro správu Office 365 aktuálně nepodporuje tyto uživatele. Externí uživatelé s účty Microsoft v důsledku toho nebudou přístup k portálu pro správu Office 365, zatímco externích uživatelů z dalších adresářů Azure AD budou ignorovány. V takovém případě, pouze místní účet uživatele, Azure AD nebo Office 365 adresáře, kde uživatel byl původně vytvořen, by byly přístupné prostřednictvím těchto prostředí.
>
>

Jak je uvedeno, Azure AD má různé placené verze. Tyto verze mají některé malé rozdíly v jejich nákup dostupnosti:

| Produkt | EA NEBO VL | Otevřenost | CSP | Práv na používání MPN | Přímé nákupu | Zkušební verze |
| --- | --- | --- | --- | --- | --- | --- |
| Enterprise Mobility Suite |X |X |X |X | |X |
| Azure AD Premium |X |X |X | |X |X |
| Azure AD Basic |X |X |X |X |<br /> |<br /> |

### <a name="select-one-or-more-license-trials"></a>Vyberte jeden nebo více licencí zkušební verze
 Ve všech případech můžete aktivovat zkušební předplatné služby Azure AD Premium nebo Enterprise Mobility Suite výběrem konkrétní zkušební verze, které mají na kartě licencí ve vašem adresáři. Buď zkušební verze obsahuje 30denní předplatné s 100 licencí.

![Plány zkušební licence Azure Active Directory](./media/active-directory-licensing-what-is/trial_plans.png)

![Enterprise Mobility Suite zkušební licenční plány](./media/active-directory-licensing-what-is/EMS_trial_plan.png)

![Aktivní zkušební licenční plány](./media/active-directory-licensing-what-is/active_license_trials.png)

### <a name="assign-licenses"></a>Přiřazení licencí
Jakmile bude předplatné aktivní, doporučujeme přiřadit licenci k sami a aktualizujte prohlížeč a ujistěte se, že se zobrazuje všechny funkce. Dalším krokem je přiřadit licence pro uživatele, kteří se potřebují přístup k nebo být součástí placené funkce Azure AD. Jak jsme zmíněné v "Přiřazení licence", nejlepší způsob, jak to udělat je k identifikaci skupiny představující požadované cílovou skupinu a přiřadit licence; Tímto způsobem se uživatelé, kteří jsou přidat nebo odebrat ze skupiny přes životního cyklu přiřazené nebo odebrat z licence.

Chcete-li přiřadit licenci na skupinu nebo jednotlivé uživatele, vyberte licenční plán, kterou chcete přiřadit a klikněte na tlačítko **přiřadit** na panelu příkazů.

![Aktivní zkušební licenční plány](./media/active-directory-licensing-what-is/assign_licenses.png)

Jakmile se v dialogovém okně přiřazení vybraného plánu, můžete vybrat uživatele a přidá do **přiřadit** sloupec na pravé straně. Můžete procházet seznam uživatelů nebo vyhledejte konkrétní osoby. pomocí vyhledávání skleněné nahoře napravo od uživatele mřížky. Chcete-li přiřadit skupiny, vyberte "Skupiny" z **zobrazit** nabídky a pak klikněte na tlačítko kontroly na pravé straně aktualizace přiřazení, které se zobrazují.

![Přiřazování licencí do skupin](./media/active-directory-licensing-what-is/assign_licenses_to_groups.png)

Teď můžete hledat nebo stránky pomocí skupiny a přidat je do **přiřadit** sloupec stejným způsobem. Můžete tyto přiřadit kombinaci uživatelů a skupin v rámci jedné operace. Dokončete proces přiřazení, klikněte na tlačítko zaškrtnutí v pravém dolním rohu stránky.

![Zpráva o průběhu přiřazení licencí](./media/active-directory-licensing-what-is/license_assignment_progress_message.png)

Když je skupina přiřazena, její členy dědit licencí do 30 minut, ale obvykle v rámci 1 – 2 minuty.

Přiřazení chyby může dojít během přiřazení licence Azure AD, ale jsou relativně vzácné. Potenciální chyby při přiřazení jsou omezeny na:

* Konflikt se přiřazení – v případě uživatel byl dříve přiřazenou licenci, která není kompatibilní s aktuální licencí. Přiřazení nové licence v takovém případě bude vyžadovat odebrání předchozí.
* Překročil dostupné licence – když počet uživatelů v přiřazených skupinách překročit dostupných licencí, bude odrážet stav přiřazení uživatelů selhání přiřadit z důvodu chybějící licence.

### <a name="view-assigned-licenses"></a>Zobrazení přiřazené licence
Souhrnné zobrazení přiřazené licence, včetně životního cyklu událost odběru k dispozici, přiřazené a další jsou zobrazené na **licence** kartě.

![Zobrazení počtu přiřazené licence](./media/active-directory-licensing-what-is/view_assigned_licenses.png)

Podrobný seznam přiřazené uživatele a skupiny, včetně stavu přiřazení a cestu (přímý nebo zděděné z jedné nebo více skupin) je dostupná, když přejdete do licenční plán.

![Zobrazení podrobností pro licenční plán přiřazené licence](./media/active-directory-licensing-what-is/assigned_licenses_detail.png)

Odebrání licencí je stejně jednoduché jako jejich přiřazení. Pokud je uživatel přímo přiřazena nebo přiřazené skupiny, můžete odebrat licence výběrem typ licence, pak výběrem **odebrat**, přidání uživatele nebo skupinu, do seznamu odebrat a potvrzení akce. Alternativně můžete otevřít typ licence, vyberte konkrétní uživatel nebo skupina a klepněte na **odebrat** na panelu příkazů. Na konec dědičnosti uživatelské licence ze skupiny, jednoduše odeberte uživatele ze skupiny.

### <a name="extending-trials"></a>Rozšíření zkušební verze
Zkušební verze rozšíření pro zákazníky, jsou k dispozici jako samoobslužné služby prostřednictvím portálu Office 365. Správce zákazníka můžete přejít na [portál Office](https://portal.office.com/#Billing) (přístup závisí na oprávnění k portálu Office) a vyberte zkušební období služby Azure AD Premium. Klikněte **zkušební verzi prodloužit** propojit a postupujte podle pokynů. Budete muset zadat platební karty, ale jeho nebude nic účtováno.

![Rozšíření a zkušební licencí na portálu Office](./media/active-directory-licensing-what-is/extend_license_trial.png)

Zákazníci také může požádat o prodloužení zkušební doby odesláním žádosti o podporu. Správce zákazníka můžete přejít na portál Office 365 [stránku podpory](http://aka.ms/extendAADtrial) (přístup závisí na oprávnění pro podporu stránku Office). Na této stránce vyberte "Odběry a zkušební verze" v části funkce a "Otázek zkušební verze" v části příznakem. Nakonec zadejte informace o okolnostech

![Rozšíření a zkušební licence pomocí žádosti o podporu](./media/active-directory-licensing-what-is/alternate_office_aad_trial_extension.png)

## <a name="next-steps"></a>Další kroky
Může být nyní připraveni konfigurovat a používat některé funkce Azure AD Premium.

* [Samoobslužné resetování hesla](active-directory-manage-passwords.md)
* [Samoobslužná správa skupin](active-directory-accessmanagement-self-service-group-management.md)
* [Azure AD Connect stavu](active-directory-aadconnect-health.md)
* [Přiřazení skupiny aplikací](active-directory-manage-groups.md)
* [Azure Multi-Factor Authentication](../multi-factor-authentication/multi-factor-authentication.md)
* [Nákup licencí Azure AD Premium](http://aka.ms/buyaadp)
