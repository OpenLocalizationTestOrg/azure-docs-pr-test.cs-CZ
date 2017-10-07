---
title: "zásady podmíněného přístupu na základě zařízení služby Azure Active Directory aaaConfigure | Microsoft Docs"
description: "Zjistěte, jak tooconfigure Azure Active Directory na základě zařízení podmíněný přístup, zásady."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: a27862a6-d513-43ba-97c1-1c0d400bf243
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: cc5923b8ccee4335442c17ef63b2ee6f098e104e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-azure-active-directory-device-based-conditional-access-policies"></a>Nakonfigurujte zásady podmíněného přístupu na základě zařízení služby Azure Active Directory

S [podmíněného přístupu Azure Active Directory (Azure AD)](active-directory-conditional-access-azure-portal.md), můžete upřesnit jak oprávněným uživatelům můžete přístup k prostředkům. Například můžete omezit hello přístup toocertain prostředky tootrusted zařízení. Zásady podmíněného přístupu, která vyžaduje důvěryhodné zařízení je také označován jako zásady podmíněného přístupu podle zařízení.

Toto téma vám poskytne informace o tom, jak tooconfigure na zařízení podmíněný přístup, zásady pro aplikace Azure AD připojené. 


## <a name="before-you-begin"></a>Než začnete

Podmíněný přístup využívající zařízení ties **podmíněného přístupu Azure AD** a **správy zařízení služby Azure AD společně**. Pokud nejste obeznámeni s jedním z těchto oblastí ještě, přečtěte si následující témata, nejprve hello:

- **[Podmíněný přístup v Azure Active Directory](active-directory-conditional-access-azure-portal.md)**  – Toto téma obsahuje vám koncepční přehled podmíněného přístupu a hello související terminologie.

- **[Úvod toodevice správy ve službě Azure Active Directory](device-management-introduction.md)**  – Toto téma poskytuje přehled o hello různé možnosti máte tooconnect zařízení s Azure AD. 


## <a name="trusted-devices"></a>Důvěryhodná zařízení

První mobilní, cloudové první světě Azure Active Directory umožňuje toodevices přihlašování, aplikacím a službám odkudkoli. U některých prostředků ve vašem prostředí, udělení přístupu toohello ti správní uživatelé nemusí být dostatečně dobrý. Kromě toho toohello ti správní uživatelé, můžete také potřebovat toobe důvěryhodné zařízení používá tooaccess prostředku. Ve vašem prostředí, můžete definovat, co je důvěryhodné zařízení na základě hello následující součásti:

- Hello [platformy zařízení](active-directory-conditional-access-azure-portal.md#device-platforms) na zařízení
- Jestli je zařízení kompatibilní
- Jestli je zařízení připojené k doméně 

Hello [platformy zařízení](active-directory-conditional-access-azure-portal.md#device-platforms) je charakterizovaná hello operační systém, který běží na vašem zařízení. V zásadách podmíněného přístupu na základě zařízení můžete omezit přístup toocertain prostředky toospecific zařízení platformy.



V zásadách podmíněného přístupu podle zařízení můžete vyžadovat toobe důvěryhodných zařízení označit jako kompatibilní.

![Cloudové aplikace](./media/active-directory-conditional-access-policy-connected-applications/24.png)

Zařízení může být označen jako kompatibilní v adresáři hello podle:

- Intune 
- Systém správy mobilních zařízení třetích stran, který se integruje se službou Azure AD  

Jenom zařízení, které jsou připojené tooAzure AD může být označen jako kompatibilní. tooconnect zařízení tooAzure služby Active Directory, máte následující možnosti hello: 

- Zaregistrovat Azure AD
- Připojené k Azure AD
- Hybridní připojený k Azure AD

    ![Cloudové aplikace](./media/active-directory-conditional-access-policy-connected-applications/26.png)

Pokud budete mít místní službě Active Directory (AD) nároky, můžete zvážit zařízení, které nejsou připojené tooAzure AD ale připojený k tooyour AD toobe důvěryhodné.

![Cloudové aplikace](./media/active-directory-conditional-access-policy-connected-applications/25.png)


## <a name="next-steps"></a>Další kroky

Před konfigurací zásad podmíněného přístupu na základě zařízení ve vašem prostředí, měli byste podniknout podívejte se na hello [osvědčené postupy pro podmíněný přístup v Azure Active Directory](active-directory-conditional-access-best-practices.md).

