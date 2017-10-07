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
# <a name="run-an-unattended-installation-of-azure-backup-server-v2"></a><span data-ttu-id="15cf5-104">Spusťte bezobslužnou instalaci serveru Azure Backup v2</span><span class="sxs-lookup"><span data-stu-id="15cf5-104">Run an unattended installation of Azure Backup Server v2</span></span>

<span data-ttu-id="15cf5-105">Zjistěte, jak toorun bezobslužnou instalaci serveru Azure Backup v2.</span><span class="sxs-lookup"><span data-stu-id="15cf5-105">Learn how toorun an unattended installation of Azure Backup Server v2.</span></span> 

<span data-ttu-id="15cf5-106">Tyto kroky neplatí, pokud instalujete Azure Backup Server v1.</span><span class="sxs-lookup"><span data-stu-id="15cf5-106">These steps do not apply if you are installing Azure Backup Server v1.</span></span>

## <a name="install-backup-server-v2"></a><span data-ttu-id="15cf5-107">Nainstalujte Backup Server v2</span><span class="sxs-lookup"><span data-stu-id="15cf5-107">Install Backup Server v2</span></span>

1. <span data-ttu-id="15cf5-108">Na serveru hello, který je hostitelem serveru Azure Backup v2 vytvořte textový soubor.</span><span class="sxs-lookup"><span data-stu-id="15cf5-108">On hello server that hosts Azure Backup Server v2, create a text file.</span></span> <span data-ttu-id="15cf5-109">(Můžete vytvořit soubor hello v poznámkovém bloku nebo v jiném textu editoru.) Uložte soubor hello jako MABSSetup.ini.</span><span class="sxs-lookup"><span data-stu-id="15cf5-109">(You can create hello file in Notepad or in another text editor.) Save hello file as MABSSetup.ini.</span></span> 

2. <span data-ttu-id="15cf5-110">Vložte následující kód v souboru MABSSetup.ini hello hello.</span><span class="sxs-lookup"><span data-stu-id="15cf5-110">Paste hello following code in hello MABSSetup.ini file.</span></span> <span data-ttu-id="15cf5-111">Nahraďte text hello uvnitř hello závorek (\< \>) s hodnotami ze svého prostředí.</span><span class="sxs-lookup"><span data-stu-id="15cf5-111">Replace hello text inside hello brackets (\< \>) with values from your environment.</span></span> <span data-ttu-id="15cf5-112">Následující text Hello je příklad:</span><span class="sxs-lookup"><span data-stu-id="15cf5-112">hello following text is an example:</span></span>

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

3. <span data-ttu-id="15cf5-113">Uložte soubor hello.</span><span class="sxs-lookup"><span data-stu-id="15cf5-113">Save hello file.</span></span> <span data-ttu-id="15cf5-114">Na příkazovém řádku se zvýšenými oprávněními na serveru pro instalaci hello, zadejte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="15cf5-114">Then, at an elevated command prompt on hello installation server, enter this command:</span></span>

  ```
  start /wait <cdlayout path>/Setup.exe /i  /f <.ini file path>/setup.ini /L <log path>/setup.log
  ```

<span data-ttu-id="15cf5-115">Pro instalaci hello můžete použít tyto příznaky:</span><span class="sxs-lookup"><span data-stu-id="15cf5-115">You can use these flags for hello installation:</span></span></br><span data-ttu-id="15cf5-116">
**/f**: cesta k souboru INI</span><span class="sxs-lookup"><span data-stu-id="15cf5-116">
**/f**: .ini file path</span></span></br><span data-ttu-id="15cf5-117">
**/l**: cesta protokolu</span><span class="sxs-lookup"><span data-stu-id="15cf5-117">
**/l**: Log path</span></span></br><span data-ttu-id="15cf5-118">
**/i**: Instalační cesta</span><span class="sxs-lookup"><span data-stu-id="15cf5-118">
**/i**: Installation path</span></span></br><span data-ttu-id="15cf5-119">
**/x**: Odinstalujte cesta</span><span class="sxs-lookup"><span data-stu-id="15cf5-119">
**/x**: Uninstall path</span></span></br>

## <a name="next-steps"></a><span data-ttu-id="15cf5-120">Další kroky</span><span class="sxs-lookup"><span data-stu-id="15cf5-120">Next steps</span></span>
<span data-ttu-id="15cf5-121">Když nainstalujete Backup Server, zjistěte, jak tooprepare váš server, nebo začít chránit zatížení.</span><span class="sxs-lookup"><span data-stu-id="15cf5-121">After you install Backup Server, learn how tooprepare your server, or begin protecting a workload.</span></span>

- [<span data-ttu-id="15cf5-122">Příprava úlohy zálohování serveru</span><span class="sxs-lookup"><span data-stu-id="15cf5-122">Prepare Backup Server workloads</span></span>](backup-azure-microsoft-azure-backup.md)
- [<span data-ttu-id="15cf5-123">Pomocí zálohování serveru tooback server VMware</span><span class="sxs-lookup"><span data-stu-id="15cf5-123">Use Backup Server tooback up a VMware server</span></span>](backup-azure-backup-server-vmware.md)
- [<span data-ttu-id="15cf5-124">Použít tooback zálohování serveru SQL Server</span><span class="sxs-lookup"><span data-stu-id="15cf5-124">Use Backup Server tooback up SQL Server</span></span>](backup-azure-sql-mabs.md)
- [<span data-ttu-id="15cf5-125">Přidat tooBackup moderní úložiště zálohování serveru</span><span class="sxs-lookup"><span data-stu-id="15cf5-125">Add Modern Backup Storage tooBackup Server</span></span>](backup-mabs-add-storage.md)
