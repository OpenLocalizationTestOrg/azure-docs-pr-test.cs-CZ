---
title: "Výběr uživatelská jména pro Linux | Microsoft Docs"
description: "Zjistěte, jak vybrat uživatelská jména pro virtuální počítač s Linuxem v Azure."
services: virtual-machines-linux
documentationcenter: 
author: szarkos
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 33b50c97-92f1-46c9-a623-e37f67459c5c
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/02/2017
ms.author: szark
ms.openlocfilehash: 1874d72e5f88816036667932371ff28704d186c8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="selecting-user-names-for-linux-on-azure"></a><span data-ttu-id="019b4-103">Výběr uživatelských jmen pro Linux v Azure</span><span class="sxs-lookup"><span data-stu-id="019b4-103">Selecting User Names for Linux on Azure</span></span>
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="019b4-104">Při zřizování virtuální počítač s Linuxem v Azure musíte zadat název jiné kořenové uživatele, který můžete později použít k přihlášení do virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="019b4-104">When you provision a Linux virtual machine on Azure you must specify the name of a non-root user that you can later use to log into the VM.</span></span> <span data-ttu-id="019b4-105">Můžete se rozhodnout název nového uživatele, nebo pokud zřizování prostřednictvím portálu Azure classic můžete přijmout výchozí název "azureuser".</span><span class="sxs-lookup"><span data-stu-id="019b4-105">You may choose the name of the new user, or if provisioning via the Azure classic portal you can accept the default name "azureuser".</span></span>

<span data-ttu-id="019b4-106">Ve většině případů tohoto uživatele, nebude existovat bitová kopie a vytvoří se během procesu zřizování.</span><span class="sxs-lookup"><span data-stu-id="019b4-106">In most cases this user won't exist on the base image and will be created during the provisioning process.</span></span> <span data-ttu-id="019b4-107">Pokud uživatel na základní image virtuálního počítače existuje, pak Azure Linux agent jednoduše nakonfiguruje heslo nebo klíč SSH pro tohoto uživatele na základě informací, které jste zadali při vytváření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="019b4-107">If the user exists on the base VM image, then the Azure Linux agent simply configures the password and/or SSH key for that user based on the information you specified when creating the VM.</span></span>

<span data-ttu-id="019b4-108">**Ale**, Linux definuje sadu uživatelská jména, která by se neměla používat.</span><span class="sxs-lookup"><span data-stu-id="019b4-108">**However**, Linux defines a set of user names that should not be used.</span></span> <span data-ttu-id="019b4-109">Proces zřizování bude **nezdaří** Pokud se pokusíte zřízení virtuálního počítače s Linuxem pomocí stávajícího uživatele systému, která je definována jako uživatel s UID 0-99.</span><span class="sxs-lookup"><span data-stu-id="019b4-109">The provisioning process will **fail** if you try to provision a Linux VM using an existing system user, which is defined as a user with UID 0-99.</span></span> <span data-ttu-id="019b4-110">Typickým příkladem je `root` uživatele, který má UID 0.</span><span class="sxs-lookup"><span data-stu-id="019b4-110">A typical example is the `root` user, which has UID 0.</span></span>

* <span data-ttu-id="019b4-111">Viz také: [základní Linux Standard - rozsahy ID uživatele](http://refspecs.linuxfoundation.org/LSB_4.1.0/LSB-Core-generic/LSB-Core-generic/uidrange.html)</span><span class="sxs-lookup"><span data-stu-id="019b4-111">See also: [Linux Standard Base - User ID Ranges](http://refspecs.linuxfoundation.org/LSB_4.1.0/LSB-Core-generic/LSB-Core-generic/uidrange.html)</span></span>

<span data-ttu-id="019b4-112">Následuje seznam běžných uživatelů předdefinované systému CentOS a Ubuntu, že byste neměli používat při zřizování virtuální počítač s Linuxem v Azure.</span><span class="sxs-lookup"><span data-stu-id="019b4-112">The following is a list of common built-in system users for CentOS and Ubuntu that you should avoid using when provisioning a Linux virtual machine on Azure.</span></span> <span data-ttu-id="019b4-113">Tento seznam je jenom jako příklad naleznete v dokumentaci k distribuční zajistit, že zadané uživatelské jméno, které zvolíte nejsou v konfliktu s existujícím uživatelem systému.</span><span class="sxs-lookup"><span data-stu-id="019b4-113">This list is just an example, please refer to the documentation for your distribution to ensure that the username you choose does not conflict with an existing system user.</span></span>

## <a name="centos"></a><span data-ttu-id="019b4-114">CentOS</span><span class="sxs-lookup"><span data-stu-id="019b4-114">CentOS</span></span>
* <span data-ttu-id="019b4-115">abrt</span><span class="sxs-lookup"><span data-stu-id="019b4-115">abrt</span></span>
* <span data-ttu-id="019b4-116">ADM</span><span class="sxs-lookup"><span data-stu-id="019b4-116">adm</span></span>
* <span data-ttu-id="019b4-117">zvuk</span><span class="sxs-lookup"><span data-stu-id="019b4-117">audio</span></span>
* <span data-ttu-id="019b4-118">Koš</span><span class="sxs-lookup"><span data-stu-id="019b4-118">bin</span></span>
* <span data-ttu-id="019b4-119">CD-ROM</span><span class="sxs-lookup"><span data-stu-id="019b4-119">cdrom</span></span>
* <span data-ttu-id="019b4-120">cgred</span><span class="sxs-lookup"><span data-stu-id="019b4-120">cgred</span></span>
* <span data-ttu-id="019b4-121">Démon procesu</span><span class="sxs-lookup"><span data-stu-id="019b4-121">daemon</span></span>
* <span data-ttu-id="019b4-122">dbus</span><span class="sxs-lookup"><span data-stu-id="019b4-122">dbus</span></span>
* <span data-ttu-id="019b4-123">síti službou</span><span class="sxs-lookup"><span data-stu-id="019b4-123">dialout</span></span>
* <span data-ttu-id="019b4-124">vyhrazené IP adresy</span><span class="sxs-lookup"><span data-stu-id="019b4-124">dip</span></span>
* <span data-ttu-id="019b4-125">disk</span><span class="sxs-lookup"><span data-stu-id="019b4-125">disk</span></span>
* <span data-ttu-id="019b4-126">disketovou</span><span class="sxs-lookup"><span data-stu-id="019b4-126">floppy</span></span>
* <span data-ttu-id="019b4-127">FTP</span><span class="sxs-lookup"><span data-stu-id="019b4-127">ftp</span></span>
* <span data-ttu-id="019b4-128">FTP</span><span class="sxs-lookup"><span data-stu-id="019b4-128">ftp</span></span>
* <span data-ttu-id="019b4-129">hry</span><span class="sxs-lookup"><span data-stu-id="019b4-129">games</span></span>
* <span data-ttu-id="019b4-130">Gopher</span><span class="sxs-lookup"><span data-stu-id="019b4-130">gopher</span></span>
* <span data-ttu-id="019b4-131">haldaemon</span><span class="sxs-lookup"><span data-stu-id="019b4-131">haldaemon</span></span>
* <span data-ttu-id="019b4-132">zastavení</span><span class="sxs-lookup"><span data-stu-id="019b4-132">halt</span></span>
* <span data-ttu-id="019b4-133">kmem</span><span class="sxs-lookup"><span data-stu-id="019b4-133">kmem</span></span>
* <span data-ttu-id="019b4-134">Zámek</span><span class="sxs-lookup"><span data-stu-id="019b4-134">lock</span></span>
* <span data-ttu-id="019b4-135">lineárního programování úloh</span><span class="sxs-lookup"><span data-stu-id="019b4-135">lp</span></span>
* <span data-ttu-id="019b4-136">E-mailu</span><span class="sxs-lookup"><span data-stu-id="019b4-136">mail</span></span>
* <span data-ttu-id="019b4-137">Man</span><span class="sxs-lookup"><span data-stu-id="019b4-137">man</span></span>
* <span data-ttu-id="019b4-138">mem</span><span class="sxs-lookup"><span data-stu-id="019b4-138">mem</span></span>
* <span data-ttu-id="019b4-139">nfsnobody</span><span class="sxs-lookup"><span data-stu-id="019b4-139">nfsnobody</span></span>
* <span data-ttu-id="019b4-140">nikdo</span><span class="sxs-lookup"><span data-stu-id="019b4-140">nobody</span></span>
* <span data-ttu-id="019b4-141">NTP</span><span class="sxs-lookup"><span data-stu-id="019b4-141">ntp</span></span>
* <span data-ttu-id="019b4-142">Operátor</span><span class="sxs-lookup"><span data-stu-id="019b4-142">operator</span></span>
* <span data-ttu-id="019b4-143">oprofile</span><span class="sxs-lookup"><span data-stu-id="019b4-143">oprofile</span></span>
* <span data-ttu-id="019b4-144">postdrop</span><span class="sxs-lookup"><span data-stu-id="019b4-144">postdrop</span></span>
* <span data-ttu-id="019b4-145">operátory přípony</span><span class="sxs-lookup"><span data-stu-id="019b4-145">postfix</span></span>
* <span data-ttu-id="019b4-146">qpidd</span><span class="sxs-lookup"><span data-stu-id="019b4-146">qpidd</span></span>
* <span data-ttu-id="019b4-147">kořenové</span><span class="sxs-lookup"><span data-stu-id="019b4-147">root</span></span>
* <span data-ttu-id="019b4-148">RPC</span><span class="sxs-lookup"><span data-stu-id="019b4-148">rpc</span></span>
* <span data-ttu-id="019b4-149">rpcuser</span><span class="sxs-lookup"><span data-stu-id="019b4-149">rpcuser</span></span>
* <span data-ttu-id="019b4-150">saslauth</span><span class="sxs-lookup"><span data-stu-id="019b4-150">saslauth</span></span>
* <span data-ttu-id="019b4-151">Vypnutí</span><span class="sxs-lookup"><span data-stu-id="019b4-151">shutdown</span></span>
* <span data-ttu-id="019b4-152">slocate</span><span class="sxs-lookup"><span data-stu-id="019b4-152">slocate</span></span>
* <span data-ttu-id="019b4-153">sshd</span><span class="sxs-lookup"><span data-stu-id="019b4-153">sshd</span></span>
* <span data-ttu-id="019b4-154">stapdev</span><span class="sxs-lookup"><span data-stu-id="019b4-154">stapdev</span></span>
* <span data-ttu-id="019b4-155">stapusr</span><span class="sxs-lookup"><span data-stu-id="019b4-155">stapusr</span></span>
* <span data-ttu-id="019b4-156">Synchronizace</span><span class="sxs-lookup"><span data-stu-id="019b4-156">sync</span></span>
* <span data-ttu-id="019b4-157">Sys</span><span class="sxs-lookup"><span data-stu-id="019b4-157">sys</span></span>
* <span data-ttu-id="019b4-158">pásky</span><span class="sxs-lookup"><span data-stu-id="019b4-158">tape</span></span>
* <span data-ttu-id="019b4-159">Test</span><span class="sxs-lookup"><span data-stu-id="019b4-159">test</span></span>
* <span data-ttu-id="019b4-160">tcpdump</span><span class="sxs-lookup"><span data-stu-id="019b4-160">tcpdump</span></span>
* <span data-ttu-id="019b4-161">TTY</span><span class="sxs-lookup"><span data-stu-id="019b4-161">tty</span></span>
* <span data-ttu-id="019b4-162">uživatelé</span><span class="sxs-lookup"><span data-stu-id="019b4-162">users</span></span>
* <span data-ttu-id="019b4-163">utempter</span><span class="sxs-lookup"><span data-stu-id="019b4-163">utempter</span></span>
* <span data-ttu-id="019b4-164">procesu utmp</span><span class="sxs-lookup"><span data-stu-id="019b4-164">utmp</span></span>
* <span data-ttu-id="019b4-165">UUCP</span><span class="sxs-lookup"><span data-stu-id="019b4-165">uucp</span></span>
* <span data-ttu-id="019b4-166">vcsa</span><span class="sxs-lookup"><span data-stu-id="019b4-166">vcsa</span></span>
* <span data-ttu-id="019b4-167">video</span><span class="sxs-lookup"><span data-stu-id="019b4-167">video</span></span>
* <span data-ttu-id="019b4-168">Wheel</span><span class="sxs-lookup"><span data-stu-id="019b4-168">wheel</span></span>

## <a name="ubuntu"></a><span data-ttu-id="019b4-169">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="019b4-169">Ubuntu</span></span>
* <span data-ttu-id="019b4-170">ADM</span><span class="sxs-lookup"><span data-stu-id="019b4-170">adm</span></span>
* <span data-ttu-id="019b4-171">Správce</span><span class="sxs-lookup"><span data-stu-id="019b4-171">admin</span></span>
* <span data-ttu-id="019b4-172">zvuk</span><span class="sxs-lookup"><span data-stu-id="019b4-172">audio</span></span>
* <span data-ttu-id="019b4-173">zálohování</span><span class="sxs-lookup"><span data-stu-id="019b4-173">backup</span></span>
* <span data-ttu-id="019b4-174">Koš</span><span class="sxs-lookup"><span data-stu-id="019b4-174">bin</span></span>
* <span data-ttu-id="019b4-175">CD-ROM</span><span class="sxs-lookup"><span data-stu-id="019b4-175">cdrom</span></span>
* <span data-ttu-id="019b4-176">crontab</span><span class="sxs-lookup"><span data-stu-id="019b4-176">crontab</span></span>
* <span data-ttu-id="019b4-177">Démon procesu</span><span class="sxs-lookup"><span data-stu-id="019b4-177">daemon</span></span>
* <span data-ttu-id="019b4-178">síti službou</span><span class="sxs-lookup"><span data-stu-id="019b4-178">dialout</span></span>
* <span data-ttu-id="019b4-179">vyhrazené IP adresy</span><span class="sxs-lookup"><span data-stu-id="019b4-179">dip</span></span>
* <span data-ttu-id="019b4-180">disk</span><span class="sxs-lookup"><span data-stu-id="019b4-180">disk</span></span>
* <span data-ttu-id="019b4-181">Fax</span><span class="sxs-lookup"><span data-stu-id="019b4-181">fax</span></span>
* <span data-ttu-id="019b4-182">disketovou</span><span class="sxs-lookup"><span data-stu-id="019b4-182">floppy</span></span>
* <span data-ttu-id="019b4-183">pojistka</span><span class="sxs-lookup"><span data-stu-id="019b4-183">fuse</span></span>
* <span data-ttu-id="019b4-184">hry</span><span class="sxs-lookup"><span data-stu-id="019b4-184">games</span></span>
* <span data-ttu-id="019b4-185">gnats</span><span class="sxs-lookup"><span data-stu-id="019b4-185">gnats</span></span>
* <span data-ttu-id="019b4-186">IRC</span><span class="sxs-lookup"><span data-stu-id="019b4-186">irc</span></span>
* <span data-ttu-id="019b4-187">kmem</span><span class="sxs-lookup"><span data-stu-id="019b4-187">kmem</span></span>
* <span data-ttu-id="019b4-188">na šířku</span><span class="sxs-lookup"><span data-stu-id="019b4-188">landscape</span></span>
* <span data-ttu-id="019b4-189">libuuid</span><span class="sxs-lookup"><span data-stu-id="019b4-189">libuuid</span></span>
* <span data-ttu-id="019b4-190">seznam</span><span class="sxs-lookup"><span data-stu-id="019b4-190">list</span></span>
* <span data-ttu-id="019b4-191">lineárního programování úloh</span><span class="sxs-lookup"><span data-stu-id="019b4-191">lp</span></span>
* <span data-ttu-id="019b4-192">E-mailu</span><span class="sxs-lookup"><span data-stu-id="019b4-192">mail</span></span>
* <span data-ttu-id="019b4-193">Man</span><span class="sxs-lookup"><span data-stu-id="019b4-193">man</span></span>
* <span data-ttu-id="019b4-194">MessageBus</span><span class="sxs-lookup"><span data-stu-id="019b4-194">messagebus</span></span>
* <span data-ttu-id="019b4-195">mlocate</span><span class="sxs-lookup"><span data-stu-id="019b4-195">mlocate</span></span>
* <span data-ttu-id="019b4-196">netdev</span><span class="sxs-lookup"><span data-stu-id="019b4-196">netdev</span></span>
* <span data-ttu-id="019b4-197">Novinky</span><span class="sxs-lookup"><span data-stu-id="019b4-197">news</span></span>
* <span data-ttu-id="019b4-198">nikdo</span><span class="sxs-lookup"><span data-stu-id="019b4-198">nobody</span></span>
* <span data-ttu-id="019b4-199">"nogroup"</span><span class="sxs-lookup"><span data-stu-id="019b4-199">nogroup</span></span>
* <span data-ttu-id="019b4-200">Operátor</span><span class="sxs-lookup"><span data-stu-id="019b4-200">operator</span></span>
* <span data-ttu-id="019b4-201">plugdev</span><span class="sxs-lookup"><span data-stu-id="019b4-201">plugdev</span></span>
* <span data-ttu-id="019b4-202">Proxy server</span><span class="sxs-lookup"><span data-stu-id="019b4-202">proxy</span></span>
* <span data-ttu-id="019b4-203">kořenové</span><span class="sxs-lookup"><span data-stu-id="019b4-203">root</span></span>
* <span data-ttu-id="019b4-204">SASL</span><span class="sxs-lookup"><span data-stu-id="019b4-204">sasl</span></span>
* <span data-ttu-id="019b4-205">stínové</span><span class="sxs-lookup"><span data-stu-id="019b4-205">shadow</span></span>
* <span data-ttu-id="019b4-206">src</span><span class="sxs-lookup"><span data-stu-id="019b4-206">src</span></span>
* <span data-ttu-id="019b4-207">SSH</span><span class="sxs-lookup"><span data-stu-id="019b4-207">ssh</span></span>
* <span data-ttu-id="019b4-208">sshd</span><span class="sxs-lookup"><span data-stu-id="019b4-208">sshd</span></span>
* <span data-ttu-id="019b4-209">zaměstnanci</span><span class="sxs-lookup"><span data-stu-id="019b4-209">staff</span></span>
* <span data-ttu-id="019b4-210">sudo</span><span class="sxs-lookup"><span data-stu-id="019b4-210">sudo</span></span>
* <span data-ttu-id="019b4-211">Synchronizace</span><span class="sxs-lookup"><span data-stu-id="019b4-211">sync</span></span>
* <span data-ttu-id="019b4-212">Sys</span><span class="sxs-lookup"><span data-stu-id="019b4-212">sys</span></span>
* <span data-ttu-id="019b4-213">syslog</span><span class="sxs-lookup"><span data-stu-id="019b4-213">syslog</span></span>
* <span data-ttu-id="019b4-214">pásky</span><span class="sxs-lookup"><span data-stu-id="019b4-214">tape</span></span>
* <span data-ttu-id="019b4-215">TTY</span><span class="sxs-lookup"><span data-stu-id="019b4-215">tty</span></span>
* <span data-ttu-id="019b4-216">uživatelé</span><span class="sxs-lookup"><span data-stu-id="019b4-216">users</span></span>
* <span data-ttu-id="019b4-217">procesu utmp</span><span class="sxs-lookup"><span data-stu-id="019b4-217">utmp</span></span>
* <span data-ttu-id="019b4-218">UUCP</span><span class="sxs-lookup"><span data-stu-id="019b4-218">uucp</span></span>
* <span data-ttu-id="019b4-219">video</span><span class="sxs-lookup"><span data-stu-id="019b4-219">video</span></span>
* <span data-ttu-id="019b4-220">hlas</span><span class="sxs-lookup"><span data-stu-id="019b4-220">voice</span></span>
* <span data-ttu-id="019b4-221">whoopsie</span><span class="sxs-lookup"><span data-stu-id="019b4-221">whoopsie</span></span>
* <span data-ttu-id="019b4-222">WWW-data</span><span class="sxs-lookup"><span data-stu-id="019b4-222">www-data</span></span>

