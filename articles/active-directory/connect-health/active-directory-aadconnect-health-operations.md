---
title: operace aaaAzure Active Directory Connect Health
description: "Tento článek popisuje další operace, které lze provést po nasazení Azure AD Connect Health."
services: active-directory
documentationcenter: 
author: karavar
manager: femila
ms.assetid: 86cc3840-60fb-43f9-8b2a-8598a9df5c94
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 1dddcee0bca3150ce08621c045a92a1b3ad9df30
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-connect-health-operations"></a>Operace v Azure Active Directory Connect Health
Toto téma popisuje hello různé operace můžete provádět pomocí Azure Active Directory (Azure AD) Connect Health.

## <a name="enable-email-notifications"></a>Povolení e-mailových oznámení
Hello Azure AD Connect Health service toosend e-mailová oznámení lze konfigurovat výstrahy indikují, že infrastruktury identity není v pořádku. K tomu dojde, pokud je vygenerována výstraha a pokud je vyřešený.

![Nastavení oznámení e-mailu snímek Azure AD Connect Health](./media/active-directory-aadconnect-health/email_noti_discover.png)

> [!NOTE]
> E-mailová oznámení jsou ve výchozím nastavení povolené.
>
>

### <a name="tooenable-azure-ad-connect-health-email-notifications"></a>tooenable Azure AD Connect Health e-mailových oznámení
1. Otevřete hello **výstrahy** okno hello služby, pro které chcete tooreceive e-mailové oznámení.
2. Z panelu hello akce, klikněte na tlačítko **nastavení oznámení**.
3. V hello e-mailové oznámení přepínače, vyberte **ON**.
4. Hello políčko zaškrtněte, pokud chcete, aby všichni globální správci tooreceive e-mailová oznámení.
5. Pokud chcete tooreceive e-mailová oznámení na jiné e-mailové adresy, zadejte je v hello **další příjemců e-mailu** pole. tooremove e-mailovou adresu z tohoto seznamu, klikněte pravým tlačítkem na položku hello a vyberte **odstranit**.
6. toofinalize hello změny, klikněte na tlačítko **Uložit**. Změny se projeví až po uložení.

## <a name="delete-a-server-or-service-instance"></a>Odstranění instance serveru nebo služby

V některých případech můžete chtít tooremove server z monitorovány. Zde je budete potřebovat tooknow tooremove server z hello Azure AD Connect Health service.

Pokud odstraňujete serveru, nezapomeňte hello následující:

* Tato akce zastaví sběr žádná další data z tohoto serveru. Tento server bude odebrán z hello monitorování služby. Po této akci se není možné tooview nové výstrahy, monitorování nebo využití analytická data pro tento server.
* Tato akce neodinstaluje hello agenta stavu ze serveru. Pokud hello agenta stavu neodinstalovali před provedením tohoto kroku, může se zobrazit chyby související toohello stav agenta na hello server.
* Tato akce neodstraní data hello již shromážděná z tohoto serveru. Data odstraněna v souladu s hello zásady uchovávání dat Azure.
* Po provedení této akce, pokud chcete monitorování toostart hello stejný server znovu, je nutné odinstalovat a přeinstalovat hello agenta stavu na tomto serveru.

### <a name="toodelete-a-server-from-hello-azure-ad-connect-health-service"></a>toodelete server z hello Azure AD Connect Health service
Azure AD Connect Health pro Active Directory Federation Services (AD FS) a Azure AD Connect (Sync):

1. Otevřete hello **Server** okno z hello **seznam serverů** okno výběrem toobe název serveru hello odebrat.
2. Na hello **Server** , z panelu hello akce, klikněte na **odstranit**.
3. Potvrďte zadáním názvu serveru hello hello potvrzení pole.
4. Klikněte na **Odstranit**.

Azure AD Connect Health pro službu Azure Active Directory Domain Services:

1. Otevřete hello **řadiče domény** řídicího panelu.
2. Odebrat toobe řadič domény vyberte hello.
3. Z panelu hello akce, klikněte na tlačítko **odstranit vybrané**.
4. Potvrďte hello akce toodelete hello serveru.
5. Klikněte na **Odstranit**.

### <a name="delete-a-service-instance-from-azure-ad-connect-health-service"></a>Odstranění instance služby ze služby Azure AD Connect Health
V některých případech můžete chtít tooremove instance služby. Tady je v tom, co jste třeba tooknow tooremove služby instanci ze služby hello Azure AD Connect Health.

Pokud odstraňujete instance služby, nezapomeňte hello následující:

* Tato akce odebere aktuální instanci služby hello z hello monitorování služby.
* Tato akce neodinstaluje ani neodebere agenta stavu hello z libovolného hello serverů, které byly monitorovány v rámci této instance služby. Pokud hello agenta stavu neodinstalovali před provedením tohoto kroku, může se zobrazit chyby související toohello agenta stavu na serverech hello.
* V souladu s hello zásady uchovávání dat Azure budou odstraněna všechna data z této instance služby.
* Po provedení této akce, pokud chcete toostart hello službu monitorování, odinstalujte a znovu nainstalujte hello agenta stavu ve všech serverech hello. Po provedení této akce, pokud chcete, aby toostart monitorování hello stejný server znovu, odinstalovat, přeinstalovat a zaregistrovat hello na tomto serveru agenta stavu.

#### <a name="toodelete-a-service-instance-from-hello-azure-ad-connect-health-service"></a>toodelete instance služby z hello Azure AD Connect Health service
1. Otevřete hello **služby** okno z hello **seznam služeb** okno výběrem identifikátoru služby hello (název farmy), které chcete tooremove.
2. Na hello **Server** , z panelu hello akce, klikněte na **odstranit**.
3. Potvrďte zadáním názvu služby hello hello potvrzení pole (například:. sts.contoso.com).
4. Klikněte na **Odstranit**.
   <br><br>

[//]: # (Start of RBAC section)
## <a name="manage-access-with-role-based-access-control"></a>Správa přístupu pomocí řízení přístupu na základě rolí
[Řízení přístupu na základě rolí (RBAC)](../role-based-access-control-configure.md) pro Azure AD Connect Health poskytuje toousers přístup a skupiny než globální správci. RBAC přiřadí role toohello určený uživatelé a skupiny a poskytuje mechanismus toolimit hello globálních správců v adresáři.

### <a name="roles"></a>Role
Azure AD Connect Health podporuje následující předdefinované role hello:

| Role | Oprávnění |
| --- | --- |
| Vlastník |Mohou vlastníci *spravovat přístup* (například přiřadit role tooa uživatele nebo skupiny), *prohlížet všechny informace* (například zobrazení výstrahy) z portálu hello a *změnit nastavení* () například e-mailová oznámení) v rámci Azure AD Connect Health. <br>Ve výchozím nastavení tuto roli nemají přiřazenou globální správce Azure AD a toto nastavení nelze změnit. |
| Přispěvatel |Přispěvatelé můžou *prohlížet všechny informace* (například zobrazení výstrahy) z portálu hello a *změnit nastavení* (například e-mailová oznámení) v rámci Azure AD Connect Health. |
| Čtenář |Můžete čtečky *prohlížet všechny informace* (například zobrazení výstrahy) z portálu hello v rámci Azure AD Connect Health. |

Všechny ostatní role (například uživatel přístup správci nebo uživatelé DevTest Labs) mít žádný vliv tooaccess v rámci Azure AD Connect Health, i když jsou dostupné v prostředí portálu hello hello role.

### <a name="access-scope"></a>Obor přístupu
Azure AD Connect Health podporuje správu přístupu na dvou úrovních:

* **Všechny instance služby**: Toto je doporučená cestu ve většině případů hello. Určuje přístup pro všechny instance služby (například farmu služby AD FS), typů role, které jsou monitorovány pomocí Azure AD Connect Health.
* **Instance služby**: V některých případech může být nutné toosegregate přístupu založených na typech role nebo instance služby. V takovém případě můžete spravovat přístup na úrovni instance služby hello.  

Je povoleno, pokud koncový uživatel má přístup buď na hello directory nebo služby na úrovni instance.

### <a name="allow-users-or-groups-access-tooazure-ad-connect-health"></a>Povolit uživatelům nebo skupinám přístup tooAzure AD Connect Health
Hello následující kroky ukazují, jak přistupovat k tooallow.
#### <a name="step-1-select-hello-appropriate-access-scope"></a>Krok 1: Vyberte odpovídající přístup obor hello
tooallow přístup uživatelů na hello *všech instancí služby* úrovně v rámci Azure AD Connect Health, otevřete hello hlavní okno v Azure AD Connect Health.<br>

#### <a name="step-2-add-users-and-groups-and-assign-roles"></a>Krok 2: Přidání uživatelů a skupin a přiřadit role
1. Z hello **konfigurace** klikněte na tlačítko **uživatelé**.<br>
   ![Snímek obrazovky Azure AD připojit RBAC stavu hlavním okně, s uživateli zvýrazněná](./media/active-directory-aadconnect-health/RBAC_main_blade.png)
2. Vyberte **Přidat**.
3. V hello **vyberte roli** podokně, vyberte roli (například **vlastníka**).<br>
   ![Snímek Azure AD Connect Health RBAC uživatelé okno](./media/active-directory-aadconnect-health/RBAC_add.png)
4. Zadejte hello název nebo identifikátor cílové hello uživatele nebo skupinu. Můžete vybrat jeden nebo více uživatelů nebo skupin v hello stejnou dobu. Klikněte na **Vybrat**.
   ![Snímek Azure AD Connect Health RBAC uživatelé okno](./media/active-directory-aadconnect-health/RBAC_select_users.png)
5. Vyberte **OK**.<br>
6. Po dokončení přiřazení role hello se zobrazí v seznamu hello hello uživatelů a skupin.<br>
   ![Připojení stavu RBAC uživatelům okno snímek obrazovky Azure AD, s nových uživatelů zvýrazněných](./media/active-directory-aadconnect-health/RBAC_user_list.png)

Nyní hello uvedené uživatelé a skupiny mají přístup, podle tootheir přiřazené role.

> [!NOTE]
> * Globální správci vždy mají plný přístup tooall hello operace, ale nejsou k dispozici v předchozím seznamu hello globálního správce účtů.
> * Hello pozvat uživatele funkce není podporována v rámci Azure AD Connect Health.
>
>

#### <a name="step-3-share-hello-blade-location-with-users-or-groups"></a>Krok 3: Sdílené složky hello okno umístění k uživatelům nebo skupinám
1. Po přiřazení oprávnění, může uživatel získat přístup Azure AD Connect Health přechodem [zde](http://aka.ms/aadconnecthealth).
2. V okně hello hello uživatele můžete Připnout okno hello nebo různé části ho toohello řídicího panelu. Jednoduše klikněte na tlačítko hello **Pin toodashboard** ikonu.<br>
   ![Snímek Azure AD Connect Health RBAC Připnout okno, se zvýrazněnou ikonu Připnutí](./media/active-directory-aadconnect-health/RBAC_pin_blade.png)

> [!NOTE]
> Uživatel s přiřazenou roli čtečky hello není možné tooget rozšíření Azure AD Connect Health hello Azure Marketplace. Hello uživatele proto nelze provést hello nezbytné "vytvořit" toodo operaci. Hello uživatele stále můžete získat toohello okno budete toohello předcházející odkaz. Pro následné použití můžete uživatele hello připnout hello okno toohello řídicího panelu.
>
>

### <a name="remove-users-or-groups"></a>Odebrat uživatele nebo skupiny
Můžete odebrat uživatele nebo skupinu přidali tooAzure AD RBAC stavu připojení. Jednoduše klikněte pravým tlačítkem na hello uživatele nebo skupinu a vyberte **odebrat**.<br>
![Připojení stavu RBAC uživatelům okno snímek obrazovky Azure AD, s odebrat zvýrazněná](./media/active-directory-aadconnect-health/RBAC_remove.png)

[//]: # (End of RBAC section)

## <a name="next-steps"></a>Další kroky
* [Azure AD Connect Health](active-directory-aadconnect-health.md)
* [Instalace agenta služby Azure AD Connect Health](active-directory-aadconnect-health-agent-install.md)
* [Používání služby Azure AD Connect Health se službou AD FS](active-directory-aadconnect-health-adfs.md)
* [Používání služby Azure AD Connect Health pro synchronizaci](active-directory-aadconnect-health-sync.md)
* [Používání služby Azure AD Connect Health se službou AD DS](active-directory-aadconnect-health-adds.md)
* [Azure AD Connect Health – nejčastější dotazy](active-directory-aadconnect-health-faq.md)
* [Historie verzí služby Azure AD Connect Health](active-directory-aadconnect-health-version-history.md)
