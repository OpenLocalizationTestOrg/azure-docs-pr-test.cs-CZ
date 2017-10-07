---
title: "aaaAzure šifrování disku pro systém Windows a virtuálních počítačů IaaS Linux | Microsoft Docs"
description: "Tento článek obsahuje přehled Microsoft Azure Disk Encryption pro systém Windows a virtuálních počítačů IaaS Linux."
services: security
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: TomSh
ms.assetid: d3fac8bb-4829-405e-8701-fa7229fb1725
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/07/2017
ms.author: kakhan
ms.openlocfilehash: b685abdcc908e66d2352ec5ac2d9996aa75af1b6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-disk-encryption-for-windows-and-linux-iaas-vms"></a>Azure Disk Encryption pro systém Windows a virtuálních počítačů Linux IaaS
Microsoft Azure je důrazně potvrdit tooensuring soukromí dat, data suverenity a umožní vám toocontrol vaše Azure hostované data prostřednictvím řadu tooencrypt pokročilé technologie, řídit a spravovat šifrovací klíče, řízení a audit přístupu k datům. To poskytuje Azure zákazníkům hello flexibilitu toochoose hello řešení, které nejlépe vyhovuje potřebám své firmy. V tomto dokumentu jsme vás seznámí tooa nové technologie řešení "Azure Disk Encryption pro systém Windows a Linux IaaS Virtuálního počítače" toohelp chránit a chrání vaše data toomeet organizační závazky zabezpečení a dodržování předpisů. Hello dokument obsahuje podrobné pokyny k jak toouse hello Azure disk encryption funkce včetně hello Podporované scénáře a hello uživatelského prostředí.

> [!NOTE]
> Některá doporučení může zvýšit data, síťové nebo využití výpočetních prostředků, což vede k další náklady na licence nebo předplatné.

## <a name="overview"></a>Přehled
Azure Disk Encryption je novou funkci, která umožňuje šifrovat disky virtuálního počítače s Windows a Linux IaaS. Azure Disk Encryption využívá standardní hello [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) funkce systému Windows a hello [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) funkce Linux tooprovide svazku šifrování pro hello operačního systému a datové disky hello. řešení Hello je integrovaná s [Azure Key Vault](https://azure.microsoft.com/documentation/services/key-vault/) toohelp řídit a spravovat hello šifrování disku klíče a tajné klíče v rámci vašeho předplatného trezoru klíčů. Hello řešení také zajistí, že jsou všechna data na disky virtuálního počítače hello zašifrovaná přinejmenším ve službě Azure storage.

Azure disk encryption pro systém Windows a virtuálních počítačů IaaS Linux je nyní v **obecné dostupnosti** do všech veřejných oblastí Azure a oblasti AzureGov standardní virtuální počítače a virtuální počítače s storage úrovně premium.

### <a name="encryption-scenarios"></a>Šifrování scénáře
Hello řešení Azure Disk Encryption podporuje následující scénáře zákazníka hello:

* Povolit šifrování na nové virtuální počítače IaaS vytvořené z předem šifrované virtuálního pevného disku a šifrovacích klíčů
* Povolit šifrování na nové virtuální počítače IaaS vytvořené z hello podporováno obrázky z Galerie Azure
* Povolit šifrování na existující virtuální počítače IaaS běžící v Azure
* Vypnout šifrování u virtuálních počítačů IaaS Windows
* Zakázat šifrování na datových jednotkách pro virtuální počítače IaaS Linux
* Povolit šifrování spravovaných disků na virtuální počítače
* Aktualizovat nastavení šifrování existující úložiště šifrované neprémiové virtuálních počítačů
* Zálohování a obnovení šifrovaných virtuálních počítačů, šifrovaný klíč šifrovacího klíče

řešení Hello podporuje následující scénáře pro virtuální počítače IaaS, pokud jsou povolené v Microsoft Azure hello:

* Integrace s Azure Key Vault
* Úroveň Standard virtuálních počítačů: [A, D, DS, G, GS, F a atd. řady virtuální počítače IaaS](https://azure.microsoft.com/pricing/details/virtual-machines/)
* Povolit šifrování na Windows a virtuálních počítačů IaaS Linux a spravovaných disků virtuálních počítačů z hello podporováno obrázky z Galerie Azure
* Zakázat šifrování na jednotkách operačního systému a dat pro virtuální počítače IaaS Windows a spravovaných disků na virtuální počítače
* Zakázat šifrování na datových jednotkách pro virtuální počítače IaaS Linux a spravovaných disků na virtuální počítače
* Povolit šifrování na virtuální počítače IaaS se systémem OS klienta Windows
* Povolit šifrování na svazcích s cestami připojení
* Povolit šifrování na virtuální počítače s Linuxem nakonfigurovaný s diskem proložení (RAID) pomocí mdadm
* Povolit šifrování na virtuální počítače s Linuxem pomocí LVM pro datové disky
* Povolit šifrování na virtuálních počítačích Windows nakonfigurovaný s prostory úložiště
* Aktualizovat nastavení šifrování existující úložiště šifrované neprémiové virtuálních počítačů
* Jsou podporovány všechny veřejné Azure a AzureGov oblastí

řešení Hello nepodporuje následující scénáře, funkce a technologie hello:

* Úroveň Basic virtuálních počítačů IaaS
* Zakázáním šifrování na jednotce operačního systému pro virtuální počítače IaaS Linux
* Zakázáním šifrování na datová jednotka, pokud je zašifrován hello jednotky operačního systému pro virtuální počítače Iaas Linux
* Virtuální počítače IaaS, vytvořené pomocí metody vytvoření virtuálního počítače classic hello
* Povolte šifrování na systém Windows a virtuálních počítačů Linux IaaS zákazníka vlastních bitových kopií není podporován. Povolit enccryption na operační systém Linux LVM disku aktuálně nepodporuje. Tato podpora bude brzy pocházet.
* Integrace s vaší místní služby správy klíčů
* Soubory Azure (systém souborů sdíleného), Network File System (NFS), dynamické svazky a virtuální počítače Windows, které jsou nakonfigurované s systémy na bázi softwaru diskového pole RAID
* Zálohování a obnovení šifrovaných virtuálních počítačů, šifrované bez klíče šifrovací klíč.
* Aktualizujte nastavení šifrování existující šifrované premium úložiště virtuálních počítačů.

> [!NOTE]
> Zálohování a obnovení šifrovaných virtuálních počítačů je podporována pouze pro virtuální počítače, které jsou zašifrované s konfigurací KEK hello. Není podporována u virtuálních počítačů, které jsou zašifrované bez KEK. KEK je volitelný parametr, který povoluje šifrování virtuálních počítačů. Tato podpora se připravuje.
> Aktualizujte nastavení šifrování virtuálního počítače nejsou podporovány existující úložiště šifrované premium Storage. Tato podpora se připravuje.

### <a name="encryption-features"></a>Funkce šifrování
Když povolíte a nasadíte Azure Disk Encryption pro virtuální počítače Azure IaaS, hello následující funkce jsou povolené, v závislosti na konfiguraci hello poskytuje:

* Šifrování hello OS svazku tooprotect hello spouštěcí svazek v klidovém stavu uložených v úložišti
* Šifrování dat svazky tooprotect hello datové svazky v klidovém stavu uložených v úložišti
* Zakázáním šifrování na hello operačního systému a datové disky pro virtuální počítače IaaS Windows
* Zakázáním šifrování dat hello disky pro virtuální počítače IaaS Linux (pouze v případě operačního systému je není šifrované jednotky)
* Zabezpečení hello šifrovacích klíčů a tajných klíčů ve vašem předplatném trezoru klíčů
* Vytváření sestav stav šifrování hello hello šifrované virtuálních počítačů IaaS
* Odebrání nastavení konfigurace šifrování disku z virtuálního počítače IaaS hello
* Zálohování a obnovení šifrovaných virtuálních počítačů pomocí služby Azure Backup hello

> [!NOTE]
> Zálohování a obnovení šifrovaných virtuálních počítačů je podporována pouze pro virtuální počítače, které jsou zašifrované s konfigurací KEK hello. Není podporována u virtuálních počítačů, které jsou zašifrované bez KEK. KEK je volitelný parametr, který povoluje šifrování virtuálních počítačů.

Azure Disk Encryption pro virtuální počítače IaaS pro Windows a Linux řešení zahrnuje:

* Hello šifrování disku rozšíření pro Windows.
* Hello šifrování disku rozšíření pro Linux.
* rutiny prostředí PowerShell Hello šifrování disku.
* Hello šifrování disku rutiny rozhraní příkazového řádku Azure (CLI).
* šablony Azure Resource Manager Hello šifrování disku.

Hello řešení Azure Disk Encryption je podporována u virtuálních počítačů IaaS, který se systémem Windows nebo operační systém Linux. Další informace o hello podporované operační systémy najdete v tématu požadavky"hello" oddílu.

> [!NOTE]
> Není k dispozici bez dalších poplatků pro šifrování disky virtuálních počítačů s Azure Disk Encryption.

### <a name="value-proposition"></a>Návrh hodnoty
Při použití řešení hello Azure Disk Encryption správy se může stát odpovědí hello následující obchodních potřeb:

* Virtuální počítače IaaS jsou zabezpečeny v klidu, protože můžete použít standardní technologie tooaddress organizační zabezpečení a dodržování předpisů požadavky na šifrování.
* Spouštění virtuálních počítačů IaaS v části klíče řídí zákazníka a zásady a auditovat jejich využití v trezoru klíčů.

### <a name="encryption-workflow"></a>Pracovní postup šifrování
šifrování disku tooenable pro systém Windows a virtuální počítače s Linuxem, hello následující:

1. Zvolte scénářem šifrování některé z těchto hello předcházející šifrování scénáře.
2. Vyjádřit výslovný souhlas tooenabling šifrování disku pomocí šablony Azure Disk Encryption Resource Manageru hello, rutiny prostředí PowerShell nebo rozhraní příkazového řádku příkaz a zadejte konfiguraci šifrování hello.

   * Hello šifrované zákazníka virtuálního pevného disku scénáři nahrajte účet úložiště tooyour hello šifrované virtuálního pevného disku a hello šifrovací klíče podstatným tooyour trezoru klíčů. Potom zadejte hello šifrování konfigurace tooenable na nový virtuální počítač IaaS.
   * Nové virtuální počítače, které jsou vytvořené pomocí hello Marketplace a stávající virtuální počítače, které jsou již spuštěny v Azure zadejte hello šifrování konfigurace tooenable na hello virtuálních počítačů IaaS.

3. Udělení přístupu toohello platformy Azure tooread hello šifrovací klíč materiálu (šifrovací klíče nástroje BitLocker pro systémy Windows) a heslo pro Linux z trezoru klíčů tooenable šifrování na hello virtuálních počítačů IaaS.

4. Zadejte hello Azure Active Directory (Azure AD) aplikace identity toowrite hello šifrovací klíče podstatným tooyour trezoru klíčů. Díky tomu povoluje šifrování na hello virtuálních počítačů IaaS pro scénáře hello uveden v kroku 2.

5. Azure aktualizuje hello modelu služby virtuálních počítačů pomocí šifrování a konfigurace hello trezoru klíčů a nastaví šifrované virtuálního počítače.

 ![Microsoft Antimalware v Azure](./media/azure-security-disk-encryption/disk-encryption-fig1.png)

### <a name="decryption-workflow"></a>Dešifrování pracovního postupu
toodisable šifrování disku pro virtuální počítače IaaS, dokončení hello následující postup vysoké úrovně:

1. Zvolte toodisable šifrování (dešifrování) na spuštění virtuálního počítače IaaS v Azure pomocí šablony Azure Disk Encryption Resource Manageru hello nebo rutiny prostředí PowerShell a zadejte konfiguraci dešifrování hello.

 Tento krok zakazuje šifrování hello operačního systému nebo hello datový svazek nebo obojí na spuštění virtuálních počítačů IaaS Windows hello. Nicméně jak je uvedeno v předchozí části hello, zakázáním šifrování disku operačního systému Linux nepodporuje. Krok dešifrování Hello je povoleno pouze pro datové jednotky v virtuální počítače s Linuxem, dokud není šifrován hello disk operačního systému.
2. Azure aktualizace hello modelu služby virtuálních počítačů a virtuálních počítačů IaaS hello je označen jako dešifrovaný. obsah Hello hello virtuálního počítače jsou již v zašifrované podobě.

> [!NOTE]
> operace disable šifrování Hello nedojde k odstranění vašeho klíče trezoru a hello šifrování materiál klíče (šifrovací klíče nástroje BitLocker pro systémy Windows) nebo přístupové heslo pro Linux.
 > Zakázáním šifrování disku operačního systému Linux není podporována. Krok dešifrování Hello je povoleno pouze pro datové jednotky v virtuální počítače s Linuxem.
Zakázání disku šifrování dat pro Linux není podporována, pokud je zašifrován hello jednotky operačního systému.

## <a name="prerequisites"></a>Požadavky
Než povolíte Azure Disk Encryption na virtuálních počítačích Azure IaaS pro hello Podporované scénáře, které bylo popsané v části "Přehled" hello, najdete v části hello následující požadavky:

* V Azure v oblastech hello podporované musí mít prostředky toocreate platný aktivní předplatné Azure.
* Azure Disk Encryption je podporována v následujících verzích Windows Server hello: Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 a Windows Server 2016.
* Azure Disk Encryption je podporována v následujících klientských verzí Windows hello: klienta systému Windows 8 a Windows 10 klienta.

> [!NOTE]
> Pro Windows Server 2008 R2 musíte mít rozhraní .NET Framework 4.5 nainstalované dříve než povolíte šifrování v Azure. Můžete ji nainstalovat z webu Windows Update nainstalováním hello volitelnou aktualizaci rozhraní Microsoft .NET Framework 4.5.2 na x64 systémů Windows Server 2008 R2 ([KB2901983](https://support.microsoft.com/kb/2901983)).

* Azure Disk Encryption se podporuje na hello následující Azure Gallery založena na Linuxových server distribucích a verzích:

| Distribuce systému Linux | Verze | Typ svazku podporovaný pro šifrování|
| --- | --- |--- |
| Ubuntu | 16.04. DENNĚ LTS | Disk operačního systému a dat |
| Ubuntu | 14.04.5-DAILY-LTS | Disk operačního systému a dat |
| Ubuntu | 12.10 | Datový disk |
| Ubuntu | 12.04 | Datový disk |
| RHEL | 7.3 | Disk operačního systému a dat |
| RHEL | 7.2 | Disk operačního systému a dat |
| RHEL | 6.8 | Disk operačního systému a dat |
| RHEL | 6.7 | Datový disk |
| CentOS | 7.3 | Disk operačního systému a dat |
| CentOS | 7.2N | Disk operačního systému a dat |
| CentOS | 6.8 | Disk operačního systému a dat |
| CentOS | 7.1 | Datový disk |
| CentOS | 7.0 | Datový disk |
| CentOS | 6.7 | Datový disk |
| CentOS | 6.6 | Datový disk |
| CentOS | 6.5 | Datový disk |
| openSUSE | 13.2 | Datový disk |
| SLES | 12 SP1 | Datový disk |
| SLES | 12-SP1 (Premium) | Datový disk |
| SLES | HPC 12 | Datový disk |
| SLES | 11-SP4 (Premium) | Datový disk |
| SLES | 11 SP4 | Datový disk |

* Azure Disk Encryption vyžaduje, aby váš trezor klíčů a virtuální počítače jsou umístěny ve hello stejné oblasti Azure a předplatné.

> [!NOTE]
> Konfigurace prostředků hello v oblastech způsobí selhání při povolování funkce Azure Disk Encryption hello.

* tooset registrace a konfigurace trezoru klíčů pro Azure Disk Encryption, najdete v tématu **nastavit registrace a konfigurace trezoru klíčů pro Azure Disk Encryption** v hello *požadavky* tohoto článku.
* tooset až a nakonfigurovat aplikace Azure AD ve službě Azure Active directory pro Azure Disk Encryption, najdete v tématu **nastavení aplikace hello Azure AD ve službě Azure Active Directory** v hello *požadavky* části tohoto článku.
* tooset registrace a konfigurace zásad přístupu hello trezoru klíčů pro aplikaci hello Azure AD, najdete v tématu **nastavte hello trezoru klíčů zásady přístupu pro aplikace hello Azure AD** v hello *požadavky* část v tomto článku.
* tooprepare předem šifrované Windows virtuální pevný disk, najdete v tématu **připravit předem šifrované virtuálního pevného disku Windows** v hello *příloha*.
* tooprepare předem šifrované Linux virtuální pevný disk, najdete v tématu **připravit předem šifrované VHD Linux** v hello *příloha*.
* potřebuje přístup toohello šifrovacích klíčů nebo tajných klíčů v váš trezor klíčů toomake zprostředkovatele Hello platformy Azure je k dispozici toohello virtuální počítač, když se spustí a dešifruje svazek virtuálního počítače OS hello. toogrant oprávnění tooAzure platformy, sada hello **EnabledForDiskEncryption** vlastnost v trezoru klíčů hello. Další informace najdete v tématu **nastavit registrace a konfigurace trezoru klíčů pro Azure Disk Encryption** v hello příloha.
* Tajný klíč trezoru klíčů a adresy URL KEK musí být verzí. Azure vynucuje toto omezení Správa verzí. Platný tajný klíč a adresy URL KEK najdete v tématu hello následující příklady:

  * Příklad platnou adresu URL tajný: *https://contosovault.vault.azure.net/secrets/BitLockerEncryptionSecretWithKek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*
  * Příklad platnou adresu URL KEK: *https://contosovault.vault.azure.net/keys/diskencryptionkek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*

* Azure Disk Encryption nepodporuje zadání čísla portu jako součást tajné klíče trezoru klíčů a adresy URL KEK. Příklady adres URL nepodporovaný a podporované trezoru klíčů najdete v tématu hello následující:

  * Adresa URL nemůže být přijata trezoru klíčů *https://contosovault.vault.azure.net:443 nebo tajných klíčů nebo contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*
  * Adresa URL přijatelné trezoru klíčů: *https://contosovault.vault.azure.net/secrets/contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*

* Funkce Azure Disk Encryption hello tooenable, hello virtuální počítače IaaS musí splňovat následující požadavky na konfiguraci koncového bodu sítě hello:
  * tooget tokenu tooconnect tooyour trezoru klíčů, hello virtuálních počítačů IaaS musí být schopný tooconnect tooan Azure Active Directory koncový bod, \[login.microsoftonline.com\].
  * toowrite hello šifrovací klíče tooyour trezoru klíčů, hello virtuálních počítačů IaaS musí být schopný tooconnect toohello trezoru klíčů koncový bod.
  * Hello virtuálních počítačů IaaS musí být schopný tooconnect tooan úložiště Azure koncový bod, hostitelé hello úložiště rozšíření Azure a účet úložiště Azure, hostitelé hello soubory virtuálního pevného disku.

  > [!NOTE]
  > Pokud vaše zásady zabezpečení omezuje přístup z virtuálních počítačů Azure toohello Internetu, můžete vyřešit hello předcházející URI a nakonfigurovat konkrétní pravidlo tooallow odchozí připojení toohello IP adresy.
  >
  >tooconfigure a přístup k Azure Key Vault za bránou firewall (https://docs.microsoft.com/en-us/azure/key-vault/key-vault-access-behind-firewall)

* Použijte nejnovější verzi Azure PowerShell SDK verze tooconfigure Azure Disk Encryption hello. Stáhněte si nejnovější verzi hello [verzi prostředí Azure PowerShell](https://github.com/Azure/azure-powershell/releases)

 > [!NOTE]
  > Azure Disk Encryption není podporována na [Azure PowerShell SDK verze 1.1.0](https://github.com/Azure/azure-powershell/releases/tag/v1.1.0-January2016). Pokud se vám zobrazuje chybu související toousing prostředí Azure PowerShell 1.1.0, najdete v části [Azure Disk Encryption chyba související tooAzure prostředí PowerShell 1.1.0](http://blogs.msdn.com/b/azuresecurity/archive/2016/02/10/azure-disk-encryption-error-related-to-azure-powershell-1-1-0.aspx).

* toorun libovolnému příkazu příkazového řádku Azure CLI a přidružit ho ke svému předplatnému Azure, je nutné nejprve nainstalovat rozhraní příkazového řádku Azure:
  * tooinstall rozhraní příkazového řádku Azure a přidružit ho ke svému předplatnému Azure, najdete v části [jak tooinstall a konfigurace rozhraní příkazového řádku Azure](../cli-install-nodejs.md).
  * toouse Azure CLI pro Mac, Linux a Windows pomocí Azure Resource Manageru, najdete v části [rozhraní příkazového řádku Azure v režimu Resource Manager](../virtual-machines/azure-cli-arm-commands.md).

* Při šifrování se spravovaným diskem, je povinné požadovaných tootake snímek hello spravované disku nebo zálohy disku hello mimo Azure Disk Encryption předchozí tooenabling šifrování.  Bez předchozího provedení zálohy na místě jakákoli Neočekávaná chyba při šifrování může vykreslit hello disk a virtuální počítač nedostupný bez možnosti obnovení.  Set-AzureRmVMDiskEncryptionExtension není aktuálně zálohovat spravovaných disků a bude chyba, je-li použít u se spravovaným diskem, pokud byl zadán parametr - skipVmBackup hello.  Tento parametr je nezabezpečený toouse, není-li zálohu již byl změněn mimo Azure Disk Encryption.   Pokud je zadán parametr - skipVmBackup hello, nebude hello rutiny vytvořit záložní kopii předchozí tooencryption hello spravované disku.  Z tohoto důvodu bude považován za povinný požadovaných toomake se, že potřeby zálohu hello spravovaného disku virtuálního počítače je v místní předchozí tooenabling Azure Disk Encryption v případě, že je obnovení novější.  
> [!NOTE]
 > Parametr - skipVmBackup Hello by nikdy nepoužívali, pokud snímek nebo zálohování již byl změněn mimo Azure Disk Encryption. 

* Hello Azure Disk Encryption řešení používá pro virtuální počítače IaaS Windows hello externí ochrany pomocí klíče Bitlockeru. Pro doménu připojené k virtuální počítače, nesmí push žádné zásady skupiny, které vynutit ochrany pomocí čipu TPM. Informace o zásadách skupiny hello "Povolit BitLocker bez kompatibilním čipem TPM" najdete v tématu [referenční příručka zásad skupiny Bitlockeru](https://technet.microsoft.com/library/ee706521).
* Zásady nástroje BitLocker na virtuální počítače připojené k doméně pomocí zásad skupiny vlastní musí zahrnovat hello následující nastavení: `Configure user storage of bitlocker recovery information -> Allow 256-bit recovery key` Azure Disk Encryption se nezdaří, pokud nejsou kompatibilní, nastavení zásad vlastní skupiny pro Bitlocker. Na počítačích, které neměl hello nastavení správné zásady, použití hello nové zásady, vynucení hello nové zásady tooupdate (gpupdate.exe/Force) a restartujte může být vyžadováno.  
* toocreate aplikaci Azure AD vytvoření trezoru klíčů, nebo nastavit existující trezor klíčů a povolit šifrování, najdete v části hello [požadovaných skript prostředí PowerShell Azure Disk Encryption](https://github.com/Azure/azure-powershell/blob/master/src/ResourceManager/Compute/Commands.Compute/Extension/AzureDiskEncryption/Scripts/AzureDiskEncryptionPreRequisiteSetup.ps1).
* požadavky tooconfigure šifrování disku pomocí hello Azure CLI, najdete v části [tento skript Bash](https://github.com/ejarvi/ade-cli-getting-started).
* toouse hello Azure Backup service tooback zálohu a obnovení šifrované virtuálních počítačů, když je povolené šifrování s Azure Disk Encryption zašifrovat virtuální počítače pomocí klíče konfigurace Azure Disk Encryption hello. Hello služby Backup podporuje virtuální počítače, které jsou šifrované pomocí pouze KEK konfigurace. V tématu [jak šifrované tooback zálohu a obnovení virtuálních počítačů s šifrováním Azure Backup](https://docs.microsoft.com/en-us/azure/backup/backup-azure-vms-encryption).

* Při šifrování svazku operačního systému Linux, je momentálně nevyžaduje na konci hello procesu hello Všimněte si, že virtuální počítač restartovat. To lze provést prostřednictvím portálu hello, prostředí powershell nebo rozhraní příkazového řádku.   průběh hello tootrack šifrování, pravidelně dotazovat vrácený Get-AzureRmVMDiskEncryptionStatus https://docs.microsoft.com/en-us/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus hello stavovou zprávu.  Po dokončení šifrování bude tuto oznámí hello hello stavovou zprávu vrácený tento příkaz.  Například "ProgressMessage: disk operačního systému úspěšně šifrování, restartujte prosím hello virtuální počítač" na tento bod hello virtuálních počítačů může být restartován a použita.  

* Azure Disk Encryption pro Linux vyžaduje datové disky toohave systému připojeného souboru v předchozí tooencryption Linux

* Rekurzivní připojené datových disků nepodporuje hello Azure Disk Encryption pro Linux. Například pokud hello cílovém systému má připojené disku na /foo/bar a pak jiné na /foo/bar/baz hello šifrování /foo/bar/baz bude úspěšné, ale šifrování s/foo nebo se nezdaří. 

* Azure Disk Encryption je podporován pouze v galerii Azure, které jsou podporované Image, které splňují hello zmíněnými požadavky. Zákazník vlastními obrázky nejsou podporovány kvůli schématy oddílu toocustom a proces chování, které mohou existovat na těchto bitových kopií. Navíc založená i galerie image Virtuálního počítače, který původně splněny požadavky ale byly upraveny po vytvoření může být nekompatibilní.  Pro, důvod, proč se hello navrhované postup pro šifrování virtuálního počítače s Linuxem je toostart z Galerie vyčištění bitové kopie, šifrování hello virtuálního počítače a poté přidejte vlastní softwaru nebo data toohello virtuální počítač podle potřeby.  

> [!NOTE]
> Zálohování a obnovení šifrovaných virtuálních počítačů je podporována pouze pro virtuální počítače, které jsou zašifrované s konfigurací KEK hello. Není podporována u virtuálních počítačů, které jsou zašifrované bez KEK. KEK je volitelný parametr, který umožňuje virtuálních počítačů.

#### <a name="set-up-hello-azure-ad-application-in-azure-active-directory"></a>Nastavit hello aplikaci Azure AD ve službě Azure Active Directory
Pokud budete potřebovat toobe šifrování povolené na spuštění virtuálního počítače v Azure, Azure Disk Encryption generuje a zapíše hello šifrovací klíče tooyour klíče trezoru. Správa šifrovacích klíčů v trezoru klíčů se vyžaduje ověřování Azure AD.

Pro tento účel vytvořte aplikaci Azure AD. Najdete podrobné pokyny pro registraci aplikace v hello "Get Identity pro hello aplikace" v tématu hello příspěvku na blogu [Azure Key Vault - krok za krokem](http://blogs.technet.com/b/kv/archive/2015/06/02/azure-key-vault-step-by-step.aspx). Tento příspěvek také obsahuje řadu užitečné příklady pro nastavení a konfiguraci trezoru klíčů. Pro účely ověření můžete použít buď ověřování na základě tajný klíč klienta nebo ověřování klientů na základě certifikátů Azure AD.

#### <a name="client-secret-based-authentication-for-azure-ad"></a>Ověřování na základě tajný klíč klienta pro Azure AD
Hello oddíly, které následují, můžete nakonfigurovat ověřování na základě tajný klíč klienta pro Azure AD.

##### <a name="create-an-azure-ad-application-by-using-azure-powershell"></a>Vytvořit aplikaci Azure AD pomocí Azure PowerShell
Použijte hello následující toocreate rutiny prostředí PowerShell aplikaci Azure AD:

    $aadClientSecret = "yourSecret"
    $azureAdApplication = New-AzureRmADApplication -DisplayName "<Your Application Display Name>" -HomePage "<https://YourApplicationHomePage>" -IdentifierUris "<https://YouApplicationUri>" -Password $aadClientSecret
    $servicePrincipal = New-AzureRmADServicePrincipal –ApplicationId $azureAdApplication.ApplicationId

> [!NOTE]
> $azureAdApplication.ApplicationId hello Azure AD ClientID a $aadClientSecret je sdílený tajný klíč klienta hello by měl použít novější tooenable Azure Disk Encryption. Tajný klíč klienta Azure AD hello zabezpečit správně.

##### <a name="setting-up-hello-azure-ad-client-id-and-secret-from-hello-azure-classic-portal"></a>Nastavení hello Azure AD ID klienta a tajný klíč z portálu Azure classic hello
Můžete vytvořit také ID klienta Azure AD a tajný klíč pomocí hello [portál Azure classic]( https://manage.windowsazure.com). tooperform tato úloha, hello následující:

1. Klikněte na tlačítko hello **služby Active Directory** kartě.

 ![Azure Disk Encryption](./media/azure-security-disk-encryption/disk-encryption-fig3.png)

2. Klikněte na tlačítko **přidat aplikaci**a pak zadejte název aplikace hello.

 ![Azure Disk Encryption](./media/azure-security-disk-encryption/disk-encryption-fig4.png)

3. Klikněte na tlačítko se šipkou hello a pak nakonfigurujte vlastnosti aplikace hello.

 ![Azure Disk Encryption](./media/azure-security-disk-encryption/disk-encryption-fig5.png)

4. Klikněte na tlačítko zaškrtnutí hello v dolním rohu toofinish hello. Zobrazí se stránka konfigurace aplikace Hello a hello ID klienta Azure AD se zobrazí v dolní části hello hello stránky.

 ![Azure Disk Encryption](./media/azure-security-disk-encryption/disk-encryption-fig6.png)

5. Uložit tajný klíč klienta Azure AD hello kliknutím hello **Uložit** tlačítko. Poznámka: sdílený tajný klíč klienta Azure AD hello hello klíče textového pole. Zabezpečit správně.

 ![Azure Disk Encryption](./media/azure-security-disk-encryption/disk-encryption-fig7.png)

 > [!NOTE]
 > Hello předcházející toku nepodporuje hello portál Azure classic.

##### <a name="use-an-existing-application"></a>Použít stávající aplikaci
tooexecute hello následující příkazy, získání a používání hello [modulu Azure AD PowerShell](https://technet.microsoft.com/library/jj151815.aspx).

> [!NOTE]
> Následující příkazy Hello musí provést z nové okno prostředí PowerShell. Nepoužívejte Azure PowerShell nebo hello Azure Resource Manager okno tooexecute hello příkazů. Doporučujeme vám tento přístup, protože jsou tyto rutiny v modulu MSOnline hello nebo Azure AD PowerShell.

    $clientSecret = ‘<yourAadClientSecret>’
    $aadClientID = '<Client ID of your Azure AD application>'
    connect-msolservice
    New-MsolServicePrincipalCredential -AppPrincipalId $aadClientID -Type password -Value $clientSecret

#### <a name="certificate-based-authentication-for-azure-ad"></a>Ověřování pomocí certifikátů pro Azure AD
> [!NOTE]
> Na virtuální počítače s Linuxem se aktuálně nepodporuje ověřování pomocí certifikátů Azure AD.

Hello oddíly, které následují zobrazit jak tooconfigure ověřování pomocí certifikátů pro Azure AD.

##### <a name="create-an-azure-ad-application"></a>Vytvořit aplikaci Azure AD
toocreate aplikaci Azure AD, spusťte následující rutiny prostředí PowerShell hello:

> [!NOTE]
> Nahraďte hello následující `yourpassword` řetězce s zabezpečeného hesla a chránit hello heslo.

    $cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate("C:\certificates\examplecert.pfx", "yourpassword")
    $keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())
    $azureAdApplication = New-AzureRmADApplication -DisplayName "<Your Application Display Name>" -HomePage "<https://YourApplicationHomePage>" -IdentifierUris "<https://YouApplicationUri>" -KeyValue $keyValue -KeyType AsymmetricX509Cert
    $servicePrincipal = New-AzureRmADServicePrincipal –ApplicationId $azureAdApplication.ApplicationId

Po dokončení tohoto kroku nahrát trezoru klíčů tooyour soubor PFX a povolte toodeploy zásad přístupu hello tento certifikát tooa virtuálních počítačů.

##### <a name="use-an-existing-azure-ad-application"></a>Použít existující aplikaci Azure AD
Při konfiguraci ověřování pomocí certifikátů pro existující aplikace, použijte rutiny prostředí PowerShell hello tady uvedené. Být jisti tooexecute jim nové okno prostředí PowerShell.

    $certLocalPath = 'C:\certs\myaadapp.cer'
    $aadClientID = '<Client ID of your Azure AD application>'
    connect-msolservice
    $cer = New-Object System.Security.Cryptography.X509Certificates.X509Certificate
    $cer.Import($certLocalPath)
    $binCert = $cer.GetRawCertData()
    $credValue = [System.Convert]::ToBase64String($binCert);
    New-MsolServicePrincipalCredential -AppPrincipalId $aadClientID -Type asymmetric -Value $credValue -Usage verify

Po dokončení tohoto kroku nahrát trezoru klíčů tooyour soubor PFX a povolte hello zásady přístupu, které je potřeba toodeploy hello certifikát tooa virtuálních počítačů.

##### <a name="upload-a-pfx-file-tooyour-key-vault"></a>Nahrát trezoru klíčů tooyour souboru PFX
Podrobné vysvětlení tohoto procesu najdete v tématu [hello oficiální Blog týmu klíč trezoru Azure](http://blogs.technet.com/b/kv/archive/2015/07/14/vm_2d00_certificates.aspx). Hello následující rutiny prostředí PowerShell jsou však všechny, které potřebujete pro úlohu hello. Být jisti tooexecute je z konzoly Azure PowerShell.

> [!NOTE]
> Nahraďte hello následující `yourpassword` řetězce s zabezpečeného hesla a chránit hello heslo.

    $certLocalPath = 'C:\certs\myaadapp.pfx'
    $certPassword = "yourpassword"
    $resourceGroupName = ‘yourResourceGroup’
    $keyVaultName = ‘yourKeyVaultName’
    $keyVaultSecretName = ‘yourAadCertSecretName’

    $fileContentBytes = get-content $certLocalPath -Encoding Byte
    $fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)

    $jsonObject = @"
    {
    "data": "$filecontentencoded",
    "dataType" :"pfx",
    "password": "$certPassword"
    }
    "@

    $jsonObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
    $jsonEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)

    Switch-AzureMode -Name AzureResourceManager
    $secret = ConvertTo-SecureString -String $jsonEncoded -AsPlainText -Force
    Set-AzureKeyVaultSecret -VaultName $keyVaultName -Name $keyVaultSecretName -SecretValue $secret
    Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -ResourceGroupName $resourceGroupName –EnabledForDeployment

##### <a name="deploy-a-certificate-in-your-key-vault-tooan-existing-vm"></a>Nasaďte certifikát v váš trezor klíčů tooan existující virtuální počítač
Po dokončení nahrávání hello PFX, nasaďte certifikát v tooan trezoru klíčů hello existující virtuální počítač s hello následující:
 ```
    $resourceGroupName = ‘yourResourceGroup’
    $keyVaultName = ‘yourKeyVaultName’
    $keyVaultSecretName = ‘yourAadCertSecretName’
    $vmName = ‘yourVMName’
    $certUrl = (Get-AzureKeyVaultSecret -VaultName $keyVaultName -Name $keyVaultSecretName).Id
    $sourceVaultId = (Get-AzureRmKeyVault -VaultName $keyVaultName -ResourceGroupName $resourceGroupName).ResourceId
    $vm = Get-AzureRmVM -ResourceGroupName $resourceGroupName -Name $vmName
    $vm = Add-AzureRmVMSecret -VM $vm -SourceVaultId $sourceVaultId -CertificateStore "My" -CertificateUrl $certUrl
    Update-AzureRmVM -VM $vm  -ResourceGroupName $resourceGroupName
 ```

#### <a name="set-up-hello-key-vault-access-policy-for-hello-azure-ad-application"></a>Nastavte zásady přístupu hello trezoru klíčů pro aplikaci hello Azure AD
Aplikace Azure AD musí práva tooaccess hello klíčů nebo tajných klíčů v trezoru hello. Použití hello [ `Set-AzureKeyVaultAccessPolicy` ](/powershell/module/azure/set-azurekeyvaultaccesspolicy?view=azuresmps-3.7.0) rutiny toogrant oprávnění toohello aplikace, pomocí ID klienta hello, (který byl vygenerován při byl zaregistrován aplikace hello) jako hello _– ServicePrincipalName_ Hodnota parametru. toolearn více, najdete v příspěvku blogu hello [Azure Key Vault - krok za krokem](http://blogs.technet.com/b/kv/archive/2015/06/02/azure-key-vault-step-by-step.aspx). Tady je příklad, jak tooperform tato úloha pomocí prostředí PowerShell:

    $keyVaultName = '<yourKeyVaultName>'
    $aadClientID = '<yourAadAppClientID>'
    $rgname = '<yourResourceGroup>'
    Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -ServicePrincipalName $aadClientID -PermissionsToKeys 'WrapKey' -PermissionsToSecrets 'Set' -ResourceGroupName $rgname

> [!NOTE]
> Azure Disk Encryption vyžaduje, abyste tooconfigure hello následující aplikace klienta tooyour Azure AD zásady přístupu: _WrapKey_ a _nastavit_ oprávnění.

## <a name="terminology"></a>Terminologie
některé běžné podmínky hello používá tuto technologii, použijte hello následující tabulka terminologie toounderstand:

| Terminologie | Definice |
| --- | --- |
| Azure AD | Azure AD je [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/). Účet Azure AD je předpokladem pro ověřování, ukládání a načítání tajné klíče z trezoru klíčů. |
| Azure Key Vault | Key Vault je služba kryptografické klíče správy, která je založena na moduly ověřit Federal Information Processing Standards FIPS hardwarového zabezpečení, které v zájmu ochrany kryptografické klíče a tajné klíče citlivé. Další informace najdete v tématu [Key Vault](https://azure.microsoft.com/services/key-vault/) dokumentaci. |
| ARM | Azure Resource Manager |
| Nástroj BitLocker |[Nástroj BitLocker](https://technet.microsoft.com/library/hh831713.aspx) je rozpoznána odvětví Windows svazku šifrovací technologie, která se používá šifrování disku tooenable na virtuální počítače IaaS Windows. |
| BEK | Nástroj BitLocker šifrovací klíče jsou použité tooencrypt hello OS spouštěcí svazek a datové svazky. klíče Bitlockeru Hello chráněna jako tajných klíčů v trezoru klíčů. |
| Rozhraní příkazového řádku | V tématu [rozhraní příkazového řádku Azure](../cli-install-nodejs.md). |
| DM-Crypt |[DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) je podsystém hello systémem Linux, transparentní-šifrování disku, který se používá šifrování disku tooenable na virtuální počítače IaaS Linux. |
| KEK | Klíče šifrovací klíč je hello asymetrický klíč (RSA 2048), můžete použít tooprotect nebo zabalení hello tajný klíč. Můžete zadat hardwaru zabezpečení (HSM) moduly-chráněný klíč, nebo softwarově chráněný klíč. Další podrobnosti najdete v tématu [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) dokumentaci. |
| PS rutiny | V tématu [rutin prostředí Azure PowerShell](/powershell/azure/overview). |

### <a name="set-up-and-configure-your-key-vault-for-azure-disk-encryption"></a>Nastavení a konfigurace trezoru klíčů pro Azure Disk Encryption
Azure Disk Encryption pomáhá chránit hello šifrování disku klíče a tajné klíče v trezoru klíčů. tooset do trezoru klíčů pro Azure Disk Encryption, dokončení hello kroky v každé z hello následující části.

#### <a name="create-a-key-vault"></a>Vytvořte trezor klíčů
toocreate trezoru klíčů, použijte jednu z hello následující možnosti:

* ["101-Key-trezoru-vytvořit" šablony Resource Manageru](https://github.com/Azure/azure-quickstart-templates/tree/master/101-key-vault-create)
* [Rutiny Azure PowerShell trezoru klíčů](/powershell/module/azurerm.keyvault/#key_vault)
* Azure Resource Manager
* Jak příliš[zabezpečit váš trezor klíčů](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-secure-your-key-vault)

> [!NOTE]
> Pokud jste již nastavili trezoru klíčů pro vaše předplatné, toohello další část přeskočte.

![Azure Key Vault](./media/azure-security-disk-encryption/keyvault-portal-fig1.png)

#### <a name="set-up-a-key-encryption-key-optional"></a>Nastavení klíče šifrovací klíč (volitelné)
Pokud chcete použít pro další úroveň zabezpečení pro šifrovací klíče nástroje BitLocker hello toouse KEK, přidejte trezoru klíčů tooyour KEK. Použití hello [ `Add-AzureKeyVaultKey` ](/powershell/module/azurerm.keyvault/add-azurermkeyvaultkey) rutiny toocreate klíče šifrovací klíč v trezoru klíčů hello. Můžete také importovat KEK z vaší místní správy k klíče HSM. Další podrobnosti najdete v tématu [klíč trezoru dokumentaci](https://azure.microsoft.com/documentation/services/key-vault/).

    Add-AzureKeyVaultKey [-VaultName] <string> [-Name] <string> -Destination <string> {HSM | Software}

Hello KEK můžete přidat tak, že přejdete tooAzure Resource Manager nebo pomocí rozhraní váš trezor klíčů.

![Azure Key Vault](./media/azure-security-disk-encryption/keyvault-portal-fig2.png)

#### <a name="set-key-vault-permissions"></a>Nastavte oprávnění trezoru klíčů
potřebuje přístup toohello šifrovacích klíčů nebo tajných klíčů v váš trezor klíčů toomake zprostředkovatele Hello platformy Azure je k dispozici toohello virtuálního počítače pro spouštění a dešifrování hello svazky. toogrant oprávnění toohello platformy Azure sadu hello **EnabledForDiskEncryption** vlastnost hello klíč trezoru pomocí rutiny prostředí PowerShell hello trezoru klíčů:

    Set-AzureRmKeyVaultAccessPolicy -VaultName <yourVaultName> -ResourceGroupName <yourResourceGroup> -EnabledForDiskEncryption

Můžete také nastavit hello **EnabledForDiskEncryption** vlastnost návštěvou hello [Průzkumníka prostředků Azure](https://resources.azure.com).

Jak už bylo zmíněno dříve, je nutné nastavit hello **EnabledForDiskEncryption** vlastnost v trezoru klíčů. V opačném hello nasazení se nezdaří.

Můžete nastavit pro vaši aplikaci Azure AD z rozhraní hello trezoru klíčů, zásady přístupu, jak je vidět tady:

![Azure Key Vault](./media/azure-security-disk-encryption/keyvault-portal-fig3.png)

![Azure Key Vault](./media/azure-security-disk-encryption/keyvault-portal-fig3b.png)

Na hello **zásady rozšířené přístupu** kartě, ujistěte se, že váš trezor klíčů je povolena pro Azure Disk Encryption:

![Azure trezoru klíčů](./media/azure-security-disk-encryption/keyvault-portal-fig4.png)

## <a name="disk-encryption-deployment-scenarios-and-user-experiences"></a>Scénáře nasazení šifrování disku a koncových uživatelů
Můžete povolit mnoho scénářů šifrování disku a hello kroky se mohou lišit podle toohello scénář. Hello následující části se věnují hello scénáře podrobněji.

### <a name="enable-encryption-on-new-iaas-vms-that-are-created-from-hello-marketplace"></a>Povolit šifrování na nové virtuální počítače IaaS, které jsou vytvořené pomocí hello Marketplace
Šifrování disku na nový virtuální počítač IaaS Windows z hello v Azure Marketplace můžete povolit pomocí hello [šablony Resource Manageru](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image).

1. V šabloně hello Azure rychlý start, klikněte na tlačítko **nasazení tooAzure**, zadejte hello šifrování konfigurace na hello **parametry** okna a pak klikněte na tlačítko **OK**.

2. Vyberte hello předplatné, skupinu prostředků, umístění skupiny prostředků, právní podmínky a smlouvy a pak klikněte na tlačítko **vytvořit** tooenable šifrování na nový virtuální počítač IaaS.

> [!NOTE]
> Tato šablona vytvoří nový virtuální počítač šifrované Windows, která využívá image Galerie hello systému Windows Server 2012.

Můžete povolit šifrování disku na nový IaaS RedHat Linux 7.2 virtuální počítač s polem RAID-0 200 GB prostřednictvím tohoto [šablony Resource Manageru](https://aka.ms/fde-rhel). Po nasazení šablony hello ověřit stav šifrování hello virtuálního počítače pomocí hello `Get-AzureRmVmDiskEncryptionStatus` rutiny, jak je popsáno v [OS šifrování jednotky na spuštěný virtuální počítač s Linuxem](#encrypting-os-drive-on-a-running-linux-vm). Když počítač hello vrátí stav _VMRestartPending_, restartujte hello virtuálních počítačů.

Hello následující tabulka uvádí hello parametrů šablony Resource Manageru pro nové virtuální počítače z hello Marketplace scénář pomocí ID klienta Azure AD:

| Parametr | Popis |
| --- | --- |
| adminUserName | Uživatelské jméno správce pro virtuální počítač hello. |
| adminPassword | Uživatelské heslo správce pro virtuální počítač hello. |
| newStorageAccountName | Název hello úložiště účet toostore operačního systému a dat virtuální pevné disky. |
| vmSize | Velikost hello virtuálních počítačů. V současné době jsou podporovány pouze standardní A, D a G řady. |
| virtualNetworkName | Název hello virtuální síť této hello síťový adaptér virtuálního počítače by měly patřit do. |
| subnetName | Název podsítě hello v hello virtuální síť této hello síťový adaptér virtuálního počítače by měly patřit do. |
| AADClientID | ID klienta aplikace hello Azure AD, který má oprávnění toowrite tajné klíče tooyour klíče trezoru. |
| AADClientSecret | Tajný klíč klienta aplikace hello Azure AD, který má oprávnění toowrite tajné klíče tooyour klíče trezoru. |
| keyVaultURL | Adresa URL hello klíče trezoru této hello klíč musí být nahrán do nástroje BitLocker. Můžete ho získat pomocí rutiny hello `(Get-AzureRmKeyVault -VaultName,-ResourceGroupName ).VaultURI`. |
| keyEncryptionKeyURL | Adresa URL hello klíče šifrovací klíč, který je použité tooencrypt hello vygenerovat klíč nástroje BitLocker (volitelné). |
| keyVaultResourceGroup | Skupiny prostředků hello trezoru klíčů. |
| vmName | Název hello virtuální počítač, který hello operace šifrování je toobe na provést. |

> [!NOTE]
> _KeyEncryptionKeyURL_ je volitelný parametr. V trezoru klíčů, můžete zahrnout vlastní KEK toofurther chránit hello datový šifrovací klíč (heslo tajný klíč).

### <a name="enable-encryption-on-new-iaas-vms-that-are-created-from-customer-encrypted-vhd-and-encryption-keys"></a>Povolit šifrování na nové virtuální počítače IaaS, vytvořený z šifrované zákazníka virtuálního pevného disku a šifrovacích klíčů
V tomto scénáři můžete povolit šifrování pomocí šablony Resource Manageru hello, rutiny prostředí PowerShell nebo rozhraní příkazového řádku. Hello následující části popisují větší šablony Resource Manageru hello podrobností a rozhraní příkazového řádku.

Postupujte podle pokynů hello z jednoho z těchto částí pro přípravu předem šifrované bitové kopie, které lze použít v Azure. Po vytvoření bitové kopie hello můžete hello kroků v další části toocreate hello šifrované virtuálního počítače Azure.

* [Příprava předem šifrované virtuálního pevného disku Windows](#preparing-a-pre-encrypted-windows-vhd)
* [Příprava předem šifrované Linux virtuální pevný disk](#preparing-a-pre-encrypted-linux-vhd)

#### <a name="using-hello-resource-manager-template"></a>Pomocí šablony Resource Manageru hello
Můžete povolit šifrování disku na svůj disk VHD šifrované pomocí hello [šablony Resource Manageru](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-pre-encrypted-vm).

1. V šabloně hello Azure rychlý start, klikněte na tlačítko **nasazení tooAzure**, zadejte hello šifrování konfigurace na hello **parametry** okna a pak klikněte na tlačítko **OK**.

2. Vyberte hello předplatné, skupinu prostředků, umístění skupiny prostředků, právní podmínky a smlouvy a pak klikněte na tlačítko **vytvořit** hello tooenable šifrování na nový virtuální počítač IaaS.

Hello následující tabulka uvádí hello parametrů šablony Resource Manageru pro vaše virtuální pevný disk šifrovaný:

| Parametr | Popis |
| --- | --- |
| newStorageAccountName | Název hello úložiště účet toostore hello šifrované virtuálního pevného disku operačního systému. Tento účet úložiště by měl již byla vytvořena v hello stejnou skupinu prostředků a stejné umístění jako hello virtuálních počítačů. |
| osVhdUri | Identifikátor URI hello virtuálního pevného disku operačního systému z účtu úložiště hello. |
| osType | Typ produktu operačního systému (Windows nebo Linuxem). |
| virtualNetworkName | Název hello virtuální síť této hello síťový adaptér virtuálního počítače by měly patřit do. Hello název by měl již byla vytvořena v hello stejnou skupinu prostředků a stejné umístění jako hello virtuálních počítačů. |
| subnetName | Název podsítě hello na hello virtuální síť této hello síťový adaptér virtuálního počítače by měly patřit do. |
| vmSize | Velikost hello virtuálních počítačů. V současné době jsou podporovány pouze standardní A, D a G řady. |
| keyVaultResourceID | Hello ResourceID, který identifikuje prostředek hello trezoru klíčů ve službě Správce prostředků Azure. Můžete ho získat pomocí rutiny prostředí PowerShell hello `(Get-AzureRmKeyVault -VaultName &lt;yourKeyVaultName&gt; -ResourceGroupName &lt;yourResourceGroupName&gt;).ResourceId`. |
| keyVaultSecretUrl | Adresa URL hello disku šifrovací klíč, který je nastavený v trezoru klíčů hello. |
| keyVaultKekUrl | Adresa URL hello klíče šifrovacího klíče pro šifrování hello vygenerovat klíč šifrování disku. |
| vmName | Název hello virtuálních počítačů IaaS. |

#### <a name="using-powershell-cmdlets"></a>Pomocí rutin prostředí PowerShell
Můžete povolit šifrování disku na svůj disk VHD šifrované pomocí rutiny prostředí PowerShell hello [ `Set-AzureRmVMOSDisk` ](/powershell/module/azurerm.compute/set-azurermvmosdisk).  

#### <a name="using-cli-commands"></a>Pomocí rozhraní příkazového řádku
šifrování disku tooenable pro tento scénář pomocí rozhraní příkazového řádku, hello následující:

1. Nastavení zásad přístupu v trezoru klíčů:

   * Sada hello **EnabledForDiskEncryption** příznak:

    `azure keyvault set-policy --vault-name <keyVaultName> --enabled-for-disk-encryption true`
   * Nastavte oprávnění tooAzure AD aplikace toowrite tajné klíče tooyour klíče trezoru:

    `azure keyvault set-policy --vault-name <keyVaultName> --spn <aadClientID> --perms-to-keys '["wrapKey"]' --perms-to-secrets '["set"]'`

2. šifrování tooenable ve stávající nebo spuštěném virtuálním počítači, zadejte:

 `azure vm enable-disk-encryption --resource-group <resourceGroupName> --name <vmName> --aad-client-id <aadClientId> --aad-client-secret <aadClientSecret> --disk-encryption-key-vault-url <keyVaultURL> --disk-encryption-key-vault-id <keyVaultResourceId> --volume-type [All|OS|Data]`

3. Načíst stav šifrování:

 `azure vm show-disk-encryption-status --resource-group <resourceGroupName> --name <vmName> --json`

4. šifrování tooenable na nový virtuální počítač z vaší šifrované virtuálního pevného disku, použijte hello následující parametry s hello `azure vm create` příkaz:

 ```
   * disk-encryption-key-vault-id <disk-encryption-key-vault-id>
   * disk-encryption-key-url <disk-encryption-key-url>
   * key-encryption-key-vault-id <key-encryption-key-vault-id>
   * key-encryption-key-url <key-encryption-key-url>
 ```

### <a name="enable-encryption-on-existing-or-running-iaas-windows-vm-in-azure"></a>Povolit šifrování na existující nebo běžící virtuální počítače IaaS Windows v Azure
V tomto scénáři můžete povolit šifrování pomocí šablony Resource Manageru hello, rutiny prostředí PowerShell nebo rozhraní příkazového řádku. Hello následující části popisují podrobněji jak tooenable ho pomocí hello šablony Resource Manageru a rozhraní příkazového řádku.

#### <a name="using-hello-resource-manager-template"></a>Pomocí šablony Resource Manageru hello
Můžete povolit šifrování disku na existující nebo spuštěných virtuálních počítačů IaaS Windows Azure pomocí hello [šablony Resource Manageru](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-windows-vm).

1. V šabloně hello Azure rychlý start, klikněte na tlačítko **nasazení tooAzure**, zadejte hello šifrování konfigurace na hello **parametry** okna a pak klikněte na tlačítko **OK**.

2. Vyberte hello předplatné, skupinu prostředků, umístění skupiny prostředků, právní podmínky a smlouvy a pak klikněte na tlačítko **vytvořit** tooenable šifrování na hello existující nebo spuštění virtuálních počítačů IaaS.

Hello následující tabulka uvádí hello parametrů šablony Resource Manageru pro existující nebo spuštěných virtuálních počítačů, které používají ID klienta Azure AD:

| Parametr | Popis |
| --- | --- |
| AADClientID | ID klienta aplikace hello Azure AD, který má oprávnění toowrite tajné klíče toohello klíče trezoru. |
| AADClientSecret | Tajný klíč klienta aplikace hello Azure AD, který má oprávnění toowrite tajné klíče toohello klíče trezoru. |
| keyVaultName | Název klíče hello trezoru této hello klíč musí být nahrán do nástroje BitLocker. Můžete ho získat pomocí rutiny hello `(Get-AzureRmKeyVault -ResourceGroupName <yourResourceGroupName>). Vaultname`. |
|  keyEncryptionKeyURL | Adresa URL hello klíče šifrovací klíč, který je použité tooencrypt hello vygenerovat klíč nástroje BitLocker. Tento parametr je nepovinný, pokud vyberete **nokek** hello UseExistingKek rozevíracího seznamu. Pokud vyberete **kek** hello UseExistingKek rozevíracího seznamu, je nutné zadat hello _keyEncryptionKeyURL_ hodnotu. |
| volumeType | Typ svazku, který hello operace šifrování. Platné hodnoty jsou _OS_, _Data_, a _všechny_. |
| sequenceVersion | Pořadí verze hello operace nástroje BitLocker. Pokaždé, když probíhá operace šifrování disku na hello zvýšit toto číslo verze stejného virtuálního počítače. |
| vmName | Název hello virtuální počítač, který hello operace šifrování je toobe na provést. |

> [!NOTE]
> _KeyEncryptionKeyURL_ je volitelný parametr. V trezoru klíčů hello můžete zahrnout vlastní KEK toofurther chránit hello datový šifrovací klíč (šifrování nástroje BitLocker tajný klíč).

#### <a name="using-powershell-cmdlets"></a>Pomocí rutin prostředí PowerShell
Informace o povolení šifrování s Azure Disk Encryption pomocí rutin prostředí PowerShell, najdete v příspěvcích na blogu hello [prozkoumat Azure Disk Encryption s prostředím Azure PowerShell – část 1](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/17/explore-azure-disk-encryption-with-azure-powershell.aspx) a [prozkoumat Azure Disk Encryption v prostředí Azure PowerShell - část 2](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx).

#### <a name="using-cli-commands"></a>Pomocí rozhraní příkazového řádku
šifrování tooenable na existující nebo v Azure pomocí rozhraní příkazového řádku, spuštěné virtuální počítače IaaS Windows hello následující:

1. zásady přístupu tooset v trezoru klíčů hello:
   * Sada hello **EnabledForDiskEncryption** příznak:

    `azure keyvault set-policy --vault-name <keyVaultName> --enabled-for-disk-encryption true`
   * Nastavte oprávnění tooAzure AD aplikace toowrite tajné klíče tooyour klíče trezoru:

    `azure keyvault set-policy --vault-name <keyVaultName> --spn <aadClientID> --perms-to-keys '["wrapKey"]' --perms-to-secrets '["set"]'`
2. šifrování tooenable na stávající nebo spuštěný virtuální počítač:

 `azure vm enable-disk-encryption --resource-group <resourceGroupName> --name <vmName> --aad-client-id <aadClientId> --aad-client-secret <aadClientSecret> --disk-encryption-key-vault-url <keyVaultURL> --disk-encryption-key-vault-id <keyVaultResourceId> --volume-type [All|OS|Data]`
3. stav šifrování tooget:

 `azure vm show-disk-encryption-status --resource-group <resourceGroupName> --name <vmName> --json`
4. šifrování tooenable na nový virtuální počítač z vaší šifrované virtuálního pevného disku, použijte hello následující parametry s hello `azure vm create` příkaz:

 ```
   * disk-encryption-key-vault-id <disk-encryption-key-vault-id>
   * disk-encryption-key-url <disk-encryption-key-url>
   * key-encryption-key-vault-id <key-encryption-key-vault-id>
   * key-encryption-key-url <key-encryption-key-url>
 ```

### <a name="enable-encryption-on-an-existing-or-running-iaas-linux-vm-in-azure"></a>Povolit šifrování na existující nebo spuštěné IaaS Linux virtuální počítač v Azure
Můžete povolit šifrování disku na existující nebo spuštěné IaaS Linux virtuální počítač v Azure pomocí hello [šablony Resource Manageru](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-linux-vm).

1. Klikněte na tlačítko **nasazení tooAzure** v šabloně hello Azure úvodní zadejte hello šifrování konfigurace na hello **parametry** okna a pak klikněte na tlačítko **OK**.

2. Vyberte hello předplatné, skupinu prostředků, umístění skupiny prostředků, právní podmínky a smlouvy a pak klikněte na tlačítko **vytvořit** tooenable šifrování na hello existující nebo spuštění virtuálních počítačů IaaS.

Hello následující tabulka obsahuje seznam parametrů šablony Resource Manageru pro existující nebo spuštěných virtuálních počítačů, které používají ID klienta Azure AD:

| Parametr | Popis |
| --- | --- |
| AADClientID | ID klienta aplikace hello Azure AD, který má oprávnění toowrite tajné klíče toohello klíče trezoru. |
| AADClientSecret | Tajný klíč klienta aplikace hello Azure AD, který má oprávnění toowrite tajné klíče tooyour klíče trezoru. |
| keyVaultName | Název klíče hello trezoru této hello klíč musí být nahrán do nástroje BitLocker. Můžete ho získat pomocí rutiny hello `(Get-AzureRmKeyVault -ResourceGroupName <yourResourceGroupName>). Vaultname`. |
|  keyEncryptionKeyURL | Adresa URL hello klíče šifrovací klíč, který je použité tooencrypt hello vygenerovat klíč nástroje BitLocker. Tento parametr je nepovinný, pokud vyberete **nokek** hello UseExistingKek rozevíracího seznamu. Pokud vyberete **kek** hello UseExistingKek rozevíracího seznamu, je nutné zadat hello _keyEncryptionKeyURL_ hodnotu. |
| volumeType | Typ svazku, který hello operace šifrování. Platné podporované hodnoty jsou _OS_ nebo _všechny_ (pro RHEL 7.2, CentOS 7.2 a Ubuntu 16.04), a _Data_ (pro všechny ostatní distribuce). |
| sequenceVersion | Pořadí verze hello operace nástroje BitLocker. Pokaždé, když probíhá operace šifrování disku na hello zvýšit toto číslo verze stejného virtuálního počítače. |
| vmName | Název hello virtuální počítač, který hello operace šifrování je toobe na provést. |
| přístupové heslo | Zadejte silné heslo jako hello datový šifrovací klíč. |

> [!NOTE]
> _KeyEncryptionKeyURL_ je volitelný parametr. V trezoru klíčů, můžete zahrnout vlastní KEK toofurther chránit hello datový šifrovací klíč (heslo tajný klíč).

#### <a name="cli-commands"></a>Rozhraní příkazového řádku
Můžete povolit šifrování disku na svůj disk VHD šifrované instalací a použitím hello [rozhraní příkazového řádku příkaz](../cli-install-nodejs.md). šifrování tooenable na existující nebo běžící virtuální počítače IaaS Linux v Azure pomocí rozhraní příkazového řádku, hello následující:

1. Nastavení zásad přístupu v trezoru klíčů hello:

 * Sada hello **EnabledForDiskEncryption** příznak:

    `azure keyvault set-policy --vault-name <keyVaultName> --enabled-for-disk-encryption true`
 * Nastavte oprávnění tooAzure AD aplikace toowrite tajné klíče tooyour klíče trezoru:

    `azure keyvault set-policy --vault-name <keyVaultName> --spn <aadClientID> --perms-to-keys '["wrapKey"]' --perms-to-secrets '["set"]'`

2. šifrování tooenable na stávající nebo spuštěný virtuální počítač:

 `azure vm enable-disk-encryption --resource-group <resourceGroupName> --name <vmName> --aad-client-id <aadClientId> --aad-client-secret <aadClientSecret> --disk-encryption-key-vault-url <keyVaultURL> --disk-encryption-key-vault-id <keyVaultResourceId> --volume-type [All|OS|Data]`

3. Načíst stav šifrování:

 `azure vm show-disk-encryption-status --resource-group <resourceGroupName> --name <vmName> --json`

4. šifrování tooenable na nový virtuální počítač z vaší šifrované virtuálního pevného disku, použijte hello následující parametry s hello `azure vm create` příkaz:
 ```
   * disk-encryption-key-vault-id <disk-encryption-key-vault-id>
   * disk-encryption-key-url <disk-encryption-key-url>
   * key-encryption-key-vault-id <key-encryption-key-vault-id>
   * key-encryption-key-url <key-encryption-key-url>
 ```

### <a name="get-hello-encryption-status-of-an-encrypted-iaas-vm"></a>Načíst stav šifrování hello šifrovaný IaaS virtuálního počítače
Pomocí Azure Resource Manager, můžete získat stav šifrování hello [rutiny prostředí PowerShell](/powershell/azure/overview), nebo rozhraní příkazového řádku. Hello následující části popisují, jak toouse hello portál Azure classic a rozhraní příkazového řádku příkazy tooget hello stav šifrování.

#### <a name="get-hello-encryption-status-of-an-encrypted-windows-vm-by-using-azure-resource-manager"></a>Získat stav šifrování hello šifrované virtuální počítač Windows pomocí Azure Resource Manager
Stav šifrování hello hello virtuálních počítačů IaaS můžete získat z Azure Resource Manager pomocí tohoto postupu hello následující:

1. Přihlaste se toohello [portál Azure classic](https://portal.azure.com/)a potom klikněte na **virtuální počítače** v levém podokně toosee hello přehled hello virtuálních počítačů v rámci vašeho předplatného. Můžete filtrovat zobrazení hello virtuální počítače, vyberte název odběru hello v hello **předplatné** rozevíracího seznamu.

2. Hello horní části hello **virtuální počítače** klikněte na tlačítko **sloupce**.

3. Na hello **vyberte sloupec** vyberte **šifrování disku**a potom klikněte na **aktualizace**. Měli byste vidět stav šifrování hello šifrování disku sloupec zobrazuje hello _povoleno_ nebo _není povoleno_ pro každý virtuální počítač, jak ukazuje následující obrázek hello:

 ![Microsoft Antimalware v Azure](./media/azure-security-disk-encryption/disk-encryption-fig2.png)

#### <a name="get-hello-encryption-status-of-an-encrypted-windowslinux-iaas-vm-by-using-hello-disk-encryption-powershell-cmdlet"></a>Načíst stav šifrování hello virtuálního počítače šifrovaný IaaS (Windows nebo Linuxem) pomocí rutiny prostředí PowerShell hello šifrování disku
Stav šifrování hello hello virtuálních počítačů IaaS můžete získat z rutiny prostředí PowerShell šifrování disku hello `Get-AzureRmVMDiskEncryptionStatus`. nastavení šifrování hello tooget pro virtuální počítač, zadejte následující hello:

    C:\> Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $ResourceGroupName -VMName $VMName
    -ExtensionName $ExtensionName

    OsVolumeEncrypted          : NotEncrypted
    DataVolumesEncrypted       : Encrypted
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : https://rheltest1keyvault.vault.azure.net/secrets/bdb6bfb1-5431-4c28-af46-b18d0025ef2a/abebacb83d864a5fa729508315020f8a

Si můžete prohlédnout výstup hello _Get-AzureRmVMDiskEncryptionStatus_ pro šifrování klíče adresy URL.

    C:\> $status = Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $ResourceGroupName -VMName
    e $VMName -ExtensionName $ExtensionName
    C:\> $status.OsVolumeEncryptionSettings

    DiskEncryptionKey                                                 KeyEncryptionKey                                               Enabled
    -----------------                                                 ----------------                                               -------
    Microsoft.Azure.Management.Compute.Models.KeyVaultSecretReference Microsoft.Azure.Management.Compute.Models.KeyVaultKeyReference    True


    C:\> $status.OsVolumeEncryptionSettings.DiskEncryptionKey.SecretUrl
    https://rheltest1keyvault.vault.azure.net/secrets/bdb6bfb1-5431-4c28-af46-b18d0025ef2a/abebacb83d864a5fa729508315020f8a
    C:\> $status.OsVolumeEncryptionSettings.DiskEncryptionKey

    SecretUrl                                                                                                               SourceVault
    ---------                                                                                                               -----------
    https://rheltest1keyvault.vault.azure.net/secrets/bdb6bfb1-5431-4c28-af46-b18d0025ef2a/abebacb83d864a5fa729508315020f8a Microsoft.Azure.Management....

Hello OSVolumeEncrypted a DataVolumesEncrypted nastavení hodnoty jsou nastaveny too_Encrypted_, který ukazuje, že jsou oba svazky šifrované pomocí Azure Disk Encryption. Informace o povolení šifrování s Azure Disk Encryption pomocí rutin prostředí PowerShell, najdete v příspěvcích na blogu hello [prozkoumat Azure Disk Encryption s prostředím Azure PowerShell – část 1](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/17/explore-azure-disk-encryption-with-azure-powershell.aspx) a [prozkoumat Azure Disk Encryption v prostředí Azure PowerShell - část 2](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx).

> [!NOTE]
> Na virtuální počítače s Linuxem, trvá tři toofour minut hello `Get-AzureRmVMDiskEncryptionStatus` stav šifrování hello tooreport rutiny.

#### <a name="get-hello-encryption-status-of-hello-iaas-vm-from-hello-disk-encryption-cli-command"></a>Získat stav šifrování hello hello virtuálních počítačů IaaS z příkazu rozhraní příkazového řádku hello šifrování disku
Stav šifrování hello hello virtuálních počítačů IaaS můžete získat pomocí příkazu rozhraní příkazového řádku šifrování disku hello `azure vm show-disk-encryption-status`. nastavení šifrování hello tooget pro virtuální počítač, zadejte relaci příkazového řádku Azure CLI:

    azure vm show-disk-encryption-status --resource-group <yourResourceGroupName> --name <yourVMName> --json  

#### <a name="disable-encryption-on-running-windows-iaas-vm"></a>Zakázat šifrování u spuštěných virtuálních počítačů IaaS Windows
Můžete zakázat šifrování na spuštěný Windows nebo virtuálních počítačů IaaS Linux pomocí šablony Azure Disk Encryption Resource Manageru hello nebo rutiny prostředí PowerShell a zadejte konfiguraci dešifrování hello.

##### <a name="windows-vm"></a>Virtuální počítač s Windows
Hello zakázat šifrování krok zakazuje šifrování hello operačního systému, hello datový svazek nebo obojí na spuštění virtuálních počítačů IaaS Windows hello. Nelze zakázat hello svazku operačního systému a nechte hello datový svazek zašifrovaná. Při provádění krok zakázat šifrování hello modelu nasazení Azure classic hello aktualizuje hello modelu služby virtuálních počítačů a virtuální počítače IaaS Windows hello je označena dešifrovaný. obsah Hello hello virtuálního počítače jsou již v zašifrované podobě. dešifrování Hello nedojde k odstranění vašeho klíče trezoru a hello šifrování materiál klíče (šifrovací klíče nástroje BitLocker pro systém Windows a heslo pro Linux).

##### <a name="linux-vm"></a>Virtuální počítač s Linuxem
Hello zakázat šifrování krok zakazuje šifrování hello datový svazek na hello spuštění virtuálního počítače s Linuxem IaaS. Tento krok je funkční pouze v případě hello disk operačního systému není zašifrován.

> [!NOTE]
> Zakázáním šifrování na disk s operačním systémem hello není povolená na virtuální počítače s Linuxem.

##### <a name="disable-encryption-on-an-existing-or-running-iaas-vm"></a>Zakázat šifrování na stávající nebo spuštěný virtuální počítač IaaS
Můžete zakázat šifrování disku na spuštěných virtuálních počítačů IaaS Windows pomocí hello [šablony Resource Manageru](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-running-windows-vm).

1. V šabloně hello Azure rychlý start, klikněte na tlačítko **nasazení tooAzure**, zadejte hello dešifrování konfigurace na hello **parametry** okna a pak klikněte na tlačítko **OK**.

2. Vyberte hello předplatné, skupinu prostředků, umístění skupiny prostředků, právní podmínky a smlouvy a pak klikněte na tlačítko **vytvořit** tooenable šifrování na nový virtuální počítač IaaS.

Pro virtuální počítače s Linuxem, můžete zakázat šifrování pomocí hello [vypnout šifrování u spuštěného virtuálního počítače s Linuxem](https://aka.ms/decrypt-linuxvm) šablony.

Hello následující tabulka uvádí parametrů šablony Resource Manageru pro zakázáním šifrování na spuštěný virtuální počítač IaaS:

| Parametr | Popis |
| --- | --- |
| vmName | Název hello virtuální počítač, který hello operace šifrování je toobe na provést.
| volumeType | Typ svazku, který na se provést operaci dešifrování. Platné hodnoty jsou _OS_, _Data_, a _všechny_. Nelze zakázat šifrování u spuštěných virtuálních počítačů IaaS Windows operačního systému nebo spouštěcí svazek bez zakázáním šifrování na hello _Data_ svazku. Všimněte si také, že zakázáním šifrování na disk s operačním systémem hello není povolená na virtuální počítače s Linuxem. |
| sequenceVersion | Pořadí verze hello operace nástroje BitLocker. Pokaždé, když provádí operaci dešifrování disku na hello zvýšit toto číslo verze stejného virtuálního počítače. |

##### <a name="disable-encryption-on-an-existing-or-running-iaas-vm"></a>Zakázat šifrování na stávající nebo spuštěný virtuální počítač IaaS
šifrování toodisable na existující nebo spuštěné IaaS virtuální počítač pomocí rutiny prostředí PowerShell text hello, najdete v části [ `Disable-AzureRmVMDiskEncryption` ](/powershell/module/azurerm.compute/disable-azurermvmdiskencryption). Tato rutina podporuje systém Windows a virtuální počítače s Linuxem. toodisable šifrování, nainstaluje rozšíření ve virtuálním počítači hello. Pokud hello _název_ není zadán parametr, rozšíření s výchozím názvem hello _AzureDiskEncryption virtuálních počítačů pro Windows_ je vytvořena.

Na virtuální počítače s Linuxem je použít hello AzureDiskEncryptionForLinux rozšíření.

> [!NOTE]
> Tato rutina restartuje hello virtuálního počítače.

### <a name="enable-encryption-on-pre-encrypted-iaas-vm-with-azure-managed-disk"></a>Povolit šifrování na předem šifrované virtuálních počítačů IaaS s diskem spravované Azure
Použít toocreate šablony Azure ARM disku spravované hello, šifrované virtuálního počítače z disku VHD předem šifrované pomocí šablony ARM hello naleznete v   
[Vytvořit nové šifrované spravovaným diskem z předem šifrované objektu blob virtuálního pevného disku nebo úložiště] (https://github.com/Azure/azure-quickstart-templates/tree/master/201-create-encrypted-managed-disk)

### <a name="enable-encryption-on-a-new-linux-iaas-vm-with-azure-managed-disk"></a>Povolit šifrování na nový virtuální počítač IaaS Linux s diskem spravované Azure
Použít toocreate šablony Azure ARM disku spravované hello nové šifrované virtuálních počítačů IaaS Linux pomocí šablony ARM hello nacházející se v   
[Nasazení RHEL 7.2 s šifrování celého disku] (https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-full-disk-encrypted-rhel)

### <a name="enable-encryption-on-a-new-windows-iaas-vm-with-azure-managed-disk"></a>Povolit šifrování na nový virtuální počítač IaaS Windows s Azure spravované disku
 Použít toocreate šablony Azure ARM disku spravované hello nové šifrované virtuálních počítačů IaaS Linux pomocí šablony ARM hello nacházející se v   
 [Vytvořit nové šifrovaný IaaS spravované disku virtuální počítač s Windows z Galerie obrázek] (https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image-managed-disks)

  > [!NOTE]
  >Je povinné toosnapshot nebo zálohování spravovaných disků na základě instance virtuálního počítače mimo a předchozí tooenabling Azure Disk Encryption.  Snímek hello spravovaných disků můžete provést z portálu hello nebo Azure Backup lze použít.  Zálohování Ujistěte se, že možnost obnovení je možné v případě hello žádné neočekávané selhání při šifrování.  Po provedení zálohy rutiny Set-AzureRmVMDiskEncryptionExtension hello lze použít tooencrypt spravované disky zadáním parametru - skipVmBackup hello.  Tento příkaz se nezdaří proti spravovaných disků na základě Virtuálního počítače, dokud zálohu byl změněn a tento parametr byla zadána.    
 
### <a name="update-encryption-settings-of-an-existing-encrypted-non-premium-vm"></a>Aktualizovat nastavení šifrování pro existující virtuálního počítače šifrovaná-úrovně premium
  Použití hello stávající Azure disk rozhraní šifrování podporované pro spuštění virtuálního počítače [PS rutiny, rozhraní příkazového řádku nebo ARM šablon] tooupdate nastavení šifrování hello jako klienta AAD ID nebo tajný klíč šifrovacího klíče [KEK] šifrovací klíč nástroje BitLocker pro virtuální počítač s Windows nebo přístupové heslo pro Nastavení šifrování aktualizace hello atd virtuálních počítačů Linux je podporována pouze pro virtuální počítače zajišťoval neprémiové úložiště. Je NNOT podporována u virtuálních počítačů založenou na storage úrovně premium.

## <a name="appendix"></a>Příloha
### <a name="connect-tooyour-subscription"></a>Připojení tooyour odběru
Než budete pokračovat, zkontrolujte hello *požadavky* v tomto článku. Po zajištění, že byly splněny všechny požadavky, připojení odběru tooyour díky hello následující:

1. Spusťte relaci prostředí Azure PowerShell a přihlaste tooyour účet Azure s hello následující příkaz:

    `Login-AzureRmAccount`

2. Pokud máte více předplatných a chcete jeden toouse toospecify, zadejte hello následující toosee hello předplatných pro váš účet:

    `Get-AzureRmSubscription`

3. předplatné hello toospecify chcete toouse, zadejte:

    `Select-AzureRmSubscription -SubscriptionName <Yoursubscriptionname>`

4. tooverify správnost hello předplatné nakonfigurované, typ:

    `Get-AzureRmSubscription`

5. tooconfirm hello Azure Disk Encryption jsou nainstalované rutiny, zadejte:

    `Get-command *diskencryption*`

6. Následující výstup Hello potvrdí instalace Azure Disk Encryption PowerShell hello:

```
    PS C:\Windows\System32\WindowsPowerShell\v1.0> get-command *diskencryption*
    CommandType  Name                                         Source                                                             
    Cmdlet       Get-AzureRmVMDiskEncryptionStatus            AzureRM.Compute                                                    
    Cmdlet       Disable-AzureRmVMDiskEncryption              AzureRM.Compute                                                    
    Cmdlet       Set-AzureRmVMDiskEncryptionExtension         AzureRM.Compute                                                     
```

### <a name="prepare-a-pre-encrypted-windows-vhd"></a>Příprava předem šifrované virtuálního pevného disku Windows
Hello oddíly, které následují, jsou potřebné tooprepare předem šifrované Windows virtuální pevný disk pro nasazení jako šifrované virtuální pevný disk v Azure IaaS. Použijte informace tooprepare hello a spustit novou Windows virtuálních počítačů (VHD) na Azure Site Recovery nebo Azure.

#### <a name="update-group-policy-tooallow-non-tpm-for-os-protection"></a>Aktualizace skupiny zásad tooallow bez čipu TPM k ochraně operačního systému
Konfigurace nastavení zásad skupiny pro BitLocker hello **nástroj BitLocker Drive Encryption**, které najdete v části **zásady místního počítače** > **konfigurace počítače**  >  **Šablony pro správu** > **součásti systému Windows**. Změnit toto nastavení příliš**jednotky operačního systému** > **vyžadovat další ověření při spuštění** > **povolit nástroj BitLocker bez kompatibilním čipem TPM**, jak ukazuje následující obrázek hello:

![Microsoft Antimalware v Azure](./media/azure-security-disk-encryption/disk-encryption-fig8.png)

#### <a name="install-bitlocker-feature-components"></a>Nainstalujte komponenty funkce nástroje BitLocker
Pro Windows Server 2012 a novější použijte následující příkaz hello:

    dism /online /Enable-Feature /all /FeatureName:BitLocker /quiet /norestart

Pro Windows Server 2008 R2 použijte následující příkaz hello:

    ServerManagerCmd -install BitLockers

#### <a name="prepare-hello-os-volume-for-bitlocker-by-using-bdehdcfg"></a>Přípravě hello svazku operačního systému pomocí nástroje BitLocker`bdehdcfg`
toocompress hello oddílu operačního systému a připravit hello počítač pro nástroj BitLocker, spustit hello následující příkaz:

    bdehdcfg -target c: shrink -quiet

#### <a name="protect-hello-os-volume-by-using-bitlocker"></a>Chránit hello svazku operačního systému pomocí nástroje BitLocker
Použití hello [ `manage-bde` ](https://technet.microsoft.com/library/ff829849.aspx) příkaz tooenable šifrování na spouštěcím svazku hello pomocí externí ochranné zařízení klíče. Také umístíte hello externí klíč (.bek soubor) na externím disku hello nebo svazek. Po dalším restartování hello je povolené šifrování na hello systémový/spouštěcí svazek.

    manage-bde -on %systemdrive% -sk [ExternalDriveOrVolume]
    reboot

> [!NOTE]
> Připravte hello virtuálních počítačů s samostatné dat nebo prostředků virtuálního pevného disku pro získání hello externího klíče pomocí nástroje BitLocker.

#### <a name="encrypting-an-os-drive-on-a-running-linux-vm"></a>Šifrování jednotce operačního systému na spuštěný virtuální počítač s Linuxem
Šifrování jednotky operačního systému na spuštěný virtuální počítač Linux je podporován v následujících distribuce hello:

* RHEL 7.2
* CentOS 7.2
* Ubuntu 16.04

##### <a name="prerequisites-for-os-disk-encryption"></a>Požadavky pro šifrování disku operačního systému

* Hello virtuálního počítače musí být vytvořený z hello Marketplace image ve službě Správce prostředků Azure.
* Virtuální počítač Azure s minimálně 4 GB paměti RAM (doporučená velikost je 7 GB).
* (Pro RHEL a CentOS) Zakážete SELinux. toodisable SELinux, najdete v části "4.4.2. Zakázání SELinux"v hello [Průvodce SELinux uživatele a správce](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/SELinux_Users_and_Administrators_Guide/sect-Security-Enhanced_Linux-Working_with_SELinux-Changing_SELinux_Modes.html#sect-Security-Enhanced_Linux-Enabling_and_Disabling_SELinux-Disabling_SELinux) na hello virtuálních počítačů.
* Po zakázání SELinux restartování aspoň jednou hello virtuálních počítačů.

##### <a name="steps"></a>Kroky
1. Vytvoření virtuálního počítače pomocí jedné z distribuce hello zadali dřív.

 7.2 CentOS je podporováno šifrování disku operačního systému přes speciální bitové kopie. toouse to obrázku, zadejte "7.2n" jako hello SKU, když vytvoříte hello virtuálních počítačů:
 ```
    Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName "OpenLogic" -Offer "CentOS" -Skus "7.2n" -Version "latest"
 ```
2. Nakonfigurujte hello virtuálního počítače podle tooyour potřebám. Chcete-li tooencrypt všech jednotkách hello (operační systém + data), hello datové jednotky potřebovat toobe zadaný a připojit z /etc/fstab.

 > [!NOTE]
 > Použít UUID =... toospecify datových jednotkách v fstab/etc/místo zadání názvu zařízení bloku hello (například/dev/sdb1). Během šifrování se změny pořadí hello jednotek v hello virtuálních počítačů. Pokud virtuálního počítače závisí na konkrétní pořadí blokovat zařízení, se nezdaří, toomount je po dokončení šifrování.

3. Odhlásit z relace SSH hello.

4. tooencrypt hello operačního systému, zadejte volumeType jako **všechny** nebo **OS** když jste [povolit šifrování](#enable-encryption-on-existing-or-running-iaas-linux-vm-in-azure).

 > [!NOTE]
 > Všechny procesy uživatelského prostoru, které nejsou spuštěny jako `systemd` služby by měl být ukončen s `SIGKILL`. Restartování hello virtuálních počítačů. Pokud povolíte šifrování disku operačního systému na spuštěný virtuální počítač, naplánujte na výpadek virtuálních počítačů.

5. Pravidelně sledovat průběh hello šifrování podle pokynů hello v hello [další části](#monitoring-os-encryption-progress).

6. Po Get-AzureRmVmDiskEncryptionStatus zobrazuje "VMRestartPending", restartujte virtuální počítač po přihlášení tooit nebo pomocí portálu hello, prostředí PowerShell nebo rozhraní příkazového řádku.
    ```
    C:\> Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $ResourceGroupName -VMName $VMName
    -ExtensionName $ExtensionName

    OsVolumeEncrypted          : VMRestartPending
    DataVolumesEncrypted       : NotMounted
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : OS disk successfully encrypted, reboot hello VM
    ```
Než restartujete, doporučujeme, abyste uložili [spouštění diagnostiky](https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/) z hello virtuálních počítačů.

#### <a name="monitoring-os-encryption-progress"></a>Sledování průběhu šifrování operačního systému
Můžete sledovat průběh šifrování OS třemi způsoby:

* Použití hello `Get-AzureRmVmDiskEncryptionStatus` rutiny a zkontrolovat hello ProgressMessage pole:
    ```
    OsVolumeEncrypted          : EncryptionInProgress
    DataVolumesEncrypted       : NotMounted
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : OS disk encryption started
    ```
 Po hello virtuálního počítače dosáhne "Spuštění šifrování disku operačního systému", jak dlouho trvá asi 40 minutách too50 na storage úrovně Premium zálohovaný virtuálních počítačů.

 Z důvodu [vydání #388](https://github.com/Azure/WALinuxAgent/issues/388) v WALinuxAgent, `OsVolumeEncrypted` a `DataVolumesEncrypted` zobrazují jako `Unknown` v některých distribucích. S WALinuxAgent verze 2.1.5 a novější, se tento problém vyřešen automaticky. Pokud se zobrazí `Unknown` ve výstupu hello si můžete ověřit stav šifrování disku pomocí hello Průzkumníka prostředků Azure.

 Přejděte příliš[Průzkumníka prostředků Azure](https://resources.azure.com/)a potom rozbalte tuto hierarchii panelu hello výběr na levé straně:

 ~~~~
 |-- subscriptions
     |-- [Your subscription]
          |-- resourceGroups
               |-- [Your resource group]
                    |-- providers
                         |-- Microsoft.Compute
                              |-- virtualMachines
                                   |-- [Your virtual machine]
                                        |-- InstanceView
~~~~                

 V hello InstanceView posuňte se dolů toosee hello stav šifrování jednotky.

 ![Zobrazení Instance virtuálního počítače](./media/azure-security-disk-encryption/vm-instanceview.png)

* Podívejte se na [spouštění diagnostiky](https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/). Zprávy ze hello ADE rozšíření by měla obsahovat předponu s `[AzureDiskEncryption]`.

* Přihlaste se toohello virtuálních počítačů pomocí protokolu SSH a získat hello rozšíření protokolu z:

    /var/log/Azure/Microsoft.Azure.Security.AzureDiskEncryptionForLinux

 Doporučujeme vám, že v toohello virtuálních počítačů nepodepíšete, když probíhá šifrování operačního systému. Kopírovat protokoly hello jenom v případě, že hello další dvě metody se nezdařilo.

#### <a name="prepare-a-pre-encrypted-linux-vhd"></a>Příprava předem šifrované Linux virtuální pevný disk
##### <a name="ubuntu-16"></a>Ubuntu 16
Nakonfigurujte šifrování během instalace distribučního hello pomocí hello následující:

1. Vyberte **konfigurovat šifrované svazky** při oddílu hello disky.

 ![Instalační program Ubuntu 16.04](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig1.png)

2. Vytvořte samostatný spouštěcí jednotka, která nesmí být zašifrovaná. Šifrování kořenové jednotce.

 ![Instalační program Ubuntu 16.04](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig2.png)

3. Zadejte přístupové heslo. Toto je heslo hello odeslat toohello trezoru klíčů.

 ![Instalační program Ubuntu 16.04](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig3.png)

4. Dokončete vytváření oddílů.

 ![Instalační program Ubuntu 16.04](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig4.png)

5. Když spustíte hello virtuálních počítačů a vyzváni k přístupové heslo, použijte hello přístupové heslo, které jste zadali v kroku 3.

 ![Instalační program Ubuntu 16.04](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig5.png)

6. Příprava hello virtuálního počítače pro odesílání do Azure pomocí [tyto pokyny](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-create-upload-ubuntu/). Ještě nespouštějte hello poslední krok (hello zrušení zřízení virtuálního počítače).

Nakonfigurujte toowork šifrování s Azure pomocí hello následující:

1. Vytvořte soubor pod /usr/local/sbin/azure_crypt_key.sh, s obsahem hello v hello následující skript. Věnujte pozornost toohello KeyFileName, protože je název souboru hello přístupové heslo používané Azure.

    ```
    #!/bin/sh
    MountPoint=/tmp-keydisk-mount
    KeyFileName=LinuxPassPhraseFileName
    echo "Trying tooget hello key from disks ..." >&2
    mkdir -p $MountPoint
    modprobe vfat >/dev/null 2>&1
    modprobe ntfs >/dev/null 2>&1
    sleep 2
    OPENED=0
    cd /sys/block
    for DEV in sd*; do

        echo "> Trying device: $DEV ..." >&2
        mount -t vfat -r /dev/${DEV}1 $MountPoint >/dev/null||
        mount -t ntfs -r /dev/${DEV}1 $MountPoint >/dev/null
        if [ -f $MountPoint/$KeyFileName ]; then
                cat $MountPoint/$KeyFileName
                umount $MountPoint 2>/dev/null
                OPENED=1
                break
        fi
        umount $MountPoint 2>/dev/null
    done

      if [ $OPENED -eq 0 ]; then
        echo "FAILED toofind suitable passphrase file ..." >&2
        echo -n "Try tooenter your password: " >&2
        read -s -r A </dev/console
        echo -n "$A"
     else
        echo "Success loading keyfile!" >&2
    fi
```

2. Změna konfigurace crypt hello v */etc/crypttab*. Mělo by to vypadat takto:
 ```
    xxx_crypt uuid=xxxxxxxxxxxxxxxxxxxxx none luks,discard,keyscript=/usr/local/sbin/azure_crypt_key.sh
    ```

3. Pokud upravujete *azure_crypt_key.sh* ve Windows a zkopíruje ho tooLinux, spusťte `dos2unix /usr/local/sbin/azure_crypt_key.sh`.

4. Přidejte skript toohello spustitelného souboru oprávnění:
 ```
    chmod +x /usr/local/sbin/azure_crypt_key.sh
 ```
5. Upravit */etc/initramfs-tools/modules* připojením řádky:
 ```
    vfat
    ntfs
    nls_cp437
    nls_utf8
    nls_iso8859-1
```
6. Spustit `update-initramfs -u -k all` tooupdate hello initramfs toomake hello `keyscript` vstoupí v platnost.

7. Nyní můžete zrušení zřízení hello virtuálních počítačů.

 ![Instalační program Ubuntu 16.04](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig6.png)

8. Pokračovat dalším krokem toohello a [nahrát svůj disk VHD](#upload-encrypted-vhd-to-an-azure-storage-account) do Azure.

##### <a name="opensuse-132"></a>openSUSE 13.2
šifrování tooconfigure během instalace distribučního hello hello následující:
1. Pokud jste oddílu hello disky, vyberte **šifrování svazku skupiny**a pak zadejte heslo. Toto je heslo hello trezoru klíčů tooyour odešlete.

 ![Instalační program openSUSE 13.2](./media/azure-security-disk-encryption/opensuse-encrypt-fig1.png)

2. Spouštěcí hello virtuálního počítače pomocí hesla.

 ![Instalační program openSUSE 13.2](./media/azure-security-disk-encryption/opensuse-encrypt-fig2.png)

3. Příprava hello virtuálního počítače pro odesílání tooAzure podle pokynů hello v [Příprava virtuálního počítače, SLES nebo openSUSE pro Azure](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-suse-create-upload-vhd/#prepare-opensuse-131). Ještě nespouštějte hello poslední krok (hello zrušení zřízení virtuálního počítače).

šifrování toowork tooconfigure s Azure, hello následující:
1. Upravit hello /etc/dracut.conf a přidejte následující řádek hello:
    ```
    add_drivers+=" vfat ntfs nls_cp437 nls_iso8859-1"
    ```
2. Komentář tyto řádky hello konec hello souboru /usr/lib/dracut/modules.d/90crypt/module-setup.sh:
 ```
    #        inst_multiple -o \
    #        $systemdutildir/system-generators/systemd-cryptsetup-generator \
    #        $systemdutildir/systemd-cryptsetup \
    #        $systemdsystemunitdir/systemd-ask-password-console.path \
    #        $systemdsystemunitdir/systemd-ask-password-console.service \
    #        $systemdsystemunitdir/cryptsetup.target \
    #        $systemdsystemunitdir/sysinit.target.wants/cryptsetup.target \
    #        systemd-ask-password systemd-tty-ask-password-agent
    #        inst_script "$moddir"/crypt-run-generator.sh /sbin/crypt-run-generator
 ```

3. Připojte hello následující řádek na začátku hello /usr/lib/dracut/modules.d/90crypt/parse-crypt.sh souboru hello:
 ```
    DRACUT_SYSTEMD=0
 ```
A změňte všechny výskyty:
 ```
    if [ -z "$DRACUT_SYSTEMD" ]; then
 ```
na:
```
    if [ 1 ]; then
```
4. Upravit /usr/lib/dracut/modules.d/90crypt/cryptroot-ask.sh a připojit ho příliš "# otevřete LUKS zařízení":

    ```
    MountPoint=/tmp-keydisk-mount
    KeyFileName=LinuxPassPhraseFileName
    echo "Trying tooget hello key from disks ..." >&2
    mkdir -p $MountPoint >&2
    modprobe vfat >/dev/null >&2
    modprobe ntfs >/dev/null >&2
    for SFS in /dev/sd*; do
    echo "> Trying device:$SFS..." >&2
    mount ${SFS}1 $MountPoint -t vfat -r >&2 ||
    mount ${SFS}1 $MountPoint -t ntfs -r >&2
    if [ -f $MountPoint/$KeyFileName ]; then
        echo "> keyfile got..." >&2
        cp $MountPoint/$KeyFileName /tmp-keyfile >&2
        luksfile=/tmp-keyfile
        umount $MountPoint >&2
        break
    fi
    done
    ```
5. Spustit `/usr/sbin/dracut -f -v` tooupdate hello initrd.

6. Teď můžete zrušení zřízení hello virtuálních počítačů a [nahrát svůj disk VHD](#upload-encrypted-vhd-to-an-azure-storage-account) do Azure.

##### <a name="centos-7"></a>CentOS 7
šifrování tooconfigure během instalace distribučního hello hello následující:
1. Vyberte **šifrovat data** při oddílu disky.

 ![Instalační program centOS 7](./media/azure-security-disk-encryption/centos-encrypt-fig1.png)

2. Zajistěte, aby **šifrovat** je vybraná pro kořenový oddíl.

 ![Instalační program centOS 7](./media/azure-security-disk-encryption/centos-encrypt-fig2.png)

3. Zadejte přístupové heslo. Toto je heslo hello trezoru klíčů tooyour odešlete.

 ![Instalační program centOS 7](./media/azure-security-disk-encryption/centos-encrypt-fig3.png)

4. Když spustíte hello virtuálních počítačů a vyzváni k přístupové heslo, použijte hello přístupové heslo, které jste zadali v kroku 3.

 ![Instalační program centOS 7](./media/azure-security-disk-encryption/centos-encrypt-fig4.png)

5. Příprava hello virtuálního počítače pro odesílání do Azure podle pokynů "CentOS 7.0 +" hello v [Příprava virtuálního počítače, na základě CentOS pro Azure](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-create-upload-centos/#centos-70). Ještě nespouštějte hello poslední krok (hello zrušení zřízení virtuálního počítače).

6. Teď můžete zrušení zřízení hello virtuálních počítačů a [nahrát svůj disk VHD](#upload-encrypted-vhd-to-an-azure-storage-account) do Azure.

šifrování toowork tooconfigure s Azure, hello následující:

1. Upravit hello /etc/dracut.conf a přidejte následující řádek hello:
    ```
    add_drivers+=" vfat ntfs nls_cp437 nls_iso8859-1"
    ```

2. Komentář tyto řádky hello konec hello souboru /usr/lib/dracut/modules.d/90crypt/module-setup.sh:
```
    #        inst_multiple -o \
    #        $systemdutildir/system-generators/systemd-cryptsetup-generator \
    #        $systemdutildir/systemd-cryptsetup \
    #        $systemdsystemunitdir/systemd-ask-password-console.path \
    #        $systemdsystemunitdir/systemd-ask-password-console.service \
    #        $systemdsystemunitdir/cryptsetup.target \
    #        $systemdsystemunitdir/sysinit.target.wants/cryptsetup.target \
    #        systemd-ask-password systemd-tty-ask-password-agent
    #        inst_script "$moddir"/crypt-run-generator.sh /sbin/crypt-run-generator
```

3. Připojte hello následující řádek na začátku hello /usr/lib/dracut/modules.d/90crypt/parse-crypt.sh souboru hello:
```
    DRACUT_SYSTEMD=0
```
A změňte všechny výskyty:
```
    if [ -z "$DRACUT_SYSTEMD" ]; then
```
na
```
    if [ 1 ]; then
```
4. Upravit /usr/lib/dracut/modules.d/90crypt/cryptroot-ask.sh a připojit toto po hello "# otevřete LUKS zařízení":
    ```
    MountPoint=/tmp-keydisk-mount
    KeyFileName=LinuxPassPhraseFileName
    echo "Trying tooget hello key from disks ..." >&2
    mkdir -p $MountPoint >&2
    modprobe vfat >/dev/null >&2
    modprobe ntfs >/dev/null >&2
    for SFS in /dev/sd*; do
    echo "> Trying device:$SFS..." >&2
    mount ${SFS}1 $MountPoint -t vfat -r >&2 ||
    mount ${SFS}1 $MountPoint -t ntfs -r >&2
    if [ -f $MountPoint/$KeyFileName ]; then
        echo "> keyfile got..." >&2
        cp $MountPoint/$KeyFileName /tmp-keyfile >&2
        luksfile=/tmp-keyfile
        umount $MountPoint >&2
        break
    fi
    done
    ```    
5. Spustit hello "/ usr/sbin/dracut - f - v" tooupdate hello initrd.

![Instalační program centOS 7](./media/azure-security-disk-encryption/centos-encrypt-fig5.png)

### <a name="upload-encrypted-vhd-tooan-azure-storage-account"></a>Nahrát šifrované účtu úložiště Azure tooan virtuálního pevného disku
Povolíte šifrování nástrojem BitLocker nebo DM-Crypt šifrování, šifrují hello místní účet úložiště tooyour toobe nahrán potřebám virtuálního pevného disku.

    Add-AzureRmVhd [-Destination] <Uri> [-LocalFilePath] <FileInfo> [[-NumberOfUploaderThreads] <Int32> ] [[-BaseImageUriToPatch] <Uri> ] [[-OverWrite]] [ <CommonParameters>]

### <a name="upload-hello-disk-encryption-secret-for-hello-pre-encrypted-vm-tooyour-key-vault"></a>Nahrát hello šifrování disku tajný klíč pro hello předem šifrované virtuálních počítačů tooyour trezoru klíčů
tajný klíč Hello šifrování disku, který jste získali dříve musí být odeslán jako tajný klíč v trezoru klíčů. Trezor klíčů Hello musí toohave šifrování disku a oprávnění, které jsou povolené pro vašeho klienta Azure AD.

    $AadClientId = "YourAADClientId"
    $AadClientSecret = "YourAADClientSecret"

    $key vault = New-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -Location $Location

    Set-AzureRmKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -ServicePrincipalName $AadClientId -PermissionsToKeys all -PermissionsToSecrets all
    Set-AzureRmKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -EnabledForDiskEncryption


#### <a name="disk-encryption-secret-not-encrypted-with-a-kek"></a>Disk encryption tajný klíč není šifrován KEK
tooset až hello tajný klíč v trezoru klíčů, použijte [Set-AzureKeyVaultSecret](/powershell/module/azurerm.keyvault/set-azurekeyvaultsecret). Pokud máte virtuální počítač Windows hello bek soubor je kódovaná jako řetězec base64 a pak odeslat tooyour trezoru klíčů pomocí hello `Set-AzureKeyVaultSecret` rutiny. Pro systémy Linux hello heslo je kódovaná jako řetězec base64 a pak odeslat toohello trezoru klíčů. Kromě toho Ujistěte se, že tento hello následující značky se nastavují při vytváření hello tajný klíč v trezoru klíčů hello.

    # This is hello passphrase that was provided for encryption during hello distribution installation
    $passphrase = "contoso-password"

    $tags = @{"DiskEncryptionKeyEncryptionAlgorithm" = "RSA-OAEP"; "DiskEncryptionKeyFileName" = "LinuxPassPhraseFileName"}
    $secretName = [guid]::NewGuid().ToString()
    $secretValue = [Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes($passphrase))
    $secureSecretValue = ConvertTo-SecureString $secretValue -AsPlainText -Force

    $secret = Set-AzureKeyVaultSecret -VaultName $KeyVaultName -Name $secretName -SecretValue $secureSecretValue -tags $tags
    $secretUrl = $secret.Id

Použití hello `$secretUrl` v hello další krok pro [připojení disku hello operačního systému bez použití KEK](#without-using-a-kek).

#### <a name="disk-encryption-secret-encrypted-with-a-kek"></a>Tajný klíč šifrování disku šifrován KEK
Před nahráním hello toohello tajný klíč trezoru, můžete volitelně šifrováním pomocí klíče šifrovací klíč. Použití hello wrap [rozhraní API](https://msdn.microsoft.com/library/azure/dn878066.aspx) toofirst šifrování tajného klíče hello pomocí hello klíče šifrovacího klíče. Hello výstup této operace zalamování je řetězec ve formátu base64 kódovaná adresou URL, které potom můžete nahrát jako tajný klíč pomocí hello [ `Set-AzureKeyVaultSecret` ](/powershell/module/azurerm.keyvault/set-azurekeyvaultsecret) rutiny.

    # This is hello passphrase that was provided for encryption during hello distribution installation
    $passphrase = "contoso-password"

    Add-AzureKeyVaultKey -VaultName $KeyVaultName -Name "keyencryptionkey" -Destination Software
    $KeyEncryptionKey = Get-AzureKeyVaultKey -VaultName $KeyVault.OriginalVault.Name -Name "keyencryptionkey"

    $apiversion = "2015-06-01"

    ##############################
    # Get Auth URI
    ##############################

    $uri = $KeyVault.VaultUri + "/keys"
    $headers = @{}

    $response = try { Invoke-RestMethod -Method GET -Uri $uri -Headers $headers } catch { $_.Exception.Response }

    $authHeader = $response.Headers["www-authenticate"]
    $authUri = [regex]::match($authHeader, 'authorization="(.*?)"').Groups[1].Value

    Write-Host "Got Auth URI successfully"

    ##############################
    # Get Auth Token
    ##############################

    $uri = $authUri + "/oauth2/token"
    $body = "grant_type=client_credentials"
    $body += "&client_id=" + $AadClientId
    $body += "&client_secret=" + [Uri]::EscapeDataString($AadClientSecret)
    $body += "&resource=" + [Uri]::EscapeDataString("https://vault.azure.net")
    $headers = @{}

    $response = Invoke-RestMethod -Method POST -Uri $uri -Headers $headers -Body $body

    $access_token = $response.access_token

    Write-Host "Got Auth Token successfully"

    ##############################
    # Get KEK info
    ##############################

    $uri = $KeyEncryptionKey.Id + "?api-version=" + $apiversion
    $headers = @{"Authorization" = "Bearer " + $access_token}

    $response = Invoke-RestMethod -Method GET -Uri $uri -Headers $headers

    $keyid = $response.key.kid

    Write-Host "Got KEK info successfully"

    ##############################
    # Encrypt passphrase using KEK
    ##############################

    $passphraseB64 = [Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes($Passphrase))
    $uri = $keyid + "/encrypt?api-version=" + $apiversion
    $headers = @{"Authorization" = "Bearer " + $access_token; "Content-Type" = "application/json"}
    $bodyObj = @{"alg" = "RSA-OAEP"; "value" = $passphraseB64}
    $body = $bodyObj | ConvertTo-Json

    $response = Invoke-RestMethod -Method POST -Uri $uri -Headers $headers -Body $body

    $wrappedSecret = $response.value

    Write-Host "Encrypted passphrase successfully"

    ##############################
    # Store secret
    ##############################

    $secretName = [guid]::NewGuid().ToString()
    $uri = $KeyVault.VaultUri + "/secrets/" + $secretName + "?api-version=" + $apiversion
    $secretAttributes = @{"enabled" = $true}
    $secretTags = @{"DiskEncryptionKeyEncryptionAlgorithm" = "RSA-OAEP"; "DiskEncryptionKeyFileName" = "LinuxPassPhraseFileName"}
    $headers = @{"Authorization" = "Bearer " + $access_token; "Content-Type" = "application/json"}
    $bodyObj = @{"value" = $wrappedSecret; "attributes" = $secretAttributes; "tags" = $secretTags}
    $body = $bodyObj | ConvertTo-Json

    $response = Invoke-RestMethod -Method PUT -Uri $uri -Headers $headers -Body $body

    Write-Host "Stored secret successfully"

    $secretUrl = $response.id

Použití `$KeyEncryptionKey` a `$secretUrl` v hello další krok pro [připojení disku hello operačního systému pomocí KEK](#using-a-kek).

### <a name="specify-a-secret-url-when-you-attach-an-os-disk"></a>Zadejte tajný adresu URL, když připojíte disku operačního systému
#### <a name="without-using-a-kek"></a>Bez použití KEK
Při připojování disku hello operačního systému, je nutné toopass `$secretUrl`. Adresa URL Hello byl vygenerován v části "šifrování disku tajný klíč není šifrován KEK" hello.

    Set-AzureRmVMOSDisk `
            -VM $VirtualMachine `
            -Name $OSDiskName `
            -SourceImageUri $VhdUri `
            -VhdUri $OSDiskUri `
            -Linux `
            -CreateOption FromImage `
            -DiskEncryptionKeyVaultId $KeyVault.ResourceId `
            -DiskEncryptionKeyUrl $SecretUrl

#### <a name="using-a-kek"></a>Použití KEK
Pokud se připojíte disk hello operačního systému, předat `$KeyEncryptionKey` a `$secretUrl`. Adresa URL Hello byl vygenerován v části "šifrování disku tajný klíč není šifrován KEK" hello.

    Set-AzureRmVMOSDisk `
            -VM $VirtualMachine `
            -Name $OSDiskName `
            -SourceImageUri $CopiedTemplateBlobUri `
            -VhdUri $OSDiskUri `
            -Linux `
            -CreateOption FromImage `
            -DiskEncryptionKeyVaultId $KeyVault.ResourceId `
            -DiskEncryptionKeyUrl $SecretUrl `
            -KeyEncryptionKeyVaultId $KeyVault.ResourceId `
            -KeyEncryptionKeyURL $KeyEncryptionKey.Id

## <a name="download-this-guide"></a>Stáhněte si tento průvodce
Tento průvodce si můžete stáhnout z hello [Galerii TechNet](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0).

## <a name="for-more-information"></a>Další informace
[Prozkoumejte Azure Disk Encryption pomocí Azure PowerShell – část 1](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/16/explore-azure-disk-encryption-with-azure-powershell.aspx?wa=wsignin1.0)  
[Prozkoumejte Azure Disk Encryption pomocí prostředí Azure PowerShell - část 2](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx)
