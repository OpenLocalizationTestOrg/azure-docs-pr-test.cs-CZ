---
title: "aaaManage databáze rolí a uživatelů ve službě Azure Analysis Services | Microsoft Docs"
description: "Zjistěte, jak toomanage databáze rolí a uživatelů na serveru služby Analysis Services v Azure."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/15/2017
ms.author: owend
ms.openlocfilehash: 2ad069a6bcce11bc43347625cb32ec400d48af18
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-database-roles-and-users"></a>Spravovat role databáze a uživatele

Na úrovni databáze hello modelu všichni uživatelé musí patřit roli tooa. Role definují uživatelé s konkrétní oprávnění pro hello modelové databáze. Přidat všechny uživatele nebo skupinu zabezpečení tooa role musí mít účet v klient služby Azure AD v hello stejného předplatného jako hello server.

Jak definovat role je jiné v závislosti na hello nástroj, který můžete použít, ale vliv hello je hello stejné.

Oprávnění role patří:
*  **Správce** -uživatelé mají úplná oprávnění pro databázi hello. Databázové role s oprávněními správce se liší od správce serveru.
*  **Proces** – uživatelé můžou připojit tooand provádět operace procesu na databázi hello a analyzovat data databáze modelu.
*  **Čtení** – uživatelé můžou použít klienta aplikace tooconnect tooand analyzovat data databáze modelu.

Při vytváření projektu tabulkového modelu, vytvořit role a přidání uživatelů nebo skupin toothose rolí pomocí Správce rolí v sadě SSDT. Server nasazené tooa použijete aplikace SSMS, [rutiny prostředí PowerShell Analysis Services](https://msdn.microsoft.com/library/hh758425.aspx), nebo [tabulkový Model skriptovací jazyk](https://msdn.microsoft.com/library/mt614797.aspx) tooadd (TMSL) nebo odebrání rolí a členy uživatelské.

## <a name="tooadd-or-manage-roles-and-users-in-ssdt"></a>tooadd nebo správě rolí a uživatelů v rozšíření SSDT  
  
1.  V sadě SSDT > **tabulkový Model Explorer**, klikněte pravým tlačítkem na **role**.  
  
2.  Ve **Správci rolí** klikněte na **Nový**.  
  
3.  Zadejte název pro roli hello.  
  
     Ve výchozím nastavení je hello název hello výchozí role číslované postupně pro každou novou roli. Doporučuje se, že zadáte název, který jednoznačně identifikuje hello člena typu, například finanční správci nebo odborníky lidských zdrojů.  
  
4.  Vyberte jednu z hello následující oprávnění:  
  
    |Oprávnění|Popis|  
    |----------------|-----------------|  
    |**None**|Členy nelze změnit schéma modelu hello a nemůže zadat dotaz na data.|  
    |**Čtení**|Členy dotazy na data (podle řádkové filtry) ale nelze změnit schéma modelu hello.|  
    |**Čtení a proces**|Členy může dotazovat data (závislosti na úrovni řádků filtry) a spusťte proces a proces všechny operace, ale nelze změnit schéma modelu hello.|  
    |**Proces**|Členy můžete spustit proces a proces všechny operace. Nelze změnit schéma modelu hello a nemůže zadat dotaz na data.|  
    |**Správce**|Členy můžete upravit schéma modelu hello a dotaz všechna data.|   
  
5.  Pokud jste roli hello vytváření má ke čtení nebo oprávnění ke čtení a proces, můžete přidat řádkové filtry pomocí vzorce DAX. Klikněte na tlačítko hello **řádkové filtry** kartě, pak vybrat tabulku, a pak klikněte na hello **DAX filtru** pole a potom zadat vzorec jazyka DAX.
  
6.  Klikněte na tlačítko **členy** > **přidat externí**.  
  
8.  V **přidat externího člena**, zadejte uživatele nebo skupiny v klientovi služby Azure AD pomocí e-mailovou adresu. Po klikněte na tlačítko OK a zavřete Role správce, role a členy role se zobrazí v Průzkumníku tabulkový Model. 
 
     ![Role a uživatele v tabulkovém Průzkumníka modelu](./media/analysis-services-database-users/aas-roles-tmexplorer.png)

9. Nasaďte tooyour serveru Azure Analysis Services.


## <a name="tooadd-or-manage-roles-and-users-in-ssms"></a>tooadd nebo správě rolí a uživatelů v aplikaci SSMS
role a uživatelé tooa tooadd nasazené modelová databáze, musí být připojené toohello serveru, jako správce serveru nebo je již v databázové role s oprávněními správce.

1. V objektu Exporer, klikněte pravým tlačítkem na **role** > **novou roli**.

2. V **vytvořit roli**, zadejte název role a popis.

3. Vyberte oprávnění.
   |Oprávnění|Popis|  
   |----------------|-----------------|  
   |**Úplné řízení (správce)**|Členové mohou změnit schéma modelu hello, zpracování a může prohledávat všechna data.| 
   |**Proces databáze**|Členy můžete spustit proces a proces všechny operace. Nelze změnit schéma modelu hello a nemůže zadat dotaz na data.|  
   |**Čtení**|Členy dotazy na data (podle řádkové filtry) ale nelze změnit schéma modelu hello.|  
  
4. Klikněte na tlačítko **členství**, pak zadejte uživatele nebo skupinu ve vašem klientovi Azure AD pomocí e-mailovou adresu.

     ![Přidání uživatele](./media/analysis-services-database-users/aas-roles-adduser-ssms.png)

5. Pokud hello role, kterou vytváříte má oprávnění pro čtení, můžete přidat řádkové filtry pomocí vzorce DAX. Klikněte na tlačítko **řádkové filtry**, vyberte tabulku a zadejte vzorec jazyka DAX hello **DAX filtru** pole. 

## <a name="tooadd-roles-and-users-by-using-a-tmsl-script"></a>tooadd role a uživatele pomocí skriptu TMSL
Když spustíte skript TMSL v okně XMLA hello v aplikaci SSMS nebo pomocí prostředí PowerShell. Použití hello [CreateOrReplace](https://docs.microsoft.com/sql/analysis-services/tabular-models-scripting-language-commands/createorreplace-command-tmsl) příkaz a hello [role](https://docs.microsoft.com/sql/analysis-services/tabular-models-scripting-language-objects/roles-object-tmsl) objektu.

**Ukázkový skript TMSL**

V této ukázce B2B externího uživatele a skupiny se přidají toohello analytik role s oprávnění ke čtení pro databázi SalesBI hello. Obě hello externího uživatele a skupiny musí být ve stejném klientovi Azure AD.

```
{
  "createOrReplace": {
    "object": {
      "database": "SalesBI",
      "role": "Analyst"
    },
    "role": {
      "name": "Users",
      "description": "All allowed users tooquery hello model",
      "modelPermission": "read",
      "members": [
        {
          "memberName": "user1@contoso.com",
          "identityProvider": "AzureAD"
        },
        {
          "memberName": "group1@adventureworks.com",
          "identityProvider": "AzureAD"
        }
      ]
    }
  }
}
```

## <a name="tooadd-roles-and-users-by-using-powershell"></a>tooadd role a uživatele pomocí prostředí PowerShell
Hello [SqlServer](https://msdn.microsoft.com/library/hh758425.aspx) modul poskytuje databáze specifické pro úlohy správy rutiny a hello pro obecné účely Invoke-ASCmd rutiny, která přijímá dotazu tabulkový Model skriptovací jazyk (TMSL) nebo skriptu. následující rutiny Hello se používají pro správu role databáze a uživatelů.
  
|Rutina|Popis|
|------------|-----------------| 
|[Přidat RoleMember](https://msdn.microsoft.com/library/hh510167.aspx)|Přidání role databáze tooa člen.| 
|[Odebrat RoleMember](https://msdn.microsoft.com/library/hh510173.aspx)|Odebrání člena z databázové role.|   
|[Vyvolání ASCmd](https://msdn.microsoft.com/library/hh479579.aspx)|Spuštění skriptu TMSL.|

## <a name="row-filters"></a>Řádkové filtry  
Řádkové filtry definovat, které řádky v tabulce může být dotazován členy určité role. Řádkové filtry jsou definovány pro každou tabulku v modelu s použitím vzorce DAX.  
  
Řádkové filtry lze definovat pouze pro role pro čtení a čtení a proces oprávnění. Ve výchozím nastavení Pokud pro konkrétní tabulku, není definován řádkový filtr Členové můžete dotazovat všechny řádky v tabulce hello Pokud křížového filtru platí z jiné tabulky.
  
 Řádkové filtry vyžadují vzorec jazyka DAX, který se musí vyhodnotit tooa hodnotu TRUE nebo FALSE, toodefine hello řádků, které může být dotazován členové této konkrétní roli. Nelze se dotazovat na řádky, které nejsou součástí hello vzorec jazyka DAX. Například hello tabulku zákazníků s hello následující výrazu řádku filtry, *= zákazníků [Země] = "USA"*, členové role prodej hello uvidí jenom zákazníkům hello USA.  
  
Řádkové filtry použít toohello zadané řádky a související řádky. Pokud tabulka obsahuje více vztahů, použít filtry zabezpečení pro hello vztah, který je aktivní. Řádkové filtry se prolínají s další řádek filtrech definované pro tabulky v relaci, třeba:  
  
|Table|Výraz jazyka DAX|  
|-----------|--------------------|  
|Oblast|= Oblast [Země] = "USA"|  
|ProductCategory|= ProductCategory [Name] = "Jízdních kol"|  
|Transakce|= Transakcí [rok] = 2016|  
  
 Hello net efekt je, že členové můžete dotazovat řádky dat, kde zákazník hello je hello USA, kategorie produktů hello je jízdních kol a rok hello je 2016. Uživatelé se nemohou dotazovat transakce mimo hello USA transakce, které nejsou jízdních kol nebo transakce není v 2016 dokud je členem jiné role, která uděluje tato oprávnění.
  
 Můžete použít možnosti filtrovat hello *=FALSE()*, toodeny přístup tooall řádků pro celou tabulku.

## <a name="next-steps"></a>Další kroky
  [Spravovat správce serveru](analysis-services-server-admins.md)   
  [Správa služby Azure Analysis Services pomocí prostředí PowerShell](analysis-services-powershell.md)  
  [Tabulkový Model skriptování referenční příručka jazyka (TMSL)](https://docs.microsoft.com/sql/analysis-services/tabular-model-scripting-language-tmsl-reference)

