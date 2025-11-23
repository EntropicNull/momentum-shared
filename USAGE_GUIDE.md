# Momentum Shared Library - Usage Guide

## Overview

The `momentum-shared` library provides platform-agnostic business logic, types, and utilities for the Momentum ecosystem. It ensures consistency between `momentum-web` and `momentum-mobile` applications.

## Installation

The library is already linked in both projects via `file:../momentum-shared` in their `package.json` files.

## Architecture

```
momentum-shared/
├── components/          # Component-specific logic and types
│   ├── MemberAvatar/
│   ├── TaskCard/
│   ├── Quest/
│   ├── StoreItem/
│   └── Modal/
├── utils/              # Shared utilities
│   ├── colors.ts
│   ├── formatting.ts
│   ├── validation.ts
│   └── helpers.ts
└── index.ts            # Main export file
```

---

## Components

### 1. MemberAvatar

**Purpose**: Generate avatar initials, colors, and text contrast logic.

#### Types
```typescript
interface MemberAvatarProps {
    name: string;
    color?: string;
    size?: number;
}
```

#### Functions

**`getAvatarInitials(name: string): string`**
- Extracts initials from a full name
- Returns up to 2 characters (first + last name initials)

```typescript
import { getAvatarInitials } from 'momentum-shared';

const initials = getAvatarInitials('John Doe'); // "JD"
```

**`getAvatarColor(name: string): string`**
- Generates a consistent color based on the name
- Returns a hex color code

```typescript
import { getAvatarColor } from 'momentum-shared';

const color = getAvatarColor('John Doe'); // "#3B82F6"
```

**`getContrastingTextColor(backgroundColor: string): string`**
- Determines whether to use white or black text based on background brightness
- Returns `'#FFFFFF'` or `'#000000'`

```typescript
import { getContrastingTextColor } from 'momentum-shared';

const textColor = getContrastingTextColor('#3B82F6'); // "#FFFFFF"
```

---

### 2. TaskCard

**Purpose**: Calculate task state and format task-related data.

#### Types
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

#### Functions

**`getTaskCardState(task: Task): TaskCardState`**
- Calculates the current state of a task
- Returns status colors, labels, and action text

```typescript
import { getTaskCardState } from 'momentum-shared';

const state = getTaskCardState(task);
console.log(state.statusLabel); // "Pending", "Completed", or "Approved"
console.log(state.statusColor); // Hex color for status badge
```

**`formatTaskPoints(points: number): string`**
- Formats point values for display
- Adds "pts" suffix

```typescript
import { formatTaskPoints } from 'momentum-shared';

const formatted = formatTaskPoints(100); // "100 pts"
```

---

### 3. Quest

**Purpose**: Manage quest states and validation logic.

#### Types
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

interface QuestCardState {
    status: string;
    color: string;
    actionLabel: string;
    canClaim: boolean;
    canComplete: boolean;
}
```

#### Functions

**`getQuestCardState(quest: Quest, userId?: string): QuestCardState`**
- Determines quest state and available actions
- Returns status, colors, and action labels

```typescript
import { getQuestCardState } from 'momentum-shared';

const state = getQuestCardState(quest, currentUserId);
console.log(state.actionLabel); // "Claim", "Complete", etc.
```

**`formatQuestPoints(points: number): string`**
- Formats quest point rewards
- Adds "pts" suffix

```typescript
import { formatQuestPoints } from 'momentum-shared';

const formatted = formatQuestPoints(500); // "500 pts"
```

**`canClaimQuest(quest: Quest, userId: string): boolean`**
- Validates if a user can claim a quest

**`canCompleteQuest(quest: Quest, userId: string): boolean`**
- Validates if a user can complete a quest

---

### 4. StoreItem

**Purpose**: Calculate store item affordability and availability.

#### Types
```typescript
interface StoreItem {
    _id: string;
    itemName: string;
    description?: string;
    cost: number;
    isAvailable?: boolean;
    stock?: number;
}

interface StoreItemCardState {
    canAfford: boolean;
    hasStock: boolean;
    isAvailable: boolean;
}
```

#### Functions

**`getStoreItemCardState(item: StoreItem, userPoints: number): StoreItemCardState`**
- Calculates if user can afford and if item is in stock

```typescript
import { getStoreItemCardState } from 'momentum-shared';

const state = getStoreItemCardState(item, 150);
console.log(state.canAfford); // true if userPoints >= item.cost
```

**`getRedeemButtonLabel(state: StoreItemCardState): string`**
- Returns appropriate button label based on state

```typescript
import { getRedeemButtonLabel } from 'momentum-shared';

const label = getRedeemButtonLabel(state); 
// "Redeem", "Insufficient Points", or "Out of Stock"
```

---

### 5. Modal & Forms

**Purpose**: Unified form validation and modal logic.

#### Types
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
```

#### Functions

**`validateForm(data: FormData, fields: FormField[]): FormValidationResult`**
- Validates form data against field definitions
- Returns validation result with errors

```typescript
import { validateForm } from 'momentum-shared';

const fields: FormField[] = [
    { name: 'title', label: 'Title', type: 'text', required: true, min: 3 },
    { name: 'points', label: 'Points', type: 'number', required: true, min: 1 }
];

const result = validateForm(formData, fields);
if (!result.isValid) {
    console.log(result.errors); // { title: "Title is required" }
}
```

**`getInitialFormData(fields: FormField[]): FormData`**
- Generates initial form state from field definitions

```typescript
import { getInitialFormData } from 'momentum-shared';

const initialData = getInitialFormData(fields);
// { title: '', points: 0 }
```

**`sanitizeFormData(data: FormData, fields: FormField[]): FormData`**
- Cleans and type-converts form data
- Trims strings, converts numbers

```typescript
import { sanitizeFormData } from 'momentum-shared';

const clean = sanitizeFormData(formData, fields);
// Ensures proper types and trimmed strings
```

**`hasFormChanges(currentData: FormData, initialData: FormData): boolean`**
- Checks if form has been modified

**`getModalSize(size: 'small' | 'medium' | 'large'): ModalSize`**
- Returns standardized modal dimensions

---

## Utilities

### Colors (`utils/colors.ts`)

**`hexToRgb(hex: string): { r: number; g: number; b: number } | null`**
- Converts hex color to RGB object

**`addOpacity(hex: string, opacity: number): string`**
- Adds alpha channel to hex color
- Opacity: 0-1

**`adjustBrightness(hex: string, percent: number): string`**
- Lightens or darkens a color
- Percent: -100 to 100

---

### Formatting (`utils/formatting.ts`)

**`formatDate(date: Date | string): string`**
- Formats date as "Jan 1, 2025"

**`formatRelativeTime(date: Date | string): string`**
- Returns relative time string
- "2 hours ago", "3 days ago", etc.

**`formatPointsWithCommas(points: number): string`**
- Adds thousand separators
- 1000 → "1,000"

**`truncateText(text: string, maxLength: number): string`**
- Truncates text with ellipsis

---

### Validation (`utils/validation.ts`)

**`isValidEmail(email: string): boolean`**
- Validates email format

**`isValidHexColor(color: string): boolean`**
- Validates hex color format

**`isValidUrl(url: string): boolean`**
- Validates URL format

---

### Helpers (`utils/helpers.ts`)

**`generateId(): string`**
- Generates unique ID

**`debounce(func: Function, wait: number): Function`**
- Debounces function calls

**`throttle(func: Function, limit: number): Function`**
- Throttles function calls

**`deepClone<T>(obj: T): T`**
- Deep clones objects

**`groupBy<T>(array: T[], key: keyof T): Record<string, T[]>`**
- Groups array by key

**`sortBy<T>(array: T[], key: keyof T, order?: 'asc' | 'desc'): T[]`**
- Sorts array by key

---

## Best Practices

1. **Always import from the root**: `import { ... } from 'momentum-shared'`
2. **Use TypeScript types**: Import and use the provided types for type safety
3. **Don't modify shared logic directly**: If you need platform-specific behavior, wrap the shared logic in your component
4. **Keep shared logic pure**: No side effects, no platform-specific code
5. **Test changes thoroughly**: Changes to shared code affect both platforms

---

## Examples

### Complete Form Example

```typescript
import { 
    validateForm, 
    getInitialFormData, 
    sanitizeFormData,
    type FormField 
} from 'momentum-shared';

const TASK_FIELDS: FormField[] = [
    { name: 'title', label: 'Title', type: 'text', required: true, min: 3 },
    { name: 'points', label: 'Points', type: 'number', required: true, min: 1, defaultValue: 10 }
];

// Initialize
const [formData, setFormData] = useState(getInitialFormData(TASK_FIELDS));
const [errors, setErrors] = useState({});

// Submit
const handleSubmit = () => {
    const validation = validateForm(formData, TASK_FIELDS);
    if (!validation.isValid) {
        setErrors(validation.errors);
        return;
    }
    
    const clean = sanitizeFormData(formData, TASK_FIELDS);
    // Submit clean data...
};
```

### Complete Avatar Example

```typescript
import { 
    getAvatarInitials, 
    getAvatarColor, 
    getContrastingTextColor 
} from 'momentum-shared';

const MemberAvatar = ({ name }) => {
    const initials = getAvatarInitials(name);
    const bgColor = getAvatarColor(name);
    const textColor = getContrastingTextColor(bgColor);
    
    return (
        <div style={{ backgroundColor: bgColor, color: textColor }}>
            {initials}
        </div>
    );
};
```

---

## Troubleshooting

### Module Resolution Issues

If you encounter "Cannot resolve momentum-shared":

1. **Check `package.json`**: Ensure `"momentum-shared": "file:../momentum-shared"` is present
2. **Reinstall**: Run `npm install` in the project
3. **Clear cache**: 
   - Web: Delete `.next` folder
   - Mobile: Run `npx expo start --clear`

### TypeScript Errors

If TypeScript can't find types:

1. **Check `tsconfig.json`**: Ensure path mapping is configured (mobile only)
2. **Restart IDE**: Sometimes TypeScript server needs a restart

---

## Contributing

When adding new shared logic:

1. Create the logic in a `.ts` file (platform-agnostic)
2. Create corresponding types in a `types.ts` file
3. Export from `index.ts`
4. Update this documentation
5. Test on both platforms before committing
