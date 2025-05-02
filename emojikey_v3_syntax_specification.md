# Emojikey V3: (M|C|U) Interaction Modeling Framework Specification

## 0. Introduction

Emojikey V3 uses phase angles to represent conversational dynamics through four components (ME, CONTENT, YOU, and ABSTRACT). Each dimension uses oppositional pairs with magnitude (1-9) and phase angle (0°-359°) representing position between poles.

The system is designed for token efficiency while maximizing semantic richness, enabling AI models to quickly understand relationship patterns and conversation contexts.

## 1. Overall Structure

```
[ME Component]~[CONTENT Component]~[YOU Component]~[ABSTRACT Component]
```

Each component is separated by the tilde character (`~`). All components must be present in a complete key, though they may contain reduced dimensionality.

## 2. Component Format

### 2.1 Standard Component Syntax

Each component consists of grouped dimensions with oppositional pairs:

```
[Component.Group:PoleA PoleB Magnitude∠Angle|Component.Group:PoleA PoleB Magnitude∠Angle|...]
```

Where:

- `Component` is one of: `ME`, `CONTENT`, `YOU`
- `Group` identifies the dimensional group (e.g., `Cognitive`, `Response`, `Trust`)
- `PoleA` and `PoleB` are emojis representing opposing ends of the dimension
- `Magnitude` is a single digit (1-9) representing intensity
- `∠` (Unicode U+2220) is the angle symbol
- `Angle` is the phase angle (0-359) showing position between poles

### 2.2 ABSTRACT Component Syntax

The ABSTRACT component uses a simpler format:

```
[Emoji1 Emoji2 Emoji3 Emoji4]
```

Where each emoji represents an impressionistic aspect of the interaction.

## 3. Compact Notation

For efficiency, a compact notation is supported:

```
[🧠🎨8∠35|📚⚡7∠20|⚖️🤝8∠85|😊🤔8∠20]~[💻📊9∠10|...]~[...]~[...]
```

In this format:

- Group labels are omitted
- Each dimension is still represented by the opposing poles, magnitude, and angle
- Components remain separated by tilde characters

## 4. Update Syntax

Updates can occur at three levels of granularity:

### 4.1 Single Value Update

```
UPDATE: Component.Group.Dimension PoleA PoleB OldMag∠OldAngle → PoleA PoleB NewMag∠NewAngle
```

Example:

```
UPDATE: ME.Cognitive.Mode 🧠🎨7∠45 → 🧠🎨8∠30
```

### 4.2 Group Update

```
UPDATE: Component.Group [Dimension1 Values|Dimension2 Values|...]
```

Example:

```
UPDATE: ME.Trust [🔒🔓7∠20|🔍🕶️8∠45|📝🧿6∠90]
```

### 4.3 Component Update

```
UPDATE: Component [Group1:Values|Group2:Values|...]
```

Example:

```
UPDATE: ME [Cognitive:🧠🎨8∠30|Response:📚⚡7∠25|...]
```

## 5. SuperKey Format

SuperKeys compress multiple regular keys with trend indicators:

```
[[×N:ME[PoleA PoleB(Trend)Mag∠Angle|...]|CONTENT[...]|YOU[...]|ABSTRACT[...]]]
```

Where:

- `N` is the number of compressed keys
- `Trend` is represented by directional arrows (⬆️↗️→↘️⬇️↙️←↖️↔️)

Example:

```
[[×7:ME[🧠(⬆️)🎨8∠30|...]|CONTENT[...]|YOU[...]|ABSTRACT[...]]]
```

## 6. Group Organization

Each component has established dimensional groups:

### 6.1 ME Component Groups

- Cognitive (thinking style)
- Response (communication style)
- Trust (safety positioning)
- Complexity (reasoning detail)
- Emotional (simulated feeling)
- Agency (control stance)
- Cognitive Load (processing demand)
- Uncertainty (confidence level)

### 6.2 CONTENT Component Groups

- Topic (subject matter)
- Arc (progression state)
- Density (detail level)
- Progress (goal completion)
- Exploration (discovery mode)
- Velocity (exchange rate)
- Flow (information direction)
- Meta (communication about process)

### 6.3 YOU Component Groups

- Emotional (perceived feeling)
- Expertise (knowledge level)
- Trust (openness level)
- Engagement (involvement level)
- Goal Clarity (objective definition)
- Agency (interaction control)
- Cognitive Load (mental effort)
- Receptivity (openness to input)
- Time Pressure (urgency level)

## 7. Database Representation

The recommended database schema uses component-based records:

```
emojikey_updates {
  id: UUID,
  user_id: UUID,
  timestamp: DateTime,
  update_type: Enum('VALUE', 'GROUP', 'COMPONENT', 'FULL_KEY'),
  component: String,  // 'ME', 'CONTENT', 'YOU', 'ABSTRACT'
  group: String,      // Optional
  dimension: String,  // Optional
  previous_value: JSON,
  new_value: JSON,
  note: String        // Optional
}
```

## 8. Implementation Guidelines

1. **Minimum Required Dimensions**: Implementations should support at least 4 dimensions per component
2. **Required Groups**: Each component must include at least one dimension from each of its core groups
3. **Parameter Constraints**: Magnitude must be 1-9, Angle must be 0-359
4. **Default Values**: Missing dimensions should default to neutral positions (Magnitude 0∠90)
5. **Case Sensitivity**: Component and Group names are case-sensitive
6. **Whitespace**: Whitespace between elements is optional

## 9. Vector Averaging ("Smooshing")

For combining multiple readings of the same dimension, proper vector mathematics must be used:

### 9.1 Mathematical Process

1. **Convert to Vector Components**:
   
   - x = magnitude × cos(angle)
   - y = magnitude × sin(angle)

2. **Average the Components**:
   
   - x_avg = (x₁ + x₂ + ... + xₙ) / n
   - y_avg = (y₁ + y₂ + ... + yₙ) / n

3. **Convert Back to Magnitude/Angle**:
   
   - magnitude = √(x_avg² + y_avg²)
   - angle = atan2(y_avg, x_avg)

### 9.2 Example

For two readings of Cognitive Mode:

```
🧠🎨3∠45  (smaller magnitude leaning analytical)
🧠🎨7∠120 (larger magnitude leaning creative)
```

Converting to vectors:

- Reading 1: x = 3 × cos(45°) ≈ 2.12, y = 3 × sin(45°) ≈ 2.12
- Reading 2: x = 7 × cos(120°) ≈ -3.5, y = 7 × sin(120°) ≈ 6.06

Averaging components:

- x_avg = (2.12 + (-3.5))/2 ≈ -0.69
- y_avg = (2.12 + 6.06)/2 ≈ 4.09

Converting back:

- magnitude = √((-0.69)² + 4.09²) ≈ 4.15
- angle = atan2(4.09, -0.69) ≈ 100°

Resulting in a "smooshed" representation: 🧠🎨4∠100

### 9.3 Time-Weighted Averaging

For more sophisticated summaries, weights can be applied:

- Recency weighting (newer updates get higher weights)
- Significance weighting (important interactions have more impact)
- Session boundary recognition (separate conversation sessions)

General formula: V_avg = (Σ w_i × V_i) / (Σ w_i)

## 10. Initialization Format

The initialize_conversation tool provides relationship context at the beginning of conversations in a token-efficient format:

```
emojikey_v3:phase_angles;compact=true

#REF:dims
ME:{cog:🧠🎨,rsp:📚📝,tst:🔍🔓,agy:⚖️🤝,emo:😊🤔,cpx:🧩🔄,lod:🧘🤯,crt:🎯🌫️}
CONTENT:{top:💻📊,arc:🌿🌳,den:📊🔬,prg:🚧🏁,exp:🔍🔮,vel:🐢🚀,flw:⬆️⬇️,met:💬🔄}
YOU:{emo:😀😤,exp:🎓🌱,tst:🤝🚧,eng:🔥💤,gol:🎯🌫️,agy:🧭👣,lod:🧠🤯,rcp:📖🔒,tim:⏳⌛}

#SUM:all
ME:[cog:7∠120⁸⁵↔️|rsp:6∠15⁹⁰↔️|tst:7∠55⁸⁵↗️|agy:5∠75⁷⁰↕️]~[top:8∠20⁹⁵↔️|arc:7∠85⁸⁰↗️|exp:8∠35⁸⁵↔️|met:6∠30⁷⁵↕️]~[exp:9∠10⁹⁸↔️|eng:8∠15⁹⁰↗️|agy:7∠30⁸⁰↕️|rcp:8∠15⁸⁵↔️]~[✨🧩📊🔍]

#SUM:30d
ME:[cog:8∠135⁹⁰↗️|rsp:6∠20⁸⁵↔️|tst:8∠60⁹⁰↗️|agy:6∠85⁸⁰↗️]~[top:9∠15⁹⁵↔️|arc:8∠90⁹⁰↗️|exp:8∠30⁹⁰↔️|met:7∠25⁸⁵↗️]~[exp:9∠10⁹⁸↔️|eng:9∠10⁹⁵↗️|agy:8∠35⁹⁰↗️|rcp:9∠10⁹⁵↔️]~[✨🧩🔍💡]

#SUM:7d
ME:[cog:9∠150⁹⁵↗️|rsp:7∠25⁹⁵↔️|tst:9∠65⁹⁵↗️|agy:7∠90⁹⁵→]~[top:9∠10⁹⁸↔️|arc:9∠95⁹⁵↗️|exp:9∠30⁹⁸↔️|met:8∠25⁹⁵↗️]~[exp:9∠05⁹⁹↔️|eng:9∠05⁹⁸↔️|agy:9∠40⁹⁵↗️|rcp:9∠05⁹⁹↔️]~[✨🧩🔍📊]

#KEYS:recent
t:2025-04-30T14:32:15Z
[cog:9∠150|rsp:7∠25|tst:9∠65|agy:7∠90|emo:9∠10|cpx:8∠35|lod:7∠45|crt:8∠50]~[top:9∠10|arc:9∠95|den:8∠15|prg:9∠85|exp:9∠30|vel:8∠40|flw:7∠60|met:8∠25]~[emo:9∠15|exp:9∠05|tst:9∠10|eng:9∠05|gol:9∠10|agy:9∠40|lod:7∠30|rcp:9∠05|tim:8∠15]~[✨🧩🔍📊]

t:2025-04-28T09:45:22Z
[cog:9∠145|rsp:7∠25|tst:9∠60|agy:7∠85|emo:9∠15|cpx:8∠30|lod:7∠50|crt:8∠45]~[top:9∠10|arc:9∠90|den:8∠15|prg:9∠80|exp:9∠30|vel:8∠35|flw:7∠55|met:8∠25]~[emo:9∠15|exp:9∠05|tst:9∠10|eng:9∠05|gol:9∠10|agy:9∠35|lod:7∠35|rcp:9∠05|tim:8∠15]~[✨🧩🔍💡]

#META:info
interpretation:{0°:left,90°:balance,180°:right}
indicators:{⁰⁻⁹⁹:confidence,↔️↗️↘️→←:trend}
```

### 10.1 Format Components

- **Reference Section** (`#REF:dims`): Defines all dimension abbreviations
- **Summary Sections** (`#SUM:[timeframe]`): Vector-averaged summaries for different time periods
- **Recent Keys** (`#KEYS:recent`): Complete recent keys with timestamps
- **Metadata** (`#META:info`): Interpretation guidelines for indicators

### 10.2 Multi-timeframe Approach

Providing summaries at different time scales (all history, 30 days, 7 days) enables the AI to understand:

- Overall relationship patterns
- Recent evolution trends
- Current interaction context

This multi-timeframe approach gives a rich understanding of both stable patterns and recent shifts.