---
title: "aaaOverview řízení přístupu v Data Lake Store | Microsoft Docs"
description: "Zde se dozvíte, jak funguje řízení přístupu v Azure Data Lake Store"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: d16f8c09-c954-40d3-afab-c86ffa8c353d
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/29/2017
ms.author: nitinme
ms.openlocfilehash: 1cc5d578f22ef0a123a1547abebfb4795ea09139
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="access-control-in-azure-data-lake-store"></a>Řízení přístupu v Azure Data Lake Store

Azure Data Lake Store implementuje model řízení přístupu, která je odvozena z HDFS, která zase je odvozena z model řízení přístupu POSIX hello. Tento článek obsahuje souhrn hello základy hello model řízení přístupu pro Data Lake Store. toolearn Další informace o hello HDFS model řízení přístupu, najdete v části [HDFS oprávnění průvodce](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html).

## <a name="access-control-lists-on-files-and-folders"></a>Seznamy řízení přístupu k souborům a složkám

Existují dva druhy seznamů řízení přístupu (ACL) – **přístupové seznamy ACL** a **výchozí seznamy ACL**.

* **Přístup k seznamy ACL**: objekt tooan přístup tyto ovládacího prvku. Přístupové seznamy ACL jsou definovány pro soubory i složky.

* **Výchozí seznamy ACL**: "Šablonu" seznamů ACL přidružené složce, která určit hello seznamy ACL přístupu pro všechny podřízené položky, které jsou vytvořené v této složce. Výchozí seznamy ACL nejsou definovány pro soubory.

![Seznamy ACL služby Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-1.png)

Seznamy ACL přístupu a výchozí seznamy ACL mají hello stejnou strukturu.

![Seznamy ACL služby Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-2.png)



> [!NOTE]
> Změna hello výchozí seznam ACL u nadřazené neovlivňuje hello přístupu ACL nebo výchozí seznam ACL podřízených položek, které již existují.
>
>

## <a name="users-and-identities"></a>Uživatelé a identity

Každý soubor a složka má samostatná oprávnění pro tyto identity:

* Hello vlastnícího uživatele hello souboru
* Hello vlastnícím skupiny
* Pojmenovaní uživatelé
* Pojmenované skupiny
* Všichni ostatní uživatelé

Hello identity uživatelů a skupin jsou identit Azure Active Directory (Azure AD). Takže pokud není uvedeno jinak, "user," v kontextu hello Data Lake Store, můžete buď znamenat uživatele Azure AD nebo skupiny zabezpečení služby Azure AD.

## <a name="permissions"></a>Oprávnění

Hello oprávnění pro objekt systému souborů jsou **čtení**, **zápisu**, a **Execute**, a použitím na soubory a složky, jak je znázorněno v následující tabulce hello:

|            |    File     |   Složka |
|------------|-------------|----------|
| **Číst (R)** | Může číst hello obsah souboru | Vyžaduje **čtení** a **Execute** toolist hello obsah složky hello|
| **Zapisovat (W)** | Můžete zapsat nebo připojit soubor tooa | Vyžaduje **zápisu** a **Execute** toocreate podřízené položky ve složce |
| **Provést (X)** | Neznamená nic v kontextu hello Data Lake Store | Požadované tootraverse hello podřízené položky složky |

### <a name="short-forms-for-permissions"></a>Zkrácené verze oprávnění

**RWX** je použité tooindicate **čtení a zápisu + provést**. Existuje více zhuštěné číselný tvar ve kterém **čtení = 4**, **zápisu = 2**, a **Execute = 1**, jejichž hello součet představuje hello oprávnění. Dále je uvedeno několik příkladů.

| Číselný tvar | Krátký tvar |      Význam     |
|--------------|------------|------------------------|
| 7            | RWX        | Číst + Zapisovat + Provést |
| 5            | R-X        | Číst + Provést         |
| 4            | R--        | Čtení                   |
| 0            | ---        | Žádná oprávnění         |


### <a name="permissions-do-not-inherit"></a>Oprávnění se nedědí

V modelu stylu POSIX hello, který je používán Data Lake Store oprávnění pro položku se ukládají na samotnou položku hello. Jinými slovy oprávnění pro položku nelze možné zdědit z nadřazené položky hello.

## <a name="common-scenarios-related-toopermissions"></a>Běžné scénáře související toopermissions

Tady jsou některé běžné scénáře toohelp porozumíte oprávnění, která jsou potřeba tooperform určité operace v účtu Data Lake Store.

### <a name="permissions-needed-tooread-a-file"></a>Oprávnění potřebná tooread soubor

![Seznamy ACL služby Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-3.png)

* Pro soubor toobe hello číst, hello volající musí **čtení** oprávnění.
* Pro všechny hello složkách v hello struktuře, které obsahují hello souboru, hello volající musí **Execute** oprávnění.

### <a name="permissions-needed-tooappend-tooa-file"></a>Oprávnění potřebná tooappend tooa souboru

![Seznamy ACL služby Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-4.png)

* Pro soubor toobe hello připojí se k němu, hello volající musí **zápisu** oprávnění.
* Pro všechny hello složek, které obsahují hello souboru, hello volající musí **Execute** oprávnění.

### <a name="permissions-needed-toodelete-a-file"></a>Oprávnění potřebná toodelete soubor

![Seznamy ACL služby Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-5.png)

* Hello nadřazené složky hello volající musí **zápisu + provést** oprávnění.
* Pro všechny hello jiné složky v cestě hello souboru, hello volající musí **Execute** oprávnění.



> [!NOTE]
> Zapsat oprávnění u souboru hello nejsou požadované toodelete ji, pokud hello předchozí dvě podmínky jsou splněny.
>
>

### <a name="permissions-needed-tooenumerate-a-folder"></a>Oprávnění potřebná tooenumerate složku

![Seznamy ACL služby Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-6.png)

* Pro složku tooenumerate hello, hello volající musí **čtení + Execute** oprávnění.
* Pro všechny hello nadřazené složky, hello volající musí **Execute** oprávnění.

## <a name="viewing-permissions-in-hello-azure-portal"></a>Oprávnění k zobrazení v hello portálu Azure

Z hello **Průzkumníku dat** okno hello účtu Data Lake Store, klikněte na tlačítko **přístup** toosee hello seznamy ACL na soubor nebo složku. Klikněte na tlačítko **přístup** toosee hello seznamy ACL pro hello **katalogu** ve složce hello **mydatastore** účtu.

![Seznamy ACL služby Data Lake Store](./media/data-lake-store-access-control/data-lake-store-show-acls-1.png)

V tomto okně hello horní části zobrazí přehled hello oprávnění, které máte. (Na snímku obrazovky hello hello uživatel je Bob.) Následující, která se zobrazí hello přístupová oprávnění. Potom z hello **přístup** okně klikněte na tlačítko **jednoduché zobrazení** toosee hello jednodušší zobrazení.

![Seznamy ACL služby Data Lake Store](./media/data-lake-store-access-control/data-lake-store-show-acls-simple-view.png)

Klikněte na tlačítko **rozšířené zobrazení** toosee hello pokročilejší zobrazení, kde jsou uvedeny koncepty hello výchozí seznamy ACL, maska a superuživatele.

![Seznamy ACL služby Data Lake Store](./media/data-lake-store-access-control/data-lake-store-show-acls-advance-view.png)

## <a name="hello-super-user"></a>Hello superuživatele

Nadřazený uživatel má většina práva všem uživatelům hello hello v hello Data Lake Store. Superuživatel:

* Má oprávnění RWX příliš**všechny** soubory a složky.
* Můžete změnit oprávnění hello na libovolný soubor nebo složku.
* Můžete změnit hello vlastnícího uživatele nebo který je vlastníkem skupiny libovolný soubor nebo složku.

V Azure má účet Data Lake Store několik rolí Azure, včetně rolí:

* Vlastníci
* Přispěvatelé
* Čtenáři

Všichni uživatelé v hello **vlastníky** role pro účet Data Lake Store je automaticky superuživatele pro tento účet. Další, najdete v části toolearn [řízení přístupu na základě Role](../active-directory/role-based-access-control-configure.md).
Pokud chcete toocreate roli vlastního ovládacího prvku na základě přístupu rolí (RBAC), který má oprávnění superuživatele, potřebuje toohave hello následující oprávnění:
- Microsoft.DataLakeStore/accounts/Superuser/action
- Microsoft.Authorization/roleAssignments/write


## <a name="hello-owning-user"></a>Hello vlastnícím uživatele

Hello uživatele, který vytvořil položku hello je automaticky hello vlastnícího uživatele hello položky. Vlastnící uživatel může:

* Změnit hello oprávnění k souboru, který je vlastněn.
* Změnit hello vlastnící skupinu souboru, který je vlastněn, tak dlouho, dokud uživatel vlastnícím hello je také členem hello cílovou skupinu.

> [!NOTE]
> Hello vlastnícího uživatele *nelze* změnit hello vlastnícího uživatele jiného ve vlastnictví souboru. Pouze nadtypem uživatelé mohou změnit hello vlastnící uživatel k souboru nebo složce.
>
>

## <a name="hello-owning-group"></a>Hello vlastnícím skupiny

V hello POSIX seznamy ACL každý uživatel je přidružen k "primární skupinu". Například uživatel "alice" může patřit toohello "finanční" skupiny. Alice můžou taky patřit toomultiple skupiny, ale jedna skupina je vždy určen jako svůj primární skupiny. V POSIX když Alice vytvoří soubor, nastavte hello vlastnící skupinu pro tohoto souboru je tooher primární skupinu, která je v tomto případě "finanční".

Při vytvoření nové položky filesystem Data Lake Store přiřadí hodnota toohello, který je vlastníkem skupiny.

* **Případ 1**: hello kořenové složky "/". Tato složka se vytvoří při vytvoření účtu Data Lake Store. V takovém případě hello vlastnícím skupina nastavení toohello uživatele, který vytvořil účet hello.
* **Případ 2** (každých dalších případ): při vytváření nové položky vlastnícím skupina hello se zkopíruje z hello nadřazené složky.

Hello vlastnícím skupiny může změnit:
* Všichni superuživatelé.
* Hello vlastnícího uživatele, pokud uživatel vlastnícím hello také členem hello cílovou skupinu.

## <a name="access-check-algorithm"></a>Algoritmus kontroly přístupu

Hello následující obrázek představuje hello přístupu zkontrolujte algoritmus pro účty Data Lake Store.

![Algoritmus seznamů ACL služby Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-algorithm.png)


## <a name="hello-mask-and-effective-permissions"></a>Maska Hello a "skutečná oprávnění"

Hello **maska** je RWX hodnoty používané toolimit přístup pro který je **s názvem Uživatelé**, hello **vlastnící skupinu**, a **s názvem skupiny** až budete algoritmus kontroly přístupu k provádění hello. Tady jsou klíčové koncepty hello pro masku hello.

* Maska Hello vytvoří "skutečná oprávnění." To znamená upraví hello oprávnění v době hello kontrolu přístupu.
* Maska Hello lze přímo upravit hello vlastníka souboru a všechny nadtypem uživatele.
* Maska Hello můžete odebrat oprávnění toocreate hello efektivní oprávnění. Maska Hello *nelze* přidejte oprávnění toohello efektivní oprávnění.

Podívejme se na několik příkladů. V následujícím příkladu hello, hello maska je nastaven příliš**RWX**, což znamená, že maska hello neodebere žádné oprávnění. Hello skutečná oprávnění pro uživatele, který vlastní skupinu a s názvem skupiny s názvem hello nejsou změněna během kontroly přístupu hello.

![Seznamy ACL služby Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-mask-1.png)

V následujícím příkladu hello, hello maska je nastaven příliš**R-X**. To znamená, že IT oddělení **vypne oprávnění k zápisu hello** pro **pojmenovaného uživatele**, **vlastnící skupinu**, a **s názvem skupiny** během hello přístupu Zkontrolujte.

![Seznamy ACL služby Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-mask-2.png)

Pro referenci tady je, kde se zobrazí hello masky pro soubor nebo složku v hello portálu Azure.

![Seznamy ACL služby Data Lake Store](./media/data-lake-store-access-control/data-lake-store-show-acls-mask-view.png)

> [!NOTE]
> Pro nový účet Data Lake Store, hello maska pro hello přístupu ACL a výchozí seznam ACL hello kořenové složky ("/"), použije výchozí hodnota tooRWX.
>
>

## <a name="permissions-on-new-files-and-folders"></a>Oprávnění pro nové soubory a složky

Při vytváření nového souboru nebo složky pod existující složku hello výchozí seznam ACL u hello nadřazené složky určuje:

- Výchozí seznam ACL a přístupový seznam ACL podřízené složky.
- Přístupový seznam ACL podřízeného souboru (pro soubory není definován výchozí seznam ACL).

### <a name="hello-access-acl-of-a-child-file-or-folder"></a>Hello přístupu ACL podřízené souboru nebo složky

Když je vytvořen podřízené souboru nebo složce, výchozí ACL hello nadřazeného objektu se zkopíruje hello přístupu ACL hello podřízené souboru nebo složce. Navíc pokud **jiných** uživatel má oprávnění RWX v nadřazené hello výchozí seznam ACL, odebere se ze seznamu ACL pro přístup k hello podřízenou položku.

![Seznamy ACL služby Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-child-items-1.png)

Ve většině scénářů hello předchozí informace je, že všechny budete potřebovat tooknow o tom, jak je určen přístupu ACL podřízenou položku. Ale pokud jste obeznámeni s POSIX systémy a toounderstand chcete podrobné, jak se dá dosáhnout Tato transformace, najdete v části hello [umask – na roli při vytváření hello přístupu ACL pro nové soubory a složky](#umasks-role-in-creating-the-access-acl-for-new-files-and-folders) dále v tomto článku.


### <a name="a-child-folders-default-acl"></a>Výchozí seznam ACL podřízené složky

Při vytváření podřízenou složku v rámci nadřazené složky ACL výchozí hello nadřazené složky se zkopíruje přes jako je seznam ACL výchozí toohello podřízenou složku.

![Seznamy ACL služby Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-child-items-2.png)

## <a name="advanced-topics-for-understanding-acls-in-data-lake-store"></a>Pokročilá témata pro pochopení seznamů ACL v Data Lake Store

Toto jsou některá Pokročilá témata toohelp víte, jak jsou seznamy ACL určit pro Data Lake Store soubory nebo složky.

### <a name="umasks-role-in-creating-hello-access-acl-for-new-files-and-folders"></a>Umask – na roli při vytváření hello přístupu ACL pro nové soubory a složky

V rámci standardu POSIX systému obecné koncept hello je, že umask – je hodnotou 9 bitů na hello nadřízené složky, která se používá tootransform hello oprávnění pro **vlastnícího uživatele**, **vlastnící skupinu**, a  **Další** na hello přístupu ACL nového podřízeného souboru nebo složky. bity Hello umask – identifikovat které tooturn bits mimo v seznamu ACL pro přístup k hello podřízenou položku. Proto se používá tooselectively zabránit hello šíření oprávnění pro **vlastnícího uživatele**, **vlastnící skupinu**, a **jiných**.

V systému HDFS umask – hello je obvykle možnosti konfigurace pro celý web, která je řízena správci. Data Lake Store používá funkci **umask v rámci účtu**, kterou nelze změnit. Následující tabulka ukazuje hello Hello odmaskujte pro Data Lake Store.

| Uživatelská skupina  | Nastavení | Vliv na přístupový seznam ACL nové podřízené položky |
|------------ |---------|---------------------------------------|
| Vlastnící uživatel | ---     | Žádný vliv                             |
| Vlastnící skupina| ---     | Žádný vliv                             |
| Ostatní       | RWX     | Odebrání oprávnění Číst + Zapisovat + Provést         |

Hello následující obrázek znázorňuje tuto umask – v akci. čistý efekt Hello je tooremove **čtení a zápisu + provést** pro **jiných** uživatele. Protože umask – hello nezadali bitů pro **vlastnícího uživatele** a **vlastnící skupinu**, nejsou transformovat tato oprávnění.

![Seznamy ACL služby Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-umask.png)

### <a name="hello-sticky-bit"></a>trvalé bit Hello

trvalé bit Hello je pokročilejší funkce POSIX systému souborů. V kontextu hello Data Lake Store není pravděpodobné, že hello trvalé bit bude potřeba.

Hello následující tabulka uvádí fungování trvalé bit hello v Data Lake Store.

| Uživatelská skupina         | File    | Složka |
|--------------------|---------|-------------------------|
| Bit sticky **VYPNUTÝ** | Žádný vliv   | Žádný vliv           |
| Bit sticky **ZAPNUTÝ**  | Žádný vliv   | Zabraňuje nikým kromě **nadtypem uživatelé** a hello **vlastnícího uživatele** podřízené položky v odstranění nebo přejmenování této podřízené položky.               |

trvalé bit Hello se nezobrazí v hello portálu Azure.

## <a name="common-questions-about-acls-in-data-lake-store"></a>Běžné otázky týkající se seznamů ACL ve službě Data Lake Store

Zde je uvedeno několik otázek, které se často vyskytují v souvislosti se seznamy ACL ve službě Data Lake Store.

### <a name="do-i-have-tooenable-support-for-acls"></a>Jsou tooenable podpora seznamů řízení přístupu?

Ne. Řízení přístupu prostřednictvím seznamů ACL je pro účet Data Lake Store vždy aktivní.

### <a name="which-permissions-are-required-toorecursively-delete-a-folder-and-its-contents"></a>Oprávnění, která jsou požadované toorecursively odstranit složku a její obsah?

* musí mít nadřazenou složku Hello **zápisu + provést** oprávnění.
* Hello toobe složky odstranit, a každé složky v něm, vyžaduje **čtení a zápisu + provést** oprávnění.

> [!NOTE]
> Není nutné oprávnění k zápisu toodelete soubory ve složkách. Navíc hello kořenové složky "/" můžete **nikdy** odstranit.
>
>

### <a name="who-is-hello-owner-of-a-file-or-folder"></a>Kdo je vlastníkem hello souboru nebo složky?

Hello tvůrce souboru nebo složky se změní na vlastníka hello.

### <a name="which-group-is-set-as-hello-owning-group-of-a-file-or-folder-at-creation"></a>Které skupiny je nastaven jako hello vlastnící skupinu souboru nebo složky při vytváření?

Skupina vlastnícím Hello se zkopíruje z hello vlastnící skupinu hello nadřazené složky pod které hello je vytvořen nový soubor nebo složku.

### <a name="i-am-hello-owning-user-of-a-file-but-i-dont-have-hello-rwx-permissions-i-need-what-do-i-do"></a>Jsem hello vlastnícího uživatele souboru, ale nemáte oprávnění RWX hello, které je potřeba. Co mám udělat?

Hello vlastnícím uživatel může změnit oprávnění hello hello souboru toogive sami RWX oprávnění, která potřebují.

### <a name="when-i-look-at-acls-in-hello-azure-portal-i-see-user-names-but-through-apis-i-see-guids-why-is-that"></a>Když se podívám na seznamy ACL v hello portál Azure zobrazena uživatelská jména, ale prostřednictvím rozhraní API, zobrazuje identifikátory GUID, proč je, že?

Položky v hello seznamy ACL jsou uloženy jako identifikátory GUID, které odpovídají toousers ve službě Azure AD. Hello rozhraní API vrátí hello identifikátory GUID, jako je. Hello portál Azure pokusí jednodušší toouse toomake seznamy řízení přístupu podle překládání hello identifikátory GUID do popisných názvů, pokud je to možné.

### <a name="why-do-i-sometimes-see-guids-in-hello-acls-when-im-using-hello-azure-portal"></a>Proč se někdy zobrazuje identifikátory GUID v ACL hello při používání hello portál Azure?

Identifikátor GUID se zobrazí, když uživatel hello neexistuje ve službě Azure AD už. Obvykle se to stane, když uživatel hello opustil hello společnosti, nebo pokud byl odstraněn svůj účet ve službě Azure AD.

### <a name="does-data-lake-store-support-inheritance-of-acls"></a>Podporuje služba Data Lake Store dědění seznamů ACL?

Ne.

### <a name="what-is-hello-difference-between-mask-and-umask"></a>Jaký je rozdíl hello maska a umask –?

| Vlastnost maska | Vlastnost umask|
|------|------|
| Hello **maska** vlastnost je k dispozici na všech souborů a složek. | Hello **umask –** je vlastností hello účtu Data Lake Store. Proto není pouze jeden umask – hello Data Lake Store.    |
| Vlastnost maska Hello k souboru nebo složce může být změněna pomocí hello vlastnícího uživatele nebo vlastnící skupinu pro soubor nebo nadtypem uživatele. | umask – vlastnost Hello nelze změnit žádný uživatel, i superuživatele. Tato hodnota je neměnná, konstantní.|
| Vlastnost maska Hello se používá během algoritmus kontrolu přístupu hello v modulu runtime toodetermine, zda uživatel má správné tooperform hello na operace na souboru nebo složce. role Hello hello masky je toocreate "skutečná oprávnění" v době hello kontrolu přístupu. | umask – Hello se nepoužívá během kontroly přístupu. umask – Hello je použité toodetermine hello přístupu ACL nové podřízené položky složky. |
| Hello maska je 3bitová RWX hodnotu, která se vztahuje toonamed uživatele, pojmenovanou skupinu a vlastnícím uživatelů v době hello kontrolu přístupu.| Hello umask – je 9 bitů hodnotu, která se vztahuje toohello vlastnící uživatel, který je vlastníkem skupiny, a **jiných** nového podřízeného.|

### <a name="where-can-i-learn-more-about-posix-access-control-model"></a>Kde najdu další informace o modelu řízení přístupu POSIX?

* [POSIX Access Control Lists on Linux (Seznamy řízení přístupu v rámci specifikace POSIX v Linuxu)](http://www.vanemery.com/Linux/ACL/POSIX_ACL_on_Linux.html)

* [HDFS Permissions Guide (Průvodce oprávněními v HDFS)](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html)

* [Nejčastější dotazy týkající se specifikace POSIX](http://www.opengroup.org/austin/papers/posix_faq.html)

* [POSIX 1003.1 2008](http://standards.ieee.org/findstds/standard/1003.1-2008.html)

* [POSIX 1003.1 2013](http://pubs.opengroup.org/onlinepubs/9699919799.2013edition/)

* [POSIX 1003.1 2016](http://pubs.opengroup.org/onlinepubs/9699919799.2016edition/)

* [POSIX ACL na Ubuntu](https://help.ubuntu.com/community/FilePermissionsACLs)

* [ACL: Using Access Control Lists on Linux (Seznamy ACL: Používání seznamů řízení přístupu v Linuxu)](http://bencane.com/2012/05/27/acl-using-access-control-lists-on-linux/)

## <a name="see-also"></a>Viz také

* [Přehled Azure Data Lake Storu](data-lake-store-overview.md)
