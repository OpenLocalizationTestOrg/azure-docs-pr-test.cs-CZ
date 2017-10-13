---
title: "Přiřadit skupiny aplikace Azure AD | Microsoft Docs"
description: "Popisuje, jak implementovat přiřazení skupiny pro aplikace Azure."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
editor: 
ms.assetid: 29b5ba89-a1c7-4f1f-a294-248a40106617
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2017
ms.author: kgremban
ms.custom: H1Hack27Feb2017
robots: noindex
ms.openlocfilehash: e0b0b87a454db96747f024e81882fe83d62fdbe2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="assign-azure-active-directory-groups-to-an-application"></a>Přiřazení skupiny Azure Active Directory k aplikaci
Než uživatelé a skupiny můžete přiřadit k aplikaci, musí vyžadovat přiřazení uživatelů. Další postupy vyžadují přiřazení uživatelů najdete v tématu [nutnosti přiřazení uživatelských](active-directory-applications-guiding-developers-requiring-user-assignment.md) článku.

Tento článek předpokládá, že jste již vytvořili skupiny ve službě active directory, který používáte pro tuto aplikaci.

## <a name="assigning-groups-to-an-application"></a>Přiřazení skupiny k aplikaci
1. Přihlaste se k portálu Azure pomocí účtu správce.
2. Klikněte na **všechny položky** položku v hlavní nabídce.
3. Vyberte adresář, který používáte pro aplikaci.
4. Klikněte na **aplikace** kartě.
5. Vyberte aplikaci ze seznamu aplikací přidružených k tomuto adresáři.
6. Klikněte **uživatele a skupiny** kartě.
7. Filtrování seznamu skupin ve službě active directory pomocí **skupiny** rozevíracího seznamu.
8. Vyberte skupinu.
9. Klikněte na tlačítko **přiřadit**.
10. Klikněte na tlačítko **Ano** po zobrazení výzvy.

## <a name="next-steps"></a>Další kroky
[!INCLUDE [active-directory-applications-guiding-developers-for-lob-applications-toc.md](../../includes/active-directory-applications-guiding-developers-for-lob-applications-toc.md)]
