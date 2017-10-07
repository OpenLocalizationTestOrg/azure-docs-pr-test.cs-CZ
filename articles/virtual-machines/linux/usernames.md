---
title: "aaaSelecting uživatelská jména pro Linux | Microsoft Docs"
description: "Zjistěte, jak se uživatel tooselect názvů pro virtuální počítač s Linuxem v Azure."
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
ms.openlocfilehash: c65e2ac46f40bb8c9d74cccbaf248a070c0fa6cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="selecting-user-names-for-linux-on-azure"></a><span data-ttu-id="7f8e8-103">Výběr uživatelských jmen pro Linux v Azure</span><span class="sxs-lookup"><span data-stu-id="7f8e8-103">Selecting User Names for Linux on Azure</span></span>
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="7f8e8-104">Při zřizování virtuální počítač s Linuxem v Azure musíte zadat název hello nekořenovými uživatele, které můžete později použít toolog do hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="7f8e8-104">When you provision a Linux virtual machine on Azure you must specify hello name of a non-root user that you can later use toolog into hello VM.</span></span> <span data-ttu-id="7f8e8-105">Můžete se rozhodnout hello název hello nového uživatele, nebo pokud zřizování prostřednictvím hello portál Azure classic můžete přijmout výchozí hello název "azureuser".</span><span class="sxs-lookup"><span data-stu-id="7f8e8-105">You may choose hello name of hello new user, or if provisioning via hello Azure classic portal you can accept hello default name "azureuser".</span></span>

<span data-ttu-id="7f8e8-106">Ve většině případů tento uživatel nebude existovat na základní image hello a vytvoří se během procesu zřizování hello.</span><span class="sxs-lookup"><span data-stu-id="7f8e8-106">In most cases this user won't exist on hello base image and will be created during hello provisioning process.</span></span> <span data-ttu-id="7f8e8-107">Pokud uživatel hello existuje na hello základní image virtuálního počítače, pak hello Azure Linux agent jednoduše nakonfiguruje hello heslo nebo klíč SSH pro tohoto uživatele na základě informací o hello, které jste zadali při vytváření hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="7f8e8-107">If hello user exists on hello base VM image, then hello Azure Linux agent simply configures hello password and/or SSH key for that user based on hello information you specified when creating hello VM.</span></span>

<span data-ttu-id="7f8e8-108">**Ale**, Linux definuje sadu uživatelská jména, která by se neměla používat.</span><span class="sxs-lookup"><span data-stu-id="7f8e8-108">**However**, Linux defines a set of user names that should not be used.</span></span> <span data-ttu-id="7f8e8-109">Zřizování se proces Hello **nezdaří** Pokud se pokusíte tooprovision virtuálního počítače s Linuxem pomocí stávajícího uživatele systému, která je definována jako uživatel s UID 0-99.</span><span class="sxs-lookup"><span data-stu-id="7f8e8-109">hello provisioning process will **fail** if you try tooprovision a Linux VM using an existing system user, which is defined as a user with UID 0-99.</span></span> <span data-ttu-id="7f8e8-110">Typickým příkladem je hello `root` uživatele, který má UID 0.</span><span class="sxs-lookup"><span data-stu-id="7f8e8-110">A typical example is hello `root` user, which has UID 0.</span></span>

* <span data-ttu-id="7f8e8-111">Viz také: [základní Linux Standard - rozsahy ID uživatele](http://refspecs.linuxfoundation.org/LSB_4.1.0/LSB-Core-generic/LSB-Core-generic/uidrange.html)</span><span class="sxs-lookup"><span data-stu-id="7f8e8-111">See also: [Linux Standard Base - User ID Ranges](http://refspecs.linuxfoundation.org/LSB_4.1.0/LSB-Core-generic/LSB-Core-generic/uidrange.html)</span></span>

<span data-ttu-id="7f8e8-112">Hello následuje seznam běžných uživatelů předdefinované systému CentOS a Ubuntu, že byste neměli používat při zřizování virtuální počítač s Linuxem v Azure.</span><span class="sxs-lookup"><span data-stu-id="7f8e8-112">hello following is a list of common built-in system users for CentOS and Ubuntu that you should avoid using when provisioning a Linux virtual machine on Azure.</span></span> <span data-ttu-id="7f8e8-113">Tento seznam je jenom jako příklad naleznete v dokumentaci toohello pro vaše tooensure distribuční tímto uživatelským jménem, které zvolíte není v konfliktu s existujícím uživatelem systému hello.</span><span class="sxs-lookup"><span data-stu-id="7f8e8-113">This list is just an example, please refer toohello documentation for your distribution tooensure that hello username you choose does not conflict with an existing system user.</span></span>

## <a name="centos"></a><span data-ttu-id="7f8e8-114">CentOS</span><span class="sxs-lookup"><span data-stu-id="7f8e8-114">CentOS</span></span>
* <span data-ttu-id="7f8e8-115">abrt</span><span class="sxs-lookup"><span data-stu-id="7f8e8-115">abrt</span></span>
* <span data-ttu-id="7f8e8-116">ADM</span><span class="sxs-lookup"><span data-stu-id="7f8e8-116">adm</span></span>
* <span data-ttu-id="7f8e8-117">zvuk</span><span class="sxs-lookup"><span data-stu-id="7f8e8-117">audio</span></span>
* <span data-ttu-id="7f8e8-118">Koš</span><span class="sxs-lookup"><span data-stu-id="7f8e8-118">bin</span></span>
* <span data-ttu-id="7f8e8-119">CD-ROM</span><span class="sxs-lookup"><span data-stu-id="7f8e8-119">cdrom</span></span>
* <span data-ttu-id="7f8e8-120">cgred</span><span class="sxs-lookup"><span data-stu-id="7f8e8-120">cgred</span></span>
* <span data-ttu-id="7f8e8-121">Démon procesu</span><span class="sxs-lookup"><span data-stu-id="7f8e8-121">daemon</span></span>
* <span data-ttu-id="7f8e8-122">dbus</span><span class="sxs-lookup"><span data-stu-id="7f8e8-122">dbus</span></span>
* <span data-ttu-id="7f8e8-123">síti službou</span><span class="sxs-lookup"><span data-stu-id="7f8e8-123">dialout</span></span>
* <span data-ttu-id="7f8e8-124">vyhrazené IP adresy</span><span class="sxs-lookup"><span data-stu-id="7f8e8-124">dip</span></span>
* <span data-ttu-id="7f8e8-125">disk</span><span class="sxs-lookup"><span data-stu-id="7f8e8-125">disk</span></span>
* <span data-ttu-id="7f8e8-126">disketovou</span><span class="sxs-lookup"><span data-stu-id="7f8e8-126">floppy</span></span>
* <span data-ttu-id="7f8e8-127">FTP</span><span class="sxs-lookup"><span data-stu-id="7f8e8-127">ftp</span></span>
* <span data-ttu-id="7f8e8-128">FTP</span><span class="sxs-lookup"><span data-stu-id="7f8e8-128">ftp</span></span>
* <span data-ttu-id="7f8e8-129">hry</span><span class="sxs-lookup"><span data-stu-id="7f8e8-129">games</span></span>
* <span data-ttu-id="7f8e8-130">Gopher</span><span class="sxs-lookup"><span data-stu-id="7f8e8-130">gopher</span></span>
* <span data-ttu-id="7f8e8-131">haldaemon</span><span class="sxs-lookup"><span data-stu-id="7f8e8-131">haldaemon</span></span>
* <span data-ttu-id="7f8e8-132">zastavení</span><span class="sxs-lookup"><span data-stu-id="7f8e8-132">halt</span></span>
* <span data-ttu-id="7f8e8-133">kmem</span><span class="sxs-lookup"><span data-stu-id="7f8e8-133">kmem</span></span>
* <span data-ttu-id="7f8e8-134">Zámek</span><span class="sxs-lookup"><span data-stu-id="7f8e8-134">lock</span></span>
* <span data-ttu-id="7f8e8-135">lineárního programování úloh</span><span class="sxs-lookup"><span data-stu-id="7f8e8-135">lp</span></span>
* <span data-ttu-id="7f8e8-136">E-mailu</span><span class="sxs-lookup"><span data-stu-id="7f8e8-136">mail</span></span>
* <span data-ttu-id="7f8e8-137">Man</span><span class="sxs-lookup"><span data-stu-id="7f8e8-137">man</span></span>
* <span data-ttu-id="7f8e8-138">mem</span><span class="sxs-lookup"><span data-stu-id="7f8e8-138">mem</span></span>
* <span data-ttu-id="7f8e8-139">nfsnobody</span><span class="sxs-lookup"><span data-stu-id="7f8e8-139">nfsnobody</span></span>
* <span data-ttu-id="7f8e8-140">nikdo</span><span class="sxs-lookup"><span data-stu-id="7f8e8-140">nobody</span></span>
* <span data-ttu-id="7f8e8-141">NTP</span><span class="sxs-lookup"><span data-stu-id="7f8e8-141">ntp</span></span>
* <span data-ttu-id="7f8e8-142">Operátor</span><span class="sxs-lookup"><span data-stu-id="7f8e8-142">operator</span></span>
* <span data-ttu-id="7f8e8-143">oprofile</span><span class="sxs-lookup"><span data-stu-id="7f8e8-143">oprofile</span></span>
* <span data-ttu-id="7f8e8-144">postdrop</span><span class="sxs-lookup"><span data-stu-id="7f8e8-144">postdrop</span></span>
* <span data-ttu-id="7f8e8-145">operátory přípony</span><span class="sxs-lookup"><span data-stu-id="7f8e8-145">postfix</span></span>
* <span data-ttu-id="7f8e8-146">qpidd</span><span class="sxs-lookup"><span data-stu-id="7f8e8-146">qpidd</span></span>
* <span data-ttu-id="7f8e8-147">kořenové</span><span class="sxs-lookup"><span data-stu-id="7f8e8-147">root</span></span>
* <span data-ttu-id="7f8e8-148">RPC</span><span class="sxs-lookup"><span data-stu-id="7f8e8-148">rpc</span></span>
* <span data-ttu-id="7f8e8-149">rpcuser</span><span class="sxs-lookup"><span data-stu-id="7f8e8-149">rpcuser</span></span>
* <span data-ttu-id="7f8e8-150">saslauth</span><span class="sxs-lookup"><span data-stu-id="7f8e8-150">saslauth</span></span>
* <span data-ttu-id="7f8e8-151">Vypnutí</span><span class="sxs-lookup"><span data-stu-id="7f8e8-151">shutdown</span></span>
* <span data-ttu-id="7f8e8-152">slocate</span><span class="sxs-lookup"><span data-stu-id="7f8e8-152">slocate</span></span>
* <span data-ttu-id="7f8e8-153">sshd</span><span class="sxs-lookup"><span data-stu-id="7f8e8-153">sshd</span></span>
* <span data-ttu-id="7f8e8-154">stapdev</span><span class="sxs-lookup"><span data-stu-id="7f8e8-154">stapdev</span></span>
* <span data-ttu-id="7f8e8-155">stapusr</span><span class="sxs-lookup"><span data-stu-id="7f8e8-155">stapusr</span></span>
* <span data-ttu-id="7f8e8-156">Synchronizace</span><span class="sxs-lookup"><span data-stu-id="7f8e8-156">sync</span></span>
* <span data-ttu-id="7f8e8-157">Sys</span><span class="sxs-lookup"><span data-stu-id="7f8e8-157">sys</span></span>
* <span data-ttu-id="7f8e8-158">pásky</span><span class="sxs-lookup"><span data-stu-id="7f8e8-158">tape</span></span>
* <span data-ttu-id="7f8e8-159">Test</span><span class="sxs-lookup"><span data-stu-id="7f8e8-159">test</span></span>
* <span data-ttu-id="7f8e8-160">tcpdump</span><span class="sxs-lookup"><span data-stu-id="7f8e8-160">tcpdump</span></span>
* <span data-ttu-id="7f8e8-161">TTY</span><span class="sxs-lookup"><span data-stu-id="7f8e8-161">tty</span></span>
* <span data-ttu-id="7f8e8-162">uživatelé</span><span class="sxs-lookup"><span data-stu-id="7f8e8-162">users</span></span>
* <span data-ttu-id="7f8e8-163">utempter</span><span class="sxs-lookup"><span data-stu-id="7f8e8-163">utempter</span></span>
* <span data-ttu-id="7f8e8-164">procesu utmp</span><span class="sxs-lookup"><span data-stu-id="7f8e8-164">utmp</span></span>
* <span data-ttu-id="7f8e8-165">UUCP</span><span class="sxs-lookup"><span data-stu-id="7f8e8-165">uucp</span></span>
* <span data-ttu-id="7f8e8-166">vcsa</span><span class="sxs-lookup"><span data-stu-id="7f8e8-166">vcsa</span></span>
* <span data-ttu-id="7f8e8-167">video</span><span class="sxs-lookup"><span data-stu-id="7f8e8-167">video</span></span>
* <span data-ttu-id="7f8e8-168">Wheel</span><span class="sxs-lookup"><span data-stu-id="7f8e8-168">wheel</span></span>

## <a name="ubuntu"></a><span data-ttu-id="7f8e8-169">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="7f8e8-169">Ubuntu</span></span>
* <span data-ttu-id="7f8e8-170">ADM</span><span class="sxs-lookup"><span data-stu-id="7f8e8-170">adm</span></span>
* <span data-ttu-id="7f8e8-171">Správce</span><span class="sxs-lookup"><span data-stu-id="7f8e8-171">admin</span></span>
* <span data-ttu-id="7f8e8-172">zvuk</span><span class="sxs-lookup"><span data-stu-id="7f8e8-172">audio</span></span>
* <span data-ttu-id="7f8e8-173">zálohování</span><span class="sxs-lookup"><span data-stu-id="7f8e8-173">backup</span></span>
* <span data-ttu-id="7f8e8-174">Koš</span><span class="sxs-lookup"><span data-stu-id="7f8e8-174">bin</span></span>
* <span data-ttu-id="7f8e8-175">CD-ROM</span><span class="sxs-lookup"><span data-stu-id="7f8e8-175">cdrom</span></span>
* <span data-ttu-id="7f8e8-176">crontab</span><span class="sxs-lookup"><span data-stu-id="7f8e8-176">crontab</span></span>
* <span data-ttu-id="7f8e8-177">Démon procesu</span><span class="sxs-lookup"><span data-stu-id="7f8e8-177">daemon</span></span>
* <span data-ttu-id="7f8e8-178">síti službou</span><span class="sxs-lookup"><span data-stu-id="7f8e8-178">dialout</span></span>
* <span data-ttu-id="7f8e8-179">vyhrazené IP adresy</span><span class="sxs-lookup"><span data-stu-id="7f8e8-179">dip</span></span>
* <span data-ttu-id="7f8e8-180">disk</span><span class="sxs-lookup"><span data-stu-id="7f8e8-180">disk</span></span>
* <span data-ttu-id="7f8e8-181">Fax</span><span class="sxs-lookup"><span data-stu-id="7f8e8-181">fax</span></span>
* <span data-ttu-id="7f8e8-182">disketovou</span><span class="sxs-lookup"><span data-stu-id="7f8e8-182">floppy</span></span>
* <span data-ttu-id="7f8e8-183">pojistka</span><span class="sxs-lookup"><span data-stu-id="7f8e8-183">fuse</span></span>
* <span data-ttu-id="7f8e8-184">hry</span><span class="sxs-lookup"><span data-stu-id="7f8e8-184">games</span></span>
* <span data-ttu-id="7f8e8-185">gnats</span><span class="sxs-lookup"><span data-stu-id="7f8e8-185">gnats</span></span>
* <span data-ttu-id="7f8e8-186">IRC</span><span class="sxs-lookup"><span data-stu-id="7f8e8-186">irc</span></span>
* <span data-ttu-id="7f8e8-187">kmem</span><span class="sxs-lookup"><span data-stu-id="7f8e8-187">kmem</span></span>
* <span data-ttu-id="7f8e8-188">na šířku</span><span class="sxs-lookup"><span data-stu-id="7f8e8-188">landscape</span></span>
* <span data-ttu-id="7f8e8-189">libuuid</span><span class="sxs-lookup"><span data-stu-id="7f8e8-189">libuuid</span></span>
* <span data-ttu-id="7f8e8-190">seznam</span><span class="sxs-lookup"><span data-stu-id="7f8e8-190">list</span></span>
* <span data-ttu-id="7f8e8-191">lineárního programování úloh</span><span class="sxs-lookup"><span data-stu-id="7f8e8-191">lp</span></span>
* <span data-ttu-id="7f8e8-192">E-mailu</span><span class="sxs-lookup"><span data-stu-id="7f8e8-192">mail</span></span>
* <span data-ttu-id="7f8e8-193">Man</span><span class="sxs-lookup"><span data-stu-id="7f8e8-193">man</span></span>
* <span data-ttu-id="7f8e8-194">MessageBus</span><span class="sxs-lookup"><span data-stu-id="7f8e8-194">messagebus</span></span>
* <span data-ttu-id="7f8e8-195">mlocate</span><span class="sxs-lookup"><span data-stu-id="7f8e8-195">mlocate</span></span>
* <span data-ttu-id="7f8e8-196">netdev</span><span class="sxs-lookup"><span data-stu-id="7f8e8-196">netdev</span></span>
* <span data-ttu-id="7f8e8-197">Novinky</span><span class="sxs-lookup"><span data-stu-id="7f8e8-197">news</span></span>
* <span data-ttu-id="7f8e8-198">nikdo</span><span class="sxs-lookup"><span data-stu-id="7f8e8-198">nobody</span></span>
* <span data-ttu-id="7f8e8-199">"nogroup"</span><span class="sxs-lookup"><span data-stu-id="7f8e8-199">nogroup</span></span>
* <span data-ttu-id="7f8e8-200">Operátor</span><span class="sxs-lookup"><span data-stu-id="7f8e8-200">operator</span></span>
* <span data-ttu-id="7f8e8-201">plugdev</span><span class="sxs-lookup"><span data-stu-id="7f8e8-201">plugdev</span></span>
* <span data-ttu-id="7f8e8-202">Proxy server</span><span class="sxs-lookup"><span data-stu-id="7f8e8-202">proxy</span></span>
* <span data-ttu-id="7f8e8-203">kořenové</span><span class="sxs-lookup"><span data-stu-id="7f8e8-203">root</span></span>
* <span data-ttu-id="7f8e8-204">SASL</span><span class="sxs-lookup"><span data-stu-id="7f8e8-204">sasl</span></span>
* <span data-ttu-id="7f8e8-205">stínové</span><span class="sxs-lookup"><span data-stu-id="7f8e8-205">shadow</span></span>
* <span data-ttu-id="7f8e8-206">src</span><span class="sxs-lookup"><span data-stu-id="7f8e8-206">src</span></span>
* <span data-ttu-id="7f8e8-207">SSH</span><span class="sxs-lookup"><span data-stu-id="7f8e8-207">ssh</span></span>
* <span data-ttu-id="7f8e8-208">sshd</span><span class="sxs-lookup"><span data-stu-id="7f8e8-208">sshd</span></span>
* <span data-ttu-id="7f8e8-209">zaměstnanci</span><span class="sxs-lookup"><span data-stu-id="7f8e8-209">staff</span></span>
* <span data-ttu-id="7f8e8-210">sudo</span><span class="sxs-lookup"><span data-stu-id="7f8e8-210">sudo</span></span>
* <span data-ttu-id="7f8e8-211">Synchronizace</span><span class="sxs-lookup"><span data-stu-id="7f8e8-211">sync</span></span>
* <span data-ttu-id="7f8e8-212">Sys</span><span class="sxs-lookup"><span data-stu-id="7f8e8-212">sys</span></span>
* <span data-ttu-id="7f8e8-213">syslog</span><span class="sxs-lookup"><span data-stu-id="7f8e8-213">syslog</span></span>
* <span data-ttu-id="7f8e8-214">pásky</span><span class="sxs-lookup"><span data-stu-id="7f8e8-214">tape</span></span>
* <span data-ttu-id="7f8e8-215">TTY</span><span class="sxs-lookup"><span data-stu-id="7f8e8-215">tty</span></span>
* <span data-ttu-id="7f8e8-216">uživatelé</span><span class="sxs-lookup"><span data-stu-id="7f8e8-216">users</span></span>
* <span data-ttu-id="7f8e8-217">procesu utmp</span><span class="sxs-lookup"><span data-stu-id="7f8e8-217">utmp</span></span>
* <span data-ttu-id="7f8e8-218">UUCP</span><span class="sxs-lookup"><span data-stu-id="7f8e8-218">uucp</span></span>
* <span data-ttu-id="7f8e8-219">video</span><span class="sxs-lookup"><span data-stu-id="7f8e8-219">video</span></span>
* <span data-ttu-id="7f8e8-220">hlas</span><span class="sxs-lookup"><span data-stu-id="7f8e8-220">voice</span></span>
* <span data-ttu-id="7f8e8-221">whoopsie</span><span class="sxs-lookup"><span data-stu-id="7f8e8-221">whoopsie</span></span>
* <span data-ttu-id="7f8e8-222">WWW-data</span><span class="sxs-lookup"><span data-stu-id="7f8e8-222">www-data</span></span>

