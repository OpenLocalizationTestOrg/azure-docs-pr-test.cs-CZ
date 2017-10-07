---
title: "aaaAzure Active Directory PoC Playbook přísady | Microsoft Docs"
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
ms.openlocfilehash: 0a7f5cd659b9d62ac86e3c27e5727294d481f4a2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-proof-of-concept-playbook-ingredients"></a>Ověření služby Azure Active Directory koncept Playbook složek 

## <a name="theme"></a>Motiv
Azure AD poskytuje řešení identit a přístupu ve více oblastech ve hello podniku. Jsme klasifikovat hello scénáře v hello následující oblasti: 

* [Velký počet aplikací, jednu identitu](active-directory-playbook-implementation.md#theme---lots-of-apps-one-identity) 
* [Zvýšit zabezpečení](active-directory-playbook-implementation.md#theme---increase-your-security) 
* [Škálování s Self Service](active-directory-playbook-implementation.md#theme---scale-with-self-service) 

Definování motiv tooframe hello PoC pomáhá toofocus hello úsilí, chodem s obchodními cíli, což často jsou aktivační události hello hello zájmu v testování konceptu na prvním místě hello. 

## <a name="environment"></a>Prostředí

Je důležité toodetermine hello podrobnosti hello prostředí, kde bude poskytovat hello testování koncepce. V ideálním případě můžete vytvořit při ji po dokončení testování koncepce hello. je velmi důležitý Hello cílové prostředí a byste měli najít rovnováhu mezi hello mezi provedení ho jako skutečné nejblíže hello režii a omezení nebo další aspekty. Hello typické prostředí pro testovací prostředí jsou:
* **Produkční:** hello scénáře budou provedeny v prostředí za provozu a nasazené cloudové služby společnosti Microsoft (produkční AD, Office 365, Azure AD klienta nebo jednotného přihlašování k řešení). 
* **Uživatel přijetí testování uživateli nebo vývojová prostředí:** máte infrastruktury testovací (paralelní AD a potenciálně Azure AD klienta nebo jednotného přihlašování k řešení) s testovací data, která se podobá produkční. Obvykle hello testovací prostředí je sdílet mezi více projektů v hello enterprise.

Většina scénářů v této příručce jsou sčítání ve své podstatě. V důsledku toho se dá nasadit v hello produkčního klienta bez ovlivnění uživatelé mimo hello PoC. V tomto dokumentu jsme se volání na scénáře, které by neměl mít vliv na úrovni klienta. V takových případech můžete chtít tooconsider mimo produkční prostředí. 


## <a name="target-users"></a>Cíloví uživatelé

Je důležité toodetermine hello cílovou sadu uživatelů, kteří budou uplatňovat hello scénáře, zejména v případě, že prostředí hello je produkčním i testovacím. kategorie Hello cíloví uživatelé pro testování koncepce jsou:
* **Pilotní uživatelé:** skutečné uživatelé v hello prostředí, které budou používat hello řešení s účtem hello používají pro své tooday den úlohy funkce
* **Testovací uživatele:** testovací účty, které jsou vytvořené v prostředí hello 

Většina scénářů v této příručce můžete provést podle uživatelů pilotního nasazení. V tomto dokumentu jsme se být volání si důležité informace o cílové uživatele v případě potřeby.


[!INCLUDE [active-directory-playbook-toc](../../includes/active-directory-playbook-steps.md)]