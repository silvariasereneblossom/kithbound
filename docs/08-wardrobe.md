# 08 — Character creation and wardrobe

Status: draft spec. Art production for this system is the largest single cost in the project; see [13](13-client-and-art-pipeline.md) and [14](14-risks-and-conflicts.md).

## 1. Goals

- Dress-up depth in the spirit of the 2D Nikki games, for every body.
- Nothing about a character is locked to a gender, and nothing about gender is assumed from anything else.
- Looks are separate from stats.
- Most garments are made by player tailors.
- Hundreds of dressed characters on screen on integrated graphics.

## 2. Two views of one character

| View | Used in | Art |
|---|---|---|
| **Atelier figure** | Wardrobe, character sheet, inspect, styling contests, photo studio, dialogue | Large, detailed, front-facing, gently animated |
| **World sprite** | The game world | Small, stylised, fully animated in four facings |

Every garment has art for both. The atelier figure is where the Nikki-level detail lives; the world sprite keeps it cheap to draw and to render.

## 3. The person

All independent, all changeable for free at any mirror:

- **Frame**: a set of body silhouettes (target six at launch), named neutrally and not tied to gender.
- **Height** within the frame.
- **Skin**: full tone range with undertones; freckles, vitiligo, scars, birthmarks, body hair.
- **Face**: eyes, brows, nose, mouth, jaw, ears chosen separately; age lines.
- **Hair**: a library that treats coils, locs, braids, twists, fades, and straight and wavy styles as equals; facial hair on any face.
- **Voice**: several sets, chosen freely.
- **Pronouns**: she, he, they, a list of neopronouns, or none shown.
- **Aids and features**: glasses, hearing aids, prosthetic limbs, canes, wheelchairs, as ordinary options with their own cosmetic variety.

There is no "gender" field. The game refers to a character by name and chosen pronouns.

## 4. Slots

About two dozen, layered:

| Group | Slots |
|---|---|
| Head | Hair, headwear, hair ornament, face (makeup, paint), eyewear, earrings |
| Body | Top, bottom, dress (replaces top and bottom), outerwear, scarf or shawl |
| Limbs | Gloves, wrist, legwear, shoes |
| Accents | Neck, brooch, waist, back, handheld, body art |
| Special | Mobility aid, companion charm, backdrop (atelier only) |

Rules:

- Every garment fits every frame. There are no frame-specific versions with different coverage.
- Layer order has sensible defaults and per-outfit toggles (tucked or untucked, coat open or closed, hood up or down).
- Garments carry a mask that hides what is underneath, so layers do not poke through.
- Head coverings and cultural or religious dress are first-class, drawn with consultation, and combine properly with hair.

## 5. Garment data

Each garment is a data record:

- slot, layer pieces (front and back of the body), hide-mask;
- up to three dye channels, plus a fabric channel for sheen and texture;
- pattern, fabric, and trim identifiers (from Tailoring);
- style tags on five axes — Simple/Ornate, Lively/Elegant, Sweet/Sharp, Soft/Bold, Warm/Cool — plus theme tags (seaside, festival, workwear);
- maker's mark.

## 6. Getting clothes

| Source | Share |
|---|---|
| Tailors (and jewellers, smiths, woodworkers for accessories and armour looks) | Most |
| Earned looks from Trials, Hunts, and vocation milestones | Some |
| Seasonal festivals | Some; patterns return every year and stay craftable |
| Pattern books, if the business model includes them | Owner decision |

- **Add to wardrobe** consumes the item and unlocks that look for the whole account, permanently.
- **Guise**: any equipped gear can show any unlocked look, free, anywhere.
- **Outfits**: saved sets, with generous slots, switchable at will.
- **Colourways**: applying dyes to an owned look creates a new permanent colourway. Dyes are consumed.
- **Hand-me-downs**: a look can be re-made into an item once, to gift it.

## 7. Scale targets

| Milestone | Patterns | Distinct looks with fabrics and colourways |
|---|---|---|
| Vertical slice | 60 | hundreds |
| Launch | 600 | tens of thousands |
| Year one | 1,200 | — |

For comparison, Love Nikki reached roughly ten thousand items over years with a large art team. The pattern × fabric × trim × dye system is how a small team approaches that feel; it does not match that count of hand-drawn pieces.

## 8. Styling as play

**Styling** is a performance vocation.

- **Salons**: themed contests. An entry is scored by tag fit to the theme (automatic) and by player votes on anonymous pairs.
- Stylists rank up by entering, by voting carefully (agreement with consensus), and by styling other players on commission.
- Rewards are titles, backdrops, poses, and Salon tokens for a permanent shop. Themes rotate and return; nothing is missable.
- Vote manipulation is limited by anonymised pairs, randomised matchups, and per-account vote weighting.

## 9. Photo studio

Poses, backdrops, framing, and group shots with friends' characters. Images are rendered by the client; sharing outside the game is the player's own action.

## 10. Safety

No player-uploaded images or free-drawn textures at launch. Names and outfit titles pass the text filter. Combinations cannot produce nudity: the base figure always has a neutral underlayer.

## 11. Open questions

See [DECISIONS.md](DECISIONS.md): number of frames at launch, art style and sourcing, and whether looks are tradeable after unlocking.
