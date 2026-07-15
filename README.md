# Virtual-Structure Formation Control — LIMO + Bebop

Controle de formação de uma estrutura virtual formada por um robô terrestre diferencial (AgileX **LIMO**) e um quadrimotor (**Parrot Bebop 2**), com desvio de obstáculo por espaço nulo (NSB) usando campo potencial gaussiano.

Trabalho prático da disciplina de **Robótica Móvel** — PPGEE / UFES — 2026/1.

## Integrantes

- Nome 1 —
- Nome 2 —
- Nome 3 —

## Descrição

A formação segue uma trajetória em lemniscata de Bernoulli no plano XY, mantendo o drone a 1,5 m de altura acima do ponto de controle do LIMO. O controlador é do tipo laço interno–laço externo:

- **Laço externo (cinemático):** controlador da formação com feedforward + saturação `tanh`.
- **Laço interno (dinâmico):** compensador dinâmico para cada robô (LIMO e Bebop).
- **Desvio de obstáculo:** subtarefa de maior prioridade, unida à formação pelo espaço nulo, com campo potencial gaussiano.

Referência: M. Sarcinelli-Filho e R. Carelli, *Control of Ground and Aerial Robots*, Springer.

## Requisitos

- MATLAB (validado no R2021) com ROS Toolbox
- ROS + OptiTrack (NatNet) no ambiente do LAB-AIR
- Joystick (para parada de emergência / modo manual)

## Como executar

1. Ajuste o IP do servidor ROS em `rosinit(...)`.
2. Configure as **TEST SWITCHES** no início do script (Seção 3):
   - `USE_LIMO`, `USE_DRONE` — habilita cada robô
   - `USE_OBSTACLE` — liga/desliga o desvio de obstáculo
   - `TRAJ_MODE` — `1` lemniscata completa / `0` posicionamento em `pos_des`
3. Deixe `TEST_MODE = 1` para a checagem pré-voo (lê a pose e o sinal de controle sem decolar) e depois `0` para o experimento.
4. Rode `final_controller.m`.

## Roteiro de testes (do mais simples ao completo)

| Teste | USE_LIMO | USE_DRONE | USE_OBSTACLE | TRAJ_MODE |
|-------|:--------:|:---------:|:------------:|:---------:|
| LIMO — ponto fixo | 1 | 0 | 0 | 0 |
| Drone — hover      | 0 | 1 | 0 | 0 |
| LIMO — lemniscata  | 1 | 0 | 0 | 1 |
| Drone — lemniscata | 0 | 1 | 0 | 1 |
| Formação — ponto fixo | 1 | 1 | 0 | 0 |
| Formação — lemniscata | 1 | 1 | 0 | 1 |
| LIMO — desvio      | 1 | 0 | 1 | 1 |
| Completo           | 1 | 1 | 1 | 1 |

## Segurança

- Botão 1 do joystick: parada de emergência (zera comandos e pousa o drone).
- Laço protegido por `try/catch` (pousa em caso de erro).
- Parede virtual (`|x|>2`, `|y|>2`, `z>1.8`).
- Watchdog de perda do corpo no OptiTrack (> 0,5 s).

## Vídeos do experimento

- Teste intermediário (somente LIMO): <link>
- Formação completa: <link>

## Arquivos

- `final_controller.m` — código principal
- `relatorio.pdf` / `relatorio.tex` — relatório do experimento
- `data.mat` — dados coletados
- `figs/` — figuras dos resultados
