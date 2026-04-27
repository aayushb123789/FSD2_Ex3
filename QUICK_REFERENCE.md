# Responsive UI Quick Reference Card

## Material UI Breakpoints Cheat Sheet

### Screen Size Reference
```
┌─────────────────────────────────────────────────────────┐
│ Breakpoint │ Min Width │ Max Width │     Device Type    │
├─────────────────────────────────────────────────────────┤
│     xs     │     0     │    599    │  Mobile Phone       │
│     sm     │    600    │    959    │  Tablets            │
│     md     │    960    │   1279    │  Laptops            │
│     lg     │   1280    │   1919    │  Large Screens      │
│     xl     │   1920    │    ∞      │  Desktops (4K)      │
└─────────────────────────────────────────────────────────┘
```

---

## Common Responsive Patterns

### 1️⃣ **Responsive Typography**
```jsx
<Typography sx={{ fontSize: { xs: '1rem', md: '2rem' } }}>
  Mobile: 16px | Desktop: 32px
</Typography>
```

### 2️⃣ **Responsive Spacing**
```jsx
<Box sx={{ padding: { xs: '8px', md: '24px' } }}>
  Mobile: 8px | Desktop: 24px
</Box>
```

### 3️⃣ **Stack Vertically on Mobile**
```jsx
<Box sx={{
  display: 'flex',
  flexDirection: { xs: 'column', md: 'row' }
}}>
  Stacks on mobile, side-by-side on desktop
</Box>
```

### 4️⃣ **Hide/Show Elements**
```jsx
<Box sx={{ display: { xs: 'none', md: 'block' } }}>
  Visible only on md and larger
</Box>
```

### 5️⃣ **Responsive Grid Columns**
```jsx
<Grid container spacing={3}>
  <Grid item xs={12} sm={6} md={4} lg={3}>
    Full | Half | Third | Quarter
  </Grid>
</Grid>
```

---

## Grid System Quick Guide

### 12-Column Grid Breakdown

**2-Column Layout:**
```
xs: [12][12]    sm: [6][6]     md: [6][6]
┌──────┐        ┌───┬───┐      ┌───┬───┐
│      │        │   │   │      │   │   │
│      │        │   │   │      │   │   │
└──────┘        └───┴───┘      └───┴───┘
```

**3-Column Layout:**
```
xs: [12]        sm: [6][6]     md: [4][4][4]
┌──────┐        ┌───┬───┐      ┌──┬──┬──┐
│      │        │   │   │      │  │  │  │
└──────┘        └───┴───┘      └──┴──┴──┘
```

**4-Column Layout:**
```
xs: [12]        sm: [6][6]     md: [3][3][3][3]
┌──────┐        ┌───┬───┐      ┌─┬─┬─┬─┐
│      │        │   │   │      │ │ │ │ │
└──────┘        └───┴───┘      └─┴─┴─┴─┘
```

---

## Component Grid Values Reference

| Layout | xs | sm | md | lg | xl | Description |
|--------|----|----|----|----|----|----|
| Full Width | 12 | 12 | 12 | 12 | 12 | Always full width |
| Half Width | 12 | 6 | 6 | 6 | 6 | Stack on mobile, split on tablet+ |
| Thirds | 12 | 6 | 4 | 4 | 4 | Full → half → third |
| Quarters | 12 | 6 | 3 | 3 | 3 | Full → half → quarter |
| Sixths | 12 | 6 | 2 | 2 | 2 | Full → half → sixth |

---

## Responsive Container

```jsx
<Container maxWidth="lg" sx={{ p: { xs: 1, sm: 2, md: 3 } }}>
  {/* Content auto-centers and has responsive padding */}
</Container>
```

| Max Width | Value |
|-----------|-------|
| xs | 100% |
| sm | 540px |
| md | 720px |
| lg | 960px |
| xl | 1140px |

---

## Spacing Scale

Material UI uses `theme.spacing(n)` where `n` multiplies 8px:

| Value | Pixels | CSS |
|-------|--------|-----|
| spacing(0) | 0px | none |
| spacing(1) | 8px | 8px |
| spacing(2) | 16px | 1rem |
| spacing(3) | 24px | 1.5rem |
| spacing(4) | 32px | 2rem |
| spacing(5) | 40px | 2.5rem |
| spacing(6) | 48px | 3rem |
| spacing(8) | 64px | 4rem |

---

## Display Property Cheat Sheet

### Show/Hide by Breakpoint
```jsx
{/* Hide on mobile, show on tablet+ */}
<Box sx={{ display: { xs: 'none', sm: 'block' } }} />

{/* Hide on mobile/tablet, show on laptop+ */}
<Box sx={{ display: { xs: 'none', md: 'block' } }} />

{/* Show only on mobile */}
<Box sx={{ display: { xs: 'block', md: 'none' } }} />

{/* Show only on tablet and up */}
<Box sx={{ display: { xs: 'none', sm: 'flex' } }} />
```

---

## Flex Direction Responsive

```jsx
{/* Stack on mobile, row on desktop */}
<Box sx={{
  display: 'flex',
  flexDirection: { xs: 'column', md: 'row' },
  gap: { xs: 1, md: 3 }
}} />

{/* Vertical on mobile, horizontal on desktop */}
<Stack direction={{ xs: 'column', md: 'row' }} spacing={2}>
  Items
</Stack>
```

---

## Common Responsive Values Object

```jsx
const responsiveStyles = {
  padding: { xs: 1, sm: 2, md: 3, lg: 4 },
  fontSize: { xs: '1rem', sm: '1.2rem', md: '1.5rem' },
  gap: { xs: 1, sm: 2, md: 3, lg: 4 },
  margin: { xs: 1, sm: 2, md: 3 },
  borderRadius: { xs: '4px', sm: '8px', md: '12px' }
};

<Box sx={responsiveStyles}>
  Content
</Box>
```

---

## Theme Object Structure

```jsx
const theme = createTheme({
  palette: {
    primary: { main: '#667eea' },
    secondary: { main: '#764ba2' },
    background: { default: '#f5f5f5', paper: '#fff' },
    text: { primary: '#000', secondary: '#666' }
  },
  typography: {
    fontFamily: '"Roboto", sans-serif',
    h1: { fontSize: '2.5rem', fontWeight: 700 },
    body1: { fontSize: '1rem', lineHeight: 1.6 }
  },
  components: {
    MuiButton: {
      styleOverrides: {
        root: { borderRadius: 8 }
      }
    }
  }
});
```

---

## Styled Component Syntax

```jsx
import { styled } from '@mui/material/styles';
import { Card } from '@mui/material';

const StyledCard = styled(Card)(({ theme }) => ({
  // Base styles
  borderRadius: 12,
  padding: theme.spacing(2),
  
  // Responsive styles
  [theme.breakpoints.down('sm')]: {
    padding: theme.spacing(1),
  },
  
  // Pseudo-classes
  '&:hover': {
    boxShadow: theme.shadows[12],
  }
}));
```

---

## useMediaQuery Hook

```jsx
import { useMediaQuery, useTheme } from '@mui/material';

function MyComponent() {
  const theme = useTheme();
  const isMobile = useMediaQuery(theme.breakpoints.down('sm'));
  const isTablet = useMediaQuery(theme.breakpoints.up('sm'));
  const isDesktop = useMediaQuery(theme.breakpoints.up('md'));
  
  return isMobile ? <MobileView /> : <DesktopView />;
}
```

---

## SX Prop vs Styled Components

### Using SX (Inline Styles)
```jsx
<Box sx={{
  p: { xs: 1, md: 3 },
  fontSize: { xs: '1rem', md: '1.5rem' },
  backgroundColor: 'primary.main',
  '&:hover': { boxShadow: 2 }
}}>
  Content
</Box>
```

### Using Styled Components (Reusable)
```jsx
const StyledBox = styled(Box)(({ theme }) => ({
  padding: theme.spacing(1),
  [theme.breakpoints.up('md')]: {
    padding: theme.spacing(3),
  }
}));

<StyledBox>Content</StyledBox>
```

---

## Common Breakpoint Patterns

### Pattern 1: Mobile-First (xs → md)
```jsx
<Grid item xs={12} md={6} lg={4}>
  Mobile: Full | Tablet: Half | Desktop: Third
</Grid>
```

### Pattern 2: Responsive Columns
```jsx
<Grid container spacing={{ xs: 2, sm: 3, md: 4, lg: 5 }}>
  Spacing increases with screen size
</Grid>
```

### Pattern 3: Hide on Mobile
```jsx
<Box sx={{ display: { xs: 'none', md: 'block' } }}>
  Not visible on mobile/tablet
</Box>
```

### Pattern 4: Stack Layout
```jsx
<Box sx={{
  display: 'grid',
  gridTemplateColumns: { xs: '1fr', md: '1fr 1fr', lg: '1fr 1fr 1fr' }
}}>
  Responsive CSS Grid
</Box>
```

---

## Color System Quick Reference

```jsx
// Primary Gradient
background: 'linear-gradient(135deg, #667eea 0%, #764ba2 100%)'

// Status Colors
success: '#51cf66'  // Green
error: '#ff6b6b'    // Red
warning: '#ffd43b'  // Yellow
info: '#4facfe'     // Blue

// Text Colors
primary: '#000'     // Dark text
secondary: '#666'   // Gray text
disabled: '#999'    // Disabled text
```

---

## Shadow Reference

```jsx
theme.shadows[0]  // No shadow
theme.shadows[2]  // Subtle
theme.shadows[4]  // Light
theme.shadows[8]  // Medium
theme.shadows[12] // Elevated
theme.shadows[16] // Strong
theme.shadows[20] // Extra strong
```

---

## Quick Copy-Paste Templates

### Responsive Hero Section
```jsx
<Box sx={{
  background: 'linear-gradient(135deg, #667eea 0%, #764ba2 100%)',
  color: 'white',
  py: { xs: 4, md: 8 },
  textAlign: 'center'
}}>
  <Typography variant="h2" sx={{ fontSize: { xs: '2rem', md: '3rem' } }}>
    Title
  </Typography>
</Box>
```

### Responsive Card Grid
```jsx
<Grid container spacing={{ xs: 2, md: 3 }}>
  {items.map((item) => (
    <Grid item xs={12} sm={6} md={4} key={item.id}>
      <Card>Content</Card>
    </Grid>
  ))}
</Grid>
```

### Responsive Flex Container
```jsx
<Box sx={{
  display: 'flex',
  flexDirection: { xs: 'column', md: 'row' },
  gap: { xs: 2, md: 4 },
  alignItems: { xs: 'stretch', md: 'center' }
}}>
  Item 1
  Item 2
</Box>
```

---

## Debugging Responsive Layouts

```jsx
// Temporary breakpoint indicator
<Box sx={{
  position: 'fixed',
  bottom: 10,
  right: 10,
  zIndex: 9999,
  backgroundColor: 'black',
  color: 'white',
  padding: 1
}}>
  <Typography>
    {isMobile ? 'xs' : isTablet ? 'sm' : 'md+'}
  </Typography>
</Box>
```

---

## Performance Checklist

✅ Use responsive `sx` prop instead of multiple media query CSS files
✅ Leverage `useMediaQuery` hook for conditional logic
✅ Memoize components to prevent unnecessary re-renders
✅ Lazy load images for mobile devices
✅ Use CSS Grid for complex layouts
✅ Optimize bundle size by tree-shaking unused components
✅ Test on actual devices, not just browser DevTools

---

**Print this card and keep it handy!** 📋
