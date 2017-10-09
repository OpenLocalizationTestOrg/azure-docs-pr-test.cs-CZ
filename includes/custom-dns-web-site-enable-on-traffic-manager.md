Po rozšíření mít hello záznamy pro název domény, měli byste být schopný toouse vaše tooverify prohlížeče, který vlastní název domény může být použité tooaccess vaší webové aplikace v Azure App Service.

> [!NOTE]
> Může trvat nějakou dobu, než vaše CNAME toopropagate prostřednictvím hello systému DNS. Je třeba použít službu <a href="http://www.digwebinterface.com/">http://www.digwebinterface.com/</a> tooverify, který hello CNAME je k dispozici.
> 
> 

Pokud jste ještě nepřidali vaší webové aplikace jako koncový bod Traffic Manager, musíte to provést před překlad bude fungovat jako hello vlastní domény název trasy tooTraffic správce systému. Traffic Manager pak směruje tooyour webové aplikace. Hello informace v [přidat nebo odstranit koncové body](../articles/traffic-manager/traffic-manager-endpoints.md) tooadd vaší webové aplikace jako koncový bod ve vašem profilu Traffic Manageru.

> [!NOTE]
> Pokud vaše webová aplikace není uveden při přidávání koncový bod, ověřte, zda je nakonfigurován pro **standardní** režimu plán služby App Service. Je nutné použít **standardní** režim pro webové aplikace v pořadí toowork s nástrojem Traffic Manager.
> 
> 

1. V prohlížeči otevřete hello [portálu Azure](https://portal.azure.com).
2. V hello **webové aplikace** , klikněte na název vaší webové aplikaci, vyberte hello **nastavení**a potom vyberte **vlastní domény**
   
    ![](./media/custom-dns-web-site/dncmntask-cname-6.png)
3. V hello **vlastní domény** okně klikněte na tlačítko **přidat název hostitele**.
4. Použití hello **Hostname** textových polí tooenter hello Traffic Manager domény název tooassociate s touto webovou aplikací.
   
    ![](./media/custom-dns-web-site/dncmntask-cname-8.png)
5. Klikněte na tlačítko **ověřením** konfigurace názvu domény toosave hello.
6. Po kliknutí na tlačítko **ověřením** Azure bude ji pracovní postup ověření domény. To bude kontrolovat vlastnictví domény a také název hostitele dostupnosti a sestava úspěch nebo podrobné chybové s doporučený guidence na tom, jak toofix hello chyby.    
7. Po úspěšném ověření **přidat název hostitele** tlačítko je aktivní a bude moct toohello přiřadit název hostitele. Nyní přejděte tooyour vlastní název domény v prohlížeči. Teď byste měli vidět spuštění vaší aplikace pomocí vlastního názvu domény. 
   
   Po dokončení konfigurace hello vlastní název domény budou zobrazeny v hello **názvy domén** část vaší webové aplikace.

V tomto okamžiku by měl být název název domény Traffic Manageru hello možné tooenter v prohlížeči a v tématu, že je úspěšně přejdete tooyour webové aplikace.

