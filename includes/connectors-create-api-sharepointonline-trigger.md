<span data-ttu-id="b99e3-101">V tomto příkladu I vám ukáže, jak toouse hello **SharePoint Online - při vytvoření nové položky** aktivují tooinitiate pracovní postup aplikace logiky, když se vytvoří novou položku v seznamu SharePoint Online.</span><span class="sxs-lookup"><span data-stu-id="b99e3-101">In this example, I will show you how toouse hello **SharePoint Online - When a new item is created** trigger tooinitiate a logic app workflow when a new item is created in a SharePoint Online list.</span></span>

> [!NOTE]
> <span data-ttu-id="b99e3-102">Zobrazí se výzvami toosign k účtu služby SharePoint Pokud jste ještě nevytvořili *připojení* tooSharePoint Online.</span><span class="sxs-lookup"><span data-stu-id="b99e3-102">You will get prompted toosign into your SharePoint account if you have not already created a *connection* tooSharePoint Online.</span></span>  
> 
> 

1. <span data-ttu-id="b99e3-103">Zadejte *sharepoint* hello vyhledávacího pole v designeru aplikace logiky hello zvolte hello **SharePoint Online - při vytvoření nové položky** aktivační události.</span><span class="sxs-lookup"><span data-stu-id="b99e3-103">Enter *sharepoint* in hello search box on hello logic apps designer then select hello **SharePoint Online - When a new item is created**  trigger.</span></span>  
   <span data-ttu-id="b99e3-104">![SharePoint online aktivace image](./media/connectors-create-api-sharepointonline/trigger-1.png)</span><span class="sxs-lookup"><span data-stu-id="b99e3-104">![SharePoint online trigger image ](./media/connectors-create-api-sharepointonline/trigger-1.png)</span></span>  
2. <span data-ttu-id="b99e3-105">Hello **při vytvoření nové položky** ovládací prvek je zobrazen.</span><span class="sxs-lookup"><span data-stu-id="b99e3-105">hello **When a new item is created** control is displayed.</span></span>  
   <span data-ttu-id="b99e3-106">![SharePoint online aktivace obrázek 2](./media/connectors-create-api-sharepointonline/trigger-2.png)</span><span class="sxs-lookup"><span data-stu-id="b99e3-106">![SharePoint online trigger image 2](./media/connectors-create-api-sharepointonline/trigger-2.png)</span></span>   
3. <span data-ttu-id="b99e3-107">Vyberte **URL webu**.</span><span class="sxs-lookup"><span data-stu-id="b99e3-107">Select a **Site URL**.</span></span> <span data-ttu-id="b99e3-108">Toto je hello SharePoint online lokalita má toomonitor pro nové položky tootrigger pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="b99e3-108">This is hello SharePoint online site you want toomonitor for new items tootrigger your workflow.</span></span>  
   <span data-ttu-id="b99e3-109">![SharePoint online aktivace image 3](./media/connectors-create-api-sharepointonline/trigger-3.png)</span><span class="sxs-lookup"><span data-stu-id="b99e3-109">![SharePoint online trigger image 3](./media/connectors-create-api-sharepointonline/trigger-3.png)</span></span>   
4. <span data-ttu-id="b99e3-110">Vyberte **název seznamu**.</span><span class="sxs-lookup"><span data-stu-id="b99e3-110">Select a **List name**.</span></span> <span data-ttu-id="b99e3-111">Toto je seznam hello na web služby SharePoint Online, které chcete použít pro nové položky, které aktivují pracovní postup toomonitor hello.</span><span class="sxs-lookup"><span data-stu-id="b99e3-111">This is hello list on hello SharePoint Online site you want toomonitor for new items that will trigger your workflow.</span></span>  
   <span data-ttu-id="b99e3-112">![SharePoint online aktivace obrázek 4](./media/connectors-create-api-sharepointonline/trigger-4.png)</span><span class="sxs-lookup"><span data-stu-id="b99e3-112">![SharePoint online trigger image 4](./media/connectors-create-api-sharepointonline/trigger-4.png)</span></span>   

<span data-ttu-id="b99e3-113">Aplikace logiky v tomto okamžiku je nakonfigurovaná s aktivační událost, která zahájí spuštění hello jiných triggery a akce v pracovním postupu hello.</span><span class="sxs-lookup"><span data-stu-id="b99e3-113">At this point, your logic app has been configured with a trigger that will begin a run of hello other triggers and actions in hello workflow.</span></span> <span data-ttu-id="b99e3-114">Bude to trvat místní pokaždé, když se vytvoří novou položku v seznamu SharePoint Online, které jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="b99e3-114">This will take place each time a new item is created in SharePoint Online list you selected.</span></span>  
