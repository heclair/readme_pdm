01. Coleta e Limpeza dos Textos

Para elaboração do treinamento da api de fake news foi utilizado a base de dados Fake.br-Corpus a qual já é disponibilizada pré-processada, ou seja, classificada com labels de true ou false. 
Importante destacar que esta base possui um volume total de 7.000 notícias, mas que foi incrementada de forma manual com mais 1.000 frases curtas do tipo "água é saudável", "frutas fazem mal a saúde" e diversas variações para incremento do dataset e melhora da generalização de frases curtas, houve também um segundo incremento de mais 1.000 frases com teor etinico e social do tipo "homossexuais não devem ter direitos" e "homossexuais devem possuir os mesmos direitos" para que o modelo pudesse compreender outros tipos de contexto.

Processo de Limpeza e Processamento

Para iniciar o projeto é necessário a remoção de pontuação, números e símbolos especiais, espaços em branco duplicados e susbstituição de URL e menções como "@pessoa"
Abaixo temos um exemplo:

"URGENTE: Vacina pode causar efeitos colaterais graves, dizem usuários do Twitter!!! #vacina #perigo https://t.co/abc123"

urgente vacina causar efeito colateral grave dizer usuario twitter vacina perigo

Esses textos limpos foram utilizados para gerar os vetores TF-IDF e também para criação de embeddings com BERT, garantindo uma base de entrada robusta e consistente para o treinamento dos modelos.

02. Vetorização dos Textos

Primeiro precisamos definir o que é a vetorização de texto, para isto é necessário entender que uma máquina não é capaz de interpretar um texto diretamente, ou seja, é necessário uma conversão de letrasp ara números, isto é a criação de vetores, que podem ser processados por algoritmos de aprendizado de máquina.


O método utilizado pelo projeto é o TF-IDF que representa os textos com base na frequência de palavras, ponderando termos comuns e raros

Abaixo temos um exemplo de vetorização com TF-IDF:

Texto

"água faz mal à saúde"

Vetor(fictício)
[0.0, 0.23, 0.15, 0.0, 0.42, ..., 0.0]


No projeto elaborado api_fake_news os embeddings utilizam o modelo BERTimbau onde os vetores são codificados em 768 dimensões:

Exemplo com BERT:

Texto:
"vacinas salvam vidas"

Vetor resultante:
[ 0.0123, -0.5567, ..., 0.1945 ]  ← vetor com 768 números


3. Treinamento do Modelo

Como uma boa prática o dataset foi dividido entre treino/teste, ou seja, utilizamos a métrica de 80% para treino e 20% para teste. Esta divisão foi feita utilizando a função train_test_split() do Scikit-learn.

O algorítmo utilizado foi o de Regressão Logística que é eficiente e interpretável, ou seja, comumente utilizado para classificação binária, onde ele:

a. Calcula a probabilidade de um exemplo permanecer a classe (fake ou real).
b. Utiliza uma função para produzir o resultado entre 0 e1.
c. A classe com maior probabilidade é escolhida como o resultado final

4. Avaliação dos Modelos

Para validar a proporção total de previsões corretas utilizamos a métrica de acurácia, ou seja, uma acurácia alta indica um resultado positivo, mas não é necessariamente real, devido ao conceito de overfitting ou dataset desbalanceado e por este motivo houve a inserção manual de novos itens no dataset original Fake.br-Corpus

A precisão é utilizada para medir os falsos positivos, ou seja, quando diz que uma notícia é falsa, geralmente está certo.

O recall é uma mátrica para acompanhar or positivos reais que o modelo conseguiu identificar corretamente.

abaixo temos as métricas

              precision    recall  f1-score   

        true       0.88      0.90      0.89       
        fake       0.89      0.87      0.88       

    accuracy                           0.88       
   macro avg       0.88      0.88      0.88       
weighted avg       0.88      0.88      0.88       


