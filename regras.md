
## TODO:
- [ ] Módulos
    - [x] oração subordinada
    - [ ] conjunções coordenadas
    - [ ] aposto
    - [ ] transitividade
- [ ] Pré-processamento
- [ ] Extração
## Dicionário

1. **HEAD**: token no qual o token atual tem dependência.
2. **deprel**: Relação de dependência com o HEAD.
3. **postag**: categoria gramatical da palavra.
## Extração
Os seguintes módulos estão disponíveis:
1. oração subordinada
2. conjunções coordenadas
3. aposto
4. transitividade
   A extração dos fatos na acontece por meio de um script seguindo a ordem abaixo:
1. [Encontrar Sujeito](#encontrar-sujeito)
2. [Encontrar Relação](#encontrar-relacao)
### Encontrar Sujeito

tokens marcados como por 'nsubj' ou 'nsubj:pass'.

para cada sujeito na frase vai fazer a busca em profundidade em cada um dos tokens filhos  marcando se é um sujeito relacionado com o sujeito principal se cumprirem com algum dos parâmetros abaixo:
1. deprel ''nummod"
2. deprel "advmod"
3. deprel "appos" e postag "NUM"
4. deprel "nmod"
5. deprel "amod"
6. deprel "dep"
7. deprel "obj"
8. deprel "det"
9. deprel "case"
10. deprel "punct" e é uma das pontuações:  "(", ")", "{", "}", """, "'", "\[", "]", '","
11. deprel "conj" e postag "conj"

#### oração subordinada
Se o módulo de oração subordinada estiver habilitado, vai buscar os pronomes relativos nesse momento também

Para cada um dos tokens de sujeitos encontrados, verifica se ele tem o postag "PRON" e tem dependência de outro token cujo deprel é "acl:relcl"

Então irá fazer a mesma busca em profundidade em todos os tokens filhos desses sujeitos para marcar como relacionado se algum dos critérios abaixo for atendido:
1. ID do token filho é menor que o da raiz e compre algum critério abaixo:
    1. deprel "obj"
    2. deprel "dep"
    3. deprel "nummod"
    4. deprel "advmod"
    5. deprel "det"
    6. deprel "case"
    7. deprel "punct" e não é uma virgula (",")
2. Ou o ID do filho é maior e atende algum dos critérios abaixo:
    1. deprel "nummod"
    2. deprel "conj" e se o token é o núcleo de uma relação usando regras abaixo:
        1. postag "VERB"
        2. se algum token filho tiver deprel "cc" não pode ser nenhum dos valores "e", "ou", e ",".
        3. Não pode ter algum token filho com ID menor que o token verificado com deprel diferente de "punct" todos os tokens com ID entre eles deve respeitar os critérios:
            1. não ter nenhum dos postag "ADP" e "DET"
            2. não nenhum dos deprel "punct", "mark", "advmod", "aux", "cop" e "expl:pv"
    3. deprel "advmod"
    4. deprel "appos" e postag "NUM"
    5. deprel "nmod"
    6. deprel "amod"
    7. deprel "dep"
    8. deprel "obj" e ID do filho deve se maior que a token raiz da busca em profundidade
    9. deprel "det"
    10. deprel "case"
    11. deprel "punct" e é uma das pontuações:  "(", ")", "{", "}", """, "'", "\[", "]", '","
### Encontrar Relação
