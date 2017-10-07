---
title: "Uživatelé aaaLicense v Azure Active Directory | Microsoft Docs"
description: "Zjistěte, jak toolicense vy a vaši uživatelé v Azure Active Directory."
services: active-directory
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: f8b932bc-8b4f-42b5-a2d3-f2c076234a78
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: jeffgilb
custom: it-pro
ms.openlocfilehash: ae0bc030fa02b79d1dd01ca961b4e96e6b9c470d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="quickstart-license-users-in-azure-active-directory"></a>Rychlý úvod: Licence uživatelům v Azure Active Directory
Azure AD na základě licenční služby pracovní aktivací předplatné služby Azure Active Directory (Azure AD) ve vašem klientu Azure. Po hello předplatné je aktivní, jsou možnosti služby spravují správci nástroje Azure AD a používá licencovaní uživatelé. Pokud si koupíte Enterprise Mobility + Security, Azure AD Premium nebo Azure AD Basic, vašeho klienta je aktualizována hello předplatného, včetně doby platnosti a předplacený licence. Informace o vašem předplatném, včetně hello počet licencí, přiřazené nebo k dispozici, je k dispozici prostřednictvím hello Azure portálu pod **Azure Active Directory** podle otevírání hello **licence** dlaždici. Hello **licence** okno je také hello nejlepší místo toomanage vaše přiřazení licence.

I když získání předplatného je, že všechny budete potřebovat tooconfigure placené funkce, musí stále přiřadit uživatelské licence pro placené služby Azure AD placené funkce. Každý uživatel, který mají mít přístup k nebo který je spravován prostřednictvím Azure AD placené funkce musí být přiřazena licence. Přiřazení licence je mapování mezi uživateli a zakoupené služby, jako je například Azure AD Premium, Basic nebo Enterprise Mobility + Security.

Můžete použít [přiřazení na základě skupiny licencí](active-directory-licensing-whatis-azure-portal.md) tooset si například hello následující pravidla:
* Všichni uživatelé v adresáři automaticky získat licenci
* Všichni uživatelé s názvem příslušné úlohy hello využít licenci
* Můžete delegovat hello rozhodnutí tooother správci v organizaci hello (pomocí [samoobslužných skupin](active-directory-accessmanagement-self-service-group-management.md))

> [!TIP]
> Podrobnou diskuzi o toogroups přiřazení licence, včetně pokročilé scénáře a Office 365 licencování scénáře, najdete v části [přiřazení licence toousers podle členství ve skupině v Azure Active Directory](active-directory-licensing-group-assignment-azure-portal.md).

## <a name="assign-licenses-toousers-and-groups"></a>Přiřazení toousers licencí a skupin
Pomocí aktivní předplatné, musí nejprve přiřadit licence tooyourself a aktualizovat vaše tooensure prohlížeče, který uvidíte všechny hello očekává funkce obsažené ve vašem předplatném. dalším krokem Hello je tooassign licence toohello uživatele, kteří potřebují funkce Azure AD toopaid přístup. Licence snadno tooassign je tooassign licence toogroups uživatelů místo tooindividuals. Když přiřadíte tooa skupinu licencí, jsou všechny členy skupiny přiřazenou licenci. Je-li přidat nebo odebrat ze skupiny hello uživatelů, licence vhodnou hello automaticky přiřadit nebo odebrat. 

> [!NOTE]
> Některé služby společnosti Microsoft nejsou k dispozici ve všech umístěních. Před tooa uživatele lze přiřadit licenci, hello správce musí určit hello **umístění využití** vlastnost pro uživatele hello. Můžete nastavit tuto vlastnost pod **uživatele** &gt; **profil** &gt; **nastavení** v hello portálu Azure. Při použití přiřazení skupiny licencí, zdědí všechny uživatele, jejichž umístění využití nezadáte hello umístění adresáře hello.

tooassign licenci, v části **Azure Active Directory** &gt; **licence** &gt; **všechny produkty**, vyberte jeden nebo více produktů a pak vyberte **Přiřadit** na panelu příkazů hello.

![Vyberte tooassign licencí](media/license-users-groups/select-license-to-assign.png)

Můžete použít hello **uživatelů a skupin** okno toochoose víc uživatelů nebo skupin nebo služby toodisable plány v produktu hello. Pomocí vyhledávacího pole hello na nejvyšší toosearch pro názvy uživatelů a skupin.

![Vyberte uživatele nebo skupiny pro přiřazení licence](media/license-users-groups/select-user-for-license-assignment.png)

Když přiřadíte tooa skupinu licencí, může trvat nějakou dobu, než všichni uživatelé dědění hello licencí v závislosti na velikosti hello hello skupiny. Můžete zkontrolovat stav zpracování hello hello **skupiny** okno, v části hello **licence** dlaždici.

![Stav přiřazení licencí](media/license-users-groups/license-assignment-status.png)

Přiřazení chyby může dojít během přiřazení licence Azure AD, ale jsou relativně vzácné, při správě Azure AD a Enterprise Mobility + Security produkty. Potenciální chyby při přiřazení jsou omezeny na:
- Přiřazení konflikt: když uživatel byl dříve přiřazen licenci, která není kompatibilní s aktuální licencí hello. V takovém případě přiřazování novou licenci hello vyžaduje odebrání hello stávající.
- Překročil dostupné licence: Pokud hello počet uživatelů v přiřazených skupinách překročí dostupné licence hello, stav přiřazení uživatele, odráží tooassign selhání kvůli toomissing licence.

### <a name="azure-ad-b2b-collaboration-licensing"></a>Licencování Azure AD s B2B spolupráce

Spolupráce B2B můžete uživatele typu Host tooinvite do služeb tooAzure AD přístup tooprovide klienta Azure AD a veškeré prostředky Azure dáte k dispozici.  

Není nijak zpoplatněn pozvat B2B uživatele a jejich přiřazení tooan aplikace ve službě Azure AD. Až too10 aplikace každému hostovi uživatele a 3 základních sestav jsou také zdarma pro uživatele spolupráce B2B. Pokud vaše uživatele guest má přiřazené v klientovi Azure AD hello partnera všechny odpovídající licence, že budete mít licenci v váš také.

Není to nutné, ale pokud chcete, aby funkce toopaid Azure AD tooprovide přístupu, musí mít licenci tyto uživatele typu Host B2B s příslušným licence Azure AD. Klient služby pozváním s placenou licenci Azure AD můžete přiřadit uživatelská práva spolupráce B2B pozvat uživatele typu Host tooan další pět toohello klienta. Scénáře a informace najdete v tématu [spolupráce B2B licencování pokyny](active-directory-b2b-licensing.md).

## <a name="view-assigned-licenses"></a>Zobrazení přiřazené licence

Souhrnné zobrazení přiřazené a k dispozici licence se zobrazí v části **Azure Active Directory** &gt; **licence** &gt; **všechny produkty**.

![Zobrazení souhrnu licencí](media/license-users-groups/view-license-summary.png)

Podrobný seznam přiřazené uživatelů a skupin je k dispozici, když vyberete určitý produkt. Hello **licencovaní uživatelé** seznam obsahuje všechny aktuálně spotřebě licenci a jestli hello licencí byl přímo přiřazen toohello uživatele nebo pokud je zděděno od skupiny uživatelů.

![Zobrazit podrobnosti o licenci](media/license-users-groups/view-license-detail.png)

Podobně hello **licenci skupiny** seznamu jsou uvedeny všechny skupiny toowhich licence byla přiřazena. Vyberte uživatele nebo skupinu text hello tooopen **licence** okno, které znázorňuje všechny licence přiřadit toothat objekt.

## <a name="remove-a-license"></a>Odebrání licence

tooremove licenci, přejděte toohello uživatele nebo skupinu a otevřete hello **licence** dlaždici. Vyberte hello licenci a klikněte na tlačítko **odebrat**.

![Odebrání licence](media/license-users-groups/remove-license.png)

Licence zdědí hello uživatele ze skupiny nelze odebrat přímo. Místo toho odeberte hello uživatele ze skupiny hello, ze kterého se jsou dědění hello licence.


## <a name="next-steps"></a>Další kroky
V tento rychlý start když jste se naučili jak tooassign licence toousers a skupiny v adresáři služby Azure AD. 

Můžete použít hello následujících přiřazením licencí předplatného tooconfigure odkaz ve službě Azure AD z hello portálu Azure.

> [!div class="nextstepaction"]
> [Přiřazení licencí Azure AD](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/LicensesMenuBlade/Overview) 