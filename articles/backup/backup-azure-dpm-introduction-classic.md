---
title: "aaaBack zálohu DPM úlohy tooAzure klasického portálu | Microsoft Docs"
description: "Toobacking Úvod do serverů DPM pomocí služby zálohování Azure hello"
services: backup
documentationcenter: 
author: Nkolli1
manager: shreeshd
editor: 
keywords: "System Center Data Protection Manager, nástroje data protection manager, zálohy aplikace dpm"
ms.assetid: 8f23972b-d167-4231-b331-e198db3b18b4
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: nkolli;giridham;markgal
ms.openlocfilehash: f408957db69d45f745d5e89bd97030a341405b72
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="preparing-tooback-up-workloads-tooazure-with-dpm"></a>Příprava tooback až tooAzure úlohy s aplikací DPM
> [!div class="op_single_selector"]
> * [Azure Backup Server](backup-azure-microsoft-azure-backup.md)
> * [SCDPM](backup-azure-dpm-introduction.md)
> * [Server Azure Backup (klasické)](backup-azure-microsoft-azure-backup-classic.md)
> * [SCDPM (klasické)](backup-azure-dpm-introduction-classic.md)
>
>

Tento článek obsahuje úvod toousing Microsoft Azure Backup tooprotect vaše servery System Center Data Protection Manager (DPM) a úlohy. Načtením ji budete porozumíte:

* Jak funguje Azure zálohování serveru
* Hello požadavky tooachieve smooth zálohování prostředí
* Hello typické došlo k chybám a jak toodeal s nimi
* Podporované scénáře

System Center DPM zálohuje data souborů a aplikací. Data zálohovaná tooDPM můžete uložit na pásce, na disku, nebo zálohovat tooAzure s Microsoft Azure Backup. Aplikace DPM komunikuje s Azure Backup následujícím způsobem:

* **Aplikace DPM nasazená jako fyzický server nebo místní virtuální počítač** – Pokud je aplikace DPM nasazená jako fyzický server nebo jako místní virtuální počítač technologie Hyper-V můžete zálohovat si data tooan Azure Backup trezoru kromě toodisk a zálohování na pásku.
* **Aplikace DPM nasazená jako virtuální počítač Azure** – ze System Center 2012 R2 s aktualizací 3, DPM dá nasadit jako virtuální počítač Azure. Pokud je aplikace DPM nasazená jako virtuální počítač Azure, které můžete zálohovat data tooAzure disky připojené virtuálnímu počítači DPM Azure toohello nebo může přenést úložiště dat hello zálohováním tooan trezoru zálohování Azure.

## <a name="why-backup-your-dpm-servers"></a>Proč zálohovat servery DPM?
Příklady výhod Hello firmy pomocí služby Azure Backup k zálohování serverů aplikace DPM:

* Pro místní nasazení aplikace DPM můžete použít zálohování Azure jako tootape alternativní toolong termín nasazení.
* Pro nasazení aplikace DPM v Azure Azure Backup toooffload úložiště z disku Azure a hello vám umožní tooscale až uložením starších data v Azure Backup a nových dat na disku.

## <a name="how-does-dpm-server-backup-work"></a>Funkce Zálohování serveru aplikace DPM
tooback virtuální počítač, prvního bodu v čase snímek hello dat je potřeba. Hello Azure Backup service iniciuje hello úloha zálohování na hello naplánovaný čas a aktivační události hello rozšíření zálohování tootake snímku. Rozšíření zálohování souřadnice Hello s hello v hostovi služby Stínová kopie svazku služby tooachieve konzistence a vyvolá hello blob snímku API hello služby Azure Storage dosažení konzistence. Toto je Hotovo tooget konzistentního snímku hello disků hello virtuálního počítače, bez nutnosti tooshut ho dolů.

Co hello snímku byla přijata, hello data se přenáší přes hello Azure Backup service toohello úložiště záloh. Hello služba má na starosti identifikace a přenáší pouze bloky hello, které se změnily od poslední zálohy hello zvýšení efektivity hello zálohování úložiště a sítě. Po dokončení přenosu dat hello hello snímek odebrán a vytvoří bod obnovení. Tento bod obnovení se zobrazí v hello portál Azure classic.

> [!NOTE]
> Pro virtuální počítače s Linuxem je možné pouze soubor zálohování s konzistentními.
>
>

## <a name="prerequisites"></a>Požadavky
Příprava zálohování Azure tooback zálohovat data aplikace DPM následujícím způsobem:

1. **Vytvořte úložiště záloh**. Pokud jste nevytvořili úložiště záloh ve vašem předplatném, přečtěte si téma hello Azure portálu verzi tohoto článku - [Příprava tooback až tooAzure úlohy s aplikací DPM](backup-azure-dpm-introduction.md).

  > [!IMPORTANT]
  > Od března 2017 se už můžete trezory Backup portálu classic toocreate hello.
  > Teď můžete upgradovat vaše trezory služeb tooRecovery trezory Backup. Podrobnosti najdete v tématu hello článku [upgradu tooa trezoru zálohování trezor služeb zotavení](backup-azure-upgrade-backup-to-recovery-services.md). Společnost Microsoft doporučuje tooupgrade zálohování trezory tooRecovery trezory služeb.<br/> Po 15 říjen 2017 nemůžete použít trezory Backup toocreate prostředí PowerShell. **Do 1. listopadu 2017:**
  >- Všechny zbývající trezory Backup bude automaticky upgradovaný tooRecovery trezory služeb.
  >- Můžete nebudou moct tooaccess zálohovaných dat na portálu classic hello. Místo toho použijte hello Azure portálu tooaccess zálohovaných dat v trezory služeb zotavení.
  >

2. **Stažení přihlašovacích údajů trezoru** – v Azure Backup, jste vytvořili trezor toohello certifikát pro správu nahrávání hello.
3. **Nainstalujte hello Azure Backup Agent a zaregistrujte hello server** – z Azure Backup, nainstalujte agenta hello na každém serveru DPM a server aplikace DPM hello zaregistrovat v trezoru záloh hello.

[!INCLUDE [backup-download-credentials](../../includes/backup-download-credentials.md)]

[!INCLUDE [backup-install-agent](../../includes/backup-install-agent.md)]

## <a name="requirements-and-limitations"></a>Požadavky (a omezení)
* Aplikace DPM může být spuštěná jako fyzický server nebo virtuální počítač technologie Hyper-V nainstalovaná v System Center 2012 SP1 nebo System Center 2012 R2. Můžete také používat jako virtuální počítač Azure používá aspoň na System Center 2012 R2 s kumulativní aktualizace 3 pro DPM 2012 R2 nebo virtuálního počítače s Windows v prostředí VMWare alespoň systémem System Center 2012 R2 s kumulativní aktualizací 5.
* Pokud spouštíte aplikaci DPM s nástrojem System Center 2012 SP1, nainstalujte kumulativní aktualizaci 2 pro System Center Data Protection Manager SP1. To je potřeba, před instalací hello Azure Backup Agent.
* Hello DPM server by měl mít prostředí Windows PowerShell a rozhraní .net Framework 4.5 nainstalované.
* Aplikace DPM můžete zálohovat tooAzure většinu úloh zálohování. Úplný seznam, co je podporováno najdete text hello Azure Backup podporují následující položky.
* Data uložená ve službě Azure Backup nelze obnovit pomocí možnosti "zkopírovat tootape" hello.
* Budete potřebovat účet Azure s povolenou funkcí zálohování Azure hello. Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Přečtěte si informace o [cenách zálohování Azure](https://azure.microsoft.com/pricing/details/backup/).
* Použití zálohování Azure vyžaduje toobe hello Azure Backup Agent nainstalovaný na hello servery, které chcete tooback nahoru. Každý server musí mít minimálně 10 % velikosti hello hello dat, která se zálohuje, k dispozici jako místní volný úložný prostor. Například zálohování 100 GB dat vyžaduje minimálně 10 GB volného místa v umístění pomocné hello. Při minimální hello je 10 %, se doporučuje 15 % toobe místního volného úložného místa pro umístění mezipaměti hello.
* Data budou uložena v hello trezor služby Azure storage. Neexistuje žádné omezení toohello množství dat, které můžete zálohovat trezoru zálohování Azure tooan ale hello velikost zdroje dat (třeba virtuální počítač nebo databáze) by neměl být delší než 54,400 GB.

Pro zálohování tooAzure se podporují tyto typy souborů:

* Šifrované (pouze úplné zálohy)
* Komprimované (je podporováno přírůstkové zálohování)
* Zhuštěné (je podporováno přírůstkové zálohování)
* Komprimované a zhuštěné (zpracovány jako zhuštěné)

A tyto nejsou podporovány:

* Servery v systémech souborů s rozlišením velkých nejsou podporovány.
* Pevné odkazy (vynecháno)
* Body rozboru (vynecháno)
* Zašifrované a komprimované (vynecháno)
* Šifrované a zhuštěné (vynecháno)
* Komprimovaný datový proud
* Zhuštěný datový proud

> [!NOTE]
> Z v System Center DPM 2012 s aktualizací SP1 a vyšší, můžete zálohovat do zatížení chráněné aplikací DPM tooAzure pomocí služby Microsoft Azure Backup.
>
>
