---
title: "Specifické chybové zprávy protokolu RDP pro virtuální počítače Azure | Microsoft Docs"
description: "Srozumitelná specifické chybové zprávy, které se mohou zobrazit při pokusu o použití připojení ke vzdálené ploše do virtuálního počítače s Windows v Azure"
keywords: "Vzdálené plochy chyba, Chyba připojení ke vzdálené ploše, nelze se připojit k virtuální počítač, řešení potíží vzdálené plochy"
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
ms.openlocfilehash: e7c049106726a15e96d4ebe7c7c0388a29c546c8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting-specific-rdp-error-messages-to-a-windows-vm-in-azure"></a><span data-ttu-id="0af19-104">Řešení potíží s konkrétní chybové zprávy protokolu RDP pro virtuální počítač s Windows v Azure</span><span class="sxs-lookup"><span data-stu-id="0af19-104">Troubleshooting specific RDP error messages to a Windows VM in Azure</span></span>
<span data-ttu-id="0af19-105">Při použití připojení ke vzdálené ploše na Windows virtuální počítač (VM) v Azure, může se zobrazit konkrétní chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="0af19-105">You may receive a specific error message when using Remote Desktop connection to a Windows virtual machine (VM) in Azure.</span></span> <span data-ttu-id="0af19-106">Tento článek podrobně popisuje některé z nejběžnějších chybových zpráv došlo, společně s řešení potíží s kroky k jejich řešení.</span><span class="sxs-lookup"><span data-stu-id="0af19-106">This article details some of the more common error messages encountered, along with troubleshooting steps to resolve them.</span></span> <span data-ttu-id="0af19-107">Pokud máte problémy s připojením k virtuálnímu počítači pomocí protokolu RDP ale proveďte není stane konkrétní chybová zpráva, přečtěte si [Průvodce řešením potíží pro vzdálené plochy](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="0af19-107">If you are having issues connecting to your VM using RDP but do not encounter a specific error message, see the [troubleshooting guide for Remote Desktop](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="0af19-108">Informace o specifické chybové zprávy naleznete v následujících tématech:</span><span class="sxs-lookup"><span data-stu-id="0af19-108">For information on specific error messages, see the following:</span></span>

* <span data-ttu-id="0af19-109">[Vzdálená relace byla odpojena, protože nejsou k dispozici vzdálené plochy licence k dispozici žádné servery k poskytnutí licence](#rdplicense).</span><span class="sxs-lookup"><span data-stu-id="0af19-109">[The remote session was disconnected because there are no Remote Desktop License Servers available to provide a license](#rdplicense).</span></span>
* <span data-ttu-id="0af19-110">[Vzdálená plocha nenašla počítač "název"](#rdpname).</span><span class="sxs-lookup"><span data-stu-id="0af19-110">[Remote Desktop can't find the computer "name"](#rdpname).</span></span>
* <span data-ttu-id="0af19-111">[Došlo k chybě ověřování. Místní úřad zabezpečení nelze kontaktovat](#rdpauth).</span><span class="sxs-lookup"><span data-stu-id="0af19-111">[An authentication error has occurred. The Local Security Authority cannot be contacted](#rdpauth).</span></span>
* <span data-ttu-id="0af19-112">[Chyba zabezpečení systému Windows: přihlašovací údaje nebyla úspěšná](#wincred).</span><span class="sxs-lookup"><span data-stu-id="0af19-112">[Windows Security error: Your credentials did not work](#wincred).</span></span>
* <span data-ttu-id="0af19-113">[Počítač se nemůže připojit ke vzdálenému počítači](#rdpconnect).</span><span class="sxs-lookup"><span data-stu-id="0af19-113">[This computer can't connect to the remote computer](#rdpconnect).</span></span>

<a id="rdplicense"></a>

## <a name="the-remote-session-was-disconnected-because-there-are-no-remote-desktop-license-servers-available-to-provide-a-license"></a><span data-ttu-id="0af19-114">Vzdálená relace byla odpojena, protože nejsou k dispozici vzdálené plochy licence k dispozici žádné servery k poskytnutí licence.</span><span class="sxs-lookup"><span data-stu-id="0af19-114">The remote session was disconnected because there are no Remote Desktop License Servers available to provide a license.</span></span>
<span data-ttu-id="0af19-115">Příčina: 120 licencování dní pro roli serveru vzdálené plochy vypršela platnost a budete muset nainstalovat licence.</span><span class="sxs-lookup"><span data-stu-id="0af19-115">Cause: The 120-day licensing grace period for the Remote Desktop Server role has expired and you need to install licenses.</span></span>

<span data-ttu-id="0af19-116">Jako alternativní řešení uloží místní kopii souboru RDP z portálu a spusťte tento příkaz na příkazovém řádku prostředí PowerShell pro připojení.</span><span class="sxs-lookup"><span data-stu-id="0af19-116">As a workaround, save a local copy of the RDP file from the portal and run this command at a PowerShell command prompt to connect.</span></span> <span data-ttu-id="0af19-117">Tento krok zakazuje licencování pro právě toto připojení:</span><span class="sxs-lookup"><span data-stu-id="0af19-117">This step disables licensing for just that connection:</span></span>

        mstsc <File name>.RDP /admin

<span data-ttu-id="0af19-118">Pokud nepotřebujete ve skutečnosti víc než dvou souběžných připojení ke vzdálené ploše do virtuálního počítače, můžete použít Správce serveru k odebrání role serveru vzdálené plochy.</span><span class="sxs-lookup"><span data-stu-id="0af19-118">If you don't actually need more than two simultaneous Remote Desktop connections to the VM, you can use Server Manager to remove the Remote Desktop Server role.</span></span>

<span data-ttu-id="0af19-119">Další informace naleznete v příspěvku blogu [virtuálního počítače Azure se nezdaří s "Vzdálené plochy licence k dispozici žádné servery"](https://blogs.msdn.microsoft.com/mast/2014/01/21/rdp-to-azure-vm-fails-with-no-remote-desktop-license-servers-available/).</span><span class="sxs-lookup"><span data-stu-id="0af19-119">For more information, see the blog post [Azure VM fails with "No Remote Desktop License Servers available"](https://blogs.msdn.microsoft.com/mast/2014/01/21/rdp-to-azure-vm-fails-with-no-remote-desktop-license-servers-available/).</span></span>

<a id="rdpname"></a>

## <a name="remote-desktop-cant-find-the-computer-name"></a><span data-ttu-id="0af19-120">Vzdálená plocha nenašla počítač "název".</span><span class="sxs-lookup"><span data-stu-id="0af19-120">Remote Desktop can't find the computer "name".</span></span>
<span data-ttu-id="0af19-121">Příčina: Klient vzdálené plochy ve vašem počítači nelze vyřešit název počítače v nastavení souboru protokolu RDP.</span><span class="sxs-lookup"><span data-stu-id="0af19-121">Cause: The Remote Desktop client on your computer can't resolve the name of the computer in the settings of the RDP file.</span></span>

<span data-ttu-id="0af19-122">Možná řešení:</span><span class="sxs-lookup"><span data-stu-id="0af19-122">Possible solutions:</span></span>

* <span data-ttu-id="0af19-123">Pokud jste na intranetu organizace, ujistěte se, že váš počítač má přístup k proxy serveru a mohou odesílat provoz HTTPS na ni.</span><span class="sxs-lookup"><span data-stu-id="0af19-123">If you're on an organization's intranet, make sure that your computer has access to the proxy server and can send HTTPS traffic to it.</span></span>
* <span data-ttu-id="0af19-124">Pokud používáte místně uložených souborů protokolu RDP, zkuste použít ten, který je generovaný na portálu.</span><span class="sxs-lookup"><span data-stu-id="0af19-124">If you're using a locally stored RDP file, try using the one that's generated by the portal.</span></span> <span data-ttu-id="0af19-125">Tento krok zajistí, že máte správný název DNS pro virtuální počítač, nebo cloudové služby a port koncový bod virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="0af19-125">This step ensures that you have the correct DNS name for the virtual machine, or the cloud service and the endpoint port of the VM.</span></span> <span data-ttu-id="0af19-126">Tady je ukázkový soubor RDP vytvořený portál:</span><span class="sxs-lookup"><span data-stu-id="0af19-126">Here is a sample RDP file generated by the portal:</span></span>
  
        full address:s:tailspin-azdatatier.cloudapp.net:55919
        prompt for credentials:i:1

<span data-ttu-id="0af19-127">Obsahuje části adresy tohoto souboru protokolu RDP:</span><span class="sxs-lookup"><span data-stu-id="0af19-127">The address portion of this RDP file has:</span></span>

* <span data-ttu-id="0af19-128">Název plně kvalifikované domény cloudové služby, která obsahuje virtuální počítač ("tailspin-azdatatier.cloudapp.net" v tomto příkladu).</span><span class="sxs-lookup"><span data-stu-id="0af19-128">The fully qualified domain name of the cloud service that contains the VM ("tailspin-azdatatier.cloudapp.net" in this example).</span></span>
* <span data-ttu-id="0af19-129">Externí port TCP koncového bodu pro přenosy vzdálené plochy (55919).</span><span class="sxs-lookup"><span data-stu-id="0af19-129">The external TCP port of the endpoint for Remote Desktop traffic (55919).</span></span>

<a id="rdpauth"></a>

## <a name="an-authentication-error-has-occurred-the-local-security-authority-cannot-be-contacted"></a><span data-ttu-id="0af19-130">Došlo k chybě ověřování.</span><span class="sxs-lookup"><span data-stu-id="0af19-130">An authentication error has occurred.</span></span> <span data-ttu-id="0af19-131">Místní úřad zabezpečení nelze kontaktovat.</span><span class="sxs-lookup"><span data-stu-id="0af19-131">The Local Security Authority cannot be contacted.</span></span>
<span data-ttu-id="0af19-132">Příčina: Cílový počítač nelze najít oprávnění zabezpečení v části název uživatelské přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="0af19-132">Cause: The target VM can't locate the security authority in the user name portion of your credentials.</span></span>

<span data-ttu-id="0af19-133">Pokud je vaše uživatelské jméno ve tvaru *SecurityAuthority*\\*uživatelské jméno* (Příklad: CORP\User1), *SecurityAuthority* část je buď Virtuálního počítače název počítače (pro místní autority zabezpečení) nebo název domény služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="0af19-133">When your user name is in the form *SecurityAuthority*\\*UserName* (example: CORP\User1), the *SecurityAuthority* portion is either the VM's computer name (for the local security authority) or an Active Directory domain name.</span></span>

<span data-ttu-id="0af19-134">Možná řešení:</span><span class="sxs-lookup"><span data-stu-id="0af19-134">Possible solutions:</span></span>

* <span data-ttu-id="0af19-135">Pokud je účet místního k virtuálnímu počítači, ujistěte se, že název virtuálního počítače je napsán správně.</span><span class="sxs-lookup"><span data-stu-id="0af19-135">If the account is local to the VM, make sure that the VM name is spelled correctly.</span></span>
* <span data-ttu-id="0af19-136">Pokud je účet v doméně služby Active Directory, zkontrolujte správnost názvu domény.</span><span class="sxs-lookup"><span data-stu-id="0af19-136">If the account is on an Active Directory domain, check the spelling of the domain name.</span></span>
* <span data-ttu-id="0af19-137">Pokud je účet domény služby Active Directory a názvu domény je napsán správně, ověřte, zda je řadič domény k dispozici v této doméně.</span><span class="sxs-lookup"><span data-stu-id="0af19-137">If it is an Active Directory domain account and the domain name is spelled correctly, verify that a domain controller is available in that domain.</span></span> <span data-ttu-id="0af19-138">Je běžné problémy, se v Azure virtuální sítě, které obsahují řadiče domény, řadiče domény nedostupný, protože nebylo spuštěno.</span><span class="sxs-lookup"><span data-stu-id="0af19-138">It's a common issue in Azure virtual networks that contain domain controllers that a domain controller is unavailable because it hasn't been started.</span></span> <span data-ttu-id="0af19-139">Jako řešení můžete místo účtu domény místního účtu správce.</span><span class="sxs-lookup"><span data-stu-id="0af19-139">As a workaround, you can use a local administrator account instead of a domain account.</span></span>

<a id="wincred"></a>

## <a name="windows-security-error-your-credentials-did-not-work"></a><span data-ttu-id="0af19-140">Chyba zabezpečení systému Windows: přihlašovací údaje nebyla úspěšná.</span><span class="sxs-lookup"><span data-stu-id="0af19-140">Windows Security error: Your credentials did not work.</span></span>
<span data-ttu-id="0af19-141">Příčina: Cíl virtuálního počítače nelze ověřit název účtu a heslo.</span><span class="sxs-lookup"><span data-stu-id="0af19-141">Cause: The target VM can't validate your account name and password.</span></span>

<span data-ttu-id="0af19-142">Počítače se systémem Windows můžete ověřit přihlašovací údaje místní účet nebo účet domény.</span><span class="sxs-lookup"><span data-stu-id="0af19-142">A Windows-based computer can validate the credentials of either a local account or a domain account.</span></span>

* <span data-ttu-id="0af19-143">Pro místní účty, použijte *ComputerName*\\*uživatelské jméno* syntaxe (Příklad: SQL1\Admin4798).</span><span class="sxs-lookup"><span data-stu-id="0af19-143">For local accounts, use the *ComputerName*\\*UserName* syntax (example: SQL1\Admin4798).</span></span>
* <span data-ttu-id="0af19-144">Pro účty domén, použijte *DomainName*\\*uživatelské jméno* syntaxe (Příklad: CONTOSO\peterodman).</span><span class="sxs-lookup"><span data-stu-id="0af19-144">For domain accounts, use the *DomainName*\\*UserName* syntax (example: CONTOSO\peterodman).</span></span>

<span data-ttu-id="0af19-145">Pokud jste zvýšili virtuálního počítače pro řadič domény v nové doménové struktury služby Active Directory, účet místního správce, který jste se přihlásili jsou převedeny na ekvivalentní účtu, který má stejné heslo v nové doménové struktuře a doméně.</span><span class="sxs-lookup"><span data-stu-id="0af19-145">If you have promoted your VM to a domain controller in a new Active Directory forest, the local administrator account that you signed in with is converted to an equivalent account with the same password in the new forest and domain.</span></span> <span data-ttu-id="0af19-146">Odstraní místní účet.</span><span class="sxs-lookup"><span data-stu-id="0af19-146">The local account is then deleted.</span></span>

<span data-ttu-id="0af19-147">Například pokud jste přihlášeni s použitím místní účet DC1\DCAdmin a poté vyzval virtuální počítač jako řadič domény v nové doménové struktury v doméně corp.contoso.com, místní účet DC1\DCAdmin se odstranila a je nový účet domény (CORP\DCAdmin) vytvořené pomocí stejné heslo.</span><span class="sxs-lookup"><span data-stu-id="0af19-147">For example, if you signed in with the local account DC1\DCAdmin, and then promoted the virtual machine as a domain controller in a new forest for the corp.contoso.com domain, the DC1\DCAdmin local account gets deleted and a new domain account (CORP\DCAdmin) is created with the same password.</span></span>

<span data-ttu-id="0af19-148">Ujistěte se, že název účtu je název, který virtuální počítač můžete ověřit jako platný účet a heslo je správné.</span><span class="sxs-lookup"><span data-stu-id="0af19-148">Make sure that the account name is a name that the virtual machine can verify as a valid account, and that the password is correct.</span></span>

<span data-ttu-id="0af19-149">Pokud potřebujete změnit heslo účtu místního správce, přečtěte si téma [jak resetovat heslo nebo služby Vzdálená plocha pro virtuální počítače s Windows](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="0af19-149">If you need to change the password of the local administrator account, see [How to reset a password or the Remote Desktop service for Windows virtual machines](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<a id="rdpconnect"></a>

## <a name="this-computer-cant-connect-to-the-remote-computer"></a><span data-ttu-id="0af19-150">Tento počítač se nemůže připojit ke vzdálenému počítači.</span><span class="sxs-lookup"><span data-stu-id="0af19-150">This computer can't connect to the remote computer.</span></span>
<span data-ttu-id="0af19-151">Příčina: Účet, který slouží k propojení nemá vzdálené plochy přihlašovací práva.</span><span class="sxs-lookup"><span data-stu-id="0af19-151">Cause: The account that's used to connect does not have Remote Desktop sign-in rights.</span></span>

<span data-ttu-id="0af19-152">Každý počítač se systémem Windows má vzdálené plochy uživatele místní skupina, která obsahuje účty a skupiny, které se můžete přihlásit do ní vzdáleně.</span><span class="sxs-lookup"><span data-stu-id="0af19-152">Every Windows computer has a Remote Desktop users local group, which contains the accounts and groups that can sign into it remotely.</span></span> <span data-ttu-id="0af19-153">Členové místní skupiny administrators také mají přístup, i když tyto účty nejsou uvedené v místní skupině uživatelů vzdálené plochy.</span><span class="sxs-lookup"><span data-stu-id="0af19-153">Members of the local administrators group also have access, even though those accounts are not listed in the Remote Desktop users local group.</span></span> <span data-ttu-id="0af19-154">Pro počítače připojené k doméně místní skupiny administrators obsahuje také správci domény pro doménu.</span><span class="sxs-lookup"><span data-stu-id="0af19-154">For domain-joined machines, the local administrators group also contains the domain administrators for the domain.</span></span>

<span data-ttu-id="0af19-155">Ujistěte se, zda má účet, který používáte pro připojení k vzdálené plochy přihlašovací práva.</span><span class="sxs-lookup"><span data-stu-id="0af19-155">Make sure that the account you're using to connect with has Remote Desktop sign-in rights.</span></span> <span data-ttu-id="0af19-156">Jako alternativní řešení připojit přes vzdálenou plochu pomocí domény nebo účet místního správce.</span><span class="sxs-lookup"><span data-stu-id="0af19-156">As a workaround, use a domain or local administrator account to connect over Remote Desktop.</span></span> <span data-ttu-id="0af19-157">Požadovaný účet přidat do místní skupiny uživatelů vzdálené plochy, použijte modul snap-in konzoly Microsoft Management Console (**systémové nástroje > Místní uživatelé a skupiny > skupiny > Remote Desktop Users**).</span><span class="sxs-lookup"><span data-stu-id="0af19-157">To add the desired account to the Remote Desktop users local group, use the Microsoft Management Console snap-in (**System Tools > Local Users and Groups > Groups > Remote Desktop Users**).</span></span>

## <a name="next-steps"></a><span data-ttu-id="0af19-158">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0af19-158">Next steps</span></span>
<span data-ttu-id="0af19-159">Pokud žádná z těchto chyb došlo k chybě a máte neznámé vydávat pomocí připojení pomocí protokolu RDP, najdete v článku [Průvodce řešením potíží pro vzdálené plochy](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="0af19-159">If none of these errors occurred and you have an unknown issue with connecting using RDP, see the [troubleshooting guide for Remote Desktop](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

* <span data-ttu-id="0af19-160">Řešení potíží s kroky v přístupu k aplikacím spuštěným na virtuálním počítači, najdete v části [řešení potíží s přístupem k aplikaci spuštěné na virtuálním počítači Azure](../linux/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="0af19-160">For troubleshooting steps in accessing applications running on a VM, see [Troubleshoot access to an application running on an Azure VM](../linux/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
* <span data-ttu-id="0af19-161">Pokud máte problémy s Secure Shell (SSH) pro připojení k virtuální počítač s Linuxem v Azure najdete v části [řešení SSH připojení k virtuální počítač s Linuxem v Azure](../linux/troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="0af19-161">If you are having issues using Secure Shell (SSH) to connect to a Linux VM in Azure, see [Troubleshoot SSH connections to a Linux VM in Azure](../linux/troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

