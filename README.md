
Readme · MD
# Termo (Wordle em C)
 
Jogo de adivinhação de palavras de 5 letras inspirado no Termo/Wordle, feito em C puro. O programa sorteia uma palavra de uma lista fixa e dá feedback letra a letra até o jogador acertar.
 
## Funcionalidades
 
| Funcionalidade | Descrição |
|---|---|
| Banco de palavras | Lista com mais de 200 palavras de 5 letras em português |
| Sorteio aleatório | Escolhe a palavra secreta usando `rand()` com seed baseada em `time()` |
| Feedback por posição | Letra maiúscula = posição certa; minúscula = letra existe mas posição errada; `X` = letra não existe na palavra |
| Loop de tentativas | Repete a leitura até o jogador acertar a palavra exata |
 
## Sobre a organização do código
 
O programa inteiro roda dentro do `main()`, sem funções auxiliares — decisão razoável pra um jogo desse tamanho, já que não há reaproveitamento de lógica em múltiplos pontos do código nem necessidade de testar partes isoladamente.
 
A lógica de comparação usa dois laços aninhados por tentativa: um percorre a palavra digitada (`npalavra`) e, pra cada letra, um segundo laço percorre a palavra secreta (`palavra`) procurando uma correspondência. Essa abordagem é O(n²), mas como as palavras têm só 5 letras o custo é irrelevante — não compensaria complicar com estruturas como hash maps só pra ganhar performance que não faz diferença aqui.
 
A checagem de posição certa é feita primeiro (`npalavra[a] == palavra[a]`), e só quando ela falha o código entra no laço de busca por posição errada. Essa ordem evita marcar como "letra errada" uma letra que já está certa, e o `continue` corta a iteração assim que a posição certa é confirmada, evitando trabalho desnecessário.
 
O array `lista` é declarado como `char *lista[]` (array de ponteiros para string) em vez de uma matriz de char de tamanho fixo, o que evita desperdício de memória, já que cada palavra tem exatamente 5 caracteres mais o terminador nulo.
 
## Tratamento de erros
 
O código não valida se a entrada do jogador tem exatamente 5 letras nem se contém apenas caracteres alfabéticos. O `scanf("%11s", ...)` limita o tamanho do buffer pra evitar overflow, mas uma entrada mais curta que 5 letras (tipo "oi") quebra a lógica de comparação, já que os laços usam `strlen(npalavra)` como referência e vão gerar resultados incorretos ou comparações fora do esperado. Não há tratamento de acentuação na validação — o programa assume que a entrada já vem no formato certo (por isso a limitação de trabalhar só com palavras acentuadas fixas na lista).
 
## Aprendizados aplicados
 
- Diferença entre comparação de posição exata e busca por existência em outra posição, replicando a lógica real do Wordle
- Uso de `toupper`/`tolower` pra codificar informação (posição certa vs errada) usando a própria letra como indicador visual, sem precisar de estruturas extras
- Manipulação de arrays de strings com ponteiros (`char *lista[]`) e cálculo do tamanho de um array em tempo de compilação (`sizeof(lista) / sizeof(lista[0])`)
- Geração de números aleatórios com seed variável (`srand(time(NULL))`) pra evitar sortear sempre a mesma palavra
- Limitação do `scanf` com `%11s` como prevenção básica de buffer overflow