---
title: Tichou instalaci serveru Azure Backup v2 | Microsoft Docs
description: "Použití skriptu prostředí PowerShell k bezobslužné instalaci serveru Azure Backup v2. Tento typ instalace je také označován bezobslužné instalace."
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
ms.openlocfilehash: 91778a67f9ef523aa87b7918197e0d0ded0f5702
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="run-an-unattended-installation-of-azure-backup-server-v2"></a><span data-ttu-id="ce785-104">Spusťte bezobslužnou instalaci serveru Azure Backup v2</span><span class="sxs-lookup"><span data-stu-id="ce785-104">Run an unattended installation of Azure Backup Server v2</span></span>

<span data-ttu-id="ce785-105">Postup spuštění bezobslužné instalace serveru Azure Backup v2.</span><span class="sxs-lookup"><span data-stu-id="ce785-105">Learn how to run an unattended installation of Azure Backup Server v2.</span></span> 

<span data-ttu-id="ce785-106">Tyto kroky neplatí, pokud instalujete Azure Backup Server v1.</span><span class="sxs-lookup"><span data-stu-id="ce785-106">These steps do not apply if you are installing Azure Backup Server v1.</span></span>

## <a name="install-backup-server-v2"></a><span data-ttu-id="ce785-107">Nainstalujte Backup Server v2</span><span class="sxs-lookup"><span data-stu-id="ce785-107">Install Backup Server v2</span></span>

1. <span data-ttu-id="ce785-108">Na serveru, který je hostitelem serveru Azure Backup v2 vytvořte textový soubor.</span><span class="sxs-lookup"><span data-stu-id="ce785-108">On the server that hosts Azure Backup Server v2, create a text file.</span></span> <span data-ttu-id="ce785-109">(Soubor můžete vytvořit v poznámkovém bloku nebo v jiném textovém editoru.) Uložte soubor jako MABSSetup.ini.</span><span class="sxs-lookup"><span data-stu-id="ce785-109">(You can create the file in Notepad or in another text editor.) Save the file as MABSSetup.ini.</span></span> 

2. <span data-ttu-id="ce785-110">Vložte následující kód v souboru MABSSetup.ini.</span><span class="sxs-lookup"><span data-stu-id="ce785-110">Paste the following code in the MABSSetup.ini file.</span></span> <span data-ttu-id="ce785-111">Nahraďte text v závorkách (\< \>) s hodnotami ze svého prostředí.</span><span class="sxs-lookup"><span data-stu-id="ce785-111">Replace the text inside the brackets (\< \>) with values from your environment.</span></span> <span data-ttu-id="ce785-112">Tento text je příklad:</span><span class="sxs-lookup"><span data-stu-id="ce785-112">The following text is an example:</span></span>

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

3. <span data-ttu-id="ce785-113">Uložte soubor.</span><span class="sxs-lookup"><span data-stu-id="ce785-113">Save the file.</span></span> <span data-ttu-id="ce785-114">Na příkazovém řádku se zvýšenými oprávněními na serveru pro instalaci, zadejte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="ce785-114">Then, at an elevated command prompt on the installation server, enter this command:</span></span>

  ```
  start /wait <cdlayout path>/Setup.exe /i  /f <.ini file path>/setup.ini /L <log path>/setup.log
  ```

<span data-ttu-id="ce785-115">Pro instalaci můžete použít tyto příznaky:</span><span class="sxs-lookup"><span data-stu-id="ce785-115">You can use these flags for the installation:</span></span></br><span data-ttu-id="ce785-116">
**/f**: cesta k souboru INI</span><span class="sxs-lookup"><span data-stu-id="ce785-116">
**/f**: .ini file path</span></span></br><span data-ttu-id="ce785-117">
**/l**: cesta protokolu</span><span class="sxs-lookup"><span data-stu-id="ce785-117">
**/l**: Log path</span></span></br><span data-ttu-id="ce785-118">
**/i**: Instalační cesta</span><span class="sxs-lookup"><span data-stu-id="ce785-118">
**/i**: Installation path</span></span></br><span data-ttu-id="ce785-119">
**/x**: Odinstalujte cesta</span><span class="sxs-lookup"><span data-stu-id="ce785-119">
**/x**: Uninstall path</span></span></br>

## <a name="next-steps"></a><span data-ttu-id="ce785-120">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ce785-120">Next steps</span></span>
<span data-ttu-id="ce785-121">Po instalaci serveru zálohování, zjistěte, jak připravit server nebo začít chránit zatížení.</span><span class="sxs-lookup"><span data-stu-id="ce785-121">After you install Backup Server, learn how to prepare your server, or begin protecting a workload.</span></span>

- [<span data-ttu-id="ce785-122">Příprava úlohy zálohování serveru</span><span class="sxs-lookup"><span data-stu-id="ce785-122">Prepare Backup Server workloads</span></span>](backup-azure-microsoft-azure-backup.md)
- [<span data-ttu-id="ce785-123">Pomocí zálohování serveru zálohovat VMware server</span><span class="sxs-lookup"><span data-stu-id="ce785-123">Use Backup Server to back up a VMware server</span></span>](backup-azure-backup-server-vmware.md)
- [<span data-ttu-id="ce785-124">Použít zálohování serveru k zálohování systému SQL Server</span><span class="sxs-lookup"><span data-stu-id="ce785-124">Use Backup Server to back up SQL Server</span></span>](backup-azure-sql-mabs.md)
- [<span data-ttu-id="ce785-125">Přidat moderní úložiště zálohování k zálohování serveru</span><span class="sxs-lookup"><span data-stu-id="ce785-125">Add Modern Backup Storage to Backup Server</span></span>](backup-mabs-add-storage.md)
