---
title: aaaHow tooConfigure v1 App Service Environment
description: "Konfiguraci, správu a monitorování hello v1 App Service Environment"
services: app-service
documentationcenter: 
author: ccompy
manager: stefsch
editor: 
ms.assetid: b5a1da49-4cab-460d-b5d2-edd086ec32f4
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: ccompy
ms.openlocfilehash: f9539a72517276d8a1e340a408841561e8b8f56d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-an-app-service-environment-v1"></a>Konfigurování aplikace, služby prostředí v1

> [!NOTE]
> Tento článek je o hello App Service Environment v1.  Není k dispozici novější verze hello služby App Service Environment, je snazší toouse a běží na výkonnější infrastruktury. Další informace o nové verzi hello začínat hello toolearn [toohello Úvod App Service Environment](../app-service/app-service-environment/intro.md).
> 

## <a name="overview"></a>Přehled
Na vysoké úrovni služby Azure App Service Environment se skládá z několika hlavní součásti:

* Výpočetní prostředky, které jsou spuštěny v hello App Service Environment hostované služby
* Úložiště
* Databáze
* Classic(V1) nebo prostředků Azure Manager(V2) virtuální síť (VNet) 
* Podsíť s hello App Service Environment hostované služby spuštěné v ní

### <a name="compute-resources"></a>Výpočetní prostředky
Použijete hello výpočetní prostředky pro vaše fondy zdrojů čtyři.  Každé aplikaci služby prostředí (App Service Environment) obsahuje sadu front-end a tři fondy možné pracovních procesů. Nepotřebujete toouse všechny fondy pracovních procesů tři – Pokud chcete, můžete stačí použít jeden nebo dva kusy.

hostitelé Hello ve fondech zdrojů hello (front-end a pracovníci) nejsou přímo přístupné tootenants. Nelze použít protokol RDP (Remote Desktop) tooconnect toothem, změnit jejich zřizování nebo fungovat jako správce na ně.

Můžete nastavit prostředků fondu množství a velikosti. V App Service Environment máte čtyři možnosti velikosti, které jsou označeny P1 prostřednictvím P4. Podrobnosti o těchto velikosti a jejich cenách najdete v tématu [služby App Service – ceny](../app-service/app-service-value-prop-what-is.md).
Změna hello množství nebo velikost je volána operace škálování.  Operace pouze jeden škálování může být v průběhu najednou.

**Front-končí**: hello front-end jsou hello koncových bodů protokolu HTTP nebo HTTPS pro vaše aplikace, které jsou uložené v vaší App Service Environment. Úlohy nelze spustit v hello front-end.

* App Service Environment začíná dva P2s, což je dostačující pro procesy vývoje/testování a nízké úrovně produkčním prostředí. Důrazně doporučujeme P3s pro střední tooheavy úlohy v produkčním prostředí.
* Střední tooheavy úlohy v produkčním prostředí doporučujeme, abyste měli aspoň čtyři P3s tooensure nejsou dostatečná front-end spuštěn, když dojde k plánované údržby. Aktivity plánované údržby se otevře dolů jeden front-endu najednou. To snižuje celkové dostupné kapacity front-end během údržby aktivity.
* Front-end může trvat až hodinu tooprovision tooan. 
* Pro další doladění škálování, měli byste sledovat hello procento využití procesoru, procentuální využití paměti a metriky aktivních požadavků pro fond front-end hello. Pokud hello procenta využití procesoru nebo paměti jsou vyšší než 70 procent při spuštění P3s, přidejte další front-end. Pokud se too15, 000 too20 000 požadavků za front-endu zobrazí průměr hello aktivních požadavků hodnota, měli byste také přidat další front-end. Hello obecným cílem je tookeep procesoru a paměti hodnoty nižší než 70 % a průměrování out toobelow 15 000 požadavků za front-endu, když používáte P3s aktivních požadavků.  

**Pracovníci**: hello pracovníků je, kde vaše aplikace ve skutečnosti spuštěna. Při vertikální navýšení kapacity plány aplikační služby, že používá si zaměstnanci z hello přidružené fondu pracovních procesů.

* Nelze přidat okamžitě pracovních procesů. Se může trvat až hodinu tooprovision tooan.
* Škálování hello velikost výpočetních prostředků pro žádného fondu bude trvat < 1 hodina za aktualizaci domény. V App Service Environment nejsou 20 aktualizaci domény. Při změně měřítka hello výpočetní velikost fondu pracovních procesů s 10 instancí může trvat až toocomplete too10 hodin.
* Pokud změníte velikost hello hello výpočetní prostředky, které se používají ve fondu pracovních procesů, způsobí cold spustí hello aplikací, které jsou spuštěné v tomto fondu pracovních procesů.

nejrychlejší způsob Hello toochange hello výpočetní prostředek velikost fondu pracovních procesů, které neběží žádné aplikace je:

* Snižovat hello objemu too2 pracovních procesů.  Hello minimální vertikálně velikost portálu hello je 2. Bude trvat několik minut toodeallocate vaše instance. 
* Vyberte novou velikost výpočetních hello a počet instancí. Tady to trvat až toocomplete too2 hodin.

Pokud vaše aplikace vyžadují větší velikost výpočetních prostředků, nelze využít výhod hello předchozí pokyny. Místo změna hello velikosti fondu hello pracovního procesu, který je hostitelem tyto aplikace, můžete naplnit jiný pracovní fond s pracovníci hello potřeby velikosti a přesuňte toothat fondu aplikací.

* Vytvořte další instance hello hello potřeby výpočetní velikost v jiném fondu pracovních procesů. Bude to trvat až hodinu toocomplete tooan.
* Přiřadit váš plán služby App Service, které jsou hostiteli hello aplikace, které potřebují větší velikost fondu pracovních procesů toohello nově nakonfigurovaný. Jedná se o rychlé operaci, který by měl mít méně než minutu toocomplete.  
* Pokud již nejsou potřebné tyto nepoužívané instance snižovat hello první fondu pracovních procesů. Tato operace trvá několik minut toocomplete.

**Automatické škálování**: jeden hello nástrojů, které vám mohou pomoci toomanage vaší spotřeby prostředků výpočetního provádí automatické škálování. Můžete použít automatické škálování pro front-end nebo fondy pracovních procesů. Můžete provádění akcí, například zvýšení vaší instance libovolného typu fondu v hello ráno a snížit v večer hello. Nebo možná přidat instance při hello počet pracovních procesů, které jsou dostupné ve fondu pracovních procesů klesne pod určitou mez.

Pokud chcete pravidla škálování tooset kolem metrika fondu výpočetních prostředků, mějte na paměti hello čas této zřizování vyžaduje. Další podrobnosti o automatické škálování prostředí App Service najdete v tématu [jak tooconfigure škálování ve službě App Service Environment][ASEAutoscale].

### <a name="storage"></a>Úložiště
Každý App Service Environment je nakonfigurovaný s 500 GB úložiště. Tento prostor se používá napříč všechny aplikace hello hello App Service Environment. Tento prostor úložiště je součástí hello App Service Environment a momentálně nemůže být vypnuté toouse prostor úložiště. Pokud provádíte úpravy tooyour virtuální sítě směrování nebo zabezpečení, je třeba toostill povolit přístup tooAzure úložiště--nebo nemůže funkce hello App Service Environment.

### <a name="database"></a>Databáze
Hello databáze obsahuje hello informace, které definují hello prostředí, stejně jako hello podrobnosti o hello aplikace, které běží v něm. Toto je příliš součástí hello předplatné Azure uchovávat. Není něco, co uživatel má přímé možnost toomanipulate. Pokud provádíte úpravy tooyour virtuální sítě směrování nebo zabezpečení, je třeba toostill povolit přístup tooSQL Azure – nebo nemůže funkce hello App Service Environment.

### <a name="network"></a>Síť
Hello virtuální síť, která se používá s vaší App Service Environment může být ten, který jste při vytváření hello App Service Environment nebo ten, který jste předem. Když vytvoříte podsítě hello během vytváření App Service Environment, vynutí si hello App Service Environment toobe v hello stejné skupině prostředků jako virtuální síť hello. Pokud potřebujete skupinu prostředků hello používá vaše App Service Environment toobe liší od vaší virtuální sítě, budete potřebovat toocreate vaše App Service Environment pomocí šablony správce prostředků.

Existují některá omezení na hello virtuální síť, která se používá pro App Service Environment:

* Hello virtuální síť musí mít regionální virtuální síť.
* Je potřeba toobe podsíť s 8 nebo více adres, které se nasadí hello App Service Environment.
* Po podsíť je použité toohost App Service Environment, nebude možné změnit hello rozsah adres podsítě hello. Z tohoto důvodu doporučujeme, abyste že této podsíti hello obsahuje minimálně 64 adresy tooaccommodate případnému budoucímu růstu App Service Environment.
* Může být nic jiného v podsíti hello ale hello App Service Environment.

Na rozdíl od hello hostovaná služba, která obsahuje hello App Service Environment, hello [virtuální sítě] [ virtualnetwork] a podsíť je za uživatelský ovládací prvek.  Můžete spravovat virtuální sítě prostřednictvím hello virtuální sítě uživatelského rozhraní nebo Powershellu.  App Service Environment se dá nasadit v klasický nebo virtuální sítě Resource Manager.  Hello portál a rozhraní API prostředí se mírně liší Classic a Resource Manager virtuálních sítí, ale hello prostředí App Service Environment je hello stejné.

Hello virtuální síť, která je použité toohost App Service Environment můžete použít buď soukromé RFC1918 IP adresy nebo ji můžete použít veřejné IP adresy.  Pokud chcete toouse rozsah adres IP, která není předmětem RFC1918 (10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16), nutné toocreate vaše virtuální síť a podsíť toobe používané vaší App Service Environment před vytvoření App Service Environment.

Protože tato možnost umístí do virtuální sítě hello Azure App Service, znamená to, že vaše aplikace, které jsou hostované v vaší App Service Environment nyní přístup k prostředkům, které jsou k dispozici prostřednictvím ExpressRoute nebo virtuální privátní sítě site-to-site (VPN) přímo. Hello aplikace, které jsou v rámci služby App Service Environment nevyžadují další síťové funkce tooaccess prostředky k dispozici toohello virtuální síť, která je hostitelem služby App Service Environment. To znamená, že nepotřebujete toouse integrace virtuální sítě nebo hybridní připojení tooresources tooget v nebo tooyour připojené virtuální sítě. Přesto můžete obou těchto funkcí když tooaccess prostředky v sítích, které nejsou připojené tooyour virtuální sítě.

Například můžete toointegrate integrace virtuální sítě pomocí virtuální sítě, která je v rámci vašeho předplatného, ale není připojený toohello virtuální síť, kterou vaše App Service Environment je v. Stále také můžete prostředky tooaccess hybridní připojení, které jsou v jiných sítích, podobně jako obvykle.  

Pokud máte virtuální síť nakonfigurované připojení VPN pomocí ExpressRoute byste měli vědět některé hello směrování potřeb, které má App Service Environment. Existují některé konfigurace trasy definované uživatelem (UDR), které jsou kompatibilní s App Service Environment. Další podrobnosti o spuštění App Service Environment ve virtuální síti s ExpressRoute najdete v tématu [spuštění služby App Service Environment ve virtuální síti s ExpressRoute][ExpressRoute].

#### <a name="securing-inbound-traffic"></a>Zabezpečení příchozí provoz
Existují dvě základní metody toocontrol příchozí provoz tooyour App Service Environment.  Toocontrol Groups(NSGs) zabezpečení sítě můžete použít jaké IP adresy můžete přístup k vaší App Service Environment podle postupu popsaného tady [jak toocontrol příchozí provoz ve službě App Service Environment](app-service-app-service-environment-control-inbound-traffic.md) a můžete také nakonfigurovat vaše App Service Environment se interní zatížení Balancer(ILB).  Tyto funkce lze také použít společně toorestrict pro přístup pomocí skupin Nsg tooyour ILB App Service Environment.

Při vytváření App Service Environment, vytvoří virtuální IP adresu ve vaší virtuální síti.  Existují dva typy virtuálních IP adres, externí i interní.  Při vytváření App Service Environment pomocí externí VIP vašich aplikací ve vaší App Service Environment budou přístupné přes internet směrovatelné IP adresu. Když vyberete interní vaší App Service Environment bude nakonfigurován s ILB a nebude se přímo přístup z Internetu.  App Service Environment ILB stále vyžaduje externí VIP, ale používá se pouze pro přístup k Azure správy a údržby.  

Během vytváření App Service Environment ILB zadejte hello subdomény používané hello ILB App Service Environment a bude mít toomanage vlastní DNS hello subdomény, které určíte.  Vzhledem k tomu, že nastavíte název subdomény hello je také nutné toomanage hello certifikát používaný pro přístup k protokolu HTTPS.  Po App Service Environment vytváření, který se zobrazí výzva tooprovide hello certifikátu.  Přečtěte si další informace o vytváření a používání App Service Environment ILB toolearn [interní pro vyrovnávání zatížení pomocí služby App Service Environment][ILBASE]. 

## <a name="portal"></a>Portál
Můžete spravovat a monitorovat služby App Service Environment pomocí hello uživatelského rozhraní v hello portálu Azure. Pokud máte App Service Environment, pak jste pravděpodobně toosee hello symbol služby App Service na vaše bočním panelu. Tento symbol je použité toorepresent prostředí App Service v hello portálu Azure:

![Symbol App Service Environment][1]

tooopen hello uživatelské rozhraní, které jsou uvedeny všechny vaše prostředí App Service, můžete použít ikonu hello nebo vyberte hello dvojitou (">" symbol) dole hello hello bočním panelu tooselect prostředí App Service. Pokud vyberete jednu z uvedených ASEs hello, otevřete hello uživatelské rozhraní, které je použité toomonitor a k její správě.

![Uživatelské rozhraní pro monitorování a Správa služby App Service Environment][2]

Toto první okno obsahuje některé vlastnosti týkající se vaší App Service Environment, společně s metriky grafu na fond zdrojů. Některé hello vlastnosti, které jsou zobrazeny v hello **Essentials** bloku jsou i hypertextové odkazy, které se otevře okno hello, který je k ní přidružena. Například můžete vybrat hello **virtuální sítě** název tooopen až hello uživatelského rozhraní přidružené hello virtuální síť, kterou vaše App Service Environment běží v. **Plánů služby App Service** a **aplikace** každý otevřete okna, které obsahují tyto položky, které jsou ve vaší App Service Environment.  

### <a name="monitoring"></a>Monitorování
Hello grafy umožňují toosee různých metrik výkonu v každém fondu zdrojů. Hello front-end fondu můžete monitorovat hello průměrné využití procesoru a paměti. Pro fondy pracovních procesů můžete monitorovat hello množství, které se používá a hello množství, které je k dispozici.

Použití více App Service, které můžete provést plány hello pracovní procesy ve fondu pracovních procesů. v hello stejné dotazování stejně jako u servery front-end hello, takže využití procesoru a paměti hello neposkytují hodně způsobem hello užitečných informací, není distribuován Hello zatížení. Je důležité tootrack kolik pracovních procesů, které jste už použili a jsou k dispozici – zejména v případě, že tento systém spravujete pro ostatní toouse.  

Můžete také použít všechny hello metriky, které lze sledovat v hello grafy tooset si výstrahy. Nastavení výstrah zde, že funguje stejně jako jinde hello ve službě App Service. Výstrahu můžete nastavit od buď hello **výstrahy** uživatelského rozhraní část nebo z podrobnostem všechny metriky uživatelského rozhraní a výběrem **přidat výstraha**.

![Metriky uživatelského rozhraní][3]

Hello metriky, které byly právě popsané jsou hello App Service Environment metriky. Existují také metriky, které jsou k dispozici na hello úroveň plánu služby App Service. Toto je, kde sledování procesoru a paměti umožňuje spoustu smysl.

V App Service Environment všechny hello plánů služby App Service jsou vyhrazené plány služby App Service. To znamená, že hello jenom aplikace, které jsou spuštěny na hostitelích přidělené toothat hello plán služby App Service jsou hello aplikace v tomto plánu služby App Service. Podrobnosti o toosee na plán služby App Service zprovoznit plán služby App Service ze všech hello seznamů v hello App Service Environment uživatelského rozhraní nebo z **plánů Procházet služby App Service** (které jsou uvedeny všechny z nich).   

### <a name="settings"></a>Nastavení
V okně hello App Service Environment není **nastavení** oddíl, který obsahuje několik důležitých funkcí:

**Nastavení** > **vlastnosti**: hello **nastavení** automaticky otevře se okno při nasazování okně vaší App Service Environment. V hello je horní **vlastnosti**. Existuje několik položek sem, které jsou redundantní toowhat můžete vidět v **Essentials**, ale co je velmi užitečná je **virtuální IP adresy**, a také **odchozí IP adresy**.

![Okno nastavení a vlastností][4]

**Nastavení** > **IP adresy**: Když vytvoříte aplikaci IP Secure Sockets Layer (SSL) ve vašem App Service Environment, potřebujete adresu IP SSL. V pořadí tooobtain jeden musí vaše App Service Environment adres IP protokolu SSL, které vlastní, které mohou být přiděleny. Když je vytvořen App Service Environment, má jednu IP SSL adresu pro tento účel, ale můžete přidat další. Není zpoplatněny pro další SSL IP adresy, jak je znázorněno v [služby App Service – ceny] [ AppServicePricing] (v oddílu hello na připojení SSL). Další cena Hello je hello cena IP SSL.

**Nastavení** > **Front End fondu** / **fondy pracovních procesů**: každý z těchto oknech fondu prostředků nabízí hello možnost toosee informace pouze na tento fond zdrojů , kromě tooproviding prvky toofully škálování tento fond zdrojů.  

Hello základní okno pro každý fond zdrojů obsahuje graf s metriky pro tento fond zdrojů. Stejně jako s hello grafy v okně App Service Environment hello, můžete přejít do grafu hello a nastavit výstrahy podle potřeby. Nastavení v okně hello App Service Environment pro fond zdrojů pro konkrétní výstrahu hello samé jako provádění z fondu prostředků hello. Z fondu pracovních procesů hello **nastavení** okno, máte přístup tooall hello aplikace nebo plánů služby App Service, používáte v tomto fondu pracovního procesu.

![Nastavení fondu pracovní uživatelského rozhraní][5]

### <a name="portal-scale-capabilities"></a>Funkce portálu škálování
Existují tři operace škálování:

* Změna hello počet IP adres v hello App Service Environment, které jsou k dispozici pro použití protokolu SSL IP.
* Změna velikosti hello hello výpočtový prostředek, který se používá ve fondu zdrojů.
* Změna hello počet výpočetní prostředky, které se používají ve fondu zdrojů, buď ručně, nebo prostřednictvím automatické škálování.

Hello portálu existují tři způsoby toocontrol počet serverů, které máte ve fondech zdrojů:

* Operace škálování v hlavním okně App Service Environment hello v horní části hello. Můžete provést několik škálování změny konfigurace toohello front-endu a fondy pracovních procesů. Všechny jsou použity jako jediná operace.
* Operace ruční škálování z fondu prostředků jednotlivých hello **škálování** okno, které je pod **nastavení**.
* Automatické škálování, který jste nastavili z fondu prostředků jednotlivých hello **škálování** okno.

operace škálování hello toouse v okně App Service Environment hello, přetáhněte hello posuvníku toohello množství a uložte. Toto uživatelské rozhraní podporuje také změna velikosti hello.  

![Škálování uživatelského rozhraní][6]

Funkce ruční nebo automatické škálování hello toouse ve fondu konkrétní prostředek, přejděte příliš**nastavení** > **Front End fondu** / **fondy pracovních procesů** jako vhodné. Potom otevře hello fondu, které chcete toochange. Přejděte příliš**nastavení** > **horizontální navýšení kapacity** nebo **nastavení** > **vertikálně navýšit kapacitu**. Hello **horizontální navýšení kapacity** okno vám umožní množství toocontrol instance. **Vertikálně navýšit kapacitu** vám umožní velikost toocontrol prostředků.  

![Nastavení škálování uživatelského rozhraní][7]

## <a name="fault-tolerance-considerations"></a>Důležité informace o odolnosti proti chybám
Můžete nakonfigurovat App Service Environment toouse až too55 celkový výpočetní prostředky. Tyto 55 výpočetní prostředky může být jen 50 použité toohost úlohy. Důvod Hello má dva účely. Není k dispozici minimálně 2 front-end výpočetní prostředky.  Která zůstane až too53 toosupport hello fondu pracovních procesů přidělení. Pořadí tooprovide odolnost proti chybám je třeba toohave na další výpočetní prostředek, který je přidělen podle toohello následující pravidla:

* Každý fond pracovních procesů musí alespoň 1 Další výpočtový prostředek, který není k dispozici toobe přiřazené zatížení.
* Když hello objemu výpočetní prostředky ve fondu pracovních procesů překročí určitou hodnotu, jiné výpočetních prostředků je vyžadována pro odolnost proti chybám. Toto není hello front-end fondu v takovém případě hello.

V rámci žádného fondu jednoho pracovního hello odolnost proti chybám požadavky jsou pro dané hodnoty X přiřazené prostředky tooa fondu pracovních procesů:

* Pokud X je mezi 2 a 20, hello množství použitelné výpočetní prostředky, které můžete použít pro úlohy je X-1.
* Pokud X je 21 až 40, hello množství použitelné výpočetní prostředky, které můžete použít pro úlohy je X-2.
* Pokud X je mezi 41 a 53, hello množství použitelné výpočetní prostředky, které můžete použít pro úlohy je X-3.

Hello nároků má 2 front-end servery a 2 pracovní procesy.  S hello výše pak příkazy zde je několik příkladů tooclarify:  

* Pokud máte 30 pracovních procesů v jednom fondu, 28 z nich může být použité toohost úlohy.
* Pokud máte v jednom fondu 2 pracovní procesy, může být 1 použité toohost úlohy.
* Pokud máte 20 pracovních procesů v jednom fondu, může být 19 použité toohost úlohy.  
* Pokud máte 21 pracovních procesů v jednom fondu, stále pouze 19 mohou být použité toohost úlohy.  

aspekt odolnost proti chybám Hello je důležité, ale je nutné ji nezapomeňte jako můžete škálovat prahovou hodnotu pro určité tookeep. Pokud chcete tooadd víc kapacity přecházející z 20 instance a pak přejděte too22 nebo vyšší protože 21 nepřidává žádné další kapacitu. Dobrý den, které platí to budete výše 40, kde je hello další číslo, které přidá kapacity 42.  

## <a name="deleting-an-app-service-environment"></a>Odstranění služby App Service Environment
Pokud chcete, aby toodelete služby App Service Environment, jednoduše použijte hello **odstranit** akce hello horní části okna služby App Service Environment hello. Když to uděláte, budete výzvami tooenter hello název vaší služby App Service Environment tooconfirm Opravdu chcete toodo to. Všimněte si, že při odstranění služby App Service Environment, můžete odstranit všechny hello obsahu v něm také.  

![Odstranění služby App Service Environment uživatelského rozhraní][9]  

## <a name="getting-started"></a>Začínáme
tooget začít s prostředí App Service najdete v části [jak toocreate služby App Service Environment](app-service-web-how-to-create-an-app-service-environment.md).

Další informace o platformě Azure App Service hello najdete v tématu [Azure App Service](../app-service/app-service-value-prop-what-is.md).

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!--Image references-->
[1]: ./media/app-service-web-configure-an-app-service-environment/ase-icon.png
[2]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-aseblade.png
[3]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-poolchart.png
[4]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-properties.png
[5]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-poolblade.png
[6]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-scalecommand.png
[7]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-poolscale.png
[8]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-pricingtiers.png
[9]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-deletease.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[Appserviceplans]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[HowtoCreateASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[HowtoScale]: http://azure.microsoft.com/documentation/articles/app-service-web-scale-a-web-app-in-an-app-service-environment/
[ControlInbound]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[ASEAutoscale]: http://azure.microsoft.com/documentation/articles/app-service-environment-auto-scale/
[ExpressRoute]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-configuration-expressroute/
[ILBASE]: http://azure.microsoft.com/documentation/articles/app-service-environment-with-internal-load-balancer/
