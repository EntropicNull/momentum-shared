# Momentum Shared Library - Migration Checklist

This document tracks the migration of components and utilities to the shared library.

## ✅ Completed Migrations

### Components

- [x] **MemberAvatar**
  - [x] Shared logic (`getAvatarInitials`, `getAvatarColor`, `getContrastingTextColor`)
  - [x] Shared types (`MemberAvatarProps`)
  - [x] Mobile implementation updated
  - [x] Web implementation created
  - [x] Documentation complete

- [x] **TaskCard**
  - [x] Shared logic (`getTaskCardState`, `formatTaskPoints`)
  - [x] Shared types (`Task`, `TaskCardProps`, `TaskCardState`)
  - [x] Mobile implementation updated
  - [x] Web implementation created
  - [x] Documentation complete

- [x] **QuestCard**
  - [x] Shared logic (`getQuestCardState`, `formatQuestPoints`, `canClaimQuest`, `canCompleteQuest`)
  - [x] Shared types (`Quest`, `QuestCardProps`, `QuestCardState`)
  - [x] Mobile implementation updated
  - [x] Web implementation created
  - [x] Documentation complete

- [x] **StoreItemCard**
  - [x] Shared logic (`getStoreItemCardState`, `getRedeemButtonLabel`)
  - [x] Shared types (`StoreItem`, `StoreItemCardProps`, `StoreItemCardState`)
  - [x] Mobile implementation updated
  - [x] Web implementation created
  - [x] Documentation complete

### Forms & Modals

- [x] **Form Validation System**
  - [x] Shared logic (`validateForm`, `getInitialFormData`, `sanitizeFormData`, `hasFormChanges`)
  - [x] Shared types (`FormField`, `FormData`, `FormValidationResult`)
  - [x] Documentation complete

- [x] **CreateTaskModal**
  - [x] Mobile implementation updated
  - [x] Web implementation updated

- [x] **CreateQuestModal**
  - [x] Mobile implementation updated
  - [x] Web implementation updated

- [x] **CreateStoreItemModal**
  - [x] Mobile implementation updated
  - [x] Web implementation updated

### Utilities

- [x] **Color Utilities**
  - [x] `hexToRgb`
  - [x] `addOpacity`
  - [x] `adjustBrightness`
  - [x] Documentation complete

- [x] **Formatting Utilities**
  - [x] `formatDate`
  - [x] `formatRelativeTime`
  - [x] `formatPointsWithCommas`
  - [x] `truncateText`
  - [x] Documentation complete

- [x] **Validation Utilities**
  - [x] `isValidEmail`
  - [x] `isValidHexColor`
  - [x] `isValidUrl`
  - [x] Documentation complete

- [x] **Helper Utilities**
  - [x] `generateId`
  - [x] `debounce`
  - [x] `throttle`
  - [x] `deepClone`
  - [x] `groupBy`
  - [x] `sortBy`
  - [x] Documentation complete

---

## 📋 Potential Future Migrations

### Components

- [ ] **EditTaskModal**
  - [ ] Extract shared validation logic
  - [ ] Unify with CreateTaskModal patterns

- [ ] **EditQuestModal**
  - [ ] Extract shared validation logic
  - [ ] Unify with CreateQuestModal patterns

- [ ] **EditStoreItemModal**
  - [ ] Extract shared validation logic
  - [ ] Unify with CreateStoreItemModal patterns

- [ ] **DeleteConfirmationModal**
  - [ ] Create shared confirmation modal logic
  - [ ] Standardize delete flows

- [ ] **MealCard**
  - [ ] Extract meal display logic
  - [ ] Create shared meal types

- [ ] **Calendar Components**
  - [ ] Extract date calculation logic
  - [ ] Create shared calendar utilities

### Utilities

- [ ] **API Error Handling**
  - [ ] Create shared error parsing logic
  - [ ] Standardize error messages

- [ ] **WebSocket Utilities**
  - [ ] Extract connection management logic
  - [ ] Create shared event types

- [ ] **Storage Utilities**
  - [ ] Create platform-agnostic storage interface
  - [ ] Unify localStorage/AsyncStorage patterns

---

## 🎯 Migration Guidelines

### When to Migrate

Migrate logic to the shared library when:

1. **Duplication exists**: The same logic appears in both mobile and web
2. **Business logic**: It's core business logic, not UI rendering
3. **Platform-agnostic**: It doesn't depend on platform-specific APIs
4. **Stable**: The logic is unlikely to diverge between platforms

### When NOT to Migrate

Keep logic platform-specific when:

1. **UI-specific**: It's tightly coupled to React Native or React DOM
2. **Platform APIs**: It uses platform-specific APIs (e.g., Camera, Notifications)
3. **Diverging requirements**: Mobile and web need different behavior
4. **Performance-critical**: Platform-specific optimizations are needed

### Migration Process

1. **Create shared logic file** in `momentum-shared/components/[ComponentName]/logic.ts`
2. **Create shared types file** in `momentum-shared/components/[ComponentName]/types.ts`
3. **Export from index.ts**
4. **Update mobile implementation** to use shared logic
5. **Update web implementation** to use shared logic
6. **Test both platforms**
7. **Update documentation**
8. **Update this checklist**

---

## 📊 Migration Metrics

- **Components Migrated**: 4 (MemberAvatar, TaskCard, QuestCard, StoreItemCard)
- **Modals Migrated**: 3 (CreateTask, CreateQuest, CreateStoreItem)
- **Utility Functions**: 25+
- **Lines of Duplicate Code Removed**: ~2000+
- **Type Safety Improvements**: 100% of migrated components

---

## 🔍 Testing Checklist

After each migration, verify:

- [ ] Mobile app compiles without errors
- [ ] Web app compiles without errors
- [ ] TypeScript types are correctly inferred
- [ ] Component behavior is identical on both platforms
- [ ] No runtime errors in production builds
- [ ] Documentation is updated

---

## 📝 Notes

- The shared library uses TypeScript for maximum type safety
- All shared logic must be pure functions (no side effects)
- Platform-specific rendering stays in the platform projects
- The library is linked via `file:../momentum-shared` in package.json
- Metro bundler (mobile) and Next.js (web) both support the shared library
