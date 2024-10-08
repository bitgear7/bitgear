# bitgear

![bitgear logo](https://raw.githubusercontent.com/bitgear7/bitgear/refs/heads/main/bitgear.png)

## Introduction

bitgear is a protocol to inscribe randomly generated gear deriving from Bitcoin blocks.

Imagine extracting a piece of equipment from each future Bitcoin block!

To extract the data of a bitgear, slice the last 9 hex digits of a newly mined Bitcoin block hash.

Genesis Block Hash Example:

```javascript
'000000000019d6689c085ae165831e934ff763ae46a2a6c172b3f1b60a8ce26f'
```

Last 9 hex digits:

```javascript
'60a8ce26f'
```

### Types

bitgear can be of 1 of 7 possible categories.

To determine the Type of a bitgear, map the 4th hex character:

```javascript
'8' 
```

against the table below:

| Type    | Hex Range |
| -------- | ------- |
| Head | 0-1 |
| Body | 2-4 |
| Arms | 5-6 |
| Main Hand Weapon | 7-9 |
| Off-Hand | a-b |
| Legs | c-d |
| Accessory | e-f |

Since '8' is within the range of 7-9, the Type of bitgear is a Main Hand Weapon.

To determine the actual bitgear, extract the 5th and 6th hex characters:

```javascript
'ce'
```

and map against the Main Hand Weapon table below.

ce is within the Hex Range of c8-cf, therefore the Main Hand Weapon is a:

```
Tome
```

To inscribe, create a text inscription with MIME type:

```
text/plain
```

with namespace:

```
.bitgear
```

Combining these values, the valid text that could be inscribed in Block 1:

```
Tome.bitgear
```

### Prefix

bitgear have a chance to have a randomly determined prefix.

Using the following block hash of Bitcoin Block 864585:

```javascript
'000000000000000000003b485da71d761e3946459e33301e9227005014d32fe3'
```

Last 9 digits:

```javascript
'014d32fe3'
```

The 1st character of the last 9 hex digits determines if the bitgear has a Prefix value.

If the character is equal to 0 or 1 (12.5%) chance, then map those hex characters against the range of the Prefix table.

Since the 1st hex character is:

```javascript
'0'
```

We then extract the 2nd and 3rd characters:

```javascript
'14'
```

Map the value '14' against the Prefixes table below.

'14' maps to Dorian's, so this bitgear would be prefixed with

```
Dorian's
```

The 4th character is

```javascript
'd'
```

which maps to the type Legs.

The 5th and 6th characters are 

```javascript
'32'
```

which map to

```
Mythril Greaves
```

Therefore the valid bitgear text inscription that could be in the subsequent block 864586 is:

```
Dorian's Mythril Greaves.bitgear
```

### Suffix

bitgear also have a chance to have a randomly generated Suffix.

Using the block hash of Bitcoin block 864552:

```javascript
'00000000000000000002742e4d0a0bd028a8984bba1ee6332d602916872951f6'
```

Last 9 digits:

```javascript
'6872951f6'
```

The 7th character of the last 9 hex digits determines if the bitgear has a Suffix value.

If the character is equal to 0 or 1 (12.5%) chance, then map those hex characters against the range of the Suffix table.

In this example, since the 7th character is 

```javascript
'1'
```

We then extract the 8th and 9th characters:

```javascript
'f6'
```

which map to

```
of the Wolf
```

The 4th character is

```javascript
'2'
```

which maps to the type Body.

The 5th and 6th characters are 

```javascript
'95'
```

which map to

```javascript
Sash
```

Therefore the valid bitgear text inscription that could be in the following block 864553 is:

```
Sash of the Wolf.bitgear
```

### Example with both a Prefix and a Suffix

Especially rare bitgear (~1.5%) can have both a randomly generated Prefix and Suffix.

Using block 864533 as an example:

```javascript
'00000000000000000001506e706d2b9501036742e13e1a285507aaa018605153'
```

Last 9 digits:

```javascript
'018605153'
```

Both the 1st '0' and 7th characters '1' result in both a Prefix and Suffix.

Applying the rules altogether, the bitgear inscription that would be valid in block 864534 would be:

```
Arcane Leather Gloves of Soul.bitgear
```

### Inscribing Rules

To inscribe a valid bitgear, follow the ruleset above, and create a text inscription adhering to the [Ordinals](https://docs.ordinals.com/) protocol of the name of the generated bitgear appended with the namespace:

```
.bitgear
```

Using the Genesis block (Block 0) as an example, as demonstrated above, the bitgear generated was a Tome.

Therefore the valid text inscription is:

```
Tome.bitgear
```

The name of the bitgear and its namespace ARE CASE-SENSITIVE and must be SPELLED CORRECTLY WITHOUT ANY ADDED SPACING OR OTHER NON-DOCUMENTED CHARACTERS.

Therefore the following examples have quotes to illustrate invalid inscriptions:

```javascript
'tome.bitgear'
```

```javascript
'TomE.bitgear'
```

```javascript
'tome.Bitgear'
```

```javascript
'Tome.bitgear '
```

```javascript
'Tome. bitgear'
```

```javascript
' Tome.bitgear'
```

Each of these would be invalid.

The text must follow the rules set forth in this document to be valid adhering to the bitgear protocol.

### Block Hash Referencing

bitgear can only be inscribed in the next block, referencing the previous blockhash. 

Applying the rules defined above, the inscription:

```javascript
'Tome.bitgear'
```

generated from the Genesis block (block 0) hash:

```
'000000000019d6689c085ae165831e934ff763ae46a2a6c172b3f1b60a8ce26f'
```

is valid in the bitgear protocol in Block 1. 

Attempting to inscribe Tome.bitgear in Block 2 would not be valid as Block 1 now has a new blockhash that must be referenced - unless the hex mapping from the newly mined block hash in Block 1 AGAIN results in the inscription:

```javascript
'Tome.bitgear'
```

If no inscription occurs in a block, then no bitgear is created for that block.

1st-is-1st rules apply, meaning if multiple valid entries of Tome.bitgear occured in Block 1, the FIRST transaction ordered in the block inscribing the bitgear would be valid. Any successive inscriptions in Block 1 would be invalid.

Furthermore, if an invalid inscription of:

```javascript
'tome .bitgear'
```

is the 1st valid inscription in Block 1, but the next found inscription in Block 1 adhering to the protocol is:

```javascript
'Tome.bitgear'
```

Then that 2nd found inscription is valid, as it is the 1st VALID inscription found in the block.

These rules work somewhat similarly to the [bitmap](https://gitbook.bitmap.land/) protocol for inscribing, post-blockout.

### Protocol Start

The protocol recognizes valid bitgear inscriptions as of the FIRST CONFIRMED block AFTER THE BLOCK that this file is inscribed in.

The primary reason for this is to simply link the previous block hash with the inscribed text, without burdening the inscriber with additional protocol metadata, or indexers with more complicated logic.

For example, if this text file is first seen and confirmed in block 864587, with blockhash:

```javascript
'00000000000000000000041de080bdc56f9cc2ed88400235f2a825aa533a548a'
```

Then valid bitgear can begin to be inscribed in the following block 864588.

As an example, THAT bitgear would be generated like so:

```javascript
'a533a548a'
```

The 4th hex character is:

```javascript
'3'
```

which maps to a Body piece.

Hex characters 5-6 are:

```javascript
 'a5'
```
 
which maps to Gi

| Gi  | a0-a7 |

The VALID bitgear inscription for the next block (being mined in block 864588):

```javascript
'Gi.bitgear'
```

### Type Mapping Tables

| Type    | Hex Range |
| -------- | ------- |
| Head | 0-1 |
| Body | 2-4 |
| Arms | 5-6 |
| Main Hand Weapon | 7-9 |
| Off-Hand | a-b |
| Legs | c-d |
| Accessory | e-f |

### Head
| Head                   | Hex Range |
|------------------------|-----------|
| Leather Helmet         | 00-07     |
| Bronze Helmet          | 08-0f     |
| Steel Helmet           | 10-17     |
| Mythril Helmet         | 18-1f     |
| Dragoon Helmet         | 20-27     |
| Dark Knight Helmet     | 28-2f     |
| Bandana                | 30-37     |
| Hood                   | 38-3f     |
| Cap                    | 40-47     |
| Circlet                | 48-4f     |
| Skullcap               | 50-57     |
| Mask                   | 58-5f     |
| Tiara                  | 60-67     |
| Horns                  | 68-6f     |
| Top Hat                | 70-77     |
| Hooded Cape            | 78-7f     |
| Ninja Mask             | 80-87     |
| Oni Mask               | 88-8f     |
| Hannya Mask            | 90-97     |
| Kitsune Mask           | 98-9f     |
| Kendo                  | a0-a7     |
| Kappa Mask             | a8-af     |
| Sorceress Circlet      | b0-b7     |
| Wizard Hat             | b8-bf     |
| Hachigane              | c0-c7     |
| Jingasa                | c8-cf     |
| Monocle                | d0-d7     |
| Witch Hat              | d8-df     |
| Veil                   | e0-e7     |
| Feathered Cap          | e8-ef     |
| Crown                  | f0-f7     |
| Headband               | f8-ff     |

### Body
| Body         | Hex Range |
|-----------------------|-----------|
| Leather Armor         | 00-07     |
| Bronze Armor          | 08-0f     |
| Steel Armor           | 10-17     |
| Mythril Armor         | 18-1f     |
| Dragoon Armor         | 20-27     |
| Dark Knight Armor     | 28-2f     |
| Robe                  | 30-37     |
| Gown                  | 38-3f     |
| Cloak                 | 40-47     |
| Wizard Robe           | 48-4f     |
| Sorceress Robe        | 50-57     |
| Silk Robe             | 58-5f     |
| Witch Robe            | 60-67     |
| Husk                  | 68-6f     |
| Demonica              | 70-77     |
| Platemail             | 78-7f     |
| Assassin Garb         | 80-87     |
| Ninja Garb            | 88-8f     |
| Sash                  | 90-97     |
| Chainmail             | 98-9f     |
| Gi                    | a0-a7     |
| Tunic                 | a8-af     |
| Tuxedo                | b0-b7     |
| Kimono                | b8-bf     |
| Yukata                | c0-c7     |
| Yoroi                 | c8-cf     |
| Witch Gown            | d0-d7     |
| Trenchcoat            | d8-df     |
| Hide                  | e0-e7     |
| Furcoat               | e8-ef     |
| Brocade               | f0-f7     |
| Cuirass               | f8-ff     |

### Arms
| Arms                   | Hex Range |
|------------------------|-----------|
| Leather Gloves         | 00-0f     |
| Bronze Gloves          | 10-1f     |
| Steel Gauntlets        | 20-2f     |
| Mythril Gauntlets      | 30-3f     |
| Dragoon Gauntlets      | 40-4f     |
| Dark Knight Gauntlets  | 50-5f     |
| Scales                 | 60-6f     |
| Bracers                | 70-7f     |
| Mitts                  | 80-8f     |
| Sorceress Gloves       | 90-9f     |
| Wristbands             | a0-af     |
| Vambraces              | b0-bf     |
| Fisticuffs             | c0-cf     |
| Shuko                  | d0-df     |
| Claws                  | e0-ef     |
| Catclaws               | f0-ff     |

### Main Hand Weapons
| Main Hand Weapons    | Hex Range |
|------------|-----------|
| Dagger     | 00-07     |
| Knife      | 08-0f     |
| Needle     | 10-17     |
| Shortsword | 18-1f     |
| Longsword  | 20-27     |
| Rapier     | 28-2f     |
| Claymore   | 30-37     |
| Axe        | 38-3f     |
| Halberd    | 40-47     |
| Tomahawk   | 48-4f     |
| Bow        | 50-57     |
| Longbow    | 58-5f     |
| Sniperbow  | 60-67     |
| Crossbow   | 68-6f     |
| Staff      | 70-77     |
| Cane       | 78-7f     |
| Scepter    | 80-87     |
| Hammer     | 88-8f     |
| Mace       | 90-97     |
| Wand       | 98-9f     |
| Club       | a0-a7     |
| Spear      | a8-af     |
| Lance      | b0-b7     |
| Whip       | b8-bf     |
| Javelin    | c0-c7     |
| Tome       | c8-cf     |
| Sickle     | d0-d7     |
| Scythe     | d8-df     |
| Katana     | e0-e7     |
| Naginata   | e8-ef     |
| Odachi     | f0-f7     |
| Kunai      | f8-ff     |

### Off-Hand
| Off-Hand               | Hex Range |
|------------------------|-----------|
| Leather Shield         | 00-0f     |
| Bronze Shield          | 10-1f     |
| Steel Shield           | 20-2f     |
| Mythril Shield         | 30-3f     |
| Dragoon Shield         | 40-4f     |
| Dark Knight Shield     | 50-5f     |
| Parrying Dagger        | 60-6f     |
| Buckler                | 70-7f     |
| Aspis                  | 80-8f     |
| Caestus                | 90-9f     |
| Blunderbuss            | a0-af     |
| Tanto                  | b0-bf     |
| Wakizashi              | c0-cf     |
| Targe                  | d0-df     |
| Tate                   | e0-ef     |
| Greatshield            | f0-ff     |

### Legs
| Legs                   | Hex Range |
|------------------------|-----------|
| Leather Boots          | 00-0f     |
| Bronze Boots           | 10-1f     |
| Steel Greaves          | 20-2f     |
| Mythril Greaves        | 30-3f     |
| Dragoon Boots          | 40-4f     |
| Dark Knight Greaves    | 50-5f     |
| Slippers               | 60-6f     |
| Longboots              | 70-7f     |
| Silk Leggings          | 80-8f     |
| Leggings               | 90-9f     |
| Scalers                | a0-af     |
| Dress Shoes            | b0-bf     |
| Wizard Boots           | c0-cf     |
| Sorceress Slippers     | d0-df     |
| Tabi                   | e0-ef     |
| Witch Heels            | f0-ff     |

### Accessories
| Accessories            | Hex Range |
|------------------------|-----------|
| Ring                   | 00-0f     |
| Scroll                 | 10-1f     |
| Necklace               | 20-2f     |
| Amulet                 | 30-3f     |
| Belt                   | 40-4f     |
| Earrings               | 50-5f     |
| Charm                  | 60-6f     |
| Cape                   | 70-7f     |
| Shuriken               | 80-8f     |
| Bowtie                 | 90-9f     |
| Scarf                  | a0-af     |
| Ribbon                 | b0-bf     |
| Effigy                 | c0-cf     |
| Bracelet               | d0-df     |
| Anklet                 | e0-ef     |
| Orb                    | f0-ff     |

### Prefixes

| Prefixes               | Hex Range |
|------------------------|-----------|
| Satoshi's              | 00-07     |
| Hal's                  | 08-0f     |
| Dorian's               | 10-17     |
| Arcane                 | 18-1f     |
| Divine                 | 20-27     |
| Fabled                 | 28-2f     |
| Demonic                | 30-37     |
| Wailing                | 38-3f     |
| Cursed                 | 40-47     |
| Holy                   | 48-4f     |
| Angelic                | 50-57     |
| Profane                | 58-5f     |
| Unyielding             | 60-67     |
| Everbearing            | 68-6f     |
| Renowned               | 70-77     |
| Blessed                | 78-7f     |
| Undead                 | 80-87     |
| Spectral               | 88-8f     |
| Savage                 | 90-97     |
| Pure                   | 98-9f     |
| Sacred                 | a0-a7     |
| Radiant                | a8-af     |
| Immaculate             | b0-b7     |
| Barbaric               | b8-bf     |
| Ravenous               | c0-c7     |
| Grotesque              | c8-cf     |
| Profound               | d0-d7     |
| Serene                 | d8-df     |
| Sacrilegious           | e0-e7     |
| Nourishing             | e8-ef     |
| Crestfallen            | f0-f7     |
| Fractal                | f8-ff     |

### Suffixes
| Suffixes               | Hex Range |
|------------------------|-----------|
| of Temperance          | 00-03     |
| of Prudence            | 04-07     |
| of Fortitude           | 08-0b     |
| of Justice             | 0c-0f     |
| of Protection          | 10-13     |
| of Destruction         | 14-17     |
| of Strength            | 18-1b     |
| of Dexterity           | 1c-1f     |
| of Intelligence        | 20-23     |
| of Agility             | 24-27     |
| of Spirit              | 28-2b     |
| of Vitality            | 2c-2f     |
| of Luck                | 30-33     |
| of Fortune             | 34-37     |
| of Mercy               | 38-3b     |
| of Ruin                | 3c-3f     |
| of Sorrow              | 40-43     |
| of Hope                | 44-47     |
| of Fate                | 48-4b     |
| of Despair             | 4c-4f     |
| of Soul                | 50-53     |
| of Rapture             | 54-57     |
| of Salvation           | 58-5b     |
| of Runes               | 5c-5f     |
| of Healing             | 60-63     |
| of Nightmares          | 64-67     |
| of Dreams              | 68-6b     |
| of Greed               | 6c-6f     |
| of Gluttony            | 70-73     |
| of Envy                | 74-77     |
| of Pride               | 78-7b     |
| of Wrath               | 7c-7f     |
| of Sloth               | 80-83     |
| of Lust                | 84-87     |
| of the Rat             | 88-8b     |
| of the Ox              | 8c-8f     |
| of the Tiger           | 90-93     |
| of the Rabbit          | 94-97     |
| of the Dragon          | 98-9b     |
| of the Snake           | 9c-9f     |
| of the Horse           | a0-a3     |
| of the Goat            | a4-a7     |
| of the Monkey          | a8-ab     |
| of the Rooster         | ac-af     |
| of the Dog             | b0-b3     |
| of the Pig             | b4-b7     |
| of the Fox             | b8-bb     |
| of the Night Fox       | bc-bf     |
| of the Frog            | c0-c3     |
| of the Panda           | c4-c7     |
| of the Duck            | c8-cb     |
| of the Kraken          | cc-cf     |
| of the Ape             | d0-d3     |
| of the Peacock         | d4-d7     |
| of the Stag            | d8-db     |
| of the Bull            | dc-df     |
| of the Bear            | e0-e3     |
| of the Whale           | e4-e7     |
| of the Tanuki          | e8-eb     |
| of the Squirrel        | ec-ef     |
| of the Cat             | f0-f3     |
| of the Wolf            | f4-f7     |
| of the Moon            | f8-fb     |
| of Pepe                | fc-ff     |

### FAQ

#### Does the bitgear protocol support duplicates?

Yes, as long as the bitgear values are correctly mapped from block hashes in different blocks. 

Only one bitgear inscription can be valid per mined Bitcoin block.

For a duplicate to exist, two different block hashes must map to the same bitgear, where those are inscribed properly in those two different blocks.

#### What happens if a block re-organization occurs?

If any inscribed bitgear are affected such that its previously referenced block hash do not result in the correct mapping, then that inscription becomes invalid.

Simply put, the protocol does not support or handle re-orgs. Each inscription in a block should have a valid mapping to the its previous block hash.

If this mapping link between the previous block hash and text inscription is broken, then the inscription becomes invalid, without exception.

#### Is bitgear inflationary?

Yes, in perpetuity, 1 bitgear can be created per Bitcoin block.

If the Bitcoin hashrate becomes so powerful such that the leading zeroes in mined blocks begin to consume the last 8 digits of the block header (exclude the prefix, those bitgear can always have a prefix) then bitgear inscriptions become invalid.

This would be the scenario where the bitgear supply becomes capped temporarily, or forever.

#### Can bitgear be implemented on other blockchains?

Yes, for any chain that has block headers with a hexadecimal representation such that the bitgear protocol can adhere to.

For example, bitgear can be deployed on Bitcoin, Fractal and other Bitcoin forks, so long as those chains support inscriptions.

To bootstrap bitgear on other chains, first inscribe this text file and valid inscriptions will start from the following block.

### Influences

This protocol is influenced by the following:

- bitmap
- brc-20
- Sats Names
- Satoshi Nakamoto
