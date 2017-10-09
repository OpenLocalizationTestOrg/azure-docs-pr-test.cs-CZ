## <a name="create-an-iot-hub"></a>Vytvoření centra IoT
Vytvoření služby IoT hub pro vaše aplikace tooconnect simulované zařízení k. Hello následující kroky vám ukážou, jak toocomplete tato úloha pomocí portálu Azure pomocí hello.

1. Přihlaste se toohello [portál Azure][lnk-portal].
1. V hello vlevo, klikněte na **nový** > **Internet věcí** > **IoT Hub**.
   
    ![Panel portálu Azure][1]
1. V hello **služby IoT hub** okně zvolte hello konfiguraci pro službu IoT hub.
   
    ![Okno centra IoT][2]
   
   1. V hello **název** pole, zadejte název své služby IoT hub. Pokud hello **název** je platný a dostupný, zobrazí se zelená značka zaškrtnutí v hello **název** pole.
    [!INCLUDE [iot-hub-pii-note-naming-hub](iot-hub-pii-note-naming-hub.md)]
   
   1. Vyberte [cenovou a škálovací úroveň][lnk-pricing]. Tento kurz nevyžaduje konkrétní úroveň. V tomto kurzu použijte bezplatnou úroveň F1 hello.
   1. V části **Skupina prostředků** vytvořte skupinu prostředků nebo vyberte existující skupinu. Další informace najdete v tématu [pomocí prostředků skupiny prostředků Azure toomanage][lnk-resource-groups].
   1. V **umístění**, vyberte hello toohost umístění služby IoT hub. V tomto kurzu zvolte nejbližší umístění.
1. Po výběru možností konfigurace centra IoT klikněte na **Vytvořit**.  Může trvat několik minut, než Azure toocreate IoT hub. Stav hello toocheck, můžete sledovat průběh hello hello úvodním panelu nebo panelu oznámení hello.
   
    ![Stav nového centra IoT][3]
1. Po úspěšném vytvoření služby IoT hub hello, klikněte na hello dlaždici nového centra IoT hello Azure portálu tooopen hello okně hello novou službu IoT hub. Poznamenejte si hello **Hostname**a potom klikněte na **zásady sdíleného přístupu**.
   
    ![Okno nového centra IoT][4]
1. V hello **zásady sdíleného přístupu** okně klikněte na tlačítko hello **iothubowner** zásady a pak zkopírujte a poznamenejte připojovací řetězec služby IoT Hub hello hello **iothubowner** okno. Další informace najdete v tématu [řízení přístupu] [ lnk-access-control] v hello "Příručka vývojáře IoT Hub."
   
    ![Okno zásad sdíleného přístupu][5]

<!-- Images. -->
[1]: ./media/iot-hub-get-started-create-hub/create-iot-hub1.png
[2]: ./media/iot-hub-get-started-create-hub/create-iot-hub2.png
[3]: ./media/iot-hub-get-started-create-hub/create-iot-hub3.png
[4]: ./media/iot-hub-get-started-create-hub/create-iot-hub4.png
[5]: ./media/iot-hub-get-started-create-hub/create-iot-hub5.png

<!-- Links -->
[lnk-resource-groups]: ../articles/azure-resource-manager/resource-group-portal.md
[lnk-portal]: https://portal.azure.com/
[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub/
[lnk-access-control]: ../articles/iot-hub/iot-hub-devguide-security.md
