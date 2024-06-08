# Projet 

Pour notre projet nous avons décidé de réaliser un classifieur qui identifie les textes générés par ChatGPT VS les textes rédigés par un être humain.

# Cahier des charges 

Réaliser un classifieur de textes. 

* Classification supervisée afin de reconnaître les textes générés par une intelligence artificielle. 

* Le classifieur sera réalisé à partir d'un modèle pré-éntrainé de votre choix ainsi que l'usage de transformer.

* Votre jeu de données devra être complet et pertinent pour l'entraînement recherché.

* Afin d'entraîner votre modèle, vous utiliserez une librairie de machine learning de type PyTorch ou TensorFlow.

* Une fois le modèle entraîné, il est important de le tester sur de nouvelles données. Vous inclurez les tests effectués dans les livrables.



# Introduction 

Pour notre projet de Python du second semestre nous avons fine-tuné un modèle de langue pré-entrainé appelé DistilBERT. Le concept de "fine-tuning" en Machine et Deep Learning consiste à adapter, au travers de parmètres précis, un modèle qui a déja été entrainé. Dans notre cas, nous avons adapté notre modèle à base de transformer pour qu'il puisse identifier lorsqu'un texte a été rédigé par un humain ou généré par une intelligence artificielle, nous nous sommes concentrées sur des textes générés par ChatGPT.

DistilBERT est une version plus légère et plus rapide de BERT. Il est à la fois un transformer et un modèle de langue de type LLM.

Lorsque l'on utilise un réseau de neurones tel qu'un transformer, les données textuelles sont généralement prétraitées pour les adapter à l'entrée du modèle. Cela peut inclure des étapes telles que le tokenization, la vectorisation, et le padding (ajout de zéros pour que toutes les séquences aient la même longueur). 

## Qu'est-ce qu'un Transformer ?

Les transformers sont un type d'architecture de réseau neuronal très utilisés en Traitement automatique des langues.

Les transformers s'appuient sur un mécanisme appelé "auto-attention" pour pondérer l'importance des différents mots dans une phrase ou une séquence. Cela leur permet de capturer les dépendances entre les mots indépendamment de leur position dans la séquence, ce qui les rend très efficaces pour des tâches telles que la traduction de langues, la génération de texte et l'analyse de sentiment.

Les transformers se composent d'un encodeur et d'un décodeur, chacun composé de plusieurs couches de mécanismes d'auto-attention et de réseaux neuronaux à propagation avant. Pendant l'entraînement, le modèle apprend à mapper des séquences d'entrée sur des séquences de sortie en ajustant ses paramètres pour minimiser une fonction de perte choisie, généralement à l'aide de techniques telles que la rétropropagation et la descente de gradient.

## Le Jeu de données

Avant toute chose, il est important d'analyser à quoi ressemble le ou les jeux de données qui vont nous permettre d'entrainer notre modèle par la suite.

Notre jeu de données provient du site [Hugging Face](https://huggingface.co/datasets/Hello-SimpleAI/HC3). Il s'agit des questions-réponses tirées directement du web, **toutes classées en 5 subsets** : finance, médecine, reddit (un site web communautaire américain), questions ouvertes et demndes de définitions (wiki-csai). Les datasets sont téléchargeable localement sous format jsonl, il est aussi possible de les télécharger directement dans son code avec la fonction **load_dataset**. Nous avons choisi de les importer directement dans notre code, une fois télécharger le jeu de données est un objet au format datasets. 

Il est nécéssaire lors de l'entrainement d'un modèle de préablement diviser son jeu de données en **une partie entrainement** et **une partie test**. 

Pour entraîner notre modèle nous avons utilisé les données des 5 subsets, ce qui représente un jeu de données assez conséquent pour notre modèle.

## Tokenizer 

Le tokenizer utilisé est déjà configuré pour le modèle DistilBERT, nous le définissons sous forme d'objet afin de faciliter son utilisation. Les tokens encodés sont introduits dans le modèle. 

Les tokens encodés passent à travers plusieurs couches de transformer. Chaque couche contient des mécanismes d'auto-attention et des réseaux de neurones à propagation avant. L'auto-attention permet au modèle de comprendre les relations entre les mots dans le contexte de la séquence, tandis que les réseaux de neurones à propagation avant permettent de capturer des caractéristiques de plus haut niveau.

## Padding 

La classe « DataCollatorWithPadding » des transformers Hugging Face permet de préparer des lots (batches) de données pour l'entraînement des modèles transformers. Plus précisément, elle est conçue pour gérer les cas où les séquences d'entrée ont des longueurs différentes.

Lors de l'entraînement d'un modèle de transformer, il est courant de regrouper les séquences pour un traitement plus efficace. Cependant, comme les séquences peuvent avoir des longueurs différentes, elles doivent être ramenées à une longueur commune dans chaque batch. La classe « DataCollatorWithPadding » automatise ce processus.


# Inférences 
Une fois le modèle entraîné, il faut le tester avec des nouvelles données afin d'évaluer sa performance. La manière la plus simple pour tester notre modèle est d'utiliser une pipeline. 

## Pipeline 

Une pipeline est une manière d'organiser une série d'opérations ou de fonctions qui traitent certaines données. La sortie d'une opération devient l'entrée de la suivante, et ainsi de suite, jusqu'à l'obtention du résultat final. Une pipeline peut être visualisé comme une chaîne de tuyaux, où les données circulent d'un tuyau à l'autre, en subissant une transformation ou une manipulation en cours de route.

On instancie une pipeline pour la classification de texte, mais il existe pleins d'autres fonctions en TAL qui peuvent nécessiter une pipeline (l'analyse de sentiments, la génération de texte etc). Notre pipeline est appelé "Classifier" et nous renvoie un label 0 pour les réponses humaines, ou un label 1 pour les réponses de Chatgpt.