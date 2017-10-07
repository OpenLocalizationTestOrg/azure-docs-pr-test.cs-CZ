---
title: "aaaOverview Azure Batch pro vývojáře | Microsoft Docs"
description: "Další funkce hello hello služby Batch a jejích rozhraní API z hlediska vývoje."
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
ms.assetid: 416b95f8-2d7b-4111-8012-679b0f60d204
ms.service: batch
ms.devlang: multiple
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-compute
ms.date: 06/28/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3da9d82572b28d05d1923166bd08c26725ca3316
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="develop-large-scale-parallel-compute-solutions-with-batch"></a>Vývoj rozsáhlých paralelních výpočetních řešení pomocí služby Batch

V tomto přehledu hello základní součásti služby Azure Batch hello probereme funkce hello primární služby a prostředky, vývojáři Batch můžete použít rozsáhlé paralelní toobuild výpočetní řešení.

Zda vyvíjíte distribuovaná výpočetní aplikace nebo služba, která vydává přímé [REST API] [ batch_rest_api] volání nebo pracujete s některou z hello [SDK služby Batch](batch-apis-tools.md#azure-accounts-for-batch-development), které budete používat Mnoho hello prostředky a funkce popsané v tomto článku.

> [!TIP]
> Vyšší úrovně Úvod toohello služby Batch naleznete v části [základy Azure Batch](batch-technical-overview.md).
>
>

## <a name="batch-service-workflow"></a>Pracovní postup služby Batch
Hello následující pracovní postup vysoké úrovně je typický pro téměř všechny aplikace a služby, které používají hello služby Batch ke zpracování paralelní úlohy:

1. Nahrát hello **datové soubory** , které chcete tooprocess tooan [Azure Storage] [ azure_storage] účtu. Batch zahrnuje integrovanou podporu pro přístup k úložišti objektů Blob v Azure a vaše úkoly můžete stáhnout tyto soubory příliš[výpočetní uzly](#compute-node) spuštění úlohy hello.
2. Nahrát hello **soubory aplikace** , budou vaše úkoly spouštět. Tyto soubory lze binární soubory nebo skripty a jejich závislosti a provedení hello úlohami v úlohách. Vaše úkoly tyto soubory můžete stáhnout z vašeho účtu úložiště, nebo můžete použít hello [balíčky aplikací](#application-packages) funkcí služby Batch pro nasazení a Správa aplikací.
3. Vytvořte [fond](#pool) výpočetních uzlů. Když vytvoříte fond, určíte hello počet výpočetních uzlů pro hello fondu, jejich velikosti a hello operačního systému. Když každý úkol v úloze běží, je přiřazen tooexecute na jednom z hello uzly ve fondu.
4. Vytvořte [úlohu](#job). Úloha spravuje kolekci úkolů. Můžete přiřadit konkrétní fond každé úlohy tooa, kde bude spuštění této úlohy.
5. Přidat [úlohy](#task) toohello úlohy. Každá úloha se spustí aplikace hello nebo skript, který jste nahráli tooprocess hello datové soubory, které stáhne ze svého účtu úložiště. Protože každý úkol dokončí, můžete nahrát jeho výstup tooAzure úložiště.
6. Sledujte průběh úlohy a načíst výstup úlohy hello ze služby Azure Storage.

Hello následující části popisují tyto a další prostředky služby Batch, které povolit váš distribuovaný výpočetní scénář hello.

> [!NOTE]
> Je nutné [účet Batch](#account) toouse hello služby Batch. Většina řešení Batch také používá pro ukládání a načítání souborů účet [Azure Storage][azure_storage]. Batch aktuálně podporuje pouze hello **pro obecné účely** typ účtu úložiště, jak je popsáno v kroku 5 tohoto [vytvořit účet úložiště](../storage/common/storage-create-storage-account.md#create-a-storage-account) v [účty Azure storage](../storage/common/storage-create-storage-account.md).
>
>

## <a name="batch-service-resources"></a>Prostředky služby Batch
Některé hello následující prostředky – účty, výpočetní uzly, fondy, úlohy a úkoly – jsou vyžadované všechna řešení, které používají služby Batch hello. Další, jako například plány úloh nebo balíčky aplikací, jsou užitečné, ale volitelné funkce.

* [Účet](#account)
* [Výpočetní uzel](#compute-node)
* [Fond](#pool)
* [Úloha](#job)

  * [Plány úlohy](#scheduled-jobs)
* [Úkol](#task)

  * [Spouštěcí úkol](#start-task)
  * [Úkol správce úloh](#job-manager-task)
  * [Úkoly přípravy a uvolnění úloh](#job-preparation-and-release-tasks)
  * [Úkoly s více instancemi (MPI)](#multi-instance-tasks)
  * [Závislosti úkolů](#task-dependencies)
* [Balíčky aplikací](#application-packages)

## <a name="account"></a>Účet
Účet Batch je jednoznačně identifikovaná entita v rámci hello služby Batch. Veškeré zpracování je přidruženo k účtu Batch.

Můžete vytvořit účet Azure Batch pomocí hello [portál Azure](batch-account-create-portal.md) nebo prostřednictvím kódu programu, například jako s hello [knihovny Batch Management .NET](batch-management-dotnet.md). Při vytváření účtu hello můžete přidružit účet úložiště Azure.

### <a name="pool-allocation-mode"></a>Režim přidělování fondů

Při vytváření účtu Batch můžete zadat způsob přidělování [fondů](#pool) výpočetních uzlů. Můžete zvolit tooallocate fondy počítačových uzlů v předplatném spravuje Azure Batch nebo kterou můžete přidělit v svého vlastního předplatného. Hello *režim přidělení fondu* vlastnost pro účet hello Určuje, kde jsou přiděleny fondy. 

toodecide, které fond toouse režimu přidělení, zvažte, které co nejlépe odpovídá vašemu scénáři:

* **Služba batch**: Služba Batch je hello výchozí fond přidělení režim, ve kterém jsou přiděleny fondy pozadí hello v Azure Spravovat odběry. Mějte na paměti tyto klíčové body o režimu přidělení fondu služby Batch hello:

    - Režim přidělení fondu služby Batch Hello podporuje fondy cloudové služby a virtuální počítač.
    - Hello režim přidělení fondu služby Batch podporuje obě ověření pomocí sdíleného klíče nebo [ověřování Azure Active Directory](batch-aad-auth.md) (Azure AD). 
    - Můžete buď vyhrazené nebo nízkou prioritu výpočetních uzlů ve fondech přidělené s režimem přidělení fondu služby Batch hello.
    - Režim přidělení fondu služby Batch hello nepoužívejte, pokud máte v plánu toocreate fondů virtuálních počítačů Azure z vlastní Image virtuálních počítačů, nebo pokud máte v plánu toouse virtuální sítě. Vytvoření účtu s režimem přidělení fondu uživatele předplatné hello místo.
    - Virtuální počítač fondy zřízené v účet je vytvořen s režimem přidělení fondu služby Batch hello musí být vytvořeny z [Marketplace virtuálních počítačů Azure] [ vm_marketplace] bitové kopie.

* **Uživatel předplatné**: V režimu přidělení fondu hello uživatele předplatného jsou fondy Batch přidělené v hello předplatné Azure, kde je vytvoření účtu hello. Mějte na paměti tyto klíčové body o hello uživatele předplatného fondu přidělení režimu:
     
    - Hello uživatele předplatného fondu přidělení režim podporuje pouze fondy virtuálního počítače. Nepodporuje fondy cloudových služeb.
    - toocreate virtuální počítač fondů z vlastní Image virtuálních počítačů nebo toouse virtuální síť s fondy virtuálního počítače, je nutné použít režim přidělení fondu hello předplatné uživatele.  
    - Je nutné použít [ověřování Azure Active Directory](batch-aad-auth.md) s fondy, které jsou přiděleny v předplatném hello uživatele. 
    - Musí nastavení vašeho účtu Batch klíče trezoru služby Azure, pokud je režim přidělení fondu hello nastavená tooUser předplatné. 
    - Můžete vytvořit pouze vyhrazený výpočetní uzly ve fondech v účet je vytvořen s režimem přidělení fondu uživatele předplatné hello. Uzly s nízkou prioritou se nepodporují.
    - Virtuální počítač fondy zřízené v účet s režimem přidělení fondu uživatele předplatné hello lze vytvořit buď z [Marketplace virtuálních počítačů Azure] [ vm_marketplace] bitové kopie, nebo z vlastní Image, kterou jste Zadejte.

Hello následující tabulka porovnává hello služby Batch a uživatel předplatného fondu přidělení režimy.

| **Režim přidělování fondů**                 | **Služba Batch**                                                                                       | **Předplatné uživatele**                                                              |
|-------------------------------------------|---------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------|
| **Fondy se přidělují v**               | Předplatné spravované Azure                                                                           | Hello uživatele předplatné, ve které hello Batch je vytvořen účet                        |
| **Podporované konfigurace**             | <ul><li>Konfigurace cloudové služby</li><li>Konfigurace virtuálního počítače (Linux a Windows)</li></ul> | <ul><li>Konfigurace virtuálního počítače (Linux a Windows)</li></ul>                |
| **Podporované image virtuálních počítačů**                  | <ul><li>Image z webu Azure Marketplace</li></ul>                                                              | <ul><li>Image z webu Azure Marketplace</li><li>Vlastní image</li></ul>                   |
| **Podporované typy výpočetních uzlů**         | <ul><li>Vyhrazené uzly</li><li>Uzly s nízkou prioritou</li></ul>                                            | <ul><li>Vyhrazené uzly</li></ul>                                                  |
| **Podporované metody ověřování**             | <ul><li>Sdílený klíč</li><li>Azure AD</li></ul>                                                           | <ul><li>Azure AD</li></ul>                                                         |
| **Vyžaduje se služba Azure Key Vault**             | Ne                                                                                                      | Ano                                                                                |
| **Kvóta pro jádra**                           | Určuje se podle kvóty pro jádra služby Batch                                                                          | Určuje se podle kvóty pro jádra předplatného                                              |
| **Podpora virtuální sítě Azure** | Fondy, které jsou vytvořené pomocí hello konfigurace cloudové služby                                                      | Fondy, které jsou vytvořené pomocí hello konfigurace virtuálního počítače                               |
| **Podporovaný model nasazení virtuální sítě**      | Virtuální sítě vytvořené pomocí modelu nasazení Classic                                                             | Virtuální sítě vytvořené pomocí modelu nasazení classic hello nebo Azure Resource Manager |

## <a name="azure-storage-account"></a>Účet služby Azure Storage

Většina řešení Batch pro ukládání souborů prostředků a výstupních souborů používá službu Azure Storage.  

Batch aktuálně podporuje pouze typ účtu úložiště pro obecné účely hello, jak je popsáno v kroku 5 tohoto [vytvořit účet úložiště](../storage/common/storage-create-storage-account.md#create-a-storage-account) v [účty Azure storage](../storage/common/storage-create-storage-account.md). Úkoly služby Batch (včetně standardních úkolů, spouštěcích úkolů, úkolů přípravy úloh a úkolů uvolnění úloh) musí určovat soubory prostředků, které jsou umístěné jenom v účtech úložiště pro obecné účely.


## <a name="compute-node"></a>Výpočetní uzel
Výpočetní uzel je virtuálních počítačů (VM) Azure nebo Cloudová služba virtuálního počítače, který je vyhrazený tooprocessing část zatížení vaší aplikace. Hello velikost uzlu určuje hello počet jader procesoru, kapacita paměti a velikost místního souboru systému, který je přidělen toohello uzlu. Můžete vytvářet fondy uzlů Windows nebo Linux pomocí Azure Cloud Services, bitové kopie z hello [Marketplace virtuálních počítačů Azure][vm_marketplace], nebo vlastní Image, které můžete připravit. Viz následující hello [fondu](#pool) části Další informace o těchto možnostech.

Uzly mohou spouštět všechny spustitelný soubor nebo skript, který podporuje prostředí operačního systému hello hello uzlu. To zahrnuje soubory s příponami \*.exe, \*cmd, \*.bat a skripty PowerShellu pro Windows a binární soubory, shell a Python skripty pro Linux.

Součástí všech výpočetních uzlů ve službě Batch také jsou:

* Standardní [struktura složek](#files-and-directories) a přidružené [proměnné prostředí](#environment-settings-for-tasks), které jsou úkolu k dispozici.
* **Brány firewall** nastavení, které jsou nakonfigurované toocontrol přístup.
* [Vzdálený přístup](#connecting-to-compute-nodes) tooboth systému Windows (protokol RDP (Remote Desktop)) a uzly Linux (Secure Shell (SSH)).

## <a name="pool"></a>Fond
Fond je kolekce uzlů, na kterých běží příslušná aplikace. Hello fondu možné vytvářet ručně nebo automaticky pomocí hello služby Batch při zadání toobe pracovní hello provést. Můžete vytvořit a spravovat fond, který splňuje požadavky na prostředky hello vaší aplikace. Fond můžete používat jenom hello dávkový účet, ve kterém byla vytvořena. Účet Batch může mít více než jeden fond.

Fondy Azure Batch sestavení nad hello základní výpočetní platformě Azure. Poskytují rozsáhlé přidělení, instalaci aplikací, distribuci dat, sledování stavu a flexibilní úpravy hello počet výpočetních uzlů v rámci fondu ([škálování](#scaling-compute-resources)).

Každý uzel, který je přidán tooa fondu je přiřazen jedinečný název a IP adresu. Pokud uzel je odebrán z fondu, veškeré změny provedené toohello operačním systému nebo souborech budou ztraceny a jeho název a IP adresy jsou uvolněny pro pozdější použití. Jestliže uzel opustí fond, je jeho životnost ukončena.

Při vytváření fondu můžete zadat následující atributy hello. Některá nastavení se liší v závislosti na režimu přidělení fondu hello hello Batch [účet](#account):

- Operační systém a verze výpočetního uzlu
- Typ výpočetního uzlu a cílový počet uzlů
- Velikost výpočetních uzlů hello
- Zásady škálování
- Zásady plánování úkolů
- Stav komunikace pro výpočetní uzly
- Spouštěcí úkoly pro výpočetní uzly
- Balíčky aplikací
- Konfigurace sítě

Každé z těchto nastavení je podrobně popsaná v další v následující části hello.

> [!IMPORTANT]
> Vytvoření s režimem přidělení fondu služby Batch hello účty batch mají výchozí kvótu, která omezuje hello počet jader na účtu Batch. Hello počet jader odpovídá toohello počet výpočetních uzlů. Můžete najít hello výchozí kvóty a pokyny o tom, příliš[navýšení kvóty](batch-quota-limit.md#increase-a-quota) v [kvóty a omezení pro hello služby Azure Batch](batch-quota-limit.md). Pokud váš fond nedosahuje jeho cílový počet uzlů, může být kvóta na jádra hello hello důvod.
>
>Účty batch, které jsou vytvořené pomocí režim přidělení fondu hello uživatele předplatné není sledovat hello kvóty služby Batch. Místo toho sdílet v hello základní kvóta pro hello zadaný odběr. Další informace najdete v tématu [Omezení virtuálních počítačů](../azure-subscription-service-limits.md#virtual-machines-limits) v tématu [Limity, kvóty a omezení předplatného a služeb Azure](../azure-subscription-service-limits.md).
>
>

### <a name="compute-node-operating-system-and-version"></a>Operační systém a verze výpočetního uzlu

Při vytváření fondu služby Batch, můžete zadat konfiguraci pro virtuální počítač Azure hello a hello typ operačního systému, který má toorun na každém výpočetním uzlu ve fondu hello. jsou dva typy Hello konfigurací, které jsou k dispozici v dávce:

- Hello **konfigurace virtuálního počítače**, která určuje, že fond hello se skládá z virtuálních počítačů Azure. Tyto virtuální počítače mohou být vytvořené na základě image Linuxu nebo Windows. 

    Když vytvoříte fond podle hello konfigurace virtuálního počítače, je nutné zadat nejen hello velikost hello uzlů a hello zdroj obrázků použitých toocreate hello je, ale také hello **odkaz na obrázek virtuálního počítače** a hello Batch **uzlu agenta SKU** toobe nainstalované na uzlech hello. Další informace o zadávání těchto vlastností fondů najdete v článku [Zřízení linuxových výpočetních uzlů ve fondech Azure Batch](batch-linux-nodes.md) 

- Hello **konfigurace cloudových služeb**, která určuje, že fond hello se skládá z uzlů Azure Cloud Services. Služba Cloud Services poskytuje *jenom* výpočetní uzly Windows.

    K dispozici operační systémy pro fondy konfigurace cloudových služeb, jsou uvedeny v hello [verze hostovaného operačního systému Azure a kompatibilních sad SDK](../cloud-services/cloud-services-guestos-update-matrix.md). Když vytvoříte fond, který obsahuje uzly cloudové služby, je nutné velikost uzlu hello toospecify a jeho *operačních systémů*. Cloudové služby jsou nasazené tooAzure rychleji než virtuální počítače se systémem Windows. Pokud chcete fondy výpočetních uzlů Windows, můžete zjistit, že služba Cloud Services představuje výhodu z hlediska času nasazení.

    * Hello *operačních systémů* také určuje, jaké verze rozhraní .NET jsou nainstalovány s hello operačního systému.
    * Jako u rolí pracovního procesu v rámci cloudové služby, můžete zadat *verze operačního systému* (Další informace o rolích pracovního procesu naleznete v tématu hello [o cloudové služby](../cloud-services/cloud-services-choose-me.md#tell-me-about-cloud-services) část v hello [cloudové služby Přehled](../cloud-services/cloud-services-choose-me.md)).
    * Jako u rolí pracovního procesu, doporučujeme, že zadáváte `*` pro hello *verze operačního systému* tak, aby hello uzly budou automaticky upgradovány a neexistuje žádná práce potřebné toocater toonewly vydané verze. Hello případem primárního použití pro výběr konkrétní verze operačního systému je tooensure kompatibility aplikací, což umožňuje testování toobe provést před povolením toobe verze hello aktualizovat zpětné kompatibility. Po ověření, hello *verze operačního systému* hello fond může být aktualizována a hello novou bitovou kopii operačního systému můžete instalovat – všechny spuštěné úlohy jsou přerušena a zařazena.

Když vytvoříte fond, budete potřebovat tooselect hello odpovídající **nodeAgentSkuId**, v závislosti na hello OS hello základní image vaší virtuálního pevného disku. Získáte mapování tootheir ID SKU agenta uzlu k dispozici odkazy na bitovou kopii operačního systému tak, že volání hello [seznamu podporované uzlu agenta SKU](https://docs.microsoft.com/rest/api/batchservice/list-supported-node-agent-skus) operaci.

V tématu hello [účet](#account) části informace o nastavení režimu přidělení fondu hello při vytváření účtu Batch.

#### <a name="custom-images-for-virtual-machine-pools"></a>Vlastní image pro fondy virtuálních počítačů

toouse vlastní image virtuálního počítače fondy tooprovision, vytvoření účtu Batch s režimem přidělení fondu uživatele předplatné hello. Při tomto režimu jsou fondy Batch přidělí hello předplatné, které se nachází hello účet. V tématu hello [účet](#account) části informace o nastavení režimu přidělení fondu hello při vytváření účtu Batch.

toouse vlastní image, budete potřebovat tooprepare hello image podle generalizací ho. Informace o přípravě vlastních imagí Linux z virtuálních počítačů Azure najdete v tématu [zaznamenat virtuální počítač Azure s Linuxem toouse jako šablona](../virtual-machines/linux/capture-image-nodejs.md). Informace o přípravě vlastních imagí Windows z virtuálních počítačů Azure najdete v tématu [Vytváření vlastních imagí virtuálních počítačů pomocí Azure PowerShellu](../virtual-machines/windows/tutorial-custom-images.md). 

> [!IMPORTANT]
> Při přípravě vlastní image, mějte na paměti hello následující:
> - Ujistěte se, hello základní OS bitovou kopii pomocí tooprovision fondech Batch nemá žádné předem nenainstalovali rozšíření Azure, jako je například rozšíření vlastních skriptů hello. Pokud hello image obsahuje předem nainstalovaná rozšíření, Azure setkat s problémy, které nasazení hello virtuálních počítačů.
> - Ujistěte se, že hello základní image OS, které zadáte, že používá hello výchozí dočasné jednotce jako hello agenta uzlu Batch aktuálně očekává hello výchozí dočasné jednotce.
>
>

toocreate fondu konfigurace virtuálního počítače pomocí vlastní image, budete potřebovat jeden nebo více standardní Azure Storage účty toostore vlastních bitových kopií virtuálního pevného disku. Vlastní image se ukládají jako objekty blob. tooreference vaše vlastní Image při vytváření fondu, zadejte hello identifikátory objektů BLOB VHD hello vlastní image pro hello [osDisk](https://docs.microsoft.com/rest/api/batchservice/add-a-pool-to-an-account#bk_osdisk) vlastnost hello [virtualMachineConfiguration](https://docs.microsoft.com/rest/api/batchservice/add-a-pool-to-an-account#bk_vmconf) vlastnost.

Ujistěte se, že splňují vaše účty úložiště hello následující kritéria:   

- Hello účty úložiště objektů BLOB VHD vlastní image obsahující hello potřebovat toobe v hello stejného předplatného jako hello účtu Batch (hello uživatele předplatné).
- Hello zadané účty úložiště potřebovat toobe v hello stejné oblasti jako hello účtu Batch.
- V současnosti jsou podporované jenom účty úložiště úrovně Standard pro obecné účely. Azure Premium storage bude v budoucí hello podporovány.
- Můžete zadat jeden účet úložiště s několika vlastními objekty blob VHD, nebo několik účtů úložiště, z nichž každý má jeden objekt blob. Doporučujeme toouse účtů více úložiště tooget lepší výkon.
- Jeden objekt blob VHD jedinečný vlastní image může podporovat až too40, které instance virtuálního počítače s Linuxem nebo 20 instance virtuálního počítače s Windows. Budete potřebovat toocreate kopie hello virtuálního pevného disku blob toocreate fondy s více virtuálních počítačů. Například fond s 200 virtuálních počítačů Windows potřebuje 10 jedinečný BLOB VHD zadané pro hello **osDisk** vlastnost.

toocreate fond z vlastní image pomocí hello portálu Azure:

1. Přejděte tooyour účtu Batch na portálu Azure hello.
2. Na hello **nastavení** okně, vyberte hello **fondy** položku nabídky.
3. Na hello **fondy** okně, vyberte hello **přidat** příkaz; hello **přidat fond** zobrazí se okno.
4. Vyberte **vlastní obrázek (Linux nebo Windows)** z hello **typ obrázku** rozevíracího seznamu. Hello portálu se zobrazí hello **vlastní Image** výběr. Vyberte jeden nebo více virtuálních pevných disků ze hello stejné kontejneru a klikněte na tlačítko hello **vyberte** tlačítko. 
    Podpora pro více virtuálních pevných disků ze jiným účtům úložiště a různé kontejnery bude přidána v budoucí hello.
5. Vyberte správný hello **vydavatele nebo nabídka nebo Sku** pro vlastní virtuální pevné disky, vyberte požadovaného hello **ukládání do mezipaměti** režimu a potom výplně ve všech dalších parametrů pro fond hello hello.
6. toocheck Pokud fond je založená na vlastní image, najdete v části hello **operačního systému** vlastnost v hello prostředků souhrnné části hello **fondu** okno. Hello hodnota této vlastnosti musí být **image virtuálního počítače vlastní**.
7. Zobrazí se všechny vlastní VHD přidružený fond na hello fondu **vlastnosti** okno.

### <a name="compute-node-type-and-target-number-of-nodes"></a>Typ výpočetního uzlu a cílový počet uzlů

Při vytváření fondu můžete zadat typy, které výpočetní uzly mají a hello cílovým počtem pro každý. Hello dva typy výpočetní uzly jsou:

- **Vyhrazené výpočetní uzly.** Vyhrazené výpočetní uzly jsou rezervované pro vaše úlohy. Jsou dražší než uzly nízkou prioritu, ale je zaručeno, že se zrušené toonever.

- **Výpočetní uzly s nízkou prioritou.** Uzly nízkou prioritu využít výhod kapacitní v Azure toorun vaše úloh služby Batch. Uzly s nízkou prioritou mají nižší hodinové náklady než vyhrazené uzly a umožňují používat úlohy náročné na výpočetní výkon. Další informace najdete v tématu [Použití virtuálních počítačů s nízkou prioritou ve službě Batch](batch-low-pri-vms.md).

    Pokud má Azure nedostatek nadbytečné kapacity, může dojít ke zrušení výpočetních uzlů s nízkou prioritou. Pokud uzel je zrušené při spuštění úlohy, jsou úlohy hello zařazena a spusťte znovu po výpočetního uzlu opět k dispozici. Nízkou prioritu uzly jsou dobrou volbou pro úlohy, kde čas dokončení úlohy hello je flexibilní a pracovní hello je distribuován do mnoha uzly. Než rozhodnete toouse nízkou prioritu uzly pro váš scénář, ujistěte se, že veškerou práci, ztratili kvůli toopreemption bude toorecreate minimální a snadné.

    Jsou k dispozici pouze pro účty Batch, které jsou vytvořené pomocí režimu přidělení fondu hello nastavit příliš nízkou prioritu výpočetní uzly**služba Batch**.

Může mít obě nízkou prioritu a vyhrazený výpočetní uzly v hello stejného fondu. Každý typ uzlu &mdash; nízkou prioritu a vyhrazené &mdash; má svou vlastní nastavení cíl, pro které můžete zadat hello požadovaného počtu uzlů. 
    
Hello počet výpočetních uzlů se označují tooas *cíl* proto, že v některých případech nemusí fondu dosáhnout hello požadovaného počtu uzlů. Například fond pravděpodobně nedosáhnete hello cíl nedosáhne hello [kvóta na jádra](batch-quota-limit.md) nejprve pro váš dávkový účet. Nebo hello fondu pravděpodobně nedosáhnete hello cíl, pokud jste použili automatické škálování vzorce toohello fondu, který omezuje hello maximální počet uzlů.

Informace o cenách výpočetních uzlů s nízkou prioritou a vyhrazených uzlů najdete v tématu [Ceny služby Batch](https://azure.microsoft.com/pricing/details/batch/).

### <a name="size-of-hello-compute-nodes"></a>Velikost výpočetních uzlů hello

Velikosti výpočetních uzlů z **konfigurace služby Cloud Services** jsou uvedeny v seznamu [Velikosti pro Cloud Services](../cloud-services/cloud-services-sizes-specs.md). Služba Batch podporuje všechny velikosti pro Cloud Services kromě `ExtraSmall`, `STANDARD_A1_V2` a `STANDARD_A2_V2`.

Velikosti výpočetních uzlů z **konfigurace virtuálního počítače** jsou uvedeny v seznamech [Velikosti pro virtuální počítače v Azure](../virtual-machines/linux/sizes.md) (Linux) a [Velikosti pro virtuální počítače v Azure](../virtual-machines/windows/sizes.md) (Windows). Služba Batch podporuje všechny velikosti VM Azure kromě `STANDARD_A0` a těch, které mají úložiště Premium (série `STANDARD_GS`, `STANDARD_DS` a `STANDARD_DSV2`).

Když vyberete velikost výpočetním uzlu, zvažte hello charakteristiky a požadavky hello aplikací, které je potřeba spustit na uzlech hello. Aspekty jako jestli je vícevláknové aplikace hello a kolik paměti spotřebuje může pomoct určit hello nejvíce vhodnou a cenově efektivní velikost uzlu. Je typické tooselect velikost uzlu, za předpokladu, že jeden úkol se spustí na uzlu současně. Je však možné toohave více úloh (a tedy několik instancí aplikace) [souběžně](batch-parallel-node-tasks.md) během provádění úlohy na výpočetních uzlech. V takovém případě je běžné toochoose větší uzel velikost tooaccommodate hello vyšší požadavek spouštění paralelních úloh. Další informace najdete v části [Zásady plánování úkolů](#task-scheduling-policy).

Všechny hello uzlů ve fondu jsou hello stejnou velikost. Pokud chcete toorun aplikace s odlišnými požadavky na systém anebo úrovně zatížení, doporučujeme vám, že používáte samostatné fondy.

### <a name="scaling-policy"></a>Zásady škálování

Pro dynamické úlohy, můžete napsat a použít [vzorec automatického škálování](#scaling-compute-resources) tooa fondu. Hello služba Batch pravidelně vyhodnocuje vzorec a upraví hello počet uzlů v rámci hello fondu na základě různých fondu, úlohy a parametry úlohy, které můžete zadat.

### <a name="task-scheduling-policy"></a>Zásady plánování úkolů

Hello [maximálního počtu úkolů na uzlu](batch-parallel-node-tasks.md) možnost konfigurace určuje maximální počet úkolů, které lze spustit souběžně na každém výpočetním uzlu v rámci fondu hello hello.

Výchozí konfigurace Hello určuje současně jeden úkol běží na uzlu, ale existují scénáře, kdy je výhodné toohave dvě nebo více úloh na uzlu spuštěný současně. Najdete v části hello [ukázkový scénář](batch-parallel-node-tasks.md#example-scenario) v hello [souběžných uzlu úlohy](batch-parallel-node-tasks.md) toosee článek, jak můžete využívat výhod více úkolů na uzlu.

Můžete také zadat *typ výplně* který určuje, zda šíří hello úkoly rovnoměrně mezi všechny uzly v rámci fondu Batch, nebo sady každý uzel s hello maximální počet úkolů před přiřazením úkolů tooanother uzlu.

### <a name="communication-status-for-compute-nodes"></a>Stav komunikace pro výpočetní uzly

Ve většině scénářů pracují úkoly nezávisle a nepotřebují toocommunicate sebou. Existují však aplikace, ve kterých úkoly musí komunikovat, například [scénáře MPI](batch-mpi.md).

Můžete nakonfigurovat fond tooallow **internodium komunikace**tak, aby v době běhu může komunikovat uzly v rámci fondu. Pokud je komunikace mezi uzly povolena, uzly ve fondech z konfigurace služby Cloud Services mohou vzájemně komunikovat na portech vyšších než 1100. Fondy z konfigurace virtuálního počítače neomezují provoz na žádném portu.

Upozorňujeme, že povolení komunikace internodium také ovlivňuje umístění hello hello uzlů v rámci clusterů a z důvodu omezení nasazení může být omezen hello maximální počet uzlů ve fondu. Pokud aplikace nevyžaduje komunikaci mezi uzly, hello služba Batch můžete přidělit potenciálně velký počet uzlů toohello fondu z mnoha různých clusterů a datových center tooenable zvýšený výkon paralelního zpracování.

### <a name="start-tasks-for-compute-nodes"></a>Spouštěcí úkoly pro výpočetní uzly

Hello volitelné *spouštěcí úkol* spustí na každém uzlu jako takový uzel připojí hello fondu, a pokaždé, když uzel restartovat nebo obnovit z Image. Úloha spuštění Hello je zvláště užitečná pro přípravu výpočetních uzlů k provádění úloh, jako je instalace hello aplikace, které vaše úlohy běží na výpočetních uzlech hello hello.

### <a name="application-packages"></a>Balíčky aplikací

Můžete zadat [balíčky aplikací](#application-packages) toodeploy toohello výpočetních uzlů ve fondu hello. Balíčky aplikací poskytují jednodušší nasazení a správa verzí hello aplikací, které vaše úkoly spouštět. Balíčky aplikací, které pro fond určíte, se nainstalují na každý uzel, který se do daného fondu připojí, a také pokaždé, když se uzel restartuje nebo obnoví z image.

> [!NOTE]
> Balíčky aplikací jsou podporované ve všech fondech služby Batch vytvořených po 5. červenci 2017. Že jsou podporované v fondy Batch vytvořit až 10. března 2016 5 2017 července pouze v případě, že hello fondu byla vytvořena pomocí konfigurace cloudové služby. Fondy batch vytvořen předchozí too10. března 2016 nepodporují balíčky aplikací. Další informace o použití aplikace balíčky toodeploy uzly Batch tooyour aplikace, najdete v části [nasazení uzly toocompute aplikací pomocí balíčků aplikací Batch](batch-application-packages.md).
>
>

### <a name="network-configuration"></a>Konfigurace sítě

Můžete zadat podsíť hello Azure [virtuální síť (VNet)](../virtual-network/virtual-networks-overview.md) v kterému hello fondu výpočetních uzlů by měl být vytvořen. V tématu hello [konfigurace sítě fondu](#pool-network-configuration) části Další informace.


## <a name="job"></a>Úloha
Úloha je kolekce úkolů. Spravuje, jak se provádí výpočet jeho úlohy na hello výpočetních uzlů ve fondu.

* Úloha Hello určuje hello **fondu** v které hello pracovní je toobe spustit. Můžete vytvořit nový fond pro každou úlohu nebo použít jeden fond pro mnoho úloh. Můžete vytvořit fond pro každou úlohu, která je přidružená k plánu úlohy, nebo pro všechny úlohy, které jsou spojeny s plánem úlohy.
* Volitelně můžete zadat **prioritu úlohy**. Když je úloha odeslána s vyšší prioritou než úlohy, které jsou aktuálně probíhá, hello úlohy pro úlohu hello s vyšší prioritou vloženy do hello fronty před úlohy pro úlohy s nižší prioritou hello. Úkoly v rámci úloh s nižší prioritou, které jsou již spuštěny, se neruší.
* Úlohu můžete použít **omezení** toospecify určitá omezení pro úlohy:

    Můžete nastavit **maximální uplynulý čas**tak, aby v případě, že úloha běží po dobu delší než hello maximální uplynulý čas, který je zadán, jsou ukončen hello úlohy a všechny její úkoly.

    Služba Batch umí zjistit a poté opakovat neúspěšné úkoly. Můžete zadat hello **maximální počet opakování úkolů** jako omezení, včetně toho, jestli je úloha *vždy* nebo *nikdy* opakovat. Opakování úkolu znamená, že tuto úlohu hello je zařazena toobe spusťte znovu.
* Klientské aplikace můžete přidat úloha tooa úlohy, nebo můžete zadat [úkolu Správce úloh](#job-manager-task). Úkol správce obsahuje hello informace, které jsou nezbytné toocreate hello požadované úlohy pro úlohu, s hello úkolu Správce úloh se spouští v jednom hello výpočetních uzlů ve fondu hello. Hello úkolu Správce úloh se zpracovává konkrétně podle Batch – je uložený ve frontě co nejrychleji hello úlohy je vytvořen a je restartovat, pokud se nezdaří. Úkolu Správce úloh je *požadované* pro úlohy, které jsou vytvořené pomocí [plán úloh](#scheduled-jobs) vzhledem k tomu, že je hello pouze způsob toodefine hello úlohy před vytvořením instance úlohy hello.
* Ve výchozím nastavení zůstanou úlohy v aktivním stavu hello po dokončení všech úloh v rámci úlohy hello. Toto chování můžete změnit tak, aby hello úlohy je automaticky ukončen po dokončení všech úloh v úloze hello. Úloha sady hello **onAllTasksComplete** vlastnost ([OnAllTasksComplete] [ net_onalltaskscomplete] v Batch .NET) příliš*terminatejob* tooautomatically Ukončete úlohu hello, když jsou všechny její úkoly ve stavu hello byla dokončena.

    Všimněte si, že služba Batch hello zvažuje úlohu s *žádné* toohave úlohy dokončit všechny její úkoly. Tato možnost se proto nejčastěji používá pro [úkoly správce úloh](#job-manager-task). Pokud chcete úlohu automatického ukončení toouse bez Správce úloh, měli byste nejdřív nastavit novou úlohu **onAllTasksComplete** vlastnost příliš*noaction*, nastavte příliš*terminatejob* až po dokončení přidávání úlohy toohello úlohy.

### <a name="job-priority"></a>Priorita úloh
Můžete přiřadit prioritu toojobs, který vytvoříte v dávce. Hello služba Batch používá hodnotu priority hello hello úlohy toodetermine hello pořadí plánování úlohy v rámci účtu (Toto není toobe zaměňovat s [naplánovaná úloha](#scheduled-jobs)). hodnoty priority Hello v rozsahu od -1000 too1000, s -1000 je hello nejnižší prioritu a 1000 se hello nejvyšší. tooupdate hello prioritu úlohy, volání hello [aktualizovat vlastnosti hello úlohy] [ rest_update_job] operace (Batch REST), nebo upravit hello [CloudJob.Priority] [ net_cloudjob_priority] vlastnost (Batch .NET).

V rámci hello stejný účet, s vyšší prioritou mají přednost plánování před úlohami s nižší prioritou úlohy. Úloha s vyšší hodnotou priority v jednom účtu nemá přednost při plánování před jinou úlohou s nižší hodnotou priority v jiném účtu.

Plánování úloh mezi fondy je nezávislé. Mezi různými fondy není zaručeno, že úloha s vyšší prioritou je naplánována jako první, pokud její přidružený fond nemá dostatek nečinných uzlů. V hello stejného fondu, úlohy s hello stejnou úroveň priority mají stejnou šanci být naplánované.

### <a name="scheduled-jobs"></a>Naplánované úlohy
[Plány úlohy] [ rest_job_schedules] umožňují toocreate opakované úlohy v rámci hello služby Batch. Plán úlohy Určuje když toorun úloh a obsahuje specifikace hello toobe hello úlohy spustit. Můžete zadat dobu trvání hello hello plán – jak dlouho a když hello plán je v platnosti – a jak často jsou vytvořeny úlohy během hello naplánované období.

## <a name="task"></a>Úkol
Úkol je jednotka výpočtu, která je přidružena k úloze. Běží na uzlu. Úkoly jsou přiřazeny tooa uzlu pro provádění nebo jsou zařazeny do fronty, dokud se uzel neuvolní. Jednoduše PUT, úloha běží jeden nebo více programů nebo skriptů na výpočetní uzel tooperform hello práci, které potřebujete.

Při vytvoření úkolu můžete zadat:

* Hello **příkazového řádku** pro úlohu hello. Toto je hello příkazového řádku, který spouští vaše aplikace nebo skriptu na výpočetním uzlu hello.

    Je důležité, že toonote, který hello příkazového řádku není ve skutečnosti spuštěn v rámci prostředí. Proto jej nelze využít nativně prostředí funkcí jako [proměnnou prostředí](#environment-settings-for-tasks) rozšíření (to zahrnuje hello `PATH`). tootake využít těchto funkcí, je nutné vyvolat hello prostředí v příkazovém řádku hello – například spuštěním `cmd.exe` v uzlech systému Windows nebo `/bin/sh` v systému Linux:

    `cmd /c MyTaskApplication.exe %MY_ENV_VAR%`

    `/bin/sh -c MyTaskApplication $MY_ENV_VAR`

    Pokud vaše úkoly potřebovat toorun aplikace nebo skriptu, který není součástí hello uzlu `PATH` nebo referenční proměnné prostředí, vyvolání hello prostředí explicitně v hello úlohu příkazového řádku.
* **Soubory prostředků** obsahující toobe hello dat zpracovat. Tyto soubory jsou automaticky zkopírovaný toohello uzlu z úložiště objektů Blob v účtu úložiště Azure pro obecné účely, před provedením úloh hello příkazového řádku. Další informace najdete v části hello [spouštěcí úkol](#start-task) a [souborů a adresářů](#files-and-directories).
* Hello **proměnné prostředí** požadované aplikace. Další informace najdete v tématu hello [nastavení prostředí pro úkoly](#environment-settings-for-tasks) části.
* Hello **omezení** v rámci které hello by měla spustit úlohu. Omezení patří například maximální doba hello tuto úlohu hello je povoleno toorun hello maximální počet opakování nezdařené úlohy by měl být, a maximální hello času, které jsou zachovány soubory v pracovního adresáře úkolu hello.
* **Balíčky aplikací** toodeploy toohello výpočetního uzlu, na které hello úloha je naplánovaná toorun. [Balíčky aplikací](#application-packages) poskytují jednodušší nasazení a správa verzí hello aplikací, které vaše úkoly spouštět. Balíčky aplikací úrovni úkolů jsou obzvláště užitečná v prostředích sdílených fondu různé úlohy se spouštějí na jeden fond, kde hello fondu se neodstraní po dokončení úlohy. Pokud vaše úlohy má méně úloh než uzly ve fondu hello, balíčky aplikací úloh Minimalizovat přenos dat vzhledem k tomu, že je vaše aplikace nasazené toohello pouze uzly, které využívají úlohy.

Kromě toho tootasks definujete výpočetní tooperform na uzlu, hello následující zvláštní úkoly jsou také poskytované služby Batch hello:

* [Spouštěcí úkol](#start-task)
* [Úkol správce úloh](#job-manager-task)
* [Úkoly přípravy a uvolnění úloh](#job-preparation-and-release-tasks)
* [Úkoly s více instancemi (MPI)](#multi-instance-tasks)
* [Závislosti úkolů](#task-dependencies)

### <a name="start-task"></a>Spouštěcí úkol
Tím, že přidružíte **spouštěcí úkol** k fondu, můžete připravit hello provozní prostředí jeho uzlů. Například můžete provádět akce jako instalace hello aplikace, které běží vaše úlohy nebo spuštění procesů na pozadí. Hello spouštěcí úkol běží při každém spuštění uzlu, pro tak dlouho, dokud zůstává ve fondu hello – včetně hello uzlu při prvním přidání toohello fondu a kdy je restartovat nebo obnovit z Image.

Hlavní výhodou spouštěcího úkolu hello je, že může obsahovat všechny hello informace, které jsou nezbytné tooconfigure výpočetního uzlu a instalovat aplikace hello, které jsou požadovány pro provedení úlohy. Proto je zvýšení hello počet uzlů ve fondu stejně jednoduché jako určení počtu hello nových cílových uzlů. Úloha spuštění Hello poskytuje hello Batch služby hello informace, které je potřebné tooconfigure hello nových uzlů a jejich přípravu pro přijetí úkolů.

Jako u jakékoli jiné úlohy Azure Batch, můžete určit seznam **soubory prostředků** v [Azure Storage][azure_storage], kromě tooa **příkazového řádku** toobe spustit. Hello služba Batch nejprve zkopíruje hello prostředků soubory toohello uzlu ze služby Azure Storage a poté spustí hello příkazového řádku. Pro spouštěcí úkol fondu obvykle obsahuje seznam souborů hello hello úloh aplikace a jeho závislosti.

Spouštěcí úkol hello ale mohly by také zahrnovat referenční data toobe používána ve všech úkolech, které jsou spuštěny na výpočetním uzlu hello. Například může provést úlohu spustit příkazový řádek `robocopy` operace toocopy soubory aplikace (které bylo zadáno jako soubory prostředků a stáhnout toohello uzlu) z hello spuštění úkolu [pracovní adresář](#files-and-directories) toohello [sdílenou složku](#files-and-directories), a pak spustit soubor MSI nebo `setup.exe`.

Je obvykle žádoucí, aby toowait služby Batch hello pro hello spuštění úloh toocomplete před zvažování hello uzlu připravené toobe přiřazené úlohy, ale to můžete nakonfigurovat.

Pokud na výpočetním uzlu selže spouštěcí úkol, pak hello stav uzlu hello aktualizované tooreflect hello selhání a uzel hello nemá přiřazeny žádné úlohy. Spouštěcí úkol může selhat, pokud je problém s kopírováním jeho souborů prostředků z úložiště, nebo pokud hello proces spuštěný pomocí svého příkazového řádku vrátí nenulový ukončovací kód.

Pokud přidáte nebo aktualizujete hello spouštěcí úkol pro existující fond, musíte restartovat svůj výpočetní uzly pro hello spuštění úloh toobe použít toohello uzly.

>[!NOTE]
> Hello celková velikost spouštěcí úkol musí být menší než nebo rovna too32768 znaků, včetně zdrojových souborů a proměnných prostředí. tooensure, že spouštěcí úkol splňuje tento požadavek, můžete použít jednu ze dvou následujících metod:
>
> 1. V každém uzlu ve fondu Batch můžete použít aplikaci balíčky toodistribute aplikace nebo data. Další informace o balíčky aplikací najdete v tématu [nasazení uzly toocompute aplikací pomocí balíčků aplikací Batch](batch-application-packages.md).
> 2. Můžete ručně vytvořit komprimovaný archiv obsahující vaše soubory aplikací. Nahrajte váš komprimované archivu tooAzure úložiště jako objekt blob. Zadejte archivu ZIP hello jako soubor prostředků pro spouštěcí úkol. Než spustíte hello příkazového řádku pro spouštěcí úkol, rozbalte hello archivu z příkazového řádku hello. 
>
>    toounzip hello archiv, můžete použít hello archivace nástroj podle svého výběru. Budete potřebovat tooinclude hello nástroj použít toounzip hello archivu jako soubor prostředků pro spouštěcí úkol hello.
>
>

### <a name="job-manager-task"></a>Úkol správce úloh
Obvykle se používá **úkolu Správce úloh** toocontrol nebo monitorování úlohy provádění – například toocreate a odeslání hello úlohy pro úlohu, určení dalších úloh toorun a určit, kdy je práce dokončena. Úkolu Správce úloh však není omezený toothese aktivity. Jde o plnohodnotný úkol, který může provádět všechny akce, které jsou požadovány pro úlohu hello. Úkol správce může například stáhnout soubor, který je zadaný jako parametr, analyzovat obsah hello tohoto souboru a odeslat dalších úkoly na základě těchto obsahů.

Úkol správce úloh je spuštěn před všemi ostatními úkoly. Poskytuje hello následující funkce:

* Ho je automaticky odeslán jako úkol podle hello služby Batch při vytvoření úlohy hello.
* Ho je naplánované tooexecute před hello další úkoly v úloze.
* Jeho přidružený uzel je poslední toobe hello odebrat z fondu, pokud hello fond zmenšován.
* Jeho ukončení může být vázanou toohello ukončení všech úkolů v úloze hello.
* Úkolu Správce úloh je uveden hello nejvyšší prioritou, když potřebuje toobe restartovat. Pokud není k dispozici nečinný uzel, hello služba Batch může ukončit jeden z hello ostatních spuštěných úkolů ve hello fondu toomake prostor pro toorun úkol Správce úloh hello.
* Úkolu Správce úloh v jedné úloze nemá přednost přes hello úkoly jiných úloh. Mezi úlohami jsou dodržovány pouze priority s úrovní úlohy.

### <a name="job-preparation-and-release-tasks"></a>Úkoly přípravy a uvolnění úloh
Služba Batch zajišťuje úkoly přípravy úloh pro nastavení před provedením úlohy. Úkoly uvolnění úloh jsou určeny pro údržbu nebo čištění po provedení úlohy.

* **Úkol přípravy úlohy**: úkol přípravy úlohy se spustí na všech výpočetních uzlech, které jsou naplánované toorun úlohy, před spuštěním hello se provést další úlohy. Data toocopy úkol přípravy úlohy, která je sdílena všemi úkoly, ale je jedinečné toohello úlohy, například můžete použít.
* **Úkol uvolnění úlohy**: Po dokončení úlohy úkol uvolnění úlohy se spustí na každém uzlu ve fondu hello, která spouští alespoň jeden úkol. Úloha verze úloh toodelete data, která je zkopírovaná pomocí hello úkol přípravy úlohy nebo toocompress a odesílání diagnostických dat protokolu, můžete například použít.

Obě úlohy přípravy a uvolnění úlohy umožňují toospecify toorun příkazového řádku při vyvolání úlohy hello. Nabízejí funkce, jako například stahování souborů, provádění se zvýšenými oprávněními, vlastní proměnné prostředí, maximální dobu provádění, počet opakování a dobu uchovávání souboru.

Další informace ohledně úkolů přípravy a uvolnění úloh najdete v části [Spouštění úkolů přípravy a dokončení úlohy na výpočetních uzlech Azure Batch](batch-job-prep-release.md).

### <a name="multi-instance-task"></a>Úkoly s více instancemi
A [úkol s více instancemi](batch-mpi.md) je úkol, který je nakonfigurovaný toorun na více než jednom výpočetním uzlu současně. Pomocí úkolů s více instancemi můžete povolit vysoce výkonné výpočetní scénáře, které vyžadují skupinu výpočetních uzlů, které jsou přidělené společně tooprocess jediné úlohy (jako je rozhraní MPI (Message Passing)).

Podrobné informace o spouštění úloh MPI ve službě Batch pomocí knihovny Batch .NET hello, podívejte se na [použití s více instancemi úloh toorun aplikací rozhraní MPI (Message Passing) v Azure Batch](batch-mpi.md).

### <a name="task-dependencies"></a>Závislosti úkolů
[Závislosti úkolů](batch-task-dependencies.md)jako hello již název napovídá, umožňují toospecify, který úkol závisí na hello dokončení jiného úkolu před jeho spuštěním. Tato funkce poskytuje podporu pro situace, ve kterých "podřízený" úkol spotřebovává výstup hello "nadřazeného" úkolu--nebo když nadřazený úkol provádí inicializaci, který je požadován podřízeným úkolem. toouse tato funkce je potřeba první povolit závislosti úkolů v úloze služby Batch. Potom pro každý úkol, který závisí na jiném (nebo mnoha jiných), zadejte hello úlohy, které tento úkol závisí.

Pomocí závislosti úkolů můžete nakonfigurovat jako hello následující scénáře:

* *Úkol B* závisí na *úkolu A* (*úkol B* nezahájí provádění, dokud se nedokončí *úkol A*).
* *Úkol C* závisí na *úkolu A* i *úkolu B*.
* *Úkol D* závisí na celé řadě úkolů, například na úkolu *1* až *10*, než se provede

Podívejte se na [úkolů závislostí v Azure Batch](batch-task-dependencies.md) a hello [TaskDependencies] [ github_sample_taskdeps] ukázka kódu v hello [azure-batch-samples] [ github_samples] Úložiště GitHub další podrobné informace o této funkci.

## <a name="environment-settings-for-tasks"></a>Nastavení prostředí pro úlohy
Každý úkol provedený hello služba Batch má přístup tooenvironment proměnné, které se nastaví na výpočetních uzlech. To zahrnuje proměnných definovaných systémem hello služby Batch ([služby definované][msdn_env_vars]) a vlastní proměnné prostředí definující pro vaše úkoly. Hello aplikace a skripty, které vaše úkoly spouští mít přístup k proměnným prostředí toothese během provádění.

Vlastní proměnné prostředí můžete nastavit na úrovni úloh hello naplněním hello *nastavení prostředí* vlastnost pro tyto entity. Například v tématu hello [přidat úloha tooa úlohu] [ rest_add_task] operace (Batch REST API) nebo hello [CloudTask.EnvironmentSettings] [ net_cloudtask_env] a [ CloudJob.CommonEnvironmentSettings] [ net_job_env] vlastnosti v Batch .NET.

Klientská aplikace nebo služby můžete získat proměnné prostředí úkolu, definované služby i vlastní, pomocí hello [získat informace o úkolu] [ rest_get_task_info] operace (Batch REST) nebo přístupem k Hello [CloudTask.EnvironmentSettings] [ net_cloudtask_env] vlastnost (Batch .NET). Procesy prováděné na výpočetním uzlu přístupné a jiné proměnné prostředí v hello uzlu, například pomocí hello známé `%VARIABLE_NAME%` (Windows) nebo `$VARIABLE_NAME` syntaxe (Linux).

Úplný seznam všech proměnných prostředí definovaných službou najdete v článku [Proměnné prostředí výpočetního uzlu][msdn_env_vars].

## <a name="files-and-directories"></a>Soubory a adresáře
Každý úkol má *pracovní adresář*, pod kterým vytváří žádný nebo více souborů a adresářů. Pracovní adresář může být použity k uložení programu hello, spuštěného úlohou hello hello data, která zpracovává, a hello výstup hello zpracování, které provádí. Všechny soubory a adresáře úlohy jsou vlastněny hello úloh uživatele.

Hello služba Batch zveřejňuje část systému souborů hello na uzlu jako hello *kořenový adresář*. Úlohy můžete přístup hello kořenový adresář odkazem hello `AZ_BATCH_NODE_ROOT_DIR` proměnné prostředí. Další informace o používání proměnných prostředí najdete v tématu [Nastavení prostředí pro úkoly](#environment-settings-for-tasks).

Hello kořenový adresář obsahuje následující adresářovou strukturu hello:

![Adresářová struktura výpočetního uzlu][1]

* **sdílené**: Tento adresář poskytuje přístup pro čtení a zápis příliš*všechny* úkoly, které běží na uzlu. Všechny úlohy, které běží na uzlu hello můžete vytvářet, číst, aktualizovat a odstranit soubory v tomto adresáři. Úlohy můžete přístup k tomuto adresáři odkazem hello `AZ_BATCH_NODE_SHARED_DIR` proměnné prostředí.
* **startup**: Tento adresář je využíván spouštěcím úkolem jako jeho pracovní adresář. Všechny soubory hello, které jsou stažené toohello uzlu hello spuštění úlohy jsou zde uloženy. Hello spouštěcí úkol může vytvořit, číst, aktualizovat a odstranit soubory v tomto adresáři. Úlohy můžete přístup k tomuto adresáři odkazem hello `AZ_BATCH_NODE_STARTUP_DIR` proměnné prostředí.
* **Úlohy**: adresář se vytvoří pro každý úkol, který běží na uzlu hello. Je přístupný odkazem hello `AZ_BATCH_TASK_DIR` proměnné prostředí.

    V rámci každého adresáře úkolu vytvoří hello služba Batch pracovní adresář (`wd`) jehož jedinečná cesta je zadána hello `AZ_BATCH_TASK_WORKING_DIR` proměnné prostředí. Tento adresář poskytuje úloha toohello přístupu pro čtení a zápis. Hello úloh můžete vytvářet, číst, aktualizovat a odstranit soubory v tomto adresáři. Tento adresář je zachován podle hello *RetentionTime* omezení, které je zadané pro úlohu hello.

    `stdout.txt`a `stderr.txt`: tyto soubory jsou zapsány do složky toohello úkolů během provádění hello hello úlohy.

> [!IMPORTANT]
> Pokud uzel je odebrán z fondu hello *všechny* Dobrý den soubory, které jsou uloženy na uzlu hello byly odebrány.
>
>

## <a name="application-packages"></a>Balíčky aplikací
Hello [balíčky aplikací](batch-application-packages.md) funkce poskytuje snadnou správu a nasazení aplikací toohello výpočetních uzlů ve fondech. Můžete nahrát a spravovat více verzí hello spuštění aplikací prostřednictvím úkolů, včetně jejich binárních a podpůrných souborů. Potom můžete automaticky nasadit jednu nebo více z těchto aplikací toohello výpočetní uzly ve fondu.

Můžete zadat balíčky aplikací na úrovni hello fondu a úloh. Když zadáte balíčky fondu aplikací, aplikace hello je nasazené tooevery uzlu ve fondu hello. Když zadáte úloh balíčky aplikací, aplikace hello je nasazené pouze toonodes, které jsou naplánované toorun alespoň jeden z hello úlohy, těsně před hello příkazový řádek úkolu spuštění.

Batch zpracovává hello podrobnosti o práci s Azure Storage toostore balíčky aplikací a nasadit je toocompute uzly, můžete zjednodušit režijní náklady na kód a správu.

toofind Další informace o funkci balíčku aplikace hello, podívejte se na [nasazení uzly toocompute aplikací pomocí balíčků aplikací Batch](batch-application-packages.md).

> [!NOTE]
> Pokud přidáte fondu aplikací balíčky tooan *existující* fondu, musíte restartovat svůj výpočetní uzly pro uzly toohello toobe nasadit balíčky aplikace hello.
>
>

## <a name="pool-and-compute-node-lifetime"></a>Životnost fondu a výpočetního uzlu
Při návrhu řešení Azure Batch, máte toomake návrhu rozhodnutí o tom, a když fondy vytvářejí a jak dlouho jsou výpočetní uzly v rámci těchto fondů udržovány dostupné.

Na jednom konci spektra hello můžete vytvořit fond pro každou úlohu, která odešlete a odstraní hello fondu, jakmile její úkoly dokončí provádění. To maximalizuje využití, protože hello uzly jsou přiděleny pouze tehdy, když potřeby a vypnout, jakmile jsou nečinnosti. To znamená, že hello úloha musí čekat toobe uzly hello přidělené, je důležité toonote, které úlohy jsou naplánovány pro spuštění, jakmile jsou jednotlivě k dispozici uzly, přiděleno a hello spouštěcí úkol je dokončen. Batch nemá *není* Počkejte, dokud před přiřazením úkolů toohello uzly jsou k dispozici všechny uzly v rámci fondu. Tím je zajištěno maximální využití všech dostupných uzlů.

V hello druhém konci spektra hello, pokud okamžité spuštění úloh hello nejvyšší prioritou, můžete vytvořit fond dopředu a zpřístupnit jeho uzly před odesláním úloh. V tomto scénáři můžete úloh spustit okamžitě, ale uzly mohou být v nečinnosti při čekání na jejich toobe přiřazen.

V případě zpracovávání proměnlivého, ale stálého zatížení se obvykle používá kombinovaný přístup. Můžete mít fond, že se odešlou do více úloh, ale můžete škálovat hello počet uzlů, nahoru nebo dolů podle zatížení úlohy toohello (viz [škálování výpočetní prostředky](#scaling-compute-resources) v následující části hello). Toto přizpůsobování kapacity můžete provádět reaktivně, na základě aktuálního zatížení, nebo proaktivně, pokud lze zatížení předpovídat.

## <a name="virtual-network-vnet-and-firewall-configuration"></a>Konfigurace virtuální sítě a brány firewall 

Při zřizování fond výpočetních uzlů ve službě Azure Batch můžete fondu hello přidružit podsíť Azure [virtuální síť (VNet)](../virtual-network/virtual-networks-overview.md). toolearn Další informace o vytvoření virtuální sítě s podsítěmi, najdete v části [vytvořit virtuální síť Azure s podsítěmi](../virtual-network/virtual-networks-create-vnet-arm-pportal.md). 

 * Hello virtuální sítě přidružený fondu musí být:

   * V hello stejné Azure **oblast** jako hello účtu Azure Batch.
   * V hello stejné **předplatné** jako hello účtu Azure Batch.

* Hello typ virtuální sítě podporované závisí na přidělování fondů pro hello účtu Batch:

    - Pokud je režim přidělení fondu hello vašeho účtu Batch nastaven tooBatch služby, pak můžete přiřadit pouze toopools virtuální sítě vytvořené pomocí hello **konfigurace cloudových služeb**. Kromě toho hello zadat virtuální síť musí být vytvářeny pomocí modelu nasazení classic hello. Virtuální sítě vytvořené pomocí modelu nasazení Azure Resource Manager hello nejsou podporovány.
 
    - Pokud je režim přidělení fondu hello vašeho účtu Batch nastaven tooUser předplatné, pak můžete přiřadit pouze toopools virtuální sítě vytvořené pomocí hello **konfigurace virtuálního počítače**. Fondy, které jsou vytvořené pomocí hello **konfigurace služby Cloud** nejsou podporovány. Dobrý den, které mohou být vytvořeny přidružené virtuální sítě pomocí modelu nasazení Azure Resource Manager hello nebo modelu nasazení classic hello.

    Pro tabulku shrnutí podpora virtuální sítě podle toopool přidělení režimu, najdete v části hello [režim přidělení fondu](#pool-allocation-mode) části.

* Pokud tooBatch služeb má nastaven režim přidělení fondu hello vašeho účtu Batch, pak musíte poskytnout oprávnění pro hello tooaccess hlavní služby Batch hello virtuální sítě. Hello virtuální síť musí přiřadit hello [Classic Virtual Machine Contributor Role-Based řízení přístupu (RBAC)](https://azure.microsoft.com/documentation/articles/role-based-access-built-in-roles/#classic-virtual-machine-contributor) role toohello Batch instanční objekt. Pokud hello zadané RBAC role není zadaný, služba Batch hello vrátí 400 (Chybný požadavek). role hello tooadd v hello portálu Azure:

    1. Vyberte hello **VNet**, pak **přístup k ovládacímu prvku (IAM)** > **role** > **Přispěvatel virtuálních počítačů**  >  **Přidat**.
    2. Na hello **přidat oprávnění** okně, vyberte hello **Přispěvatel virtuálních počítačů** role.
    3. Na hello **přidat oprávnění** okno, vyhledejte hello Batch API. Hledání pro každý z těchto řetězců zase vyhledejte hello rozhraní API:
        1. **MicrosoftAzureBatch**.
        2. **Microsoft Azure Batch**. Novější tenanti služby Azure AD mohou používat tento název.
        3. **ddbf3205-c6bd-46ae-8127-60eb93363864** se hello ID hello Batch API. 
    3. Vyberte hello Batch API instanční objekt. 
    4. Klikněte na **Uložit**.

        ![Přiřazení role Přispěvatel virtuálních počítačů, tooBatch objektu služby](./media/batch-api-basics/iam-add-role.png)


* Hello Zadaná podsíť by měl mít dostatek volného **IP adresy** tooaccommodate hello celkový počet cílové uzly; to znamená, hello součet hello `targetDedicatedNodes` a `targetLowPriorityNodes` vlastnosti fondu hello. Pokud podsíť hello nemá dostatek volné IP adresy, hello služba Batch částečně přiděluje hello výpočetních uzlů ve fondu hello a vrátí chybu změny velikosti.

* Hello Zadaná podsíť musí umožňovat komunikaci z toobe služby Batch hello možné tooschedule úlohy na výpočetních uzlech hello. Pokud je komunikace toohello výpočetních uzlů je odepřen **skupina zabezpečení sítě (NSG)** přidružené hello sítě VNet, pak hello služba Batch nastaví stav hello hello výpočetních uzlů příliš**nepoužitelná**.

* Pokud hello zadané virtuální síti přidruženy **skupiny zabezpečení sítě (Nsg)** nebo **brány firewall**, pak musí být povoleny několik vyhrazené zápisy systémové porty pro příchozí komunikaci:

- Pro fondy vytvořené s konfigurací virtuálního počítače povolte porty 29876 a 29877 také port 22 pro Linux a port 3389 pro Windows. 
- Pro fondy vytvořené s konfigurací cloudové služby povolte porty 10100, 20100 a 30100. 
- Povolte odchozí připojení tooAzure úložiště na portu 443. Také se ujistěte, že všechny vlastní servery DNS obsluhující virtuální síť dokáží přeložit váš koncový bod služby Azure Storage. Konkrétně, adresa URL hello formátu `<account>.table.core.windows.net` musí být možné přeložit.

    Hello následující tabulka popisuje hello příchozí porty, které je třeba tooenable pro fondy, které jste vytvořili pomocí hello konfigurace virtuálního počítače:

    |    Cílové porty    |    Zdrojová IP adresa      |    Přidala služba Batch skupiny NSG?    |    Vyžaduje se pro virtuální počítač toobe použitelné?    |    Akce ze strany uživatele   |
    |---------------------------|---------------------------|----------------------------|-------------------------------------|-----------------------|
    |    <ul><li>Pro fondy, které jsou vytvořené pomocí konfigurace virtuálního počítače hello: 29876, 29877</li><li>Pro fondy, které jsou vytvořené pomocí konfigurace služby cloud hello: 10100, 20100, 30100</li></ul>         |    Jenom IP adresy role služby Batch |    Ano. Batch přidá skupiny Nsg na úrovni hello tooVMs rozhraní (NIC) připojené sítě. Tyto skupiny zabezpečení sítě povolují přenos jenom z IP adres role služby Batch. I v případě, že otevřete tyto porty pro celý web hello, provoz hello získat zablokuje v hello síťový adaptér. |    Ano  |  Není nutné toospecify skupina NSG, protože Batch umožňuje pouze dávky IP adres. <br /><br /> Pokud ale NSG zadáte, nezapomeňte prosím zajistit, aby tyto porty byly otevřené pro příchozí přenos. <br /><br /> Pokud zadáte * jako hello zdrojové IP adresy ve vaší NSG, dávky stále přidá skupiny Nsg na úrovni hello síťovou kartu připojenou tooVMs. |
    |    3389, 22               |    Uživatel počítače slouží pro ladění, aby vzdálený přístup k hello virtuálních počítačů.    |    Ne                                    |    Ne                     |    Pokud chcete toohello toopermit vzdáleného přístupu (RDP/SSH) virtuálních počítačů, přidáte skupiny Nsg.   |                 

    Hello následující tabulka popisuje hello odchozí port, je nutné, aby tooenable toopermit přístup tooAzure úložiště:

    |    Odchozí porty    |    Cíl    |    Přidala služba Batch skupiny NSG?    |    Vyžaduje se pro virtuální počítač toobe použitelné?    |    Akce ze strany uživatele    |
    |------------------------|-------------------|----------------------------|-------------------------------------|------------------------|
    |    443    |    Azure Storage    |    Ne    |    Ano    |    Přidejte všechny skupiny Nsg a pak zajistěte, aby tento port je-li otevřít toooutbound data.    |


## <a name="scaling-compute-resources"></a>Škálování výpočetních prostředků
S [automatické škálování](batch-automatic-scaling.md), může být služba Batch hello dynamicky upravoval hello počet výpočetních uzlů ve fondu podle toohello aktuální zatížení a využití prostředků výpočetního scénáře. To vám umožní toolower hello celkové náklady na provozování vaší aplikace pomocí pouze hello prostředky, je nutné, a uvolněním těch nepotřebujete.

Automatické škálování můžete zapnout napsáním [vzorce automatického škálování](batch-automatic-scaling.md#automatic-scaling-formulas) a jeho přidružením k fondu. Hello služba Batch používá hello vzorce toodetermine hello cílový počet uzlů ve fondu hello pro hello další interval škálování (interval, který můžete konfigurovat). Při vytvoření, nebo povolit škálování později ve fondu, můžete zadat nastavení automatického škálování hello pro fond. Můžete také aktualizovat hello škálování nastavení fondu s povoleným.

Jako příklad: úloha může vyžadovat odeslání velkého počtu toobe úlohy provést. Můžete přiřadit škálování vzorce toohello fondu, která se přizpůsobí hello počet uzlů ve fondu hello na základě hello aktuální počet úloh zařazených do fronty a míře dokončení hello hello úkoly v úloze hello. Hello služba Batch pravidelně vyhodnocuje vzorec hello a změní velikost fondu hello, na základě vytížení a další nastavení vzorce. Služba Hello přidá uzly podle potřeby, pokud existuje velký počet úloh zařazených do fronty a odebere uzly, pokud nejsou žádné zařazených do fronty nebo spuštěné úlohy.

Vzorec škálování může být založen na hello následující metriky:

* **Časová metrika** jsou na základě statistik shromažďovaných každých pět minut za hello zadaný počet hodin.
* **Metriky prostředků** jsou založeny na využití procesoru, šířky pásma a paměti a na počtu uzlů.
* **Metriky úkolů** jsou založeny na stavu úkolů, jako je například *Aktivní* (zařazený do fronty), *Spuštěný* nebo *Dokončený*.

Když automatické škálování sníží hello počet výpočetních uzlů ve fondu, musíte zvážit, jak snížit toohandle úloh, které jsou spuštěny v době hello hello operaci. tooaccommodate se Batch poskytuje *možnost navrácení uzlu* , kterou můžete zahrnout ve vzorcích. Například můžete zadat, že spuštěné úlohy se zastaví okamžitě, zastaví okamžitě a zařazena pro spuštění v jiném uzlu nebo povoleny toofinish před hello uzel je odebrán z fondu hello.

Další informace o automatickém škálování aplikace najdete v tématu [Automatické škálování výpočetních uzlů ve fondu Azure Batch](batch-automatic-scaling.md).

> [!TIP]
> toomaximize výpočetní využití prostředků, nastavte hello cílový počet uzlů toozero na konci hello úlohy, ale povolit toofinish spuštěné úlohy.
>
>

## <a name="security-with-certificates"></a>Zabezpečení pomocí certifikátů
Je obvykle třeba toouse certifikáty při šifrování nebo dešifrování citlivých informací pro úkoly, jako je hello klíč pro [účet úložiště Azure][azure_storage]. toosupport, můžete nainstalovat certifikáty na uzly. Šifrované tajné klíče jsou předány tootasks prostřednictvím parametrů příkazového řádku nebo vložené v jednom prostředků úkolu hello a hello nainstalované certifikáty lze použít toodecrypt je.

Použít hello [přidat certifikát] [ rest_add_cert] operace (Batch REST) nebo [CertificateOperations.CreateCertificate] [ net_create_cert] – metoda (Batch .NET) tooadd tooa certifikát účtu Batch. Certifikát hello můžete pak přidružit k novému nebo existujícímu fondu. Pokud je certifikát přidružený k fondu, hello služba Batch nainstaluje certifikát hello na každém uzlu ve fondu hello. Hello služba Batch nainstaluje příslušné certifikáty hello při hello spuštění uzlu před spuštěním úkolů (včetně hello spouštěcího úkolu a úkolu Správce úloh).

Pokud přidáte certifikáty tooan *existující* fondu, musíte restartovat svůj výpočetní uzly hello certifikáty toobe použít toohello uzly.

## <a name="error-handling"></a>Zpracování chyb
Může být nutné toohandle selhání úkolů a aplikací v rámci vašeho řešení Batch.

### <a name="task-failure-handling"></a>Zpracování selhání úkolů
Selhání úkolů spadá do následujících kategorií:

* **Selhání předběžného zpracování**

    Pokud se úloha nezdaří toostart, chyba předběžného zpracování nastavena pro úlohu hello.  

    Předběžné zpracování chyb může dojít, pokud byl přesunut hello úloh soubory prostředků, hello účet úložiště už není k dispozici nebo došlo k jinému problému, aby zabránila hello úspěšnému zkopírování souborů toohello uzlu.

* **Selhání nahrání souborů**

    Pokud z nějakého důvodu nahrávání souborů, které jsou určené pro úlohu selže, Chyba odeslání souboru nastavena pro úlohu hello.

    Odesílání chyb může dojít, pokud hello SAS zadaný pro přístup k úložišti Azure je neplatný nebo neposkytuje oprávnění k zápisu, pokud účet úložiště hello již není k dispozici, nebo pokud byl jiný problém došlo k která zabránila hello úspěšnému zkopírování souborů ze souboru uzel Hello.    

* **Selhání aplikace**

    Hello proces, který je zadán pomocí příkazového řádku úkolu hello také může selhat. Hello proces se považuje toohave selhalo při hello proces, který se spustí úloha hello vrátí nenulový ukončovací kód (viz *úloh ukončovací kód* v další části hello).

    Při selhání aplikace, můžete nakonfigurovat Batch tooautomatically opakování hello úloh si tooa zadaný počet opakování.

* **Selhání omezení**

    Můžete nastavit omezení, která určuje hello maximální dobu trvání provádění úlohy nebo úkolu, hello *maxWallClockTime*. To může být užitečné pro ukončení úlohy, které nesplní tooprogress.

    Při překročení maximální množství času hello hello úkol označen jako *Dokončit*, ale hello ukončovací kód je nastaven příliš`0xC000013A` a hello *schedulingError* pole je označen jako `{ category:"ServerError", code="TaskEnded"}`.

### <a name="debugging-application-failures"></a>Ladění chyb aplikace
* `stderr` a `stdout`

    Během provádění může aplikace generovat výstup diagnostiky, které můžete použít tootroubleshoot problémy. Jak je uvedeno v hello dříve části [souborů a adresářů](#files-and-directories), hello služba Batch zapíše standardní výstupní zařízení a standardní chyba výstup příliš`stdout.txt` a `stderr.txt` soubory v adresáři úkolů hello na hello výpočetního uzlu. Hello portál Azure nebo jeden z toodownload hello SDK služby Batch můžete použít tyto soubory. Například můžete načíst tyto a další soubory pro účely řešení problémů pomocí [ComputeNode.GetNodeFile] [ net_getfile_node] a [CloudTask.GetNodeFile] [ net_getfile_task] v knihovny Batch .NET hello.

* **Ukončovací kódy úkolů**

    Jak už bylo zmíněno dříve, je úkol označen jako neúspěšná službou Batch hello, pokud proces hello spuštěný úlohou hello vrátí nenulový ukončovací kód. Když úloha provede proces, Batch naplní hello úloh ukončovací kód vlastnost s hello *návratový kód procesu hello*. Je důležité toonote, který je úkolu ukončovací kód **není** dáno hello služby Batch. Úkolu ukončovací kód je určen podle samotný proces hello nebo hello operační systém, na které hello proces spuštěný.

### <a name="accounting-for-task-failures-or-interruptions"></a>Monitorování účtů pro selhání úkolů nebo přerušení
Úkoly mohou občas selhat nebo být přerušeny. Hello vlastní aplikace úkolu může selhat, hello uzlu, na které hello je spuštěn úkol může být restartován nebo uzel hello může být odebrán z fondu hello během operace změny velikosti Pokud Pokud hello zásady zrušení přidělení fondu je sada tooremove uzlů okamžitě bez čekání na toofinish úlohy. Ve všech případech hello úloha může být automaticky zařazena dávkou pro spuštění v jiném uzlu.

Je také možné dočasný problém toocause toohang úloh nebo trvat příliš dlouho tooexecute. Můžete nastavit interval hello maximální provádění úlohy. Pokud dojde k překročení maximální provádění interval hello, hello služba Batch přeruší aplikaci úkolu hello.

### <a name="connecting-toocompute-nodes"></a>Připojení toocompute uzly
Můžete provést další ladění a řešení potíží s přihlášením tooa výpočetním uzlu vzdáleně. Můžete použít pro uzly Windows hello Azure portálu toodownload soubor protokolu RDP (Remote Desktop) a získat informace o připojení Secure Shell (SSH) pro Linuxové uzly. Můžete to provést taky pomocí hello rozhraní API služby Batch – například s [Batch .NET] [ net_rdpfile] nebo [Batch Python](batch-linux-nodes.md#connect-to-linux-nodes-using-ssh).

> [!IMPORTANT]
> tooconnect tooa uzlu prostřednictvím protokolu RDP nebo SSH, musíte nejdřív vytvořit uživateli na uzlu hello. toodo, můžete použít hello portál Azure, [přidat uživatele účtu tooa uzel] [ rest_create_user] pomocí hello Batch REST API volat hello [ComputeNode.CreateComputeNodeUser] [ net_create_user] metoda v Batch .NET nebo volání hello [add_user] [ py_add_user] metoda v modulu Batch Python hello.
>
>

### <a name="troubleshooting-problematic-compute-nodes"></a>Řešení potíží s problematickými výpočetními uzly
V situacích, kdy některé úkoly selhávají můžete zkontrolovat hello metadata tooidentify úlohy se nezdařilo hello identifikovala uzel, který Batch klientská aplikace nebo služby. Každý uzel ve fondu je přiřazeno jedinečné číslo ID a hello uzlu, na kterém běží úlohy je zahrnut v metadatech úkolu hello. Po identifikaci problémového uzlu s ním můžete provést několik akcí:

* **Restartovat uzel hello** ([REST][rest_reboot] | [.NET][net_reboot])

    Restartování hello uzlu může někdy odstranit latentní problémy, jako jsou zablokované nebo zhroucené procesy. Všimněte si, že pokud váš fond používá spouštěcí úkol nebo úkol přípravy úlohy, jsou prováděna při restartování uzlu hello.
* **Obnovení z Image hello uzlu** ([REST][rest_reimage] | [.NET][net_reimage])

    Tato možnost přeinstaluje hello operační systém na uzlu hello. Stejně jako u restartování uzlu zahájení úloh a přípravy úlohy znovu spuštěny poté, po hello uzel má byl obnoven z Image.
* **Odebrat hello uzel z fondu hello** ([REST][rest_remove] | [.NET][net_remove])

    Někdy je nezbytné toocompletely odebrat hello uzel z fondu hello.
* **Zakázat plánování úkolů na uzlu hello** ([REST][rest_offline] | [.NET][net_offline])

    To efektivně převede hello uzlu do režimu offline, aby žádné další úlohy jsou přiřazeny tooit, ale umožňuje hello uzlu tooremain spuštěná a ve fondu hello. To vám umožní tooperform další šetření do hello příčina hello selhání bez ztráty dat hello nezdařené úlohy a bez hello uzel způsobil selhání dalších úkolů. Můžete například zakázat plánování úkolů na uzlu hello, pak [přihlásit vzdáleně](#connecting-to-compute-nodes) tooexamine hello protokoly událostí uzlu nebo provádět jiné řešení potíží. Jakmile dokončíte šetření, můžete převést uzel hello zpět do režimu online povolením plánování úkolů ([REST][rest_online] | [.NET] [ net_online]), nebo proveďte jeden z hello dalších akcí uvedených výše.

> [!IMPORTANT]
> S každou akci, která je popsaná v této části--restartování, obnovení z Image, odebrání a zakázat plánování úkolů--budete moct toospecify zpracování úlohy aktuálně spuštěné na uzlu hello při provádění akce hello. Například když zakážete plánování úkolů na uzlu pomocí klientské knihovny Batch .NET hello, můžete zadat [DisableComputeNodeSchedulingOption] [ net_offline_option] výčtu hodnota toospecify zda příliš**Ukončit** spouštění úloh, **znovu** je pro plánování na jiných uzlech nebo umožnit spuštěné úlohy toocomplete před provedením akce hello (**TaskCompletion**).
>
>

## <a name="next-steps"></a>Další kroky
* Další informace o hello [nástroje a rozhraní API služby Batch](batch-apis-tools.md) dostupné pro vytváření řešení Batch.
* Provede ukázkovou aplikaci Batch krok za krokem v [začít pracovat s hello Azure Batch Library pro .NET](batch-dotnet-get-started.md). K dispozici je také [verze Python](batch-python-tutorial.md) hello kurzu spuštěnou úlohu v systému Linux výpočetních uzlů.
* Stáhněte si a sestavte hello [Batch Explorer] [ github_batchexplorer] ukázkový projekt pro použití při vývoji řešení Batch. Pomocí Průzkumníka služby Batch hello, můžete provést následující hello a další:

  * Monitorování a manipulace s fondy, úlohami a úkoly v rámci vašeho účtu Batch
  * Stažení `stdout.txt`, `stderr.txt` a dalších souborů z uzlů
  * Vytvořit uživatele na uzlech a stáhnout soubory protokolu RDP pro vzdálené přihlášení
* Zjistěte, jak příliš[vytvořit fondy Linuxových výpočetních uzlů](batch-linux-nodes.md).
* Navštivte hello [fórum Azure Batch] [ batch_forum] na webu MSDN. fórum Hello je správné umístění tooask otázky, ať už jsou jenom získávání informací nebo jste odborník v pomocí služby Batch.

[1]: ./media/batch-api-basics/node-folder-structure.png

[azure_storage]: https://azure.microsoft.com/services/storage/
[batch_forum]: https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurebatch
[cloud_service_sizes]: ../cloud-services/cloud-services-sizes-specs.md
[msmpi]: https://msdn.microsoft.com/library/bb524831.aspx
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_sample_taskdeps]:  https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/TaskDependencies
[github_batchexplorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[batch_net_api]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[msdn_env_vars]: https://msdn.microsoft.com/library/azure/mt743623.aspx
[net_cloudjob_jobmanagertask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobmanagertask.aspx
[net_cloudjob_priority]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.priority.aspx
[net_cloudpool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx
[net_cloudtask_env]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.environmentsettings.aspx
[net_create_cert]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificateoperations.createcertificate.aspx
[net_create_user]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.createcomputenodeuser.aspx
[net_getfile_node]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.getnodefile.aspx
[net_getfile_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.getnodefile.aspx
[net_job_env]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.commonenvironmentsettings.aspx
[net_multiinstancesettings]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.aspx
[net_onalltaskscomplete]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.onalltaskscomplete.aspx
[net_rdp]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.getrdpfile.aspx
[net_reboot]: https://msdn.microsoft.com/library/azure/mt631495.aspx
[net_reimage]: https://msdn.microsoft.com/library/azure/mt631496.aspx
[net_remove]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.removefrompoolasync.aspx
[net_offline]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.disableschedulingasync.aspx
[net_online]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.enableschedulingasync.aspx
[net_offline_option]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.common.disablecomputenodeschedulingoption.aspx
[net_rdpfile]: https://msdn.microsoft.com/library/azure/Mt272127.aspx
[vnet]: https://msdn.microsoft.com/library/azure/dn820174.aspx#bk_netconf

[py_add_user]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.ComputeNodeOperations.add_user

[batch_rest_api]: https://msdn.microsoft.com/library/azure/Dn820158.aspx
[rest_add_job]: https://msdn.microsoft.com/library/azure/mt282178.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[rest_add_cert]: https://msdn.microsoft.com/library/azure/dn820169.aspx
[rest_add_task]: https://msdn.microsoft.com/library/azure/dn820105.aspx
[rest_create_user]: https://msdn.microsoft.com/library/azure/dn820137.aspx
[rest_get_task_info]: https://msdn.microsoft.com/library/azure/dn820133.aspx
[rest_job_schedules]: https://msdn.microsoft.com/library/azure/mt282179.aspx
[rest_multiinstance]: https://msdn.microsoft.com/library/azure/mt637905.aspx
[rest_multiinstancesettings]: https://msdn.microsoft.com/library/azure/dn820105.aspx#multiInstanceSettings
[rest_update_job]: https://msdn.microsoft.com/library/azure/dn820162.aspx
[rest_rdp]: https://msdn.microsoft.com/library/azure/dn820120.aspx
[rest_reboot]: https://msdn.microsoft.com/library/azure/dn820171.aspx
[rest_reimage]: https://msdn.microsoft.com/library/azure/dn820157.aspx
[rest_remove]: https://msdn.microsoft.com/library/azure/dn820194.aspx
[rest_offline]: https://msdn.microsoft.com/library/azure/mt637904.aspx
[rest_online]: https://msdn.microsoft.com/library/azure/mt637907.aspx

[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/
