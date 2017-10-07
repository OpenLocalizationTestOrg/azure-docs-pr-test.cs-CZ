---
title: aaaChange hello klienta Azure Active Directory v Azure Remoteappu | Microsoft Docs
description: "Zjistěte, jak klienta Azure Active Directory hello toochange spojené s Azure Remoteappem"
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
ms.openlocfilehash: d0928b099b7fcfb3ab16077e295d7aaf519c3653
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="change-hello-azure-active-directory-tenant-in-azure-remoteapp"></a>Změna klienta Azure Active Directory hello v Azure Remoteappu
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.
> 
> 

Azure RemoteApp používá přístup uživatele tooallow Azure Active Directory (Azure AD). klienta Hello pouze Azure AD, který můžete použít v Azure Remoteappu je hello jeden přidružené hello předplatného Azure. Hello přidružené předplatné můžete zobrazit na hello **nastavení** stránku hello portálu. Podívejte se na hello **Directory** sloupec hello **odběry** kartě.

> [!NOTE]
> Pro tuto změnu toosucceed nejprve odeberte všechny uživatele z existujícího klienta Azure Active Directory hello ze všech kolekcí Azure Remoteappu. toodo se přejděte toohello portálu Azure, přejděte toohello **Azure RemoteApp** kartě a otevřete každou kolekci Azure Remoteappu. Přejděte toohello **uživatelé** kartě a odebrat uživatele, které patří tooyour aktuální klienta Azure Active Directory. Opakujte pro všechny existující kolekce Azure Remoteappu. Bez v takovém případě nebudete moct toocreate nebo oprava kolekce.
> 
> 

Pokud chcete toouse různých klienta, použijte tyto kroky toochange hello přidružení s vaším předplatným:

1. Hello portálu, odeberte všechny toowhich uživatele Azure AD jste dali přístup tooAzure kolekce vzdálené aplikace RemoteApp. (Viz poznámka hello výše pokyny o tom, toodo to.)
2. Nastavte účet Microsoft (dřív označovaných jako Live ID) jako správce služeb hello. (Nevíte, pokud jste již Dobrý den, Správce služby? Můžete získat kliknutím **Nastavení -> správci**.) Nyní tady je způsob, změny:
   
   1. Klikněte na uživatele hello v pravém horním rohu hello a pak klikněte na tlačítko **Zobrazit Moje faktury**.
   2. Klikněte na předplatné hello. Potom přejděte dolů na novou stránku hello a klikněte na tlačítko **upravit podrobnosti o předplatném** v pravém hello. (Řazení vpravo, pokud tento hello střední dole vám pomůže ho najít.)
   3. Zadejte účet Microsoft hello hello uživatel, který by měl být správce služby hello
3. Nyní se odhlásit z hello portálu a pak se přihlaste zpět pomocí účtu Microsoft, který jste zadali v předchozím kroku hello hello.
4. Klikněte na tlačítko **nový -> aplikace služby -> Active Directory -> Directory -> vlastní vytvořit**.
5. V části **Directory**, zvolte **použít existující adresář**. Vytvoříme toohave toosign mimo portál hello, takže zvolíte **jsem připravené toobe Odhlásit**.
6. Přihlaste se znovu do hello portálu jako globální správce adresáře hello chcete tooadd. (Pokud již nebyly jste globální správce, bude po zaokrouhlit z Přihlaste se a přihlaste.)
7. Zobrazí se dotaz, když se přihlásíte, pokud chcete stávající klientovi služby AD, toouse s vaším předplatným. Klikněte na tlačítko **pokračovat**a potom klikněte na **Odhlásit**.
8. Přihlaste se zpět znovu a vraťte se příliš**Nastavení -> odběry**. Vyberte předplatné a pak klikněte na **upravit adresář**. Vyberte, které chcete toouse klienta hello Azure AD.

Teď můžete použít hello nové Azure AD klienta toocontrol přístup toohello Azure předplatného a tooconfigure přístup uživatele v Azure Remoteappu.

