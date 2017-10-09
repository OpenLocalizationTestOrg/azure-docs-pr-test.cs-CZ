---
title: "aaaFAQ o virtuálních počítačích Windows v Azure | Microsoft Docs"
description: "Poskytuje odpovědi toosome z hello časté otázky týkající se virtuální počítače s Windows, které jsou vytvořené pomocí modelu Resource Manager hello."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-management
ms.assetid: 757da816-a050-4889-a010-6f75d7978eb7
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: cynthn
ms.openlocfilehash: ee366a04bda347ce2be07bde4fc6bad306cc1da9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="frequently-asked-question-about-windows-virtual-machines"></a>Časté otázky o virtuálních počítačích s Windows
Tento článek se zaměřuje na některé běžné dotazy týkající se virtuální počítače s Windows, které jsou vytvořené v Azure pomocí modelu nasazení Resource Manager hello. Hello Linux verzi tohoto tématu, naleznete v části [často kladené otázky o virtuálních počítačích s Linuxem](../linux/faq.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="what-can-i-run-on-an-azure-vm"></a>Co můžu spouštět na virtuálním počítači Azure?
Všichni předplatitelé můžou na virtuálním počítači Azure spouštět serverový software. Informace o hello zásady podpory pro spuštěné serverového softwaru společnosti Microsoft v Azure najdete v tématu [podporu softwaru server společnosti Microsoft pro virtuální počítače Azure](https://support.microsoft.com/kb/2721672)

Některé verze systému Windows 7, Windows 8.1 a Windows 10 jsou k dispozici tooMSDN Azure benefit odběratele a odběratelé MSDN vývoj a testování průběžné platby pro vývoj a testování úlohy. Podrobnosti, včetně pokynů a omezení, najdete v tématu [Image klienta Windows pro předplatitele MSDN](http://azure.microsoft.com/blog/2014/05/29/windows-client-images-on-azure/). 

## <a name="how-much-storage-can-i-use-with-a-virtual-machine"></a>Kolik úložiště můžu využít s virtuálním počítačem?
Každý datový disk může být až too1 TB. Hello počet datových disků, které můžete použít závisí na velikosti hello hello virtuálního počítače. Podrobnosti najdete v článku [Velikosti služeb Virtual Machines](sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Azure spravované disky jsou hello nové a doporučené disk úložiště nabídky pro použití s virtuálními počítači Azure pro trvalé úložiště dat. Pro každý virtuální počítač můžete použít několik služeb Managed Disks. Služba Managed Disks nabízí dva typy odolných úložišť: Premium a Standard. Informace o cenách najdete v části [spravované ceny disků](https://azure.microsoft.com/pricing/details/managed-disks).

Účty úložiště Azure můžete také poskytují úložiště pro hello disk operačního systému a všechny datové disky. Každý disk je soubor .vhd uložený jako objekt blob stránky. Podrobnosti o cenách najdete v tématu [Podrobnosti o cenách úložiště](https://azure.microsoft.com/pricing/details/storage/).

## <a name="how-can-i-access-my-virtual-machine"></a>Jak lze získat přístup k virtuální počítač?
Vytvořte vzdálené připojení pomocí připojení vzdálené plochy (RDP) pro virtuální počítač s Windows. Pokyny najdete v tématu [jak se tooconnect a protokol na tooan Azure virtuální počítač, systém Windows spuštěný](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Maximální počet souběžných připojení dvě jsou podporovány, pokud je hello server nakonfigurovaný jako hostitel relace vzdálené plochy.  

Pokud máte problémy s vzdálené plochy, přečtěte si téma [tooa připojení řešení Vzdálená plocha systému Windows virtuálního počítače Azure](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

Pokud jste obeznámeni s technologií Hyper-V, může být díváte pro podobné tooVMConnect nástroje. Azure nepodporuje poskytují podobné nástroj, protože konzole přístup tooa virtuálního počítače není podporován.

## <a name="can-i-use-hello-temporary-disk-hello-d-drive-by-default-toostore-data"></a>Můžete použít hello dočasným diskovým (hello D: jednotky ve výchozím nastavení) toostore dat?
Nepoužívejte hello dočasným diskovým toostore data. Je pouze dočasné úložiště, takže by riziko ztráty dat, které nelze obnovit. Může dojít ke ztrátě dat. Pokud hello virtuální počítač přesune tooa jiného hostitele. Změna velikosti virtuálního počítače, aktualizace hello hostitele nebo selhání hardwaru na hostiteli hello jsou některé z důvodů hello, které může přesunout virtuální počítač.

Pokud máte aplikaci, která vyžaduje písmeno jednotky D: hello toouse, můžete opětovně přiřadit písmena jednotek, aby hello dočasné disk používá něco jiného než D:. Pokyny najdete v tématu [změnit písmeno jednotky hello dočasné disku Windows hello](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).


## <a name="how-can-i-change-hello-drive-letter-of-hello-temporary-disk"></a>Jak můžete změnit písmeno jednotky hello hello dočasné disku?
Můžete změnit písmeno jednotky hello přesunutí souborů stránku hello a nové přiřazení písmena jednotek, ale potřebujete toomake se, že že Hello kroky v určitém pořadí. Pokyny najdete v tématu [změnit písmeno jednotky hello dočasné disku Windows hello](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

## <a name="can-i-add-an-existing-vm-tooan-availability-set"></a>Můžete přidat stávající sadu dostupnosti tooan virtuálního počítače?
Ne. Pokud chcete, aby váš virtuální počítač toobe součástí skupiny dostupnosti, je třeba toocreate hello virtuálních počítačů v rámci sady hello. Momentálně není k dispozici způsob tooadd s dostupností tooan virtuálního počítače nastavit po jeho vytvoření.

## <a name="can-i-upload-a-virtual-machine-tooazure"></a>Můžete nahrát tooAzure virtuálního počítače?
Ano. Pokyny najdete v tématu [migrace místní virtuální počítače tooAzure](on-prem-to-azure.md).

## <a name="can-i-resize-hello-os-disk"></a>Můžete změnit velikost hello disk operačního systému?
Ano. Pokyny najdete v tématu [jak tooexpand hello jednotky operačního systému virtuálního počítače ve skupině prostředků Azure](expand-os-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="can-i-copy-or-clone-an-existing-azure-vm"></a>Můžete kopírovat nebo klonovat existující virtuální počítač Azure?
Ano. Pomocí spravovaného bitové kopie, můžete vytvořit bitovou kopii virtuálního počítače a potom pomocí bitové kopie toobuild hello více nové virtuální počítače. Pokyny najdete v tématu [vytvořit vlastní image virtuálního počítače](tutorial-custom-images.md).

## <a name="why-am-i-not-seeing-canada-central-and-canada-east-regions-through-azure-resource-manager"></a>Proč nejsou zobrazeny Kanada centrální a Východní Kanada oblastí prostřednictvím Správce Azure Resource Manager?

dvě nové oblasti Hello Kanada centrální a Východní Kanada nejsou automaticky registrované pro vytvoření virtuálního počítače pro existující předplatných Azure. Tato registrace se provádí automaticky při nasazení virtuálního počítače prostřednictvím portálu Azure tooany hello jiné oblasti pomocí Azure Resource Manager. Virtuální počítač po nasazené tooany jiné oblasti Azure hello nové oblasti by měly být dostupné pro následující virtuální počítače.

## <a name="does-azure-support-linux-vms"></a>Podporuje Azure virtuální počítače s Linuxem?
Ano. tooquickly vytvoření virtuálního počítače s Linuxem tootry limitu, viz [vytvoření virtuálního počítače s Linuxem v Azure pomocí hello portál](../linux/quick-create-portal.md).

## <a name="can-i-add-a-nic-toomy-vm-after-its-created"></a>Můžete přidat po vytvoření toomy síťový adaptér virtuálního počítače?
Ano, to je nyní možné. deallocated byla zastavena toobe první potřebám Hello virtuálních počítačů. Potom můžete přidat nebo odebrat síťový adaptér (Pokud je hello poslední síťovou kartu v hello virtuálních počítačů). 

## <a name="are-there-any-computer-name-requirements"></a>Existují jakékoli požadavky na název počítače?
Ano. název počítače Hello může být maximálně 15 znaků. V tématu [pojmenování konvence pravidla a omezení](/architecture/best-practices/naming-conventions#naming-rules-and-restrictions?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) Další informace ohledně pojmenování vašich prostředků.

## <a name="are-there-any-resource-group-name-requirements"></a>Dochází k jakémukoli prostředku, požadavky na název skupiny?
Ano. Název skupiny prostředků Hello může být maximálně 90 znaků. V tématu [pojmenování konvence pravidla a omezení](/architecture/best-practices/naming-conventions#naming-rules-and-restrictions?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) Další informace o skupinách prostředků.

## <a name="what-are-hello-username-requirements-when-creating-a-vm"></a>Jaké jsou požadavky hello uživatelského jména, při vytváření virtuálního počítače?

Uživatelská jména, může být maximálně 20 znaků a nesmí končit tečkou ("."). 


Hello následující uživatelská jména nejsou povoleny:
<table>
    <tr>
        <td style="text-align:center">Správce </td><td style="text-align:center"> Správce </td><td style="text-align:center"> Uživatel </td><td style="text-align:center"> Uživatel1</td>
    </tr>
    <tr>
        <td style="text-align:center">Test </td><td style="text-align:center"> uživatel2 </td><td style="text-align:center"> test1 </td><td style="text-align:center"> UŽIVATEL3</td>
    </tr>    <tr>
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

## <a name="what-are-hello-password-requirements-when-creating-a-vm"></a>Jaké jsou požadavky na heslo hello při vytváření virtuálního počítače?
Hesla musí být 12 123 znaků a musí splňovat 3 z následujících 4 požadavky na složitost hello:

* Mít nižší znaků
* Horní znaky
* Mít číslice.
* Mít speciální znak (regulární výraz odpovídat [\W_])

Hello následující hesla nejsou povoleny:

<table>
    <tr>
        <td>abc@123 </td>
        <td>P@$$w0rd </td>
        <td>P@ssw0rd </td>
        <td>P@ssword123 </td>
        <td>Pa$ $ aplikace word </td>
    </tr>
    <tr>
        <td>pass@word1 </td>
        <td>Heslo! </td>
        <td>Heslo1 </td>
        <td>Password22 </td>
        <td>ILOVEYOU! </td>
    </tr>
</table>
