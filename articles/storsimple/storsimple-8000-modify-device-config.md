---
title: "Konfigurace zařízení řady StorSimple 8000 hello aaaModify | Microsoft Docs"
description: "Popisuje, jak toouse hello tooreconfigure služby StorSimple Manager zařízení StorSimple zařízení, která již byla nasazena."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 07/19/2017
ms.author: alkohli
ms.openlocfilehash: 79711b45eafe732c1eed1e562b05e3837fbc9d03
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-toomodify-your-storsimple-device-configuration"></a>Použít toomodify služby StorSimple Manager zařízení hello konfiguraci zařízení StorSimple

## <a name="overview"></a>Přehled

portál Azure Hello **nastavení zařízení** část v hello **nastavení** okno obsahuje všechny parametry hello zařízení, které můžete změnit konfiguraci na zařízení StorSimple, který je spravován pomocí Správce zařízení StorSimple Služba. Tento kurz vysvětluje, jak je možné používat hello **nastavení** hello tooperform okno následující úlohy na úrovni zařízení:

* Změnit popisný název zařízení
* Upravit nastavení času zařízení
* Přiřadit sekundární server DNS
* Upravit síťová rozhraní
* Swap – nebo změna přiřazení IP adresy

## <a name="modify-device-friendly-name"></a>Změnit popisný název zařízení

Můžete použít název hello Azure portálu toochange hello zařízení a přiřaďte ho jedinečný popisný název podle svého výběru. Použití hello **obecné nastavení** okno na vašem zařízení toomodify hello popisný název zařízení. Hello popisný název může obsahovat libovolné znaky a může být maximálně 64 znaků.

> [!NOTE] 
> Název zařízení hello v hello portálu Azure můžete upravit pouze před dokončením instalačního programu hello zařízení. Po dokončení hello minimální instalace zařízení nemůžete změnit název zařízení hello.

![Název zařízení obecné nastavení](./media/storsimple-8000-modify-device-config/modify-general-settings3.png)

Zařízení StorSimple, který je připojený toohello služby StorSimple Manager zařízení je přiřazen výchozí název. Výchozí název Hello obvykle odráží hello sériové číslo zařízení hello. Výchozí název zařízení, která je 15 znaků, jako je například 8600-SHX0991003G44HT, například označuje hello následující:

* **8600** – model zařízení označuje hello.
* **TVX** – označuje hello výrobní lokality.
* **0991003** -označuje konkrétní produkt.
* **G44HT**– hello posledních 5 číslic jsou zvýšena toocreate jedinečný sériová čísla. Toto nemusí být po sobě jdoucích sadu.

## <a name="modify-device-description"></a>Upravit popis zařízení

Použití hello **obecné nastavení** okně popis zařízení hello toomodify vaše zařízení.

![Popis zařízení v obecné nastavení](./media/storsimple-8000-modify-device-config/modify-general-settings4.png)

Popis zařízení obvykle pomáhá identifikovat hello vlastníka a hello fyzické umístění hello zařízení. pole popisu Hello musí obsahovat méně než 256 znaků.

## <a name="modify-time-settings"></a>Upravit nastavení času

Zařízení musí synchronizovat čas v pořadí tooauthenticate u vašeho poskytovatele služeb úložiště v cloudu. Použití hello **obecné nastavení** okno na nastavení zařízení toomodify hello zařízení času.

![Popis zařízení v obecné nastavení](./media/storsimple-8000-modify-device-config/modify-general-settings2.png)

 Hello rozevíracím seznamu vyberte časové pásmo. Můžete zadat až servery tootwo Protokol NTP (Network Time):

 - **Primární server NTP** -hello konfigurace je povinná a je zadána při použití Windows Powershellu pro StorSimple tooconfigure zařízení. Můžete zadat výchozí hello systému Windows Server **time.windows.com** jako NTP server. Hello primární NTP serveru konfiguraci prostřednictvím hello portálu Azure můžete zobrazit, ale je nutné použít hello prostředí Windows PowerShell rozhraní toochange ho. Použití hello `Set-HcsNTPClientServerAddress` rutiny toomodify hello primární server NTP vašeho zařízení. Další informace přejděte toosynxtax pro rutinu [Set-HcsNTPClientServerAddress] (https://technet.microsoft.com/library/dn688138.aspx).

- **Sekundární server NTP** -hello konfigurace je volitelné. Můžete použít hello portálu tooconfigure sekundárního serveru NTP.

Při konfiguraci serveru NTP hello, ujistěte se, že vaše síť umožňuje toopass provoz hello NTP z vašeho datového centra toohello Internetu. Při zadávání veřejného serveru NTP, musí se ujistěte, že vaše síťové brány firewall a dalších zařízení zabezpečení jsou nakonfigurované tooallow NTP provoz tootravel tooand z hello mimo síť. Pokud přenos dat NTP obousměrného není povolena, je nutné použít interní server NTP (řadič domény systému Windows poskytuje funkce). Pokud vaše zařízení nelze synchronizovat čas, nemusí být možné toocommunicate u svého poskytovatele úložiště v cloudu.

toosee seznam veřejné servery NTP, přejděte toohello [webové servery NTP](http://support.ntp.org/bin/view/Servers/WebHome).

### <a name="what-happens-if-hello-device-is-deployed-in-a-different-time-zone"></a>Co se stane, když zařízení hello je nasazena v jiném časovém pásmu?

Pokud zařízení hello je nasazena v jiném časovém pásmu, změní se hello zařízení časové pásmo. Vzhledem k tomu, že všechny zásady zálohování hello použít hello zařízení časové pásmo, zásady zálohování hello automaticky upraví v souladu s novým časovým pásmem hello. Není vyžadován žádný zásah uživatele.

## <a name="modify-dns-settings"></a>Změna nastavení DNS

DNS server se používá, když se zařízení pokusí toocommunicate u vašeho poskytovatele služeb úložiště v cloudu. Použití hello **nastavení síťového** okno na vašem zařízení tooview a upravit nastavení DNS hello nakonfigurované. 

![Nastavení DNS v nastavení sítě](./media/storsimple-8000-modify-device-config/modify-network-settings1.png)

Pro zajištění vysoké dostupnosti jsou požadované tooconfigure obou hello primární a hello sekundární servery DNS při nasazení hello počáteční zařízení.

**Primární server DNS** -StorSimple toofirst zadejte hello primárního serveru DNS při počátečním nastavení hello použít hello prostředí Windows PowerShell. Můžete změnit konfiguraci hello primární server DNS pouze pomocí rozhraní Windows PowerShell hello. Použití hello `Set-HcsDNSClientServerAddress` rutiny toomodify hello primární server DNS vašeho zařízení. Další informace, přejděte toosynxtax [Set-HcsDNSClientServerAddress](https://technet.microsoft.com/library/dn688138.aspx) rutiny.

**Sekundární server DNS** -toomodify hello sekundární server DNS, použijte hello `Set-HcsDNSClientServerAddress` rutiny v prostředí Windows PowerShell rozhraní hello hello zařízení nebo **nastavení síťového** okno zařízení StorSimple v hello Azure portál.

toomodify hello sekundární server DNS na portálu Azure, proveďte následující kroky hello.

1. Přejděte služby StorSimple Manager zařízení tooyour. Hello seznam zařízení vyberte a klikněte na zařízení.

2. V hello **nastavení** okně přejděte příliš**nastavení zařízení > sítě**. Otevře se hello **nastavení síťového** okno. Klikněte na tlačítko **nastavení DNS** dlaždici. Upravte hello sekundární IP adresa serveru DNS.

    ![Upravit sekundární adderss IP serveru DNS](./media/storsimple-8000-modify-device-config/modify-secondary-dns1.png)

4. Na panelu příkazů hello, klikněte na **Uložit** a po zobrazení výzvy k potvrzení, klikněte na tlačítko **OK**.

    ![Uložte a potvrďte změny](./media/storsimple-8000-modify-device-config/modify-secondary-dns-2.png)



## <a name="modify-network-interfaces"></a>Upravit síťová rozhraní

Vaše zařízení má šest zařízení síťových rozhraní, čtyři z nich jsou 1gbe a dvě z nich jsou 10 GbE. Tato rozhraní jsou označeny jako DATA 0 – DATA 5. DATA 0, 1 dat, DATA 4 a 5 dat jsou 1gbe, zatímco 10 GbE síťová rozhraní DATA 2 a DATA 3.

Použití hello **nastavení síťového** okno tooconfigure každý z hello rozhraní toobe použít.

![Konfigurace síťových rozhraní pomocí nastavení sítě](./media/storsimple-8000-modify-device-config/modify-network-settings3.png)

tooensure vysokou dostupnost, doporučujeme mít aspoň dvě rozhraní iSCSI a dvě rozhraní povolenou podporu cloudu na zařízení. Jsme doporučujeme, ale nevyžadují zakázat nepoužívané rozhraní.

Pro každé síťové rozhraní zobrazí se hello následující parametry:

* **Rychlost** – není uživatelsky konfigurovatelného parametr. DATA 0, 1 dat, DATA 4 a 5 dat jsou vždy 1 GbE, zatímco 10 GbE rozhraní DATA 2 a DATA 3.
  
  > [!NOTE]
  > Rychlost a duplexní režim jsou vždy automaticky vyjednal. Rámce typu Jumbo nejsou podporovány.
  
* **Stav rozhraní** – rozhraní můžete povolit nebo zakázat. Pokud je povoleno, zařízení hello pokusí toouse hello rozhraní. Doporučujeme, aby byl povolen pouze rozhraní, které jsou připojené toohello sítě a použít. Zakažte všechny rozhraní, které nepoužíváte.
* **Typ rozhraní** – tento parametr můžete přenosy iSCSI tooisolate od provozu cloudu úložiště. Tento parametr může být jedna z následujících hello:
  
  * **Cloud povolené** – když je povolené, hello zařízení bude používat toto rozhraní toocommunicate hello cloudu.
  * **iSCSI povoleno** – když je povolené, zařízení hello použije toto rozhraní toocommunicate hello iSCSI hostitele.
    
    Doporučujeme, abyste izolovat přenosy iSCSI od provozu cloudu úložiště. Všimněte si, pokud je váš hostitel v rámci hello stejné podsíti jako zařízení, není nutné tooassign bránu; ale pokud váš hostitel je v jiné podsíti, než zařízení, budete potřebovat tooassign bránu.
* **IP adresa** – Pokud nakonfigurujete všechny hello síťových rozhraní, je nutné nakonfigurovat virtuální IP (VIP). To může být IPv4 nebo IPv6 nebo obojí. Hello IPv4 i IPv6 rodiny adres jsou podporovány pro hello zařízení síťových rozhraní. Při použití protokolu IPv4, zadejte IP adresu 32-bit (*xxx.xxx.xxx.xxx*) v notaci tečkou decimal. Pokud používáte IPv6, jednoduše zadejte předponu 4 číslice a adresu 128-bit budou automaticky generovány pro vaše zařízení síťové rozhraní na základě této předpony.
* **Podsíť** – to odkazuje toohello masku podsítě a je nakonfigurován pomocí rozhraní Windows PowerShell hello.
* **Brána** – Toto je výchozí brána hello, který toto rozhraní se použije, když se ho pokusí toocommunicate s uzly, které nejsou v rámci hello stejné adresní prostor IP adres (podsítě). Hello výchozí brány musí být v hello stejné adresní prostor (podsítě) jako hello rozhraní IP adresa, počítáno od hello maska podsítě.
* **Pevná IP adresa** – toto pole je dostupné pouze tehdy, když nakonfigurujete hello DATA 0 rozhraní. Pro operace, jako aktualizace nebo řešení potíží hello zařízení, pravděpodobně bude třeba tooconnect přímo toohello řadiče zařízení. Hello pevné IP adresy lze použít tooaccess hello aktivní a pasivní řadiče hello na vašem zařízení.

> [!NOTE]
> * tooensure správné operace, ověřte rychlost rozhraní hello a duplexní režim na přepínači hello, která je každé rozhraní zařízení připojena k. Přepínač rozhraní by měl buď vyjednat s nebo se dá nakonfigurovat pro adaptéry Gigabit Ethernet (1000 Mb/s) a být plně duplexní. Rozhraní pracující na nižší rychlost nebo v poloduplexní způsobí problémy s výkonem.
> * toominimize přerušení a výpadky, doporučujeme, abyste povolili portfast na každém hello přepínače, které porty, které hello síťové rozhraní iSCSI vašeho zařízení se připojují k. Tím bude zajištěno, že v případě hello převzetí služeb při selhání můžete rychle vytvořit připojení k síti.

### <a name="configure-data-0"></a>Konfigurovat DATA 0

Rozhraní DATA 0 má povolenou podporu cloudu ve výchozím nastavení. Při konfiguraci DATA 0, jste také vyžaduje tooconfigure dva pevné IP adresy, jednu pro každý kontroler. Tyto pevné IP adresy lze použít tooaccess řadiče zařízení hello přímo a jsou užitečné, když instalujete aktualizace na hello zařízení nebo když přistupujete hello řadiče hello za účelem řešení potíží.

Hello pevné IP řadiče prostřednictvím okno hello DATA 0 nastavení můžete změnit konfiguraci.

![Nakonfigurujte rozhraní sítě - DATA 0](./media/storsimple-8000-modify-device-config/modify-network-settings2.png)

> [!NOTE]
> pevné IP adresy pro řadič hello Hello se používají pro obsluhu hello aktualizace toohello zařízení. Proto hello pevné IP adresy musí být směrovatelné a musí umožňovat tooconnect toohello Internetu.

### <a name="configure-data-1---data-5"></a>Konfigurovat DATA 1 - DATA 5

Pro DATA 1 - 5 síťová rozhraní DATA, můžete nakonfigurovat všechna nastavení sítě hello, jak ukazuje následující snímek obrazovky hello:

![Nakonfigurujte síťová rozhraní DATA 1 - 5 dat](./media/storsimple-8000-modify-device-config/modify-network-settings4.png)


## <a name="swap-or-reassign-ips"></a>Swap – nebo změna přiřazení IP adresy

Pokud žádné síťové rozhraní na řadič hello je přiřazeny VIP, která je používána (podle hello stejné zařízení nebo jiné zařízení v síti hello), pak řadič hello bude převzetí služeb při selhání. Swap nebo změnu přiřazení virtuální IP adresy pro síťové rozhraní zařízení, je nutné postupovat podle správné postupu jako můžete vytvořit duplicitní IP situaci.

Proveďte následující kroky tooswap hello nebo změna přiřazení hello VIP pro žádné hello síťových rozhraní:

#### <a name="tooreassign-ips"></a>tooreassign IP adresy

1. Vymazat hello IP adresu pro obě rozhraní.
2. Po hello IP adresy jsou vymazány, přiřaďte nové IP adresy hello toohello příslušných rozhraní.

## <a name="next-steps"></a>Další kroky

* Zjistěte, jak příliš[konfigurace funkce MPIO pro zařízení StorSimple](storsimple-8000-configure-mpio-windows-server.md).
* Zjistěte, jak příliš[použití hello tooadminister service Manager zařízení StorSimple zařízení StorSimple](storsimple-8000-manager-service-administration.md).

