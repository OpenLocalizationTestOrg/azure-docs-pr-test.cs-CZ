---
title: "Začínáme s licencování v Azure Active Directory | Microsoft Docs"
description: "Jak Azure Active Directory funguje licencování, jak začít pracovat s osvědčenými postupy"
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
ms.openlocfilehash: 6fa7cbbc452861870136482aa320d268e78fe3d9
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
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

Podobně jako mnoho dalších služeb Microsoft online services nejvíce Azure AD placené verze se doručují prostřednictvím uživatelská oprávnění jsou v Office 365, Microsoft Intune a Azure AD. V těchto případech zakoupení služby je reprezentována jeden nebo více odběrů a každé předplatné zahrnuje některé prepurchased licencí ve vašem klientovi. Uživatelská oprávnění jsou dosahuje:

* Přiřazení licence. 
* Vytváření propojení mezi uživatelem a produktu.
* Povolení součásti služby pro uživatele.
* Jedna ze záloh licencí využívání.

[Vyzkoušejte Azure AD Premium.](https://portal.office.com/Signup/Signup.aspx?OfferId=01824d11-5ad8-447f-8523-666b0848b381&ali=1#0)

Komplexní přehled o možnosti služby Azure AD, najdete v části [co je Azure AD?](active-directory-whatis.md). Další informace najdete v tématu naše [stránku smlouvy o úrovni služeb](https://azure.microsoft.com/support/legal/sla/active-directory/v1_0/).

> [!NOTE]
> Povolit vytváření prostředků Azure a mapovat je na váš způsob platby předplatná Azure průběžnými platbami. Neexistují žádné počty licencí, které jsou přidružené k předplatnému. Když udělíte oprávnění uživatele k provozu na prostředky Azure, které jsou namapované na předplatné, jsou přidružený k předplatnému a můžete spravovat prostředky předplatného.

## <a name="how-does-azure-active-directory-licensing-work"></a>Jak funguje licencování v Azure Active Directory

Azure AD na základě licenční služby pracovní podle aktivaci předplatného v klientovi služby Azure AD. Po předplatné je aktivní, jsou možnosti služby spravují správci nástroje Azure AD a používá licencovaní uživatelé.

### <a name="manage-subscription-information"></a>Spravovat informace o předplatném

Při nákupu Enterprise Mobility + Security, Azure AD Premium nebo Azure AD Basic vašeho klienta je aktualizován předplatného, včetně jeho platnosti a předplacenou licence. Informace o vašem předplatném, včetně počtu licencí, přiřazené nebo k dispozici, je k dispozici prostřednictvím portálu Azure: v části **Azure Active Directory**, otevřete **licence** dlaždice pro konkrétní adresář. **Licence** okno je také nejlepší místo k spravovat vaše přiřazení licence.

Každé předplatné se skládá z jedné nebo více plánů služeb, jako je Azure AD, služba Multi-Factor Authentication, Intune, Exchange Online nebo SharePoint Online.  Správa licencí Azure AD nemá *není* vyžadují správy úrovně služeb plánu. Office 365 se liší, protože závisí na tomto režimu pokročilou konfiguraci můžete spravovat přístup k obsažených služeb. Azure AD je závislá na provozu konfiguraci funkce povolit a spravovat jednotlivé oprávnění.

> [!IMPORTANT]
> Azure AD Premium, Azure AD Basic a Enterprise Mobility + Security odběry jsou omezeny na jejich zřízené adresáře nebo klienta. Odběry nelze rozdělit mezi adresáře nebo lze získat oprávnění uživatelé ostatních adresářů. Přesouvání mezi adresáři odběr je možné, ale vyžaduje odeslat lístek podpory, nebo zrušení a repurchase pro přímé nákupy.
>
> Pokud Azure AD nebo Enterprise Mobility + Security je zakoupený prostřednictvím multilicenčních předplatné, a pokud smlouvu zahrnuje jiných služeb Microsoft Online (například Office 365), aktivace probíhá automaticky. 

### <a name="assign-licenses"></a>Přiřazení licencí

I když získání předplatného je vše, budete muset nakonfigurovat placené funkce, je nutné stále distribuovat licence pro placené funkce pro uživatele Azure AD. Každý uživatel, který mají mít přístup k nebo který je spravován prostřednictvím služby Azure AD placené funkce musí být přiřazena licence. Přiřazení licence je mapování mezi uživateli a zakoupené služby, jako je například Azure AD Premium, Basic nebo Enterprise Mobility + Security.


Správa, které uživatelé v adresáři by měl mít licenci se dá udělat: 

* Přiřazování licencí do skupin v [portál Azure](active-directory-licensing-whatis-azure-portal.md).
* Přiřazení licencí přímo uživatelům prostřednictvím portálu, prostředí PowerShell nebo rozhraní API. 

Když přiřazujete do skupiny licencí, jsou všechny členy skupiny přiřazenou licenci. Je-li přidat nebo odebrat ze skupiny uživatelů, licence vhodnou přiřazený nebo odebrat. Přiřazení skupiny můžete využívat správu všechny skupiny, která je k dispozici a je konzistentní s přiřazením na základě skupiny aplikací.

Můžete použít [přiřazení na základě skupiny licencí](active-directory-licensing-whatis-azure-portal.md) pravidel například následující:
* Všichni uživatelé v adresáři automaticky získat licenci
* Všichni uživatelé s názvem příslušné úlohy využít licenci
* Můžete delegovat rozhodnutí jiné správce v organizaci (pomocí [samoobslužných skupin](active-directory-accessmanagement-self-service-group-management.md))

Podrobné informace o přiřazení licence na skupiny, včetně pokročilé scénáře a Office 365 licencování scénáře, najdete v části [přiřadit licence pro uživatele na základě členství ve skupině v Azure Active Directory](active-directory-licensing-group-assignment-azure-portal.md).

## <a name="get-started-with-azure-ad-licensing"></a>Začínáme s Azure AD licencování

Začínáme s Azure AD je snadné. Můžete kdykoli vytvořit adresáře jako součást registrujete [bezplatné zkušební verzi Azure](https://azure.microsoft.com/offers/ms-azr-0044p/).

Následující osvědčené postupy může pomoci zajistit, že váš klient je v souladu s jinými službami Microsoftu, které může spotřebovávat a cílů pro službu:

- Pokud už používáte některou z organizace služby společnosti Microsoft, že máte klient služby Azure AD. Je vhodné použít stejné klienta pro jiné služby, aby základní správu identit, včetně zřizování a hybridní jednotné přihlašování (SSO), lze použít v rámci služby. S jednotné přihlašování uživatelé využívat bohaté možnosti rámci služeb. Proto pokud se rozhodnete koupit Azure AD placené služby pro pracovníky, doporučujeme použít stejný klienta znovu.

- Doporučujeme, pokud máte v úmyslu použít nového klienta na portálu Azure:
  - Používání Azure AD pro jinou sadu uživatele (například zákazníky nebo partnery).
  - Vyhodnoťte služby Azure AD izolovaně od provozního služby.
  - Nastavení prostředí izolovaného prostoru pro vaše služby.

  Nový adresář se vytvořil s vaším účtem jako externí uživatel s oprávněními globálního správce. Při přihlášení k portálu Azure s tímto účtem, můžete zobrazit tohoto klienta a přístup všechny úkoly správy.

> [!NOTE]
> Azure AD podporuje "hosta uživatelé", které jsou uživatelské účty v klient služby Azure AD, které byly vytvořeny prostřednictvím účtu Microsoft nebo Azure AD identity z jiného klienta. Portálu pro správu Office 365 aktuálně nepodporuje tyto uživatele. Uživatele typu Host s účty Microsoft nejsou přístup k portálu pro správu Office 365, zatímco uživatele typu Host z jiných klientů Azure AD jsou ignorovány. V takovém případě pouze místní účet uživatele, ve službě Azure AD nebo klienta Office 365, kde uživatel původně vytvořil, je přístupný.

### <a name="select-one-or-more-license-trials"></a>Vyberte jeden nebo více licencí zkušební verze

Můžete aktivovat Azure AD Premium nebo Enterprise Mobility + zkušební předplatné zabezpečení v části **Azure Active Directory** &gt; **úvodní**.

![Vyberte a zkušební licence](media/active-directory-licensing-get-started-azure-portal/select-a-license-trial.png)

Nejsou k dispozici na zkušební licence **licence** okno.

### <a name="assign-licenses-to-users-and-groups"></a>Přiřazení licencí uživatelům a skupinám

Po předplatné je aktivní, byste měli přiřadit licenci k sami. Potom aktualizujte prohlížeč a ujistěte se, že se vám zobrazí všechny funkce. Dalším krokem je přiřadit licence uživatelům, kteří potřebují přístup k placené funkce Azure AD. Jak je popsáno v [přiřadit licence](#assign-licenses), jeden snadný způsob, jak přiřadit licence je identifikovat skupině představující požadované cílovou skupinu a přiřadit licence. Tímto způsobem jsou uživatelé, kteří jsou přidat nebo odebrat ze skupiny přes životního cyklu přiřadit nebo odebrat z licenci, v uvedeném pořadí.

> [!NOTE]
> Některé služby společnosti Microsoft nejsou k dispozici ve všech umístěních. Předtím, než je možné přiřadit licence pro uživatele, Správce musí určit **umístění využití** vlastnost pro uživatele. Můžete nastavit tuto vlastnost pod **uživatele** &gt; **profil** &gt; **nastavení** na portálu Azure. Při použití přiřazení skupiny licencí, zdědí všechny uživatele, jejichž umístění využití nezadáte umístění adresáře.

Chcete-li přiřadit licenci, v části **Azure Active Directory** &gt; **licence** &gt; **všechny produkty, které**, vyberte jeden nebo více produktů a pak vyberte **přiřadit** na panelu příkazů.

![Vyberte licence přiřadit](media/active-directory-licensing-get-started-azure-portal/select-license-to-assign.png)

Můžete použít **uživatelů a skupin** okno vybrat více uživatelů nebo skupin nebo zakázat plány služeb v rámci produktu. Pomocí vyhledávacího pole v horní části vyhledat názvy uživatelů a skupin.

![Vyberte uživatele nebo skupiny pro přiřazení licence](media/active-directory-licensing-get-started-azure-portal/select-user-for-license-assignment.png)

Když přiřazujete licenci do skupiny, může trvat nějakou dobu, než dědění licenci, podle toho, kolik uživatelů ve skupině všichni uživatelé. Stav zpracování můžete zkontrolovat na **skupiny** okno, v části **licence** dlaždici.

![Stav přiřazení licencí](media/active-directory-licensing-get-started-azure-portal/license-assignment-status.png)

Přiřazení chyby může dojít během přiřazení licence Azure AD, ale jsou relativně vzácné, při správě Azure AD a Enterprise Mobility + Security produkty. Potenciální chyby při přiřazení jsou omezeny na:
- Přiřazení konflikt: když uživatel byl dříve přiřazen licenci, která není kompatibilní s aktuální licencí. V takovém případě přiřazování novou licenci vyžaduje odeberete aktuální.
- Překročil dostupné licence: Pokud počet uživatelů v přiřazených skupinách překročí dostupné licence, stav přiřazení uživatele, odráží selhání přiřadit z důvodu chybějící licence.

#### <a name="azure-ad-b2b-collaboration-licensing"></a>Licencování Azure AD s B2B spolupráce

Spolupráce B2B můžete pozvat uživatele typu Host do vašeho klienta Azure AD k poskytování přístupu ke službám Azure AD a veškeré prostředky Azure dáte k dispozici.  

Není nijak zpoplatněn pozvat B2B uživatele a přiřadit je k aplikaci ve službě Azure AD. Až 10 aplikací na uživatele guest a 3 základních sestav jsou také zdarma pro uživatele spolupráce B2B. Pokud vaše uživatele guest má přiřazené v klientovi Azure AD jeho všechny odpovídající licence, že budete mít licenci v váš také.

Není to nutné, ale pokud chcete poskytnout přístup k placené funkce Azure AD, musí mít licenci tyto uživatele typu Host B2B s příslušným licence Azure AD. Klient služby pozváním s placenou licenci Azure AD můžete přiřadit spolupráce B2B uživatelská práva dalším uživatelům pět hosta pozvánku, abyste klienta. Scénáře a informace najdete v tématu [spolupráce B2B licencování pokyny](active-directory-b2b-licensing.md).

### <a name="view-assigned-licenses"></a>Zobrazení přiřazené licence

Souhrnné zobrazení přiřazené a k dispozici licence se zobrazí v části **Azure Active Directory** &gt; **licence** &gt; **všechny produkty**.

![Zobrazení souhrnu licencí](media/active-directory-licensing-get-started-azure-portal/view-license-summary.png)

Podrobný seznam přiřazené uživatelů a skupin je k dispozici, když vyberete určitý produkt. **Licencovaní uživatelé** seznam obsahuje všechny aktuálně spotřebě licenci a jestli byl přiřazen licence, které jsou přímo na uživatele nebo pokud je zděděno od skupiny uživatelů.

![Zobrazit podrobnosti o licenci](media/active-directory-licensing-get-started-azure-portal/view-license-detail.png)

Podobně **licenci skupiny** seznamu jsou uvedeny všechny skupiny, ke kterým byla přiřazena licence. Vyberte uživatele nebo skupiny otevřete **licence** okno, které znázorňuje všechny licence se přiřadila tento objekt.

### <a name="remove-a-license"></a>Odebrání licence

Chcete-li odebrat licenci, přejděte na uživatele nebo skupinu a otevřete **licence** dlaždici. Vyberte licenci a klikněte na tlačítko **odebrat**.

![Odebrání licence](media/active-directory-licensing-get-started-azure-portal/remove-license.png)

Licence zdědí uživatele ze skupiny nelze odebrat přímo. Místo toho odeberte uživatele ze skupiny, ze kterého se jsou dědění licence.

### <a name="extend-trials"></a>Rozšíření zkušební verze

Zkušební verze rozšíření pro zákazníky, jsou k dispozici jako samoobslužné registrace prostřednictvím portálu Office 365. Správce zákazníka můžete přejít na portálu Office (přístup závisí na oprávnění k portálu Office) a vyberte zkušební verze Azure AD Premium. Kliknutím **zkušební verzi prodloužit** odkaz zahájí proces rozšíření. Byla požadována platební karta, ale není účtován.

![Prodlužte zkušební verzi portálu Azure](media/active-directory-licensing-get-started-azure-portal/extend-trial-beginning.png)

## <a name="next-steps"></a>Další kroky

Další informace o pokročilé scénáře pro správu licencí prostřednictvím skupiny, najdete v článku [přiřazování licencí pro skupinu](active-directory-licensing-group-assignment-azure-portal.md).

Zde jsou informace o tom, jak konfigurovat a používat další funkce placené služby Azure AD:

* [Samoobslužné resetování hesla](active-directory-manage-passwords.md)
* [Samoobslužná správa skupin](active-directory-accessmanagement-self-service-group-management.md)
* [Azure AD Connect health](active-directory-aadconnect-health.md)
* [Azure Multi-Factor Authentication](../multi-factor-authentication/multi-factor-authentication.md)
* [Nákup licencí Azure AD Premium](http://aka.ms/buyaadp)
