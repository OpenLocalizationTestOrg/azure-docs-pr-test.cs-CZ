---
title: "aaaSecuring data uložená v Azure Data Lake Store | Microsoft Docs"
description: "Zjistěte, jak řídit toosecure data v Azure Data Lake Store pomocí skupin a přístup seznamy"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: ca35e65f-3986-4f1b-bf93-9af6066bb716
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: 2b4ed7e322e1843ca47d6968ec8801ac19ea6399
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="securing-data-stored-in-azure-data-lake-store"></a>Zabezpečení dat uložených v Azure Data Lake Store
Zabezpečení dat v Azure Data Lake Store je přístup třech krocích.

1. Začněte vytvořením skupiny zabezpečení v Azure Active Directory (AAD). Tyto skupiny zabezpečení slouží tooimplement řízení přístupu na základě rolí (RBAC) na portálu Azure. Další informace najdete v části [řízení přístupu na základě rolí ve službě Microsoft Azure](../active-directory/role-based-access-control-configure.md).
2. Přiřaďte hello AAD zabezpečení skupiny toohello účtu Azure Data Lake Store. Tato volba určuje účtu Data Lake Store toohello přístup z hello portál a management operace z portálu hello nebo rozhraní API.
3. Přiřazení skupiny zabezpečení AAD hello jako seznamy řízení přístupu (ACL) v systému souborů hello Data Lake Store.
4. Kromě toho můžete také nastavit rozsah IP adres pro klienty, kteří mohou přistupovat k hello dat v Data Lake Store.

Tento článek obsahuje pokyny jak toouse hello Azure portálu tooperform hello výše úlohy. Podrobnější informace o tom, jak Data Lake Store implementuje zabezpečení na úrovni hello účet a data, najdete v části [zabezpečení v Azure Data Lake Store](data-lake-store-security-overview.md). Nabídnout detailní informace o tom, jak jsou implementované seznamy ACL v Azure Data Lake Store najdete v tématu [přehled o řízení přístupu v Data Lake Store](data-lake-store-access-control.md).

## <a name="prerequisites"></a>Požadavky
Než začnete tento kurz, musíte mít následující hello:

* **Předplatné Azure**. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Účet Azure Data Lake Store**. Návod, jak jeden, viz toocreate [Začínáme s Azure Data Lake Store](data-lake-store-get-started-portal.md)

## <a name="create-security-groups-in-azure-active-directory"></a>Vytvoření skupin zabezpečení v Azure Active Directory
Návod, jak skupin zabezpečení AAD toocreate a jak skupiny uživatelů toohello tooadd, najdete v části [Správa skupin zabezpečení v Azure Active Directory](../active-directory/active-directory-accessmanagement-manage-groups.md).

> [!NOTE] 
> Můžete přidat uživatele i jiné skupiny tooa skupiny ve službě Azure AD pomocí portálu Azure hello. Ale v pořadí tooadd skupinu hlavní tooa služby, použijte prosím [modulu Azure AD PowerShell](../active-directory/active-directory-accessmanagement-groups-settings-v2-cmdlets.md).
> 
> ```powershell
> # Get hello desired group and service principal and identify hello correct object IDs
> Get-AzureADGroup -SearchString "<group name>"
> Get-AzureADServicePrincipal -SearchString "<SPI name>"
> 
> # Add hello service principal toohello group
> Add-AzureADGroupMember -ObjectId <Group object ID> -RefObjectId <SPI object ID>
> ```
 
## <a name="assign-users-or-security-groups-tooazure-data-lake-store-accounts"></a>Přiřadit uživatele nebo skupiny zabezpečení tooAzure účty Data Lake Store
Když přiřadíte uživatele nebo skupiny zabezpečení účtů Data Lake Store tooAzure, můžete řídit přístup toohello operace správy v hello účet pomocí hello portál Azure a rozhraní API Správce Azure Resource Manager. 

1. Otevřete účet Azure Data Lake Store. V levém podokně hello, klikněte na **Procházet**, klikněte na tlačítko **Data Lake Store**a potom v okně Data Lake Store hello, klikněte na toowhich název účtu hello chcete tooassign uživatele nebo skupinu zabezpečení.

2. V okně Nastavení účtu Data Lake Store, klikněte na tlačítko **řízení přístupu (IAM)**. okno Hello výchozí seznamy **správci předplatného** skupiny jako vlastníka.
   
    ![Přiřazení účtu Data Lake Store tooAzure skupiny zabezpečení](./media/data-lake-store-secure-data/adl.select.user.icon.png "přiřadit účet Data Lake Store tooAzure skupiny zabezpečení")

    Existují dva způsoby tooadd skupiny a přiřadit příslušných rolí.
   
    * Přidat účet toohello uživatele nebo skupiny a pak mu přiřaďte roli, nebo
    * Přidání role a potom přiřadit toorole uživatele nebo skupiny.
     
    V této části se podíváme na prvním přístupem hello, přidání skupiny a pak přiřazení rolí. Můžete provést podobné kroky toofirst vyberte roli a potom přiřadit role toothat skupiny.
4. V hello **uživatelé** okně klikněte na tlačítko **přidat** tooopen hello **přidat přístup** okno. V hello **přidat přístup** okně klikněte na tlačítko **vyberte roli**a potom vyberte roli pro hello uživatele nebo skupiny.
   
    ![Přidání role pro uživatele hello](./media/data-lake-store-secure-data/adl.add.user.1.png "přidání role pro uživatele hello")
   
    Hello **vlastníka** a **Přispěvatel** role poskytují přístup tooa řadu funkcí správy v účtu data lake hello. Pro uživatele, kteří budou pracovat s daty v hello data lake, můžete je přidat toohello ** čtečky ** role. rozsah Hello tyto role je omezená toohello management operace související toohello účtu Azure Data Lake Store.
   
    Pro data oprávnění systému operations jednotlivých souborů definovat, co můžete dělat uživatelé hello. Proto uživatele s role Čtenář můžete pouze zobrazení nastavení pro správu přidružené k účtu hello, ale může potenciálně čtení a zápisu dat podle oprávnění systému souborů přiřazena toothem. Oprávnění systému souborů data Lake Store jsou popsány v [skupinu zabezpečení přiřadit jako seznamy ACL toohello systém souborů Azure Data Lake Store](#filepermissions).
5. V hello **přidat přístup** okně klikněte na tlačítko **přidat uživatele** tooopen hello **přidat uživatele** okno. V tomto okně vyhledejte hello skupiny zabezpečení, kterou jste vytvořili dříve v Azure Active Directory. Pokud máte spoustu skupin toosearch z, použijte hello textového pole v horní toofilter hello na název skupiny hello. Klikněte na **Vybrat**.
   
    ![Přidat skupinu zabezpečení](./media/data-lake-store-secure-data/adl.add.user.2.png "přidat skupinu zabezpečení")
   
    Pokud chcete tooadd skupinu nebo uživatele, který není uveden, můžete je pozvat pomocí hello **pozvat** ikonu a zadání hello e-mailovou adresu pro hello uživatele nebo skupiny.
6. Klikněte na **OK**. Měli byste vidět skupinu zabezpečení hello přidali, jak je uvedeno níže.
   
    ![Skupiny zabezpečení přidat](./media/data-lake-store-secure-data/adl.add.user.3.png "přidat skupinu zabezpečení")

7. Vaše skupiny zabezpečení uživatelů nebo má nyní přístup k účtu Azure Data Lake Store toohello. Pokud chcete, aby uživatelé toospecific tooprovide přístup, můžete je přidat skupinu zabezpečení toohello. Podobně pokud chcete, aby toorevoke přístupu pro uživatele, můžete je odebrat ze skupiny zabezpečení hello. Můžete také přiřadit víc skupin zabezpečení tooan účtu. 

## <a name="filepermissions"></a>Přiřadit uživatele nebo skupiny zabezpečení jako seznamy ACL toohello systém souborů Azure Data Lake Store
Po přiřazení skupiny zabezpečení uživatelů nebo systém souborů Azure Data Lake toohello, nastavit řízení přístupu na hello data uložená v Azure Data Lake Store.

1. V okně účtu Data Lake Store klikněte na možnost **Průzkumník dat**.
   
    ![Vytváření adresářů v účtu Data Lake Store](./media/data-lake-store-secure-data/adl.start.data.explorer.png "vytváření adresářů v účtu Data Lake")
2. V hello **Průzkumníku dat** okně klikněte na tlačítko hello souboru nebo složky, pro který chcete tooconfigure hello seznamu ACL a pak klikněte na tlačítko **přístup**. soubor tooa tooassign seznamu ACL, musíte kliknout na **přístup** z hello **náhled souboru** okno.
   
    ![Nastavit seznamy ACL v systému souborů Data Lake](./media/data-lake-store-secure-data/adl.acl.1.png "nastavit seznamy ACL v Data Lake systému souborů")
3. Hello **přístup** okno uvádí přístup hello standardní a vlastní přístup již přiřazen toohello kořenové. Klikněte na tlačítko hello **přidat** ikonu tooadd úrovni vlastní seznamy ACL.
   
    ![Standardní a vlastní přístup](./media/data-lake-store-secure-data/adl.acl.2.png "standardní a vlastní přístup")
   
   * **Standardní přístup** je hello stylu systému UNIX přístup, kde můžete určit čtení, zápisu, provést třídy jedinečných uživatelů toothree (rwx): vlastník, skupiny a dalších.
   * **Vlastní přístup** odpovídá toohello POSIX ACL, který vám umožní tooset oprávnění pro konkrétní jmenovaní uživatelé nebo skupiny a ne jen hello vlastníka souboru nebo skupiny. 
     
     Další informace najdete v tématu [seznamy ACL HDFS](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html#ACLs_Access_Control_Lists). Další informace o tom, jak jsou implementované seznamy ACL v Data Lake Store najdete v tématu [řízení přístupu v Data Lake Store](data-lake-store-access-control.md).
4. Klikněte na tlačítko hello **přidat** ikonu tooopen hello **přidat vlastní přístup** okno. V tomto okně klikněte na tlačítko **vybrat uživatele nebo skupinu**a potom v **vybrat uživatele nebo skupinu** okno, podívejte se na skupinu zabezpečení hello, kterou jste vytvořili dříve v Azure Active Directory. Pokud máte spoustu skupin toosearch z, použijte hello textového pole v horní toofilter hello na název skupiny hello. Klikněte na skupinu hello tooadd a pak klikněte na tlačítko **vyberte**.
   
    ![Přidat skupinu](./media/data-lake-store-secure-data/adl.acl.3.png "přidat skupinu")
5. Klikněte na tlačítko **vyberte oprávnění**, vyberte hello oprávnění a jestli chcete tooassign hello oprávnění jako výchozí seznam ACL, získat přístup k seznamu ACL, nebo obojí. Klikněte na **OK**.
   
    ![Přiřazení oprávnění toogroup](./media/data-lake-store-secure-data/adl.acl.4.png "přiřadit oprávnění toogroup")
   
    Další informace o oprávněních v Data Lake Store a seznamy ACL výchozí nebo přístupu najdete v tématu [řízení přístupu v Data Lake Store](data-lake-store-access-control.md).
6. V hello **přidat vlastní přístup** okně klikněte na tlačítko **OK**. Nově přidaná Hello skupiny s oprávněními hello spojené se teď uvedený v hello **přístup** okno.
   
    ![Přiřazení oprávnění toogroup](./media/data-lake-store-secure-data/adl.acl.5.png "přiřadit oprávnění toogroup")
   
   > [!IMPORTANT]
   > V aktuální verzi hello, může mít pouze 9 položky v rámci **vlastní přístup**. Pokud chcete tooadd více než 9 uživatelé, měli byste vytvořit skupiny zabezpečení přidat skupiny toosecurity uživatele, přidání poskytnout přístup skupiny zabezpečení toothose hello účtu Data Lake Store.
   > 
   > 
7. V případě potřeby můžete také upravit hello přístupová oprávnění, po přidání skupiny hello. Hello vyberte nebo zrušte zaškrtnutí políčka pro každé oprávnění typ (čtení, zápis, provést) založené na tom, zda chcete tooremove nebo přiřaďte této skupině zabezpečení oprávnění toohello. Klikněte na tlačítko **Uložit** toosave hello změny, nebo **zahodit** tooundo hello změny.

## <a name="set-ip-address-range-for-data-access"></a>Nastavit rozsah IP adres pro přístup k datům
Azure Data Lake Store umožňuje toofurther uzamčení úložiště dat tooyour přístupu na úrovni sítě. Můžete povolit bránu firewall, zadejte IP adresu nebo zadejte rozsah IP adres pro klienty důvěryhodné. Jakmile povolíte, můžete připojit pouze klienti, kteří mají hello IP adresy v definovaném rozsahu toohello úložiště.

![Nastavení brány firewall a IP přístup](./media/data-lake-store-secure-data/firewall-ip-access.png "nastavení a IP adresu brány Firewall")

## <a name="remove-security-groups-for-an-azure-data-lake-store-account"></a>Odebrání skupiny zabezpečení k účtu Azure Data Lake Store
Když odeberete skupin zabezpečení z účtů Azure Data Lake Store, měníte jenom operace správy toohello přístup na účet hello pomocí hello portálu Azure a rozhraní API Správce Azure Resource Manager.

1. V okně účtu Data Lake Store, klikněte na tlačítko **nastavení**. Z hello **nastavení** okně klikněte na tlačítko **uživatelé**.
   
    ![Přiřazení účtu Data Lake tooAzure skupiny zabezpečení](./media/data-lake-store-secure-data/adl.select.user.icon.png "přiřadit účet Data Lake tooAzure skupiny zabezpečení")
2. V hello **uživatelé** okně klikněte na skupinu zabezpečení hello chcete tooremove.
   
    ![Tooremove skupiny zabezpečení](./media/data-lake-store-secure-data/adl.add.user.3.png "tooremove skupiny zabezpečení")
3. V okně hello hello skupiny zabezpečení, klikněte na **odebrat**.
   
    ![Odebrat skupinu zabezpečení](./media/data-lake-store-secure-data/adl.remove.group.png "odebrat skupinu zabezpečení")

## <a name="remove-security-group-acls-from-azure-data-lake-store-file-system"></a>Odebrat skupinu zabezpečení seznamy ACL systému souborů Azure Data Lake Store
Když odeberete skupiny zabezpečení seznamy ACL systému souborů Azure Data Lake Store, můžete změnit přístup k datům toohello v hello Data Lake Store.

1. V okně účtu Data Lake Store klikněte na možnost **Průzkumník dat**.
   
    ![Vytváření adresářů v účtu Data Lake](./media/data-lake-store-secure-data/adl.start.data.explorer.png "vytváření adresářů v účtu Data Lake")
2. V hello **Průzkumníku dat** okně klikněte na tlačítko hello souboru nebo složky, pro které chcete tooremove hello seznamu ACL a klikněte v okně účtu, hello **přístup** ikonu. tooremove seznamu ACL pro soubor, musíte kliknout na **přístup** z hello **náhled souboru** okno.
   
    ![Nastavit seznamy ACL v systému souborů Data Lake](./media/data-lake-store-secure-data/adl.acl.1.png "nastavit seznamy ACL v Data Lake systému souborů")
3. V hello **přístup** okno z hello **vlastní přístup** klikněte na skupinu zabezpečení hello chcete tooremove. V hello **vlastní přístup** okně klikněte na tlačítko **odebrat** a pak klikněte na **OK**.
   
    ![Přiřazení oprávnění toogroup](./media/data-lake-store-secure-data/adl.remove.acl.png "přiřadit oprávnění toogroup")

## <a name="see-also"></a>Viz také
* [Přehled Azure Data Lake Store](data-lake-store-overview.md)
* [Kopírování dat z Azure úložiště objektů BLOB tooData Lake Store](data-lake-store-copy-data-azure-storage-blob.md)
* [Použití Azure Data Lake Analytics se službou Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Použití Azure HDInsight se službou Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)
* [Začínáme s Data Lake Store pomocí prostředí PowerShell](data-lake-store-get-started-powershell.md)
* [Začínáme s Data Lake Store pomocí sady .NET SDK](data-lake-store-get-started-net-sdk.md)
* [Zobrazení protokolů diagnostiky pro Data Lake Store](data-lake-store-diagnostic-logs.md)

