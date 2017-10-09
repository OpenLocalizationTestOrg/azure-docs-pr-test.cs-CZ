---
title: aaaGeneric krok konektor SQL za krok | Microsoft Docs
description: "Tento článek je proti můžete prostřednictvím jednoduchý systém HR podrobné pomocí hello obecné konektor SQL."
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: 28c1cc60-24fd-4d0d-a36d-b4aba6de86e7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: b1b5f89ab588de6f92f173a7bc00f97180067669
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="generic-sql-connector-step-by-step"></a>Podrobný postup pro obecný konektor SQL
Toto téma je podrobný průvodce. Vytvoří databáze HR jednoduchého ukázkového a použít jej pro import někteří uživatelé a jejich členství ve skupině.

## <a name="prepare-hello-sample-database"></a>Příprava hello ukázkové databáze
Na serveru se systémem SQL Server, spusťte skript SQL hello najít v [příloha A](#appendix-a). Tento skript vytvoří ukázkovou databázi s názvem hello GSQLDEMO. Hello objektový model pro hello vytvořit databázi vypadá podobně jako tento obrázek:  
![Objektový Model](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/objectmodel.png)

Také vytvořte uživatele chcete toouse tooconnect toohello databáze. V tomto návodu je uživatel hello názvem FABRIKAM\SQLUser a umístěné v doméně hello.

## <a name="create-hello-odbc-connection-file"></a>Vytvoření souboru připojení ODBC hello
Hello obecné konektor SQL je pomocí rozhraní ODBC tooconnect toohello vzdáleného serveru. Nejdřív potřebujeme toocreate soubor s hello informace o připojení ODBC.

1. Spusťte nástroj pro správu rozhraní ODBC hello na serveru:  
   ![ODBC](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc.png)
2. Karta vyberte hello **souborové DSN**. Klikněte na tlačítko **přidat...** .  
   ![ODBC1](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc1.png)
3. Hello out-of-box ovladačů funguje přesně, takže vyberte ho a klikněte na tlačítko **Další >**.  
   ![ODBC2](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc2.png)
4. Pojmenujte soubor hello, jako například **GenericSQL**.  
   ![ODBC3](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc3.png)
5. Klikněte na **Dokončit**.  
   ![ODBC4](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc4.png)
6. Čas tooconfigure hello připojení. Zadejte zdroj dat hello funkční popis a zadejte název hello hello serveru se systémem SQL Server.  
   ![ODBC5](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc5.png)
7. Vyberte, jak tooauthenticate pomocí SQL. V tomto případě používáme ověřování systému Windows.  
   ![ODBC6](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc6.png)
8. Zadejte jméno hello hello ukázkové databáze, **GSQLDEMO**.  
   ![ODBC7](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc7.png)
9. Udržování všechno výchozí na této obrazovce. Klikněte na **Dokončit**.  
   ![ODBC8](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc8.png)
10. Klikněte na možnost vše funguje podle očekávání, tooverify **Test zdroje dat**.  
    ![ODBC9](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc9.png)
11. Zajistěte, aby hello test je úspěšné.  
    ![ODBC10](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc10.png)
12. konfigurační soubor rozhraní ODBC Hello by teď měly být viditelné v souboru DSN.  
    ![ODBC11](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc11.png)

Nyní je k dispozici soubor hello budeme potřebovat a můžete začít vytvářet hello konektor.

## <a name="create-hello-generic-sql-connector"></a>Vytvoření hello obecné konektor SQL
1. V hello uživatelského rozhraní Správce služby synchronizace, vyberte **konektory** a **vytvořit**. Vyberte **obecné SQL (Microsoft)** a pojmenujte ho popisný název.  
   ![Connector1](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector1.png)
2. Najít soubor hello názvu DSN, kterou jste vytvořili v předchozí části hello a nahrajte ho toohello serveru. Zadejte hello pověření tooconnect toohello databáze.  
   ![Connector2](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector2.png)
3. V tomto návodu budeme jsou a usnadnit tak nám a Řekněme, že existují dva typy objektů, **uživatele** a **skupiny**.
   ![Connector3](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector3.png)
4. atributy hello toofind, chceme hello konektor toodetect těmito atributy prohlížením samotné tabulky hello. Vzhledem k tomu **uživatelé** je vyhrazené slovo v SQL, potřebujeme tooprovide v hranaté závorky [].  
   ![Connector4](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector4.png)
5. Čas toodefine hello ukotvení atribut a atribut rozlišující název hello. Pro **uživatelé**, používáme hello kombinace uživatelského jména hello dva atributy a EmployeeID. Pro **skupiny**, používáme GroupName (ne realistické ve skutečném, ale pro tento postup funguje).
   ![Connector5](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector5.png)
6. Ne všechny typy atributů lze zjistit v databázi SQL. Typ atributu Hello odkaz na konkrétní nelze. Pro typ objektu skupiny hello potřebujeme toochange hello ID vlastníka a MemberID tooreference.  
   ![Connector6](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector6.png)
7. atributy Hello jsme vybrali jako atributy typu odkaz v předchozím kroku hello vyžadují hello typ objektu, který tyto hodnoty jsou odkaz na. V našem případě hello typ objektu uživatele.  
   ![Connector7](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector7.png)
8. Na stránce globální parametry hello vyberte **vodoznak** jako hello rozdílů strategie. Také zadat ve formátu data a času hello **rrrr MM-dd hh: mm:**.
   ![Connector8](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector8.png)
9. Na hello **konfigurace oddílů a hierarchií** vyberte oba typy objektů.
   ![Connector9](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector9.png)
10. Na hello **vyberte typy objektů** a **vybrat atributy**, vyberte typy objektů a všech atributů. Na hello **konfigurace kotvy** klikněte na tlačítko **Dokončit**.

## <a name="create-run-profiles"></a>Vytvoření profilů spuštění
1. V hello uživatelského rozhraní Správce služby synchronizace, vyberte **konektory**, a **konfigurovat profily spuštění**. Klikněte na tlačítko **nový profil**. Začneme s **úplný Import**.  
   ![Runprofile1](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile1.png)
2. Vyberte typ hello **úplný Import (jenom fázi)**.  
   ![Runprofile2](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile2.png)
3. Vyberte oddíl hello **OBJEKT = uživatele**.  
   ![Runprofile3](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile3.png)
4. Vyberte **tabulky** a typ **[uživatelé]**. Posuňte se dolů toohello více hodnot objektu typu oddílu a zadejte hello data jako hello následující obrázek. Vyberte **Dokončit** toosave hello krok.  
   ![Runprofile4a](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile4a.png)  
   ![Runprofile4b](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile4b.png)  
5. Vyberte **nový krok**. Tentokrát vyberte **OBJEKT = skupiny**. Na poslední stránku hello použijte hello konfiguraci jako hello následující obrázek. Klikněte na **Dokončit**.  
   ![Runprofile5a](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile5a.png)  
   ![Runprofile5b](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile5b.png)  
6. Volitelné: Pokud chcete, můžete nakonfigurovat další profilů spuštění. V tomto návodu se používá pouze hello úplný Import.
7. Klikněte na tlačítko **OK** toofinish změna profilů spuštění.

## <a name="add-some-test-data-and-test-hello-import"></a>Přidat některé testovací data a testovací import hello
Vyplňte nějaká testovací data v ukázkové databázi. Až budete připravení, vyberte **spustit** a **úplný import**.

Zde je uživatel s dvě telefonní čísla a skupiny se některé členy.  
![cs1](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/cs1.png)  
![CS2](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/cs2.png)  

## <a name="appendix-a"></a>Příloha A
**SQL skriptu toocreate hello ukázkové databáze**

```SQL
---Creating hello Database---------
Create Database GSQLDEMO
Go
-------Using hello Database-----------
Use [GSQLDEMO]
Go
-------------------------------------
USE [GSQLDEMO]
GO
/****** Object:  Table [dbo].[GroupMembers]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[GroupMembers](
    [MemberID] [int] NOT NULL,
    [Group_ID] [int] NOT NULL
) ON [PRIMARY]

GO
/****** Object:  Table [dbo].[GROUPS]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[GROUPS](
    [GroupID] [int] NOT NULL,
    [GROUPNAME] [nvarchar](200) NOT NULL,
    [DESCRIPTION] [nvarchar](200) NULL,
    [WATERMARK] [datetime] NULL,
    [OwnerID] [int] NULL,
PRIMARY KEY CLUSTERED
(
    [GroupID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
) ON [PRIMARY]

GO
/****** Object:  Table [dbo].[USERPHONE]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
SET ANSI_PADDING ON
GO
CREATE TABLE [dbo].[USERPHONE](
    [USER_ID] [int] NULL,
    [Phone] [varchar](20) NULL
) ON [PRIMARY]

GO
SET ANSI_PADDING OFF
GO
/****** Object:  Table [dbo].[USERS]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[USERS](
    [USERID] [int] NOT NULL,
    [USERNAME] [nvarchar](200) NOT NULL,
    [FirstName] [nvarchar](100) NULL,
    [LastName] [nvarchar](100) NULL,
    [DisplayName] [nvarchar](100) NULL,
    [ACCOUNTDISABLED] [bit] NULL,
    [EMPLOYEEID] [int] NOT NULL,
    [WATERMARK] [datetime] NULL,
PRIMARY KEY CLUSTERED
(
    [USERID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
) ON [PRIMARY]

GO
ALTER TABLE [dbo].[GroupMembers]  WITH CHECK ADD  CONSTRAINT [FK_GroupMembers_GROUPS] FOREIGN KEY([Group_ID])
REFERENCES [dbo].[GROUPS] ([GroupID])
GO
ALTER TABLE [dbo].[GroupMembers] CHECK CONSTRAINT [FK_GroupMembers_GROUPS]
GO
ALTER TABLE [dbo].[GroupMembers]  WITH CHECK ADD  CONSTRAINT [FK_GroupMembers_USERS] FOREIGN KEY([MemberID])
REFERENCES [dbo].[USERS] ([USERID])
GO
ALTER TABLE [dbo].[GroupMembers] CHECK CONSTRAINT [FK_GroupMembers_USERS]
GO
ALTER TABLE [dbo].[GROUPS]  WITH CHECK ADD  CONSTRAINT [FK_GROUPS_USERS] FOREIGN KEY([OwnerID])
REFERENCES [dbo].[USERS] ([USERID])
GO
ALTER TABLE [dbo].[GROUPS] CHECK CONSTRAINT [FK_GROUPS_USERS]
GO
ALTER TABLE [dbo].[USERPHONE]  WITH CHECK ADD  CONSTRAINT [FK_USERPHONE_USER] FOREIGN KEY([USER_ID])
REFERENCES [dbo].[USERS] ([USERID])
GO
ALTER TABLE [dbo].[USERPHONE] CHECK CONSTRAINT [FK_USERPHONE_USER]
GO
```
