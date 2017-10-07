---
title: "funkce zabezpečení v síti aaaProtect osobní data v Azure | Microsoft Docs"
description: "Ochrana osobních dat pomocí funkce zabezpečení sítě Azure"
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/22/2017
ms.author: barclayn
ms.custom: 
ms.openlocfilehash: be112a9408d327ccedf871656afe800fc7f775e3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="protect-personal-data-with-network-security-features-azure-application-gateway-and-network-security-groups"></a>Ochrana osobních dat pomocí funkce zabezpečení sítě: Azure Application Gateway a skupiny zabezpečení sítě

Tento článek obsahuje informace a postupy, které vám pomůže používat Azure Application Gateway a skupiny zabezpečení sítě tooprotect osobní data.

Důležitým prvkem v víceúrovňová zabezpečení strategie tooprotect hello ochrany osobních údajů osobních údajů je obrana proti běžné zneužití ohrožení zabezpečení, například Injektáž SQL nebo skriptování mezi servery. Zachování nežádoucí síťový provoz z virtuální sítě Azure pomáhá chránit proti potenciální ohrožení citlivých dat a nástroje toohelp chránit vaše data před útočníkům poskytuje Microsoft Azure.

## <a name="scenario"></a>Scénář

Velké výletních společnosti, centrálou ve Spojených státech amerických hello je rozšířit jeho itineráře toooffer operace v hello Středozemního, Jaderského a baltský moři, jakož i hello Britské ostrovy. Při podpoře těchto úsilí, získala několik menších výletní řádky na základě v Itálii, Německo, Dánsko a hello Spojené království

Hello společnost používá Microsoft Azure cloud toostore podnikových dat v hello a spouštět aplikace na virtuální počítače, které zpracovat a přístup k těmto datům. Tato data obsahují osobní identifikovatelné údaje, například jména, adresy, telefonních čísel a informace o kreditní kartě z jeho základní globální zákazníka. Ve všech umístěních zahrnuje také tradiční informace lidských zdrojů, jako jsou adresy, telefonních čísel, daň identifikačními čísly a lékařské informace o zaměstnance společnosti. Hello výletních řádku také udržuje velké databáze potřebu a věrnost program členů, která zahrnuje osobní údaje tootrack vztahů se zákazníky aktuální a starší.

Zaměstnanci společnosti přístupu hello síti ze vzdálených pobočkách a agenty cesta nachází kolem hello, world hello společnosti mají přístup k prostředkům společnosti toosome a používat webové aplikace hostované ve virtuálních počítačích Azure toointeract s ním.

## <a name="problem-statement"></a>Popis problému

Hello společnosti musí chránit hello ochranu osobních údajů zákazníků a osobní data zaměstnanců útočníky, kteří využívají softwaru ohrožení zabezpečení toorun škodlivého kódu, který mohla vystavit osobní data uložená nebo používané hello společnosti cloudové aplikace.

## <a name="company-goal"></a>Cílem společnosti

Hello tooensure cílem společnosti, který neoprávněné osoby nemají přístup k podnikové sítě virtuální Azure a hello aplikace a data, která jsou umístěny existuje zneužitím známých chyb zabezpečení. 

## <a name="solutions"></a>Řešení

Microsoft Azure poskytuje zabezpečení mechanismy toohelp zabránit nežádoucí provoz v přechodu do virtuálních sítí Azure. Řízení příchozí a odchozí přenosy tradičně provádí brány firewall. V Azure můžete použít hello Application Gateway s hello brány Firewall webových aplikací a skupin zabezpečení sítě (NSG), které fungují jako jednoduchý distribuovanou bránou firewall. Tyto nástroje umožňují toodetect a blokovat nežádoucí síťový provoz.

### <a name="application-gatewayweb-application-firewall"></a>Brány Firewall aplikací brány nebo webové aplikace

Hello [brány Firewall webových aplikací](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) (firewall webových aplikací) součást hello [Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction) chrání webových aplikací, které jsou stále cíle nebezpečné útoky, které využívají známé běžné chyby zabezpečení. Centralizované firewall webových aplikací chrání před útoky na web a zjednodušuje správu zabezpečení bez nutnosti změny aplikace.

Různé kategorie útoku, včetně Injektáž SQL, skriptování mezi weby, porušení protokolu HTTP a anomálií, robotů, prohledávací moduly, skenerů, časté nesprávné konfigurace aplikace, HTTP Denial of Service a další běžné útoky, jako adresy Azure firewall webových aplikací příkaz vkládání, požadavek HTTP pašování, rozdělení odpovědi HTTP a útoky zahrnutí vzdáleného souboru. 

Můžete vytvořit služby application gateway s firewall webových aplikací nebo přidat firewall webových aplikací tooan existující aplikační brány. V obou případech Azure Application Gateway vyžaduje vlastní podsíti.

#### <a name="how-do-i-create-an-application-gateway-with-waf"></a>Vytvoření služby application gateway s firewall webových aplikací 

toocreate novou aplikační bránu s firewall webových aplikací povoleno, hello následující:

1. Protokol v toohello portál Azure a hello **Oblíbené** podokně hello portálu, klikněte na tlačítko **nový**

2. V hello **nový** okně klikněte na tlačítko **sítě**.

3. Klikněte na tlačítko **Aplikační brána**.

4. Přejděte toohello portál Azure, **kliknutím na tlačítko Nová \> sítě \> Application Gateway.**

   ![vytváření application Gateway](media/protect-netsec/app-gateway-01.png)

5. V hello **Základy** okno, které se zobrazí, zadejte hodnoty hello hello následující pole: název, vrstvy (Standard nebo firewall webových aplikací), velikost SKU (malé, střední nebo velké) Instance počet (2 pro zajištění vysoké dostupnosti), předplatné, skupinu prostředků, a Umístění.

6. V hello **nastavení** okno, které se zobrazí pod **virtuální síť**, klikněte na tlačítko **vyberte virtuální síť**. Tento krok, který otevře zadejte hello vyberte okna virtuální sítě.

7. Klikněte na tlačítko **vytvořit nový** tooopen hello **vytvořit virtuální síť** okno.

8. Zadejte následující hodnoty hello: název, adresní prostor, název podsítě, rozsah adres podsítě. Klikněte na **OK**.

9. Na hello **nastavení** okno pod **konfigurace IP front-endu**, vyberte typ hello IP adresy.

10. Klikněte na tlačítko **zvolte veřejnou IP adresu,** pak **vytvořit nové.**

11. Přijměte výchozí hodnotu hello a klikněte na tlačítko **OK.**

12. Na hello **nastavení** okno pod **konfiguraci naslouchacího procesu**, vyberte toouse protokolu HTTP nebo HTTPS v části **protokol**. toouse HTTPS, je vyžadován certifikát.

13. Konfigurovat konkrétní nastavení hello firewall webových aplikací: **brány Firewall stav** (**povoleno**) a **režimu brány Firewall** (**prevence**). Pokud se rozhodnete **detekce** jako hello režimu, je provoz jenom protokolována.

14. Zkontrolujte hello **Souhrn** a klikněte na tlačítko **OK**. Aplikační brána hello je nyní zařazen do fronty a vytvořit.

Po vytvoření hello aplikační bránu, můžete přejděte tooit hello portálu a pokračovat v konfiguraci hello aplikační brány.

![Vytvoření aplikační brány](media/protect-netsec/adatum-app-gateway.png)

#### <a name="how-do-i-add-waf-tooan-existing-application"></a>Jak přidat firewall webových aplikací tooan stávající aplikaci?

tooupdate existující aplikaci brány toosupport firewall webových aplikací v režimu prevence hello následující:

1. V portálu Azure hello **Oblíbené** podokně klikněte na tlačítko **všechny prostředky**.

2. Klikněte na tlačítko hello existující aplikační brána v hello **všechny prostředky** okno. 
>[!NOTE]
Poznámka: Pokud hello odběr, který jste již vybrali neobsahuje několik prostředků, můžete zadat název hello v hello filtr podle názvu... pole tooeasily přístup hello DNS zóny.
3. Klikněte na tlačítko **brány firewall webových aplikací** a aktualizovat nastavení služby application gateway hello: **Upgrade tooWAF vrstvy** (zaškrtnuto), **brány Firewall stav** (povoleno),  **Režimu brány firewall** (prevence). Musíte taky tooconfigure hello sada pravidel a nakonfigurujte zakázaná pravidla.

Podrobné informace o tom, toocreate novou aplikační bránu s firewall webových aplikací a jak tooadd firewall webových aplikací tooan existující aplikační bránu, najdete v části [vytvoření služby application gateway pomocí brány firewall webových aplikací pomocí portálu hello.](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-portal)

### <a name="network-security-groups"></a>Network Security Groups (Skupiny zabezpečení sítě)

A [skupinu zabezpečení sítě](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) (NSG) obsahuje seznam pravidel zabezpečení, která povolují nebo odepírají tooresources provoz sítě připojené příliš[virtuálních sítí Azure](https://docs.microsoft.com/azure/virtual-network/) (VNet). Skupiny Nsg můžou být přidružené toosubnets nebo jednotlivé virtuální počítače. Pokud skupinu NSG přidružená tooa podsíť, hello pravidla použít tooall prostředky připojené toohello podsítě. Provoz se dá dál omezit tím, že také přidružíte NSG tooa virtuálního počítače nebo síťový adaptér.

Skupiny Nsg obsahují čtyři vlastnosti: název, oblast, skupinu prostředků a pravidla.
>[!Note]
Ačkoli skupina NSG existuje ve skupině prostředků, může být přidružené tooresources v libovolné skupině prostředků, tak dlouho, dokud prostředek hello je součástí hello stejné oblasti Azure jako hello NSG.

Pravidla NSG obsahují devět vlastnosti: název, protokol (TCP, UDP nebo \*, což zahrnuje ICMP a také protokolu UDP a TCP), zdroj rozsah portů, rozsah cílových portů, zdrojová adresa předpony, předpona cílové adresy směr (příchozí nebo odchozí), () s prioritou rozsahu od 100 do 4096) a přístup k typu (povolit nebo zakázat). Všechny skupiny Nsg obsahují sadu výchozích pravidel, která je možné odstranit, nebo přepsat hello pravidla, která vytvoříte.

#### <a name="how-do-i-implement-nsgs"></a>Jak implementovat skupiny Nsg?

Implementací skupin Nsg vyžaduje plánování a je nutné tootake v úvahu několik aspektů návrhu. Patří mezi ně omezení hello počet skupin Nsg na předplatné a pravidla na skupinu NSG; Virtuální síť a podsíť návrhu, zvláštní pravidla, provoz protokolu ICMP, izolace vrstev s podsítí, nástroje pro vyrovnávání zatížení a další.

Další pokyny plánování a implementace skupiny Nsg a vzorový scénář nasazení najdete v tématu [filtrování provozu sítě přenosů se skupinami zabezpečení sítě.](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg)

#### <a name="how-do-i-create-rules-in-an-nsg"></a>Vytvoření pravidla v skupinu NSG

toocreate příchozí pravidla v existující skupině, hello následující:

1. Klikněte na tlačítko **Procházet**a potom **skupin zabezpečení sítě**.

2. Hello seznamu skupin Nsg, klikněte na **NSG front-endu**a potom **příchozí pravidla zabezpečení.**

3. V seznamu hello příchozí pravidla zabezpečení, klikněte na tlačítko **přidat.**

4. Zadejte hodnoty hello v hello následující pole: název, Priority, zdroj, protokol, zdrojový rozsah, cíl, cílový rozsah portů a akce.

nové pravidlo Hello se zobrazí v hello NSG za několik sekund.

![pravidla zabezpečení sítě](media/protect-netsec/inbound-security.png)

Další pokyny, jak toocreate skupin Nsg v podsítích, vytvořte pravidla a přidružte skupinu NSG podsíť front-end a back-end, najdete v tématu [vytvoření skupin zabezpečení sítě pomocí hello portálu Azure.](https://docs.microsoft.com/azure/virtual-network/virtual-networks-create-nsg-arm-pportal)

## <a name="next-steps"></a>Další kroky

[Zabezpečení sítě Azure](https://azure.microsoft.com/blog/azure-network-security/)

[Osvědčené postupy zabezpečení sítě Azure](https://docs.microsoft.com/azure/security/azure-security-network-security-best-practices)

[Získání informací o skupinu zabezpečení sítě](https://docs.microsoft.com/rest/api/network/virtualnetwork/get-information-about-a-network-security-group)

[Brány firewall webových aplikací (firewall webových aplikací)](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview)
