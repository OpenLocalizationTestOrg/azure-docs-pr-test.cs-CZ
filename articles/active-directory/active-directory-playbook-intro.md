---
title: "aaaAzure Active Directory PoC Playbook Úvod | Microsoft Docs"
description: "Prozkoumejte a rychle implementovat scénáře identita a správa přístupu"
services: active-directory
keywords: "Azure active directory, scénářem, testování konceptu, PoC"
documentationcenter: 
author: dstefanMSFT
manager: asuthar
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 4/12/2017
ms.author: dstefan
ms.openlocfilehash: 655524e9692de46e831fc68e1636e6c20d41958f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-proof-of-concept-playbook-introduction"></a>Ověření služby Azure Active Directory koncept Playbook: Úvod

Tento článek obsahuje pokyny pro možnosti tooexplore různých Azure AD testování konceptu (PoC). Hello zamýšlená cílovou skupinu tohoto dokumentu je Identity architektům, odborníci v oblasti IT a integrátory systémů

## <a name="how-toouse-this-playbook"></a>Jak toouse tento Playbook

1. Použití hello [motiv](active-directory-playbook-ingredients.md#theme) části a vyberte vlastnost hello zájmu na základě potřeb.  
2. Obor hello PoC výběrem hello scénáře, které jsou zarovnané s obchodním cílům. Hello kratší hello lepší. Doporučujeme, abyste to jako krátké a stručným jako hodnota hello možné tooconvey toohello zúčastněným stranám a současně minimalizujete její hello složitost toorealize ho.  
3. Použití hello [implementace](active-directory-playbook-implementation.md) části toounderstand hello scénáře a by jejich význam pro vaše prostředí. V každém scénáři jsme popisují, jak tooset ho nahoru (takzvaný [stavební bloky](active-directory-playbook-building-blocks.md)), a jak toonavigate hello scénáře. 
4. Každý stavebním blokem vysvětluje nezbytných požadavků hello, jakož i toocomplete Přibližná doba. To vám může pomoci při plánování procesu hello. 
5. Podle toho, 1 – 3 výše, definovat hello [prostředí](active-directory-playbook-ingredients.md#environment) v které tooexecute. Doporučujeme toostrive pro produkční prostředí tooget dobrý chování hello prostředí pro uživatele. 
6. Pokud s konfliktní požadavky, použijte tato matice užitečné kompromis 
   * Motiv na střed zobrazení hodnoty  
   * Scénáře plynulost tooprepare, tooset nahoru a tooexecute hello 
   * Minimální doba tooexecute hello cílové scénáře 
   * Jako zavřít tooproduction jako vhodný v rámci vaší omezení 

>[!NOTE]
> V tomto článku zobrazí se některé produkty, které jsou uvedené jako příklady ke zvýšení pohodlí a aplikací konkrétní třetí strany. Azure AD podporuje tisíce aplikace v našem [galerii aplikací](https://azuremarketplace.microsoft.com/marketplace/apps/category/azure-active-directory-apps) , které můžete použít podle konkrétních potřeb a prostředí. 



[!INCLUDE [active-directory-playbook-toc](../../includes/active-directory-playbook-steps.md)]