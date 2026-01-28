# Semantic Tokens Reference

Complete list of all valid semantic color tokens in the IBKR design system.

## CSS Class Prefixes

All tokens can be used with these prefixes:
- `bg-{token}` - Background color
- `text-{token}` - Text color
- `decoration-{token}` - Text decoration color
- `outline-{token}` - Outline color
- `accent-{token}` - Accent color
- `fill-{token}` - SVG fill
- `stroke-{token}` - SVG stroke
- `shadow-{token}` - Box shadow color
- `ring-{token}` - Ring color

Border tokens are used directly without prefix: `border-accent`, `border-buy`, etc.

## State Modifiers

Pseudo-class variants available:
- `hover:{prefix}-{token}` - Hover state
- `focus:{prefix}-{token}` - Focus state
- `active:{prefix}-{token}` - Active state
- `disabled:{prefix}-{token}` - Disabled state

## Surface Tokens

| Token | Light Mode | Dark Mode | Use For |
|-------|------------|-----------|---------|
| `surface` | #FFFFFF | #16191F | Main background |
| `surface-hover` | #FFFFFF | #16191F | Hovered surface |
| `surface-pressed` | #FFFFFF | #16191F | Pressed surface |
| `surface-low` | #F3F3F4 | #000000 | Sunken areas |
| `surface-low-hover` | #F3F3F4 | #000000 | Sunken hover |
| `surface-low-pressed` | #F3F3F4 | #000000 | Sunken pressed |
| `surface-high` | #F3F3F4 | #000000 | Elevated areas |
| `surface-high-hover` | #F3F3F4 | #000000 | Elevated hover |
| `surface-high-pressed` | #F3F3F4 | #000000 | Elevated pressed |
| `surface-container` | #E8E8E9 | #2D3036 | Card backgrounds |
| `surface-container-low` | #F3F3F4 | #22252A | Subtle containers |
| `surface-container-lowest` | #F5F7FA | #1D2025 | Most subtle containers |
| `surface-container-high` | #D0D1D2 | #383B41 | Emphasized containers |
| `surface-container-highest` | #D0D1D2 | #383B41 | Most emphasized |
| `surface-inverse` | #2D3036 | #E8E8E9 | Inverted background |
| `surface-inverse-hover` | #2D3036 | #E8E8E9 | Inverted hover |
| `surface-inverse-pressed` | #2D3036 | #E8E8E9 | Inverted pressed |

## On-Surface Tokens (Text)

| Token | Light Mode | Dark Mode | Use For |
|-------|------------|-----------|---------|
| `on-surface` | #16191F | #FFFFFF | Primary text |
| `on-surface-secondary` | #44474D | #D0D1D2 | Secondary text |
| `on-surface-tertiary` | #727579 | #A2A3A5 | Muted text |
| `on-surface-disabled` | #A2A3A5 | #727579 | Disabled text |
| `on-surface-inverse` | #FFFFFF | #16191F | Text on inverse |
| `on-surface-inverse-secondary` | #FFFFFF | #16191F | Secondary inverse |

## Accent Tokens

| Token | Light Mode | Dark Mode | Use For |
|-------|------------|-----------|---------|
| `accent` | #0055D4 | #5998F5 | Primary actions |
| `accent-hover` | #0449B0 | #83B2F7 | Hovered accent |
| `accent-pressed` | #093D8C | #ACCBFA | Pressed accent |
| `accent-subtle` | #D6E5FC | #122543 | Subtle background |
| `accent-subtle-hover` | #ACCBFA | #0F2B55 | Subtle hover |
| `accent-subtle-pressed` | #83B2F7 | #0D3167 | Subtle pressed |
| `accent-disabled` | #83B2F7 | #0D3167 | Disabled accent |
| `accent-inverse` | #83B2F7 | #0D3167 | Inverted accent |
| `on-accent` | #FFFFFF | #16191F | Text on accent |
| `on-accent-subtle` | #0449B0 | #5998F5 | Text on subtle |
| `on-accent-disabled` | #0449B0 | #5998F5 | Disabled text |

### Fixed Accent Tokens (Don't change with theme)
- `accent-fixed`, `accent-fixed-hover`, `accent-fixed-pressed`
- `accent-subtle-fixed`, `accent-subtle-fixed-hover`, `accent-subtle-fixed-pressed`
- `on-accent-fixed`, `on-accent-subtle-fixed`

## Buy Tokens (Long/Purchase Actions)

| Token | Light Mode | Dark Mode |
|-------|------------|-----------|
| `buy` | #0055D4 | #5998F5 |
| `buy-hover` | #0449B0 | #83B2F7 |
| `buy-pressed` | #093D8C | #ACCBFA |
| `buy-subtle` | #D6E5FC | #122543 |
| `buy-subtle-hover` | #ACCBFA | #0F2B55 |
| `buy-subtle-pressed` | #83B2F7 | #0D3167 |
| `buy-disabled` | #83B2F7 | #0D3167 |
| `on-buy` | #FFFFFF | #16191F |
| `on-buy-subtle` | #0449B0 | #5998F5 |

## Sell Tokens (Short/Sell Actions)

| Token | Light Mode | Dark Mode |
|-------|------------|-----------|
| `sell` | #D80E1F | #FD5B69 |
| `sell-hover` | #B1101F | #FD848E |
| `sell-pressed` | #8A121F | #FEADB4 |
| `sell-subtle` | #FED6D9 | #3D171F |
| `sell-subtle-hover` | #FEADB4 | #50161F |
| `sell-subtle-pressed` | #FD848E | #64151F |
| `sell-disabled` | #FD848E | #64151F |
| `on-sell` | #FFFFFF | #16191F |
| `on-sell-subtle` | #B1101F | #FD5B69 |

## Positive Tokens (Gains/Up)

| Token | Light Mode | Dark Mode |
|-------|------------|-----------|
| `positive` | #018740 | #3EC27C |
| `positive-hover` | #057139 | #6ED19D |
| `positive-pressed` | #095B33 | #9FE1BD |
| `positive-subtle` | #CFF0DE | #122F26 |
| `positive-subtle-hover` | #9FE1BD | #103A29 |
| `positive-subtle-pressed` | #6ED19D | #0E452C |
| `positive-disabled` | #6ED19D | #0E452C |
| `on-positive` | #FFFFFF | #16191F |
| `on-positive-subtle` | #057139 | #3EC27C |

## Negative Tokens (Losses/Down)

| Token | Light Mode | Dark Mode |
|-------|------------|-----------|
| `negative` | #D80E1F | #FD5B69 |
| `negative-hover` | #B1101F | #FD848E |
| `negative-pressed` | #8A121F | #FEADB4 |
| `negative-subtle` | #FED6D9 | #3D171F |
| `negative-subtle-hover` | #FEADB4 | #50161F |
| `negative-subtle-pressed` | #FD848E | #64151F |
| `negative-disabled` | #FD848E | #64151F |
| `on-negative` | #FFFFFF | #16191F |
| `on-negative-subtle` | #B1101F | #FD5B69 |

## Yes/No Tokens (Confirmation Actions)

### Yes Tokens
| Token | Light Mode | Dark Mode |
|-------|------------|-----------|
| `yes` | #018740 | #3EC27C |
| `yes-hover` | #057139 | #6ED19D |
| `yes-pressed` | #095B33 | #9FE1BD |
| `yes-subtle` | #CFF0DE | #122F26 |
| `on-yes` | #FFFFFF | #16191F |

### No Tokens
| Token | Light Mode | Dark Mode |
|-------|------------|-----------|
| `no` | #D80E1F | #FD5B69 |
| `no-hover` | #B1101F | #FD848E |
| `no-pressed` | #8A121F | #FEADB4 |
| `no-subtle` | #FED6D9 | #3D171F |
| `on-no` | #FFFFFF | #16191F |

## Success Tokens

| Token | Light Mode | Dark Mode |
|-------|------------|-----------|
| `success` | #018740 | #3EC27C |
| `success-hover` | #057139 | #6ED19D |
| `success-pressed` | #095B33 | #9FE1BD |
| `success-subtle` | #CFF0DE | #122F26 |
| `on-success` | #6ED19D | #0E452C |
| `on-success-subtle` | #6ED19D | #0E452C |

## Error Tokens

| Token | Light Mode | Dark Mode |
|-------|------------|-----------|
| `error` | #D80E1F | #FD5B69 |
| `error-hover` | #B1101F | #FD848E |
| `error-pressed` | #8A121F | #FEADB4 |
| `error-subtle` | #FED6D9 | #3D171F |
| `on-error` | #FFFFFF | #16191F |
| `on-error-subtle` | #B1101F | #FD5B69 |

## Alert Tokens

| Token | Light Mode | Dark Mode |
|-------|------------|-----------|
| `alert` | #D80E1F | #FD5B69 |
| `alert-hover` | #B1101F | #FD848E |
| `alert-pressed` | #8A121F | #FEADB4 |
| `alert-subtle` | #FED6D9 | #3D171F |
| `on-alert` | #FFFFFF | #64151F |
| `on-alert-subtle` | #FD848E | #64151F |

## Warning Tokens

| Token | Light Mode | Dark Mode |
|-------|------------|-----------|
| `warning` | #FFB300 | #FFB300 |
| `warning-hover` | #E8A403 | #FFC233 |
| `warning-pressed` | #D09406 | #FFD166 |
| `warning-subtle` | #FFF0CC | #453819 |
| `on-warning` | #5C4716 | #5C4716 |
| `on-warning-subtle` | #8A6610 | #FFC233 |

## Important Tokens

| Token | Light Mode | Dark Mode |
|-------|------------|-----------|
| `important` | #FFB300 | #FFB300 |
| `important-hover` | #E8A403 | #FFC233 |
| `important-pressed` | #D09406 | #FFD166 |
| `important-subtle` | #FFF0CC | #453819 |
| `on-important` | #5C4716 | #8A6610 |
| `on-important-subtle` | #8A6610 | #8A6610 |

## AI Tokens (AI Features)

| Token | Light Mode | Dark Mode |
|-------|------------|-----------|
| `ai` | #771CD9 | #B373F8 |
| `ai-hover` | #641BB4 | #C696FA |
| `ai-pressed` | #501B8F | #D9B9FB |
| `ai-subtle` | #F5EDFE | #291A44 |
| `on-ai` | #FFFFFF | #16191F |
| `on-ai-subtle` | #771CD9 | #B373F8 |

## Border Tokens

Use directly as CSS classes (not with `border-` prefix):

| Class | Light Mode | Dark Mode | Use For |
|-------|------------|-----------|---------|
| `border` | #D0D1D2 | #383B41 | Default borders |
| `border-hover` | #D0D1D2 | #383B41 | Hovered borders |
| `border-subtle` | #D0D1D2 | #383B41 | Subtle borders |
| `border-strong` | #727579 | #A2A3A5 | Emphasized borders |
| `border-strongest` | #16191F | #D0D1D2 | Maximum contrast |
| `border-inverse` | #16191F | #D0D1D2 | Inverted borders |
| `border-disabled` | - | - | Disabled state |

### Semantic Border Colors
| Class | Use For |
|-------|---------|
| `border-accent` | Accent/primary borders |
| `border-accent-subtle` | Subtle accent borders |
| `border-accent-strong` | Strong accent borders |
| `border-buy` | Buy action borders |
| `border-buy-subtle` | Subtle buy borders |
| `border-sell` | Sell action borders |
| `border-sell-subtle` | Subtle sell borders |
| `border-positive` | Positive/gain borders |
| `border-negative` | Negative/loss borders |
| `border-success` | Success state borders |
| `border-error` | Error state borders |
| `border-warning` | Warning state borders |
| `border-important` | Important borders |
| `border-ai` | AI feature borders |

## Usage Examples

### Basic Card
```html
<div class="bg-surface-container border border-subtle rounded-lg p-4">
  <h3 class="text-on-surface font-semibold">Card Title</h3>
  <p class="text-on-surface-secondary">Description text</p>
</div>
```

### Trading Display
```html
<div class="flex items-center gap-2">
  <span class="text-on-surface-tertiary">Change:</span>
  <span class="text-positive">+2.45%</span>
</div>
```

### Action Buttons
```html
<div class="flex gap-2">
  <button class="bg-buy hover:bg-buy-hover text-on-buy px-4 py-2 rounded">
    Buy
  </button>
  <button class="bg-sell hover:bg-sell-hover text-on-sell px-4 py-2 rounded">
    Sell
  </button>
</div>
```

### Status Badge
```html
<span class="bg-success-subtle text-on-success-subtle px-2 py-1 rounded text-sm">
  Filled
</span>
```
