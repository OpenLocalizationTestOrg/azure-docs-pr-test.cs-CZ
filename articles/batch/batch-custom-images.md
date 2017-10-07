---
title: "fondy Azure Batch aaaProvision z vlastních bitových kopií | Microsoft Docs"
description: "Můžete vytvořit dávce fondu tooprovision vlastní image výpočetních uzlů, které obsahují hello software a data, která je nutné pro vaši aplikaci. Vlastní Image jsou účinný způsob tooconfigure výpočetní uzly toorun vaše úloh služby Batch."
services: batch
author: tamram
manager: timlt
ms.service: batch
ms.topic: article
ms.date: 08/07/2017
ms.author: tamram
ms.openlocfilehash: 5cb698ee90f7d3ec9ffe69fa4dc602132c3f7569
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-a-custom-image-toocreate-a-pool-of-virtual-machines"></a>Použít vlastní image toocreate fondu virtuálních počítačů

Při vytváření fondu virtuálních počítačů v Azure Batch, zadejte bitovou kopii virtuálního počítače (VM), která poskytuje hello operační systém pro každý výpočetní uzel ve fondu hello. Fond virtuálních počítačů můžete vytvořit buď pomocí image Azure Marketplace, nebo zadáním vlastní image virtuálního pevného disku, který jste připravili. Když zadáte vlastní image, máte kontrolu nad konfiguraci hello operačního systému v době hello, který je zajištěný každý výpočetní uzel. Vlastní image může také obsahovat aplikace a referenčních dat, které jsou dostupné na hello výpočetního uzlu, jakmile je zřízený.

Použití vlastní image můžete ušetřit čas při získávání váš fond výpočetních uzlů připraveni toorun úlohy Batch. Zatímco můžete vždy použít image Azure Marketplace a instalovat software na každém výpočetním uzlu po byla zřízení, může být tento přístup míň efektivní než pomocí vlastní image. 

Některé z důvodů toouse vlastní image, který je nakonfigurovaný pro váš scénář patří museli:

- **Konfigurace hello operační systém (OS)** žádnou zvláštní konfiguraci operačního systému hello lze provést na hello vlastní image. 
- **Nainstalujte velké aplikace.** Instalace aplikace na vlastní image je efektivnější než je nainstalujete na každém výpočetním uzlu po je zřízený.
- **Zkopírujte poměrně velké množství dat.** Pokud jsou hello data zkopírovaný toohello vlastní image, potřebuje pouze toobe zkopírovat jednou, místo tooeach výpočetním uzlu, ukládání čas a šířku pásma.
- **Během procesu instalace hello restartování hello virtuálních počítačů.** Restartování hello virtuálních počítačů může být časově náročné, zvláště pokud máte počet výpočetních uzlů.

## <a name="prerequisites"></a>Požadavky

- **Účet Batch vytvořit s režimem přidělení fondu uživatele předplatné hello.** toouse vlastní image virtuálního počítače fondy tooprovision, vytvoření účtu Batch s hello uživatele předplatné [režim přidělení fondu](batch-api-basics.md#pool-allocation-mode). Při tomto režimu jsou fondy Batch přidělí hello předplatné, které se nachází hello účet. V tématu hello [účet](batch-api-basics.md#account) kapitoly [rozsáhlé paralelní vývoj výpočetní řešení pomocí služby Batch](batch-api-basics.md) informace o nastavení režimu přidělení fondu hello při vytváření účtu Batch.

- **Účet služby Azure Storage.** toocreate fondu virtuálních počítačů pomocí vlastní image, potřebujete účet Azure Storage standardní, obecný v hello stejném předplatném, oblasti. Pokud vytvoříte vlastní image z virtuálního počítače Azure, bude zkopírujte hello bitové kopie toohello účet úložiště, kde se nachází disk s operačním systémem hello Virtuálního počítače, a nebudete potřebovat toocreate účet samostatného úložiště. 
    
## <a name="prepare-a-custom-image"></a>Příprava vlastní image

tooprepare vlastní image pro použití se službou Batch, musí generalize existující instalace systému Linux nebo Windows. Generalizací instalace operačního systému odebere informace specifické pro počítač. Výsledkem Hello je obrázek, který lze nainstalovat na jiných počítačích nebo virtuálních počítačů.  

> [!IMPORTANT]
> Batch aktuálně nepodporuje použití Azure spravovat obrázky tooprovision fondu. vlastní image Hello použijete tooprovision fondu musí být uložený ve službě Azure Storage. 
>
> Při přípravě vlastní image, mějte na paměti hello následující body:
> - Ujistěte se, hello základní OS bitovou kopii pomocí tooprovision fondech Batch nemá žádné předem nenainstalovali rozšíření Azure, jako je například rozšíření vlastních skriptů hello. Pokud hello image obsahuje předem nainstalovaná rozšíření, Azure setkat s problémy, které nasazení hello virtuálních počítačů.
> - Ujistěte se, že hello základní image OS, které zadáte, že používá hello výchozí dočasné jednotce jako hello agenta uzlu Batch aktuálně očekává hello výchozí dočasné jednotce.
>
>

Všechny existující připravené systému Windows nebo Linux bitové kopie můžete použít jako vlastní image. Například pokud chcete toouse místní bitové kopie, můžete odeslat hello image tooan Azure Storage účtu, který je v hello stejném předplatném, oblasti jako váš účet Batch pomocí [AzCopy](../storage/storage-use-azcopy.md) nebo jiný nástroj pro odesílání.

Můžete také připravit vlastní image z nového nebo existujícího virtuálního počítače Azure. Pokud vytváříte nový virtuální počítač, můžete image Azure Marketplace použít jako základní bitová kopie hello pro vlastní bitovou kopii a pak jej přizpůsobit. toocustomize hello základní image, vytvořte virtuální počítač Azure a přidejte aplikace nebo data tooit. Potom generalize hello tooserve virtuálního počítače jako vlastní image a uložte jej tooAzure úložiště. 

tooprepare vlastní image z virtuálního počítače Azure, postupujte takto:

1. Vytvoření **nespravované** virtuálního počítače Azure z image Azure Marketplace. Azure Marketplace obsahuje bitové kopie pro obě [Windows](../virtual-machines/windows/quick-create-portal.md) a [Linux](../virtual-machines/linux/quick-create-portal.md).
    
    V kroku 3 hello proces vytváření virtuálního počítače, ověřte, že jste vybrali **ne** pro **úložiště: použijte spravované disky** možnost. Také si poznamenejte hello název účtu úložiště pro disk s operačním systémem hello Virtuálního počítače, protože tento účet úložiště se taky, kde Azure uloží vlastní bitovou kopii:

    ![Vytvoření nespravované virtuálního počítače a název účtu úložiště hello Poznámka](media/batch-custom-images/vm-create-storage.png)
 
2. Dokončení procesu hello vytvoření virtuálního počítače a počkejte na její toobe přidělené v Azure. Tady je obrázek, který zobrazuje v hello portál Azure hello spuštěný virtuální počítač:

    ![Vytvoření virtuálního počítače z image pořízenou prostřednictvím marketplace](media/batch-custom-images/vm-status-running.png)

3. Jakmile je spuštěna hello virtuálních počítačů, připojte tooit prostřednictvím protokolu RDP (pro Windows) nebo SSH (pro Linux). Nainstalujte potřebný software nebo zkopírujte požadované data a pak generalize hello virtuálních počítačů. Postupujte podle kroků hello popsaných v [generalizace hello virtuálních počítačů](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sa-copy-generalized.md#generalize-the-vm). 
   
4. Postupujte podle kroků hello příliš[přihlásit tooAzure prostředí PowerShell](../virtual-machines/windows/sa-copy-generalized.md#log-in-to-azure-powershell). tooinstall prostředí Azure PowerShell najdete v části [Přehled prostředí Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.2.0). 

5. Potom postupujte podle kroků hello příliš[Deallocate hello virtuálních počítačů a sadu hello stavu toogeneralized](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sa-copy-generalized#deallocate-the-vm-and-set-the-state-to-generalized). 

    V hello portálu Azure Všimněte si, že virtuální počítač navrácený hello:

    ![Ujistěte se, že virtuální počítač navrácený hello](media/batch-custom-images/vm-status-deallocated.png)

6.  Vytvoření a uložení hello virtuálních počítačů bitové kopie tooAzure úložiště pomocí hello [uložit AzureRmVMImage](https://docs.microsoft.com/powershell/module/azurerm.compute/save-azurermvmimage) rutiny prostředí PowerShell. Postupujte podle pokynů hello popsané v [vytvořit hello image](../virtual-machines/windows/sa-copy-generalized.md#create-the-image).
    
    účet úložiště Azure toohello vytvořili při vytvoření hello virtuálních počítačů, jak je znázorněno v kroku 1 tohoto postupu je uložen obrázek Hello virtuálních počítačů. Hello uložit AzureRmVMImage rutina uloží hello image toohello **systému** kontejneru v daném účtu úložiště. Hello `-DestinationContainername` parametr názvy virtuální adresáře v rámci hello **systému** kontejneru. Hello `-VHDNamePrefix` parametr určuje předponu pro název objektu blob hello. Tato předpona je název objektu blob přidá jako předpona toohello s pomlčkou. 

    Předpokládejme například, že volání AzureRmVMImage uložit s hello následující parametry:  

        Save-AzureRmVMImage -ResourceGroupName sample-resource-group -Name vm-custom-image -DestinationContainerName batchimages -VHDNamePrefix custom -Path C:\Temp\Images\vm-custom-image.json

    výsledný obraz Hello je uložena toohello umístění a název objektu blob, které jsou zobrazeny zde:

    ![Umístění uloženého virtuálního pevného disku v kontejneru systému](media/batch-custom-images/vhd-in-vm-storage-account.png)

    > [!NOTE]
    > Azure nespravované virtuálního počítače vytvoří několik účtů úložiště pro jiné účely. Pokud není poznamenejte si název hello hello kontejner úložiště pro disk hello operačního systému při hello byl vytvořen virtuální počítač, pak Najít účet hello přidruženého úložiště, který obsahuje hello **systému** kontejneru. Procházet hello **systému** kontejneru toofind hello vlastní image pomocí hello hodnot, které jste zadali pro hello **uložit AzureRmVMImage** příkaz.

## <a name="create-a-pool-from-a-custom-image-in-hello-portal"></a>Vytvořit fond z vlastní image hello portálu

Jakmile jste uložili vlastní bitovou kopii a znáte jeho umístění, můžete vytvořit fondu služby Batch z této bitové kopie. Postupujte podle těchto kroků toocreate fond z hello portálu Azure:

1. Přejděte tooyour účtu Batch na portálu Azure hello. Tento účet musí být v hello stejného předplatného a oblasti jako úložiště hello účtu obsahující hello vlastní image. 
2. V hello **nastavení** okno na hello vlevo, vyberte hello **fondy** položku nabídky.
3. V hello **fondy** okno, vyberte hello **přidat** příkaz.
4. Na hello **přidat fond** vyberte **vlastní obrázek (Linux nebo Windows)** z hello **typ obrázku** rozevíracího seznamu. Hello portálu se zobrazí hello **vlastní Image** výběr. Přejděte toohello účet úložiště, kde se nachází vlastní image, a vyberte hello požadované virtuální pevný disk z kontejneru hello. 
5. Vyberte správný hello **vydavatele nebo nabídka nebo Sku** pro vlastního virtuálního pevného disku.
6. Zadejte zbývající hello požadované nastavení, včetně hello **velikost uzlu**, **cílové uzly vyhrazené**, a **nízkou prioritu uzly**, stejně jako všechny potřeby volitelná nastavení.

    Například pro Microsoft Windows Server Datacenter 2016 vlastní image, hello **přidat fond** okno se zobrazí, jak je vidět tady:

    ![Přidejte fond z vlastní bitovou kopii systému Windows](media/batch-custom-images/add-pool-custom-image.png)
  
toocheck zda existující fond je založena na vlastní image, najdete v části hello **operačního systému** vlastnost v hello prostředků souhrnné části hello **fondu** okno. Pokud fond hello byl vytvořen z vlastní image, je nastaven příliš**vlastní Image virtuálního počítače**.

Zobrazí se všechny vlastní VHD přidružený fond na hello fondu **vlastnosti** okno.
 
## <a name="next-steps"></a>Další kroky

- Podrobnější přehled služby Batch, najdete v tématu [rozsáhlé paralelní vývoj výpočetní řešení pomocí služby Batch](batch-api-basics.md).
- Informace o vytvoření účtu Batch najdete v tématu [vytvořit dávkový účet s hello portál Azure](batch-account-create-portal.md).
