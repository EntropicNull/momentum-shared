# Momentum Shared Library

Platform-agnostic business logic, types, and utilities for the Momentum ecosystem.

## 📚 Documentation

- **[Usage Guide](./USAGE_GUIDE.md)** - Comprehensive guide with examples for all components and utilities
- **[API Reference](./API_REFERENCE.md)** - Quick reference for all exported functions and types
- **[Migration Checklist](./MIGRATION_CHECKLIST.md)** - Track migration progress and guidelines

## 🎯 Quick Start

```typescript
// Import what you need
import { 
    getAvatarInitials, 
    validateForm, 
    formatDate 
} from 'momentum-shared';

// Use in your components
const initials = getAvatarInitials('John Doe'); // "JD"
const validation = validateForm(data, fields);
const formatted = formatDate(new Date()); // "Nov 23, 2025"
```

## 📦 What's Included

### Components
- **MemberAvatar** - Avatar generation logic
- **TaskCard** - Task state management
- **QuestCard** - Quest state and validation
- **StoreItemCard** - Store item affordability logic
- **Modal & Forms** - Form validation and modal utilities

### Utilities
- **Colors** - Color manipulation (hex to RGB, opacity, brightness)
- **Formatting** - Date, time, and number formatting
- **Validation** - Email, URL, and color validation
- **Helpers** - ID generation, debounce, throttle, deep clone, sorting

## 🏗️ Architecture

```
momentum-shared/
├── components/          # Component-specific logic
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
└── index.ts            # Main export
```

## ✨ Benefits

- ✅ **Single Source of Truth** - Business logic lives in one place
- ✅ **Type Safety** - Full TypeScript support
- ✅ **Consistency** - Same behavior across platforms
- ✅ **Maintainability** - Update once, apply everywhere
- ✅ **Reduced Duplication** - ~2000+ lines of duplicate code removed

## 🔧 Development

### Adding New Shared Logic

1. Create logic file: `components/[Name]/logic.ts`
2. Create types file: `components/[Name]/types.ts`
3. Export from `index.ts`
4. Update documentation
5. Test on both platforms

### Testing

After changes, verify:
- Mobile app compiles (`npx expo start`)
- Web app compiles (`npm run dev`)
- TypeScript types work correctly
- Behavior is consistent across platforms

## 📝 Best Practices

1. **Keep it pure** - No side effects, no platform-specific code
2. **Type everything** - Use TypeScript for all exports
3. **Document changes** - Update docs when adding features
4. **Test thoroughly** - Changes affect both platforms

## 🤝 Contributing

See [MIGRATION_CHECKLIST.md](./MIGRATION_CHECKLIST.md) for migration guidelines and tracking.

## 📄 License

MIT

---

**Version:** 1.0.0  
**Last Updated:** November 23, 2025
