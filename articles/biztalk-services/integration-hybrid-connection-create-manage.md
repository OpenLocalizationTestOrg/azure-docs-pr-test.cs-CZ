---
title: "aaaCreate a správa hybridních připojení | Microsoft Docs"
description: "Zjistěte, jak spravovat hello připojení toocreate hybridní připojení a nainstalujte hello správce hybridního připojení. MABS, WABS"
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: erikre
editor: 
ms.assetid: aac0546b-3bae-41e0-b874-583491a395ea
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/18/2016
ms.author: ccompy
ms.openlocfilehash: 561d8f3dd97318130a05c3bb2874ee8022e7f417
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-hybrid-connections"></a>Vytvoření a správa hybridních připojení

> [!IMPORTANT]
> Hybridní připojení BizTalk jsou vyřazena z provozu a nahrazena hybridními připojeními App Service. Další informace, včetně jak toomanage vaše stávající hybridní připojení BizTalk, najdete v části [Azure App Service hybridní připojení](../app-service/app-service-hybrid-connections.md).


## <a name="overview-of-hello-steps"></a>Přehled kroků hello
1. Vytvořit hybridní připojení tak, že zadáte hello **název hostitele** nebo **plně kvalifikovaný název domény** hello místního prostředku v privátní síti.
2. Propojte webové aplikace Azure nebo mobilní aplikace Azure toohello hybridní připojení.
3. Nainstalujte na místnímu prostředku hello správce hybridního připojení a připojení toohello konkrétní hybridní připojení. Hello portál Azure poskytuje tooinstall prostředí jedním kliknutím a připojení.
4. Spravovat hybridní připojení a jejich klíče připojení.

Toto téma obsahuje tyto kroky. 

> [!IMPORTANT]
> Je možné tooset hybridní připojení IP adresa tooan koncového bodu. Pokud chcete použít IP adresu, může nebo nemusí dosáhnout hello místnímu prostředku, v závislosti na vašeho klienta. Hello hybridního připojení závisí na klienta hello provádění vyhledávání DNS. Ve většině případů hello **klienta** je kód aplikace. Pokud hello klienta nebude provádět vyhledávání DNS, (nepokouší tooresolve hello IP adresu jako by to byl platný název domény (x.x.x.x)), pak není přenosy budou odesílat prostřednictvím hello hybridní připojení.
> 
> Pro příklad (pseudokódu), můžete definovat **10.4.5.6** jako místního hostitele:
> 
> **Hello následující scénáři funguje:**  
> `Application code -> GetHostByName("10.4.5.6") -> Resolves too127.0.0.3 -> Connect("127.0.0.3") -> Hybrid Connection -> on-prem host`
> 
> **předchozí postup nebude fungovat podle scénáře Hello:**  
> `Application code -> Connect("10.4.5.6") -> ?? -> No route toohost`
> 
> 

## <a name="CreateHybridConnection"></a>Vytvořit hybridní připojení.
Hybridní připojení se dají vytvářet v hello portál Azure pomocí webových aplikací **nebo** pomocí služby BizTalk Services. 

**toocreate hybridní připojení pomocí webové aplikace**, najdete v části [tooan připojení Azure Web Apps místnímu prostředku](../app-service-web/web-sites-hybrid-connection-get-started.md). Hello hybridní připojení správce (HCM) můžete také nainstalovat z vaší webové aplikace, což je metoda hello upřednostňovaný. 

**toocreate hybridní připojení ve službě BizTalk Services**:

1. Přihlaste se toohello [portál Azure classic](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. V levém navigačním podokně hello vyberte **BizTalk Services** a potom vyberte svoji službu BizTalk. 
   
    Pokud nemáte existující službu BizTalk, můžete [vytvoření služby BizTalk](biztalk-provision-services.md).
3. Vyberte hello **hybridní připojení** karty:  
   ![Karta hybridní připojení][HybridConnectionTab]
4. Vyberte **vytvořit hybridní připojení** nebo vyberte hello **přidat** tlačítka na hlavním panelu hello. Zadejte hello následující:
   
   | Vlastnost | Popis |
   | --- | --- |
   | Name (Název) |Hello hybridní připojení. název musí být jedinečné a nesmí být hello stejný název jako hello služby BizTalk. Můžete zadat libovolný název, ale buďte konkrétní s jeho účel. Příklady obsahují:<br/><br/>Mzdy*SQLServer*<br/>SupplyList*SharepointServer*<br/>Zákazníci*OracleServer* |
   | Název hostitele |Zadejte hello plně kvalifikovaný název hostitele, pouze název hostitele hello nebo hello IPv4 adresu hello místnímu prostředku. Příklady obsahují:<br/><br/>Můjsqlserver<br/>*Můjsqlserver*. *Domény*. corp.*společnost*.com<br/>*myHTTPSharePointServer*<br/>*myHTTPSharePointServer*. *Společnost*.com<br/>10.100.10.10<br/><br/>Pokud používáte hello IPv4 adresu, Všimněte si, že kódu klienta nebo aplikace nemusí vyřešit hello IP adresu. V tématu hello důležité Poznámka hello horní části tohoto tématu. |
   | Port |Zadejte číslo portu hello hello místnímu prostředku. Například pokud používáte webové aplikace, zadejte port 80 nebo 443. Pokud používáte systém SQL Server, zadejte port 1433. |
5. Vyberte instalační program hello zaškrtnutí toocomplete hello. 

#### <a name="additional"></a>Další
* Můžete vytvořit více hybridní připojení. V tématu hello [BizTalk Services: Tabulka edic](biztalk-editions-feature-chart.md) hello počet povolených připojení. 
* Každé hybridní připojení se vytvoří dvojice připojovacích řetězců: aplikace klíče tohoto SEND a místní klíče, které NASLOUCHAT. Každý pár má primární a sekundární klíč. 

## <a name="LinkWebSite"></a>Propojte mobilní aplikace nebo webové aplikace Azure App Service
toolink webovou aplikaci nebo mobilní aplikace v Azure App Service tooan stávající hybridní připojení, vyberte **použít stávající hybridní připojení** v okně hello hybridní připojení. V tématu [přístup k místním prostředkům v Azure App Service pomocí hybridních připojení](../app-service-web/web-sites-hybrid-connection-get-started.md).

## <a name="InstallHCM"></a>Nainstalujte hello správce hybridního připojení na místě
Po vytvoření hybridní připojení, nainstalujte na hello místnímu prostředku hello správce hybridního připojení. Ho můžete stáhnout z vaší webové aplikace Azure nebo z vaší služby BizTalk. BizTalk Services kroky: 

1. Přihlaste se toohello [portál Azure classic](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. V levém navigačním podokně hello vyberte **BizTalk Services** a potom vyberte svoji službu BizTalk. 
3. Vyberte hello **hybridní připojení** karty:  
   ![Karta hybridní připojení][HybridConnectionTab]
4. Hello hlavním panelu vyberte **místní instalace**:  
   ![Místní instalace][HCOnPremSetup]
5. Vyberte **instalace a konfigurace** toorun nebo stažení hello správce hybridního připojení na hello lokálního systému. 
6. Vyberte hello zaškrtnutí toostart hello instalace. 

<!--
You can also download hello Hybrid Connection Manager MSI file and copy hello file tooyour on-premises resource. Specific steps:

1. Copy hello on-premises primary Connection String. See [Manage Hybrid Connections](#ManageHybridConnection) in this topic for hello specific steps.
2. Download hello Hybrid Connection Manager MSI file. 
3. On hello on-premises resource, install hello Hybrid Connection Manager from hello MSI file. 
4. Using Windows PowerShell, type: 
> Add-HybridConnection -ConnectionString “*Your On-Premises Connection String that you copied*” 
--> 

#### <a name="additional"></a>Další
* Správce hybridního připojení lze nainstalovat na hello následující operační systémy:
  
  * Windows Server 2008 R2 (rozhraní .NET Framework 4.5 + a Windows Management Framework 4.0 + požadované)
  * Windows Server 2012 (Windows Management Framework 4.0 + požadované)
  * Windows Server 2012 R2
* Po instalaci hello správce hybridního připojení dojde k následujícímu hello: 
  
  * Hello hybridní připojení, které jsou hostované v Azure je automaticky nakonfigurované toouse hello primární aplikace připojovací řetězec. 
  * Hello místní prostředek je automaticky nakonfigurované toouse hello primární připojovací řetězec místní.
* Hello správce hybridního připojení musí používat platný místní připojovací řetězec pro ověřování. Hello Azure Web Apps nebo Mobile Apps musí platnou aplikaci připojovací řetězec pro autorizaci použít.
* Hybridní připojení je možné škálovat nainstalováním jiná instance hello správce hybridního připojení na jiném serveru. Nakonfigurujte hello místní naslouchací proces toouse hello stejné adresy jako první místní naslouchání hello. V takovém případě je provoz hello náhodně distribuované (kruhové dotazování) mezi naslouchací procesy active místní hello. 

## <a name="ManageHybridConnection"></a>Správa hybridních připojení
toomanage hybridní připojení, můžete:

* Použít hello portál Azure a přejít tooyour služby BizTalk. 
* Použití [rozhraní REST API](http://msdn.microsoft.com/library/azure/dn232347.aspx).

#### <a name="copyregenerate-hello-hybrid-connection-strings"></a>Kopírování nebo znovu vygenerovat hello hybridní připojovací řetězce
1. Přihlaste se toohello [portál Azure classic](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. V levém navigačním podokně hello vyberte **BizTalk Services** a potom vyberte svoji službu BizTalk. 
3. Vyberte hello **hybridní připojení** karty:  
   ![Karta hybridní připojení][HybridConnectionTab]
4. Vyberte hello hybridní připojení. Hello hlavním panelu vyberte **spravovat připojení**:  
   ![Spravovat možnosti][HCManageConnection]
   
    **Správa připojení** seznamy hello aplikace a místní připojovací řetězce. Můžete kopírovat hello připojovací řetězce nebo znovu vygenerovat přístupový klíč se používá v připojovacím řetězci hello hello. 
   
    **Pokud vyberete znovu vygenerovat**, hello sdílený přístupový klíč používat v rámci hello je změnit připojovací řetězec. Hello následující:
   
   * V hello portál Azure classic, vyberte **synchronizace klíčů** v hello aplikaci Azure.
   * Znovu spustit hello **místní instalace**. Když znovu spustíte hello místní instalace hello místnímu prostředku, je automaticky nakonfigurovat toouse hello aktualizovat primární připojovací řetězec.

#### <a name="use-group-policy-toocontrol-hello-on-premises-resources-used-by-a-hybrid-connection"></a>Použití skupiny zásad toocontrol hello místní prostředky využívané třídou hybridní připojení
1. Stáhnout hello [šablon správy Správce hybridního připojení](http://www.microsoft.com/download/details.aspx?id=42963).
2. Extrahujte soubory hello.
3. Na počítači hello, který upravuje zásad skupiny hello následující:  
   
   * Zkopírujte hello. ADMX soubory toohello *%WINROOT%\PolicyDefinitions* složky.
   * Zkopírujte hello. ADML soubory toohello *%WINROOT%\PolicyDefinitions\en-us* složky.

Po zkopírování, můžete použít zásady hello Editor zásad skupiny toochange.

## <a name="next"></a>Další
[Připojení Azure Web Apps tooan místnímu prostředku](../app-service-web/web-sites-hybrid-connection-get-started.md)  
[Připojit tooon místní SQL Server z Azure Web Apps](../app-service-web/web-sites-hybrid-connection-connect-on-premises-sql-server.md)   
[Přehled hybridních připojení](integration-hybrid-connection-overview.md)

## <a name="see-also"></a>Viz také
[REST API pro správu služby BizTalk Services v Microsoft Azure](http://msdn.microsoft.com/library/azure/dn232347.aspx)  
[BizTalk Services: Tabulka edic](biztalk-editions-feature-chart.md)  
[Vytvoření služby BizTalk pomocí portálu Azure classic](biztalk-provision-services.md)  
[BizTalk Services: Karty Řídicí panel, Sledování a Škálování](biztalk-dashboard-monitor-scale-tabs.md)

[HybridConnectionTab]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionTab.png
[HCOnPremSetup]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionOnPremSetup.png
[HCManageConnection]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionManageConn.png 
