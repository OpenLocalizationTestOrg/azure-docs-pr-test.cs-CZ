---
title: "Brána dat aaaInstall místní - Azure Logic Apps | Microsoft Docs"
description: "Než budete přistupovat ke zdrojům dat místně, nainstalujte bránu dat hello místní pro přenos dat rychlý a šifrování mezi zdrojů dat na místní a aplikacích logiky"
keywords: "přístup k datům na místní, přenos dat, šifrování, zdroje dat"
services: logic-apps
documentationcenter: 
author: jeffhollan
manager: anneta
editor: 
ms.assetid: 47e3024e-88a0-4017-8484-8f392faec89d
ms.service: logic-apps
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 07/13/2017
ms.author: LADocs; dimazaid; estfan
ms.openlocfilehash: 01386a904d856ff545f2eca8eb1b008dcdc08574
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="install-hello-on-premises-data-gateway-for-azure-logic-apps"></a>Nainstalovat bránu dat hello místní pro Azure Logic Apps

Předtím, než aplikace logiky můžete přistupovat ke zdrojům dat místní, musíte nainstalovat a nastavit hello místní data gateway. Hello brány funguje jako mostu, který poskytuje přenos rychlé dat a šifrování mezi místními systémy a aplikace logiky. Brána Hello předává data z místního zdroje na šifrované kanály prostřednictvím hello Azure Service Bus. Veškerý provoz pochází jako zabezpečené odchozí provoz z agenta brány hello. Další informace o [fungování brány dat hello](#gateway-cloud-service).

Hello brána podporuje místní zdroje dat toothese připojení:

*   BizTalk Server 2016
*   DB2  
*   Systém souborů
*   Informix
*   MQ
*   MySQL
*   Oracle Database
*   PostgreSQL
*   Aplikace serveru SAP 
*   Zpráva serveru SAP
*   SharePoint
*   SQL Server
*   Teradata

Tyto kroky ukazují, jak instalace hello toofirst místní brána dat před [nastavit připojení mezi bránou hello a aplikace logiky](./logic-apps-gateway-connection.md). Další informace o podporované konektory najdete v tématu [konektory pro Azure Logic Apps](https://docs.microsoft.com/azure/connectors/apis-list). 

Informace o tom, jak toouse hello brány s jinými službami najdete v těchto článcích:

*   [Microsoft Power BI místní brány dat](https://powerbi.microsoft.com/documentation/powerbi-gateway-onprem/)
*   [Služba Azure gateway místní dat služby Analysis Services](../analysis-services/analysis-services-gateway.md)
*   [Microsoft Flow místní brány dat](https://flow.microsoft.com/documentation/gateway-manage/)
*   [Microsoft PowerApps místní brány dat](https://powerapps.microsoft.com/tutorials/gateway-management/)

<a name="requirements"></a>
## <a name="requirements"></a>Požadavky

**Minimální**:

* Rozhraní .NET 4.5 framework
* 64bitová verze systému Windows 7 nebo Windows Server 2008 R2 (nebo novější)

**Doporučená**:

* 8 jader procesoru
* 8 GB paměti
* 64bitová verze systému Windows 2012 R2 (nebo novější)

**Je důležité zvážit**:

* Nainstalujte bránu dat místní hello pouze v místním počítači.
Hello bránu nejde nainstalovat na řadič domény.

   > [!TIP]
   > Nemáte tooinstall hello brány na hello stejného počítače jako zdroj dat. latence toominimize, můžete nainstalovat hello brány jako zavřete jako zdroj dat možné tooyour nebo na hello stejný počítač, za předpokladu, že máte oprávnění.

* Neinstalujte hello brána na počítači, který vypne, přejde toosleep nebo toohello Internet není připojit, protože v těchto případech nelze spustit hello brány. Navíc může sníží výkon brány přes bezdrátové sítě.

* Během instalace, musíte se odhlásit se [pracovní nebo školní účet](https://docs.microsoft.com/azure/active-directory/sign-up-organization) , který je spravován službou Azure Active Directory (Azure AD), nikoli účet Microsoft. 

  Máte toouse hello stejný pracovní nebo školní účet později v hello Azure portál při vytváření a přidružte prostředek brány k vaší instalace brány. Když vytvoříte hello připojení mezi logiku aplikace a hello místní zdroje dat pak vyberete tento prostředek brány. [Proč musí používat Azure AD pracovní nebo školní účet?](#why-azure-work-school-account)

  > [!TIP]
  > Pokud jste se zaregistrovali do nabídky služeb Office 365 a nedodal e-mailu samotnou práci, může vypadat přihlašovací adresa jeff@contoso.onmicrosoft.com. 

* Pokud máte existující bránu, kterou jste vytvořili pomocí Instalační program, který je starší než verze 14.16.6317.4, nelze změnit umístění vaše brána spuštěná hello nejnovější verzi Instalační služby. Můžete však použít hello nejnovější instalační program tooset novou bránu s hello umístění, které chcete místo toho.
  
  Pokud máte bránu instalační program, který je starší než verze 14.16.6317.4, ale nemáte nainstalovanou bránu ještě, můžete stáhnout a použít nejnovější verzi instalačního programu hello.

<a name="install-gateway"></a>

## <a name="install-hello-data-gateway"></a>Nainstalovat bránu dat hello

1.  [Stáhněte a spusťte instalační program brány hello v místním počítači](http://go.microsoft.com/fwlink/?LinkID=820931&clcid=0x409).

2. Přečtěte si a přijměte podmínky použití a ochrana osobních údajů příkaz hello.

3. Zadejte hello cestu v místním počítači místo, kam chcete bránu tooinstall hello.

4. Pokud budete vyzváni, přihlaste se pomocí vaší Azure pracovního nebo školního účtu, nikoli účet Microsoft.

   ![Přihlaste se pomocí Azure pracovní nebo školní účet](./media/logic-apps-gateway-install/sign-in-gateway-install.png)

5. Nyní zaregistrovat nainstalovanou bránu s hello [cloudové službě Brána pro](#gateway-cloud-service). Zvolte **registrace nové brány na tomto počítači**.

   cloudové službě Brána pro Hello šifruje a ukládá pověření ke zdroji dat a podrobnosti brány. 
   Hello služby také směrování dotazy a jejich výsledky mezi svou aplikaci logiky, hello místní data brány a zdroje dat místně.

6. Zadejte název pro instalaci brány. Vytvořit klíče pro obnovení a pak potvrďte obnovovací klíč. 

   > [!IMPORTANT] 
   > Obnovovací klíč musí obsahovat alespoň osm znaků. Zajistěte, aby si uložit a zachovat hello klíč na bezpečném místě. Musíte také tento klíč, když chcete toomigrate, obnovení nebo převzetí existující brány.

   1. Zvolte toochange hello výchozí oblast pro cloudové službě Brána pro hello a Azure Service Bus používá vaše instalace brány **změnu oblast**.

      ![Oblast změny](./media/logic-apps-gateway-install/change-region-gateway-install.png)

      Hello výchozí oblast je oblast hello přidružené klientovi Azure AD.

   2. Na další podokno hello, otevřete hello **vyberte oblast** příliš zvolte jiné oblasti.

      ![Vyberte jinou oblast](./media/logic-apps-gateway-install/select-region-gateway-install.png)

      Například je může vyberte hello stejné oblasti jako svou aplikaci logiky, nebo vyberte hello oblast nejbližší tooyour na místní datové zdroje, můžete snížit latenci. Brána prostředků a logiku aplikace může mít jiné umístění.

      > [!IMPORTANT]
      > Tato oblast nelze změnit po instalaci. Tato oblast také určuje a omezuje hello umístění, kde si můžete vytvořit hello prostředků Azure pro bránu. Při vytváření prostředku brány v Azure, tak zkontrolujte, že umístění prostředku hello odpovídá hello oblast, kterou jste vybrali v průběhu instalace brány.
      > 
      > Pokud chcete toouse v jiné oblasti pro bránu později, musíte nastavit novou bránu.

   3. Až budete připraveni, zvolte **provádí**.

7. Nyní postupujte podle těchto kroků v hello portálu Azure, abyste mohli [vytvořit prostředek služby Azure pro bránu](../logic-apps/logic-apps-gateway-connection.md). 

Další informace o [fungování brány dat hello](#gateway-cloud-service).

## <a name="migrate-restore-or-take-over-an-existing-gateway"></a>Migrace, obnovení nebo převzetí existující brány

tooperform tyto úlohy, musí mít hello obnovovací klíč, který byl zadán při instalaci brány hello.

1. Z nabídky Start v počítači, zvolte **místní brána dat**.

2. Po hello instalační program se otevře, přihlaste se pomocí hello stejné Azure pracovní nebo školní účet, který byl dříve používá tooinstall hello brány.

3. Zvolte **migrace, obnovení nebo převzetí existující brány**.

4. Zadejte název a obnovení klíč hello hello brány, kterou chcete toomigrate, obnovení nebo proveďte přes.

<a name="restart-gateway"></a>
## <a name="restart-hello-gateway"></a>Restartujte bránu hello

Hello brány se spouští jako služby systému Windows. Stejně jako ostatní služby systému Windows můžete spustit a zastavit službu hello několika způsoby. Můžete například otevřete příkazový řádek se zvýšenými oprávněními v počítači hello se spuštěným systémem hello brány a spusťte buď tyto příkazy:

* toostop hello služba, spusťte tento příkaz:
  
    `net stop PBIEgwService`

* toostart hello služba, spusťte tento příkaz:
  
    `net start PBIEgwService`

### <a name="windows-service-account"></a>Účet služby systému Windows

Brána dat Hello místní nastavení toouse `NT SERVICE\PBIEgwService` pro hello Windows služby přihlašovací údaje. Ve výchozím nastavení hello brány má pro počítač hello právo "Přihlásit jako službu" hello, kde instalujete hello brány.

> [!NOTE]
> účet služby systému Windows Hello se liší od účtu hello použité pro připojování tooon místní zdroje dat a od hello Azure pracovní nebo školní účet použít toosign v toocloud služby.

## <a name="configure-a-firewall-or-proxy"></a>Konfigurace brány firewall nebo proxy server

Hello brány vytvoří odchozí připojení příliš [Azure Service Bus](https://azure.microsoft.com/services/service-bus/). informace o proxy serveru tooprovide pro bránu, najdete v části [konfigurace nastavení proxy serveru](https://powerbi.microsoft.com/documentation/powerbi-gateway-proxy/).

toocheck zda brána firewall nebo proxy, může blokovat připojení, zkontrolujte, jestli váš počítač může připojit ve skutečnosti toohello Internetu a hello [Azure Service Bus](https://azure.microsoft.com/services/service-bus/). Z řádku prostředí PowerShell spusťte tento příkaz:

`Test-NetConnection -ComputerName watchdog.servicebus.windows.net -Port 9350`

> [!NOTE]
> Tento příkaz pouze Otestuje připojení k síti a toohello připojení k Azure Service Bus. Takže příkaz hello neobsahuje nic toodo s hello bránu nebo hello cloudové službě brány, který šifruje a ukládá přihlašovací údaje a podrobnosti brány. 
>
> Tento příkaz je taky pouze k dispozici v systému Windows Server 2012 R2 nebo novější a Windows 8.1 nebo novější. V dřívějších verzích operačního systému, můžete použít Telnet příliš test připojení. Další informace o [Azure Service Bus a hybridní řešení](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md).

Výsledky by měl vypadat podobně jako příklad toothis:

```text
ComputerName           : watchdog.servicebus.windows.net
RemoteAddress          : 70.37.104.240
RemotePort             : 5672
InterfaceAlias         : vEthernet (Broadcom NetXtreme Gigabit Ethernet - Virtual Switch)
SourceAddress          : 10.120.60.105
PingSucceeded          : False
PingReplyDetails (RTT) : 0 ms
TcpTestSucceeded       : True
```

Pokud **TcpTestSucceeded** není nastaven příliš**True**, vám může blokována bránou firewall. Pokud chcete toobe komplexní, nahraďte hello **ComputerName** a **Port** hodnoty s hodnotami hello uvedené v části [konfigurace portů](#configure-ports) v tomto tématu.

brány firewall Hello také mohou blokovat připojení této hello díky toohello datových centrech Azure Azure Service Bus. Pokud tento scénář se stane, schválit (odblokovat) všechny hello IP adres pro tyto datových center ve vašem regionu. Pro tyto IP adresy [získání hello Azure IP adresy seznamu zde](https://www.microsoft.com/download/details.aspx?id=41653).

## <a name="configure-ports"></a>Konfigurace portů

Hello brány vytvoří odchozí připojení příliš[Azure Service Bus](https://azure.microsoft.com/services/service-bus/) a komunikuje na odchozí porty: TCP 443 (výchozí), 5671, 5672, 9350 prostřednictvím 9354. Brána Hello nevyžaduje příchozí porty. Další informace o [Azure Service Bus a hybridní řešení](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md).

| NÁZVY DOMÉN | ODCHOZÍ PORTY | POPIS |
| --- | --- | --- |
| *. analysis.windows.net | 443 | HTTPS | 
| *. login.windows.net | 443 | HTTPS | 
| *. servicebus.windows.net | 5671-5672 | Pokročilé zpráv služby Řízení front Protocol (AMQP) | 
| *. servicebus.windows.net | 443, 9350-9354 | Moduly pro naslouchání na předávání přes Service Bus přes TCP (vyžaduje 443 pro získání tokenu řízení přístupu) | 
| *. frontend.clouddatahub.net | 443 | HTTPS | 
| *. core.windows.net | 443 | HTTPS | 
| Login.microsoftonline.com | 443 | HTTPS | 
| *. msftncsi.com | 443 | Připojení k Internetu tootest použít, když brána hello nedostupná pro hello služby Power BI. | 

Pokud máte tooapprove IP adresy místo hello domény, můžete stáhnout a použít hello [rozsahy IP Datacentra Azure Microsoft seznamu](https://www.microsoft.com/download/details.aspx?id=41653). V některých případech jsou vytvářeny hello Azure Service Bus připojení s IP adresou, nikoli plně kvalifikované názvy domény.

<a name="gateway-cloud-service"></a>
## <a name="how-does-hello-data-gateway-work"></a>Jak funguje hello bránu dat?

Brána dat Hello usnadňuje rychle a bezpečně komunikaci mezi svou aplikaci logiky, cloudové službě Brána pro hello a zdroje dat na místě. 

![Diagram-for-On-Premises-data-Gateway-Flow](./media/logic-apps-gateway-install/how-on-premises-data-gateway-works-flow-diagram.png)

Proto když uživatel hello v cloudu hello komunikuje element, který byl připojen tooyour místní zdroj dat:

1. cloudové službě Brána pro Hello vytvoří dotaz, společně s hello šifrovat přihlašovací údaje pro zdroj dat hello a odešle hello dotazu toohello fronty pro tooprocess hello brány.

2. cloudové službě Brána pro Hello analyzuje hello dotazu a nabízených oznámení hello požadavek toohello Azure Service Bus.

3. Brána dat místní Hello dotazuje hello Azure Service Bus pro žádosti čekající na vyřízení.

4. Brána Hello získá hello dotazu, dešifruje hello přihlašovací údaje a připojí zdroj dat toohello tyto přihlašovací údaje.

5. Brána Hello odešle datový zdroj toohello hello dotazu pro provedení.

6. výsledky Hello jsou odesílány z hello zdroj dat, back toohello brány a cloudové službě Brána pro toohello. Hello cloudové službě Brána pak použije hello výsledky.

<a name="faq"></a>
## <a name="frequently-asked-questions"></a>Nejčastější dotazy

### <a name="general"></a>Obecné

**Q**: Potřebuji bránu pro zdroje dat v cloudu hello, jako je například SQL Azure? <br/>
**A**: Ne. Brána se připojuje pouze zdroje dat tooon místní.

**Q**: nemá hello bránu nainstalovat na stejný počítač jako zdroj dat hello hello toobe? <br/>
**A**: Ne. Brána Hello připojí toohello zdroje dat pomocí hello informace o připojení, který byl poskytnut. Vezměte v úvahu hello brány jako klientskou aplikaci v tomto smyslu. bránu Hello je právě hello schopností tooconnect toohello název serveru, který byl poskytnut.

<a name="why-azure-work-school-account"></a>

**Q**: Proč musí I používáte Azure pracovní nebo školní účet toosign v? <br/>
**A**: pouze můžete použít Azure pracovní nebo školní účet, když instalujete hello místní data gateway. Váš přihlašovací účet je uložený v klientovi, který je spravován pomocí služby Azure Active Directory (Azure AD). Obvykle účet Azure AD hlavní název uživatele (UPN) odpovídá hello e-mailovou adresu.

**Q**: uložení pověření? <br/>
**A**: hello přihlašovací údaje, které zadáte pro zdroj dat jsou zašifrovány a uložené v cloudové službě Brána pro hello. přihlašovací údaje Hello se dešifrují v hello místní data gateway.

**Q**: existují všechny požadavky pro šířku pásma sítě? <br/>
**A**: doporučujeme, aby připojení k síti dobrý propustnost. Každé prostředí je jiné a hello množství dat odesílaných ovlivňuje hello výsledky. Pomocí ExpressRoute může pomoct tooguarantee úrovní propustnosti mezi místními a hello datových centrech Azure.
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
**A**: V rámci služby Services hello brána nazývá služba Power BI Enterprise Gateway.

**Q**: můžete hello služba Windows Brána pro spuštění pomocí účtu Azure Active Directory? <br/>
**A**: Ne. služba systému Windows Hello musí mít platný účet systému Windows. Ve výchozím nastavení spouští hello služby s hello SID služby NT SERVICE\PBIEgwService.

### <a name="high-availability-and-disaster-recovery"></a>Vysoká dostupnost a zotavení po havárii

**Q**: jaké možnosti jsou dostupné pro zotavení po havárii? <br/>
**A**: můžete použít hello obnovení klíče toorestore nebo přesunout bránu. Když instalujete hello brány, zadejte hello obnovovací klíč.

**Q**: co je hello výhodou hello obnovovací klíč? <br/>
**A**: hello obnovovací klíč poskytuje způsob toomigrate nebo obnovení po havárii nastavení brány.

**Q**: existují všechny plány pro povolení scénáře s vysokou dostupností s bránou hello? <br/>
**A**: tyto scénáře jsou na hello plán, ale ještě nemáme časové osy.

## <a name="troubleshooting"></a>Řešení potíží

[!INCLUDE [existing-gateway-location-changed](../../includes/logic-apps-existing-gateway-location-changed.md)]

**Q**: Jak můžu zjistit, jaké dotazy jsou odesílány zdroj dat pro místní toohello? <br/>
**A**: můžete povolit trasování dotazů, což zahrnuje hello dotazy, které se odesílají. Mějte na paměti, toochange dotazu trasování zpět toohello původní hodnotu po dokončení odstraňování potíží. Trasování dotazů, které jsou zapnuté vytvoří větší protokoly.

Můžete také zobrazit nástroje, které má váš zdroj dat pro trasování dotazů. Můžete například použít rozšířených událostí nebo profileru SQL pro SQL Server a služby Analysis Services.

**Q**: kde jsou protokoly hello gateway? <br/>
**A**: viz nástroje později v tomto tématu.

### <a name="update-toohello-latest-version"></a>Aktualizace toohello nejnovější verzi

Mnohé problémy můžete surface při verze brány hello stanou zastaralými. Jako vhodné obecné Ujistěte se, že používáte nejnovější verzi hello. Pokud jste neprovedli aktualizaci brány hello dobu jednoho měsíce nebo déle, můžete zvažte instalaci hello nejnovější verzi této brány hello a v tématu, pokud jste reprodukujte problém hello.

### <a name="error-failed-tooadd-user-toogroup--2147463168-pbiegwservice-performance-log-users"></a>Chyba: Nepodařilo toogroup tooadd uživatele. (-2147463168 PBIEgwService výkonu protokolu uživatelů)

Může se tato chyba, pokud se pokusíte tooinstall hello brány na řadiči domény, což není podporováno. Ujistěte se, že nasazujete hello brána na počítači, který není řadičem domény.

## <a name="tools"></a>Nástroje

### <a name="collect-logs-from-hello-gateway-configurer"></a>Shromažďování protokolů z brány configurer hello

Můžete shromáždit několik protokolů pro bránu hello. Vždy začínejte hello protokoly!

#### <a name="installer-logs"></a>Instalační protokoly

`%localappdata%\Temp\Power_BI_Gateway_–Enterprise.log`

#### <a name="configuration-logs"></a>Konfigurace protokolů

`%localappdata%\Microsoft\Power BI Enterprise Gateway\GatewayConfigurator.log`

#### <a name="enterprise-gateway-service-logs"></a>Protokoly služby Enterprise gateway

`C:\Users\PBIEgwService\AppData\Local\Microsoft\Power BI Enterprise Gateway\EnterpriseGateway.log`

#### <a name="event-logs"></a>Protokoly událostí

Můžete najít hello protokoly Brána pro správu dat a PowerBIGateway pod **protokoly aplikací a služeb**.

### <a name="fiddler-trace"></a>Fiddler trasování

[Fiddler](http://www.telerik.com/fiddler) je bezplatný nástroj na webu Telerik, který monitoruje provoz protokolu HTTP. Zobrazí se tyto přenosy s hello služby Power BI z hello klientský počítač. Tato služba může zobrazit chyby a další související informace.

## <a name="next-steps"></a>Další kroky
    
* [Připojení místní tooon dat z aplikace logiky](../logic-apps/logic-apps-gateway-connection.md)
* [Funkce Integrace organizace](../logic-apps/logic-apps-enterprise-integration-overview.md)
* [Konektory pro Azure Logic Apps](../connectors/apis-list.md)
