V tomto příkladu I vám ukáže, jak toouse hello **SharePoint Online - při vytvoření nové položky** aktivují tooinitiate pracovní postup aplikace logiky, když se vytvoří novou položku v seznamu SharePoint Online.

> [!NOTE]
> Zobrazí se výzvami toosign k účtu služby SharePoint Pokud jste ještě nevytvořili *připojení* tooSharePoint Online.  
> 
> 

1. Zadejte *sharepoint* hello vyhledávacího pole v designeru aplikace logiky hello zvolte hello **SharePoint Online - při vytvoření nové položky** aktivační události.  
   ![SharePoint online aktivace image](./media/connectors-create-api-sharepointonline/trigger-1.png)  
2. Hello **při vytvoření nové položky** ovládací prvek je zobrazen.  
   ![SharePoint online aktivace obrázek 2](./media/connectors-create-api-sharepointonline/trigger-2.png)   
3. Vyberte **URL webu**. Toto je hello SharePoint online lokalita má toomonitor pro nové položky tootrigger pracovního postupu.  
   ![SharePoint online aktivace image 3](./media/connectors-create-api-sharepointonline/trigger-3.png)   
4. Vyberte **název seznamu**. Toto je seznam hello na web služby SharePoint Online, které chcete použít pro nové položky, které aktivují pracovní postup toomonitor hello.  
   ![SharePoint online aktivace obrázek 4](./media/connectors-create-api-sharepointonline/trigger-4.png)   

Aplikace logiky v tomto okamžiku je nakonfigurovaná s aktivační událost, která zahájí spuštění hello jiných triggery a akce v pracovním postupu hello. Bude to trvat místní pokaždé, když se vytvoří novou položku v seznamu SharePoint Online, které jste vybrali.  

