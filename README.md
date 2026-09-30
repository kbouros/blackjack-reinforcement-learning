# Blackjack Reinforcement Learning

A custom Blackjack environment and reinforcement learning agents developed for the **Reinforcement Learning and Dynamic Optimization** course at the **Technical University of Crete**, in 2025.

The project explores how card counting and learned betting strategies affect performance, comparing **tabular Q-learning** with **Deep Q-Networks (DQN)**. The implementation, training, evaluation, and policy visualizations are contained in a single Jupyter notebook.

## Repository contents

| File | Description |
| --- | --- |
| [RL_Project_Phase2_final.ipynb](RL_Project_Phase2_final.ipynb) | Main implementation and saved experiment outputs |
| [RL_Project_Final.pdf](RL_Project_Final.pdf) | Phase 2 report describing the methods, experiments, and findings |
| [RL_final_presentation.pdf](RL_final_presentation.pdf) | Presentation covering both phases of the course project |

## Environment

The notebook implements a finite-deck Blackjack environment with:

- **Hit, stand, double, and split** actions, with state flags indicating when doubling and splitting are available.
- A **3:2 payout** for a winning two-card Blackjack.
- Card counting and configurable numbers of decks.
- Deck replenishment and reshuffling at the start of a game when 10 or fewer cards remain.
- Fixed bets or a learned betting policy.
- An interactive console for human play or observing trained agents.

The state includes the player's hand total, usable aces, the dealer's visible card, counting information, and action-availability flags. The DQN playing policies also include the bet amount.

## Approaches

### Task 1: Playing policy with fixed bets

Tabular Q-learning learns a playing policy using a fixed bet of 1 unit and three counting states: low, neutral, and high. The notebook visualizes the learned decisions for each counting state.

### Task 2: Playing and betting policies

The agent also chooses an initial bet between 1 and 40 units. Three approaches are explored:

- **Tabular Q-learning with true count:** uses a discretized true count clipped to the range -7 to 7. The playing policy is trained first with unit bets, followed by a separate betting-policy training stage.
- **DQN with true count:** uses separate playing and betting networks. The playing network receives a supervised warm start from a tabular policy.
- **DQN with complete count:** uses per-card-rank occurrence counts and the number of cards remaining, without the tabular warm start.

The report also discusses a four-deck variation of the tabular experiment.

## Results

The following values are taken from the **saved outputs of the supplied notebook**, using its one-deck configuration. Profit per episode is the net simulated reward divided by the number of evaluated episodes, expressed in betting units.

| Approach | Initial bet | Evaluation episodes | Average profit per episode |
| --- | --- | ---: | ---: |
| Task 1: Tabular Q-learning, low/neutral/high count | Fixed at 1 | 10,000,000 | +0.0115 |
| Task 2: Tabular Q-learning, true count | Learned, 1-40 | 10,000,000 | +0.4465 |
| Task 2: DQN, true count | Learned, 1-40 | 500,000 | -0.3912 |
| Task 2: DQN, complete count | Learned, 1-40 | 500,000 | -1.8251 |

The tabular true-count approach achieved the highest observed average return in these runs. Both tested DQN configurations produced negative returns.

These experiments use different training budgets: 10 million episodes for Task 1, 10 million per training stage for Task 2's tabular approach, and 100,000 RL training episodes per DQN variant. Betting exposure and initialization also differ, so the results are not a controlled comparison of algorithm quality. Returns depend on the custom simulator's rules and are not evidence of real-world profitability.

The report summarizes the tabular Task 2 return as approximately 0.42 units per episode; the table above preserves the notebook's saved output. Exact results may vary between runs.

## Running the notebook

The project was developed in Google Colab. Open `RL_Project_Phase2_final.ipynb` in Colab, or run it locally with Jupyter.

For local use, install the dependencies in a Python environment:

```bash
python -m pip install jupyter numpy matplotlib seaborn torch tqdm
jupyter notebook RL_Project_Phase2_final.ipynb
```

Run the import and definition cells, then the training, visualization, and evaluation cells in order. The true-count DQN warm start depends on the earlier tabular training.

The console-play cells request interactive input; skip them if you only want to run the experiments. Training and evaluation can take substantial time, particularly for DQN. Reduce the `episodes` arguments for a quick trial, though this will change the learned policies and results. Saved plots and outputs can be inspected without rerunning training.

## Authors and acknowledgments

**Konstantinos Bouros and Pavlos Zounits**  
Electrical and Computer Engineering, Technical University of Crete  
Course instructor: **Thrasyvoulos Spyropoulos**  
Project submitted in July 2025.

As documented in the presentation, GeeksforGeeks material informed the Q-learning and console implementation, policy visualization code was adapted from a Gymnasium Blackjack tutorial, and AI tools, mainly ChatGPT, assisted with debugging and DQN implementation. Further methodological details are provided in the report and notebook comments.
