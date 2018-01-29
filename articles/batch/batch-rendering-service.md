---
title: "Služba Azure Batch Rendering – vykreslování na úrovni cloudu | Dokumentace Microsoftu"
description: "Úlohy vykreslování na virtuálních počítačích Azure přímo z aplikace Maya a s platbami za použití."
services: batch
author: v-dotren
manager: timlt
ms.service: batch
ms.topic: hero-article
ms.date: 09/14/2017
ms.author: danlep
ms.openlocfilehash: aab68b4275edc48e1726b26a7400bbfdd8d40d6f
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/11/2017
---
# <a name="get-started-with-the-batch-rendering-service"></a>Začínáme se službou Batch Rendering

Služba Azure Batch Rendering nabízí možnosti vykreslování škálované na cloud a s platbami za použití. Služba Batch Rendering zajišťuje plánování úloh a jejich zařazování do fronty, správu chyb a opakování a automatické škálování úloh vykreslování. Služba Batch Rendering podporuje aplikace [Autodesk Maya](https://www.autodesk.com/products/maya/overview), [3ds Max](https://www.autodesk.com/products/3ds-max/overview), [Arnold](https://www.autodesk.com/products/arnold/overview) a [V-Ray](https://www.chaosgroup.com/vray/maya). Modul plug-in služby Batch pro aplikaci Maya 2017 umožňuje snadné spouštění úloh vykreslování v Azure přímo z plochy.

V případě aplikací Maya a 3ds Max můžete úlohy spouštět pomocí desktopové aplikace [BatchLabs](https://github.com/Azure/BatchLabs) nebo [šablon rozhraní příkazového řádku služby Batch](batch-cli-templates.md). Pomocí rozhraní příkazového řádku služby Azure Batch můžete spouštět úlohy Batch bez psaní kódu. Místo toho můžete k vytvoření úkolů, úloh a fondů Batch použít šablony. Další informace najdete v tématu [Použití šablon rozhraní příkazového řádku služby Azure Batch a přenos souborů](batch-cli-templates.md).


## <a name="supported-applications"></a>Podporované aplikace

Služba Batch Rendering aktuálně podporuje následující aplikace:

- Autodesk Maya
- Autodesk 3ds Max
- Autodesk Arnold pro Maya
- Autodesk Arnold pro 3ds Max
- Chaos Group V-Ray pro Maya
- Chaos Group V-Ray pro 3ds Max

## <a name="prerequisites"></a>Požadavky

Pokud chcete používat službu Batch Rendering, potřebujete:

- [Účet Azure](https://azure.microsoft.com/free/).
- **Účet Azure Batch.** Pokyny k vytvoření účtu Batch na webu Azure Portal najdete v tématu [Vytvoření účtu Batch pomocí webu Azure Portal](batch-account-create-portal.md).
- **Účet služby Azure Storage.** Ve službě Azure Storage se ukládají prostředky používané pro vaši úlohu vykreslování. Účet úložiště můžete vytvořit automaticky při nastavování účtu Batch. Můžete také použít existující účet úložiště. Další informace o účtech služby Storage najdete v tématu [Vytvoření, správa nebo odstranění účtu úložiště na webu Azure Portal](https://docs.microsoft.com/azure/storage/storage-create-storage-account).
- **BatchLabs** (volitelné). [BatchLabs](https://azure.github.io/BatchLabs) je bezplatný a samostatný klientský nástroj s bohatými funkcemi, který pomáhá vytvářet, ladit a monitorovat aplikace Azure Batch. Přestože se nevyžaduje k používání služby vykreslování, představuje užitečnou možnost vývoje a ladění řešení Batch.

Pokud chcete používat modul plug-in služby Batch pro aplikaci Maya, potřebujete:

- [Autodesk Maya 2017](https://www.autodesk.com/products/maya/overview).
- Podporovaný renderer, například Arnold pro Maya nebo V-Ray pro Maya.

## <a name="basic-batch-concepts"></a>Koncepty služby Batch úrovně Basic

Než začnete používat službu Batch Rendering, je vhodné se seznámit s několika koncepty služby Batch, mezi které patří výpočetní uzly, fondy a úlohy. Další obecné informace o službě Azure Batch najdete v tématu [Spouštění vnitřně paralelních úloh pomocí služby Batch](batch-technical-overview.md).

### <a name="pools"></a>Fondy

Batch je služba platformy pro spouštění úloh náročných na výpočetní výkon, jako je vykreslování, ve **fondu** **výpočetních uzlů**. Každý výpočetní uzel ve fondu je virtuální počítač Azure s Linuxem nebo Windows. 

Další informace o fondech a výpočetních uzlech služby Batch najdete v částech [Fond](batch-api-basics.md#pool) a [Výpočetní uzel](batch-api-basics.md#compute-node) v tématu [Vývoj rozsáhlých paralelních výpočetních řešení pomocí služby Batch](batch-api-basics.md).

### <a name="jobs"></a>Úlohy

**Úloha** služby Batch je kolekce úkolů, které se spouštějí na výpočetních uzlech ve fondu. Když odešlete úlohu vykreslování, služba Batch úlohu rozdělí na úkoly a ty distribuuje ke spuštění do výpočetních uzlů ve fondu.

Pomocí webu [Azure Portal](https://ms.portal.azure.com/) můžete monitorovat úlohy a diagnostikovat úkoly, které selhaly, stažením aplikačních protokolů a vzdáleným připojením k jednotlivým virtuálním počítačům přes RDP nebo SSH. Správu, monitorování a ladění můžete provádět také pomocí [nástroje BatchLabs](https://azure.github.io/BatchLabs).

Další informace o úlohách služby Batch najdete v části [Úloha](batch-api-basics.md#job) v tématu [Vývoj rozsáhlých paralelních výpočetních řešení pomocí služby Batch](batch-api-basics.md).

## <a name="options-for-provisioning-required-applications"></a>Možnosti zřízení požadovaných aplikací

V případě potřeby je možné pro vykreslení úlohy vyžádat více aplikací, například kombinaci Maya a Arnold nebo 3ds Max a V-Ray, stejně jako další moduly plug-in třetích stran. Zákazníci si navíc můžou vyžádat konkrétní verze těchto aplikací. Proto je k dispozici několik metod pro zřizování požadovaných aplikací a softwaru:

### <a name="pre-configured-vm-images"></a>Předkonfigurované image virtuálních počítačů

Azure poskytuje image Windows a Linuxu s předinstalovanými samostatnými verzemi aplikací Maya, 3ds Max, Arnold a V-Ray připravenými k použití. Tyto image můžete vybrat při vytváření fondu na webu [Azure Portal](https://portal.azure.com), v modulu plug-in Maya nebo v nástroji [BatchLabs](https://azure.github.io/BatchLabs).

Na webu Azure Portal a v nástroji BatchLabs můžete některou k imagí virtuálních počítačů s předinstalovanými aplikacemi nainstalovat následujícím způsobem: V části Fondy vašeho účtu Batch vyberte **Nový** a pak v části **Přidat fond** z rozevíracího seznamu **Typ image** vyberte **Grafika a Vykreslování (Linux/Windows)**:

![Výběr typu image pro účet Batch](./media/batch-rendering-service/add-pool.png)

Přejděte dolů, kliknutím na **Licencování grafiky a vykreslování** otevřete okno **Zvolte licence** a vyberte jednu nebo více licencí na software:

![Výběr licence na grafiku a vykreslování pro fond](./media/batch-rendering-service/graphics-licensing.png)

Tady jsou konkrétní zadané verze licencí:

- Maya 2017
- 3ds Max 2018
- Arnold pro Maya 5.0.1.1
- Arnold pro 3ds Max 1.0.836
- V-Ray pro Maya 3.52.03
- V-Ray pro 3ds Max 3.60.01

### <a name="custom-images"></a>Vlastní image

Azure Batch umožňuje zadání vlastní image. Pomocí této možnosti můžete na virtuálním počítači nakonfigurovat konkrétní aplikace a verze, které požadujete. Další informace najdete v tématu popisujícím [použití vlastní image k vytvoření fondu virtuálních počítačů](https://docs.microsoft.com/azure/batch/batch-custom-images). Poznámka: Společnosti Autodesk a Chaos Group nakonfigurovaly aplikace Arnold a V-Ray k ověření u naší vlastní licenční služby. Budete muset zajistit, že máte verze těchto aplikací s touto podporou, jinak licencování s platbami za použití nebude fungovat. U aplikací Maya a 3ds Max toto ověření licencí není nutné, protože aktuální publikované verze při bezobslužném spouštění (v dávkovém režimu nebo v režimu příkazového řádku) nevyžadují licenční server. Pokud si u této možnosti nejste jisti dalším postupem, kontaktujte podporu Azure.

## <a name="options-for-submitting-a-render-job"></a>Možnosti odeslání úlohy vykreslování

V závislosti na 3D aplikaci, kterou používáte, existují pro odeslání úloh vykreslování do služby různé možnosti:

### <a name="maya"></a>Maya

V případě aplikace Maya můžete použít:

- [Modul plug-in služby Batch pro Maya](https://docs.microsoft.com/azure/batch/batch-rendering-service#use-the-batch-plug-in-for-maya-to-submit-a-render-job)
- Desktopovou aplikaci [BatchLabs](https://azure.github.io/BatchLabs)
- [Šablony rozhraní příkazového řádku služby Batch](batch-cli-templates.md)

### <a name="3ds-max"></a>3ds Max

V případě aplikace 3ds Max můžete použít:

- Desktopovou aplikaci [BatchLabs](https://azure.github.io/BatchLabs) (pokyny k použití šablon BatchLabs pro 3ds Max najdete v úložišti [BatchLabs-data](https://github.com/Azure/BatchLabs-data/tree/master/ncj/3dsmax) na GitHubu)
- [Šablony rozhraní příkazového řádku služby Batch](batch-cli-templates.md)

Šablony Batch Labs pro 3ds Max umožňují pomocí služby Azure Batch Rendering vykreslovat scény VRay a Arnold. Existují dvě varianty šablon pro VRay a Arnold, jedna pro standardní scény a jedna pro složitější scény, které vyžadují soubor cest 3ds Max k prostředkům a texturám (soubor .mxp). Další informace o šablonách BatchLabs pro 3ds Max najdete v úložišti [BatchLabs-data](https://github.com/Azure/BatchLabs-data/tree/master/ncj/3dsmax) na GitHubu.

Kromě toho můžete pomocí sady [Batch Python SDK](https://docs.microsoft.com/azure/batch/batch-python-tutorial) integrovat službu vykreslování do svého existujícího kanálu.


## <a name="use-the-batch-plug-in-for-maya-to-submit-a-render-job"></a>Odeslání úlohy vykreslování pomocí modulu plug-in služby Batch pro aplikaci Maya

S modulem plug-in služby Batch pro aplikaci Maya můžete úlohu odeslat do služby Batch Rendering přímo z aplikace Maya. Následující části popisují, jak v modulu plug-in nakonfigurovat úlohu a pak ji odeslat. 

### <a name="load-the-batch-plug-in-for-maya"></a>Načtení modulu plug-in služby Batch pro Maya

Modul plug-in služby Batch je k dispozici na [GitHubu](https://github.com/Azure/azure-batch-maya/releases). Rozbalte archiv do libovolného adresáře. Modul plug-in můžete načíst přímo z adresáře *azure_batch_maya*.

Načtení modulu plug-in v aplikaci Maya:

1. Spusťte aplikaci Maya.
2. Otevřete **Window** (Okno) > **Settings/Preferences** (Nastavení/Předvolby) > **Plug-in Manager** (Správce modulů plug-in).
3. Klikněte na **Browse** (Procházet).
4. Přejděte k souboru *azure_batch_maya/plug-in/AzureBatch.py* a vyberte ho.

### <a name="authenticate-access-to-your-batch-and-storage-accounts"></a>Ověření přístupu k účtům Batch a Storage

Abyste mohli modul plug-in používat, je potřeba se přihlásit pomocí klíčů účtů Azure Batch a Azure Storage. Načtení klíčů účtů:

1. Zobrazte modul plug-in v aplikaci Maya a vyberte kartu **Config** (Konfigurace).
2. Přejděte na [Azure Portal](https://portal.azure.com).
3. Z nabídky vlevo vyberte **Účty Batch**. V případě potřeby klikněte na **Další služby** a vyfiltrujte _Batch_.
4. V seznamu vyhledejte požadovaný účet Batch.
5. Vyberte položku nabídky **Klíče**. Zobrazí se název účtu, adresa URL účtu a přístupové klíče:
    - Vložte adresu URL účtu Batch do pole **Service** (Služba) v modulu plug-in služby Batch.
    - Vložte název účtu do pole **Batch Account** (Účet Batch).
    - Vložte primární klíč účtu do pole **Batch Key** (Klíč služby Batch).
7. Z nabídky vlevo vyberte Účty úložiště. V případě potřeby klikněte na **Další služby** a vyfiltrujte _Storage_.
8. V seznamu vyhledejte požadovaný účet služby Storage.
9. Vyberte položku nabídky **Přístupové klíče**. Zobrazí se název účtu úložiště a klíče.
    - Vložte název účtu služby Storage do pole **Storage Account** (Účet služby Storage) v modulu plug-in služby Batch.
    - Vložte primární klíč účtu do pole **Storage Key** (Klíč služby Storage).
10. Klikněte na **Authenticate** (Ověřit) a ujistěte se, že má modul plug-in přístup k oběma účtům.

Jakmile úspěšně proběhne ověření, modul plug-in nastaví pole stavu na hodnotu **Authenticated** (Ověřeno): 

![Ověření účtů Batch a Storage](./media/batch-rendering-service/authentication.png)

### <a name="configure-a-pool-for-a-render-job"></a>Konfigurace fondu pro úlohu vykreslování

Po ověření účtů Batch a Storage nastavte fond pro úlohu vykreslování. Modul plug-in ukládá vaše výběry mezi relacemi. Jakmile nastavíte upřednostňovanou konfiguraci, nebude nutné ji upravovat, dokud se nezmění.

Následující části vás provedou dostupnými možnostmi na kartě **Submit** (Odeslat):

#### <a name="specify-a-new-or-existing-pool"></a>Zadání nového nebo existujícího fondu

Pokud chcete zadat fond, ve kterém se má spustit úloha vykreslování, vyberte kartu **Submit** (Odeslat). Tato karta nabízí možnosti pro vytvoření fondu nebo výběr existujícího fondu:

- Můžete automaticky zřídit fond pro tuto úlohu (výchozí možnost **Auto provision a pool for this job**). Pokud vyberete tuto možnost, služba Batch vytvoří fond výhradně pro aktuální úlohu a po dokončení úlohy vykreslování tento fond automaticky odstraní. Tato možnost je nejlepší v případě, že máte na dokončení jedinou úlohu vykreslování.
- Můžete znovu použít existující trvalý fond (možnost **Reuse an existing persistent pool**). Pokud máte existující fond, který je nečinný, můžete ho zadat pro spuštění úlohy vykreslování výběrem z rozevíracího seznamu. Opětovné použití existujícího trvalého fondu šetří čas potřebný ke zřízení fondu.  
- Můžete vytvořit nový trvalý fond (možnost **Create a new persistent pool**). Pokud vyberete tuto možnost, pro spuštění úlohy se vytvoří nový fond. Po dokončení úlohy se fond neodstraní, takže ho můžete znovu použít pro budoucí úlohy. Tuto možnost vyberte, pokud budete i nadále potřebovat spouštět úlohy vykreslování. U dalších úloh můžete vybrat možnost **Reuse an existing persistent pool** (Znovu použít existující trvalý fond) a použít trvalý fond, který jste vytvořili pro první úlohu.

![Zadání fondu, image operačního systému, velikosti virtuálního počítače a licence](./media/batch-rendering-service/submit.png)

Další informace o tom, jak nabíhají poplatky za virtuální počítače Azure, najdete v [nejčastějších dotazech k cenám pro Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/#faq) a [nejčastějších dotazech k cenám pro Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/#faq).

#### <a name="specify-the-os-image-to-provision"></a>Zadání image operačního systému ke zřízení

Typ image operačního systému, který se použije ke zřízení výpočetních uzlů ve fondu, můžete zadat na kartě **Env** (Environment, Prostředí). Služba Batch aktuálně pro úlohy vykreslování podporuje následující možnosti imagí:

|Operační systém  |Image  |
|---------|---------|
|Linux     |Batch CentOS Preview |
|Windows     |Batch Windows Preview |

#### <a name="choose-a-vm-size"></a>Výběr velikosti virtuálního počítače

Velikost virtuálního počítače můžete zadat na kartě **Env** (Prostředí). Další informace o dostupných velikostech virtuálních počítačů najdete v tématech [Velikosti virtuálních počítačů s Linuxem v Azure](https://docs.microsoft.com/azure/virtual-machines/linux/sizes) a [Velikosti virtuálních počítačů s Windows v Azure](https://docs.microsoft.com/azure/virtual-machines/windows/sizes). 

![Zadání image operačního systému a velikosti virtuálního počítače na kartě Env (Prostředí)](./media/batch-rendering-service/environment.png)

#### <a name="specify-licensing-options"></a>Zadání možností licencování

Licence, které chcete použít, můžete zadat na kartě **Env** (Prostředí). Mezi možnosti patří:

- **Maya** – Tato možnost je povolená ve výchozím nastavení.
- **Arnold** – Tato možnost je povolená, pokud se v aplikaci Maya zjistí Arnold jako aktivní vykreslovací jádro.

 Pokud chcete vykreslovat s použitím vlastní licence, můžete nakonfigurovat koncový bod licence přidáním odpovídajících proměnných prostředí do tabulky. Pokud to uděláte, nezapomeňte zrušit výběr výchozích možností licencování.

> [!IMPORTANT]
> Používání licencí se vám účtuje po dobu spuštění virtuálních počítačů ve fondu, i když se tyto virtuální počítače právě nepoužívají pro vykreslování. Pokud se chcete vyhnout dalším poplatkům, přejděte na kartu **Pools** (Fondy) a změňte velikost fondu na 0 uzlů, dokud nebudete připraveni spustit další úlohu vykreslování. 
>
>

#### <a name="manage-persistent-pools"></a>Správa trvalých fondů

Existující trvalý fond můžete spravovat na kartě **Pools** (Fondy). Po výběru fondu ze seznamu se zobrazí jeho aktuální stav.

Na kartě **Pools** (Fondy) můžete také odstranit fond a měnit počet virtuálních počítačů ve fondu. Můžete změnit velikost fondu na 0 uzlů a vyhnout se tak účtování poplatků mezi úlohami.

![Zobrazení, změna velikosti a odstranění fondů](./media/batch-rendering-service/pools.png)

### <a name="configure-a-render-job-for-submission"></a>Konfigurace úlohy vykreslování pro odeslání

Po zadání parametrů pro fond, ve kterém se spustí úloha vykreslování, nakonfigurujte samotnou úlohu. 

#### <a name="specify-scene-parameters"></a>Zadání parametrů scény

Modul plug-in služby Batch rozpozná, které vykreslovací jádro v aplikaci Maya aktuálně používáte, a na kartě **Submit** (Odeslat) zobrazí odpovídající nastavení. Tato nastavení zahrnují počáteční snímek, konečný snímek, předponu výstupu a krokování snímků. Nastavení vykreslení souboru scény můžete přepsat zadáním jiného nastavení v modulu plug-in. Změny, které provedete v nastavení modulu plug-in, se neuchovávají v nastavení vykreslení souboru scény, takže můžete provádět změny pro jednotlivé úlohy, aniž byste museli znovu nahrávat soubor scény.

Modul plug-in vás upozorní, pokud vybrané vykreslovací jádro v aplikaci Maya není podporováno.

Pokud načítáte novou scénu, když je modul plug-in otevřený, klikněte na tlačítko **Refresh** (Aktualizovat), abyste zajistili aktualizaci nastavení.

#### <a name="resolve-asset-paths"></a>Překlad cest k prostředkům

Modul plug-in po načtení vyhledá v souboru scény všechny odkazy na externí soubory. Tyto odkazy se zobrazí na kartě **Assets** (Prostředky). Pokud odkazovanou cestu není možné přeložit, modul plug-in se pokusí soubor najít v několika výchozích umístěních, mezi která patří:

- Umístění souboru scény 
- Adresář _sourceimages_ aktuálního projektu
- Aktuální pracovní adresář 

Pokud prostředek stáje není možné najít, je uvedený s ikonou upozornění:

![Chybějící prostředky jsou zobrazené s ikonou upozornění](./media/batch-rendering-service/missing_assets.png)

Pokud znáte umístění nepřeloženého odkazu na soubor, můžete kliknout na ikonu upozornění a zobrazí se výzva k přidání cesty pro hledání. Modul plug-in se pak s použitím této cesty pro hledání pokusí přeložit chybějící prostředky. Můžete přidat libovolný počet dalších cest pro hledání.

Když se odkaz přeloží, je uvedený s ikonou zeleného světla:

![Přeložené prostředky s ikonou zeleného světla](./media/batch-rendering-service/found_assets.png)

Pokud vaše scéna vyžaduje další soubory, které modul plug-in nerozpoznal, můžete přidat další soubory nebo adresáře. Použijte k tomu tlačítka **Add Files** (Přidat soubory) a **Add Directory** (Přidat adresář). Pokud načítáte novou scénu, když je modul plug-in otevřený, nezapomeňte kliknout na **Refresh** (Aktualizovat), aby se aktualizovaly odkazy scény.

#### <a name="upload-assets-to-an-asset-project"></a>Nahrání prostředků do projektu prostředků

Po odeslání úlohy vykreslování se odkazované soubory zobrazené na kartě **Assets** (Prostředky) automaticky nahrají do služby Azure Storage jako projekt prostředků. Soubory prostředků můžete také nahrát nezávisle na úloze vykreslování pomocí tlačítka **Upload** (Nahrát) na kartě **Assets** (Prostředky). Název projektu prostředků se zadává v poli **Project** (Projekt) a ve výchozím nastavení je pojmenovaný podle aktuálního projektu v aplikaci Maya. Při nahrávání souborů prostředků je zachována místní struktura souborů. 

Po nahrání může na prostředky odkazovat jakýkoli počet úloh vykreslování. Všechny nahrané prostředky jsou k dispozici pro jakoukoli úlohu, která odkazuje na projekt prostředků, bez ohledu na to, jestli jsou prostředky součástí scény. Pokud chcete u další úlohy změnit projekt prostředků, na který odkazuje, změňte název v poli **Project** (Projekt) na kartě **Assets** (Prostředky). Pokud chcete nějaké odkazované soubory vyloučit z nahrání, zrušte jejich výběr pomocí zeleného tlačítka vedle příslušného záznamu.

#### <a name="submit-and-monitor-the-render-job"></a>Odeslání a monitorování úlohy vykreslování

Po nakonfigurování úlohy vykreslování v modulu plug-in kliknutím na tlačítko **Submit Job** (Odeslat úlohu) na kartě **Submit** (Odeslat) úlohu odešlete do služby Batch:

![Odeslání úlohy vykreslování](./media/batch-rendering-service/submit_job.png)

Probíhající úlohu můžete monitorovat na kartě **Jobs** (Úlohy) modulu plug-in. Pokud chcete zobrazit aktuální stav úlohy, vyberte úlohu ze seznamu. Na této kartě můžete také rušit a odstraňovat úlohy nebo stáhnout výstupy a protokoly vykreslování. 

Pokud chcete stáhnout výstupy, upravte pole **Outputs** (Výstupy) a nastavte požadovaný cílový adresář. Kliknutím na ikonu ozubeného kola spusťte proces na pozadí, který sleduje úlohu a průběžně stahuje výstupy: 

![Zobrazení stavu úlohy a stažení výstupů](./media/batch-rendering-service/jobs.png)

Aplikaci Maya můžete zavřít, aniž by se přerušil proces stahování.

## <a name="next-steps"></a>Další kroky

Další informace o službě Batch najdete v tématu [Spouštění vnitřně paralelních úloh pomocí služby Batch](batch-technical-overview.md).
