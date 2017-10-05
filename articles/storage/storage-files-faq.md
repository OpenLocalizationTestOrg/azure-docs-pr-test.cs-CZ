---
title: "Časté otázky k Azure File storage | Microsoft Docs"
description: "Odpovědi na nejčastější dotazy týkající se Azure File storage."
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
ms.openlocfilehash: cb2134502a585c4f9772e594f635c10f1841e46f
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/29/2017
---
# <a name="frequently-asked-questions-about-azure-file-storage"></a>Nejčastější dotazy o úložišti Azure File

## <a name="general"></a>Obecné
* **OTÁZKY. Co je Azure File storage?**  
   
    Úložiště Azure File je distribuovaný systém souborů v Azure. Poskytuje rozhraní protokolu SMB, což umožňuje uživatelům připojit úložiště jako nativní sdílenou složku na podporovaném virtuálním počítači Azure nebo na místním počítači.

* **OTÁZKY. Proč je užitečné Azure File storage?**  
   
    Azure File storage poskytuje přístup sdílená data napříč více virtuálních počítačů a platformy. Odkazovat na [proč Azure File storage je užitečné](storage-files-introduction.md#why-azure-file-storage-is-useful).

* **OTÁZKY. Kdy použít Azure File vs objektů Blob v Azure vs disku Azure?**  
   
    Microsoft Azure poskytuje několik způsobů, jak ukládat a přistupovat k datům v cloudu.  
   
    Úložiště Azure File – poskytuje rozhraní protokolu SMB, knihovny klienta a rozhraní REST, která umožňuje snadno přistupovat z libovolného místa na uložených souborů.  
   
    Azure objektů BLOB – poskytuje rozhraní REST, která umožňuje nestrukturovaných dat na ukládat a přistupovat v masivním měřítku v objekty BLOB bloku a knihovny klienta.  
   
    Azure Data disky – poskytuje klientské knihovny a rozhraní REST, které umožňuje dat trvale ukládat a přistupovat z připojený virtuální pevný disk.  
   
    Další informace na [rozhodování o použití objektů BLOB služby Azure, Azure soubory nebo datové disky Azure](storage-decide-blobs-files-disks.md)

* **OTÁZKY. Jak můžu začít pracovat používání Azure File storage?**  
   
    Můžete spustit tak, že vytvoříte sdílenou složku Azure. Můžete vytvořit sdílené složky Azure File pomocí portálu Azure, rutin prostředí PowerShell Azure Storage, knihovny klienta Azure Storage nebo REST API pro Azure Storage. V tomto kurzu se dozvíte:

    * [Naučte se vytvořit sdílenou složku Azure File pomocí portálu](storage-file-how-to-create-file-share.md#create-file-share-through-the-portal)
    * [Naučte se vytvářet Azure sdílenou složku pomocí prostředí Powershell](storage-file-how-to-create-file-share.md#create-file-share-through-powershell)
    * [Naučte se vytvořit sdílenou složku Azure File pomocí rozhraní příkazového řádku](storage-file-how-to-create-file-share.md#create-file-share-through-command-line-interface-cli)

* **OTÁZKY. Jaké replikace podporuje Azure File storage?**  
   
    Úložiště Azure File podporuje pouze LRS nebo GRS hned teď. Plánujeme podporu pro RA-GRS, ale nemáme pro to teď žádný časový horizont.

## <a name="security-authentication-and-access-control"></a>Zabezpečení, ověřování a řízení přístupu

* **OTÁZKY. Jaké jsou různé způsoby, jak přistupovat k souborům v Azure File storage?**
    
    Můžete připojit sdílenou složku na místním počítači pomocí protokolu SMB 3.0 nebo pomocí nástrojů, jako [Storage Explorer](http://storageexplorer.com/) přístup k souborům ve sdílené složce. Z vaší aplikace můžete použít knihovny klienta úložiště, rozhraní API REST nebo Powershell získat přístup k souborům v Azure sdílené složky.

* **OTÁZKY. Jak může poskytnout přístup k konkrétní soubor pomocí webového prohlížeče?**
    
    Pomocí SAS můžete vygenerovat tokeny se specifickými oprávněními, které jsou platné pro zadaný časový interval. Můžete například vygenerovat token s přístupem pouze pro čtení ke konkrétnímu souboru po určité časové období. Každý, kdo má tuto adresu url můžete přístup k souboru přímo z libovolného webového prohlížeče, ale je platný. Klíče SAS snadno vygenerujete z uživatelského rozhraní, jako je Průzkumník úložišť.

* **OTÁZKY. Je možné zadat jen pro čtení nebo jen pro zápis oprávnění pro složky ve sdílené složce?**
    
    Takovou míru kontroly nad oprávněními nemáte, pokud sdílenou složku připojíte přes SMB. Můžete toho však dosáhnout vytvořením sdíleného přístupového podpisu (SAS) přes REST API nebo knihovny klienta.  

* **OTÁZKY. Jak lze povolit šifrování na straně serveru pro Azure File storage?**

    [Šifrování na straně serveru](storage-service-encryption.md) pro Azure File storage je všeobecně dostupná v všech oblastech a veřejných a národních cloudů. Pro používání Azure File storage můžete povolit SSE [portál Azure](https://portal.azure.com/),[API zprostředkovatele služby Microsoft Azure Storage prostředků](/rest/api/storagerp/storageaccounts), [prostředí Azure Powershell](https://msdn.microsoft.com/library/azure/mt607151.aspx) nebo [rozhraní příkazového řádku Azure](storage-azure-cli.md).
    
    Po povolení SSE na Azure File storage, žádná nová data zapsána do úložiště file v daném účtu úložiště automaticky zašifruje. Tato funkce je dostupná pro všechna nově zapsaná data do existujících nebo nových sdílených složek v existujícím nebo novém účtu úložiště. Povolení této funkce je bez dalších poplatků. Další informace na [povolení SSE v Azure File storage](storage-service-encryption.md).

* **OTÁZKY. Ověřování na základě služby Active Directory podporuje Azure File storage?**
   
    Aktuálně nepodporujeme ověření založené na AD nebo ACL, je to však na našem seznamu požadovaných funkcí. Prozatím se k ověření ke sdílené složce používají klíče účtů Azure Storage. Nabízíme alternativní řešení, které využívá sdílené přístupové podpisy (SAS) přes REST API nebo knihovny klienta. Pomocí SAS můžete vygenerovat tokeny se specifickými oprávněními, které jsou platné pro zadaný časový interval. Například můžete vygenerovat token s přístupem jen pro čtení pro daný soubor s vypršení platnosti 10 minut. Každý, kdo takový token má po dobu jeho platnosti má přístup jen pro čtení k souboru pro těchto 10 minut.
   
    SAS se podporuje jen přes REST API nebo knihovny klienta. Když připojíte sdílenou složku přes protokol SMB, nemůžete použít SAS pro delegování přístupu k jejímu obsahu. 
    
* **OTÁZKY. Co jsou zásady dodržování předpisů dat podporována pro Azure File storage?**

   Úložiště Azure File je spuštěné nad architektuře úložiště jako ostatní služby úložiště ve službě Azure Storage a platí stejné zásady dodržování předpisů data. Další informace o dodržování předpisů dat úložiště Azure, můžete stáhnout a odkazovat na [dokumentu Microsoft Azure Data Protection](http://go.microsoft.com/fwlink/?LinkID=398382&clcid=0x409).

## <a name="on-premises-access"></a>Místní přístup

* **Q.Do je nutné použít pro připojení k Azure File storage z virtuálního počítače místní Azure ExpressRoute?**
   
    Ne. Pokud nemáte ExpressRoute, můžete ke sdílené složce přistupovat z místního prostředí za předpokladu, že máte port 445 (odchozí TCP) otevřený pro přístup k internetu. Však můžete ExpressRoute s úložištěm Azure File Pokud chcete.

* **OTÁZKY. Jak můžete připojit Azure sdílené složky na místním počítači?** 
    
    Dokud je otevřený port 445 (odchozí TCP) a váš klient podporuje protokol SMB 3.0 můžete připojit sdílenou složku přes protokol SMB (například používáte Windows 10 nebo Windows Server 2012). Pokud chcete tento port odblokovat, obraťte se na svého poskytovatele internetových služeb. Do té doby můžete zobrazit vaše soubory pomocí [Storage Explorer](../vs-azure-tools-storage-explorer-files.md#view-a-file-shares-contents).


## <a name="billing-and-pricing"></a>Fakturaci a cenách
* **OTÁZKY. Sítě provoz mezi virtuálním počítači Azure a sdílenou složkou jako externí šířku pásma, které jsou zpoplatněné nad rámec předplatného?**
   
    Pokud sdílená složka a virtuálních počítačů jsou ve stejné oblasti Azure, provoz mezi nimi je zdarma. Pokud jsou v různých oblastech, provoz mezi nimi bude účtován jako externí šířky pásma.

## <a name="backup"></a>Zálohování

* **OTÁZKY. Jak mohu zálohovat Moje sdílené složky Azure?**
    
    Můžete použít AzCopy, RoboCopy, nebo 3. stran zálohování nástroj, který může zálohovat připojené sdílené složky. Očekáváme, že má schopnost pořizovat snímky sdílených složek v blízké budoucnosti; bude moci pomocí této funkce lze zálohovat Azure sdílené složky.

## <a name="performance"></a>Výkon

* **OTÁZKY. Jaké jsou limity škálování pro Azure File storage?**
    Informace o škálovatelnosti a cílech výkonnosti pro Azure File storage najdete v tématu [a cíle výkonnosti služby Azure Storage Scalability](storage-scalability-targets.md#scalability-targets-for-blobs-queues-tables-and-files).

* **OTÁZKY. Moje šlo to velmi pomalu při pokusu o rozbalit soubory do úložiště Azure File. Co bych měl/a dělat?**
    
    Do úložiště Azure File přenášet větší počet souborů, doporučujeme použít AzCopy (Windows, verze Preview pro Linux a Unix) nebo Azure Powershell jako tyto nástroje jsou optimalizované pro síťové přenosy.

* **OTÁZKY. Opravy, co bylo vydáno opravit problém pomalý výkon s Azure File storage?**
    
    Tým Windows nedávno vydal opravu problému s nízkou rychlostí, když zákazník přistupuje k Azure File storage z Windows 8.1 nebo Windows Server 2012 R2. Další informace najdete v článku znalostní báze [pomalý výkon při přístupu k Azure File storage z Windows 8.1 nebo Server 2012 R2](https://support.microsoft.com/kb/3114025).

## <a name="features-and-interoperability-with-other-services"></a>Funkce a vzájemná funkční spolupráce s jinými službami
* **OTÁZKY. Je "určující sdílená složka" pro cluster s podporou převzetí služeb při selhání jedním z případů použití pro Azure File storage?**
   
    To není aktuálně podporováno.

* **OTÁZKY. Je k dispozici v rozhraní REST API operaci přejmenovat?**
   
    V tuto chvíli to není možné.

* **OTÁZKY. Může mít člověk vnořené sdílené složky, jinými slovy sdílenou složku ve sdílené složce?**
    
    Ne. Sdílená složka je virtuální jednotka, kterou můžete připojit, takže vnořené sdílené složky se nepodporují.

* **OTÁZKY. Používání Azure File storage s IBM MQ**
    
    Společnost IBM vydala dokument zákazníky IBM MQ Provede při konfiguraci Azure File storage pro jejich službu. Další informace najdete v tématu [Nastavení Manažera fronty více instancí IBM MQ se službou Microsoft Azure File](https://github.com/ibm-messaging/mq-azure/wiki/How-to-setup-IBM-MQ-Multi-instance-queue-manager-with-Microsoft-Azure-File-Service).


## <a name="troubleshooting"></a>Řešení potíží
* **OTÁZKY. Jak odstranit chyby Azure File storage?**
    
    Můžete se podívat do [Azure File storage článku Poradce při potížích s](storage-troubleshoot-file-connection-problems.md) pro pokyny k odstraňování problémů začátku do konce. 


## <a name="see-also"></a>Viz také
Další informace o úložišti Azure File jsou dostupné na těchto odkazech.

### <a name="conceptual-articles-and-videos"></a>Koncepční články a videa
* [Azure File Storage: hladký cloudový souborový systém SMB pro Windows a Linux](https://azure.microsoft.com/documentation/videos/azurecon-2015-azure-files-storage-a-frictionless-cloud-smb-file-system-for-windows-and-linux/)
* [Jak používat Azure File Storage s Linuxem](storage-how-to-use-files-linux.md)

### <a name="tooling-support-for-file-storage"></a>Podpora nástrojů pro úložiště File
* [Použití Azure Powershell s Azure Storage](storage-powershell-guide-full.md)
* [Použití nástroje AzCopy s Microsoft Azure Storage](storage-use-azcopy.md)
* [Použití Azure CLI s Azure Storage](storage-azure-cli.md)
* [Řešení potíží se službou Azure File Storage](storage-troubleshoot-file-connection-problems.md)

### <a name="blog-posts"></a>Příspěvky na blozích
* [Úložiště Azure File je nyní dostupné pro veřejnost](https://azure.microsoft.com/blog/azure-file-storage-now-generally-available/)
* [Uvnitř Azure File Storage](https://azure.microsoft.com/blog/inside-azure-file-storage/)
* [Představujeme službu Microsoft Azure File](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx)
* [Migrace dat do úložiště Azure File](https://azure.microsoft.com/blog/migrating-data-to-microsoft-azure-files/)

### <a name="reference"></a>Referenční informace
* [Klientská knihovna Storage pro .NET – referenční informace](https://msdn.microsoft.com/library/azure/dn261237.aspx)
* [REST API služby File – referenční informace](http://msdn.microsoft.com/library/azure/dn167006.aspx)
