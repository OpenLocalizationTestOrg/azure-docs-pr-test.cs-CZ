---
title: "aaaGet začít s licencování v Azure Active Directory | Microsoft Docs"
description: "Jak Azure Active Directory licencování funguje, jak tooget pracovat s osvědčenými postupy"
services: active-directory
keywords: "Licencování Azure AD"
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/26/2017
ms.author: curtand
ms.reviewer: piotrci
ms.custom: H1Hack27Feb2017;it-pro
ms.openlocfilehash: 268dab806b8b959790341d630a0355c6a43871d0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="license-yourself-and-your-users-in-azure-active-directory"></a>Licence vy a vaši uživatelé v Azure Active Directory

> [!div class="op_single_selector"]
> * [Pokyny k portálu Azure](active-directory-licensing-get-started-azure-portal.md)
> * [Získání Azure classic portálu informací o](active-directory-licensing-what-is.md)
>
>

Azure Active Directory (Azure AD) je identity jako služby (IDaaS) řešení a platforma společnosti Microsoft. Azure AD je k dispozici v edicích jinou službu:

* Azure Active Directory volná, která je k dispozici žádné službou Microsoft, jako je například Office 365, Dynamics, Microsoft Intune nebo Azure. Azure AD negeneruje žádné poplatky za využívání v tomto režimu.

* Azure AD placené edice, jako například:
  - Enterprise Mobility + Security 
  - Azure AD Premium (P1 a P2)
  - Azure AD Basic
  - Azure Multi-Factor Authentication

Podobně jako mnoho dalších služeb Microsoft online services nejvíce Azure AD placené verze se doručují prostřednictvím uživatelská oprávnění jsou v Office 365, Microsoft Intune a Azure AD. V těchto případech nákupu služby hello je reprezentována jeden nebo více odběrů a každé předplatné zahrnuje některé prepurchased licencí ve vašem klientovi. Uživatelská oprávnění jsou dosahuje:

* Přiřazení licence. 
* Vytváření propojení mezi hello uživatele a hello produktu.
* Povolení hello součásti služby pro uživatele hello.
* Jeden z hello využívání předplacený licence.

[Vyzkoušejte Azure AD Premium.](https://portal.office.com/Signup/Signup.aspx?OfferId=01824d11-5ad8-447f-8523-666b0848b381&ali=1#0)

Komplexní přehled o možnosti služby Azure AD, najdete v části [co je Azure AD?](active-directory-whatis.md). Další informace najdete v tématu naše [stránku smlouvy o úrovni služeb](https://azure.microsoft.com/support/legal/sla/active-directory/v1_0/).

> [!NOTE]
> Povolit vytváření prostředků Azure a namapovat je způsob platby tooyour předplatná Azure průběžnými platbami. Neexistují žádné počty licencí, které jsou přidružené k předplatnému hello. Když udělíte toohello předplatného je mapován toooperate oprávnění uživatele na prostředky Azure, jsou spojeny s předplatným hello a můžete spravovat prostředky předplatného.

## <a name="how-does-azure-active-directory-licensing-work"></a>Jak funguje licencování v Azure Active Directory

Azure AD na základě licenční služby pracovní podle aktivaci předplatného v klientovi služby Azure AD. Po hello předplatné je aktivní, jsou možnosti služby hello spravují správci nástroje Azure AD a používá licencovaní uživatelé.

### <a name="manage-subscription-information"></a>Spravovat informace o předplatném

Pokud si koupíte Enterprise Mobility + Security, Azure AD Premium nebo Azure AD Basic, vašeho klienta je aktualizována hello předplatného, včetně doby platnosti a předplacený licence. Informace o vašem předplatném, včetně hello počet licencí, přiřazené nebo k dispozici, je k dispozici prostřednictvím portálu Azure hello: v části **Azure Active Directory**, otevřete hello **licence** dlaždici hello konkrétní adresář. Hello **licence** okno je také hello nejlepší místo toomanage vaše přiřazení licence.

Každé předplatné se skládá z jedné nebo více plánů služeb, jako je Azure AD, služba Multi-Factor Authentication, Intune, Exchange Online nebo SharePoint Online.  Správa licencí Azure AD nemá *není* vyžadují správy úrovně služeb plánu. Office 365 se liší, protože závisí na této pokročilou konfiguraci režimu toomanage přístup tooincluded služby. Azure AD závisí na funkcích tooenable provozu konfigurace a správa jednotlivých oprávnění.

> [!IMPORTANT]
> Azure AD Premium, Azure AD Basic a Enterprise Mobility + Security odběry jsou uzavřeného tootheir zřízený adresáře nebo klienta. Odběry nelze rozdělit mezi adresáře nebo použít tooentitle uživatelů v jiných adresářích. Přesouvání mezi adresáři odběr je možné, ale vyžaduje odeslat lístek podpory, nebo zrušení a repurchase pro přímé nákupy.
>
> Pokud Azure AD nebo Enterprise Mobility + Security je zakoupený prostřednictvím multilicenčních předplatné, a pokud hello smlouvu zahrnuje jiných služeb Microsoft Online (například Office 365), aktivace probíhá automaticky. 

### <a name="assign-licenses"></a>Přiřazení licencí

I když získání předplatného je, že všechny budete potřebovat tooconfigure placené funkce, je nutné stále distribuovat licence pro Azure AD placené funkce toousers. Každý uživatel, který by měl mít tooor přístup, který je spravován prostřednictvím Azure AD placené funkce musí být přiřazena licence. Přiřazení licence je mapování mezi uživateli a zakoupené služby, jako je například Azure AD Premium, Basic nebo Enterprise Mobility + Security.


Správa, které uživatelé v adresáři by měl mít licenci se dá udělat: 

* Přiřazení licencí toogroups v hello [portál Azure](active-directory-licensing-whatis-azure-portal.md).
* Přiřazení licencí přímo toousers prostřednictvím portálu hello, prostředí PowerShell nebo rozhraní API. 

Když přiřazujete tooa skupiny licencí, jsou všechny členy skupiny přiřazenou licenci. Je-li přidat nebo odebrat ze skupiny hello uživatelů, licence vhodnou hello je přiřazovat a odebírat. Přiřazení skupiny můžete využít tooyou k dispozici žádné skupiny správy, a je konzistentní s tooapplications přiřazování na základě skupiny.

Můžete použít [přiřazení na základě skupiny licencí](active-directory-licensing-whatis-azure-portal.md) tooset si například hello následující pravidla:
* Všichni uživatelé v adresáři automaticky získat licenci
* Všichni uživatelé s názvem příslušné úlohy hello využít licenci
* Můžete delegovat hello rozhodnutí tooother správci v organizaci hello (pomocí [samoobslužných skupin](active-directory-accessmanagement-self-service-group-management.md))

Podrobnou diskuzi o toogroups přiřazení licence, včetně pokročilé scénáře a Office 365 licencování scénáře, najdete v části [přiřazení licence toousers podle členství ve skupině v Azure Active Directory](active-directory-licensing-group-assignment-azure-portal.md).

## <a name="get-started-with-azure-ad-licensing"></a>Začínáme s Azure AD licencování

Začínáme s Azure AD je snadné. Můžete kdykoli vytvořit adresáře jako součást registrujete [bezplatné zkušební verzi Azure](https://azure.microsoft.com/offers/ms-azr-0044p/).

Hello následující osvědčené postupy může pomoci zajistit, že váš klient je v souladu s jinými službami Microsoftu, které může spotřebovávat a cílů služby hello:

- Pokud už používáte některou z hello organizace služby společnosti Microsoft, že máte klient služby Azure AD. Je užitečné toouse hello stejné klienta pro jiné služby, aby základní správu identit, včetně zřizování a hybridní jednotné přihlašování (SSO), můžete použít ve službách hello. S jednotné přihlašování uživatelé využívat bohaté možnosti hello ve službách hello. Proto pokud se rozhodnete toobuy službou Azure AD placené pro pracovníky, doporučujeme vám použít hello stejné klienta znovu.

- Doporučujeme, pokud máte v úmyslu použít nového klienta v hello portálu Azure:
  - Používání Azure AD pro jinou sadu uživatele (například zákazníky nebo partnery).
  - Vyhodnoťte služby Azure AD izolovaně od provozního služby.
  - Nastavení prostředí izolovaného prostoru pro vaše služby.

  nový adresář Hello se vytvořil s vaším účtem jako externí uživatel s oprávněními globálního správce. Při přihlášení toohello portál Azure s tímto účtem, můžete zobrazit tohoto klienta a přístup všechny úkoly správy.

> [!NOTE]
> Azure AD podporuje "hosta uživatelé", které jsou uživatelské účty v klient služby Azure AD, které byly vytvořeny prostřednictvím účtu Microsoft nebo Azure AD identity z jiného klienta. portálu pro správu Hello Office 365 aktuálně nepodporuje tyto uživatele. Uživatele typu Host s účty Microsoft nejsou portálu pro správu může tooaccess hello Office 365, průběhu uživatele typu Host z jiných klientů Azure AD jsou ignorovány. V druhém případě hello pouze hello místní účet uživatele v hello Azure AD nebo klienta Office 365, které bylo původně vytvořeno hello uživatele, je přístupný.

### <a name="select-one-or-more-license-trials"></a>Vyberte jeden nebo více licencí zkušební verze

Můžete aktivovat Azure AD Premium nebo Enterprise Mobility + zkušební předplatné zabezpečení v části **Azure Active Directory** &gt; **úvodní**.

![Vyberte a zkušební licence](media/active-directory-licensing-get-started-azure-portal/select-a-license-trial.png)

Zkušební licence jsou k dispozici na hello **licence** okno.

### <a name="assign-licenses-toousers-and-groups"></a>Přiřazení toousers licencí a skupin

Po hello předplatné je aktivní, byste měli přiřadit licence tooyourself. Potom aktualizujte tooensure hello prohlížeče, že se vám zobrazí všechny funkce hello. dalším krokem Hello je tooassign licence toohello uživatele, kteří potřebují funkce Azure AD toopaid přístup. Jak je popsáno v [přiřadit licence](#assign-licenses), jeden snadno tooassign licence je skupina hello tooidentify představující hello potřeby cílovou skupinu a přiřadit licence tooit hello. Tímto způsobem jsou uživatelé, kteří jsou přidat nebo odebrat ze skupiny hello přes životního cyklu přiřadit nebo odebrat z hello licenci, v uvedeném pořadí.

> [!NOTE]
> Některé služby společnosti Microsoft nejsou k dispozici ve všech umístěních. Před tooa uživatele lze přiřadit licenci, hello správce musí určit hello **umístění využití** vlastnost pro uživatele hello. Můžete nastavit tuto vlastnost pod **uživatele** &gt; **profil** &gt; **nastavení** v hello portálu Azure. Při použití přiřazení skupiny licencí, zdědí všechny uživatele, jejichž umístění využití nezadáte hello umístění adresáře hello.

tooassign licenci, v části **Azure Active Directory** &gt; **licence** &gt; **všechny produkty**, vyberte jeden nebo více produktů a pak vyberte **Přiřadit** na panelu příkazů hello.

![Vyberte tooassign licencí](media/active-directory-licensing-get-started-azure-portal/select-license-to-assign.png)

Můžete použít hello **uživatelů a skupin** okno toochoose víc uživatelů nebo skupin nebo služby toodisable plány v produktu hello. Pomocí vyhledávacího pole hello na nejvyšší toosearch pro názvy uživatelů a skupin.

![Vyberte uživatele nebo skupiny pro přiřazení licence](media/active-directory-licensing-get-started-azure-portal/select-user-for-license-assignment.png)

Když přiřazujete tooa skupinu licencí, může trvat nějakou dobu, než hello licenci, v závislosti na hello počet uživatelů ve skupině hello dědí všechny uživatele. Můžete zkontrolovat stav zpracování hello hello **skupiny** okno, v části hello **licence** dlaždici.

![Stav přiřazení licencí](media/active-directory-licensing-get-started-azure-portal/license-assignment-status.png)

Přiřazení chyby může dojít během přiřazení licence Azure AD, ale jsou relativně vzácné, při správě Azure AD a Enterprise Mobility + Security produkty. Potenciální chyby při přiřazení jsou omezeny na:
- Přiřazení konflikt: když uživatel byl dříve přiřazen licenci, která není kompatibilní s aktuální licencí hello. V takovém případě přiřazování novou licenci hello vyžaduje odebrání hello stávající.
- Překročil dostupné licence: Pokud hello počet uživatelů v přiřazených skupinách překročí dostupné licence hello, stav přiřazení uživatele, odráží tooassign selhání kvůli toomissing licence.

#### <a name="azure-ad-b2b-collaboration-licensing"></a>Licencování Azure AD s B2B spolupráce

Spolupráce B2B můžete uživatele typu Host tooinvite do služeb tooAzure AD přístup tooprovide klienta Azure AD a veškeré prostředky Azure dáte k dispozici.  

Není nijak zpoplatněn pozvat B2B uživatele a jejich přiřazení tooan aplikace ve službě Azure AD. Až too10 aplikace každému hostovi uživatele a 3 základních sestav jsou také zdarma pro uživatele spolupráce B2B. Pokud vaše uživatele guest má přiřazené v klientovi Azure AD hello partnera všechny odpovídající licence, že budete mít licenci v váš také.

Není to nutné, ale pokud chcete, aby funkce toopaid Azure AD tooprovide přístupu, musí mít licenci tyto uživatele typu Host B2B s příslušným licence Azure AD. Klient služby pozváním s placenou licenci Azure AD můžete přiřadit uživatelská práva spolupráce B2B pozvat uživatele typu Host tooan další pět toohello klienta. Scénáře a informace najdete v tématu [spolupráce B2B licencování pokyny](active-directory-b2b-licensing.md).

### <a name="view-assigned-licenses"></a>Zobrazení přiřazené licence

Souhrnné zobrazení přiřazené a k dispozici licence se zobrazí v části **Azure Active Directory** &gt; **licence** &gt; **všechny produkty**.

![Zobrazení souhrnu licencí](media/active-directory-licensing-get-started-azure-portal/view-license-summary.png)

Podrobný seznam přiřazené uživatelů a skupin je k dispozici, když vyberete určitý produkt. Hello **licencovaní uživatelé** seznam obsahuje všechny aktuálně spotřebě licenci a jestli hello licencí byl přímo přiřazen toohello uživatele nebo pokud je zděděno od skupiny uživatelů.

![Zobrazit podrobnosti o licenci](media/active-directory-licensing-get-started-azure-portal/view-license-detail.png)

Podobně hello **licenci skupiny** seznamu jsou uvedeny všechny skupiny toowhich licence byla přiřazena. Vyberte uživatele nebo skupinu text hello tooopen **licence** okno, které znázorňuje všechny licence přiřadit toothat objekt.

### <a name="remove-a-license"></a>Odebrání licence

tooremove licenci, přejděte toohello uživatele nebo skupinu a otevřete hello **licence** dlaždici. Vyberte hello licenci a klikněte na tlačítko **odebrat**.

![Odebrání licence](media/active-directory-licensing-get-started-azure-portal/remove-license.png)

Licence zdědí hello uživatele ze skupiny nelze odebrat přímo. Místo toho odeberte hello uživatele ze skupiny hello, ze kterého se jsou dědění hello licence.

### <a name="extend-trials"></a>Rozšíření zkušební verze

Zkušební verze rozšíření pro zákazníky, jsou k dispozici jako samoobslužné registrace prostřednictvím portálu hello Office 365. Správce zákazníka můžete přejít na portál Office toohello (přístup závisí na oprávněních pro portál Office hello) a vyberte zkušební verze Azure AD Premium hello. Kliknutím na hello **zkušební verzi prodloužit** odkaz spustí proces rozšíření hello. Byla požadována platební karta, ale není účtován.

![Rozšíření zkušební verze v hello portálu Azure](media/active-directory-licensing-get-started-azure-portal/extend-trial-beginning.png)

## <a name="next-steps"></a>Další kroky

Další informace o toolearn pokročilé scénáře pro správu licencí pomocí skupin, přečtěte si článek hello [přiřazení licence tooa skupiny](active-directory-licensing-group-assignment-azure-portal.md).

Zde jsou informace o tom, tooconfigure a použít jiné službě Azure AD placené funkce:

* [Samoobslužné resetování hesla](active-directory-manage-passwords.md)
* [Samoobslužná správa skupin](active-directory-accessmanagement-self-service-group-management.md)
* [Azure AD Connect health](active-directory-aadconnect-health.md)
* [Azure Multi-Factor Authentication](../multi-factor-authentication/multi-factor-authentication.md)
* [Nákup licencí Azure AD Premium](http://aka.ms/buyaadp)
