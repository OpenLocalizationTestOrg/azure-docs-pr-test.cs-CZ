---
title: "aaaHow toocreate image vlastní šablony pro Azure Remoteappu | Microsoft Docs"
description: "Zjistěte, jak obrázků toocreate vlastní šablony pro Azure RemoteApp. Tuto šablonu můžete použít s hybridní nebo cloudové kolekce."
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
ms.openlocfilehash: 9e3a0b2d3cc56177ea51406e6cecfe19ff235340
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-a-custom-template-image-for-azure-remoteapp"></a>Jak obrázků toocreate vlastní šablony pro Azure RemoteApp
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.
> 
> 

Azure RemoteApp používá všechny hello programy, které chcete tooshare s uživateli toohost image šablony Windows Server 2012 R2. toocreate vlastní image šablony vzdálené aplikace RemoteApp, můžete spustit pomocí stávající image nebo vytvořte novou. 

> [!TIP]
> Věděli jste, že můžete vytvořit bitovou kopii z virtuálního počítače Azure? Hodnota TRUE, scénáře a snižuje hello množství času se trvá tooimport hello image. Podívejte se na postup hello [zde](remoteapp-image-on-azurevm.md).
> 
> 

požadavky na Hello hello image, která mohou být nahrány pro použití s Azure Remoteappem jsou:

* velikost bitové kopie Hello musí být násobkem MB. Pokud se pokusíte tooupload obrázek, který není přesný více, nahrávání hello se nezdaří.
* velikost obrazu Hello musí být 127 GB nebo menší.
* Musí být na soubor virtuálního pevného disku (VHDX soubory [technologie Hyper-V virtuální pevné disky] nejsou aktuálně podporovány.).
* Hello virtuálního pevného disku nesmí být virtuální počítač generace 2.
* Hello virtuální pevný disk může být buď pevné velikosti, nebo dynamicky se zvětšující. Dynamicky se zvětšující virtuální pevný disk je doporučená, protože trvá méně času tooupload tooAzure než soubor virtuálního pevného disku pevné velikosti.
* Hello disku musí být inicializován pomocí rozdělení styl hello hlavní spouštěcí záznam (MBR). Hello stylem oddílů GUID (GPT) oddílu není podporováno.
* Hello virtuální pevný disk musí obsahovat jeden instalace systému Windows Server 2012 R2. Může obsahovat několik svazků, ale jenom jeden s obsahuje instalace systému Windows.
* musí být nainstalován Hello hostitele relace vzdálené plochy (RDSH) role a funkce Možnosti práce s počítačem hello.
* role Zprostředkovatel připojení ke vzdálené ploše Hello musí *není* nainstalovat.
* musí se zakázat Hello systém souborů (Encrypting File System).
* Hello bitová kopie musí být SYSPREPed pomocí parametrů hello **/oobe / generalize/shutdown** (nesmí použít hello **/mode:vm** parametr).
* Odesílání svůj disk VHD z řetěz snímku není podporována.

**Než začnete**

Budete potřebovat následující hello toodo před vytvořením služby hello:

* [Zaregistrujte si](https://azure.microsoft.com/services/remoteapp/) pro vzdálenou aplikaci RemoteApp.
* Vytvořte uživatelský účet ve službě Active Directory toouse hello účet služby RemoteApp. Omezte hello oprávnění pro tento účet tak, aby jej pouze připojit k doméně toohello počítače. V tématu [konfigurace Azure Active Directory pro RemoteApp](remoteapp-ad.md) Další informace.
* Shromážděte informace o síti na pracovišti: IP adresa informace a podrobnosti o zařízení VPN.
* Nainstalujte hello [prostředí Azure PowerShell](/powershell/azure/overview) modulu.
* Shromážděte informace o uživatelích hello, které chcete toogrant přístup k. To může být buď informace o účtu Microsoft, nebo informace o účtu služby Active Directory práce pro uživatele.

## <a name="create-a-template-image"></a>Vytvoření image šablony
Toto jsou kroky vysoké úrovně toocreate hello novou bitovou kopii šablony ze začátku:

1. Vyhledejte disk DVD systému Windows Server 2012 R2 Update nebo ISO image.
2. Vytvořte soubor virtuálního pevného disku.
3. Nainstalujte Windows Server 2012 R2.
4. Nainstalujte hello hostitele relace vzdálené plochy (RDSH) role a funkce Možnosti práce s počítačem hello.
5. Nainstalujte další funkce, které vyžadují aplikace.
6. Instalace a konfigurace aplikace. sdílení aplikace toomake jednodušší, přidejte všechny aplikace nebo programy, které chcete tooshare toohello **spustit** nabídky hello bitové kopie, konkrétně v **%systemdrive%\ProgramData\Microsoft\Windows\Start Start\Programy.
7. Proveďte žádné další konfigurace Windows vyžadují aplikace.
8. Zakážete hello systém souborů (Encrypting File System).
9. **REQUIRED:** přejděte tooWindows aktualizace a nainstalujte všechny důležité aktualizace.
10. Hello bitové kopie nástroje SYSPREP.

Hello podrobné kroky pro vytvoření nové bitové kopie jsou:

1. Vyhledejte disk DVD systému Windows Server 2012 R2 Update nebo ISO image.
2. Vytvořte soubor virtuálního pevného disku pomocí správy disků.
   
   1. Spusťte správu disků (diskmgmt.msc).
   2. Vytvořte dynamicky se zvětšující virtuální pevný disk 40 GB nebo více velikostí. (Odhad hello množství místa, které jsou potřebné pro Windows, vaše aplikace a přizpůsobení. Windows Server s hello RDSH role a funkce desktopové prostředí nainstalovaná bude vyžadovat přibližně 10 GB místa).
      
      1. Klikněte na tlačítko **akce > Vytvoření virtuálního pevného disku**.
      2. Zadejte umístění hello, velikosti a formátu virtuálního pevného disku. Vyberte **dynamicky se zvětšující**a potom klikněte na **OK**.
      
      Tím se spustí na několik sekund. Po dokončení hello vytvoření virtuálního pevného disku, měli byste vidět nový disk bez jakékoli písmeno jednotky a ve stavu "Nebyla inicializována" v konzole Správa disků hello.
      
      * Klikněte pravým tlačítkem na disk hello (ne hello volné místo) a pak klikněte na **inicializovat Disk**. Vyberte **MBR** (hlavní spouštěcí záznam) jako hello styl oddílu a pak klikněte na tlačítko **OK**.
      * Vytvořte nový svazek: klikněte pravým tlačítkem na hello volné místo a pak klikněte na **nový jednoduchý svazek**. Můžete přijmout výchozí hodnoty hello v Průvodci hello, ale ujistěte se, že když přiřadíte potenciální problémy jednotky písmeno tooavoid, když nahrajete image šablony hello.
      * Klikněte pravým tlačítkem na hello disk a pak klikněte na **odpojit virtuální pevný disk**.
3. Nainstalujte Windows Server 2012 R2:
   
   1. Vytvoření nového virtuálního počítače. Použijte hello Průvodce novým virtuálním počítačem ve Správci technologie Hyper-V nebo klienta Hyper-V.
      1. Na stránce zadat generaci hello zvolte **generace 1**.
      2. Na stránce připojit virtuální pevný Disk hello vyberte **použít existující virtuální pevný disk**a vyhledejte toohello virtuálního pevného disku, které jste vytvořili v předchozím kroku hello.
      3. Na stránce Možnosti instalace hello vyberte **nainstalovat operační systém ze spouštěcího disku CD/DVD_ROM**a potom vyberte hello umístění instalačního média systému Windows Server 2012 R2.
      4. Zvolte jiné možnosti v hello Průvodce nezbytné tooinstall Windows a aplikací. Dokončení Průvodce hello.
   2. Po dokončení Průvodce hello upravit nastavení hello hello virtuálního počítače a proveďte další změny v systému Windows nezbytné tooinstall a programy, jako je například hello počet virtuálních procesorů a pak klikněte na tlačítko **OK**.
   3. Připojte toohello virtuální počítač a nainstalujte Windows Server 2012 R2.
4. Instalace role Hostitel relace vzdálené plochy (RDSH) hello a hello funkce desktopové prostředí:
   1. Spusťte správce serveru.
   2. Klikněte na tlačítko **přidat role a funkce** na úvodní obrazovce hello nebo z hello **spravovat** nabídky.
   3. Klikněte na tlačítko **Další** na stránce než začnete hello.
   4. Vyberte **instalace na základě rolí nebo na základě funkcí**a potom klikněte na **Další**.
   5. Vyberte místní počítač hello hello seznamu a pak klikněte na tlačítko **Další**.
   6. Vyberte **služby Vzdálená plocha**a potom klikněte na **Další**.
   7. Rozbalte položku **uživatelská rozhraní a infrastruktura** a vyberte **možnosti práce s počítačem**.
   8. Klikněte na tlačítko **přidat funkce**a potom klikněte na **Další**.
   9. Na stránce hello vzdálené plochy, klikněte na **Další**.
   10. Klikněte na tlačítko **hostitel relace vzdálené plochy**.
   11. Klikněte na tlačítko **přidat funkce**a potom klikněte na **Další**.
   12. Na stránce Výběr instalace hello potvrdit, vyberte **restartování hello cílový server automaticky podle potřeby**a potom klikněte na **Ano** na hello upozornění na restartování.
   13. Klikněte na **Nainstalovat**. Hello počítač se restartuje.
5. Nainstalujte další funkce, které vyžadují aplikace, jako je například hello rozhraní .NET Framework 3.5. tooinstall hello funkce, spusťte Průvodce hello přidání rolí a funkcí.
6. Nainstalujte a nakonfigurujte hello programy a aplikace chcete toopublish prostřednictvím vzdálené aplikace RemoteApp.

> [!IMPORTANT]
> Nainstalujte roli RDSH hello před instalací aplikace tooensure, že jsou všechny problémy s kompatibilitou aplikací zjišťuje před hello image nahrát tooRemoteApp.
> 
> Zajistěte, aby k aplikaci tooyour zástupce (**lnk** souboru) se zobrazí v hello **spustit** nabídky pro všechny uživatele (%systemdrive%\ProgramData\Microsoft\Windows\Start Start\Programy). Ujistěte se také, že hello ikonu se zobrazí v hello **spustit** nabídka je co chcete toosee uživatele. V opačném případě ho změnit. (Ho použít nechcete *mít* nabídky Start toohello aplikace hello tooadd, ale je mnohem snazší hello aplikace toopublish v Remoteappu. Jinak máte tooprovide hello instalační cestu pro aplikace hello při publikování aplikace hello.)
> 
> 

1. Proveďte žádné další konfigurace Windows vyžadují aplikace.
2. Zakážete hello systém souborů (Encrypting File System). Spusťte následující příkaz v okno s vyššími oprávněními příkaz hello:
   
     Chování příkazu fsutil nastavit disableencryption 1
   
   Alternativně můžete nastavit nebo přidejte následující hodnotu DWORD v registru hello hello:
   
     HKLM\System\CurrentControlSet\Control\FileSystem\NtfsDisableEncryption = 1
3. Pokud vytváříte image uvnitř virtuálního počítače Azure, přejmenujte hello  **\%windir%\Panther\Unattend.xml** souborů, jako jsou třeba zablokují se tím skriptu nahrávání hello později použití v práci. Změňte název hello tento soubor tooUnattend.old tak, aby soubor hello budete mít stále v případě, že potřebujete toorevert vaše nasazení.
4. Přejděte tooWindows aktualizace a nainstalujte všechny důležité aktualizace. Můžete potřebovat toorun služby Windows Update tooget více než jednou. všechny aktualizace. (Někdy nainstalujete aktualizaci, a tuto aktualizaci, samotné vyžaduje aktualizace.)
5. Hello bitové kopie nástroje SYSPREP. Na příkazovém řádku se zvýšenými oprávněními spusťte následující příkaz hello:
   
   **C:\Windows\System32\sysprep\sysprep.exe / generalize/oobe / shutdown**
   
   **Poznámka:** nepoužívejte hello **/mode:vm** přepínač hello příkaz SYSPREP i v případě, že toto je virtuální počítač.

## <a name="next-steps"></a>Další kroky
Teď, když máte image vlastní šablony, musíte tooupload této bitové kopie tooyour kolekce vzdálené aplikace RemoteApp. Pomocí informací hello v hello následující články toocreate kolekce:

* [Jak toocreate hybridní kolekce vzdálené aplikace RemoteApp](remoteapp-create-hybrid-deployment.md)
* [Jak toocreate cloudové kolekce vzdálené aplikace RemoteApp](remoteapp-create-cloud-deployment.md)

