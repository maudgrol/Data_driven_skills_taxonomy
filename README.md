# Data-driven skills taxonomy

## Context
A skills taxonomy could show workers other jobs that they can transition to with the skills they have or with minimum retraining. For this project I explored how an established labour market framework can be used to start developing a map of skills. This project uses the European Skills, Competences, Qualifications and Occupations (ESCO) framework. ESCO has been developed by the European Commission and includes information on relationships between 2,942 occupations and 13,485 skills/competences.

## Data
- occupations_en.csv contains information on 2,942 detailed occupations, including alternative occupation titles and occupation descriptions. These occupations are aligned with a hierarchical occupational classification International Standard Classification of Occupations (ISCO). The ISCO system has four levels based on the number of digits in the code (four digit code is the most finely grained classification and first digit is the coarsest classification). The detailed occupations in the file represent a further more granular level of the classification. The field 'iscoGroup' refers to the code of the parent ISCO occupational group. The European Skills, Competences, Qualifications and Occupations (ESCO) framework closely resembles ISCO and has a similar structure.
- skills_en.csv describes individual skills in the ESCO framework.
- ESCO_occup_skills.json is a dictionary, where the title of a detailed ESCO occupation serves as a key, and the description of this occupation is stored as a set of nested dictionaries. This file provides a link between occupations and skills that they require under label '_links' and lists skills under further nested dictionaries.

## Goal
To build a data-driven skills taxonomy

## General approach
The link between required skills and occupations could be a useful basis for building the taxonomy, because it provides contexts in which certain skills occur frequently together. That is, certain skills co-occur more often in a certain context, here across occupations listed in the ESCO framework. Additionally I use the similarity between more elaborate skill descriptions as calculated using Natural Language Processing tools. The skills can then be represented as a network with weighted edges according to these attributes, they can be hierarchically grouped into clusters using community detection algorithms.

## Code
The project was written in Python 3. The code and output are provided in a Jupyter notebook.

## Future work
- There are different techniques to extract information from raw text data. The advantage of using Word Embeddings, for example with Google's Word2Vec, is that the semantic meaning of a word can be reflected in its embedding, whereas TF-IDF vectorization is unable to capture the semantics. However, to train your own model for word embeddings would require more data. Alternatively, one could use a pre-trained model, but this corpus also contains some quite specialized vocabulary so the results may not be optimal in that case. For this project I therefore decided to use TF-IDF vectorization. In future work, I could also try Word Embeddings instead and see how well it does with different pre-trained models.
- I currently compare algorithms that detect 'crisp' communities, where each node belongs to only one community. This makes the taxonomy more intuitive to understand and visualize, but certain skills may be linked to multiple clusters, especially in the third layer of the taxonomy with more specialized skill clusters. An idea for the future would therefore be to test fuzzy community detection algorithms where skills could be assigned to more than one community with different degrees of likelihood.
- Increase stability of cluster solutions by using cluster ensemble methods: generate x number of base clustering solutions based on the original graph. Then use cluster ensemble techniques to generate a consensus partitioning solution. The definitive consensus clustering could be determined based on the highest average mutual information score between all the iterations and the resulting consensus cluster solution. 
- The most clear and concise labels for skill clusters are probably assigned manually, but ideally one would develop an automatic way of labeling, because this step is quite laborious. Data-driven methods could assist manual labeling of the very broad skill clusers (layer 1 of the taxonomy). One idea would be to use topic modelling (e.g., Latent Dirichlet Allocation) to help discover themes in clusters at the top layer of the taxonomy. 
- A visualisation of the entire taxonomy could include pop up boxes with a few representative skills and the link to a few representative occupations for which these skills are required. 

