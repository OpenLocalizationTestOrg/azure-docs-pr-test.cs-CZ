---
title: "aaaVirtual velikosti počítačů pro služby Azure Cloud | Microsoft Docs"
description: "Uvádí velikosti hello jiný virtuální počítač (a ID) pro Azure cloud service webových a pracovních rolí."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 1127c23e-106a-47c1-a2e9-40e6dda640f6
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 07/18/2017
ms.author: adegeo
ms.openlocfilehash: 93d91a67afc352f3d18c31e0dd5cf976bf46350c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sizes-for-cloud-services"></a>Velikosti pro cloudové služby
Toto téma popisuje dostupné velikosti hello a možnosti pro cloudové služby role instance (webových rolí a rolí pracovního procesu). Poskytuje také toobe aspekty nasazení myslet při plánování toouse tyto prostředky. ID, které vložíte v má velikost pro všechny vaše [souboru definice služby](cloud-services-model-and-package.md#csdef). Ceny pro každou velikost jsou k dispozici na hello [ceník služby Cloud Services](https://azure.microsoft.com/pricing/details/cloud-services/) stránky.

> [!NOTE]
> toosee související omezení Azure najdete v tématu [předplatné Azure a omezení služby, kvóty a omezení](../azure-subscription-service-limits.md)
>
>

## <a name="sizes-for-web-and-worker-role-instances"></a>Velikosti pro webové a pracovní role instance
Existuje více standardních formátů toochoose z v Azure. Tady je několik aspektů, které je třeba při volbě velikosti zvážit:

* Virtuální počítače, D-series jsou navrženou toorun aplikace, které potřebují vyšší výpočetní výkon a výkon dočasné disku. Virtuální počítače D-series zadejte rychlejších procesorů vyšší poměr paměti jádra a na jednotku SSD (SSD) pro dočasným diskovým hello. Podrobnosti najdete v tématu hello oznámení na hello Azure blog [nové velikosti virtuálního počítače D-Series](https://azure.microsoft.com/blog/2014/09/22/new-d-series-virtual-machine-sizes/).
* Dv2-series, pokračovací toohello původní D-series, funkce výkonnější procesor. Hello Dv2-series procesoru je asi 35 % rychlejší než hello D-series procesoru. Je založena na hello nejnovější generace 2.4 v3® GHz Intel Xeon E5-2673 procesoru (Haswell) a s hello Intel Turbo nárůst technologie 2.0, můžete přejít do too3.1 GHz. má Hello Dv2-series hello stejné konfigurace paměti a disku jako hello D-series.
* Virtuální počítače G-series nabízejí hello nejvíce paměti a spustit na hostitelích, které mají procesory Intel Xeon E5 V3 rodiny.
* Hello A-series virtuálních počítačů můžete nasadit na různé typy hardwaru a procesory. velikost Hello je omezen na hello hardware, výkon toooffer konzistentní procesoru pro hello spuštěna instance, bez ohledu na to hello hardwaru, který je nasazen na základě. toodetermine hello fyzický hardware na kterém je nasazený této velikosti, dotaz hello virtuální hardware z v rámci hello virtuálního počítače.
* Hello A0 velikost je povolená odebíraných na fyzickém hardwaru hello. Pro jenom tato konkrétní velikost jiné zákaznických nasazení může mít vliv na výkon hello spuštěné úlohy. relativní výkon Hello popsané níže jako základní hello očekávání, předmět tooan přibližnou variabilita 15 procent.

velikost Hello hello virtuálního počítače, ovlivňuje hello ceny. velikost Hello ovlivní také hello zpracování, paměti a úložnou kapacitu hello virtuálního počítače. Náklady na úložiště se počítá samostatně na základě použitých stránek v účtu úložiště hello. Podrobnosti najdete v tématu [podrobnosti o cenách na Cloud Services](https://azure.microsoft.com/pricing/details/cloud-services/) a [Azure Storage – ceny](https://azure.microsoft.com/pricing/details/storage/).

Hello následující aspekty mohou pomoci při rozhodování, na velikost:

* Hello velikosti A8-A11 a H-series se také označují jako *náročné instance*. Hello hardwaru, který spouští tyto velikosti je navržena a optimalizována pro náročné a aplikace, modelování a simulace clusteru aplikace náročné na sítě, včetně vysoce výkonné výpočetní (HPC). Hello A8-A11 řady používá Intel Xeon E5-. 2670 @ 2.6 GHZ a hello H-series. Intel Xeon E5-2667 v3 @ 3,2 GHz. Podrobné informace o použití těchto velikostí, najdete v části [vysokovýkonné výpočetní velikosti virtuálních počítačů](../virtual-machines/windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Dv2-series, D-series G-series, jsou ideální pro aplikace, které potřebují rychlejších procesorů, místní lepší výkon na disku, nebo mít vyšší nároky na paměť. Nabízejí výkonnou kombinaci pro mnoho podnikových aplikací.
* Některé hello fyzických hostitelích v datových center Azure nemusí podporovat větší velikosti virtuálních počítačů, jako je například A5 – A11. V důsledku toho mohou se zobrazit chybová zpráva hello **se nezdařilo tooconfigure virtuálního počítače {název počítače}** nebo **se nezdařilo toocreate virtuálního počítače {název počítače}** při změně velikosti existující virtuální počítač tooa nové velikost; Vytvoření nového virtuálního počítače ve virtuální síti vytvořené před 16. dubna 2013; nebo přidání nového virtuálního počítače tooan existující cloudové služby. V tématu [Chyba: "Se nezdařilo tooconfigure virtuální počítač"](https://social.msdn.microsoft.com/Forums/9693f56c-fcd3-4d42-850e-5e3b56c7d6be/error-failed-to-configure-virtual-machine-with-a5-a6-or-a7-vm-size?forum=WAVirtualMachinesforWindows) na hello fórum podpory pro řešení pro jednotlivé scénáře nasazení.
* Vaše předplatné může také omezit hello počet jader, které můžete nasadit v určitých velikost řady. tooincrease kvótu, kontaktujte podporu Azure.

## <a name="performance-considerations"></a>Otázky výkonu
Vytvořili jsme hello konceptu hello Azure výpočetní jednotky (ACU) tooprovide způsob porovnání výkonu výpočetní (CPU) napříč Azure SKU a tooidentify, což SKU je pravděpodobně toosatisfy výkon potřebuje.  Jednotka ACU je aktuálně stanovená tak, že malý virtuální počítač (Standard_A1) má 100 ACU a ostatní jednotky SKU jsou pak ohodnoceny podle relativního výsledku standardního srovnávacího testu.

> [!IMPORTANT]
> Hello ACU je jenom obecných zásad. Hello výsledky pro úlohy se může lišit.
>
>

<br>

| Rodina SKU | ACU na jádro |
| --- | --- |
| [ExtraSmall](#a-series) |50 |
| [Malá ExtraLarge](#a-series) |100 |
| [A5-7](#a-series) |100 |
| [Standard_A1-8v2](#av2-series) |100 |
| [Standard_A2m-8mv2](#av2-series) |100 |
| [A8-A11](#a-series) |225* |
| [D1-14](#d-series) |160 |
| [D1-15v2](#dv2-series) |210 - 250* |
| [G1-5](#g-series) |180 - 240* |
| [H](#h-series) |290 - 300* |

ACUs označené * použijte frekvence tooincrease procesoru technologie Intel® Turbo a poskytují zvýšení výkonu. Hello množství hello nárůst může lišit v závislosti na velikost virtuálního počítače hello, úlohy a další úlohy běžící na hello stejného hostitele.

## <a name="size-tables"></a>Tabulky velikostí
Hello následující tabulky popisují hello velikosti a hello kapacity, které poskytují.

* Kapacita úložiště je v jednotkách GiB, tj. 1024^3 bajtů. Při porovnávání disků měřená v GB (1000 ^ 3 bajtů) toodisks měřená v GiB (1024 ^ 3) mějte na paměti, že kapacita čísel na základě předané v GiB může zobrazit menší. Například 1023 GiB = 1098,4 GB
* Propustnost disku se měří v počtu V/V operací za sekundu (IOPS) a v MB/s, kde 1 MB/s = 10^6 bajtů/s.
* Disky pro ukládání dat můžou fungovat v režimu s mezipamětí, nebo bez ní. Pro data uložená v mezipaměti operaci disku, hello hostitele mezipaměti režim je nastaven příliš**jen pro čtení** nebo **ReadWrite**. Pro operace bez vyrovnávací paměti dat disku, hello hostitele mezipaměti režim je nastaven příliš**žádné**.
* Maximální šířku pásma sítě je hello maximální agregované šířky pásma přidělené a přiřazené podle typu virtuálního počítače. Maximální šířka pásma Hello poskytuje pokyny pro výběr hello správné virtuálních počítačů typu tooensure odpovídající dostatečnou kapacitu sítě je k dispozici. Při přesunu mezi nízká, střední, vysoká a velmi vysoké, se zvyšuje i hello propustnost. Skutečný výkon sítě bude záviset na mnoha faktorech včetně zatížení aplikací a sítě a síťového nastavení aplikace.

## <a name="a-series"></a>A-Series
| Velikost            | Procesorová jádra | Paměť: GiB  | Místní HDD: GiB       | Max. počet NIC / Šířka pásma sítě |
|---------------- | --------- | ------------ | -------------------- | ---------------------------- |
| ExtraSmall      | 1         | 0,768        | 20                   | 1 / malá |
| Krátkodobé používání           | 1         | 1,75         | 225                  | 1 / střední |
| Střednědobé používání          | 2         | 3,5 GB       | 490                  | 1 / střední |
| Dlouhodobé používání           | 4         | 7            | 1000                 | 2 / vysoká |
| ExtraLarge      | 8         | 14           | 2040                 | 4 / vysoká |
| A5              | 2         | 14           | 490                  | 1 / střední |
| A6              | 4         | 28           | 1000                 | 2 / vysoká |
| A7              | 8         | 56           | 2040                 | 4 / vysoká |

## <a name="a-series---compute-intensive-instances"></a>A-series – Instance náročné na výpočetní výkon
Informace a důležité informace o použití těchto velikosti najdete v tématu [vysokovýkonné výpočetní velikosti virtuálních počítačů](../virtual-machines/windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

| Velikost            | Procesorová jádra | Paměť: GiB  | Místní HDD: GiB       | Max. počet NIC / Šířka pásma sítě |
|---------------- | --------- | ------------ | -------------------- | ---------------------------- |
| A8 *             |8          | 56           | 1817                 | 2 / vysoká |
| A9 *             |16         | 112          | 1817                 | 4 / velmi vysoká |
| A10             |8          | 56           | 1817                 | 2 / vysoká |
| A11             |16         | 112          | 1817                 | 4 / velmi vysoká |

\*Podporující RDMA

## <a name="av2-series"></a>Av2-series

| Velikost            | Procesorová jádra | Paměť: GiB  | Místní SSD: GiB       | Max. počet NIC / Šířka pásma sítě |
|---------------- | --------- | ------------ | -------------------- | ---------------------------- |
| Standard_A1_v2  | 1         | 2            | 10                   | 1 / střední                 |
| Standard_A2_v2  | 2         | 4            | 20                   | 2 / střední                 |
| Standard_A4_v2  | 4         | 8            | 40                   | 4 / vysoká                     |
| Standard_A8_v2  | 8         | 16           | 80                   | 8 / vysoká                     |
| Standard_A2m_v2 | 2         | 16           | 20                   | 2 / střední                 |
| Standard_A4m_v2 | 4         | 32           | 40                   | 4 / vysoká                     |
| Standard_A8m_v2 | 8         | 64           | 80                   | 8 / vysoká                     |


## <a name="d-series"></a>D-series
| Velikost            | Procesorová jádra | Paměť: GiB  | Místní SSD: GiB       | Max. počet NIC / Šířka pásma sítě |
|---------------- | --------- | ------------ | -------------------- | ---------------------------- |
| Standard_D1     | 1         | 3,5          | 50                   | 1 / střední |
| Standard_D2     | 2         | 7            | 100                  | 2 / vysoká |
| Standard_D3     | 4         | 14           | 200                  | 4 / vysoká |
| Standard_D4     | 8         | 28           | 400                  | 8 / vysoká |
| Standard_D11    | 2         | 14           | 100                  | 2 / vysoká |
| Standard_D12    | 4         | 28           | 200                  | 4 / vysoká |
| Standard_D13    | 8         | 56           | 400                  | 8 / vysoká |
| Standard_D14    | 16        | 112          | 800                  | 8 / velmi vysoká |

## <a name="dv2-series"></a>Dv2-series
| Velikost            | Procesorová jádra | Paměť: GiB  | Místní SSD: GiB       | Max. počet NIC / Šířka pásma sítě |
|---------------- | --------- | ------------ | -------------------- | ---------------------------- |
| Standard_D1_v2  | 1         | 3,5          | 50                   | 1 / střední |
| Standard_D2_v2  | 2         | 7            | 100                  | 2 / vysoká |
| Standard_D3_v2  | 4         | 14           | 200                  | 4 / vysoká |
| Standard_D4_v2  | 8         | 28           | 400                  | 8 / vysoká |
| Standard_D5_v2  | 16        | 56           | 800                  | 8 / velmi vysoká |
| Standard_D11_v2 | 2         | 14           | 100                  | 2 / vysoká |
| Standard_D12_v2 | 4         | 28           | 200                  | 4 / vysoká |
| Standard_D13_v2 | 8         | 56           | 400                  | 8 / vysoká |
| Standard_D14_v2 | 16        | 112          | 800                  | 8 / velmi vysoká |
| Standard_D15_v2 | 20        | 140          | 1 000                | 8 / velmi vysoká |

## <a name="g-series"></a>G-series
| Velikost            | Procesorová jádra | Paměť: GiB  | Místní SSD: GiB       | Max. počet NIC / Šířka pásma sítě |
|---------------- | --------- | ------------ | -------------------- | ---------------------------- |
| Standard_G1     | 2         | 28           | 384                  |1 / vysoká |
| Standard_G2     | 4         | 56           | 768                  |2 / vysoká |
| Standard_G3     | 8         | 112          | 1 536                |4 / velmi vysoká |
| Standard_G4     | 16        | 224          | 3 072                |8 / velmi vysoká |
| Na úrovni Standard_G5     | 32        | 448          | 6 144                |8 / velmi vysoká |

## <a name="h-series"></a>H-series
Virtuální počítače Azure H-series jsou hello další generace s vysokým výkonem, že virtuální počítače zaměřené na výpočetní potřeby vysoké end, jako molekulární modelování a výpočet dynamiky kapaliny. Tyto 8 a 16 jader virtuální počítače jsou postavené na technologii procesoru hello Intel. Haswell E5-2667 V3 poskytuje funkci DDR4 paměti a místní úložiště založená na SSD.

Kromě toho toohello významné výkon procesoru, hello H-series nabízí různé možnosti sítě s nízkou latencí RDMA pomocí FDR InfiniBand a několik paměti konfigurace toosupport náročné výpočetní požadavky na paměť.

| Velikost            | Procesorová jádra | Paměť: GiB  | Místní SSD: GiB       | Max. počet NIC / Šířka pásma sítě |
|---------------- | --------- | ------------ | -------------------- | ---------------------------- |
| Standard_H8     | 8         | 56           | 1000                 | 8 / vysoká |
| Standard_H16    | 16        | 112          | 2000                 | 8 / velmi vysoká |
| Standard_H8m    | 8         | 112          | 1000                 | 8 / vysoká |
| Standard_H16m   | 16        | 224          | 2000                 | 8 / velmi vysoká |
| Standard_H16r*  | 16        | 112          | 2000                 | 8 / velmi vysoká |
| Standard_H16mr* | 16        | 224          | 2000                 | 8 / velmi vysoká |

\*Podporující RDMA

## <a name="configure-sizes-for-cloud-services"></a>Konfigurace velikostí pro cloudové služby
Můžete zadat velikost virtuálního počítače hello instance role jako součást modelu služby hello popsaného hello [souboru definice služby](cloud-services-model-and-package.md#csdef). velikost Hello hello role určuje hello počet jader procesoru, kapacita paměti hello a velikost hello místního souboru systému, který je přidělen tooa spuštěna instance. Zvolte velikost role hello založené na požadavku prostředek vaší aplikace.

Tady je příklad pro nastavení toobe velikost role hello [Standard_D2](#general-purpose-d) pro instanci webové Role:

```xml
<WorkerRole name="Worker1" vmsize="Standard_D2">
...
</WorkerRole>
```

## <a name="changing-hello-size-of-an-existing-role"></a>Změna velikosti hello existující role

Jako hello povaze změny zatížení nebo nové velikosti virtuálních počítačů, které jsou k dispozici může být vhodné toochange hello velikost role. toodo, musíte změnit velikost virtuálního počítače hello v souboru definice služby (jak je uvedeno výše), znovu zabalte cloudové služby a nasadíte ho. Není možné toochange velikosti virtuálních počítačů přímo z portálu hello nebo prostředí PowerShell.

>[!TIP]
> Může být vhodné toouse různé velikosti virtuálních počítačů pro vaši roli v různých prostředích (např. produkční vs test). Jedním ze způsobů toodo to je toocreate více souborů definice (.csdef) služby ve vašem projektu a pak vytvořit jiné cloudové služby balíčky za prostředí během vaše automatizované sestavení pomocí nástroje CSPack hello. balíček toolearn Další informace o hello prvky cloud services a jak toocreate, najdete v části [co je hello cloudové služby modelu a jak ho balíček?](cloud-services-model-and-package.md)
>
>

## <a name="get-a-list-of-sizes"></a>Získat seznam velikostí
Můžete použít PowerShell nebo rozhraní REST API tooget seznam velikostí hello. Hello REST API je popsána [zde](https://msdn.microsoft.com/library/azure/dn469422.aspx). Hello následující kód je příkaz prostředí PowerShell, který zobrazí seznam všech velikostí hello aktuálně k dispozici pro cloudové služby.

```powershell
Get-AzureRoleSize | where SupportedByWebWorkerRoles -eq $true | select InstanceSize
```

## <a name="next-steps"></a>Další kroky
* Další informace o [Limitech, kvótách a omezeních předplatného a služeb Azure](../azure-subscription-service-limits.md)
* Další informace [o vysokovýkonné výpočetní velikosti virtuálních počítačů](../virtual-machines/windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) pro úlohy v prostředí HPC.
