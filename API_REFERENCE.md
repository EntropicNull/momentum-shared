# Momentum Shared Library - API Reference

Quick reference for all exported functions, types, and utilities.

---

## Table of Contents

- [MemberAvatar](#memberavatar)
- [TaskCard](#taskcard)
- [Quest](#quest)
- [StoreItem](#storeitem)
- [Modal & Forms](#modal--forms)
- [Color Utilities](#color-utilities)
- [Formatting Utilities](#formatting-utilities)
- [Validation Utilities](#validation-utilities)
- [Helper Utilities](#helper-utilities)

---

## MemberAvatar

### Functions

#### `getAvatarInitials(name: string): string`
Returns the initials from a full name (up to 2 characters).

**Parameters:**
- `name` (string): Full name

**Returns:** string - Initials (e.g., "JD" for "John Doe")

**Example:**
```typescript
getAvatarInitials('John Doe') // "JD"
getAvatarInitials('Alice') // "A"
```

---

#### `getAvatarColor(name: string): string`
Generates a consistent color based on the name.

**Parameters:**
- `name` (string): Name to generate color for

**Returns:** string - Hex color code

**Example:**
```typescript
getAvatarColor('John Doe') // "#3B82F6"
```

---

#### `getContrastingTextColor(backgroundColor: string): string`
Determines optimal text color (white or black) for a given background.

**Parameters:**
- `backgroundColor` (string): Hex color code

**Returns:** string - "#FFFFFF" or "#000000"

**Example:**
```typescript
getContrastingTextColor('#3B82F6') // "#FFFFFF"
getContrastingTextColor('#FFEB3B') // "#000000"
```

---

### Types

```typescript
interface MemberAvatarProps {
    name: string;
    color?: string;
    size?: number;
}
```

---

## TaskCard

### Functions

#### `getTaskCardState(task: Task): TaskCardState`
Calculates the current state of a task.

**Parameters:**
- `task` (Task): Task object

**Returns:** TaskCardState - State object with status, colors, and labels

**Example:**
```typescript
const state = getTaskCardState(task);
// {
//   isPending: true,
//   isCompleted: false,
//   isApproved: false,
//   statusColor: "#F59E0B",
//   statusLabel: "Pending",
//   actionLabel: "Mark Complete"
// }
```

---

#### `formatTaskPoints(points: number): string`
Formats point values for display.

**Parameters:**
- `points` (number): Point value

**Returns:** string - Formatted points with "pts" suffix

**Example:**
```typescript
formatTaskPoints(100) // "100 pts"
```

---

### Types

```typescript
interface Task {
    _id: string;
    title: string;
    description?: string;
    pointsValue: number;
    status: 'Pending' | 'Completed' | 'Approved';
    assignedTo?: any[];
    completedBy?: string;
    completedAt?: Date | string;
}

interface TaskCardProps {
    task: Task;
    onComplete?: () => void;
    onApprove?: () => void;
    showActions?: boolean;
}

interface TaskCardState {
    isPending: boolean;
    isCompleted: boolean;
    isApproved: boolean;
    statusColor: string;
    statusLabel: string;
    actionLabel: string;
}
```

---

## Quest

### Functions

#### `getQuestId(quest: Quest): string`
Gets the quest ID, handling both `_id` and `id` fields.

**Parameters:**
- `quest` (Quest): Quest object

**Returns:** string - Quest ID

---

#### `getQuestPoints(quest: Quest): number`
Gets the quest point value.

**Parameters:**
- `quest` (Quest): Quest object

**Returns:** number - Point value

---

#### `getQuestCardState(quest: Quest, userId?: string): QuestCardState`
Determines quest state and available actions.

**Parameters:**
- `quest` (Quest): Quest object
- `userId` (string, optional): Current user ID

**Returns:** QuestCardState - State object

**Example:**
```typescript
const state = getQuestCardState(quest, userId);
// {
//   status: "Available",
//   color: "#10B981",
//   actionLabel: "Claim Quest",
//   canClaim: true,
//   canComplete: false
// }
```

---

#### `formatQuestPoints(points: number): string`
Formats quest point rewards.

**Parameters:**
- `points` (number): Point value

**Returns:** string - Formatted points

---

#### `canClaimQuest(quest: Quest, userId: string): boolean`
Validates if a user can claim a quest.

**Parameters:**
- `quest` (Quest): Quest object
- `userId` (string): User ID

**Returns:** boolean

---

#### `canCompleteQuest(quest: Quest, userId: string): boolean`
Validates if a user can complete a quest.

**Parameters:**
- `quest` (Quest): Quest object
- `userId` (string): User ID

**Returns:** boolean

---

### Types

```typescript
interface Quest {
    _id: string;
    title: string;
    description?: string;
    pointsValue: number;
    status: 'Available' | 'Active' | 'PendingApproval' | 'Completed';
    claimedBy?: string | string[];
    completedBy?: string | string[];
    questType?: 'one-time' | 'recurring';
    recurrence?: {
        frequency: 'daily' | 'weekly' | 'monthly';
    };
}

interface QuestCardProps {
    quest: Quest;
    userId?: string;
    onClaim?: () => void;
    onComplete?: () => void;
    onApprove?: () => void;
    showActions?: boolean;
}

interface QuestCardState {
    status: string;
    color: string;
    actionLabel: string;
    canClaim: boolean;
    canComplete: boolean;
}
```

---

## StoreItem

### Functions

#### `getStoreItemCardState(item: StoreItem, userPoints: number): StoreItemCardState`
Calculates if user can afford and if item is in stock.

**Parameters:**
- `item` (StoreItem): Store item object
- `userPoints` (number): User's current points

**Returns:** StoreItemCardState - State object

**Example:**
```typescript
const state = getStoreItemCardState(item, 150);
// {
//   canAfford: true,
//   hasStock: true,
//   isAvailable: true
// }
```

---

#### `getRedeemButtonLabel(state: StoreItemCardState): string`
Returns appropriate button label based on state.

**Parameters:**
- `state` (StoreItemCardState): State object

**Returns:** string - Button label

**Example:**
```typescript
getRedeemButtonLabel(state) // "Redeem" | "Insufficient Points" | "Out of Stock"
```

---

### Types

```typescript
interface StoreItem {
    _id: string;
    itemName: string;
    description?: string;
    cost: number;
    isAvailable?: boolean;
    stock?: number;
}

interface StoreItemCardProps {
    item: StoreItem;
    userPoints: number;
    onRedeem?: () => void;
    showActions?: boolean;
}

interface StoreItemCardState {
    canAfford: boolean;
    hasStock: boolean;
    isAvailable: boolean;
}
```

---

## Modal & Forms

### Functions

#### `validateForm(data: FormData, fields: FormField[]): FormValidationResult`
Validates form data against field definitions.

**Parameters:**
- `data` (FormData): Form data object
- `fields` (FormField[]): Field definitions

**Returns:** FormValidationResult - Validation result with errors

**Example:**
```typescript
const result = validateForm(data, fields);
if (!result.isValid) {
    console.log(result.errors); // { title: "Title is required" }
}
```

---

#### `getInitialFormData(fields: FormField[]): FormData`
Generates initial form state from field definitions.

**Parameters:**
- `fields` (FormField[]): Field definitions

**Returns:** FormData - Initial form data

---

#### `sanitizeFormData(data: FormData, fields: FormField[]): FormData`
Cleans and type-converts form data.

**Parameters:**
- `data` (FormData): Form data to sanitize
- `fields` (FormField[]): Field definitions

**Returns:** FormData - Sanitized data

---

#### `hasFormChanges(currentData: FormData, initialData: FormData): boolean`
Checks if form has been modified.

**Parameters:**
- `currentData` (FormData): Current form data
- `initialData` (FormData): Initial form data

**Returns:** boolean

---

#### `getModalSize(size: 'small' | 'medium' | 'large'): ModalSize`
Returns standardized modal dimensions.

**Parameters:**
- `size` ('small' | 'medium' | 'large'): Modal size

**Returns:** ModalSize - { maxWidth: number, padding: number }

---

### Types

```typescript
interface FormField {
    name: string;
    label: string;
    type: 'text' | 'number' | 'textarea' | 'select' | 'email' | 'color';
    required?: boolean;
    placeholder?: string;
    options?: Array<{ label: string; value: string | number }>;
    min?: number;
    max?: number;
    pattern?: string;
    defaultValue?: any;
}

interface FormData {
    [key: string]: any;
}

interface FormValidationResult {
    isValid: boolean;
    errors: Record<string, string>;
}

interface ModalSize {
    maxWidth: number;
    padding: number;
}
```

---

## Color Utilities

#### `hexToRgb(hex: string): { r: number; g: number; b: number } | null`
Converts hex color to RGB object.

**Example:**
```typescript
hexToRgb('#3B82F6') // { r: 59, g: 130, b: 246 }
```

---

#### `addOpacity(hex: string, opacity: number): string`
Adds alpha channel to hex color.

**Parameters:**
- `hex` (string): Hex color
- `opacity` (number): Opacity (0-1)

**Example:**
```typescript
addOpacity('#3B82F6', 0.5) // "#3B82F680"
```

---

#### `adjustBrightness(hex: string, percent: number): string`
Lightens or darkens a color.

**Parameters:**
- `hex` (string): Hex color
- `percent` (number): Brightness adjustment (-100 to 100)

**Example:**
```typescript
adjustBrightness('#3B82F6', 20) // Lighter blue
adjustBrightness('#3B82F6', -20) // Darker blue
```

---

## Formatting Utilities

#### `formatDate(date: Date | string): string`
Formats date as "Jan 1, 2025".

**Example:**
```typescript
formatDate(new Date()) // "Nov 23, 2025"
```

---

#### `formatRelativeTime(date: Date | string): string`
Returns relative time string.

**Example:**
```typescript
formatRelativeTime(twoHoursAgo) // "2 hours ago"
```

---

#### `formatPointsWithCommas(points: number): string`
Adds thousand separators.

**Example:**
```typescript
formatPointsWithCommas(1000) // "1,000"
```

---

#### `truncateText(text: string, maxLength: number): string`
Truncates text with ellipsis.

**Example:**
```typescript
truncateText("Long text here", 10) // "Long te..."
```

---

## Validation Utilities

#### `isValidEmail(email: string): boolean`
Validates email format.

---

#### `isValidHexColor(color: string): boolean`
Validates hex color format.

---

#### `isValidUrl(url: string): boolean`
Validates URL format.

---

## Helper Utilities

#### `generateId(): string`
Generates unique ID.

---

#### `debounce(func: Function, wait: number): Function`
Debounces function calls.

---

#### `throttle(func: Function, limit: number): Function`
Throttles function calls.

---

#### `deepClone<T>(obj: T): T`
Deep clones objects.

---

#### `groupBy<T>(array: T[], key: keyof T): Record<string, T[]>`
Groups array by key.

---

#### `sortBy<T>(array: T[], key: keyof T, order?: 'asc' | 'desc'): T[]`
Sorts array by key.
