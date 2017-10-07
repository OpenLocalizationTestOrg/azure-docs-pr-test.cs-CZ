---
title: "aaaHow podporuje ukládání uživatelských dat a nastavení Azure RemoteApp? | Dokumentace Microsoftu"
description: "Zjistěte, jak Azure RemoteApp uloží dat uživatele pomocí hello disk profilu uživatele."
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
ms.openlocfilehash: dccebc7861e8a0d87cb3ee174f399a2df7fe023c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-does-azure-remoteapp-save-user-data-and-settings"></a>Jak Azure RemoteApp uložte nastavení a dat uživatele?
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.
> 
> 

Azure RemoteApp uloží identitu uživatele a přizpůsobení v zařízeních a relací. Tato uživatelská data je uložená na disku pro kolekci uživatelů, označované jako disk profilu uživatele (UPD). Hello disk následuje hello uživatele a zajišťuje, že má uživatel hello konzistentního prostředí, bez ohledu na to, kde přihlášení.

Disky profilu uživatele jsou uživatele zcela transparentní toohello – uživatelé uložte složku Dokumenty tootheir dokumenty (na co se zobrazí toobe místní disk) a změnit jejich nastavení aplikace jako obvykle. Na hello stejný čas, všechna osobní nastavení zachovat při připojování tooAzure vzdálené aplikace RemoteApp z libovolného zařízení. Všechny hello uživateli se zobrazí se jejich data v hello jednom místě.

Každý UPD má 50GB trvalé úložiště a obsahuje obě nastavení aplikací a dat uživatele. 

Přečtěte si pro konkrétní na data uživatelského profilu.

> [!NOTE]
> Třeba toodisable hello UPD? Můžete provést nyní - kontroly se na Pavithra příspěvku na blogu [zakázat disky profilu uživatele (UPD) v Azure Remoteappu](https://blogs.technet.microsoft.com/enterprisemobility/2015/11/11/disable-user-profile-disks-upds-in-azure-remoteapp/), podrobnosti.
> 
> 

## <a name="how-can-an-admin-get-toohello-data"></a>Jak může správce získat toohello data?
Pokud potřebujete tooaccess hello dat pro jeden z vašich uživatelů (pro zotavení po havárii nebo pokud hello uživatel odejde hello společnosti), kontaktujte podporu Azure a zadejte informace o předplatném hello hello kolekce a hello identitu uživatele. tým služby Azure RemoteApp Hello zadejte adresu URL toohello virtuálního pevného disku. Stáhněte si tento virtuální pevný disk a načíst všechny dokumenty nebo soubory, které potřebujete. Všimněte si, že hello virtuálního pevného disku je 50GB, bude vyžadovat bit toodownload ho.

## <a name="is-hello-data-backed-up"></a>Je hello data zálohovat?
Ano, uložíme zálohu hello uživatelská data na zeměpisné umístění. Hello data jsou jen pro čtení a může být přístupné v hello stejný způsobem, jak budou regulární údaje hello (obraťte se na Azure RemoteApp tooget ho), pokud primární datové centrum hello je vypnutý. Hello data je zkopírovaný v reálném čase toohello umístění zálohy a nezachovat kopie různé verze. Ano, na poškození dat, nebude možné toorestore ho hello tooa dříve známé dobré verze, ale pokud hello primární datové centrum není v provozu, nebudete moct tooget dat uživatele z jiného umístění.

## <a name="how-do-users-see-hello-upd-on-hello-server-side"></a>Jak uživatelé uvidí hello UPD na straně serveru hello
Každý uživatel bude mít vlastní adresář na hello serveru, který mapuje tootheir UPD: c:\Users\username.

## <a name="whats-hello-best-way-toouse-outlook-and-upd"></a>Jaký je nejlepší způsob, jak toouse hello Outlook a UPD?
Azure RemoteApp uloží stav aplikace Outlook hello (poštovních schránek, PST) mezi relacemi. tooenable, budeme potřebovat hello toobe PST uložené v hello data uživatelského profilu (c:\users\<uživatelské jméno >). Toto je hello výchozí umístění pro data hello, takže pokud nezměníte hello umístění, bude uchovávat hello data mezi relacemi.

Doporučujeme také používat v aplikaci Outlook "v mezipaměti" režim a režim "serveru nebo online" pro vyhledávání.

Podívejte se na [v tomto článku](remoteapp-outlook.md) pro další informace o použití aplikace Outlook s Azure Remoteappem.

## <a name="what-about-redirection"></a>Co o přesměrování?
Můžete nakonfigurovat Azure RemoteApp toolet uživatelé přístup k místním zařízením nastavením [přesměrování](remoteapp-redirection.md). Místní zařízení pak bude moct tooaccess hello data na hello UPD.

## <a name="can-i-use-my-upd-as-a-network-share"></a>Můžete použít Moje UPD jako sdílené síťové složky?
Ne. UPD nelze použít jako sdílené síťové složky. UPD je připojen pouze k dispozici toohello uživatel v případě uživatel hello je aktivně tooAzure vzdálené aplikace RemoteApp.

## <a name="if-i-delete-a-user-from-a-collection-is-their-upd-deleted"></a>Pokud uživatel odstranit z kolekce, je jejich UPD odstranit?
Když odstraníte uživatele, ne, jsme neodstraňujte automaticky hello UPD – místo toho jsme ukládání dat hello dokud neodstraníte hello kolekce. 90 dnů po odstranění kolekce hello jsme odstranit všechny UPD. 

Pokud potřebujete toodelete UPD z kolekce, obraťte se na Azure RemoteApp – jsme odstranit UPD z naší straně.

## <a name="can-i-access-my-users-upds-either-current-or-deleted-users"></a>Může přistupovat ke svým uživatelům UPD (aktuální nebo odstraněných uživatelů)?
Ano, obrátíte-li [Azure RemoteApp](mailto:remoteappforum@microsoft.com), jsme můžete nastavit můžete s URL tooaccess hello data. Budete mít o 10 hodin toodownload data ani soubory z hello UPD před vypršením platnosti hello přístup.

## <a name="are-upds-available-offline"></a>Jsou UPD k dispozici do offline režimu?
Tuto chvíli není poskytována tooUPDs přístup v režimu offline, nad rámec okna přístup 10 hodin hello popsané výše. To znamená, že jsme nyní není k dispozici způsob tooprovide, s přístup pro dostatečně dlouhé toocomplete složitěji, úlohy, jako je antivirový software systémem hello UPD nebo přístup k datům pro auditování.

## <a name="do-registry-key-settings-persist"></a>Nastavení klíčů registru k zachování?
Ano, nic zapsána tooHKEY_Current_User je součástí hello UPD.

## <a name="can-i-disable-upds-for-a-collection"></a>Můžete zakázat UPD pro kolekci?
Ano, můžete požádat Azure RemoteApp toodisable UPD pro předplatné, ale nemůže provádět, že sami. To znamená, že pro všechny kolekce v hello předplatné bude zakázáno UPD.

Můžete chtít toodisable UPD v některém z hello následující situace: 

* Je nutné dokončit přístup a kontrolu nad uživatelská data (pro audit a zkontrolujte účely, například ve finančních institucích).
* Máte 3. stran uživatele profil správy řešení místně a chcete toocontinue jejich používání ve vašem nasazení Azure RemoteApp připojený k doméně. To by znamenalo hello profil agenta toobe načtena do hello zlaté image. 
* Nepotřebujete žádné místní úložiště dat nebo máte všechna data v cloudu nebo sdílené složky hello a chcete toocontrol ukládání dat místně pomocí Azure RemoteApp.

V tématu [zakázat disky profilu uživatele (UPD) v Azure Remoteappu](https://blogs.technet.microsoft.com/enterprisemobility/2015/11/11/disable-user-profile-disks-upds-in-azure-remoteapp/) Další informace.

## <a name="can-i-restrict-users-from-saving-data-toohello-system-drive"></a>Můžete omezit uživatelům v ukládání dat toohello systémového disku?
Ano, ale budete potřebovat tooset, který se v šabloně hello bitové kopie, před vytvořením kolekce hello. Použijte následující postup tooblock přístup toohello systémová jednotka hello:

1. Spustit **gpedit.msc** na image šablony hello.
2. Přejděte příliš**konfigurace uživatele > šablony pro správu > součásti systému Windows > Explorer**.
3. Vyberte hello následující možnosti:
   * **Skrýt tyto jednotky v okně Tento počítač**
   * **Zabránit toodrives přístup z tohoto počítače**

## <a name="can-i-seed-upds-i-want-tooput-some-data-in-hello-upd-thats-available-hello-first-time-hello-user-signs-in"></a>Můžete počáteční hodnoty UPD? Chci tooput, který obsahuje data hello UPD, který je k dispozici hello hello nový uživatel se přihlásí.
Ano, když vytvoříte hello image šablony, můžete přidat informace toohello výchozí profil. Tyto informace se pak přidá toohello UPD.

## <a name="can-i-change-hello-size-of-hello-upd-depending-on-how-much-data-i-want-toostore"></a>Můžete změnit velikost hello hello UPD v závislosti na množství dat, které chcete toostore?
Ne, všechny UPD mít 50 GB úložiště. Pokud chcete, aby toostore různé objemy dat, zkuste následující hello:

1. Zakážete UPD pro kolekci hello.
2. Nastavení sdílené složky pro tooaccess uživatele.
3. Zatížení hello sdílené složky pomocí spouštěcího skriptu. Níže naleznete podrobnosti o spouštění skriptů v Azure Remoteappu.
4. Přímé toosave uživatelé všech dat toohello sdílené složky.

## <a name="how-do-i-run-a-startup-script-in-azure-remoteapp"></a>Jak spustit spouštěcí skript v Azure Remoteappu?
Pokud chcete, aby toorun spouštěcí skript, začněte vytvořením naplánovanou úlohu v image šablony hello budete toouse pro kolekci hello. (K tomu *před* spustit nástroj sysprep.) 

![Vytvořit systémovou úlohu.](./media/remoteapp-upd/upd1.png)

![Vytvořit systémovou úlohu, která se spouští při přihlášení uživatele](./media/remoteapp-upd/upd2.png)

Na hello **Obecné** kartě, zda text hello toochange být **uživatelský účet** zabezpečení příliš "BUILTIN\Users."

![Změnit skupinu tooa účtů hello uživatele](./media/remoteapp-upd/upd4.png)

Hello naplánovaná úloha spustí skript spuštění s použitím přihlašovacích údajů uživatele hello. Plánování úkolů toorun hello každých, čas a uživatel se přihlásí.

![Sada hello aktivační událost pro úlohu hello jako "Na protokol na"](./media/remoteapp-upd/upd3.png)

Můžete také použít [na základě zásad skupiny spouštěcí skripty](https://technet.microsoft.com/library/cc779329%28v=ws.10%29.aspx). 

## <a name="what-about-placing-a-startup-script-in-hello-start-menu-would-that-work"></a>Nabídka Start co o uvedení spouštěcí skript hello? By, funguje?
Jinými slovy lze vytvořit soubor .bat, která spouští skript okno Konfigurace a uložte ho toohello c:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp složky a pak mít skriptu spustit vždy, když uživatel spustí relaci vzdálené aplikace RemoteApp?

Ne, který není podporován s Azure RemoteApp, která používá RDSH, který také nepodporuje spouštěcí skripty v nabídce Start hello.

## <a name="can-i-use-mstscexe-hello-remote-desktop-program-tooconfigure-logon-scripts"></a>Můžete použít mstsc.exe (program ke vzdálené ploše hello) tooconfigure přihlašovacích skriptů?
Nope které nepodporují Azure RemoteApp.

## <a name="can-i-store-data-on-hello-vm-locally"></a>Můžou ukládat data do místně hello virtuálního počítače?
Data uložená na hello virtuálních počítačů jinak než v hello UPD kdekoli Ne, budou ztraceny. Je velmi pravděpodobné hello uživatel nebude získat hello stejný virtuální počítač hello příštím se přihlásit k Azure RemoteApp. Jsme neudržují trvalost uživatele virtuálních počítačů, tak nebude přihlaste hello uživatele hello stejného virtuálního počítače a hello data budou ztracena. Kromě toho když jsme se aktualizovat kolekci hello, hello stávající virtuální počítače jsou nahrazeno novou sadu virtuálních počítačů – tzn. všechna data uložená na hello virtuální počítač bude ztracena. Hello doporučuje toostore data v hello UPD, sdílené úložiště, jako jsou soubory Azure, souborový server uvnitř virtuální sítě, nebo v cloudu hello použití úložiště systému cloudové jako je DropBox.

## <a name="how-do-i-mount-an-azure-file-share-on-a-vm-using-powershell"></a>Jak se připojit Azure sdílené složky na virtuálním počítači, pomocí prostředí PowerShell?
Hello Net PSDrive rutiny toomount hello disku, můžete použít takto:

    New-PSDrive -Name <drive-name> -PSProvider FileSystem -Root \\<storage-account-name>.file.core.windows.net\<share-name> -Credential :<storage-account-name>


Můžete také uložit přihlašovací údaje spuštěním hello následující:

    cmdkey /add:<storage-account-name>.file.core.windows.net /user:<storage-account-name> /pass:<storage-account-key>


Který umožňuje přeskočit hello - parametr přihlašovacích údajů v rutině New-PSDrive hello.

