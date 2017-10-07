---
title: "aaaAzure šifrování služby úložiště pro Data v klidovém stavu | Microsoft Docs"
description: "Použijte hello šifrování služby úložiště Azure funkce tooencrypt službě Azure Blob Storage na straně služby hello při ukládání dat hello a při získávání dat hello ho dešifrovat."
services: storage
documentationcenter: .net
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: edabe3ee-688b-41e0-b34f-613ac9c3fdfd
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/09/2017
ms.author: robinsh
ms.openlocfilehash: 304f1c21200f86f2084ce98788b2fc7ca893d1f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-storage-service-encryption-for-data-at-rest"></a>Šifrování služby Azure Storage pro neaktivní uložená data
Azure Storage Service šifrování (SSE) pro Data v klidovém stavu pomáhá chránit a chrání vaše data toomeet zabezpečení organizace a dodržování předpisů závazky. Pomocí této funkce Azure Storage automaticky toostorage předchozí toopersisting vaše data šifruje a dešifruje předchozí tooretrieval. Hello šifrování, dešifrování a správu klíčů jsou zcela transparentní toousers.

Hello následující témata poskytují podrobné pokyny o tom, jak funkce šifrování služby úložiště hello toouse, jakož i hello Podporované scénáře a koncových uživatelů.

## <a name="overview"></a>Přehled
Úložiště Azure poskytuje komplexní sadu funkcí zabezpečení, které společně umožňují vývojářům toobuild zabezpečených aplikací. Data byla zabezpečená během přenosu mezi aplikací a Azure pomocí [šifrování na straně klienta](storage-client-side-encryption.md), HTTPs nebo SMB 3.0. Šifrování služby úložiště poskytuje šifrování v klidovém stavu, zpracování šifrování, dešifrování a správu klíčů způsobem zcela transparentní. Všechna data se šifrují pomocí 256 bitů [šifrování AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), jednu z nejsilnějších bloku hello šifer k dispozici.

SSE funguje tak, že šifrování dat hello, pokud je napsána tooAzure úložiště a lze použít pro Azure Blob Storage a úložiště File. Funguje pro hello následující:

* Standardní úložiště: Účtů úložiště pro obecné účely pro objekty BLOB a souborů, úložiště a účty úložiště Blob
* Storage úrovně Premium 
* Všechny redundance úrovně (LRS, ZRS, GRS a RA-GRS)
* Účty úložiště Azure Resource Manager (ale ne classic) 
* Všechny oblasti.

toolearn více, naleznete toohello – nejčastější dotazy.

tooenable nebo můžete zakázat šifrování služby úložiště pro účet úložiště, přihlaste se k hello [portál Azure](https://portal.azure.com) a vyberte účet úložiště. V okně Nastavení hello vyhledejte hello části služby objektů Blob, jak je vidět na tomto snímku obrazovky a klikněte na šifrování.

![Možnost šifrování portálu snímek obrazovky zobrazující](./media/storage-service-encryption/image1.png)
<br/>*Obrázek 1: Povolení SSE pro služby objektů Blob (krok 1)*

![Možnost šifrování portálu snímek obrazovky zobrazující](./media/storage-service-encryption/image3.png)
<br/>*Obrázek 2: Povolení SSE pro služby souborů (krok 1)*

Po kliknutí na tlačítko Nastavení šifrování hello, můžete povolit nebo zakázat šifrování služby úložiště.

![Vlastnosti šifrování portálu snímek obrazovky zobrazující](./media/storage-service-encryption/image2.png)
<br/>*Obrázek 3: Povolení SSE pro objekt Blob a souboru služby (krok 2)*

## <a name="encryption-scenarios"></a>Šifrování scénáře
Šifrování služby úložiště se dá nastavit na úrovni účtu úložiště. Jakmile bude povoleno, vybere zákazníkům které tooencrypt služby. Podporuje následující scénáře zákazníka hello:

* Šifrování úložiště objektů Blob a File Storage v účtech Resource Manager.
* Šifrování objektů Blob a služba souborů v klasické účty úložiště jednou migrovali účty úložiště tooResource správce.

SSE má hello následující omezení:

* Šifrování klasické účty úložiště není podporováno.
* Stávající Data - SSE šifruje nově vytvořeného datového pouze po hello šifrování je povoleno. Pokud například vytvoříte nový účet úložiště Resource Manager, ale nikdy nezapínat na šifrování a pak nahrajte objektů BLOB nebo archivovaný účet úložiště toothat virtuální pevné disky a potom zapněte SSE, nebude možné tyto objekty BLOB zašifrované, pokud jsou přepsaná nebo zkopírovat.
* Podpora Marketplace - povolit šifrování virtuálních počítačů vytvořena z hello Marketplace pomocí hello [portál Azure](https://portal.azure.com), prostředí PowerShell a rozhraní příkazového řádku Azure. základní image virtuálního pevného disku Hello zůstat nezašifrovaný; ale bude se šifrovat všech zápisu provést poté, co má spuštěné hello virtuálních počítačů.
* Tabulky a fronty data nebudou šifrována.

## <a name="getting-started"></a>Začínáme
### <a name="step-1-create-a-new-storage-accountstorage-create-storage-accountmd"></a>Krok 1: [vytvořit nový účet úložiště](storage-create-storage-account.md).
### <a name="step-2-enable-encryption"></a>Krok 2: Povolit šifrování.
Můžete povolit šifrování pomocí hello [portál Azure](https://portal.azure.com).

> [!NOTE]
> Chcete-li tooprogrammatically povolit nebo zakázat hello šifrování služby úložiště na účet úložiště, můžete použít hello [REST API služby Azure Storage prostředků zprostředkovatele](https://msdn.microsoft.com/library/azure/mt163683.aspx), hello [knihovny klienta poskytovatele prostředků úložiště pro platformu .NET](https://msdn.microsoft.com/library/azure/mt131037.aspx), [prostředí Azure PowerShell](/powershell/azureps-cmdlets-docs), nebo hello [rozhraní příkazového řádku Azure](storage-azure-cli.md).
> 
> 

### <a name="step-3-copy-data-toostorage-account"></a>Krok 3: Kopírování dat toostorage účtu
Pokud povolíte SSE hello služby objektů Blob, bude se šifrovat všechny objekty BLOB zapsána toothat účet úložiště. Všechny objekty BLOB již nachází v daném účtu úložiště nebudou šifrována, dokud budou přepsána. Můžete zkopírovat hello data z jednoho úložiště účet tooone s SSE zašifrovaná, nebo i povolit SSE a zkopírujte hello objekty BLOB z jednoho kontejneru typu tooanother toosure, že předchozí data se šifrují. Můžete použít některý z následujících nástrojů tooaccomplish hello to. To je hello stejné chování pro úložiště File také.

#### <a name="using-azcopy"></a>Pomocí AzCopy
AzCopy je nástroj příkazového řádku systému Windows určené pro kopírování tooand dat z úložiště objektů Blob v Microsoft Azure, souboru a tabulky pomocí jednoduchých příkazů optimální výkon. Můžete použít tento toocopy objektů BLOB nebo soubory z jednoho tooanother účet úložiště, ten, který má SSE povoleno. 

toolearn víc, navštivte [přenos dat pomocí příkazového řádku Azcopy hello](storage-use-azcopy.md).

#### <a name="using-smb"></a>Pomocí protokolu SMB
Azure File storage nabízí sdílené složky v cloudu hello pomocí standardního protokol SMB hello. Můžete připojit sdílenou složku z klienta na místní nebo v Azure. Jakmile připojen, nástrojů, například Robocopy lze použít toocopy soubory přes tooAzure, které sdílené složky. Další informace najdete v tématu [jak toomount Azure sdílení souborů v systému Windows](storage-file-how-to-use-files-windows.md) a [jak toomount Azure File sdílet v systému Linux](storage-how-to-use-files-linux.md).


#### <a name="using-hello-storage-client-libraries"></a>Použití knihovny klienta úložiště hello
Tooand data objektů blob nebo soubor můžete zkopírovat z úložiště objektů blob nebo mezi účty úložiště pomocí našich širokou škálu knihovny klienta úložiště včetně .NET, C++, Java, Android, Node.js, PHP, Python nebo Ruby.

toolearn víc, navštivte naše [Začínáme s Azure Blob storage pomocí rozhraní .NET](storage-dotnet-how-to-use-blobs.md).

#### <a name="using-a-storage-explorer"></a>Pomocí Průzkumníka úložiště
Můžete použít účty úložiště Storage explorer toocreate, odeslání a stahování dat, zobrazení obsahu objektů BLOB a procházení adresářů. Můžete použít jednu z těchto účtů úložiště tooyour objekty BLOB tooupload s povolit šifrování. Některé průzkumníci úložiště taky zkopírovat data z existující kontejner úložiště objektů blob tooa různých v účtu úložiště hello nebo nový účet úložiště, který má SSE povolena.

toolearn víc, navštivte [Průzkumníci úložiště Azure](storage-explorers.md).

### <a name="step-4-query-hello-status-of-hello-encrypted-data"></a>Krok 4: Stav hello dotazu hello šifrovaná data
Byly nasazeny aktualizovanou verzi knihovny klienta úložiště hello, která umožňuje tooquery hello stav toodetermine k objektu v případě jsou šifrovaná, nebo ne. Toto je aktuálně dostupné jen pro úložiště objektů Blob. Podpora pro úložiště souborů je na plán hello. 

V hello té doby můžete volat [získat vlastnosti účtu](https://msdn.microsoft.com/library/azure/mt163553.aspx) tooverify, který hello účtu úložiště má povolit šifrování nebo zobrazení hello vlastnosti účtu úložiště v hello portálu Azure.

## <a name="encryption-and-decryption-workflow"></a>Šifrování a dešifrování pracovního postupu
Zde je stručný popis pracovního postupu šifrování a dešifrování hello:

* Zákazník Hello povoluje šifrování na účet úložiště hello.
* Když zákazník hello zapíše nové tooBlob data (PUT objektů Blob, PUT bloku, PUT stránky, UMÍSTIT soubor atd.) nebo soubor úložiště; každém zápisu jsou šifrována pomocí 256 bitů [šifrování AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), jednu z nejsilnějších bloku hello šifer k dispozici.
* Když zákazník hello potřebuje tooaccess dat (získání objektu Blob atd.), data jsou automaticky dešifrovaný před vrácením toohello uživatele.
* Pokud šifrování je zakázáno, nové zápisy již nejsou zašifrované a existující šifrovaná data zůstává zašifrovaný, dokud přepsaná uživatelem hello. Když je povolené šifrování, zapíše tooBlob nebo úložiště souborů se budou šifrovat. Stav Hello dat s uživatelem hello při přepínání mezi povolení nebo zákaz šifrování pro účet úložiště hello nezmění.
* Všechny šifrovací klíče jsou uložené, zašifrovaná a spravován společností Microsoft.

## <a name="frequently-asked-questions-about-storage-service-encryption-for-data-at-rest"></a>Nejčastější dotazy o šifrování služby úložiště pro Data v klidovém stavu
**Otázka: je nutné stávající účet úložiště classic. Můžete povolit SSE na něm?**

Odpověď: Ne, SSE je podporována pouze na účty úložiště Resource Manager.

**Otázka: jak můžete šifrovat data ve svůj účet úložiště classic?**

Odpověď: můžete vytvořit nový účet úložiště Resource Manager a kopírování dat pomocí [AzCopy](storage-use-azcopy.md) z existujícího účtu úložiště classic tooyour nově vytvořený účet úložiště Resource Manager. 

Pokud provádíte migraci vaší tooa účet úložiště classic účet správce prostředků úložiště, tato operace je okamžitou, změní hello typ účtu, ale neovlivňuje stávající data. Žádná nová data zapsána zašifruje až po aktivaci šifrování. Další informace najdete v tématu [platformy podporované migrace z IaaS prostředky z tooResource klasického správce](https://azure.microsoft.com/blog/iaas-migration-classic-resource-manager/). Upozorňujeme, že to je podporována pouze pro služby objektů Blob a souboru.

**Otázka: je nutné stávající účet úložiště Resource Manager. Můžete povolit SSE na něm?**

Odpověď: Ano, ale jenom nově zapisovat data budou šifrovány. Není přejděte zpět a šifrování dat, který již byl uložen. Ještě nejsou podporované pro hello náhled souboru úložiště.

**Otázka: nastavit jako tooencrypt hello aktuální data v existující účet úložiště Resource Manager?**

Odpověď: můžete povolit SSE kdykoli v účtu úložiště, Resource Manager. Data, které byly již existuje však nebudou šifrována. tooencrypt stávající data, můžete zkopírovat tooanother název nebo jiný kontejner a potom odeberte verze hello bez šifrování.

**Otázka: používám storage úrovně Premium; můžete použít SSE?**

Odpověď: Ano SSE je podporována na úložiště úrovně Standard a Premium Storage.  Storage úrovně Premium není podporována pro hello služba souborů.

**Otázka: Pokud lze vytvořit nový účet úložiště a povolit SSE a pak vytvořit nový virtuální počítač pomocí tohoto účtu úložiště, znamená to, že virtuální počítač je zašifrovaná?**

Odpověď: Ano. Disky vytvořené, které používají nový účet úložiště hello budou zašifrovaná, tak dlouho, dokud se vytvářely po povolení SSE. Pokud byl virtuální počítač vytvořen pomocí Azure Marketplace, základní image virtuálního pevného disku hello hello zůstane nezašifrované; ale bude se šifrovat všech zápisu provést poté, co má spuštěné hello virtuálních počítačů.

**Otázka: je možné vytvořit nové účty úložiště s SSE povolit pomocí Azure PowerShell a rozhraní příkazového řádku Azure?**

Odpověď: Ano.

**Otázka: jak mnohem víc Azure Storage náklady Pokud je povoleno SSE?**

Odpověď: není bez dalších nákladů.

**Otázka: kdo spravuje hello šifrovacích klíčů?**

Odpověď: hello klíče spravuje Microsoft.

**Otázka: je možné použít vlastní šifrovací klíče?**

Odpověď: pracujeme na poskytnutí možnosti pro zákazníky toobring vlastní šifrovací klíče.

**Otázka: je možné odvolat přístup toohello šifrovací klíče?**

Odpověď: není v tuto chvíli; Hello klíče jsou plně spravované microsoftem.

**Otázka: je SSE ve výchozím nastavení povolené, po vytvoření nového účtu úložiště?**

Odpověď: SSE není povoleno ve výchozím nastavení; můžete použít hello Azure portálu tooenable ho. Můžete také prostřednictvím kódu programu povolit tuto funkci pomocí hello REST API poskytovatele prostředků úložiště.

**Otázka: jak se to liší od Azure Disk Encryption?**

Odpověď: Tato funkce je použité tooencrypt dat v úložišti objektů Blob v Azure. Hello Azure Disk Encryption je použité tooencrypt operačního systému a datové disky v virtuální počítače IaaS. Další podrobnosti, navštivte naše [Průvodce zabezpečením úložiště](storage-security-guide.md).

**Otázka: Co když povolit SSE a potom přejděte a povolit Azure Disk Encryption na discích hello?**

Odpověď: to bude fungovat bez problémů. Vaše data se budou šifrovat pomocí obou metod.

**Otázka: svůj účet úložiště je nastavený toobe geo redundantně replikovat. Pokud lze povolit SSE, budou mé redundantní kopie také zašifrovaná?**

Odpověď: Ano, všechny kopie hello účtu úložiště se šifrují, a podporují všechny možnosti redundance – místně redundantní úložiště (LRS), zóny-Redundant Storage (ZRS), Geo-Redundant Storage (GRS) a přístup pro čtení Geo-Redundant Storage (RA-GRS) –.

**Otázka: I nelze povolit šifrování na svůj účet úložiště.**

Odpověď: je účet správce prostředků úložiště? Klasické účty úložiště nejsou podporovány. 

**Otázka: je SSE povolen pouze v určitých oblastí?**

Odpověď: hello SSE je k dispozici ve všech oblastech pro úložiště objektů Blob. Zkontrolujte, zda text hello dostupnosti část úložiště souborů. 

**Otázka: jak I contact někdo po jakýchkoli problémů nebo slyšet tooprovide názor?**

A: Obraťte se prosím na [ ssediscussions@microsoft.com ](mailto:ssediscussions@microsoft.com) pro všechny problémy související s tooStorage šifrování služby.

## <a name="next-steps"></a>Další kroky
Úložiště Azure poskytuje komplexní sadu funkcí zabezpečení, které společně umožňují vývojářům toobuild zabezpečených aplikací. Další podrobnosti najdete v článku hello [Průvodce zabezpečením úložiště](storage-security-guide.md).

