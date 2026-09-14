# Ms. Pac-Man DQN — Class 3 experiment

I trained the [class's supplied DQN notebook](https://github.com/pepealonso95/pacman-dqn) for **100 games**. A DQN learns estimates of which moves may lead to more future points. On the same five evaluation games, its mean score rose from **492** before training to **734** after training. This small test does not prove reliable play.

## Choices and prediction (recorded before training)

| Setting | Choice | Reason |
| --- | ---: | --- |
| Exploration | 0.20 | After 1,000 random warm-up decisions, about one in five training moves is random, giving the agent chances to try other routes. |
| Training games (episodes) | 100 | The notebook's starting budget provides practice beyond a five-game setup check. |
| Learning rate | 0.0001 | Small changes to the network may be steadier than larger jumps. |

**Prediction:** The trained agent may earn more points on average than the untrained agent, but 100 games may be too little for reliable play. A falling training loss alone would not prove better gameplay.

## What happened

The baseline is an **untrained network**, not an agent choosing random moves every time. Both networks played the same five game seeds with 5% random evaluation moves and the same 3,000-decision limit. No game reached that limit.

| Evaluation game (seed) | Before | After |
| --- | ---: | ---: |
| 1 (101) | 350 | 860 |
| 2 (202) | 500 | 450 |
| 3 (303) | 320 | 780 |
| 4 (404) | 800 | 780 |
| 5 (505) | 490 | 800 |
| **Mean** | **492** | **734** |

The mean improved by **242 points** (about 49%). Three games improved and two declined. The training plot shows scores changing substantially from game to game; the mean training loss rose over the run, so I would not describe the loss as evidence of steady improvement. The five-game comparison is the clearest evidence here. [Exact evaluation data](results/comparison.json)

The run finished **100/100 training games**, with **63,943 game decisions**, **15,736 learning updates**, and **294 seconds (4 minutes 54 seconds)** elapsed, including periodic demos. It used an Apple M5 Mac's **MPS GPU**, Python 3.13.15, and PyTorch 2.14.0. [Settings and hardware](results/config.json) · [Per-game training data](results/training.csv) · [Run summary](results/training_summary.json)

## Gameplay and training evidence

These GIFs show only the first **up to 20 seconds** of a game, replayed faster for viewing. The final GIF was selected as the best-scoring demonstration among the five final evaluation games, so it is not a typical or full game. Watch the agent move around the maze, but use the five full-game scores above to judge the result.

| Before training | Best trained demonstration |
| --- | --- |
| ![Untrained Ms. Pac-Man gameplay](results/episode_0000.gif) | ![Best trained Ms. Pac-Man gameplay](results/final_best.gif) |

Intermediate glimpses after 25, 50, 75, and 100 training games:

| 25 games | 50 games | 75 games | 100 games |
| --- | --- | --- | --- |
| ![Game clip after 25 training games](results/episode_0025.gif) | ![Game clip after 50 training games](results/episode_0050.gif) | ![Game clip after 75 training games](results/episode_0075.gif) | ![Game clip after 100 training games](results/episode_0100.gif) |

![Training scores, learning loss, and exploration rate](results/training_dashboard.png)

## What the agent learns

An **observation** is four recent small grayscale game screens, which help the agent detect movement. An **action** is a joystick move. **Reward** comes from game points; during learning the notebook clips individual rewards to the range −1 to 1, while the evaluation table reports actual game scores. The network updates its estimates using remembered observations, moves, and rewards.

**Limitation:** Performance is uneven: two of the five evaluation games scored slightly worse after training, and 100 training games are not enough to know how well the agent would play across many new games. The short, best-selected GIF can look more convincing than the full set of scores.

**Next experiment (not run):** Increase only the training budget from 100 to **200 games**, keeping exploration at 0.20, learning rate at 0.0001, and evaluation settings fixed. Compare the five-game mean to see whether more practice helps.

## Open or reproduce

Open the **[executed notebook](pacman_dqn.ipynb)** on GitHub to inspect its saved outputs, including the final score table, plot, and GIFs. To rerun locally, use Python 3.11–3.13 and Jupyter, put [`pacman_player.py`](pacman_player.py) beside the notebook, install [`requirements.txt`](requirements.txt), and run the notebook cells from top to bottom. A new run creates its own folder under `pacman_runs/` and a ZIP. Training can give different scores on other hardware or runs.

The **complete results ZIP**, including the untrained, intermediate, and final `.pt` model checkpoints, is kept locally at `pacman_runs/20260914_130002_885760.zip` (36 MB). `pacman_runs/` is intentionally excluded from this public repository; the selected results above are included in `results/`.
