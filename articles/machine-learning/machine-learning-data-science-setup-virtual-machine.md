---
title: "Nastavte virtuální počítač jako server IPython Poznámkový blok | Microsoft Docs"
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
ms.openlocfilehash: 66fd9e5573390ac6faeb82ad5b0f7ddb18d50a77
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="set-up-an-azure-virtual-machine-as-an-ipython-notebook-server-for-advanced-analytics"></a>Nastavení virtuálního počítače Azure jako serveru IPython Notebook pro pokročilé analýzy
Toto téma ukazuje, jak zřídit a nakonfigurovat virtuální počítač Azure pro pokročilou analýzu, který slouží jako součást prostředí vědecké účely data. Virtuální počítač Windows nakonfigurovaný s Podpora nástrojů, například Poznámkový blok IPython, Azure Storage Explorer, AzCopy, jakož i jiné nástroje, které jsou užitečné pro pokročilou analýzu projekty. Azure Storage Explorer a AzCopy, například poskytují pohodlný způsoby odesílání dat do úložiště objektů blob v Azure z místního počítače nebo se stáhne do místního počítače z úložiště objektů blob.

## <a name="create-vm"></a>Krok 1: Vytvoření pro obecné účely virtuální počítač Azure
Pokud už máte virtuální počítač Azure a chcete jen nastavení serveru na něm IPython Poznámkový blok, můžete tento krok přeskočit a přejít k [krok 2: Přidání koncového bodu pro poznámkové bloky IPython do existujícího virtuálního počítače](#add-endpoint).

Před zahájením procesu vytvoření virtuálního počítače na platformě Azure, budete muset určit velikost počítače, který je potřebný pro zpracování dat pro jejich projektu. Mít menší počítače méně paměti a méně jader procesoru, než větší počítače, ale jsou také levnější. Seznam typů počítačů a ceny, najdete v článku <a href="http://azure.microsoft.com/pricing/details/virtual-machines/" target="_blank">ceny služeb Virtual Machines </a> stránky

1. Přihlaste se k <a href="https://manage.windowsazure.com" target="_blank">portál Azure classic</a>a klikněte na tlačítko **nový** v levém dolním rohu. Bude překryvné okno. Vyberte **výpočetní** -> **VIRTUÁLNÍHO počítače** -> **FROM GALERIE**.
   
    ![Vytvořit pracovní prostor][24]
2. Vyberte jednu z následujících bitových kopií:
   
   * Windows Server 2012 R2 Datacenter
   * Windows Server Essentials Experience (Windows Server 2012 R2)
     
     Potom klikněte na šipku vpravo v pravém dolním přejít na další stránku konfigurace.
     
     ![Vytvořit pracovní prostor][25]
3. Zadejte název pro virtuální počítač, který chcete vytvořit, vyberte velikost počítače (výchozí: A3) na základě velikost dat na počítač se bude proces a jak výkonné, který se má počítač (velikost paměti a počet výpočetních jader) , zadejte uživatelské jméno a heslo pro tento počítač. Potom klikněte na šipku vpravo na Přejít na další stránku konfigurace.
   
    ![Vytvořit pracovní prostor][26]
4. Vyberte **oblasti nebo vztahů skupiny nebo virtuální síť** obsahující **účet úložiště** , že máte v úmyslu použít pro tento virtuální počítač a potom vyberte daný účet úložiště. Přidání koncového bodu v dolní části v **koncové body** pole tak, že zadáte název koncového bodu ("IPython" sem). Můžete vybrat libovolný řetězec, jako **název** koncový bod a jakékoliv celé číslo mezi 0 a 65536, který je **k dispozici** jako **veřejný PORT**. **PRIVÁTNÍ PORT** musí být **9999**. Měli byste **vyhnout** pomocí veřejného porty, které již byly přiřazeny pro internetové služby. <a href="http://www.chebucto.ns.ca/~rakerman/port-table.html" target="_blank">Porty pro internetové služby</a> poskytuje seznam portů, které byly přiřazeny a je nutno.
   
    ![Vytvořit pracovní prostor][27]
5. Kliknutím na značku zaškrtnutí zahájíte proces zřizování virtuálního počítače.
   
    ![Vytvořit pracovní prostor][28]

Může trvat 15-25 minut na dokončení procesu zřizování virtuálního počítače. Po vytvoření virtuálního počítače, jako by měl zobrazit stav tohoto počítače **systémem**.

![Vytvořit pracovní prostor][29]

## <a name="add-endpoint"></a>Krok 2: Přidání koncového bodu pro poznámkové bloky IPython do existujícího virtuálního počítače
Pokud jste vytvořili virtuální počítač podle pokynů v kroku 1, již byl přidán koncový bod pro IPython poznámkového bloku a můžete tento krok přeskočen.

Pokud virtuální počítač již existuje a je nutné přidat koncový bod pro IPython Poznámkový blok, který bude instalovat v kroku 3 pod prvním přihlášení k portálu Azure classic, vyberte virtuální počítač a přidání koncového bodu pro server IPython Poznámkový blok. Následující obrázek obsahuje snímek obrazovky portálu po koncový bod pro IPython poznámkového bloku byl přidán do virtuálního počítače s Windows.

![Vytvořit pracovní prostor][17]

## <a name="run-commands"></a>Krok 3: Instalace IPython Poznámkový blok a další podpůrné nástroje
Po vytvoření virtuálního počítače pomocí protokolu RDP (Remote Desktop) pro přihlášení k systému Windows virtuálního počítače. Pokyny najdete v tématu [postup Přihlaste se k Windows serveru spuštěný virtuální počítač](../virtual-machines/windows/classic/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json). Otevřete **příkazového řádku** (**není příkazové okno prostředí Powershell**) jako **správce** a spusťte následující příkaz.

    set script='https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/MachineSetup/Azure_VM_Setup_Windows.ps1'

    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString(%script%))"

Po dokončení instalace serveru IPython poznámkového bloku se spouští automaticky v *C:\\uživatelé\\\<uživatelské jméno\>\\dokumenty\\IPython poznámkových bloků*  adresáře.

Po zobrazení výzvy zadejte heslo pro IPython poznámkového bloku a heslo správce počítače. To umožňuje IPython poznámkového bloku ke spuštění jako služba na počítači.

## <a name="access"></a>Krok 4: Poznámkových bloků IPython přístup z webového prohlížeče
Pro přístup k serveru IPython poznámkového bloku, otevřete webového prohlížeče a vstup *https://&#60;virtual název DNS počítače >: & č. 60; veřejný port číslo >* do textového pole adresy URL. Zde *& č. 60; veřejný port číslo >* by měla být číslo portu, který jste zadali, pokud byl přidán koncový bod IPython Poznámkový blok.

*& Č. 60; název DNS virtuálního počítače >* najdete na portálu Azure classic. Po přihlášení k portálu classic, klikněte na tlačítko **virtuální počítače**, vyberte počítač, který jste vytvořili a potom vyberte **řídicí panel**, název DNS se zobrazí následujícím způsobem:

![Vytvořit pracovní prostor][19]

Upozornění oznamující, že se setkají *došlo k potížím s certifikátem zabezpečení tohoto webu* (Internet Explorer) nebo *připojení není privátní* (Chrome), jak je znázorněno v následující obrázky . Klikněte na tlačítko **pokračovat na tento web (nedoporučujeme)** (Internet Explorer) nebo **Upřesnit** a potom  **pokračovat na & č. 60;* Název DNS*> (unsafe) ** (Chrome) Chcete-li pokračovat. Pak zadejte heslo, které jste dříve zadaný pro přístup k IPython poznámkových bloků.

**Internet Explorer:**
![vytvořit pracovní prostor][20]

**Chrome:**
![vytvořit pracovní prostor][21]

Po přihlášení k IPython poznámkového bloku, adresář *DataScienceSamples* se zobrazí v prohlížeči. Tento adresář obsahuje ukázkové poznámkových bloků IPython, které Microsoftu pomáhají uživatelům provedení úlohy vědecké účely dat sdílí. Poznámkové bloky tyto ukázkové IPython jsou rezervovány z [ **úložiště GitHub** ](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks) na virtuální počítače během procesu instalace serveru IPython Poznámkový blok. Společnost Microsoft udržuje a často aktualizuje toto úložiště. Uživatelé můžou navštívit úložišti GitHub, abyste si nedávno aktualizovaného notebooky IPython ukázka.
![Vytvořit pracovní prostor][18]

## <a name="upload"></a>Krok 5: Nahrajte existující IPython poznámkového bloku z místního počítače k serveru IPython Poznámkový blok
Poznámkových bloků IPython poskytují snadný způsob pro uživatelům odesílat existujícího poznámkového bloku IPython na své místní počítače k serveru IPython poznámkového bloku na virtuálních počítačích. Po přihlášení do poznámkového bloku IPython ve webovém prohlížeči, klikněte na tlačítko do **directory** který poznámkového bloku IPython bude nahrán do. Potom vyberte soubor poznámkového bloku IPython .ipynb nahrát z místního počítače v **Průzkumníka souborů**a přetáhněte ji do poznámkového bloku IPython adresáře na webovém prohlížeči. Klikněte **nahrát** tlačítko Nahrát soubor .ipynb k serveru IPython Poznámkový blok. Ostatní uživatelé pak jej začít používat v z webových prohlížečů.

![Vytvořit pracovní prostor][22]

![Vytvořit pracovní prostor][23]

## <a name="shutdown"></a>Vypnout a zrušit přidělení virtuálního počítače, když není používán
Virtuální počítače Azure jsou cenově jako **platíte jenom pro co používáte**. Aby se zajistilo, že nejsou se fakturuje Pokud nepoužíváte virtuální počítač, musí být v **zastaveno (Deallocated)** stavu, když není používán.

> [!NOTE]
> Pokud vypnout virtuální počítač z ve virtuálním počítači (s použitím možnosti napájení systému Windows), virtuální počítač je zastavena, ale zůstává přidělené. Aby se zajistilo, není nadále účtovány poplatky, vždy zastaví virtuální počítače z [portál Azure classic](http://manage.windowsazure.com/). Můžete také zastavit virtuální počítač pomocí prostředí Powershell voláním **ShutdownRoleOperation** s "PostShutdownAction" rovno "StoppedDeallocated".
> 
> 

Vypnutí a zrušit přidělení virtuálního počítače:

1. Přihlaste se k [portál Azure classic](http://manage.windowsazure.com/) pomocí svého účtu.  
2. Vyberte **virtuální počítače** levém navigačním panelu.
3. V seznamu virtuálních počítačů, klikněte na název virtuálního počítače potom přejděte na stránku **řídicí panel** stránky.
4. V dolní části stránky klikněte na tlačítko **vypnutí**.

![Vypnutí virtuálního počítače][15]

Virtuální počítač bude zrušeno, ale nebyl odstraněn. Kdykoli z portálu Azure classic můžete restartovat virtuální počítač.

## <a name="your-azure-vm-is-ready-to-use-whats-next"></a>Virtuální počítač Azure je připravené k použití: co je další?
Virtuální počítač je nyní připraven k použití v cvičení vědecké účely vaše data. Virtuální počítač je taky připravené pro použití jako server IPython Poznámkový blok pro zkoumání a zpracování dat a dalších úloh ve spojení s Azure Machine Learning a vědecké účely procesu dat Team.

Další kroky v procesu Team dat. vědecké účely, jsou namapované v [učení cesta](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) a může obsahovat kroky, které přesun dat do HDInsight, proces a ukázkové ho existuje v rámci přípravy learning z dat pomocí Azure Machine Learning.

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
