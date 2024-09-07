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


```sql



SELECT

    person_id,
    first_name,
    last_name,
    team_name,
    season_exp,
    height,
    weight,
    rosterstatus,
    from_year,
    to_year

FROM common_player_info

WHERE season_exp <> 0
ORDER BY
    season_exp DESC;


```

Resultado da query dos 5 primeiros resultados:
| person_id | first_name | last_name | team_name   | season_exp | weight | rosterstatus | years_of_experience | from_year | to_year |
|-----------|------------|-----------|-------------|------------|--------|--------------|----------------------|-----------|---------|
| 1713      | Vince      | Carter    | Raptors     | 23.0       | 220    | Inactive     | 21.0                 | 1998.0    | 2019.0  |
| 708       | Kevin      | Garnett   | Timberwolves| 22.0       | 240    | Inactive     | 20.0                 | 1995.0    | 2015.0  |
| 1717      | Dirk       | Nowitzki  | Mavericks   | 22.0       | 245    | Inactive     | 20.0                 | 1998.0    | 2018.0  |
| 788       | Kevin      | Willis    | Hawks       | 22.0       | 245    | Inactive     | 22.0                 | 1984.0    | 2006.0  |
| 305       | Robert     | Parish    | Celtics     | 21.0       | 244    | Inactive     | 20.0                 | 1976.0    | 1996.0  |

A query mostra um total de 3630 linhas, excluindo valores onde a experiencia de temporada é diferente de zero.

## E desses jogadores, quantos estão ativos e inativos?

Para responder a está pergunta, utiliza-se a query abaixo


```sql

SELECT
    active AS Player_Status,
    COUNT(*) AS Status_Count

FROM (
    SELECT
	is_active,
	full_name,
        (CASE 
            WHEN is_active = 1 THEN 'Player Active' ELSE 'Player Not Active'
        END) AS active
    FROM player
	ORDER BY
	    is_active DESC


) 
GROUP BY
    is_active

```

Tendo como resultado

| Player_Status    | Status_Count |
|------------------|--------------|
| Player Not Active| 4280         |
| Player Active    | 535          |


E então sabemos que apensa 12,5% dos jogadores listados ainda estão ativos. 



## Existe alguma tendência ou concentração de jogador por cidade?

Para ter o resultado acima, foi necessário realizar a seguinte consulta no SQL Server:


```sql
WITH city_count AS (
    SELECT 
        COALESCE(player1_team_city, 'Unknown') AS TeamCity,
        COUNT(DISTINCT player1_id) AS PlayerCount
    FROM play_by_play
    GROUP BY TeamCity
),
TotalCount AS (
    SELECT
        SUM(PlayerCount) AS Total_players
    FROM city_count
)

SELECT
    TeamCity,
    PlayerCount,
    ROUND((PlayerCount * 100.0 / Total_players), 2) AS percentage
FROM city_count, TotalCount
ORDER BY PlayerCount DESC;



```

Tendo como resultado da query (apenas os 10 primeiros valores)

| TeamCity         | PlayerCount | Percentage |
|------------------|-------------|------------|
| Unknown          | 1133        | 12.01%     |
| Los Angeles      | 408         | 4.33%      |
| Cleveland        | 292         | 3.10%      |
| Philadelphia     | 281         | 2.98%      |
| Atlanta          | 280         | 2.97%      |
| Dallas           | 279         | 2.96%      |
| Toronto          | 276         | 2.93%      |
| Milwaukee        | 275         | 2.92%      |
| Golden State     | 273         | 2.89%      |
| Houston          | 272         | 2.88%      |


Portanto, vemos que a maior parte dos jogadores concentra-se em cidadaes que não foram especificadas, indicando que talvez será preciso revisão de dados para fins de maior precisão analítica. 


## E quanto a concentração de times em relação as cidades? 


Podemos obter a resposta com a query:

```sql

SELECT

    city, 
    COUNT(DISTINCT nickname) AS [team_count]

FROM (
    SELECT 
        team_details.nickname, 
        CAST(yearfounded AS INTEGER) AS year_founded, 
        team_details.city, 
        team_details.arenacapacity
    FROM team_details
    JOIN team ON team.abbreviation = team_details.abbreviation
)

GROUP BY city
ORDER BY
    team_count DESC;


```


E obtemos a seguinte tabela dos 10 primeiros resultados:


| City           | Team_Count |
|----------------|------------|
| Los Angeles    | 2          |
| Utah           | 1          |
| Toronto        | 1          |
| Sacramento     | 1          |
| Portland       | 1          |
| Phoenix        | 1          |
| Philadelphia   | 1          |
| Orlando        | 1          |
| Oklahoma City  | 1          |
| New York       | 1          |

Vemos que existe uma distribuição uniforme dos times em relação a cidade, o que nos permite inferir que o esporte está distribuido uniformemente por todo o país.



## E quanto as vitórias e derrotas de cada time?

Para extrair esta informação podemos utilizar a query seguinte
```sql

WITH NbaTeamAnalysis AS (

    SELECT
        game.season_id AS [season],
        team.full_name AS [team],
        game.wl_home AS "result",
        team.year_founded,
        SUBSTR(game.matchup_home, LENGTH(game.matchup_home) - 2, 3) AS Played_Against
    FROM game
    JOIN team ON team.abbreviation = game.team_abbreviation_home

)
SELECT

    NbaTeamAnalysis.team,
    COUNT(CASE WHEN result = 'W' THEN 1 END) AS Wins,
    COUNT(CASE WHEN result = 'L' THEN 1 END) AS Losses,
    CAST(year_founded AS INTEGER) AS [year_founded]
	
FROM NbaTeamAnalysis

GROUP BY team, year_founded
ORDER BY Wins DESC;


```

E obtemos as vitórias e derrotas por cada time ordenado por número de vitória, bem como o ano de fundação

| Team                   | Wins | Losses | Year_Founded |
|------------------------|------|--------|--------------|
| Boston Celtics         | 2200 | 923    | 1946         |
| Los Angeles Lakers     | 1850 | 811    | 1948         |
| New York Knicks        | 1763 | 1164   | 1946         |
| Detroit Pistons        | 1522 | 1023   | 1948         |
| Phoenix Suns           | 1480 | 772    | 1968         |
| Chicago Bulls          | 1443 | 870    | 1966         |
| Milwaukee Bucks        | 1425 | 828    | 1968         |
| Portland Trail Blazers | 1422 | 723    | 1970         |
| Atlanta Hawks          | 1380 | 837    | 1949         |
| Houston Rockets        | 1369 | 802    | 1967         |
| Cleveland Cavaliers    | 1273 | 873    | 1970         |
| Denver Nuggets         | 1247 | 705    | 1976         |
| Indiana Pacers         | 1210 | 736    | 1976         |
| Washington Wizards     | 1195 | 924    | 1961         |
| Dallas Mavericks       | 1085 | 748    | 1980         |
| Miami Heat             | 949  | 596    | 1988         |
| San Antonio Spurs      | 869  | 341    | 1976         |
| Los Angeles Clippers   | 855  | 781    | 1970         |
| Sacramento Kings       | 843  | 709    | 1948         |
| Orlando Magic          | 828  | 599    | 1989         |
| Utah Jazz              | 786  | 364    | 1974         |
| Golden State Warriors  | 707  | 454    | 1946         |
| Minnesota Timberwolves | 682  | 708    | 1989         |
| Toronto Raptors        | 675  | 512    | 1995         |
| Philadelphia 76ers     | 630  | 517    | 1949         |
| Memphis Grizzlies      | 554  | 389    | 1995         |
| Oklahoma City Thunder  | 405  | 246    | 1967         |
| Charlotte Hornets      | 379  | 393    | 1988         |
| Brooklyn Nets          | 229  | 223    | 1976         |
| New Orleans Pelicans   | 229  | 198    | 2002         |



## Na tabela de jogador possui uma métrica de desempnho denominada "experiência por temporada", em inglês season experience. Dos times, quais são os times que estão no top 5 dessa métrica de desempenho?



```sql


WITH Players_list AS (

    SELECT
        common_player_info.person_id,
        common_player_info.first_name,
        common_player_info.last_name,
        common_player_info.birthdate,
        common_player_info.country,
        common_player_info.height,
        common_player_info.weight,
        common_player_info.season_exp,
        common_player_info.rosterstatus,
        common_player_info.team_name
    FROM common_player_info
)

SELECT 
    team_name,
    ROUND(AVG(season_exp), 2) AS season_xp_average_per_team
FROM Players_list
WHERE rosterstatus = 'Active'
GROUP BY team_name
ORDER BY 
    season_xp_average_per_team DESC
LIMIT 5


```

| Team Name   | Season XP Average Per Team |
|-------------|----------------------------|
| Clippers    | 7.77                       |
| Bucks       | 7.55                       |
| Mavericks   | 7.00                       |
| Heat        | 6.93                       |
| Warriors    | 6.67                       |


Dentre todos os jogadores que estão ativo, distribuidos entre os times, os cinco maiores times são Clippers, Bucks, Mavericks, Head e Warriors.

# Insights

![image alt](https://github.com/ryan-sampaio/SQL-Server-Analise-de-Dados-NBA/blob/main/ApresentacaoNBA/Slide1.JPG?raw=true)


![image alt](https://github.com/ryan-sampaio/SQL-Server-Analise-de-Dados-NBA/blob/main/ApresentacaoNBA/Slide2.JPG?raw=true)

![image alt](https://github.com/ryan-sampaio/SQL-Server-Analise-de-Dados-NBA/blob/main/ApresentacaoNBA/Slide3.JPG?raw=true)

![image alt](https://github.com/ryan-sampaio/SQL-Server-Analise-de-Dados-NBA/blob/main/ApresentacaoNBA/Slide4.JPG?raw=true)



