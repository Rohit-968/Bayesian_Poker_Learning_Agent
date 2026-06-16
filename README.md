# Bayesian_Poker_Learning_Agent

**Heads-Up No-Limit Texas Hold'em AI built on Bayesian inference — real-time, interpretable, and statistically validated.**

[![Python](https://img.shields.io/badge/Python-3.8+-3776AB?style=flat-square&logo=python&logoColor=white)](https://python.org)
[![Flask](https://img.shields.io/badge/Flask-Web%20Interface-000000?style=flat-square&logo=flask&logoColor=white)](https://flask.palletsprojects.com)
[![License](https://img.shields.io/badge/License-MIT-green?style=flat-square)](LICENSE)
[![Status](https://img.shields.io/badge/Status-Active-brightgreen?style=flat-square)]()

---

## Why this exists

Most poker AIs use Counterfactual Regret Minimization (CFR) — powerful, but computationally heavy and opaque. This agent takes a different approach: compress the opponent's possible hands into structured probability distributions, update them with Bayes' theorem after every action, and compute Expected Value (EV) in real time.

The result is an agent that is fast, mathematically grounded, and fully transparent about why it makes every decision.

---

## Results

**Bayesian Agent vs. EV Agent — 1,500 hands**

```
Mean chip gain per hand:  +4.47
95% confidence interval:  [3.30, 5.65]
```

The confidence interval lies entirely above zero — statistically significant outperformance, driven by dynamic opponent exploitation rather than static range assumptions.

---

## How it works

The agent follows a **Sense → Think → Act** pipeline:

```
Observed action
      │
      ▼
┌──────────────┐     Bayes' theorem       ┌────────────────────────┐
│ Belief module│ ──────────────────────▶ │ P(hand bucket | action) |
└──────────────┘                          └──────────┬─────────────┘
                                                     │
                                                     ▼
                                        ┌───────────────────────┐
                                        │  Monte Carlo equity   │
                                        │  + EV computation     │
                                        │  (fold / call / raise)│
                                        └──────────┬────────────┘
                                                   │
                                        ┌──────────▼────────────┐
                                        │  Entropy check        │
                                        │  High H → explore     │
                                        │  Low  H → exploit     │
                                        └──────────┬────────────┘
                                                   │
                                                   ▼
                                              Action taken
```

**Hand bucketing** compresses the opponent's range into four human-readable categories:

| Bucket | Examples |
|---|---|
| Premium | AA, KK, QQ, AK suited |
| Strong draw | Flush draw, open-ended straight draw |
| Weak pair | Second pair, low kicker |
| Air | Missed draws, low unconnected cards |

**Shannon Entropy** governs exploration: when belief distributions are diffuse (high uncertainty), the agent bluffs and probes; when concentrated (high confidence), it plays pure EV.

---

## Architecture

```
bayesian_poker_agent/
├── engine/          # Texas Hold'em rules, pot tracking, legal actions
├── belief/          # Bayesian belief updater — P(bucket | action history)
├── decision/        # EV computation for fold / call / bet / raise
├── evaluation/      # Monte Carlo win equity estimation
├── static/          # Dashboard assets (charts, belief visualisations)
├── app.py           # Flask web interface
└── main.py          # Headless simulation runner
```

---

## Agent types

| Agent | Strategy | Use case |
|---|---|---|
| Random | Uniform action selection | Baseline / sanity check |
| EV | ABC poker, flat opponent range | Strong baseline, exploitable |
| Bayesian | Dynamic belief update + EV vs. refined range | Full agent — exploits tendencies |

---

## Setup

```bash
git clone https://github.com/yourusername/bayesian_poker_agent.git
cd bayesian_poker_agent
pip install flask matplotlib
```

**Play against the AI (web interface)**
```bash
python app.py
# Open http://localhost:5000
```

**Run headless simulations**
```bash
python main.py
# Outputs: mean chip gain, 95% CI, per-seed breakdown
```

The web interface exposes the agent's full internal state in real time — opponent belief distributions, per-action EV values, and entropy score — making it a "glass box" rather than a black box.

---

## Roadmap

- [ ] **RL opponent modeling** — replace static likelihood tables with a learned model that adapts to individual opponent tendencies
- [ ] **Multi-street planning** — integrate PPO for bet sizing and bluff sequences across streets, moving beyond one-step EV
- [ ] **Unsupervised bucketing** — cluster hands by equity and playability with K-Means instead of heuristic buckets

---

## Contributing

Issues and pull requests are welcome. Open an issue to discuss new opponent models, evaluation strategies, or RL integration before submitting a PR.

---

## License

MIT — see [LICENSE](LICENSE) for details.
