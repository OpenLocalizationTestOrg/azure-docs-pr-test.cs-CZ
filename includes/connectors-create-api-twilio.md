### <a name="prerequisites"></a><span data-ttu-id="01e2c-101">Požadavky</span><span class="sxs-lookup"><span data-stu-id="01e2c-101">Prerequisites</span></span>
* <span data-ttu-id="01e2c-102">Účet Twilio</span><span class="sxs-lookup"><span data-stu-id="01e2c-102">A Twilio account</span></span>
* <span data-ttu-id="01e2c-103">Ověřené telefonní číslo Twilio, který může přijímat zprávy SMS</span><span class="sxs-lookup"><span data-stu-id="01e2c-103">A verified Twilio phone number that can receive SMS</span></span>
* <span data-ttu-id="01e2c-104">Ověřené Twilio telefonní číslo, které může poslat SMS</span><span class="sxs-lookup"><span data-stu-id="01e2c-104">A verified Twilio phone number that can send SMS</span></span>

> [!NOTE]
> <span data-ttu-id="01e2c-105">Pokud používáte zkušební účet Twilio, lze odeslat pouze SMS k **ověřit** telefonních čísel.</span><span class="sxs-lookup"><span data-stu-id="01e2c-105">If you are using a Twilio trial account, you can only send SMS to **verified** phone numbers.</span></span>  
> 
> 

<span data-ttu-id="01e2c-106">Než účtu Twilio můžete použít v aplikaci logiky, musíte je nejdříve autorizovat aplikaci logiky se připojit ke svému účtu Twilio.</span><span class="sxs-lookup"><span data-stu-id="01e2c-106">Before you can use your Twilio account in a Logic app, you must authorize the Logic app to connect to your Twilio account.</span></span> <span data-ttu-id="01e2c-107">Naštěstí můžete k tomu snadno z v rámci aplikace logiky na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="01e2c-107">Fortunately, you can do this easily from within your Logic app on the Azure Portal.</span></span> 

<span data-ttu-id="01e2c-108">Tady jsou kroky k autorizaci aplikace logiky pro připojení k účtu Twilio:</span><span class="sxs-lookup"><span data-stu-id="01e2c-108">Here are the steps to authorize your Logic app to connect to your Twilio account:</span></span>

1. <span data-ttu-id="01e2c-109">Chcete-li vytvořit připojení k Twiliu, v návrháři aplikace logiky, vyberte **zobrazit Microsoft spravované rozhraní API** v rozevíracím seznamu zadejte *Twilio* do vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="01e2c-109">To create a connection to Twilio, in the Logic app designer, select **Show Microsoft managed APIs** in the drop down list then enter *Twilio* in the search box.</span></span> <span data-ttu-id="01e2c-110">Vyberte aktivační události nebo akci, kterou budete chtít použít:</span><span class="sxs-lookup"><span data-stu-id="01e2c-110">Select the trigger or action you'll like to use:</span></span>  
   ![](./media/connectors-create-api-twilio/twilio-0.png)
2. <span data-ttu-id="01e2c-111">Pokud jste nevytvořili žádné připojení k Twiliu před, budete získat zobrazí výzva k zadání přihlašovacích údajů vaší Twilio.</span><span class="sxs-lookup"><span data-stu-id="01e2c-111">If you haven't created any connections to Twilio before, you'll get prompted to provide your Twilio credentials.</span></span> <span data-ttu-id="01e2c-112">Tyto přihlašovací údaje se použije k autorizaci aplikace logiky pro připojení k a přístup k datům účtu Twilio:</span><span class="sxs-lookup"><span data-stu-id="01e2c-112">These credentials will be used to authorize your Logic app to connect to, and access your Twilio account's data:</span></span>  
   ![](./media/connectors-create-api-twilio/twilio-1.png)  
3. <span data-ttu-id="01e2c-113">Budete potřebovat **id účtu Twilio** a **Twilio přístupový token** z řídicího panelu Twilio, přihlaste se k účtu Twilio teď se získat tyto dva údaje:</span><span class="sxs-lookup"><span data-stu-id="01e2c-113">You'll need the **Twilio account id** and **Twilio access token**  from the dashboard in Twilio, so log in to your Twilio account now to grab these two pieces of information:</span></span>  
   ![](./media/connectors-create-api-twilio/twilio-2.png)  
4. <span data-ttu-id="01e2c-114">K identifikaci tyto dvě údaje Twilio a logiku aplikace použít odlišné názvy.</span><span class="sxs-lookup"><span data-stu-id="01e2c-114">Twilio and Logic apps use different names to identify these two pieces of infomation.</span></span> <span data-ttu-id="01e2c-115">Zde je, jak je nutné mapovat do dialogového okna aplikace logiky:![](./media/connectors-create-api-twilio/twilio-3.png)</span><span class="sxs-lookup"><span data-stu-id="01e2c-115">Here is how you must map them to the Logic apps dialog: ![](./media/connectors-create-api-twilio/twilio-3.png)</span></span>  
5. <span data-ttu-id="01e2c-116">Vyberte **vytvořit připojení** tlačítko:</span><span class="sxs-lookup"><span data-stu-id="01e2c-116">Select the **Create connection** button:</span></span>  
   ![](./media/connectors-create-api-twilio/twilio-4.png)
6. <span data-ttu-id="01e2c-117">Všimněte si vytvořil připojení a je nyní můžete pokračovat v dalších krocích v aplikaci logiky:</span><span class="sxs-lookup"><span data-stu-id="01e2c-117">Notice the connection has been created and you are now free to proceed with the other steps in your Logic app:</span></span>  
   ![](./media/connectors-create-api-twilio/twilio-5.png)

