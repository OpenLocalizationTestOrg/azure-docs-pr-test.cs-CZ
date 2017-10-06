---
title: "aaaUse skupiny toomanage tooresources přístup v Azure Active Directory | Microsoft Docs"
description: "Jak toouse skupin v Azure Active Directory toomanage uživatel přistupovat k tooon místní a cloudové aplikace a prostředky."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 714120d0-cdf9-465d-afee-39bef591c6b3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 876a356c8095505432e9346721f35c7943819e9d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-access-tooresources-with-azure-active-directory-groups"></a>Spravovat přístup tooresources pomocí skupin Azure Active Directory
Azure Active Directory (Azure AD) je komplexní identit a přístupu řešení správy, poskytuje výkonnou sadu funkcí toomanage přístup tooon místní a cloudové aplikace a prostředky, včetně služeb Microsoft online services třeba Office 365 a celou řadu aplikací Microsoft SaaS. Tento článek obsahuje přehled, ale pokud chcete teď skupiny toostart pomocí služby Azure AD, postupujte podle pokynů hello v [Správa skupin zabezpečení ve službě Azure AD](active-directory-accessmanagement-manage-groups.md). Pokud chcete, aby toosee použití prostředí PowerShell toomanage skupiny ve službě Azure Active directory si můžete přečíst více v [rutiny služby Azure Active Directory pro správu skupin](active-directory-accessmanagement-groups-settings-v2-cmdlets.md).

> [!NOTE]
> toouse Azure Active Directory, potřebujete účet Azure. Pokud účet nemáte, můžete [si zaregistrovat bezplatný účet Azure](https://azure.microsoft.com/pricing/free-trial/).
>
>

V rámci Azure AD je jednou z hlavních funkcí hello hello možnost toomanage přístup tooresources. Tyto prostředky můžou být součástí hello adresář, jako v případě hello oprávnění toomanage objektů pomocí rolí v adresáři hello nebo prostředky, které jsou externí toohello adresáři, například aplikace SaaS, služby Azure a weby Sharepointu nebo místní prostředky. Existují čtyři způsoby, které uživatele lze přiřadit přístup práva tooa prostředků:

1. Přímé přiřazení

    Uživatelé se dají přiřadit přímo tooa prostředků hello vlastník prostředku.
2. Členství ve skupině

    Skupinu lze přiřadit tooa prostředků vlastníkem prostředku hello a díky tomu, udělení hello členové skupiny přístup toohello prostředku. Členství ve skupině hello potom můžete spravovat hello vlastník skupiny hello. Delegáti vlastníka prostředku hello prakticky hello oprávnění tooassign uživatelé tootheir toohello vlastník prostředku skupiny hello.
3. Založený na pravidlech

    vlastník prostředku Hello můžete použít pravidlo tooexpress, kteří uživatelé by se měla přiřadit přístup tooa prostředků. výsledek Hello hello pravidlo závisí na atributy hello používané v tomto pravidle a jejich hodnoty pro konkrétní uživatele, a díky tomu vlastník prostředku hello efektivně deleguje hello správné toomanage přístup tootheir prostředků toohello autoritativní zdroj pro hello atributy, které se používají v pravidle hello. vlastník prostředku Hello stále spravuje hello pravidlo samotné a určí, které atributy a hodnoty poskytnout přístup tootheir prostředků.
4. Externí autority

    Hello přístup tooa prostředků je odvozený z externího zdroje; například skupina, která se synchronizují z autoritativní zdroj například místního adresáře nebo SaaS aplikace například WorkDay. vlastník prostředku Hello přiřadí hello skupiny tooprovide přístup toohello prostředků a externí zdroj hello spravuje hello členy skupiny hello.

   ![Přehled diagram správy přístupu](./media/active-directory-access-management-groups/access-management-overview.png)

## <a name="watch-a-video-that-explains-access-management"></a>Přehrát video, které popisuje správu přístupu
Podívejte se na krátké video, které vysvětluje více o toto:

**Azure AD: Úvod toodynamic členství ve skupinách**

> [!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Azure-AD--Introduction-to-Dynamic-Memberships-for-Groups/player]
>
>

## <a name="how-does-access-management-in-azure-active-directory-work"></a>Jak Správa v Azure Active Directory pracovní přístupu?
V hello center hello řešení pro správu přístupu k Azure AD je skupina zabezpečení hello. Použití zabezpečení skupiny toomanage přístup tooresources je dobře známé zlepší, která umožňuje flexibilní a snadno srozumitelné způsob tooprovide přístup tooa prostředku pro hello určený skupinu uživatelů. vlastník prostředku Hello (nebo hello správce adresáře hello) můžete přiřadit skupiny tooprovide určitých přístup správné toohello prostředků, které vlastní. Hello členy skupiny hello bude poskytnuta hello přístup a vlastník prostředku hello můžete delegovat hello správné toomanage hello seznamu členy skupiny toosomeone else, jako je například Správce oddělení nebo technickou podporu správce.

![Diagram správy přístupu služby Azure Active Directory](./media/active-directory-access-management-groups/active-directory-access-management-works.png)

Hello vlastník skupiny, můžete zpřístupnit této skupiny pro samoobslužné služby požadavky. Při tom koncový uživatel můžete vyhledat a najít skupinu hello a zkontrolujte toojoin požadavek efektivně vyhledávání oprávnění tooaccess hello prostředky, které jsou spravované prostřednictvím hello skupiny. Vlastník Hello hello skupiny můžete nastavit skupiny hello tak, aby požadavků spojení budou automaticky schváleny nebo vyžadovat schválení vlastníka hello hello skupiny. Když uživatel provede požadavek toojoin skupinu, žádost o připojení hello se předají toohello vlastníky hello skupiny. Pokud jeden z hello vlastníky schválí požadavek hello, je upozornění hello žádajícího uživatele a uživatel hello je připojený k toohello skupiny. Pokud jeden z hello vlastníky odmítne žádost hello, hello žádajícího uživatele upozornění, ale není připojen toohello skupiny.

## <a name="getting-started-with-access-management"></a>Začínáme se správou přístupu
Připraveno tooget spustit? Vyzkoušejte si hello základní úlohy, které můžete provést pomocí skupin Azure AD. Pomocí těchto možností tooprovide specializuje přístup toodifferent skupiny uživatelů pro různé prostředky ve vaší organizaci. Seznam základní první kroky jsou uvedeny níže.

* [Vytvoření jednoduché pravidlo tooconfigure dynamické členství ve skupinách pro skupinu](active-directory-accessmanagement-manage-groups.md#how-can-i-manage-the-membership-of-a-group-dynamically)
* [Pomocí skupiny toomanage přístup k tooSaaS aplikacím](active-directory-accessmanagement-group-saasapps.md)
* [Zpřístupnění skupiny pro koncového uživatele samoobslužné služby](active-directory-accessmanagement-self-service-group-management.md)
* [Synchronizuje se místní skupiny tooAzure pomocí služby Azure AD Connect](active-directory-aadconnect.md)
* [Správa vlastníků pro skupinu](active-directory-accessmanagement-managing-group-owners.md)

## <a name="next-steps"></a>Další kroky
Teď, když porozuměl hello základy správu přístupu tady jsou některé další rozšířené možnosti dostupné v Azure Active Directory pro správu přístup k tooyour aplikacím a prostředkům.

* [Toocreate advanced pravidel pomocí atributů.](active-directory-accessmanagement-groups-with-advanced-rules.md)
* [Správa skupin zabezpečení ve službě Azure AD](active-directory-accessmanagement-manage-groups.md)
* [Nastavení vyhrazené skupiny ve službě Azure AD](active-directory-accessmanagement-dedicated-groups.md)
* [Referenční dokumentace rozhraní Graph API pro skupiny](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/groups-operations#GroupFunctions)
* [Rutiny služby Azure Active Directory pro konfiguraci nastavení skupiny](active-directory-accessmanagement-groups-settings-cmdlets.md)
