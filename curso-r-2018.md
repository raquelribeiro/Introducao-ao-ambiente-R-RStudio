Introdução ao ambiente R e RStudio
========================================================
author: Prof. Dr. Caio Maximino, Profa. Dra. Raquel Ribeiro da Silva
date: 28-Abr-2018
autosize: true

<style>
.small-code pre code {
  font-size: 1em;
}
</style>


Segunda parte: Aplicações
========================================================

Esses slides foram construídos usando o RStudio!

- Estatística descritiva e representação gráfica exploratória
- Análise estatística básica: Testes T, ANOVA, qui-quadado
- Representação gráfica avançada com ggplot2

Estatística descritiva
========================================================

- Etapa inicial da análise, utilizada para descrever e resumir os dados.
- Útil para termos uma visão inicial, *exploratória* dos dados.
- Útil também para inferirmos erros de transcrição de dados.

Medidas de resumo
========================================================

- Medidas de posição: Moda, Média, Mediana, Percentís, &c.
- Medidas de dispersão: Amplitude, Intervalos Inter-quartil, Variância, Desvio padrão, Intervalos de confiança

Baixando o nosso conjunto de dados
========================================================
Vamos verificar se temos a biblioteca necessária, instalá-la se for o caso, e abri-la


```r
if(!require(RCurl)){
    install.packages("RCurl")
    library(RCurl)
}
```

Para quem está sem Internet...
========================================================
- Crie uma pasta no Desktop chamada "RPacks"

{r}
if(!require(RCurl)){
    install.packages("RCurl", lib ="C:\users\seuusuario\Desktop")
    library(RCurl)
}


Agora vamos baixar os dados do repositório
========================================================

```r
x1 <- getURL("https://raw.githubusercontent.com/raquelribeiro/Introducao-ao-ambiente-R-RStudio/master/6.%20CursoR.csv")
exp1 <- read.csv(text = x1)
View(exp1)
```

Algumas funções
========================================================

```r
mean(exp1$Corpo)
```

```
[1] 87.38462
```

```r
sd(exp1$Corpo)
```

```
[1] 15.02912
```

Parece que simplesmente usar a função não nos ajuda muito analisar dados organizados em data.frame...

Algumas funções
========================================================
Existem muitas soluções para esse problema, a maioria no StackOverflow (p. ex., http://tiny.cc/znb4sy). Iremos adotar uma que usa a função aggregate

```r
aggregate(Corpo ~ Especie, exp1, mean)
```

```
             Especie    Corpo
1            Calomys 79.28571
2       Gracilinanus 81.12500
3           Necromys 99.45455
4 Thalpomys lasiotis 85.38462
```

Algumas funções
========================================================

```r
aggregate(Corpo ~ Especie, exp1, sd)
```

```
             Especie    Corpo
1            Calomys 13.07305
2       Gracilinanus 19.08206
3           Necromys 14.28540
4 Thalpomys lasiotis  6.71489
```

Agora façam o mesmo para média e desvio-padrão por sexo!

Observando interações
========================================================
Mas é provável que o tamanho do corpo não seja igual para machos e fêmeas de espécies diferentes (i.e., há _interação_). Como fazemos?

```r
aggregate(Corpo ~ Especie*Sexo, exp1, mean)
```

```
             Especie Sexo     Corpo
1            Calomys    F  69.75000
2       Gracilinanus    F  77.00000
3           Necromys    F  94.16667
4 Thalpomys lasiotis    F  81.85714
5            Calomys    M  92.00000
6       Gracilinanus    M  83.60000
7           Necromys    M 105.80000
8 Thalpomys lasiotis    M  89.50000
```

Observando interações
========================================================

```r
aggregate(Corpo ~ Especie*Sexo, exp1, sd)
```

```
             Especie Sexo     Corpo
1            Calomys    F  5.123475
2       Gracilinanus    F 12.529964
3           Necromys    F 17.092884
4 Thalpomys lasiotis    F  6.914443
5            Calomys    M  7.000000
6       Gracilinanus    M 23.201293
7           Necromys    M  7.259477
8 Thalpomys lasiotis    M  3.619392
```

Agora façam o mesmo para tamanho de cauda e tamanho de pata!

Outras funções descritivas
========================================================
Para amplitude:
- min()
- max()

Para quartis:
- quantile(x, c(0.25, 0.5, 0.75))

Extraiam valores mínimo, máximo, e quartis por espécie e sexo

A função summary()
========================================================
class: small-code

```r
aggregate(Corpo ~ Especie*Sexo, exp1, summary)
```

```
             Especie Sexo Corpo.Min. Corpo.1st Qu. Corpo.Median Corpo.Mean
1            Calomys    F   63.00000      67.50000     70.50000   69.75000
2       Gracilinanus    F   64.00000      71.00000     78.00000   77.00000
3           Necromys    F   70.00000      84.50000     93.00000   94.16667
4 Thalpomys lasiotis    F   72.00000      76.50000     83.00000   81.85714
5            Calomys    M   84.00000      89.50000     95.00000   92.00000
6       Gracilinanus    M   64.00000      71.00000     76.00000   83.60000
7           Necromys    M   98.00000      99.00000    107.00000  105.80000
8 Thalpomys lasiotis    M   84.00000      88.25000     89.50000   89.50000
  Corpo.3rd Qu. Corpo.Max.
1      72.75000   75.00000
2      83.50000   89.00000
3     109.00000  113.00000
4      87.50000   90.00000
5      96.00000   97.00000
6      84.00000  123.00000
7     110.00000  115.00000
8      90.75000   95.00000
```

Gráficos exploratórios
========================================================
- Faremos uma distinção não muito formal entre "gráficos exploratórios" e "gráficos finais"
- A função dos gráficos exploratórios é, basicamente, checar os dados
- Boxplots, histogramas, gráficos de barras, gráficos de densidade

Gráficos exploratórios: Boxplots para uma única variável
========================================================
- Representação visual do sumário de cinco pontos; úteis para encontrar outliers
![plot of chunk unnamed-chunk-9](curso-r-2018-figure/unnamed-chunk-9-1.png)

Gráficos exploratórios: Histogramas para uma única variável
========================================================
- Representação visual da distribuição
![plot of chunk unnamed-chunk-10](curso-r-2018-figure/unnamed-chunk-10-1.png)

Gráficos exploratórios: Boxplots para interações
========================================================
![plot of chunk unnamed-chunk-11](curso-r-2018-figure/unnamed-chunk-11-1.png)

Gráficos exploratórios: Scatterplots
========================================================
- Representação visual da relação entre duas ou mais variáveis
![plot of chunk unnamed-chunk-12](curso-r-2018-figure/unnamed-chunk-12-1.png)

Gráficos exploratórios: Scatterplots
========================================================
- Podemos usar a cor para separar o gráfico em categorias
![plot of chunk unnamed-chunk-13](curso-r-2018-figure/unnamed-chunk-13-1.png)

Análise estatística básica
========================================================
Vamos analisar brevemente a sintaxe dos testes mais utilizados:
- Teste T de Student
- ANOVA de uma e duas vias
- Qui-quadrado

Análise estatística básica: Testes T
========================================================
class: small-code
Sintaxe básica: t.test(VI ~ VD, data = data)

```r
t.test(Cauda ~ Sexo, data = exp1)
```

```

	Welch Two Sample t-test

data:  Cauda by Sexo
t = -1.1536, df = 27.639, p-value = 0.2586
alternative hypothesis: true difference in means is not equal to 0
95 percent confidence interval:
 -29.010082   8.115345
sample estimates:
mean in group F mean in group M 
       72.50000        82.94737 
```

Análise estatística básica: ANOVA de uma via
========================================================
class: small-code
- Sintaxe básica: aov(VI ~ VD, data = data);
- Usar summary para ver a tabela de ANOVA

```r
oneway.anova <- aov(Cauda ~ Especie, data = exp1)
oneway.anova
```

```
Call:
   aov(formula = Cauda ~ Especie, data = exp1)

Terms:
                  Especie Residuals
Sum of Squares   7437.354 22372.081
Deg. of Freedom         3        35

Residual standard error: 25.28245
Estimated effects may be unbalanced
```
- Lembre-se que a seta <- cria um objeto; é esse objeto que vamos "sumarizar"

Análise estatística básica: ANOVA de uma via
========================================================
class: small-code
- Sintaxe básica: aov(VI ~ VD, data = data);
- Usar summary para ver a tabela de ANOVA

```r
summary(oneway.anova)
```

```
            Df Sum Sq Mean Sq F value Pr(>F)  
Especie      3   7437  2479.1   3.878 0.0171 *
Residuals   35  22372   639.2                 
---
Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

Análise estatística básica: ANCOVA de uma via
========================================================
class: small-code
Sintaxe básica: aov(VI ~ VD + co-variável, data = data)

```r
oneway.ancova <- aov(Cauda ~ Especie + Corpo, data = exp1)
oneway.ancova
```

```
Call:
   aov(formula = Cauda ~ Especie + Corpo, data = exp1)

Terms:
                  Especie     Corpo Residuals
Sum of Squares   7437.354  1924.926 20447.156
Deg. of Freedom         3         1        34

Residual standard error: 24.52319
Estimated effects may be unbalanced
```

Análise estatística básica: Pós-teste de Tukey
========================================================
class: small-code
Sintaxe básica: TukeyHSD(modelo)

```r
TukeyHSD(oneway.anova)
```

```
  Tukey multiple comparisons of means
    95% family-wise confidence level

Fit: aov(formula = Cauda ~ Especie, data = exp1)

$Especie
                                     diff        lwr       upr     p adj
Gracilinanus-Calomys             39.67857   4.389837 74.967306 0.0225078
Necromys-Calomys                  5.61039 -27.356301 38.577081 0.9674177
Thalpomys lasiotis-Calomys       12.89011 -19.075189 44.855409 0.6993346
Necromys-Gracilinanus           -34.06818 -65.750717 -2.385646 0.0310724
Thalpomys lasiotis-Gracilinanus -26.78846 -57.427663  3.850740 0.1044200
Thalpomys lasiotis-Necromys       7.27972 -20.653568 35.213009 0.8952703
```

Análise estatística básica: ANOVA de duas vias
========================================================
class: small-code
Sintaxe básica: aov(VI ~ VD1*VD2, data = data)

```r
twoway.anova <- aov(Cauda ~ Especie*Sexo, data = exp1)
twoway.anova
```

Análise estatística básica: ANOVA de duas vias
========================================================
class: small-code
Sintaxe básica: aov(VI ~ VD1*VD2, data = data)

```r
twoway.anova <- aov(Cauda ~ Especie*Sexo, data = exp1)
twoway.anova
```

```
Call:
   aov(formula = Cauda ~ Especie * Sexo, data = exp1)

Terms:
                  Especie      Sexo Especie:Sexo Residuals
Sum of Squares   7437.354   433.825     1790.723 20147.533
Deg. of Freedom         3         1            3        31

Residual standard error: 25.49354
Estimated effects may be unbalanced
```

Análise estatística básica: ANOVA de duas vias
========================================================
class: small-code
- Sintaxe básica: aov(VI ~ VD1*VD2, data = data);
- Usar summary para ver a tabela de ANOVA

```r
summary(twoway.anova)
```

Análise estatística básica: ANOVA de duas vias
========================================================
class: small-code
- Sintaxe básica: aov(VI ~ VD1*VD2, data = data);
- Usar summary para ver a tabela de ANOVA

```r
summary(twoway.anova)
```

```
             Df Sum Sq Mean Sq F value Pr(>F)  
Especie       3   7437  2479.1   3.814 0.0195 *
Sexo          1    434   433.8   0.668 0.4202  
Especie:Sexo  3   1791   596.9   0.918 0.4434  
Residuals    31  20148   649.9                 
---
Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```
Análise estatística básica: Qui-Quadrado
========================================================
class: small-code
- Sintaxe básica: chisq.test(tbl);
- Vamos abrir conjuntos de dados para treino do R

```r
if(!require(MASS)){
    install.packages("MASS")
    library(MASS)
}
```

Análise estatística básica: Qui-Quadrado
========================================================
class: small-code
- Sintaxe básica: chisq.test(tbl);
- Vamos abrir conjuntos de dados para treino do R

```r
tbl <- table(survey$Smoke, survey$Exer) 
tbl
```

```
       
        Freq None Some
  Heavy    7    1    3
  Never   87   18   84
  Occas   12    3    4
  Regul    9    1    7
```

Análise estatística básica: Qui-Quadrado
========================================================
class: small-code
- Sintaxe básica: chisq.test(tbl);
- Vamos abrir conjuntos de dados para treino do R

```r
chisq.test(tbl)
```

```

	Pearson's Chi-squared test

data:  tbl
X-squared = 5.4885, df = 6, p-value = 0.4828
```

Gráficos avançados no ggplot2
========================================================
- Certamente, um dos maiores atrativos do R é a versatilidade, incluindo a capacidade de fazer gráficos fantásticos
- O pacote ggplot2 representa uma "gramática de gráficos estatísticos"
- Construções iterativas no começo; uma vez estabelecidos os resultados, vale a pena usar scripts
- A maioria do que falaremos aqui é baseada em https://analisereal.com/2015/09/19/introducao-ao-ggplot2/

A gramática dos gráficos
========================================================
- Um gráfico estatístico, nessa lógica, é um mapeamento dos dados para propriedades
  - Estéticas (cor, forma, tamanho)
  - Geométricas (pontos, linhas, barras)
- O gráfico pode conter transformações estatísticas
- A combinação de todas essas camadas forma o gráfico

Instalando os pacotes
========================================================
A primeira coisa a fazer, obviamente, é instalar os pacotes necessários

```r
if(!require(ggplot2)){
    install.packages("ggplot2")
    library(ggplot2)
}
```

Um scatterplot
========================================================
class: small-code
Vejamos um exemplo simples de scatterplot com os dados do repositório

```r
ggplot(data = exp1, aes(x = Corpo, y = Cauda)) + geom_point()
```

![plot of chunk unnamed-chunk-27](curso-r-2018-figure/unnamed-chunk-27-1.png)

Um scatterplot
========================================================
- A função ggplot() inicializa um gráfico com os seguintes parâmetros:
 - data: a que dados estamos nos referindo
 - aes: os elementos _estéticos_ que estamos mapeando
  - Mapear o eixo x na variável "Corpo" e o eixo y na variável "Cauda"
 - geom_point(), com o qual dizemos para o ggplot que queremos adicionar pontos como objeto geométrico
  - Experimentem agora rodar o código anterior _sem_ geom_point()

Complexificando os parâmetros estéticos
========================================================
- Vamos agora mexer com outros parâmetros estéticos, incluindo outras variáveis no nosso gráfico
- Para exemplificar, vamos mapear espécie na cor e sexo na forma dos pontos

```r
ggplot(data = exp1, aes(x = Corpo, y = Cauda, color = Especie, shape = Sexo)) + geom_point()
```

![plot of chunk unnamed-chunk-28](curso-r-2018-figure/unnamed-chunk-28-1.png)

Parâmetros geométricos: Pontos, retas, boxplots, regressões
========================================================
- O ggplot2 vem com vários geoms diferentes, para fazer diferentes gráficos
 - geom_point() - scatterplot
 - geom_bar() - gráfico de barras
 - geom_boxplot() - boxplot
 - geom_line() - gráfico de linhas
 - geom_histogram() - histograma
 - geom_density() - gráfico de densidade
 - geom_smooth() - aplica modelo estatístico
 
Um boxplot como exemplo
========================================================

```r
ggplot(data = exp1, aes(x = Especie, y = Corpo)) + geom_boxplot(aes(fill = Sexo))
```

Um boxplot como exemplo
========================================================

```r
ggplot(data = exp1, aes(x = Especie, y = Corpo)) + geom_boxplot(aes(fill = Sexo))
```

![plot of chunk unnamed-chunk-30](curso-r-2018-figure/unnamed-chunk-30-1.png)

Combinando estética e geometria
========================================================
- Os gráficos do ggplot2 são construídos em etapas, de maneira iterativa
- Toda informação que você passa dentro do comando inicial ggplot() é repassada para os geoms() seguintes
- As estéticas que você mapeia dentro dos geoms valem apenas para aquele geom especificamente. 

Combinando estética e geometria
========================================================
- Compartilhando a aes() cor

```r
ggplot(data = exp1, aes(x = Corpo, y = Cauda, color = Sexo)) + geom_point() + geom_smooth(method="lm")
```

Combinando estética e geometria
========================================================
- Compartilhando a aes() cor
![plot of chunk unnamed-chunk-32](curso-r-2018-figure/unnamed-chunk-32-1.png)

Combinando estética e geometria
========================================================
- Mas e se você quiser uma linha de regressão única?

```r
ggplot(data = exp1, aes(x = Corpo, y = Cauda)) + geom_point(aes(color = Sexo)) + geom_smooth(method="lm")
```

Combinando estética e geometria
========================================================
- Mas e se você quiser uma linha de regressão única?
![plot of chunk unnamed-chunk-34](curso-r-2018-figure/unnamed-chunk-34-1.png)

Adicionando facetas
========================================================
- No ggplot2, você pode dividir o gráfico em diversos sub-gráficos usando variáveis categóricas

```r
ggplot(data = exp1, aes(x = Corpo, y = Cauda)) + geom_point(aes(color = Sexo)) + geom_smooth(method="lm") + facet_wrap(~Especie)
```

Adicionando facetas
========================================================
- No ggplot2, você pode dividir o gráfico em diversos sub-gráficos usando variáveis categóricas
![plot of chunk unnamed-chunk-36](curso-r-2018-figure/unnamed-chunk-36-1.png)

Personalizando o gráfico
========================================================
- No ggplot2 é possível fazer o ajuste fino de vários elementos do gráfico

```r
plot1 <- ggplot(data = exp1, aes(x = Corpo, y = Cauda)) + geom_point(aes(color = Sexo)) + geom_smooth(method="lm") + facet_wrap(~Especie)
plot1
plot1 + xlab("Tamanho do corpo (cm)") + ylab("Comprimento da cauda (cm)") + ggtitle("Gráfico bacanudo") + theme_bw()
```

Personalizando o gráfico
========================================================
![plot of chunk unnamed-chunk-38](curso-r-2018-figure/unnamed-chunk-38-1.png)![plot of chunk unnamed-chunk-38](curso-r-2018-figure/unnamed-chunk-38-2.png)

Fim
========================================================
Obrigado!
