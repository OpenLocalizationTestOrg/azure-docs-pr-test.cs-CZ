---
title: "Řešení potíží: Chybějící data v hello stáhli protokoly aktivity Azure Active Directory | Microsoft Docs"
description: "Poskytuje data toomissing řešení stažené protokolů aktivita služby Azure Active Directory."
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
ms.openlocfilehash: 027b70e6efc570f81d3c836f50ee52aaa89be71a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="i-cant-find-any-data-in-hello-azure-active-directory-activity-logs-i-have-downloaded"></a>Nelze najít v protokolech hello Azure Active Directory aktivity, které staženým žádná data


## <a name="symptoms"></a>Příznaky

Protokoly aktivity hello (auditu nebo přihlášení) byl stažen a všechny záznamy hello se nezobrazí dobu hello, který byl vybrán. Proč? 

 ![Vytváření sestav](./media/active-directory-reporting-troubleshoot-missing-data-download/01.png)
 

## <a name="cause"></a>Příčina

Při stahování protokolů aktivity v hello portál Azure je omezená hello škálování too120K záznamy, seřazené podle většinu poslední. 

## <a name="resolution"></a>Řešení

Můžete využít [rozhraní API pro vytváření sestav Azure AD](active-directory-reporting-api-getting-started.md) toofetch až tooa milionů záznamy v libovolném časovém okamžiku. Naše doporučený postup je toorun skript na základě plánu, který volá rozhraní API toofetch reporting hello zaznamenává přírůstkové způsobem přes v časovém intervalu (například denně nebo týdně).

## <a name="next-steps"></a>Další kroky
V tématu hello [Azure Active Directory, vytváření sestav – nejčastější dotazy](active-directory-reporting-faq.md).

