---
title: "aaaIntroduction tooAzure úložiště File | Microsoft Docs"
description: "Úvod tooAzure úložiště souboru, která umožňuje souborů v síti složky v cloudu Microsoft hello"
services: storage
documentationcenter: 
author: RenaShahMSFT
manager: aungoo
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/27/2017
ms.author: renash
ms.openlocfilehash: fe6826e79c364a6956831d2e273c4342a5fd47f3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooazure-file-storage"></a>Úvod tooAzure úložiště File

Azure File storage nabízí sdílené síťové složky v cloudu hello pomocí standardní hello [zpráva bloku protokol Server (SMB)](https://msdn.microsoft.com/library/windows/desktop/aa365233.aspx) a [Common Internet File System (CIFS)](https://technet.microsoft.com/library/cc939973.aspx). Sdílené složky Azure může současně připojit služba Azure Virtual Machines a místní nasazení se systémem Windows, macOS nebo Linux. Poskytuje účet úložiště pro obecné účely přístup tooAzure File storage, Azure Blob storage a Azure Queue storage.

## <a name="videos"></a>Videa
| Představení služby Azure File Storage (27 minut) | Kurz služby Azure File Storage (5 minut)  |
|-|-|
| [![Klikněte na tlačítko záznam dění na monitoru hello představení Azure File storage videa - tooplay!](./media/storage-files-introduction/azure-files-introduction-video-snapshot1.png)](https://www.youtube.com/watch?v=zlrpomv5RLs) | [![Záznam dění na monitoru úložiště Azure File hello kurz – klikněte na tlačítko tooplay!](./media/storage-files-introduction/azure-files-introduction-video-snapshot2.png)](https://channel9.msdn.com/Blogs/Azure/Azure-File-storage-with-Windows/) |

## <a name="why-azure-file-storage-is-useful"></a>Proč je služba Azure File Storage užitečná

Azure File storage vám umožní tooreplace serveru Windows, Linux, nebo na základě NAS souborové servery hostovaný místně nebo v cloudu hello se souborem cloudu operačního systému bez sdílení. Úložiště Azure File má hello následující výhody:

* **Sdílené přístupové** Azure sdílené složky odvětví hello podpory standardní protokol SMB, což znamená, že můžete bez obav o kompatibilitě aplikací bezproblémově nahradit vaše místní sdílené složky se sdílenými složkami Azure. Je možné tooaccess sdílené položky z více počítačů a aplikací nebo instance je významné výhody s Azure File storage.

* **Plně spravovaná** bez hello nutné toomanage hardware a operační systém, což znamená, nemáte toodeal s opravy OS serveru hello s upgrady kritické zabezpečení nebo výměna vadný pevné disky můžete vytvořit sdílené složky Azure File.

* **Skriptování a Tooling** rutiny prostředí PowerShell a rozhraní příkazového řádku Azure můžou být použité toocreate připojovat a spravovat sdílené složky Azure File jako součást hello správy aplikací Azure. Můžete vytvořit a spravovat sdílené složky Azure File pomocí hello [portál Azure](https://portal.azure.com) a hello [Azure Storage Explorer](https://storageexplorer.com). 

* **Odolnost proti chybám** Azure File storage sestavený z hello pozadí až toobe vždy k dispozici. Nahraďte místní sdílené složky Azure File storage znamená, že už máte toowake až toodeal s místní výpadky nebo problémů se sítí. 

* **Známé programovatelnosti** aplikace běžící v Azure můžete přístup k datům ve sdílené složce hello prostřednictvím [souboru vstupně-výstupních operací rozhraní API systému](https://msdn.microsoft.com/library/system.io.file.aspx). Vývojáři tedy můžou využít své stávající kód a dovednosti toomigrate stávající aplikace. Kromě toho tooSystem vstupně-výstupní operace rozhraní API, můžete použít všechny hello úložiště Azure klientské knihovny, například hello, jednu pro [.NET](/dotnet/api/overview/azure/storage?view=azure-dotnet), nebo hello [REST API pro Azure Storage](/rest/api/storageservices/file-service-rest-api).

Sdílené složky Azure umožňují:

* **Nahraďte místní souborové servery** Azure File storage lze použít toocompletely nahradit sdílené složky na tradičních místních souborových serverech nebo zařízeních NAS. Oblíbených operačních systémů, například Windows, systému macOS a Linux můžete snadno připojit Azure sdílené složky, kde se nacházejí v hello, world.

* **Migrace aplikací metodou „lift and shift“:**

    Úložiště Azure File je snadné příliš "navýšení a posunutí" toohello cloudu aplikace, který pomocí místního souboru sdílí tooshare dat mezi částí aplikace hello. tooimplement, každý virtuální počítač připojí toohello sdílené složky a potom může číst a zapisovat soubory stejně, jako je by proti soubor místní sdílené složky.

* **Zjednodušení vývoje pro cloud:**
    
    Úložiště Azure File lze použít v mnoha různými způsoby toosimplify nové cloudové vývoj projekty.
    
    * **Sdílená nastavení aplikací:**
    
        Běžný vzor pro distribuované aplikace je toohave konfigurační soubory v centrálním umístění, kde jsou dostupné z mnoha různých virtuálních počítačů. Tyto konfigurační soubory teď můžou být uložené ve sdílené složce Azure a můžou je číst všechny instance aplikací. Tato nastavení můžete taky spravovat prostřednictvím rozhraní REST hello, který umožňuje přístup po celém světě toohello konfigurační soubory.

    * **Sdílená složka pro diagnostiku:**
    
        Sdílenou složku Azure může být také použít toosave diagnostiky souborech, jako jsou protokoly, metriky a výpisy stavu systému. Sdílené složky, které jsou k dispozici prostřednictvím hello protokolu SMB a rozhraní REST s umožňuje toobuild aplikace nebo využít celou řadu nástrojů pro analýzu pro zpracování a analýzu hello diagnostická data.

    * **Vývoj, testování a ladění:**

        Když vývojáři nebo správci pracují na virtuálních počítačích v cloudu hello, potřebují často sadu nástrojů nebo pomůcek. Instalace a distribuce těchto pomůcek na každý virtuální počítač, kde jsou potřeba, může být časově náročné. S Azure File storage vývojáře nebo správce může ukládat své oblíbené nástroje ve sdílené složce, který lze snadno připojené toofrom jakéhokoli virtuálního počítače.
        
## <a name="how-does-it-work"></a>Jak to funguje?

Správa sdílených složek Azure je mnohem jednodušší než správa sdílených složek v místním prostředí. Hello následující diagram znázorňuje hello Azure File storage management konstrukce:

![Struktura souborů](./media/storage-files-introduction/files-concepts.png)

* **Účet úložiště** všechny přístup tooAzure úložiště se provádí prostřednictvím účtu úložiště. Podrobné informace o kapacitě účtu úložiště najdete v článku [Škálovatelnost a cíle výkonosti](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

* **Sdílená složka:** Sdílená složka služby File Storage představuje sdílenou složku protokolu SMB v Azure. Všechny adresáře a soubory musí být vytvořeny v nadřazené sdílené složce. Účet může obsahovat neomezený počet sdílených složek a sdílená složka můžete obsahovat neomezený počet souborů, až toohello 5 TB celkové kapacity hello sdílené složky.

* **Adresář:** Volitelná hierarchie adresářů.

* **Soubor** souboru ve sdílené složce hello. Soubor může mít až velikost too1 TB.

* **Formát adresy URL** soubory jsou adresovatelné hello následující formát adresy URL:  

    ```
    https://<storage account>.file.core.windows.net/<share>/<directory/directories>/<file>
    ```

## <a name="next-steps"></a>Další kroky

* [Vytvoření sdílené složky Azure](storage-how-to-create-file-share.md)
* [Připojení v systému Windows](storage-how-to-use-files-windows.md)
* [Připojení v systému Linux](storage-how-to-use-files-linux.md)
* [Připojení v systému macOS](storage-how-to-use-files-mac.md)
* [Nejčastější dotazy](../storage-files-faq.md)
* [Řešení potíží ve Windows](storage-troubleshoot-windows-file-connection-problems.md)   
* [Řešení potíží v Linuxu](storage-troubleshoot-linux-file-connection-problems.md)   

<!-- Rena I would remove any articles from here that are more than a year old. - Robin-->
### <a name="conceptual-articles-and-videos"></a>Koncepční články a videa
* [Azure File Storage: hladký cloudový souborový systém SMB pro Windows a Linux](https://azure.microsoft.com/documentation/videos/azurecon-2015-azure-files-storage-a-frictionless-cloud-smb-file-system-for-windows-and-linux/)

### <a name="tooling-support-for-azure-file-storage"></a>Podpora nástrojů pro službu Azure File Storage
* [Jak toouse AzCopy s Microsoft Azure Storage](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)
* [Použití hello rozhraní příkazového řádku Azure s Azure Storage](../common/storage-azure-cli.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#create-and-manage-file-shares)

### <a name="blog-posts"></a>Příspěvky na blozích
* [Úložiště Azure File je nyní dostupné pro veřejnost](https://azure.microsoft.com/blog/azure-file-storage-now-generally-available/)
* [Uvnitř Azure File Storage](https://azure.microsoft.com/blog/inside-azure-file-storage/)
* [Představujeme službu Microsoft Azure File](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx)
* [Migrace dat tooAzure souboru](https://azure.microsoft.com/blog/migrating-data-to-microsoft-azure-files/)

### <a name="reference"></a>Referenční informace
* [Klientská knihovna Storage pro .NET – referenční informace](https://msdn.microsoft.com/library/azure/dn261237.aspx)
* [REST API služby File – referenční informace](http://msdn.microsoft.com/library/azure/dn167006.aspx)
