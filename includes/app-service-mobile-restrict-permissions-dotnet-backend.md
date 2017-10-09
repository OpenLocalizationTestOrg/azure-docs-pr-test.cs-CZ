
Ve výchozím nastavení můžete rozhraní API v back-end mobilní aplikace volá anonymně. Dále musíte toorestrict přístup tooonly ověření klientů.  

* **Node.js zpět ukončení (prostřednictvím portálu Azure hello)** :  

    V nastavení mobilní aplikace, klikněte na tlačítko **snadno tabulky** a vyberte tabulku. Klikněte na tlačítko **změnit oprávnění**, vyberte **ověřený přístup pouze** pro všechna oprávnění a pak klikněte na tlačítko **Uložit**.
* **Rozhraní .NET back-end (C#)**:  

    V projektu server hello přejděte příliš**řadiče** > **TodoItemController.cs**. Přidat hello `[Authorize]` atribut toohello **TodoItemController** třídy následujícím způsobem. toorestrict přístup pouze toospecific metod, můžete taky použít tyto metody jenom toothose atribut místo hello třídy. Znovu publikujte hello serverový projekt.

        [Authorize]
        public class TodoItemController : TableController<TodoItem>

* **Back-end Node.js (prostřednictvím kódu Node.js)** :  

    toorequire ověřování pro přístup k tabulce, přidejte následující řádek toohello Node.js serveru skriptu hello:

        table.access = 'authenticated';

    Další podrobnosti najdete v tématu [postupy: ověřování vyžadovat pro přístup k tootables](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#howto-tables-auth). jak toodownload hello rychlé spuštění kódu projektu z vašeho webu, najdete v části toolearn [postupy: stažení hello Node.js back-end rychlé spuštění kódu projektu pomocí Git](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart).
