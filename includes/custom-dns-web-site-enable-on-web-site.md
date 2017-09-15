<span data-ttu-id="65db1-101">Po rozšíření mají záznamy pro název domény, je třeba přidružit k vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="65db1-101">After the records for your domain name have propagated, you must associate them with your Web App.</span></span> <span data-ttu-id="65db1-102">Pomocí následujících kroků povolte názvů domén pomocí webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="65db1-102">Use the following steps to enable the domain names using your web browser.</span></span>

> [!NOTE]
> <span data-ttu-id="65db1-103">Může trvat nějakou dobu záznamů TXT vytvořili v předchozích krocích rozšíří v rámci systému DNS.</span><span class="sxs-lookup"><span data-stu-id="65db1-103">It can take some time for TXT records created in the previous steps to propagate through the DNS system.</span></span> <span data-ttu-id="65db1-104">Název domény nelze přidat do vaší webové aplikace, dokud se rozšíří záznam TXT.</span><span class="sxs-lookup"><span data-stu-id="65db1-104">You cannot add the domain name of to your web app until the TXT record has propagated.</span></span> <span data-ttu-id="65db1-105">Pokud používáte záznam A, nelze přidat název záznamu domény A do vaší webové aplikace dokud má rozšíří záznam TXT vytvořili v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="65db1-105">If you are using an A record, you cannot add the A record domain name to your web app until the TXT record created in the previous step has propagated.</span></span>
> 
> <span data-ttu-id="65db1-106">Je třeba použít službu <a href="http://www.digwebinterface.com/">http://www.digwebinterface.com/</a> k ověření, že záznam TXT je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="65db1-106">You can use a service such as <a href="http://www.digwebinterface.com/">http://www.digwebinterface.com/</a> to verify that the TXT record is available.</span></span>
> 
> 

1. <span data-ttu-id="65db1-107">V prohlížeči otevřete [portálu Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="65db1-107">In your browser, open the [Azure Portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="65db1-108">V **webové aplikace** , klikněte na název vaší webové aplikace a pak vyberte **vlastní domény**</span><span class="sxs-lookup"><span data-stu-id="65db1-108">In the **Web Apps** tab, click the name of your web app, and then select **Custom domains**</span></span>
   
    ![](./media/custom-dns-web-site/dncmntask-cname-6.png)
3. <span data-ttu-id="65db1-109">V **vlastní domény** okně klikněte na tlačítko **přidat název hostitele**.</span><span class="sxs-lookup"><span data-stu-id="65db1-109">In the **Custom domains** blade, click **Add hostname**.</span></span>
4. <span data-ttu-id="65db1-110">Použití **Hostname** textová pole zadejte název domény, který chcete přidružit k této webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="65db1-110">Use the **Hostname** text boxes to enter the domain names to associate with this web app.</span></span>
   
    ![](./media/custom-dns-web-site/add-custom-domain.png)
5. <span data-ttu-id="65db1-111">Klikněte na tlačítko **ověření**.</span><span class="sxs-lookup"><span data-stu-id="65db1-111">Click **Validate**.</span></span>
6. <span data-ttu-id="65db1-112">Po kliknutí na tlačítko **ověřením** Azure bude ji pracovní postup ověření domény.</span><span class="sxs-lookup"><span data-stu-id="65db1-112">Upon clicking **Validate** Azure will kick off Domain Verification workflow.</span></span> <span data-ttu-id="65db1-113">To bude kontrolovat vlastnictví domény a také název hostitele dostupnosti a sestava úspěch nebo podrobné chybové s doporučený guidence o tom, jak chybu opravit.</span><span class="sxs-lookup"><span data-stu-id="65db1-113">This will check for Domain ownership as well as Hostname availability and report success or detailed error with prescriptive guidence on how to fix the error.</span></span>    

<span data-ttu-id="65db1-114">V tomto okamžiku by měl být zadejte vlastní název domény v prohlížeči a zobrazit tak, že je úspěšně přejdete do vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="65db1-114">At this point, you should be able to enter the custom domain name in your browser and see that it successfully takes you to your web app.</span></span>

