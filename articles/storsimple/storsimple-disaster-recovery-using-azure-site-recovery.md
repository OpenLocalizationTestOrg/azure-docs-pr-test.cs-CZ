---
title: "sdílení souborů StorSimple aaaAutomate zotavení po Havárii s Azure Site Recovery | Microsoft Docs"
description: "Popisuje hello kroky a osvědčené postupy pro vytváření řešení zotavení po havárii pro sdílené složky hostované na úložiště Microsoft Azure StorSimple."
services: storsimple
documentationcenter: NA
author: vidarmsft
manager: syadav
editor: 
ms.assetid: 23049a2c-055e-4d0e-b8f5-af2a87ecf53f
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/09/2017
ms.author: vidarmsft
ms.openlocfilehash: fa3e8d4e77ca0f6a7b5f9bbb956a4de12547642e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="automated-disaster-recovery-solution-using-azure-site-recovery-for-file-shares-hosted-on-storsimple"></a>Automatizované zotavení po havárii řešení pomocí Azure Site Recovery pro sdílené složky hostované na StorSimple
## <a name="overview"></a>Přehled
Microsoft Azure StorSimple je řešení hybridní cloudové úložiště, které adresy hello složitosti nestrukturovaných dat běžně spojovaných se sdílenými složkami. StorSimple používá úložiště v cloudu jako rozšíření hello místní řešení a automaticky úrovně dat mezi místní úložiště a úložiště v cloudu. Integrovat ochranu dat, místní a cloudových snímků, eliminuje nutnost hello rozrůstající infrastruktury úložiště.

[Azure Site Recovery](../site-recovery/site-recovery-overview.md) je služba na základě Azure, která poskytuje možnosti zotavení po havárii tím, že orchestruje replikaci, převzetí služeb při selhání a obnovení virtuálních počítačů. Azure Site Recovery podporuje mnoho replikace technologie tooconsistently replikovat, ochranu a selhání plynule přepne virtuální počítače a aplikace tooprivate a veřejného nebo hostované cloudy.

Pomocí Azure Site Recovery, replikaci virtuálního počítače a možností snímku StorSimple cloudu, můžete chránit prostředí serveru hello celý soubor. V případě hello narušení můžete jedním kliknutím toobring, vaše sdílené složky online v Azure za několik minut.

Tento dokument podrobně vysvětluje, jak můžete vytvořit řešení zotavení po havárii pro vaše sdílené složky hostované na StorSimple úložiště a provést plánované, neplánované a testovací převzetí služeb při selhání pomocí plán obnovení jedním kliknutím. V podstatě ukazuje, jak lze upravit hello plán obnovení v vaší tooenable trezoru Azure Site Recovery StorSimple převzetí služeb při selhání během scénáře po havárii. Kromě toho popisuje podporované konfigurace a požadavky. Tento dokument předpokládá, že jste obeznámeni s hello základní informace o Azure Site Recovery a StorSimple architektury.

## <a name="supported-azure-site-recovery-deployment-options"></a>Podporované možnosti nasazení Azure Site Recovery
Zákazníci může nasazovat souborové servery, jako fyzických serverech nebo virtuálních počítačů (VM) systémem Hyper-V nebo VMware a poté vytvořit sdílené složky ze svazků carved mimo úložišti StorSimple. Azure Site Recovery můžete chránit oba fyzické a virtuální nasazení tooeither sekundární lokality nebo tooAzure. Tento dokument popisuje podrobnosti řešení zotavení po Havárii s Azure jako hello obnovení lokality pro souborový server, který virtuální počítač hostovaný na Hyper-V a sdílené složky na úložišti StorSimple. Podobně se dá implementovat scénáře, ve které hello souborového serveru se virtuální počítač na virtuální počítače VMware nebo fyzický počítač.

## <a name="prerequisites"></a>Požadavky
Implementace řešení obnovení po havárii jedním kliknutím, které využívá Azure Site Recovery pro sdílené složky hostované na úložišti StorSimple má hello následující požadavky:

* Místní server Windows Server 2012 R2 souboru, který virtuální počítač hostovaný na Hyper-V nebo VMware nebo fyzický počítač
* StorSimple úložiště místním zařízení zaregistrované pomocí nástroje Azure StorSimple manager
* Zařízení StorSimple cloudu vytvořené v hello Azure StorSimple manager (to je možné mít v vypnutí dolů stavu)
* Sdílené složky hostované na svazcích hello nakonfigurované na zařízení úložiště StorSimple hello
* [Trezor služeb Azure Site Recovery](../site-recovery/site-recovery-vmm-to-vmm.md) vytvořené v odběru Microsoft Azure

Kromě toho, pokud je váš web obnovení Azure, spusťte hello [nástroj hodnocení připravenosti virtuálního počítače Azure](http://azure.microsoft.com/downloads/vm-readiness-assessment/) na tooensure virtuální počítače, aby byly kompatibilní s virtuálními počítači Azure a Azure Site Recovery services.

tooavoid problémy s latencí (které může mít za následek vyšší poplatky za), ujistěte se, že můžete vytvořit vaše zařízení StorSimple cloudu, účet automation a hello účty úložiště ve stejné oblasti.

## <a name="enable-dr-for-storsimple-file-shares"></a>Povolit zotavení po Havárii pro StorSimple sdílené složky
Jednotlivé komponenty hello místního prostředí musí toobe chráněné tooenable dokončení replikace a obnovení. Tato část popisuje, jak:

* Nastavení replikace služby Active Directory a DNS (volitelné)
* Použití ochrany Azure Site Recovery tooenable hello souborového serveru virtuálního počítače
* Povolit ochranu svazky zařízení StorSimple
* Nakonfigurujte síť hello

### <a name="set-up-active-directory-and-dns-replication-optional"></a>Nastavení replikace služby Active Directory a DNS (volitelné)
Pokud chcete chránit počítače využívající služby Active Directory a DNS, aby byly k dispozici v lokalitě hello zotavení po Havárii, budete potřebovat tooexplicitly hello tooprotect je (tak, aby hello souborových serverů jsou dostupné po převzetí služeb při selhání s ověřováním). Existují dvě doporučené možnosti podle složitosti hello hello zákazníka v místním prostředí.

#### <a name="option-1"></a>možnost 1
Pokud má zákazník hello malý počet aplikací, jeden řadič domény pro celou hello místní lokality a bude selhání hello celý web, pak doporučujeme použít Azure Site Recovery replikaci tooreplicate hello domény Řadič počítač sekundární lokalita tooa (to je použít pro site-to-site a site Azure).

#### <a name="option-2"></a>Možnost 2
Pokud zákazník hello má velký počet aplikací, běží doménové struktury služby Active Directory a současně se selhání několik aplikací, pak doporučujeme nastavení dalšího řadiče domény v lokalitě hello zotavení po Havárii (sekundární lokality nebo v Azure).

Naleznete příliš[řešení automatizované zotavení po Havárii pro Active Directory a DNS pomocí Azure Site Recovery](../site-recovery/site-recovery-active-directory.md) pokyny při zpřístupnění řadič domény v lokalitě hello zotavení po Havárii. Pro hello zbývající část tohoto dokumentu budeme předpokládat, že řadič domény k dispozici na webu hello zotavení po Havárii.

### <a name="use-azure-site-recovery-tooenable-protection-of-hello-file-server-vm"></a>Použití ochrany Azure Site Recovery tooenable hello souborového serveru virtuálního počítače
Tento krok vyžaduje, abyste Příprava hello místního souboru serveru prostředí, vytvoření a příprava trezoru služby Azure Site Recovery a povolit ochranu souboru hello virtuálních počítačů.

#### <a name="tooprepare-hello-on-premises-file-server-environment"></a>prostředí serveru tooprepare hello místního souboru
1. Sada hello **řízení uživatelských účtů** příliš**Nikdy neupozorňovat**. To je potřeba, aby po selhání převezme Azure Site Recovery můžete cíle iSCSI hello tooconnect skripty služby Azure automation.

   1. Stiskněte klávesu Windows hello + Q a vyhledejte **nástroje Řízení uživatelských účtů**.
   2. Vyberte **řízení uživatelských účtů změnu nastavení**.
   3. Přetáhněte hello panelu toohello dolní směrem **Nikdy neupozorňovat**.
   4. Klikněte na tlačítko **OK** a pak vyberte **Ano** po zobrazení výzvy.

      ![](./media/storsimple-disaster-recovery-using-azure-site-recovery/image1.png)
2. Hello agenta virtuálního počítače nainstalujte na každém souborovém serveru hello virtuálních počítačů. To je potřeba, aby skripty pro automatizaci Azure můžete spustit na hello převzal virtuálních počítačů.

   1. [Stáhnout agenta hello](http://aka.ms/vmagentwin) příliš`C:\\Users\\<username>\\Downloads`.
   2. Otevřete prostředí Windows PowerShell v režimu správce (Spustit jako správce) a potom zadejte následující příkaz toonavigate toohello stažení umístění hello:

      `cd C:\\Users\\<username>\\Downloads\\WindowsAzureVmAgent.2.6.1198.718.rd\_art\_stable.150415-1739.fre.msi`

      > [!NOTE]
      > Název souboru Hello může změnit v závislosti na verzi hello.
      >
      >
3. Klikněte na **Další**.
4. Přijměte hello **podmínky smlouvy** a pak klikněte na **Další**.
5. Klikněte na **Dokončit**.
6. Vytvoření sdílené složky pomocí svazky carved mimo úložišti StorSimple. Další informace najdete v tématu [použít svazky toomanage služby StorSimple Manager hello](storsimple-manage-volumes.md).

   1. V místní virtuální počítače, stiskněte klávesu Windows hello + Q a vyhledejte **iSCSI**.
   2. Vyberte **iniciátor iSCSI**.
   3. Vyberte hello **konfigurace** kartě a zkopírujte název iniciátoru hello.
   4. Přihlaste se toohello [portál Azure](https://portal.azure.com/).
   5. Vyberte hello **StorSimple** kartu a pak vyberte hello služby StorSimple Manager, který obsahuje hello fyzického zařízení.
   6. Vytvořte svazek kontejnery a pak vytvořte svazky. (Tyto svazky jsou pro hello sdílených složek souborů na souborovém serveru hello virtuálních počítačů). Zkopírujte název iniciátoru hello a poskytněte vhodný název pro hello záznamy řízení přístupu při vytváření hello svazky.
   7. Vyberte hello **konfigurace** kartě a zapište hello IP adresa zařízení hello.
   8. Na místní virtuální počítače, přejděte toohello **iniciátor iSCSI** znovu a zadejte IP adresu hello v hello rychle se připojit. Klikněte na tlačítko **rychlé připojení** (hello zařízení by měl být připojili).
   9. Otevřete hello portál Azure a vyberte hello **svazky a zařízení** kartě. Klikněte na tlačítko **automatické konfigurace**. by se zobrazit Hello svazek, který jste právě vytvořili.
   10. Hello portálu, vyberte hello **zařízení** a pak vyberte **vytvoření nového virtuálního zařízení.** (Tato virtuální zařízení bude použit, pokud dojde k selhání). Toto nové virtuální zařízení můžete uchovávat v tooavoid stavu offline poplatkům. tootake hello virtuální zařízení offline, přejděte toohello **virtuální počítače** části na hello portál a ho vypnout.
   11. Přejděte zpět toohello místní virtuální počítače a spusťte správu disků (stiskněte klávesu Windows hello + X a vyberte **Správa disků**).
   12. Si všimnete některé další disky (v závislosti na počtu hello svazky, které jste vytvořili). Klikněte pravým tlačítkem na dobrý den první z nich, vyberte **inicializovat Disk**a vyberte **OK**. Klikněte pravým tlačítkem na hello **Unallocated** vyberte **nový jednoduchý svazek**, přiřaďte písmeno jednotky a ukončíte průvodce hello.
   13. Opakujte krok l pro všechny disky hello. Nyní uvidíte všechny disky hello na **tento počítač** v Průzkumníku Windows hello.
   14. Použijte hello Souborová služba a služba úložiště role toocreate sdílené složky v těchto svazcích.

#### <a name="toocreate-and-prepare-an-azure-site-recovery-vault"></a>toocreate a připravte trezoru služby Azure Site Recovery
Odkazovat toohello [dokumentace Azure Site Recovery](../site-recovery/site-recovery-hyper-v-site-to-azure.md) tooget začít s Azure Site Recovery než začnete chránit souborový server hello virtuálních počítačů.

#### <a name="tooenable-protection"></a>tooenable ochrany
1. Odpojit hello iSCSI cíle (cílů) z hello místní virtuální počítače, které chcete tooprotect prostřednictvím Azure Site Recovery:

   1. Stiskněte klávesu Windows + Q a vyhledejte **iSCSI**.
   2. Vyberte **nastavit iniciátor iSCSI**.
   3. Odpojte zařízení StorSimple hello, který jste dříve připojen. Alternativně můžete vypnout hello souborový server pro několik minut při povolování ochrany.

   > [!NOTE]
   > To způsobí, že toobe sdílené složky souboru hello není dočasně k dispozici.
   >
   >
2. [Povolení ochrany virtuálního počítače](../site-recovery/site-recovery-hyper-v-site-to-azure.md) hello souborového serveru virtuálního počítače z portálu Azure Site Recovery hello.
3. Po zahájení hello počáteční synchronizace se můžete připojit cílové hello znovu. Přejděte iniciátor iSCSI toohello, vyberte hello zařízení StorSimple a klikněte na tlačítko **Connect**.
4. Když po dokončení synchronizace hello a stav hello hello virtuálního počítače je **chráněné**vyberte hello virtuálního počítače, vyberte hello **konfigurace** kartě a odpovídajícím způsobem aktualizovat hello síti hello virtuálních počítačů (Toto je hello sítě Tento hello převzal virtuální počítače bude součástí i nadále). Pokud hello sítě nezobrazí, znamená to, stále probíhají hello synchronizace.

### <a name="enable-protection-of-storsimple-volumes"></a>Povolit ochranu svazky zařízení StorSimple
Pokud nevyberete hello **povolit výchozí zálohování tohoto svazku** možnost hello svazky zařízení StorSimple, přejděte příliš**zásady zálohování** v hello služby StorSimple Manager a vytvořit zálohu vhodný zásady pro všechny svazky hello. Doporučujeme, abyste nastavili hello četnost zálohování toohello plánovaného bodu obnovení (RPO), které chcete toosee aplikace hello.

### <a name="configure-hello-network"></a>Nakonfigurujte síť hello
Pro souborový server hello virtuálních počítačů konfigurace nastavení sítě v Azure Site Recovery, aby hello sítě virtuálních počítačů po převzetí služeb při selhání připojené toohello správné síťové zotavení po Havárii.

Můžete vybrat hello virtuálních počítačů v hello **replikované položky** kartě tooconfigure hello síťová nastavení, jak ukazuje následující obrázek hello.

![](./media/storsimple-disaster-recovery-using-azure-site-recovery/image2.png)

## <a name="create-a-recovery-plan"></a>Vytvoření plánu obnovení
Můžete vytvořit plán obnovení v nástroji automatické obnovení systému tooautomate hello převzetí služeb při selhání procesu hello sdílených složek. Pokud dojde k narušení služeb, můžete zahrnout hello sdílené složky za několik minut, s právě jedním kliknutím. tooenable tato automatizace, budete potřebovat účet Azure automation.

#### <a name="toocreate-an-automation-account"></a>toocreate účet služby Automation.
1. Toohello portálu Azure přejděte &gt; **automatizace** části.
2. Klikněte na tlačítko **+ přidat** tlačítko, otevře se okno pod okno.

   ![](./media/storsimple-disaster-recovery-using-azure-site-recovery/image11.png)

   * Název – zadejte nový účet automation.
   * Předplatné – zvolte předplatné
   * Prostředek skupiny – vytvořit novou nebo vyberte existující skupinu prostředků
   * Umístění – vyberte umístění, uložte jej v hello stejnou geograficky nebo oblast, ve které hello byly vytvořeny cloudu zařízení StorSimple a účty úložiště.
   * Vytvoření účtu spustit v Azure jako - vyberte **Ano** možnost.

3. Přejděte toohello účet Automation, klikněte na tlačítko **Runbooky** &gt; **procházet galerii** tooimport všechny hello požadované sady runbook do účtu automation hello.
4. Přidejte následující sady runbook tak, že najdete hello **zotavení po havárii** značky v galerii hello:

   * Vyčištění svazky zařízení StorSimple po převzetí služeb při selhání testu (TFO)
   * Kontejnery svazků zařízení StorSimple převzetí služeb při selhání
   * Po převzetí služeb při selhání připojte svazky na zařízení StorSimple
   * Odinstalovat rozšíření vlastních skriptů ve virtuálním počítači Azure
   * Spustit virtuální zařízení StorSimple

     ![](./media/storsimple-disaster-recovery-using-azure-site-recovery/image3.png)

5. Publikovat všechny skripty hello výběrem hello runbook v účtu automation hello a klikněte na tlačítko **upravit** &gt; **publikovat** a potom **Ano** toohello ověření zpráva. Po provedení tohoto kroku hello **Runbooky** se zobrazí karta následujícím způsobem:

    ![](./media/storsimple-disaster-recovery-using-azure-site-recovery/image4.png)

6. V účtu automation hello, vyberte hello **prostředky** kartě &gt; klikněte na tlačítko **proměnné** &gt; **přidat proměnnou** a přidejte následující proměnné hello. Můžete tooencrypt tyto prostředky. Tyto proměnné jsou specifické pro plán obnovení. Pokud vaše obnovení plánu (které vytvoříte v dalším kroku hello) TestPlan je název a potom proměnných by měl být TestPlan StorSimRegKey, TestPlan AzureSubscriptionName a tak dále.

   * *RecoveryPlanName***- StorSimRegKey**: hello registrační klíč pro hello služby StorSimple Manager.
   * *RecoveryPlanName***- AzureSubscriptionName**: název hello hello předplatného Azure.
   * *RecoveryPlanName***- ResourceName**: název hello hello StorSimple prostředek, který má zařízení StorSimple hello.
   * *RecoveryPlanName***- DeviceName**: hello zařízení, která obsahuje toobe převzetí služeb při selhání.
   * *RecoveryPlanName***- VolumeContainers**: řetězec kontejnery svazků oddělených čárkami k dispozici hello zařízení vyžadující toobe se nezdařilo přes, například volcon1, volcon2, volcon3.
   * *RecoveryPlanName***- TargetDeviceName**: hello zařízení StorSimple cloudu, na které hello jsou kontejnery toobe převzetí služeb při selhání.
   * *RecoveryPlanName***- TargetDeviceDnsName**: název služby hello hello cílové zařízení (lze najít v hello **virtuální počítač** části: název služby hello je hello stejné jako hello Název DNS).
   * *RecoveryPlanName***- StorageAccountName**: název účtu úložiště hello, ve které hello skriptu (která toorun na hello převzal služby při selhání virtuálního počítače) se uloží. To může být jakékoli úložiště účet, který má některé místo toostore hello skriptu dočasně.
   * *RecoveryPlanName***- StorageAccountKey**: hello přístupový klíč pro hello výše účet úložiště.
   * *RecoveryPlanName***- ScriptContainer**: název hello hello kontejner, ve které hello skriptu budou uložené v cloudu hello. Pokud hello kontejner neexistuje, bude vytvořen.
   * *RecoveryPlanName***- VMGUIDS**: při ochraně virtuálních počítačů, přiřazuje Azure Site Recovery každý virtuální počítač jedinečné ID, která poskytuje podrobnosti o hello hello při selhání virtuálního počítače. hello tooobtain VMGUID, vyberte hello **služeb zotavení** a klikněte na **chráněné položky** &gt; **skupin ochrany** &gt; **Počítače** &gt; **vlastnosti**. Pokud máte víc virtuálních počítačů, přidejte identifikátory GUID hello jako řetězec oddělených čárkami.
   * *RecoveryPlanName***- AutomationAccountName** – hello název účtu automation hello, ve které jste přidali hello runbooky a prostředky hello.

  Například pokud hello název plánu obnovení hello je fileServerpredayRP, pak vaše **pověření** & **proměnné** karet se zobrazí následující po přidání všechny prostředky hello.

   ![](./media/storsimple-disaster-recovery-using-azure-site-recovery/image5.png)

7. Přejděte toohello **služeb zotavení** oddílu a vyberte hello trezoru Azure Site Recovery, který jste vytvořili dříve.
8. Vyberte hello **plány obnovení (služba Site Recovery)** možnost z **spravovat** skupiny a vytvoření nového plánu obnovení následujícím způsobem:

   a.  Klikněte na tlačítko **+ plán obnovení** tlačítko, otevře se okno pod okno.

      ![](./media/storsimple-disaster-recovery-using-azure-site-recovery/image6.png)

   b.  Zadejte název plánu obnovení, vyberte zdroje, cíl a nasazení modelu hodnoty.

   c.  Vyberte virtuální počítače hello ze skupiny ochrany hello chcete tooinclude hello plánu obnovení a klikněte na **OK** tlačítko.

   d.  Vyberte plán obnovení, který jste předtím vytvořili, klikněte na **přizpůsobit** tlačítko zobrazení pro vlastní plán tooopen hello obnovení.

   e.  Klikněte pravým tlačítkem na **všechny skupiny vypnutí** a klikněte na tlačítko **akce bez přidání**.

   f.  Otevře okno akce vložit, zadejte název, vyberte **primární straně** možnost v kde možnost toorun, vyberte účet Automation (ve kterém jste přidali hello sady runbook) a pak vyberte hello  **Převzetí služeb při selhání StorSimple svazku kontejnery** sady runbook.

   g.  Klikněte pravým tlačítkem na **1. skupina: spuštění** a klikněte na tlačítko **přidat chráněné položky** možnost, pak vyberte hello virtuálních počítačů, které jsou chráněné v plánu obnovení hello a klikněte na toobe **Ok** tlačítko. Volitelné, pokud je již vybrána virtuálních počítačů.

   h.  Klikněte pravým tlačítkem na **1. skupina: spuštění** a klikněte na tlačítko **Post akce** možnost pak přidejte všechny hello tyto skripty:

   * Runbook Start-StorSimple-virtuální-zařízení
   * Selhání runbook přes StorSimple svazku kontejnery
   * Připojení svazky po převzetí runbook
   * Odinstalujte--rozšíření vlastních skriptů runbook

   i.  Přidat ruční akce po hello výše 4 skriptů v hello stejné **1. skupina: po kroky** části. Tato akce je hello bod, ve kterém můžete ověřit, zda vše funguje správně. Tato akce musí toobe přidat jenom jako součást testovací převzetí služeb při selhání (takže vyberte pouze hello **testovací převzetí služeb při selhání** políčko).

   j.  Po ruční akci hello, přidejte hello **čištění** skript hello stejným způsobem, který jste použili pro hello jiné runbooky. **Uložit** hello plánu obnovení.

    > [!NOTE]
    > Při spouštění testovací převzetí služeb, by všechno, co je v kroku manuální akce hello ověřit, protože hello svazky zařízení StorSimple, které měl byla klonovat na hello cílové zařízení se odstraní jako součást hello čištění po dokončení hello manuální akce.
    >

    ![](./media/storsimple-disaster-recovery-using-azure-site-recovery/image7.png)

## <a name="perform-a-test-failover"></a>Provedení testu převzetí služeb
Odkazovat toohello [Active Directory DR řešení](../site-recovery/site-recovery-active-directory.md) doprovodná Příručka pro konkrétní tooActive aspekty Directory během hello testovací převzetí služeb při selhání. Hello místní instalace nebude vůbec narušen, když dojde k hello testovací převzetí služeb při selhání. Hello svazky zařízení StorSimple, které byly připojené toohello místní virtuální počítač se klonovaný toohello cloudu zařízení StorSimple v Azure. Virtuální počítač pro účely testování je zapínají v Azure a hello klonovaný jsou svazky připojené toohello virtuálních počítačů.

#### <a name="tooperform-hello-test-failover"></a>tooperform hello testovací převzetí služeb při selhání
1. V hello portálu Azure vyberte váš trezor site recovery.
2. Klikněte na tlačítko pro souborový server hello virtuálního počítače vytvořit plán obnovení hello.
3. Klikněte na tlačítko **testovací převzetí služeb při selhání**.
4. Vyberte hello virtuální síť Azure toowhich virtuální počítače Azure připojí po převzetí služeb při selhání.

   ![](./media/storsimple-disaster-recovery-using-azure-site-recovery/image8.png)
5. Klikněte na tlačítko **OK** toobegin hello převzetí služeb při selhání. Průběh můžete sledovat, když kliknete na virtuální počítač tooopen hello jeho vlastnosti, nebo na hello **testovací převzetí služeb při selhání úlohy** v název trezoru &gt; **úlohy** &gt; **úlohy Site Recovery**.
6. Po dokončení převzetí služeb při selhání hello, měli byste také mít možnost toosee hello repliky počítač Azure se zobrazí v hello portál Azure &gt; **virtuální počítače**. Vaše ověření můžete provést.
7. Po ověření hello hotovi, klikněte na tlačítko **dokončení ověření**. Toto bude čištění text hello svazky zařízení StorSimple a vypnutí hello cloudu zařízení StorSimple.
8. Jakmile jste hotovi, klikněte na možnost **vyčistit testovací převzetí služeb při selhání** v plánu obnovení hello. V poznámkách k záznamu a uložte jakékoli připomínky související s hello testovací převzetí služeb při selhání. Tato akce odstraní hello virtuálního počítače, které se vytvořily během testovacího převzetí služeb při selhání.

## <a name="perform-a-planned-failover"></a>Proveďte plánované převzetí služeb při selhání
   Při plánovaném převzetí služeb při souboru hello místní server, který je korektně vypnout virtuální počítač a cloudem pořízení snímku zálohy hello svazků v zařízení StorSimple. svazky zařízení StorSimple Hello jsou převzetí služeb při selhání virtuálního zařízení toohello repliku, kterou je zapínají virtuálních počítačů v Azure, a jsou hello svazky připojené toohello virtuálních počítačů.

#### <a name="tooperform-a-planned-failover"></a>tooperform plánované převzetí služeb při selhání
1. V hello portálu Azure, vyberte **služeb zotavení** trezoru &gt; **plány obnovení (služba Site recovery)** &gt; **recoveryplan_name** vytvořené pro Souborový server Hello virtuálních počítačů.
2. V okně plán obnovení hello, klikněte na tlačítko **Další** &gt; **plánované převzetí služeb při selhání**.  

   ![](./media/storsimple-disaster-recovery-using-azure-site-recovery/image9.png)
3. Na hello **potvrďte plánované převzetí služeb při selhání** okně zvolte hello zdrojové a cílové umístění a vyberte Cílová síť a klikněte na hello zkontrolujte ikonu ✓ toostart hello převzetí služeb při selhání procesu.
4. Po vytvoření repliky virtuálních počítačů jsou ve stavu čekání na potvrzení. Klikněte na tlačítko **potvrdit** toocommit hello převzetí služeb při selhání.
5. Po dokončení replikace hello virtuální počítače se spustí na hello sekundárního umístění.

## <a name="perform-a-failover"></a>Provedení převzetí služeb při selhání
Během neplánované převzetí služeb při selhání svazky zařízení StorSimple hello jsou převzetí služeb při selhání virtuálního zařízení toohello repliku, kterou virtuální počítač se zapínají v Azure, a jsou hello svazky připojené toohello virtuálních počítačů.

#### <a name="tooperform-a-failover"></a>tooperform převzetí služeb při selhání
1. V hello portálu Azure, vyberte **služeb zotavení** trezoru &gt; **plány obnovení (služba Site recovery)** &gt; **recoveryplan_name** vytvořené pro Souborový server Hello virtuálních počítačů.
2. V okně plán obnovení hello, klikněte na tlačítko **Další** &gt; **převzetí služeb při selhání**.  
3. Na hello **potvrzení převzetí služeb při selhání** okně zvolte hello zdrojové a cílové umístění.
4. Vyberte **vypnout virtuální počítače a synchronizovat nejnovější data hello** toospecify, by měl zkuste tooshut dolů hello chráněný virtuální počítač a synchronizaci dat hello tak, aby hello nejnovější verze hello dat bude Site Recovery převzetí služeb při selhání.
5. Po převzetí služeb při selhání hello hello virtuální počítače jsou ve stavu čekání na potvrzení. Klikněte na tlačítko **potvrdit** toocommit hello převzetí služeb při selhání.


## <a name="perform-a-failback"></a>Provést navrácení služeb po obnovení
Během navrácení služeb po obnovení jsou kontejnery svazků zařízení StorSimple převzal back toohello fyzického zařízení po zálohování se provede.

#### <a name="tooperform-a-failback"></a>tooperform navrácení služeb po obnovení
1. V hello portálu Azure, vyberte **služeb zotavení** trezoru &gt; **plány obnovení (služba Site Recovery)** &gt; **recoveryplan_name** vytvořené pro Souborový server Hello virtuálních počítačů.
2. V okně plán obnovení hello, klikněte na tlačítko **Další** &gt; **plánované převzetí služeb při selhání**.  
3. Zvolte hello zdrojové a cílové umístění, vyberte hello příslušné synchronizace dat a možností vytvoření virtuálního počítače.
4. Klikněte na tlačítko **OK** tlačítko toostart hello navrácení služeb po obnovení procesu.

   ![](./media/storsimple-disaster-recovery-using-azure-site-recovery/image10.png)

## <a name="best-practices"></a>Osvědčené postupy
### <a name="capacity-planning-and-readiness-assessment"></a>Kapacity assessment plánování a připravenosti
#### <a name="hyper-v-site"></a>Web Hyper-V
Použití hello [nástroje Plánovač kapacity uživatele](http://www.microsoft.com/download/details.aspx?id=39057) toodesign hello serveru, úložiště a síťové infrastruktury pro vaše prostředí repliky technologie Hyper-V.

#### <a name="azure"></a>Azure
Můžete spustit hello [nástroj hodnocení připravenosti virtuálního počítače Azure](http://azure.microsoft.com/downloads/vm-readiness-assessment/) na tooensure virtuální počítače, aby byly kompatibilní s virtuálními počítači Azure a služeb Azure Site Recovery. Hello Readiness Assessment Tool zkontroluje konfigurací virtuálních počítačů a upozorní, když konfigurace nejsou kompatibilní s Azure. Například vydá varování, pokud je jednotka C: větší než 127 GB.

Plánování kapacity se skládá z nejméně dva důležité procesy:

* Mapování místní virtuální počítače Hyper-V tooAzure velikosti virtuálních počítačů (například A6, A7, A8 a A9).
* Určení hello požadované šířky pásma Internetu.

## <a name="limitations"></a>Omezení
* V současné době pouze 1 zařízení StorSimple můžete převzít služby při selhání (tooa jednotné zařízení StorSimple cloudu). scénář Hello souborového serveru, která zahrnuje několik zařízení StorSimple se ještě nepodporuje.
* Pokud dojde k chybě při povolování ochrany pro virtuální počítač, ujistěte se, že jste odpojeni hello cíle iSCSI.
* Všechny hello kontejnery svazků, které byly seskupeny dohromady z důvodu zásady zálohování pokrývání uzlů napříč kontejnery svazků převezme služby při selhání společně.
* Všechny svazky hello v hello kontejnery svazků, kterou jste vybrali převezme služby při selhání.
* Svazky, které dohromady toomore než 64 TB nelze při selhání, protože maximální kapacita hello o jeden Cloud zařízení StorSimple je 64 TB.
* Pokud selže plánované/neplánované převzetí služeb při selhání hello a hello virtuální počítače jsou vytvořeny v Azure, poté proveďte není vyčistit hello virtuálních počítačů. Místo toho proveďte navrácení služeb po obnovení. Pokud odstraníte virtuální počítače hello pak hello místní, že virtuální počítače nelze znovu zapnout.
* Po selhání Pokud nemůžete toosee hello svazky, přejděte toohello virtuální počítače, spusťte správu disků, Prohledat disky hello a pak je uvést online.
* V některých případech může být jiná než hello písmena místní hello písmena jednotek v lokalitě hello zotavení po Havárii. Pokud k tomu dojde, musíte po dokončení převzetí služeb při selhání hello toomanually správné hello problém.
* Pro hello Azure pověření, která je zadána v účtu automation hello jako prostředek je třeba zakázat službu Multi-Factor authentication. Pokud toto ověřování není zakázán, skripty nebudou povolena, toorun automaticky a hello plánu obnovení se nezdaří.
* Časový limit úlohy převzetí služeb při selhání: hello StorSimple skript bude vypršení časového limitu, pokud převzetí služeb při selhání hello kontejnery svazků trvá déle než limit Azure Site Recovery hello za skriptu (aktuálně 120 minut).
* Časový limit úlohy zálohování: hello StorSimple skriptu časového limitu, pokud hello zálohování svazků trvá déle než limit Azure Site Recovery hello za skriptu (aktuálně 120 minut).

  > [!IMPORTANT]
  > Hello zálohování spustit ručně z hello portál Azure a poté znovu spusťte hello plánu obnovení.

* Časový limit úlohy klonovat: hello StorSimple skriptu časového limitu, pokud hello klonování svazků trvá déle než limit Azure Site Recovery hello za skriptu (aktuálně 120 minut).
* Čas synchronizace Chyba: hello StorSimple skriptů chyby se informacemi o tom, že byly hello zálohování úspěšné i v případě, že je záloha hello úspěšné hello portálu. Možnou příčinou to může být, že zařízení StorSimple hello čas může být mimo rozsah synchronizace s hello aktuální čas v časovém pásmu hello.

  > [!IMPORTANT]
  > Hello zařízení čas synchronizace s hello aktuální čas v časovém pásmu hello.

* Chyba převzetí služeb při selhání zařízení: hello StorSimple skript může selhat, pokud dojde k selhání zařízení, když běží hello plánu obnovení.

  > [!IMPORTANT]
  > Plán obnovení hello znovu po dokončení převzetí služeb při selhání hello zařízení.


## <a name="summary"></a>Souhrn
Pomocí Azure Site Recovery, můžete vytvořit plán obnovení po havárii dokončení automatizované pro souborový server virtuálního počítače s sdílené složky hostované na úložišti StorSimple. Můžete zahájit převzetí služeb při selhání hello během několika sekund z libovolného místa v hello událostí přerušení a zprovoznění aplikace hello za pár minut.
