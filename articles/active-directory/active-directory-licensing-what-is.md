---
title: "Uživatelé Azure Active Directory aaaLicense v hello portál Azure classic | Microsoft Docs"
description: "Popis licencování služby Microsoft Azure Active Directory, jak to funguje, jak spustit tooget a osvědčených postupů, včetně služeb Office 365, Microsoft Intune a Azure Active Directory Premium a Basic edice"
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
ms.openlocfilehash: 4d5f244cbee2ae37a30976f70b5d4f21c3516d90
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-microsoft-azure-active-directory-licensing-in-hello-azure-classic-portal"></a>Co je licencování služby Microsoft Azure Active Directory v portálu Azure classic hello?

> [!div class="op_single_selector"]
> * [Získají pokyny k portálu Azure](active-directory-licensing-get-started-azure-portal.md)
> * [Azure classic portálu informace](active-directory-licensing-what-is.md)
>
>

Azure Active Directory (Azure AD) je společnosti Microsoft Identity jako služby (IDaaS) řešení a platforma. Azure AD je k dispozici v několika verzích funkční a technické od Azure AD Free, která je k dispozici žádné službou Microsoft, jako je například Office 365, Dynamics, Microsoft Intune a Azure (Azure AD negeneruje žádné poplatky za využívání v tomto režimu), tooAzure AD placené verze například Enterprise Mobility Suite (EMS), Azure AD Premium a Basic, jakož i Azure Multi-Factor Authentication (MFA). Podobně jako mnoho dalších služeb Microsoft online services nejvíce Azure AD placené verze se doručují prostřednictvím uživatelská oprávnění jsou v Office 365, Microsoft Intune a Azure AD. V těchto případech je reprezentována hello služby nákupu s jeden nebo více odběrů a každé předplatné zahrnuje několik před nákupem licencí ve vašem klientovi. Uživatelská oprávnění se dosahuje prostřednictvím přiřazení licence, vytváření propojení mezi hello uživatele a hello produktu, povolení hello součásti služby pro uživatele hello a využívání mezi hello předplacený licence.

> [!IMPORTANT]
> Společnost Microsoft doporučuje, která můžete spravovat Azure AD pomocí hello [centra pro správu Azure AD](https://aad.portal.azure.com) v hello hello portál Azure místo použití portálu Azure classic, kterou se odkazuje v tomto článku. Jak tooassign rolí správce ve službě Azure AD Dobrý den, správce center, najdete v části [licence vy a vaši uživatelé ve službě Azure AD](active-directory-licensing-get-started-azure-portal.md).

[Vyzkoušejte Azure AD premium.](https://portal.office.com/Signup/Signup.aspx?OfferId=01824d11-5ad8-447f-8523-666b0848b381&ali=1#0)

> [!NOTE]
> Portálu pro správu Azure AD je součástí hello portál Azure classic. Při používání služby Azure AD nevyžaduje žádný Azure nákup, přístup k tomuto portálu vyžaduje aktivní předplatné Azure nebo [zkušební předplatné](https://azure.microsoft.com/pricing/free-trial/).
>
>

Komplexní přehled o možnosti služby Azure AD, najdete v části [co je Azure AD](active-directory-whatis.md).
[Další informace o úrovních služby Azure AD](https://azure.microsoft.com/support/legal/sla/)

> [!NOTE]
> Předplatná Azure platím průběžně se liší: během také reprezentované ve vašem adresáři, tato předplatná povolit vytváření prostředků Azure a jejich namapování tooyour platby. V tomto případě jsou žádné počty licencí, které jsou přidružené k předplatnému hello. Přidružení uživatelů s předplatným hello předplatné toomanaging hello uživatelům přístup k prostředkům, se dosáhne je udělení oprávnění toooperate v předplatném toohello namapované prostředků Azure.
>
>

## <a name="how-does-azure-ad-licensing-work"></a>Jak funguje licencování Azure AD
Na základě licencí (nárok na základě) Azure AD služby pracovní podle aktivaci předplatného v klientovi directory nebo služby Azure AD. Jakmile hello předplatné je aktivní možnosti služby hello můžete spravují správci nástroje directory nebo služby a používá licencovaní uživatelé.

Při nákupu nebo aktivaci Enterprise Mobility Suite, Azure AD Premium nebo Azure AD Basic, adresáře je aktualizováno hello předplatného, včetně doby platnosti a předplacený licence. Informace o vašem předplatném, včetně stavu, následující události životního cyklu a hello počet licencí, přiřazené nebo k dispozici je k dispozici prostřednictvím portálu Azure classic kartě hello licence pro konkrétní adresář hello hello. To je také toobest místní toomanage vaše přiřazení licence.

Každé předplatné se skládá z jedné nebo více plánů služeb, každý mapování hello zahrnuté úroveň funkčnosti typu hello služby; například Azure AD, Azure MFA, Microsoft Intune, Exchange Online nebo SharePoint Online. Správa licencí Azure AD nevyžaduje správy úrovně služeb plánu. To se liší od Office 365, která závisí na službách tooincluded toomanage přístup k této pokročilou konfiguraci režimu. Azure AD využívá v konfiguraci služby, funkce tooenable a spravovat jednotlivé oprávnění.

Obecně platí informace o předplatném Azure AD je spravovat prostřednictvím portálu Azure classic, kartě hello licence pro konkrétní adresář hello hello. Předplatná Azure AD, s výjimkou hello Azure AD Premium, se nezobrazí na portálu Office hello.

> [!IMPORTANT]
> Azure AD Premium a Basic, a také odběry Enterprise Mobility Suite, jsou uzavřeného tootheir zřízený adresáře nebo klienta. Odběry nelze rozdělit mezi adresáře nebo použít tooentitle uživatelů v jiných adresářích. Přesun předplatného mezi adresáře je možné, ale vyžaduje odesláním lístku podpory nebo zrušení a znovu zakoupit v případě hello přímých nákupů.
>
> Při nákupu Azure AD nebo Enterprise Mobility Suite prostřednictvím multilicenčních Aktivace předplatného se stane automaticky po hello smlouvu zahrnuje jiných služeb Microsoft Online, například Office 365.
>
>

Placené funkce Azure AD rozpětí hello spektra hello adresáře. Příklady obsahují:

* Tooapplications přiřazení na základě skupiny, která je povolena v rámci hello konkrétní aplikaci, kterou spravujete.
* Pokročilé a možnosti správy samoobslužné služby skupiny jsou k dispozici v části Konfigurace directory hello nebo v určité skupině hello.
* Sestavy zabezpečení Premium jsou na kartě hello generování sestav
* Zjišťování cloudové aplikace se zobrazí v hello portál Azure pod identitou.

### <a name="assigning-licenses"></a>Přiřazování licencí
Při získávání předplatné je, že všechny budete potřebovat tooconfigure placené funkce, pomocí služby Azure AD placené funkce vyžaduje distribuci správné jednotlivce toohello licence. Obecně platí každý uživatel, který by měl mít tooor přístup, který je spravován prostřednictvím Azure AD placené funkce musí být přiřazena licence. Přiřazení licence je mapování mezi uživateli a zakoupené služby, například Azure AD Premium, Basic nebo Enterprise Mobility Suite.

Správa, které uživatelé v adresáři by měl mít licenci je jednoduché. Lze provést přiřazením tooa skupiny toocreate pravidla přiřazení prostřednictvím portálu pro správu Azure AD hello nebo přiřazování licencí přímo toohello správné jednotlivce prostřednictvím portálu, prostředí PowerShell nebo rozhraní API. Při přiřazování licencí tooa skupiny, všechny členy skupiny přiřazenou licenci. Pokud přidat nebo odebrat ze skupiny hello uživatelé budou přiřazené nebo odebrané hello příslušnou licenci. Přiřazení skupiny můžete využít tooyou k dispozici žádné skupiny správy a je konzistentní s tooapplications přiřazování na základě skupiny. Tento přístup může nastavit pravidla tak, aby všichni uživatelé v adresáři se automaticky přiřazují, zajistěte, aby všichni s názvem příslušné úlohy hello licence nebo i delegovat hello rozhodnutí tooother správci v organizaci hello.

Pomocí přiřazení na základě skupiny licencí zdědí všechny uživatele chybí umístění využití umístění adresáře hello při přiřazení. Toto umístění může kdykoli změnit správce hello. V případech, kde hello automatizované přiřazení se nezdařilo z důvodu tooerror hello informace o uživateli v rámci, typ licence bude odrážet tomto stavu.

## <a name="getting-started-with-azure-ad-licensing"></a>Začínáme s licencováním Azure AD
Začínáme s Azure AD je snadné; vždy můžete vytvořit adresáře jako součást registrace do tooa zkušební verzi Azure volné. [Další informace o registraci organizace](sign-up-organization.md). Následující Hello můžete Ujistěte se, že je adresáře nejlépe souladu s jinými službami Microsoftu, může být využívání nebo plánování tooconsume a cílů při získávání hello služby.

Tady je několik osvědčených postupů:

* Pokud už používáte některé z organizace služeb společnosti Microsoft, už máte adresář služby Azure AD. V takovém případě by měly pokračovat toouse hello stejný adresář pro jiné služby, aby základní správu identit, včetně zřizování a hybridní jednotné přihlašování, můžete použít ve službách hello. Vaši uživatelé budou mít v jedné přihlášení rozhraní a budou využívat bohatší možnosti ve službách hello. Výsledkem je, pokud se rozhodnete toobuy na Azure AD placené služby pro pracovníky, doporučujeme použít stejný adresář toodo hello to.
* Pokud plánujete toouse Azure AD pro jinou sadu uživatelů (partnery, zákazníky a tak dále), nebo pokud jste chtěli služby tooevaluate Azure AD a chcete toodo v izolace vaše produkční služby, nebo pokud hledáte toosetup prostředí izolovaného prostoru pro vaše služby doporučujeme, abyste nejdřív vytvořili nový adresář prostřednictvím portálu Azure Azure classic hello. [Další informace o vytvoření nové adresář Azure AD v hello portál Azure classic](active-directory-licensing-directory-independence.md). Hello nový adresář bude vytvořen s vaším účtem, jako externí uživatel s oprávněními globálního správce. Při přihlášení toohello Azure classic portálu s tímto účtem je bude možné toosee tento adresář a přístup všechny úlohy pro správu adresáře. Doporučujeme vám vytvořit místní účet s náležitými oprávněními toomanage jiných služeb společnosti Microsoft, (ty není k dispozici prostřednictvím hello portál Azure classic). [Další informace o vytváření uživatelských účtů ve službě Azure AD](active-directory-create-users.md).

> [!NOTE]
> Azure AD podporuje "externí uživatele,", které jsou uživatelské účty v instanci Azure AD, které byly vytvořené pomocí Microsoft účtu (MSA) nebo Azure AD identity z jiného adresáře. Jsme jsou zaneprázdněn rozšíření tato funkce do celé organizace služby společnosti Microsoft, nyní tyto účty nejsou podporovány v některé z prostředí hello služeb; například portálu pro správu hello Office 365 aktuálně nepodporuje tyto uživatele. V důsledku toho externích uživatelů s účty Microsoft nebude portálu pro správu může tooaccess hello Office 365 vůbec, zatímco externích uživatelů z dalších adresářů Azure AD budou ignorovány. V druhém případě hello, pouze hello uživatel místní účet, hello Azure AD nebo Office 365 adresáře, které bylo původně vytvořeno hello uživatele, budou přístupné prostřednictvím těchto.
>
>

Jak je uvedeno, Azure AD má různé placené verze. Tyto verze mají některé malé rozdíly v jejich nákup dostupnosti:

| Produkt | EA NEBO VL | Otevřenost | CSP | Práv na používání MPN | Přímé nákupu | Zkušební verze |
| --- | --- | --- | --- | --- | --- | --- |
| Enterprise Mobility Suite |X |X |X |X | |X |
| Azure AD Premium |X |X |X | |X |X |
| Azure AD Basic |X |X |X |X |<br /> |<br /> |

### <a name="select-one-or-more-license-trials"></a>Vyberte jeden nebo více licencí zkušební verze
 Ve všech případech můžete aktivovat zkušební předplatné služby Azure AD Premium nebo Enterprise Mobility Suite výběrem konkrétní zkušební verze hello, které chcete na kartě hello licencí ve vašem adresáři. Buď zkušební verze obsahuje 30denní předplatné s 100 licencí.

![Plány zkušební licence Azure Active Directory](./media/active-directory-licensing-what-is/trial_plans.png)

![Enterprise Mobility Suite zkušební licenční plány](./media/active-directory-licensing-what-is/EMS_trial_plan.png)

![Aktivní zkušební licenční plány](./media/active-directory-licensing-what-is/active_license_trials.png)

### <a name="assign-licenses"></a>Přiřazení licencí
Jakmile hello předplatné je aktivní, měli přiřadit licence tooyourself a aktualizujte tooensure prohlížeče hello se zobrazuje všechny funkce. Hello dalším krokem je tooassign licencí toohello uživatele, kteří budou potřebovat tooaccess nebo být součástí placené funkce Azure AD. Jak jsme zmíněné v "Přiřazení licence," hello nejlepší způsob, jak toodo to je tooidentify hello skupiny představující hello potřeby cílovou skupinu a přiřaďte ho toohello licencí; Tímto způsobem uživatelé, kteří jsou přidat nebo odebrat ze skupiny hello přes životního cyklu přiřadí tooor odebrána z hello licence.

tooassign tooa skupinu licencí nebo jednotlivé uživatele, vyberte hello licenčního plánu chcete vytvořit tooassign a klikněte na tlačítko **přiřadit** na panelu příkazů hello.

![Aktivní zkušební licenční plány](./media/active-directory-licensing-what-is/assign_licenses.png)

Jakmile se v dialogovém okně přiřazení hello hello vybraného plánu, můžete vybrat uživatele a jejich přidáním toohello **přiřadit** sloupce v pravém hello. Můžete procházet seznam uživatelů hello nebo vyhledejte konkrétní osoby. pomocí vyhledávání hello skleněné na hello top napravo od hello uživatele mřížky. tooassign skupiny, vyberte z hello "Skupiny" **zobrazit** nabídce a potom klikněte na tlačítko Zkontrolovat hello na hello správné toorefresh hello přiřazení, které jsou zobrazeny.

![Přiřazení licencí toogroups](./media/active-directory-licensing-what-is/assign_licenses_to_groups.png)

Teď můžete hledat nebo stránky pomocí skupiny a přidat je toohello **přiřadit** sloupec v hello stejným způsobem. Můžete použít tyto tooassign kombinaci uživatelů a skupin v rámci jedné operace. toocomplete hello procesu přiřazení, klikněte na tlačítko zaškrtnutí hello v hello pravém dolním rohu stránky hello.

![Zpráva o průběhu přiřazení licencí](./media/active-directory-licensing-what-is/license_assignment_progress_message.png)

Když je skupina přiřazena, její členy dědit hello licencí do 30 minut, ale obvykle v rámci 1 – 2 minuty.

Přiřazení chyby může dojít během přiřazení licence Azure AD, ale jsou relativně vzácné. Potenciální chyby při přiřazení jsou omezeny na:

* Konflikt se přiřazení – v případě uživatel byl dříve přiřazenou licenci, která není kompatibilní s aktuální licencí hello. Přiřazení hello novou licenci v takovém případě bude vyžadovat odebrání hello předchozí.
* Překročil dostupné licence - při hello počet uživatelů v přiřazených skupinách překročí dostupné licence, bude odrážet stavu přiřazení uživatelů hello tooassign selhání kvůli toomissing licence.

### <a name="view-assigned-licenses"></a>Zobrazení přiřazené licence
Souhrnné zobrazení přiřazené licence, včetně životního cyklu událost odběru k dispozici, přiřazené a další jsou zobrazené na hello **licence** kartě.

![Zobrazení hello počet přiřazené licence](./media/active-directory-licensing-what-is/view_assigned_licenses.png)

Podrobný seznam přiřazené uživatele a skupiny, včetně stavu přiřazení a cestu (přímý nebo zděděné z jedné nebo více skupin) je dostupná, když přejdete do licenční plán.

![Zobrazení podrobností pro licenční plán přiřazené licence](./media/active-directory-licensing-what-is/assigned_licenses_detail.png)

Odebrání licencí je stejně jednoduché jako jejich přiřazení. Pokud je uživatel hello přímo přiřazena nebo přiřazené skupiny, můžete odebrat licence hello výběrem hello typ licence, výběr **odebrat**, přidání hello uživatele nebo skupiny toohello odebrat seznamu a potvrzení hello akce. Alternativně můžete otevřít typ licence, vyberte hello konkrétního uživatele nebo skupiny a klepněte na **odebrat** na panelu příkazů hello. tooend dědičnosti uživatelské licence ze skupiny, jednoduše odebrat hello uživatele ze skupiny hello.

### <a name="extending-trials"></a>Rozšíření zkušební verze
Zkušební verze rozšíření pro zákazníky, jsou k dispozici jako samoobslužné služby prostřednictvím portálu hello Office 365. Správce zákazníka můžete přejít toohello [portál Office](https://portal.office.com/#Billing) (přístup závisí na oprávněních pro portál Office hello) a vyberte zkušební období služby Azure AD Premium. Klikněte na tlačítko hello **zkušební verzi prodloužit** propojení a postupujte podle pokynů hello. Budete potřebovat tooenter platební karty, ale jeho nebude nic účtováno.

![Rozšíření a zkušební licencí na portálu Office hello](./media/active-directory-licensing-what-is/extend_license_trial.png)

Zákazníci také může požádat o prodloužení zkušební doby odesláním žádosti o podporu. Správce zákazníka můžete přejít na portál toohello Office 365 [stránku podpory](http://aka.ms/extendAADtrial) (přístup závisí na oprávnění pro stránku podpory Office hello). Na této stránce vyberte "Odběry a zkušební verze" v části funkce a "Otázek zkušební verze" v části příznakem. Nakonec zadejte informace na okolností hello

![Rozšíření a zkušební licence pomocí žádosti o podporu](./media/active-directory-licensing-what-is/alternate_office_aad_trial_extension.png)

## <a name="next-steps"></a>Další kroky
Nyní může být připravené tooconfigure a využívat některé funkce Azure AD Premium.

* [Samoobslužné resetování hesla](active-directory-manage-passwords.md)
* [Samoobslužná správa skupin](active-directory-accessmanagement-self-service-group-management.md)
* [Azure AD Connect stavu](active-directory-aadconnect-health.md)
* [Tooapplications přiřazení skupiny](active-directory-manage-groups.md)
* [Azure Multi-Factor Authentication](../multi-factor-authentication/multi-factor-authentication.md)
* [Nákup licencí Azure AD Premium](http://aka.ms/buyaadp)
