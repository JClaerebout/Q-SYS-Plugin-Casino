# Q-SYS Casino

Six local, play-chip games in one Q-SYS plugin: **Blackjack, Baccarat, Video Poker, Higher or Lower, Roulette, and Slots**. Each game has its own component page with controls that can be copied into a UCI. Cards, the Roulette wheel, and slot symbols are generated as SVG; no images or external services are required.

## Installation

1. Install `Blackjack.qplug` in your Q-SYS Designer plugin directory.
2. Reload the plugin, then add **Q-SYS Casino** to your design.
3. Start emulation or run the design on a Core and open the component.
4. Choose a game page, set a bet, and deal or spin.

## Shared chips and controls

- Start with **1,000 play chips**, betting **20** per round.
- `BetDown` / `BetUp` change the next stake in increments of 10.
- `Balance`, `Bet`, and `Score` show the shared wallet, next stake, and session wins/losses/pushes on every page.
- `NewGame` resets all six games, the wallet, bet, and statistics between rounds.
- Only one round can be active. Finish that round before starting another game. Page navigation does not cancel a wager. Both disabled buttons and runtime input guards enforce this rule.
- Bets are deducted when a round begins. Returns include the original stake; a returned stake counts as a push. A Blackjack split counts each hand separately.
- Below 10 chips, reset the casino to play again. Runtime restarts reset the session. Chips have no monetary value and no persistence.

## Games

### Blackjack

`Deal`, `Hit`, `Stand`, `Double`, and `Split` retain their existing behavior. A fresh 52-card deck is shuffled each round. Aces count as 1 or 11; the dealer stands on all 17s. Natural Blackjack pays 3:2, ordinary wins 1:1, and ties return the stake. Dealer naturals are checked immediately.

One exact-rank split is allowed, including doubling after a split. Split aces receive one additional card each. Split 21 pays as an ordinary win. No insurance or surrender.

`DealerCards`, `PlayerCards`, and `SplitCards` each contain 11 card displays. `DealerTotal`, `PlayerTotal`, `SplitTotal`, and `Status` provide feedback.

### Baccarat

Select `BaccaratPlayer`, `BaccaratBanker`, or `BaccaratTie`, then press `BaccaratDeal`. Cards reveal in sequence and the round settles automatically. A fresh single deck is shuffled each round. Aces count as 1, tens and faces as 0, and totals use the last digit.

Naturals of 8 or 9 stand. Otherwise Player draws on 0–5. If Player stands, Banker draws on 0–5. If Player draws, Banker draws on 0–2; on 3 unless Player's third card is 8; on 4 with a third card of 2–7; on 5 with 4–7; on 6 with 6–7; and stands on 7.

Player pays 1:1, Banker pays 0.95:1 (5% commission), and Tie pays 8:1. Player and Banker wagers push on a tie. Fractional chip balances are supported.

Displays: `BaccaratPlayerCards` and `BaccaratBankerCards` (three each), plus `BaccaratStatus`. The selected wager is highlighted.

### Video Poker

Press `PokerDealDraw`, select any of the five `PokerHold` toggles, then press Draw. Unheld cards are replaced from the remaining deck; held cards stay in their positions. There is one draw per wager, with no additional charge. The five `PokerCards` displays and `PokerStatus` show the result.

Jacks or Better returns, **including stake**, per chip bet:

| Hand | Return |
| --- | ---: |
| Royal flush | 800× |
| Straight flush | 50× |
| Four of a kind | 25× |
| Full house | 9× |
| Flush | 6× |
| Straight (including A–2–3–4–5) | 4× |
| Three of a kind | 3× |
| Two pair | 2× |
| Pair of jacks, queens, kings, or aces | 1× |
| Other | 0× |

The royal-flush rate applies at every stake; there is no separate maximum-coin bonus.

### Higher or Lower

Press `HigherDeal`, then `Higher` or `Lower` to predict the next card. One guess settles the round. Aces are low, kings high; suits do not matter. Correct guesses pay 1:1 and equal ranks return the stake. Cards come from a fresh single deck without replacement.

Displays: `HigherCards` (two) and `HigherStatus`.

### Roulette

Select one number using `RouletteNumber` (array indexes 1–37 represent numbers 0–36), or choose `RouletteRed`, `RouletteBlack`, `RouletteEven`, or `RouletteOdd`. Press `RouletteSpin` to animate the SVG wheel. The selected bet is highlighted; the result number receives a gold highlight when the wheel stops.

European single-zero wheel, with one bet per spin. Straight numbers pay 35:1 and outside bets pay 1:1. Zero loses all four outside bets. Split, street, dozen, column, and multiple simultaneous bets are not implemented.

Displays: `RouletteWheel` and `RouletteStatus`.

### Slots

Press `SlotsSpin` for three animated SVG reels that stop in sequence. Each reel independently selects one of six equally likely symbols. One payline is evaluated.

| Combination | Return including stake |
| --- | ---: |
| Three SEVEN | 50× |
| Three STAR | 20× |
| Three BELL | 10× |
| Three GEM | 8× |
| Three LEMON | 5× |
| Three CHERRY | 3× |
| Exactly two CHERRY | 1× |
| Other | 0× |

Displays: `SlotReels` (three) and `SlotsStatus`. There are no wilds, bonus rounds, or progressive jackpots.

## UCI integration

Action controls expose input pins, feedback text exposes output pins, and poker holds expose both. SVG displays are UI-only buttons. Copy controls from the appropriate component page into your UCI; this plugin does not automatically create UCI pages. Include the wallet and bet controls on each UCI page, and keep a way to return to an unfinished game.

The plugin uses Q-SYS [page/layout callbacks](https://help.qsys.com/DeveloperHelp/Content/Code_Examples/Basic_Plugin_Framework.htm) and a shared [timer](https://help.qsys.com/q-sys_7.0/content/Control_Scripting/Using_Lua_in_Q-Sys/Timer.htm) for animations. Spin outcomes are selected before animation; animation does not alter the outcome.

## License

[MIT](LICENSE). Copyright (c) 2026 Jens Claerebout.
