---
title: "aaaConfigure Hive zásad v doméně HDInsight - Azure | Microsoft Docs"
description: "Zjistěte..."
services: hdinsight
documentationcenter: 
author: saurinsh
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 3fade1e5-c2e1-4ad5-b371-f95caea23f6d
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/25/2016
ms.author: saurinsh
ms.openlocfilehash: 56f2bf9d872abc5f772b886fcf91c2e2422092f4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-hive-policies-in-domain-joined-hdinsight-preview"></a><span data-ttu-id="81364-103">Konfigurace zásad Hivu ve službě HDInsight připojené k doméně (Preview)</span><span class="sxs-lookup"><span data-stu-id="81364-103">Configure Hive policies in Domain-joined HDInsight (Preview)</span></span>
<span data-ttu-id="81364-104">Zjistěte, jak zásady Apache škálu tooconfigure pro Hive.</span><span class="sxs-lookup"><span data-stu-id="81364-104">Learn how tooconfigure Apache Ranger policies for Hive.</span></span> <span data-ttu-id="81364-105">V tomto článku vytvoříte dva škálu zásady toorestrict přístup toohello hivesampletable.</span><span class="sxs-lookup"><span data-stu-id="81364-105">In this article, you create two Ranger policies toorestrict access toohello hivesampletable.</span></span> <span data-ttu-id="81364-106">Hello hivesampletable se dodává s clustery HDInsight.</span><span class="sxs-lookup"><span data-stu-id="81364-106">hello hivesampletable comes with HDInsight clusters.</span></span> <span data-ttu-id="81364-107">Po nakonfigurování zásady hello použijete aplikace Excel a rozhraní ODBC ovladač tooconnect tooHive tabulky v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="81364-107">After you have configured hello policies, you use Excel and ODBC driver tooconnect tooHive tables in HDInsight.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="81364-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="81364-108">Prerequisites</span></span>
* <span data-ttu-id="81364-109">Cluster HDInsight připojený k doméně.</span><span class="sxs-lookup"><span data-stu-id="81364-109">A Domain-joined HDInsight cluster.</span></span> <span data-ttu-id="81364-110">Viz [Konfigurace clusterů HDInsight připojených k doméně](hdinsight-domain-joined-configure.md).</span><span class="sxs-lookup"><span data-stu-id="81364-110">See [Configure Domain-joined HDInsight clusters](hdinsight-domain-joined-configure.md).</span></span>
* <span data-ttu-id="81364-111">Pracovní stanice s Office 2016, Office 2013 Professional Plus, Office 365 Pro Plus, Excel 2013 Standalone nebo Office 2010 Professional Plus.</span><span class="sxs-lookup"><span data-stu-id="81364-111">A workstation with Office 2016, Office 2013 Professional Plus, Office 365 Pro Plus, Excel 2013 Standalone, or Office 2010 Professional Plus.</span></span>

## <a name="connect-tooapache-ranger-admin-ui"></a><span data-ttu-id="81364-112">Připojit tooApache škálu správce uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="81364-112">Connect tooApache Ranger Admin UI</span></span>
<span data-ttu-id="81364-113">**tooconnect tooRanger uživatelského rozhraní správce**</span><span class="sxs-lookup"><span data-stu-id="81364-113">**tooconnect tooRanger Admin UI**</span></span>

1. <span data-ttu-id="81364-114">V prohlížeči připojte tooRanger uživatelského rozhraní správce.</span><span class="sxs-lookup"><span data-stu-id="81364-114">From a browser, connect tooRanger Admin UI.</span></span> <span data-ttu-id="81364-115">Adresa URL Hello je https://&lt;ClusterName >.azurehdinsight.net/Ranger/.</span><span class="sxs-lookup"><span data-stu-id="81364-115">hello URL is https://&lt;ClusterName>.azurehdinsight.net/Ranger/.</span></span>

   > [!NOTE]
   > <span data-ttu-id="81364-116">Ranger používá jiné přihlašovací údaje než cluster Hadoop.</span><span class="sxs-lookup"><span data-stu-id="81364-116">Ranger uses different credentials than Hadoop cluster.</span></span> <span data-ttu-id="81364-117">tooprevent prohlížeče pomocí pověření uložených v mezipaměti Hadoop, použijte nové prohlížeče inprivate okno tooconnect toohello škálu správce uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="81364-117">tooprevent browsers using cached Hadoop credentials, use new inprivate browser window tooconnect toohello Ranger Admin UI.</span></span>
   >
   >
2. <span data-ttu-id="81364-118">Přihlaste se pomocí hello clusteru správce domény uživatelské jméno a heslo:</span><span class="sxs-lookup"><span data-stu-id="81364-118">Log in using hello cluster administrator domain user name and password:</span></span>

    ![Domovská stránka Ranger služby HDInsight připojené k doméně](./media/hdinsight-domain-joined-run-hive/hdinsight-domain-joined-ranger-home-page.png)

    <span data-ttu-id="81364-120">V současné době Ranger funguje pouze s Yarn a Hivem.</span><span class="sxs-lookup"><span data-stu-id="81364-120">Currently, Ranger only works with Yarn and Hive.</span></span>

## <a name="create-domain-users"></a><span data-ttu-id="81364-121">Vytvoření uživatelů domén</span><span class="sxs-lookup"><span data-stu-id="81364-121">Create Domain users</span></span>
<span data-ttu-id="81364-122">V tématu [Konfigurace clusterů HDInsight připojených k doméně](hdinsight-domain-joined-configure.md#create-and-configure-azure-ad-ds-for-your-azure-ad) jste vytvořili uživatele hiveuser1 a hiveuser2.</span><span class="sxs-lookup"><span data-stu-id="81364-122">In [Configure Domain-joined HDInsight clusters](hdinsight-domain-joined-configure.md#create-and-configure-azure-ad-ds-for-your-azure-ad), you have created hiveruser1 and hiveuser2.</span></span> <span data-ttu-id="81364-123">V tomto kurzu použijete hello dvě uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="81364-123">You will use hello two user account in this tutorial.</span></span>

## <a name="create-ranger-policies"></a><span data-ttu-id="81364-124">Vytvoření zásad Ranger</span><span class="sxs-lookup"><span data-stu-id="81364-124">Create Ranger policies</span></span>
<span data-ttu-id="81364-125">V této části vytvoříte dvě zásady Ranger pro přistupování k hivesampletable.</span><span class="sxs-lookup"><span data-stu-id="81364-125">In this section, you will create two Ranger policies for accessing hivesampletable.</span></span> <span data-ttu-id="81364-126">Udělíte oprávnění Vybrat na různé sady sloupců.</span><span class="sxs-lookup"><span data-stu-id="81364-126">You give select permission on different set of columns.</span></span> <span data-ttu-id="81364-127">Oba uživatelé byli vytvořeni v tématu [Konfigurace clusterů HDInsight připojených k doméně](hdinsight-domain-joined-configure.md#create-and-configure-azure-ad-ds-for-your-azure-ad).</span><span class="sxs-lookup"><span data-stu-id="81364-127">Both users were created in [Configure Domain-joined HDInsight clusters](hdinsight-domain-joined-configure.md#create-and-configure-azure-ad-ds-for-your-azure-ad).</span></span>  <span data-ttu-id="81364-128">V další části hello budete testovat hello dvě zásady v aplikaci Excel.</span><span class="sxs-lookup"><span data-stu-id="81364-128">In hello next section, you will test hello two policies in Excel.</span></span>

<span data-ttu-id="81364-129">**toocreate škálu zásady**</span><span class="sxs-lookup"><span data-stu-id="81364-129">**toocreate Ranger policies**</span></span>

1. <span data-ttu-id="81364-130">Otevřete uživatelské rozhraní správce Ranger.</span><span class="sxs-lookup"><span data-stu-id="81364-130">Open Ranger Admin UI.</span></span> <span data-ttu-id="81364-131">V tématu [připojit tooApache uživatelského rozhraní správce škálu](#connect-to-apache-ranager-admin-ui).</span><span class="sxs-lookup"><span data-stu-id="81364-131">See [Connect tooApache Ranger Admin UI](#connect-to-apache-ranager-admin-ui).</span></span>
2. <span data-ttu-id="81364-132">V části **Hive** klikněte na **&lt;název_clusteru>_hive**.</span><span class="sxs-lookup"><span data-stu-id="81364-132">Click **&lt;ClusterName>_hive**, under **Hive**.</span></span> <span data-ttu-id="81364-133">Měly by se zobrazit dvě předem nakonfigurované zásady.</span><span class="sxs-lookup"><span data-stu-id="81364-133">You shall see two pre-configure policies.</span></span>
3. <span data-ttu-id="81364-134">Klikněte na tlačítko **přidat nové zásady**a pak zadejte hello následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="81364-134">Click **Add New Policy**, and then enter hello following values:</span></span>

   * <span data-ttu-id="81364-135">Policy name (Název zásady): read-hivesampletable-all</span><span class="sxs-lookup"><span data-stu-id="81364-135">Policy name: read-hivesampletable-all</span></span>
   * <span data-ttu-id="81364-136">Hive Database (Databáze Hivu): default (výchozí)</span><span class="sxs-lookup"><span data-stu-id="81364-136">Hive Database: default</span></span>
   * <span data-ttu-id="81364-137">table (tabulka): hivesampletable</span><span class="sxs-lookup"><span data-stu-id="81364-137">table: hivesampletable</span></span>
   * <span data-ttu-id="81364-138">Hive column (Sloupec Hivu):*</span><span class="sxs-lookup"><span data-stu-id="81364-138">Hive column: *</span></span>
   * <span data-ttu-id="81364-139">Select User (Vybrat uživatele): hiveuser1</span><span class="sxs-lookup"><span data-stu-id="81364-139">Select User: hiveuser1</span></span>
   * <span data-ttu-id="81364-140">Permissions (Oprávnění): select (vybrat)</span><span class="sxs-lookup"><span data-stu-id="81364-140">Permissions: select</span></span>

     ![Konfigurace zásady Hivu v Ranger služby HDInsight připojené k doméně](./media/hdinsight-domain-joined-run-hive/hdinsight-domain-joined-configure-ranger-policy.png)<span data-ttu-id="81364-142">.</span><span class="sxs-lookup"><span data-stu-id="81364-142">.</span></span>

     > [!NOTE]
     > <span data-ttu-id="81364-143">Pokud uživatel domény není vyplněný v vyberte uživatele, počkejte několik minut pro škálu toosync v AAD.</span><span class="sxs-lookup"><span data-stu-id="81364-143">If a domain user is not populated in Select User, wait a few moments for Ranger toosync with AAD.</span></span>
     >
     >
4. <span data-ttu-id="81364-144">Klikněte na tlačítko **přidat** toosave hello zásad.</span><span class="sxs-lookup"><span data-stu-id="81364-144">Click **Add** toosave hello policy.</span></span>
5. <span data-ttu-id="81364-145">Zopakujte poslední dva kroky toocreate hello jiné zásady s hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="81364-145">Repeat hello last two steps toocreate another policy with hello following properties:</span></span>

   * <span data-ttu-id="81364-146">Policy name (Název zásady): read-hivesampletable-devicemake</span><span class="sxs-lookup"><span data-stu-id="81364-146">Policy name: read-hivesampletable-devicemake</span></span>
   * <span data-ttu-id="81364-147">Hive Database (Databáze Hivu): default (výchozí)</span><span class="sxs-lookup"><span data-stu-id="81364-147">Hive Database: default</span></span>
   * <span data-ttu-id="81364-148">table (tabulka): hivesampletable</span><span class="sxs-lookup"><span data-stu-id="81364-148">table: hivesampletable</span></span>
   * <span data-ttu-id="81364-149">Hive column (Sloupec Hivu): clientid, devicemake</span><span class="sxs-lookup"><span data-stu-id="81364-149">Hive column: clientid, devicemake</span></span>
   * <span data-ttu-id="81364-150">Select User (Vybrat uživatele): hiveuser2</span><span class="sxs-lookup"><span data-stu-id="81364-150">Select User: hiveuser2</span></span>
   * <span data-ttu-id="81364-151">Permissions (Oprávnění): select (vybrat)</span><span class="sxs-lookup"><span data-stu-id="81364-151">Permissions: select</span></span>

## <a name="create-hive-odbc-data-source"></a><span data-ttu-id="81364-152">Vytvoření zdroje dat Hive ODBC</span><span class="sxs-lookup"><span data-stu-id="81364-152">Create Hive ODBC data source</span></span>
<span data-ttu-id="81364-153">Hello pokyny naleznete v [zdroje dat ODBC Hive vytvořit](hdinsight-connect-excel-hive-odbc-driver.md).</span><span class="sxs-lookup"><span data-stu-id="81364-153">hello instructions can be found in [Create Hive ODBC data source](hdinsight-connect-excel-hive-odbc-driver.md).</span></span>  

    <span data-ttu-id="81364-154">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="81364-154">Property</span></span>|<span data-ttu-id="81364-155">Popis</span><span class="sxs-lookup"><span data-stu-id="81364-155">Description</span></span>
    ---|---
    <span data-ttu-id="81364-156">Název zdroje dat</span><span class="sxs-lookup"><span data-stu-id="81364-156">Data Source Name</span></span>|<span data-ttu-id="81364-157">Zadejte název zdroje dat tooyour</span><span class="sxs-lookup"><span data-stu-id="81364-157">Give a name tooyour data source</span></span>
    <span data-ttu-id="81364-158">Hostitel</span><span class="sxs-lookup"><span data-stu-id="81364-158">Host</span></span>|<span data-ttu-id="81364-159">Zadejte &lt;název_clusteru_HDInsight>.azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="81364-159">Enter &lt;HDInsightClusterName>.azurehdinsight.net.</span></span> <span data-ttu-id="81364-160">Například mujHDICluster.azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="81364-160">For example, myHDICluster.azurehdinsight.net</span></span>
    <span data-ttu-id="81364-161">Port</span><span class="sxs-lookup"><span data-stu-id="81364-161">Port</span></span>|<span data-ttu-id="81364-162">Použijte <strong>443</strong>.</span><span class="sxs-lookup"><span data-stu-id="81364-162">Use <strong>443</strong>.</span></span> <span data-ttu-id="81364-163">(Tento port se změnil z 563 too443.)</span><span class="sxs-lookup"><span data-stu-id="81364-163">(This port has been changed from 563 too443.)</span></span>
    <span data-ttu-id="81364-164">Databáze</span><span class="sxs-lookup"><span data-stu-id="81364-164">Database</span></span>|<span data-ttu-id="81364-165">Použijte <strong>Výchozí</strong>.</span><span class="sxs-lookup"><span data-stu-id="81364-165">Use <strong>Default</strong>.</span></span>
    <span data-ttu-id="81364-166">Typ serveru Hive</span><span class="sxs-lookup"><span data-stu-id="81364-166">Hive Server Type</span></span>|<span data-ttu-id="81364-167">Vyberte <strong>Hive Server 2</strong>.</span><span class="sxs-lookup"><span data-stu-id="81364-167">Select <strong>Hive Server 2</strong></span></span>
    <span data-ttu-id="81364-168">Mechanismus</span><span class="sxs-lookup"><span data-stu-id="81364-168">Mechanism</span></span>|<span data-ttu-id="81364-169">Vyberte <strong>Služba Azure HDInsight</strong></span><span class="sxs-lookup"><span data-stu-id="81364-169">Select <strong>Azure HDInsight Service</strong></span></span>
    <span data-ttu-id="81364-170">Cesta HTTP</span><span class="sxs-lookup"><span data-stu-id="81364-170">HTTP Path</span></span>|<span data-ttu-id="81364-171">Ponechte prázdné.</span><span class="sxs-lookup"><span data-stu-id="81364-171">Leave it blank.</span></span>
    <span data-ttu-id="81364-172">Uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="81364-172">User Name</span></span>|<span data-ttu-id="81364-173">Zadejte hiveuser1@contoso158.onmicrosoft.com. Aktualizujte název domény hello, pokud se liší.</span><span class="sxs-lookup"><span data-stu-id="81364-173">Enter hiveuser1@contoso158.onmicrosoft.com. Update hello domain name if it is different.</span></span>
    <span data-ttu-id="81364-174">Heslo</span><span class="sxs-lookup"><span data-stu-id="81364-174">Password</span></span>|<span data-ttu-id="81364-175">Zadejte heslo hello hiveuser1.</span><span class="sxs-lookup"><span data-stu-id="81364-175">Enter hello password for hiveuser1.</span></span>
    </table>

<span data-ttu-id="81364-176">Ujistěte se, že tooclick **Test** před uložením zdroj dat hello.</span><span class="sxs-lookup"><span data-stu-id="81364-176">Make sure tooclick **Test** before saving hello data source.</span></span>

## <a name="import-data-into-excel-from-hdinsight"></a><span data-ttu-id="81364-177">Import dat do Excelu ze služby HDInsight</span><span class="sxs-lookup"><span data-stu-id="81364-177">Import data into Excel from HDInsight</span></span>
<span data-ttu-id="81364-178">V poslední části hello jste nakonfigurovali dvě zásady.</span><span class="sxs-lookup"><span data-stu-id="81364-178">In hello last section, you have configured two policies.</span></span>  <span data-ttu-id="81364-179">hiveuser1 má hello vyberte oprávnění pro všechny sloupce hello a hiveuser2 má hello vyberte oprávnění na dva sloupce.</span><span class="sxs-lookup"><span data-stu-id="81364-179">hiveuser1 has hello select permission on all hello columns, and hiveuser2 has hello select permission on two columns.</span></span> <span data-ttu-id="81364-180">V této části zosobnit hello dva uživatelé tooimport dat do aplikace Excel.</span><span class="sxs-lookup"><span data-stu-id="81364-180">In this section, you impersonate hello two users tooimport data into Excel.</span></span>

1. <span data-ttu-id="81364-181">V Excelu otevřete nový nebo existující sešit.</span><span class="sxs-lookup"><span data-stu-id="81364-181">Open a new or existing workbook in Excel.</span></span>
2. <span data-ttu-id="81364-182">Z hello **Data** , klikněte na **z jiných zdrojů dat**a potom klikněte na **z Průvodce datovým připojením** toolaunch hello **Průvodce datovým připojením**.</span><span class="sxs-lookup"><span data-stu-id="81364-182">From hello **Data** tab, click **From Other Data Sources**, and then click **From Data Connection Wizard** toolaunch hello **Data Connection Wizard**.</span></span>

    <span data-ttu-id="81364-183">![Otevřete Průvodce připojením dat][img-hdi-simbahiveodbc.excel.dataconnection]</span><span class="sxs-lookup"><span data-stu-id="81364-183">![Open data connection wizard][img-hdi-simbahiveodbc.excel.dataconnection]</span></span>
3. <span data-ttu-id="81364-184">Vyberte **název DSN rozhraní ODBC** jako zdroj dat hello a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="81364-184">Select **ODBC DSN** as hello data source, and then click **Next**.</span></span>
4. <span data-ttu-id="81364-185">Ze zdroje dat ODBC, název, který jste vytvořili v předchozím kroku hello zdroje dat vyberte hello a pak klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="81364-185">From ODBC data sources, select hello data source name that you created in hello previous step, and then  click **Next**.</span></span>
5. <span data-ttu-id="81364-186">Znovu zadejte heslo hello hello clusteru v Průvodci hello a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="81364-186">Re-enter hello password for hello cluster in hello wizard, and then click **OK**.</span></span> <span data-ttu-id="81364-187">Počkejte hello **vybrat databázi a tabulku** tooopen dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="81364-187">Wait for hello **Select Database and Table** dialog tooopen.</span></span> <span data-ttu-id="81364-188">Může to trvat několik sekund.</span><span class="sxs-lookup"><span data-stu-id="81364-188">This can take a few seconds.</span></span>
6. <span data-ttu-id="81364-189">Vyberte **hivesampletable** a pak klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="81364-189">Select **hivesampletable**, and then click **Next**.</span></span>
7. <span data-ttu-id="81364-190">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="81364-190">Click **Finish**.</span></span>
8. <span data-ttu-id="81364-191">V hello **importovat Data** dialogové okno, můžete změnit nebo zadejte dotaz hello.</span><span class="sxs-lookup"><span data-stu-id="81364-191">In hello **Import Data** dialog, you can change or specify hello query.</span></span> <span data-ttu-id="81364-192">Ano, klikněte na tlačítko toodo **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="81364-192">toodo so, click **Properties**.</span></span> <span data-ttu-id="81364-193">Může to trvat několik sekund.</span><span class="sxs-lookup"><span data-stu-id="81364-193">This can take a few seconds.</span></span>
9. <span data-ttu-id="81364-194">Klikněte na tlačítko hello **definice** text příkazu kartě hello je:</span><span class="sxs-lookup"><span data-stu-id="81364-194">Click hello **Definition** tab. hello command text is:</span></span>

       SELECT * FROM "HIVE"."default"."hivesampletable"

   <span data-ttu-id="81364-195">Hiveuser1 zásadami hello škálu definovaných, má oprávnění select pro všechny sloupce hello.</span><span class="sxs-lookup"><span data-stu-id="81364-195">By hello Ranger policies you defined,  hiveuser1 has select permission on all hello columns.</span></span>  <span data-ttu-id="81364-196">Takže tento dotaz funguje s přihlašovacími údaji uživatele hiveuser1, ale nefunguje s přihlašovacími údaji uživatele hiveuser2.</span><span class="sxs-lookup"><span data-stu-id="81364-196">So this query works with hiveuser1's credentials, but this query does not not work with hiveuser2's credentials.</span></span>

   <span data-ttu-id="81364-197">![Vlastnosti připojení][img-hdi-simbahiveodbc-excel-connectionproperties]</span><span class="sxs-lookup"><span data-stu-id="81364-197">![Connection Properties][img-hdi-simbahiveodbc-excel-connectionproperties]</span></span>
10. <span data-ttu-id="81364-198">Klikněte na tlačítko **OK** dialogové okno Vlastnosti připojení tooclose hello.</span><span class="sxs-lookup"><span data-stu-id="81364-198">Click **OK** tooclose hello Connection Properties dialog.</span></span>
11. <span data-ttu-id="81364-199">Klikněte na tlačítko **OK** tooclose hello **importovat Data** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="81364-199">Click **OK** tooclose hello **Import Data** dialog.</span></span>  
12. <span data-ttu-id="81364-200">Zadejte znovu heslo hello hiveuser1 a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="81364-200">Reenter hello password for hiveuser1, and then click **OK**.</span></span> <span data-ttu-id="81364-201">Jak dlouho trvá několik sekund, než získá importované tooExcel data.</span><span class="sxs-lookup"><span data-stu-id="81364-201">It takes a few seconds before data gets imported tooExcel.</span></span> <span data-ttu-id="81364-202">Po dokončení importu byste měli vidět 11 sloupců dat.</span><span class="sxs-lookup"><span data-stu-id="81364-202">When it is done, you shall see 11 columns of data.</span></span>

<span data-ttu-id="81364-203">Druhá zásada tootest hello (čtení devicemake hivesampletable), který jste vytvořili v poslední části hello</span><span class="sxs-lookup"><span data-stu-id="81364-203">tootest hello second policy (read-hivesampletable-devicemake) you created in hello last section</span></span>

1. <span data-ttu-id="81364-204">Přidejte v Excelu nový list.</span><span class="sxs-lookup"><span data-stu-id="81364-204">Add a new sheet in Excel.</span></span>
2. <span data-ttu-id="81364-205">Postupujte podle hello poslední postup tooimport hello data.</span><span class="sxs-lookup"><span data-stu-id="81364-205">Follow hello last procedure tooimport hello data.</span></span>  <span data-ttu-id="81364-206">Hello pouze změny, které provedete je toouse hiveuser2 pověření místo hiveuser1 společnosti.</span><span class="sxs-lookup"><span data-stu-id="81364-206">hello only change you will make is toouse hiveuser2's credentials instead of hiveuser1's.</span></span> <span data-ttu-id="81364-207">To se nezdaří, protože hiveuser2 má pouze oprávnění toosee dva sloupce.</span><span class="sxs-lookup"><span data-stu-id="81364-207">This will fail because hiveuser2 only has permission toosee two columns.</span></span> <span data-ttu-id="81364-208">Získáte hello následující chybě:</span><span class="sxs-lookup"><span data-stu-id="81364-208">You shall get hello following error:</span></span>

        [Microsoft][HiveODBC] (35) Error from Hive: error code: '40000' error message: 'Error while compiling statement: FAILED: HiveAccessControlException Permission denied: user [hiveuser2] does not have [SELECT] privilege on [default/hivesampletable/clientid,country ...]'.
3. <span data-ttu-id="81364-209">Postupujte podle hello stejný postup tooimport data.</span><span class="sxs-lookup"><span data-stu-id="81364-209">Follow hello same procedure tooimport data.</span></span> <span data-ttu-id="81364-210">Tentokrát použijte přihlašovací údaje pro hiveuser2 a také upravit příkaz select hello z:</span><span class="sxs-lookup"><span data-stu-id="81364-210">This time, use hiveuser2's credentials, and also modify hello select statement from:</span></span>

        SELECT * FROM "HIVE"."default"."hivesampletable"

    <span data-ttu-id="81364-211">na:</span><span class="sxs-lookup"><span data-stu-id="81364-211">to:</span></span>

        SELECT clientid, devicemake FROM "HIVE"."default"."hivesampletable"

    <span data-ttu-id="81364-212">Po dokončení importu byste měli vidět naimportované dva sloupce dat.</span><span class="sxs-lookup"><span data-stu-id="81364-212">When it is done, you shall see two columns of data imported.</span></span>

## <a name="next-steps"></a><span data-ttu-id="81364-213">Další kroky</span><span class="sxs-lookup"><span data-stu-id="81364-213">Next steps</span></span>
* <span data-ttu-id="81364-214">Pokud chcete konfigurovat cluster HDInsight připojený k doméně, přečtěte si téma [Konfigurace clusterů HDInsight připojených k doméně](hdinsight-domain-joined-configure.md).</span><span class="sxs-lookup"><span data-stu-id="81364-214">For configuring a Domain-joined HDInsight cluster, see [Configure Domain-joined HDInsight clusters](hdinsight-domain-joined-configure.md).</span></span>
* <span data-ttu-id="81364-215">Pokud chcete spravovat clustery HDInsight připojené k doméně, přečtěte si téma [Správa clusterů HDInsight připojených k doméně](hdinsight-domain-joined-manage.md).</span><span class="sxs-lookup"><span data-stu-id="81364-215">For managing a Domain-joined HDInsight clusters, see [Manage Domain-joined HDInsight clusters](hdinsight-domain-joined-manage.md).</span></span>
* <span data-ttu-id="81364-216">Spuštění dotazů Hive pomocí protokolu SSH v clusterech HDInsight připojený k doméně, najdete v části [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).</span><span class="sxs-lookup"><span data-stu-id="81364-216">For running Hive queries using SSH on Domain-joined HDInsight clusters, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).</span></span>
* <span data-ttu-id="81364-217">Připojení Hive pomocí Hive JDBC, najdete v části [připojit tooHive v Azure HDInsight pomocí ovladače Hive JDBC hello](hdinsight-connect-hive-jdbc-driver.md)</span><span class="sxs-lookup"><span data-stu-id="81364-217">For Connecting Hive using Hive JDBC, see [Connect tooHive on Azure HDInsight using hello Hive JDBC driver](hdinsight-connect-hive-jdbc-driver.md)</span></span>
* <span data-ttu-id="81364-218">Připojování tooHadoop aplikace Excel pomocí Hive ODBC, najdete v části [tooHadoop připojení aplikace Excel s hello jednotky Microsoft Hive ODBC](hdinsight-connect-excel-hive-odbc-driver.md)</span><span class="sxs-lookup"><span data-stu-id="81364-218">For connecting Excel tooHadoop using Hive ODBC, see [Connect Excel tooHadoop with hello Microsoft Hive ODBC drive](hdinsight-connect-excel-hive-odbc-driver.md)</span></span>
* <span data-ttu-id="81364-219">Připojování tooHadoop aplikace Excel pomocí doplňku Power Query, najdete v části [tooHadoop připojení aplikace Excel pomocí doplňku Power Query](hdinsight-connect-excel-power-query.md)</span><span class="sxs-lookup"><span data-stu-id="81364-219">For connecting Excel tooHadoop using Power Query, see [Connect Excel tooHadoop by using Power Query](hdinsight-connect-excel-power-query.md)</span></span>
