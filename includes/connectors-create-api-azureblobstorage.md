### <a name="prerequisites"></a>Požadavky
* Účet Azure; můžete vytvořit [bezplatný účet](https://azure.microsoft.com/free)
* [Účtu úložiště objektů Blob Azure](../articles/storage/common/storage-create-storage-account.md) včetně hello název účtu úložiště a jeho přístupový klíč. Tyto informace je uvedena v hello vlastnosti účtu úložiště hello v hello portálu Azure. Další informace o [Azure Storage](../articles/storage/common/storage-introduction.md).

Před použitím svého účtu úložiště objektů Blob Azure v aplikaci logiky, připojte tooyour účtu úložiště objektů Blob Azure. Můžete k tomu snadno v rámci aplikace logiky na hello portálu Azure.  

Připojte tooyour účtu Azure Blob Storage pomocí hello následující kroky:  

1. Vytvoření aplikace logiky. V Návrháři hello Logic Apps přidejte aktivační událost a potom přidat akci. Vyberte **zobrazit Microsoft spravované rozhraní API** v hello rozevíracím seznamu a potom zadejte do vyhledávacího pole hello "blob". Vyberte jednu z akcí hello:  
   
    ![Azure Blob Storage – krok vytvoření připojení](./media/connectors-create-api-azureblobstorage/azureblobstorage-1.png)  
2. Pokud jste dosud nevytvořili všechna připojení tooAzure úložiště, budete vyzváni k podrobnosti připojení hello:   
   
    ![Azure Blob Storage – krok vytvoření připojení](./media/connectors-create-api-azureblobstorage/connection-details.png)  
3. Zadejte podrobnosti o účtu úložiště hello. Vyžadují se vlastnosti s hvězdičkou.
   
   | Vlastnost | Podrobnosti |
   | --- | --- |
   | Název připojení * |Zadejte libovolný název pro připojení. |
   | Název účtu úložiště Azure * |Zadejte název účtu úložiště hello. název účtu úložiště Hello se zobrazí ve vlastnostech hello úložiště v hello portálu Azure. |
   | Přístupový klíč účtu úložiště Azure * |Zadejte klíč účtu úložiště hello. Hello přístupové klíče se zobrazí ve vlastnostech hello úložiště v hello portálu Azure. |
   
    Tyto přihlašovací údaje jsou použité tooauthorize vaše tooconnect logiku aplikace a přistupovat k datům. 
4. Vyberte **Vytvořit**.
5. Všimněte si hello připojení bylo vytvořeno. Teď pokračujte hello další kroky v aplikaci logiky: 
   
    ![Azure Blob Storage – krok vytvoření připojení](./media/connectors-create-api-azureblobstorage/azureblobstorage-3.png)  

