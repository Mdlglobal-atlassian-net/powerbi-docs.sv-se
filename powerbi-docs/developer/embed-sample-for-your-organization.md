---
title: Bädda in Power BI-innehåll i ett program för din organisation
description: Lär dig att integrera eller bädda in en rapport, instrumentpanel eller panel i en webbapp med hjälp av Power BI-API:er för din organisation.
author: markingmyname
ms.author: maghan
ms.date: 07/13/2018
ms.topic: tutorial
ms.service: powerbi
ms.component: powerbi-developer
ms.custom: mvc
manager: kfile
ms.openlocfilehash: 544429528ed51dd2928eb82632f512ff3f7d5afd
ms.sourcegitcommit: fecea174721d0eb4e1927c1116d2604a822e4090
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/31/2018
ms.locfileid: "39359741"
---
# <a name="tutorial-embed-a-power-bi-report-dashboard-or-tile-into-an-application-for-your-organization"></a>Självstudier: Bädda in en Power BI-rapport, instrumentpanel eller panel till ett program för din organisation
Den här självstudien visar hur du integrerar en rapport i ett program som använder **Power BI .NET SDK** tillsammans med **Power BI JavaScript API** när du bäddar in **Power BI** i ett program för din organisation. Med **Power BI** kan du bädda in rapporter, instrumentpaneler eller paneler i ett program med **användare äger data**. **Användare äger data** gör att ditt program kan utöka Power BI-tjänsten.

![Visa program](media/embed-sample-for-your-organization/embed-sample-for-your-organization-035.png)

I de här självstudierna får du lära dig att
>[!div class="checklist"]
>* Registrera ett program i Azure.
>* Bädda in en Power BI-rapport i ett program.

## <a name="prerequisites"></a>Förutsättningar
Om du vill komma igång behöver du ett **Power BI Pro**-konto och en **Microsoft Azure**-prenumeration.

* Om du inte har registrerat dig för **Power BI Pro**, [registrerar du dig för en kostnadsfri utvärderingsversion](https://powerbi.microsoft.com/en-us/pricing/) innan du börjar.
* Om du inte har någon Azure-prenumeration kan du [skapa ett kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.
* Du måste ha en egen installation för [Azure Active Directory-klient](create-an-azure-active-directory-tenant.md).
* Du behöver [Visual Studio](https://www.visualstudio.com/) installerad (version 2013 eller senare).

## <a name="setup-your-embedded-analytics-development-environment"></a>Konfigurera den inbäddade utvecklingsmiljön för analysverktyg

Innan du börjar bädda in rapporter, instrumentpaneler eller paneler i din app måste du se till att din miljö har ställts in så att inbäddning tillåts. Som en del av installationen behöver du göra följande.

Med [integrationsverktyget](https://aka.ms/embedsetup/UserOwnsData) kommer du snabbt igång och kan ladda ned ett exempelprogram som steg för steg beskriver hur du skapar en miljö och bäddar in en rapport.

Om du i stället vill konfigurera miljön manuellt, fortsätter du bara nedan.
### <a name="register-an-application-in-azure-active-directory-azure-ad"></a>Registrera ett program i Azure Active Directory (Azure AD)

Du kan registrera din app med Azure Active Directory så att ditt program får åtkomst till Power BI REST-API:er. Därmed kan du upprätta en identitet för din app och ange behörigheter till Power BI REST-resurser.

1. Godkänn [villkoren för Microsoft Power BI-API](https://powerbi.microsoft.com/api-terms).

2. Logga in på [Azure Portal](https://portal.azure.com).

    ![Huvuddel för Azure Portal](media/embed-sample-for-your-organization/embed-sample-for-your-organization-002.png)

3. I det vänstra navigeringsfönstret väljer du **Alla tjänster**, **App-registreringar** och sedan **Ny appregistrering**.

    ![Sök efter appregistrering](media/embed-sample-for-your-organization/embed-sample-for-your-organization-003.png)</br>
    ![Ny appregistrering](media/embed-sample-for-your-organization/embed-sample-for-your-organization-004.png)

4. Följ anvisningarna och skapa ett nytt program. För **användare äger data** måste du använda **Webbapp/API** som programtyp. Du måste också ange en **inloggnings-URL** som **Azure AD** använder för att returnera tokensvar. Ange ett specifikt värde för ditt program (t. ex. http://localhost:13526/).

    ![Skapa app](media/embed-sample-for-your-organization/embed-sample-for-your-organization-005.png)

### <a name="apply-permissions-to-your-application-within-azure-active-directory"></a>Tillämpa behörigheter för ditt program i Azure Active Directory

Du måste aktivera ytterligare behörigheter för ditt program utöver vad som fanns på app-registreringssidan. Du måste du logga in med ett konto för en *global administratör* för att aktivera behörigheter.

### <a name="use-the-azure-active-directory-portal"></a>Använd Azure Active Directory-portalen

1. Bläddra till [App-registreringar](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ApplicationsListBlade) i Azure Portal och välj den app som du använder för att bädda in.

    ![Att välja App](media/embed-sample-for-your-organization/embed-sample-for-your-organization-006.png)

2. Välj **Inställningar**, sedan under **API-åtkomst** välj **Nödvändiga behörigheter**.

    ![Nödvändiga behörigheter](media/embed-sample-for-your-organization/embed-sample-for-your-organization-008.png)

3. Välj **Windows Azure Active Directory** och kontrollera att **Åtkomst till katalogen som den inloggade användaren** är markerad. Välj **Spara**.

    ![Windows Azure AD-behörigheter](media/embed-sample-for-your-organization/embed-sample-for-your-organization-011.png)

4. Välj **Lägg till**.

    ![Lägg till behörigheter](media/embed-sample-for-your-organization/embed-sample-for-your-organization-012.png)

5. Välj **Välj en API**.

    ![Lägg till API-åtkomst](media/embed-sample-for-your-organization/embed-sample-for-your-organization-013.png)

6. Välj **Power BI-tjänsten**och välj **Välj**.

    ![Välj PBI-tjänster](media/embed-sample-for-your-organization/embed-sample-for-your-organization-014.png)

7. Välj alla behörigheter under **Delegerade behörigheter**. Du måste välja dem separat för valen ska sparas. Välj **Spara** när du är klar.

    ![Välj delegerade behörigheter](media/embed-sample-for-your-organization/embed-sample-for-your-organization-015.png)

## <a name="setup-your-power-bi-environment"></a>Konfigurera din Power BI-miljö

### <a name="create-an-app-workspace"></a>Skapa en app-arbetsyta

Om du bäddar in rapporter, instrumentpaneler eller paneler för kunderna, måste du placera innehållet i en app-arbetsyta.

1. Börja med att skapa arbetsytan. Välj **Arbetsytor** > **Skapa apparbetsyta**. Det är här du placerar innehåll som appen behöver åtkomst till.

    ![Skapa arbetsyta](media/embed-sample-for-your-organization/embed-sample-for-your-organization-020.png)

2. Ge arbetsytan ett namn. Om motsvarande **Arbetsyte-ID** inte är tillgängligt, kan du redigera det för att få fram ett unikt ID. Detta ska också vara namnet på appen.

    ![Namn på arbetsytan](media/embed-sample-for-your-organization/embed-sample-for-your-organization-021.png)

3. Det finns ett par alternativ som du måste ställa in. Om du väljer **Offentlig** kan alla i din organisation se vad som finns på arbetsytan. Om du väljer **Privat** kan endast medlemmar i arbetsytan se innehållet.

    ![Privat/offentlig](media/embed-sample-for-your-organization/embed-sample-for-your-organization-022.png)

    Du kan inte ändra inställningen för Offentlig/Privat när du har skapat gruppen.

4. Du kan också välja om medlemmarna ska kunna **redigera** eller ha **skrivskyddad** åtkomst.

    ![Lägger till medlemmar](media/embed-sample-for-your-organization/embed-sample-for-your-organization-023.png)

5. Lägg till e-postadresserna för de personer som du vill ska ha åtkomst till arbetsytan och välj **Lägg till**. Du kan inte lägga till gruppalias, bara enskilda användare.

6. Bestäm för varje person om den vara medlem eller administratör. Administratörer kan redigera arbetsytan samt lägga till andra medlemmar. Medlemmar kan redigera innehållet i arbetsytan, såvida de inte har skrivskyddad åtkomst. Både administratörer och medlemmar kan publicera appen.

    Nu kan du visa det nya arbetsområdet. Power BI skapar arbetsytan och öppnar den. Den visas i listan med arbetsytor där du är medlem. Eftersom du är administratör kan du välja ellipsen (...) för att gå tillbaka och göra ändringar, lägga till nya medlemmar eller ändra deras behörigheter.

    ![Skapa arbetsyta](media/embed-sample-for-your-organization/embed-sample-for-your-organization-025.png)

### <a name="create-and-publish-your-reports"></a>Skapa och publicera rapporter

Du kan skapa rapporter och datauppsättningar som använder Power BI Desktop och publicera dessa rapporter till en apparbetsyta. Slutanvändaren som publicerar rapporterna måste ha en Power BI Pro-licens för att kunna publicera till en apparbetsyta.

1. Ladda ner exemplet [Bloggdemo](https://github.com/Microsoft/powerbi-desktop-samples) från GitHub.

    ![rapportera exempel](media/embed-sample-for-your-organization/embed-sample-for-your-organization-026-1.png)

2. Öppna PBIX-exempelrapporten i **Power BI Desktop**

   ![PBI-skrivbordsrapport](media/embed-sample-for-your-organization/embed-sample-for-your-organization-027.png)

3. Publicera till **app-arbetsytan**

   ![PBI-skrivbordsrapport](media/embed-sample-for-your-organization/embed-sample-for-your-organization-028.png)

    Nu kan du visa rapporten i Power BI-tjänsten online.

   ![PBI-skrivbordsrapport](media/embed-sample-for-your-organization/embed-sample-for-your-organization-029.png)

## <a name="embed-your-content-using-the-sample-application"></a>Bädda in innehåll med exempelprogrammet

Följ de här stegen om du vill börja bädda in innehåll med hjälp av ett exempelprogram.

1. Ladda ner [exempel på användare äger data](https://github.com/Microsoft/PowerBI-Developer-Samples) från GitHub för att komma igång.  Det finns tre olika exempelprogram. Ett för [rapporter](https://github.com/Microsoft/PowerBI-Developer-Samples/tree/master/User%20Owns%20Data/integrate-report-web-app), ett för [instrumentpaneler](https://github.com/Microsoft/PowerBI-Developer-Samples/tree/master/User%20Owns%20Data/integrate-dashboard-web-app) och ett för [paneler](https://github.com/Microsoft/PowerBI-Developer-Samples/tree/master/User%20Owns%20Data/integrate-tile-web-app).  Den här artikeln handlar om programmet för **rapporter** och vi går igenom stegen nedan.

    ![Exempelprogram för användare äger data](media/embed-sample-for-your-organization/embed-sample-for-your-organization-026.png)

2. Öppna filen Cloud.config i exempelprogrammet. Du måste fylla i några fält för att kunna köra programmet. **ClientID** och **ClientSecret**.

    ![Cloud Config-fil](media/embed-sample-for-your-organization/embed-sample-for-your-organization-030.png)

    Fyll i informationen **ClientID** med **Program-ID** från **Azure**. **ClientID** används av programmet för att identifiera sig för användare som du begär behörighet från.

    För att hämta **ClientID** gör du följande:

    Logga in på [Azure Portal](https://portal.azure.com).

    ![Huvuddel för Azure Portal](media/embed-sample-for-your-organization/embed-sample-for-your-organization-002.png)

    I det vänstra navigeringsfönstret väljer du **Alla tjänster** och **App-registreringar**.

    ![Sök efter appregistrering](media/embed-sample-for-your-organization/embed-sample-for-your-organization-003.png)

    Välj det program som behöver använda **ClientID**.

    ![Att välja App](media/embed-sample-for-your-organization/embed-sample-for-your-organization-006.png)

    Du bör se ett **program-ID** som har listats som en GUID. Använd detta **program-ID** som **ClientID** för programmet.

    ![ClientID](media/embed-sample-for-your-organization/embed-sample-for-your-organization-007.png)

    Fyll i informationen **ClientSecret** från avsnittet **Nycklar** från avsnittet **Appregistreringar** i **Azure**.

    För att hämta **ClientSecret** gör du följande:

    Logga in på [Azure Portal](https://portal.azure.com).

    ![Huvuddel för Azure Portal](media/embed-sample-for-your-organization/embed-sample-for-your-organization-002.png)

    I det vänstra navigeringsfönstret väljer du **Alla tjänster** och **App-registreringar**.

    ![Sök efter appregistrering](media/embed-sample-for-your-organization/embed-sample-for-your-organization-003.png)

    Välj det program som behöver använda **ClientSecret**.

    ![Att välja App](media/embed-sample-for-your-organization/embed-sample-for-your-organization-006.png)

    Välj **inställningar**.

    ![Inställningar](media/embed-sample-for-your-organization/embed-sample-for-your-organization-038.png)

    Välj **Nycklar**.

    ![Nycklar](media/embed-sample-for-your-organization/embed-sample-for-your-organization-039.png)

    Fyll i **Beskrivningen** med ett namn och välj en **Varaktighet**. Välj sedan **Spara** för att hämta **Värdet** för ditt program. När du stänger bladet **Nycklar** efter att ha sparat **nyckelvärdet** visas värdefältet endast som **_Dolt_** och då kan du inte hämta **nyckelvärdet**. Om du tappar bort **nyckelvärdet** måste du skapa ett nytt på **Microsoft Azure-portalen**.

    ![Nycklar](media/embed-sample-for-your-organization/embed-sample-for-your-organization-031.png)

     Fyll i **groupId**-information med **app-arbetsytan GUID** från Power BI.

    ![groupId](media/embed-sample-for-customers/embed-sample-for-customers-031.png)

    Fyll i **reportId**-information med **rapportera GUID** från Power BI.

    ![reportId](media/embed-sample-for-customers/embed-sample-for-customers-032.png)

3. Kör programmet!

    Välj först **Kör** i **Visual Studio**.

    ![Kör programmet](media/embed-sample-for-your-organization/embed-sample-for-your-organization-033.png)

    Välj sedan **Hämta rapport**.

    ![Välj ett innehåll](media/embed-sample-for-your-organization/embed-sample-for-your-organization-034.png)

    Nu kan du visa rapporten i exempelprogrammet.

    ![Visa program](media/embed-sample-for-your-organization/embed-sample-for-your-organization-035.png)

## <a name="embed-your-content-within-your-application"></a>Bädda in innehåll i programmet
Innehåll kan bäddas in med hjälp av [Power BI REST API:er](https://docs.microsoft.com/rest/api/power-bi/), men exempelkoderna som beskrivs i den här artikeln görs med **.NET SDK**.

Om du vill integrera en rapport i en webbapp använder du **Power BI REST-API** eller **Power BI C# SDK** och en Azure Active Directory-**åtkomsttoken** för auktorisering till att hämta en rapport. Sedan kan du läsa in rapporten med samma **åtkomsttoken**. **Power BI REST-API** ger programmeringsåtkomst till vissa **Power BI**-resurser. Mer information finns i [Power BI REST API](https://docs.microsoft.com/rest/api/power-bi/) och [Power BI JavaScript API](https://github.com/Microsoft/PowerBI-JavaScript).

### <a name="get-an-access-token-from-azure-ad"></a>Hämta en åtkomsttoken från Azure AD
I ditt program måste du först hämta en **åtkomsttoken** från Azure AD innan du kan anropa Power BI REST API:t. Mer information finns i [Autentisera användare och hämta en Azure AD-åtkomsttoken för din Power BI-app](get-azuread-access-token.md).

### <a name="get-a-report"></a>Hämta en rapport
Hämta en **Power BI**-rapport genom att använda åtgärden [Hämta rapporter](https://docs.microsoft.com/rest/api/power-bi/reports/getreports) som hämtar en lista med **Power BI-rapporter**. Du kan hämta ett rapport-ID från listan med rapporter.

### <a name="get-reports-using-an-access-token"></a>Hämta rapporter med hjälp av en åtkomsttoken
Åtgärden [Hämta rapporter](https://docs.microsoft.com/rest/api/power-bi/reports/getreports) returnerar en lista med rapporter. Du kan hämta en enda rapport från listan med rapporter.

Om du vill göra REST API-anrop måste du inkludera en *auktoriserings*rubrik i formatet *Ägare {åtkomsttoken}*.

#### <a name="get-reports-with-the-rest-api"></a>Hämta rapporter med REST API

Här är ett kodexempel på hur du hämtar rapporter med **REST API**.

*Ett exempel på hur du hämtar ett innehållsobjekt som du vill bädda in (rapport, instrumentpanel eller panel) finns i filen **_Default.aspx.cs_** i [exempelprogrammet](#embed-your-content-using-the-sample-application).*

```csharp
using Newtonsoft.Json;

//Get a Report. In this sample, you get the first Report.
protected void GetReport(int index)
{
    //Configure Reports request
    System.Net.WebRequest request = System.Net.WebRequest.Create(
        String.Format("{0}/Reports",
        baseUri)) as System.Net.HttpWebRequest;

    request.Method = "GET";
    request.ContentLength = 0;
    request.Headers.Add("Authorization", String.Format("Bearer {0}", accessToken.Value));

    //Get Reports response from request.GetResponse()
    using (var response = request.GetResponse() as System.Net.HttpWebResponse)
    {
        //Get reader from response stream
        using (var reader = new System.IO.StreamReader(response.GetResponseStream()))
        {
            //Deserialize JSON string
            PBIReports Reports = JsonConvert.DeserializeObject<PBIReports>(reader.ReadToEnd());

            //Sample assumes at least one Report.
            //You could write an app that lists all Reports
            if (Reports.value.Length > 0)
            {
                var report = Reports.value[index];

                txtEmbedUrl.Text = report.embedUrl;
                txtReportId.Text = report.id;
                txtReportName.Text = report.name;
            }
        }
    }
}

//Power BI Reports used to deserialize the Get Reports response.
public class PBIReports
{
    public PBIReport[] value { get; set; }
}
public class PBIReport
{
    public string id { get; set; }
    public string name { get; set; }
    public string webUrl { get; set; }
    public string embedUrl { get; set; }
}
```

#### <a name="get-reports-using-the-net-sdk"></a>Hämta rapporter med .NET SDK
Du kan använda .NET SDK för att hämta en lista med rapporter i stället för att anropa REST API direkt. Här är ett kodexempel på hur du skapar en lista över rapporter.

```csharp
using Microsoft.IdentityModel.Clients.ActiveDirectory;
using Microsoft.PowerBI.Api.V2;
using Microsoft.PowerBI.Api.V2.Models;

var tokenCredentials = new TokenCredentials(<ACCESS TOKEN>, "Bearer");

// Create a Power BI Client object. It is used to call Power BI APIs.
using (var client = new PowerBIClient(new Uri(ApiUrl), tokenCredentials))
{
    // Get the first report all reports in that workspace
    ODataResponseListReport reports = client.Reports.GetReports();

    Report report = reports.Value.FirstOrDefault();

    var embedUrl = report.EmbedUrl;
}
```

### <a name="load-a-report-using-javascript"></a>Läs in en rapport med hjälp av JavaScript
Du kan använda JavaScript för att läsa in en rapport till olika element på webbsidan.

Här är ett kodexempel på hur du hämtar en rapport från en given arbetsyta.

*Ett exempel på hur du läser in ett innehållsobjekt, vare sig du vill bädda in en rapport, instrumentpanel eller panel, finns i filen **_Default.aspx_** i [exempelprogrammet](#embed-your-content-using-the-sample-application).*

```javascript
<!-- Embed Report-->
<div> 
    <asp:Panel ID="PanelEmbed" runat="server" Visible="true">
        <div>
            <div><b class="step">Step 3</b>: Embed a report</div>

            <div>Enter an embed url for a report from Step 2 (starts with https://):</div>
            <input type="text" id="tb_EmbedURL" style="width: 1024px;" />
            <br />
            <input type="button" id="bEmbedReportAction" value="Embed Report" />
        </div>

        <div id="reportContainer"></div>
    </asp:Panel>
</div>
```

**Site.master**

```javascript
window.onload = function () {
    // client side click to embed a selected report.
    var el = document.getElementById("bEmbedReportAction");
    if (el.addEventListener) {
        el.addEventListener("click", updateEmbedReporte, false);
    } else {
        el.attachEvent('onclick', updateEmbedReport);
    }

    // handle server side post backs, optimize for reload scenarios
    // show embedded report if all fields were filled in.
    var accessTokenElement = document.getElementById('MainContent_accessTokenTextbox');
    if (accessTokenElement !== null) {
        var accessToken = accessTokenElement.value;
        if (accessToken !== "")
            updateEmbedReport();
    }
};

// update embed report
function updateEmbedReport() {

    // check if the embed url was selected
    var embedUrl = document.getElementById('tb_EmbedURL').value;
    if (embedUrl === "")
        return;

    // get the access token.
    accessToken = document.getElementById('MainContent_accessTokenTextbox').value;

    // Embed configuration used to describe the what and how to embed.
    // This object is used when calling powerbi.embed.
    // You can find more information at https://github.com/Microsoft/PowerBI-JavaScript/wiki/Embed-Configuration-Details.
    var config = {
        type: 'report',
        accessToken: accessToken,
        embedUrl: embedUrl
    };

    // Grab the reference to the div HTML element that will host the report.
    var reportContainer = document.getElementById('reportContainer');

    // Embed the report and display it within the div container.
    var report = powerbi.embed(reportContainer, config);

    // report.on will add an event handler which prints to Log window.
    report.on("error", function (event) {
        var logView = document.getElementById('logView');
        logView.innerHTML = logView.innerHTML + "Error<br/>";
        logView.innerHTML = logView.innerHTML + JSON.stringify(event.detail, null, "  ") + "<br/>";
        logView.innerHTML = logView.innerHTML + "---------<br/>";
    }
  );
}
```

## <a name="using-a-power-bi-premium-dedicated-capacity"></a>Använda en dedikerad kapacitet med Power BI Premium

Nu när du är färdig med att utveckla ditt program är det dags att skapa dedikerad kapacitet för apparbetsytan.

### <a name="create-a-dedicated-capacity"></a>Skapa en dedikerad kapacitet
Genom att skapa en dedikerad kapacitet kan du dra nytta av att ha en dedikerad resurs för innehållet i din apps arbetsyta. Du kan skapa en dedikerad kapacitet med hjälp av [Power BI Premium ](../service-premium.md).

Följande tabell innehåller de tillgängliga Power BI Premium-SKU:erna i [Office 365](../service-admin-premium-purchase.md).

| Kapacitetsnod | Totalt antal virtuella kärnor<br/>*(Serverdel + klientdel)* | Virtuella kärnor för serverdel | Virtuella kärnor för klientdel | DirectQuery/begränsningar vid liveanslutning | Max sidåtergivningar vid högbelastning |
| --- | --- | --- | --- | --- | --- |
| EM1 |1 v-kärnor |5 virtuella kärnor, 10 GB RAM |0,5 virtuella kärnor |3,75 per sekund |150-300 |
| EM2 |2 v-kärnor |1 virtuell kärna, 10 GB RAM |1 v-kärnor |7,5 per sekund |301-600 |
| EM3 |4 v-kärnor |2 virtuella kärnor, 10 GB RAM-minne |2 v-kärnor |15 per sekund |601–1200 |
| P1 |8 v-kärnor |4 virtuella kärnor, 25 GB RAM-minne |4 v-kärnor |30 per sekund |1201–2400 |
| P2 |16 v-kärnor |8 virtuella kärnor, 50 GB RAM-minne |8 v-kärnor |60 per sekund |2401–4800 |
| P3 |32 v-kärnor |16 virtuella kärnor, 100 GB RAM-minne |16 v-kärnor |120 per sekund |4801–9600 |
| P4 |64 virtuella kärnor |32 virtuella kärnor, 200 GB RAM |32 v-kärnor |240 per sekund |9601-19200
| P5 |128 virtuella kärnor |64 virtuella kärnor, 400 GB RAM |64 virtuella kärnor |480 per sekund |19201-38400

*Med **_EM SKU:er_** **kan du** komma åt innehåll med en kostnadsfri Power BI-licens när du provar att bädda in med **_MS Office-appar_**, men **du kan inte komma åt** innehåll med en kostnadsfri Power BI-licens när du använder **_Powerbi.com_** eller **_Power BI mobile_**.*

*Med **_P SKU:er_** **kan du** komma åt innehåll med en kostnadsfri Power BI-licens när du provar att bädda in med **_MS Office-appar_**, när du använder **_Powerbi.com_** eller när du använder **_Power BI Mobile_**.*

### <a name="assign-an-app-workspace-to-a-dedicated-capacity"></a>Tilldela en apparbetsyta till en dedikerad kapacitet

När du har skapat en dedikerad kapacitet kan du tilldela apparbetsytan till den dedikerade kapaciteten. Gör så här för att slutföra detta:

1. I **Power BI-tjänsten** expanderar du arbetsytorna och väljer ellipsen för arbetsytan som du vill bädda in ditt innehåll med. Välj sedan **Redigera arbetsytor**.

    ![Redigera arbetsyta](media/embed-sample-for-your-organization/embed-sample-for-your-organization-036.png)

2. Expandera **Avancerat**, aktivera **Dedikerad kapacitet** och välj den dedikerade kapacitet du skapade. Välj sedan **Spara**.

    ![Tilldela dedikerad kapacitet](media/embed-sample-for-your-organization/embed-sample-for-your-organization-024.png)

3. När du har valt **Spara** bör du se en **romb** bredvid namnet på apparbetsytan.

    ![apparbetsyta som hör till en kapacitet](media/embed-sample-for-your-organization/embed-sample-for-your-organization-037.png)

## <a name="admin-settings"></a>Administratörsinställningar

Globala eller Power BI-tjänstadministratörer kan aktivera eller inaktivera REST API:er för en klient. Power BI-administratörer kan ange den här inställningen för hela organisationen eller för enskilda säkerhetsgrupper. Det har aktiverats för hela organisationen som standard. Du kan göra detta via [Power BI-administratörsportalen](../service-admin-portal.md).

## <a name="next-steps"></a>Nästa steg
I den här självstudien har du lärt dig hur du bäddar in Power BI-innehåll i ett program med hjälp av ditt **Power BI-organisationskonto**. Du kan nu prova att bädda in Power BI-innehåll i ett program med hjälp av appar.  Du kan även prova att bädda in Power BI-innehåll för dina kunder.

> [!div class="nextstepaction"]
> [Bädda in från appar](embed-from-apps.md)

> [!div class="nextstepaction"]
>[Bädda in för dina kunder](embed-sample-for-customers.md)

Har du fler frågor? [Fråga Power BI Community](http://community.powerbi.com/)