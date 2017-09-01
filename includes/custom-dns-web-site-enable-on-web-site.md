Po rozšíření mají záznamy pro název domény, je třeba přidružit k vaší webové aplikace. Pomocí následujících kroků povolte názvů domén pomocí webového prohlížeče.

> [!NOTE]
> Může trvat nějakou dobu záznamů TXT vytvořili v předchozích krocích rozšíří v rámci systému DNS. Název domény nelze přidat do vaší webové aplikace, dokud se rozšíří záznam TXT. Pokud používáte záznam A, nelze přidat název záznamu domény A do vaší webové aplikace dokud má rozšíří záznam TXT vytvořili v předchozím kroku.
> 
> Je třeba použít službu <a href="http://www.digwebinterface.com/">http://www.digwebinterface.com/</a> k ověření, že záznam TXT je k dispozici.
> 
> 

1. V prohlížeči otevřete [portálu Azure](https://portal.azure.com).
2. V **webové aplikace** , klikněte na název vaší webové aplikace a pak vyberte **vlastní domény**
   
    ![](./media/custom-dns-web-site/dncmntask-cname-6.png)
3. V **vlastní domény** okně klikněte na tlačítko **přidat název hostitele**.
4. Použití **Hostname** textová pole zadejte název domény, který chcete přidružit k této webové aplikace.
   
    ![](./media/custom-dns-web-site/add-custom-domain.png)
5. Klikněte na tlačítko **ověření**.
6. Po kliknutí na tlačítko **ověřením** Azure bude ji pracovní postup ověření domény. To bude kontrolovat vlastnictví domény a také název hostitele dostupnosti a sestava úspěch nebo podrobné chybové s doporučený guidence o tom, jak chybu opravit.    

V tomto okamžiku by měl být zadejte vlastní název domény v prohlížeči a zobrazit tak, že je úspěšně přejdete do vaší webové aplikace.

