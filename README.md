# Virtual-Structure Formation Control — LIMO + Bebop

Controle de formação de uma estrutura virtual formada por um robô terrestre diferencial (AgileX **LIMO**) e um quadrimotor (**Parrot Bebop 2**), com desvio de obstáculo por espaço nulo (NSB) usando campo potencial gaussiano.

Trabalho prático da disciplina de **Robótica Móvel** — PPGEE / UFES — 2026/1.

## Grupo

- Ruyther Maximo
- Samuel Bucher
- Zacchaeus Oladipo

## Descrição

A formação segue uma trajetória em lemniscata de Bernoulli no plano XY, mantendo o drone a 1,5 m de altura acima do ponto de controle do LIMO. O controlador é do tipo laço interno–laço externo:

- **Laço externo (cinemático):** controlador da formação com feedforward + saturação `tanh`.
- **Laço interno (dinâmico):** compensador dinâmico para cada robô (LIMO e Bebop).
- **Desvio de obstáculo:** subtarefa de maior prioridade, unida à formação pelo espaço nulo, com campo potencial gaussiano.

Referência: M. Sarcinelli-Filho e R. Carelli, *Control of Ground and Aerial Robots*, Springer.

## Requisitos

- MATLAB (validado no R2022)
- ROS + OptiTrack (NatNet) no ambiente do LAB-AIR
- Joystick (para parada de emergência / modo manual)

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

- [Disponível clicando aqui](https://drive.google.com/drive/folders/1d5_fQV3iPW84FAu4oiGnuEy1Hro_iWej?usp=sharing)

## Arquivos

- `final_controller.m` — código principal
- `Final_Project_Robotics.pdf` — relatório do experimento
- `outputs/data.mat` — dados coletados
- `figs/` — figuras dos resultados
