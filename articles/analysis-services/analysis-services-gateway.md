---
title: "Brána dat aaaOn místní | Microsoft Docs"
description: "Bránu místní je nezbytný, pokud váš server služby Analysis Services v Azure se budou připojovat tooon místní datové zdroje."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: cd596155-b608-4a34-935e-e45c95d884a9
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/21/2017
ms.author: owend
ms.openlocfilehash: fc7b9c69e6f81b41deb7a5d6d963225593845d84
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connecting-tooon-premises-data-sources-with-azure-on-premises-data-gateway"></a>Připojování tooon místní zdroje dat pomocí Azure místní brány dat
Brána dat místní Hello funguje jako mostu, zajištění zabezpečených dat přenos mezi místní zdroje dat a vaše servery Azure Analysis Services v cloudu hello. V přidání tooworking s více servery Azure Analysis Services v hello stejné oblasti, hello nejnovější verzi této brány hello taky spolupracuje se službou Azure Logic Apps, Power BI, Power aplikace a Flow společnosti Microsoft. Můžete přidružit více služeb v hello stejné oblasti s jednou bránou. 

 Azure Analysis Services vyžaduje prostředek brány v hello stejné oblasti. Například pokud máte servery Azure Analysis Services v oblasti Východ USA 2 hello, musíte prostředek brány v oblasti Východ USA 2 hello. Několik serverů v oblasti Východ USA 2 můžete použít hello stejné bráně.

Získávání nastavení s hello brány hello poprvé je proces, který sestávající ze čtyř částí:

- **Stáhněte a spusťte instalační program** – tento krok nainstaluje služba brány do počítače ve vaší organizaci.

- **Zaregistrujte bránu** – v tomto kroku, zadejte název a obnovení klíče pro bránu a vyberte oblast, hello cloudové službě Brána pro registraci brány.

- **Vytvořte prostředek brány v Azure** – v tomto kroku vytvoříte bránu prostředků ve vašem předplatném Azure.

- **Připojte prostředek brány tooyour vaše servery** – až budete mít bránu prostředků v rámci vašeho předplatného, můžete začít připojení tooit vaše servery.

Až budete mít bránu prostředek nakonfigurovaný pro vaše předplatné, se můžete připojit víc serverů a dalších služeb tooit. Pouze potřebovat tooinstall jinou bránu a vytvářet prostředky další brány, pokud máte servery nebo jiné služby v jiné oblasti.

tooget spustit hned, najdete v části [nainstalujte a nakonfigurujte místní brána dat](analysis-services-gateway-install.md).

## <a name="how-it-works"></a>Jak to funguje
Brána Hello nainstalujete na počítač ve vaší organizaci používá jako služby systému Windows, **místní brána dat**. Tato místní služba není zaregistrována hello cloudové službě Brána pro přes Azure Service Bus. Pak vytvořte prostředek brány cloudové službě brány pro vaše předplatné Azure. Vaše Azure Analysis Services, které jsou potom servery připojené tooyour prostředek brány. Když modely na váš server nutné tooconnect tooyour místní zdroje dat pro dotazy nebo zpracování, hello dotaz a datové toku traverses hello brány prostředku, Azure Service Bus, místní služba Brána dat a zdrojům dat. 

![Jak to funguje](./media/analysis-services-gateway/aas-gateway-how-it-works.png)

Tok dotazy a data:

1. Dotaz byl vytvořený hello cloudové služby s hello šifrovat přihlašovací údaje pro zdroj dat pro místní hello. Potom odeslal tooa fronty pro tooprocess hello brány.
2. cloudové službě Brána pro Hello analyzuje hello dotazu a nabízených oznámení hello požadavek toohello [Azure Service Bus](https://azure.microsoft.com/documentation/services/service-bus/).
3. Brána dat místní Hello dotazuje hello Azure Service Bus pro žádosti čekající na vyřízení.
4. Brána Hello získá hello dotazu, dešifruje hello přihlašovací údaje a připojí toohello zdroje dat pomocí těchto přihlašovacích údajů.
5. Brána Hello odešle datový zdroj toohello hello dotazu pro provedení.
6. výsledky Hello se odesílají ze zdroje dat hello, back toohello brány a pak na hello Cloudová služba a serverem.

## <a name="windows-service-account"></a>Účtu služby systému Windows
Hello místní brána dat je nakonfigurované toouse *NT SERVICE\PBIEgwService* pro přihlašovací pověření služby systému Windows hello. Ve výchozím nastavení má hello vpravo přihlášení jako služba; v kontextu hello hello počítače, který instalujete na hello brány. Tento přihlašovací údaj není hello stejné zdroje dat tooon místní účet používaný tooconnect nebo účtu Azure.  

Pokud narazíte na potíže s proxy serveru z důvodu tooauthentication, může být vhodné toochange hello Windows služby účet uživatele domény tooa nebo spravovaný účet služby.

## <a name="ports"></a>Porty
Hello brány vytvoří tooAzure odchozí připojení k Service Bus. Komunikuje na odchozí porty: TCP 443 (výchozí), 5671, 5672, 9350 prostřednictvím 9354.  Brána Hello nevyžaduje příchozí porty.

Doporučujeme povolených hello IP adresy pro vaši oblast dat v bráně firewall. Můžete si stáhnout hello [seznamu Microsoft Azure Datacenter IP](https://www.microsoft.com/download/details.aspx?id=41653). Tento seznam je aktualizovaný týdně.

> [!NOTE]
> v notaci CIDR, jsou uvedené v seznamu IP Datacentra Azure hello Hello IP adresy. Například 10.0.0.0/24 neznamená 10.0.0.0 prostřednictvím 10.0.0.24. Další informace o hello [notaci CIDR](http://whatismyipaddress.com/cidr).
>
>

Hello následují hello plně kvalifikovaný názvy domén používat hello gateway.

| Názvy domén | Odchozí porty | Popis |
| --- | --- | --- |
| *. powerbi.com |80 |Instalační program toodownload hello používá protokol HTTP. |
| *. powerbi.com |443 |HTTPS |
| *. analysis.windows.net |443 |HTTPS |
| *. login.windows.net |443 |HTTPS |
| *. servicebus.windows.net |5671-5672 |Pokročilé zpráv služby Řízení front Protocol (AMQP) |
| *. servicebus.windows.net |443, 9350-9354 |Moduly pro naslouchání na předávání přes Service Bus přes TCP (vyžaduje 443 pro získání tokenu řízení přístupu) |
| *. frontend.clouddatahub.net |443 |HTTPS |
| *. core.windows.net |443 |HTTPS |
| Login.microsoftonline.com |443 |HTTPS |
| *. msftncsi.com |443 |Připojení k Internetu tootest použít, pokud brána hello nedostupná pro hello služby Power BI. |
| *.microsoftonline p.com |443 |Slouží k ověření v závislosti na konfiguraci. |

### <a name="force-https"></a>Vynucení komunikaci přes protokol HTTPS s Azure Service Bus
Hello brány toocommunicate s Azure Service Bus můžete vynutit pomocí protokolu HTTPS místo přímé TCP; ale to tak může výrazně snížit výkon. Můžete upravit hello *Microsoft.PowerBI.DataMovement.Pipeline.GatewayCore.dll.config* souboru tak, že změníte hodnotu hello z `AutoDetect` příliš`Https`. Tento soubor se obvykle nachází ve *brána dat Files\On místní C:\Program*.

```
<setting name="ServiceBusSystemConnectivityModeString" serializeAs="String">
    <value>Https</value>
</setting>
```

## <a name="faq"></a>Nejčastější dotazy

### <a name="general"></a>Obecné

**Q**: Potřebuji bránu pro zdroje dat v cloudu hello, jako je například Azure SQL Database? <br/>
**A**: Ne. Brána se připojuje pouze zdroje dat tooon místní.

**Q**: nemá hello bránu nainstalovat na stejný počítač jako zdroj dat hello hello toobe? <br/>
**A**: Ne. Brána Hello připojí toohello zdroje dat pomocí hello informace o připojení, který byl poskytnut. Vezměte v úvahu hello brány jako klientskou aplikaci v tomto smyslu. Hello právě musí brána hello schopností tooconnect toohello název serveru, který byl poskytnut, obvykle na hello stejné síti.

<a name="why-azure-work-school-account"></a>

**Q**: Proč potřebovat toouse pracovní nebo školní účet toosign v? <br/>
**A**: pouze můžete použít Azure pracovní nebo školní účet, když instalujete hello místní data gateway. Váš přihlašovací účet je uložený v klientovi, který je spravován pomocí služby Azure Active Directory (Azure AD). Obvykle účet Azure AD hlavní název uživatele (UPN) odpovídá hello e-mailovou adresu.

**Q**: uložení pověření? <br/>
**A**: hello přihlašovací údaje, které zadáte pro zdroj dat jsou zašifrovány a uloženy v hello cloudové službě Brána pro. přihlašovací údaje Hello se dešifrují v hello místní data gateway.

**Q**: existují všechny požadavky pro šířku pásma sítě? <br/>
**A**: ho má doporučujeme síti připojení má dobrou propustnost. Každé prostředí je jiné a hello množství dat odesílaných ovlivňuje hello výsledky. Pomocí ExpressRoute může pomoct tooguarantee úrovní propustnosti mezi místními a hello datových centrech Azure.
Můžete vytvořit hello nástroj třetí strany Azure rychlost testovací aplikace toohelp měřidla vaší propustnost.

**Q**: co je hello latence pro zdroj dat tooa spuštěné dotazy z brány hello? Co je nejlepší architektura hello? <br/>
**A**: tooreduce latence sítě, brána hello instalace jako zdroj dat zavřít toohello nejdříve. Pokud nainstalujete hello brány na zdroj dat skutečné hello tento blízkosti minimalizuje latenci hello zavedená. Zvažte příliš hello datových centrech. Například pokud používá službu hello západní USA datacenter a vy musíte SQL Server hostované ve virtuálním počítači Azure, svého virtuálního počítače Azure musí být v hello západní USA příliš. Tato blízkosti minimalizuje latenci a zabraňuje s nimi spojeným nákladům na hello virtuálního počítače Azure.

**Q**: jak jsou výsledky odeslána zpět toohello cloudu? <br/>
**A**: výsledky se odesílají přes hello Azure Service Bus.

**Q**: existují jakékoli brány toohello příchozí připojení z cloudu hello? <br/>
**A**: Ne. Brána Hello používá tooAzure odchozí připojení služby Service Bus.

**Q**: Co když blokovat odchozí připojení? Co dělat, je potřeba tooopen? <br/>
**A**: najdete v části hello porty a hostitelů, které hello používá bránu.

**Q**: co je skutečný služba systému Windows hello volána?<br/>
**A**: V služeb hello brána nazývá služba brány pro místní data.

**Q**: můžete hello služba Windows Brána pro spuštění pomocí účtu Azure Active Directory? <br/>
**A**: Ne. služba systému Windows Hello musí mít platný účet systému Windows. Ve výchozím nastavení spouští hello služby s hello SID služby NT SERVICE\PBIEgwService.

### <a name="high-availability"></a>Vysoká dostupnost a zotavení po havárii

**Q**: jaké možnosti jsou dostupné pro zotavení po havárii? <br/>
**A**: můžete použít hello obnovení klíče toorestore nebo přesunout bránu. Když instalujete hello brány, zadejte hello obnovovací klíč.

**Q**: co je hello výhodou hello obnovovací klíč? <br/>
**A**: hello obnovovací klíč poskytuje způsob toomigrate nebo obnovení po havárii nastavení brány.

## <a name="troubleshooting"></a>Řešení potíží

**Q**: Jak můžu zjistit, jaké dotazy jsou odesílány zdroj dat pro místní toohello? <br/>
**A**: můžete povolit trasování dotazů, což zahrnuje hello dotazy, které se odesílají. Mějte na paměti, toochange dotazu trasování zpět toohello původní hodnotu po dokončení odstraňování potíží. Trasování dotazů, které jsou zapnuté vytvoří větší protokoly.

Můžete také zobrazit nástroje, které má váš zdroj dat pro trasování dotazů. Můžete například použít rozšířených událostí nebo profileru SQL pro SQL Server a služby Analysis Services.

**Q**: kde jsou protokoly hello gateway? <br/>
**A**: viz protokoly později v tomto tématu.

### <a name="update"></a>Aktualizace toohello nejnovější verzi

Mnohé problémy můžete surface při verze brány hello stanou zastaralými. Jako vhodné obecné Ujistěte se, že používáte nejnovější verzi hello. Pokud jste neprovedli aktualizaci brány hello dobu jednoho měsíce nebo déle, můžete zvažte instalaci hello nejnovější verzi této brány hello a v tématu, pokud jste reprodukujte problém hello.

### <a name="error-failed-tooadd-user-toogroup--2147463168-pbiegwservice-performance-log-users"></a>Chyba: Nepodařilo toogroup tooadd uživatele. (-2147463168 PBIEgwService výkonu protokolu uživatelů)

Může se tato chyba, pokud se pokusíte tooinstall hello brány na řadiči domény, což není podporováno. Ujistěte se, že nasazujete hello brána na počítači, který není řadičem domény.

## <a name="logs"></a>Protokoly

Soubory protokolu jsou důležité prostředků při řešení potíží.

#### <a name="enterprise-gateway-service-logs"></a>Protokoly služby Enterprise gateway

`C:\Users\PBIEgwService\AppData\Local\Microsoft\On-premises data gateway\<yyyyymmdd>.<Number>.log`

#### <a name="configuration-logs"></a>Konfigurace protokolů

`C:\Users\<username>\AppData\Local\Microsoft\On-premises data gateway\GatewayConfigurator.log`




#### <a name="event-logs"></a>Protokoly událostí

Můžete najít hello protokoly Brána pro správu dat a PowerBIGateway pod **protokoly aplikací a služeb**.


## <a name="telemetry"></a>Telemetrie
Telemetrická data lze použít pro monitorování a řešení potíží. Ve výchozím nastavení

**tooturn na telemetrie**

1.  Zkontrolujte hello místní data brány klienta adresář v počítači hello. Obvykle je **%systemdrive%\Program Files\On místní brána dat**. Nebo můžete spustit konzolu služby a zkontrolujte tooexecutable cesta hello: vlastnost služba brány pro hello místní data.
2.  V souboru Microsoft.PowerBI.DataMovement.Pipeline.GatewayCore.dll.config hello z adresáře klienta. Změňte tootrue nastavení SendTelemetry hello.
        
    ```
        <setting name="SendTelemetry" serializeAs="String">
                    <value>true</value>
        </setting>
    ```

3.  Uložte změny a restartovat službu systému Windows hello: místní služba brány pro data.




## <a name="next-steps"></a>Další kroky
* [Správa služby Analysis Services](analysis-services-manage.md)
* [Získání dat z Azure Analysis Services](analysis-services-connect.md)
