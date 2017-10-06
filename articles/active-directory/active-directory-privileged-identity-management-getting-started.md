---
title: "aaaGet začít s Azure AD Privileged Identity managementu | Microsoft Docs"
description: "Zjistěte, jak toomanage privilegované identity s hello aplikace Azure Active Directory Privileged Identity Management na portálu Azure."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 2299db7d-bee7-40d0-b3c6-8d628ac61071
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/27/2017
ms.author: billmath
ms.custom: pim ; H1Hack27Feb2017
ms.openlocfilehash: a89205023a8dbcc3649fa732735ca927e64736ce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="start-using-azure-ad-privileged-identity-management"></a>Začátek práce s aplikací Azure AD Privileged Identity Management
Pomocí aplikace Azure Active Directory (AD) Privileged Identity Management můžete spravovat, řídit a sledovat přístup v rámci organizace. Tento rozsah zahrnuje tooresources přístupu ve službě Azure AD a dalších služeb Microsoft online services jako je Office 365 nebo Microsoft Intune.

Tento článek vysvětluje, jak tooadd hello tooyour aplikace Azure AD PIM řídicí panel portálu Azure.

## <a name="add-hello-privileged-identity-management-application"></a>Přidání aplikace Privileged Identity Management hello
Před použitím Azure AD Privileged Identity Management musíte tooadd hello aplikace tooyour řídicí panel portálu Azure.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com/) jako globální správce adresáře.
2. Pokud má vaše organizace více než jeden adresář, vyberte své uživatelské jméno v hello pravém horním rohu hello portálu Azure. Vyberte adresář hello místo toouse PIM.
3. Vyberte **další služby** a používat hello filtru textbox toosearch pro **Azure AD Privileged Identity Management**.
4. Zkontrolujte **Pin toodashboard** a pak klikněte na **vytvořit**. Otevře se Hello aplikace Privileged Identity Management.

Pokud jste hello první, kdo toouse Azure AD Privileged Identity Management ve vašem adresáři, se automaticky přiřazují hello **správce zabezpečení** a **správce privilegovaných rolí** role v adresáři hello. Pouze správci privilegovaných rolí můžou spravovat přiřazení rolí uživatelů. Kromě toho můžete zvolit toorun hello [Průvodce zabezpečení.](active-directory-privileged-identity-management-security-wizard.md) vás provede počáteční zjišťování a přiřazení prostředí hello.

## <a name="navigate-tooyour-tasks"></a>Přejděte tooyour úlohy
Jakmile Azure AD Privileged Identity Management nastavená, zobrazí se při každém otevření aplikace hello hello navigačním okně. Použijte toto okno tooaccomplish vaše úkoly správy identit.

![Úkoly nejvyšší úrovně v PIM – snímek obrazovky](./media/active-directory-privileged-identity-management-getting-started/PIM_Tasks_New.png)

* **Moje role** přejdete tooa seznam rolí, které jsou přiřazeny tooyou. V této části můžete aktivovat role, ke kterým máte oprávnění.
* **Schválit žádosti (Preview)** zobrazí seznam žádostí uživatelů ve vašem adresáři o aktivaci, které čekají na schválení. [Další informace](./privileged-identity-management/azure-ad-pim-approval-workflow.md)
* **Nevyřízené žádosti (Preview)** zobrazí všechny aktuální toohave provedené tooactivate požadavky.
* **Kontrolujte přístup** trvá tooany čekající na vyřízení přístupu zkontroluje, je nutné, aby toocomplete, zda jste kontroly přístupu k sobě nebo někomu jinému.
* **Role adresář Azure AD** najdete v části "Správa" hello je hello řídicího panelu pro přiřazení rolí toomanage privilegované role správců, změnit nastavení aktivace role, počáteční přístup recenze a další. Možnosti Hello v tomto řídicím panelu jsou zakázaná pro každý, kdo není privilegované role správce.

## <a name="next-steps"></a>Další kroky
Hello [přehled Azure AD Privileged Identity Management](active-directory-privileged-identity-management-configure.md) obsahuje podrobné informace o tom, jak můžete spravovat přístup pro správu ve vaší organizaci.

[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-configure/PIM_EnablePim.png
