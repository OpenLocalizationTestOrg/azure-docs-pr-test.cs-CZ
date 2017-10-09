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
# <a name="troubleshooting-specific-rdp-error-messages-tooa-windows-vm-in-azure"></a>Řešení potíží s konkrétní RDP chybové zprávy tooa virtuální počítač s Windows v Azure
Když v Azure pomocí vzdálené plochy připojení tooa Windows virtuální počítač (VM), může se zobrazit konkrétní chybová zpráva. Tento článek podrobnosti některé z nejběžnějších chybových zpráv došlo, společně s řešení potíží s kroky tooresolve hello je. Pokud máte problémy s připojením tooyour virtuálních počítačů pomocí protokolu RDP ale proveďte není stane konkrétní chybová zpráva, přečtěte si hello [Průvodce řešením potíží pro vzdálené plochy](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Informace o specifické chybové zprávy najdete v části hello následující:

* [Hello relace byla odpojena, protože neexistují žádné licenční servery vzdálené plochy k dispozici tooprovide licenci](#rdplicense).
* [Vzdálená plocha nenašla hello počítač "název"](#rdpname).
* [Došlo k chybě ověřování. Hello místní úřad zabezpečení nelze kontaktovat](#rdpauth).
* [Chyba zabezpečení systému Windows: přihlašovací údaje nebyla úspěšná](#wincred).
* [Počítač se nemůže připojit vzdálený počítač toohello](#rdpconnect).

<a id="rdplicense"></a>

## <a name="hello-remote-session-was-disconnected-because-there-are-no-remote-desktop-license-servers-available-tooprovide-a-license"></a>Hello relace byla odpojena, protože nejsou k dispozici žádné licenční servery vzdálené plochy k dispozici tooprovide licenci.
Příčina: hello 120 licencování dní pro roli serveru vzdálené plochy hello vypršela a je třeba tooinstall licence.

Jako alternativní řešení uloží místní kopii souboru RDP hello z portálu hello a spustit tento příkaz příkazovém řádku tooconnect prostředí PowerShell. Tento krok zakazuje licencování pro právě toto připojení:

        mstsc <File name>.RDP /admin

Pokud nepotřebujete ve skutečnosti víc než dvou souběžných připojení ke vzdálené ploše připojení toohello virtuálních počítačů, můžete roli serveru vzdálené plochy hello tooremove správce serveru.

Další informace najdete v tématu hello blogu [virtuálního počítače Azure se nezdaří s "Vzdálené plochy licence k dispozici žádné servery"](https://blogs.msdn.microsoft.com/mast/2014/01/21/rdp-to-azure-vm-fails-with-no-remote-desktop-license-servers-available/).

<a id="rdpname"></a>

## <a name="remote-desktop-cant-find-hello-computer-name"></a>Vzdálená plocha nenašla počítač hello "name".
Příčina: hello klienta vzdálené plochy ve vašem počítači nelze přeložit název hello hello počítače v hello nastavení souboru protokolu RDP hello.

Možná řešení:

* Pokud jste na intranetu organizace, ujistěte se, že váš počítač má přístup toohello proxy server a může odesílat tooit provoz HTTPS.
* Pokud používáte místně uložených souborů protokolu RDP, zkuste použít hello jeden, který je generovaný hello portálu. Tento krok zajistí, že máte hello správný název DNS pro hello virtuálního počítače, nebo hello Cloudová služba a port koncového bodu hello hello virtuálních počítačů. Tady je ukázkový soubor RDP generované hello portálu:
  
        full address:s:tailspin-azdatatier.cloudapp.net:55919
        prompt for credentials:i:1

část adresy Hello tento soubor RDP má:

* Hello plně kvalifikovaný název domény hello cloudové služby, který obsahuje hello virtuálních počítačů ("tailspin-azdatatier.cloudapp.net" v tomto příkladu).
* Hello externí port TCP hello koncového bodu pro přenosy vzdálené plochy (55919).

<a id="rdpauth"></a>

## <a name="an-authentication-error-has-occurred-hello-local-security-authority-cannot-be-contacted"></a>Došlo k chybě ověřování. Nelze kontaktovat Hello místní úřad zabezpečení.
Příčina: hello cílovém virtuálním počítači nelze najít úřad zabezpečení hello v hello část názvu uživatelské přihlašovací údaje.

Pokud je vaše uživatelské jméno v podobě hello *SecurityAuthority*\\*uživatelské jméno* (Příklad: CORP\User1), hello *SecurityAuthority* část je buď systém hello virtuálních počítačů název počítače (pro hello místní úřad zabezpečení) nebo název domény služby Active Directory.

Možná řešení:

* Pokud je účet hello místní toohello virtuálních počítačů, zkontrolujte, zda že tento název virtuálního počítače hello je napsán správně.
* Pokud hello účet v doméně služby Active Directory, zkontrolujte pravopis hello hello názvu domény.
* Pokud je účet domény služby Active Directory a název domény hello je napsán správně, ověřte, zda je řadič domény k dispozici v této doméně. Je běžné problémy, se v Azure virtuální sítě, které obsahují řadiče domény, řadiče domény nedostupný, protože nebylo spuštěno. Jako řešení můžete místo účtu domény místního účtu správce.

<a id="wincred"></a>

## <a name="windows-security-error-your-credentials-did-not-work"></a>Chyba zabezpečení systému Windows: přihlašovací údaje nebyla úspěšná.
Příčina: cíl hello virtuálního počítače nelze ověřit název účtu a heslo.

Počítače se systémem Windows můžete ověřit přihlašovací údaje hello místní účet nebo účet domény.

* Pro místní účty, použijte hello *ComputerName*\\*uživatelské jméno* syntaxe (Příklad: SQL1\Admin4798).
* Pro účty domén, použijte hello *DomainName*\\*uživatelské jméno* syntaxe (Příklad: CONTOSO\peterodman).

Pokud jste zvýšili řadiče domény tooa virtuálních počítačů v nové doménové struktury služby Active Directory, je převést hello účet místního správce, který jste se přihlásili tooan ekvivalentní účet s hello stejné heslo v hello nové doménové struktury a domény. místní účet Hello se odstraní.

Například pokud jste přihlášeni s použitím místní účet hello DC1\DCAdmin a poté vyzval hello virtuální počítač jako řadič domény v nové doménové struktury pro hello doméně corp.contoso.com, hello DC1\DCAdmin místní účet se odstranila a nový účet domény (CORP\DCAdmin ) je vytvořen s hello stejné heslo.

Ujistěte se, že název účtu hello je název hello virtuálního počítače můžete ověřit jako účet platný, a je zadáno správné heslo tohoto hello.

Pokud potřebujete toochange hello heslo účtu místního správce hello, přečtěte si [jak tooreset heslo nebo hello vzdálené plochy služby pro virtuální počítače s Windows](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

<a id="rdpconnect"></a>

## <a name="this-computer-cant-connect-toohello-remote-computer"></a>Počítač se nemůže připojit toohello vzdáleného počítače.
Příčina: hello účet, který byl použit tooconnect nemá vzdálené plochy přihlašovací práva.

Každý počítač se systémem Windows má skupinu místního uživatele vzdálené plochy, kterou obsahuje hello účty a skupiny, které můžete do ní vzdáleně. Členové skupiny místní správci hello také mají přístup, i když tyto účty nejsou uvedené v místní skupině Uživatelé hello vzdálené plochy. Pro počítače připojené k doméně hello místní skupiny administrators také obsahuje hello správci domény pro doménu hello.

Ujistěte se, zda má účet hello, kterou používáte tooconnect pomocí vzdálené plochy přihlašovací práva. Jako řešení použijte vzdálenou plochu domény nebo tooconnect účet místního správce. tooadd hello požadovaný účet toohello vzdálené plochy uživatele místní skupiny, použijte modul snap-in konzoly Microsoft Management Console hello (**systémové nástroje > Místní uživatelé a skupiny > skupiny > Remote Desktop Users**).

## <a name="next-steps"></a>Další kroky
Pokud žádná z těchto chyb došlo k chybě a máte neznámé potíže s připojením pomocí protokolu RDP, najdete v části hello [Průvodce řešením potíží pro vzdálené plochy](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

* Řešení potíží s kroky v přístupu k aplikacím spuštěným na virtuálním počítači, najdete v části [Poradce při potížích přístup tooan aplikace běžící na virtuálním počítači Azure](../linux/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Pokud máte problémy s tooa tooconnect Secure Shell (SSH) virtuálního počítače s Linuxem v Azure, najdete v části [řešení SSH připojení tooa virtuálního počítače s Linuxem v Azure](../linux/troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

