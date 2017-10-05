---
title: "Postup vytvoření vlastního image šablony pro Azure Remoteappu | Microsoft Docs"
description: "Zjistěte, jak vytvořit vlastní image šablony pro Azure RemoteApp. Tuto šablonu můžete použít s hybridní nebo cloudové kolekce."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
editor: 
ms.assetid: b9ec5b51-f7cd-470b-8545-d0fd714c5982
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: f529a5526f266c7dbac3c719b244d05ab7705941
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-create-a-custom-template-image-for-azure-remoteapp"></a>Postup vytvoření vlastního image šablony pro Azure RemoteApp
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).
> 
> 

Azure RemoteApp používá šablony bitovou kopii systému Windows Server 2012 R2 k hostování všechny programy, které chcete sdílet s uživateli. Pokud chcete vytvořit vlastní image šablony vzdálené aplikace RemoteApp, můžete spustit pomocí stávající image nebo vytvořte novou. 

> [!TIP]
> Věděli jste, že můžete vytvořit bitovou kopii z virtuálního počítače Azure? Hodnota TRUE, scénáře a snižuje množství času ho trvá import bitovou kopii. Podívejte se na postup [zde](remoteapp-image-on-azurevm.md).
> 
> 

Požadavky na bitovou kopii, která mohou být nahrány pro použití s Azure Remoteappem jsou:

* Velikost obrázku musí být násobkem MB. Pokud se pokusíte odeslat obrázek, který není přesný více, nahrávání se nezdaří.
* Velikost obrázku musí být 127 GB nebo menší.
* Musí být na soubor virtuálního pevného disku (VHDX soubory [technologie Hyper-V virtuální pevné disky] nejsou aktuálně podporovány.).
* Virtuální pevný disk nesmí být virtuální počítač generace 2.
* Virtuální pevný disk může být buď pevné velikosti, nebo dynamicky se zvětšující. Dynamicky se zvětšující virtuální pevný disk je doporučená, protože trvá méně času k nahrání do Azure, než soubor virtuálního pevného disku pevné velikosti.
* Disk musí být inicializován pomocí hlavní spouštěcí záznam (MBR) oddílů. Stylem oddílů GUID (GPT) není podporována.
* Virtuální pevný disk musí obsahovat jeden instalace systému Windows Server 2012 R2. Může obsahovat několik svazků, ale jenom jeden s obsahuje instalace systému Windows.
* Musí být nainstalována role Hostitel relace vzdálené plochy (RDSH) a funkci Možnosti práce s počítačem.
* Role Zprostředkovatel připojení ke vzdálené ploše musí *není* nainstalovat.
* Musí se zakázat systému souborů EFS (ENCRYPTING File System).
* Obrázek musí být SYSPREPed pomocí parametrů **/oobe / generalize/shutdown** (se nepoužije **/mode:vm** parametr).
* Odesílání svůj disk VHD z řetěz snímku není podporována.

**Než začnete**

Je třeba provést následující před vytvořením služby:

* [Zaregistrujte si](https://azure.microsoft.com/services/remoteapp/) pro vzdálenou aplikaci RemoteApp.
* Vytvoření uživatelského účtu ve službě Active Directory, který bude použit jako účet služby RemoteApp. Omezte oprávnění pro tento účet tak, aby ho pouze připojení počítače k doméně. V tématu [konfigurace Azure Active Directory pro RemoteApp](remoteapp-ad.md) Další informace.
* Shromážděte informace o síti na pracovišti: IP adresa informace a podrobnosti o zařízení VPN.
* Nainstalujte [prostředí Azure PowerShell](/powershell/azure/overview) modulu.
* Shromážděte informace o uživatelích, které chcete udělit přístup. To může být buď informace o účtu Microsoft, nebo informace o účtu služby Active Directory práce pro uživatele.

## <a name="create-a-template-image"></a>Vytvoření image šablony
Toto jsou základní kroky k vytvoření nové image šablony ze začátku:

1. Vyhledejte disk DVD systému Windows Server 2012 R2 Update nebo ISO image.
2. Vytvořte soubor virtuálního pevného disku.
3. Nainstalujte Windows Server 2012 R2.
4. Nainstalujte roli hostitele relace vzdálené plochy (RDSH) a funkci Možnosti práce s počítačem.
5. Nainstalujte další funkce, které vyžadují aplikace.
6. Instalace a konfigurace aplikace. Usnadnění sdílení aplikací, přidejte všechny aplikace nebo programy, které chcete sdílet **spustit** nabídky obrázku, konkrétně v **%systemdrive%\ProgramData\Microsoft\Windows\Start Start\Programy.
7. Proveďte žádné další konfigurace Windows vyžadují aplikace.
8. Zakázání systému souborů EFS (Encrypting File).
9. **REQUIRED:** přejděte do služby Windows Update a nainstalujte všechny důležité aktualizace.
10. Nástroj SYSPREP bitovou kopii.

Podrobné kroky pro vytvoření nové bitové kopie jsou:

1. Vyhledejte disk DVD systému Windows Server 2012 R2 Update nebo ISO image.
2. Vytvořte soubor virtuálního pevného disku pomocí správy disků.
   
   1. Spusťte správu disků (diskmgmt.msc).
   2. Vytvořte dynamicky se zvětšující virtuální pevný disk 40 GB nebo více velikostí. (Odhad množství místa, které jsou potřebné pro Windows, vaše aplikace a přizpůsobení. Windows Server s rolí RDSH a nainstalovanou funkci Možnosti práce s počítačem bude vyžadovat přibližně 10 GB místa).
      
      1. Klikněte na tlačítko **akce > Vytvoření virtuálního pevného disku**.
      2. Zadejte umístění, velikosti a formátu virtuálního pevného disku. Vyberte **dynamicky se zvětšující**a potom klikněte na **OK**.
      
      Tím se spustí na několik sekund. Po dokončení vytvoření virtuálního pevného disku, měli byste vidět nový disk bez jakékoli písmeno jednotky a ve stavu "Nebyla inicializována" v konzole Správa disků.
      
      * Klikněte pravým tlačítkem na disk (ne na volné místo) a pak klikněte na tlačítko **inicializovat Disk**. Vyberte **MBR** (hlavní spouštěcí záznam) jako styl oddílu a pak klikněte na tlačítko **OK**.
      * Vytvořte nový svazek: klikněte pravým tlačítkem na volné místo a pak klikněte na tlačítko **nový jednoduchý svazek**. Můžete přijmout výchozí hodnoty v průvodci, ale ujistěte se, že přiřadit písmeno jednotky, aby se zabránilo potenciální problémy, když nahrajete image šablony.
      * Klikněte pravým tlačítkem na disk a pak klikněte na **odpojit virtuální pevný disk**.
3. Nainstalujte Windows Server 2012 R2:
   
   1. Vytvoření nového virtuálního počítače. Technologie Hyper-V Manager nebo klienta technologie Hyper-v pomocí Průvodce novým virtuálním počítačem.
      1. Na stránce zadat generaci zvolte **generace 1**.
      2. Na stránce připojit virtuální pevný Disk, vyberte **použít existující virtuální pevný disk**a přejděte na virtuální pevný disk jste vytvořili v předchozím kroku.
      3. Na stránce Možnosti instalace vyberte **nainstalovat operační systém ze spouštěcího disku CD/DVD_ROM**a pak vyberte umístění instalačního média systému Windows Server 2012 R2.
      4. Zvolte jiné možnosti v Průvodci potřebné k instalaci systému Windows a aplikací. Dokončete průvodce.
   2. Po dokončení průvodce upravte nastavení virtuálního počítače a proveďte potřebné k instalaci systému Windows a programy, jako je například počet virtuálních procesorů a pak klikněte na tlačítko Další změny **OK**.
   3. Připojit k virtuálnímu počítači a nainstalujte Windows Server 2012 R2.
4. Instalace role Hostitel relace vzdálené plochy (RDSH) a funkci desktopové prostředí:
   1. Spusťte správce serveru.
   2. Klikněte na tlačítko **přidat role a funkce** na úvodní obrazovce nebo z **spravovat** nabídky.
   3. Klikněte na tlačítko **Další** na stránce než začnete.
   4. Vyberte **instalace na základě rolí nebo na základě funkcí**a potom klikněte na **Další**.
   5. Vyberte místní počítač, ze seznamu a pak klikněte na **Další**.
   6. Vyberte **služby Vzdálená plocha**a potom klikněte na **Další**.
   7. Rozbalte položku **uživatelská rozhraní a infrastruktura** a vyberte **možnosti práce s počítačem**.
   8. Klikněte na tlačítko **přidat funkce**a potom klikněte na **Další**.
   9. Na stránce služby Vzdálená plocha, klikněte na **Další**.
   10. Klikněte na tlačítko **hostitel relace vzdálené plochy**.
   11. Klikněte na tlačítko **přidat funkce**a potom klikněte na **Další**.
   12. Na stránce Potvrdit vybrané možnosti instalace vyberte **cílový server automaticky restartovat, pokud je to nutné**a potom klikněte na **Ano** na upozornění na restartování.
   13. Klikněte na **Nainstalovat**. Počítač se restartuje.
5. Nainstalujte další funkce, které vyžadují aplikace, jako je rozhraní .NET Framework 3.5. Chcete-li nainstalovat funkce, spusťte funkce Průvodce přidáním rolí a.
6. Nainstalujte a nakonfigurujte programy a aplikace, kterou chcete publikovat prostřednictvím vzdálené aplikace RemoteApp.

> [!IMPORTANT]
> Nainstalujte roli RDSH před instalací aplikace k zajištění, že jsou všechny problémy s kompatibilitou aplikací zjišťuje před bitovou kopii se nahraje vzdálené aplikace RemoteApp.
> 
> Zkontrolujte, zda zástupce aplikace (**lnk** souboru) se zobrazí v **spustit** nabídky pro všechny uživatele (%systemdrive%\ProgramData\Microsoft\Windows\Start Start\Programy). Také zajistěte, aby na ikonu se zobrazí v **spustit** nabídka je chcete uživatelům zobrazit. V opačném případě ho změnit. (Ho použít nechcete *mít* přidat aplikace pro spuštění nabídce, ale je mnohem jednodušší k publikování aplikace v Remoteappu. Jinak budete muset zadat cestu instalace pro aplikaci, při publikování aplikace.)
> 
> 

1. Proveďte žádné další konfigurace Windows vyžadují aplikace.
2. Zakázání systému souborů EFS (Encrypting File). Spusťte následující příkaz na okno s vyššími oprávněními příkaz:
   
     Chování příkazu fsutil nastavit disableencryption 1
   
   Alternativně můžete nastavit nebo přidejte následující hodnotu DWORD s hodnotou v registru:
   
     HKLM\System\CurrentControlSet\Control\FileSystem\NtfsDisableEncryption = 1
3. Pokud vytváříte image uvnitř virtuálního počítače Azure, přejmenujte  **\%windir%\Panther\Unattend.xml** souborů, jako jsou třeba zablokují se tím skriptu nahrávání později použití v práci. Změňte název tohoto souboru do Unattend.old tak, aby v případě, že budete potřebovat obnovit vaše nasazení bude stále mít soubor.
4. Přejděte do služby Windows Update a nainstalujte všechny důležité aktualizace. Možná budete muset spustit několikrát zobrazíte všechny aktualizace služby Windows Update. (Někdy nainstalujete aktualizaci, a tuto aktualizaci, samotné vyžaduje aktualizace.)
5. Nástroj SYSPREP bitovou kopii. Na příkazovém řádku se zvýšenými oprávněními spusťte následující příkaz:
   
   **C:\Windows\System32\sysprep\sysprep.exe / generalize/oobe / shutdown**
   
   **Poznámka:** nepoužívejte **/mode:vm** přepínač příkaz SYSPREP to i v případě, že toto je virtuální počítač.

## <a name="next-steps"></a>Další kroky
Teď, když máte image vlastní šablony, budete muset nahrát této bitové kopie do kolekcí vzdálené aplikace RemoteApp. K vytvoření kolekce, použijte informace v následujících článcích:

* [Vytvoření hybridní kolekce vzdálené aplikace RemoteApp](remoteapp-create-hybrid-deployment.md)
* [Postup vytvoření cloudové kolekce vzdálené aplikace RemoteApp](remoteapp-create-cloud-deployment.md)

