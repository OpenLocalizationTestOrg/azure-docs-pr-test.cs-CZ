---
title: "aaaSetting služby Azure AD Join pro vaše uživatele | Microsoft Docs"
description: "Vysvětluje, jak mohou správci nastavit službu Azure AD Join pro místní adresář a registraci zařízení."
services: active-directory
documentationcenter: 
author: femila
manager: swadhwa
editor: 
tags: azure-classic-portal
ms.assetid: bfc5d415-c918-4d8b-afee-b3f41cc28469
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/16/2017
ms.author: markvi
ms.openlocfilehash: 60a5aeb11292cb6057ab1065c3ab77e5981d0cdb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="setting-up-azure-ad-join-in-your-organization"></a>Nastavení služby Azure AD Join v organizaci
Před nastavením Azure Active Directory Join (Azure AD Join), je nutné tooeither synchronizace registrace vašeho místního adresáře uživatelů toohello cloudu nebo ručně vytvořit spravované účty v Azure AD.

Podrobné pokyny pro synchronizaci tooAzure vaše místní uživatele AD, najdete v článku [integrace místních identit s Azure Active Directory](active-directory-aadconnect.md).

toomanually vytváření a Správa uživatelů ve službě Azure AD, najdete v příliš[Správa uživatelů ve službě Azure AD](https://msdn.microsoft.com/library/azure/hh967609.aspx).

## <a name="set-up-device-registration"></a>Nastavení registrace zařízení
1. Přihlaste se na toohello portálu Azure jako správce.
2. V levém podokně hello vyberte **služby Active Directory**.
3. Na hello **Directory** vyberte adresáře.
4. Vyberte hello **konfigurace** kartě.
5. Přejděte toohello **zařízení** části.
6. Na hello **zařízení** nastavte hello následující:  
   * **MAXIMÁLNÍ počet zařízení na uživatele**: Vyberte hello maximální počet zařízení, která uživatel může mít ve službě Azure AD.  Pokud uživatel dosáhne této kvóty, nebude možné tooadd další zařízení, dokud nejméně jeden z jejich stávajících zařízení se odeberou.
   * **VYŽADOVAT Multi-FACTOR AUTH tooJOIN zařízení**: nastavte, zda uživatelé jsou požadované tooprovide druhé ověření zohlednit toojoin jejich zařízení tooAzure AD. Další informace o ověřování Azure Multi-Factor Authentication, naleznete v části [Začínáme s Azure Multi-Factor Authentication v cloudu hello](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md).
   * **Uživatelé, kteří mohou zařízení připojit k AZURE AD**: Vyberte hello uživatelů a skupin, které jsou povoleny toojoin zařízení tooAzure AD.
   * **ZAŘÍZENÍ připojená k další správci ve službě AZURE AD**: U Azure AD Premium nebo hello Enterprise Mobility Suite (EMS), je možné, kteří uživatelé budou mít oprávnění místního správce toohello zařízení. Globální správci a vlastníci zařízení mají oprávnění místního správce automaticky.

<center>![Nastavení registrace zařízení](./media/active-directory-azureadjoin/active-directory-aadjoin-configure-devices.png)</center>

Po nastavení služby Azure AD Join pro vaši uživatelé mohli připojit tooAzure AD prostřednictvím jejich podnikové nebo osobní zařízení.

Tady jsou tři scénáře hello můžete použít tooenable tooset vaši uživatelé služby Azure AD Join:

* Uživatelé připojení k zařízení ve vlastnictví společnosti přímo tooAzure AD.
* Uživatelé připojení k doméně vlastněných společností zařízení toohello místní služby Active Directory a potom rozšíří hello zařízení tooAzure AD.
* Uživatelé přidat pracovní nebo školní účty tooWindows na osobním zařízení

## <a name="additional-information"></a>Další informace
* [Windows 10 pro podnik hello: způsoby toouse zařízení pro práci](active-directory-azureadjoin-windows10-devices-overview.md)
* [Rozšíření cloudových funkcí tooWindows 10 zařízení prostřednictvím Azure Active Directory Join](active-directory-azureadjoin-user-upgrade.md)
* [Další informace o scénářích použití pro službu Azure AD Join](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Připojení zařízení připojených k doméně tooAzure AD pro prostředí Windows 10](active-directory-azureadjoin-devices-group-policy.md)
* [Nastavení služby Azure AD Join](active-directory-azureadjoin-setup.md)

