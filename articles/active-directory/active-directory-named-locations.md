---
title: "aaaNamed umístění v Azure Active Directory | Microsoft Docs"
description: "Konfigurace s názvem umístění, nemusíte mít IP adresy, které jsou vlastněny organizaci generovat falešně pozitivních pro umístění tooatypical Neuskutečnitelná cesta hello riziko typ události."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: f56e042a-78d5-4ea3-be33-94004f2a0fc3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 591e4b94b2ec9d45e20c01711e922f9972e047e5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="named-locations-in-azure-active-directory"></a>Pojmenované umístění v Azure Active Directory

S hello s názvem umístění funkce služby Azure Active Directory můžete označit důvěryhodné rozsahy IP adres ve vaší organizace. Ve vašem prostředí, můžete použít s názvem umístění v kontextu hello hello zjišťování [rizik události](active-directory-reporting-risk-events.md). Funkce Hello pomáhá snižovat hello počet hlášených falešně pozitivních pro hello *Neuskutečnitelná cesta tooatypical umístění* rizik typ události. 

## <a name="configuration"></a>Konfigurace

tooconfigure s názvem umístění:

1. Přihlaste se toohello [portál Azure](https://portal.azure.com) jako globální správce.

2. V levém podokně hello, klikněte na **Azure Active Directory**.

    ![v levém podokně hello Hello odkaz Azure Active Directory](./media/active-directory-named-locations/01.png)

3. Na hello **Azure Active Directory** okno, v hello **zabezpečení** klikněte na tlačítko **podmíněného přístupu**.

    ![Hello příkaz podmíněného přístupu](./media/active-directory-named-locations/05.png)


4. Na hello **podmíněného přístupu** okno, v hello **spravovat** klikněte na tlačítko **s názvem umístění**.

    ![Hello pojmenované umístění příkaz](./media/active-directory-named-locations/06.png)


5. Na hello **s názvem umístění** okně klikněte na tlačítko **nové umístění**.

    ![Hello nový příkaz umístění](./media/active-directory-named-locations/07.png)


6. Na hello **nový** okně hello následující:

    ![Hello nové okno](./media/active-directory-named-locations/08.png)

    a. V hello **název** zadejte název pro vaše s názvem umístění.

    b. V hello **rozsahy IP adres** zadejte rozsah adres IP. rozsah IP Hello musí toobe v hello *směrování mezi doménami* formátu (CIDR).  

    c. Klikněte na možnost **Vytvořit**.



## <a name="what-you-should-know"></a>Důležité informace

**Hromadné aktualizace**: při vytváření nebo aktualizaci s názvem umístění pro hromadné aktualizace, můžete nahrát nebo stáhnout soubor CSV s rozsahy IP hello. Nahrávaný přidá hello rozsahy IP adres v seznamu toohello hello souboru místo přepsání hello seznamu.

![Hello nahrávání a stahování odkazy](./media/active-directory-named-locations/09.png)


**Omezení**: můžete definovat nesmí být delší než 60 s názvem umístění, s jeden rozsah přiřazené tooeach IP z nich. Pokud máte pouze jednu s názvem umístění nakonfigurovaný, můžete definovat až too500 rozsahy IP adres pro ni.


## <a name="next-steps"></a>Další kroky

toolearn Další informace o rizikových událostech, najdete v části [Azure Active Directory rizikových událostech](active-directory-reporting-risk-events.md).

