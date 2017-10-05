---
title: "Změna klienta Azure Active Directory v Azure Remoteappu | Microsoft Docs"
description: "Změna klienta Azure Active Directory, které jsou přidružené k Azure RemoteApp"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 20faf169-6e48-428a-8bdd-f231daff19fa
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 7c6c4ded8a11d8399968b2c32aff055d7f3ae5f8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="change-the-azure-active-directory-tenant-in-azure-remoteapp"></a>Změna klienta Azure Active Directory v Azure Remoteappu
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).
> 
> 

Azure RemoteApp používá Azure Active Directory (Azure AD) pro povolení přístupu uživatele. Klientovi pouze Azure AD, který můžete použít v Azure Remoteappu, která je spojená s předplatným služby Azure. Přidružené předplatné můžete zobrazit na **nastavení** na portálu. Podívejte se na **Directory** sloupec **odběry** kartě.

> [!NOTE]
> Než se změna neúspěšné, nejprve odebrat všechny uživatele z existujícího klienta Azure Active Directory ze všech kolekcí Azure Remoteappu. Chcete-li to provést, přejděte na portál Azure, přejděte na **Azure RemoteApp** kartě a otevřete každou kolekci Azure Remoteappu. Přejděte na **uživatelé** kartě a odebrat uživatele, kteří patří ke klientovi aktuální Azure Active Directory. Opakujte pro všechny existující kolekce Azure Remoteappu. Bez v takovém případě nebudete moci vytvořit ani opravovat kolekce.
> 
> 

Pokud chcete použít jiný klienta, změna přidružení k předplatnému pomocí těchto kroků:

1. Na portálu odeberte všechny uživatele Azure AD, ke kterým jste dali přístup do kolekcí Azure Remoteappu. (Viz poznámka výše pokyny o tom, jak to udělat.)
2. Nastavte účet Microsoft (dřív označovaných jako Live ID) jako správce služeb. (Nevíte, pokud již jste správcem služby? Můžete získat kliknutím **Nastavení -> správci**.) Nyní tady je způsob, změny:
   
   1. Klikněte na uživatele v pravém horním rohu a pak klikněte na tlačítko **Zobrazit Moje faktury**.
   2. Klikněte na předplatné. Pak na nové stránce, posuňte se dolů a klikněte na tlačítko **upravit podrobnosti o předplatném** v pravém. (Řazení vpravo, pokud tento střední dole vám pomůže ho najít.)
   3. Zadejte účet Microsoft pro uživatele, který by měl být správce služby.
3. Nyní se odhlásit z portálu a pak se přihlaste zpět pomocí účtu Microsoft, který jste zadali v předchozím kroku.
4. Klikněte na tlačítko **nový -> aplikace služby -> Active Directory -> Directory -> vlastní vytvořit**.
5. V části **Directory**, zvolte **použít existující adresář**. Vytvoříme máte nyní vás odhlásit z portálu, takže zvolte **nyní mě můžete odhlásit**.
6. Přihlaste se znovu do portálu jako globální správce adresáře, který chcete přidat. (Pokud již nebyly jste globální správce, bude po zaokrouhlit z Přihlaste se a přihlaste.)
7. Zobrazí se dotaz, při přihlášení Pokud chcete použít existující klientovi AD s vaším předplatným. Klikněte na tlačítko **pokračovat**a potom klikněte na **Odhlásit**.
8. Přihlaste se zpět znovu a vraťte se zpátky a **Nastavení -> odběry**. Vyberte předplatné a pak klikněte na **upravit adresář**. Vyberte klienta Azure AD, kterou chcete použít.

Můžete teď používání nové Azure AD klienta pro řízení přístupu k předplatnému Azure a ke konfiguraci přístupu uživatele v Azure Remoteappu.

