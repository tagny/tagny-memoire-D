# Why would you do better
## annotation des sections
*  annotation manuelle des 4 sections: entete, litige, motifs, dispositif
*  sélection des descripteurs
*  sélection de la représentation de segment
*  lemmatisation
*  annoter des juridictions variées: tribunaux et cours d'appel
*  annotation au niveau du mot en considérant des morceaux du documents (300 mots par exemples)
## annotation des entités
*  annoter des juridictions variées: tribunaux et cours d'appel
## extraction des demandes
*  apprentissage des termes sur la lemmatisation totale du doc
*  extraction après conservation des sommes d'argent, des verbes d'énoncés et des ponctuations
*  extraction des demandes et résultat indépendamment de la section
*  extraction des termes clés sur la lemmatisation
*  apprentissage des termes clés par one-live-out ou 5 ou 10-fold crossvalidation (prendre l'union des termes sélectionnés)
## modélisation des circonstances factuelles
*  expérimenter la combinaison clustering + topic modeling
## démo
*  un notebook jupyter avec des méthodes de stats sur la BD
*  double-histogramme zoomable: qr en haut et qd en bas
 

select EXTRACT(YEAR FROM date_decision) as annee, count(id_demande) as r, lower(ville) as v 
from (((decision 
inner join demande on decision.id_decision=demande.id_decision)
inner join categorie on categorie.id_categorie=demande.id_categorie)
inner join norme on norme.id_norme=demande.id_norme)
where objet="amende civile" AND norme="32-1 code de procédure civile + 559 code de procédure civile : pour procédure abusive" AND EXTRACT(YEAR FROM date_decision) = "2008"
group by v, annee
order by annee asc;



select count(id_demande) as c_val, count(id_demande)*100/t.total, ville as v
from ((((decision 
inner join demande on decision.id_decision=demande.id_decision)
inner join categorie on categorie.id_categorie=demande.id_categorie)
inner join norme on norme.id_norme=demande.id_norme)
join (
    select count(id_demande) as total, ville as v
    from (((decision 
    inner join demande on decision.id_decision=demande.id_decision)
    inner join categorie on categorie.id_categorie=demande.id_categorie)
    inner join norme on norme.id_norme=demande.id_norme)
    where objet="amende civile" AND norme="32-1 code de procédure civile + 559 code de procédure civile : pour procédure abusive" AND EXTRACT(YEAR FROM date_decision) = "2008"
    group by v
) as t on  v = t.v)
where objet="amende civile" AND norme="32-1 code de procédure civile + 559 code de procédure civile : pour procédure abusive" AND EXTRACT(YEAR FROM date_decision) = "2008" AND resultat = "accepte"
group by v;


select count(id_demande) * 100 / (select count(*) from demande) as "%accepte"
from demande
where resultat = "accepte";


select EXTRACT(YEAR FROM date_decision) as annee, resultat, lower(ville) as v 
from ((decision 
inner join demande on decision.id_decision=demande.id_decision)
inner join categorie on categorie.id_categorie=demande.id_categorie)
where objet="amende civile"
group by v, annee
order by annee asc;


select EXTRACT(YEAR FROM date_decision) as annee, count(id_demande) as n, lower(ville) as v 
from ((decision 
inner join demande on decision.id_decision=demande.id_decision)
inner join categorie on categorie.id_categorie=demande.id_categorie)
where objet="amende civile" AND resultat="accepte" AND ville != ""
group by v, annee
order by annee asc;

select EXTRACT(YEAR FROM date_decision) as annee, count(id_demande) as n_decisions, lower(ville) as v 
from decision inner join demande on decision.id_decision=demande.id_decision
where objet="amende civile" AND resultat="accepte"
group by v, annee 
order by annee asc;


# Extraction des circonstances factuelles par restriction sur les phrases de la catégorie dans les motifs

## Introduction
Les faits se retrouvent dans les arguments des parties et le raisonnement des juges. Les premières lignes de MOTIFS discutent des faits généralement. Il serait idéal de distinguer les énoncés de raisonnement de ceux des résultats. Mais nous nous contentons de travailler avec la 1ere moitié de MOTIFS (la motié des phrases).

Le principe suit les phases suivantes:
1.  sectionnement corrigé des documents XML de sections lemmatisées
2.  extraction des 1e phrases de MOTIFS
3.  vectorisation du texte sélectionné (TF-IDF) sur les clusters des mots ou lemmes
4.  comparaison des algos classiques vs propositions (penser à intégrer la détermination du nombre de cluster)

## Sectionnement corrigé des documents XML de sections lemmatisées

Après avoir appliqué **uniquement le modèle de sectionnement** (CRF de Mallet), j'ai corrigé manuellement les 80 documents du corpus.

## Extraction des 1e phrases de MOTIFS

### découpage des motifs en phrases

Code Java: `taj.extraction.demande.pipeline.ConstantAndEnumerates.splitDocumentsIntoSentences`

### Restriction des phrases

Code Java: `taj.extraction.circonstances_factuelles.FocusOnMotifs.MotifsSentenceSelection`

## vectorisation du texte sélectionné (TF-IDF) sur les clusters des mots ou lemmes

### remplacement des mots par les IDs des clusters de mots

Code Java: `taj.data_representation_pls.CorpusPreprocessor.replaceWordByClusterIdInFile` appelé dans `taj.extraction.circonstances_factuelles.FocusOnMotifs.replaceTokenByClusterId`

### Vectorisation TF-IDF

Code Java: `extraction.circonstances_factuelles.Vectorization` appelé dans `extraction.circonstances_factuelles.ExtracteurDeCirconstancesFactuelles.generateMatricesFromTxt`


