---
title: "aaaAdding Mobile Services pomocí připojené služby v sadě Visual Studio | Microsoft Docs"
description: "Přidání Mobile Services pomocí dialogové okno hello Visual Studio přidat připojení služby"
services: visual-studio-online
documentationcenter: na
author: mlhoop
manager: douge
editor: 
ms.assetid: 75c3cb93-88e1-476d-a416-f34caa3608e3
ms.service: visual-studio-online
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: mobile
ms.date: 12/16/2015
ms.author: mlearned
ms.openlocfilehash: c47b6fb63dc99fbc012e1c627c6c7e95249de7a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="adding-mobile-services-by-using-visual-studio-connected-services"></a>Přidání Mobile Services pomocí Visual Studio připojené Services
Pomocí sady Visual Studio 2015, se můžete připojit pomocí hello tooAzure Mobile Services **přidat připojení službě** dialogové okno. Můžete připojit z každou klientskou aplikaci C#, žádné aplikace JavaScript nebo aplikace Cordova napříč platformami. Jakmile se připojíte, můžete vytvořit a přístup k datům, vytvořte vlastní rozhraní API a naplánované úlohy nebo přidat podporu pro nabízená oznámení.  Hello připojených služeb operaci přidá všechny příslušné odkazy a kód připojení. Můžete také využít výhod integrovanou podporu pro ověřování s různými schémata oblíbených identity, jako je Azure AD, Facebook, Twitter a Accounts Microsoft.

## <a name="supported-project-types"></a>Typy podporované projektů
> [!NOTE]
> Přidávání projekty Universal Windows (Windows 10) tooa Azure Mobile Services pomocí dialogu přidat připojení služby hello není podporováno v sadě Visual Studio 2015. Azure Mobile Services můžete přidat nainstalováním hello vhodné balíčky pomocí hello Správce balíčků NuGet pro projekt.
> 
> 

Můžete použít hello připojené služby dialogové okno tooconnect tooAzure Mobile Services v hello následující typy projektů.

* Rozhraní .NET Windows 8.1 Store, Phone a univerzální aplikace pro projekty
* JavaScript Windows 8.1 Store, Phone a univerzální aplikace pro projekty
* Projekty vytvořené pomocí nástroje sady Visual Studio pro Apache Cordova

## <a name="connect-tooazure-mobile-services-using-hello-add-connected-services-dialog"></a>Připojit pomocí dialogu přidat připojení služby hello tooAzure Mobile Services
1. Ujistěte se, že máte účet Azure. Pokud nemáte účet Azure, můžete si zaregistrovat [bezplatnou zkušební verzi](http://go.microsoft.com/fwlink/?LinkId=518146).
2. Otevřete hello **přidat připojení služby** dialogové okno.
   
   * Pro aplikace .NET, otevřete projekt v sadě Visual Studio, otevřete hello kontextovou nabídku hello **odkazy** uzlu v Průzkumníku řešení a potom zvolte **přidat připojení služby**
     
        ![Připojení tooAzure mobilní služby](./media/vs-azure-tools-connected-services-add-mobile-services/IC797635.png)
   * Pro projekty aplikace Apache Cordova, otevřete projekt v sadě Visual Studio, otevřete hello kontextovou nabídku hello uzel projektu v Průzkumníku řešení a potom zvolte **přidat připojení službě**.
3. V hello **přidat připojení službě** dialogovém okně vyberte **Azure Mobile Services**a potom zvolte hello **konfigurace** tlačítko. Pokud jste tak již neučinili, může být výzvami toolog do Azure.
   
    ![Přidání mobilní služby Azure](./media/vs-azure-tools-connected-services-add-mobile-services/IC797636.png)
4. V hello **Azure Mobile Services** dialogovém okně vyberte stávající mobilní službu, pokud nemáte. Pokud potřebujete toocreate nové mobilní služby Azure, postupujte podle postupu hello níže toodo tak. Jinak přejděte toohello další krok.
   
    toocreate nový účet mobilní služby:
   
   1. Zvolte hello ** vytvořit službu ** odkaz dole hello v dialogovém okně hello.
       ![Přidejte novou mobilní službu připojené](./media/vs-azure-tools-connected-services-add-mobile-services/IC797637.png)
   2. Na hello **vytvoření mobilní služby** dialogové okno, můžete vybrat mobilní služby back-end JavaScript, nebo rozhraní .NET back-end mobilní služby z hello **Runtime** rozevíracího seznamu. 
      
       ![Vytvoření mobilní služby](./media/vs-azure-tools-connected-services-add-mobile-services/IC797638.png)
      
       Služba back-end JavaScript je jednoduchá ale účinná. Pokud vytvoříte mobilní službu back-end JavaScript, kód JavaScript na straně serveru hello je uložen v hello cloudu, ale skripty serveru můžete upravit pomocí Průzkumníka serveru nebo portál pro správu Azure hello. 
      
       Rozhraní .NET back-end mobilní služby vám dává hello úplné výkon a flexibilitu webového rozhraní API a rozhraní Entity Framework. Pokud vytvoříte .NET back-end mobilní službu, projektu se vytvoří pro vás a přidat tooyour řešení. 
   3. Zvolte hello **oblast** kde chcete hello mobilní služby a pak zadejte uživatelské jméno a heslo pro hello server.
   4. Po zadání všech hello požadované informace, vyberte hello **vytvořit** tlačítko toocreate hello mobilní služby.
   5. Hello nové mobilní služby by se měla objevit v seznamu služby hello na hello **Azure Mobile Services** dialogové okno. Vyberte v seznamu hello hello nové mobilní služby a potom zvolte hello **přidat** tlačítko tooadd hello služby tooyour projektu.
5. Zkontrolujte hello Začínáme stránky, která se zobrazí a zjistěte, jak byla změněna projektu. Na stránce Začínáme se zobrazí v prohlížeči vždy při přidání připojené služby. Můžete zkontrolovat hello navrhované další kroky a příklady kódu nebo přepínač toohello co se stalo toosee stránku, odkazy, které byly přidány tooyour projektu a jak se soubory kódu a konfigurace upravených.
6. Pomocí ukázky kódu hello jako vodítko spusťte psaní kódu tooaccess mobilní službě!

## <a name="how-your-project-is-modified"></a>Jak se mění projektu
Jak upraví projektu v sadě Visual Studio, závisí na typu projektu hello. C# klientských aplikací, najdete v části [co se stalo – projekty c](http://go.microsoft.com/fwlink/p/?LinkId=513119). JavaScript klientských aplikací, najdete v části [co se stalo – projekty JavaScript](http://go.microsoft.com/fwlink/p/?LinkId=513120). Pro aplikace Cordova, najdete v části [co se stalo – projekty Cordova](http://go.microsoft.com/fwlink/p/?LinkId=513116).

## <a name="next-steps"></a>Další kroky
Klást otázky a získat pomoc: 

* [Fórum MSDN: Azure Mobile Services](https://social.msdn.microsoft.com/forums/azure/home?forum=azuremobile)
* [Azure Mobile Services v hello Blog týmu Microsoft Azure](https://azure.microsoft.com/blog/topics/mobile/)
* [Azure Mobile Services na webu azure.microsoft.com](https://azure.microsoft.com/services/mobile-services/)
* [Azure Mobile Services dokumentaci na webu azure.microsoft.com](https://azure.microsoft.com/documentation/services/mobile-services/)

