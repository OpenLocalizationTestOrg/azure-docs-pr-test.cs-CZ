---
title: "Jak Azure RemoteApp uložte nastavení a dat uživatele? | Dokumentace Microsoftu"
description: "Zjistěte, jak Azure RemoteApp uloží dat uživatele pomocí disk profilu uživatele."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: e125af62-5c1a-4186-a238-52f537f7bb34
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 4993bf507536659d7951e1552559cf0a6061d345
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-does-azure-remoteapp-save-user-data-and-settings"></a>Jak Azure RemoteApp uložte nastavení a dat uživatele?
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).
> 
> 

Azure RemoteApp uloží identitu uživatele a přizpůsobení v zařízeních a relací. Tato uživatelská data je uložená na disku pro kolekci uživatelů, označované jako disk profilu uživatele (UPD). Disk postupuje podle uživatele a zajistí, že uživatel má konzistentního prostředí, bez ohledu na to, kde přihlášení.

Disky profilu uživatele jsou pro uživatele zcela transparentní – uživatelé ukládají dokumenty do své dokumenty (na co se zdá být místním disku) a změnit jejich nastavení aplikace jako obvykle. Ve stejnou dobu uchování všechna osobní nastavení při připojení k Azure Remoteappu z libovolného zařízení. Všechny, které se uživateli zobrazí je svá data v jednom místě.

Každý UPD má 50GB trvalé úložiště a obsahuje obě nastavení aplikací a dat uživatele. 

Přečtěte si pro konkrétní na data uživatelského profilu.

> [!NOTE]
> Je třeba zakázat UPD? Můžete provést nyní - kontroly se na Pavithra příspěvku na blogu [zakázat disky profilu uživatele (UPD) v Azure Remoteappu](https://blogs.technet.microsoft.com/enterprisemobility/2015/11/11/disable-user-profile-disks-upds-in-azure-remoteapp/), podrobnosti.
> 
> 

## <a name="how-can-an-admin-get-to-the-data"></a>Jak může správce získat k datům?
Pokud potřebujete pro přístup k datům pro jeden z vašich uživatelů (pro zotavení po havárii nebo pokud uživatel odejde ze společnosti), kontaktujte podporu Azure a zadejte informace o odběru pro kolekce a identity uživatele. Tým služby Azure RemoteApp zadejte adresu URL na virtuální pevný disk. Stáhněte si tento virtuální pevný disk a načíst všechny dokumenty nebo soubory, které potřebujete. Upozorňujeme, že virtuální pevný disk se 50GB, takže ho bude chvíli trvat se stáhne.

## <a name="is-the-data-backed-up"></a>Vytvoření zálohy dat?
Ano, uložíme zálohování dat uživatele na zeměpisné umístění. Data jsou jen pro čtení a je přístupný stejným způsobem jako regulární dat by být (kontakt Azure RemoteApp se dá stáhnout), pokud primární datové centrum je vypnutý. Data budou zkopírována do umístění zálohy v reálném čase a nezachovat kopie různé verze. Na poškození dat, tedy jsme nebude možné obnovit do předchozího známého dobrý verze ale pokud primární datové centrum je vypnutý, bude možné získat uživatelská data z jiného umístění.

## <a name="how-do-users-see-the-upd-on-the-server-side"></a>Jak uživatelé uvidí UPD na straně serveru
Každý uživatel bude mít vlastní adresář na serveru, který se mapuje na jejich UPD: c:\Users\username.

## <a name="whats-the-best-way-to-use-outlook-and-upd"></a>Co je nejlepší způsob, jak používat aplikaci Outlook a UPD?
Azure RemoteApp uloží stav aplikace Outlook (poštovních schránek, PST) mezi relacemi. Chcete-li povolit, potřebujeme PST k uložení do data uživatelského profilu (c:\users\<uživatelské jméno >). Toto je výchozí umístění pro data, takže pokud nezměníte umístění, data se uchová mezi relacemi.

Doporučujeme také používat v aplikaci Outlook "v mezipaměti" režim a režim "serveru nebo online" pro vyhledávání.

Podívejte se na [v tomto článku](remoteapp-outlook.md) pro další informace o použití aplikace Outlook s Azure Remoteappem.

## <a name="what-about-redirection"></a>Co o přesměrování?
Můžete nakonfigurovat tak, aby uživatelé přístup k místním zařízením nastavením Azure RemoteApp [přesměrování](remoteapp-redirection.md). Pak bude mít přístup k datům na UPD místní zařízení.

## <a name="can-i-use-my-upd-as-a-network-share"></a>Můžete použít Moje UPD jako sdílené síťové složky?
Ne. UPD nelze použít jako sdílené síťové složky. UPD je k dispozici pro uživatele, pouze pokud je uživatel aktivně připojené k Azure RemoteApp.

## <a name="if-i-delete-a-user-from-a-collection-is-their-upd-deleted"></a>Pokud uživatel odstranit z kolekce, je jejich UPD odstranit?
Když odstraníte uživatele, ne, jsme neodstraňujte automaticky UPD – místo toho jsme ukládání dat do odstranění kolekce. 90 dnů po odstranění kolekce, jsme odstranit všechny UPD. 

Pokud je potřeba odstranit UPD z kolekce, obraťte se na Azure RemoteApp - jsme UPD odstranit z naší straně.

## <a name="can-i-access-my-users-upds-either-current-or-deleted-users"></a>Může přistupovat ke svým uživatelům UPD (aktuální nebo odstraněných uživatelů)?
Ano, obrátíte-li [Azure RemoteApp](mailto:remoteappforum@microsoft.com), jsme můžete nastavit je tak, adresou URL pro přístup k datům. Stažení všech dat a souborů z UPD před vypršením platnosti přístup budete mít asi 10 hodin.

## <a name="are-upds-available-offline"></a>Jsou UPD k dispozici do offline režimu?
Tuto chvíli jsme neposkytuje offline přístup k UPD, nad rámec okně přístupu 10 hodin popsané výše. To znamená, že jsme nyní není k dispozici způsob, jak můžete poskytnout přístup dlouho dostatek dokončit složitější úlohy, jako je antivirový software systémem UPD nebo přístup k datům pro auditování.

## <a name="do-registry-key-settings-persist"></a>Nastavení klíčů registru k zachování?
Ano, nic se zapisují do HKEY_Current_User je součástí UPD.

## <a name="can-i-disable-upds-for-a-collection"></a>Můžete zakázat UPD pro kolekci?
Ano, můžete požádat Azure RemoteApp zakázat UPD pro předplatné, ale nemůže provádět, že sami. To znamená, že UPD bude zakázáno pro všechny kolekce v rámci předplatného.

Můžete chtít zakázat UPD v následujících situacích: 

* Je nutné dokončit přístup a kontrolu nad uživatelská data (pro audit a zkontrolujte účely, například ve finančních institucích).
* Máte 3. stran uživatele profil správy řešení místně a chcete pokračovat v používání je ve vašem nasazení Azure RemoteApp připojený k doméně. Profil agenta má být načten do zlaté image bude vyžadovat. 
* Nepotřebujete žádné místní úložiště dat nebo máte všechna data v cloudu nebo sdílené složky a řídí ukládání dat místně pomocí Azure RemoteApp.

V tématu [zakázat disky profilu uživatele (UPD) v Azure Remoteappu](https://blogs.technet.microsoft.com/enterprisemobility/2015/11/11/disable-user-profile-disks-upds-in-azure-remoteapp/) Další informace.

## <a name="can-i-restrict-users-from-saving-data-to-the-system-drive"></a>Můžete omezit uživatelům v ukládání dat na systémovou jednotku?
Ano, ale budete muset nastavit v image šablony před vytvořením kolekce. Blokovat přístup k adresáři systémové jednotky pomocí následujících kroků:

1. Spustit **gpedit.msc** na image šablony.
2. Přejděte na **konfigurace uživatele > šablony pro správu > součásti systému Windows > Explorer**.
3. Vyberte následující možnosti:
   * **Skrýt tyto jednotky v okně Tento počítač**
   * **Zabránit přístupu do jednotky z tohoto počítače**

## <a name="can-i-seed-upds-i-want-to-put-some-data-in-the-upd-thats-available-the-first-time-the-user-signs-in"></a>Můžete počáteční hodnoty UPD? Chcete uvést některá data do UPD, která je k dispozici při prvním přihlášení uživatele.
Ano, při vytváření image šablony můžete přidat informace do výchozího profilu. Tyto informace se pak přidá do UPD.

## <a name="can-i-change-the-size-of-the-upd-depending-on-how-much-data-i-want-to-store"></a>Můžete změnit velikost UPD v závislosti na množství dat, které chcete uložit?
Ne, všechny UPD mít 50 GB úložiště. Pokud chcete uložit různé objemy dat, zkuste následující postup:

1. Zakážete UPD pro kolekci.
2. Nastavení sdílené složky pro přístup k uživatelům.
3. Načtěte sdílené složky pomocí spouštěcího skriptu. Níže naleznete podrobnosti o spouštění skriptů v Azure Remoteappu.
4. Uživatelé se všechna data uložit do sdílené složky.

## <a name="how-do-i-run-a-startup-script-in-azure-remoteapp"></a>Jak spustit spouštěcí skript v Azure Remoteappu?
Pokud chcete spustit skript, spuštění, začněte vytvořením naplánovanou úlohu v bitové kopii šablony, které chcete použít pro kolekci. (K tomu *před* spustit nástroj sysprep.) 

![Vytvořit systémovou úlohu.](./media/remoteapp-upd/upd1.png)

![Vytvořit systémovou úlohu, která se spouští při přihlášení uživatele](./media/remoteapp-upd/upd2.png)

Na **Obecné** kartě, je nutné změnit **uživatelský účet** v části zabezpečení "BUILTIN\Users."

![Změna uživatelského účtu do skupiny](./media/remoteapp-upd/upd4.png)

Naplánovaná úloha spustí skript spuštění s použitím přihlašovacích údajů uživatele. Naplánování spuštění úlohy každou, čas a uživatel se přihlásí.

![Nastavit aktivační událost pro úlohu jako "přihlášení"](./media/remoteapp-upd/upd3.png)

Můžete také použít [na základě zásad skupiny spouštěcí skripty](https://technet.microsoft.com/library/cc779329%28v=ws.10%29.aspx). 

## <a name="what-about-placing-a-startup-script-in-the-start-menu-would-that-work"></a>Co o umístění spouštěcí skript v nabídce Start? By, funguje?
Jinými slovy lze vytvořit soubor .bat, která spouští skript okno Konfigurace a uložit ho do složky Menu\Programs\StartUp c:\ProgramData\Microsoft\Windows\Start a pak mít skriptu spustit vždy, když uživatel spustí relaci vzdálené aplikace RemoteApp?

Ne, který není podporován s Azure RemoteApp, která používá RDSH, který také nepodporuje spouštěcí skripty v nabídce Start.

## <a name="can-i-use-mstscexe-the-remote-desktop-program-to-configure-logon-scripts"></a>Můžete použít mstsc.exe (program ke vzdálené ploše) ke konfiguraci přihlašovacích skriptů?
Nope které nepodporují Azure RemoteApp.

## <a name="can-i-store-data-on-the-vm-locally"></a>Můžete ukládat data místně do virtuálního počítače?
Data uložená kamkoli do virtuálního počítače než do UPD Ne, budou ztraceny. Je velmi pravděpodobné uživatel nebude získat stejného virtuálního počítače další čas, který se přihlásit k Azure RemoteApp. Jsme neudržují trvalost počítač uživatele tak, aby uživatel nebude přihlášení do stejného virtuálního počítače a data se ztratí. Kromě toho když jsme aktualizace kolekce, stávající virtuální počítače jsou nahrazeny novou sadu virtuálních počítačů -, která znamená, že dojde ke ztrátě všech dat uložených na virtuální počítač. Doporučuje se pro ukládání dat do UPD sdílené úložiště, jako jsou soubory Azure, souborový server uvnitř virtuální sítě, nebo v cloudu pomocí cloudového úložiště systému jako je DropBox.

## <a name="how-do-i-mount-an-azure-file-share-on-a-vm-using-powershell"></a>Jak se připojit Azure sdílené složky na virtuálním počítači, pomocí prostředí PowerShell?
Můžete použít rutinu Net PSDrive připojit jednotku, následujícím způsobem:

    New-PSDrive -Name <drive-name> -PSProvider FileSystem -Root \\<storage-account-name>.file.core.windows.net\<share-name> -Credential :<storage-account-name>


Můžete také uložit přihlašovací údaje tak, že spustíte následující:

    cmdkey /add:<storage-account-name>.file.core.windows.net /user:<storage-account-name> /pass:<storage-account-key>


Který umožňuje přeskočit parametr - Credential v rutině New-PSDrive.

