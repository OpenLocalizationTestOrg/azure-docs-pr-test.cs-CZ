---
title: "Řešení potíží: Chybějící data ve stažených protokolech aktivity služby Azure Active Directory | Dokumentace Microsoftu"
description: "Nabízí řešení pro chybějící data ve stažených protokolech aktivity služby Azure Active Directory."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: ffce7eb1-99da-4ea7-9c4d-2322b755c8ce
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/15/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 3d56f89035da4d1a0074256b165663f81fc2b01e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="i-cant-find-any-data-in-the-azure-active-directory-activity-logs-i-have-downloaded"></a>V stažených protokolech aktivity služby Azure Active Directory nejde najít žádná data


## <a name="symptoms"></a>Příznaky

Ve stažených protokolech aktivity (auditu nebo přihlášení) se nezobrazují žádné záznamy pro zvolený čas. Proč? 

 ![Vytváření sestav](./media/active-directory-reporting-troubleshoot-missing-data-download/01.png)
 

## <a name="cause"></a>Příčina

Když si stáhnete protokoly aktivity na webu Azure Portal, omezujeme rozsah na 120 tisíc záznamů řazených od nejposlednějších. 

## <a name="resolution"></a>Řešení

Můžete využít [rozhraní API pro vytváření sestav Azure AD](active-directory-reporting-api-getting-started.md), abyste načetli až milion záznamů v libovolném časovém okamžiku. Náš doporučený postup je pravidelně spouštět skript, který volá rozhraní API pro vytváření sestav a načítá záznamy přírůstkově v časovém intervalu (například denně nebo týdně).

## <a name="next-steps"></a>Další kroky
Přečtěte si [nejčastější dotazy ke generování sestav v Azure Active Directory](active-directory-reporting-faq.md).

