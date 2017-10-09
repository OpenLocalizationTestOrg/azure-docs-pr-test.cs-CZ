---
title: "aaaWork s existujícími místními proxy servery a Azure AD | Microsoft Docs"
description: "Popisuje, jak toowork s existujícími místními proxy servery."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2017
ms.author: kgremban
ms.openlocfilehash: 7f8cec4f676f99bead5211bcbcf23056bd7f211f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="work-with-existing-on-premises-proxy-servers"></a>Práce s existující místní proxy servery

Tento článek vysvětluje, jak toowork konektory tooconfigure Proxy aplikace služby Azure Active Directory (Azure AD) s odchozí proxy servery. Je určený pro zákazníky s sítě prostředích, která mají existující proxy.

Můžeme začít hledáním v těchto scénářích hlavní nasazení:
* Nakonfigurujte konektory toobypass vaše místní odchozí proxy.
* Nakonfigurujte konektory toouse tooaccess odchozího proxy serveru proxy aplikace služby Azure AD.

Další informace o fungování konektory najdete v tématu [pochopit Azure AD Application Proxy konektory](application-proxy-understand-connectors.md).

## <a name="configure-hello-outbound-proxy"></a>Konfigurace odchozího proxy serveru hello

Pokud máte ve vašem prostředí odchozího proxy serveru, použijte účet s příslušnými oprávněními tooconfigure hello odchozí proxy. Protože hello instalační program se spustí v kontextu hello hello uživatele, který provádí instalaci hello, můžete zkontrolovat konfiguraci hello pomocí Microsoft Edge nebo jiný prohlížeč Internetu.

nastavení proxy serveru tooconfigure hello v Microsoft Edge:

1. Přejděte příliš**nastavení** > **zobrazení Upřesnit nastavení** > **otevřete nastavení proxy serveru** > **ruční instalace Proxy** .
2. Nastavit **použít proxy server** příliš**na**, vyberte hello **nepoužívejte hello proxy serveru pro místní adresy** zaškrtávací políčko a potom změnu hello adresu a port tooreflect váš místní proxy server.
3. Zadejte nastavení proxy serveru potřebné hello.

   ![Dialogové okno pro nastavení proxy serveru](./media/application-proxy-working-with-proxy-servers/proxy-bypass-local-addresses.png)

## <a name="bypass-outbound-proxies"></a>Odchozí proxy jednorázové přihlášení

Konektory mít základní součásti operačního systému, které odchozí požadavky. Automaticky se pokusí tyto součásti toolocate proxy server v síti hello. Používají Proxy Auto-Discovery WPAD (Web), pokud je povoleno v prostředí hello.

součásti operačního systému Hello pokusit toolocate proxy server pomocí uskutečňují vyhledávání DNS pro wpad.domainsuffix. Pokud to řeší ve službě DNS, požadavek HTTP je potom k toohello IP adresu pro wpad.dat. Tento požadavek se změní na skript konfigurace proxy serveru hello ve vašem prostředí. konektor Hello používá tento skript tooselect serveru odchozího proxy serveru. Ale provoz konektor nemusí stále projít, z důvodu nastavení konfigurace na hello proxy.

Můžete nakonfigurovat toobypass konektor hello vaší tooensure místní proxy server, který používá přímé připojení toohello Azure services. Doporučujeme, abyste tento přístup (Pokud je pro něj umožňuje zásady sítě), protože znamená, že máte jeden menší toomaintain konfigurace.

využití toodisable odchozího proxy serveru pro konektor hello, upravte soubor C:\Program Files\Microsoft AAD aplikace Proxy Connector\ApplicationProxyConnectorService.exe.config hello a přidejte hello *system.net* části uvedené v této ukázce kódu :

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.net>
    <defaultProxy enabled="false"></defaultProxy>
  </system.net>
  <runtime>
    <gcServer enabled="true"/>
  </runtime>
  <appSettings>
    <add key="TraceFilename" value="AadAppProxyConnector.log" />
  </appSettings>
</configuration>
```
tooensure service Connector Updater hello také obchází hello proxy, ujistěte se, podobně jako změnu toohello ApplicationProxyConnectorUpdaterService.exe.config soubor nacházející se v C:\Program Files\Microsoft AAD aplikace Proxy Connector Updater.

Zda toomake kopie hello původní soubory, se v případě, že potřebujete toorevert toohello výchozí .config soubory.

## <a name="use-hello-outbound-proxy-server"></a>Použít hello odchozí proxy server

Některé prostředí vyžaduje všechny odchozí přenosy toogo prostřednictvím odchozího proxy serveru, bez výjimky. V důsledku toho obcházení hello proxy není možné.

Hello konektor provoz toogo prostřednictvím hello odchozího proxy serveru, můžete nakonfigurovat, jak je znázorněno v následujícím diagramu hello:

 ![Konfigurace konektoru toogo provoz prostřednictvím tooAzure odchozího proxy serveru AD Proxy aplikace](./media/application-proxy-working-with-proxy-servers/configure-proxy-settings.png)

V důsledku toho, že pouze odchozí přenosy, neexistuje žádný tooconfigure nutné příchozí přístup přes vaší brány firewall.

### <a name="step-1-configure-hello-connector-and-related-services-toogo-through-hello-outbound-proxy"></a>Krok 1: Konfigurace hello konektoru a související služby toogo prostřednictvím hello odchozího proxy serveru

Jak je popsané dříve, pokud je povoleno v prostředí hello a správně nakonfigurována WPAD, konektor hello automaticky zjistit hello odchozí proxy server a pokus o toouse ho. Můžete však explicitně nakonfigurovat konektor toogo hello prostřednictvím odchozího proxy serveru.

toodo tedy upravit soubor C:\Program Files\Microsoft AAD aplikace Proxy Connector\ApplicationProxyConnectorService.exe.config hello a přidat hello *system.net* části uvedené v této ukázce kódu. Změna *proxyserver:8080* tooreflect vaše místní proxy server název nebo IP adresa a hello portu naslouchá na.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.net>  
    <defaultProxy>   
      <proxy proxyaddress="http://proxyserver:8080" bypassonlocal="True" usesystemdefault="True"/>   
    </defaultProxy>  
  </system.net>
  <runtime>
    <gcServer enabled="true"/>
  </runtime>
  <appSettings>
    <add key="TraceFilename" value="AadAppProxyConnector.log" />
  </appSettings>
</configuration>
```

V dalším kroku nakonfigurujte proxy server hello Connector Updater služby toouse hello tím, že v C:\Program Files\Microsoft AAD aplikace proxy serveru konektoru Updater\ApplicationProxyConnectorUpdaterService.exe.config podobné soubor toohello změnu.

### <a name="step-2-configure-hello-proxy-tooallow-traffic-from-hello-connector-and-related-services-tooflow-through"></a>Krok 2: Konfigurace hello proxy tooallow provoz z hello konektoru a související služby tooflow prostřednictvím

Existují čtyři aspekty tooconsider v hello odchozího proxy serveru:
* Odchozí pravidla proxy
* Ověřování proxy serverem
* Porty proxy
* Kontrolu SSL

#### <a name="proxy-outbound-rules"></a>Odchozí pravidla proxy
Povolit přístup toohello následující koncové body pro přístup k službě konektor:

* *. msappproxy.net
* *. servicebus.windows.net

Pro počáteční registraci povolit přístup toohello následující koncové body:

* login.windows.net
* Login.microsoftonline.com

Pokud nelze povolit připojení ve plně kvalifikovaný název domény a místo toho musí toospecify rozsahy IP adres, použijte tyto možnosti:

* Povolí odchozí přístup hello konektor tooall cíle.
* Povolit odchozí přístup hello konektor příliš[rozsahy IP adres Azure datacenter](https://www.microsoft.com/en-gb/download/details.aspx?id=41653). Hello výzvy s použitím hello seznam rozsahů IP adres Azure datacenter je, že je aktualizovaný týdně. Je nutné tooput procesu v místě tooensure, že jsou vaše pravidla přístupu k příslušným způsobem aktualizuje.

#### <a name="proxy-authentication"></a>Ověřování proxy serverem

Ověřování proxy není aktuálně podporován. Naše doporučení aktuální je tooallow hello konektor anonymní přístup toohello Internet cíle.

#### <a name="proxy-ports"></a>Porty proxy

Hello konektor umožňuje odchozí připojení založené na protokolu SSL pomocí metody CONNECT hello. Tato metoda je v podstatě nastavuje tunelové propojení prostřednictvím hello odchozího proxy serveru. Nakonfigurujte hello proxy server tooallow tunelování tooports 443 a 80.

>[!NOTE]
>Spuštění služby Service Bus přes protokol HTTPS používá port 443. Ve výchozím nastavení, ale Service Bus pokusí přímé připojení TCP a spadne zpět tooHTTPS pouze v případě, že přímé připojení se nezdaří.

tooensure, který hello Service Bus se odesílá i přes hello odchozí proxy server, ujistěte se, že tento hello connector se nemůže připojit přímo toohello Azure services pro porty 9350, 9352 a 5671.

#### <a name="ssl-inspection"></a>Kontrolu SSL
Nepoužívejte kontrolu SSL pro provoz hello konektoru, protože způsobuje problémy týkající se provozu konektor hello.

## <a name="troubleshoot-connector-proxy-problems-and-service-connectivity-issues"></a>Řešení potíží s konektor proxy problémy a potíže s připojením služby
Nyní byste měli vidět veškerý provoz přes proxy server hello. Pokud máte potíže, by měly pomoci hello následující informace o odstraňování potíží.

Hello nejlepší způsob, jak tooidentify a řešení potíží s připojením konektor problémy je tootake síť zaznamenat na službu konektoru hello při spouštění služby konektoru hello. To může být složitý úkol, takže Podíváme se na rychlé tipy k zaznamenání a filtrování trasování sítě.

Můžete použít hello monitorování nástroj podle svého výběru. Pro účely hello tohoto článku použili jsme Microsoft Network Monitor 3.4. Můžete [ji stáhnout z Microsoft](https://www.microsoft.com/download/details.aspx?id=4865).

Příklady Hello a filtry, které používáme v hello následující části jsou konkrétní tooNetwork monitorování, ale hello zásad může být použité tooany nástroj pro analýzu.

### <a name="take-a-capture-by-using-network-monitor"></a>Proveďte zachycení pomocí sledování sítě

toostart zachycení:

1. Otevřete sledování sítě a klikněte na tlačítko **nové zachycení**.
2. Klikněte na tlačítko hello **spustit** tlačítko.

   ![Okno monitorování sítě](./media/application-proxy-working-with-proxy-servers/network-capture.png)

Po dokončení zachycení, klikněte na tlačítko hello **Zastavit** tooend tlačítko ji.

### <a name="take-a-capture-of-connector-traffic"></a>Proveďte zachycení provozu konektoru

Pro počáteční řešení potíží s proveďte hello následující kroky:

1. Z services.msc zastavte službu konektoru Proxy aplikace služby Azure AD hello.
2. Spusťte hello zachycení dat ze sítě.
3. Spusťte službu konektoru Proxy aplikace služby Azure AD hello.
4. Zastavte hello zachycení dat ze sítě.

   ![Služba Azure AD konektor Proxy aplikace v services.msc](./media/application-proxy-working-with-proxy-servers/services-local.png)

### <a name="look-at-hello-requests-from-hello-connector-toohello-proxy-server"></a>Podívejte se na požadavky hello z hello konektor toohello proxy serveru

Teď, když máte k dispozici zachycení dat ze sítě, jste připravené toofilter ho. Hello klíče toolooking na hello trasování je pochopení, jak zachytit toofilter hello.

Jeden filtr je následující (kde 8080 je port služby proxy hello):

**(http. Požadavek nebo http. Odpověď) a tcp.port==8080**

Pokud zadáte tento filtr v hello **filtru zobrazení** a vyberte **použít**, filtruje hello zachycení provozu na základě filtru hello.

Hello předchozí filtr zobrazuje jenom hello požadavky a odpovědi HTTP z hello port proxy serveru. Pro spuštění konektoru kde hello konektor je nakonfigurované toouse proxy server hello filtr by zobrazit přibližně takto:

 ![Příklad seznamu Filtrované požadavky a odpovědi HTTP](./media/application-proxy-working-with-proxy-servers/http-requests.png)

Nyní konkrétně hledáte hello CONNECT požadavky, které zobrazit komunikaci se serverem proxy hello. Po úspěšné zobrazí se odpověď HTTP OK (200).

Pokud se zobrazí další kódy odpovědí, jako je například 407 nebo 502, hello proxy je vyžadování ověřování nebo není umožňuje hello provoz z jiného důvodu. V tomto okamžiku zaujmout váš tým podpory proxy serveru.

### <a name="identify-failed-tcp-connection-attempts"></a>Identifikovat neúspěšné pokusy o připojení protokolu TCP

Hello další běžné scénáře, který vás může zajímat je při hello konektoru se pokouší tooconnect přímo, ale selhává.

Jiné sledování sítě filtr, který pomáhá tooeasily identifikovat tento problém je:

**Vlastnost. TCPSynRetransmit**

Paket SYN je odeslán paket první hello tooestablish připojení TCP. Pokud tomuto paketu nevrací odpověď, je reattempted hello SYN. Hello předcházející toosee filtru můžete použít všechny požadavky SYN opakovaně. Potom můžete zkontrolovat, zda tyto požadavky SYN odpovídají tooany související konektor provoz.

Hello následující příklad ukazuje, pokus o připojení se nezdařilo tooService sběrnice port 9352:

 ![Příklad odpověď pro pokus o připojení se nezdařilo](./media/application-proxy-working-with-proxy-servers/failed-connection-attempt.png)

Pokud se zobrazí přibližně hello předcházející odpovědi, konektor hello se pokouší toocommunicate přímo s hello služby Azure Service Bus. Pokud očekáváte hello konektor toomake přímé připojení toohello Azure services, tato odpověď není jasný náznak, že máte síť nebo brána firewall problému.

>[!NOTE]
>Pokud jste nakonfigurované toouse proxy server, tato odpověď může znamenat, že Service Bus se pokouší o přímé připojení TCP před přepnutím tooattempting připojení přes protokol HTTPS.
>

Analýzy trasování sítě není u všech uživatelů. Ale může být a cenným nástrojem tooget rychlé informace o co se děje s vaší sítě.

Pokud budete pokračovat toostruggle s problémy s připojením k konektor, vytvořte lístek s náš tým podpory. Hello tým pomoct s další informace o řešení.

Informace o řešení chyb s konektor Proxy aplikace najdete v tématu [Poradce při potížích s Proxy aplikace](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-troubleshoot).

## <a name="next-steps"></a>Další kroky

[Pochopení konektory proxy aplikace služby Azure AD](application-proxy-understand-connectors.md)<br>
[Jak toosilently instalovat hello konektoru Proxy aplikace služby Azure AD](active-directory-application-proxy-silent-installation.md)
