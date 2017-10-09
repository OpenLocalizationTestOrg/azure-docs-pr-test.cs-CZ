---
title: "aaaHow toocreate hybridní kolekci Azure Remoteappu | Microsoft Docs"
description: "Zjistěte, jak toocreate nasazení vzdálené aplikace RemoteApp, která se připojuje tooyour interní síti."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
editor: 
ms.assetid: 08ea0ce3-3a2c-4ddf-9394-6d75c8030cb1
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 3fba29acc676e0af48e995da406f889c532c44c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-a-hybrid-collection-for-azure-remoteapp"></a>Jak toocreate hybridní kolekci Azure RemoteApp
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.
> 
> 

Existují dva typy kolekcí Azure RemoteApp:

* Cloud: nachází zcela v Azure. Můžete zvolit toosave všechna data v cloudu hello (tak čistě cloudové kolekce) nebo tooconnect tooa vaší kolekce virtuální sítě a uložení dat existuje.   
* Hybridní: obsahuje virtuální síť pro přístup k místní – to vyžaduje hello pomocí služby Azure AD a místní prostředí služby Active Directory.

Nevíte, které potřebujete? Podívejte se na [který typ kolekce potřebujete pro Azure RemoteApp](remoteapp-collections.md).

Tento kurz vás provede procesem vytvoření hybridní kolekce hello. Je osmi kroky:

1. Rozhodněte, [image](remoteapp-imageoptions.md) toouse kolekce. Můžete vytvořit vlastní image nebo použít jednu z imagí Microsoft hello součástí vašeho předplatného.
2. Vytvoření virtuální sítě. Podívejte se na hello [plánování virtuální sítě](remoteapp-planvnet.md) a [sizing](remoteapp-vnetsizing.md) informace.
3. Vytvořte kolekci.
4. Připojení k vaší doméně místní tooyour kolekce.
5. Přidání kolekce tooyour image šablony.
6. Konfigurace synchronizace adresářů. Azure RemoteApp vyžaduje, aby integraci s Azure Active Directory pomocí buď 1) konfigurace Azure Active Directory Sync se hello možnosti synchronizace hesel nebo 2) konfigurace Azure Active Directory Sync bez hello možnost synchronizace hesla ale používáte doménu, která je federované tooAD služby FS. Podívejte se na hello [informace o konfiguraci pro službu Active Directory s Remoteappem](remoteapp-ad.md).
7. Publikování aplikace RemoteApp.
8. Konfigurace přístupu uživatelů.

**Než začnete**

Budete potřebovat následující hello toodo před vytvořením kolekce hello:

* [Zaregistrujte si](https://azure.microsoft.com/services/remoteapp/) pro Azure RemoteApp.
* Vytvořte uživatelský účet ve službě Active Directory toouse hello účet služby Azure RemoteApp. Omezte hello oprávnění pro tento účet tak, aby jej pouze připojit k doméně toohello počítače.
* Shromážděte informace o síti na pracovišti: IP adresa informace a podrobnosti o zařízení VPN.
* Nainstalujte hello [prostředí Azure PowerShell](/powershell/azure/overview) modulu.
* Shromážděte informace o uživatelích hello, které chcete toogrant přístup k. Bude nutné hello hlavní název uživatele Azure Active Directory (například name@contoso.com) pro každého uživatele. Ujistěte se, že hello (UPN) odpovídá mezi službou Azure AD a služby Active Directory.
* Zvolte image šablony. Image šablony Azure Remoteappu obsahuje hello aplikace a programy, které chcete toopublish pro vaše uživatele. V tématu [možnosti image Azure Remoteappu](remoteapp-imageoptions.md) Další informace.
* Chcete image Office 365 ProPlus hello toouse? Přečtěte si informace o [zde](remoteapp-officesubscription.md).
* [Konfigurace služby Active Directory pro RemoteApp](remoteapp-ad.md).

## <a name="step-1-set-up-your-virtual-network"></a>Krok 1: Nastavení virtuální sítě
Můžete nasadit hybridní kolekci, která použije existující virtuální síť Azure, nebo můžete vytvořit novou virtuální síť. Virtuální síť umožňuje uživatelům přistupovat k datům v místní síti prostřednictvím vzdálené aplikace RemoteApp vzdálené prostředky. Pomocí virtuální sítě Azure nabízí vaší kolekce přímého síťového přístupu tooother služby Azure a virtuální počítače nasazené toothat virtuální sítě.

Ujistěte se, zkontrolujte hello [plánování virtuální sítě](remoteapp-planvnet.md) a [VNET velikost](remoteapp-vnetsizing.md) informace předtím, než vytvoříte virtuální síť.

### <a name="create-an-azure-vnet-and-join-it-tooyour-active-directory-deployment"></a>Vytvoření o virtuální síť Azure a připojit ho tooyour nasazení služby Active Directory
Začněte vytvořením [virtuální sítě](../virtual-network/virtual-networks-create-vnet-arm-pportal.md). To se provádí na hello **sítě** ve hello portálu Azure. Je nutné tooconnect vaší virtuální sítě toohello nasazení služby Active Directory, které jsou synchronizované tooyour klienta Azure Active Directory.

V tématu [vytvoření virtuální sítě pomocí portálu Azure hello](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) Další informace.

### <a name="make-sure-your-virtual-network-is-ready-for-azure-remoteapp"></a>Ujistěte se, že virtuální sítě je připraven pro Azure RemoteApp
Před vytvořením kolekce ujistíme, že nové virtuální sítě je připravený. Můžete to ověřit pomocí tohoto postupu hello následující:

1. Vytvořte virtuální počítač Azure uvnitř hello podsíť hello virtuální sítě, který jste právě vytvořili pro vzdálené aplikace RemoteApp.
2. Použití vzdálené plochy tooconnect toohello virtuálního počítače. (Klikněte na tlačítko **připojit**.)
3. Připojení k toohello stejné nasazení služby Active Directory, že chcete toouse pro vzdálenou aplikaci RemoteApp.

Který pracoval? Virtuální síť a podsíť jsou připravené pro Azure RemoteApp!

Můžete najít další informace o vytvoření virtuálních počítačích Azure a připojení toothem pomocí vzdálené plochy [zde](https://msdn.microsoft.com/library/azure/jj156003.aspx).

## <a name="step-2-create-an-azure-remoteapp-collection"></a>Krok 2: Vytvořte kolekci Azure RemoteApp
1. V hello [portál Azure](http://manage.windowsazure.com), přejděte toohello stránky Azure RemoteApp.
2. Klikněte na tlačítko **nový > vytvořit s VNET**.
3. Zadejte název kolekce.
4. Zvolte plán hello, které chcete toouse – standard a basic.
5. Vyberte virtuální síť hello rozevírací seznam a potom podsíť.
6. Zvolte toojoin ho tooyour domény.
7. Klikněte na tlačítko **vytvořit kolekci RemoteApp**.

Po vytvoření kolekce vzdálené aplikace Azure RemoteApp, klikněte dvakrát na název hello hello kolekce. Bude zprovoznit hello **rychlý Start** stránka - Toto je, kde dokončení konfigurace hello kolekce.

Něco být problém? Podívejte se na hello [hybridní kolekci informace o odstraňování potíží](remoteapp-hybridtrouble.md).

## <a name="step-3-link-your-collection-toohello-local-domain"></a>Krok 3: Propojení vaší kolekce toohello místní domény
1. Na hello **rychlý Start** klikněte na tlačítko **připojit k místní doméně**.
2. Přidejte hello Azure RemoteApp doména účtu služby tooyour místní služby Active Directory. Budete potřebovat hello název domény, organizační jednotky, uživatelské jméno účtu služby a heslo.
   
    Toto je hello informace, které jste shromáždili, pokud jste postupovali podle kroků hello v [konfigurace služby Active Directory pro Azure RemoteApp](remoteapp-ad.md).

## <a name="step-4-link-tooan-azure-remoteapp-image"></a>Krok 4: Propojení tooan Azure RemoteApp image
Image šablony Azure Remoteappu obsahuje hello programy, které chcete tooshare s uživateli. Můžete buď vytvořit novou [image šablony](remoteapp-imageoptions.md) nebo odkaz tooan stávající image (jeden již naimportována nebo nahrát tooAzure vzdálené aplikace RemoteApp). Můžete také propojit tooone hello Azure RemoteApp [Image šablony](remoteapp-images.md) obsahující Office 365 nebo Office 2013 (pro zkušební verzi) programy.

Pokud odesíláte hello novou bitovou kopii, je třeba tooenter hello název a vyberte umístění hello hello bitové kopie. Na další stránku hello hello průvodce budete sada rutin prostředí PowerShell - kopírování a spusťte tyto rutiny z zvýšenými oprávněními prostředí Windows PowerShell výzva tooupload hello zadané bitové kopie.

Pokud se připojujete tooan stávající image šablony, jednoduše zadejte název bitové kopie hello, umístění a spojeno předplatné služby Azure.

## <a name="step-5-configure-active-directory-directory-synchronization"></a>Krok 5: Konfigurace synchronizace adresáře služby Active Directory
Azure RemoteApp vyžaduje, aby integraci s Azure Active Directory pomocí buď 1) konfigurace Azure Active Directory Sync se hello možnosti synchronizace hesel nebo 2) konfigurace Azure Active Directory Sync bez hello možnost synchronizace hesla ale používáte doménu, která je federované tooAD služby FS.

Podívejte se na [AD Connect](https://blogs.technet.microsoft.com/enterprisemobility/2014/08/04/connecting-ad-and-azure-ad-only-4-clicks-with-azure-ad-connect/) – Tento článek slouží k nastavení integrace adresáře 4 kroků.

V tématu [synchronizace adresářů: Rozcestník](http://msdn.microsoft.com//library/azure/hh967642.aspx) pro plánování informace a podrobné pokyny.

## <a name="step-6-publish-apps"></a>Krok 6: Publikování aplikace
Aplikace Azure RemoteApp je aplikace hello nebo zadat tooyour uživatelé programu. Nachází se ve hello image šablony, který jste nahráli pro kolekci hello. Když uživatel získá přístup k aplikaci, zobrazí se toorun ve svém místním prostředí, ale je skutečně spuštěna v Azure.

Než uživatelé mohou získat přístup k aplikacím, je třeba toopublish je – díky tomu mají uživatelé přístup hello aplikací pomocí klienta vzdálené plochy hello.

Můžete publikovat více tooyour kolekce aplikací. Na stránce hello publikování, klikněte na tlačítko **publikovat** tooadd aplikace. Můžete buď publikovat z hello **spustit** nabídky image šablony hello nebo zadat cestu hello na image šablony hello aplikace hello. Pokud zvolíte tooadd z hello **spustit** nabídce zvolte tooadd programu hello. Pokud si zvolíte tooprovide hello cesta toohello aplikace, zadejte název aplikace hello a toowhere hello cestu, je nainstalován na image šablony hello.

## <a name="step-7-configure-user-access"></a>Krok 7: Konfigurace přístupu uživatelů
Teď, když jste vytvořili kolekci, je nutné tooadd hello uživatelé mají možnost toouse toobe vzdálené prostředky. Hello uživatelům poskytnout přístup tooneed tooexist v klientovi služby Active Directory hello spojené s předplatným hello používá toocreate tuto kolekci Azure RemoteApp.

1. Na stránce hello rychlý Start, klikněte na tlačítko **konfigurace přístupu uživatelů**.
2. Zadejte hello pracovní účet (ze služby Active Directory) nebo účtu Microsoft, které chcete toogrant přístup.
   
   **Poznámky:**
   
   Ujistěte se, že používáte hello  *user@domain.com*  formátu.
   
   Pokud používáte Office 365 ProPlus v kolekci, musíte použít hello identit služby Active Directory pro vaše uživatele. To pomáhá ověřit licencování.
3. Po ověření uživatele hello, klikněte na **Uložit**.

## <a name="next-steps"></a>Další kroky
Je to – jste úspěšně vytvořili a nasadili hybridní kolekci Azure RemoteApp. dalším krokem Hello je toohave uživatelům stáhnout a nainstalovat klienta vzdálené plochy hello. Hello adresu URL pro klienta hello najdete na stránce Rychlý Start Azure RemoteApp hello. Potom mít uživatelé přihlášení do hello klienta a získat přístup k aplikacím hello, kterou jste publikovali.

### <a name="help-us-help-you"></a>Umožněte nám, abychom vám mohli pomoct
Věděli jste, že v přidání toorating v tomto článku a přidání komentářů pod, článkem můžete provést samotný článek toohello změny? Něco chybí? Něco není v pořádku? Něco je matoucí? Vraťte se na začátek a klikněte na tlačítko **upravit na Githubu** toomake změny – ty, převzato toous ke kontrole, a potom po na nich, uvidíte změny a vylepšení přímo tady.

