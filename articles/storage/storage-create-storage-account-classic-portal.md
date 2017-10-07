---
title: "aaaHow toocreate, spravovat nebo odstranit účet úložiště v hello portál Azure classic | Microsoft Docs"
description: "Vytvořit nový účet úložiště, spravovat klíče pro přístup k účtu nebo odstranit účet úložiště na hello portálu Azure. Získejte informace o účtech úložiště Standard a Premium."
services: storage
documentationcenter: 
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 5e4f4360-3f81-4d63-a0b1-e7771b67af11
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/23/2017
ms.author: robinsh
ms.openlocfilehash: 6ee2359e02c7c9e9c111e1fc87a6160bb8b785b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="about-azure-storage-accounts"></a>Informace o účtech Azure Storage
[!INCLUDE [storage-selector-portal-create-storage-account](../../includes/storage-selector-portal-create-storage-account.md)]

[!INCLUDE [storage-try-azure-tools](../../includes/storage-try-azure-tools.md)]

## <a name="overview"></a>Přehled
Úložiště Azure poskytuje účet můžete získat přístup ke službám Azure Blob, fronty, tabulky a soubor toohello ve službě Azure Storage. Váš účet úložiště poskytuje jedinečný obor názvů hello pro vaše datové objekty Azure Storage. Ve výchozím nastavení je hello data ve vašem účtu dostupná pouze tooyou hello vlastníka účtu.

Jsou dva druhy účtů úložiště:

* Standardní účet úložiště zahrnuje úložiště Blob, Table, Queue a File.
* Prémiový účet úložiště aktuálně podporuje jen disky virtuálních počítačů Azure. Podrobné vysvětlení úložiště Premium Storage najdete v tématu [Premium Storage: vysoce výkonné úložiště pro úlohy virtuálních počítačů Azure](storage-premium-storage.md).

## <a name="storage-account-billing"></a>Fakturace účtu úložiště
Fakturuje se využívání Azure Storage podle vašeho účtu úložiště. Náklady na úložiště se odvíjí od čtyř faktorů: kapacity úložiště, schématu replikace, transakcích úložiště a odchozích dat.

* Kapacita úložiště znamená toohow mnohem svého účtu úložiště využíváte, kterou používáte toostore data. Hello náklady na prosté uložení dat je určen podle množství dat, které ukládáte a jak se replikují.
* Replikace udává, kolik kopií vašich dat se spravuje zároveň a v jakých umístěních.
* Transakce naleznete tooall pro čtení a zápisu tooAzure operace úložiště.
* Odchozí data zahrnují toodata mimo oblast Azure přenášet. Po hello datům ve vašem účtu úložiště přistupuje aplikace, která není spuštěn v hello stejné oblasti, jestli které aplikace je Cloudová služba nebo jiný typ aplikace, a budou se vám účtovat odchozí data. (Pro služby Azure, můžete provést kroky toogroup datům a službám v hello stejná data centra tooreduce nebo odstranit data s nimi spojeným nákladům.)  

Hello [Azure Storage – ceny](https://azure.microsoft.com/pricing/details/storage) stránka obsahuje podrobné informace o cenách za kapacitu úložiště, replikaci a transakce. Hello [o cenách přenosů dat](https://azure.microsoft.com/pricing/details/data-transfers/) stránka obsahuje podrobné informace o sazbách za odchozí data.

Podrobné informace o kapacitě úložiště a cílech výkonnosti najdete v tématu [Škálovatelnost a cíle výkonnosti Azure Storage](storage-scalability-targets.md).

> [!NOTE]
> Když vytvoříte virtuální počítač Azure, účet úložiště se vytvoří pro vás automaticky v umístění nasazení hello Pokud již nemáte účet úložiště v tomto umístění. Takže není nutné toofollow hello kroků toocreate účet úložiště pro disky virtuálního počítače. název účtu úložiště Hello budou založeny na název virtuálního počítače hello. V tématu hello [dokumentace Azure Virtual Machines](https://azure.microsoft.com/documentation/services/virtual-machines/) další podrobnosti.
> 
> 

## <a name="create-a-storage-account"></a>vytvořit účet úložiště
1. Přihlaste se toohello [portálu Azure Classic](https://manage.windowsazure.com).
2. Klikněte na tlačítko **nový** na panelu hello v hello dolní části stránky hello. Vyberte **Data Services** | **Úložiště** a klikněte na **Rychlé vytvoření**.
   
    ![NovýÚčetÚložiště](./media/storage-create-storage-account-classic-portal/storage_NewStorageAccount.png)
3. Do pole **Adresa URL** zadejte název účtu úložiště.
   
   > [!NOTE]
   > Názvy účtů úložiště musí mít od 3 do 24 znaků a můžou obsahovat jenom čísla a malá písmena.
   > 
   > Váš název účtu úložiště musí být jedinečný v rámci Azure. Hello portálu Azure Classic označí, pokud je už zabraný hello název účtu úložiště, které vyberete.
   > 
   > 
   
    V tématu [koncové body účtu úložiště](#storage-account-endpoints) pod pro podrobností o název účtu úložiště hello jak budou používané tooaddress vašich objektů v Azure Storage.
4. V **umístění/skupině vztahů**, vyberte umístění vašeho účtu úložiště, který je zavřít tooyou nebo tooyour zákazníků. Pokud data ve vašem účtu úložiště bude přistupovat jiná služba Azure, například virtuální počítač Azure nebo Cloudová služba, můžete tooselect skupiny vztahů ze seznamu toogroup hello svůj účet úložiště hello jednoho datového centra s jinými službami Azure že používáte tooimprove výkonu a snížení nákladů.
   
    Nezapomeňte, že při vytváření účtu taky musíte vybrat nějakou skupinu vztahů. Nelze přesunout stávající skupiny vztahů tooan účtu. Další informace o skupinách vztahů najdete níže v tématu [Společné umístění služeb pomocí skupiny vztahů](#service-co-location-with-an-affinity-group).
   
   > [!IMPORTANT]
   > toodetermine, která umístění jsou dostupná pro vaše předplatné, můžete volat hello [zobrazit seznam všech poskytovatelů prostředků](https://msdn.microsoft.com/library/azure/dn790524.aspx) operaci. volání toolist poskytovatelů z prostředí PowerShell, [Get-AzureLocation](https://msdn.microsoft.com/library/azure/dn757693.aspx). Z rozhraní .NET, použijte hello [seznamu](https://msdn.microsoft.com/library/azure/microsoft.azure.management.resources.provideroperationsextensions.list.aspx) metoda hello třídy ProviderOperationsExtensions.
   > 
   > V tématu [Oblasti Azure](https://azure.microsoft.com/regions/#services) najdete další informace o tom, které služby jsou dostupné v konkrétních oblastech.
   > 
   > 
5. Pokud máte více než jedno předplatné, pak hello **předplatné** zobrazí pole. V **předplatné**, zadejte hello předplatného Azure, které chcete toouse hello účet úložiště.
6. V **replikace**, vyberte hello požadovanou úroveň replikace pro váš účet úložiště. Hello doporučená možnost replikace je geo redundantní replikaci, která poskytuje maximální odolnost pro vaše data. Další informace o možnostech replikace pro Azure Storage najdete v tématu [Replikace Azure Storage](storage-redundancy.md).
7. Klikněte na **Vytvořit účet úložiště**.
   
    Může trvat několik minut toocreate účtu úložiště. Stav hello toocheck, můžete sledovat oznámení hello dole hello hello portálu Azure Classic. Po vytvoření účtu úložiště hello má váš nový účet úložiště **Online** stavu a je připravený k použití.

![StoragePage](./media/storage-create-storage-account-classic-portal/Storage_StoragePage.png)

### <a name="storage-account-endpoints"></a>Koncové body účtu úložiště
Každý objekt, který uložíte v úložišti Azure Storage, má jedinečnou adresu URL. forms název účtu úložiště Hello hello subdoménu dané adresy. Hello kombinace názvu domény a subdomény, která je služba konkrétní tooeach, tvoří *koncový bod* pro váš účet úložiště.

Například pokud je název účtu úložiště *můj_účet_úložiště*, pak jsou výchozí koncové body hello pro váš účet úložiště:

* Služba Blob service: http://*můj_účet_úložiště*.blob.core.windows.net
* Služba Table: http://*můj_účet_úložiště*.table.core.windows.net
* Služba front: http://*můj_účet_úložiště*.queue.core.windows.net
* Služba souborů: http://*můj_účet_úložiště*.file.core.windows.net

Zobrazí se hello koncové body pro váš účet úložiště na hello panelu úložiště v hello [portálu Azure Classic](https://manage.windowsazure.com) po vytvoření účtu hello.

Hello adresu URL pro přístup k objektu v účtu úložiště je sestavena připojením umístění objektu hello v hello koncový bod toohello účtu úložiště. Například adresa účtu pro objekty blob může mít tento formát: http://*můj_účet_úložiště*.blob.core.windows.net/*můj_kontejner*/*můj_objekt_blob*.

Můžete také nakonfigurovat toouse název vlastní doménu se svým účtem úložiště. Podrobnější informace najdete v tématu [Konfigurace vlastního názvu doménu pro koncový bod služby Blob Storage](storage-custom-domain-name.md).

### <a name="service-co-location-with-an-affinity-group"></a>Společné umístění služeb pomocí skupiny vztahů
*Skupina vztahů* je geografické seskupení vašich služeb Azure a virtuálních počítačů a vaším účtem úložiště Azure. Skupina vztahů může zlepšit výkon služby vyhledáním úloh počítačů v hello stejná data center nebo v blízkosti hello cílové skupiny uživatelů. Navíc žádné poplatky se vám neúčtují odchozí při přístupu k datům v účtu úložiště z jiné služby, který je součástí hello stejnou skupinu vztahů.

> [!NOTE]
> toocreate skupiny vztahů, otevřete hello <b>nastavení</b> oblasti hello [portálu Azure Classic](https://manage.windowsazure.com), klikněte na tlačítko <b>skupiny vztahů</b>a pak klikněte buď <b>přidat Skupina vztahů</b> nebo hello <b>přidat</b> tlačítko. Můžete také vytvořit a spravovat skupiny vztahů pomocí hello Azure Service Management API. Další informace najdete v tématu <a href="http://msdn.microsoft.com/library/azure/ee460798.aspx">Operace pro skupiny vztahů</a>.
> 
> 

## <a name="view-copy-and-regenerate-storage-access-keys"></a>Zobrazení, kopírování a opětovné vygenerování přístupových klíčů k úložišti
Při vytváření účtu úložiště vygeneruje Azure dva 512bitové přístupové klíče k úložišti, které se použijí k ověření při přístupu k účtu úložiště hello. Poskytnutím dvou přístupových klíčů k úložišti Azure vám umožní tooregenerate hello klíče bez přerušení tooyour úložiště služby a služby toothat přístup.

> [!NOTE]
> Doporučujeme vám přístupové klíče k úložišti s nikým nesdílet. toopermit toostorage přístup k prostředkům bez poskytnutí přístupových klíčů, můžete použít *sdílený přístupový podpis*. Sdílený přístupový podpis poskytuje přístup tooa prostředků ve vašem účtu pro časový interval, který definujete a s hello oprávnění, které zadáte. Další informace najdete v tématu [Použití sdílených přístupových podpisů (SAS)](storage-dotnet-shared-access-signature-part-1.md).
> 
> 

V hello [portálu Azure Classic](https://manage.windowsazure.com), použijte **Správa klíčů** na řídicím panelu hello nebo hello **úložiště** stránka tooview, kopírování a opětovné vytváření hello přístupových klíčů úložiště, které se používají tooaccess hello služby objektů Blob, Table a Queue.

### <a name="copy-a-storage-access-key"></a>Zkopírování přístupového klíče k úložišti
Můžete použít **Správa klíčů** toocopy klíče toouse úložiště přístup v připojovacím řetězci. připojovací řetězec Hello vyžaduje název účtu úložiště hello a klíče toouse v ověřování. Informace o konfiguraci připojovací řetězce tooaccess služby Azure storage, najdete v části [nakonfigurování připojovacích řetězců úložiště Azure](storage-configure-connection-string.md).

1. V hello [portálu Azure Classic](https://manage.windowsazure.com), klikněte na tlačítko **úložiště**a potom klikněte na název hello hello úložiště účet tooopen hello řídicího panelu.
2. Klikněte na **Správu klíčů**.
   
     Otevře se **Správa přístupových klíčů**.
   
    ![Správaklíčů](./media/storage-create-storage-account-classic-portal/Storage_ManageKeys.png)
3. toocopy přístupový klíč k úložišti, vyberte hello text klíče. Potom na něj klikněte pravým tlačítkem a klikněte na **Kopírovat**.

### <a name="regenerate-storage-access-keys"></a>Opětovné vygenerování přístupových klíčů k úložišti
Doporučujeme vám, že změníte hello přístupové klíče účtu úložiště tooyour pravidelně toohelp zajistit zabezpečení připojení k úložišti. Dva přístupové klíče se přiřazují proto, abyste mohli pro účet úložiště toohello připojení pomocí jeden přístupový klíč, zatímco si znovu vygenerujete hello druhý přístupový klíč.

> [!WARNING]
> Opětovné generování přístupových klíčů může ovlivnit služby v Azure, jakož i vlastní aplikace, které jsou závislé na účtu úložiště hello. Všichni klienti, kteří používají účet úložiště hello přístupu k klíče tooaccess hello musí být aktualizované toouse hello nový klíč.
> 
> 

**Služba Media services** – Pokud máte služby media services, které jsou závislé na vašem účtu úložiště, musíte znovu synchronizovat hello přístupové klíče s touto službou po opětovném vygenerování klíčů hello.

**Aplikace** – Pokud máte webové aplikace nebo cloudových služeb daný účet úložiště hello použijte, pokud, ztratíte připojení hello obnovit klíče, klíče nezaregistrujete. 

**Průzkumníci úložiště** – Pokud používáte některou [aplikací Průzkumníka úložiště](storage-explorers.md), budete pravděpodobně potřebovat tooupdate hello úložiště klíč používaný těmito aplikacemi.

Tady je hello proces pro výměnu přístupových klíčů úložiště:

1. Aktualizujte připojovací řetězce hello ve vaší aplikaci kód tooreference hello sekundární přístupový klíč účtu úložiště hello.
2. Znovu vygenerujte primární přístupový klíč hello vašeho účtu úložiště. V hello [portálu Azure Classic](https://manage.windowsazure.com), z řídicího panelu hello nebo hello **konfigurace** klikněte na tlačítko **Správa klíčů**. Klikněte na tlačítko **znovu vygenerovat** v části hello primární přístupový klíč a potom klikněte na **Ano** tooconfirm, které chcete toogenerate nový klíč.
3. Aktualizujte hello připojovacích řetězců v kódu tooreference hello nové primární přístupový klíč.
4. Znova vygenerujte sekundární přístupový klíč hello.

## <a name="delete-a-storage-account"></a>Odstranění účtu úložiště
tooremove účet úložiště, který už nepoužíváte, použijte **odstranit** na řídicím panelu hello nebo hello **konfigurace** stránky. **Odstranit** hello odstraní celý účet úložiště, včetně všech hello objekty BLOB, tabulek a v účtu hello.

> [!WARNING]
> Není možné toorestore je účet úložiště se odstranil nebo načíst žádný obsah hello, který obsahuje před odstraněním. Být jisti tooback až nic chcete toosave před odstraněním účtu hello. To platí také pro všechny prostředky v hello účtu – po odstranění jsou objekt blob, tabulka, fronta nebo soubor, je trvale odstraněn.
> 
> Pokud váš účet úložiště obsahuje soubory virtuálního pevného disku pro virtuální počítač Azure, pak musíte odstranit všechny Image a disky, které používají tyto soubory virtuálního pevného disku, než budete moct odstranit účet úložiště hello. Nejprve zastavit hello virtuální počítač, pokud běží a poté jej odstraňte. toodelete disky, přejděte toohello **disky** a odstraňte požadované disky. toodelete Image, přejděte toohello **bitové kopie** a odstraňte všechny Image, které jsou uložené v účtu hello.
> 
> 

1. V hello [portálu Azure Classic](https://manage.windowsazure.com), klikněte na tlačítko **úložiště**.
2. Klikněte kamkoli do položky účtu úložiště hello kromě hello název a pak klikněte na **odstranit**.
   
     - nebo -
   
    Klikněte na název hello hello úložiště účet tooopen hello řídicího panelu a pak klikněte na **odstranit**.
3. Klikněte na tlačítko **Ano** tooconfirm, které chcete účet úložiště toodelete hello.

## <a name="next-steps"></a>Další kroky
* toolearn Další informace o Azure Storage, najdete v části hello [dokumentaci pro Azure Storage](https://azure.microsoft.com/documentation/services/storage/).
* Navštivte hello [Blog týmu Azure Storage](http://blogs.msdn.com/b/windowsazurestorage/).
* [Přenos dat pomocí příkazového řádku Azcopy hello](storage-use-azcopy.md)

