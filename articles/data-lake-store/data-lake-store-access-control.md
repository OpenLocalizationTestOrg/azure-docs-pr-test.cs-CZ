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
# <a name="access-control-in-azure-data-lake-store"></a><span data-ttu-id="a94db-103">Řízení přístupu v Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="a94db-103">Access control in Azure Data Lake Store</span></span>

<span data-ttu-id="a94db-104">Azure Data Lake Store implementuje model řízení přístupu, která je odvozena z HDFS, která zase je odvozena z model řízení přístupu POSIX hello.</span><span class="sxs-lookup"><span data-stu-id="a94db-104">Azure Data Lake Store implements an access control model that derives from HDFS, which in turn derives from hello POSIX access control model.</span></span> <span data-ttu-id="a94db-105">Tento článek obsahuje souhrn hello základy hello model řízení přístupu pro Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="a94db-105">This article summarizes hello basics of hello access control model for Data Lake Store.</span></span> <span data-ttu-id="a94db-106">toolearn Další informace o hello HDFS model řízení přístupu, najdete v části [HDFS oprávnění průvodce](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html).</span><span class="sxs-lookup"><span data-stu-id="a94db-106">toolearn more about hello HDFS access control model, see [HDFS Permissions Guide](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html).</span></span>

## <a name="access-control-lists-on-files-and-folders"></a><span data-ttu-id="a94db-107">Seznamy řízení přístupu k souborům a složkám</span><span class="sxs-lookup"><span data-stu-id="a94db-107">Access control lists on files and folders</span></span>

<span data-ttu-id="a94db-108">Existují dva druhy seznamů řízení přístupu (ACL) – **přístupové seznamy ACL** a **výchozí seznamy ACL**.</span><span class="sxs-lookup"><span data-stu-id="a94db-108">There are two kinds of access control lists (ACLs), **Access ACLs** and **Default ACLs**.</span></span>

* <span data-ttu-id="a94db-109">**Přístup k seznamy ACL**: objekt tooan přístup tyto ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="a94db-109">**Access ACLs**: These control access tooan object.</span></span> <span data-ttu-id="a94db-110">Přístupové seznamy ACL jsou definovány pro soubory i složky.</span><span class="sxs-lookup"><span data-stu-id="a94db-110">Files and folders both have Access ACLs.</span></span>

* <span data-ttu-id="a94db-111">**Výchozí seznamy ACL**: "Šablonu" seznamů ACL přidružené složce, která určit hello seznamy ACL přístupu pro všechny podřízené položky, které jsou vytvořené v této složce.</span><span class="sxs-lookup"><span data-stu-id="a94db-111">**Default ACLs**: A "template" of ACLs associated with a folder that determine hello Access ACLs for any child items that are created under that folder.</span></span> <span data-ttu-id="a94db-112">Výchozí seznamy ACL nejsou definovány pro soubory.</span><span class="sxs-lookup"><span data-stu-id="a94db-112">Files do not have Default ACLs.</span></span>

![Seznamy ACL služby Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-1.png)

<span data-ttu-id="a94db-114">Seznamy ACL přístupu a výchozí seznamy ACL mají hello stejnou strukturu.</span><span class="sxs-lookup"><span data-stu-id="a94db-114">Both Access ACLs and Default ACLs have hello same structure.</span></span>

![Seznamy ACL služby Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-2.png)



> [!NOTE]
> <span data-ttu-id="a94db-116">Změna hello výchozí seznam ACL u nadřazené neovlivňuje hello přístupu ACL nebo výchozí seznam ACL podřízených položek, které již existují.</span><span class="sxs-lookup"><span data-stu-id="a94db-116">Changing hello Default ACL on a parent does not affect hello Access ACL or Default ACL of child items that already exist.</span></span>
>
>

## <a name="users-and-identities"></a><span data-ttu-id="a94db-117">Uživatelé a identity</span><span class="sxs-lookup"><span data-stu-id="a94db-117">Users and identities</span></span>

<span data-ttu-id="a94db-118">Každý soubor a složka má samostatná oprávnění pro tyto identity:</span><span class="sxs-lookup"><span data-stu-id="a94db-118">Every file and folder has distinct permissions for these identities:</span></span>

* <span data-ttu-id="a94db-119">Hello vlastnícího uživatele hello souboru</span><span class="sxs-lookup"><span data-stu-id="a94db-119">hello owning user of hello file</span></span>
* <span data-ttu-id="a94db-120">Hello vlastnícím skupiny</span><span class="sxs-lookup"><span data-stu-id="a94db-120">hello owning group</span></span>
* <span data-ttu-id="a94db-121">Pojmenovaní uživatelé</span><span class="sxs-lookup"><span data-stu-id="a94db-121">Named users</span></span>
* <span data-ttu-id="a94db-122">Pojmenované skupiny</span><span class="sxs-lookup"><span data-stu-id="a94db-122">Named groups</span></span>
* <span data-ttu-id="a94db-123">Všichni ostatní uživatelé</span><span class="sxs-lookup"><span data-stu-id="a94db-123">All other users</span></span>

<span data-ttu-id="a94db-124">Hello identity uživatelů a skupin jsou identit Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="a94db-124">hello identities of users and groups are Azure Active Directory (Azure AD) identities.</span></span> <span data-ttu-id="a94db-125">Takže pokud není uvedeno jinak, "user," v kontextu hello Data Lake Store, můžete buď znamenat uživatele Azure AD nebo skupiny zabezpečení služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a94db-125">So unless otherwise noted, a "user," in hello context of Data Lake Store, can either mean an Azure AD user or an Azure AD security group.</span></span>

## <a name="permissions"></a><span data-ttu-id="a94db-126">Oprávnění</span><span class="sxs-lookup"><span data-stu-id="a94db-126">Permissions</span></span>

<span data-ttu-id="a94db-127">Hello oprávnění pro objekt systému souborů jsou **čtení**, **zápisu**, a **Execute**, a použitím na soubory a složky, jak je znázorněno v následující tabulce hello:</span><span class="sxs-lookup"><span data-stu-id="a94db-127">hello permissions on a filesystem object are **Read**, **Write**, and **Execute**, and they can be used on files and folders as shown in hello following table:</span></span>

|            |    <span data-ttu-id="a94db-128">File</span><span class="sxs-lookup"><span data-stu-id="a94db-128">File</span></span>     |   <span data-ttu-id="a94db-129">Složka</span><span class="sxs-lookup"><span data-stu-id="a94db-129">Folder</span></span> |
|------------|-------------|----------|
| <span data-ttu-id="a94db-130">**Číst (R)**</span><span class="sxs-lookup"><span data-stu-id="a94db-130">**Read (R)**</span></span> | <span data-ttu-id="a94db-131">Může číst hello obsah souboru</span><span class="sxs-lookup"><span data-stu-id="a94db-131">Can read hello contents of a file</span></span> | <span data-ttu-id="a94db-132">Vyžaduje **čtení** a **Execute** toolist hello obsah složky hello</span><span class="sxs-lookup"><span data-stu-id="a94db-132">Requires **Read** and **Execute** toolist hello contents of hello folder</span></span>|
| <span data-ttu-id="a94db-133">**Zapisovat (W)**</span><span class="sxs-lookup"><span data-stu-id="a94db-133">**Write (W)**</span></span> | <span data-ttu-id="a94db-134">Můžete zapsat nebo připojit soubor tooa</span><span class="sxs-lookup"><span data-stu-id="a94db-134">Can write or append tooa file</span></span> | <span data-ttu-id="a94db-135">Vyžaduje **zápisu** a **Execute** toocreate podřízené položky ve složce</span><span class="sxs-lookup"><span data-stu-id="a94db-135">Requires **Write** and **Execute** toocreate child items in a folder</span></span> |
| <span data-ttu-id="a94db-136">**Provést (X)**</span><span class="sxs-lookup"><span data-stu-id="a94db-136">**Execute (X)**</span></span> | <span data-ttu-id="a94db-137">Neznamená nic v kontextu hello Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="a94db-137">Does not mean anything in hello context of Data Lake Store</span></span> | <span data-ttu-id="a94db-138">Požadované tootraverse hello podřízené položky složky</span><span class="sxs-lookup"><span data-stu-id="a94db-138">Required tootraverse hello child items of a folder</span></span> |

### <a name="short-forms-for-permissions"></a><span data-ttu-id="a94db-139">Zkrácené verze oprávnění</span><span class="sxs-lookup"><span data-stu-id="a94db-139">Short forms for permissions</span></span>

<span data-ttu-id="a94db-140">**RWX** je použité tooindicate **čtení a zápisu + provést**.</span><span class="sxs-lookup"><span data-stu-id="a94db-140">**RWX** is used tooindicate **Read + Write + Execute**.</span></span> <span data-ttu-id="a94db-141">Existuje více zhuštěné číselný tvar ve kterém **čtení = 4**, **zápisu = 2**, a **Execute = 1**, jejichž hello součet představuje hello oprávnění.</span><span class="sxs-lookup"><span data-stu-id="a94db-141">A more condensed numeric form exists in which **Read=4**, **Write=2**, and **Execute=1**, hello sum of which represents hello permissions.</span></span> <span data-ttu-id="a94db-142">Dále je uvedeno několik příkladů.</span><span class="sxs-lookup"><span data-stu-id="a94db-142">Following are some examples.</span></span>

| <span data-ttu-id="a94db-143">Číselný tvar</span><span class="sxs-lookup"><span data-stu-id="a94db-143">Numeric form</span></span> | <span data-ttu-id="a94db-144">Krátký tvar</span><span class="sxs-lookup"><span data-stu-id="a94db-144">Short form</span></span> |      <span data-ttu-id="a94db-145">Význam</span><span class="sxs-lookup"><span data-stu-id="a94db-145">What it means</span></span>     |
|--------------|------------|------------------------|
| <span data-ttu-id="a94db-146">7</span><span class="sxs-lookup"><span data-stu-id="a94db-146">7</span></span>            | <span data-ttu-id="a94db-147">RWX</span><span class="sxs-lookup"><span data-stu-id="a94db-147">RWX</span></span>        | <span data-ttu-id="a94db-148">Číst + Zapisovat + Provést</span><span class="sxs-lookup"><span data-stu-id="a94db-148">Read + Write + Execute</span></span> |
| <span data-ttu-id="a94db-149">5</span><span class="sxs-lookup"><span data-stu-id="a94db-149">5</span></span>            | <span data-ttu-id="a94db-150">R-X</span><span class="sxs-lookup"><span data-stu-id="a94db-150">R-X</span></span>        | <span data-ttu-id="a94db-151">Číst + Provést</span><span class="sxs-lookup"><span data-stu-id="a94db-151">Read + Execute</span></span>         |
| <span data-ttu-id="a94db-152">4</span><span class="sxs-lookup"><span data-stu-id="a94db-152">4</span></span>            | <span data-ttu-id="a94db-153">R--</span><span class="sxs-lookup"><span data-stu-id="a94db-153">R--</span></span>        | <span data-ttu-id="a94db-154">Čtení</span><span class="sxs-lookup"><span data-stu-id="a94db-154">Read</span></span>                   |
| <span data-ttu-id="a94db-155">0</span><span class="sxs-lookup"><span data-stu-id="a94db-155">0</span></span>            | ---        | <span data-ttu-id="a94db-156">Žádná oprávnění</span><span class="sxs-lookup"><span data-stu-id="a94db-156">No permissions</span></span>         |


### <a name="permissions-do-not-inherit"></a><span data-ttu-id="a94db-157">Oprávnění se nedědí</span><span class="sxs-lookup"><span data-stu-id="a94db-157">Permissions do not inherit</span></span>

<span data-ttu-id="a94db-158">V modelu stylu POSIX hello, který je používán Data Lake Store oprávnění pro položku se ukládají na samotnou položku hello.</span><span class="sxs-lookup"><span data-stu-id="a94db-158">In hello POSIX-style model that's used by Data Lake Store, permissions for an item are stored on hello item itself.</span></span> <span data-ttu-id="a94db-159">Jinými slovy oprávnění pro položku nelze možné zdědit z nadřazené položky hello.</span><span class="sxs-lookup"><span data-stu-id="a94db-159">In other words, permissions for an item cannot be inherited from hello parent items.</span></span>

## <a name="common-scenarios-related-toopermissions"></a><span data-ttu-id="a94db-160">Běžné scénáře související toopermissions</span><span class="sxs-lookup"><span data-stu-id="a94db-160">Common scenarios related toopermissions</span></span>

<span data-ttu-id="a94db-161">Tady jsou některé běžné scénáře toohelp porozumíte oprávnění, která jsou potřeba tooperform určité operace v účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="a94db-161">Following are some common scenarios toohelp you understand which permissions are needed tooperform certain operations on a Data Lake Store account.</span></span>

### <a name="permissions-needed-tooread-a-file"></a><span data-ttu-id="a94db-162">Oprávnění potřebná tooread soubor</span><span class="sxs-lookup"><span data-stu-id="a94db-162">Permissions needed tooread a file</span></span>

![Seznamy ACL služby Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-3.png)

* <span data-ttu-id="a94db-164">Pro soubor toobe hello číst, hello volající musí **čtení** oprávnění.</span><span class="sxs-lookup"><span data-stu-id="a94db-164">For hello file toobe read, hello caller needs **Read** permissions.</span></span>
* <span data-ttu-id="a94db-165">Pro všechny hello složkách v hello struktuře, které obsahují hello souboru, hello volající musí **Execute** oprávnění.</span><span class="sxs-lookup"><span data-stu-id="a94db-165">For all hello folders in hello folder structure that contain hello file, hello caller needs **Execute** permissions.</span></span>

### <a name="permissions-needed-tooappend-tooa-file"></a><span data-ttu-id="a94db-166">Oprávnění potřebná tooappend tooa souboru</span><span class="sxs-lookup"><span data-stu-id="a94db-166">Permissions needed tooappend tooa file</span></span>

![Seznamy ACL služby Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-4.png)

* <span data-ttu-id="a94db-168">Pro soubor toobe hello připojí se k němu, hello volající musí **zápisu** oprávnění.</span><span class="sxs-lookup"><span data-stu-id="a94db-168">For hello file toobe appended to, hello caller needs **Write** permissions.</span></span>
* <span data-ttu-id="a94db-169">Pro všechny hello složek, které obsahují hello souboru, hello volající musí **Execute** oprávnění.</span><span class="sxs-lookup"><span data-stu-id="a94db-169">For all hello folders that contain hello file, hello caller needs **Execute** permissions.</span></span>

### <a name="permissions-needed-toodelete-a-file"></a><span data-ttu-id="a94db-170">Oprávnění potřebná toodelete soubor</span><span class="sxs-lookup"><span data-stu-id="a94db-170">Permissions needed toodelete a file</span></span>

![Seznamy ACL služby Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-5.png)

* <span data-ttu-id="a94db-172">Hello nadřazené složky hello volající musí **zápisu + provést** oprávnění.</span><span class="sxs-lookup"><span data-stu-id="a94db-172">For hello parent folder, hello caller needs **Write + Execute** permissions.</span></span>
* <span data-ttu-id="a94db-173">Pro všechny hello jiné složky v cestě hello souboru, hello volající musí **Execute** oprávnění.</span><span class="sxs-lookup"><span data-stu-id="a94db-173">For all hello other folders in hello file’s path, hello caller needs **Execute** permissions.</span></span>



> [!NOTE]
> <span data-ttu-id="a94db-174">Zapsat oprávnění u souboru hello nejsou požadované toodelete ji, pokud hello předchozí dvě podmínky jsou splněny.</span><span class="sxs-lookup"><span data-stu-id="a94db-174">Write permissions on hello file are not required toodelete it as long as hello previous two conditions are true.</span></span>
>
>

### <a name="permissions-needed-tooenumerate-a-folder"></a><span data-ttu-id="a94db-175">Oprávnění potřebná tooenumerate složku</span><span class="sxs-lookup"><span data-stu-id="a94db-175">Permissions needed tooenumerate a folder</span></span>

![Seznamy ACL služby Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-6.png)

* <span data-ttu-id="a94db-177">Pro složku tooenumerate hello, hello volající musí **čtení + Execute** oprávnění.</span><span class="sxs-lookup"><span data-stu-id="a94db-177">For hello folder tooenumerate, hello caller needs **Read + Execute** permissions.</span></span>
* <span data-ttu-id="a94db-178">Pro všechny hello nadřazené složky, hello volající musí **Execute** oprávnění.</span><span class="sxs-lookup"><span data-stu-id="a94db-178">For all hello ancestor folders, hello caller needs **Execute** permissions.</span></span>

## <a name="viewing-permissions-in-hello-azure-portal"></a><span data-ttu-id="a94db-179">Oprávnění k zobrazení v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="a94db-179">Viewing permissions in hello Azure portal</span></span>

<span data-ttu-id="a94db-180">Z hello **Průzkumníku dat** okno hello účtu Data Lake Store, klikněte na tlačítko **přístup** toosee hello seznamy ACL na soubor nebo složku.</span><span class="sxs-lookup"><span data-stu-id="a94db-180">From hello **Data Explorer** blade of hello Data Lake Store account, click **Access** toosee hello ACLs for a file or a folder.</span></span> <span data-ttu-id="a94db-181">Klikněte na tlačítko **přístup** toosee hello seznamy ACL pro hello **katalogu** ve složce hello **mydatastore** účtu.</span><span class="sxs-lookup"><span data-stu-id="a94db-181">Click **Access** toosee hello ACLs for hello **catalog** folder under hello **mydatastore** account.</span></span>

![Seznamy ACL služby Data Lake Store](./media/data-lake-store-access-control/data-lake-store-show-acls-1.png)

<span data-ttu-id="a94db-183">V tomto okně hello horní části zobrazí přehled hello oprávnění, které máte.</span><span class="sxs-lookup"><span data-stu-id="a94db-183">On this blade, hello top section shows an overview of hello permissions that you have.</span></span> <span data-ttu-id="a94db-184">(Na snímku obrazovky hello hello uživatel je Bob.) Následující, která se zobrazí hello přístupová oprávnění.</span><span class="sxs-lookup"><span data-stu-id="a94db-184">(In hello screenshot, hello user is Bob.) Following that, hello access permissions are shown.</span></span> <span data-ttu-id="a94db-185">Potom z hello **přístup** okně klikněte na tlačítko **jednoduché zobrazení** toosee hello jednodušší zobrazení.</span><span class="sxs-lookup"><span data-stu-id="a94db-185">After that, from hello **Access** blade, click **Simple View** toosee hello simpler view.</span></span>

![Seznamy ACL služby Data Lake Store](./media/data-lake-store-access-control/data-lake-store-show-acls-simple-view.png)

<span data-ttu-id="a94db-187">Klikněte na tlačítko **rozšířené zobrazení** toosee hello pokročilejší zobrazení, kde jsou uvedeny koncepty hello výchozí seznamy ACL, maska a superuživatele.</span><span class="sxs-lookup"><span data-stu-id="a94db-187">Click **Advanced View** toosee hello more advanced view, where hello concepts of Default ACLs, mask, and super-user are shown.</span></span>

![Seznamy ACL služby Data Lake Store](./media/data-lake-store-access-control/data-lake-store-show-acls-advance-view.png)

## <a name="hello-super-user"></a><span data-ttu-id="a94db-189">Hello superuživatele</span><span class="sxs-lookup"><span data-stu-id="a94db-189">hello super-user</span></span>

<span data-ttu-id="a94db-190">Nadřazený uživatel má většina práva všem uživatelům hello hello v hello Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="a94db-190">A super-user has hello most rights of all hello users in hello Data Lake Store.</span></span> <span data-ttu-id="a94db-191">Superuživatel:</span><span class="sxs-lookup"><span data-stu-id="a94db-191">A super-user:</span></span>

* <span data-ttu-id="a94db-192">Má oprávnění RWX příliš**všechny** soubory a složky.</span><span class="sxs-lookup"><span data-stu-id="a94db-192">Has RWX Permissions too**all** files and folders.</span></span>
* <span data-ttu-id="a94db-193">Můžete změnit oprávnění hello na libovolný soubor nebo složku.</span><span class="sxs-lookup"><span data-stu-id="a94db-193">Can change hello permissions on any file or folder.</span></span>
* <span data-ttu-id="a94db-194">Můžete změnit hello vlastnícího uživatele nebo který je vlastníkem skupiny libovolný soubor nebo složku.</span><span class="sxs-lookup"><span data-stu-id="a94db-194">Can change hello owning user or owning group of any file or folder.</span></span>

<span data-ttu-id="a94db-195">V Azure má účet Data Lake Store několik rolí Azure, včetně rolí:</span><span class="sxs-lookup"><span data-stu-id="a94db-195">In Azure, a Data Lake Store account has several Azure roles, including:</span></span>

* <span data-ttu-id="a94db-196">Vlastníci</span><span class="sxs-lookup"><span data-stu-id="a94db-196">Owners</span></span>
* <span data-ttu-id="a94db-197">Přispěvatelé</span><span class="sxs-lookup"><span data-stu-id="a94db-197">Contributors</span></span>
* <span data-ttu-id="a94db-198">Čtenáři</span><span class="sxs-lookup"><span data-stu-id="a94db-198">Readers</span></span>

<span data-ttu-id="a94db-199">Všichni uživatelé v hello **vlastníky** role pro účet Data Lake Store je automaticky superuživatele pro tento účet.</span><span class="sxs-lookup"><span data-stu-id="a94db-199">Everyone in hello **Owners** role for a Data Lake Store account is automatically a super-user for that account.</span></span> <span data-ttu-id="a94db-200">Další, najdete v části toolearn [řízení přístupu na základě Role](../active-directory/role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="a94db-200">toolearn more, see [Role-based access control](../active-directory/role-based-access-control-configure.md).</span></span>
<span data-ttu-id="a94db-201">Pokud chcete toocreate roli vlastního ovládacího prvku na základě přístupu rolí (RBAC), který má oprávnění superuživatele, potřebuje toohave hello následující oprávnění:</span><span class="sxs-lookup"><span data-stu-id="a94db-201">If you want toocreate a custom role-based-access control (RBAC) role that has super-user permissions, it needs toohave hello following permissions:</span></span>
- <span data-ttu-id="a94db-202">Microsoft.DataLakeStore/accounts/Superuser/action</span><span class="sxs-lookup"><span data-stu-id="a94db-202">Microsoft.DataLakeStore/accounts/Superuser/action</span></span>
- <span data-ttu-id="a94db-203">Microsoft.Authorization/roleAssignments/write</span><span class="sxs-lookup"><span data-stu-id="a94db-203">Microsoft.Authorization/roleAssignments/write</span></span>


## <a name="hello-owning-user"></a><span data-ttu-id="a94db-204">Hello vlastnícím uživatele</span><span class="sxs-lookup"><span data-stu-id="a94db-204">hello owning user</span></span>

<span data-ttu-id="a94db-205">Hello uživatele, který vytvořil položku hello je automaticky hello vlastnícího uživatele hello položky.</span><span class="sxs-lookup"><span data-stu-id="a94db-205">hello user who created hello item is automatically hello owning user of hello item.</span></span> <span data-ttu-id="a94db-206">Vlastnící uživatel může:</span><span class="sxs-lookup"><span data-stu-id="a94db-206">An owning user can:</span></span>

* <span data-ttu-id="a94db-207">Změnit hello oprávnění k souboru, který je vlastněn.</span><span class="sxs-lookup"><span data-stu-id="a94db-207">Change hello permissions of a file that is owned.</span></span>
* <span data-ttu-id="a94db-208">Změnit hello vlastnící skupinu souboru, který je vlastněn, tak dlouho, dokud uživatel vlastnícím hello je také členem hello cílovou skupinu.</span><span class="sxs-lookup"><span data-stu-id="a94db-208">Change hello owning group of a file that is owned, as long as hello owning user is also a member of hello target group.</span></span>

> [!NOTE]
> <span data-ttu-id="a94db-209">Hello vlastnícího uživatele *nelze* změnit hello vlastnícího uživatele jiného ve vlastnictví souboru.</span><span class="sxs-lookup"><span data-stu-id="a94db-209">hello owning user *cannot* change hello owning user of another owned file.</span></span> <span data-ttu-id="a94db-210">Pouze nadtypem uživatelé mohou změnit hello vlastnící uživatel k souboru nebo složce.</span><span class="sxs-lookup"><span data-stu-id="a94db-210">Only super-users can change hello owning user of a file or folder.</span></span>
>
>

## <a name="hello-owning-group"></a><span data-ttu-id="a94db-211">Hello vlastnícím skupiny</span><span class="sxs-lookup"><span data-stu-id="a94db-211">hello owning group</span></span>

<span data-ttu-id="a94db-212">V hello POSIX seznamy ACL každý uživatel je přidružen k "primární skupinu".</span><span class="sxs-lookup"><span data-stu-id="a94db-212">In hello POSIX ACLs, every user is associated with a "primary group."</span></span> <span data-ttu-id="a94db-213">Například uživatel "alice" může patřit toohello "finanční" skupiny.</span><span class="sxs-lookup"><span data-stu-id="a94db-213">For example, user "alice" might belong toohello "finance" group.</span></span> <span data-ttu-id="a94db-214">Alice můžou taky patřit toomultiple skupiny, ale jedna skupina je vždy určen jako svůj primární skupiny.</span><span class="sxs-lookup"><span data-stu-id="a94db-214">Alice might also belong toomultiple groups, but one group is always designated as her primary group.</span></span> <span data-ttu-id="a94db-215">V POSIX když Alice vytvoří soubor, nastavte hello vlastnící skupinu pro tohoto souboru je tooher primární skupinu, která je v tomto případě "finanční".</span><span class="sxs-lookup"><span data-stu-id="a94db-215">In POSIX, when Alice creates a file, hello owning group of that file is set tooher primary group, which in this case is "finance."</span></span>

<span data-ttu-id="a94db-216">Při vytvoření nové položky filesystem Data Lake Store přiřadí hodnota toohello, který je vlastníkem skupiny.</span><span class="sxs-lookup"><span data-stu-id="a94db-216">When a new filesystem item is created, Data Lake Store assigns a value toohello owning group.</span></span>

* <span data-ttu-id="a94db-217">**Případ 1**: hello kořenové složky "/".</span><span class="sxs-lookup"><span data-stu-id="a94db-217">**Case 1**: hello root folder "/".</span></span> <span data-ttu-id="a94db-218">Tato složka se vytvoří při vytvoření účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="a94db-218">This folder is created when a Data Lake Store account is created.</span></span> <span data-ttu-id="a94db-219">V takovém případě hello vlastnícím skupina nastavení toohello uživatele, který vytvořil účet hello.</span><span class="sxs-lookup"><span data-stu-id="a94db-219">In this case, hello owning group is set toohello user who created hello account.</span></span>
* <span data-ttu-id="a94db-220">**Případ 2** (každých dalších případ): při vytváření nové položky vlastnícím skupina hello se zkopíruje z hello nadřazené složky.</span><span class="sxs-lookup"><span data-stu-id="a94db-220">**Case 2** (Every other case): When a new item is created, hello owning group is copied from hello parent folder.</span></span>

<span data-ttu-id="a94db-221">Hello vlastnícím skupiny může změnit:</span><span class="sxs-lookup"><span data-stu-id="a94db-221">hello owning group can be changed by:</span></span>
* <span data-ttu-id="a94db-222">Všichni superuživatelé.</span><span class="sxs-lookup"><span data-stu-id="a94db-222">Any super-users.</span></span>
* <span data-ttu-id="a94db-223">Hello vlastnícího uživatele, pokud uživatel vlastnícím hello také členem hello cílovou skupinu.</span><span class="sxs-lookup"><span data-stu-id="a94db-223">hello owning user, if hello owning user is also a member of hello target group.</span></span>

## <a name="access-check-algorithm"></a><span data-ttu-id="a94db-224">Algoritmus kontroly přístupu</span><span class="sxs-lookup"><span data-stu-id="a94db-224">Access check algorithm</span></span>

<span data-ttu-id="a94db-225">Hello následující obrázek představuje hello přístupu zkontrolujte algoritmus pro účty Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="a94db-225">hello following illustration represents hello access check algorithm for Data Lake Store accounts.</span></span>

![Algoritmus seznamů ACL služby Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-algorithm.png)


## <a name="hello-mask-and-effective-permissions"></a><span data-ttu-id="a94db-227">Maska Hello a "skutečná oprávnění"</span><span class="sxs-lookup"><span data-stu-id="a94db-227">hello mask and "effective permissions"</span></span>

<span data-ttu-id="a94db-228">Hello **maska** je RWX hodnoty používané toolimit přístup pro který je **s názvem Uživatelé**, hello **vlastnící skupinu**, a **s názvem skupiny** až budete algoritmus kontroly přístupu k provádění hello.</span><span class="sxs-lookup"><span data-stu-id="a94db-228">hello **mask** is an RWX value that is used toolimit access for **named users**, hello **owning group**, and **named groups** when you're performing hello access check algorithm.</span></span> <span data-ttu-id="a94db-229">Tady jsou klíčové koncepty hello pro masku hello.</span><span class="sxs-lookup"><span data-stu-id="a94db-229">Here are hello key concepts for hello mask.</span></span>

* <span data-ttu-id="a94db-230">Maska Hello vytvoří "skutečná oprávnění."</span><span class="sxs-lookup"><span data-stu-id="a94db-230">hello mask creates "effective permissions."</span></span> <span data-ttu-id="a94db-231">To znamená upraví hello oprávnění v době hello kontrolu přístupu.</span><span class="sxs-lookup"><span data-stu-id="a94db-231">That is, it modifies hello permissions at hello time of access check.</span></span>
* <span data-ttu-id="a94db-232">Maska Hello lze přímo upravit hello vlastníka souboru a všechny nadtypem uživatele.</span><span class="sxs-lookup"><span data-stu-id="a94db-232">hello mask can be directly edited by hello file owner and any super-users.</span></span>
* <span data-ttu-id="a94db-233">Maska Hello můžete odebrat oprávnění toocreate hello efektivní oprávnění.</span><span class="sxs-lookup"><span data-stu-id="a94db-233">hello mask can remove permissions toocreate hello effective permission.</span></span> <span data-ttu-id="a94db-234">Maska Hello *nelze* přidejte oprávnění toohello efektivní oprávnění.</span><span class="sxs-lookup"><span data-stu-id="a94db-234">hello mask *cannot* add permissions toohello effective permission.</span></span>

<span data-ttu-id="a94db-235">Podívejme se na několik příkladů.</span><span class="sxs-lookup"><span data-stu-id="a94db-235">Let's look at some examples.</span></span> <span data-ttu-id="a94db-236">V následujícím příkladu hello, hello maska je nastaven příliš**RWX**, což znamená, že maska hello neodebere žádné oprávnění.</span><span class="sxs-lookup"><span data-stu-id="a94db-236">In hello following example, hello mask is set too**RWX**, which means that hello mask does not remove any permissions.</span></span> <span data-ttu-id="a94db-237">Hello skutečná oprávnění pro uživatele, který vlastní skupinu a s názvem skupiny s názvem hello nejsou změněna během kontroly přístupu hello.</span><span class="sxs-lookup"><span data-stu-id="a94db-237">hello effective permissions for hello named user, owning group, and named group are not altered during hello access check.</span></span>

![Seznamy ACL služby Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-mask-1.png)

<span data-ttu-id="a94db-239">V následujícím příkladu hello, hello maska je nastaven příliš**R-X**.</span><span class="sxs-lookup"><span data-stu-id="a94db-239">In hello following example, hello mask is set too**R-X**.</span></span> <span data-ttu-id="a94db-240">To znamená, že IT oddělení **vypne oprávnění k zápisu hello** pro **pojmenovaného uživatele**, **vlastnící skupinu**, a **s názvem skupiny** během hello přístupu Zkontrolujte.</span><span class="sxs-lookup"><span data-stu-id="a94db-240">This means that it **turns off hello Write permissions** for **named user**, **owning group**, and **named group** at hello time of access check.</span></span>

![Seznamy ACL služby Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-mask-2.png)

<span data-ttu-id="a94db-242">Pro referenci tady je, kde se zobrazí hello masky pro soubor nebo složku v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="a94db-242">For reference, here is where hello mask for a file or folder appears in hello Azure portal.</span></span>

![Seznamy ACL služby Data Lake Store](./media/data-lake-store-access-control/data-lake-store-show-acls-mask-view.png)

> [!NOTE]
> <span data-ttu-id="a94db-244">Pro nový účet Data Lake Store, hello maska pro hello přístupu ACL a výchozí seznam ACL hello kořenové složky ("/"), použije výchozí hodnota tooRWX.</span><span class="sxs-lookup"><span data-stu-id="a94db-244">For a new Data Lake Store account, hello mask for hello Access ACL and Default ACL of hello root folder ("/") defaults tooRWX.</span></span>
>
>

## <a name="permissions-on-new-files-and-folders"></a><span data-ttu-id="a94db-245">Oprávnění pro nové soubory a složky</span><span class="sxs-lookup"><span data-stu-id="a94db-245">Permissions on new files and folders</span></span>

<span data-ttu-id="a94db-246">Při vytváření nového souboru nebo složky pod existující složku hello výchozí seznam ACL u hello nadřazené složky určuje:</span><span class="sxs-lookup"><span data-stu-id="a94db-246">When a new file or folder is created under an existing folder, hello Default ACL on hello parent folder determines:</span></span>

- <span data-ttu-id="a94db-247">Výchozí seznam ACL a přístupový seznam ACL podřízené složky.</span><span class="sxs-lookup"><span data-stu-id="a94db-247">A child folder’s Default ACL and Access ACL.</span></span>
- <span data-ttu-id="a94db-248">Přístupový seznam ACL podřízeného souboru (pro soubory není definován výchozí seznam ACL).</span><span class="sxs-lookup"><span data-stu-id="a94db-248">A child file's Access ACL (files do not have a Default ACL).</span></span>

### <a name="hello-access-acl-of-a-child-file-or-folder"></a><span data-ttu-id="a94db-249">Hello přístupu ACL podřízené souboru nebo složky</span><span class="sxs-lookup"><span data-stu-id="a94db-249">hello Access ACL of a child file or folder</span></span>

<span data-ttu-id="a94db-250">Když je vytvořen podřízené souboru nebo složce, výchozí ACL hello nadřazeného objektu se zkopíruje hello přístupu ACL hello podřízené souboru nebo složce.</span><span class="sxs-lookup"><span data-stu-id="a94db-250">When a child file or folder is created, hello parent's Default ACL is copied as hello Access ACL of hello child file or folder.</span></span> <span data-ttu-id="a94db-251">Navíc pokud **jiných** uživatel má oprávnění RWX v nadřazené hello výchozí seznam ACL, odebere se ze seznamu ACL pro přístup k hello podřízenou položku.</span><span class="sxs-lookup"><span data-stu-id="a94db-251">Also, if **other** user has RWX permissions in hello parent's default ACL, it is removed from hello child item's Access ACL.</span></span>

![Seznamy ACL služby Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-child-items-1.png)

<span data-ttu-id="a94db-253">Ve většině scénářů hello předchozí informace je, že všechny budete potřebovat tooknow o tom, jak je určen přístupu ACL podřízenou položku.</span><span class="sxs-lookup"><span data-stu-id="a94db-253">In most scenarios, hello previous information is all you need tooknow about how a child item’s Access ACL is determined.</span></span> <span data-ttu-id="a94db-254">Ale pokud jste obeznámeni s POSIX systémy a toounderstand chcete podrobné, jak se dá dosáhnout Tato transformace, najdete v části hello [umask – na roli při vytváření hello přístupu ACL pro nové soubory a složky](#umasks-role-in-creating-the-access-acl-for-new-files-and-folders) dále v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="a94db-254">However, if you are familiar with POSIX systems and want toounderstand in-depth how this transformation is achieved, see hello section [Umask’s role in creating hello Access ACL for new files and folders](#umasks-role-in-creating-the-access-acl-for-new-files-and-folders) later in this article.</span></span>


### <a name="a-child-folders-default-acl"></a><span data-ttu-id="a94db-255">Výchozí seznam ACL podřízené složky</span><span class="sxs-lookup"><span data-stu-id="a94db-255">A child folder's Default ACL</span></span>

<span data-ttu-id="a94db-256">Při vytváření podřízenou složku v rámci nadřazené složky ACL výchozí hello nadřazené složky se zkopíruje přes jako je seznam ACL výchozí toohello podřízenou složku.</span><span class="sxs-lookup"><span data-stu-id="a94db-256">When a child folder is created under a parent folder, hello parent folder's Default ACL is copied over as is toohello child folder's Default ACL.</span></span>

![Seznamy ACL služby Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-child-items-2.png)

## <a name="advanced-topics-for-understanding-acls-in-data-lake-store"></a><span data-ttu-id="a94db-258">Pokročilá témata pro pochopení seznamů ACL v Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="a94db-258">Advanced topics for understanding ACLs in Data Lake Store</span></span>

<span data-ttu-id="a94db-259">Toto jsou některá Pokročilá témata toohelp víte, jak jsou seznamy ACL určit pro Data Lake Store soubory nebo složky.</span><span class="sxs-lookup"><span data-stu-id="a94db-259">Following are some advanced topics toohelp you understand how ACLs are determined for Data Lake Store files or folders.</span></span>

### <a name="umasks-role-in-creating-hello-access-acl-for-new-files-and-folders"></a><span data-ttu-id="a94db-260">Umask – na roli při vytváření hello přístupu ACL pro nové soubory a složky</span><span class="sxs-lookup"><span data-stu-id="a94db-260">Umask’s role in creating hello Access ACL for new files and folders</span></span>

<span data-ttu-id="a94db-261">V rámci standardu POSIX systému obecné koncept hello je, že umask – je hodnotou 9 bitů na hello nadřízené složky, která se používá tootransform hello oprávnění pro **vlastnícího uživatele**, **vlastnící skupinu**, a  **Další** na hello přístupu ACL nového podřízeného souboru nebo složky.</span><span class="sxs-lookup"><span data-stu-id="a94db-261">In a POSIX-compliant system, hello general concept is that umask is a 9-bit value on hello parent folder that's used tootransform hello permission for **owning user**, **owning group**, and **other** on hello Access ACL of a new child file or folder.</span></span> <span data-ttu-id="a94db-262">bity Hello umask – identifikovat které tooturn bits mimo v seznamu ACL pro přístup k hello podřízenou položku.</span><span class="sxs-lookup"><span data-stu-id="a94db-262">hello bits of a umask identify which bits tooturn off in hello child item’s Access ACL.</span></span> <span data-ttu-id="a94db-263">Proto se používá tooselectively zabránit hello šíření oprávnění pro **vlastnícího uživatele**, **vlastnící skupinu**, a **jiných**.</span><span class="sxs-lookup"><span data-stu-id="a94db-263">Thus it is used tooselectively prevent hello propagation of permissions for **owning user**, **owning group**, and **other**.</span></span>

<span data-ttu-id="a94db-264">V systému HDFS umask – hello je obvykle možnosti konfigurace pro celý web, která je řízena správci.</span><span class="sxs-lookup"><span data-stu-id="a94db-264">In an HDFS system, hello umask is typically a sitewide configuration option that is controlled by administrators.</span></span> <span data-ttu-id="a94db-265">Data Lake Store používá funkci **umask v rámci účtu**, kterou nelze změnit.</span><span class="sxs-lookup"><span data-stu-id="a94db-265">Data Lake Store uses an **account-wide umask** that cannot be changed.</span></span> <span data-ttu-id="a94db-266">Následující tabulka ukazuje hello Hello odmaskujte pro Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="a94db-266">hello following table shows hello unmask for Data Lake Store.</span></span>

| <span data-ttu-id="a94db-267">Uživatelská skupina</span><span class="sxs-lookup"><span data-stu-id="a94db-267">User group</span></span>  | <span data-ttu-id="a94db-268">Nastavení</span><span class="sxs-lookup"><span data-stu-id="a94db-268">Setting</span></span> | <span data-ttu-id="a94db-269">Vliv na přístupový seznam ACL nové podřízené položky</span><span class="sxs-lookup"><span data-stu-id="a94db-269">Effect on new child item's Access ACL</span></span> |
|------------ |---------|---------------------------------------|
| <span data-ttu-id="a94db-270">Vlastnící uživatel</span><span class="sxs-lookup"><span data-stu-id="a94db-270">Owning user</span></span> | ---     | <span data-ttu-id="a94db-271">Žádný vliv</span><span class="sxs-lookup"><span data-stu-id="a94db-271">No effect</span></span>                             |
| <span data-ttu-id="a94db-272">Vlastnící skupina</span><span class="sxs-lookup"><span data-stu-id="a94db-272">Owning group</span></span>| ---     | <span data-ttu-id="a94db-273">Žádný vliv</span><span class="sxs-lookup"><span data-stu-id="a94db-273">No effect</span></span>                             |
| <span data-ttu-id="a94db-274">Ostatní</span><span class="sxs-lookup"><span data-stu-id="a94db-274">Other</span></span>       | <span data-ttu-id="a94db-275">RWX</span><span class="sxs-lookup"><span data-stu-id="a94db-275">RWX</span></span>     | <span data-ttu-id="a94db-276">Odebrání oprávnění Číst + Zapisovat + Provést</span><span class="sxs-lookup"><span data-stu-id="a94db-276">Remove Read + Write + Execute</span></span>         |

<span data-ttu-id="a94db-277">Hello následující obrázek znázorňuje tuto umask – v akci.</span><span class="sxs-lookup"><span data-stu-id="a94db-277">hello following illustration shows this umask in action.</span></span> <span data-ttu-id="a94db-278">čistý efekt Hello je tooremove **čtení a zápisu + provést** pro **jiných** uživatele.</span><span class="sxs-lookup"><span data-stu-id="a94db-278">hello net effect is tooremove **Read + Write + Execute** for **other** user.</span></span> <span data-ttu-id="a94db-279">Protože umask – hello nezadali bitů pro **vlastnícího uživatele** a **vlastnící skupinu**, nejsou transformovat tato oprávnění.</span><span class="sxs-lookup"><span data-stu-id="a94db-279">Because hello umask did not specify bits for **owning user** and **owning group**, those permissions are not transformed.</span></span>

![Seznamy ACL služby Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-umask.png)

### <a name="hello-sticky-bit"></a><span data-ttu-id="a94db-281">trvalé bit Hello</span><span class="sxs-lookup"><span data-stu-id="a94db-281">hello sticky bit</span></span>

<span data-ttu-id="a94db-282">trvalé bit Hello je pokročilejší funkce POSIX systému souborů.</span><span class="sxs-lookup"><span data-stu-id="a94db-282">hello sticky bit is a more advanced feature of a POSIX filesystem.</span></span> <span data-ttu-id="a94db-283">V kontextu hello Data Lake Store není pravděpodobné, že hello trvalé bit bude potřeba.</span><span class="sxs-lookup"><span data-stu-id="a94db-283">In hello context of Data Lake Store, it is unlikely that hello sticky bit will be needed.</span></span>

<span data-ttu-id="a94db-284">Hello následující tabulka uvádí fungování trvalé bit hello v Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="a94db-284">hello following table shows how hello sticky bit works in Data Lake Store.</span></span>

| <span data-ttu-id="a94db-285">Uživatelská skupina</span><span class="sxs-lookup"><span data-stu-id="a94db-285">User group</span></span>         | <span data-ttu-id="a94db-286">File</span><span class="sxs-lookup"><span data-stu-id="a94db-286">File</span></span>    | <span data-ttu-id="a94db-287">Složka</span><span class="sxs-lookup"><span data-stu-id="a94db-287">Folder</span></span> |
|--------------------|---------|-------------------------|
| <span data-ttu-id="a94db-288">Bit sticky **VYPNUTÝ**</span><span class="sxs-lookup"><span data-stu-id="a94db-288">Sticky bit **OFF**</span></span> | <span data-ttu-id="a94db-289">Žádný vliv</span><span class="sxs-lookup"><span data-stu-id="a94db-289">No effect</span></span>   | <span data-ttu-id="a94db-290">Žádný vliv</span><span class="sxs-lookup"><span data-stu-id="a94db-290">No effect.</span></span>           |
| <span data-ttu-id="a94db-291">Bit sticky **ZAPNUTÝ**</span><span class="sxs-lookup"><span data-stu-id="a94db-291">Sticky bit **ON**</span></span>  | <span data-ttu-id="a94db-292">Žádný vliv</span><span class="sxs-lookup"><span data-stu-id="a94db-292">No effect</span></span>   | <span data-ttu-id="a94db-293">Zabraňuje nikým kromě **nadtypem uživatelé** a hello **vlastnícího uživatele** podřízené položky v odstranění nebo přejmenování této podřízené položky.</span><span class="sxs-lookup"><span data-stu-id="a94db-293">Prevents anyone except **super-users** and hello **owning user** of a child item from deleting or renaming that child item.</span></span>               |

<span data-ttu-id="a94db-294">trvalé bit Hello se nezobrazí v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="a94db-294">hello sticky bit is not shown in hello Azure portal.</span></span>

## <a name="common-questions-about-acls-in-data-lake-store"></a><span data-ttu-id="a94db-295">Běžné otázky týkající se seznamů ACL ve službě Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="a94db-295">Common questions about ACLs in Data Lake Store</span></span>

<span data-ttu-id="a94db-296">Zde je uvedeno několik otázek, které se často vyskytují v souvislosti se seznamy ACL ve službě Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="a94db-296">Here are some questions that come up often about ACLs in Data Lake Store.</span></span>

### <a name="do-i-have-tooenable-support-for-acls"></a><span data-ttu-id="a94db-297">Jsou tooenable podpora seznamů řízení přístupu?</span><span class="sxs-lookup"><span data-stu-id="a94db-297">Do I have tooenable support for ACLs?</span></span>

<span data-ttu-id="a94db-298">Ne.</span><span class="sxs-lookup"><span data-stu-id="a94db-298">No.</span></span> <span data-ttu-id="a94db-299">Řízení přístupu prostřednictvím seznamů ACL je pro účet Data Lake Store vždy aktivní.</span><span class="sxs-lookup"><span data-stu-id="a94db-299">Access control via ACLs is always on for a Data Lake Store account.</span></span>

### <a name="which-permissions-are-required-toorecursively-delete-a-folder-and-its-contents"></a><span data-ttu-id="a94db-300">Oprávnění, která jsou požadované toorecursively odstranit složku a její obsah?</span><span class="sxs-lookup"><span data-stu-id="a94db-300">Which permissions are required toorecursively delete a folder and its contents?</span></span>

* <span data-ttu-id="a94db-301">musí mít nadřazenou složku Hello **zápisu + provést** oprávnění.</span><span class="sxs-lookup"><span data-stu-id="a94db-301">hello parent folder must have **Write + Execute** permissions.</span></span>
* <span data-ttu-id="a94db-302">Hello toobe složky odstranit, a každé složky v něm, vyžaduje **čtení a zápisu + provést** oprávnění.</span><span class="sxs-lookup"><span data-stu-id="a94db-302">hello folder toobe deleted, and every folder within it, requires **Read + Write + Execute** permissions.</span></span>

> [!NOTE]
> <span data-ttu-id="a94db-303">Není nutné oprávnění k zápisu toodelete soubory ve složkách.</span><span class="sxs-lookup"><span data-stu-id="a94db-303">You do not need Write permissions toodelete files in folders.</span></span> <span data-ttu-id="a94db-304">Navíc hello kořenové složky "/" můžete **nikdy** odstranit.</span><span class="sxs-lookup"><span data-stu-id="a94db-304">Also, hello root folder "/" can **never** be deleted.</span></span>
>
>

### <a name="who-is-hello-owner-of-a-file-or-folder"></a><span data-ttu-id="a94db-305">Kdo je vlastníkem hello souboru nebo složky?</span><span class="sxs-lookup"><span data-stu-id="a94db-305">Who is hello owner of a file or folder?</span></span>

<span data-ttu-id="a94db-306">Hello tvůrce souboru nebo složky se změní na vlastníka hello.</span><span class="sxs-lookup"><span data-stu-id="a94db-306">hello creator of a file or folder becomes hello owner.</span></span>

### <a name="which-group-is-set-as-hello-owning-group-of-a-file-or-folder-at-creation"></a><span data-ttu-id="a94db-307">Které skupiny je nastaven jako hello vlastnící skupinu souboru nebo složky při vytváření?</span><span class="sxs-lookup"><span data-stu-id="a94db-307">Which group is set as hello owning group of a file or folder at creation?</span></span>

<span data-ttu-id="a94db-308">Skupina vlastnícím Hello se zkopíruje z hello vlastnící skupinu hello nadřazené složky pod které hello je vytvořen nový soubor nebo složku.</span><span class="sxs-lookup"><span data-stu-id="a94db-308">hello owning group is copied from hello owning group of hello parent folder under which hello new file or folder is created.</span></span>

### <a name="i-am-hello-owning-user-of-a-file-but-i-dont-have-hello-rwx-permissions-i-need-what-do-i-do"></a><span data-ttu-id="a94db-309">Jsem hello vlastnícího uživatele souboru, ale nemáte oprávnění RWX hello, které je potřeba.</span><span class="sxs-lookup"><span data-stu-id="a94db-309">I am hello owning user of a file but I don’t have hello RWX permissions I need.</span></span> <span data-ttu-id="a94db-310">Co mám udělat?</span><span class="sxs-lookup"><span data-stu-id="a94db-310">What do I do?</span></span>

<span data-ttu-id="a94db-311">Hello vlastnícím uživatel může změnit oprávnění hello hello souboru toogive sami RWX oprávnění, která potřebují.</span><span class="sxs-lookup"><span data-stu-id="a94db-311">hello owning user can change hello permissions of hello file toogive themselves any RWX permissions they need.</span></span>

### <a name="when-i-look-at-acls-in-hello-azure-portal-i-see-user-names-but-through-apis-i-see-guids-why-is-that"></a><span data-ttu-id="a94db-312">Když se podívám na seznamy ACL v hello portál Azure zobrazena uživatelská jména, ale prostřednictvím rozhraní API, zobrazuje identifikátory GUID, proč je, že?</span><span class="sxs-lookup"><span data-stu-id="a94db-312">When I look at ACLs in hello Azure portal I see user names but through APIs, I see GUIDs, why is that?</span></span>

<span data-ttu-id="a94db-313">Položky v hello seznamy ACL jsou uloženy jako identifikátory GUID, které odpovídají toousers ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a94db-313">Entries in hello ACLs are stored as GUIDs that correspond toousers in Azure AD.</span></span> <span data-ttu-id="a94db-314">Hello rozhraní API vrátí hello identifikátory GUID, jako je.</span><span class="sxs-lookup"><span data-stu-id="a94db-314">hello APIs return hello GUIDs as is.</span></span> <span data-ttu-id="a94db-315">Hello portál Azure pokusí jednodušší toouse toomake seznamy řízení přístupu podle překládání hello identifikátory GUID do popisných názvů, pokud je to možné.</span><span class="sxs-lookup"><span data-stu-id="a94db-315">hello Azure portal tries toomake ACLs easier toouse by translating hello GUIDs into friendly names when possible.</span></span>

### <a name="why-do-i-sometimes-see-guids-in-hello-acls-when-im-using-hello-azure-portal"></a><span data-ttu-id="a94db-316">Proč se někdy zobrazuje identifikátory GUID v ACL hello při používání hello portál Azure?</span><span class="sxs-lookup"><span data-stu-id="a94db-316">Why do I sometimes see GUIDs in hello ACLs when I'm using hello Azure portal?</span></span>

<span data-ttu-id="a94db-317">Identifikátor GUID se zobrazí, když uživatel hello neexistuje ve službě Azure AD už.</span><span class="sxs-lookup"><span data-stu-id="a94db-317">A GUID is shown when hello user doesn't exist in Azure AD anymore.</span></span> <span data-ttu-id="a94db-318">Obvykle se to stane, když uživatel hello opustil hello společnosti, nebo pokud byl odstraněn svůj účet ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a94db-318">Usually this happens when hello user has left hello company or if their account has been deleted in Azure AD.</span></span>

### <a name="does-data-lake-store-support-inheritance-of-acls"></a><span data-ttu-id="a94db-319">Podporuje služba Data Lake Store dědění seznamů ACL?</span><span class="sxs-lookup"><span data-stu-id="a94db-319">Does Data Lake Store support inheritance of ACLs?</span></span>

<span data-ttu-id="a94db-320">Ne.</span><span class="sxs-lookup"><span data-stu-id="a94db-320">No.</span></span>

### <a name="what-is-hello-difference-between-mask-and-umask"></a><span data-ttu-id="a94db-321">Jaký je rozdíl hello maska a umask –?</span><span class="sxs-lookup"><span data-stu-id="a94db-321">What is hello difference between mask and umask?</span></span>

| <span data-ttu-id="a94db-322">Vlastnost maska</span><span class="sxs-lookup"><span data-stu-id="a94db-322">mask</span></span> | <span data-ttu-id="a94db-323">Vlastnost umask</span><span class="sxs-lookup"><span data-stu-id="a94db-323">umask</span></span>|
|------|------|
| <span data-ttu-id="a94db-324">Hello **maska** vlastnost je k dispozici na všech souborů a složek.</span><span class="sxs-lookup"><span data-stu-id="a94db-324">hello **mask** property is available on every file and folder.</span></span> | <span data-ttu-id="a94db-325">Hello **umask –** je vlastností hello účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="a94db-325">hello **umask** is a property of hello Data Lake Store account.</span></span> <span data-ttu-id="a94db-326">Proto není pouze jeden umask – hello Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="a94db-326">So there is only a single umask in hello Data Lake Store.</span></span>    |
| <span data-ttu-id="a94db-327">Vlastnost maska Hello k souboru nebo složce může být změněna pomocí hello vlastnícího uživatele nebo vlastnící skupinu pro soubor nebo nadtypem uživatele.</span><span class="sxs-lookup"><span data-stu-id="a94db-327">hello mask property on a file or folder can be altered by hello owning user or owning group of a file or a super-user.</span></span> | <span data-ttu-id="a94db-328">umask – vlastnost Hello nelze změnit žádný uživatel, i superuživatele.</span><span class="sxs-lookup"><span data-stu-id="a94db-328">hello umask property cannot be modified by any user, even a super-user.</span></span> <span data-ttu-id="a94db-329">Tato hodnota je neměnná, konstantní.</span><span class="sxs-lookup"><span data-stu-id="a94db-329">It is an unchangeable, constant value.</span></span>|
| <span data-ttu-id="a94db-330">Vlastnost maska Hello se používá během algoritmus kontrolu přístupu hello v modulu runtime toodetermine, zda uživatel má správné tooperform hello na operace na souboru nebo složce.</span><span class="sxs-lookup"><span data-stu-id="a94db-330">hello mask property is used during hello access check algorithm at runtime toodetermine whether a user has hello right tooperform on operation on a file or folder.</span></span> <span data-ttu-id="a94db-331">role Hello hello masky je toocreate "skutečná oprávnění" v době hello kontrolu přístupu.</span><span class="sxs-lookup"><span data-stu-id="a94db-331">hello role of hello mask is toocreate "effective permissions" at hello time of access check.</span></span> | <span data-ttu-id="a94db-332">umask – Hello se nepoužívá během kontroly přístupu.</span><span class="sxs-lookup"><span data-stu-id="a94db-332">hello umask is not used during access check at all.</span></span> <span data-ttu-id="a94db-333">umask – Hello je použité toodetermine hello přístupu ACL nové podřízené položky složky.</span><span class="sxs-lookup"><span data-stu-id="a94db-333">hello umask is used toodetermine hello Access ACL of new child items of a folder.</span></span> |
| <span data-ttu-id="a94db-334">Hello maska je 3bitová RWX hodnotu, která se vztahuje toonamed uživatele, pojmenovanou skupinu a vlastnícím uživatelů v době hello kontrolu přístupu.</span><span class="sxs-lookup"><span data-stu-id="a94db-334">hello mask is a 3-bit RWX value that applies toonamed user, named group, and owning user at hello time of access check.</span></span>| <span data-ttu-id="a94db-335">Hello umask – je 9 bitů hodnotu, která se vztahuje toohello vlastnící uživatel, který je vlastníkem skupiny, a **jiných** nového podřízeného.</span><span class="sxs-lookup"><span data-stu-id="a94db-335">hello umask is a 9-bit value that applies toohello owning user, owning group, and **other** of a new child.</span></span>|

### <a name="where-can-i-learn-more-about-posix-access-control-model"></a><span data-ttu-id="a94db-336">Kde najdu další informace o modelu řízení přístupu POSIX?</span><span class="sxs-lookup"><span data-stu-id="a94db-336">Where can I learn more about POSIX access control model?</span></span>

* [<span data-ttu-id="a94db-337">POSIX Access Control Lists on Linux (Seznamy řízení přístupu v rámci specifikace POSIX v Linuxu)</span><span class="sxs-lookup"><span data-stu-id="a94db-337">POSIX Access Control Lists on Linux</span></span>](http://www.vanemery.com/Linux/ACL/POSIX_ACL_on_Linux.html)

* [<span data-ttu-id="a94db-338">HDFS Permissions Guide (Průvodce oprávněními v HDFS)</span><span class="sxs-lookup"><span data-stu-id="a94db-338">HDFS permission guide</span></span>](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html)

* [<span data-ttu-id="a94db-339">Nejčastější dotazy týkající se specifikace POSIX</span><span class="sxs-lookup"><span data-stu-id="a94db-339">POSIX FAQ</span></span>](http://www.opengroup.org/austin/papers/posix_faq.html)

* [<span data-ttu-id="a94db-340">POSIX 1003.1 2008</span><span class="sxs-lookup"><span data-stu-id="a94db-340">POSIX 1003.1 2008</span></span>](http://standards.ieee.org/findstds/standard/1003.1-2008.html)

* [<span data-ttu-id="a94db-341">POSIX 1003.1 2013</span><span class="sxs-lookup"><span data-stu-id="a94db-341">POSIX 1003.1 2013</span></span>](http://pubs.opengroup.org/onlinepubs/9699919799.2013edition/)

* [<span data-ttu-id="a94db-342">POSIX 1003.1 2016</span><span class="sxs-lookup"><span data-stu-id="a94db-342">POSIX 1003.1 2016</span></span>](http://pubs.opengroup.org/onlinepubs/9699919799.2016edition/)

* [<span data-ttu-id="a94db-343">POSIX ACL na Ubuntu</span><span class="sxs-lookup"><span data-stu-id="a94db-343">POSIX ACL on Ubuntu</span></span>](https://help.ubuntu.com/community/FilePermissionsACLs)

* [<span data-ttu-id="a94db-344">ACL: Using Access Control Lists on Linux (Seznamy ACL: Používání seznamů řízení přístupu v Linuxu)</span><span class="sxs-lookup"><span data-stu-id="a94db-344">ACL using access control lists on Linux</span></span>](http://bencane.com/2012/05/27/acl-using-access-control-lists-on-linux/)

## <a name="see-also"></a><span data-ttu-id="a94db-345">Viz také</span><span class="sxs-lookup"><span data-stu-id="a94db-345">See also</span></span>

* [<span data-ttu-id="a94db-346">Přehled Azure Data Lake Storu</span><span class="sxs-lookup"><span data-stu-id="a94db-346">Overview of Azure Data Lake Store</span></span>](data-lake-store-overview.md)
