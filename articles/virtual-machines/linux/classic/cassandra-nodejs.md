---
title: aaaRun Cassandra s Linuxem v Azure | Microsoft Docs
description: "Jak toorun Cassandra clusteru v systému Linux v Azure Virtual Machines z aplikace Node.js"
services: virtual-machines-linux
documentationcenter: nodejs
author: tomarcher
manager: routlaw
editor: 
tags: azure-service-management
ms.assetid: 30de1f29-e97d-492f-ae34-41ec83488de0
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: tarcher
ms.openlocfilehash: 381ca301bbe88d3740cf182f9c44fada5b9ba7cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="running-cassandra-with-linux-on-azure-and-accessing-it-from-nodejs"></a>Spuštění Cassandry s Linuxem v Azure a přístup pomocí Node.js
> [!IMPORTANT] 
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Classic hello. Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello. Zobrazit šablony Resource Manageru pro [Datastax Enterprise](https://azure.microsoft.com/documentation/templates/datastax) a [Spark clusteru a Cassandra na CentOS](https://azure.microsoft.com/documentation/templates/spark-and-cassandra-on-centos/).

## <a name="overview"></a>Přehled
Microsoft Azure je platforma otevřete cloudu, která spouští obou Microsoft jako i v jiných společností než Microsoft software, který zahrnuje operační systémy, aplikační servery, zasílání zpráv middleware, jakož i databází SQL a NoSQL z obou modelů komerční s otevřeným zdrojem. Vytváření odolné služeb na veřejných cloudů, včetně Azure vyžaduje pečlivé plánování a architektura záměrné pro oba servery aplikace jako dobře úložiště vrstvy. Architektura distribuovaného úložiště pro Cassandra přirozeně pomáhá při vytváření vysoce dostupných systémy, které jsou odolné proti chybám pro selhání clusteru. Cassandra je cloudové škálování databáze NoSQL udržovat Apache Software Foundation cassandra.apache.org; Cassandra je napsán v jazyce Java a proto běží na obou v systému Windows a Linux platformy.

Hello cílem tohoto článku je tooshow Cassandra nasazení na Ubuntu jako cluster s podporou jeden a více data center využívat virtuální počítače Microsoft Azure a virtuální sítě. Hello nasazení clusteru pro úlohy v produkčním prostředí optimalizované je mimo rámec tohoto článku jak to vyžaduje konfigurace uzlu více disku, potřeba odpovídající kruhová topologie návrh a modelování toosupport hello dat replikace, konzistenci dat, propustnost a požadavky na vysokou dostupnost.

Tento článek trvá základní přístup tooshow, co je součástí hello vytváření clusteru Cassandra porovnání Docker, Chef nebo Puppet, které můžete provést hello nasazení infrastruktury mnoho jednodušší.  

## <a name="hello-deployment-models"></a>Hello modely nasazení
Microsoft Azure sítě umožňuje nasazení hello izolované privátní clusterů hello přístupu, které může být zabezpečení s omezeným přístupem tooattain jemné možnosti sítě.  Vzhledem k tomu, že tento článek se týká zobrazující hello Cassandra nasazení na základní úrovni, zaměříme nebude na úroveň konzistence hello a návrh optimální úložiště hello propustnost. Tady je seznam hello síťové požadavky pro naše hypotetický clusteru:

* Externí systémy nelze získat přístup k Cassandra databáze z uvnitř nebo vně Azure
* Cassandra cluster má toobe za službou Vyrovnávání zatížení pro provoz thrift
* Nasazení Cassandra uzly ve dvou skupinách v každé datové centrum pro rozšířené clusteru dostupnosti
* Zamknout hello clusteru, který pouze serverové farmy aplikace má databáze access toohello přímo
* Žádné veřejné sítě koncové body než SSH
* Každý uzel Cassandra musí pevnou interní IP adresu

Cassandra můžou být nasazené tooa jedné oblasti Azure nebo toomultiple oblasti závislosti na povaze distribuovaných hello hello zatížení. Model nasazení s více oblasti lze využít tooserve koncoví uživatelé blíže tooa konkrétní geografický prostřednictvím hello stejnou Cassandra infrastrukturu. Cassandra pro předdefinovaný uzel replikace trvá pozor hello synchronizace více hlavního serveru zapíše pocházející z několika datových centrech a uvede konzistentní zobrazení dat tooapplications hello. Nasazení s více oblast může také pomoci s zmírnění rizik hello hello širší výpadků služby Azure. Přizpůsobitelné konzistence na Cassandra a replikační topologie usnadní pokrývat různých plánovaný bod obnovení z aplikace.

### <a name="single-region-deployment"></a>Nasazení jedné oblasti
Jsme se spustí s nasazení jedné oblasti a sklizně hello learnings při vytváření více oblast modelu. Virtuální síť Azure bude podsítě používané toocreate izolované tak, aby výše uvedené požadavky na zabezpečení hello sítě lze splnit.  Hello proces popsaný při vytváření nasazení jedné oblasti hello používá Ubuntu 14.04 LTS a Cassandra 2.08; ale hello procesu můžete snadno být přijatých toohello ostatní varianty Linux. Hello Toto jsou některé systémové charakteristiky hello hello nasazení jedné oblasti.  

**Vysoká dostupnost:** hello Cassandra uzly ze hello obrázek 1 nasazených tootwo dostupnosti nastaví tak, aby uzly hello se šíří mezi více domén selhání pro zajištění vysoké dostupnosti. Poznámky s každou skupinu dostupnosti virtuálních počítačů je namapované too2 domén selhání.  Microsoft Azure používá hello koncept toomanage domény selhání neplánované výpadek (např. selhání hardwaru nebo softwaru) při hello konceptu upgradu domény (např. hostitele nebo hosta OS opravy a upgrady, upgrady aplikací) se používá pro správu naplánované výpadek. Najdete v tématu [zotavení po havárii a vysoká dostupnost pro aplikací Azure](http://msdn.microsoft.com/library/dn251004.aspx) pro roli hello selhání a upgradu domén v dosažení vysoké dostupnosti.

![Nasazení jedné oblasti](./media/cassandra-nodejs/cassandra-linux1.png)

Obrázek 1: Nasazení jedné oblasti

Všimněte si, že v době psaní tohoto textu hello Azure neumožňuje hello explicitní mapování skupiny virtuálních počítačů domény konkrétní selhání tooa; v důsledku toho i s modelem nasazení hello na obrázku 1, je statisticky pravděpodobné, že všechny virtuální počítače hello může být namapované tootwo domén selhání místo čtyři.

**Přenosy Thrift Vyrovnávání zatížení:** Thrift klientské knihovny uvnitř hello webový server připojení clusteru toohello přes interní nástroj. To vyžaduje hello proces přidávání podsíť hello interní služby load vyrovnávání toohello "data" (viz obrázek 1) v kontextu hello hello cloudové služby hostování hello Cassandra clusteru. Po definování Vyrovnávání zatížení interní hello každý uzel vyžaduje hello skupinu s vyrovnáváním zatížení koncového bodu toobe přidána s hello poznámky skupiny vyrovnáváním zatížení s dříve název nástroje pro vyrovnávání zatížení definované. V tématu [interní Vyrovnávání zatížení Azure ](../../../load-balancer/load-balancer-internal-overview.md)další podrobnosti.

**Cluster semen:** je důležité tooselect hello většina vysoce dostupných uzlů pro semena jako hello nové uzly budou komunikovat s uzly počáteční hodnoty toodiscover hello topologii hello clusteru. Jeden uzel z každé skupiny dostupnosti je určený jako počáteční hodnoty uzly tooavoid jediný bod selhání.

**Zařízení a úroveň konzistence replikace:** Cassandra na sestavení v vysokou dostupnost a data odolnost je charakterizovaná hello replikace faktor RF - počet kopií každého řádku uložené v clusteru hello a úroveň konzistence (počet repliky toobe číst nebo zapisovat před vrácením hello výsledek toohello volající). Faktor replikace je zadán během hello vytvoření KEYSPACE (podobně jako relační databáze tooa), zatímco úroveň konzistence hello je zadána při vydání dotazů CRUD hello. Naleznete v dokumentaci k Cassandra v [konfigurace pro konzistence](http://www.datastax.com/documentation/cassandra/2.0/cassandra/dml/dml_config_consistency_c.html) konzistence podrobnosti a hello vzorec pro výpočty kvora.

Cassandra podporuje dva typy modelů integrity dat – konzistence a konzistence typu případné; Hello Multi-Factor replikace a úroveň konzistence společně určí Pokud hello data budou konzistentní při dokončení operace zápisu nebo bude nakonec byl konzistentní. Například zadání KVORA jako hello úroveň konzistence bude vždy zajišťuje data konzistence při jakékoli úroveň konzistence, pod hello počet replik toobe zapisují jako potřebné tooattain výsledkem KVORA (například jeden) a data se nakonec byl konzistentní.

Hello 8 uzlů clusteru s faktorem replikace 3 a KVORA zobrazeno výše, (2 uzly jsou číst nebo zapisovat konzistenci) pro čtení a zápis úroveň konzistence, přežijí hello teoretické ztrátě v hello většina 1 uzel za replikační skupiny před spuštěním aplikace hello vašeho povšimnutí hello selhání. Předpokladem je, všechny klíče mezery hello mít vyrovnáváním i požadavky na čtení a zápis.  Hello následují hello parametry, které budeme používat pro cluster hello nasazení:

Konfigurace clusteru Cassandra jedné oblasti:

| Parametr clusteru | Hodnota | Poznámky |
| --- | --- | --- |
| Počet uzlů (ne) |8 |Celkový počet uzlů v clusteru hello |
| Replikace faktor (RF) |3 |Počet replik daného řádku |
| Úroveň konzistence (zápisu) |QUORUM[(RF/2) +1) = 2] hello výsledek z hello vzorec se zaokrouhlí směrem dolů |Zapíše na hello většina 2 repliky před odesláním odpovědi hello toohello volající; 3. replika se zapíše nakonec byl konzistentní způsobem. |
| Úroveň konzistence (čtení) |KVORA [(RF/2) + 1 = 2] hello výsledek vzorec hello se zaokrouhlí směrem dolů |Přečte 2 repliky před odesláním odpovědi toohello volajícího. |
| Strategie replikace |Viz NetworkTopologyStrategy [replikaci dat](http://www.datastax.com/documentation/cassandra/2.0/cassandra/architecture/architectureDataDistributeReplication_c.html) v dokumentaci k Cassandra pro další informace |Jste srozuměni s tím topologie nasazení hello a umístí repliky na uzlech, aby všechny repliky hello nemáte skončili na hello stejné rack |
| Snitch |Viz GossipingPropertyFileSnitch [Snitches](http://www.datastax.com/documentation/cassandra/2.0/cassandra/architecture/architectureSnitchesAbout_c.html) v dokumentaci k Cassandra pro další informace |NetworkTopologyStrategy používá koncept snitch toounderstand hello topologie. GossipingPropertyFileSnitch poskytuje lepší kontrolu mapování každý uzel toodata center a racku. povídání toopropagate Hello clusteru pak použije tyto informace. To je mnohem jednodušší dynamické IP nastavení relativní tooPropertyFileSnitch |

**Azure aspekty pro Cassandra Cluster:** funkce Microsoft Azure Virtual Machines používá úložiště objektů Blob v Azure disku trvalosti; Úložiště Azure uloží 3 repliky každého disku pro vysokou odolnost. To znamená, že každý řádek dat vloženy do tabulky Cassandra je již uložen v 3 repliky, a proto konzistenci dat je již postaráno i v případě hello replikace faktor (RF) je 1. Hello hlavní problém s Multi-Factor replikace se 1 je aplikace hello všimnete výpadek i v případě selhání jednoho uzlu Cassandra. Ale pokud uzel je vypnutý hello problémy (např. hardwaru, softwaru selhání systému) rozpoznáno Kontroleru prostředků infrastruktury Azure, zřídí nový uzel v jeho místní pomocí hello stejné jednotky úložiště. Zřizování nový uzel tooreplace hello staré jeden může trvat několik minut.  Pro plánované údržby činnosti, jako například změny hostovaný operační systém, podobně jako Cassandra upgraduje a změny aplikace Kontroleru prostředků infrastruktury Azure provádí vrácení upgrady hello uzlů v clusteru hello.  Vrácení upgradu také může trvat dolů několika uzlů najednou a proto hello clusteru setkat s krátkou prodlevou mezi migrací pro několik oddílů. Ale hello nedojde ke ztrátě kvůli toohello vestavěná redundance úložiště Azure.  

Pro systémy nasazené tooAzure, která nevyžaduje vysokou dostupnost (například přibližně 99,9 který je ekvivalentní too8.76 hodin nebo rok; viz [vysokou dostupnost](http://en.wikipedia.org/wiki/High_availability) podrobnosti) může být schopný toorun s RF = 1 a úroveň konzistence = jeden.  Pro aplikace s požadavky na vysokou dostupnost, RF = 3 a úroveň konzistence = KVORA budou tolerovat hello výpadek jednoho z uzlů hello jeden hello replik. RF = 1 v tradiční nasazení (například místní) nelze použít kvůli ztrátě dat. toohello vyplývající z problémů, jako je selhání disku.   

## <a name="multi-region-deployment"></a>Nasazení s více oblast
Datové centrum s deklaracemi replikace a konzistence model popsané výše pomáhá s nasazení s více oblast hello předinstalované hello bez hello na Cassandra nutnost žádné externí nástroje. Toto je výrazně lišit od tradičních relačních databází hello, kde může být poměrně složité hello nastavení pro zrcadlení databáze pro více hlavních zápisy. Cassandra v několika oblasti nastavit můžou pomoct s scénáře použití hello včetně hello následující:

**Bezkontaktní komunikace na základě nasazení:** víceklientským aplikacím s zrušte mapování uživatelů klienta-na-oblast, můžete prospěch podle clusteru více oblast hello nízkou latenci. Například správy učení systémy pro vzdělávací instituce můžete nasadit cluster distribuované ve východní USA a západní USA oblasti tooserve hello příslušných univerzity pro transakční i analytics. Hello data můžou být místně konzistentní v hello čas čtení a zápisu a může být v obou oblastech hello nakonec byl konzistentní. Existují další příklady jako distribuční média, elektronické obchodování a nic a všechno, co se uživatel geograficky soustředí slouží základní je případ vhodné využít pro tento model nasazení.

**Vysoká dostupnost:** redundance je klíčovým faktorem k dosažení vysoké dostupnosti softwaru a hardwaru; podrobnosti najdete v části vytváření spolehlivé cloudové systémy v Microsoft Azure. V Microsoft Azure je nasazení clusteru s podporou více oblast hello pouze spolehlivý způsob dosažení true redundance. Mohou být aplikace nasazeny v režimu aktivní aktivní nebo aktivní – pasivní, a pokud jeden z oblasti hello je vypnutý, Azure Traffic Manageru můžete přesměrovat provoz toohello aktivní oblasti.  Při nasazení jedné oblasti hello, pokud je dostupnost hello 99,9, oblast dvě nasazení můžete zajistit s dostupností 99.9999 vypočítávají podle vzorce hello: (1-(1-0.999) * (1-0,999)) * 100); v tématu hello výše dokumentu podrobnosti.

**Zotavení po havárii:** Cassandra více oblast clusteru, pokud správně navržená tak, odolat závažné data center výpadků. Pokud jedné oblasti je vypnutý, můžete začít hello aplikace nasazena tooother oblasti obsluhující hello koncovým uživatelům. Stejně jako jakékoli jiné firmy kontinuity implementace má aplikace hello toobe odolný vůči chybám pro ztrátu dat, která je výsledkem dat hello v kanálu asynchronní hello. Ale Cassandra umožňuje obnovení hello mnohem rychlejší, než doba hello procesy tradiční databáze obnovení. Obrázek 2 ukazuje hello model typické nasazení s více oblasti s osmi uzlů v každé oblasti. Obě oblasti jsou Image zrcadlení vzájemně pro hello stejné symetrie; návrhy skutečných závisí na typu úloh hello (např. transakční či analytical), plánovaný bod obnovení, RTO, konzistenci dat a požadavky dostupnosti.

![Nasazení více oblast](./media/cassandra-nodejs/cassandra-linux2.png)

Obrázek 2: Nasazení s více oblast Cassandra

### <a name="network-integration"></a>Integrace sítě
Nastaví virtuálních počítačů nasazených tooprivate sítě nachází na dvou oblastí komunikuje se navzájem pomocí tunelového připojení sítě VPN. tunelové propojení VPN Hello připojí dvě brány softwaru zřízenou během procesu nasazení sítě hello. Obě oblasti mají podobné síťovou architekturu z hlediska podsítě "web" a "data"; Azure sítě umožňuje vytváření hello tolik podsítí podle potřeby a použít seznamy řízení přístupu podle potřeb zabezpečení sítě. Při navrhování topologie clusteru hello mimo data center komunikace latenci a hello hospodářského dopad hello síťový provoz nutné toobe považována za.

### <a name="data-consistency-for-multi-data-center-deployment"></a>Konzistence dat pro nasazení s více datového centra
Distribuované nasazení nutné toobe vědět hello clusteru topologie dopad na propustnost a vysokou dostupnost. Hello RF a úroveň konzistence toobe nutné vybrané takovým způsobem, který hello kvora nezávisí na dostupnost hello všechny hello datových centrech.
Pro systém, který potřebuje vysokou konzistence, LOCAL_QUORUM pro úroveň konzistence (pro čtení a zápisy) se ujistěte se, že hello místní čtení a zápisů jsou splněny z hello místní uzly data replikují asynchronně toohello vzdálené datových centrech.  Tabulka 2 shrnuje konfigurace hello podrobnosti o clusteru více oblast hello popsané dál v hello zápisu nahoru.

**Konfigurace clusteru Cassandra dva oblast**

| Parametr clusteru | Hodnota | Poznámky |
| --- | --- | --- |
| Počet uzlů (ne) |8 + 8 |Celkový počet uzlů v clusteru hello |
| Replikace faktor (RF) |3 |Počet replik daného řádku |
| Úroveň konzistence (zápisu) |LOCAL_QUORUM [(sum(RF)/2) +1) = 4] hello výsledek vzorec hello se zaokrouhlí směrem dolů |2 uzly se zapíšou toohello první datového centra synchronně; Další 2 uzly Hello potřebné pro kvorum, bude napsán asynchronně toohello 2. datové centrum. |
| Úroveň konzistence (čtení) |LOCAL_QUORUM ((RF/2) + 1) = 2 hello výsledek vzorec hello se zaokrouhlí směrem dolů |Požadavky pro čtení jsou splnit v pouze jedna oblast; 2 uzly jsou přečtěte si před hello odpověď je odeslána zpět toohello klienta. |
| Strategie replikace |Viz NetworkTopologyStrategy [replikaci dat](http://www.datastax.com/documentation/cassandra/2.0/cassandra/architecture/architectureDataDistributeReplication_c.html) v dokumentaci k Cassandra pro další informace |Jste srozuměni s tím topologie nasazení hello a umístí repliky na uzlech, aby všechny repliky hello nemáte skončili na hello stejné rack |
| Snitch |Viz GossipingPropertyFileSnitch [Snitches](http://www.datastax.com/documentation/cassandra/2.0/cassandra/architecture/architectureSnitchesAbout_c.html) v dokumentaci k Cassandra pro další informace |NetworkTopologyStrategy používá koncept snitch toounderstand hello topologie. GossipingPropertyFileSnitch poskytuje lepší kontrolu mapování každý uzel toodata center a racku. povídání toopropagate Hello clusteru pak použije tyto informace. To je mnohem jednodušší dynamické IP nastavení relativní tooPropertyFileSnitch |

## <a name="hello-software-configuration"></a>Hello konfigurace softwaru
Hello následující verze softwaru použitá při nasazení hello:

<table>
<tr><th>Software</th><th>Zdroj</th><th>Verze</th></tr>
<tr><td>PROSTŘEDÍ JRE    </td><td>[PROSTŘEDÍ JRE 8](http://www.oracle.com/technetwork/java/javase/downloads/server-jre8-downloads-2133154.html) </td><td>8U5</td></tr>
<tr><td>JNA    </td><td>[JNA](https://github.com/twall/jna) </td><td> 3.2.7</td></tr>
<tr><td>Cassandra</td><td>[Apache Cassandra 2.0.8](http://www.apache.org/dist/cassandra/2.0.8/apache-cassandra-2.0.8-bin.tar.gz)</td><td> 2.0.8</td></tr>
<tr><td>Ubuntu    </td><td>[Microsoft Azure](https://azure.microsoft.com/) </td><td>14.04 LTS</td></tr>
</table>

Vzhledem k tomu, že stahování prostředí JRE vyžaduje ruční přijetí Oracle licence, nasazení hello toosimplify, stažení všechny požadované softwarové toohello plochy později nahrát do image hello Ubuntu šablony, které budeme vytvářet jako cluster toohello prekurzorů hello nasazení.

Stáhněte si hello výše softwaru do adresáře dobře známé stahování (např. %TEMP%/downloads v systému Windows nebo ~/Downloads na Linuxových distribucích většiny Mac) na místním počítači hello.

### <a name="create-ubuntu-vm"></a>VYTVOŘENÍ VIRTUÁLNÍHO POČÍTAČE S UBUNTU
V tomto kroku procesu hello Ubuntu image pomocí vytvoříme hello požadovaného softwaru tak, aby hello bitovou kopii můžete znovu použít pro několik uzlů Cassandra zřizování.  

#### <a name="step-1-generate-ssh-key-pair"></a>Krok 1: Vygenerovat pár klíčů SSH
Azure potřebuje X509 veřejný klíč, který je PEM nebo DER kódovaný v hello zřizování čas. Generovat pár veřejného a privátního klíče pomocí pokynů hello umístěný na to, jak tooUse SSH s Linuxem v Azure. Pokud máte v plánu toouse putty.exe jako klient SSH buď na systému Windows nebo Linux, máte tooconvert hello PEM kódovaný formátu tooPPK privátního klíče RSA s využitím puttygen.exe; pokyny Hello naleznete v hello výše webové stránky.

#### <a name="step-2-create-ubuntu-template-vm"></a>Krok 2: Vytvoření Ubuntu šablony virtuálních počítačů
toocreate hello šablony virtuálních počítačů, přihlaste se k hello Azure classic portal a používání hello následující pořadí: klikněte na tlačítko Nový, výpočetní, virtuální počítač, FROM GALERIE, UBUNTU a Ubuntu Server 14.04 LTS a pak klikněte na šipku vpravo hello. Kurz, který popisuje, jak toocreate virtuálního počítače s Linuxem, najdete v části vytvořit virtuální počítač systémem Linux.

Zadejte následující informace na obrazovce hello "Konfigurace virtuálního počítače" #1 hello:

<table>
<tr><th>NÁZEV POLE              </td><td>       HODNOTA POLE               </td><td>         POZNÁMKY                </td><tr>
<tr><td>DATUM VYDÁNÍ VERZE    </td><td> Vyberte datum z hello rozevíracího seznamu</td><td></td><tr>
<tr><td>NÁZEV VIRTUÁLNÍHO POČÍTAČE    </td><td> Cass šablony.                   </td><td> Toto je název hostitele hello hello virtuálních počítačů </td><tr>
<tr><td>VRSTVY                     </td><td> STANDARD                           </td><td> Ponechte výchozí hello              </td><tr>
<tr><td>VELIKOST                     </td><td> A1                              </td><td>Vyberte hello virtuálních počítačů v závislosti na hello vstupně-výstupní operace musí; pro tento účel ponechte výchozí hello </td><tr>
<tr><td> NOVÉ UŽIVATELSKÉ JMÉNO             </td><td> localadmin                       </td><td> "admin" je vyhrazené uživatelské jméno ve 12. xx Ubuntu a</td><tr>
<tr><td> OVĚŘOVÁNÍ         </td><td> Klikněte na zaškrtávací políčko                 </td><td>Zkontrolujte, pokud chcete toosecure s klíčem SSH </td><tr>
<tr><td> CERTIFIKÁT             </td><td> Název souboru certifikátu veřejného klíče hello </td><td> Použití hello veřejný klíč vygenerovaný dříve</td><tr>
<tr><td> Nové heslo    </td><td> silné heslo </td><td> </td><tr>
<tr><td> Potvrzení hesla    </td><td> silné heslo </td><td></td><tr>
</table>

Zadejte následující informace na obrazovce hello "Konfigurace virtuálního počítače" #2 hello:

<table>
<tr><th>NÁZEV POLE             </th><th> HODNOTA POLE                       </th><th> POZNÁMKY                                 </th></tr>
<tr><td> CLOUDOVÉ SLUŽBY    </td><td> Vytvořte novou cloudovou službu    </td><td>Cloudová služba je kontejner výpočetní prostředky, a to jako virtuální počítače</td></tr>
<tr><td> NÁZEV CLOUDOVÉ SLUŽBY DNS    </td><td>Ubuntu template.cloudapp.net    </td><td>Pojmenujte nástroje pro vyrovnávání zatížení lhostejné počítače</td></tr>
<tr><td> OBLASTI NEBO SKUPINY VZTAHŮ NEBO VIRTUÁLNÍ SÍTĚ </td><td>    Západní USA    </td><td> Vyberte oblast, ve kterém webových aplikací, přístup ke clusteru Cassandra hello</td></tr>
<tr><td>ÚČET ÚLOŽIŠTĚ </td><td>    Použít výchozí    </td><td>Použití hello výchozí účet úložiště nebo předem vytvořený účet úložiště v určité oblasti.</td></tr>
<tr><td>SADY DOSTUPNOSTI. </td><td>    Žádný </td><td>    Ponechat prázdné</td></tr>
<tr><td>KONCOVÉ BODY    </td><td>Použít výchozí </td><td>    Použít konfiguraci SSH výchozí hello </td></tr>
</table>

Klikněte na šipku vpravo, nechte výchozí hodnoty hello na úvodní obrazovka #3 a klikněte na hello "kontrola" tlačítko toocomplete hello virtuálních počítačů zřizování procesu. Po několika minutách by měly mít hello virtuálních počítačů s hello název "ubuntu šablona" stav "spuštění".

### <a name="install-hello-necessary-software"></a>NAINSTALUJTE hello nezbytný SOFTWARE
#### <a name="step-1-upload-tarballs"></a>Krok 1: Nahrávání tarballs
Spojovací bod služby nebo pscp používáte, kopie hello dříve stažené softwaru příliš ~ / directory stahování pomocí hello příkaz formátu:

##### <a name="pscp-server-jre-8u5-linux-x64targz-localadminhk-cas-templatecloudappnethomelocaladmindownloadsserver-jre-8u5-linux-x64targz"></a>pscp server-prostředí jre-8u5-linux-x64.tar.gzlocaladmin@hk-cas-template.cloudapp.net:/home/localadmin/downloads/server-jre-8u5-linux-x64.tar.gz
Opakujte hello výše příkaz pro prostředí JRE také jako hello Cassandra bits.

#### <a name="step-2-prepare-hello-directory-structure-and-extract-hello-archives"></a>Krok 2: Příprava hello adresářovou strukturu a extrahovat hello archivy
Přihlaste se k hello virtuálních počítačů a vytvořit hello adresářovou strukturu a extrahovat softwaru jako superuživatele pomocí skriptu bash hello níže:

    #!/bin/bash
    CASS_INSTALL_DIR="/opt/cassandra"
    JRE_INSTALL_DIR="/opt/java"
    CASS_DATA_DIR="/var/lib/cassandra"
    CASS_LOG_DIR="/var/log/cassandra"
    DOWNLOADS_DIR="~/downloads"
    JRE_TARBALL="server-jre-8u5-linux-x64.tar.gz"
    CASS_TARBALL="apache-cassandra-2.0.8-bin.tar.gz"
    SVC_USER="localadmin"

    RESET_ERROR=1
    MKDIR_ERROR=2

    reset_installation ()
    {
       rm -rf $CASS_INSTALL_DIR 2> /dev/null
       rm -rf $JRE_INSTALL_DIR 2> /dev/null
       rm -rf $CASS_DATA_DIR 2> /dev/null
       rm -rf $CASS_LOG_DIR 2> /dev/null
    }
    make_dir ()
    {
       if [ -z "$1" ]
       then
          echo "make_dir: invalid directory name"
          exit $MKDIR_ERROR
       fi

       if [ -d "$1" ]
       then
          echo "make_dir: directory already exists"
          exit $MKDIR_ERROR
       fi

       mkdir $1 2>/dev/null
       if [ $? != 0 ]
       then
          echo "directory creation failed"
          exit $MKDIR_ERROR
       fi
    }

    unzip()
    {
       if [ $# == 2 ]
       then
          tar xzf $1 -C $2
       else
          echo "archive error"
       fi

    }

    if [ -n "$1" ]
    then
       SVC_USER=$1
    fi

    reset_installation
    make_dir $CASS_INSTALL_DIR
    make_dir $JRE_INSTALL_DIR
    make_dir $CASS_DATA_DIR
    make_dir $CASS_LOG_DIR

    #unzip JRE and Cassandra
    unzip $HOME/downloads/$JRE_TARBALL $JRE_INSTALL_DIR
    unzip $HOME/downloads/$CASS_TARBALL $CASS_INSTALL_DIR

    #Change hello ownership toohello service credentials

    chown -R $SVC_USER:$GROUP $CASS_DATA_DIR
    chown -R $SVC_USER:$GROUP $CASS_LOG_DIR
    echo "edit /etc/profile tooadd JRE toohello PATH"
    echo "installation is complete"


Pokud tento skript můžete vložit do okna vim, ujistěte se, že tooremove hello znaků CR návratový ('\r ") pomocí hello následující příkaz:

    tr -d '\r' <infile.sh >outfile.sh

#### <a name="step-3-edit-etcprofile"></a>Krok 3: Úprava atd nebo profil
Připojte následující hello na konci hello:

    JAVA_HOME=/opt/java/jdk1.8.0_05
    CASS_HOME= /opt/cassandra/apache-cassandra-2.0.8
    PATH=$PATH:$HOME/bin:$JAVA_HOME/bin:$CASS_HOME/bin
    export JAVA_HOME
    export CASS_HOME
    export PATH

#### <a name="step-4-install-jna-for-production-systems"></a>Krok 4: Instalace JNA pro produkční systémy.
Použití hello následující příkaz pořadí: hello následující příkaz, bude instalace jna-3.2.7.jar a jna. Platforma 3.2.7.jar too/usr/share.java directory sudo výstižný get nainstalovat libjna java

Vytvořte symbolické odkazy v adresáři CASS_HOME/lib $, aby tyto JAR najít Cassandra spouštěcí skript:

    ln -s /usr/share/java/jna-3.2.7.jar $CASS_HOME/lib/jna.jar

    ln -s /usr/share/java/jna-platform-3.2.7.jar $CASS_HOME/lib/jna-platform.jar

#### <a name="step-5-configure-cassandrayaml"></a>Krok 5: Konfigurace cassandra.yaml
Upravte cassandra.yaml na každý tooreflect konfigurace virtuálního počítače potřebné pro všechny virtuální počítače hello [jsme se vylepšení to při zřizování skutečné hello]:

<table>
<tr><th>Název pole   </th><th> Hodnota  </th><th>    Poznámky </th></tr>
<tr><td>název_clusteru </td><td>    "CustomerService"    </td><td> Použít hello název, který se vztahuje k nasazení</td></tr>
<tr><td>listen_address    </td><td>[necháte prázdné]    </td><td> Odstranit "localhost" </td></tr>
<tr><td>rpc_addres   </td><td>[necháte prázdné]    </td><td> Odstranit "localhost" </td></tr>
<tr><td>semen    </td><td>"10.1.2.4, 10.1.2.6, 10.1.2.8"    </td><td>Seznam všech hello IP adres, které jsou určené jako vstupní.</td></tr>
<tr><td>endpoint_snitch </td><td> org.apache.cassandra.locator.GossipingPropertyFileSnitch </td><td> Toto je používáno hello NetworkTopologyStrateg pro odvození hello datového centra a hello stojany hello virtuálních počítačů</td></tr>
</table>

#### <a name="step-6-capture-hello-vm-image"></a>Krok 6: Zachycení image virtuálního počítače hello
Přihlaste se k hello virtuálního počítače pomocí názvu hostitele hello (hk template.cloudapp.net certifikační autority) a hello SSH privátní klíč dříve vytvořili. V tématu Jak tooUse SSH s Linuxem v Azure pro podrobnosti na tom, jak toolog pomocí hello příkazu ssh nebo putty.exe.

Spusťte následující posloupnost akcí toocapture hello image hello:

##### <a name="1-deprovision"></a>1. Deprovision
Použijte hello příkaz "sudo příkaz waagent – deprovision + uživatele" tooremove konkrétní informace o instanci virtuálního počítače. Zobrazit [jak tooCapture virtuální počítač s Linuxem](capture-image.md) tooUse jako šablona více podrobností na proces zachycení image hello.

##### <a name="2-shutdown-hello-vm"></a>2: hello vypnutí virtuálního počítače
Ujistěte se, že je označený hello virtuálního počítače a klikněte na odkaz vypnutí hello z hello dolní příkazového řádku.

##### <a name="3-capture-hello-image"></a>3: hello přípravné bitové kopie
Ujistěte se, že je označený hello virtuálního počítače a klikněte na odkaz zachycení hello z hello dolní příkazového řádku. Na další obrazovce hello zadejte název bitové kopie (například hk-cas-2-08-ub-14-04-2014071), příslušné popis bitové kopie a klikněte na označit "kontrola" hello, toofinish proces zachycení hello.

Bude to trvat několik sekund a hello obrázku musí být k dispozici v části Moje Image z Galerie obrázků hello. Hello zdrojového virtuálního počítače se automaticky odstraní po úspěšně zachycení bitové kopie hello. 

## <a name="single-region-deployment-process"></a>Proces nasazení jedné oblasti
**Krok 1: Vytvoření virtuální sítě hello** Přihlaste se k hello portál Azure a vytvoření virtuální sítě (klasické) s atributy hello uvedené v následující tabulce hello. V tématu [vytvoření virtuální sítě (klasické) pomocí portálu Azure hello](../../../virtual-network/virtual-networks-create-vnet-classic-pportal.md) podrobné kroky procesu hello.      

<table>
<tr><th>Atribut název virtuálního počítače.</th><th>Hodnota</th><th>Poznámky</th></tr>
<tr><td>Name (Název)</td><td>vnet-cass západní USA</td><td></td></tr>
<tr><td>Oblast</td><td>Západní USA</td><td></td></tr>
<tr><td>Servery DNS</td><td>Žádný</td><td>Toto ignorovat, protože jsme nejsou pomocí serveru DNS</td></tr>
<tr><td>Adresní prostor</td><td>10.1.0.0/16</td><td></td></tr>    
<tr><td>Počáteční IP adresu</td><td>10.1.0.0</td><td></td></tr>    
<tr><td>CIDR </td><td>/16 (65531)</td><td></td></tr>
</table>

Přidejte hello následující podsítě:

<table>
<tr><th>Name (Název)</th><th>Počáteční IP adresu</th><th>CIDR</th><th>Poznámky</th></tr>
<tr><td>webové</td><td>10.1.1.0</td><td>/24 (251)</td><td>Podsíť pro hello webové farmy</td></tr>
<tr><td>data</td><td>10.1.2.0</td><td>/24 (251)</td><td>Podsíť pro uzly databáze hello</td></tr>
</table>

Data a webové podsítě se dají chránit pomocí skupin zabezpečení sítě hello pokrytí se sahá nad rámec tohoto článku.  

**Krok 2: Zřizování virtuálních počítačů** pomocí bitové kopie hello vytvořili dříve, bude vytvoření hello následující virtuální počítače v cloudu hello serveru "hk c-svc západ" a navázat je toohello příslušných podsítí, jak je uvedeno níže:

<table>
<tr><th>Název počítače    </th><th>Podsíť    </th><th>IP adresa    </th><th>Skupina dostupnosti</th><th>DC/Rack</th><th>Počáteční hodnoty?</th></tr>
<tr><td>(Hong Kong) c1-západní USA    </td><td>data    </td><td>10.1.2.4    </td><td>(Hong Kong) c sada-1    </td><td>DC = WESTUS rack = rack1 </td><td>Ano</td></tr>
<tr><td>(Hong Kong) c2-západní USA    </td><td>data    </td><td>10.1.2.5    </td><td>(Hong Kong) c sada-1    </td><td>DC = WESTUS rack = rack1    </td><td>Ne </td></tr>
<tr><td>(Hong Kong) c3-západní USA    </td><td>data    </td><td>10.1.2.6    </td><td>(Hong Kong) c sada-1    </td><td>DC = WESTUS rack = rack2    </td><td>Ano</td></tr>
<tr><td>(Hong Kong) c4-západní USA    </td><td>data    </td><td>10.1.2.7    </td><td>(Hong Kong) c sada-1    </td><td>DC = WESTUS rack = rack2    </td><td>Ne </td></tr>
<tr><td>(Hong Kong) c5-západní USA    </td><td>data    </td><td>10.1.2.8    </td><td>(Hong Kong) c sada-2    </td><td>DC = WESTUS rack = rack3    </td><td>Ano</td></tr>
<tr><td>(Hong Kong) c6-západní USA    </td><td>data    </td><td>10.1.2.9    </td><td>(Hong Kong) c sada-2    </td><td>DC = WESTUS rack = rack3    </td><td>Ne </td></tr>
<tr><td>(Hong Kong)-s c7 – západní USA    </td><td>data    </td><td>10.1.2.10    </td><td>(Hong Kong) c sada-2    </td><td>DC = WESTUS rack = rack4    </td><td>Ano</td></tr>
<tr><td>(Hong Kong) c8-západní USA    </td><td>data    </td><td>10.1.2.11    </td><td>(Hong Kong) c sada-2    </td><td>DC = WESTUS rack = rack4    </td><td>Ne </td></tr>
<tr><td>(Hong Kong) w1 – západní USA    </td><td>webové    </td><td>10.1.1.4    </td><td>(Hong Kong) w sada-1    </td><td>                       </td><td>Není k dispozici</td></tr>
<tr><td>(Hong Kong) w2 – západní USA    </td><td>webové    </td><td>10.1.1.5    </td><td>(Hong Kong) w sada-1    </td><td>                       </td><td>Není k dispozici</td></tr>
</table>

Vytvoření hello výše seznam virtuálních počítačů vyžaduje hello následující proces:

1. Vytvoření prázdné cloudové služby v určité oblasti.
2. Vytvoření virtuálního počítače z hello dříve zaznamenané bitové kopie a připojte ji toohello virtuální sítě vytvořené dříve; Tento postup opakujte pro všechny virtuální počítače hello
3. Přidejte cloudové služby interní služby load vyrovnávání toohello a připojte ji podsíť toohello "data.
4. Pro každý virtuální počítač předtím vytvořili přidáte koncový bod Vyrovnávání zatížení pro provoz thrift prostřednictvím Vyrovnávání zatížení interní skupinu s vyrovnáváním zatížení připojený sadu toohello vytvořili

Hello výše proces lze provést pomocí portálu Azure classic; počítače s Windows (použití a virtuálních počítačů v Azure, pokud nemáte počítač Windows tooa přístup), použijte všechny virtuální počítače 8 automaticky používat hello následující tooprovision skript prostředí PowerShell.

**Seznam 1: Skript prostředí PowerShell pro zřizování virtuálních počítačů**

        #Tested with Azure Powershell - November 2014
        #This powershell script deployes a number of VMs from an existing image inside an Azure region
        #Import your Azure subscription into hello current Powershell session before proceeding
        #hello process: 1. create Azure Storage account, 2. create virtual network, 3.create hello VM template, 2. crate a list of VMs from hello template

        #fundamental variables - change these tooreflect your subscription
        $country="us"; $region="west"; $vnetName = "your_vnet_name";$storageAccount="your_storage_account"
        $numVMs=8;$prefix = "hk-cass";$ilbIP="your_ilb_ip"
        $subscriptionName = "Azure_subscription_name";
        $vmSize="ExtraSmall"; $imageName="your_linux_image_name"
        $ilbName="ThriftInternalLB"; $thriftEndPoint="ThriftEndPoint"

        #generated variables
        $serviceName = "$prefix-svc-$region-$country"; $azureRegion = "$region $country"

        $vmNames = @()
        for ($i=0; $i -lt $numVMs; $i++)
        {
           $vmNames+=("$prefix-vm"+($i+1) + "-$region-$country" );
        }

        #select an Azure subscription already imported into Powershell session
        Select-AzureSubscription -SubscriptionName $subscriptionName -Current
        Set-AzureSubscription -SubscriptionName $subscriptionName -CurrentStorageAccountName $storageAccount

        #create an empty cloud service
        New-AzureService -ServiceName $serviceName -Label "hkcass$region" -Location $azureRegion
        Write-Host "Created $serviceName"

        $VMList= @()   # stores hello list of azure vm configuration objects
        #create hello list of VMs
        foreach($vmName in $vmNames)
        {
           $VMList += New-AzureVMConfig -Name $vmName -InstanceSize ExtraSmall -ImageName $imageName |
           Add-AzureProvisioningConfig -Linux -LinuxUser "localadmin" -Password "Local123" |
           Set-AzureSubnet "data"
        }

        New-AzureVM -ServiceName $serviceName -VNetName $vnetName -VMs $VMList

        #Create internal load balancer
        Add-AzureInternalLoadBalancer -ServiceName $serviceName -InternalLoadBalancerName $ilbName -SubnetName "data" -StaticVNetIPAddress "$ilbIP"
        Write-Host "Created $ilbName"
        #Add add hello thrift endpoint toohello internal load balancer for all hello VMs
        foreach($vmName in $vmNames)
        {
            Get-AzureVM -ServiceName $serviceName -Name $vmName |
                Add-AzureEndpoint -Name $thriftEndPoint -LBSetName "ThriftLBSet" -Protocol tcp -LocalPort 9160 -PublicPort 9160 -ProbePort 9160 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -InternalLoadBalancerName $ilbName |
                Update-AzureVM

            Write-Host "created $vmName"     
        }

**Krok 3: Konfigurace Cassandra na jednotlivé virtuální počítače**

Přihlaste se k hello virtuálního počítače a provádět hello následující:

* Upravte $CASS_HOME/conf/cassandra-rackdc.properties toospecify hello datového centra a rack vlastnosti:
  
       dc =EASTUS, rack =rack1
* Upravte uzly počáteční hodnoty tooconfigure cassandra.yaml jak je uvedeno níže:
  
       Seeds: "10.1.2.4,10.1.2.6,10.1.2.8,10.1.2.10"

**Krok 4: Spuštění hello virtuální počítače a otestovat hello clusteru**

Přihlášení do jednoho z uzlů hello, (například (Hong Kong) c1-západní us) a hello spusťte následující příkaz toosee hello stav hello clusteru:

       nodetool –h 10.1.2.4 –p 7199 status

Měli byste vidět hello zobrazení podobné toohello jeden níže 8 uzlů clusteru:

<table>
<tr><th>Status</th><th>Adresa    </th><th>Načtení    </th><th>Tokeny    </th><th>Vlastní </th><th>ID hostitele    </th><th>Rack</th></tr>
<tr><th>ZRUŠENÍ    </td><td>10.1.2.4     </td><td>87.81 KB    </td><td>256    </td><td>38.0%    </td><td>Identifikátor GUID (odebrat)</td><td>rack1</td></tr>
<tr><th>ZRUŠENÍ    </td><td>10.1.2.5     </td><td>41.08 KB    </td><td>256    </td><td>68.9%    </td><td>Identifikátor GUID (odebrat)</td><td>rack1</td></tr>
<tr><th>ZRUŠENÍ    </td><td>10.1.2.6     </td><td>55.29 KB    </td><td>256    </td><td>68.8%    </td><td>Identifikátor GUID (odebrat)</td><td>rack2</td></tr>
<tr><th>ZRUŠENÍ    </td><td>10.1.2.7     </td><td>55.29 KB    </td><td>256    </td><td>68.8%    </td><td>Identifikátor GUID (odebrat)</td><td>rack2</td></tr>
<tr><th>ZRUŠENÍ    </td><td>10.1.2.8     </td><td>55.29 KB    </td><td>256    </td><td>68.8%    </td><td>Identifikátor GUID (odebrat)</td><td>rack3</td></tr>
<tr><th>ZRUŠENÍ    </td><td>10.1.2.9     </td><td>55.29 KB    </td><td>256    </td><td>68.8%    </td><td>Identifikátor GUID (odebrat)</td><td>rack3</td></tr>
<tr><th>ZRUŠENÍ    </td><td>10.1.2.10     </td><td>55.29 KB    </td><td>256    </td><td>68.8%    </td><td>Identifikátor GUID (odebrat)</td><td>rack4</td></tr>
<tr><th>ZRUŠENÍ    </td><td>10.1.2.11     </td><td>55.29 KB    </td><td>256    </td><td>68.8%    </td><td>Identifikátor GUID (odebrat)</td><td>rack4</td></tr>
</table>

## <a name="test-hello-single-region-cluster"></a>Test hello jeden Cluster oblast
Použijte následující postup tootest hello clusteru hello:

1. Pomocí příkaz Get-AzureInternalLoadbalancer příkaz prostředí Powershell text hello, získejte IP adresu hello služby Vyrovnávání zatížení interní hello (např.)  10.1.2.101). Hello syntaxe příkazu hello je zobrazena níže: Get-AzureLoadbalancer – ServiceName "(Hong Kong) c-svc západní USA" [zobrazí podrobnosti o hello nástroje pro vyrovnávání zatížení interní hello společně s jeho IP adresy]
2. Přihlaste se k hello webové farmy virtuálního počítače (například hk-w1 – – západ us) pomocí klienta Putty ssh nebo
3. Spuštění $CASS_HOME/bin/cqlsh 10.1.2.101 9160
4. Použijte následující příkazy tooverify CQL Pokud funguje hello clusteru hello:
   
     Vytvoření KEYSPACE customers_ks REPLIKACE s = {'class': 'SimpleStrategy', 'replication_factor': 3};   POUŽITÍ customers_ks;   Vytvoření tabulky Customers(customer_id int PRIMARY KEY, firstname text, lastname text);   Vložit do VALUES(1, 'John', 'Doe') Customers(customer_id, firstname, lastname);   Vložit do Customers(customer_id, firstname, lastname) hodnoty (2, 'Jana', 'Doe');
   
     Vyberte * od zákazníků;

Měli byste vidět zobrazení jako hello jeden níže:

<table>
  <tr><th> customer_id </th><th> FirstName </th><th> Příjmení </th></tr>
  <tr><td> 1 </td><td> Jan </td><td> Doe </td></tr>
  <tr><td> 2 </td><td> Jana </td><td> Doe </td></tr>
</table>

Upozorňujeme, že tento hello keyspace vytvořili v kroku 4 používá SimpleStrategy s replication_factor 3. SimpleStrategy se doporučuje pro jednoho datového centra nasazení zatímco NetworkTopologyStrategy pro více data center nasazení. Replication_factor 3 získáte odolnost proti selhání uzlu.

## <a id="tworegion"></a>Procesu nasazení s více oblast
Bude využívat hello jedné oblasti nasazení bylo úspěšně dokončeno a opakujte hello stejný proces pro instalaci druhé oblast hello. Hello klíče rozdíl mezi hello jeden a více nasazení oblast je hello nastavení tunelové propojení VPN pro komunikaci mezi oblast; jsme bude začínat hello síťovou instalaci, zřídíte hello virtuální počítače a nakonfigurujete Cassandra.

### <a name="step-1-create-hello-virtual-network-at-hello-2nd-region"></a>Krok 1: Vytvoření hello virtuální sítě na hello oblast 2.
Přihlaste se k hello portál Azure classic a vytvoření virtuální sítě s hello atributy zobrazit v tabulce hello. V tématu [konfigurace virtuální sítě Cloud-Only v hello portál Azure classic](../../../virtual-network/virtual-networks-create-vnet-classic-pportal.md) podrobné kroky procesu hello.      

<table>
<tr><th>Název atributu    </th><th>Hodnota    </th><th>Poznámky</th></tr>
<tr><td>Name (Název)    </td><td>vnet-cass východ nám</td><td></td></tr>
<tr><td>Oblast    </td><td>Východ USA</td><td></td></tr>
<tr><td>Servery DNS        </td><td></td><td>Toto ignorovat, protože jsme nejsou pomocí serveru DNS</td></tr>
<tr><td>Konfigurace VPN typu point-to-site</td><td></td><td>        Můžete tuto zprávu ignorovat</td></tr>
<tr><td>Konfigurace sítě Site-to-Site VPN</td><td></td><td>        Můžete tuto zprávu ignorovat</td></tr>
<tr><td>Adresní prostor    </td><td>10.2.0.0/16</td><td></td></tr>
<tr><td>Počáteční IP adresu    </td><td>10.2.0.0    </td><td></td></tr>
<tr><td>CIDR    </td><td>/16 (65531)</td><td></td></tr>
</table>

Přidejte hello následující podsítě:

<table>
<tr><th>Name (Název)    </th><th>Počáteční IP adresu    </th><th>CIDR    </th><th>Poznámky</th></tr>
<tr><td>webové    </td><td>10.2.1.0    </td><td>/24 (251)    </td><td>Podsíť pro hello webové farmy</td></tr>
<tr><td>data    </td><td>10.2.2.0    </td><td>/24 (251)    </td><td>Podsíť pro uzly databáze hello</td></tr>
</table>


### <a name="step-2-create-local-networks"></a>Krok 2: Vytvoření místní sítě
Do místní sítě v Azure virtuální sítě je prostor adres proxy serveru, který mapuje tooa vzdálené lokality, včetně privátního cloudu nebo jiné oblasti Azure. Tento adresní prostor proxy je Brána vzdálené vázané tooa pro směrování sítě toohello právo sítě cíle. V tématu [konfigurace virtuální sítě tooVNet připojení](../../../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md) pokyny hello navazování připojení VNET-to-VNET.

Vytvořte dvě místní sítě za hello následující podrobnosti:

| Název sítě | Adresa brány sítě VPN | Adresní prostor | Poznámky |
| --- | --- | --- | --- |
| HK-lnet-map-to-East-us |23.1.1.1 |10.2.0.0/16 |Při vytváření hello místní sítě poskytněte zástupný symbol adresu brány. Adresa brány skutečné Hello vyplněno po vytvoření brány hello. Ujistěte se, zda text hello adresní prostor přesně odpovídá hello příslušné virtuální sítě vzdálené; v takovém případě hello síť VNET vytvořena v hello oblast východní USA. |
| HK-lnet-map-to-West-us |23.2.2.2 |10.1.0.0/16 |Při vytváření hello místní sítě poskytněte zástupný symbol adresu brány. Adresa brány skutečné Hello vyplněno po vytvoření brány hello. Ujistěte se, zda text hello adresní prostor přesně odpovídá hello příslušné virtuální sítě vzdálené; v takovém případě hello síť VNET vytvořena v hello oblast západní USA. |

### <a name="step-3-map-local-network-toohello-respective-vnets"></a>Krok 3: Mapy "Local" sítě toohello příslušné virtuální sítě
Z hello portál Azure classic vyberte každý virtuální síť, klikněte na tlačítko "Konfigurace", "Connect toohello místní sítě" a vyberte hello místní sítě za hello následující podrobnosti:

| Virtual Network | Místní sítě |
| --- | --- |
| (Hong Kong) vnet západní USA |HK-lnet-map-to-East-us |
| (Hong Kong) vnet-– východ nám |HK-lnet-map-to-West-us |

### <a name="step-4-create-gateways-on-vnet1-and-vnet2"></a>Krok 4: Vytvoření brány na VNET1 a VNET2
Z řídicího panelu hello obou hello virtuálních sítí klikněte na možnost vytvořit BRÁNU, která aktivuje bránu VPN hello procesu zřizování. Po pár minut hello řídicí panel každý virtuální sítě by měl zobrazit adresu hello skutečné brány.

### <a name="step-5-update-local-networks-with-hello-respective-gateway-addresses"></a>Krok 5: Aktualizace "Local" sítě s adresami příslušných "brány" hello
Upravte obě hello místní sítě tooreplace hello zástupný symbol brány IP adresu s hello skutečné IP adresu hello jenom zřídit brány. Použijte následující mapování hello:

<table>
<tr><th>Místní sítě    </th><th>Brána virtuální sítě</th></tr>
<tr><td>HK-lnet-map-to-East-us </td><td>Brána (Hong Kong) vnet západní USA</td></tr>
<tr><td>HK-lnet-map-to-West-us </td><td>Brána (Hong Kong) vnet-– východ nám</td></tr>
</table>

### <a name="step-6-update-hello-shared-key"></a>Krok 6: Aktualizace sdílený klíč hello
Použití hello následující klíč protokolu IPSec prostředí Powershell skriptu tooupdate hello každý brány VPN [použití hello saké klíč pro obě brány hello]: hk-lnet-map-to-west-us - LocalNetworkSiteName v Set AzureVNetGatewayKey - VNetName (Hong Kong) vnet-– východ us - SharedKey D9E76BKK Set-AzureVNetGatewayKey - VNetName (Hong Kong) vnet západní USA - LocalNetworkSiteName hk-lnet-map-to-east-us - SharedKey D9E76BKK

### <a name="step-7-establish-hello-vnet-to-vnet-connection"></a>Krok 7: Připojení hello VNET-to-VNET
Z hello portál Azure classic použijte nabídku "Řídicího PANELU" hello připojení brány brány tooestablish virtuální sítě i hello. Pomocí položky nabídky "Připojit" hello v hello dolním panelu nástrojů. Po několika minutách by měl hello řídicí panel zobrazit podrobnosti připojení hello graficky.

### <a name="step-8-create-hello-virtual-machines-in-region-2"></a>Krok 8: Vytvoření hello virtuálních počítačů v oblasti #2
Vytvoření bitové kopie Ubuntu hello, jak je popsáno v oblasti #1 nasazení podle následující hello stejné kroky nebo zkopírujte hello bitové kopie virtuálního pevného disku soubor toohello účtu úložiště Azure umístěný v oblasti #2 a vytvoření bitové kopie hello. Použijte tuto bitovou kopii a vytvoření hello následující seznam virtuálních počítačů do novou cloudovou službu, hk-c-svc východ nám:

| Název počítače | Podsíť | IP adresa | Skupina dostupnosti | DC/Rack | Počáteční hodnoty? |
| --- | --- | --- | --- | --- | --- |
| (Hong Kong) c1-východ nám |data |10.2.2.4 |(Hong Kong) c sada-1 |DC = EASTUS rack = rack1 |Ano |
| (Hong Kong) c2-východ nám |data |10.2.2.5 |(Hong Kong) c sada-1 |DC = EASTUS rack = rack1 |Ne |
| (Hong Kong) c3-východ nám |data |10.2.2.6 |(Hong Kong) c sada-1 |DC = EASTUS rack = rack2 |Ano |
| (Hong Kong) c5-východ nám |data |10.2.2.8 |(Hong Kong) c sada-2 |DC = EASTUS rack = rack3 |Ano |
| (Hong Kong) c6-východ nám |data |10.2.2.9 |(Hong Kong) c sada-2 |DC = EASTUS rack = rack3 |Ne |
| (Hong Kong)-s c7 – východ nám |data |10.2.2.10 |(Hong Kong) c sada-2 |DC = EASTUS rack = rack4 |Ano |
| (Hong Kong) c8-východ nám |data |10.2.2.11 |(Hong Kong) c sada-2 |DC = EASTUS rack = rack4 |Ne |
| (Hong Kong) w1 – východ nám |webové |10.2.1.4 |(Hong Kong) w sada-1 |Není k dispozici |Není k dispozici |
| (Hong Kong) w2 – východ nám |webové |10.2.1.5 |(Hong Kong) w sada-1 |Není k dispozici |Není k dispozici |

Postupujte podle hello stejné pokyny jako oblast #1 ale použít 10.2.xxx.xxx adresní prostor.

### <a name="step-9-configure-cassandra-on-each-vm"></a>Krok 9: Konfigurace Cassandra na jednotlivé virtuální počítače
Přihlaste se k hello virtuálního počítače a provádět hello následující:

1. Upravit toospecify $CASS_HOME/conf/cassandra-rackdc.properties hello datového centra a rack vlastnosti ve formátu hello: dc = EASTUS rack = rack1
2. Upravit uzly počáteční hodnoty tooconfigure cassandra.yaml: semen: "10.1.2.4,10.1.2.6,10.1.2.8,10.1.2.10,10.2.2.4,10.2.2.6,10.2.2.8,10.2.2.10"

### <a name="step-10-start-cassandra"></a>Krok 10: Spuštění Cassandra
Přihlaste se k každý virtuální počítač a spusťte Cassandra hello pozadí spuštěním hello následující příkaz: $CASS_HOME/bin/cassandra

## <a name="test-hello-multi-region-cluster"></a>Test hello clusteru více oblast
Nyní Cassandra byl nasazený too16 uzly s 8 uzlů v každé oblasti Azure. Tyto uzly jsou hello stejné clusteru na základě hello běžný název clusteru a konfigurace uzlu hello počáteční hodnoty. Použijte následující proces tootest hello clusteru hello:

### <a name="step-1-get-hello-internal-load-balancer-ip-for-both-hello-regions-using-powershell"></a>Krok 1: Získání IP nástroje pro vyrovnávání zatížení pro vnitřní hello pro obě hello oblasti pomocí prostředí PowerShell
* Get-AzureInternalLoadbalancer - ServiceName "(Hong Kong) c-svc západní USA"
* Get-AzureInternalLoadbalancer - ServiceName "(Hong Kong) c-svc Východ USA"  
  
    Všimněte si hello IP adresy (například – západ - 10.1.2.101, východ - 10.2.2.101) zobrazí.

### <a name="step-2-execute-hello-following-in-hello-west-region-after-logging-into-hk-w1-west-us"></a>Krok 2: Spuštění hello následující po přihlášení na hk-w1 – západní USA v oblasti západní hello
1. Spuštění $CASS_HOME/bin/cqlsh 10.1.2.101 9160
2. Spusťte následující příkazy CQL hello:
   
     Vytvoření KEYSPACE customers_ks REPLIKACE s = {'class': 'NetworkToplogyStrategy', 'WESTUS': 3, 'EASTUS': 3};   POUŽITÍ customers_ks;   Vytvoření tabulky Customers(customer_id int PRIMARY KEY, firstname text, lastname text);   Vložit do VALUES(1, 'John', 'Doe') Customers(customer_id, firstname, lastname);   Vložit do Customers(customer_id, firstname, lastname) hodnoty (2, 'Jana', 'Doe');   Vyberte * od zákazníků;

Měli byste vidět zobrazení jako hello jeden níže:

| customer_id | FirstName | Příjmení |
| --- | --- | --- |
| 1 |Jan |Doe |
| 2 |Jana |Doe |

### <a name="step-3-execute-hello-following-in-hello-east-region-after-logging-into-hk-w1-east-us"></a>Krok 3: Spusťte následující hello v oblasti Východ hello po přihlášení na hk-w1 – východ us:
1. Spuštění $CASS_HOME/bin/cqlsh 10.2.2.101 9160
2. Spusťte následující příkazy CQL hello:
   
     POUŽITÍ customers_ks;   Vytvoření tabulky Customers(customer_id int PRIMARY KEY, firstname text, lastname text);   Vložit do VALUES(1, 'John', 'Doe') Customers(customer_id, firstname, lastname);   Vložit do Customers(customer_id, firstname, lastname) hodnoty (2, 'Jana', 'Doe');   Vyberte * od zákazníků;

Měli byste vidět hello stejné zobrazit, jak je vidět pro oblast západní hello:

| customer_id | FirstName | Příjmení |
| --- | --- | --- |
| 1 |Jan |Doe |
| 2 |Jana |Doe |

Provést několik další vložení a zobrazit, že ty získat replikované toowest-nám součástí clusteru hello.

## <a name="test-cassandra-cluster-from-nodejs"></a>Test Cassandra Cluster z Node.js
Pomocí jednoho z virtuálních počítačů Linux hello dříve vytvářet ve vrstvě "web" hello, jsme provede jednoduché tooread hello dříve vložit dat skriptu Node.js

**Krok 1: Instalace Node.js a Cassandra klienta**

1. Nainstalovat Node.js a npm
2. Instalace uzlu balíček "cassandra klienta" pomocí npm
3. Spusťte následující skript prostředí řádku hello, který zobrazí řetězce formátu json hello hello načíst dat hello:
   
        var pooledCon = require('cassandra-client').PooledConnection;
        var ksName = "custsupport_ks";
        var cfName = "customers_cf";
        var hostList = ['internal_loadbalancer_ip:9160'];
        var ksConOptions = { hosts: hostList,
                             keyspace: ksName, use_bigints: false };
   
        function createKeyspace(callback){
           var cql = 'CREATE KEYSPACE ' + ksName + ' WITH strategy_class=SimpleStrategy AND strategy_options:replication_factor=1';
           var sysConOptions = { hosts: hostList,  
                                 keyspace: 'system', use_bigints: false };
           var con = new pooledCon(sysConOptions);
           con.execute(cql,[],function(err) {
           if (err) {
             console.log("Failed toocreate Keyspace: " + ksName);
             console.log(err);
           }
           else {
             console.log("Created Keyspace: " + ksName);
             callback(ksConOptions, populateCustomerData);
           }
           });
           con.shutdown();
        }
   
        function createColumnFamily(ksConOptions, callback){
          var params = ['customers_cf','custid','varint','custname',
                        'text','custaddress','text'];
          var cql = 'CREATE COLUMNFAMILY ? (? ? PRIMARY KEY,? ?, ? ?)';
        var con =  new pooledCon(ksConOptions);
          con.execute(cql,params,function(err) {
              if (err) {
                 console.log("Failed toocreate column family: " + params[0]);
                 console.log(err);
              }
              else {
                 console.log("Created column family: " + params[0]);
                 callback();
              }
          });
          con.shutdown();
        }
   
        //populate Data
        function populateCustomerData() {
           var params = ['John','Infinity Dr, TX', 1];
           updateCustomer(ksConOptions,params);
   
           params = ['Tom','Fermat Ln, WA', 2];
           updateCustomer(ksConOptions,params);
        }
   
        //update will also insert hello record if none exists
        function updateCustomer(ksConOptions,params)
        {
          var cql = 'UPDATE customers_cf SET custname=?,custaddress=? where custid=?';
          var con = new pooledCon(ksConOptions);
          con.execute(cql,params,function(err) {
              if (err) console.log(err);
              else console.log("Inserted customer : " + params[0]);
          });
          con.shutdown();
        }
   
        //read hello two rows inserted above
        function readCustomer(ksConOptions)
        {
          var cql = 'SELECT * FROM customers_cf WHERE custid IN (1,2)';
          var con = new pooledCon(ksConOptions);
          con.execute(cql,[],function(err,rows) {
              if (err)
                 console.log(err);
              else
                 for (var i=0; i<rows.length; i++)
                    console.log(JSON.stringify(rows[i]));
            });
           con.shutdown();
        }
   
        //exectue hello code
        createKeyspace(createColumnFamily);
        readCustomer(ksConOptions)

## <a name="conclusion"></a>Závěr
Microsoft Azure je flexibilní platforma, která umožňuje spuštění hello Microsoft jak software s otevřeným zdrojem, jak je ukázáno v tomto cvičení. Clustery s vysokou dostupností Cassandra můžete nasadit na jednoho datového centra prostřednictvím hello šíření uzlů clusteru hello napříč více domén selhání. Také je možné instalovat clustery Cassandra nad několika oblastmi Azure geograficky vzdáleným doklad systémů po havárii. Azure a umožňuje společně Cassandra hello konstrukce vysoce škálovatelnou a vysoce dostupné a po havárii obnovitelné cloudové služby potřeby škálovat podle dnešní internetové služby.  

## <a name="references"></a>Odkazy
* [http://cassandra.apache.org](http://cassandra.apache.org)
* [http://www.datastax.com](http://www.datastax.com)
* [http://www.nodejs.org](http://www.nodejs.org)

