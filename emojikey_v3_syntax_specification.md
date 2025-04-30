# Emojikey V3: Formal Syntax Specification

## 1. Overall Structure

```
[ME Component]|[CONTENT Component]|[YOU Component]|[ABSTRACT Component]
```

Each component is separated by the pipe character (`|`). All components must be present in a complete key, though they may contain reduced dimensionality.

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
[🧠🎨8∠35|📚⚡7∠20|⚖️🤝8∠85|😊🤔8∠20]|[💻📊9∠10|...]|[...]|[...]
```

In this format:
- Group labels are omitted
- Each dimension is still represented by the opposing poles, magnitude, and angle
- Components remain separated by pipe characters

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
4. **Default Values**: Missing dimensions should default to neutral positions (Magnitude 5∠90)
5. **Case Sensitivity**: Component and Group names are case-sensitive
6. **Whitespace**: Whitespace between elements is optional
