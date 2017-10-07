---
title: "aaaHow toomigrate vaše jednotlivé licencovaní uživatelé tooa skupiny ve službě Azure Active Directory | Microsoft Docs"
description: "Jak tooswitch z jednotlivých uživatelských licencí, na základě toogroup licencování pomocí služby Azure Active Directory"
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
ms.date: 06/05/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 25e5c760b7e632ba71edde10d937fe580aa6ed35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooadd-licensed-users-tooa-group-for-licensing-in-azure-active-directory"></a>Jak tooadd licencuje tooa skupiny uživatelů pro licencování v Azure Active Directory

Existující toousers licence, nasazení může mít v organizacích hello prostřednictvím "přímé přiřazení"; To znamená pomocí skriptů prostředí PowerShell nebo jiných nástrojů tooassign jednotlivých uživatelských licencí. Pokud chcete toostart pomocí na základě skupiny licencování toomanage licencí ve vaší organizaci, budete potřebovat migrace plánu tooseamlessly nahradit existující řešení na základě skupin licencí.

Hello nejdůležitějších věcí tookeep v paměti je, že vyhněte situaci, kdy licencování migrace na základě toogroup nebudou uživatelé dočasně došlo ke ztrátě jejich aktuálně přiřazené licence. Jakýkoli proces, který může mít za následek odebrání licencí by měla být vyhnout tooremove hello riziko ztráty přístupu tooservices a jejich data uživatele.

## <a name="recommended-migration-process"></a>Proces migrace doporučené

1. Máte existující automatizace (například prostředí PowerShell) Správa licencí přiřazení a odebrání pro uživatele. Necháte spuštěné, jako je.

2. Vytvořit novou skupinu licencování (nebo rozhodnout, které stávající skupiny toouse) a ujistěte se, že všechny požadované uživatelé jsou přidány jako členové.

3. Přiřazení licencí hello požadované skupiny toothose; vaším cílem mělo být tooreflect hello stejné licencování stavu vaší existující automatizace (například prostředí PowerShell) je použití toothose uživatele.

4. Ověřte, že licence byla použité tooall uživatelé v těchto skupinách. To lze provést kontrolou stavu hello zpracování pro každou skupinu a kontrolou protokoly auditu.

  - Můžete místo kontrola jednotlivých uživatelů podle jejich licenčních podrobnosti. Uvidíte, že mají hello stejné "přímo" přiřazené licence a "zděděná" ze skupin.

  - Můžete spustit skript prostředí PowerShell příliš[ověřte přidělování toousers licence](active-directory-licensing-group-advanced.md#use-powershell-to-see-who-has-inherited-and-direct-licenses).

  - Když hello stejné produktu licence se nepřiřadí uživatele toohello přímo a současně prostřednictvím skupiny, je pouze jednu licenci spotřebovávána hello uživatele. Proto nejsou žádné další licence požadované tooperform migrace.

5. Ověřte, že žádná přiřazení licence se nezdařilo kontrolou každou skupinu pro uživatele v chybovém stavu. Další informace najdete v tématu [identifikuje a řešení potíží s licencí pro skupinu](active-directory-licensing-group-problem-resolution-azure-portal.md).

6. Zvažte odebrání původní přímé přiřazení hello; může být vhodné toodo postupně, v "vlnami", toomonitor text hello výsledek na určitou podskupinu uživatelů.

  Může necháte hello původní přímé přiřazení na uživatele, ale když někteří uživatelé hello opustí jejich skupin licencí, které bude stále zachovávají původní licenci hello, která může být, aby, že chcete.

## <a name="an-example"></a>Příklad

Máme organizaci s 1 000 uživatelů. Všichni uživatelé vyžadují Enterprise Mobility + Security (EMS) licence. 200 uživatelů jsou v hello finančního oddělení a vyžadují Office 365 Enterprise E3 licence. Máme skript prostředí PowerShell, který je spuštěn místně přidávání a odebírání licence od uživatelů, dokud budou pocházet a přejděte. Chceme tooreplace hello skript na základě skupiny licencí, licence jsou spravovány automaticky službou Azure AD.

Tady je proces migrace jaké hello může vypadá jako:

1. Pomocí portálu Azure přiřadit hello EMS licence toohello hello **všichni uživatelé** skupiny ve službě Azure AD. Přiřazení licencí toohello hello E3 **finančního oddělení** skupinu, která obsahuje všechny uživatele, vyžaduje hello.

2. Pro každou skupinu potvrďte, že se dokončila přiřazení licence pro všechny uživatele. Okno přejděte toohello pro každou skupinu, vyberte **licence**a zkontrolujte stav zpracování hello hello horní části hello **licence** okno.

  - Podívejte se pro "nejnovější změny licencí byly použité tooall uživatelé" tooconfirm zpracování se dokončilo.

  - Hledejte v horní části o všechny uživatele, pro které licence pravděpodobně nebyla přiřazena úspěšně oznámení. Jsme dostatek licencí pro některé uživatele? Mají někteří uživatelé konfliktní licence SKU, které brání dědění skupiny licencí?

3. Zkontrolujte místo tooverify některé uživatele, které mají hello přímé a skupinu licencí použít. Okno přejděte toohello pro uživatele, vyberte **licence**a zkontrolujte stav hello licencí.

  - Toto je hello očekávaný stav uživatele v průběhu migrace:

      ![Stav očekávané uživatele](media/active-directory-licensing-group-migration-azure-portal/expected-user-state.png)

  Tím se potvrzuje, že tento hello uživatel má přímé a zděděné licencí. Vidíte, jak **EMS** a **E3** jsou přiřazeny.

  - Vyberte každý licence tooshow podrobnosti o službách hello povolena. Může to být použité toocheck, pokud hello přímé a skupinu licencí povolit hello přesně stejnou plány služby pro uživatele hello.

      ![Zkontrolujte plány služeb](media/active-directory-licensing-group-migration-azure-portal/check-service-plans.png)

4. Po potvrzení, že odpovídají direct a skupinu licencí, můžete začít odebrání přímé licence od uživatelů. Můžete si otestovat to jejich odebráním pro jednotlivé uživatele portálu hello a spusťte skripty toohave automatizace, který je v hromadné odebrána. Tady je příklad stejného uživatele s hello přímé licence odebrat prostřednictvím portálu hello hello. Všimněte si, že stav licence hello zůstává beze změny, ale už vidíme přímé přiřazení.

  ![odebrat přímý licencí](media/active-directory-licensing-group-migration-azure-portal/direct-licenses-removed.png)


## <a name="next-steps"></a>Další kroky

Další informace o scénáře pro správu licencí pomocí skupin, přečtěte si toolearn

* [Přiřazení licencí tooa skupiny ve službě Azure Active Directory](active-directory-licensing-group-assignment-azure-portal.md)
* [Co je na základě skupin licencování v Azure Active Directory?](active-directory-licensing-whatis-azure-portal.md)
* [Identifikace a řešení potíží s licencí pro skupinu v Azure Active Directory](active-directory-licensing-group-problem-resolution-azure-portal.md)
* [Azure Active Directory na základě skupin licencí další scénáře](active-directory-licensing-group-advanced.md)
