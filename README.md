# Projeto Final: Sistema de Recomendação de Produtos por Similaridade Visual

**Autor:** *Michel Santos Rebouças*

**Curso:** *BairesDev - Machine Learning Training*

## Descrição do Projeto

Este notebook implementa um sistema de recomendação de produtos baseado em **filtragem de conteúdo por similaridade visual**. O objetivo é recomendar itens com base em sua aparência física (formato, cor, textura), em vez de metadados tradicionais (preço, marca, categoria textual).

Para isso, utilizamos técnicas de **Transfer Learning** para extrair características visuais (embeddings) de imagens de produtos. Em seguida, uma biblioteca de busca por vizinhos próximos (`Annoy`) é usada para indexar essas características e permitir consultas de similaridade em tempo real. Finalmente, uma aplicação web interativa é criada com `Streamlit` para demonstrar o sistema.

## Passo 1: Configuração do Ambiente e Carregamento dos Dados

Nesta primeira etapa, preparamos o ambiente do Google Colab para execução e carregamos nosso conjunto de imagens. As ações realizadas são:
1.  **Instalação de Bibliotecas:** Garantimos que todas as dependências necessárias, como `streamlit` e `annoy`, estejam instaladas.
2.  **Conexão com o Google Drive:** Montamos o Google Drive para garantir a persistência dos nossos dados (dataset, modelos salvos, índices). Isso evita a perda de arquivos importantes quando a sessão do Colab é encerrada.
3.  **Carregamento e Extração do Dataset:** Descompactamos nosso conjunto de imagens (arquivo `.zip` ou `.rar`) do Google Drive para o ambiente local do Colab, tornando-o pronto para o processamento.

## Passo 2: Vetorização das Imagens com Transfer Learning

Esta é a etapa central do projeto, onde transformamos informações visuais em dados numéricos que o computador pode comparar.

- **O que é Vetorização?** É o processo de converter uma imagem em um vetor (uma longa lista de números), também conhecido como *embedding*. Este vetor representa as características essenciais da imagem, como formas, cores e texturas.
- **Como funciona?** Usamos um poderoso modelo de Deep Learning pré-treinado (**Google BiT M-R50x3/1**) disponível no TensorFlow Hub. Como o modelo já foi treinado com milhões de imagens, ele "sabe" como extrair essas características de forma muito eficaz. Esse processo é chamado de **Transfer Learning**.

O código a seguir irá iterar sobre cada imagem do nosso dataset, passá-la pelo modelo e salvar o vetor de características resultante em uma nova pasta. Ao final, teremos um vetor para cada imagem.

## Passo 3: Indexação dos Vetores para Busca Rápida

Com milhares de vetores, comparar uma nova imagem com todas as outras do dataset uma por uma seria muito lento. Para resolver isso, precisamos de um sistema de busca otimizado.

- **Qual a solução?** Utilizamos a biblioteca `Annoy` (Approximate Nearest Neighbors Oh Yeah).
- **O que ela faz?** O Annoy constrói uma estrutura de dados chamada índice, que organiza os vetores de forma inteligente. Isso permite encontrar os vetores mais similares (os "vizinhos mais próximos") de forma quase instantânea, mesmo em um grande volume de dados.

Nesta etapa, o código irá carregar todos os vetores que criamos no passo anterior e construir este índice. O índice (`index.ann`) e um arquivo de mapeamento (que liga cada vetor ao seu nome de arquivo original) são salvos no Google Drive.

## Passo 4: Teste de Similaridade (Prova de Conceito)

Antes de construir a aplicação final, é fundamental verificar se nosso sistema de busca está funcionando como esperado. Este passo serve como uma prova de conceito rápida.

O código irá:
1.  Carregar o índice `Annoy` e o mapeamento salvos no Google Drive.
2.  Selecionar uma imagem aleatória do nosso dataset para usar como consulta.
3.  Utilizar o índice para encontrar as 5 imagens mais similares.
4.  Exibir a imagem de consulta e as imagens recomendadas, permitindo uma validação visual da qualidade das recomendações.

## Passo 5: Criação e Lançamento da Aplicação Web

Esta é a etapa final, onde unimos tudo em uma aplicação web interativa para que qualquer pessoa possa usar nosso sistema de recomendação.

### 5.1 - Escrevendo o Código da Aplicação com Streamlit

Utilizamos a biblioteca `Streamlit`, uma ferramenta poderosa para criar aplicações de dados de forma rápida e intuitiva. O código abaixo não é executado diretamente, mas sim usado para criar um arquivo `app.py` no ambiente do Colab.

Este script `app.py` contém toda a lógica da aplicação:
-   Carrega o índice `Annoy` e o modelo do TensorFlow.
-   Cria a interface do usuário com um título e um botão para upload de arquivos.
-   Quando um usuário envia uma imagem, ela é processada, vetorizada, e usada para consultar o índice.
-   As imagens mais similares são encontradas e exibidas na tela para o usuário.

### 5.2 - Lançando a Aplicação com Ngrok

Como nossa aplicação está rodando dentro do ambiente Colab, ela não é acessível publicamente. Para resolver isso, usamos o `Ngrok`.

-   **O que o Ngrok faz?** Ele cria um túnel seguro entre a nossa aplicação rodando localmente no Colab e um endereço web público (`...ngrok-free.app`).

O código a seguir irá iniciar o servidor Streamlit em segundo plano e, em seguida, gerar o link público para você acessar e testar a aplicação final em seu navegador.

#Obrigado!
