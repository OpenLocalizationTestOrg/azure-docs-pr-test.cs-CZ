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
# <a name="manage-database-roles-and-users"></a><span data-ttu-id="70fd9-103">Spravovat role databáze a uživatele</span><span class="sxs-lookup"><span data-stu-id="70fd9-103">Manage database roles and users</span></span>

<span data-ttu-id="70fd9-104">Na úrovni databáze hello modelu všichni uživatelé musí patřit roli tooa.</span><span class="sxs-lookup"><span data-stu-id="70fd9-104">At hello model database level, all users must belong tooa role.</span></span> <span data-ttu-id="70fd9-105">Role definují uživatelé s konkrétní oprávnění pro hello modelové databáze.</span><span class="sxs-lookup"><span data-stu-id="70fd9-105">Roles define users with particular permissions for hello model database.</span></span> <span data-ttu-id="70fd9-106">Přidat všechny uživatele nebo skupinu zabezpečení tooa role musí mít účet v klient služby Azure AD v hello stejného předplatného jako hello server.</span><span class="sxs-lookup"><span data-stu-id="70fd9-106">Any user or security group added tooa role must have an account in an Azure AD tenant in hello same subscription as hello server.</span></span>

<span data-ttu-id="70fd9-107">Jak definovat role je jiné v závislosti na hello nástroj, který můžete použít, ale vliv hello je hello stejné.</span><span class="sxs-lookup"><span data-stu-id="70fd9-107">How you define roles is different depending on hello tool you use, but hello effect is hello same.</span></span>

<span data-ttu-id="70fd9-108">Oprávnění role patří:</span><span class="sxs-lookup"><span data-stu-id="70fd9-108">Role permissions include:</span></span>
*  <span data-ttu-id="70fd9-109">**Správce** -uživatelé mají úplná oprávnění pro databázi hello.</span><span class="sxs-lookup"><span data-stu-id="70fd9-109">**Administrator** - Users have full permissions for hello database.</span></span> <span data-ttu-id="70fd9-110">Databázové role s oprávněními správce se liší od správce serveru.</span><span class="sxs-lookup"><span data-stu-id="70fd9-110">Database roles with Administrator permissions are different from server administrators.</span></span>
*  <span data-ttu-id="70fd9-111">**Proces** – uživatelé můžou připojit tooand provádět operace procesu na databázi hello a analyzovat data databáze modelu.</span><span class="sxs-lookup"><span data-stu-id="70fd9-111">**Process** - Users can connect tooand perform process operations on hello database, and analyze model database data.</span></span>
*  <span data-ttu-id="70fd9-112">**Čtení** – uživatelé můžou použít klienta aplikace tooconnect tooand analyzovat data databáze modelu.</span><span class="sxs-lookup"><span data-stu-id="70fd9-112">**Read** -  Users can use a client application tooconnect tooand analyze model database data.</span></span>

<span data-ttu-id="70fd9-113">Při vytváření projektu tabulkového modelu, vytvořit role a přidání uživatelů nebo skupin toothose rolí pomocí Správce rolí v sadě SSDT.</span><span class="sxs-lookup"><span data-stu-id="70fd9-113">When creating a tabular model project, you create roles and add users or groups toothose roles by using Role Manager in SSDT.</span></span> <span data-ttu-id="70fd9-114">Server nasazené tooa použijete aplikace SSMS, [rutiny prostředí PowerShell Analysis Services](https://msdn.microsoft.com/library/hh758425.aspx), nebo [tabulkový Model skriptovací jazyk](https://msdn.microsoft.com/library/mt614797.aspx) tooadd (TMSL) nebo odebrání rolí a členy uživatelské.</span><span class="sxs-lookup"><span data-stu-id="70fd9-114">When deployed tooa server, you use SSMS, [Analysis Services PowerShell cmdlets](https://msdn.microsoft.com/library/hh758425.aspx), or [Tabular Model Scripting Language](https://msdn.microsoft.com/library/mt614797.aspx) (TMSL) tooadd or remove roles and user members.</span></span>

## <a name="tooadd-or-manage-roles-and-users-in-ssdt"></a><span data-ttu-id="70fd9-115">tooadd nebo správě rolí a uživatelů v rozšíření SSDT</span><span class="sxs-lookup"><span data-stu-id="70fd9-115">tooadd or manage roles and users in SSDT</span></span>  
  
1.  <span data-ttu-id="70fd9-116">V sadě SSDT > **tabulkový Model Explorer**, klikněte pravým tlačítkem na **role**.</span><span class="sxs-lookup"><span data-stu-id="70fd9-116">In SSDT > **Tabular Model Explorer**, right-click **Roles**.</span></span>  
  
2.  <span data-ttu-id="70fd9-117">Ve **Správci rolí** klikněte na **Nový**.</span><span class="sxs-lookup"><span data-stu-id="70fd9-117">In **Role Manager**, click **New**.</span></span>  
  
3.  <span data-ttu-id="70fd9-118">Zadejte název pro roli hello.</span><span class="sxs-lookup"><span data-stu-id="70fd9-118">Type a name for hello role.</span></span>  
  
     <span data-ttu-id="70fd9-119">Ve výchozím nastavení je hello název hello výchozí role číslované postupně pro každou novou roli.</span><span class="sxs-lookup"><span data-stu-id="70fd9-119">By default, hello name of hello default role is incrementally numbered for each new role.</span></span> <span data-ttu-id="70fd9-120">Doporučuje se, že zadáte název, který jednoznačně identifikuje hello člena typu, například finanční správci nebo odborníky lidských zdrojů.</span><span class="sxs-lookup"><span data-stu-id="70fd9-120">It's recommended you type a name that clearly identifies hello member type, for example, Finance Managers or Human Resources Specialists.</span></span>  
  
4.  <span data-ttu-id="70fd9-121">Vyberte jednu z hello následující oprávnění:</span><span class="sxs-lookup"><span data-stu-id="70fd9-121">Select one of hello following permissions:</span></span>  
  
    |<span data-ttu-id="70fd9-122">Oprávnění</span><span class="sxs-lookup"><span data-stu-id="70fd9-122">Permission</span></span>|<span data-ttu-id="70fd9-123">Popis</span><span class="sxs-lookup"><span data-stu-id="70fd9-123">Description</span></span>|  
    |----------------|-----------------|  
    |<span data-ttu-id="70fd9-124">**None**</span><span class="sxs-lookup"><span data-stu-id="70fd9-124">**None**</span></span>|<span data-ttu-id="70fd9-125">Členy nelze změnit schéma modelu hello a nemůže zadat dotaz na data.</span><span class="sxs-lookup"><span data-stu-id="70fd9-125">Members cannot modify hello model schema and cannot query data.</span></span>|  
    |<span data-ttu-id="70fd9-126">**Čtení**</span><span class="sxs-lookup"><span data-stu-id="70fd9-126">**Read**</span></span>|<span data-ttu-id="70fd9-127">Členy dotazy na data (podle řádkové filtry) ale nelze změnit schéma modelu hello.</span><span class="sxs-lookup"><span data-stu-id="70fd9-127">Members can query data (based on row filters) but cannot modify hello model schema.</span></span>|  
    |<span data-ttu-id="70fd9-128">**Čtení a proces**</span><span class="sxs-lookup"><span data-stu-id="70fd9-128">**Read and Process**</span></span>|<span data-ttu-id="70fd9-129">Členy může dotazovat data (závislosti na úrovni řádků filtry) a spusťte proces a proces všechny operace, ale nelze změnit schéma modelu hello.</span><span class="sxs-lookup"><span data-stu-id="70fd9-129">Members can query data (based on row-level filters) and run Process and Process All operations, but cannot modify hello model schema.</span></span>|  
    |<span data-ttu-id="70fd9-130">**Proces**</span><span class="sxs-lookup"><span data-stu-id="70fd9-130">**Process**</span></span>|<span data-ttu-id="70fd9-131">Členy můžete spustit proces a proces všechny operace.</span><span class="sxs-lookup"><span data-stu-id="70fd9-131">Members can run Process and Process All operations.</span></span> <span data-ttu-id="70fd9-132">Nelze změnit schéma modelu hello a nemůže zadat dotaz na data.</span><span class="sxs-lookup"><span data-stu-id="70fd9-132">Cannot modify hello model schema and cannot query data.</span></span>|  
    |<span data-ttu-id="70fd9-133">**Správce**</span><span class="sxs-lookup"><span data-stu-id="70fd9-133">**Administrator**</span></span>|<span data-ttu-id="70fd9-134">Členy můžete upravit schéma modelu hello a dotaz všechna data.</span><span class="sxs-lookup"><span data-stu-id="70fd9-134">Members can modify hello model schema and query all data.</span></span>|   
  
5.  <span data-ttu-id="70fd9-135">Pokud jste roli hello vytváření má ke čtení nebo oprávnění ke čtení a proces, můžete přidat řádkové filtry pomocí vzorce DAX.</span><span class="sxs-lookup"><span data-stu-id="70fd9-135">If hello role you are creating has Read or Read and Process permission, you can add row filters by using a DAX formula.</span></span> <span data-ttu-id="70fd9-136">Klikněte na tlačítko hello **řádkové filtry** kartě, pak vybrat tabulku, a pak klikněte na hello **DAX filtru** pole a potom zadat vzorec jazyka DAX.</span><span class="sxs-lookup"><span data-stu-id="70fd9-136">Click hello **Row Filters** tab, then select a table, then click hello **DAX Filter** field, and then type a DAX formula.</span></span>
  
6.  <span data-ttu-id="70fd9-137">Klikněte na tlačítko **členy** > **přidat externí**.</span><span class="sxs-lookup"><span data-stu-id="70fd9-137">Click **Members** > **Add External**.</span></span>  
  
8.  <span data-ttu-id="70fd9-138">V **přidat externího člena**, zadejte uživatele nebo skupiny v klientovi služby Azure AD pomocí e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="70fd9-138">In **Add External Member**, enter users or groups in your tenant Azure AD by email address.</span></span> <span data-ttu-id="70fd9-139">Po klikněte na tlačítko OK a zavřete Role správce, role a členy role se zobrazí v Průzkumníku tabulkový Model.</span><span class="sxs-lookup"><span data-stu-id="70fd9-139">After you click OK and close Role Manager, roles and role members appear in Tabular Model Explorer.</span></span> 
 
     ![Role a uživatele v tabulkovém Průzkumníka modelu](./media/analysis-services-database-users/aas-roles-tmexplorer.png)

9. <span data-ttu-id="70fd9-141">Nasaďte tooyour serveru Azure Analysis Services.</span><span class="sxs-lookup"><span data-stu-id="70fd9-141">Deploy tooyour Azure Analysis Services server.</span></span>


## <a name="tooadd-or-manage-roles-and-users-in-ssms"></a><span data-ttu-id="70fd9-142">tooadd nebo správě rolí a uživatelů v aplikaci SSMS</span><span class="sxs-lookup"><span data-stu-id="70fd9-142">tooadd or manage roles and users in SSMS</span></span>
<span data-ttu-id="70fd9-143">role a uživatelé tooa tooadd nasazené modelová databáze, musí být připojené toohello serveru, jako správce serveru nebo je již v databázové role s oprávněními správce.</span><span class="sxs-lookup"><span data-stu-id="70fd9-143">tooadd roles and users tooa deployed model database, you must be connected toohello server as a Server administrator or already in a database role with administrator permissions.</span></span>

1. <span data-ttu-id="70fd9-144">V objektu Exporer, klikněte pravým tlačítkem na **role** > **novou roli**.</span><span class="sxs-lookup"><span data-stu-id="70fd9-144">In Object Exporer, right-click **Roles** > **New Role**.</span></span>

2. <span data-ttu-id="70fd9-145">V **vytvořit roli**, zadejte název role a popis.</span><span class="sxs-lookup"><span data-stu-id="70fd9-145">In **Create Role**, enter a role name and description.</span></span>

3. <span data-ttu-id="70fd9-146">Vyberte oprávnění.</span><span class="sxs-lookup"><span data-stu-id="70fd9-146">Select a permission.</span></span>
   |<span data-ttu-id="70fd9-147">Oprávnění</span><span class="sxs-lookup"><span data-stu-id="70fd9-147">Permission</span></span>|<span data-ttu-id="70fd9-148">Popis</span><span class="sxs-lookup"><span data-stu-id="70fd9-148">Description</span></span>|  
   |----------------|-----------------|  
   |<span data-ttu-id="70fd9-149">**Úplné řízení (správce)**</span><span class="sxs-lookup"><span data-stu-id="70fd9-149">**Full control (Administrator)**</span></span>|<span data-ttu-id="70fd9-150">Členové mohou změnit schéma modelu hello, zpracování a může prohledávat všechna data.</span><span class="sxs-lookup"><span data-stu-id="70fd9-150">Members can modify hello model schema, process, and can query all data.</span></span>| 
   |<span data-ttu-id="70fd9-151">**Proces databáze**</span><span class="sxs-lookup"><span data-stu-id="70fd9-151">**Process database**</span></span>|<span data-ttu-id="70fd9-152">Členy můžete spustit proces a proces všechny operace.</span><span class="sxs-lookup"><span data-stu-id="70fd9-152">Members can run Process and Process All operations.</span></span> <span data-ttu-id="70fd9-153">Nelze změnit schéma modelu hello a nemůže zadat dotaz na data.</span><span class="sxs-lookup"><span data-stu-id="70fd9-153">Cannot modify hello model schema and cannot query data.</span></span>|  
   |<span data-ttu-id="70fd9-154">**Čtení**</span><span class="sxs-lookup"><span data-stu-id="70fd9-154">**Read**</span></span>|<span data-ttu-id="70fd9-155">Členy dotazy na data (podle řádkové filtry) ale nelze změnit schéma modelu hello.</span><span class="sxs-lookup"><span data-stu-id="70fd9-155">Members can query data (based on row filters) but cannot modify hello model schema.</span></span>|  
  
4. <span data-ttu-id="70fd9-156">Klikněte na tlačítko **členství**, pak zadejte uživatele nebo skupinu ve vašem klientovi Azure AD pomocí e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="70fd9-156">Click **Membership**, then enter a user or group in your tenant Azure AD by email address.</span></span>

     ![Přidání uživatele](./media/analysis-services-database-users/aas-roles-adduser-ssms.png)

5. <span data-ttu-id="70fd9-158">Pokud hello role, kterou vytváříte má oprávnění pro čtení, můžete přidat řádkové filtry pomocí vzorce DAX.</span><span class="sxs-lookup"><span data-stu-id="70fd9-158">If hello role you are creating has Read permission, you can add row filters by using a DAX formula.</span></span> <span data-ttu-id="70fd9-159">Klikněte na tlačítko **řádkové filtry**, vyberte tabulku a zadejte vzorec jazyka DAX hello **DAX filtru** pole.</span><span class="sxs-lookup"><span data-stu-id="70fd9-159">Click **Row Filters**, select a table, and then type a DAX formula in hello **DAX Filter** field.</span></span> 

## <a name="tooadd-roles-and-users-by-using-a-tmsl-script"></a><span data-ttu-id="70fd9-160">tooadd role a uživatele pomocí skriptu TMSL</span><span class="sxs-lookup"><span data-stu-id="70fd9-160">tooadd roles and users by using a TMSL script</span></span>
<span data-ttu-id="70fd9-161">Když spustíte skript TMSL v okně XMLA hello v aplikaci SSMS nebo pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="70fd9-161">You can run a TMSL script in hello XMLA window in SSMS or by using PowerShell.</span></span> <span data-ttu-id="70fd9-162">Použití hello [CreateOrReplace](https://docs.microsoft.com/sql/analysis-services/tabular-models-scripting-language-commands/createorreplace-command-tmsl) příkaz a hello [role](https://docs.microsoft.com/sql/analysis-services/tabular-models-scripting-language-objects/roles-object-tmsl) objektu.</span><span class="sxs-lookup"><span data-stu-id="70fd9-162">Use hello [CreateOrReplace](https://docs.microsoft.com/sql/analysis-services/tabular-models-scripting-language-commands/createorreplace-command-tmsl) command and hello [Roles](https://docs.microsoft.com/sql/analysis-services/tabular-models-scripting-language-objects/roles-object-tmsl) object.</span></span>

<span data-ttu-id="70fd9-163">**Ukázkový skript TMSL**</span><span class="sxs-lookup"><span data-stu-id="70fd9-163">**Sample TMSL script**</span></span>

<span data-ttu-id="70fd9-164">V této ukázce B2B externího uživatele a skupiny se přidají toohello analytik role s oprávnění ke čtení pro databázi SalesBI hello.</span><span class="sxs-lookup"><span data-stu-id="70fd9-164">In this sample, a B2B external user and a group are added toohello Analyst role with Read permissions for hello SalesBI database.</span></span> <span data-ttu-id="70fd9-165">Obě hello externího uživatele a skupiny musí být ve stejném klientovi Azure AD.</span><span class="sxs-lookup"><span data-stu-id="70fd9-165">Both hello external user and group must be in same tenant Azure AD.</span></span>

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

## <a name="tooadd-roles-and-users-by-using-powershell"></a><span data-ttu-id="70fd9-166">tooadd role a uživatele pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="70fd9-166">tooadd roles and users by using PowerShell</span></span>
<span data-ttu-id="70fd9-167">Hello [SqlServer](https://msdn.microsoft.com/library/hh758425.aspx) modul poskytuje databáze specifické pro úlohy správy rutiny a hello pro obecné účely Invoke-ASCmd rutiny, která přijímá dotazu tabulkový Model skriptovací jazyk (TMSL) nebo skriptu.</span><span class="sxs-lookup"><span data-stu-id="70fd9-167">hello [SqlServer](https://msdn.microsoft.com/library/hh758425.aspx) module provides task-specific database management cmdlets and hello general-purpose Invoke-ASCmd cmdlet that accepts a Tabular Model Scripting Language (TMSL) query or script.</span></span> <span data-ttu-id="70fd9-168">následující rutiny Hello se používají pro správu role databáze a uživatelů.</span><span class="sxs-lookup"><span data-stu-id="70fd9-168">hello following cmdlets are used for managing database roles and users.</span></span>
  
|<span data-ttu-id="70fd9-169">Rutina</span><span class="sxs-lookup"><span data-stu-id="70fd9-169">Cmdlet</span></span>|<span data-ttu-id="70fd9-170">Popis</span><span class="sxs-lookup"><span data-stu-id="70fd9-170">Description</span></span>|
|------------|-----------------| 
|[<span data-ttu-id="70fd9-171">Přidat RoleMember</span><span class="sxs-lookup"><span data-stu-id="70fd9-171">Add-RoleMember</span></span>](https://msdn.microsoft.com/library/hh510167.aspx)|<span data-ttu-id="70fd9-172">Přidání role databáze tooa člen.</span><span class="sxs-lookup"><span data-stu-id="70fd9-172">Add a member tooa database role.</span></span>| 
|[<span data-ttu-id="70fd9-173">Odebrat RoleMember</span><span class="sxs-lookup"><span data-stu-id="70fd9-173">Remove-RoleMember</span></span>](https://msdn.microsoft.com/library/hh510173.aspx)|<span data-ttu-id="70fd9-174">Odebrání člena z databázové role.</span><span class="sxs-lookup"><span data-stu-id="70fd9-174">Remove a member from a database role.</span></span>|   
|[<span data-ttu-id="70fd9-175">Vyvolání ASCmd</span><span class="sxs-lookup"><span data-stu-id="70fd9-175">Invoke-ASCmd</span></span>](https://msdn.microsoft.com/library/hh479579.aspx)|<span data-ttu-id="70fd9-176">Spuštění skriptu TMSL.</span><span class="sxs-lookup"><span data-stu-id="70fd9-176">Execute a TMSL script.</span></span>|

## <a name="row-filters"></a><span data-ttu-id="70fd9-177">Řádkové filtry</span><span class="sxs-lookup"><span data-stu-id="70fd9-177">Row filters</span></span>  
<span data-ttu-id="70fd9-178">Řádkové filtry definovat, které řádky v tabulce může být dotazován členy určité role.</span><span class="sxs-lookup"><span data-stu-id="70fd9-178">Row filters define which rows in a table can be queried by members of a particular role.</span></span> <span data-ttu-id="70fd9-179">Řádkové filtry jsou definovány pro každou tabulku v modelu s použitím vzorce DAX.</span><span class="sxs-lookup"><span data-stu-id="70fd9-179">Row filters are defined for each table in a model by using DAX formulas.</span></span>  
  
<span data-ttu-id="70fd9-180">Řádkové filtry lze definovat pouze pro role pro čtení a čtení a proces oprávnění.</span><span class="sxs-lookup"><span data-stu-id="70fd9-180">Row filters can be defined only for roles with Read and Read and Process permissions.</span></span> <span data-ttu-id="70fd9-181">Ve výchozím nastavení Pokud pro konkrétní tabulku, není definován řádkový filtr Členové můžete dotazovat všechny řádky v tabulce hello Pokud křížového filtru platí z jiné tabulky.</span><span class="sxs-lookup"><span data-stu-id="70fd9-181">By default, if a row filter is not defined for a particular table, members  can query all rows in hello table unless cross-filtering applies from another table.</span></span>
  
 <span data-ttu-id="70fd9-182">Řádkové filtry vyžadují vzorec jazyka DAX, který se musí vyhodnotit tooa hodnotu TRUE nebo FALSE, toodefine hello řádků, které může být dotazován členové této konkrétní roli.</span><span class="sxs-lookup"><span data-stu-id="70fd9-182">Row filters require a DAX formula, which must evaluate tooa TRUE/FALSE value, toodefine hello rows that can be queried by members of that particular role.</span></span> <span data-ttu-id="70fd9-183">Nelze se dotazovat na řádky, které nejsou součástí hello vzorec jazyka DAX.</span><span class="sxs-lookup"><span data-stu-id="70fd9-183">Rows not included in hello DAX formula cannot be queried.</span></span> <span data-ttu-id="70fd9-184">Například hello tabulku zákazníků s hello následující výrazu řádku filtry, *= zákazníků [Země] = "USA"*, členové role prodej hello uvidí jenom zákazníkům hello USA.</span><span class="sxs-lookup"><span data-stu-id="70fd9-184">For example, hello Customers table with hello following row filters expression, *=Customers [Country] = “USA”*, members of hello Sales role can only see customers in hello USA.</span></span>  
  
<span data-ttu-id="70fd9-185">Řádkové filtry použít toohello zadané řádky a související řádky.</span><span class="sxs-lookup"><span data-stu-id="70fd9-185">Row filters apply toohello specified rows and related rows.</span></span> <span data-ttu-id="70fd9-186">Pokud tabulka obsahuje více vztahů, použít filtry zabezpečení pro hello vztah, který je aktivní.</span><span class="sxs-lookup"><span data-stu-id="70fd9-186">When a table has multiple relationships, filters apply security for hello relationship that is active.</span></span> <span data-ttu-id="70fd9-187">Řádkové filtry se prolínají s další řádek filtrech definované pro tabulky v relaci, třeba:</span><span class="sxs-lookup"><span data-stu-id="70fd9-187">Row filters are intersected with other row filers defined for related tables, for example:</span></span>  
  
|<span data-ttu-id="70fd9-188">Table</span><span class="sxs-lookup"><span data-stu-id="70fd9-188">Table</span></span>|<span data-ttu-id="70fd9-189">Výraz jazyka DAX</span><span class="sxs-lookup"><span data-stu-id="70fd9-189">DAX expression</span></span>|  
|-----------|--------------------|  
|<span data-ttu-id="70fd9-190">Oblast</span><span class="sxs-lookup"><span data-stu-id="70fd9-190">Region</span></span>|<span data-ttu-id="70fd9-191">= Oblast [Země] = "USA"</span><span class="sxs-lookup"><span data-stu-id="70fd9-191">=Region[Country]=”USA”</span></span>|  
|<span data-ttu-id="70fd9-192">ProductCategory</span><span class="sxs-lookup"><span data-stu-id="70fd9-192">ProductCategory</span></span>|<span data-ttu-id="70fd9-193">= ProductCategory [Name] = "Jízdních kol"</span><span class="sxs-lookup"><span data-stu-id="70fd9-193">=ProductCategory[Name]=”Bicycles”</span></span>|  
|<span data-ttu-id="70fd9-194">Transakce</span><span class="sxs-lookup"><span data-stu-id="70fd9-194">Transactions</span></span>|<span data-ttu-id="70fd9-195">= Transakcí [rok] = 2016</span><span class="sxs-lookup"><span data-stu-id="70fd9-195">=Transactions[Year]=2016</span></span>|  
  
 <span data-ttu-id="70fd9-196">Hello net efekt je, že členové můžete dotazovat řádky dat, kde zákazník hello je hello USA, kategorie produktů hello je jízdních kol a rok hello je 2016.</span><span class="sxs-lookup"><span data-stu-id="70fd9-196">hello net effect is members can query rows of data where hello customer is in hello USA, hello product category is bicycles, and hello year is 2016.</span></span> <span data-ttu-id="70fd9-197">Uživatelé se nemohou dotazovat transakce mimo hello USA transakce, které nejsou jízdních kol nebo transakce není v 2016 dokud je členem jiné role, která uděluje tato oprávnění.</span><span class="sxs-lookup"><span data-stu-id="70fd9-197">Users cannot query transactions outside of hello USA, transactions that are not bicycles, or transactions not in 2016 unless they are a member of another role that grants these permissions.</span></span>
  
 <span data-ttu-id="70fd9-198">Můžete použít možnosti filtrovat hello *=FALSE()*, toodeny přístup tooall řádků pro celou tabulku.</span><span class="sxs-lookup"><span data-stu-id="70fd9-198">You can use hello filter, *=FALSE()*, toodeny access tooall rows for an entire table.</span></span>

## <a name="next-steps"></a><span data-ttu-id="70fd9-199">Další kroky</span><span class="sxs-lookup"><span data-stu-id="70fd9-199">Next steps</span></span>
  <span data-ttu-id="70fd9-200">[Spravovat správce serveru](analysis-services-server-admins.md) </span><span class="sxs-lookup"><span data-stu-id="70fd9-200">[Manage server administrators](analysis-services-server-admins.md) </span></span>  
  [<span data-ttu-id="70fd9-201">Správa služby Azure Analysis Services pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="70fd9-201">Manage Azure Analysis Services with PowerShell</span></span>](analysis-services-powershell.md)  
  [<span data-ttu-id="70fd9-202">Tabulkový Model skriptování referenční příručka jazyka (TMSL)</span><span class="sxs-lookup"><span data-stu-id="70fd9-202">Tabular Model Scripting Language (TMSL) Reference</span></span>](https://docs.microsoft.com/sql/analysis-services/tabular-model-scripting-language-tmsl-reference)

