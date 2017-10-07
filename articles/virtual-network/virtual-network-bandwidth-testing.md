---
title: "propustnost sítě virtuálního počítače Azure aaaTesting | Microsoft Docs"
description: "Zjistěte, jak tootest virtuální počítač Azure sítě propustnost."
services: virtual-network
documentationcenter: na
author: steveesp
manager: Gerald DeGrace
editor: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/21/2017
ms.author: steveesp
ms.openlocfilehash: 2da85c27bc8d16a443b215891f4cd0460f41926f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="bandwidththroughput-testing-ntttcp"></a>Šířky pásma nebo propustnost testování (NTTTCP)

Při testování propustnost sítě v Azure, je nejlepší toouse nástroj, který cílí hello sítě pro testování a minimalizuje hello použití další prostředky, které by mohlo mít vliv na výkon. Doporučuje se NTTTCP.

Zkopírujte nástroj tootwo hello virtuální počítače Azure hello stejnou velikost. Jeden virtuální počítač funguje jako odesílatele a hello jiných jako příjemce.

#### <a name="deploying-vms-for-testing"></a>Nasazení virtuálních počítačů pro testování
Pro účely tohoto testu hello hello dva virtuální počítače by měla být ve buď hello stejné cloudové služby nebo hello stejné skupiny dostupnosti, aby jsme mohli používat svoje interní IP adresy a vyloučit hello nástroje pro vyrovnávání zatížení z testu hello. Je možné tootest s hello virtuální IP adresy, ale tento druh testování je mimo rozsah hello tohoto dokumentu.
 
Poznamenejte si hello příjemce IP adresy. Umožňuje volání této IP "a.b.c.r"

Poznamenejte si hello počet jader na hello virtuálních počítačů. To umožňuje volání "\#num\_jader"
 
Spusťte hello NTTTCP otestovat 300 sekund (nebo 5 minut) na hello odesílatele virtuálních počítačů a příjemce virtuálních počítačů.

Tip: Při nastavování tento test pro hello poprvé, můžete se pokusit kratší názor období tooget testovací dříve. Jakmile nástroj hello funguje podle očekávání, rozšiřte hello zkušební období too300 sekund hello nejpřesnější výsledky.

> [!NOTE]
> Odesílatel Hello **a** musíte zadat příjemce **hello stejné** testovací parametr doba trvání (-t).

tootest jednoho datového proudu TCP 10 sekund:

Příjemce parametry: ntttcp - r -t 10 - P 1

Odesílatel parametry: ntttcp-s10.27.33.7 -t 10 - n 1 -P 1

> [!NOTE]
> Hello předchozích ukázkových by měly být použité tooconfirm jenom při konfiguraci. Testování platný příklady jsou popsané dál v tomto dokumentu.

## <a name="testing-vms-running-windows"></a>Testování virtuální počítače se systémem WINDOWS:

#### <a name="get-ntttcp-onto-hello-vms"></a>NTTTCP dostanou do hello virtuálních počítačů.

Stažení nejnovější verze hello: <https://gallery.technet.microsoft.com/NTttcp-Version-528-Now-f8b12769>

Nebo je-li přesunout hledání: <https://www.bing.com/search?q=ntttcp+download> \< – musí být nejprve dosáhl

Zvažte umístění NTTTCP samostatné složky, jako je c:\\nástroje

#### <a name="allow-ntttcp-through-hello-windows-firewall"></a>Povolit NTTTCP přes bránu firewall systému Windows hello
Na hello příjemce vytvořte pravidlo povolení na hello brány Windows Firewall tooallow tooarrive provoz NTTTCP. Místo příchozí tooallow specifické porty protokolu TCP je nejjednodušší tooallow hello celý NTTTCP program podle názvu.

Povolit ntttcp prostřednictvím hello brány Windows Firewall takto:

netsh advfirewall firewall přidejte pravidlo program =\<cesta\>\\ntttcp.exe název = "ntttcp" protokol = všechny dir = v akci = povolit povolit = yes profilu = ANY

Například, pokud jste zkopírovali ntttcp.exe toohello "c:\\nástroje" složky, bude příkaz hello: 

netsh advfirewall firewall přidejte pravidlo program = c:\\nástroje\\ntttcp.exe název = "ntttcp" protokol = všechny dir = v akci = povolit povolit = yes profilu = ANY

#### <a name="running-ntttcp-tests"></a>Spouštění testů NTTTCP

Spusťte NTTTCP na hello příjemce (**spustit z CMD**, nikoli z prostředí PowerShell):

ntttcp - r – m [2\*\#num\_jader],\*, a.b.c.r -t 300

Pokud hello virtuálních počítačů má čtyři jader a IP adresu 10.0.0.4, by vypadat například takto:

ntttcp - r – m 8,\*, 10.0.0.4 -t 300


Spusťte NTTTCP na hello odesílatele (**spustit z CMD**, nikoli z prostředí PowerShell):

ntttcp -s – m 8,\*, 10.0.0.4 -t 300 

Počkejte, než hello výsledků.


## <a name="testing-vms-running-linux"></a>Testování virtuální počítače se systémem LINUX:

Použijte nttcp pro linux. Je k dispozici z <https://github.com/Microsoft/ntttcp-for-linux>

Na hello virtuální počítače s Linuxem (odesílatele i příjemce) spusťte tyto příkazy a příprava ntttcp pro linux na virtuální počítače:

CentOS – instalace Git:
``` bash
  yum install gcc -y  
  yum install git -y
```
Ubuntu – instalace Git:
``` bash
 apt-get -y install build-essential  
 apt-get -y install git
```
Ujistěte se a nainstalujte na obou:
``` bash
 git clone <https://github.com/Microsoft/ntttcp-for-linux>
 cd ntttcp-for-linux/src
 make && make install
```

Jako třeba Windows hello jsme předpokládají, že příjemce Linux hello IP je 10.0.0.4

Spusťte na hello příjemce NTTTCP pro Linux:

``` bash
ntttcp -r -t 300
```

A na hello odesílatele, spusťte:

``` bash
ntttcp -s10.0.0.4 -t 300
```
 
Test délka výchozí too60 sekund Pokud žádný čas parametr je zadána.

## <a name="testing-between-vms-running-windows-and-linux"></a>Testování mezi virtuální počítače se systémem Windows a LINUX:

Pro tohoto scénáře jsme by měl povolit režim hello no-sync, můžete spustit hello test. To se provádí pomocí hello **-N příznak** pro systémy Linux a **příznak -ns** pro systém Windows.

#### <a name="from-linux-toowindows"></a>Z Linux tooWindows:

Příjemce <Windows>:

``` bash
ntttcp -r -m <2 x nr cores>,*,<Windows server IP>
```

Odesílatel <Linux> :

``` bash
ntttcp -s -m <2 x nr cores>,*,<Windows server IP> -N -t 300
```

#### <a name="from-windows-toolinux"></a>Z Windows tooLinux:

Příjemce <Linux>:

``` bash
ntttcp -r -m <2 x nr cores>,*,<Linux server IP>
```

Odesílatel <Windows>:

``` bash
ntttcp -s -m <2 x nr cores>,*,<Linux  server IP> -ns -t 300
```

## <a name="next-steps"></a>Další kroky
* V závislosti na výsledky, je možné, místnosti příliš[optimalizovat počítače propustnost sítě](virtual-network-optimize-network-bandwidth.md) pro váš scénář.
* Další informace s [Azure Virtual Network nejčastější dotazy (FAQ)](virtual-networks-faq.md)
