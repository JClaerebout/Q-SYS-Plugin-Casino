# Q-SYS Plugin for Blackjack

## Overview

The **Blackjack Q-SYS Plugin** brings player-versus-dealer blackjack to Q-SYS, with SVG playing cards, play chips, and controls for use in a UCI. The game runs locally in the plugin and requires no external service.

## Features

- Start with 1,000 play chips and adjust bets in increments of 10
- Hit, stand, double down, or split a pair
- Display dealer, player, and split hands with SVG playing cards
- Hide the dealer's second card until the player finishes
- Track wins, losses, and pushes for the current session
- Play in Q-SYS Designer emulation or on a Q-SYS Core

## Plugin Information

| Property | Value |
| -------- | ----- |
| Name | Blackjack |
| Version | 2.0.1 |
| Author | Jens Claerebout |
| Plugin file | `Blackjack.qplug` |
| License | [MIT](LICENSE) |

## Installation

1. Place `Blackjack.qplug` in the Q-SYS plugin directory or deploy it through Q-SYS Designer.
2. Reload the plugin in Designer if an earlier version is already loaded.
3. Add the Blackjack component to the design.
4. Run the design on a Core or start emulation.
5. Open the component, choose a bet, and press **Deal**.

To create a touchscreen interface, add the plugin's card displays, action buttons, and feedback controls to a UCI.

## Configuration

The plugin has no configurable properties or connection settings. A new session starts with 1,000 chips and a bet of 20.

### Action Controls

| Control | Description |
| ------- | ----------- |
| `Deal` | Start a round with the selected bet |
| `Hit` | Draw another card for the active hand |
| `Stand` | Finish the active hand |
| `Double` | Double the active hand's bet, draw one card, and stand |
| `Split` | Split an eligible exact-rank pair into two hands |
| `BetDown` / `BetUp` | Decrease or increase the next bet by 10 between rounds |
| `NewGame` | Reset chips, bet, and statistics between rounds |

Action controls are exposed as input pins. Buttons are disabled when their action is unavailable.

### Display Controls

| Control | Description |
| ------- | ----------- |
| `DealerCards` | Dealer card displays (11 slots) |
| `PlayerCards` | First player hand displays (11 slots) |
| `SplitCards` | Second player hand displays (11 slots) |
| `DealerTotal` | Visible dealer total |
| `PlayerTotal` / `SplitTotal` | Hand totals, bets, active-hand indicator, and results |
| `Status` | Game instructions and round result |
| `Balance` | Available play chips |
| `Bet` | Next round's bet |
| `Score` | Session wins, losses, and pushes |

Text feedback controls are exposed as output pins. Card displays are UI controls only.

## Game Rules

- A fresh shuffled 52-card deck is used each round.
- Aces count as 1 or 11. The dealer stands on all 17s, including soft 17.
- Natural blackjack pays 3:2; ordinary wins pay 1:1; pushes return the stake.
- Dealer blackjack is checked immediately after the initial deal.
- One split is allowed per round, and pairs must have the same rank.
- Doubling after splitting is allowed. Split aces receive one additional card each.
- A split-hand 21 pays as an ordinary win.
- Insurance and surrender are not available.

## Notes

- Chips are for play only. Runtime restarts reset the session.
- Wins, losses, and pushes count each split hand separately.
- This version retains the previous single-card plugin's ID. Existing UCI bindings to the old controls must be recreated.
- The separate `blackjack.lua` file is the original Text Controller card example; it is not required by `Blackjack.qplug`.

## Development

If the local test harness is available, run it with Lua 5.3 or newer:

```sh
lua tests/blackjack_test.lua
```

The tests cover control layout names, deterministic rules and payouts, invalid actions, and 500 shuffled rounds. Pass `--svg` to emit all 53 card SVGs inside an XML wrapper for validation. Designer SVG rendering and touchscreen behavior require manual verification.

The test harness and original Lua example are excluded from version control in the current local setup.

## License

This project is licensed under the [MIT License](LICENSE).

Copyright (c) 2026 Jens Claerebout.
