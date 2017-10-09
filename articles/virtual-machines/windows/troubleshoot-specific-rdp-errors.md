---
title: "aaaSpecific RDP chybové zprávy pro virtuální počítače Azure | Microsoft Docs"
description: "Srozumitelná specifické chybové zprávy, které se mohou zobrazit při pokusu o použití vzdálené plochy připojení tooa Windows virtuálního počítače v Azure"
keywords: "Vzdálené plochy chyba, Chyba připojení ke vzdálené ploše, nelze se připojit tooVM, řešení potíží vzdálené plochy"
services: virtual-machines-windows
documentationcenter: 
author: genlin
manager: timlt
editor: 
tags: top-support-issue,azure-service-management,azure-resource-manager
ms.assetid: 5feb1d64-ee6f-4907-949a-a7cffcbc6153
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: support-article
ms.date: 05/26/2017
ms.author: genli
ms.openlocfilehash: 8e1551be23e696bd60adbd76c3e1ea86d9dd11aa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-specific-rdp-error-messages-tooa-windows-vm-in-azure"></a><span data-ttu-id="fe02e-104">Řešení potíží s konkrétní RDP chybové zprávy tooa virtuální počítač s Windows v Azure</span><span class="sxs-lookup"><span data-stu-id="fe02e-104">Troubleshooting specific RDP error messages tooa Windows VM in Azure</span></span>
<span data-ttu-id="fe02e-105">Když v Azure pomocí vzdálené plochy připojení tooa Windows virtuální počítač (VM), může se zobrazit konkrétní chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="fe02e-105">You may receive a specific error message when using Remote Desktop connection tooa Windows virtual machine (VM) in Azure.</span></span> <span data-ttu-id="fe02e-106">Tento článek podrobnosti některé z nejběžnějších chybových zpráv došlo, společně s řešení potíží s kroky tooresolve hello je.</span><span class="sxs-lookup"><span data-stu-id="fe02e-106">This article details some of hello more common error messages encountered, along with troubleshooting steps tooresolve them.</span></span> <span data-ttu-id="fe02e-107">Pokud máte problémy s připojením tooyour virtuálních počítačů pomocí protokolu RDP ale proveďte není stane konkrétní chybová zpráva, přečtěte si hello [Průvodce řešením potíží pro vzdálené plochy](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="fe02e-107">If you are having issues connecting tooyour VM using RDP but do not encounter a specific error message, see hello [troubleshooting guide for Remote Desktop](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="fe02e-108">Informace o specifické chybové zprávy najdete v části hello následující:</span><span class="sxs-lookup"><span data-stu-id="fe02e-108">For information on specific error messages, see hello following:</span></span>

* <span data-ttu-id="fe02e-109">[Hello relace byla odpojena, protože neexistují žádné licenční servery vzdálené plochy k dispozici tooprovide licenci](#rdplicense).</span><span class="sxs-lookup"><span data-stu-id="fe02e-109">[hello remote session was disconnected because there are no Remote Desktop License Servers available tooprovide a license](#rdplicense).</span></span>
* <span data-ttu-id="fe02e-110">[Vzdálená plocha nenašla hello počítač "název"](#rdpname).</span><span class="sxs-lookup"><span data-stu-id="fe02e-110">[Remote Desktop can't find hello computer "name"](#rdpname).</span></span>
* <span data-ttu-id="fe02e-111">[Došlo k chybě ověřování. Hello místní úřad zabezpečení nelze kontaktovat](#rdpauth).</span><span class="sxs-lookup"><span data-stu-id="fe02e-111">[An authentication error has occurred. hello Local Security Authority cannot be contacted](#rdpauth).</span></span>
* <span data-ttu-id="fe02e-112">[Chyba zabezpečení systému Windows: přihlašovací údaje nebyla úspěšná](#wincred).</span><span class="sxs-lookup"><span data-stu-id="fe02e-112">[Windows Security error: Your credentials did not work](#wincred).</span></span>
* <span data-ttu-id="fe02e-113">[Počítač se nemůže připojit vzdálený počítač toohello](#rdpconnect).</span><span class="sxs-lookup"><span data-stu-id="fe02e-113">[This computer can't connect toohello remote computer](#rdpconnect).</span></span>

<a id="rdplicense"></a>

## <a name="hello-remote-session-was-disconnected-because-there-are-no-remote-desktop-license-servers-available-tooprovide-a-license"></a><span data-ttu-id="fe02e-114">Hello relace byla odpojena, protože nejsou k dispozici žádné licenční servery vzdálené plochy k dispozici tooprovide licenci.</span><span class="sxs-lookup"><span data-stu-id="fe02e-114">hello remote session was disconnected because there are no Remote Desktop License Servers available tooprovide a license.</span></span>
<span data-ttu-id="fe02e-115">Příčina: hello 120 licencování dní pro roli serveru vzdálené plochy hello vypršela a je třeba tooinstall licence.</span><span class="sxs-lookup"><span data-stu-id="fe02e-115">Cause: hello 120-day licensing grace period for hello Remote Desktop Server role has expired and you need tooinstall licenses.</span></span>

<span data-ttu-id="fe02e-116">Jako alternativní řešení uloží místní kopii souboru RDP hello z portálu hello a spustit tento příkaz příkazovém řádku tooconnect prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="fe02e-116">As a workaround, save a local copy of hello RDP file from hello portal and run this command at a PowerShell command prompt tooconnect.</span></span> <span data-ttu-id="fe02e-117">Tento krok zakazuje licencování pro právě toto připojení:</span><span class="sxs-lookup"><span data-stu-id="fe02e-117">This step disables licensing for just that connection:</span></span>

        mstsc <File name>.RDP /admin

<span data-ttu-id="fe02e-118">Pokud nepotřebujete ve skutečnosti víc než dvou souběžných připojení ke vzdálené ploše připojení toohello virtuálních počítačů, můžete roli serveru vzdálené plochy hello tooremove správce serveru.</span><span class="sxs-lookup"><span data-stu-id="fe02e-118">If you don't actually need more than two simultaneous Remote Desktop connections toohello VM, you can use Server Manager tooremove hello Remote Desktop Server role.</span></span>

<span data-ttu-id="fe02e-119">Další informace najdete v tématu hello blogu [virtuálního počítače Azure se nezdaří s "Vzdálené plochy licence k dispozici žádné servery"](https://blogs.msdn.microsoft.com/mast/2014/01/21/rdp-to-azure-vm-fails-with-no-remote-desktop-license-servers-available/).</span><span class="sxs-lookup"><span data-stu-id="fe02e-119">For more information, see hello blog post [Azure VM fails with "No Remote Desktop License Servers available"](https://blogs.msdn.microsoft.com/mast/2014/01/21/rdp-to-azure-vm-fails-with-no-remote-desktop-license-servers-available/).</span></span>

<a id="rdpname"></a>

## <a name="remote-desktop-cant-find-hello-computer-name"></a><span data-ttu-id="fe02e-120">Vzdálená plocha nenašla počítač hello "name".</span><span class="sxs-lookup"><span data-stu-id="fe02e-120">Remote Desktop can't find hello computer "name".</span></span>
<span data-ttu-id="fe02e-121">Příčina: hello klienta vzdálené plochy ve vašem počítači nelze přeložit název hello hello počítače v hello nastavení souboru protokolu RDP hello.</span><span class="sxs-lookup"><span data-stu-id="fe02e-121">Cause: hello Remote Desktop client on your computer can't resolve hello name of hello computer in hello settings of hello RDP file.</span></span>

<span data-ttu-id="fe02e-122">Možná řešení:</span><span class="sxs-lookup"><span data-stu-id="fe02e-122">Possible solutions:</span></span>

* <span data-ttu-id="fe02e-123">Pokud jste na intranetu organizace, ujistěte se, že váš počítač má přístup toohello proxy server a může odesílat tooit provoz HTTPS.</span><span class="sxs-lookup"><span data-stu-id="fe02e-123">If you're on an organization's intranet, make sure that your computer has access toohello proxy server and can send HTTPS traffic tooit.</span></span>
* <span data-ttu-id="fe02e-124">Pokud používáte místně uložených souborů protokolu RDP, zkuste použít hello jeden, který je generovaný hello portálu.</span><span class="sxs-lookup"><span data-stu-id="fe02e-124">If you're using a locally stored RDP file, try using hello one that's generated by hello portal.</span></span> <span data-ttu-id="fe02e-125">Tento krok zajistí, že máte hello správný název DNS pro hello virtuálního počítače, nebo hello Cloudová služba a port koncového bodu hello hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="fe02e-125">This step ensures that you have hello correct DNS name for hello virtual machine, or hello cloud service and hello endpoint port of hello VM.</span></span> <span data-ttu-id="fe02e-126">Tady je ukázkový soubor RDP generované hello portálu:</span><span class="sxs-lookup"><span data-stu-id="fe02e-126">Here is a sample RDP file generated by hello portal:</span></span>
  
        full address:s:tailspin-azdatatier.cloudapp.net:55919
        prompt for credentials:i:1

<span data-ttu-id="fe02e-127">část adresy Hello tento soubor RDP má:</span><span class="sxs-lookup"><span data-stu-id="fe02e-127">hello address portion of this RDP file has:</span></span>

* <span data-ttu-id="fe02e-128">Hello plně kvalifikovaný název domény hello cloudové služby, který obsahuje hello virtuálních počítačů ("tailspin-azdatatier.cloudapp.net" v tomto příkladu).</span><span class="sxs-lookup"><span data-stu-id="fe02e-128">hello fully qualified domain name of hello cloud service that contains hello VM ("tailspin-azdatatier.cloudapp.net" in this example).</span></span>
* <span data-ttu-id="fe02e-129">Hello externí port TCP hello koncového bodu pro přenosy vzdálené plochy (55919).</span><span class="sxs-lookup"><span data-stu-id="fe02e-129">hello external TCP port of hello endpoint for Remote Desktop traffic (55919).</span></span>

<a id="rdpauth"></a>

## <a name="an-authentication-error-has-occurred-hello-local-security-authority-cannot-be-contacted"></a><span data-ttu-id="fe02e-130">Došlo k chybě ověřování.</span><span class="sxs-lookup"><span data-stu-id="fe02e-130">An authentication error has occurred.</span></span> <span data-ttu-id="fe02e-131">Nelze kontaktovat Hello místní úřad zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="fe02e-131">hello Local Security Authority cannot be contacted.</span></span>
<span data-ttu-id="fe02e-132">Příčina: hello cílovém virtuálním počítači nelze najít úřad zabezpečení hello v hello část názvu uživatelské přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="fe02e-132">Cause: hello target VM can't locate hello security authority in hello user name portion of your credentials.</span></span>

<span data-ttu-id="fe02e-133">Pokud je vaše uživatelské jméno v podobě hello *SecurityAuthority*\\*uživatelské jméno* (Příklad: CORP\User1), hello *SecurityAuthority* část je buď systém hello virtuálních počítačů název počítače (pro hello místní úřad zabezpečení) nebo název domény služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="fe02e-133">When your user name is in hello form *SecurityAuthority*\\*UserName* (example: CORP\User1), hello *SecurityAuthority* portion is either hello VM's computer name (for hello local security authority) or an Active Directory domain name.</span></span>

<span data-ttu-id="fe02e-134">Možná řešení:</span><span class="sxs-lookup"><span data-stu-id="fe02e-134">Possible solutions:</span></span>

* <span data-ttu-id="fe02e-135">Pokud je účet hello místní toohello virtuálních počítačů, zkontrolujte, zda že tento název virtuálního počítače hello je napsán správně.</span><span class="sxs-lookup"><span data-stu-id="fe02e-135">If hello account is local toohello VM, make sure that hello VM name is spelled correctly.</span></span>
* <span data-ttu-id="fe02e-136">Pokud hello účet v doméně služby Active Directory, zkontrolujte pravopis hello hello názvu domény.</span><span class="sxs-lookup"><span data-stu-id="fe02e-136">If hello account is on an Active Directory domain, check hello spelling of hello domain name.</span></span>
* <span data-ttu-id="fe02e-137">Pokud je účet domény služby Active Directory a název domény hello je napsán správně, ověřte, zda je řadič domény k dispozici v této doméně.</span><span class="sxs-lookup"><span data-stu-id="fe02e-137">If it is an Active Directory domain account and hello domain name is spelled correctly, verify that a domain controller is available in that domain.</span></span> <span data-ttu-id="fe02e-138">Je běžné problémy, se v Azure virtuální sítě, které obsahují řadiče domény, řadiče domény nedostupný, protože nebylo spuštěno.</span><span class="sxs-lookup"><span data-stu-id="fe02e-138">It's a common issue in Azure virtual networks that contain domain controllers that a domain controller is unavailable because it hasn't been started.</span></span> <span data-ttu-id="fe02e-139">Jako řešení můžete místo účtu domény místního účtu správce.</span><span class="sxs-lookup"><span data-stu-id="fe02e-139">As a workaround, you can use a local administrator account instead of a domain account.</span></span>

<a id="wincred"></a>

## <a name="windows-security-error-your-credentials-did-not-work"></a><span data-ttu-id="fe02e-140">Chyba zabezpečení systému Windows: přihlašovací údaje nebyla úspěšná.</span><span class="sxs-lookup"><span data-stu-id="fe02e-140">Windows Security error: Your credentials did not work.</span></span>
<span data-ttu-id="fe02e-141">Příčina: cíl hello virtuálního počítače nelze ověřit název účtu a heslo.</span><span class="sxs-lookup"><span data-stu-id="fe02e-141">Cause: hello target VM can't validate your account name and password.</span></span>

<span data-ttu-id="fe02e-142">Počítače se systémem Windows můžete ověřit přihlašovací údaje hello místní účet nebo účet domény.</span><span class="sxs-lookup"><span data-stu-id="fe02e-142">A Windows-based computer can validate hello credentials of either a local account or a domain account.</span></span>

* <span data-ttu-id="fe02e-143">Pro místní účty, použijte hello *ComputerName*\\*uživatelské jméno* syntaxe (Příklad: SQL1\Admin4798).</span><span class="sxs-lookup"><span data-stu-id="fe02e-143">For local accounts, use hello *ComputerName*\\*UserName* syntax (example: SQL1\Admin4798).</span></span>
* <span data-ttu-id="fe02e-144">Pro účty domén, použijte hello *DomainName*\\*uživatelské jméno* syntaxe (Příklad: CONTOSO\peterodman).</span><span class="sxs-lookup"><span data-stu-id="fe02e-144">For domain accounts, use hello *DomainName*\\*UserName* syntax (example: CONTOSO\peterodman).</span></span>

<span data-ttu-id="fe02e-145">Pokud jste zvýšili řadiče domény tooa virtuálních počítačů v nové doménové struktury služby Active Directory, je převést hello účet místního správce, který jste se přihlásili tooan ekvivalentní účet s hello stejné heslo v hello nové doménové struktury a domény.</span><span class="sxs-lookup"><span data-stu-id="fe02e-145">If you have promoted your VM tooa domain controller in a new Active Directory forest, hello local administrator account that you signed in with is converted tooan equivalent account with hello same password in hello new forest and domain.</span></span> <span data-ttu-id="fe02e-146">místní účet Hello se odstraní.</span><span class="sxs-lookup"><span data-stu-id="fe02e-146">hello local account is then deleted.</span></span>

<span data-ttu-id="fe02e-147">Například pokud jste přihlášeni s použitím místní účet hello DC1\DCAdmin a poté vyzval hello virtuální počítač jako řadič domény v nové doménové struktury pro hello doméně corp.contoso.com, hello DC1\DCAdmin místní účet se odstranila a nový účet domény (CORP\DCAdmin ) je vytvořen s hello stejné heslo.</span><span class="sxs-lookup"><span data-stu-id="fe02e-147">For example, if you signed in with hello local account DC1\DCAdmin, and then promoted hello virtual machine as a domain controller in a new forest for hello corp.contoso.com domain, hello DC1\DCAdmin local account gets deleted and a new domain account (CORP\DCAdmin) is created with hello same password.</span></span>

<span data-ttu-id="fe02e-148">Ujistěte se, že název účtu hello je název hello virtuálního počítače můžete ověřit jako účet platný, a je zadáno správné heslo tohoto hello.</span><span class="sxs-lookup"><span data-stu-id="fe02e-148">Make sure that hello account name is a name that hello virtual machine can verify as a valid account, and that hello password is correct.</span></span>

<span data-ttu-id="fe02e-149">Pokud potřebujete toochange hello heslo účtu místního správce hello, přečtěte si [jak tooreset heslo nebo hello vzdálené plochy služby pro virtuální počítače s Windows](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="fe02e-149">If you need toochange hello password of hello local administrator account, see [How tooreset a password or hello Remote Desktop service for Windows virtual machines](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<a id="rdpconnect"></a>

## <a name="this-computer-cant-connect-toohello-remote-computer"></a><span data-ttu-id="fe02e-150">Počítač se nemůže připojit toohello vzdáleného počítače.</span><span class="sxs-lookup"><span data-stu-id="fe02e-150">This computer can't connect toohello remote computer.</span></span>
<span data-ttu-id="fe02e-151">Příčina: hello účet, který byl použit tooconnect nemá vzdálené plochy přihlašovací práva.</span><span class="sxs-lookup"><span data-stu-id="fe02e-151">Cause: hello account that's used tooconnect does not have Remote Desktop sign-in rights.</span></span>

<span data-ttu-id="fe02e-152">Každý počítač se systémem Windows má skupinu místního uživatele vzdálené plochy, kterou obsahuje hello účty a skupiny, které můžete do ní vzdáleně.</span><span class="sxs-lookup"><span data-stu-id="fe02e-152">Every Windows computer has a Remote Desktop users local group, which contains hello accounts and groups that can sign into it remotely.</span></span> <span data-ttu-id="fe02e-153">Členové skupiny místní správci hello také mají přístup, i když tyto účty nejsou uvedené v místní skupině Uživatelé hello vzdálené plochy.</span><span class="sxs-lookup"><span data-stu-id="fe02e-153">Members of hello local administrators group also have access, even though those accounts are not listed in hello Remote Desktop users local group.</span></span> <span data-ttu-id="fe02e-154">Pro počítače připojené k doméně hello místní skupiny administrators také obsahuje hello správci domény pro doménu hello.</span><span class="sxs-lookup"><span data-stu-id="fe02e-154">For domain-joined machines, hello local administrators group also contains hello domain administrators for hello domain.</span></span>

<span data-ttu-id="fe02e-155">Ujistěte se, zda má účet hello, kterou používáte tooconnect pomocí vzdálené plochy přihlašovací práva.</span><span class="sxs-lookup"><span data-stu-id="fe02e-155">Make sure that hello account you're using tooconnect with has Remote Desktop sign-in rights.</span></span> <span data-ttu-id="fe02e-156">Jako řešení použijte vzdálenou plochu domény nebo tooconnect účet místního správce.</span><span class="sxs-lookup"><span data-stu-id="fe02e-156">As a workaround, use a domain or local administrator account tooconnect over Remote Desktop.</span></span> <span data-ttu-id="fe02e-157">tooadd hello požadovaný účet toohello vzdálené plochy uživatele místní skupiny, použijte modul snap-in konzoly Microsoft Management Console hello (**systémové nástroje > Místní uživatelé a skupiny > skupiny > Remote Desktop Users**).</span><span class="sxs-lookup"><span data-stu-id="fe02e-157">tooadd hello desired account toohello Remote Desktop users local group, use hello Microsoft Management Console snap-in (**System Tools > Local Users and Groups > Groups > Remote Desktop Users**).</span></span>

## <a name="next-steps"></a><span data-ttu-id="fe02e-158">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fe02e-158">Next steps</span></span>
<span data-ttu-id="fe02e-159">Pokud žádná z těchto chyb došlo k chybě a máte neznámé potíže s připojením pomocí protokolu RDP, najdete v části hello [Průvodce řešením potíží pro vzdálené plochy](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="fe02e-159">If none of these errors occurred and you have an unknown issue with connecting using RDP, see hello [troubleshooting guide for Remote Desktop](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

* <span data-ttu-id="fe02e-160">Řešení potíží s kroky v přístupu k aplikacím spuštěným na virtuálním počítači, najdete v části [Poradce při potížích přístup tooan aplikace běžící na virtuálním počítači Azure](../linux/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="fe02e-160">For troubleshooting steps in accessing applications running on a VM, see [Troubleshoot access tooan application running on an Azure VM](../linux/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
* <span data-ttu-id="fe02e-161">Pokud máte problémy s tooa tooconnect Secure Shell (SSH) virtuálního počítače s Linuxem v Azure, najdete v části [řešení SSH připojení tooa virtuálního počítače s Linuxem v Azure](../linux/troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="fe02e-161">If you are having issues using Secure Shell (SSH) tooconnect tooa Linux VM in Azure, see [Troubleshoot SSH connections tooa Linux VM in Azure](../linux/troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

