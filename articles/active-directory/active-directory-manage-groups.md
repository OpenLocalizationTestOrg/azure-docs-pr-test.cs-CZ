---
title: "Použití skupin pro správu přístupu k prostředkům v Azure Active Directory | Microsoft Docs"
description: "Postup používání skupin v Azure Active Directory můžete spravovat přístup uživatelů k místní a cloudové aplikace a prostředky."
services: active-directory
documentationcenter: 
author: curtand
manager: mtillman
editor: 
ms.assetid: 714120d0-cdf9-465d-afee-39bef591c6b3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017;it-pro
ms.reviewer: piotrci
ms.openlocfilehash: aaccc501526d313a572692ff8f2f5c9da38849d3
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="manage-access-to-resources-with-azure-active-directory-groups"></a>Spravovat přístup k prostředkům pomocí skupin Azure Active Directory
Azure Active Directory (Azure AD) je komplexní identit a přístupu řešení správy, poskytuje výkonnou sadu funkcí, které chcete spravovat přístup k místní a cloudové aplikace a prostředky, včetně služeb Microsoft online services třeba Office 365 a World aplikací Microsoft SaaS. Tento článek obsahuje přehled, ale pokud chcete spustit pomocí služby Azure AD seskupení právě teď, postupujte podle pokynů v [Správa skupin zabezpečení ve službě Azure AD](active-directory-groups-create-azure-portal.md). Pokud chcete zobrazit, jak můžete použít PowerShell ke správě skupin ve službě Azure Active directory si můžete přečíst více v [rutiny služby Azure Active Directory pro správu skupin](active-directory-accessmanagement-groups-settings-v2-cmdlets.md).

> [!NOTE]
> Chcete-li používat Azure Active Directory, potřebujete účet Azure. Pokud účet nemáte, můžete [si zaregistrovat bezplatný účet Azure](https://azure.microsoft.com/pricing/free-trial/).
>
>

V rámci Azure AD mezi hlavní funkce je schopnost spravovat přístup k prostředkům. Tyto prostředky můžou být součástí adresáři, jako v případě oprávnění ke správě objektů pomocí rolí v adresáři nebo prostředky, které jsou externí vzhledem k adresáři, jako je například aplikace SaaS, služby Azure a webů služby SharePoint nebo místních prostředků. Existují čtyři způsoby, které uživatele lze přiřadit přístupová práva k prostředku:

1. Přímé přiřazení

    Vlastník prostředku může být přímo k prostředku přiřazena uživatelům.
2. Členství ve skupině

    Skupinu můžete přiřadit k prostředku vlastníkem prostředku a díky tomu, udělení členové skupiny přístup k prostředku. Členství ve skupině potom můžete spravovat vlastníkem skupiny. Vlastník prostředku deleguje prakticky oprávnění přiřazovat uživatele k jeho prostředku vlastníkovi skupiny.
3. Založený na pravidlech

    Vlastník prostředku může použít pravidlo k express, kteří uživatelé by se měla přiřadit přístup k prostředku. Výsledek pravidlo závisí na atributy používané v tomto pravidle a jejich hodnoty pro konkrétní uživatele, a díky tomu vlastník prostředku deleguje efektivně právo spravovat přístup k jejich prostředek pro autoritativní zdroj pro atributy, které se používají v Toto pravidlo. Vlastník prostředku stále spravuje pravidlo samotné a určí, které atributy a hodnoty poskytovat přístup k jejich prostředkům.
4. Externí autority

    Přístup k prostředku je odvozený z externího zdroje; například skupina, která se synchronizují z autoritativní zdroj například místního adresáře nebo SaaS aplikace například WorkDay. Vlastník prostředku přiřadí skupinu a poskytne přístup k prostředku a externí zdroj spravuje členy skupiny.

   ![Přehled diagram správy přístupu](./media/active-directory-access-management-groups/access-management-overview.png)

## <a name="watch-a-video-that-explains-access-management"></a>Přehrát video, které popisuje správu přístupu
Podívejte se na krátké video, které vysvětluje více o toto:

**Azure AD: Úvod do dynamické členství ve skupinách**

> [!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Azure-AD--Introduction-to-Dynamic-Memberships-for-Groups/player]
>
>

## <a name="how-does-access-management-in-azure-active-directory-work"></a>Jak Správa v Azure Active Directory pracovní přístupu?
V centru Azure AD je řešení pro správu přístupu skupiny zabezpečení. Pomocí skupiny zabezpečení ke správě přístupu k prostředkům je dobře známé zlepší, která umožňuje flexibilní a snadno srozumitelné způsob, jak poskytnout přístup k prostředku pro zamýšlené skupiny uživatelů. Vlastník prostředku (nebo správce adresáře) můžete přiřadit skupiny k poskytování určité přístupu k prostředkům, které vlastní. Členové skupiny se poskytnout přístup a vlastníka prostředku můžete udělit oprávnění ke správě seznamu členů skupiny jinému uživateli, jako je například oddělení správce nebo správce technické podpory.

![Diagram správy přístupu služby Azure Active Directory](./media/active-directory-access-management-groups/active-directory-access-management-works.png)

Vlastník skupiny, můžete zpřístupnit této skupiny pro samoobslužné služby požadavky. Tím, koncový uživatel může hledat a najít skupinu a podání žádosti o připojení, efektivně vyhledávání oprávnění pro přístup k prostředkům, které jsou spravované prostřednictvím skupiny. Vlastník skupiny můžete nastavit skupiny tak, aby požadavků spojení budou automaticky schváleny nebo vyžadovat schválení vlastníka skupiny. Když uživatel odešle žádost o připojení ke skupině, žádost o připojení se předají na vlastníky skupiny. Pokud jeden z vlastníci schválí požadavek, je upozornění žádajícího uživatele a uživatel je připojený ke skupině. Pokud jeden z vlastníci odmítne požadavek, je žádajícího uživatele oznámení, ale není připojen ke skupině.

## <a name="getting-started-with-access-management"></a>Začínáme se správou přístupu
Jste připravení začít? Vyzkoušejte si základní úlohy, které můžete provést pomocí skupin Azure AD. Použijte tyto možnosti zajistit specializované přístup na různé skupiny uživatelů pro různé prostředky ve vaší organizaci. Seznam základní první kroky jsou uvedeny níže.

* [Vytvoření jednoduché pravidlo konfigurace dynamické členství ve skupině](active-directory-groups-create-azure-portal.md)
* [Pomocí skupiny pro správu přístupu k aplikacím SaaS](active-directory-accessmanagement-group-saasapps.md)
* [Zpřístupnění skupiny pro koncového uživatele samoobslužné služby](active-directory-accessmanagement-self-service-group-management.md)
* [Synchronizuje se místní skupině do Azure pomocí Azure AD Connect](active-directory-aadconnect.md)
* [Správa vlastníků pro skupinu](active-directory-accessmanagement-managing-group-owners.md)

## <a name="next-steps"></a>Další kroky
Teď, když porozuměl základní informace o správu přístupu tady jsou některé další rozšířené možnosti dostupné v Azure Active Directory pro správu přístupu k aplikacím a prostředkům.

* [Vytváření rozšířených pravidel pomocí atributů](active-directory-groups-dynamic-membership-azure-portal.md)
* [Správa skupin zabezpečení ve službě Azure AD](active-directory-groups-create-azure-portal.md)
* [Nastavení vyhrazené skupiny ve službě Azure AD](active-directory-accessmanagement-dedicated-groups.md)
* [Referenční dokumentace rozhraní Graph API pro skupiny](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/groups-operations#GroupFunctions)
* [Rutiny služby Azure Active Directory pro konfiguraci nastavení skupiny](active-directory-accessmanagement-groups-settings-cmdlets.md)
