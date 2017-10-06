---
title: aaaWhat je Azure RemoteApp? | Dokumentace Microsoftu
description: "Zjistěte, jak tooshare aplikacím a prostředkům zařízení tooany prostřednictvím Azure RemoteApp."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
editor: 
ms.assetid: d7a8a311-e70a-4463-ac85-c7f62c500921
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: e13909f3a5698f58c7e4348f3ce53f43560f9b1d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-azure-remoteapp"></a>Co je Azure RemoteApp?
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.
> 
> 

Azure RemoteApp přináší funkce programu Microsoft RemoteApp hello místní zálohován služby Vzdálená plocha, tooAzure hello. Azure RemoteApp pomáhá poskytovat zabezpečený vzdálený přístup tooapplications z mnoha různých uživatelských zařízení. Azure RemoteApp v podstatě hostuje dočasné relace terminálového serveru v cloudu hello a získat toouse je a sdílet je s uživateli.

S Azure RemoteAppem můžete sdílet aplikace a prostředky s uživateli na téměř jakémkoliv zařízení. Vaše aplikace v cloudu hello Hostujeme, což znamená, že se že staráme o hello hardware a škálování toomeet uživatelské požadavky. Všechny máte toodo je nahrávat aplikace hello chcete tooshare a potom získat toouse vaši uživatelé tyto aplikace. [Uživatelé získají tookeep svá vlastní zařízení](remoteapp-clients.md), zatímco vy vše spravujete prostřednictvím hello portálu Azure. I byla možnost hello pomocí svých podnikových přihlašovacích údajů, takže můžete zajistit hello zabezpečení aplikací a dat.

Přečtěte si další informace o Azure RemoteAppu, nebo, pokud jsme vás už přesvědčili, si ho rovnou [vyzkoušejte](https://azure.microsoft.com/services/remoteapp/).

Máte k Azure RemoteAppu otázky? Podívejte se na odpovědi na [časté otázky](remoteapp-faq.md).

Azure RemoteApp je součástí hello [infrastruktury virtuálních klientských počítačů Microsoft](http://www.microsoft.com/server-cloud/products/virtual-desktop-infrastructure/explore.aspx).

**Nové!** Chcete toolearn Další informace o Azure RemoteApp? Nebo připraveni toovalidate Azure RemoteApp ve velkém měřítku? Připojit k našemu týdennímu webináři [požádejte hello odborníky webinář](https://azureinfo.microsoft.com/AzureRemoteAppAskTheExperts-Registration-Page.html?ls=Website).

## <a name="azure-remoteapp-collections"></a>Kolekce Azure RemoteAppu
Existují dva typy [kolekcí Azure RemoteAppu](remoteapp-collections.md):

* A **Cloudová kolekce** je hostována v a ukládá data programů v cloudu hello. Uživatelé mohou získat přístup k aplikacím přihlášením pomocí svého účtu Microsoft nebo podnikových přihlašovacích údajů synchronizovaných nebo federovaných se službou Azure Active Directory.
  
    Cloudovou kolekci zvolte, když chcete tooshare aplikace hello nevyžaduje prostředek připojení tooany privátní síti vaší společnosti (například prostřednictvím zařízení VPN). Pokud aplikace hello používá prostředky na hello Internetu, OneDrive nebo na Azure, bude pro vás fungovat cloudové kolekce. Je také nejrychlejší toocreate hello.
* A **hybridní kolekci** je hostován v a ukládá data v hello cloudu Azure, ale také umožňuje uživatelům přistupovat k datům a prostředkům uloženým v místní síti. Uživatelé mohou získat přístup k aplikacím přihlášením pomocí svých podnikových přihlašovacích údajů synchronizovaných nebo sdružených se službou Azure Active Directory.
  
    Hybridní kolekci zvolte, pokud budete potřebovat připojení tooresources v privátní síti vaší společnosti. Například pokud hello aplikace potřebuje přístup tooone hello následující:
  
  * souborové servery v síti intranet
  * Quicken
  * databáze za bránou firewall
    
    To je obecně vzato užitečnější pro velké společnosti s množstvím prostředků ve svých privátních sítích, které nelze přesunout toohello cloudu.

Hello různých kolekcí nabízejí různé možnosti, včetně sítí, takže zjistěte [kterou kolekci](remoteapp-collections.md) nejlépe vyhovuje. 

### <a name="updating-your-collection"></a>Aktualizování kolekce
Jedním z hello hlavní rozdíly mezi hello hybridními a cloudovými kolekcemi je způsob zpracování aktualizací softwaru. S cloudovou kolekcí, která využívá předinstalovaný image Office 365 ProPlus nebo Office 2013 na hello nemáte tooworry o žádné aktualizace. Služba Hello sama udržuje a vydává aktualizace na průběžně tooboth aplikace a hello operačního systému.

Pro hybridní kolekce, jakož i cloudových kolekcí, které používají vlastní image šablony odpovídáte za údržbu hello image a aplikací. V případě imagů připojených k doméně řídíte aktualizace pomocí nástrojů, jako jsou služba Windows Update, zásady skupiny nebo System Center.

Po aktualizaci vlastního image šablony nahrát hello novou bitovou kopii toohello cloudu Azure a pak aktualizujte hello kolekce toouse hello novou bitovou kopii. (Můžete to provést z hello Azure RemoteApp **rychlý Start** stránky nebo hello řídicího panelu.)

Další informace najdete v článku [Aktualizace kolekce](remoteapp-update.md).

## <a name="supported-remoteapp-clients"></a>Podporované klienty RemoteAppu
Azure RemoteApp je podporován v hello klientských aplikací Remoteappu pro Windows a Windows RT, jakož i hello aplikacích vzdálené plochy Microsoft pro Mac, iOS a Android. Uživatele můžete použít tyto aplikace na svém mobilním nebo výpočetní zařízení tooaccess hello novým programům Azure Remoteappu.

V tématu [přístup k aplikacím v Azure Remoteappu](remoteapp-clients.md) Další informace o klientech hello.

## <a name="next-steps"></a>Další kroky
Nyní si můžete vše vyzkoušet. Určitě to udělejte! Tyto články vám pomůžou začít s Azure RemoteAppem:

* [Jaký typ kolekce potřebujete pro Azure RemoteApp?](remoteapp-collections.md)
* [Vytvoření image Azure RemoteAppu](remoteapp-imageoptions.md)
* [Jak toocreate cloudové kolekce Azure Remoteappu](remoteapp-create-cloud-deployment.md)
* [Jak toocreate hybridní kolekce Azure Remoteappu](remoteapp-create-hybrid-deployment.md)
* [Jak v Azure RemoteAppu funguje licencování?](remoteapp-licensing.md)
* [Osvědčené postupy používání Azure RemoteAppu](remoteapp-bestpractices.md)
* [Časté otázky k Azure RemoteAppu](remoteapp-faq.md)

### <a name="help-us-help-you"></a>Umožněte nám, abychom vám mohli pomoct
Věděli jste, že v přidání toorating v tomto článku a přidání komentářů pod, článkem můžete provést samotný článek toohello změny? Něco chybí? Něco není v pořádku? Něco je matoucí? Vraťte se na začátek a klikněte na tlačítko **upravit na Githubu** nebo **upravit** toomake změny – ty, převzato toous ke kontrole, a potom po na nich, uvidíte změny a vylepšení přímo tady.

