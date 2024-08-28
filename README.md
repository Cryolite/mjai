# Standardization Project for mjai Format Specification

## Background

Currently, the mjai format has become the de facto standard replay format used by many Mahjong AI projects. Therefore, adopting the mjai format in Mahjong AI projects is highly beneficial for improving interoperability with other Mahjong AI projects. Additionally, when exchanging replays between online Mahjong game platforms like Mahjong Soul, Tenhou, and Mahjong Ichibangai, which use their own unique replay formats, or between Mahjong AI projects like Kanachan, which also use unique replay formats, there is a significant advantage in implementing only conversions to and from the mjai format. By doing so, we can achieve compatibility with various replay formats through the mjai format as an intermediary. In other words, the mjai format can serve as a "hub format" at the center of various replay formats. This is similar to how Unicode serves as a central standard for converting between different character encodings.

## Problem

Currently, the mjai format specification is somewhat vague. The [original page describing the mjai format specification](https://gimite.net/pukiwiki/index.php?Mjai%20麻雀AI対戦サーバ) does not comprehensively cover all cases. Moreover, many Mahjong AI projects have added their own extensions to the mjai format. This could potentially hinder the conversion and interoperability of replays via the mjai format.

## Objective

Considering the above, the objective of this project is to standardize the mjai format specification and enhance interoperability between various replay formats. Specifically, the goals are as follows:

- To officially incorporate existing extensions added to the mjai format into the standardized specification.
- To expand the mjai format specification to accommodate various Mahjong rules.
- To implement converters between the standardized mjai format and the replay formats of major online Mahjong game platforms, such as Mahjong Soul, Tenhou, and Mahjong Ichibangai.

# Specification

## In-game Mode and Replay Mode

There are two aspects to the use of the mjai format. One is its use as a format for exchanging information during a game, and the other is its use as a format for recording the game (known as a game record or "牌譜"). The format used for exchanging information during a game allows only the information visible to a specific player to be seen, while other information remains hidden. Additionally, it serves the role of communicating the actions chosen by that player to the game host. The format used for recording the game (game record) makes all information visible. The former is called the "in-game mode" and the latter the "replay mode".

## Directions

In in-game mode, messages have two directions: host-to-player and player-to-host. Host-to-player messages notify a player of game events from the game host. Player-to-host messages are used to notify the game host of the player's choices during the game.

## Line-by-line Mode and Batch Mode

(TODO)

## Messages

### None

#### Directions

Player-to-host.

#### Scheme

[schema/none.json](schema/none.json)

#### Examples

```json
{"type":"none"}
```

#### Next Possible Message

(TODO)

### Hello

#### Directions

Host-to-player.

#### Schema

[schema/hello.json](schema/hello.json)

#### Examples

```json
{"type":"hello","protocol":"mjsonp","protocol_version":1}
```

#### Next Possible Message

- [Join](#join) (outbound)

### Join

#### Directions

Player-to-host.

#### Schema

[schema/join.json](schema/join.json)

#### Examples

```json
{"type":"join","name":"shanten","room":"default"}
```

#### Next Possible Message

- [Start of Game](#start-of-game) (Inbound)

### Start of Game

#### Directions

Host-to-player.

#### Schema

[schema/start_game.json](schema/start_game.json)

#### Examples

```json
{"type":"start_game","id":1,"names":["shanten","shanten","shanten","shanten"]}
```

#### Next Possible Message

- [None](#none) (outbound)

### Start of Round

#### Directions

Host-to-player.

#### Schema

[schema/start_kyoku.json](schema/start_kyoku.json)

#### Examples

```json
{"type":"start_kyoku","bakaze":"E","kyoku":1,"honba":0,"kyotaku":0,"oya":0,"dora_marker":"7s","tehais":[["?","?","?","?","?","?","?","?","?","?","?","?","?"],["3m","4m","3p","5pr","7p","9p","4s","4s","5sr","7s","7s","W","N"],["?","?","?","?","?","?","?","?","?","?","?","?","?"],["?","?","?","?","?","?","?","?","?","?","?","?","?"]]}
```

#### Next Possible Message

(TODO)

### Tsumo (Self-draw)

#### Directions

Host-to-player.

#### Schema

[schema/tsumo.json](schema/tsumo.json)

#### Examples

```json
{"type":"tsumo","actor":0,"pai":"?"}
```

#### Next Possible Message

- [None](#none)
- [Dahai (Tile Discard)](#dahai-tile-discard)
- [Hora (Winning)](#hora-winning)
- [Reach (Riichi)](#reach-riichi)
- [Ankan (Concealed Kong)](#ankan-concealed-kong)
- [Kakan (Add Kong)](#kakan-add-kong)
- [Ryukyoku (No Game)](#ryukyoku-no-game)

### Dahai (Tile Discard)

#### Directions

Host-to-player and player-to-host.

#### Schema

[schema/dahai.json](schema/dahai.json)

#### Examples

```json
{"type":"dahai","actor":1,"pai":"7s","tsumogiri":false}
```

#### Next Possible Message

- [Tsumo (Self-draw)](#tsumo-self-draw)
- [Pon (Peng)](#pon-peng)
- [Chi](#chi)
- [Daiminkan (Open Kong)](#daiminkan-open-kong)
- [Hora (Winning)](#hora-winning)
- [Ryukyoku (No Game)](#ryukyoku-no-game)

### Chi

#### Directions

Host-to-player and player-to-host.

#### Schema

[schema/chi.json](schema/chi.json)

#### Examples

```json
{"type":"chi","actor":1,"target":0,"pai":"6s","consumed":["5sr","7s"]}
```

#### Next Possible Message

(TODO)

### Pon (Peng)

#### Directions

Host-to-player and player-to-host.

#### Schema

[schema/pon.json](schema/pon.json)

#### Examples

```json
{"type":"pon","actor":1,"target":0,"pai":"C","consumed":["C","C"]}
```

#### Next Possible Message

(TODO)

### Daiminkan (Open Kong)

#### Directions

Host-to-player and player-to-host.

#### Schema

[schema/daiminkan.json](schema/daiminkan.json)

#### Examples

```json
{"type":"daiminkan","actor":2,"target":0,"pai":"5p","consumed":["5pr","5p","5p"]}
```

#### Next Possible Message

(TODO)

### Kakan (Add Kong)

#### Directions

Host-to-player and player-to-host.

#### Schema

[schema/kakan.json](schema/kakan.json)

#### Examples

```json
{"type":"kakan","actor":3,"pai":"S","consumed":["S","S","S"]}
```

### Ankan (Concealed Kong)

#### Directions

Host-to-player and player-to-host.

#### Schema

[schema/ankan.json](schema/ankan.json)

#### Examples

```json
{"type":"ankan","actor":1,"consumed":["N","N","N","N"]}
```

#### Next Possible Message

(TODO)

### Dora

#### Directions

Host-to-player.

#### Schema

[schema/dora.json](schema/dora.json)

#### Examples

```json
{"type":"dora","dora_marker":"8p"}
```

#### Next Possible Message

(TODO)

### Reach (Riichi)

#### Directions

Host-to-player and player-to-host.

#### Schema

[schema/reach.json](schema/reach.json)

#### Examples

```json
{"type":"reach","actor":1}
```

#### Next Possible Message

(TODO)

### Reach Accepted (Successful Riichi)

#### Directions

Host-to-player.

#### Schema

[reach_accepted.json](reach_accepted.json)

#### Examples

```json
{"type":"reach_accepted","actor":2}
```

#### Next Possible Message

(TODO)

### Hora (Winning)

#### Directions

Host-to-player and player-to-host.

#### Schema

(TODO)

#### Examples

(TODO)

#### Next Possible Message

(TODO)

### Ryukyoku (No Game)

#### Directions

Host-to-player and player-to-host.

#### Schema

(TODO)

#### Examples

(TODO)

#### Next Possible Message

(TODO)

### End of Round

#### Directions

Host-to-player.

#### Schema

[schema/end_kyoku.json](schema/end_kyoku.json)

#### Examples

```json
{"type":"end_kyoku"}
```

#### Next Possible Message

(TODO)

### End of Game

#### Directions

Host-to-player.

#### Schema

[schema/end_game.json](schema/end_game.json)

#### Examples

```json
{"type":"end_game"}
```

#### Next Possible Message

(TODO)
