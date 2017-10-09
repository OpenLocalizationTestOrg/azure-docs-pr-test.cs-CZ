Teď, když jste přidali aktivační událost, jeho čas toodo něco zajímavé s hello data, která je generován hello aktivační události. Postupujte podle těchto kroků tooadd hello **SharePoint Online – vytvoření souboru** akce. Tato akce vytvoří soubor ve službě SharePoint Online každý čas hello nové položky aktivační událost je aktivována. 

tooconfigure hello tuto akci, budete potřebovat tooprovide hello následující informace. Jako zadání těchto informací si všimnete, že je snadno toouse data generována aktivační událost hello jako vstup pro některé hello vlastnosti pro nový soubor hello:

| Vlastnost souboru | Popis |
| --- | --- |
| Adresa URL webu |Toto je adresa URL hello webu služby SharePoint Online hello místo toocreate hello nový soubor. Vyberte lokalitu, hello hello seznamu uvedené. |
| Cesta ke složce |To je hello složku (v hello adresa URL webu vybrali v předchozím kroku hello) umístění hello nový soubor. Vyhledejte a vyberte složku hello. |
| Název souboru |Toto je název hello souboru hello vytváří. |
| Obsah souboru |Hello obsah, který bude zapsán toohello souboru. |

1. Vyberte **+ nový krok** tooadd hello akce.  
   ![SharePoint online akce obrázek 1](./media/connectors-create-api-sharepointonline/action-1.png)  
2. Vyberte hello **přidat akci** odkaz. Pole pro vyhledávání otevře hello kde můžete vyhledat všechny akce jste chtěli tootake. V tomto příkladu jsou SharePoint akce zájmu.    
   ![Obrázek SharePoint online akce 2](./media/connectors-create-api-sharepointonline/action-2.png)    
3. Zadejte *sharepoint* toosearch pro akce související tooSharePoint.
4. Vyberte **SharePoint Online – vytvoření souboru** jako hello tootake akce.   **Poznámka:**: vám bude výzvami tooauthorize vaše tooaccess aplikace logiky vaší SharePoint účtu, pokud jste dosud nevytvořili procházet tooSharePoint připojení, který je Online.    
   ![Obrázek SharePoint online akce 3](./media/connectors-create-api-sharepointonline/action-3.png)    
5. Hello **vytvořit soubor** řízení otevře.   
   ![SharePoint online akce obrázek 4](./media/connectors-create-api-sharepointonline/action-4.png)     
6. Vyberte **adresa URL webu** a procházet toofind hello webu, kam chcete soubor toocreate hello.     
   ![SharePoint online akce obrázku 5](./media/connectors-create-api-sharepointonline/action-5.png)  
7. Vyberte **cesta ke složce** a procházet toofind hello složku, kde budou umístěné hello nový soubor.  
   ![SharePoint online akce obrázek 6](./media/connectors-create-api-sharepointonline/action-6.png)  
8. Vyberte hello **název souboru** řízení a zadejte název hello hello souboru chcete toocreate. Zde můžete zadat název souboru hello přímo nebo můžete použít libovolnou hello vlastností ze hello aktivační události, které jste vytvořili dříve. To uděláte tak, že vyberete ze seznamu hello vlastnosti **výstupy z při vytvoření nové položky**. Tento seznam je zobrazit pouze po výběru hello **název souboru** ovládacího prvku. V této walkthough I vybrali ID (ID hello hello nové položky seznamu) jako hello název souboru hello právě vytvořené hello **SharePoint Online – vytvoření souboru** akce.    
   ![Obrázek SharePoint online akce 7](./media/connectors-create-api-sharepointonline/action-7.png)  
9. Vyberte hello **souboru obsahu** řízení a zadejte hello obsah, který bude zapsán toohello soubor, který bude vytvořen. Obsah souboru hello Všimněte si, které můžete použít jakékoli vlastnosti hello z hello aktivační události, které jste vytvořili dříve. Jednoduše vyberte vlastnosti hello hello seznamu uvedené. Alternativně můžete zadat hello **souboru obsahu** text přímo do ovládacího prvku hello. V tomto příkladu I vybrali některé vlastnosti a přidá mezery a pomlčka mezi každou vlastnost.        
   ![SharePoint online akce obrázek 8](./media/connectors-create-api-sharepointonline/action-8.png)  
10. Uložení pracovního postupu tooyour změny hello  
11. Blahopřejeme, nyní máte logiku plně funkční aplikaci, která se aktivuje, když je přidána nová položka seznamu tooa SharePoint Online. aplikace Hello pak vytvoří soubor pomocí některé vlastnosti hello z hello novou položku seznamu.  Nyní ji můžete otestovat tak, že vytvoříte novou položku v seznamu SharePoint hello. 

