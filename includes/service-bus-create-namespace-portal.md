fronty toobegin pomocí služby Service Bus v Azure, musíte nejdřív vytvořit obor názvů s názvem, který je v rámci Azure jedinečný. Obor názvů poskytuje kontejner oboru pro adresování prostředků služby Service Bus v rámci vaší aplikace.

toocreate obor názvů:

1. Přihlaste se toohello [portál Azure][Azure portal].
2. V levém navigačním podokně hello hello portálu, klikněte na **nový**, pak klikněte na tlačítko **Enterprise integrace**a potom klikněte na **Service Bus**.
3. V hello **vytvoření oboru názvů** dialogové okno, zadejte název oboru názvů. systém Hello toosee ověří okamžitě, pokud je k dispozici název hello.
4. Po provedení že název oboru názvů hello je k dispozici, zvolte hello cenová úroveň (Basic, Standard nebo Premium).
5. V hello **předplatné** pole, zvolte předplatné Azure v oboru názvů které toocreate hello.
6. V hello **skupiny prostředků** pole, vyberte existující skupinu prostředků v které hello obor názvů se za provozu nebo vytvořte novou.      
7. V **umístění**, zvolte hello zemi nebo oblast, ve kterém by hostovat vašeho oboru názvů.
   
    ![Vytvoření oboru názvů][create-namespace]
8. Klikněte na možnost **Vytvořit**. Hello systém teď vytvoří obor názvů a povolí ho. Toowait může mít několik minut hello systém zřídí prostředky pro váš účet.

### <a name="obtain-hello-management-credentials"></a>Získání pověření pro správu hello

1. V seznamu hello oborů názvů klikněte na tlačítko hello nově vytvořený obor názvů.
2. V okně hello obor názvů, klikněte na tlačítko **zásady sdíleného přístupu**.
3. V hello **zásady sdíleného přístupu** okně klikněte na tlačítko **RootManageSharedAccessKey**.
   
    ![connection-info][connection-info]
4. V hello **zásady: RootManageSharedAccessKey** okně klikněte na tlačítko Kopírovat hello další příliš**připojovací řetězec – primární klíč**, toocopy hello připojovací řetězec tooyour schránky pro pozdější použití. Vložte tuto hodnotu do Poznámkového bloku nebo jiného dočasného umístění.
   
    ![connection-string][connection-string]

5. Předchozí krok zopakujte hello, kopírování a vkládání hello hodnotu **primární klíč** tooa dočasného umístění pro pozdější použití.

<!--Image references-->

[create-namespace]: ./media/service-bus-create-namespace-portal/create-namespace.png
[connection-info]: ./media/service-bus-create-namespace-portal/connection-info.png
[connection-string]: ./media/service-bus-create-namespace-portal/connection-string.png
[Azure portal]: https://portal.azure.com
