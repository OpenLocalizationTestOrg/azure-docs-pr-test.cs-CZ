---
title: "Oprávnění kořenového aaaUse na virtuální počítače s Linuxem | Microsoft Docs"
description: "Zjistěte, jak toouse kořenové oprávnění na virtuální počítač s Linuxem v Azure."
services: virtual-machines-linux
documentationcenter: 
author: szarkos
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: a2c106a2-dceb-43a3-9dd1-50ed77685952
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/02/2017
ms.author: szark
ms.openlocfilehash: 9411588c5fd0c86c4c73b3e44fbb56ab150013d5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-root-privileges-on-linux-virtual-machines-in-azure"></a><span data-ttu-id="b5dba-103">Použití kořenových oprávnění u virtuálních počítačů s Linuxem v Azure</span><span class="sxs-lookup"><span data-stu-id="b5dba-103">Using root privileges on Linux virtual machines in Azure</span></span>
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="b5dba-104">Ve výchozím nastavení, hello `root` je uživatel zakázán na virtuální počítače s Linuxem v Azure.</span><span class="sxs-lookup"><span data-stu-id="b5dba-104">By default, hello `root` user is disabled on Linux virtual machines in Azure.</span></span> <span data-ttu-id="b5dba-105">Uživatelé mohou spouštět příkazy se zvýšeným oprávněním pomocí hello `sudo` příkaz.</span><span class="sxs-lookup"><span data-stu-id="b5dba-105">Users can run commands with elevated privileges by using hello `sudo` command.</span></span> <span data-ttu-id="b5dba-106">Hello prostředí však mohou lišit v závislosti na tom, jak zřízenou hello systému.</span><span class="sxs-lookup"><span data-stu-id="b5dba-106">However, hello experience may vary depending on how hello system was provisioned.</span></span>

1. <span data-ttu-id="b5dba-107">**Klíč SSH a heslo nebo heslo pouze** -zřízenou hello virtuální počítač buď certifikátem (`.CER` soubor) nebo klíč SSH a také heslo, nebo jenom uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="b5dba-107">**SSH key and password OR password only** - hello virtual machine was provisioned with either a certificate (`.CER` file) or SSH key as well as a password, or just a user name and password.</span></span> <span data-ttu-id="b5dba-108">V takovém případě `sudo` výzvou k zadání hesla hello uživatele, před provedením příkazu hello.</span><span class="sxs-lookup"><span data-stu-id="b5dba-108">In this case `sudo` will prompt for hello user's password before executing hello command.</span></span>
2. <span data-ttu-id="b5dba-109">**Klíč SSH pouze** -zřízenou hello virtuální počítač s certifikátem (`.cer`, `.pem`, nebo `.pub` soubor) nebo klíč SSH, ale žádné heslo.</span><span class="sxs-lookup"><span data-stu-id="b5dba-109">**SSH key only** - hello virtual machine was provisioned with a certificate (`.cer`, `.pem`, or `.pub` file) or SSH key, but no password.</span></span>  <span data-ttu-id="b5dba-110">V takovém případě `sudo` **nikoli** výzvy pro heslo hello uživatele před provedením příkazu hello.</span><span class="sxs-lookup"><span data-stu-id="b5dba-110">In this case `sudo` **will not** prompt for hello user's password before executing hello command.</span></span>

## <a name="ssh-key-and-password-or-password-only"></a><span data-ttu-id="b5dba-111">SSH klíč a heslo nebo pouze heslo</span><span class="sxs-lookup"><span data-stu-id="b5dba-111">SSH Key and Password, or Password Only</span></span>
<span data-ttu-id="b5dba-112">Přihlaste se k hello Linux virtuálního počítače pomocí ověřování SSH klíč nebo heslo a potom spusťte příkazy pomocí `sudo`, například:</span><span class="sxs-lookup"><span data-stu-id="b5dba-112">Log into hello Linux virtual machine using SSH key or password authentication, then run commands using `sudo`, for example:</span></span>

    # sudo <command>
    [sudo] password for azureuser:

<span data-ttu-id="b5dba-113">V takovém případě hello uživateli zobrazí výzva k zadání hesla.</span><span class="sxs-lookup"><span data-stu-id="b5dba-113">In this case hello user will be prompted for a password.</span></span> <span data-ttu-id="b5dba-114">Po zadání hesla hello `sudo` spustí příkaz hello s `root` oprávnění.</span><span class="sxs-lookup"><span data-stu-id="b5dba-114">After entering hello password `sudo` will run hello command with `root` privileges.</span></span>

<span data-ttu-id="b5dba-115">Můžete také povolit passwordless sudo úpravou hello `/etc/sudoers.d/waagent` souboru, například:</span><span class="sxs-lookup"><span data-stu-id="b5dba-115">You can also enable passwordless sudo by editing hello `/etc/sudoers.d/waagent` file, for example:</span></span>

    #/etc/sudoers.d/waagent
    azureuser ALL = (ALL) NOPASSWD: ALL

<span data-ttu-id="b5dba-116">Tato změna umožňuje passwordless sudo uživatelem hello "azureuser".</span><span class="sxs-lookup"><span data-stu-id="b5dba-116">This change will allow for passwordless sudo by hello user "azureuser".</span></span>

## <a name="ssh-key-only"></a><span data-ttu-id="b5dba-117">SSH pouze klíč</span><span class="sxs-lookup"><span data-stu-id="b5dba-117">SSH Key Only</span></span>
<span data-ttu-id="b5dba-118">Přihlaste se k hello Linux virtuálního počítače pomocí ověření pomocí klíče SSH, a poté spusťte příkazy pomocí `sudo`, například:</span><span class="sxs-lookup"><span data-stu-id="b5dba-118">Log into hello Linux virtual machine using SSH key authentication, then run commands using `sudo`, for example:</span></span>

    # sudo <command>

<span data-ttu-id="b5dba-119">V takovém případě bude uživatel hello **není** vyzváni k zadání hesla.</span><span class="sxs-lookup"><span data-stu-id="b5dba-119">In this case hello user will **not** be prompted for a password.</span></span> <span data-ttu-id="b5dba-120">Po stiskněte `<enter>`, `sudo` spustí příkaz hello s `root` oprávnění.</span><span class="sxs-lookup"><span data-stu-id="b5dba-120">After pressing `<enter>`, `sudo` will run hello command with `root` privileges.</span></span>

