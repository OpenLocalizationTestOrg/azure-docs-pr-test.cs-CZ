---
title: "Konfigurace zařízení StorSimple hello aaaModify | Microsoft Docs"
description: "Popisuje, jak toouse hello tooreconfigure služby StorSimple Manager zařízení StorSimple, která již byla nasazena."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: bb679264-8d46-429c-9ef7-630dc3b4a423
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 07/19/2017
ms.author: v-sharos
ms.openlocfilehash: 10a54c191260bf1baba58d28cdbfa0ed72217f48
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-toomodify-your-storsimple-device-configuration"></a>Použít toomodify služby StorSimple Manager hello konfiguraci zařízení StorSimple
## <a name="overview"></a>Přehled
portál Azure classic Hello **konfigurace** stránka obsahuje všechny parametry hello zařízení, které můžete změnit konfiguraci na zařízení StorSimple, který je spravovaný nástrojem služby StorSimple Manager. Tento kurz vysvětluje, jak je možné používat hello **konfigurace** stránku hello tooperform následující úlohy na úrovni zařízení:

* Upravit nastavení zařízení 
* Upravit nastavení času 
* Změna nastavení DNS 
* Upravit síťová rozhraní
* Swap – nebo změna přiřazení IP adresy

## <a name="modify-device-settings"></a>Upravit nastavení zařízení
nastavení zařízení Hello zahrnují hello popisný název zařízení hello a popis zařízení hello.

> [!NOTE] 
> Nelze upravit název zařízení hello v hello portál Azure classic. Přejmenování zařízení hello není podporováno.

Zařízení StorSimple, který je připojený toohello služby StorSimple Manager je přiřazen výchozí název. Výchozí název Hello obvykle odráží hello sériové číslo zařízení hello. Výchozí název zařízení, která je 15 znaků, jako je například 8600-SHX0991003G44HT, například označuje hello následující:

* **8600** – model zařízení označuje hello.
* **TVX** – označuje hello výrobní lokality.
* **0991003** -označuje konkrétní produkt.
* **G44HT**– hello posledních 5 číslic jsou zvýšena toocreate jedinečný sériová čísla. Toto nemusí být po sobě jdoucích sadu.

Můžete zadat popis zařízení. Popis zařízení obvykle pomáhá identifikovat hello vlastníka a hello fyzické umístění hello zařízení. pole popisu Hello musí obsahovat méně než 256 znaků.

## <a name="modify-time-settings"></a>Upravit nastavení času
Zařízení musí synchronizovat čas v pořadí tooauthenticate u vašeho poskytovatele služeb úložiště v cloudu. Vyberte časové pásmo hello rozevíracího seznamu a zadejte až servery tootwo Protokol NTP (Network Time). primární server NTP Hello je vyžadován a je zadána při použití Windows Powershellu pro StorSimple tooconfigure zařízení. Můžete zadat výchozí hello systému Windows Server **time.windows.com** jako NTP server. Hello primární NTP serveru konfiguraci prostřednictvím hello portál Azure classic můžete zobrazit, ale je nutné použít hello prostředí Windows PowerShell rozhraní toochange ho.

Konfigurace serveru NTP sekundární Hello je volitelný. Můžete použít hello klasického portálu tooconfigure sekundárního serveru NTP. 

Při konfiguraci serveru NTP hello, ujistěte se, že vaše síť umožňuje toopass provoz hello NTP z vašeho datového centra toohello Internetu. Při zadávání veřejného serveru NTP, musí se ujistěte, že vaše síťové brány firewall a dalších zařízení zabezpečení jsou nakonfigurované tooallow NTP provoz tootravel tooand z hello mimo síť. Pokud přenos dat NTP obousměrného není povolena, je nutné použít interní server NTP (řadič domény systému Windows poskytuje funkce). Pokud vaše zařízení nelze synchronizovat čas, nemusí být možné toocommunicate u svého poskytovatele úložiště v cloudu.

toosee seznam veřejné servery NTP, přejděte toohello [webové servery NTP](http://support.ntp.org/bin/view/Servers/WebHome). 

### <a name="what-happens-if-hello-device-is-deployed-in-a-different-time-zone"></a>Co se stane, když zařízení hello je nasazena v jiném časovém pásmu?
Pokud zařízení hello je nasazena v jiném časovém pásmu, změní se hello zařízení časové pásmo. Vzhledem k tomu, že všechny zásady zálohování hello použít hello zařízení časové pásmo, zásady zálohování hello automaticky upraví v souladu s novým časovým pásmem hello. Není vyžadován žádný zásah uživatele.

## <a name="modify-dns-settings"></a>Změna nastavení DNS
DNS server se používá, když se zařízení pokusí toocommunicate u vašeho poskytovatele služeb úložiště v cloudu. Pro zajištění vysoké dostupnosti jsou požadované tooconfigure obou hello primární a hello sekundární servery DNS při nasazení hello počáteční zařízení. tooreconfigure hello primární server DNS, budete potřebovat rozhraní Windows PowerShell hello toouse zařízení StorSimple.

toomodify hello sekundární server DNS, můžete použít hello portál Azure classic.

## <a name="modify-network-interfaces"></a>Upravit síťová rozhraní
Vaše zařízení má šest zařízení síťových rozhraní, čtyři z nich jsou 1gbe a dvě z nich jsou 10 GbE. Tato rozhraní jsou označeny jako DATA 0 – DATA 5. DATA 0, 1 dat, DATA 4 a 5 dat jsou 1gbe, zatímco 10 GbE síťová rozhraní DATA 2 a DATA 3.

Konfigurace **nastavení síťového rozhraní** pro každou z toobe rozhraní hello používá. tooensure vysokou dostupnost, doporučujeme mít aspoň dvě rozhraní iSCSI a dvě rozhraní povolenou podporu cloudu na zařízení. Jsme doporučujeme, ale nevyžadují zakázat nepoužívané rozhraní.

Když konfigurujete žádné hello síťových rozhraní, musíte nakonfigurovat virtuální IP (VIP).

Rozhraní DATA 0 má povolenou podporu cloudu ve výchozím nastavení. Při konfiguraci DATA 0, jste také vyžaduje tooconfigure dva pevné IP adresy, jednu pro každý kontroler. Tyto pevné IP adresy lze použít tooaccess řadiče zařízení hello přímo a jsou užitečné, když instalujete aktualizace na hello zařízení nebo když přistupujete hello řadiče hello za účelem řešení potíží.

V zařízení StorSimple 8000 řady Update 1 hello směrování metrika dat 0 je nastavený toohello nejnižší; Proto pokud vaše zařízení používá StorSimple 8000 řady Update 1, veškerý provoz cloudové hello budou směrovány přes DATA 0. Poznamenejte si tohoto Pokud máte více než jedno povolenou podporu cloudu síťové rozhraní na zařízení StorSimple.

> [!NOTE]
> pevné IP adresy pro řadič hello Hello se používají pro obsluhu hello aktualizace toohello zařízení. Proto hello pevné IP adresy musí být směrovatelné a musí umožňovat tooconnect toohello Internetu.
> 
> 

Pro každé síťové rozhraní zobrazí se hello následující parametry:

* **Rychlost** – není uživatelsky konfigurovatelného parametr. DATA 0, 1 dat, DATA 4 a 5 dat jsou vždy 1 GbE, zatímco 10 GbE rozhraní DATA 2 a DATA 3.
  
  > [!NOTE]
  > Rychlost a duplexní režim jsou vždy automaticky vyjednal. Rámce typu Jumbo nejsou podporovány.
  > 
  > 
* **Stav rozhraní** – rozhraní můžete povolit nebo zakázat. Pokud je povoleno, zařízení hello pokusí toouse hello rozhraní. Doporučujeme, aby byl povolen pouze rozhraní, které jsou připojené toohello sítě a použít. Zakažte všechny rozhraní, které nepoužíváte.
* **Typ rozhraní** – tento parametr můžete přenosy iSCSI tooisolate od provozu cloudu úložiště. Tento parametr může být jedna z následujících hello:
  
  * **Cloud povolené** – když je povolené, hello zařízení bude používat toto rozhraní toocommunicate hello cloudu.
  * **iSCSI povoleno** – když je povolené, zařízení hello použije toto rozhraní toocommunicate hello iSCSI hostitele.
    
    Doporučujeme, abyste izolovat přenosy iSCSI od provozu cloudu úložiště. Všimněte si, pokud je váš hostitel v rámci hello stejné podsíti jako zařízení, není nutné tooassign bránu; ale pokud váš hostitel je v jiné podsíti, než zařízení, budete potřebovat tooassign bránu.
* **IP adresa** – to může být IPv4 nebo IPv6 nebo obojí. Hello IPv4 i IPv6 rodiny adres jsou podporovány pro hello zařízení síťových rozhraní. Při použití protokolu IPv4, zadejte IP adresu 32-bit (*xxx.xxx.xxx.xxx*) v notaci tečkou decimal. Pokud používáte IPv6, jednoduše zadejte předponu 4 číslice a adresu 128-bit budou automaticky generovány pro vaše zařízení síťové rozhraní na základě této předpony.
* **Podsíť** – to odkazuje toohello masku podsítě a je nakonfigurován pomocí rozhraní Windows PowerShell hello.
* **Brána** – Toto je výchozí brána hello, který toto rozhraní se použije, když se ho pokusí toocommunicate s uzly, které nejsou v rámci hello stejné adresní prostor IP adres (podsítě). Hello výchozí brány musí být v hello stejné adresní prostor (podsítě) jako hello rozhraní IP adresa, počítáno od hello maska podsítě.
* **Pevná IP adresa** – toto pole je dostupné pouze tehdy, když nakonfigurujete hello DATA 0 rozhraní. Pro operace, jako aktualizace nebo řešení potíží hello zařízení, pravděpodobně bude třeba tooconnect přímo toohello řadiče zařízení. Hello pevné IP adresy lze použít tooaccess hello aktivní a pasivní řadiče hello na vašem zařízení.

Prostřednictvím hello portál Azure classic můžete znovu nakonfigurovat řadič 0 a řadič 1.

> [!NOTE]
> * tooensure správné operace, ověřte rychlost rozhraní hello a duplexní režim na přepínači hello, která je každé rozhraní zařízení připojena k. Přepínač rozhraní by měl buď vyjednat s nebo se dá nakonfigurovat pro adaptéry Gigabit Ethernet (1000 Mb/s) a být plně duplexní. Rozhraní pracující na nižší rychlost nebo v poloduplexní způsobí problémy s výkonem.
> * toominimize přerušení a výpadky, doporučujeme, abyste povolili portfast na každém hello přepínače, které porty, které hello síťové rozhraní iSCSI vašeho zařízení se připojují k. Tím bude zajištěno, že v případě hello převzetí služeb při selhání můžete rychle vytvořit připojení k síti.
> 
> 

## <a name="swap-or-reassign-ips"></a>Swap – nebo změna přiřazení IP adresy
Pokud žádné síťové rozhraní na řadič hello je přiřazeny VIP, která je používána (podle hello stejné zařízení nebo jiné zařízení v síti hello), pak řadič hello bude převzetí služeb při selhání. Proto musíte toofollow hello správné postup pokud jsou vzájemná záměna virtuální IP adresy pro síťové rozhraní hello zařízení, vzhledem k tomu, že se vytvoří duplicitní IP situaci.

Proveďte následující kroky tooswap hello nebo změna přiřazení hello VIP pro žádné hello síťových rozhraní:

#### <a name="tooreassign-ips"></a>tooreassign IP adresy
1. Vymazat hello IP adresu pro obě rozhraní.
2. Po hello IP adresy jsou vymazány, přiřaďte nové IP adresy hello toohello příslušných rozhraní.

## <a name="next-steps"></a>Další kroky
* Zjistěte, jak příliš[konfigurace funkce MPIO pro zařízení StorSimple](storsimple-configure-mpio-windows-server.md).
* Zjistěte, jak příliš[použití hello tooadminister služby StorSimple Manager zařízení StorSimple](storsimple-manager-service-administration.md).

