---
title: "Seznam účtů úložiště Azure"
description: "Spravovat nastavení účtu úložiště pomocí sady nástrojů pro Azure pro Eclipse"
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: bbacfcd8-dbf5-4265-a930-59f508de5325
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: f859efa389d3fe0b4b7b16255d57f1aa13123319
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-storage-account-list"></a>Seznam účtů úložiště Azure
Účty úložiště Azure povolit umístění stahování, který se má použít pro vaše JDK, aplikační server a libovolné součásti a také pro ukládání stavu, při použití ukládání do mezipaměti. Eclipse udržuje seznam známých úložiště účty, které jsou k dispozici do vašich projektů v pracovním prostoru Eclipse. Otevřete **účty úložiště** dialog, který se používá ke správě tohoto seznamu, v rámci prostředí Eclipse, klikněte na tlačítko **okno**, klikněte na tlačítko **Předvolby**, rozbalte položku **Azure**a potom klikněte na **účty úložiště**.

Následující ukazuje **účty úložiště** dialogové okno.

![][ic719496]

Tento dialog můžete také otevřít z **účty** odkaz v dialogových oknech, které používají účty úložiště, jako jsou následující:

* **JDK** kartě **konfigurace serveru** dialogové okno.
* **Server** kartě **konfigurace serveru** dialogové okno.
* **Přidat součást** dialogové okno.
* **Ukládání do mezipaměti** dialogovém okně Vlastnosti.

## <a name="to-import-your-storage-accounts-using-a-publish-settings-file"></a>Chcete-li importovat soubor nastavení publikování pomocí účtů úložiště
1. V rámci **účty úložiště** dialogové okno, klikněte na tlačítko **Import ze souboru nastavení publikování**.

2. (Tento krok přeskočte, pokud jste už uložili soubor nastavení publikování do místního počítače.) V **importovat informace o předplatném** dialogové okno, klikněte na tlačítko **stáhnout soubor nastavení publikování**. Pokud ještě nejste přihlášeni k účtu Azure, budete vyzváni k přihlášení. Potom budete vyzváni k uložení Azure soubor nastavení publikování. (Můžete ignorovat výsledné pokynů na stránkách přihlášení - jejich jsou poskytovány na portálu Azure a jsou určeny pro Visual Studio uživatele.) Uložte ho do místního počítače.

3. Pořád ještě v **importovat informace o předplatném** dialogové okno, klikněte na tlačítko **Procházet** tlačítko, vyberte soubor nastavení publikování, který jste si uložili místně dříve a pak klikněte na tlačítko **otevřete**.

4. Klikněte na tlačítko **OK** zavřete **importovat informace o předplatném** dialogové okno.

## <a name="to-create-a-new-storage-account"></a>Chcete-li vytvořit nový účet úložiště
1. V rámci **účty úložiště** dialogové okno, klikněte na tlačítko **přidat**.

2. V rámci **přidat účet úložiště** dialogové okno, klikněte na tlačítko **nový**.

3. V rámci **nový účet úložiště** dialogové okno, zadejte hodnoty pro následující:

   * Název účtu úložiště.

   * Umístění účtu úložiště.

   * Popis účtu úložiště.

   * Odběr, do které patří k účtu úložiště.

4. Klikněte na tlačítko **OK** zavřete **nový účet úložiště** dialogové okno.

Ho může trvat několik minut, než váš účet úložiště, který se má vytvořit. Po vytvoření, klikněte na tlačítko **OK** zavřete **přidat účet úložiště** dialogové okno a váš nový účet úložiště se zařadí do seznamu účtů úložiště k dispozici.

## <a name="to-add-an-existing-storage-account-to-the-list"></a>Chcete-li přidat do seznamu existující účet úložiště
1. Pokud již účet úložiště Azure nemáte, vytvořte ho pomocí následujících kroků uvedených v **k vytvoření nového účtu úložiště v tématu** výše. (Alternativně můžete vytvořit nový účet úložiště na [portálu pro správu Azure][Azure Management Portal].)

2. V rámci **účty úložiště** dialogové okno, klikněte na tlačítko **přidat**.

3. V rámci **přidat účet úložiště** dialogové okno, zadejte hodnoty pro **název** a **přístupový klíč**. Název a přístupový klíč účtu musí být pro existující účet úložiště Azure. Použití **úložiště** části [portálu pro správu Azure] [ Azure Management Portal] zobrazíte názvy účtů úložiště a klíče. Vaše **přidat účet úložiště** dialogové okno bude vypadat podobně jako následující.
   
   ![][ic719497]

4. Klikněte na tlačítko **OK** zavřete **přidat účet úložiště** dialogové okno.

## <a name="to-modify-a-storage-account-to-use-a-new-access-key"></a>Chcete-li upravit účet úložiště, který pomocí nového přístupového klíče
1. V rámci **účty úložiště** dialogové okno, klikněte na účet úložiště, které chcete upravit a pak klikněte na tlačítko **upravit**.

2. V rámci **upravit přístupový klíč účtu úložiště** dialogu Upravit **přístupový klíč** hodnotu.

3. Klikněte na tlačítko **OK** zavřete **upravit přístupový klíč účtu úložiště** dialogové okno.

## <a name="to-remove-a-storage-account-from-the-list-maintained-in-eclipse"></a>Chcete odstranit účet úložiště ze seznamu udržovat v prostředí Eclipse
1. V rámci **účty úložiště** dialogové okno, klikněte na účet úložiště, které chcete upravit a pak klikněte na tlačítko **odebrat**.

2. Klikněte na tlačítko **OK** po zobrazení výzvy k odebrání účtu úložiště.

> [!NOTE]
> Odebrání účtu úložiště prostřednictvím **účty úložiště** dialogové okno pouze odebere ze seznamu účtů úložiště lze zobrazit v Eclipse. Neodebere účet úložiště ze svého předplatného Azure. Kromě toho účet úložiště může znovu po zobrazí v seznamu Eclipse znovu načte podrobnosti svého předplatného.
> 
> 

## <a name="see-also"></a>Viz také
[Azure nástrojů pro Eclipse][Azure Toolkit for Eclipse]

[Instalace Azure Toolkit pro Eclipse][Installing the Azure Toolkit for Eclipse] 

[Vytvoření aplikace Hello World služby Azure v prostředí Eclipse][Creating a Hello World Application for Azure in Eclipse]

Další informace o používání Azure s Java najdete v tématu [Azure střediska pro vývojáře Java][Azure Java Developer Center].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[What's New in the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699552

<!-- IMG List -->

[ic719496]: ./media/azure-toolkit-for-eclipse-azure-storage-account-list/ic719496.png
[ic719497]: ./media/azure-toolkit-for-eclipse-azure-storage-account-list/ic719497.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn205108.aspx -->
