                ._,.
            ."..-..pf
            -L   ..#'
          .+_L  ."]#
          ,'j' .+.j`                 -'.__..,.,p.
         _~ #..<..0.                 .J-.``..._f.
        .7..#_.. _f.                .....-..,`4'
        ;` ,#j.  T'      ..         ..J....,'.j`
       .` .."^.,-0.,,,,yMMMMM,.    ,-.J...+`.j@
      .'.`...' .yMMMMM0M@^=`""g.. .'..J..".'.jH
      j' .'1`  q'^)@@#"^".`"='BNg_...,]_)'...0-
     .T ...I. j"    .'..+,_.'3#MMM0MggCBf....F.
     j/.+'.{..+       `^~'-^~~""""'"""?'"``'1`
     .... .y.}                  `.._-:`_...jf
     g-.  .Lg'                 ..,..'-....,'.
    .'.   .Y^                  .....',].._f
    ......-f.                 .-,,.,.-:--&`
                              .`...'..`_J`
                              .~......'#'  Ray Brunner
                              '..,,.,_]`
                              .L..`..``.

Instruções
==========
- Os projetos devem ser feitos no Visual Studio 2015 (preferencialmente) ou no qtcreator.
- Caso seu código não compile ele não será avaliado.
- Você será avaliado tanto pela corretude da resposta quanto pelo estilo de programação.
- Prazo de envio: 21/12/2016 até às 08:00:00 da manhã.
- Enviar projetos (questão 1 e 3) e resposta da questão 2 para os emails descobertos no primeiro desafio.
- C++11 não é Java, nem C# :)
- Não programe em C++11 como se estivesse programando em C.

### Resolva da melhor forma possível, considerando padrões, uso da memória, simplicidade, complexidade, melhores práticas e o que há de melhor no C++11 para resolver os problemas abaixo:

* 1) Implemente uma solução que modele uma Matriz capaz de satisfazer as seguintes restrições:
	* Ser capaz de lidar com qualquer tipo de valor (bool, int, double, float, etc).
	* Ser capaz de adicionar/excluir linhas e colunas da matriz.
	* Economizar uso de memória.
	* Ser capaz de incluir/retornar/modificar valores da matriz.
	* Ser capaz de somar valores de uma determinada linha ou coluna da matriz.
	* Salvar/Carregar matriz de arquivo texto.

* 2) Descreva da forma mais detalhada possível o que o código abaixo realiza e como ele fez isto.

```C++
#include <algorithm>
#include <cstdlib>
#include <fstream>
#include <initializer_list>
#include <iostream>
#include <locale>
#include <string>
#include <vector>
#include <iso646.h>

template<class C>
struct text : std::basic_string<C>
{
    text() : text{""} {}
    text(char const* s) : std::basic_string<C>(s) {}
    text(text const&) = default;
    text& operator=(text const&) = default;
};

template<class Ch>
auto read(std::basic_istream<Ch>& in) -> std::vector<text<Ch>>
{
    std::vector<text<Ch>> result;
    text<Ch> line;
    while (std::getline(in, line))
    {
        result.push_back(line);
    }
    return result;
}

int main(int argc, char* argv[])
{
    try
    {
        std::cin.exceptions(std::ios_base::badbit);
        std::vector<text<char>> text; ///< Store the lines of text here
        if (argc < 2)
        {
            std::exit(EXIT_FAILURE);
        }
        else
        {
            std::ifstream in(argv[1]);
            if (not in)
            {
                std::perror(argv[1]);
                return EXIT_FAILURE;
            }
            text = read(in);
        }
        std::locale const& loc{ std::locale(argc >= 3 ? argv[2] : "") };
        std::collate<char> const& collate( std::use_facet<std::collate<char>>(loc) );
        std::sort(text.begin(), text.end(), [&collate](std::string const& a, std::string const& b)
        {
            return collate.compare(a.data(), a.data()+a.size(),
            b.data(), b.data()+b.size()) < 0;
        });
        for (auto const& line : text)
        {
            std::cout << line << '\n';
        }
    }
    catch (std::exception& ex)
    {
        std::cerr << "Caught exception: " << ex.what() << '\n';
        std::cerr << "Terminating program.\n";
        std::exit(EXIT_FAILURE);
    }
    catch (...)
    {
        std::cerr << "Caught unknown exception type.\nTerminating program.\n";
        std::exit(EXIT_FAILURE);
    }
    return 0;
}
```

* 3) Implemente uma estrutura de dados do tipo árvore com as seguintes características:
	* Possua 3 níveis.
	* O primeiro nível deve armazenar um ano qualquer.
	* O segundo nível deve armazenar os meses daquele ano.
	* O terceiro nível deve armazenar os dias de um determinado mês.
	* Cada nó pai deve possuir o somatório dos valores de cada coluna identificada contida em seus filhos. Isto ficará mais claro no exemplo mais abaixo.
	* Cada nível deve possuir uma estrutura capaz armazenar os valores associados a cada coluna identificada.
	* Ser capaz de preencher/modificar valores a partir de uma data e um índice
	* Ser capaz de retornar os valores associados a cada (ano, índice)
	* Ler/Salvar os dados em arquivo texto.	
	
### Visualização de um exemplo da árvore

```
					    1978
	jan						    fev			         ...	 dez
1 2 3 4 5 ... 31		1 2 3 4 5 ...28		     1 2 3 ... 31
```

### A árvore deve ler um arquivo de entrada do tipo

```
		110	 111	112 <- Identificador de valores de cada coluna de uma determinada data
01/01/1978	1.0	 2.0	3.0
02/01/1978	4.0	 5.0	6.0

01/02/1978	7.0	 8.0	9.0
02/02/1978	10.0     11.0	12.0

01/03/1978	13.0     14.0	15.0
02/03/1978	16.0     17.0	18.0
```

### E preencher a árvore para que ela fique da seguinte forma

![alt tag](https://ibin.co/35Zx0hmmJywC.png)


