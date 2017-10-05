---
title: "Nakonfigurovat zásady Hive v HDInsight připojený k doméně - Azure | Microsoft Docs"
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
ms.openlocfilehash: de537d5e39dd0d3f75ff802948c7372e4d65d127
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configure-hive-policies-in-domain-joined-hdinsight-preview"></a><span data-ttu-id="22ff4-103">Konfigurace zásad Hivu ve službě HDInsight připojené k doméně (Preview)</span><span class="sxs-lookup"><span data-stu-id="22ff4-103">Configure Hive policies in Domain-joined HDInsight (Preview)</span></span>
<span data-ttu-id="22ff4-104">Zjistěte, jak nakonfigurovat zásady Apache Rangeru pro Hive.</span><span class="sxs-lookup"><span data-stu-id="22ff4-104">Learn how to configure Apache Ranger policies for Hive.</span></span> <span data-ttu-id="22ff4-105">V tomto článku vytvoříte dvě zásady Ranger pro omezení přístupu k hivesampletable.</span><span class="sxs-lookup"><span data-stu-id="22ff4-105">In this article, you create two Ranger policies to restrict access to the hivesampletable.</span></span> <span data-ttu-id="22ff4-106">Hivesampletable je součástí clusterů HDInsight.</span><span class="sxs-lookup"><span data-stu-id="22ff4-106">The hivesampletable comes with HDInsight clusters.</span></span> <span data-ttu-id="22ff4-107">Po nakonfigurování zásad použijete Excel nebo ovladač ODBC a připojíte se k tabulkám Hivu ve službě HDInsight.</span><span class="sxs-lookup"><span data-stu-id="22ff4-107">After you have configured the policies, you use Excel and ODBC driver to connect to Hive tables in HDInsight.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="22ff4-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="22ff4-108">Prerequisites</span></span>
* <span data-ttu-id="22ff4-109">Cluster HDInsight připojený k doméně.</span><span class="sxs-lookup"><span data-stu-id="22ff4-109">A Domain-joined HDInsight cluster.</span></span> <span data-ttu-id="22ff4-110">Viz [Konfigurace clusterů HDInsight připojených k doméně](hdinsight-domain-joined-configure.md).</span><span class="sxs-lookup"><span data-stu-id="22ff4-110">See [Configure Domain-joined HDInsight clusters](hdinsight-domain-joined-configure.md).</span></span>
* <span data-ttu-id="22ff4-111">Pracovní stanice s Office 2016, Office 2013 Professional Plus, Office 365 Pro Plus, Excel 2013 Standalone nebo Office 2010 Professional Plus.</span><span class="sxs-lookup"><span data-stu-id="22ff4-111">A workstation with Office 2016, Office 2013 Professional Plus, Office 365 Pro Plus, Excel 2013 Standalone, or Office 2010 Professional Plus.</span></span>

## <a name="connect-to-apache-ranger-admin-ui"></a><span data-ttu-id="22ff4-112">Připojení k uživatelskému rozhraní správce Apache Ranger</span><span class="sxs-lookup"><span data-stu-id="22ff4-112">Connect to Apache Ranger Admin UI</span></span>
<span data-ttu-id="22ff4-113">**Připojení k uživatelskému rozhraní správce Ranger**</span><span class="sxs-lookup"><span data-stu-id="22ff4-113">**To connect to Ranger Admin UI**</span></span>

1. <span data-ttu-id="22ff4-114">V prohlížeči se připojte k uživatelskému rozhraní správce Ranger.</span><span class="sxs-lookup"><span data-stu-id="22ff4-114">From a browser, connect to Ranger Admin UI.</span></span> <span data-ttu-id="22ff4-115">Adresa URL je: https://&lt;název_clusteru>.azurehdinsight.net/Ranger/.</span><span class="sxs-lookup"><span data-stu-id="22ff4-115">The URL is https://&lt;ClusterName>.azurehdinsight.net/Ranger/.</span></span>

   > [!NOTE]
   > <span data-ttu-id="22ff4-116">Ranger používá jiné přihlašovací údaje než cluster Hadoop.</span><span class="sxs-lookup"><span data-stu-id="22ff4-116">Ranger uses different credentials than Hadoop cluster.</span></span> <span data-ttu-id="22ff4-117">Abyste zabránili prohlížeči v použití přihlašovacích údajů systému Hadoop uložených v mezipaměti, použijte pro připojení k uživatelskému rozhraní správce Ranger nové okno prohlížeče v režimu InPrivate.</span><span class="sxs-lookup"><span data-stu-id="22ff4-117">To prevent browsers using cached Hadoop credentials, use new inprivate browser window to connect to the Ranger Admin UI.</span></span>
   >
   >
2. <span data-ttu-id="22ff4-118">Přihlaste se pomocí doménového uživatelského jména a hesla správce clusteru:</span><span class="sxs-lookup"><span data-stu-id="22ff4-118">Log in using the cluster administrator domain user name and password:</span></span>

    ![Domovská stránka Ranger služby HDInsight připojené k doméně](./media/hdinsight-domain-joined-run-hive/hdinsight-domain-joined-ranger-home-page.png)

    <span data-ttu-id="22ff4-120">V současné době Ranger funguje pouze s Yarn a Hivem.</span><span class="sxs-lookup"><span data-stu-id="22ff4-120">Currently, Ranger only works with Yarn and Hive.</span></span>

## <a name="create-domain-users"></a><span data-ttu-id="22ff4-121">Vytvoření uživatelů domén</span><span class="sxs-lookup"><span data-stu-id="22ff4-121">Create Domain users</span></span>
<span data-ttu-id="22ff4-122">V tématu [Konfigurace clusterů HDInsight připojených k doméně](hdinsight-domain-joined-configure.md#create-and-configure-azure-ad-ds-for-your-azure-ad) jste vytvořili uživatele hiveuser1 a hiveuser2.</span><span class="sxs-lookup"><span data-stu-id="22ff4-122">In [Configure Domain-joined HDInsight clusters](hdinsight-domain-joined-configure.md#create-and-configure-azure-ad-ds-for-your-azure-ad), you have created hiveruser1 and hiveuser2.</span></span> <span data-ttu-id="22ff4-123">V tomto kurzu použijete tyto dva uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="22ff4-123">You will use the two user account in this tutorial.</span></span>

## <a name="create-ranger-policies"></a><span data-ttu-id="22ff4-124">Vytvoření zásad Ranger</span><span class="sxs-lookup"><span data-stu-id="22ff4-124">Create Ranger policies</span></span>
<span data-ttu-id="22ff4-125">V této části vytvoříte dvě zásady Ranger pro přistupování k hivesampletable.</span><span class="sxs-lookup"><span data-stu-id="22ff4-125">In this section, you will create two Ranger policies for accessing hivesampletable.</span></span> <span data-ttu-id="22ff4-126">Udělíte oprávnění Vybrat na různé sady sloupců.</span><span class="sxs-lookup"><span data-stu-id="22ff4-126">You give select permission on different set of columns.</span></span> <span data-ttu-id="22ff4-127">Oba uživatelé byli vytvořeni v tématu [Konfigurace clusterů HDInsight připojených k doméně](hdinsight-domain-joined-configure.md#create-and-configure-azure-ad-ds-for-your-azure-ad).</span><span class="sxs-lookup"><span data-stu-id="22ff4-127">Both users were created in [Configure Domain-joined HDInsight clusters](hdinsight-domain-joined-configure.md#create-and-configure-azure-ad-ds-for-your-azure-ad).</span></span>  <span data-ttu-id="22ff4-128">V další části tyto dvě zásady otestujete v Excelu.</span><span class="sxs-lookup"><span data-stu-id="22ff4-128">In the next section, you will test the two policies in Excel.</span></span>

<span data-ttu-id="22ff4-129">**Vytvoření zásad Ranger**</span><span class="sxs-lookup"><span data-stu-id="22ff4-129">**To create Ranger policies**</span></span>

1. <span data-ttu-id="22ff4-130">Otevřete uživatelské rozhraní správce Ranger.</span><span class="sxs-lookup"><span data-stu-id="22ff4-130">Open Ranger Admin UI.</span></span> <span data-ttu-id="22ff4-131">Viz [Připojení k uživatelskému rozhraní správce Apache Ranger](#connect-to-apache-ranager-admin-ui).</span><span class="sxs-lookup"><span data-stu-id="22ff4-131">See [Connect to Apache Ranger Admin UI](#connect-to-apache-ranager-admin-ui).</span></span>
2. <span data-ttu-id="22ff4-132">V části **Hive** klikněte na **&lt;název_clusteru>_hive**.</span><span class="sxs-lookup"><span data-stu-id="22ff4-132">Click **&lt;ClusterName>_hive**, under **Hive**.</span></span> <span data-ttu-id="22ff4-133">Měly by se zobrazit dvě předem nakonfigurované zásady.</span><span class="sxs-lookup"><span data-stu-id="22ff4-133">You shall see two pre-configure policies.</span></span>
3. <span data-ttu-id="22ff4-134">Klikněte na **Add New Policy (Přidat novou zásadu)** a pak zadejte následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="22ff4-134">Click **Add New Policy**, and then enter the following values:</span></span>

   * <span data-ttu-id="22ff4-135">Policy name (Název zásady): read-hivesampletable-all</span><span class="sxs-lookup"><span data-stu-id="22ff4-135">Policy name: read-hivesampletable-all</span></span>
   * <span data-ttu-id="22ff4-136">Hive Database (Databáze Hivu): default (výchozí)</span><span class="sxs-lookup"><span data-stu-id="22ff4-136">Hive Database: default</span></span>
   * <span data-ttu-id="22ff4-137">table (tabulka): hivesampletable</span><span class="sxs-lookup"><span data-stu-id="22ff4-137">table: hivesampletable</span></span>
   * <span data-ttu-id="22ff4-138">Hive column (Sloupec Hivu):*</span><span class="sxs-lookup"><span data-stu-id="22ff4-138">Hive column: *</span></span>
   * <span data-ttu-id="22ff4-139">Select User (Vybrat uživatele): hiveuser1</span><span class="sxs-lookup"><span data-stu-id="22ff4-139">Select User: hiveuser1</span></span>
   * <span data-ttu-id="22ff4-140">Permissions (Oprávnění): select (vybrat)</span><span class="sxs-lookup"><span data-stu-id="22ff4-140">Permissions: select</span></span>

     ![Konfigurace zásady Hivu v Ranger služby HDInsight připojené k doméně](./media/hdinsight-domain-joined-run-hive/hdinsight-domain-joined-configure-ranger-policy.png)<span data-ttu-id="22ff4-142">.</span><span class="sxs-lookup"><span data-stu-id="22ff4-142">.</span></span>

     > [!NOTE]
     > <span data-ttu-id="22ff4-143">Pokud uživatel domény v části Select User (Vybrat uživatele) není k dispozici, chvíli počkejte, než se Ranger synchronizuje s AAD.</span><span class="sxs-lookup"><span data-stu-id="22ff4-143">If a domain user is not populated in Select User, wait a few moments for Ranger to sync with AAD.</span></span>
     >
     >
4. <span data-ttu-id="22ff4-144">Kliknutím na **Přidat** uložte zásadu.</span><span class="sxs-lookup"><span data-stu-id="22ff4-144">Click **Add** to save the policy.</span></span>
5. <span data-ttu-id="22ff4-145">Zopakujte poslední dva kroky a vytvořte další zásadu s následujícími vlastnostmi:</span><span class="sxs-lookup"><span data-stu-id="22ff4-145">Repeat the last two steps to create another policy with the following properties:</span></span>

   * <span data-ttu-id="22ff4-146">Policy name (Název zásady): read-hivesampletable-devicemake</span><span class="sxs-lookup"><span data-stu-id="22ff4-146">Policy name: read-hivesampletable-devicemake</span></span>
   * <span data-ttu-id="22ff4-147">Hive Database (Databáze Hivu): default (výchozí)</span><span class="sxs-lookup"><span data-stu-id="22ff4-147">Hive Database: default</span></span>
   * <span data-ttu-id="22ff4-148">table (tabulka): hivesampletable</span><span class="sxs-lookup"><span data-stu-id="22ff4-148">table: hivesampletable</span></span>
   * <span data-ttu-id="22ff4-149">Hive column (Sloupec Hivu): clientid, devicemake</span><span class="sxs-lookup"><span data-stu-id="22ff4-149">Hive column: clientid, devicemake</span></span>
   * <span data-ttu-id="22ff4-150">Select User (Vybrat uživatele): hiveuser2</span><span class="sxs-lookup"><span data-stu-id="22ff4-150">Select User: hiveuser2</span></span>
   * <span data-ttu-id="22ff4-151">Permissions (Oprávnění): select (vybrat)</span><span class="sxs-lookup"><span data-stu-id="22ff4-151">Permissions: select</span></span>

## <a name="create-hive-odbc-data-source"></a><span data-ttu-id="22ff4-152">Vytvoření zdroje dat Hive ODBC</span><span class="sxs-lookup"><span data-stu-id="22ff4-152">Create Hive ODBC data source</span></span>
<span data-ttu-id="22ff4-153">Pokyny najdete v tématu [Vytvoření zdroje dat Hive ODBC](hdinsight-connect-excel-hive-odbc-driver.md).</span><span class="sxs-lookup"><span data-stu-id="22ff4-153">The instructions can be found in [Create Hive ODBC data source](hdinsight-connect-excel-hive-odbc-driver.md).</span></span>  

    <span data-ttu-id="22ff4-154">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="22ff4-154">Property</span></span>|<span data-ttu-id="22ff4-155">Popis</span><span class="sxs-lookup"><span data-stu-id="22ff4-155">Description</span></span>
    ---|---
    <span data-ttu-id="22ff4-156">Název zdroje dat</span><span class="sxs-lookup"><span data-stu-id="22ff4-156">Data Source Name</span></span>|<span data-ttu-id="22ff4-157">Zadejte název zdroje dat.</span><span class="sxs-lookup"><span data-stu-id="22ff4-157">Give a name to your data source</span></span>
    <span data-ttu-id="22ff4-158">Hostitel</span><span class="sxs-lookup"><span data-stu-id="22ff4-158">Host</span></span>|<span data-ttu-id="22ff4-159">Zadejte &lt;název_clusteru_HDInsight>.azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="22ff4-159">Enter &lt;HDInsightClusterName>.azurehdinsight.net.</span></span> <span data-ttu-id="22ff4-160">Například mujHDICluster.azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="22ff4-160">For example, myHDICluster.azurehdinsight.net</span></span>
    <span data-ttu-id="22ff4-161">Port</span><span class="sxs-lookup"><span data-stu-id="22ff4-161">Port</span></span>|<span data-ttu-id="22ff4-162">Použijte <strong>443</strong>.</span><span class="sxs-lookup"><span data-stu-id="22ff4-162">Use <strong>443</strong>.</span></span> <span data-ttu-id="22ff4-163">(Tento port se změnil z 563 na 443.)</span><span class="sxs-lookup"><span data-stu-id="22ff4-163">(This port has been changed from 563 to 443.)</span></span>
    <span data-ttu-id="22ff4-164">Databáze</span><span class="sxs-lookup"><span data-stu-id="22ff4-164">Database</span></span>|<span data-ttu-id="22ff4-165">Použijte <strong>Výchozí</strong>.</span><span class="sxs-lookup"><span data-stu-id="22ff4-165">Use <strong>Default</strong>.</span></span>
    <span data-ttu-id="22ff4-166">Typ serveru Hive</span><span class="sxs-lookup"><span data-stu-id="22ff4-166">Hive Server Type</span></span>|<span data-ttu-id="22ff4-167">Vyberte <strong>Hive Server 2</strong>.</span><span class="sxs-lookup"><span data-stu-id="22ff4-167">Select <strong>Hive Server 2</strong></span></span>
    <span data-ttu-id="22ff4-168">Mechanismus</span><span class="sxs-lookup"><span data-stu-id="22ff4-168">Mechanism</span></span>|<span data-ttu-id="22ff4-169">Vyberte <strong>Služba Azure HDInsight</strong></span><span class="sxs-lookup"><span data-stu-id="22ff4-169">Select <strong>Azure HDInsight Service</strong></span></span>
    <span data-ttu-id="22ff4-170">Cesta HTTP</span><span class="sxs-lookup"><span data-stu-id="22ff4-170">HTTP Path</span></span>|<span data-ttu-id="22ff4-171">Ponechte prázdné.</span><span class="sxs-lookup"><span data-stu-id="22ff4-171">Leave it blank.</span></span>
    <span data-ttu-id="22ff4-172">Uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="22ff4-172">User Name</span></span>|<span data-ttu-id="22ff4-173">Zadejte hiveuser1@contoso158.onmicrosoft.com.</span><span class="sxs-lookup"><span data-stu-id="22ff4-173">Enter hiveuser1@contoso158.onmicrosoft.com.</span></span> <span data-ttu-id="22ff4-174">Aktualizujte název domény, pokud se liší.</span><span class="sxs-lookup"><span data-stu-id="22ff4-174">Update the domain name if it is different.</span></span>
    <span data-ttu-id="22ff4-175">Heslo</span><span class="sxs-lookup"><span data-stu-id="22ff4-175">Password</span></span>|<span data-ttu-id="22ff4-176">Zadejte heslo uživatele hiveuser1.</span><span class="sxs-lookup"><span data-stu-id="22ff4-176">Enter the password for hiveuser1.</span></span>
    </table>

<span data-ttu-id="22ff4-177">Nezapomeňte před uložením zdroje dat kliknout na **Otestovat**.</span><span class="sxs-lookup"><span data-stu-id="22ff4-177">Make sure to click **Test** before saving the data source.</span></span>

## <a name="import-data-into-excel-from-hdinsight"></a><span data-ttu-id="22ff4-178">Import dat do Excelu ze služby HDInsight</span><span class="sxs-lookup"><span data-stu-id="22ff4-178">Import data into Excel from HDInsight</span></span>
<span data-ttu-id="22ff4-179">V předchozí části jste nakonfigurovali dvě zásady.</span><span class="sxs-lookup"><span data-stu-id="22ff4-179">In the last section, you have configured two policies.</span></span>  <span data-ttu-id="22ff4-180">Uživatel hiveuser1 má oprávnění Vybrat na všech sloupcích a hiveuser2 má oprávnění Vybrat na dvou sloupcích.</span><span class="sxs-lookup"><span data-stu-id="22ff4-180">hiveuser1 has the select permission on all the columns, and hiveuser2 has the select permission on two columns.</span></span> <span data-ttu-id="22ff4-181">V této části se budete vydávat za tyto dva uživatele za účelem importu dat do Excelu.</span><span class="sxs-lookup"><span data-stu-id="22ff4-181">In this section, you impersonate the two users to import data into Excel.</span></span>

1. <span data-ttu-id="22ff4-182">V Excelu otevřete nový nebo existující sešit.</span><span class="sxs-lookup"><span data-stu-id="22ff4-182">Open a new or existing workbook in Excel.</span></span>
2. <span data-ttu-id="22ff4-183">Na kartě **Data** klikněte na **Z jiných zdrojů dat** a pak kliknutím na **Z Průvodce datovým připojením** spusťte **Průvodce datovým připojením**.</span><span class="sxs-lookup"><span data-stu-id="22ff4-183">From the **Data** tab, click **From Other Data Sources**, and then click **From Data Connection Wizard** to launch the **Data Connection Wizard**.</span></span>

    <span data-ttu-id="22ff4-184">![Otevřete Průvodce připojením dat][img-hdi-simbahiveodbc.excel.dataconnection]</span><span class="sxs-lookup"><span data-stu-id="22ff4-184">![Open data connection wizard][img-hdi-simbahiveodbc.excel.dataconnection]</span></span>
3. <span data-ttu-id="22ff4-185">Jako zdroj dat vyberte **ODBC DSN** a pak klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="22ff4-185">Select **ODBC DSN** as the data source, and then click **Next**.</span></span>
4. <span data-ttu-id="22ff4-186">Ze zdrojů dat ODBC vyberte název zdroje dat, který jste vytvořili v předchozím kroku, a klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="22ff4-186">From ODBC data sources, select the data source name that you created in the previous step, and then  click **Next**.</span></span>
5. <span data-ttu-id="22ff4-187">V průvodci znovu zadejte heslo pro cluster a pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="22ff4-187">Re-enter the password for the cluster in the wizard, and then click **OK**.</span></span> <span data-ttu-id="22ff4-188">Počkejte, než se otevře dialogové okno **Vybrat databázi a tabulku**.</span><span class="sxs-lookup"><span data-stu-id="22ff4-188">Wait for the **Select Database and Table** dialog to open.</span></span> <span data-ttu-id="22ff4-189">Může to trvat několik sekund.</span><span class="sxs-lookup"><span data-stu-id="22ff4-189">This can take a few seconds.</span></span>
6. <span data-ttu-id="22ff4-190">Vyberte **hivesampletable** a pak klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="22ff4-190">Select **hivesampletable**, and then click **Next**.</span></span>
7. <span data-ttu-id="22ff4-191">Klikněte na **Finish** (Dokončit).</span><span class="sxs-lookup"><span data-stu-id="22ff4-191">Click **Finish**.</span></span>
8. <span data-ttu-id="22ff4-192">V dialogovém okně **Import dat** můžete změnit, nebo zadat dotaz.</span><span class="sxs-lookup"><span data-stu-id="22ff4-192">In the **Import Data** dialog, you can change or specify the query.</span></span> <span data-ttu-id="22ff4-193">To provedete kliknutím na **Vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="22ff4-193">To do so, click **Properties**.</span></span> <span data-ttu-id="22ff4-194">Může to trvat několik sekund.</span><span class="sxs-lookup"><span data-stu-id="22ff4-194">This can take a few seconds.</span></span>
9. <span data-ttu-id="22ff4-195">Klikněte na kartu **Definice**.</span><span class="sxs-lookup"><span data-stu-id="22ff4-195">Click the **Definition** tab.</span></span> <span data-ttu-id="22ff4-196">Text příkazu je:</span><span class="sxs-lookup"><span data-stu-id="22ff4-196">The command text is:</span></span>

       SELECT * FROM "HIVE"."default"."hivesampletable"

   <span data-ttu-id="22ff4-197">Podle zásad Ranger, které jste nadefinovali, má uživatel hiveuser1 oprávnění Vybrat na všech sloupcích.</span><span class="sxs-lookup"><span data-stu-id="22ff4-197">By the Ranger policies you defined,  hiveuser1 has select permission on all the columns.</span></span>  <span data-ttu-id="22ff4-198">Takže tento dotaz funguje s přihlašovacími údaji uživatele hiveuser1, ale nefunguje s přihlašovacími údaji uživatele hiveuser2.</span><span class="sxs-lookup"><span data-stu-id="22ff4-198">So this query works with hiveuser1's credentials, but this query does not not work with hiveuser2's credentials.</span></span>

   <span data-ttu-id="22ff4-199">![Vlastnosti připojení][img-hdi-simbahiveodbc-excel-connectionproperties]</span><span class="sxs-lookup"><span data-stu-id="22ff4-199">![Connection Properties][img-hdi-simbahiveodbc-excel-connectionproperties]</span></span>
10. <span data-ttu-id="22ff4-200">Kliknutím na **OK** zavřete dialogové okno Vlastnosti připojení.</span><span class="sxs-lookup"><span data-stu-id="22ff4-200">Click **OK** to close the Connection Properties dialog.</span></span>
11. <span data-ttu-id="22ff4-201">Kliknutím na **OK** zavřete dialogové okno **Import Dat**.</span><span class="sxs-lookup"><span data-stu-id="22ff4-201">Click **OK** to close the **Import Data** dialog.</span></span>  
12. <span data-ttu-id="22ff4-202">Znovu zadejte heslo uživatele hiveuser1 a pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="22ff4-202">Reenter the password for hiveuser1, and then click **OK**.</span></span> <span data-ttu-id="22ff4-203">Import dat do Excelu trvá několik sekund.</span><span class="sxs-lookup"><span data-stu-id="22ff4-203">It takes a few seconds before data gets imported to Excel.</span></span> <span data-ttu-id="22ff4-204">Po dokončení importu byste měli vidět 11 sloupců dat.</span><span class="sxs-lookup"><span data-stu-id="22ff4-204">When it is done, you shall see 11 columns of data.</span></span>

<span data-ttu-id="22ff4-205">Testování druhé zásady (read-hivesampletable-devicemake) vytvořené v předchozí části</span><span class="sxs-lookup"><span data-stu-id="22ff4-205">To test the second policy (read-hivesampletable-devicemake) you created in the last section</span></span>

1. <span data-ttu-id="22ff4-206">Přidejte v Excelu nový list.</span><span class="sxs-lookup"><span data-stu-id="22ff4-206">Add a new sheet in Excel.</span></span>
2. <span data-ttu-id="22ff4-207">K importu dat použijte předchozí postup.</span><span class="sxs-lookup"><span data-stu-id="22ff4-207">Follow the last procedure to import the data.</span></span>  <span data-ttu-id="22ff4-208">Jediná změna, kterou provedete, bude použití přihlašovacích údajů uživatele hiveuser2 namísto uživatele hiveuser1.</span><span class="sxs-lookup"><span data-stu-id="22ff4-208">The only change you will make is to use hiveuser2's credentials instead of hiveuser1's.</span></span> <span data-ttu-id="22ff4-209">To se nezdaří, protože uživatel hiveuser2 má pouze oprávnění k zobrazení dvou sloupců.</span><span class="sxs-lookup"><span data-stu-id="22ff4-209">This will fail because hiveuser2 only has permission to see two columns.</span></span> <span data-ttu-id="22ff4-210">Měla by se zobrazit následující chyba:</span><span class="sxs-lookup"><span data-stu-id="22ff4-210">You shall get the following error:</span></span>

        [Microsoft][HiveODBC] (35) Error from Hive: error code: '40000' error message: 'Error while compiling statement: FAILED: HiveAccessControlException Permission denied: user [hiveuser2] does not have [SELECT] privilege on [default/hivesampletable/clientid,country ...]'.
3. <span data-ttu-id="22ff4-211">K importu dat použijte stejný postup.</span><span class="sxs-lookup"><span data-stu-id="22ff4-211">Follow the same procedure to import data.</span></span> <span data-ttu-id="22ff4-212">Tentokrát použijte přihlašovací údaje uživatele hiveuser2 a také změňte příkaz SELECT z:</span><span class="sxs-lookup"><span data-stu-id="22ff4-212">This time, use hiveuser2's credentials, and also modify the select statement from:</span></span>

        SELECT * FROM "HIVE"."default"."hivesampletable"

    <span data-ttu-id="22ff4-213">na:</span><span class="sxs-lookup"><span data-stu-id="22ff4-213">to:</span></span>

        SELECT clientid, devicemake FROM "HIVE"."default"."hivesampletable"

    <span data-ttu-id="22ff4-214">Po dokončení importu byste měli vidět naimportované dva sloupce dat.</span><span class="sxs-lookup"><span data-stu-id="22ff4-214">When it is done, you shall see two columns of data imported.</span></span>

## <a name="next-steps"></a><span data-ttu-id="22ff4-215">Další kroky</span><span class="sxs-lookup"><span data-stu-id="22ff4-215">Next steps</span></span>
* <span data-ttu-id="22ff4-216">Pokud chcete konfigurovat cluster HDInsight připojený k doméně, přečtěte si téma [Konfigurace clusterů HDInsight připojených k doméně](hdinsight-domain-joined-configure.md).</span><span class="sxs-lookup"><span data-stu-id="22ff4-216">For configuring a Domain-joined HDInsight cluster, see [Configure Domain-joined HDInsight clusters](hdinsight-domain-joined-configure.md).</span></span>
* <span data-ttu-id="22ff4-217">Pokud chcete spravovat clustery HDInsight připojené k doméně, přečtěte si téma [Správa clusterů HDInsight připojených k doméně](hdinsight-domain-joined-manage.md).</span><span class="sxs-lookup"><span data-stu-id="22ff4-217">For managing a Domain-joined HDInsight clusters, see [Manage Domain-joined HDInsight clusters](hdinsight-domain-joined-manage.md).</span></span>
* <span data-ttu-id="22ff4-218">Spuštění dotazů Hive pomocí protokolu SSH v clusterech HDInsight připojený k doméně, najdete v části [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).</span><span class="sxs-lookup"><span data-stu-id="22ff4-218">For running Hive queries using SSH on Domain-joined HDInsight clusters, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).</span></span>
* <span data-ttu-id="22ff4-219">Pokud se chcete připojit k Hivu pomocí Hive JDBC, přečtěte si téma [Připojení k Hivu ve službě Azure HDInsight pomocí ovladače Hive JDBC](hdinsight-connect-hive-jdbc-driver.md).</span><span class="sxs-lookup"><span data-stu-id="22ff4-219">For Connecting Hive using Hive JDBC, see [Connect to Hive on Azure HDInsight using the Hive JDBC driver](hdinsight-connect-hive-jdbc-driver.md)</span></span>
* <span data-ttu-id="22ff4-220">Pokud chcete připojit Excel k systému Hadoop pomocí rozhraní Hive ODBC, přečtěte si téma [Připojení Excelu k systému Hadoop pomocí ovladače Microsoft Hive ODBC](hdinsight-connect-excel-hive-odbc-driver.md).</span><span class="sxs-lookup"><span data-stu-id="22ff4-220">For connecting Excel to Hadoop using Hive ODBC, see [Connect Excel to Hadoop with the Microsoft Hive ODBC drive](hdinsight-connect-excel-hive-odbc-driver.md)</span></span>
* <span data-ttu-id="22ff4-221">Pokud chcete připojit Excel k systému Hadoop pomocí doplňku Power Query, přečtěte si téma [Připojení Excelu k systému Hadoop pomocí doplňku Power Query](hdinsight-connect-excel-power-query.md).</span><span class="sxs-lookup"><span data-stu-id="22ff4-221">For connecting Excel to Hadoop using Power Query, see [Connect Excel to Hadoop by using Power Query](hdinsight-connect-excel-power-query.md)</span></span>
