# SQL_portfolio
Requêtage d'une base de donnée sur Mysql 


## Shéma de la base donnée
![](https://github.com/Edercine/SQL_portfolio/blob/main/SQL%20schema%20relation%20de%20donn%C3%A9es.png)

/*******************************************************************************

Clients non américains : Fournissez une requête affichant les
Clients (leurs noms complets, ID client et pays) qui ne sont pas aux États-Unis (USA)

    
    SELECT CustomerId, LastName, Country
    
    FROM customer
    
    WHERE Country <> "USA";
    

/*******************************************************************************

Clients brésiliens : Fournissez une requête affichant uniquement
les Clients provenant du Brésil (Brazil)


    SELECT CustomerId, LastName, Country
    
    FROM customer
    
    WHERE Country LIKE "Brazil";


/*******************************************************************************

Factures des clients brésiliens : Fournissez une requête affichant
les factures des clients qui sont du Brésil (Brazil)



    SELECT customer.CustomerId, customer.LastName,InvoiceId, InvoiceDate, Country
    
    FROM invoice,customer
    
    WHERE invoice.InvoiceId = customer.CustomerId AND Country LIKE "Brazil";


/*******************************************************************************

Agents de vente : Fournissez une requête affichant uniquement
les employés qui sont des Agents de Vente



    SELECT * 
    
    FROM employee
    
    WHERE Title = "Sales Support Agent";


/*******************************************************************************

Pays uniques dans les factures : Fournissez une requête affichant
une liste unique des pays de facturation présents dans la table Invoice 


    SELECT Country, count(*) AS nb_factures FROM invoice
    
    INNER JOIN customer ON customer.CustomerId = invoice.InvoiceId
    
    GROUP BY Country
    
    ORDER BY nb_factures DESC;


/*******************************************************************************

Factures par agent de vente : Fournissez une requête affichant les
factures associées à chaque agent de vente


    SELECT InvoiceDate, BillingAddress, Total, employee.LastName, employee.FirstName, employee.Title 
    
    FROM employee, invoice
    
    INNER JOIN customer ON customer.CustomerId = invoice.InvoiceId
    
    WHERE Title = "Sales Support Agent";


/*******************************************************************************

Détails des factures : Fournissez une requête affichant le total de
chaque facture, le nom du client, le pays et le nom de l'agent de vente


    SELECT invoice.InvoiceId, invoice.Total, customer.LastName AS nom_client, customer.Country, employee.LastName AS nom_vendeur, employee.Title
    
    FROM invoice
    
    LEFT JOIN customer ON customer.CustomerId = invoice.InvoiceId
    
    LEFT JOIN employee ON employee.EmployeeId = customer.CustomerId
    
    WHERE employee.Title = "Sales Support Agent";

/*******************************************************************************

Ventes par année : Combien de factures y a-t-il eu en 2009 et
2011 ? Quels sont les montants totaux des ventes pour chacune de ces années ?


    SELECT YEAR(InvoiceDate) AS annee, count(*) AS nb_factures, sum(Total) AS total_ventes
    
    FROM invoice
    
    GROUP BY annee;

/*******************************************************************************

Articles pour une facture donnée : Fournissez une requête
comptant le nombre d'articles (line items) pour l'ID de facture 37.


    SELECT *
    
    FROM invoice
    
    WHERE CustomerId = '37';

/*******************************************************************************

Articles par facture : Fournissez une requête comptant le nombre
d'articles (line items) pour chaque facture 


    SELECT  CustomerId, count(CustomerId) AS nb_items
    
    FROM invoice
    
    GROUP BY CustomerId;

/*******************************************************************************

Nom des morceaux : Fournissez une requête incluant le nom du
morceau pour chaque ligne de facture

    
    SELECT InvoiceId, CustomerId, track.name AS nom_morceau
    
    FROM invoice
    
    LEFT JOIN track ON track.TrackId = invoice.InvoiceId 
    
    LEFT JOIN album ON album.AlbumId = track.AlbumId  
    
    ORDER BY CustomerId ASC;

/*******************************************************************************

Morceaux et artistes : Fournissez une requête incluant le nom du
morceau acheté ET le nom de l'artiste pour chaque ligne de facture 


    SELECT InvoiceId, CustomerId, track.name AS nom_morceau, artist.Name AS nom_artist
    
    FROM invoice
    
    LEFT JOIN track ON track.TrackId = invoice.InvoiceId 
    
    LEFT JOIN album ON album.AlbumId = track.AlbumId  
    
    LEFT JOIN artist ON artist.ArtistId = album.ArtistId
    
    ORDER BY CustomerId ASC;

/*******************************************************************************

Nombre de factures par pays : Fournissez une requête affichant
le nombre de factures par pays


    SELECT BillingCountry, COUNT(InvoiceId) AS nb_factures
    
    FROM invoice
    
    GROUP BY BillingCountry
    
    ORDER BY nb_factures DESC;

/*******************************************************************************

Nombre de morceaux par playlist : Fournissez une requête
affichant le nombre total de morceaux dans chaque playlist.
Le nom de la playlist doit être inclus dans le tableau résultant

    SELECT COUNT(track.Name) AS nb_titre, playlist.Name AS Playlist_name
    
    FROM track
    
    LEFT JOIN playlisttrack ON playlisttrack.TrackId = track.TrackId
    
    LEFT JOIN playlist ON playlist.playlistId = playlisttrack.playlistId
    
    GROUP BY Playlist_name
    
    ORDER BY nb_titre DESC;

/*******************************************************************************

Liste des morceaux : Fournissez une requête affichant tous les
morceaux (Tracks), mais sans afficher les IDs.
Le tableau résultant doit inclure le nom de l'album, le type de média et le genre


    SELECT track.Name AS morceaux, album.Title AS titre_album, genre.Name AS genre, mediatype.name AS media_type
    
    FROM track
    
    LEFT JOIN album ON album.AlbumId = track.AlbumId
    
    LEFT JOIN genre ON genre.GenreId = track.GenreId
    
    LEFT JOIN mediatype ON mediatype.MediaTypeId = track.MediaTypeId;

/*******************************************************************************

Factures et articles : Fournissez une requête affichant toutes les
factures, avec le nombre d'articles par facture


    SELECT invoice.InvoiceId, COUNT(invoiceline.InvoiceLineId) AS nb_articles
    
    FROM invoice, invoiceline
    
    WHERE invoice.InvoiceId = invoiceline.InvoiceId
    
    GROUP BY invoice.InvoiceId;

/*******************************************************************************

Ventes par agent de vente : Fournissez une requête affichant les
ventes totales réalisées par chaque agent de vente


    SELECT EmployeeId, Title, SUM(invoice.Total) AS Total_ventes
    
    FROM employee, customer, invoice
    
    WHERE employee.EmployeeId = customer.SupportRepId AND invoice.CustomerId = customer.CustomerId
    
    GROUP BY employee.EmployeeId;

/*******************************************************************************

Meilleur agent de 2009 : Quel agent de vente a réalisé le plus de
ventes en 2009 ?


    SELECT EmployeeId, Title, YEAR(invoice.InvoiceDate) AS annee, SUM(invoice.Total) AS total_vente
    
    FROM employee, customer, invoice
    
    WHERE employee.EmployeeId = customer.SupportRepId AND invoice.CustomerId = customer.CustomerId AND YEAR(invoice.InvoiceDate) = "2009"
    
    GROUP BY EmployeeId, annee;

/*******************************************************************************

Meilleur agent global : Quel agent de vente a réalisé le plus de
ventes en tout ?


    SELECT EmployeeId, employee.LastName, Title, SUM(invoice.Total) AS total_vente
    
    FROM employee, customer, invoice
    
    WHERE employee.EmployeeId = customer.SupportRepId AND invoice.CustomerId = customer.CustomerId
    
    GROUP BY EmployeeId
    
    ORDER BY total_vente DESC
    
    LIMIT 1;

/*******************************************************************************

Classement des ventes total des agents de vente par année


    SELECT EmployeeId, employee.LastName, Title,

		SUM(invoice.Total) AS total_vente,
  
		YEAR(invoice.InvoiceDate) AS annee,
  
		RANK() OVER( PARTITION BY YEAR(invoice.InvoiceDate) ORDER BY SUM(invoice.Total)) AS Ranking
  
    FROM employee, customer, invoice

    WHERE employee.EmployeeId = customer.SupportRepId AND invoice.CustomerId = customer.CustomerId

    GROUP BY EmployeeId, annee;

/*******************************************************************************

Avec la requete précédente récupérer unique les 2 meilleurs des agents de vente par année



    SELECT *

    FROM (SELECT EmployeeId, employee.LastName, Title,

		  SUM(invoice.Total) AS total_vente,
  
	  	YEAR(invoice.InvoiceDate) AS annee,
  
		  RANK() OVER( PARTITION BY YEAR(invoice.InvoiceDate) ORDER BY SUM(invoice.Total)) AS Ranking
  
  	FROM employee, customer, invoice
 
	  WHERE employee.EmployeeId = customer.SupportRepId AND invoice.CustomerId = customer.CustomerId
 
	  GROUP BY EmployeeId, annee) AS BETA
 
    WHERE BETA.Ranking <= 2;
