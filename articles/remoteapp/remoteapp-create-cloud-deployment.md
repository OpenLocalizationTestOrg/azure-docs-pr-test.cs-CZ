---
title: "aaaHow toocreate cloudové kolekce Azure Remoteappu | Microsoft Docs"
description: "Zjistěte, jak hello toocreate nasazení služby Azure RemoteApp, která ukládá data v cloudu Azure."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
editor: 
ms.assetid: 4d7c6956-7e4a-4a41-b7f2-7e5832bf01e3
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: a072ad19d8293016382831d48d0af8e0f5e0d458
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-a-cloud-collection-of-azure-remoteapp"></a>Jak toocreate cloudové kolekce Azure Remoteappu
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.
> 
> 

Existují dva typy [kolekcí Azure RemoteAppu](remoteapp-collections.md): 

* Cloud: nachází zcela v Azure. Můžete zvolit toosave všechna data v cloudu hello (tak čistě cloudové kolekce) nebo tooconnect tooa vaší kolekce virtuální sítě a uložení dat existuje.   
* Hybridní: obsahuje virtuální síť pro přístup k místní – to vyžaduje hello pomocí služby Azure AD a místní prostředí služby Active Directory.

Tento kurz vás provede procesem vytvoření cloudové kolekce hello. Existují čtyři kroky: 

1. Vytvořte kolekci Azure RemoteApp.
2. Volitelně můžete nakonfigurujte synchronizaci adresářů. Pokud používáte Azure AD + služby Active Directory, máte toosynchronize uživatele, kontakty a hesla z klientovi místní služby Active Directory tooyour Azure AD.
3. Publikování aplikací.
4. Konfigurace přístupu uživatelů.

**Než začnete**

Budete potřebovat následující hello toodo před vytvořením kolekce hello:

* [Zaregistrujte si](https://azure.microsoft.com/services/remoteapp/) pro Azure RemoteApp. 
* Shromážděte informace o uživatelích hello, které chcete toogrant přístup k. To může být buď informace o účtu Microsoft, nebo informace o účtu služby Active Directory práce pro uživatele.
* Tento postup předpokládá, že jste buď probíhající toouse jeden z Image šablony hello zadané v rámci vašeho předplatného nebo že máte již načtený chcete toouse image šablony hello. Pokud potřebujete tooupload bitovou kopii jinou šablonu, můžete to udělat z hello Image šablony stránky. Stačí kliknout na **nahrajte image šablony** a postupujte podle kroků hello v Průvodci hello. 
* Chcete image Office 365 ProPlus hello toouse? Přečtěte si informace o [zde](remoteapp-officesubscription.md).
* Chcete tooprovide vlastních aplikací nebo LOB programy? Vytvořte novou [image](remoteapp-imageoptions.md) a použít ho v kolekci cloudu.
* Zjistěte, jestli je potřeba tooconnect tooa virtuální sítě. Pokud si zvolíte tooconnect tooa virtuální síť, ujistěte se, splňuje hello [pokyny pro definování velikosti](remoteapp-vnetsizing.md) a aby [můžete připojit tooRemoteApp](remoteapp-vnet.md). Podívejte se na hello [článku plánování virtuální sítě ](remoteapp-planvnet.md)Další informace.
* Pokud používáte virtuální síť, rozhodnout, zda má toojoin ho tooyour místní domény služby Active Directory.

## <a name="step-1-create-a-cloud-collection---with-or-without-a-vnet"></a>Krok 1: Vytvoření cloudové kolekce - s nebo bez virtuální sítě
Použití hello následující kroky příliš**vytvoření cloudové kolekce**:

1. Na portálu pro správu hello přejděte stránku toohello vzdálené aplikace RemoteApp.
2. Klikněte na tlačítko **nový > rychlé vytvoření**.
3. Zadejte název kolekce a vyberte oblast.
4. Zvolte plán hello, které chcete toouse – standard a basic.
5. Zvolte hello toouse šablonu pro tuto kolekci. 
   
    **Tip:** vaše předplatné pro vzdálenou aplikaci RemoteApp se dodává s [Image šablony](remoteapp-images.md) obsahující Office 365 nebo Office 2013 (pro zkušební verzi) programy, některé publikována (například Word) a dalších připraveni toopublish. Můžete také vytvořit novou [image](remoteapp-imageoptions.md) a použít ho v kolekci cloudu.
6. Klikněte na tlačítko **vytvořit kolekci RemoteApp**.
   
    **Důležité:** může trvat až minut tooprovision too30 vaší kolekce.

Po vytvoření kolekce vzdálené aplikace Azure RemoteApp, klikněte dvakrát na název hello hello kolekce. Bude zprovoznit hello **rychlý Start** stránka - Toto je, kde dokončení konfigurace hello kolekce.

Použití hello následující kroky toocreate **cloudu + kolekce virtuální sítě**:

1. Na portálu pro správu hello přejděte stránku toohello Azure RemoteApp.
2. Klikněte na tlačítko **nové** > **vytvořit s VNET**.
3. Zadejte název kolekce.
4. Zvolte plán hello, které chcete toouse – standard a basic.
5. Zvolte hello jste již vytvořili virtuální sítě. Nevíte, jak toodo který? Prozatím se hello kroky jsou v hello [hybridní](remoteapp-create-hybrid-deployment.md) tématu.
6. Rozhodněte, jestli chcete toojoin doménu tooyour kolekce. Pokud ano, budete potřebovat toouse AD Connect toointegrate Azure AD a prostředí služby Active Directory. Který je zahrnutý v níže v **kroku 2**.
7. Klikněte na tlačítko **vytvořit kolekci RemoteApp**.

## <a name="step-2-configure-active-directory-directory-synchronization-optional"></a>Krok 2: Konfigurace synchronizace adresáře služby Active Directory (volitelné)
Pokud chcete toouse služby Active Directory, vyžaduje Azure RemoteApp synchronizace adresářů mezi službami Azure Active Directory a vaší místní služby Active Directory toosynchronize uživatele, kontakty a klienta Azure Active Directory tooyour hesla. V tématu [konfigurace služby Active Directory pro Azure RemoteApp](remoteapp-ad.md) informace týkající se plánování. Můžete také přejít přímo příliš[AD Connect](https://blogs.technet.microsoft.com/enterprisemobility/2014/08/04/connecting-ad-and-azure-ad-only-4-clicks-with-azure-ad-connect/) informace.

## <a name="step-3-publish-apps"></a>Krok 3: Publikování aplikace
Aplikace Azure RemoteApp je aplikace hello nebo zadat tooyour uživatelé programu. Nachází se ve hello image šablony, který jste nahráli pro kolekci hello. Když uživatel přistoupí aplikaci, aplikace hello zobrazí toorun ve svém místním prostředí, ale ve skutečnosti běží ve virtuálním počítači v Azure. 

Než uživatelé mohou získat přístup k aplikacím, je třeba toopublish je – publikování aplikace umožňuje uživatelům přístup k aplikacím hello prostřednictvím hello klienta vzdálené plochy.

Můžete publikovat víc aplikací tooyour kolekci Azure Remoteappu. Na stránce hello publikování, klikněte na tlačítko **publikovat** tooadd program. Můžete buď publikovat z hello **spustit** nabídky image šablony hello nebo zadat cestu hello na image šablony hello aplikace hello. Pokud zvolíte tooadd z hello **spustit** nabídce zvolte toopublish aplikace hello. Pokud si zvolíte tooprovide hello cesta toohello aplikace, zadejte název aplikace hello a toowhere hello cestu, je nainstalován na image šablony hello.

## <a name="step-4-configure-user-access"></a>Krok 4: Konfigurace přístupu uživatelů
Teď, když jste vytvořili kolekci, je nutné tooadd hello uživatelé mají možnost toouse toobe vzdálené prostředky. Pokud používáte služby Active Directory, hello uživatelům poskytnout přístup tooneed tooexist v klientovi služby Active Directory hello spojené s předplatným hello používá toocreate této kolekce.

1. Na stránce hello rychlý Start, klikněte na tlačítko **konfigurace přístupu uživatelů**. 
2. Zadejte hello pracovní účet (ze služby Active Directory) nebo účtu Microsoft, které chcete toogrant přístup.
   
   **Poznámky:** 
   
   Ujistěte se, že používáte hello  *user@domain.com*  formátu.
   
   Pokud používáte Office 365 ProPlus v kolekci, musíte použít hello identit služby Active Directory pro vaše uživatele. To pomáhá ověřit licencování. 
3. Po ověření jsou uživatelé hello, klikněte na tlačítko **Uložit**.

## <a name="next-steps"></a>Další kroky
Je to – jste úspěšně vytvořili a nasadili vaší cloudové kolekce Azure Remoteappu. dalším krokem Hello je toohave uživatelům stáhnout a nainstalovat klienta vzdálené plochy hello. Hello adresu URL pro klienta hello najdete na stránce Rychlý Start Azure RemoteApp hello. Potom mít uživatelé přihlášení do hello klienta a získat přístup k aplikacím hello, kterou jste publikovali.

### <a name="help-us-help-you"></a>Umožněte nám, abychom vám mohli pomoct
Věděli jste, že v přidání toorating v tomto článku a přidání komentářů pod, článkem můžete provést samotný článek toohello změny? Něco chybí? Něco není v pořádku? Něco je matoucí? Vraťte se na začátek a klikněte na tlačítko **upravit na Githubu** toomake změny – ty, převzato toous ke kontrole, a potom po na nich, uvidíte změny a vylepšení přímo tady.

