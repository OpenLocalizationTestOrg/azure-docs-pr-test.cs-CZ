### <a name="prerequisites"></a>Požadavky
Musíte mít [Service Bus](https://azure.microsoft.com/services/service-bus/) účtu.  

Než v aplikaci logiky můžete použít váš účet Azure Service Bus, je nutné autorizovat sběrnice účet služby tooconnect tooyour hello logiku aplikace. Naštěstí můžete k tomu snadno z v rámci aplikace logiky na hello portálu Azure.  

Zde jsou kroky tooauthorize hello vaší logiku aplikace tooconnect tooyour Service Bus účet:  

1. Vyberte připojení tooService sběrnice, v návrháři aplikace logiky hello, toocreate **zobrazit Microsoft spravované rozhraní API** v rozevíracím seznamu hello. Potom zadejte **služby sběrnice** hello vyhledávacího pole. Vyberte hello aktivační události nebo akci, kterou toouse.  
    ![Obrázek připojení služby Service Bus 1](./media/connectors-create-api-servicebus/servicebus-1.png)  
2. Pokud jste nevytvořili žádné tooService připojení sběrnice před, budete mít výzvami tooprovide vaše pověření Service Bus. Tyto přihlašovací údaje jsou použité tooauthorize vaše logiku aplikace tooconnect tooand přístup k datům účtu služby Service Bus. konektor Service Bus Hello musí hello připojovací řetězec pro obor názvů sběrnice hello. Také budete potřebovat **spravovat** oprávnění. Tooknow vhodný způsob, pokud je váš připojovací řetězec pro obor názvů hello nebo konkrétní entitu je, pokud obsahuje hello `EntityPath` parametr. Pokud ano, není hello správný připojovací řetězec pro aplikace logiky.  
    ![Připojovací řetězec sběrnice služeb](./media/connectors-create-api-servicebus/connectionstring.png)
3. Po přijetí hello připojovací řetězec pro obor názvů hello můžete pro připojení k rozhraní API v Logic Apps hello.  
    ![Obrázek připojení služby Service Bus 2](./media/connectors-create-api-servicebus/servicebus-2.png)  
4. Všimněte si hello připojení bylo vytvořeno a jsou nyní volné tooproceed s hello další kroky v aplikaci logiky.  
    ![Obrázek připojení služby Service Bus 3](./media/connectors-create-api-servicebus/servicebus-3.png)   

