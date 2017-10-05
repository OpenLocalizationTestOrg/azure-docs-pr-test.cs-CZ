---
title: "Postup vytvoření cloudové kolekce Azure remoteappu | Microsoft Docs"
description: "Naučte se vytvořit nasazení služby Azure RemoteApp, která ukládá data v cloudu Azure."
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
ms.openlocfilehash: 52d5a073c0de42a77cd7163bf402ed2cdf598d95
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-create-a-cloud-collection-of-azure-remoteapp"></a>Vytvoření cloudové kolekce Azure RemoteAppu
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).
> 
> 

Existují dva typy [kolekcí Azure RemoteAppu](remoteapp-collections.md): 

* Cloud: nachází zcela v Azure. Je možné uložit všechna data v cloudu (tak čistě cloudové kolekce) nebo do kolekce se připojit k virtuální síti a uložení dat existuje.   
* Hybridní: obsahuje virtuální síť pro přístup k místní – to vyžaduje použití služby Azure AD a místní prostředí služby Active Directory.

Tento kurz vás provede procesem vytvoření cloudové kolekce. Existují čtyři kroky: 

1. Vytvořte kolekci Azure RemoteApp.
2. Volitelně můžete nakonfigurujte synchronizaci adresářů. Pokud používáte Azure AD + služby Active Directory, budete muset synchronizovat uživatele, kontakty a hesla ze služby Active Directory v místě ke klientovi Azure AD.
3. Publikování aplikací.
4. Konfigurace přístupu uživatelů.

**Než začnete**

Je třeba provést před vytvořením kolekce následující:

* [Zaregistrujte si](https://azure.microsoft.com/services/remoteapp/) pro Azure RemoteApp. 
* Shromážděte informace o uživatelích, které chcete udělit přístup. To může být buď informace o účtu Microsoft, nebo informace o účtu služby Active Directory práce pro uživatele.
* Tento postup předpokládá, že jste buď chystáte používat jednu z Image šablony zadané v rámci vašeho předplatného nebo že máte již načtený image šablony, kterou chcete použít. Pokud potřebujete nahrajte image různé šablony, můžete to udělat na stránce Image šablony. Stačí kliknout na **nahrajte image šablony** a postupujte podle kroků v průvodci. 
* Chcete použít image Office 365 ProPlus? Přečtěte si informace o [zde](remoteapp-officesubscription.md).
* Chcete zadat vlastní aplikace nebo obchodní programy? Vytvořte novou [image](remoteapp-imageoptions.md) a použít ho v kolekci cloudu.
* Zjistěte, jestli chcete připojit k virtuální síti. Pokud zvolíte možnost připojit k virtuální síti, ujistěte se, splňuje [pokyny pro definování velikosti](remoteapp-vnetsizing.md) a aby [se může připojit k vzdálené aplikace RemoteApp](remoteapp-vnet.md). Podívejte se [článku plánování virtuální sítě ](remoteapp-planvnet.md)Další informace.
* Pokud používáte virtuální síť, rozhodněte, jestli se chcete připojit k místní doméně služby Active Directory.

## <a name="step-1-create-a-cloud-collection---with-or-without-a-vnet"></a>Krok 1: Vytvoření cloudové kolekce - s nebo bez virtuální sítě
Použijte následující kroky, aby **vytvoření cloudové kolekce**:

1. Na portálu management portal přejděte na stránku vzdálené aplikace RemoteApp.
2. Klikněte na tlačítko **nový > rychlé vytvoření**.
3. Zadejte název kolekce a vyberte oblast.
4. Vyberte plán, který chcete použít – standard a basic.
5. Vyberte šablonu, kterou chcete použít pro tuto kolekci. 
   
    **Tip:** vaše předplatné pro vzdálenou aplikaci RemoteApp se dodává s [Image šablony](remoteapp-images.md) obsahující Office 365 nebo Office 2013 (pro zkušební verzi) programy, některé publikované (například Word) a ostatní je připravena k publikování. Můžete také vytvořit novou [image](remoteapp-imageoptions.md) a použít ho v kolekci cloudu.
6. Klikněte na tlačítko **vytvořit kolekci RemoteApp**.
   
    **Důležité:** může trvat až 30 minut se zřídit kolekci.

Po vytvoření kolekce vzdálené aplikace Azure RemoteApp, klikněte dvakrát na název kolekce. Která se zobrazí **rychlý Start** stránka - Toto je, kde dokončíte konfiguraci kolekce.

Pomocí následujících kroků můžete vytvořit **cloudu + kolekce virtuální sítě**:

1. Na portálu management portal přejděte na stránku Azure RemoteApp.
2. Klikněte na tlačítko **nové** > **vytvořit s VNET**.
3. Zadejte název kolekce.
4. Vyberte plán, který chcete použít – standard a basic.
5. Vyberte virtuální síť, jste již vytvořili. Nemusíte vědět, jak to udělat? Prozatím se kroky jsou v [hybridní](remoteapp-create-hybrid-deployment.md) tématu.
6. Rozhodněte, jestli chcete připojit k vaší kolekce k vaší doméně. Pokud ano, budete muset používat AD Connect k integraci služby Azure AD a prostředí služby Active Directory. Který je zahrnutý v níže v **kroku 2**.
7. Klikněte na tlačítko **vytvořit kolekci RemoteApp**.

## <a name="step-2-configure-active-directory-directory-synchronization-optional"></a>Krok 2: Konfigurace synchronizace adresáře služby Active Directory (volitelné)
Pokud chcete používat službu Active Directory, vyžaduje Azure RemoteApp synchronizace adresářů mezi službami Azure Active Directory a služby Active Directory v místě synchronizace uživatele, kontakty a hesla pro vašeho klienta Azure Active Directory. V tématu [konfigurace služby Active Directory pro Azure RemoteApp](remoteapp-ad.md) informace týkající se plánování. Můžete také přejít přímo na [AD Connect](https://blogs.technet.microsoft.com/enterprisemobility/2014/08/04/connecting-ad-and-azure-ad-only-4-clicks-with-azure-ad-connect/) informace.

## <a name="step-3-publish-apps"></a>Krok 3: Publikování aplikace
Aplikace Azure RemoteApp je aplikace nebo program, který zadáte pro vaše uživatele. Nachází se ve image šablony, který jste nahráli pro kolekci. Když uživatel získá přístup k aplikaci, aplikace se zobrazí ke spuštění ve svém místním prostředí, ale skutečně běží ve virtuálním počítači v Azure. 

Než uživatelé mohou získat přístup k aplikacím, je třeba publikovat je – publikování aplikace umožňuje uživatelům přístup k aplikacím prostřednictvím klienta vzdálené plochy.

Vaše kolekce vzdálené aplikace Azure RemoteApp můžete publikovat víc aplikací. Na stránce publikování, klikněte na tlačítko **publikovat** přidání programu. Můžete buď publikovat z **spustit** nabídky Image šablony, nebo zadáním cesty na image šablony pro aplikaci. Pokud zvolíte možnost přidat z **spustit** nabídce vyberte aplikaci, chcete-li publikovat. Pokud zvolíte možnost zadat cestu k aplikaci, zadejte název a cesta k kde je nainstalovaný na image šablony aplikace.

## <a name="step-4-configure-user-access"></a>Krok 4: Konfigurace přístupu uživatelů
Teď, když jste vytvořili kolekci, je nutné přidat uživatele, kteří mají být možné používat vzdálené prostředky. Pokud používáte služby Active Directory, uživatelé, můžete poskytnout přístup k musí existovat v přidružené k předplatnému klienta služby Active Directory, které jste použili k vytvoření této kolekce.

1. Na stránce Rychlý Start klikněte na tlačítko **konfigurace přístupu uživatelů**. 
2. Zadejte pracovní účet (ze služby Active Directory) nebo účtu Microsoft, které chcete udělit přístup.
   
   **Poznámky:** 
   
   Ujistěte se, že používáte  *user@domain.com*  formátu.
   
   Pokud používáte Office 365 ProPlus v kolekci, musíte použít identit služby Active Directory pro vaše uživatele. To pomáhá ověřit licencování. 
3. Po ověření uživatele, klikněte na tlačítko **Uložit**.

## <a name="next-steps"></a>Další kroky
Je to – jste úspěšně vytvořili a nasadili vaší cloudové kolekce Azure Remoteappu. Dalším krokem je vaši uživatelé stáhnout a nainstalovat klienta vzdálené plochy. Můžete získat adresu URL pro klienta na stránce Rychlý Start Azure RemoteApp. Potom mít uživatelé přihlášení do klienta a přístup k aplikacím, které jste publikovali.

### <a name="help-us-help-you"></a>Umožněte nám, abychom vám mohli pomoct
Věděli jste, že kromě hodnocení tohoto článku a přidání komentářů pod článkem také můžete měnit samotný článek? Něco chybí? Něco není v pořádku? Něco je matoucí? Vraťte se na začátek článku, klikněte na **Upravit na GitHubu** a proveďte změny – změny se nám odešlou ke kontrole a po jejich schválení je doplníme do článku.

