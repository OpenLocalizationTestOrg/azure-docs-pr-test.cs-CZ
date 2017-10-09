### <a name="prerequisites"></a>Požadavky
* Účet Twilio
* Ověřené telefonní číslo Twilio, který může přijímat zprávy SMS
* Ověřené Twilio telefonní číslo, které může poslat SMS

> [!NOTE]
> Pokud používáte zkušební účet Twilio, lze odeslat pouze SMS příliš**ověřit** telefonních čísel.  
> 
> 

Než účtu Twilio můžete použít v aplikaci logiky, musíte je nejdříve autorizovat hello logiku aplikace tooconnect tooyour Twilio účtu. Naštěstí můžete k tomu snadno z v rámci aplikace logiky na hello portálu Azure. 

Zde jsou kroky tooauthorize hello vaší logiku aplikace tooconnect tooyour Twilio účet:

1. Vyberte připojení tooTwilio, v návrháři aplikace logiky hello, toocreate **zobrazit Microsoft spravované rozhraní API** v hello rozevíracím seznamu a potom zadejte *Twilio* hello vyhledávacího pole. Vyberte hello aktivační události nebo akce, budete jako toouse:  
   ![](./media/connectors-create-api-twilio/twilio-0.png)
2. Pokud jste nevytvořili žádné připojení tooTwilio před, získáte výzvami tooprovide přihlašovacích údajů Twilio. Tyto přihlašovací údaje bude použité tooauthorize vaše tooconnect aplikace logiky k a získat přístup k datům účtu Twilio:  
   ![](./media/connectors-create-api-twilio/twilio-1.png)  
3. Budete potřebovat hello **id účtu Twilio** a **Twilio přístupový token** z řídicího panelu hello v Twilio, takže přihlásit tooyour Twilio účet teď toograb tyto dva údaje:  
   ![](./media/connectors-create-api-twilio/twilio-2.png)  
4. Twilio a logiku aplikace použít odlišné názvy tooidentify tyto dvě údaje. Zde je, jak je nutné mapovat je dialogové okno aplikace logiky toohello:![](./media/connectors-create-api-twilio/twilio-3.png)  
5. Vyberte hello **vytvořit připojení** tlačítko:  
   ![](./media/connectors-create-api-twilio/twilio-4.png)
6. Všimněte si hello připojení bylo vytvořeno a jste nyní volné tooproceed s hello další kroky v aplikaci logiky:  
   ![](./media/connectors-create-api-twilio/twilio-5.png)

