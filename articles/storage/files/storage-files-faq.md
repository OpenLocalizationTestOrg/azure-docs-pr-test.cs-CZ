---
title: "aaaFrequently kladené dotazy týkající se Azure File storage | Microsoft Docs"
description: "Vyhledejte odpovědi toofrequently kladené dotazy týkající se Azure File storage."
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
ms.date: 07/19/2017
ms.author: renash
ms.openlocfilehash: d8a94fdc77e928dbc6996e1e635cd3527e0c67d4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="frequently-asked-questions-about-azure-file-storage"></a>Nejčastější dotazy o úložišti Azure File

## <a name="general"></a>Obecné
* **OTÁZKY. Co je Azure File storage?**  
   
    Úložiště Azure File je distribuovaný systém souborů v Azure. Poskytuje rozhraní protokolu SMB, která umožňuje uživatelům toomount hello úložiště jako nativní sdílenou složku na podporovaném virtuálním počítači Azure nebo na místním počítači.

* **OTÁZKY. Proč je užitečné Azure File storage?**  
   
    Azure File storage poskytuje přístup sdílená data napříč více virtuálních počítačů a platformy. Odkazovat příliš[proč Azure File storage je užitečné](storage-files-introduction.md#why-azure-file-storage-is-useful).

* **OTÁZKY. Kdy použít Azure File vs objektů Blob v Azure vs disku Azure?**  
   
    Microsoft Azure poskytuje několik způsobů přístupu a toostore data v cloudu hello.  
   
    Úložiště Azure File – poskytuje rozhraní protokolu SMB, knihovny klienta a rozhraní REST, která umožňuje snadný přístup odkudkoli toostored soubory.  
   
    Azure objektů BLOB – poskytuje rozhraní REST, která umožňuje toobe Nestrukturovaná data, uložená a získat přístup v masivním měřítku v objekty BLOB bloku a knihovny klienta.  
   
    Azure Data disky – poskytuje klientské knihovny a rozhraní REST, který umožňuje data toobe trvale uložené a k němu přistupovat z připojených virtuálního pevného disku.  
   
    Další informace na [rozhodnutí při objektů BLOB Azure toouse, soubory Azure nebo Azure datových disků](../common/storage-decide-blobs-files-disks.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)

* **OTÁZKY. Jak můžu začít pracovat používání Azure File storage?**  
   
    Můžete spustit tak, že vytvoříte sdílenou složku Azure. Můžete vytvořit sdílené složky Azure File pomocí portálu Azure, hello rutiny prostředí PowerShell Azure Storage, knihovny klienta Azure Storage hello nebo hello REST API pro Azure Storage. V tomto kurzu se dozvíte:

    * [Zjistěte, jak toocreate Azure File sdílet pomocí hello portálu](storage-how-to-create-file-share.md#create-file-share-through-the-portal)
    * [Zjistěte, jak toocreate Azure File sdílet pomocí prostředí Powershell](storage-how-to-create-file-share.md#create-file-share-through-powershell)
    * [Zjistěte, jak toocreate Azure File sdílet pomocí rozhraní příkazového řádku](storage-how-to-create-file-share.md#create-file-share-through-command-line-interface-cli)

* **OTÁZKY. Jaké replikace podporuje Azure File storage?**  
   
    Úložiště Azure File podporuje pouze LRS nebo GRS hned teď. Plánujeme toosupport RA-GRS, ale neexistuje žádný časový horizont tooshare ještě.

## <a name="security-authentication-and-access-control"></a>Zabezpečení, ověřování a řízení přístupu

* **OTÁZKY. Jaké jsou různé způsoby tooaccess souborů v Azure File storage?**
    
    Můžete připojit hello sdílené složky na místním počítači pomocí protokolu SMB 3.0 nebo pomocí nástrojů, jako [Storage Explorer](http://storageexplorer.com/) tooaccess soubory do sdílené složky. Z vaší aplikace můžete použít knihovny klienta úložiště, rozhraní REST API nebo tooaccess prostředí Powershell, které sdílet vaše soubory v souboru Azure.

* **OTÁZKY. Jak může poskytnout přístup tooa konkrétní soubor pomocí webového prohlížeče?**
    
    Pomocí SAS můžete vygenerovat tokeny se specifickými oprávněními, které jsou platné pro zadaný časový interval. Můžete například vygenerovat token s oprávnění jen pro čtení tooa určitého souboru pro určitého časového období. Každý, kdo má tuto adresu url můžete přístup hello souboru přímo z libovolného webového prohlížeče, ale je platný. Klíče SAS snadno vygenerujete z uživatelského rozhraní, jako je Průzkumník úložišť.

* **OTÁZKY. Je možné toospecify oprávnění jen pro čtení nebo jen pro zápis pro složky ve sdílené složce hello?**
    
    Tato úroveň kontroly nad oprávněními nemáte, pokud připojíte sdílenou složku hello přes protokol SMB. Můžete to však dosáhnout vytvořením sdíleného přístupového podpisu (SAS) přes hello REST API nebo knihovny klienta.  

* **OTÁZKY. Jak lze povolit šifrování na straně serveru pro Azure File storage?**

    [Šifrování na straně serveru](../common/storage-service-encryption.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) pro Azure File storage je všeobecně dostupná v všech oblastech a veřejných a národních cloudů. Pro používání Azure File storage můžete povolit SSE [portál Azure](https://portal.azure.com/),[API zprostředkovatele služby Microsoft Azure Storage prostředků](/rest/api/storagerp/storageaccounts), [prostředí Azure Powershell](https://msdn.microsoft.com/library/azure/mt607151.aspx) nebo [rozhraní příkazového řádku Azure](../common/storage-azure-cli.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).
    
    Po povolení SSE na Azure File storage, žádná nová data zapsána úložiště file toohello v daném účtu úložiště automaticky zašifruje. Tato funkce je dostupná pro všechna nová data tooexisting nebo nové sdílené složky na stávajícího nebo nového účtu úložiště. Povolení této funkce je bez dalších poplatků. Další informace na [jak tooenable SSE na Azure File storage](../common/storage-service-encryption.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

* **OTÁZKY. Ověřování na základě služby Active Directory podporuje Azure File storage?**
   
    Aktuálně nepodporujeme ověření založené na AD nebo ACL, je to však na našem seznamu požadovaných funkcí. Teď klíče účtu úložiště Azure hello jsou použité tooprovide ověřování toohello sdílené složky. Nabízíme alternativní řešení používá sdílené přístupové podpisy (SAS) přes REST API nebo hello hello klientské knihovny. Pomocí SAS můžete vygenerovat tokeny se specifickými oprávněními, které jsou platné pro zadaný časový interval. Můžete například vygenerovat token s danou soubor s vypršení platnosti 10 minut tooa oprávnění jen pro čtení. Každý, kdo takový token má po dobu jeho platnosti má oprávnění jen pro čtení toothat soubor pro těchto 10 minut.
   
    SAS se podporuje jen přes hello REST API nebo knihovny klienta. Když připojíte sdílenou složku hello přes hello protokolu SMB, nemůžete použít SAS toodelegate přístup tooits obsah. 
    
* **OTÁZKY. Co jsou zásady dodržování předpisů data hello je podporované pro Azure File storage?**

   Úložiště Azure soubor spouští na hello stejnou architekturu úložiště jako další úložiště služby ve službě Azure Storage a platí hello stejné zásady dodržování předpisů data. Další informace o dodržování předpisů dat úložiště Azure, můžete stáhnout a odkazovat příliš[dokumentu Microsoft Azure Data Protection](http://go.microsoft.com/fwlink/?LinkID=398382&clcid=0x409).

## <a name="on-premises-access"></a>Místní přístup

* **Je nutné úložiště File toouse Azure ExpressRoute tooconnect tooAzure z virtuálního počítače místní Q.Do?**
   
    Ne. Pokud nemáte ExpressRoute, můžete ke sdílené složce hello z místního tak dlouho, dokud máte port 445 (odchozí TCP) otevřený pro přístup k Internetu. Však můžete ExpressRoute s úložištěm Azure File Pokud chcete.

* **OTÁZKY. Jak můžete připojit Azure sdílené složky na místním počítači?** 
    
    Jak dlouho, dokud je otevřený port 445 (odchozí TCP) a váš klient podporuje protokol SMB 3.0 hello se můžete připojit sdílenou složku hello přes hello protokolu SMB (například používáte Windows 10 nebo Windows Server 2012). Spojte se s místního portu hello zprostředkovatele toounblock poskytovatele internetových služeb. V dočasné hello, můžete zobrazit vaše soubory pomocí [Storage Explorer](../../vs-azure-tools-storage-explorer-files.md#view-a-file-shares-contents).


## <a name="billing-and-pricing"></a>Fakturaci a cenách
* **OTÁZKY. Hello síťový provoz mezi virtuálním počítači Azure a sdílenou složkou jako externí šířka pásma, která je účtován toohello předplatné?**
   
    Pokud hello sdílené složky a virtuálních počítačů v hello stejné oblasti Azure, hello provoz mezi nimi je zdarma. Pokud jsou v různých oblastech, provoz hello mezi nimi bude účtován jako externí šířky pásma.

## <a name="backup"></a>Zálohování

* **OTÁZKY. Jak mohu zálohovat Moje sdílené složky Azure?**
    
    Můžete použít AzCopy, RoboCopy, nebo 3. stran zálohování nástroj, který může zálohovat připojené sdílené složky. Očekáváme, že v blízké budoucnosti; hello toohave hello možnost tootake snímky sdílených složek bude moct toouse sdílené složky Azure File toobackup této funkce.

## <a name="performance"></a>Výkon

* **OTÁZKY. Jaká jsou omezení škálování hello Azure File storage?**
    Informace o škálovatelnosti a cílech výkonnosti pro Azure File storage najdete v tématu [a cíle výkonnosti služby Azure Storage Scalability](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#scalability-targets-for-blobs-queues-tables-and-files).

* **OTÁZKY. Moje šlo to velmi pomalu při pokusu o toounzip soubory do úložiště Azure File. Co bych měl/a dělat?**
    
    tootransfer velké počty souborů do úložiště Azure File, doporučujeme použít AzCopy (Windows, verze Preview pro Linux a Unix) nebo Azure Powershell jako tyto nástroje jsou optimalizované pro síťové přenosy.

* **OTÁZKY. Co opravy bylo vydáno toofix pomalý výkon problém s Azure File storage?**
    
    Hello tým Windows nedávno vydal toofix oprava problému s nízkou rychlostí, když hello zákazník přistupuje k Azure File storage z Windows 8.1 nebo Windows Server 2012 R2. Další informace, prosím rezervaci hello přidružené článku znalostní báze KB, [pomalý výkon při přístupu k Azure File storage z Windows 8.1 nebo Server 2012 R2](https://support.microsoft.com/kb/3114025).

## <a name="features-and-interoperability-with-other-services"></a>Funkce a vzájemná funkční spolupráce s jinými službami
* **OTÁZKY. Je "určující sdílená složka" pro cluster s podporou převzetí služeb při selhání mezi hello případy používání pro Azure File storage?**
   
    To není aktuálně podporováno.

* **OTÁZKY. Je k dispozici v hello REST API operaci přejmenovat?**
   
    V tuto chvíli to není možné.

* **OTÁZKY. Může mít člověk vnořené sdílené složky, jinými slovy sdílenou složku ve sdílené složce?**
    
    Ne. Hello sdílené složky je hello virtuální jednotka, kterou můžete připojit, takže vnořené sdílené složky se nepodporují.

* **OTÁZKY. Používání Azure File storage s IBM MQ**
    
    Společnost IBM vydala dokumentu tooguide IBM MQ zákazníci při konfiguraci Azure File storage pro jejich službu. Další informace najdete v článku [jak toosetup IBM MQ Multi instance správce front službou Microsoft Azure File](https://github.com/ibm-messaging/mq-azure/wiki/How-to-setup-IBM-MQ-Multi-instance-queue-manager-with-Microsoft-Azure-File-Service).


## <a name="troubleshooting"></a>Řešení potíží
* **OTÁZKY. Jak odstranit chyby Azure File storage?**
    
    Můžete se podívat příliš[Azure File storage článku Poradce při potížích s](storage-troubleshoot-windows-file-connection-problems.md) pro pokyny k odstraňování problémů začátku do konce. 


## <a name="see-also"></a>Viz také
Další informace o úložišti Azure File jsou dostupné na těchto odkazech.

### <a name="conceptual-articles-and-videos"></a>Koncepční články a videa
* [Azure File Storage: hladký cloudový souborový systém SMB pro Windows a Linux](https://azure.microsoft.com/documentation/videos/azurecon-2015-azure-files-storage-a-frictionless-cloud-smb-file-system-for-windows-and-linux/)
* [Jak toouse Azure File storage s Linuxem](storage-how-to-use-files-linux.md)

### <a name="tooling-support-for-file-storage"></a>Podpora nástrojů pro úložiště File
* [Jak toouse AzCopy s Microsoft Azure Storage](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)
* [Použití hello rozhraní příkazového řádku Azure s Azure Storage](../common/storage-azure-cli.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)
* [Řešení potíží se službou Azure File Storage](storage-troubleshoot-linux-file-connection-problems.md)

### <a name="blog-posts"></a>Příspěvky na blozích
* [Úložiště Azure File je nyní dostupné pro veřejnost](https://azure.microsoft.com/blog/azure-file-storage-now-generally-available/)
* [Uvnitř Azure File Storage](https://azure.microsoft.com/blog/inside-azure-file-storage/)
* [Představujeme službu Microsoft Azure File](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx)
* [Migrace dat tooAzure úložiště File](https://azure.microsoft.com/blog/migrating-data-to-microsoft-azure-files/)

### <a name="reference"></a>Referenční informace
* [Klientská knihovna Storage pro .NET – referenční informace](https://msdn.microsoft.com/library/azure/dn261237.aspx)
* [REST API služby File – referenční informace](http://msdn.microsoft.com/library/azure/dn167006.aspx)
