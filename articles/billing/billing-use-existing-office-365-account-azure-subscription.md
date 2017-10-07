---
title: "aaaSign pro službu Azure pomocí účtu Office 365 | Microsoft Docs"
description: "Zjistěte, jak toocreate předplatné služby Azure pomocí účtu Office 365"
services: 
documentationcenter: 
author: JiangChen79
manager: adpick
editor: 
tags: billing,top-support-issue
ms.assetid: 129cdf7a-2165-483d-83e4-8f11f0fa7f8b
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 08/03/2017
ms.author: cjiang
ms.openlocfilehash: cc331bf7222b5b03e740cb40a214bc13ef585f54
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sign-up-for-an-azure-subscription-with-your-office-365-account"></a>Zaregistrujte si předplatné Azure pomocí svého účtu Office 365
Pokud máte předplatné služeb Office 365, můžete vytvořit vaše toocreate účet Office 365 předplatné Azure. Přihlaste se toohello [portál Azure](https://portal.azure.com/) pomocí Office 365 uživatelské jméno a heslo. Chcete-li tooset až virtuálních počítačů nebo používat jinými službami Azure, musíte se musí zaregistrovat pro předplatné Azure. Vaše předplatné Azure můžete sdílet s ostatními a [pomocí řízení přístupu na základě Role toomanage přístup tooyour předplatného Azure a prostředky](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure)

Pokud již máte účet Office 365 a předplatné Azure, najdete v části [přidružit Office 365 klienta tooan předplatné](billing-add-office-365-tenant-to-azure-subscription.md).

## <a name="get-an-azure-subscription-using-your-office-365-account"></a>Získat předplatné Azure pomocí svého účtu Office 365

Ušetřit čas a vyhnout se účet šíření nutná registrace do Azure pomocí Office 365 uživatelské jméno a heslo. 

1. Zaregistrovat na [Azure.com](https://account.azure.com/signup?offer=MS-AZR-0044p&appId=docs). 
2. Přihlaste se pomocí Office 365 uživatelské jméno a heslo. Hello účet nevyžaduje oprávnění správce toohave. Pokud máte více než jeden účet Office 365, ujistěte se, že používáte hello pověření pro účet hello Office 365, které chcete tooassociate s předplatným Azure. 

   ![Snímek obrazovky zobrazující přihlašovací stránku hello.](./media/billing-use-existing-office-365-account-azure-subscription/billing-sign-in-with-office-365-account.png)

3. Zadejte informace požadované hello a hello dokončení procesu registrace. Pokud již máte účet Office 365, nemusí být některé informace požadované.

    ![Snímek obrazovky zobrazující hello registračního formuláře.](./media/billing-use-existing-office-365-account-azure-subscription/billing-azure-sign-up-fill-information.png)

- Pokud budete potřebovat tooadd jiní lidé ve vaší organizaci toohello předplatné Azure, najdete v části [Začínáme se správou přístupu v hello portál Azure](../active-directory/role-based-access-control-what-is.md). 

## <a id="more-about-subs">Další informace o předplatných Azure a Office 365</a>
Office 365 a Azure používat uživatelé toomanage služby Azure AD hello a odběry. Hello adresáře Azure je jako kontejner, ve kterém můžete skupině uživatelů a odběry. toouse hello stejných uživatelských účtů pro předplatné Azure a Office 365, musíte toomake se, že hello Azure odběry jsou vytvořené v hello stejný adresář jako odběry hello Office 365. Mějte na paměti hello následující body:

* Předplatné získá vytvořil v rámci adresáře
* Uživatelé patří toodirectories
* Předplatné pojmenováváme v adresáři hello hello uživatele, který vytvoří předplatné hello. Předplatné služeb Office 365 je vázané toohello stejný účet jako vašeho předplatného Azure.
* Předplatná Azure jsou vlastněny jednotlivé uživatele v adresáři hello
* Předplatná Office 365 jsou vlastněny hello directory sám sebe. Uživatelé s hello správná oprávnění v rámci hello adresář můžete spravovat tyto odběry.

![Snímek obrazovky, který znázorňuje vztah hello hello adresáře, uživatelů a odběry.](./media/billing-use-existing-office-365-account-azure-subscription/19-background-information.png)

Další informace najdete v tématu [asociování předplatných Azure se službou Azure Active Directory](../active-directory/active-directory-how-subscriptions-associated-directory.md).

## <a name="need-help-contact-support"></a>Potřebujete pomoct? Obraťte se na podporu.
Pokud stále potřebujete pomoc, [obraťte se na podporu](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget rychle vyřešit problém. 
