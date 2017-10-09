## <a name="what-are-service-bus-topics-and-subscriptions"></a>Co jsou témata a předplatné služby Service Bus?
Témata a předplatné služby Service Bus podporují komunikační model zasílání zpráv *publikování/přihlášení*. Součásti distribuované aplikace při používání témat a předplatných nekomunikují navzájem přímo. Místo toho si zprávy vyměňují prostřednictvím tématu, které slouží jako zprostředkovatel.

![TopicConcepts](./media/howto-service-bus-topics/sb-topics-01.png)

Na rozdíl od front služby Service Bus, ve kterých každou zprávu zpracuje jeden spotřebitel, témata a předplatné nabízejí komunikaci v podobě „1:N“ a používají vzor publikování/přihlášení. Je možné zaregistrovat několik předplatných tooa tématu. Při odeslání zprávy tooa tématu, je pak k proces toohandle předplatného dostupná tooeach nezávisle.

Téma tooa předplatného se podobá virtuální frontě, která obdrží kopii zprávy hello, které byly odeslány toohello tématu. Volitelně můžete zaregistrovat pravidla filtru pro téma na základě za předplatné, které umožňuje toofilter nebo omezit, které téma tooa zprávy jsou přijímány předplatných tématu.

Témata a odběry Service Bus umožňují tooscale a zpracování velkého počtu zpráv mnoha uživatelů a aplikací.

## <a name="create-a-namespace"></a>Vytvoření oboru názvů
toobegin používání témat a odběrů Service Bus v Azure, musíte nejdřív vytvořit *oboru názvů služby*. Obor názvů poskytuje kontejner oboru pro adresování prostředků služby Service Bus v rámci vaší aplikace.

toocreate obor názvů:

1. Přihlaste se toohello [portál Azure][Azure portal].
2. V levém navigačním podokně hello hello portálu, klikněte na **nový**, pak klikněte na tlačítko **Enterprise integrace**a potom klikněte na **Service Bus**.
3. V hello **vytvoření oboru názvů** dialogové okno, zadejte název oboru názvů. systém Hello toosee ověří okamžitě, pokud je k dispozici název hello.
4. Po provedení že název oboru názvů hello je k dispozici, zvolte hello cenová úroveň (Basic, Standard nebo Premium).
5. V hello **předplatné** pole, zvolte předplatné Azure v oboru názvů které toocreate hello.
6. V hello **skupiny prostředků** pole, vyberte existující skupinu prostředků v které hello obor názvů se za provozu nebo vytvořte novou.      
7. V **umístění**, zvolte hello zemi nebo oblast, ve kterém by hostovat vašeho oboru názvů.
   
    ![Vytvoření oboru názvů][create-namespace]
8. Klikněte na tlačítko hello **vytvořit** tlačítko. Hello systém teď vytvoří obor názvů a povolí ho. Toowait může mít několik minut hello systém zřídí prostředky pro váš účet.

### <a name="obtain-hello-credentials"></a>Získání přihlašovacích údajů hello
1. V seznamu hello oborů názvů klikněte na tlačítko hello nově vytvořený obor názvů.
2. V hello **oboru názvů Service Bus** okně klikněte na tlačítko **zásady sdíleného přístupu**.
3. V hello **zásady sdíleného přístupu** okně klikněte na tlačítko **RootManageSharedAccessKey**.
   
    ![connection-info][connection-info]
4. V hello **zásady: RootManageSharedAccessKey** okně klikněte na tlačítko Kopírovat hello další příliš**připojovací řetězec – primární klíč**, toocopy hello připojovací řetězec tooyour schránky pro pozdější použití.
   
    ![connection-string][connection-string]

[Azure portal]: https://portal.azure.com
[create-namespace]: ./media/howto-service-bus-topics/create-namespace.png
[connection-info]: ./media/howto-service-bus-topics/connection-info.png
[connection-string]: ./media/howto-service-bus-topics/connection-string.png


