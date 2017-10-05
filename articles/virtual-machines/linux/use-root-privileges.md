---
title: "Pomocí oprávnění kořenového na virtuální počítače s Linuxem | Microsoft Docs"
description: "Další informace o použití oprávnění kořenového na virtuální počítač s Linuxem v Azure."
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
ms.openlocfilehash: dc39db1f5fecffb60499a5420bfe72850e2fffd9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="using-root-privileges-on-linux-virtual-machines-in-azure"></a><span data-ttu-id="01c94-103">Použití kořenových oprávnění u virtuálních počítačů s Linuxem v Azure</span><span class="sxs-lookup"><span data-stu-id="01c94-103">Using root privileges on Linux virtual machines in Azure</span></span>
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="01c94-104">Ve výchozím nastavení `root` je uživatel zakázán na virtuální počítače s Linuxem v Azure.</span><span class="sxs-lookup"><span data-stu-id="01c94-104">By default, the `root` user is disabled on Linux virtual machines in Azure.</span></span> <span data-ttu-id="01c94-105">Uživatelé mohou spouštět příkazy se zvýšeným oprávněním pomocí `sudo` příkaz.</span><span class="sxs-lookup"><span data-stu-id="01c94-105">Users can run commands with elevated privileges by using the `sudo` command.</span></span> <span data-ttu-id="01c94-106">Možnosti však mohou lišit v závislosti na tom, jak zřízenou systému.</span><span class="sxs-lookup"><span data-stu-id="01c94-106">However, the experience may vary depending on how the system was provisioned.</span></span>

1. <span data-ttu-id="01c94-107">**Klíč SSH a heslo nebo heslo pouze** -zřízenou virtuální počítač buď certifikátem (`.CER` soubor) nebo klíč SSH a také heslo, nebo jenom uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="01c94-107">**SSH key and password OR password only** - the virtual machine was provisioned with either a certificate (`.CER` file) or SSH key as well as a password, or just a user name and password.</span></span> <span data-ttu-id="01c94-108">V takovém případě `sudo` výzvou k zadání hesla před provedením příkazu.</span><span class="sxs-lookup"><span data-stu-id="01c94-108">In this case `sudo` will prompt for the user's password before executing the command.</span></span>
2. <span data-ttu-id="01c94-109">**Klíč SSH pouze** -zřízenou virtuální počítač s certifikátem (`.cer`, `.pem`, nebo `.pub` soubor) nebo klíč SSH, ale žádné heslo.</span><span class="sxs-lookup"><span data-stu-id="01c94-109">**SSH key only** - the virtual machine was provisioned with a certificate (`.cer`, `.pem`, or `.pub` file) or SSH key, but no password.</span></span>  <span data-ttu-id="01c94-110">V takovém případě `sudo` **nikoli** výzvy pro heslo uživatele před provedením příkazu.</span><span class="sxs-lookup"><span data-stu-id="01c94-110">In this case `sudo` **will not** prompt for the user's password before executing the command.</span></span>

## <a name="ssh-key-and-password-or-password-only"></a><span data-ttu-id="01c94-111">SSH klíč a heslo nebo pouze heslo</span><span class="sxs-lookup"><span data-stu-id="01c94-111">SSH Key and Password, or Password Only</span></span>
<span data-ttu-id="01c94-112">Přihlaste se k virtuálnímu počítači Linux pomocí ověřování SSH klíč nebo heslo a potom spusťte příkazy pomocí `sudo`, například:</span><span class="sxs-lookup"><span data-stu-id="01c94-112">Log into the Linux virtual machine using SSH key or password authentication, then run commands using `sudo`, for example:</span></span>

    # sudo <command>
    [sudo] password for azureuser:

<span data-ttu-id="01c94-113">V tomto případě uživatel se vyzve k zadání hesla.</span><span class="sxs-lookup"><span data-stu-id="01c94-113">In this case the user will be prompted for a password.</span></span> <span data-ttu-id="01c94-114">Po zadání hesla `sudo` spustí příkaz s `root` oprávnění.</span><span class="sxs-lookup"><span data-stu-id="01c94-114">After entering the password `sudo` will run the command with `root` privileges.</span></span>

<span data-ttu-id="01c94-115">Můžete také povolit passwordless sudo úpravou `/etc/sudoers.d/waagent` souboru, například:</span><span class="sxs-lookup"><span data-stu-id="01c94-115">You can also enable passwordless sudo by editing the `/etc/sudoers.d/waagent` file, for example:</span></span>

    #/etc/sudoers.d/waagent
    azureuser ALL = (ALL) NOPASSWD: ALL

<span data-ttu-id="01c94-116">Tato změna umožňuje passwordless sudo uživatelem "azureuser".</span><span class="sxs-lookup"><span data-stu-id="01c94-116">This change will allow for passwordless sudo by the user "azureuser".</span></span>

## <a name="ssh-key-only"></a><span data-ttu-id="01c94-117">SSH pouze klíč</span><span class="sxs-lookup"><span data-stu-id="01c94-117">SSH Key Only</span></span>
<span data-ttu-id="01c94-118">Přihlaste se k virtuálnímu počítači Linux pomocí ověření pomocí klíče SSH, a poté spusťte příkazy pomocí `sudo`, například:</span><span class="sxs-lookup"><span data-stu-id="01c94-118">Log into the Linux virtual machine using SSH key authentication, then run commands using `sudo`, for example:</span></span>

    # sudo <command>

<span data-ttu-id="01c94-119">V takovém případě bude uživatel **není** vyzváni k zadání hesla.</span><span class="sxs-lookup"><span data-stu-id="01c94-119">In this case the user will **not** be prompted for a password.</span></span> <span data-ttu-id="01c94-120">Po stiskněte `<enter>`, `sudo` spustí příkaz s `root` oprávnění.</span><span class="sxs-lookup"><span data-stu-id="01c94-120">After pressing `<enter>`, `sudo` will run the command with `root` privileges.</span></span>

