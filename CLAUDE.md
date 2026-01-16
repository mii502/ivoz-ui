# ivoz-ui Development - Claude Code Context

This directory contains the forked ivoz-ui source code - the React UI component library used by IvozProvider portals.

---

## Repository Information

| Property | Value |
|----------|-------|
| **Fork** | https://github.com/mii502/ivoz-ui |
| **Upstream** | https://github.com/irontec/ivoz-ui |
| **Version** | 1.7.18 |
| **License** | GPL-3.0 |
| **Type** | npm package (@irontec/ivoz-ui) |

---

## What is ivoz-ui?

ivoz-ui is a React component library that provides:

1. **Entity Management** - CRUD components for API entities
2. **Form Components** - Input fields, selectors, file uploads
3. **Table/List Components** - Data tables with filtering, sorting, pagination
4. **Router Integration** - Route generation based on entity definitions
5. **State Management** - easy-peasy store for application state
6. **Theme/Styling** - MUI-based theming with SCSS customization

IvozProvider's portals (client, brand, platform) import components from this library.

---

## Directory Structure

```
ivoz-ui/
├── CLAUDE.md                   # This file
├── package.json                # Monorepo root (yarn workspaces)
├── yarn.lock                   # Dependencies
├── library/                    # Main library package
│   ├── package.json            # @irontec/ivoz-ui
│   └── src/
│       ├── components/         # React components
│       ├── entities/           # Entity definition types
│       ├── helpers/            # Utility functions
│       ├── hooks/              # React hooks
│       ├── icons/              # Icon components
│       ├── router/             # Routing utilities
│       ├── sass/               # SCSS styles
│       ├── services/           # API services
│       ├── store/              # State management
│       └── translations/       # i18n files
└── demo/                       # Demo application
    ├── package.json
    └── src/
```

---

## Key Components for IvozProvider Integration

### Entity System

| Component | Purpose |
|-----------|---------|
| `EntityInterface` | Base type for all API entities |
| `PropertySpec` | Defines entity property types and UI |
| `ForeignKeySpec` | Foreign key relationship handling |

### Form Components

| Component | Purpose |
|-----------|---------|
| `EntityForm` | Auto-generated forms from entity specs |
| `PropertyInput` | Input components by property type |
| `FormFieldFactory` | Creates form fields from spec |

### Table Components

| Component | Purpose |
|-----------|---------|
| `EntityList` | List view with CRUD operations |
| `ListContent` | Table content rendering |
| `FilterDialog` | Search and filter UI |

---

## When to Modify ivoz-ui

**DO modify** when you need to:
- Add new form field types (e.g., custom DID selector)
- Create new entity list behaviors
- Add global UI patterns used across portals
- Fix bugs in base components

**DON'T modify** when:
- Adding portal-specific UI (use portal's own components)
- Creating entity definitions (define in portal, not library)
- Customizing single-use components

---

## Development Workflow

### 1. Local Development

```bash
cd servers/vm-ivozprovider-lab/src/ivoz-ui
yarn install

# Build library
cd library
yarn build

# Run demo for testing
cd ../demo
yarn start
```

### 2. Testing with IvozProvider

Option A - Link locally (npm link):
```bash
cd library
npm link

cd ../../ivozprovider/web/portal/client
npm link @irontec/ivoz-ui
```

Option B - Copy dist to server:
```bash
cd library
yarn build
rsync -av dist/ user@185.16.41.36:/usr/share/ivozprovider/web/node_modules/@irontec/ivoz-ui/
```

### 3. Syncing with Upstream

```bash
git fetch upstream
git checkout main
git merge upstream/main --no-edit
git push origin main
```

---

## Key Files Reference

### Entity Types

```typescript
// library/src/entities/EntityInterface.ts
interface EntityInterface {
  id?: number;
  [key: string]: unknown;
}

// library/src/entities/DefaultEntityBehavior.ts
// Default CRUD behaviors for entities
```

### Property Specifications

```typescript
// library/src/services/entity/EntityService.ts
type PropertySpec = {
  label: string;
  type: 'string' | 'number' | 'boolean' | 'select' | 'file' | ...;
  required?: boolean;
  // ...
};
```

### Store Actions

```typescript
// library/src/store/
// easy-peasy store models for:
// - API state management
// - Form state
// - List state
// - Route state
```

---

## Relationship with IvozProvider

```
┌──────────────────────────────────────────────────────────────┐
│                     IvozProvider Portals                      │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐          │
│  │   Client    │  │    Brand    │  │  Platform   │          │
│  │   Portal    │  │   Portal    │  │   Portal    │          │
│  └──────┬──────┘  └──────┬──────┘  └──────┬──────┘          │
│         │                │                │                   │
│         └────────────────┼────────────────┘                   │
│                          │                                    │
│                          ▼                                    │
│               ┌──────────────────┐                            │
│               │    @irontec/     │  ◄── THIS REPO             │
│               │     ivoz-ui      │                            │
│               └──────────────────┘                            │
│                          │                                    │
│                          ▼                                    │
│               ┌──────────────────┐                            │
│               │   MUI / React    │                            │
│               │   easy-peasy     │                            │
│               └──────────────────┘                            │
└──────────────────────────────────────────────────────────────┘
```

---

## Potential Modifications for Our Features

### DID Marketplace (Future)

If needed, we could add:
- `DdiAvailabilityIndicator` - Shows DID availability status
- `PriceDisplayField` - Currency formatting for DID prices
- `BalanceWarning` - Alert when balance is low

### Custom Form Fields (Future)

- `PhoneNumberInput` - With country selector
- `CurrencyInput` - For credit/balance inputs
- `InventorySelector` - For DID selection with filtering

---

## References

### Local
- **IvozProvider source:** `../ivozprovider/` (sibling directory)
- **IvozProvider CLAUDE.md:** `../CLAUDE.md` (parent context)
- **Feature spec:** `integration/research/ivozprovider-feature-development.md`

### External
- **ivoz-ui GitHub:** https://github.com/irontec/ivoz-ui
- **MUI Documentation:** https://mui.com/
- **easy-peasy docs:** https://easy-peasy.dev/
- **IvozProvider docs:** https://irontec.github.io/ivozprovider/en/
