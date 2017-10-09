1. Klikněte na tlačítko hello **nový** nalezeno tlačítko na hello levém horním rohu hello portálu Azure.

1. Klikněte na **Vypočítat** > **Function App** a vyberte svoje **Předplatné**. Potom použijte nastavení aplikace hello funkce jako tabulka hello.

    ![Vytvoření aplikace pro funkce v hello portálu Azure](./media/functions-create-function-app-portal/function-app-create-flow.png)

    | Nastavení      | Navrhovaná hodnota  | Popis                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Název aplikace** | Globálně jedinečný název | Název identifikující novou aplikaci Function App. | 
    | **[Skupina prostředků](../articles/azure-resource-manager/resource-group-overview.md)** |  myResourceGroup | Název pro novou skupinu prostředků hello v které toocreate funkce aplikace. | 
    | **[Plán hostování](../articles/azure-functions/functions-scale.md)** |   Plán Consumption | Hostování plán, který definuje, jak prostředky se přidělují tooyour funkce aplikace. Ve výchozím nastavení hello **plánování spotřeba**, prostředky se přidají dynamicky podle požadavků vaší funkce. Platíte jenom dobu hello spuštění funkcí.   |
    | **Umístění** | Západní Evropa | Vyberte umístění ve vaší blízkosti nebo v blízkosti jiných služeb, ke kterým budou funkce získávat přístup. |
    | **[Účet úložiště](../articles/storage/common/storage-create-storage-account.md#create-a-storage-account)** |  Globálně jedinečný název |  Název nového účtu úložiště hello používá funkce aplikace. Názvy účtů úložiště musí mít od 3 do 24 znaků a můžou obsahovat jenom číslice a malá písmena. Můžete taky použít existující účet. |

1. Klikněte na tlačítko **vytvořit** tooprovision a nasaďte novou aplikaci funkce hello.
