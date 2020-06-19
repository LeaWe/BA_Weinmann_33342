# BA (kommentierter Code)
## Legende
*g* = Gesamtnetzwerk  
*g2* = bereinigtes Netzwerk, alle gelöscht mit degree < 3  
*g3* = Elite in der Elite, alle gelöscht mit degree < 20  
*preistraegerfinal* = Preisträgernetzwerk  
  nach Jahren: *preistraeger2015, preistraeger2016,...*  
*juryfinal* = Jurynetzwerk  
   nach Jahren: *juryfinal2015, juryfinal2016,...*  
*ind-/outd-* = Präfix für In-/Outdegree-Netzwerke  
*preistraeger_xx* = Preisträgernetzwerk eines Teilnetzwerks  
*jury_xx* = Jurynetzwerk eines Teilnetzwerks  
*gruppenpreise2015, gruppenpreise2016...* = Teilnetzwerke mit Gruppenpreisen der einzelnen Jahre  
*einzelpreise2015,...* = Teilnetzwerke mit Einzelpreisen der einzelnen Jahre 

### Bezeichnungen der Preise  
Alternativer Medienpreis:  
```
id: alternativermedienpreis
name: "Alternativer Medienpreis"
```
Axel-Springer-Preis:  
```
id: axelspringerpreis 
name: "Axel-Springer-Preis für junge Journalisten"
```
Bremer Fernsehpreis:  
```
id: bremerfernsehpreis
name: "Bremer Fernsehpreis"
```
Deutscher Journalistenpreis Wirtschaft:  
```
id: djp  
name: "Deutscher Journalistenpreis Wirtschaft | Börse | Finanzen (djp)"
```
Deutscher Radiopreis:  
```
id: deutscherradiopreis
name: "Deutscher Radiopreis"
```
Deutscher Reporterpreis:  
```
id: deutscherreporterpreis
name: "Deutscher Reporterpreis"
```
Ernst-Schneider-Preis:  
```
id: ernstschneiderpreis 
name: "Ernst-Schneider-Preis"
```
Georg v- Holtzbrinck-Preis:  
```
id: gvhpreis
name: "Georg von Holtzbrinck-Preis für Wirtschaftspublizistik bzw. Ferdinand Simoneit-Nachwuchspreis"
```
Grimme Online Award:  
```
id: grimmeonlineaward
name: "Grimme Online Award"
```
Helmut-Schmidt-Preis:  
```
id: helmutschmidtpreis
name: "Helmut-Schmidt-Journalistenpreis"
```
Herbert-Quandt-Preis:  
```
id: herbertquandtpreis
name: "Herbert Quandt Medienpreis"
```
Journalist des Jahres:  
```
id: journalistdesjahres
name: "Journalist des Jahres"
```
Katholischer Medienpreis:  
```
id: katholischermedienpreis
name: "Katholischer Medienpreis"
```
Kurt-Tucholsky-Preis:  
```
id: kurttucholskypreis
name: "Kurt-Tucholsky-Preis für literarische Publizistik"
```
Leuchtturm:  
```
id: leuchtturm
name: "Leuchtturm für besondere publizistische Leistungen"
```
Ludwig-Börne-Preis:  
```
id: ludwigboernepreis
name: "Ludwig-Börne-Preis"
```
Ludwig-Erhard-Preis:  
```
id: ludwigerhardpreis
name: "Ludwig-Erhard-Preis für Wirtschaftspublizistik"
```
Nannenpreis:  
```
id: nannenpreis
name: "Nannen Preis"
```
Robert-Geisendörfer-Preis: 
```
id: robertgeisendoerferpreis
name: "Robert Geisendörfer Preis"
```
Theodor-Wolff-Preis:  
```
id: theodorwolffpreis
name: "Theodor-Wolff-Preis"
```
Wächterpreis:  
```
id: waechterpreis 
name: "Wächterpreis der Tagespresse"
```
Standard-Visualisierung zum Kopieren:
```
plot(xx, edge.arrow.size=.1, edge.label.degree=0, vertex.frame.color="white", vertex.label.family="Helvetica", vertex.label.dist=0.5, vertex.label.cex=.6, layout = layout_with_kk)
```

## Netzwerk laden
```
library(igraph)
el <- read.csv("el.csv", header = TRUE, as.is=T, sep = ";")
nl <- read.csv("nl.csv", header = TRUE, as.is=T, sep = ";")
is.data.frame(el)
is.data.frame(nl)
g <- graph.data.frame(el, directed=TRUE, vertices=nl)
g
plot(g)
head(el)
head(nl)
```

## Grundsätzliche Darstellung
```
options(max.print = 99999)
E(g)$curved=.2 
E(g)$vertex.label.family="Helvetica"
```

## Darstellung von Personen und Institutionen, bi-partite

Färbt edges unterschiedlicher types verschieden ein & gib ihnen unterscheidbare Formen
```
V(g)[V(g)$type == 1]$shape <- "square"
V(g)[V(g)$type == 0]$shape <- "circle"
V(g)[V(g)$type == 0]$color <- "cornflowerblue"
V(g)[V(g)$type == 1]$color <- "aquamarine2"
```

Färbt verschiedene relations unterschiedlich ein
```
E(g)[relation == 3]$color <- "green"
E(g)[relation == 2]$color <- "orange"
E(g)[relation == 1]$color <- "blue"

plot(g)
```

Zeigt Attribute des Netzwerks an
```
E(g)
V(g)
edge_attr(g)
vertex_attr(g)
```

## Beschreibung Gesamtnetzwerk

Zeigt Attribute an, die "institutiontype" = 1 haben (= Medienunternehmen)
```
vertex_attr(g, ("institutiontype" == 1), index=V(g))
```
Erzeugt Teilnetzwerk mit ausschließlich Preisen (Frage: Wie viele Preise im Gesamtnetzwerk?)
```
preise <- induced_subgraph(g, V(g)[which (institutiontype == 1)])
preise
```
Erzeugt Teilnetzwerk mit ausschließlich Medien (Frage: Wie viele Medien im Gesamtnetzwerk?)
```
medien <- induced_subgraph(g, V(g)[which (institutiontype == 2)])
medien
```
Erzeugt Teilnetzwerk mit ausschließlich Personen (Frage: Wie viele Personen im Gesamtnetzwerk?)
```
personen <- induced_subgraph(g, V(g)[which (type == 0)])
personen
```
Erzeugt Teilnetzwerk mit ausschließlich Preisträgern (Frage: Wie viele Preisträger im Gesamtnetzwerk?)
```
preistraeger <- induced_subgraph(personen, V(personen)[which (is.na(workinmedia))])
preistraeger
```
Erzeugt Teilnetzwerk mit ausschließlich Juroren (Frage: Wie viele Juroren im Gesamtnetzwerk?)
```
juroren <- induced_subgraph(personen, V(personen)[which (!is.na(workinmedia))])
juroren
```
Erzeugt Teilnetzwerk mit ausschließlich Männern (Frage: Wie viele Männer im Gesamtnetzwerk?)
```
maenner <- induced_subgraph(personen, V(personen)[which (sex == 1)])
maenner
```
Erzeugt Teilnetzwerk mit ausschließlich Frauen (Frage: Wie viele Frauen im Gesamtnetzwerk?)
```
frauen <- induced_subgraph(personen, V(personen)[which (sex == 2)])
frauen
```
Erzeugt Teilnetzwerk mit ausschließlich Preisverleihungen (Frage: Wie viele Preisverleihungen an Einzelpersonen gab es?)
```
verleihung1 <- delete_edges(g, E(g)[relation == 3])
verleihung <- delete_edges(verleihung1, E(verleihung1)[relation == 2])
verleihung
```

## Netzwerkmaße

**Out-Degree im Gesamtnetzwerk**
```
outd <- degree(g, mode="out")
outd
sort (outd)
```

**In-Degree im Gesamtnetzwerk**
```
ind <- degree(g, mode="in")
ind
sort(ind)
```

**andere Netzwerkmaße**
```
edge_density(g) # Dichte
centr_betw(g, directed=TRUE) # Betweenness-Zentralität
betw <- betweenness(g, directed=TRUE) # Betweenness
bet <- betweenness(g, directed = TRUE)

```
## Preisträger-Netzwerk

1. Entfernt alle Nodes, die bei "workinmedia" _nicht_ NA haben (trifft auf alle Nodes außer Jurymitgliedern zu)
```
preistraeger1 <- delete_vertices(g, V(g)[!is.na(workinmedia)])
preistraeger1
```
2. Entfernt aus oberem Netzwerk alle Jury-Beziehungen ("relation" = 3)
```
preistraeger2 <- delete_edges (preistraeger1, E(preistraeger1)[relation==3])
preistraeger2
```
3. Entfernt aus oberem Netzwerk alle Institutionen, die keine Verbindung zu Preisträgern haben (Degree-Wert = 0)
```
preistraegerfinal <- delete_vertices (preistraeger2, V(preistraeger2)[degree(preistraeger2, mode="all")=="0"])
preistraegerfinal
```
4. Wirft das Preisträgernetzwerk aus
```
plot (preistraegerfinal, edge.arrow.size=.1, edge.label.degree=0, vertex.frame.color="white", vertex.label.family="Helvetica", vertex.label.dist=0.5, vertex.label.cex=.6, layout = layout_with_kk)
```
Berechnet Out-Degree von Preisträgernetzwerk 
```
outdpreistraeger <- degree(preistraegerfinal, mode="out")
outdpreistraeger
sort(outdpreistraeger)
```
Berechnet In-Degree von Preisträgernetzwerk 
```
indpreistraeger <- degree(preistraegerfinal, mode="in")
indpreistraeger
sort(indpreistraeger)
```
## Jury-Netzwerk

1. Entfernt alle Preisträger-Beziehungen aus edgelist
```
jury1 <- delete_edges (g, E(g)[relation==1])
jury1
```
*V(jury1)$workinmedia=="1"
V(jury1)$workinmedia=="2"*

2. Entfernt aus oberem Netzwerk alle Personen (type = 0), die bei "workinmedia" = NA haben (also alle Preisträger)
```
jury2 <- delete_vertices(jury1, V(jury1)[(is.na(workinmedia)) & (type == 0)])
jury2
```
3. Entfernt aus oberem Netzwerk alle Institutionen, die keine Verbindung zu Jurymitgliedern haben (Degree-Wert = 0)
```
juryfinal <- delete_vertices (jury2, V(jury2)[degree(jury2, mode="all")=="0"])
juryfinal
```
4. Wirft das Jurynetzwerk aus
```
plot (juryfinal, edge.arrow.size=.1, edge.label.degree=0, vertex.frame.color="white", vertex.label.family="Helvetica", vertex.label.dist=0.5, vertex.label.cex=.6, layout = layout_with_kk)
```
Berechnet Out-Degree von Jurynetzwerk
```
outdjury <- degree(juryfinal, mode="out")
outdjury
sort(outdjury)
```
Berechnet In-Degree von Jurynetzwerk
```
indjury <- degree(juryfinal, mode="in")
indjury
sort(indjury)
```

## Teilnetzwerke

Schritt 1: **Bereinigtes Netzwerk** (Entferne alle Nodes mit degree < 3)
```
g2_1 <- delete_vertices (g, V(g)[(type == 0) &(degree(g, mode="out")<3)])
g2 <- delete_vertices (g2_1, V(g2_1)[degree(g2_1, mode="all")=="0"])
g2
plot(g2)
```

Schritt 2: **Elite in Elite** (Entferne alle Nodes mit degree < 20)
## Elite in Elite
```
g3_1 <- delete_vertices (g, V(g)[(type == 0) &(degree(g, mode="out")<20)])
g3 <- delete_vertices (g3_1, V(g3_1)[degree(g3_1, mode="all")=="0"])
g3
plot(g3)
```

## Einzelne Preisnetzwerke

Erstellt Teilnetzwerke der einzelnen Preise

### Preisauswahl
Zuweisung des einzelnen Preisnamen (Bsp. "Alternativer Medienpreis", siehe Legende), dann run der Befehle

```
name <- "Alternativer Medienpreis"
```
Erzeugt Preisnetzwerk 1. Grades
```
x <- subgraph <- make_ego_graph(g, order=1, name)
x
```
Erzeugt Preisnetzwerk 2. Grades (mit Arbeitgebern und anderen Preisen)
```
x2 <- subgraph <- make_ego_graph(g, order=2, name)
x2
```
Einfacher Plot des Preisnetzwerks
```
plot(x[[1]],
     edge.arrow.size=.02,
     edge.label.degree=0.1,
     vertex.frame.color="white",
     vertex.label.family="Helvetica",
     vertex.label.dist=0.5,
     vertex.label.cex=.6,
     layout = layout_with_kk)
```
Einfacher Plot des Netzwerks 2. Grades (mit Arbeitgebern)
```
plot(x2[[1]],
     edge.arrow.size=.02,
     edge.label.degree=0.1,
     vertex.frame.color="white",
     vertex.label.family="Helvetica",
     vertex.label.dist=0.5,
     vertex.label.cex=.6,
     layout = layout_with_kk)
```
Alle Preise aus Netzwerk 2. Grades löschen (**op = ohne Preise**), nur noch Arbeitsverhältnisse
```
x2_oP <- delete_vertices(x2[[1]], V(x2[[1]])[which (institutiontype == 1)])
plot(x2_oP,
     edge.arrow.size=.02,
     edge.label.degree=0.1,
     vertex.frame.color="white",
     vertex.label.family="Helvetica",
     vertex.label.dist=0.5,
     vertex.label.cex=.6,
     layout = layout_with_kk)
```
Nach **Indegree** sortieren (Welcher Arbeitgeber taucht am Häufigsten auf?)
```
ind_x2_oP <- degree(x2_oP, mode="in")
sort(ind_x2_oP)
```

### Umfang Preisnetzwerk
Anzahl Jurymitglieder im Preisnetzwerk
```
jury_x <- E(x[[1]])$relation == 3
sum(jury_x, na.rm = TRUE)
```
Anzahl Preisträger im Preisnetzwerk
```
preistraeger_x <- E(x[[1]])$relation == 1
sum(preistraeger_x, na.rm = TRUE)
```
Wie viele Personen gibt es (jede/r nur einfach gezählt) im Preisnetzwerk?
```
personen_x <- V(x[[1]])$type == 0
sum(personen_x, na.rm = TRUE)
```
Anzahl Gruppenpreise im Preisnetzwerk
```
gruppenpreise_x <- (E(x[[1]])$format == 2)
sum(gruppenpreise_x, na.rm = TRUE)
```

Wie viele **Beziehungen zum Preis** (über Jurymitgliedschaft oder Preisträgerschaft) gibt es im Preisnetzwerk?
```
beziehungen_x <- (E(x[[1]])$relation == 1) + (E(x[[1]])$relation == 3)
sum(beziehungen_x, na.rm = TRUE)
```
Wie viele **Arbeitgeberbeziehungen** gibt es im Preisnetzwerk?
```
ag_x <- (E(x2[[1]])$relation == 2)
sum(ag_x, na.rm = TRUE)
```

### Preisträgernetzwerke einzelner Preise

Lösche alle Jurymitglieder
```
x2_pt <- delete_vertices(x2[[1]], V(x2[[1]])[!is.na(workinmedia)])
x2_pt
```
Anzahl Arbeitgeberbeziehungen allein unter Preisträgern
```
ag_x_pt <- (E(x2_pt)$relation == 2)
sum(ag_x_pt, na.rm = TRUE)
```
Ergebnis nach **Indegree** sortieren
```
ind_x_pt <- degree(x2_pt, mode="in")
sort(ind_x_pt)
```

### Männer-/Frauenverteilung
Anzahl Frauen gesamt
```
frauen_x <- delete.vertices(x[[1]], V(x[[1]])[which (sex == 1)])
gsize(frauen_x)
```
Anzahl Frauen in der Jury
```
frauen_jury_x <- E(frauen_x)$relation == 3
sum(frauen_jury_x, na.rm = TRUE)
```
Anzahl Frauen unter den Preisträgern
```
frauen_preistraeger_x <- E(frauen_x)$relation == 1	
sum(frauen_preistraeger_x, na.rm = TRUE)
```
### Alternativer Medienpreis
Gesamt
```
alternativermedienpreis <- subgraph <- make_ego_graph(g, order=1, c("Alternativer Medienpreis"))
alternativermedienpreis
plot(alternativermedienpreis[[1]])
```
Jury
```
jury_alternativermedienpreis <- E(alternativermedienpreis[[1]])$relation == 3
sum(jury_alternativermedienpreis, na.rm = TRUE)
```
Preisträger
```
preistraeger_alternativermedienpreis <- E(alternativermedienpreis[[1]])$relation == 1
sum(preistraeger_alternativermedienpreis, na.rm = TRUE)
```

**Medienkonzentration unter einzelnen Preisen**
Wie viele Personen gibt es (einfach gezählt) im Preisnetzwerk?
```
personen_alternativermedienpreis <- V(alternativermedienpreis[[1]])$type == 0
sum(personen_alternativermedienpreis, na.rm = TRUE)
```
Wie viele Beziehungen zum Preis (über Jurymitgliedschaft oder Preisträgerschaft) gibt es im Preisnetzwerk?
```
beziehungen_alternativermedienpreis <- (E(alternativermedienpreis[[1]])$relation == 1) + (E(alternativermedienpreis[[1]])$relation == 3)
sum(beziehungen_alternativermedienpreis, na.rm = TRUE)
```
Wie viele Gruppenpreisverleihungen gibt es im Preisnetzwerk (um _Gruppenpreis-Verzerrung_ zu erkennen)?
```
gruppenpreise_alternativermedienpreis <- (E(alternativermedienpreis[[1]])$format == 2)
sum(gruppenpreise_alternativermedienpreis, na.rm = TRUE)
```
Brauche Preisnetzwerk 2. Grades (damit Arbeitgeber auch im Netzwerk sind)
```
alternativermedienpreis_2grad <- subgraph <- make_ego_graph(g, order=2, c("Alternativer Medienpreis"))
alternativermedienpreis_2grad
plot(alternativermedienpreis_2grad[[1]])
```
Alle Beziehungen zu Preisen rauslöschen (institutiontype = 1)
```
alternativermedienpreis_2grad_oP <- delete_vertices(alternativermedienpreis_2grad[[1]], V(alternativermedienpreis_2grad[[1]])[which (institutiontype == 1)])
plot(alternativermedienpreis_2grad_oP)
```
Nach Indegree sortieren:
```
ind_alternativermedienpreis_2grad_oP <- degree(alternativermedienpreis_2grad_oP, mode="in")
sort(ind_alternativermedienpreis_2grad_oP)
```
Zähle und summiere alle Arbeitgeberberziehungen (relation = 2) im Preisnetzwerk
```
ag_alternativermedienpreis <- (E(alternativermedienpreis_2grad[[1]])$relation == 2)
sum(ag_alternativermedienpreis, na.rm = TRUE)
```
Konzentration unter Preisträgern (Jurymitglieder (workinmedia != NA) löschen) im Preisnetzwerk
```
alternativermedienpreis_2grad_pt <- delete_vertices(alternativermedienpreis_2grad[[1]], V(alternativermedienpreis_2grad[[1]])[!is.na(workinmedia)])
alternativermedienpreis_2grad_pt
```
Zähle alle Arbeitgeberbeziehungen unter _Preisträgern_ im Preisnetzwerk
```
ag_alternativermedienpreis_pt <- (E(alternativermedienpreis_2grad_pt)$relation == 2)
sum(ag_alternativermedienpreis_pt, na.rm = TRUE)
```
Nach Indegree sortieren:
```
ind_alternativermedienpreis_pt <- degree(alternativermedienpreis_2grad_pt, mode="in")
sort(ind_alternativermedienpreis_pt)
```

**Männer-/Frauenverteilung nach Preisen (Wie viele Frauen waren vertreten?)** 
  1. Zähle, wie oft eine Frau am Preis beteiligt war (Preisträger & Jury, einzelne Personen werden mehrfach gezählt)	
```
frauen_alternativermedienpreis <- delete_vertices(alternativermedienpreis, V(alternativermedienpreis)[which (sex == 1)])
gsize(frauen_alternativermedienpreis)
```

2. Zähle alle Frauen, die in der Jury saßen (einzelne Personen werden mehrfach gezählt)
```
frauen_jury_alternativermedienpreis <- E(frauen_alternativermedienpreis)$relation == 3
sum(frauen_jury_alternativermedienpreis, na.rm = TRUE)
```

3. Zähle alle Frauen, die einen Preis gewonnen haben (einzelne Personen werden mehrfach gezählt)
```
frauen_preistraeger_alternativermedienpreis <- E(frauen_alternativermedienpreis)$relation == 1	
sum(frauen_preistraeger_alternativermedienpreis, na.rm = TRUE)
```


### Axel-Springer-Preis
Gesamt


## Auswertung nach Jahren

### Gesamt

#### 2019
Erzeugt Teilnetzwerk mit allen edges aus dem Jahr 2019
```
year2019 <- subgraph.edges(g, E(g)[year == "2019"]) 
year2019
plot (year2019)
indyear2019 <- 
```

#### 2018
Erzeugt Teilnetzwerk mit allen edges aus dem Jahr 2018
```
year2018 <- subgraph.edges(g, E(g)[year == "2018"]) 
year2018
plot (year2018)
```

#### 2017
Erzeugt Teilnetzwerk mit allen edges aus dem Jahr 2017
```
year2017 <- subgraph.edges(g, E(g)[year == "2017"]) 
year2017
plot (year2017)
```

#### 2016
Erzeugt Teilnetzwerk mit allen edges aus dem Jahr 2016
```
year2016 <- subgraph.edges(g, E(g)[year == "2016"]) 
year2016
plot (year2016)
```

#### 2015
Erzeugt Teilnetzwerk mit allen edges aus dem Jahr 2015
```
year2015 <- subgraph.edges(g, E(g)[year == "2015"]) 
year2015
plot (year2015)
```

### Gruppenpreise
Wirft alle edges aus, die eine Gruppenpreis-Verleihung im Jahr 2015 erfasst haben
```
gruppenpreise2015_einzeln <- E(year2015)$format == 2
gruppenpreise2015_einzeln
```
Summiert alle Gruppenpreisverleihungen, die es im Jahr 2015 gab
```
sum(gruppenpreise2015_einzeln, na.rm = TRUE)
```
Für 2016:
```
gruppenpreise2016_einzeln <- E(year2016)$format == 2
gruppenpreise2016_einzeln
sum(gruppenpreise2016_einzeln, na.rm = TRUE)
```
Für 2017:
```
gruppenpreise2017_einzeln <- E(year2017)$format == 2
gruppenpreise2017_einzeln
sum(gruppenpreise2017_einzeln, na.rm = TRUE)
```
Für 2018:
```
gruppenpreise2018_einzeln <- E(year2018)$format == 2
gruppenpreise2018_einzeln
sum(gruppenpreise2018_einzeln, na.rm = TRUE)
```
Für 2019:
```
gruppenpreise2019_einzeln <- E(year2019)$format == 2
gruppenpreise2019_einzeln
sum(gruppenpreise2019_einzeln, na.rm = TRUE)
```

### Einzelpreise
Wirft alle edges aus, die eine Einzelpreis-Verleihung im Jahr 2015 erfasst haben
```
einzelpreise2015_einzeln <- E(year2015)$format == 1
einzelpreise2015_einzeln
```
Summiert alle Einzelpreisverleihungen, die es im Jahr 2015 gab
```
sum(einzelpreise2015_einzeln, na.rm = TRUE)
```
Für 2016:
```
einzelpreise2016_einzeln <- E(year2016)$format == 1
einzelpreise2016_einzeln
sum(einzelpreise2016_einzeln, na.rm = TRUE)
```
Für 2017:
```
einzelpreise2017_einzeln <- E(year2017)$format == 1
einzelpreise2017_einzeln
sum(einzelpreise2017_einzeln, na.rm = TRUE)
```
Für 2018:
```
einzelpreise2018_einzeln <- E(year2018)$format == 1
einzelpreise2018_einzeln
sum(einzelpreise2018_einzeln, na.rm = TRUE)
```
Für 2019:
```
einzelpreise2019_einzeln <- E(year2019)$format == 1
einzelpreise2019_einzeln
sum(einzelpreise2019_einzeln, na.rm = TRUE)
```

### Männer/Frauen
Teilnetzwerk, nur männliche Nodes im Jurynetzwerk (und ihre Summe)
```
maennerjury <- V(juryfinal)$sex == 1
sum (maennerjury, na.rm = TRUE)
```
Teilnetzwerk, nur weibliche Nodes im Jurynetzwerk (und ihre Summe)
```
frauenjury <- V(juryfinal)$sex == 2
sum (frauenjury, na.rm = TRUE)
```

#### ...gesamt
Teilnetzwerk, nur männliche Nodes im Gesamtnetzwerk 2015 (und ihre Summe):
```
maenner2015 <- V(year2015)$sex == 1
sum (maenner2015, na.rm = TRUE)
```
Teilnetzwerk, nur männliche Nodes im Gesamtnetzwerk 2016 (und ihre Summe):
```
maenner2016 <- V(year2016)$sex == 1
sum (maenner2016, na.rm = TRUE)
```
Teilnetzwerk, nur männliche Nodes im Gesamtnetzwerk 2017 (und ihre Summe):
```
maenner2017<- V(year2017)$sex == 1
sum (maenner2017, na.rm = TRUE)
```
Teilnetzwerk, nur männliche Nodes im Gesamtnetzwerk 2018 (und ihre Summe):
```
maenner2018 <- V(year2018)$sex == 1
sum (maenner2018, na.rm = TRUE)
```
Teilnetzwerk, nur männliche Nodes im Gesamtnetzwerk 2019 (und ihre Summe):
```
maenner2019 <- V(year2019)$sex == 1
sum (maenner2019, na.rm = TRUE)
```

Teilnetzwerk, nur weibliche Nodes im Gesamtnetzwerk 2015 (und ihre Summe):
```
frauen2015 <- V(year2015)$sex == 2
sum (frauen2015, na.rm = TRUE)
```
Teilnetzwerk, nur weibliche Nodes im Gesamtnetzwerk 2016 (und ihre Summe):
```
frauen2016 <- V(year2016)$sex == 2
sum (frauen2016, na.rm = TRUE)
```
Teilnetzwerk, nur weibliche Nodes im Gesamtnetzwerk 2017 (und ihre Summe):
```
frauen2017 <- V(year2017)$sex == 2
sum (frauen2017, na.rm = TRUE)
```
Teilnetzwerk, nur weibliche Nodes im Gesamtnetzwerk 2018 (und ihre Summe):
```
frauen2018 <- V(year2018)$sex == 2
sum (frauen2018, na.rm = TRUE)
```
Teilnetzwerk, nur weibliche Nodes im Gesamtnetzwerk 2019 (und ihre Summe):
```
frauen2019 <- V(year2019)$sex == 2
sum (frauen2019, na.rm = TRUE)
```

#### ...in Jury
**2015:**
1. Erstelle Teilnetzwerk aus Jurynetzwerk, ausschließlich mit Verbindungen aus 2015
```
juryfinal2015 <- subgraph.edges(juryfinal, E(juryfinal)[year == "2015"])
```
2. Filtere alle Frauen aus dem oberen Netzwerk
```
frauenjury2015 <- V(juryfinal2015)$sex == 2
```
3. Summiere alle Frauen aus dem Netzwerk
```
sum (frauenjury2015, na.rm = TRUE)
```
4. Filtere alle Männer aus dem Netzwerk und summiere alle Männer
```
maennerjury2015 <- V(juryfinal2015)$sex == 1
sum (maennerjury2015, na.rm = TRUE)
```
**2016:**
```
juryfinal2016 <- subgraph.edges(juryfinal, E(juryfinal)[year == "2016"])
frauenjury2016 <- V(juryfinal2016)$sex == 2
sum (frauenjury2016, na.rm = TRUE)
maennerjury2016 <- V(juryfinal2016)$sex == 1
sum (maennerjury2016, na.rm = TRUE)
```
**2017:**
```
juryfinal2017 <- subgraph.edges(juryfinal, E(juryfinal)[year == "2017"])
frauenjury2017 <- V(juryfinal2017)$sex == 2
sum (frauenjury2017, na.rm = TRUE)
maennerjury2017 <- V(juryfinal2017)$sex == 1
sum (maennerjury2017, na.rm = TRUE)
```
**2018:**
```
juryfinal2018 <- subgraph.edges(juryfinal, E(juryfinal)[year == "2018"])
frauenjury2018 <- V(juryfinal2018)$sex == 2
sum (frauenjury2018, na.rm = TRUE)
maennerjury2018 <- V(juryfinal2018)$sex == 1
sum (maennerjury2018, na.rm = TRUE)
```
**2019:**
```
juryfinal2019 <- subgraph.edges(juryfinal, E(juryfinal)[year == "2019"])
frauenjury2019 <- V(juryfinal2019)$sex == 2
sum (frauenjury2019, na.rm = TRUE)
maennerjury2019 <- V(juryfinal2019)$sex == 1
sum (maennerjury2019, na.rm = TRUE)
```

#### ...unter Preisträgern
1. Erstelle Teilnetzwerk um alle Preisträger des Jahres **2015**
```
preistraeger2015 <- subgraph.edges(preistraegerfinal, E(preistraegerfinal)[year == "2015"])
```
2. Filtere alle weiblichen Nodes heraus
```
frauenpreistraeger2015 <- V(preistraeger2015)$sex == 2
```
3. Summiere alle weiblichen Nodes
```
sum (frauenpreistraeger2015, na.rm = TRUE)
```
4. Ebenso für alle männlichen Nodes
```
maennerpreistraeger2015 <- V(preistraeger2015)$sex == 1
sum (maennerpreistraeger2015, na.rm = TRUE)
```

Für **2016**:
```
preistraeger2016 <- subgraph.edges(preistraegerfinal, E(preistraegerfinal)[year == "2016"])
frauenpreistraeger2016 <- V(preistraeger2016)$sex == 2
preistraeger2016
sum (frauenpreistraeger2016, na.rm = TRUE)
maennerpreistraeger2016 <- V(preistraeger2016)$sex == 1
sum (maennerpreistraeger2016, na.rm = TRUE)
```
Für **2017**:
```
preistraeger2017 <- subgraph.edges(preistraegerfinal, E(preistraegerfinal)[year == "2017"])
frauenpreistraeger2017 <- V(preistraeger2017)$sex == 2
sum (frauenpreistraeger2017, na.rm = TRUE)
maennerpreistraeger2017 <- V(preistraeger2017)$sex == 1
sum (maennerpreistraeger2017, na.rm = TRUE)
```
Für **2018**:
```
preistraeger2018 <- subgraph.edges(preistraegerfinal, E(preistraegerfinal)[year == "2018"])
frauenpreistraeger2018 <- V(preistraeger2018)$sex == 2
sum (frauenpreistraeger2018, na.rm = TRUE)
maennerpreistraeger2018 <- V(preistraeger2018)$sex == 1
sum (maennerpreistraeger2018, na.rm = TRUE)
```
Für **2019**:
```
preistraeger2019 <- subgraph.edges(preistraegerfinal, E(preistraegerfinal)[year == "2019"])
frauenpreistraeger2019 <- V(preistraeger2019)$sex == 2
sum (frauenpreistraeger2019, na.rm = TRUE)
maennerpreistraeger2019 <- V(preistraeger2019)$sex == 1
sum (maennerpreistraeger2019, na.rm = TRUE)
```

### Dominanz einzelner Medienunternehmen nach Jahren

**Wer (Personen) hat die meisten Preise abgeräumt (nach Jahren)? --> daraus auch: Dominanz der Medien unter Personen, die 2 oder mehr Preise gewonnen haben.**

Für **2015**:
```
preistraeger2015 <- subgraph.edges(g, E(g)[year == "2015"]) 
nur_preistraeger2015 <- subgraph.edges(preistraeger2015, E(preistraeger2015)[relation == 1]) 
```
weil: Wenn man Outdegree von gesamtem Preisträgernetzwerk sortiert, werden alle Preisträger, die mehrere Arbetsverhältnisse haben, auch doppelt und dreifach gezählt --> will nur Anzahl der Preis-Relations!! 
```
outd_nur_preistraeger2015 <- degree(nur_preistraeger2015, mode="out")
sort(outd_nur_preistraeger2015)
```
Für **2016**:
```
preistraeger2016 <- subgraph.edges(g, E(g)[year == "2016"]) 
nur_preistraeger2016 <- subgraph.edges(preistraeger2016, E(preistraeger2016)[relation == 1])

outd_nur_preistraeger2016 <- degree(nur_preistraeger2016, mode="out")
sort(outd_nur_preistraeger2016)
```
Für **2017**:
```
preistraeger2017 <- subgraph.edges(g, E(g)[year == "2017"]) 
nur_preistraeger2017 <- subgraph.edges(preistraeger2017, E(preistraeger2017)[relation == 1])
outd_nur_preistraeger2017 <- degree(nur_preistraeger2017, mode="out")
sort(outd_nur_preistraeger2017)
```
Für **2018**:
```
preistraeger2018 <- subgraph.edges(g, E(g)[year == "2018"]) 
nur_preistraeger2018 <- subgraph.edges(preistraeger2018, E(preistraeger2018)[relation == 1])
outd_nur_preistraeger2018 <- degree(nur_preistraeger2018, mode="out")
sort(outd_nur_preistraeger2018)
```
Für **2019**:
```
preistraeger2019 <- subgraph.edges(g, E(g)[year == "2019"]) 
nur_preistraeger2019 <- subgraph.edges(preistraeger2019, E(preistraeger2019)[relation == 1])
outd_nur_preistraeger2019 <- degree(nur_preistraeger2019, mode="out")
sort(outd_nur_preistraeger2019)
```

#### Wer (Medienunternehmen) beschäftigt die meisten Preisträger und Jurymitglieder (nach Jahren)?

**größter Indegree (gesamt, nach Jahren)**
```
ind2015 <- degree(year2015, mode="in")
sort(ind2015)

ind2016 <- degree(year2016, mode="in")
sort(ind2016)

ind2017 <- degree(year2017, mode="in")
sort(ind2017)

ind2018 <- degree(year2018, mode="in")
sort(ind2018)

ind2019 <- degree(year2019, mode="in")
sort(ind2019)
```

**größter Indegree (unter PT, nach Jahren)**
```
indpreistraeger2015 <- degree(preistraeger2015, mode="in")
sort(indpreistraeger2015)

indpreistraeger2016 <- degree(preistraeger2016, mode="in")
sort(indpreistraeger2016)

indpreistraeger2017 <- degree(year2017, mode="in")
sort(indpreistraeger2017)

indpreistraeger2018 <- degree(preistraeger2018, mode="in")
sort(indpreistraeger2018)

indpreistraeger2019 <- degree(preistraeger2019, mode="in")
sort(indpreistraeger2019)
```

_Zusatz: Indegree von Gruppen- & Einzelpreisnetzwerken:_
Gruppenpreise:
```
gruppenpreise1 <- delete_edges(preistraegerfinal, E(preistraegerfinal)[which(format == 1)])
gruppenpreise2 <- delete_vertices (gruppenpreise1, V(gruppenpreise1)[(degree(gruppenpreise1, mode="out")=="1") & (type == 0)])
gruppenpreise <- delete_vertices (gruppenpreise2, V(gruppenpreise2)[degree(gruppenpreise2, mode="all")=="0"])
indgruppenpreise <- degree(gruppenpreise, mode="in")
sort(indgruppenpreise)

gruppenpreise2015 <- subgraph.edges(gruppenpreise, E(gruppenpreise)[year == "2015"]) 
indgruppenpreise2015 <- degree(gruppenpreise2015, mode="in")
sort(indgruppenpreise2015)

gruppenpreise2016 <- subgraph.edges(gruppenpreise, E(gruppenpreise)[year == "2016"]) 
indgruppenpreise2016 <- degree(gruppenpreise2016, mode="in")
sort(indgruppenpreise2016)

gruppenpreise2017 <- subgraph.edges(gruppenpreise, E(gruppenpreise)[year == "2017"]) 
indgruppenpreise2017 <- degree(gruppenpreise2017, mode="in")
sort(indgruppenpreise2017)

gruppenpreise2018 <- subgraph.edges(gruppenpreise, E(gruppenpreise)[year == "2018"]) 
indgruppenpreise2018 <- degree(gruppenpreise2018, mode="in")
sort(indgruppenpreise2018)

gruppenpreise2019 <- subgraph.edges(gruppenpreise, E(gruppenpreise)[year == "2019"]) 
indgruppenpreise2019 <- degree(gruppenpreise2019, mode="in")
sort(indgruppenpreise2019)
```
Einzelpreise:
```
einzelpreise1 <- delete_edges(preistraegerfinal, E(preistraegerfinal)[which(format == 2)])
einzelpreise2 <- delete_vertices (einzelpreise1, V(einzelpreise1)[(degree(einzelpreise1, mode="out")=="1") & (type == 0)])
einzelpreise <- delete_vertices (einzelpreise2, V(einzelpreise2)[degree(einzelpreise2, mode="all")=="0"])
indeinzelpreise <- degree(einzelpreise, mode="in")
sort(indeinzelpreise)

einzelpreise2015 <- subgraph.edges(einzelpreise, E(einzelpreise)[year == "2015"]) 
indeinzelpreise2015 <- degree(einzelpreise2015, mode="in")
sort(indeinzelpreise2015)

einzelpreise2016 <- subgraph.edges(einzelpreise, E(einzelpreise)[year == "2016"]) 
indeinzelpreise2016 <- degree(einzelpreise2016, mode="in")
sort(indeinzelpreise2016)

einzelpreise2017 <- subgraph.edges(einzelpreise, E(einzelpreise)[year == "2017"]) 
indeinzelpreise2017 <- degree(einzelpreise2017, mode="in")
sort(indeinzelpreise2017)

einzelpreise2018 <- subgraph.edges(einzelpreise, E(einzelpreise)[year == "2018"]) 
indeinzelpreise2018 <- degree(einzelpreise2018, mode="in")
sort(indeinzelpreise2018)

einzelpreise2019 <- subgraph.edges(einzelpreise, E(einzelpreise)[year == "2019"]) 
indeinzelpreise2019 <- degree(einzelpreise2019, mode="in")
sort(indeinzelpreise2019)
```

**größter Indegree (unter Jury, nach Jahren)**
```
indjury2015 <- degree(juryfinal2015, mode="in")
sort(indjury2015)

indjury2016 <- degree(juryfinal2016, mode="in")
sort(indjury2016)

indjury2017 <- degree(juryfinal2017, mode="in")
sort(indjury2017)

indjury2018 <- degree(juryfinal2018, mode="in")
sort(indjury2018)

indjury2019 <- degree(juryfinal2019, mode="in")
sort(indjury2019)
```





_Bremer Fernsehpreis_
```
personen_bremerfernsehpreis <- V(bremerfernsehpreis[[1]])$type == 0
sum(personen_bremerfernsehpreis, na.rm = TRUE)
beziehungen_bremerfernsehpreis <- (E(bremerfernsehpreis[[1]])$relation == 1) + (E(bremerfernsehpreis[[1]])$relation == 3)
sum(beziehungen_bremerfernsehpreis, na.rm = TRUE)
gruppenpreise_bremerfernsehpreis <- (E(bremerfernsehpreis[[1]])$format == 2)
sum(gruppenpreise_bremerfernsehpreis, na.rm = TRUE)

bremerfernsehpreis_2grad <- subgraph <- make_ego_graph(g, order=2, c("Bremer Fernsehpreis"))
bremerfernsehpreis_2grad
plot(bremerfernsehpreis_2grad[[1]])

bremerfernsehpreis_2grad_oP <- delete_vertices(bremerfernsehpreis_2grad[[1]], V(bremerfernsehpreis_2grad[[1]])[which (institutiontype == 1)])
plot(bremerfernsehpreis_2grad_oP)

ind_bremerfernsehpreis_2grad_oP <- degree(bremerfernsehpreis_2grad_oP, mode="in")
sort(ind_bremerfernsehpreis_2grad_oP)

ag_bremerfernsehpreis <- (E(bremerfernsehpreis_2grad[[1]])$relation == 2)
sum(ag_bremerfernsehpreis, na.rm = TRUE)
```

_djp_
```
personen_djp <- V(djp[[1]])$type == 0
sum(personen_djp, na.rm = TRUE)
beziehungen_djp <- (E(djp[[1]])$relation == 1) + (E(djp[[1]])$relation == 3)
sum(beziehungen_djp, na.rm = TRUE)
gruppenpreise_djp <- (E(djp[[1]])$format == 2)
sum(gruppenpreise_djp, na.rm = TRUE)

djp_2grad <- subgraph <- make_ego_graph(g, order=2, c("Deutscher Journalistenpreis Wirtschaft | Börse | Finanzen (djp)"))
djp_2grad
plot(djp_2grad[[1]])

djp_2grad_oP <- delete_vertices(djp_2grad[[1]], V(djp_2grad[[1]])[which (institutiontype == 1)])
plot(djp_2grad_oP)

ind_djp_2grad_oP <- degree(djp_2grad_oP, mode="in")
sort(ind_djp_2grad_oP)

ag_djp <- (E(djp_2grad[[1]])$relation == 2)
sum(ag_djp, na.rm = TRUE)
```

_Deutscher Radiopreis_
```
personen_deutscherradiopreis <- V(deutscherradiopreis[[1]])$type == 0
sum(personen_deutscherradiopreis, na.rm = TRUE)
beziehungen_deutscherradiopreis <- (E(deutscherradiopreis[[1]])$relation == 1) + (E(deutscherradiopreis[[1]])$relation == 3)
sum(beziehungen_deutscherradiopreis, na.rm = TRUE)
gruppenpreise_deutscherradiopreis <- (E(deutscherradiopreis[[1]])$format == 2)
sum(gruppenpreise_deutscherradiopreis, na.rm = TRUE)

deutscherradiopreis_2grad <- subgraph <- make_ego_graph(g, order=2, c("Deutscher Radiopreis"))
deutscherradiopreis_2grad
plot(deutscherradiopreis_2grad[[1]])

deutscherradiopreis_2grad_oP <- delete_vertices(deutscherradiopreis_2grad[[1]], V(deutscherradiopreis_2grad[[1]])[which (institutiontype == 1)])
plot(deutscherradiopreis_2grad_oP)

ind_deutscherradiopreis_2grad_oP <- degree(deutscherradiopreis_2grad_oP, mode="in")
sort(ind_deutscherradiopreis_2grad_oP)

ag_deutscherradiopreis <- (E(deutscherradiopreis_2grad[[1]])$relation == 2)
sum(ag_deutscherradiopreis, na.rm = TRUE)
```

_Deutscher Reporterpreis_
```
personen_deutscherreporterpreis <- V(deutscherreporterpreis[[1]])$type == 0
sum(personen_deutscherreporterpreis, na.rm = TRUE)
beziehungen_deutscherreporterpreis <- (E(deutscherreporterpreis[[1]])$relation == 1) + (E(deutscherreporterpreis[[1]])$relation == 3)
sum(beziehungen_deutscherreporterpreis, na.rm = TRUE)
gruppenpreise_deutscherreporterpreis <- (E(deutscherreporterpreis[[1]])$format == 2)
sum(gruppenpreise_deutscherreporterpreis, na.rm = TRUE)

deutscherreporterpreis_2grad <- subgraph <- make_ego_graph(g, order=2, c("Deutscher Reporterpreis"))
deutscherreporterpreis_2grad
plot(deutscherreporterpreis_2grad[[1]])

deutscherreporterpreis_2grad_oP <- delete_vertices(deutscherreporterpreis_2grad[[1]], V(deutscherreporterpreis_2grad[[1]])[which (institutiontype == 1)])
plot(deutscherreporterpreis_2grad_oP)

ind_deutscherreporterpreis_2grad_oP <- degree(deutscherreporterpreis_2grad_oP, mode="in")
sort(ind_deutscherreporterpreis_2grad_oP)

ag_deutscherreporterpreis <- (E(deutscherreporterpreis_2grad[[1]])$relation == 2)
sum(ag_deutscherreporterpreis, na.rm = TRUE)
```

_Ernst-Schneider-Preis_
```
personen_ernstschneiderpreis <- V(ernstschneiderpreis[[1]])$type == 0
sum(personen_ernstschneiderpreis, na.rm = TRUE)
beziehungen_ernstschneiderpreis <- (E(ernstschneiderpreis[[1]])$relation == 1) + (E(ernstschneiderpreis[[1]])$relation == 3)
sum(beziehungen_ernstschneiderpreis, na.rm = TRUE)
gruppenpreise_ernstschneiderpreis <- (E(ernstschneiderpreis[[1]])$format == 2)
sum(gruppenpreise_ernstschneiderpreis, na.rm = TRUE)

ernstschneiderpreis_2grad <- subgraph <- make_ego_graph(g, order=2, c("Ernst-Schneider-Preis"))
ernstschneiderpreis_2grad
plot(ernstschneiderpreis_2grad[[1]])

ernstschneiderpreis_2grad_oP <- delete_vertices(ernstschneiderpreis_2grad[[1]], V(ernstschneiderpreis_2grad[[1]])[which (institutiontype == 1)])
plot(ernstschneiderpreis_2grad_oP)

ind_ernstschneiderpreis_2grad_oP <- degree(ernstschneiderpreis_2grad_oP, mode="in")
sort(ind_ernstschneiderpreis_2grad_oP)

ag_ernstschneiderpreis <- (E(ernstschneiderpreis_2grad[[1]])$relation == 2)
sum(ag_ernstschneiderpreis, na.rm = TRUE)
```

_GvH-Preis_
```
personen_gvhpreis <- V(gvhpreis[[1]])$type == 0
sum(personen_gvhpreis, na.rm = TRUE)
beziehungen_gvhpreis <- (E(gvhpreis[[1]])$relation == 1) + (E(gvhpreis[[1]])$relation == 3)
sum(beziehungen_gvhpreis, na.rm = TRUE)
gruppenpreise_gvhpreis <- (E(gvhpreis[[1]])$format == 2)
sum(gruppenpreise_gvhpreis, na.rm = TRUE)

gvhpreis_2grad <- subgraph <- make_ego_graph(g, order=2, c("Georg von Holtzbrinck-Preis für Wirtschaftspublizistik bzw. Ferdinand Simoneit-Nachwuchspreis"))
gvhpreis_2grad
plot(gvhpreis_2grad[[1]])

gvhpreis_2grad_oP <- delete_vertices(gvhpreis_2grad[[1]], V(gvhpreis_2grad[[1]])[which (institutiontype == 1)])
plot(gvhpreis_2grad_oP)

ind_gvhpreis_2grad_oP <- degree(gvhpreis_2grad_oP, mode="in")
sort(ind_gvhpreis_2grad_oP)

ag_gvhpreis <- (E(gvhpreis_2grad[[1]])$relation == 2)
sum(ag_gvhpreis, na.rm = TRUE)
```

_Grimme Online Award_
```
personen_grimmeonlineaward <- V(grimmeonlineaward[[1]])$type == 0
sum(personen_grimmeonlineaward, na.rm = TRUE)
beziehungen_grimmeonlineaward <- (E(grimmeonlineaward[[1]])$relation == 1) + (E(grimmeonlineaward[[1]])$relation == 3)
sum(beziehungen_grimmeonlineaward, na.rm = TRUE)
gruppenpreise_grimmeonlineaward <- (E(grimmeonlineaward[[1]])$format == 2)
sum(gruppenpreise_grimmeonlineaward, na.rm = TRUE)

grimmeonlineaward_2grad <- subgraph <- make_ego_graph(g, order=2, c("Grimme Online Award"))
grimmeonlineaward_2grad
plot(grimmeonlineaward_2grad[[1]])

grimmeonlineaward_2grad_oP <- delete_vertices(grimmeonlineaward_2grad[[1]], V(grimmeonlineaward_2grad[[1]])[which (institutiontype == 1)])
plot(grimmeonlineaward_2grad_oP)

ind_grimmeonlineaward_2grad_oP <- degree(grimmeonlineaward_2grad_oP, mode="in")
sort(ind_grimmeonlineaward_2grad_oP)

ag_grimmeonlineaward <- (E(grimmeonlineaward_2grad[[1]])$relation == 2)
sum(ag_grimmeonlineaward, na.rm = TRUE)
```

_Helmut-Schmidt-Preis_
```
personen_helmutschmidtpreis <- V(helmutschmidtpreis[[1]])$type == 0
sum(personen_helmutschmidtpreis, na.rm = TRUE)
beziehungen_helmutschmidtpreis <- (E(helmutschmidtpreis[[1]])$relation == 1) + (E(helmutschmidtpreis[[1]])$relation == 3)
sum(beziehungen_helmutschmidtpreis, na.rm = TRUE)
gruppenpreise_helmutschmidtpreis <- (E(helmutschmidtpreis[[1]])$format == 2)
sum(gruppenpreise_helmutschmidtpreis, na.rm = TRUE)

helmutschmidtpreis_2grad <- subgraph <- make_ego_graph(g, order=2, c("Helmut-Schmidt-Journalistenpreis"))
helmutschmidtpreis_2grad
plot(helmutschmidtpreis_2grad[[1]])

helmutschmidtpreis_2grad_oP <- delete_vertices(helmutschmidtpreis_2grad[[1]], V(helmutschmidtpreis_2grad[[1]])[which (institutiontype == 1)])
plot(helmutschmidtpreis_2grad_oP)

ind_helmutschmidtpreis_2grad_oP <- degree(helmutschmidtpreis_2grad_oP, mode="in")
sort(ind_helmutschmidtpreis_2grad_oP)

ag_helmutschmidtpreis <- (E(helmutschmidtpreis_2grad[[1]])$relation == 2)
sum(ag_helmutschmidtpreis, na.rm = TRUE)
```

_Herbert-Quandt-Preis_
```
personen_herbertquandtpreis <- V(herbertquandtpreis[[1]])$type == 0
sum(personen_herbertquandtpreis, na.rm = TRUE)
beziehungen_herbertquandtpreis <- (E(herbertquandtpreis[[1]])$relation == 1) + (E(herbertquandtpreis[[1]])$relation == 3)
sum(beziehungen_herbertquandtpreis, na.rm = TRUE)
gruppenpreise_herbertquandtpreis <- (E(herbertquandtpreis[[1]])$format == 2)
sum(gruppenpreise_herbertquandtpreis, na.rm = TRUE)

herbertquandtpreis_2grad <- subgraph <- make_ego_graph(g, order=2, c("Herbert Quandt Medienpreis"))
herbertquandtpreis_2grad
plot(herbertquandtpreis_2grad[[1]])

herbertquandtpreis_2grad_oP <- delete_vertices(herbertquandtpreis_2grad[[1]], V(herbertquandtpreis_2grad[[1]])[which (institutiontype == 1)])
plot(herbertquandtpreis_2grad_oP)

ind_herbertquandtpreis_2grad_oP <- degree(herbertquandtpreis_2grad_oP, mode="in")
sort(ind_herbertquandtpreis_2grad_oP)

ag_herbertquandtpreis <- (E(herbertquandtpreis_2grad[[1]])$relation == 2)
sum(ag_herbertquandtpreis, na.rm = TRUE)
```

_Journalist des Jahres_
```
personen_journalistdesjahres <- V(journalistdesjahres[[1]])$type == 0
sum(personen_journalistdesjahres, na.rm = TRUE)
beziehungen_journalistdesjahres <- (E(journalistdesjahres[[1]])$relation == 1) + (E(journalistdesjahres[[1]])$relation == 3)
sum(beziehungen_journalistdesjahres, na.rm = TRUE)
gruppenpreise_journalistdesjahres <- (E(journalistdesjahres[[1]])$format == 2)
sum(gruppenpreise_journalistdesjahres, na.rm = TRUE)

journalistdesjahres_2grad <- subgraph <- make_ego_graph(g, order=2, c("Journalist des Jahres"))
journalistdesjahres_2grad
plot(journalistdesjahres_2grad[[1]])

journalistdesjahres_2grad_oP <- delete_vertices(journalistdesjahres_2grad[[1]], V(journalistdesjahres_2grad[[1]])[which (institutiontype == 1)])
plot(journalistdesjahres_2grad_oP)

ind_journalistdesjahres_2grad_oP <- degree(journalistdesjahres_2grad_oP, mode="in")
sort(ind_journalistdesjahres_2grad_oP)

ag_journalistdesjahres <- (E(journalistdesjahres_2grad[[1]])$relation == 2)
sum(ag_journalistdesjahres, na.rm = TRUE)
```

_Kath. Medienpreis_
```
personen_katholischermedienpreis <- V(katholischermedienpreis[[1]])$type == 0
sum(personen_katholischermedienpreis, na.rm = TRUE)
beziehungen_katholischermedienpreis <- (E(katholischermedienpreis[[1]])$relation == 1) + (E(katholischermedienpreis[[1]])$relation == 3)
sum(beziehungen_katholischermedienpreis, na.rm = TRUE)
gruppenpreise_katholischermedienpreis <- (E(katholischermedienpreis[[1]])$format == 2)
sum(gruppenpreise_katholischermedienpreis, na.rm = TRUE)

katholischermedienpreis_2grad <- subgraph <- make_ego_graph(g, order=2, c("Katholischer Medienpreis"))
katholischermedienpreis_2grad
plot(katholischermedienpreis_2grad[[1]])

katholischermedienpreis_2grad_oP <- delete_vertices(katholischermedienpreis_2grad[[1]], V(katholischermedienpreis_2grad[[1]])[which (institutiontype == 1)])
plot(katholischermedienpreis_2grad_oP)

ind_katholischermedienpreis_2grad_oP <- degree(katholischermedienpreis_2grad_oP, mode="in")
sort(ind_katholischermedienpreis_2grad_oP)

ag_katholischermedienpreis <- (E(katholischermedienpreis_2grad[[1]])$relation == 2)
sum(ag_katholischermedienpreis, na.rm = TRUE)
```

_Kurt-Tucholsky-Preis_
```
personen_kurttucholskypreis <- V(kurttucholskypreis[[1]])$type == 0
sum(personen_kurttucholskypreis, na.rm = TRUE)
beziehungen_kurttucholskypreis <- (E(kurttucholskypreis[[1]])$relation == 1) + (E(kurttucholskypreis[[1]])$relation == 3)
sum(beziehungen_kurttucholskypreis, na.rm = TRUE)
gruppenpreise_kurttucholskypreis <- (E(kurttucholskypreis[[1]])$format == 2)
sum(gruppenpreise_kurttucholskypreis, na.rm = TRUE)

kurttucholskypreis_2grad <- subgraph <- make_ego_graph(g, order=2, c("Kurt-Tucholsky-Preis für literarische Publizistik"))
kurttucholskypreis_2grad
plot(kurttucholskypreis_2grad[[1]])

kurttucholskypreis_2grad_oP <- delete_vertices(kurttucholskypreis_2grad[[1]], V(kurttucholskypreis_2grad[[1]])[which (institutiontype == 1)])
plot(kurttucholskypreis_2grad_oP)

ind_kurttucholskypreis_2grad_oP <- degree(kurttucholskypreis_2grad_oP, mode="in")
sort(ind_kurttucholskypreis_2grad_oP)

ag_kurttucholskypreis <- (E(kurttucholskypreis_2grad[[1]])$relation == 2)
sum(ag_kurttucholskypreis, na.rm = TRUE)
```

_Leuchtturm_
```
personen_leuchtturm <- V(leuchtturm[[1]])$type == 0
sum(personen_leuchtturm, na.rm = TRUE)
beziehungen_leuchtturm <- (E(leuchtturm[[1]])$relation == 1) + (E(leuchtturm[[1]])$relation == 3)
sum(beziehungen_leuchtturm, na.rm = TRUE)
gruppenpreise_leuchtturm <- (E(leuchtturm[[1]])$format == 2)
sum(gruppenpreise_leuchtturm, na.rm = TRUE)

leuchtturm_2grad <- subgraph <- make_ego_graph(g, order=2, c("Leuchtturm für besondere publizistische Leistungen"))
leuchtturm_2grad
plot(leuchtturm_2grad[[1]])

leuchtturm_2grad_oP <- delete_vertices(leuchtturm_2grad[[1]], V(leuchtturm_2grad[[1]])[which (institutiontype == 1)])
plot(leuchtturm_2grad_oP)

ind_leuchtturm_2grad_oP <- degree(leuchtturm_2grad_oP, mode="in")
sort(ind_leuchtturm_2grad_oP)

ag_leuchtturm <- (E(leuchtturm_2grad[[1]])$relation == 2)
sum(ag_leuchtturm, na.rm = TRUE)
```

_Ludwig-Börne-Preis_
```
personen_ludwigboernepreis <- V(ludwigboernepreis[[1]])$type == 0
sum(personen_ludwigboernepreis, na.rm = TRUE)
beziehungen_ludwigboernepreis <- (E(ludwigboernepreis[[1]])$relation == 1) + (E(ludwigboernepreis[[1]])$relation == 3)
sum(beziehungen_ludwigboernepreis, na.rm = TRUE)
gruppenpreise_ludwigboernepreis <- (E(ludwigboernepreis[[1]])$format == 2)
sum(gruppenpreise_ludwigboernepreis, na.rm = TRUE)

ludwigboernepreis_2grad <- subgraph <- make_ego_graph(g, order=2, c("Ludwig-Börne-Preis"))
ludwigboernepreis_2grad
plot(ludwigboernepreis_2grad[[1]])

ludwigboernepreis_2grad_oP <- delete_vertices(ludwigboernepreis_2grad[[1]], V(ludwigboernepreis_2grad[[1]])[which (institutiontype == 1)])
plot(ludwigboernepreis_2grad_oP)

ind_ludwigboernepreis_2grad_oP <- degree(ludwigboernepreis_2grad_oP, mode="in")
sort(ind_ludwigboernepreis_2grad_oP)

ag_ludwigboernepreis <- (E(ludwigboernepreis_2grad[[1]])$relation == 2)
sum(ag_ludwigboernepreis, na.rm = TRUE)
```

_Ludwig-Erhard-Preis_
```
personen_ludwigerhardpreis <- V(ludwigerhardpreis[[1]])$type == 0
sum(personen_ludwigerhardpreis, na.rm = TRUE)
beziehungen_ludwigerhardpreis <- (E(ludwigerhardpreis[[1]])$relation == 1) + (E(ludwigerhardpreis[[1]])$relation == 3)
sum(beziehungen_ludwigerhardpreis, na.rm = TRUE)
gruppenpreise_ludwigerhardpreis <- (E(ludwigerhardpreis[[1]])$format == 2)
sum(gruppenpreise_ludwigerhardpreis, na.rm = TRUE)

ludwigerhardpreis_2grad <- subgraph <- make_ego_graph(g, order=2, c("Ludwig-Erhard-Preis für Wirtschaftspublizistik"))
ludwigerhardpreis_2grad
plot(ludwigerhardpreis_2grad[[1]])

ludwigerhardpreis_2grad_oP <- delete_vertices(ludwigerhardpreis_2grad[[1]], V(ludwigerhardpreis_2grad[[1]])[which (institutiontype == 1)])
plot(ludwigerhardpreis_2grad_oP)

ind_ludwigerhardpreis_2grad_oP <- degree(ludwigerhardpreis_2grad_oP, mode="in")
sort(ind_ludwigerhardpreis_2grad_oP)

ag_ludwigerhardpreis <- (E(ludwigerhardpreis_2grad[[1]])$relation == 2)
sum(ag_ludwigerhardpreis, na.rm = TRUE)
```

_Nannenpreis_
```
personen_nannenpreis <- V(nannenpreis[[1]])$type == 0
sum(personen_nannenpreis, na.rm = TRUE)
beziehungen_nannenpreis <- (E(nannenpreis[[1]])$relation == 1) + (E(nannenpreis[[1]])$relation == 3)
sum(beziehungen_nannenpreis, na.rm = TRUE)
gruppenpreise_nannenpreis <- (E(nannenpreis[[1]])$format == 2)
sum(gruppenpreise_nannenpreis, na.rm = TRUE)

nannenpreis_2grad <- subgraph <- make_ego_graph(g, order=2, c("Nannen Preis"))
nannenpreis_2grad
plot(nannenpreis_2grad[[1]])

nannenpreis_2grad_oP <- delete_vertices(nannenpreis_2grad[[1]], V(nannenpreis_2grad[[1]])[which (institutiontype == 1)])
plot(nannenpreis_2grad_oP)

ind_nannenpreis_2grad_oP <- degree(nannenpreis_2grad_oP, mode="in")
sort(ind_nannenpreis_2grad_oP)

ag_nannenpreis <- (E(nannenpreis_2grad[[1]])$relation == 2)
sum(ag_nannenpreis, na.rm = TRUE)
```

_Robert-Geisendörfer-Preis_
```
personen_robertgeisendoerferpreis <- V(robertgeisendoerferpreis[[1]])$type == 0
sum(personen_robertgeisendoerferpreis, na.rm = TRUE)
beziehungen_robertgeisendoerferpreis <- (E(robertgeisendoerferpreis[[1]])$relation == 1) + (E(robertgeisendoerferpreis[[1]])$relation == 3)
sum(beziehungen_robertgeisendoerferpreis, na.rm = TRUE)
gruppenpreise_robertgeisendoerferpreis <- (E(robertgeisendoerferpreis[[1]])$format == 2)
sum(gruppenpreise_robertgeisendoerferpreis, na.rm = TRUE)

robertgeisendoerferpreis_2grad <- subgraph <- make_ego_graph(g, order=2, c("Robert Geisendörfer Preis"))
robertgeisendoerferpreis_2grad
plot(robertgeisendoerferpreis_2grad[[1]])

robertgeisendoerferpreis_2grad_oP <- delete_vertices(robertgeisendoerferpreis_2grad[[1]], V(robertgeisendoerferpreis_2grad[[1]])[which (institutiontype == 1)])
plot(robertgeisendoerferpreis_2grad_oP)

ind_robertgeisendoerferpreis_2grad_oP <- degree(robertgeisendoerferpreis_2grad_oP, mode="in")
sort(ind_robertgeisendoerferpreis_2grad_oP)

ag_robertgeisendoerferpreis <- (E(robertgeisendoerferpreis_2grad[[1]])$relation == 2)
sum(ag_robertgeisendoerferpreis, na.rm = TRUE)
```

_Theodor-Wolff-Preis_
```
personen_theodorwolffpreis <- V(theodorwolffpreis[[1]])$type == 0
sum(personen_theodorwolffpreis, na.rm = TRUE)
beziehungen_theodorwolffpreis <- (E(theodorwolffpreis[[1]])$relation == 1) + (E(theodorwolffpreis[[1]])$relation == 3)
sum(beziehungen_theodorwolffpreis, na.rm = TRUE)
gruppenpreise_theodorwolffpreis <- (E(theodorwolffpreis[[1]])$format == 2)
sum(gruppenpreise_theodorwolffpreis, na.rm = TRUE)

theodorwolffpreis_2grad <- subgraph <- make_ego_graph(g, order=2, c("Theodor-Wolff-Preis"))
theodorwolffpreis_2grad
plot(theodorwolffpreis_2grad[[1]])

theodorwolffpreis_2grad_oP <- delete_vertices(theodorwolffpreis_2grad[[1]], V(theodorwolffpreis_2grad[[1]])[which (institutiontype == 1)])
plot(theodorwolffpreis_2grad_oP)

ind_theodorwolffpreis_2grad_oP <- degree(theodorwolffpreis_2grad_oP, mode="in")
sort(ind_theodorwolffpreis_2grad_oP)

ag_theodorwolffpreis <- (E(theodorwolffpreis_2grad[[1]])$relation == 2)
sum(ag_theodorwolffpreis, na.rm = TRUE)
```

_Wächterpreis_
```
personen_waechterpreis <- V(waechterpreis[[1]])$type == 0
sum(personen_waechterpreis, na.rm = TRUE)
beziehungen_waechterpreis <- (E(waechterpreis[[1]])$relation == 1) + (E(waechterpreis[[1]])$relation == 3)
sum(beziehungen_waechterpreis, na.rm = TRUE)
gruppenpreise_waechterpreis <- (E(waechterpreis[[1]])$format == 2)
sum(gruppenpreise_waechterpreis, na.rm = TRUE)

waechterpreis_2grad <- subgraph <- make_ego_graph(g, order=2, c("Wächterpreis der Tagespresse"))
waechterpreis_2grad
plot(waechterpreis_2grad[[1]])

waechterpreis_2grad_oP <- delete_vertices(waechterpreis_2grad[[1]], V(waechterpreis_2grad[[1]])[which (institutiontype == 1)])
plot(waechterpreis_2grad_oP)

ind_waechterpreis_2grad_oP <- degree(waechterpreis_2grad_oP, mode="in")
sort(ind_waechterpreis_2grad_oP)

ag_waechterpreis <- (E(waechterpreis_2grad[[1]])$relation == 2)
sum(ag_waechterpreis, na.rm = TRUE)
```

## Visualisierungen

### Elite in Elite-Netzwerk
(noch nicht schön!)
```
plot(g3be,
     edge.arrow.size=.02,
     edge.label.degree=0.1,
     vertex.frame.color="white",
     vertex.label.family="Helvetica",
     vertex.label.dist=0.5,
     vertex.label.cex=.6,
     layout = layout_with_kk)
```

## ab hier: Spielwiese 

# Ego-Netzwerk Obermayer
```
edge_attr(g, "relation", index = E(g))
h <- simplify(g, edge.attr.comb = "sum"))
h
obermayer <- subgraph <- make_ego_graph(h, order=1, c("Bastian Obermayer"))
obermayer
plot(obermayer[[1]], edge.arrow.size=.1, edge.label.degree=0, vertex.frame.color="white", vertex.label.family="Helvetica", vertex.label.dist=0.5, vertex.label.cex=.6, layout = layout_with_kk)
```
