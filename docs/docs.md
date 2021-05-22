# Robótica Móvel


## Aula 01 - Introdução e Conceitos Gerais

Alguns conceitos importantes para conhecer antes de aprofundar-se na robótica:

- A história da robótica 
    > [Intro. à Robótica - História - UFMG](https://homepages.dcc.ufmg.br/~doug/cursos/lib/exe/fetch.php?media=cursos:introrobotica:2017-1:aula2-historia.pdf), [Vídeo sobre a história da robótica](https://www.youtube.com/watch?v=8jV0JADp7Vs)
- Sensores 
    > [Sensores e Atuadores](https://www.feis.unesp.br/Home/departamentos/engenhariaeletrica/aula-5---sensor-25-04-2013b.pdf)
- Meios de locomoção 
    > [Intro. à Robótica - Locomoção - UFMG](https://homepages.dcc.ufmg.br/~doug/cursos/lib/exe/fetch.php?media=cursos:introrobotica:2019-1:aula13-locomocao.pdf)

## Aula 02 - Transformações Homogêneas

Conceitos importantes relacionados ao movimento dos robôs:
- Cinemática
- Sistemas de coordenadas (frames)
	- Movimentação: translação e rotação
		- Translação: 
		$$x_{(1)}=x_{(0)}+\color{red}\Delta x$$
        $$y_{(1)}=y_{(0)}+\color{lime}\Delta y$$
		$$\begin{bmatrix} x_{(1)}\\x_{(2)}\\1 \end{bmatrix}= \begin{bmatrix} 1&0&\Delta x\\0&1&\Delta y\\0&0&1 \end{bmatrix}$$ $$\begin{bmatrix} x_{(1)}\\x_{(2)}\\1 \end{bmatrix}$$ Substitui-se a quantidade a se transladar em $$\Delta x$$ e $$\Delta y$$ 

## Aula 03 - Modelo Cinemático Diferencial

- A teoria com a prática deve estar bem fundamentada. As convenções (ex.: [Regra da mão Direita](https://pt.khanacademy.org/science/physics/discoveries/electromagnet/a/right-hand-rule), [Denavit-Hartenberg](https://pt.wikipedia.org/wiki/Par%C3%A2metros_de_Denavit-Hartenberg), etc.) devem ser adotadas por todos na equipe, para que todas as partes dos projetos possam se comunicar quando forem juntadas.

- No Modelo Diferencial, os robôs possuem duas rodas motorizadas de maneira independente
- A representação de um robô no Modelo Diferencial é realizada através da visão de planta baixa (visão 2D de cima)
- Posição espacial do robô
	- $$P=\begin{bmatrix}x\\y\\\theta\end{bmatrix}$$ Sendo x a posição em x, y a posição em y e $$\theta$$ a orientação
em relação ao frame inercial (ex.: em uma sala, escolher um dos seus cantos como referênca inercial)
- Movimentação: rodas esquerda e direita ($$\omega_r$$ - velocidade angular direita e $$\omega_l$$ - velocidade angular esquerda); distância entre os eixos das rodas; raio das rodas; orientação do robô em relação ao frame inercial (através de relações matemáticas ou sensores, como a bússola)
- Como o robô é um corpo rígido, podemos resumí-lo em um único ponto, o ponto médio (ponto equidistante a todos os lados)
- Esse ponto pode ter duas propriedades: Translação (com uma velocidade liner no eixo x) e Rotação (com uma velocidade angular no eixo y)
- Relação entre velocidade linear e angular: $$v=R \times \omega$$. Sendo R o raio da roda
- Obtendo a velocidade linear: 
    $$v_l=r\phi_l$$ e $$v_r=r\phi_r$$
    Cada roda contribui com **metade** da velocidade linear do ponto médio:
    $$v= \frac{r \phi r}{2} + \frac{r \phi l}{2} = \frac{r}{2} (\phi_l + \phi_r) = \frac{r}{2} (\omega_l + \omega_r)$$
- Obtendo a velocidade angular:
    $$\omega_r,pm = \frac{v_r,pm}{r_r,pm} = (\frac{r \phi_r}{2})/2$$ e $$\omega_l,pm = \frac{v_l,pm}{r_l,pm} = - (\frac{r \phi_l}{2})/2$$
    $$\omega= \frac{r\phi_r}{2l} +\left(-\frac{r\phi_r}{2l} \right)$$ $$\implies$$ $$\omega=\frac{r}{2l}(\omega_r-\omega_l)$$
- Modelo cinemático no frame global:
    Tendo as velocidades e a orientação do robô, decompõe-se isso para se obter em relação ao frame inercial:
    $$P=\begin{bmatrix}X_I\\Y_I\\\theta_I\end{bmatrix}=\begin{bmatrix}v_x\\v_y\\\omega\end{bmatrix}=\begin{bmatrix}vcos(\theta)\\vsen(\theta)\\\omega\end{bmatrix}$$
    Desacoplando $$v$$ e $$\omega$$ temos:
    $$v = \dot{P}=\begin{bmatrix}\dot{X_I}\\\dot{Y_I}\\\theta_I\end{bmatrix}=\begin{bmatrix}cos(\theta_I)&0\\sen(\theta_I)&0\\0&1\end{bmatrix}$$ $$\begin{bmatrix}v\\\omega\end{bmatrix}$$ Modelo cinemático não linear para o robô
- Obtendo a posição a partir da velocidade:
    - Computacionalmente, para cada instante de $$t$$ faça:
        - Obter as velocidades ($$v, \omega$$) do instante atual $$t$$: $$(v_t, \omega_t)$$
        São obtidas via algoritmos de controle, joysticks, usuários, etc.
    - Obter a orientação atual do robô no frame inercial: $$\theta_{t-1}$$
        - É o valor obtido do próprio robô, da posição anterior $$(t-1)$$
    - Decomponha as velocidades no frame inercial: $$(v_{x,t}, v_{y,t}, \omega_t)$$
        - $$v_t = \dot{P}_t=\begin{bmatrix}v_{x,t}\\v_{y,t}\\\omega_t\end{bmatrix}=\begin{bmatrix}v_tcos(\theta_{t-1})\\vsen(\theta_{t-1})\\\omega_t\end{bmatrix}$$
    - Integrar (somar) a contribuição de cada componente no robô, atualizando assim a posição ro robô para o instante atual: $$P_t$$
        - $$P_t = P_{t-1}+\dot{P_t}\times dt$$
