---
title: aaaDedicated skupin v Azure Active Directory | Microsoft Docs
description: "Přehled o tom, jak vyhrazené skupiny fungují v Azure Active Directory a jak se vytvářejí."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 86158909-083a-41fe-8090-955e96ad1865
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2017
ms.author: curtand
ms.openlocfilehash: 8feec6e1a4e6b384470392d3043caeeec2b03dd2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="dedicated-groups-in-azure-active-directory"></a>Vyhrazené skupiny ve službě Azure Active Directory
V Azure Active Directory (Azure AD) funkce vyhrazené skupiny hello automaticky vytvoří a naplní členství ve skupinách Azure AD předdefinované. Nelze přidat členy vyhrazené skupiny nebo odebrané pomocí hello Azure classic portálu, rutiny prostředí Windows PowerShell, nebo prostřednictvím kódu programu.

> [!NOTE]
> Vyhrazené skupiny vyžadovat, že je přiřazený licenci Azure AD Premium
>
> * Hello správce, který spravuje hello pravidlo pro skupinu
> * všichni uživatelé, kteří ve hello jsou vybrané pravidlo toobe členem skupiny hello
>
>

**tooenable vyhrazené skupiny**

1. V hello [portál Azure classic](https://manage.windowsazure.com), vyberte **služby Active Directory**a pak otevřete adresáři vaší organizace.
2. Vyberte hello **skupiny** kartu a pak otevřete hello skupiny chcete tooedit.
3. Vyberte hello **konfigurace** kartě a poté nastavte **povolte vyhrazené skupiny** příliš**Ano**.

Jakmile hello přepínač Povolit vyhrazené skupiny je nastaven příliš**Ano**, můžete povolit další hello directory tooautomatically vytvořit hello všichni uživatelé vyhrazené skupiny nastavení hello **povolit "Všichni uživatelé" skupiny** přepínač příliš**Ano**. Pak se taky dají upravovat hello název této vyhrazené skupiny zadáním názvu do hello **zobrazovaný název "Všichni uživatelé" skupina** pole.

Hello všechny uživatele skupiny lze tooassign hello stejné oprávnění tooall hello uživatelé v adresáři. Například můžete udělit všichni uživatelé ve vaší directory přístup tooa aplikace SaaS přiřadit přístup pro hello všichni uživatelé vyhrazenou skupinu toothis aplikaci.

Hello vyhrazené všichni uživatelé skupina obsahuje všechny uživatele v adresáři hello, včetně hostů a externí uživatele. Pokud potřebujete skupinu, vyloučí externí uživatele, a poté se dá dosáhnout vytvořením skupiny se na základě atributů dynamické pravidlo například hello následující:

                (user.userPrincipalName -notContains "#EXT#@")

Pro skupinu, která nezahrnuje všechny hosté použijte pravidlo například hello následující:

                (user.userType -ne "Guest")

toolearn o tom, toocreate *rozšířené* pravidel (která může obsahovat několik porovnání) pro dynamické členství ve skupině, najdete v části [pravidel pomocí atributů toocreate rozšířené](active-directory-accessmanagement-groups-with-advanced-rules.md).

### <a name="next-steps"></a>Další kroky
Následující články poskytují další informace o službě Azure Active Directory.

* [Správa přístupu tooresources pomocí skupin Azure Active Directory](active-directory-manage-groups.md)
* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md)
* [Představení služby Azure Active Directory](active-directory-whatis.md)
* [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md)
