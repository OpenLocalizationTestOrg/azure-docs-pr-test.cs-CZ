---
title: "aaaRun libovolné aplikace pro Windows v jakémkoliv zařízení s Azure Remoteappem | Microsoft Docs"
description: "Zjistěte, jak tooshare libovolné aplikace pro Windows s uživateli pomocí Azure Remoteappu."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
editor: 
ms.assetid: 961d40ca-9673-4977-aa54-d6b22fc61ce1
ms.service: remoteapp
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: compute
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 750f3b881e0cb671ff6e8f3e851bccdf2262d156
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="run-any-windows-app-on-any-device-with-azure-remoteapp"></a>Spuštění libovolné aplikace pro Windows v jakémkoliv zařízení s Azure RemoteAppem
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.
> 
> 

Aplikaci pro Windows můžete hned teď spustit kdekoliv a v jakémkoliv zařízení. Neděláme si srandu – stačí použít Azure RemoteApp. Ať už se jedná vlastní aplikaci napsanou před 10 lety, nebo aplikaci Office, uživatelé nebudou mít toobe svázané tooa konkrétní operační systém (např. Windows XP) aby těchto pár aplikací.

S Azure Remoteappem vaši uživatelé můžete také použít vlastní Android nebo zařízení Apple a získání hello stejné prostředí jako v systému Windows (nebo na zařízení Windows Phone). Toho se dosáhne hostováním aplikací pro Windows v kolekci virtuálních počítačů Windows ve službě Azure – uživatelé k nim budou mít přístup odkudkoliv, kde je připojení k internetu. 

Přečtěte si příklad přesně jak toodo to.

V tomto článku vytvoříme tooshare přístup ke všem uživatelům. Můžete však použít LIBOVOLNOU aplikaci. Také můžete nainstalovat aplikace v počítači Windows Server 2012 R2, můžete pomocí následujících kroků hello sdílet. Můžete zkontrolovat hello [požadavky aplikací na](remoteapp-appreqs.md) toomake, že vaše aplikace bude fungovat.

Prosím Poznámka, protože Access je databázová aplikace a chceme toobe této databáze užitečná, provedeme několik kroků navíc toolet uživatelé přístup ke sdílení dat Accessu hello. Pokud vaše aplikace není databázová nebo nepotřebujete mít tooaccess vaše uživatele toobe sdílené složky, můžete přeskočit tyto kroky v tomto kurzu

> [!NOTE]
> <a name="note"></a>Je třeba účtu Azure toocomplete v tomto kurzu:
> 
> * Můžete [zdarma otevřít účet Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F): získáte kredity, můžete použít tootry na placené služby Azure a i po jejich použití až můžete hello účet ponechat a používat bezplatné služby Azure, jako jsou weby. Platební karty nikdy odečte, není-li explicitně změnit nastavení a požádejte toobe účtovat.
> * Můžete si [aktivovat výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F): Vaše předplatné MSDN vám každý měsíc dává kredity, které můžete použít k placení za služby Azure.
> 
> 

## <a name="create-a-collection-in-remoteapp"></a>Vytvoření kolekce v RemoteAppu
Začněte vytvořením kolekce. Hello kolekce slouží jako kontejner pro vaše aplikace a uživatele. Každá kolekce je založena na imagi – můžete vytvořit vlastní nebo použít image ve vašem předplatném. V tomto kurzu používáme image zkušební hello Office 2013 – obsahuje aplikace hello, že má být tooshare.

1. V hello portálu Azure přejděte dolů v hello levém navigační stromu do části RemoteApp. Otevřete danou stránku.
2. Klikněte na tlačítko **Vytvořit kolekci RemoteApp**.
3. Klikněte na tlačítko **Rychlé vytvoření** a zadejte název kolekce.
4. Vyberte oblast hello chcete toouse toocreate vaší kolekce. Pro hello co nejlepších výsledků vyberte hello oblast, která je geograficky nejblíže toohello umístění, kde budou vaši uživatelé přístup k aplikaci hello. Například v tomto kurzu hello uživatelé budou nacházet v Redmond, Washington. nejbližší oblast Azure Hello je **západní USA**.
5. Vyberte hello fakturační plán, že který má toouse. Hello základním fakturačním plánu používá 16 uživatelů na velký virtuální počítač Azure, zatímco hello standardním fakturačním plánu používá 10 uživateli na velký virtuální počítač Azure. Jako v příkladu obecné základní plán hello dobře funguje pro pracovní postupy zadávání data. Pro kancelářských aplikací, jako je Office je vhodnější standardní plán hello.
6. Nakonec vyberte image Office 2013 Professional hello. Tento image obsahuje aplikace Office 2013. Jen upozorníme, že tento image je vhodný pouze pro zkušební kolekce a testovací prostředí. Nemůžete ho používat v produkční kolekci.
7. Nyní klikněte na **Vytvořit kolekci RemoteApp**.

![Vytvoření cloudové kolekce v RemoteAppu](./media/remoteapp-anyapp/ra-anyappcreatecollection.png)

Spustí se vytváření kolekce, ale může to trvat až hodinu tooan.

Nyní jste se připravené tooadd vaši uživatelé.

## <a name="share-hello-app-with-users"></a>Aplikace hello sdílení s uživateli
Po úspěšném vytvoření kolekce, je čas toopublish přístup toousers a přidání hello uživatelů, kteří mají mít přístup tooit.

Pokud jste přešli z uzlu Azure Remoteappu hello při vytváření kolekce hello, začněte tím, že váš způsob zálohování tooit z hello domovské stránky Azure.

1. Klikněte na tlačítko hello kolekce, které jste vytvořili starší tooaccess další možnosti a nakonfigurujte hello kolekce.
   ![Nová cloudová kolekce RemoteApp](./media/remoteapp-anyapp/ra-anyappcollection.png)
2. Na hello **publikování** , klikněte na **publikovat** v dolní části hello úvodní obrazovka, a pak klikněte na **programy v nabídce Start publikování**.
   ![Publikování aplikace RemoteAppu](./media/remoteapp-anyapp/ra-anyapppublish.png)
3. Vyberte hello aplikace, které mají toopublish hello seznamu. Pro naše účel jsme zvolili Access. Klikněte na **Dokončit**. Počkejte, než pro publikování toofinish aplikace hello.
   ![Publikování aplikace Access v RemoteAppu](./media/remoteapp-anyapp/ra-anyapppublishaccess.png)
4. Po dokončení publikování aplikace hello zamiřte toohello **přístup uživatelů** kartě tooadd všechny hello uživatele, kteří potřebují přístup k aplikacím tooyour. Zadejte uživatelská jména (e-mailové adresy) a potom klikněte na **Uložit**.

![Přidat uživatele tooRemoteApp](./media/remoteapp-anyapp/ra-anyappaddusers.png)

1. Nyní je čas tootell uživatelům o těchto nových aplikacích a jak tooaccess je. toodo to, že uživatelům pošlete e-mail s odkazem toohello adresa URL stažení klienta vzdálené plochy.
   ![stažení klientského Hello adresu URL pro vzdálené aplikace RemoteApp](./media/remoteapp-anyapp/ra-anyappurl.png)

## <a name="configure-access-tooaccess"></a>Konfigurace přístupu tooAccess
Některé aplikace po nasazení prostřednictvím RemoteAppu vyžadují další konfiguraci. Konkrétně pro přístup, jsme toocreate probíhající sdílenou v Azure, každý uživatel může přistupovat. (Pokud nechcete, aby toodo, že můžete vytvořit [hybridní kolekci](remoteapp-create-hybrid-deployment.md) [namísto naší cloudové kolekce], která uživatelům umožňuje přístup k souborům a informacím v místní síti.) Potom budeme potřebovat tootell naši uživatelé toomap místní jednotky na jejich počítač toohello systém souborů Azure.

Hello první část, které jako dobrý den, správce provést. Potom máme několik kroků pro vaše uživatele.

1. Spusťte rozhraním publikování hello příkazový řádek (cmd.exe). V hello **publikování** vyberte **cmd**a potom klikněte na **publikovat > publikovat program pomocí cesty**.
2. Zadejte název hello hello aplikace a cestu hello. Pro naše účel použijte jako název hello a "% SYSTEMDRIVE%\windows\explorer.exe" jako cestu hello "Průzkumník souborů".
   ![Publikování souboru cmd.exe hello.](./media/remoteapp-anyapp/ra-publishcmd.png)
3. Nyní je třeba toocreate Azure [účet úložiště](../storage/common/storage-create-storage-account.md). "Accessstorage", náš jsme pojmenovali vyberte název, který je smysluplný tooyou. (toomisquote unikátní, může existovat pouze jedna "accessstorage".) ![Náš účet úložiště Azure](./media/remoteapp-anyapp/ra-anyappazurestorage.png)
4. Nyní přejděte zpět tooyour řídicí panel, kde získáte hello cesta tooyour úložiště (umístění koncového bodu). Za chvilku ji budete potřebovat, takže si ji někam zkopírujte.
   ![Cesta k účtu úložiště Hello](./media/remoteapp-anyapp/ra-anyappstoragelocation.png)
5. Po vytvoření účtu úložiště hello dále musíte hello primární přístupový klíč. Klikněte na tlačítko **spravovat přístupové klíče**a potom kopie hello primární přístupový klíč.
6. Nyní nastavit kontext hello hello účtu úložiště a vytvořit nové sdílené složky pro přístup. Spusťte následující rutiny v okno prostředí Windows PowerShell se zvýšenými oprávněními hello:
   
        $ctx=New-AzureStorageContext <account name> <account key>
        $s = New-AzureStorageShare <share name> -Context $ctx
   
    Proto případě naší sdílené složky, ty jsou rutiny hello, spustíme:
   
        $ctx=New-AzureStorageContext accessstorage <key>
        $s = New-AzureStorageShare <share name> -Context $ctx

Nyní je zapnout hello uživatele. Nejdříve nechte uživatele nainstalovat [klienta RemoteApp](remoteapp-clients.md). V dalším kroku hello uživatelé potřebovat toomap jednotku ze svého účtu toothat Azure file sdílet, kterou jste vytvořili a přidat své soubory Access. Udělají to takto:

1. V klientovi RemoteApp hello přístup hello publikované aplikace. Spusťte hello cmd.exe program.
2. Spusťte následující příkaz toomap hello jednotku ze sdílené složky toohello počítače:
   
        net use z: \\<accountname>.file.core.windows.net\<share name> /u:<user name> <account key>
   
    Pokud nastavíte hello **/ persistent** parametr tooyes hello mapovaná jednotka se zachová napříč relacemi.
3. Nyní spusťte aplikaci Průzkumník souborů hello ze vzdálené aplikace RemoteApp. Zkopírujte všechny soubory Accessu chcete toouse toohello hello sdílené aplikaci pro sdílení souborů.
   ![Umístění souborů Accessu do sdílené složky Azure](./media/remoteapp-anyapp/ra-anyappuseraccess.png)
4. Nakonec otevřete Access a pak otevřete hello databáze, kterou jste právě Nasdíleli. Měli byste vidět vaše data v Access spuštěná z cloudu hello.
   ![Skutečná databáze Access spuštěná z cloudu hello](./media/remoteapp-anyapp/ra-anyapprunningaccess.png)

Nyní můžete Access používat v libovolném zařízení – stačí si jen nainstalovat klienta RemoteApp.

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps"></a>Další kroky
Teď, když jste zvládli vytvoření kolekce, zkuste vytvořit [kolekci používající Office 365](remoteapp-tutorial-o365anywhere.md). Nebo můžete vytvořit [hybridní kolekci ](remoteapp-create-hybrid-deployment.md), která má přístup k místní síti.

<!--Image references-->

