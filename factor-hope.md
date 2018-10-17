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

Code Java: `extraction.circonstances_factuelles.Vectorization`

