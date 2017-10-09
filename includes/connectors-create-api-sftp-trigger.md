Přidejme aktivační událost.

1. Zadejte *sftp* hello vyhledávacího pole v designeru aplikace logiky hello zvolte hello **SFTP – Pokud je soubor přidat ani upravit** aktivační události   
   ![Obrázek aktivační události pomocí protokolu SFTP 1](./media/connectors-create-api-sftp/trigger-1.png)  
2. Hello **je přidána nebo upravena souboru** otevře se ovládací prvek  
   ![Obrázek aktivační události pomocí protokolu SFTP 2](./media/connectors-create-api-sftp/trigger-2.png)  
3. Vyberte hello **...**  nachází na pravé straně hello hello řízení. Otevře se ovládací prvek pro výběr složky hello  
   ![Obrázek aktivační události pomocí protokolu SFTP 3](./media/connectors-create-api-sftp/action-1.png)  
4. Vyberte hello **SFTP** tooselect hello kořenové složky jako hello toomonitor složku pro nové nebo upravení soubory. Všimněte si hello kořenové složky se nyní zobrazí v hello **složky** ovládacího prvku.  
   ![Obrázek aktivační události pomocí protokolu SFTP 4](./media/connectors-create-api-sftp/action-2.png)   

Aplikace logiky v tomto okamžiku je nakonfigurovaná s aktivační událost, která bude zahájena spuštění hello jiných triggery a akce v pracovním postupu hello po upravit nebo vytvořit v konkrétní složce SFTP hello souboru. 

> [!NOTE]
> Pro logiku aplikace toobe funkční musí obsahovat nejméně jedna aktivační událost a jedna akce. Postupujte podle kroků hello v další části tooadd hello akce.  
> 
> 

