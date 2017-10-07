---
title: "škálování aaaTroubleshoot s sady škálování virtuálního počítače | Microsoft Docs"
description: "Řešení potíží s škálování s sady škálování virtuálního počítače. Pochopení typické problémy vzniklé a jak tooresolve je."
services: virtual-machine-scale-sets
documentationcenter: 
author: gbowerman
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: c7d87b72-ee24-4e52-9377-a42f337f76fa
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: windows
ms.devlang: na
ms.topic: article
ms.date: 10/28/2016
ms.author: guybo
ms.openlocfilehash: 4c9a70992348d87fb43646421a90a027bf400a17
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-autoscale-with-virtual-machine-scale-sets"></a>Řešení potíží s škálování s sady škálování virtuálního počítače
**Problém** – jste vytvořili automatické škálování infrastruktury v správce Azure Resource Manager pomocí sady škálování virtuálního počítače – například nasazením šablonu takto: https://github.com/Azure/azure-quickstart-templates/tree/master/201- vmss bottle škálování – máte definované pravidel škálování a vyhovující postup, s tím rozdílem, že bez ohledu na to, kolik zatížení umístěných na hello virtuálních počítačů, se nebude škálování.

## <a name="troubleshooting-steps"></a>Řešení potíží
Některé věci tooconsider patří:

* Kolik jader každý virtuální počítač nemá a jsou načítání každý jádra? Šablona Azure Quickstart Hello příkladu výše má do_work.php skript, který načte jediného jádra. Pokud používáte virtuálního počítače větší než jedním jádrem velikost virtuálního počítače jako Standard_A1 nebo D1 pak budete potřebovat toorun tato zatížení vícekrát. Zkontrolujte, kolik virtuálních počítačů cores kontrolou [velikosti pro systém Windows virtuálních počítačů v Azure](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* Tom, kolik virtuálních počítačů v hello sady škálování virtuálního počítače, můžete pracujeme na každé z nich?
  
    Horizontální navýšení kapacity událostí bude, až hello průměrnou hodnotu CPU napříč **všechny** hello virtuální počítače ve škálovací sadě překračuje hello prahovou hodnotu, časem hello interní definované v pravidlech automatického škálování hello.
* Vynechali jste žádné události škálování?
  
    Zkontrolujte, zda protokoly auditu hello v hello portál Azure pro škálování události. Možná se škálování nahoru a nebyla provedena vertikálně. Můžete filtrovat podle "Škálování"...
  
    ![Protokoly auditu][audit]
* Jsou vaše škálování in a škálování prahové hodnoty dostatečně neliší?
  
    Předpokládejme, že nastavíte pravidlo tooscale se při průměrné využití procesoru je větší než 50 % za 5 minut a tooscale v Pokud průměrné využití procesoru je menší než 50 %. To by způsobilo "flapping" problém při využití procesoru je prahová hodnota zavřít toothis, s akcí škálování, které neustále rostoucí a klesající hello velikost sady hello. Z toho důvodu hello škálování služby pokusí tooprevent "netřepotá", který můžete manifest jako není škálování. Proto zkontrolujte, zda se Škálováním na více systémů a škálování v prahové hodnoty jsou dostatečně neliší tooallow některé prostoru mezi škálování.
* Můžete psát vlastní šablonu JSON?
  
    Je snadno toomake chyb, takže začínat šablony jako hello nad které prokazatelně toowork a proveďte malé přírůstkové změny. 
* Můžete je ručně škálovat, příchozí nebo odchozí?
  
    Zkuste opětovného nasazení hello prostředků s číslem různých "kapacitou" nastavení toochange hello virtuálních počítačů s Škálovací sady virtuálního počítače ručně. Příklad šablony toodo Toto je tady: https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing – může být nutné tooedit hello šablony toomake se obsahuje hello stejný počítač velikost jako Škálovací sady používá. Pokud hello počet virtuálních počítačů můžete úspěšně ručně změnit, pak víte, že problém hello je izolovaná tooautoscale.
* Zkontrolujte vaše Microsoft.Compute/virtualMachineScaleSet a Microsoft.Insights prostředky v hello [Průzkumníka prostředků Azure](https://resources.azure.com/)
  
    Jedná se nepostradatelný řešení potíží s nástroji, které se dozvíte, hello stav svých prostředků Azure Resource Manager. Klikněte na vaše předplatné a podívejte se na hello při odstraňování skupiny prostředků. V rámci hello výpočetních prostředků zprostředkovatele podívejte se na hello jste vytvořili sady škálování virtuálního počítače a zkontrolujte hello zobrazení Instance, které se dozvíte, hello stavu nasazení. Také zkontrolujte hello zobrazení instance virtuálních počítačů v hello Škálovací sady virtuálního počítače. Pak přejděte do zprostředkovatele prostředků Microsoft.Insights hello a zkontrolujte, zda pravidla pro automatické škálování hello vypadat dobře.
* Je hello rozšíření diagnostiky práce a generování údaje o výkonu?
  
    **Aktualizace:** Azure škálování bylo rozšířené toouse hostitele na základě kanály metriky, které už vyžaduje toobe rozšíření diagnostiky nainstalována. To znamená, že hello další několik odstavců přestanou platit, pokud vytvoříte automatické škálování aplikace pomocí hello nový kanál. Příklad šablony Azure, které byly převedený toouse hello hostitele kanálu je: https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale. 
  
    Použití hostitele na základě metriky pro škálování je lepší pro hello následujících důvodů:
  
  * Méně přesunutí části jako žádná rozšíření diagnostiky potřebovat toobe nainstalována.
  * Jednodušší šablony. Stačí přidáte Statistika škálování pravidla tooan existující škálovací sady šablony.
  * Vytváření sestav spolehlivější a rychlejší spouštění nové virtuální počítače.
    
    Hello pouze důvodů, které že můžete pomocí diagnostiky rozšíření tookeep by pokud budete potřebovat paměti diagnostiky reporting/škálování. Metriky hostitele na základě neoznamuje paměti.
    
    Si uvědomit pouze podle hello zbývající části tohoto článku Pokud stále používáte rozšíření diagnostiky pro vaše automatické škálování.
    
    Škálování ve službě Správce prostředků Azure můžete pracovat (ale už má k) prostřednictvím rozšíření virtuálního počítače, názvem hello rozšíření diagnostiky. Ho vysílá výkonu dat tooa úložiště účet, který definujete v šabloně hello. Tato data se pak agregovat službou Azure monitorování hello.
    
    Pokud hello službě Statistika nelze číst data z hello virtuálních počítačů, by mělo toosend vám e-mail – například pokud hello virtuální počítače byly dolů, proto zkontrolujte e-mailu (hello jeden jste zadali při vytváření hello účet Azure).
    
    Můžete také přejít a prohlížet hello data. Podívejte se na hello účtu úložiště Azure pomocí Průzkumníku cloudu. Například při použití hello [cloudu Průzkumníka Visual Studio](https://visualstudiogallery.msdn.microsoft.com/aaef6e67-4d99-40bc-aacf-662237db85a2), přihlaste se a vyberte hello předplatné Azure, který používáte a hello účet úložiště diagnostiky název odkazuje v hello Definice rozšíření diagnostiky ve vašem nasazení Šablona...
    
    ![Průzkumník cloudu][explorer]
    
    Zde se zobrazí ve svazku tabulek, které ukládají hello dat z jednotlivých virtuálních počítačů. Převádět Linux a hello procesoru metrika jako příklad, podívejte se na poslední řádky hello. Průzkumník cloudu Hello Visual Studio podporuje dotazovací jazyk, takže je možné spustit dotaz jako "časové razítko gt data a času: 2016-02-02T21:20:00Z'" toomake zda získat hello nejaktuálnějších událostí (považovat za čas v UTC). Hello dat, které vidíte, že existuje odpovídají toohello škálování pravidla, která nastavíte? V příkladu hello níže hello procesoru pro počítač 20 spuštění, zvýšit too100 % přes hello posledních 5 minut...
    
    ![Úložiště tabulek][tables]
    
    Pokud není hello dat, pak předpokládají, že je hello problém s rozšířením diagnostiky hello spuštěné v hello virtuálních počítačů. Pokud hello dat existuje, znamená to, jestli existuje problém s vaší pravidel škálování, nebo se službou Statistika hello. Zkontrolujte [Azure stav](https://azure.microsoft.com/status/).
    
    Jakmile jste byl pomocí těchto kroků, pokud máte pořád problémy škálování nešlo zkuste fóra hello [MSDN](https://social.msdn.microsoft.com/forums/azure/home?category=windowsazureplatform%2Cazuremarketplace%2Cwindowsazureplatformctp), nebo [přetečení zásobníku](http://stackoverflow.com/questions/tagged/azure), nebo protokolu volání podpory. Mít připravený tooshare hello šablony a zobrazení dat výkonu hello.

[audit]: ./media/virtual-machine-scale-sets-troubleshoot/image3.png
[explorer]: ./media/virtual-machine-scale-sets-troubleshoot/image1.png
[tables]: ./media/virtual-machine-scale-sets-troubleshoot/image4.png
