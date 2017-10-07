---
title: instalace aaaSilent v2 serveru Azure Backup | Microsoft Docs
description: "Použití toosilently skript prostředí PowerShell nainstalujte Azure Backup Server v2. Tento typ instalace je také označován bezobslužné instalace."
services: backup
documentationcenter: " "
author: markgalioto
manager: carmonm
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 05/30/2017
ms.author: markgal;masaran
ms.openlocfilehash: 6b94b4a278bfcd5f8c5c363cb811bd8eec984243
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="run-an-unattended-installation-of-azure-backup-server-v2"></a>Spusťte bezobslužnou instalaci serveru Azure Backup v2

Zjistěte, jak toorun bezobslužnou instalaci serveru Azure Backup v2. 

Tyto kroky neplatí, pokud instalujete Azure Backup Server v1.

## <a name="install-backup-server-v2"></a>Nainstalujte Backup Server v2

1. Na serveru hello, který je hostitelem serveru Azure Backup v2 vytvořte textový soubor. (Můžete vytvořit soubor hello v poznámkovém bloku nebo v jiném textu editoru.) Uložte soubor hello jako MABSSetup.ini. 

2. Vložte následující kód v souboru MABSSetup.ini hello hello. Nahraďte text hello uvnitř hello závorek (\< \>) s hodnotami ze svého prostředí. Následující text Hello je příklad:

  ```
  [OPTIONS]
  UserName=administrator
  CompanyName=<Microsoft Corporation>
  SQLMachineName=localhost
  SQLInstanceName=<SQL instance name>
  SQLMachineUserName=administrator
  SQLMachinePassword=<admin password>
  SQLMachineDomainName=<machine domain>
  ReportingMachineName=localhost
  ReportingInstanceName=<reporting instance name>
  SqlAccountPassword=<admin password>
  ReportingMachineUserName=<username>
  ReportingMachinePassword=<reporting admin password>
  ReportingMachineDomainName=<domain>
  VaultCredentialFilePath=<vault credential full path and complete name>
  SecurityPassphrase=<passphrase>
  PassphraseSaveLocation=<passphrase save location>
  UseExistingSQL=<1/0 use or do not use existing SQL>
  ```

3. Uložte soubor hello. Na příkazovém řádku se zvýšenými oprávněními na serveru pro instalaci hello, zadejte tento příkaz:

  ```
  start /wait <cdlayout path>/Setup.exe /i  /f <.ini file path>/setup.ini /L <log path>/setup.log
  ```

Pro instalaci hello můžete použít tyto příznaky:</br>
**/f**: cesta k souboru INI</br>
**/l**: cesta protokolu</br>
**/i**: Instalační cesta</br>
**/x**: Odinstalujte cesta</br>

## <a name="next-steps"></a>Další kroky
Když nainstalujete Backup Server, zjistěte, jak tooprepare váš server, nebo začít chránit zatížení.

- [Příprava úlohy zálohování serveru](backup-azure-microsoft-azure-backup.md)
- [Pomocí zálohování serveru tooback server VMware](backup-azure-backup-server-vmware.md)
- [Použít tooback zálohování serveru SQL Server](backup-azure-sql-mabs.md)
- [Přidat tooBackup moderní úložiště zálohování serveru](backup-mabs-add-storage.md)
