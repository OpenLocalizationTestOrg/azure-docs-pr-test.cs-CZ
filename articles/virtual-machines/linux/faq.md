---
title: "Nejčastější dotazy pro virtuální počítače s Linuxem v Azure | Microsoft Docs"
description: "Poskytuje odpovědi na některé časté otázky týkající se virtuální počítače Linux vytvořené pomocí modelu Resource Manager."
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-management
ms.assetid: 3648e09c-1115-4818-93c6-688d7a54a353
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: cynthn
ms.openlocfilehash: 0e06d21bd0b6ef807f38e41dcd50c9cd715607a3
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="frequently-asked-question-about-linux-virtual-machines"></a>Časté otázky o virtuálních počítačích s Linuxem
Tento článek se zaměřuje na některé běžné dotazy týkající se virtuální počítače Linux vytvořené v Azure pomocí modelu nasazení Resource Manager. Windows verzi tohoto tématu naleznete v části [často kladené otázky o virtuálních počítačích s Windows](../windows/faq.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="what-can-i-run-on-an-azure-vm"></a>Co můžu spouštět na virtuálním počítači Azure?
Všichni předplatitelé můžou na virtuálním počítači Azure spouštět serverový software. Další informace najdete v tématu [Linux na Azure-Endorsed distribuce](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="how-much-storage-can-i-use-with-a-virtual-machine"></a>Kolik úložiště můžu využít s virtuálním počítačem?
Každý datový disk může mít velikost až 1 TB. Počet datových disků, které můžete využít, závisí na velikosti virtuálního počítače. Podrobnosti najdete v článku [Velikosti služeb Virtual Machines](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Účet úložiště Azure poskytuje úložiště pro disk operačního systému a všechny datové disky. Každý disk je soubor .vhd uložený jako objekt blob stránky. Podrobnosti o cenách najdete v tématu [Podrobnosti o cenách úložiště](https://azure.microsoft.com/pricing/details/storage/).

## <a name="how-can-i-access-my-virtual-machine"></a>Jak lze získat přístup k virtuální počítač?
Vytvořit vzdálené připojení k přihlášení k virtuálnímu počítači pomocí protokolu Secure Shell (SSH). Přečtěte si pokyny o tom, jak připojit [ze systému Windows](ssh-from-windows.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) nebo [z Linuxu a Macu](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). SSH ve výchozím nastavení umožňuje maximálně 10 souběžných připojení. Toto číslo můžete navýšit upravením konfiguračního souboru.

Pokud máte problémy, podívejte se na [řešení potíží s Secure Shell (SSH) připojení](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="can-i-use-the-temporary-disk-devsdb1-to-store-data"></a>Můžete použít dočasným diskovým (/ dev/sdb1) k uložení dat?
K ukládání dat nepoužívejte dočasným diskovým (/ dev/sdb1). Pro dočasné úložiště je pouze existuje. Riskujete ztráty dat, které nelze obnovit.

## <a name="can-i-copy-or-clone-an-existing-azure-vm"></a>Můžete kopírovat nebo klonovat existující virtuální počítač Azure?
Ano. Pokyny najdete v tématu [vytvoření kopie virtuální počítač s Linuxem v modelu nasazení Resource Manager](copy-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="why-am-i-not-seeing-canada-central-and-canada-east-regions-through-azure-resource-manager"></a>Proč nejsou zobrazeny Kanada centrální a Východní Kanada oblastí prostřednictvím Správce Azure Resource Manager?
Dvě nové oblasti Kanada centrální a Východní Kanada nejsou automaticky registrované pro vytvoření virtuálního počítače pro existující předplatných Azure. Tato registrace se provádí automaticky při nasazení virtuálního počítače prostřednictvím portálu Azure do žádné jiné oblasti pomocí Azure Resource Manager. Po nasazení virtuálního počítače v jiné oblasti Azure nové oblasti musí být k dispozici pro následující virtuální počítače.

## <a name="can-i-add-a-nic-to-my-vm-after-its-created"></a>Mohu přidat síťový adaptér k virtuálnímu počítači po jeho vytvoření?
Ano, to je nyní možné. Virtuální počítač nejdřív je potřeba zastavit deallocated. Potom můžete přidat nebo odebrat síťový adaptér (Pokud je poslední síťový adaptér ve virtuálním počítači). 

## <a name="are-there-any-computer-name-requirements"></a>Existují jakékoli požadavky na název počítače?
Ano. Název počítače nesmí být delší než 64 znaků. V tématu [pojmenování konvence pravidla a omezení](/architecture/best-practices/naming-conventions#naming-rules-and-restrictions?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Další informace ohledně pojmenování vašich prostředků.

## <a name="are-there-any-resource-group-name-requirements"></a>Dochází k jakémukoli prostředku, požadavky na název skupiny?
Ano. Název skupiny prostředků může být maximálně 90 znaků. V tématu [pojmenování konvence pravidla a omezení](/architecture/best-practices/naming-conventions#naming-rules-and-restrictions?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Další informace o skupinách prostředků.

## <a name="what-are-the-username-requirements-when-creating-a-vm"></a>Jaké jsou požadavky na uživatelské jméno při vytváření virtuálního počítače?
Uživatelská jména musí být 1-64 znaků.

Následující uživatelská jména nejsou povoleny:

<table>
    <tr>
        <td style="text-align:center">Správce </td><td style="text-align:center"> Správce </td><td style="text-align:center"> Uživatel </td><td style="text-align:center"> Uživatel1</td>
    </tr>
    <tr>
        <td style="text-align:center">Test </td><td style="text-align:center"> uživatel2 </td><td style="text-align:center"> test1 </td><td style="text-align:center"> UŽIVATEL3</td>
    </tr>
    <tr>
        <td style="text-align:center">admin1 </td><td style="text-align:center"> 1 </td><td style="text-align:center"> 123 </td><td style="text-align:center"> A</td>
    </tr>
    <tr>
        <td style="text-align:center">actuser  </td><td style="text-align:center"> ADM </td><td style="text-align:center"> admin2 </td><td style="text-align:center"> ASPNET</td>
    </tr>
    <tr>
        <td style="text-align:center">zálohování </td><td style="text-align:center"> Konzola </td><td style="text-align:center"> David </td><td style="text-align:center"> hosta</td>
    </tr>
    <tr>
        <td style="text-align:center">Jan </td><td style="text-align:center"> Vlastník </td><td style="text-align:center"> kořenové </td><td style="text-align:center"> server</td>
    </tr>
    <tr>
        <td style="text-align:center">SQL </td><td style="text-align:center"> Podpora </td><td style="text-align:center"> support_388945a0 </td><td style="text-align:center"> Sys</td>
    </tr>
    <tr>
        <td style="text-align:center">test2 </td><td style="text-align:center"> Test3 </td><td style="text-align:center"> Uživatel4 </td><td style="text-align:center"> user5</td>
    </tr>
</table>


## <a name="what-are-the-password-requirements-when-creating-a-vm"></a>Jaké jsou požadavky na heslo, když vytvoření virtuálního počítače?
Hesla musí mít délku 6 72 znaků a musí splňovat 3 z následujících 4 složitost:

* Mít nižší znaků
* Horní znaky
* Mít číslice.
* Mít speciální znak (regulární výraz odpovídat [\W_])

Nejsou povoleny následující hesla:

<table>
    <tr>
        <td style="text-align:center">abc@123</td>
        <td style="text-align:center">P@$$w0rd</td>
        <td style="text-align:center">P@ssw0rd</td>
        <td style="text-align:center">P@ssword123</td>
        <td style="text-align:center">Pa$ $ aplikace word</td>
    </tr>
    <tr>
        <td style="text-align:center">pass@word1</td>
        <td style="text-align:center">Heslo!</td>
        <td style="text-align:center">Heslo1</td>
        <td style="text-align:center">Password22</td>
        <td style="text-align:center">ILOVEYOU!</td>
    </tr>
</table>
