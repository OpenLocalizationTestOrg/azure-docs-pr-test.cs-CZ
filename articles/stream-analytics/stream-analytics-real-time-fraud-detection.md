# <a name="get-started-using-azure-stream-analytics-real-time-fraud-detection"></a>Začněte používat Azure Stream Analytics: odhalování podvodů v reálném čase

V tomto kurzu poskytuje ilustraci začátku do konce jak toouse Azure Stream Analytics. Získáte informace o těchto tématech: 

* Přepněte do instance služby Azure Event Hubs vysílání datových proudů. V tomto kurzu budete používat aplikaci, která poskytujeme která simuluje proud záznamů metadat mobilního telefonu.

* Zápis SQL jako Stream Analytics dotazy tootransform dat, agregace informací o nebo hledáte vzory. Zobrazí se, jak toouse tooexamine dotazu hello příchozího datového proudu a vyhledejte volání, které mohou být podvodné.

* Odešlete hello výsledky tooan výstupní jímku (úložiště), abyste mohli analyzovat další statistiky. V takovém případě budete odesílat hello podezřelé volání data tooAzure Blob storage.

V tomto kurzu používáme hello příklad odhalování podvodů v reálném čase, které jsou na základě dat telefonního hovoru. Ale hello techniku, které jsme ilustrují vhodná taky pro jiné typy odhalování podvodů, jako je krádež platební karty podvodu nebo identity. 

## <a name="scenario-telecommunications-and-sim-fraud-detection-in-real-time"></a>Scénář: Telecommunications a SIM odhalování podvodů v reálném čase

Společnost telecommunications má velký objem dat pro příchozí volání. Hello společnost chce toodetect podvodné volá v reálném čase, tak, aby oznámit zákazníků nebo vypnout službu pro specifické číslo. Jeden typ SIM podvodů zahrnuje několik volání z hello stejnou identitu kolem hello stejný čas ale v geograficky různých umístění. toodetect tento typ podvodů, hello společnosti musí tooexamine příchozí phone záznamy a vyhledat konkrétní vzory – v takovém případě pro volání provedená kolem hello současně v různých zemí. Všechny záznamy phone, které do této kategorie patří jsou zapsány toostorage pro další analýzu.

## <a name="prerequisites"></a>Požadavky

V tomto kurzu budete simulovat telefonní hovor dat pomocí klientskou aplikaci, která generuje ukázka telefonní hovor metadat. Některé hello záznamy, které hello aplikace vytvoří vypadají podvodné volání. 

Než začnete, ujistěte se, že máte hello následující:

* Účet Azure.
* Hello události volání generátor aplikace. Tím získáte stažením hello [TelcoGenerator.zip soubor](http://download.microsoft.com/download/8/B/D/8BD50991-8D54-4F59-AB83-3354B69C8A7E/TelcoGenerator.zip) z hello Microsoft Download Center. Rozbalte tento balíček do složky v počítači. Pokud chcete toosee hello zdrojový kód a spusťte hello aplikace v ladicí program, můžete získat zdrojovém kódu aplikace hello z [Githubu](https://aka.ms/azure-stream-analytics-telcogenerator). 

    >[!NOTE]
    >Windows mohou blokovat hello stáhnout soubor .zip. Pokud nelze rozbalte ji, klikněte pravým tlačítkem na soubor hello a vyberte **vlastnosti**. Pokud se zobrazí hello zpráva "Tento soubor pochází z jiného počítače a může být blokovaný toohelp chránit tento počítač", vyberte hello **Odblokovat** a potom klikněte na **použít**.

Pokud chcete výsledky hello tooexamine úlohy streamování Analytics hello, musíte taky nástroj pro zobrazení obsahu hello kontejneru Azure Blob Storage. Pokud používáte Visual Studio, můžete použít [nástroje Azure pro sadu Visual Studio](https://docs.microsoft.com/en-us/azure/vs-azure-tools-storage-resources-server-explorer-browse-manage) nebo [cloudu Průzkumníka Visual Studio](https://docs.microsoft.com/en-us/azure/vs-azure-tools-resources-managing-with-cloud-explorer). Alternativně můžete nainstalovat samostatných nástrojů, jako je například [Azure Storage Explorer](http://storageexplorer.com/) nebo [Azure Explorer](http://www.cerebrata.com/products/azure-explorer/introduction). 

## <a name="create-an-azure-event-hubs-tooingest-events"></a>Vytvoření Azure event hubs tooingest události

tooanalyze datový proud je *ingestování* do Azure. Tooingest dat obvyklým způsobem je toouse [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md), což vám umožní přijímat miliony událostí za sekundu a pak zpracování a ukládání informací o události hello. V tomto kurzu vytvoříte centra událostí a pak mít hello události volání generátor aplikace odesílání volání data toothat centra událostí. Další informace o službě event hubs naleznete v tématu hello [dokumentace Azure Service Bus](https://docs.microsoft.com/en-us/azure/service-bus/).

>[!NOTE]
>Podrobnější verzi tohoto postupu, naleznete v části [vytvoření oboru názvů Event Hubs a Centrum událostí pomocí portálu Azure hello](../event-hubs/event-hubs-create.md). 

### <a name="create-a-namespace-and-event-hub"></a>Vytvořit obor názvů a události rozbočovače
V tomto postupu vytvoříte na obor názvů centra událostí a poté přidejte namepsace toothat centra událostí. Obory názvů centra událostí jsou používány toologically seskupení souvisejících událostí sběrnice instancí. 

1. Přihlaste se k hello portál Azure a klikněte na tlačítko **nový** > **Internet věcí** > **centra událostí**. 

2. V hello **vytvoření oboru názvů** okno, zadejte název oboru názvů, jako `<yourname>-eh-ns-demo`. Můžete použít libovolný název pro obor názvů hello, ale název hello musí být platné adresy URL a musí být jedinečný v Azure. 
    
3. Vyberte předplatné a vytvořit nebo vybrat skupinu prostředků a potom klikněte na tlačítko **vytvořit**. 

    ![Vytvoření oboru názvů události rozbočovače](./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-eventhub-namespace-new-portal.png)
 
4. Po dokončení nasazení obor názvů hello najdete hello události rozbočovače oboru názvů v seznamu prostředků Azure. 

5. Klikněte na tlačítko Nový obor názvů hello a v okně hello obor názvů, klikněte na tlačítko  **+ &nbsp;centra událostí**. 

    ![tlačítko Přidat centra událostí Hello pro vytvoření nového centra událostí ](./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-eventhub-button-new-portal.png)    
 
6. Název hello nového centra událostí `sa-eh-frauddetection-demo`. Můžete použít jiný název. Pokud tak učiníte, poznamenejte si ho, protože potřebujete název hello později. Nepotřebujete tooset další možnosti pro centra událostí hello hned teď.

    ![Okno pro vytvoření nového centra událostí](./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-eventhub-new-portal.png)
    
 
7. Klikněte na možnost **Vytvořit**.
### <a name="grant-access-toohello-event-hub-and-get-a-connection-string"></a>Udělit přístup k Centru událostí toohello a získat připojovací řetězec

Předtím, než se proces může odesílat data centra událostí tooan, musí mít centra událostí hello zásadu, která umožňuje odpovídající přístup. zásady přístupu Hello vytvoří připojovací řetězec, který obsahuje informace o ověření.

1.  V okně oboru názvů událostí hello, klikněte na tlačítko **Event Hubs** a pak klikněte na název hello nového centra událostí.

2.  V okně Centrum událostí hello, klikněte na tlačítko **zásady sdíleného přístupu** a pak klikněte na  **+ &nbsp;přidat**.

    >[!NOTE]
    >Ujistěte se, že pracujete s hello centra událostí, není hello názvů centra událostí.

3.  Přidat zásadu s názvem `sa-policy-manage-demo` a **deklarace identity**, vyberte **spravovat**.

    ![Okno pro vytvoření nové zásady přístupu centra událostí](./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-shared-access-policy-manage-new-portal.png)
 
4.  Klikněte na možnost **Vytvořit**.

5.  Po hello byla zásada nasazená, klikněte na něj v seznamu hello zásady sdíleného přístupu.

6.  Najít hello políčko s názvem **PŘIPOJOVACÍ řetězec primární klíč** a klikněte na tlačítko hello kopie tlačítko Další toohello připojovací řetězec. 
    
    ![Kopírování hello primární připojovací řetězec klíč ze zásad přístupu hello](./media/stream-analytics-real-time-fraud-detection/stream-analytics-shared-access-policy-copy-connection-string-new-portal.png)
 
7.  Vložte připojovací řetězec hello do textového editoru. Tento připojovací řetězec pro další části hello, musíte po provedení některé tooit malé úpravy.

    Hello připojovací řetězec vypadá takto:

        Endpoint=sb://YOURNAME-eh-ns-demo.servicebus.windows.net/;SharedAccessKeyName=sa-policy-manage-demo;SharedAccessKey=Gw2NFZwU1Di+rxA2T+6hJYAtFExKRXaC2oSQa0ZsPkI=;EntityPath=sa-eh-frauddetection-demo

    Všimněte si, že hello připojovací řetězec obsahuje více párů klíč hodnota oddělené středníky: `Endpoint`, `SharedAccessKeyName`, `SharedAccessKey`, a `EntityPath`.  

## <a name="configure-and-start-hello-event-generator-application"></a>Nakonfigurujte a spusťte aplikaci generátor hello událostí

Než začnete hello TelcoGenerator aplikace, musíte ho nakonfigurovat tak, aby se budou odesílat volání záznamy toohello události rozbočovače, který jste právě vytvořili.

### <a name="configure-hello-telcogeneratorapp"></a>Konfigurace hello TelcoGeneratorapp

1.  V editoru hello, kam jste zkopírovali hello připojovací řetězec, poznamenejte si hello `EntityPath` hodnotu a pak odeberte hello `EntityPath` pár (Nezapomeňte tooremove hello středník, předcházejícího). 

2.  Ve složce hello, kde jste rozbalené hello TelcoGenerator.zip souboru otevřete soubor telcodatagen.exe.config hello v editoru. (Existuje více než jeden soubor .config, proto se ujistěte, že otevíráte hello doprava o jednu.)

3.  V hello `<appSettings>` elementu, to udělat:

    * Nastavte hodnotu hello hello `EventHubName` klíče toohello název centra událostí (tedy toohello hodnota hello entity cesty).
    * Nastavte hodnotu hello hello `Microsoft.ServiceBus.ConnectionString` klíče toohello připojovací řetězec. 

    Hello `<appSettings>` části bude vypadat podobně jako následující příklad hello. (Pro přehlednost, jsme zabalená hello řádky a odebrat některé znaky z hello autorizační token.)

    ![Soubor konfigurace aplikace TelcoGenerator zobrazující hello event hub název a připojovací řetězec](./media/stream-analytics-real-time-fraud-detection/stream-analytics-telcogenerator-config-file-app-settings.png)
 
4.  Uložte soubor hello. 

### <a name="start-hello-app"></a>Spuštění aplikace hello
1.  Otevřete okno příkazového řádku a změňte toohello složku, kde je rozbalené aplikace TelcoGenerator hello.
2.  Zadejte hello následující příkaz:

        telcodatagen.exe 1000 .2 2

    Hello parametry jsou: 

    * Počet disky CDR za hodinu. 
    * SIM karta podvod pravděpodobnosti: Jak často, jako procento všechna volání hello aplikaci by měla simulovat podvodné volání. Hodnota Hello.2 znamená, že bude vypadat přibližně 20 % hello volání záznamy podvodné.
    * Doba v hodinách. Hello počet hodin, které hello aplikace měly být spuštěny. Můžete také zastavit aplikace hello kdykoli stisknutím kombinace kláves Ctrl + C v příkazovém řádku hello.

    Za několik sekund spuštění aplikace hello zobrazení telefonní hovor záznamy v úvodní obrazovka jako odešle je toohello centra událostí.

Hello klíčová pole, které budete používat v této aplikaci zjišťování podvodů v reálném čase se hello následující:

|**Záznam**|**Definice**|
|----------|--------------|
|`CallrecTime`|časové razítko Hello hello volání počáteční čas. |
|`SwitchNum`|přepínač telefonní Hello používá tooconnect hello volání. V tomto příkladu hello přepínače jsou řetězce, které představují hello země původu (USA, Čína, UK, Německu nebo Austrálie). |
|`CallingNum`|telefonní číslo Hello hello volajícího. |
|`CallingIMSI`|Hello mezinárodní Mobile Subscriber Identity IMSI (). Toto je jedinečný identifikátor volajícím hello hello. |
|`CalledNum`|Hello telefonní číslo příjemce volání hello. |
|`CalledIMSI`|Mezinárodní mobilní odběratele Identity (IMSI). Toto je jedinečný identifikátor hello hello volání příjemce. |


## <a name="create-a-stream-analytics-job-toomanage-streaming-data"></a>Vytvoření toomanage úlohy Stream Analytics, streamování dat

Teď, když máte datový proud událostí volání, můžete nastavit úlohu služby Stream Analytics. Úloha Hello bude číst data z hello centra událostí, které jste nastavili. 

### <a name="create-hello-job"></a>Vytvořit úlohu hello 

1. V hello portálu Azure, klikněte na **nový** > **Internet věcí** > **úlohy služby Stream Analytics**.

2. Název úlohy hello `sa_frauddetection_job_demo`, určete předplatné, skupinu prostředků a umístění.

    Je vhodné tooplace hello úlohy a hello událostí rozbočovač v hello stejné oblasti pro nejlepší výkon a tak, že nemáte platíte tootransfer dat mezi oblastmi.

    ![Vytvoření nové úlohy Stream Analytics](./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-sa-job-new-portal.png)

3. Klikněte na možnost **Vytvořit**.

    bude vytvořena úloha Hello a hello portálu se zobrazí podrobnosti úlohy. Nic běží ještě, přestože – máte tooconfigure hello úloha může být zahájena.

### <a name="configure-job-input"></a>Konfigurace úlohy vstup

1. V řídicím panelu hello nebo hello **všechny prostředky** okno, vyhledejte a vyberte hello `sa_frauddetection_job_demo` úlohy služby Stream Analytics. 
2. V hello **úlohy topologie** části hello okno úlohy Stream Analytics, klikněte na tlačítko hello **vstup** pole.

    ![Vstupní pole v části topologie v okně úlohy streamování Analytics hello](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-input-box-new-portal.png)
 
3. Klikněte na tlačítko  **+ &nbsp;přidat** a potom vyplňte hello okno s těmito hodnotami:

    * **Vstupní alias**: použití hello název `CallStream`. Pokud použijete jiný název, poznamenejte si jeho vzhledem k tomu, že ho budete potřebovat později.
    * **Typ zdroje**: vyberte **datový proud**. (**Referenční data** odkazuje toostatic vyhledávání data, která nebude používat v tomto kurzu.)
    * **Zdroj**: vyberte **centra událostí**.
    * **Import možnost**: vyberte **použijte Centrum událostí z aktuálního předplatného**. 
    * **Obor názvů sběrnice**: Vyberte hello události rozbočovače názvů, který jste vytvořili dříve (`<yourname>-eh-ns-demo`).
    * **Centra událostí**: Vyberte hello centra událostí, který jste vytvořili dříve (`sa-eh-frauddetection-demo`).
    * **Název zásady centra událostí**: Vyberte hello zásady přístupu, kterou jste vytvořili dříve (`sa-policy-manage-demo`).

    ![Vytvořit nový vstupní úlohy streamování Analytics](./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-sa-input-new-portal.png)

4. Klikněte na možnost **Vytvořit**.

## <a name="create-queries-tootransform-real-time-data"></a>Vytváření dotazů tootransform data v reálném čase

V tuto chvíli máte úloha Stream Analytics nastavit tooread příchozí datový proud. dalším krokem Hello je toocreate transformaci, která analyzuje hello data v reálném čase. To uděláte tak, že vytvoříte dotaz. Stream Analytics podporuje jednoduchý a deklarativní dotazu model, který popisuje transformace pro zpracování v reálném čase. Hello dotazy pomocí SQL jako jazyk, který má některé analytics konkrétní toostream rozšíření. 

Velmi jednoduchý dotaz může být jednoduše načíst všechna data příchozí hello. Ale často vytvořit dotazy, které vypadají pro konkrétní data, nebo pro vztahy v datech hello. V této části kurzu hello vytvoříte a otestovat několik dotazů toolearn několik způsobů, ve kterých můžete převést vstupní datový proud pro analýzu. 

Hello dotazy, které tady vytvoříte právě zobrazí úvodní obrazovka toohello Transformovaná data. V další části nakonfigurujete výstupní jímku a dotaz, který zapíše hello Transformovaná data toothat jímky.

toolearn Další informace o jazyce hello, najdete v části hello [referenční příručka Azure Stream Analytics Query Language](https://msdn.microsoft.com/library/dn834998.aspx).

### <a name="get-sample-data-for-testing-queries"></a>Načíst ukázková data pro testování dotazů

Hello TelcoGenerator aplikace odesílá centra událostí toohello volání záznamy a vaše úlohy služby Stream Analytics je nakonfigurované tooread z centra událostí hello. Můžete použít dotazu tootest hello úlohy toomake jistotu, že je správně čtení. příliš testování dotazu v hello konzoly Azure, musíte ukázková data. V tomto návodu budete extrahovat ukázková data z datového proudu hello, který přichází do centra událostí hello.

1. Zajistěte, aby tuto aplikaci TelcoGenerator hello používá a vytváření záznamů volání.
2. V portálu hello vrátíte okno úlohy streamování Analytics toohello. (Pokud jste zavřeli okno hello, vyhledejte `sa_frauddetection_job_demo` v hello **všechny prostředky** okno.)
3. Klikněte na tlačítko hello **dotazu** pole. Azure obsahuje hello vstupy a výstupy, které jsou nakonfigurované pro hello úlohy a umožňuje vám vytvořit dotaz, který umožňuje transformovat vstupního datového proudu hello při přenosu toohello výstup.
4. V hello **dotazu** okně klikněte na tlačítko Další toohello hello tečky `CallStream` vstup a pak vyberte **vzorová data ze vstupu**.

    ![Nabídka Možnosti toouse ukázková data pro hello položka se "Ukázkových dat ze vstupu" vybrané úlohy streamování Analytics](./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-sample-data-from-input.png)

    Otevře se okno, které umožňuje určit, kolik tooget ukázková data, definována v tom, jak dlouho tooread hello vstupní datový proud.

5. Nastavit **minut** too3 a pak klikněte na **OK**. 
    
    ![Možnosti vzorkování hello vstupního datového proudu, s vybrané "3 minut".](./media/stream-analytics-real-time-fraud-detection/stream-analytics-input-create-sample-data.png)

    Azure ukázky data z vstupního datového proudu hello za 3 minuty a vás upozorní, jakmile je připraven hello ukázková data. (To trvá nějakou dobu.) 

Hello ukázková data jsou ukládána dočasně a je k dispozici, když máte otevřete okno dotazu hello. Pokud zavřete okno dotazu hello hello ukázková data zahozena a toocreate budete mít novou sadu ukázková data. 

Jako alternativu můžete získat soubor .json, který se nachází ukázková data [z Githubu](https://github.com/Azure/azure-stream-analytics/blob/master/Sample%20Data/telco.json)a pak nahrajte tento toouse soubor .json jako ukázková data pro hello `CallStream` vstupní. 

### <a name="test-using-a-pass-through-query"></a>Testování pomocí předávacího dotazu

Pokud chcete tooarchive každé události, můžete použít předávací dotaz tooread všechna pole hello v datové části hello hello události.

1. V okně dotazu hello zadejte tento dotaz:

        SELECT 
            *
        FROM 
            CallStream

    >[!NOTE]
    >Stejně jako u SQL, klíčová slova nejsou velká a malá písmena a prázdný znak není důležité.

    V tomto dotazu `CallStream` je alias hello, kterou jste zadali při vytvoření hello vstup. Pokud jste použili jiný alias, použijte tento název.

2. Klikněte na tlačítko **Test**.

    Úloha Stream Analytics Hello spouští dotaz hello hello ukázková data a zobrazí výstup hello v hello dolní části okna hello. Znamená to, zda jsou správně nakonfigurovány hello centra událostí a úlohy streamování Analytics hello. (Jak je uvedeno, později vytvoříte výstupní jímku této hello dotaz může zapisovat data.)

    ![Úlohy Stream Analytics se výstup, zobrazují se 73 záznamy vygenerované](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-sample-output.png)

    Hello přesný počet záznamů, které vidíte bude záviset na počet záznamů zaznamenalo v 3 minut ukázky.
 
### <a name="reduce-hello-number-of-fields-using-a-column-projection"></a>Snižte počet hello polí pomocí sloupce projekce

V mnoha případech nepotřebuje analýzy všechny sloupce hello ze vstupního proudu hello. Můžete použít dotazu tooproject menší sadu vrátí pole než hello předávací dotaz.

1. Změna dotazu hello v hello kód editor toohello následující:

        SELECT CallRecTime, SwitchNum, CallingIMSI, CallingNum, CalledNum 
        FROM 
            CallStream

2. Klikněte na tlačítko **Test** znovu. 

    ![Úlohy Stream Analytics se výstup. pro projekci, zobrazují se 25 záznamy vygenerované](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-sample-output-projection.png)
 
### <a name="count-incoming-calls-by-region-tumbling-window-with-aggregation"></a>Počet příchozích volání podle oblasti: Přeskakující okno s průběhem

Předpokládejme, že chcete toocount hello počet příchozí volání na oblast. Streamovaných dat užitečné, když chcete, aby tooperform agregační funkce jako počítání, je nutné toosegment hello datový proud do dočasné jednotky (protože je efektivně nekonečná hello datový proud sám sebe). Můžete to udělat pomocí Analytics se streamování [oddílovou funkci](stream-analytics-window-functions.md). Pak můžete pracovat s daty hello uvnitř toto okno jako jednotku.

Tato transformace se má pořadí dočasné Windows, které nepřekrývaly – každé okno bude mít diskrétní sadu dat, která můžete seskupovat a agregaci. Tento typ okna se označují tooas *Přeskakující okno* . V rámci hello Přeskakující okno, můžete získat počet příchozích hovorů hello seskupené podle `SwitchNum`, která představuje hello zemi, kde hello volání vytvořena. 

1. Změna dotazu hello v hello kód editor toohello následující:

        SELECT 
            System.Timestamp as WindowEnd, SwitchNum, COUNT(*) as CallCount 
        FROM
            CallStream TIMESTAMP BY CallRecTime 
        GROUP BY TUMBLINGWINDOW(s, 5), SwitchNum

    Tento dotaz používá hello `Timestamp By` – klíčové slovo v hello `FROM` klauzule toospecify které pole časového razítka v hello vstupní datový proud toouse toodefine hello Přeskakující okno. V takovém případě hello okno rozdělí hello data na segmenty ve hello `CallRecTime` pole všechny záznamy. (Pokud není zadaný žádný pole, operace oddílová hello používá hello dobu, po kterou všechny události dorazí na hello centra událostí. V části "Čas aplikace Vs čas doručení" v [Stream Analytics referenční příručka jazyka dotazů](https://msdn.microsoft.com/library/azure/dn834998.aspx). 

    projekce Hello zahrnuje `System.Timestamp`, která vrací časové razítko pro element end hello jednotlivých období. 

    toospecify, které chcete toouse Přeskakující okno, použijte hello [TUMBLINGWINDOW](https://msdn.microsoft.com/library/dn835055.aspx) funkce v hello `GROUP BY `klauzule. Ve funkci hello zadejte časovou jednotku (od mikrosekund tooa denně) a velikost okna (počet jednotek). V tomto příkladu hello Přeskakující okno se skládá z 5 sekund intervalech, zobrazí se počet podle země pro volání za každých 5 sekund.

2. Klikněte na tlačítko **Test** znovu. Ve výsledcích hello, Všimněte si, že hello časová razítka v části **WindowEnd** jsou v přírůstcích po 5 sekund.

    ![Úlohy Stream Analytics se výstup za účelem agregace, zobrazují se 13 záznamy vygenerované](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-sample-output-aggregation.png)
 
### <a name="detect-sim-fraud-using-a-self-join"></a>Odhalování podvodů SIM pomocí vlastní spojení

V tomto příkladu jsme můžete zvážit podvodné použití toobe volání, které pocházejí z hello stejné uživatele, ale v různých umístěních do 5 sekund od sebe navzájem. Například hello stejného uživatele nelze vytvořit legálně volání z hello USA a Austrálie v hello stejnou dobu. 

toocheck pro tyto případy, můžete použít vlastní spojení hello streamování dat toojoin hello datového proudu tooitself podle hello `CallRecTime` hodnotu. Potom můžete zobrazit pro volání zaznamenává kde hello `CallingIMSI` hodnotu (hello pocházející číslo) je hello stejné, ale hello `SwitchNum` hodnotu (země původu) není hello stejné.

Když použijete ke spojení s streamovaných dat užitečné, spojení hello musíte zadat, že některá omezení na tom, jak úplně hello odpovídající řádky je možné oddělit v čase. (Jak jsme uvedli dříve, streamování dat hello je efektivně nekonečná.) rozsah čas Hello hello relace jsou určeny uvnitř hello `ON` klauzuli spojení hello pomocí hello `DATEDIFF` funkce. V takovém případě hello spojení je založena na 5 sekund intervalu dat volání.

1. Změna dotazu hello v hello kód editor toohello následující: 

        SELECT  System.Timestamp as Time, 
            CS1.CallingIMSI, 
            CS1.CallingNum as CallingNum1, 
            CS2.CallingNum as CallingNum2, 
            CS1.SwitchNum as Switch1, 
            CS2.SwitchNum as Switch2 
        FROM CallStream CS1 TIMESTAMP BY CallRecTime 
            JOIN CallStream CS2 TIMESTAMP BY CallRecTime 
            ON CS1.CallingIMSI = CS2.CallingIMSI 
            AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5 
        WHERE CS1.SwitchNum != CS2.SwitchNum

    Tento dotaz není třeba žádné připojení k SQL s výjimkou hello `DATEDIFF` funkce v hello spojení. Toto je verze `DATEDIFF` se konkrétní tooStreaming analýzy, a musí se nacházet v hello `ON...BETWEEN` klauzule. Hello parametry jsou časovou jednotku (v sekundách v tomto příkladu) a aliasy hello hello dvou zdrojů pro připojení k hello. (To se liší od standardní SQL hello `DATEDIFF` funkce.) 

    Hello `WHERE` klauzule zahrnuje hello podmínku, která flags podvodné volání hello: hello původní přepínače nejsou hello stejné. 

2. Klikněte na tlačítko **Test** znovu. 

    ![Výstup pro spojení sama na sebe, zobrazení 6 záznamů vygenerovaných úlohy Stream Analytics](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-sample-output-self-join.png)

3. Klikněte na **Uložit**. To umožňuje ušetřit hello spojení sama na sebe dotaz jako součást úlohy streamování Analytics hello. (Ho nebude uložen hello ukázková data.)

    ![Uložit úlohu služby Stream Analytics](./media/stream-analytics-real-time-fraud-detection/stream-analytics-query-editor-save-button-new-portal.png)

## <a name="create-an-output-sink-toostore-transformed-data"></a>Vytvoření výstupní jímku toostore transformovat data

Přes hello datového proudu, které jste definovali datového proudu událostí, události vstupní tooingest centra událostí a dotaz tooperform transformace. posledním krokem Hello je toodefine výstupní jímku pro úlohu hello – to znamená, místní toowrite hello transformaci datového proudu k. 

Mnoho prostředků můžete použít jako výstup jímky – databázi systému SQL Server, úložiště table, úložiště Data Lake, Power BI a i jiné centra událostí. V tomto kurzu napíšete hello datového proudu tooAzure úložiště objektů Blob, což je typické volbou pro shromažďování informací o události pro pozdější analýzu, protože počítá Nestrukturovaná data.

Pokud máte stávající účet úložiště objektů blob, můžete použít, který. V tomto kurzu ukážeme jak toocreate nové úložiště účet pouze pro účely tohoto kurzu.

### <a name="create-an-azure-blob-storage-account"></a>Vytvoření účtu Azure Blob Storage

1. V hello portálu Azure vrátíte okno úlohy streamování Analytics toohello. (Pokud jste zavřeli okno hello, vyhledejte `sa_frauddetection_job_demo` v hello **všechny prostředky** okno.)
2. V hello **úlohy topologie** klikněte na tlačítko hello **výstup** pole. 
3. V hello **výstupy** okně klikněte na tlačítko  **+ &nbsp;přidat** a potom vyplňte hello okno s těmito hodnotami:

    * **Alias pro výstup**: použití hello název `CallStream-FraudulentCalls`. 
    * **Jímky**: vyberte **úložiště objektů Blob**.
    * **Možnosti importu**: vyberte **pomocí úložiště objektů blob z aktuálního předplatného**.
    * **Účet úložiště**. Vyberte **vytvořit nový účet úložiště.**
    * **Účet úložiště** (druhé pole). Zadejte `YOURNAMEsademo`, kde `YOURNAME` je název vaší nebo jiné jedinečné řetězce. v názvu Hello lze použít pouze malá písmena a čísla a musí být jedinečný v Azure. 
    * **Kontejner**. Zadejte `sa-fraudulentcalls-demo`.
    název účtu úložiště Hello a název kontejneru jsou používané společně tooprovide identifikátor URI pro úložiště objektů blob hello, například takto: 

    `http://yournamesademo.blob.core.windows.net/sa-fraudulentcalls-demo/...`
    
    !["Nové výstup" okno úlohy Stream Analytics](./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-output-blob-storage-new-console.png)
    
4. Klikněte na možnost **Vytvořit**. 

    Azure vytvoří účet úložiště hello a automaticky vygeneruje klíč. 

5. Zavřít hello **výstupy** okno. 

## <a name="start-hello-streaming-analytics-job"></a>Spustit úlohu streamování Analytics hello

Úloha Hello je nyní nakonfigurována. Zadané vstup (centra událostí hello), transformaci (hello dotazu toolook pro podvodné volání) a výstup (úložiště objektů blob). Nyní můžete spustit úlohy hello. 

1. Zkontrolujte, zda text hello TelcoGenerator aplikace běží.

2. V okně úlohy hello, klikněte na tlačítko **spustit**.

    ![Spuštění úlohy Stream Analytics hello](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-start-output.png)

3. V hello **spuštění úlohy** okně pro výstup čas spuštění úlohy, vyberte **nyní**. 

4. Klikněte na tlačítko **spustit**. 

    !["Spuštění úlohy" okno úlohy Stream Analytics hello](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-start-job-blade.png)

    Azure vás upozorní, jakmile byla spuštěna úloha hello a v okně úlohy hello hello stav se zobrazí jako **systémem**.

    ![Stav úlohy Stream Analytics, zobrazující "Spuštěný"](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-running-status.png)
    

## <a name="examine-hello-transformed-data"></a>Zkontrolujte hello transformovat data

Nyní máte dokončení úlohy streamování Analytics. Úloha Hello je zkoumání proud metadata telefonní hovor, hledá podvodné telefonní hovory v reálném čase a zápis informace o těchto toostorage podvodné volání. 

toocomplete tohoto kurzu můžete chtít toolook v hello dat probíhá zachycenou úlohy streamování Analytics hello. Hello zapisuje data tooAzure Blog úložiště v bloky dat (soubory). Můžete použít jakýkoli nástroj, který čte Azure Blob Storage. Jak jsme uvedli v hello v části předpoklady, můžete použít rozšíření Azure v sadě Visual Studio, nebo můžete použít nástroje, jako je [Azure Storage Explorer](http://storageexplorer.com/) nebo [Azure Explorer](http://www.cerebrata.com/products/azure-explorer/introduction). 

Při kontrole hello obsah souboru v úložišti objektů blob, můžete vidět něco podobného jako hello následující:

![Úložiště objektů blob v Azure s výstupem analýzy datových proudů](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-blob-storage-view.png)
 

## <a name="clean-up-resources"></a>Vyčištění prostředků

Máme další články, které dál se scénářem hello odhalování podvodů a které používají hello prostředky, které jste vytvořili v tomto kurzu. Pokud chcete toocontinue, najdete návrhy hello pod **další kroky** později.

Ale pokud s tím budete hotovi a nepotřebujete hello prostředky, které jste vytvořili, odstraněním je tak, aby vám zbytečně nenabíhaly poplatky nepotřebné Azure. V takovém případě doporučujeme, aby hello následující:

1. Zastavte úlohu streamování Analytics hello. V hello **úlohy** okně klikněte na tlačítko **Zastavit** v horní části hello.
2. Zastavte aplikaci Telco generátor hello. V jakém jste začínali hello aplikace hello příkazové okno stiskněte kombinaci kláves Ctrl + C.
3. Pokud jste vytvořili nový účet úložiště objektů blob jenom pro účely tohoto kurzu, odstraňte jej. 
4. Odstraňte úlohu streamování Analytics hello.
5. Odstraňte hello centra událostí.
6. Odstraňte názvový prostor hello události rozbočovače.

## <a name="get-support"></a>Získat podporu

Pro další pomoc, vyzkoušejte naše [fórum Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Další kroky

V tomto kurzu můžete pokračovat v hello následující články:

* [Stream Analytics a Power BI: řídicí panel analýzy v reálném čase pro datový proud](stream-analytics-power-bi-dashboard.md). Tento článek ukazuje, jak toosend hello TelCo výstup hello Stream Analytics úlohy tooPower BI v reálném čase vizualizaci a analýzu.
* [Jak toostore data ze služby Azure Stream Analytics v Azure Redis Cache pomocí Azure Functions](stream-analytics-functions-redis.md). Tento článek popisuje, jakým způsobem volá toouse Azure Functions toowrite podvodné tooan Azure Redis cache prostřednictvím fronty Service Bus.

Další informace o Stream Analytics obecně platí, zkuste tyto články:

* [Úvod tooAzure Stream Analytics](stream-analytics-introduction.md)
* [Škálování služby Stream Analytics](stream-analytics-scale-jobs.md)
* [Referenční příručka k jazyku Azure Stream Analytics Query Language](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)
