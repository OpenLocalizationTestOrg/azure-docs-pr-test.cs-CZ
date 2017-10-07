---
title: "aaaDeciding při objektů BLOB Azure toouse, soubory Azure nebo Azure datových disků"
description: "Další informace o data toostore a přístup různé způsoby hello v Azure toohelp, že se že můžete rozhodnout, které toouse technologie."
services: storage
documentationcenter: 
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: robinsh
ms.openlocfilehash: cd43abde43daf33dd7c43aa2696a9c8d5cd19612
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deciding-when-toouse-azure-blobs-azure-files-or-azure-data-disks"></a>Při rozhodování o tom při objektů BLOB Azure toouse, soubory Azure nebo Azure datových disků

Microsoft Azure poskytuje několik funkcí ve službě Azure Storage k ukládání a přístup k datům v cloudu hello. Tento článek popisuje soubory Azure, objekty BLOB a datové disky a je navrženou toohelp, můžete si zvolit mezi těmito funkcemi.

## <a name="scenarios"></a>Scénáře

Hello následující tabulka porovnává soubory, objekty BLOB a datové disky a uvádí příklad scénáře vhodné pro každý.

| Funkce | Popis | Když toouse |
|--------------|-------------|-------------|
| **Soubory Azure** | Poskytuje rozhraní protokolu SMB, knihovny klienta a [rozhraní REST](/rest/api/storageservices/file-service-rest-api) umožňuje přístup odkudkoli toostored soubory. | Chcete-li příliš "navýšení a posunutí" cloudu toohello aplikaci, což už používá hello nativní rozhraní API tooshare data systému mezi nimi a dalších aplikací běžících v Azure.<br/><br/>Chcete toostore vývoj a ladicí nástroje, které je třeba toobe k němu přistupovat z mnoha virtuálních počítačů. |
| **Azure BLOB** | Poskytuje klientské knihovny a [rozhraní REST](/rest/api/storageservices/blob-service-rest-api) nestrukturovaných dat, který umožňuje příliš ukládat a přistupovat v masivním měřítku v objekty BLOB bloku. | Chcete streamování toosupport aplikace a scénáře náhodný přístup.<br/><br/>Chcete data aplikací mít tooaccess toobe odkudkoli. |
| **Azure datových disků** | Poskytuje klientské knihovny a [rozhraní REST](/rest/api/compute/virtualmachines/virtualmachines-create-or-update) data toobe trvale uložené a k němu přistupovat z připojený virtuální pevný disk, který umožňuje. | Chcete toolift a posunutí aplikace používající tooread rozhraní API systému nativní souboru a zápisu dat toopersistent disky.<br/><br/>Chcete, aby byl toostore data, která nejsou požadované toobe k němu přistupovat z disku hello toowhich mimo hello virtuálního počítače je připojen. |

## <a name="comparison-files-and-blobs"></a>Porovnání: Soubory a objekty BLOB

Hello následující tabulka porovnává soubory Azure s Azure BLOB.  
  
||||  
|-|-|-|  
|**Atribut**|**Azure BLOB**|**Soubory Azure**|  
|Možnosti odolnost|LRS, ZRS (GRS a RA-GRS vyšší dostupnost)|LRS, GRS|  
|Usnadnění přístupu|Rozhraní REST API|Rozhraní REST API<br /><br /> SMB 2.1 a SMB 3.0 (systém souborů standardní rozhraní API)|  
|Připojení|Rozhraní REST API – po celém světě|Rozhraní REST API – po celém světě<br /><br /> Protokol SMB 2.1--v rámci oblasti<br /><br /> Protokol SMB 3.0 – po celém světě|  
|Koncové body|`http://myaccount.blob.core.windows.net/mycontainer/myblob`|`\\myaccount.file.core.windows.net\myshare\myfile.txt`<br /><br /> `http://myaccount.file.core.windows.net/myshare/myfile.txt`|  
|Adresáře|Plochý obor názvů|Hodnota TRUE, directory objekty|  
|Rozlišování názvů|Malá a velká písmena|Případ malých a velkých písmen, ale případu zachování|  
|Kapacita|Až too500 TB kontejnery|5 TB sdílené složky|  
|Propustnost|Až too60 MB za sekundu na jeden objekt blob bloku|Až too60 MB/s za sdílené složky|  
|Velikost objektu|Až objekt blob too200 GB nebo bloku|Too1TB nebo soubor|  
|Fakturovaná kapacity|Podle zapsaných bajtů|Podle velikosti souboru|  
|Klientské knihovny|Více jazyků|Více jazyků|  
  
## <a name="comparison-files-and-data-disks"></a>Porovnání: Soubory a datové disky

Soubory Azure doplňují Azure datových disků. Datový disk může být pouze připojené tooone virtuálního počítače Azure v čase. Datové disky jsou pevného formátu virtuální pevné disky uložené jako objekty BLOB stránky ve službě Azure Storage a jsou používány trvalá data toostore hello virtuálního počítače. Sdílené složky v Azure soubory jsou přístupné v hello stejný způsobem, jak je místní disk hello přistupuje (pomocí systému souborů nativní rozhraní API) a můžete sdílet mezi více virtuálních počítačů.  
 
Hello následující tabulka porovnává soubory Azure s Azure datových disků.  
 
||||  
|-|-|-|  
|**Atribut**|**Azure datových disků**|**Soubory Azure**|  
|Rozsah|Výhradní tooa jeden virtuální počítač|Sdílený přístup napříč více virtuálních počítačů|  
|Snímků a kopírování|Ano|Ne|  
|Konfigurace|Připojení při spuštění virtuálního počítače hello|Připojení po spuštění virtuálního počítače hello|  
|Authentication|Integrované|Nastavit pomocí příkazu net use|  
|Čištění|Automatické|Ruční|  
|Přístup pomocí REST|Nelze získat přístup k souborů v rámci hello virtuálního pevného disku|Soubory uložené ve sdílené složce je přístupná.|  
|Maximální velikost|Disk 1 TB|5 TB sdílení souborů a 1 TB soubor ve složce|  
|Maximální IOps 8KB|500 IOps|1000 IOps|  
|Propustnost|Až too60 MB/s na Disk|Až too60 MB/s na základě sdílené složky|  

## <a name="next-steps"></a>Další kroky

Při rozhodování o tom, jak je vaše data uložené a získat přístup, měli byste také zvážit hello náklady související se situací. Další informace najdete v tématu [Azure Storage – ceny](https://azure.microsoft.com/pricing/details/storage/).
  
Některé funkce SMB nejsou použitelné toohello cloudu. Další informace najdete v tématu [funkce nepodporované hello služby Azure File](/rest/api/storageservices/features-not-supported-by-the-azure-file-service).
  
Další informace o datových disků najdete v tématu [Správa disků a bitové kopie](../../virtual-machines/windows/about-disks-and-vhds.md) a [jak tooAttach tooa datový Disk virtuálního počítače Windows](../../virtual-machines/windows/classic/attach-disk.md).
