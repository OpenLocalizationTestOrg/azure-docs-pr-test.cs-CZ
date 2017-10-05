---
title: "Jak používat předplatné služeb Office 365 s Azure Remoteappem | Microsoft Docs"
description: "Zjistěte, jak používat předplatné služeb Office 365 v Azure Remoteappu sdílet aplikace Office."
services: remoteapp
documentationcenter: 
author: piotrci
manager: mbaldwin
ms.assetid: f82b6e23-2b71-47be-846d-fd93f2652f3c
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: e58ee0444517074d6b756652d03fc79c2370e69e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-your-office-365-subscription-with-azure-remoteapp"></a>Jak používat předplatné služeb Office 365 s Azure Remoteappem
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).
> 
> 

Věděli jste, že můžete použít stávající předplatné služeb Office 365 v Azure Remoteappu sdílet aplikace Office z cloudu? Přečtěte si informace o Office 365 + Azure RemoteApp možnosti, včetně odkazy na články o Office 365, které vám pomohou maximálně využít vašeho předplatného.

## <a name="how-do-i-use-office-365-accounts-for-azure-remoteapp"></a>Jak použít účty služeb Office 365 pro Azure RemoteApp?
Podívejte se na nový článek je pro všechny informace: [používání Azure Remoteappu s uživatelských účtů Office 365](remoteapp-o365user.md)

## <a name="can-i-use-my-office-365-subscription-to-run-office-applications-in-azure-remoteapp"></a>Můžete použít Moje předplatné Office 365 používají aplikace Office v Azure Remoteappu?
Ano! Pomocí svého předplatného služeb Office 365 ve skutečnosti je jediný způsob, jak přenést vaše aplikace Office pro Azure RemoteApp.

(Poznámka: Pokud vaše nasazení Azure RemoteApp je doručil partnerský server pro hostování, může být schopen poskytnout Office licencí na základě [služby zprostředkovatele licenční smlouvy](http://www.microsoft.com/en-us/Licensing/licensing-programs/spla-program.aspx))

Skvělé věc o předplatné služeb Office 365 je, že se vám umožní používat stejné uživatelské licence na mnoha různých platforem a prostředí, včetně cloudu Azure. Pokud používáte aplikace Office v Azure Remoteappu nepotřebujete zakoupit další licence nebo nakonfigurujte své licence existující žádným zvláštním způsobem. Stačí je předplatné služeb Office 365, která zahrnuje [Office 365 ProPlus](https://technet.microsoft.com/library/Gg702619.aspx).

Office 365 ProPlus umožňuje [sdílené aktivace počítače](https://technet.microsoft.com/library/Dn782860.aspx) – tato funkce umožňuje dočasné založená na uživateli aktivace pro Office v virtuální a jako cloudová prostředí Azure RemoteApp (a Vzdálená plocha).

Které plány služeb Office 365 obsahovat Office 365 ProPlus? Podívejte se [služby dostupnosti v rámci každé plán](https://technet.microsoft.com/library/office-365-plan-options.aspx) tabulky. Všimněte si, že ne všechny plány obsahovat Office 365 ProPlus (například plán Office 365 Business). Pokud váš plán nemá, zvažte upgradování na plán, který nemá (např. Office 365 Education E3).

## <a name="ok-so-how-are-my-office-365-proplus-licenses-used-with-azure-remoteapp"></a>OK, tak jak jsou moje Office 365 ProPlus licence použít s Azure RemoteApp?
Každá uživatelská licence pro Office 365 ProPlus umožňuje jeden uživatel aktivovat aplikace Office až 5 počítačů plus tabletů a telefonů. Každá aktivace, je registrován s uživatelem, dokud se v zařízení deaktivovat Office. (Uživatelé mohou spravovat svá zařízení v [portál Office 365](https://portal.office365.com/).)

S Azure Remoteappem jeden uživatel může zaprotokolovat do několika počítačů ve stejný den bez porozumění ho. Je to způsobeno službu automaticky spravuje a škáluje prostředky v cloudu, zatímco uživateli se zobrazí jenom aplikace a programy, které jste sdíleli. Pro tento scénář Office 365 ProPlus nabízí režim aktivace sdílený počítač – to znamená, že tento uživatel nemusí dělat žádné Správa licencí pro přístup k tyto prostředky a který jednotlivé počítače není započítává do aktivačního limitu 5 počítače.

Tak dlouho, dokud (správce) přiřadíte Office 365 ProPlus licencí pro vaše uživatele, ale mohli použít Office na svých osobních zařízeních, a také prostřednictvím kolekcí vzdálené aplikace Azure RemoteApp.

## <a name="which-office-applications-can-i-use-with-office-365-and-azure-remoteapp"></a>Aplikace Office, které můžete používat s Office 365 a Azure RemoteApp?
Předplatné služeb Office 365 můžete použít k aktivaci a sdílet Office 2013 v nasazeních Azure Remoteappu. Aktuálně nepodporujeme použití jiné verze systému Office s Azure Remoteappem. To zahrnuje Office 2003, Office 2007, Office 2010 a Office 2016.

## <a name="what-about-visio-pro-or-project-pro"></a>Co o Visio Pro a Project Pro?
Image Office 365 ProPlus, které jsou součástí vašeho předplatného vzdálené aplikace RemoteApp zahrnuje Visio Pro a Project Pro. Ale vaše předplatné Office 365 ProPlus nelze použít k aktivaci těchto programů – každá má své vlastní licenci. Můžete je v aktivovat [portál Office 365](https://portal.office365.com/). 

Nemáte licenci tyto programy, pokud nechcete, aby jejich použití. Pouze aktivujte programy, které chcete použít - a ostatní přeskočit. Budete stále vidět na obrázku, ale nelze je použít. 

## <a name="how-do-i-get-started-with-office-365-and-azure-remoteapp"></a>Jak můžu začít pracovat s Office 365 a Azure RemoteApp?
Nyní když znáte podrobnosti o licencování Office 365, můžeme vám pomůžou začít používat v Azure Remoteappu – je velmi snadné:

Při vytváření kolekcí vzdálené aplikace Azure RemoteApp použít **Office 365 ProPlus (požadován odběr)** bitové kopie.

![Azure RemoteApp bitové kopie s Office 365 Pro Plus](./media/remoteapp-officesubscription/remoteapp-officeimage.png)

Tato bitová kopie obsahuje nejnovější verzi systému Windows Server a Office 365 ProPlus. Po dokončení konfigurace vaší kolekce (včetně publikování aplikace), uživatelé jednoduše Přihlaste se k Azure RemoteApp (s použitím jejich klienta RemoteApp) a zadejte své přihlašovací údaje Office 365 pro všechny aplikace Office. Licence jsou automaticky doručit bez nastavených nebo správu nutné.

## <a name="can-i-create-a-custom-image-with-office-365-proplus"></a>Můžete vytvořit vlastní image s Office 365 ProPlus?
Můžete vytvořit vlastní image pro svoji kolekci, která obsahuje Office 365 ProPlus. Existují 2 volby – použít bitovou kopii Galerie Azure, které poskytujeme nebo můžete vytvořit svoji vlastní image a instalaci Office 365 ProPlus existuje.

### <a name="use-the-azure-gallery-image"></a>Použít bitovou kopii Galerie Azure
Nejjednodušší způsob, jak nasadit do kolekce Office 365 ProPlus je [spustit s jedním z Galerie Azure image](remoteapp-image-on-azurevm.md) součástí vašeho předplatného Azure RemoteApp. Zajistěte, aby si zvolíte **Windows serveru hostitele relace vzdálené plochy s Office 365 ProPlus předinstalovaným** bitové kopie. Nainstalujte všechny ostatní aplikace, které chcete do této bitové kopie a můžete se pustit do práce.

### <a name="use-a-custom-image"></a>Použít vlastní image
Vždy můžete vytvořit vlastní image – můžete vytvořit [virtuálního počítače Azure](remoteapp-image-on-azurevm.md) nebo [vytvořit bitovou kopii místně](remoteapp-create-custom-image.md) a nahrajte ho do Azure. V obou případech zajistěte, aby že instalaci Office 365 ProPlus pomocí uzlu aktivace sdílený počítač. Použití [nástroj pro nasazení Office](http://blogs.technet.com/b/odsupport/archive/2014/07/11/using-the-office-deployment-tool.aspx) a postupujte podle pokynů [pokyny](https://technet.microsoft.com/library/Dn782858.aspx) pro instalaci.  

### <a name="disable-automatic-updates-for-office-365-proplus-in-your-custom-image---important"></a>Zakázat automatické aktualizace Office 365 ProPlus ve vaší vlastní image - důležité
Azure RemoteApp slouží vlastní bitovou kopii jako šablona pro přidání další prostředky jako vyžádání ze zvyšuje vaši uživatelé. Aby se zabránilo zpoždění a problémů s připojením, zakážete automatické aktualizace pro Office v bitové kopii. Pokud ho použít nechcete, pak po spuštění automaticky aktualizuje každých prostředků vytvořené pomocí této šablony. Místo toho používejte standardní proces Azure RemoteApp pro aktualizace vlastní image. Tímto způsobem aktualizace aplikace Office jednou na image šablony a potom nechat Azure RemoteApp postará o získávání aktualizací pro vaše uživatele.

Chcete-li zakázat automatické aktualizace, přidejte následující do konfiguračního souboru nasazení nástroje pro nasazení Office:

        <Updates Enabled="FALSE" />

Konfigurační soubor, takže teď musí obsahovat tyto řádky:

        <Display Level="NONE" AcceptEULA="TRUE" />
        <Property Name="SharedComputerLicensing" Value="1" />
        <Updates Enabled="FALSE" />

## <a name="so-how-can-i-update-an-image-with-office-365-proplus"></a>Jak tedy můžete aktualizovat image s Office 365 ProPlus?
Existuje mnoho důvodů, které chcete aktualizovat image v kolekci. Tady je pár:

* Získat nejnovější aktualizace systému Windows 
* Získat nejnovější aktualizace Office 365 ProPlus aplikace
* Aktualizovat vlastní aplikaci
* Změnit další nastavení konfigurace pro samotné image

Postup začátku do konce pro aktualizaci vaší kolekce použít image můžete aktualizovat je uveden [zde](remoteapp-update.md). Ale informace o tom, jak aktualizovat bitové kopie a Office 365 ProPlus, projděte si následující informace.

Je k dispozici dvě možnosti pro aktualizaci bitové kopie – nahraďte image úplně novým nebo ručně aktualizovat stávající image.

### <a name="replace-your-image-with-the-latest-azure-gallery-image--add-customizations"></a>Nahraďte image bitovou kopii nejnovější Galerie Azure + přidat přizpůsobení
Pomocí této možnosti umožníte Microsoft, postará o aktualizace systému Windows Server a Office 365 ProPlus. Místo aktualizace stávající image, vytvoříte zcela novou bitovou kopii na základě nejnovější bitové kopie galerie. Potom opakujte všechny kroky předtím přizpůsobit image-instalace vlastních aplikací, upravte konfiguraci bitové kopie, atd.

Galerie obrázků jsou pravidelně aktualizovány, takže umístěte easy, zároveň budete vědět, že aplikace pro Windows Server a Office 365 ProPlus jsou aktuální. Samozřejmě o kompromisu je, že budete muset použít vlastní nastavení pokaždé, když získat novou bitovou kopii. Můžete vytvořit skripty pro automatizaci nastavení příslušná přizpůsobení.

### <a name="manually-update-your-existing-image"></a>Ručně aktualizovat stávající image
Pomocí této možnosti použití standardních nástrojů systému Windows při použití aktualizací na bitovou kopii. Pro Office 365 ProPlus pomocí nástroje pro nasazení Office stáhněte a nainstalujte nejnovější aktualizace nebo verze systému Office 365 ProPlus.

> [!IMPORTANT]
> Mějte na paměti, – Office 365 ProPlus automatické aktualizace vypnout.
> 
> 

Potřebujete další informace o používání nástroje pro nasazení Office aktualizací?

* [Nasazení klikněte na tlačítko Spustit produktů Office 365 pomocí nástroje pro nasazení Office](https://technet.microsoft.com/library/JJ219423.aspx)
* [Nasazení a aktualizace Office 365 ProPlus pomocí nástroje pro nasazení Office](https://channel9.msdn.com/Events/Ignite/2015/BRK3168) (video)
* [Nakonfigurujte nastavení aktualizace pro Office 365 ProPlus](https://technet.microsoft.com/library/dn761708.aspx)

