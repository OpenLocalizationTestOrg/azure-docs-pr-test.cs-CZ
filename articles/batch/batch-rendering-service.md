---
title: "aaaUse hello Azure Batch vykreslování služby toorender v cloudu hello | Microsoft Docs"
description: "Úlohy vykreslování na virtuálních počítačích Azure přímo z aplikace Maya a s platbami za použití."
services: batch
author: tamram
manager: timlt
ms.service: batch
ms.topic: hero-article
ms.date: 07/31/2017
ms.author: tamram
ms.openlocfilehash: 3fb78d883311bbc3ab62743b7d1b111ffad177cd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-batch-rendering-service"></a>Začínáme s hello služby Batch vykreslování

Hello služby Azure Batch vykreslování nabízí možnosti vykreslování cloudového škálovatelného na základě platím za použití. Hello služby Batch vykreslování zpracovává plánování úloh a queueing, Správa selhání a opakování a automatické škálování pro vaši úlohu vykreslování. Hello služby Batch vykreslování podporuje Autodesk Maya, 3ds Max a Arnold s podporu dalších aplikací již brzy. Hello Batch modulu plug-in pro Maya 2017 umožňuje snadno toostart úlohu vykreslování na platformě Azure správné z plochy. 

## <a name="supported-applications"></a>Podporované aplikace

Hello služby vykreslování Batch aktuálně podporuje hello následující aplikace:

- Autodesk Maya
- Autodesk 3ds Max
- Autodesk Arnold

## <a name="prerequisites"></a>Požadavky

Služba Batch vykreslování toouse hello, budete potřebovat:

- [Účet Azure](https://azure.microsoft.com/free/). 
- **Účet Azure Batch.** Pokyny týkající se vytváření účtu Batch hello portálu Azure najdete v tématu [vytvořit dávkový účet s hello portál Azure](batch-account-create-portal.md).
- **Účet služby Azure Storage.** ve službě Azure Storage jsou uložené prostředky Hello používá pro vaši úlohu vykreslování. Účet úložiště můžete vytvořit automaticky při nastavování účtu Batch. Můžete také použít existující účet úložiště. toolearn Další informace o účtech úložiště najdete v části [jak toocreate, spravovat nebo odstranit účet úložiště na portálu Azure hello](https://docs.microsoft.com/azure/storage/storage-create-storage-account).

toouse hello Batch modulu plug-in pro Maya, budete potřebovat:

- **Maya 2017**
- **Arnold pro Maya**

Můžete taky hello [portál Azure](https://portal.azure.com) toocreate fondy virtuálních počítačů, které jsou předem nakonfigurovaným rozhraním Maya 3ds Max a Arnold. Můžete použít hello portálu toomonitor úloh a diagnostikovat neúspěšné úlohy stažením protokoly aplikací a vzdáleně připojením tooindividual virtuálních počítačů pomocí protokolu RDP nebo SSH.

## <a name="basic-batch-concepts"></a>Koncepty služby Batch úrovně Basic

Než začnete pomocí služby Batch vykreslování hello, je užitečné toobe seznámit s několika koncepty Batch, včetně výpočetní uzly, fondy a úlohy. Další informace o Azure Batch obecně naleznete v tématu toolearn [spustit vnitřně paralelní úlohy pomocí služby Batch](batch-technical-overview.md).

### <a name="pools"></a>Fondy

Batch je služba platformy pro spouštění úloh náročných na výpočetní výkon, jako je vykreslování, ve **fondu** **výpočetních uzlů**. Každý výpočetní uzel ve fondu je virtuální počítač Azure s Linuxem nebo Windows. 

Další informace o fondech Batch a výpočetních uzlů najdete v tématu hello [fondu](batch-api-basics.md#pool) a [výpočetním uzlu](batch-api-basics.md#compute-node) v částech [rozsáhlé paralelní vývoj výpočetní řešení pomocí služby Batch](batch-api-basics.md).

### <a name="jobs"></a>Úlohy

Batch **úlohy** je kolekcí úkolů, které běží na hello výpočetních uzlů ve fondu. Při odesílání úlohy vykreslování Batch rozděluje úlohy hello do úlohy a distribuuje hello úlohy toohello výpočetních uzlů ve fondu toorun hello.

Další informace o dávkových úloh najdete v tématu hello [úlohy](batch-api-basics.md#job) kapitoly [rozsáhlé paralelní vývoj výpočetní řešení pomocí služby Batch](batch-api-basics.md).

## <a name="use-hello-batch-plug-in-for-maya-toosubmit-a-render-job"></a>Použít hello Batch modulu plug-in pro Maya toosubmit úlohu vykreslování

S hello Batch modulu plug-in pro Maya můžete odeslat úlohu toohello vykreslování Batch přímo Maya služby. Hello následující části popisují, jak tooconfigure hello úlohu, která z hello modulů plug-in a potom odešlete ji. 

### <a name="load-hello-batch-plug-in-in-maya"></a>Zatížení hello Batch v Maya modulu plug-in

Hello Batch modul plug-in je k dispozici na [Githubu](https://github.com/Azure/azure-batch-maya/releases). Rozbalte hello Archivní adresář tooa podle svého výběru. Hello modul plug-in můžete načíst přímo z hello *azure_batch_maya* adresáře.

hello tooload modul plug-in v Maya:

1. Spusťte aplikaci Maya.
2. Otevřete **Window** (Okno) > **Settings/Preferences** (Nastavení/Předvolby) > **Plug-in Manager** (Správce modulů plug-in).
3. Klikněte na **Browse** (Procházet).
4. Přejděte vyberte tooand *azure_batch_maya/plug-in/AzureBatch.py*.

### <a name="authenticate-access-tooyour-batch-and-storage-accounts"></a>Účty Batch a Storage tooyour přístup ověřit

hello toouse modul plug-in, musíte tooauthenticate pomocí Azure Batch a klíče účtu úložiště Azure. tooretrieve klíče účtu:

1. Zobrazení hello modul plug-in v Maya a vyberte hello **konfigurace** kartě.
2. Přejděte toohello [portál Azure](https://portal.azure.com).
3. Vyberte **účty Batch** z nabídky na levé straně hello. V případě potřeby klikněte na **Další služby** a vyfiltrujte _Batch_.
4. Účet Batch hello potřeby vyhledejte v seznamu hello.
5. Vyberte hello **klíče** nabídky položky toodisplay váš název účtu, adresa URL účtu a přístupové klíče:
    - Adresa URL účtu Batch hello vložit do hello **služby** pole hello Batch modulu plug-in.
    - Název účtu hello vložení do hello **účtu Batch** pole.
    - Vložení hello primární účet klíč do hello **dávky klíč** pole.
7. Vyberte účty úložiště z nabídky na levé straně hello. V případě potřeby klikněte na **Další služby** a vyfiltrujte _Storage_.
8. Účet úložiště hello potřeby vyhledejte v seznamu hello.
9. Vyberte hello **přístupové klíče** toodisplay položky nabídky, hello název účtu úložiště a klíče.
    - Název účtu úložiště hello vložení do hello **účet úložiště** pole hello Batch modulu plug-in.
    - Vložení hello primární účet klíč do hello **klíč úložiště** pole.
10. Klikněte na tlačítko **ověřit** tooensure, který hello modulu plug-in přístupné oba účty.

Jakmile úspěšně jste se ověřili, modul plug-in sady hello hello stav pole příliš**ověřený**: 

![Ověření účtů Batch a Storage](./media/batch-rendering-service/authentication.png)

### <a name="configure-a-pool-for-a-render-job"></a>Konfigurace fondu pro úlohu vykreslování

Po ověření účtů Batch a Storage nastavte fond pro úlohu vykreslování. modul plug-in Hello uloží váš výběr mezi relacemi. Jakmile jste nastavili konfiguraci upřednostňovaných, nebude nutné toomodify ji pouze v případě se změní.

Hello následující části vás provede procesem hello dostupné možnosti k dispozici na hello **odeslání** karty:

#### <a name="specify-a-new-or-existing-pool"></a>Zadání nového nebo existujícího fondu

toospecify fondu, na která toorun hello vykreslení úloha, vyberte hello **odeslání** kartě. Tato karta nabízí možnosti pro vytvoření fondu nebo výběr existujícího fondu:

- Můžete **automatické přidělení fondu pro tuto úlohu** (hello výchozí možnost). Pokud vyberete tuto možnost, Batch vytvoří fond hello výhradně pro aktuální úlohu hello a automaticky odstraní hello fondu při vykreslení hello úloha skončí. Tato možnost je nejvhodnější, až budete mít jeden vykreslení toocomplete úlohy.
- Můžete znovu použít existující trvalý fond (možnost **Reuse an existing persistent pool**). Pokud máte existující fond, který nečinný, můžete tento fond pro spuštěná úloha vykreslení hello výběrem z rozevíracího seznamu hello. Opětovné použití existujícího fondu trvalé uloží hello čas potřebný tooprovision hello fondu.  
- Můžete vytvořit nový trvalý fond (možnost **Create a new persistent pool**). Výběrem této možnosti se vytvoří nový fond pro spuštění úlohy hello. Po hello dokončení úlohy, takže můžete opakovaně použít jej pro budoucí úlohy neodstraní hello fondu. Tuto možnost vyberte, když máte průběžné nutné toorun, vykreslení úlohy. Na následné úlohy, můžete vybrat **znovu použít existující fond trvalé** toouse hello trvalé fondu, který jste vytvořili pro první úlohu hello.

![Zadání fondu, image operačního systému, velikosti virtuálního počítače a licence](./media/batch-rendering-service/submit.png)

Další informace o tom, jak nabíhat poplatky pro virtuální počítače Azure, najdete v části hello [Linux ceny – nejčastější dotazy](https://azure.microsoft.com/pricing/details/virtual-machines/linux/#faq) a [Windows ceny – nejčastější dotazy](https://azure.microsoft.com/pricing/details/virtual-machines/windows/#faq).

#### <a name="specify-hello-os-image-tooprovision"></a>Zadejte bitovou kopii tooprovision hello operačního systému

Můžete zadat typ hello OS image toouse tooprovision výpočetních uzlů ve fondu hello na hello **Env** karta (prostředí). Batch aktuálně podporuje následující možnosti bitové kopie pro vykreslování úlohy hello:

|Operační systém  |Image  |
|---------|---------|
|Linux     |Batch CentOS Preview |
|Windows     |Batch Windows Preview |

#### <a name="choose-a-vm-size"></a>Výběr velikosti virtuálního počítače

Můžete zadat velikost virtuálního počítače hello na hello **Env** kartě. Další informace o dostupných velikostech virtuálních počítačů najdete v tématech [Velikosti virtuálních počítačů s Linuxem v Azure](https://docs.microsoft.com/azure/virtual-machines/linux/sizes) a [Velikosti virtuálních počítačů s Windows v Azure](https://docs.microsoft.com/azure/virtual-machines/windows/sizes). 

![Zadejte bitovou kopii operačního systému virtuálního počítače hello a velikost na kartě Env hello](./media/batch-rendering-service/environment.png)

#### <a name="specify-licensing-options"></a>Zadání možností licencování

Můžete zadat hello licencí chcete toouse na hello **Env** kartě. Mezi možnosti patří:

- **Maya** – Tato možnost je povolená ve výchozím nastavení.
- **Arnold**, která je povolena, pokud se zjistí Arnold hello modul active vykreslení v Maya.

 Pokud chcete toorender pomocí vlastní licenci, můžete nakonfigurovat vaší licence koncového bodu přidáním hello příslušné prostředí proměnné toohello tabulky. Být zda toodeselect možnostem licencování výchozí hello, pokud tak učiníte.

> [!IMPORTANT]
> Fakturuje se využívání licencí hello při virtuální počítače jsou spuštěné ve fondu hello i v případě, že hello virtuální počítače nejsou aktuálně používá pro vykreslování. tooavoid další poplatky, přejděte toohello **fondy** kartě a změňte velikost uzly too0 fondu hello tak, abyste byli připravené toorun jiná úloha vykreslení. 
>
>

#### <a name="manage-persistent-pools"></a>Správa trvalých fondů

Můžete spravovat existující trvalé fond na hello **fondy** kartě. Výběr fond hello seznamu se zobrazí aktuální stav fondu hello hello.

Z hello **fondy** kartě, můžete také odstranit hello fond a změnit velikost hello počet virtuálních počítačů ve fondu hello. Můžete změnit velikost tooavoid fondu too0 uzly by docházelo k náklady mezi úlohy.

![Zobrazení, změna velikosti a odstranění fondů](./media/batch-rendering-service/pools.png)

### <a name="configure-a-render-job-for-submission"></a>Konfigurace úlohy vykreslování pro odeslání

Po zadání parametrů hello hello fondu, který se spustí úlohu vykreslování hello konfiguraci hello úlohy, sám sebe. 

#### <a name="specify-scene-parameters"></a>Zadání parametrů scény

Hello Batch modulu plug-in zjistí, které modul vykreslování aktuálně používáte v Maya a nastavení na hello vykreslení hello zobrazí příslušné **odeslání** kartě. Tato nastavení zahrnují hello počáteční snímek, koncový snímek, předpona výstup a krok snímku. Hello scény souboru vykreslení nastavení můžete přepsat zadáním jiné nastavení v modulu plug-in hello. Změny, které provedete toohello nastavení modulu plug-in nejsou trvalou back toohello scény souboru vykreslení nastavení, umožní vám provádět změny na základě úlohy úlohy bez nutnosti tooreupload hello scény souboru.

Hello modulu plug-in varuje, že vám, zda text hello vykreslení modul, který jste vybrali v Maya není podporován.

Pokud jste při načítání nové scény hello modul plug-in je otevřený, klikněte na tlačítko hello **aktualizovat** aktualizaci tlačítko toomake zda hello nastavení.

#### <a name="resolve-asset-paths"></a>Překlad cest k prostředkům

Při načítání modulu plug-in hello kontroluje hello scény souboru pro všechny odkazy na externí soubor. Tyto odkazy se zobrazují v hello **prostředky** kartě. Pokud odkazované cesty nelze přeložit, modul plug-in hello pokusů o toolocate hello souboru v několik výchozích umístění, včetně:

- Hello umístění souboru scény hello 
- Hello aktuálního projektu _sourceimages_ adresáře
- Hello aktuální pracovní adresář. 

Pokud hello asset stále nelze nalézt, je uvedena ikona upozornění:

![Chybějící prostředky jsou zobrazené s ikonou upozornění](./media/batch-rendering-service/missing_assets.png)

Pokud znáte hello umístění odkaz na nerozpoznané souboru, klikněte na tlačítko hello upozornění, že ikona toobe výzva tooadd cesta hledání. potom modul plug-in Hello používá tento vyhledávací cesta tooattempt tooresolve všechny chybějící prostředky. Můžete přidat libovolný počet dalších cest pro hledání.

Když se odkaz přeloží, je uvedený s ikonou zeleného světla:

![Přeložené prostředky s ikonou zeleného světla](./media/batch-rendering-service/found_assets.png)

Pokud vaše scény vyžaduje další soubory nebyla zjištěna této hello modul plug-in, můžete přidat další soubory nebo adresáře. Použití hello **přidat soubory** a **přidat adresář** tlačítka. Pokud jste při načítání nové scény hello modul plug-in je otevřený, být zda tooclick **aktualizovat** scény hello tooupdate odkazy.

#### <a name="upload-assets-tooan-asset-project"></a>Nahrát prostředky tooan asset projektu

Když odešlete úlohu vykreslování hello odkazuje soubory zobrazené v hello **prostředky** karta se automaticky nahrané tooAzure úložiště jako projekt asset. Můžete také nahrát soubory prostředku hello nezávisle na úlohu vykreslování pomocí hello **nahrát** na hello tlačítko **prostředky** kartě hello asset je zadaný projekt hello **projektu**pole a pojmenované podle aktuálního projektu Maya hello ve výchozím nastavení. Při asset soubory jsou odeslány, struktura místního souboru hello zachována. 

Po nahrání může na prostředky odkazovat jakýkoli počet úloh vykreslování. Všechny nahrané prostředky jsou k dispozici tooany úlohu, která odkazuje na projekt hello asset, zda jsou součástí scény hello. toochange hello asset projekt odkazuje na položku Další úlohy, název hello změn v hello **projektu** pole hello **prostředky** kartě. Pokud existují odkazované soubory, které chcete tooexclude z nahrávání, zrušte výběr těchto počítačů pomocí hello zelené tlačítko vedle položky seznamu hello.

#### <a name="submit-and-monitor-hello-render-job"></a>Odeslání a monitorování hello vykreslení úlohy

Po nakonfigurování hello vykreslení úlohy v hello modul plug-in, klikněte na tlačítko hello **odeslat úlohu** na hello tlačítko **odeslání** kartě toosubmit hello úlohy tooBatch:

![Odeslat úlohu vykreslování hello](./media/batch-rendering-service/submit_job.png)

Můžete sledovat úlohy, která je v průběhu z hello **úlohy** karty v hello modulu plug-in. Vyberte úlohu z hello seznamu toodisplay hello aktuální stav úlohy hello. Můžete také použít tato karta toocancel a odstranění úlohy, a také toodownload hello výstupy a vykreslování protokoly. 

toodownload výstupy upravit hello **výstupy** pole tooset hello požadované cílový adresář. Klikněte na tlačítko hello ozubené kolečko ikonu toostart proces na pozadí, která sleduje hello úlohy a stáhne výstupy průběhu: 

![Zobrazení stavu úlohy a stažení výstupů](./media/batch-rendering-service/jobs.png)

Maya můžete zavřít a to bez přerušení hello proces stahování.

## <a name="next-steps"></a>Další kroky

toolearn Další informace o Batch, najdete v části [spustit vnitřně paralelní úlohy pomocí služby Batch](batch-technical-overview.md).
