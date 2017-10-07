---
title: "tooAzure přístup aaaManage fakturace použití rolí | Microsoft Docs"
description: 
services: 
documentationcenter: 
author: vikramdesai01
manager: vikdesai
editor: 
tags: billing
ms.assetid: e4c4d136-2826-4938-868f-a7e67ff6b025
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: vikdesai
ms.openlocfilehash: 5937fac5ffa5ca204eb03a1dcbc5e800b3d5eb74
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-access-toobilling-information-for-azure-using-role-based-access-control"></a>Spravovat přístup toobilling informace pro Azure pomocí řízení přístupu na základě rolí

Můžete udělit přístup Azure fakturační informace toomembers týmu přiřazením mezi hello následující uživatelské role tooyour předplatné: správce účtu, Správce služby, spolusprávcem, vlastník, Přispěvatel, Čtenář a fakturace čtečky. By mají přístup k informacím toobilling v hello [portál Azure](https://portal.azure.com/), a používají hello [fakturace rozhraní API](billing-usage-rate-card-overview.md) tooprogrammatically stažení faktury (jednou přihlásí) a podrobnosti o použití. Další informace o, který můžete udělit role, a které role co naleznete na adrese [role v Azure RBAC](../active-directory/role-based-access-built-in-roles.md).

## <a name="opt-in"></a>Povolení dalších uživatelů tooaccess faktury

Hello správce účtu musí vyjádřit výslovný souhlas pomocí hello [portál Azure](https://portal.azure.com/) povolit přístup tooinvoices pro ostatní uživatele nebo přes rozhraní API.

1. Jako hello účet správce, vyberte své předplatné z hello [odběry okno](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) na portálu Azure.

1. Vyberte **faktury** a potom **přístup tooinvoices**.

    ![Snímek obrazovky ukazuje, jak toodelegate přistupovat k tooinvoices](./media/billing-manage-access/AA-optin.png)

1. Zapnout **na** hello přístup a ukládají se změny hello, uživatelé tooallow v odběru vymezená role toodownload faktury.

    ![Snímek obrazovky ukazuje tooinvoice přístup toodelegate zapnutí a vypnutí](./media/billing-manage-access/AA-optinAllow.png)

Výslovným souhlasem umožňuje služby správce, spolusprávce, vlastník, Přispěvatel, Čtenář a fakturace čtečky u hello předplatné toodownload PDF faktury v hello portálu Azure. Faktury starší než prosinec 2016 jsou však k dispozici pouze toohello správce účtu teď.

Hello správce účtu můžete taky nakonfigurovat toohave faktury odesílaly e-mailem. Další, najdete v části toolearn [získat faktury v e-mailu](billing-download-azure-invoice-daily-usage-date.md).

## <a name="adding-users-toohello-billing-reader-role"></a>Přidání role fakturace čtečky toohello uživatelé

role čtenáře fakturace Hello má oprávnění jen pro čtení toosubscription fakturační informace na portálu Azure a žádné tooservices přístup, jako je například virtuální počítače a účty úložiště. Přiřadit toosomeone hello fakturace čtečky role, která potřebuje přístup k informacím fakturace předplatného toohello ale není hello toomanage možnost Azure services. Tato role je vhodný pro uživatele v organizaci, kteří pouze provádět správu finanční a nákladů pro předplatná Azure.

1. Vyberte své předplatné z hello [odběry okno](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) na portálu Azure.

1. Vyberte **přístup k ovládacímu prvku (IAM)** a pak klikněte na **přidat**.

    ![Snímek obrazovky ukazuje IAM v okně předplatné hello](./media/billing-manage-access/select-iam.PNG)

1. Zvolte **fakturace čtečky** v hello **vyberte roli** stránky.

    ![Snímek obrazovky ukazuje fakturace čtečky v automaticky otevřeném okně zobrazení hello](./media/billing-manage-access/select-roles.PNG)

1. Zadejte hello e-mailu pro uživatele hello tooinvite a pak klikněte na tlačítko **OK** toosend hello pozvánku.

    ![Snímek obrazovky zobrazující tooinvite tooenter e-mailu, někdo](./media/billing-manage-access/add-user.PNG)

1. Postupujte podle pokynů v hello pozvání e-mailu toolog v jako čtečku fakturace.

    ![Snímek obrazovky, který zobrazuje, co hello čtečky fakturace můžete zobrazit na portálu Azure](./media/billing-manage-access/billing-reader-view.png)

> [!NOTE]
> Hello fakturace čtečky funkce je ve verzi preview a zatím nepodporuje předplatného typu enterprise (EA) nebo neglobální cloudy.

## <a name="adding-users-tooother-roles"></a>Přidávání uživatelů tooother rolí

Uživatelé v jiných rolí, například vlastníka nebo přispěvatele, můžou používat jenom fakturační informace, ale také služby Azure. najdete v těchto rolí toomanage [přidání nebo změna role Správce služby Azure, které spravují předplatné hello nebo služby](billing-add-change-azure-subscription-administrator.md).

## <a name="who-can-access-hello-account-centerhttpsaccountwindowsazurecom"></a>Kdo má přístup k hello [centra účtů](https://account.windowsazure.com)?

V centru účtů toohello můžou přihlásit pouze hello správce účtu. Hello správce účtu je hello právní vlastníka předplatného hello. Ve výchozím nastavení, hello osobě, která registraci aplikace nebo koupili hello předplatné Azure je hello účet správce, pokud hello [byl přenos vlastnictví předplatného](billing-subscription-transfer.md) toosomebody else. Hello správce účtu můžete vytvářet odběry, zrušit předplatné, změnit hello fakturační adresu pro přihlášení k odběru a spravovat zásady přístupu pro předplatné hello.

## <a name="need-help-contact-support"></a>Potřebujete pomoct? Obraťte se na podporu.

Pokud máte další otázky, [obraťte se na podporu](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget rychle vyřešit problém.
