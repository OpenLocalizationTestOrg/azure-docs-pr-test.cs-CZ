---
title: "aaaAssign licence tooa skupiny ve službě Azure Active Directory | Microsoft Docs"
description: "Jak tooassign licence toousers prostřednictvím Azure Active Directory skupiny licencí"
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
ms.openlocfilehash: 148fe1bdd6c7f477a00c1f76bd8fa7d29c7b1f2c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="assign-licenses-toousers-by-group-membership-in-azure-active-directory"></a>Přiřazení licencí toousers podle členství ve skupině v Azure Active Directory

Tento článek vás provede přiřazení produktu licence tooa skupiny uživatelů v Azure Active Directory (Azure AD) a potom ověření, že máte licenci správně.

V tomto příkladu hello klienta obsahuje skupiny zabezpečení s názvem **Personální oddělení**. Tato skupina zahrnuje všechny členové oddělení lidských zdrojů hello (přibližně 1 000 uživatelů). Chcete tooassign Office 365 Enterprise E3 licence toohello celý oddělení. Hello Yammer Enterprise služba, která je součástí produktu hello musí dočasně zakázána, dokud hello oddělení je připraven toostart jeho použití. Chcete taky toodeploy Enterprise Mobility + Security licence toohello stejnou skupinu uživatelů.

> [!NOTE]
> Některé služby společnosti Microsoft nejsou k dispozici ve všech umístěních. Před tooa uživatele lze přiřadit licenci, má správce hello toospecify hello využití umístění vlastnost hello uživatele.

> Pro přiřazení skupiny licencí zdědí všechny uživatele bez zadané umístění využití hello umístění adresáře hello. Pokud máte uživatele na více místech, doporučujeme vždy nastavit umístění využití v rámci vaší toku vytvoření uživatele ve službě Azure AD (např. přes AAD Connect konfigurace) –, který zajistí hello výsledek přiřazení licence je vždy správný a uživatelé neobdrží. služby v umístění, které nejsou povoleny.

## <a name="step-1-assign-hello-required-licenses"></a>Krok 1: Přiřazení licencí, které vyžaduje hello

1. Přihlaste se toohello [ **portál Azure** ](https://portal.azure.com) s účtem správce. toomanage licence, hello účtu musí být globální správce správce účtu roli nebo uživatele.

2. Vyberte **další služby** v hello levém navigačním podokně a pak vyberte **Azure Active Directory**. Můžete přidat toto okno tooFavorites nebo připnout toohello řídicí panel portálu.

3. Na hello **Azure Active Directory** vyberte **licence**. Otevře se okno, kde můžete zobrazit a spravovat všechny produkty provozovatelný v klientovi hello.

4. V části **všechny produkty**, vyberte Office 365 Enterprise E3 a Enterprise Mobility + Security výběrem hello názvy produktů. toostart hello přiřazení, vyberte **přiřadit** hello horní části okna hello.

   ![Všechny produkty, přiřadit licence](media/active-directory-licensing-group-assignment-azure-portal/all-products-assign.png)

5. Na hello **přiřazení licence** okně klikněte na tlačítko **uživatelů a skupin** tooopen hello **uživatelů a skupin** okno. Vyhledávání pro název skupiny hello *Personální oddělení*, vyberte skupinu hello, a potom mít jistotu tooconfirm kliknutím **vyberte** v hello dolní části okna hello.

   ![Vyberte skupinu.](media/active-directory-licensing-group-assignment-azure-portal/select-a-group.png)

6. Na hello **přiřazení licence** okně klikněte na tlačítko **přiřazení možností (volitelné)**, zobrazuje všechny plány služeb součástí hello dva produkty, které jsme vybrali dříve. Najít **Yammer Enterprise** a zapnout ho **vypnout** toodisable, který ze licenci produktu hello. Potvrďte kliknutím **OK** hello dolnímu okraji **přiřazení možností**.

   ![Přiřazení možností](media/active-directory-licensing-group-assignment-azure-portal/assignment-options.png)

7. přiřazení hello toocomplete, na hello **přiřazení licence** okně klikněte na tlačítko **přiřadit** v hello dolní části okna hello.

8. V pravém horním rohu hello, který ukazuje stav hello a výsledek procesu hello se zobrazí oznámení. Pokud hello přiřazení toohello skupiny nebylo možné dokončit (například z důvodu předchozích licencí ve skupině hello), klikněte na tlačítko Podrobnosti tooview oznámení hello hello selhání.

Nyní jsme jste zadali šablona licence pro skupinu Personální oddělení hello. Proces na pozadí ve službě Azure AD byl spuštěn tooprocess všechny stávající členy této skupiny. Tato počáteční operace může trvat delší dobu, v závislosti na aktuální velikost hello hello skupiny. V dalším kroku hello jsme budete popisují, jak má dokončení tohoto procesu hello tooverify a zjistit, jestli další pozornost požadované tooresolve problémy.

> [!NOTE]
> Můžete spustit hello stejné přiřazení z alternativního umístění: **uživatelů a skupin** ve službě Azure AD. Přejděte příliš**Azure Active Directory** > **uživatelů a skupin** > **všechny skupiny**. Vyhledejte hello skupiny, vyberte ho a pak použijte toohello **licence** kartě hello **přiřadit** tlačítko nad hello okno otevře okno Přiřazení licence hello.

## <a name="step-2-verify-that-hello-initial-assignment-has-finished"></a>Krok 2: Ověření, že byla dokončena počáteční přiřazení hello

1. Přejděte příliš**Azure Active Directory** > **uživatelů a skupin** > **všechny skupiny**. Vyhledejte hello **Personální oddělení** skupinu, která licencí bylo přiřazeno k.

2. Na hello **Personální oddělení** okno skupiny, vyberte **licence**. Díky tomu můžete rychle potvrďte Pokud licence je toousers plně přiřazen, a pokud nejsou žádné chyby, které je třeba toolook do. je k dispozici Hello následující informace:

   - Seznam licencí produktu, které jsou aktuálně přiřazen skupině toohello. Vyberte položka tooshow hello konkrétní služby, které byly povoleny a toomake změny.

   - Stav hello nejnovější licence změny provedené toohello skupiny (pokud jsou zpracovávány hello změny nebo pokud dokončení zpracování pro všechny členy uživatele).

   - Informace o uživatelích, kteří jsou v chybovém stavu, protože nebylo možné přiřadit licence toothem.

   ![Přiřazení možností](media/active-directory-licensing-group-assignment-azure-portal/assignment-errors.png)

3. Podrobnější informace o licenci zpracování v rámci **Azure Active Directory** > **uživatelů a skupin** > *název skupiny*  >  **Protokoly auditu**. Všimněte si hello následující činnosti:

   - Aktivita: **spustit použití skupinu na základě licencí toousers**. To se protokolují, když systém hello převezme hello změnit přiřazení licence na hello skupiny a jeho použití členy uživatelské tooall spustí. Obsahuje informace o změně hello, která byla vytvořená.

   - Aktivita: **dokončit použití skupinu na základě licencí toousers**. To se protokolují, když hello systému dokončí zpracování všechny uživatele ve skupině hello. Obsahuje souhrn počet uživatelů, kteří byly úspěšně zpracovány a kolik uživatelů nebylo možné přiřadit skupinu licencí.

   [Tato část](./active-directory-licensing-group-advanced.md#use-audit-logs-to-monitor-group-based-licensing-activity) toolearn informace o tom, jak protokoly auditu může být použité tooanalyze změny provedené na základě skupin licencí.

## <a name="step-3-check-for-license-problems-and-resolve-them"></a>Krok 3: Zkontrolujte licenční problémy a jejich řešení

1. Přejděte příliš**Azure Active Directory** > **uživatelů a skupin** > **všechny skupiny**a najít hello **Personální oddělení**skupinu, která licencí bylo přiřazeno k.
2. Na hello **Personální oddělení** okno skupiny, vyberte **licence**. Hello oznámení nad hello okno ukazuje, že 10 uživatelů, které nebylo možné přiřadit licence do. Na něj kliknete, otevře se seznam všech uživatelů v licencování chybový stav pro tuto skupinu.
3. Hello **se nezdařilo přiřazení** sloupec víme, že oba licence k produktům nebylo možné přiřadit toohello uživatele. Hello **Top důvodem selhání** sloupec obsahuje hello příčinu selhání hello. V takovém případě má **plány služby konfliktní**.

   ![Přiřazení se nezdařila](media/active-directory-licensing-group-assignment-azure-portal/failed-assignments.png)

4. Vyberte uživatel tooopen hello **licence** okno. Toto okno obsahuje všechny licence, které jsou přiřazeny toohello uživatele. V tomto příkladu má uživatel hello hello Office 365 Enterprise E1 licenci, která byla zděděna od hello **celoobrazovkovém uživatelé** skupiny. Tato možnost v konfliktu s hello E3 licenci, která hello tooapply systému se pokusili z hello **Personální oddělení** skupiny. V důsledku toho žádné licence hello z dané skupiny bylo přiděleno toohello uživatele.

   ![Zobrazení licence pro uživatele](media/active-directory-licensing-group-assignment-azure-portal/user-license-view.png)

5. toosolve tomuto konfliktu, uživatel hello odebrat z hello **celoobrazovkovém uživatelé** skupiny. Po Azure AD zpracovává hello změnu, hello **Personální oddělení** správně přiřazené licence.

   ![Správně přiřazenou licenci](media/active-directory-licensing-group-assignment-azure-portal/license-correctly-assigned.png)

## <a name="next-steps"></a>Další kroky

toolearn informace o funkci hello sada pro správu licencí prostřednictvím skupiny, najdete v části hello následující články:

* [Co je na základě skupin licencování v Azure Active Directory?](active-directory-licensing-whatis-azure-portal.md)
* [Identifikace a řešení potíží s licencí pro skupinu v Azure Active Directory](active-directory-licensing-group-problem-resolution-azure-portal.md)
* [Jak toomigrate jednotlivé licencované uživatele toogroup základě licencování v Azure Active Directory](active-directory-licensing-group-migration-azure-portal.md)
* [Azure Active Directory na základě skupin licencí další scénáře](active-directory-licensing-group-advanced.md)
