# MemberAvatar Component

## Overview
Platform-agnostic avatar component that displays user initials in a colored circle.

## Shared Logic

### Functions

#### `getInitials(name: string): string`
Extracts the first letter from a name and returns it uppercase.

**Example:**
```typescript
getInitials("John Doe") // Returns "J"
getInitials("") // Returns "?"
```

#### `getAvatarStyles(color: string, size: number): AvatarStyles`
Calculates all style properties based on size and color.

**Example:**
```typescript
getAvatarStyles("#4F46E5", 48)
// Returns:
// {
//   backgroundColor: "#4F46E5",
//   width: 48,
//   height: 48,
//   borderRadius: 24,
//   fontSize: 19
// }
```

#### `validateColor(color: string): string`
Validates hex color format and returns default gray if invalid.

**Example:**
```typescript
validateColor("#4F46E5") // Returns "#4F46E5"
validateColor("invalid") // Returns "#808080"
```

#### `getContrastingTextColor(backgroundColor: string): string`
Calculates whether white or black text provides better contrast.

**Example:**
```typescript
getContrastingTextColor("#4F46E5") // Returns "#FFFFFF" (white on dark blue)
getContrastingTextColor("#F0F0F0") // Returns "#000000" (black on light gray)
```

## Usage

### Web Implementation
```typescript
import MemberAvatar from './components/shared/MemberAvatar';

<MemberAvatar
  name="John Doe"
  color="#4F46E5"
  size={48}
  showName={true}
/>
```

### Mobile Implementation
```typescript
import MemberAvatar from './components/shared/MemberAvatar';

<MemberAvatar
  name="John Doe"
  color="#4F46E5"
  size={48}
  showName={true}
/>
```

## Props

| Prop | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `name` | `string` | Yes | - | User's name (first letter will be shown) |
| `color` | `string` | Yes | - | Hex color for avatar background |
| `size` | `number` | Yes | - | Avatar diameter in pixels |
| `showName` | `boolean` | No | `false` | Whether to display name next to avatar |
| `fontSize` | `number` | No | `size * 0.4` | Custom font size for initials |
| `style` | `any` | No | - | Platform-specific additional styles |

## Design Decisions

1. **Single Letter**: Shows only the first letter to keep avatars simple and recognizable
2. **Automatic Contrast**: Calculates text color automatically for accessibility
3. **Proportional Sizing**: Font size is 40% of avatar size for optimal readability
4. **Validation**: Gracefully handles invalid colors with fallback
5. **Platform Agnostic**: All logic works identically on web and mobile
