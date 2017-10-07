---
title: "aaaSet virtuální počítač jako server poznámkového bloku IPython | Microsoft Docs"
description: "Nastavte si virtuální počítač Azure pro použití v prostředí datového vědecké účely IPython serveru pro pokročilou analýzu."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 818617c1-048e-49c2-b941-f9a983f93998
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: xibingao;bradsev
ms.openlocfilehash: 58386140ec7742ade1f7e183ec842a2b09b9dfca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-an-azure-virtual-machine-as-an-ipython-notebook-server-for-advanced-analytics"></a>Nastavení virtuálního počítače Azure jako serveru IPython Notebook pro pokročilé analýzy
Toto téma ukazuje, jak tooprovision a konfigurace virtuálního počítače Azure pro pokročilou analýzu, který slouží jako součást prostředí vědecké účely data. virtuální počítač Windows Hello nastavena podpora nástrojů, například Poznámkový blok IPython, Azure Storage Explorer, AzCopy, jakož i jiné nástroje, které jsou užitečné pro pokročilou analýzu projekty. Azure Storage Explorer a AzCopy, například poskytují pohodlný způsoby úložiště objektů blob tooAzure tooupload data z místního počítače nebo toodownload ho tooyour místního počítače z úložiště objektů blob.

## <a name="create-vm"></a>Krok 1: Vytvoření pro obecné účely virtuální počítač Azure
Pokud už máte virtuální počítač Azure a chcete jen tooset až serveru IPython poznámkového bloku na něm, můžete tento krok přeskočit a přejít příliš[krok 2: Přidání koncového bodu pro poznámkové bloky IPython tooan existující virtuální počítač](#add-endpoint).

Před zahájením hello proces vytváření virtuálního počítače na platformě Azure, musíte toodetermine hello velikost hello počítače, který je potřebný tooprocess hello data pro své projekty. Mít menší počítače méně paměti a méně jader procesoru, než větší počítače, ale jsou také levnější. Seznam typů počítačů a ceny, naleznete v části hello <a href="http://azure.microsoft.com/pricing/details/virtual-machines/" target="_blank">ceny služeb Virtual Machines </a> stránky

1. Přihlaste se příliš<a href="https://manage.windowsazure.com" target="_blank">portál Azure classic</a>a klikněte na tlačítko **nový** v levém dolním rohu hello. Bude překryvné okno. Vyberte **výpočetní** -> **VIRTUÁLNÍHO počítače** -> **FROM GALERIE**.
   
    ![Vytvořit pracovní prostor][24]
2. Vyberte jednu z hello následující bitové kopie:
   
   * Windows Server 2012 R2 Datacenter
   * Windows Server Essentials Experience (Windows Server 2012 R2)
     
     Potom klikněte na hello šipka vpravo na hello nižší správné toogo hello další konfigurační stránku.
     
     ![Vytvořit pracovní prostor][25]
3. Zadejte název pro virtuální počítač hello chcete toocreate, vyberte hello velikost hello počítače (výchozí: A3) na základě velikosti hello hello data hello počítače je tooprocess probíhající a jak výkonné chcete hello počítač toobe (paměti velikost a hello počet výpočetních jader) , zadejte uživatelské jméno a heslo pro počítač hello. Potom klikněte na hello šipka vpravo toogo toohello další konfigurační stránku.
   
    ![Vytvořit pracovní prostor][26]
4. Vyberte hello **oblasti nebo vztahů skupiny nebo virtuální síť** obsahující hello **účet úložiště** plánování toouse pro tento virtuální počítač a potom vyberte daný účet úložiště. Přidání koncového bodu v dolní části hello v hello **koncové body** pole tak, že zadáte název hello hello koncového bodu ("IPython" sem). Můžete vybrat libovolný řetězec jako hello **název** hello koncový bod a jakékoliv celé číslo mezi 0 a 65536, který je **k dispozici** jako hello **veřejný PORT**. Hello **PRIVÁTNÍ PORT** má toobe **9999**. Měli byste **vyhnout** pomocí veřejného porty, které již byly přiřazeny pro internetové služby. <a href="http://www.chebucto.ns.ca/~rakerman/port-table.html" target="_blank">Porty pro internetové služby</a> poskytuje seznam portů, které byly přiřazeny a je nutno.
   
    ![Vytvořit pracovní prostor][27]
5. Klikněte na tlačítko hello zaškrtnutí toostart hello virtuálního počítače procesu zřizování.
   
    ![Vytvořit pracovní prostor][28]

Může trvat 15-25 minut toocomplete hello virtuálního počítače procesu zřizování. Po vytvoření virtuálního počítače hello hello stav tohoto počítače by měl zobrazit jako **systémem**.

![Vytvořit pracovní prostor][29]

## <a name="add-endpoint"></a>Krok 2: Přidání koncového bodu pro poznámkové bloky IPython tooan existující virtuální počítač
Pokud jste vytvořili hello virtuálního počítače podle pokynů hello v kroku 1, již byla přidána hello koncový bod pro IPython poznámkového bloku a můžete tento krok přeskočen.

Pokud již existuje hello virtuální počítač, a potřebujete tooadd koncový bod pro IPython Poznámkový blok, který budete instalovat v kroku 3 níže, první přihlášení tooAzure portálu classic, vyberte hello virtuální počítač a přidání hello koncového bodu pro server IPython Poznámkový blok. Hello následující obrázek obsahuje snímek obrazovky aplikace hello portál po hello koncový bod pro poznámkový blok IPython přidala tooa Windows virtuálního počítače.

![Vytvořit pracovní prostor][17]

## <a name="run-commands"></a>Krok 3: Instalace IPython Poznámkový blok a další podpůrné nástroje
Po vytvoření virtuálního počítače hello použijte na virtuální počítač Windows toohello toolog protokol RDP (Remote Desktop). Pokyny najdete v tématu [jak tooLog na tooa virtuální počítač spuštěný Windows Server](../virtual-machines/windows/classic/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json). Otevřete hello **příkazového řádku** (**není hello prostředí Powershell příkazové okno**) jako **správce** a hello spusťte následující příkaz.

    set script='https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/MachineSetup/Azure_VM_Setup_Windows.ps1'

    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString(%script%))"

Po instalaci hello dokončení hello poznámkového bloku IPython server se spustí automaticky v hello *C:\\uživatelé\\\<uživatelské jméno\>\\dokumenty\\IPython Poznámkových bloků* adresáře.

Po zobrazení výzvy zadejte heslo pro hello IPython Poznámkový blok a hello heslo správce počítače hello. To umožňuje hello poznámkového bloku IPython toorun jako služba na počítači hello.

## <a name="access"></a>Krok 4: Poznámkových bloků IPython přístup z webového prohlížeče
hello tooaccess server IPython Poznámkový blok otevřete webového prohlížeče a vstup *https://&#60;virtual název DNS počítače >: & č. 60; veřejný port číslo >* hello URL textového pole. Zde hello *& č. 60; veřejný port číslo >* by měla být číslo portu hello jste zadali při hello poznámkového bloku IPython koncový bod byl přidán.

Hello *& č. 60; název DNS virtuálního počítače >* najdete na portálu Azure classic hello. Po protokolování na portálu classic toohello, klikněte na tlačítko **virtuální počítače**, vyberte počítač hello jste vytvořili a potom vyberte **řídicí panel**, název DNS hello zobrazí následujícím způsobem:

![Vytvořit pracovní prostor][19]

Upozornění oznamující, že se setkají *došlo k potížím s certifikátem zabezpečení tohoto webu* (Internet Explorer) nebo *připojení není privátní* (Chrome), jak je znázorněno v následující hello Následující obrázky. Klikněte na tlačítko **pokračovat toothis web (nedoporučujeme)** (Internet Explorer) nebo **Upřesnit** a potom  **pokračovat příliš & č. 60;* Název DNS*> (unsafe) ** toocontinue (Chrome). Pak zadejte heslo hello je zadaný starší tooaccess hello IPython poznámkových bloků.

**Internet Explorer:**
![vytvořit pracovní prostor][20]

**Chrome:**
![vytvořit pracovní prostor][21]

Po přihlášení toohello IPython Poznámkový blok, adresář *DataScienceSamples* se zobrazí v prohlížeči hello. Tento adresář obsahuje ukázkové poznámkových bloků IPython, které sdílí Microsoft toohelp uživatelé chování datové vědy úlohy. Poznámkové bloky tyto ukázkové IPython jsou rezervovány z [ **úložiště GitHub** ](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks) toohello virtuálních počítačů během hello procesu instalace serveru IPython Poznámkový blok. Společnost Microsoft udržuje a často aktualizuje toto úložiště. Uživatelé můžou navštívit hello Githubu úložiště tooget hello nedávno aktualizovaný ukázkový IPython poznámkových bloků.
![Vytvořit pracovní prostor][18]

## <a name="upload"></a>Krok 5: Nahrát na server existujícího poznámkového bloku IPython z místního počítače toohello server IPython Poznámkový blok
Poznámkových bloků IPython poskytují snadný způsob pro uživatele tooupload existujícího poznámkového bloku IPython na své místní počítače toohello poznámkového bloku IPython server hello virtuálními počítači. Po přihlášení toohello IPython Poznámkový blok ve webovém prohlížeči, klikněte na tlačítko do hello **directory** této hello IPython Poznámkový blok bude nahrán do. Pak vyberte tooupload .ipynb soubor poznámkového bloku IPython z místního počítače hello v hello **Průzkumníka souborů**a přetáhněte ji toohello poznámkového bloku IPython directory hello webový prohlížeč. Klikněte na tlačítko hello **nahrát** tlačítko tooupload hello .ipynb souboru toohello server IPython Poznámkový blok. Ostatní uživatelé pak jej začít používat v z webových prohlížečů.

![Vytvořit pracovní prostor][22]

![Vytvořit pracovní prostor][23]

## <a name="shutdown"></a>Vypnout a zrušit přidělení virtuálního počítače, když není používán
Virtuální počítače Azure jsou cenově jako **platíte jenom pro co používáte**. tooensure, která nejsou se účtují, pokud nepoužíváte virtuální počítač, má toobe v hello **zastaveno (Deallocated)** stavu, když není používán.

> [!NOTE]
> Pokud vypnete hello virtuálního počítače z uvnitř hello virtuální počítač (pomocí možnosti napájení systému Windows), hello virtuálního počítače je zastavena, ale přidělené zůstanou. tooensure nebudete pokračovat toobe účtují, vždy zastavit virtuální počítače z hello [portál Azure classic](http://manage.windowsazure.com/). Můžete také zastavit hello virtuálních počítačů pomocí prostředí Powershell voláním **ShutdownRoleOperation** s "PostShutdownAction" rovnat příliš "StoppedDeallocated".
> 
> 

tooshut dolů a zrušit přidělení hello virtuálního počítače:

1. Přihlaste se toohello [portál Azure classic](http://manage.windowsazure.com/) pomocí svého účtu.  
2. Vyberte **virtuální počítače** z hello levém navigačním panelu.
3. V seznamu hello virtuálních počítačů, klikněte na název virtuálního počítače a přejděte toohello hello **řídicí panel** stránky.
4. V dolní části hello hello stránky, klikněte na tlačítko **vypnutí**.

![Vypnutí virtuálního počítače][15]

Hello virtuálního počítače budou navrácena, ale nebyl odstraněn. Kdykoli hello portál Azure classic můžete restartovat virtuální počítač.

## <a name="your-azure-vm-is-ready-toouse-whats-next"></a>Virtuální počítač Azure je připravené toouse: co je další?
Virtuální počítač je nyní připraven toouse v cvičení vědecké účely vaše data. virtuální počítač Hello je také připravený k použití jako server služby IPython Poznámkový blok pro zkoumání hello a zpracování dat a další úkoly ve spojení s Azure Machine Learning a hello proces vědecké účely dat týmu.

Hello další kroky v procesu vědecké účely Team dat jsou namapované v hello hello [učení cesta](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) a může obsahovat kroky, které přesun dat do HDInsight, proces a ukázkové ho existuje v rámci přípravy učení se z dat hello s počítači Azure Učení.

[15]: ./media/machine-learning-data-science-setup-virtual-machine/vmshutdown.png
[17]: ./media/machine-learning-data-science-setup-virtual-machine/add-endpoints-after-creation.png
[18]: ./media/machine-learning-data-science-setup-virtual-machine/sample-ipnbs.png
[19]: ./media/machine-learning-data-science-setup-virtual-machine/dns-name-and-host-name.png
[20]: ./media/machine-learning-data-science-setup-virtual-machine/browser-warning-ie.png
[21]: ./media/machine-learning-data-science-setup-virtual-machine/browser-warning.png
[22]: ./media/machine-learning-data-science-setup-virtual-machine/upload-ipnb-1.png
[23]: ./media/machine-learning-data-science-setup-virtual-machine/upload-ipnb-2.png
[24]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-1.png
[25]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-2.png
[26]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-3.png
[27]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-4.png
[28]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-5.png
[29]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-6.png
