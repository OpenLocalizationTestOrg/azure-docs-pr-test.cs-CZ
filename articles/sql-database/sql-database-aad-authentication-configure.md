---
title: "ověřování služby Azure Active Directory aaaConfigure - SQL | Microsoft Docs"
description: "Zjistěte, jak tooconnect tooSQL databáze a SQL Data Warehouse pomocí ověřování Azure Active Directory."
services: sql-database
documentationcenter: 
author: BYHAM
manager: jhubbard
editor: 
tags: 
ms.assetid: 7e2508a1-347e-4f15-b060-d46602c5ce7e
ms.service: sql-database
ms.custom: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 07/10/2017
ms.author: rickbyh
ms.openlocfilehash: d6222da0b840f96d4bcfbc02964dc7c54d5ea1ee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-and-manage-azure-active-directory-authentication-with-sql-database-or-sql-data-warehouse"></a>Konfigurovat a spravovat ověřování Azure Active Directory s SQL Database nebo SQL Data Warehouse

Tento článek ukazuje, jak toocreate a naplnit Azure AD a potom pomocí služby Azure AD s Azure SQL Database a SQL Data Warehouse. Přehled najdete v tématu [ověřování Azure Active Directory](sql-database-aad-authentication.md).

>  [!NOTE]  
>  Připojení tooSQL Server běžící na virtuálním počítači Azure se nepodporuje, pomocí účtu Azure Active Directory. Místo toho používejte doménu účtu služby Active Directory.

## <a name="create-and-populate-an-azure-ad"></a>Vytvořit a naplnit Azure AD
Vytvoření Azure AD a jeho naplnění uživatelů a skupin. Azure AD může být spravované doméně hello počáteční domény Azure AD. Azure AD může být také místní Active Directory Domain Services, je sdružených se službou hello Azure AD.

Další informace najdete v tématu [integrace místních identit s Azure Active Directory](../active-directory/active-directory-aadconnect.md), [přidat vlastní tooAzure název domény AD](../active-directory/active-directory-add-domain.md), [Microsoft Azure teď podporuje federaci se Windows Server Active Directory](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/), [Správa adresáře služby Azure AD](https://msdn.microsoft.com/library/azure/hh967611.aspx), [Správa Azure AD pomocí prostředí Windows PowerShell](/powershell/azure/overview?view=azureadps-2.0), a [hybridní identita Požadované porty a protokoly](../active-directory/active-directory-aadconnect-ports.md).

## <a name="optional-associate-or-change-hello-active-directory-that-is-currently-associated-with-your-azure-subscription"></a>Volitelné: Přidružení nebo změňte hello služby active directory, který je aktuálně přidružena předplatného Azure
tooassociate databáze s adresářem hello Azure AD pro vaši organizaci, aby adresář hello důvěryhodné adresář pro databázi hostování hello hello předplatného Azure. Další informace najdete v článku [Asociování předplatných Azure se službou Azure AD](https://msdn.microsoft.com/library/azure/dn629581.aspx).

**Další informace:** předplatné každých Azure má vztah důvěryhodnosti s instancí Azure AD. To znamená, že tento adresář tooauthenticate uživatelů, služeb a zařízení. Několik předplatných může důvěřovat hello stejný adresář, ale předplatné důvěřuje pouze jednomu adresáři. Můžete zobrazit adresář, kterému důvěřují vaše předplatné v části hello **nastavení** kartě v [https://manage.windowsazure.com/](https://manage.windowsazure.com/). Tento vztah důvěryhodnosti, který má předplatné s adresářem je na rozdíl od hello vztah, který má předplatné se všemi ostatními prostředky v Azure (weby, databáze a tak dále), které jsou spíše podřízené prostředky předplatného. Pokud vyprší platnost předplatného a pak přístup toothose další prostředky přidružené k předplatnému hello také zastaví. Ale hello directory zůstává v Azure, a můžete přidružit jiné předplatné adresáře a pokračovat toomanage hello adresáře uživatelů. Další informace o prostředcích najdete v tématu [Principy přístupu k prostředkům v Azure](https://msdn.microsoft.com/library/azure/dn584083.aspx).

Hello následující postupy popisují, jak toochange hello přidružené adresář pro dané předplatné.
1. Připojit tooyour [portálu Azure Classic](https://manage.windowsazure.com/) pomocí Správce předplatného Azure.
2. Na levém banner hello, vyberte **nastavení**.
3. V úvodní obrazovka nastavení se zobrazí vaše předplatná. Podle potřeby hello předplatného se nezobrazí, klikněte na tlačítko **odběry** hello horní rozevírací nabídku hello **filtr podle DIRECTORY** pole a vyberte hello adresář, který obsahuje předplatné a pak klikněte na **Použít**.
   
    ![Vyberte předplatné][4]
4. V hello **nastavení** oblast, klikněte na vaše předplatné a pak klikněte na **upravit adresář** na hello dolní části stránky hello.
   
    ![portálu nastavení AD][5]
5. V hello **upravit adresář** vyberte hello Azure Active Directory, který je přidružen k serveru SQL Server nebo SQL Data Warehouse a potom klikněte na šipku další, hello.
   
    ![Vyberte možnost Upravit adresář][6]
6. V hello **POTVRDIT** directory dialogové okno mapování, zkontrolujte, že "**se odeberou všechny spolusprávci.**"
   
    ![Upravit adresář potvrzení][7]
7. Klikněte na tlačítko hello kontrola tooreload hello portálu.

   > [!NOTE]
   > Když změníte hello adresáře, přístup tooall spolusprávci, Azure AD Uživatelé a skupiny a uživatelů zálohovaná directory prostředků se odeberou a už mají přístup toothis předplatného nebo jeho prostředky. Pouze, jako správce služeb můžete nakonfigurovat přístup pro objekty zabezpečení založené na nový adresář hello. Tato změna může trvat vyžadovat značné množství času toopropagate tooall prostředky. Změna adresáře hello, také změny hello správce Azure AD pro SQL Database a SQL Data Warehouse a zakáže přístup k databázi pro všechny stávající uživatele Azure AD. Dobrý den Azure AD, Správce musí být resetování (jak je popsáno níže) a nové Azure AD Uživatelé musí být vytvořen.
   >  

## <a name="create-an-azure-ad-administrator-for-azure-sql-server"></a>Vytvoření správce Azure AD pro server Azure SQL
Každý server Azure SQL (který je hostitelem SQL Database nebo SQL Data Warehouse) spustí pomocí účtu správce jeden server, který je správcem hello hello celý server Azure SQL. Druhý správce systému SQL Server musí být vytvořeny, který je účet Azure AD. Tento objekt se vytvoří jako databáze s omezením uživatel v hlavní databázi hello. Jako správce, jsou účty správce systému server hello členy hello **db_owner** role v každého uživatele databáze a potom zadejte každou databázi uživatele jako hello **dbo** uživatele. Další informace o účtech správce serveru hello najdete v tématu [Správa databází a přihlašovacích údajů ve službě Azure SQL Database](sql-database-manage-logins.md).

Při použití Azure Active Directory s geografickou replikací, hello správce Azure Active Directory musí být nakonfigurované hello primární a sekundární servery hello. Pokud server nemá správce Azure Active Directory, pak přihlášení Azure Active Directory a uživatelé k chybě "Nelze se připojit" tooserver.

> [!NOTE]
> Uživatelé, které nejsou založené na účet služby Azure AD (včetně účtu správce serveru Azure SQL hello), nelze vytvořit Azure AD na základě uživatele, protože nemají oprávnění toovalidate navrhované uživatelé databáze s hello Azure AD.
> 

## <a name="provision-an-azure-active-directory-administrator-for-your-azure-sql-server"></a>Zřízení správcem služby Azure Active Directory pro server Azure SQL

Hello následující dva postupy ukazují, jak tooprovision správce Azure Active Directory pro server Azure SQL v hello portál Azure a pomocí prostředí PowerShell.

### <a name="azure-portal"></a>portál Azure
1. V hello [portál Azure](https://portal.azure.com/), v pravém horním rohu text hello, klikněte na vaše připojení toodrop dolů seznam možných Active Directory. Zvolte hello opravte služby Active Directory jako výchozí hello Azure AD. Používá se tento krok odkazy hello předplatné přidružení se službou Active Directory s Azure SQL serveru a ujistěte se, který hello stejného předplatného pro Azure AD a SQL Server. (server Azure SQL hello můžete hostování, Azure SQL Database nebo Azure SQL Data Warehouse.)   
    ![Zvolte ad][8]   
    
2. V hello vlevo vyberte banner **servery SQL**, vyberte vaše **systému SQL server**a potom v hello **systému SQL Server** okně klikněte na tlačítko **správce Active Directory**.   
3. V hello **správce Active Directory** okně klikněte na tlačítko **nastavit správce**.   
    ![Vyberte možnost active directory](./media/sql-database-aad-authentication/select-active-directory.png)  
    
4. V hello **přidat správce** , vyhledejte uživatele, vyberte hello uživatele nebo skupiny toobe správce a pak klikněte na tlačítko **vyberte**. (okno Správce služby Active Directory hello ukazuje všechny členy a skupin služby Active Directory. Uživatelé nebo skupiny, které jsou zobrazena šedě nelze vybrat, protože nejsou podporovány jako správci služby Azure AD. (Viz seznam hello podporované správců v hello **funkce Azure AD a omezení** části [použití Azure ověřování služby Active Directory k ověřování připojení k SQL Database nebo SQL Data Warehouse](sql-database-aad-authentication.md).) Řízení přístupu na základě role (RBAC) se vztahuje pouze toohello portálu a není šířený tooSQL serveru.   
    ![Vyberte správce](./media/sql-database-aad-authentication/select-admin.png)  
    
5. Hello horní části hello **správce Active Directory** okně klikněte na tlačítko **Uložit**.   
    ![Uložit správce](./media/sql-database-aad-authentication/save-admin.png)   

proces Hello změny hello správce může trvat několik minut. Pak hello nový správce se zobrazí v hello **správce Active Directory** pole.

   > [!NOTE]
   > Při nastavování správce hello Azure AD, hello nové jméno správce (uživatele či skupinu) nemůže být již přítomny v hlavní databázi hello virtuální jako uživatel ověřování systému SQL Server. Pokud existuje, se nezdaří instalace správce Azure AD hello; vrácení zpět jeho vytvoření a označující, že takový správce (název) již existuje. Vzhledem k tomu, že takové systému SQL Server ověřování uživatele není součástí hello Azure AD, všechny úsilí tooconnect toohello serveru pomocí ověřování Azure AD se nezdaří.
   > 


toolater odebrat správce, hello horní části hello **správce Active Directory** okně klikněte na tlačítko **odebrat správce**a potom klikněte na **Uložit**.

### <a name="powershell"></a>PowerShell
toorun rutiny prostředí PowerShell, je nutné toohave prostředí Azure PowerShell nainstalovaná a spuštěná. Podrobné informace najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).

tooprovision správce Azure AD, spusťte následující příkazy prostředí Azure PowerShell hello:

* Add-AzureRmAccount
* Select-AzureRmSubscription

Rutiny používá tooprovision a spravovat správce Azure AD:

| Název rutiny | Popis |
| --- | --- |
| [Set-AzureRmSqlServerActiveDirectoryAdministrator](/powershell/module/azurerm.sql/set-azurermsqlserveractivedirectoryadministrator) |Zřídí správce Azure Active Directory pro server Azure SQL nebo Azure SQL Data Warehouse. (Musí být z aktuálního předplatného hello.) |
| [Odebrat AzureRmSqlServerActiveDirectoryAdministrator](/powershell/module/azurerm.sql/remove-azurermsqlserveractivedirectoryadministrator) |Odebere správce Azure Active Directory pro server Azure SQL nebo Azure SQL Data Warehouse. |
| [Get-AzureRmSqlServerActiveDirectoryAdministrator](/powershell/module/azurerm.sql/get-azurermsqlserveractivedirectoryadministrator) |Vrací informace o aktuálně nakonfigurován pro server Azure SQL hello nebo Azure SQL Data Warehouse správce Azure Active Directory. |

Pomocí prostředí PowerShell příkaz get-help toosee další podrobnosti pro každou z těchto příkazů, například ``get-help Set-AzureRmSqlServerActiveDirectoryAdministrator``.

Hello následující skript zřizuje skupině Správce služby Azure AD s názvem **DBA_Group** (id objektu `40b79501-b343-44ed-9ce7-da4c8cc7353f`) pro hello **demo_server** serveru ve skupině prostředků s názvem **skupiny 23** :

```
Set-AzureRmSqlServerActiveDirectoryAdministrator -ResourceGroupName "Group-23"
-ServerName "demo_server" -DisplayName "DBA_Group"
```

Hello **DisplayName** vstupní parametr přijímá hello Azure AD zobrazovaný název nebo hello hlavní název uživatele. Například ``DisplayName="John Smith"`` a ``DisplayName="johns@contoso.com"``. Zobrazovaný název služby Azure AD skupiny pouze hello Azure AD je podporováno.

> [!NOTE]
> Hello prostředí Azure PowerShell příkaz ```Set-AzureRmSqlServerActiveDirectoryAdministrator``` nezabrání zřizování správci Azure AD pro nepodporovaný uživatele. Nepodporované uživatele může být zřízen, ale nemůže připojit databázi tooa. 
> 

Hello následující příklad používá hello volitelné **ObjectID**:

```
Set-AzureRmSqlServerActiveDirectoryAdministrator -ResourceGroupName "Group-23"
-ServerName "demo_server" -DisplayName "DBA_Group" -ObjectId "40b79501-b343-44ed-9ce7-da4c8cc7353f"
```

> [!NOTE]
> Hello Azure AD **ObjectID** je požadován při hello **DisplayName** není jedinečný. tooretrieve hello **ObjectID** a **DisplayName** hodnoty, použijte hello služby Active Directory část portálu Azure Classic a zobrazit vlastnosti hello uživatele nebo skupiny.
> 

Následující příklad vrátí informace o aktuální Azure AD Dobrý den, správce pro server Azure SQL Hello:

```
Get-AzureRmSqlServerActiveDirectoryAdministrator -ResourceGroupName "Group-23" -ServerName "demo_server" | Format-List
```

Následující ukázka Hello odebere správce Azure AD:

```
Remove-AzureRmSqlServerActiveDirectoryAdministrator -ResourceGroupName "Group-23" -ServerName "demo_server"
```

Můžete také vytvářet správce Azure Active Directory pomocí hello rozhraní REST API. Další informace najdete v tématu [odkaz na službu správy REST API a operace pro operace databáze SQL Azure pro databáze SQL Azure](https://msdn.microsoft.com/library/azure/dn505719.aspx)

### <a name="cli"></a>Rozhraní příkazového řádku  
Správce služby Azure AD můžete také vytvářet pomocí volání hello následující příkazy rozhraní příkazového řádku:
| Příkaz | Popis |
| --- | --- |
|[vytvoření ad správce az sql serveru](https://docs.microsoft.com/cli/azure/sql/server/ad-admin#create) |Zřídí správce Azure Active Directory pro server Azure SQL nebo Azure SQL Data Warehouse. (Musí být z aktuálního předplatného hello.) |
|[AZ sql server ad správce odstranit](https://docs.microsoft.com/cli/azure/sql/server/ad-admin#delete) |Odebere správce Azure Active Directory pro server Azure SQL nebo Azure SQL Data Warehouse. |
|[AZ sql server ad správce seznamu](https://docs.microsoft.com/cli/azure/sql/server/ad-admin#list) |Vrací informace o aktuálně nakonfigurován pro server Azure SQL hello nebo Azure SQL Data Warehouse správce Azure Active Directory. |
|[aktualizace ad správce az sql server](https://docs.microsoft.com/cli/azure/sql/server/ad-admin#update) |Aktualizace správce Active Directory hello pro server Azure SQL nebo Azure SQL Data Warehouse. |

Další informace o rozhraní příkazového řádku najdete v tématu [SQL - az sql](https://docs.microsoft.com/cli/azure/sql/server).  


## <a name="configure-your-client-computers"></a>Konfigurace klientských počítačů
Na všechny klientské počítače, ze kterých vaše aplikace nebo uživatele připojení tooAzure SQL Database nebo Azure SQL Data Warehouse pomocí Azure AD identity, musíte nainstalovat následující software hello:

* Rozhraní .NET framework 4.6 nebo novější z [https://msdn.microsoft.com/library/5a4x27ek.aspx](https://msdn.microsoft.com/library/5a4x27ek.aspx).
* Knihovna ověřování Azure Active Directory pro SQL Server (**ADALSQL. Knihovny DLL**) je k dispozici v několika jazycích (x86 a amd64) hello stažení softwaru prostřednictvím [Microsoft Active Directory Authentication Library pro Microsoft SQL Server](http://www.microsoft.com/download/details.aspx?id=48742).

Můžete splňovat tyto požadavky podle:

* Instalace buď [SQL Server 2016 Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) nebo [SQL Server Data Tools pro Visual Studio 2015](https://msdn.microsoft.com/library/mt204009.aspx) hello splňuje požadavek na rozhraní .NET Framework 4.6.
* Aplikace SSMS nainstaluje verzi hello x86 **ADALSQL. Knihovny DLL**.
* Rozšíření SSDT nainstaluje verzi amd64 hello **ADALSQL. Knihovny DLL**.
* Hello nejnovější sadu Visual Studio pomocí [Visual Studio stáhne](https://www.visualstudio.com/downloads/download-visual-studio-vs) splňuje požadavek na hello rozhraní .NET Framework 4.6, ale nenainstaluje verze požadované amd64 hello **ADALSQL. Knihovny DLL**.

## <a name="create-contained-database-users-in-your-database-mapped-tooazure-ad-identities"></a>Vytvoření uživatele databáze s omezením ve vaší databázi namapované tooAzure identit AD

Ověřování Azure Active Directory vyžaduje toobe uživatelé databázi vytvořit, protože uživatelé databáze s omezením. Uživatel databáze, která nemá přihlášení v hlavní databázi hello a které mapy tooan identity v hello adresář Azure AD, který je přidružen hello databáze je uživatel databáze s omezením na základě identity služby Azure AD. Hello identit Azure AD může být buď samostatný účet nebo skupinu. Další informace o uživatele databáze s omezením naleznete v tématu [obsažené uživatelů databáze provádění vaše přenosné databáze](https://msdn.microsoft.com/library/ff929188.aspx).

> [!NOTE]
> Uživatelé databáze (s výjimkou hello správci) nelze vytvořit pomocí portálu. Role RBAC nejsou šířený tooSQL Server, SQL Database nebo SQL Data Warehouse. Azure role RBAC slouží ke správě prostředků Azure a toodatabase oprávnění se nevztahují. Například hello **SQL serveru Přispěvatel** role neuděluje přístup tooconnect toohello SQL Database nebo SQL Data Warehouse. oprávnění k Hello musí mít udělen přímo v databázi hello pomocí příkazů Transact-SQL.
>

Azure AD na základě obsažené databáze uživatelé (jiné než hello správce serveru, která vlastní databáze hello), připojení databáze toohello s identitou služby Azure AD jako uživatel s toocreate alespoň hello **ALTER ANY uživatele** oprávnění. Potom pomocí následující syntaxe Transact-SQL hello:

```
CREATE USER <Azure_AD_principal_name> FROM EXTERNAL PROVIDER;
```

*Azure_AD_principal_name* může být hello hlavní název uživatele Azure AD uživatele nebo hello zobrazovaný název pro skupinu služby Azure AD.

**Příklady:** toocreate uživatele databáze s omezením představující Azure AD federovaný nebo spravovaný uživatele domény:
```
CREATE USER [bob@contoso.com] FROM EXTERNAL PROVIDER;
CREATE USER [alice@fabrikam.onmicrosoft.com] FROM EXTERNAL PROVIDER;
```

toocreate uživatele databáze s omezením představující Azure AD nebo federovaných skupiny domény, zadejte hello zobrazovaný název skupiny zabezpečení:
```
CREATE USER [ICU Nurses] FROM EXTERNAL PROVIDER;
```

toocreate uživatele databáze s omezením představující aplikaci, která se připojuje pomocí tokenu Azure AD:

```
CREATE USER [appName] FROM EXTERNAL PROVIDER;
```

>  [!TIP]
>  Uživatele nelze vytvořit přímo z Azure Active Directory než hello Azure Active Directory, která souvisí s předplatným Azure. Ale členy jiné Active Directory, které jsou importované uživatele v hello související služby Active Directory (označuje jako externí uživatelé) lze přidat skupiny služby Active Directory tooan v hello klienta služby Active Directory. Vytvořením uživatele databáze s omezením pro tuto skupinu AD hello z hello externí služby Active Directory mohou uživatelé získat přístup k tooSQL databáze.   

Další informace o vytváření obsahovala databázi uživatelů podle identit Azure Active Directory, najdete v části [vytvořit uživatele (Transact-SQL)](http://msdn.microsoft.com/library/ms173463.aspx).

> [!NOTE]
> Odebírání hello správce Azure Active Directory pro server Azure SQL zabrání žádné uživatele Azure AD authentication připojení toohello serveru. Pokud potřebné, nepoužitelná uživatele Azure AD může být přetažen ručně správcem databáze SQL.   

>  [!NOTE]
>  Pokud se zobrazí **vypršel časový limit připojení**, může být nutné tooset hello `TransparentNetworkIPResolution` parametr toofalse řetězec připojení hello. Další informace najdete v tématu [problém časový limit připojení v rozhraní .NET Framework 4.6.1 - TransparentNetworkIPResolution](https://blogs.msdn.microsoft.com/dataaccesstechnologies/2016/05/07/connection-timeout-issue-with-net-framework-4-6-1-transparentnetworkipresolution/).   

   
Když vytvoříte uživatel databáze, tento uživatel obdrží hello **připojit** oprávnění a můžou se připojit databáze toothat jako člen skupiny hello **veřejné** role. Původně hello pouze oprávnění uživatele k dispozici toohello jsou všechna oprávnění udělená toohello **veřejné** role nebo všechna oprávnění udělená tooany skupiny systému Windows, že jsou členem. Po zřízení služby Azure AD na základě obsažené uživatel databáze, můžete udělit další oprávnění uživatele hello hello stejný způsobem, jako byste udělit oprávnění tooany jiný typ uživatele. Obvykle udělit oprávnění role toodatabase a přidejte tooroles uživatele. Další informace najdete v tématu [základy oprávnění databáze modul](http://social.technet.microsoft.com/wiki/contents/articles/4433.database-engine-permission-basics.aspx). Další informace o rolích speciální databáze SQL najdete v tématu [Správa databází a přihlašovacích údajů ve službě Azure SQL Database](sql-database-manage-logins.md).
Federovanou doménu uživatele, která byla importována do domény spravovat, musíte použít identitu hello spravované domény.

> [!NOTE]
> Azure AD Uživatelé jsou označené v metadata databáze hello s typem E (EXTERNAL_USER) a pro skupiny typu X (EXTERNAL_GROUPS). Další informace najdete v tématu [sys.database_principals](https://msdn.microsoft.com/library/ms187328.aspx). 
>

## <a name="connect-toohello-user-database-or-data-warehouse-by-using-ssms-or-ssdt"></a>Připojit toohello uživatel databáze nebo datového skladu pomocí aplikace SSMS nebo rozšíření SSDT  
Správce hello Azure AD tooconfirm správně nastavit, aby připojení toohello **hlavní** databázi pomocí účtu správce hello Azure AD.
tooprovision Azure AD na základě obsažené databáze uživatelé (jiné než hello správce serveru, která vlastní databáze hello), připojení databáze toohello s identitou služby Azure AD, která má přístup toohello databáze.

> [!IMPORTANT]
> Podpora pro ověřování Azure Active Directory je k dispozici s [SQL Server 2016 Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) a [SQL Server Data Tools](https://msdn.microsoft.com/library/mt204009.aspx) ve Visual Studiu 2015. Hello srpna 2016 verzi aplikace SSMS zahrnuje taky podporu pro univerzální ověřování služby Active Directory, která umožňuje správci toorequire Multi-Factor Authentication pomocí telefonního hovoru, textové zprávy, čipové karty s PIN kód nebo oznámení mobilní aplikace.
 
## <a name="using-an-azure-ad-identity-tooconnect-using-ssms-or-ssdt"></a>Pomocí služby Azure AD identity tooconnect pomocí aplikace SSMS nebo rozšíření SSDT  

Hello následující postupy ukazují, jak tooconnect tooa SQL databáze s Azure AD identity pomocí SQL Server Management Studio nebo nástroje databáze systému SQL Server.

### <a name="active-directory-integrated-authentication"></a>Integrované ověřování Active Directory

Tuto metodu použijte, pokud jste přihlášeni tooWindows pomocí svých přihlašovacích údajů Azure Active Directory z federované domény.

1. Spustit Management Studio nebo nástrojů Data a v hello **připojit tooServer** (nebo **připojit tooDatabase modul**) dialogové okno, v hello **ověřování** vyberte **Služby active Directory - integrované**. Žádné heslo je potřeba, nebo můžete zadat, protože zobrazí vaše stávající přihlašovací údaje pro připojení hello.   

    ![Vyberte integrované ověřování AD][11]
2. Klikněte na tlačítko hello **možnosti** tlačítko a na hello **vlastnosti připojení** stránku hello **připojit toodatabase** pole, název typu hello hello uživatelskou databázi, kterou chcete tooconnect . (hello **ID názvu nebo klienta domény AD**"možnost je podporována pouze pro **Universal s MFA připojení** možnosti, jinak se šedé.)  

    ![Vyberte název databáze hello][13]

## <a name="active-directory-password-authentication"></a>Ověřování hesla Active Directory

Tuto metodu použijte, pokud připojíte přes hlavní název služby Azure AD pomocí služby Azure AD hello spravované domény. Můžete ji použít i pro federované účet bez přístupu toohello doménu, například při práci se vzdáleně.

Tuto metodu použijte, pokud jste se přihlásili pomocí přihlašovacích údajů z domény, která není sdružených se službou Azure tooWindows nebo při použití Azure AD pomocí ověřování Azure AD podle hello počáteční nebo hello domény klienta.

1. Spustit Management Studio nebo nástrojů Data a v hello **připojit tooServer** (nebo **připojit tooDatabase modul**) dialogové okno, v hello **ověřování** vyberte **Active Directory – heslo**.
2. V hello **uživatelské jméno** zadejte jméno uživatele Azure Active Directory ve formátu hello  **username@domain.com** . Toto musí být účet z hello Azure Active Directory nebo účet z domény vytvořit federaci s hello Azure Active Directory.
3. V hello **heslo** pole, zadejte heslo uživatele pro účet služby Azure Active Directory hello nebo federovaných účet domény.

    ![Vyberte možnost ověřování hesla služby AD][12]
4. Klikněte na tlačítko hello **možnosti** tlačítko a na hello **vlastnosti připojení** stránku hello **připojit toodatabase** pole, název typu hello hello uživatelskou databázi, kterou chcete tooconnect . (Viz obrázek hello v hello předchozí možnost.)

## <a name="using-an-azure-ad-identity-tooconnect-from-a-client-application"></a>Pomocí služby Azure AD identity tooconnect z klientské aplikace

Hello následující postupy ukazují, jak tooconnect tooa SQL databáze s Azure AD identity z klientské aplikace.

###  <a name="active-directory-integrated-authentication"></a>Integrované ověřování Active Directory

toouse integrované ověřování systému Windows, vaše doména služby Active Directory musí být sdružených se službou Azure Active Directory. Klientské aplikace (nebo služby) připojení databáze toohello musí používat na počítači připojeném k doméně, v části přihlašovací údaje uživatele domény.

tooconnect tooa databázi pomocí integrované ověřování a identita Azure AD, musí být nastavena hello ověřování – klíčové slovo v připojovací řetězec databáze hello tooActive Directory integrované. Hello následující ukázka kódu C# používá rozhraní ADO .NET.

```
string ConnectionString =
@"Data Source=n9lxnyuzhv.database.windows.net; Authentication=Active Directory Integrated; Initial Catalog=testdb;";
SqlConnection conn = new SqlConnection(ConnectionString);
conn.Open();
```

Hello klíčové slovo připojovacího řetězce ``Integrated Security=True`` není podporována pro připojení tooAzure databáze SQL. Při vytváření připojení ODBC, budete potřebovat tooremove prostory a nastavit ověřování too'ActiveDirectoryIntegrated'.

### <a name="active-directory-password-authentication"></a>Ověřování hesla Active Directory

tooconnect tooa databázi pomocí integrované ověřování a identita Azure AD, hello ověřování – klíčové slovo musí být nastavena tooActive heslo adresáře. Hello připojovací řetězec musí obsahovat identifikátor ID uživatele nebo uživatele a heslo/PWD klíčová slova a hodnoty. Hello následující ukázka kódu C# používá rozhraní ADO .NET.

```
string ConnectionString =
@"Data Source=n9lxnyuzhv.database.windows.net; Authentication=Active Directory Password; Initial Catalog=testdb;  UID=bob@contoso.onmicrosoft.com; PWD=MyPassWord!";
SqlConnection conn = new SqlConnection(ConnectionString);
conn.Open();
```

Další informace o metodách ověřování Azure AD pomocí ukázky kódu ukázku hello k dispozici na [Azure AD Authentication Githubu ukázku](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/security/azure-active-directory-auth).

## <a name="azure-ad-token"></a>Azure AD token
Tato metoda ověřování umožňuje tooAzure tooconnect střední vrstvy služby SQL Database nebo Azure SQL Data Warehouse pomocí získání tokenu z Azure Active Directory (AAD). Umožňuje sofistikované scénáře, včetně ověřování pomocí certifikátů. Je třeba provést čtyři základní kroky toouse Azure AD tokenu ověřování:

1. Registrace aplikace pomocí Azure Active Directory a získejte id klienta hello kódu. 
2. Vytvořte aplikaci představující hello databáze uživatele. (Dokončit výše v kroku 6.)
3. Vytvoření certifikátu na hello klientský počítač spustí aplikace hello.
4. Přidáte certifikát hello jako klíč pro vaši aplikaci.

Ukázka připojovací řetězec:

```
string ConnectionString =@"Data Source=n9lxnyuzhv.database.windows.net; Initial Catalog=testdb;"
SqlConnection conn = new SqlConnection(ConnectionString);
connection.AccessToken = "Your JWT token"
conn.Open();
```

Další informace najdete v tématu [SQL Server Security Blog](https://blogs.msdn.microsoft.com/sqlsecurity/2016/02/09/token-based-authentication-support-for-azure-sql-db-using-azure-ad-auth/).

### <a name="sqlcmd"></a>sqlcmd

Dobrý den následující příkazy, připojte se pomocí 13.1 verzi sqlcmd, která je k dispozici z hello [Download Center](http://go.microsoft.com/fwlink/?LinkID=825643).

```
sqlcmd -S Target_DB_or_DW.testsrv.database.windows.net  -G  
sqlcmd -S Target_DB_or_DW.testsrv.database.windows.net -U bob@contoso.com -P MyAADPassword -G -l 30
```

## <a name="next-steps"></a>Další kroky
- Přehled řízení a přístupu pro SQL Database najdete v tématu věnovaném [řízení a přístupu k SQL Database](sql-database-control-access.md).
- Přehled přihlášení, uživatelů a databázových rolí ve službě SQL Database najdete v tématu věnovaném [přihlášením, uživatelům a databázovým rolím](sql-database-manage-logins.md).
- Další informace o objektech zabezpečení databáze najdete v tématu [Objekty zabezpečení](https://msdn.microsoft.com/library/ms181127.aspx).
- Další informace o databázových rolích najdete v tématu věnovaném [databázovým rolím](https://msdn.microsoft.com/library/ms189121.aspx).
- Další informace o pravidlech brány firewall pro SQL Database najdete v tématu [Pravidla brány firewall služby SQL Database](sql-database-firewall-configure.md).

<!--Image references-->

[1]: ./media/sql-database-aad-authentication/1aad-auth-diagram.png
[2]: ./media/sql-database-aad-authentication/2subscription-relationship.png
[3]: ./media/sql-database-aad-authentication/3admin-structure.png
[4]: ./media/sql-database-aad-authentication/4select-subscription.png
[5]: ./media/sql-database-aad-authentication/5ad-settings-portal.png
[6]: ./media/sql-database-aad-authentication/6edit-directory-select.png
[7]: ./media/sql-database-aad-authentication/7edit-directory-confirm.png
[8]: ./media/sql-database-aad-authentication/8choose-ad.png
[10]: ./media/sql-database-aad-authentication/10choose-admin.png
[11]: ./media/sql-database-aad-authentication/active-directory-integrated.png
[12]: ./media/sql-database-aad-authentication/12connect-using-pw-auth2.png
[13]: ./media/sql-database-aad-authentication/13connect-to-db2.png

