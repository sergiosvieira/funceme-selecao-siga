Instruções
==========
- Os projetos devem ser feitos no Visual Studio 2015 (preferencialmente) ou no qtcreator.
- Caso seu código não compile ele não será avaliado.
- Você será avaliado pela corretude da resposta, estilo de programação e desempenho.
- Prazo de envio: 22/05/2017 até às 08:00:00 da manhã.
- Enviar projetos para os 2 para os emails descobertos no primeiro desafio.

Atenção
=======
- ### C++11 NÃO é Java, nem C#  :rage4:
- ### Não programe em C++11 como se estivesse programando em C  :rage4:

### Resolva da melhor forma possível, considerando padrões, uso da memória, simplicidade, complexidade, melhores práticas e o que há de melhor no C++11 para resolver os problemas abaixo:
 
### 1) Em hidrologia, é comum existir grandes reservatórios de água distribuídos no Ceará. Cada reservatório possui uma capacidade máxima de armazenamento (hm^3) assim como um volume mínimo aceitável. Um dos problemas encontrados pelos gestores da água é como distribuí-la considerando as seguintes categorias: consumo humano, animal, industral etc. Uma solução encontrada, é fazer com que cada reservatório possua faixas de liberação de água conforme a prioridade escolhida para cada categoria em questão. `O propósito desta tarefa é desenvolver a lógica do método zonesAvailableSupply para que ele seja capaz de retornar um vetor de zonas de prioridades contendo a quantidade de água disponível em cada faixa de liberação`. 

Classe Priority Zone
====================
```c++
#ifndef __PRIORITY_ZONE__
#define __PRIORITY_ZONE__

/** priorityZone.h **/

class PriorityZone
{
  protected:
    double m_availableVolume;
    int m_priority;
  public:
    PriorityZone(double availableVolume, int priority);
    double getAvailableVolume();
    int getPriority();
}

/** priorityZone.cpp **/
#include "priorityZone.h"

PriorityZone::PriorityZone(double availableVolume, 
                           int priority) : m_availableVolume(availableVolume),
                                           m_priority(priority)
{
}

double PriorityZone::getAvailableVolume()
{
  return m_availableVolume;
}

int PriorityZone::getPriority()
{
  return m_priority;
}

#endif /* __PRIORITY_ZONE__ */
```
Classe Priority Utils
=====================
```c++
#ifndef __PRIORITY_UTILS__
#define __PRIORITY_UTILS__

#include "priorityZone.h"

using VectorOfPriorityZones = std::vector<PriorityZone>;

class PriorityUtils
{
  public:
  /** @brief
  * Retorna o volume de água disponível em cada zona de prioridade
  * @param pArray é um vetor contendo as prioridades de cada zona
  * @param vArray é o limite superior de cada zona
  * @param minimumVolume é o volume mínimo do reservatório
  * @param maximumVolume é o volume máximo do reservatório
  * @param currentVolume é o volume atual do reservatório
  */
  /** Crie aqui o método zonesAvailableSupply segundo as especificações logo acima **/
}

#endif /** __PRIORITY_UTILS__ **/
```
![alt tag](https://ibin.co/3LrZEp6nILk6)

Exemplo de Entrada
==================
```
pArray = [3, 1, 2]
vArray = [8000.0, 1000.0, 2500.0]
minimumVolume = 500.0
maximumVolume = 8000.0
currentVolume = 2650.0
```
Saída Esperada
==============
```
[
  {1, 500.0},
  {2, 1500.0},
  {3, 150.0},
]
```

### 2) Crie uma interface utilizando wxWidgets (versão 3.0 ou superior) para permitir ao usuário criar ou remover reservatórios (considerando as informações de volume máximo, mínimo e atual) além de poder preencher os valores das faixas de liberação de cada um deles. Para isto, você deverá criar uma classe para guardar as informações dos reservatórios, outra para armazenar a prioridade e limite superior da faixa e utilizar as classes desenvolvidas na questão anterior para mostrar em um messagebox ou em outra interface a quantidade de água disponível em cada faixa.


Atenção
=======
- Deve sempre existir a validação dos valores digitados pelo usuário.

Interface de Preenchimento de Volumes do Reservatório Selecionado
=================================================================
![alt tag](https://ibin.co/3LlIk8beIr3u.png)

Interface de Preenchimento das Zonas de Liberação do Reservatório Selecionado
=============================================================================

Atenção
=======
- A coluna limite inferior é calculada em tempo de execução.
- Os valores de prioridade devem estar sempre ordenados do menor para o maior.
- O Limite superior nunca pode ser menor ou igual ao limite superior da linha anterior.
- A coluna Prioridade deve aceitar apenas valores inteiros.
- O usuário pode inserir novas linhas no grid.
- O usuário pode remover sempre a última linha do grid, caso ela exista.
- Esta grid pode ser salva em um arquivo texto.
- Esta grid pode ser carregada de um arquivo texto.
- Os valores da grid devem ser persistidos em um vetor contendo uma classe que armazene a prioridade e o limite superior da faixa.
- Estes valores serão persistidos quando o usuário clicar na seta verde.

![alt tag](https://ibin.co/3LlJCY8lnyLg.png)

### 3) Crie uma estrutura para armazenar um grafo direcionado onde cada vértice deve possuir um identificador único e um valor de ponto flutuante associado a ele. Os vértices podem ser de 3 tipos diferentes (Fornecedor, Junção ou Cliente).

Quando o vértice for:
1) Fornecedor, o valor de ponto flutuante associado é a quantidade de produto que ele pode entregar a um cliente.
2) Junção, ele deve repassar igualmente o valor recebido pelo fornecer para os clientes ligados a ele.
3) Cliente, o valor de ponto flutuante associado é a quantidade de produto que ele deve pedir a um fornecedor ligado a ele. O cliente sempre possuirá a mesma demanda.
4) Um Fornecedor pode estar ligado (-->--) somente a um cliente ou uma junção.
5) Uma junção sempre deve estar ligada a um fornecedor e a um cliente.
6) Uma junção não pode estar ligada a uma junção
6) Somente existirá um único fornecedor
7) Poderá existir uma ou mais junções
8) Poderá existir um ou mais clientes

Ligações Válidas
===================
```
[F]-->--[J]-->--[C]

[F]-->--[C]

[F]-->--[J]-->--[C]
         |
         v
         |
        [C]
[C]
 |
 ^
 |
[F]-->--[J]-->--[C]
         |
         v
         |
        [C]
```
Crie um método para contar em quantas passos de tempo o fornecedor será capaz de entregar os produtos para seus clientes.

Exemplo de Entrada
==================

Atenção - O grafo não precisa necessariamente ser carregado de um arquivo texto. Você pode criar os vértices e arestas dentro do seu programa.

```
[F]-->--[C1]
F(1) = 10 - Quantidade disponível no fornecedor F no tempo um
C1 = 3 - Quantidade pedida por C1
```
Exemplo de Saída
================
```
Passo 1
=======
F(1) = 10
F libera 3 para C1
Passo 2
=======
F(2) = 7
F libera 3 para C1
Passo 3
=======
F(3) = 4
F libera 3 para C1
Passo 4
=======
F(4) = 1
F não consegue mais liberar para C1

Quantidade de vezes que F foi capaz de liberar produtos para C1 = 3 vezes
```









