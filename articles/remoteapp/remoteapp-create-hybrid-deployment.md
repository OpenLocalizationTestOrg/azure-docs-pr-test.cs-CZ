---
title: "Vytvoření hybridní kolekce pro Azure Remoteappu | Microsoft Docs"
description: "Naučte se vytvořit nasazení vzdálené aplikace RemoteApp, která se připojuje k interní síti."
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
ms.openlocfilehash: 346a5fe3e4011985e4247ceef0d5ca858049fd28
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-create-a-hybrid-collection-for-azure-remoteapp"></a>Postup vytvoření hybridní kolekce pro Azure RemoteApp
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).
> 
> 

Existují dva typy kolekcí Azure RemoteApp:

* Cloud: nachází zcela v Azure. Je možné uložit všechna data v cloudu (tak čistě cloudové kolekce) nebo do kolekce se připojit k virtuální síti a uložení dat existuje.   
* Hybridní: obsahuje virtuální síť pro přístup k místní – to vyžaduje použití služby Azure AD a místní prostředí služby Active Directory.

Nevíte, které potřebujete? Podívejte se na [který typ kolekce potřebujete pro Azure RemoteApp](remoteapp-collections.md).

Tento kurz vás provede procesem vytvoření hybridní kolekce. Je osmi kroky:

1. Rozhodněte, [image](remoteapp-imageoptions.md) pro vaše kolekce. Můžete vytvořit vlastní image nebo použít jednu z imagí Microsoft součástí vašeho předplatného.
2. Vytvoření virtuální sítě. Podívejte se [plánování virtuální sítě](remoteapp-planvnet.md) a [sizing](remoteapp-vnetsizing.md) informace.
3. Vytvořte kolekci.
4. Kolekce připojení k místní doméně.
5. Image šablony přidáte do kolekce.
6. Konfigurace synchronizace adresářů. Azure RemoteApp vyžaduje, integrovat buď 1) konfigurace Azure Active Directory Sync s možností synchronizace hesel nebo 2) konfigurace Azure Active Directory Sync bez možnosti synchronizace hesla ale domény, která je ve službě AD FS Federovaná pomocí Azure Active Directory. Podívejte se [informace o konfiguraci pro službu Active Directory s Remoteappem](remoteapp-ad.md).
7. Publikování aplikace RemoteApp.
8. Konfigurace přístupu uživatelů.

**Než začnete**

Je třeba provést před vytvořením kolekce následující:

* [Zaregistrujte si](https://azure.microsoft.com/services/remoteapp/) pro Azure RemoteApp.
* Vytvoření uživatelského účtu ve službě Active Directory, který bude použit jako účet služby Azure RemoteApp. Omezte oprávnění pro tento účet tak, aby ho pouze připojení počítače k doméně.
* Shromážděte informace o síti na pracovišti: IP adresa informace a podrobnosti o zařízení VPN.
* Nainstalujte [prostředí Azure PowerShell](/powershell/azure/overview) modulu.
* Shromážděte informace o uživatelích, které chcete udělit přístup. Budete potřebovat hlavní název uživatele Azure Active Directory (například name@contoso.com) pro každého uživatele. Ujistěte se, že jména UPN odpovídá mezi službou Azure AD a služby Active Directory.
* Zvolte image šablony. Image šablony Azure Remoteappu obsahuje aplikace a programy, které chcete publikovat pro vaše uživatele. V tématu [možnosti image Azure Remoteappu](remoteapp-imageoptions.md) Další informace.
* Chcete použít image Office 365 ProPlus? Přečtěte si informace o [zde](remoteapp-officesubscription.md).
* [Konfigurace služby Active Directory pro RemoteApp](remoteapp-ad.md).

## <a name="step-1-set-up-your-virtual-network"></a>Krok 1: Nastavení virtuální sítě
Můžete nasadit hybridní kolekci, která použije existující virtuální síť Azure, nebo můžete vytvořit novou virtuální síť. Virtuální síť umožňuje uživatelům přistupovat k datům v místní síti prostřednictvím vzdálené aplikace RemoteApp vzdálené prostředky. Vaše kolekce přímý přístup k síti pomocí virtuální sítě Azure poskytuje k dalším službám Azure a virtuální počítače nasazené do této virtuální sítě.

Ujistěte se, můžete zkontrolovat [plánování virtuální sítě](remoteapp-planvnet.md) a [velikost virtuální síť](remoteapp-vnetsizing.md) informace předtím, než vytvoříte virtuální síť.

### <a name="create-an-azure-vnet-and-join-it-to-your-active-directory-deployment"></a>Vytvoření o virtuální síť Azure a připojte ho k nasazení služby Active Directory
Začněte vytvořením [virtuální sítě](../virtual-network/virtual-networks-create-vnet-arm-pportal.md). To se provádí na **sítě** na portálu Azure. Budete muset připojit virtuální síť k nasazení služby Active Directory, který je synchronizován se ke klientovi Azure Active Directory.

V tématu [vytvoření virtuální sítě pomocí portálu Azure](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) Další informace.

### <a name="make-sure-your-virtual-network-is-ready-for-azure-remoteapp"></a>Ujistěte se, že virtuální sítě je připraven pro Azure RemoteApp
Před vytvořením kolekce ujistíme, že nové virtuální sítě je připravený. Můžete to ověřit následujícím způsobem:

1. Vytvořte virtuální počítač Azure v podsíti virtuální sítě, kterou jste právě vytvořili pro vzdálenou aplikaci RemoteApp.
2. K připojení k virtuálnímu počítači pomocí vzdálené plochy. (Klikněte na tlačítko **připojit**.)
3. Připojte se do stejného nasazení služby Active Directory, kterou chcete použít pro vzdálenou aplikaci RemoteApp.

Který pracoval? Virtuální síť a podsíť jsou připravené pro Azure RemoteApp!

Další informace o vytvoření virtuálních počítačích Azure a připojení můžete najít pomocí vzdálené plochy na je [zde](https://msdn.microsoft.com/library/azure/jj156003.aspx).

## <a name="step-2-create-an-azure-remoteapp-collection"></a>Krok 2: Vytvořte kolekci Azure RemoteApp
1. V [portál Azure](http://manage.windowsazure.com), přejděte na stránku Azure RemoteApp.
2. Klikněte na tlačítko **nový > vytvořit s VNET**.
3. Zadejte název kolekce.
4. Vyberte plán, který chcete použít – standard a basic.
5. Vyberte z rozevíracího seznamu a potom podsíť virtuální sítě.
6. Zvolte připojení k vaší doméně.
7. Klikněte na tlačítko **vytvořit kolekci RemoteApp**.

Po vytvoření kolekce vzdálené aplikace Azure RemoteApp, klikněte dvakrát na název kolekce. Která se zobrazí **rychlý Start** stránka - Toto je, kde dokončíte konfiguraci kolekce.

Něco být problém? Podívejte se [hybridní kolekci informace o odstraňování potíží](remoteapp-hybridtrouble.md).

## <a name="step-3-link-your-collection-to-the-local-domain"></a>Krok 3: Odkaz kolekce k místní doméně
1. Na **rychlý Start** klikněte na tlačítko **připojit k místní doméně**.
2. Přidejte účet služby Azure Remoteappu k místní doméně služby Active Directory. Budete potřebovat název domény, organizační jednotky, uživatelské jméno účtu služby a heslo.
   
    Tyto informace, které jste shromáždili, pokud jste postupovali podle kroků v [konfigurace služby Active Directory pro Azure RemoteApp](remoteapp-ad.md).

## <a name="step-4-link-to-an-azure-remoteapp-image"></a>Krok 4: Propojení na bitovou kopii Azure RemoteApp
Image šablony Azure Remoteappu obsahuje programy, které chcete sdílet s uživateli. Můžete buď vytvořit novou [image šablony](remoteapp-imageoptions.md) nebo proveďte propojení na existující bitové kopie (jeden už importovat nebo nahrán do Azure Remoteappu). Můžete také propojit na jednu z Azure Remoteappu [Image šablony](remoteapp-images.md) obsahující Office 365 nebo Office 2013 (pro zkušební verzi) programy.

Pokud nahráváte nová bitová kopie, musíte zadat název a vyberte umístění pro bitovou kopii. Na další stránce průvodce budete sada rutin prostředí PowerShell - kopírování a spusťte tyto rutiny řádku se zvýšenými oprávněními prostředí Windows PowerShell pro nahrání zadanou bitovou kopii.

Pokud se připojujete k bitovou kopii existující šablony, jednoduše zadejte název bitové kopie, umístění a spojeno předplatné služby Azure.

## <a name="step-5-configure-active-directory-directory-synchronization"></a>Krok 5: Konfigurace synchronizace adresáře služby Active Directory
Azure RemoteApp vyžaduje, integrovat buď 1) konfigurace Azure Active Directory Sync s možností synchronizace hesel nebo 2) konfigurace Azure Active Directory Sync bez možnosti synchronizace hesla ale domény, která je ve službě AD FS Federovaná pomocí Azure Active Directory.

Podívejte se na [AD Connect](https://blogs.technet.microsoft.com/enterprisemobility/2014/08/04/connecting-ad-and-azure-ad-only-4-clicks-with-azure-ad-connect/) – Tento článek slouží k nastavení integrace adresáře 4 kroků.

V tématu [synchronizace adresářů: Rozcestník](http://msdn.microsoft.com//library/azure/hh967642.aspx) pro plánování informace a podrobné pokyny.

## <a name="step-6-publish-apps"></a>Krok 6: Publikování aplikace
Aplikace Azure RemoteApp je aplikace nebo program, který zadáte pro vaše uživatele. Nachází se ve image šablony, který jste nahráli pro kolekci. Když uživatel získá přístup k aplikaci, zobrazí se ke spuštění ve svém místním prostředí, ale je skutečně spuštěna v Azure.

Než uživatelé mohou získat přístup k aplikacím, je třeba publikovat je – díky tomu mají uživatelé přístup k aplikacím prostřednictvím klienta vzdálené plochy.

Můžete publikovat víc aplikací pro vaše kolekce. Na stránce publikování, klikněte na tlačítko **publikovat** přidat aplikaci. Můžete buď publikovat z **spustit** nabídky Image šablony, nebo zadáním cesty na image šablony pro aplikaci. Pokud zvolíte možnost přidat z **spustit** nabídce vyberte program, který chcete přidat. Pokud zvolíte možnost zadat cestu k aplikaci, zadejte název a cesta k kde je nainstalovaný na image šablony aplikace.

## <a name="step-7-configure-user-access"></a>Krok 7: Konfigurace přístupu uživatelů
Teď, když jste vytvořili kolekci, je nutné přidat uživatele, kteří mají být možné používat vzdálené prostředky. Uživatelé, můžete poskytnout přístup k musí existovat v klientovi služby Active Directory přidruženou k odběru, který jste použili k vytvoření této kolekce Azure Remoteappu.

1. Na stránce Rychlý Start klikněte na tlačítko **konfigurace přístupu uživatelů**.
2. Zadejte pracovní účet (ze služby Active Directory) nebo účtu Microsoft, které chcete udělit přístup.
   
   **Poznámky:**
   
   Ujistěte se, že používáte  *user@domain.com*  formátu.
   
   Pokud používáte Office 365 ProPlus v kolekci, musíte použít identit služby Active Directory pro vaše uživatele. To pomáhá ověřit licencování.
3. Jakmile se uživatelé ověřují, klikněte na možnost **Uložit**.

## <a name="next-steps"></a>Další kroky
Je to – jste úspěšně vytvořili a nasadili hybridní kolekci Azure RemoteApp. Dalším krokem je vaši uživatelé stáhnout a nainstalovat klienta vzdálené plochy. Můžete získat adresu URL pro klienta na stránce Rychlý Start Azure RemoteApp. Potom mít uživatelé přihlášení do klienta a přístup k aplikacím, které jste publikovali.

### <a name="help-us-help-you"></a>Umožněte nám, abychom vám mohli pomoct
Věděli jste, že kromě hodnocení tohoto článku a přidání komentářů pod článkem také můžete měnit samotný článek? Něco chybí? Něco není v pořádku? Něco je matoucí? Vraťte se na začátek článku, klikněte na **Upravit na GitHubu** a proveďte změny – změny se nám odešlou ke kontrole a po jejich schválení je doplníme do článku.

