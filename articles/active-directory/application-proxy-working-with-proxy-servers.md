---
title: "Práce s existujícími místními proxy servery a Azure AD | Microsoft Docs"
description: "Popisuje, jak pracovat s existující místní proxy servery."
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
ms.openlocfilehash: bdca442755507c4ffe8d43692c5b7f2aa3a746f3
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="work-with-existing-on-premises-proxy-servers"></a>Práce s existující místní proxy servery

Tento článek vysvětluje, jak konfigurovat konektory Proxy aplikace služby Azure Active Directory (Azure AD) pro práci s odchozí proxy servery. Je určený pro zákazníky s sítě prostředích, která mají existující proxy.

Můžeme začít hledáním v těchto scénářích hlavní nasazení:
* Nakonfigurujte konektory pro vynechat vaše místní odchozí proxy.
* Nakonfigurujte konektory používat odchozího proxy serveru pro přístup k Azure AD Application Proxy.

Další informace o fungování konektory najdete v tématu [pochopit Azure AD Application Proxy konektory](application-proxy-understand-connectors.md).

## <a name="configure-the-outbound-proxy"></a>Konfigurace odchozího proxy serveru

Pokud máte ve vašem prostředí odchozího proxy serveru, použijte účet s příslušnými oprávněními ke konfiguraci odchozího proxy serveru. Vzhledem k tomu, že instalační program se spustí v kontextu uživatele, který provádí instalaci, můžete zkontrolovat konfiguraci pomocí Microsoft Edge nebo jiný prohlížeč Internetu.

Konfigurace nastavení proxy serveru v Microsoft Edge:

1. Přejděte na **nastavení** > **zobrazení upřesňující nastavení** > **otevřete nastavení proxy serveru** > **instalace ruční Proxy**.
2. Nastavit **použít proxy server** k **na**, vyberte **nechcete používat proxy server pro místní adresy** zaškrtněte políčko a potom změňte adresu a port, aby odrážela místní proxy server.
3. Zadejte nastavení potřebné proxy serveru.

   ![Dialogové okno pro nastavení proxy serveru](./media/application-proxy-working-with-proxy-servers/proxy-bypass-local-addresses.png)

## <a name="bypass-outbound-proxies"></a>Odchozí proxy jednorázové přihlášení

Konektory mít základní součásti operačního systému, které odchozí požadavky. Tyto součásti se automaticky pokusí vyhledat proxy server v síti. Používají Proxy Auto-Discovery WPAD (Web), pokud je povolena v prostředí.

Součásti operačního systému se pokusí vyhledat proxy server pomocí vyhledávání DNS pro wpad.domainsuffix. Pokud to řeší ve službě DNS, je IP adresa pro wpad.dat potom k požadavku HTTP. Tento požadavek se změní na skript konfigurace proxy serveru ve vašem prostředí. Konektor používá tento skript k vyberte odchozí proxy server. Ale provoz konektor nemusí stále projít, z důvodu nastavení konfigurace na proxy serveru.

Můžete nakonfigurovat konektor Nepoužívat proxy místní zajistit, že používá přímé připojení ke službám Azure. Doporučujeme, abyste tento přístup (Pokud je pro něj umožňuje zásady sítě), protože znamená, že máte jeden menší konfiguraci k údržbě.

Zakázat používání odchozího proxy serveru pro konektor, upravte soubor C:\Program Files\Microsoft AAD aplikace Proxy Connector\ApplicationProxyConnectorService.exe.config a přidat *system.net* části uvedené v této ukázce kódu:

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
Zajistit, že službu Connector Updater také obchází proxy server, změňte podobné ApplicationProxyConnectorUpdaterService.exe.config umístěného v C:\Program Files\Microsoft AAD aplikace Proxy Connector Updater.

Ujistěte se, aby byly kopie aplikace původních souborů, v případě, že potřebujete obnovit výchozí soubory .config.

## <a name="use-the-outbound-proxy-server"></a>Použít odchozí proxy server

Některé prostředí vyžaduje všechny odchozí přenosy projít odchozího proxy serveru, bez výjimky. V důsledku toho obcházení proxy serveru není možné.

Konektor provoz projít odchozího proxy serveru, můžete nakonfigurovat, jak je znázorněno v následujícím diagramu:

 ![Konfigurace konektoru provoz projít odchozího proxy serveru, na proxy aplikace služby Azure AD](./media/application-proxy-working-with-proxy-servers/configure-proxy-settings.png)

V důsledku s pouze odchozí přenosy, není nutné konfigurovat příchozí přístup přes vaší brány firewall.

### <a name="step-1-configure-the-connector-and-related-services-to-go-through-the-outbound-proxy"></a>Krok 1: Konfigurace konektoru a související služby projít odchozího proxy serveru

Jak je popsané dříve, pokud je povoleno v prostředí a správně nakonfigurována WPAD, konektor automaticky zjišťovat odchozí proxy server a pokus o použití ho. Můžete však explicitně nakonfigurovat konektor projít odchozího proxy serveru.

Uděláte to tak, upravte soubor C:\Program Files\Microsoft AAD aplikace Proxy Connector\ApplicationProxyConnectorService.exe.config a přidat *system.net* části uvedené v této ukázce kódu. Změna *proxyserver:8080* tak, aby odrážela název místní proxy serveru nebo IP adresu a port, který naslouchá na.

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

V dalším kroku nakonfigurujte službu Connector Updater na používání proxy serveru tak, že podobné změny do souboru, složce C:\Program Files\Microsoft AAD aplikace proxy serveru konektoru Updater\ApplicationProxyConnectorUpdaterService.exe.config.

### <a name="step-2-configure-the-proxy-to-allow-traffic-from-the-connector-and-related-services-to-flow-through"></a>Krok 2: Konfigurace proxy serveru, který chcete povolit přenosy z konektoru a související služby k procházet skrz

Existují čtyři aspekty, které je třeba zvážit v odchozího proxy serveru:
* Odchozí pravidla proxy
* Ověřování proxy serverem
* Porty proxy
* Kontrolu SSL

#### <a name="proxy-outbound-rules"></a>Odchozí pravidla proxy
Povolit přístup k vytvoření následujících koncových bodů pro přístup k službě konektor:

* *. msappproxy.net
* *. servicebus.windows.net

Pro počáteční registraci povolte přístup k vytvoření následujících koncových bodů:

* login.windows.net
* Login.microsoftonline.com

Pokud nemůžete povolit připojení ve plně kvalifikovaný název domény a je nutné místo toho zadat rozsahy IP adres, použijte tyto možnosti:

* Povolí odchozí přístup konektoru na všechna místa určení.
* Povolit konektor odchozí přístup k [rozsahy IP adres Azure datacenter](https://www.microsoft.com/en-gb/download/details.aspx?id=41653). Problém s použitím seznamu rozsahů IP adres Azure datacenter je, že je aktualizovaný týdně. Musíte uvést proces k zajištění, že jsou vaše pravidla přístupu k příslušným způsobem aktualizuje.

#### <a name="proxy-authentication"></a>Ověřování proxy serverem

Ověřování proxy není aktuálně podporován. Naše doporučení aktuální je umožnit konektor anonymní přístup k Internetu cíle.

#### <a name="proxy-ports"></a>Porty proxy

Konektor umožňuje odchozí připojení založené na protokolu SSL pomocí metody připojení. Tato metoda je v podstatě nastavuje tunelové propojení prostřednictvím odchozího proxy serveru. Konfigurace proxy serveru tak, aby tunelového propojení pro porty 443 a 80.

>[!NOTE]
>Spuštění služby Service Bus přes protokol HTTPS používá port 443. Ve výchozím nastavení, ale Service Bus pokusí přímé připojení TCP a spadne zpět na HTTPS pouze v případě, že přímé připojení se nezdaří.

Aby se zajistilo, že Service Bus se také odesílá přes odchozí proxy serveru, ověřte, zda konektor nelze připojit přímo ke službám Azure pro porty 9350, 9352 a 5671.

#### <a name="ssl-inspection"></a>Kontrolu SSL
Pro provoz konektor, nepoužívejte kontrolu SSL, protože způsobuje problémy týkající se provozu konektor.

## <a name="troubleshoot-connector-proxy-problems-and-service-connectivity-issues"></a>Řešení potíží s konektor proxy problémy a potíže s připojením služby
Nyní byste měli vidět veškerý provoz prostřednictvím proxy serveru. Pokud máte potíže, by měly pomoci následující informace o odstraňování potíží.

Nejlepší způsob, jak identifikovat a řešit problémy s připojením k konektor je převést zachycení dat ze sítě na službu konektoru při spouštění služby konektoru. To může být složitý úkol, takže Podíváme se na rychlé tipy k zaznamenání a filtrování trasování sítě.

Můžete použít nástroj monitorování podle svého výběru. Pro účely tohoto článku použili jsme Microsoft Network Monitor 3.4. Můžete [ji stáhnout z Microsoft](https://www.microsoft.com/download/details.aspx?id=4865).

Příklady a filtry, které používáme v následujících částech jsou specifické pro sledování sítě, ale zásady lze použít pro nástroj pro analýzu.

### <a name="take-a-capture-by-using-network-monitor"></a>Proveďte zachycení pomocí sledování sítě

Spuštění zachycení:

1. Otevřete sledování sítě a klikněte na tlačítko **nové zachycení**.
2. Klikněte **spustit** tlačítko.

   ![Okno monitorování sítě](./media/application-proxy-working-with-proxy-servers/network-capture.png)

Po dokončení zachycení, klikněte **Zastavit** tlačítko ukončete ji.

### <a name="take-a-capture-of-connector-traffic"></a>Proveďte zachycení provozu konektoru

Pro počáteční řešení potíží, proveďte následující kroky:

1. Ze souboru services.msc zastavte službu konektoru Proxy aplikace služby Azure AD.
2. Spusťte zachycení dat ze sítě.
3. Spusťte službu konektoru Proxy aplikace služby Azure AD.
4. Zastavte zachycení dat ze sítě.

   ![Služba Azure AD konektor Proxy aplikace v services.msc](./media/application-proxy-working-with-proxy-servers/services-local.png)

### <a name="look-at-the-requests-from-the-connector-to-the-proxy-server"></a>Podívejte se na žádosti z konektoru k proxy serveru

Teď, když máte k dispozici zachycení dat ze sítě, jste připravení ho filtrovat. Klíč k prohlížení trasování je pochopení, jak filtrovat zachytávání.

Jeden filtr je následující (kde 8080 je port proxy serveru služby):

**(http. Požadavek nebo http. Odpověď) a tcp.port==8080**

Pokud zadáte tento filtr v **filtru zobrazení** a vyberte **použít**, filtruje zachycená data podle filtru.

Předchozí filtr zobrazuje jenom požadavky a odpovědi HTTP z port proxy serveru. Pro spuštění konektoru konfigurovaným konektor používat proxy server by filtr zobrazit přibližně takto:

 ![Příklad seznamu Filtrované požadavky a odpovědi HTTP](./media/application-proxy-working-with-proxy-servers/http-requests.png)

Nyní konkrétně díváte pro požadavky připojení, které se zobrazí komunikace se serverem proxy. Po úspěšné zobrazí se odpověď HTTP OK (200).

Pokud se zobrazí další kódy odpovědí, jako je například 407 nebo 502, proxy server je vyžadování ověřování nebo že nepovolí provoz z jiného důvodu. V tomto okamžiku zaujmout váš tým podpory proxy serveru.

### <a name="identify-failed-tcp-connection-attempts"></a>Identifikovat neúspěšné pokusy o připojení protokolu TCP

Další běžné scénáře, který vás může zajímat je když tento konektor se snaží připojit přímo, ale selhává.

Jiný filtr sledování sítě, který vám umožní snadno identifikovat tento problém je:

**Vlastnost. TCPSynRetransmit**

Paket SYN je první paket odeslaný k navázání připojení TCP. Pokud tomuto paketu nevrací odpověď, je reattempted SYN. Předchozí filtrem můžete zobrazit všechny požadavky SYN opakovaně. Potom můžete zkontrolovat, zda tyto požadavky SYN odpovídají přenosy dat souvisejících s konektoru.

Následující příklad ukazuje, pokus o selhání připojení k Service Bus port 9352:

 ![Příklad odpověď pro pokus o připojení se nezdařilo](./media/application-proxy-working-with-proxy-servers/failed-connection-attempt.png)

Pokud se něco podobného jako předchozí odpovědi, konektor se pokouší komunikovat přímo se službou Azure Service Bus. Pokud očekáváte, konektor navázat přímé připojení ke službám Azure, je tato odezva jasný náznak, že máte síť nebo brána firewall problému.

>[!NOTE]
>Pokud je nakonfigurována k používání proxy serveru, tato odpověď může znamenat, že Service Bus se pokouší o přímé připojení TCP před přepnutím do pokusu o připojení přes protokol HTTPS.
>

Analýzy trasování sítě není u všech uživatelů. Ale může být cenným nástrojem pro rychlé informace o co se děje s vaší sítě.

Pokud budete pokračovat, potíže se čtením s problémy s připojením k konektor, vytvořte lístek s náš tým podpory. Tým vám mohou pomoci s další informace o řešení.

Informace o řešení chyb s konektor Proxy aplikace najdete v tématu [Poradce při potížích s Proxy aplikace](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-troubleshoot).

## <a name="next-steps"></a>Další kroky

[Pochopení konektory proxy aplikace služby Azure AD](application-proxy-understand-connectors.md)<br>
[Postup při bezobslužné instalaci konektoru Proxy aplikace Azure AD](active-directory-application-proxy-silent-installation.md)
