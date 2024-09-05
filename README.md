# Análise da Base de Dados da NBA

Este projeto tem como objetivo o estudo e análise estatística utilizando técnicas de visualização de dados, com o intuito de extrair insights valiosos e padrões da NBA. Além disso, serve como prática para o aprimoramento das minhas habilidades em SQL e estatística descritiva.


# Banco de Dados

O Banco de Dados da NBA possui cerca de 65 mil jogos registrado desde a temporada inaugural da NBA (1946-1947), mais de 4.800 jogadores e cerca de 30 times de basquete. Essa base abrange uma vasta coleção de dados históricos, como por exemplo, uma tabela especifica que detalha jogada a jogada (play-by-play) com mais de 13 milhões de linhas de dados detalhados.

# Dataset
 
Para o estudo de caso e entendimento de cada tabela, foi-se necessária uma query básica para compreender os valores e a estrutura dos dados presentes em cada tabela.

# Tabelas

O banco de dados possui 15 tabelas, sendo elas respectivamente: 

### common_player_info  

Contém informação sobre o jogador, tais como: número único da tabela (person_id), primeiro e último nome, data de nascimento, escola onde estudou, país de nascimento, altura, peso, experiência de temporada, posição no time dentre outras.

### draft_combine_stats

Contém valores como temporada, primeiro e último nome, posição em que joga, altura com e sem sapatos, peso dentre outras.

### draft_history

Contém informações sobre drafts (seleção de jogadores para partidas futuras)

### game

Contém informações sobre em qual temporada ocorreu determinada partida, id e abreviação do time, nome do time, contra quem jogou, resultado da partida, data em que ocorreu a partida bem como múltiplas colunas com escores não-arbitrários.

### game_info

Semelhante a tabela “game”, no entanto, só contém data, identificador e duração da partida, e número de pessoas que assistiram ao jogo presencialmente.

### game_summary

Semelhante a game ou game_info, essa tabela armazena valores como data e hora da partida, código da partida, código de time visitante e time da “casa”, temporada em que ocorreu a partida.

### inactive_players

Tabela que contém as informações sobre jogadores que atualmente estão inativos ou aposentados, como nome, time em que jogou, cidade do time, número da camisa.

### line_score

semelhante a tabela ‘play_by_play’, armazena valores sobre pontuações e seus critérios, além de armazenar informações sobre a partida, como tive, resultado, e identificadores de chaves estrangeira de outras tabelas.

### officials
compreende informações sobre os oficiais ou árbitros das partidas, tais como: nome completo, número da camisa, e número de identificador. 

### other_stats
Contém valores como, identificador de divisão e partida, cidade do e abreviação do time, e pontos realizados seguindo alguns critérios como número de tentativas e distância de saque.

### play_by_play

Esta tabela contém primariamente valores das jogadas, como em que partida ocorreu, tempo e duração da jogada, quem efeituou e a que time pertence. 

### player

Responsável por armazenar valores dos jogadores, tais como identificador, nome completo, primeiro e último nome, além do status se está ou não ativo.

### team

Contém informações como identificador do time, nome completo e abreviação, cidade, estado e ano de fundação.

### team_details

Contém as mesmas informações da tabela “team”, porém abranja mais detalhes como, o nome e capacidade da arena, bem como proprietário e administrador, técnico do time, filiação da temporada, e links de redes sociais.

### team_history

Contém informações de times, como nome, cidade, apelido e ano de fundação, bem como o ano de desativação.


# Analise dos Dados

No início da análise dos dados, focamos em explorar e entender as informações relativas aos jogadores, times e partidas.  A partir daí, implementamos análises mais aprofundadas para investigar padrões e extrair insights que pudessem oferecer uma compreensão mais detalhada do funcionamento da NBA.

## Jogadores


