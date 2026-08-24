# Modelo do arquivo de build de um marco da Peneira.
#
# ESCRITO UMA VEZ, para a linguagem. Quem o preenche por marco e
# tools/gerar_marcos.exe (specs/marcos-executaveis.md). Os arquivos gerados a
# partir dele — marcos/NN/CMakeLists.txt — NAO se editam a mao: a edicao some na
# proxima geracao, e a lista de fontes deixa de corresponder ao marco.
#
# CUIDADO AO EDITAR ESTE MODELO: a substituicao dos marcadores alcanca o arquivo
# INTEIRO, comentario incluido. Citar um marcador aqui em cima, para explicar o
# que ele faz, injeta a lista de fontes dentro do comentario e quebra o parser —
# aconteceu na primeira versao deste arquivo.
cmake_minimum_required(VERSION 3.10.0)
project(peneira01 VERSION 0.1.0 LANGUAGES CXX)

# O padrao e declarado uma vez, aqui, e nao repetido por compilador.
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# Os fontes deste marco: os modulos 01 a 01, e mais nada. A lista e derivada,
# nunca escrita — e o que impede o capitulo 01 de exibir uma peca que so vai
# existir adiante.
add_executable(peneira01
    ../../01_linguagem.cpp
    ../../01_pipeline.cpp
    ../../demos/01_demo.cpp
)

# Aviso e erro. Incomoda no primeiro dia e economiza semanas depois — num programa
# que manipula indices de tabela o tempo inteiro, um aviso de conversao implicita
# ignorado e um defeito adiado, nao um defeito evitado.
if(MSVC)
    target_compile_options(peneira01 PRIVATE /W4 /WX /permissive- /utf-8 /EHsc)
else()
    target_compile_options(peneira01 PRIVATE -Wall -Wextra -Wpedantic -Werror)
endif()

include(CTest)
enable_testing()

# A demonstracao deste marco roda como teste, e o diretorio de trabalho e a raiz
# da variante: os arcos que leem descricoes de `exemplos/` dependem disso, e sem
# ele reprovariam por nao achar o arquivo — falha por motivo que nada tem a ver
# com o que a demonstracao mede.
add_test(NAME demo_01 COMMAND peneira01)
set_tests_properties(demo_01 PROPERTIES
    WORKING_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/../..")


---
Textos, fotos, artes e vídeos deste site estão protegidos pela legislação sobre direito autoral. Não reproduza o conteúdo em qualquer meio de comunicação, eletrônico ou impresso, sem autorização.