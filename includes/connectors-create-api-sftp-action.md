Teď, když jste přidali aktivační událost, jeho čas toodo něco zajímavé s hello data, která je generován hello aktivační události. Postupujte podle těchto kroků tooadd hello **SFTP - extract složky** akce. Tato akce extrahuje obsah hello souboru hello definované podmínek. 

tooconfigure hello tuto akci, budete potřebovat tooprovide hello následující informace. Si všimnete, že je snadno toouse data generována aktivační událost hello jako vstup pro některé hello vlastnosti pro nový soubor hello:

| Pomocí protokolu SFTP - vlastnost složky extrakce | Popis |
| --- | --- |
| Cestu ke zdrojovému souboru archivu |Toto je hello cestu pro soubor hello se extrahují. Můžete vyberte jednu z hello tokeny starší akce nebo procházet cesta souboru hello SFTP serveru toofind hello. |
| Cesta k cílové složce |Toto je hello cestu, kde budou umístěné soubory extrahovány hello. Můžete vybrat jednu hello tokenů z dřívějších akce jako hello cílovou cestu nebo procházet hello SFTP serveru a vyberte cestu. |
| Chcete přepsat? |Označuje, pokud soubor s hello stejný název jako hello extrahované soubor naleznete ve hello cestu k cílové složce Pokud hello existující soubor by měl být přepsat, nebo ne. |

Začněme přidáním hello akce tooextract hello souborů, pokud je podmínka hello definovaného dříve výsledkem příliš*True*. 

1. Vyberte **přidat akci**.        
   ![Obrázek stavu akce SFTP 6](./media/connectors-create-api-sftp/condition-6.png)   
2. Vyberte hello **SFTP - Extract složky** akce      
   ![Obrázek stavu akce SFTP 7](./media/connectors-create-api-sftp/condition-7.png)   
3. Vyberte **cestu ke zdrojovému souboru archivu**              
   ![Obrázek stavu akce SFTP 9](./media/connectors-create-api-sftp/condition-9.png)   
4. Vyberte hello **cesta k souboru** tokenu. To znamená, že použijete cesta k souboru hello hello souboru, který hello najít jako cesta k souboru archivu hello zdroj aktivační události.           
   ![Obrázek stavu akce SFTP 10](./media/connectors-create-api-sftp/condition-10.png)   
5. Vyberte **cestu k cílové složce**           
   ![Obrázek stavu akce SFTP 11](./media/connectors-create-api-sftp/condition-11.png)   
6. Vyberte hello **cesta k souboru** tokenu. To znamená, že použijete cesta k souboru hello hello souboru, který hello najdou hello cílovou cestu pro soubory hello extrahovat aktivační události.   
7. Zadejte *\ExtractedFile* v hello **cestu k cílové složce** ovládacího prvku. To lze proveďte pouze po token path hello souborů v cílové složce cesta řízení hello.         
   ![Obrázek stavu akce SFTP 12](./media/connectors-create-api-sftp/condition-12.png)   
8. Zadejte *True* v hello **přepsat?* tooindicate ovládací prvek, který by měl být existující soubory přepsána, pokud mají stejný název jako hello hello rozbalené soubory.      
   ![Obrázek stavu akce SFTP 13](./media/connectors-create-api-sftp/condition-13.png)   
9. Uložení pracovního postupu tooyour změny hello  

