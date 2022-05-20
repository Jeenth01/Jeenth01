# Opdrachten Les 3 - Database

Het verbinden, bevragen, en updaten van een database kan relatief eenvoudig met Dapper.
Dapper wordt ook wel een Micro ORM genoemd.

* [Dapper Getting Started](https://dapper-tutorial.net/dapper)
* [Learn Dapper](https://www.learndapper.com/)

Tip: schrijf eerst je SQL query's en probeer deze uit, voordat je ze gebruik i.c.m. Dapper. Dan kun je al een hoop fouten voorkomen.
Tip: Als je in Rider (van JetBrains) verbinding maakt met je Database dan helpt Rider (ik heb geen aandelen Jetbrains :-)) je met code completion voor SQL en als je fouten hebt in je SQL-code laat hij dit ook zien, 
zelfs als de SQL tussen C# code staat. Visual Studio heeft bij mijn weten deze hulp (helaas) niet. Dus: test je SQL query eerst voordat je deze gebruikt.   
Tip: bekijk de voorbeelden (zie directory `Examples/Pages/Lesson3/`).

## Opdracht 1 NHLStenden Café - Drankenkaart (Product CRUD)

Het idee is om CRUD pagina's toe te voegen aan de eindopdracht (NHLStenden Café).

Er zijn al een aantal klassen aangemaakt (deze mag je gebruiken, hoeft niet per se), namelijk:  
- `Product` o.a. drankjes die besteld kunnen worden in het café.
- `Category` ieder drankje heeft 1 category, dus `Product` heeft een verwijzing (associatie/reference) naar `Category` (en de database dus een foreign key).  

Maak een database aan met de volgende naam: `NHLStendenCafe`, voer het `MysqlCafe.sql` script uit, deze staat in de directory `Database/MySQLCafe.SQL`.
Deze maakt tabellen `Product` en `Category` aan die respectievelijk overeenkomen met de klassen in de `Models` directory en vult de database met data.

- `DbUtils.GetDbConnection()` maakt verbinding met de Database, de connectionstring wordt door `Program.cs` opgehaald uit `appsettings.Development.json` en geplaatst deze in `DbUtils.ConnectionString`. 
Controleer of je connectionstring klopt met jouw situatie (mocht deze anders zijn)! 
  
Op [connectionstrings](https://www.connectionstrings.com/) kan je vinden hoe een connectiestring is opgebouwd.
Hierbij is het belangrijk te realiseren dat je verbinding maakt met een database m.b.v. een driver. De driver regelt de communicatie tussen de database en C# code.
Belangrijk om te realiseren is dat je vaak een .NET driver nodig bent om vanuit C# te werken met je database. 
Iedere driver heeft een specifieke connectiestring. Vaak is de opbouw te vinden in de documentatie van de driver die je gebruikt en op [connectionstrings](https://www.connectionstrings.com/).  

De driver die je nodig bent voor MySQL wordt normaal gesproken geïnstalleerd met NuGet, zie [NuGet - Mysql.Data](https://www.nuget.org/packages/MySql.Data/). Dit is al voor je gedaan.

In de directory `Pages\Categories` staan CRUD-pagina's (Read = Index.cshtml, Create = Create.cshtml, etc).
In de directory `Repository` staat de klasse `CategoryRepository`. 
Maak nu zelf een Repository voor `Product` genaamd `ProductRepository`. Zorg ervoor dat je de relatie tussen `Product` en `Category` kan aanmaken en aanpassen.
Maak daarvoor een nieuwe directory `Products` aan weaarin je de code zet van de pages zet. Tip: gebruik als inspiratie mijn voorbeeld van `Pages\Categories`.

De volgende pagina's dienen gerealiseerd te worden:
- `Index.cshtml` - Read - Dit is de drankenkaart (`Product`). Op deze pagina staat een `<h1>` per categorie (gesorteerd op naam), met daaronder een lijst (of tabel) met daarin de producten (gesoort op naam, prijs) van de desbetreffende categorie.
Stel we hebben N categorieën dan zouden we 1 query nodig hebben om de categorieën op te halen en 1 query per category (N query's). Dit is niet wenselijk omdat we dan te maken hebben met het N+1 probleem.
De oplossing is dat we gebruik maken van een join query. We hebben dan slecht 1 query nodig i.p.v. N+1 query's. Hoe je gebruik kan maken van relaties (joins) in Dapper wordt hier uitgelegd:
[Managing Relationships With Dapper](https://www.learndapper.com/relationships). Dit voorkomt het N+1 probleem.
Vanuit de drankenkaart moet je naar onderstaande pagina's kunnen navigeren, deze functionaliteit moet alleen beschikbaar zijn voor ingelogde obers.
- `Create.cshtml` - Gebruik een select list om de category aan een product te koppelen, zie [Select Lists in a Razor Pages Form](https://www.learnrazorpages.com/razor-pages/forms/select-lists).  
Gebruik model binding om te controleren of de input correct is, zie [Model Binding & Validation](https://www.learnrazorpages.com/razor-pages/validation).  
- `Update.cshtml` - Pas een bestaand `Product` aan.       
- `Delete.cshtml` - Verwijder een bestaand `Product`.   
Vraag of de gebruiker zeker weet dat hij het product wil verwijderen. Nadat een `Product` is verwijderd (of cancel) stuur je de gebruiker terug naar `Index.cshtml`.
