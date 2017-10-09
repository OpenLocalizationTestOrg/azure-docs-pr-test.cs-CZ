Teď, když jste přidali podmínku, jeho čas toodo něco zajímavé s hello data, která je generován hello aktivační události. Postupujte podle těchto kroků tooadd hello **Salesforce - Get objekt** akce. Tato akce se získat hello data pokaždé, když se vytvoří nové zájemce. Přidejte také druhou akci, který bude používat hello data z hello Salesforce - Get toosend akce k objektu e-mailu pomocí konektoru hello Office 365.  

tooconfigure hello tuto akci, budete potřebovat tooprovide hello následující informace. Si všimnete, že je snadno toouse data generována aktivační událost hello jako vstup pro některé hello vlastnosti pro nový soubor hello:

| Vlastnost souboru | Popis |
| --- | --- |
| Typ objektu |Toto je hello typ objektu služby Salesforce, které vás zajímají. Příklady jsou realizace, účet, atd. |
| ID objektu |Reprezentuje identifikátor pro objekt hello. |

1. Vyberte **přidat akci** odkaz. Pole pro vyhledávání otevře hello kde můžete vyhledat všechny akce jste chtěli tootake. V tomto příkladu jsou Salesforce akce zájmu.      
   ![Obrázek akce Salesforce 1](./media/connectors-create-api-salesforce/action-1.png)  
2. Zadejte *salesforce* toosearch pro akce související toosalesforce.
3. Vyberte **Salesforce - Get objekt** jako hello tootake akce.   **Poznámka:**: vám bude výzvami tooauthorize vaše tooaccess aplikace logiky vaše služby Salesforce účtu, pokud jste tak dosud neučinili dříve.    
   ![Obrázek akce Salesforce 2](./media/connectors-create-api-salesforce/action-2.png)    
4. Hello **objekt Get** řízení otevře.  
5. Vyberte *vést* jako typ objektu hello.
6. Vyberte hello **ID objektu** ovládacího prvku.
7. Vyberte **...**  tooexpand hello seznam tokeny, které lze použít jako vstup pro akce.       
   ![Obrázek akce Salesforce 3](./media/connectors-create-api-salesforce/action-3.png)    
8. Vyberte **vést ID** řízení otevře.   
   ![Obrázek akce Salesforce 4](./media/connectors-create-api-salesforce/action-4.png)     
9. Všimněte si, že tento token vést ID hello je nyní v hello řízení ID objektu, která určuje, že hello Get akce objektu bude hledat realizace s ID, které je rovna toohello realizace ID realizace, která aktivuje tuto aplikaci logiky.  
   ![Obrázek akce Salesforce 5](./media/connectors-create-api-salesforce/action-5.png)  
10. Uložte práci. Je to, jste přidali hello Get objektu akce tooyour logiku aplikace. Ovládací prvek objekt Get by měl vypadat takto:    
    ![Obrázek akce Salesforce 6](./media/connectors-create-api-salesforce/action-6.png)  

Teď, když jste přidali akci tooget zájemce, můžete toodo něco zajímavé s realizace hello nově vytvořený. V podniku může být vhodné toosend toonotify e-mailu distribuční seznam vytvořený nové zájemce. Použijeme toosend konektor hello Office 365 e-mailu s některými hello příslušné informace z hello nový objekt realizace v Salesforce.  

1. Vyberte **přidat akci** zadejte *e-mailu* v ovládacím prvku vyhledávání hello. Tím se odfiltrují toothose hello akce, které jsou související toosending a příjmu e-mailu.  
2. Vyberte hello **Office 365 Outlook - e-mailu** položky seznamu. Pokud jste ještě nevytvořili *připojení* tooyour účtu Office 365, bude se vaše přihlašovací údaje toocreate Office 365 výzvami tooenter it teď. Po dokončení hello **e-mailovou zprávu** řízení otevře.        
   ![Obrázek akce Salesforce 7](./media/connectors-create-api-salesforce/action-7.png)  
3. Zadejte hello e-mailovou adresu, které chcete toosend e-mailu tooin hello **k** ovládacího prvku.
4. V hello **subjektu** řídit, zadejte *vytvořit nové vést* – vyberte hello *společnosti* tokenu. Bude se zobrazovat hello *společnosti* pole z hello nové zájemce vytvořené v Salesforce.  
5. V hello **textu** ovládací prvek, můžete vybrat všechny tokeny hello od nový objekt realizace hello a můžete také zadat jakýkoli text, které chcete toodisplay v textu hello hello e-mailu. Tady je příklad:  
   ![Obrázek akce Salesforce 8](./media/connectors-create-api-salesforce/action-8.png)   
6. Uložení pracovního postupu.  

A je to. Aplikace logiky je nyní dokončen.  

Nyní můžete testovat svou aplikaci logiky: v Salesforce, vytvořte nové zájemce, který splňuje podmínky hello jste vytvořili.  Pokud jste postupovali plně podle tohoto návodu, vytvořte s e-mailovou adresu, která obsahuje jenom zájemce *amazon.com* v ní. Za několik sekund by měla spustit aplikace logiky a výsledky hello vypadat podobně jako toothis:  
![Obrázek akce Salesforce 9](./media/connectors-create-api-salesforce/action-9.png)  

