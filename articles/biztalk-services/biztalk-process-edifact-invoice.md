---
title: "Kurz: Zpracovat EDIFACT faktury pomocí služby Azure BizTalk Services | Microsoft Docs"
description: "Jak toocreate a nakonfigurovat konektor pole nebo rozhraní API aplikace hello a použít ho v aplikaci logiky ve službě Azure App Service"
services: biztalk-services
documentationcenter: .net,nodejs,java
author: msftman
manager: erikre
editor: 
ms.assetid: 7e0b93fa-3e2b-4a9c-89ef-abf1d3aa8fa9
ms.service: biztalk-services
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 05/31/2016
ms.author: deonhe
ms.openlocfilehash: 4ca021c9084b9b6540c93ef15fc3d44be0966970
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-process-edifact-invoices-using-azure-biztalk-services"></a>Kurz: Proces EDIFACT faktury pomocí služby Azure BizTalk Services

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

Můžete použít hello portál služeb BizTalk tooconfigure a nasadit X12 a EDIFACT smlouvy. V tomto kurzu podíváme na tom, jak toocreate smlouvy EDIFACT pro výměnu faktury mezi obchodními partnery. Tento kurz je napsán kolem začátku do konce obchodní řešení zahrnující dva obchodní partneři Northwind a Contoso, které exchange EDIFACT zprávy.  

## <a name="sample-based-on-this-tutorial"></a>Ukázka podle tohoto kurzu
Tento kurz je napsán kolem ukázku, **odesílání EDIFACT faktury pomocí BizTalk Services**, která je k dispozici toodownload z hello [galerie kódů MSDN](http://go.microsoft.com/fwlink/?LinkId=401005). Můžete použít ukázkové hello a projít tento kurz toounderstand, jak byl sestaven ukázka hello. Nebo můžete použít tento kurz toocreate základů vlastní řešení. V tomto kurzu je zaměřeno hello druhý přístup tak, aby vám pochopit, jak byl sestaven toto řešení. Také co nejvíce hello kurzu je konzistentní s ukázkou hello a používá hello stejné názvy artefakty (například schémat, transformací) jako použité v ukázce hello.  

> [!NOTE]
> Protože toto řešení zahrnuje odesílání zprávy ze EAI most tooan EDI mostu, se znovu použije hello [most služby BizTalk řetězení ukázka](http://code.msdn.microsoft.com/BizTalk-Bridge-chaining-2246b104) ukázka.  
> 
> 

## <a name="what-does-hello-solution-do"></a>Jakým způsobem hello řešení?
V tomto řešení Northwind obdrží EDIFACT faktury ze společnosti Contoso. Tyto faktury nejsou ve standardním formátu EDI. Ano před odesláním tooNorthwind hello faktury, musí být transformovaných tooan EDIFACT faktura (také nazývané INVOIC) dokumentu. Na přijetí musíte Northwind zpracovat hello EDIFACT faktury a vrátí tooContoso řízení zpráv (také nazývané CONTRL).

![][1]  

tooachieve v tomto scénáři obchodní Contoso používá hello funkce poskytované službou Microsoft Azure BizTalk Services.

* Společnost Contoso využívá EAI mostů tootransform hello původní faktury tooEDIFACT INVOIC.
* Most EAI Hello pak odešle tooan zpráva hello EDI odeslat most? víc informací, které jsou nasazeny jako součást smlouvu nakonfigurované v hello portál služby BizTalk.
* Hello EDI odesílání most zpracovává hello EDIFACT INVOIC a nasměruje ho tooNorthwind.
* Po přijetí hello faktury, vrátí Northwind toohello zprávy CONTRL EDI přijímat most? víc informací, které jsou nasazeny jako součást hello smlouvy.  

> [!NOTE]
> Volitelně můžete toto řešení také ukazuje, jak toouse dávkování toosend hello faktury v dávkách, místo abyste odesílali každé faktury samostatně.  
> 
> 

toocomplete hello scénáři jsme používat fronty Service Bus toosend faktura z Contoso tooNorthwind nebo dostávat Northwind potvrzení. Tyto fronty si můžete vytvořit pomocí klientské aplikace, která je k dispozici ke stažení a je součástí balíčku hello ukázka, která je k dispozici v rámci tohoto kurzu.  

## <a name="prerequisites"></a>Požadavky
* Musí mít obor názvů sběrnice. Pokyny pro vytvoření oboru názvů, naleznete v části [postupy: vytvoření nebo úprava Namespace služby Service Bus](https://msdn.microsoft.com/library/azure/hh674478.aspx). Předpokládejme, že už máte obor názvů sběrnice zřízený, nazývá **edifactbts**.
* Musíte mít předplatné služby BizTalk Services. Pokyny najdete v tématu [vytvoření služby BizTalk pomocí portálu Azure classic](http://go.microsoft.com/fwlink/?LinkID=302280). V tomto kurzu, dejte nám předpokládá, že máte předplatné služby BizTalk, názvem **contosowabs**.
* Zaregistrujte své předplatné služby BizTalk Services na hello portál služby BizTalk. Pokyny najdete v tématu [registrace nasazení služby BizTalk v hello portál služby BizTalk](https://msdn.microsoft.com/library/hh689837.aspx)
* Musíte mít nainstalovanou sadu Visual Studio.
* Musíte mít nainstalované sady SDK služby BizTalk. Můžete si stáhnout hello SDK z [http://go.microsoft.com/fwlink/?LinkId=235057](http://go.microsoft.com/fwlink/?LinkId=235057)  

## <a name="step-1-create-hello-service-bus-queues"></a>Krok 1: Vytvoření hello fronty Service Bus
Toto řešení používá zprávy tooexchange fronty Service Bus mezi obchodními partnery. Contoso a Northwind odesílat zprávy fronty toohello z kde mosty EAI a EDI hello je můžou využívat. Pro toto řešení je třeba tři fronty Service Bus:

* **northwindreceive** – Northwind přijme hello faktury z Contoso přes tuto frontu.
* **contosoreceive** – Contoso přijme hello potvrzení z Northwind přes tuto frontu.
* **pozastaveno** – všechny pozastavené zprávy jsou směrované toothis fronty. Zprávy jsou pozastavené, pokud se nepodaří během zpracování.

Tyto fronty Service Bus můžete vytvořit pomocí klientské aplikace součástí balíčku ukázka hello.  

1. Hello umístění, kam jste stáhli ukázkový text hello, otevřete **kurzu odesílání faktury pomocí BizTalk Services EDI Bridges.sln**.
2. Stiskněte klávesu **F5** toobuild a spusťte hello **kurzu klienta** aplikace.
3. Úvodní obrazovka zadejte obor názvů Service Bus ACS hello, název vystavitele a klíč vystavitele.
   
   ![][2]  
4. Okno se zprávou vyzve, aby tři fronty budou vytvořeny v oboru názvů sběrnice. Klikněte na **OK**.
5. Ponechejte hello kurzu klienta spuštěna. Otevřete hello, klikněte na **Service Bus** > ***vašeho oboru názvů Service Bus*** > **fronty**a ověřte, že jsou vytvořeny hello tři fronty.  

## <a name="step-2-create-and-deploy-trading-partner-agreement"></a>Krok 2: Vytvoření a nasazení smlouvy obchodním partnerem
Vytvoření smlouvy s obchodním partnerem mezi Contoso a Northwind. Smlouvy s obchodním partnerem definuje kontrakt obchodu mezi obchodními partnery hello dva, například které toouse schématu zprávy, které zasílání zpráv protokolu toouse atd. Smlouvy s obchodním partnerem obsahuje dva mosty EDI, jeden toosend zprávy tootrading partnery (volat hello **EDI odeslat most**) a zprávy tooreceive jeden z obchodních partnerů (názvem hello **EDI přijímat most**).

V kontextu hello tohoto řešení hello EDI odesílání most odpovídá straně odeslání toohello hello smlouvy a je použité toosend hello EDIFACT faktury z Contoso tooNorthwind. Podobně hello EDI zobrazí most odpovídá toohello škálování na straně hello smlouvy a je použité tooreceive potvrzení z Northwind.  

### <a name="create-hello-trading-partners"></a>Vytvoření hello obchodních partnerů
toostart, vytvořit obchodní partneři společnosti Contoso a Northwind.  

1. V hello portál služby BizTalk, na hello **partnery** , klikněte na **přidat**.
2. V hello nová stránka partnera, zadejte **Contoso** jako název partnera a pak klikněte na tlačítko **Uložit**.
3. Opakování hello krok toocreate hello druhý partner **Northwind**.  

### <a name="create-hello-agreement"></a>Vytvoření smlouvy hello
Smluv s obchodními partnery se vytvoří mezi profily obchodní obchodních partnerů. Toto řešení používá hello výchozí partnera profily, které se automaticky vytvoří, když jsme vytvořili hello partnery.  

1. V hello portál služby BizTalk, klikněte na **smlouvy** > **přidat**.
2. Hello nové smlouvy **obecné nastavení** stránky, zadejte hodnoty hello, jak ukazuje následující obrázek hello a pak klikněte na tlačítko **pokračovat**.
   
   ![][3]  
   
   Po kliknutí na tlačítko **pokračovat**, dvě karty **přijímat nastavení** a **odeslat nastavení** k dispozici.
3. Vytvořte hello odesílání smlouvu mezi společností Contoso a Northwind. Tato smlouva řídí, jak Contoso odešle tooNorthwind faktury EDIFACT hello.
   
   1. Klikněte na tlačítko **odeslat nastavení**.
   2. Zachovat hello výchozí hodnoty na hello **příchozí adresy URL**, **transformace**, a **Batching** karty.
   3. Na hello **protokol** karta, v části hello **schémata** části tím, že nahrajete hello **EFACT_D93A_INVOIC.xsd** schématu. Toto schéma je k dispozici s balíčkem ukázka hello.
      
      ![][4]  
   4. Na hello **přenosu** kartě, zadejte podrobnosti hello hello front služby Service Bus. Pro hello straně odesílání smlouvu, používáme hello **northwindreceive** fronty toosend hello EDIFACT faktury tooNorthwind a hello **pozastaveno** všechny zprávy, které nesplní během zpracování a jsou ve frontě tooroute pozastaveno. Můžete vytvořit tyto fronty v **krok 1: vytvoření fronty Service Bus hello** (v tomto tématu).
      
      ![][5]  
      
      V části **přenosu nastavení > typ přenosu** a **nastavení pozastavení zpráv > typ přenosu**vyberte Azure Service Bus a zadejte hodnoty hello, jak je znázorněno v bitové kopii hello.
4. Vytvoření hello přijímat smlouvu mezi společností Contoso a Northwind. Tato smlouva řídí, jak Contoso přijímá hello potvrzení z Northwind.
   
   1. Klikněte na tlačítko **obdrží nastavení**.
   2. Zachovat hello výchozí hodnoty na hello **přenosu** a **transformace** karty.
   3. Na hello **protokol** karta, v části hello **schémata** části tím, že nahrajete hello **EFACT_4.1_CONTRL.xsd** schématu. Toto schéma je k dispozici s balíčkem ukázka hello.
   4. Na hello **trasy** kartě, vytvořte tooensure filtr, který potvrzení z Northwind jsou směrované tooContoso pouze. V části **trasy nastavení**, klikněte na tlačítko **přidat** toocreate hello směrování filtru.
      
      ![][6]  
      
      1. Zadejte hodnoty pro **název pravidla**, **trasy pravidlo**, a **trasy cílové** viz obrázek hello.
      2. Klikněte na **Uložit**.
   5. Na hello **trasy** kartě znovu, zadejte, kde jsou směrovány pozastavenou potvrzení (potvrzení, které nesplní během zpracování). Nastavit hello přenosu typu tooAzure Service Bus, směrovat cílový typ příliš**fronty**, ověřování, zadejte příliš**sdíleného přístupového podpisu** (SAS), zadejte hello SAS připojovací řetězec pro hello Service Bus obor názvů a pak zadejte název fronty hello jako **pozastaveno**.
5. Nakonec klikněte na **nasadit** toodeploy hello smlouvy. Poznámka: hello koncových bodů, kde hello odesílat a přijímat smlouvy nasadí.
   
   * Na hello **odeslat nastavení** v části **příchozí adresy URL**, Všimněte si hello koncový bod. toosend zprávu od společnosti Contoso tooNorthwind pomocí hello EDI odeslat most, je nutné odeslat toothis koncový bod zprávy.
   * Na hello **přijímat nastavení** v části **přenosu**, Všimněte si hello koncový bod. toosend zprávu od Northwind tooContoso pomocí hello EDI přijímat most, je nutné odeslat toothis koncový bod zprávy.  

## <a name="step-3-create-and-deploy-hello-biztalk-services-project"></a>Krok 3: Vytvoření a nasazení projektu služby BizTalk Services hello
V předchozím kroku hello jste nasadili hello EDI odeslání a přijetí smlouvy tooprocess EDIFACT faktury a potvrzení. Tyto smlouvy můžou jenom zpracování zpráv odpovídajících směrnicím toohello standardní EDIFACT zpráva schématu. Však za hello scénář pro toto řešení, odešle společnosti Contoso tooNorthwind faktury ve schématu vlastní interní. Ano před toohello EDI odeslat most je odeslána zpráva hello, se musí transformaci od hello interní schématu toohello standardní EDIFACT faktury schématu. BizTalk Services EAI projektu Hello nemá který.

projektu služby BizTalk Services Hello **InvoiceProcessingBridge**, že transformací hello zprávy je také součástí hello ukázka jste si stáhli. projekt Hello zahrnuje následující artefakty hello:

* **INHOUSEINVOICE. XSD** – schéma hello interní faktury, která je odeslána tooNorthwind.
* **EFACT_D93A_INVOIC. XSD** – schéma hello standardní EDIFACT faktury.
* **EFACT_4.1_CONTRL. XSD** – schéma hello EDIFACT potvrzení, že odešle Northwind tooContoso.
* **INHOUSEINVOICE_to_D93AINVOIC. TRFM** – hello transformace, která mapuje hello interní faktury schématu toohello standardní EDIFACT faktury schématu.  

### <a name="create-hello-biztalk-services-project"></a>Vytvoření projektu služby BizTalk Services hello
1. V hello řešení sady Visual Studio, rozbalte hello InvoiceProcessingBridge projektu a pak otevřete hello **MessageFlowItinerary.bcs** souboru.
2. Klikněte kamkoli na plátně hello a nastavte hello **adresy URL služby BizTalk** hello vlastnost pole toospecify předplatné název vaší služby BizTalk Services. Například, `https://contosowabs.biztalk.windows.net`.
   
   ![][7]  
3. Z nástrojů hello přetáhněte **Xml One-Way most** toohello plátno. Sada hello **název Entity** a **relativní adresu** vlastnosti hello přemostění příliš**ProcessInvoiceBridge**. Klikněte dvakrát na **ProcessInvoiceBridge** tooopen hello most konfigurace prostor.
4. V rámci hello **typy zpráv** pole, klikněte na tlačítko hello plus (**+**) tlačítko toospecify hello schéma hello příchozí zprávy. Protože hello příchozí zprávy pro hello EAI most jsou vždycky hello interní faktury, nastavte příliš**INHOUSEINVOICE**.
   
   ![][8]  
5. Klikněte na tlačítko hello **transformace Xml** tvaru a v hello vlastnosti pro hello **mapy** vlastnosti, klikněte na tlačítko se třemi tečkami hello (**...** ) tlačítko. V hello **mapy výběr** dialogové okno, vyberte hello **INHOUSEINVOICE_to_D93AINVOIC** transformační soubor a potom klikněte na **OK**.
   
   ![][9]  
6. Přejděte zpět příliš**MessageFlowItinerary.bcs**a z nástrojů hello přetáhněte **obousměrný externí koncový bod služby** toohello napravo od hello **ProcessInvoiceBridge**. Nastavte její **název Entity** vlastnost příliš**EDIBridge**.
7. V Průzkumníku řešení hello, rozbalte položku hello **MessageFlowItinerary.bcs** a dvakrát klikněte na hello **EDIBridge.config** souboru. Nahraďte obsah hello hello **EDIBridge.config** s následující hello.
   
   > [!NOTE]
   > Proč musím souboru .config hello tooedit? koncový bod Hello externí služby, jsme přidali plátna návrháře toohello most představuje mostů hello EDI, které jsme dříve nasadili. EDI mostů jsou obousměrný mostů s odesílání a na straně příjmu. Hello most EAI, že jsme přidali toohello most designer je však jednosměrný most. Ano toohandle hello jiná zpráva exchange vzorce dva mosty hello, používáme vlastní most chování včetně jeho konfiguraci v souboru .config hello. Kromě toho vlastní chování hello také obstará hello toohello EDI odesílání most koncový bod ověřování. Toto vlastní chování je k dispozici jako samostatné ukázka v [most služby BizTalk řetězení ukázka - EAI tooEDI](http://code.msdn.microsoft.com/BizTalk-Bridge-chaining-2246b104). Toto řešení opětovně používá ukázka hello.  
   > 
   > 
   
   ```
   <?xml version="1.0" encoding="utf-8"?>
   <configuration>
     <system.serviceModel>
       <extensions>
         <behaviorExtensions>
           <add name="BridgeAuthentication" 
                 type="Microsoft.BizTalk.Bridge.Behaviour.BridgeBehaviorElement, Microsoft.BizTalk.Bridge.Behaviour, Version=1.0.0.0, Culture=neutral, PublicKeyToken=ae58f69b69495c05" />
         </behaviorExtensions>
       </extensions>
       <behaviors>
         <endpointBehaviors>
           <behavior name="BridgeAuthenticationConfiguration">
             <!-- Enter hello ACS namespace, issuer name and issuer secret of hello BizTalk Services deployment -->
             <BridgeAuthentication acsnamespace="[YOUR ACS NAMESPACE]" 
                                   issuername="owner" 
                                   issuersecret="[YOUR ACS SECRET]" />
             <webHttp />
           </behavior>
         </endpointBehaviors>
       </behaviors>
       <bindings>
         <webHttpBinding>
           <binding name="BridgeBindingConfiguration">
             <security mode="Transport" />
           </binding>
         </webHttpBinding>
       </bindings>
       <client>
         <clear />
         <!--
           Go BizTalk Portal > Agreement > Send Settings > Inbound URL
           Copy hello Endpoint URL and paste it in hello below address field
         -->
         <endpoint name="TwoWayExternalServiceEndpointReference1" 
                   address="[YOUR EDI BRIDGE SEND URI]" 
                   behaviorConfiguration="BridgeAuthenticationConfiguration" 
                   binding="webHttpBinding" 
                   bindingConfiguration="BridgeBindingConfiguration" 
                   contract="System.ServiceModel.Routing.IRequestReplyRouter" />
       </client>
     </system.serviceModel>
   </configuration>
   
   ```
8. Aktualizovat podrobnosti konfigurace tooinclude souboru EDIBridge.config hello
   
   * V části  *<behaviors>* , poskytovat hello oboru názvů služby ACS a klíč přidružený hello předplatného služby BizTalk Services.
   * V části  *<client>* , poskytovat hello koncový bod, kde je nasazená hello EDI odeslání smlouvy.
   
   Uložte změny a zavřete hello konfigurační soubor.
9. Z hello sada nástrojů, klikněte na tlačítko hello **konektor** a připojení k hello **ProcessInvoiceBridge** a **EDIBridge** součásti. Vyberte konektor hello a v poli vlastnosti, nastavte **podmínku filtrování** příliš**všechny shody**. To zajistí, že všechny zprávy, které jsou zpracovávány hello EAI most směrují toohello EDI most.
   
   ![][10]  
10. Uložte změny toohello řešení.  

### <a name="deploy-hello-project"></a>Nasazení projektu hello
1. Na počítači hello, kde jste vytvořili projektu služby BizTalk Services hello stáhněte a nainstalujte certifikát SSL hello pro vaše předplatné služby BizTalk Services. Klikněte v části služby BizTalk Services na **řídicí panel**a potom klikněte na **stáhnout certifikát SSL**. Dvakrát klikněte na certifikát hello a postupujte podle hello výzva toocomplete hello instalace. Zajistěte, aby certifikát hello instalujete **důvěryhodné kořenové certifikační autority** úložiště certifikátů.
2. V Průzkumníku řešení Visual Studio, klikněte pravým tlačítkem na hello **InvoiceProcessingBridge** projektu a pak klikněte na **nasadit**.
3. Zadejte hodnoty hello, jak je znázorněno v hello image a pak klikněte na **nasadit**. Kliknutím můžete získat přihlašovací údaje hello ACS služby BizTalk Services **informace o připojení** z řídicího panelu služby BizTalk Services hello.
   
   ![][11]  
   
   V podokně výstup hello, zkopírujte hello koncový bod, kde hello EAI most je nasazen, například `https://contosowabs.biztalk.windows.net/default/ProcessInvoiceBridge`. Tato adresa URL koncového bodu budete potřebovat později.  

## <a name="step-4-test-hello-solution"></a>Krok 4: Test hello řešení
V tomto tématu se podíváme na tom, jak tootest hello řešení pomocí hello **kurzu klienta** zadaný jako součást hello ukázkové aplikace.  

1. V sadě Visual Studio, stiskněte klávesu F5 toostart hello **kurzu klienta**.
2. úvodní obrazovka musí mít hodnoty hello naplněnými z kroku hello, které jsme vytvořili hello fronty Service Bus. Klikněte na **Další**.
3. V dalším intervalu hello, zadejte přihlašovací údaje služby ACS pro předplatné služby BizTalk Services a hello koncové body kde EAI a EDI (přijímat) jsou nasazeny mostů.
   
   Hello EAI most endpoint měl zkopírovali v předchozím kroku hello. Pro EDI neobdrží most endpoint hello portál služby BizTalk, přejděte toohello smlouvy > přijímat Nastavení > přenosu > koncový bod.
   
   ![][12]  
4. V dalším okně hello pod Contoso, klikněte na hello **odeslat interní faktury** tlačítko. V hello soubor otevřete dialogové okno, otevřete soubor INHOUSEINVOICE.txt hello. Zkontrolujte obsah hello hello souboru a pak klikněte na tlačítko **OK** toosend hello faktury.
   
   ![][13]  
5. V pár sekund hello faktury přijme Northwind. Klikněte na tlačítko hello **zobrazení zpráv** odkaz toosee hello faktury přijatých Northwind. Všimněte si, jak je hello faktury přijatých Northwind ve standardní schématu EDIFACT při hello jeden poslal společnosti Contoso se interní schématu.
   
   ![][14]  
6. Vyberte hello faktury a pak klikněte na tlačítko **odeslat potvrzení**. V poli hello dialogové okno, která se objeví Všimněte si, že hello výměnu ID je stejný v potvrzení pro faktury a hello přijaté hello je odesílána. Klikněte na tlačítko OK v hello **odeslat potvrzení** dialogové okno.
   
   ![][15]  
7. Během pár sekund je úspěšně přijato potvrzení hello společnosti Contoso.
   
   ![][16]  

## <a name="step-5-optional-send-edifact-invoice-in-batches"></a>Krok 5 (volitelné): faktury odeslat EDIFACT v dávkách
BizTalk Services EDI mostů podporuje také dávkování odchozích zpráv. Tato funkce je užitečná pro příjem partnery, které dávají přednost tooreceive dávku zpráv (splňující určité kritérium) namísto jednotlivé zprávy.

Nejdůležitější aspekt Hello při práci s dávky je hello skutečné verzi hello batch, označované taky jako hello verze kritéria. kritéria verze Hello může být založen na jak hello přijímající partner chce tooreceive zprávy. Pokud je povoleno dávkování, hello EDI most neodešle hello odchozí zprávy toohello přijetí partnera, dokud hello verze je splněna kritéria. Například dávkování kritéria na základě odešle zprávu velikost dávky pouze tehdy, když n zprávy jsou zpracovat v dávce. Kritéria batch může být také založené na čase, tak, aby dávky je odeslána v pevný čas každý den. V tomto řešení jsme zkuste hello velikost zprávy na základě kritérií.

1. V hello portál služby BizTalk klikněte na tlačítko hello smlouvy, kterou jste vytvořili dříve. Klikněte na tlačítko Nastavení odesílání > dávkování > Přidat Batch.
2. Název listu, zadejte **InvoiceBatch**, zadejte popis a pak klikněte na tlačítko **Další**.
3. Zadejte kritéria, dávky, která definuje, které zprávy musí být zpracovat v dávce. V tomto řešení jsme dávky všechny zprávy. Ano, vyberte hello použití rozšířené definice možnost a zadejte **1 = 1**. Toto je stav, který bude mít vždy hodnotu true, a proto všechny zprávy se zpracovat v dávce. Klikněte na **Další**.
   
   ![][17]  
4. Zadejte kritéria, verze batch. Hello rozevíracím, vyberte **MessageCountBased**a pro **počet**, zadejte **3**. To znamená, že dávku zpráv tři odešle tooNorthwind. Klikněte na **Další**.
   
   ![][18]  
5. Zkontrolujte souhrn hello a pak klikněte na **Uložit**. Klikněte na tlačítko **nasadit** tooredeploy hello smlouvy.
6. Přejděte zpět toohello **kurzu klienta**, klikněte na tlačítko **odeslat interní faktury**a postupujte podle pokynů hello výzvy toosend hello faktury. Si všimnete, že žádné faktury je obdrželi v Northwind, protože velikost dávky hello není splněná. Tento krok opakujte dvakrát, takže budete mít tři faktury zprávy odeslané tooNorthwind. To splňuje hello batch kritéria verze 3 zprávy a měli byste vidět faktury v Northwind.

<!--Image references-->
[1]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-1.PNG  
[2]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-2.PNG  
[3]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-3.PNG
[4]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-4.PNG  
[5]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-5.PNG  
[6]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-6.PNG  
[7]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-7.PNG  
[8]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-8.PNG
[9]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-9.PNG  
[10]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-10.PNG  
[11]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-11.PNG  
[12]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-12.PNG  
[13]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-13.PNG
[14]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-14.PNG  
[15]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-15.PNG  
[16]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-16.PNG  
[17]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-17.PNG  
[18]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-18.PNG

