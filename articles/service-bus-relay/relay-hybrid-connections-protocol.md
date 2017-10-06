---
title: "Hybridní připojení aaaAzure přenosového protokolu Průvodce | Microsoft Docs"
description: "Azure Průvodce protokol předávání hybridní připojení."
services: service-bus-relay
documentationcenter: na
author: clemensv
manager: timlt
editor: 
ms.assetid: 149f980c-3702-4805-8069-5321275bc3e8
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/03/2017
ms.author: sethm;clemensv
ms.openlocfilehash: 2d145d919d606ae4722b063e1baf39fb845a600a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# Azure hybridní připojení předávací protokol
Předávání přes Azure je jedním z hello klíče schopností pilíře hello platformy Azure Service Bus. Hello nové *hybridní připojení* funkce předávání je zabezpečený, otevřete protokol evolution na základě protokolu HTTP a objekty WebSockets. Nahrazuje hello dřívějším, stejně s názvem *BizTalk Services* funkce, který byl postavený na vlastnickým protokolem foundation. integrace Hello hybridní připojení do Azure App Services bude pokračovat toofunction jako-je.

Hybridní připojení umožňuje obousměrnou, binární datový proud komunikaci mezi dvěma síťových aplikací, během které může být buď nebo obou stran umístěn za zařízení NAT nebo brány firewall. Tento článek popisuje interakce na straně klienta hello s hello hybridní připojení přenosu pro připojení klientů v naslouchací proces a odesílatele rolí a jak naslouchací procesy přijímat nová připojení.

## Interakce modelu
předávání Hello hybridní připojení dvě strany připojí tím, že poskytuje bod potkávací v cloudu Azure, která obou stran můžete vyhledat a připojit vlastní síť perspektivy toofrom hello. Tento bod potkávací se nazývá "Hybridní připojení" v tomto a dalších dokumentace v hello rozhraní API a také v hello portálu Azure. Hello hybridní připojení je koncový bod služby označuje tooas hello "služba" pro hello zbývající části tohoto článku. model interakce Hello leans na hello klasifikace vymezenému mnoho jiná síťová rozhraní API.

Je naslouchací proces, který nejprve znamená připravenosti toohandle příchozí připojení a následně je přijímá po doručení. Na dobrý den druhé straně, je připojování klienta, který se připojuje ke hello naslouchací proces, byla očekávána tohoto připojení toobe přijaté pro navázání obousměrné komunikační cestu.
"Připojit" "Naslouchání," a "Přijmout" jsou hello stejné podmínky najdete ve většině soketu rozhraní API.

Všechny přenosu komunikační model má buď strany odchozí připojení ke koncovému bodu služby, které vytváří naslouchání"hello" i "client" v obecné použití, může také způsobit dalšími přetíženími terminologie. Hello přesné terminologií, které jsme proto použít pro hybridní připojení je následující:

vzhledem k tomu, že jsou klienti toohello služby, se nazývají Hello programy na obou stranách připojení "klienty,". Hello klienta, který čeká a přijímá připojení je "naslouchací proces", nebo je uvedená toobe hello "naslouchací proces role." Hello klienta, který iniciuje nové připojení směrem naslouchací proces prostřednictvím služby hello se označuje jako "sender" hello, nebo je v "odesílatele role."

### Naslouchací proces interakce
Hello naslouchací proces má čtyři interakce s hello služby; všechny podrobnosti přenosová jsou popsané dále v tomto článku v části odkaz hello.

#### Naslouchání
tooindicate připravenosti služby toohello že naslouchací proces je připraven tooaccept připojení, vytvoří odchozí připojení protokolu WebSocket. metoda handshake připojení Hello představuje název hello hybridní připojení nakonfigurovaná na obor názvů hello předávání a token zabezpečení, která uděluje hello "Naslouchání" přímo na tento název.
Když hello protokolu WebSocket je přijatá službou hello, hello registrace je dokončena a hello vytvořit web, ke které se ukládají jako hello "řídicí kanál" pro povolení všechny následné interakce zachování připojení protokolu WebSocket. Služba Hello umožňuje až too25 souběžných posluchače na hybridní připojení. Pokud existují dvě nebo více active naslouchací procesy, jsou mezi nimi rozložit příchozí připojení v náhodném pořadí; správného distribuční není zaručena.

#### Přijmout
Při otevření nové připojení služby hello odesílatele hello služby vybere a upozorní jednu aktivní naslouchacího procesu hello na hello hybridní připojení. Toto oznámení se odesílá naslouchací proces toohello přes hello otevřít řídicí kanál jako JSON zprávu, že obsahující hello adresu URL koncového bodu protokolu WebSocket hello, který hello naslouchacího procesu musí připojit toofor přijímá připojení hello.

Adresa URL Hello můžete a musí používat přímo naslouchací proces hello bez další zátěže.
informace o Hello kódovaný je platný pouze na krátkou dobu běhu v podstatě po dobu, po jako odesílatele hello ochotná toowait hello připojení toobe navázat klient server, ale až tooa maximálně 30 sekund. Adresa URL Hello dá použít jenom pro jeden úspěšného pokusu o připojení. Co nejdříve hello připojení s hello potkávací adresa URL je stanoveno, všechny další aktivity na tento protokol WebSocket je přes předávací službu z protokolu WebSocket a toohello odesílatele, bez zásahu nebo interpretace službou hello.

#### Obnovit
Hello token zabezpečení, který je nutné použít tooregister hello naslouchací proces a údržbu, že řídicí kanál může vyprší během naslouchací proces hello je aktivní. vypršení platnosti tokenu Hello nemá vliv na probíhající připojení, ale způsobit hello řízení kanál toobe vyřadit službou hello v nebo krátce po hello okamžiku vypršení platnosti. operace "obnovit" Hello je, že zprávu JSON, která hello naslouchací proces, můžete odeslat token hello tooreplace přidružené hello řídicí kanál, tak, aby hello řídicí kanál je možné udržovat po delší dobu.

#### Ping
Je-li hello řídicí kanál nečinnosti, po dlouhou dobu, prostředníci na hello způsobem, například zatížení vyrovnávání nebo zařízení NAT. může dojít k přerušení připojení TCP hello. zabraňuje operace "ping" Hello, který odesláním malé množství dat na hello kanálu, který upozorní, všichni členové hello síťovou trasu připojení hello je určen toobe zachování připojení, a slouží také jako "živé" test pro naslouchací proces hello. V případě selhání příkazu ping hello hello řídicí kanál by měl být považován za nepoužitelný a naslouchací proces hello by měl znovu připojit.

### Odesílatel interakce
Odesílatel Hello má jenom jeden interakce službou hello: připojení.

#### Připojení
operace "připojit" Hello otevře protokolu WebSocket hello služby, poskytnete hello název hello hybridní připojení a (volitelně, ale vyžaduje ve výchozím nastavení) token zabezpečení udělující oprávnění "Odeslat" v řetězci dotazu hello. Služba Hello potom komunikuje s naslouchací proces hello v hello způsobem popsané a naslouchací proces hello vytvoří potkávací připojení, který je spojen s Tento protokolu WebSocket. Po přijetí hello protokolu WebSocket, jsou všechny další interakce na tomto protokolu WebSocket připojené naslouchací proces.

### Interakce souhrn
Výsledkem Hello tohoto modelu interakce je, že tento klient odesílatele hello dodává mimo dohodnout s "čistou" WebSocket, který je připojený tooa naslouchací proces a potřebného žádné další preambles nebo přípravu. Tento model umožňuje prakticky jakékoli existující protokolu WebSocket klienta implementace tooreadily využít výhody hello hybridní připojení služby zadáním adresy URL správně vytvořená do jejich vrstvy klienta protokolu WebSocket.

Hello potkávací připojení protokolu WebSocket, který hello naslouchací proces získává prostřednictvím přijmout interakce je taky čistou a mohou být předávány tooany existující protokolu WebSocket serveru implementace s některé minimální navíc abstrakce, která rozlišuje mezi "přijmout" operace na jejich framework naslouchací procesy místní sítě a hybridních připojení vzdálené "přijmout" operace.

## Referenční informace o protokolu

Tato část popisuje hello podrobnosti o interakcích protokol hello popsané.

Všechna připojení protokolu WebSocket probíhají na portu 443 jako upgrade z verze 1.1 HTTPS, který je obvykle abstrahované některé protokolu WebSocket framework nebo rozhraní API. Popis tady je udržováno implementace neutrální, bez návrhy konkrétní rozhraní.

### Naslouchací proces protokolu
naslouchací proces protokolu Hello se skládá z dvě připojení gesta a tři operace zpráv.

#### Naslouchací proces připojení kanálu ovládací prvek
s vytvoření připojení protokolu WebSocket k otevření Hello řídicí kanál:

```
wss://{namespace-address}/$hc/{path}?sb-hc-action=...[&sb-hc-id=...]&sb-hc-token=...
```

Hello `namespace-address` je hello plně kvalifikovaný název domény hello předávání přes Azure oboru názvů, hostitelé hello hybridní připojení, obvykle ve formátu hello `{myname}.servicebus.windows.net`.

Možnosti parametr řetězce dotazu Hello jsou následujícím způsobem.

| Parametr | Požaduje se | Popis |
| --- | --- | --- |
| `sb-hc-action` |Ano |Hello role hello naslouchacího procesu musí být parametr **sb-hc-action = naslouchání** |
| `{path}` |Ano |Cesta oboru názvů kódovaná adresou URL Hello hello předkonfigurované tooregister hybridní připojení této naslouchací proces na. Tento výraz určen připojením toohello pevné `$hc/` část adresy obsahující cestu. |
| `sb-hc-token` |Ano\* |Hello naslouchací proces musíte zadat platný, kódovaná adresou URL služby sběrnice sdíleného přístupu Token pro obor názvů hello nebo hybridní připojení, která uděluje hello **naslouchání** správné. |
| `sb-hc-id` |Ne |Toto volitelné ID klienta zadaný umožňuje začátku do konce diagnostické trasování. |

Pokud se hello připojení protokolu WebSocket nezdaří z důvodu toohello hybridní připojení cesta není zaregistrované, nebo token neplatný nebo chybějící nebo některé jiné chyby, poskytnutí zpětné vazby chyba hello pomocí hello regulární HTTP 1.1 Stav zpětné vazby modelu. Popis stav obsahuje chyby sledování id, které může být oznamovat pracovníky technické podpory Azure:

| Kód | Chyba | Popis |
| --- | --- | --- |
| 404 |Nebyl nalezen |Hello hybridní připojení cesta je neplatná nebo je špatná adresa URL základní hello. |
| 401 |Neautorizováno |token zabezpečení Hello je chybějící nebo poškozený nebo neplatný. |
| 403 |Je zakázané |token zabezpečení Hello není platný pro tuto cestu pro tuto akci. |
| 500 |Vnitřní chyba |Došlo k chybě ve službě hello. |

Pokud hello připojení protokolu WebSocket záměrně ukončení služby hello po začátku bylo nastaveno tak hello důvodem tak informace se předávají pomocí odpovídající kód chyby protokolu WebSocket společně s popisný chybová zpráva, která zahrnuje i pro sledování ID. Hello service nebude ukončena řídicí kanál bez zjištění chybový stav. Všechny čistého vypnutí je klient řídí.

| Stav WS | Popis |
| --- | --- |
| 1001 |Hello hybridní připojení cesty byla odstraněna nebo zakázána. |
| 1008 |vypršela platnost tokenu zabezpečení Hello, takže porušení zásad autorizace hello. |
| 1011 |Došlo k chybě ve službě hello. |

### Přijměte metody handshake
Hello "přijmout" jsou oznámení odesílána tím hello služby toohello naslouchací proces prostřednictvím administrate řídicí kanál jako zprávy JSON v objektu WebSocket. Neexistuje žádné toothis zpráva s odpovědí.

Hello zpráva obsahuje objekt JSON s názvem "přijmout,", který definuje hello následující vlastnosti v tuto chvíli:

* **Adresa** – hello toobe řetězce adresy URL použitá pro stanovení hello protokolu WebSocket toothe služby tooaccept příchozí připojení.
* **ID** – hello jedinečný identifikátor pro toto připojení. Pokud byl hello ID odesílatele klientem hello, je hello odesílatele zadána hodnota, v opačném případě je hodnota vygenerované systémem.
* **connectHeaders** – všechny hlavičky protokolu HTTP, které byly zadaný koncový bod předávání toohello hello odesílatel, také zahrnující hello protokolu WebSocket sekundu a hlavičky Sec. WebSocket rozšíření.

#### Přijmout zprávu

```json
{                                                           
    "accept" : {
        "address" : "wss://168.61.148.205:443/$hc/{path}?..."    
        "id" : "4cb542c3-047a-4d40-a19f-bdc66441e736",  
        "connectHeaders" : {                                         
            "Host" : "...",                                                
            "Sec-WebSocket-Protocol" : "...",                              
            "Sec-WebSocket-Extensions" : "..."                             
        }                                                            
     }                                                         
}
```

Adresa URL Hello zadaný v hello je zpráva JSON používají hello naslouchací proces ke zřízení hello protokolu WebSocket pro přijetí nebo odmítnutí hello odesílatele soketu.

#### Přijímání soketu hello
tooaccept, vytvoří naslouchací proces hello adresu toohello poskytuje připojení protokolu WebSocket.

Pokud hello "přijmout" zpráva má u sebe `Sec-WebSocket-Protocol` záhlaví, očekává se, že naslouchací proces hello přijímá hello protokolu WebSocket pouze, pokud ji podporuje tento protokol. Kromě toho nastaví záhlaví hello jako hello navázání protokolu WebSocket.

Hello totéž platí i toohello `Sec-WebSocket-Extensions` záhlaví. Pokud hello framework podporuje rozšíření, je potřeba nastavit hello záhlaví toohello serverové odpověď hello požadované `Sec-WebSocket-Extensions` metody handshake pro rozšíření hello.

Adresa URL Hello je nutné použít jako-jsou pro stanovení hello přijmout soketu, ale obsahuje následující parametry:

| Parametr | Požaduje se | Popis |
| --- | --- | --- |
| `sb-hc-action` |Ano |Pro příjem soketu, musí být parametr hello`sb-hc-action=accept` |
| `{path}` |Ano |(viz následující odstavce hello) |
| `sb-hc-id` |Ne |Viz popis v předchozí **id**. |

`{path}`je, že cesta oboru názvů kódovaná adresou URL hello hello předkonfigurované hybridní připojení, na které tooregister tento naslouchací proces. Tento výraz určen připojením toothe pevné `$hc/` část adresy obsahující cestu. 

Hello `path` výraz může být rozšířena příponu a výrazu řetězec dotazu, který následuje po dělicí lomítkem hello registrovaný název. To umožňuje hello odesílatele klienta toopass odesílání argumenty toohello přijímá naslouchacího procesu, když není možné tooinclude HTTP hlavičky. Hello očekávání je tento naslouchací proces hello framework analyzuje část adresy obsahující cestu pevné hello a registrovaný název hello z cesty a vytvoří hello zbývající pravděpodobně bez argumentů řetězce dotazu předponu `sb-`, aplikace k dispozici toohello pro rozhodnutí zda tooaccept hello připojení.

Další informace najdete v následující části "Odesílatele protokol" hello.

Pokud dojde k chybě, můžete službu hello odpovědět následujícím způsobem:

| Kód | Chyba | Popis |
| --- | --- | --- |
| 403 |Je zakázané |Adresa URL Hello není platný. |
| 500 |Vnitřní chyba |Došlo k chybě ve službě hello |

Po navázání připojení hello hello vypnutí serveru hello protokolu WebSocket při hello odesílatele protokolu WebSocket vypne, nebo s hello následující stav:

| Stav WS | Popis |
| --- | --- |
| 1001 |Hello odesílatele klienta vypne hello připojení. |
| 1001 |Hello hybridní připojení cesty byla odstraněna nebo zakázána. |
| 1008 |vypršela platnost tokenu zabezpečení Hello, takže porušení zásad autorizace hello. |
| 1011 |Došlo k chybě ve službě hello. |

#### Odmítat hello soketů
Odmítat hello soketu po provádějící kontrolu uvítací zprávu "přijmout" vyžaduje podobné metody handshake proto, aby hello stavový kód a popis stavu komunikaci, že můžete toku důvod zamítnutí hello toohello odesílatele.

Hello protokol návrhu volba je toouse ověření typu handshake protokolu WebSocket (který je navrženou tooend ve stavu chyby definované) tak, aby naslouchací proces implementacích klienta na klienta protokolu WebSocket lze dále toorely a není nutné využívat navíc, úplné klienta HTTP.

tooreject hello soketu, hello klienta přebírá hello adresu URI ze zprávy "přijmout" hello a připojí dvě tooit parametrů řetězce dotazu, následujícím způsobem:

| Param | Požaduje se | Popis |
| --- | --- | --- |
| statusCode |Ano |Číselné stavový kód HTTP. |
| Popis_stavu |Ano |Lidské čitelný důvod zamítnutí hello. |

Hello výsledné URI je pak používá tooestablish připojení protokolu WebSocket.

Při dokončení správně, tato metoda handshake záměrně selže, s kódem chyby protokolu HTTP 410, protože žádné protokolu WebSocket. Pokud se operace nezdaří, popisují hello následující kódy chyb hello:

| Kód | Chyba | Popis |
| --- | --- | --- |
| 403 |Je zakázané |Adresa URL Hello není platný. |
| 500 |Vnitřní chyba |Došlo k chybě ve službě hello. |

### Naslouchací proces obnovení tokenu
Pokud token naslouchací proces hello o tooexpire, ho můžete nahradit odesláním služby textového rámečku zpráva toohello prostřednictvím hello navázat řídicí kanál. Zpráva obsahuje objekt JSON s názvem `renewToken`, která definuje hello v tuto chvíli následující vlastnosti:

* **token** – přístup k službě sběrnice sdílené token platný, kódovaná adresou URL pro obor názvů nebo hybridní připojení, která uděluje hello **naslouchání** správné.

#### zpráva renewToken

```json
{                                                                                                                                                                        
    "renewToken" : {                                                                                                                                                      
        "token" : "SharedAccessSignature sr=http%3a%2f%2fcontoso.servicebus.windows.net%2fhyco%2f&amp;sig=XXXXXXXXXX%3d&amp;se=1471633754&amp;skn=SasKeyName"  
    }                                                                                                                                                                     
}
```

Pokud se nezdaří ověření tokenu hello, byl odepřen přístup a hello Cloudová služba zavře hello řídicí kanál protokolu WebSocket s chybou. V opačném případě nepřijde žádná odpověď.

| Stav WS | Popis |
| --- | --- |
| 1008 |vypršela platnost tokenu zabezpečení Hello, takže porušení zásad autorizace hello. |

## Odesílatel protokolu
protokol odesílatele Hello je efektivně identické toohello způsob, jakým se vytvořit naslouchací proces.
cílem Hello je maximální průhlednost pro hello začátku do konce protokolu WebSocket. Hello adresu pro připojení toois hello stejná jako u hello naslouchání, ale hello "action" se liší a token potřebuje různých oprávnění:

```
wss://{namespace-address}/$hc/{path}?sb-hc-action=...&sb-hc-id=...&sbc-hc-token=...
```

Hello *obor názvů adresy* je hello plně kvalifikovaný název domény hello předávání přes Azure oboru názvů, hostitelé hello hybridní připojení, obvykle ve formátu hello `{myname}.servicebus.windows.net`.

žádost o Hello může obsahovat libovolné další hlavičky protokolu HTTP, včetně těch definované aplikací. Všechna zadaná hlavičky toku toohello naslouchací proces a můžete najít na hello `connectHeader` objekt hello **přijmout** řídicí zpráva.

Možnosti parametr řetězce dotazu Hello jsou následující:

| Param | Povinné? | Popis |
| --- | --- | --- |
| `sb-hc-action` |Ano |Pro roli hello odesílatele, musí být parametr hello `action=connect`. |
| `{path}` |Ano |(viz následující odstavce hello) |
| `sb-hc-token` |Ano\* |Hello naslouchací proces musíte zadat platný, kódovaná adresou URL služby sběrnice sdíleného přístupu Token pro obor názvů hello nebo hybridní připojení, která uděluje hello **odeslat** správné. |
| `sb-hc-id` |Ne |Volitelné ID, které umožňuje začátku do konce diagnostické trasování a je vytvořen naslouchací proces k dispozici toohello během hello přijmout metody handshake. |

Hello `{path}` je cesta oboru názvů kódovaná adresou URL hello hello předkonfigurované hybridní připojení, na které tooregister tento naslouchací proces. Hello `path` výraz může být rozšířena příponu a toocommunicate výraz řetězec dotazu další. Pokud hello hybridní připojení je registrován v cestě hello `hyco`, hello `path` výraz může být `hyco/suffix?param=value&...` a potom podle parametrů řetězce dotazu hello zde definované. Výraz dokončení může být pak následujícím způsobem:

```
wss://{namespace-address}/$hc/hyco/suffix?param=value&sb-hc-action=...[&sb-hc-id=...&]sbc-hc-token=...
```

Hello `path` prostřednictvím naslouchací proces toohello hello adresu URI, které jsou obsažené v řídicí zpráva "přijmout" hello je předán výraz.

Pokud se hello připojení protokolu WebSocket nezdaří z důvodu toohello hybridní připojení cesta není zaregistrovaný, token neplatný nebo chybějící nebo některé jiné chyby, poskytnutí zpětné vazby chyba hello pomocí hello regulární HTTP 1.1 Stav zpětné vazby modelu. Popis stavu obsahuje chybu sledování ID, které mohou být předávány na pracovníky podpory Azure:

| Kód | Chyba | Popis |
| --- | --- | --- |
| 404 |Nebyl nalezen |Hello hybridní připojení cesta je neplatná nebo je špatná adresa URL základní hello. |
| 401 |Neautorizováno |token zabezpečení Hello je chybějící nebo poškozený nebo neplatný. |
| 403 |Je zakázané |Hello token zabezpečení není platný pro tuto cestu a pro tuto akci. |
| 500 |Vnitřní chyba |Došlo k chybě ve službě hello. |

Pokud hello připojení protokolu WebSocket je záměrně vypnout službou hello po jeho byla původně nastavena, hello důvodem tak informace se předávají pomocí odpovídající kód chyby protokolu WebSocket společně s popisný chybová zpráva, která zahrnuje i ID sledování.

| Stav WS | Popis |
| --- | --- |
| 1000 |naslouchací proces Hello vypnout hello soketu. |
| 1001 |Hello hybridní připojení cesty byla odstraněna nebo zakázána. |
| 1008 |vypršela platnost tokenu zabezpečení Hello, takže porušení zásad autorizace hello. |
| 1011 |Došlo k chybě ve službě hello. |

## Další kroky
* [Přenos – nejčastější dotazy](relay-faq.md)
* [Vytvoření oboru názvů](relay-create-namespace-portal.md)
* [Začínáme s .NET](relay-hybrid-connections-dotnet-get-started.md)
* [Začínáme s aplikací Node](relay-hybrid-connections-node-get-started.md)

