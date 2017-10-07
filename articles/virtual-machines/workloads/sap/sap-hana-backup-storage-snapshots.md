---
title: "zálohování HANA Azure aaaSAP podle úložiště snímků | Microsoft Docs"
description: "Existují dvě hlavní zálohování možnosti pro SAP HANA na virtuálních počítačích Azure, tento článek se zabývá SAP HANA zálohování podle úložiště snímků"
services: virtual-machines-linux
documentationcenter: 
author: hermanndms
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ums.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 3/13/2017
ms.author: rclaus
ms.openlocfilehash: 32bb80f5a928a2cf63699bfe4f4cf5bbad81e6fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sap-hana-backup-based-on-storage-snapshots"></a>Zálohování SAP HANA založené na snímcích úložiště

## <a name="introduction"></a>Úvod

Toto je část třemi částmi řady související články v záloze SAP HANA. [Příručce zálohování pro SAP HANA ve virtuálních počítačích Azure](sap-hana-backup-guide.md) poskytuje přehled a informace o začátcích, a [SAP HANA Azure Backup na úrovni souborů](sap-hana-backup-file-level.md) zahrnuje hello možnost zálohování na základě souborů.

Při použití funkce zálohování virtuálních počítačů pro jednoduchou vše v jednom ukázku systém, jeden zvažte provedení zálohy virtuálního počítače místo Správa HANA zálohy v hello úroveň operačního systému. Alternativou je tootake objektů blob v Azure snímky toocreate kopie jednotlivé virtuální disky, které jsou připojené tooa virtuálního počítače a udržovat hello HANA datových souborů. Ale kritický bod konzistence aplikace při vytváření snímku zálohy nebo disk virtuálního počítače při hello systém zapnutý a spouštění. V tématu _konzistenci dat SAP HANA při pořizování snímků úložiště_ v souvisejícím článku hello [zálohování Průvodce pro SAP HANA ve virtuálních počítačích Azure](sap-hana-backup-guide.md). SAP HANA obsahuje funkce, která podporuje tyto typy úložiště snímků.

## <a name="sap-hana-snapshots"></a>Snímky SAP HANA

V SAP HANA, který podporuje pořizování snímku úložiště je funkce. Od prosince 2016, je však omezení systémy toosingle kontejneru. Víceklientské kontejneru konfigurace nepodporují tento druh snímek databáze (viz [vytvořit úložiště snímků (SAP HANA Studio)](https://help.sap.com/saphelp_hanaplatform/helpdata/en/a0/3f8f08501e44d89115db3c5aa08e3f/content.htm)).

Funguje takto:

- Příprava pro vytvoření snímku úložiště pomocí inicializace snímku hello SAP HANA
- Spustit hello úložiště snímků (objektů blob v Azure vytvořit snímek, například)
- Potvrďte snímku hello SAP HANA

![Tento snímek obrazovky ukazuje, že SAP HANA data snímku lze vytvořit pomocí příkazu jazyka SQL](media/sap-hana-backup-storage-snapshots/image011.png)

Tento snímek obrazovky ukazuje, že SAP HANA data snímku lze vytvořit pomocí příkazu jazyka SQL.

![Hello snímku pak se zobrazí také v hello zálohování katalogu v nástroji SAP HANA Studio](media/sap-hana-backup-storage-snapshots/image012.png)

Hello snímku pak se zobrazí také v hello zálohování katalogu v nástroji SAP HANA Studio.

![Na disku hello snímku se zobrazí v adresáři data hello SAP HANA](media/sap-hana-backup-storage-snapshots/image013.png)

Na disku hello snímku se objeví v adresáři data hello SAP HANA.

Jeden má tooensure, který před spuštěním hello úložiště snímků během SAP HANA režim přípravy snímku hello také zaručeně hello konzistence systému souborů. V tématu _konzistenci dat SAP HANA při pořizování snímků úložiště_ v souvisejícím článku hello [zálohování Průvodce pro SAP HANA ve virtuálních počítačích Azure](sap-hana-backup-guide.md).

Po dokončení hello úložiště snímků je důležité tooconfirm hello SAP HANA snímku. Neexistuje odpovídající toorun příkaz SQL: zavřít SNÍMKU dat zálohy (v tématu [dat zavřít SNÍMKU příkaz BACKUP (zálohování a obnovení)](https://help.sap.com/saphelp_hanaplatform/helpdata/en/c3/9739966f7f4bd5818769ad4ce6a7f8/content.htm)).

> [!IMPORTANT]
> Potvrďte hello HANA snímku. Kvůli příliš&quot;kopie při zápisu,&quot; SAP HANA může vyžadovat další místo na disku v Příprava snímku režimu, a není možné toostart nových záloh. dokud hello SAP HANA snímku je potvrzen.

## <a name="hana-vm-backup-via-azure-backup-service"></a>Zálohování virtuálních počítačů HANA prostřednictvím služby zálohování Azure

Od prosince 2016 hello agenta zálohování hello služba Azure Backup není k dispozici pro virtuální počítače s Linuxem. toomake použití zálohování Azure na úrovni souboru nebo adresáře hello se jeden by zkopírujte tooa SAP HANA záložní soubory virtuálního počítače s Windows a pak použijte agenta zálohování hello. Pouze úplná záloha virtuálního počítače s Linuxem, jinak je možné prostřednictvím hello služba Azure Backup. V tématu [přehled hello funkce ve službě Azure Backup](../../../backup/backup-introduction-to-azure-backup.md) toofind Další.

Hello služby Azure Backup nabízí tooback možnost až a obnovte virtuální počítač. Další informace o této služby a jak to funguje najdete v článku hello [plánování vaší infrastruktury zálohování virtuálních počítačů v Azure](../../../backup/backup-azure-vms-introduction.md).

Existují dvě důležité aspekty podle článku toothat:

_&quot;Pro virtuální počítače s Linuxem je možné, pouze konzistentními soubory zálohy, protože Linux nemá tooVSS ekvivalentní platformy.&quot;_

_&quot;Aplikace potřebují tooimplement vlastní &quot;opravu&quot; mechanismus na hello obnovit data.&quot;_

Proto má jeden toomake se, že SAP HANA v konzistentním stavu na disku při spuštění zálohování hello. V tématu _SAP HANA snímky_ popsané výše v dokumentu hello. Ale existuje potenciální problém při SAP HANA zůstane v tomto režimu přípravy snímku. V tématu [vytvořit úložiště snímků (SAP HANA Studio)](https://help.sap.com/saphelp_hanaplatform/helpdata/en/a0/3f8f08501e44d89115db3c5aa08e3f/content.htm) Další informace.

Tento článek stavy:

_&quot;Je důrazně doporučujeme tooconfirm nebo zrušte snímku úložiště co nejdříve po jeho vytvoření. Při hello úložiště snímků připraveném nebo vytvoření hello snímek příslušných dat nereaguje. Při současném zachování ukotvené hello snímek příslušných dat, může stále provádět změny v databázi hello. Tyto změny nezpůsobí hello pozastaveny toobe snímek příslušných dat změnit. Místo toho hello změny jsou zapsány toopositions ve hello datové oblasti, které jsou oddělené od hello úložiště snímků. Změny se zapisují také toohello protokolu. Ale hello delší hello snímku relevantní data se ukládají ukotvené, hello další hello, které můžou růst datový svazek.&quot;_

Zálohování Azure má na starosti hello konzistence systému souborů prostřednictvím rozšíření virtuálního počítače Azure. Tato rozšíření nejsou k dispozici samostatná a fungovat pouze v kombinaci s služby zálohování Azure. Je však stále požadavek toomanage SAP HANA snímku tooguarantee aplikace konzistence.

Zálohování Azure má dva hlavní fáze:

- Pořízení snímku
- Přenos dat toovault

Po dokončení fáze služby Azure Backup hello pořízení snímku apod. může potvrďte hello SAP HANA snímku. Může to trvat několik minut toosee v hello portálu Azure.

![Následující obrázek ukazuje součástí hello úlohu zálohování seznam služby Azure Backup](media/sap-hana-backup-storage-snapshots/image014.png)

Následující obrázek ukazuje součástí hello úlohu zálohování seznam služby Azure Backup, která byla použité tooback až hello HANA testovací virtuální počítač.

![Podrobnosti úlohy hello tooshow, klikněte na úlohu zálohování hello v hello portálu Azure](media/sap-hana-backup-storage-snapshots/image015.png)

Podrobnosti úlohy hello tooshow, klikněte na úlohu zálohování hello v hello portálu Azure. Zde jeden můžete zobrazit hello dvě fáze. Může trvat několik minut, dokud zobrazuje hello snímku fáze jako dokončenou. Většinu hello doba strávená ve fázi přenos dat hello.

## <a name="hana-vm-backup-automation-via-azure-backup-service"></a>Zálohování automatizace HANA virtuálních počítačů pomocí služby zálohování Azure

Hello SAP HANA snímku jeden ručně může potvrdit, jakmile hello Azure Backup snímku fáze je hotová, jak je popsáno výše, ale je užitečné tooconsider automatizace, protože správce nemusí monitorování hello úlohu zálohování seznamu v hello portál Azure.

Následuje vysvětlení, jak je možné dosáhnout pomocí rutin prostředí Azure PowerShell.

![Služby Azure Backup byl vytvořen s hello název hana--úložiště záloh](media/sap-hana-backup-storage-snapshots/image016.png)

Služby Azure Backup byl vytvořen s názvem hello &quot;hana-zálohování-trezoru.&quot; hello PS příkaz **Get AzureRmRecoveryServicesVault-název úložiště záloh hana** načte hello odpovídající objekt. Tento objekt je pak použít tooset hello zálohování kontextu, jak je vidět na následující obrázek hello.

![Jeden můžete zkontrolovat pro úlohu zálohování hello aktuálně probíhá](media/sap-hana-backup-storage-snapshots/image017.png)

Po nastavení hello správný kontext jednu úlohu zálohování hello právě probíhající kontrolovat a potom vyhledejte jeho podrobnosti úlohy. Hello dílčí úlohy seznam obsahuje, pokud je již dokončeno hello snímku fázi hello Azure úlohy zálohování:

```
$ars = Get-AzureRmRecoveryServicesVault -Name hana-backup-vault
Set-AzureRmRecoveryServicesVaultContext -Vault $ars
$jid = Get-AzureRmRecoveryServicesBackupJob -Status InProgress | select -ExpandProperty jobid
Get-AzureRmRecoveryServicesBackupJobDetails -Jobid $jid | select -ExpandProperty subtasks
```

![Hodnota hello dotazování ve smyčce, dokud se změní tooCompleted](media/sap-hana-backup-storage-snapshots/image018.png)

Jakmile hello podrobnosti úlohy jsou uložené v proměnné, je jednoduše PS syntaxe tooget toohello první položkou pole a načíst hodnotu stavu hello. změní skriptu pro automatizaci hello toocomplete, dotazování hello hodnotu ve smyčce dokud příliš&quot;dokončeno.&quot;

```
$st = Get-AzureRmRecoveryServicesBackupJobDetails -Jobid $jid | select -ExpandProperty subtasks
$st[0] | select -ExpandProperty status
```

## <a name="hana-license-key-and-vm-restore-via-azure-backup-service"></a>Licenční klíč HANA a virtuální počítač obnovit pomocí služby zálohování Azure

Hello služby Azure Backup je navrženou toocreate nový virtuální počítač během obnovení. Neexistuje žádný plán teď správné toodo &quot;na místě&quot; obnovení existující virtuální počítač Azure.

![Následující obrázek ukazuje možnost obnovení hello hello služby Azure v hello portálu Azure](media/sap-hana-backup-storage-snapshots/image019.png)

Následující obrázek ukazuje možnost obnovení hello hello služby Azure v hello portálu Azure. Jeden můžete vybrat mezi vytvoření virtuálního počítače během obnovení nebo při obnovování hello disky. Po obnovení hello disků, je stále nutné toocreate nový virtuální počítač nad ho. Vždy, když se vytvoří nový virtuální počítač na změny, jedinečné ID virtuálního počítače Azure hello (viz [zpřístupnění a pomocí jedinečné ID virtuálního počítače Azure](https://azure.microsoft.com/blog/accessing-and-using-azure-vm-unique-id/)).

![Následující obrázek ukazuje jedinečné ID virtuálního počítače Azure hello před a po obnovení hello prostřednictvím služby Azure Backup](media/sap-hana-backup-storage-snapshots/image020.png)

Následující obrázek ukazuje jedinečné ID virtuálního počítače Azure hello před a po obnovení hello prostřednictvím služby Azure Backup. Hello SAP hardwaru klíč, který se používá pro SAP licencování, používá tato jedinečné ID virtuálních počítačů. V důsledku toho novou licenci SAP má toobe nainstalovali až po obnovení virtuálního počítače.

Nová funkce Azure Backup se zobrazí v režimu preview během vytváření hello této příručce zálohování. To umožňuje obnovení souboru úrovně podle hello snímku virtuálního počítače, který nebyla provedena pro hello virtuálních počítačů zálohování. Tím předejdete hello nutné toodeploy nový virtuální počítač a proto hello hello jedinečné ID virtuálního počítače zůstane stejný a není třeba žádný nový licenční klíč SAP HANA. Další dokumentaci k této funkce bude poskytnuta po je plně testována.

Zálohování Azure se nakonec povolit zálohování Azure jednotlivé virtuální disky a soubory a adresáře z uvnitř hello virtuálních počítačů. Hlavní výhodou Azure Backup je jeho správu všechny zálohy hello ukládání hello zákazníka odpadne toodo ho. Pokud obnovení je nutné, bude zálohování Azure vyberte správné zálohování toouse hello.

## <a name="sap-hana-vm-backup-via-manual-disk-snapshot"></a>Zálohování virtuálních počítačů SAP HANA prostřednictvím snímku ruční disku

Místo použití hello služba Azure Backup, jeden může nakonfigurovat do jednotlivých řešení pro zálohování vytváření snímků objektu blob Azure virtuální pevné disky ručně pomocí prostředí PowerShell. V tématu [snímky použití objektů blob v prostředí PowerShell](https://blogs.msdn.microsoft.com/cie/2016/05/17/using-blob-snapshots-with-powershell/) popis kroků hello.

Poskytuje větší flexibilitu, ale nevyřeší hello problémy popsané výše v tomto dokumentu:

- Jeden stále musí Ujistěte se, že SAP HANA je v konzistentním stavu
- disk s operačním systémem Hello nelze přepsat, i když existuje hello virtuální počítač navrácený chyba oznamující, že zapůjčení. Funguje pouze po odstranění hello virtuálních počítačů, což by vedlo tooa nové jedinečné ID virtuálního počítače a hello nutné tooinstall novou licenci SAP.

![Je možné toorestore pouze hello datové disky virtuálního počítače Azure](media/sap-hana-backup-storage-snapshots/image021.png)

Je možné toorestore pouze hello datových disků z virtuálního počítače Azure, zabraňující hello problém získávání nové jedinečné ID virtuálního počítače a, proto zrušena hello SAP licencí:

- Pro hello test dva disky dat Azure byly připojené tooa virtuálních počítačů a softwaru diskového pole RAID byl definován nad je 
- Funkce snímku SAP HANA bylo potvrzeno, že SAP HANA bylo v konzistentním stavu
- Freeze – systém souborů (v tématu _konzistenci dat SAP HANA při pořizování snímků úložiště_ v souvisejícím článku hello [zálohování Průvodce pro SAP HANA ve virtuálních počítačích Azure](sap-hana-backup-guide.md))
- Objekt BLOB snímky byly odebrány z obou datových disků
- Uvolní systému souborů
- Potvrzení snímku SAP HANA
- toorestore hello datových disků, hello virtuální počítač byl vypnut a oba disky odpojit
- Po odpojení hello disky, měla přepsat s hello bývalé blob snímky
- Pak byly připojené virtuální disky hello obnovit znovu toohello virtuálních počítačů
- Po počáteční hello virtuálních počítačů, vše na hello softwaru diskového pole RAID fungovala bez problémů a byla nastavena zpět toohello blob snímku čas
- HANA byl nastaven zpět toohello HANA snímku

Pokud bylo možné tooshut dolů SAP HANA před hello blob snímky, bude postup hello méně složitých. V takovém případě jeden může přeskočit hello HANA snímku a, pokud nic jiného se děje v systému hello také přeskočit hello souboru systému ukotvit. Náročnost hello obrázek stává, když je nezbytné toodo snímky, zatímco všechno, co je online. V tématu _konzistenci dat SAP HANA při pořizování snímků úložiště_ v souvisejícím článku hello [zálohování Průvodce pro SAP HANA ve virtuálních počítačích Azure](sap-hana-backup-guide.md).

## <a name="next-steps"></a>Další kroky
* [Příručce zálohování pro SAP HANA ve virtuálních počítačích Azure](sap-hana-backup-guide.md) nabízí přehled a informace o zahájení práce.
* [Na základě SAP HANA zálohování na úrovni souborů](sap-hana-backup-file-level.md) zahrnuje hello možnost zálohování na základě souborů.
* jak tooestablish vysokou dostupnost a plán pro zotavení po havárii SAP HANA v Azure (velké instance), najdete v části toolearn [SAP HANA (velké instance) vysoké dostupnosti a zotavení po havárii v Azure](hana-overview-high-availability-disaster-recovery.md).
